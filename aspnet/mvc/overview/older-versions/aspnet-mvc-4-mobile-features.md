---
uid: mvc/overview/older-versions/aspnet-mvc-4-mobile-features
title: Mobile ASP.NET MVC 4-Funktionen | Microsoft Docs
author: Rick-Anderson
description: Es ist jetzt eine MVC 5-Version dieses Lernprogramm mit Codebeispiele unter Bereitstellen einer ASP.NET MVC 5 Mobile Web-Anwendung auf Azure-Websites.
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/15/2012
ms.topic: article
ms.assetid: 27dc4fc8-1b51-43b0-933f-fc1b52476523
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/aspnet-mvc-4-mobile-features
msc.type: authoredcontent
ms.openlocfilehash: f4e0e4eb558e0c7b9e94fc83ede986fa4c666739
ms.sourcegitcommit: 016f4d58663bcd442930227022de23fb3abee0b3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/12/2018
---
<a name="aspnet-mvc-4-mobile-features"></a>ASP.NET MVC 4-Funktionen für mobile Geräte
====================
Durch [Rick Anderson](https://github.com/Rick-Anderson)

> Es gibt jetzt eine MVC 5-Version dieses Lernprogramm mit Codebeispiele unter [Bereitstellen einer ASP.NET MVC 5 Mobile Web-Anwendung auf Azure-Websites](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/).


In diesem Lernprogramm erfahren Sie die Grundlagen der Arbeit mit mobilen Funktionen in einer ASP.NET MVC 4-Web-Anwendung. Für dieses Lernprogramm können Sie [Visual Studio Express 2012](https://www.microsoft.com/visualstudio/11/products/express) oder Visual Web Developer 2010 Express Service Pack 1 (&quot;Visual Web Developer oder VWD&quot;). Sie können die professional-Version von Visual Studio verwenden, wenn Sie bereits, die verfügen.

Bevor Sie beginnen, stellen Sie sicher, dass Sie die unten aufgeführten erforderlichen Komponenten installiert haben.

- [Visual Studio Express 2012](https://www.microsoft.com/visualstudio/11/products/express) (empfohlen) oder Visual Studio Web Developer Express SP1. Visual Studio 2012 enthält ASP.NET MVC 4. Wenn Sie Visual Web Developer 2010 verwenden, müssen Sie installieren [ASP.NET MVC 4](https://go.microsoft.com/fwlink/?LinkId=243392).

Sie benötigen auch einen mobilen Browser-Emulator. Eine der folgenden funktioniert:

- [Windows 7, Windows Phone-Emulator](https://msdn.microsoft.com/library/ff402563(VS.92).aspx). (Dies ist der Emulator, der in den meisten die Screenshots in diesem Lernprogramm verwendet wird.)
- Ändern Sie die Benutzer-Agent-Zeichenfolge, um ein iPhone zu emulieren. Finden Sie unter [dies](http://www.howtogeek.com/113439/how-to-change-your-browsers-user-agent-without-installing-any-extensions/) Blogeintrag.
- [Opera Mobile-Emulator](http://www.opera.com/developer/tools/mobile/)
- [Apple Safari](http://www.apple.com/safari/download/) mit dem Benutzeragent auf dem iPhone festgelegt. Anleitungen zum Festlegen des Benutzer-Agents in Safari auf "iPhone" finden Sie unter [Vorgehensweise können Sie vorgeben, es handelt sich um Internet Explorer Safari](http://www.davidalison.com/2008/05/how-to-let-safari-pretend-its-ie.html) David Alison Blog.

Visual Studio-Projekten mit C#-Quellcode stehen zu diesem Thema steht zur Verfügung:

- [Starter Projekt zum Herunterladen](https://go.microsoft.com/fwlink/?linkid=228307&amp;clcid=0x409)
- [Projekt zum Herunterladen abgeschlossen](https://go.microsoft.com/fwlink/?linkid=228306&amp;clcid=0x409)

### <a name="what-youll-build"></a>Was müssen Sie erstellen

In diesem Lernprogramm fügen Sie mobile-Features für die einfache Konferenz-Angebot-Anwendung, die in bereitgestellt ist die [Startprojekt](https://go.microsoft.com/fwlink/?LinkId=228307). Der folgende Screenshot zeigt die Seite "Tags" von der fertigen Anwendung aus, wie in der [Windows Phone-Emulator für Windows 7](https://msdn.microsoft.com/library/ff402563(VS.92).aspx). Finden Sie unter [Tastatur Zuordnung für Windows Phone-Emulator](https://msdn.microsoft.com/library/ff754352(v=vs.92).aspx) um Tastatureingaben zu vereinfachen.

[![p1_Tags_CompletedProj](aspnet-mvc-4-mobile-features/_static/image2.png)](aspnet-mvc-4-mobile-features/_static/image1.png)

Können Sie Internet Explorer Version 9 oder 10, FireFox oder Chrome Entwickeln Ihrer mobile Anwendung durch Festlegen der [Benutzer-Agent-Zeichenfolge](http://www.howtogeek.com/113439/how-to-change-your-browsers-user-agent-without-installing-any-extensions/). Die folgende Abbildung zeigt den abgeschlossene Lernprogramm zur Verwendung von Internet Explorer, um ein iPhone emulieren. Können Sie den Internet Explorer F-12 Developer Tools und die [Fiddler-Tool](http://www.fiddler2.com/fiddler2/) , um die Anwendung zu debuggen.

![](aspnet-mvc-4-mobile-features/_static/image3.png)

### <a name="skills-youll-learn"></a>Fähigkeiten, die Sie erfahren

Hier ist Sie lernen:

- Verwendung von ASP.NET MVC 4-Vorlagen die HTML5 `viewport` Attribut und das adaptive Rendering verbessern auf mobilen Geräten anzeigen.
- Vorgehensweise: Erstellen von Mobile-spezifischen Ansichten.
- Vorgehensweise: eine Ansicht wechseln, können Benutzer zum ein-/ausschalten zwischen mobilen und desktop einen Überblick über die Anwendung erstellen.

### <a name="getting-started"></a>Erste Schritte

Die Anwendung Konferenz-Angebot für das Startprojekt über den folgenden Link herunterladen: [herunterladen](https://go.microsoft.com/fwlink/?LinkId=228307). Klicken Sie dann im Windows-Explorer mit der Maustaste die *MvcMobile.zip* Datei, und wählen Sie **Eigenschaften**. In der **MvcMobile.zip Eigenschaften** Dialogfeld Wählen Sie die **zum Aufheben der Sperre** Schaltfläche. (Zum Aufheben der Blockierung verhindert, dass eine sicherheitswarnung angezeigt, das auftritt, wenn Sie versuchen, eine *ZIP* -Datei, die Sie aus dem Web heruntergeladen haben.)

![p1_unBlock](aspnet-mvc-4-mobile-features/_static/image4.png)

Mit der rechten Maustaste die *MvcMobile.zip* Datei, und wählen Sie **alle extrahieren** um die Datei zu entpacken. Öffnen Sie in Visual Studio die *MvcMobile.sln* Datei.

Drücken Sie STRG + F5, um die Anwendung auszuführen, die es in Ihrem Browser desktop angezeigt wird. Starten Sie den Emulator mobilen Browser, kopieren Sie die URL für die Konferenz-Anwendung in den Emulator und klicken Sie dann auf die **Tag Durchsuchen** Link. Wenn Sie den Windows Phone-Emulator verwenden, klicken Sie in der URL-Leiste, und drücken Sie Pause-Taste, um den Zugriff zu erhalten. Die folgende Abbildung zeigt die *AllTags* Ansicht (Parallelitätsproblemen **durchsuchen, indem Tag**).

[![p1_browseTag](aspnet-mvc-4-mobile-features/_static/image6.png)](aspnet-mvc-4-mobile-features/_static/image5.png)

Die Anzeige ist sehr gut lesbar auf einem mobilen Gerät. Wählen Sie den Link für ASP.NET.

[![p1_tagged_ASPNET](aspnet-mvc-4-mobile-features/_static/image8.png)](aspnet-mvc-4-mobile-features/_static/image7.png)

Die ASP.NET-Tag-Ansicht ist sehr überladen. Z. B. die **Datum** Spalte ist sehr schwer zu lesen. Später in diesem Lernprogramm erstellen Sie eine Version der *AllTags* anzeigen, die speziell für mobile Browser ist und wird, die die Anzeige lesbar machen.

Hinweis: Zurzeit ein Fehler in der mobilen caching-Engine. Für Produktionsanwendungen müssen Sie installieren die [festen DisplayModes](http://nuget.org/packages/Microsoft.AspNet.Mvc.FixedDisplayModes) Nugget-Paket. Finden Sie unter [ASP.NET MVC 4 Mobile Zwischenspeichern Fehler festen](https://blogs.msdn.com/b/rickandy/archive/2012/09/17/asp-net-mvc-4-mobile-caching-bug-fixed.aspx) Einzelheiten über die Korrektur.

## <a name="css-media-queries"></a>CSS-Medienabfragen

[CSS-Medienabfragen](http://www.w3.org/TR/css3-mediaqueries/) sind eine Erweiterung zu CSS-Medientypen. Sie ermöglichen Ihnen die Erstellung von Regeln, die die Standard-CSS-Regeln für bestimmte Browser (Benutzer-Agents) zu überschreiben. Maximale Bildschirmgröße definiert eine allgemeine Regel CSS, die mobile Browser ausgerichtet ist. Die *Content\Site.css* -Datei, die erstellt wird, wenn Sie ein neues ASP.NET MVC 4-Internet-Projekt erstellen enthält die folgenden Medienabfrage:

[!code-css[Main](aspnet-mvc-4-mobile-features/samples/sample1.css)]

Wenn das Browserfenster 850 Pixel breit oder weniger ist, wird die CSS-Regeln innerhalb dieses Blocks Medien verwendet. Sie können CSS-Medienabfragen wie folgt Geben Sie eine bessere Anzeige der HTML-Inhalt in kleine Browsern (z. B. mobile Browser) als die Standard-CSS-Regeln, die entwickelt wurden für das breiter zeigt der Desktopbrowsern.

## <a name="the-viewport-meta-tag"></a>Das Viewport-Meta-Tag

Die meisten mobile Browsern definieren Sie eine virtuelle Browser Fensterbreite (die *Viewport*), ist viel größer als die tatsächliche Breite des mobilen Geräts. Dies ermöglicht es mobile Browsern an die gesamte Webseite innerhalb der virtuellen Anzeige anpassen. Benutzer können dann interessante Inhalte vergrößern. Wenn Sie die Breite des Anzeigebereichs an die Breite des tatsächlichen Gerät festlegen, ist jedoch keine vergrößern/verkleinern erforderlich ist, da der Inhalt in der mobilen Browser passt.

Der Viewport `<meta>` -Tag in der ASP.NET MVC 4-Layout-Datei legt den Viewport an die Breite des Geräts. Die folgende Zeile zeigt, Viewports `<meta>` -Tag in der ASP.NET MVC 4-Layout-Datei.

[!code-html[Main](aspnet-mvc-4-mobile-features/samples/sample2.html)]

## <a name="examining-the-effect-of-css-media-queries-and-the-viewport-meta-tag"></a>Untersuchen die Auswirkungen der CSS-Medienabfragen sowie das Viewport-Meta-Tag

Öffnen der *Views\Shared\\_Layout.cshtml* Datei im Editor, und kommentieren Sie den Viewport `<meta>` Tag. Das folgende Markup zeigt die auskommentierte Zeile.

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample3.cshtml)]

Öffnen der *MvcMobile\Content\Site.css* Datei im Editor, und ändern Sie die maximale Breite in der Media-Abfrage auf 0 (null) Pixel. Dadurch wird verhindert, dass die CSS-Regeln in mobilen Browsern verwendet wird. Die folgende Zeile zeigt die geänderte Medienabfrage:

[!code-css[Main](aspnet-mvc-4-mobile-features/samples/sample4.css)]

Speichern Sie die Änderungen zu, und navigieren Sie zu der Konferenz-Anwendung in einen mobilen Browser-Emulator. Die kleine Text in der folgenden Abbildung ist das Ergebnis des Entfernens des Viewports `<meta>` Tag. Mit keinen Viewport verfügen `<meta>` Tag, der Browser wird verkleinern, Viewports Standardbreite (850 Pixel oder breiter für die meisten mobilen Browsern.)

[![p1_noViewPort](aspnet-mvc-4-mobile-features/_static/image10.png)](aspnet-mvc-4-mobile-features/_static/image9.png)

Ihre Änderungen rückgängig zu machen – kommentieren Sie den Viewport `<meta>` -Tag in die Layoutdatei und Wiederherstellen der Medienabfrage 850 Pixel in der *"Site.CSS" ändern* Datei. Speichern Sie die Änderungen zu, und aktualisieren Sie den mobilen Browser, um sicherzustellen, dass die Anzeige mobilgerätefreundliche wiederhergestellt wurde.

Der Viewport `<meta>` Tags und CSS-Medienabfrage sind nicht spezifisch für ASP.NET MVC 4, und profitieren Sie von diesen Funktionen in einer beliebigen Webanwendung. Aber sie sind jetzt integriert die Dateien, die generiert werden, wenn Sie ein neues ASP.NET MVC 4-Projekt erstellen.

Weitere Informationen zu den Viewport `<meta>` zu kennzeichnen, finden Sie unter [eine Geschichte von zwei Ansichten – Teil 2](http://www.quirksmode.org/mobile/viewports2.html).

Im nächsten Abschnitt sehen Sie, wie Mobile-Browser auf bestimmte Ansichten bereitgestellt werden.

## <a name="overriding-views-layouts-and-partial-views"></a>Überschreiben von Ansichten, Layouts und Teilansichten

Eine bedeutende neue Funktion in ASP.NET MVC 4 ist einen einfachen Mechanismus, mit dem Sie eine beliebige Ansicht (einschließlich Layouts und Teilansichten) außer Kraft setzen für mobile Browser im Allgemeinen für einen einzelnen mobilen Browser oder für alle Browser an. Um eine Mobile-spezifische Ansicht zu gewährleisten, können Sie eine Datei kopieren und hinzufügen *. Mobile* an den Dateinamen an. Um beispielsweise eine Mobile *Index* anzeigen, kopieren Sie *Views\Home\Index.cshtml* auf *Views\Home\Index.Mobile.cshtml*.

In diesem Abschnitt erstellen Sie eine Mobile-spezifische Layoutdatei.

Kopieren Sie zum Starten *Views\Shared\\_Layout.cshtml* auf *Views\Shared\\_Layout.Mobile.cshtml*. Open  *\_Layout.Mobile.cshtml* und ändern Sie den Titel aus **MVC4-Konferenz** auf **Konferenz (Mobil)**.

In den einzelnen `Html.ActionLink` aufrufen, entfernen Sie "Durchsuchen nach" in jeder Link *ActionLink*. Der folgende Code zeigt die mobile Layoutdatei abgeschlossenen Textabschnitt.

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample5.cshtml)]

Kopieren der *Views\Home\AllTags.cshtml* Datei *Views\Home\AllTags.Mobile.cshtml*. Öffnen Sie die neue Datei, und ändern Sie die `<h2>` Element von "Transponder", "Tags (M)":

[!code-html[Main](aspnet-mvc-4-mobile-features/samples/sample6.html)]

Navigieren Sie zu der Seite "Tags" mithilfe eines Browsers desktop und mobile Browser-Emulator verwenden. Der Emulator mobilen Browser zeigt die zwei Änderungen, die Sie vorgenommen haben.

[![p2m_layoutTags.mobile](aspnet-mvc-4-mobile-features/_static/image12.png)](aspnet-mvc-4-mobile-features/_static/image11.png)

Im Gegensatz dazu hat die desktop Anzeige nicht geändert werden.

[![p2_layoutTagsDesktop](aspnet-mvc-4-mobile-features/_static/image14.png)](aspnet-mvc-4-mobile-features/_static/image13.png)

## <a name="browser-specific-views"></a>Browser-spezifischen Ansichten

Zusätzlich zu den spezifischen Mobile und Desktop-spezifischen Ansichten können Sie Ansichten für einen einzelnen Browser erstellen. Beispielsweise können Sie Ansichten erstellen, die speziell für den iPhone-Browser sind. In diesem Abschnitt erstellen Sie ein Layout für den iPhone-Browser und ein iPhone-Version von der *AllTags* anzeigen.

Öffnen der *"Global.asax"* Datei, und fügen Sie den folgenden Code hinzu. die `Application_Start` Methode.

[!code-csharp[Main](aspnet-mvc-4-mobile-features/samples/sample7.cs)]

Dieser Code definiert einen neuen Anzeigemodus mit dem Namen "iPhone", die nach dem für jede eingehende Anforderung gesucht werden soll. Die eingehende Anforderung die Bedingung übereinstimmt, die Sie definiert (d. h., wenn der Benutzer-Agent die Zeichenfolge "iPhone" enthält), sieht ASP.NET MVC für Ansichten, deren Name das Suffix "iPhone" enthält.

In den Code, mit der Maustaste `DefaultDisplayMode`, wählen Sie **beheben**, und wählen Sie dann `using System.Web.WebPages;`. Dadurch wird einen Verweis auf die `System.Web.WebPages` -Namespace, der ist, die `DisplayModes` und `DefaultDisplayMode` Typen definiert sind.

[![p2_resolve](aspnet-mvc-4-mobile-features/_static/image16.png)](aspnet-mvc-4-mobile-features/_static/image15.png)

Alternativ können Sie nur manuell die folgende Zeile zum Hinzufügen der `using` Abschnitt der Datei.

[!code-csharp[Main](aspnet-mvc-4-mobile-features/samples/sample8.cs)]

Der vollständige Inhalt der der *"Global.asax"* Datei wird unten gezeigt.

[!code-csharp[Main](aspnet-mvc-4-mobile-features/samples/sample9.cs)]

Speichern Sie die Änderungen. Kopieren der *MvcMobile\Views\Shared\\_Layout.Mobile.cshtml* Datei *MvcMobile\Views\Shared\\_Layout.iPhone.cshtml*. Öffnen Sie die neue Datei, und ändern Sie dann die `h1` Überschrift aus `Conference (Mobile)` auf `Conference (iPhone)`.

Kopieren der *MvcMobile\Views\Home\AllTags.Mobile.cshtml* Datei *MvcMobile\Views\Home\AllTags.iPhone.cshtml*. Ändern Sie in der neuen Datei das `<h2>` Element von "Tags (M)", "Transponder (iPhone)".

Führen Sie die Anwendung aus. Einen mobilen Browser-Emulator ausführen, stellen Sie sicher, dass ihre Benutzer-Agent auf "iPhone" festgelegt ist, und navigieren Sie zu der *AllTags* anzeigen. Der folgende Screenshot zeigt die *AllTags* Ansicht gerendert wird, der [Safari](http://www.apple.com/safari/download/) Browser. Sie können Safari für Windows herunterladen [hier](https://support.apple.com/kb/DL1531).

[![p2_iphoneView](aspnet-mvc-4-mobile-features/_static/image18.png)](aspnet-mvc-4-mobile-features/_static/image17.png)

In diesem Abschnitt haben wir gesehen, wie mobile Layouts und Ansichten erstellt und zum Erstellen des Layouts und Ansichten für bestimmte Geräte wie z. B. das iPhone. Im nächsten Abschnitt sehen Sie, wie Sie jQuery Mobile Weitere überzeugende mobilen Ansichten nutzen.

## <a name="using-jquery-mobile"></a>Verwenden von jQuery Mobile

Die [jQuery Mobile](http://jquerymobile.com/demos/1.0b3/#/demos/1.0b3/docs/about/intro.html) Bibliothek bietet ein Benutzeroberflächen-Framework, die auf allen wichtigen mobile Browsern arbeitet. jQuery Mobile gilt *schrittweise zunehmenden Verbesserungen* auf mobilen Browsern, CSS und JavaScript zu unterstützen. Schrittweise zunehmenden Verbesserungen ermöglicht allen Browsern den grundlegende Inhalt einer Webseite angezeigt wird, und lässt Anzahl leistungsfähigerer Browsern und Geräten, um eine umfangreichere Anzeige haben. Die JavaScript und CSS-Dateien, die mit jQuery Mobile enthalten sind, formatieren viele Elemente zum mobile Browsern passt, ohne Änderungen vorzunehmen Markup.

In diesem Abschnitt Installieren Sie die *jQuery.Mobile.MVC* NuGet-Paket, wodurch jQuery Mobile und ansichtumschaltung Widgets installiert wird.

Löschen Sie zum Starten der *Shared\\_Layout.Mobile.cshtml* und *Shared\\_Layout.iPhone.cshtml* Dateien, die Sie zuvor erstellt haben.

Benennen Sie *Views\Home\AllTags.Mobile.cshtml* und *Views\Home\AllTags.iPhone.cshtml* Dateien *Views\Home\AllTags.iPhone.cshtml.hide* und  *Views\Home\AllTags.Mobile.cshtml.hide*. Da die Dateien nicht mehr enthalten eine *cshtml* Erweiterung, sie wird nicht zum Rendern von ASP.NET MVC-Laufzeit verwendet werden die *AllTags* anzeigen.

Installieren der *jQuery.Mobile.MVC* NuGet-Paket auf diese Weise:

1. Aus der **Tools** klicken Sie im Menü **Bibliothekspaket-Manager**, und wählen Sie dann **Package Manager Console**.

    [![p3_packageMgr](aspnet-mvc-4-mobile-features/_static/image20.png)](aspnet-mvc-4-mobile-features/_static/image19.png)
2. In der **Package Manager Console**, geben Sie`Install-Package jQuery.Mobile.MVC -version 1.0.0`

Die folgende Abbildung zeigt die Dateien hinzugefügt und dem Projekt MvcMobile geändert werden, indem die jQuery.Mobile.MVC NuGet-Paket. Dateien, die hinzugefügt werden [hinzufügen] nach den Dateinamen angefügt haben. Anzeigen des Bilds nicht das GIF und PNG-Dateien hinzugefügt werden, um die *Content\images* Ordner.

![](aspnet-mvc-4-mobile-features/_static/image21.png)

Das jQuery.Mobile.MVC NuGet-Paket wird Folgendes installiert:

- Die *App\_Start\BundleMobileConfig.cs* -Datei, die benötigt werden, um die hinzugefügten jQuery JavaScript und CSS-Dateien verweisen. Sie müssen die Anweisungen unten befolgen und mobile Paket definiert, die in dieser Datei verweisen.
- jQuery Mobile-CSS-Dateien.
- Ein `ViewSwitcher` Controller Widget (*Controllers\ViewSwitcherController.cs*).
- jQuery Mobile JavaScript-Dateien.
- Eine jQuery Mobile formatiertes Layoutdatei (*Views\Shared\\_Layout.Mobile.cshtml*).
- Eine Teilansicht ansichtumschaltung *(MvcMobile\Views\Shared\\_ViewSwitcher.cshtml*), die einen Link am Anfang jeder Seite von Desktops und mobile Ansicht wechseln bereitstellt.
- Mehrere*PNG* und *GIF* Bilddateien in der *Content\images* Ordner.

Öffnen der *"Global.asax"* Datei, und fügen Sie den folgenden Code als die letzte Zeile des der `Application_Start` Methode.

[!code-csharp[Main](aspnet-mvc-4-mobile-features/samples/sample10.cs)]

Der folgende Code zeigt die vollständige *"Global.asax"* Datei.

[!code-csharp[Main](aspnet-mvc-4-mobile-features/samples/sample11.cs?highlight=26)]

> [!NOTE]
> Wenn Sie Internet Explorer 9 verwenden und nicht angezeigt werden die `BundleMobileConfig` Zeile oben in gelb hervorheben, klicken Sie auf die [Kompatibilitätssicht Schaltfläche](https://windows.microsoft.com/windows7/How-to-use-Compatibility-View-in-Internet-Explorer-9)![Überblick über die Schaltfläche "Kompatibilität anzeigen" (deaktiviert)] (http://res2.windows.microsoft.com/resbox/en/Windows 7/main/f080e77f-9b66-4ac8-9af0-803c4f8a859c_15.jpg " Überblick über die Schaltfläche "Kompatibilität anzeigen" (deaktiviert)") in Internet Explorer auf das Symbol, das Ändern von Konturen stellen ![Überblick über die Schaltfläche "Kompatibilität anzeigen" (deaktiviert)](http://res2.windows.microsoft.com/resbox/en/Windows 7/main/f080e77f-9b66-4ac8-9af0-803c4f8a859c_15.jpg "Überblick über die Schaltfläche "Kompatibilität anzeigen" (deaktiviert) ") auf eine Volltonfarbe ![Bild der Schaltfläche-kompatibilitätssicht angezeigt (on)](http://res1.windows.microsoft.com/resbox/en/Windows 7/main/156805ff-3130-481b-a12d-4d3a96470f36_14.jpg "Bild der Schaltfläche-kompatibilitätssicht angezeigt (on)"). Alternativ können Sie dieses Lernprogramm in Firefox- oder Chrome anzeigen.


Öffnen der *MvcMobile\Views\Shared\\_Layout.Mobile.cshtml* Datei, und fügen Sie das folgende Markup direkt nach der `Html.Partial` aufrufen:

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample12.cshtml)]

Die vollständige *MvcMobile\Views\Shared\\_Layout.Mobile.cshtml* Datei wird unten gezeigt:

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample13.cshtml)]

Erstellen Sie die Anwendung, und navigieren Sie im Emulator mobilen Browser zu dem *AllTags* anzeigen. Sie finden die folgenden Themen:

[![p3_afterNuGet](aspnet-mvc-4-mobile-features/_static/image23.png)](aspnet-mvc-4-mobile-features/_static/image22.png)

> [!NOTE]
> Sie können bestimmten mobilen Code durch Debuggen [Festlegen der Benutzer-Agent-Zeichenfolge](http://www.howtogeek.com/113439/how-to-change-your-browsers-user-agent-without-installing-any-extensions/) für Internet Explorer oder Chrome iPhone, und klicken Sie dann mit den F-12 Developer-Tools. Wenn Ihre mobile Browser angezeigt werden, nicht die **Home**, **Lautsprecher**, **Tag**, und **Datum** Links als Schaltflächen, die Verweise auf jQuery Mobile Skripts und CSS-Dateien sind möglicherweise nicht richtig.


Zusätzlich zu den Änderungen Stil finden Sie unter **mobile Ansicht anzeigen** und einen Link, den Sie von mobilen in desktop-Ansicht wechseln kann. Wählen Sie die **Desktopansicht** Link und der Desktopansicht angezeigt wird.

[![p3_desktopView](aspnet-mvc-4-mobile-features/_static/image25.png)](aspnet-mvc-4-mobile-features/_static/image24.png)

Die Desktopansicht stellt keine Möglichkeit, navigieren Sie direkt an die mobile Ansicht bereit. Sie müssen die nun behoben werden kann. Öffnen der *Views\Shared\\_Layout.cshtml* Datei. Direkt unter der Seite `body` Element, fügen Sie den folgenden Code, die das ansichtumschaltung Widget rendert hinzu:

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample14.cshtml)]

Aktualisieren der *AllTags* in der mobilen Browser anzeigen. Sie können nun zwischen desktop und mobile Ansichten navigieren.

[![p3_desktopViewWithMobileLink](aspnet-mvc-4-mobile-features/_static/image27.png)](aspnet-mvc-4-mobile-features/_static/image26.png)

> [!NOTE]
> Debuggen Hinweis: Sie können den folgenden Code am Ende der Views\Shared hinzufügen\\_ViewSwitcher.cshtml, um Sichten zu debuggen, wenn auf einem mobilen Gerät mit einem Browser die Benutzer-Agent-Zeichenfolge festgelegt werden.
> 
> [!code-csharp[Main](aspnet-mvc-4-mobile-features/samples/sample15.cs)]
> 
>  und das Hinzufügen der folgenden Spaltenüberschrift, um die *Views\Shared\\_Layout.cshtml* Datei.  
> 
> [!code-html[Main](aspnet-mvc-4-mobile-features/samples/sample16.html)]


Navigieren Sie zu der *AllTags* Seite in einen Desktopbrowser. Das ansichtumschaltung Widget wird nicht in einen Desktopbrowser angezeigt, da es nur mobile Layoutseite hinzugefügt wird. Später in diesem Lernprogramm sehen Sie, wie Sie die Desktopansicht ansichtumschaltung Widgets hinzufügen können.

[![p3_desktopBrowser](aspnet-mvc-4-mobile-features/_static/image29.png)](aspnet-mvc-4-mobile-features/_static/image28.png)

## <a name="improving-the-speakers-list"></a>Verbessern der Lautsprecher-Liste

Wählen Sie in der mobilen Browser die **Lautsprecher** Link. Da keine mobile Ansicht vorhanden ist (*AllSpeakers.Mobile.cshtml*), zeigen Sie die Standard-Lautsprecher (*AllSpeakers.cshtml*) wird mit der mobilen Layoutansicht gerendert ( *\_ Layout.Mobile.cshtml*).

[![p3_speakersDeskTop](aspnet-mvc-4-mobile-features/_static/image31.png)](aspnet-mvc-4-mobile-features/_static/image30.png)

Sie können eine Standardansicht für die (stationäre) Rendern in einem mobilen Layout global deaktivieren, indem `RequireConsistentDisplayMode` auf `true` in der *Ansichten\\Ansichten "_viewstart.cshtml"* -Datei wie folgt:

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample17.cshtml)]

Wenn `RequireConsistentDisplayMode` festgelegt ist, um `true`, mobile Layout (*\_Layout.Mobile.cshtml*) wird nur für mobilen Ansichten verwendet. (Die Ansichtsdatei ist also der Form ***ViewName**. Mobile.cshtml*.) Möglicherweise möchten Sie festlegen `RequireConsistentDisplayMode` zu `true` , wenn das Layout Ihren mobilen nicht gut mit Ansichten stationäre funktioniert. Der Screenshot unten zeigt wie die *Lautsprecher* Seite gerendert wird, wenn `RequireConsistentDisplayMode` festgelegt ist, um `true`.

[![p3_speakersConsistent](aspnet-mvc-4-mobile-features/_static/image33.png)](aspnet-mvc-4-mobile-features/_static/image32.png)

Sie können in einer Ansicht ein konsistenter Anzeigemodus deaktivieren, indem `RequireConsistentDisplayMode` zu `false` in der Datei anzeigen. Das folgende Markup in der *Views\Home\AllSpeakers.cshtml* Dateisätzen `RequireConsistentDisplayMode` auf `false`:

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample18.cshtml)]

## <a name="creating-a-mobile-speakers-view"></a>Erstellen einer mobilen Lautsprecher-Sicht

Wie Sie eben gesehen haben, die *Lautsprecher* Ansicht ist lesbar, aber die Links sind klein und sind schwer zu tippen Sie auf einem mobilen Gerät auf. In diesem Abschnitt erstellen Sie eine Mobile-spezifische *Lautsprecher* Ansicht sieht eine moderne mobile Anwendung – es große angezeigt wird, leicht Tap verknüpft und enthält ein Suchfeld Lautsprecher schnell zu finden.

Kopie *AllSpeakers.cshtml* auf *AllSpeakers.Mobile.cshtml*. Öffnen der *AllSpeakers.Mobile.cshtml* Datei, und entfernen Sie die `<h2>` Überschriftenelement.

In der `<ul>` zu kennzeichnen, fügen Sie der `data-role` Attribut, und legen Sie dessen Wert auf `listview`. Wie bei anderen [ `data-*` Attribute](http://html5doctor.com/html5-custom-data-attributes/), `data-role="listview"` macht die große Listenelemente einfacher, tippen Sie auf. Dies ist das vollständige Markup aussieht:

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample19.cshtml)]

Aktualisieren Sie den mobilen Browser an. Die aktualisierte Ansicht sieht wie folgt:

[![p3_updatedSpeakerView1](aspnet-mvc-4-mobile-features/_static/image35.png)](aspnet-mvc-4-mobile-features/_static/image34.png)

Obwohl die mobile Ansicht verbessert wurde, ist es schwierig, die lange Liste der Lautsprecher zu navigieren. Zur Behebung von diesem in die `<ul>` zu kennzeichnen, fügen Sie der `data-filter` Attribut, und legen Sie es auf `true`. Der folgende Code zeigt die `ul` Markup.

[!code-html[Main](aspnet-mvc-4-mobile-features/samples/sample20.html)]

Die folgende Abbildung zeigt das Suchfeld für die Filter am oberen Rand der Seite, die von der `data-filter` Attribut.

[![ps_Data_Filter](aspnet-mvc-4-mobile-features/_static/image37.png)](aspnet-mvc-4-mobile-features/_static/image36.png)

Wie Sie jedes Buchstabens im Suchfeld eingeben, filtert jQuery Mobile die angezeigte Liste aus, wie in der folgenden Abbildung dargestellt.

[![ps_data_filter_SC](aspnet-mvc-4-mobile-features/_static/image39.png)](aspnet-mvc-4-mobile-features/_static/image38.png)

## <a name="improving-the-tags-list"></a>Verbessern die Tag-Liste

Wie Sie die Standardeinstellung *Lautsprecher* anzeigen, die *Tags* Ansicht ist lesbar, aber die Links sind, klein und schwer zu tippen Sie auf einem mobilen Gerät auf. In diesem Abschnitt berichtigen Sie die *Tags* korrigierte genauso Anzeigen der *Lautsprecher* anzeigen.

Entfernen Sie die &quot;ausblenden&quot; -Suffix der *Views\Home\AllTags.Mobile.cshtml.hide* daher ist der Name der Datei *Views\Home\AllTags.Mobile.cshtml*. Öffnen Sie die umbenannte Datei, und entfernen Sie die `<h2>` Element.

Hinzufügen der `data-role` und `data-filter` -Attribute verwenden, um die `<ul>` zu kennzeichnen, wie hier gezeigt:

[!code-html[Main](aspnet-mvc-4-mobile-features/samples/sample21.html)]

Die folgende Abbildung zeigt die RFID-Transponder-Seite, die Filterung auf den Buchstaben `J`.

[![p3_tags_J](aspnet-mvc-4-mobile-features/_static/image41.png)](aspnet-mvc-4-mobile-features/_static/image40.png)

## <a name="improving-the-dates-list"></a>Verbessern die Liste der Datumsangaben

Sie verbessern, können die *Datumsangaben* anzuzeigen, wie Sie verbessert die *Lautsprecher* und *Tags* anzeigt, damit es einfacher, auf einem mobilen Gerät zu verwenden ist.

Kopieren der *Views\Home\AllDates.cshtml* Datei *Views\Home\AllDates.Mobile.cshtml*. Öffnen Sie die neue Datei, und entfernen Sie die `<h2>` Element.

Hinzufügen `data-role="listview"` auf die `<ul>` -Tag wie folgt:

[!code-html[Main](aspnet-mvc-4-mobile-features/samples/sample22.html)]

Die folgende Abbildung zeigt, was die **Datum** sieht Seite mit den `data-role` Attribut vorhanden.

[![p3_dates1](aspnet-mvc-4-mobile-features/_static/image43.png)](aspnet-mvc-4-mobile-features/_static/image42.png) ersetzen Sie den Inhalt von der *Views\Home\AllDates.Mobile.cshtml* Datei durch den folgenden Code:

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample23.cshtml)]

Dieser Code gruppiert alle Sitzungen nach Tagen an. Erstellt eine Liste Unterteilers für jeden neuen Tag, und listet alle Sitzungen für jeden Tag unter eine Trennlinie. Hier ist, sieht es aus, wenn dieser Code ausgeführt wird:

[![p3_dates2](aspnet-mvc-4-mobile-features/_static/image45.png)](aspnet-mvc-4-mobile-features/_static/image44.png)

## <a name="improving-the-sessionstable-view"></a>Verbessern der SessionsTable-Ansicht

In diesem Abschnitt erstellen Sie eine Mobile-spezifische Ansicht der Sitzungen. Die Änderungen, die wir treffen werden mehr so umfangreich wie in den anderen Ansichten wir erstellten.

Tippen Sie in der mobilen Browser auf die **Lautsprecher** Schaltfläche, und geben Sie dann `Sc` in das Suchfeld.

[![ps_data_filter_SC](aspnet-mvc-4-mobile-features/_static/image47.png)](aspnet-mvc-4-mobile-features/_static/image46.png)

Tippen Sie auf die **Scott Hanselman** Link.

[![p3_scottHa](aspnet-mvc-4-mobile-features/_static/image49.png)](aspnet-mvc-4-mobile-features/_static/image48.png)

Wie Sie sehen können, ist die Anzeige auf einem mobilen Browser gelesen schwierig. Die Date-Spalte ist schwer zu lesen, und die Tags-Spalte wird aus der Sicht. Kopieren Sie zur Beseitigung dieses Problems *Views\Home\SessionsTable.cshtml* auf *Views\Home\SessionsTable.Mobile.cshtml*, und Ersetzen Sie den Inhalt der Datei durch den folgenden Code:

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample24.cshtml)]

Der Code entfernt Raum und tags Spalten und den Titel, Lautsprecher und Datum vertikal so formatiert, dass alle diese Informationen auf einem mobilen Browser gelesen werden kann. Die folgende Abbildung gibt die codeänderungen wieder.

[![ps_SessionsByScottHa](aspnet-mvc-4-mobile-features/_static/image51.png)](aspnet-mvc-4-mobile-features/_static/image50.png)

## <a name="improving-the-sessionbycode-view"></a>Verbessern der SessionByCode-Ansicht

Schließlich erstellen Sie einen Überblick über Mobile-spezifische der *SessionByCode* anzeigen. Tippen Sie in der mobilen Browser auf die **Lautsprecher** Schaltfläche, und geben Sie dann `Sc` in das Suchfeld.

[![ps_data_filter_SC](aspnet-mvc-4-mobile-features/_static/image53.png)](aspnet-mvc-4-mobile-features/_static/image52.png)

Tippen Sie auf die **Scott Hanselman** Link. Scott Hanselman Sitzungen werden angezeigt.

[![ps_SessionsByScottHa](aspnet-mvc-4-mobile-features/_static/image55.png)](aspnet-mvc-4-mobile-features/_static/image54.png)

Wählen Sie die **eine Übersicht über den MS Web Stapel von diesbezüglich führen, schätzen** Link.

[![ps_love](aspnet-mvc-4-mobile-features/_static/image57.png)](aspnet-mvc-4-mobile-features/_static/image56.png)

Die Standardansicht für den desktop eignet sich gut, aber Sie können verbessern.

Kopieren der *Views\Home\SessionByCode.cshtml* auf *Views\Home\SessionByCode.Mobile.cshtml* und Ersetzen Sie den Inhalt von der *Views\Home\SessionByCode.Mobile.cshtml*Datei durch Folgendes Markup:

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample25.cshtml)]

Das neue Markup verwendet die `data-role` Attribut, um das Layout der Ansicht zu verbessern.

Aktualisieren Sie den mobilen Browser an. Die folgende Abbildung gibt die codeänderungen, die Sie gerade vorgenommen haben:

[![p3_love2](aspnet-mvc-4-mobile-features/_static/image59.png)](aspnet-mvc-4-mobile-features/_static/image58.png)

## <a name="wrapup-and-review"></a>Nachbereitung und Review

In diesem Lernprogramm wurde die neuen mobilen Funktionen von ASP.NET MVC 4 Developer Preview eingeführt. Die mobile-Features aufgeführt:

- Die Fähigkeit, Layout und Ansichten, Teilansichten, global und für eine einzelne Ansicht außer Kraft setzen.
- Kontrolle über das Layout und die partielle Außerkraftsetzung-Erzwingung verwenden die `RequireConsistentDisplayMode` Eigenschaft.
- Ansichtumschaltung Widgets für mobile Geräte Sichten als auch in desktop-Ansichten angezeigt werden können.
- Unterstützung für bestimmte Browsern, z. B. der iPhone-Browser unterstützen.

## <a name="see-also"></a>Siehe auch

- [jQuery Mobile](http://jquerymobile.com) Standort.
- [jQuery Mobile (Übersicht)](http://jquerymobile.com/demos/1.0b3/docs/about/intro.html)
- [W3C-Empfehlung Mobile Web Application bewährte Methoden](http://www.w3.org/TR/mwabp/)
- [W3C Candidate Recommendation für Media-Abfragen](http://www.w3.org/TR/css3-mediaqueries/)
