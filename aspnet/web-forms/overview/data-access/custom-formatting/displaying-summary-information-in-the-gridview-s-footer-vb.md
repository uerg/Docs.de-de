---
uid: web-forms/overview/data-access/custom-formatting/displaying-summary-information-in-the-gridview-s-footer-vb
title: "Anzeigen von Zusammenfassungsinformationen in die GridView Fußzeile (VB) | Microsoft Docs"
author: rick-anderson
description: "Zusammenfassende Informationen wird häufig am unteren Rand des Berichts in eine Summenzeile angezeigt. Des GridView-Steuerelements kann eine Fußzeile enthalten, deren Zellen Wir Pr können..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/31/2010
ms.topic: article
ms.assetid: 41c818b7-603a-402b-8847-890a63547b6f
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/custom-formatting/displaying-summary-information-in-the-gridview-s-footer-vb
msc.type: authoredcontent
ms.openlocfilehash: e5b7e39a44d43a857c62842ea3e1dddcacf05c9b
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="displaying-summary-information-in-the-gridviews-footer-vb"></a>Anzeigen von Zusammenfassungsinformationen in die GridView Fußzeile (VB)
====================
durch [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Beispiel-App herunterladen](http://download.microsoft.com/download/5/7/0/57084608-dfb3-4781-991c-407d086e2adc/ASPNET_Data_Tutorial_15_VB.exe) oder [PDF herunterladen](displaying-summary-information-in-the-gridview-s-footer-vb/_static/datatutorial15vb1.pdf)

> Zusammenfassende Informationen wird häufig am unteren Rand des Berichts in eine Summenzeile angezeigt. Des GridView-Steuerelements kann eine Fußzeile enthalten, in die Zellen wir programmgesteuert Aggregatdaten einfügen können. In diesem Lernprogramm sehen wir die Vorgehensweise beim Anzeigen von aggregierten Daten in dieser Zeile Fußzeile.


## <a name="introduction"></a>Einführung

Zusätzlich zum Anzeigen aller Preise der Produkte, Einheiten im Lager, Einheiten Order-und Ebenen neu anordnen, könnte ein Benutzer auch interessant, aggregierte Informationen, z. B. den durchschnittlichen Preis, für die Anzahl der Einheiten im Lager, und so weiter. Diese Zusammenfassungsinformationen wird häufig am unteren Rand des Berichts in eine Summenzeile angezeigt. Des GridView-Steuerelements kann eine Fußzeile enthalten, in die Zellen wir programmgesteuert Aggregatdaten einfügen können.

Dieser Task stellt uns mit drei Herausforderungen:

1. Konfigurieren die GridView seine Fußzeile anzeigen
2. Bestimmen die Zusammenfassungsdaten; d. h. berechnen wie wir den Durchschnittspreis oder die Summe der Einheiten im Lager?
3. Räumen in den zusammengefassten Daten in die entsprechenden Zellen der Fußzeile

In diesem Lernprogramm sehen wir, wie diese Herausforderungen. Insbesondere erstellen wir eine Seite, die die Kategorien in einer Dropdownliste mit der ausgewählten Kategorie Produkte angezeigt, die in einem GridView auflistet. Die GridView wird eine Fußzeile enthalten, die den Durchschnittspreis und die gesamte Anzahl der Einheiten im Lager und modelladministratorberechtigung für Produkte in dieser Kategorie angezeigt.


[![Zusammenfassende Informationen wird in der GridView Fußzeile angezeigt.](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image2.png)](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image1.png)

**Abbildung 1**: Zusammenfassung der Informationen in der GridView Fußzeile angezeigt ([klicken Sie hier, um das Bild in voller Größe angezeigt](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image3.png))


Dieses Lernprogramm mit seine Kategorie Produkte Master/Detail-Schnittstelle, von der oben behandelten Begriffen aufbaut [Master/Detail-Filtern mit einer DropDownList](../masterdetail/master-detail-filtering-with-a-dropdownlist-cs.md) Lernprogramm. Wenn Sie noch nicht über die früheren Lernprogramm gearbeitet haben, holen Sie dies vor dem Fortsetzen hindurchscheinen.

## <a name="step-1-adding-the-categories-dropdownlist-and-products-gridview"></a>Schritt 1: Hinzufügen der DropDownList Kategorien und die Produkte GridView

Vor dem bereit, die uns mit der GridView Fußzeile zusammenfassende Informationen hinzugefügt, zuerst einfach erstellen Sie den Master-/Detail-Bericht. Betrachten wir Sobald wir im ersten Schritt abgeschlossen haben, wie Zusammenfassungsdaten eingeschlossen.

Öffnen Sie zunächst die `SummaryDataInFooter.aspx` auf der Seite der `CustomFormatting` Ordner. Hinzufügen eines DropDownList-Steuerelements, und legen Sie dessen `ID` auf `Categories`. Klicken Sie dann auf den Link Datenquelle auswählen, aus der DropDownList-Smarttag und wahlweise eine neue, mit dem Namen ObjectDataSource hinzufügen `CategoriesDataSource` aufruft, die die `CategoriesBLL` Klasse `GetCategories()` Methode.


[![Fügen Sie eine neue, mit dem Namen CategoriesDataSource ObjectDataSource hinzu](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image5.png)](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image4.png)

**Abbildung 2**: Hinzufügen einer neuen ObjectDataSource namens `CategoriesDataSource` ([klicken Sie hier, um das Bild in voller Größe angezeigt](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image6.png))


[![Aufrufen der CategoriesBLL Klassenmethode GetCategories() ObjectDataSource haben](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image8.png)](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image7.png)

**Abbildung 3**: haben das ObjectDataSource Aufrufen der `CategoriesBLL` Klasse `GetCategories()` Methode ([klicken Sie hier, um das Bild in voller Größe angezeigt](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image9.png))


Nach dem Konfigurieren der ObjectDataSource, gibt der Assistent uns der DropDownList Datenquellenkonfiguration von dem an, welche Datenfeldwert müssen Assistenten angezeigt werden sollen und welche auf den Wert von der DropDownList entsprechensoll`ListItem` s. Haben die `CategoryName` Feld angezeigt wird und die Verwendung der `CategoryID` als Wert.


[![Verwenden Sie die CategoryName und CategoryID Felder als Text und der Wert für die Listenelemente](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image11.png)](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image10.png)

**Abbildung 4**: Verwenden der `CategoryName` und `CategoryID` von Feldern nach der `Text` und `Value` für die `ListItem` s, bzw. ([klicken Sie hier, um das Bild in voller Größe angezeigt](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image12.png))


An diesem Punkt haben wir eine DropDownList (`Categories`), die im System sind die Kategorien aufgeführt. Jetzt müssen wir eine GridView hinzufügen, die diese Produkte aufgeführt sind, die der ausgewählten Kategorie angehören. Bevor wir, aber tun führen Sie einen Moment Zeit, in der DropDownList-Smarttag AutoPostBack aktivieren Kontrollkästchen. Wie in beschrieben die *Master/Detail-Filtern mit einer DropDownList* Lernprogramm durch Festlegen der DropDownList `AutoPostBack` Eigenschaft `True` die Seite werden im Verteilungsverlauf wieder jedes Mal der DropDownList-Wert geändert wird. Dies bewirkt die GridView aktualisiert werden, die Produkte für die neu ausgewählte Kategorie angezeigt. Wenn die `AutoPostBack` -Eigenschaftensatz auf `False` (Standard), ändern die Kategorie führen nicht zu einem Postback und werden daher die aufgelisteten Produkte nicht aktualisiert.


[![Aktivieren Sie das Kontrollkästchen aktivieren AutoPostBack, in der DropDownList-Smarttag](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image14.png)](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image13.png)

**Abbildung 5**: Überprüfen Sie das Kontrollkästchen zum Aktivieren des AutoPostBack in der DropDownList-Smarttag ([klicken Sie hier, um das Bild in voller Größe angezeigt](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image15.png))


Hinzufügen eines GridView-Steuerelements auf der Seite um die Produkte für die ausgewählte Kategorie anzuzeigen. Legen Sie der GridView `ID` auf `ProductsInCategory` und binden es an eine neue ObjectDataSource mit dem Namen `ProductsInCategoryDataSource`.


[![Fügen Sie eine neue, mit dem Namen ProductsInCategoryDataSource ObjectDataSource hinzu](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image17.png)](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image16.png)

**Abbildung 6**: Hinzufügen einer neuen ObjectDataSource namens `ProductsInCategoryDataSource` ([klicken Sie hier, um das Bild in voller Größe angezeigt](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image18.png))


Das ObjectDataSource so konfigurieren, dass er ruft die `ProductsBLL` Klasse `GetProductsByCategoryID(categoryID)` Methode.


[![Haben Sie das Aufrufen der Methode GetProductsByCategoryID(categoryID) ObjectDataSource](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image20.png)](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image19.png)

**Abbildung 7**: haben Sie die ObjectDataSource Aufrufen der `GetProductsByCategoryID(categoryID)` Methode ([klicken Sie hier, um das Bild in voller Größe angezeigt](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image21.png))


Da die `GetProductsByCategoryID(categoryID)` Methode akzeptiert einen Eingabeparameter im letzten Schritt des Assistenten können wir die Quelle der Wert des Parameters angeben. Um Produkte aus der ausgewählten Kategorie angezeigt, haben Sie den Parameter aus herausgezogen werden die `Categories` DropDownList.


[![Die CategoryID-Parameterwert aus der ausgewählten Kategorien DropDownList abrufen](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image23.png)](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image22.png)

**Abbildung 8**: Abrufen der  *`categoryID`*  Parameterwert aus der ausgewählten Kategorien DropDownList ([klicken Sie hier, um das Bild in voller Größe angezeigt](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image24.png))


Nach Abschluss des Assistenten müssen die GridView ein BoundField für jede Produkt-Eigenschaft. Bereinigen Sie wir diese BoundFields so, dass nur die `ProductName`, `UnitPrice`, `UnitsInStock`, und `UnitsOnOrder` BoundFields werden angezeigt. Wahlweise können Sie alle Einstellungen auf den verbleibenden BoundFields hinzufügen (z. B. beim Formatieren der `UnitPrice` als Währung). Nachdem diese Änderungen vorgenommen wurden, sollte die GridView deklarative Markup etwa wie folgt aussehen:


[!code-aspx[Main](displaying-summary-information-in-the-gridview-s-footer-vb/samples/sample1.aspx)]

Zu diesem Zeitpunkt haben wir einen voll funktionsfähige Master/Detail-Bericht, der den Namen, Einzelpreis Einheiten im Lager und Einheiten modelladministratorberechtigung für diese Produkte anzeigt, die der ausgewählten Kategorie angehören.


[![Die CategoryID-Parameterwert aus der ausgewählten Kategorien DropDownList abrufen](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image26.png)](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image25.png)

**Abbildung 9**: Abrufen der  *`categoryID`*  Parameterwert aus der ausgewählten Kategorien DropDownList ([klicken Sie hier, um das Bild in voller Größe angezeigt](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image27.png))


## <a name="step-2-displaying-a-footer-in-the-gridview"></a>Schritt 2: Anzeigen eine Fußzeile in die GridView

GridView-Steuerelements kann sowohl eine Kopf- und Fußzeile Zeile angezeigt. Diese Zeilen werden angezeigt, abhängig von den Werten von der `ShowHeader` und `ShowFooter` Eigenschaften, jeweils mit `ShowHeader` direktionales `True` und `ShowFooter` auf `False`. Legen Sie die GridView einfach eine Fußzeile einschließt seine `ShowFooter` Eigenschaft `True`.


[![Der GridView ShowFooter-Eigenschaft auf "true" festgelegt](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image29.png)](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image28.png)

**Abbildung 10**: Legen Sie der GridView `ShowFooter` Eigenschaft `True` ([klicken Sie hier, um das Bild in voller Größe angezeigt](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image30.png))


Die Fußzeile verfügt eine Zelle für jedes der Felder in der GridView definiert; Diese Zellen sind jedoch standardmäßig leer. Nehmen Sie einen Moment Zeit, um unseren Fortschritt in einem Browser anzuzeigen. Mit der `ShowFooter` Eigenschaft jetzt festgelegt werden, um `True`, GridView enthält eine leere Fußzeile.


[![Die GridView jetzt umfasst eine Fußzeile](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image32.png)](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image31.png)

**Abbildung 11**: GridView enthält nun eine Fußzeile ([klicken Sie hier, um das Bild in voller Größe angezeigt](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image33.png))


Die Fußzeile in Abbildung 11 hervorzuheben nicht, er einen weißen Hintergrund aufweist. Erstellen wir eine `FooterStyle` CSS-Klasse `Styles.css` , die dunkle rotem Hintergrund gibt an, und konfigurieren Sie dann die `GridView.skin` Designdatei in der `DataWebControls` Design zuweisen dieses CSS-Klasse an der GridView `FooterStyle`des `CssClass` Eigenschaft. Wenn Sie über Designs Kenntnisse auffrischen möchten, verweisen Sie zurück auf die [Anzeigen von Daten mit der ObjectDataSource](../basic-reporting/displaying-data-with-the-objectdatasource-cs.md) Lernprogramm.

Starten, indem Sie die folgenden CSS-Klasse hinzufügen `Styles.css`:


[!code-css[Main](displaying-summary-information-in-the-gridview-s-footer-vb/samples/sample2.css)]

Die `FooterStyle` CSS-Klasse ähnelt in Stil in die `HeaderStyle` Klasse, obwohl die `HeaderStyle`des Hintergrundfarbe Besonderheit dunkler und der Text wird in fett formatierter Schrift angezeigt. Darüber hinaus wird der Text in der Fußzeile rechtsbündig während des Headers Text zentriert ist.

Um jede GridView Fußzeile dieses CSS-Klasse zuzuordnen, öffnen Sie als Nächstes die `GridView.skin` in der Datei die `DataWebControls` Design und legen die `FooterStyle`des `CssClass` Eigenschaft. Nach dem Hinzufügen sollte der Datei Markup aussehen:


[!code-aspx[Main](displaying-summary-information-in-the-gridview-s-footer-vb/samples/sample3.aspx)]

Diese Änderung ermöglicht wie die Abbildung unten zeigt die Fußzeile abheben mehr.


[![Der GridView Fußzeile verfügt jetzt über eine rötliche Hintergrundfarbe](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image35.png)](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image34.png)

**Abbildung 12**: die GridView Fußzeile verfügt jetzt über eine rötliche Hintergrundfarbe ([klicken Sie hier, um das Bild in voller Größe angezeigt](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image36.png))


## <a name="step-3-computing-the-summary-data"></a>Schritt 3: Berechnen der Zusammenfassungsdaten

Mit der GridView Fußzeile angezeigt ist der nächste Herausforderung uns wie in den zusammengefassten Daten berechnet. Es gibt zwei Möglichkeiten, diese aggregierte Informationen zu berechnen:

1. Über eine SQL-Abfrage konnte, stellen wir eine weitere Abfrage auf die Datenbank an die Zusammenfassungsdaten für eine bestimmte Kategorie zu berechnen. SQL umfasst eine Reihe von Aggregatfunktionen zusammen mit einem `GROUP BY` -Klausel, um die Daten anzugeben, über den die Daten zusammengefasst werden sollen. Die folgende SQL-Abfrage würde die benötigte Informationen wiederherzustellen:  

    [!code-sql[Main](displaying-summary-information-in-the-gridview-s-footer-vb/samples/sample4.sql)]

    Natürlich Sie möchten diese Abfrage direkt aus ausgeben der `SummaryDataInFooter.aspx` Seite, sondern durch das Erstellen einer Methode in der `ProductsTableAdapter` und `ProductsBLL`.
2. Diese Informationen zu berechnen, wie er an die GridView hinzugefügt wird wie in erläutert [benutzerdefinierte Formatierung basierend auf Daten](custom-formatting-based-upon-data-cs.md) Lernprogramm GridView des `RowDataBound` -Ereignishandler wird einmal für jede Zeile hinzugefügt wird, an die GridView nach ausgelöst. seine wurde datengebundene. Erstellen einen Ereignishandler für dieses Ereignis können wir eine laufende speichern Gesamtbetrag der Werte, die wir möchten aggregieren. Nach dem letzten wurde Datenzeile an die GridView gebunden wir die Summen und die Informationen zur Berechnung der durchschnittlichen benötigt haben.

Ich aber in der Regel gibt den zweiten Ansatz wie einem Vorgang auf dem die Datenbank und der Aufwand zur Implementierung der zusammenfassenden Funktionen in der Datenzugriffsebene und Business Logic Layer gespeichert, aber beide Vorgehensweisen ausreichen würde. Für dieses Lernprogramm nehmen wir verwenden Sie die zweite Option und verfolgt die laufende Summe mithilfe der `RowDataBound` -Ereignishandler.

Erstellen einer `RowDataBound` -Ereignishandler für die GridView GridView im Designer auswählen, indem durch Klicken auf das Blitzsymbol über das Eigenschaftenfenster, und doppelklicken dem `RowDataBound` Ereignis. Alternativ können Sie aus den Dropdown-Listen am oberen Rand der ASP.NET Code-Behind-Klassendatei GridView und seine RowDataBound Ereignis auswählen. Dies erstellt einen neuen Ereignishandler namens `ProductsInCategory_RowDataBound` in die `SummaryDataInFooter.aspx` Seite des Code-Behind-Klasse.


[!code-vb[Main](displaying-summary-information-in-the-gridview-s-footer-vb/samples/sample5.vb)]

Um einen laufenden insgesamt verwalten müssen wir Variablen außerhalb des Bereichs der Ereignishandler zu definieren. Erstellen Sie die folgenden vier Seitenebene Variablen:

- `_totalUnitPrice`, des Typs`Decimal`
- `_totalNonNullUnitPriceCount`, des Typs`Integer`
- `_totalUnitsInStock`, des Typs`Integer`
- `_totalUnitsOnOrder`, des Typs`Integer`

Als Nächstes schreiben den Code, um diese drei Variablen zu erhöhen, für jede Datenzeile in ermittelt die `RowDataBound` -Ereignishandler.


[!code-vb[Main](displaying-summary-information-in-the-gridview-s-footer-vb/samples/sample6.vb)]

Die `RowDataBound` Ereignishandler gestartet wird, indem Sie sicherstellen, dass es mit einer DataRow arbeiten. Nach, die hergestellt wurde, wird die `Northwind.ProductsRow` -Instanz, die gerade gebunden war die `GridViewRow` -Objekt in `e.Row` in der Variablen gespeichert ist `product`. Weiter, ausgeführte insgesamt Variablen werden durch die entsprechenden Werte für das aktuelle Produkt inkrementiert (vorausgesetzt, dass sie eine Datenbank enthalten keine `NULL` Wert). Wir Nachverfolgen von sowohl die Ausführung `UnitPrice` gesamtspeicherlimit "und" die Anzahl der nicht -`NULL` `UnitPrice` aufgezeichnet werden, da der Durchschnittspreis der Quotient diese beiden Zahlen ist.

## <a name="step-4-displaying-the-summary-data-in-the-footer"></a>Schritt 4: Anzeigen von zusammengefassten Daten in der Fußzeile

Im letzte Schritt werden mit den Zusammenfassungsdaten summiert ihn in die GridView Fußzeile anzeigen. Diese Aufgabe zu verwenden, kann erreicht werden programmgesteuert über die `RowDataBound` -Ereignishandler. Bedenken Sie, dass die `RowDataBound` -Ereignishandler ausgelöst wird, für die *jeder* Zeile, die an die GridView, einschließlich der Fußzeile gebunden ist. Daher können wir unsere Ereignishandler zum Anzeigen der Daten in der Fußzeile, die mit dem folgenden Code erweitern:


[!code-vb[Main](displaying-summary-information-in-the-gridview-s-footer-vb/samples/sample7.vb)]

Da der Fußzeile an die GridView hinzugefügt wird, nachdem alle Datenzeilen hinzugefügt wurden, können wir sicher sein, dass nach der Zeit können sich jetzt in den zusammengefassten Daten in der Fußzeile angezeigt, die laufende Summe Berechnungen abgeschlossen haben, werden. Der letzte Schritt ist dann diese Werte in der Fußzeile Zellen festlegen.

Verwenden Sie zum Anzeigen von Text in einer bestimmten Zelle `e.Row.Cells(index).Text = value`, wobei die `Cells` Indizierung beginnt bei 0. Der folgende Code berechnet den Durchschnittspreis (der Gesamtpreis dividiert durch die Anzahl der Produkte) und zusammen mit der Gesamtanzahl der Einheiten im Bestand und Einheiten in der Reihenfolge, in der entsprechenden Fußzeilenzellen GridView angezeigt.


[!code-vb[Main](displaying-summary-information-in-the-gridview-s-footer-vb/samples/sample8.vb)]

Abbildung 13 zeigt den Bericht aus, nachdem dieser Code hinzugefügt wurde. Hinweis wie die `ToString("c")` bewirkt, dass der Durchschnittspreis zusammenfassende Informationen, z. B. eine Währung formatiert werden.


[![Der GridView Fußzeile verfügt jetzt über eine rötliche Hintergrundfarbe](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image38.png)](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image37.png)

**Abbildung 13**: die GridView Fußzeile verfügt jetzt über eine rötliche Hintergrundfarbe ([klicken Sie hier, um das Bild in voller Größe angezeigt](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image39.png))


## <a name="summary"></a>Zusammenfassung

Anzeigen von zusammengefassten Daten ist eine häufige Anforderung für den Bericht, und des GridView-Steuerelements erleichtert das solche Informationen in der Fußzeile einschließen. Fußzeile angezeigt wird bei der der GridView `ShowFooter` -Eigenschaftensatz auf `True` und kann den Text in den Zellen programmgesteuert durch Festlegen der `RowDataBound` -Ereignishandler. Berechnen die zusammengefassten Daten kann entweder durch erneutes Abfragen der Datenbank oder mithilfe von Code in die CodeBehind-Klasse der ASP.NET-Seite programmgesteuert die zusammengefassten Daten berechnet erfolgen.

In diesem Lernprogramm wird unsere Untersuchung von benutzerdefinierte Formatierung mit GridView, DetailsView und FormView abgeschlossen. Unser Tutorial weiter startet unsere Untersuchung des einfügen, aktualisieren und Löschen von Daten, die diese gleichen Steuerelemente verwenden.

Viel Spaß beim Programmieren!

## <a name="about-the-author"></a>Informationen zum Autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor von sieben ASP/ASP.NET-Büchern und Gründer von [4GuysFromRolla.com](http://www.4guysfromrolla.com), Microsoft Web-Technologien seit 1998 arbeitet. Scott fungiert als ein unabhängiger Berater, Trainer und Writer. Sein neueste Buch wird [ *Sams Schulen selbst ASP.NET 2.0 in 24 Stunden*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Er die erreicht werden kann, zur [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) oder über seinen Blog die finden Sie unter [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

>[!div class="step-by-step"]
[Zurück](using-the-formview-s-templates-vb.md)
