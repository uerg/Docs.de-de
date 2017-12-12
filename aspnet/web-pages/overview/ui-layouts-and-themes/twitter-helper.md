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
<a name="twitter-helper-with-aspnet-web-pages"></a>Twitter-Hilfsprogramm mit ASP.NET Web Pages
====================
durch [Tom FitzMacken](https://github.com/tfitzmac)

> Dieses Thema und die Anwendung gezeigt, wie ein Twitter-Hilfsprogramm zu Ihrer WebMatrix-3-Projekt hinzufügen. Es enthält den Code der Twitter-Hilfsprogramm und gezeigt, wie die Hilfsmethoden aufrufen.
> 
> Dieser Code für die Datei Twitter.cshtml wurde von entwickelt **Tian Pan** von Microsoft.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>In diesem Lernprogramm verwendeten Versionen der Software
> 
> 
> - ASP.NET Web Pages (Razor) 3
>   
> 
> Dieses Lernprogramm funktioniert auch mit ASP.NET Web Pages 2.


## <a name="introduction"></a>Einführung

In diesem Thema veranschaulicht, wie ein Twitter-Hilfsprogramm zur Anwendung hinzufügen und verwenden Razor-Syntax, um die Hilfemethoden aufzurufen. Twitter-Hilfsprogramm vereinfacht das Twitter-Schaltflächen und Widgets in Ihrer Anwendung zu integrieren. Mit einem Widget "Twitter", z. B. eines Benutzers Zeitachse oder die Ergebnisse der Suche nach einem Hashtag erstellen Sie zuerst die [Widget auf Twitter](https://twitter.com/settings/widgets). Nach dem Erstellen der Widget, erhalten Sie eine Widget-Id. Sie übergeben dieses Widget-Id als Parameter beim Aufrufen von Hilfsmethoden, die das Widget veranschaulichen.

Dieses Thema wurde für Version 1.1 von der Twitter-API geschrieben. Durch den Twitter-Hilfsprogramm-Code wird direkt zu Ihrem Projekt hinzufügen, können Sie die Hilfscode aktualisiert, wenn der Twitter-API sich ändert.

Informationen zum Installieren von WebMatrix finden Sie unter [Einführung in ASP.NET Web Pages 2 - erste Schritte](../getting-started/introducing-aspnet-web-pages-2/getting-started.md).

## <a name="add-twitter-helper-to-your-project"></a>Twitter-Hilfsprogramm zu Ihrem Projekt hinzufügen

Um die Twitter-Hilfsprogramm hinzuzufügen, fügen Sie zunächst einen Ordner namens **App\_Code** zu Ihrem Projekt. Erstellen Sie dann eine Datei namens **Twitter.cshtml**.

![Ordner "App_Code"](twitter-helper/_static/image1.png)

Ersetzen Sie den Standardcode in Twitter.cshtml durch den folgenden Code ein.

[!code-cshtml[Main](twitter-helper/samples/sample1.cshtml)]

## <a name="call-twitter-methods-from-your-web-pages"></a>Twitter-Methoden von Webseiten aus aufrufen

Im folgende Beispiel wird gezeigt, wie Methoden Twitter-Hilfsprogramm von einer Seite in Ihrem Projekt verwendet werden. In Ihrem Projekt sollten Sie die Parameterwerte durch Werte ersetzen, die an Ihre Bedürfnisse relevant sind. Können Sie die bereitgestellten Widget-Ids um zu untersuchen, wie die Methoden funktionieren allerdings möchten Sie eigene Widgets für Ihr Projekt generieren.

Nicht alle der unten angezeigten Parameter sind erforderlich. Die optionalen Parameter werden verwendet, um anzupassen, wie die Schaltfläche oder das Widget angezeigt wird. Beispielsweise wird die Schaltfläche "folgen" den Benutzernamen ein, führen erfordert, aber im Beispiel wird gezeigt, wie die Anzahl der nachfolgende Aktivitäten, und geben Sie die Größe der Schaltfläche und die jeweilige Sprache wie.

[!code-html[Main](twitter-helper/samples/sample2.html)]

## <a name="see-the-results"></a>Die Ergebnisse anzuzeigen

Der obige Code generiert die folgenden Schaltflächen und Widgets. Diese Schaltflächen und Widgets voll funktionsfähig sind, nicht Screenshots. Die Schaltfläche "folgen" wird in Spanisch angezeigt, da der Sprachparameter, um festgelegt wurde **vorangestellten**.

### <a name="follow-button"></a>Führen Sie die Schaltfläche

[Führen Sie die @aspnet)](https://twitter.com/aspnet)<script>!-Funktion ("d", "s", "Id") {Var Js, Fjs = d.getElementsByTagName(s) [0], p = /^http:/.test(d.location)? "http": "Https"; Wenn (! d.getElementById(id)) {Js = d.createElement(s); js.id = Id; js.src = p + ': / / platform.twitter.com/widgets.js'; fjs.parentNode.insertBefore (Js, Fjs);}} (Dokument, "Skript", 'Twitter-Wjs');</script>

### <a name="tweet-button"></a>Tweet-Schaltfläche

[Tweet](https://twitter.com/share)<script>!-Funktion ("d", "s", "Id") {Var Js, Fjs = d.getElementsByTagName(s) [0], p = /^http:/.test(d.location)? "http": "Https"; Wenn (! d.getElementById(id)) {Js = d.createElement(s); js.id = Id; js.src = p + ': / / platform.twitter.com/widgets.js'; fjs.parentNode.insertBefore (Js, Fjs);}} (Dokument, "Skript", 'Twitter-Wjs');</script>

### <a name="user-timeline-profile"></a>Benutzer-Zeitachse (Profil)

[TWEETS von @aspnet ](https://twitter.com/aspnet) <script>!-Funktion ("d", "s", "Id") {Var Js, Fjs = d.getElementsByTagName(s) [0], p = /^http:/.test(d.location)? "http": "Https"; Wenn (! d.getElementById(id)) {Js = d.createElement(s); js.id = Id; js.src = p + ": / / platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore (Js, Fjs);}} (Dokument, "Skript", "Twitter-Wjs");</script>

### <a name="favorites"></a>Favoriten

[Favoriten Tweets von @Microsoft ](https://twitter.com/Microsoft/favorites) <script>!-Funktion ("d", "s", "Id") {Var Js, Fjs = d.getElementsByTagName(s) [0], p = /^http:/.test(d.location)? "http": "Https"; Wenn (! d.getElementById(id)) {Js = d.createElement(s); js.id = Id; js.src = p + ": / / platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore (Js, Fjs);}} (Dokument, "Skript", "Twitter-Wjs");</script>

### <a name="list"></a>Liste

[TWEETS aus @Microsoft/MS \_Consumer\_Bänder](https://twitter.com/microsoft/ms-consumer-brands/)<script>!-Funktion ("d", "s", "Id") {Var Js, Fjs = d.getElementsByTagName(s) [0], p = /^http:/.test(d.location)? "http": "Https"; Wenn (! d.getElementById(id)) {Js = d.createElement(s); js.id = Id; js.src = p + ": / / platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore (Js, Fjs);}} (Dokument, "Skript", "Twitter-Wjs");</script>

### <a name="search"></a>Suchen

[TWEETS zu &quot;#asp.net&quot;](https://twitter.com/search?q=%23asp.net)<script>!-Funktion ("d", "s", "Id") {Var Js, Fjs = d.getElementsByTagName(s) [0], p = /^http:/.test(d.location)? "http": "Https"; Wenn (! d.getElementById(id)) {Js = d.createElement(s); js.id = Id; js.src = p + ": / / platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore (Js, Fjs);}} (Dokument, "Skript", "Twitter-Wjs");</script>
