---
uid: mvc/overview/older-versions-1/controllers-and-routing/adding-dynamic-content-to-a-cached-page-cs
title: Hinzufügen von dynamischen Inhalt zu einer zwischengespeicherten Seite (c#) | Microsoft Docs
author: microsoft
description: Informationen Sie zum dynamischen und zwischengespeicherte Inhalte auf der gleichen Seite mischen. Nach der Zwischenspeicher können Sie dynamische Inhalte wie Banner Ankündigungen o anzeigen...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/27/2009
ms.topic: article
ms.assetid: 2ddd4407-d143-4a94-877c-21771bfb97a6
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/adding-dynamic-content-to-a-cached-page-cs
msc.type: authoredcontent
ms.openlocfilehash: 9f91cc07bc531cfb3edf577ab79e91fd94a57a3c
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
ms.locfileid: "30868580"
---
<a name="adding-dynamic-content-to-a-cached-page-c"></a><span data-ttu-id="1d249-104">Hinzufügen von dynamischen Inhalt zu einer zwischengespeicherten Seite (c#)</span><span class="sxs-lookup"><span data-stu-id="1d249-104">Adding Dynamic Content to a Cached Page (C#)</span></span>
====================
<span data-ttu-id="1d249-105">durch [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="1d249-105">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="1d249-106">Informationen Sie zum dynamischen und zwischengespeicherte Inhalte auf der gleichen Seite mischen.</span><span class="sxs-lookup"><span data-stu-id="1d249-106">Learn how to mix dynamic and cached content in the same page.</span></span> <span data-ttu-id="1d249-107">Nach der Zwischenspeicher können Sie dynamische Inhalte, z. B. Banner Ankündigungen oder Nachrichtenelemente innerhalb einer Seite eine Ausgabe wurde hat zwischengespeichert.</span><span class="sxs-lookup"><span data-stu-id="1d249-107">Post-cache substitution enables you to display dynamic content, such as banner advertisements or news items, within a page that has been output cached.</span></span>


<span data-ttu-id="1d249-108">Durch Zwischenspeichern der Ausgabe nutzen, können Sie die Leistung einer ASP.NET MVC-Anwendung deutlich verbessern.</span><span class="sxs-lookup"><span data-stu-id="1d249-108">By taking advantage of output caching, you can dramatically improve the performance of an ASP.NET MVC application.</span></span> <span data-ttu-id="1d249-109">Anstelle von Neugenerieren eine Seite jedes Mal, wenn, das die Seite angefordert wird, kann die Seite einmal generiert und für mehrere Benutzer im Arbeitsspeicher zwischengespeichert werden.</span><span class="sxs-lookup"><span data-stu-id="1d249-109">Instead of regenerating a page each and every time the page is requested, the page can be generated once and cached in memory for multiple users.</span></span>

<span data-ttu-id="1d249-110">Es ist jedoch ein Problem.</span><span class="sxs-lookup"><span data-stu-id="1d249-110">But there is a problem.</span></span> <span data-ttu-id="1d249-111">Was geschieht, wenn müssen Sie dynamische Inhalte auf der Seite anzeigen?</span><span class="sxs-lookup"><span data-stu-id="1d249-111">What if you need to display dynamic content in the page?</span></span> <span data-ttu-id="1d249-112">Angenommen Sie, dass ein Werbebanner auf der Seite angezeigt werden soll.</span><span class="sxs-lookup"><span data-stu-id="1d249-112">For example, imagine that you want to display a banner advertisement in the page.</span></span> <span data-ttu-id="1d249-113">Sie möchten schließlich nicht die Werbebanner zwischengespeichert werden, sodass jeder Benutzer die selbe Ankündigung angezeigt wird.</span><span class="sxs-lookup"><span data-stu-id="1d249-113">You don't want the banner advertisement to be cached so that every user sees the very same advertisement.</span></span> <span data-ttu-id="1d249-114">Auf diese Weise würde nicht Sie Geld machen!</span><span class="sxs-lookup"><span data-stu-id="1d249-114">You wouldn't make any money that way!</span></span>

<span data-ttu-id="1d249-115">Glücklicherweise ist eine einfache Lösung.</span><span class="sxs-lookup"><span data-stu-id="1d249-115">Fortunately, there is an easy solution.</span></span> <span data-ttu-id="1d249-116">Profitieren Sie von einer Funktion von ASP.NET-Framework aufgerufen, *cache nach der Ersetzung*.</span><span class="sxs-lookup"><span data-stu-id="1d249-116">You can take advantage of a feature of the ASP.NET framework called *post-cache substitution*.</span></span> <span data-ttu-id="1d249-117">Nach der Zwischenspeicher ermöglicht Ihnen, dynamische Inhalte auf einer Seite ersetzen, die im Arbeitsspeicher zwischengespeichert wurde.</span><span class="sxs-lookup"><span data-stu-id="1d249-117">Post-cache substitution enables you to substitute dynamic content in a page that has been cached in memory.</span></span>


<span data-ttu-id="1d249-118">Wenn Cache eine Seite mit dem Attribut [OutputCache] ausgegeben wird, wird die Seite in der Regel wird auf dem Server und der Client (Webbrowser) zwischengespeichert.</span><span class="sxs-lookup"><span data-stu-id="1d249-118">Normally, when you output cache a page by using the [OutputCache] attribute, the page is cached on both the server and the client (the web browser).</span></span> <span data-ttu-id="1d249-119">Wenn Sie nach der Zwischenspeicher verwenden, wird eine Seite nur auf dem Server zwischengespeichert.</span><span class="sxs-lookup"><span data-stu-id="1d249-119">When you use post-cache substitution, a page is cached only on the server.</span></span>


#### <a name="using-post-cache-substitution"></a><span data-ttu-id="1d249-120">Verwenden nach der Zwischenspeicher</span><span class="sxs-lookup"><span data-stu-id="1d249-120">Using Post-Cache Substitution</span></span>

<span data-ttu-id="1d249-121">Verwenden nach der Zwischenspeicher sind zwei Schritte erforderlich.</span><span class="sxs-lookup"><span data-stu-id="1d249-121">Using post-cache substitution requires two steps.</span></span> <span data-ttu-id="1d249-122">Zunächst müssen Sie eine Methode definieren, die eine Zeichenfolge zurückgibt, die die dynamische Inhalte darstellt, die in der zwischengespeicherten Seite angezeigt werden soll.</span><span class="sxs-lookup"><span data-stu-id="1d249-122">First, you need to define a method that returns a string that represents the dynamic content that you want to display in the cached page.</span></span> <span data-ttu-id="1d249-123">Anschließend rufen Sie die HttpResponse.WriteSubstitution()-Methode, um den dynamischen Inhalt in die Seite einfügen.</span><span class="sxs-lookup"><span data-stu-id="1d249-123">Next, you call the HttpResponse.WriteSubstitution() method to inject the dynamic content into the page.</span></span>

<span data-ttu-id="1d249-124">Stellen Sie sich vor, z. B., dass nach dem Zufallsprinzip verschiedenen Nachrichtenelemente in einer zwischengespeicherten Seite angezeigt werden soll.</span><span class="sxs-lookup"><span data-stu-id="1d249-124">Imagine, for example, that you want to randomly display different news items in a cached page.</span></span> <span data-ttu-id="1d249-125">Die Klasse im Codebeispiel 1 stellt eine einzelne Methode, mit dem Namen RenderNews(), die nach dem Zufallsprinzip ein Newselement aus einer Liste von drei Nachrichtenelemente zurückgibt.</span><span class="sxs-lookup"><span data-stu-id="1d249-125">The class in Listing 1 exposes a single method, named RenderNews(), that randomly returns one news item from a list of three news items.</span></span>

<span data-ttu-id="1d249-126">**1 – Models\News.cs auflisten**</span><span class="sxs-lookup"><span data-stu-id="1d249-126">**Listing 1 – Models\News.cs**</span></span>

[!code-csharp[Main](adding-dynamic-content-to-a-cached-page-cs/samples/sample1.cs)]

<span data-ttu-id="1d249-127">Um nach der Zwischenspeicher nutzen zu können, rufen Sie die HttpResponse.WriteSubstitution()-Methode.</span><span class="sxs-lookup"><span data-stu-id="1d249-127">To take advantage of post-cache substitution, you call the HttpResponse.WriteSubstitution() method.</span></span> <span data-ttu-id="1d249-128">Die WriteSubstitution()-Methode richtet den Code, um einen Bereich der zwischengespeicherten Seite mit dynamischen Inhalten zu ersetzen.</span><span class="sxs-lookup"><span data-stu-id="1d249-128">The WriteSubstitution() method sets up the code to replace a region of the cached page with dynamic content.</span></span> <span data-ttu-id="1d249-129">Die WriteSubstitution()-Methode wird verwendet, zum Anzeigen des zufälligen News-Elements in der Ansicht 2 aufgelistet.</span><span class="sxs-lookup"><span data-stu-id="1d249-129">The WriteSubstitution() method is used to display the random news item in the view in Listing 2.</span></span>

<span data-ttu-id="1d249-130">**2 – Views\Home\Index.aspx auflisten**</span><span class="sxs-lookup"><span data-stu-id="1d249-130">**Listing 2 – Views\Home\Index.aspx**</span></span>

[!code-aspx[Main](adding-dynamic-content-to-a-cached-page-cs/samples/sample2.aspx)]

<span data-ttu-id="1d249-131">Die RenderNews-Methode wird an die WriteSubstitution()-Methode übergeben.</span><span class="sxs-lookup"><span data-stu-id="1d249-131">The RenderNews method is passed to the WriteSubstitution() method.</span></span> <span data-ttu-id="1d249-132">Beachten Sie, dass die RenderNews-Methode nicht aufgerufen wird (es gibt keine Klammern).</span><span class="sxs-lookup"><span data-stu-id="1d249-132">Notice that the RenderNews method is not called (there are no parentheses).</span></span> <span data-ttu-id="1d249-133">Stattdessen wird ein Verweis auf die Methode an WriteSubstitution() übergeben.</span><span class="sxs-lookup"><span data-stu-id="1d249-133">Instead a reference to the method is passed to WriteSubstitution().</span></span>

<span data-ttu-id="1d249-134">Die Indexansicht wird zwischengespeichert.</span><span class="sxs-lookup"><span data-stu-id="1d249-134">The Index view is cached.</span></span> <span data-ttu-id="1d249-135">Die Sicht wird von der Controller im Codebeispiel 3 zurückgegeben.</span><span class="sxs-lookup"><span data-stu-id="1d249-135">The view is returned by the controller in Listing 3.</span></span> <span data-ttu-id="1d249-136">Beachten Sie, dass die Index()-Aktion mit einem [OutputCache]-Attribut ergänzt wird, die bewirkt, dass die Indexansicht 60 Sekunden lang zwischengespeichert werden soll.</span><span class="sxs-lookup"><span data-stu-id="1d249-136">Notice that the Index() action is decorated with an [OutputCache] attribute that causes the Index view to be cached for 60 seconds.</span></span>

<span data-ttu-id="1d249-137">**3 – Controllers\HomeController. cs auflisten**</span><span class="sxs-lookup"><span data-stu-id="1d249-137">**Listing 3 – Controllers\HomeController.cs**</span></span>

[!code-csharp[Main](adding-dynamic-content-to-a-cached-page-cs/samples/sample3.cs)]

<span data-ttu-id="1d249-138">Obwohl die Indexansicht zwischengespeichert wird, werden verschiedene zufällige Newsgroupbeiträgen angezeigt, wenn Sie die Indexseite anfordern.</span><span class="sxs-lookup"><span data-stu-id="1d249-138">Even though the Index view is cached, different random news items are displayed when you request the Index page.</span></span> <span data-ttu-id="1d249-139">Wenn Sie die Indexseite anfordern, ändert die von der Seite angezeigte Uhrzeit nicht 60 Sekunden (siehe Abbildung 1).</span><span class="sxs-lookup"><span data-stu-id="1d249-139">When you request the Index page, the time displayed by the page does not change for 60 seconds (see Figure 1).</span></span> <span data-ttu-id="1d249-140">Die Tatsache, die die Zeit nicht ändert beweist, dass die Seite zwischengespeichert wird.</span><span class="sxs-lookup"><span data-stu-id="1d249-140">The fact that the time does not change proves that the page is cached.</span></span> <span data-ttu-id="1d249-141">Allerdings wird der Inhalt durch der WriteSubstitution() – das zufällige Newselement – ändert mit jeder Anforderung eingefügt.</span><span class="sxs-lookup"><span data-stu-id="1d249-141">However, the content injected by the WriteSubstitution() method – the random news item – changes with each request .</span></span>

<span data-ttu-id="1d249-142">**Abbildung 1 – dynamische Nachrichtenelemente in einer zwischengespeicherten Seite Räumen**</span><span class="sxs-lookup"><span data-stu-id="1d249-142">**Figure 1 – Injecting dynamic news items in a cached page**</span></span>

![clip_image002](adding-dynamic-content-to-a-cached-page-cs/_static/image1.jpg)

#### <a name="using-post-cache-substitution-in-helper-methods"></a><span data-ttu-id="1d249-144">Verwenden nach der Zwischenspeicher in Hilfsmethoden</span><span class="sxs-lookup"><span data-stu-id="1d249-144">Using Post-Cache Substitution in Helper Methods</span></span>

<span data-ttu-id="1d249-145">Eine einfachere Möglichkeit, nach der Zwischenspeicher nutzen ist, den Aufruf der Methode WriteSubstitution() innerhalb einer benutzerdefinierten Hilfsmethode zu kapseln.</span><span class="sxs-lookup"><span data-stu-id="1d249-145">An easier way to take advantage of post-cache substitution is to encapsulate the call to the WriteSubstitution() method within a custom helper method.</span></span> <span data-ttu-id="1d249-146">Dieser Ansatz wird durch die Hilfsmethode auflisten 4 veranschaulicht.</span><span class="sxs-lookup"><span data-stu-id="1d249-146">This approach is illustrated by the helper method in Listing 4.</span></span>

<span data-ttu-id="1d249-147">**4 – AdHelper.cs auflisten**</span><span class="sxs-lookup"><span data-stu-id="1d249-147">**Listing 4 – AdHelper.cs**</span></span>

[!code-csharp[Main](adding-dynamic-content-to-a-cached-page-cs/samples/sample4.cs)]

<span data-ttu-id="1d249-148">Auflisten von 4 enthält eine statische Klasse, die zwei Methoden verfügbar macht: RenderBanner() und RenderBannerInternal().</span><span class="sxs-lookup"><span data-stu-id="1d249-148">Listing 4 contains a static class that exposes two methods: RenderBanner() and RenderBannerInternal().</span></span> <span data-ttu-id="1d249-149">Die RenderBanner() Methode darstellt, die tatsächliche Hilfsmethode.</span><span class="sxs-lookup"><span data-stu-id="1d249-149">The RenderBanner() method represents the actual helper method.</span></span> <span data-ttu-id="1d249-150">Diese Methode erweitert die standardmäßige ASP.NET MVC HtmlHelper-Klasse, damit Sie Html.RenderBanner() in einer Ansicht genau wie alle anderen Hilfsmethode aufrufen können.</span><span class="sxs-lookup"><span data-stu-id="1d249-150">This method extends the standard ASP.NET MVC HtmlHelper class so that you can call Html.RenderBanner() in a view just like any other helper method.</span></span>

<span data-ttu-id="1d249-151">Die RenderBanner()-Methode ruft die HttpResponse.WriteSubstitution()-Methode, die die RenderBannerInternal()-Methode der WriteSubsitution()-Methode übergeben.</span><span class="sxs-lookup"><span data-stu-id="1d249-151">The RenderBanner() method calls the HttpResponse.WriteSubstitution() method passing the RenderBannerInternal() method to the WriteSubsitution() method.</span></span>

<span data-ttu-id="1d249-152">Die RenderBannerInternal()-Methode ist eine private Methode.</span><span class="sxs-lookup"><span data-stu-id="1d249-152">The RenderBannerInternal() method is a private method.</span></span> <span data-ttu-id="1d249-153">Diese Methode wird nicht als eine Hilfsmethode verfügbar gemacht werden.</span><span class="sxs-lookup"><span data-stu-id="1d249-153">This method won't be exposed as a helper method.</span></span> <span data-ttu-id="1d249-154">Die RenderBannerInternal()-Methode gibt ein Banner Ankündigung Bild nach dem Zufallsprinzip aus einer Liste mit drei Banner Ankündigung Bilder.</span><span class="sxs-lookup"><span data-stu-id="1d249-154">The RenderBannerInternal() method randomly returns one banner advertisement image from a list of three banner advertisement images.</span></span>

<span data-ttu-id="1d249-155">Die geänderte Indexansicht auflisten 5 wird veranschaulicht, wie Sie die RenderBanner()-Hilfsmethode.</span><span class="sxs-lookup"><span data-stu-id="1d249-155">The modified Index view in Listing 5 illustrates how you can use the RenderBanner() helper method.</span></span> <span data-ttu-id="1d249-156">Beachten Sie, dass ein zusätzliches &lt;% @ Import %&gt; Richtlinie finden Sie am oberen Rand der Ansicht, um den MvcApplication1.Helpers-Namespace zu importieren.</span><span class="sxs-lookup"><span data-stu-id="1d249-156">Notice that an additional &lt;%@ Import %&gt; directive is included at the top of the view to import the MvcApplication1.Helpers namespace.</span></span> <span data-ttu-id="1d249-157">Wenn Sie keine dieses Namespace zu importieren, wird nicht die RenderBanner() Methode als Methode für die Html-Eigenschaft angezeigt.</span><span class="sxs-lookup"><span data-stu-id="1d249-157">If you neglect to import this namespace, then the RenderBanner() method won't appear as a method on the Html property.</span></span>

<span data-ttu-id="1d249-158">**Auflisten von 5 – Views\Home\Index.aspx (mit RenderBanner()-Methode)**</span><span class="sxs-lookup"><span data-stu-id="1d249-158">**Listing 5 – Views\Home\Index.aspx (with RenderBanner() method)**</span></span>

[!code-aspx[Main](adding-dynamic-content-to-a-cached-page-cs/samples/sample5.aspx)]

<span data-ttu-id="1d249-159">Wenn Sie auf die Seite gerendert, die von der Sicht im Codebeispiel 5 anfordern, wird mit jeder Anforderung verschiedene Werbebanner angezeigt (siehe Abbildung 2).</span><span class="sxs-lookup"><span data-stu-id="1d249-159">When you request the page rendered by the view in Listing 5, a different banner advertisement is displayed with each request (see Figure 2).</span></span> <span data-ttu-id="1d249-160">Die Seite wird zwischengespeichert, aber die Banner Ankündigung wird dynamisch durch die Hilfsmethode RenderBanner() eingefügt.</span><span class="sxs-lookup"><span data-stu-id="1d249-160">The page is cached, but the banner advertisement is injected dynamically by the RenderBanner() helper method.</span></span>

<span data-ttu-id="1d249-161">**Abbildung 2 – die Indexansicht Werbebanner zufälliges anzeigen**</span><span class="sxs-lookup"><span data-stu-id="1d249-161">**Figure 2 – The Index view displaying a random banner advertisement**</span></span>

![clip_image004](adding-dynamic-content-to-a-cached-page-cs/_static/image2.jpg)

#### <a name="summary"></a><span data-ttu-id="1d249-163">Zusammenfassung</span><span class="sxs-lookup"><span data-stu-id="1d249-163">Summary</span></span>

<span data-ttu-id="1d249-164">In diesem Lernprogramm wird erläutert, wie Sie Inhalte in einer zwischengespeicherten Seite dynamisch aktualisieren können.</span><span class="sxs-lookup"><span data-stu-id="1d249-164">This tutorial explained how you can dynamically update content in a cached page.</span></span> <span data-ttu-id="1d249-165">Sie haben gelernt, wie Sie die HttpResponse.WriteSubstitution()-Methode verwenden, um in einer zwischengespeicherten Seite eingefügt werden dynamische Inhalte zu ermöglichen.</span><span class="sxs-lookup"><span data-stu-id="1d249-165">You learned how to use the HttpResponse.WriteSubstitution() method to enable dynamic content to be injected in a cached page.</span></span> <span data-ttu-id="1d249-166">Außerdem haben Sie gelernt, den Aufruf der Methode WriteSubstitution() innerhalb einer HTML-Hilfsmethode kapseln.</span><span class="sxs-lookup"><span data-stu-id="1d249-166">You also learned how to encapsulate the call to the WriteSubstitution() method within an HTML helper method.</span></span>

<span data-ttu-id="1d249-167">Nutzen Sie die Vorteile des Zwischenspeicherns, wann immer möglich – sie können eine drastische Auswirkungen auf die Leistung Ihrer Webanwendungen verfügen.</span><span class="sxs-lookup"><span data-stu-id="1d249-167">Take advantage of caching whenever possible – it can have a dramatic impact on the performance of your web applications.</span></span> <span data-ttu-id="1d249-168">Wie in diesem Lernprogramm erläutert wird, können Sie nutzen zwischenspeichern, selbst wenn dynamische Inhalte zu Ihren Seiten anzuzeigen müssen.</span><span class="sxs-lookup"><span data-stu-id="1d249-168">As explained in this tutorial, you can take advantage of caching even when you need to display dynamic content in your pages.</span></span>

## 

## 

> [!div class="step-by-step"]
> <span data-ttu-id="1d249-169">[Zurück](improving-performance-with-output-caching-cs.md)
> [Weiter](creating-a-controller-cs.md)</span><span class="sxs-lookup"><span data-stu-id="1d249-169">[Previous](improving-performance-with-output-caching-cs.md)
[Next](creating-a-controller-cs.md)</span></span>
