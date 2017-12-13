---
uid: web-pages/overview/ui-layouts-and-themes/twitter-helper
title: Twitter-Hilfsprogramm mit ASP.NET Web Pages | Microsoft Docs
author: tfitzmac
description: "Dieses Thema und die Anwendung gezeigt, wie ein Twitter-Hilfsprogramm zu Ihrer WebMatrix-3-Projekt hinzufügen. Es enthält den Code der Twitter-Hilfsprogramm und zeigt, wie das Hilfsprogramm..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/07/2014
ms.topic: article
ms.assetid: c1a1244e-b9c8-42e6-a00b-8456a4ec027c
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/twitter-helper
msc.type: authoredcontent
ms.openlocfilehash: 07d9c4d485c42b78a42c54c9740af5f67cb44763
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="twitter-helper-with-aspnet-web-pages"></a><span data-ttu-id="8f9c1-104">Twitter-Hilfsprogramm mit ASP.NET Web Pages</span><span class="sxs-lookup"><span data-stu-id="8f9c1-104">Twitter Helper with ASP.NET Web Pages</span></span>
====================
<span data-ttu-id="8f9c1-105">durch [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="8f9c1-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="8f9c1-106">Dieses Thema und die Anwendung gezeigt, wie ein Twitter-Hilfsprogramm zu Ihrer WebMatrix-3-Projekt hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="8f9c1-106">This topic and application show how to add a Twitter Helper to your WebMatrix 3 project.</span></span> <span data-ttu-id="8f9c1-107">Es enthält den Code der Twitter-Hilfsprogramm und gezeigt, wie die Hilfsmethoden aufrufen.</span><span class="sxs-lookup"><span data-stu-id="8f9c1-107">It contains the Twitter Helper code and shows how to call the helper methods.</span></span>
> 
> <span data-ttu-id="8f9c1-108">Dieser Code für die Datei Twitter.cshtml wurde von entwickelt **Tian Pan** von Microsoft.</span><span class="sxs-lookup"><span data-stu-id="8f9c1-108">This code for the Twitter.cshtml file was developed by **Tian Pan** of Microsoft.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="8f9c1-109">In diesem Lernprogramm verwendeten Versionen der Software</span><span class="sxs-lookup"><span data-stu-id="8f9c1-109">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="8f9c1-110">ASP.NET Web Pages (Razor) 3</span><span class="sxs-lookup"><span data-stu-id="8f9c1-110">ASP.NET Web Pages (Razor) 3</span></span>
>   
> 
> <span data-ttu-id="8f9c1-111">Dieses Lernprogramm funktioniert auch mit ASP.NET Web Pages 2.</span><span class="sxs-lookup"><span data-stu-id="8f9c1-111">This tutorial also works with ASP.NET Web Pages 2.</span></span>


## <a name="introduction"></a><span data-ttu-id="8f9c1-112">Einführung</span><span class="sxs-lookup"><span data-stu-id="8f9c1-112">Introduction</span></span>

<span data-ttu-id="8f9c1-113">In diesem Thema veranschaulicht, wie ein Twitter-Hilfsprogramm zur Anwendung hinzufügen und verwenden Razor-Syntax, um die Hilfemethoden aufzurufen.</span><span class="sxs-lookup"><span data-stu-id="8f9c1-113">This topic demonstrates how to add a Twitter Helper to your application and use Razor syntax to call the helper methods.</span></span> <span data-ttu-id="8f9c1-114">Twitter-Hilfsprogramm vereinfacht das Twitter-Schaltflächen und Widgets in Ihrer Anwendung zu integrieren.</span><span class="sxs-lookup"><span data-stu-id="8f9c1-114">The Twitter Helper makes it easy to incorporate Twitter buttons and widgets in your application.</span></span> <span data-ttu-id="8f9c1-115">Mit einem Widget "Twitter", z. B. eines Benutzers Zeitachse oder die Ergebnisse der Suche nach einem Hashtag erstellen Sie zuerst die [Widget auf Twitter](https://twitter.com/settings/widgets).</span><span class="sxs-lookup"><span data-stu-id="8f9c1-115">To use a Twitter widget, such as a user's timeline or the search results for a hashtag, you must first create the [widget on Twitter](https://twitter.com/settings/widgets).</span></span> <span data-ttu-id="8f9c1-116">Nach dem Erstellen der Widget, erhalten Sie eine Widget-Id. Sie übergeben dieses Widget-Id als Parameter beim Aufrufen von Hilfsmethoden, die das Widget veranschaulichen.</span><span class="sxs-lookup"><span data-stu-id="8f9c1-116">After creating your widget, you will receive a widget id. You pass this widget id as a parameter when calling the helper methods that show widget.</span></span>

<span data-ttu-id="8f9c1-117">Dieses Thema wurde für Version 1.1 von der Twitter-API geschrieben.</span><span class="sxs-lookup"><span data-stu-id="8f9c1-117">This topic was written for version 1.1 of the Twitter API.</span></span> <span data-ttu-id="8f9c1-118">Durch den Twitter-Hilfsprogramm-Code wird direkt zu Ihrem Projekt hinzufügen, können Sie die Hilfscode aktualisiert, wenn der Twitter-API sich ändert.</span><span class="sxs-lookup"><span data-stu-id="8f9c1-118">By directly adding the Twitter Helper code to your project, you can update the helper code if the Twitter API changes.</span></span>

<span data-ttu-id="8f9c1-119">Informationen zum Installieren von WebMatrix finden Sie unter [Einführung in ASP.NET Web Pages 2 - erste Schritte](../getting-started/introducing-aspnet-web-pages-2/getting-started.md).</span><span class="sxs-lookup"><span data-stu-id="8f9c1-119">For information about installing WebMatrix, see [Introducing ASP.NET Web Pages 2 - Getting Started](../getting-started/introducing-aspnet-web-pages-2/getting-started.md).</span></span>

## <a name="add-twitter-helper-to-your-project"></a><span data-ttu-id="8f9c1-120">Twitter-Hilfsprogramm zu Ihrem Projekt hinzufügen</span><span class="sxs-lookup"><span data-stu-id="8f9c1-120">Add Twitter Helper to your project</span></span>

<span data-ttu-id="8f9c1-121">Um die Twitter-Hilfsprogramm hinzuzufügen, fügen Sie zunächst einen Ordner namens **App\_Code** zu Ihrem Projekt.</span><span class="sxs-lookup"><span data-stu-id="8f9c1-121">To add the Twitter Helper, first, add a folder named **App\_Code** to your project.</span></span> <span data-ttu-id="8f9c1-122">Erstellen Sie dann eine Datei namens **Twitter.cshtml**.</span><span class="sxs-lookup"><span data-stu-id="8f9c1-122">Then, create a file named **Twitter.cshtml**.</span></span>

![Ordner "App_Code"](twitter-helper/_static/image1.png)

<span data-ttu-id="8f9c1-124">Ersetzen Sie den Standardcode in Twitter.cshtml durch den folgenden Code ein.</span><span class="sxs-lookup"><span data-stu-id="8f9c1-124">Replace the default code in Twitter.cshtml with the following code.</span></span>

[!code-cshtml[Main](twitter-helper/samples/sample1.cshtml)]

## <a name="call-twitter-methods-from-your-web-pages"></a><span data-ttu-id="8f9c1-125">Twitter-Methoden von Webseiten aus aufrufen</span><span class="sxs-lookup"><span data-stu-id="8f9c1-125">Call Twitter methods from your web pages</span></span>

<span data-ttu-id="8f9c1-126">Im folgende Beispiel wird gezeigt, wie Methoden Twitter-Hilfsprogramm von einer Seite in Ihrem Projekt verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="8f9c1-126">The following example shows how to use the Twitter Helper methods from a page in your project.</span></span> <span data-ttu-id="8f9c1-127">In Ihrem Projekt sollten Sie die Parameterwerte durch Werte ersetzen, die an Ihre Bedürfnisse relevant sind.</span><span class="sxs-lookup"><span data-stu-id="8f9c1-127">In your project, you will want to replace the parameter values with values that are relevant to your needs.</span></span> <span data-ttu-id="8f9c1-128">Können Sie die bereitgestellten Widget-Ids um zu untersuchen, wie die Methoden funktionieren allerdings möchten Sie eigene Widgets für Ihr Projekt generieren.</span><span class="sxs-lookup"><span data-stu-id="8f9c1-128">You can use the provided widget ids to explore how the methods work, but you will want to generate your own widgets for your project.</span></span>

<span data-ttu-id="8f9c1-129">Nicht alle der unten angezeigten Parameter sind erforderlich.</span><span class="sxs-lookup"><span data-stu-id="8f9c1-129">Not all of the parameters shown below are required.</span></span> <span data-ttu-id="8f9c1-130">Die optionalen Parameter werden verwendet, um anzupassen, wie die Schaltfläche oder das Widget angezeigt wird.</span><span class="sxs-lookup"><span data-stu-id="8f9c1-130">The optional parameters are used to customize how the button or widget is displayed.</span></span> <span data-ttu-id="8f9c1-131">Beispielsweise wird die Schaltfläche "folgen" den Benutzernamen ein, führen erfordert, aber im Beispiel wird gezeigt, wie die Anzahl der nachfolgende Aktivitäten, und geben Sie die Größe der Schaltfläche und die jeweilige Sprache wie.</span><span class="sxs-lookup"><span data-stu-id="8f9c1-131">For example, the Follow Button only requires the user name to follow, but the example shows how to include the number of followers, and how specify the size of the button and the language.</span></span>

[!code-html[Main](twitter-helper/samples/sample2.html)]

## <a name="see-the-results"></a><span data-ttu-id="8f9c1-132">Die Ergebnisse anzuzeigen</span><span class="sxs-lookup"><span data-stu-id="8f9c1-132">See the results</span></span>

<span data-ttu-id="8f9c1-133">Der obige Code generiert die folgenden Schaltflächen und Widgets.</span><span class="sxs-lookup"><span data-stu-id="8f9c1-133">The above code produces the following buttons and widgets.</span></span> <span data-ttu-id="8f9c1-134">Diese Schaltflächen und Widgets voll funktionsfähig sind, nicht Screenshots.</span><span class="sxs-lookup"><span data-stu-id="8f9c1-134">These buttons and widgets are fully-functional, not screenshots.</span></span> <span data-ttu-id="8f9c1-135">Die Schaltfläche "folgen" wird in Spanisch angezeigt, da der Sprachparameter, um festgelegt wurde **vorangestellten**.</span><span class="sxs-lookup"><span data-stu-id="8f9c1-135">The Follow Button is displayed in Spanish because the language parameter was set to **es**.</span></span>

### <a name="follow-button"></a><span data-ttu-id="8f9c1-136">Führen Sie die Schaltfläche</span><span class="sxs-lookup"><span data-stu-id="8f9c1-136">Follow Button</span></span>

<span data-ttu-id="8f9c1-137">[Führen Sie die @aspnet)](https://twitter.com/aspnet)<script>!-Funktion ("d", "s", "Id") {Var Js, Fjs = d.getElementsByTagName(s) [0], p = /^http:/.test(d.location)? "http": "Https"; Wenn (! d.getElementById(id)) {Js = d.createElement(s); js.id = Id; js.src = p + ': / / platform.twitter.com/widgets.js'; fjs.parentNode.insertBefore (Js, Fjs);}} (Dokument, "Skript", 'Twitter-Wjs');</script></span><span class="sxs-lookup"><span data-stu-id="8f9c1-137">[Follow @aspnet)](https://twitter.com/aspnet)<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + '://platform.twitter.com/widgets.js'; fjs.parentNode.insertBefore(js, fjs); } }(document, 'script', 'twitter-wjs');</script></span></span>

### <a name="tweet-button"></a><span data-ttu-id="8f9c1-138">Tweet-Schaltfläche</span><span class="sxs-lookup"><span data-stu-id="8f9c1-138">Tweet Button</span></span>

<span data-ttu-id="8f9c1-139">[Tweet](https://twitter.com/share)<script>!-Funktion ("d", "s", "Id") {Var Js, Fjs = d.getElementsByTagName(s) [0], p = /^http:/.test(d.location)? "http": "Https"; Wenn (! d.getElementById(id)) {Js = d.createElement(s); js.id = Id; js.src = p + ': / / platform.twitter.com/widgets.js'; fjs.parentNode.insertBefore (Js, Fjs);}} (Dokument, "Skript", 'Twitter-Wjs');</script></span><span class="sxs-lookup"><span data-stu-id="8f9c1-139">[Tweet](https://twitter.com/share)<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + '://platform.twitter.com/widgets.js'; fjs.parentNode.insertBefore(js, fjs); } }(document, 'script', 'twitter-wjs');</script></span></span>

### <a name="user-timeline-profile"></a><span data-ttu-id="8f9c1-140">Benutzer-Zeitachse (Profil)</span><span class="sxs-lookup"><span data-stu-id="8f9c1-140">User Timeline (Profile)</span></span>

<span data-ttu-id="8f9c1-141">[TWEETS von @aspnet ](https://twitter.com/aspnet) <script>!-Funktion ("d", "s", "Id") {Var Js, Fjs = d.getElementsByTagName(s) [0], p = /^http:/.test(d.location)? "http": "Https"; Wenn (! d.getElementById(id)) {Js = d.createElement(s); js.id = Id; js.src = p + ": / / platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore (Js, Fjs);}} (Dokument, "Skript", "Twitter-Wjs");</script></span><span class="sxs-lookup"><span data-stu-id="8f9c1-141">[Tweets by @aspnet](https://twitter.com/aspnet)<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + "://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore(js, fjs); } }(document, "script", "twitter-wjs");</script></span></span>

### <a name="favorites"></a><span data-ttu-id="8f9c1-142">Favoriten</span><span class="sxs-lookup"><span data-stu-id="8f9c1-142">Favorites</span></span>

<span data-ttu-id="8f9c1-143">[Favoriten Tweets von @Microsoft ](https://twitter.com/Microsoft/favorites) <script>!-Funktion ("d", "s", "Id") {Var Js, Fjs = d.getElementsByTagName(s) [0], p = /^http:/.test(d.location)? "http": "Https"; Wenn (! d.getElementById(id)) {Js = d.createElement(s); js.id = Id; js.src = p + ": / / platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore (Js, Fjs);}} (Dokument, "Skript", "Twitter-Wjs");</script></span><span class="sxs-lookup"><span data-stu-id="8f9c1-143">[Favorite Tweets by @Microsoft](https://twitter.com/Microsoft/favorites)<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + "://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore(js, fjs); } }(document, "script", "twitter-wjs");</script></span></span>

### <a name="list"></a><span data-ttu-id="8f9c1-144">Liste</span><span class="sxs-lookup"><span data-stu-id="8f9c1-144">List</span></span>

<span data-ttu-id="8f9c1-145">[TWEETS aus @Microsoft/MS \_Consumer\_Bänder](https://twitter.com/microsoft/ms-consumer-brands/)<script>!-Funktion ("d", "s", "Id") {Var Js, Fjs = d.getElementsByTagName(s) [0], p = /^http:/.test(d.location)? "http": "Https"; Wenn (! d.getElementById(id)) {Js = d.createElement(s); js.id = Id; js.src = p + ": / / platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore (Js, Fjs);}} (Dokument, "Skript", "Twitter-Wjs");</script></span><span class="sxs-lookup"><span data-stu-id="8f9c1-145">[Tweets from @Microsoft/MS\_Consumer\_Bands](https://twitter.com/microsoft/ms-consumer-brands/)<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + "://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore(js, fjs); } }(document, "script", "twitter-wjs");</script></span></span>

### <a name="search"></a><span data-ttu-id="8f9c1-146">Suchen</span><span class="sxs-lookup"><span data-stu-id="8f9c1-146">Search</span></span>

<span data-ttu-id="8f9c1-147">[TWEETS zu &quot;#asp.net&quot;](https://twitter.com/search?q=%23asp.net)<script>!-Funktion ("d", "s", "Id") {Var Js, Fjs = d.getElementsByTagName(s) [0], p = /^http:/.test(d.location)? "http": "Https"; Wenn (! d.getElementById(id)) {Js = d.createElement(s); js.id = Id; js.src = p + ": / / platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore (Js, Fjs);}} (Dokument, "Skript", "Twitter-Wjs");</script></span><span class="sxs-lookup"><span data-stu-id="8f9c1-147">[Tweets about &quot;#asp.net&quot;](https://twitter.com/search?q=%23asp.net)<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + "://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore(js, fjs); } }(document, "script", "twitter-wjs");</script></span></span>
