---
uid: web-forms/overview/ajax-control-toolkit/animation/animation-depending-on-a-condition-cs
title: "Animation abhängig von einer Bedingung (c#) | Microsoft Docs"
author: wenz
description: "Animation-Steuerelement in ASP.NET AJAX-Steuerelement-Toolkit ist nicht nur ein Steuerelement, aber eine gesamte Framework Animationen an ein Steuerelement hinzufügen. Gibt an, ob eine Animation ist..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: b7a28c0d-efb9-443a-80a4-1a5ee54671cd
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/animation-depending-on-a-condition-cs
msc.type: authoredcontent
ms.openlocfilehash: 13366a86be01f41e27db1869b93192520190387a
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="animation-depending-on-a-condition-c"></a>Animation abhängig von einer Bedingung (c#)
====================
durch [Christian Wenz](https://github.com/wenz)

[Herunterladen von Code](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation4.cs.zip) oder [PDF herunterladen](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation4CS.pdf)

> Animation-Steuerelement in ASP.NET AJAX-Steuerelement-Toolkit ist nicht nur ein Steuerelement, aber eine gesamte Framework Animationen an ein Steuerelement hinzufügen. Gibt an, ob eine Animation ausgeführt wird, können auch auf eine Bedingung in Form von JavaScript-Code abhängen.


## <a name="overview"></a>Übersicht

Animation-Steuerelement in ASP.NET AJAX-Steuerelement-Toolkit ist nicht nur ein Steuerelement, aber eine gesamte Framework Animationen an ein Steuerelement hinzufügen. Gibt an, ob eine Animation ausgeführt wird, können auch auf eine Bedingung in Form von JavaScript-Code abhängen.

## <a name="steps"></a>Schritte

Erstens sind die `ScriptManager` auf der Seite; klicken Sie dann die ASP.NET AJAX-Bibliothek geladen ist, wodurch das Steuerelement-Toolkit verwenden:

[!code-aspx[Main](animation-depending-on-a-condition-cs/samples/sample1.aspx)]

Die Animation wird auf einen Bereich des Texts angewendet werden, die wie folgt aussieht:

[!code-aspx[Main](animation-depending-on-a-condition-cs/samples/sample2.aspx)]

Definieren Sie in der zugehörigen CSS-Klasse für den Bereich eine gute Hintergrundfarbe und auch festlegen Sie eine feste Breite für den Bereich:

[!code-css[Main](animation-depending-on-a-condition-cs/samples/sample3.css)]

Fügen Sie dann die `AnimationExtender` auf der Seite "Bereitstellen einer `ID`, die `TargetControlID` Attribut und der Auswahlparameter`runat="server":`

[!code-aspx[Main](animation-depending-on-a-condition-cs/samples/sample4.aspx)]

Innerhalb der `<Animations>` Knoten verwenden `<OnLoad>` Animationen ausgeführt, nachdem die Seite vollständig geladen wurde. Statt in einem regulären Animationen der `<Condition>` Element, kommt es zu. Der JavaScript-Code als der Wert der `ConditionScript` Attribut zur Laufzeit ausgeführt wird. Wenn sie auf "true" ausgewertet wird, wird die Animation, andernfalls nicht ausgeführt. Das folgende Markup bietet zwei Animationen, jede von ihnen in 50 % der Fälle nach zufälligen ausgeführt wird. Da kann nur innerhalb einer Animation `<OnLoad>`, die beiden `<Condition>` Animationen verknüpft sind, zusammen mit den `<Sequence>` Element:

[!code-aspx[Main](animation-depending-on-a-condition-cs/samples/sample5.aspx)]

Beachten Sie, dass das kleiner-als-Zeichen (`<`) in der `ConditionScript` Attribut muss (mit Escapezeichen) sein. Wenn Sie dieses Skript, entweder keine Animation ausgeführt wird, ausführen oder einen der beiden keine, oder beide.


[![Das Panel wird ohne Änderung der Größe, ausblenden, damit die zweite Animation-ausgeführt wird, das erste Schema nicht](animation-depending-on-a-condition-cs/_static/image2.png)](animation-depending-on-a-condition-cs/_static/image1.png)

Das Panel wird ausblenden ohne Änderung der Größe, damit die zweite Animation-ausgeführt wird, das erste Schema nicht ([klicken Sie hier, um das Bild in voller Größe angezeigt](animation-depending-on-a-condition-cs/_static/image3.png))

>[!div class="step-by-step"]
[Zurück](executing-several-animations-after-each-other-cs.md)
[Weiter](picking-one-animation-out-of-a-list-cs.md)
