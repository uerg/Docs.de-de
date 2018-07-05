---
uid: web-pages/overview/performance-and-traffic/14-analyzing-traffic
title: Die Überwachungsinformationen Besucher (Analytics) für eine ASP.NET-Webseiten (Razor) Standort | Microsoft-Dokumentation
author: tfitzmac
description: Nachdem Sie Ihre Website jetzt gelangt sind, empfiehlt es sich um Ihre Website-Datenverkehr zu analysieren.
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/17/2014
ms.topic: article
ms.assetid: 360bc6e1-84c5-4b8e-a84c-ea48ab807aa4
ms.technology: dotnet-webpages
msc.legacyurl: /web-pages/overview/performance-and-traffic/14-analyzing-traffic
msc.type: authoredcontent
ms.openlocfilehash: 48782606083b4aa1e32adf6163bcb3f2d9828bc3
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/03/2018
ms.locfileid: "37387136"
---
<a name="tracking-visitor-information-analytics-for-an-aspnet-web-pages-razor-site"></a><span data-ttu-id="7f136-103">Überwachungsinformationen Besucher (Analytics) für eine ASP.NET Web Pages (Razor)</span><span class="sxs-lookup"><span data-stu-id="7f136-103">Tracking Visitor Information (Analytics) for an ASP.NET Web Pages (Razor) Site</span></span>
====================
<span data-ttu-id="7f136-104">durch [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="7f136-104">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="7f136-105">Dieser Artikel beschreibt, wie Sie eine Hilfsprogramm, um Seiten auf einer Website für ASP.NET Web Pages (Razor) websiteanalyse hinzuzufügen.</span><span class="sxs-lookup"><span data-stu-id="7f136-105">This article describes how to use a helper to add website analytics to pages in an ASP.NET Web Pages (Razor) website.</span></span>
> 
> <span data-ttu-id="7f136-106">Sie lernen Folgendes:</span><span class="sxs-lookup"><span data-stu-id="7f136-106">What you'll learn:</span></span>
> 
> - <span data-ttu-id="7f136-107">Wie Sie Informationen zu Ihrer Website-Datenverkehr an einen analyseanbieter zu senden.</span><span class="sxs-lookup"><span data-stu-id="7f136-107">How to send information about your website traffic to an analytics provider.</span></span>
> 
> <span data-ttu-id="7f136-108">Hierbei handelt es sich um das ASP.NET-PROGRAMMIERMODELL in diesem Artikel vorgestellten Funktionen dar:</span><span class="sxs-lookup"><span data-stu-id="7f136-108">These are the ASP.NET programming features introduced in the article:</span></span>
> 
> - <span data-ttu-id="7f136-109">Die `Analytics` Helper.</span><span class="sxs-lookup"><span data-stu-id="7f136-109">The `Analytics` helper.</span></span>
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="7f136-110">Softwareversionen, die in diesem Tutorial verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="7f136-110">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="7f136-111">ASP.NET Web Pages (Razor) 2</span><span class="sxs-lookup"><span data-stu-id="7f136-111">ASP.NET Web Pages (Razor) 2</span></span>
> - <span data-ttu-id="7f136-112">ASP.NET Web Helpers Library (NuGet-Paket)</span><span class="sxs-lookup"><span data-stu-id="7f136-112">ASP.NET Web Helpers Library (NuGet package)</span></span>


<span data-ttu-id="7f136-113">Analytics ist ein allgemeiner Begriff für die Technologie, die Datenverkehr auf Ihrer Website misst, damit Sie nachvollziehen können, wie Benutzer die Site verwenden.</span><span class="sxs-lookup"><span data-stu-id="7f136-113">Analytics is a general term for technology that measures traffic on your website so you can understand how people use the site.</span></span> <span data-ttu-id="7f136-114">Viele Analytics-Dienste sind verfügbar, einschließlich Google, Yahoo, StatCounter und andere Dienste.</span><span class="sxs-lookup"><span data-stu-id="7f136-114">Many analytics services are available, including services from Google, Yahoo, StatCounter, and others.</span></span>

<span data-ttu-id="7f136-115">Die Möglichkeit Analyse funktioniert liegt darin, dass Sie sich für ein Konto mit dem Analytics-Anbieter registrieren, in dem Sie die Website registrieren, die Sie möchten nachverfolgen. Der Anbieter sendet Ihnen einen Ausschnitt des JavaScript-Code, der eine ID oder das Verfolgen von Code für Ihr Konto enthält.</span><span class="sxs-lookup"><span data-stu-id="7f136-115">The way analytics works is that you sign up for an account with the analytics provider, where you register the site that you want to track. The provider sends you a snippet of JavaScript code that includes an ID or tracking code for your account.</span></span> <span data-ttu-id="7f136-116">Die Webseiten auf der Website, die Sie nachverfolgen möchten, hinzugefügt den JavaScript-Codeausschnitt. (Hinzugefügt in der Regel den Analytics-Codeausschnitt eine Fußzeile oder das Layout der Seite oder andere HTML-Markup, das auf jeder Seite auf Ihrer Website angezeigt wird.) Wenn Benutzer eine Seite, die eine der folgenden JavaScript-Codeausschnitt enthält anfordern, sendet der Codeausschnitt Informationen über die aktuelle Seite auf den Analytics-Anbieter, der verschiedenen Details über die Seite zeichnet.</span><span class="sxs-lookup"><span data-stu-id="7f136-116">You add the JavaScript snippet to the web pages on the site that you want to track. (You typically add the analytics snippet to a footer or layout page or other HTML markup that appears on every page in your site.) When users request a page that contains one of these JavaScript snippets, the snippet sends information about the current page to the analytics provider, who records various details about the page.</span></span>

<span data-ttu-id="7f136-117">Wenn Sie einen Blick auf Ihre Website-Statistiken möchten, melden Sie sich in der Analyse Website des Anbieters.</span><span class="sxs-lookup"><span data-stu-id="7f136-117">When you want to have a look at your site statistics, you log into the analytics provider's website.</span></span> <span data-ttu-id="7f136-118">Sie können dann wie alle Arten von Berichten zu Ihrer Website anzeigen:</span><span class="sxs-lookup"><span data-stu-id="7f136-118">You can then view all sorts of reports about your site, like:</span></span>

- <span data-ttu-id="7f136-119">Die Anzahl der Seitenaufrufe für einzelne Seiten.</span><span class="sxs-lookup"><span data-stu-id="7f136-119">The number of page views for individual pages.</span></span> <span data-ttu-id="7f136-120">Dadurch sehen Sie (ungefähr) wie viele Personen die Website besuchen und welche Seiten auf Ihrer Website am häufigsten genutzt werden.</span><span class="sxs-lookup"><span data-stu-id="7f136-120">This tells you (roughly) how many people are visiting the site, and which pages on your site are the most popular.</span></span>
- <span data-ttu-id="7f136-121">Wie viel Zeit verbringen Benutzer auf bestimmte Seiten ein.</span><span class="sxs-lookup"><span data-stu-id="7f136-121">How long people spend on specific pages.</span></span> <span data-ttu-id="7f136-122">Dies kann Ihnen sagen z.B. gibt an, ob Ihre Startseite geweckt Interesse beibehalten wird.</span><span class="sxs-lookup"><span data-stu-id="7f136-122">This can tell you things like whether your home page is keeping people's interest.</span></span>
- <span data-ttu-id="7f136-123">Welche Websites Benutzer waren auf, bevor sie Ihre Website besucht.</span><span class="sxs-lookup"><span data-stu-id="7f136-123">What sites people were on before they visited your site.</span></span> <span data-ttu-id="7f136-124">Dadurch können Sie ermitteln, ob Links von Suchvorgängen und So weiter Ihren Datenverkehr stammt.</span><span class="sxs-lookup"><span data-stu-id="7f136-124">This helps you understand whether your traffic is coming from links, from searches, and so on.</span></span>
- <span data-ttu-id="7f136-125">Wenn Benutzer Ihre Site besuchen, und wie lange sie bleiben.</span><span class="sxs-lookup"><span data-stu-id="7f136-125">When people visit your site and how long they stay.</span></span>
- <span data-ttu-id="7f136-126">Welchen Ländern stammen die Besucher Ihrer aus.</span><span class="sxs-lookup"><span data-stu-id="7f136-126">What countries your visitors are from.</span></span>
- <span data-ttu-id="7f136-127">Welche Browser und Betriebssysteme die Besucher Ihrer verwenden.</span><span class="sxs-lookup"><span data-stu-id="7f136-127">What browsers and operating systems your visitors are using.</span></span>

    ![Ch14traffic-1](14-analyzing-traffic/_static/image1.jpg)

## <a name="using-a-helper-to-add-analytics-to-a-page"></a><span data-ttu-id="7f136-129">Verwenden eines Hilfsprogramms Analytics zu einer Seite hinzufügen</span><span class="sxs-lookup"><span data-stu-id="7f136-129">Using a Helper to Add Analytics to a Page</span></span>

<span data-ttu-id="7f136-130">ASP.NET Web Pages enthält mehrere Analytics-Hilfsprogramme (`Analytics.GetGoogleHtml`, `Analytics.GetYahooHtml`, und `Analytics.GetStatCounterHtml`), mit denen die JavaScript-Codeausschnitte, die verwendet werden, für die Analyse zu verwalten.</span><span class="sxs-lookup"><span data-stu-id="7f136-130">ASP.NET Web Pages includes several analytics helpers (`Analytics.GetGoogleHtml`, `Analytics.GetYahooHtml`, and `Analytics.GetStatCounterHtml`) that make it easy to manage the JavaScript snippets used for analytics.</span></span> <span data-ttu-id="7f136-131">Statt herauszufinden, wie und wo platzieren Sie den JavaScript-Code müssen Sie lediglich das Hilfsobjekt zu einer Seite hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="7f136-131">Instead of figuring out how and where to put the JavaScript code, all you have to do is add the helper to a page.</span></span> <span data-ttu-id="7f136-132">Die einzige Informationen, die Sie bereitstellen müssen ist Ihr Kontoname, ID oder Nachverfolgungscode.</span><span class="sxs-lookup"><span data-stu-id="7f136-132">The only information you need to provide is your account name, ID, or tracking code.</span></span> <span data-ttu-id="7f136-133">(Für StatCounter müssen Sie auch einige zusätzliche Werte angeben.)</span><span class="sxs-lookup"><span data-stu-id="7f136-133">(For StatCounter, you also have to provide a few additional values.)</span></span>

<span data-ttu-id="7f136-134">In diesem Verfahren erstellen Sie eine Layoutseite, verwendet der `GetGoogleHtml` Helper.</span><span class="sxs-lookup"><span data-stu-id="7f136-134">In this procedure, you'll create a layout page that uses the `GetGoogleHtml` helper.</span></span> <span data-ttu-id="7f136-135">Wenn Sie bereits über ein Konto mit einem der anderen analyseanbieter verfügen, können Sie verwenden Sie stattdessen dieses Konto und geringfügige Anpassungen vornehmen, je nach Bedarf.</span><span class="sxs-lookup"><span data-stu-id="7f136-135">If you already have an account with one of the other analytics providers, you can use that account instead and make slight adjustments as needed.</span></span>

> [!NOTE]
> <span data-ttu-id="7f136-136">Wenn Sie ein Analytics-Konto erstellen, registrieren Sie die URL der Website, die nachverfolgt werden sollen.</span><span class="sxs-lookup"><span data-stu-id="7f136-136">When you create an analytics account, you register the URL of the site that you want to be tracking.</span></span> <span data-ttu-id="7f136-137">Wenn Sie alles, was auf dem lokalen Computer testen, Sie wird nicht nachverfolgt werden tatsächliche Datenverkehr (nur der Datenverkehr ist Sie), damit Sie nicht aufzeichnen und Anzeigen von Websitestatistiken können.</span><span class="sxs-lookup"><span data-stu-id="7f136-137">If you're testing everything on your local computer, you won't be tracking actual traffic (the only traffic is you), so you won't be able to record and view site statistics.</span></span> <span data-ttu-id="7f136-138">Aber dieses Verfahren zeigt, wie Sie eine Analytics-Hilfe zu einer Seite hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="7f136-138">But this procedure shows how you add an analytics helper to a page.</span></span> <span data-ttu-id="7f136-139">Wenn Sie Ihre Website veröffentlichen, sendet die live-Website Informationen an Ihren analyseanbieter für die.</span><span class="sxs-lookup"><span data-stu-id="7f136-139">When you publish your site, the live site will send information to your analytics provider.</span></span>


1. <span data-ttu-id="7f136-140">Die ASP.NET Web Helpers Library zu Ihrer Website hinzufügen, wie in beschrieben [Hilfsprogramme installieren, in einer ASP.NET Web Pages-Website](https://go.microsoft.com/fwlink/?LinkId=252372), falls Sie bereits hinzugefügt haben.</span><span class="sxs-lookup"><span data-stu-id="7f136-140">Add the ASP.NET Web Helpers Library to your website as described in [Installing Helpers in an ASP.NET Web Pages Site](https://go.microsoft.com/fwlink/?LinkId=252372), if you haven't already added it.</span></span>
2. <span data-ttu-id="7f136-141">Erstellen Sie ein Konto mit Google Analytics, und notieren Sie den Kontonamen.</span><span class="sxs-lookup"><span data-stu-id="7f136-141">Create an account with Google Analytics and record the account name.</span></span>
3. <span data-ttu-id="7f136-142">Erstellen Sie eine Layoutseite, die mit dem Namen *Analytics.cshtml* und fügen Sie das folgende Markup hinzu:</span><span class="sxs-lookup"><span data-stu-id="7f136-142">Create a layout page named *Analytics.cshtml* and add the following markup:</span></span>

    [!code-cshtml[Main](14-analyzing-traffic/samples/sample1.cshtml)]

    > [!NOTE]
    > <span data-ttu-id="7f136-143">Müssen Sie den Aufruf zum Platzieren der `Analytics` Hilfsprogramm im Text einer Webseite (vor der `</body>` Tag).</span><span class="sxs-lookup"><span data-stu-id="7f136-143">You must place the call to the `Analytics` helper in the body of your web page (before the `</body>` tag).</span></span> <span data-ttu-id="7f136-144">Andernfalls wird der Browser nicht das Skript ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="7f136-144">Otherwise, the browser will not run the script.</span></span>

    <span data-ttu-id="7f136-145">Wenn Sie einen anderen Analytics-Anbieter verwenden, verwenden Sie eine der folgenden Helfer stattdessen:</span><span class="sxs-lookup"><span data-stu-id="7f136-145">If you're using a different analytics provider, use one of the following helpers instead:</span></span>

    - <span data-ttu-id="7f136-146">(Yahoo) `@Analytics.GetYahooHtml("myaccount")`</span><span class="sxs-lookup"><span data-stu-id="7f136-146">(Yahoo) `@Analytics.GetYahooHtml("myaccount")`</span></span>
    - <span data-ttu-id="7f136-147">(StatCounter) `@Analytics.GetStatCounterHtml("project", "security")`</span><span class="sxs-lookup"><span data-stu-id="7f136-147">(StatCounter) `@Analytics.GetStatCounterHtml("project", "security")`</span></span>
4. <span data-ttu-id="7f136-148">Ersetzen Sie dies `myaccount` mit dem Namen des Kontos, ID oder Nachverfolgungscode, die Sie in Schritt 1 erstellt haben.</span><span class="sxs-lookup"><span data-stu-id="7f136-148">Replace `myaccount` with the name of the account, ID, or tracking code that you created in step 1.</span></span>
5. <span data-ttu-id="7f136-149">Führen Sie die Seite im Browser.</span><span class="sxs-lookup"><span data-stu-id="7f136-149">Run the page in the browser.</span></span> <span data-ttu-id="7f136-150">(Stellen Sie sicher, dass die Seite ist ausgewählt, der **Dateien** Arbeitsbereich vor der Ausführung.)</span><span class="sxs-lookup"><span data-stu-id="7f136-150">(Make sure the page is selected in the **Files** workspace before you run it.)</span></span>
6. <span data-ttu-id="7f136-151">Zeigen Sie den Quellcode der Seite im Browser.</span><span class="sxs-lookup"><span data-stu-id="7f136-151">In the browser, view the page source.</span></span> <span data-ttu-id="7f136-152">Sie werden den gerenderten Analytics-Code zu sehen:</span><span class="sxs-lookup"><span data-stu-id="7f136-152">You'll be able to see the rendered analytics code:</span></span>

    [!code-html[Main](14-analyzing-traffic/samples/sample2.html)]
7. <span data-ttu-id="7f136-153">Melden Sie sich auf der Website von Google Analytics, und überprüfen Sie die Statistiken für Ihre Website.</span><span class="sxs-lookup"><span data-stu-id="7f136-153">Log onto the Google Analytics site and examine the statistics for your site.</span></span> <span data-ttu-id="7f136-154">Wenn Sie die Seite auf einer live-Website ausführen, sehen Sie einen Eintrag, der den Besuch auf Ihrer Seite protokolliert.</span><span class="sxs-lookup"><span data-stu-id="7f136-154">If you're running the page on a live site, you see an entry that logs the visit to your page.</span></span>

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a><span data-ttu-id="7f136-155">Zusätzliche Ressourcen</span><span class="sxs-lookup"><span data-stu-id="7f136-155">Additional Resources</span></span>

- [<span data-ttu-id="7f136-156">Google Analytics-Website</span><span class="sxs-lookup"><span data-stu-id="7f136-156">Google Analytics site</span></span>](https://www.google.com/analytics/)
- [<span data-ttu-id="7f136-157">Yahoo! Web Analytics-Website</span><span class="sxs-lookup"><span data-stu-id="7f136-157">Yahoo! Web Analytics site</span></span>](http://help.yahoo.com/l/us/yahoo/ywa/)
- [<span data-ttu-id="7f136-158">StatCounter-Website</span><span class="sxs-lookup"><span data-stu-id="7f136-158">StatCounter site</span></span>](http://statcounter.com/)
