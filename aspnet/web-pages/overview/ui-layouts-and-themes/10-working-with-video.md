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
<a name="displaying-video-in-an-aspnet-web-pages-razor-site"></a><span data-ttu-id="cd95c-103">Anzeigen von Video in einer ASP.NET Web Pages (Razor)-Website</span><span class="sxs-lookup"><span data-stu-id="cd95c-103">Displaying Video in an ASP.NET Web Pages (Razor) Site</span></span>
====================
<span data-ttu-id="cd95c-104">durch [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="cd95c-104">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="cd95c-105">In diesem Artikel wird erläutert, Gewusst wie: Verwenden Sie einen Video (Medien)-Player auf einer Website für ASP.NET Web Pages (Razor), damit Benutzer Videos ansehen, die auf der Website gespeichert sind.</span><span class="sxs-lookup"><span data-stu-id="cd95c-105">This article explains how to use a video (media) player in an ASP.NET Web Pages (Razor) website to let users view videos that are stored on the site.</span></span> <span data-ttu-id="cd95c-106">ASP.NET Web Pages mit Razor-Syntax können Sie die Wiedergabe Flash (*. SWF*), Media Player (*.wmv*), und Silverlight (*XAP*) Videos.</span><span class="sxs-lookup"><span data-stu-id="cd95c-106">ASP.NET Web Pages with Razor syntax lets you play Flash (*.swf*), Media Player (*.wmv*), and Silverlight (*.xap*) videos.</span></span>
> 
> <span data-ttu-id="cd95c-107">Sie lernen Folgendes:</span><span class="sxs-lookup"><span data-stu-id="cd95c-107">What you'll learn:</span></span>
> 
> - <span data-ttu-id="cd95c-108">So wählen Sie einen video Player.</span><span class="sxs-lookup"><span data-stu-id="cd95c-108">How to choose a video player.</span></span>
> - <span data-ttu-id="cd95c-109">Informationen zum Hinzufügen von Videos zu einer Webseite.</span><span class="sxs-lookup"><span data-stu-id="cd95c-109">How to add video to a web page.</span></span>
> - <span data-ttu-id="cd95c-110">Wie die video-Player-Attribute festzulegen.</span><span class="sxs-lookup"><span data-stu-id="cd95c-110">How to set video player attributes.</span></span>
> 
> <span data-ttu-id="cd95c-111">Hierbei handelt es sich um den ASP.NET Razor-Seiten in diesem Artikel vorgestellten Funktionen dar:</span><span class="sxs-lookup"><span data-stu-id="cd95c-111">These are the ASP.NET Razor pages features introduced in the article:</span></span>
> 
> - <span data-ttu-id="cd95c-112">Die `Video` Helper.</span><span class="sxs-lookup"><span data-stu-id="cd95c-112">The `Video` helper.</span></span>
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="cd95c-113">Softwareversionen, die in diesem Tutorial verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="cd95c-113">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="cd95c-114">ASP.NET Web Pages (Razor) 2</span><span class="sxs-lookup"><span data-stu-id="cd95c-114">ASP.NET Web Pages (Razor) 2</span></span>
> - <span data-ttu-id="cd95c-115">WebMatrix 2</span><span class="sxs-lookup"><span data-stu-id="cd95c-115">WebMatrix 2</span></span>
>   
> 
> <span data-ttu-id="cd95c-116">In diesem Tutorial funktioniert auch mit WebMatrix 3.</span><span class="sxs-lookup"><span data-stu-id="cd95c-116">This tutorial also works with WebMatrix 3.</span></span>


## <a name="introduction"></a><span data-ttu-id="cd95c-117">Einführung</span><span class="sxs-lookup"><span data-stu-id="cd95c-117">Introduction</span></span>

<span data-ttu-id="cd95c-118">Sie möchten ein Video auf Ihrer Website anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="cd95c-118">You might want to display a video on your site.</span></span> <span data-ttu-id="cd95c-119">Eine Möglichkeit dafür ist auf einer Website zu verknüpfen, die bereits die Videos, z.B. YouTube.</span><span class="sxs-lookup"><span data-stu-id="cd95c-119">One way to do that is to link to a site that already has the video, like YouTube.</span></span> <span data-ttu-id="cd95c-120">Wenn Sie ein Video von diesen Sites direkt in Ihre eigenen Seiten einbetten möchten, können Sie HTML-Markup in der Regel von der Website zu erhalten, und kopieren Sie diese in Ihre Seite.</span><span class="sxs-lookup"><span data-stu-id="cd95c-120">If you want to embed a video from these sites directly in your own pages, you can usually get HTML markup from the site and then copy it into your page.</span></span> <span data-ttu-id="cd95c-121">Z. B. das folgende Beispiel zeigt das Einbetten von YouTube video:</span><span class="sxs-lookup"><span data-stu-id="cd95c-121">For example, the following example shows how to embed a YouTube video:</span></span>

[!code-html[Main](10-working-with-video/samples/sample1.html?highlight=10-14)]

<span data-ttu-id="cd95c-122">Sollten Sie ein Video abspielen, die auf Ihrer eigenen Website (nicht auf einer öffentlichen sharing-Video-Website) ist, können nicht Sie direkt mit eingebetteten Markup wie dieses verknüpfen.</span><span class="sxs-lookup"><span data-stu-id="cd95c-122">If you want to play a video that's on your own website (not on a public video-sharing site), you can't directly link to it using embedded markup like this.</span></span> <span data-ttu-id="cd95c-123">Sie können jedoch Videos von Ihrem Standort wiedergeben, mit der `Video` Hilfsprogramm, das einen MediaPlayer direkt in einer Seite gerendert wird.</span><span class="sxs-lookup"><span data-stu-id="cd95c-123">However, you can play videos from your site by using the `Video` helper, which renders a media player directly in a page.</span></span>

<a id="Choosing_a_Video_Player"></a>
## <a name="choosing-a-video-player"></a><span data-ttu-id="cd95c-124">Wählen einen Videoplayer</span><span class="sxs-lookup"><span data-stu-id="cd95c-124">Choosing a Video Player</span></span>

<span data-ttu-id="cd95c-125">Es gibt eine Vielzahl von Formaten für Videodateien, und jedes Format in der Regel ist erforderlich, einen anderen Player und ein anderes Verfahren zum Konfigurieren des Spielers.</span><span class="sxs-lookup"><span data-stu-id="cd95c-125">There are lots of formats for video files, and each format typically requires a different player and a different way to configure the player.</span></span> <span data-ttu-id="cd95c-126">In ASP.NET Razor-Seiten, können Sie ein Video in einer Webseite mit Wiedergeben der `Video` Helper.</span><span class="sxs-lookup"><span data-stu-id="cd95c-126">In ASP.NET Razor pages, you can play a video in a web page using the `Video` helper.</span></span> <span data-ttu-id="cd95c-127">Die `Video` Hilfsprogramm vereinfacht das Einbetten von Videos auf einer Webseite, da es automatisch generiert die `object` und `embed` HTML-Elemente, die normalerweise zum Hinzufügen von Videos auf der Seite verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="cd95c-127">The `Video` helper simplifies the process of embedding videos in a web page because it automatically generates the `object` and `embed` HTML elements that are normally used to add video to the page.</span></span>

<span data-ttu-id="cd95c-128">Die `Video` Hilfsprogramm unterstützt die folgenden MediaPlayer:</span><span class="sxs-lookup"><span data-stu-id="cd95c-128">The `Video` helper supports the following media players:</span></span>

- <span data-ttu-id="cd95c-129">Adobe Flash</span><span class="sxs-lookup"><span data-stu-id="cd95c-129">Adobe Flash</span></span>
- <span data-ttu-id="cd95c-130">Windows Media Player</span><span class="sxs-lookup"><span data-stu-id="cd95c-130">Windows MediaPlayer</span></span>
- <span data-ttu-id="cd95c-131">Microsoft Silverlight</span><span class="sxs-lookup"><span data-stu-id="cd95c-131">Microsoft Silverlight</span></span>

### <a name="the-flash-player"></a><span data-ttu-id="cd95c-132">Macromedia Flash Player</span><span class="sxs-lookup"><span data-stu-id="cd95c-132">The Flash Player</span></span>

<span data-ttu-id="cd95c-133">Die `Flash` Player, der die `Video` Hilfsprogramm können Sie die Wiedergabe Flash-Videos (*. SWF* Dateien) auf einer Webseite.</span><span class="sxs-lookup"><span data-stu-id="cd95c-133">The `Flash` player of the `Video` helper let you play Flash videos (*.swf* files) in a web page.</span></span> <span data-ttu-id="cd95c-134">Zumindest müssen Sie einen Pfad zu der Videodatei angeben.</span><span class="sxs-lookup"><span data-stu-id="cd95c-134">At a minimum, you have to provide a path to the video file.</span></span> <span data-ttu-id="cd95c-135">Wenn Sie lediglich den Pfad angeben, verwendet der Player Standardwerte, die von der aktuellen Version von Flash festgelegt werden.</span><span class="sxs-lookup"><span data-stu-id="cd95c-135">If you specify nothing but the path, the player uses default values that are set by the current version of Flash.</span></span> <span data-ttu-id="cd95c-136">Typische Standardeinstellungen sind folgende:</span><span class="sxs-lookup"><span data-stu-id="cd95c-136">Typical default settings are:</span></span>

- <span data-ttu-id="cd95c-137">Das Video angezeigt wird, verwenden die Standardbreite und-Höhe und ohne eine Hintergrundfarbe.</span><span class="sxs-lookup"><span data-stu-id="cd95c-137">The video is displayed using its default width and height and without a background color.</span></span>
- <span data-ttu-id="cd95c-138">Das Video automatisch wiedergegeben, wenn die Seite geladen.</span><span class="sxs-lookup"><span data-stu-id="cd95c-138">The video plays automatically when the page loads.</span></span>
- <span data-ttu-id="cd95c-139">Das Video Schleifen kontinuierlich verwendet werden, bis er explizit beendet wird.</span><span class="sxs-lookup"><span data-stu-id="cd95c-139">The video loops continuously until it's explicitly stopped.</span></span>
- <span data-ttu-id="cd95c-140">Das Video wird skaliert, um alle Videos, anstatt die Zuschneiden von Video entsprechend eine bestimmte Größe anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="cd95c-140">The video is scaled to show all of the video, rather than cropping the video to fit a specific size.</span></span>
- <span data-ttu-id="cd95c-141">Das Video in einem Fenster wiedergegeben wird.</span><span class="sxs-lookup"><span data-stu-id="cd95c-141">The video plays in a window.</span></span>

### <a name="the-mediaplayer-player"></a><span data-ttu-id="cd95c-142">Der MediaPlayer-Player</span><span class="sxs-lookup"><span data-stu-id="cd95c-142">The MediaPlayer Player</span></span>

<span data-ttu-id="cd95c-143">Die `MediaPlayer` Player, der die `Video` -Hilfsobjekt ermöglicht Ihnen das Windows Media Videos wiedergeben (*WMV* Dateien), Windows Media Audio (*.wma* Dateien), und MP3 (*MP3* -Dateien) auf einer Webseite.</span><span class="sxs-lookup"><span data-stu-id="cd95c-143">The `MediaPlayer` player of the `Video` helper lets you play Windows Media videos (*.wmv* files), Windows Media audio (*.wma* files), and MP3 (*.mp3* files) in a web page.</span></span> <span data-ttu-id="cd95c-144">Sie müssen den Pfad der Mediendatei spielen einschließen; Alle anderen Parameter sind optional.</span><span class="sxs-lookup"><span data-stu-id="cd95c-144">You must include path of the media file to play; all other parameters are optional.</span></span> <span data-ttu-id="cd95c-145">Wenn Sie nur einen Pfad angeben, verwendet der Spieler Standardeinstellungen, die von der aktuellen Version von Media Player, festlegen, wie z. B.:</span><span class="sxs-lookup"><span data-stu-id="cd95c-145">If you specify only a path, the player uses default settings set by the current version of MediaPlayer, such as:</span></span>

- <span data-ttu-id="cd95c-146">Das Video angezeigt wird, verwenden die standardmäßigen Breite und Höhe.</span><span class="sxs-lookup"><span data-stu-id="cd95c-146">The video is displayed using its default width and height.</span></span>
- <span data-ttu-id="cd95c-147">Das Video automatisch wiedergegeben, wenn die Seite geladen.</span><span class="sxs-lookup"><span data-stu-id="cd95c-147">The video plays automatically when the page loads.</span></span>
- <span data-ttu-id="cd95c-148">Das Video einmal wiedergegeben (diese Schleife nicht).</span><span class="sxs-lookup"><span data-stu-id="cd95c-148">The video plays once (it doesn't loop).</span></span>
- <span data-ttu-id="cd95c-149">Der Player zeigt den vollständigen Satz von Steuerelementen in der Benutzeroberfläche.</span><span class="sxs-lookup"><span data-stu-id="cd95c-149">The player displays the full set of controls in the user interface.</span></span>
- <span data-ttu-id="cd95c-150">Das Video in einem Fenster wiedergegeben wird.</span><span class="sxs-lookup"><span data-stu-id="cd95c-150">The video plays in a window.</span></span>

### <a name="the-silverlight-player"></a><span data-ttu-id="cd95c-151">Silverlight-Players</span><span class="sxs-lookup"><span data-stu-id="cd95c-151">The Silverlight Player</span></span>

<span data-ttu-id="cd95c-152">Die `Silverlight` Player, der die `Video` -Hilfsobjekt ermöglicht Ihnen das Windows Media Video wiedergeben (*WMV* Dateien), Windows Media Audio (*.wma* Dateien), und MP3 (*MP3* -Dateien).</span><span class="sxs-lookup"><span data-stu-id="cd95c-152">The `Silverlight` player of the `Video` helper lets you play Windows Media Video (*.wmv* files), Windows Media Audio (*.wma* files), and MP3 (*.mp3* files).</span></span> <span data-ttu-id="cd95c-153">Sie müssen festlegen, dass die Path-Parameter, um auf ein Paket der Silverlight-Anwendung zu verweisen (*XAP* Datei).</span><span class="sxs-lookup"><span data-stu-id="cd95c-153">You must set the path parameter to point to a Silverlight-based application package (*.xap* file).</span></span> <span data-ttu-id="cd95c-154">Sie müssen auch die Parameter für Breite und Höhe festlegen.</span><span class="sxs-lookup"><span data-stu-id="cd95c-154">You also must set the width and height parameters.</span></span> <span data-ttu-id="cd95c-155">Alle anderen Parameter sind optional.</span><span class="sxs-lookup"><span data-stu-id="cd95c-155">All other parameters are optional.</span></span> <span data-ttu-id="cd95c-156">Bei Verwendung von Silverlight-Players für Video, wenn Sie festlegen, dass nur die erforderlichen Parameter zeigt der Silverlight-Player das Video, ohne eine Hintergrundfarbe.</span><span class="sxs-lookup"><span data-stu-id="cd95c-156">When you use the Silverlight player for video, if you set only the required parameters, the Silverlight player displays the video without a background color.</span></span>

> [!NOTE]
> <span data-ttu-id="cd95c-157">Für den Fall, dass Sie nicht bereits Silverlight kennen: die *XAP* Datei ist eine komprimierte Datei, die layoutanweisungen in enthält eine *XAML* -Datei, verwalteten Code in Assemblys und optionale Ressourcen.</span><span class="sxs-lookup"><span data-stu-id="cd95c-157">In case you don't already know Silverlight: the *.xap* file is a compressed file that contains layout instructions in a *.xaml* file, managed code in assemblies, and optional resources.</span></span> <span data-ttu-id="cd95c-158">Sie erstellen eine *XAP* -Datei in Visual Studio als ein Silverlight-Anwendungsprojekt.</span><span class="sxs-lookup"><span data-stu-id="cd95c-158">You can create a *.xap* file in Visual Studio as a Silverlight application project.</span></span>


<span data-ttu-id="cd95c-159">Die `Silverlight` Videoplayer, der verwendet wird, sowohl die Einstellungen, die Sie für den Spieler bereitzustellen und die Einstellungen, die finden Sie in der *XAP* Datei.</span><span class="sxs-lookup"><span data-stu-id="cd95c-159">The `Silverlight` video player uses both the settings that you provide for the player and the settings that are provided in the *.xap* file.</span></span>

> [!TIP] 
> 
> <a id="SB_MimeTypes"></a>
> ### <a name="mime-types"></a><span data-ttu-id="cd95c-160">MIME-Typen</span><span class="sxs-lookup"><span data-stu-id="cd95c-160">MIME Types</span></span>
> 
> <span data-ttu-id="cd95c-161">Wenn eine Datei in ein Browser heruntergeladen wird, bewirkt, dass der Browser Sie sicher, dass die mit dem Dateityp den MIME-Typ übereinstimmt, der für das Dokument angegeben wird, die gerendert wird.</span><span class="sxs-lookup"><span data-stu-id="cd95c-161">When a browser downloads a file, the browser makes sure that the file type matches the MIME type that's specified for the document that's being rendered.</span></span> <span data-ttu-id="cd95c-162">Der MIME-Typ ist der Typ oder Media Inhaltstyp einer Datei.</span><span class="sxs-lookup"><span data-stu-id="cd95c-162">The MIME type is the content type or media type of a file.</span></span> <span data-ttu-id="cd95c-163">Die `Video` Hilfsprogramm verwendet die folgenden MIME-Typen:</span><span class="sxs-lookup"><span data-stu-id="cd95c-163">The `Video` helper uses the following MIME types:</span></span>
> 
> - `application/x-shockwave-flash`
> - `application/x-mplayer2`
> - `application/x-silverlight-2`


<a id="Playing_Flash"></a>
## <a name="playing-flash-swf-videos"></a><span data-ttu-id="cd95c-164">Wiedergabe von Videos mit Flash (. SWF)</span><span class="sxs-lookup"><span data-stu-id="cd95c-164">Playing Flash (.swf) Videos</span></span>

<span data-ttu-id="cd95c-165">Dieses Verfahren zeigt, wie ein Flash-Video mit dem Namen abgespielt *sample.swf*.</span><span class="sxs-lookup"><span data-stu-id="cd95c-165">This procedure shows you how to play a Flash video named *sample.swf*.</span></span> <span data-ttu-id="cd95c-166">Das Verfahren setzt voraus, dass Sie einen Ordner namens haben *Media* auf Ihrer Website, und dass die *. SWF* Datei ist in diesem Ordner.</span><span class="sxs-lookup"><span data-stu-id="cd95c-166">The procedure assumes that you've got a folder named *Media* on your site and that the *.swf* file is in that folder.</span></span>

1. <span data-ttu-id="cd95c-167">Die ASP.NET Web Helpers Library zu Ihrer Website hinzufügen, wie in beschrieben [Hilfsprogramme installieren, in einer ASP.NET Web Pages-Website](https://go.microsoft.com/fwlink/?LinkId=252372), falls Sie bereits hinzugefügt haben.</span><span class="sxs-lookup"><span data-stu-id="cd95c-167">Add the ASP.NET Web Helpers Library to your website as described in [Installing Helpers in an ASP.NET Web Pages Site](https://go.microsoft.com/fwlink/?LinkId=252372), if you haven't already added it.</span></span>
2. <span data-ttu-id="cd95c-168">Klicken Sie auf der Website fügen Sie eine Seite, und nennen Sie sie *FlashVideo.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="cd95c-168">In the website, add a page and name it *FlashVideo.cshtml*.</span></span>
3. <span data-ttu-id="cd95c-169">Fügen Sie auf der Seite das folgende Markup hinzu:</span><span class="sxs-lookup"><span data-stu-id="cd95c-169">Add the following markup to the page:</span></span> 

    [!code-cshtml[Main](10-working-with-video/samples/sample2.cshtml)]
4. <span data-ttu-id="cd95c-170">Führen Sie die Seite in einem Browser.</span><span class="sxs-lookup"><span data-stu-id="cd95c-170">Run the page in a browser.</span></span> <span data-ttu-id="cd95c-171">(Stellen Sie sicher, dass die Seite ist ausgewählt, der **Dateien** Arbeitsbereich vor der Ausführung.) Die Seite wird angezeigt, und das Video automatisch wiedergegeben.</span><span class="sxs-lookup"><span data-stu-id="cd95c-171">(Make sure the page is selected in the **Files** workspace before you run it.) The page is displayed and the video plays automatically.</span></span> 

    <span data-ttu-id="cd95c-172">![[Image]](10-working-with-video/_static/image1.jpg "ch08_video-1.jpg")</span><span class="sxs-lookup"><span data-stu-id="cd95c-172">![[image]](10-working-with-video/_static/image1.jpg "ch08_video-1.jpg")</span></span>

<span data-ttu-id="cd95c-173">Sie können festlegen, die `quality` -Parameter für ein Video mit einer Flash `low`, `autolow`, `autohigh`, `medium`, `high`, und `best`:</span><span class="sxs-lookup"><span data-stu-id="cd95c-173">You can set the `quality` parameter for a Flash video to `low`, `autolow`, `autohigh`, `medium`, `high`, and `best`:</span></span>

[!code-cshtml[Main](10-working-with-video/samples/sample3.cshtml)]

<span data-ttu-id="cd95c-174">Sie können ändern, die Flash-Video zur Wiedergabe auf eine bestimmte Größe mithilfe der `scale` -Parameter, der Sie die folgenden festlegen können:</span><span class="sxs-lookup"><span data-stu-id="cd95c-174">You can change the Flash video to play at a specific size using the `scale` parameter, which you can set to the following:</span></span>

- <span data-ttu-id="cd95c-175">`showall`</span><span class="sxs-lookup"><span data-stu-id="cd95c-175">`showall`.</span></span> <span data-ttu-id="cd95c-176">Dadurch werden das gesamte Video unter Beibehaltung der ursprünglichen Seitenverhältnisses sichtbar.</span><span class="sxs-lookup"><span data-stu-id="cd95c-176">This makes the entire video visible while maintaining the original aspect ratio.</span></span> <span data-ttu-id="cd95c-177">Allerdings können Sie Rahmen auf jeder Seite erhalten.</span><span class="sxs-lookup"><span data-stu-id="cd95c-177">However, you might end up with borders on each side.</span></span>
- <span data-ttu-id="cd95c-178">`noorder`</span><span class="sxs-lookup"><span data-stu-id="cd95c-178">`noorder`.</span></span> <span data-ttu-id="cd95c-179">Hiermit skalieren Sie das Video, während das Originalseitenverhältnis beibehalten, aber es zugeschnitten werden kann.</span><span class="sxs-lookup"><span data-stu-id="cd95c-179">This scales the video while maintaining the original aspect ratio, but it might be cropped.</span></span>
- <span data-ttu-id="cd95c-180">`exactfit`</span><span class="sxs-lookup"><span data-stu-id="cd95c-180">`exactfit`.</span></span> <span data-ttu-id="cd95c-181">Dadurch werden das gesamte Video sichtbar ohne gleichzeitig die ursprünglichen Seitenverhältnisse beizubehalten, aber Verzerrung auftreten.</span><span class="sxs-lookup"><span data-stu-id="cd95c-181">This makes the entire video visible without preserving the original aspect ratio, but distortion may occur.</span></span>

<span data-ttu-id="cd95c-182">Wenn Sie nicht angeben einer `scale` Parameter das gesamte Video werden angezeigt und das ursprüngliche Seitenverhältnis wird beibehalten werden, ohne zu sämtlichen Zuschneidevorgängen aussieht.</span><span class="sxs-lookup"><span data-stu-id="cd95c-182">If you don't specify a `scale` parameter, the entire video will be visible and the original aspect ratio will be maintained without any cropping.</span></span> <span data-ttu-id="cd95c-183">Das folgende Beispiel zeigt, wie Sie mit der `scale` Parameter:</span><span class="sxs-lookup"><span data-stu-id="cd95c-183">The following example shows how to use the `scale` parameter:</span></span>

[!code-cshtml[Main](10-working-with-video/samples/sample4.cshtml)]

<span data-ttu-id="cd95c-184">Macromedia Flash Player unterstützt einen Einstellung mit dem Namen Videomodus `windowMode`.</span><span class="sxs-lookup"><span data-stu-id="cd95c-184">The Flash player supports a video mode setting named `windowMode`.</span></span> <span data-ttu-id="cd95c-185">Sie können dies festlegen, um `window`, `opaque`, und `transparent`.</span><span class="sxs-lookup"><span data-stu-id="cd95c-185">You can set this to `window`, `opaque`, and `transparent`.</span></span> <span data-ttu-id="cd95c-186">In der Standardeinstellung die `windowMode` nastaven NA hodnotu `window`, wird angezeigt, die das Video in einem separaten Fenster auf der Webseite.</span><span class="sxs-lookup"><span data-stu-id="cd95c-186">By default, the `windowMode` is set to `window`, which displays the video in a separate window on the web page.</span></span> <span data-ttu-id="cd95c-187">Die `opaque` Einstellung blendet alles, was hinter das Video auf der Webseite.</span><span class="sxs-lookup"><span data-stu-id="cd95c-187">The `opaque` setting hides everything behind the video on the web page.</span></span> <span data-ttu-id="cd95c-188">Die `transparent` Einstellung ermöglicht es den Hintergrund der Webseite anzeigen, die durch das Video, vorausgesetzt, alle Teil des Videos ist transparent.</span><span class="sxs-lookup"><span data-stu-id="cd95c-188">The `transparent` setting lets the background of the web page show through the video, assuming any part of the video is transparent.</span></span>

<a id="Playing_MediaPlayer"></a>
## <a name="playing-mediaplayer-wmv-videos"></a><span data-ttu-id="cd95c-189">Media Player wiedergeben (*.wmv*) Videos</span><span class="sxs-lookup"><span data-stu-id="cd95c-189">Playing MediaPlayer (*.wmv*) Videos</span></span>

<span data-ttu-id="cd95c-190">Das folgende Verfahren zeigt, wie zur Wiedergabe eines Fensters Media-Videos, die mit dem Namen *"Sample.wmv"* , die sich in der *Media* Ordner.</span><span class="sxs-lookup"><span data-stu-id="cd95c-190">The following procedure shows you how to play a Window Media video named *sample.wmv* that's in the *Media* folder.</span></span>

1. <span data-ttu-id="cd95c-191">Die ASP.NET Web Helpers Library zu Ihrer Website hinzufügen, wie in beschrieben [Hilfsprogramme installieren, in einer ASP.NET Web Pages-Website](https://go.microsoft.com/fwlink/?LinkId=252372), sofern Sie noch nicht geschehen.</span><span class="sxs-lookup"><span data-stu-id="cd95c-191">Add the ASP.NET Web Helpers Library to your website as described in [Installing Helpers in an ASP.NET Web Pages Site](https://go.microsoft.com/fwlink/?LinkId=252372), if you haven't already.</span></span>
2. <span data-ttu-id="cd95c-192">Erstellen Sie eine neue Seite mit dem Namen *MediaPlayerVideo.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="cd95c-192">Create a new page named *MediaPlayerVideo.cshtml*.</span></span>
3. <span data-ttu-id="cd95c-193">Fügen Sie auf der Seite das folgende Markup hinzu:</span><span class="sxs-lookup"><span data-stu-id="cd95c-193">Add the following markup to the page:</span></span> 

    [!code-cshtml[Main](10-working-with-video/samples/sample5.cshtml)]
4. <span data-ttu-id="cd95c-194">Führen Sie die Seite in einem Browser.</span><span class="sxs-lookup"><span data-stu-id="cd95c-194">Run the page in a browser.</span></span> <span data-ttu-id="cd95c-195">Das Video wird geladen und automatisch wiedergegeben.</span><span class="sxs-lookup"><span data-stu-id="cd95c-195">The video loads and plays automatically.</span></span> 

    <span data-ttu-id="cd95c-196">![[Image]](10-working-with-video/_static/image2.jpg "ch08_video-2.jpg")</span><span class="sxs-lookup"><span data-stu-id="cd95c-196">![[image]](10-working-with-video/_static/image2.jpg "ch08_video-2.jpg")</span></span>

<span data-ttu-id="cd95c-197">Sie können festlegen, `playCount` in eine ganze Zahl, der angibt, wie viele Male automatisch wiedergeben des Videos:</span><span class="sxs-lookup"><span data-stu-id="cd95c-197">You can set `playCount` to an integer that indicates how many times to play the video automatically:</span></span>

[!code-cshtml[Main](10-working-with-video/samples/sample6.cshtml)]

<span data-ttu-id="cd95c-198">Die `uiMode` Parameter können Sie angeben, welche Steuerelemente in der Benutzeroberfläche angezeigt.</span><span class="sxs-lookup"><span data-stu-id="cd95c-198">The `uiMode` parameter lets you specify which controls show up in the user interface.</span></span> <span data-ttu-id="cd95c-199">Sie können festlegen, `uiMode` zu `invisible`, `none`, `mini`, oder `full`.</span><span class="sxs-lookup"><span data-stu-id="cd95c-199">You can set `uiMode` to `invisible`, `none`, `mini`, or `full`.</span></span> <span data-ttu-id="cd95c-200">Wenn Sie nicht angeben einer `uiMode` Parameter das Video werden mit dem Statusfenster angezeigt, zu suchen, zu steuern, Schaltflächen und Volume-Steuerelemente, zusätzlich zu den video-Fenster.</span><span class="sxs-lookup"><span data-stu-id="cd95c-200">If you don't specify a `uiMode` parameter, the video will be displayed with the status window, seek bar, control buttons, and volume controls in addition to the video window.</span></span> <span data-ttu-id="cd95c-201">Diese Steuerelemente werden auch angezeigt werden, wenn Sie den Player verwenden, um die Wiedergabe einer Audiodatei.</span><span class="sxs-lookup"><span data-stu-id="cd95c-201">These controls will also be displayed if you use the player to play an audio file.</span></span> <span data-ttu-id="cd95c-202">Hier ist ein Beispiel zur Verwendung der `uiMode` Parameter:</span><span class="sxs-lookup"><span data-stu-id="cd95c-202">Here's an example of how to use the `uiMode` parameter:</span></span>

[!code-cshtml[Main](10-working-with-video/samples/sample7.cshtml)]

<span data-ttu-id="cd95c-203">Standardmäßig ist Audio auf, wenn das Video wiedergegeben wird.</span><span class="sxs-lookup"><span data-stu-id="cd95c-203">By default, audio is on when the video plays.</span></span> <span data-ttu-id="cd95c-204">Sie können das Audio Stummschalten, durch Festlegen der `mute` Parameter auf "true":</span><span class="sxs-lookup"><span data-stu-id="cd95c-204">You can mute the audio by setting the `mute` parameter to true:</span></span>

[!code-cshtml[Main](10-working-with-video/samples/sample8.cshtml)]

<span data-ttu-id="cd95c-205">Sie können Steuern der Audiopegel der MediaPlayer-Video durch Festlegen der `volume` auf einen Wert zwischen 0 und 100.</span><span class="sxs-lookup"><span data-stu-id="cd95c-205">You can control the audio level of the MediaPlayer video by setting the `volume` parameter to a value between 0 and 100.</span></span> <span data-ttu-id="cd95c-206">Der Standardwert ist 50.</span><span class="sxs-lookup"><span data-stu-id="cd95c-206">The default value is 50.</span></span> <span data-ttu-id="cd95c-207">Im Folgenden ein Beispiel:</span><span class="sxs-lookup"><span data-stu-id="cd95c-207">Here's an example:</span></span>

[!code-cshtml[Main](10-working-with-video/samples/sample9.cshtml)]

<a id="Playing_Silverlight"></a>
## <a name="playing-silverlight-videos"></a><span data-ttu-id="cd95c-208">Wiedergabe von Silverlight-Videos</span><span class="sxs-lookup"><span data-stu-id="cd95c-208">Playing Silverlight Videos</span></span>

<span data-ttu-id="cd95c-209">Dieses Verfahren zeigt, wie zur Wiedergabe von Video in Silverlight enthaltenen *XAP* Seite in einem Ordner mit dem Namen *Media*.</span><span class="sxs-lookup"><span data-stu-id="cd95c-209">This procedure shows you how to play video contained in a Silverlight *.xap* page that's in a folder named *Media*.</span></span>

1. <span data-ttu-id="cd95c-210">Die ASP.NET Web Helpers Library zu Ihrer Website hinzufügen, wie in beschrieben [Hilfsprogramme installieren, in einer ASP.NET Web Pages-Website](https://go.microsoft.com/fwlink/?LinkId=252372), sofern Sie noch nicht geschehen.</span><span class="sxs-lookup"><span data-stu-id="cd95c-210">Add the ASP.NET Web Helpers Library to your website as described in [Installing Helpers in an ASP.NET Web Pages Site](https://go.microsoft.com/fwlink/?LinkId=252372), if you haven't already .</span></span>
2. <span data-ttu-id="cd95c-211">Erstellen Sie eine neue Seite mit dem Namen *SilverlightVideo.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="cd95c-211">Create a new page named *SilverlightVideo.cshtml*.</span></span>
3. <span data-ttu-id="cd95c-212">Fügen Sie auf der Seite das folgende Markup hinzu:</span><span class="sxs-lookup"><span data-stu-id="cd95c-212">Add the following markup to the page:</span></span> 

    [!code-cshtml[Main](10-working-with-video/samples/sample10.cshtml)]
4. <span data-ttu-id="cd95c-213">Führen Sie die Seite in einem Browser.</span><span class="sxs-lookup"><span data-stu-id="cd95c-213">Run the page in a browser.</span></span> 

    <span data-ttu-id="cd95c-214">![[Image]](10-working-with-video/_static/image3.jpg "ch08_video-3.jpg")</span><span class="sxs-lookup"><span data-stu-id="cd95c-214">![[image]](10-working-with-video/_static/image3.jpg "ch08_video-3.jpg")</span></span>

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a><span data-ttu-id="cd95c-215">Zusätzliche Ressourcen</span><span class="sxs-lookup"><span data-stu-id="cd95c-215">Additional Resources</span></span>


<span data-ttu-id="cd95c-216">[Übersicht über das Silverlight](https://msdn.microsoft.com/library/bb404700(VS.95).aspx)</span><span class="sxs-lookup"><span data-stu-id="cd95c-216">[Silverlight Overview](https://msdn.microsoft.com/library/bb404700(VS.95).aspx)</span></span>

[<span data-ttu-id="cd95c-217">Flash-Objekt und EINBETTEN Tagattribute</span><span class="sxs-lookup"><span data-stu-id="cd95c-217">Flash OBJECT and EMBED tag attributes</span></span>](http://kb2.adobe.com/cps/127/tn_12701.html)

<span data-ttu-id="cd95c-218">[Windows Media Player 11 SDK PARAM-Tags](https://msdn.microsoft.com/library/aa392321(VS.85).aspx)</span><span class="sxs-lookup"><span data-stu-id="cd95c-218">[Windows Media Player 11 SDK PARAM Tags](https://msdn.microsoft.com/library/aa392321(VS.85).aspx)</span></span>
