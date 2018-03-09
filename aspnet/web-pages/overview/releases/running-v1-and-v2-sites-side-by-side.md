---
uid: web-pages/overview/releases/running-v1-and-v2-sites-side-by-side
title: "Unterschiedliche Versionen von ASP.NET Web Pages (Razor) parallel ausführen | Microsoft Docs"
author: tfitzmac
description: "In diesem Artikel wird erläutert, wie ASP.NET Web Pages (Razor)-Websites auf dem gleichen Computer oder Server ausgeführt wird, wenn die Websites konfiguriert sind, unterschiedliche Versionen verwenden..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/10/2014
ms.topic: article
ms.assetid: a861409b-4ae6-4868-9e09-87edfac3535f
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/releases/running-v1-and-v2-sites-side-by-side
msc.type: authoredcontent
ms.openlocfilehash: c11399b0bde59d18fa378ed48c15844454c1f956
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="running-different-versions-of-aspnet-web-pages-razor-side-by-side"></a><span data-ttu-id="1f400-103">Verschiedene Versionen von ASP.NET Web Pages (Razor) nebeneinander ausgeführt</span><span class="sxs-lookup"><span data-stu-id="1f400-103">Running Different Versions of ASP.NET Web Pages (Razor) Side by Side</span></span>
====================
<span data-ttu-id="1f400-104">durch [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="1f400-104">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="1f400-105">In diesem Artikel erläutert das Ausführen von ASP.NET Web Pages (Razor) Websites auf dem gleichen Computer oder Server, wenn die Websites mit verschiedenen Versionen von ASP.NET Web Pages konfiguriert sind.</span><span class="sxs-lookup"><span data-stu-id="1f400-105">This article explains how to run ASP.NET Web Pages (Razor) websites on the same computer or server when the websites are configured to use different versions of ASP.NET Web Pages.</span></span>
> 
> <span data-ttu-id="1f400-106">Lernen Sie:</span><span class="sxs-lookup"><span data-stu-id="1f400-106">What you'll learn:</span></span>
> 
> - <span data-ttu-id="1f400-107">Was ist das Standardverhalten in ASP.NET aus, wenn Sie mit ASP.NET Web Pages erstellten Websites verfügen.</span><span class="sxs-lookup"><span data-stu-id="1f400-107">What the default behavior is in ASP.NET when you have sites built with ASP.NET Web Pages.</span></span>
> - <span data-ttu-id="1f400-108">So konfigurieren Sie einen neuen Standort mit einer älteren Version von ASP.NET Web Pages ausgeführt werden.</span><span class="sxs-lookup"><span data-stu-id="1f400-108">How to configure a new site to run with an older version of ASP.NET Web Pages.</span></span>
>   
> 
> <span data-ttu-id="1f400-109">Dies ist die ASP.NET-Funktion im Artikel eingeführt:</span><span class="sxs-lookup"><span data-stu-id="1f400-109">This is the ASP.NET feature introduced in the article:</span></span>
> 
> - <span data-ttu-id="1f400-110">Die `webPages:Version` -Konfigurationseinstellung.</span><span class="sxs-lookup"><span data-stu-id="1f400-110">The `webPages:Version` configuration setting.</span></span>
>   
> 
> ## <a name="software-versions"></a><span data-ttu-id="1f400-111">Versionen der Software</span><span class="sxs-lookup"><span data-stu-id="1f400-111">Software versions</span></span>
> 
> 
> - <span data-ttu-id="1f400-112">ASP.NET Web Pages (Razor) 3</span><span class="sxs-lookup"><span data-stu-id="1f400-112">ASP.NET Web Pages (Razor) 3</span></span>
>   
> 
> <span data-ttu-id="1f400-113">Dieses Lernprogramm funktioniert auch mit ASP.NET Web Pages 2 und ASP.NET Web Pages-1.0.</span><span class="sxs-lookup"><span data-stu-id="1f400-113">This tutorial also works with ASP.NET Web Pages 2 and ASP.NET Web Pages 1.0.</span></span>


<span data-ttu-id="1f400-114">ASP.NET Web Pages unterstützt die Fähigkeit zum Ausführen von Websites nebeneinander angezeigt werden.</span><span class="sxs-lookup"><span data-stu-id="1f400-114">ASP.NET Web Pages supports the ability to run websites side by side.</span></span> <span data-ttu-id="1f400-115">Dadurch können Sie weiterhin Ihre älteren ASP.NET Web Pages-Anwendungen ausführen, neue ASP.NET Web Pages-Anwendungen erstellen und alle von ihnen auf dem gleichen Computer ausführen.</span><span class="sxs-lookup"><span data-stu-id="1f400-115">This lets you continue to run your older ASP.NET Web Pages applications, build new ASP.NET Web Pages applications, and run all of them on the same computer.</span></span>

<span data-ttu-id="1f400-116">Hier sind einige Punkte zu beachten, wenn Sie die Webseiten mit WebMatrix installieren:</span><span class="sxs-lookup"><span data-stu-id="1f400-116">Here are some things to remember when you install the Web Pages with WebMatrix:</span></span>

- <span data-ttu-id="1f400-117">Standardmäßig werden die vorhandene Web Pages-Anwendungen als letzte Version auf Ihrem Computer ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="1f400-117">By default, existing Web Pages applications will run as the latest version on your computer.</span></span> <span data-ttu-id="1f400-118">(Die Assemblys im globalen Assemblycache (GAC) installiert und werden automatisch verwendet.)</span><span class="sxs-lookup"><span data-stu-id="1f400-118">(The assemblies are installed in the global assembly cache (GAC) and are used automatically.)</span></span>
- <span data-ttu-id="1f400-119">Wenn Sie eine Website mit einer anderen Version von ASP.NET Web Pages ausführen möchten, können Sie den Standort zu diesem Zweck konfigurieren.</span><span class="sxs-lookup"><span data-stu-id="1f400-119">If you want to run a site using a different version of ASP.NET Web Pages, you can configure the site to do that.</span></span> <span data-ttu-id="1f400-120">Wenn Ihre Website noch keinem *"Web.config"* Datei im Stammverzeichnis der Site, ein neues erstellen und kopieren Sie die folgenden XML-Code hinein, Überschreiben der vorhandenen Inhalts.</span><span class="sxs-lookup"><span data-stu-id="1f400-120">If your site doesn't already have a *web.config* file in the root of the site, create a new one and copy the following XML into it, overwriting the existing content.</span></span> <span data-ttu-id="1f400-121">Wenn der Standort bereits enthält eine *"Web.config"* hinzufügen. ein `<appSettings>` Element wie den folgenden Ausdruck ein, um die `<configuration>` Abschnitt.</span><span class="sxs-lookup"><span data-stu-id="1f400-121">If the site already contains a *web.config* file, add an `<appSettings>` element like the following one to the `<configuration>` section.</span></span>

    [!code-xml[Main](running-v1-and-v2-sites-side-by-side/samples/sample1.xml)]
<span data-ttu-id="1f400-122">"-Wenn Sie keine Version im Angeben der *" Web.config "* Datei, einen Standort als letzte Version bereitgestellt wird.</span><span class="sxs-lookup"><span data-stu-id="1f400-122">\`- If you do not specify a version in the *web.config* file, a site is deployed as the latest version.</span></span> <span data-ttu-id="1f400-123">(Die Assemblys werden in kopiert die *"bin"* Ordner auf der Website bereitgestellt.)</span><span class="sxs-lookup"><span data-stu-id="1f400-123">(The assemblies are copied to the *bin* folder in the deployed site.)</span></span>
- <span data-ttu-id="1f400-124">Neue Anwendungen, die mithilfe der Websitevorlagen im Web Matrix gehören die Assemblys der Web Pages-Version am Standort *"bin"* Ordner.</span><span class="sxs-lookup"><span data-stu-id="1f400-124">New applications that you create using the site templates in Web Matrix include the Web Pages version assemblies in the site's *bin* folder.</span></span>

<span data-ttu-id="1f400-125">Im Allgemeinen können Sie immer steuern, welche Version von Webseiten für Ihre Website verwendet werden, mithilfe von NuGet die entsprechenden Assemblys in der Website installieren *"bin"* Ordner.</span><span class="sxs-lookup"><span data-stu-id="1f400-125">In general, you can always control which version of Web Pages to use with your site by using NuGet to install the appropriate assemblies into the site's *bin* folder.</span></span> <span data-ttu-id="1f400-126">Um Pakete zu suchen, besuchen Sie [NuGet.org](http://NuGet.org).</span><span class="sxs-lookup"><span data-stu-id="1f400-126">To find packages, visit [NuGet.org](http://NuGet.org).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="1f400-127">Zusätzliche Ressourcen</span><span class="sxs-lookup"><span data-stu-id="1f400-127">Additional Resources</span></span>

[<span data-ttu-id="1f400-128">Die wichtigsten Funktionen in ASP.NET Web Pages 2</span><span class="sxs-lookup"><span data-stu-id="1f400-128">The Top Features in ASP.NET Web Pages 2</span></span>](top-features-in-web-pages-2.md)
