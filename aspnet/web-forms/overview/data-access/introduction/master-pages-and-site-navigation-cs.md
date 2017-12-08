---
uid: web-forms/overview/data-access/introduction/master-pages-and-site-navigation-cs
title: Masterseiten und Website-Navigation (c#) | Microsoft Docs
author: rick-anderson
description: Eine gemeinsame Merkmale der benutzerfreundliche Websites ist, dass sie eine konsistente, standortweite Layout und das Navigationsschema aufweisen. Dieses Lernprogramm befasst sich y...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/31/2010
ms.topic: article
ms.assetid: 5aee8202-a4e3-4aa9-8a95-cd5d156cea4c
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/introduction/master-pages-and-site-navigation-cs
msc.type: authoredcontent
ms.openlocfilehash: 32ddda8d883a99805d2448c9673e585bfe9ef2f4
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="master-pages-and-site-navigation-c"></a>Masterseiten und Website-Navigation (c#)
====================
durch [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Beispiel-App herunterladen](http://download.microsoft.com/download/4/6/3/463cf87c-4724-4cbc-b7b5-3f866f43ba50/ASPNET_Data_Tutorial_3_CS.exe) oder [PDF herunterladen](master-pages-and-site-navigation-cs/_static/datatutorial03cs1.pdf)

> Eine gemeinsame Merkmale der benutzerfreundliche Websites ist, dass sie eine konsistente, standortweite Layout und das Navigationsschema aufweisen. In diesem Lernprogramm prüft ein konsistentes Aussehen und Verhalten für alle Seiten erstellen können, die problemlos aktualisiert werden können.


## <a name="introduction"></a>Einführung

Eine gemeinsame Merkmale der benutzerfreundliche Websites ist, dass sie eine konsistente, standortweite Layout und das Navigationsschema aufweisen. ASP.NET 2.0 verfügt über zwei neue Funktionen, die erheblich vereinfachen, implementieren eine standortweite Seite Layout und Navigation der Schemas: "master" Seiten und Websitenavigation. Masterseiten ermöglichen Entwicklern das Erstellen einer Vorlage mit der standortweiten mit festgelegten bearbeitbare Bereiche. Diese Vorlage kann anschließend auf ASP.NET-Seiten am Standort angewendet werden. Solche ASP.NET-Seiten müssen nur die Inhalte bereitstellen, für die Gestaltungsvorlage bearbeitbare Bereiche angegeben, der alle anderen Markup in die Masterseite identisch ist, über alle ASP.NET-Seiten, die Masterseite verwenden. Dieses Modell ermöglicht Entwicklern das Definieren und zu zentralisieren eine standortweite Seitenlayout, sodass es einfacher, ein einheitliches Erscheinungsbild für alle Seiten zu erstellen, die problemlos aktualisiert werden können.

Die [Navigation Standortsystem](http://aspnet.4guysfromrolla.com/articles/111605-1.aspx) bietet einen Mechanismus für Seitenentwickler zum Definieren einer Siteübersicht, und eine API für diese Siteübersicht programmgesteuert abgefragt werden. Neue Web Navigationssteuerelemente, denen die im Menü, TreeView und SiteMapPath erleichtern, rendern oder einen Teil der Siteübersicht in eine allgemeine Navigation Benutzeroberflächenelement. Wir verwenden die Standard-Website-Navigationsanbieter Dies bedeutet, dass unsere Siteübersicht in eine XML Datei definiert wird.

Um diese Konzepte veranschaulichen und unsere Website Lernprogramme sich nutzbarer machen, werfen wir kurz in dieser Lektion definieren eine standortweite Seitenlayout und Implementieren einer Siteübersicht Navigationsbenutzeroberfläche hinzufügen. Am Ende dieses Lernprogramms müssen wir ein professionelles Website-Design für unser Tutorial Webseiten zu erstellen.


[![Das Endergebnis dieses Lernprogramms](master-pages-and-site-navigation-cs/_static/image2.png)](master-pages-and-site-navigation-cs/_static/image1.png)

**Abbildung 1**: The-End-Ergebnis von diesem Lernprogramm ([klicken Sie hier, um das Bild in voller Größe angezeigt](master-pages-and-site-navigation-cs/_static/image3.png))


## <a name="step-1-creating-the-master-page"></a>Schritt 1: Erstellen der Masterseite

Der erste Schritt besteht darin die Gestaltungsvorlage für den Standort zu erstellen. Mit der rechten Maustaste jetzt nur die typisierte DataSet unserer Website besteht (`Northwind.xsd`in der `App_Code` Ordner), die BLL-Klassen (`ProductsBLL.cs`, `CategoriesBLL.cs`usw., sich im die `App_Code` Ordner), der Datenbank (`NORTHWND.MDF`in der `App_Data` Ordner), die Konfigurationsdatei (`Web.config`), und eine CSS-Stylesheet-Datei (`Styles.css`). Ich bereinigt die Seiten und Dateien, die veranschaulichen, verwenden die DAL und BLL aus der ersten beiden Lernprogramme, da wir diesen Beispielen ausführlicher in zukünftigen Lernprogrammen überprüfen.


![Die Dateien im Projekt](master-pages-and-site-navigation-cs/_static/image4.png)

**Abbildung 2**: die Dateien im Projekt


Klicken Sie zum Erstellen einer Masterseite mit der rechten Maustaste auf den Projektnamen im Projektmappen-Explorer, und wählen Sie Neues Element hinzufügen. Anschließend wählen Sie aus der Liste der Vorlagen den Gestaltungsvorlage und nennen Sie sie `Site.master`.


[![Fügen Sie eine neue Master-Seite auf der Website](master-pages-and-site-navigation-cs/_static/image6.png)](master-pages-and-site-navigation-cs/_static/image5.png)

**Abbildung 3**: Fügen Sie eine neue Master-Seite auf der Website ([klicken Sie hier, um das Bild in voller Größe angezeigt](master-pages-and-site-navigation-cs/_static/image7.png))


Definieren Sie die standortweite Seitenlayout hier in der Masterseite. Können Sie mithilfe die Entwurfsansicht und Hinzufügen von beliebigen Layout oder Web-Steuerelemente Sie müssen, oder Sie können das Markup manuell in der Datenquellensicht manuell hinzufügen. In meiner Masterseite ich verwende [cascading Stylesheets](http://www.w3schools.com/css/default.asp) zum Positionieren und Formatvorlagen mit den CSS-Einstellungen in der externen Datei definiert `Style.css`. Die CSS-Regeln werden definiert, während Sie nicht aus dem unten gezeigten Markup erkennen können, sodass die Navigation `<div>`des Inhalt ist absolut positioniert, sodass er auf der linken Seite angezeigt und verfügt über eine feste Breite von 200 Pixel.

Site.Master


[!code-aspx[Main](master-pages-and-site-navigation-cs/samples/sample1.aspx)]

Eine Masterseite definiert das Layout der Seite "static" und die Bereiche, die durch die ASP.NET-Seiten bearbeitet werden können, die die Gestaltungsvorlage verwenden. Dieser Inhalt bearbeitbare Bereiche des Warnschwellenwerts durch ContentPlaceHolder-Steuerelements, der im Inhalt angezeigt werden kann, `<div>`. Unsere Masterseite verfügt über eine einzelne ContentPlaceHolder (`MainContent`), aber der Masterseite kann mehrere ContentPlaceHolders enthalten.

Durch das Markup oben eingegebenen zeigt das Umschalten zur Entwurfsansicht der Masterseite Layout aus. Alle ASP.NET-Seiten, mit denen diese Masterseite müssen diesem uniform Layout mit der Möglichkeit, geben Sie das Markup für die `MainContent` Region.


[![Die Master-Seite, wenn durch die Entwurfsansicht angezeigt](master-pages-and-site-navigation-cs/_static/image9.png)](master-pages-and-site-navigation-cs/_static/image8.png)

**Abbildung 4**: die Gestaltungsvorlage, wenn angezeigt über die Entwurfsansicht ([klicken Sie hier, um das Bild in voller Größe angezeigt](master-pages-and-site-navigation-cs/_static/image10.png))


## <a name="step-2-adding-a-homepage-to-the-website"></a>Schritt 2: Hinzufügen einer Homepage der Website

Mit der Masterseite definiert wird können wir die ASP.NET-Seiten für die Website hinzufügen. Beginnen wir durch Hinzufügen von `Default.aspx`, unsere Website-Homepage. Mit der rechten Maustaste auf den Projektnamen im Projektmappen-Explorer, und wählen Sie Neues Element hinzufügen. Wählen Sie der Option Web Form aus der Vorlagenliste und dem Namen die Datei `Default.aspx`. Prüfen Sie außerdem das Kontrollkästchen "Masterseite auswählen".


[![Fügen Sie eine neue WebForm, überprüfen die Masterseite auswählen Kontrollkästchen hinzu](master-pages-and-site-navigation-cs/_static/image12.png)](master-pages-and-site-navigation-cs/_static/image11.png)

**Abbildung 5**: Fügen Sie eine neue WebForm, überprüfen die Masterseite auswählen Kontrollkästchen ([klicken Sie hier, um das Bild in voller Größe angezeigt](master-pages-and-site-navigation-cs/_static/image13.png))


Nach dem Klicken auf die Schaltfläche "OK", werden wir gefragt, welche Masterseite auswählen dieses neue ASP.NET-Seite verwendet werden soll. Das Projekt kann zwar mehrere Masterseiten, haben wir nur ein.


[![Wählen Sie die Master-Seite, die diese ASP.NET-Seite verwendet werden soll](master-pages-and-site-navigation-cs/_static/image15.png)](master-pages-and-site-navigation-cs/_static/image14.png)

**Abbildung 6**: Wählen Sie die Master-Seite dieses ASP.NET Seite sollte verwenden ([klicken Sie hier, um das Bild in voller Größe angezeigt](master-pages-and-site-navigation-cs/_static/image16.png))


Nach Entnahme der Masterseite, enthält der neue ASP.NET-Seiten das folgende Markup:

Default.aspx


[!code-aspx[Main](master-pages-and-site-navigation-cs/samples/sample2.aspx)]

In der `@Page` Richtlinie vorhanden ist, einen Verweis auf die Masterseite-Datei verwendet (`MasterPageFile="~/Site.master"`), und der ASP.NET-Seite Markup enthält ein Inhaltssteuerelement für jeden der ContentPlaceHolder-Steuerelemente, die in die Masterseite mit des Steuerelements definiert `ContentPlaceHolderID` Zuordnung des Inhalts die Steuerung an eine bestimmte ContentPlaceHolder. Das Inhaltssteuerelement wird, platzieren Sie das Markup in der entsprechenden ContentPlaceHolder angezeigt werden sollen. Legen Sie die `@Page` Richtlinie `Title` -Attribut zur Startseite und das Inhaltssteuerelement einige überall der Fall Inhalt hinzugefügt:

Default.aspx


[!code-aspx[Main](master-pages-and-site-navigation-cs/samples/sample3.aspx)]

Die `Title` Attribut in der `@Page` Richtlinie erlaubt es uns, legen Sie den Titel der Seite von der ASP.NET-Seite, obwohl die `<title>` Element wird in die Masterseite definiert. Es können auch den Titel programmgesteuert festlegen, mit `Page.Title`. Beachten Sie, dass die Gestaltungsvorlage Verweise auf Stylesheets (z. B. `Style.css`) automatisch aktualisiert, damit sie in jeder ASP.NET-Seite, unabhängig davon, welches Verzeichnis funktionieren die ASP.NET-Seite relativ zur Masterseite ist.

Wechseln zur Entwurfsansicht, dass wir sehen, wie unsere-Seite in einem Browser angezeigt wird. Beachten Sie, die im Entwurf für die ASP.NET-Seite, dass nur der Inhalt bearbeitbare Bereiche bearbeitet werden nicht ContentPlaceHolder Markup in der master-Seite definierte anzeigen wird abgeblendet.


[![Die Entwurfsansicht für die ASP.NET-Seite zeigt der bearbeitbare und nicht bearbeitbare Bereiche](master-pages-and-site-navigation-cs/_static/image18.png)](master-pages-and-site-navigation-cs/_static/image17.png)

**Abbildung 7**: die Entwurfsansicht für die ASP.NET Seite zeigt sowohl den bearbeitet werden und nicht bearbeitbar Regionen ([klicken Sie hier, um das Bild in voller Größe angezeigt](master-pages-and-site-navigation-cs/_static/image19.png))


Wenn die `Default.aspx` Seite von einem Browser aufgerufen wird, das Modul für ASP.NET automatisch zusammengeführt, den Inhalt der Seite Gestaltungsvorlage und ASP. NET den Inhalt und rendert den zusammengeführten Inhalt in den letzten HTML-Code, die an den Browser, nach unten gesendet wird. Wenn die Gestaltungsvorlage Inhalt aktualisiert wird, müssen alle ASP.NET-Seiten, die diese Masterseite verwenden ihre Inhalte mit der neuen Masterseite Inhalte, das sie angefordert werden das nächste Mal wieder zusammengeführt. Kurz gesagt, der Gestaltungsvorlage Modell können für eine einzelne Seite in der Layoutvorlage werden definiert (Masterseite), deren Änderungen werden sofort übernommen, für die gesamte Website.

## <a name="adding-additional-aspnet-pages-to-the-website"></a>Hinzufügen von zusätzlichen ASP.NET-Seiten zu der Website

Werfen Sie einen Moment Zeit, die dem Standort zusätzliche ASP.NET Seite Stubs hinzufügen, die letztendlich die verschiedenen reporting Demos enthalten soll. Es werden mehr als 35 Demos insgesamt also anstatt aller Seiten Stub wir erstellen nur ein paar zu erstellen. Da es auch viele Kategorien von Demos werden, hinzufügen, um besser verwalten die Demos einen Ordner für die Kategorien. Fügen Sie jetzt die folgenden drei Ordner hinzu:

- `BasicReporting`
- `Filtering`
- `CustomFormatting`

Abschließend fügen Sie neue Dateien hinzu, wie im Projektmappen-Explorer in Abbildung 8 gezeigt. Denken Sie daran, jede Datei hinzufügen, überprüfen Sie das Kontrollkästchen "Masterseite auswählen".


![Fügen Sie die folgenden Dateien hinzu.](master-pages-and-site-navigation-cs/_static/image20.png)

**Abbildung 8**: Fügen Sie die folgenden Dateien hinzu.


## <a name="step-2-creating-a-site-map"></a>Schritt 2: Erstellen einer Siteübersicht

Eine der Herausforderungen der Verwaltung von einer Website besteht aus mehr als eine Handvoll von Seiten liefert eine einfache Möglichkeit für Besucher der Website navigieren. Zunächst muss die Website-Navigationsstruktur definiert werden. Als Nächstes muss diese Struktur in navigierbaren Elemente der Benutzeroberfläche, z. B. Menüs oder Breadcrumbs übersetzt werden. Schließlich muss der gesamte Vorgang beibehalten und aktualisiert, wenn neue Seiten, die auf diese Site und vorhandene entfernt hinzugefügt werden. Vor ASP.NET 2.0 wurden Entwickler zum Erstellen ihrer eigenen für Navigationsstruktur für den Standort, Beibehaltung und übersetzen es in die navigierbaren Elemente der Benutzeroberfläche. Mit ASP.NET 2.0 können allerdings Entwickler sehr flexibel nutzen im Navigationsbereich Standortsystem erstellt.

Die ASP.NET 2.0-Standort-Navigationssystem bietet eine Möglichkeit für einen Entwickler eine Siteübersicht definieren und dann diese Informationen über eine programmgesteuerte-API zugreifen. Im Lieferumfang von ASP.NET sind eines Site-Zuordnung-Anbieters, der erwartet Siteübersichtsdaten in eine XML-Datei, die auf eine bestimmte Weise formatiert gespeichert werden. Aber, da das Standortsystem für die Navigation basiert auf der [Anbietermodell](http://aspnet.4guysfromrolla.com/articles/101905-1.aspx) kann erweitert werden, um alternative Möglichkeiten zum Serialisieren der Standortinformationen für die Zuordnung zu unterstützen. Prosises Artikel [The SQL Standort zuordnen Anbieter Sie habe wurde warten für](https://msdn.microsoft.com/msdnmag/issues/06/02/WickedCode/default.aspx) wird gezeigt, wie einem Kartenanbieter Standort erstellen, die Siteübersicht in einer SQL Server-Datenbank speichert; eine weitere Möglichkeit ist die Erstellung [einem Kartenanbieter Standort anhand der Dateisystemstruktur](http://aspnet.4guysfromrolla.com/articles/020106-1.aspx).

Für dieses Lernprogramm jedoch wir verwenden den Standardanbieter der Siteübersicht, der geliefert wird mit ASP.NET 2.0. Um die Website zu erstellen, einfach mit der rechten Maustaste auf den Projektnamen im Projektmappen-Explorer, wählen Sie Neues Element hinzufügen, und wählen Sie die Option Siteübersicht. Lassen Sie den Namen als `Web.sitemap` , und klicken Sie auf die Schaltfläche "hinzufügen".


[![Hinzufügen einer Siteübersicht zum Projekt](master-pages-and-site-navigation-cs/_static/image22.png)](master-pages-and-site-navigation-cs/_static/image21.png)

**Abbildung 9**: Hinzufügen einer Website-Zuordnung zu einem Projekt ([klicken Sie hier, um das Bild in voller Größe angezeigt](master-pages-and-site-navigation-cs/_static/image23.png))


Die Zuordnungsdatei für die Website ist eine XML-Datei. Beachten Sie, dass Visual Studio IntelliSense für die Siteübersichtsstruktur bereitstellt. Die Standort-Zuordnungsdatei benötigen die `<siteMap>` -Knoten befindet wie der Stammknoten, der genau eine enthalten muss `<siteMapNode>` untergeordnetes Element. Diese erste `<siteMapNode>` Element kann dann eine beliebige Anzahl von untergeordneten enthalten `<siteMapNode>` Elemente.

Definieren Sie die Website-Karte, um die Struktur des Dateisystems imitieren. D. h. Hinzufügen einer `<siteMapNode>` -Element für jede der drei Ordner und untergeordnete `<siteMapNode>` Elemente für jeden der ASP.NET-Seiten in diesen Ordnern wie folgt:

Web.sitemap


[!code-xml[Main](master-pages-and-site-navigation-cs/samples/sample4.xml)]

Die Siteübersicht definiert die Website Navigationsstruktur, also eine Hierarchie, die in den verschiedenen Abschnitten des Standorts beschreibt. Jede `<siteMapNode>` Element im `Web.sitemap` stellt einen Abschnitt in der Navigationsstruktur des Standorts dar.


[![Die Siteübersicht stellt eine hierarchische Navigationsstruktur](master-pages-and-site-navigation-cs/_static/image25.png)](master-pages-and-site-navigation-cs/_static/image24.png)

**Abbildung 10**: die Siteübersicht stellt einen hierarchischen Navigationsstruktur ([klicken Sie hier, um das Bild in voller Größe angezeigt](master-pages-and-site-navigation-cs/_static/image26.png))


ASP.NET stellt der Siteübersicht Struktur über das .NET Framework [SiteMap Klasse](https://msdn.microsoft.com/en-us/library/system.web.sitemap.aspx). Diese Klasse verfügt über eine `CurrentNode` Eigenschaft, die Informationen über den Abschnitt der Benutzer wird gerade besuchen; Zurückgeben der `RootNode` Eigenschaft gibt den Stamm der Siteübersicht zurück (in unserem Siteübersicht Home). Sowohl die `CurrentNode` und `RootNode` Eigenschaften zurückgeben [SiteMapNode](https://msdn.microsoft.com/en-us/library/system.web.sitemapnode.aspx) Instanzen, die über Eigenschaften verfügen wie `ParentNode`, `ChildNodes`, `NextSibling`, `PreviousSibling`und so weiter, mit denen für die Website-Karte die Hierarchie an, die durchlaufen werden.

## <a name="step-3-displaying-a-menu-based-on-the-site-map"></a>Schritt 3: Anzeigen eines Menüs basierend auf der Website-Karte

Zugreifen auf Daten in ASP.NET 2.0 kann programmgesteuert erreicht, wie in ASP.NET 1.x oder deklarativ über die neue [Datenquellensteuerelementen](https://msdn.microsoft.com/en-us/library/ms227679.aspx). Es gibt mehrere integrierte Datenquellensteuerelemente z. B. die SqlDataSource-Steuerelement für den Zugriff auf relationale Daten, das ObjectDataSource-Steuerelement für den Zugriff auf Daten aus Klassen und andere. Sie können auch eigene erstellen [benutzerdefinierte Datenquellensteuerelementen](https://msdn.microsoft.com/asp.net/reference/data/default.aspx?pull=/library/en-us/dnvs05/html/DataSourceCon1.asp).

Die Datenquellen-Steuerelementen dienen als Proxy zwischen die ASP.NET-Seite und den zugrunde liegenden Daten. Um ein Datenquellensteuerelement abgerufenen Daten anzeigen zu können, müssen wir in der Regel fügen ein weiteres Websteuerelement auf der Seite und zu den Datenquellen-Steuerelement zu binden. Um ein Websteuerelement an ein Datenquellen-Steuerelement binden möchten, legen Sie einfach des Websteuerelements `DataSourceID` -Eigenschaft auf den Wert für des Datenquellensteuerelements `ID` Eigenschaft.

Zur Unterstützung bei der Arbeit mit Daten der Siteübersicht enthält ASP.NET die SiteMapDataSource-Steuerelement, das wir ein Websteuerelement gegen unsere Website Siteübersicht binden kann. Zwei Websteuerelementen das TreeView und im Menü werden häufig verwendet, um eine Benutzeroberfläche für die Navigation bereitstellen. Um auf einen dieser beiden Steuerelemente der Siteübersichtsdaten zu binden, fügen Sie einfach eine SiteMapDataSource auf der Seite zusammen mit einer ' TreeView ' oder im Menü-Steuerelement, dessen `DataSourceID` Eigenschaft entsprechend festgelegt. Wir können beispielsweise ein Menüsteuerelement der Masterseite verwenden das folgende Markup hinzufügen:


[!code-aspx[Main](master-pages-and-site-navigation-cs/samples/sample5.aspx)]

Für eine feinere Kontrolle über die ausgegebene HTML, binden wir die SiteMapDataSource-Steuerelements an Wiederholungsmodul-Steuerelement wie folgt:


[!code-aspx[Main](master-pages-and-site-navigation-cs/samples/sample6.aspx)]

Der SiteMapDataSource steuerelementrückgabe Websiteebene Zuordnung Hierarchie eine zu einem Zeitpunkt ab, mit dem Stammknoten der Website zuordnen (Home, in unserem Siteübersicht), klicken Sie dann die nächste Ebene (Basisberichte, Filtern von Berichten und Formatierung angepasst) und So weiter. Wenn SiteMapDataSource an ein Repeater binden, listet er die erste Ebene zurückgegeben, und instanziiert die `ItemTemplate` für jede `SiteMapNode` Instanz in dieser ersten Ebene. Eine bestimmte Eigenschaft der Zugriff auf die `SiteMapNode`, verwenden wir `Eval(propertyName)`, wie jede erhalten wir also `SiteMapNode`des `Url` und `Title` Eigenschaften für das Linksteuerelement.

Das obige Repeater-Beispiel wird das folgende Markup Rendern:


[!code-html[Main](master-pages-and-site-navigation-cs/samples/sample7.html)]

Diese Zuordnung Websiteknoten (Basisberichte, Filtern von Berichten und Formatierung angepasst) bilden die *zweite* der Siteübersicht gerendert wird, wird nicht die erste Ebene. Grund hierfür ist die SiteMapDataSource `ShowStartingNode` auf "false", Eigenschaft festgelegt ist, verursacht die SiteMapDataSource Stammknotens der Zuordnung zu umgehen und stattdessen wird durch Zurückgeben der zweiten Ebene in der Map-Standorthierarchie beginnen.

Die untergeordneten Elemente für die Basisberichte, Filtern von Berichten und benutzerdefinierte Formatierung anzuzeigende `SiteMapNode` s, können wir eine andere Repeater der anfänglichen Repeater hinzufügen `ItemTemplate`. Diese zweite Repeater wird gebunden werden, um die `SiteMapNode` Instanz `ChildNodes` Eigenschaft wie folgt:


[!code-aspx[Main](master-pages-and-site-navigation-cs/samples/sample8.aspx)]

Diese zwei Repeater führen das folgende Markup (einige Markup wurde aus Gründen der Übersichtlichkeit entfernt):


[!code-html[Main](master-pages-and-site-navigation-cs/samples/sample9.html)]

Verwendung von CSS Formatvorlagen ausgewählt aus [Rachel Andrew](http://www.rachelandrew.co.uk/)der Mappe [der CSS-Anthologie: 101 wichtige Tipps, Tricks &amp; Hacks](https://www.amazon.com/gp/product/0957921888/qid=1137565739/sr=8-1/ref=pd_bbs_1/103-0562306-3386214?n=507846&amp;s=books&amp;v=glance), die `<ul>` und `<li>` Elemente formatiert werden, dass Das Markup erzeugt die folgende visual Ausgabe:


![Ein Menü besteht aus zwei Repeater und CSS](master-pages-and-site-navigation-cs/_static/image27.png)

**Abbildung 11**: ein Menü besteht aus zwei Repeater und CSS


Dieses Menü ist auf der Masterseite und an der Siteübersicht definierten gebunden `Web.sitemap`, d. h., dass jede Änderung an der Siteübersicht sofort auf allen wiedergegeben werden Seiten, die die `Site.master` Masterseite.

## <a name="disabling-viewstate"></a>Deaktivieren von "ViewState" Speichern

Alle ASP.NET-Steuerelemente können optional ihren Zustand zu beizubehalten der [Ansichtszustand](https://msdn.microsoft.com/msdnmag/issues/03/02/CuttingEdge/), die als ein ausgeblendetes Formularfeld im gerenderten HTML-Code serialisiert wird. Ansichtszustand wird von Steuerelementen verwendet, programmgesteuert geändert Zustand über mehrere Postbacks merken zu müssen, wie z. B. die Daten an ein Webserver-Steuerelement gebunden. Während der Ansichtszustand Informationen über Postbacks speichern erlaubt, vergrößert es das Markup, das an den Client gesendet werden muss und kann zu schweren Seite Aufblähen führen, wenn Sie nicht genau überwacht. Data-Websteuerelemente insbesondere die GridView sind besonders oftmals eine Dutzende zusätzlicher Markup in Kilobyte zu einer Seite hinzufügen. Während eine solche Preiserhöhung für Breitband oder Intranet Benutzer unerheblich sein kann, kann Ansichtszustand einige Sekunden Roundtrips für DFÜ-Benutzer hinzufügen.

Um die Auswirkung des Status anzeigen, finden Sie auf einer Seite in einem Browser, und zeigen Sie dann die Quelle gesendet, indem Sie auf der Webseite anzuzeigen (in Internet Explorer, wechseln Sie zum Menü "Ansicht" und wählen Sie die Quelle aus). Sie können auch aktivieren [Seitenablaufverfolgung](https://msdn.microsoft.com/en-us/library/sfbfw58f.aspx) , finden in der Ansicht Status Zuordnung der Steuerelemente auf der Seite verwendet. Die Ansichtszustandsinformationen erfolgt die Serialisierung in ein ausgeblendetes Formularfeld mit dem Namen `__VIEWSTATE`befindet sich im ein `<div>` -Element unmittelbar nach dem Öffnen `<form>` Tag. Ansichtszustand wird nur beibehalten, wenn es ist ein Web Form verwendet wird; Wenn die ASP.NET-Seite keine umfasst eine `<form runat="server">` in seiner deklarativen Syntax, die es keine `__VIEWSTATE` ausgeblendetes Formularfeld in der gerenderten Markups.

Die `__VIEWSTATE` Formularfelds generiert, die für die Gestaltungsvorlage generierten Seitenmarkup ungefähr 1.800 Bytes hinzugefügt. Diese zusätzlichen Aufblähen ist in erster Linie zum Wiederholungsmodul-Steuerelement, wie der Inhalt des Steuerelements SiteMapDataSource Ansichtszustand beibehalten werden. Während einer zusätzlichen 1.800 Bytes nicht wie viel, begeistert abrufen, wenn eine GridView mit vielen Felder und Datensätze mit besonders intuitiv erscheinen, kann der Ansichtszustand problemlos mit einem Faktor von 10 oder mehr wächst.

Ansichtszustand kann auf der Seite oder Steuerelement deaktiviert werden, durch Festlegen der `EnableViewState` Eigenschaft `false`, wodurch Verringern der Größe des gerenderten Markups. Seit der Ansichtszustand für eine Web-Steuerelement an den Daten-Websteuerelement über Postbacks gebundenen Daten weiterhin besteht, beim Deaktivieren des Ansichtszustands für ein Webserver-Steuerelement müssen die Daten für jede Postback gebunden werden. In ASP.NET-Version 1.x, die diese Verantwortung, auf den Entwickler der Seite fallen; mit ASP.NET 2.0 werden jedoch die Websteuerelementen Daten erneut an ihre Datenquellen-Steuerelements auf jedes Postback binden bei Bedarf.

Legen Sie Anzeigezustand der Seite wir reduzieren Wiederholungsmodul-Steuerelement `EnableViewState` Eigenschaft `false`. Dies kann über das Eigenschaftenfenster im Designer oder deklarativ in der Quellansicht erfolgen. Nach dieser Änderung sollte die Repeater deklarative Markup formuliert werden:


[!code-aspx[Main](master-pages-and-site-navigation-cs/samples/sample10.aspx)]

Nachdem diese Änderung wird die Seite des gerenderten Ansicht, dass die Größe des Benutzerzustands auf einem reinen verkleinert wurde Ansichtszustand 52 Bytes, eine 97 % in den Bereichen im Größe! In den Lernprogrammen für diese Reihe müssen wir den Ansichtszustand des ausgewählten Websteuerelemente standardmäßig deaktivieren, um die Größe des gerenderten Markups zu reduzieren. In den meisten der Beispiele der `EnableViewState` -Eigenschaftensatz auf `false` und ohne Angabe geschehen. Nur dann anzeigen, dass der Status näher erläutert wird ist in Szenarien, in denen sie damit die Daten aktiviert werden muss, Web steuern, um dessen zu erwartender Funktionalität bereitzustellen.

## <a name="step-4-adding-breadcrumb-navigation"></a>Schritt 4: Hinzufügen der Brotkrümelnavigation

Um die Gestaltungsvorlage abzuschließen, fügen Sie ein Benutzeroberflächenelement der Breadcrumb-Navigation auf jeder Seite. Die Breadcrumb-Leiste zeigt Benutzer schnell ihre aktuelle Position innerhalb der Standorthierarchie. Breadcrumb in ASP.NET 2.0 ist einfach nur hinzufügen ein SiteMapPath-Steuerelement auf der Seite. Es ist kein Code erforderlich.

Fügen Sie dieses Steuerelement für unsere Website, die dem Header `<div>`:


[!code-aspx[Main](master-pages-and-site-navigation-cs/samples/sample11.aspx)]

Die Breadcrumb-Leiste zeigt der aktuellen Seite der Benutzer wird in der Standorthierarchie für die Zuordnung als auch für diesen Standort zuordnen des Knotens "Vorgänger," besuchen, ganz Stamm (in unserem Siteübersicht Home).


![Die Breadcrumb-Leiste zeigt die aktuelle Seite und seiner Vorgänger auf der Website zuordnen, Hierarchie](master-pages-and-site-navigation-cs/_static/image28.png)

**Abbildung 12**: die Breadcrumb-Leiste zeigt die aktuelle Seite und seiner Vorgänger auf der Website zuordnen, Hierarchie


## <a name="step-5-adding-the-default-page-for-each-section"></a>Schritt 5: Hinzufügen der Standardseite für die einzelnen Abschnitte

Die Lernprogramme in unserer Website sind in verschiedenen Kategorien Basisberichte, filtern, die benutzerdefinierte Formatierung mit einen Ordner für jede Kategorie und die entsprechenden Lernprogramme als ASP.NET-Seiten in diesem Ordner und so weiter unterteilt. Darüber hinaus jeder Ordner enthält eine `Default.aspx` Seite. Für diese Standardseite wir alle Anzeigen der Lernprogramme für den aktuellen Bereich. D. h. für die `Default.aspx` in der `BasicReporting` Ordner hätten wir Links zu `SimpleDisplay.aspx`, `DeclarativeParams.aspx`, und `ProgrammaticParams.aspx`. Hier erneut, wir können die `SiteMap` Klasse und ein Websteuerelement zum Anzeigen dieser Informationen auf Grundlage der Siteübersicht definierten `Web.sitemap`.

Wir zeigen eine ungeordnete Liste mit ein Repeater erneut, diesmal jedoch wir Titel und Beschreibung der Lernprogramme darstellen. Da Markup und Code zum Ausführen dieser wird für jede wiederholt werden müssen `Default.aspx` Seite, können wir diese Benutzeroberflächen-Logik in Kapseln einer [Benutzersteuerelement](https://msdn.microsoft.com/en-us/library/y6wb1a0e.aspx). Erstellen Sie einen Ordner auf der Website aufgerufen `UserControls` und hinzuzufügen, die ein neues Element vom Typ Webbenutzer-Steuerelement mit dem Namen `SectionLevelTutorialListing.ascx`, und fügen Sie das folgende Markup hinzu:


[![Fügen Sie ein neue Web-Benutzersteuerelement in den Ordner Benutzersteuerelemente](master-pages-and-site-navigation-cs/_static/image30.png)](master-pages-and-site-navigation-cs/_static/image29.png)

**Abbildung 13**: Fügen Sie eine neue Web-Benutzersteuerelement auf die `UserControls` Ordner ([klicken Sie hier, um das Bild in voller Größe angezeigt](master-pages-and-site-navigation-cs/_static/image31.png))


SectionLevelTutorialListing.ascx


[!code-aspx[Main](master-pages-and-site-navigation-cs/samples/sample12.aspx)]

SectionLevelTutorialListing.ascx.cs


[!code-csharp[Main](master-pages-and-site-navigation-cs/samples/sample13.cs)]

Im vorherigen Beispiel Repeater gebunden wir die `SiteMap` Daten Repeater deklarativ; das `SectionLevelTutorialListing` Benutzersteuerelement, allerdings tut programmgesteuert. In der `Page_Load` -Ereignishandler wird überprüft, um sicherzustellen, dass diese Seite s URL einem Knoten in der Siteübersicht entspricht. Wenn dieses Benutzersteuerelement auf einer Seite verwendet wird, die eine entsprechende keinen `<siteMapNode>` Eintrag `SiteMap.CurrentNode` zurück `null` und keine Daten werden an die Repeater gebunden werden. Angenommen, wir haben eine `CurrentNode`, binden wir die `ChildNodes` Auflistung Wiederholungsmoduls. Da unsere Siteübersicht eingerichtet ist, dass die `Default.aspx` Seite in jedem Bereich ist der übergeordnete Knoten aller die Lernprogramme in diesem Abschnitt, dieser Code zeigt Links zu und Beschreibungen aller im Abschnitt Lernprogramme, wie im Screenshot unten gezeigt.

Nachdem diese Repeater erstellt wurde, öffnen Sie die `Default.aspx` Seiten in jeder der Ordner, wechseln Sie zur Entwurfsansicht, und ziehen Sie einfach das Benutzersteuerelement im Projektmappen-Explorer auf die Entwurfsoberfläche, in dem Sie Tutorial Liste angezeigt werden sollen.


[![Das Benutzersteuerelement hat "default.aspx" hinzugefügt wurde](master-pages-and-site-navigation-cs/_static/image33.png)](master-pages-and-site-navigation-cs/_static/image32.png)

**Abbildung 14**: Benutzersteuerelement hat hinzugefügtes `Default.aspx` ([klicken Sie hier, um das Bild in voller Größe angezeigt](master-pages-and-site-navigation-cs/_static/image34.png))


[![Die grundlegenden Reporting Lernprogramme sind aufgeführt.](master-pages-and-site-navigation-cs/_static/image36.png)](master-pages-and-site-navigation-cs/_static/image35.png)

**Abbildung 15**: der grundlegenden Reporting Lernprogramme aufgelisteten ([klicken Sie hier, um das Bild in voller Größe angezeigt](master-pages-and-site-navigation-cs/_static/image37.png))


## <a name="summary"></a>Zusammenfassung

Mit der Siteübersicht definierten und die Masterseite abgeschlossen haben wir nun eine konsistente Layout und das Navigationsschema für datenbezogene Lernprogramme. Unabhängig davon, wie viele Seiten, die wir unsere Website hinzu, ist das Aktualisieren der standortweiten Layout bzw. einen anderen Standort Navigation Seiteninformationen kurze und einfache Prozesses, weil diese Informationen werden zentral aus. Insbesondere wird die Seitenlayoutinformationen definiert, in die Masterseite `Site.master` und den Standort in Zuordnung `Web.sitemap`. Es hatte keinen Bedarf schreiben *alle* code, um diesen Mechanismus standortweite-Seite, Layout und die Navigation zu erreichen, und wir werden vollständige WYSIWYG-Designer-Unterstützung in Visual Studio beibehalten.

Die Datenzugriffsebene und Business Logic Layer abgeschlossen und eine konsistentes Layout und die Website Seitennavigation definiert haben sollten, wir beginnen, allgemeine reporting Muster untersuchen. In der [Weiter](../basic-reporting/displaying-data-with-the-objectdatasource-cs.md) drei Lernprogramme betrachten wir grundlegende Aufgaben anzeigen von Daten aus der BLL im GridView, DetailsView, abgerufen und FormView steuert.

Viel Spaß beim Programmieren!

## <a name="further-reading"></a>Weiterführende Themen

Weitere Informationen zu den Themen in diesem Lernprogramm erläutert finden Sie in den folgenden Ressourcen:

- [Übersicht über ASP.NET Master-Seiten](https://msdn.microsoft.com/en-us/library/wtxbf3hh.aspx)
- [Masterseiten in ASP.NET 2.0](http://odetocode.com/Articles/419.aspx)
- [ASP.NET 2.0 Entwurfsvorlagen](https://msdn.microsoft.com/asp.net/reference/design/templates/default.aspx)
- [Übersicht über die Navigation ASP.NET-Website](https://msdn.microsoft.com/en-us/library/e468hxky.aspx)
- [Untersuchen ASP.NET 2.0 der Websitenavigation](http://aspnet.4guysfromrolla.com/articles/111605-1.aspx)
- [Funktionen für die Navigation von ASP.NET 2.0-Standort](https://weblogs.asp.net/scottgu/archive/2005/11/20/431019.aspx)
- [Grundlegendes zu ASP.NET-Ansichtszustand](https://msdn.microsoft.com/library/default.asp?url=/library/en-us/dnaspp/html/viewstate.asp)
- [Vorgehensweise: Aktivieren der Ablaufverfolgung für eine ASP.NET-Seite](https://msdn.microsoft.com/en-us/library/94c55d08%28VS.80%29.aspx)
- [ASP.NET-Benutzersteuerelemente](https://msdn.microsoft.com/en-us/library/y6wb1a0e.aspx)

## <a name="about-the-author"></a>Informationen zum Autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor von sieben ASP/ASP.NET-Büchern und Gründer von [4GuysFromRolla.com](http://www.4guysfromrolla.com), Microsoft Web-Technologien seit 1998 arbeitet. Scott fungiert als ein unabhängiger Berater, Trainer und Writer. Sein neueste Buch wird [ *Sams Schulen selbst ASP.NET 2.0 in 24 Stunden*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Er die erreicht werden kann, zur [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) oder über seinen Blog die finden Sie unter [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Besonderen Dank an

Diese Reihe von Lernprogrammen wurde durch viele nützliche Bearbeiter überprüft. Lead Prüfer für dieses Lernprogramm wurden Liz Shulok, Dennis Patterson und Hilton Giesenow. Meine bevorstehende MSDN-Artikel Überprüfen von Interesse? Wenn dies der Fall ist, löschen Sie mich zeilenweise [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[Zurück](creating-a-business-logic-layer-cs.md)
[Weiter](creating-a-data-access-layer-vb.md)
