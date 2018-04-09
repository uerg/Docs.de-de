---
uid: web-forms/overview/data-access/database-driven-site-maps/building-a-custom-database-driven-site-map-provider-vb
title: Erstellen eine benutzerdefinierte Website mit Datenbank-Driven Kartenanbieter (VB) | Microsoft Docs
author: rick-anderson
description: Der Standardanbieter der Siteübersicht in ASP.NET 2.0 ruft seine Daten aus einer statischen XML-Datei ab. Während der XML-basierte Anbieter so viele kleine und mittlere ößen-ist...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/26/2007
ms.topic: article
ms.assetid: f904cd2c-a408-4484-9324-8b8d7fe33893
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/database-driven-site-maps/building-a-custom-database-driven-site-map-provider-vb
msc.type: authoredcontent
ms.openlocfilehash: df295f1b8bf0b83647ffb90501936181894634d7
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
---
<a name="building-a-custom-database-driven-site-map-provider-vb"></a>Erstellen eine benutzerdefinierte Website mit Datenbank-Driven Kartenanbieter (VB)
====================
durch [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Herunterladen von Code](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_62_VB.zip) oder [PDF herunterladen](building-a-custom-database-driven-site-map-provider-vb/_static/datatutorial62vb1.pdf)

> Der Standardanbieter der Siteübersicht in ASP.NET 2.0 ruft seine Daten aus einer statischen XML-Datei ab. Während der XML-basierte Anbieter zu viele kleine und mittelgroße Websites geeignet ist, erfordern größere Webanwendungen eine dynamischere Siteübersicht. In diesem Lernprogramm erstellen wir eine benutzerdefinierte Website Kartenanbieter, die die Daten aus der Business Logic Layer abruft, die wiederum Daten aus der Datenbank abgerufen.


## <a name="introduction"></a>Einführung

ASP.NET 2.0 s Site Map-Funktion kann der Seitenentwickler eine Siteübersicht des Web-Anwendung s in einige persistent "Mittel", z. B. in eine XML-Datei zu definieren. Sobald definiert, die Siteübersichtsdaten möglich programmgesteuert über die [ `SiteMap` Klasse](https://msdn.microsoft.com/library/system.web.sitemap.aspx) in der [ `System.Web` Namespace](https://msdn.microsoft.com/library/system.web.aspx) oder über eine Reihe von Navigation Web-Steuerelemente, z. B. die SiteMapPath, Menüs und TreeView-Steuerelemente. Verwendet das Standortsystem für die Zuordnung der [Anbietermodell](http://aspnet.4guysfromrolla.com/articles/101905-1.aspx) , damit anderen Standort Zuordnung Serialisierung Implementierungen erstellt und an eine Webanwendung angeschlossen werden können. Der Standardanbieter der Siteübersicht, die mit ASP.NET 2.0 geliefert wird, behält Siteübersichtsstruktur in eine XML-Datei. Wieder in die [Masterseiten und Websitenavigation](../introduction/master-pages-and-site-navigation-vb.md) Lernprogramm wir eine Datei namens erstellt `Web.sitemap` , die dieser Struktur enthalten und haben mit jedem neuen Tutorial Abschnitt der XML-aktualisiert wurde.

Die XML-basierte Standardanbieter der Siteübersicht funktioniert gut, wenn die s Siteübersichtsstruktur für diese Lernprogramme relativ statisch sein, z. B. ist. In vielen Szenarien ist jedoch eine dynamischere Siteübersicht erforderlich. Betrachten Sie die Siteübersicht, siehe Abbildung 1, wobei jede Kategorie und jedes Produkt als Abschnitte in der Website-s-Struktur angezeigt. Mit dieser Siteübersicht möglicherweise Besuch einer Webseite, den Stammknoten entspricht der Kategorien Auflisten aller während einer bestimmten Kategorie s-Webseite besuchen würden diese Kategorie s Produkte aufgeführt, und ein bestimmtes Produkt s-Webseite anzeigen dieses Produkt s Details anzeigen möchten.


[![Die Kategorien und Produkte Zusammensetzung Siteübersichtsstruktur s](building-a-custom-database-driven-site-map-provider-vb/_static/image1.gif)](building-a-custom-database-driven-site-map-provider-vb/_static/image1.png)

**Abbildung 1**: die Kategorien und Produkte Zusammensetzung der Siteübersicht-s-Struktur ([klicken Sie hier, um das Bild in voller Größe angezeigt](building-a-custom-database-driven-site-map-provider-vb/_static/image2.png))


Während dieser Struktur Category und Product-basierte hartcodiert werden, konnte die `Web.sitemap` Datei, die Datei jedes Mal eine Kategorie aktualisiert werden müssen oder Produkt hinzugefügt, entfernt oder umbenannt wurde. Daher würde der Karte Standortwartungstask maßgeblich vereinfacht werden, wenn die Struktur aus der Datenbank oder im Idealfall aus der Business Logic Layer der Anwendungsarchitektur s abgerufen wurde. Auf diese Weise, wie Produkte und Kategorien hinzugefügt wurden, umbenannt oder gelöscht, die Siteübersicht würden automatisch aktualisiert werden, um diese Änderungen widerzuspiegeln.

Seit ASP.NET 2.0 s Standort Zuordnung Serialisierung Anbietermodell aufbaut, können wir unser eigenes Kartenanbieter benutzerdefinierte Website erstellen, die zugehörigen Daten aus einer alternativen Datenspeicher, z. B. die Datenbank oder der Architektur grabs. In diesem Lernprogramm fügen wir einen benutzerdefinierten Anbieter erstellen, der die Daten aus der BLL abruft. Lassen Sie s beginnen!

> [!NOTE]
> Die benutzerdefinierte Website Kartenanbieter in diesem Lernprogramm erstellten ist eng an der Anwendung s-Architektur und -Datenmodell verbunden. Jeff Prosise s [Sitemaps in SQL Server speichern](https://msdn.microsoft.com/msdnmag/issues/05/06/WickedCode/) und [der SQL-Standort-Kartenanbieter Sie Ve gewartet wurde](https://msdn.microsoft.com/msdnmag/issues/06/02/wickedcode/default.aspx) Artikeln untersuchen einen generalisierten Ansatz zum Speichern von Siteübersichtsdaten in SQL Server.


## <a name="step-1-creating-the-custom-site-map-provider-web-pages"></a>Schritt 1: Erstellen, die benutzerdefinierte Zuordnung Anbieter Webseiten

Bevor wir beginnen, erstellen eine benutzerdefinierte Website Kartenanbieter, können Sie zunächst die ASP.NET-Seiten hinzufügen, die wir für dieses Lernprogramm benötigen s an. Starten, indem Sie einen neuen Ordner namens `SiteMapProvider`. Fügen Sie die folgenden ASP.NET-Seiten in diesen Ordner, und dabei sicherstellen, ordnen Sie jede Seite mit den `Site.master` Gestaltungsvorlage:

- `Default.aspx`
- `ProductsByCategory.aspx`
- `ProductDetails.aspx`

Auch hinzufügen, eine `CustomProviders` Unterordner, um die `App_Code` Ordner.


![Fügen Sie die ASP.NET-Seiten für die Website Zuordnung anbieterbezogenen-Lernprogramme](building-a-custom-database-driven-site-map-provider-vb/_static/image2.gif)

**Abbildung 2**: Fügen Sie die ASP.NET-Seiten für den Standort zuordnen anbieterbezogenen Lernprogramme


Da es nur ein Lernprogramm für diesen Abschnitt ist, Verschlüsselungskennwort wir t müssen `Default.aspx` die Lernprogramme Abschnitt s aufgelistet. Stattdessen `Default.aspx` werden die Kategorien in einem GridView-Steuerelement angezeigt. Wir werden diese in Schritt2 konfigurieren.

Aktualisieren Sie als Nächstes `Web.sitemap` enthalten einen Verweis auf die `Default.aspx` Seite. Fügen Sie das folgende Markup insbesondere nach dem Caching `<siteMapNode>`:


[!code-xml[Main](building-a-custom-database-driven-site-map-provider-vb/samples/sample1.xml)]

Nach der Aktualisierung `Web.sitemap`, nehmen einen Moment Zeit, um die Lernprogramme-Website über einen Browser anzuzeigen. Klicken Sie im Menü auf der linken Seite enthält nun ein Element für das einzige Standort Zuordnung Anbieter-Lernprogramm.


![Die Siteübersicht enthält nun einen Eintrag für die Website Zuordnung Anbieter-Lernprogramm](building-a-custom-database-driven-site-map-provider-vb/_static/image3.gif)

**Abbildung 3**: die Siteübersicht enthält nun einen Eintrag für die Website Zuordnung Anbieter-Lernprogramm


Dieses Lernprogramm s hauptsächlich ist zur Veranschaulichung erstellen eine benutzerdefinierte Website Kartenanbieter und Konfiguration einer Webanwendung für die Verwendung dieses Anbieters. Insbesondere müssen Sie einen Anbieter erstellen, der eine Siteübersicht, die zusammen mit einem Knoten für jede Kategorie und jedes Produkt, einen Stammknoten enthält zurückgibt, wie in Abbildung 1 dargestellt. Jeder Knoten in der Siteübersicht kann im Allgemeinen eine URL angeben. Für unsere Siteübersicht Stamm-URL des Knotens s werden `~/SiteMapProvider/Default.aspx`, dem Listet alle Kategorien in der Datenbank. Jede Kategorieknoten in der Siteübersicht müssen eine URL, die auf zeigt `~/SiteMapProvider/ProductsByCategory.aspx?CategoryID=categoryID`, dem Listet alle Produkte in der angegebenen *CategoryID*. Schließlich wird jedes Produkt Zuordnung Websiteknoten zeigen Sie auf `~/SiteMapProvider/ProductDetails.aspx?ProductID=productID`, wird die Details zu bestimmten Produkt s angezeigt.

Starten wir erstellen müssen die `Default.aspx`, `ProductsByCategory.aspx`, und `ProductDetails.aspx` Seiten. Diese Seiten werden in den Schritten 2, 3 und 4, abgeschlossen. Da der Großteil der in diesem Lernprogramm zum Anbietern der Siteübersicht ist und seit der letzten Lernprogramme erstellen untersucht haben meldet diese Arten von mehreren Seiten Master/Detail, wir über die Schritte 2 bis 4 Beeilen Sie sich wird. Wenn die gewünschte aufzufrischen zum Erstellen von Master-/Detail-Berichten, die über mehrere Seiten erstrecken, verweisen Sie zurück auf die [Master/Detail-Filterung über zwei Seiten](../masterdetail/master-detail-filtering-across-two-pages-vb.md) Lernprogramm.

## <a name="step-2-displaying-a-list-of-categories"></a>Schritt 2: Anzeigen einer Liste von Kategorien

Öffnen der `Default.aspx` auf der Seite der `SiteMapProvider` Ordner, und ziehen Sie eine GridView aus der Toolbox in den Designer, Festlegen seiner `ID` auf `Categories`. Aus den GridView s smart Tag, binden Sie es auf eine neue ObjectDataSource mit dem Namen `CategoriesDataSource` und ihn so konfigurieren, dass, die Daten mithilfe abgerufen der `CategoriesBLL` Klasse s `GetCategories` Methode. Da diese GridView nur die Kategorien zeigt und Möglichkeiten zur Datenänderung bietet, legen Sie die Dropdownlisten in der Update-, INSERT-, und Löschen von Registerkarten (keine).


[![Konfigurieren der ObjectDataSource zum Zurückgeben von Kategorien, die mit der GetCategories-Methode](building-a-custom-database-driven-site-map-provider-vb/_static/image4.gif)](building-a-custom-database-driven-site-map-provider-vb/_static/image3.png)

**Abbildung 4**: Konfigurieren der ObjectDataSource zurückgeben Kategorien mit den `GetCategories` Methode ([klicken Sie hier, um das Bild in voller Größe angezeigt](building-a-custom-database-driven-site-map-provider-vb/_static/image4.png))


[![Legen Sie die Dropdownlisten in der Update-, INSERT- und Löschen von Registerkarten auf (keine)](building-a-custom-database-driven-site-map-provider-vb/_static/image5.gif)](building-a-custom-database-driven-site-map-provider-vb/_static/image5.png)

**Abbildung 5**: Legen Sie das Dropdown-Listen in aktualisieren, einfügen und Löschen von Registerkarten auf (keine) ([klicken Sie hier, um das Bild in voller Größe angezeigt](building-a-custom-database-driven-site-map-provider-vb/_static/image6.png))


Nach Abschluss des Assistenten für die Datenquelle konfigurieren Visual Studio ein BoundField für fügen `CategoryID`, `CategoryName`, `Description`, `NumberOfProducts`, und `BrochurePath`. Die GridView bearbeiten, sodass sie nur enthält die `CategoryName` und `Description` BoundFields und Aktualisieren der `CategoryName` BoundField-s `HeaderText` Eigenschaft zur Kategorie.

Als Nächstes fügen Sie eine HyperLinkField hinzu und positionieren Sie ihn so, dass die It s das linke Feld. Festlegen der `DataNavigateUrlFields` Eigenschaft `CategoryID` und die `DataNavigateUrlFormatString` Eigenschaft `~/SiteMapProvider/ProductsByCategory.aspx?CategoryID={0}`. Legen Sie die `Text` Eigenschaft, um Produkte anzuzeigen.


![Eine HyperLinkField an die GridView Kategorien hinzufügen](building-a-custom-database-driven-site-map-provider-vb/_static/image6.gif)

**Abbildung 6**: Hinzufügen einer HyperLinkField auf die `Categories` GridView


Nach dem Erstellen von "ObjectDataSource", und die GridView-s-Feldern anpassen, werden die beiden Steuerelemente deklarativem Markup wie folgt aussehen:


[!code-aspx[Main](building-a-custom-database-driven-site-map-provider-vb/samples/sample2.aspx)]

Abbildung 7 zeigt `Default.aspx` über einen Browser angezeigt. Klicken auf eine Kategorie s Produkte anzeigen Link gelangen Sie zur `ProductsByCategory.aspx?CategoryID=categoryID`, die wir in Schritt 3 erstellt wird.


[![Jede Kategorie ist aufgeführten zusammen mit einer Ansichtsverknüpfung-Produkte](building-a-custom-database-driven-site-map-provider-vb/_static/image7.gif)](building-a-custom-database-driven-site-map-provider-vb/_static/image7.png)

**Abbildung 7**: jede Kategorie aufgeführten zusammen mit einer Ansicht Produkte Verknüpfung ist ([klicken Sie hier, um das Bild in voller Größe angezeigt](building-a-custom-database-driven-site-map-provider-vb/_static/image8.png))


## <a name="step-3-listing-the-selected-category-s-products"></a>Schritt 3: Auflisten von der ausgewählten Kategorie-s-Produkte

Öffnen der `ProductsByCategory.aspx` Seite, und fügen Sie eine GridView, benennen es `ProductsByCategory`. Binden Sie aus seinem Smarttag GridView an eine neue, mit dem Namen ObjectDataSource `ProductsByCategoryDataSource`. Konfigurieren der ObjectDataSource verwenden die `ProductsBLL` Klasse s `GetProductsByCategoryID(categoryID)` -Methode, und legen die Dropdownlisten auf (keine) auf den Registerkarten Update-, INSERT- und DELETE.


[![Verwenden Sie die ProductsBLL Klasse s GetProductsByCategoryID(categoryID)-Methode](building-a-custom-database-driven-site-map-provider-vb/_static/image8.gif)](building-a-custom-database-driven-site-map-provider-vb/_static/image9.png)

**Abbildung 8**: Verwenden der `ProductsBLL` Klasse s `GetProductsByCategoryID(categoryID)` Methode ([klicken Sie hier, um das Bild in voller Größe angezeigt](building-a-custom-database-driven-site-map-provider-vb/_static/image10.png))


Der letzte Schritt im Konfigurieren von Datenquellen-Assistenten aufgefordert, eine Parameterquelle für *CategoryID*. Da diese Informationen über das Feld "Querystring" übergeben wird `CategoryID`, wählen Sie die Abfragezeichenfolge aus der Dropdown-Liste und in das Textfeld QueryStringField CategoryID eingeben, wie in Abbildung 9 gezeigt. Klicken Sie auf ' Fertig stellen ', um den Assistenten abzuschließen.


[![Verwenden Sie das CategoryID Querystring-Feld für die CategoryID-Parameter](building-a-custom-database-driven-site-map-provider-vb/_static/image9.gif)](building-a-custom-database-driven-site-map-provider-vb/_static/image11.png)

**Abbildung 9**: Verwenden der `CategoryID` Querystring-Field für die *CategoryID* Parameter ([klicken Sie hier, um das Bild in voller Größe angezeigt](building-a-custom-database-driven-site-map-provider-vb/_static/image12.png))


Nach Abschluss des Assistenten wird Visual Studio entsprechenden BoundFields und eine CheckBoxField an die GridView für die Product-Datenfelder hinzufügen. Entfernen Sie alle außer den `ProductName`, `UnitPrice`, und `SupplierName` BoundFields. Diese drei BoundFields anpassen `HeaderText` Eigenschaften, Produkt, Preis und Lieferanten, zu lesen. Format der `UnitPrice` BoundField als Währung.

Als Nächstes fügen Sie eine HyperLinkField hinzu, und verschieben Sie sie auf die linke Position. Festlegen seiner `Text` Eigenschaft zum Anzeigen von Details seiner `DataNavigateUrlFields` Eigenschaft, um `ProductID`, und die zugehörige `DataNavigateUrlFormatString` Eigenschaft, um `~/SiteMapProvider/ProductDetails.aspx?ProductID={0}`.


![Hinzufügen einer Ansicht Details HyperLinkField, die auf ProductDetails.aspx verweist](building-a-custom-database-driven-site-map-provider-vb/_static/image10.gif)

**Abbildung 10**: Hinzufügen einer Detailansicht HyperLinkField, die auf verweist `ProductDetails.aspx`


Nachdem Sie diese Anpassungen vornehmen, sollte die GridView und ObjectDataSource s deklarative Markup folgendermaßen aussehen:


[!code-aspx[Main](building-a-custom-database-driven-site-map-provider-vb/samples/sample3.aspx)]

Kehren Sie zum Anzeigen von zurück `Default.aspx` für Getränke über einen Browser und klicken Sie auf die Ansicht Produkte zu verknüpfen. Dadurch gelangen Sie zur `ProductsByCategory.aspx?CategoryID=1`, Anzeigen von Namen, Preise und Lieferanten der Produkte in der Northwind-Datenbank, die die Kategorie "Getränke" angehören (siehe Abbildung 11). Wahlweise können Sie diese Seite, um enthalten einen Link, um Benutzer zur Kategorie Angebotsseite zurückzukehren, um weiter zu verbessern (`Default.aspx`) und ein DetailsView oder FormView-Steuerelement, in der ausgewählten Kategorie s-Name und Beschreibung angezeigt.


[![Getränke Namen, Preise und Lieferanten werden angezeigt.](building-a-custom-database-driven-site-map-provider-vb/_static/image11.gif)](building-a-custom-database-driven-site-map-provider-vb/_static/image13.png)

**Abbildung 11**: die Namen der Getränke, Preise und Lieferanten angezeigt werden ([klicken Sie hier, um das Bild in voller Größe angezeigt](building-a-custom-database-driven-site-map-provider-vb/_static/image14.png))


## <a name="step-4-showing-a-product-s-details"></a>Schritt 4: Anzeigen einer s Produktdetails

Die letzte Seite `ProductDetails.aspx`, zeigt die Details für die ausgewählten Produkte. Open `ProductDetails.aspx` und eine DetailsView aus der Toolbox in den Designer ziehen. Legen Sie die s DetailsView `ID` Eigenschaft, um `ProductInfo` und löschen, dessen `Height` und `Width` Eigenschaftswerte. Binden Sie aus seinem Smarttag, DetailsView an eine neue ObjectDataSource mit dem Namen `ProductDataSource`, konfigurieren das ObjectDataSource zum Abrufen der zugehörigen Daten aus der `ProductsBLL` Klasse s `GetProductByProductID(productID)` Methode. Wie bei den vorherigen Web-Seiten in den Schritten 2 und 3 erstellt haben, legen Sie die Dropdownlisten in der Update-, INSERT- und Löschen von Registerkarten (keine).


[![Konfigurieren der ObjectDataSource zur Verwendung der GetProductByProductID(productID)-Methode](building-a-custom-database-driven-site-map-provider-vb/_static/image12.gif)](building-a-custom-database-driven-site-map-provider-vb/_static/image15.png)

**Abbildung 12**: Konfigurieren der ObjectDataSource verwenden die `GetProductByProductID(productID)` Methode ([klicken Sie hier, um das Bild in voller Größe angezeigt](building-a-custom-database-driven-site-map-provider-vb/_static/image16.png))


Der letzte Schritt des Assistenten zum Konfigurieren von Datenquellen für die Quelle des fordert die *"ProductID"* Parameter. Da diese Daten über das Feld "Querystring" stammen `ProductID`, legen Sie die Dropdownliste auf QueryString und QueryStringField Textbox "ProductID". Klicken Sie abschließend auf "Fertig stellen", um den Assistenten abzuschließen.


[![Konfigurieren Sie die Parameter zum Abrufen von seinen Werts von "ProductID" Querystring-Feld "ProductID"](building-a-custom-database-driven-site-map-provider-vb/_static/image13.gif)](building-a-custom-database-driven-site-map-provider-vb/_static/image17.png)

**Abbildung 13**: Konfigurieren der *"ProductID"* Parameter zum Abrufen des Werts aus der `ProductID` Querystring-Field ([klicken Sie hier, um das Bild in voller Größe angezeigt](building-a-custom-database-driven-site-map-provider-vb/_static/image18.png))


Nach Abschluss des Assistenten für die Datenquelle konfigurieren erstellt Visual Studio entsprechenden BoundFields und eine CheckBoxField in DetailsView für das Produkt von Datenfeldern. Entfernen Sie die `ProductID`, `SupplierID`, und `CategoryID` BoundFields und konfigurieren Sie die verbleibenden Felder aus, nach Bedarf. Nachdem eine Handvoll Layoutgründen Konfigurationen die meine, DetailsView und ObjectDataSource, deklarative s-Markup wie folgt gesucht:


[!code-aspx[Main](building-a-custom-database-driven-site-map-provider-vb/samples/sample4.aspx)]

Um diese Seite zu testen, zurück zu `Default.aspx` , und klicken Sie auf Produkte anzeigen, für die Kategorie "Getränke". Klicken Sie aus der Auflistung der Getränke Produkte an auf den Link "Details anzeigen" für Chai Tee. Dadurch gelangen Sie zur `ProductDetails.aspx?ProductID=1`, es zeigt eine Chai Tee s Details (siehe Abbildung 14).


[![Chai Tee s Lieferanten, Kategorie, Preis und andere Informationen wird angezeigt.](building-a-custom-database-driven-site-map-provider-vb/_static/image14.gif)](building-a-custom-database-driven-site-map-provider-vb/_static/image19.png)

**Abbildung 14**: Chai Tee s Lieferanten, Kategorie, Preis und andere Informationen wird angezeigt ([klicken Sie hier, um das Bild in voller Größe angezeigt](building-a-custom-database-driven-site-map-provider-vb/_static/image20.png))


## <a name="step-5-understanding-the-inner-workings-of-a-site-map-provider"></a>Schritt 5: Verstehen der internen Funktionsweise von einem Kartenanbieter Standort

Die Siteübersicht liegt in der Web-Server-s-Speicher als eine Auflistung von `SiteMapNode` Instanzen, die eine Hierarchie zu bilden. Es muss genau einen Stammknoten, alle nicht-Stammknoten müssen genau einen übergeordneten Knoten haben und alle Knoten handelt es sich möglicherweise um eine beliebige Anzahl von untergeordneten Elementen. Jede `SiteMapNode` Objekt stellt einen Abschnitt in der Website-s-Struktur dar; diese Abschnitte verfügen häufig über eine entsprechende Webseite. Folglich die [ `SiteMapNode` Klasse](https://msdn.microsoft.com/library/system.web.sitemapnode.aspx) verfügt über Eigenschaften, z. B. `Title`, `Url`, und `Description`, die Aufschluss für den Abschnitt der `SiteMapNode` darstellt. Es gibt auch eine `Key` Eigenschaft, die jeweils eindeutig `SiteMapNode` in der Hierarchie sowie Eigenschaften verwendet, um diese Hierarchie herzustellen `ChildNodes`, `ParentNode`, `NextSibling`, `PreviousSibling`usw. lauten.

Abbildung 15 zeigt allgemeine Siteübersichtsstruktur in Abbildung 1, jedoch mit den Implementierungsdetails zu skizziert genauere Details.


[![Jede SiteMapNode verfügt über Eigenschaften wie Titel, Url, Schlüssel und usw.](building-a-custom-database-driven-site-map-provider-vb/_static/image16.gif)](building-a-custom-database-driven-site-map-provider-vb/_static/image15.gif)

**Abbildung 15**: jede `SiteMapNode` verfügt über Eigenschaften wie `Title`, `Url`, `Key`usw. ([klicken Sie hier, um das Bild in voller Größe angezeigt](building-a-custom-database-driven-site-map-provider-vb/_static/image17.gif))


Die Siteübersicht Zugriff erfolgt über die [ `SiteMap` Klasse](https://msdn.microsoft.com/library/system.web.sitemap.aspx) in der [ `System.Web` Namespace](https://msdn.microsoft.com/library/system.web.aspx). Diese Klasse s `RootNode` Eigenschaft gibt das Stammverzeichnis der Site-Zuordnung s `SiteMapNode` Instanz. `CurrentNode` gibt die `SiteMapNode` , deren `Url` Eigenschaft entspricht die URL der derzeit angeforderten Seite. Diese Klasse wird intern von ASP.NET 2.0 s Web Navigationssteuerelemente.

Wenn die `SiteMap` Klasseneigenschaften s erfolgt, muss die Siteübersichtsstruktur aus einigen permanenten Medium in den Speicher serialisieren. Allerdings Serialisierungslogik der Site-Zuordnung ist nicht hartcodiert in die `SiteMap` Klasse. Stattdessen zur Laufzeit die `SiteMap` -Klasse bestimmt, welche Siteübersicht *Anbieter* für die Serialisierung verwendet. Wird standardmäßig die [ `XmlSiteMapProvider` Klasse](https://msdn.microsoft.com/library/system.web.xmlsitemapprovider.aspx) verwendet wird, die Siteübersichtsstruktur s aus einer ordnungsgemäß formatierten XML-Datei liest. Jedoch können wir unser eigenes Kartenanbieter benutzerdefinierte Website mit ein wenig Arbeit erstellen.

Von allen Anbietern der Siteübersicht abgeleitet werden die [ `SiteMapProvider` Klasse](https://msdn.microsoft.com/library/system.web.sitemapprovider.aspx), darunter die wichtigen Methoden und Eigenschaften für die Website erforderlich sind Anbieter zuordnen, lässt aber viele Implementierungsdetails. Eine zweite Klasse [ `StaticSiteMapProvider` ](https://msdn.microsoft.com/library/system.web.staticsitemapprovider.aspx), erweitert der `SiteMapProvider` Klasse und enthält eine robustere Implementierung erforderlichen Funktionen. Intern die `StaticSiteMapProvider` speichert die `SiteMapNode` Instanzen von der Website zuordnen, einer `Hashtable` und bietet Methoden wie `AddNode(child, parent)`, `RemoveNode(siteMapNode),` und `Clear()` , das Hinzufügen und Entfernen von `SiteMapNode` s mit dem internen `Hashtable`. `XmlSiteMapProvider` wird von `StaticSiteMapProvider` abgeleitet.

Beim Erstellen einer benutzerdefinierten Website Kartenanbieter, erweitert `StaticSiteMapProvider`, es gibt zwei abstrakte Methoden, die überschrieben werden müssen: [ `BuildSiteMap` ](https://msdn.microsoft.com/library/system.web.staticsitemapprovider.buildsitemap.aspx) und [ `GetRootNodeCore` ](https://msdn.microsoft.com/library/system.web.sitemapprovider.getrootnodecore.aspx). `BuildSiteMap`, wie der Name schon sagt, ist verantwortlich für das Laden der Siteübersichtsstruktur aus dem permanenten Speicher, und erstellen es im Arbeitsspeicher. `GetRootNodeCore` Gibt den Stammknoten in der Siteübersicht zurück.

Die Gültigkeit einer können Anwendung eine Website Kartenanbieter, die sie in der Anwendungskonfiguration s registriert werden muss. Wird standardmäßig die `XmlSiteMapProvider` Klasse registriert ist, mit dem Namen `AspNetXmlSiteMapProvider`. Um zusätzliche Website Zuordnung Anbieter zu registrieren, fügen Sie das folgende Markup zum Rendern `Web.config`:


[!code-xml[Main](building-a-custom-database-driven-site-map-provider-vb/samples/sample5.xml)]

Die *Namen* Wert weist einen menschenlesbaren Namen für den Anbieter beim *Typ* gibt den vollqualifizierten Typnamen des Site-Zuordnung-Anbieters. Genauer konkrete Werte für die *Namen* und *Typ* Werte in Schritt 7 nach wir haben unsere benutzerdefinierte Kartenanbieter erstellt.

Standort Zuordnungsklasse Anbieter instanziiert wird beim ersten über erfolgt die `SiteMap` Klasse und verbleibt im Arbeitsspeicher für die Lebensdauer der Webanwendung. Da es nur eine Instanz des Site-Zuordnung-Anbieters, die über mehrere, gleichzeitige Websitebesuchern aufgerufen werden kann ist, ist es obligatorisch, dass die s-Anbieter-Methoden werden *Thread-sichere*.

Leistung und Skalierbarkeit Gründe es wichtig, dass wir die Website im Arbeitsspeicher Zwischenspeichern ordnen Sie Struktur und zurückgeben, dieser Struktur, anstatt neu erstellt wird jedes Mal zwischengespeichert die `BuildSiteMap` Methode aufgerufen wird. `BuildSiteMap` kann mehrmals pro Anforderung pro Benutzer, je nachdem die Navigationssteuerelemente verwendet, auf der Seite und die Tiefe der Siteübersichtsstruktur Seite aufgerufen werden. In jedem Fall, wenn es nicht in der Siteübersichtsstruktur zwischenspeichern `BuildSiteMap` dann bei jedem Aufruf erfolgte wir müssen das Produkt und die Kategorie der Architektur erneut abrufen von Informationen vom (die in einer Abfrage mit der Datenbank führen würde). Wie in den vorherigen zwischenspeichernden Lernprogrammen erläutert, können die zwischengespeicherte Daten veralten. Um dies zu begegnen, können wir Zeit- oder SQL-Cache Abhängigkeit basierende Cacheobjekte.

> [!NOTE]
> Eine Website Kartenanbieter kann optional außer Kraft setzen die [ `Initialize` Methode](https://msdn.microsoft.com/library/system.web.sitemapprovider.initialize.aspx). `Initialize` wird aufgerufen, wenn der Standort Kartenanbieter zuerst instanziiert wird und alle benutzerdefinierten Attribute, die an den Anbieter in zugewiesen übergeben `Web.config` in der `<add>` Element wie: `<add name="name" type="type" customAttribute="value" />`. Es ist hilfreich, wenn Sie zulassen, den Entwickler einer Seite verschiedene Standort Zuordnung anbieterbezogene Einstellungen angeben, ohne die Anbieter-s-Code ändern zu müssen möchten. Wenn wir die Kategorie und Produkte Daten direkt aus der Datenbank statt über die Architektur, wir d wahrscheinlich lesen wurden z. B. können Sie die Datenbank-Verbindungszeichenfolge durch Angeben der Entwickler der Seite `Web.config` anstatt mit einem hartcodierten der Wert in den Anbietercode s. Überschreibt die benutzerdefinierte Website Kartenanbieter in Schritt 6 erstellen müssen nicht dies `Initialize` Methode. Ein Beispiel der Verwendung der `Initialize` -Methode finden Sie unter [Jeff Prosise](http://www.wintellect.com/Weblogs/CategoryView,category,Jeff%20Prosise.aspx) s [Sitemaps in SQL Server speichern](https://msdn.microsoft.com/msdnmag/issues/05/06/WickedCode/) Artikel.


## <a name="step-6-creating-the-custom-site-map-provider"></a>Schritt 6: Erstellen der benutzerdefinierten Website Kartenanbieter

Um eine benutzerdefinierte Website Kartenanbieter zu erstellen, in dem die Siteübersicht aus den Kategorien und Produkte in der Northwind-Datenbank erstellt, muss eine Klasse erstellen, die erweitert `StaticSiteMapProvider`. In Schritt 1 ich Sie gebeten, Hinzufügen einer `CustomProviders` Ordner in der `App_Code` Ordner - Hinzufügen einer neuen Klasse in diesem Ordner mit dem Namen `NorthwindSiteMapProvider`. Fügen Sie der `NorthwindSiteMapProvider` -Klasse folgenden Code hinzu:


[!code-vb[Main](building-a-custom-database-driven-site-map-provider-vb/samples/sample6.vb)]

Beginnen Sie mit dieser Klasse untersuchen s können `BuildSiteMap` -Methode, die mit beginnt ein [ `lock` Anweisung](https://msdn.microsoft.com/library/c5kehkcz.aspx). Die `lock` Anweisung lässt nur ein Thread zu einem Zeitpunkt eingeben, und dadurch Zugriff auf ihren Code zu serialisieren und verhindert, dass zwei gleichzeitige Threads ausführen in Einzelschritten, auf die anderen s damals.

Der Klassenebene `SiteMapNode` Variable `root` wird verwendet, um die Siteübersichtsstruktur zwischenzuspeichern. Wenn die Siteübersicht erstellt wird, zum ersten Mal oder zum ersten Mal nach der zugrunde liegenden Daten geändert wurden, `root` werden `Nothing` und die Siteübersichtsstruktur erstellt. Der Stammknoten der Site-Zuordnung s zugewiesen `root` während der Konstruktion Prozess so, dass das nächste Mal, diese Methode aufgerufen wird, `root` nicht `Nothing`. Daher so lange `root` nicht `Nothing` der Siteübersichtsstruktur wird an den Aufrufer zurückgegeben werden, ohne ihn neu erstellen.

Wenn der Stamm ist `Nothing`, die Siteübersichtsstruktur wird anhand des Produkts und Kategorie erstellt. Die Siteübersicht wird erstellt, durch das Erstellen der `SiteMapNode` Instanzen und dann bilden die Hierarchie über Aufrufe an die `StaticSiteMapProvider` Klasse s `AddNode` Methode. `AddNode` führt die internen vertiefen, speichern die zusammengestellte `SiteMapNode` Instanzen in einer `Hashtable`. Bevor wir beginnen, erstellen die Hierarchie, beginnen wir durch Aufrufen der `Clear` -Methode, die die Elemente aus dem internen löscht `Hashtable`. Als Nächstes wird die `ProductsBLL` Klasse s `GetProducts` -Methode und die daraus resultierende `ProductsDataTable` in lokalen Variablen gespeichert werden.

Die Site-Zuordnung s-Erstellung beginnt, durch den Stammknoten erstellen und zuweisen zu `root`. Die Überladung von der [ `SiteMapNode` s Konstruktor](https://msdn.microsoft.com/library/system.web.sitemapnode.sitemapnode.aspx) verwendet hier und in diesem `BuildSiteMap` wird übergeben, die folgende Informationen:

- Ein Verweis auf die Website Kartenanbieter (`Me`).
- Die `SiteMapNode` s `Key`. Dies erforderlich Wert muss für jeden eindeutigen `SiteMapNode`.
- Die `SiteMapNode` s `Url`. `Url` ist optional, aber wenn angegeben, von denen jede `SiteMapNode` s `Url` Wert muss eindeutig sein.
- Die `SiteMapNode` s `Title`, was erforderlich ist.

Die `AddNode(root)` Methodenaufruf fügt die `SiteMapNode` `root` der Sitemap als Stamm. Als Nächstes wird jede `ProductRow` in der `ProductsDataTable` aufgelistet. Wenn es bereits vorhanden ist eine `SiteMapNode` für die aktuelle s Produktkategorie, darauf verwiesen wird. Andernfalls ein neues `SiteMapNode` für die Kategorie erstellt und als untergeordnetes Element hinzugefügt wird die `SiteMapNode``root` über die `AddNode(categoryNode, root)` -Methodenaufruf. Nach der jeweiligen Kategorie `SiteMapNode` Knoten gefunden oder erstellt haben, wurde eine `SiteMapNode` für das aktuelle Produkt erstellt und hinzugefügt werden, als untergeordnetes Element der Kategorie `SiteMapNode` über `AddNode(productNode, categoryNode)`. Beachten Sie, dass die Kategorie `SiteMapNode` s `Url` Eigenschaftswert ist `~/SiteMapProvider/ProductsByCategory.aspx?CategoryID=categoryID` dagegen das Produkt `SiteMapNode` s `Url` -Eigenschaft zugewiesen `~/SiteMapNode/ProductDetails.aspx?ProductID=productID`.

> [!NOTE]
> Diese Produkte mit einer Datenbank `NULL` Wert für ihre `CategoryID` werden in einer Kategorie gruppiert `SiteMapNode` , deren `Title` Eigenschaftensatz keine und deren `Url` Eigenschaft auf eine leere Zeichenfolge festgelegt ist. Entschied ich mich, legen Sie `Url` auf eine leere Zeichenfolge seit der `ProductBLL` Klasse s `GetProductsByCategory(categoryID)` Methode fehlt derzeit die Möglichkeit, nur für diese Produkte mit einem `NULL` `CategoryID` Wert. Darüber hinaus ich zeigen möchten, wie die Navigationssteuerelemente gerendert eine `SiteMapNode` , die verfügt nicht über einen Wert für die `Url` Eigenschaft. Überzeugen Sie sich in diesem Lernprogramm erweitern, damit keine `SiteMapNode` s `Url` -Eigenschaft verweist auf `ProductsByCategory.aspx`, noch zeigt nur die Produkte mit `NULL` `CategoryID` Werte.


Nach dem Erstellen der Siteübersicht, ein beliebiges Objekt hinzugefügt wird, für den Datencache mithilfe einer SQL-Cacheabhängigkeit auf die `Categories` und `Products` Tabellen über eine `AggregateCacheDependency` Objekt. Wir verwenden von SQL-Cache-Abhängigkeiten in dem vorherigen Lernprogramm untersucht *mithilfe von SQL-Cache-Abhängigkeiten*. Die benutzerdefinierte Website Kartenanbieter verwendet jedoch eine Überladung der Datencache s `Insert` Methode, dass wir Ve noch zu untersuchen. Diese Überladung wird ein Delegat, der aufgerufen wird, wenn das Objekt aus dem Cache entfernt wird als der letzte Eingabeparameter akzeptiert. Insbesondere wird in einem neuen übergeben [ `CacheItemRemovedCallback` Delegieren](https://msdn.microsoft.com/library/system.web.caching.cacheitemremovedcallback.aspx) , verweist auf die `OnSiteMapChanged` Methode definiert weiteren unten in der `NorthwindSiteMapProvider` Klasse.

> [!NOTE]
> Die in-Memory-Darstellung der Siteübersicht wird über die Variable auf Klassenebene zwischengespeichert `root`. Da es nur eine Instanz der Klasse die Zuordnung, und ist da diese Instanz in der Webanwendung auf allen Threads freigegeben wird, dient diese Klassenvariable als Cache. Die `BuildSiteMap` Methode verwendet auch den Datencache, jedoch nur als Mittel zum Benachrichtigung erhalten, wenn die zugrunde liegende Datenbank Daten in die `Categories` oder `Products` Änderungen Tabellen. Beachten Sie, dass der Wert der Datencache abgelegt nur das aktuelle Datum und die Uhrzeit aus. Ist die tatsächliche Siteübersichtsdaten *nicht* im Zwischenspeicher abgelegt.


Die `BuildSiteMap` Methode abgeschlossen wird, indem Sie den Stammknoten der Siteübersicht zurückgeben.

Die restlichen Methoden sind recht einfach. `GetRootNodeCore` ist verantwortlich für die Rückgabe des Stammknotens. Da `BuildSiteMap` gibt das Stammelement, `GetRootNodeCore` gibt einfach auftragsantwortnachrichten zurück `BuildSiteMap` s-Rückgabewert. Die `OnSiteMapChanged` -Methode legt `root` an `Nothing` Wenn das Element im Cache entfernt wird. Mit einem angegebenen Stamm legen wieder auf `Nothing`, das nächste Mal `BuildSiteMap` wird aufgerufen, die Siteübersichtsstruktur wird neu erstellt. Abschließend wird die `CachedDate` Eigenschaft gibt den gespeicherten im Datencache, Datum und Uhrzeit-Wert aus, wenn ein solcher Wert vorhanden ist. Diese Eigenschaft kann von einem Seitenentwickler verwendet werden, um zu bestimmen, wann die Siteübersichtsdaten zuletzt zwischengespeichert wurde.

## <a name="step-7-registering-thenorthwindsitemapprovider"></a>Schritt 7: Registrieren der`NorthwindSiteMapProvider`

In der Reihenfolge für unsere Webanwendung für die Verwendung der `NorthwindSiteMapProvider` Kartenanbieter der Site in Schritt 6 erstellt haben, müssen wir registrieren Sie ihn in die `<siteMap>` Teil `Web.config`. Insbesondere, fügen Sie das folgende Markup innerhalb der `<system.web>` Element im `Web.config`:


[!code-xml[Main](building-a-custom-database-driven-site-map-provider-vb/samples/sample7.xml)]

Dieses Markup führt zwei Aufgaben aus: zuerst, bedeutet dies, dass die integrierte `AspNetXmlSiteMapProvider` ist der Standardanbieter der Siteübersicht; Zweitens, registriert die benutzerdefinierte Website Kartenanbieter in Schritt 6 mit einem von Menschen benutzerfreundliche Namen Northwind erstellt.

> [!NOTE]
> Für Anbietern der Siteübersicht befindet sich in der Anwendung s `App_Code` Ordner, der Wert der `type` -Attribut ist einfach der Name der Klasse. Alternativ können Sie die benutzerdefinierte Website Kartenanbieter hätte erstellt in einem separaten Klassenbibliotheksprojekt mit die kompilierte Assembly platziert, in der Web-Anwendung s `/Bin` Verzeichnis. In diesem Fall die `type` Attributwert wäre *Namespace*. *Klassenname*, *AssemblyName* .


Nach der Aktualisierung `Web.config`, nehmen einen Moment Zeit, um eine beliebige Seite in den Lernprogrammen in einem Browser anzuzeigen. Beachten Sie, dass die Navigationsschnittstelle auf der linken Seite weiterhin in den Abschnitten zeigt und Lernprogramme, die in definierten `Web.sitemap`. Dies ist darauf zurückzuführen wir `AspNetXmlSiteMapProvider` als Standardanbieter. Um ein Benutzeroberflächenelement für die Navigation zu erstellen, verwendet die `NorthwindSiteMapProvider`, benötigen wir explizit angeben, dass der Northwind-Site-Zuordnung-Anbieter verwendet werden soll. Wir sehen, wie dies in Schritt 8 zu erreichen.

## <a name="step-8-displaying-site-map-information-using-the-custom-site-map-provider"></a>Schritt 8: Anzeigen von Standortinformationen Zuordnung mithilfe der benutzerdefinierten Website Kartenanbieter

Mit der benutzerdefinierten Website Kartenanbieter erstellt und registriert `Web.config`, wir re kann jetzt Steuerelemente zur Navigation zum Hinzufügen der `Default.aspx`, `ProductsByCategory.aspx`, und `ProductDetails.aspx` Seiten in der `SiteMapProvider` Ordner. Öffnen Sie zunächst die `Default.aspx` Seite, und ziehen Sie eine `SiteMapPath` aus der Toolbox in den Designer. Das Steuerelement SiteMapPath befindet sich im Abschnitt "Navigation" der Toolbox.


[![Hinzufügen einer SiteMapPath "default.aspx"](building-a-custom-database-driven-site-map-provider-vb/_static/image19.gif)](building-a-custom-database-driven-site-map-provider-vb/_static/image18.gif)

**Abbildung 16**: Hinzufügen einer SiteMapPath auf `Default.aspx` ([klicken Sie hier, um das Bild in voller Größe angezeigt](building-a-custom-database-driven-site-map-provider-vb/_static/image20.gif))


Das SiteMapPath-Steuerelement zeigt Breadcrumb, der angibt, der aktuelle Seite s Position innerhalb der Siteübersicht. Wir eine SiteMapPath hinzugefügt, an den Anfang der Masterseite wieder in die *Masterseiten und Websitenavigation* Lernprogramm.

Nehmen Sie einen Moment Zeit, um diese Seite über einen Browser anzuzeigen. In Abbildung 16 hinzugefügt SiteMapPath verwendet den Standardanbieter der Siteübersicht, ziehen die zugehörigen Daten aus `Web.sitemap`. Aus diesem Grund zeigt die Breadcrumb-Leiste Startseite &gt; Anpassen der Siteübersicht, genau wie die Breadcrumb-Leiste in der oberen rechten Ecke.


[![Die Breadcrumb-Leiste verwendet den Standardanbieter der Siteübersicht](building-a-custom-database-driven-site-map-provider-vb/_static/image22.gif)](building-a-custom-database-driven-site-map-provider-vb/_static/image21.gif)

**Abbildung 17**: der Brotkrümelnavigation verwendet die standardmäßige Website Kartenanbieter ([klicken Sie hier, um das Bild in voller Größe angezeigt](building-a-custom-database-driven-site-map-provider-vb/_static/image23.gif))


Damit die SiteMapPath in Abbildung 16 hinzugefügt, die die benutzerdefinierte Website Kartenanbieter verwenden wir das in Schritt 6 erstellt haben, legen Sie dessen [ `SiteMapProvider` Eigenschaft](https://msdn.microsoft.com/library/system.web.ui.webcontrols.sitemappath.sitemapprovider.aspx) Northwind, den Namen wir zugewiesen, um die `NorthwindSiteMapProvider` in `Web.config`. Leider Designer weiterhin den Standardanbieter für Standort Zuordnung verwenden, aber wenn Sie die Seite über einen Browser besuchen, nach dieser Eigenschaft Änderung werden Sie sehen, dass die Breadcrumb-Leiste jetzt die benutzerdefinierte Website Kartenanbieter verwendet.


[![Die Breadcrumb-Leiste verwendet jetzt die benutzerdefinierte Zuordnung Anbieter NorthwindSiteMapProvider](building-a-custom-database-driven-site-map-provider-vb/_static/image25.gif)](building-a-custom-database-driven-site-map-provider-vb/_static/image24.gif)

**Abbildung 18**: der Brotkrümelnavigation verwendet jetzt die benutzerdefinierte Website Kartenanbieter `NorthwindSiteMapProvider` ([klicken Sie hier, um das Bild in voller Größe angezeigt](building-a-custom-database-driven-site-map-provider-vb/_static/image26.gif))


Das SiteMapPath-Steuerelement zeigt eine komplexere Benutzeroberfläche in der `ProductsByCategory.aspx` und `ProductDetails.aspx` Seiten. Hinzufügen einer SiteMapPath auf diese Seiten, Festlegen der `SiteMapProvider` Eigenschaft sowohl an Northwind. Von `Default.aspx` klicken Sie auf den Link Produkte anzeigen, für Getränke und dann auf den Link "Details anzeigen" für Chai Tee. Wie in Abbildung 19 gezeigt, enthält die Breadcrumb-Leiste den aktuellen Standort Zuordnung Abschnitt (Chai Tee) und dessen Vorgänger: Getränke und aller Kategorien.


[![Die Breadcrumb-Leiste verwendet jetzt die benutzerdefinierte Zuordnung Anbieter NorthwindSiteMapProvider](building-a-custom-database-driven-site-map-provider-vb/_static/image27.gif)](building-a-custom-database-driven-site-map-provider-vb/_static/image21.png)

**Abbildung 19**: der Brotkrümelnavigation verwendet jetzt die benutzerdefinierte Website Kartenanbieter `NorthwindSiteMapProvider` ([klicken Sie hier, um das Bild in voller Größe angezeigt](building-a-custom-database-driven-site-map-provider-vb/_static/image22.png))


Andere Elemente einer Benutzeroberfläche Navigation können zusätzlich zu den SiteMapPath, z. B. die Menü- und TreeView-Steuerelemente verwendet werden. Die `Default.aspx`, `ProductsByCategory.aspx`, und `ProductDetails.aspx` die Seiten im Download für dieses Lernprogramm enthalten alle Menüsteuerelemente (siehe Abbildung 20). Finden Sie unter [Untersuchen von ASP.NET 2.0 s Navigationsfunktionen Standort](http://aspnet.4guysfromrolla.com/articles/111605-1.aspx) und die [Standort Navigationssteuerelemente mithilfe](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/navigation/sitenavcontrols.aspx) Teil der [ASP.NET 2.0-Schnellstarts](https://quickstarts.asp.net/QuickStartv20/aspnet/) für eine eingehendere Betrachtung der Steuerelemente für die Seitennavigation und Standortsystem über die Zuordnung in ASP.NET 2.0.


[![Das Menüsteuerelement enthält alle Kategorien und Produkte](building-a-custom-database-driven-site-map-provider-vb/_static/image29.gif)](building-a-custom-database-driven-site-map-provider-vb/_static/image28.gif)

**Abbildung 20**: die im Menü Steuerelement führt jede der Kategorien und die Produkte ([klicken Sie hier, um das Bild in voller Größe angezeigt](building-a-custom-database-driven-site-map-provider-vb/_static/image30.gif))


Wie weiter oben in diesem Lernprogramm erwähnt, die Siteübersichtsstruktur möglich, programmgesteuert über die `SiteMap` Klasse. Der folgende Code gibt den Stamm `SiteMapNode` des Standardanbieters:


[!code-vb[Main](building-a-custom-database-driven-site-map-provider-vb/samples/sample8.vb)]

Da die `AspNetXmlSiteMapProvider` ist der Standardanbieter für unsere Anwendung würde der obige Code zurückgeben, den Stammknoten in definierten `Web.sitemap`. Um einem Standort Kartenanbieter außer dem Standardnamen zu verweisen, verwenden die `SiteMap` Klasse s [ `Providers` Eigenschaft](https://msdn.microsoft.com/library/system.web.sitemap.providers.aspx) wie folgt:


[!code-vb[Main](building-a-custom-database-driven-site-map-provider-vb/samples/sample9.vb)]

Wobei *Namen* ist der Name des benutzerdefinierten Site Map-Anbieters (Northwind, für die Webanwendung).

Verwenden Sie für den Zugriff auf einen Member, die spezifisch für eine Website Kartenanbieter `SiteMap.Providers["name"]` die Anbieterinstanz abzurufen und es dann in den entsprechenden Typ umwandeln. Z. B. zum Anzeigen der `NorthwindSiteMapProvider` s `CachedDate` Eigenschaft auf einer ASP.NET-Seite verwenden Sie folgenden Code:


[!code-vb[Main](building-a-custom-database-driven-site-map-provider-vb/samples/sample10.vb)]

> [!NOTE]
> Achten Sie darauf, dass die SQL-Cache-Abhängigkeit-Funktion zu testen. Nach dem Aufrufen der `Default.aspx`, `ProductsByCategory.aspx`, und `ProductDetails.aspx` Seiten, wechseln Sie zu den Tutorials in bearbeiten, einfügen und Löschen von Abschnitt und bearbeiten Sie den Namen einer Kategorie oder Produkt. Fahren Sie dann mit einer der Seiten in der `SiteMapProvider` Ordner. Vorausgesetzt, die Abrufmechanismus Beachten Sie die Änderung der zugrunde liegenden Datenbank ausreichend Zeit verstrichen ist, sollte die Siteübersicht aktualisiert werden, um das neue Produkt oder den Kategorienamen anzuzeigen.


## <a name="summary"></a>Zusammenfassung

ASP.NET 2.0 s Zuordnung Websitefunktionen umfasst eine `SiteMap` -Klasse, eine Anzahl von integrierten Navigation Web steuert und ein Standard-Site-Zuordnung-Anbieter, der erwartet, die Standortinformationen für die Zuordnung dass in eine XML-Datei beibehalten. Um Standortinformationen für die Zuordnung von einer anderen Quelle wie z. B. aus einer Datenbank zu verwenden, die Anwendungsarchitektur s oder einem Remotewebdienst zum Erstellen einer benutzerdefinierten Website müssen Anbieter zuordnen. Dies schließt die Erstellung einer Klasse, die direkt oder indirekt, abgeleitet aus dem `SiteMapProvider` Klasse.

In diesem Lernprogramm wurde erläutert, wie eine benutzerdefinierte Website Kartenanbieter erstellen, auf denen das Produkt und die Kategorie Informationen aus der Anwendungsarchitektur grafikgeräts Siteübersicht basiert. Unsere Anbieter erweitert die `StaticSiteMapProvider` Klassen- und resultierendem erstellen eine `BuildSiteMap` Methode, die die Daten abgerufen erstellt die Standorthierarchie für die Karte und die entstehende Struktur in einer Variablen auf Klassenebene zwischengespeichert. Wird eine SQL-Cacheabhängigkeit mit eine Rückruffunktion um für ungültig zu erklärende die zwischengespeicherten strukturieren, wenn die zugrunde liegende `Categories` oder `Products` Daten geändert.

Viel Spaß beim Programmieren!

## <a name="further-reading"></a>Weiterführende Themen

Weitere Informationen zu den Themen in diesem Lernprogramm erläutert finden Sie in den folgenden Ressourcen:

- [Speichern von Sitemaps in SQL Server](https://msdn.microsoft.com/msdnmag/issues/05/06/WickedCode/) und [der SQL-Standort-Kartenanbieter Sie Ve gewartet wurde](https://msdn.microsoft.com/msdnmag/issues/06/02/wickedcode/default.aspx)
- [Ein Blick auf ASP.NET 2.0 s-Anbieter-Modell](http://aspnet.4guysfromrolla.com/articles/101905-1.aspx)
- [Die Anbietertoolkit](https://msdn.microsoft.com/asp.net/aa336558.aspx)
- [Untersuchen ASP.NET 2.0 Navigationsfunktionen für s-Standorts](http://aspnet.4guysfromrolla.com/articles/111605-1.aspx)

## <a name="about-the-author"></a>Informationen zum Autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor von sieben ASP/ASP.NET-Büchern und Gründer von [4GuysFromRolla.com](http://www.4guysfromrolla.com), Microsoft Web-Technologien seit 1998 arbeitet. Scott fungiert als ein unabhängiger Berater, Trainer und Writer. Sein neueste Buch wird [ *Sams Schulen selbst ASP.NET 2.0 in 24 Stunden*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Er die erreicht werden kann, zur [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) oder über seinen Blog die finden Sie unter [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Besonderen Dank an

Diese Reihe von Lernprogrammen wurde durch viele nützliche Bearbeiter überprüft. Lead Prüfer für dieses Lernprogramm wurden Dave Gardner, Zack Jones Teresa Murphy und Bernadette Leigh. Meine bevorstehende MSDN-Artikel Überprüfen von Interesse? Wenn dies der Fall ist, löschen Sie mich zeilenweise [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Vorherige](building-a-custom-database-driven-site-map-provider-cs.md)
