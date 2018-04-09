---
uid: web-forms/overview/older-versions-getting-started/master-pages/specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs
title: Angeben der Titel, Meta-Tags und andere HTML-Header in der Gestaltungsvorlage (c#) | Microsoft Docs
author: rick-anderson
description: Prüft auf verschiedene Techniken zum Definieren der ausgewählten &lt;Head&gt; Elemente in der Master-Seite von der Seite Inhalt.
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/21/2008
ms.topic: article
ms.assetid: 0aa1c84f-c9e2-4699-b009-0e28643ecbc6
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/master-pages/specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs
msc.type: authoredcontent
ms.openlocfilehash: 2f9399be5b95f608f0d635b69b132dcb1d27909a
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
---
<a name="specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-c"></a>Angeben der Titel, Meta-Tags und andere HTML-Header in der Gestaltungsvorlage (c#)
====================
durch [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Herunterladen von Code](http://download.microsoft.com/download/e/e/f/eef369f5-743a-4a52-908f-b6532c4ce0a4/ASPNET_MasterPages_Tutorial_03_CS.zip) oder [PDF herunterladen](http://download.microsoft.com/download/8/f/6/8f6349e4-6554-405a-bcd7-9b094ba5089a/ASPNET_MasterPages_Tutorial_03_CS.pdf)

> Prüft auf verschiedene Techniken zum Definieren der ausgewählten &lt;Head&gt; Elemente in der Master-Seite von der Seite Inhalt.


## <a name="introduction"></a>Einführung

Neue Masterseiten in Visual Studio 2008 erstellt haben, werden standardmäßig zwei ContentPlaceHolder-Steuerelemente: eines mit dem Namen Head und befindet sich in der `<head>` -Element ist; und dasjenige mit dem Namen `ContentPlaceHolder1`, in das Web Form platziert. Der Zweck der `ContentPlaceHolder1` besteht darin, einen Bereich in das Web Form definieren, die auf Basis von Seite angepasst werden kann. Die `head` ContentPlaceHolder ermöglicht Seiten zum Hinzufügen von benutzerdefinierten Inhalten zu den `<head>` Abschnitt. (Natürlich diese zwei ContentPlaceHolders geändert oder entfernt werden können, und der Gestaltungsvorlage möglicherweise zusätzliche ContentPlaceHolder hinzugefügt werden. Unsere Masterseite `Site.master`, derzeit verfügt über vier ContentPlaceHolder-Steuerelemente.)

Der HTML-Code `<head>` Element dient als Repository für Informationen über die Webseite-Dokument, das nicht Teil des Dokuments selbst ist. Hierzu gehören Informationen wie der Titel der Webseite, Meta-Informationen von Suchmaschinen oder interne Suchmaschinen und Links zu externen Ressourcen, z. B. RSS-Feeds, JavaScript und CSS-Dateien verwendet. Einige dieser Informationen können für alle Seiten der Website sein. Sie möchten z. B. die gleichen CSS-Regeln und JavaScript-Dateien für jede ASP.NET-Seite Global zu importieren. Es gibt jedoch Teile der `<head>` Element, das speziell für Codepage sind. Der Seitenname ist ein gutes Beispiel.

In diesem Lernprogramm untersuchen wir zum Definieren von globalen und seitenspezifische `<head>` Abschnitt Markup in der Masterseite und seine Inhaltsseiten.

## <a name="examining-the-master-pagesheadsection"></a>Untersuchen die Gestaltungsvorlage`<head>`Abschnitt

Die Standardeinstellung Masterseitendatei von Visual Studio 2008 erstellte enthält das folgende Markup in seiner `<head>` Abschnitt:


[!code-aspx[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/samples/sample1.aspx)]

Beachten Sie, dass die `<head>` Element enthält eine `runat="server"` -Attribut, das angibt, dass es ein Webserversteuerelement (anstelle von statischem HTML-Code). Alle ASP.NET-Seiten Ableiten der [ `Page` Klasse](https://msdn.microsoft.com/library/system.web.ui.page.aspx), befindet sich der `System.Web.UI` Namespace. Diese Klasse enthält eine `Header` -Eigenschaft, die Zugriff auf der Seite bietet `<head>` Region. Mithilfe der [ `Header` Eigenschaft](https://msdn.microsoft.com/library/system.web.ui.page.header.aspx) können wir einer ASP.NET-Seite Titel festlegen, oder fügen Sie zusätzliches Markup, das gerenderte `<head>` Abschnitt. Es ist möglich, klicken Sie dann zum Anpassen einer Inhaltsseite `<head>` Element durch das Schreiben von viel Code auf der Seite `Page_Load` -Ereignishandler. Untersuchen wir wie Titel der Seite programmgesteuert in Schritt 1 festgelegt.

Das Markup angezeigt, der `<head>` Element oben enthält auch ein ContentPlaceHolder-Steuerelement, das mit dem Namen Head. Dieses ContentPlaceHolder-Steuerelement ist nicht erforderlich, wie Inhaltsseiten benutzerdefinierten Inhalten hinzufügen, können die `<head>` Element programmgesteuert. Es ist hilfreich in Situationen, in denen Inhaltsseite statische Markup hinzufügen muss, jedoch die `<head>` -Element als statische Markup deklarativ programmgesteuert, statt an das entsprechende Inhaltssteuerelement hinzugefügt werden können.

Zusätzlich zu den `<title>` Element und Head ContentPlaceHolder, der Gestaltungsvorlage des `<head>` Element sollte keines `<head>`-Ebene Markup, das für alle Seiten gemeinsam ist. Verwenden Sie die CSS-Regeln, die im auf unserer Website alle Seiten der `Styles.css` Datei. Daher wir aktualisiert die `<head>` Element in der [ *erstellen ein Layout standortweite mit Masterseiten für* ](creating-a-site-wide-layout-using-master-pages-cs.md) Lernprogramm aus, um ein entsprechendes enthalten `<link>` Element. Unsere `Site.master` Masterseite die aktuelle `<head>` Markup wird unten gezeigt.


[!code-aspx[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/samples/sample2.aspx)]

## <a name="step-1-setting-a-content-pages-title"></a>Schritt 1: Festlegen einer Inhaltsseite Titel

Der Titel der Webseite wird angegeben, über die `<title>` Element. Es ist wichtig, jede Seitentitel auf einen geeigneten Wert festzulegen. Beim Zugriff auf eine Seite, wird der Titel in der Titelleiste des Browsers angezeigt. Darüber hinaus verwenden, wenn eine Seite zu merken, Browsern Titel der Seite als vorgeschlagener Name für das Lesezeichen. Viele Suchmaschinen zeigt darüber an, den Titel der Seite Suchergebnisse angezeigt.

> [!NOTE]
> Standardmäßig legt Visual Studio die `<title>` Element in die Masterseite auf der Seite"unbenannt". Auf ähnliche Weise neue ASP.NET-Seiten haben ihre `<title>` legen Sie auf "Neue Seite" zu. Da es leicht vergessen, den Titel der Seite auf einen geeigneten Wert festgelegt werden kann, sind viele Seiten, auf das Internet mit dem Titel "Seite" unbenannt "". Durch das Durchsuchen von Google für Webseiten mit diesem Titel gibt ungefähr 2,460,000 Ergebnisse zurück. Auch Microsoft ist anfällig für das Veröffentlichen von Webseiten mit dem Titel "Seite" unbenannt "". Zum Zeitpunkt der Erstellung dieses Dokuments wurde eine Google-Suche 236 solche Webseiten in der Domäne "Microsoft.com" gemeldet.


Eine ASP.NET-Seite kann den Titel in einem der folgenden Arten angeben:

- Durch Speichern des Werts, der direkt in die `<title>` Element
- Mithilfe der `Title` Attribut in der `<%@ Page %>` Richtlinie
- Programmgesteuertes Festlegen der Seite `Title` Eigenschaft, die Verwendung eines Codes `Page.Title="title"` oder `Page.Header.Title="title"`.

Inhalt keine Seiten eine `<title>` -Funktionselement zu, wie es in die Masterseite definiert ist. Aus diesem Grund zum Festlegen des Titels einer Inhaltsseite können Sie entweder die `<%@ Page %>` Richtlinie `Title` Attribut oder programmgesteuert festlegen.

### <a name="setting-the-pages-title-declaratively"></a>Titel der Seite festlegen deklarativ

Eine Inhaltsseite Titel kann festgelegt werden, deklarativ über die `Title` Attribut von der [ `<%@ Page %>` Richtlinie](https://msdn.microsoft.com/library/ydy4x04a.aspx). Diese Eigenschaft kann festgelegt werden, durch direkte Modifizierung der `<%@ Page %>` Richtlinie oder über das Fenster "Eigenschaften". Betrachten Sie beide Ansätze aus.

Suchen Sie aus der Datenquellensicht an, die `<%@ Page %>` -Direktive, die am oberen Rand deklarativem Markup der Seite ist. Die `<%@ Page %>` für die Richtlinie `Default.aspx` folgt:


[!code-aspx[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/samples/sample3.aspx)]

Die `<%@ Page %>` -Direktive angibt, seitenspezifische Attribute, die durch das Modul ASP.NET beim Analysieren und Kompilieren die Seite verwendet. Dies schließt die Masterseitendatei, den Speicherort von der Codedatei und den Titel, neben anderen Informationen.

Wenn eine neue Seite erstellen, Visual Studio legt, standardmäßig die `Title` -Attribut auf der Seite "unbenannt". Änderung `Default.aspx`des `Title` -Attribut aus der Seite"unbenannt" auf "Master-Seite-Lernprogramme", und zeigen Sie dann auf die Seite über einen Browser. Abbildung 1 zeigt die Titelleiste des Browsers, was den neue Seitentitel widerspiegelt.


![Im Browsers auf die Titelleiste zeigt jetzt &quot;Master Seite Lernprogramme&quot; anstelle von &quot;Seite "unbenannt"&quot;](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/_static/image1.png)

**Abbildung 01**: im Browsers auf die Titelleiste zeigt jetzt "Master Seite Lernprogramme" anstelle von "Seite"unbenannt""


Titel der Seite kann auch im Eigenschaftenfenster festgelegt werden. Wählen Sie im Fenster Eigenschaften Dokument aus der Dropdown-Liste zum Laden der Seitenebene-Eigenschaften, die `Title` Eigenschaft. Abbildung 2 zeigt im Eigenschaftenfenster nach `Title` auf "Master-Seite-Lernprogramme" festgelegt wurde.


![Sie können den Titel aus dem Eigenschaftenfenster zu konfigurieren](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/_static/image2.png)

**Abbildung 02**: Sie können den Titel aus dem Eigenschaftenfenster zu konfigurieren


### <a name="setting-the-pages-title-programmatically"></a>Programmgesteuertes Festlegen der Titel der Seite

Der Gestaltungsvorlage `<head runat="server">` Markup übersetzt in ein [ `HtmlHead` Klasse](https://msdn.microsoft.com/library/system.web.ui.htmlcontrols.htmlhead.aspx) -Instanz auf, wenn die Seite vom Modul ASP.NET gerendert wird. Die `HtmlHead` -Klasse verfügt über eine [ `Title` Eigenschaft](https://msdn.microsoft.com/library/system.web.ui.htmlcontrols.htmlhead.title.aspx) wiedergegeben, deren Wert in der gerenderten `<title>` Element. Diese Eigenschaft wird von einer ASP.NET-Seite Code-Behind-Klasse über zugänglich `Page.Header.Title`; diesem dieselbe Eigenschaft kann auch über zugegriffen werden `Page.Title`.

Üben Sie die Titel der Seite programmgesteuert festzulegen, navigieren zu der `About.aspx` Seite des Code-Behind-Klasse, und erstellen Sie einen Ereignishandler für der Seite `Load` Ereignis. Legen Sie anschließend den Titel der Seite auf "Master Seite Lernprogramme:: zu:: *Datum*", wobei *Datum* ist das aktuelle Datum. Nach dem Hinzufügen dieses Codes Ihrer `Page_Load` Ereignishandler sollte etwa wie folgt aussehen:


[!code-csharp[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/samples/sample4.cs)]

Abbildung 3 zeigt die Konfigurationsbrowser, Startseite, beim Zugriff auf die `About.aspx` Seite.


![Titel der Seite wird programmgesteuert festgelegt und das aktuelle Datum enthält](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/_static/image3.png)

**Abbildung 03**: der Seitentitel programmgesteuert festgelegt wird, und schließt das aktuelle Datum


## <a name="step-2-automatically-assigning-a-page-title"></a>Schritt 2: Zuweisen automatisch eine Seitentitel

Wie in Schritt 1 veranschaulichte Filtermenü, kann eine Seite Titel deklarativ oder programmgesteuert festgelegt werden. Wenn Sie vergessen, die explizit auf den Titel in einen aussagekräftigeren Namen ändern, wird jedoch Ihre Seite den Standardtitel "Seite unbenannt" haben. Im Idealfall würden den Titel der Seite automatisch für uns festgelegt, wenn wir den Wert explizit angeben, nicht. Wenn zur Laufzeit den Titel der Seite "unbenannt" befindet, sollten wir z. B. den Titel automatisch aktualisiert, um die ASP.NET-Seite Dateinamen identisch sein müssen. Die gute Nachricht ist, dass Sie mit ein wenig Vorarbeiten ist es möglich, dass den Titel, der automatisch zugewiesen.

Leiten Sie alle ASP.NET-Webseiten aus der `Page` -Klasse in der `System.Web.UI` Namespace. Die `Page` Klasse definiert die grundlegenden Funktionen von einer ASP.NET-Seite benötigt und wichtige Eigenschaften wie `IsPostBack`, `IsValid`, `Request`, und `Response`, u. a. viele. Häufig, erfordert jede Seite in einer Webanwendung zusätzliche Features oder Funktionen. Eine gängige Methode bereitstellen, dies ist zum Erstellen einer benutzerdefinierten Basisseite-Klasse. Eine benutzerdefinierte Basisseite-Klasse ist eine Klasse erstellen, abgeleitet wird, die `Page` -Klasse und weitere Funktionen enthält. Nachdem diese Basisklasse erstellt wurde, haben Sie können sich davon Herleiten ASP.NET-Seiten (statt über das `Page` Klasse), wodurch die erweiterte Funktionalität zu ASP.NET-Seiten anbietet.

In diesem Schritt erstellen Sie eine Basis-Seite, die Titel der Seite in der ASP.NET-Seite Filename automatisch festgelegt, wenn der Titel andernfalls nicht explizit festgelegt wurde. Schritt 3 prüft die basierend auf der Siteübersicht Seitentitel festlegen.

> [!NOTE]
> Eine gründliche Prüfung erstellen und Verwenden von benutzerdefinierten Basisseitenklassen ist nicht Gegenstand dieser Reihe von Lernprogrammen. Weitere Informationen finden Sie unter [verwenden für Ihre ASP.NET-Seiten Code-Behind-Klassen eine Basisklasse für benutzerdefinierte](http://aspnet.4guysfromrolla.com/articles/041305-1.aspx).


### <a name="creating-the-base-page-class"></a>Erstellen die Seite-Basisklasse

Unsere erste Aufgabe ist die Erstellung eine Klasse Basisseite ist eine Klasse, die erweitert die `Page` Klasse. Starten Sie durch Hinzufügen einer `App_Code` Ordner zu Ihrem Projekt, indem mit der rechten Maustaste auf den Projektnamen im Projektmappen-Explorer auswählen, ASP.NET-Ordner hinzufügen, und wählen Sie dann `App_Code`. Als Nächstes mit der rechten Maustaste auf die `App_Code` Ordner, und fügen Sie eine neue Klasse mit dem Namen `BasePage.cs`. Abbildung 4 zeigt der Projektmappen-Explorer nach dem `App_Code` Ordner und `BasePage.cs` Klasse hinzugefügt wurden.


![Fügen Sie einen Ordner "App_Code" und eine Klasse namens BasePage hinzu](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/_static/image4.png)

**Abbildung 04**: Hinzufügen einer `App_Code` Ordner und eine Klasse namens `BasePage`


> [!NOTE]
> Visual Studio unterstützt zwei Bereitstellungsmodi Projektmanagement: Websiteprojekte und Webanwendungsprojekte. Die `App_Code` Ordner mit dem Websiteprojekt-Modell verwendet werden soll. Wenn Sie das Webanwendungsprojekt-Modell verwenden, platzieren Sie die `BasePage.cs` Klasse in einem Ordner namens etwas anders als `App_Code`, wie z. B. `Classes`. Weitere Informationen zu diesem Thema finden Sie unter [migrieren eine Website-Projekt zu einem Webanwendungsprojekt](http://webproject.scottgu.com/CSharp/Migration2/Migration2.aspx).


Da benutzerdefinierte Basisseite als Basisklasse für die Code-Behind-Klassen ASP.NET-Seiten dient, muss Sie zum Erweitern der `Page` Klasse.


[!code-csharp[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/samples/sample5.cs)]

Wenn eine ASP.NET-Seite angefordert wird, wird er durch eine Reihe von Phasen, mit der angeforderten Seite in HTML gerendert wird fortgesetzt. Wir können in eine Stufe tippen, durch Überschreiben der `Page` Klasse `OnEvent` Methode. Wir für unsere Basis Seite automatisch festlegen des Titels, wenn er vom nicht explizit angegeben wurde die `LoadComplete` Phase (die, wie Sie vielleicht, tritt ein, nachdem die `Load` Stufe).

Um dies zu erreichen, überschreiben die `OnLoadComplete` Methode, und geben Sie den folgenden Code:


[!code-csharp[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/samples/sample6.cs)]

Die `OnLoadComplete` Methode startet, indem Sie bestimmen, ob die `Title` Eigenschaft nicht noch explizit festgelegt wurde. Wenn die `Title` Eigenschaft ist `null`, eine leere Zeichenfolge oder hat den Wert "Unbenannt Seite" wird der Dateiname der angeforderten ASP.NET-Seite zugewiesen. Der physische Pfad auf die angeforderte ASP.NET-Seite - `C:\MySites\Tutorial03\Login.aspx`, befindet sich z. B. - Zugriff über die `Request.PhysicalPath` Eigenschaft. Die `Path.GetFileNameWithoutExtension` Methode wird verwendet, um nur die Dateinamen Teil herausziehen und der Dateiname wird anschließend zugewiesen der `Page.Title` Eigenschaft.

> [!NOTE]
> Ich Laden Sie diese Logik zur Verbesserung der des Format des Titels zu verbessern. Beispielsweise ist der Dateiname der Seite `Company-Products.aspx`, der obige Code wird den Titel "Firmenprodukte" erzeugen, aber im Idealfall würde der Bindestrich mit einem Leerzeichen, wie in "Firmenprodukte" ersetzt. Berücksichtigen Sie auch ein Leerzeichen hinzugefügt, sobald sich eine Änderung der Groß-/Kleinschreibung. Fügen Code, der den Dateinamen transformiert, also Sie ggf. `OurBusinessHours.aspx` einen beliebigen Titel von "Unsere Geschäftszeiten einrichten".


### <a name="having-the-content-pages-inherit-the-base-page-class"></a>Der Inhalt Seiten die Seite Basisklasse erben.

Jetzt müssen wir die ASP.NET-Seiten in unserer Website zum Ableiten von der benutzerdefinierten Basisseite aktualisieren (`BasePage`) anstelle der `Page` Klasse. Gehen Sie dazu auf jedes Code-Behind-Klasse zu erreichen, und ändern die Deklaration der Klasse aus:


[!code-csharp[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/samples/sample7.cs)]

Nach:


[!code-csharp[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/samples/sample8.cs)]

Suchen Sie nach der Anmeldung die Website über einen Browser aus. Wenn Sie eine Website besuchen, deren Titel explizit, wie z. B. festlegen wird `Default.aspx` oder `About.aspx`, der explizit angegebene Titel verwendet wird. Wenn allerdings Sie eine Website besuchen, deren Titel von der Standardseite ("unbenannt") nicht geändert wurde, legt die Basisseite-Klasse den Titel in Filename den Anzeigezustand der Seite fest.

Abbildung 5 zeigt die `MultipleContentPlaceHolders.aspx` Seite, wenn Sie über einen Browser angezeigt. Beachten Sie, dass der Titel genau den Anzeigezustand der Seite Dateiname (abzüglich der Erweiterung) ist "MultipleContentPlaceHolders".


[![Wenn ein Titel nicht explizit angegeben ist, ist der Seite Dateiname automatisch verwendet.](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/_static/image6.png)](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/_static/image5.png)

**Abbildung 05**: Wenn ein Titel nicht explizit angegeben ist, die Seite Dateiname ist automatisch verwendet ([klicken Sie hier, um das Bild in voller Größe angezeigt](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/_static/image7.png))


## <a name="step-3-basing-the-page-title-on-the-site-map"></a>Schritt 3: Anhand der Siteübersicht Seitentitel

ASP.NET bietet ein stabile Standort Zuordnung Framework Entwicklern eine hierarchische Siteübersicht in eine externe Ressource (z. B. eine XML-Datei oder Datenbank die Tabelle) sowie Web-Steuerelemente zum Anzeigen von Informationen über die Website-Zuordnung (z. B. die SiteMapPath definieren Menü, und TreeView-Steuerelement).

Die Siteübersichtsstruktur kann auch programmgesteuert mithilfe einer ASP.NET-Seite Code-Behind-Klasse zugegriffen werden. Auf diese Weise können wir automatisch eine Seitentitel an den Titel der entsprechenden Knoten in der Siteübersicht festlegen. Wir verbessern die `BasePage` Klasse, die in Schritt2 erstellt werden, sodass er diese Funktionalität bietet. Aber wir müssen zunächst eine Siteübersicht für unsere Website erstellen.

> [!NOTE]
> In diesem Lernprogramm wird davon ausgegangen, dass der Reader bereits mit ASP vertraut ist. NET Website-Map-Funktionen. Weitere Informationen finden Sie unter der Siteübersicht, finden Sie in meinem mehrteiligen Artikelserie [ASP untersuchen. NET Websitenavigation](http://aspnet.4guysfromrolla.com/articles/111605-1.aspx).


### <a name="creating-the-site-map"></a>Erstellen der Site-Zuordnung

Das Map-Standortsystem wird erstellt, über die [Anbietermodell](http://aspnet.4guysfromrolla.com/articles/101905-1.aspx), dem entkoppelt die Siteübersicht API von der Logik, die Standortinformationen über die Zuordnung zwischen Arbeitsspeicher und einem persistenten Speicher serialisiert. .NET Framework im Lieferumfang der [ `XmlSiteMapProvider` Klasse](https://msdn.microsoft.com/library/system.web.xmlsitemapprovider.aspx), dies ist der Standardanbieter der Siteübersicht. Wie der Name schon sagt, `XmlSiteMapProvider` wird eine XML-Datei als seine Zuordnung Websitespeicher verwendet. Ermöglicht die Verwendung dieser Anbieter für unsere Siteübersicht definieren.

Starten Sie durch das Erstellen einer Website-Zuordnungsdatei mit dem Namen der Website-Stammordner `Web.sitemap`. Um dies zu erreichen, mit der rechten Maustaste auf den Namen der Website im Projektmappen-Explorer, neues Element hinzufügen, und wählen Sie die Vorlage Siteübersicht. Stellen Sie sicher, dass die Datei heißt `Web.sitemap` , und klicken Sie auf Hinzufügen.


[![Fügen Sie eine Datei namens Web.sitemap zum Stammordner der Website hinzu.](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/_static/image9.png)](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/_static/image8.png)

**Abbildung 06**: Hinzufügen einer Datei mit dem Namen `Web.sitemap` zum Stammordner der Website ([klicken Sie hier, um das Bild in voller Größe angezeigt](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/_static/image10.png))


Hinzufügen der folgenden XML-Code die `Web.sitemap` Datei:


[!code-xml[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/samples/sample9.xml)]

Dieser XML-Code definiert die hierarchischen Siteübersichtsstruktur siehe Abbildung 7.


![Der Siteübersicht ist derzeit nicht zusammengesetzt von drei Zuordnung Websiteknoten](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/_static/image11.png)

**Abbildung 07**: der Siteübersicht ist derzeit nicht zusammengesetzt von drei Zuordnung Websiteknoten


Wir wird die Siteübersichtsstruktur in zukünftigen Lernprogrammen aktualisiert, wenn wir neue Beispiele hinzufügen.

### <a name="updating-the-master-page-to-include-navigation-web-controls"></a>Aktualisieren die Master-Seite, um Web Navigationssteuerelemente einschließen

Nun, da wir eine Siteübersicht definiert haben, aktualisieren wir die Gestaltungsvorlage Einbeziehung Web Navigationssteuerelemente. Insbesondere, fügen Sie einem ListView-Steuerelement in die linke Spalte im Abschnitt Lektionen, der eine ungeordnete Liste mit ein Listenelement für jeden Knoten in der Siteübersicht definierten rendert.

> [!NOTE]
> ListView-Steuerelement ist neu in ASP.NET Version 3.5. Wenn Sie eine frühere Version von ASP.NET verwenden, verwenden Sie stattdessen Wiederholungsmodul-Steuerelement. Weitere Informationen zu ListView-Steuerelement, finden Sie unter [ListView-Steuerelement und DataPager-Steuerelemente mithilfe von ASP.NET 3.5](http://aspnet.4guysfromrolla.com/articles/122607-1.aspx).


Durch das Entfernen der vorhandenen ungeordnete Liste Markup im Abschnitt "Lektionen" starten. Als Nächstes einem ListView-Steuerelement aus der Toolbox ziehen, und legen Sie es unterhalb der Lektionen Überschrift. ListView befindet sich im Datenabschnitt der Toolbox, zusammen mit den anderen Steuerelemente: GridView, DetailsView und FormView. Legen Sie das ListView-ID-Eigenschaft auf `LessonsList`.

Wählen Sie den Konfigurations-Assistenten zum Binden von ListView an ein neues SiteMapDataSource-Steuerelement mit dem Namen `LessonsDataSource`. Das Steuerelement SiteMapDataSource gibt die hierarchische Struktur vom Standortsystem Zuordnung zurück.


[![Binden eines SiteMapDataSource-Steuerelements an LessonsList ListView-Steuerelement](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/_static/image13.png)](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/_static/image12.png)

**Abbildung 08**: Binden eines SiteMapDataSource-Steuerelements, um die `LessonsList` ListView-Steuerelement ([klicken Sie hier, um das Bild in voller Größe angezeigt](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/_static/image14.png))


Nach dem Erstellen des Steuerelements SiteMapDataSource, müssen wir die ListView-Vorlagen zu definieren, sodass eine ungeordnete Liste mit ein Listenelement für jeden Knoten zurückgegeben, die vom Steuerelement SiteMapDataSource gerendert. Dies kann erreicht werden, verwenden das folgende Vorlagenmarkup:


[!code-aspx[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/samples/sample10.aspx)]

Die `LayoutTemplate` generiert das Markup für eine ungeordnete Liste (`<ul>...</ul>`) während der `ItemTemplate` rendert jedes Element der SiteMapDataSource als ein Listenelement zurückgegebenes (`<li>`), die einen Link zu einer bestimmten Lektion enthält.

Nach dem Konfigurieren der ListView-Vorlagen, besuchen Sie die Website. Wie in Abbildung 9 gezeigt, enthält die Lektionen im Abschnitt ein einzelnes Element mit Aufzählungszeichen, Startseite an. Wo sind die Info und mehrere ContentPlaceHolder Steuerelemente Lektionen verwenden? Die SiteMapDataSource zurückzugebenden eine hierarchische Zusammenstellung von Daten ausgelegt ist, aber das ListView-Steuerelement kann nur eine einzelne Ebene der Hierarchie angezeigt. Daher wird nur die erste Ebene der Karte Websiteknoten SiteMapDataSource zurückgegebenes angezeigt.


[![Die Lektionen-Abschnitt enthält ein einzelnes Listenelement](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/_static/image16.png)](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/_static/image15.png)

**Abbildung 09**: Abschnitt Lektionen enthält ein einzelnes Element der Liste ([klicken Sie hier, um das Bild in voller Größe angezeigt](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/_static/image17.png))


Anzuzeigende mehrere Ebenen schachteln wir mehrere Listenansichten innerhalb der `ItemTemplate`. Diese Technik wurde in untersucht die [ *Masterseiten und Websitenavigation* Lernprogramm](../../data-access/introduction/master-pages-and-site-navigation-cs.md) von Meine [arbeiten mit Tutorial Datenreihe](../../data-access/index.md). Für dieses Lernprogramm Reihe unsere Siteübersicht enthält jedoch nur eine zwei Ebenen: Home (die oberste Ebene); und jede Lektion als untergeordnetes Element der Startseite. Anstatt das Erstellen einer geschachtelten ListView, können wir stattdessen anweisen SiteMapDataSource nicht zurückzugebenden Startknoten durch Festlegen seiner [ `ShowStartingNode` Eigenschaft](https://msdn.microsoft.com/library/system.web.ui.webcontrols.sitemapdatasource.showstartingnode.aspx) auf `false`. Im Endeffekt ist, dass die SiteMapDataSource gestartet wird, wird durch Zurückgeben des Websiteknoten für die Zuordnung der zweiten Ebene.

Durch diese Änderung ListView Aufzählungszeichen Elemente für die Info und mehrere ContentPlaceHolder-Steuerelemente mithilfe von Lektionen, lässt aber eine Aufzählungspunkt für die Startseite. Wir können zur Behebung des Problems, explizit eine Aufzählungspunkt hinzufügen, für die Startseite in der `LayoutTemplate`:


[!code-aspx[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/samples/sample11.aspx)]

Konfigurieren die SiteMapDataSource zum Auslassen des Startknotens, und eine Startseite Aufzählungspunkt explizit hinzufügen zeigt Abschnitt Lektionen jetzt die beabsichtigte Ausgabe.


[![Die Lektionen-Abschnitt enthält eine Aufzählungspunkt für Home und jeder untergeordnete Knoten](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/_static/image19.png)](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/_static/image18.png)

**Abbildung 10**: der Lektionen Abschnitt enthält eine Aufzählungspunkt für Startseite "und" jedem untergeordneten Knoten ([klicken Sie hier, um das Bild in voller Größe angezeigt](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/_static/image20.png))


### <a name="setting-the-title-based-on-the-site-map"></a>Festlegen des Titels basierend auf der Siteübersicht

Mit der Siteübersicht erfüllt, können wir aktualisieren unsere `BasePage` Klasse, die den Titel in der Siteübersicht angegeben verwendet. Wie in Schritt2, möchten wir nur Titel der Website zuordnen des Knotens zu verwenden, wenn der Titel der Seite vom Entwickler Seite nicht explizit festgelegt wurde. Wenn die angeforderten Seite explizit festgelegten keinen Seitentitel und befindet sich nicht in der Siteübersicht, und klicken Sie dann wir die angeforderte Seite Filename (abzüglich der Erweiterung), Veröffentlichungsmethoden müssen wie in Schritt2. Abbildung 11 zeigt dieser Entscheidungsprozess.


![In Ermangelung einer explizit Seitentitel festlegen wird die entsprechende Zuordnung Websiteknoten des Titel verwendet.](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/_static/image21.png)

**Abbildung 11**: In Abwesenheit einer explizit Seitentitel festlegen, wird die entsprechende Zuordnung Websiteknoten der Titel verwendet


Update der `BasePage` Klasse `OnLoadComplete` Methode, um den folgenden Code einfügen:


[!code-csharp[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/samples/sample12.cs)]

Wie zuvor die `OnLoadComplete` Methode startet, wird festgestellt, ob der Titel der Seite explizit festgelegt wurde. Wenn `Page.Title` ist `null`, eine leere Zeichenfolge oder den Wert "Seite" unbenannt "" zugewiesen ist, und klicken Sie dann der Code automatisch einen Wert zuweist `Page.Title`.

Zum Bestimmen des Titels, startet der Code durch Verweisen auf die [ `SiteMap` Klasse](https://msdn.microsoft.com/library/system.web.sitemap.aspx)des [ `CurrentNode` Eigenschaft](https://msdn.microsoft.com/library/system.web.sitemap.currentnode.aspx). `CurrentNode` Gibt die [ `SiteMapNode` ](https://msdn.microsoft.com/library/system.web.sitemapnode.aspx) Instanz in der Siteübersicht, die die derzeit angeforderte Seite entspricht. Vorausgesetzt, die gerade angeforderte Seite befindet sich innerhalb der Siteübersicht der `SiteMapNode`des `Title` Titel der Seite Eigenschaft zugewiesen ist. Ist die aktuell angeforderte Seite nicht in der Siteübersicht `CurrentNode` gibt `null` und Dateiname für die angeforderte Seite dient als Titel (wie in Schritt2 vorgenommen wurde).

Abbildung 12 zeigt die `MultipleContentPlaceHolders.aspx` Seite, wenn Sie über einen Browser angezeigt. Da auf dieser Seite Titel nicht explizit festgelegt ist, wird die entsprechende Zuordnung Websiteknoten der Titel wird stattdessen verwendet.


![Titel der MultipleContentPlaceHolders.aspx Seite der Siteübersicht per Pull abgerufen wird](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/_static/image22.png)

**Abbildung 12**: die `MultipleContentPlaceHolders.aspx` des Seitentitel der Siteübersicht per Pull abgerufen wird


## <a name="step-4-adding-other-page-specific-markup-to-theheadsection"></a>Schritt 4: Hinzufügen von Markups seitenspezifische, um die`<head>`Abschnitt

Erläutert die Schritte 1, 2 und 3, Anpassen der `<title>` Element auf Grundlage von Seite. Zusätzlich zu `<title>`, `<head>` Abschnitt enthalten möglicherweise `<meta>` Elemente und `<link>` Elemente. Wie weiter oben in diesem Lernprogramm erwähnt `Site.master`des `<head>` Abschnitt enthält eine `<link>` Element `Styles.css`. Da dies `<link>` Element innerhalb der Masterseite definiert ist, befindet sich auf die `<head>` Abschnitt für alle Inhaltsseiten. Aber wie gehen wir zum Hinzufügen von `<meta>` und `<link>` Elemente auf Basis von Seite?

Die einfachste Möglichkeit, die speziell für Codepage Inhalt hinzufügen der `<head>` Abschnitt ist durch das Erstellen eines Steuerelements ContentPlaceHolder auf der Masterseite. Wir haben bereits eine solche ContentPlaceHolder (mit dem Namen `head`). Aus diesem Grund zum Hinzufügen benutzerdefinierter `<head>` Markup, erstellen Sie ein entsprechendes Inhaltssteuerelement auf der Seite, und platzieren Sie das Markup vorhanden.

Zum Hinzufügen von benutzerdefinierten veranschaulichen `<head>` Markup wir zu einer Seite gehören eine `<meta>` unseren aktuellen Satz von Inhaltsseiten Description-Element. Die `<meta>` Description-Element enthält eine kurze Beschreibung zur Webseite; die meisten Suchmaschinen integrieren Sie diese Informationen in einer Form beim Anzeigen von Suchergebnissen.

Ein `<meta>` Description-Element weist folgende Form:


[!code-html[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/samples/sample13.html)]

Um dieses Markup einer Inhaltsseite hinzuzufügen, fügen Sie den oben genannten Text an das Inhaltssteuerelement, das die Gestaltungsvorlage Head ContentPlaceHolder zugeordnet. Um beispielsweise Definieren eine `<meta>` Description-Element für `Default.aspx`, fügen Sie das folgende Markup hinzu:


[!code-aspx[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/samples/sample14.aspx)]

Da die Head ContentPlaceHolder nicht innerhalb der HTML-Seite-Texts ist, wird das Markup, das Inhaltssteuerelement hinzugefügt nicht in der Entwurfsansicht angezeigt. Anzeigen der `<meta>` Beschreibung Element Besuch `Default.aspx` über einen Browser. Nachdem die Seite geladen wurde, zeigen Sie die Quelle, und beachten Sie, dass die `<head>` Abschnitt enthält das Markup im Inhaltssteuerelement angegeben.

Hinzuzufügende erkundet `<meta>` Beschreibungselemente `About.aspx`, `MultipleContentPlaceHolders.aspx`, und `Login.aspx`.

### <a name="programmatically-adding-markup-to-theheadregion"></a>Programmgesteuertes Hinzufügen von Markup der`<head>`Region

Die Head ContentPlaceHolder erlaubt es uns, der Gestaltungsvorlage deklarativ benutzerdefinierte Markup hinzugefügt `<head>` Region. Benutzerdefinierte Markup kann auch programmgesteuert hinzugefügt werden. Bedenken Sie, dass die `Page` Klasse `Header` -Eigenschaft gibt die `HtmlHead` Instanz in die Masterseite definiert (die `<head runat="server">`).

Können programmgesteuert Inhalt hinzugefügt wird die `<head>` Region ist nützlich, wenn es sich bei der hinzuzufügenden Inhalt dynamisch ist. Möglicherweise ist der Benutzer Zugriff auf die Seite abhängig; Es ist möglicherweise aus einer Datenbank abgerufen werden. Unabhängig von der Ursache können Sie Inhalt zum Hinzufügen der `HtmlHead` durch Hinzufügen von Steuerelementen der steuerelementeauflistung etwa so:


[!code-csharp[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/samples/sample15.cs)]

Der obige Code fügt der `<meta>` Keywords-Element, um die `<head>` Region, die eine durch Trennzeichen getrennte Liste mit Schlüsselwörtern bereitstellt, die die Seite zu beschreiben. Beachten Sie, dass Hinzufügen einer `<meta>` Tag, die Sie erstellen eine [ `HtmlMeta` ](https://msdn.microsoft.com/library/system.web.ui.htmlcontrols.htmlmeta.aspx) Instanz, legen dessen `Name` und `Content` Eigenschaften, und fügen Sie diese der `Header`des `Controls` Auflistung. Auf ähnliche Weise programmgesteuert hinzufügen einer `<link>` Element, erstellen eine [ `HtmlLink` ](https://msdn.microsoft.com/library/system.web.ui.htmlcontrols.htmllink.aspx) Objekt, dessen Eigenschaften festlegen und fügen Sie es auf die `Header`des `Controls` Auflistung.

> [!NOTE]
> Beliebiges Markup hinzufügen möchten, erstellen Sie eine [ `LiteralControl` ](https://msdn.microsoft.com/library/system.web.ui.literalcontrol.aspx) Instanz, legen dessen `Text` -Eigenschaft, und fügen Sie diese der `Header`des `Controls` Auflistung.


## <a name="summary"></a>Zusammenfassung

In diesem Lernprogramm erläutert, eine Vielzahl von Möglichkeiten zum Hinzufügen von `<head>` Region Markup auf Basis von Seite. Eine Masterseite einschließen sollte ein `HtmlHead` Instanz (`<head runat="server">`) mit einem ContentPlaceHolder. Die `HtmlHead` Instanz ermöglicht den programmgesteuerten Zugriff auf Inhaltsseiten der `<head>` Region und deklarativ und programmgesteuert über die Seite festlegen des title; das Steuerelement ContentPlaceHolder ermöglicht, benutzerdefinierte Markup hinzugefügt werden die `<head>` Abschnitt deklarativ über die ein Steuerelement.

Viel Spaß beim Programmieren!

### <a name="further-reading"></a>Weiterführende Themen

Weitere Informationen zu den Themen in diesem Lernprogramm erläutert finden Sie in den folgenden Ressourcen:

- [Festlegen von Titel der Seite dynamisch in ASP.NET](http://aspnet.4guysfromrolla.com/articles/051006-1.aspx)
- [ASP wird untersucht. NET Website-Navigation](http://aspnet.4guysfromrolla.com/articles/111605-1.aspx)
- [Zum Verwenden von HTML-Meta-Tags](http://searchenginewatch.com/showPage.html?page=2167931)
- [Masterseiten in ASP.NET](http://www.odetocode.com/articles/419.aspx)
- [Mithilfe von ASP.NET 3.5 ListView-Steuerelement und DataPager-Steuerelemente](http://aspnet.4guysfromrolla.com/articles/122607-1.aspx)
- [Verwendung einer benutzerdefinierten Basisklasse für die ASP.NET-Seiten Code-Behind-Klassen](http://aspnet.4guysfromrolla.com/articles/041305-1.aspx)

### <a name="about-the-author"></a>Informationen zum Autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor mehrerer ASP/ASP.NET-Büchern und Gründer von 4GuysFromRolla.com arbeitet mit Microsoft-Web-Technologien seit 1998. Scott fungiert als ein unabhängiger Berater, Trainer und Writer. Sein neueste Buch wird [ *Sams Schulen selbst ASP.NET 3.5 in 24 Stunden*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Scott erreicht werden kann, zur [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com) oder über seinen Blog unter [ http://ScottOnWriting.NET ](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Besonderen Dank an

Diese Reihe von Lernprogrammen wurde durch viele nützliche Bearbeiter überprüft. Lead Prüfer für dieses Lernprogramm wurden Zack Jones und Suchi Banerjee. Meine bevorstehende MSDN-Artikel Überprüfen von Interesse? Wenn dies der Fall ist, löschen Sie mich zeilenweise [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com).

> [!div class="step-by-step"]
> [Zurück](multiple-contentplaceholders-and-default-content-cs.md)
> [Weiter](urls-in-master-pages-cs.md)
