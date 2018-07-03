---
uid: web-forms/overview/data-access/custom-formatting/using-the-formview-s-templates-vb
title: Mithilfe der FormView Vorlagen (VB) | Microsoft-Dokumentation
author: rick-anderson
description: Im Gegensatz zu DetailsView besteht das FormView-Steuerelement keine Felder. Stattdessen wird das FormView-Steuerelement gerendert, mithilfe von Vorlagen. In diesem Tutorial untersuchen wir mithilfe der f...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/31/2010
ms.topic: article
ms.assetid: 67b25f4c-2823-42b6-b07d-1d650b3fd711
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/data-access/custom-formatting/using-the-formview-s-templates-vb
msc.type: authoredcontent
ms.openlocfilehash: 66ad79c0c385afed75c1888b81328220ee048d19
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/03/2018
ms.locfileid: "37389913"
---
<a name="using-the-formviews-templates-vb"></a>Mithilfe der FormView Vorlagen (VB)
====================
durch [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Beispiel-App herunter](http://download.microsoft.com/download/5/7/0/57084608-dfb3-4781-991c-407d086e2adc/ASPNET_Data_Tutorial_14_VB.exe) oder [PDF-Datei herunterladen](using-the-formview-s-templates-vb/_static/datatutorial14vb1.pdf)

> Im Gegensatz zu DetailsView besteht das FormView-Steuerelement keine Felder. Stattdessen wird das FormView-Steuerelement gerendert, mithilfe von Vorlagen. Verwenden in diesem Lernprogramm wird untersucht das FormView-Steuerelement, um eine weniger strenge Anzeige von Daten zu präsentieren.


## <a name="introduction"></a>Einführung

In den letzten beiden Tutorials wurde erläutert, wie die GridView und DetailsView-Steuerelemente Ausgaben verwenden von TemplateFields anpassen. Von TemplateFields ermöglichen, für den Inhalt für ein bestimmtes Feld hoch angepasst werden, aber letztendlich GridView und DetailsView haben ein Recht erhalten Sie kastenförmige, Raster-ähnliches Aussehen. Solche ein Raster-ähnliches Layout ist ideal für viele Szenarien, aber manchmal eine flexiblere, weniger strenge Anzeige erforderlich ist. Wenn Sie einen einzelnen Datensatz anzeigen zu können, kann solche ein flüssiges Layout über das FormView-Steuerelement.

Im Gegensatz zu DetailsView besteht das FormView-Steuerelement keine Felder. Sie können keinem FormView-Steuerelement eine BoundField- oder TemplateField hinzufügen. Stattdessen wird das FormView-Steuerelement gerendert, mithilfe von Vorlagen. Stellen Sie sich das FormView-Steuerelement als ein DetailsView-Steuerelement, ein einzelnes TemplateField enthält. Die FormView-Steuerelement unterstützt die folgenden Vorlagen:

- `ItemTemplate` verwendet, um den entsprechenden Datensatz angezeigt, in das FormView-Steuerelement zu rendern.
- `HeaderTemplate` verwendet, um einen optional-Header-Zeile anzugeben.
- `FooterTemplate` zum Angeben einer optionalen Fußzeile
- `EmptyDataTemplate` Wenn der FormView `DataSource` verfügt nicht über die auf Datensätze, die `EmptyDataTemplate` wird anstelle von verwendet die `ItemTemplate` für das Rendern von Markup des Steuerelements
- `PagerTemplate` kann verwendet werden, die Paging-Schnittstelle für FormViews anpassen, die Auslagerung ist aktiviert
- `EditItemTemplate` / `InsertItemTemplate` verwendet, um die Bearbeitung Schnittstelle oder Einfügen von für FormViews anzupassen, die eine solche Funktionalität unterstützen.

In diesem Lernprogramm untersuchenden verwenden das FormView-Steuerelement, um eine weniger strenge Anzeige von Produkten zu präsentieren. Statt später durch die Felder für die Name, Kategorie, Lieferanten und So weiter, das FormView `ItemTemplate` zeigt, dass diese Werte mit einer Kombination aus einem Header-Element und ein `<table>` (siehe Abbildung 1).


[![Das Raster-ähnliches Layout im DetailsView betrachtet, bricht das FormView-Steuerelement](using-the-formview-s-templates-vb/_static/image2.png)](using-the-formview-s-templates-vb/_static/image1.png)

**Abbildung 1**: FormView, die aus der Grid-Like Layout dargestellt im DetailsView Zeilenumbrüche ([klicken Sie, um das Bild in voller Größe anzeigen](using-the-formview-s-templates-vb/_static/image3.png))


## <a name="step-1-binding-the-data-to-the-formview"></a>Schritt 1: Binden der Daten an das FormView-Steuerelement

Öffnen der `FormView.aspx` Seite und einem FormView-Steuerelement aus der Toolbox in den Designer ziehen. Beim erstmaligen FormView hinzufügen wird es als ein graues Feld, das uns angewiesen, eine `ItemTemplate` ist erforderlich.


[![Die FormView-Steuerelement kann im Designer nicht gerendert werden, bis eine Elementvorlage bereitgestellt wird](using-the-formview-s-templates-vb/_static/image5.png)](using-the-formview-s-templates-vb/_static/image4.png)

**Abbildung 2**: das FormView-Steuerelement nicht gerendert werden, in den Designer an, bis ein `ItemTemplate` bereitgestellt wird ([klicken Sie, um das Bild in voller Größe anzeigen](using-the-formview-s-templates-vb/_static/image6.png))


Die `ItemTemplate` kann manuell erstellt werden (mithilfe der deklarativen Syntax) oder können durch die Bindung der FormView-Steuerelement an ein Datenquellen-Steuerelement mithilfe des Designers werden automatisch erstellt. Dieser automatisch erstellten `ItemTemplate` enthält HTML-Code enthält der Namen jedes Feld und eine Bezeichnung, deren steuern `Text` Eigenschaft gebunden ist, um den Wert des Felds. Dieser Ansatz auch Auto-erstellt ein `InsertItemTemplate` und `EditItemTemplate`, beide für die einzelnen Datenfelder vom Datenquellen-Steuerelement mit Eingabesteuerelementen aufgefüllt.

Wenn Sie die Vorlage automatisch erstellen möchten, fügen Sie aus der FormView Smarttag ein neues ObjectDataSource-Steuerelement, das aufruft der `ProductsBLL` Klasse `GetProducts()` Methode. Hiermit wird einem FormView-Steuerelement mit einem `ItemTemplate`, `InsertItemTemplate`, und `EditItemTemplate`. Entfernen Sie aus der Datenquellensicht an, die `InsertItemTemplate` und `EditItemTemplate` da nicht von Interesse sind einem FormView-Steuerelement zu erstellen, die unterstützt werden, noch einfügen oder bearbeiten. Weiter, löschen Sie das Markup in der `ItemTemplate` , damit Sie schon von Grund auf neu beginnen, über zu arbeiten.

Wenn Sie eher, werden erstellen würden die `ItemTemplate` Sie können manuell hinzufügen und dem ObjectDataSource-Steuerelement konfigurieren, indem Sie sie aus der Toolbox in den Designer ziehen. Allerdings festlegen Sie nicht das FormView Datenquelle aus dem Designer. Stattdessen wechseln Sie zur Quellansicht und manuell festlegen, der FormView `DataSourceID` Eigenschaft, um die `ID` Wert, der dem ObjectDataSource-Steuerelement. Fügen Sie als Nächstes die `ItemTemplate`.

Unabhängig davon, welchen Ansatz Sie sich entschieden, in Anspruch nehmen, an diesem Punkt FormViews deklarative Markup aussehen sollte:


[!code-aspx[Main](using-the-formview-s-templates-vb/samples/sample1.aspx)]

Nehmen Sie einen Moment Zeit, um das Paging aktivieren in das FormView Smarttag-Kontrollkästchen; Hiermit wird die `AllowPaging="True"` deklarative Syntax für das FormView Attribut. Legen Sie außerdem die `EnableViewState` Eigenschaft auf "false".

## <a name="step-2-defining-theitemtemplates-markup"></a>Schritt 2: Definieren der`ItemTemplate`Markup

Mit der FormView-Steuerelement an das ObjectDataSource-Steuerelement gebunden, und konfiguriert zur Unterstützung von Paging wir möchten, geben Sie den Inhalt für die `ItemTemplate`. Für dieses Tutorial den Namen des Produkts angezeigt wir haben eine `<h3>` Überschrift. Danach verwenden wir eine HTML `<table>` zum Anzeigen von der restlichen Produkteigenschaften in einer Tabelle mit vier Spalten, in dem die erste und dritte Spalte auflisten, die Eigenschaftennamen und die zweiten und vierten die Werte Liste.

Dieses Markup kann eingegeben wird, über das FormView Vorlagen bearbeiten-Schnittstelle im Designer oder manuell mithilfe der deklarativen Syntax eingegeben werden. Beim Arbeiten mit Vorlagen finde ich in der Regel es schneller, arbeiten Sie direkt mit der deklarativen Syntax, aber gerne verwenden, welcher Technik, die Sie am besten mit vertraut sind.

Das folgende Markup zeigt die FormView deklarative Markup nach dem `ItemTemplate`der Struktur wurde abgeschlossen:


[!code-aspx[Main](using-the-formview-s-templates-vb/samples/sample2.aspx)]

Beachten Sie, dass die Datenbindungssyntax - `<%# Eval("ProductName") %>`für Beispiel direkt in die Ausgabe der Vorlage eingefügt werden kann. D. h. sie müssen nicht zugewiesen werden ein Label-Steuerelement `Text` Eigenschaft. Angenommen, wir haben die `ProductName` in der angezeigten Werten ein `<h3>` Element mit `<h3><%# Eval("ProductName") %></h3>`, die für das Produkt als Chai gerendert werden `<h3>Chai</h3>`.

Die `ProductPropertyLabel` und `ProductPropertyValue` CSS-Klassen werden zum Angeben der Stil, der die Produkt-Eigenschaftennamen und Werte in der `<table>`. Diese CSS-Klassen sind in definiert `Styles.css` und dazu führen, dass die Eigenschaftennamen fett und rechtsbündig ausgerichtet, und fügen Sie ein Recht, die die Eigenschaftswerte Auffüllung hinzu.

Da es sich um keine CheckBoxFields zur Verfügung, mit der FormView-Steuerelement zum Anzeigen der `Discontinued` Wert als Kontrollkästchen müssen wir unsere eigene CheckBox-Steuerelement hinzufügen. Die `Enabled` -Eigenschaftensatz auf "false", somit schreibgeschützt, und des Kontrollkästchen des `Checked` auf den Wert der Eigenschaft gebunden ist die `Discontinued` Feld "Daten".

Mit der `ItemTemplate` abgeschlossen ist, wird die Produktinformationen auf deutlich flexiblere Weise angezeigt. Vergleichen Sie die DetailsView-Ausgabe aus dem letzten Tutorial (Abbildung 3), mit der Ausgabe von FormView generiert werden, in diesem Tutorial (Abbildung 4).


[![Die starren DetailsView-Ausgabe](using-the-formview-s-templates-vb/_static/image8.png)](using-the-formview-s-templates-vb/_static/image7.png)

**Abbildung 3**: die starren DetailsView Output ([klicken Sie, um das Bild in voller Größe anzeigen](using-the-formview-s-templates-vb/_static/image9.png))


[![Die fließende FormView-Ausgabe](using-the-formview-s-templates-vb/_static/image11.png)](using-the-formview-s-templates-vb/_static/image10.png)

**Abbildung 4**: die fließende FormView Output ([klicken Sie, um das Bild in voller Größe anzeigen](using-the-formview-s-templates-vb/_static/image12.png))


## <a name="summary"></a>Zusammenfassung

Obwohl die GridView und DetailsView-Steuerelemente ihre Ausgabe angepasst wird, verwenden von TemplateFields verfügen können, stellen beide weiterhin ihre Daten in einem Raster-ähnliches, erhalten Sie kastenförmige Format an. Für Zeiten, wenn ein einzelner Datensatz angezeigt werden muss, mit einem weniger strenge Layout, das FormView-Steuerelement ist die ideale Wahl. Wie DetailsView, rendert die FormView ein einzelnes Datensatzes aus der `DataSource`, aber im Gegensatz zu DetailsView besteht nur aus Vorlagen und unterstützt keine Felder.

Wie wir in diesem Tutorial gesehen haben, können FormView bei der Anzeige eines einzelnen Datensatzes für ein flexibleres Layout. In Zukunft Tutorials die DataList- oder Repeater-Steuerelemente, untersuchen wir die bieten den gleichen Grad an Flexibilität wie die FormsView, jedoch können mehrere Datensätze (z. B. GridView) anzeigen.

Viel Spaß beim Programmieren!

## <a name="about-the-author"></a>Der Autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor von sieben Büchern zu ASP/ASP.NET und Gründer von [4GuysFromRolla.com](http://www.4guysfromrolla.com), arbeitet mit Microsoft-Web-Technologien seit 1998. Er ist als ein unabhängiger Berater, Schulungsleiter und Autor. Sein neueste Buch wird [*Sams Schulen selbst ASP.NET 2.0 in 24 Stunden*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Er ist unter [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) oder über seinen Blog finden Sie unter [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Besonderen Dank an

Diese tutorialreihe wurde durch viele hilfreiche Reviewer überprüft. Führen Sie Prüfer für dieses Tutorial E.R. wurde Gärtner ein. Meine zukünftigen MSDN-Artikeln überprüfen möchten? Wenn dies der Fall ist, löschen Sie mir eine Linie an [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Zurück](using-templatefields-in-the-detailsview-control-vb.md)
> [Weiter](displaying-summary-information-in-the-gridview-s-footer-vb.md)
