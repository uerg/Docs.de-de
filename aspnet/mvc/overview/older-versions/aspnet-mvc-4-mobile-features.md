---
uid: mvc/overview/older-versions/aspnet-mvc-4-mobile-features
title: Mobile ASP.NET MVC 4-Features | Microsoft-Dokumentation
author: Rick-Anderson
description: Es gibt jetzt eine MVC 5-Version dieses Tutorials mit Codebeispielen, unter dem Bereitstellen einer ASP.NET MVC 5 Mobile-Webanwendung auf Azure-Websites.
ms.author: riande
ms.date: 08/15/2012
ms.assetid: 27dc4fc8-1b51-43b0-933f-fc1b52476523
msc.legacyurl: /mvc/overview/older-versions/aspnet-mvc-4-mobile-features
msc.type: authoredcontent
ms.openlocfilehash: 8b82b8b9b1ee6646072931da889c643afb34d474
ms.sourcegitcommit: 7b4e3936feacb1a8fcea7802aab3e2ea9c8af5b4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/04/2018
ms.locfileid: "48578158"
---
<a name="aspnet-mvc-4-mobile-features"></a>ASP.NET MVC 4-Funktionen für mobile Geräte
====================
durch [Rick Anderson]((https://twitter.com/RickAndMSFT))

> Es gibt jetzt eine MVC 5-Version dieses Tutorials mit Codebeispielen, unter [Bereitstellen einer ASP.NET MVC 5 Mobile-Webanwendung auf Azure-Websites](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/).


In diesem Tutorial lernen Sie die Grundlagen der Verwendung mobiler Funktionen in einer ASP.NET MVC 4-Web-Anwendung. In diesem Tutorial können Sie [Visual Studio Express 2012](https://www.microsoft.com/visualstudio/11/products/express) oder Visual Web Developer 2010 Express Service Pack 1 (&quot;Visual Web Developer oder VWD&quot;). Sie können die professional-Version von Visual Studio verwenden, wenn Sie bereits vorhanden.

Bevor Sie beginnen, stellen Sie sicher, dass Sie die unten aufgeführten erforderlichen Komponenten installiert haben.

- [Visual Studio Express 2012](https://www.microsoft.com/visualstudio/11/products/express) (empfohlen) oder Visual Studio Web Developer Express SP1. Visual Studio 2012 enthält die ASP.NET MVC 4. Wenn Sie Visual Web Developer 2010 verwenden, müssen Sie installieren [ASP.NET MVC 4](https://go.microsoft.com/fwlink/?LinkId=243392).

Sie benötigen auch einen Emulator für mobile Browser. Eine der folgenden funktioniert:

- [Windows 7, Windows Phone-Emulator](https://msdn.microsoft.com/library/ff402563(VS.92).aspx). (Dies ist der Emulator, der in den meisten die Screenshots in diesem Tutorial verwendet wird.)
- Ändern Sie die Benutzer-Agent-Zeichenfolge, um ein iPhone zu emulieren. Finden Sie unter [dies](http://www.howtogeek.com/113439/how-to-change-your-browsers-user-agent-without-installing-any-extensions/) Blogeintrag.
- [Opera Mobile-Emulator](http://www.opera.com/developer/tools/mobile/)
- [Apple Safari](http://www.apple.com/safari/download/) mit Benutzer-Agent auf dem iPhone festgelegt. Anweisungen dazu, wie Sie den Benutzer-Agent in Safari "iPhone" festlegen, finden Sie unter [Vorgehensweise können Sie Safari nehmen, es handelt sich um Internet Explorer](http://www.davidalison.com/2008/05/how-to-let-safari-pretend-its-ie.html) David Alison Blog.

Visual Studio-Projekte mit C#-Quellcode sind zur Ergänzung dieses Themas verfügbar:

- [Herunterladen eines starterprojekts](https://go.microsoft.com/fwlink/?linkid=228307&amp;clcid=0x409)
- [Herunterladen eines fertigen Projekts](https://go.microsoft.com/fwlink/?linkid=228306&amp;clcid=0x409)

### <a name="what-youll-build"></a>Sie lernen Folgendes

In diesem Tutorial fügen Sie mobile Funktionen für die einfache Auflistung von Konferenzdaten-Anwendung, die in bereitgestellt wird die [Startprojekt](https://go.microsoft.com/fwlink/?LinkId=228307). Der folgende Screenshot zeigt die Seite "Tags", der fertigen Anwendung, wie in der [Phone-Emulator für Windows 7](https://msdn.microsoft.com/library/ff402563(VS.92).aspx). Finden Sie unter [Tastatur Zuordnung für Windows Phone-Emulator](https://msdn.microsoft.com/library/ff754352(v=vs.92).aspx) um Tastatureingaben zu vereinfachen.

[![p1_Tags_CompletedProj](aspnet-mvc-4-mobile-features/_static/image2.png)](aspnet-mvc-4-mobile-features/_static/image1.png)

Sie können Internet Explorer Version 9 oder 10, FireFox oder Chrome zum Entwickeln einer mobilen Anwendung durch Festlegen der [Benutzeragentzeichenfolge](http://www.howtogeek.com/113439/how-to-change-your-browsers-user-agent-without-installing-any-extensions/). Die folgende Abbildung zeigt das vollständige Tutorial mithilfe von Internet Explorer, um ein iPhone emulieren. Sie können die F-12 für Internet Explorer-Entwicklertools verwenden und die [Fiddler-Tool](http://www.fiddler2.com/fiddler2/) zum Debuggen Ihrer Anwendung.

![](aspnet-mvc-4-mobile-features/_static/image3.png)

### <a name="skills-youll-learn"></a>Fähigkeiten, mit denen, die Sie lernen Folgendes

Hier ist Sie lernen Folgendes:

- Verwenden Sie die ASP.NET MVC 4-Vorlagen wie HTML5 `viewport` -Attribut und adaptive Rendering zur Verbesserung der anzeigen, auf mobilen Geräten.
- Vorgehensweise: Erstellen von Ansichten.
- So erstellen Sie eine ansichtumschaltung, können Benutzer Umschalten zwischen der mobilen und desktop einen Überblick über die Anwendung.

### <a name="getting-started"></a>Erste Schritte

Laden Sie die Auflistung von Konferenzdaten-Anwendung für das Startprojekt über den folgenden Link: [herunterladen](https://go.microsoft.com/fwlink/?LinkId=228307). Klicken Sie dann im Windows-Explorer mit der Maustaste der *MvcMobile.zip* Datei, und wählen **Eigenschaften**. In der **MvcMobile.zip Eigenschaften** Dialogfeld auf die **Unblock** Schaltfläche. (Aufheben der Sperre wird verhindert, dass eine sicherheitswarnung angezeigt, das auftritt, wenn Sie versuchen, eine *ZIP* -Datei, die Sie aus dem Web heruntergeladen haben.)

![p1_unBlock](aspnet-mvc-4-mobile-features/_static/image4.png)

Mit der rechten Maustaste die *MvcMobile.zip* und wählen Sie **alle extrahieren** auf die Datei zu entpacken. Öffnen Sie in Visual Studio die *MvcMobile.sln* Datei.

Drücken Sie STRG + F5, um die Anwendung auszuführen, die sie in Ihrer desktop-Browser angezeigt wird. Starten Sie Ihre Emulator für mobile Browser, kopieren Sie die URL für die konferenzanwendung in den Emulator und klicken Sie dann auf die **durchsuchen nach Markierung** Link. Wenn Sie den Windows Phone-Emulator verwenden, klicken Sie in der URL-Leiste auf, und drücken Sie die Pause-Taste, um den Zugriff zu erhalten. Die folgende Abbildung zeigt die *AllTags* anzeigen (Wahl **durchsuchen nach Markierung**).

[![p1_browseTag](aspnet-mvc-4-mobile-features/_static/image6.png)](aspnet-mvc-4-mobile-features/_static/image5.png)

Die Anzeige ist auf einem mobilen Gerät sehr gut lesbar. Wählen Sie den Link für ASP.NET.

[![p1_tagged_ASPNET](aspnet-mvc-4-mobile-features/_static/image8.png)](aspnet-mvc-4-mobile-features/_static/image7.png)

Die ASP.NET-Tag-Ansicht ist sehr überladen. Z. B. die **Datum** Spalte sehr schwer zu lesen ist. Später in diesem Tutorial erstellen Sie eine Version der *AllTags* anzeigen, die speziell für mobile Browser verfügbar ist und wird, die die Anzeige lesbar machen.

Hinweis: Ein Fehler wird in der mobilen caching-Engine derzeit vorhanden ist. Für Produktionsanwendungen müssen Sie installieren die [festen DisplayModes](http://nuget.org/packages/Microsoft.AspNet.Mvc.FixedDisplayModes) NuGet-Paket. Finden Sie unter [ASP.NET MVC 4 Mobile Zwischenspeichern Fehler festen](https://blogs.msdn.com/b/rickandy/archive/2012/09/17/asp-net-mvc-4-mobile-caching-bug-fixed.aspx) Details für diese Korrektur.

## <a name="css-media-queries"></a>CSS-Medienabfragen

[CSS-Medienabfragen](http://www.w3.org/TR/css3-mediaqueries/) sind eine Erweiterung zu CSS-Medientypen. Sie ermöglichen Ihnen die Erstellung von Regeln, die die Standard-CSS-Regeln für bestimmte Browser (Benutzer-Agents) zu überschreiben. Allgemeine Faustregel für CSS, die mobile Browser ausgerichtet ist, wird die maximale Bildschirmgröße definieren. Die *Content\Site.css* -Datei, die erstellt wird, wenn Sie ein neues ASP.NET MVC 4-Internet-Projekt erstellen, enthält die folgende Medienabfrage:

[!code-css[Main](aspnet-mvc-4-mobile-features/samples/sample1.css)]

Wenn das Browserfenster 850 Pixel breit oder weniger ist, wird die CSS-Regeln innerhalb dieses Blocks Medien verwendet. Sie können die CSS-Medienabfragen wie folgt bereitgestellt, eine bessere Darstellung der HTML-Inhalt in kleine Browser (z. B. mobile Browser) als die Standard-CSS-Regeln, die entwickelt wurden für die größere Anzeige von Desktopbrowsern verwenden.

## <a name="the-viewport-meta-tag"></a>Die Viewport-Meta-Tag

Die meisten mobilen Browser definieren die Breite einer virtuellen Browser-Fensters (der *Viewport*), ist viel größer als die tatsächliche Breite des mobilen Geräts. Dies ermöglicht mobile Browsern an die gesamte Webseite in die virtuelle Anzeige anpassen. Benutzer können dann interessante Inhalte vergrößern. Jedoch wenn Sie die Breite des Viewports auf die tatsächlichen Gerätebreite festlegen, ist keine Zoomen erforderlich, da der Inhalt im mobilen Browser passt.

Der Viewport `<meta>` Tag in der ASP.NET MVC 4 Layoutdatei des Viewports auf die Gerätebreite festgelegt. Die folgende Zeile zeigt den Viewport `<meta>` Tag in der ASP.NET MVC 4 Layoutdatei.

[!code-html[Main](aspnet-mvc-4-mobile-features/samples/sample2.html)]

## <a name="examining-the-effect-of-css-media-queries-and-the-viewport-meta-tag"></a>Untersuchen des Effekts von CSS-Medienabfragen und des Viewports Meta-Tags

Öffnen der *Views\Shared\\"_Layout.cshtml"* Datei im Editor, und kommentieren Sie den Viewport `<meta>` Tag. Das folgende Markup zeigt die auskommentierte Zeile.

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample3.cshtml)]

Öffnen der *MvcMobile\Content\Site.css* Datei im Editor, und ändern Sie die maximale Breite in der Medienabfrage in NULL Pixel. Dadurch wird verhindert, dass der CSS-Regeln in mobilen Browsern verwendet wird. Die folgende Zeile zeigt die geänderte Medienabfrage:

[!code-css[Main](aspnet-mvc-4-mobile-features/samples/sample4.css)]

Speichern Sie die Änderungen zu, und rufen Sie die Konferenz-Anwendung in einem Emulator für mobile Browser. Der kleine Text in der folgenden Abbildung ist das Ergebnis des Entfernens des Viewports `<meta>` Tag. Mit keinen Viewport verfügen `<meta>` -Tag, der Browser ist verkleinern, die Breite des Viewports (850 Pixel oder für die meisten mobilen Browser breiter.)

[![p1_noViewPort](aspnet-mvc-4-mobile-features/_static/image10.png)](aspnet-mvc-4-mobile-features/_static/image9.png)

Die Änderungen rückgängig machen, heben Sie die auskommentierung des Viewports `<meta>` in der Layoutdatei markieren und Wiederherstellen der Medienabfrage 850 Pixeln in der *"Site.CSS"* Datei. Speichern Sie die Änderungen zu und aktualisieren Sie den mobilen Browser, um sicherzustellen, dass die für Mobiltelefone optimierte Anzeige wiederhergestellt wurde.

Der Viewport `<meta>` Tag und der CSS-Medienabfrage sind nicht spezifisch für ASP.NET MVC 4, und profitieren Sie von diesen Funktionen in einer beliebigen Webanwendung. Aber sie sind jetzt integriert die Dateien, die generiert werden, wenn Sie ein neues ASP.NET MVC 4-Projekt erstellen.

Weitere Informationen zu den Viewport `<meta>` markieren, finden Sie unter [– eine Geschichte zwei Ansichten – Teil 2](http://www.quirksmode.org/mobile/viewports2.html).

Im nächsten Abschnitt sehen Sie, wie Sie bestimmte Sichten für Mobile Browser bereitstellen.

## <a name="overriding-views-layouts-and-partial-views"></a>Überschreiben der Ansichten, Layouts und Teilansichten

Ein wichtige neues Feature in ASP.NET MVC 4 ist es sich um ein einfaches Verfahren, das einer beliebigen Ansicht (einschließlich Layouts und Teilansichten) außer Kraft setzen für mobile Browser im Allgemeinen für einzelne mobile Browser oder für einen beliebigen spezifischen Browser ermöglicht. Um eine mobiltaugliche Ansicht zu ermöglichen, können Sie eine Ansichtsdatei kopieren und fügen *. Mobile* an den Dateinamen an. Um beispielsweise erstellen Sie eine Mobile *Index* anzeigen, kopieren Sie *Views\Home\Index.cshtml* zu *Views\Home\Index.Mobile.cshtml*.

In diesem Abschnitt erstellen Sie eine mobiltaugliche Layoutdatei.

Kopieren Sie zunächst *Views\Shared\\"_Layout.cshtml"* zu *Views\Shared\\_Layout.Mobile.cshtml*. Open  *\_Layout.Mobile.cshtml* , und ändern Sie den Titel von **MVC4-Konferenz** zu **Conference (Mobil)**.

In den einzelnen `Html.ActionLink` aufrufen, entfernen Sie "Durchsuchen nach" in jedem *ActionLink*. Der folgende Code zeigt die abgeschlossene Textabschnitt mobilen Layoutdatei.

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample5.cshtml)]

Kopieren der *Views\Home\AllTags.cshtml* Datei *Views\Home\AllTags.Mobile.cshtml*. Öffnen Sie die neue Datei, und ändern Sie die `<h2>` Element von "Tags", "Tags (M)":

[!code-html[Main](aspnet-mvc-4-mobile-features/samples/sample6.html)]

Navigieren Sie zu der Seite "Tags", verwenden einen Desktopbrowser und Verwenden von Emulator für mobile Browser. Der Emulator für mobile Browser zeigt die beiden Änderungen vornehmen, die Sie vorgenommen.

[![p2m_layoutTags.mobile](aspnet-mvc-4-mobile-features/_static/image12.png)](aspnet-mvc-4-mobile-features/_static/image11.png)

Im Gegensatz dazu wurde die Desktopanzeige nicht geändert werden.

[![p2_layoutTagsDesktop](aspnet-mvc-4-mobile-features/_static/image14.png)](aspnet-mvc-4-mobile-features/_static/image13.png)

## <a name="browser-specific-views"></a>Ansichten für spezielle Browser

Zusätzlich zu den Ansichten speziell für Mobile und Desktop-spezifische können Sie Ansichten für individuelle Browser erstellen. Beispielsweise können Sie Ansichten erstellen, die speziell für den iPhone-Browser sind. In diesem Abschnitt erstellen Sie ein Layout für den iPhone-Browser und eine iPhone-Version, der die *AllTags* anzeigen.

Öffnen der *"Global.asax"* Datei, und fügen Sie den folgenden Code der `Application_Start` Methode.

[!code-csharp[Main](aspnet-mvc-4-mobile-features/samples/sample7.cs)]

Dieser Code definiert einen neue Anzeigemodus "iPhone", die mit jeder eingehenden Anforderung abgeglichen werden. Wenn die eingehende Anforderung der Bedingung entspricht, die Sie definiert haben (d.h., wenn der Benutzer-Agent die Zeichenfolge "iPhone" enthält), sucht ASP.NET MVC für Ansichten, deren Name das Suffix "iPhone" enthält.

In den Code, mit der Maustaste `DefaultDisplayMode`, wählen Sie **beheben**, und wählen Sie dann `using System.Web.WebPages;`. Dies fügt einen Verweis auf die `System.Web.WebPages` -Namespace, der ist, die `DisplayModes` und `DefaultDisplayMode` Typen definiert sind.

[![p2_resolve](aspnet-mvc-4-mobile-features/_static/image16.png)](aspnet-mvc-4-mobile-features/_static/image15.png)

Alternativ können Sie manuell die folgende Zeile zum Hinzufügen der `using` -Abschnitt der Datei.

[!code-csharp[Main](aspnet-mvc-4-mobile-features/samples/sample8.cs)]

Der vollständige Inhalt der dem *"Global.asax"* Datei ist unten dargestellt.

[!code-csharp[Main](aspnet-mvc-4-mobile-features/samples/sample9.cs)]

Speichern Sie die Änderungen. Kopieren der *MvcMobile\Views\Shared\\_Layout.Mobile.cshtml* Datei *MvcMobile\Views\Shared\\_Layout.iPhone.cshtml*. Öffnen Sie die neue Datei, und ändern Sie dann die `h1` Überschrift von `Conference (Mobile)` zu `Conference (iPhone)`.

Kopieren der *MvcMobile\Views\Home\AllTags.Mobile.cshtml* Datei *MvcMobile\Views\Home\AllTags.iPhone.cshtml*. Ändern Sie in der neuen Datei den `<h2>` Element von "Tags (M)", "Tags (iPhone)".

Führen Sie die Anwendung aus. Führen Sie einen Emulator für mobile Browser, stellen Sie sicher, dass die Benutzer-Agent auf "iPhone" festgelegt ist, und navigieren Sie zu der *AllTags* anzeigen. Der folgende Screenshot zeigt die *AllTags* Ansicht gerendert, der [Safari](http://www.apple.com/safari/download/) Browser. Sie können auch Safari für Windows herunterladen [hier](https://support.apple.com/kb/DL1531).

[![p2_iphoneView](aspnet-mvc-4-mobile-features/_static/image18.png)](aspnet-mvc-4-mobile-features/_static/image17.png)

In diesem Abschnitt haben wir gesehen, wie Sie mobile Layouts und Ansichten erstellen sowie zum Erstellen von Layouts und Ansichten für bestimmte Geräte wie das iPhone. Im nächsten Abschnitt sehen Sie, wie Sie jQuery Mobile für weitere ansprechenden mobilen Ansichten nutzen können.

## <a name="using-jquery-mobile"></a>Mithilfe von jQuery Mobile

Die [jQuery Mobile](http://jquerymobile.com/demos/1.0b3/#/demos/1.0b3/docs/about/intro.html) -Bibliothek bietet ein Benutzeroberflächen-Framework, die auf die alle wichtigen mobile Browsern funktioniert. jQuery Mobile gilt *progressive Verbesserung* für mobile Browser, die CSS- und JavaScript zu unterstützen. Progressive Verbesserung ermöglicht allen Browsern, die grundlegende Inhalte von einer Webseite angezeigt wird, gleichzeitig leistungsfähigere Browsern und Geräten, um eine umfangreichere Anzeige zu erhalten. Die JavaScript und CSS-Dateien, die mit jQuery Mobile enthalten sind die vielen Stilelemente mobile Browser anpassen, ohne dass Markupänderungen.

In diesem Abschnitt Installieren Sie die *jQuery.Mobile.MVC* NuGet-Paket, das jQuery Mobile und eine ansichtumschaltung Widget installiert.

Löschen Sie zum Starten der *Shared\\_Layout.Mobile.cshtml* und *Shared\\_Layout.iPhone.cshtml* Dateien, die Sie zuvor erstellt haben.

Benennen Sie *Views\Home\AllTags.Mobile.cshtml* und *Views\Home\AllTags.iPhone.cshtml* Dateien *Views\Home\AllTags.iPhone.cshtml.hide* und  *Views\Home\AllTags.Mobile.cshtml.hide*. Da die Dateien nicht mehr müssen eine *cshtml* -Erweiterung, sie wird nicht von der ASP.NET MVC-Laufzeit zum Rendern verwendet werden die *AllTags* anzeigen.

Installieren Sie die *jQuery.Mobile.MVC* NuGet-Paket auf diese Weise:

1. Von der **Tools** , wählen Sie im Menü **Bibliothekspaket-Manager**, und wählen Sie dann **-Paket-Manager-Konsole**.

    [![p3_packageMgr](aspnet-mvc-4-mobile-features/_static/image20.png)](aspnet-mvc-4-mobile-features/_static/image19.png)
2. In der **-Paket-Manager-Konsole**, geben Sie `Install-Package jQuery.Mobile.MVC -version 1.0.0`

Die folgende Abbildung zeigt die Dateien hinzugefügt und auf das Projekt MvcMobile vom NuGet-Paket jQuery.Mobile.MVC geändert. Dateien, die hinzugefügt werden [add] nach dem Dateinamen angefügt wurden. Das Bild nicht das GIF-Bild angezeigt, und PNG-Dateien hinzugefügt, um die *Content\images* Ordner.

![](aspnet-mvc-4-mobile-features/_static/image21.png)

Das jQuery.Mobile.MVC NuGet-Paket installiert die folgenden:

- Die *App\_Start\BundleMobileConfig.cs* -Datei, die erforderlich ist, auf die jQuery JavaScript und CSS-Dateien hinzugefügt. Sie führen Sie die nachstehenden Anweisungen, und verweisen auf das mobile Paket, das in dieser Datei definierten.
- jQuery Mobile-CSS-Dateien.
- Ein `ViewSwitcher` Controller-Widget (*Controllers\ViewSwitcherController.cs*).
- jQuery Mobile JavaScript-Dateien.
- Ein jQuery Mobile formatiertes Layoutdatei (*Views\Shared\\_Layout.Mobile.cshtml*).
- Eine Teilansicht ansichtumschaltung *(MvcMobile\Views\Shared\\_ViewSwitcher.cshtml*), der einen Link am oberen Rand jeder Seite, um von desktop zur mobilen Ansicht und umgekehrt wechseln bereitstellt.
- Mehrere<em>PNG</em> und <em>GIF</em> Bilddateien in den <em>Content\images</em> Ordner.

Öffnen der *"Global.asax"* Datei, und fügen Sie den folgenden Code, als die letzte Zeile der `Application_Start` Methode.

[!code-csharp[Main](aspnet-mvc-4-mobile-features/samples/sample10.cs)]

Der folgende Code zeigt die vollständige *"Global.asax"* Datei.

[!code-csharp[Main](aspnet-mvc-4-mobile-features/samples/sample11.cs?highlight=26)]

> [!NOTE]
> Wenn Sie Internet Explorer 9 verwenden und nicht angezeigt der `BundleMobileConfig` Zeile oben in der gelbe Hervorhebung, klicken Sie auf die [Schaltfläche](https://windows.microsoft.com/windows7/How-to-use-Compatibility-View-in-Internet-Explorer-9)![Überblick über die Schaltfläche (deaktiviert)](http://res2.windows.microsoft.com/resbox/en/Windows 7/main/f080e77f-9b66-4ac8-9af0-803c4f8a859c_15.jpg " Schaltfläche \"Kompatibilitätsansicht\" (deaktiviert)") in Internet Explorer auf das Symbol, ändern Sie in einer Gliederung vornehmen ![Überblick über die Schaltfläche (deaktiviert)](http://res2.windows.microsoft.com/resbox/en/Windows 7/main/f080e77f-9b66-4ac8-9af0-803c4f8a859c_15.jpg "Überblick über die Schaltfläche (deaktiviert) ") auf eine Volltonfarbe ![(Anmelden) Schaltfläche "Kompatibilitätsansicht"](http://res1.windows.microsoft.com/resbox/en/Windows 7/main/156805ff-3130-481b-a12d-4d3a96470f36_14.jpg "(Anmelden) Schaltfläche \"Kompatibilitätsansicht\""). Alternativ können Sie in diesem Tutorial in FireFox oder Chrome anzeigen.


Öffnen der *MvcMobile\Views\Shared\\_Layout.Mobile.cshtml* Datei, und fügen Sie das folgende Markup direkt nach der `Html.Partial` aufrufen:

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample12.cshtml)]

Die vollständige *MvcMobile\Views\Shared\\_Layout.Mobile.cshtml* Datei ist unten dargestellt:

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample13.cshtml)]

Erstellen Sie die Anwendung, und navigieren Sie in Ihrem Emulator für mobile Browser zu den *AllTags* anzeigen. Sie finden hier:

[![p3_afterNuGet](aspnet-mvc-4-mobile-features/_static/image23.png)](aspnet-mvc-4-mobile-features/_static/image22.png)

> [!NOTE]
> Sie können bestimmten mobilen Code durch Debuggen [Festlegen der Zeichenfolge des Benutzer-Agenten](http://www.howtogeek.com/113439/how-to-change-your-browsers-user-agent-without-installing-any-extensions/) für Internet Explorer oder Chrome auf iPhone- und klicken Sie dann mit den F-12 Developer-Tools. Wenn es sich bei Ihrem mobilen Browser nicht angezeigt wird der **Startseite**, **Redner**, **Tag**, und **Datum** Links als Schaltflächen, die Verweise auf jQuery Mobile Skripts und CSS-Dateien sind möglicherweise nicht richtig.


Zusätzlich zu den Änderungen an Formatvorlagen daraufhin **Anzeige der mobilen Ansicht** und einen Link mit der Sie die von mobile zur Desktopansicht wechseln kann. Wählen Sie die **Desktopansicht** Link und Desktopansicht angezeigt wird.

[![p3_desktopView](aspnet-mvc-4-mobile-features/_static/image25.png)](aspnet-mvc-4-mobile-features/_static/image24.png)

Desktopansicht stellt eine Möglichkeit, navigieren Sie direkt an die mobile Ansicht bereit. Sie soll nun behoben werden. Öffnen der *Views\Shared\\"_Layout.cshtml"* Datei. Direkt unter der Seite `body` -Element, fügen Sie den folgenden Code, der ansichtumschaltung Widget gerendert wird:

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample14.cshtml)]

Aktualisieren Sie die *AllTags* Ansicht im mobilen Browser. Sie können nun zwischen Desktop- und mobile Ansichten navigieren.

[![p3_desktopViewWithMobileLink](aspnet-mvc-4-mobile-features/_static/image27.png)](aspnet-mvc-4-mobile-features/_static/image26.png)

> [!NOTE]
> Hinweis zu debuggen: können Sie den folgenden Code hinzufügen, am Ende der Views\Shared\\_ViewSwitcher.cshtml um Ansichten zu debuggen, wenn auf einem mobilen Gerät mit einem Browser der Benutzer-Agent-Zeichenfolge festgelegt werden.
> 
> [!code-csharp[Main](aspnet-mvc-4-mobile-features/samples/sample15.cs)]
> 
>  und das Hinzufügen des folgenden Abschnitts aus, um die *Views\Shared\\"_Layout.cshtml"* Datei.  
> 
> [!code-html[Main](aspnet-mvc-4-mobile-features/samples/sample16.html)]


Navigieren Sie zu der *AllTags* Seite in einem Desktopbrowser. Das ansichtumschaltung Widget wird nicht in einem Desktopbrowser angezeigt, da es nur auf die Seite mobiles Layout hinzugefügt wird. Später in diesem Tutorial sehen Sie, wie Sie Desktopansicht ansichtumschaltung Widgets hinzufügen können.

[![p3_desktopBrowser](aspnet-mvc-4-mobile-features/_static/image29.png)](aspnet-mvc-4-mobile-features/_static/image28.png)

## <a name="improving-the-speakers-list"></a>Verbessern der Speakers-Liste

Wählen Sie im mobilen Browser den **Lautsprecher** Link. Da es keine mobile Ansicht ist (*AllSpeakers.Mobile.cshtml*), die Speakers anzuzeigen (*AllSpeakers.cshtml*) gerendert wird, mit der mobilen Layoutansicht ( *\_ Layout.Mobile.cshtml*).

[![p3_speakersDeskTop](aspnet-mvc-4-mobile-features/_static/image31.png)](aspnet-mvc-4-mobile-features/_static/image30.png)

Sie können eine standardmäßige (nicht Mobile) Ansicht vom Rendering in einem mobilen Layout global deaktivieren, indem `RequireConsistentDisplayMode` zu `true` in die *Ansichten\\_ViewStart.cshtml* Datei wird wie folgt:

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample17.cshtml)]

Wenn `RequireConsistentDisplayMode` nastaven NA hodnotu `true`, das mobile Layout (<em>\_Layout.Mobile.cshtml</em>) wird nur für mobile Ansichten verwendet. (Die Ansichtsdatei wird im Format <em>** ViewName</em><em>. Mobile.cshtml</em>.) Möglicherweise möchten Sie festlegen `RequireConsistentDisplayMode` zu `true` sollte Ihr mobile Layout nicht gut mit den nicht mobilen Ansichten funktionieren. Der folgende Screenshot zeigt die <em>Lautsprecher</em> Seite gerendert wird, wenn `RequireConsistentDisplayMode` nastaven NA hodnotu `true`.

[![p3_speakersConsistent](aspnet-mvc-4-mobile-features/_static/image33.png)](aspnet-mvc-4-mobile-features/_static/image32.png)

Sie können konsistenten Anzeigemodus in einer Ansicht deaktivieren, indem `RequireConsistentDisplayMode` zu `false` in der Datei anzeigen. Das folgende Markup in der *Views\Home\AllSpeakers.cshtml* legt `RequireConsistentDisplayMode` zu `false`:

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample18.cshtml)]

## <a name="creating-a-mobile-speakers-view"></a>Erstellen einer mobilen Speakers-Ansicht

Wie Sie gerade gesehen haben, die *Lautsprecher* Ansicht ist lesbar, aber die Links sind klein und sind schwer zu tippen Sie auf einem mobilen Gerät auf. In diesem Abschnitt erstellen Sie eine Mobile-spezifische *Lautsprecher* Ansicht sieht eine moderne mobile Anwendung – es zeigt große, einfach anzutippende links und ein Suchfeld zum schnellen Auffinden der Speakers enthält.

Kopie *AllSpeakers.cshtml* zu *AllSpeakers.Mobile.cshtml*. Öffnen der *AllSpeakers.Mobile.cshtml* Datei, und entfernen Sie die `<h2>` Überschriftenelement.

In der `<ul>` markieren, fügen Sie der `data-role` Attribut, und setzen ihren Wert auf `listview`. Wie bei anderen [ `data-*` Attribute](http://html5doctor.com/html5-custom-data-attributes/), `data-role="listview"` macht die große Listenelemente einfacher, tippen Sie auf. Dies ist das vollständige Markup aussieht:

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample19.cshtml)]

Aktualisieren Sie den mobilen Browser an. Die aktualisierte Ansicht sieht folgendermaßen aus:

[![p3_updatedSpeakerView1](aspnet-mvc-4-mobile-features/_static/image35.png)](aspnet-mvc-4-mobile-features/_static/image34.png)

Obwohl die mobile Ansicht optimiert wurde, ist es schwierig, die lange Liste der Sprecher zu navigieren. Um dies zu korrigieren, in der `<ul>` markieren, fügen Sie der `data-filter` Attribut, und legen Sie ihn auf `true`. Der folgende Code zeigt die `ul` Markup.

[!code-html[Main](aspnet-mvc-4-mobile-features/samples/sample20.html)]

Die folgende Abbildung zeigt das Suchfeld für die Filter am oberen Rand der Seite, die aus der `data-filter` Attribut.

[![ps_Data_Filter](aspnet-mvc-4-mobile-features/_static/image37.png)](aspnet-mvc-4-mobile-features/_static/image36.png)

Wenn Sie jeden Buchstaben in das Suchfeld eingeben, filtert jQuery Mobile die angezeigte Liste aus, wie in der folgenden Abbildung dargestellt.

[![ps_data_filter_SC](aspnet-mvc-4-mobile-features/_static/image39.png)](aspnet-mvc-4-mobile-features/_static/image38.png)

## <a name="improving-the-tags-list"></a>Verbessern der Tags-Liste

Wie der vordefinierte *Lautsprecher* anzeigen, die *Tags* Ansicht ist lesbar, aber die Links sind klein und schwierig, tippen Sie auf einem mobilen Gerät auf. In diesem Abschnitt, beheben Sie die *Tags* anzeigen die gleiche Weise, wie Sie behoben die *Lautsprecher* anzeigen.

Entfernen der &quot;ausblenden&quot; -Suffix zu den *Views\Home\AllTags.Mobile.cshtml.hide* ist der Name der Datei *Views\Home\AllTags.Mobile.cshtml*. Öffnen Sie die umbenannte Datei und entfernen Sie die `<h2>` Element.

Hinzufügen der `data-role` und `data-filter` Attribute der `<ul>` zu markieren, wie hier gezeigt:

[!code-html[Main](aspnet-mvc-4-mobile-features/samples/sample21.html)]

Die folgende Abbildung zeigt die Seite "Tags" Filtern nach dem Buchstaben `J`.

[![p3_tags_J](aspnet-mvc-4-mobile-features/_static/image41.png)](aspnet-mvc-4-mobile-features/_static/image40.png)

## <a name="improving-the-dates-list"></a>Verbessern der Dates-Liste

Können Sie verbessern die *Datumsangaben* anzeigen, wie die *Lautsprecher* und *Tags* anzeigt, sodass sie einfacher, auf einem mobilen Gerät zu verwenden ist.

Kopieren der *Views\Home\AllDates.cshtml* Datei *Views\Home\AllDates.Mobile.cshtml*. Öffnen Sie die neue Datei und entfernen Sie die `<h2>` Element.

Hinzufügen `data-role="listview"` auf die `<ul>` Tag wie folgt:

[!code-html[Main](aspnet-mvc-4-mobile-features/samples/sample22.html)]

Die folgende Abbildung zeigt, was die **Datum** sieht Seite mit den `data-role` Attribut vorhanden.

[![p3_dates1](aspnet-mvc-4-mobile-features/_static/image43.png)](aspnet-mvc-4-mobile-features/_static/image42.png) ersetzen den Inhalt von der *Views\Home\AllDates.Mobile.cshtml* -Datei mit den folgenden Code:

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample23.cshtml)]

Dieser Code werden alle Sitzungen nach Tagen gruppiert. Für jeden neuen Tag einen Unterteiler Liste erstellt, und listet alle Sitzungen für jeden Tag unter einen Unterteiler. So sieht es wie bei dieser Code ausgeführt wird:

[![p3_dates2](aspnet-mvc-4-mobile-features/_static/image45.png)](aspnet-mvc-4-mobile-features/_static/image44.png)

## <a name="improving-the-sessionstable-view"></a>Verbessern der SessionsTable-Ansicht

In diesem Abschnitt erstellen Sie eine mobiltaugliche Ansicht von Sitzungen. Die Änderungen vornehmen, wir werden umfangreichere als in den anderen Ansichten, die wir erstellt haben.

Tippen Sie in den mobilen Browser, auf die **Redner** Schaltfläche, und geben Sie dann `Sc` in das Suchfeld.

[![ps_data_filter_SC](aspnet-mvc-4-mobile-features/_static/image47.png)](aspnet-mvc-4-mobile-features/_static/image46.png)

Tippen Sie auf die **Scott Hanselman** Link.

[![p3_scottHa](aspnet-mvc-4-mobile-features/_static/image49.png)](aspnet-mvc-4-mobile-features/_static/image48.png)

Wie Sie sehen können, ist die Anzeige schwierig, die auf einem mobilen Browser gelesen. Die Date-Spalte ist schwer zu lesen, und der Spalte Tags enthalten ist, nicht in der Ansicht. Um dieses Problem zu beheben, kopieren Sie *Views\Home\SessionsTable.cshtml* zu *Views\Home\SessionsTable.Mobile.cshtml*, und Ersetzen Sie den Inhalt der Datei mit dem folgenden Code:

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample24.cshtml)]

Der Code entfernt Raum und tags von Spalten und den Titel, Referent und Datum vertikal so formatiert, dass alle diese Informationen in einem mobilen Browser gelesen werden. Die folgende Abbildung zeigt die codeänderungen.

[![ps_SessionsByScottHa](aspnet-mvc-4-mobile-features/_static/image51.png)](aspnet-mvc-4-mobile-features/_static/image50.png)

## <a name="improving-the-sessionbycode-view"></a>Verbessern der SessionByCode-Ansicht

Abschließend erstellen Sie eine mobiltaugliche Ansicht der der *SessionByCode* anzeigen. Tippen Sie in den mobilen Browser, auf die **Redner** Schaltfläche, und geben Sie dann `Sc` in das Suchfeld.

[![ps_data_filter_SC](aspnet-mvc-4-mobile-features/_static/image53.png)](aspnet-mvc-4-mobile-features/_static/image52.png)

Tippen Sie auf die **Scott Hanselman** Link. Scott Hanselmans Sitzungen werden angezeigt.

[![ps_SessionsByScottHa](aspnet-mvc-4-mobile-features/_static/image55.png)](aspnet-mvc-4-mobile-features/_static/image54.png)

Wählen Sie die **eine Übersicht über die MS Web Stack des Liebe** Link.

[![ps_love](aspnet-mvc-4-mobile-features/_static/image57.png)](aspnet-mvc-4-mobile-features/_static/image56.png)

Die desktop-Standardansicht ist in Ordnung, aber Sie können es verbessern.

Kopieren der *Views\Home\SessionByCode.cshtml* zu *Views\Home\SessionByCode.Mobile.cshtml* , und Ersetzen Sie den Inhalt der *Views\Home\SessionByCode.Mobile.cshtml*Datei mit folgendem Markup:

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample25.cshtml)]

Das neue Markup verwendet den `data-role` Attribut, um das Layout der Ansicht zu verbessern.

Aktualisieren Sie den mobilen Browser an. Das folgende Bild zeigt die codeänderungen, die Sie gerade vorgenommen haben:

[![p3_love2](aspnet-mvc-4-mobile-features/_static/image59.png)](aspnet-mvc-4-mobile-features/_static/image58.png)

## <a name="wrapup-and-review"></a>Nachbereitung und -Überprüfung

In diesem Tutorial wurde die neuen SMS-Funktionen von ASP.NET MVC 4 Developer Preview eingeführt. Die mobile Features umfassen:

- Die Fähigkeit, Layouts, Ansichten und Teilansichten, sowohl global als auch für eine einzelne Ansicht außer Kraft setzen.
- Kontrolle über das Layout und die partielle außer Kraft setzen-Erzwingung verwenden die `RequireConsistentDisplayMode` Eigenschaft.
- Eine ansichtumschaltung Widget für Mobile Ansichten als auch in desktop-Ansichten angezeigt werden können.
- Unterstützung für bestimmte Browser wie den iPhone-Browser unterstützen.

## <a name="see-also"></a>Siehe auch

- [jQuery Mobile](http://jquerymobile.com) Standort.
- [jQuery Mobile-Übersicht](http://jquerymobile.com/demos/1.0b3/docs/about/intro.html)
- [W3C Recommendation Mobile Web Application Best Practices](http://www.w3.org/TR/mwabp/)
- [W3C-Empfehlungskandidaten für Medienabfragen](http://www.w3.org/TR/css3-mediaqueries/)
