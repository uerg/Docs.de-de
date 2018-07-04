---
uid: web-forms/overview/data-access/database-driven-site-maps/building-a-custom-database-driven-site-map-provider-vb
title: Erstellen einer benutzerdefinierten datenbankgesteuerte Siteübersichtsanbieters (VB) | Microsoft-Dokumentation
author: rick-anderson
description: Die Standardsiteübersichtsanbieter in ASP.NET 2.0 ruft Daten aus einer statischen XML-Datei ab. Während der XML-basierten Anbieter geeignet ist, die viele kleine und mittlere-ößen-ist...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/26/2007
ms.topic: article
ms.assetid: f904cd2c-a408-4484-9324-8b8d7fe33893
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/data-access/database-driven-site-maps/building-a-custom-database-driven-site-map-provider-vb
msc.type: authoredcontent
ms.openlocfilehash: 8e041a5a9163c7f9fe55c6aa06f35301cbdb48a8
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/03/2018
ms.locfileid: "37393969"
---
<a name="building-a-custom-database-driven-site-map-provider-vb"></a>Erstellen einer benutzerdefinierten datenbankgesteuerte Siteübersichtsanbieters (VB)
====================
durch [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Code herunterladen](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_62_VB.zip) oder [PDF-Datei herunterladen](building-a-custom-database-driven-site-map-provider-vb/_static/datatutorial62vb1.pdf)

> Die Standardsiteübersichtsanbieter in ASP.NET 2.0 ruft Daten aus einer statischen XML-Datei ab. Während der XML-basierte-Anbieter für viele kleine und mittelgroße Websites geeignet ist, erfordern größere Webanwendungen eine dynamischere Siteübersicht. In diesem Tutorial, die hier einen benutzerdefinierter Sitezuordnungsprovider erstellt wird, der von der Geschäftslogikebene seine Daten abruft, die wiederum Daten aus der Datenbank abgerufen.


## <a name="introduction"></a>Einführung

ASP.NET 2.0 Siteübersichtsfeature s kann ein Entwickler eine Seite auf eine Anwendung s Websiteübersicht in einige persistenten Medium, z. B. in eine XML-Datei zu definieren. Nachdem definiert, die Siteübersichtsdaten erfolgen können programmgesteuert über die [ `SiteMap` Klasse](https://msdn.microsoft.com/library/system.web.sitemap.aspx) in die [ `System.Web` Namespace](https://msdn.microsoft.com/library/system.web.aspx) oder über eine Vielzahl von Navigation Web-Steuerelemente, z. B. die SiteMapPath, Menu und TreeView-Steuerelemente. Das Site Map-System verwendet die [Anbietermodell](http://aspnet.4guysfromrolla.com/articles/101905-1.aspx) , damit andere Site Map Serialisierung Implementierungen erstellt und eine Webanwendung angeschlossen werden können. Die standardmäßigen SiteMapProvider, die mit ASP.NET 2.0 geliefert wird weiterhin Siteübersichtsstruktur in eine XML-Datei besteht. In der [Masterseiten und Sitenavigation](../introduction/master-pages-and-site-navigation-vb.md) Tutorial, die wir erstellt, eine Datei namens haben `Web.sitemap` , die dieser Struktur enthalten, und haben, wurde der XML-mit jedem neuen Abschnitt des Tutorials aktualisieren.

Die XML-basierte Standardsiteübersichtsanbieter funktioniert gut, wenn die s Siteübersichtsstruktur für diese Tutorials relativ statisch sein, z. B. ist. In vielen Szenarien ist jedoch eine dynamischere Sitemap erforderlich. Erwägen Sie die Sitemap, dargestellt in Abbildung 1, in dem jede Kategorie und jedes Produkt als Abschnitte in der Website-s-Struktur angezeigt werden. Mit diesem Siteübersicht möglicherweise Besuch einer Webseite für den Stammknoten alle Kategorien, Liste während der Besuch einer bestimmten Kategorie s-Webseite, Kategorie-s-Produkte aufgelistet, und ein bestimmtes Produkt s-Webseite anzeigen würden dieses Produkt s Details.


[![Die Kategorien und Produkte Zusammensetzung der Siteübersichtsstruktur s](building-a-custom-database-driven-site-map-provider-vb/_static/image1.gif)](building-a-custom-database-driven-site-map-provider-vb/_static/image1.png)

**Abbildung 1**: die Kategorien und Produkte Zusammensetzung der Sitemap-s-Struktur ([klicken Sie, um das Bild in voller Größe anzeigen](building-a-custom-database-driven-site-map-provider-vb/_static/image2.png))


Diese Kategorie und Produkt-basierte Struktur hartcodiert sein könnte die `Web.sitemap` -Datei, die Datei jedes Mal eine Kategorie aktualisiert werden müssen oder Produkt hinzugefügt, entfernt oder umbenannt wurde. Daher würde der Karte Standortwartungstask erheblich vereinfacht werden, wenn die Struktur aus der Datenbank oder am besten, von der Geschäftslogikebene der Anwendungsarchitektur s abgerufen wurde. Auf diese Weise, wie Produkte und Kategorien hinzugefügt wurden, umbenannt oder gelöscht, die Sitemap wird automatisch aktualisiert, um diese Änderungen widerzuspiegeln.

Seit ASP.NET 2.0 s Site Map Serialisierung auf dem Anbietermodell erstellt wurde, können wir unseren eigenen benutzerdefinierter Sitezuordnungsprovider erstellen, der erfasst die Daten aus der einen alternativen Datenspeicher, z. B. die Datenbank oder der Architektur. In diesem Tutorial erstellen wir einen benutzerdefinierten Anbieter, der die Daten aus der BLL abruft. Lassen Sie s beginnen!

> [!NOTE]
> In diesem Tutorial erstellten benutzerdefinierten Siteübersichtsanbieter ist auf die-s-Architektur und Anwendungsmodell eng gekoppelt. Jeff Prosise s [Sitemaps in SQL Server speichern](https://msdn.microsoft.com/msdnmag/issues/05/06/WickedCode/) und [der SQL-SiteMapProvider Sie Ve gewartet](https://msdn.microsoft.com/msdnmag/issues/06/02/wickedcode/default.aspx) Artikeln untersuchen einen generalisierten Ansatz zum Speichern von Sitemap-Daten in SQL Server.


## <a name="step-1-creating-the-custom-site-map-provider-web-pages"></a>Schritt 1: Erstellen, die benutzerdefinierte Website Map Provider-Webseiten

Bevor wir beginnen, einen benutzerdefinierten Sitezuordnungsprovider erstellen, können Sie s zunächst die ASP.NET-Seiten hinzufügen, die wir für dieses Tutorial benötigen. Starten, indem Sie einen neuen Ordner namens hinzufügen `SiteMapProvider`. Fügen Sie die folgenden ASP.NET-Seiten in diesen Ordner, um sicherzustellen, ordnen Sie jeder Seite mit den `Site.master` Masterseite:

- `Default.aspx`
- `ProductsByCategory.aspx`
- `ProductDetails.aspx`

Hinzufügen einer `CustomProviders` Unterordner der `App_Code` Ordner.


![Fügen Sie die ASP.NET-Seiten für die Site Map Anbieterbezogene-Lernprogramme](building-a-custom-database-driven-site-map-provider-vb/_static/image2.gif)

**Abbildung 2**: hinzufügen, die ASP.NET-Seiten für den Standort zuordnen Anbieterbezogene Tutorials


Da es nur ein Lernprogramm für diesen Abschnitt ist, wir Raten t müssen `Default.aspx` in den Tutorials Abschnitt s aufgelistet. Stattdessen `Default.aspx` zeigt die Kategorien in einem GridView-Steuerelement. Wir werden dies in Schritt2 in Angriff nehmen.

Aktualisieren Sie als Nächstes `Web.sitemap` sollen einen Verweis auf die `Default.aspx` Seite. Fügen Sie das folgende Markup insbesondere nach der Zwischenspeicherung `<siteMapNode>`:


[!code-xml[Main](building-a-custom-database-driven-site-map-provider-vb/samples/sample1.xml)]

Nach der Aktualisierung `Web.sitemap`, können Sie die Lernprogramme-Website über einen Browser anzeigen. Klicken Sie im Menü auf der linken Seite enthält jetzt ein Element für das einzige Standort Map Provider-Lernprogramm.


![Die Sitemap enthält nun einen Eintrag für das Site Map Provider-Tutorial](building-a-custom-database-driven-site-map-provider-vb/_static/image3.gif)

**Abbildung 3**: die Sitemap enthält nun einen Eintrag für das Site Map Provider-Tutorial


Dieses Tutorial s Hauptziel ist zur Veranschaulichung erstellen einen benutzerdefinierten Sitezuordnungsprovider und Konfigurieren von Web-Apps verwenden diesen Anbieter. Insbesondere erstellen wir einen Anbieter an, der eine Sitemap, die einen Stammknoten zusammen mit der ein Knoten für jede Kategorie und das Produkt enthält zurückgibt, wie in Abbildung 1 dargestellt. Jeder Knoten der sitezuordnung kann im Allgemeinen eine URL angeben. Für unsere Websiteübersicht, werden die Stamm-URL des Knoten s `~/SiteMapProvider/Default.aspx`, die Listet alle Kategorien in der Datenbank. Jede Kategorieknoten der sitezuordnung hat eine URL, die auf zeigt `~/SiteMapProvider/ProductsByCategory.aspx?CategoryID=categoryID`, die Listet alle Produkte in der angegebenen *CategoryID*. Schließlich zeigt jedes Produkt Siteübersichtsknoten auf `~/SiteMapProvider/ProductDetails.aspx?ProductID=productID`, wird die Details zu bestimmten Produkt s angezeigt.

Starten wir müssen zum Erstellen der `Default.aspx`, `ProductsByCategory.aspx`, und `ProductDetails.aspx` Seiten. Diese Seiten werden in den Schritten 2, 3 und 4 ausgeführt. Da der Großteil der in diesem Tutorial auf Sitezuordnungsprovider ist und seit der letzten Tutorials erstellen behandelt diese Arten von mehreren Seiten von Master-/Detailberichten meldet, wir über die Schritte 2 bis 4 beschleunigen wird. Wenn Sie eine Auffrischung zum Erstellen von Master-/Detail-Berichten, die mehrere Seiten erstrecken benötigen, verweisen zurück auf die [Anzeigen von Filtern von Master-/Detailberichten über zwei Seiten](../masterdetail/master-detail-filtering-across-two-pages-vb.md) Tutorial.

## <a name="step-2-displaying-a-list-of-categories"></a>Schritt 2: Anzeigen einer Liste von Kategorien

Öffnen der `Default.aspx` auf der Seite die `SiteMapProvider` Ordner, und ziehen Sie einer GridView-Ansicht aus der Toolbox auf den Designer, Festlegen der `ID` zu `Categories`. Von GridView s Smarttags, binden Sie es an eine neue, mit dem Namen "ObjectDataSource" `CategoriesDataSource` er so konfiguriert, dass, die Daten mithilfe abgerufen der `CategoriesBLL` Klasse s `GetCategories` Methode. Da diese GridView die Kategorien zeigt einfach und keine Möglichkeiten zur Datenänderung bietet, legen Sie die Dropdownlisten in der Update-, INSERT-, und Löschen von Registerkarten (keine) aus.


[![Konfigurieren Sie das "ObjectDataSource" zum Zurückgeben von Kategorien, die mit der GetCategories-Methode](building-a-custom-database-driven-site-map-provider-vb/_static/image4.gif)](building-a-custom-database-driven-site-map-provider-vb/_static/image3.png)

**Abbildung 4**: Konfigurieren von dem ObjectDataSource-Steuerelement zur Verwendung von Kategorien zurück der `GetCategories` Methode ([klicken Sie, um das Bild in voller Größe anzeigen](building-a-custom-database-driven-site-map-provider-vb/_static/image4.png))


[![Legen Sie die Dropdownlisten in der Update-, INSERT- und DELETE werden Registerkarten (keine)](building-a-custom-database-driven-site-map-provider-vb/_static/image5.gif)](building-a-custom-database-driven-site-map-provider-vb/_static/image5.png)

**Abbildung 5**: Legen Sie die Dropdownlisten in der Update-, INSERT- und Löschen von Registerkarten auf (keine) ([klicken Sie, um das Bild in voller Größe anzeigen](building-a-custom-database-driven-site-map-provider-vb/_static/image6.png))


Nach Abschluss der Konfigurieren von Datenquellen-Assistenten fügt Visual Studio eine BoundField für `CategoryID`, `CategoryName`, `Description`, `NumberOfProducts`, und `BrochurePath`. GridView bearbeiten, sodass sie nur enthält die `CategoryName` und `Description` BoundFields und aktualisieren Sie die `CategoryName` BoundField-s `HeaderText` Eigenschaft zur Kategorie.

Als Nächstes fügen Sie eine HyperLinkField hinzu und positionieren Sie es also, dass die It s Feld ganz links. Legen Sie die `DataNavigateUrlFields` Eigenschaft `CategoryID` und die `DataNavigateUrlFormatString` Eigenschaft `~/SiteMapProvider/ProductsByCategory.aspx?CategoryID={0}`. Legen Sie die `Text` Eigenschaft, um Produkte anzeigen.


![Eine HyperLinkField an die GridView Kategorien hinzufügen](building-a-custom-database-driven-site-map-provider-vb/_static/image6.gif)

**Abbildung 6**: Hinzufügen einer HyperLinkField auf die `Categories` GridView


Nach dem ObjectDataSource-Steuerelement erstellen und Anpassen der GridView-s-Felder, werden zwei Steuerelemente deklarativen Markup wie folgt aussehen:


[!code-aspx[Main](building-a-custom-database-driven-site-map-provider-vb/samples/sample2.aspx)]

Abbildung 7 zeigt `Default.aspx` über einen Browser angezeigt. Eine Kategorie s Produkte anzeigen Link gelangen Sie auf `ProductsByCategory.aspx?CategoryID=categoryID`, die wir in Schritt 3 erstellt wird.


[![Jede Kategorie ist aufgeführten zusammen mit einem Link des Ansicht-Produkte](building-a-custom-database-driven-site-map-provider-vb/_static/image7.gif)](building-a-custom-database-driven-site-map-provider-vb/_static/image7.png)

**Abbildung 7**: jede Kategorie aufgeführten zusammen mit einem Link des Ansicht-Produkte ist ([klicken Sie, um das Bild in voller Größe anzeigen](building-a-custom-database-driven-site-map-provider-vb/_static/image8.png))


## <a name="step-3-listing-the-selected-category-s-products"></a>Schritt 3: Auflisten von der ausgewählten Kategorie-s-Produkte

Öffnen der `ProductsByCategory.aspx` Seite und Hinzufügen einer GridView-Ansicht, nennen Sie es `ProductsByCategory`. Aus sein Smarttag, GridView zu binden, eine neue, mit dem Namen "ObjectDataSource" `ProductsByCategoryDataSource`. Konfigurieren Sie mit dem ObjectDataSource-Steuerelement die `ProductsBLL` Klasse s `GetProductsByCategoryID(categoryID)` Methode, und legen die Dropdownlisten auf (keine) auf den Registerkarten Update-, INSERT- und DELETE.


[![Verwenden Sie die ProductsBLL Klasse s GetProductsByCategoryID(categoryID)-Methode](building-a-custom-database-driven-site-map-provider-vb/_static/image8.gif)](building-a-custom-database-driven-site-map-provider-vb/_static/image9.png)

**Abbildung 8**: Verwenden der `ProductsBLL` Klasse s `GetProductsByCategoryID(categoryID)` Methode ([klicken Sie, um das Bild in voller Größe anzeigen](building-a-custom-database-driven-site-map-provider-vb/_static/image10.png))


Der letzte Schritt im Konfigurieren von Datenquellen-Assistenten aufgefordert, einen Parameterquelle für *CategoryID*. Da diese Informationen über das Feld "Querystring" übergeben wird `CategoryID`, wählen Sie die Abfragezeichenfolge aus der Dropdown-Liste, und geben Sie CategoryID im Textfeld für die QueryStringField Sie, wie in Abbildung 9 dargestellt. Klicken Sie auf "Fertig stellen", um den Assistenten abzuschließen.


[![Verwenden Sie die CategoryID Querystring-Feld für die CategoryID-Parameter](building-a-custom-database-driven-site-map-provider-vb/_static/image9.gif)](building-a-custom-database-driven-site-map-provider-vb/_static/image11.png)

**Abbildung 9**: Verwenden der `CategoryID` Querystring Field für die *CategoryID* Parameter ([klicken Sie, um das Bild in voller Größe anzeigen](building-a-custom-database-driven-site-map-provider-vb/_static/image12.png))


Nach Abschluss des Assistenten wird Visual Studio entsprechende BoundFields und eine CheckBoxField an die GridView für die Product-Datenfelder hinzufügen. Entfernen Sie alle außer den `ProductName`, `UnitPrice`, und `SupplierName` BoundFields. Passen Sie diese drei BoundFields `HeaderText` zu Produkt, Preis und Lieferanten jeweils zu lesenden Eigenschaften. Format der `UnitPrice` BoundField als Währung.

Als Nächstes fügen Sie eine HyperLinkField aus, und verschieben Sie sie in der äußersten linken Position. Legen Sie seine `Text` Eigenschaft, um Details anzeigen, die `DataNavigateUrlFields` Eigenschaft, um `ProductID`, und die zugehörige `DataNavigateUrlFormatString` Eigenschaft, um `~/SiteMapProvider/ProductDetails.aspx?ProductID={0}`.


![Hinzufügen einer Ansicht Details HyperLinkField, die auf ProductDetails.aspx verweist](building-a-custom-database-driven-site-map-provider-vb/_static/image10.gif)

**Abbildung 10**: Hinzufügen einer Detailansicht HyperLinkField, die auf verweist `ProductDetails.aspx`


Nach diesen Anpassungen sollte GridView und "ObjectDataSource" s deklarative Markup wie folgt aussehen:


[!code-aspx[Main](building-a-custom-database-driven-site-map-provider-vb/samples/sample3.aspx)]

Kehren Sie zum Anzeigen von zurück `Default.aspx` über einen Browser, und klicken Sie auf die Ansicht Produkte für Getränke zu verknüpfen. Dadurch gelangen Sie zu `ProductsByCategory.aspx?CategoryID=1`, Anzeigen von Namen, Preise und Lieferanten der Produkte in der Northwind-Datenbank, die die Kategorie "Getränke" angehören (siehe Abbildung 11). Auf dieser Seite, um einen Link, um Benutzer auf die Kategorie-Angebotsseite zurückzukehren einzuschließen weiter verbessern können (`Default.aspx`) und ein DetailsView oder FormView-Steuerelement, in der ausgewählten Kategorie s-Name und Beschreibung angezeigt.


[![Die Namen von Getränken, Preise und Lieferanten werden angezeigt.](building-a-custom-database-driven-site-map-provider-vb/_static/image11.gif)](building-a-custom-database-driven-site-map-provider-vb/_static/image13.png)

**Abbildung 11**: die Namen der Getränke, Preise und Lieferanten werden angezeigt ([klicken Sie, um das Bild in voller Größe anzeigen](building-a-custom-database-driven-site-map-provider-vb/_static/image14.png))


## <a name="step-4-showing-a-product-s-details"></a>Schritt 4: Zeigt eine s-Produktinformationen

Der letzten Seite `ProductDetails.aspx`, zeigt die Details des ausgewählten Produkte. Open `ProductDetails.aspx` und eine DetailsView aus der Toolbox in den Designer ziehen. Legen Sie die s DetailsView `ID` Eigenschaft `ProductInfo` und lösche die `Height` und `Width` Eigenschaftswerte. Binden Sie aus der Smarttag, DetailsView an eine neue, mit dem Namen "ObjectDataSource" `ProductDataSource`, konfigurieren das "ObjectDataSource", um die Daten aus der pull die `ProductsBLL` s-Klasse `GetProductByProductID(productID)` Methode. Wie bei den vorherigen Web-Seiten in den Schritten 2 und 3 erstellt haben, legen Sie die Dropdownlisten in der Update-, INSERT-, und löschen Sie die Registerkarten (keine) aus.


[![Konfigurieren von dem ObjectDataSource-Steuerelement zur Verwendung der GetProductByProductID(productID)-Methode](building-a-custom-database-driven-site-map-provider-vb/_static/image12.gif)](building-a-custom-database-driven-site-map-provider-vb/_static/image15.png)

**Abbildung 12**: Konfigurieren von dem ObjectDataSource-Steuerelement verwenden die `GetProductByProductID(productID)` Methode ([klicken Sie, um das Bild in voller Größe anzeigen](building-a-custom-database-driven-site-map-provider-vb/_static/image16.png))


Der letzte Schritt des Assistenten zum Konfigurieren von Datenquellen für die Quelle des fordert die *"ProductID"* Parameter. Da diese Daten über das Feld "Querystring" bestehen `ProductID`, legen Sie die Dropdown-Liste auf QueryString und der QueryStringField-Textbox, "ProductID". Klicken Sie abschließend auf die Schaltfläche Fertig stellen, um den Assistenten abzuschließen.


[![Konfigurieren Sie die Produkt-ID-Parameter, um den Wert aus dem Querystring-Feld "ProductID" Abrufen](building-a-custom-database-driven-site-map-provider-vb/_static/image13.gif)](building-a-custom-database-driven-site-map-provider-vb/_static/image17.png)

**Abbildung 13**: Konfigurieren der *"ProductID"* Parameter, um den Wert von Pull die `ProductID` Querystring-Field ([klicken Sie, um das Bild in voller Größe anzeigen](building-a-custom-database-driven-site-map-provider-vb/_static/image18.png))


Nach Abschluss der Konfigurieren von Datenquellen-Assistenten erstellt Visual Studio entsprechende BoundFields und eine CheckBoxField in DetailsView für die Product-Datenfelder. Entfernen Sie die `ProductID`, `SupplierID`, und `CategoryID` BoundFields und konfigurieren Sie die verbleibenden Felder nach Bedarf. Nach ein paar ästhetischen Konfigurationen sah Meine DetailsView und ObjectDataSource-Steuerelement, deklarative s-Markup wie folgt aus:


[!code-aspx[Main](building-a-custom-database-driven-site-map-provider-vb/samples/sample4.aspx)]

Zum Testen auf dieser Seite zurück zu `Default.aspx` und klicken Sie auf Produkte anzeigen, für die Kategorie "Getränke". Klicken Sie auf den Link "Details anzeigen" für Chai Tee, aus der Liste der trinken Produkte. Dadurch gelangen Sie zu `ProductDetails.aspx?ProductID=1`, zeigt eine Chai Tee s Details (siehe Abbildung 14).


[![Chai Tee s Lieferanten, Kategorie, Preise und andere Informationen wird angezeigt.](building-a-custom-database-driven-site-map-provider-vb/_static/image14.gif)](building-a-custom-database-driven-site-map-provider-vb/_static/image19.png)

**Abbildung 14**: Chai Tee s Lieferanten, Kategorie, Preise und andere Informationen wird angezeigt ([klicken Sie, um das Bild in voller Größe anzeigen](building-a-custom-database-driven-site-map-provider-vb/_static/image20.png))


## <a name="step-5-understanding-the-inner-workings-of-a-site-map-provider"></a>Schritt 5: Verstehen, die interne Funktionsweise von ein Siteübersichtsanbieter

Die Sitemap wird in der Web-Server s-Speicher als eine Auflistung von dargestellt `SiteMapNode` -Instanzen, die eine Hierarchie zu bilden. Es muss genau ein Stammelement vorhanden sein, alle nicht-Stammknoten müssen genau einen übergeordneten Knoten und alle Knoten können eine beliebige Anzahl von untergeordneten Elemente aufweisen. Jede `SiteMapNode` Objekt stellt einen Abschnitt in der Website-s-Struktur dar, die in diesen Abschnitten verfügen häufig über eine entsprechende Webseite. Daher die [ `SiteMapNode` Klasse](https://msdn.microsoft.com/library/system.web.sitemapnode.aspx) verfügt über Eigenschaften wie `Title`, `Url`, und `Description`, die Angaben für den Abschnitt der `SiteMapNode` darstellt. Es gibt auch eine `Key` Eigenschaft, die jeweils eindeutig `SiteMapNode` in der Hierarchie angezeigt werden, sowie Eigenschaften verwendet, um diese Hierarchie herzustellen `ChildNodes`, `ParentNode`, `NextSibling`, `PreviousSibling`und so weiter.

Abbildung 15 zeigt die allgemeine Siteübersichtsstruktur aus Abbildung 1, jedoch mit den Implementierungsdetails genauere Details skizziert.


[![Jeder Sitemapknoten verfügt über Eigenschaften wie Titel, Url, Schlüssel und usw.](building-a-custom-database-driven-site-map-provider-vb/_static/image16.gif)](building-a-custom-database-driven-site-map-provider-vb/_static/image15.gif)

**Abbildung 15**: jede `SiteMapNode` verfügt über Eigenschaften wie `Title`, `Url`, `Key`und So weiter ([klicken Sie, um das Bild in voller Größe anzeigen](building-a-custom-database-driven-site-map-provider-vb/_static/image17.gif))


Die Siteübersicht ist über die [ `SiteMap` Klasse](https://msdn.microsoft.com/library/system.web.sitemap.aspx) in die [ `System.Web` Namespace](https://msdn.microsoft.com/library/system.web.aspx). Diese Klasse s `RootNode` Eigenschaft gibt das Stammverzeichnis der Site Map s `SiteMapNode` -Instanz `CurrentNode` gibt die `SiteMapNode` , deren `Url` Eigenschaft mit der URL, der die gerade angeforderte Seite übereinstimmt. Diese Klasse wird intern von ASP.NET 2.0 Web s Navigationssteuerelementen verwendet.

Wenn die `SiteMap` s-Klasse erfolgt, muss der Siteübersichtsstruktur aus einigen persistenten Medium in den Speicher serialisieren. Allerdings die Site Map Serialisierungslogik ist nicht hartcodiert in die `SiteMap` Klasse. Stattdessen zur Laufzeit die `SiteMap` -Klasse bestimmt, welche Sitemap *Anbieter* für die Serialisierung verwendet. In der Standardeinstellung die [ `XmlSiteMapProvider` Klasse](https://msdn.microsoft.com/library/system.web.xmlsitemapprovider.aspx) verwendet wird, die die Siteübersichtsstruktur s aus einer ordnungsgemäß formatierten XML-Datei lesen. Allerdings können wir unseren eigenen benutzerdefinierten Siteübersichtsanbieter mit ein wenig Arbeit erstellen.

Alle Sitezuordnungsprovider abgeleitet werden müssen die [ `SiteMapProvider` Klasse](https://msdn.microsoft.com/library/system.web.sitemapprovider.aspx), wozu die wichtigen Methoden und Eigenschaften, die für die Website benötigt Siteübersichtsanbieter, lässt aber viele der Details der Implementierung. Eine zweite Klasse [ `StaticSiteMapProvider` ](https://msdn.microsoft.com/library/system.web.staticsitemapprovider.aspx), erweitert die `SiteMapProvider` Klasse und enthält eine robustere Implementierung der erforderlichen Funktionen. Intern die `StaticSiteMapProvider` speichert die `SiteMapNode` ordnen Sie Instanzen der Website in eine `Hashtable` und bietet Methoden wie `AddNode(child, parent)`, `RemoveNode(siteMapNode),` und `Clear()` hinzugefügt oder entfernt `SiteMapNode` s, um die interne `Hashtable`. `XmlSiteMapProvider` wird von `StaticSiteMapProvider` abgeleitet.

Wenn einen benutzerdefinierter Sitezuordnungsprovider erstellen, die erweitert `StaticSiteMapProvider`, es gibt zwei abstrakte Methoden, die überschrieben werden müssen: [ `BuildSiteMap` ](https://msdn.microsoft.com/library/system.web.staticsitemapprovider.buildsitemap.aspx) und [ `GetRootNodeCore` ](https://msdn.microsoft.com/library/system.web.sitemapprovider.getrootnodecore.aspx). `BuildSiteMap`, wie der Name schon sagt, ist verantwortlich für die Siteübersichtsstruktur aus dem permanenten Speicher geladen und es im Arbeitsspeicher erstellen. `GetRootNodeCore` Gibt den Stammknoten in der Siteübersicht zurück.

Einer können Anwendung einen Siteübersichtsanbieter, die, den Sie in der Konfiguration der Anwendung registriert sein muss. In der Standardeinstellung die `XmlSiteMapProvider` -Klasse registriert wird, mit dem Namen `AspNetXmlSiteMapProvider`. Um zusätzliche Sitezuordnungsprovider zu registrieren, fügen Sie das folgende Markup zu `Web.config`:


[!code-xml[Main](building-a-custom-database-driven-site-map-provider-vb/samples/sample5.xml)]

Die *Namen* Wert weist einen lesbaren Namen für den Anbieter beim *Typ* gibt an, die den vollqualifizierten Typnamen des Siteübersichtsanbieters. Wir lernen konkrete Werte für die *Namen* und *Typ* Werte, die in Schritt 7, nachdem wir haben unsere benutzerdefinierter Sitezuordnungsprovider erstellt.

Die Site Map Provider-Klasse instanziiert wird beim ersten über erfolgt die `SiteMap` -Klasse und bleibt im Arbeitsspeicher für die Lebensdauer der Webanwendung. Da es nur eine Instanz des Siteübersichtsanbieters, die über mehrere, gleichzeitige Websitebesucher aufgerufen werden kann ist, ist es zwingend erforderlich, dass die s-Anbieter-Methoden werden *threadsichere*.

Für Leistung und Skalierbarkeit Gründe es wichtig, dass wir die in-Memory-Website Zwischenspeichern Struktur zuzuordnen und zurückgeben, dies zwischengespeichert, Struktur, anstatt ihn jedes Mal neu die `BuildSiteMap` -Methode wird aufgerufen. `BuildSiteMap` kann mehrere Male pro Seitenanforderung pro Benutzer und je nach der Steuerelemente für die Navigation verwendet auf der Seite und die Tiefe der Siteübersichtsstruktur aufgerufen werden. In jedem Fall, wenn es nicht in der Siteübersichtsstruktur zwischengespeichert `BuildSiteMap` dann jedes Mal, es aufgerufen wird, wir müssen der Produkt und die Kategorie der Architektur erneut abrufen von Informationen vom (die in einer Abfrage mit der Datenbank führen würde). Wie in den vorherigen Tutorials für die Zwischenspeicherung erläutert, können zwischengespeicherte Daten veralten. Um dieses Problem zu begegnen, können wir Time oder SQL-Cache Abhängigkeit basierende Cacheobjekte verwenden.

> [!NOTE]
> Ein Siteübersichtsanbieter kann optional außer Kraft setzen der [ `Initialize` Methode](https://msdn.microsoft.com/library/system.web.sitemapprovider.initialize.aspx). `Initialize` wird aufgerufen, wenn der Siteübersichtsanbieter zuerst instanziiert wird, und übergeben wird, die der Anbieter zugewiesen benutzerdefinierte Attribute `Web.config` in die `<add>` Element wie: `<add name="name" type="type" customAttribute="value" />`. Es ist hilfreich, wenn Sie zulassen, ein Seitenentwickler verschiedene Site Map anbieterbezogene Einstellungen angeben, ohne den Anbieter-s-Code ändern zu müssen möchten. Wenn wir die Kategorie und Produkte direkt von der Datenbank statt über die Architektur, d wir wahrscheinlich Lesen von Daten wurden z. B. den Seitenentwickler, die die Datenbank-Verbindungszeichenfolge durch angeben können `Web.config` statt einer hartcodierten der Wert im Anbietercode s. Überschreibt die benutzerdefinierter Sitezuordnungsprovider wir in Schritt 6 Erstellen nicht dadurch `Initialize` Methode. Ein Beispiel der Verwendung der `Initialize` -Methode finden Sie unter [Jeff Prosise](http://www.wintellect.com/Weblogs/CategoryView,category,Jeff%20Prosise.aspx) s [Sitemaps in SQL Server speichern](https://msdn.microsoft.com/msdnmag/issues/05/06/WickedCode/) Artikel.


## <a name="step-6-creating-the-custom-site-map-provider"></a>Schritt 6: Erstellen des benutzerdefinierten Siteübersichtsanbieters

Um einen benutzerdefinierten Sitezuordnungsprovider erstellen, in dem die Sitemap aus den Kategorien und Produkte in der Northwind-Datenbank erstellt, müssen wir eine Klasse erstellen, das erweitert `StaticSiteMapProvider`. In Schritt 1 der frage, Hinzufügen einer `CustomProviders` Ordner in der `App_Code` Ordner: Fügen Sie eine neue Klasse in diesem Ordner mit dem Namen `NorthwindSiteMapProvider`. Fügen Sie der `NorthwindSiteMapProvider` -Klasse folgenden Code hinzu:


[!code-vb[Main](building-a-custom-database-driven-site-map-provider-vb/samples/sample6.vb)]

Beginnen Sie mit dieser Klasse s untersuchen s können `BuildSiteMap` -Methode, die mit beginnt eine [ `lock` Anweisung](https://msdn.microsoft.com/library/c5kehkcz.aspx). Die `lock` Anweisung nur einem Thread gestattet gleichzeitig eingeben, wodurch Serialisierung des Zugriffs auf ihren Code und verhindert, dass zwei gleichzeitige Threads auf anderen s Zehen schrittweise.

Die auf Klassenebene `SiteMapNode` Variable `root` wird verwendet, um die Siteübersichtsstruktur Zwischenspeichern. Wenn die Sitemap erstellt wird, zum ersten Mal, oder klicken Sie zum ersten Mal nach die zugrunde liegenden Daten geändert wurde, `root` werden `Nothing` und der Siteübersichtsstruktur erstellt. Der s Stamm Siteübersichtsknoten zugewiesen `root` während der Erstellung des Prozesses, damit das nächste Mal, diese Methode aufgerufen wird, `root` nicht `Nothing`. Daher so lange `root` nicht `Nothing` der Siteübersichtsstruktur wird an den Aufrufer zurückgegeben werden, ohne Sie neu erstellt.

Wenn Stamm `Nothing`, die Siteübersichtsstruktur wird anhand der Informationen zum Produkt und die Kategorie erstellt. Die Sitemap wird erstellt, durch das Erstellen der `SiteMapNode` -Instanzen, und klicken Sie dann bilden die Hierarchie durch Aufrufe von der `StaticSiteMapProvider` s-Klasse `AddNode` Methode. `AddNode` führt die internen Verwaltungsaufgaben, speichern die gemischte `SiteMapNode` -Instanzen in einer `Hashtable`. Bevor wir beginnen, erstellen die Hierarchie, wir rufen Sie zunächst die `Clear` -Methode, die die Elemente aus der internen löscht `Hashtable`. Als Nächstes die `ProductsBLL` s-Klasse `GetProducts` -Methode und die daraus resultierende `ProductsDataTable` werden in lokalen Variablen gespeichert.

Das Site Map s erstellen, zunächst den Stammknoten erstellen und zuweisen zu `root`. Die Überladung von der [ `SiteMapNode` s Konstruktor](https://msdn.microsoft.com/library/system.web.sitemapnode.sitemapnode.aspx) verwendet, hier und in diesem gesamten `BuildSiteMap` die folgende Informationen übergeben wird:

- Ein Verweis auf das Site Map Provider (`Me`).
- Die `SiteMapNode` s `Key`. Dies erforderliche Wert für jede eindeutig sein muss `SiteMapNode`.
- Die `SiteMapNode` s `Url`. `Url` ist optional, aber wenn angegeben, von denen jede `SiteMapNode` s `Url` Wert muss eindeutig sein.
- Die `SiteMapNode` s `Title`, was erforderlich ist.

Die `AddNode(root)` Methodenaufruf fügt die `SiteMapNode` `root` der Sitemap als Stamm. Als Nächstes wird jede `ProductRow` in die `ProductsDataTable` aufgelistet. Wenn es bereits vorhanden ist eine `SiteMapNode` für die aktuelle s Produktkategorie, darauf verwiesen wird. Andernfalls ein neues `SiteMapNode` für die Kategorie erstellt und als untergeordnetes Element hinzugefügt wird die `SiteMapNode``root` über die `AddNode(categoryNode, root)` Methodenaufruf. Nach der entsprechenden Kategorie `SiteMapNode` Knoten gefunden oder erstellt haben, wurde eine `SiteMapNode` erstellt für das aktuelle Produkt und ist als untergeordnetes Element der Kategorie `SiteMapNode` über `AddNode(productNode, categoryNode)`. Beachten Sie, dass die Kategorie `SiteMapNode` s `Url` Eigenschaftswert ist `~/SiteMapProvider/ProductsByCategory.aspx?CategoryID=categoryID` zwar das Produkt `SiteMapNode` s `Url` -Eigenschaft zugewiesen `~/SiteMapNode/ProductDetails.aspx?ProductID=productID`.

> [!NOTE]
> Diese Produkte mit einer Datenbank `NULL` Wert für ihre `CategoryID` werden in einer Kategorie gruppiert `SiteMapNode` , deren `Title` -Eigenschaftensatz auf Sie keiner und deren `Url` Eigenschaft auf eine leere Zeichenfolge festgelegt ist. Ich habe mich entschieden, um festzulegen `Url` auf eine leere Zeichenfolge seit der `ProductBLL` s-Klasse `GetProductsByCategory(categoryID)` Methode derzeit verfügt nicht über die Möglichkeit, nur die Produkte mit einer `NULL` `CategoryID` Wert. Außerdem wollte ich veranschaulichen, wie die Steuerelemente für die Navigation gerendert eine `SiteMapNode` fehlt einen Wert für die `Url` Eigenschaft. Sollten Sie in diesem Tutorial erweitern, damit die None `SiteMapNode` s `Url` Eigenschaft verweist auf `ProductsByCategory.aspx`, noch zeigt nur die Produkte ohne `NULL` `CategoryID` Werte.


Nach dem Erstellen der Sitemap, ein beliebiges Objekt mit einer SQL-Cacheabhängigkeit in Datencache hinzugefügt wird die `Categories` und `Products` Tabellen über eine `AggregateCacheDependency` Objekt. Verwenden von SQL-cacheabhängigkeiten im vorherigen Tutorial haben wir untersucht *mithilfe von SQL-Cacheabhängigkeiten*. Benutzerdefinierte Siteübersichtsanbieter, verwendet jedoch eine Überladung der Datencache s `Insert` Methode, dass wir haben noch zu untersuchen. Diese Überladung, die als letzte Eingabeparameter akzeptiert ein Delegat, der aufgerufen wird, wenn das Objekt aus dem Cache entfernt wird. Insbesondere übergeben wir in einem neuen [ `CacheItemRemovedCallback` Delegieren](https://msdn.microsoft.com/library/system.web.caching.cacheitemremovedcallback.aspx) , verweist auf die `OnSiteMapChanged` Methode definiert die weiteren unten in der `NorthwindSiteMapProvider` Klasse.

> [!NOTE]
> Die in-Memory-Darstellung der Siteübersicht zwischengespeichert wird, über die Variable auf Klassenebene `root`. Da es nur eine Instanz der benutzerdefinierten Website Map Provider-Klasse, und ist da diese Instanz von allen Threads in der Webanwendung gemeinsam verwendet wird, dient diese Klassenvariable als Cache an. Die `BuildSiteMap` Methode verwendet auch den Datencache, jedoch nur als Mittel zur Benachrichtigung erhalten, wenn die zugrunde liegende Datenbank Daten in die `Categories` oder `Products` Änderungen Tabellen. Beachten Sie, dass der Wert, fügen Sie in Datencache nur das aktuelle Datum und die Uhrzeit ist. Ist die tatsächliche Siteübersichtsdaten *nicht* in Datencache einfügen.


Die `BuildSiteMap` Methode abgeschlossen wird, indem Sie den Stammknoten der Siteübersicht zurückgeben.

Die übrigen Methoden sind relativ einfach. `GetRootNodeCore` ist verantwortlich für die Rückgabe des Stammknotens. Da `BuildSiteMap` gibt das Stammelement, `GetRootNodeCore` gibt einfach auftragsantwortnachrichten zurück `BuildSiteMap` s Wert zurückgeben. Die `OnSiteMapChanged` Methode legt `root` an `Nothing` Wenn das Element im Cache entfernt wird. Mit dem Stamm zurückgesetzt `Nothing`nächsten `BuildSiteMap` wird aufgerufen, die Siteübersichtsstruktur wird neu erstellt. Und schließlich die `CachedDate` Eigenschaft gibt den gespeicherten in Datencache, Datum und Uhrzeit-Wert zurück, wenn ein solcher Wert vorhanden ist. Diese Eigenschaft kann von einem Seitenentwickler verwendet werden, um zu bestimmen, wann die Siteübersichtsdaten zuletzt zwischengespeichert wurde.

## <a name="step-7-registering-thenorthwindsitemapprovider"></a>Schritt 7: Registrieren der`NorthwindSiteMapProvider`

In der Reihenfolge für die Webanwendung verwendet die `NorthwindSiteMapProvider` Siteübersichtsanbieter in Schritt 6 erstellt haben, müssen wir registrieren in der `<siteMap>` Abschnitt `Web.config`. Insbesondere fügen das folgende Markup innerhalb der `<system.web>` Element im `Web.config`:


[!code-xml[Main](building-a-custom-database-driven-site-map-provider-vb/samples/sample7.xml)]

Dieses Markup führt zwei Aufgaben aus: zuerst, bedeutet dies, dass die integrierte `AspNetXmlSiteMapProvider` ist der Standard-Siteübersichtsanbieter; Zweitens registriert sie die benutzerdefinierter Sitezuordnungsprovider in Schritt 6 mit Benutzerfreundlicher Namen Northwind erstellt.

> [!NOTE]
> Für die Sitezuordnungsprovider befindet sich in der Anwendung s `App_Code` -Ordner, den Wert des der `type` -Attribut ist einfach der Klassenname. Alternativ die benutzerdefinierter Sitezuordnungsprovider hätte erstellt in einem separaten Klassenbibliotheksprojekt mit die kompilierte Assembly platziert, in der Web Application-s `/Bin` Verzeichnis. In diesem Fall die `type` Attributwert wäre *Namespace*. *ClassName*, *AssemblyName* .


Nach der Aktualisierung `Web.config`, können Sie jeder Seite in den Tutorials in einem Browser anzeigen. Beachten Sie, dass die Navigationsschnittstelle auf der linken Seite weiterhin in den Abschnitten zeigt und Tutorials in definierten `Web.sitemap`. Dies ist darauf zurückzuführen, dass wir `AspNetXmlSiteMapProvider` als Standardanbieter. Um ein Element der Benutzeroberfläche Navigation zu erstellen, verwendet der `NorthwindSiteMapProvider`, müssen wir explizit anzugeben, dass der Northwind-Siteübersichtsanbieter verwendet werden soll. Wir sehen, wie das in Schritt 8 geht.

## <a name="step-8-displaying-site-map-information-using-the-custom-site-map-provider"></a>Schritt 8: Anzeigen von Sitemapinformationen über die benutzerdefinierter Sitezuordnungsprovider

Mit der benutzerdefinierten Website Map Provider erstellt und registriert Sie `Web.config`, wir erneut bereit zum Hinzufügen von Navigationssteuerelementen zu den `Default.aspx`, `ProductsByCategory.aspx`, und `ProductDetails.aspx` Seiten in der `SiteMapProvider` Ordner. Öffnen Sie zunächst die `Default.aspx` Seite, und ziehen Sie eine `SiteMapPath` aus der Toolbox in den Designer. Das Steuerelement SiteMapPath befindet sich im Abschnitt "Navigation" der Toolbox.


[![Fügen Sie ein SiteMapPath an "default.aspx" hinzu.](building-a-custom-database-driven-site-map-provider-vb/_static/image19.gif)](building-a-custom-database-driven-site-map-provider-vb/_static/image18.gif)

**Abbildung 16**: Hinzufügen einer SiteMapPath, `Default.aspx` ([klicken Sie, um das Bild in voller Größe anzeigen](building-a-custom-database-driven-site-map-provider-vb/_static/image20.gif))


Das Steuerelement SiteMapPath zeigt Breadcrumb, der angibt, der des aktuellen Standort s innerhalb der sitezuordnung. Wir haben ein SiteMapPath hinzugefügt, an den Anfang der Masterseite zurück in die *Masterseiten und Sitenavigation* Tutorial.

Nehmen Sie einen Moment Zeit, zum Anzeigen dieser Seite über einen Browser ein. Die SiteMapPath hinzugefügt, die in Abbildung 16 verwendet den standardmäßigen SiteMapProvider, ziehen die Daten aus der `Web.sitemap`. Aus diesem Grund zeigt die Breadcrumb-Leiste Startseite &gt; Anpassen der Siteübersicht, genau wie die Breadcrumb-Leiste in der oberen rechten Ecke.


[![Die Breadcrumb-Leiste wird den Standard-Siteübersichtsanbieter verwendet.](building-a-custom-database-driven-site-map-provider-vb/_static/image22.gif)](building-a-custom-database-driven-site-map-provider-vb/_static/image21.gif)

**Abbildung 17**: der Brotkrümelnavigation verwendet die Standard-Siteübersichtsanbieter ([klicken Sie, um das Bild in voller Größe anzeigen](building-a-custom-database-driven-site-map-provider-vb/_static/image23.gif))


Wenn die SiteMapPath in Abbildung 16 hinzugefügt, die benutzerdefinierte Siteübersichtsanbieter verwenden wir in Schritt 6 erstellt haben, legen Sie dessen [ `SiteMapProvider` Eigenschaft](https://msdn.microsoft.com/library/system.web.ui.webcontrols.sitemappath.sitemapprovider.aspx) Northwind, den Namen wir zugewiesen, um die `NorthwindSiteMapProvider` in `Web.config`. Leider der Designer verwendet weiterhin den standardmäßigen SiteMapProvider, aber wenn Sie die Seite über einen Browser besuchen, nach dieser Eigenschaft Änderung sehen Sie, dass der Brotkrümelnavigation jetzt die benutzerdefinierter Sitezuordnungsprovider verwendet.


[![Die Breadcrumb-Leiste verwendet jetzt die benutzerdefinierte Website Map Provider NorthwindSiteMapProvider](building-a-custom-database-driven-site-map-provider-vb/_static/image25.gif)](building-a-custom-database-driven-site-map-provider-vb/_static/image24.gif)

**Abbildung 18**: der Brotkrümelnavigation verwendet jetzt die benutzerdefinierte Siteübersichtsanbieter `NorthwindSiteMapProvider` ([klicken Sie, um das Bild in voller Größe anzeigen](building-a-custom-database-driven-site-map-provider-vb/_static/image26.gif))


Das Steuerelement SiteMapPath zeigt eine funktionalere Schnittstelle für Benutzer in der `ProductsByCategory.aspx` und `ProductDetails.aspx` Seiten. Fügen Sie ein SiteMapPath auf diese Seiten, die Einstellung der `SiteMapProvider` Eigenschaft sowohl an Northwind. Von `Default.aspx` klicken Sie auf den Link Produkte anzeigen, für Getränke, und klicken Sie dann auf den Link "Details anzeigen" für Chai Tee. Wie in Abbildung 19 gezeigt, wird die Breadcrumb-Leiste enthält, dem aktuellen Standort Kartenbereich (Chai Tee) und dessen Vorgänger: Getränke und aller Kategorien.


[![Die Breadcrumb-Leiste verwendet jetzt die benutzerdefinierte Website Map Provider NorthwindSiteMapProvider](building-a-custom-database-driven-site-map-provider-vb/_static/image27.gif)](building-a-custom-database-driven-site-map-provider-vb/_static/image21.png)

**Abbildung 19**: der Brotkrümelnavigation verwendet jetzt die benutzerdefinierte Siteübersichtsanbieter `NorthwindSiteMapProvider` ([klicken Sie, um das Bild in voller Größe anzeigen](building-a-custom-database-driven-site-map-provider-vb/_static/image22.png))


Andere Elemente der Benutzeroberfläche Navigation können zusätzlich zu den SiteMapPath, z. B. die Steuerelemente Menu und TreeView verwendet werden. Die `Default.aspx`, `ProductsByCategory.aspx`, und `ProductDetails.aspx` Seiten im Download für dieses Tutorial, z. B. alle Menu-Steuerelemente (siehe Abbildung 20) enthalten. Finden Sie unter [Untersuchung von ASP.NET 2.0-s-Site Navigationsfunktionen](http://aspnet.4guysfromrolla.com/articles/111605-1.aspx) und [mithilfe von Steuerelementen](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/navigation/sitenavcontrols.aspx) Teil der [Schnellstarts zu ASP.NET 2.0](https://quickstarts.asp.net/QuickStartv20/aspnet/) eine ausführlichere Betrachtung der Steuerelemente für die Seitennavigation und Standortsystem über die Zuordnung in ASP.NET 2.0.


[![Das Menüsteuerelement enthält alle Kategorien und Produkte](building-a-custom-database-driven-site-map-provider-vb/_static/image29.gif)](building-a-custom-database-driven-site-map-provider-vb/_static/image28.gif)

**Abbildung 20**: die Menü-Steuerelement enthält jede der Kategorien und Produkte ([klicken Sie, um das Bild in voller Größe anzeigen](building-a-custom-database-driven-site-map-provider-vb/_static/image30.gif))


Wie weiter oben in diesem Tutorial erwähnt, die Siteübersichtsstruktur zugegriffen werden kann programmgesteuert über die `SiteMap` Klasse. Der folgende Code gibt den Stamm `SiteMapNode` des standardmäßigen Anbieters an:


[!code-vb[Main](building-a-custom-database-driven-site-map-provider-vb/samples/sample8.vb)]

Da die `AspNetXmlSiteMapProvider` ist der Standardanbieter bei unserer Anwendung würde der obige Code zurückgeben, den Stammknoten in definierten `Web.sitemap`. Um ein Siteübersichtsanbieter als den Standardwert zu verweisen, verwenden die `SiteMap` Klasse s [ `Providers` Eigenschaft](https://msdn.microsoft.com/library/system.web.sitemap.providers.aspx) wie folgt:


[!code-vb[Main](building-a-custom-database-driven-site-map-provider-vb/samples/sample9.vb)]

Wo *Namen* ist der Name der benutzerdefinierten Siteübersichtsanbieter (Northwind, für die Webanwendung).

Verwenden Sie den Zugriff auf einen Member, die spezifisch für ein Siteübersichtsanbieter `SiteMap.Providers["name"]` rufen Sie die Anbieterinstanz, und es dann in den entsprechenden Typ umwandeln. Beispielsweise zum Anzeigen der `NorthwindSiteMapProvider` s `CachedDate` -Eigenschaft in einer ASP.NET-Seite mithilfe des folgenden Codes:


[!code-vb[Main](building-a-custom-database-driven-site-map-provider-vb/samples/sample10.vb)]

> [!NOTE]
> Achten Sie darauf, dass die SQL-Cache-Abhängigkeit-Funktion zu testen. Nach dem Besuchen der `Default.aspx`, `ProductsByCategory.aspx`, und `ProductDetails.aspx` Seiten, wechseln Sie zu den Tutorials in das Bearbeiten, einfügen und Löschen im Abschnitt, und bearbeiten Sie den Namen einer Kategorie oder Produkt. Klicken Sie dann zurück zu einer der Seiten in der `SiteMapProvider` Ordner. Unter der Annahme der Abrufmechanismus Beachten Sie die Änderung der zugrunde liegenden Datenbank ausreichend Zeit verstrichen ist, sollte die Sitemap aktualisiert werden, um das neue Produkt oder den Kategorienamen anzuzeigen.


## <a name="summary"></a>Zusammenfassung

ASP.NET 2.0 s Site Map-Features umfasst eine `SiteMap` -Klasse, eine Anzahl von integrierten Navigation Web steuert und ein Standard-Siteübersichtsanbieter, die die Siteübersichtsinformationen erwartet, die in eine XML-Datei gespeichert. Um Siteübersichtsinformationen aus einer anderen Quelle wie z. B. aus einer Datenbank, die-s-Anwendungsarchitektur oder einen Remotewebdienst, den wir einen benutzerdefinierten Sitezuordnungsprovider erstellen müssen zu verwenden. Dies schließt die Erstellung einer Klasse, die direkt oder indirekt abgeleitet aus den `SiteMapProvider` Klasse.

In diesem Tutorial wurde erläutert, wie einen benutzerdefinierter Sitezuordnungsprovider erstellen, der die Sitemap auf das Produkt und die Kategorie-Informationen, die herausgefiltert, die von der Anwendungsarchitektur basieren. Unsere erweiterte Anbieter die `StaticSiteMapProvider` -Klasse und resultierendem erstellen eine `BuildSiteMap` -Methode, die die Daten abgerufen. die Map-Standorthierarchie erstellt und die resultierende Struktur in einer Variablen auf Klassenebene zwischengespeichert. Eine SQL-Cacheabhängigkeit und eine Callback-Funktion die zwischengespeicherten ungültig gemacht werden, verwendet beim Strukturieren der zugrunde liegende `Categories` oder `Products` Daten geändert.

Viel Spaß beim Programmieren!

## <a name="further-reading"></a>Weiterführende Themen

Weitere Informationen zu den Themen in diesem Tutorial erläutert finden Sie in den folgenden Ressourcen:

- [Speichern von Sitezuordnungen in SQL Server](https://msdn.microsoft.com/msdnmag/issues/05/06/WickedCode/) und [der SQL-SiteMapProvider Sie Ve gewartet](https://msdn.microsoft.com/msdnmag/issues/06/02/wickedcode/default.aspx)
- [Ein Blick auf ASP.NET 2.0-Model-s-Anbieter](http://aspnet.4guysfromrolla.com/articles/101905-1.aspx)
- [Die Anbietertoolkit](https://msdn.microsoft.com/asp.net/aa336558.aspx)
- [Untersuchen von ASP.NET 2.0 Navigationsfunktionen für s-Standorts](http://aspnet.4guysfromrolla.com/articles/111605-1.aspx)

## <a name="about-the-author"></a>Der Autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor von sieben Büchern zu ASP/ASP.NET und Gründer von [4GuysFromRolla.com](http://www.4guysfromrolla.com), arbeitet mit Microsoft-Web-Technologien seit 1998. Er ist als ein unabhängiger Berater, Schulungsleiter und Autor. Sein neueste Buch wird [*Sams Schulen selbst ASP.NET 2.0 in 24 Stunden*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Er ist unter [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) oder über seinen Blog finden Sie unter [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Besonderen Dank an

Diese tutorialreihe wurde durch viele hilfreiche Reviewer überprüft. Führendes Prüfer für dieses Tutorial wurden Dave Gardner, Zack Jones, Teresa Murphy und Bernadette Leigh. Meine zukünftigen MSDN-Artikeln überprüfen möchten? Wenn dies der Fall ist, löschen Sie mir eine Linie an [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Vorherige](building-a-custom-database-driven-site-map-provider-cs.md)
