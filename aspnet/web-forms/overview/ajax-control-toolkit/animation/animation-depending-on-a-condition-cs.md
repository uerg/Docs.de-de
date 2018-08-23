---
uid: web-forms/overview/ajax-control-toolkit/animation/animation-depending-on-a-condition-cs
title: Animation Abhängigkeit einer Bedingung (c#) | Microsoft-Dokumentation
author: wenz
description: Die Animation-Steuerelement in ASP.NET AJAX Control Toolkit ist nicht nur ein Steuerelement, aber ein ganzes Framework Animationen an ein Steuerelement hinzufügen. Gibt an, ob eine Animation zu ist...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: b7a28c0d-efb9-443a-80a4-1a5ee54671cd
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/animation-depending-on-a-condition-cs
msc.type: authoredcontent
ms.openlocfilehash: e4705b6c590f153043082759f1269c8f2d927abe
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2018
ms.locfileid: "41832344"
---
<a name="animation-depending-on-a-condition-c"></a>Animation Abhängigkeit einer Bedingung (c#)
====================
durch [Christian Wenz](https://github.com/wenz)

[Code herunterladen](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation4.cs.zip) oder [PDF-Datei herunterladen](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation4CS.pdf)

> Die Animation-Steuerelement in ASP.NET AJAX Control Toolkit ist nicht nur ein Steuerelement, aber ein ganzes Framework Animationen an ein Steuerelement hinzufügen. Gibt an, ob eine Animation ausgeführt wird oder nicht, kann auch auf einer Bedingung in Form von JavaScript-Code abhängen.


## <a name="overview"></a>Übersicht

Die Animation-Steuerelement in ASP.NET AJAX Control Toolkit ist nicht nur ein Steuerelement, aber ein ganzes Framework Animationen an ein Steuerelement hinzufügen. Gibt an, ob eine Animation ausgeführt wird oder nicht, kann auch auf einer Bedingung in Form von JavaScript-Code abhängen.

## <a name="steps"></a>Schritte

Zunächst einmal sind die `ScriptManager` in die Seite klicken Sie dann die ASP.NET AJAX-Bibliothek wird geladen, lässt sich das Steuerelement-Toolkit verwenden:

[!code-aspx[Main](animation-depending-on-a-condition-cs/samples/sample1.aspx)]

Die Animation wird auf einen Bereich des Texts angewendet werden, der so aussieht:

[!code-aspx[Main](animation-depending-on-a-condition-cs/samples/sample2.aspx)]

Definieren Sie in der zugehörigen CSS-Klasse für den Bereich eine gute Hintergrundfarbe aus, und auch festlegen Sie eine feste Breite für den Bereich:

[!code-css[Main](animation-depending-on-a-condition-cs/samples/sample3.css)]

Fügen Sie dann die `AnimationExtender` auf der Seite Bereitstellen einer `ID`, `TargetControlID` -Attribut und das obligatorische `runat="server":`

[!code-aspx[Main](animation-depending-on-a-condition-cs/samples/sample4.aspx)]

In der `<Animations>` Knoten verwenden `<OnLoad>` die Animationen ausgeführt werden, nachdem die Seite vollständig geladen wurde. Statt in einem regulären-Animationen die `<Condition>` Element ins Spiel. Der JavaScript-Code bereitgestellt, die als Wert für die `ConditionScript` Attribut zur Laufzeit ausgeführt wird. Wenn sie auf "true" ausgewertet wird, wird die Animation, andernfalls nicht ausgeführt. Das folgende Markup stellt zwei Animationen, die jeweils 50 % der Fälle, bei der zufälligen ausgeführt wird. Da es möglicherweise nur eine Animation in `<OnLoad>`, die beiden `<Condition>` Animationen angehören, mithilfe der `<Sequence>` Element:

[!code-aspx[Main](animation-depending-on-a-condition-cs/samples/sample5.aspx)]

Beachten Sie, dass das kleiner-als-Zeichen (`<`) in der `ConditionScript` -Attribut muss mit Escapezeichen (-) sein. Wenn Sie führen dieses Skript entweder keine Animation ausgeführt wird, ist einer der beiden oder beides.


[![Der Bereich wird ohne Änderung der Größe, ausgeblendet wird, damit nicht die zweite Animation-ausgeführt wird, die erste Bedingung](animation-depending-on-a-condition-cs/_static/image2.png)](animation-depending-on-a-condition-cs/_static/image1.png)

Der Bereich wird ausgeblendet wird, ohne Änderung der Größe, damit die zweite Animation-ausgeführt wird, die erste Bedingung nicht ([klicken Sie, um das Bild in voller Größe anzeigen](animation-depending-on-a-condition-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Zurück](executing-several-animations-after-each-other-cs.md)
> [Weiter](picking-one-animation-out-of-a-list-cs.md)
