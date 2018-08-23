---
uid: web-pages/overview/ui-layouts-and-themes/10-working-with-video
title: Anzeigen von Video in einer ASP.NET Web Pages (Razor) Standort | Microsoft-Dokumentation
author: tfitzmac
description: In diesem Kapitel wird erläutert, wie Video in einer ASP.NET Web Pages mit Razor-Syntax-Seite angezeigt wird.
ms.author: riande
ms.date: 02/20/2014
ms.assetid: 332fb3da-e2a5-460d-bb90-dd911e1e2c95
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/10-working-with-video
msc.type: authoredcontent
ms.openlocfilehash: 28884d8c4998ea5b00a60bf094f41b915b565bd8
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2018
ms.locfileid: "41836501"
---
<a name="displaying-video-in-an-aspnet-web-pages-razor-site"></a>Anzeigen von Video in einer ASP.NET Web Pages (Razor)-Website
====================
durch [Tom FitzMacken](https://github.com/tfitzmac)

> In diesem Artikel wird erläutert, Gewusst wie: Verwenden Sie einen Video (Medien)-Player auf einer Website für ASP.NET Web Pages (Razor), damit Benutzer Videos ansehen, die auf der Website gespeichert sind. ASP.NET Web Pages mit Razor-Syntax können Sie die Wiedergabe Flash (*. SWF*), Media Player (*.wmv*), und Silverlight (*XAP*) Videos.
> 
> Sie lernen Folgendes:
> 
> - So wählen Sie einen video Player.
> - Informationen zum Hinzufügen von Videos zu einer Webseite.
> - Wie die video-Player-Attribute festzulegen.
> 
> Hierbei handelt es sich um den ASP.NET Razor-Seiten in diesem Artikel vorgestellten Funktionen dar:
> 
> - Die `Video` Helper.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Softwareversionen, die in diesem Tutorial verwendet werden.
> 
> 
> - ASP.NET Web Pages (Razor) 2
> - WebMatrix 2
>   
> 
> In diesem Tutorial funktioniert auch mit WebMatrix 3.


## <a name="introduction"></a>Einführung

Sie möchten ein Video auf Ihrer Website anzuzeigen. Eine Möglichkeit dafür ist auf einer Website zu verknüpfen, die bereits die Videos, z.B. YouTube. Wenn Sie ein Video von diesen Sites direkt in Ihre eigenen Seiten einbetten möchten, können Sie HTML-Markup in der Regel von der Website zu erhalten, und kopieren Sie diese in Ihre Seite. Z. B. das folgende Beispiel zeigt das Einbetten von YouTube video:

[!code-html[Main](10-working-with-video/samples/sample1.html?highlight=10-14)]

Sollten Sie ein Video abspielen, die auf Ihrer eigenen Website (nicht auf einer öffentlichen sharing-Video-Website) ist, können nicht Sie direkt mit eingebetteten Markup wie dieses verknüpfen. Sie können jedoch Videos von Ihrem Standort wiedergeben, mit der `Video` Hilfsprogramm, das einen MediaPlayer direkt in einer Seite gerendert wird.

<a id="Choosing_a_Video_Player"></a>
## <a name="choosing-a-video-player"></a>Wählen einen Videoplayer

Es gibt eine Vielzahl von Formaten für Videodateien, und jedes Format in der Regel ist erforderlich, einen anderen Player und ein anderes Verfahren zum Konfigurieren des Spielers. In ASP.NET Razor-Seiten, können Sie ein Video in einer Webseite mit Wiedergeben der `Video` Helper. Die `Video` Hilfsprogramm vereinfacht das Einbetten von Videos auf einer Webseite, da es automatisch generiert die `object` und `embed` HTML-Elemente, die normalerweise zum Hinzufügen von Videos auf der Seite verwendet werden.

Die `Video` Hilfsprogramm unterstützt die folgenden MediaPlayer:

- Adobe Flash
- Windows Media Player
- Microsoft Silverlight

### <a name="the-flash-player"></a>Macromedia Flash Player

Die `Flash` Player, der die `Video` Hilfsprogramm können Sie die Wiedergabe Flash-Videos (*. SWF* Dateien) auf einer Webseite. Zumindest müssen Sie einen Pfad zu der Videodatei angeben. Wenn Sie lediglich den Pfad angeben, verwendet der Player Standardwerte, die von der aktuellen Version von Flash festgelegt werden. Typische Standardeinstellungen sind folgende:

- Das Video angezeigt wird, verwenden die Standardbreite und-Höhe und ohne eine Hintergrundfarbe.
- Das Video automatisch wiedergegeben, wenn die Seite geladen.
- Das Video Schleifen kontinuierlich verwendet werden, bis er explizit beendet wird.
- Das Video wird skaliert, um alle Videos, anstatt die Zuschneiden von Video entsprechend eine bestimmte Größe anzuzeigen.
- Das Video in einem Fenster wiedergegeben wird.

### <a name="the-mediaplayer-player"></a>Der MediaPlayer-Player

Die `MediaPlayer` Player, der die `Video` -Hilfsobjekt ermöglicht Ihnen das Windows Media Videos wiedergeben (*WMV* Dateien), Windows Media Audio (*.wma* Dateien), und MP3 (*MP3* -Dateien) auf einer Webseite. Sie müssen den Pfad der Mediendatei spielen einschließen; Alle anderen Parameter sind optional. Wenn Sie nur einen Pfad angeben, verwendet der Spieler Standardeinstellungen, die von der aktuellen Version von Media Player, festlegen, wie z. B.:

- Das Video angezeigt wird, verwenden die standardmäßigen Breite und Höhe.
- Das Video automatisch wiedergegeben, wenn die Seite geladen.
- Das Video einmal wiedergegeben (diese Schleife nicht).
- Der Player zeigt den vollständigen Satz von Steuerelementen in der Benutzeroberfläche.
- Das Video in einem Fenster wiedergegeben wird.

### <a name="the-silverlight-player"></a>Silverlight-Players

Die `Silverlight` Player, der die `Video` -Hilfsobjekt ermöglicht Ihnen das Windows Media Video wiedergeben (*WMV* Dateien), Windows Media Audio (*.wma* Dateien), und MP3 (*MP3* -Dateien). Sie müssen festlegen, dass die Path-Parameter, um auf ein Paket der Silverlight-Anwendung zu verweisen (*XAP* Datei). Sie müssen auch die Parameter für Breite und Höhe festlegen. Alle anderen Parameter sind optional. Bei Verwendung von Silverlight-Players für Video, wenn Sie festlegen, dass nur die erforderlichen Parameter zeigt der Silverlight-Player das Video, ohne eine Hintergrundfarbe.

> [!NOTE]
> Für den Fall, dass Sie nicht bereits Silverlight kennen: die *XAP* Datei ist eine komprimierte Datei, die layoutanweisungen in enthält eine *XAML* -Datei, verwalteten Code in Assemblys und optionale Ressourcen. Sie erstellen eine *XAP* -Datei in Visual Studio als ein Silverlight-Anwendungsprojekt.


Die `Silverlight` Videoplayer, der verwendet wird, sowohl die Einstellungen, die Sie für den Spieler bereitzustellen und die Einstellungen, die finden Sie in der *XAP* Datei.

> [!TIP] 
> 
> <a id="SB_MimeTypes"></a>
> ### <a name="mime-types"></a>MIME-Typen
> 
> Wenn eine Datei in ein Browser heruntergeladen wird, bewirkt, dass der Browser Sie sicher, dass die mit dem Dateityp den MIME-Typ übereinstimmt, der für das Dokument angegeben wird, die gerendert wird. Der MIME-Typ ist der Typ oder Media Inhaltstyp einer Datei. Die `Video` Hilfsprogramm verwendet die folgenden MIME-Typen:
> 
> - `application/x-shockwave-flash`
> - `application/x-mplayer2`
> - `application/x-silverlight-2`


<a id="Playing_Flash"></a>
## <a name="playing-flash-swf-videos"></a>Wiedergabe von Videos mit Flash (. SWF)

Dieses Verfahren zeigt, wie ein Flash-Video mit dem Namen abgespielt *sample.swf*. Das Verfahren setzt voraus, dass Sie einen Ordner namens haben *Media* auf Ihrer Website, und dass die *. SWF* Datei ist in diesem Ordner.

1. Die ASP.NET Web Helpers Library zu Ihrer Website hinzufügen, wie in beschrieben [Hilfsprogramme installieren, in einer ASP.NET Web Pages-Website](https://go.microsoft.com/fwlink/?LinkId=252372), falls Sie bereits hinzugefügt haben.
2. Klicken Sie auf der Website fügen Sie eine Seite, und nennen Sie sie *FlashVideo.cshtml*.
3. Fügen Sie auf der Seite das folgende Markup hinzu: 

    [!code-cshtml[Main](10-working-with-video/samples/sample2.cshtml)]
4. Führen Sie die Seite in einem Browser. (Stellen Sie sicher, dass die Seite ist ausgewählt, der **Dateien** Arbeitsbereich vor der Ausführung.) Die Seite wird angezeigt, und das Video automatisch wiedergegeben. 

    ![[Image]](10-working-with-video/_static/image1.jpg "ch08_video-1.jpg")

Sie können festlegen, die `quality` -Parameter für ein Video mit einer Flash `low`, `autolow`, `autohigh`, `medium`, `high`, und `best`:

[!code-cshtml[Main](10-working-with-video/samples/sample3.cshtml)]

Sie können ändern, die Flash-Video zur Wiedergabe auf eine bestimmte Größe mithilfe der `scale` -Parameter, der Sie die folgenden festlegen können:

- `showall` Dadurch werden das gesamte Video unter Beibehaltung der ursprünglichen Seitenverhältnisses sichtbar. Allerdings können Sie Rahmen auf jeder Seite erhalten.
- `noorder` Hiermit skalieren Sie das Video, während das Originalseitenverhältnis beibehalten, aber es zugeschnitten werden kann.
- `exactfit` Dadurch werden das gesamte Video sichtbar ohne gleichzeitig die ursprünglichen Seitenverhältnisse beizubehalten, aber Verzerrung auftreten.

Wenn Sie nicht angeben einer `scale` Parameter das gesamte Video werden angezeigt und das ursprüngliche Seitenverhältnis wird beibehalten werden, ohne zu sämtlichen Zuschneidevorgängen aussieht. Das folgende Beispiel zeigt, wie Sie mit der `scale` Parameter:

[!code-cshtml[Main](10-working-with-video/samples/sample4.cshtml)]

Macromedia Flash Player unterstützt einen Einstellung mit dem Namen Videomodus `windowMode`. Sie können dies festlegen, um `window`, `opaque`, und `transparent`. In der Standardeinstellung die `windowMode` nastaven NA hodnotu `window`, wird angezeigt, die das Video in einem separaten Fenster auf der Webseite. Die `opaque` Einstellung blendet alles, was hinter das Video auf der Webseite. Die `transparent` Einstellung ermöglicht es den Hintergrund der Webseite anzeigen, die durch das Video, vorausgesetzt, alle Teil des Videos ist transparent.

<a id="Playing_MediaPlayer"></a>
## <a name="playing-mediaplayer-wmv-videos"></a>Media Player wiedergeben (*.wmv*) Videos

Das folgende Verfahren zeigt, wie zur Wiedergabe eines Fensters Media-Videos, die mit dem Namen *"Sample.wmv"* , die sich in der *Media* Ordner.

1. Die ASP.NET Web Helpers Library zu Ihrer Website hinzufügen, wie in beschrieben [Hilfsprogramme installieren, in einer ASP.NET Web Pages-Website](https://go.microsoft.com/fwlink/?LinkId=252372), sofern Sie noch nicht geschehen.
2. Erstellen Sie eine neue Seite mit dem Namen *MediaPlayerVideo.cshtml*.
3. Fügen Sie auf der Seite das folgende Markup hinzu: 

    [!code-cshtml[Main](10-working-with-video/samples/sample5.cshtml)]
4. Führen Sie die Seite in einem Browser. Das Video wird geladen und automatisch wiedergegeben. 

    ![[Image]](10-working-with-video/_static/image2.jpg "ch08_video-2.jpg")

Sie können festlegen, `playCount` in eine ganze Zahl, der angibt, wie viele Male automatisch wiedergeben des Videos:

[!code-cshtml[Main](10-working-with-video/samples/sample6.cshtml)]

Die `uiMode` Parameter können Sie angeben, welche Steuerelemente in der Benutzeroberfläche angezeigt. Sie können festlegen, `uiMode` zu `invisible`, `none`, `mini`, oder `full`. Wenn Sie nicht angeben einer `uiMode` Parameter das Video werden mit dem Statusfenster angezeigt, zu suchen, zu steuern, Schaltflächen und Volume-Steuerelemente, zusätzlich zu den video-Fenster. Diese Steuerelemente werden auch angezeigt werden, wenn Sie den Player verwenden, um die Wiedergabe einer Audiodatei. Hier ist ein Beispiel zur Verwendung der `uiMode` Parameter:

[!code-cshtml[Main](10-working-with-video/samples/sample7.cshtml)]

Standardmäßig ist Audio auf, wenn das Video wiedergegeben wird. Sie können das Audio Stummschalten, durch Festlegen der `mute` Parameter auf "true":

[!code-cshtml[Main](10-working-with-video/samples/sample8.cshtml)]

Sie können Steuern der Audiopegel der MediaPlayer-Video durch Festlegen der `volume` auf einen Wert zwischen 0 und 100. Der Standardwert ist 50. Im Folgenden ein Beispiel:

[!code-cshtml[Main](10-working-with-video/samples/sample9.cshtml)]

<a id="Playing_Silverlight"></a>
## <a name="playing-silverlight-videos"></a>Wiedergabe von Silverlight-Videos

Dieses Verfahren zeigt, wie zur Wiedergabe von Video in Silverlight enthaltenen *XAP* Seite in einem Ordner mit dem Namen *Media*.

1. Die ASP.NET Web Helpers Library zu Ihrer Website hinzufügen, wie in beschrieben [Hilfsprogramme installieren, in einer ASP.NET Web Pages-Website](https://go.microsoft.com/fwlink/?LinkId=252372), sofern Sie noch nicht geschehen.
2. Erstellen Sie eine neue Seite mit dem Namen *SilverlightVideo.cshtml*.
3. Fügen Sie auf der Seite das folgende Markup hinzu: 

    [!code-cshtml[Main](10-working-with-video/samples/sample10.cshtml)]
4. Führen Sie die Seite in einem Browser. 

    ![[Image]](10-working-with-video/_static/image3.jpg "ch08_video-3.jpg")

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>Zusätzliche Ressourcen


[Übersicht über das Silverlight](https://msdn.microsoft.com/library/bb404700(VS.95).aspx)

[Flash-Objekt und EINBETTEN Tagattribute](http://kb2.adobe.com/cps/127/tn_12701.html)

[Windows Media Player 11 SDK PARAM-Tags](https://msdn.microsoft.com/library/aa392321(VS.85).aspx)
