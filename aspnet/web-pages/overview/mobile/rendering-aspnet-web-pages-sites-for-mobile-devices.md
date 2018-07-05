---
uid: web-pages/overview/mobile/rendering-aspnet-web-pages-sites-for-mobile-devices
title: Rendern von ASP.NET Web Pages-(Razor)-Websites für Mobilgeräte | Microsoft-Dokumentation
author: tfitzmac
description: 'Dieser Artikel beschreibt, wie Sie Seiten in einer ASP.NET Web Pages (Razor)-Website zu erstellen, die auf mobilen Geräten ordnungsgemäß gerendert wird. Sie lernen Folgendes: wie Sie...'
ms.author: aspnetcontent
ms.date: 02/17/2014
ms.assetid: f15ab392-c05e-4269-83bf-7c6d2b8c8ec8
msc.legacyurl: /web-pages/overview/mobile/rendering-aspnet-web-pages-sites-for-mobile-devices
msc.type: authoredcontent
ms.openlocfilehash: dcc44e0d78814ef9a8537b01efa17259a12e9c76
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2018
ms.locfileid: "37816342"
---
<a name="rendering-aspnet-web-pages-razor-sites-for-mobile-devices"></a><span data-ttu-id="80f02-104">Rendern von ASP.NET Web Pages (Razor)-Websites für Mobilgeräte</span><span class="sxs-lookup"><span data-stu-id="80f02-104">Rendering ASP.NET Web Pages (Razor) Sites for Mobile Devices</span></span>
====================
<span data-ttu-id="80f02-105">durch [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="80f02-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="80f02-106">Dieser Artikel beschreibt, wie Sie Seiten in einer ASP.NET Web Pages (Razor)-Website zu erstellen, die auf mobilen Geräten ordnungsgemäß gerendert wird.</span><span class="sxs-lookup"><span data-stu-id="80f02-106">This article describes how to create pages in an ASP.NET Web Pages (Razor) site that will render appropriately on mobile devices.</span></span>
> 
> <span data-ttu-id="80f02-107">Sie lernen Folgendes:</span><span class="sxs-lookup"><span data-stu-id="80f02-107">What you'll learn:</span></span>
> 
> - <span data-ttu-id="80f02-108">So eine Benennungskonvention zu verwenden, um anzugeben, dass eine Seite, die speziell für mobile Geräte konzipiert ist.</span><span class="sxs-lookup"><span data-stu-id="80f02-108">How to use a naming convention to specify that a page is designed specifically for mobile devices.</span></span>
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="80f02-109">Softwareversionen, die in diesem Tutorial verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="80f02-109">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="80f02-110">ASP.NET Web Pages (Razor) 3</span><span class="sxs-lookup"><span data-stu-id="80f02-110">ASP.NET Web Pages (Razor) 3</span></span>
>   
> 
> <span data-ttu-id="80f02-111">In diesem Tutorial funktioniert auch mit ASP.NET Web Pages 2.</span><span class="sxs-lookup"><span data-stu-id="80f02-111">This tutorial also works with ASP.NET Web Pages 2.</span></span>


<span data-ttu-id="80f02-112">ASP.NET Web Pages ermöglicht das Erstellen benutzerdefinierter angezeigt, die zum Rendern von Inhalt auf Mobilgeräten und anderen Geräten.</span><span class="sxs-lookup"><span data-stu-id="80f02-112">ASP.NET Web Pages lets you create custom displays for rendering content on mobile or other devices.</span></span>

<span data-ttu-id="80f02-113">Die einfachste Möglichkeit zum Erstellen von gerätespezifischen-Seite in einer ASP.NET Web Pages-Website wird mithilfe eines Musters Dateibenennung wie folgt: <em>FileName.</em> <em>Mobile</em><em>.cshtml</em>.</span><span class="sxs-lookup"><span data-stu-id="80f02-113">The simplest way to create device-specific page in an ASP.NET Web Pages site is by using a file-naming pattern like this: <em>FileName.</em><em>Mobile</em><em>.cshtml</em>.</span></span> <span data-ttu-id="80f02-114">Sie können zwei Versionen einer Seite erstellen (z. B. einen benannten <em>MyFile.cshtml</em> und eine mit dem Namen <em>MyFile.Mobile.cshtml</em>).</span><span class="sxs-lookup"><span data-stu-id="80f02-114">You can create two versions of a page (for example, one named <em>MyFile.cshtml</em> and one named <em>MyFile.Mobile.cshtml</em>).</span></span> <span data-ttu-id="80f02-115">Zur Laufzeit, wenn ein mobiles Gerät fordert <em>MyFile.cshtml</em>, rendert den Inhalt von ASP.NET <em>MyFile.Mobile.cshtml</em>.</span><span class="sxs-lookup"><span data-stu-id="80f02-115">At run time, when a mobile device requests <em>MyFile.cshtml</em>, ASP.NET renders the content from <em>MyFile.Mobile.cshtml</em>.</span></span> <span data-ttu-id="80f02-116">Andernfalls <em>MyFile.cshtml</em> gerendert wird.</span><span class="sxs-lookup"><span data-stu-id="80f02-116">Otherwise, <em>MyFile.cshtml</em> is rendered.</span></span>

<span data-ttu-id="80f02-117">Das folgende Beispiel zeigt, wie mobile renderingaufträge durch Hinzufügen einer Inhaltsseite für mobile Geräte.</span><span class="sxs-lookup"><span data-stu-id="80f02-117">The following example shows how to enable mobile rendering by adding a content page for mobile devices.</span></span> <span data-ttu-id="80f02-118">*Page1.cshtml* enthält Inhalte sowie eine navigationsrandleiste.</span><span class="sxs-lookup"><span data-stu-id="80f02-118">*Page1.cshtml* contains content plus a navigation sidebar.</span></span> <span data-ttu-id="80f02-119">*Page1.Mobile.cshtml* den gleichen Inhalt enthält, lässt aber die Randleiste.</span><span class="sxs-lookup"><span data-stu-id="80f02-119">*Page1.Mobile.cshtml* contains the same content, but omits the sidebar.</span></span>

1. <span data-ttu-id="80f02-120">Erstellen Sie in einer ASP.NET Web Pages-Website, eine Datei namens *Page1.cshtml* , und Ersetzen Sie den aktuellen Inhalt durch Folgendes Markup.</span><span class="sxs-lookup"><span data-stu-id="80f02-120">In an ASP.NET Web Pages site, create a file named *Page1.cshtml* and replace the current content with following markup.</span></span>

    [!code-html[Main](rendering-aspnet-web-pages-sites-for-mobile-devices/samples/sample1.html)]
2. <span data-ttu-id="80f02-121">Erstellen Sie eine Datei mit dem Namen *Page1.Mobile.cshtml* , und Ersetzen Sie den vorhandenen Inhalt durch Folgendes Markup.</span><span class="sxs-lookup"><span data-stu-id="80f02-121">Create a file named *Page1.Mobile.cshtml* and replace the existing content with the following markup.</span></span> <span data-ttu-id="80f02-122">Beachten Sie, dass es sich bei die mobile Version der Seite der Navigationsbereich für eine bessere Rendern auf einer kleineren Bildschirm lässt.</span><span class="sxs-lookup"><span data-stu-id="80f02-122">Notice that the mobile version of the page omits the navigation section for better rendering on a smaller screen.</span></span>

    [!code-html[Main](rendering-aspnet-web-pages-sites-for-mobile-devices/samples/sample2.html)]
3. <span data-ttu-id="80f02-123">Führen Sie einen desktop-Browser und navigieren Sie zu *Page1.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="80f02-123">Run a desktop browser and browse to *Page1.cshtml*.</span></span> <span data-ttu-id="80f02-124">![mobilesites-1](rendering-aspnet-web-pages-sites-for-mobile-devices/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="80f02-124">![mobilesites-1](rendering-aspnet-web-pages-sites-for-mobile-devices/_static/image1.png)</span></span>
4. <span data-ttu-id="80f02-125">Führen Sie einen mobilen Browser (oder einen Emulator für mobile Geräte), und navigieren Sie zu *Page1.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="80f02-125">Run a mobile browser (or a mobile device emulator) and browse to *Page1.cshtml*.</span></span> <span data-ttu-id="80f02-126">(Beachten Sie, die Sie nicht einschließen, *.mobile.*</span><span class="sxs-lookup"><span data-stu-id="80f02-126">(Notice that you do not include *.mobile.*</span></span> <span data-ttu-id="80f02-127">als Teil der URL.) Obwohl die Anforderung ist *Page1.cshtml*, rendert ASP.NET *Page1.Mobile.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="80f02-127">as part of the URL.) Even though the request is to *Page1.cshtml*, ASP.NET renders *Page1.Mobile.cshtml*.</span></span>

    ![mobilesites-2](rendering-aspnet-web-pages-sites-for-mobile-devices/_static/image2.png)

> [!NOTE]
> <span data-ttu-id="80f02-129">Um mobiler Seiten zu testen, können Sie einen Simulator für mobile Geräte, der auf einem Desktopcomputer ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="80f02-129">To test mobile pages, you can use a mobile device simulator that runs on a desktop computer.</span></span> <span data-ttu-id="80f02-130">Mit diesem Tool können Sie Webseiten zu testen, wie sie auf mobilen Geräten aussehen würde (d. h. in der Regel eine viel kleinere anzeigen mit Bereich).</span><span class="sxs-lookup"><span data-stu-id="80f02-130">This tool lets you test web pages as they would look on mobile devices (that is, typically with a much smaller display area).</span></span> <span data-ttu-id="80f02-131">Ein Beispiel für einen Simulator ist die [Benutzer-Agent Switcher-Add-On](http://addons.mozilla.org/firefox/addon/user-agent-switcher/) für Mozilla Firefox, sodass Sie verschiedenen mobilen Browsern aus einer desktop-Version von Firefox zu emulieren.</span><span class="sxs-lookup"><span data-stu-id="80f02-131">One example of a simulator is the [User Agent Switcher add-on](http://addons.mozilla.org/firefox/addon/user-agent-switcher/) for Mozilla Firefox, which lets you emulate various mobile browsers from a desktop version of Firefox.</span></span>


<a id="Additional_Resources"></a>
## <a name="additional-resources"></a><span data-ttu-id="80f02-132">Zusätzliche Ressourcen</span><span class="sxs-lookup"><span data-stu-id="80f02-132">Additional Resources</span></span>


<span data-ttu-id="80f02-133">[Windows Phone-Emulator](https://msdn.microsoft.com/library/ff402563(v=VS.92).aspx)</span><span class="sxs-lookup"><span data-stu-id="80f02-133">[Windows Phone Emulator](https://msdn.microsoft.com/library/ff402563(v=VS.92).aspx)</span></span>
