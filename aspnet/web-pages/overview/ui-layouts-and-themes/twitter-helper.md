---
uid: web-pages/overview/ui-layouts-and-themes/twitter-helper
title: Twitter-Hilfsprogramm mit ASP.NET Web Pages | Microsoft-Dokumentation
author: Rick-Anderson
description: Dieses Thema und die Anwendung zeigen, wie ein Twitter-Hilfsprogramm das WebMatrix 3-Projekt hinzu. Es enthält den Code für die Twitter-Hilfsprogramm und zeigt, wie Sie das Hilfsobjekt aufrufen...
ms.author: riande
ms.date: 02/07/2014
ms.assetid: c1a1244e-b9c8-42e6-a00b-8456a4ec027c
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/twitter-helper
msc.type: authoredcontent
ms.openlocfilehash: 89c8c520cd32ca2ee24e6cd90e11f7bdf39c7a80
ms.sourcegitcommit: 2d3e5422d530203efdaf2014d1d7df31f88d08d0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/05/2018
ms.locfileid: "51020572"
---
<a name="twitter-helper-with-aspnet-web-pages"></a><span data-ttu-id="2c974-104">Twitter-Hilfsprogramm mit ASP.NET Web Pages</span><span class="sxs-lookup"><span data-stu-id="2c974-104">Twitter Helper with ASP.NET Web Pages</span></span>
====================
<span data-ttu-id="2c974-105">durch [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="2c974-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="2c974-106">Dieses Thema und die Anwendung zeigen, wie ein Twitter-Hilfsprogramm das WebMatrix 3-Projekt hinzu.</span><span class="sxs-lookup"><span data-stu-id="2c974-106">This topic and application show how to add a Twitter Helper to your WebMatrix 3 project.</span></span> <span data-ttu-id="2c974-107">Es enthält den Code für die Twitter-Hilfsprogramm und zeigt, wie die Hilfsmethoden.</span><span class="sxs-lookup"><span data-stu-id="2c974-107">It contains the Twitter Helper code and shows how to call the helper methods.</span></span>
> 
> <span data-ttu-id="2c974-108">Dieser Code für die Datei Twitter.cshtml wurde entwickelt, indem **Tian Pan** von Microsoft.</span><span class="sxs-lookup"><span data-stu-id="2c974-108">This code for the Twitter.cshtml file was developed by **Tian Pan** of Microsoft.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="2c974-109">Softwareversionen, die in diesem Tutorial verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="2c974-109">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="2c974-110">ASP.NET Web Pages (Razor) 3</span><span class="sxs-lookup"><span data-stu-id="2c974-110">ASP.NET Web Pages (Razor) 3</span></span>
>   
> 
> <span data-ttu-id="2c974-111">In diesem Tutorial funktioniert auch mit ASP.NET Web Pages 2.</span><span class="sxs-lookup"><span data-stu-id="2c974-111">This tutorial also works with ASP.NET Web Pages 2.</span></span>


## <a name="introduction"></a><span data-ttu-id="2c974-112">Einführung</span><span class="sxs-lookup"><span data-stu-id="2c974-112">Introduction</span></span>

<span data-ttu-id="2c974-113">Dieses Thema veranschaulicht das Hinzufügen einer Twitter-Hilfsprogramm zu Ihrer Anwendung und Razor-Syntax zum Aufrufen von der Hilfsmethoden verwenden.</span><span class="sxs-lookup"><span data-stu-id="2c974-113">This topic demonstrates how to add a Twitter Helper to your application and use Razor syntax to call the helper methods.</span></span> <span data-ttu-id="2c974-114">Die Twitter-Hilfsprogramm vereinfacht das Twitter-Schaltflächen und Widgets in Ihrer Anwendung zu integrieren.</span><span class="sxs-lookup"><span data-stu-id="2c974-114">The Twitter Helper makes it easy to incorporate Twitter buttons and widgets in your application.</span></span> <span data-ttu-id="2c974-115">Verwenden ein Twitter-Widgets, wie z. B. die Timeline eines Benutzers oder der Ergebnisse der Suche nach einem Hashtag, erstellen Sie zuerst die [Widget auf Twitter](https://twitter.com/settings/widgets).</span><span class="sxs-lookup"><span data-stu-id="2c974-115">To use a Twitter widget, such as a user's timeline or the search results for a hashtag, you must first create the [widget on Twitter](https://twitter.com/settings/widgets).</span></span> <span data-ttu-id="2c974-116">Nach dem Erstellen Ihres Widgets, erhalten Sie eine Widget-Id. Sie übergeben dieses Widget-Id als Parameter beim Aufrufen von Methoden des Hilfsprogramms, die Widget anzeigen.</span><span class="sxs-lookup"><span data-stu-id="2c974-116">After creating your widget, you will receive a widget id. You pass this widget id as a parameter when calling the helper methods that show widget.</span></span>

<span data-ttu-id="2c974-117">In diesem Thema wurde für Version 1.1 der Twitter-API geschrieben.</span><span class="sxs-lookup"><span data-stu-id="2c974-117">This topic was written for version 1.1 of the Twitter API.</span></span> <span data-ttu-id="2c974-118">Durch das direkte Hinzufügen des Twitter-Hilfsprogramm-Codes zu Ihrem Projekt können Sie des hilfscodes aktualisieren, wenn der Twitter-API ändern.</span><span class="sxs-lookup"><span data-stu-id="2c974-118">By directly adding the Twitter Helper code to your project, you can update the helper code if the Twitter API changes.</span></span>

<span data-ttu-id="2c974-119">Weitere Informationen zum Installieren von WebMatrix, finden Sie unter [Einführung in ASP.NET Web Pages 2 - erste Schritte](../getting-started/introducing-aspnet-web-pages-2/getting-started.md).</span><span class="sxs-lookup"><span data-stu-id="2c974-119">For information about installing WebMatrix, see [Introducing ASP.NET Web Pages 2 - Getting Started](../getting-started/introducing-aspnet-web-pages-2/getting-started.md).</span></span>

## <a name="add-twitter-helper-to-your-project"></a><span data-ttu-id="2c974-120">Hinzufügen von Twitter-Hilfsprogramm zu Ihrem Projekt</span><span class="sxs-lookup"><span data-stu-id="2c974-120">Add Twitter Helper to your project</span></span>

<span data-ttu-id="2c974-121">Um die Twitter-Hilfsprogramm hinzuzufügen, fügen Sie zunächst einen Ordner namens **App\_Code** zu Ihrem Projekt.</span><span class="sxs-lookup"><span data-stu-id="2c974-121">To add the Twitter Helper, first, add a folder named **App\_Code** to your project.</span></span> <span data-ttu-id="2c974-122">Erstellen Sie dann eine Datei namens **Twitter.cshtml**.</span><span class="sxs-lookup"><span data-stu-id="2c974-122">Then, create a file named **Twitter.cshtml**.</span></span>

![Ordner "App_Code"](twitter-helper/_static/image1.png)

<span data-ttu-id="2c974-124">Ersetzen Sie den Standardcode in Twitter.cshtml durch den folgenden Code ein.</span><span class="sxs-lookup"><span data-stu-id="2c974-124">Replace the default code in Twitter.cshtml with the following code.</span></span>

[!code-cshtml[Main](twitter-helper/samples/sample1.cshtml)]

## <a name="call-twitter-methods-from-your-web-pages"></a><span data-ttu-id="2c974-125">Rufen Sie Ihren Webseiten Twitter-Methoden</span><span class="sxs-lookup"><span data-stu-id="2c974-125">Call Twitter methods from your web pages</span></span>

<span data-ttu-id="2c974-126">Das folgende Beispiel zeigt, wie Sie die Twitter-Hilfsprogramm-Methoden von einer Seite in Ihrem Projekt zu verwenden.</span><span class="sxs-lookup"><span data-stu-id="2c974-126">The following example shows how to use the Twitter Helper methods from a page in your project.</span></span> <span data-ttu-id="2c974-127">In Ihrem Projekt möchten Sie die Parameterwerte durch Werte ersetzen, die an Ihre Anforderungen relevant sind.</span><span class="sxs-lookup"><span data-stu-id="2c974-127">In your project, you will want to replace the parameter values with values that are relevant to your needs.</span></span> <span data-ttu-id="2c974-128">Können Sie die bereitgestellten Widget-Ids um zu untersuchen, wie die Methoden funktionieren, aber Sie möchten eigene Widgets für Ihr Projekt zu generieren.</span><span class="sxs-lookup"><span data-stu-id="2c974-128">You can use the provided widget ids to explore how the methods work, but you will want to generate your own widgets for your project.</span></span>

<span data-ttu-id="2c974-129">Nicht alle der unten angezeigte Parameter sind erforderlich.</span><span class="sxs-lookup"><span data-stu-id="2c974-129">Not all of the parameters shown below are required.</span></span> <span data-ttu-id="2c974-130">Die optionalen Parameter werden verwendet, um anzupassen, wie die Schaltfläche oder das Widget angezeigt wird.</span><span class="sxs-lookup"><span data-stu-id="2c974-130">The optional parameters are used to customize how the button or widget is displayed.</span></span> <span data-ttu-id="2c974-131">Z. B. die Schaltfläche "folgen" erfordert den Benutzernamen ein, führen, aber das Beispiel zeigt, wie die Anzahl der Follower, und wie die Größe der Schaltfläche und die Sprache angeben.</span><span class="sxs-lookup"><span data-stu-id="2c974-131">For example, the Follow Button only requires the user name to follow, but the example shows how to include the number of followers, and how specify the size of the button and the language.</span></span>

[!code-html[Main](twitter-helper/samples/sample2.html)]

## <a name="see-the-results"></a><span data-ttu-id="2c974-132">Die Ergebnisse angezeigt</span><span class="sxs-lookup"><span data-stu-id="2c974-132">See the results</span></span>

<span data-ttu-id="2c974-133">Der obige Code generiert die folgenden Schaltflächen und Widgets.</span><span class="sxs-lookup"><span data-stu-id="2c974-133">The above code produces the following buttons and widgets.</span></span> <span data-ttu-id="2c974-134">Diese Schaltflächen und die Widgets sind voll funktionsfähige, nicht-Screenshots.</span><span class="sxs-lookup"><span data-stu-id="2c974-134">These buttons and widgets are fully-functional, not screenshots.</span></span> <span data-ttu-id="2c974-135">Die Schaltfläche "folgen" wird in Spanisch angezeigt, da es sich bei der Language-Parameter festgelegt wurde, um **es**.</span><span class="sxs-lookup"><span data-stu-id="2c974-135">The Follow Button is displayed in Spanish because the language parameter was set to **es**.</span></span>

### <a name="follow-button"></a><span data-ttu-id="2c974-136">Führen Sie die Schaltfläche "</span><span class="sxs-lookup"><span data-stu-id="2c974-136">Follow Button</span></span>

<span data-ttu-id="2c974-137">[Führen Sie @aspnet)](https://twitter.com/aspnet)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + '://platform.twitter.com/widgets.js'; fjs.parentNode.insertBefore(js, fjs); } }(document, 'script', 'twitter-wjs');</script>`</span><span class="sxs-lookup"><span data-stu-id="2c974-137">[Follow @aspnet)](https://twitter.com/aspnet)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + '://platform.twitter.com/widgets.js'; fjs.parentNode.insertBefore(js, fjs); } }(document, 'script', 'twitter-wjs');</script>`</span></span>

### <a name="tweet-button"></a><span data-ttu-id="2c974-138">Schaltfläche "Tweet"</span><span class="sxs-lookup"><span data-stu-id="2c974-138">Tweet Button</span></span>

<span data-ttu-id="2c974-139">[Tweet](https://twitter.com/share)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + '://platform.twitter.com/widgets.js'; fjs.parentNode.insertBefore(js, fjs); } }(document, 'script', 'twitter-wjs');</script>`</span><span class="sxs-lookup"><span data-stu-id="2c974-139">[Tweet](https://twitter.com/share)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + '://platform.twitter.com/widgets.js'; fjs.parentNode.insertBefore(js, fjs); } }(document, 'script', 'twitter-wjs');</script>`</span></span>

### <a name="user-timeline-profile"></a><span data-ttu-id="2c974-140">Timeline des Benutzers (Profil)</span><span class="sxs-lookup"><span data-stu-id="2c974-140">User Timeline (Profile)</span></span>

<span data-ttu-id="2c974-141">[TWEETS nach @aspnet](https://twitter.com/aspnet)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + "://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore(js, fjs); } }(document, "script", "twitter-wjs");</script>`</span><span class="sxs-lookup"><span data-stu-id="2c974-141">[Tweets by @aspnet](https://twitter.com/aspnet)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + "://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore(js, fjs); } }(document, "script", "twitter-wjs");</script>`</span></span>

### <a name="favorites"></a><span data-ttu-id="2c974-142">Favoriten</span><span class="sxs-lookup"><span data-stu-id="2c974-142">Favorites</span></span>

<span data-ttu-id="2c974-143">[Bevorzugte Tweets nach @Microsoft](https://twitter.com/Microsoft/favorites)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + "://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore(js, fjs); } }(document, "script", "twitter-wjs");</script>`</span><span class="sxs-lookup"><span data-stu-id="2c974-143">[Favorite Tweets by @Microsoft](https://twitter.com/Microsoft/favorites)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + "://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore(js, fjs); } }(document, "script", "twitter-wjs");</script>`</span></span>

### <a name="list"></a><span data-ttu-id="2c974-144">Liste</span><span class="sxs-lookup"><span data-stu-id="2c974-144">List</span></span>

<span data-ttu-id="2c974-145">[TWEETS aus @Microsoft/MS \_Consumer\_Bänder](https://twitter.com/microsoft/ms-consumer-brands/)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + "://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore(js, fjs); } }(document, "script", "twitter-wjs");</script>`</span><span class="sxs-lookup"><span data-stu-id="2c974-145">[Tweets from @Microsoft/MS\_Consumer\_Bands](https://twitter.com/microsoft/ms-consumer-brands/)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + "://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore(js, fjs); } }(document, "script", "twitter-wjs");</script>`</span></span>

### <a name="search"></a><span data-ttu-id="2c974-146">Suchen</span><span class="sxs-lookup"><span data-stu-id="2c974-146">Search</span></span>

<span data-ttu-id="2c974-147">[Einen Tweet zu &quot;#asp.net&quot;](https://twitter.com/search?q=%23asp.net)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + "://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore(js, fjs); } }(document, "script", "twitter-wjs");</script>`</span><span class="sxs-lookup"><span data-stu-id="2c974-147">[Tweets about &quot;#asp.net&quot;](https://twitter.com/search?q=%23asp.net)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + "://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore(js, fjs); } }(document, "script", "twitter-wjs");</script>`</span></span>
