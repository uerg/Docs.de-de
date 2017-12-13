---
uid: web-pages/overview/ui-layouts-and-themes/10-working-with-video
title: Anzeigen von Video in einer ASP.NET Web-Seiten (Razor) Standort | Microsoft Docs
author: tfitzmac
description: "In diesem Kapitel wird erläutert, wie Video in einer ASP.NET Web Pages mit Razor-Syntax-Seite angezeigt wird."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2014
ms.topic: article
ms.assetid: 332fb3da-e2a5-460d-bb90-dd911e1e2c95
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/10-working-with-video
msc.type: authoredcontent
ms.openlocfilehash: 0e1849fb780908b55520d8108e2227d046759987
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="displaying-video-in-an-aspnet-web-pages-razor-site"></a><span data-ttu-id="a3a09-103">Anzeigen von Video an einem Standort der ASP.NET Web Pages (Razor)</span><span class="sxs-lookup"><span data-stu-id="a3a09-103">Displaying Video in an ASP.NET Web Pages (Razor) Site</span></span>
====================
<span data-ttu-id="a3a09-104">durch [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="a3a09-104">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="a3a09-105">In diesem Artikel wird erläutert, wie verwenden Sie einen Player Video (Medium) auf einer Website für ASP.NET Web Pages (Razor), damit Sie Benutzern das Anzeigen von Videos, die auf der Website gespeichert sind.</span><span class="sxs-lookup"><span data-stu-id="a3a09-105">This article explains how to use a video (media) player in an ASP.NET Web Pages (Razor) website to let users view videos that are stored on the site.</span></span> <span data-ttu-id="a3a09-106">ASP.NET Web Pages mit Razor-Syntax können Sie die Wiedergabe von Flash (*SWF*), Media Player (*.wmv*), und Silverlight (*XAP*) Videos.</span><span class="sxs-lookup"><span data-stu-id="a3a09-106">ASP.NET Web Pages with Razor syntax lets you play Flash (*.swf*), Media Player (*.wmv*), and Silverlight (*.xap*) videos.</span></span>
> 
> <span data-ttu-id="a3a09-107">Lernen Sie:</span><span class="sxs-lookup"><span data-stu-id="a3a09-107">What you'll learn:</span></span>
> 
> - <span data-ttu-id="a3a09-108">Wie Sie einen video Player auswählen.</span><span class="sxs-lookup"><span data-stu-id="a3a09-108">How to choose a video player.</span></span>
> - <span data-ttu-id="a3a09-109">Das Hinzufügen von Videos zu einer Webseite.</span><span class="sxs-lookup"><span data-stu-id="a3a09-109">How to add video to a web page.</span></span>
> - <span data-ttu-id="a3a09-110">Vorgehensweise Videoplayer Attribute festgelegt werden.</span><span class="sxs-lookup"><span data-stu-id="a3a09-110">How to set video player attributes.</span></span>
> 
> <span data-ttu-id="a3a09-111">Hierbei handelt es sich um den ASP.NET Razor-Seiten in diesem Artikel vorgestellten Funktionen dar:</span><span class="sxs-lookup"><span data-stu-id="a3a09-111">These are the ASP.NET Razor pages features introduced in the article:</span></span>
> 
> - <span data-ttu-id="a3a09-112">Die `Video` Helper.</span><span class="sxs-lookup"><span data-stu-id="a3a09-112">The `Video` helper.</span></span>
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="a3a09-113">In diesem Lernprogramm verwendeten Versionen der Software</span><span class="sxs-lookup"><span data-stu-id="a3a09-113">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="a3a09-114">ASP.NET Web Pages (Razor) 2</span><span class="sxs-lookup"><span data-stu-id="a3a09-114">ASP.NET Web Pages (Razor) 2</span></span>
> - <span data-ttu-id="a3a09-115">WebMatrix 2</span><span class="sxs-lookup"><span data-stu-id="a3a09-115">WebMatrix 2</span></span>
>   
> 
> <span data-ttu-id="a3a09-116">Dieses Lernprogramm funktioniert auch mit WebMatrix-3.</span><span class="sxs-lookup"><span data-stu-id="a3a09-116">This tutorial also works with WebMatrix 3.</span></span>


## <a name="introduction"></a><span data-ttu-id="a3a09-117">Einführung</span><span class="sxs-lookup"><span data-stu-id="a3a09-117">Introduction</span></span>

<span data-ttu-id="a3a09-118">Möglicherweise möchten die Darstellung eines Videos auf Ihrer Website.</span><span class="sxs-lookup"><span data-stu-id="a3a09-118">You might want to display a video on your site.</span></span> <span data-ttu-id="a3a09-119">Eine Möglichkeit dazu besteht darin zu einer Website zu verknüpfen, die bereits im Video, z. B. YouTube.</span><span class="sxs-lookup"><span data-stu-id="a3a09-119">One way to do that is to link to a site that already has the video, like YouTube.</span></span> <span data-ttu-id="a3a09-120">Wenn Sie ein Video von diesen Standorten direkt in Ihren eigenen Seiten einbetten möchten, können Sie HTML-Markup in der Regel aus der Website abrufen und kopieren sie dann in die Seite.</span><span class="sxs-lookup"><span data-stu-id="a3a09-120">If you want to embed a video from these sites directly in your own pages, you can usually get HTML markup from the site and then copy it into your page.</span></span> <span data-ttu-id="a3a09-121">Z. B. das folgende Beispiel zeigt einen YouTube Einbettung video:</span><span class="sxs-lookup"><span data-stu-id="a3a09-121">For example, the following example shows how to embed a YouTube video:</span></span>

[!code-html[Main](10-working-with-video/samples/sample1.html?highlight=10-14)]

<span data-ttu-id="a3a09-122">Wenn Sie möchten ein Video abspielen, die auf eine eigene Website (nicht auf eine öffentliche Freigabe mit dem Video Website) ist, können nicht Sie direkt mit eingebetteten Markup wie folgt verknüpfen.</span><span class="sxs-lookup"><span data-stu-id="a3a09-122">If you want to play a video that's on your own website (not on a public video-sharing site), you can't directly link to it using embedded markup like this.</span></span> <span data-ttu-id="a3a09-123">Sie Videos über Ihren Standort wiedergeben, mit der `Video` Hilfsprogramm, das einen Media-Player direkt auf einer Seite gerendert wird.</span><span class="sxs-lookup"><span data-stu-id="a3a09-123">However, you can play videos from your site by using the `Video` helper, which renders a media player directly in a page.</span></span>

<a id="Choosing_a_Video_Player"></a>
## <a name="choosing-a-video-player"></a><span data-ttu-id="a3a09-124">Einen Video Player auswählen</span><span class="sxs-lookup"><span data-stu-id="a3a09-124">Choosing a Video Player</span></span>

<span data-ttu-id="a3a09-125">Es gibt viele Formate für Videodateien, und jedes Format in der Regel erfordert einen anderen Player und eine andere Möglichkeit, die der Spieler zu konfigurieren.</span><span class="sxs-lookup"><span data-stu-id="a3a09-125">There are lots of formats for video files, and each format typically requires a different player and a different way to configure the player.</span></span> <span data-ttu-id="a3a09-126">In ASP.NET Razor-Seiten, können Sie ein Video in einer Webseite mit spielen die `Video` Helper.</span><span class="sxs-lookup"><span data-stu-id="a3a09-126">In ASP.NET Razor pages, you can play a video in a web page using the `Video` helper.</span></span> <span data-ttu-id="a3a09-127">Die `Video` Hilfsprogramm vereinfacht das Videos auf einer Webseite einbetten, da es automatisch generiert die `object` und `embed` HTML-Elemente, mit denen normalerweise Video auf der Seite hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="a3a09-127">The `Video` helper simplifies the process of embedding videos in a web page because it automatically generates the `object` and `embed` HTML elements that are normally used to add video to the page.</span></span>

<span data-ttu-id="a3a09-128">Die `Video` Hilfsprogramm unterstützt die folgenden MediaPlayer:</span><span class="sxs-lookup"><span data-stu-id="a3a09-128">The `Video` helper supports the following media players:</span></span>

- <span data-ttu-id="a3a09-129">Adobe Flash</span><span class="sxs-lookup"><span data-stu-id="a3a09-129">Adobe Flash</span></span>
- <span data-ttu-id="a3a09-130">Windows Media Player</span><span class="sxs-lookup"><span data-stu-id="a3a09-130">Windows MediaPlayer</span></span>
- <span data-ttu-id="a3a09-131">Microsoft Silverlight</span><span class="sxs-lookup"><span data-stu-id="a3a09-131">Microsoft Silverlight</span></span>

### <a name="the-flash-player"></a><span data-ttu-id="a3a09-132">Flash Player</span><span class="sxs-lookup"><span data-stu-id="a3a09-132">The Flash Player</span></span>

<span data-ttu-id="a3a09-133">Die `Flash` Player, der die `Video` Hilfsprogramm können Sie die Wiedergabe von Videos Flash (*SWF* Dateien) auf einer Webseite.</span><span class="sxs-lookup"><span data-stu-id="a3a09-133">The `Flash` player of the `Video` helper let you play Flash videos (*.swf* files) in a web page.</span></span> <span data-ttu-id="a3a09-134">Zumindest müssen Sie einen Pfad zu der Videodatei angeben.</span><span class="sxs-lookup"><span data-stu-id="a3a09-134">At a minimum, you have to provide a path to the video file.</span></span> <span data-ttu-id="a3a09-135">Wenn Sie ausschließlich den Pfad angeben, verwendet der Player Standardwerte aufgeführt, die von der aktuellen Version von Flash festgelegt sind.</span><span class="sxs-lookup"><span data-stu-id="a3a09-135">If you specify nothing but the path, the player uses default values that are set by the current version of Flash.</span></span> <span data-ttu-id="a3a09-136">Übliche Standardnamespace Einstellungen sind:</span><span class="sxs-lookup"><span data-stu-id="a3a09-136">Typical default settings are:</span></span>

- <span data-ttu-id="a3a09-137">Das Video angezeigt wird, verwenden die Standardbreite und-Höhe und ohne eine Hintergrundfarbe.</span><span class="sxs-lookup"><span data-stu-id="a3a09-137">The video is displayed using its default width and height and without a background color.</span></span>
- <span data-ttu-id="a3a09-138">Das Video werden beim Laden der Seite automatisch wiedergegeben.</span><span class="sxs-lookup"><span data-stu-id="a3a09-138">The video plays automatically when the page loads.</span></span>
- <span data-ttu-id="a3a09-139">Das Video Schleifen fortlaufend, bis er explizit beendet wird.</span><span class="sxs-lookup"><span data-stu-id="a3a09-139">The video loops continuously until it's explicitly stopped.</span></span>
- <span data-ttu-id="a3a09-140">Das Video wird skaliert, um alle das Video, anstatt das Video entsprechend eine bestimmte Größe zuschneiden anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="a3a09-140">The video is scaled to show all of the video, rather than cropping the video to fit a specific size.</span></span>
- <span data-ttu-id="a3a09-141">Das Video in einem Fenster wiedergegeben wird.</span><span class="sxs-lookup"><span data-stu-id="a3a09-141">The video plays in a window.</span></span>

### <a name="the-mediaplayer-player"></a><span data-ttu-id="a3a09-142">Der Media Player-Player</span><span class="sxs-lookup"><span data-stu-id="a3a09-142">The MediaPlayer Player</span></span>

<span data-ttu-id="a3a09-143">Die `MediaPlayer` Player, der die `Video` -Hilfsobjekt ermöglicht Ihnen die Wiedergabe von Windows Media-Videos (*.wmv* Dateien), Windows Media Audio (*.wma* Dateien), und MP3 (*MP3* Dateien) auf einer Webseite.</span><span class="sxs-lookup"><span data-stu-id="a3a09-143">The `MediaPlayer` player of the `Video` helper lets you play Windows Media videos (*.wmv* files), Windows Media audio (*.wma* files), and MP3 (*.mp3* files) in a web page.</span></span> <span data-ttu-id="a3a09-144">Sie müssen die Mediendatei zum Wiedergeben von Pfadangaben; Alle anderen Parameter sind optional.</span><span class="sxs-lookup"><span data-stu-id="a3a09-144">You must include path of the media file to play; all other parameters are optional.</span></span> <span data-ttu-id="a3a09-145">Wenn Sie nur einen Pfad angeben, verwendet der Spieler Standardeinstellungen, die von der aktuellen Version von Media Player, wie z. B. festgelegt:</span><span class="sxs-lookup"><span data-stu-id="a3a09-145">If you specify only a path, the player uses default settings set by the current version of MediaPlayer, such as:</span></span>

- <span data-ttu-id="a3a09-146">Das Video wird mit dessen Standardwert Breite und Höhe angezeigt.</span><span class="sxs-lookup"><span data-stu-id="a3a09-146">The video is displayed using its default width and height.</span></span>
- <span data-ttu-id="a3a09-147">Das Video werden beim Laden der Seite automatisch wiedergegeben.</span><span class="sxs-lookup"><span data-stu-id="a3a09-147">The video plays automatically when the page loads.</span></span>
- <span data-ttu-id="a3a09-148">Das Video einmal wiedergegeben werden (es keine Schleife).</span><span class="sxs-lookup"><span data-stu-id="a3a09-148">The video plays once (it doesn't loop).</span></span>
- <span data-ttu-id="a3a09-149">Der Spieler zeigt den vollständigen Satz von Steuerelementen in der Benutzeroberfläche.</span><span class="sxs-lookup"><span data-stu-id="a3a09-149">The player displays the full set of controls in the user interface.</span></span>
- <span data-ttu-id="a3a09-150">Das Video spielt bei der in einem Fenster.</span><span class="sxs-lookup"><span data-stu-id="a3a09-150">The video plays in in a window.</span></span>

### <a name="the-silverlight-player"></a><span data-ttu-id="a3a09-151">Die Silverlight-Players</span><span class="sxs-lookup"><span data-stu-id="a3a09-151">The Silverlight Player</span></span>

<span data-ttu-id="a3a09-152">Die `Silverlight` Player, der die `Video` -Hilfsobjekt ermöglicht Ihnen die Wiedergabe von Windows Media Video (*.wmv* Dateien), Windows Media Audio (*.wma* Dateien), und MP3 (*MP3* -Dateien).</span><span class="sxs-lookup"><span data-stu-id="a3a09-152">The `Silverlight` player of the `Video` helper lets you play Windows Media Video (*.wmv* files), Windows Media Audio (*.wma* files), and MP3 (*.mp3* files).</span></span> <span data-ttu-id="a3a09-153">Sie müssen die Path-Parameter, zeigen Sie auf ein Silverlight-basierten Anwendungspaket festlegen (*XAP* Datei).</span><span class="sxs-lookup"><span data-stu-id="a3a09-153">You must set the path parameter to point to a Silverlight-based application package (*.xap* file).</span></span> <span data-ttu-id="a3a09-154">Sie müssen auch die Parameter für Breite und Höhe festlegen.</span><span class="sxs-lookup"><span data-stu-id="a3a09-154">You also must set the width and height parameters.</span></span> <span data-ttu-id="a3a09-155">Alle anderen Parameter sind optional.</span><span class="sxs-lookup"><span data-stu-id="a3a09-155">All other parameters are optional.</span></span> <span data-ttu-id="a3a09-156">Bei Verwendung von Silverlight-Players Video, wenn Sie festlegen, dass nur die erforderlichen Parameter zeigt der Silverlight-Player das Video ohne eine Hintergrundfarbe.</span><span class="sxs-lookup"><span data-stu-id="a3a09-156">When you use the Silverlight player for video, if you set only the required parameters, the Silverlight player displays the video without a background color.</span></span>

> [!NOTE]
> <span data-ttu-id="a3a09-157">Für den Fall, dass Sie Silverlight noch nicht kennen: die *XAP* Datei ist eine komprimierte Datei mit layoutanweisungen in einer *XAML* Datei von verwaltetem Code in Assemblys und optionale Ressourcen.</span><span class="sxs-lookup"><span data-stu-id="a3a09-157">In case you don't already know Silverlight: the *.xap* file is a compressed file that contains layout instructions in a *.xaml* file, managed code in assemblies, and optional resources.</span></span> <span data-ttu-id="a3a09-158">Sie erstellen eine *XAP* Datei in Visual Studio als ein Silverlight-Anwendungsprojekt.</span><span class="sxs-lookup"><span data-stu-id="a3a09-158">You can create a *.xap* file in Visual Studio as a Silverlight application project.</span></span>


<span data-ttu-id="a3a09-159">Die `Silverlight` Videoplayer verwendet sowohl die Einstellungen, die Sie für den Player bereitstellen und die Einstellungen, die finden Sie in der *XAP* Datei.</span><span class="sxs-lookup"><span data-stu-id="a3a09-159">The `Silverlight` video player uses both the settings that you provide for the player and the settings that are provided in the *.xap* file.</span></span>

> [!TIP] 
> 
> <a id="SB_MimeTypes"></a>
> ### <a name="mime-types"></a><span data-ttu-id="a3a09-160">MIME-Typen</span><span class="sxs-lookup"><span data-stu-id="a3a09-160">MIME Types</span></span>
> 
> <span data-ttu-id="a3a09-161">Wenn eine Datei von ein Browser heruntergeladen wird, sendet der Browser sicher, dass der Dateityp den MIME-Typ übereinstimmt, der für das Dokument angegeben wird, die gerendert wird.</span><span class="sxs-lookup"><span data-stu-id="a3a09-161">When a browser downloads a file, the browser makes sure that the file type matches the MIME type that's specified for the document that's being rendered.</span></span> <span data-ttu-id="a3a09-162">Der MIME-Typ ist der Typ oder ein Medium Inhaltstyp einer Datei.</span><span class="sxs-lookup"><span data-stu-id="a3a09-162">The MIME type is the content type or media type of a file.</span></span> <span data-ttu-id="a3a09-163">Die `Video` Hilfsprogramm verwendet die folgenden MIME-Typen:</span><span class="sxs-lookup"><span data-stu-id="a3a09-163">The `Video` helper uses the following MIME types:</span></span>
> 
> - `application/x-shockwave-flash`
> - `application/x-mplayer2`
> - `application/x-silverlight-2`


<a id="Playing_Flash"></a>
## <a name="playing-flash-swf-videos"></a><span data-ttu-id="a3a09-164">Wiedergabe von Videos Flash (SWF)</span><span class="sxs-lookup"><span data-stu-id="a3a09-164">Playing Flash (.swf) Videos</span></span>

<span data-ttu-id="a3a09-165">Dieses Verfahren zeigt, wie ein Flash Video mit dem Namen abgespielt *sample.swf*.</span><span class="sxs-lookup"><span data-stu-id="a3a09-165">This procedure shows you how to play a Flash video named *sample.swf*.</span></span> <span data-ttu-id="a3a09-166">Vorausgesetzt, Sie haben einen Ordner namens *Medien* auf Ihrer Website, und dass die *SWF* Datei ist in diesem Ordner.</span><span class="sxs-lookup"><span data-stu-id="a3a09-166">The procedure assumes that you've got a folder named *Media* on your site and that the *.swf* file is in that folder.</span></span>

1. <span data-ttu-id="a3a09-167">Der ASP.NET Web Helpers Library zu Ihrer Website hinzufügen, wie in beschrieben [installieren-Hilfsprogramme in einer ASP.NET Web Pages-Website](https://go.microsoft.com/fwlink/?LinkId=252372), wenn Sie bereits hinzugefügt haben.</span><span class="sxs-lookup"><span data-stu-id="a3a09-167">Add the ASP.NET Web Helpers Library to your website as described in [Installing Helpers in an ASP.NET Web Pages Site](https://go.microsoft.com/fwlink/?LinkId=252372), if you haven't already added it.</span></span>
2. <span data-ttu-id="a3a09-168">Klicken Sie auf der Website, fügen Sie eine Seite, und nennen Sie sie *FlashVideo.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="a3a09-168">In the website, add a page and name it *FlashVideo.cshtml*.</span></span>
3. <span data-ttu-id="a3a09-169">Fügen Sie auf der Seite das folgende Markup hinzu:</span><span class="sxs-lookup"><span data-stu-id="a3a09-169">Add the following markup to the page:</span></span> 

    [!code-cshtml[Main](10-working-with-video/samples/sample2.cshtml)]
4. <span data-ttu-id="a3a09-170">Führen Sie die Seite in einem Browser aus.</span><span class="sxs-lookup"><span data-stu-id="a3a09-170">Run the page in a browser.</span></span> <span data-ttu-id="a3a09-171">(Stellen Sie sicher, dass die Seite ist ausgewählt, der **Dateien** Arbeitsbereich vor der Ausführung.) Die Seite wird angezeigt, und das Video automatisch wiedergegeben.</span><span class="sxs-lookup"><span data-stu-id="a3a09-171">(Make sure the page is selected in the **Files** workspace before you run it.) The page is displayed and the video plays automatically.</span></span> 

    <span data-ttu-id="a3a09-172">![[Image] ] (10-working-with-video/_static/image1.jpg "ch08_video 1.jpg")</span><span class="sxs-lookup"><span data-stu-id="a3a09-172">![[image]](10-working-with-video/_static/image1.jpg "ch08_video-1.jpg")</span></span>

<span data-ttu-id="a3a09-173">Sie können festlegen, die `quality` -Parameter für ein Flash Video `low`, `autolow`, `autohigh`, `medium`, `high`, und `best`:</span><span class="sxs-lookup"><span data-stu-id="a3a09-173">You can set the `quality` parameter for a Flash video to `low`, `autolow`, `autohigh`, `medium`, `high`, and `best`:</span></span>

[!code-cshtml[Main](10-working-with-video/samples/sample3.cshtml)]

<span data-ttu-id="a3a09-174">Sie können das Flash Video an eine bestimmte Größe mit wiedergegeben Ändern der `scale` -Parameter, der Sie die folgenden festlegen können:</span><span class="sxs-lookup"><span data-stu-id="a3a09-174">You can change the Flash video to play at a specific size using the `scale` parameter, which you can set to the following:</span></span>

- <span data-ttu-id="a3a09-175">`showall`.</span><span class="sxs-lookup"><span data-stu-id="a3a09-175">`showall`.</span></span> <span data-ttu-id="a3a09-176">Dies macht das gesamte Video beim Beibehalten des ursprünglichen Seitenverhältnisses sichtbar.</span><span class="sxs-lookup"><span data-stu-id="a3a09-176">This makes the entire video visible while maintaining the original aspect ratio.</span></span> <span data-ttu-id="a3a09-177">Allerdings können Sie Rahmen auf jeder Seite am Ende.</span><span class="sxs-lookup"><span data-stu-id="a3a09-177">However, you might end up with borders on each side.</span></span>
- <span data-ttu-id="a3a09-178">`noorder`.</span><span class="sxs-lookup"><span data-stu-id="a3a09-178">`noorder`.</span></span> <span data-ttu-id="a3a09-179">Skaliert das Video während das Originalseitenverhältnis beibehalten, aber zugeschnitten werden kann.</span><span class="sxs-lookup"><span data-stu-id="a3a09-179">This scales the video while maintaining the original aspect ratio, but it might be cropped.</span></span>
- <span data-ttu-id="a3a09-180">`exactfit`.</span><span class="sxs-lookup"><span data-stu-id="a3a09-180">`exactfit`.</span></span> <span data-ttu-id="a3a09-181">Dies macht das gesamte Video sichtbar ohne das ursprüngliche Seitenverhältnis beibehalten, aber Verzerrung auftreten.</span><span class="sxs-lookup"><span data-stu-id="a3a09-181">This makes the entire video visible without preserving the original aspect ratio, but distortion may occur.</span></span>

<span data-ttu-id="a3a09-182">Wenn Sie nicht angeben einer `scale` Parameter, das gesamte Video werden angezeigt, und das ursprüngliche Seitenverhältnis beibehalten ohne sämtlichen Zuschneidevorgängen aussieht.</span><span class="sxs-lookup"><span data-stu-id="a3a09-182">If you don't specify a `scale` parameter, the entire video will be visible and the original aspect ratio will be maintained without any cropping.</span></span> <span data-ttu-id="a3a09-183">Das folgende Beispiel zeigt, wie Sie die `scale` Parameter:</span><span class="sxs-lookup"><span data-stu-id="a3a09-183">The following example shows how to use the `scale` parameter:</span></span>

[!code-cshtml[Main](10-working-with-video/samples/sample4.cshtml)]

<span data-ttu-id="a3a09-184">Flash Player unterstützt einen Einstellung mit dem Namen Videomodus `windowMode`.</span><span class="sxs-lookup"><span data-stu-id="a3a09-184">The Flash player supports a video mode setting named `windowMode`.</span></span> <span data-ttu-id="a3a09-185">Sie können festlegen, um `window`, `opaque`, und `transparent`.</span><span class="sxs-lookup"><span data-stu-id="a3a09-185">You can set this to `window`, `opaque`, and `transparent`.</span></span> <span data-ttu-id="a3a09-186">Wird standardmäßig die `windowMode` festgelegt ist, um `window`, dem das Video in einem separaten Fenster auf der Webseite angezeigt.</span><span class="sxs-lookup"><span data-stu-id="a3a09-186">By default, the `windowMode` is set to `window`, which displays the video in a separate window on the web page.</span></span> <span data-ttu-id="a3a09-187">Die `opaque` Einstellung alles, was hinter das Video auf der Webseite werden ausgeblendet.</span><span class="sxs-lookup"><span data-stu-id="a3a09-187">The `opaque` setting hides everything behind the video on the web page.</span></span> <span data-ttu-id="a3a09-188">Die `transparent` Einstellung ermöglicht es den Hintergrund der Webseite anzeigen, die durch das Video vorausgesetzt eines beliebigen Teils des Videos ist transparent.</span><span class="sxs-lookup"><span data-stu-id="a3a09-188">The `transparent` setting lets the background of the web page show through the video, assuming any part of the video is transparent.</span></span>

<a id="Playing_MediaPlayer"></a>
## <a name="playing-mediaplayer-wmv-videos"></a><span data-ttu-id="a3a09-189">Wiedergeben von MediaPlayer (*.wmv*) Videos</span><span class="sxs-lookup"><span data-stu-id="a3a09-189">Playing MediaPlayer (*.wmv*) Videos</span></span>

<span data-ttu-id="a3a09-190">Das folgende Verfahren veranschaulicht das Fenster Medien mit dem Namen Videowiedergabe *"Sample.wmv"* , die sich in der *Medien* Ordner.</span><span class="sxs-lookup"><span data-stu-id="a3a09-190">The following procedure shows you how to play a Window Media video named *sample.wmv* that's in the *Media* folder.</span></span>

1. <span data-ttu-id="a3a09-191">Der ASP.NET Web Helpers Library zu Ihrer Website hinzufügen, wie in beschrieben [installieren-Hilfsprogramme in einer ASP.NET Web Pages-Website](https://go.microsoft.com/fwlink/?LinkId=252372), sofern Sie noch nicht geschehen.</span><span class="sxs-lookup"><span data-stu-id="a3a09-191">Add the ASP.NET Web Helpers Library to your website as described in [Installing Helpers in an ASP.NET Web Pages Site](https://go.microsoft.com/fwlink/?LinkId=252372), if you haven't already.</span></span>
2. <span data-ttu-id="a3a09-192">Erstellen Sie eine neue Seite mit dem Namen *MediaPlayerVideo.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="a3a09-192">Create a new page named *MediaPlayerVideo.cshtml*.</span></span>
3. <span data-ttu-id="a3a09-193">Fügen Sie auf der Seite das folgende Markup hinzu:</span><span class="sxs-lookup"><span data-stu-id="a3a09-193">Add the following markup to the page:</span></span> 

    [!code-cshtml[Main](10-working-with-video/samples/sample5.cshtml)]
4. <span data-ttu-id="a3a09-194">Führen Sie die Seite in einem Browser aus.</span><span class="sxs-lookup"><span data-stu-id="a3a09-194">Run the page in a browser.</span></span> <span data-ttu-id="a3a09-195">Das Video geladen und automatisch wiedergegeben.</span><span class="sxs-lookup"><span data-stu-id="a3a09-195">The video loads and plays automatically.</span></span> 

    <span data-ttu-id="a3a09-196">![[Image] ] (10-working-with-video/_static/image2.jpg "ch08_video 2.jpg")</span><span class="sxs-lookup"><span data-stu-id="a3a09-196">![[image]](10-working-with-video/_static/image2.jpg "ch08_video-2.jpg")</span></span>

<span data-ttu-id="a3a09-197">Sie können festlegen, `playCount` in eine ganze Zahl, der angibt, wie oft automatisch das Video abzuspielen:</span><span class="sxs-lookup"><span data-stu-id="a3a09-197">You can set `playCount` to an integer that indicates how many times to play the video automatically:</span></span>

[!code-cshtml[Main](10-working-with-video/samples/sample6.cshtml)]

<span data-ttu-id="a3a09-198">Die `uiMode` Parameter können Sie angeben, welche Steuerelemente in der Benutzeroberfläche angezeigt.</span><span class="sxs-lookup"><span data-stu-id="a3a09-198">The `uiMode` parameter lets you specify which controls show up in the user interface.</span></span> <span data-ttu-id="a3a09-199">Sie können festlegen, `uiMode` auf `invisible`, `none`, `mini`, oder `full`.</span><span class="sxs-lookup"><span data-stu-id="a3a09-199">You can set `uiMode` to `invisible`, `none`, `mini`, or `full`.</span></span> <span data-ttu-id="a3a09-200">Wenn Sie nicht angeben einer `uiMode` Parameter das Video werden seek mit dem Statusfenster angezeigt, Balken-, Schaltflächen und Volume-Steuerelementen neben der video-Fenster zu steuern.</span><span class="sxs-lookup"><span data-stu-id="a3a09-200">If you don't specify a `uiMode` parameter, the video will be displayed with the status window, seek bar, control buttons, and volume controls in addition to the video window.</span></span> <span data-ttu-id="a3a09-201">Diese Steuerelemente werden auch angezeigt werden, wenn Sie den Player verwenden, um eine Audiodatei.</span><span class="sxs-lookup"><span data-stu-id="a3a09-201">These controls will also be displayed if you use the player to play an audio file.</span></span> <span data-ttu-id="a3a09-202">Hier ist ein Beispiel zum Verwenden der `uiMode` Parameter:</span><span class="sxs-lookup"><span data-stu-id="a3a09-202">Here's an example of how to use the `uiMode` parameter:</span></span>

[!code-cshtml[Main](10-working-with-video/samples/sample7.cshtml)]

<span data-ttu-id="a3a09-203">Standardmäßig ist Audio auf, bei der Wiedergabe des Videos.</span><span class="sxs-lookup"><span data-stu-id="a3a09-203">By default, audio is on when the video plays.</span></span> <span data-ttu-id="a3a09-204">Sie können das Audio Stummschalten, durch Festlegen der `mute` Parameter auf "true":</span><span class="sxs-lookup"><span data-stu-id="a3a09-204">You can mute the audio by setting the `mute` parameter to true:</span></span>

[!code-cshtml[Main](10-working-with-video/samples/sample8.cshtml)]

<span data-ttu-id="a3a09-205">Sie können das audio Maß an das Video MediaPlayer steuern, durch Festlegen der `volume` Parameter auf einen Wert zwischen 0 und 100.</span><span class="sxs-lookup"><span data-stu-id="a3a09-205">You can control the audio level of the MediaPlayer video by setting the `volume` parameter to a value between 0 and 100.</span></span> <span data-ttu-id="a3a09-206">Der Standardwert ist 50.</span><span class="sxs-lookup"><span data-stu-id="a3a09-206">The default value is 50.</span></span> <span data-ttu-id="a3a09-207">Im Folgenden ein Beispiel:</span><span class="sxs-lookup"><span data-stu-id="a3a09-207">Here's an example:</span></span>

[!code-cshtml[Main](10-working-with-video/samples/sample9.cshtml)]

<a id="Playing_Silverlight"></a>
## <a name="playing-silverlight-videos"></a><span data-ttu-id="a3a09-208">Wiedergabe von Silverlight-Videos</span><span class="sxs-lookup"><span data-stu-id="a3a09-208">Playing Silverlight Videos</span></span>

<span data-ttu-id="a3a09-209">Dieses Verfahren wird gezeigt, wie in Silverlight enthaltenen Video abzuspielen *XAP* Seite in einem Ordner mit dem Namen *Medien*.</span><span class="sxs-lookup"><span data-stu-id="a3a09-209">This procedure shows you how to play video contained in a Silverlight *.xap* page that's in a folder named *Media*.</span></span>

1. <span data-ttu-id="a3a09-210">Der ASP.NET Web Helpers Library zu Ihrer Website hinzufügen, wie in beschrieben [installieren-Hilfsprogramme in einer ASP.NET Web Pages-Website](https://go.microsoft.com/fwlink/?LinkId=252372), sofern Sie noch nicht geschehen.</span><span class="sxs-lookup"><span data-stu-id="a3a09-210">Add the ASP.NET Web Helpers Library to your website as described in [Installing Helpers in an ASP.NET Web Pages Site](https://go.microsoft.com/fwlink/?LinkId=252372), if you haven't already .</span></span>
2. <span data-ttu-id="a3a09-211">Erstellen Sie eine neue Seite mit dem Namen *SilverlightVideo.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="a3a09-211">Create a new page named *SilverlightVideo.cshtml*.</span></span>
3. <span data-ttu-id="a3a09-212">Fügen Sie auf der Seite das folgende Markup hinzu:</span><span class="sxs-lookup"><span data-stu-id="a3a09-212">Add the following markup to the page:</span></span> 

    [!code-cshtml[Main](10-working-with-video/samples/sample10.cshtml)]
4. <span data-ttu-id="a3a09-213">Führen Sie die Seite in einem Browser aus.</span><span class="sxs-lookup"><span data-stu-id="a3a09-213">Run the page in a browser.</span></span> 

    <span data-ttu-id="a3a09-214">![[Image] ] (10-working-with-video/_static/image3.jpg "ch08_video 3.jpg")</span><span class="sxs-lookup"><span data-stu-id="a3a09-214">![[image]](10-working-with-video/_static/image3.jpg "ch08_video-3.jpg")</span></span>

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a><span data-ttu-id="a3a09-215">Zusätzliche Ressourcen</span><span class="sxs-lookup"><span data-stu-id="a3a09-215">Additional Resources</span></span>


<span data-ttu-id="a3a09-216">[Silverlight-Übersicht](https://msdn.microsoft.com/en-us/library/bb404700(VS.95).aspx)</span><span class="sxs-lookup"><span data-stu-id="a3a09-216">[Silverlight Overview](https://msdn.microsoft.com/en-us/library/bb404700(VS.95).aspx)</span></span>

[<span data-ttu-id="a3a09-217">Flash-Objekt und das EMBED-Tag-Attributen</span><span class="sxs-lookup"><span data-stu-id="a3a09-217">Flash OBJECT and EMBED tag attributes</span></span>](http://kb2.adobe.com/cps/127/tn_12701.html)

<span data-ttu-id="a3a09-218">[Windows Media Player 11 SDK PARAM-Tags](https://msdn.microsoft.com/en-us/library/aa392321(VS.85).aspx)</span><span class="sxs-lookup"><span data-stu-id="a3a09-218">[Windows Media Player 11 SDK PARAM Tags](https://msdn.microsoft.com/en-us/library/aa392321(VS.85).aspx)</span></span>
