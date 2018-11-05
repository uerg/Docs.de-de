---
uid: web-pages/overview/routing/creating-readable-urls-in-aspnet-web-pages-sites
title: Erstellen von lesbaren URLs in der ASP.NET Web Pages (Razor) Sites | Microsoft-Dokumentation
author: Rick-Anderson
description: Dieser Artikel beschreibt die in einer Website für ASP.NET Web Pages (Razor), und wie diese können Sie URLs verwenden, die besser lesbar und besser für SEO-routing. Was sind Sie in der...
ms.author: riande
ms.date: 02/17/2014
ms.assetid: a8aac1ac-89de-4415-afe0-97a41c6423d2
msc.legacyurl: /web-pages/overview/routing/creating-readable-urls-in-aspnet-web-pages-sites
msc.type: authoredcontent
ms.openlocfilehash: 26d8f94b2e38fe5205a37e3d37b4e3bd509a3874
ms.sourcegitcommit: 2d3e5422d530203efdaf2014d1d7df31f88d08d0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/05/2018
ms.locfileid: "51021081"
---
<a name="creating-readable-urls-in-aspnet-web-pages-razor-sites"></a><span data-ttu-id="fc782-104">Erstellen von lesbaren URLs in ASP.NET Web Pages (Razor)-Websites</span><span class="sxs-lookup"><span data-stu-id="fc782-104">Creating Readable URLs in ASP.NET Web Pages (Razor) Sites</span></span>
====================
<span data-ttu-id="fc782-105">durch [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="fc782-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="fc782-106">Dieser Artikel beschreibt die in einer Website für ASP.NET Web Pages (Razor), und wie diese können Sie URLs verwenden, die besser lesbar und besser für SEO-routing.</span><span class="sxs-lookup"><span data-stu-id="fc782-106">This article describes routing in an ASP.NET Web Pages (Razor) website, and how this lets you use URLs that are more readable and better for SEO.</span></span>
> 
> <span data-ttu-id="fc782-107">Sie lernen Folgendes:</span><span class="sxs-lookup"><span data-stu-id="fc782-107">What you'll learn:</span></span>
> 
> - <span data-ttu-id="fc782-108">Wie ASP.NET verwendet routing, um Sie besser lesbar und durchsuchbaren URLs verwenden können.</span><span class="sxs-lookup"><span data-stu-id="fc782-108">How ASP.NET uses routing to let you use more readable and searchable URLs.</span></span>
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="fc782-109">Softwareversionen, die in diesem Tutorial verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="fc782-109">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="fc782-110">ASP.NET Web Pages (Razor) 3</span><span class="sxs-lookup"><span data-stu-id="fc782-110">ASP.NET Web Pages (Razor) 3</span></span>
>   
> 
> <span data-ttu-id="fc782-111">In diesem Tutorial funktioniert auch mit ASP.NET Web Pages 2.</span><span class="sxs-lookup"><span data-stu-id="fc782-111">This tutorial also works with ASP.NET Web Pages 2.</span></span>


## <a name="about-routing"></a><span data-ttu-id="fc782-112">Informationen zum Routing</span><span class="sxs-lookup"><span data-stu-id="fc782-112">About Routing</span></span>

<span data-ttu-id="fc782-113">Die URLs für die Seiten auf Ihrer Website haben Auswirkungen auf die Website wie gut funktioniert.</span><span class="sxs-lookup"><span data-stu-id="fc782-113">The URLs for the pages in your site can have an impact on how well the site works.</span></span> <span data-ttu-id="fc782-114">Eine URL, die &quot;benutzerfreundliche&quot; können sie leichter für Personen, die Website zu verwenden.</span><span class="sxs-lookup"><span data-stu-id="fc782-114">A URL that's &quot;friendly&quot; can make it easier for people to use the site.</span></span> <span data-ttu-id="fc782-115">Sie können auch mit der Such-Engine Optimization (SEO) für den Standort.</span><span class="sxs-lookup"><span data-stu-id="fc782-115">It can also help with search-engine optimization (SEO) for the site.</span></span> <span data-ttu-id="fc782-116">ASP.NET-Websites umfassen die Möglichkeit, freundlichen URLs automatisch verwenden.</span><span class="sxs-lookup"><span data-stu-id="fc782-116">ASP.NET websites include the ability to use friendly URLs automatically.</span></span>

<span data-ttu-id="fc782-117">ASP.NET können Sie aussagekräftige URLs zu erstellen, die Aktionen des Benutzers statt nur auf eine Datei auf dem Server Zeigt beschreiben.</span><span class="sxs-lookup"><span data-stu-id="fc782-117">ASP.NET lets you create meaningful URLs that describe user actions instead of just pointing to a file on the server.</span></span> <span data-ttu-id="fc782-118">Betrachten Sie diese URLs für einen fiktiven Blog:</span><span class="sxs-lookup"><span data-stu-id="fc782-118">Consider these URLs for a fictional blog:</span></span>

- `http://www.contoso.com/Blog/blog.cshtml?categories=hardware`
- `http://www.contoso.com//Blog/blog.cshtml?startdate=2009-11-01&enddate=2009-11-30`

<span data-ttu-id="fc782-119">Vergleichen Sie diese URLs, die unten aufgeführten:</span><span class="sxs-lookup"><span data-stu-id="fc782-119">Compare those URLs to the following ones:</span></span>

- `http://www.contoso.com/Blog/categories/hardware/`
- `http://www.contoso.com/Blog/2009/November`

<span data-ttu-id="fc782-120">Im ersten Paar ist, muss ein Benutzer wissen, dass das Blog mit angezeigt wird der *blog.cshtml* Seite und müsste dann eine Abfragezeichenfolge erstellt, die die richtige Kategorie oder den Datumsbereich abruft.</span><span class="sxs-lookup"><span data-stu-id="fc782-120">In the first pair, a user would have to know that the blog is displayed using the *blog.cshtml* page, and would then have to construct a query string that gets the right category or date range.</span></span> <span data-ttu-id="fc782-121">Die zweite Gruppe der Beispiele ist viel einfacher zu verstehen und zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="fc782-121">The second set of examples is much easier to comprehend and create.</span></span>

<span data-ttu-id="fc782-122">Zeigen Sie die URLs für das erste Beispiel auch direkt auf eine bestimmte Datei (*blog.cshtml*).</span><span class="sxs-lookup"><span data-stu-id="fc782-122">The URLs for the first example also point directly to a specific file (*blog.cshtml*).</span></span> <span data-ttu-id="fc782-123">Wenn aus irgendeinem Grund im Blog in einen anderen Ordner auf dem Server verschoben wurden, oder im Blog neu geschrieben wurden, um eine andere Seite verwenden, würde die Links auch falsch sein.</span><span class="sxs-lookup"><span data-stu-id="fc782-123">If for some reason the blog were moved to another folder on the server, or if the blog were rewritten to use a different page, the links would be wrong.</span></span> <span data-ttu-id="fc782-124">Der zweite Satz von URLs nicht mit einer bestimmten Seite, zeigen Sie würden also selbst, wenn die Blog-Implementierung oder den Speicherort geändert, die URLs weiterhin gültig sein.</span><span class="sxs-lookup"><span data-stu-id="fc782-124">The second set of URLs doesn't point to a specific page, so even if the blog implementation or location changes, the URLs would still be valid.</span></span>

<span data-ttu-id="fc782-125">In ASP.NET Web Pages, können Sie freundlicher URLs wie in den obigen Beispielen erstellen, da ASP.NET verwendet *routing*.</span><span class="sxs-lookup"><span data-stu-id="fc782-125">In ASP.NET Web Pages, you can create friendlier URLs like those in the above examples because ASP.NET uses *routing*.</span></span> <span data-ttu-id="fc782-126">Routing erstellt logische Zuordnung über eine URL zu einer Seite (Seiten), die die Anforderung erfüllen kann.</span><span class="sxs-lookup"><span data-stu-id="fc782-126">Routing creates logical mapping from a URL to a page (or pages) that can fulfill the request.</span></span> <span data-ttu-id="fc782-127">Da die Zuordnung logischer ist (nicht physischen, auf eine bestimmte Datei), routing bietet viel Flexibilität bei, wie Sie die URLs für Ihre Website definieren.</span><span class="sxs-lookup"><span data-stu-id="fc782-127">Because the mapping is logical (not physical, to a specific file), routing provides great flexibility in how you define the URLs for your site.</span></span>

## <a name="how-routing-works"></a><span data-ttu-id="fc782-128">Funktionsweise des Routing</span><span class="sxs-lookup"><span data-stu-id="fc782-128">How Routing Works</span></span>

<span data-ttu-id="fc782-129">Wenn ASP.NET eine Anforderung verarbeitet, liest er die URL, um zu bestimmen, wie die Weiterleitung erfolgt.</span><span class="sxs-lookup"><span data-stu-id="fc782-129">When ASP.NET processes a request, it reads the URL to determine how to route it.</span></span> <span data-ttu-id="fc782-130">ASP.NET versucht, einzelne Segmente der URL, um Dateien auf dem Datenträger, von links nach rechts.</span><span class="sxs-lookup"><span data-stu-id="fc782-130">ASP.NET tries to match individual segments of the URL to files on disk, going from left to right.</span></span> <span data-ttu-id="fc782-131">Wenn eine Übereinstimmung vorliegt, wird nichts verbleiben in der URL übergeben, auf die Seite als *Pfadinformationen*.</span><span class="sxs-lookup"><span data-stu-id="fc782-131">If there's a match, anything remaining in the URL is passed to the page as *path information*.</span></span>

<span data-ttu-id="fc782-132">Stellen Sie sich, dass jemand eine Anforderung, die über diese URL sendet:</span><span class="sxs-lookup"><span data-stu-id="fc782-132">Imagine that someone makes a request using this URL:</span></span>

`http://www.contoso.com/a/b/c`

<span data-ttu-id="fc782-133">Die Suche läuft folgendermaßen ab:</span><span class="sxs-lookup"><span data-stu-id="fc782-133">The search goes like this:</span></span>

1. <span data-ttu-id="fc782-134">Gibt es eine Datei mit den Pfad und Namen der */a/b/c.cshtml*?</span><span class="sxs-lookup"><span data-stu-id="fc782-134">Is there a file with the path and name of */a/b/c.cshtml*?</span></span> <span data-ttu-id="fc782-135">Wenn dies der Fall ist, führen Sie diese Seite, und weitergeben Sie es sind keine Informationen an die Klasse.</span><span class="sxs-lookup"><span data-stu-id="fc782-135">If so, run that page and pass no information to it.</span></span> <span data-ttu-id="fc782-136">Andernfalls...</span><span class="sxs-lookup"><span data-stu-id="fc782-136">Otherwise ...</span></span>
2. <span data-ttu-id="fc782-137">Gibt es eine Datei mit den Pfad und Namen der */a/b.cshtml*?</span><span class="sxs-lookup"><span data-stu-id="fc782-137">Is there a file with the path and name of */a/b.cshtml*?</span></span> <span data-ttu-id="fc782-138">Falls Ja, führen Sie diese Seite, und übergeben Sie den Wert `c` zuzuweisen.</span><span class="sxs-lookup"><span data-stu-id="fc782-138">If so, run that page and pass the value `c` to it.</span></span> <span data-ttu-id="fc782-139">Andernfalls...</span><span class="sxs-lookup"><span data-stu-id="fc782-139">Otherwise …</span></span>
3. <span data-ttu-id="fc782-140">Gibt es eine Datei mit den Pfad und Namen der */a.cshtml*?</span><span class="sxs-lookup"><span data-stu-id="fc782-140">Is there a file with the path and name of */a.cshtml*?</span></span> <span data-ttu-id="fc782-141">Falls Ja, führen Sie diese Seite, und übergeben Sie den Wert `b/c` zuzuweisen.</span><span class="sxs-lookup"><span data-stu-id="fc782-141">If so, run that page and pass the value `b/c` to it.</span></span>

<span data-ttu-id="fc782-142">Wenn für die Suche finden Sie nicht genau übereinstimmt *.cshtml* -Dateien in ihre angegebene Ordner ASP.NET bleibt auch weiterhin im Gegenzug für diese Dateien zu suchen:</span><span class="sxs-lookup"><span data-stu-id="fc782-142">If the search found no exact matches for *.cshtml* files in their specified folders, ASP.NET continues looking for these files in turn:</span></span>

1. <span data-ttu-id="fc782-143">*/a/b/c/default.cshtml* (keine Pfadinformationen).</span><span class="sxs-lookup"><span data-stu-id="fc782-143">*/a/b/c/default.cshtml* (no path information).</span></span>
2. <span data-ttu-id="fc782-144">*/a/b/c/Index.cshtml* (keine Pfadinformationen).</span><span class="sxs-lookup"><span data-stu-id="fc782-144">*/a/b/c/index.cshtml* (no path information).</span></span>

> [!NOTE]
> <span data-ttu-id="fc782-145">Zur Anforderungen für bestimmte Seiten (d.h. Anforderungen, die enthalten die *.cshtml* Dateierweiterung) funktionieren nur, wie Sie erwarten.</span><span class="sxs-lookup"><span data-stu-id="fc782-145">To be clear, requests for specific pages (that is, requests that include the *.cshtml* filename extension) work just like you'd expect.</span></span> <span data-ttu-id="fc782-146">Eine Anforderung wie `http://www.contoso.com/a/b.cshtml` führen Sie die Seite *"b.cshtml"* einwandfrei.</span><span class="sxs-lookup"><span data-stu-id="fc782-146">A request like `http://www.contoso.com/a/b.cshtml` will run the page *b.cshtml* just fine.</span></span>


<span data-ttu-id="fc782-147">Innerhalb einer Seite erhalten Sie die Pfadinformationen, mit der `UrlData` -Eigenschaft, die ein Wörterbuch handelt.</span><span class="sxs-lookup"><span data-stu-id="fc782-147">Inside a page, you can get the path information via the page's `UrlData` property, which is a dictionary.</span></span> <span data-ttu-id="fc782-148">Angenommen, Sie haben, dass eine Datei namens *ViewCustomers.cshtml* und Ihre Website ruft diese Anforderung ab:</span><span class="sxs-lookup"><span data-stu-id="fc782-148">Imagine that you have a file named *ViewCustomers.cshtml* and your site gets this request:</span></span>

`http://mysite.com/myWebSite/ViewCustomers/1000`

<span data-ttu-id="fc782-149">Wie in der oben genannten Regeln beschrieben, wird die Anforderung zur Ihre Seite wechseln.</span><span class="sxs-lookup"><span data-stu-id="fc782-149">As described in the rules above, the request will go to your page.</span></span> <span data-ttu-id="fc782-150">In der Seite können Sie Code wie den folgenden abrufen und Anzeigen von Informationen über die Pfade (in diesem Fall wird der Wert &quot;1000&quot;):</span><span class="sxs-lookup"><span data-stu-id="fc782-150">Inside the page, you can use code like the following to get and display the path information (in this case, the value &quot;1000&quot;):</span></span>

[!code-html[Main](creating-readable-urls-in-aspnet-web-pages-sites/samples/sample1.html)]

> [!NOTE]
> <span data-ttu-id="fc782-151">Da routing vollständige Dateinamen beinhalten nicht möglich Mehrdeutigkeiten bei Seiten, die den gleichen Namen aber unterschiedliche Dateinamenerweiterungen (z. B. *MyPage.cshtml* und *MyPage.html*) .</span><span class="sxs-lookup"><span data-stu-id="fc782-151">Because routing doesn't involve complete file names, there can be ambiguity if you have pages that have the same name but different file-name extensions (for example, *MyPage.cshtml* and *MyPage.html*).</span></span> <span data-ttu-id="fc782-152">Um Probleme mit dem routing zu vermeiden, empfiehlt es sich um sicherzustellen, dass Sie Seiten auf Ihrer Website nicht nur in ihrer Erweiterung, deren Namen zu unterscheiden.</span><span class="sxs-lookup"><span data-stu-id="fc782-152">In order to avoid problems with routing, it's best to make sure that you don't have pages in your site whose names differ only in their extension.</span></span>


<a id="Additional_Resources"></a>
## <a name="additional-resources"></a><span data-ttu-id="fc782-153">Zusätzliche Ressourcen</span><span class="sxs-lookup"><span data-stu-id="fc782-153">Additional Resources</span></span>

<span data-ttu-id="fc782-154">[WebMatrix - URLs, UrlData und Routing für SEO](http://www.mikesdotnetting.com/Article/165/WebMatrix-URLs-UrlData-and-Routing-for-SEO).</span><span class="sxs-lookup"><span data-stu-id="fc782-154">[WebMatrix - URLs, UrlData and Routing for SEO](http://www.mikesdotnetting.com/Article/165/WebMatrix-URLs-UrlData-and-Routing-for-SEO).</span></span> <span data-ttu-id="fc782-155">In diesem Blogeintrag von Mike Brind enthält einige weitere Details auf der Funktionsweise von routing in ASP.NET Web Pages.</span><span class="sxs-lookup"><span data-stu-id="fc782-155">This blog entry by Mike Brind provides some additional details on how routing works in ASP.NET Web Pages.</span></span>
