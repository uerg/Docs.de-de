---
uid: web-forms/overview/data-access/custom-formatting/custom-formatting-based-upon-data-cs
title: Benutzerdefinierte Formatierung basierend auf Daten (c#) | Microsoft-Dokumentation
author: rick-anderson
description: Anpassen der das Format der GridView, DetailsView oder FormView basierend auf die Daten gebunden ist, kann auf verschiedene Weise erreicht werden. In diesem Tutorial werden wir l...
ms.author: aspnetcontent
ms.date: 03/31/2010
ms.assetid: 871a4574-f89c-4214-b786-79253ed3653b
msc.legacyurl: /web-forms/overview/data-access/custom-formatting/custom-formatting-based-upon-data-cs
msc.type: authoredcontent
ms.openlocfilehash: 05f83fc178cb3f79a86638d0159e692aef4410ca
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2018
ms.locfileid: "37819265"
---
<a name="custom-formatting-based-upon-data-c"></a>Benutzerdefinierte Formatierung basierend auf Daten (c#)
====================
durch [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Beispiel-App herunter](http://download.microsoft.com/download/9/6/9/969e5c94-dfb6-4e47-9570-d6d9e704c3c1/ASPNET_Data_Tutorial_11_CS.exe) oder [PDF-Datei herunterladen](custom-formatting-based-upon-data-cs/_static/datatutorial11cs1.pdf)

> Anpassen der das Format der GridView, DetailsView oder FormView basierend auf die Daten gebunden ist, kann auf verschiedene Weise erreicht werden. In diesem Tutorial untersuchen wir an, wie die datengebundene Formatierung durch die Verwendung von DataBound- und RowDataBound-Ereignishandlern zu erreichen.


## <a name="introduction"></a>Einführung

Die Darstellung der GridView, DetailsView oder FormView-Steuerelemente kann über eine Vielzahl von Formatvorlagen bezogenen Eigenschaften angepasst werden. Eigenschaften wie `CssClass`, `Font`, `BorderWidth`, `BorderStyle`, `BorderColor`, `Width`, und `Height`, u. a. Legen Sie das allgemeine Erscheinungsbild des Steuerelements gerendert. Eigenschaften, einschließlich `HeaderStyle`, `RowStyle`, `AlternatingRowStyle`, und andere diese demselben Style-Einstellungen, die auf bestimmte Bereiche angewendet werden können. Ebenso können diese formateinstellungen auf Feldebene angewendet werden.

In vielen Szenarien hängen jedoch die Formatierungen Anforderungen den Wert der angezeigten Daten. Z. B. um eingreifen, um von Produkten Kursdiagramme zeichnen, ein Bericht, in der Produktinformationen möglicherweise legen die Hintergrundfarbe auf Gelb fest für diese Produkte, deren `UnitsInStock` und `UnitsOnOrder` Felder sind beide gleich 0. Um die teurer Produkte zu markieren, sollten wir die Preise der Produkte Kosten mehr als $75,00 in fett formatierter Schrift angezeigt.

Anpassen der das Format der GridView, DetailsView oder FormView basierend auf die Daten gebunden ist, kann auf verschiedene Weise erreicht werden. In diesem Tutorial betrachten wir Vorgehensweise datengebundene Formatierung durch die Verwendung von der `DataBound` und `RowDataBound` -Ereignishandler. Im nächsten Tutorial werden wir eine alternative Methode untersuchen.

## <a name="using-the-detailsview-controlsdataboundevent-handler"></a>Das DetailsView-Steuerelement mit`DataBound`-Ereignishandler

Wenn Daten gebunden ist mit einer DetailsView, entweder aus einem Datenquellen-Steuerelement oder durch programmgesteuertes Zuweisen von Daten des Steuerelements `DataSource` -Eigenschaft ab, und rufen die `DataBind()` -Methode, die folgende Sequenz von Schritten ausgeführt:

1. Der Daten-Websteuerelement `DataBinding` -Ereignis ausgelöst wird.
2. Die Daten, die an den Daten-Websteuerelement gebunden ist.
3. Der Daten-Websteuerelement `DataBound` -Ereignis ausgelöst wird.

Benutzerdefinierter Logik kann unmittelbar auf die Schritte 1 und 3 über einen Ereignishandler eingefügt werden. Erstellen Sie einen Ereignishandler für die `DataBound` Ereignis wir programmgesteuert zu ermitteln können, die Daten, die wurde an das Web-Steuerelement gebunden, und passen Sie die Formatierung, je nach Bedarf. Um dies zu veranschaulichen wir erstellen Sie eine DetailsView, die Listet allgemeine Informationen zu einem Produkt, sondern zeigt die `UnitPrice` Wert in eine ***fett, kursiv Schriftart*** $75,00 überschreitet.

## <a name="step-1-displaying-the-product-information-in-a-detailsview"></a>Schritt 1: Anzeigen der Produktinformationen in einem DetailsView

Öffnen der `CustomColors.aspx` auf der Seite die `CustomFormatting` Ordner einem DetailsView-Steuerelement aus der Toolbox in den Designer ziehen, legen Sie seine `ID` Eigenschaftswert `ExpensiveProductsPriceInBoldItalic`, und binden Sie es an ein neues ObjectDataSource-Steuerelement, das aufruft der `ProductsBLL` Klasse `GetProducts()` Methode. Die ausführlichen Schritte, um dies zu erreichen, werden hier aus Gründen der Übersichtlichkeit weggelassen, da wir sie in vorherigen Tutorials ausführlich untersucht.

Nachdem Sie dem ObjectDataSource-Steuerelement zur DetailsView gebunden haben, können Sie die Feldliste zu ändern. Ich habe mich entschieden habe, zum Entfernen der `ProductID`, `SupplierID`, `CategoryID`, `UnitsInStock`, `UnitsOnOrder`, `ReorderLevel`, und `Discontinued` BoundFields umbenannt, und die verbleibenden BoundFields umformatiert. Ich ebenfalls deaktiviert die `Width` und `Height` Einstellungen. Da DetailsView nur einen einzelnen Datensatz angezeigt wird, müssen wir Paging zu aktivieren, damit dem Benutzer alle Produkte anzeigen können. Klicken Sie dazu das Kontrollkästchen Paging aktivieren im DetailsView Smarttag.


[![Überprüfen Sie das Aktivieren von Paging im DetailsView Smarttag](custom-formatting-based-upon-data-cs/_static/image2.png)](custom-formatting-based-upon-data-cs/_static/image1.png)

**Abbildung 1**: das Kontrollkästchen aktivieren Sie Paging im DetailsView Smarttag ([klicken Sie, um das Bild in voller Größe anzeigen](custom-formatting-based-upon-data-cs/_static/image3.png))


Nach diesen Änderungen werden das DetailsView-Markup:


[!code-aspx[Main](custom-formatting-based-upon-data-cs/samples/sample1.aspx)]

Nehmen Sie einen Moment Zeit, um diese Seite in Ihrem Browser zu testen.


[![DetailsView-Steuerelement wird ein Produkt zu einem Zeitpunkt angezeigt.](custom-formatting-based-upon-data-cs/_static/image5.png)](custom-formatting-based-upon-data-cs/_static/image4.png)

**Abbildung 2**: das DetailsView-Steuerelement zeigt ein Produkt zu einem Zeitpunkt ([klicken Sie, um das Bild in voller Größe anzeigen](custom-formatting-based-upon-data-cs/_static/image6.png))


## <a name="step-2-programmatically-determining-the-value-of-the-data-in-the-databound-event-handler"></a>Schritt 2: Programmgesteuertes bestimmen den Wert der Daten in den datengebundenen-Ereignishandler

Um den Preis in einer Schriftart fett, kursiv für diese Produkte anzuzeigen, deren `UnitPrice` Wert überschreitet, $75,00, müssen wir zuerst programmgesteuert ermitteln, können die `UnitPrice` Wert. Für die DetailsView, dies kann erreicht werden der `DataBound` -Ereignishandler. Das Ereignis erstellt wird Handler klicken Sie auf der DetailsView im Designer, und navigieren Sie zu dem Fenster "Eigenschaften". Drücken Sie F4, um angezeigt, ist dies nicht sichtbar ist, oder wechseln Sie zu dem Menü "Ansicht", und wählen Sie die Menüoption "-" Fenster "Eigenschaften". Fenster "Eigenschaften" klicken Sie auf das Blitzsymbol, DetailsViews Veranstaltungen aufgeführt. Doppelklicken Sie dann entweder die `DataBound` Ereignis- oder geben Sie den Namen der Ereignishandler, die Sie erstellen möchten.


![Erstellen Sie einen Ereignishandler für das DataBound-Ereignis](custom-formatting-based-upon-data-cs/_static/image7.png)

**Abbildung 3**: Erstellen Sie einen Ereignishandler für die `DataBound` Ereignis


Auf diese Weise werden automatisch den Ereignishandler zu erstellen und gelangen Sie zu den Codeteil, in dem hat er hinzugefügt wurde. An diesem Punkt wird angezeigt:


[!code-csharp[Main](custom-formatting-based-upon-data-cs/samples/sample2.cs)]

An der DetailsView gebundenen Daten können auf das über die `DataItem` Eigenschaft. Denken Sie daran, dass wir unsere Steuerelemente zu einer DataTable stark typisierte Bindung das aus einer Auflistung von stark typisierten DataRow-Instanzen besteht. Wenn DataTable zu DetailsView gebunden ist, wird die erste DataRow in der Datentabelle DetailsViews zugewiesen `DataItem` Eigenschaft. Insbesondere die `DataItem` -Eigenschaft zugewiesen ist eine `DataRowView` Objekt. Wir können die `DataRowView`des `Row` Eigenschaft für den Zugriff auf das zugrunde liegende DataRow-Objekt handelt es sich eigentlich um ein `ProductsRow` Instanz. Sobald wir dies haben `ProductsRow` Instanz können wir unsere Entscheidung treffen, durch die einfache Überprüfung der Eigenschaftswerte des Objekts.

Der folgende Code zeigt, wie Sie feststellen, ob die `UnitPrice` Wert, der an das DetailsView-Steuerelement gebunden ist größer als $75,00:


[!code-csharp[Main](custom-formatting-based-upon-data-cs/samples/sample3.cs)]

> [!NOTE]
> Da `UnitPrice` kann eine `NULL` Wert in der Datenbank, wir prüfen zuerst, um sicherzustellen, dass wir nicht zu tun haben eine `NULL` Wert vor dem Zugriff auf die `ProductsRow`des `UnitPrice` Eigenschaft. Diese Überprüfung ist wichtig, da Wenn wir versuchen, den Zugriff auf die `UnitPrice` Eigenschaft, wenn sie verfügt über eine `NULL` Wert der `ProductsRow` Objekt löst ein [StrongTypingException-Ausnahme](https://msdn.microsoft.com/library/system.data.strongtypingexception.aspx).


## <a name="step-3-formatting-the-unitprice-value-in-the-detailsview"></a>Schritt 3: Formatieren der UnitPrice-Wert in DetailsView

An diesem Punkt können wir bestimmen, ob die `UnitPrice` Wert zur DetailsView gebunden wurde einen Wert, der $75,00 überschreitet, aber wir haben noch Informationen zum programmgesteuerten Anpassen der DetailsView entsprechend Formatieren des. Zum Ändern der Formatierung der eine ganze Zeile in der DetailsView programmgesteuerten Zugriff auf die Zeile mit `DetailsViewID.Rows[index]`; um zu eine bestimmte Zelle ändern, die Zugriff auf die Verwendung `DetailsViewID.Rows[index].Cells[index]`. Sobald wir einen Verweis auf die Zeile oder Zelle, die wir seine Darstellung haben durch Festlegen der Formatvorlagen bezogenen Eigenschaften dann anpassen können.

Programmgesteuerter Zugriff auf eine Zeile ist erforderlich, dass man den Index der Zeile, die bei 0 beginnt. Die `UnitPrice` Zeile ist die fünfte Zeile in der DetailsView, ihm einen Index 4 aus, und somit die programmgesteuert zugegriffen werden kann mithilfe von `ExpensiveProductsPriceInBoldItalic.Rows[4]`. An diesem Punkt können wir die gesamte Zeile Inhalte, die mit den folgenden Code in einer Schriftart fett, kursiv angezeigt haben:


[!code-csharp[Main](custom-formatting-based-upon-data-cs/samples/sample4.cs)]

Dadurch wird allerdings, *sowohl* die Bezeichnung (Preis) und den Wert fett und kursiv. Wenn Sie möchten wir lediglich den Wert fett und kursiv, den müssen wir dies Formatierung auf die zweite Zelle in der Zeile, die mit dem folgenden durchgeführt werden kann:


[!code-csharp[Main](custom-formatting-based-upon-data-cs/samples/sample5.cs)]

Da unsere Tutorials bisher Stylesheets verwendet haben, um eine saubere Trennung zwischen dem gerenderten Markup und Formatvorlagen bezogenen Informationen verwalten, anstatt die spezifischen Eigenschaften festlegen, wie wir stattdessen oben verwenden Sie eine CSS-Klasse ein. Öffnen der `Styles.css` Stylesheet, und fügen Sie eine neue CSS-Klasse, die mit dem Namen `ExpensivePriceEmphasis` mit folgender Definition:


[!code-css[Main](custom-formatting-based-upon-data-cs/samples/sample6.css)]

Klicken Sie auf die `DataBound` -Ereignishandler festgelegt, der Zelle `CssClass` Eigenschaft `ExpensivePriceEmphasis`. Der folgende code zeigt die `DataBound` -Ereignishandler in seiner Gesamtheit:


[!code-csharp[Main](custom-formatting-based-upon-data-cs/samples/sample7.cs)]

Beim Anzeigen von Chai entspricht, die weniger als $75,00 kostet, ist der Preis in normaler Schriftart angezeigt (siehe Abbildung 4). Allerdings beim Anzeigen von Mishi Kobe Niku, die einen Preis von $97.00 verfügt, der Preis wird in einer Schriftart fett, kursiv (siehe Abbildung 5).


[![Weniger als $75,00 Preise werden in normaler Schrift angezeigt](custom-formatting-based-upon-data-cs/_static/image9.png)](custom-formatting-based-upon-data-cs/_static/image8.png)

**Abbildung 4**: weniger als $75,00 Preise werden in normaler Schriftart angezeigt ([klicken Sie, um das Bild in voller Größe anzeigen](custom-formatting-based-upon-data-cs/_static/image10.png))


[![Teuersten Produkte Preise werden in einem fett, kursiv Schriftart angezeigt.](custom-formatting-based-upon-data-cs/_static/image12.png)](custom-formatting-based-upon-data-cs/_static/image11.png)

**Abbildung 5**: teuersten Produkte Preise werden in einem fett, kursiv Schriftart angezeigt ([klicken Sie, um das Bild in voller Größe anzeigen](custom-formatting-based-upon-data-cs/_static/image13.png))


## <a name="using-the-formview-controlsdataboundevent-handler"></a>Verwenden das FormView-Steuerelement`DataBound`-Ereignishandler

Die Schritte zum Ermitteln der zugrunde liegenden Daten, die an einem FormView-Steuerelement gebunden sind identisch mit denen für eine DetailsView erstellen eine `DataBound` umgewandelt-Ereignishandler der `DataItem` Eigenschaft, um den geeigneten Typ für das Steuerelement gebunden, und bestimmen Sie, wie ein, um den Vorgang fortzusetzen. Die FormView und DetailsView unterscheiden sich jedoch, in wie ihre Benutzeroberfläche Darstellung aktualisiert wird.

Die FormView enthält keine BoundFields und aus diesem Grund verfügt nicht über die `Rows` Auflistung. Stattdessen besteht aus einem FormView-Steuerelement von Vorlagen, die eine Mischung aus statischen HTML-Daten enthalten können, Websteuerelemente und Databinding-Syntax. Anpassen der den Stil der einem FormView-Steuerelement in der Regel wird das Format von mindestens einer von der Web-Steuerelementen in der FormView Vorlagen.

Um dies zu veranschaulichen, wir verwenden, die einem FormView-Steuerelement auf Liste Produkte wie wir im vorherigen Beispiel, aber dieses Mal anzeigen nur der Produktname und die Einheiten auf Lager mit den Einheiten auf Lager in roter Schrift angezeigt werden, wenn er kleiner als oder gleich 10 ist.

## <a name="step-4-displaying-the-product-information-in-a-formview"></a>Schritt 4: Anzeigen der Produktinformationen in einem FormView-Steuerelement

Fügen Sie einem FormView-Steuerelement auf der `CustomColors.aspx` Seite unterhalb der DetailsView und legen seine `ID` Eigenschaft `LowStockedProductsInRed`. Binden Sie das FormView-Steuerelement, an das ObjectDataSource-Steuerelement, das aus dem vorherigen Schritt erstellt haben. Hiermit wird ein `ItemTemplate`, `EditItemTemplate`, und `InsertItemTemplate` für das FormView-Steuerelement. Entfernen der `EditItemTemplate` und `InsertItemTemplate` und vereinfachen die `ItemTemplate` sollen nur die `ProductName` und `UnitsInStock` Werte, die jeweils in eigene entsprechend benannten Label-Steuerelemente. Wie bei der DetailsView aus dem vorherigen Beispiel, auch das Kontrollkästchen Sie Paging aktivieren in das FormView Smarttag.

Nach diesen Änderungen sollte Ihre FormView Markup etwa wie folgt aussehen:


[!code-aspx[Main](custom-formatting-based-upon-data-cs/samples/sample8.aspx)]

Beachten Sie, dass die `ItemTemplate` enthält:

- **Statisches HTML** den Text "Produkt:" und "im Lager vorrätigen Stückzahl:" zusammen mit der `<br />` und `<b>` Elemente.
- **Web-Steuerelemente** zwei Label-Steuerelemente, `ProductNameLabel` und `UnitsInStockLabel`.
- **DataBinding-Syntax** der `<%# Bind("ProductName") %>` und `<%# Bind("UnitsInStock") %>` Syntax, die die Werte aus diesen Feldern den Bezeichnungsfeldern weist `Text` Eigenschaften.

## <a name="step-5-programmatically-determining-the-value-of-the-data-in-the-databound-event-handler"></a>Schritt 5: Programmgesteuertes bestimmen den Wert der Daten in den datengebundenen-Ereignishandler

Durch das FormView Markup ist abgeschlossen, wird der nächste Schritt programmgesteuert ermittelt werden, ob die `UnitsInStock` Wert ist kleiner als oder gleich 10. Dies erfolgt auf genau dieselbe Weise mit der FormView-Steuerelement, mit der DetailsView vorlag. Zunächst erstellen Sie einen Ereignishandler für der FormView `DataBound` Ereignis.


![Erstellen Sie den Ereignishandler für Datenbindung](custom-formatting-based-upon-data-cs/_static/image14.png)

**Abbildung 6**: Erstellen der `DataBound` -Ereignishandler


In dieser Handler der FormView umgewandelt `DataItem` Eigenschaft, um eine `ProductsRow` -Instanz, und bestimmen, ob die `UnitsInPrice` Wert ist, wir es in roter Schrift angezeigt müssen.


[!code-csharp[Main](custom-formatting-based-upon-data-cs/samples/sample9.cs)]

## <a name="step-6-formatting-the-unitsinstocklabel-label-control-in-the-formviews-itemtemplate"></a>Schritt 6: Formatieren von das UnitsInStockLabel Label-Steuerelement in das FormView ItemTemplate-Element

Der letzte Schritt ist das angezeigte formatieren `UnitsInStock` Wert in roter Schrift, wenn der Wert kleiner oder gleich 10 ist. Um dies zu erreichen, müssen wir die programmgesteuerten Zugriff auf, die `UnitsInStockLabel` steuern, der `ItemTemplate` und seine Style-Eigenschaften festlegen, sodass der Text rot angezeigt wird. Verwenden Sie ein Steuerelement in einer Vorlage für den Zugriff auf die `FindControl("controlID")` Methode wie folgt:


[!code-csharp[Main](custom-formatting-based-upon-data-cs/samples/sample10.cs)]

In unserem Beispiel auf eine Bezeichnung soll-Steuerelement, dessen `ID` Wert `UnitsInStockLabel`, sodass wir verwenden würden:


[!code-csharp[Main](custom-formatting-based-upon-data-cs/samples/sample11.cs)]

Nachdem wir einen programmgesteuerten Verweis auf das Websteuerelement haben, können wir die Formatvorlagen bezogenen Eigenschaften ändern, je nach Bedarf. Wie im früheren Beispiel eine CSS-Klasse in erstellte `Styles.css` mit dem Namen `LowUnitsInStockEmphasis`. Um dieses Format, das Label-Steuerelement anzuwenden, legen dessen `CssClass` Eigenschaft entsprechend.


[!code-csharp[Main](custom-formatting-based-upon-data-cs/samples/sample12.cs)]

> [!NOTE]
> Die Syntax für das Formatieren einer Vorlage, die programmgesteuerten Zugriff auf das Steuerelement mit `FindControl("controlID")` und anschließend Festlegen der Formatvorlagen bezogenen Eigenschaften kann auch verwendet werden, wenn mit [von TemplateFields](https://msdn.microsoft.com/library/system.web.ui.webcontrols.templatefield(VS.80).aspx) im DetailsView oder GridView -Steuerelemente. In unserem nächsten Tutorial untersuchen wir die von TemplateFields.


Abbildung 7 zeigt FormView beim Anzeigen eines Produkts, dessen `UnitsInStock` Wert ist größer als 10 ist, während das Produkt in Abbildung 8 die kleiner als 10 ist.


[![Für Produkte mit einer ausreichend großen Units In Stock ist keine benutzerdefinierte Formatierung angewendet](custom-formatting-based-upon-data-cs/_static/image16.png)](custom-formatting-based-upon-data-cs/_static/image15.png)

**Abbildung 7**: für Produkte mit einer ausreichend großen Units In Stock, die keine benutzerdefinierte Formatierung angewendet wird ([klicken Sie, um das Bild in voller Größe anzeigen](custom-formatting-based-upon-data-cs/_static/image17.png))


[![Die Einheiten im Lager Anzahl wird für diese Produkte mit Werten von 10 oder weniger in Rot angezeigt](custom-formatting-based-upon-data-cs/_static/image19.png)](custom-formatting-based-upon-data-cs/_static/image18.png)

**Abbildung 8**: der Einheiten im Lager Anzahl wird für diese Produkte mit Werten von 10 oder weniger in Rot angezeigt ([klicken Sie, um das Bild in voller Größe anzeigen](custom-formatting-based-upon-data-cs/_static/image20.png))


## <a name="formatting-with-the-gridviewsrowdataboundevent"></a>Formatierung mit GridView`RowDataBound`Ereignis

Früher untersuchten wir die Abfolge der Schritte DetailsView und FormView steuert Status über, während der Datenbindung. Sehen wir uns diese Schritte erneut Erinnerung.

1. Der Daten-Websteuerelement `DataBinding` -Ereignis ausgelöst wird.
2. Die Daten, die an den Daten-Websteuerelement gebunden ist.
3. Der Daten-Websteuerelement `DataBound` -Ereignis ausgelöst wird.

Diese drei einfachen Schritten sind ausreichend für die DetailsView und FormView-Steuerelement, da sie nur einen einzelnen Datensatz anzeigen. Für GridView zeigt die *alle* Datensätze gebunden ist Schritt 2, (nicht nur die erste) etwas komplizierter.

In Schritt 2 die GridView Listet die Datenquelle und für die einzelnen Datensätze erstellt eine `GridViewRow` -Instanz und des aktuellen Datensatzes an es gebunden. Für jede `GridViewRow` hinzugefügt haben, an die GridView, zwei Ereignisse ausgelöst:

- **`RowCreated`** wird ausgelöst, nachdem die `GridViewRow` erstellt wurde
- **`RowDataBound`** wird ausgelöst, nachdem der aktuelle Datensatz an gebunden wurde die `GridViewRow`.

Für GridView wird die Datenbindung anschließend genauer mit der folgenden Sequenz von Schritten beschrieben:

1. GridView `DataBinding` -Ereignis ausgelöst wird.
2. Die Daten, die an die GridView gebunden ist.   
  
   Für jeden Datensatz in der Datenquelle 

    1. Erstellen Sie eine `GridViewRow` Objekt
    2. Auslösen der `RowCreated` Ereignis
    3. Binden Sie den Datensatz auf der `GridViewRow`
    4. Auslösen der `RowDataBound` Ereignis
    5. Hinzufügen der `GridViewRow` auf die `Rows` Auflistung
3. GridView `DataBound` -Ereignis ausgelöst wird.

Um das Format von GridView einzelne Datensätze zu anzupassen, dann müssen wir zum Erstellen eines ereignishandlers für das `RowDataBound` Ereignis. Um dies zu veranschaulichen, fügen Sie einer GridView-Ansicht, die `CustomColors.aspx` Seite, die Listet den Namen, der Kategorie und der Preis für jedes Produkt, markieren die Produkte, deren Preis unter $10.00 durch eine gelbe Hintergrundfarbe.

## <a name="step-7-displaying-product-information-in-a-gridview"></a>Schritt 7: Anzeige von Produktinformationen in einer GridView-Ansicht

Hinzufügen einer GridView-Ansicht unterhalb der FormView-Steuerelement aus dem vorherigen Beispiel, und legen Sie dessen `ID` Eigenschaft `HighlightCheapProducts`. Da wir bereits ein ObjectDataSource-Steuerelement, die alle Produkte auf der Seite zurückgibt verfügen, gebunden Sie die GridView, die an, die werden. Bearbeiten Sie zum Schluss GridView BoundFields enthält nur die Produkte Namen, Kategorien und Preise. Nachdem diese Änderungen sollte die GridView Markup aussehen:


[!code-aspx[Main](custom-formatting-based-upon-data-cs/samples/sample13.aspx)]

Abbildung 9 zeigt unseren Fortschritt bis zum angegebenen Zeitpunkt aus, wenn Sie über einen Browser angezeigt.


[![Das GridView enthält der Name, Kategorie und Preis für jedes Produkt](custom-formatting-based-upon-data-cs/_static/image22.png)](custom-formatting-based-upon-data-cs/_static/image21.png)

**Abbildung 9**: GridView enthält der Name, Kategorie und Preis für jedes Produkt ([klicken Sie, um das Bild in voller Größe anzeigen](custom-formatting-based-upon-data-cs/_static/image23.png))


## <a name="step-8-programmatically-determining-the-value-of-the-data-in-the-rowdatabound-event-handler"></a>Schritt 8: Programmgesteuertes bestimmen den Wert der Daten in der RowDataBound-Ereignis-Handler

Wenn die `ProductsDataTable` an die GridView gebunden ist seine `ProductsRow` Instanzen sind, aufgelistet und für jedes `ProductsRow` eine `GridViewRow` erstellt wird. Die `GridViewRow`des `DataItem` -Eigenschaft zugewiesen, die bestimmten `ProductRow`, nach der GridView `RowDataBound` Ereignishandler ausgelöst wird. Um zu bestimmen, die `UnitPrice` Wert für jedes Produkt an die GridView gebunden wird, müssen wir erstellen einen Ereignishandler für der GridView `RowDataBound` Ereignis. In diesem Ereignishandler können wir überprüfen den `UnitPrice` Wert für die aktuelle `GridViewRow` und eine Formatierung Entscheidung für diese Zeile.

Dieser Ereignishandler kann mit der gleichen Reihe von Schritten wie FormView und DetailsView erstellt werden.


![Erstellen Sie einen Ereignishandler für der GridView RowDataBound-Ereignis](custom-formatting-based-upon-data-cs/_static/image24.png)

**Abbildung 10**: Erstellen Sie einen Ereignishandler für der GridView `RowDataBound` Ereignis


Erstellen auf diese Weise die-Ereignishandler führt dazu, dass den folgenden Code Codeteil der ASP.NET-Seite automatisch hinzugefügt werden:


[!code-csharp[Main](custom-formatting-based-upon-data-cs/samples/sample14.cs)]

Wenn die `RowDataBound` Ereignis wird ausgelöst, der Ereignishandler übergeben wird als zweiten Parameter ein Objekt des Typs `GridViewRowEventArgs`, die besitzt eine Eigenschaft namens `Row`. Diese Eigenschaft gibt einen Verweis auf die `GridViewRow` , die nur Daten, die gebunden wurde. Für den Zugriff auf die `ProductsRow` Instanz gebunden werden, um die `GridViewRow` verwenden wir die `DataItem` Eigenschaft wie folgt:


[!code-csharp[Main](custom-formatting-based-upon-data-cs/samples/sample15.cs)]

Bei der Arbeit mit der `RowDataBound` -Ereignishandler, es wichtig ist zu beachten, die die GridView besteht aus verschiedenen Arten von Zeilen und die dieses Ereignis wird ausgelöst, für die *alle* Zeile Typen. Ein `GridViewRow`Typ kann bestimmt werden, indem Sie seine `RowType` -Eigenschaft, und kann einen der möglichen Werte aufweisen:

- `DataRow` eine Zeile, die an einen Datensatz aus des GridView gebunden ist `DataSource`
- `EmptyDataRow` die Zeile angezeigt, wenn der GridView `DataSource` ist leer.
- `Footer` die Footerzeile dargestellt If GridView `ShowFooter` Eigenschaft auf festgelegt ist `true`
- `Header` der Headerzeile. angezeigt, wenn die GridView ShowHeader-Eigenschaft auf festgelegt `true` (Standard)
- `Pager` für die GridView zu implementieren, Paginierung, die Zeile, die die Pagingbenutzeroberfläche angezeigt wird
- `Separator` nicht für die GridView, sondern ein, die die `RowType` Eigenschaften des DataList- und Wiederholungssteuerelements gesteuert wird, zwei Daten beschäftigen wir uns in Zukunft Tutorials Websteuerelemente

Da die `EmptyDataRow`, `Header`, `Footer`, und `Pager` Zeilen sind nicht zugeordnet. ein `DataSource` aufzeichnen, sie weisen immer eine `null` Wert für ihre `DataItem` Eigenschaft. Aus diesem Grund ist, bevor Sie versuchen, arbeiten mit der aktuellen `GridViewRow`des `DataItem` -Eigenschaft, wir zunächst muss sicherstellen, dass wir zu tun haben eine `DataRow`. Dies kann erreicht werden, indem Sie überprüfen die `GridViewRow`des `RowType` Eigenschaft wie folgt:


[!code-csharp[Main](custom-formatting-based-upon-data-cs/samples/sample16.cs)]

## <a name="step-9-highlighting-the-row-yellow-when-the-unitprice-value-is-less-than-1000"></a>Schritt 9: Markieren den gelben bei der UnitPrice Zeilenwert weniger als $10,00 ist

Der letzte Schritt ist, markieren Sie programmgesteuert auf die gesamte `GridViewRow` Wenn die `UnitPrice` -Wert für diese Zeile weniger als $10,00 ist. Die Syntax für den Zugriff auf Zeilen oder Zellen einer GridView-Ansicht ist derselbe wie der DetailsView `GridViewID.Rows[index]` auf die gesamte Zeile `GridViewID.Rows[index].Cells[index]` auf eine bestimmte Zelle. Jedoch, wenn die `RowDataBound` Ereignishandler ausgelöst wird, die Datenbindung `GridViewRow` ist noch nicht an der GridView, die hinzugefügt werden `Rows` Auflistung. Aus diesem Grund kann nicht dem aktuellen auf `GridViewRow` -Instanz aus der `RowDataBound` -Ereignishandler die Auflistung von Zeilen verwenden.

Anstelle von `GridViewID.Rows[index]`, wir können die aktuelle verweisen `GridViewRow` -Instanz der `RowDataBound` Event Handler mit `e.Row`. D. h. in Reihenfolge Hervorheben der aktuellen `GridViewRow` -Instanz aus der `RowDataBound` -Ereignishandler wir verwenden:


[!code-csharp[Main](custom-formatting-based-upon-data-cs/samples/sample17.cs)]

Anstatt die `GridViewRow`des `BackColor` Eigenschaft direkt wir bleiben zunächst bei der Verwendung von CSS-Klassen. Ich habe eine CSS-Klasse, die mit dem Namen erstellt `AffordablePriceEmphasis` , die die Hintergrundfarbe auf Gelb festlegt. Die abgeschlossene `RowDataBound` Ereignishandler folgt:


[!code-csharp[Main](custom-formatting-based-upon-data-cs/samples/sample18.cs)]


[![Die meisten kostengünstige Produkte sind gelb hervorgehoben](custom-formatting-based-upon-data-cs/_static/image26.png)](custom-formatting-based-upon-data-cs/_static/image25.png)

**Abbildung 11**: die meisten kostengünstige-Produkte befinden sich in gelb hervorgehoben ([klicken Sie, um das Bild in voller Größe anzeigen](custom-formatting-based-upon-data-cs/_static/image27.png))


## <a name="summary"></a>Zusammenfassung

In diesem Tutorial wurde erläutert, wie die GridView, DetailsView und FormView basierend auf dem Steuerelement gebundenen Daten formatieren. Um dies zu erreichen wir erstellt, einen Ereignishandler für haben die `DataBound` oder `RowDataBound` Ereignisse, in dem die zugrunde liegenden Daten zusammen mit einer Änderung der Formatierung, untersucht wurde Falls erforderlich. Für den Zugriff auf die Daten an ein DetailsView oder FormView-Steuerelement gebunden, verwenden wir die `DataItem` -Eigenschaft in der `DataBound` Ereignishandler; einer GridView-Ansicht, jede `GridViewRow` Instanz `DataItem` Eigenschaft enthält die Daten gebunden werden, um diese Zeile, die in der verfügbarist`RowDataBound` -Ereignishandler.

Die Syntax für Programmgesteuertes Anpassen der Formatierung des Web-Steuerelements hängt davon ab, das Websteuerelement und wie die Daten formatiert werden angezeigt werden. Für DetailsView und GridView-Steuerelemente, die Zeilen und Zellen durch ein Ordinalindex möglich. Für das FormView-Steuerelement, das Vorlagen verwendet wird, die `FindControl("controlID")` Methode wird häufig verwendet, um ein Websteuerelement aus, in der Vorlage zu suchen.

Im nächsten Tutorial betrachten wir wie Vorlagen mit GridView und DetailsView. Darüber hinaus sehen wir ein weiteres Verfahren zum Anpassen der Formatierung basierend auf den zugrunde liegenden Daten.

Viel Spaß beim Programmieren!

## <a name="about-the-author"></a>Der Autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor von sieben Büchern zu ASP/ASP.NET und Gründer von [4GuysFromRolla.com](http://www.4guysfromrolla.com), arbeitet mit Microsoft-Web-Technologien seit 1998. Er ist als ein unabhängiger Berater, Schulungsleiter und Autor. Sein neueste Buch wird [*Sams Schulen selbst ASP.NET 2.0 in 24 Stunden*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Er ist unter [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) oder über seinen Blog finden Sie unter [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Besonderen Dank an

Diese tutorialreihe wurde durch viele hilfreiche Reviewer überprüft. Führen Sie Prüfer für dieses Tutorial wurden E.R. Gärtner ein, Dennis Patterson und Dan Jagers. Meine zukünftigen MSDN-Artikeln überprüfen möchten? Wenn dies der Fall ist, löschen Sie mir eine Linie an [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Nächste](using-templatefields-in-the-gridview-control-cs.md)
