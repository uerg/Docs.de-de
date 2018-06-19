---
uid: web-forms/overview/ajax-control-toolkit/animation/picking-one-animation-out-of-a-list-vb
title: Auswählen einer Animation aus einer (VB) | Microsoft Docs
author: wenz
description: Animation-Steuerelement in ASP.NET AJAX-Steuerelement-Toolkit ist nicht nur ein Steuerelement, aber eine gesamte Framework Animationen an ein Steuerelement hinzufügen. Das Framework auch zulassen...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 81ba9116-d485-40c0-8ff6-7e9ae23e0a0c
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/picking-one-animation-out-of-a-list-vb
msc.type: authoredcontent
ms.openlocfilehash: f2bd1b3cc72595da7e8901786ea8415d7c1c524a
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
ms.locfileid: "30872064"
---
<a name="picking-one-animation-out-of-a-list-vb"></a>Auswählen einer Animation aus einer (VB)
====================
durch [Christian Wenz](https://github.com/wenz)

[Herunterladen von Code](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation5.vb.zip) oder [PDF herunterladen](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation5VB.pdf)

> Animation-Steuerelement in ASP.NET AJAX-Steuerelement-Toolkit ist nicht nur ein Steuerelement, aber eine gesamte Framework Animationen an ein Steuerelement hinzufügen. Das Framework ermöglicht es auch den Programmierer, die eine Animation aus einer Liste der gültigen Animationen, je nach der Auswertung des JavaScript-Code auswählen.


## <a name="overview"></a>Übersicht

Animation-Steuerelement in ASP.NET AJAX-Steuerelement-Toolkit ist nicht nur ein Steuerelement, aber eine gesamte Framework Animationen an ein Steuerelement hinzufügen. Das Framework ermöglicht es auch den Programmierer, die eine Animation aus einer Liste der gültigen Animationen, je nach der Auswertung des JavaScript-Code auswählen.

## <a name="steps"></a>Schritte

Erstens sind die `ScriptManager` auf der Seite; klicken Sie dann die ASP.NET AJAX-Bibliothek geladen ist, wodurch das Steuerelement-Toolkit verwenden:

[!code-aspx[Main](picking-one-animation-out-of-a-list-vb/samples/sample1.aspx)]

Die Animation wird auf einen Bereich des Texts angewendet werden, die wie folgt aussieht:

[!code-aspx[Main](picking-one-animation-out-of-a-list-vb/samples/sample2.aspx)]

Definieren Sie in der zugehörigen CSS-Klasse für den Bereich eine gute Hintergrundfarbe und auch festlegen Sie eine feste Breite für den Bereich:

[!code-css[Main](picking-one-animation-out-of-a-list-vb/samples/sample3.css)]

Fügen Sie dann die `AnimationExtender` auf der Seite "Bereitstellen einer `ID`, die `TargetControlID` Attribut und der Auswahlparameter `runat="server":`

[!code-aspx[Main](picking-one-animation-out-of-a-list-vb/samples/sample4.aspx)]

Innerhalb der `<Animations>` Knoten verwenden `<OnLoad>` Animationen ausgeführt, nachdem die Seite vollständig geladen wurde. Statt in einem regulären Animationen der `<Case>` Element, kommt es zu. Der Wert des Attributs SelectScript ausgewertet wird. der Rückgabewert muss numerische sein. Je nach dieser Anzahl, eines der Subanimation innerhalb &lt;Fall&gt; ausgeführt wird. Z. B. SelectScript 2 ergibt, führt das Steuerelement-Toolkit dritte Animation in &lt;Fall&gt; (beginnend mit 0).

Das folgende Markup definiert drei Subanimation: Ändern der Größe der Breite und Ändern der Größe der Höhe ausblenden. Der JavaScript-Code (`Math.floor(3 * Math.random())`) nimmt dann eine Zahl zwischen 0 und 2, sodass einer der drei Animationen ausgeführt wird:

[!code-aspx[Main](picking-one-animation-out-of-a-list-vb/samples/sample5.aspx)]


[![Einer der drei möglichen Animationen: Bereich ruft breiter](picking-one-animation-out-of-a-list-vb/_static/image2.png)](picking-one-animation-out-of-a-list-vb/_static/image1.png)

Einer der drei möglichen Animationen: Bereich ruft breiter ([klicken Sie hier, um das Bild in voller Größe angezeigt](picking-one-animation-out-of-a-list-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Zurück](animation-depending-on-a-condition-vb.md)
> [Weiter](animating-in-response-to-user-interaction-vb.md)
