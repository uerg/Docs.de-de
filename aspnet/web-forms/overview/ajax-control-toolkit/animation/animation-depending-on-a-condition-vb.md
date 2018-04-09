---
uid: web-forms/overview/ajax-control-toolkit/animation/animation-depending-on-a-condition-vb
title: Animation abhängig von einer Bedingung (VB) | Microsoft Docs
author: wenz
description: Animation-Steuerelement in ASP.NET AJAX-Steuerelement-Toolkit ist nicht nur ein Steuerelement, aber eine gesamte Framework Animationen an ein Steuerelement hinzufügen. Gibt an, ob eine Animation ist...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 1b87d8d6-b3f7-4126-b51c-d41442fbf947
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/animation-depending-on-a-condition-vb
msc.type: authoredcontent
ms.openlocfilehash: d3a648ff8299c9720e9f34522f271595ab1b9bc9
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
---
<a name="animation-depending-on-a-condition-vb"></a>Animation abhängig von einer Bedingung (VB)
====================
durch [Christian Wenz](https://github.com/wenz)

[Herunterladen von Code](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation4.vb.zip) oder [PDF herunterladen](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation4VB.pdf)

> Animation-Steuerelement in ASP.NET AJAX-Steuerelement-Toolkit ist nicht nur ein Steuerelement, aber eine gesamte Framework Animationen an ein Steuerelement hinzufügen. Gibt an, ob eine Animation ausgeführt wird, können auch auf eine Bedingung in Form von JavaScript-Code abhängen.


## <a name="overview"></a>Übersicht

Animation-Steuerelement in ASP.NET AJAX-Steuerelement-Toolkit ist nicht nur ein Steuerelement, aber eine gesamte Framework Animationen an ein Steuerelement hinzufügen. Gibt an, ob eine Animation ausgeführt wird, können auch auf eine Bedingung in Form von JavaScript-Code abhängen.

## <a name="steps"></a>Schritte

Erstens sind die `ScriptManager` auf der Seite; klicken Sie dann die ASP.NET AJAX-Bibliothek geladen ist, wodurch das Steuerelement-Toolkit verwenden:

[!code-aspx[Main](animation-depending-on-a-condition-vb/samples/sample1.aspx)]

Die Animation wird auf einen Bereich des Texts angewendet werden, die wie folgt aussieht:

[!code-aspx[Main](animation-depending-on-a-condition-vb/samples/sample2.aspx)]

Definieren Sie in der zugehörigen CSS-Klasse für den Bereich eine gute Hintergrundfarbe und auch festlegen Sie eine feste Breite für den Bereich:

[!code-css[Main](animation-depending-on-a-condition-vb/samples/sample3.css)]

Fügen Sie dann die `AnimationExtender` auf der Seite "Bereitstellen einer `ID`, die `TargetControlID` Attribut und der Auswahlparameter `runat="server":`

[!code-aspx[Main](animation-depending-on-a-condition-vb/samples/sample4.aspx)]

Innerhalb der `<Animations>` Knoten verwenden `<OnLoad>` Animationen ausgeführt, nachdem die Seite vollständig geladen wurde. Statt in einem regulären Animationen der `<Condition>` Element, kommt es zu. Der JavaScript-Code als der Wert der `ConditionScript` Attribut zur Laufzeit ausgeführt wird. Wenn sie auf "true" ausgewertet wird, wird die Animation, andernfalls nicht ausgeführt. Das folgende Markup bietet zwei Animationen, jede von ihnen in 50 % der Fälle nach zufälligen ausgeführt wird. Da kann nur innerhalb einer Animation `<OnLoad>`, die beiden `<Condition>` Animationen verknüpft sind, zusammen mit den `<Sequence>` Element:

[!code-aspx[Main](animation-depending-on-a-condition-vb/samples/sample5.aspx)]

Beachten Sie, dass das kleiner-als-Zeichen (`<`) in der `ConditionScript` Attribut muss (mit Escapezeichen) sein. Wenn Sie dieses Skript, entweder keine Animation ausgeführt wird, ausführen oder einen der beiden keine, oder beide.


[![Das Panel wird ohne Änderung der Größe, ausblenden, damit die zweite Animation-ausgeführt wird, das erste Schema nicht](animation-depending-on-a-condition-vb/_static/image2.png)](animation-depending-on-a-condition-vb/_static/image1.png)

Das Panel wird ausblenden ohne Änderung der Größe, damit die zweite Animation-ausgeführt wird, das erste Schema nicht ([klicken Sie hier, um das Bild in voller Größe angezeigt](animation-depending-on-a-condition-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Zurück](executing-several-animations-after-each-other-vb.md)
> [Weiter](picking-one-animation-out-of-a-list-vb.md)
