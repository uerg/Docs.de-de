---
uid: web-forms/overview/ajax-control-toolkit/animation/adding-animation-to-a-control-vb
title: Hinzufügen von Animationen zu einem Steuerelement (VB) | Microsoft-Dokumentation
author: wenz
description: Die Animation-Steuerelement in ASP.NET AJAX Control Toolkit ist nicht nur ein Steuerelement, aber ein ganzes Framework Animationen an ein Steuerelement hinzufügen. Dieses Tutorial zeigt, wie...
ms.author: aspnetcontent
ms.date: 06/02/2008
ms.assetid: c120187e-963e-4439-bb85-32771bc7f1f4
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/adding-animation-to-a-control-vb
msc.type: authoredcontent
ms.openlocfilehash: 7ae2fd6c680ed89022772c62bb6148808d2f4daf
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2018
ms.locfileid: "37818125"
---
<a name="adding-animation-to-a-control-vb"></a>Hinzufügen von Animationen zu einem Steuerelement (VB)
====================
durch [Christian Wenz](https://github.com/wenz)

[Code herunterladen](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation1.vb.zip) oder [PDF-Datei herunterladen](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation1VB.pdf)

> Die Animation-Steuerelement in ASP.NET AJAX Control Toolkit ist nicht nur ein Steuerelement, aber ein ganzes Framework Animationen an ein Steuerelement hinzufügen. In diesem Tutorial zeigt, wie Sie eine solche Animation einrichten.


## <a name="overview"></a>Übersicht

Die Animation-Steuerelement in ASP.NET AJAX Control Toolkit ist nicht nur ein Steuerelement, aber ein ganzes Framework Animationen an ein Steuerelement hinzufügen. In diesem Tutorial zeigt, wie Sie eine solche Animation einrichten.

## <a name="steps"></a>Schritte

Der erste Schritt ist wie üblich, enthalten die `ScriptManager` auf der Seite, damit die ASP.NET AJAX-Bibliothek geladen wird und das Steuerelement-Toolkit verwendet werden kann:

[!code-aspx[Main](adding-animation-to-a-control-vb/samples/sample1.aspx)]

Die Animation in diesem Szenario wird auf einen Bereich des Texts angewendet werden, der so aussieht:

[!code-aspx[Main](adding-animation-to-a-control-vb/samples/sample2.aspx)]

Die zugeordnete CSS-Klasse für den Bereich definiert eine Hintergrundfarbe und eine Breite:

[!code-css[Main](adding-animation-to-a-control-vb/samples/sample3.css)]

Als Nächstes eingerichtet ist, müssen wir die `AnimationExtender`. Nach der Bereitstellung einer `ID` und die üblichen `runat="server"`, `TargetControlID` Attribut muss auf das Steuerelement zum Animieren in unserem Fall ist der Bereich festgelegt werden:

[!code-aspx[Main](adding-animation-to-a-control-vb/samples/sample4.aspx)]

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

[!code-aspx[Main](adding-animation-to-a-control-vb/samples/sample5.aspx)]

Wenn Sie dieses Skript ausführen, wird der Bereich wird angezeigt, und in 1,5 Sekunden ausgeblendet wird.


[![Der Bereich wird ausgeblendet wird](adding-animation-to-a-control-vb/_static/image2.png)](adding-animation-to-a-control-vb/_static/image1.png)

Der Bereich wird ausgeblendet wird, ([klicken Sie, um das Bild in voller Größe anzeigen](adding-animation-to-a-control-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Zurück](dynamically-controlling-updatepanel-animations-cs.md)
> [Weiter](executing-several-animations-at-the-same-time-vb.md)
