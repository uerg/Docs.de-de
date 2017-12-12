---
uid: web-forms/overview/ajax-control-toolkit/animation/executing-several-animations-after-each-other-cs
title: "Ausführen mehrerer Animationen nach anderen (c#) | Microsoft Docs"
author: wenz
description: "Animation-Steuerelement in ASP.NET AJAX-Steuerelement-Toolkit ist nicht nur ein Steuerelement, aber eine gesamte Framework Animationen an ein Steuerelement hinzufügen. Sie können durch Fallenlassen ausführen..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 7dc02b18-2b5d-4844-b7c5-cbd818477163
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/executing-several-animations-after-each-other-cs
msc.type: authoredcontent
ms.openlocfilehash: 9d4322690132fe3829e3454f0aa7ff38acd8eb04
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="executing-several-animations-after-each-other-c"></a>Ausführen mehrerer Animationen nach anderen (c#)
====================
durch [Christian Wenz](https://github.com/wenz)

[Herunterladen von Code](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation3.cs.zip) oder [PDF herunterladen](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation3CS.pdf)

> Animation-Steuerelement in ASP.NET AJAX-Steuerelement-Toolkit ist nicht nur ein Steuerelement, aber eine gesamte Framework Animationen an ein Steuerelement hinzufügen. Es kann mehrere Animationen eine nacheinander ausgeführt.


Animation-Steuerelement in ASP.NET AJAX-Steuerelement-Toolkit ist nicht nur ein Steuerelement, aber eine gesamte Framework Animationen an ein Steuerelement hinzufügen. Es kann mehrere Animationen eine nacheinander ausgeführt.

## <a name="steps"></a>Schritte

Erstens sind die `ScriptManager` auf der Seite; klicken Sie dann die ASP.NET AJAX-Bibliothek geladen ist, wodurch das Steuerelement-Toolkit verwenden:

[!code-aspx[Main](executing-several-animations-after-each-other-cs/samples/sample1.aspx)]

Die Animation wird auf einen Bereich des Texts angewendet werden, die wie folgt aussieht:

[!code-aspx[Main](executing-several-animations-after-each-other-cs/samples/sample2.aspx)]

Definieren Sie in der zugehörigen CSS-Klasse für den Bereich eine gute Hintergrundfarbe und auch festlegen Sie eine feste Breite für den Bereich:

[!code-css[Main](executing-several-animations-after-each-other-cs/samples/sample3.css)]

Fügen Sie dann die `AnimationExtender` auf der Seite "Bereitstellen einer `ID`, die `TargetControlID` Attribut und der Auswahlparameter`runat="server":`

[!code-aspx[Main](executing-several-animations-after-each-other-cs/samples/sample4.aspx)]

Innerhalb der `<Animations>` Knoten verwenden `<OnLoad>` Animationen ausgeführt, nachdem die Seite vollständig geladen wurde. Im allgemeinen `<OnLoad>` akzeptiert nur eine Animation. Die Animation-Framework bietet die Möglichkeit, mehrere Animationen in einer mit join die `<Sequence>` Element. Alle Animationen in `<Sequence>` nacheinander ausgeführt werden. Hier ist die mögliche-Markup für die `AnimationExtender` -Steuerelement, zuerst den größeren Bereich vornehmen und dann seine Höhe zu verkürzen:

[!code-aspx[Main](executing-several-animations-after-each-other-cs/samples/sample5.aspx)]

Wenn Sie dieses Skript im Bereich erste ruft breiter und dann kleinere ausführen.


[![Zuerst wird die Breite erhöht.](executing-several-animations-after-each-other-cs/_static/image2.png)](executing-several-animations-after-each-other-cs/_static/image1.png)

Zuerst wird die Breite erhöht ([klicken Sie hier, um das Bild in voller Größe angezeigt](executing-several-animations-after-each-other-cs/_static/image3.png))


[![Anschließend wird die Höhe verringert.](executing-several-animations-after-each-other-cs/_static/image5.png)](executing-several-animations-after-each-other-cs/_static/image4.png)

Dann die Höhe verringert ([klicken Sie hier, um das Bild in voller Größe angezeigt](executing-several-animations-after-each-other-cs/_static/image6.png))

>[!div class="step-by-step"]
[Zurück](executing-several-animations-at-the-same-time-cs.md)
[Weiter](animation-depending-on-a-condition-cs.md)
