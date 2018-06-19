---
uid: web-forms/overview/data-access/filtering-scenarios-with-the-datalist-and-repeater/master-detail-filtering-with-a-dropdownlist-datalist-cs
title: Master/Detail-Filtern mit einer DropDownList (c#) | Microsoft Docs
author: rick-anderson
description: In diesem Lernprogramm sehen wir die Vorgehensweise beim Anzeigen von Master/Detail-Berichte in einer einzelnen Webseite mit DropDownLists 'master' Datensätze und DataList, bol angezeigt...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/18/2007
ms.topic: article
ms.assetid: 07fa47ae-e491-4a2f-b265-d342b9ddef46
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/filtering-scenarios-with-the-datalist-and-repeater/master-detail-filtering-with-a-dropdownlist-datalist-cs
msc.type: authoredcontent
ms.openlocfilehash: c84902ccf028c976246380abfaebb6a76c573603
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
ms.locfileid: "30880673"
---
<a name="masterdetail-filtering-with-a-dropdownlist-c"></a>Master/Detail-Filtern mit einer DropDownList (c#)
====================
durch [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Beispiel-App herunterladen](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_33_CS.exe) oder [PDF herunterladen](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/datatutorial33cs1.pdf)

> In diesem Lernprogramm sehen wir die Vorgehensweise beim Anzeigen von Master/Detail-Berichte in einer einzelnen Webseite mit DropDownLists zum Anzeigen der "master" Datensätze "und" DataList "Details" anzuzeigen.


## <a name="introduction"></a>Einführung

Master/Detail-Berichts, die wir bei der Erstellung mit GridView in den früheren [Master/Detail-Filtern mit einer DropDownList](../masterdetail/master-detail-filtering-with-a-dropdownlist-cs.md) Lernprogramm beginnt, durch einen Satz mit "master" Datensätze anzeigen. Der Benutzer kann dann einen der master Datensätze Drilldown in und Anzeigen von diesen master Datensatz "Details." Master/Detail-Berichte sind eine ideale Option für die visuelle Darstellung der 1: n-Beziehungen und zum Anzeigen von detaillierten Informationen aus besonders "Breite" Tabellen (Argumente, die viele Spalten haben). Wir haben zum Implementieren von Master/Detail-Berichte, die mit den Steuerelementen GridView und DetailsView in vorherigen Lernprogrammen untersucht. In diesem Lernprogramm und die nächsten beiden wir werden diese Konzepte, aber den Fokus zur Verwendung von DataList nochmals und Repeater stattdessen steuert.

In diesem Lernprogramm lernen wir mit einer DropDownList der "master" Datensätze, die "details" Datensätze in einem DataList enthalten.

## <a name="step-1-adding-the-masterdetail-tutorial-web-pages"></a>Schritt 1: Hinzufügen von Webseiten der Master-/Detail-Lernprogramm

Bevor wir in diesem Lernprogramm beginnen, zuerst werfen wir einen Moment Zeit, fügen Sie die Ordner und ASP.NET-Seiten, die wir für dieses Lernprogramm und die nächsten beiden Umgang mit Master-/Detail-Berichte, die mit den verschiedenen Steuerelementen benötigen. Starten, indem Sie einen neuen Ordner erstellen, in das Projekt mit dem Namen `DataListRepeaterFiltering`. Fügen Sie die folgenden fünf ASP.NET-Seiten in diesen Ordner, dass alle von ihnen für die Verwendung die Masterseite konfiguriert `Site.master`:

- `Default.aspx`
- `FilterByDropDownList.aspx`
- `CategoryListMaster.aspx`
- `ProductsForCategoryDetails.aspx`
- `CategoriesAndProducts.aspx`


![Erstellen Sie einen Ordner DataListRepeaterFiltering, und fügen Sie dem Tutorial ASP.NET-Seiten hinzu](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image1.png)

**Abbildung 1**: Erstellen einer `DataListRepeaterFiltering` Ordner, und fügen Sie dem Tutorial ASP.NET-Seiten hinzu


Öffnen Sie als Nächstes die `Default.aspx` Seite, und ziehen Sie die `SectionLevelTutorialListing.ascx` Benutzersteuerelement aus den `UserControls` Ordner auf die Entwurfsoberfläche. Dieses Benutzersteuerelement, die wir in erstellt die [Masterseiten und Websitenavigation](../introduction/master-pages-and-site-navigation-cs.md) Lernprogramm, zählt die Siteübersicht und zeigt die Lernprogramme aus dem aktuellen Abschnitt in einer Liste mit Aufzählungszeichen aus.


[![Das Benutzersteuerelement SectionLevelTutorialListing.ascx "default.aspx" hinzufügen](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image3.png)](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image2.png)

**Abbildung 2**: Hinzufügen der `SectionLevelTutorialListing.ascx` Benutzersteuerelement `Default.aspx` ([klicken Sie hier, um das Bild in voller Größe angezeigt](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image4.png))


Um die Anzeige der Liste mit Aufzählungszeichen haben müssen wir Master/Detail-Lernprogramme, die wir erstellen, die um sie in der Siteübersicht hinzuzufügen. Öffnen der `Web.sitemap` Datei, und fügen Sie das folgende Markup nach "Anzeigen von Daten mit der DataList und Repeater" Site Zuordnung Knoten Markup hinzu:

[!code-xml[Main](master-detail-filtering-with-a-dropdownlist-datalist-cs/samples/sample1.xml)]


![Aktualisieren Sie die Website-Karte, um die neue ASP.NET-Seiten enthalten](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image5.png)

**Abbildung 3**: Aktualisieren Sie die Website-Karte, um die neue ASP.NET-Seiten enthalten


## <a name="step-2-displaying-the-categories-in-a-dropdownlist"></a>Schritt 2: Anzeigen der Kategorien in einer DropDownList

Unsere Master/Detail-Bericht listet die Kategorien in einer DropDownList mit dem ausgewählten Listenelement Produkte angezeigt weiter unten auf der Seite in einem DataList. Anschließend werden die erste Aufgabe vor uns, haben die Kategorien, die in einer Dropdownliste angezeigt. Öffnen Sie zunächst die `FilterByDropDownList.aspx` auf der Seite der `DataListRepeaterFiltering` Ordner, und ziehen Sie eine DropDownList aus der Toolbox auf die Seite-Designer. Legen Sie anschließend die DropDownList `ID` Eigenschaft `Categories`. Klicken Sie auf den Link Datenquelle wählen Sie aus der DropDownList-Smarttag, und erstellen Sie eine neue, mit dem Namen ObjectDataSource `CategoriesDataSource`.


[![Fügen Sie eine neue, mit dem Namen CategoriesDataSource ObjectDataSource hinzu](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image7.png)](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image6.png)

**Abbildung 4**: Hinzufügen einer neuen ObjectDataSource namens `CategoriesDataSource` ([klicken Sie hier, um das Bild in voller Größe angezeigt](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image8.png))


Die neue ObjectDataSource so konfigurieren, dass er ruft die `CategoriesBLL` Klasse `GetCategories()` Methode. Nach dem Konfigurieren der ObjectDataSource müssen wir noch angeben, welches Feld der Datenquelle in der Dropdownliste angezeigt werden soll und welche sollte einen als der Wert für jedes Listenelement zugeordnet ist. Haben die `CategoryName` -Felds ist, als die Anzeige und `CategoryID` als Wert für jedes Listenelement.


[![Weisen Sie die CategoryName-Feld und CategoryID verwenden der DropDownList-Anzeige auf, als Wert](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image10.png)](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image9.png)

**Abbildung 5**: haben Sie die Anzeige der DropDownList der `CategoryName` Feld und Verwendung `CategoryID` als Wert ([klicken Sie hier, um das Bild in voller Größe angezeigt](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image11.png))


An diesem Punkt haben wir ein DropDownList-Steuerelement, das mit den Datensätzen aufgefüllt wird die `Categories` Tabelle (alle in ungefähr sechs Sekunden erreicht). Abbildung 6 zeigt unseren Fortschritt bisher ein, wenn Sie über einen Browser angezeigt.


[![Eine Dropdownliste werden die aktuellen Kategorien aufgelistet.](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image13.png)](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image12.png)

**Abbildung 6**: ein Dropdown-Listet die aktuellen Kategorien ([klicken Sie hier, um das Bild in voller Größe angezeigt](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image14.png))


## <a name="step-2-adding-the-products-datalist"></a>Schritt 2: Hinzufügen von DataList Produkte

Der letzte Schritt in unserem Master/Detail-Bericht wird die Produkte, die der ausgewählten Kategorie zugeordnet aufgeführt. Klicken Sie hierzu die Seite DataList hinzu, und erstellen Sie eine neue ObjectDataSource mit dem Namen `ProductsByCategoryDataSource`. Haben die `ProductsByCategoryDataSource` Steuerelement abzurufen, dessen Daten aus der `ProductsBLL` Klasse `GetProductsByCategoryID(categoryID)` Methode. Da dieser Bericht Master/Detail-schreibgeschützt ist, wählen Sie, dass die (None) auf den Registerkarten INSERT-, Update- und DELETE-option.


[![Wählen Sie die GetProductsByCategoryID(categoryID)-Methode](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image16.png)](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image15.png)

**Abbildung 7**: Wählen Sie die `GetProductsByCategoryID(categoryID)` Methode ([klicken Sie hier, um das Bild in voller Größe angezeigt](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image17.png))


Nach dem Klicken auf "Weiter" ObjectDataSource-Assistenten werden Sie uns für die Quelle des Werts für die `GetProductsByCategoryID(categoryID)` Methode *`categoryID`* Parameter. Verwenden Sie den Wert des ausgewählten `categories` DropDownList Element legen Sie als Quelle für die Parameter zum Steuern und die ControlID auf `Categories`.


[![Legen Sie die CategoryID-Parameter auf den Wert der DropDownList Kategorien](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image19.png)](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image18.png)

**Abbildung 8**: Festlegen der *`categoryID`* auf den Wert von der `Categories` DropDownList ([klicken Sie hier, um das Bild in voller Größe angezeigt](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image20.png))


Nach Abschluss der Assistent zum Konfigurieren von Datenquellen, generiert Visual Studio automatisch eine `ItemTemplate` für DataList, die den Namen und Wert der einzelnen Datenfelder anzeigt. Wir verbessern die DataList, verwenden Sie stattdessen eine `ItemTemplate` , die zeigt nur den Namen des Produkts, Kategorie, Lieferanten, Menge pro Einheit und Preis zusammen mit einer `SeparatorTemplate` injiziert, die eine `<hr>` Element zwischen den einzelnen Elementen. Ich werde verwenden die `ItemTemplate` aus einem Beispiel, in der [Anzeigen von Daten mit dem DataList und Repeater](../displaying-data-with-the-datalist-and-repeater/displaying-data-with-the-datalist-and-repeater-controls-cs.md) Lernprogramm jedoch gerne verwenden Sie die Vorlagenmarkup visuell ansprechender finden Sie.

Nachdem diese Änderungen vorgenommen wurden, sollte DataList und seine ObjectDataSource Markup etwa wie folgt aussehen:

[!code-aspx[Main](master-detail-filtering-with-a-dropdownlist-datalist-cs/samples/sample2.aspx)]

Nehmen Sie einen Moment Zeit, um unseren Fortschritt in einem Browser zu überprüfen. Bei der ersten Seite besuchen, werden die Produkte, die zur ausgewählten Kategorie (Getränke) angezeigt, (wie in Abbildung 9 gezeigt), aber der DropDownList ändern nicht aktualisiert, die Daten. Dies ist, da ein Postback, damit das DataList auftreten muss zu aktualisieren. Legen Sie hierfür entweder können der DropDownList `AutoPostBack` Eigenschaft `true` oder fügen Sie ein Websteuerelement Schaltfläche auf der Seite. Für dieses Lernprogramm ich haben sich entschieden, zum Festlegen der DropDownList `AutoPostBack` Eigenschaft `true`.

Abbildung 9 und 10 veranschaulichen den Master-/Detail-Bericht in Aktion.


[![Bei der ersten Seite besuchen, werden die Produkte Getränke angezeigt.](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image22.png)](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image21.png)

**Abbildung 9**: bei der ersten Seite besuchen, werden die Produkte Getränke angezeigt ([klicken Sie hier, um das Bild in voller Größe angezeigt](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image23.png))


[![Durch Auswahl eines neuen Produkts (erzeugen) automatisch bewirkt, dass ein Postback handeln, das DataList aktualisieren](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image25.png)](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image24.png)

**Abbildung 10**: durch Auswahl eines neuen Produkts (erzeugen) automatisch bewirkt, dass ein Postback handeln, aktualisieren die DataList ([klicken Sie hier, um das Bild in voller Größe angezeigt](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image26.png))


## <a name="adding-a----choose-a-category----list-item"></a>Hinzufügen eines Listenelements "– Wählen Sie eine Kategorie –"

Beim ersten Zugriff auf die `FilterByDropDownList.aspx` Seite die Kategorien DropDownLists erste Listenelement (Getränke) wird standardmäßig angezeigt, die mit den Produkten Getränke DataList ausgewählt ist. In der *Master/Detail-Filtern mit einer DropDownList* Lernprogramm DropDownList, mit denen wurde standardmäßig ausgewählt, und bei Auswahl dieser Option angezeigt wurde eine Option "– Wählen Sie eine Kategorie –" hinzugefügt *alle* von der Produkte in der Datenbank. Ein derartiger Ansatz wurde verwaltbare beim Auflisten der Produkte in einer GridView wie jede Produktzeile oben eine kleine Menge an Bildschirmfläche gedauert hat. Mit DataList beansprucht jedoch Informationen für jedes Produkt eine viel größere Segment des Bildschirms. Nehmen wir weiterhin eine Option "– Wählen Sie eine Kategorie –" hinzufügen und haben sie standardmäßig ausgewählt, aber anstatt sie alle Produkte anzeigen bei Auswahl dieser Option wir es so konfigurieren, dass es keine Produkte zeigt.

Um ein neues Listenelement DropDownList hinzuzufügen, wechseln Sie zu dem Eigenschaftenfenster, und klicken Sie auf die Auslassungszeichen in der `Items` Eigenschaft. Hinzufügen ein neuen Listenelements mit dem `Text` "– Wählen Sie eine Kategorie –" und die `Value` `0`.


![Hinzufügen einer](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image27.png)

**Abbildung 11**: Hinzufügen eines Listenelements "– Wählen Sie eine Kategorie –"


Alternativ können Sie das Listenelement hinzufügen, indem Sie das folgende Markup DropDownList hinzufügen:

[!code-aspx[Main](master-detail-filtering-with-a-dropdownlist-datalist-cs/samples/sample3.aspx)]

Darüber hinaus müssen wir die DropDownList-Steuerelement festgelegt `AppendDataBoundItems` auf `true` da falls festgelegt `false` (Standard), wenn die Kategorien aus dem ObjectDataSource DropDownList gebunden sind überschreiben diese müssen eine beliebige Liste manuell hinzugefügt Elemente.


![Legen Sie die AppendDataBoundItems-Eigenschaft auf "true"](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image28.png)

**Abbildung 12**: Legen Sie die `AppendDataBoundItems` Eigenschaft auf "true"


Der Grund dafür haben wir den Wert `0` für die Liste "– Wählen Sie eine Kategorie –" Element ist, da es keine Kategorien im System mit einem Wert von gibt `0`, daher keine Produktdatensätze werden zurückgegeben, wenn das Listenelement "– Wählen Sie eine Kategorie –" ausgewählt ist. Nehmen Sie einen Moment Zeit, um die Seite über einen Browser besuchen, um dies zu bestätigen. Wie in Abbildung 13 gezeigt, wenn zunächst Anzeige der Seite das Listenelement "– Wählen Sie eine Kategorie –" ausgewählt ist und keine Produkte angezeigt werden.


[![Wenn die](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image30.png)](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image29.png)

**Abbildung 13**: No Produkte werden angezeigt, wenn das Listenelement "– Wählen Sie eine Kategorie –" ausgewählt ist, ([klicken Sie hier, um das Bild in voller Größe angezeigt](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image31.png))


Wenn Sie stattdessen anzeigen würde *alle* der Produkte, wenn die Option "– Wählen Sie eine Kategorie –" ausgewählt ist, verwenden Sie den Wert `-1` stattdessen. Aufmerksame Leser erinnern, wieder in der *Master/Detail-Filtern mit einer DropDownList* Lernprogramm wir aktualisiert, die `ProductsBLL` -Klasse `GetProductsByCategoryID(categoryID)` Methode so, dass wenn eine *`categoryID`* Wert des `-1` übergeben wurde, alle Produkte wurden Einträge zurückgegeben.

## <a name="summary"></a>Zusammenfassung

Zum Anzeigen von hierarchisch produktbezogene Daten leichter häufig zum Präsentieren von Daten mithilfe von Master/Detail-Berichte, die der Benutzer kann aus denen die Daten aus der obersten Hierarchieebene beispieldatasets starten und Drilldown auf Details. In diesem Lernprogramm untersucht wir erstellen einen einfache Master/Detail-Bericht, der Produkte einer ausgewählten Kategorie anzeigt. Dies wurde mithilfe einer DropDownList für die Liste der Kategorien und DataList für die Produkte, die der ausgewählten Kategorie gehören.

In den nächsten Lernprogrammen betrachten wir die Datensätze Master und Details über zwei Seiten aufteilen. Auf der ersten Seite wird eine Liste der "master" Datensätze mit einem Link zum Anzeigen der Details angezeigt werden. Durch Klicken auf den Link wird den Benutzer auf die zweite Seite weiter, der die Details für den ausgewählten master Datensatz angezeigt wird.

Viel Spaß beim Programmieren!

## <a name="about-the-author"></a>Informationen zum Autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor von sieben ASP/ASP.NET-Büchern und Gründer von [4GuysFromRolla.com](http://www.4guysfromrolla.com), Microsoft Web-Technologien seit 1998 arbeitet. Scott fungiert als ein unabhängiger Berater, Trainer und Writer. Sein neueste Buch wird [ *Sams Schulen selbst ASP.NET 2.0 in 24 Stunden*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Er die erreicht werden kann, zur [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) oder über seinen Blog die finden Sie unter [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Besonderen Dank an...

Diese Reihe von Lernprogrammen wurde durch viele nützliche Bearbeiter überprüft. Lead Prüfer für dieses Lernprogramm wurde Randy Schmidt. Meine bevorstehende MSDN-Artikel Überprüfen von Interesse? Wenn dies der Fall ist, löschen Sie mich zeilenweise [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Nächste](master-detail-filtering-acess-two-pages-datalist-cs.md)
