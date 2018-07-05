---
uid: web-forms/overview/data-access/displaying-data-with-the-datalist-and-repeater/displaying-data-with-the-datalist-and-repeater-controls-cs
title: Anzeigen von Daten mit dem DataList- und Wiederholungssteuerelement (c#) | Microsoft-Dokumentation
author: rick-anderson
description: In den vorherigen Tutorials haben wir das GridView-Steuerelement verwendet, um Daten anzuzeigen. In diesem Tutorial beginnen, betrachten wir allgemeine reporting Muster mit erstellen...
ms.author: aspnetcontent
ms.date: 09/13/2006
ms.assetid: 0591cacc-b34b-4cf6-885e-2c9953bb0946
msc.legacyurl: /web-forms/overview/data-access/displaying-data-with-the-datalist-and-repeater/displaying-data-with-the-datalist-and-repeater-controls-cs
msc.type: authoredcontent
ms.openlocfilehash: 6368580d43e6216ea75d3013fb13aa59d9eeb6ba
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2018
ms.locfileid: "37805941"
---
<a name="displaying-data-with-the-datalist-and-repeater-controls-c"></a>Anzeigen von Daten mit dem DataList- und Wiederholungssteuerelement (c#)
====================
durch [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Beispiel-App herunter](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_29_CS.exe) oder [PDF-Datei herunterladen](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/datatutorial29cs1.pdf)

> In den vorherigen Tutorials haben wir das GridView-Steuerelement verwendet, um Daten anzuzeigen. In diesem Tutorial beginnen, sehen wir uns erstellen allgemeine reporting Muster mit dem DataList- und Wiederholungssteuerelement-Steuerelementen, beginnend mit den Grundlagen von Daten mit diesen Steuerelementen anzeigt.


## <a name="introduction"></a>Einführung

In allen Beispielen in der Vergangenheit 28 Tutorials, wenn Sie mehrere Datensätze aus einer Datenquelle anzeigen, wir auf das GridView-Steuerelement aktiviert. GridView rendert eine Zeile für jeden Datensatz in der Datenquelle, die Datenfelder Datensatz s in Spalten angezeigt. Während die GridView ein Kinderspiel für die Anzeige, durchblättern, sortieren, bearbeiten und Löschen von Daten macht, ist seine Darstellung erhalten Sie ein wenig kastenförmige. Darüber hinaus das Markup, das verantwortlich für die GridView-s-Struktur das Problem behoben ist enthält eine HTML `<table>` mit einer Zeile einer Tabelle (`<tr>`) für jeden Datensatz und einer Tabellenzelle (`<td>`) für jedes Feld.

Um einen höheren Grad der Anpassung in die Darstellung und das gerenderte Markup bereitzustellen, wenn mehrere Einträge angezeigt, bietet ASP.NET 2.0 die Steuerelemente DataList- oder Wiederholungssteuerelement (beide Waren auch verfügbar in ASP.NET-Version 1.x). Die DataList- oder Repeater-Steuerelemente, ihre Inhalte mithilfe von Vorlagen anstelle von BoundFields, CheckBoxFields, ButtonFields, rendern und so weiter. Wie GridView, DataList-Steuerelement, die als HTML rendert `<table>`, jedoch können für mehrere Datenquellen-Datensätze, die pro Zeile der Tabelle angezeigt werden. Repeater, wird auf der anderen Seite als was Sie explizit angeben, und ist ein idealer Kandidat aus, wenn Sie präzise Kontrolle über das Markup ausgegeben, benötigen keine zusätzliches Markup gerendert.

Über den Tutorials Dutzend als Nächstes betrachten wir erstellen allgemeine reporting Muster mit dem DataList- und Wiederholungssteuerelement-Steuerelemente, die Grundlagen zum Anzeigen von Daten mit diesen Vorlagen Steuerelemente ab. Erfahren Sie, wie so formatieren Sie diese Steuerelemente, wie Sie ändern das Layout des Datenquellen-Datensätze im DataList, Master-/Detail-Szenarios, Möglichkeiten zum Bearbeiten und Löschen von Daten, wie Seite durch die Datensätze und so weiter.

## <a name="step-1-adding-the-datalist-and-repeater-tutorial-web-pages"></a>Schritt 1: Hinzufügen des DataList- und Wiederholungssteuerelement Tutorial von Webseiten

Bevor wir in diesem Tutorial beginnen, können Sie zuerst nehmen einen Moment Zeit, um die ASP.NET-Seiten hinzuzufügen, benötigen wir für dieses Lernprogramm sowie die nächsten einige Lernprogramme Umgang mit Anzeigen von Daten mit dem DataList- und Wiederholungssteuerelement s ein. Zunächst erstellen Sie einen neuen Ordner im Projekt mit dem Namen `DataListRepeaterBasics`. Fügen Sie die folgenden fünf ASP.NET-Seiten in diesen Ordner, wenn all diese Einstellungen zu konfigurieren, um die Masterseite verwenden `Site.master`:

- `Default.aspx`
- `Basics.aspx`
- `Formatting.aspx`
- `RepeatColumnAndDirection.aspx`
- `NestedControls.aspx`


![Erstellen Sie einen Ordner DataListRepeaterBasics, und fügen Sie die Tutorial ASP.NET-Seiten](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image1.png)

**Abbildung 1**: Erstellen einer `DataListRepeaterBasics` Ordner, und fügen Sie dem Tutorial ASP.NET-Seiten hinzu


Öffnen der `Default.aspx` Seite, und ziehen Sie die `SectionLevelTutorialListing.ascx` Benutzersteuerelement aus der `UserControls` Ordner auf die Entwurfsoberfläche. Dieses Benutzersteuerelement, die wir in den erstellt die [Masterseiten und Sitenavigation](../introduction/master-pages-and-site-navigation-cs.md) Tutorial, listet die Sitemap und zeigt Sie in den Tutorials aus dem aktuellen Abschnitt in einer Liste mit Aufzählungszeichen.


[![Fügen Sie das SectionLevelTutorialListing.ascx-Benutzersteuerelement an "default.aspx"](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image3.png)](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image2.png)

**Abbildung 2**: Hinzufügen der `SectionLevelTutorialListing.ascx` Benutzersteuerelement `Default.aspx` ([klicken Sie, um das Bild in voller Größe anzeigen](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image4.png))


Um die Anzeige der Liste mit Aufzählungszeichen müssen wir die DataList- oder Wiederholungssteuerelement Lernprogramme, die wir erstellen, die um sie der Sitemap hinzuzufügen. Öffnen der `Web.sitemap` Datei, und fügen Sie das folgende Markup nach dem Hinzufügen von benutzerdefinierten Schaltflächen Site Map Knoten Markup hinzu:


[!code-xml[Main](displaying-data-with-the-datalist-and-repeater-controls-cs/samples/sample1.xml)]


![Aktualisieren Sie die Website-Karte, um die neuen ASP.NET-Seiten enthalten](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image5.png)

**Abbildung 3**: Aktualisieren Sie die Website-Karte, um die neuen ASP.NET-Seiten enthalten


## <a name="step-2-displaying-product-information-with-the-datalist"></a>Schritt 2: Anzeigen von Produktinformationen und das DataList-Steuerelement

Ähnlich wie das FormView-Steuerelement, hängt von das DataList-Steuerelement s, die gerenderte Ausgabe Vorlagen statt BoundFields CheckBoxFields und So weiter. Im Gegensatz zu FormView DataList-Steuerelement soll einen Satz von Datensätzen anstelle einer einzelne anzuzeigen. Lassen Sie s, die mit diesem Tutorial mit einem Blick auf die Product-Bindungsinformationen in einem DataList-Steuerelement beginnen. Öffnen Sie zunächst die `Basics.aspx` auf der Seite die `DataListRepeaterBasics` Ordner. Ziehen Sie jetzt einem DataList-Steuerelement aus der Toolbox in den Designer. Wie Abbildung 4 zeigt, bevor Sie die Vorlagen DataList s angeben, wird der Designer als ein graues Feld.


[![Ziehen Sie DataList-Steuerelement wird aus der Toolbox in den Designer.](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image7.png)](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image6.png)

**Abbildung 4**: Ziehen Sie das DataList-Steuerelement aus der Toolbox auf die Designer ([klicken Sie, um das Bild in voller Größe anzeigen](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image8.png))


Aus DataList-Steuerelement s Smarttag, fügen Sie eine neue "ObjectDataSource" hinzu und konfigurieren Sie ihn zur Verwendung der `ProductsBLL` Klasse s `GetProducts` Methode. Da wir erneut einem nur-Lese DataList-Steuerelement in diesem Tutorial erstellen Festlegen der Dropdown-Liste (keine), in den Assistenten s einfügen, aktualisieren und Löschen von Registerkarten.


[![Aktivieren Sie zum Erstellen einer neuen "ObjectDataSource"](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image10.png)](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image9.png)

**Abbildung 5**: Erstellen einer neuen "ObjectDataSource" abonnieren ([klicken Sie, um das Bild in voller Größe anzeigen](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image11.png))


[![Konfigurieren von dem ObjectDataSource-Steuerelement zur Verwendung der ProductsBLL-Klasse](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image13.png)](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image12.png)

**Abbildung 6**: Konfigurieren von dem ObjectDataSource-Steuerelement verwenden die `ProductsBLL` Klasse ([klicken Sie, um das Bild in voller Größe anzeigen](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image14.png))


[![Abrufen von Informationen zu allen Produkten mithilfe der GetProducts-Methode](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image16.png)](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image15.png)

**Abbildung 7**: Abrufen von Informationen über alle der Produkte mithilfe der `GetProducts` Methode ([klicken Sie, um das Bild in voller Größe anzeigen](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image17.png))


Nach dem Konfigurieren von dem ObjectDataSource-Steuerelement aus, und es DataList-Steuerelement über die Smarttags zugeordnet, Visual Studio erstellt automatisch eine `ItemTemplate` im DataList-Steuerelement, das den Namen und Wert der einzelnen Datenfelder, die von der Datenquelle zurückgegebenen anzeigt (finden Sie unter der unten stehende Markup). Diese Standardeinstellung `ItemTemplate` Darstellung ist identisch mit den Vorlagen, die automatisch erstellt, wenn eine Datenquelle an das FormView-Steuerelement mithilfe des Designers zu binden.


[!code-aspx[Main](displaying-data-with-the-datalist-and-repeater-controls-cs/samples/sample2.aspx)]

> [!NOTE]
> Denken Sie daran, dass beim Binden einer Datenquelle an einem FormView-Steuerelement über das FormView-s-Smarttag Visual Studio erstellt eine `ItemTemplate`, `InsertItemTemplate`, und `EditItemTemplate`. Mit dem DataList, jedoch nur eine `ItemTemplate` erstellt wird. Dies ist da DataList-Steuerelement nicht die gleiche integrierte bearbeiten und Einfügen von FormView bereitgestellte Unterstützung verfügt. DataList-Steuerelement enthält bearbeiten und löschen-bezogene Ereignisse, und bearbeiten und Löschen von Unterstützung können hinzugefügt werden mit ein wenig Code, aber es s keine einfachen Out-of-the-Box-Unterstützung als mit der FormView-Steuerelement. Gewusst wie: Einschließen von bearbeiten und Löschen von Unterstützung und das DataList-Steuerelement in einem späteren Tutorial sehen.


Lassen Sie s, die die Darstellung dieser Vorlage verbessert werden. Können Sie anstelle der Anzeige aller Datenfelder, s, die nur angezeigt, die s-Produktname, Lieferanten, Kategorie, Anzahl pro Einheit und Preis pro Einheit. Darüber hinaus können s zeigen Sie die Bezeichnung in ein `<h4>` Überschrift, und Erstellen des Layouts für die verbleibenden Felder, die mit einem `<table>` unter der Überschrift.

Um diese Änderungen vorzunehmen, die Sie können, verwenden entweder die Vorlage, die Bearbeitungsfunktionen in den Designer DataList-Steuerelement s smart Tag klicken, auf den Link für die Vorlagen bearbeiten oder ändern Sie die Vorlage manuell über die Seite s deklarative Syntax. Wenn Sie die Vorlagen bearbeiten-Option im Designer verwenden, das daraus resultierende Markup möglicherweise nicht exakt das folgende Markup, aber bei der Anzeige über ein Browser sieht sehr ähnlich wie im Screenshot dargestellt in Abbildung 8.


[!code-aspx[Main](displaying-data-with-the-datalist-and-repeater-controls-cs/samples/sample3.aspx)]

> [!NOTE]
> Im Beispiel oben wird Bezeichnung Web Steuerelemente, deren `Text` Eigenschaft erhält den Wert des die Datenbindungssyntax. Sie können auch konnte wir die Bezeichnungen vollständig zu entfernen und weggelassen haben nur die Databinding-Syntax eingeben. Das heißt, anstelle von `<asp:Label ID="CategoryNameLabel" runat="server" Text='<%# Eval("CategoryName") %>' />` wir stattdessen hätte die deklarative Syntax `<%# Eval("CategoryName") %>`.


In der Bezeichnung Websteuerelementen verlassen, Sie allerdings zwei Vorteile bietet. Zum einen bietet sie eine einfachere Möglichkeit zum Formatieren von Daten basierend auf den Daten, wie wir im nächsten Tutorial sehen werden. Zweitens wird die Option Vorlagen bearbeiten, in der Designer t Display deklarative Datenbindung-Syntax, die außerhalb von einigen Websteuerelement. Stattdessen die Vorlagen bearbeiten-Schnittstelle wurde entwickelt, um die Arbeit mit statischen Markup zu vereinfachen und Web-Steuerelemente sowie wird davon ausgegangen, dass Datenbindung über das Dialogfeld DataBindings bearbeiten, erfolgt, der von die Smarttags für die Web-Steuerelemente aus zugänglich ist.

Aus diesem Grund bei der Verwendung von DataList-Steuerelement, bietet die Möglichkeit, Bearbeitung von Vorlagen mithilfe des Designers, ich Bezeichnung Websteuerelemente verwenden möchten, damit die Inhalte über die Vorlagen bearbeiten-Schnittstelle zugegriffen werden kann. Wie wir in Kürze sehen werden, erfordert der Repeater, aus der Datenquellensicht an den Inhalt der Vorlage s bearbeitet werden. Daher beim Erstellen der Repeater s Vorlagen, die ich häufig die Bezeichnung Web weglassen, wird gesteuert, es sei denn, ich weiß, dass ich benötige, zum Formatieren gebunden die Darstellung der Daten, Text basierend auf programmgesteuerte Logik.


[![Jedes Produkt s Ausgabe ist gerendert DataList-Steuerelement s ItemTemplate-Element](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image19.png)](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image18.png)

**Abbildung 8**: jedes Produkt s Ausgabe ist gerendert DataList-Steuerelement s `ItemTemplate` ([klicken Sie, um das Bild in voller Größe anzeigen](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image20.png))


## <a name="step-3-improving-the-appearance-of-the-datalist"></a>Schritt 3: Verbessern der Darstellung eines DataList-Steuerelement

Wie GridView bietet DataList-Steuerelement eine Reihe von Formatvorlagen bezogenen Eigenschaften, z. B. `Font`, `ForeColor`, `BackColor`, `CssClass`, `ItemStyle`, `AlternatingItemStyle`, `SelectedItemStyle`und so weiter. Beim Arbeiten mit GridView und DetailsView wir die Skin-Dateien in erstellt die `DataWebControls` Design, das vordefinierte der `CssClass` Eigenschaften für diese beiden Steuerelemente und die `CssClass` Eigenschaft für eine Reihe von deren untergeordneten Eigenschaften (`RowStyle` `HeaderStyle`und so weiter). Lassen Sie s für DataList-Steuerelement verwenden müssen.

Siehe die [Anzeigen von Daten mit dem ObjectDataSource-Steuerelement](../basic-reporting/displaying-data-with-the-objectdatasource-cs.md) Tutorial, eine Skin-Datei gibt an, die Standardeigenschaften darstellungsbezogene für ein Steuerelement, ein Design ist eine Sammlung von Skin, CSS, Image und JavaScript-Dateien, definieren ein bestimmtes Aussehen und Verhalten für eine Website. In der *Anzeigen von Daten mit dem ObjectDataSource-Steuerelement* Tutorial erstellt eine `DataWebControls` Design (die wird implementiert, als einen Ordner innerhalb der `App_Themes` Ordner), die sind derzeit zwei Skin-Dateien - `GridView.skin` und `DetailsView.skin`. Lassen Sie s eine dritte an die vordefinierte frameart-Einstellungen für DataList-Steuerelement-Skindatei hinzufügen.

Um eine Skin-Datei hinzuzufügen, mit der Maustaste auf die `App_Themes/DataWebControls` , wählen Sie ein neues Element hinzufügen, und wählen Sie aus der Liste die Skin-File-Option. Nennen Sie die Datei `DataList.skin`.


[![Erstellen Sie eine neue Skindatei mit dem Namen DataList.skin](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image22.png)](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image21.png)

**Abbildung 9**: Erstellen einer neuen Skin-Datei mit dem Namen `DataList.skin` ([klicken Sie, um das Bild in voller Größe anzeigen](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image23.png))


Verwenden Sie das folgende Markup für die `DataList.skin` Datei:


[!code-aspx[Main](displaying-data-with-the-datalist-and-repeater-controls-cs/samples/sample4.aspx)]

Diese Einstellungen weisen die gleichen CSS-Klassen die entsprechenden DataList-Eigenschaften mit GridView und DetailsView verwendet wurden. Die CSS-Klassen, die hier verwendete `DataWebControlStyle`, `AlternatingRowStyle`, `RowStyle`usw. werden definiert, der `Styles.css` Datei, und in den vorherigen Tutorials hinzugefügt wurden.

Durch das Hinzufügen dieser Datei Skin wird die Darstellung der DataList-Steuerelement im Designer aktualisiert (möglicherweise müssen zum Aktualisieren der Ansicht-Designer, um die Auswirkungen der neuen Skin-Datei, aus dem Menü "Ansicht", wählen die Aktualisierung). Wie in Abbildung 10 gezeigt, hat jedes Produkt abwechselnde ein helles rosa Hintergrundfarbe an.


[![Erstellen Sie eine neue Skindatei mit dem Namen DataList.skin](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image25.png)](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image24.png)

**Abbildung 10**: Erstellen einer neuen Skin-Datei mit dem Namen `DataList.skin` ([klicken Sie, um das Bild in voller Größe anzeigen](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image26.png))


## <a name="step-4-exploring-the-datalist-s-other-templates"></a>Schritt 4: Untersuchen der DataList s anderen Vorlagen

Zusätzlich zu den `ItemTemplate`, DataList-Steuerelement unterstützt sechs weitere optionalen Vorlagen:

- `HeaderTemplate` Wenn angegeben, die Ausgabe eine Kopfzeile hinzugefügt und wird verwendet, um diese Zeile zu rendern.
- `AlternatingItemTemplate` abwechselnde Elemente gerendert.
- `SelectedItemTemplate` verwendet, um das ausgewählte Element zu rendern. das ausgewählte Element ist das Element, dessen Index, an die Datenliste s entspricht [ `SelectedIndex` Eigenschaft](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalist.selectedindex.aspx)
- `EditItemTemplate` verwendet zum Rendern des Elements, das bearbeitet wird
- `SeparatorTemplate` Wenn angegeben, fügt ein Trennzeichen zwischen den einzelnen Elementen und wird verwendet, um dieses Trennzeichen zu rendern.
- `FooterTemplate` -Wenn angegeben, die Ausgabe eine Fußzeile hinzugefügt und wird verwendet, um diese Zeile zu rendern.

Beim Angeben der `HeaderTemplate` oder `FooterTemplate`, DataList-Steuerelement fügt eine zusätzliche Zeile der Kopf- oder Fußzeile auf der gerenderten Ausgabe. Wie werden mit GridView-s-Header und Footer Zeilen, die Header und Fußzeile in einem DataList-Steuerelement nicht an Daten gebunden. Aus diesem Grund jede Databinding-Syntax in der `HeaderTemplate` oder `FooterTemplate` , dass Zugriffsversuche auf gebundenen Daten eine leere Zeichenfolge zurückgegeben werden.

> [!NOTE]
> Wie in beschrieben der [Zusammenfassungsinformationen anzeigen, in den GridView-s-Fußzeile](../custom-formatting/displaying-summary-information-in-the-gridview-s-footer-cs.md) Tutorial, während die Kopf- und Fußzeile Zeilen mit t-Syntax Datenbindung unterstützt, Daten-spezifische Informationen Einbau zusätzlichen injiziert werden kann, direkt in die folgenden Zeilen aus der GridView-s `RowDataBound` -Ereignishandler. Diese Technik nicht verwendet werden kann sowohl die laufende Summen berechnen oder andere Informationen aus den Daten an das Steuerelement gebunden als auch diese Informationen in die Fußzeile zuweisen. Dasselbe Konzept kann auf die DataList- oder Repeater-Steuerelemente angewendet werden. der einzige Unterschied ist, die dem DataList- und Wiederholungssteuerelement erstellt einen Ereignishandler für die `ItemDataBound` Ereignis (anstelle von für die `RowDataBound` Ereignis).


In unserem Beispiel können s haben den Titel Produktinformationen angezeigt werden, am oberen Rand der DataList-s-Ergebnisse in eine `<h3>` Überschrift. Um dies zu erreichen, fügen einen `HeaderTemplate` durch das entsprechende Markup. Aus dem Designer, dies kann erreicht werden durch Klicken auf den Link Vorlagen bearbeiten, in das Smarttag DataList s, die Header-Vorlage aus der Dropdown-Liste auswählen und der Eingabe im Text nach dem Auswählen der Überschrift3-Option vom Stil Dropdown-Liste (siehe Abbildung 11).


[![Fügen Sie eine Header-Vorlage mit die Text-Produktinformationen](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image28.png)](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image27.png)

**Abbildung 11**: Hinzufügen einer `HeaderTemplate` mit die Text-Produktinformationen ([klicken Sie, um das Bild in voller Größe anzeigen](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image29.png))


Auch dies kann hinzugefügt werden deklarativ durch Eingabe von das folgende Markup innerhalb der `<asp:DataList>` Tags:


[!code-html[Main](displaying-data-with-the-datalist-and-repeater-controls-cs/samples/sample5.html)]

Wenn etwas Abstand zwischen den einzelnen Produktliste hinzufügen möchten, können Sie s Hinzufügen einer `SeparatorTemplate` , eine Linie zwischen jeder Abschnitt enthält. Das horizontale Trennlinie-Tag (`<hr>`), fügt diese einen Unterteiler. Erstellen der `SeparatorTemplate` so, dass sie das folgende Markup:


[!code-html[Main](displaying-data-with-the-datalist-and-repeater-controls-cs/samples/sample6.html)]

> [!NOTE]
> Wie die `HeaderTemplate` und `FooterTemplates`, `SeparatorTemplate` nicht zu einem beliebigen Datensatz aus der Datenquelle gebunden ist, und aus diesem Grund kann nicht direkt zugreifen, die die Datenquelle Datensätze, die an die Datenliste gebunden.


Treffen Sie diese hinzufügen, wenn Sie die Seite über einen Browser anzeigen. es sollte ungefähr Abbildung 12. Beachten Sie die Kopfzeile und die Linie zwischen den einzelnen Produktliste.


[![DataList-Steuerelement enthält eine Kopfzeile und eine horizontale Trennlinie zwischen einzelnen Produktliste](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image31.png)](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image30.png)

**Abbildung 12**: DataList-Steuerelement enthält eine Kopfzeile und eine horizontale Regel zwischen jeder Produktliste ([klicken Sie, um das Bild in voller Größe anzeigen](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image32.png))


## <a name="step-5-rendering-specific-markup-with-the-repeater-control"></a>Schritt 5: Rendern von bestimmten Markup mit dem Repeater-Steuerelement

Eine Ansicht/Source aus Ihrem Browser Fall beim Zugriff auf das DataList-Beispiel aus Abbildung 12 sehen Sie, dass DataList-Steuerelement eine HTML ausgibt `<table>` , enthält eine Zeile einer Tabelle (`<tr>`) mit einer einzelnen Tabellenzelle (`<td>`) für jedes Element gebunden der DataList-Steuerelement. Diese Ausgabe entspricht in der Tat wie von einer GridView-Ansicht mit einem einzelnen TemplateField ausgegeben werden sollen. In einem späteren Tutorial sehen, lässt DataList-Steuerelement Weitere Anpassung der Ausgabe, ermöglicht uns, mehrere Datenquellen-Datensätze pro Zeile der Tabelle angezeigt.

Was geschieht, wenn t eine HTML-ausgeben möchten Einbau zusätzlichen `<table>`, obwohl? Insgesamt und vollständige Kontrolle über das Markup, das von einem Websteuerelement Daten generiert müssen wir das Repeater-Steuerelement verwenden. Wie DataList wird Repeater erstellt basierend auf Vorlagen. Der Repeater, bietet jedoch nur die folgenden fünf Vorlagen:

- `HeaderTemplate` Wenn angegeben, fügt dem angegebenen Markup vor den Elementen hinzu.
- `ItemTemplate` verwendet, um Elemente zu rendern.
- `AlternatingItemTemplate` Wenn angegeben, verwendet, um abwechselnde Elemente zu rendern.
- `SeparatorTemplate` Wenn angegeben, fügt dem angegebenen Markup zwischen den einzelnen Elementen hinzu.
- `FooterTemplate` -Wenn angegeben, fügt Sie das angegebene Markup nach den Elementen

In ASP.NET 1.x, die Repeater-Steuerelement häufig verwendet wurde, um eine Liste mit Aufzählungszeichen anzuzeigen, deren Daten aus einer Datenquelle stammen. In diesem Fall die `HeaderTemplate` und `FooterTemplates` enthält den öffnenden und schließenden `<ul>` , while-Tags, die `ItemTemplate` enthält `<li>` Elemente mit Databinding-Syntax. Dieser Ansatz kann immer noch in ASP.NET 2.0 verwendet werden, wie wir in beiden Beispielen gesehen haben die [Masterseiten und Sitenavigation](../introduction/master-pages-and-site-navigation-cs.md) Lernprogramm:

- In der `Site.master` Masterseite ein Repeater wurde verwendet, um eine Aufzählung der Standort auf oberster Ebene Zuordnung Inhalte (grundlegende Berichterstellung, Filterungsberichte, angepasste Formatierung, usw.) angezeigt; eine andere, geschachtelte Repeater wurde verwendet, um die untergeordneten Elemente Abschnitte zeigen die Abschnitten auf oberster Ebene
- In `SectionLevelTutorialListing.ascx`, einem Wiederholungssteuerelement wurde verwendet, um eine Aufzählung der untergeordneten Elemente Abschnitte der dem aktuellen Standort Kartenbereich angezeigt

> [!NOTE]
> ASP.NET 2.0 werden die neuen [BulletedList-Steuerelement](https://msdn.microsoft.com/library/ms228101.aspx), die an ein Datenquellen-Steuerelement um eine einfache Aufzählung angezeigt gebunden werden kann. Mit dem Steuerelement BulletedList müssen wir nicht zum Angeben des HTML-bezogene Liste; Stattdessen zeigen wir einfach das Datenfeld als Text für jedes Listenelement anzuzeigen.


Des Repeaters fungiert als ein Catch alle Daten Websteuerelement. Wird es kein vorhandenes Steuerelement, das das erforderliche Markup generiert, kann das Repeater-Steuerelement verwendet werden. Zur Veranschaulichung des Repeaters verwenden, können Sie s die Liste der Kategorien, die weiter oben in Schritt2 erstellte Produkt Informationen DataList angezeigt haben. Insbesondere können s haben die Kategorien angezeigt, die in einer einzelnen Zeile HTML `<table>` mit jeder Kategorie, die als eine Spalte in der Tabelle angezeigt.

Um dies zu erreichen, ziehen Sie zuerst ein Repeater-Steuerelement aus der Toolbox auf den Designer, oben Produkt Informationen DataList-Steuerelement. Wie bei DataList, Repeater werden zuerst angezeigt wie ein graues Feld bis dessen Vorlagen definiert wurden.


[![Fügen Sie in den Designer einen Repeater](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image34.png)](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image33.png)

**Abbildung 13**: Fügen Sie in den Designer einem Wiederholungssteuerelement ([klicken Sie, um das Bild in voller Größe anzeigen](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image35.png))


Dort s nur eine Option im Smarttag Repeater s: Datenquelle auswählen. Aktivieren Sie zum Erstellen einer neuen "ObjectDataSource" und konfigurieren ihn zur Verwendung der `CategoriesBLL` Klasse s `GetCategories` Methode.


[![Erstellen Sie eine neue "ObjectDataSource"](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image37.png)](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image36.png)

**Abbildung 14**: Erstellen einer neuen "ObjectDataSource" ([klicken Sie, um das Bild in voller Größe anzeigen](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image38.png))


[![Konfigurieren von dem ObjectDataSource-Steuerelement zur Verwendung der CategoriesBLL-Klasse](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image40.png)](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image39.png)

**Abbildung 15**: Konfigurieren von dem ObjectDataSource-Steuerelement verwenden die `CategoriesBLL` Klasse ([klicken Sie, um das Bild in voller Größe anzeigen](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image41.png))


[![Abrufen von Informationen über alle Kategorien mithilfe der GetCategories-Methode](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image43.png)](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image42.png)

**Abbildung 16**: Abrufen von Informationen über alle der Kategorien mithilfe der `GetCategories` Methode ([klicken Sie, um das Bild in voller Größe anzeigen](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image44.png))


Im Gegensatz zu DataList-Steuerelement erstellt Visual Studio nicht automatisch eine Elementvorlage für das Repeater nach dem Element an eine Datenquelle gebunden wird. Darüber hinaus wird die Repeater-s-Vorlagen können nicht mithilfe des Designers konfiguriert werden und müssen deklarativ angegeben werden.

Um die Kategorien anzuzeigen, als einzelne Zeile `<table>` mit einer Spalte für jede Kategorie, die wir benötigen die Repeater, ähnlich dem folgenden Markup auszugeben:


[!code-html[Main](displaying-data-with-the-datalist-and-repeater-controls-cs/samples/sample7.html)]

Da die `<td>Category X</td>` Text ist der Teil, der auftritt, wird dies im Wiederholungsmodul s ItemTemplate angezeigt,. Das Markup, das vor – wird `<table><tr>` -platziert werden, der `HeaderTemplate` beim Endpunkt Markup - `</tr></table>` -in platziert wird die `FooterTemplate`. Um diese Vorlageneinstellungen einzugeben, wechseln Sie zu den deklarativen Teil der ASP.NET-Seite aus, durch Klicken auf die Schaltfläche "Quelle" in der unteren linken Ecke, und geben Sie die folgende Syntax:


[!code-aspx[Main](displaying-data-with-the-datalist-and-repeater-controls-cs/samples/sample8.aspx)]

Der Repeater gibt gemäß der Vorlagen, nicht mehr, nichts weniger präzise Markup. Abbildung 17 zeigt die Repeater-s-Ausgabe über einen Browser angezeigt.


[![Eine einzelne Zeile HTML &lt;Tabelle&gt; listet jede Kategorie in einer separaten Spalte](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image46.png)](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image45.png)

**Abbildung 17**: eine einzelne Zeile HTML `<table>` listet jede Kategorie in einer separaten Spalte ([klicken Sie, um das Bild in voller Größe anzeigen](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image47.png))


## <a name="step-6-improving-the-appearance-of-the-repeater"></a>Schritt 6: Verbessern der Darstellung des Wiederholungsmoduls

Da der Repeater genau das Markup, anhand dessen Vorlagen ausgibt, sollte es als nicht verwunderlich, dass steht für, dass keine Formatvorlagen bezogenen Eigenschaften für das Repeater vorhanden sind. Um die Darstellung des Wiederholungsmoduls generierten Inhalte zu ändern, müssen wir den erforderlichen HTML- oder CSS-Inhalt manuell direkt an den Repeater-s-Vorlagen hinzufügen.

In unserem Beispiel können Sie s, die die Kategorie Spalten Hintergrundfarben an, wie mit den abwechselnden Zeilen im DataList-Steuerelement zu wechseln. Zu diesem Zweck müssen wir weisen der `RowStyle` CSS-Klasse, jedes Repeater-Element und die `AlternatingRowStyle` CSS-Klasse, jedes abwechselnde Repeater-Element über die `ItemTemplate` und `AlternatingItemTemplate` Vorlagen wie folgt:


[!code-aspx[Main](displaying-data-with-the-datalist-and-repeater-controls-cs/samples/sample9.aspx)]

Lassen Sie s, die die Ausgabe mit dem Text Product Categories auch eine Kopfzeile hinzugefügt. Da wir Raten t wissen, wie viele Spalten unsere resultierende `<table>` wird umfassen, ist die einfachste Möglichkeit, eine Kopfzeile zu generieren, die garantiert ist, um alle Spalten mit *zwei* `<table>` s. Die erste `<table>` enthält zwei Zeilen der Kopfzeile und eine Zeile, die die zweite einzeiliges enthält `<table>` , die eine Spalte für jede Kategorie in das System hat. D. h. möchten wir das folgende Markup auszugeben:


[!code-html[Main](displaying-data-with-the-datalist-and-repeater-controls-cs/samples/sample10.html)]

Die folgenden `HeaderTemplate` und `FooterTemplate` führen zu der gewünschten Markup:


[!code-aspx[Main](displaying-data-with-the-datalist-and-repeater-controls-cs/samples/sample11.aspx)]

Abbildung 18 zeigt die Repeater, nachdem diese Änderungen vorgenommen wurden.


[![Die Kategorie-Spalten in Hintergrundfarbe alternative und enthält eine Kopfzeile](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image49.png)](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image48.png)

**Abbildung 18**: die Kategorie Spalten alternative Hintergrundfarbe und enthält eine Kopfzeile ([klicken Sie, um das Bild in voller Größe anzeigen](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image50.png))


## <a name="summary"></a>Zusammenfassung

Während das GridView-Steuerelement einfach angezeigt werden können, ist die bearbeiten, löschen, Sortieren und Paging durch die Daten, auf die Darstellung, sehr erhalten Sie kastenförmige und Raster-ähnliches. Zur besseren Steuerung der Darstellung müssen wir für den DataList oder Repeater-Steuerelemente zu aktivieren. Diese beiden Steuerelemente zeigen eine Reihe von Datensätzen, die mithilfe von Vorlagen anstelle von BoundFields, CheckBoxFields und So weiter.

DataList-Steuerelement als HTML rendert `<table>` , die in der Standardeinstellung zeigt jeden Datensatz der Datenquelle in einer einzelnen Tabellenzeile, wie einer GridView-Ansicht mit einem einzelnen TemplateField. Wie wir in einem späteren Tutorial sehen, lässt jedoch DataList-Steuerelement mehrere Datensätze, die pro Zeile der Tabelle angezeigt werden wird. Der Repeater ausgibt ausschließlich auf der anderen Seite das Markup in der Vorlagen angegeben; Es werden keine zusätzliches Markup hinzugefügt und aus diesem Grund wird häufig verwendet, um Daten in HTML-Elementen nicht anzeigen einer `<table>` (z. B. in einer Liste mit Aufzählungszeichen).

Trotz des DataList- und Wiederholungssteuerelements mehr Flexibilität bei der die gerenderte Ausgabe fehlen für viele der integrierten Funktionen finden Sie in den GridView-Ansicht. Wie wir in zukünftigen Lernprogrammen untersuchen müssen, einige dieser Features kann dynamisch geladen werden, wieder ohne großen Aufwand, aber führen Sie denken Sie daran, die verwenden weiterhin DataList-Steuerelement oder Repeater anstelle der GridView schränkt die Features, die Sie verwenden können, ohne dass diese Features zu implementieren selbst.

Viel Spaß beim Programmieren!

## <a name="about-the-author"></a>Der Autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor von sieben Büchern zu ASP/ASP.NET und Gründer von [4GuysFromRolla.com](http://www.4guysfromrolla.com), arbeitet mit Microsoft-Web-Technologien seit 1998. Er ist als ein unabhängiger Berater, Schulungsleiter und Autor. Sein neueste Buch wird [*Sams Schulen selbst ASP.NET 2.0 in 24 Stunden*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Er ist unter [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) oder über seinen Blog finden Sie unter [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Besonderen Dank an

Diese tutorialreihe wurde durch viele hilfreiche Reviewer überprüft. Führendes Prüfer für dieses Tutorial wurden Yaakov Ellis, Liz Shulok, Randy Schmidt und Stacy Park. Meine zukünftigen MSDN-Artikeln überprüfen möchten? Wenn dies der Fall ist, löschen Sie mir eine Linie an [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Nächste](formatting-the-datalist-and-repeater-based-upon-data-cs.md)
