---
uid: web-forms/overview/data-access/masterdetail/master-detail-filtering-across-two-pages-cs
title: Master/Detail-Filtern über zwei Seiten (c#) | Microsoft-Dokumentation
author: rick-anderson
description: In diesem Tutorial werden wir dieses Muster implementieren, mit einer GridView-Ansicht die Lieferanten in der Datenbank aufgelistet. Jede Zeile Lieferanten in den GridView-Ansicht enthält eine Vie...
ms.author: riande
ms.date: 03/31/2010
ms.assetid: 552d2d50-fe73-4153-9a7f-2b379bec4625
msc.legacyurl: /web-forms/overview/data-access/masterdetail/master-detail-filtering-across-two-pages-cs
msc.type: authoredcontent
ms.openlocfilehash: 69e5f010507784229360f71cf6f570b342f5ff46
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2018
ms.locfileid: "41827272"
---
<a name="masterdetail-filtering-across-two-pages-c"></a>Master/Detail-Filtern über zwei Seiten (c#)
====================
durch [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Beispiel-App herunter](http://download.microsoft.com/download/4/6/3/463cf87c-4724-4cbc-b7b5-3f866f43ba50/ASPNET_Data_Tutorial_9_CS.exe) oder [PDF-Datei herunterladen](master-detail-filtering-across-two-pages-cs/_static/datatutorial09cs1.pdf)

> In diesem Tutorial werden wir dieses Muster implementieren, mit einer GridView-Ansicht die Lieferanten in der Datenbank aufgelistet. Jede Zeile Lieferanten in den GridView-Ansicht enthält einen Link für Produkte anzeigen, wenn auf Sie geklickt wird, gelangt der Benutzer auf eine separate Seite, diese Produkte für den ausgewählten Lieferanten aufgelistet sind.


## <a name="introduction"></a>Einführung

In den vorherigen beiden Tutorials erläutert, wie [Master/Detail-Berichte in einer einzelnen Webseite mit DropDownList-Steuerelementen anzeigen](master-detail-filtering-with-a-dropdownlist-cs.md) zu [anzuzeigen, die "master" Datensätze als auch ein GridView- oder DetailsView-Steuerelement](master-detail-filtering-with-two-dropdownlists-cs.md) zum Anzeigen der " Details." Ein weiteres allgemeines Muster für Master-/Detail-Berichte verwendet, ist die master-Datensätze auf einer Webseite und die Details, die auf einem anderen angezeigt. Ein Forum-Website, z. B. [der ASP.NET-Foren](https://forums.asp.net/), ist ein gutes Beispiel dieses Musters in der Praxis. Die ASP.NET-Foren bestehen aus einer Vielzahl von Foren beginnen, Webformulare, Steuerelemente für Präsentation und so weiter. Jedes Forum besteht aus vielen Threads aus, und jeder Thread besteht aus einer Reihe von Beiträgen. Auf der Startseite ASP.NET-Foren werden in den Foren aufgeführt. Klicken in einem Forum Verlaufsbereich Ihnen, eine `ShowForum.aspx` Seite, die die Threads dieses Forum enthält. Ebenso in einem Thread gelangen Sie auf `ShowPost.aspx`, woraufhin die Beiträge für den Thread, auf die geklickt wurde.

In diesem Tutorial werden wir dieses Muster implementieren, mit einer GridView-Ansicht die Lieferanten in der Datenbank aufgelistet. Jede Zeile Lieferanten in den GridView-Ansicht enthält einen Link für Produkte anzeigen, wenn auf Sie geklickt wird, gelangt der Benutzer auf eine separate Seite, diese Produkte für den ausgewählten Lieferanten aufgelistet sind.

## <a name="step-1-addingsupplierlistmasteraspxandproductsforsupplierdetailsaspxpages-to-thefilteringfolder"></a>Schritt 1: Hinzufügen von`SupplierListMaster.aspx`und`ProductsForSupplierDetails.aspx`auf Seiten der`Filtering`Ordner

Bei das Seitenlayout in das dritte Tutorial definiert haben wir eine Anzahl von Seiten von "Starter" in der `BasicReporting`, `Filtering`, und `CustomFormatting` Ordner. Allerdings haben wir keine Startseite für dieses Tutorial zu diesem Zeitpunkt hinzugefügt, daher können Sie zwei neue Seiten zum Hinzufügen der `Filtering` Ordner: `SupplierListMaster.aspx` und `ProductsForSupplierDetails.aspx`. `SupplierListMaster.aspx` Listet die "master"-Einträge (den Lieferanten) beim `ProductsForSupplierDetails.aspx` zeigt die Produkte für den ausgewählten Lieferanten.

Wenn diese zwei neuen Seiten erstellen sicher sein, die sie zugeordnet werden sollen die `Site.master` Masterseite.


![Fügen Sie die SupplierListMaster.aspx und ProductsForSupplierDetails.aspx Seiten, zu dem Ordner filtern](master-detail-filtering-across-two-pages-cs/_static/image1.png)

**Abbildung 1**: Hinzufügen der `SupplierListMaster.aspx` und `ProductsForSupplierDetails.aspx` auf Seiten der `Filtering` Ordner


Darüber hinaus neue Seiten zum Projekt hinzufügen möchten, achten Sie beim Aktualisieren der Sitezuordnungsdatei, `Web.sitemap`dementsprechend. In diesem Tutorial fügen Sie einfach die `SupplierListMaster.aspx` Seite, um die Sitemap, die mit dem folgenden XML-Inhalt als untergeordnetes Element der Berichte filtern `<siteMapNode>` Element:


[!code-xml[Main](master-detail-filtering-across-two-pages-cs/samples/sample1.xml)]

> [!NOTE]
> Helfen Sie automatisieren den Prozess der Aktualisierung der Sitezuordnungsdatei, beim Hinzufügen der neuen ASP.NET Seiten mit [K. Scott Allen](http://odetocode.com/Blogs/scott/)die kostenlose Visual Studio [Site Map-Makro](http://odetocode.com/Blogs/scott/archive/2005/11/29/2537.aspx).


## <a name="step-2-displaying-the-supplier-list-insupplierlistmasteraspx"></a>Schritt 2: Anzeigen der Lieferantenliste in`SupplierListMaster.aspx`

Mit der `SupplierListMaster.aspx` und `ProductsForSupplierDetails.aspx` Seiten erstellt wurden, im nächsten Schritt ist die Erstellung von Lieferanten in GridView `SupplierListMaster.aspx`. Fügen Sie einer GridView-Ansicht auf der Seite hinzu, und binden Sie es an eine neue "ObjectDataSource". Diese "ObjectDataSource" verwenden, sollten die `SuppliersBLL` Klasse `GetSuppliers()` Methode, um alle Lieferanten zurückzugeben.


[![Wählen Sie die SuppliersBLL-Klasse](master-detail-filtering-across-two-pages-cs/_static/image3.png)](master-detail-filtering-across-two-pages-cs/_static/image2.png)

**Abbildung 2**: Wählen Sie die `SuppliersBLL` Klasse ([klicken Sie, um das Bild in voller Größe anzeigen](master-detail-filtering-across-two-pages-cs/_static/image4.png))


[![Konfigurieren von dem ObjectDataSource-Steuerelement zur Verwendung der GetSuppliers()-Methode](master-detail-filtering-across-two-pages-cs/_static/image6.png)](master-detail-filtering-across-two-pages-cs/_static/image5.png)

**Abbildung 3**: Konfigurieren von dem ObjectDataSource-Steuerelement verwenden die `GetSuppliers()` Methode ([klicken Sie, um das Bild in voller Größe anzeigen](master-detail-filtering-across-two-pages-cs/_static/image7.png))


Wir müssen einen Link mit dem Titel zeigt Produkte in jeder GridView-Zeile, die beim Klicken auf gelangt der Benutzer zur `ProductsForSupplierDetails.aspx` übergeben, die in der ausgewählten Zeile `SupplierID` Wert über die Abfragezeichenfolge. Z. B., wenn Benutzer auf den Link Produkte anzeigen, für den Lieferanten Tokyo Traders klickt (die eine `SupplierID` Wert von 4), sie gesendet werden sollen `ProductsForSupplierDetails.aspx?SupplierID=4`.

Um dies zu erreichen, fügen einen [HyperLinkField](https://msdn.microsoft.com/library/system.web.ui.webcontrols.hyperlinkfield.aspx) an die GridView, die einen Link auf jede GridView-Zeile hinzufügt. Starten Sie, indem Sie auf den Link "Spalten bearbeiten" aus den GridView Smarttag. Als nächstes wählen Sie die HyperLinkField aus der Liste in der oberen linken Ecke, und klicken Sie auf Hinzufügen, um die HyperLinkField in den GridView Feldliste enthalten.


[![Hinzufügen einer HyperLinkField an die GridView](master-detail-filtering-across-two-pages-cs/_static/image9.png)](master-detail-filtering-across-two-pages-cs/_static/image8.png)

**Abbildung 4**: Hinzufügen einer HyperLinkField an die GridView ([klicken Sie, um das Bild in voller Größe anzeigen](master-detail-filtering-across-two-pages-cs/_static/image10.png))


Die HyperLinkField kann so konfiguriert werden, dass Sie verwenden den gleichen Text oder URL den Link in jeder Zeile GridView Werte oder kann als Grundlage für diese Werte die Datenwerte, die auf jede Zeile der bestimmte gebunden. An einen statischen Wert übergreifend für alle Zeilen verwenden, die HyperLinkField des `Text` oder `NavigateUrl` Eigenschaften. Da wir den Text des Links für alle Zeilen identisch sein soll, legen Sie die HyperLinkField des `Text` Eigenschaft, um Produkte anzeigen.


[![Legen Sie die HyperLinkField des Text-Eigenschaft auf Produkte anzeigen](master-detail-filtering-across-two-pages-cs/_static/image12.png)](master-detail-filtering-across-two-pages-cs/_static/image11.png)

**Abbildung 5**: Legen Sie die HyperLinkField des `Text` Eigenschaft, um Produkte anzeigen ([klicken Sie, um das Bild in voller Größe anzeigen](master-detail-filtering-across-two-pages-cs/_static/image13.png))


Geben Sie zum Festlegen der Text oder die URL-Werte auf Basis der zugrunde liegenden Daten, die an die GridView-Zeile gebunden werden die Daten Felder, die Text oder URL-Werte abgerufen werden soll, aus der `DataTextField` oder `DataNavigateUrlFields` Eigenschaften. `DataTextField` kann nur auf ein einzelnes Datenfeld festgelegt werden. `DataNavigateUrlFields`, jedoch kann festgelegt werden, um eine durch Trennzeichen getrennte Liste von Datenfeldern. Wir müssen häufig, wenn der Text oder eine URL auf eine Kombination aus der aktuellen Zeile Datenfeldwert und statischen Markup basieren. In diesem Tutorial haben wir z. B. die URL der HyperLinkFields Links auf `ProductsForSupplierDetails.aspx?SupplierID=supplierID`, wobei *`supplierID`* ist jede GridView Zeile `SupplierID` Wert. Beachten Sie, dass wir sowohl statische benötigen und hier für datengesteuerte Werte: die `ProductsForSupplierDetails.aspx?SupplierID=` Teil der URL des Links ist statisch, während die *`supplierID`* Teil wird von datengesteuerten wie der Wert jeder Zeile des eigenen `SupplierID` Wert.

Um eine Kombination aus statischen und datengesteuerten Werte anzugeben, verwenden Sie die `DataTextFormatString` und `DataNavigateUrlFormatString` Eigenschaften. Diese Eigenschaften geben Sie die statische Markup nach Bedarf und verwenden Sie dann die Markierung `{0}` möchten Sie den Wert des Felds angegeben wird, der `DataTextField` oder `DataNavigateUrlFields` Eigenschaften angezeigt werden. Wenn die `DataNavigateUrlFields` Eigenschaft verfügt über mehrere Felder angegebene Verwendung `{0}` bei der ersten Feldwert eingefügt wird, soll `{1}` für den zweiten Wert des Felds, und So weiter.

Dies auf unser Tutorial angewendet haben, müssen wir legen die `DataNavigateUrlFields` Eigenschaft `SupplierID`, da dies das Datenfeld ist, dessen Wert auf einer Basis pro Zeile anpassen möchten und die `DataNavigateUrlFormatString` Eigenschaft, um `ProductsForSupplierDetails.aspx?SupplierID={0}`.


[![Konfigurieren Sie die HyperLinkField Einbeziehung die richtige Link-URL, die auf Grundlage der SupplierID](master-detail-filtering-across-two-pages-cs/_static/image15.png)](master-detail-filtering-across-two-pages-cs/_static/image14.png)

**Abbildung 6**: Konfigurieren der HyperLinkField für die Einbeziehung der richtige Link-URL basierend auf den `SupplierID` ([klicken Sie, um das Bild in voller Größe anzeigen](master-detail-filtering-across-two-pages-cs/_static/image16.png))


Nach dem Hinzufügen der HyperLinkField, gerne anpassen und GridView Felder neu anordnen. Das folgende Markup zeigt die GridView, nachdem ich einige kleinen Anpassungen der Feldebene vorgenommen haben.


[!code-aspx[Main](master-detail-filtering-across-two-pages-cs/samples/sample2.aspx)]

Anzeigen in Ruhe die `SupplierListMaster.aspx` Seite über einen Browser. Wie in Abbildung 7 dargestellt, listet die Seite derzeit alle die Lieferanten, die mit einem Link Produkte anzeigen. Durch Klicken auf die Ansicht Produkte Link gelangen Sie zur `ProductsForSupplierDetails.aspx`, und übergeben Sie entlang des Lieferanten `SupplierID` in der Abfragezeichenfolge.


[![Jeder Lieferant Zeile enthält eine Ansichtsverknüpfung-Produkte](master-detail-filtering-across-two-pages-cs/_static/image18.png)](master-detail-filtering-across-two-pages-cs/_static/image17.png)

**Abbildung 7**: jede Zeile der Lieferant enthält, eine Ansichtsverknüpfung-Produkte ([klicken Sie, um das Bild in voller Größe anzeigen](master-detail-filtering-across-two-pages-cs/_static/image19.png))


## <a name="step-3-listing-the-suppliers-products-inproductsforsupplierdetailsaspx"></a>Schritt 3: Auflisten des Lieferanten in`ProductsForSupplierDetails.aspx`

An diesem Punkt die `SupplierListMaster.aspx` Seite sendet Benutzer `ProductsForSupplierDetails.aspx`, übergeben des ausgewählten Lieferanten `SupplierID` in der Abfragezeichenfolge. Das Tutorial der letzte Schritt ist für die Produkte in einer GridView-Ansicht in `ProductsForSupplierDetails.aspx` , deren `SupplierID` entspricht der `SupplierID` in der Abfragezeichenfolge übergeben. Starten erreicht wird, durch das Hinzufügen einer GridView-Ansicht, die `ProductsForSupplierDetails.aspx` Seite, ein neues ObjectDataSource-Steuerelement, das mit dem Namen `ProductsBySupplierDataSource` aufruft, die die `GetProductsBySupplierID(supplierID)` Methode aus der `ProductsBLL` Klasse.


[![Fügen Sie eine neue, mit dem Namen ProductsBySupplierDataSource "ObjectDataSource"](master-detail-filtering-across-two-pages-cs/_static/image21.png)](master-detail-filtering-across-two-pages-cs/_static/image20.png)

**Abbildung 8**: Hinzufügen einer neuen "ObjectDataSource" mit dem Namen `ProductsBySupplierDataSource` ([klicken Sie, um das Bild in voller Größe anzeigen](master-detail-filtering-across-two-pages-cs/_static/image22.png))


[![Wählen Sie die ProductsBLL-Klasse](master-detail-filtering-across-two-pages-cs/_static/image24.png)](master-detail-filtering-across-two-pages-cs/_static/image23.png)

**Abbildung 9**: Wählen Sie die `ProductsBLL` Klasse ([klicken Sie, um das Bild in voller Größe anzeigen](master-detail-filtering-across-two-pages-cs/_static/image25.png))


[![Haben Sie dem Aufrufen der Methode GetProductsBySupplierID(supplierID) ObjectDataSource-Steuerelement](master-detail-filtering-across-two-pages-cs/_static/image27.png)](master-detail-filtering-across-two-pages-cs/_static/image26.png)

**Abbildung 10**: haben Sie die "ObjectDataSource" Aufrufen der `GetProductsBySupplierID(supplierID)` Methode ([klicken Sie, um das Bild in voller Größe anzeigen](master-detail-filtering-across-two-pages-cs/_static/image28.png))


Der letzte Schritt des Assistenten zum Konfigurieren der Datenquelle gefragt werden, geben Sie die Quelle des der `GetProductsBySupplierID(supplierID)` Methode *`supplierID`* Parameter. Um den Querystring-Wert verwenden zu können, legen Sie die Parameter-Quelle auf QueryString aus, und geben Sie den Namen der Querystring-Wert, der im Textfeld für die QueryStringField verwenden (`SupplierID`).


[![Füllen Sie die SupplierID Parameterwert aus dem SupplierID Querystring-Wert](master-detail-filtering-across-two-pages-cs/_static/image30.png)](master-detail-filtering-across-two-pages-cs/_static/image29.png)

**Abbildung 11**: Auffüllen der *`supplierID`* Parameterwert aus dem `SupplierID` Querystring-Wert ([klicken Sie, um das Bild in voller Größe anzeigen](master-detail-filtering-across-two-pages-cs/_static/image31.png))


Das ist alles schon! Abbildung 12 zeigt die `ProductsForSupplierDetails.aspx` Seite besucht haben, indem Sie auf den Link Tokyo Traders aus `SupplierListMaster.aspx`.


[![Die Produkte, die von der Tokio Traders bereitgestellt werden angezeigt.](master-detail-filtering-across-two-pages-cs/_static/image33.png)](master-detail-filtering-across-two-pages-cs/_static/image32.png)

**Abbildung 12**: die Produkte von Tokyo Traders bereitgestellt werden angezeigt ([klicken Sie, um das Bild in voller Größe anzeigen](master-detail-filtering-across-two-pages-cs/_static/image34.png))


## <a name="displaying-supplier-information-inproductsforsupplierdetailsaspx"></a>Anzeigen von Lieferanteninformationen in`ProductsForSupplierDetails.aspx`

Wie in Abbildung 12 gezeigt, die `ProductsForSupplierDetails.aspx` Seite listet die Produkte, die von bereitgestellte der `SupplierID` in der Abfragezeichenfolge angegeben. Ein Benutzer direkt zu dieser Seite gesendet würde jedoch nicht wissen, dass Abbildung 12 Tokio Traders Products angezeigt wird. Zur Behebung des Problems können wir auf dieser Seite auch Lieferanteninformationen anzeigen.

Starten Sie durch das Hinzufügen von einem FormView-Steuerelement über die GridView-Produkte. Erstellen Sie ein neues ObjectDataSource-Steuerelement, das mit dem Namen `SuppliersDataSource` aufruft, die die `SuppliersBLL` Klasse `GetSupplierBySupplierID(supplierID)` Methode.


[![Wählen Sie die SuppliersBLL-Klasse](master-detail-filtering-across-two-pages-cs/_static/image36.png)](master-detail-filtering-across-two-pages-cs/_static/image35.png)

**Abbildung 13**: Wählen Sie die `SuppliersBLL` Klasse ([klicken Sie, um das Bild in voller Größe anzeigen](master-detail-filtering-across-two-pages-cs/_static/image37.png))


[![Haben Sie dem Aufrufen der Methode GetSupplierBySupplierID(supplierID) ObjectDataSource-Steuerelement](master-detail-filtering-across-two-pages-cs/_static/image39.png)](master-detail-filtering-across-two-pages-cs/_static/image38.png)

**Abbildung 14**: haben Sie die "ObjectDataSource" Aufrufen der `GetSupplierBySupplierID(supplierID)` Methode ([klicken Sie, um das Bild in voller Größe anzeigen](master-detail-filtering-across-two-pages-cs/_static/image40.png))


Wie bei der `ProductsBySupplierDataSource`, haben die *`supplierID`* Parameter der Wert zugewiesen. die `SupplierID` Querystring-Wert.


[![Füllen Sie die SupplierID Parameterwert aus dem SupplierID Querystring-Wert](master-detail-filtering-across-two-pages-cs/_static/image42.png)](master-detail-filtering-across-two-pages-cs/_static/image41.png)

**Abbildung 15**: Auffüllen der *`supplierID`* Parameterwert aus dem `SupplierID` Querystring-Wert ([klicken Sie, um das Bild in voller Größe anzeigen](master-detail-filtering-across-two-pages-cs/_static/image43.png))


Beim Binden der FormView-Steuerelement an das "ObjectDataSource" in der Entwurfsansicht Visual Studio erstellt automatisch der FormView `ItemTemplate`, `InsertItemTemplate`, und `EditItemTemplate` mit der Bezeichnung und TextBox Web für die einzelnen Datenfelder vom den "ObjectDataSource". Da wir nur die Lieferanten Informationen gerne entfernen anzeigen möchten die `InsertItemTemplate` und `EditItemTemplate`. ItemTemplate bearbeiten Sie anschließend, um anzuzeigen, dass die Lieferanten Unternehmensname in einer `<h3>` -Element und die Adresse, Ort, Land und Telefonnummer unterhalb des Unternehmens an. Alternativ können Sie manuell der FormView festlegen `DataSourceID` , und erstellen Sie die `ItemTemplate` Markup, wie in der "[Anzeigen von Daten mit dem ObjectDataSource-Steuerelement](../basic-reporting/displaying-data-with-the-objectdatasource-cs.md)" Tutorial.

Nach diesen Änderungen sollte das FormView deklarative Markup etwa wie folgt aussehen:


[!code-aspx[Main](master-detail-filtering-across-two-pages-cs/samples/sample3.aspx)]

Abbildung 16 zeigt einen Screenshot, der die `ProductsForSupplierDetails.aspx` Seite, nachdem die oben beschriebenen Lieferanteninformationen eingefügt wurde.


[![Die Liste der Produkte enthält eine Zusammenfassung zum Lieferanten](master-detail-filtering-across-two-pages-cs/_static/image45.png)](master-detail-filtering-across-two-pages-cs/_static/image44.png)

**Abbildung 16**: die Liste der Produkte enthält eine Zusammenfassung über die Lieferanten ([klicken Sie, um das Bild in voller Größe anzeigen](master-detail-filtering-across-two-pages-cs/_static/image46.png))


## <a name="applying-the-final-touches-for-theproductsforsupplierdetailsaspxui"></a>Anwenden den letzten berührt, für die`ProductsForSupplierDetails.aspx`UI

Zur Verbesserung des Benutzers gibt Erfahrung für diesen Bericht gibt es eine Reihe von Ergänzungen, die wir sollte an die `ProductsForSupplierDetails.aspx` Seite. Derzeit die einzige Möglichkeit, die ein Benutzer kann, wechseln Sie von der `ProductsForSupplierDetails.aspx` Seite wieder an die Liste seiner Lieferanten wird auf ihren Browser die Schaltfläche "zurück" klicken. Fügen Sie ein HyperLink-Steuerelement, um die `ProductsForSupplierDetails.aspx` Seite, die Links zu sichern `SupplierListMaster.aspx`, eine weitere Möglichkeit für den Benutzer wieder der Hauptliste bereitstellen.


[![Fügen Sie ein HyperLink-Steuerelement, um den Benutzer zurück zum SupplierListMaster.aspx nutzen](master-detail-filtering-across-two-pages-cs/_static/image48.png)](master-detail-filtering-across-two-pages-cs/_static/image47.png)

**Abbildung 17**: Fügen Sie ein HyperLink-Steuerelement, um den Benutzer zurück zur nutzen `SupplierListMaster.aspx` ([klicken Sie, um das Bild in voller Größe anzeigen](master-detail-filtering-across-two-pages-cs/_static/image49.png))


Klickt der Benutzer auf den Link Produkte anzeigen, um einen Lieferanten, Produkte, die nicht die `ProductsBySupplierDataSource` zu "ObjectDataSource" `ProductsForSupplierDetails.aspx` keine Ergebnisse zurück. Die GridView gebunden werden, um dem ObjectDataSource-Steuerelement wird nicht in einen leeren Bereich auf der Seite im Browser des Benutzers resultierende Markup gerendert. Mehr für den Benutzer kommunizieren, sind keine Produkte, die den ausgewählten Hersteller zugeordnet können wir festlegen, des GridView `EmptyDataText` Eigenschaft, um die Meldung angezeigt werden sollten, wenn dies der Fall ist. Ich habe diese Eigenschaft auf "Sind keine Produkte, die von diesem Lieferanten bereitgestellte" festgelegt.

Geben Sie in der Standardeinstellung alle Lieferanten in der Employees-Datenbank mindestens ein Produkt. Allerdings für dieses Tutorial ich manuell verändert die `Products` Tabelle, damit der Lieferant Escargots Nouveaux nicht mehr alle Produkte zugeordnet ist. Abbildung 18 zeigt die Seite "Details" für Escargots Nouveaux, nachdem diese Änderung vorgenommen wurde.


[![Benutzer werden darüber informiert, dass der Lieferant keine Produkte bereitstellt](master-detail-filtering-across-two-pages-cs/_static/image51.png)](master-detail-filtering-across-two-pages-cs/_static/image50.png)

**Abbildung 18**: Benutzer werden darüber informiert, dass der Lieferant keine Produkte bereitstellt ([klicken Sie, um das Bild in voller Größe anzeigen](master-detail-filtering-across-two-pages-cs/_static/image52.png))


## <a name="summary"></a>Zusammenfassung

Master/Detail-Berichte können sowohl die Master- und Detaildatensätzen auf einer einzelnen Seite anzeigen, aber auf vielen Websites diese auf zwei Webseiten verteilt. In diesem Tutorial erläutert, wie ein solchen Master/Detail-Berichts implementiert wird, dass die Lieferanten, die in einer GridView-Ansicht auf der Webseite "master" aufgeführt und die zugehörigen Produkte, die auf der Seite "Details" aufgeführt. Jede Zeile der Lieferant in die Masterseite für Web enthalten einen Link zur Detailseite, die der Zeile übergeben `SupplierID` Wert. Solche Verknüpfungen zeilenspezifische können ganz einfach mithilfe von GridView HyperLinkField hinzugefügt werden.

In der Seite Details zum Abrufen dieser Produkte für den angegebenen Lieferanten wurde erreicht durch Aufrufen der `ProductsBLL` Klasse `GetProductsBySupplierID(supplierID)` Methode. Die *`supplierID`* Parameterwert wurde mit deklarativ als Parameterquelle der Abfragezeichenfolge angegeben. Wir haben uns auch an, wie die Lieferanten-Details in der Detailseite, die mit einem FormView-Steuerelement angezeigt.

Unsere [nächsten Tutorial](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs.md) ist der endgültige für Master/Detail-Berichte. Betrachten wir eine Liste der Produkte in einer GridView anzeigen, in dem jede Zeile eine Auswahlschaltfläche aufweist. Durch Klicken auf die auswählen-Schaltfläche werden Details zum entsprechenden Produkt in einem DetailsView-Steuerelement auf derselben Seite angezeigt.

Viel Spaß beim Programmieren!

## <a name="about-the-author"></a>Der Autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor von sieben Büchern zu ASP/ASP.NET und Gründer von [4GuysFromRolla.com](http://www.4guysfromrolla.com), arbeitet mit Microsoft-Web-Technologien seit 1998. Er ist als ein unabhängiger Berater, Schulungsleiter und Autor. Sein neueste Buch wird [*Sams Schulen selbst ASP.NET 2.0 in 24 Stunden*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Er ist unter [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) oder über seinen Blog finden Sie unter [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Besonderen Dank an

Diese tutorialreihe wurde durch viele hilfreiche Reviewer überprüft. Führendes Prüfer für dieses Tutorial ist Hilton Giesenow. Meine zukünftigen MSDN-Artikeln überprüfen möchten? Wenn dies der Fall ist, löschen Sie mir eine Linie an [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Zurück](master-detail-filtering-with-two-dropdownlists-cs.md)
> [Weiter](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs.md)
