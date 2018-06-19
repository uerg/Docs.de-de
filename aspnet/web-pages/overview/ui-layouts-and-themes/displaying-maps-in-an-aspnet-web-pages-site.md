---
uid: web-pages/overview/ui-layouts-and-themes/displaying-maps-in-an-aspnet-web-pages-site
title: Anzeigen von Zuordnungen in einer ASP.NET Web Pages (Razor) Standort | Microsoft Docs
author: tfitzmac
description: In diesem Artikel wird erläutert, wie interaktive Karten auf Seiten in einer ASP.NET Web Pages (Razor) Website anhand der Zuordnung von Bing, Google, Ma bereitgestellten Dienste anzeigen...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2014
ms.topic: article
ms.assetid: b5c268dd-ca6a-4562-b94c-a220fcf01f58
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/displaying-maps-in-an-aspnet-web-pages-site
msc.type: authoredcontent
ms.openlocfilehash: 608dab8760bad7b877ab6fd4f89b21e980f5b1db
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
ms.locfileid: "30893861"
---
<a name="displaying-maps-in-an-aspnet-web-pages-razor-site"></a><span data-ttu-id="4b7e4-103">Anzeigen von Maps an einem Standort der ASP.NET Web Pages (Razor)</span><span class="sxs-lookup"><span data-stu-id="4b7e4-103">Displaying Maps in an ASP.NET Web Pages (Razor) Site</span></span>
====================
<span data-ttu-id="4b7e4-104">durch [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="4b7e4-104">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="4b7e4-105">In diesem Artikel wird erläutert, wie interaktive Karten auf Seiten in einer ASP.NET Web Pages (Razor) Website anhand der Zuordnung von Bing, Google, MapQuest und Yahoo bereitgestellten Dienste angezeigt.</span><span class="sxs-lookup"><span data-stu-id="4b7e4-105">This article explains how to display interactive maps on pages in an ASP.NET Web Pages (Razor) website based on mapping services provided by Bing, Google, MapQuest, and Yahoo.</span></span>
> 
> <span data-ttu-id="4b7e4-106">Lernen Sie:</span><span class="sxs-lookup"><span data-stu-id="4b7e4-106">What you'll learn:</span></span>
> 
> - <span data-ttu-id="4b7e4-107">Wie Sie eine Zuordnung basierend auf einer Adresse zu generieren.</span><span class="sxs-lookup"><span data-stu-id="4b7e4-107">How to generate a map based on an address.</span></span>
> - <span data-ttu-id="4b7e4-108">So generieren Sie eine Zuordnung basierend auf Breiten-und Längenkoordinaten.</span><span class="sxs-lookup"><span data-stu-id="4b7e4-108">How to generate a map based on latitude and longitude coordinates.</span></span>
> - <span data-ttu-id="4b7e4-109">Informationen zum Registrieren einer Bing Maps-Entwicklerkonto und einen Schlüssel für die Verwendung mit Bing Maps abzurufen.</span><span class="sxs-lookup"><span data-stu-id="4b7e4-109">How to register a Bing Maps Developer Account and get a key to use with Bing Maps.</span></span>
> 
> <span data-ttu-id="4b7e4-110">Dies ist die ASP.NET-Funktion im Artikel eingeführt:</span><span class="sxs-lookup"><span data-stu-id="4b7e4-110">This is the ASP.NET feature introduced in the article:</span></span>
> 
> - <span data-ttu-id="4b7e4-111">Die `Maps` Helper.</span><span class="sxs-lookup"><span data-stu-id="4b7e4-111">The `Maps` helper.</span></span>
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="4b7e4-112">In diesem Lernprogramm verwendeten Versionen der Software</span><span class="sxs-lookup"><span data-stu-id="4b7e4-112">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="4b7e4-113">ASP.NET Web Pages (Razor) 2</span><span class="sxs-lookup"><span data-stu-id="4b7e4-113">ASP.NET Web Pages (Razor) 2</span></span>
> - <span data-ttu-id="4b7e4-114">WebMatrix 2</span><span class="sxs-lookup"><span data-stu-id="4b7e4-114">WebMatrix 2</span></span>
>   
> 
> <span data-ttu-id="4b7e4-115">Dieses Lernprogramm funktioniert auch mit WebMatrix-3.</span><span class="sxs-lookup"><span data-stu-id="4b7e4-115">This tutorial also works with WebMatrix 3.</span></span>


<span data-ttu-id="4b7e4-116">Webseiten, können Sie Zuordnungen auf einer Seite anzeigen, indem Sie mithilfe von `Maps` Helper.</span><span class="sxs-lookup"><span data-stu-id="4b7e4-116">In Web Pages, you can display maps on a page by using `Maps` helper.</span></span> <span data-ttu-id="4b7e4-117">Sie können Zuordnungen basierend auf einer Adresse oder einen Satz von Längen- und Breitengrad Koordinaten generieren.</span><span class="sxs-lookup"><span data-stu-id="4b7e4-117">You can generate maps based either on an address or on a set of longitude and latitude coordinates.</span></span> <span data-ttu-id="4b7e4-118">Die `Maps` -Klasse können Sie einen an beliebten Zuordnung Module Aufruf, einschließlich Bing, Google, MapQuest und Yahoo.</span><span class="sxs-lookup"><span data-stu-id="4b7e4-118">The `Maps` class lets you call into popular map engines including Bing, Google, MapQuest, and Yahoo.</span></span>

<span data-ttu-id="4b7e4-119">Die Schritte zum Hinzufügen der Zuordnung zu einer Seite sind identisch, unabhängig davon, die welche die Zuordnung Module, die Sie aufrufen.</span><span class="sxs-lookup"><span data-stu-id="4b7e4-119">The steps for adding mapping to a page are the same regardless of which of the map engines you call.</span></span> <span data-ttu-id="4b7e4-120">Sie fügen nur einen Verweis der JavaScript-Datei, mit der verfügbaren Methoden, um die Zuordnung anzuzeigen, und rufen dann Methoden der der `Maps` Helper.</span><span class="sxs-lookup"><span data-stu-id="4b7e4-120">You just add a JavaScript file reference that makes available methods to display the map, and then you call methods of the `Maps` helper.</span></span>

<span data-ttu-id="4b7e4-121">Sie wählen einen Map-Dienst abhängig davon, welcher `Maps` Hilfsmethode, die Sie verwenden.</span><span class="sxs-lookup"><span data-stu-id="4b7e4-121">You choose a map service based on which `Maps` helper method you use.</span></span> <span data-ttu-id="4b7e4-122">Sie können diese verwenden:</span><span class="sxs-lookup"><span data-stu-id="4b7e4-122">You can use any of these:</span></span>

- `Maps.GetBingHtml`
- `Maps.GetGoogleHtml`
- `Maps.GetYahooHtml`
- `Maps.GetMapQuestHtml`

## <a name="installing-the-pieces-you-need"></a><span data-ttu-id="4b7e4-123">Installieren die einzelnen Teile, die Sie benötigen</span><span class="sxs-lookup"><span data-stu-id="4b7e4-123">Installing the Pieces You Need</span></span>

<span data-ttu-id="4b7e4-124">Wenn Zuordnungen anzeigen möchten, benötigen Sie diese:</span><span class="sxs-lookup"><span data-stu-id="4b7e4-124">To display maps, you need these pieces:</span></span>

- <span data-ttu-id="4b7e4-125">Die `Maps` Helper.</span><span class="sxs-lookup"><span data-stu-id="4b7e4-125">The `Maps` helper.</span></span> <span data-ttu-id="4b7e4-126">Dieses Hilfsprogramm ist in der ASP.NET Web Helpers Library-Version 2.</span><span class="sxs-lookup"><span data-stu-id="4b7e4-126">This helper is in version 2 of the ASP.NET Web Helpers Library.</span></span> <span data-ttu-id="4b7e4-127">Wenn Sie bereits mit die Bibliothek hinzugefügt haben, können Sie es an Ihrem Standort als NuGet-Paket installieren.</span><span class="sxs-lookup"><span data-stu-id="4b7e4-127">If you haven't already added the library, you can install it in your site as a NuGet package.</span></span> <span data-ttu-id="4b7e4-128">Weitere Informationen finden Sie unter [installieren-Hilfsprogramme in einer ASP.NET Web Pages-Website](https://go.microsoft.com/fwlink/?LinkId=252372).</span><span class="sxs-lookup"><span data-stu-id="4b7e4-128">For details, see [Installing Helpers in an ASP.NET Web Pages Site](https://go.microsoft.com/fwlink/?LinkId=252372).</span></span> <span data-ttu-id="4b7e4-129">(Suchen Sie in der Galerie der `microsoft-web-helpers` Paket.)</span><span class="sxs-lookup"><span data-stu-id="4b7e4-129">(In the Gallery, search for the `microsoft-web-helpers` package.)</span></span>
- <span data-ttu-id="4b7e4-130">Die jQuery-Bibliothek.</span><span class="sxs-lookup"><span data-stu-id="4b7e4-130">The jQuery library.</span></span> <span data-ttu-id="4b7e4-131">Einige der WebMatrix-Websitevorlagen enthalten bereits jQuery-Bibliotheken in ihre *Skript* Ordner.</span><span class="sxs-lookup"><span data-stu-id="4b7e4-131">Several of the WebMatrix site templates already include jQuery libraries in their *Script* folders.</span></span> <span data-ttu-id="4b7e4-132">Wenn Sie nicht über diese Bibliotheken verfügen, können Sie herunterladen die neueste jQuery-Bibliothek direkt aus der [jQuery.org](http://jQuery.org) Standort.</span><span class="sxs-lookup"><span data-stu-id="4b7e4-132">If you do not have these libraries, you can download the latest jQuery library directly from the [jQuery.org](http://jQuery.org) site.</span></span> <span data-ttu-id="4b7e4-133">Oder Sie können einen neuen Standort mithilfe einer Vorlage erstellen (z. B. die **Starter Site** Vorlage), und kopieren Sie die jQuery-Dateien von diesem Standort zu Ihrem aktuellen Standort.</span><span class="sxs-lookup"><span data-stu-id="4b7e4-133">Or you can create a new site using a template (for example, the **Starter Site** template) and then copy the jQuery files from that site to your current site.</span></span>

<span data-ttu-id="4b7e4-134">Schließlich, wenn Sie die Bing Maps verwenden möchten, müssen Sie zuerst erstellen ein free-Konto und einen Schlüssel abzurufen.</span><span class="sxs-lookup"><span data-stu-id="4b7e4-134">Finally, if you want to use Bing maps, you must first create a (free) account and get a key.</span></span> <span data-ttu-id="4b7e4-135">Um einen Schlüssel zu erhalten, gehen Sie folgendermaßen vor:</span><span class="sxs-lookup"><span data-stu-id="4b7e4-135">To get a key, follow these steps:</span></span>

1. <span data-ttu-id="4b7e4-136">Erstellen Sie ein Konto auf dem [Bing Maps-Entwicklerkonto](https://www.microsoft.com/maps/developers/web.aspx).</span><span class="sxs-lookup"><span data-stu-id="4b7e4-136">Create an account on the [Bing Maps Developer Account](https://www.microsoft.com/maps/developers/web.aspx).</span></span> <span data-ttu-id="4b7e4-137">Benötigen Sie ein Microsoft-Konto (Windows Live ID) sowie ein.</span><span class="sxs-lookup"><span data-stu-id="4b7e4-137">You must have a Microsoft account (Windows Live ID) as well.</span></span>

    <span data-ttu-id="4b7e4-138">Sie können angeben, dass Sie die Verwendung des Schlüssels für möchten **Auswertung/Test**.</span><span class="sxs-lookup"><span data-stu-id="4b7e4-138">You can specify that you want to use the key for **Evaluation/Test**.</span></span> <span data-ttu-id="4b7e4-139">Wenn Sie auf dem lokalen Computer mithilfe von WebMatrix und IIS Express die Zuordnungsfunktion testen, rufen Sie die **Website** Arbeitsbereich, und beachten Sie die URL Ihrer Website (z. B. `http://localhost:50408`, obwohl die Portnummer wahrscheinlich unterschiedlich sind).</span><span class="sxs-lookup"><span data-stu-id="4b7e4-139">If you are testing the mapping function on your own computer using WebMatrix and IIS Express, go the **Site** workspace and note the URL of your site (for example, `http://localhost:50408`, although your port number will probably be different).</span></span> <span data-ttu-id="4b7e4-140">Sie können dies *"localhost"* Adresse wie der Standort, wenn Sie sich registrieren.</span><span class="sxs-lookup"><span data-stu-id="4b7e4-140">You can use this *localhost* address as the site when you register.</span></span>
2. <span data-ttu-id="4b7e4-141">Nachdem Sie für ein Konto registriert haben, wechseln Sie zu den Bing Maps Account Center, und klicken Sie auf **erstellen oder Anzeigen von Schlüsseln**:</span><span class="sxs-lookup"><span data-stu-id="4b7e4-141">After you have registered for an account, go to the Bing Maps Account Center and click **Create or view keys**:</span></span>

    ![mapping-2](displaying-maps-in-an-aspnet-web-pages-site/_static/image1.png)
3. <span data-ttu-id="4b7e4-143">Notieren Sie sich den Schlüssel, den Bing erstellt.</span><span class="sxs-lookup"><span data-stu-id="4b7e4-143">Record the key that Bing creates.</span></span>

## <a name="creating-a-map-based-on-an-address-using-google"></a><span data-ttu-id="4b7e4-144">Erstellen einer Zuordnung basierend auf einer Adresse (mithilfe von Google)</span><span class="sxs-lookup"><span data-stu-id="4b7e4-144">Creating a Map Based on an Address (Using Google)</span></span>

<span data-ttu-id="4b7e4-145">Das folgende Beispiel zeigt, wie eine Seite zu erstellen, die eine Zuordnung basierend auf einer Adresse rendert.</span><span class="sxs-lookup"><span data-stu-id="4b7e4-145">The following example shows how to create a page that renders a map based on an address.</span></span> <span data-ttu-id="4b7e4-146">In diesem Beispiel wird gezeigt, wie Google Maps verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="4b7e4-146">This example shows how to use Google Maps.</span></span>

1. <span data-ttu-id="4b7e4-147">Erstellen Sie eine Datei mit dem Namen *MapAddress.cshtml* im Stammverzeichnis der Website.</span><span class="sxs-lookup"><span data-stu-id="4b7e4-147">Create a file named *MapAddress.cshtml* in the root of the site.</span></span> <span data-ttu-id="4b7e4-148">Auf dieser Seite generiert eine Zuordnung basierend auf eine Adresse, die Sie übergeben.</span><span class="sxs-lookup"><span data-stu-id="4b7e4-148">This page will generate a map based on an address that you pass to it.</span></span>
2. <span data-ttu-id="4b7e4-149">Kopieren Sie den folgenden Code in die Datei überschreiben der vorhandenen Inhalts.</span><span class="sxs-lookup"><span data-stu-id="4b7e4-149">Copy the following code into the file, overwriting the existing content.</span></span>

    [!code-cshtml[Main](displaying-maps-in-an-aspnet-web-pages-site/samples/sample1.cshtml)]

    <span data-ttu-id="4b7e4-150">Beachten Sie die folgenden Funktionen von der Seite ":</span><span class="sxs-lookup"><span data-stu-id="4b7e4-150">Notice the following features of the page:</span></span>

    - <span data-ttu-id="4b7e4-151">Die `<script>` Element in der `<head>` Element.</span><span class="sxs-lookup"><span data-stu-id="4b7e4-151">The `<script>` element in the `<head>` element.</span></span> <span data-ttu-id="4b7e4-152">Im Beispiel die `<script>` Elementverweise der *Jquery-1.6.4.min.js* -Datei, die eine verkleinerte (komprimiert) Version der jQuery-Bibliothek Version 1.6.4 ist.</span><span class="sxs-lookup"><span data-stu-id="4b7e4-152">In the example, the `<script>` element references the *jquery-1.6.4.min.js* file, which is a minified (compressed) version of the jQuery library, version 1.6.4.</span></span> <span data-ttu-id="4b7e4-153">Beachten Sie, dass der Verweis annimmt, dass die *js* Datei befindet sich in der *Skripts* Ordner der Website.</span><span class="sxs-lookup"><span data-stu-id="4b7e4-153">Note that the reference assumes that the *.js* file is in the *Scripts* folder of your site.</span></span> 

        > [!NOTE]
        > <span data-ttu-id="4b7e4-154">Wenn Sie eine andere Version von jQuery-Bibliothek verwenden, achten Sie darauf, dass Sie ordnungsgemäß auf diese Version Zeigt sind.</span><span class="sxs-lookup"><span data-stu-id="4b7e4-154">If you're using a different version of the jQuery library, just make sure that you're pointing to that version correctly.</span></span>
    - <span data-ttu-id="4b7e4-155">Der Aufruf der `@Maps.GetGoogleHtml` im Text der Seite.</span><span class="sxs-lookup"><span data-stu-id="4b7e4-155">The call to the `@Maps.GetGoogleHtml` in the body of the page.</span></span> <span data-ttu-id="4b7e4-156">Um eine Adresse zuordnen, müssen Sie eine Adresszeichenfolge übergeben.</span><span class="sxs-lookup"><span data-stu-id="4b7e4-156">To map an address, you must pass an address string.</span></span> <span data-ttu-id="4b7e4-157">Die Methoden für die andere Module der Karte funktionieren auf ähnliche Weise (`@Maps.GetYahooHtml`, `@Maps.GetMapQuestHtml`).</span><span class="sxs-lookup"><span data-stu-id="4b7e4-157">The methods for the other map engines work in a similar way (`@Maps.GetYahooHtml`, `@Maps.GetMapQuestHtml`).</span></span>
3. <span data-ttu-id="4b7e4-158">Führen Sie die Seite, und geben Sie eine Adresse.</span><span class="sxs-lookup"><span data-stu-id="4b7e4-158">Run the page and enter an address.</span></span> <span data-ttu-id="4b7e4-159">Die Seite zeigt eine Zuordnung, basierend auf Google Maps, die den Speicherort zeigt, den Sie angegeben haben.</span><span class="sxs-lookup"><span data-stu-id="4b7e4-159">The page displays a map, based on Google Maps, that shows the location that you specified.</span></span>

     ![mapping-1](displaying-maps-in-an-aspnet-web-pages-site/_static/image2.png)

## <a name="creating-a-map-based-on-latitude-and-longitude-coordinates-using-bing"></a><span data-ttu-id="4b7e4-161">Erstellen einer Zuordnung basierend auf den Breiten- und Längengrad Koordinaten (über Bing)</span><span class="sxs-lookup"><span data-stu-id="4b7e4-161">Creating a Map Based on Latitude and Longitude Coordinates (Using Bing)</span></span>

<span data-ttu-id="4b7e4-162">In diesem Beispiel wird gezeigt, wie zum Erstellen einer Zuordnung basierend auf Koordinaten wird.</span><span class="sxs-lookup"><span data-stu-id="4b7e4-162">This example shows how to create a map based on coordinates.</span></span> <span data-ttu-id="4b7e4-163">Dieses Beispiel zeigt, wie zur Verwendung von Bing Maps und Ihre Bing-Schlüssel enthalten.</span><span class="sxs-lookup"><span data-stu-id="4b7e4-163">This example shows how to use Bing maps and how to include your Bing key.</span></span> <span data-ttu-id="4b7e4-164">(Sie können eine Zuordnung basierend auf die andere Module der Zuordnung auch ohne Verwendung eines Bing-Schlüssels mit Koordinaten erstellen.)</span><span class="sxs-lookup"><span data-stu-id="4b7e4-164">(You can create a map based on coordinates using the other map engines also, without using a Bing key.)</span></span>

1. <span data-ttu-id="4b7e4-165">Erstellen Sie eine Datei mit dem Namen *MapCoordinates.cshtml* im Stammverzeichnis der Website und Ersetzen Sie den vorhandenen Inhalt durch den folgenden Code und Markup:</span><span class="sxs-lookup"><span data-stu-id="4b7e4-165">Create a file named *MapCoordinates.cshtml* in the root of the site and replace the existing content with the following code and markup:</span></span>

    [!code-cshtml[Main](displaying-maps-in-an-aspnet-web-pages-site/samples/sample2.cshtml)]
2. <span data-ttu-id="4b7e4-166">Ersetzen Sie `your-key-here` mit dem Bing Maps-Schlüssel, die Sie zuvor generiert.</span><span class="sxs-lookup"><span data-stu-id="4b7e4-166">Replace `your-key-here` with the Bing Maps key that you generated earlier.</span></span>
3. <span data-ttu-id="4b7e4-167">Führen Sie die *MapCoordinates.cshtml* Seite Geben Sie die Breiten-und Längenkoordinaten, und klicken Sie dann auf die **Map It!**</span><span class="sxs-lookup"><span data-stu-id="4b7e4-167">Run the *MapCoordinates.cshtml* page, enter latitude and longitude coordinates, and then click the **Map It!**</span></span> <span data-ttu-id="4b7e4-168">Schaltfläche.</span><span class="sxs-lookup"><span data-stu-id="4b7e4-168">button.</span></span> <span data-ttu-id="4b7e4-169">(Wenn Sie nicht, dass alle Koordinaten wissen, versuchen Sie Folgendes.</span><span class="sxs-lookup"><span data-stu-id="4b7e4-169">(If you don't know any coordinates, try the following.</span></span> <span data-ttu-id="4b7e4-170">Dies ist ein Speicherort in das Microsoft in Redmond wurde auf.)</span><span class="sxs-lookup"><span data-stu-id="4b7e4-170">This is a location on the Microsoft Redmond campus.)</span></span>

   - <span data-ttu-id="4b7e4-171">Breitengrad: 47.6781005859375</span><span class="sxs-lookup"><span data-stu-id="4b7e4-171">Latitude: 47.6781005859375</span></span>
   - <span data-ttu-id="4b7e4-172">Longitude: -122.158317565918</span><span class="sxs-lookup"><span data-stu-id="4b7e4-172">Longitude: -122.158317565918</span></span>

     <span data-ttu-id="4b7e4-173">Die Seite wird angezeigt, über die Koordinaten, die Sie angegeben haben.</span><span class="sxs-lookup"><span data-stu-id="4b7e4-173">The page is displayed using the coordinates that you specified.</span></span>

     ![mapping-3](displaying-maps-in-an-aspnet-web-pages-site/_static/image3.png)

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a><span data-ttu-id="4b7e4-175">Zusätzliche Ressourcen</span><span class="sxs-lookup"><span data-stu-id="4b7e4-175">Additional Resources</span></span>


[<span data-ttu-id="4b7e4-176">Microsoft.Maps-API-Referenz</span><span class="sxs-lookup"><span data-stu-id="4b7e4-176">Microsoft.Maps API Reference</span></span>](https://msdn.microsoft.com/library/gg427611.aspx)
