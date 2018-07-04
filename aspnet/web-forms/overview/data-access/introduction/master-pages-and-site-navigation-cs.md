---
uid: web-forms/overview/data-access/introduction/master-pages-and-site-navigation-cs
title: Masterseiten und Sitenavigation (c#) | Microsoft-Dokumentation
author: rick-anderson
description: Eine gemeinsame Merkmale von benutzerfreundlichen Websites ist, dass sie eine konsistente, standortweite Layout und das Navigationsschema. Dieses Tutorial befasst sich y...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/31/2010
ms.topic: article
ms.assetid: 5aee8202-a4e3-4aa9-8a95-cd5d156cea4c
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/data-access/introduction/master-pages-and-site-navigation-cs
msc.type: authoredcontent
ms.openlocfilehash: 7f152dcd32b1a7e3022d939c27860a81396a34b0
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/03/2018
ms.locfileid: "37370360"
---
<a name="master-pages-and-site-navigation-c"></a>Masterseiten und Sitenavigation (c#)
====================
durch [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Beispiel-App herunter](http://download.microsoft.com/download/4/6/3/463cf87c-4724-4cbc-b7b5-3f866f43ba50/ASPNET_Data_Tutorial_3_CS.exe) oder [PDF-Datei herunterladen](master-pages-and-site-navigation-cs/_static/datatutorial03cs1.pdf)

> Eine gemeinsame Merkmale von benutzerfreundlichen Websites ist, dass sie eine konsistente, standortweite Layout und das Navigationsschema. In diesem Tutorial untersucht davon ab, wie Sie einen konsistenten Aussehens und Verhaltens für alle Seiten erstellen, die problemlos aktualisiert werden können.


## <a name="introduction"></a>Einführung

Eine gemeinsame Merkmale von benutzerfreundlichen Websites ist, dass sie eine konsistente, standortweite Layout und das Navigationsschema. ASP.NET 2.0 werden zwei neue Features, die erheblich vereinfachen, implementieren beide eine standortweite Layout und das Navigationsschema: Masterseiten und Sitenavigation. Masterseiten ermöglichen Entwicklern das Erstellen einer Vorlage für die gesamte Website mit der angegebenen bearbeitbare Bereiche. Mit dieser Vorlage können Sie dann auf ASP.NET-Seiten in der Website angewendet werden. Solche ASP.NET-Seiten müssen nur die Inhalte bereitstellen, für die Masterseite bearbeitbare Bereiche angegeben, die alle anderen Markup auf der Masterseite ist identisch für alle ASP.NET-Seiten, die die Masterseite verwenden. Dieses Modell ermöglicht Entwicklern das Definieren und zu zentralisieren ein standortweite Seitenlayout, wodurch erleichtert Ihnen die Erstellung ein konsistentes Aussehens und Verhaltens für alle Seiten, die problemlos aktualisiert werden können.

Die [Navigation Standortsystem](http://aspnet.4guysfromrolla.com/articles/111605-1.aspx) bietet sowohl einen Mechanismus für Seitenentwickler, die zum Definieren einer Sitemap und eine API für die sitezuordnung für die programmgesteuert abgefragt werden. Die neue Navigation-Websteuerelemente, die, denen das Menü, TreeView und SiteMapPath erleichtern, Rendern ganz oder teilweise die Sitemap in einer allgemeinen Navigation-Benutzeroberflächenelement. Wir verwenden die Website Standardnavigationsanbieters, was bedeutet, dass unsere Sitemap in einer XML-Datei definiert werden.

Um diese Konzepte zu veranschaulichen, und unsere Tutorials Website besser verwendbar zu machen, werfen wir kurz in dieser Lektion definieren ein Layout für die gesamte Website, implementieren eine Sitemap und Navigationsbenutzeroberfläche hinzufügen. Nach Abschluss dieses Lernprogramms müssen einen ansprechenden Website-Entwurf für unsere Tutorials Webseiten erstellen.


[![Das Endergebnis dieses Tutorials](master-pages-and-site-navigation-cs/_static/image2.png)](master-pages-and-site-navigation-cs/_static/image1.png)

**Abbildung 1**: The End Ergebnis von diesem Lernprogramm ([klicken Sie, um das Bild in voller Größe anzeigen](master-pages-and-site-navigation-cs/_static/image3.png))


## <a name="step-1-creating-the-master-page"></a>Schritt 1: Erstellen der Masterseite

Der erste Schritt ist die Masterseite für die Website zu erstellen. Derzeit nur das typisierte DataSet unserer Website besteht (`Northwind.xsd`in die `App_Code` Ordner), die BLL-Klassen (`ProductsBLL.cs`, `CategoriesBLL.cs`, und so weiter, alle in der `App_Code` Ordner), der Datenbank (`NORTHWND.MDF`in die `App_Data` Ordner ""), die Konfigurationsdatei (`Web.config`), und eine CSS-Stylesheet-Datei (`Styles.css`). Ich bereinigt diese Seiten und Dateien, die veranschaulichen, mit der DAL und der BLL aus den ersten beiden Tutorials, da wir diesen Beispielen ausführlich in zukünftigen Lernprogrammen überprüfen.


![Die Dateien in Ihrem Projekt](master-pages-and-site-navigation-cs/_static/image4.png)

**Abbildung 2**: die Dateien in Ihrem Projekt


Klicken Sie zum Erstellen einer Masterseite mit der rechten Maustaste auf den Projektnamen im Projektmappen-Explorer, und wählen Sie Neues Element hinzufügen. Klicken Sie dann wählen Sie die Masterseite aus der Liste der Vorlagen, und nennen Sie sie `Site.master`.


[![Fügen Sie eine neue Master-Seite auf der Website](master-pages-and-site-navigation-cs/_static/image6.png)](master-pages-and-site-navigation-cs/_static/image5.png)

**Abbildung 3**: Fügen Sie eine neue Master-Seite auf der Website ([klicken Sie, um das Bild in voller Größe anzeigen](master-pages-and-site-navigation-cs/_static/image7.png))


Definieren Sie die standortweite Seitenlayout hier auf der Masterseite. Sie verwenden Sie die Entwurfsansicht zu sehen und beliebige Layout oder Web-Steuerelemente, die benötigten hinzufügen können, oder Sie können das Markup manuell hinzufügen, die manuell in der Datenquellensicht. In Meine Masterseite verwende ich [cascading Stylesheets](http://www.w3schools.com/css/default.asp) zum Positionieren und Stile, die mit den CSS-Einstellungen, die in der externen Datei definiert `Style.css`. Während Sie aus dem unten gezeigten Markup nicht erkennen können, werden die CSS-Regeln definiert, dass die Navigation `<div>`Inhalt ist absolut positioniert, damit sie auf der linken Seite angezeigt wird und eine feste von 200 Pixel Breite.

Site.Master


[!code-aspx[Main](master-pages-and-site-navigation-cs/samples/sample1.aspx)]

Eine Masterseite definiert sowohl statische Seitenlayouts und Bereiche, die durch die ASP.NET-Seiten bearbeitet werden können, die die Masterseite verwenden. Dieser Inhalt bearbeitbare Bereiche werden durch die ContentPlaceHolder-Steuerelement, das im Inhalt angezeigt werden angegeben `<div>`. Unsere Masterseite verfügt über ein einzelnes ContentPlaceHolder-Objekt (`MainContent`), aber der Masterseite kann mehrere ContentPlaceHolder-Steuerelemente enthalten.

Mit dem Markup oben eingegebenen zeigt das Umschalten zur Entwurfsansicht der Masterseite Layout aus. Alle ASP.NET-Seiten, die diese Masterseite verwenden, müssen diese einheitliche Layout, mit der Möglichkeit, geben Sie das Markup für die `MainContent` Region.


[![Die Masterseite, wenn Sie über die Entwurfsansicht angezeigt](master-pages-and-site-navigation-cs/_static/image9.png)](master-pages-and-site-navigation-cs/_static/image8.png)

**Abbildung 4**: der Masterseite, wenn angezeigt durch das Design View ([klicken Sie, um das Bild in voller Größe anzeigen](master-pages-and-site-navigation-cs/_static/image10.png))


## <a name="step-2-adding-a-homepage-to-the-website"></a>Schritt 2: Hinzufügen einer Homepage der Website

Mit der Masterseite, die definiert sind wir bereit sind, die ASP.NET-Seiten für die Website hinzuzufügen. Wir fügen zunächst `Default.aspx`, unsere Website-Homepage. Mit der rechten Maustaste auf den Projektnamen im Projektmappen-Explorer, und wählen Sie Neues Element hinzufügen. Wählen Sie die Option "Webformular" aus der Vorlagenliste und den Namen die Datei `Default.aspx`. Überprüfen Sie außerdem das Kontrollkästchen "Masterseite auswählen".


[![Fügen Sie ein neues Webformular, überprüfen die Masterseite auswählen das Kontrollkästchen hinzu.](master-pages-and-site-navigation-cs/_static/image12.png)](master-pages-and-site-navigation-cs/_static/image11.png)

**Abbildung 5**: Fügen Sie ein neues Webformular, überprüfen die Masterseite auswählen das Kontrollkästchen ([klicken Sie, um das Bild in voller Größe anzeigen](master-pages-and-site-navigation-cs/_static/image13.png))


Nach dem Klicken auf die Schaltfläche "OK", werden wir aufgefordert, auf welche Masterseite, diese neue Seite mit ASP.NET verwenden soll. Während Sie mehrere Masterseiten in Ihrem Projekt verwenden können, müssen wir nur ein.


[![Wählen Sie die Master-Seite, die diese ASP.NET-Seite verwendet werden soll](master-pages-and-site-navigation-cs/_static/image15.png)](master-pages-and-site-navigation-cs/_static/image14.png)

**Abbildung 6**: Wählen Sie auf der Masterseite diese ASP.NET Seite sollte Verwendung ([klicken Sie, um das Bild in voller Größe anzeigen](master-pages-and-site-navigation-cs/_static/image16.png))


Wählen Sie die Masterseite, enthält die ASP.NET-Seiten das folgende Markup:

Default.aspx


[!code-aspx[Main](master-pages-and-site-navigation-cs/samples/sample2.aspx)]

In der `@Page` Direktive vorhanden ist, einen Verweis auf die Masterseite-Datei verwendet (`MasterPageFile="~/Site.master"`), und der ASP.NET-Seite Markup enthält ein ContentControl-Element für jede der ContentPlaceHolder-Steuerelemente, die in die Masterseite mit des Steuerelements definiert `ContentPlaceHolderID` Zuordnung des Inhalts die Steuerung an eine bestimmte ContentPlaceHolder. Das Steuerelement ist, platzieren Sie das Markup in der entsprechenden ContentPlaceHolder angezeigt werden sollen. Legen Sie die `@Page` -Direktive `Title` -Attribut auf Start, und einige und Inhalt in das Inhaltssteuerelement hinzufügen:

Default.aspx


[!code-aspx[Main](master-pages-and-site-navigation-cs/samples/sample3.aspx)]

Die `Title` -Attribut in der `@Page` -Direktive ermöglicht uns, legen Sie den Titel der Seite von der ASP.NET-Seite, obwohl die `<title>` Element auf der Masterseite definiert ist. Wir können auch den Titel programmgesteuert festlegen, mit `Page.Title`. Beachten Sie, dass die Masterseite-Verweise auf Stylesheets (z. B. `Style.css`) automatisch aktualisiert, damit Sie funktionieren in einer ASP.NET-Seite, unabhängig davon, welchem Verzeichnis die ASP.NET-Seite Bezug auf die Masterseite ist.

Wechseln zur Entwurfsansicht, dass wir sehen können, wie die Seite in einem Browser aussehen wird. Beachten Sie, dass der Entwurf anzeigen für die ASP.NET-Seite, dass nur die bearbeitbaren Inhaltsbereiche bearbeitet werden, das nicht ContentPlaceHolder-Markup, das in der Masterseite definierte ist abgeblendet.


[![Die Entwurfsansicht für die ASP.NET-Seite zeigt der bearbeitbare und nicht bearbeitbare Bereiche](master-pages-and-site-navigation-cs/_static/image18.png)](master-pages-and-site-navigation-cs/_static/image17.png)

**Abbildung 7**: die Entwurfsansicht für die ASP.NET Seite zeigt sowohl den Bearbeitbarer und nicht-Editable-Regionen ([klicken Sie, um das Bild in voller Größe anzeigen](master-pages-and-site-navigation-cs/_static/image19.png))


Wenn die `Default.aspx` Seite wird besucht, von einem Browser, die ASP.NET-Engine automatisch zusammengeführt, den Inhalt der Seite Masterseite und ASP. NET den Inhalt, und stellt den zusammengeführten Inhalt in der endgültigen HTML-Code, der an den anfordernden Browser, nach unten gesendet wird. Wenn die Masterseite Inhalt aktualisiert wird, müssen alle ASP.NET-Seiten, die diese Masterseite verwenden ihre Inhalte wieder das nächste Mal, die, das Sie angefordert werden, mit der neue Inhalt Masterseite zusammengeführt. Kurz gesagt, die Masterseite Modell können für eine einzelne Seite in der Layoutvorlage sein definiert (Masterseite), deren Änderungen werden sofort übernommen, für die gesamte Website.

## <a name="adding-additional-aspnet-pages-to-the-website"></a>Hinzufügen von zusätzlichen ASP.NET-Seiten auf der Website

Werfen Sie einen Moment Zeit, die zusätzliche ASP.NET Seite Stubs des Standorts hinzufügen, die letztlich die verschiedenen reporting Demos enthalten soll. Es werden mehr als 35 Demos insgesamt also statt Erstellen aller Seiten Stub wir nur ein paar zu erstellen. Da darüber hinaus die Anzahl der Kategorien von Demos ist, Hinzufügen der Demos für eine bessere Verwaltung einen Ordner für die Kategorien. Fügen Sie jetzt die folgenden drei Ordner hinzu:

- `BasicReporting`
- `Filtering`
- `CustomFormatting`

Abschließend fügen Sie neue Dateien hinzu, wie im Projektmappen-Explorer in Abbildung 8 gezeigt. Denken Sie daran, jede Datei hinzufügen, überprüfen Sie das Kontrollkästchen "Masterseite auswählen".


![Fügen Sie die folgenden Dateien hinzu.](master-pages-and-site-navigation-cs/_static/image20.png)

**Abbildung 8**: Fügen Sie die folgenden Dateien hinzu.


## <a name="step-2-creating-a-site-map"></a>Schritt 2: Erstellen einer Siteübersicht

Eine der Herausforderungen der Verwaltung von einer Website besteht aus mehr als eine Reihe von Seiten ist eine einfache Möglichkeit für Besucher zum Navigieren durch die Website bereitstellen. Zunächst muss die Navigationsstruktur der Website definiert werden. Als Nächstes muss diese Struktur in einer navigierbaren Elemente der Benutzeroberfläche, z. B. Menüs oder der Breadcrumb-Leiste übersetzt werden. Abschließend muss dieser gesamte Prozess verwaltet und aktualisiert werden, wie neue Seiten, auf die Website und vorhandene entfernt hinzugefügt werden werden. Vor ASP.NET 2.0 waren Entwickler zum Erstellen eigener für Navigationsstruktur für die Website, verwalten sie und Übersetzung in die navigierbaren Elemente der Benutzeroberfläche. Mit ASP.NET 2.0 können jedoch Entwickler sehr flexibel nutzen Sitenavigationssystems integriert.

Des Sitenavigationssystems in ASP.NET 2.0 bietet eine Möglichkeit für einen Entwickler die Sitemap definiert, und klicken Sie dann Zugriff auf diese Informationen über eine programmgesteuerte API. Im Lieferumfang von ASP.NET ist eines Siteübersichtsanbieters, der erwartet, dass Sitemap-Daten in eine XML-Datei, die auf eine bestimmte Weise formatiert gespeichert werden. Aber da des Sitenavigationssystems basiert auf der [Anbietermodell](http://aspnet.4guysfromrolla.com/articles/101905-1.aspx) kann erweitert werden, um alternative Methoden zum Serialisieren der Siteübersichtsinformationen zu unterstützen. Prosises Artikel [der SQL-Website zuordnen Anbieter Sie habe wurde warten für](https://msdn.microsoft.com/msdnmag/issues/06/02/WickedCode/default.aspx) zeigt, wie einen Siteübersichtsanbieter erstellen, die Sitemap in einer SQL Server-Datenbank speichert, die eine andere Möglichkeit ist die Erstellung [ein Siteübersichtsanbieter basierend auf die Struktur des Dateisystems](http://aspnet.4guysfromrolla.com/articles/020106-1.aspx).

In diesem Tutorial jedoch verwenden wir die standardmäßigen SiteMapProvider, die bereitgestellt wird mit ASP.NET 2.0. Um die Sitemap zu erstellen, einfach mit der rechten Maustaste auf den Projektnamen im Projektmappen-Explorer, neues Element hinzufügen, und wählen Sie die Option für die Siteübersicht. Lassen Sie den Namen als `Web.sitemap` , und klicken Sie auf die Schaltfläche "hinzufügen".


[![Hinzufügen einer Sitezuordnung zu Ihrem Projekt](master-pages-and-site-navigation-cs/_static/image22.png)](master-pages-and-site-navigation-cs/_static/image21.png)

**Abbildung 9**: Hinzufügen einer Site-Zuordnung zu Ihres Projekts ([klicken Sie, um das Bild in voller Größe anzeigen](master-pages-and-site-navigation-cs/_static/image23.png))


Der Sitezuordnungsdatei ist eine XML-Datei. Beachten Sie, dass Visual Studio IntelliSense für die Siteübersichtsstruktur bereitstellt. Der Sitezuordnungsdatei müssen die `<siteMap>` Knoten wie der Stammknoten, genau einen enthalten muss `<siteMapNode>` untergeordnetes Element. Diese als erstes `<siteMapNode>` Element kann dann eine beliebige Anzahl von untergeordneten enthalten `<siteMapNode>` Elemente.

Definieren Sie die Website-Karte, um die Struktur des Dateisystems zu imitieren. Hinzufügen, also eine `<siteMapNode>` -Element für jede der drei Ordner und untergeordnete `<siteMapNode>` Elemente für jeden die ASP.NET-Seiten in diesen Ordnern, wie folgt:

Web.sitemap


[!code-xml[Main](master-pages-and-site-navigation-cs/samples/sample4.xml)]

Die Sitemap definiert der Website Navigationsstruktur, die eine Hierarchie ist, die die verschiedenen Abschnitte der Website beschreibt. Jede `<siteMapNode>` Element im `Web.sitemap` stellt einen Abschnitt in der Navigationsstruktur der Website dar.


[![Die Siteübersicht darstellt, eine hierarchische Navigationsstruktur](master-pages-and-site-navigation-cs/_static/image25.png)](master-pages-and-site-navigation-cs/_static/image24.png)

**Abbildung 10**: die Siteübersicht darstellt, einer hierarchischen Navigationsstruktur ([klicken Sie, um das Bild in voller Größe anzeigen](master-pages-and-site-navigation-cs/_static/image26.png))


ASP.NET stellt die Sitemap-Struktur durch die .NET Framework [SiteMap-Klasse](https://msdn.microsoft.com/library/system.web.sitemap.aspx). Diese Klasse verfügt über eine `CurrentNode` -Eigenschaft, die Informationen über den Abschnitt der Benutzer wird gerade besuchen; gibt die `RootNode` Eigenschaft gibt den Stamm der Siteübersicht zurück (Home, unsere sitezuordnung). Sowohl die `CurrentNode` und `RootNode` Eigenschaften zurückgeben [SiteMapNode](https://msdn.microsoft.com/library/system.web.sitemapnode.aspx) -Instanzen, die Eigenschaften wie `ParentNode`, `ChildNodes`, `NextSibling`, `PreviousSibling`und so weiter, die für die Siteübersicht zulassen die Hierarchie an, die durchlaufen werden.

## <a name="step-3-displaying-a-menu-based-on-the-site-map"></a>Schritt 3: Anzeigen eines Menüs, die basierend auf der Sitemap

Zugreifen auf Daten in ASP.NET 2.0 möglich programmgesteuert ausgeführt werden, wie in ASP.NET 1.x oder deklarativ über die neue [Datenquellen-Steuerelemente](https://msdn.microsoft.com/library/ms227679.aspx). Es gibt mehrere integrierte Datenquellen-Steuerelemente wie SqlDataSource-Steuerelement, für den Zugriff auf das ObjectDataSource-Steuerelement, für den Zugriff auf Daten von Klassen und anderen relationalen Datenbankdaten. Sie können auch eigene erstellen [benutzerdefinierte Datenquellen-Steuerelemente](https://msdn.microsoft.com/asp.net/reference/data/default.aspx?pull=/library/dnvs05/html/DataSourceCon1.asp).

Die Datenquellen-Steuerelemente dienen als Proxy zwischen Ihrer ASP.NET-Seite und die zugrunde liegenden Daten. Um ein Datenquellensteuerelement abgerufenen Daten anzuzeigen, wir in der Regel fügen Sie auf der Seite eine andere Websteuerelement hinzu und binden es an das Datenquellen-Steuerelement. Um ein Steuerelement an ein Datenquellen-Steuerelement zu binden, legen Sie einfach des Websteuerelements `DataSourceID` -Eigenschaft auf den Wert, der des Datenquellensteuerelements `ID` Eigenschaft.

Um bei der Arbeit mit der Sitemap-Daten zu unterstützen, bietet ASP.NET die SiteMapDataSource-Steuerelement, das Bezeichnung ermöglicht uns ein Websteuerelement für unsere Website, die Sitemap zu binden. Zwei Web-Steuerelemente, die TreeView- und Menu werden häufig verwendet, um eine Benutzeroberfläche für die Navigation bereitzustellen. Um die Sitemap-Daten an eine dieser zwei Steuerelemente zu binden, einfach zur Seite zusammen mit einem TreeView-Steuerelement hinzufügen einer SiteMapDataSource verbunden ist, oder im Menü-Steuerelement, dessen `DataSourceID` -Eigenschaft entsprechend festgelegt ist. Wir können beispielsweise ein Menüsteuerelement der Masterseite, die mit dem folgenden Markup hinzufügen:


[!code-aspx[Main](master-pages-and-site-navigation-cs/samples/sample5.aspx)]

Für eine feiner abgestufte Kontrolle über die ausgegebene HTML, können wir die SiteMapDataSource-Steuerelement an das Repeater-Steuerelement binden wie folgt:


[!code-aspx[Main](master-pages-and-site-navigation-cs/samples/sample6.aspx)]

Das SiteMapDataSource-Steuerelement gibt die Site Map Hierarchieebene eine zu einem Zeitpunkt der Stamm-Siteübersichtsknoten ab (Home, Site Map), klicken Sie dann die nächste Stufe (grundlegende Berichterstellung, Berichte filtern und angepasste Formatierung) und So weiter. Beim Binden der SiteMapDataSource an einem Wiederholungssteuerelement listet sie auf der ersten Ebene zurückgegeben, und instanziiert die `ItemTemplate` für jede `SiteMapNode` -Instanz in dieser ersten Ebene. Eine bestimmte Eigenschaft der Zugriff auf die `SiteMapNode`, verwenden wir `Eval(propertyName)`, d.h. wie wir jede erhalten `SiteMapNode`des `Url` und `Title` Eigenschaften für das Linksteuerelement.

Das obige Repeater-Beispiel wird das folgende Markup Rendern:


[!code-html[Main](master-pages-and-site-navigation-cs/samples/sample7.html)]

Diese Siteübersichtsknoten (grundlegende Berichterstellung, Berichte filtern und angepasste Formatierung) bilden die *zweite* der Siteübersicht gerendert wird, wird nicht die erste Ebene. Grund hierfür ist SiteMapDataSources `ShowStartingNode` -Eigenschaftensatz auf "false", verursacht die SiteMapDataSource den Stamm der Siteübersichtsknoten zu umgehen und stattdessen durch Rückgabe der zweiten Ebene in der Map-Standorthierarchie beginnen.

Die untergeordneten Elemente für die grundlegende Berichterstellung, Berichte filtern und angepasste Formatierung anzuzeigende `SiteMapNode` s, wir können einen anderen Repeater hinzufügen, auf des ersten Repeaters `ItemTemplate`. Diese zweite Repeater wird gebunden werden, um die `SiteMapNode` Instanz `ChildNodes` -Eigenschaft wie folgt:


[!code-aspx[Main](master-pages-and-site-navigation-cs/samples/sample8.aspx)]

Diese zwei Repeater führen das folgende Markup (Markup wurde aus Gründen der Übersichtlichkeit entfernt):


[!code-html[Main](master-pages-and-site-navigation-cs/samples/sample9.html)]

Mit CSS formatiert ausgewählt aus [Rachel Andrew](http://www.rachelandrew.co.uk/)Buchen der [der CSS-Anthologie: 101 wichtige Tipps, Tricks, &amp; Hacks](https://www.amazon.com/gp/product/0957921888/qid=1137565739/sr=8-1/ref=pd_bbs_1/103-0562306-3386214?n=507846&amp;s=books&amp;v=glance), wird die `<ul>` und `<li>` Elemente formatiert werden, Das Markup erzeugt die folgende visual Ausgabe:


![Ein Menü besteht aus zwei Repeater und etwas CSS](master-pages-and-site-navigation-cs/_static/image27.png)

**Abbildung 11**: ein Menü besteht aus zwei Repeater und etwas CSS aus.


Dieses Menü auf der Masterseite ist und an die Sitemap definiert gebunden `Web.sitemap`, das bedeutet, dass jede Änderung an der Sitemap auf allen sofort wiedergegeben werden Seiten, die die `Site.master` Masterseite.

## <a name="disabling-viewstate"></a>Deaktivieren von ViewState

Allen ASP.NET-Steuerelementen können optional beibehalten können ihren Zustand in der [Ansichtszustand](https://msdn.microsoft.com/msdnmag/issues/03/02/CuttingEdge/), die als einen ausgeblendeten Formularfelds in der gerenderten HTML-Code serialisiert wird. Ansichtszustand wird von Steuerelementen verwendet, auf ihren programmgesteuert geändert Status postbackübergreifend, denken Sie daran, wie z. B. die Daten an ein Web-Steuerelement gebunden. Während der Ansichtszustand Informationen, die über Postbacks hinweg gespeichert werden kann, wird die Größe der das Markup, das an den Client gesendet werden muss und kann zu schweren Seite Überfrachtung führen, wenn Sie nicht genau überwacht. Datenwebsteuerelemente besonders GridView sind besonders oftmals Dutzende von zusätzlichen KB Markup einer Seite hinzufügen. Während diese erhöhten Breitband "oder" Intranet-Benutzern vernachlässigbar sein kann, kann Ansichtszustand einige Sekunden den Roundtrip für DFÜ-Benutzer hinzufügen.

Um die Auswirkungen von Status anzeigen, finden Sie auf einer Seite in einem Browser, und zeigen Sie dann auf die Quelle der Webseite per anzuzeigen (in Internet Explorer finden Sie unter dem Menü "Ansicht" und wählen Sie die Quell-Option). Sie können auch aktivieren [Seitenablaufverfolgung](https://msdn.microsoft.com/library/sfbfw58f.aspx) um die Ansicht-Status-Zuordnung ein, die jedes der Steuerelemente auf der Seite anzuzeigen. Die Informationen zum Ansichtszustand in einem ausgeblendeten Formularfeld mit dem Namen serialisiert `__VIEWSTATE`befindet sich im eine `<div>` Element unmittelbar nach dem öffnenden `<form>` Tag. Ansichtszustand beibehalten wird nur bei einem Web Form verwendet wird; Wenn die ASP.NET-Seite keine umfasst eine `<form runat="server">` in seiner deklarativen Syntax, die keine `__VIEWSTATE` ausgeblendeten Formularfelds in dem gerenderten Markup.

Die `__VIEWSTATE` Formularfeld generiert, die von der Masterseite generierte Markup der Seite ungefähr 1.800 Bytes hinzugefügt. Diese zusätzlichen Überfrachtung liegt hauptsächlich das Repeater-Steuerelement, wie der Inhalt des Steuerelements SiteMapDataSource Ansichtszustand beibehalten werden. Während einer zusätzlichen 1.800 Bytes wie viel abzurufenden begeistert, dass, wenn viele Felder und Datensätze einer GridView-Ansicht mit nicht mag, kann das Feld Ansichtszustand ganz einfach mit einem Faktor von 10 oder mehr ansteigen.

Ansichtszustand kann auf der Seite oder das Steuerelement deaktiviert werden, durch Festlegen der `EnableViewState` Eigenschaft `false`, wodurch Verringern der Größe eines gerenderten Markup. Seit der Ansichtszustand für eine Web-Steuerelement an den Daten-Websteuerelement postbackübergreifend gebundenen Daten beibehält, beim Deaktivieren des Ansichtszustands für eine Webserver-Steuerelement müssen die Daten auf jede Postback gebunden werden. In ASP.NET-Version 1.x, die diese Verantwortung auf den Seitenentwickler; fiel mit ASP.NET 2.0 werden jedoch die datenwebsteuerelementen an ihre Datenquellen-Steuerelement bei jedem Postback Zeilenergebnissen erneut binden bei Bedarf.

Legen Sie auf den Ansichtszustand der Seite reduzieren wir des Repeater-Steuerelements `EnableViewState` Eigenschaft `false`. Dies kann über das Eigenschaftenfenster im Designer oder deklarativ in der Datenquellensicht erfolgen. Nach dieser Änderung sollte des Repeaters deklarative Markup aussehen:


[!code-aspx[Main](master-pages-and-site-navigation-cs/samples/sample10.aspx)]

Nach dieser Änderung wird die Seite des gerenderten Ansicht, dass die Größe des Benutzerzustands auf eine reine verkleinert wurde Zustandsgröße 52 Bytes, die eine Einsparung von 97 % in der Ansicht! In den Tutorials in dieser Reihe werden wir den Ansichtszustand des Daten-Websteuerelemente standardmäßig deaktivieren, um die Größe des gerenderten Markups zu reduzieren. In den meisten der Beispiele die `EnableViewState` Eigenschaft auf festgelegt `false` und ohne Erwähnung getan. Nur dann anzeigen, dass der Zustand erläutert wird ist in Szenarien, in denen sie in der Reihenfolge für die Daten aktiviert werden muss, Web steuern, um dessen zu erwartender Funktionalität bereitzustellen.

## <a name="step-4-adding-breadcrumb-navigation"></a>Schritt 4: Hinzufügen der Breadcrumb-Navigation

Um die Masterseite abgeschlossen haben, fügen Sie ein Benutzeroberflächenelement der Breadcrumb-Navigation für jede Seite. Die Breadcrumb-Leiste zeigt Benutzer schnell den aktuellen Position in der Hierarchie. Breadcrumb in ASP.NET 2.0 ist einfach nur hinzufügen ein SiteMapPath-Steuerelement auf der Seite. Es ist kein Code erforderlich.

Fügen Sie dieses Steuerelement für unsere Website, auf den Header `<div>`:


[!code-aspx[Main](master-pages-and-site-navigation-cs/samples/sample11.aspx)]

Die Breadcrumb-Leiste zeigt der aktuellen Seite des Benutzers wird in der Standorthierarchie-Zuordnung als auch für diesen Standort Zuordnung des Knotens "Vorgängerelemente," besuchen, ganz bis zum Stamm (Home, unsere sitezuordnung).


![Die Breadcrumb-Leiste zeigt die aktuelle Seite und dessen Vorgänger auf der Website Sitezuordnungshierarchie](master-pages-and-site-navigation-cs/_static/image28.png)

**Abbildung 12**: Breadcrumb zeigt die aktuelle Seite und dessen Vorgänger auf der Website Sitezuordnungshierarchie


## <a name="step-5-adding-the-default-page-for-each-section"></a>Schritt 5: Hinzufügen der Standardseite für die einzelnen Abschnitte

Die Lernprogramme in unserer Website werden in verschiedene Kategorien Basisberichte, filtern, benutzerdefinierte Formatierung, und so weiter mit einem Ordner für jede Kategorie und den entsprechenden Tutorials als ASP.NET-Seiten in diesem Ordner unterteilt. Jeder Ordner enthält außerdem eine `Default.aspx` Seite. Für diese Standardseite lassen Sie uns alle Anzeigen der Lernprogramme für den aktuellen Abschnitt. D. h. für die `Default.aspx` in die `BasicReporting` Ordner müssten wir Links zu `SimpleDisplay.aspx`, `DeclarativeParams.aspx`, und `ProgrammaticParams.aspx`. Hier in diesem Fall können verwenden wir die `SiteMap` -Klasse und eine Web-Steuerelement zum Anzeigen dieser Informationen auf Grundlage der Siteübersicht definierten `Web.sitemap`.

Wir zeigen eine ungeordnete Liste mit einem Wiederholungssteuerelement, erneut aus, aber dieses Mal mit dem wir den Titel und Beschreibung der Lernprogramme wird angezeigt. Da Markup und Code zum Ausführen dieser wird für jede ausgeführt werden müssen `Default.aspx` Seite können wir diese UI-Logik in Kapseln einer [Benutzersteuerelement](https://msdn.microsoft.com/library/y6wb1a0e.aspx). Erstellen Sie einen Ordner auf der Website namens `UserControls` und hinzufügen, die ein neues Element vom Typ mit dem Namen der Web-Benutzersteuerelement `SectionLevelTutorialListing.ascx`, und fügen Sie das folgende Markup hinzu:


[![Hinzufügen eines neuen Web-Benutzersteuerelements in den Ordner UserControls](master-pages-and-site-navigation-cs/_static/image30.png)](master-pages-and-site-navigation-cs/_static/image29.png)

**Abbildung 13**: Hinzufügen einer neuen Web-Benutzersteuerelement, das `UserControls` Ordner ([klicken Sie, um das Bild in voller Größe anzeigen](master-pages-and-site-navigation-cs/_static/image31.png))


SectionLevelTutorialListing.ascx


[!code-aspx[Main](master-pages-and-site-navigation-cs/samples/sample12.aspx)]

SectionLevelTutorialListing.ascx.cs


[!code-csharp[Main](master-pages-and-site-navigation-cs/samples/sample13.cs)]

Im vorherigen Beispiel Repeater wir gebunden der `SiteMap` Daten des Repeaters deklarativ; die `SectionLevelTutorialListing` Benutzersteuerelement, jedoch führt Sie dies programmgesteuert. In der `Page_Load` -Ereignishandler wird überprüft, um sicherzustellen, dass diese Seite s URL zu einem Knoten der sitezuordnung zugeordnet wird. Wenn dieses Steuerelement auf einer Seite verwendet wird, die eine entsprechende keinen `<siteMapNode>` Eintrag `SiteMap.CurrentNode` zurück `null` und keine Daten an den Repeater gebunden. Angenommen, wir verfügen über eine `CurrentNode`, binden wir die `ChildNodes` -Auflistung, um die Repeater. Da unsere Sitemap eingerichtet ist, dass die `Default.aspx` Seite in den einzelnen Abschnitten ist der übergeordnete Knoten aller die Lernprogramme in diesem Abschnitt, der dieser Code zeigt Links zu und Beschreibungen aller im Abschnitt des Tutorials, wie im folgenden Screenshot gezeigt.

Nachdem Sie diesen Repeater erstellt wurde, öffnen Sie die `Default.aspx` Seiten in jedem Ordner, wechseln Sie zur Entwurfsansicht, und ziehen Sie einfach das Benutzersteuerelement aus dem Projektmappen-Explorer auf die Entwurfsoberfläche, in denen die tutorialliste angezeigt werden sollen.


[![Das Benutzersteuerelement wurde an "default.aspx" hinzugefügt wurde](master-pages-and-site-navigation-cs/_static/image33.png)](master-pages-and-site-navigation-cs/_static/image32.png)

**Abbildung 14**: Benutzersteuerelement hat, wurde hinzugefügt, um `Default.aspx` ([klicken Sie, um das Bild in voller Größe anzeigen](master-pages-and-site-navigation-cs/_static/image34.png))


[![Die grundlegenden Reporting-Tutorials werden aufgeführt.](master-pages-and-site-navigation-cs/_static/image36.png)](master-pages-and-site-navigation-cs/_static/image35.png)

**Abbildung 15**: finden Sie die grundlegenden Reporting-Tutorials ([klicken Sie, um das Bild in voller Größe anzeigen](master-pages-and-site-navigation-cs/_static/image37.png))


## <a name="summary"></a>Zusammenfassung

Die Sitemap definiert und die Masterseite abgeschlossen haben wir jetzt ein konsistentes Layout und das Navigationsschema für die datenbezogenen Lernprogramme. Unabhängig davon, wie viele Seiten, die wir auf unserer Website hinzufügen, wird die Aktualisierung der Seitenlayout- oder Sitenavigationsinformationen von standortweite für ein schneller und einfacher Prozess aufgrund dieser Informationen wird zentralisiert. Insbesondere die Seitenlayoutinformationen definiert ist, auf der Masterseite `Site.master` und den Standort in Zuordnung `Web.sitemap`. Wir mussten schreiben *alle* code zum Erreichen dieser standortweite Seite Layout und Navigation den Mechanismus aus, und wir behalten die vollständige WYSIWYG-Designer-Unterstützung in Visual Studio.

Nach Abschluss der Datenzugriffsebene und den Geschäftslogikebene müssen eine konsistente Layout und die Website Seitennavigation definiert, und wir und zu untersuchen, häufige Muster von berichterstellung bereit. In der [Weiter](../basic-reporting/displaying-data-with-the-objectdatasource-cs.md) drei Lernprogramme betrachten wir grundlegende Berichtstasks Anzeigen von Daten, die von der BLL in der GridView, DetailsView, abgerufen und FormView steuert.

Viel Spaß beim Programmieren!

## <a name="further-reading"></a>Weiterführende Themen

Weitere Informationen zu den Themen in diesem Tutorial erläutert finden Sie in den folgenden Ressourcen:

- [Übersicht über ASP.NET-Masterseiten](https://msdn.microsoft.com/library/wtxbf3hh.aspx)
- [Masterseiten in ASP.NET 2.0](http://odetocode.com/Articles/419.aspx)
- [ASP.NET 2.0-Entwurfs-Vorlagen](https://msdn.microsoft.com/asp.net/reference/design/templates/default.aspx)
- [Übersicht über ASP.NET Website-Navigation](https://msdn.microsoft.com/library/e468hxky.aspx)
- [Untersuchen von ASP.NET 2.0 die Website-Navigation](http://aspnet.4guysfromrolla.com/articles/111605-1.aspx)
- [ASP.NET 2.0 Website Navigationsfunktionen](https://weblogs.asp.net/scottgu/archive/2005/11/20/431019.aspx)
- [Grundlegendes zu ASP.NET-Ansichtszustand](https://msdn.microsoft.com/library/default.asp?url=/library/dnaspp/html/viewstate.asp)
- [Vorgehensweise: Aktivieren der Ablaufverfolgung für eine ASP.NET-Seite](https://msdn.microsoft.com/library/94c55d08%28VS.80%29.aspx)
- [Benutzersteuerelemente von ASP.NET](https://msdn.microsoft.com/library/y6wb1a0e.aspx)

## <a name="about-the-author"></a>Der Autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor von sieben Büchern zu ASP/ASP.NET und Gründer von [4GuysFromRolla.com](http://www.4guysfromrolla.com), arbeitet mit Microsoft-Web-Technologien seit 1998. Er ist als ein unabhängiger Berater, Schulungsleiter und Autor. Sein neueste Buch wird [*Sams Schulen selbst ASP.NET 2.0 in 24 Stunden*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Er ist unter [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) oder über seinen Blog finden Sie unter [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Besonderen Dank an

Diese tutorialreihe wurde durch viele hilfreiche Reviewer überprüft. Führendes Prüfer für dieses Tutorial wurden Liz Shulok, Dennis Patterson und Hilton Giesenow. Meine zukünftigen MSDN-Artikeln überprüfen möchten? Wenn dies der Fall ist, löschen Sie mir eine Linie an [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Zurück](creating-a-business-logic-layer-cs.md)
> [Weiter](creating-a-data-access-layer-vb.md)
