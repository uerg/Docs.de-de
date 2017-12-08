---
uid: web-forms/overview/ajax-control-toolkit/animation/executing-animations-using-client-side-code-cs
title: "Ausführen von Animationen mit clientseitigem Code (c#) | Microsoft Docs"
author: wenz
description: "Animation-Steuerelement in ASP.NET AJAX-Steuerelement-Toolkit ist nicht nur ein Steuerelement, aber eine gesamte Framework Animationen an ein Steuerelement hinzufügen. Die Ausführung der Animation..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 0270e0df-6fde-4a8f-a2cb-2cacc55143f2
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/executing-animations-using-client-side-code-cs
msc.type: authoredcontent
ms.openlocfilehash: 6b1911686a79aa692ef193430cd0746a2511105a
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="executing-animations-using-client-side-code-c"></a>Ausführen von Animationen mit clientseitigem Code (c#)
====================
durch [Christian Wenz](https://github.com/wenz)

[Herunterladen von Code](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation10.cs.zip) oder [PDF herunterladen](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation10CS.pdf)

> Animation-Steuerelement in ASP.NET AJAX-Steuerelement-Toolkit ist nicht nur ein Steuerelement, aber eine gesamte Framework Animationen an ein Steuerelement hinzufügen. Die Ausführung der Animation kann auch mit benutzerdefinierten clientseitigen JavaScript-Code ausgelöst werden.


## <a name="overview"></a>Übersicht

Animation-Steuerelement in ASP.NET AJAX-Steuerelement-Toolkit ist nicht nur ein Steuerelement, aber eine gesamte Framework Animationen an ein Steuerelement hinzufügen. Die Ausführung der Animation kann auch mit benutzerdefinierten clientseitigen JavaScript-Code ausgelöst werden.

## <a name="steps"></a>Schritte

Erstens sind die `ScriptManager` auf der Seite; klicken Sie dann die ASP.NET AJAX-Bibliothek geladen ist, wodurch das Steuerelement-Toolkit verwenden:

[!code-aspx[Main](executing-animations-using-client-side-code-cs/samples/sample1.aspx)]

Die Animation wird auf einen Bereich des Texts angewendet werden, die wie folgt aussieht:

[!code-aspx[Main](executing-animations-using-client-side-code-cs/samples/sample2.aspx)]

Definieren Sie in der zugehörigen CSS-Klasse für den Bereich eine gute Hintergrundfarbe und auch festlegen Sie eine feste Breite für den Bereich:

[!code-css[Main](executing-animations-using-client-side-code-cs/samples/sample3.css)]

Fügen Sie dann die `AnimationExtender` auf der Seite "Bereitstellen einer `ID`, die `TargetControlID` Attribut und der Auswahlparameter `runat="server"`:

[!code-aspx[Main](executing-animations-using-client-side-code-cs/samples/sample4.aspx)]

Innerhalb der `<Animations>` Knoten verwenden `<OnClick>` klickt Sie auf den Animationen an die Benutzer nur einmalig ausgeführt im Bereich. Fügen Sie zwei Animationen parallelly ausgeführt werden:

[!code-xml[Main](executing-animations-using-client-side-code-cs/samples/sample5.xml)]

Aus Gründen der Demo werden dieser Animation (und anderen Animation, die mit dem Steuerelement Toolkit erstellt) ausgeführt mithilfe von JavaScript-Code, sobald die Seite ausgeführt wird. Erstens wir benötigen Zugriff auf die `AnimationExtender` Steuerelement. Die ASP.NET AJAX-Bibliothek stellt die `$find()` Funktion für diese Aufgabe:

[!code-csharp[Main](executing-animations-using-client-side-code-cs/samples/sample6.cs)]

Die `AnimationExtender` Steuerelement macht eine umfangreiche API, einschließlich der Methoden, mit dem Namen identisch ist, an die Ereignishandler in der XML-Markup verwendet: `OnClick()`, `OnLoad()`und so weiter. Z. B. einen Aufruf von der `OnClick()` Methode ausgeführt wird, die Animation innerhalb der `<OnClick>` Element von der `AnimationExtender` Steuerelement:

[!code-javascript[Main](executing-animations-using-client-side-code-cs/samples/sample7.js)]

Hier wird die vollständige clientseitigen JavaScript-Code, der dem Klicken auf den Bereich emuliert, nachdem die Seite vollständig geladen wurde Beachten Sie, dass die `pageLoad()` Funktionsname wird verwendet, der von ASP.NET AJAX einmal die Seite aufgerufen wird und alle enthaltenen JavaScript-Bibliotheken wurden geladen.

[!code-html[Main](executing-animations-using-client-side-code-cs/samples/sample8.html)]


[![Die Animation wird sofort ohne einen Mausklick ausgeführt.](executing-animations-using-client-side-code-cs/_static/image2.png)](executing-animations-using-client-side-code-cs/_static/image1.png)

Die Animation wird sofort ausgeführt, ohne einen Mausklick ([klicken Sie hier, um das Bild in voller Größe angezeigt](executing-animations-using-client-side-code-cs/_static/image3.png))

>[!div class="step-by-step"]
[Zurück](modifying-animations-from-the-server-side-cs.md)
[Weiter](changing-an-animation-using-client-side-code-cs.md)
