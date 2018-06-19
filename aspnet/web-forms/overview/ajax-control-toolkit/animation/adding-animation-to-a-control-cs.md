---
uid: web-forms/overview/ajax-control-toolkit/animation/adding-animation-to-a-control-cs
title: Hinzufügen von Animationen zu einem Steuerelement (c#) | Microsoft Docs
author: wenz
description: Animation-Steuerelement in ASP.NET AJAX-Steuerelement-Toolkit ist nicht nur ein Steuerelement, aber eine gesamte Framework Animationen an ein Steuerelement hinzufügen. In diesem Lernprogramm wird gezeigt, wie...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 0f1fc1f5-9dbd-44e7-931e-387d42f0342b
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/adding-animation-to-a-control-cs
msc.type: authoredcontent
ms.openlocfilehash: ba122660045c3f5dd4b11f118df174a79de814a1
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
ms.locfileid: "30872120"
---
<a name="adding-animation-to-a-control-c"></a>Hinzufügen von Animationen zu einem Steuerelement (c#)
====================
durch [Christian Wenz](https://github.com/wenz)

[Herunterladen von Code](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation1.cs.zip) oder [PDF herunterladen](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation1CS.pdf)

> Animation-Steuerelement in ASP.NET AJAX-Steuerelement-Toolkit ist nicht nur ein Steuerelement, aber eine gesamte Framework Animationen an ein Steuerelement hinzufügen. Dieses Lernprogramm zeigt, wie eine solche Animation einrichten.


## <a name="overview"></a>Übersicht

Animation-Steuerelement in ASP.NET AJAX-Steuerelement-Toolkit ist nicht nur ein Steuerelement, aber eine gesamte Framework Animationen an ein Steuerelement hinzufügen. Dieses Lernprogramm zeigt, wie eine solche Animation einrichten.

## <a name="steps"></a>Schritte

Der erste Schritt besteht wie gewohnt enthalten die `ScriptManager` auf der Seite, damit die ASP.NET AJAX-Bibliothek geladen wird und die Control-Toolkit verwendet werden können:

[!code-aspx[Main](adding-animation-to-a-control-cs/samples/sample1.aspx)]

Die Animation in diesem Szenario wird auf einen Bereich des Texts angewendet werden, die wie folgt aussieht:

[!code-aspx[Main](adding-animation-to-a-control-cs/samples/sample2.aspx)]

Die zugeordnete CSS-Klasse für den Bereich definiert eine Hintergrundfarbe und eine Breitenangabe:

[!code-css[Main](adding-animation-to-a-control-cs/samples/sample3.css)]

Wir als Nächstes, müssen die `AnimationExtender`. Nach der Bereitstellung einer `ID` und die üblichen `runat="server"`, die `TargetControlID` Attribut muss an das Steuerelement zu animierende in unserem Fall Bereich festgelegt werden:

[!code-aspx[Main](adding-animation-to-a-control-cs/samples/sample4.aspx)]

Der gesamte deklarativ Anwendung mithilfe einer XML-Syntax, leider zurzeit nicht vollständig von Visual Studio IntelliSense unterstützt. Der Stammknoten entspricht `<Animations>;` innerhalb dieses Knotens sind mehrere Ereignisse zulässig, wenn Animationen Stelle take(s) bestimmen:

- `OnClick` (Mausklick)
- `OnHoverOut` (wenn die Maus für ein Steuerelement verlässt)
- `OnHoverOver` (wenn die Maus über ein Steuerelement bewegt wird, beenden die `OnHoverOut` Animation)
- `OnLoad` (wenn die Seite geladen wurde)
- `OnMouseOut` (wenn die Maus für ein Steuerelement verlässt)
- `OnMouseOver` (wenn die Maus über ein Steuerelement bewegt wird, beenden Sie nicht die `OnMouseOut` Animation)

Das Framework enthält eine Reihe von Animationen, jeweils durch eine eigene XML-Element dargestellt wird. Hier ist eine Auswahl aus:

- `<Color>` (ändern eine Farbe)
- `<FadeIn>` (Ausblenden in)
- `<FadeOut>` (ausblenden)
- `<Property>` (Ändern eines Steuerelements-Eigenschaft)
- `<Pulse>` (pulsating)
- `<Resize>` (Ändern der Größe)
- `<Scale>` (proportional ändern der Größe)

In diesem Beispiel wird der Bereich ausgeblendet. Die Animation treffen 1,5 Sekunden (`Duration` Attribut), Anzeigen von 24 Frames (Animationsschritte) pro Sekunde (`Fps` Attributs). Hier wird das vollständige Markup für die `AnimationExtender` Steuerelement:

[!code-aspx[Main](adding-animation-to-a-control-cs/samples/sample5.aspx)]

Wenn Sie dieses Skript ausführen, wird im Bereich angezeigt, und in 1,5 Sekunden ausgeblendet.


[![Das Panel ausblenden](adding-animation-to-a-control-cs/_static/image2.png)](adding-animation-to-a-control-cs/_static/image1.png)

Das Panel ausblenden ([klicken Sie hier, um das Bild in voller Größe angezeigt](adding-animation-to-a-control-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Nächste](executing-several-animations-at-the-same-time-cs.md)
