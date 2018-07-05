---
uid: web-forms/overview/data-access/filtering-scenarios-with-the-datalist-and-repeater/master-detail-filtering-with-a-dropdownlist-datalist-vb
title: Mit einem DropDownList-Steuerelement (VB) Filtern von Master-/Detailberichten | Microsoft-Dokumentation
author: rick-anderson
description: In diesem Tutorial sehen wir die Vorgehensweise beim Anzeigen von Master/Detail-Berichte in einer einzelnen Webseite DropDownList-Steuerelementen verwenden, die "master"-Datensätze und einem DataList-Steuerelement, bol angezeigt...
ms.author: aspnetcontent
ms.date: 07/18/2007
ms.assetid: ad0f1014-1eff-465f-bdc6-93058de00e44
msc.legacyurl: /web-forms/overview/data-access/filtering-scenarios-with-the-datalist-and-repeater/master-detail-filtering-with-a-dropdownlist-datalist-vb
msc.type: authoredcontent
ms.openlocfilehash: e88a7459529e003b73ef5a42456de3501e3db461
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2018
ms.locfileid: "37829320"
---
<a name="masterdetail-filtering-with-a-dropdownlist-vb"></a>Master/Detail-Filtern mit einem DropDownList-Steuerelement (VB)
====================
durch [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Beispiel-App herunter](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_33_VB.exe) oder [PDF-Datei herunterladen](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/datatutorial33vb1.pdf)

> In diesem Tutorial erfahren Sie wie Master/Detail-Berichte in einer einzelnen Webseite mit DropDownList-Steuerelementen zum Anzeigen der "master"-Datensätze und einem DataList-Steuerelement zum Anzeigen der "Details" angezeigt.


## <a name="introduction"></a>Einführung

Master/Detail-Berichts, die wir zuerst erstellt unter Verwendung eines GridView weiter oben [Master/Detail-Filtern mit einer DropDownList](../masterdetail/master-detail-filtering-with-a-dropdownlist-vb.md) Lernprogramm beginnt mit einem Satz von "master"-Datensätze. Der Benutzer kann dann einen der master-Datensätze aufgliedern und Anzeigen des Datensatzes, master "." Master/Detail-Berichte sind eine ideale Option für die Visualisierung von 1: n Beziehungen und zum Anzeigen von Informationen aus besonders "Breite" Tabellen (von denen viele Spalten). Wir haben die Implementierung von Master/Detail-Berichte, die mit den GridView und DetailsView-Steuerelementen in vorherigen Tutorials untersucht. In diesem Tutorial und die nächsten beiden wir werden diese Konzepte, aber der Schwerpunkt auf die mit DataList nochmals aus, und stattdessen Repeater-Steuerelementen.

In diesem Tutorial betrachten wir, wie Sie mithilfe einer DropDownList für die "master"-Datensätze, die "details" Datensätze in einem DataList-Steuerelement.

## <a name="step-1-adding-the-masterdetail-tutorial-web-pages"></a>Schritt 1: Hinzufügen von Webseiten das Master/Detail-Tutorial

Bevor wir in diesem Tutorial beginnen, zuerst werfen wir einen Moment Zeit, fügen Sie den Ordner und ASP.NET-Seiten benötigen wir für dieses Lernprogramm sowie die nächsten beiden Umgang mit Master/Detail-Berichte, die mit den Steuerelementen DataList- oder Wiederholungssteuerelement. Zunächst erstellen Sie einen neuen Ordner im Projekt mit dem Namen `DataListRepeaterFiltering`. Fügen Sie die folgenden fünf ASP.NET-Seiten in diesen Ordner, wenn all diese Einstellungen zu konfigurieren, um die Masterseite verwenden `Site.master`:

- `Default.aspx`
- `FilterByDropDownList.aspx`
- `CategoryListMaster.aspx`
- `ProductsForCategoryDetails.aspx`
- `CategoriesAndProducts.aspx`


![Erstellen Sie einen Ordner DataListRepeaterFiltering, und fügen Sie die Tutorial ASP.NET-Seiten](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image1.png)

**Abbildung 1**: Erstellen einer `DataListRepeaterFiltering` Ordner, und fügen Sie dem Tutorial ASP.NET-Seiten hinzu


Öffnen Sie als Nächstes die `Default.aspx` Seite, und ziehen Sie die `SectionLevelTutorialListing.ascx` Benutzersteuerelement aus der `UserControls` Ordner auf die Entwurfsoberfläche. Dieses Benutzersteuerelement, die wir in den erstellt die [Masterseiten und Sitenavigation](../introduction/master-pages-and-site-navigation-vb.md) Tutorial, listet die Sitemap und zeigt Sie in den Tutorials aus dem aktuellen Abschnitt in einer Liste mit Aufzählungszeichen.


[![Fügen Sie das SectionLevelTutorialListing.ascx-Benutzersteuerelement an "default.aspx"](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image3.png)](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image2.png)

**Abbildung 2**: Hinzufügen der `SectionLevelTutorialListing.ascx` Benutzersteuerelement `Default.aspx` ([klicken Sie, um das Bild in voller Größe anzeigen](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image4.png))


Um die Anzeige der Liste mit Aufzählungszeichen müssen wir die Master-/Detail-Lernprogramme, die wir erstellen, die um sie der Sitemap hinzuzufügen. Öffnen der `Web.sitemap` Datei, und fügen Sie das folgende Markup nach "Anzeigen von Daten mit dem DataList und Repeater" Site Map Knoten Markup:

[!code-xml[Main](master-detail-filtering-with-a-dropdownlist-datalist-vb/samples/sample1.xml)]


![Aktualisieren Sie die Website-Karte, um die neuen ASP.NET-Seiten enthalten](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image5.png)

**Abbildung 3**: Aktualisieren Sie die Website-Karte, um die neuen ASP.NET-Seiten enthalten


## <a name="step-2-displaying-the-categories-in-a-dropdownlist"></a>Schritt 2: Anzeigen der Kategorien in einem DropDownList-Steuerelement

Unsere Master/Detail-Berichts Listet die Kategorien in einem DropDownList-Steuerelement, mit dem ausgewählten Listenelement Produkte angezeigt weiter unten auf der Seite in einem DataList-Steuerelement. Anschließend werden die erste Aufgabe vor uns, haben die Kategorien, die in einem DropDownList-Steuerelement angezeigt. Öffnen Sie zunächst die `FilterByDropDownList.aspx` auf der Seite die `DataListRepeaterFiltering` Ordner, und ziehen Sie einem DropDownList-Steuerelement aus der Toolbox auf der Seite Designer. Legen Sie als Nächstes die DropDownList `ID` Eigenschaft `Categories`. Klicken Sie auf den Link "Datenquelle auswählen" aus der Dropdownlistes Smarttag, und erstellen Sie eine neue, mit dem Namen "ObjectDataSource" `CategoriesDataSource`.


[![Fügen Sie eine neue, mit dem Namen CategoriesDataSource "ObjectDataSource"](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image7.png)](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image6.png)

**Abbildung 4**: Hinzufügen einer neuen "ObjectDataSource" mit dem Namen `CategoriesDataSource` ([klicken Sie, um das Bild in voller Größe anzeigen](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image8.png))


Die neue "ObjectDataSource" So konfigurieren, dass sie ruft die `CategoriesBLL` Klasse `GetCategories()` Methode. Nach dem Konfigurieren der müssen noch angeben, welches Feld der Datenquelle in der Dropdownliste angezeigt werden soll und welche "ObjectDataSource" sollte eine zugeordnet ist, als der Wert für jedes Listenelement. Haben die `CategoryName` -Feld als die Anzeige und `CategoryID` als Wert für jedes Listenelement.


[![Haben Sie die DropDownList-Anzeige "CategoryName" Feld aus, und verwenden CategoryID als Wert](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image10.png)](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image9.png)

**Abbildung 5**: haben Sie die DropDownList-Anzeige der `CategoryName` Feld und die Verwendung `CategoryID` als Wert ([klicken Sie, um das Bild in voller Größe anzeigen](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image11.png))


An diesem Punkt haben wir ein DropDownList-Steuerelement, das die Datensätze aus enthält die `Categories` Tabelle (alle erreicht, etwa sechs Sekunden). Abbildung 6 zeigt unseren Fortschritt bisher ein, wenn Sie über einen Browser angezeigt.


[![Eine Dropdown-Liste sind die aktuellen Kategorien aufgeführt.](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image13.png)](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image12.png)

**Abbildung 6**: ein Dropdownlisten der aktuellen Kategorien ([klicken Sie, um das Bild in voller Größe anzeigen](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image14.png))


## <a name="step-2-adding-the-products-datalist"></a>Schritt 2: Hinzufügen von Produkten DataList-Steuerelement

Der letzte Schritt in unserer Master/Detail-Berichts wird die ausgewählte Kategorie zugeordneten Produkte aufgeführt. Um dies zu erreichen, fügen Sie einem DataList-Steuerelement auf der Seite, und erstellen Sie eine neue, mit dem Namen "ObjectDataSource" `ProductsByCategoryDataSource`. Haben die `ProductsByCategoryDataSource` Steuerelement abzurufen, die Daten aus der die `ProductsBLL` Klasse `GetProductsByCategoryID(categoryID)` Methode. Da diese Master/Detail-Berichts schreibgeschützt ist, wählen Sie, dass die (None) auf den Registerkarten INSERT-, Update- und DELETE-option.


[![Wählen Sie die GetProductsByCategoryID(categoryID)-Methode](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image16.png)](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image15.png)

**Abbildung 7**: Wählen Sie die `GetProductsByCategoryID(categoryID)` Methode ([klicken Sie, um das Bild in voller Größe anzeigen](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image17.png))


Nach dem Klicken auf "Weiter" des Assistenten "ObjectDataSource" fragt uns nach der Quelle des Werts für die `GetProductsByCategoryID(categoryID)` Methode *`categoryID`* Parameter. Der Wert des ausgewählten `categories` DropDownList-Element legen Sie die Quelle für die Parameter zum Steuern und die ControlID zu `Categories`.


[![Legen Sie die CategoryID-Parameter auf den Wert von DropDownList Kategorien](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image19.png)](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image18.png)

**Abbildung 8**: Legen Sie die *`categoryID`* Parameter, um den Wert des der `Categories` DropDownList ([klicken Sie, um das Bild in voller Größe anzeigen](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image20.png))


Nach Abschluss des Konfigurieren von Datenquellen-Assistenten, Visual Studio generiert automatisch ein `ItemTemplate` für DataList-Steuerelement, das den Namen und Wert der einzelnen Datenfelder anzeigt. Erweitern wir jetzt DataList-Steuerelement zu verwenden. eine `ItemTemplate` , anzeigt, nur den Namen des Produkts, Kategorie, Lieferanten, Menge pro Einheit und Preis zusammen mit einer `SeparatorTemplate` einfügt, die eine `<hr>` Element zwischen den einzelnen Elementen. Ich verwende die `ItemTemplate` aus einem Beispiel in der [Anzeigen von Daten mit dem DataList- und Wiederholungssteuerelement](../displaying-data-with-the-datalist-and-repeater/displaying-data-with-the-datalist-and-repeater-controls-vb.md) Tutorial, aber gerne verwenden alle Vorlagenmarkup am visuell ansprechend finden Sie.

Nach diesen Änderungen sollte Ihre DataList-Steuerelement und seine "ObjectDataSource" Markup etwa wie folgt aussehen:

[!code-aspx[Main](master-detail-filtering-with-a-dropdownlist-datalist-vb/samples/sample2.aspx)]

Nehmen Sie einen Moment Zeit, unseren Fortschritt in einem Browser ausprobieren. Wenn die Seite zuerst besuchen zu können, werden diese Produkte der ausgewählten Kategorie (Getränke) angezeigt, (wie in Abbildung 9 gezeigt), aber DropDownList ändern nicht die Daten aktualisiert. Dies ist, da ein Postback, damit DataList-Steuerelement auftreten muss aktualisieren. Legen Sie hierfür entweder können die DropDownList `AutoPostBack` Eigenschaft `true` oder ein Steuerelement Schaltfläche auf der Seite hinzufügen. Für dieses Tutorial, ich habe mich entschieden habe zum Festlegen des DropDownList `AutoPostBack` Eigenschaft `true`.

Abbildung 9 und 10 veranschaulichen die Master/Detail-Berichts in Aktion.


[![Wenn die Seite zuerst besuchen zu können, werden die trinken Produkte angezeigt.](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image22.png)](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image21.png)

**Abbildung 9**: die trinken-Produkte werden angezeigt, wenn die Seite zuerst besuchen zu können, ([klicken Sie, um das Bild in voller Größe anzeigen](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image23.png))


[![Beim Auswählen eines neuen Produkts (erstellen) automatisch bewirkt, dass ein PostBack, DataList-Steuerelement wird aktualisiert.](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image25.png)](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image24.png)

**Abbildung 10**: Auswählen eines neuen Produkts (generieren) werden auch automatisch bewirkt, dass ein PostBack, DataList-Steuerelement wird aktualisiert ([klicken Sie, um das Bild in voller Größe anzeigen](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image26.png))


## <a name="adding-a----choose-a-category----list-item"></a>Hinzufügen eines Listenelements "– eine Kategorie auswählen:"

Beim ersten Zugriff auf die `FilterByDropDownList.aspx` Seite die Kategorien DropDownLists ersten Listenelements (Getränke) ist standardmäßig aktiviert und die Produkte Getränke im DataList-Steuerelement angezeigt. In der *Master/Detail-Filtern mit einer DropDownList* Tutorial, die wir, eine Option hinzugefügt "--eine Kategorie auswählen:" DropDownList, die standardmäßig ausgewählt und wurde, angezeigt, wenn ausgewählt, *alle* von der Produkte in der Datenbank. Dieser Ansatz war verwaltbare beim Auflisten der Produkte in einer GridView-Ansicht, wie jede Produktzeile um eine kleine Menge Bildschirmplatz gedauert hat. Mit dem DataList-Steuerelement beansprucht jedoch des Produkts Informationen einem viel größeren Block des Bildschirms. Lassen Sie uns noch eine Option "--eine Kategorie auswählen:" hinzufügen und es standardmäßig ausgewählt, aber anstatt alle Produkte angezeigt. bei Auswahl dieser Option lassen Sie uns konfigurieren, damit es keine Produkte angezeigt.

Um ein neues Listenelement DropDownList hinzuzufügen, wechseln Sie zu dem Fenster "Eigenschaften", und klicken Sie auf die Auslassungspunkte in der `Items` Eigenschaft. Hinzufügen ein neuen Listenelements mit dem `Text` "– eine Kategorie auswählen:" und die `Value` `0`.


![Hinzufügen einer](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image27.png)

**Abbildung 11**: Hinzufügen eines Listenelements "– eine Kategorie auswählen:"


Alternativ können Sie das Listenelement hinzufügen, indem Sie das folgende Markup DropDownList hinzufügen:

[!code-aspx[Main](master-detail-filtering-with-a-dropdownlist-datalist-vb/samples/sample3.aspx)]

Darüber hinaus müssen wir die DropDownList-Steuerelement festgelegt `AppendDataBoundItems` zu `true` da Wenn festgelegt ist `false` (Standardeinstellung), wenn die Kategorien zu DropDownList aus dem ObjectDataSource-Steuerelement gebunden sind überschreiben diese werden alle manuell hinzugefügten Liste die Elemente.


![Legen Sie die AppendDataBoundItems-Eigenschaft auf "true"](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image28.png)

**Abbildung 12**: Legen Sie die `AppendDataBoundItems` Eigenschaft auf "true"


Der Grund wählen wir den Wert `0` Element für die Liste "– eine Kategorie auswählen:" ist, da keine Kategorien vorhanden, in das System mit einem Wert von sind `0`, daher keine Produktdatensätze werden zurückgegeben, wenn das Listenelement "– eine Kategorie auswählen:" ausgewählt ist. Nehmen Sie einen Moment Zeit, um die Seite über einen Browser besuchen, um dies zu bestätigen. Wie in Abbildung 13 dargestellt, wenn zunächst Anzeige der Seite das Listenelement "– eine Kategorie auswählen:" ausgewählt ist, und keine Produkte angezeigt werden.


[![Wenn die](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image30.png)](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image29.png)

**Abbildung 13**: No-Produkte werden angezeigt, wenn das Listenelement "– eine Kategorie auswählen:" aktiviert ist, ([klicken Sie, um das Bild in voller Größe anzeigen](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image31.png))


Wenn Sie stattdessen anzeigen würde *alle* der Produkte, wenn die Option "--eine Kategorie auswählen:" ausgewählt ist, verwenden Sie den Wert `-1` stattdessen. Aufmerksame Leser werden sich daran erinnern, wieder in die *Master/Detail-Filtern mit einer DropDownList* Tutorial wir aktualisiert die `ProductsBLL` -Klasse `GetProductsByCategoryID(categoryID)` Methode so, dass wenn eine *`categoryID`* Wert des `-1` übergeben wurde, alle Produkt Datensätze zurückgegeben wurden.

## <a name="summary"></a>Zusammenfassung

Zum Anzeigen von hierarchisch-bezogenen Daten leichter häufig zur Darstellung der Daten mithilfe von Master/Detail-Berichte, aus denen kann der Benutzer starten darauf verwenden, die Daten aus der oben in der Hierarchie und einen Drilldown in die Details. In diesem Tutorial untersucht wir das Erstellen eines einfachen Master/Detail-Berichts, die Produkte einer ausgewählten Kategorie angezeigt. Dies wurde mithilfe einer DropDownList für die Liste der Kategorien und einem DataList-Steuerelement für die Produkte, die der ausgewählten Kategorie gehören.

Im nächsten Tutorial betrachten wir die Datensätze Master und Details über zwei Seiten aufteilen. In der ersten Seite wird eine Liste der "master"-Datensätze mit einem Link zum Anzeigen der Details angezeigt werden. Auf den Link wird den Benutzer auf der zweiten Seite weiter, der die Details für den ausgewählten master Datensatz angezeigt wird.

Viel Spaß beim Programmieren!

## <a name="about-the-author"></a>Der Autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor von sieben Büchern zu ASP/ASP.NET und Gründer von [4GuysFromRolla.com](http://www.4guysfromrolla.com), arbeitet mit Microsoft-Web-Technologien seit 1998. Er ist als ein unabhängiger Berater, Schulungsleiter und Autor. Sein neueste Buch wird [*Sams Schulen selbst ASP.NET 2.0 in 24 Stunden*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Er ist unter [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) oder über seinen Blog finden Sie unter [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Besonderen Dank an...

Diese tutorialreihe wurde durch viele hilfreiche Reviewer überprüft. Führendes Prüfer für dieses Tutorial wurde Randy Schmidt. Meine zukünftigen MSDN-Artikeln überprüfen möchten? Wenn dies der Fall ist, löschen Sie mir eine Linie an [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Zurück](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs.md)
> [Weiter](master-detail-filtering-acess-two-pages-datalist-vb.md)
