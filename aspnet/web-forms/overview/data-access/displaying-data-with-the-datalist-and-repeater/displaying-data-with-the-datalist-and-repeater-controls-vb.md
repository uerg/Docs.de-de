---
uid: web-forms/overview/data-access/displaying-data-with-the-datalist-and-repeater/displaying-data-with-the-datalist-and-repeater-controls-vb
title: Anzeigen von Daten mit dem DataList und Wiederholungsmodul-Steuerelement (VB) | Microsoft Docs
author: rick-anderson
description: In den vorherigen Lernprogrammen haben wir des GridView-Steuerelements verwendet, um Daten anzuzeigen. Mit diesem Lernprogramm beginnen, untersuchen wir häufige Muster von Berichten mit erstellen...
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/13/2006
ms.topic: article
ms.assetid: 58618954-a9ed-4ca0-8c2d-95a5ffd9c03e
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/displaying-data-with-the-datalist-and-repeater/displaying-data-with-the-datalist-and-repeater-controls-vb
msc.type: authoredcontent
ms.openlocfilehash: 6aa7cb76295d18711d88dd9855b43b259b558060
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
---
<a name="displaying-data-with-the-datalist-and-repeater-controls-vb"></a>Anzeigen von Daten mit dem DataList und Wiederholungsmodul-Steuerelement (VB)
====================
durch [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Beispiel-App herunterladen](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_29_VB.exe) oder [PDF herunterladen](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/datatutorial29vb1.pdf)

> In den vorherigen Lernprogrammen haben wir des GridView-Steuerelements verwendet, um Daten anzuzeigen. Mit diesem Lernprogramm beginnen, untersuchen wir häufige Muster von Berichten mit den verschiedenen Steuerelementen erstellen die Grundlagen zum Anzeigen von Daten mit diesen Steuerelementen ab.


## <a name="introduction"></a>Einführung

In allen von den Beispielen in der Vergangenheit 28 Lernprogramme, wenn Sie mehrere Datensätze aus einer Datenquelle anzeigen wir, um die GridView-Steuerelements aktiviert. Die GridView rendert eine Zeile für jeden Datensatz in der Datenquelle der Datensatz s von Datenfeldern in Spalten angezeigt. Während die GridView ein Kinderspiel für die Anzeige, durchblättern, sortieren, bearbeiten und Löschen von Daten-macht, ist dessen Darstellung erhalten Sie ein wenig kastenförmige. Darüber hinaus führt das Markup verantwortlich für die GridView-s-Struktur dieses Problem behoben ist enthält ein HTML `<table>` mit einer Zeile einer Tabelle (`<tr>`) für jeden Datensatz und einer Tabellenzelle (`<td>`) für jedes Feld.

Um eine bessere Anpassung von Darstellung und gerenderten Markups bereitzustellen, wenn mehrere Datensätze anzeigen, ASP.NET 2.0 bietet die verschiedenen Steuerelemente (beide Waren auch verfügbar in ASP.NET-Version 1.x). Die verschiedenen Steuerelemente ihre Inhalte mithilfe von Vorlagen statt BoundFields, CheckBoxFields, ButtonFields, rendern und so weiter. Wie die GridView DataList als HTML gerendert wird `<table>`, jedoch können Sie mehrere Daten für Quelldatensätzen pro Tabellenzeile angezeigt werden. Repeater, wird auf der anderen Seite als was Sie explizit angeben, und ist ideal geeignet, wenn Sie präzise Kontrolle über das Markup ausgegeben, benötigen keine zusätzliches Markup gerendert.

Über die Lernprogramme Dutzend als Nächstes betrachten wir häufige Muster von Berichten mit den verschiedenen Steuerelementen erstellen die Grundlagen zum Anzeigen von Daten mit diesen Vorlagen Steuerelemente ab. Wir sehen, wie diese Steuerelemente formatiert Gewusst wie: Ändern des Layouts des Datenquellen-Datensätze im DataList, allgemeine Master-/Detail-Szenarien, Möglichkeiten zum Bearbeiten und Löschen von Daten, wie Datensätze durchblättern und so weiter.

## <a name="step-1-adding-the-datalist-and-repeater-tutorial-web-pages"></a>Schritt 1: Hinzufügen der DataList und Repeater Tutorial Webseiten

Bevor wir in diesem Lernprogramm beginnen, können Sie s, schalten Sie zuerst einen Moment Zeit, die ASP.NET-Seiten hinzufügen, die wir für dieses Lernprogramm und die nächste einige Lernprogramme Umgang mit Anzeige von Daten mit dem DataList und Repeater benötigen. Starten, indem Sie einen neuen Ordner erstellen, in das Projekt mit dem Namen `DataListRepeaterBasics`. Fügen Sie die folgenden fünf ASP.NET-Seiten in diesen Ordner, dass alle von ihnen für die Verwendung die Masterseite konfiguriert `Site.master`:

- `Default.aspx`
- `Basics.aspx`
- `Formatting.aspx`
- `RepeatColumnAndDirection.aspx`
- `NestedControls.aspx`


![Erstellen Sie einen Ordner DataListRepeaterBasics, und fügen Sie dem Tutorial ASP.NET-Seiten hinzu](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image1.png)

**Abbildung 1**: Erstellen einer `DataListRepeaterBasics` Ordner, und fügen Sie dem Tutorial ASP.NET-Seiten hinzu


Öffnen der `Default.aspx` Seite, und ziehen Sie die `SectionLevelTutorialListing.ascx` Benutzersteuerelement aus den `UserControls` Ordner auf die Entwurfsoberfläche. Dieses Benutzersteuerelement, die wir in erstellt die [Masterseiten und Websitenavigation](../introduction/master-pages-and-site-navigation-vb.md) Lernprogramm, zählt die Siteübersicht und zeigt die Lernprogramme aus dem aktuellen Abschnitt in einer Liste mit Aufzählungszeichen aus.


[![Das Benutzersteuerelement SectionLevelTutorialListing.ascx "default.aspx" hinzufügen](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image3.png)](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image2.png)

**Abbildung 2**: Hinzufügen der `SectionLevelTutorialListing.ascx` Benutzersteuerelement `Default.aspx` ([klicken Sie hier, um das Bild in voller Größe angezeigt](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image4.png))


Um die Liste mit Aufzählungszeichen Anzeige haben müssen wir die DataList und Repeater Lernprogramme, die wir erstellen, die um sie in der Siteübersicht hinzuzufügen. Öffnen der `Web.sitemap` Datei, und fügen Sie das folgende Markup nach Hinzufügen von benutzerdefinierten Schaltflächen Standort Zuordnung Knoten Markup hinzu:


[!code-xml[Main](displaying-data-with-the-datalist-and-repeater-controls-vb/samples/sample1.xml)]


![Aktualisieren Sie die Website-Karte, um die neue ASP.NET-Seiten enthalten](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image5.png)

**Abbildung 3**: Aktualisieren Sie die Website-Karte, um die neue ASP.NET-Seiten enthalten


## <a name="step-2-displaying-product-information-with-the-datalist"></a>Schritt 2: Anzeigen von Produktinformationen mit DataList

Ähnlich wie die FormView DataList-Steuerelement s, die gerenderte Ausgabe richtet sich nach Vorlagen statt BoundFields, CheckBoxFields und So weiter. Im Gegensatz zu den FormView DataList soll eine Reihe von Datensätze, statt eine bezugslosen anzuzeigen. Lassen Sie s, die mit diesem Lernprogramm mit einem Blick auf die Produkt-Bindungsinformationen in einem DataList beginnen. Öffnen Sie zunächst die `Basics.aspx` auf der Seite der `DataListRepeaterBasics` Ordner. Ziehen Sie anschließend eine DataList aus der Toolbox in den Designer. Abbildung 4 zeigt, bevor Sie die Vorlagen DataList s angeben, zeigt der Designer es als ein "graues Feld" an.


[![Ziehen Sie DataList wird aus der Toolbox in den Designer.](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image7.png)](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image6.png)

**Abbildung 4**: Ziehen Sie das DataList aus der Toolbox auf die Designer ([klicken Sie hier, um das Bild in voller Größe angezeigt](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image8.png))


Von DataList s Smarttag, fügen Sie eine neue ObjectDataSource hinzu und konfigurieren, um die Verwendung der `ProductsBLL` Klasse s `GetProducts` Methode. Da wir re DataList schreibgeschützt sind und in diesem Lernprogramm erstellen festgelegt die Dropdown-Liste (keine), in den Assistenten s einfügen, aktualisieren und Löschen von Registerkarten.


[![Zum Erstellen einer neuen ObjectDataSource abonnieren](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image10.png)](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image9.png)

**Abbildung 5**: Erstellen einer neuen ObjectDataSource Opt ([klicken Sie hier, um das Bild in voller Größe angezeigt](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image11.png))


[![Konfigurieren der ObjectDataSource zur Verwendung der ProductsBLL-Klasse](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image13.png)](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image12.png)

**Abbildung 6**: Konfigurieren der ObjectDataSource verwenden die `ProductsBLL` Klasse ([klicken Sie hier, um das Bild in voller Größe angezeigt](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image14.png))


[![Abrufen von Informationen zu allen Produkten mithilfe der GetProducts-Methode](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image16.png)](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image15.png)

**Abbildung 7**: Abrufen von Informationen über alle der Produkte unter Verwendung der `GetProducts` Methode ([klicken Sie hier, um das Bild in voller Größe angezeigt](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image17.png))


Nach dem Konfigurieren der ObjectDataSource und DataList über seine Smarttag zuordnen, Visual Studio erstellt automatisch ein `ItemTemplate` in DataList, die den Namen und Wert der einzelnen Datenfelder, die von der Datenquelle zurückgegebenen anzeigt (finden Sie unter der Markup unten). Diese Standardeinstellung `ItemTemplate` s Darstellung identisch mit der Vorlagen automatisch erstellt, wenn die FormView mithilfe des Designers eine Datenquelle gebunden ist.


[!code-aspx[Main](displaying-data-with-the-datalist-and-repeater-controls-vb/samples/sample2.aspx)]

> [!NOTE]
> Beachten Sie, dass beim Binden einer Datenquelle an ein Steuerelement FormView über das Smarttag für die FormView s Visual Studio erstellt eine `ItemTemplate`, `InsertItemTemplate`, und `EditItemTemplate`. Mit der DataList, jedoch nur eine `ItemTemplate` wird erstellt. Dies ist, weil das DataList nicht das gleiche integrierte bearbeiten und Einfügen der Unterstützung von FormView angeboten wird. DataList bearbeiten und löschen-bezogene Ereignisse enthalten, und bearbeiten und Löschen von Unterstützung können hinzugefügt werden mit einem gewissen Grad Code, sondern es s keine einfache Out-of-Box-Unterstützung als mit der FormView. Wir sehen zum Bearbeiten und Löschen von Unterstützung mit DataList in einem späteren Lernprogramm einschließen.


Lassen Sie s einen Moment Zeit, die Darstellung dieser Vorlage zu verbessern. Können Sie anstelle der Anzeige der Datenfelder, s, die nur angezeigt, die s-Produktname, die Lieferanten, die Kategorie, die Menge pro Einheit und Preis pro Einheit. Darüber hinaus können s zeigen Sie die Bezeichnung in ein `<h4>` Überschrift und Layout für die verbleibenden Felder, die mit einem `<table>` unter der Überschrift.

Um diese Änderungen vorzunehmen, die Sie können, verwenden entweder die Vorlage, die Bearbeitungsfunktionen in den Designer DataList s smart Tag klicken Sie auf den Link Vorlagen bearbeiten oder ändern Sie die Vorlage manuell über die Seite s deklarative Syntax. Wenn Sie die Vorlagen bearbeiten-Option im Designer verwenden, das resultierende Markup möglicherweise nicht das folgende Markup exakt überein, aber bei der Anzeige über ein Browser sollte ungefähr sehr im Screenshot, der in Abbildung 8 gezeigt.


[!code-aspx[Main](displaying-data-with-the-datalist-and-repeater-controls-vb/samples/sample3.aspx)]

> [!NOTE]
> Im Beispiel oben verwendet Web Bezeichnung steuert, dessen `Text` Eigenschaft der Wert der Databinding-Syntax zugewiesen. Alternativ können Sie haben wir die Bezeichnungen vollständig ausgelassen nur die Databinding-Syntax eingeben. D. h. anstatt `<asp:Label ID="CategoryNameLabel" runat="server" Text='<%# Eval("CategoryName") %>' />` wir stattdessen hätte die deklarative Syntax `<%# Eval("CategoryName") %>`.


Bietet in der Bezeichnung Websteuerelementen verlassen, jedoch zwei Vorteile. Erstens stellt sie eine einfachere Möglichkeit zum Formatieren der Daten basierend auf den Daten in den nächsten Lernprogrammen eingehendem ein. Zweitens wird die Option Vorlagen bearbeiten, in der Designer verfügt über t Anzeige deklarative Databinding-Syntax, die außerhalb von einigen Websteuerelement. Stattdessen die Vorlagen bearbeiten-Schnittstelle dient zum Arbeiten mit statischen Markup zu vereinfachen und Web-Steuerelemente sowie die wird davon ausgegangen, dass alle Databinding über das Dialogfeld DataBindings bearbeiten durchgeführt wird, zugegriffen werden kann von den Web Controls-Smarttags.

Daher bei der Arbeit mit DataList, das die Möglichkeit, bearbeiten die Vorlagen mithilfe des Designers bereitstellt, möchte ich Bezeichnung Websteuerelemente verwenden, damit der Inhalt über die Benutzeroberfläche für Vorlagen bearbeiten zugänglich ist. Sehen wir kurz, erfordert Repeater an, dass der Inhalt der s bearbeitet werden, aus der Datenquellensicht an. Wenn Objekt die Repeater s Vorlagen, die ich häufig die Bezeichnung Web weglassen, wird, wenn ich, dass ich müssen wissen so formatieren Sie Steuerelemente gebunden die Darstellung der Daten folglich Text basierend auf programmgesteuerte Logik.


[![Jedes Produkt s Ausgabe ist gerendert DataList s ItemTemplate mit](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image19.png)](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image18.png)

**Abbildung 8**: jedes Produkt s Ausgabe ist gerendert DataList s mit `ItemTemplate` ([klicken Sie hier, um das Bild in voller Größe angezeigt](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image20.png))


## <a name="step-3-improving-the-appearance-of-the-datalist"></a>Schritt 3: Verbessern der Darstellung eines DataList

Wie die GridView bietet DataList eine Reihe von Formatvorlagen bezogenen Steuerelementeigenschaften, z. B. `Font`, `ForeColor`, `BackColor`, `CssClass`, `ItemStyle`, `AlternatingItemStyle`, `SelectedItemStyle`und so weiter. Bei der Arbeit mit GridView und DetailsView erstellten Skin-Dateien in den `DataWebControls` Design, das vordefinierte der `CssClass` Eigenschaften für diese zwei Steuerelemente und die `CssClass` Eigenschaft für eine Reihe von deren untergeordneten (`RowStyle` `HeaderStyle`usw.). Lassen Sie s für das DataList verwenden müssen.

Wie in beschrieben die [Anzeigen von Daten mit der ObjectDataSource](../basic-reporting/displaying-data-with-the-objectdatasource-vb.md) Lernprogramm eine Designdatei gibt die Standardeigenschaften darstellungsbezogene für ein Websteuerelement; ein Design ist eine Sammlung von Design, CSS, Bild und JavaScript-Dateien, definieren ein bestimmtes Erscheinungsbild für eine Website. In der *Anzeigen von Daten mit der ObjectDataSource* Lernprogramm erstellten eine `DataWebControls` Design (die implementiert als Ordner innerhalb der `App_Themes` Ordner), die hat derzeit zwei Designdateien - `GridView.skin` und `DetailsView.skin`. Lassen Sie s eine dritte an die vordefinierte frameart-Einstellungen für das DataList-Designdatei hinzufügen.

Um eine Designdatei hinzuzufügen, mit der Maustaste auf die `App_Themes/DataWebControls` Ordner, wählen Sie ein neues Element hinzufügen, und wählen Sie die Option Designdatei aus der Liste. Nennen Sie die Datei `DataList.skin`.


[![Erstellen Sie eine neue Skin-Datei mit dem Namen DataList.skin](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image22.png)](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image21.png)

**Abbildung 9**: Erstellen einer neuen Skin-Datei mit dem Namen `DataList.skin` ([klicken Sie hier, um das Bild in voller Größe angezeigt](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image23.png))


Verwenden Sie das folgende Markup für die `DataList.skin` Datei:


[!code-aspx[Main](displaying-data-with-the-datalist-and-repeater-controls-vb/samples/sample4.aspx)]

Diese Einstellungen weisen die gleichen CSS-Klassen der entsprechenden DataList-Eigenschaften wie mit GridView und DetailsView verwendet wurden. Die CSS-Klassen, die hier verwendete `DataWebControlStyle`, `AlternatingRowStyle`, `RowStyle`usw. definiert werden, der `Styles.css` Datei, und klicken Sie in vorherigen Lernprogrammen wurden hinzugefügt.

Durch das Hinzufügen dieser Skin-Datei wird das DataList-s-Darstellung im Designer aktualisiert (möglicherweise müssen zum Aktualisieren der Ansicht-Designer, um die Auswirkungen der neuen Designdatei; aus dem Menü "Ansicht", wählen aktualisieren). Wie in Abbildung 10 gezeigt, hat jedes Produkt abwechselnde ein helles rosa Hintergrundfarbe.


[![Erstellen Sie eine neue Skin-Datei mit dem Namen DataList.skin](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image25.png)](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image24.png)

**Abbildung 10**: Erstellen einer neuen Skin-Datei mit dem Namen `DataList.skin` ([klicken Sie hier, um das Bild in voller Größe angezeigt](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image26.png))


## <a name="step-4-exploring-the-datalist-s-other-templates"></a>Schritt 4: Untersuchen der DataList s anderen Vorlagen

Zusätzlich zu den `ItemTemplate`, DataList unterstützt sechs anderer optionalen Vorlagen:

- `HeaderTemplate` Wenn angegeben, die Ausgabe eine Kopfzeile hinzugefügt und wird verwendet, um diese Zeile zu rendern.
- `AlternatingItemTemplate` abwechselnde Elemente gerendert.
- `SelectedItemTemplate` verwendet zum Rendern des ausgewählten Elements. das ausgewählte Element ist das Element, dessen Index der DataList s entspricht [ `SelectedIndex` Eigenschaft](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalist.selectedindex.aspx)
- `EditItemTemplate` verwendet zum Rendern des Elements, das bearbeitet wird
- `SeparatorTemplate` Wenn angegeben, fügt ein Trennzeichen zwischen den einzelnen Elementen und wird verwendet, um das Trennzeichen zu rendern.
- `FooterTemplate` -Wenn angegeben, die Ausgabe eine Fußzeile hinzugefügt und wird verwendet, um diese Zeile zu rendern.

Beim Angeben der `HeaderTemplate` oder `FooterTemplate`, DataList Fügt eine zusätzliche Zeile der Kopf- oder Fußzeile auf der gerenderten Ausgabe. Wie werden mit GridView s Kopf- und Fußzeilen Zeilen, Kopf- und Fußzeilen in einem DataList nicht an Daten gebunden. Aus diesem Grund jede Databinding-Syntax in der `HeaderTemplate` oder `FooterTemplate` , dass Versuche gebundene Daten, die Zugriff auf eine leere Zeichenfolge zurückgegeben werden.

> [!NOTE]
> Wie wir gesehen, in haben der [Anzeigen von Zusammenfassungsinformationen in die GridView s Fußzeile](../custom-formatting/displaying-summary-information-in-the-gridview-s-footer-vb.md) Lernprogramm, während die Kopf- und Fußzeile Zeilen t Unterstützung Databinding-Syntax, Daten-spezifische Informationen Verschlüsselungskennwort kann injiziert direkt in diese Zeilen aus der GridView-s `RowDataBound` -Ereignishandler. Diese Technik kann verwendet werden sowohl die laufende Summen berechnen oder andere Informationen aus den Daten an das Steuerelement gebunden sowie zuweisen, die Informationen in der Fußzeile. Dasselbe Konzept kann für die DataList und Repeater Steuerelemente angewendet werden. der einzige Unterschied ist, erstellen Sie einen Ereignishandler für das DataList und Repeater der `ItemDataBound` Ereignis (anstelle von für die `RowDataBound` Ereignis).


In unserem Beispiel Let s haben den Titel Produktinformationen angezeigt, die am oberen Rand der DataList-s-Ergebnisse in eine `<h3>` Überschrift. Um dies zu erreichen, fügen einen `HeaderTemplate` durch das entsprechende Markup. Designer dies geschieht durch Klicken auf den Link Vorlagen bearbeiten, in das Smarttag DataList s, die Header-Vorlage aus der Dropdown-Liste auswählen und im Text nach dem Auswählen der Option Überschrift3 aus der Dropdown-Stil eingeben (siehe Abbildung 11).


[![Fügen Sie eine HeaderTemplate mit die Produktinformationen Text hinzu](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image28.png)](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image27.png)

**Abbildung 11**: Hinzufügen einer `HeaderTemplate` mit die Produktinformationen Text ([klicken Sie hier, um das Bild in voller Größe angezeigt](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image29.png))


Alternativ können Sie dies kann hinzugefügt werden deklarativ durch Eingabe in das folgende Markup der `<asp:DataList>` Tags:


[!code-html[Main](displaying-data-with-the-datalist-and-repeater-controls-vb/samples/sample5.html)]

Wenn ein Bit Abstand zwischen den einzelnen Produktliste hinzufügen möchten, können Sie s Hinzufügen einer `SeparatorTemplate` , eine Linie zwischen jeder Abschnitt umfasst. Das horizontale Regel Tag (`<hr>`), fügt solche eine Trennlinie. Erstellen der `SeparatorTemplate` so, dass sie das folgende Markup:


[!code-html[Main](displaying-data-with-the-datalist-and-repeater-controls-vb/samples/sample6.html)]

> [!NOTE]
> Wie die `HeaderTemplate` und `FooterTemplates`die `SeparatorTemplate` nicht an jeden Datensatz aus der Datenquelle gebunden ist und daher nicht direkt zugreifen, die die Datenquelle Datensätze an das DataList gebunden.


Nach dem Herstellen dieser Zusatz beim Anzeigen der Seite über einen Browser sollte ähnlich wie Abbildung 12 aussehen. Beachten Sie die Kopfzeile und die Linie zwischen den einzelnen Produktliste.


[![DataList enthält eine Kopfzeile und eine horizontale Linie zwischen den einzelnen Produktliste](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image31.png)](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image30.png)

**Abbildung 12**: DataList enthält eine Kopfzeile und eine horizontale Regel zwischen jedem Produktliste ([klicken Sie hier, um das Bild in voller Größe angezeigt](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image32.png))


## <a name="step-5-rendering-specific-markup-with-the-repeater-control"></a>Schritt 5: Rendern von bestimmten Markup mit Wiederholungsmodul-Steuerelement

Stimmen Sie eine Quelle anzeigen/in Ihrem Browser beim Besuchen des DataList-Beispiels in Abbildung 12 sehen Sie, dass das DataList-HTML-ausgibt `<table>` , enthält eine Zeile einer Tabelle (`<tr>`) mit einer einzelnen Tabellenzelle (`<td>`) für jedes Element gebunden der DataList. Diese Ausgabe entspricht tatsächlich, was von einer GridView mit einer einzelnen TemplateField ausgegeben werden würde. Wie wir in einem späteren Lernprogramm sehen werden, lässt das DataList weitere Anpassung der Ausgabe, aktivieren uns, mehrere Datenquellen-Datensätze pro Tabellenzeile angezeigt.

Was geschieht, wenn t ein HTML-ausgeben möchten Verschlüsselungskennwort `<table>`, obwohl? Insgesamt und vollständige Kontrolle über das Markup generiert, die für ein Data-Websteuerelement müssen wir Wiederholungsmodul-Steuerelement verwenden. Wie das DataList Repeater basierend auf Vorlagen erstellt. Repeater, bietet jedoch nur die folgenden fünf Vorlagen:

- `HeaderTemplate` Wenn angegeben, fügt das angegebene Markup vor den Elementen
- `ItemTemplate` verwendet, um Elemente zu rendern.
- `AlternatingItemTemplate` Wenn angegeben, verwendet, um abwechselnde Elemente zu rendern.
- `SeparatorTemplate` Wenn angegeben, fügt das angegebene Markup zwischen den einzelnen Elementen
- `FooterTemplate` -Wenn angegeben, fügt das angegebene Markup nach den Elementen

In ASP.NET 1.x Repeater Steuerelement häufig verwendet wurde, um eine Liste mit Aufzählungszeichen angezeigt, deren Daten von einer Datenquelle stammen. In einem solchen Fall die `HeaderTemplate` und `FooterTemplates` enthält den öffnenden und schließenden `<ul>` , while-Tags, die `ItemTemplate` enthält `<li>` Elemente mit Databinding-Syntax. Dieser Ansatz kann trotzdem in ASP.NET 2.0 verwendet werden, wie wir gesehen, in beiden Beispielen in haben der [Masterseiten und Websitenavigation](../introduction/master-pages-and-site-navigation-vb.md) Lernprogramm:

- In der `Site.master` Masterseite ein Repeater wurde verwendet, um eine Aufzählung der Standort der obersten Ebene Zuordnung Inhalte (Basisberichte, Filtern von Berichten Formatierung angepasst und So weiter) angezeigt; eine andere, geschachtelte Repeater wurde verwendet, um die untergeordneten Elemente Abschnitte der Anzeige der Abschnitten auf oberster Ebene
- In `SectionLevelTutorialListing.ascx`, ein Repeater wurde verwendet, um eine Aufzählung der in den Abschnitten der untergeordneten Elemente des aktuellen Standorts Zuordnung Abschnitts anzeigen

> [!NOTE]
> ASP.NET 2.0 bietet die neuen [BulletedList-Steuerelement](https://msdn.microsoft.com/library/ms228101.aspx), die an ein Datenquellen-Steuerelement um eine einfache Liste mit Aufzählungszeichen anzuzeigen gebunden werden kann. Mit dem Steuerelement BulletedList müssen wir nicht zum Angeben der HTML-listenbezogene; Stattdessen zeigen wir einfach das Feld "Daten" als Text für jedes Listenelement angezeigt.


Repeater fungiert als ein Catch Gerätedaten Websteuerelement. Besteht kein vorhandenes Steuerelement, das die erforderliche Markup generiert, kann die Wiederholungsmodul-Steuerelement verwendet werden. Zur Veranschaulichung Repeater verwenden, können Sie s die Liste der Kategorien, die über das Produkt Informationen DataList in Schritt2 erstellte angezeigt haben. Insbesondere können s haben die Kategorien angezeigt, die in einer einzelnen Zeile HTML `<table>` mit jeder Kategorie, die als eine Spalte in der Tabelle angezeigt.

Um dies zu erreichen, ziehen eine Wiederholungsmodul-Steuerelement aus der Toolbox in den Designer, über das Produkt Informationen DataList starten. Wie bei der DataList werden Repeater Anfangs als graues Feld, bis dessen Vorlagen definiert wurden.


[![Fügen Sie in den Designer ein Repeater hinzu](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image34.png)](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image33.png)

**Abbildung 13**: Fügen Sie in den Designer einen Repeater ([klicken Sie hier, um das Bild in voller Größe angezeigt](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image35.png))


Dort s nur eine Option im Wiederholungsmodul s Smarttag: Datenquelle auswählen. Wahlweise eine neue ObjectDataSource erstellen und konfigurieren sie mithilfe der `CategoriesBLL` Klasse s `GetCategories` Methode.


[![Erstellen Sie eine neue ObjectDataSource](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image37.png)](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image36.png)

**Abbildung 14**: Erstellen einer neuen ObjectDataSource ([klicken Sie hier, um das Bild in voller Größe angezeigt](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image38.png))


[![Konfigurieren der ObjectDataSource zur Verwendung der CategoriesBLL-Klasse](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image40.png)](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image39.png)

**Abbildung 15**: Konfigurieren der ObjectDataSource verwenden die `CategoriesBLL` Klasse ([klicken Sie hier, um das Bild in voller Größe angezeigt](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image41.png))


[![Abrufen von Informationen über alle Kategorien, die mit der GetCategories-Methode](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image43.png)](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image42.png)

**Abbildung 16**: Abrufen von Informationen über alle der Kategorien unter Verwendung der `GetCategories` Methode ([klicken Sie hier, um das Bild in voller Größe angezeigt](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image44.png))


Im Gegensatz zu den DataList erstellt Visual Studio nicht automatisch ein ItemTemplate für Wiederholungsmoduls nach dem binden es an eine Datenquelle. Darüber hinaus die Repeater s Vorlagen können nicht konfiguriert werden, durch den Designer und deklarativ angegeben werden müssen.

Um die Kategorien anzuzeigen, als einzelne Zeile `<table>` mit einer Spalte für jede Kategorie, benötigen wir Repeater auszugebende Markup ähnlich dem folgenden:


[!code-html[Main](displaying-data-with-the-datalist-and-repeater-controls-vb/samples/sample7.html)]

Da die `<td>Category X</td>` Text ist der Teil, der wiederholt wird, wird dies im Wiederholungsmodul s ItemTemplate angezeigt,. Das Markup, das vor - `<table><tr>` -platziert werden, in der `HeaderTemplate` während der Endwert Markup - `</tr></table>` -platziert, der `FooterTemplate`. Wenn diese Vorlageneinstellungen eingeben möchten, wechseln Sie zu der deklarativen Teil der ASP.NET-Seite durch Klicken auf die Schaltfläche "Quelle", in der unteren linken Ecke und geben Sie die folgende Syntax:


[!code-aspx[Main](displaying-data-with-the-datalist-and-repeater-controls-vb/samples/sample8.aspx)]

Repeater ausgibt, entsprechend den Angaben von dessen Vorlagen, nichts, nothing weniger präzise Markup. Abbildung 17 zeigt die Repeater s Ausgabe, wenn Sie über einen Browser angezeigt.


[![Ein einzeiliges HTML &lt;Tabelle&gt; listet jede Kategorie in einer separaten Spalte](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image46.png)](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image45.png)

**Abbildung 17**: ein einzeiliges HTML `<table>` listet jede Kategorie in einer separaten Spalte ([klicken Sie hier, um das Bild in voller Größe angezeigt](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image47.png))


## <a name="step-6-improving-the-appearance-of-the-repeater"></a>Schritt 6: Verbessern der Darstellung des Wiederholungsmoduls

Da Repeater genau das Markup gemäß dessen Vorlagen ausgibt, muss als keine Überraschung liegen, dass keine Formatvorlagen bezogenen Eigenschaften für die Repeater vorhanden sind. Um die Darstellung des Inhalts von Wiederholungsmoduls generierten zu ändern, müssen wir den erforderlichen HTML- oder CSS-Inhalt manuell direkt an den Vorlagen Repeater s hinzufügen.

In unserem Beispiel können Sie s die alternative Hintergrundfarben, wie mit den abwechselnden Zeilen in der DataList Kategorie-Spalten aufweisen. Um dies zu erreichen, müssen wir weisen die `RowStyle` CSS-Klasse, um jedes Element Repeater und `AlternatingRowStyle` CSS-Klasse, um jede abwechselnde Repeater Element über die `ItemTemplate` und `AlternatingItemTemplate` Vorlagen wie folgt:


[!code-aspx[Main](displaying-data-with-the-datalist-and-repeater-controls-vb/samples/sample9.aspx)]

Lassen Sie s, die die Ausgabe mit dem Text Product Categories auch eine Kopfzeile hinzugefügt. Da wir Verschlüsselungskennwort t wissen, wie viele Spalten unsere resultierende `<table>` wird umfassen, ist die einfachste Möglichkeit, eine Kopfzeile generieren, die alle Spalten umfassen garantiert verwenden *zwei* `<table>` s. Die erste `<table>` enthält zwei Zeilen der Kopfzeile und eine Zeile, die die zweite, einzeiliges enthält `<table>` , die eine Spalte für jede Kategorie in das System hat. D. h., möchten wir das folgende Markup ausgeben:


[!code-html[Main](displaying-data-with-the-datalist-and-repeater-controls-vb/samples/sample10.html)]

Die folgenden `HeaderTemplate` und `FooterTemplate` dazu führen, das gewünschte Markup:


[!code-aspx[Main](displaying-data-with-the-datalist-and-repeater-controls-vb/samples/sample11.aspx)]

Abbildung 18 zeigt Repeater, nachdem diese Änderungen vorgenommen wurden.


[![Die Kategorie-Spalten in Background-Farbe Alternate und enthält eine Kopfzeile](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image49.png)](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image48.png)

**Abbildung 18**: die Kategorie Spalten alternative Hintergrundfarbe und enthält eine Kopfzeile ([klicken Sie hier, um das Bild in voller Größe angezeigt](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image50.png))


## <a name="summary"></a>Zusammenfassung

Während des GridView-Steuerelements anzuzeigenden erleichtert, ist die bearbeiten, löschen, Sortieren und Paging durch die Daten, auf die Darstellung, sehr erhalten Sie kastenförmige und Raster-ähnliches. Zur besseren Steuerung der Darstellung müssen wir an der DataList oder Repeater Steuerelemente zu aktivieren. Diese Steuerelemente zeigen eine Gruppe von Datensätzen mithilfe von Vorlagen anstelle von BoundFields, CheckBoxFields und So weiter.

DataList als HTML rendert `<table>` , die standardmäßig zeigt jeden Datensatz der Datenquelle in einer einzelnen Tabellenzeile, wie eine GridView mit einer einzelnen TemplateField. Wie wir in einem späteren Lernprogramm sehen werden, jedoch lässt das DataList mehrere Datensätze pro Tabellenzeile angezeigt werden. Die Repeater ausgibt andererseits, ausschließlich das Markup, das in seiner Vorlagen angegeben; Es werden keine zusätzliches Markup hinzugefügt und aus diesem Grund wird häufig verwendet, um Daten im HTML-Elemente anzuzeigen, außer einem `<table>` (z. B. in einer Liste mit Aufzählungszeichen).

Während die DataList und Repeater mehr Flexibilität bei der gerenderten Ausgabe anbieten, fehlen viele integrierte Funktionen in der GridView gefunden. Wie wir in zukünftigen Lernprogrammen untersucht werden, einige dieser Funktionen kann wieder bei ohne zu viel Aufwand angeschlossen werden jedoch führen Denken Sie daran, die verwenden weiterhin das DataList oder Repeater anstelle der GridView schränkt die Features, die Sie verwenden können, ohne dass diese Funktionen zu implementieren selbst.

Viel Spaß beim Programmieren!

## <a name="about-the-author"></a>Informationen zum Autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor von sieben ASP/ASP.NET-Büchern und Gründer von [4GuysFromRolla.com](http://www.4guysfromrolla.com), Microsoft Web-Technologien seit 1998 arbeitet. Scott fungiert als ein unabhängiger Berater, Trainer und Writer. Sein neueste Buch wird [ *Sams Schulen selbst ASP.NET 2.0 in 24 Stunden*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Er die erreicht werden kann, zur [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) oder über seinen Blog die finden Sie unter [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Besonderen Dank an

Diese Reihe von Lernprogrammen wurde durch viele nützliche Bearbeiter überprüft. Lead Prüfer für dieses Lernprogramm wurden Yaakov Ellis, Liz Shulok Randy Schmidt und Stacy Park. Meine bevorstehende MSDN-Artikel Überprüfen von Interesse? Wenn dies der Fall ist, löschen Sie mich zeilenweise [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Zurück](nested-data-web-controls-cs.md)
> [Weiter](formatting-the-datalist-and-repeater-based-upon-data-vb.md)
