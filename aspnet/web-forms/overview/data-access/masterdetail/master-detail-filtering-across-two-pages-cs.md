---
uid: web-forms/overview/data-access/masterdetail/master-detail-filtering-across-two-pages-cs
title: Master/Detail-Filterung über zwei Seiten (c#) | Microsoft Docs
author: rick-anderson
description: In diesem Lernprogramm implementieren wir dieses Muster mit GridView, die Lieferanten in der Datenbank aufzulisten. Jede Zeile Lieferanten in der GridView enthält eine Vie...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/31/2010
ms.topic: article
ms.assetid: 552d2d50-fe73-4153-9a7f-2b379bec4625
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/masterdetail/master-detail-filtering-across-two-pages-cs
msc.type: authoredcontent
ms.openlocfilehash: 0858bf9c8dc380898647293825145654ac1dbc34
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
---
<a name="masterdetail-filtering-across-two-pages-c"></a>Master/Detail-Filterung über zwei Seiten (c#)
====================
durch [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Beispiel-App herunterladen](http://download.microsoft.com/download/4/6/3/463cf87c-4724-4cbc-b7b5-3f866f43ba50/ASPNET_Data_Tutorial_9_CS.exe) oder [PDF herunterladen](master-detail-filtering-across-two-pages-cs/_static/datatutorial09cs1.pdf)

> In diesem Lernprogramm implementieren wir dieses Muster mit GridView, die Lieferanten in der Datenbank aufzulisten. Jede Zeile Lieferanten in der GridView enthält einen Link Produkte anzeigen, dass beim geklickt haben, wird den Benutzer auf ein anderes Zeichenblatt in Anspruch nehmen, die Produkte für den ausgewählten Lieferanten aufgelistet sind.


## <a name="introduction"></a>Einführung

In den vorherigen zwei Lernprogrammen Filtermenü wie [Master/Detail-Berichte in einer einzelnen Webseite mit DropDownLists angezeigt](master-detail-filtering-with-a-dropdownlist-cs.md) auf [anzuzeigen, die "master" Datensätze als auch ein GridView oder DetailsView-Steuerelement](master-detail-filtering-with-two-dropdownlists-cs.md) zum Anzeigen der " Details." Weiteres gängiges Muster für Master-/Detail-Berichte verwendet werden die master-Datensätze auf einer Webseite und die Details, die auf einem anderen gezeigt haben. Forum-Website, z. B. [ASP.NET Forums](https://forums.asp.net/), ist ein hervorragendes Beispiel für dieses Muster in der Praxis. Die ASP.NET Foren bestehen aus einer Vielzahl von Foren Einstieg, Web Forms, Präsentation Datensteuerelementen, und so weiter. Jede Forum besteht aus vielen Threads und jeder Thread besteht aus einer Anzahl von Übermittlungen. Auf der Startseite ASP.NET Foren sind die Foren aufgeführt. Klicken in einem Forum gelangen werden Sie zum augenblicklich eine `ShowForum.aspx` Seite, die die Threads dieses Forum aufgeführt. Ebenso durch Klicken auf einen Thread gelangen Sie zur `ShowPost.aspx`, die anzeigt, dass die Beiträge für den Thread, auf den geklickt wurde.

In diesem Lernprogramm implementieren wir dieses Muster mit GridView, die Lieferanten in der Datenbank aufzulisten. Jede Zeile Lieferanten in der GridView enthält einen Link Produkte anzeigen, dass beim geklickt haben, wird den Benutzer auf ein anderes Zeichenblatt in Anspruch nehmen, die Produkte für den ausgewählten Lieferanten aufgelistet sind.

## <a name="step-1-addingsupplierlistmasteraspxandproductsforsupplierdetailsaspxpages-to-thefilteringfolder"></a>Schritt 1: Hinzufügen von`SupplierListMaster.aspx`und`ProductsForSupplierDetails.aspx`Seiten auf den`Filtering`Ordner

Beim Definieren des Seitenlayouts in das dritte Lernprogramm hinzugefügt wir eine Anzahl von Startseiten von "" in der `BasicReporting`, `Filtering`, und `CustomFormatting` Ordner. Allerdings haben wir eine Seite "Starter" nicht für dieses Lernprogramm hinzugefügt, zu diesem Zeitpunkt sind, können Sie einen Moment Zeit, um zwei neue Seiten zum Hinzufügen der `Filtering` Ordner: `SupplierListMaster.aspx` und `ProductsForSupplierDetails.aspx`. `SupplierListMaster.aspx` Listet die "master" Datensätze (Suppliers) beim `ProductsForSupplierDetails.aspx` werden die Produkte für den ausgewählten Lieferanten angezeigt.

Wenn diese zwei neue Seiten erstellen werden sicher, dass diese mit Zuordnen der `Site.master` Masterseite.


![Hinzufügen des SupplierListMaster.aspx und ProductsForSupplierDetails.aspx Seiten zu den Filtern Ordner](master-detail-filtering-across-two-pages-cs/_static/image1.png)

**Abbildung 1**: Hinzufügen der `SupplierListMaster.aspx` und `ProductsForSupplierDetails.aspx` Seiten auf den `Filtering` Ordner


Wenn Sie neue Seiten zum Projekt hinzufügen, werden darüber hinaus sicher, dass die Standort-Zuordnungsdatei aktualisieren `Web.sitemap`dementsprechend. Für dieses Lernprogramm fügen Sie einfach die `SupplierListMaster.aspx` Seite mit den folgenden XML-Inhalt als untergeordnetes Element der Berichte filtern Siteübersicht `<siteMapNode>` Element:


[!code-xml[Main](master-detail-filtering-across-two-pages-cs/samples/sample1.xml)]

> [!NOTE]
> Tragen Sie zum Automatisieren der Aktualisierung von der Website-Zuordnungsdatei, beim Hinzufügen von neuen ASP.NET Seiten mit [K. Scott Allen](http://odetocode.com/Blogs/scott/)des Visual Studio frei [Standort Zuordnungsmakros](http://odetocode.com/Blogs/scott/archive/2005/11/29/2537.aspx).


## <a name="step-2-displaying-the-supplier-list-insupplierlistmasteraspx"></a>Schritt 2: Anzeigen der Lieferantenliste in`SupplierListMaster.aspx`

Mit der `SupplierListMaster.aspx` und `ProductsForSupplierDetails.aspx` Seiten erstellt wurden, im nächsten Schritt ist die Erstellung die GridView Lieferanten in `SupplierListMaster.aspx`. Die Seite eine GridView hinzu, und binden Sie es an eine neue ObjectDataSource. Diese ObjectDataSource sollten verwenden die `SuppliersBLL` Klasse `GetSuppliers()` Methode, um alle Lieferanten zurückzugeben.


[![Wählen Sie die SuppliersBLL-Klasse](master-detail-filtering-across-two-pages-cs/_static/image3.png)](master-detail-filtering-across-two-pages-cs/_static/image2.png)

**Abbildung 2**: Wählen Sie die `SuppliersBLL` Klasse ([klicken Sie hier, um das Bild in voller Größe angezeigt](master-detail-filtering-across-two-pages-cs/_static/image4.png))


[![Konfigurieren der ObjectDataSource zur Verwendung der GetSuppliers()-Methode](master-detail-filtering-across-two-pages-cs/_static/image6.png)](master-detail-filtering-across-two-pages-cs/_static/image5.png)

**Abbildung 3**: Konfigurieren der ObjectDataSource verwenden die `GetSuppliers()` Methode ([klicken Sie hier, um das Bild in voller Größe angezeigt](master-detail-filtering-across-two-pages-cs/_static/image7.png))


Wir müssen einen Link zum mit dem Titel anzeigen Produkte in jeder Zeile GridView, die beim Klicken auf wird der Benutzer auf `ProductsForSupplierDetails.aspx` übergeben, die in der ausgewählten Zeile `SupplierID` Wert über die Abfragezeichenfolge. Angenommen, klickt der Benutzer auf den Link Produkte anzeigen, für den Lieferanten Tokyo Traders (verfügt über eine `SupplierID` Wert 4), sie gesendet werden soll, um `ProductsForSupplierDetails.aspx?SupplierID=4`.

Um dies zu erreichen, fügen einen [HyperLinkField](https://msdn.microsoft.com/library/system.web.ui.webcontrols.hyperlinkfield.aspx) an die GridView, die einen Link auf jede Zeile GridView hinzufügt. Starten Sie, indem Sie auf den Link "Spalten bearbeiten" aus der GridView Smarttag. Als nächstes wählen Sie die HyperLinkField aus der Liste in der oberen linken Ecke, und klicken Sie auf Hinzufügen, um die GridView-Feldliste die HyperLinkField einschließt.


[![Hinzufügen einer HyperLinkField an die GridView](master-detail-filtering-across-two-pages-cs/_static/image9.png)](master-detail-filtering-across-two-pages-cs/_static/image8.png)

**Abbildung 4**: Hinzufügen einer HyperLinkField an die GridView ([klicken Sie hier, um das Bild in voller Größe angezeigt](master-detail-filtering-across-two-pages-cs/_static/image10.png))


Die HyperLinkField kann so konfiguriert werden, dass verwenden den gleichen Text oder URL der Link in jeder Zeile des GridView-Werte oder kann als Grundlage für diese Werte die Datenwerte an Datenzeilen bestimmte gebunden. Verwenden Sie an einen statischen Wert über alle Zeilen der HyperLinkField `Text` oder `NavigateUrl` Eigenschaften. Da den Text des Links für alle Zeilen identisch sein soll, legen Sie die HyperLinkField `Text` Eigenschaft, um Produkte anzuzeigen.


[![Legen Sie die HyperLinkField Texteigenschaft auf Produkte anzeigen](master-detail-filtering-across-two-pages-cs/_static/image12.png)](master-detail-filtering-across-two-pages-cs/_static/image11.png)

**Abbildung 5**: Legen Sie die HyperLinkField `Text` Eigenschaft, um Produkte anzeigen ([klicken Sie hier, um das Bild in voller Größe angezeigt](master-detail-filtering-across-two-pages-cs/_static/image13.png))


Der Text oder die URL-Werte auf der Grundlage der zugrunde liegenden Daten, die auf die Zeile GridView gebunden festlegen möchten, geben die Datenfelder des Texts oder die URL-Werte vom abgerufen werden sollen die `DataTextField` oder `DataNavigateUrlFields` Eigenschaften. `DataTextField` kann nur für ein einzelnes Datenfeld festgelegt werden. `DataNavigateUrlFields`, jedoch kann eine durch Trennzeichen getrennte Liste der Datenfelder festgelegt werden. Wir müssen häufig die Text oder die URL für eine Kombination der aktuellen Zeile Datenfeldwert auch einige statische Markup basieren. In diesem Lernprogramm angenommen, wir möchten die URL des Links für die HyperLinkField werden `ProductsForSupplierDetails.aspx?SupplierID=supplierID`, wobei *`supplierID`* ist jede GridView-Zeile `SupplierID` Wert. Beachten Sie, dass wir sowohl statische benötigen und datengesteuerte hier Werte: die `ProductsForSupplierDetails.aspx?SupplierID=` Teil der URL des Links wird statisch, während die *`supplierID`* Teil wird von datengesteuerten entspricht der Wert jeder Zeile des eigenen `SupplierID` Wert.

Um eine Kombination aus statischen und datengesteuerte Werte anzugeben, verwenden Sie die `DataTextFormatString` und `DataNavigateUrlFormatString` Eigenschaften. Diese Eigenschaften geben Sie das statische Markup nach Bedarf, und verwenden Sie dann die Markierung `{0}` , in dem den Wert des Felds im angegebenen sollen die `DataTextField` oder `DataNavigateUrlFields` Eigenschaften angezeigt werden. Wenn die `DataNavigateUrlFields` Eigenschaft verfügt über mehrere Felder angegebene Verwendung `{0}` , Sie möchten, dass den Wert des ersten Felds eingefügt, `{1}` für den zweiten Wert des Felds und So weiter.

Dies auf unser Tutorial angewendet haben, müssen wir Festlegen der `DataNavigateUrlFields` Eigenschaft, um `SupplierID`, da das Feld "Daten" ist, dessen Wert auf der Grundlage einer pro Zeile anpassen müssen und die `DataNavigateUrlFormatString` Eigenschaft `ProductsForSupplierDetails.aspx?SupplierID={0}`.


[![Konfigurieren Sie die HyperLinkField, um die richtigen Link-URL auf Grundlage der SupplierID enthalten](master-detail-filtering-across-two-pages-cs/_static/image15.png)](master-detail-filtering-across-two-pages-cs/_static/image14.png)

**Abbildung 6**: Konfigurieren der HyperLinkField der richtige Link-URL basierend auf Einbeziehung der `SupplierID` ([klicken Sie hier, um das Bild in voller Größe angezeigt](master-detail-filtering-across-two-pages-cs/_static/image16.png))


Nach dem Hinzufügen der HyperLinkField, gerne anpassen und die GridView Felder neu anordnen. Das folgende Markup zeigt die GridView aus, nachdem ich einige kleinere Feldebene Anpassungen vorgenommen haben.


[!code-aspx[Main](master-detail-filtering-across-two-pages-cs/samples/sample2.aspx)]

Erkundet zum Anzeigen der `SupplierListMaster.aspx` Seite über einen Browser. Wie Abbildung 7 zeigt ein Beispiel, listet die Seite derzeit alle Lieferanten, einschließlich eines Links Produkte anzeigen. Durch Klicken auf die Ansicht Produkte Link gelangen Sie zur `ProductsForSupplierDetails.aspx`, und übergeben Sie an des Lieferanten `SupplierID` in der Abfragezeichenfolge.


[![Jede Zeile Lieferanten enthält eine Ansichtsverknüpfung-Produkte](master-detail-filtering-across-two-pages-cs/_static/image18.png)](master-detail-filtering-across-two-pages-cs/_static/image17.png)

**Abbildung 7**: jede Zeile Lieferanten enthält eine Ansichtsverknüpfung-Produkte ([klicken Sie hier, um das Bild in voller Größe angezeigt](master-detail-filtering-across-two-pages-cs/_static/image19.png))


## <a name="step-3-listing-the-suppliers-products-inproductsforsupplierdetailsaspx"></a>Schritt 3: Auflisten des Lieferanten in`ProductsForSupplierDetails.aspx`

An diesem Punkt der `SupplierListMaster.aspx` Seite sendet Benutzer `ProductsForSupplierDetails.aspx`, übergeben des ausgewählten Lieferanten `SupplierID` in der Abfragezeichenfolge. Letzte Schritt des Lernprogramms wird für die Produkte in einer GridView in `ProductsForSupplierDetails.aspx` , deren `SupplierID` entspricht der `SupplierID` mithilfe der Abfragezeichenfolge übergeben. Zum Starten zu erreichen, indem Sie eine GridView zum Hinzufügen der `ProductsForSupplierDetails.aspx` Seite, ein neues ObjectDataSource-Steuerelement mit dem Namen `ProductsBySupplierDataSource` , aufruft der `GetProductsBySupplierID(supplierID)` Methode aus der `ProductsBLL` Klasse.


[![Fügen Sie eine neue, mit dem Namen ProductsBySupplierDataSource ObjectDataSource hinzu](master-detail-filtering-across-two-pages-cs/_static/image21.png)](master-detail-filtering-across-two-pages-cs/_static/image20.png)

**Abbildung 8**: Hinzufügen einer neuen ObjectDataSource namens `ProductsBySupplierDataSource` ([klicken Sie hier, um das Bild in voller Größe angezeigt](master-detail-filtering-across-two-pages-cs/_static/image22.png))


[![Wählen Sie die ProductsBLL-Klasse](master-detail-filtering-across-two-pages-cs/_static/image24.png)](master-detail-filtering-across-two-pages-cs/_static/image23.png)

**Abbildung 9**: Wählen Sie die `ProductsBLL` Klasse ([klicken Sie hier, um das Bild in voller Größe angezeigt](master-detail-filtering-across-two-pages-cs/_static/image25.png))


[![Haben Sie das Aufrufen der Methode GetProductsBySupplierID(supplierID) ObjectDataSource](master-detail-filtering-across-two-pages-cs/_static/image27.png)](master-detail-filtering-across-two-pages-cs/_static/image26.png)

**Abbildung 10**: haben Sie die ObjectDataSource Aufrufen der `GetProductsBySupplierID(supplierID)` Methode ([klicken Sie hier, um das Bild in voller Größe angezeigt](master-detail-filtering-across-two-pages-cs/_static/image28.png))


Der letzte Schritt des Assistenten zum Konfigurieren von Datenquellen gefragt werden, geben Sie die Quelle des der `GetProductsBySupplierID(supplierID)` Methode *`supplierID`* Parameter. Um die Querystring-Wert zu verwenden, legen Sie die Parameter-Quelle auf QueryString, und geben Sie den Namen der Querystring-Wert, der in das Textfeld QueryStringField verwenden (`SupplierID`).


[![Füllen Sie die SupplierID Parameterwert aus dem SupplierID Querystring-Wert](master-detail-filtering-across-two-pages-cs/_static/image30.png)](master-detail-filtering-across-two-pages-cs/_static/image29.png)

**Abbildung 11**: Auffüllen der *`supplierID`* Parameterwert aus dem `SupplierID` Querystring-Wert ([klicken Sie hier, um das Bild in voller Größe angezeigt](master-detail-filtering-across-two-pages-cs/_static/image31.png))


Das ist alles vorhanden ist! Abbildung 12 zeigt die `ProductsForSupplierDetails.aspx` Startseite bei besucht haben, indem Sie auf den Link Tokyo Traders aus `SupplierListMaster.aspx`.


[![Die Produkte, die von der Tokio Traders bereitgestellt werden angezeigt.](master-detail-filtering-across-two-pages-cs/_static/image33.png)](master-detail-filtering-across-two-pages-cs/_static/image32.png)

**Abbildung 12**: der Produkte von Tokyo Traders bereitgestellt werden angezeigt ([klicken Sie hier, um das Bild in voller Größe angezeigt](master-detail-filtering-across-two-pages-cs/_static/image34.png))


## <a name="displaying-supplier-information-inproductsforsupplierdetailsaspx"></a>Anzeigen von Lieferanteninformationen zu in`ProductsForSupplierDetails.aspx`

Wie in Abbildung 12 gezeigt, die `ProductsForSupplierDetails.aspx` Seite listet die Produkte, die vom angegebenen der `SupplierID` in der Abfragezeichenfolge angegeben. Ein Benutzer direkt zu dieser Seite gesendet würde jedoch nicht wissen, dass Abbildung 12 Tokyo Traders Produkte angezeigt wird. Um dieses Problem zu beheben Lieferanteninformationen auf dieser Seite auch angezeigt werden können.

Durch hinzufügen einen FormView über die Produkte GridView starten. Erstellen Sie ein neues ObjectDataSource-Steuerelement mit dem Namen `SuppliersDataSource` aufruft, die die `SuppliersBLL` Klasse `GetSupplierBySupplierID(supplierID)` Methode.


[![Wählen Sie die SuppliersBLL-Klasse](master-detail-filtering-across-two-pages-cs/_static/image36.png)](master-detail-filtering-across-two-pages-cs/_static/image35.png)

**Abbildung 13**: Wählen Sie die `SuppliersBLL` Klasse ([klicken Sie hier, um das Bild in voller Größe angezeigt](master-detail-filtering-across-two-pages-cs/_static/image37.png))


[![Haben Sie das Aufrufen der Methode GetSupplierBySupplierID(supplierID) ObjectDataSource](master-detail-filtering-across-two-pages-cs/_static/image39.png)](master-detail-filtering-across-two-pages-cs/_static/image38.png)

**Abbildung 14**: haben Sie die ObjectDataSource Aufrufen der `GetSupplierBySupplierID(supplierID)` Methode ([klicken Sie hier, um das Bild in voller Größe angezeigt](master-detail-filtering-across-two-pages-cs/_static/image40.png))


Wie bei der `ProductsBySupplierDataSource`, haben die *`supplierID`* Parameter der Wert zugewiesen. die `SupplierID` Querystring-Wert.


[![Füllen Sie die SupplierID Parameterwert aus dem SupplierID Querystring-Wert](master-detail-filtering-across-two-pages-cs/_static/image42.png)](master-detail-filtering-across-two-pages-cs/_static/image41.png)

**Abbildung 15**: Auffüllen der *`supplierID`* Parameterwert aus dem `SupplierID` Querystring-Wert ([klicken Sie hier, um das Bild in voller Größe angezeigt](master-detail-filtering-across-two-pages-cs/_static/image43.png))


Beim Binden von FormView an das ObjectDataSource in der Entwurfsansicht, Visual Studio erstellt automatisch die FormView `ItemTemplate`, `InsertItemTemplate`, und `EditItemTemplate` mit der Bezeichnung und TextBox-Web-Steuerelemente für die einzelnen Datenfelder zurückgegebenes der ObjectDataSource. Da wir nur die Lieferanten Informationen gerne entfernen anzeigen möchten die `InsertItemTemplate` und `EditItemTemplate`. Als Nächstes ItemTemplate bearbeiten, sodass es zeigt, dass der Lieferant Unternehmensname in einer `<h3>` Element und die Adresse, City, Country und Telefonnummer unterhalb des Unternehmens an. Alternativ können Sie manuell die FormView festlegen `DataSourceID` , und erstellen Sie die `ItemTemplate` Markup, wie es wieder in die "[Anzeigen von Daten mit der ObjectDataSource](../basic-reporting/displaying-data-with-the-objectdatasource-cs.md)" Lernprogramm.

Nach dieser Änderungen an der sollte die FormView deklarative Markup etwa wie folgt aussehen:


[!code-aspx[Main](master-detail-filtering-across-two-pages-cs/samples/sample3.aspx)]

Abbildung 16 zeigt einen Screenshot, der die `ProductsForSupplierDetails.aspx` Seite nach der oben aufgeführten Lieferanteninformationen enthalten.


[![Die Liste der Produkte enthält eine Zusammenfassung zum Lieferanten](master-detail-filtering-across-two-pages-cs/_static/image45.png)](master-detail-filtering-across-two-pages-cs/_static/image44.png)

**Abbildung 16**: die Liste der Produkte enthält eine Zusammenfassung über die Lieferanten ([klicken Sie hier, um das Bild in voller Größe angezeigt](master-detail-filtering-across-two-pages-cs/_static/image46.png))


## <a name="applying-the-final-touches-for-theproductsforsupplierdetailsaspxui"></a>Anwenden der endgültige berührt, für die`ProductsForSupplierDetails.aspx`UI

Zur Verbesserung des Benutzers gibt Erfahrung für diesen Bericht gibt es eine Reihe von Ergänzungen, die wir eingefügt werden, stellen die `ProductsForSupplierDetails.aspx` Seite. Derzeit die einzige Möglichkeit, die ein Benutzer, von wechseln kann der `ProductsForSupplierDetails.aspx` Seite zurück zur Liste der Lieferanten wird auf ihren Browser-zurück-Schaltfläche klicken. Fügen Sie ein Linksteuerelement auf der `ProductsForSupplierDetails.aspx` Seite, die zurück auf verweist `SupplierListMaster.aspx`, bietet eine weitere Möglichkeit für den Benutzer wieder die Masterliste Ihrer.


[![Hinzufügen eines Linksteuerelements, um den Benutzer wieder auf SupplierListMaster.aspx übernehmen](master-detail-filtering-across-two-pages-cs/_static/image48.png)](master-detail-filtering-across-two-pages-cs/_static/image47.png)

**Abbildung 17**: Hinzufügen eines Steuerelements Link, um den Benutzer wieder zu übernehmen, `SupplierListMaster.aspx` ([klicken Sie hier, um das Bild in voller Größe angezeigt](master-detail-filtering-across-two-pages-cs/_static/image49.png))


Klickt der Benutzer auf einem anderen Lieferanten Produkte anzeigen, die keine Produkte aus, die `ProductsBySupplierDataSource` ObjectDataSource in `ProductsForSupplierDetails.aspx` nicht keine Ergebnisse zurückgegeben. Die GridView gebunden werden, um das ObjectDataSource wird nicht in einen leeren Bereich auf der Seite im Browser des Benutzers resultierende Markup gerendert. Genauer an den Benutzer kommunizieren, sind keine Produkte, die den ausgewählten Lieferanten zugeordnet können wir der GridView festlegen `EmptyDataText` Eigenschaft, um die Nachricht, die wir angezeigt werden sollen, wenn dies der Fall ist. Diese Eigenschaft auf "Sind keine Produkte, die von diesem Lieferanten bereitgestellte" haben festgelegt werden.

Standardmäßig geben alle Lieferanten in der Employees-Datenbank mindestens ein Produkt. Allerdings für dieses Lernprogramm ich manuell geändert haben die `Products` Tabelle, damit der Lieferant Escargots Nouveaux nicht mehr alle Produkte zugeordnet ist. Abbildung 18 zeigt die Seite "Details" für Escargots Nouveaux, nachdem diese Änderung vorgenommen wurde.


[![Benutzer werden darüber informiert, dass der Lieferant keine Produkte bereitstellt](master-detail-filtering-across-two-pages-cs/_static/image51.png)](master-detail-filtering-across-two-pages-cs/_static/image50.png)

**Abbildung 18**: Benutzer werden darüber informiert, dass der Lieferant keine Produkte bereitstellt ([klicken Sie hier, um das Bild in voller Größe angezeigt](master-detail-filtering-across-two-pages-cs/_static/image52.png))


## <a name="summary"></a>Zusammenfassung

Während der Master und die Detaildatensätze auf einer einzelnen Seite Master/Detail-Berichte angezeigt werden können, werden auf vielen Websites sie über zwei Webseiten ausgelagert. In diesem Lernprogramm erläutert wir, wie solche einen Master/Detail-Bericht implementiert wird, dass die Lieferanten, die in einem GridView in der "master" Webseite aufgeführt und die zugehörigen Produkte aufgeführt, die auf der Seite "Details". Jede Zeile Lieferanten in der master-Webseite enthalten einen Link zu der Detailseite, die der Zeile übergeben `SupplierID` Wert. Solche Verknüpfungen zeilenspezifische können problemlos mit der GridView HyperLinkField hinzugefügt werden.

In der Seite "Details" Abrufen von dieser Produkte für den angegebenen Lieferanten wurde durchgeführt, durch den Aufruf der `ProductsBLL` Klasse `GetProductsBySupplierID(supplierID)` Methode. Die *`supplierID`* Parameterwert wurde deklarativ mithilfe der Abfragezeichenfolge als Parameterquelle für den angegeben. Wir haben uns auch an, wie in der Seite Details zum Verwenden eines FormView der Lieferanten-Details anzeigen.

Unsere [nächsten Lernprogramm](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs.md) ist der letzte Auftrag für Master-/Detail-Berichte. Betrachten wir eine Liste der Produkte in einer GridView anzeigen, in dem jede Zeile eine Schaltfläche auswählen aufweist. Durch Klicken auf die Schaltfläche auswählen, wird das Produkt Details in einem DetailsView-Steuerelement auf derselben Seite angezeigt.

Viel Spaß beim Programmieren!

## <a name="about-the-author"></a>Informationen zum Autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor von sieben ASP/ASP.NET-Büchern und Gründer von [4GuysFromRolla.com](http://www.4guysfromrolla.com), Microsoft Web-Technologien seit 1998 arbeitet. Scott fungiert als ein unabhängiger Berater, Trainer und Writer. Sein neueste Buch wird [ *Sams Schulen selbst ASP.NET 2.0 in 24 Stunden*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Er die erreicht werden kann, zur [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) oder über seinen Blog die finden Sie unter [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Besonderen Dank an

Diese Reihe von Lernprogrammen wurde durch viele nützliche Bearbeiter überprüft. Lead Prüfer für dieses Lernprogramm wurde Hilton Giesenow. Meine bevorstehende MSDN-Artikel Überprüfen von Interesse? Wenn dies der Fall ist, löschen Sie mich zeilenweise [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Zurück](master-detail-filtering-with-two-dropdownlists-cs.md)
> [Weiter](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs.md)
