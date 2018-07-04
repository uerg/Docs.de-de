---
uid: web-forms/overview/ajax-control-toolkit/animation/animation-depending-on-a-condition-vb
title: Animation Abhängigkeit einer Bedingung (VB) | Microsoft-Dokumentation
author: wenz
description: Die Animation-Steuerelement in ASP.NET AJAX Control Toolkit ist nicht nur ein Steuerelement, aber ein ganzes Framework Animationen an ein Steuerelement hinzufügen. Gibt an, ob eine Animation zu ist...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 1b87d8d6-b3f7-4126-b51c-d41442fbf947
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/animation-depending-on-a-condition-vb
msc.type: authoredcontent
ms.openlocfilehash: 160b53519382c215db039fd6297bb907688b81c1
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/03/2018
ms.locfileid: "37368556"
---
<a name="animation-depending-on-a-condition-vb"></a>Animation Abhängigkeit einer Bedingung (VB)
====================
durch [Christian Wenz](https://github.com/wenz)

[Code herunterladen](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation4.vb.zip) oder [PDF-Datei herunterladen](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation4VB.pdf)

> Die Animation-Steuerelement in ASP.NET AJAX Control Toolkit ist nicht nur ein Steuerelement, aber ein ganzes Framework Animationen an ein Steuerelement hinzufügen. Gibt an, ob eine Animation ausgeführt wird oder nicht, kann auch auf einer Bedingung in Form von JavaScript-Code abhängen.


## <a name="overview"></a>Übersicht

Die Animation-Steuerelement in ASP.NET AJAX Control Toolkit ist nicht nur ein Steuerelement, aber ein ganzes Framework Animationen an ein Steuerelement hinzufügen. Gibt an, ob eine Animation ausgeführt wird oder nicht, kann auch auf einer Bedingung in Form von JavaScript-Code abhängen.

## <a name="steps"></a>Schritte

Zunächst einmal sind die `ScriptManager` in die Seite klicken Sie dann die ASP.NET AJAX-Bibliothek wird geladen, lässt sich das Steuerelement-Toolkit verwenden:

[!code-aspx[Main](animation-depending-on-a-condition-vb/samples/sample1.aspx)]

Die Animation wird auf einen Bereich des Texts angewendet werden, der so aussieht:

[!code-aspx[Main](animation-depending-on-a-condition-vb/samples/sample2.aspx)]

Definieren Sie in der zugehörigen CSS-Klasse für den Bereich eine gute Hintergrundfarbe aus, und auch festlegen Sie eine feste Breite für den Bereich:

[!code-css[Main](animation-depending-on-a-condition-vb/samples/sample3.css)]

Fügen Sie dann die `AnimationExtender` auf der Seite Bereitstellen einer `ID`, `TargetControlID` -Attribut und das obligatorische `runat="server":`

[!code-aspx[Main](animation-depending-on-a-condition-vb/samples/sample4.aspx)]

In der `<Animations>` Knoten verwenden `<OnLoad>` die Animationen ausgeführt werden, nachdem die Seite vollständig geladen wurde. Statt in einem regulären-Animationen die `<Condition>` Element ins Spiel. Der JavaScript-Code bereitgestellt, die als Wert für die `ConditionScript` Attribut zur Laufzeit ausgeführt wird. Wenn sie auf "true" ausgewertet wird, wird die Animation, andernfalls nicht ausgeführt. Das folgende Markup stellt zwei Animationen, die jeweils 50 % der Fälle, bei der zufälligen ausgeführt wird. Da es möglicherweise nur eine Animation in `<OnLoad>`, die beiden `<Condition>` Animationen angehören, mithilfe der `<Sequence>` Element:

[!code-aspx[Main](animation-depending-on-a-condition-vb/samples/sample5.aspx)]

Beachten Sie, dass das kleiner-als-Zeichen (`<`) in der `ConditionScript` -Attribut muss mit Escapezeichen (-) sein. Wenn Sie führen dieses Skript entweder keine Animation ausgeführt wird, ist einer der beiden oder beides.


[![Der Bereich wird ohne Änderung der Größe, ausgeblendet wird, damit nicht die zweite Animation-ausgeführt wird, die erste Bedingung](animation-depending-on-a-condition-vb/_static/image2.png)](animation-depending-on-a-condition-vb/_static/image1.png)

Der Bereich wird ausgeblendet wird, ohne Änderung der Größe, damit die zweite Animation-ausgeführt wird, die erste Bedingung nicht ([klicken Sie, um das Bild in voller Größe anzeigen](animation-depending-on-a-condition-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Zurück](executing-several-animations-after-each-other-vb.md)
> [Weiter](picking-one-animation-out-of-a-list-vb.md)
