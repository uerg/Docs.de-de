---
uid: web-pages/overview/releases/running-v1-and-v2-sites-side-by-side
title: Unterschiedliche Versionen von ASP.NET Web Pages (Razor) parallel ausführen | Microsoft-Dokumentation
author: tfitzmac
description: In diesem Artikel wird erläutert, wie Sie Websites mit ASP.NET Web Pages (Razor) auf dem gleichen Computer oder Server ausführen, wenn die Websites konfiguriert werden, um verschiedene Versionen verwenden...
ms.author: aspnetcontent
ms.date: 02/10/2014
ms.assetid: a861409b-4ae6-4868-9e09-87edfac3535f
msc.legacyurl: /web-pages/overview/releases/running-v1-and-v2-sites-side-by-side
msc.type: authoredcontent
ms.openlocfilehash: 86b1d5babac747eb4fa7ba8abde0e6155c8b17fb
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2018
ms.locfileid: "37810055"
---
<a name="running-different-versions-of-aspnet-web-pages-razor-side-by-side"></a><span data-ttu-id="39b21-103">Verschiedene Versionen von ASP.NET Web Pages (Razor) ausführen parallel</span><span class="sxs-lookup"><span data-stu-id="39b21-103">Running Different Versions of ASP.NET Web Pages (Razor) Side by Side</span></span>
====================
<span data-ttu-id="39b21-104">durch [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="39b21-104">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="39b21-105">In diesem Artikel wird erläutert, wie Websites mit ASP.NET Web Pages (Razor) auf dem gleichen Computer oder Server ausgeführt wird, wenn die Websites konfiguriert werden, um verschiedene Versionen von ASP.NET Web Pages verwenden.</span><span class="sxs-lookup"><span data-stu-id="39b21-105">This article explains how to run ASP.NET Web Pages (Razor) websites on the same computer or server when the websites are configured to use different versions of ASP.NET Web Pages.</span></span>
> 
> <span data-ttu-id="39b21-106">Sie lernen Folgendes:</span><span class="sxs-lookup"><span data-stu-id="39b21-106">What you'll learn:</span></span>
> 
> - <span data-ttu-id="39b21-107">Was ist das Standardverhalten in ASP.NET aus, wenn Sie mit ASP.NET Web Pages erstellten Websites verfügen.</span><span class="sxs-lookup"><span data-stu-id="39b21-107">What the default behavior is in ASP.NET when you have sites built with ASP.NET Web Pages.</span></span>
> - <span data-ttu-id="39b21-108">Vorgehensweise: konfigurieren ein neues Standorts mit einer älteren Version von ASP.NET Web Pages ausführen.</span><span class="sxs-lookup"><span data-stu-id="39b21-108">How to configure a new site to run with an older version of ASP.NET Web Pages.</span></span>
>   
> 
> <span data-ttu-id="39b21-109">Dies ist die ASP.NET-Funktion, die in diesem Artikel eingeführt:</span><span class="sxs-lookup"><span data-stu-id="39b21-109">This is the ASP.NET feature introduced in the article:</span></span>
> 
> - <span data-ttu-id="39b21-110">Die `webPages:Version` Konfigurationseinstellung.</span><span class="sxs-lookup"><span data-stu-id="39b21-110">The `webPages:Version` configuration setting.</span></span>
>   
> 
> ## <a name="software-versions"></a><span data-ttu-id="39b21-111">Software-Versionen</span><span class="sxs-lookup"><span data-stu-id="39b21-111">Software versions</span></span>
> 
> 
> - <span data-ttu-id="39b21-112">ASP.NET Web Pages (Razor) 3</span><span class="sxs-lookup"><span data-stu-id="39b21-112">ASP.NET Web Pages (Razor) 3</span></span>
>   
> 
> <span data-ttu-id="39b21-113">In diesem Tutorial funktioniert auch mit ASP.NET Web Pages 2 und ASP.NET Web Pages-1.0.</span><span class="sxs-lookup"><span data-stu-id="39b21-113">This tutorial also works with ASP.NET Web Pages 2 and ASP.NET Web Pages 1.0.</span></span>


<span data-ttu-id="39b21-114">ASP.NET Web Pages unterstützt die Möglichkeit, Websites parallel auszuführen.</span><span class="sxs-lookup"><span data-stu-id="39b21-114">ASP.NET Web Pages supports the ability to run websites side by side.</span></span> <span data-ttu-id="39b21-115">Dadurch können Sie weiterhin Ihre älteren ASP.NET Web Pages-Anwendungen ausführen, neue ASP.NET Web Pages-Anwendungen erstellen und alle auf demselben Computer ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="39b21-115">This lets you continue to run your older ASP.NET Web Pages applications, build new ASP.NET Web Pages applications, and run all of them on the same computer.</span></span>

<span data-ttu-id="39b21-116">Hier sind einige Punkte zu beachten, wenn Sie die Web Pages mit WebMatrix installieren:</span><span class="sxs-lookup"><span data-stu-id="39b21-116">Here are some things to remember when you install the Web Pages with WebMatrix:</span></span>

- <span data-ttu-id="39b21-117">Standardmäßig werden der vorhandene Web Pages-Anwendungen als letzte Version auf Ihrem Computer ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="39b21-117">By default, existing Web Pages applications will run as the latest version on your computer.</span></span> <span data-ttu-id="39b21-118">(Die Assemblys im globalen Assemblycache (GAC) installiert und werden automatisch verwendet.)</span><span class="sxs-lookup"><span data-stu-id="39b21-118">(The assemblies are installed in the global assembly cache (GAC) and are used automatically.)</span></span>
- <span data-ttu-id="39b21-119">Wenn Sie einen Standort mithilfe einer anderen Version von ASP.NET Web Pages ausführen möchten, können Sie den Standort zu diesem Zweck konfigurieren.</span><span class="sxs-lookup"><span data-stu-id="39b21-119">If you want to run a site using a different version of ASP.NET Web Pages, you can configure the site to do that.</span></span> <span data-ttu-id="39b21-120">Wenn Ihre Website noch keinem *"Web.config"* Datei im Stammverzeichnis der Website, eine neue erstellen, und kopieren Sie das folgende XML, die vorhandene Inhalte überschrieben.</span><span class="sxs-lookup"><span data-stu-id="39b21-120">If your site doesn't already have a *web.config* file in the root of the site, create a new one and copy the following XML into it, overwriting the existing content.</span></span> <span data-ttu-id="39b21-121">Wenn die Website bereits enthält eine *"Web.config"* hinzufügen. ein `<appSettings>` Element wie den folgenden Ausdruck ein, um die `<configuration>` Abschnitt.</span><span class="sxs-lookup"><span data-stu-id="39b21-121">If the site already contains a *web.config* file, add an `<appSettings>` element like the following one to the `<configuration>` section.</span></span>

    [!code-xml[Main](running-v1-and-v2-sites-side-by-side/samples/sample1.xml)]
  <span data-ttu-id="39b21-122">"– Wenn Sie eine Version in nicht angeben der *" Web.config "* -Datei, einen Standort als die aktuelle Version bereitgestellt wird.</span><span class="sxs-lookup"><span data-stu-id="39b21-122">\`- If you do not specify a version in the *web.config* file, a site is deployed as the latest version.</span></span> <span data-ttu-id="39b21-123">(Die Assemblys in kopiert die *Bin* Ordner auf der bereitgestellten Website.)</span><span class="sxs-lookup"><span data-stu-id="39b21-123">(The assemblies are copied to the *bin* folder in the deployed site.)</span></span>
- <span data-ttu-id="39b21-124">Neue Anwendungen, die mithilfe der Websitevorlagen im Web Matrix schließen Sie den Assemblys der Version von Webseiten in der Website *Bin* Ordner.</span><span class="sxs-lookup"><span data-stu-id="39b21-124">New applications that you create using the site templates in Web Matrix include the Web Pages version assemblies in the site's *bin* folder.</span></span>

<span data-ttu-id="39b21-125">Im Allgemeinen, Sie können immer steuern, welche Version von Webseiten mit Ihrer Website verwendet werden soll, mithilfe von NuGet die entsprechenden Assemblys in der Website installieren *Bin* Ordner.</span><span class="sxs-lookup"><span data-stu-id="39b21-125">In general, you can always control which version of Web Pages to use with your site by using NuGet to install the appropriate assemblies into the site's *bin* folder.</span></span> <span data-ttu-id="39b21-126">Um Pakete zu finden, besuchen Sie [NuGet.org](http://NuGet.org).</span><span class="sxs-lookup"><span data-stu-id="39b21-126">To find packages, visit [NuGet.org](http://NuGet.org).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="39b21-127">Zusätzliche Ressourcen</span><span class="sxs-lookup"><span data-stu-id="39b21-127">Additional Resources</span></span>

[<span data-ttu-id="39b21-128">Die wichtigsten Funktionen in ASP.NET Web Pages 2</span><span class="sxs-lookup"><span data-stu-id="39b21-128">The Top Features in ASP.NET Web Pages 2</span></span>](top-features-in-web-pages-2.md)
