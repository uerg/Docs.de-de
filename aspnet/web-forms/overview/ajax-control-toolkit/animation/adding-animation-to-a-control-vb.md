---
uid: web-forms/overview/ajax-control-toolkit/animation/adding-animation-to-a-control-vb
title: "Hinzufügen von Animationen zu einem Steuerelement (VB) | Microsoft Docs"
author: wenz
description: "Animation-Steuerelement in ASP.NET AJAX-Steuerelement-Toolkit ist nicht nur ein Steuerelement, aber eine gesamte Framework Animationen an ein Steuerelement hinzufügen. In diesem Lernprogramm wird gezeigt, wie..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: c120187e-963e-4439-bb85-32771bc7f1f4
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/adding-animation-to-a-control-vb
msc.type: authoredcontent
ms.openlocfilehash: c2d6971ade89405245c8d23cafb6fd8bb9468639
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="adding-animation-to-a-control-vb"></a>Hinzufügen von Animationen zu einem Steuerelement (VB)
====================
durch [Christian Wenz](https://github.com/wenz)

[Herunterladen von Code](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation1.vb.zip) oder [PDF herunterladen](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation1VB.pdf)

> Animation-Steuerelement in ASP.NET AJAX-Steuerelement-Toolkit ist nicht nur ein Steuerelement, aber eine gesamte Framework Animationen an ein Steuerelement hinzufügen. Dieses Lernprogramm zeigt, wie eine solche Animation einrichten.


## <a name="overview"></a>Übersicht

Animation-Steuerelement in ASP.NET AJAX-Steuerelement-Toolkit ist nicht nur ein Steuerelement, aber eine gesamte Framework Animationen an ein Steuerelement hinzufügen. Dieses Lernprogramm zeigt, wie eine solche Animation einrichten.

## <a name="steps"></a>Schritte

Der erste Schritt besteht wie gewohnt enthalten die `ScriptManager` auf der Seite, damit die ASP.NET AJAX-Bibliothek geladen wird und die Control-Toolkit verwendet werden können:

[!code-aspx[Main](adding-animation-to-a-control-vb/samples/sample1.aspx)]

Die Animation in diesem Szenario wird auf einen Bereich des Texts angewendet werden, die wie folgt aussieht:

[!code-aspx[Main](adding-animation-to-a-control-vb/samples/sample2.aspx)]

Die zugeordnete CSS-Klasse für den Bereich definiert eine Hintergrundfarbe und eine Breitenangabe:

[!code-css[Main](adding-animation-to-a-control-vb/samples/sample3.css)]

Wir als Nächstes, müssen die `AnimationExtender`. Nach der Bereitstellung einer `ID` und die üblichen `runat="server"`, die `TargetControlID` Attribut muss an das Steuerelement zu animierende in unserem Fall Bereich festgelegt werden:

[!code-aspx[Main](adding-animation-to-a-control-vb/samples/sample4.aspx)]

Der gesamte deklarativ Anwendung mithilfe einer XML-Syntax, leider zurzeit nicht vollständig von Visual Studio IntelliSense unterstützt. Der Stammknoten entspricht `<Animations>;` innerhalb dieses Knotens sind mehrere Ereignisse zulässig, wenn Animationen Stelle take(s) bestimmen:

- `OnClick`(Mausklick)
- `OnHoverOut`(wenn die Maus für ein Steuerelement verlässt)
- `OnHoverOver`(wenn die Maus über ein Steuerelement bewegt wird, beenden die `OnHoverOut` Animation)
- `OnLoad`(wenn die Seite geladen wurde)
- `OnMouseOut`(wenn die Maus für ein Steuerelement verlässt)
- `OnMouseOver`(wenn die Maus über ein Steuerelement bewegt wird, beenden Sie nicht die `OnMouseOut` Animation)

Das Framework enthält eine Reihe von Animationen, jeweils durch eine eigene XML-Element dargestellt wird. Hier ist eine Auswahl aus:

- `<Color>`(ändern eine Farbe)
- `<FadeIn>`(Ausblenden in)
- `<FadeOut>`(ausblenden)
- `<Property>`(Ändern eines Steuerelements-Eigenschaft)
- `<Pulse>`(pulsating)
- `<Resize>`(Ändern der Größe)
- `<Scale>`(proportional ändern der Größe)

In diesem Beispiel wird der Bereich ausgeblendet. Die Animation treffen 1,5 Sekunden (`Duration` Attribut), Anzeigen von 24 Frames (Animationsschritte) pro Sekunde (`Fps` Attributs). Hier wird das vollständige Markup für die `AnimationExtender` Steuerelement:

[!code-aspx[Main](adding-animation-to-a-control-vb/samples/sample5.aspx)]

Wenn Sie dieses Skript ausführen, wird im Bereich angezeigt, und in 1,5 Sekunden ausgeblendet.


[![Das Panel ausblenden](adding-animation-to-a-control-vb/_static/image2.png)](adding-animation-to-a-control-vb/_static/image1.png)

Das Panel ausblenden ([klicken Sie hier, um das Bild in voller Größe angezeigt](adding-animation-to-a-control-vb/_static/image3.png))

>[!div class="step-by-step"]
[Zurück](dynamically-controlling-updatepanel-animations-cs.md)
[Weiter](executing-several-animations-at-the-same-time-vb.md)
