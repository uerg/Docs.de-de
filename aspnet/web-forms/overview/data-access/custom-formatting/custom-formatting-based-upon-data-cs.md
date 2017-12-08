---
uid: web-forms/overview/data-access/custom-formatting/custom-formatting-based-upon-data-cs
title: Benutzerdefinierte Formatierung basierend auf Daten (c#) | Microsoft Docs
author: rick-anderson
description: "Passen das Format des GridView, DetailsView oder FormView basierend auf der gebundenen Daten kann auf verschiedene Weise erreicht werden. In diesem Lernprogramm fügen wir l..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/31/2010
ms.topic: article
ms.assetid: 871a4574-f89c-4214-b786-79253ed3653b
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/custom-formatting/custom-formatting-based-upon-data-cs
msc.type: authoredcontent
ms.openlocfilehash: 7c44327e1196a9e7cb9f9d12c963fb5f9b6b1b41
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="custom-formatting-based-upon-data-c"></a>Benutzerdefinierte Formatierung basierend auf Daten (c#)
====================
durch [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Beispiel-App herunterladen](http://download.microsoft.com/download/9/6/9/969e5c94-dfb6-4e47-9570-d6d9e704c3c1/ASPNET_Data_Tutorial_11_CS.exe) oder [PDF herunterladen](custom-formatting-based-upon-data-cs/_static/datatutorial11cs1.pdf)

> Passen das Format des GridView, DetailsView oder FormView basierend auf der gebundenen Daten kann auf verschiedene Weise erreicht werden. In diesem Lernprogramm sehen wir uns Formatierung durch die Verwendung der Ereignishandler datengebundenen und RowDataBound gebundene Daten erläutert.


## <a name="introduction"></a>Einführung

Die Darstellung der GridView, DetailsView und FormView-Steuerelemente kann über eine Vielzahl von Formatvorlagen bezogenen Eigenschaften angepasst werden. Eigenschaften, z. B. `CssClass`, `Font`, `BorderWidth`, `BorderStyle`, `BorderColor`, `Width`, und `Height`, u. a. Geben Sie die allgemeine Darstellung des gerenderten Steuerelements. Eigenschaften, einschließlich `HeaderStyle`, `RowStyle`, `AlternatingRowStyle`, und andere Benutzer können diese gleichen formateinstellungen auf bestimmte Bereiche angewendet werden soll. Ebenso können diese stileinstellungen auf Feldebene angewendet werden.

In vielen Szenarien hängen davon ab, die Formatierung Anforderungen den Wert der angezeigten Daten. Z. B. um ein Eingreifen, um der SKU-Produkte zu zeichnen, ein Bericht, Auflisten von Produktinformationen möglicherweise Festlegen der Hintergrundfarbe für diese Produkte, deren Gelb `UnitsInStock` und `UnitsOnOrder` Felder sind beide 0 betragen. Markieren Sie die Produkte teurer, sollten wir die Preise der Produkte Kostenberechnungen mehr als $75,00 in fett formatierter Schrift angezeigt.

Passen das Format des GridView, DetailsView oder FormView basierend auf der gebundenen Daten kann auf verschiedene Weise erreicht werden. In diesem Lernprogramm lernen wir Vorgehensweise gebundene Daten durch die Verwendung der Formatierung der `DataBound` und `RowDataBound` -Ereignishandler. Im nächsten Lernprogramm wird einen alternativen Ansatz untersucht.

## <a name="using-the-detailsview-controlsdataboundevent-handler"></a>Verwenden des DetailsView-Steuerelements`DataBound`-Ereignishandler

Wenn Daten gebunden ist, eine DetailsView ein Datenquellen-Steuerelement oder über programmgesteuert Zuweisen von Daten an des Steuerelements `DataSource` -Eigenschaft und der Aufruf der `DataBind()` -Methode, die folgende Sequenz von Schritten ausgeführt:

1. Der Daten Websteuerelement `DataBinding` Ereignis ausgelöst wird.
2. Die Daten werden an die Daten Websteuerelement gebunden.
3. Der Daten Websteuerelement `DataBound` Ereignis ausgelöst wird.

Benutzerdefinierter Logik kann sofort nach der Schritte 1 und 3 über einen Ereignishandler eingegeben werden. Erstellen Sie einen Ereignishandler für das `DataBound` Ereignis wir programmgesteuert zu ermitteln können, die Daten, die an das Webserver-Steuerelement gebunden und die Formatierung nach Bedarf anpassen. Um dies zu veranschaulichen wir erstellen eine DetailsView, die Listet allgemeine Informationen zu einem Produkt, sondern zeigt den `UnitPrice` Wert in einer ***fett, kursiv Schriftart*** $75,00 überschreitet.

## <a name="step-1-displaying-the-product-information-in-a-detailsview"></a>Schritt 1: Anzeigen der Produktinformationen in einen DetailsView

Öffnen der `CustomColors.aspx` auf der Seite der `CustomFormatting` Ordner, DetailsView-Steuerelement aus der Toolbox in den Designer ziehen, legen Sie seine `ID` Eigenschaftswert an `ExpensiveProductsPriceInBoldItalic`, und binden es an ein neues ObjectDataSource-Steuerelement, das aufruft der `ProductsBLL` Klasse `GetProducts()` Methode. Die ausführlichen Schritte, um dies zu erreichen, werden hier aus Gründen der Übersichtlichkeit weggelassen, da wir ihnen im vorherigen Lernprogrammen ausführlich untersucht.

Sobald Sie das ObjectDataSource, DetailsView gebunden haben, nehmen Sie einen Moment Zeit, um die Feldliste zu ändern. Ich haben sich entschieden, entfernen Sie die `ProductID`, `SupplierID`, `CategoryID`, `UnitsInStock`, `UnitsOnOrder`, `ReorderLevel`, und `Discontinued` BoundFields umbenannt und die verbleibenden BoundFields umformatiert. Ich auch gelöscht die `Width` und `Height` Einstellungen. Da DetailsView nur einen einzelnen Datensatz angezeigt wird, müssen wir Paging aktivieren, damit die Endbenutzer können Sie alle Produkte anzeigen können. Überprüfen das Kontrollkästchen Paging aktivieren, in der DetailsView Smarttag, um dies zu tun.


[![Überprüfen Sie das Paging aktivieren in der DetailsView Smarttag](custom-formatting-based-upon-data-cs/_static/image2.png)](custom-formatting-based-upon-data-cs/_static/image1.png)

**Abbildung 1**: Kontrollkästchen Aktivieren der Auslagerungsdatei in der DetailsView Smarttag ([klicken Sie hier, um das Bild in voller Größe angezeigt](custom-formatting-based-upon-data-cs/_static/image3.png))


Nach der Änderung werden DetailsView Markup:


[!code-aspx[Main](custom-formatting-based-upon-data-cs/samples/sample1.aspx)]

Nehmen Sie einen Moment Zeit, um diese Seite in Ihrem Browser zu testen.


[![Das DetailsView-Steuerelement zeigt ein Produkt zu einem Zeitpunkt](custom-formatting-based-upon-data-cs/_static/image5.png)](custom-formatting-based-upon-data-cs/_static/image4.png)

**Abbildung 2**: die DetailsView-Steuerelement zeigt ein Produkt zu einem Zeitpunkt ([klicken Sie hier, um das Bild in voller Größe angezeigt](custom-formatting-based-upon-data-cs/_static/image6.png))


## <a name="step-2-programmatically-determining-the-value-of-the-data-in-the-databound-event-handler"></a>Schritt 2: Programmgesteuert den Wert der Daten in der datengebundenen Ereignishandler zu bestimmen

Um den Preis in einer Schriftart fett, kursiv für diese Produkte anzuzeigen, deren `UnitPrice` Wert überschreitet, $75,00, müssen wir zuerst programmgesteuert zu ermitteln können die `UnitPrice` Wert. Für DetailsView, dies geschieht der `DataBound` -Ereignishandler. Das Ereignis erstellt Handler klicken Sie auf der DetailsView im Designer und navigieren Sie dann auf Eigenschaftenfenster. Drücken Sie F4, um es gatewayserverkomponente, ist er nicht sichtbar ist, oder wechseln Sie zum Menü "Ansicht", und wählen Sie die Menüoption Fenster "Eigenschaften". Klicken Sie auf das Blitzsymbol, DetailsView Ereignisse aufzulisten, über das Eigenschaftenfenster. Doppelklicken Sie anschließend entweder den `DataBound` Ereignis oder den Namen der Ereignishandler, die Sie erstellen möchten.


![Erstellen Sie einen Ereignishandler für das datengebundene-Ereignis](custom-formatting-based-upon-data-cs/_static/image7.png)

**Abbildung 3**: Erstellen Sie einen Ereignishandler für das `DataBound` Ereignis


Auf diese Weise automatisch erstellt den Ereignishandler und enthalten Informationen zu den Teil des Codes, hat er hinzugefügt wurde. An diesem Punkt wird angezeigt:


[!code-csharp[Main](custom-formatting-based-upon-data-cs/samples/sample2.cs)]

Die Daten gebunden, DetailsView Zugriff darauf erfolgt über die `DataItem` Eigenschaft. Beachten Sie, dass wir unsere Steuerelemente zu einer DataTable stark typisierte Binden der eine Auflistung von stark typisierten DataRow-Instanzen umfasst. DataTable, DetailsView gebunden ist, wird die erste "DataRow" in der Datentabelle, DetailsView des zugewiesen `DataItem` Eigenschaft. Insbesondere die `DataItem` Eigenschaft zugewiesen ist ein `DataRowView` Objekt. Wir können die `DataRowView`des `Row` Eigenschaft, um den Zugriff auf das zugrunde liegende DataRow-Objekt, das ist tatsächlich eine `ProductsRow` Instanz. Wenn wir dies haben `ProductsRow` Instanz stellen wir unsere Entscheidung, durch die einfache Überprüfung Eigenschaftswerte des Objekts.

Der folgende Code zeigt, wie Sie feststellen, ob die `UnitPrice` Wert gebunden, DetailsView-Steuerelement ist größer als $75,00:


[!code-csharp[Main](custom-formatting-based-upon-data-cs/samples/sample3.cs)]

> [!NOTE]
> Da `UnitPrice` kann eine `NULL` Wert in der Datenbank wir prüfen Sie zunächst sicherstellen, dass es nicht zu tun haben eine `NULL` Wert vor dem Zugriff auf die `ProductsRow`des `UnitPrice` Eigenschaft. Dies ist wichtig da Wenn wir versuchen, den Zugriff auf die `UnitPrice` Eigenschaft bei einer `NULL` Wert der `ProductsRow` Objekt löst eine [StrongTypingException-Ausnahme](https://msdn.microsoft.com/en-us/library/system.data.strongtypingexception.aspx).


## <a name="step-3-formatting-the-unitprice-value-in-the-detailsview"></a>Schritt 3: Formatieren der UnitPrice-Wert in der DetailsView

An diesem Punkt können wir bestimmen, ob die `UnitPrice` , DetailsView gebundene Wert ist einen Wert, der $75,00 überschreitet, aber wir noch nicht entsprechend Formatierung, um programmgesteuert anzupassen, DetailsView finden Sie unter. Zum Ändern der Formatierung einer ganzen Zeile in der DetailsView programmgesteuerten Zugriff auf die Zeile mit `DetailsViewID.Rows[index]`; greifen Sie zum Ändern einer bestimmten Zelle verwenden `DetailsViewID.Rows[index].Cells[index]`. Sobald wir einen Verweis auf die Zeile oder Zelle, die wir seine Darstellung haben durch Einstellen der Formatvorlagen bezogenen Eigenschaften dann anpassen können.

Programmgesteuerter Zugriff auf eine Zeile ist erforderlich, dass Sie den Index der Zeile, wissen, welche die bei 0 beginnt. Die `UnitPrice` Zeile ist die fünfte Zeile in der DetailsView, und geben sie Ihnen einen Index 4 und somit die programmgesteuert zugegriffen werden kann mit `ExpensiveProductsPriceInBoldItalic.Rows[4]`. Zu diesem Zeitpunkt möglicherweise wird die gesamte Zeile Inhalt in einer Schriftart fett, kursiv angezeigt werden, indem Sie den folgenden Code:


[!code-csharp[Main](custom-formatting-based-upon-data-cs/samples/sample4.cs)]

Allerdings können *beide* die Bezeichnung (Preis) und den Wert fett und kursiv. Wenn wir möchten nur den Wert fett und kursiv zu machen, den müssen wir dies Formatierungen auf der zweiten Zelle in der Zeile, die Nutzung der folgenden Komponenten ausgeführt werden kann:


[!code-csharp[Main](custom-formatting-based-upon-data-cs/samples/sample5.cs)]

Da unseren Tutorials bisher Stylesheets verwendet haben, eine saubere Trennung zwischen dem gerenderten Markups und Formatvorlagen bezogenen Informationen verwalten, verwenden Sie, anstatt die Stileigenschaften für bestimmte festlegen, wie wir stattdessen oben eine CSS-Klasse. Öffnen der `Styles.css` Stylesheet und Hinzufügen einer neuen CSS-Klasse, die mit dem Namen `ExpensivePriceEmphasis` mit folgender Definition:


[!code-css[Main](custom-formatting-based-upon-data-cs/samples/sample6.css)]

Klicken Sie auf die `DataBound` Ereignishandler, legen Sie der Zelle `CssClass` Eigenschaft `ExpensivePriceEmphasis`. Der folgende code zeigt die `DataBound` -Ereignishandler in seiner Gesamtheit:


[!code-csharp[Main](custom-formatting-based-upon-data-cs/samples/sample7.cs)]

Beim Anzeigen von Chai, die weniger als $75,00 Kosten, wird der Preis in normaler Schrift angezeigt (siehe Abbildung 4). Allerdings beim Mishi Kobe Niku, anzeigen, der einen Preis von $97.00 verfügt, der Preis angezeigt in einer Schriftart fett, kursiv (siehe Abbildung 5).


[![Weniger als $75,00 Preise werden in normaler Schrift angezeigt](custom-formatting-based-upon-data-cs/_static/image9.png)](custom-formatting-based-upon-data-cs/_static/image8.png)

**Abbildung 4**: weniger als $75,00 Preise werden in normaler Schrift angezeigt ([klicken Sie hier, um das Bild in voller Größe angezeigt](custom-formatting-based-upon-data-cs/_static/image10.png))


[![Teure Produkte Preise werden in einem fett, kursiv Schriftart angezeigt.](custom-formatting-based-upon-data-cs/_static/image12.png)](custom-formatting-based-upon-data-cs/_static/image11.png)

**Abbildung 5**: teure Produkte Preise werden in einem fett, kursiv Schriftart angezeigt ([klicken Sie hier, um das Bild in voller Größe angezeigt](custom-formatting-based-upon-data-cs/_static/image13.png))


## <a name="using-the-formview-controlsdataboundevent-handler"></a>Verwenden des FormView-Steuerelements`DataBound`-Ereignishandler

Die Schritte zur Bestimmung der zugrunde liegenden Daten gebunden werden, um eine FormView sind identisch mit denen für eine DetailsView erstellen eine `DataBound` wandeln Sie Ereignishandler, d. h. die `DataItem` Eigenschaft auf den entsprechenden Objekttyp mit dem Steuerelement verbunden, und bestimmen Sie zum Fortsetzen des Vorgangs. Die FormView und DetailsView unterscheiden sich, allerdings in wie ihre Benutzeroberfläche Darstellung aktualisiert wird.

Die FormView enthält keine BoundFields und daher verfügt nicht über die `Rows` Auflistung. Stattdessen besteht aus ein FormView von Vorlagen, die eine Mischung aus statischem HTML-Code enthalten können, Websteuerelemente und Databinding-Syntax. Anpassen der in der Regel das Format des ein FormView umfasst die Formatvorlage aus einem oder mehreren der Web-Steuerelemente innerhalb der FormView-Vorlagen anpassen.

Um dies zu veranschaulichen, wir verwenden ein FormView sind Produkte aufgelistet wie im vorherigen Beispiel, aber diesmal wir anzeigen Produktname und Einheiten im Lager mit den Einheiten im Lager in roter Schrift angezeigt, wenn er kleiner als oder gleich 10 ist.

## <a name="step-4-displaying-the-product-information-in-a-formview"></a>Schritt 4: Anzeigen der Produktinformationen in einen FormView

Hinzufügen einen FormView der `CustomColors.aspx` Seite unter die, DetailsView und legen seine `ID` Eigenschaft `LowStockedProductsInRed`. Binden Sie die FormView an das ObjectDataSource-Steuerelement aus dem vorherigen Schritt erstellt haben. Dadurch entsteht eine `ItemTemplate`, `EditItemTemplate`, und `InsertItemTemplate` für FormView. Entfernen der `EditItemTemplate` und `InsertItemTemplate` und vereinfachen die `ItemTemplate` enthalten nur die `ProductName` und `UnitsInStock` Werte, die jeweils in eigenen Bezeichnungsfelder entsprechend benannt. Wie bei der DetailsView aus dem vorigen Beispiel, auch das Kontrollkästchen Sie Paging aktivieren in der FormView-Smarttag.

Nach dieser Änderungen an der sollte Ihre FormView Markup etwa wie folgt aussehen:


[!code-aspx[Main](custom-formatting-based-upon-data-cs/samples/sample8.aspx)]

Beachten Sie, dass die `ItemTemplate` enthält:

- **Statische HTML** den Text "Produkt:" und "Units In Stock:" zusammen mit den `<br />` und `<b>` Elemente.
- **Web-Steuerelemente** die zwei Bezeichnungsfelder `ProductNameLabel` und `UnitsInStockLabel`.
- **DataBinding-Syntax** der `<%# Bind("ProductName") %>` und `<%# Bind("UnitsInStock") %>` -Syntax, die Werte aus diesen Feldern das Bezeichnungsfeld-Steuerelementen weist `Text` Eigenschaften.

## <a name="step-5-programmatically-determining-the-value-of-the-data-in-the-databound-event-handler"></a>Schritt 5: Programmgesteuert den Wert der Daten in der datengebundenen Ereignishandler zu bestimmen

Die FormView Markup abgeschlossen, wird der nächste Schritt programmgesteuert ermittelt werden, ob die `UnitsInStock` Wert ist kleiner als oder gleich 10. Dies geschieht auf genau dieselbe Weise mit FormView mit DetailsView vorlag. Starten, indem Sie einen Ereignishandler erstellen, für die FormView `DataBound` Ereignis.


![Erstellen Sie den datengebundenen-Ereignishandler](custom-formatting-based-upon-data-cs/_static/image14.png)

**Abbildung 6**: Erstellen der `DataBound` -Ereignishandler


Der Handler die FormView umgewandelt `DataItem` Eigenschaft, um eine `ProductsRow` Instanz, und bestimmen, ob die `UnitsInPrice` Wert ist, sodass wir es in roter Schrift angezeigt müssen.


[!code-csharp[Main](custom-formatting-based-upon-data-cs/samples/sample9.cs)]

## <a name="step-6-formatting-the-unitsinstocklabel-label-control-in-the-formviews-itemtemplate"></a>Schritt 6: Formatieren der UnitsInStockLabel Label-Steuerelement in der FormView ItemTemplate

Der letzte Schritt ist so formatieren Sie das angezeigte `UnitsInStock` Wert in roter Schrift, wenn der Wert 10 oder darunter beträgt. Um dies zu erreichen, müssen wir programmgesteuerten Zugriff auf, die `UnitsInStockLabel` steuern, der `ItemTemplate` , und legen Sie dessen Stileigenschaften, damit der Text rot angezeigt wird. Verwenden Sie ein Websteuerelement in einer Vorlage für den Zugriff auf die `FindControl("controlID")` Methode wie folgt:


[!code-csharp[Main](custom-formatting-based-upon-data-cs/samples/sample10.cs)]

In unserem Beispiel möchten wir den Zugriff einer Bezeichnung-Steuerelement, dessen `ID` Wert `UnitsInStockLabel`, sodass wir verwenden würden:


[!code-csharp[Main](custom-formatting-based-upon-data-cs/samples/sample11.cs)]

Nachdem wir einen programmgesteuerten Verweis auf das Websteuerelement haben, können wir die Formatvorlagen bezogenen Eigenschaften ändern, nach Bedarf. Wie mit den oben aufgeführten Beispiel ich eine CSS-Klasse in erstellten `Styles.css` mit dem Namen `LowUnitsInStockEmphasis`. Legen Sie zum Anwenden dieser Stil auf das Label-Steuerelement seine `CssClass` Eigenschaft entsprechend.


[!code-csharp[Main](custom-formatting-based-upon-data-cs/samples/sample12.cs)]

> [!NOTE]
> Die Syntax für das Formatieren einer Vorlage, die programmgesteuerten Zugriff auf das Steuerelement mit `FindControl("controlID")` , und klicken Sie dann ihre Formatvorlagen bezogenen Eigenschaften festlegen kann auch verwendet werden, wenn mit [von TemplateFields](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.templatefield(VS.80).aspx) im DetailsView oder GridView Steuerelemente. Von TemplateFields untersuchen wir in unserer nächsten Lernprogramm.


Abbildung 7 zeigt die FormView beim Anzeigen eines Produkts, dessen `UnitsInStock` Wert ist größer als 10 ist, während das Produkt in Abbildung 8, dessen Wert kleiner als 10 verfügt.


[![Für Produkte mit einer ausreichend großen Units In Stock ist keine benutzerdefinierte Formatierung angewendet](custom-formatting-based-upon-data-cs/_static/image16.png)](custom-formatting-based-upon-data-cs/_static/image15.png)

**Abbildung 7**: für Produkte mit einer ausreichend großen Units In Stock, die keine benutzerdefinierte Formatierung angewendet wird ([klicken Sie hier, um das Bild in voller Größe angezeigt](custom-formatting-based-upon-data-cs/_static/image17.png))


[![Die Einheiten im Lager Anzahl wird rot angezeigt, für die Produkte mit Werten von 10 oder weniger](custom-formatting-based-upon-data-cs/_static/image19.png)](custom-formatting-based-upon-data-cs/_static/image18.png)

**Abbildung 8**: der Einheiten im Lager Anzahl wird rot angezeigt, für die Produkte mit Werten von 10 oder weniger ([klicken Sie hier, um das Bild in voller Größe angezeigt](custom-formatting-based-upon-data-cs/_static/image20.png))


## <a name="formatting-with-the-gridviewsrowdataboundevent"></a>Formatierung mit der GridView`RowDataBound`Ereignis

Zuvor die Abfolge der Schritte, DetailsView untersucht und FormView steuert Status über, während der Datenbindung. Sehen wir uns über diese Schritte erneut als aufzufrischen.

1. Der Daten Websteuerelement `DataBinding` Ereignis ausgelöst wird.
2. Die Daten werden an die Daten Websteuerelement gebunden.
3. Der Daten Websteuerelement `DataBound` Ereignis ausgelöst wird.

Diese drei einfache Schritte sind ausreichend, DetailsView und FormView, da sie nur ein einzelner Datensatz angezeigt. Für die GridView, woraufhin *alle* Datensätze gebunden (nicht nur den ersten), und Schritt 2 ist etwas komplizierter.

Klicken Sie in Schritt 2 die GridView Listet die Datenquelle, und für jeden Datensatz erstellt eine `GridViewRow` -Instanz und den aktuellen Datensatz an es gebunden. Für jede `GridViewRow` an die GridView hinzugefügt, werden zwei Ereignisse ausgelöst:

- **`RowCreated`**wird ausgelöst, nachdem der `GridViewRow` erstellt wurde
- **`RowDataBound`**wird ausgelöst, nachdem der aktuelle Datensatz an gebunden wurde die `GridViewRow`.

Für die GridView anschließend wird die Datenbindung genauer mit der folgenden Sequenz von Schritten beschrieben:

1. Der GridView `DataBinding` Ereignis ausgelöst wird.
2. Die Daten werden an die GridView gebunden.   
  
 Für jeden Datensatz in der Datenquelle 

    1. Erstellen einer `GridViewRow` Objekt
    2. Auslösen der `RowCreated` Ereignis
    3. Binden Sie den Datensatz der`GridViewRow`
    4. Auslösen der `RowDataBound` Ereignis
    5. Hinzufügen der `GridViewRow` auf die `Rows` Auflistung
3. Der GridView `DataBound` Ereignis ausgelöst wird.

Um das Format der einzelnen Datensätze die GridView anzupassen, dann wir müssen, erstellen Sie einen Ereignishandler für das `RowDataBound` Ereignis. Um dies zu veranschaulichen, fügen wir eine GridView, die `CustomColors.aspx` Seite mit einer Liste von Name, Kategorie und Preis für jedes Produkt, markieren die Produkte, deren Preis unter $10.00 durch eine gelbe Hintergrundfarbe.

## <a name="step-7-displaying-product-information-in-a-gridview"></a>Schritt 7: Anzeigen von Produktinformationen in einer GridView

Fügen Sie eine GridView unter FormView aus dem vorherigen Beispiel, und legen seine `ID` Eigenschaft `HighlightCheapProducts`. Da wir bereits ein ObjectDataSource, die auf der Seite alle Produkte zurückgibt verfügen, binden Sie die GridView an, die ein. Bearbeiten Sie schließlich die GridView BoundFields, um nur die Produkte Namen, Kategorien und Preise enthalten. Nach dieser Änderungen an sollte die GridView-Markup aussehen:


[!code-aspx[Main](custom-formatting-based-upon-data-cs/samples/sample13.aspx)]

Abbildung 9 zeigt unseren Fortschritt zu diesem Zeitpunkt, wenn Sie über einen Browser angezeigt.


[![Listet die GridView, Name, Kategorie und Preis für jedes Produkt](custom-formatting-based-upon-data-cs/_static/image22.png)](custom-formatting-based-upon-data-cs/_static/image21.png)

**Abbildung 9**: Listet die GridView, Name, Kategorie und Preis für jedes Produkt ([klicken Sie hier, um das Bild in voller Größe angezeigt](custom-formatting-based-upon-data-cs/_static/image23.png))


## <a name="step-8-programmatically-determining-the-value-of-the-data-in-the-rowdatabound-event-handler"></a>Schritt 8: Programmgesteuert den Wert der Daten in der RowDataBound Ereignishandler zu bestimmen

Wenn die `ProductsDataTable` an die GridView gebunden ist seine `ProductsRow` Instanzen werden aufgelistet und für jedes `ProductsRow` eine `GridViewRow` wird erstellt. Die `GridViewRow`des `DataItem` die jeweilige Eigenschaft zugewiesen ist `ProductRow`, wonach der GridView `RowDataBound` Ereignishandler ausgelöst wird. Um zu bestimmen, die `UnitPrice` Wert für jedes Produkt, die an die GridView gebunden, und wir müssen einen Ereignishandler für der GridView erstellen `RowDataBound` Ereignis. In diesem Ereignishandler überprüfen wir das `UnitPrice` Wert für den aktuellen `GridViewRow` und eine Formatierung Entscheidung für diese Zeile.

Dieser Ereignishandler kann mit der gleichen Reihe von Schritten als, DetailsView und die FormView erstellt werden.


![Erstellen Sie einen Ereignishandler für der GridView RowDataBound-Ereignis](custom-formatting-based-upon-data-cs/_static/image24.png)

**Abbildung 10**: Erstellen Sie einen Ereignishandler für der GridView `RowDataBound` Ereignis


Ereignishandler auf diese Weise erstellt wird, verursacht der folgende Code der ASP.NET-Seite Codeteil automatisch hinzugefügt werden:


[!code-csharp[Main](custom-formatting-based-upon-data-cs/samples/sample14.cs)]

Wenn die `RowDataBound` Ereignis wird ausgelöst, der Ereignishandler wird übergeben als zweiten Parameter ein Objekt vom Typ `GridViewRowEventArgs`, die über eine Eigenschaft mit dem Namen verfügt `Row`. Diese Eigenschaft gibt einen Verweis auf die `GridViewRow` , die nur die Daten gebunden wurde. Für den Zugriff auf die `ProductsRow` Instanz gebunden werden, um die `GridViewRow` verwenden wir die `DataItem` Eigenschaft wie folgt:


[!code-csharp[Main](custom-formatting-based-upon-data-cs/samples/sample15.cs)]

Bei der Arbeit mit der `RowDataBound` Ereignishandler, es wichtig ist zu bedenken, die die GridView besteht aus verschiedenen Typen von Zeilen und die dieses Ereignis wird ausgelöst, für *alle* Zeile Typen. Ein `GridViewRow`Typ kann bestimmt werden, indem seine `RowType` -Eigenschaft, und kann einen der möglichen Werte haben:

- `DataRow`eine Zeile, die an einen Datensatz aus der GridView gebunden ist`DataSource`
- `EmptyDataRow`die Zeile angezeigt, wenn der GridView `DataSource` ist leer
- `Footer`die Fußzeile; dargestellt der GridView `ShowFooter` Eigenschaft auf festgelegt ist`true`
- `Header`die Kopfzeile; angezeigt, wenn die GridView ShowHeader-Eigenschaft festgelegt ist, `true` (Standard)
- `Pager`GridView implementiert, die paging, der Zeile, die der Auslagerung-Benutzeroberfläche angezeigt wird
- `Separator`nicht für die GridView verwendet, aber von verwendet die `RowType` Eigenschaften für die DataList und Repeater Steuerelemente, die zwei Data Websteuerelemente besprechen wir in Zukunft Lernprogramme

Da die `EmptyDataRow`, `Header`, `Footer`, und `Pager` Zeilen werden nicht zugeordnet eine `DataSource` aufzuzeichnen, sie besitzen immer eine `null` Wert für ihre `DataItem` Eigenschaft. Aus diesem Grund ist, bevor Sie versuchen, arbeiten mit dem aktuellen `GridViewRow`des `DataItem` -Eigenschaft, wir zuerst müssen sicherstellen, dass wir zu tun haben eine `DataRow`. Dies kann durch Überprüfen der `GridViewRow`des `RowType` Eigenschaft wie folgt:


[!code-csharp[Main](custom-formatting-based-upon-data-cs/samples/sample16.cs)]

## <a name="step-9-highlighting-the-row-yellow-when-the-unitprice-value-is-less-than-1000"></a>Schritt 9: Hervorhebung der gelben bei der UnitPrice Zeilenwert ist kleiner als 10,00 $

Der letzte Schritt besteht darin programmgesteuert markieren Sie die gesamte `GridViewRow` Wenn die `UnitPrice` -Wert für diese Zeile weniger als 10,00 ist. Die Syntax für den Zugriff auf einer GridView Zeilen oder Zellen ist dieselbe wie bei der DetailsView `GridViewID.Rows[index]` auf die gesamte Zeile `GridViewID.Rows[index].Cells[index]` Zugriff auf eine bestimmte Zelle. Jedoch, wenn die `RowDataBound` -Ereignishandler ausgelöst wird, die datengebundene `GridViewRow` wurden noch nicht an der GridView hinzuzufügende `Rows` Auflistung. Aus diesem Grund kann nicht auf die aktuelle zugreifen `GridViewRow` -Instanz von der `RowDataBound` Ereignishandler mithilfe der Zeilen-Auflistung.

Anstelle von `GridViewID.Rows[index]`, wir können die aktuelle verweisen `GridViewRow` Instanz die `RowDataBound` Ereignishandler mit `e.Row`. D. h. in Reihenfolge, markieren Sie die aktuelle `GridViewRow` -Instanz von der `RowDataBound` Ereignishandler wir verwenden:


[!code-csharp[Main](custom-formatting-based-upon-data-cs/samples/sample17.cs)]

Statt set der `GridViewRow`des `BackColor` Eigenschaft direkt, wir bleiben Sie mit der Verwendung von CSS-Klassen. Ich habe eine CSS-Klasse, die mit dem Namen erstellt `AffordablePriceEmphasis` Background-Farbe in Gelb festlegt. Das abgeschlossene `RowDataBound` Ereignishandler folgt:


[!code-csharp[Main](custom-formatting-based-upon-data-cs/samples/sample18.cs)]


[![Die meisten kostengünstige Produkte werden gelb hervorgehoben](custom-formatting-based-upon-data-cs/_static/image26.png)](custom-formatting-based-upon-data-cs/_static/image25.png)

**Abbildung 11**: die meisten kostengünstige Produkte sind gelb hervorgehoben ([klicken Sie hier, um das Bild in voller Größe angezeigt](custom-formatting-based-upon-data-cs/_static/image27.png))


## <a name="summary"></a>Zusammenfassung

In diesem Lernprogramm wurde erläutert, wie die GridView, DetailsView und FormView basierend auf dem Steuerelement gebundenen Daten formatieren. Um dies zu erreichen wir einen Ereignishandler für erstellt das `DataBound` oder `RowDataBound` Ereignisse, in denen die zugrunde liegenden Daten zusammen mit einer Änderung der Formatierung, untersucht wurde bei Bedarf. Um eine DetailsView oder FormView gebundenen Daten zuzugreifen, verwenden wir die `DataItem` Eigenschaft in der `DataBound` Ereignishandler; für eine GridView jedes `GridViewRow` -Instanz `DataItem` Eigenschaft enthält die Daten gebunden werden, um diese Zeile, die in der verfügbarist`RowDataBound` -Ereignishandler.

Die Syntax für das Anpassen der Formatierung der Daten-Websteuerelement programmgesteuert hängt davon ab, das Websteuerelement und wie die Daten zu formatierenden angezeigt werden. Für, DetailsView und GridView können Steuerelemente, die Zeilen und Zellen vom ein Ordinalindex zugegriffen werden. Für die FormView, der Vorlagen verwendet werden, die `FindControl("controlID")` Methode wird häufig zum Suchen eines Websteuerelements innerhalb der Vorlage.

In den nächsten Lernprogrammen sehen wir uns zum Verwenden von Vorlagen mit GridView und DetailsView. Darüber hinaus sehen wir eine andere Technik zum Anpassen der Formatierung basierend auf den zugrunde liegenden Daten.

Viel Spaß beim Programmieren!

## <a name="about-the-author"></a>Informationen zum Autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor von sieben ASP/ASP.NET-Büchern und Gründer von [4GuysFromRolla.com](http://www.4guysfromrolla.com), Microsoft Web-Technologien seit 1998 arbeitet. Scott fungiert als ein unabhängiger Berater, Trainer und Writer. Sein neueste Buch wird [ *Sams Schulen selbst ASP.NET 2.0 in 24 Stunden*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Er die erreicht werden kann, zur [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) oder über seinen Blog die finden Sie unter [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Besonderen Dank an

Diese Reihe von Lernprogrammen wurde durch viele nützliche Bearbeiter überprüft. Lead Prüfer für dieses Lernprogramm wurden E.R. Gärtner ein, Dennis Patterson und Dan Jagers. Meine bevorstehende MSDN-Artikel Überprüfen von Interesse? Wenn dies der Fall ist, löschen Sie mich zeilenweise [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[Nächste](using-templatefields-in-the-gridview-control-cs.md)
