---
uid: web-forms/overview/older-versions-getting-started/master-pages/specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs
title: Der Titel, Meta-Tags und anderer HTML-Header angeben, auf der Masterseite (c#) | Microsoft-Dokumentation
author: rick-anderson
description: Untersucht verschiedene Verfahren zum Definieren von ausgewählten &lt;Head&gt; Elemente auf der Masterseite der Inhaltsseite.
ms.author: riande
ms.date: 05/21/2008
ms.assetid: 0aa1c84f-c9e2-4699-b009-0e28643ecbc6
msc.legacyurl: /web-forms/overview/older-versions-getting-started/master-pages/specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs
msc.type: authoredcontent
ms.openlocfilehash: e3846ea696a1a5a29fd53d6753878fab9dd9a95d
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2018
ms.locfileid: "41838083"
---
<a name="specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-c"></a>Der Titel, Meta-Tags und anderer HTML-Header angeben, auf der Masterseite (c#)
====================
durch [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Code herunterladen](http://download.microsoft.com/download/e/e/f/eef369f5-743a-4a52-908f-b6532c4ce0a4/ASPNET_MasterPages_Tutorial_03_CS.zip) oder [PDF-Datei herunterladen](http://download.microsoft.com/download/8/f/6/8f6349e4-6554-405a-bcd7-9b094ba5089a/ASPNET_MasterPages_Tutorial_03_CS.pdf)

> Untersucht verschiedene Verfahren zum Definieren von ausgewählten &lt;Head&gt; Elemente auf der Masterseite der Inhaltsseite.


## <a name="introduction"></a>Einführung

Neue Masterseiten in Visual Studio 2008 erstellt haben, werden standardmäßig zwei ContentPlaceHolder-Steuerelemente: eines mit dem Namen Head und befindet sich in der `<head>` Element und eine mit dem Namen `ContentPlaceHolder1`, innerhalb des Web Forms platziert. Der Zweck der `ContentPlaceHolder1` besteht darin, einen Bereich im Webformular zu definieren, die pro Seite von Seite angepasst werden können. Die `head` ContentPlaceHolder ermöglicht, Seiten zum Hinzufügen von benutzerdefinierten Inhalten der `<head>` Abschnitt. (Natürlich diese zwei ContentPlaceHolder-Steuerelemente geändert oder entfernt werden können und die Masterseite kann zusätzliche ContentPlaceHolder hinzugefügt werden. Unsere Masterseite `Site.master`, derzeit verfügt über vier ContentPlaceHolder-Steuerelemente.)

Der HTML-Code `<head>` Element dient als Repository für Informationen zu der Webseite-Dokument, das nicht Teil des Dokuments selbst ist. Hierzu gehören Informationen wie Titel der Webseite, Meta-Informationen von Suchmaschinen oder interne Crawler und Links zu externen Ressourcen, z. B. RSS-Feeds, JavaScript und CSS-Dateien verwendet. Einige dieser Informationen kann für alle Seiten der Website relevant sein. Beispielsweise empfiehlt es sich um die gleichen CSS-Regeln und JavaScript-Dateien für jede Seite ASP.NET Global zu importieren. Es gibt jedoch auch Teile der `<head>` -Element, das seitenspezifische sind. Titel der Seite ist ein hervorragendes Beispiel.

In diesem Tutorial untersuchen wir Gewusst wie: Definieren von globalen und seitenspezifische `<head>` Abschnitt Markup auf der Masterseite und auf die Inhaltsseiten.

## <a name="examining-the-master-pagesheadsection"></a>Untersuchen die Masterseite`<head>`Abschnitt

Die standardmäßigen Masterseitendatei von Visual Studio 2008 erstellt haben, enthält das folgende Markup in der `<head>` Abschnitt:


[!code-aspx[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/samples/sample1.aspx)]

Beachten Sie, dass die `<head>` Element enthält eine `runat="server"` -Attribut, das gibt an, dass es sich um ein Steuerelement (anstelle von statischem HTML-Code) ist. Alle ASP.NET-Seiten leiten Sie von der [ `Page` Klasse](https://msdn.microsoft.com/library/system.web.ui.page.aspx), befindet sich in der `System.Web.UI` Namespace. Diese Klasse enthält eine `Header` -Eigenschaft, die Zugriff auf der Seite ermöglicht `<head>` Region. Mithilfe der [ `Header` Eigenschaft](https://msdn.microsoft.com/library/system.web.ui.page.header.aspx) können wir eine ASP.NET-Seite Titel festlegen oder zusätzliches Markup hinzufügen, um den gerenderten `<head>` Abschnitt. Es ist möglich, klicken Sie dann zum Anpassen einer Inhaltsseite `<head>` Element durch das Schreiben von ein paar Codezeilen in der Seite `Page_Load` -Ereignishandler. Untersuchen wir zum programmgesteuerten der Titel der Seite in Schritt 1 festlegen.

Das Markup angezeigt, der `<head>` Element oben enthält auch ein ContentPlaceHolder-Steuerelement, das mit dem Namen Head. Dieses ContentPlaceHolder-Steuerelement ist nicht erforderlich, wie Inhaltsseiten benutzerdefinierten Inhalten hinzufügen, können die `<head>` Element programmgesteuert. Sie eignet sich jedoch in Situationen, in denen eine Inhaltsseite hinzufügen statisches Markup muss, das `<head>` -Element als static Markup deklarativ programmgesteuert, anstatt an das entsprechende Inhaltssteuerelement hinzugefügt werden können.

Zusätzlich zu den `<title>` -Element und Head ContentPlaceHolder, die Masterseite der `<head>` Element enthalten soll `<head>`-Level-Markup, das alle Seiten ist. Auf unserer Website alle Seiten der CSS-Regeln, die im Verwenden der `Styles.css` Datei. Daher wir aktualisiert die `<head>` Element in der [ *erstellen einen websiteweiten Layouts mit Masterseiten* ](creating-a-site-wide-layout-using-master-pages-cs.md) Lernprogramm aus, um die entsprechende include `<link>` Element. Unsere `Site.master` Masterseite des aktuellen `<head>` Markup wird unten angezeigt.


[!code-aspx[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/samples/sample2.aspx)]

## <a name="step-1-setting-a-content-pages-title"></a>Schritt 1: Festlegen einer Inhaltsseite Titel

Der Titel der Webseite wird angegeben, über die `<title>` Element. Es ist wichtig, jede Seitentitel auf einen geeigneten Wert festzulegen. Wenn eine Seite zu besuchen, wird dessen Titel in der Titelleiste des Browsers angezeigt. Darüber hinaus verwenden, wenn eine Seite als Lesezeichen markieren, Browser den Titel der Seite als vorgeschlagener Name für das Lesezeichen. Viele Suchmaschinen zeigen außerdem den Titel der Seite, beim Anzeigen von Suchergebnissen.

> [!NOTE]
> Standardmäßig legt Visual Studio die `<title>` Element auf der Masterseite "Unbenannte Seite". Auf ähnliche Weise ASP.NET-Seiten haben ihre `<title>` legen Sie auf "Unbenannte Seite" zu. Da sie leicht vergessen, den Titel der Seite auf einen geeigneten Wert festgelegt werden kann, gibt es viele Seiten über das Internet mit dem Titel "Unbenannte Seite" an. Durch das Durchsuchen von Google für Webseiten mit diesem Namen gibt ungefähr 2,460,000 Ergebnisse zurück. Doch selbst Microsoft bleibt anfällig für das Veröffentlichen von Webseiten mit dem Titel "Unbenannte Seite". Zum Zeitpunkt der Erstellung dieses Dokuments hat eine Google-Suche 236 solche Webseiten in der Domäne "Microsoft.com" gemeldet.


Eine ASP.NET-Seite kann den Titel in einem der folgenden Arten angeben:

- Platzieren Sie den Wert direkt in die `<title>` Element
- Mithilfe der `Title` -Attribut in der `<%@ Page %>` Richtlinie
- Programmgesteuertes Festlegen der Seite `Title` Eigenschaft mit Code wie `Page.Title="title"` oder `Page.Header.Title="title"`.

Seiten keinen Inhalt eine `<title>` Element, da es in die Masterseite definiert ist. Aus diesem Grund können Sie eine Inhaltsseite Titel festlegen können Sie entweder die `<%@ Page %>` -Direktive `Title` Attribut oder programmgesteuert festlegen.

### <a name="setting-the-pages-title-declaratively"></a>Durch das deklarative Festlegen der Titel der Seite

Eine Inhaltsseite Titel kann festgelegt werden, deklarativ über die `Title` Attribut der [ `<%@ Page %>` Richtlinie](https://msdn.microsoft.com/library/ydy4x04a.aspx). Diese Eigenschaft kann festgelegt werden, durch direktes Bearbeiten der `<%@ Page %>` -Anweisung oder über das Eigenschaftenfenster. Sehen wir uns auf beide Ansätze.

Suchen Sie aus der Datenquellensicht an, die `<%@ Page %>` -Direktive am oberen Rand der deklarativen Markup der Seite. Die `<%@ Page %>` -Direktive für `Default.aspx` folgt:


[!code-aspx[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/samples/sample3.aspx)]

Die `<%@ Page %>` Richtlinie gibt seitenspezifische Attribute, die von der Engine für ASP.NET beim Analysieren und Kompilieren die Seite verwendet. Dazu gehören die Masterseitendatei, den Speicherort der zugehörige Codedatei und den Titel, neben anderen Informationen.

Wenn Sie eine neue Inhaltsseite erstellen, Visual Studio legt, standardmäßig die `Title` -Attribut auf unbenannte Seite. Änderung `Default.aspx`des `Title` "Master-Seite-Lernprogramme"-Attribut auf"unbenannt", und zeigen Sie die Seite über einen Browser. Abbildung 1 zeigt die Titelleiste des Browsers, die angibt, den neue Seitentitel.


![Im Browsers auf die Titelleiste zeigt jetzt &quot;Master Seite Tutorials&quot; anstelle von &quot;unbenannte Seite&quot;](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/_static/image1.png)

**Abbildung 01**: im Browsers auf die Titelleiste zeigt jetzt "Master Seite Tutorials" anstelle von "Unbenannte Seite"


Der Titel der Seite kann auch im Eigenschaftenfenster festgelegt werden. Wählen Sie im Eigenschaftenfenster Dokument aus der Dropdown-Liste zum Laden der Seite auf die Eigenschaften, die enthält die `Title` Eigenschaft. Abbildung 2 zeigt das Fenster "Eigenschaften" nach `Title` auf "Master-Seite-Lernprogramme" festgelegt wurde.


![Sie können auch den Titel im Eigenschaftenfenster konfigurieren](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/_static/image2.png)

**Abbildung 02**: Sie können den Titel im Eigenschaftenfenster zu konfigurieren


### <a name="setting-the-pages-title-programmatically"></a>Programmgesteuertes Festlegen der Titel der Seite

Der Masterseite `<head runat="server">` Markup wird in übersetzt eine [ `HtmlHead` Klasse](https://msdn.microsoft.com/library/system.web.ui.htmlcontrols.htmlhead.aspx) Instanz, wenn Sie von der Engine ASP.NET die Seite gerendert wird. Die `HtmlHead` -Klasse verfügt über eine [ `Title` Eigenschaft](https://msdn.microsoft.com/library/system.web.ui.htmlcontrols.htmlhead.title.aspx) , dessen Wert sieht in der gerenderten `<title>` Element. Diese Eigenschaft ist über eine ASP.NET-Seite Code-Behind-Klasse über `Page.Header.Title`; dieselben Eigenschaft kann auch auf das über `Page.Title`.

Programmgesteuertes Festlegen der Titel der Seite zu üben, Navigieren zum die `About.aspx` Seite des Code-Behind-Klasse, und erstellen Sie einen Ereignishandler für die Seite `Load` Ereignis. Legen Sie dann, den Titel der Seite auf "Master-Seite-Tutorials:: zu:: *Datum*", wobei *Datum* ist das aktuelle Datum. Nach dem Hinzufügen dieses Codes Ihrer `Page_Load` Ereignishandler sollte etwa wie folgt aussehen:


[!code-csharp[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/samples/sample4.cs)]

Abbildung 3 zeigt die Titelleiste des Browsers, beim Zugriff auf die `About.aspx` Seite.


![Der Titel der Seite programmgesteuert festgelegt und enthält das aktuelle Datum](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/_static/image3.png)

**Abbildung 03**: der Seitentitel wird programmgesteuert festgelegt und enthält das aktuelle Datum


## <a name="step-2-automatically-assigning-a-page-title"></a>Schritt 2: Zuweisen automatisch dem Seitentitel

Wie in Schritt 1 beschrieben, kann der Titel einer Seite deklarativ oder programmgesteuert festgelegt werden. Wenn Sie vergessen, die explizit in einen aussagekräftigeren Titel ändern, jedoch haben die Seite den Standardtitel "Unbenannte Seite". Im Idealfall würde der Titel der Seite automatisch für uns festgelegt, wenn es nicht den Wert explizit angeben. Z. B. wenn der Titel der Seite zur Laufzeit "Unbenannte Seite" ist, möchten wir den Titel, die automatisch aktualisiert, um die ASP.NET-Seite Dateinamen identisch sein müssen. Die gute Nachricht ist, dass Sie mit ein wenig Vorarbeiten ist es möglich, dass den Titel, der automatisch zugewiesen.

Leiten Sie alle ASP.NET-Webseiten von der `Page` -Klasse in der `System.Web.UI` Namespace. Die `Page` Klasse definiert die minimale Funktionalität, die von einer ASP.NET-Seite erforderlich und grundlegende Eigenschaften wie `IsPostBack`, `IsValid`, `Request`, und `Response`, unter anderem. Jede Seite in einer Webanwendung ist häufig erforderlich, zusätzliche Features oder Funktionen. Eine gängige Methode für die Bereitstellung, dies ist zum Erstellen einer benutzerdefinierten Basisseite-Klasse. Eine benutzerdefinierte Basisseite-Klasse ist eine abgeleitete Klasse Sie erstellen die `Page` Klasse und zusätzliche Funktionen. Nach dieser Basisklasse erstellt wurde, können Sie ASP.NET-Seiten abgeleitet haben (anstelle der `Page` Klasse), wodurch die erweiterte Funktionen für ASP.NET-Seiten Angebot.

In diesem Schritt erstellen wir eine Basis-Seite, die den Titel der Seite auf der ASP.NET-Seite Dateinamen automatisch festgelegt, wenn der Titel andernfalls nicht explizit festgelegt wurde. Schritt 3 untersucht den Titel der Seite basierend auf der Sitemap festlegen.

> [!NOTE]
> Eine gründliche Untersuchung erstellen und Verwenden von benutzerdefinierten Basisseitenklassen sprengen den Rahmen dieser tutorialreihe ab. Weitere Informationen finden Sie [verwenden für Ihre ASP.NET-Seiten CodeBehind-Klassen eine Basisklasse für benutzerdefinierte](http://aspnet.4guysfromrolla.com/articles/041305-1.aspx).


### <a name="creating-the-base-page-class"></a>Erstellen der grundlegenden Seitenklasse

Unsere erste Aufgabe ist die Erstellung eine grundlegenden Seitenklasse, ist eine Klasse, die erweitert die `Page` Klasse. Starten Sie durch das Hinzufügen einer `App_Code` Ordner auf Ihrem Projekt, indem Sie mit der rechten Maustaste auf den Projektnamen im Projektmappen-Explorer, auswählen von ASP.NET-Ordner hinzufügen, und wählen dann `App_Code`. Als Nächstes mit der rechten Maustaste auf die `App_Code` Ordner, und fügen Sie eine neue Klasse mit dem Namen `BasePage.cs`. Abbildung 4 zeigt den Projektmappen-Explorer nach der `App_Code` Ordner und `BasePage.cs` Klasse hinzugefügt wurden.


![Fügen Sie einen Ordner "App_Code" und eine Klasse namens BasePage hinzu](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/_static/image4.png)

**Abbildung 04**: Hinzufügen einer `App_Code` Ordner und eine Klasse, die mit dem Namen `BasePage`


> [!NOTE]
> Visual Studio unterstützt zwei Modi des Projektmanagements: Websiteprojekten und Webanwendungsprojekten. Die `App_Code` Ordner mit dem Websiteprojekt-Modell verwendet werden soll. Wenn Sie das Webanwendungsprojekt-Modell verwenden, platzieren Sie die `BasePage.cs` Klasse in einen Ordner namens etwas anders als `App_Code`, z. B. `Classes`. Weitere Informationen zu diesem Thema finden Sie unter [Migrieren eines Websiteprojekts in ein Webanwendungsprojekt](http://webproject.scottgu.com/CSharp/Migration2/Migration2.aspx).


Da die benutzerdefinierte Basisseite als Basisklasse für die CodeBehind-Klassen ASP.NET-Seiten dient, muss zum Erweitern der `Page` Klasse.


[!code-csharp[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/samples/sample5.cs)]

Bei jeder Anforderung eine ASP.NET-Seite durchläuft es eine Reihe von Stufen, verbessert die angeforderte Seite in HTML gerendert wird. Wir nutzen eine Stufe durch Überschreiben der `Page` Klasse `OnEvent` Methode. Lassen Sie uns für unsere Basis Seite legen Sie den Titel automatisch, wenn er von nicht explizit angegeben wurde der `LoadComplete` Phase (die, wie Sie schon erraten haben vielleicht, tritt ein, nachdem die `Load` Phase).

Um dies zu erreichen, außer Kraft setzen der `OnLoadComplete` Methode, und geben Sie den folgenden Code:


[!code-csharp[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/samples/sample6.cs)]

Die `OnLoadComplete` Methode beginnt mit dem bestimmen, ob die `Title` Eigenschaft nicht noch explizit festgelegt wurde. Wenn die `Title` Eigenschaft `null`, eine leere Zeichenfolge oder hat den Wert "Unbenannte Seite", der Dateiname der angeforderten ASP.NET-Seite zugewiesen wird. Der physische Pfad der angeforderten ASP.NET-Seite - `C:\MySites\Tutorial03\Login.aspx`, z. B. - ist über die `Request.PhysicalPath` Eigenschaft. Die `Path.GetFileNameWithoutExtension` Methode wird verwendet, um nur den Dateinamen Teil herausziehen und der Dateiname wird anschließend zugewiesen der `Page.Title` Eigenschaft.

> [!NOTE]
> Ich lade Sie diese Logik zur Verbesserung der des Format des Titels zu erhöhen. Wenn die Seite der Dateiname ist z. B. `Company-Products.aspx`, der obige Code erzeugt den Titel "Unternehmens-Produkte", aber im Idealfall würde der Bindestrich mit einem Leerzeichen, wie in "Firmenprodukte" ersetzt. Berücksichtigen Sie auch ein Leerzeichen hinzugefügt, wenn die Groß-/Kleinschreibung geändert wird. D. h., erwägen Sie Code, der den Dateinamen transformiert `OurBusinessHours.aspx` zu einem Titel von "unsere Geschäftsstunden".


### <a name="having-the-content-pages-inherit-the-base-page-class"></a>Wenn die Inhaltsseiten der grundlegenden Seitenklasse erben

Wir müssen jetzt die ASP.NET-Seiten in unserer Website für die Ableitung von der benutzerdefinierten Basisseite aktualisieren (`BasePage`) anstelle der `Page` Klasse. Gehen Sie dazu zur einzelnen Code-Behind-Klasse erreichen, und ändern die Klassendeklaration aus:


[!code-csharp[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/samples/sample7.cs)]

Nach:


[!code-csharp[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/samples/sample8.cs)]

Nach dem auf diese Weise finden Sie auf der Website über einen Browser. Wenn Sie eine Seite besuchen, deren Titel explizit, wie z. B. festlegen ist `Default.aspx` oder `About.aspx`, der explizit angegebene Titel verwendet wird. Wenn Sie jedoch, Sie eine Seite besuchen, deren Titel nicht von der Standardeinstellung ("unbenannte Seite") geändert wurde, legt die grundlegenden Seitenklasse den Titel auf der Seite Dateinamen fest.

Abbildung 5 zeigt die `MultipleContentPlaceHolders.aspx` Seite, wenn Sie über einen Browser angezeigt. Beachten Sie, dass der Titel genau der Seite Dateinamen (nicht die Erweiterung), "MultipleContentPlaceHolders".


[![Ein Titel nicht explizit angegeben ist, ist der Seite Filename automatisch verwendet.](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/_static/image6.png)](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/_static/image5.png)

**Abbildung 05**: Wenn ein Titel nicht explizit angegeben ist, der Seite-Dateiname ist automatisch verwendet ([klicken Sie, um das Bild in voller Größe anzeigen](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/_static/image7.png))


## <a name="step-3-basing-the-page-title-on-the-site-map"></a>Schritt 3: Wenn dem Titel der Seite auf der Website-Karte

ASP.NET bietet ein stabile Site Map Framework Seitenentwickler, die zum Definieren von einer hierarchischen Siteübersicht in eine externe Ressource (z. B. einer XML-Datei oder Datenbank die Tabelle) zusammen mit der Web-Steuerelemente zum Anzeigen von Informationen über die Sitemap (z. B. die SiteMapPath Menü, und TreeView-Steuerelement).

Der Siteübersichtsstruktur kann auch programmgesteuert von einer ASP.NET-Seite CodeBehind-Klasse zugegriffen werden. Auf diese Weise können wir automatisch den Titel einer Seite, auf den Titel der entsprechenden Knoten in der Siteübersicht festlegen. Wir verbessern die `BasePage` Klasse, die in Schritt2 erstellt werden, sodass sie diese Funktionalität bietet. Doch zunächst müssen wir für unsere Website die Sitemap zu erstellen.

> [!NOTE]
> In diesem Tutorial wird davon ausgegangen, dass der Leser bereits mit ASP vertraut ist. NET Site Map-Features. Weitere Informationen zur Verwendung der Sitemap finden Sie in meiner mehrteiligen Artikelserie, [ASP untersuchen. NET Websitenavigation](http://aspnet.4guysfromrolla.com/articles/111605-1.aspx).


### <a name="creating-the-site-map"></a>Erstellen der Sitezuordnung

Das Standortsystem für die Zuordnung basiert auf der [Anbietermodell](http://aspnet.4guysfromrolla.com/articles/101905-1.aspx), die entkoppelt der Sitemap-API von der Logik, die Siteübersichtsinformationen zwischen Arbeitsspeicher und einem permanenten Speicher serialisiert. Im Lieferumfang von .NET Framework die [ `XmlSiteMapProvider` Klasse](https://msdn.microsoft.com/library/system.web.xmlsitemapprovider.aspx), dies ist der Standard-Siteübersichtsanbieter. Wie der Name schon sagt, `XmlSiteMapProvider` verwendet eine XML-Datei als Site Map Speicher. Verwenden wir diesen Anbieter zum Definieren von unserem Sitemap an.

Zunächst erstellen Sie eine Siteübersichtsdatei im mit dem Namen der Website-Stammordner `Web.sitemap`. Zu diesem Zweck mit der rechten Maustaste auf den Namen der Website im Projektmappen-Explorer, neues Element hinzufügen, und wählen Sie die Vorlage für die Siteübersicht. Stellen Sie sicher, dass die Datei heißt `Web.sitemap` , und klicken Sie auf Hinzufügen.


[![Fügen Sie die Datei Web.sitemap zum Stammordner der Website](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/_static/image9.png)](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/_static/image8.png)

**Abbildung 06**: Hinzufügen einer Datei mit dem Namen `Web.sitemap` zum Stammordner der Website ([klicken Sie, um das Bild in voller Größe anzeigen](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/_static/image10.png))


Hinzufügen der folgenden XML-Code der `Web.sitemap` Datei:


[!code-xml[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/samples/sample9.xml)]

Dieser XML-Code definiert die hierarchischen Siteübersichtsstruktur in Abbildung 7 dargestellt.


![Die Siteübersicht ist derzeit besteht aus der drei Siteübersichtsknoten](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/_static/image11.png)

**Abbildung 07**: das Site Map ist derzeit besteht aus der drei Siteübersichtsknoten


Der Siteübersichtsstruktur wird in zukünftigen Lernprogrammen aktualisiert, wenn wir neue Beispiele hinzufügen.

### <a name="updating-the-master-page-to-include-navigation-web-controls"></a>Aktualisieren die Master-Seite, um die Navigation Websteuerelemente

Nun, wir die Sitemap definiert haben, aktualisieren wir die Masterseite Navigation Websteuerelemente enthält. Insbesondere fügen Sie ein ListView-Steuerelement auf der linken Spalte in den Lektionen-Abschnitt, der eine ungeordnete Liste ein Listenelement für jeden Knoten in der Siteübersicht definierten rendert.

> [!NOTE]
> Das ListView-Steuerelement ist neu in ASP.NET Version 3.5. Wenn Sie eine frühere Version von ASP.NET verwenden, verwenden Sie stattdessen das Repeater-Steuerelement. Weitere Informationen für das ListView-Steuerelement finden Sie unter [ListView und DataPager-Steuerelemente mithilfe von ASP.NET 3.5](http://aspnet.4guysfromrolla.com/articles/122607-1.aspx).


Zunächst entfernen das vorhandene Markup für unsortierte Liste, aus dem Abschnitt Lektionen. Als Nächstes ein ListView-Steuerelement aus der Toolbox ziehen und legen Sie sie unterhalb der Lektionen Überschrift. Die ListView befindet sich im Datenabschnitt der Toolbox, zusammen mit den anderen Ansichtssteuerelementen: die GridView, DetailsView und FormView-Steuerelement. Legen Sie das ListView ID-Eigenschaft auf `LessonsList`.

Wählen Sie aus dem Konfigurations-Assistenten so binden Sie die ListView an ein neues SiteMapDataSource-Steuerelement, das mit dem Namen `LessonsDataSource`. Das Steuerelement SiteMapDataSource gibt die hierarchische Struktur vom Standortsystem Zuordnung zurück.


[![Binden eines SiteMapDataSource-Steuerelements an das LessonsList ListView-Steuerelement](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/_static/image13.png)](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/_static/image12.png)

**Abbildung 08**: Binden eines Steuerelements SiteMapDataSource, um die `LessonsList` ListView-Steuerelement ([klicken Sie, um das Bild in voller Größe anzeigen](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/_static/image14.png))


Nach dem Erstellen des Steuerelements SiteMapDataSource, müssen wir das ListView Vorlagen zu definieren, sodass er eine ungeordnete Liste ein Listenelement für jeden Knoten zurückgegeben, die von der SiteMapDataSource-Steuerelement wiedergegeben. Dies kann erreicht werden, verwenden das folgende Vorlagenmarkup:


[!code-aspx[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/samples/sample10.aspx)]

Die `LayoutTemplate` generiert das Markup für eine ungeordnete Liste (`<ul>...</ul>`) während der `ItemTemplate` rendert jedes Element, das von der SiteMapDataSource als ein Listenelement zurückgegeben (`<li>`), die einen Link zu der bestimmten Lektion enthält.

Besuchen Sie nach dem Konfigurieren der ListView Vorlagen, die Website aus. Wie in Abbildung 9 gezeigt, enthält der Abschnitt Lektionen ein einzelnes Element mit Aufzählungszeichen, Home. Wo befinden sich die Info "und" mehrere ContentPlaceHolder-Steuerelemente Lektionen verwenden? Die SiteMapDataSource soll einen hierarchischen Satz von Daten zurückgeben, aber das ListView-Steuerelement kann nur eine einzelne Ebene der Hierarchie angezeigt. Daher wird nur die erste Ebene der Siteübersichtsknoten, die von der SiteMapDataSource zurückgegeben.


[![Die Lektionen-Abschnitt enthält ein einziges Listenelement](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/_static/image16.png)](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/_static/image15.png)

**Abbildung 09**: der Lektionen-Abschnitt enthält ein einzelnes Element der Liste ([klicken Sie, um das Bild in voller Größe anzeigen](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/_static/image17.png))


Zum Anzeigen von mehreren Ebenen schachteln wir mehrere ListViews, die innerhalb der `ItemTemplate`. Diese Technik wurde untersucht, der [ *Masterseiten und Sitenavigation* Tutorial](../../data-access/introduction/master-pages-and-site-navigation-cs.md) von meiner [arbeiten mit Daten tutorialreihe](../../data-access/index.md). Für diese tutorialreihe unsere Sitemap enthält jedoch nur eine zwei Ebenen: Home (die oberste Ebene); und jede Lektion als untergeordnetes Element der Startseite. Anstatt das Erstellen einer geschachtelten ListView können wir stattdessen die SiteMapDataSource durch Festlegen den Startknoten zurückgegeben anweisen seine [ `ShowStartingNode` Eigenschaft](https://msdn.microsoft.com/library/system.web.ui.webcontrols.sitemapdatasource.showstartingnode.aspx) zu `false`. Das Endergebnis ist, dass die SiteMapDataSource gestartet wird, durch die zweite Ebene der Siteübersichtsknoten zurückgeben.

Durch diese Änderung die ListView Zeigt Elemente an Aufzählungszeichen für About und mehrere ContentPlaceHolder-Steuerelemente mithilfe von Lektionen, lässt aber Element einer Aufzählung für zuhause. Um dies zu beheben, können wir explizit Element einer Aufzählung für privaten Bereich im Hinzufügen der `LayoutTemplate`:


[!code-aspx[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/samples/sample11.aspx)]

Konfigurieren die SiteMapDataSource, um dem Startknoten auszulassen und explizite hinzufügen eine Startseite Aufzählungspunkt zeigt die Lektionen im Abschnitt nun die beabsichtigte Ausgabe.


[![Der Lektionen-Abschnitt enthält eine Aufzählungspunkt für Privatanwender und jeden untergeordneten Knoten](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/_static/image19.png)](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/_static/image18.png)

**Abbildung 10**: der Lektionen-Abschnitt enthält eine Aufzählungspunkt für Privatanwender und jeder untergeordnete Knoten ([klicken Sie, um das Bild in voller Größe anzeigen](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/_static/image20.png))


### <a name="setting-the-title-based-on-the-site-map"></a>Festlegen des Titels, der basierend auf der Sitemap

Mit der Siteübersicht vorhanden, können wir aktualisieren unsere `BasePage` Klasse, die den Titel in der Siteübersicht angegeben verwendet. Wie in Schritt2, möchten wir nur der Siteübersichtsknoten Titel verwenden, wenn der Titel der Seite durch den Seitenentwickler nicht explizit festgelegt wurde. Wenn die angeforderte Seite nicht explizit festgelegt verfügt Seitentitel und ist nicht in der Siteübersicht gefunden, und klicken Sie dann wir ausweichen werden auf die angeforderte Seite Dateinamen (nicht die Erweiterung), wie in Schritt2. Dieser Entscheidungsprozess wird in Abbildung 11 dargestellt.


![In Ermangelung einer explizit Seitentitel festlegen wird der entsprechende Siteübersichtsknoten des Titels verwendet.](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/_static/image21.png)

**Abbildung 11**: In Abwesenheit einer explizit Seitentitel festlegen, wird der entsprechende Siteübersichtsknoten des Titels verwendet


Update der `BasePage` Klasse `OnLoadComplete` Methode, um den folgenden Code einfügen:


[!code-csharp[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/samples/sample12.cs)]

Wie zuvor die `OnLoadComplete` Methode beginnt mit dem bestimmt wird, ob der Titel der Seite explizit festgelegt wurde. Wenn `Page.Title` ist `null`, eine leere Zeichenfolge oder den Wert "Unbenannte Seite" zugewiesen ist, und klicken Sie dann der Code automatisch einen Wert zuweist `Page.Title`.

Zum Bestimmen des Titels, der Code beginnt, durch Verweisen auf die [ `SiteMap` Klasse](https://msdn.microsoft.com/library/system.web.sitemap.aspx)des [ `CurrentNode` Eigenschaft](https://msdn.microsoft.com/library/system.web.sitemap.currentnode.aspx). `CurrentNode` Gibt die [ `SiteMapNode` ](https://msdn.microsoft.com/library/system.web.sitemapnode.aspx) Instanz in der Siteübersicht, das die gerade angeforderte Seite entspricht. Vorausgesetzt, die gerade angeforderte Seite befindet sich innerhalb der sitezuordnung das `SiteMapNode`des `Title` der Titel der Seite Eigenschaft zugewiesen wird. Ist die gerade angeforderte Seite nicht in der Siteübersicht `CurrentNode` gibt `null` und Dateinamen für die angeforderte Seite wird als Titel verwendet (wie in Schritt2 durchgeführt wurde).

Abbildung 12 zeigt die `MultipleContentPlaceHolders.aspx` Seite, wenn Sie über einen Browser angezeigt. Da der Titel dieser Seite nicht explizit festgelegt ist, wird dessen entsprechender Siteübersichtsknoten des Titels stattdessen verwendet.


![MultipleContentPlaceHolders.aspx der Titel der Seite wird von der Sitezuordnung abgerufen.](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/_static/image22.png)

**Abbildung 12**: die `MultipleContentPlaceHolders.aspx` Titel der Seite von der Sitezuordnung per Pull abgerufen wird


## <a name="step-4-adding-other-page-specific-markup-to-theheadsection"></a>Schritt 4: Hinzufügen anderer seitenspezifische Markup der`<head>`Abschnitt

Erläutert die Schritte 1, 2 und 3 Anpassen der `<title>` Element auf einer Seite von Seite-Basis. Zusätzlich zu `<title>`, `<head>` Abschnitt darf `<meta>` Elemente und `<link>` Elemente. Wie weiter oben in diesem Tutorial erwähnt `Site.master`des `<head>` Abschnitt enthält eine `<link>` Element `Styles.css`. Da dies `<link>` Element innerhalb der Masterseite definiert ist, befindet sich in der `<head>` Abschnitt für alle Inhaltsseiten. Aber wie gehen wir zum Hinzufügen von `<meta>` und `<link>` Elemente pro Seite von Seite?

Die einfachste Möglichkeit zum seitenspezifische Inhalte zum Hinzufügen der `<head>` Abschnitt besteht ein ContentPlaceHolder-Steuerelement auf der Masterseite zu erstellen. Wir haben bereits solche ein ContentPlaceHolder-Objekt (mit dem Namen `head`). Aus diesem Grund zum Hinzufügen benutzerdefinierter `<head>` Markup, erstellen Sie ein entsprechendes Steuerelement auf der Seite Inhalt, und platzieren Sie das Markup vorhanden.

Hinzufügen von benutzerdefinierten veranschaulichen `<head>` Markup lassen Sie uns auf die Seite umfassen eine `<meta>` unserer aktuellen Satz von Inhaltsseiten Description-Element. Die `<meta>` Description-Element bietet eine kurze Beschreibung bezüglich der Webseite, wie die meisten Suchmaschinen integrieren Sie diese Informationen in irgendeiner Form, bei der Anzeige von Suchergebnissen.

Ein `<meta>` Description-Element weist folgende Form:


[!code-html[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/samples/sample13.html)]

Um dieses Markup eine Inhaltsseite hinzuzufügen, fügen Sie den oben genannten Text, an das Inhaltssteuerelement, das die Masterseite Head ContentPlaceHolder zugeordnet. Um beispielsweise Definieren eine `<meta>` Description-Element für `Default.aspx`, fügen Sie das folgende Markup hinzu:


[!code-aspx[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/samples/sample14.aspx)]

Da das Anfangselement ContentPlaceHolder nicht innerhalb der HTML-Seite ist, wird das Markup, das an das Inhaltssteuerelement hinzugefügt, nicht in der Entwurfsansicht angezeigt. Anzeigen der `<meta>` Description-Element finden Sie unter `Default.aspx` über einen Browser. Nachdem die Seite geladen wurde, zeigen Sie die Quelle, und beachten Sie, dass die `<head>` Abschnitt enthält das Markup, das in das Inhaltssteuerelement angegeben.

Hinzufügen in Ruhe `<meta>` Beschreibungselemente `About.aspx`, `MultipleContentPlaceHolders.aspx`, und `Login.aspx`.

### <a name="programmatically-adding-markup-to-theheadregion"></a>Programmgesteuertes Hinzufügen von Markup für die`<head>`Region

Die Kopfzeile ContentPlaceHolder kann deklarativ hinzufügen, benutzerdefiniertes Markup auf der Masterseite `<head>` Region. Benutzerdefiniertes Markup kann auch programmgesteuert hinzugefügt werden. Bedenken Sie, dass die `Page` -Klasse `Header` -Eigenschaft gibt die `HtmlHead` Instanz definiert, die auf der Masterseite (die `<head runat="server">`).

Können Sie programmgesteuert hinzufügen von Inhalt an die `<head>` Region ist nützlich, wenn der Inhalt hinzuzufügenden dynamisch ist. Vielleicht basiert sie auf der Benutzer Zugriff auf der Seite "; Vielleicht ist es aus einer Datenbank abgerufen werden. Ungeachtet der Ursache können Sie Inhalte zum Hinzufügen der `HtmlHead` durch Hinzufügen von Steuerelementen zu ihrer Auflistung wie folgt:


[!code-csharp[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/samples/sample15.cs)]

Der obige Code fügt die `<meta>` Keywords-Element, um die `<head>` Region, die eine durch Trennzeichen getrennte Liste mit Schlüsselwörtern bereitstellt, die die Seite zu beschreiben. Beachten Sie, dass Hinzufügen einer `<meta>` Tag, die Sie erstellen eine [ `HtmlMeta` ](https://msdn.microsoft.com/library/system.web.ui.htmlcontrols.htmlmeta.aspx) Instanz, legen die `Name` und `Content` Eigenschaften, und fügen Sie es auf die `Header`des `Controls` Auflistung. Auf ähnliche Weise das programmgesteuerte Hinzufügen eine `<link>` -Element, erstellen eine [ `HtmlLink` ](https://msdn.microsoft.com/library/system.web.ui.htmlcontrols.htmllink.aspx) Objekt, seine Eigenschaften festlegen, und fügen Sie es auf die `Header`des `Controls` Auflistung.

> [!NOTE]
> Um beliebige Markup hinzuzufügen, erstellen eine [ `LiteralControl` ](https://msdn.microsoft.com/library/system.web.ui.literalcontrol.aspx) Instanz, legen die `Text` -Eigenschaft, und fügen Sie es auf die `Header`des `Controls` Auflistung.


## <a name="summary"></a>Zusammenfassung

In diesem Tutorial erläutert, eine Vielzahl von Möglichkeiten zum Hinzufügen von `<head>` Region Markup pro Seite von Seite. Eine Masterseite aufzunehmen ein `HtmlHead` Instanz (`<head runat="server">`) mit ein ContentPlaceHolder-Objekt. Die `HtmlHead` Instanz ermöglicht den programmgesteuerten Zugriff auf die Inhaltsseiten der `<head>` Region und deklarativ und programmgesteuert über die Seite festlegen des Titels, des ContentPlaceHolder-Steuerelements ermöglicht, benutzerdefinierte Markup hinzugefügt werden die `<head>` Abschnitt deklarativ über die ein ContentControl-Element.

Viel Spaß beim Programmieren!

### <a name="further-reading"></a>Weiterführende Themen

Weitere Informationen zu den Themen in diesem Tutorial erläutert finden Sie in den folgenden Ressourcen:

- [Der Titel der Seite festzulegen dynamisch, in ASP.NET](http://aspnet.4guysfromrolla.com/articles/051006-1.aspx)
- [Untersuchen ASP. NET Website-Navigation](http://aspnet.4guysfromrolla.com/articles/111605-1.aspx)
- [Gewusst wie: Verwenden von HTML-Meta-Tags](http://searchenginewatch.com/showPage.html?page=2167931)
- [Masterseiten in ASP.NET](http://www.odetocode.com/articles/419.aspx)
- [Mithilfe von ASP.NET 3,5 ListView "und" DataPager-Steuerelemente](http://aspnet.4guysfromrolla.com/articles/122607-1.aspx)
- [Verwenden eine benutzerdefinierte Basisklasse für Ihre ASP.NET-Seiten CodeBehind-Klassen](http://aspnet.4guysfromrolla.com/articles/041305-1.aspx)

### <a name="about-the-author"></a>Der Autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor mehrerer Büchern zu ASP/ASP.NET und Gründer von 4GuysFromRolla.com, arbeitet mit Microsoft-Web-Technologien seit 1998. Er ist als ein unabhängiger Berater, Schulungsleiter und Autor. Sein neuestes Buch heißt [ *Sams Teach selbst ASP.NET 3.5 in 24 Stunden*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Scott erreicht werden kann, zur [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com) oder über seinen Blog unter [http://ScottOnWriting.NET](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Besonderen Dank an

Diese tutorialreihe wurde durch viele hilfreiche Reviewer überprüft. Führendes Prüfer für dieses Tutorial wurden Zack Jones und Suchi Banerjee. Meine zukünftigen MSDN-Artikeln überprüfen möchten? Wenn dies der Fall ist, löschen Sie mir eine Linie an [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com).

> [!div class="step-by-step"]
> [Zurück](multiple-contentplaceholders-and-default-content-cs.md)
> [Weiter](urls-in-master-pages-cs.md)
