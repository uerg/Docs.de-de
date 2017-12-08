---
uid: web-forms/overview/data-access/custom-formatting/using-templatefields-in-the-detailsview-control-vb
title: Verwenden von TemplateFields im DetailsView-Steuerelement (VB) | Microsoft Docs
author: rick-anderson
description: "Außerdem stehen die gleichen von TemplateFields Funktionen zur Verfügung, mit der GridView mit dem DetailsView-Steuerelement zur Verfügung. In diesem Lernprogramm zeigen wir ein Produkt..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/31/2010
ms.topic: article
ms.assetid: 0b91d5f8-127d-4f6a-b204-f2e2b35ef703
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/custom-formatting/using-templatefields-in-the-detailsview-control-vb
msc.type: authoredcontent
ms.openlocfilehash: b368a651253a569865bb92fa93d3462f88d8935f
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="using-templatefields-in-the-detailsview-control-vb"></a>Verwenden von TemplateFields im DetailsView-Steuerelement (VB)
====================
durch [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Beispiel-App herunterladen](http://download.microsoft.com/download/5/7/0/57084608-dfb3-4781-991c-407d086e2adc/ASPNET_Data_Tutorial_13_VB.exe) oder [PDF herunterladen](using-templatefields-in-the-detailsview-control-vb/_static/datatutorial13vb1.pdf)

> Außerdem stehen die gleichen von TemplateFields Funktionen zur Verfügung, mit der GridView mit dem DetailsView-Steuerelement zur Verfügung. In diesem Lernprogramm zeigen wir ein Produkt zu einem Zeitpunkt mithilfe einer DetailsView, die von TemplateFields enthält.


## <a name="introduction"></a>Einführung

Die TemplateField bietet einen höheren Grad von Flexibilität bei der Renderingdaten als die BoundField-, CheckBoxField HyperLinkField und anderen Steuerelementen für Feld. In der [vorherigen Lernprogramm](using-templatefields-in-the-gridview-control-vb.md) erläutert, die TemplateField in einem GridView zu verwenden:

- Mehrere Datenfeldwerte in einer Spalte angezeigt. Insbesondere sowohl die `FirstName` und `LastName` Felder wurden in eine GridView-Spalte zusammengefasst.
- Verwenden Sie einen alternativen-Websteuerelement, um ein Datenfeldwert auszudrücken. Wurde erläutert, wie das Anzeigen der `HiredDate` mithilfe eines Kalendersteuerelements Wert.
- Anzeigen von Statusinformationen, die basierend auf den zugrunde liegenden Daten. Während der `Employees` Tabelle enthält eine Spalte, die die Anzahl der Tage zurückgibt, ein Mitarbeiter für den Auftrag wurde, nicht, konnten wir solche Informationen in die GridView-Beispiel im vorherigen Lernprogramm mit der Verwendung eines TemplateField und Formatierungsmethode angezeigt.

Außerdem stehen die gleichen von TemplateFields Funktionen zur Verfügung, mit der GridView mit dem DetailsView-Steuerelement zur Verfügung. In diesem Lernprogramm zeigen wir ein Produkt zu einem Zeitpunkt mithilfe einer DetailsView, die von zwei TemplateFields enthält. Die erste TemplateField fasst die `UnitPrice`, `UnitsInStock`, und `UnitsOnOrder` Datenfelder in einer Zeile, DetailsView. Die zweite TemplateField zeigt den Wert von der `Discontinued` Feld, jedoch eine Formatierungsmethode anzuzeigen, wenn "YES" verwendet `Discontinued` ist `True`, und andernfalls "NO".


[![Zwei von TemplateFields werden verwendet, um die Anzeige anpassen](using-templatefields-in-the-detailsview-control-vb/_static/image2.png)](using-templatefields-in-the-detailsview-control-vb/_static/image1.png)

**Abbildung 1**: zwei von TemplateFields werden verwendet, um die Anzeige anpassen ([klicken Sie hier, um das Bild in voller Größe angezeigt](using-templatefields-in-the-detailsview-control-vb/_static/image3.png))


Fangen wir an!

## <a name="step-1-binding-the-data-to-the-detailsview"></a>Schritt 1: Binden von Daten, DetailsView

Wie im vorherigen Lernprogramm erläutert, bei der Arbeit mit von TemplateFields häufig ist es am einfachsten, DetailsView-Steuerelement, das nur BoundFields enthält erstellen Sie zunächst und Hinzufügen von neuen TemplateFields oder konvertieren die vorhandenen BoundFields in von TemplateFields als erforderlich . Starten Sie dieses Lernprogramm daher von der Seite durch den Designer eine DetailsView hinzufügen und binden es an einem ObjectDataSource, die die Liste der Produkte zurückgibt. Diese Schritte erstellt eine DetailsView BoundFields für jedes der Felder für das Produkt nicht boolescher Wert und eine CheckBoxField für das Feld für einen booleschen Wert (eingestellt).

Öffnen der `DetailsViewTemplateField.aspx` Seite und eine DetailsView aus der Toolbox in den Designer ziehen. Wählen Sie aus der DetailsView Smarttag ein neues ObjectDataSource-Steuerelement hinzufügen, die aufruft der `ProductsBLL` Klasse `GetProducts()` Methode.


[![Fügen Sie ein neues ObjectDataSource-Steuerelement, das die GetProducts()-Methode aufruft](using-templatefields-in-the-detailsview-control-vb/_static/image5.png)](using-templatefields-in-the-detailsview-control-vb/_static/image4.png)

**Abbildung 2**: Fügen Sie ein neues ObjectDataSource-Steuerelement, Invokes die `GetProducts()` Methode ([klicken Sie hier, um das Bild in voller Größe angezeigt](using-templatefields-in-the-detailsview-control-vb/_static/image6.png))


Für diesen Bericht Entfernen der `ProductID`, `SupplierID`, `CategoryID`, und `ReorderLevel` BoundFields. Als Nächstes die BoundFields anordnen, damit die `CategoryName` und `SupplierName` BoundFields angezeigt werden, sofort nach der `ProductName` BoundField. Passen Sie ggf. die `HeaderText` Eigenschaften und Formatierungseigenschaften für die BoundFields als Sie passen finden Sie unter. Wie die GridView dieser Änderungen an der BoundField-Anwendungsebene über die Felder (Dialogfeld) (Zugriff durch Klicken auf den Link Felder bearbeiten, in der DetailsView-Smarttag) oder über die deklarative Syntax ausgeführt werden können. Schließlich deaktivieren, DetailsView des `Height` und `Width` Eigenschaftswerte um, DetailsView zulassen steuern, um basierend auf der angezeigten Daten zu erweitern, und aktivieren Sie das Kontrollkästchen Paging aktivieren, in das Smarttag.

Nachdem diese Änderungen vorgenommen wurden, sollte deklarativem Markup des Steuerelements DetailsView etwa wie folgt aussehen:


[!code-aspx[Main](using-templatefields-in-the-detailsview-control-vb/samples/sample1.aspx)]

Nehmen Sie einen Moment Zeit, um die Seite über einen Browser anzuzeigen. An diesem Punkt sollte ein einzelnes Produkt aufgeführt (Chai) mit Zeilen, die mit den Namen des Produkts, Kategorie, Lieferanten, Preis, Einheiten im Lager, bestellte und deren Status nicht mehr unterstützte angezeigt werden.


[![Das Produkt-Details werden mit einer Reihe von BoundFields angezeigt.](using-templatefields-in-the-detailsview-control-vb/_static/image8.png)](using-templatefields-in-the-detailsview-control-vb/_static/image7.png)

**Abbildung 3**: die Produktdetails werden mithilfe einer Reihe von BoundFields angezeigt ([klicken Sie hier, um das Bild in voller Größe angezeigt](using-templatefields-in-the-detailsview-control-vb/_static/image9.png))


## <a name="step-2-combining-the-price-units-in-stock-and-units-on-order-into-one-row"></a>Schritt 2: Kombinieren der Preis, Einheiten im Lager und Einheiten auf Reihenfolge in einer Reihe

DetailsView enthält eine Zeile für die `UnitPrice`, `UnitsInStock`, und `UnitsOnOrder` Felder. Wir können diese Datenfelder in eine einzelne Zeile mit der kombinieren ein TemplateField durch Hinzufügen einer neuen TemplateField oder durch Konvertieren eines vorhandenen `UnitPrice`, `UnitsInStock`, und `UnitsOnOrder` BoundFields in ein TemplateField. Während ich persönlich Konvertieren vorhandener BoundFields lieber, wir durch Hinzufügen einer neuen TemplateField üben.

Starten Sie durch Klicken auf den Link Felder bearbeiten, in der DetailsView Smarttag, um das Dialogfeld Felder anzuzeigen. Als Nächstes fügen Sie ein neues TemplateField hinzu, und legen Sie dessen `HeaderText` -Eigenschaft auf "Preis und Inventory" und verschieben Sie die neue TemplateField so, dass die It über positioniert ist die `UnitPrice` BoundField.


[![Hinzufügen einer neuen TemplateField, DetailsView-Steuerelement](using-templatefields-in-the-detailsview-control-vb/_static/image11.png)](using-templatefields-in-the-detailsview-control-vb/_static/image10.png)

**Abbildung 4**: Hinzufügen einer neuen TemplateField, DetailsView-Steuerelement ([klicken Sie hier, um das Bild in voller Größe angezeigt](using-templatefields-in-the-detailsview-control-vb/_static/image12.png))


Da diese neue TemplateField im aktuell angezeigten Werte enthalten, wird die `UnitPrice`, `UnitsInStock`, und `UnitsOnOrder` BoundFields, wir zu entfernen.

Zum Schluss für diesen Schritt wird zum Definieren der `ItemTemplate` Markup für den Preis und die Inventur TemplateField, können u. erreicht, entweder über die DetailsView-Vorlage, die Schnittstelle, die im Designer oder manuell durch deklarative Syntax für das Steuerelement bearbeiten. Wie bei der GridView kann der DetailsView Vorlage Bearbeitungsoberfläche durch Klicken auf den Link Vorlagen bearbeiten, in das Smarttag zugegriffen werden. Von hier aus können Sie die Vorlage aus der Dropdown-Liste bearbeiten, und fügen Sie dann alle Websteuerelemente aus der Toolbox auswählen.

Für dieses Lernprogramm starten, indem der Preis und die Hardwareinventur-TemplateField ein Label-Steuerelement hinzugefügt `ItemTemplate`. Klicken Sie dann auf den Link "DataBindings bearbeiten" aus der Bezeichnung Websteuerelement Smarttag und Binden der `Text` Eigenschaft, um die `UnitPrice` Feld.


[![Binden des Bezeichnungsfelds Texteigenschaft auf das Feld "UnitPrice Daten"](using-templatefields-in-the-detailsview-control-vb/_static/image14.png)](using-templatefields-in-the-detailsview-control-vb/_static/image13.png)

**Abbildung 5**: Binden des Bezeichnungsfelds `Text` Eigenschaft, um die `UnitPrice` Feld "Daten" ([klicken Sie hier, um das Bild in voller Größe angezeigt](using-templatefields-in-the-detailsview-control-vb/_static/image15.png))


## <a name="formatting-the-price-as-a-currency"></a>Formatieren des Preis als Währung

Durch diesen Zusatz zeigt den Label-Websteuerelement Preis und Inventur TemplateField jetzt nur den Preis für das ausgewählte Produkt. Abbildung 6 zeigt einen Screenshot der Fortschritt bisher ein, wenn Sie über einen Browser angezeigt.


[![Der Preis und die Inventur TemplateField zeigt den Preis](using-templatefields-in-the-detailsview-control-vb/_static/image17.png)](using-templatefields-in-the-detailsview-control-vb/_static/image16.png)

**Abbildung 6**: der Preis und den Bestand TemplateField zeigt den Preis ([klicken Sie hier, um das Bild in voller Größe angezeigt](using-templatefields-in-the-detailsview-control-vb/_static/image18.png))


Beachten Sie, dass das Produkt Preis nicht als Währung formatiert ist. Mit einem BoundField Formatierung kann durch Festlegen der `HtmlEncode` Eigenschaft `False` und die `DataFormatString` Eigenschaft `{0:formatSpecifier}`. Für ein TemplateField müssen allerdings Formatierung Anweisungen in der Syntax Databinding oder durch die Verwendung einer Formatierungsmethode definiert an einer beliebigen Stelle innerhalb der Anwendung Code (z. B. in der ASP.NET-Seite Code-Behind-Klasse) angegeben werden.

Um anzugeben, die Formatierung für die Databinding-Syntax im Label-Steuerelement verwendet, zurück in das Dialogfeld "DataBindings", durch Klicken auf die Verknüpfung des Bezeichnungsfelds Smarttag DataBindings bearbeiten. Sie können die Formatierung Anweisungen direkt in die Dropdownliste Format eingeben oder wählen Sie eine der Zeichenfolgen definiertes Format aufweisen. Wie Sie mit der BoundField `DataFormatString` -Eigenschaft, die Formatierung wird mit angegeben `{0:formatSpecifier}`.

Für die `UnitPrice` Feld verwenden, die das Währungsformat angegeben, entweder durch Auswählen des entsprechenden Dropdownlisten-Wertes oder durch Eingabe in `{0:C}` per hand katalogisiert wird.


[![Formatieren des Preis als Währung](using-templatefields-in-the-detailsview-control-vb/_static/image20.png)](using-templatefields-in-the-detailsview-control-vb/_static/image19.png)

**Abbildung 7**: formatieren den Preis als Währung ([klicken Sie hier, um das Bild in voller Größe angezeigt](using-templatefields-in-the-detailsview-control-vb/_static/image21.png))


Die Formatierung-Spezifikation wird deklarativ angegeben, als zweiten Parameter in der `Bind` oder `Eval` Methoden. Die Einstellungen, die gerade durch das Designer-Resultset in den folgenden Datenbindungsausdruck im deklarativen Markup vorgenommen haben:


[!code-aspx[Main](using-templatefields-in-the-detailsview-control-vb/samples/sample2.aspx)]

## <a name="adding-the-remaining-data-fields-to-the-templatefield"></a>Die übrigen Datenfelder hinzufügen in der TemplateField

Zu diesem Zeitpunkt haben wir angezeigt, und formatiert die `UnitPrice` Daten in dem Preis und den Bestand TemplateField Feld, jedoch benötigen Sie weitere anzuzeigende der `UnitsInStock` und `UnitsOnOrder` Felder. Wir zeigen diese in einer Zeile unterhalb der Preis und in Klammern. Von der Vorlage bearbeiten Schnittstelle im Designer kann solche Markup hinzugefügt werden, indem positionieren den Cursor in der Vorlage und einfach die anzuzeigende Text eingeben. Alternativ kann dieser Markup direkt in die deklarative Syntax eingegeben werden.

Die statische Markup Bezeichnung Websteuerelementen und Databinding-Syntax hinzufügen, sodass dem Preis und den Bestand TemplateField zeigt Preis-und Inventur wie folgt:

*UnitPrice*  
(**Im Lager / Modelladministratorberechtigung:** *"UnitsInStock"* / *UnitsOnOrder*)

Nach dem Ausführen dieser Aufgabe sollte Ihre DetailsView deklarative Markup etwa wie folgt aussehen:


[!code-aspx[Main](using-templatefields-in-the-detailsview-control-vb/samples/sample3.aspx)]

Durch diese Änderungen haben wir die Preis und Informationen in eine einzelne Zeile von DetailsView konsolidiert.


[![Der Preis und Inventarinformationen wird in einer einzelnen Zeile angezeigt.](using-templatefields-in-the-detailsview-control-vb/_static/image23.png)](using-templatefields-in-the-detailsview-control-vb/_static/image22.png)

**Abbildung 8**: der Preis und Inventarinformationen in einer einzelnen Zeile angezeigt wird ([klicken Sie hier, um das Bild in voller Größe angezeigt](using-templatefields-in-the-detailsview-control-vb/_static/image24.png))


## <a name="step-3-customizing-the-discontinued-field-information"></a>Schritt 3: Anpassen, die nicht mehr unterstützte Feldinformationen

Die `Products` tabellenspezifischen `Discontinued` Spalte ist ein Bitwert, der angibt, ob das Produkt eingestellt wurde. Bei einem DetailsView (oder GridView) an ein Datenquellen-Steuerelement zu binden, wie das Wahrheitswert Felder `Discontinued`, werden als CheckBoxFields implementiert, während nicht boolescher Wertfelder, wie z. B. `ProductID`, `ProductName`usw., werden als implementiert BoundFields. CheckBoxField wird als ein deaktiviertes Kontrollkästchen, das überprüft wird, wenn das Datenfeld Wert "true" deaktiviert, und andernfalls ist gerendert.

Anstelle der Anzeige der CheckBoxField sollten wir stattdessen, der angibt, der Text angezeigt, und zwar unabhängig davon, ob das Produkt nicht mehr unterstützt wird. Um dies zu erreichen konnten wir DetailsView CheckBoxField aufheben und fügen Sie dann eine BoundField, deren `DataField` -Eigenschaft wurde festgelegt, um `Discontinued`. Nehmen Sie einen Moment Zeit, zu diesem Zweck. Nach dieser Änderung wird gezeigt, DetailsView den Text "True" für nicht mehr unterstützte Produkte und "Falsch" für Produkte, die noch aktiv sind.


[![Die Zeichenfolgen werden "true" und "false" verwendet, um den Status nicht mehr unterstützte anzuzeigen](using-templatefields-in-the-detailsview-control-vb/_static/image26.png)](using-templatefields-in-the-detailsview-control-vb/_static/image25.png)

**Abbildung 9**: die Zeichenfolgen "true" und "false" werden verwendet, um den Status Discontinued anzuzeigen ([klicken Sie hier, um das Bild in voller Größe angezeigt](using-templatefields-in-the-detailsview-control-vb/_static/image27.png))


Stellen Sie sich vor, dass wir nicht die Zeichenfolgen "True" oder "False", aber "YES" und "NO" verwendet werden soll. Diese Anpassung kann mit Hilfe einer TemplateField und einer Formatierungsmethode ausgeführt werden. Eine Formatierungsmethode kann in einer beliebigen Anzahl von Eingabeparametern, aber muss den HTML-Code zum Einfügen in die Vorlage (als Zeichenfolge) zurückgeben.

Hinzufügen eine Formatierungsmethode, die `DetailsViewTemplateField.aspx` Seite des Code-Behind-Klasse, die mit dem Namen `DisplayDiscontinuedAsYESorNO` , akzeptiert eine `Northwind.ProductsRow` -Objekt als Eingabeparameter und gibt eine Zeichenfolge zurück. Wie im vorherigen Lernprogramm, diese Methode erläutert *müssen* gekennzeichnet werden, als `Protected` oder `Public` um aus der Vorlage zugegriffen werden.


[!code-vb[Main](using-templatefields-in-the-detailsview-control-vb/samples/sample4.vb)]

Diese Methode überprüft die Eingabeparameter (`discontinued`) und "YES" zurückgibt, ist er `True`, andernfalls "NO".

> [!NOTE]
> In in der vorherigen Lernprogramm wie bereits erwähnt, die wir in einem Datenfeld übergeben haben, die möglicherweise enthalten untersucht Formatierungsmethode `NULL` s und benötigt daher beim Überprüfen des Mitarbeiters `HiredDate` Eigenschaftswert musste eine Datenbank `NULL` vor dem Wert Zugreifen auf die `EmployeesRow`des `HiredDate` Eigenschaft. Eine solche Überprüfung wird nicht hier benötigt, da die `Discontinued` Spalte ist niemals Datenbank `NULL` Werte zugewiesen. Dies ist darüber, warum die Methode ein booleschen Wert Geben Sie Parameter, anstatt akzeptiert akzeptieren ein `ProductsRow` Instanz oder einen Parameter vom Typ `Object`.


Mit dieser vollständigen Formatierungsmethode übrig bleibt für die aufzurufende aus der TemplateField `ItemTemplate`. Erstellung der TemplateField entfernen Sie entweder die `Discontinued` BoundField und eine neue TemplateField hinzufügen oder konvertieren Sie die `Discontinued` BoundField in ein TemplateField. Bearbeiten Sie dann aus der Sicht deklarativem Markup der TemplateField, damit sie nur eine ItemTemplate enthält, die aufgerufen der `DisplayDiscontinuedAsYESorNO` -Methode auf und übergibt den Wert des aktuellen `ProductRow` Instanz `Discontinued` Eigenschaft. Dies kann zugegriffen werden, über die `Eval` Methode. Insbesondere sollten die TemplateField-Markup formuliert werden:


[!code-aspx[Main](using-templatefields-in-the-detailsview-control-vb/samples/sample5.aspx)]

Dies bewirkt, dass die `DisplayDiscontinuedAsYESorNO` rendern, DetailsView aufzurufende Methode übergibt der `ProductRow` Instanz `Discontinued` Wert. Da die `Eval` -Methode gibt einen Wert vom Typ `Object`, aber die `DisplayDiscontinuedAsYESorNO` Methode erwartet einen Eingabeparameter vom Typ `Boolean`, wir umgewandelt der `Eval` Methoden Rückgabewert auf `Boolean`. Die `DisplayDiscontinuedAsYESorNO` Methode wird dann zurückgeben "YES" oder "NO" je nach dem Wert sie empfängt. Der zurückgegebene Wert ist der Anzeige auf dieser DetailsView Zeile (siehe Abbildung 10).


[![Ja oder Nein-Werte werden nun in der Zeile Discontinued angezeigt.](using-templatefields-in-the-detailsview-control-vb/_static/image29.png)](using-templatefields-in-the-detailsview-control-vb/_static/image28.png)

**Abbildung 10**: Ja oder Nein-Werte sind nun in der Zeile Discontinued angezeigt ([klicken Sie hier, um das Bild in voller Größe angezeigt](using-templatefields-in-the-detailsview-control-vb/_static/image30.png))


## <a name="summary"></a>Zusammenfassung

Die TemplateField im Steuerelement DetailsView ermöglicht ein höheres Maß an Flexibilität beim Anzeigen von Daten als mit den anderen Feldsteuerelemente steht und sich ideal für Situationen eignen, in denen:

- Mehrere Datenfelder in einer GridView-Spalte angezeigt werden müssen
- Am besten ausgedrückt ein Websteuerelement statt nur-text
- Die Ausgabe hängt von der zugrunde liegenden Daten, z. B. das Anzeigen von Metadaten oder Formatieren von Daten

Während ein höheres Maß an Flexibilität beim Rendern des zugrunde liegenden Daten der DetailsView von TemplateFields zu ermöglichen, DetailsView Ausgabe noch idealer erhalten Sie ein wenig kastenförmige wie jedes Feld als eine Zeile in einem HTML gerendert wird `<table>`.

Die FormView-Steuerelement bietet ein höheres Maß an Flexibilität beim Konfigurieren von der gerenderten Ausgabe. Die FormView enthält keine Felder, jedoch nur eine Reihe von Vorlagen (`ItemTemplate`, `EditItemTemplate`, `HeaderTemplate`usw.). Wir sehen, wie die FormView zu verwenden, um noch mehr Kontrolle über das Layout des gerenderten in unserem nächsten Lernprogramm zu erzielen.

Viel Spaß beim Programmieren!

## <a name="about-the-author"></a>Informationen zum Autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor von sieben ASP/ASP.NET-Büchern und Gründer von [4GuysFromRolla.com](http://www.4guysfromrolla.com), Microsoft Web-Technologien seit 1998 arbeitet. Scott fungiert als ein unabhängiger Berater, Trainer und Writer. Sein neueste Buch wird [ *Sams Schulen selbst ASP.NET 2.0 in 24 Stunden*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Er die erreicht werden kann, zur [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) oder über seinen Blog die finden Sie unter [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Besonderen Dank an

Diese Reihe von Lernprogrammen wurde durch viele nützliche Bearbeiter überprüft. Lead Prüfer für dieses Lernprogramm wurde Dan Jagers. Meine bevorstehende MSDN-Artikel Überprüfen von Interesse? Wenn dies der Fall ist, löschen Sie mich zeilenweise [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[Zurück](using-templatefields-in-the-gridview-control-vb.md)
[Weiter](using-the-formview-s-templates-vb.md)
