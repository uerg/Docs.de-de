---
uid: web-pages/overview/ui-layouts-and-themes/twitter-helper
title: Twitter-Hilfsprogramm mit ASP.NET Web Pages | Microsoft-Dokumentation
author: Rick-Anderson
description: Dieses Thema und die Anwendung zeigen, wie ein Twitter-Hilfsprogramm das WebMatrix 3-Projekt hinzu. Es enthält den Code für die Twitter-Hilfsprogramm und zeigt, wie Sie das Hilfsobjekt aufrufen...
ms.author: riande
ms.date: 11/26/2018
ms.assetid: c1a1244e-b9c8-42e6-a00b-8456a4ec027c
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/twitter-helper
msc.type: authoredcontent
ms.openlocfilehash: fabe9f2b84d278a766dc8d8b7bfc00e1eb967127
ms.sourcegitcommit: 710fc5fcac258cc8415976dc66bdb355b3e061d5
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/26/2018
ms.locfileid: "52299429"
---
<a name="twitter-helper-with-aspnet-web-pages"></a><span data-ttu-id="9bc44-104">Twitter-Hilfsprogramm mit ASP.NET Web Pages</span><span class="sxs-lookup"><span data-stu-id="9bc44-104">Twitter Helper with ASP.NET Web Pages</span></span>
====================
<span data-ttu-id="9bc44-105">durch [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="9bc44-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> [!IMPORTANT]
> <span data-ttu-id="9bc44-106">Twitter-Hilfsprogramme sind veraltet.</span><span class="sxs-lookup"><span data-stu-id="9bc44-106">Twitter Helpers are obsolete.</span></span> <span data-ttu-id="9bc44-107">Der Twitter neueste Engagement-Tools für Websites, finden Sie unter [Twitter-Übersicht über die Websites](https://developer.twitter.com/en/docs/twitter-for-websites/overview).</span><span class="sxs-lookup"><span data-stu-id="9bc44-107">For Twitter's latest engagement tools for websites, see [Twitter for Websites Overview](https://developer.twitter.com/en/docs/twitter-for-websites/overview).</span></span>

> <span data-ttu-id="9bc44-108">Dieses Thema und die Anwendung zeigen, wie ein Twitter-Hilfsprogramm das WebMatrix 3-Projekt hinzu.</span><span class="sxs-lookup"><span data-stu-id="9bc44-108">This topic and application show how to add a Twitter Helper to your WebMatrix 3 project.</span></span> <span data-ttu-id="9bc44-109">Es enthält den Code für die Twitter-Hilfsprogramm und zeigt, wie die Hilfsmethoden.</span><span class="sxs-lookup"><span data-stu-id="9bc44-109">It contains the Twitter Helper code and shows how to call the helper methods.</span></span>
> 
> <span data-ttu-id="9bc44-110">Dieser Code für die Datei Twitter.cshtml wurde entwickelt, indem **Tian Pan** von Microsoft.</span><span class="sxs-lookup"><span data-stu-id="9bc44-110">This code for the Twitter.cshtml file was developed by **Tian Pan** of Microsoft.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="9bc44-111">Softwareversionen, die in diesem Tutorial verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="9bc44-111">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="9bc44-112">ASP.NET Web Pages (Razor) 3</span><span class="sxs-lookup"><span data-stu-id="9bc44-112">ASP.NET Web Pages (Razor) 3</span></span>
>   
> 
> <span data-ttu-id="9bc44-113">In diesem Tutorial funktioniert auch mit ASP.NET Web Pages 2.</span><span class="sxs-lookup"><span data-stu-id="9bc44-113">This tutorial also works with ASP.NET Web Pages 2.</span></span>


## <a name="introduction"></a><span data-ttu-id="9bc44-114">Einführung</span><span class="sxs-lookup"><span data-stu-id="9bc44-114">Introduction</span></span>

<span data-ttu-id="9bc44-115">Dieses Thema veranschaulicht das Hinzufügen einer Twitter-Hilfsprogramm zu Ihrer Anwendung und Razor-Syntax zum Aufrufen von der Hilfsmethoden verwenden.</span><span class="sxs-lookup"><span data-stu-id="9bc44-115">This topic demonstrates how to add a Twitter Helper to your application and use Razor syntax to call the helper methods.</span></span> <span data-ttu-id="9bc44-116">Die Twitter-Hilfsprogramm vereinfacht das Twitter-Schaltflächen und Widgets in Ihrer Anwendung zu integrieren.</span><span class="sxs-lookup"><span data-stu-id="9bc44-116">The Twitter Helper makes it easy to incorporate Twitter buttons and widgets in your application.</span></span> <span data-ttu-id="9bc44-117">Verwenden ein Twitter-Widgets, wie z. B. die Timeline eines Benutzers oder der Ergebnisse der Suche nach einem Hashtag, erstellen Sie zuerst die [Widget auf Twitter](https://twitter.com/settings/widgets).</span><span class="sxs-lookup"><span data-stu-id="9bc44-117">To use a Twitter widget, such as a user's timeline or the search results for a hashtag, you must first create the [widget on Twitter](https://twitter.com/settings/widgets).</span></span> <span data-ttu-id="9bc44-118">Nach dem Erstellen Ihres Widgets, erhalten Sie eine Widget-Id. Sie übergeben dieses Widget-Id als Parameter beim Aufrufen von Methoden des Hilfsprogramms, die Widget anzeigen.</span><span class="sxs-lookup"><span data-stu-id="9bc44-118">After creating your widget, you will receive a widget id. You pass this widget id as a parameter when calling the helper methods that show widget.</span></span>

<span data-ttu-id="9bc44-119">In diesem Thema wurde für Version 1.1 der Twitter-API geschrieben.</span><span class="sxs-lookup"><span data-stu-id="9bc44-119">This topic was written for version 1.1 of the Twitter API.</span></span> <span data-ttu-id="9bc44-120">Durch das direkte Hinzufügen des Twitter-Hilfsprogramm-Codes zu Ihrem Projekt können Sie des hilfscodes aktualisieren, wenn der Twitter-API ändern.</span><span class="sxs-lookup"><span data-stu-id="9bc44-120">By directly adding the Twitter Helper code to your project, you can update the helper code if the Twitter API changes.</span></span>

<span data-ttu-id="9bc44-121">Weitere Informationen zum Installieren von WebMatrix, finden Sie unter [Einführung in ASP.NET Web Pages 2 - erste Schritte](../getting-started/introducing-aspnet-web-pages-2/getting-started.md).</span><span class="sxs-lookup"><span data-stu-id="9bc44-121">For information about installing WebMatrix, see [Introducing ASP.NET Web Pages 2 - Getting Started](../getting-started/introducing-aspnet-web-pages-2/getting-started.md).</span></span>

## <a name="add-twitter-helper-to-your-project"></a><span data-ttu-id="9bc44-122">Hinzufügen von Twitter-Hilfsprogramm zu Ihrem Projekt</span><span class="sxs-lookup"><span data-stu-id="9bc44-122">Add Twitter Helper to your project</span></span>

<span data-ttu-id="9bc44-123">Um die Twitter-Hilfsprogramm hinzuzufügen, fügen Sie zunächst einen Ordner namens **App\_Code** zu Ihrem Projekt.</span><span class="sxs-lookup"><span data-stu-id="9bc44-123">To add the Twitter Helper, first, add a folder named **App\_Code** to your project.</span></span> <span data-ttu-id="9bc44-124">Erstellen Sie dann eine Datei namens **Twitter.cshtml**.</span><span class="sxs-lookup"><span data-stu-id="9bc44-124">Then, create a file named **Twitter.cshtml**.</span></span>

![Ordner "App_Code"](twitter-helper/_static/image1.png)

<span data-ttu-id="9bc44-126">Ersetzen Sie den Standardcode in Twitter.cshtml durch den folgenden Code ein.</span><span class="sxs-lookup"><span data-stu-id="9bc44-126">Replace the default code in Twitter.cshtml with the following code.</span></span>

[!code-cshtml[Main](twitter-helper/samples/sample1.cshtml)]

## <a name="call-twitter-methods-from-your-web-pages"></a><span data-ttu-id="9bc44-127">Rufen Sie Ihren Webseiten Twitter-Methoden</span><span class="sxs-lookup"><span data-stu-id="9bc44-127">Call Twitter methods from your web pages</span></span>

<span data-ttu-id="9bc44-128">Das folgende Beispiel zeigt, wie Sie die Twitter-Hilfsprogramm-Methoden von einer Seite in Ihrem Projekt zu verwenden.</span><span class="sxs-lookup"><span data-stu-id="9bc44-128">The following example shows how to use the Twitter Helper methods from a page in your project.</span></span> <span data-ttu-id="9bc44-129">In Ihrem Projekt möchten Sie die Parameterwerte durch Werte ersetzen, die an Ihre Anforderungen relevant sind.</span><span class="sxs-lookup"><span data-stu-id="9bc44-129">In your project, you will want to replace the parameter values with values that are relevant to your needs.</span></span> <span data-ttu-id="9bc44-130">Können Sie die bereitgestellten Widget-Ids um zu untersuchen, wie die Methoden funktionieren, aber Sie möchten eigene Widgets für Ihr Projekt zu generieren.</span><span class="sxs-lookup"><span data-stu-id="9bc44-130">You can use the provided widget ids to explore how the methods work, but you will want to generate your own widgets for your project.</span></span>

<span data-ttu-id="9bc44-131">Nicht alle der unten angezeigte Parameter sind erforderlich.</span><span class="sxs-lookup"><span data-stu-id="9bc44-131">Not all of the parameters shown below are required.</span></span> <span data-ttu-id="9bc44-132">Die optionalen Parameter werden verwendet, um anzupassen, wie die Schaltfläche oder das Widget angezeigt wird.</span><span class="sxs-lookup"><span data-stu-id="9bc44-132">The optional parameters are used to customize how the button or widget is displayed.</span></span> <span data-ttu-id="9bc44-133">Z. B. die Schaltfläche "folgen" erfordert den Benutzernamen ein, führen, aber das Beispiel zeigt, wie die Anzahl der Follower, und wie die Größe der Schaltfläche und die Sprache angeben.</span><span class="sxs-lookup"><span data-stu-id="9bc44-133">For example, the Follow Button only requires the user name to follow, but the example shows how to include the number of followers, and how specify the size of the button and the language.</span></span>

[!code-html[Main](twitter-helper/samples/sample2.html)]

## <a name="see-the-results"></a><span data-ttu-id="9bc44-134">Die Ergebnisse angezeigt</span><span class="sxs-lookup"><span data-stu-id="9bc44-134">See the results</span></span>

<span data-ttu-id="9bc44-135">Der obige Code generiert die folgenden Schaltflächen und Widgets.</span><span class="sxs-lookup"><span data-stu-id="9bc44-135">The above code produces the following buttons and widgets.</span></span> <span data-ttu-id="9bc44-136">Diese Schaltflächen und die Widgets sind voll funktionsfähige, nicht-Screenshots.</span><span class="sxs-lookup"><span data-stu-id="9bc44-136">These buttons and widgets are fully-functional, not screenshots.</span></span> <span data-ttu-id="9bc44-137">Die Schaltfläche "folgen" wird in Spanisch angezeigt, da es sich bei der Language-Parameter festgelegt wurde, um **es**.</span><span class="sxs-lookup"><span data-stu-id="9bc44-137">The Follow Button is displayed in Spanish because the language parameter was set to **es**.</span></span>

### <a name="follow-button"></a><span data-ttu-id="9bc44-138">Führen Sie die Schaltfläche "</span><span class="sxs-lookup"><span data-stu-id="9bc44-138">Follow Button</span></span>

<span data-ttu-id="9bc44-139">[Führen Sie @aspnet)](https://twitter.com/aspnet)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + '://platform.twitter.com/widgets.js'; fjs.parentNode.insertBefore(js, fjs); } }(document, 'script', 'twitter-wjs');</script>`</span><span class="sxs-lookup"><span data-stu-id="9bc44-139">[Follow @aspnet)](https://twitter.com/aspnet)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + '://platform.twitter.com/widgets.js'; fjs.parentNode.insertBefore(js, fjs); } }(document, 'script', 'twitter-wjs');</script>`</span></span>

### <a name="tweet-button"></a><span data-ttu-id="9bc44-140">Schaltfläche "Tweet"</span><span class="sxs-lookup"><span data-stu-id="9bc44-140">Tweet Button</span></span>

<span data-ttu-id="9bc44-141">[Tweet](https://twitter.com/share)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + '://platform.twitter.com/widgets.js'; fjs.parentNode.insertBefore(js, fjs); } }(document, 'script', 'twitter-wjs');</script>`</span><span class="sxs-lookup"><span data-stu-id="9bc44-141">[Tweet](https://twitter.com/share)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + '://platform.twitter.com/widgets.js'; fjs.parentNode.insertBefore(js, fjs); } }(document, 'script', 'twitter-wjs');</script>`</span></span>

### <a name="user-timeline-profile"></a><span data-ttu-id="9bc44-142">Timeline des Benutzers (Profil)</span><span class="sxs-lookup"><span data-stu-id="9bc44-142">User Timeline (Profile)</span></span>

<span data-ttu-id="9bc44-143">[TWEETS nach @aspnet](https://twitter.com/aspnet)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + "://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore(js, fjs); } }(document, "script", "twitter-wjs");</script>`</span><span class="sxs-lookup"><span data-stu-id="9bc44-143">[Tweets by @aspnet](https://twitter.com/aspnet)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + "://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore(js, fjs); } }(document, "script", "twitter-wjs");</script>`</span></span>

### <a name="favorites"></a><span data-ttu-id="9bc44-144">Favoriten</span><span class="sxs-lookup"><span data-stu-id="9bc44-144">Favorites</span></span>

<span data-ttu-id="9bc44-145">[Bevorzugte Tweets nach @Microsoft](https://twitter.com/Microsoft/favorites)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + "://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore(js, fjs); } }(document, "script", "twitter-wjs");</script>`</span><span class="sxs-lookup"><span data-stu-id="9bc44-145">[Favorite Tweets by @Microsoft](https://twitter.com/Microsoft/favorites)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + "://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore(js, fjs); } }(document, "script", "twitter-wjs");</script>`</span></span>

### <a name="list"></a><span data-ttu-id="9bc44-146">Liste</span><span class="sxs-lookup"><span data-stu-id="9bc44-146">List</span></span>

<span data-ttu-id="9bc44-147">[TWEETS aus @Microsoft/MS \_Consumer\_Bänder](https://twitter.com/microsoft/ms-consumer-brands/)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + "://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore(js, fjs); } }(document, "script", "twitter-wjs");</script>`</span><span class="sxs-lookup"><span data-stu-id="9bc44-147">[Tweets from @Microsoft/MS\_Consumer\_Bands](https://twitter.com/microsoft/ms-consumer-brands/)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + "://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore(js, fjs); } }(document, "script", "twitter-wjs");</script>`</span></span>

### <a name="search"></a><span data-ttu-id="9bc44-148">Suchen</span><span class="sxs-lookup"><span data-stu-id="9bc44-148">Search</span></span>

<span data-ttu-id="9bc44-149">[Einen Tweet zu &quot;#asp.net&quot;](https://twitter.com/search?q=%23asp.net)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + "://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore(js, fjs); } }(document, "script", "twitter-wjs");</script>`</span><span class="sxs-lookup"><span data-stu-id="9bc44-149">[Tweets about &quot;#asp.net&quot;](https://twitter.com/search?q=%23asp.net)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + "://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore(js, fjs); } }(document, "script", "twitter-wjs");</script>`</span></span>
