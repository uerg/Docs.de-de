---
uid: web-forms/overview/ajax-control-toolkit/animation/executing-animations-using-client-side-code-vb
title: "Ausführen von Animationen mit clientseitigem Code (VB) | Microsoft Docs"
author: wenz
description: "Animation-Steuerelement in ASP.NET AJAX-Steuerelement-Toolkit ist nicht nur ein Steuerelement, aber eine gesamte Framework Animationen an ein Steuerelement hinzufügen. Die Ausführung der Animation..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: f7073f50-d765-456d-9957-926ce60f35f6
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/executing-animations-using-client-side-code-vb
msc.type: authoredcontent
ms.openlocfilehash: c97ce87addd566ed1ba63ccdb81206c6449f49a2
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="executing-animations-using-client-side-code-vb"></a>Ausführen von Animationen mit clientseitigem Code (VB)
====================
durch [Christian Wenz](https://github.com/wenz)

[Herunterladen von Code](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation10.vb.zip) oder [PDF herunterladen](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation10VB.pdf)

> Animation-Steuerelement in ASP.NET AJAX-Steuerelement-Toolkit ist nicht nur ein Steuerelement, aber eine gesamte Framework Animationen an ein Steuerelement hinzufügen. Die Ausführung der Animation kann auch mit benutzerdefinierten clientseitigen JavaScript-Code ausgelöst werden.


## <a name="overview"></a>Übersicht

Animation-Steuerelement in ASP.NET AJAX-Steuerelement-Toolkit ist nicht nur ein Steuerelement, aber eine gesamte Framework Animationen an ein Steuerelement hinzufügen. Die Ausführung der Animation kann auch mit benutzerdefinierten clientseitigen JavaScript-Code ausgelöst werden.

## <a name="steps"></a>Schritte

Erstens sind die `ScriptManager` auf der Seite; klicken Sie dann die ASP.NET AJAX-Bibliothek geladen ist, wodurch das Steuerelement-Toolkit verwenden:

[!code-aspx[Main](executing-animations-using-client-side-code-vb/samples/sample1.aspx)]

Die Animation wird auf einen Bereich des Texts angewendet werden, die wie folgt aussieht:

[!code-aspx[Main](executing-animations-using-client-side-code-vb/samples/sample2.aspx)]

Definieren Sie in der zugehörigen CSS-Klasse für den Bereich eine gute Hintergrundfarbe und auch festlegen Sie eine feste Breite für den Bereich:

[!code-css[Main](executing-animations-using-client-side-code-vb/samples/sample3.css)]

Fügen Sie dann die `AnimationExtender` auf der Seite "Bereitstellen einer `ID`, die `TargetControlID` Attribut und der Auswahlparameter `runat="server"`:

[!code-aspx[Main](executing-animations-using-client-side-code-vb/samples/sample4.aspx)]

Innerhalb der `<Animations>` Knoten verwenden `<OnClick>` klickt Sie auf den Animationen an die Benutzer nur einmalig ausgeführt im Bereich. Fügen Sie zwei Animationen parallelly ausgeführt werden:

[!code-xml[Main](executing-animations-using-client-side-code-vb/samples/sample5.xml)]

Aus Gründen der Demo werden dieser Animation (und anderen Animation, die mit dem Steuerelement Toolkit erstellt) ausgeführt mithilfe von JavaScript-Code, sobald die Seite ausgeführt wird. Erstens wir benötigen Zugriff auf die `AnimationExtender` Steuerelement. Die ASP.NET AJAX-Bibliothek stellt die `$find()` Funktion für diese Aufgabe:

[!code-csharp[Main](executing-animations-using-client-side-code-vb/samples/sample6.cs)]

Die `AnimationExtender` Steuerelement macht eine umfangreiche API, einschließlich der Methoden, mit dem Namen identisch ist, an die Ereignishandler in der XML-Markup verwendet: `OnClick()`, `OnLoad()`und so weiter. Z. B. einen Aufruf von der `OnClick()` Methode ausgeführt wird, die Animation innerhalb der `<OnClick>` Element von der `AnimationExtender` Steuerelement:

[!code-javascript[Main](executing-animations-using-client-side-code-vb/samples/sample7.js)]

Hier wird die vollständige clientseitigen JavaScript-Code, der dem Klicken auf den Bereich emuliert, nachdem die Seite vollständig geladen wurde Beachten Sie, dass die `pageLoad()` Funktionsname wird verwendet, der von ASP.NET AJAX einmal die Seite aufgerufen wird und alle enthaltenen JavaScript-Bibliotheken wurden geladen.

[!code-html[Main](executing-animations-using-client-side-code-vb/samples/sample8.html)]


[![Die Animation wird sofort ohne einen Mausklick ausgeführt.](executing-animations-using-client-side-code-vb/_static/image2.png)](executing-animations-using-client-side-code-vb/_static/image1.png)

Die Animation wird sofort ausgeführt, ohne einen Mausklick ([klicken Sie hier, um das Bild in voller Größe angezeigt](executing-animations-using-client-side-code-vb/_static/image3.png))

>[!div class="step-by-step"]
[Zurück](modifying-animations-from-the-server-side-vb.md)
[Weiter](changing-an-animation-using-client-side-code-vb.md)
