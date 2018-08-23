---
uid: web-forms/overview/data-access/custom-formatting/using-templatefields-in-the-detailsview-control-vb
title: Verwenden von TemplateFields im DetailsView-Steuerelement (VB) | Microsoft-Dokumentation
author: rick-anderson
description: Außerdem stehen die gleichen von TemplateFields-Funktionen zur Verfügung, mit der GridView mit DetailsView-Steuerelement zur Verfügung. In diesem Tutorial zeigen wir ein Produkt...
ms.author: riande
ms.date: 03/31/2010
ms.assetid: 0b91d5f8-127d-4f6a-b204-f2e2b35ef703
msc.legacyurl: /web-forms/overview/data-access/custom-formatting/using-templatefields-in-the-detailsview-control-vb
msc.type: authoredcontent
ms.openlocfilehash: 1c18e8be25369d6e7d1703e71b80e75adb9f15fa
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2018
ms.locfileid: "41837142"
---
<a name="using-templatefields-in-the-detailsview-control-vb"></a>Verwenden von TemplateFields im DetailsView-Steuerelement (VB)
====================
durch [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Beispiel-App herunter](http://download.microsoft.com/download/5/7/0/57084608-dfb3-4781-991c-407d086e2adc/ASPNET_Data_Tutorial_13_VB.exe) oder [PDF-Datei herunterladen](using-templatefields-in-the-detailsview-control-vb/_static/datatutorial13vb1.pdf)

> Außerdem stehen die gleichen von TemplateFields-Funktionen zur Verfügung, mit der GridView mit DetailsView-Steuerelement zur Verfügung. In diesem Tutorial zeigen wir ein Produkt mit einem DetailsView, die von TemplateFields enthält.


## <a name="introduction"></a>Einführung

Das TemplateField bietet ein höheres Maß an Flexibilität bei der Rendering-Daten als die BoundField-, CheckBoxField, HyperLinkField und anderen feldsteuerungen der Daten. In der [vorherigen Tutorial](using-templatefields-in-the-gridview-control-vb.md) erläutert, verwenden das TemplateField in einer GridView-Ansicht zu:

- Mehrere Feldwerte für die Daten in einer Spalte angezeigt. Insbesondere sowohl die `FirstName` und `LastName` Felder in einer GridView-Spalte zusammengefasst wurden.
- Verwenden Sie einen alternativen Websteuerelement, um ein Datenfeldwert auszudrücken. Erläutert, wie zum Anzeigen der `HiredDate` Wert mithilfe eines Kalendersteuerelements.
- Anzeigen von Statusinformationen, die basierend auf den zugrunde liegenden Daten. Während der `Employees` Tabelle enthält eine Spalte, die die Anzahl der Tage zurückgibt, ein Mitarbeiter für den Auftrag wurde, nicht, wir konnten solche Informationen in den GridView-Beispiel im vorherigen Tutorial mit der Verwendung eines TemplateField und die Methode zur Formatierung angezeigt.

Außerdem stehen die gleichen von TemplateFields-Funktionen zur Verfügung, mit der GridView mit DetailsView-Steuerelement zur Verfügung. In diesem Tutorial zeigen wir ein Produkt mit einem DetailsView, die zwei von TemplateFields enthält. Die erste TemplateField fasst die `UnitPrice`, `UnitsInStock`, und `UnitsOnOrder` Datenfelder in einem DetailsView-Zeile. Die zweite TemplateField zeigt den Wert von der `Discontinued` Feld eine Formatierungsmethode anzeigen, wenn "Ja" verwenden, jedoch `Discontinued` ist `True`, und andernfalls "NO".


[![Zwei von TemplateFields werden verwendet, um die Anzeige anpassen](using-templatefields-in-the-detailsview-control-vb/_static/image2.png)](using-templatefields-in-the-detailsview-control-vb/_static/image1.png)

**Abbildung 1**: zwei von TemplateFields werden verwendet, um die Anzeige anpassen ([klicken Sie, um das Bild in voller Größe anzeigen](using-templatefields-in-the-detailsview-control-vb/_static/image3.png))


Fangen wir an!

## <a name="step-1-binding-the-data-to-the-detailsview"></a>Schritt 1: Binden der Daten zur DetailsView

Wie im vorherigen Tutorial erwähnt, bei der Verwendung von von TemplateFields, es oft am einfachsten ist, beginnen mit dem Erstellen von DetailsView-Steuerelement, das nur BoundFields enthält, und fügen Sie dann neue von TemplateFields oder konvertieren die vorhandenen BoundFields in von TemplateFields als, erforderlich. . Aus diesem Grund zunächst in diesem Tutorial eine DetailsView für die Seite mit dem Designer hinzugefügt, und binden sie an einer ObjectDataSource gegeben, die die Liste der Produkte zurückgibt. Diese Schritte werden eine DetailsView mit BoundFields für jede der des Produkts nicht boolesche Feldern und einem CheckBoxField für das Feld für einen booleschen Wert (eingestellt) erstellt werden.

Öffnen der `DetailsViewTemplateField.aspx` Seite und eine DetailsView aus der Toolbox in den Designer ziehen. Smarttag des DetailsView auswählen, um ein neues ObjectDataSource-Steuerelement hinzuzufügen, die aufruft, die `ProductsBLL` Klasse `GetProducts()` Methode.


[![Fügen Sie ein neues ObjectDataSource-Steuerelement, das die GetProducts()-Methode aufruft](using-templatefields-in-the-detailsview-control-vb/_static/image5.png)](using-templatefields-in-the-detailsview-control-vb/_static/image4.png)

**Abbildung 2**: Fügen Sie ein neues ObjectDataSource-Steuerelement, Invokes der `GetProducts()` Methode ([klicken Sie, um das Bild in voller Größe anzeigen](using-templatefields-in-the-detailsview-control-vb/_static/image6.png))


Entfernen Sie für diesen Bericht die `ProductID`, `SupplierID`, `CategoryID`, und `ReorderLevel` BoundFields. Als Nächstes neu anordnen der BoundFields, damit die `CategoryName` und `SupplierName` BoundFields unmittelbar die `ProductName` BoundField. Sie können die `HeaderText` Eigenschaften und Formatierungseigenschaften für die BoundFields, wie Sie nach Bedarf. Wie mit GridView diese Bearbeitungen BoundField-auf Serverebene über die Felder (Dialogfeld) (Zugriff durch Klicken auf die Felder bearbeiten-Link in DetailsViews-Smarttag) oder mithilfe der deklarativen Syntax ausgeführt werden können. Abschließend löschen, die DetailsView `Height` und `Width` Eigenschaftswerte damit DetailsView kann steuern, um basierend auf der angezeigten Daten zu erweitern, und aktivieren Sie das Kontrollkästchen Paging aktivieren, in das Smarttag.

Nach diesen Änderungen sollte Ihre DetailsView-Steuerelement deklaratives Markup etwa wie folgt aussehen:


[!code-aspx[Main](using-templatefields-in-the-detailsview-control-vb/samples/sample1.aspx)]

Nehmen Sie einen Moment Zeit, um die Seite über einen Browser anzuzeigen. An diesem Punkt sollte ein einzelnes Produkt aufgeführt (Chai) mit Zeilen, die mit den Namen des Produkts, Kategorie, Lieferanten, Preis, im Lager vorrätigen Stückzahl, Einheiten auf Reihenfolge und der nicht mehr unterstützte Status angezeigt werden.


[![Details zu den Produktanforderungen werden mithilfe einer Reihe von BoundFields angezeigt.](using-templatefields-in-the-detailsview-control-vb/_static/image8.png)](using-templatefields-in-the-detailsview-control-vb/_static/image7.png)

**Abbildung 3**: die Produkt-Details werden angezeigt, mit einer Reihe von BoundFields ([klicken Sie, um das Bild in voller Größe anzeigen](using-templatefields-in-the-detailsview-control-vb/_static/image9.png))


## <a name="step-2-combining-the-price-units-in-stock-and-units-on-order-into-one-row"></a>Schritt 2: Kombinieren der Preis im Lager vorrätigen Stückzahl und Einheiten auf Reihenfolge in einer Zeile

Die DetailsView weist eine Zeile für die `UnitPrice`, `UnitsInStock`, und `UnitsOnOrder` Felder. Können wir kombinieren diese Datenfelder in einer einzelnen Zeile mit ein TemplateField entweder durch ein neues TemplateField hinzufügen oder konvertieren eine der vorhandenen `UnitPrice`, `UnitsInStock`, und `UnitsOnOrder` BoundFields in ein TemplateField. Während ich persönlich konvertieren vorhandene BoundFields bevorzuge, versuchen Sie es selbst durch Hinzufügen einer neuen TemplateField.

Starten Sie durch Klicken auf die Felder bearbeiten-Link in DetailsViews Smarttag, um das Dialogfeld Felder anzuzeigen. Als Nächstes fügen Sie ein neues TemplateField hinzu und legen Sie dessen `HeaderText` Eigenschaft auf "Preis und Inventory" und verschieben Sie die neue TemplateField so, dass die It über positioniert ist die `UnitPrice` BoundField.


[![Hinzufügen einer neuen TemplateField DetailsView-Steuerelement](using-templatefields-in-the-detailsview-control-vb/_static/image11.png)](using-templatefields-in-the-detailsview-control-vb/_static/image10.png)

**Abbildung 4**: Hinzufügen einer neuen TemplateField DetailsView-Steuerelement ([klicken Sie, um das Bild in voller Größe anzeigen](using-templatefields-in-the-detailsview-control-vb/_static/image12.png))


Da diese neue TemplateField im derzeit angezeigten Werte enthalten, wird die `UnitPrice`, `UnitsInStock`, und `UnitsOnOrder` BoundFields, entfernen wir sie.

Die letzte Aufgabe für diesen Schritt wird zum Definieren der `ItemTemplate` Markup für das Preis- und Inventar TemplateField, die möglicherweise erreicht, entweder über DetailsViews-Vorlage, die Schnittstelle, die im Designer oder manuell durch deklarative Syntax des Steuerelements bearbeiten. Wie bei GridView kann bearbeiten vorlagenschnittstelle des DetailsView durch Klicken auf die Vorlagen bearbeiten im Smarttag zugegriffen werden. Von hier aus können Sie auswählen, die Vorlage, die aus der Dropdown-Liste bearbeiten, und klicken Sie dann alle Websteuerelemente aus der Toolbox hinzufügen.

In diesem Tutorial hinzufügen ein Label-Steuerelement zum Preis und zur Inventur TemplateField des zunächst `ItemTemplate`. Klicken Sie dann auf den Link "DataBindings bearbeiten" aus der Bezeichnung Websteuerelement Smarttag und Binden der `Text` Eigenschaft, um die `UnitPrice` Feld.


[![Binden Sie die Texteigenschaft der Bezeichnung auf das Datenfeld für UnitPrice](using-templatefields-in-the-detailsview-control-vb/_static/image14.png)](using-templatefields-in-the-detailsview-control-vb/_static/image13.png)

**Abbildung 5**: Binden des Bezeichnungsfelds `Text` Eigenschaft, um die `UnitPrice` Feld "Daten" ([klicken Sie, um das Bild in voller Größe anzeigen](using-templatefields-in-the-detailsview-control-vb/_static/image15.png))


## <a name="formatting-the-price-as-a-currency"></a>Formatieren den Preis als Währung

Mit folgender Ergänzung werden die Bezeichnung Websteuerelement Preis und die Hardwareinventur TemplateField jetzt nur den Preis für das ausgewählte Produkt angezeigt. Abbildung 6 zeigt einen Screenshot des unseren Fortschritt bisher ein, wenn Sie über einen Browser angezeigt.


[![Das Preis- und Inventar TemplateField zeigt die Kosten](using-templatefields-in-the-detailsview-control-vb/_static/image17.png)](using-templatefields-in-the-detailsview-control-vb/_static/image16.png)

**Abbildung 6**: der Preis und Inventar TemplateField zeigt die Kosten ([klicken Sie, um das Bild in voller Größe anzeigen](using-templatefields-in-the-detailsview-control-vb/_static/image18.png))


Beachten Sie, dass den Preis des Produkts nicht als Währung formatiert ist. Mit einem BoundField, Formatierung kann durch Festlegen der `HtmlEncode` Eigenschaft `False` und `DataFormatString` Eigenschaft `{0:formatSpecifier}`. Für ein TemplateField verwenden müssen jedoch alle formatierungsanweisungen in die Databinding-Syntax oder an einer beliebigen Stelle in den Code der Anwendung (z. B. in die ASP.NET-Seite Code-Behind-Klasse) definiert eine Methode zur Formatierung angegeben werden.

Um anzugeben, die Formatierung für die Databinding-Syntax in das Label-Steuerelement verwendet, zurück zum Dialogfeld DataBindings durch Klicken auf den Link "DataBindings bearbeiten" aus der Bezeichnung Smarttag Sie können die formatierungsanweisungen direkt in die Dropdownliste Format eingeben, oder wählen Sie eine der definierten Formatzeichenfolgen. Wie Sie mit der BoundField des `DataFormatString` -Eigenschaft, die Formatierung angegeben `{0:formatSpecifier}`.

Für die `UnitPrice` Feld mit der währungsformatierung angegeben, entweder durch Auswählen des entsprechenden Dropdownliste-Wertes oder durch Eingabe in `{0:C}` manuell.


[![Den Preis als Währung zu formatieren.](using-templatefields-in-the-detailsview-control-vb/_static/image20.png)](using-templatefields-in-the-detailsview-control-vb/_static/image19.png)

**Abbildung 7**: den Preis als Währung zu formatieren ([klicken Sie, um das Bild in voller Größe anzeigen](using-templatefields-in-the-detailsview-control-vb/_static/image21.png))


Die formatierungs-Spezifikation wird deklarativ angegeben, als zweiten Parameter in der `Bind` oder `Eval` Methoden. Die Einstellungen, die nur über das Designer-Ergebnis in den folgenden Datenbindungsausdruck im deklarativen Markup:


[!code-aspx[Main](using-templatefields-in-the-detailsview-control-vb/samples/sample2.aspx)]

## <a name="adding-the-remaining-data-fields-to-the-templatefield"></a>Die übrigen Datenfelder hinzufügen, um das TemplateField

Wir haben an diesem Punkt angezeigt und formatiert die `UnitPrice` Daten in den Preis und die Hardwareinventur TemplateField im Feld, jedoch benötigen Sie zum Anzeigen der `UnitsInStock` und `UnitsOnOrder` Felder. Jetzt zeigen wir diese in einer Zeile unterhalb der Preis und in Klammern angegeben. Von der Vorlage bearbeiten Schnittstelle im Designer kann solche Markup hinzugefügt werden, positionieren den Cursor in der Vorlage und einfach in den anzuzeigenden Text eingeben. Alternativ kann dieses Markup direkt in der deklarativen Syntax eingegeben werden.

Fügen Sie statischen Markup, Label-Websteuerelemente und Databinding-Syntax, damit Sie den Preis und die Hardwareinventur TemplateField zeigt die Informationen zum Preis und den Bestand wie folgt:

*Einzelpreis*  
(**Auf Lager / Reihenfolge:** *UnitsInStock* / *UnitsOnOrder*)

Nach dem Ausführen dieser Aufgabe sollte Ihre DetailsView deklarative Markup etwa wie folgt aussehen:


[!code-aspx[Main](using-templatefields-in-the-detailsview-control-vb/samples/sample3.aspx)]

Mit diesen Änderungen haben wir den Preis und Informationen in einer einzigen Zeile der DetailsView konsolidiert.


[![Den Preis und die Inventurinformationen wird in einer einzelnen Zeile angezeigt](using-templatefields-in-the-detailsview-control-vb/_static/image23.png)](using-templatefields-in-the-detailsview-control-vb/_static/image22.png)

**Abbildung 8**: die Preis- und Bestandsdaten in einer einzelnen Zeile angezeigt wird ([klicken Sie, um das Bild in voller Größe anzeigen](using-templatefields-in-the-detailsview-control-vb/_static/image24.png))


## <a name="step-3-customizing-the-discontinued-field-information"></a>Schritt 3: Anpassen der Feld "Discontinued"-Informationen

Die `Products` tabellenspezifischen `Discontinued` Spalte ist ein Bit-Wert, der angibt, ob das Produkt eingestellt wurde. Beim Binden einer DetailsView (oder GridView) an ein Datenquellen-Steuerelement die Boolean-Wert-Felder, wie `Discontinued`, werden als CheckBoxFields implementiert, während Sie Wertfelder nicht boolescher, wie `ProductID`, `ProductName`usw., werden als implementiert BoundFields. CheckBoxField wird als deaktiviertes Kontrollkästchen, das überprüft wird, wenn das Datenfeld Wert "true" deaktiviert, und andernfalls ist gerendert.

Anstelle der Anzeige der CheckBoxField sollten wir, der angibt, der Text zeigt stattdessen, und zwar unabhängig davon, ob das Produkt nicht mehr unterstützt wird. Um dies zu erreichen wir CheckBoxField von DetailsView entfernen und fügen Sie dann eine BoundField hinzu, deren `DataField` -Eigenschaft wurde festgelegt, um `Discontinued`. Nehmen Sie einen Moment Zeit, dies zu tun. Nach dieser Änderung werden DetailsView den Text "True" für nicht mehr unterstützte Produkte und "False" für Produkte, die noch aktiv sind.


[![Die Zeichenfolgen werden "true" und "false" verwendet, um den nicht mehr unterstützte Status anzeigen](using-templatefields-in-the-detailsview-control-vb/_static/image26.png)](using-templatefields-in-the-detailsview-control-vb/_static/image25.png)

**Abbildung 9**: die Zeichenfolgen "true" und "false" werden verwendet, um den Status Discontinued anzuzeigen ([klicken Sie, um das Bild in voller Größe anzeigen](using-templatefields-in-the-detailsview-control-vb/_static/image27.png))


Stellen Sie sich, dass die Zeichenfolgen "True" oder "False", aber "YES" und "NO" stattdessen verwendet werden soll, nicht. Eine solche Anpassung kann mithilfe der ein TemplateField und einer Formatierungsmethode ausgeführt werden. Eine Formatierungsmethode dauert in einer beliebigen Anzahl von Eingabeparametern, aber Sie müssen den HTML-Code zum Einfügen in die Vorlage (als Zeichenfolge) zurück.

Hinzufügen eine Formatierungsmethode, die `DetailsViewTemplateField.aspx` Seite des Code-Behind-Klasse, die mit dem Namen `DisplayDiscontinuedAsYESorNO` , akzeptiert eine `Northwind.ProductsRow` Objekt als Eingabeparameter und gibt eine Zeichenfolge zurück. Wie im vorherigen Tutorial, diese Methode erläutert *müssen* gekennzeichnet werden `Protected` oder `Public` um aus der Vorlage zugegriffen werden.


[!code-vb[Main](using-templatefields-in-the-detailsview-control-vb/samples/sample4.vb)]

Diese Methode überprüft die Eingabeparameter (`discontinued`) und gibt "YES" zurück, wenn es sich handelt `True`, andernfalls "NO".

> [!NOTE]
> In der Formatierungsmethode, die in den vorherigen Tutorials Rückruf, der in einem Datenfeld übergeben wurden, die möglicherweise enthalten untersucht `NULL` s und benötigt daher überprüft, ob des Mitarbeiters `HiredDate` Eigenschaftswert musste eine Datenbank `NULL` Wert vorher Zugreifen auf die `EmployeesRow`des `HiredDate` Eigenschaft. Eine solche Überprüfung ist daher nicht erforderlich hier die `Discontinued` Spalte ist niemals Datenbank `NULL` Werte zugewiesen. Darüber hinaus können aus diesem Grund die Methode ein booleschen Wert Geben Sie Parameter, anstatt akzeptiert akzeptieren ist eine `ProductsRow` Instanz oder einen Parameter vom Typ `Object`.


Mit dieser Methode zur Formatierung abgeschlossen, übrig bleibt, über das TemplateField Aufrufen `ItemTemplate`. Erstellung der TemplateField entfernen Sie entweder die `Discontinued` BoundField und ein neues TemplateField hinzufügen oder konvertieren Sie die `Discontinued` BoundField in ein TemplateField. Bearbeiten Sie dann aus der Ansicht deklaratives Markup das TemplateField, damit sie nur eine Elementvorlage enthält, die aufruft der `DisplayDiscontinuedAsYESorNO` Methode und übergeben den Wert des aktuellen `ProductRow` Instanz `Discontinued` Eigenschaft. Dies kann zugegriffen werden, über die `Eval` Methode. Insbesondere sollte das TemplateField Markup aussehen:


[!code-aspx[Main](using-templatefields-in-the-detailsview-control-vb/samples/sample5.aspx)]

Dies bewirkt, dass die `DisplayDiscontinuedAsYESorNO` Methode aufgerufen wird, beim Rendern von DetailsView, übergeben die `ProductRow` Instanz `Discontinued` Wert. Da die `Eval` Methode gibt einen Wert vom Typ `Object`, aber die `DisplayDiscontinuedAsYESorNO` Methode erwartet einen Eingabeparameter vom Typ `Boolean`, wir wandeln die `Eval` Methoden Rückgabewert auf `Boolean`. Die `DisplayDiscontinuedAsYESorNO` Methode gibt dann "YES" oder je nach Wert empfängt sie "Nein". Der zurückgegebene Wert ist die Anzeige auf dieser DetailsView Zeile (siehe Abbildung 10).


[![Ja oder Nein-Werte sind jetzt in der Zeile Discontinued angezeigt.](using-templatefields-in-the-detailsview-control-vb/_static/image29.png)](using-templatefields-in-the-detailsview-control-vb/_static/image28.png)

**Abbildung 10**: Ja oder Nein-Werte sind jetzt in der Zeile Discontinued angezeigt ([klicken Sie, um das Bild in voller Größe anzeigen](using-templatefields-in-the-detailsview-control-vb/_static/image30.png))


## <a name="summary"></a>Zusammenfassung

Das TemplateField im DetailsView-Steuerelement ermöglicht einen höheren Grad an Flexibilität beim Anzeigen von Daten als bei anderen feldsteuerungen verfügbar ist, und sich ideal für Situationen eignen, in denen:

- Mehrere Datenfelder, die in einer GridView-Spalte angezeigt werden müssen
- Die Daten ist mithilfe von eine Websteuerelement anstelle von nur-Text am besten ausgedrückt.
- Die Ausgabe hängt von der zugrunde liegenden Daten, z. B. das Anzeigen von Metadaten oder neuformatierung der das

Während ein höheres Maß an Flexibilität bei der Darstellung des zugrunde liegenden Daten des DetailsView von TemplateFields zu ermöglichen, die DetailsView-Ausgabe weiterhin fühlt erhalten Sie ein wenig kastenförmige wie jedes Feld gerendert wird, als eine Zeile in einem HTML- `<table>`.

Das FormView-Steuerelement bietet ein höheres Maß an Flexibilität bei der Konfiguration der gerenderten Ausgabe. Die FormView-Steuerelement enthält keine Felder, jedoch lediglich eine Reihe von Vorlagen (`ItemTemplate`, `EditItemTemplate`, `HeaderTemplate`und so weiter). Wir sehen, wie das FormView-Steuerelement zu verwenden, um noch bessere Kontrolle der gerenderten Layout in unserem nächsten Tutorial zu erzielen.

Viel Spaß beim Programmieren!

## <a name="about-the-author"></a>Der Autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor von sieben Büchern zu ASP/ASP.NET und Gründer von [4GuysFromRolla.com](http://www.4guysfromrolla.com), arbeitet mit Microsoft-Web-Technologien seit 1998. Er ist als ein unabhängiger Berater, Schulungsleiter und Autor. Sein neueste Buch wird [*Sams Schulen selbst ASP.NET 2.0 in 24 Stunden*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Er ist unter [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) oder über seinen Blog finden Sie unter [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Besonderen Dank an

Diese tutorialreihe wurde durch viele hilfreiche Reviewer überprüft. Führendes Prüfer für dieses Tutorial wurde Dan Jagers. Meine zukünftigen MSDN-Artikeln überprüfen möchten? Wenn dies der Fall ist, löschen Sie mir eine Linie an [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Zurück](using-templatefields-in-the-gridview-control-vb.md)
> [Weiter](using-the-formview-s-templates-vb.md)
