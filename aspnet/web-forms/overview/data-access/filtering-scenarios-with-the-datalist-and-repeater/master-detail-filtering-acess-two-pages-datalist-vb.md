---
uid: web-forms/overview/data-access/filtering-scenarios-with-the-datalist-and-repeater/master-detail-filtering-acess-two-pages-datalist-vb
title: Master/Detail-Filtern über zwei Seiten (VB) | Microsoft-Dokumentation
author: rick-anderson
description: In diesem Tutorial untersuchen wir wie Sie über zwei Seiten ein Master-/Detail-Berichts zu trennen. Auf der Seite "master" verwenden wir ein Repeater-Steuerelement zum Rendern einer Liste von Categ...
ms.author: aspnetcontent
ms.date: 10/30/2010
ms.assetid: f1a1be2c-6fd9-4a09-916e-aa1b98d5cf17
msc.legacyurl: /web-forms/overview/data-access/filtering-scenarios-with-the-datalist-and-repeater/master-detail-filtering-acess-two-pages-datalist-vb
msc.type: authoredcontent
ms.openlocfilehash: 5cfbd685344bdd223f8d07f8bad5a54b63735839
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2018
ms.locfileid: "37813688"
---
<a name="masterdetail-filtering-across-two-pages-vb"></a>Master/Detail-Filtern über zwei Seiten (VB)
====================
durch [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Beispiel-App herunter](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_34_VB.exe) oder [PDF-Datei herunterladen](master-detail-filtering-acess-two-pages-datalist-vb/_static/datatutorial34vb1.pdf)

> In diesem Tutorial untersuchen wir wie Sie über zwei Seiten ein Master-/Detail-Berichts zu trennen. Auf der Seite "master" verwenden wir ein Repeater-Steuerelement, um einer Liste der Kategorien, die gerendert werden, wenn geklickt wird, gelangt der Benutzer auf der Seite "Details" zeigt, einem DataList-Steuerelement zwei Spalten, in denen dieser Produkte, die der ausgewählten Kategorie gehören.


## <a name="introduction"></a>Einführung

In der [Anzeigen von Filtern von Master-/Detailberichten über zwei Seiten](../masterdetail/master-detail-filtering-across-two-pages-vb.md) Tutorial untersuchten wir dieses Muster unter Verwendung einer GridView-Ansicht zur Anzeige aller Lieferanten im System. Diese GridView enthalten eine HyperLinkField, das gerendert als Link zu einer zweiten Seite, und übergeben die `SupplierID` in der Abfragezeichenfolge. Die zweite Seite verwendet einer GridView-Ansicht, um die Produkte, die von den ausgewählten Lieferanten bereitgestellte aufzulisten.

Diese zwei Seiten Master/Detail-Berichte können mit Steuerelementen DataList- oder Wiederholungssteuerelement erreicht werden. Der einzige Unterschied ist, dass weder DataList-Steuerelement noch der Repeater das HyperLinkField-Steuerelement unterstützt. Stattdessen müssen wir hinzufügen ein HyperLink-Steuerelement oder ein HTML-Ankerelement (`<a>`) innerhalb des Steuerelements `ItemTemplate`. Des Links des `NavigateUrl` Eigenschaft oder des Ankers `href` Attribut kann dann mithilfe von deklarativen oder eine programmgesteuerte Methoden angepasst werden.

In diesem Tutorial werden wir ein Beispiel für untersuchen, die in einer Aufzählung auf einer Seite ein Repeater-Steuerelement mit Kategorien aufgelistet. Jedes Listenelement enthält Namen und eine Beschreibung der Kategorie mit dem Kategorienamen, die als Link zu einer zweiten Seite angezeigt. Durch Klicken auf diesen Link wird weiter den Benutzer auf der zweiten Seite, einem DataList-Steuerelement wird, in dem diese Produkte angezeigt, die der ausgewählten Kategorie gehören.

## <a name="step-1-displaying-the-categories-in-a-bulleted-list"></a>Schritt 1: Anzeigen der Kategorien in einer Liste mit Aufzählungszeichen

Der erste Schritt beim Erstellen Master/Detail-Berichts wird zunächst die "master" Datensätze anzeigen. Daher ist unsere erste Aufgabe, um die Kategorien auf der Seite "master" anzuzeigen. Öffnen der `CategoryListMaster.aspx` auf der Seite die `DataListRepeaterFiltering` Ordner hinzufügen ein Repeater-Steuerelement und, aus dem Smarttag, zu entscheiden, um eine neue "ObjectDataSource" hinzuzufügen. Die neue "ObjectDataSource" so konfigurieren, dass sie die Daten aus der greift auf die `CategoriesBLL` Klasse `GetCategories` Methode (siehe Abbildung 1).


[![Konfigurieren von dem ObjectDataSource-Steuerelement zur Verwendung der Klasse CategoriesBLL GetCategories-Methode](master-detail-filtering-acess-two-pages-datalist-vb/_static/image2.png)](master-detail-filtering-acess-two-pages-datalist-vb/_static/image1.png)

**Abbildung 1**: dem ObjectDataSource-Steuerelement zu verwendenden konfigurieren die `CategoriesBLL` Klasse `GetCategories` Methode ([klicken Sie, um das Bild in voller Größe anzeigen](master-detail-filtering-acess-two-pages-datalist-vb/_static/image3.png))


Als Nächstes definieren des Repeaters Vorlagen so, dass jeder Name und die Beschreibung als ein Element in einer Aufzählung angezeigt. Lassen Sie uns noch kümmern müssen jeder Kategorie Link zur Detailseite. Im folgenden finden deklarative Markup für das Repeater- und das ObjectDataSource-Steuerelement:

[!code-aspx[Main](master-detail-filtering-acess-two-pages-datalist-vb/samples/sample1.aspx)]

Können Sie mit diesem Markup ist abgeschlossen unseren Fortschritt über einen Browser anzeigen. Wie in Abbildung 2 gezeigt, wird Sie als eine Liste mit Aufzählungszeichen mit Namen und eine Beschreibung der einzelnen Kategorien des Repeaters gerendert.


[![Jede Kategorie wird als eine Liste mit Aufzählungszeichen Listenelement angezeigt.](master-detail-filtering-acess-two-pages-datalist-vb/_static/image5.png)](master-detail-filtering-acess-two-pages-datalist-vb/_static/image4.png)

**Abbildung 2**: jede Kategorie als eine Liste mit Aufzählungszeichen Listenelement angezeigt wird ([klicken Sie, um das Bild in voller Größe anzeigen](master-detail-filtering-acess-two-pages-datalist-vb/_static/image6.png))


## <a name="step-2-turning-the-category-name-into-a-link-to-the-details-page"></a>Schritt 2: Aktivieren den Namen der Kategorie in einen Link zur Detailseite

Um einem Benutzer zum Anzeigen der Informationen "Details" nach einer bestimmten Kategorie zu ermöglichen, müssen wir einen Link zu jeder Aufzählung Element, wenn auf Sie geklickt wird, gelangt der Benutzer auf die zweite Seite hinzufügen (`ProductsForCategoryDetails.aspx`). Dieser zweite Seite zeigt dann die Produkte für die ausgewählte Kategorie, die mit einem DataList-Steuerelement. Um die Kategorie bestimmt, deren Verknüpfung geklickt wurde, müssen wir der angeklickten Kategorie übergeben `CategoryID` auf der zweiten Seite über einen Mechanismus. Die einfachste und direkteste Möglichkeit zum Übertragen von skalarer Daten von einer Seite in ein anderes ist über die Abfragezeichenfolge, die Option, die wir in diesem Tutorial verwenden werden. Insbesondere die `ProductsForCategoryDetails.aspx` Seite erwarten, dass Sie den ausgewählten *`categoryID`* Wert, der über eine Querystring-Feld, das mit dem Namen übergeben werden `CategoryID`. Beispielsweise, um die Produkte für die Kategorie "Getränke" Anzeigen der verfügt über eine `CategoryID` von 1, würde ein Benutzer besuchen Sie `ProductsForCategoryDetails.aspx?CategoryID=1`.

So erstellen Sie einen Link für jedes Element der Liste mit Aufzählungszeichen im Wiederholungsmodul wir entweder ein HyperLink-Steuerelement oder ein HTML-Ankerelement müssen (`<a>`) auf die `ItemTemplate`. In Szenarien, in denen der Link angezeigt, für jede Zeile, die beide Vorgehensweisen ist ausreichend. Für Repeater bevorzuge ich an, mit dem Anchor-Element. Um das Anchor-Element zu verwenden, Aktualisieren des Repeaters ItemTemplate:

[!code-aspx[Main](master-detail-filtering-acess-two-pages-datalist-vb/samples/sample2.aspx)]

Beachten Sie, dass die `CategoryID` können direkt in des Anchor-Elements eingefügt werden `href` -Attribut, führen Sie also sicher sein trennen die `href` Attributwert mit Apostrophe (und beachten Sie, Anführungszeichen einschließen), da die `Eval` Methode in der `href` Attribut begrenzt die Zeichenfolge (`"CategoryID"`) in Anführungszeichen ein. Alternativ kann stattdessen ein HyperLink-Steuerelement verwendet werden:

[!code-aspx[Main](master-detail-filtering-acess-two-pages-datalist-vb/samples/sample3.aspx)]

Hinweis wie der statische Teil der URL – `ProductsForCategoryDetails.aspx?CategoryID` – angefügt ist, auf das Ergebnis des `Eval("CategoryID")` direkt in die Databinding-Syntax, die mithilfe der Verkettung von Zeichenfolgen.

Ein Vorteil der Verwendung des Linksteuerelements ist, dass sie programmgesteuert kann, aus der Repeater zugegriffen werden `ItemDataBound` -Ereignishandler bei Bedarf. Beispielsweise empfiehlt es sich um den Namen der Kategorie als Text und nicht als Link für die Kategorien mit keine zugehörigen Produkte anzuzeigen. Eine solche Überprüfung kann programmgesteuert ausgeführt werden, der `ItemDataBound` Ereignishandler; für Kategorien ohne verknüpfte Produkte, den Link des `NavigateUrl` Eigenschaft kann festgelegt werden, auf eine leere Zeichenfolge, die in dieser bestimmten Kategorienamen Ergebnis die Darstellung als nur-Text (anstatt als Link). Verweisen zurück auf die [Formatieren des DataList- und Wiederholungssteuerelement basierend auf Daten](../displaying-data-with-the-datalist-and-repeater/formatting-the-datalist-and-repeater-based-upon-data-vb.md) -Tutorial für Weitere Informationen zum Formatieren des DataList- und Wiederholungssteuerelements des Inhalts auf der Grundlage von programmgesteuerte Logik über die `ItemDataBound` -Ereignishandler.

Wenn Sie befolgt werden, können Sie das Ankerelement oder HyperLink-Steuerelement-Ansatz auf der Seite verwenden. Unabhängig vom Ansatz, wenn Sie die Seite über einen Browser anzeigen jeder Kategoriename soll, als Link zum gerendert werden `ProductsForCategoryDetails.aspx`, und übergeben Sie in der jeweiligen `CategoryID` Wert (siehe Abbildung 3).


[![Die Kategorienamen jetzt Verknüpfen mit ProductsForCategoryDetails.aspx.](master-detail-filtering-acess-two-pages-datalist-vb/_static/image8.png)](master-detail-filtering-acess-two-pages-datalist-vb/_static/image7.png)

**Abbildung 3**: die Kategorie Namen jetzt Link zur `ProductsForCategoryDetails.aspx` ([klicken Sie, um das Bild in voller Größe anzeigen](master-detail-filtering-acess-two-pages-datalist-vb/_static/image9.png))


## <a name="step-3-listing-the-products-that-belong-to-the-selected-category"></a>Schritt 3: Auflisten der Produkte, die der ausgewählten Kategorie gehören.

Mit der `CategoryListMaster.aspx` Seite abgeschlossen, wir können unsere Aufmerksamkeit für die Implementierung der Seite "Details" zu aktivieren `ProductsForCategoryDetails.aspx`. Öffnen Sie diese Seite, einem DataList-Steuerelement aus der Toolbox in den Designer ziehen und legen Sie dessen `ID` Eigenschaft `ProductsInCategory`. Wählen Sie dann aus der DataList-Steuerelement-Smarttag zum Hinzufügen einer neuen "ObjectDataSource" auf der Seite, und nennen Sie es `ProductsInCategoryDataSource`. Konfigurieren sie so, dass sie ruft die `ProductsBLL` -Klasse `GetProductsByCategoryID(categoryID)` Methode, die Gruppe, die die Dropdownlisten auf den Registerkarten INSERT-, Update- und DELETE auf (keine).


[![Konfigurieren von dem ObjectDataSource-Steuerelement zur Verwendung der Klasse ProductsBLL GetProductsByCategoryID(categoryID)-Methode](master-detail-filtering-acess-two-pages-datalist-vb/_static/image11.png)](master-detail-filtering-acess-two-pages-datalist-vb/_static/image10.png)

**Abbildung 4**: dem ObjectDataSource-Steuerelement zu verwendenden konfigurieren die `ProductsBLL` Klasse `GetProductsByCategoryID(categoryID)` Methode ([klicken Sie, um das Bild in voller Größe anzeigen](master-detail-filtering-acess-two-pages-datalist-vb/_static/image12.png))


Da die `GetProductsByCategoryID(categoryID)` -Methode akzeptiert einen Eingabeparameter (*`categoryID`*), der Datenquelle auswählen-Assistent bietet uns die Gelegenheit zum Angeben des Parameters-Quelle. Legen Sie die Parameterquelle mit der QueryStringField QueryString `CategoryID`.


[![Verwenden Sie die CategoryID Querystring-Feld als Quelle für den Parameter des](master-detail-filtering-acess-two-pages-datalist-vb/_static/image14.png)](master-detail-filtering-acess-two-pages-datalist-vb/_static/image13.png)

**Abbildung 5**: Verwenden Sie das Querystring-Field `CategoryID` als Quelle für des Parameters ([klicken Sie, um das Bild in voller Größe anzeigen](master-detail-filtering-acess-two-pages-datalist-vb/_static/image15.png))


Wie wir in vorherigen Tutorials gesehen haben, nach Abschluss des Assistenten Datenquelle auswählen, erstellt Visual Studio automatisch eine `ItemTemplate` für DataList-Steuerelement, das jede Data-Feldname und Wert enthält. Ersetzen Sie diese Vorlage durch eine, die nur des produktanforderungen Name, Lieferanten und Preis auflistet. Legen Sie außerdem die DataList-Steuerelement `RepeatColumns` Eigenschaft auf 2. Nach diesen Änderungen sollte Ihre DataList und ObjectDataSource deklarative Markup etwa wie folgt aussehen:

[!code-aspx[Main](master-detail-filtering-acess-two-pages-datalist-vb/samples/sample4.aspx)]

Um diese Seite in Aktion anzuzeigen, starten Sie über die `CategoryListMaster.aspx` Seite; klicken Sie anschließend auf einen Link in der Aufzählung der Kategorien. Auf diese Weise gelangen Sie zur `ProductsForCategoryDetails.aspx`, und übergeben Sie entlang der `CategoryID` über die Abfragezeichenfolge. Die `ProductsInCategoryDataSource` zu "ObjectDataSource" `ProductsForCategoryDetails.aspx` klicken Sie dann nur die Produkte für die angegebene Kategorie abrufen und anzeigen im DataList-Steuerelement, das zwei Produkte pro Zeile gerendert wird. Abbildung 6 zeigt einen Screenshot der `ProductsForCategoryDetails.aspx` beim Anzeigen der Getränke.


[![Die Getränke werden angezeigt, zwei pro Zeile](master-detail-filtering-acess-two-pages-datalist-vb/_static/image17.png)](master-detail-filtering-acess-two-pages-datalist-vb/_static/image16.png)

**Abbildung 6**: die Getränke werden angezeigt, zwei pro Zeile ([klicken Sie, um das Bild in voller Größe anzeigen](master-detail-filtering-acess-two-pages-datalist-vb/_static/image18.png))


## <a name="step-4-displaying-category-information-on-productsforcategorydetailsaspx"></a>Schritt 4: Anzeigen von Informationen zu Auftragskategorien auf ProductsForCategoryDetails.aspx

Wenn ein Benutzer klickt auf eine Kategorie in `CategoryListMaster.aspx`, die verwendet werden, damit `ProductsForCategoryDetails.aspx` und die Produkte, die der ausgewählten Kategorie gehören angezeigt. Allerdings `ProductsForCategoryDetails.aspx` stehen keine visuelle Hinweise, welche Kategorie ausgewählt wurde. Ein Benutzer, der Getränke jedoch versehentlich geklickt "Gewürze", klicken Sie auf sollen hat keine Möglichkeit der Umsetzung ihrer Fehler aus, sobald sie erreichen `ProductsForCategoryDetails.aspx`. Um dieses potenzielle Problem zu umgehen, können wir Informationen über die ausgewählte Kategorie anzeigen – Name und Beschreibung, am oberen Rand der `ProductsForCategoryDetails.aspx` Seite.

Um dies zu erreichen, fügen Sie einem FormView-Steuerelement über dem Repeater-Steuerelement im `ProductsForCategoryDetails.aspx`. Fügen Sie eine neue "ObjectDataSource" auf der Seite über das FormView smart Tag mit dem Namen `CategoryDataSource` und konfigurieren Sie ihn zur Verwendung der `CategoriesBLL` Klasse `GetCategoryByCategoryID(categoryID)` Methode.


[![Zugriff auf Informationen über die Kategorie der CategoriesBLL Klasse GetCategoryByCategoryID(categoryID)-Methode](master-detail-filtering-acess-two-pages-datalist-vb/_static/image20.png)](master-detail-filtering-acess-two-pages-datalist-vb/_static/image19.png)

**Abbildung 7**: Zugriff auf Informationen über die Kategorie über die `CategoriesBLL` Klasse `GetCategoryByCategoryID(categoryID)` Methode ([klicken Sie, um das Bild in voller Größe anzeigen](master-detail-filtering-acess-two-pages-datalist-vb/_static/image21.png))


Wie bei der `ProductsInCategoryDataSource` "ObjectDataSource" in Schritt 3 hinzugefügt der `CategoryDataSource`des Assistenten zum Konfigurieren von Datenquellen verlangt eine Quelle für die `GetCategoryByCategoryID(categoryID)` Methode der Eingabeparameter. Verwenden Sie exakten denselben Einstellungen wie vorher, indem Sie die Parameterquelle QueryString und der QueryStringField-Wert, der `CategoryID` (siehe Abbildung 5).

Nach dem Fertigstellen des Assistenten für Visual Studio erstellt automatisch eine `ItemTemplate`, `EditItemTemplate`, und `InsertItemTemplate` für das FormView-Steuerelement. Da wir eine nur-Lese Schnittstelle bereitstellen, können Sie entfernen die `EditItemTemplate` und `InsertItemTemplate`. Darüber hinaus können Sie zum Anpassen der FormView `ItemTemplate`. Nach dem Entfernen der überflüssigen Vorlagen und Anpassen von ItemTemplate, sollte Ihre FormView und ObjectDataSource deklarative Markup etwa wie folgt aussehen:

[!code-aspx[Main](master-detail-filtering-acess-two-pages-datalist-vb/samples/sample5.aspx)]

Abbildung 8 zeigt einen Screenshot, wenn Sie diese Seite über einen Browser anzeigen.

> [!NOTE]
> Zusätzlich zu den FormView-Steuerelement, habe ich auch ein HyperLink-Steuerelement über das FormView-Steuerelement, mit denen den Benutzer zurück zur Liste der Kategorien gelangen hinzugefügt (`CategoryListMaster.aspx`). Können Sie diesen Link an anderer Stelle zu platzieren oder ihn vollständig auslassen.


[![Informationen zu Auftragskategorien ist jetzt am oberen Rand der Seite angezeigt.](master-detail-filtering-acess-two-pages-datalist-vb/_static/image23.png)](master-detail-filtering-acess-two-pages-datalist-vb/_static/image22.png)

**Abbildung 8**: Informationen zu Auftragskategorien ist jetzt am oberen Rand der Seite angezeigt ([klicken Sie, um das Bild in voller Größe anzeigen](master-detail-filtering-acess-two-pages-datalist-vb/_static/image24.png))


## <a name="step-5-displaying-a-message-if-no-products-belong-to-the-selected-category"></a>Schritt 5: Anzeigen einer Meldung auf, wenn keine Produkte der ausgewählten Kategorie gehören.

Die `CategoryListMaster.aspx` Seite listet alle Kategorien im System, unabhängig davon, ob vorhandene verknüpfte Produkte. Wenn ein Benutzer auf eine Kategorie ohne zugeordnete Serverprodukte DataList-Steuerelement in klickt `ProductsForCategoryDetails.aspx` nicht gerendert werden, da die Datenquelle keine Elemente. Wie wir in den letzten Tutorials gesehen haben, bietet die GridView ein `EmptyDataText` -Eigenschaft, die verwendet werden kann, um eine SMS, um anzuzeigen, wenn keine Datensätze vorhanden, in der Datenquelle sind anzugeben. Leider hat weder die Repeater das DataList-Steuerelement eine solche Eigenschaft.

Um eine Meldung, die den Benutzer darüber informiert, dass es keine entsprechenden Produkte für die ausgewählte Kategorie hinzufügen, anzeigen eine Bezeichnung-Steuerelement auf der Seite, dessen `Text` -Eigenschaft wird die Meldung angezeigt, es keine entsprechenden Produkte sind zugewiesen. Dann müssen wir zum programmgesteuerten Festlegen der `Visible` -Eigenschaft basierend auf, und zwar unabhängig davon, ob DataList-Steuerelement keine Elemente enthält.

Zu diesem Zweck zunächst eine Bezeichnung unterhalb DataList-Steuerelement hinzufügen. Legen Sie seine `ID` Eigenschaft `NoProductsMessage` und dessen `Text` Eigenschaft auf "Sind keine Produkte für die ausgewählte Kategorie..." Als Nächstes müssen wir diese Bezeichnung, die programmgesteuert festgelegt `Visible` -Eigenschaft basierend auf, und zwar unabhängig davon, ob alle Daten an gebunden war die `ProductsInCategory` DataList-Steuerelement. Diese Zuweisung muss vorgenommen werden, nachdem die Daten an die Datenliste gebunden wurde. Für die GridView, DetailsView und FormView-Steuerelement, erstellen wir könnten einen Ereignishandler für des Steuerelements des `DataBound` Ereignis, das ausgelöst wird, nach Abschluss der Datenbindung. Jedoch weder DataList-Steuerelement noch der Repeater verfügt über eine `DataBound` Ereignis verfügbar.

Bei diesem besonderen Beispiel können wir die Bezeichnung zuweisen `Visible` -Eigenschaft in der `Page_Load` -Ereignishandler, da die Daten werden an die Datenliste vor der Seite zugewiesen wurden `Load` Ereignis. Dieser Ansatz würde jedoch nicht im Allgemeinen, funktionieren, wie die Daten aus dem ObjectDataSource-Steuerelement an die Datenliste später im Lebenszyklus der Seite gebunden werden können. Z. B. wenn die angezeigten Daten auf dem Wert in einem anderen Steuerelement basiert, wie sie ist, wenn eine Master/Detail-Berichts mit einem DropDownList-Steuerelement auf der "master" Datensätze anzeigen, die Daten möglicherweise nicht erneut gebunden an das Web-Steuerelement erst die `PreRender` Staging die Lebenszyklus der Seite.

Eine Lösung das funktioniert in allen Fällen besteht darin, weisen Sie die `Visible` Eigenschaft `False` im des DataList-Steuerelement `ItemDataBound` (oder `ItemCreated`) Ereignishandler beim Binden eines Elementtyps, der `Item` oder `AlternatingItem`. In diesem Fall wissen wir, die es ist mindestens eine Element in der Datenquelle und aus diesem Grund können Ausblenden der `NoProductsMessage` Bezeichnung. Zusätzlich zu diesem Ereignishandler benötigen wir auch einen Ereignishandler für das DataList-Steuerelement `DataBinding` Ereignis, in dem wir des Bezeichnungsfelds initialisieren `Visible` Eigenschaft `True`. Da die `DataBinding` Ereignis wird ausgelöst, bevor Sie die `ItemDataBound` Ereignisse, die Bezeichnung des `Visible` Eigenschaft zunächst auf festgelegt `True`, wenn alle Datenelemente vorhanden sind es wird jedoch festgelegt werden, um `False`. Der folgende Code wird diese Logik implementiert:

[!code-vb[Main](master-detail-filtering-acess-two-pages-datalist-vb/samples/sample6.vb)]

Alle Kategorien in der Northwind-Datenbank sind mit einem oder mehreren Produkten verknüpft. Um dieses Feature zu testen, ich habe manuell angepasst die Northwind-Datenbank für dieses Tutorial Neuzuweisen aller Produkte, die der erstellen-Kategorie zugeordnet (`CategoryID` = 7), der Kategorie "Seafood" (`CategoryID` = 8). Dies geschieht im Server-Explorer durch neue Abfrage auswählen und mithilfe des folgenden `UPDATE` Anweisung:

[!code-sql[Main](master-detail-filtering-acess-two-pages-datalist-vb/samples/sample7.sql)]

Nach dem die Datenbank entsprechend zu aktualisieren, zurück zu den `CategoryListMaster.aspx` Seite, und klicken Sie auf den Link erstellen. Da es sich nicht mehr alle Produkte in der Kategorie erstellen, sollte die Meldung "Es gibt keine Produkte für die ausgewählte Kategorie..." angezeigt werden, wie in Abbildung 9 gezeigt.


[![Es wird eine Meldung angezeigt, wenn es keine Produkte gehören zur Kategorie ausgewählt sind](master-detail-filtering-acess-two-pages-datalist-vb/_static/image26.png)](master-detail-filtering-acess-two-pages-datalist-vb/_static/image25.png)

**Abbildung 9**: eine Nachricht wird angezeigt, wenn keine Produkte gehören zur Kategorie ausgewählt ([klicken Sie, um das Bild in voller Größe anzeigen](master-detail-filtering-acess-two-pages-datalist-vb/_static/image27.png))


## <a name="summary"></a>Zusammenfassung

Master/Detail-Berichte können sowohl die Master- und Detaildatensätzen auf einer einzelnen Seite anzeigen, aber auf vielen Websites diese auf zwei Webseiten verteilt. In diesem Tutorial erläutert, wie Sie ein solchen Master/Detail-Berichts zu implementieren, indem Sie mit den Kategorien aufgeführt, die in einer Aufzählung, die mit einem Wiederholungssteuerelement auf der Webseite "master" und die zugehörigen Produkte aufgeführt, die auf der Seite "Details". Jedes Listenelement in der master-Webseite enthalten einen Link zur Detailseite, die der Zeile übergeben `CategoryID` Wert.

In der Seite Details zum Abrufen dieser Produkte für den angegebenen Lieferanten wurde erreicht, über die `ProductsBLL` Klasse `GetProductsByCategoryID(categoryID)` Methode. Die *`categoryID`* Parameterwert angegeben wurde, deklarativ mithilfe der `CategoryID` Querystring-Wert als die Parameterquelle. Wir haben uns auch wie Kategoriedetails Details mit einem FormView-Steuerelement angezeigt wird und wie Sie eine Meldung angezeigt, wenn es keine Produkte gab, die der ausgewählten Kategorie gehören.

Viel Spaß beim Programmieren!

## <a name="about-the-author"></a>Der Autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor von sieben Büchern zu ASP/ASP.NET und Gründer von [4GuysFromRolla.com](http://www.4guysfromrolla.com), arbeitet mit Microsoft-Web-Technologien seit 1998. Er ist als ein unabhängiger Berater, Schulungsleiter und Autor. Sein neueste Buch wird [*Sams Schulen selbst ASP.NET 2.0 in 24 Stunden*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Er ist unter [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) oder über seinen Blog finden Sie unter [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Besonderen Dank an...

Diese tutorialreihe wurde durch viele hilfreiche Reviewer überprüft. Führendes Prüfer für dieses Tutorial wurden Zack Jones und Liz Shulok. Meine zukünftigen MSDN-Artikeln überprüfen möchten? Wenn dies der Fall ist, löschen Sie mir eine Linie an [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Zurück](master-detail-filtering-with-a-dropdownlist-datalist-vb.md)
> [Weiter](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb.md)
