---
uid: web-forms/overview/ajax-control-toolkit/animation/executing-several-animations-after-each-other-vb
title: Ausführen mehrerer Animationen nacheinander (VB) | Microsoft-Dokumentation
author: wenz
description: Die Animation-Steuerelement in ASP.NET AJAX Control Toolkit ist nicht nur ein Steuerelement, aber ein ganzes Framework Animationen an ein Steuerelement hinzufügen. Er ermöglicht es, durch Fallenlassen ausführen...
ms.author: aspnetcontent
ms.date: 06/02/2008
ms.assetid: 21ece509-79cc-4d9d-892d-7b6e9c4d3502
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/executing-several-animations-after-each-other-vb
msc.type: authoredcontent
ms.openlocfilehash: 3d94619ca40d288f8a5a391ef02103902bd2550c
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2018
ms.locfileid: "37835954"
---
<a name="executing-several-animations-after-each-other-vb"></a>Ausführen mehrerer Animationen nacheinander (VB)
====================
durch [Christian Wenz](https://github.com/wenz)

[Code herunterladen](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation3.vb.zip) oder [PDF-Datei herunterladen](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation3VB.pdf)

> Die Animation-Steuerelement in ASP.NET AJAX Control Toolkit ist nicht nur ein Steuerelement, aber ein ganzes Framework Animationen an ein Steuerelement hinzufügen. Sie können zum Ausführen mehrerer Animationen eine nach dem anderen.


## <a name="overview"></a>Übersicht

Die Animation-Steuerelement in ASP.NET AJAX Control Toolkit ist nicht nur ein Steuerelement, aber ein ganzes Framework Animationen an ein Steuerelement hinzufügen. Sie können zum Ausführen mehrerer Animationen eine nach dem anderen.

## <a name="steps"></a>Schritte

Zunächst einmal sind die `ScriptManager` in die Seite klicken Sie dann die ASP.NET AJAX-Bibliothek wird geladen, lässt sich das Steuerelement-Toolkit verwenden:

[!code-aspx[Main](executing-several-animations-after-each-other-vb/samples/sample1.aspx)]

Die Animation wird auf einen Bereich des Texts angewendet werden, der so aussieht:

[!code-aspx[Main](executing-several-animations-after-each-other-vb/samples/sample2.aspx)]

Definieren Sie in der zugehörigen CSS-Klasse für den Bereich eine gute Hintergrundfarbe aus, und auch festlegen Sie eine feste Breite für den Bereich:

[!code-css[Main](executing-several-animations-after-each-other-vb/samples/sample3.css)]

Fügen Sie dann die `AnimationExtender` auf der Seite Bereitstellen einer `ID`, `TargetControlID` -Attribut und das obligatorische `runat="server":`

[!code-aspx[Main](executing-several-animations-after-each-other-vb/samples/sample4.aspx)]

In der `<Animations>` Knoten verwenden `<OnLoad>` die Animationen ausgeführt werden, nachdem die Seite vollständig geladen wurde. Im allgemeinen `<OnLoad>` akzeptiert nur eine Animation. Der Animation-Framework können Sie mehrere Animationen in einer mit join die `<Sequence>` Element. Alle Animationen in `<Sequence>` sind, werden nach dem anderen. Hier ist die einem möglichen Markup für die `AnimationExtender` Steuerelement, sodass zuerst den größeren Bereich und dann Verringern der Höhe:

[!code-aspx[Main](executing-several-animations-after-each-other-vb/samples/sample5.aspx)]

Wenn Sie dieses Skript im Bereich erste ruft breiter und dann kleinere ausführen.


[![Zuerst wird die Breite erhöht.](executing-several-animations-after-each-other-vb/_static/image2.png)](executing-several-animations-after-each-other-vb/_static/image1.png)

Zuerst wird die Breite erhöht ([klicken Sie, um das Bild in voller Größe anzeigen](executing-several-animations-after-each-other-vb/_static/image3.png))


[![Anschließend wird die Höhe verringert.](executing-several-animations-after-each-other-vb/_static/image5.png)](executing-several-animations-after-each-other-vb/_static/image4.png)

Und dann die Höhe verringert wird ([klicken Sie, um das Bild in voller Größe anzeigen](executing-several-animations-after-each-other-vb/_static/image6.png))

> [!div class="step-by-step"]
> [Zurück](executing-several-animations-at-the-same-time-vb.md)
> [Weiter](animation-depending-on-a-condition-vb.md)
