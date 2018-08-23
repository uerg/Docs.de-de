---
uid: web-forms/overview/data-access/custom-formatting/displaying-summary-information-in-the-gridview-s-footer-cs
title: Anzeigen von Zusammenfassungsinformationen im GridView Fuß (c#) | Microsoft-Dokumentation
author: rick-anderson
description: Zusammenfassende Informationen wird häufig am unteren Rand des Berichts in eine Zusammenfassungszeile angezeigt. Das GridView-Steuerelement kann eine Fußzeile enthalten, in die Zellen Pr können...
ms.author: riande
ms.date: 03/31/2010
ms.assetid: d50edc31-9286-4c6a-8635-be09e72752a4
msc.legacyurl: /web-forms/overview/data-access/custom-formatting/displaying-summary-information-in-the-gridview-s-footer-cs
msc.type: authoredcontent
ms.openlocfilehash: 76df8ea925f4485b52090723b2f0a37b25f7e684
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2018
ms.locfileid: "41836438"
---
<a name="displaying-summary-information-in-the-gridviews-footer-c"></a>Anzeigen von Zusammenfassungsinformationen im GridView Fuß (c#)
====================
durch [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Beispiel-App herunter](http://download.microsoft.com/download/9/6/9/969e5c94-dfb6-4e47-9570-d6d9e704c3c1/ASPNET_Data_Tutorial_15_CS.exe) oder [PDF-Datei herunterladen](displaying-summary-information-in-the-gridview-s-footer-cs/_static/datatutorial15cs1.pdf)

> Zusammenfassende Informationen wird häufig am unteren Rand des Berichts in eine Zusammenfassungszeile angezeigt. Das GridView-Steuerelement kann eine Fußzeile enthalten, in die Zellen wir programmgesteuert aggregierte Daten einfügen können. In diesem Tutorial sehen wir, wie Aggregieren von Daten in dieser Fußzeile angezeigt wird.


## <a name="introduction"></a>Einführung

Ein Benutzer kann nicht nur jedes der Preise für die Produkte, die im Lager vorrätigen Stückzahl, Einheiten auf Reihenfolge und Neuanordnen von Stufen, auch interessant, aggregierte Informationen, z. B. den durchschnittlichen Preis, der die Gesamtzahl der Einheiten im Lager, und so weiter. Eine solche Zusammenfassung wird häufig am unteren Rand des Berichts in eine Zusammenfassungszeile angezeigt. Das GridView-Steuerelement kann eine Fußzeile enthalten, in die Zellen wir programmgesteuert aggregierte Daten einfügen können.

Diese Aufgabe zeigt drei Herausforderungen:

1. Konfigurieren der GridView, um die Footerzeile angezeigt
2. Bestimmen die Zusammenfassungsdaten; d. h. berechnen wie wir den durchschnittlichen Preis oder die Summe der Einheiten im Lager?
3. Die Zusammenfassungsdaten einfügen in die entsprechenden Zellen der Footerzeile

In diesem Tutorial sehen wir, wie Sie diese Herausforderungen zu bewältigen. Insbesondere erstellen wir eine Seite, die die Kategorien in einer Dropdownliste mit der ausgewählten Kategorie-Produkten, die in einer GridView-Ansicht angezeigt werden aufgelistet. Das GridView wird eine Fußzeile enthalten, die den Durchschnittspreis und die Gesamtzahl der Einheiten im Lager und Reihenfolge für Produkte in dieser Kategorie angezeigt.


[![Zusammenfassende Informationen wird in des GridView Fußzeile angezeigt.](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image2.png)](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image1.png)

**Abbildung 1**: Zusammenfassung der Informationen in des GridView Fußzeile angezeigt ([klicken Sie, um das Bild in voller Größe anzeigen](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image3.png))


Dieses Tutorial, mit der Kategorie, Produkte Master/Detail-Schnittstelle, baut auf den weiter oben behandelten Begriffen [Master/Detail-Filtern mit einer DropDownList](../masterdetail/master-detail-filtering-with-a-dropdownlist-cs.md) Tutorial. Wenn Sie noch nicht mit dem früheren Tutorial gearbeitet haben, machen Sie dies vor dem Fortsetzen dieses.

## <a name="step-1-adding-the-categories-dropdownlist-and-products-gridview"></a>Schritt 1: Hinzufügen von DropDownList für Kategorien und Produkte GridView

Bevor betreffen selbst, mit dem Hinzufügen von Zusammenfassungsinformationen zu den GridView Fuß, lassen Sie uns einfach erstellen Sie zuerst das Master/Detail-Berichts. Sobald wir im ersten Schritt abgeschlossen haben, suchen wir an, wie zusammengefasste Daten enthalten.

Öffnen Sie zunächst die `SummaryDataInFooter.aspx` auf der Seite die `CustomFormatting` Ordner. Fügen Sie einem DropDownList-Steuerelement hinzu, und legen Sie dessen `ID` zu `Categories`. Klicken Sie dann auf den Link "Datenquelle auswählen" aus der Dropdownlistes Smarttag und zum Hinzufügen einer neuen, mit dem Namen "ObjectDataSource" opt `CategoriesDataSource` aufruft, die die `CategoriesBLL` Klasse `GetCategories()` Methode.


[![Fügen Sie eine neue, mit dem Namen CategoriesDataSource "ObjectDataSource"](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image5.png)](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image4.png)

**Abbildung 2**: Hinzufügen einer neuen "ObjectDataSource" mit dem Namen `CategoriesDataSource` ([klicken Sie, um das Bild in voller Größe anzeigen](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image6.png))


[![Haben Sie dem ObjectDataSource-Steuerelement GetCategories()-Methode der CategoriesBLL-Klasse aufrufen.](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image8.png)](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image7.png)

**Abbildung 3**: haben, rufen Sie das ObjectDataSource-Steuerelement die `CategoriesBLL` Klasse `GetCategories()` Methode ([klicken Sie, um das Bild in voller Größe anzeigen](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image9.png))


Nach der Konfiguration dem ObjectDataSource-Steuerelement, gibt der Assistent, wir DropDownLists-Datenquellen, die aus dem müssen wir angeben welche Datenfeldwert Assistenten angezeigt werden sollen und welche auf den Wert des der DropDownList entsprechenmuss`ListItem` s. Haben die `CategoryName` angezeigte Feld und die Verwendung der `CategoryID` als Wert.


[![Verwenden Sie die CategoryID-Felder und "CategoryName" als Wert für die ListItems und Text](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image11.png)](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image10.png)

**Abbildung 4**: Verwenden der `CategoryName` und `CategoryID` von Feldern nach der `Text` und `Value` für die `ListItem` s, bzw. ([klicken Sie, um das Bild in voller Größe anzeigen](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image12.png))


An diesem Punkt haben wir einem DropDownList-Steuerelement (`Categories`), die im System sind die Kategorien aufgeführt. Nun müssen wir einer GridView-Ansicht hinzufügen, die diese Produkte aufgeführt sind, die der ausgewählten Kategorie gehören. Bevor wir, allerdings tun können Sie das AutoPostBack aktivieren Kontrollkästchen in der Dropdownlistes Smarttag. Siehe die *Master/Detail-Filtern mit einer DropDownList* Tutorial durch Festlegen des DropDownList `AutoPostBack` Eigenschaft, um `true` die Seite wird bereitgestellt werden, wieder jedes Mal die DropDownList-Wert geändert wird. Dies bewirkt die GridView aktualisiert werden, zeigt dieser Produkte für die neu ausgewählte Kategorie. Wenn die `AutoPostBack` -Eigenschaftensatz auf `false` (Standardeinstellung), ändern die Kategorie wird nicht dazu führen, dass einen Postback und daher nicht die aufgeführten Produkte aktualisiert.


[![Aktivieren Sie das AutoPostBack Kontrollkästchen aktivieren, in der Dropdownliste des Smarttag](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image14.png)](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image13.png)

**Abbildung 5**: das Kontrollkästchen aktivieren AutoPostBack in der Dropdownlistes Smarttag ([klicken Sie, um das Bild in voller Größe anzeigen](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image15.png))


Fügen Sie ein GridView-Steuerelement auf der Seite hinzu, um die Produkte für die ausgewählte Kategorie anzuzeigen. Legen Sie des GridView `ID` zu `ProductsInCategory` und binden sie an eine neue, mit dem Namen "ObjectDataSource" `ProductsInCategoryDataSource`.


[![Fügen Sie eine neue, mit dem Namen ProductsInCategoryDataSource "ObjectDataSource"](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image17.png)](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image16.png)

**Abbildung 6**: Hinzufügen einer neuen "ObjectDataSource" mit dem Namen `ProductsInCategoryDataSource` ([klicken Sie, um das Bild in voller Größe anzeigen](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image18.png))


Dem ObjectDataSource-Steuerelement so konfigurieren, dass sie ruft die `ProductsBLL` Klasse `GetProductsByCategoryID(categoryID)` Methode.


[![Haben Sie dem Aufrufen der Methode GetProductsByCategoryID(categoryID) ObjectDataSource-Steuerelement](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image20.png)](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image19.png)

**Abbildung 7**: haben Sie die "ObjectDataSource" Aufrufen der `GetProductsByCategoryID(categoryID)` Methode ([klicken Sie, um das Bild in voller Größe anzeigen](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image21.png))


Da die `GetProductsByCategoryID(categoryID)` Methode nimmt einen Eingabeparameter, im letzten Schritt des Assistenten können wir die Quelle der Wert des Parameters angeben. Um diese Produkte aus der ausgewählten Kategorie anzuzeigen, müssen Sie den Parameter, die mithilfe von Pull aus der `Categories` DropDownList.


[![Rufen Sie die CategoryID Parameterwert aus DropDownList Kategorien ausgewählt](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image23.png)](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image22.png)

**Abbildung 8**: Abrufen der *`categoryID`* Parameterwert aus der Dropdownliste für die ausgewählten Kategorien ([klicken Sie, um das Bild in voller Größe anzeigen](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image24.png))


Nach Abschluss des Assistenten wird die GridView ein BoundField für jedes der Produkteigenschaften haben. Lassen Sie uns, damit nur diese BoundFields bereinigen die `ProductName`, `UnitPrice`, `UnitsInStock`, und `UnitsOnOrder` BoundFields werden angezeigt. Alle Einstellungen auf den verbleibenden BoundFields hinzufügen können (z. B. beim Formatieren der `UnitPrice` als Währung). Nach diesen Änderungen sollte GridView deklarative Markup etwa wie folgt aussehen:


[!code-aspx[Main](displaying-summary-information-in-the-gridview-s-footer-cs/samples/sample1.aspx)]

An diesem Punkt haben wir einen voll funktionsfähigen Master/Detail-Bericht mit der Name, Preis, im Lager vorrätigen Stückzahl und Einheiten modelladministratorberechtigung für diese Produkte, die der ausgewählten Kategorie gehören.


[![Rufen Sie die CategoryID Parameterwert aus DropDownList Kategorien ausgewählt](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image26.png)](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image25.png)

**Abbildung 9**: Abrufen der *`categoryID`* Parameterwert aus der Dropdownliste für die ausgewählten Kategorien ([klicken Sie, um das Bild in voller Größe anzeigen](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image27.png))


## <a name="step-2-displaying-a-footer-in-the-gridview"></a>Schritt 2: Anzeigen eine Fußzeile in den GridView-Ansicht

Das GridView-Steuerelement kann sowohl eine Kopf- und Fußzeile Zeile anzeigen. Diese Zeilen werden angezeigt, abhängig von den Werten der der `ShowHeader` und `ShowFooter` Eigenschaften mit `ShowHeader` Standardwert `true` und `ShowFooter` zu `false`. Legen Sie die GridView einfach eine Fußzeile einschließt seine `ShowFooter` Eigenschaft `true`.


[![GridView ShowFooter-Eigenschaft auf True festgelegt.](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image29.png)](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image28.png)

**Abbildung 10**: Legen Sie des GridView `ShowFooter` Eigenschaft `true` ([klicken Sie, um das Bild in voller Größe anzeigen](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image30.png))


Der Fußzeile verfügt eine Zelle, für jedes der Felder in den GridView-Ansicht definiert; Diese Zellen sind jedoch standardmäßig leer. Nehmen Sie einen Moment Zeit, um unseren Fortschritt in einem Browser anzuzeigen. Mit der `ShowFooter` jetzt-eigenschaftseinstellung `true`, GridView enthält eine leere Fußzeile.


[![Das GridView-jetzt umfasst eine Fußzeile](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image32.png)](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image31.png)

**Abbildung 11**: GridView enthält jetzt eine Fußzeile ([klicken Sie, um das Bild in voller Größe anzeigen](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image33.png))


Die Footerzeile in Abbildung 11 hervorzuheben nicht, wie sie einen weißen Hintergrund hat. Erstellen wir eine `FooterStyle` CSS-Klasse `Styles.css` , die einem dunkelroten Hintergrund gibt an, und konfigurieren Sie dann die `GridView.skin` Skin-Datei in die `DataWebControls` Design zuweisen dieser CSS-Klasse an der GridView `FooterStyle`des `CssClass` Eigenschaft. Wenn Sie über Designs zu auffrischen möchten, verweisen zurück auf die [Anzeigen von Daten mit dem ObjectDataSource-Steuerelement](../basic-reporting/displaying-data-with-the-objectdatasource-cs.md) Tutorial.

Starten, indem die folgende CSS-Klasse hinzufügen `Styles.css`:


[!code-css[Main](displaying-summary-information-in-the-gridview-s-footer-cs/samples/sample2.css)]

Die `FooterStyle` CSS-Klasse ähnelt in der Formatvorlage, die die `HeaderStyle` Klasse, obwohl die `HeaderStyle`die Hintergrundfarbe ist dunkler Besonderheit und der Text fett formatiert angezeigt wird. Darüber hinaus ist der Text in der Fußzeile rechtsbündig ausgerichtet, während des Headers Text zentriert ist.

Um diese CSS-Klasse alle GridView Fuß zuzuordnen, öffnen Sie als Nächstes die `GridView.skin` Datei die `DataWebControls` Design und legen die `FooterStyle`des `CssClass` Eigenschaft. Nach dem Hinzufügen sollte der Datei Markup aussehen:


[!code-aspx[Main](displaying-summary-information-in-the-gridview-s-footer-cs/samples/sample3.aspx)]

Wie der Screenshot unten zeigt, wird diese Änderung ermöglicht die Fußzeile noch besser eindeutig.


[![GridView Fußzeile verfügt jetzt über eine Rötlich Hintergrundfarbe](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image35.png)](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image34.png)

**Abbildung 12**: der GridView Fußzeile verfügt jetzt über eine Rötlich Hintergrundfarbe ([klicken Sie, um das Bild in voller Größe anzeigen](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image36.png))


## <a name="step-3-computing-the-summary-data"></a>Schritt 3: Berechnen von den zusammengefassten Daten

Mit den GridView Fuß angezeigt ist die nächste Herausforderung uns um Zusammenfassungsdaten zu berechnen. Es gibt zwei Möglichkeiten, diese aggregierte Informationen zu berechnen:

1. Über eine SQL-Abfrage konnte, schicken wir eine weitere Abfrage mit der Datenbank, um die Zusammenfassungsdaten für eine bestimmte Kategorie zu berechnen. SQL umfasst eine Reihe von Aggregatfunktionen zusammen mit einem `GROUP BY` Klausel, um die Daten anzugeben, in dem die Daten zusammengefasst werden soll. Die folgende SQL-Abfrage wird wieder die benötigte Informationen übereinstimmt:  

    [!code-sql[Main](displaying-summary-information-in-the-gridview-s-footer-cs/samples/sample4.sql)]

    Natürlich nicht empfehlenswert Geben Sie diese Abfrage direkt aus der `SummaryDataInFooter.aspx` Seite, sondern erstellen eine Methode in der `ProductsTableAdapter` und `ProductsBLL`.
2. Diese Informationen zu berechnen, wie sie an die GridView hinzugefügt wird wie unter [benutzerdefinierte Formatierung basierend auf Daten](custom-formatting-based-upon-data-cs.md) Tutorial GridView `RowDataBound` -Ereignishandler wird einmal für jede an die GridView nach der hinzugefügten Zeile ausgelöst. die wurde Datenbindung. Durch Erstellen eines ereignishandlers für dieses Ereignis können wir speichern einen laufenden Gesamtbetrag der Werte, die wir möchten zu aggregieren. Nach dem letzten wurde Datenzeile an die GridView gebunden haben die Gesamtsummen sowie die erforderlichen Informationen zum Berechnen des Durchschnitts.

Ich setze den zweiten Ansatz in der Regel auf, wie eine Fahrt auf die Datenbank und der Aufwand für die Zusammenfassung Funktionalität zu implementieren, in der Datenzugriffsebene und den Geschäftslogikebene gespeichert, aber beide Vorgehensweisen ausreichen würde. Für dieses Tutorial sehen wir die zweite Option verwenden und Verwalten von die laufende Summe mithilfe der `RowDataBound` -Ereignishandler.

Erstellen Sie eine `RowDataBound` -Ereignishandler für das GridView GridView im Designer auswählen, indem auf das Blitzsymbol aus dem Fenster "Eigenschaften" klicken, und doppelklicken dem `RowDataBound` Ereignis. Dies erstellt einen neuen Ereignishandler mit dem Namen `ProductsInCategory_RowDataBound` in die `SummaryDataInFooter.aspx` Seite des Code-Behind-Klasse.


[!code-csharp[Main](displaying-summary-information-in-the-gridview-s-footer-cs/samples/sample5.cs)]

Um einen laufenden insgesamt gewährleisten müssen wir Variablen außerhalb des Bereichs, der den Ereignishandler zu definieren. Erstellen Sie die folgenden vier auf Seitenebene Variablen:

- `_totalUnitPrice`, des Typs `decimal`
- `_totalNonNullUnitPriceCount`, des Typs `int`
- `_totalUnitsInStock`, des Typs `int`
- `_totalUnitsOnOrder`, des Typs `int`

Schreiben Sie als Nächstes den Code, um diese drei Variablen zu erhöhen, für jede Datenzeile in ermittelt die `RowDataBound` -Ereignishandler.


[!code-csharp[Main](displaying-summary-information-in-the-gridview-s-footer-cs/samples/sample6.cs)]

Die `RowDataBound` Ereignishandler startet, indem sichergestellt wird, dass wir mit einer DataRow zu tun haben. Sobald, eingerichtet ist, die `Northwind.ProductsRow` -Instanz, die gerade gebunden war die `GridViewRow` -Objekt in `e.Row` befindet sich in der Variablen `product`. Weiter, ausgeführte insgesamt Variablen durch entsprechende Werte für das aktuelle Produkt erhöht werden (vorausgesetzt, dass sie eine Datenbank enthalten, nicht `NULL` Wert). Wir behalten die Übersicht der ausgeführten `UnitPrice` gesamtspeicherlimit "und" die Anzahl der nicht -`NULL` `UnitPrice` Datensätze aus, da der Durchschnittspreis der Quotient diese beiden Zahlen ist.

## <a name="step-4-displaying-the-summary-data-in-the-footer"></a>Schritt 4: Anzeigen von Zusammenfassungsdaten in der Fußzeile

Im letzte Schritt werden mit den Zusammenfassungsdaten summiert es in den GridView Fußzeile angezeigt. Diese Aufgabe auch kann erreicht werden, programmgesteuert über die `RowDataBound` -Ereignishandler. Bedenken Sie, dass die `RowDataBound` Ereignishandler ausgelöst wird, für die *jeder* Zeile, die an die GridView, einschließlich der Fußzeile gebunden ist. Aus diesem Grund können wir unsere-Ereignishandler zum Anzeigen der Daten in der Fußzeile, die mit dem folgenden Code erweitern:


[!code-csharp[Main](displaying-summary-information-in-the-gridview-s-footer-cs/samples/sample7.cs)]

Da der Fußzeile an die GridView hinzugefügt wird, nachdem alle Datenzeilen hinzugefügt wurden, können wir sicher sein, dass mit der Zeit können sich jetzt die Zusammenfassungsdaten in der Fußzeile angezeigt, die die laufende Summe Berechnungen werden abgeschlossen haben. Der letzte Schritt ist, um diese Werte in der Fußzeile der Zellen festlegen.

Verwenden Sie zum Anzeigen von Text in einem bestimmten Fußzeilenzelle `e.Row.Cells[index].Text = value`, wobei die `Cells` Indizierung beginnt bei 0. Der folgende Code berechnet den durchschnittlichen Preis (der Gesamtpreis geteilt durch die Anzahl der Produkte) und zusammen mit der Gesamtanzahl der Einheiten im Bestand und Einheiten auf der Reihenfolge, in die Fußzeilenzellen der entsprechenden der GridView angezeigt.


[!code-csharp[Main](displaying-summary-information-in-the-gridview-s-footer-cs/samples/sample8.cs)]

Abbildung 13 zeigt den Bericht aus, nachdem dieser Code hinzugefügt wurde. Beachten Sie die `ToString("c")` bewirkt, dass der Durchschnittspreis zusammenfassende Informationen, z. B. eine Währung formatiert werden.


[![GridView Fußzeile verfügt jetzt über eine Rötlich Hintergrundfarbe](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image38.png)](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image37.png)

**Abbildung 13**: der GridView Fußzeile verfügt jetzt über eine Rötlich Hintergrundfarbe ([klicken Sie, um das Bild in voller Größe anzeigen](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image39.png))


## <a name="summary"></a>Zusammenfassung

Anzeigen von zusammenfassenden Daten ist eine verbreitete Anforderung für den Bericht und das GridView-Steuerelement erleichtert Ihnen solche Informationen in der Fußzeile enthalten. Die Footerzeile angezeigt wird bei der GridView `ShowFooter` -Eigenschaftensatz auf `true` und kann den Text in deren Zellen, legen Sie programmgesteuert über die `RowDataBound` -Ereignishandler. Die Zusammenfassungsdaten Computing kann entweder durch erneutes Abfragen der Datenbank oder mithilfe von Code in die CodeBehind-Klasse der ASP.NET-Seite programmgesteuert die zusammengefassten Daten berechnet erfolgen.

Dieses Lernprogramm schließt unserer Untersuchung der benutzerdefinierte Formatierung mit der GridView, DetailsView oder FormView-Steuerelemente. Unserem nächsten Tutorial startet von unserem Seminar einfügen, aktualisieren und Löschen von Daten, die mit diesen gleichen-Steuerelementen.

Viel Spaß beim Programmieren!

## <a name="about-the-author"></a>Der Autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor von sieben Büchern zu ASP/ASP.NET und Gründer von [4GuysFromRolla.com](http://www.4guysfromrolla.com), arbeitet mit Microsoft-Web-Technologien seit 1998. Er ist als ein unabhängiger Berater, Schulungsleiter und Autor. Sein neueste Buch wird [*Sams Schulen selbst ASP.NET 2.0 in 24 Stunden*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Er ist unter [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) oder über seinen Blog finden Sie unter [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

> [!div class="step-by-step"]
> [Zurück](using-the-formview-s-templates-cs.md)
> [Weiter](custom-formatting-based-upon-data-vb.md)
