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
<a name="twitter-helper-with-aspnet-web-pages"></a>Twitter-Hilfsprogramm mit ASP.NET Web Pages
====================
durch [Tom FitzMacken](https://github.com/tfitzmac)

> [!IMPORTANT]
> Twitter-Hilfsprogramme sind veraltet. Der Twitter neueste Engagement-Tools für Websites, finden Sie unter [Twitter-Übersicht über die Websites](https://developer.twitter.com/en/docs/twitter-for-websites/overview).

> Dieses Thema und die Anwendung zeigen, wie ein Twitter-Hilfsprogramm das WebMatrix 3-Projekt hinzu. Es enthält den Code für die Twitter-Hilfsprogramm und zeigt, wie die Hilfsmethoden.
> 
> Dieser Code für die Datei Twitter.cshtml wurde entwickelt, indem **Tian Pan** von Microsoft.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Softwareversionen, die in diesem Tutorial verwendet werden.
> 
> 
> - ASP.NET Web Pages (Razor) 3
>   
> 
> In diesem Tutorial funktioniert auch mit ASP.NET Web Pages 2.


## <a name="introduction"></a>Einführung

Dieses Thema veranschaulicht das Hinzufügen einer Twitter-Hilfsprogramm zu Ihrer Anwendung und Razor-Syntax zum Aufrufen von der Hilfsmethoden verwenden. Die Twitter-Hilfsprogramm vereinfacht das Twitter-Schaltflächen und Widgets in Ihrer Anwendung zu integrieren. Verwenden ein Twitter-Widgets, wie z. B. die Timeline eines Benutzers oder der Ergebnisse der Suche nach einem Hashtag, erstellen Sie zuerst die [Widget auf Twitter](https://twitter.com/settings/widgets). Nach dem Erstellen Ihres Widgets, erhalten Sie eine Widget-Id. Sie übergeben dieses Widget-Id als Parameter beim Aufrufen von Methoden des Hilfsprogramms, die Widget anzeigen.

In diesem Thema wurde für Version 1.1 der Twitter-API geschrieben. Durch das direkte Hinzufügen des Twitter-Hilfsprogramm-Codes zu Ihrem Projekt können Sie des hilfscodes aktualisieren, wenn der Twitter-API ändern.

Weitere Informationen zum Installieren von WebMatrix, finden Sie unter [Einführung in ASP.NET Web Pages 2 - erste Schritte](../getting-started/introducing-aspnet-web-pages-2/getting-started.md).

## <a name="add-twitter-helper-to-your-project"></a>Hinzufügen von Twitter-Hilfsprogramm zu Ihrem Projekt

Um die Twitter-Hilfsprogramm hinzuzufügen, fügen Sie zunächst einen Ordner namens **App\_Code** zu Ihrem Projekt. Erstellen Sie dann eine Datei namens **Twitter.cshtml**.

![Ordner "App_Code"](twitter-helper/_static/image1.png)

Ersetzen Sie den Standardcode in Twitter.cshtml durch den folgenden Code ein.

[!code-cshtml[Main](twitter-helper/samples/sample1.cshtml)]

## <a name="call-twitter-methods-from-your-web-pages"></a>Rufen Sie Ihren Webseiten Twitter-Methoden

Das folgende Beispiel zeigt, wie Sie die Twitter-Hilfsprogramm-Methoden von einer Seite in Ihrem Projekt zu verwenden. In Ihrem Projekt möchten Sie die Parameterwerte durch Werte ersetzen, die an Ihre Anforderungen relevant sind. Können Sie die bereitgestellten Widget-Ids um zu untersuchen, wie die Methoden funktionieren, aber Sie möchten eigene Widgets für Ihr Projekt zu generieren.

Nicht alle der unten angezeigte Parameter sind erforderlich. Die optionalen Parameter werden verwendet, um anzupassen, wie die Schaltfläche oder das Widget angezeigt wird. Z. B. die Schaltfläche "folgen" erfordert den Benutzernamen ein, führen, aber das Beispiel zeigt, wie die Anzahl der Follower, und wie die Größe der Schaltfläche und die Sprache angeben.

[!code-html[Main](twitter-helper/samples/sample2.html)]

## <a name="see-the-results"></a>Die Ergebnisse angezeigt

Der obige Code generiert die folgenden Schaltflächen und Widgets. Diese Schaltflächen und die Widgets sind voll funktionsfähige, nicht-Screenshots. Die Schaltfläche "folgen" wird in Spanisch angezeigt, da es sich bei der Language-Parameter festgelegt wurde, um **es**.

### <a name="follow-button"></a>Führen Sie die Schaltfläche "

[Führen Sie @aspnet)](https://twitter.com/aspnet)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + '://platform.twitter.com/widgets.js'; fjs.parentNode.insertBefore(js, fjs); } }(document, 'script', 'twitter-wjs');</script>`

### <a name="tweet-button"></a>Schaltfläche "Tweet"

[Tweet](https://twitter.com/share)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + '://platform.twitter.com/widgets.js'; fjs.parentNode.insertBefore(js, fjs); } }(document, 'script', 'twitter-wjs');</script>`

### <a name="user-timeline-profile"></a>Timeline des Benutzers (Profil)

[TWEETS nach @aspnet](https://twitter.com/aspnet)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + "://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore(js, fjs); } }(document, "script", "twitter-wjs");</script>`

### <a name="favorites"></a>Favoriten

[Bevorzugte Tweets nach @Microsoft](https://twitter.com/Microsoft/favorites)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + "://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore(js, fjs); } }(document, "script", "twitter-wjs");</script>`

### <a name="list"></a>Liste

[TWEETS aus @Microsoft/MS \_Consumer\_Bänder](https://twitter.com/microsoft/ms-consumer-brands/)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + "://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore(js, fjs); } }(document, "script", "twitter-wjs");</script>`

### <a name="search"></a>Suchen

[Einen Tweet zu &quot;#asp.net&quot;](https://twitter.com/search?q=%23asp.net)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + "://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore(js, fjs); } }(document, "script", "twitter-wjs");</script>`
