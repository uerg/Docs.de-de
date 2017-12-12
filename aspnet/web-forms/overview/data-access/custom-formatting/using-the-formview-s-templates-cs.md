---
uid: web-forms/overview/data-access/custom-formatting/using-the-formview-s-templates-cs
title: Die FormView-Vorlagen (c#) mit | Microsoft Docs
author: rick-anderson
description: Im Gegensatz zu DetailsView, besteht FormView keine Felder. Stattdessen wird die FormView gerendert mithilfe von Vorlagen. Untersuchen wir in diesem Lernprogramm verwenden den f...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/31/2010
ms.topic: article
ms.assetid: d3f062af-88cf-426d-af44-e41f32c41672
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/custom-formatting/using-the-formview-s-templates-cs
msc.type: authoredcontent
ms.openlocfilehash: 18e76a763e22c0d1046acc60e095bbd11960c5e6
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="using-the-formviews-templates-c"></a>Verwenden die FormView-Vorlagen (c#)
====================
durch [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Beispiel-App herunterladen](http://download.microsoft.com/download/9/6/9/969e5c94-dfb6-4e47-9570-d6d9e704c3c1/ASPNET_Data_Tutorial_14_CS.exe) oder [PDF herunterladen](using-the-formview-s-templates-cs/_static/datatutorial14cs1.pdf)

> Im Gegensatz zu DetailsView, besteht FormView keine Felder. Stattdessen wird die FormView gerendert mithilfe von Vorlagen. Verwenden in diesem Lernprogramm untersuchenden der FormView-Steuerelement eine weniger strenge Anzeige von Daten zu präsentieren.


## <a name="introduction"></a>Einführung

In den letzten beiden Lernprogrammen wurde erläutert, wie Steuerelemente GridView und DetailsView-Ausgaben, die mithilfe von TemplateFields anpassen. Von TemplateFields ermöglicht wird, für den Inhalt für ein bestimmtes Feld hoch angepasst werden, aber am Ende der GridView und die DetailsView haben ein stattdessen erhalten Sie kastenförmige, Raster-ähnliches Erscheinungsbild. Solche ein Raster-ähnliches Layout ist ideal für viele Szenarien, aber manchmal eine flexiblere, weniger strenge Anzeige erforderlich ist. Zum Anzeigen von einem einzelnen Datensatz ist flüssigen Layout möglich, mithilfe des FormView-Steuerelements.

Im Gegensatz zu DetailsView, besteht FormView keine Felder. Sie können keine FormView eine BoundField- oder TemplateField hinzufügen. Stattdessen wird die FormView gerendert mithilfe von Vorlagen. Die FormView als DetailsView-Steuerelement, das ein einzelnes TemplateField enthält vorstellen. Die FormView unterstützt die folgenden Vorlagen:

- `ItemTemplate`verwendet, um den entsprechenden Datensatz in die FormView angezeigt Rendern
- `HeaderTemplate`zur Angabe einer optionalen Kopfzeile
- `FooterTemplate`zur Angabe einer optionalen Fußzeile
- `EmptyDataTemplate`Wenn die FormView `DataSource` verfügt nicht über alle Datensätze der `EmptyDataTemplate` wird anstelle von verwendet die `ItemTemplate` für das Rendern von Markup des Steuerelements
- `PagerTemplate`kann verwendet werden, die Paging-Schnittstelle für FormViews anpassen, die Paging aktiviert haben
- `EditItemTemplate` / `InsertItemTemplate`verwendet, um die Bearbeitungsoberfläche oder Einfügen von Schnittstelle für FormViews anzupassen, die solche Funktionen unterstützen

Verwenden in diesem Lernprogramm untersuchenden der FormView-Steuerelement eine weniger strenge Anzeige von Produkten darstellen. Statt später durch die Felder für Name, Kategorie, Lieferanten und usw. FormView des `ItemTemplate` zeigt diese Werte mit einer Kombination aus einem Header-Element und ein `<table>` (siehe Abbildung 1).


[![Die FormView unterbricht Raster-ähnliches Layout in der DetailsView angezeigt](using-the-formview-s-templates-cs/_static/image2.png)](using-the-formview-s-templates-cs/_static/image1.png)

**Abbildung 1**: FormView wird aus der Grid-Like Layout gesehen in DetailsView unterbrochen ([klicken Sie hier, um das Bild in voller Größe angezeigt](using-the-formview-s-templates-cs/_static/image3.png))


## <a name="step-1-binding-the-data-to-the-formview"></a>Schritt 1: Binden von Daten an die FormView

Öffnen der `FormView.aspx` Seite und einen FormView aus der Toolbox in den Designer ziehen. Beim erstmaligen FormView hinzufügen erscheint als graues Feld, informieren uns, die eine `ItemTemplate` ist erforderlich.


[![Die FormView kann nicht im Designer nicht gerendert werden, bis ein ItemTemplate bereitgestellt wird](using-the-formview-s-templates-cs/_static/image5.png)](using-the-formview-s-templates-cs/_static/image4.png)

**Abbildung 2**: die FormView nicht gerendert werden, in den Designer, bis ein `ItemTemplate` bereitgestellt wird ([klicken Sie hier, um das Bild in voller Größe angezeigt](using-the-formview-s-templates-cs/_static/image6.png))


Die `ItemTemplate` (über die deklarative Syntax) manuell erstellt werden oder können durch binden die FormView an ein Datenquellen-Steuerelement mithilfe des Designers werden automatisch erstellt. Dies automatisch erstellten `ItemTemplate` enthält HTML-Code, um Listen der Namen jedes Feld und einer Bezeichnung, deren steuern `Text` Eigenschaft den Wert des Felds gebunden ist. Dieser Ansatz auch Auto-erstellt ein `InsertItemTemplate` und `EditItemTemplate`, beide mit Eingabesteuerelemente aufgefüllt werden, für die einzelnen Datenfelder, die von der Datenquelle zurückgegeben.

Wenn Sie automatisch die Vorlage erstellen möchten, aus der FormView-Smarttag hinzufügen ein neues ObjectDataSource-Steuerelement, die aufruft, die `ProductsBLL` Klasse `GetProducts()` Methode. Dadurch wird einen FormView mit erstellt ein `ItemTemplate`, `InsertItemTemplate`, und `EditItemTemplate`. Entfernen Sie aus der Datenquellensicht an, die `InsertItemTemplate` und `EditItemTemplate` da wir nicht interessiert sind einen FormView erstellen, bearbeiten oder Einfügen von noch unterstützt. Als Nächstes deaktivieren Sie das Markup in der `ItemTemplate` , damit wir mit eine bereinigten Liste ausgeführt, mit dem Sie arbeiten müssen.

Wenn Sie lieber, werden erstellen würde die `ItemTemplate` manuell, können Sie hinzufügen und konfigurieren Sie das ObjectDataSource, indem Sie diese aus der Toolbox in den Designer ziehen. Jedoch festlegen Sie nicht die FormView-Datenquelle aus dem Designer. Stattdessen wechseln Sie zur Quellansicht und manuell festlegen, die FormView `DataSourceID` Eigenschaft, um die `ID` Wert von "ObjectDataSource". Als Nächstes fügen Sie manuell die `ItemTemplate`.

Unabhängig davon, welchen Ansatz Sie sich entschieden, in Anspruch nehmen, zu diesem Zeitpunkt Ihrer FormView deklarative Markup aussehen sollte:


[!code-aspx[Main](using-the-formview-s-templates-cs/samples/sample1.aspx)]

Nehmen Sie einen Moment Zeit, um das Paging aktivieren, in der FormView-Smarttag Kontrollkästchen; Dadurch wird hinzugefügt der `AllowPaging="True"` -Attribut auf die FormView-Deklarationssyntax. Legen Sie außerdem die `EnableViewState` Eigenschaft auf "false".

## <a name="step-2-defining-theitemtemplates-markup"></a>Schritt 2: Definieren der`ItemTemplate`Markup

Mit FormView an das ObjectDataSource-Steuerelement gebunden und konfiguriert zur Unterstützung von Paging wir jetzt an den Inhalt für die `ItemTemplate`. Für dieses Lernprogramm den Namen des Produkts im angezeigten wir haben eine `<h3>` Überschrift. Verwenden Sie nach, dass wir ein HTML `<table>` zum Anzeigen von der verbleibenden Produkteigenschaften in eine Tabelle mit vier Spalten, in dem die erste und dritte Spalte aus, die Eigenschaftennamen und ihre Werte aufgeführt, die zweiten und vierten.

Dieses Markup kann eingegeben wird, über die FormView Vorlage bearbeiten Schnittstelle im Designer oder manuell über die deklarative Syntax eingegeben werden. Beim Arbeiten mit Vorlagen finden ich in der Regel es schneller, direkt mit der deklarativen Syntax arbeiten, aber wünschen, verwenden die geeignete Methode, die Sie am häufigsten mit vertraut sind.

Das folgende Markup zeigt das FormView deklarative Markup nach dem `ItemTemplate`Struktur wurde abgeschlossen:


[!code-aspx[Main](using-the-formview-s-templates-cs/samples/sample2.aspx)]

Beachten Sie, dass die Syntax der Databinding - `<%# Eval("ProductName") %>`für Beispiel direkt in der Ausgabe der Vorlage eingefügt werden kann. D. h. es muss nicht zugewiesen werden ein Label-Steuerelement `Text` Eigenschaft. Angenommen, wir haben die `ProductName` in der angezeigten Werten ein `<h3>` Element mit `<h3><%# Eval("ProductName") %></h3>`, die für das Produkt als Chai gerendert werden `<h3>Chai</h3>`.

Die `ProductPropertyLabel` und `ProductPropertyValue` CSS-Klassen dienen zum Angeben der Stil der Product-Eigenschaftennamen und Werte in der `<table>`. Diese CSS-Klassen sind in definierten `Styles.css` und dazu führen, dass die Eigenschaftennamen, fett und rechts ausgerichtet sein und eine Auffüllung der Eigenschaftswerte Recht hinzufügen.

Da es sich um keine CheckBoxFields zur Verfügung, mit dem FormView zum Anzeigen der `Discontinued` Wert als ein Kontrollkästchen müssen wir unser eigenes CheckBox-Steuerelement hinzufügen. Die `Enabled` Eigenschaftensatz wird auf "false", und somit schreibgeschützt, und des Kontrollkästchen `Checked` auf den Wert der Eigenschaft gebunden ist die `Discontinued` Feld "Daten".

Mit der `ItemTemplate` abgeschlossen ist, wird die Produktinformationen in einer weitaus flüssigen Weise angezeigt. Vergleichen Sie die DetailsView-Ausgabe aus dem letzten Lernprogramm (Abbildung 3), mit der Ausgabe von FormView generiert wird, in diesem Lernprogramm (Abbildung 4).


[![Die starren DetailsView-Ausgabe](using-the-formview-s-templates-cs/_static/image8.png)](using-the-formview-s-templates-cs/_static/image7.png)

**Abbildung 3**: die starren DetailsView Output ([klicken Sie hier, um das Bild in voller Größe angezeigt](using-the-formview-s-templates-cs/_static/image9.png))


[![Die flüssigen FormView-Ausgabe](using-the-formview-s-templates-cs/_static/image11.png)](using-the-formview-s-templates-cs/_static/image10.png)

**Abbildung 4**: die Fluid FormView Output ([klicken Sie hier, um das Bild in voller Größe angezeigt](using-the-formview-s-templates-cs/_static/image12.png))


## <a name="summary"></a>Zusammenfassung

Obwohl die GridView und DetailsView ihre Ausgabe mithilfe von TemplateFields angepasst haben können, stellen sowohl weiterhin ihre Daten in einem Raster-ähnliches, erhalten Sie kastenförmige Format an. Für die Zeiten, wenn ein einzelner Datensatz angezeigt werden muss, mit einem weniger strenge Layout, FormView ist eine ideale Option. Wie DetailsView, FormView rendert einen einzelnen Datensatz aus seiner `DataSource`, aber im Gegensatz zu DetailsView besteht nur Vorlagen und unterstützt keine Felder.

Wie wir in diesem Lernprogramm gesehen haben, können FormView bei der Anzeige eines einzelnen Datensatzes für ein flexibleres Layout. In zukünftigen Lernprogrammen DataList und Repeater-Steuerelemente untersuchen wir, die das gleiche Maß an Flexibilität als die FormsView bereitstellen, sondern können mehrere Datensätze (z. B. die GridView) anzeigen.

Viel Spaß beim Programmieren!

## <a name="about-the-author"></a>Informationen zum Autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor von sieben ASP/ASP.NET-Büchern und Gründer von [4GuysFromRolla.com](http://www.4guysfromrolla.com), Microsoft Web-Technologien seit 1998 arbeitet. Scott fungiert als ein unabhängiger Berater, Trainer und Writer. Sein neueste Buch wird [ *Sams Schulen selbst ASP.NET 2.0 in 24 Stunden*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Er die erreicht werden kann, zur [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) oder über seinen Blog die finden Sie unter [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Besonderen Dank an

Diese Reihe von Lernprogrammen wurde durch viele nützliche Bearbeiter überprüft. Lead Prüfer für dieses Lernprogramm wurde E.R. Gärtner ein. Meine bevorstehende MSDN-Artikel Überprüfen von Interesse? Wenn dies der Fall ist, löschen Sie mich zeilenweise [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[Zurück](using-templatefields-in-the-detailsview-control-cs.md)
[Weiter](displaying-summary-information-in-the-gridview-s-footer-cs.md)
