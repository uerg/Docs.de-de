---
uid: web-forms/overview/data-access/filtering-scenarios-with-the-datalist-and-repeater/master-detail-filtering-acess-two-pages-datalist-cs
title: "Master/Detail-Filterung über zwei Seiten (c#) | Microsoft Docs"
author: rick-anderson
description: "In diesem Lernprogramm betrachten wir einen Master/Detail-Bericht über zwei Seiten Trennung ein. Auf der Seite 'master' verwenden wir Wiederholungsmodul-Steuerelement, um eine Liste der Categ rendern..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/30/2010
ms.topic: article
ms.assetid: 68b8c023-92fa-4df6-9563-1764e16e4b04
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/filtering-scenarios-with-the-datalist-and-repeater/master-detail-filtering-acess-two-pages-datalist-cs
msc.type: authoredcontent
ms.openlocfilehash: bb86db509ca26dde0c24341dee402e7af4355507
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="masterdetail-filtering-across-two-pages-c"></a>Master/Detail-Filterung über zwei Seiten (c#)
====================
durch [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Beispiel-App herunterladen](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_34_CS.exe) oder [PDF herunterladen](master-detail-filtering-acess-two-pages-datalist-cs/_static/datatutorial34cs1.pdf)

> In diesem Lernprogramm betrachten wir einen Master/Detail-Bericht über zwei Seiten Trennung ein. In "Masterseite" wird ein Wiederholungsmodul-Steuerelement verwendet, einer Liste der Kategorien, die beim geklickt haben, dauert des Benutzers auf der Seite "Details", in denen die Produkte, die der ausgewählten Kategorie gehören zeigt eine zweispaltige DataList gerendert.


## <a name="introduction"></a>Einführung

In der [vorherigen Lernprogramm](master-detail-filtering-with-a-dropdownlist-datalist-cs.md) Filtermenü Vorgehensweise beim Anzeigen von Master/Detail-Berichte in einer einzelnen Webseite mit DropDownLists zum Anzeigen der "master" Datensätze und DataList anzuzeigenden "Details". Weiteres gängiges Muster für Master-/Detail-Berichte verwendet wird, haben die master-Datensätze auf einer Webseite und die Details auf einen anderen. In den früheren [Master/Detail-Filterung über zwei Seiten](../masterdetail/master-detail-filtering-across-two-pages-cs.md) Lernprogramm, die dieses Muster mit GridView zum Anzeigen aller Lieferanten in das System untersucht. Diese GridView enthalten eine HyperLinkField, die als Link zu einer zweiten Seite weiter und übergibt dabei gerendert der `SupplierID` in der Abfragezeichenfolge. Die zweite Seite verwendet eine GridView, um die Produkte, die von den ausgewählten Lieferanten bereitgestellte aufzulisten.

Solche zweiseitige Master/Detail-Berichte können mithilfe von Steuerelementen DataList und Repeater erreicht werden. Der einzige Unterschied ist, dass weder das DataList noch Repeater Unterstützung für das Steuerelement HyperLinkField bereitstellt. Stattdessen müssen wir hinzufügen ein HyperLink-Websteuerelement oder ein HTML-Ankerelement (`<a>`) innerhalb des Steuerelements `ItemTemplate`. Des Hyperlinks `NavigateUrl` Eigenschaft oder der Verankerung `href` Attribut kann dann verwenden deklarative oder programmgesteuerte Ansätze angepasst werden.

In diesem Lernprogramm werden, die die Kategorien in einer Aufzählung auf einer Seite mit einem Wiederholungsmodul-Steuerelement enthält ein Beispiel vorgestellt. Jedes Listenelement enthält Namen und eine Beschreibung der Kategorie mit dem Kategorienamen als Link zu einer zweiten Seite angezeigt. Mit diesem Link wird weiter den Benutzer auf die zweite Seite DataList wird, in denen diese Produkte angezeigt, die der ausgewählten Kategorie angehören.

## <a name="step-1-displaying-the-categories-in-a-bulleted-list"></a>Schritt 1: Anzeigen der Kategorien in einer Liste mit Aufzählungszeichen

Der erste Schritt bei der Erstellung jeder Master/Detail-Bericht ist von der "master" Datensätze anzeigen. Aus diesem Grund ist unsere erste Aufgabe die Kategorien in der "Masterseite" angezeigt. Öffnen der `CategoryListMaster.aspx` auf der Seite der `DataListRepeaterFiltering` Ordner Wiederholungsmodul-Steuerelement hinzufügen und aus dem Smarttag abonnieren, um eine neue ObjectDataSource hinzuzufügen. Die neue ObjectDataSource so konfigurieren, dass Zugriff auf die Daten aus der `CategoriesBLL` Klasse `GetCategories` Methode (siehe Abbildung 1).


[![Konfigurieren der ObjectDataSource zur Verwendung der CategoriesBLL Klasse GetCategories-Methode](master-detail-filtering-acess-two-pages-datalist-cs/_static/image2.png)](master-detail-filtering-acess-two-pages-datalist-cs/_static/image1.png)

**Abbildung 1**: Konfigurieren der ObjectDataSource verwenden die `CategoriesBLL` Klasse `GetCategories` Methode ([klicken Sie hier, um das Bild in voller Größe angezeigt](master-detail-filtering-acess-two-pages-datalist-cs/_static/image3.png))


Als Nächstes definieren Sie die Repeater Vorlagen so, dass jeder Kategoriename und die Beschreibung als ein Element in einer Aufzählung angezeigt. Wir noch nicht kümmern müssen jeder Kategorie Link, um die Seite "Details". Im folgenden gezeigt deklarative Markup für die Repeater und ObjectDataSource:

[!code-aspx[Main](master-detail-filtering-acess-two-pages-datalist-cs/samples/sample1.aspx)]

Bei diesem Markup abgeschlossen ist verwendet werden Sie, können Sie unseren Fortschritt über einen Browser anzeigen. Wie in Abbildung 2 gezeigt, wird Sie als eine Liste mit Aufzählungszeichen mit Namen und eine Beschreibung jeder Kategorie Repeater gerendert.


[![Jede Kategorie wird als ein Aufzählungszeichen Listenelement angezeigt.](master-detail-filtering-acess-two-pages-datalist-cs/_static/image5.png)](master-detail-filtering-acess-two-pages-datalist-cs/_static/image4.png)

**Abbildung 2**: jede Kategorie wird als ein Aufzählungszeichen Listenelement angezeigt ([klicken Sie hier, um das Bild in voller Größe angezeigt](master-detail-filtering-acess-two-pages-datalist-cs/_static/image6.png))


## <a name="step-2-turning-the-category-name-into-a-link-to-the-details-page"></a>Schritt 2: Aktivieren den Kategorienamen in einen Link auf der Seite "Details"

Damit wird einen Benutzer die Informationen "Details" nach einer bestimmten Kategorie anzuzeigen, müssen wir einen Link auf jede Aufzählung Element, das beim geklickt haben, dauert des Benutzers auf die zweite Seite hinzufügen (`ProductsForCategoryDetails.aspx`). Dieser zweite Seite zeigt dann die Produkte für die ausgewählte Kategorie, die mit einem DataList. Um die Kategorie zu ermitteln, deren Link geklickt wurde, müssen wir der angeklickte Kategorie übergeben `CategoryID` auf der zweiten Seite über einen Mechanismus. Die einfachste und einfachste Möglichkeit zum Übertragen von skalarer Daten von einer Seite zu einem anderen ist über die Abfragezeichenfolge der die Option, die wir in diesem Lernprogramm verwenden. Insbesondere die `ProductsForCategoryDetails.aspx` Seite erwarten, dass das ausgewählte  *`categoryID`*  Wert eine Querystring-Feld mit dem Namen weitergereicht werden `CategoryID`. Um beispielsweise die Produkte für die Kategorie "Getränke" Anzeigen der verfügt über eine `CategoryID` 1, würde ein Benutzer besuchen Sie `ProductsForCategoryDetails.aspx?CategoryID=1`.

So erstellen Sie einen Link für jedes Element der Liste mit Aufzählungszeichen aus im Wiederholungsmodul wir entweder ein HyperLink-Websteuerelement oder ein HTML-Ankerelement müssen (`<a>`) auf die `ItemTemplate`. In Szenarien, in denen der Link angezeigt, wenn für jede Zeile, die beide Vorgehensweisen reicht aus. Für Repeater möchte ich, mithilfe des Ankerelements. Um die Ankerelement zu verwenden, aktualisieren Sie die Repeater ItemTemplate:

[!code-aspx[Main](master-detail-filtering-acess-two-pages-datalist-cs/samples/sample2.aspx)]

Beachten Sie, dass die `CategoryID` können direkt in des Ankerelements eingegeben werden `href` -Attribut; jedoch Seien Sie daher sicher, dass die Begrenzung der `href` Attributwert Apostrophe (und Anmerkung Anführungszeichen) seit der `Eval` Methode innerhalb der `href` Attribut begrenzt die Zeichenfolge (`"CategoryID"`) in Anführungszeichen ein. Alternativ kann ein Link-Websteuerelement stattdessen verwendet werden:

[!code-aspx[Main](master-detail-filtering-acess-two-pages-datalist-cs/samples/sample3.aspx)]

Hinweis wie die statische Teil der URL – `ProductsForCategoryDetails.aspx?CategoryID` – angefügt ist, auf das Ergebnis des `Eval("CategoryID")` direkt in die Verkettung von Zeichenfolgen mit Databinding-Syntax.

Ein Vorteil der Verwendung von des Linksteuerelements ist, dass es programmgesteuert kann, aus der Repeater zugegriffen werden `ItemDataBound` Ereignishandler, d. h. bei Bedarf. Beispielsweise empfiehlt es sich um die Kategorienamen als Text statt als einen Link für die Kategorien mit keine zugehörigen Produkte anzuzeigen. Eine solche Überprüfung kann programmgesteuert ausgeführt werden, der `ItemDataBound` Ereignishandler; bei Kategorien ohne zugeordnete Produkte, den Link des `NavigateUrl` Eigenschaftensatz konnte auf eine leere Zeichenfolge, wodurch diese bestimmten Kategorienamen die Darstellung als nur-Text (anstatt als Link). Verweisen auf die [Formatierung des DataList und Repeater basierend auf Daten](../displaying-data-with-the-datalist-and-repeater/formatting-the-datalist-and-repeater-based-upon-data-cs.md) -Lernprogramm für Weitere Informationen zum Formatieren der DataList und des Wiederholungsmoduls Inhalt auf der Grundlage von programmgesteuerte Logik über die `ItemDataBound` -Ereignishandler.

Wenn Sie entlang vorgehen, können Sie Ihrer Seite Ankerelement oder Link-Steuerelement Ansatz verwenden. Unabhängig vom Ansatz, wenn die Seite über einen Browser anzeigen jedes Kategorienamen soll, als Link zum gerendert werden `ProductsForCategoryDetails.aspx`, und übergeben Sie die anwendbare `CategoryID` Wert (siehe Abbildung 3).


[![Die Kategorienamen jetzt Verknüpfen mit ProductsForCategoryDetails.aspx.](master-detail-filtering-acess-two-pages-datalist-cs/_static/image8.png)](master-detail-filtering-acess-two-pages-datalist-cs/_static/image7.png)

**Abbildung 3**: die Namen jetzt Kategorielink auf `ProductsForCategoryDetails.aspx` ([klicken Sie hier, um das Bild in voller Größe angezeigt](master-detail-filtering-acess-two-pages-datalist-cs/_static/image9.png))


## <a name="step-3-listing-the-products-that-belong-to-the-selected-category"></a>Schritt 3: Auflisten der Produkte, die der ausgewählten Kategorie angehören

Mit der `CategoryListMaster.aspx` Seite vollständige wir jetzt können Sie unsere Aufmerksamkeit für die Implementierung der Seite "Details" aktivieren `ProductsForCategoryDetails.aspx`. Öffnen Sie die Seite, ein DataList aus der Toolbox in den Designer ziehen und Festlegen der `ID` Eigenschaft `ProductsInCategory`. Wählen Sie anschließend aus der DataList smart Tag auf der Seite, benennen sie eine neue ObjectDataSource hinzufügen `ProductsInCategoryDataSource`. Konfigurieren, er ruft die `ProductsBLL` Klasse `GetProductsByCategoryID(categoryID)` Methode, die Menge der Dropdown-, in die INSERT-, Update- und DELETE-Registerkarten, um (keine Listen).


[![Konfigurieren der ObjectDataSource zur Verwendung der ProductsBLL Klasse GetProductsByCategoryID(categoryID)-Methode](master-detail-filtering-acess-two-pages-datalist-cs/_static/image11.png)](master-detail-filtering-acess-two-pages-datalist-cs/_static/image10.png)

**Abbildung 4**: Konfigurieren der ObjectDataSource verwenden die `ProductsBLL` Klasse `GetProductsByCategoryID(categoryID)` Methode ([klicken Sie hier, um das Bild in voller Größe angezeigt](master-detail-filtering-acess-two-pages-datalist-cs/_static/image12.png))


Da die `GetProductsByCategoryID(categoryID)` Methode akzeptiert einen Eingabeparameter (*`categoryID`*), der Datenquelle auswählen-Assistent bietet uns die Gelegenheit zur Quelle des Parameters angeben. Legen Sie die Parameterquelle mit der QueryStringField QueryString `CategoryID`.


[![Verwenden Sie die CategoryID Querystring-Feld als Quelle für den Parameter](master-detail-filtering-acess-two-pages-datalist-cs/_static/image14.png)](master-detail-filtering-acess-two-pages-datalist-cs/_static/image13.png)

**Abbildung 5**: Verwenden Sie das Querystring-Field `CategoryID` als Quelle für den Parameter ([klicken Sie hier, um das Bild in voller Größe angezeigt](master-detail-filtering-acess-two-pages-datalist-cs/_static/image15.png))


Wie wir im vorherigen Lernprogrammen gesehen haben, nach Abschluss des Assistenten Datenquelle auswählen, erstellt Visual Studio automatisch eine `ItemTemplate` für DataList, die jede Datenfeldnamen und Wert enthält. Ersetzen Sie diese Vorlage mit einem, das nur der Product Name, Lieferanten und Preis auflistet. Legen Sie außerdem die DataList `RepeatColumns` -Eigenschaft auf 2. Nach der Änderung sollte Ihre DataList und der ObjectDataSource deklarative Markup etwa wie folgt aussehen:

[!code-aspx[Main](master-detail-filtering-acess-two-pages-datalist-cs/samples/sample4.aspx)]

Starten Sie zum Anzeigen dieser Seite in Aktion über die `CategoryListMaster.aspx` Seite; danach klicken Sie auf einen Link in der Aufzählung der Kategorien auf. Auf diese Weise gelangen Sie zu `ProductsForCategoryDetails.aspx`, und übergeben Sie entlang der `CategoryID` über die Abfragezeichenfolge. Die `ProductsInCategoryDataSource` ObjectDataSource in `ProductsForCategoryDetails.aspx` klicken Sie dann nur die Produkte für die angegebene Kategorie abrufen und in DataList, die beiden Produkte pro Zeile rendert anzuzeigen. Abbildung 6 zeigt einen Screenshot der `ProductsForCategoryDetails.aspx` beim Anzeigen der Getränke.


[![Die Getränke werden angezeigt, zwei pro Zeile](master-detail-filtering-acess-two-pages-datalist-cs/_static/image17.png)](master-detail-filtering-acess-two-pages-datalist-cs/_static/image16.png)

**Abbildung 6**: die Getränke werden angezeigt, zwei pro Zeile ([klicken Sie hier, um das Bild in voller Größe angezeigt](master-detail-filtering-acess-two-pages-datalist-cs/_static/image18.png))


## <a name="step-4-displaying-category-information-on-productsforcategorydetailsaspx"></a>Schritt 4: Anzeigen von Informationen zu Auftragskategorien auf ProductsForCategoryDetails.aspx

Wenn ein Benutzer klickt auf eine Kategorie in `CategoryListMaster.aspx`, werden erstellt, um `ProductsForCategoryDetails.aspx` und die Produkte, die der ausgewählten Kategorie angezeigt. Allerdings in `ProductsForCategoryDetails.aspx` stehen keine visuellen Hinweise auf welche Kategorie ausgewählt wurde. Ein Benutzer vorgesehen, Sie Getränke jedoch versehentlich geklickt wurde "Gewürze", klicken Sie auf hat keine Möglichkeit, ihre Fehler bemerken, sobald sie erreichen `ProductsForCategoryDetails.aspx`. Um dieses potenzielle Problem zu beheben, können wir werden Informationen zu der ausgewählten Kategorie angezeigt, dessen Name und Beschreibung – am oberen Rand der `ProductsForCategoryDetails.aspx` Seite.

Um dies zu erreichen, fügen Sie einen FormView oben im Wiederholungsmodul-Steuerelement `ProductsForCategoryDetails.aspx`. Fügen Sie eine neue ObjectDataSource auf der Seite aus der FormView-Smarttag mit dem Namen `CategoryDataSource` und konfigurieren es für das Verwenden der `CategoriesBLL` Klasse `GetCategoryByCategoryID(categoryID)` Methode.


[![Zugriff auf Informationen über die Kategorie über die CategoriesBLL Klasse GetCategoryByCategoryID(categoryID)-Methode](master-detail-filtering-acess-two-pages-datalist-cs/_static/image20.png)](master-detail-filtering-acess-two-pages-datalist-cs/_static/image19.png)

**Abbildung 7**: Zugriff auf Informationen über die Kategorie über die `CategoriesBLL` Klasse `GetCategoryByCategoryID(categoryID)` Methode ([klicken Sie hier, um das Bild in voller Größe angezeigt](master-detail-filtering-acess-two-pages-datalist-cs/_static/image21.png))


Wie bei der `ProductsInCategoryDataSource` ObjectDataSource hinzugefügt, in Schritt 3 die `CategoryDataSource`des Konfigurieren von Datenquellen-Assistent fordert uns als Quelle für die `GetCategoryByCategoryID(categoryID)` Methode der Eingabeparameter. Verwenden Sie exakten denselben Einstellungen als vorher, indem Sie die Parameterquelle QueryString und der Wert QueryStringField `CategoryID` (siehe Abbildung 5).

Nach Abschluss des Assistenten, erstellt Visual Studio automatisch ein `ItemTemplate`, `EditItemTemplate`, und `InsertItemTemplate` für FormView. Da wir eine nur-Lese Schnittstelle bereitstellen, können Sie entfernen die `EditItemTemplate` und `InsertItemTemplate`. Darüber hinaus können Sie die FormView anpassen `ItemTemplate`. Nach dem Entfernen überflüssigen Vorlagen und Anpassen von ItemTemplate, sollte Ihre FormView und der ObjectDataSource deklarative Markup etwa wie folgt aussehen:

[!code-aspx[Main](master-detail-filtering-acess-two-pages-datalist-cs/samples/sample5.aspx)]

Abbildung 8 zeigt einen Screenshot, wenn Sie diese Seite über einen Browser anzeigen.

> [!NOTE]
> Zusätzlich zu den FormView haben ich auch ein Linksteuerelement oben, mit denen den Benutzer zurück zur Liste der Kategorien gelangen FormView hinzugefügt (`CategoryListMaster.aspx`). Gerne auf diesen Link an anderer Stelle platzieren, oder er vollständig weggelassen werden soll.


[![Informationen zu Auftragskategorien ist jetzt am oberen Rand der Seite angezeigt.](master-detail-filtering-acess-two-pages-datalist-cs/_static/image23.png)](master-detail-filtering-acess-two-pages-datalist-cs/_static/image22.png)

**Abbildung 8**: Kategorieinformationen ist jetzt am oberen Rand der Seite angezeigt ([klicken Sie hier, um das Bild in voller Größe angezeigt](master-detail-filtering-acess-two-pages-datalist-cs/_static/image24.png))


## <a name="step-5-displaying-a-message-if-no-products-belong-to-the-selected-category"></a>Schritt 5: Anzeigen einer Meldung auf, wenn keine Produkte zu der ausgewählten Kategorie gehören

Die `CategoryListMaster.aspx` Seite listet alle Kategorien im System, unabhängig davon, ob vorhandene verknüpfte-Produkten. Wenn ein Benutzer auf eine Kategorie mit keine zugehörigen Produkte, die DataList in klickt `ProductsForCategoryDetails.aspx` werden nicht gerendert werden, da dessen Datenquelle keine Elemente. Wie wir im letzten Lernprogramme gesehen haben, enthält die GridView ein `EmptyDataText` -Eigenschaft, die zum Angeben einer SMS an angezeigt, wenn keine Datensätze, in der Datenquelle vorliegen verwendet werden kann. Leider hat weder DataList noch Repeater eine solche Eigenschaft.

Um eine Meldung, die den Benutzer darüber informiert, dass es sind keine übereinstimmenden Produkte für die ausgewählte Kategorie, müssen wir hinzufügen angezeigt, eine Bezeichnung-Steuerelement auf der Seite ", dessen `Text` -Eigenschaft wird die Meldung angezeigt, es keine übereinstimmenden Produkte sind zugewiesen. Wir müssen dann programmgesteuert festlegen seiner `Visible` -Eigenschaft basierend auf, und zwar unabhängig davon, ob das DataList Elemente enthält.

Um dies zu erreichen, starten Sie durch Hinzufügen einer Bezeichnung unterhalb der DataList. Legen Sie seine `ID` Eigenschaft, um `NoProductsMessage` und seine `Text` -Eigenschaft auf "Sind keine Produkte für die ausgewählte Kategorie..." Als Nächstes müssen wir diese Bezeichnung programmgesteuert festgelegt `Visible` Eigenschaft fest, ob alle Daten an gebunden wurde anhand der `ProductsInCategory` DataList. Diese Zuweisung muss vorgenommen werden, nachdem die Daten an das DataList gebunden wurde. Es konnte für die GridView, DetailsView und FormView, einen Ereignishandler für des Steuerelements erstellt `DataBound` Ereignis, das ausgelöst wird, nach dem Datenbindung abgeschlossen wurde. Allerdings weder DataList noch Repeater verfügt über eine `DataBound` Ereignis verfügbar.

Bei diesem speziellen Beispiel können wir der Bezeichnung zuweisen `Visible` Eigenschaft in der `Page_Load` -Ereignishandler, da die Daten vor der Seite DataList zugewiesen wurden werden `Load` Ereignis. Allerdings würde dieser Ansatz im allgemeinen Fall funktioniert nicht wie die Daten aus der ObjectDataSource zu einem späteren Zeitpunkt im Lebenszyklus der Seite DataList gebunden werden können. Z. B. wenn die angezeigten Daten auf den Wert in ein anderes Steuerelement basiert, z. B. ist, wenn einen Master/Detail-Bericht mithilfe einer DropDownList zum Speichern der "master" Datensätze anzeigen, die Daten möglicherweise nicht erneut gebunden an das Webserver-Steuerelement bis der `PreRender` Stufe der Lebenszyklus der Seite.

Eine Lösung, die in allen Fällen funktioniert zugewiesen ist die `Visible` Eigenschaft `False` in das DataList `ItemDataBound` (oder `ItemCreated`) Ereignishandler beim Binden der Elementtyp der `Item` oder `AlternatingItem`. In einem solchen Fall wissen wir, die es ist mindestens ein Data Element in der Datenquelle und aus diesem Grund können Ausblenden der `NoProductsMessage` Bezeichnung. Zusätzlich zu diesem Ereignishandler benötigen wir auch einen Ereignishandler für das DataList `DataBinding` Ereignis, in dem der Bezeichnung initialisiert `Visible` Eigenschaft `True`. Seit der `DataBinding` Ereignis wird ausgelöst, bevor die `ItemDataBound` Ereignisse, die Bezeichnung des `Visible` wird anfänglich-Eigenschaftensatz auf `True`; alle Datenelemente vorhanden sind, wenn es wird jedoch festgelegt werden, um `False`. Der folgende Code implementiert diese Logik:

[!code-csharp[Main](master-detail-filtering-acess-two-pages-datalist-cs/samples/sample6.cs)]

Alle Kategorien in der Northwind-Datenbank sind mit einem oder mehreren Produkten verknüpft. Um diese Funktion zu testen, ich haben manuell angepasst die Northwind-Datenbank für dieses Lernprogramm Neuzuweisen aller Produkte, die der Kategorie erzeugen zugeordnet (`CategoryID` = 7) zu "Seafood" (`CategoryID` = 8). Dies geschieht im Server-Explorer durch neue Abfrage auswählen und mithilfe des folgenden `UPDATE` Anweisung:

[!code-sql[Main](master-detail-filtering-acess-two-pages-datalist-cs/samples/sample7.sql)]

Nach der Aktualisierung der Datenbank entsprechend, zurückgeben, um die `CategoryListMaster.aspx` Seite, und klicken Sie auf den Link erstellt werden sollen. Da es sich nicht mehr alle Produkte aus der Kategorie erstellen, gehören, sehen Sie die "Sind keine Produkte für die ausgewählte Kategorie..." Meldung, wie in Abbildung 9 gezeigt.


[![Wenn es keine Produkte gehören zur Kategorie ausgewählt sind, wird eine Meldung angezeigt.](master-detail-filtering-acess-two-pages-datalist-cs/_static/image26.png)](master-detail-filtering-acess-two-pages-datalist-cs/_static/image25.png)

**Abbildung 9**: eine Nachricht wird angezeigt, wenn es keine Produkte gehören zur Kategorie ausgewählt sind ([klicken Sie hier, um das Bild in voller Größe angezeigt](master-detail-filtering-acess-two-pages-datalist-cs/_static/image27.png))


## <a name="summary"></a>Zusammenfassung

Während der Master und die Detaildatensätze auf einer einzelnen Seite Master/Detail-Berichte angezeigt werden können, werden auf vielen Websites sie über zwei Webseiten ausgelagert. In diesem Lernprogramm erläutert wir, wie solche einen Master/Detail-Bericht implementiert, da die Kategorien aufgeführt, die in einer Aufzählung einen Repeater in der "master" Webseite und die zugehörigen Produkte aufgeführt, die auf der Seite "Details". Jedes Listenelement in der master-Webseite enthalten einen Link zu der Detailseite, die der Zeile übergeben `CategoryID` Wert.

In der Seite "Details" Abrufen von dieser Produkte für den angegebenen Lieferanten wurde durchgeführt, über die `ProductsBLL` Klasse `GetProductsByCategoryID(categoryID)` Methode. Die  *`categoryID`*  Parameterwert angegeben wurde, deklarativ mithilfe der `CategoryID` Querystring-Wert als Parameterquelle für. Wir haben uns auch wie Kategoriedetails in der Seite Details zum Verwenden eines FormView angezeigt werden und wie eine Meldung angezeigt, wenn es keine Produkte wurden, die der ausgewählten Kategorie gehören.

Viel Spaß beim Programmieren!

## <a name="about-the-author"></a>Informationen zum Autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor von sieben ASP/ASP.NET-Büchern und Gründer von [4GuysFromRolla.com](http://www.4guysfromrolla.com), Microsoft Web-Technologien seit 1998 arbeitet. Scott fungiert als ein unabhängiger Berater, Trainer und Writer. Sein neueste Buch wird [ *Sams Schulen selbst ASP.NET 2.0 in 24 Stunden*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Er die erreicht werden kann, zur [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) oder über seinen Blog die finden Sie unter [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Besonderen Dank an...

Diese Reihe von Lernprogrammen wurde durch viele nützliche Bearbeiter überprüft. Lead Prüfer für dieses Lernprogramm wurden Zack Jones und Liz Shulok. Meine bevorstehende MSDN-Artikel Überprüfen von Interesse? Wenn dies der Fall ist, löschen Sie mich zeilenweise [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[Zurück](master-detail-filtering-with-a-dropdownlist-datalist-cs.md)
[Weiter](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs.md)
