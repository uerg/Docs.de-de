---
uid: web-forms/overview/ajax-control-toolkit/animation/adding-animation-to-a-control-cs
title: Hinzufügen von Animationen zu einem Steuerelement (c#) | Microsoft-Dokumentation
author: wenz
description: Die Animation-Steuerelement in ASP.NET AJAX Control Toolkit ist nicht nur ein Steuerelement, aber ein ganzes Framework Animationen an ein Steuerelement hinzufügen. Dieses Tutorial zeigt, wie...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 0f1fc1f5-9dbd-44e7-931e-387d42f0342b
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/adding-animation-to-a-control-cs
msc.type: authoredcontent
ms.openlocfilehash: aa977883af931bb74b791104cf4ee3212079e43a
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2018
ms.locfileid: "41833943"
---
<a name="adding-animation-to-a-control-c"></a>Hinzufügen von Animationen zu einem Steuerelement (c#)
====================
durch [Christian Wenz](https://github.com/wenz)

[Code herunterladen](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation1.cs.zip) oder [PDF-Datei herunterladen](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation1CS.pdf)

> Die Animation-Steuerelement in ASP.NET AJAX Control Toolkit ist nicht nur ein Steuerelement, aber ein ganzes Framework Animationen an ein Steuerelement hinzufügen. In diesem Tutorial zeigt, wie Sie eine solche Animation einrichten.


## <a name="overview"></a>Übersicht

Die Animation-Steuerelement in ASP.NET AJAX Control Toolkit ist nicht nur ein Steuerelement, aber ein ganzes Framework Animationen an ein Steuerelement hinzufügen. In diesem Tutorial zeigt, wie Sie eine solche Animation einrichten.

## <a name="steps"></a>Schritte

Der erste Schritt ist wie üblich, enthalten die `ScriptManager` auf der Seite, damit die ASP.NET AJAX-Bibliothek geladen wird und das Steuerelement-Toolkit verwendet werden kann:

[!code-aspx[Main](adding-animation-to-a-control-cs/samples/sample1.aspx)]

Die Animation in diesem Szenario wird auf einen Bereich des Texts angewendet werden, der so aussieht:

[!code-aspx[Main](adding-animation-to-a-control-cs/samples/sample2.aspx)]

Die zugeordnete CSS-Klasse für den Bereich definiert eine Hintergrundfarbe und eine Breite:

[!code-css[Main](adding-animation-to-a-control-cs/samples/sample3.css)]

Als Nächstes eingerichtet ist, müssen wir die `AnimationExtender`. Nach der Bereitstellung einer `ID` und die üblichen `runat="server"`, `TargetControlID` Attribut muss auf das Steuerelement zum Animieren in unserem Fall ist der Bereich festgelegt werden:

[!code-aspx[Main](adding-animation-to-a-control-cs/samples/sample4.aspx)]

Die gesamte deklarativ animiert werden mithilfe einer XML-Syntax, die leider zurzeit nicht vollständig von Visual Studio IntelliSense unterstützt. Der Stammknoten ist `<Animations>;` innerhalb des Knotens, sind mehrere Ereignisse zulässig, die zu bestimmen, wann die Animation(s) Ort take(s):

- `OnClick` (Mausklick)
- `OnHoverOut` (wenn die Maus für ein Steuerelement verlässt)
- `OnHoverOver` (wenn die Maus über ein Steuerelement bewegt wird, beenden die `OnHoverOut` Animation)
- `OnLoad` (wenn die Seite geladen wurde)
- `OnMouseOut` (wenn die Maus für ein Steuerelement verlässt)
- `OnMouseOver` (wenn die Maus über ein Steuerelement bewegt wird, beenden Sie nicht die `OnMouseOut` Animation)

Das Framework bietet einen Satz von Animationen, jeweils durch eine eigene XML-Element dargestellt wird. Hier ist eine Auswahl aus:

- `<Color>` (ändern eine Farbe)
- `<FadeIn>` (in ausblenden)
- `<FadeOut>` (ausblenden)
- `<Property>` (ändern die Eigenschaft eines Steuerelements)
- `<Pulse>` (pulsating)
- `<Resize>` (Ändern der Größe)
- `<Scale>` (proportional die Größe ändern)

In diesem Beispiel wird der Bereich ausblenden. Die Animation treffen 1,5 Sekunden (`Duration` Attribut), Anzeige (Animationsschritte) zur 24 Bildern pro Sekunde (`Fps` Attributs). Hier ist das vollständige Markup für die `AnimationExtender` Steuerelement:

[!code-aspx[Main](adding-animation-to-a-control-cs/samples/sample5.aspx)]

Wenn Sie dieses Skript ausführen, wird der Bereich wird angezeigt, und in 1,5 Sekunden ausgeblendet wird.


[![Der Bereich wird ausgeblendet wird](adding-animation-to-a-control-cs/_static/image2.png)](adding-animation-to-a-control-cs/_static/image1.png)

Der Bereich wird ausgeblendet wird, ([klicken Sie, um das Bild in voller Größe anzeigen](adding-animation-to-a-control-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Nächste](executing-several-animations-at-the-same-time-cs.md)
