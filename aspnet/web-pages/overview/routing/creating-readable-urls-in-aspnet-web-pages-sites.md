---
uid: web-pages/overview/routing/creating-readable-urls-in-aspnet-web-pages-sites
title: Lesbare URLs erstellen, in der ASP.NET Web Pages (Razor)-Websites | Microsoft Docs
author: tfitzmac
description: "Dieser Artikel beschreibt in einer Website für ASP.NET Web Pages (Razor), und wie können Sie die URLs verwenden, die besser lesbar und besser für SEO routing. Was sind Sie in der..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/17/2014
ms.topic: article
ms.assetid: a8aac1ac-89de-4415-afe0-97a41c6423d2
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/routing/creating-readable-urls-in-aspnet-web-pages-sites
msc.type: authoredcontent
ms.openlocfilehash: 7858b7cbd6dccafb2867ed9a1d102561e211e435
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="creating-readable-urls-in-aspnet-web-pages-razor-sites"></a><span data-ttu-id="2fa85-104">Erstellen lesbare URLs in ASP.NET Web Pages (Razor)-Websites</span><span class="sxs-lookup"><span data-stu-id="2fa85-104">Creating Readable URLs in ASP.NET Web Pages (Razor) Sites</span></span>
====================
<span data-ttu-id="2fa85-105">durch [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="2fa85-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="2fa85-106">Dieser Artikel beschreibt in einer Website für ASP.NET Web Pages (Razor), und wie können Sie die URLs verwenden, die besser lesbar und besser für SEO routing.</span><span class="sxs-lookup"><span data-stu-id="2fa85-106">This article describes routing in an ASP.NET Web Pages (Razor) website, and how this lets you use URLs that are more readable and better for SEO.</span></span>
> 
> <span data-ttu-id="2fa85-107">Lernen Sie:</span><span class="sxs-lookup"><span data-stu-id="2fa85-107">What you'll learn:</span></span>
> 
> - <span data-ttu-id="2fa85-108">Wie ASP.NET verwendet routing, um Sie besser lesbar und durchsuchbaren URLs verwenden können.</span><span class="sxs-lookup"><span data-stu-id="2fa85-108">How ASP.NET uses routing to let you use more readable and searchable URLs.</span></span>
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="2fa85-109">In diesem Lernprogramm verwendeten Versionen der Software</span><span class="sxs-lookup"><span data-stu-id="2fa85-109">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="2fa85-110">ASP.NET Web Pages (Razor) 3</span><span class="sxs-lookup"><span data-stu-id="2fa85-110">ASP.NET Web Pages (Razor) 3</span></span>
>   
> 
> <span data-ttu-id="2fa85-111">Dieses Lernprogramm funktioniert auch mit ASP.NET Web Pages 2.</span><span class="sxs-lookup"><span data-stu-id="2fa85-111">This tutorial also works with ASP.NET Web Pages 2.</span></span>


## <a name="about-routing"></a><span data-ttu-id="2fa85-112">Zum Routing</span><span class="sxs-lookup"><span data-stu-id="2fa85-112">About Routing</span></span>

<span data-ttu-id="2fa85-113">Die URLs für die Seiten auf Ihrer Website möglich wirkt sich auf die Website wie gut funktioniert.</span><span class="sxs-lookup"><span data-stu-id="2fa85-113">The URLs for the pages in your site can have an impact on how well the site works.</span></span> <span data-ttu-id="2fa85-114">Eine URL, die &quot;angezeigten&quot; Ausgabenanzeige für Personen, die Website zu verwenden.</span><span class="sxs-lookup"><span data-stu-id="2fa85-114">A URL that's &quot;friendly&quot; can make it easier for people to use the site.</span></span> <span data-ttu-id="2fa85-115">Sie können auch mit Search Engine Optimization (SEO) für den Standort.</span><span class="sxs-lookup"><span data-stu-id="2fa85-115">It can also help with search-engine optimization (SEO) for the site.</span></span> <span data-ttu-id="2fa85-116">ASP.NET-Websites bieten die Möglichkeit, automatisch friendly URLs verwendet.</span><span class="sxs-lookup"><span data-stu-id="2fa85-116">ASP.NET websites include the ability to use friendly URLs automatically.</span></span>

<span data-ttu-id="2fa85-117">ASP.NET können Sie aussagekräftige URLs erstellen, die Benutzeraktionen, sondern nur in eine Datei auf dem Server zu beschreiben.</span><span class="sxs-lookup"><span data-stu-id="2fa85-117">ASP.NET lets you create meaningful URLs that describe user actions instead of just pointing to a file on the server.</span></span> <span data-ttu-id="2fa85-118">Betrachten Sie diese URLs für eine fiktive Blog:</span><span class="sxs-lookup"><span data-stu-id="2fa85-118">Consider these URLs for a fictional blog:</span></span>

- `http://www.contoso.com/Blog/blog.cshtml?categories=hardware`
- `http://www.contoso.com//Blog/blog.cshtml?startdate=2009-11-01&enddate=2009-11-30`

<span data-ttu-id="2fa85-119">Vergleichen Sie diese URLs in den folgenden Vorgängen:</span><span class="sxs-lookup"><span data-stu-id="2fa85-119">Compare those URLs to the following ones:</span></span>

- `http://www.contoso.com/Blog/categories/hardware/`
- `http://www.contoso.com/Blog/2009/November`

<span data-ttu-id="2fa85-120">Ein Benutzer müsste in das erste Paar zu wissen, dass im Blog mit angezeigt wird der *blog.cshtml* Seite und dann müssten Sie eine Abfragezeichenfolge erstellt, die die richtige Kategorie oder den Datumsbereich abruft.</span><span class="sxs-lookup"><span data-stu-id="2fa85-120">In the first pair, a user would have to know that the blog is displayed using the *blog.cshtml* page, and would then have to construct a query string that gets the right category or date range.</span></span> <span data-ttu-id="2fa85-121">Der zweite Satz von Beispielen ist viel einfacher zu verstehen und zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="2fa85-121">The second set of examples is much easier to comprehend and create.</span></span>

<span data-ttu-id="2fa85-122">Zeigen Sie die URLs für das erste Beispiel auch direkt an eine bestimmte Datei (*blog.cshtml*).</span><span class="sxs-lookup"><span data-stu-id="2fa85-122">The URLs for the first example also point directly to a specific file (*blog.cshtml*).</span></span> <span data-ttu-id="2fa85-123">Wenn aus irgendeinem Grund im Blog in einen anderen Ordner verschoben wurden, auf dem Server oder der Blog so umgeschrieben wurden, dass eine andere Seite verwendet, würde die Links falsch sein.</span><span class="sxs-lookup"><span data-stu-id="2fa85-123">If for some reason the blog were moved to another folder on the server, or if the blog were rewritten to use a different page, the links would be wrong.</span></span> <span data-ttu-id="2fa85-124">Der zweite Satz von URLs zu einer bestimmten Seite zeigt nicht würden also auch, wenn die Blog-Implementierung oder den Speicherort geändert, die URLs weiterhin gültig sein.</span><span class="sxs-lookup"><span data-stu-id="2fa85-124">The second set of URLs doesn't point to a specific page, so even if the blog implementation or location changes, the URLs would still be valid.</span></span>

<span data-ttu-id="2fa85-125">In ASP.NET Web Pages können Sie benutzerfreundlicheren URLs wie in den obigen Beispielen erstellen, da ASP.NET verwendet *routing*.</span><span class="sxs-lookup"><span data-stu-id="2fa85-125">In ASP.NET Web Pages, you can create friendlier URLs like those in the above examples because ASP.NET uses *routing*.</span></span> <span data-ttu-id="2fa85-126">Routing erstellt logische Zuordnung von einer URL zu einem (Seiten), die die Anforderung erfüllt werden kann.</span><span class="sxs-lookup"><span data-stu-id="2fa85-126">Routing creates logical mapping from a URL to a page (or pages) that can fulfill the request.</span></span> <span data-ttu-id="2fa85-127">Da die Zuordnung der logischen ist (nicht physischen, um eine bestimmte Datei), das routing bietet große Flexibilität beim wie Sie die URLs für Ihre Website definieren.</span><span class="sxs-lookup"><span data-stu-id="2fa85-127">Because the mapping is logical (not physical, to a specific file), routing provides great flexibility in how you define the URLs for your site.</span></span>

## <a name="how-routing-works"></a><span data-ttu-id="2fa85-128">Funktionsweise von Routing</span><span class="sxs-lookup"><span data-stu-id="2fa85-128">How Routing Works</span></span>

<span data-ttu-id="2fa85-129">Wenn ASP.NET eine Anforderung verarbeitet, liest er die URL, um zu bestimmen, wie für die Weiterleitung an.</span><span class="sxs-lookup"><span data-stu-id="2fa85-129">When ASP.NET processes a request, it reads the URL to determine how to route it.</span></span> <span data-ttu-id="2fa85-130">ASP.NET versucht, die einzelne Segmente für die URL zu Dateien auf dem Datenträger für den Wechsel von links nach rechts übereinstimmen.</span><span class="sxs-lookup"><span data-stu-id="2fa85-130">ASP.NET tries to match individual segments of the URL to files on disk, going from left to right.</span></span> <span data-ttu-id="2fa85-131">Wenn eine Übereinstimmung vorhanden ist, wird nichts in der URL verbleibende übergeben, auf die Seite als *Pfadinformationen*.</span><span class="sxs-lookup"><span data-stu-id="2fa85-131">If there's a match, anything remaining in the URL is passed to the page as *path information*.</span></span>

<span data-ttu-id="2fa85-132">Stellen Sie sich vor, dass jemand eine Anforderung, die über diese URL sendet:</span><span class="sxs-lookup"><span data-stu-id="2fa85-132">Imagine that someone makes a request using this URL:</span></span>

`http://www.contoso.com/a/b/c`

<span data-ttu-id="2fa85-133">Die Suche ist wie folgt:</span><span class="sxs-lookup"><span data-stu-id="2fa85-133">The search goes like this:</span></span>

1. <span data-ttu-id="2fa85-134">Gibt es eine Datei mit dem Pfad und Name der */a/b/c.cshtml*?</span><span class="sxs-lookup"><span data-stu-id="2fa85-134">Is there a file with the path and name of */a/b/c.cshtml*?</span></span> <span data-ttu-id="2fa85-135">Wenn dies der Fall ist, führen Sie diese Seite, und weitergegeben Sie es werden keine Informationen an diesen.</span><span class="sxs-lookup"><span data-stu-id="2fa85-135">If so, run that page and pass no information to it.</span></span> <span data-ttu-id="2fa85-136">Andernfalls...</span><span class="sxs-lookup"><span data-stu-id="2fa85-136">Otherwise ...</span></span>
2. <span data-ttu-id="2fa85-137">Gibt es eine Datei mit dem Pfad und Name der */a/b.cshtml*?</span><span class="sxs-lookup"><span data-stu-id="2fa85-137">Is there a file with the path and name of */a/b.cshtml*?</span></span> <span data-ttu-id="2fa85-138">Wenn deshalb führen Sie diese Seite, und des Werts übergeben `c` darauf.</span><span class="sxs-lookup"><span data-stu-id="2fa85-138">If so, run that page and pass the value `c` to it.</span></span> <span data-ttu-id="2fa85-139">Andernfalls...</span><span class="sxs-lookup"><span data-stu-id="2fa85-139">Otherwise …</span></span>
3. <span data-ttu-id="2fa85-140">Gibt es eine Datei mit dem Pfad und Name der */a.cshtml*?</span><span class="sxs-lookup"><span data-stu-id="2fa85-140">Is there a file with the path and name of */a.cshtml*?</span></span> <span data-ttu-id="2fa85-141">Wenn deshalb führen Sie diese Seite, und des Werts übergeben `b/c` darauf.</span><span class="sxs-lookup"><span data-stu-id="2fa85-141">If so, run that page and pass the value `b/c` to it.</span></span>

<span data-ttu-id="2fa85-142">Wenn die Suche keine genaue gefunden für eine Übereinstimmung *cshtml* -Dateien im angegebenen Ordner ASP.NET weiterhin wiederum für diese Dateien zu suchen:</span><span class="sxs-lookup"><span data-stu-id="2fa85-142">If the search found no exact matches for *.cshtml* files in their specified folders, ASP.NET continues looking for these files in turn:</span></span>

1. <span data-ttu-id="2fa85-143">*/a/b/c/default.cshtml* (keine Pfadinformationen).</span><span class="sxs-lookup"><span data-stu-id="2fa85-143">*/a/b/c/default.cshtml* (no path information).</span></span>
2. <span data-ttu-id="2fa85-144">*/a/b/c/Index.cshtml* (keine Pfadinformationen).</span><span class="sxs-lookup"><span data-stu-id="2fa85-144">*/a/b/c/index.cshtml* (no path information).</span></span>

> [!NOTE]
> <span data-ttu-id="2fa85-145">Zu deaktivieren, werden Anforderungen für bestimmte Seiten (also Anforderungen, die implizit enthalten die *cshtml* Dateierweiterung) wie erwartet funktionieren.</span><span class="sxs-lookup"><span data-stu-id="2fa85-145">To be clear, requests for specific pages (that is, requests that include the *.cshtml* filename extension) work just like you'd expect.</span></span> <span data-ttu-id="2fa85-146">Eine Anforderung wie `http://www.contoso.com/a/b.cshtml` führen Sie die Seite *b.cshtml* einwandfrei.</span><span class="sxs-lookup"><span data-stu-id="2fa85-146">A request like `http://www.contoso.com/a/b.cshtml` will run the page *b.cshtml* just fine.</span></span>


<span data-ttu-id="2fa85-147">Innerhalb einer Seite erhalten Sie Informationen über der Seite für den Pfad `UrlData` Eigenschaft, die ein Wörterbuch darstellt.</span><span class="sxs-lookup"><span data-stu-id="2fa85-147">Inside a page, you can get the path information via the page's `UrlData` property, which is a dictionary.</span></span> <span data-ttu-id="2fa85-148">Angenommen, Sie eine Datei namens haben *ViewCustomers.cshtml* und Ihre Website ruft diese Anforderung:</span><span class="sxs-lookup"><span data-stu-id="2fa85-148">Imagine that you have a file named *ViewCustomers.cshtml* and your site gets this request:</span></span>

`http://mysite.com/myWebSite/ViewCustomers/1000`

<span data-ttu-id="2fa85-149">Wie im obigen Regeln beschrieben, wird die Anforderung zur Ihre Seite wechseln.</span><span class="sxs-lookup"><span data-stu-id="2fa85-149">As described in the rules above, the request will go to your page.</span></span> <span data-ttu-id="2fa85-150">Auf der Seite können Sie Code wie den folgenden abrufen und Anzeigen von Informationen für den Pfad (in diesem Fall werden die Werte &quot;1000&quot;):</span><span class="sxs-lookup"><span data-stu-id="2fa85-150">Inside the page, you can use code like the following to get and display the path information (in this case, the value &quot;1000&quot;):</span></span>

[!code-html[Main](creating-readable-urls-in-aspnet-web-pages-sites/samples/sample1.html)]

> [!NOTE]
> <span data-ttu-id="2fa85-151">Da routing vollständige Dateinamen nicht einschließen, treten möglicherweise Mehrdeutigkeiten bei Seiten, die den gleichen Namen aber unterschiedliche Dateinamenerweiterungen (z. B. *MyPage.cshtml* und *MyPage.html*) .</span><span class="sxs-lookup"><span data-stu-id="2fa85-151">Because routing doesn't involve complete file names, there can be ambiguity if you have pages that have the same name but different file-name extensions (for example, *MyPage.cshtml* and *MyPage.html*).</span></span> <span data-ttu-id="2fa85-152">Um Probleme mit routing zu vermeiden, empfiehlt es sich um sicherzustellen, dass Seiten auf Ihrer Website stehen Ihnen nicht nur in ihre Erweiterung, deren Namen zu unterscheiden.</span><span class="sxs-lookup"><span data-stu-id="2fa85-152">In order to avoid problems with routing, it's best to make sure that you don't have pages in your site whose names differ only in their extension.</span></span>


<a id="Additional_Resources"></a>
## <a name="additional-resources"></a><span data-ttu-id="2fa85-153">Zusätzliche Ressourcen</span><span class="sxs-lookup"><span data-stu-id="2fa85-153">Additional Resources</span></span>

<span data-ttu-id="2fa85-154">[WebMatrix - URLs, UrlData und Routing für SEO](http://www.mikesdotnetting.com/Article/165/WebMatrix-URLs-UrlData-and-Routing-for-SEO).</span><span class="sxs-lookup"><span data-stu-id="2fa85-154">[WebMatrix - URLs, UrlData and Routing for SEO](http://www.mikesdotnetting.com/Article/165/WebMatrix-URLs-UrlData-and-Routing-for-SEO).</span></span> <span data-ttu-id="2fa85-155">Dieser Blog von Mike Brind enthält einige weitere Details für das routing funktioniert in ASP.NET Web Pages.</span><span class="sxs-lookup"><span data-stu-id="2fa85-155">This blog entry by Mike Brind provides some additional details on how routing works in ASP.NET Web Pages.</span></span>
