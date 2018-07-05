---
uid: web-forms/overview/ajax-control-toolkit/animation/triggering-an-animation-in-another-control-vb
title: Auslösen einer Animation in einem anderen Steuerelement (VB) | Microsoft-Dokumentation
author: wenz
description: Die Animation-Steuerelement in ASP.NET AJAX Control Toolkit ist nicht nur ein Steuerelement, aber ein ganzes Framework Animationen an ein Steuerelement hinzufügen. Starten Sie in der Regel ein...
ms.author: aspnetcontent
ms.date: 06/02/2008
ms.assetid: 25ebaf1f-5a9f-423d-98c7-1d694e93664f
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/triggering-an-animation-in-another-control-vb
msc.type: authoredcontent
ms.openlocfilehash: d9ef7d35e34bd4f2b1433f7fb02d0c48834b2357
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2018
ms.locfileid: "37804891"
---
<a name="triggering-an-animation-in-another-control-vb"></a>Auslösen einer Animation in einem anderen Steuerelement (VB)
====================
durch [Christian Wenz](https://github.com/wenz)

[Code herunterladen](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation8.vb.zip) oder [PDF-Datei herunterladen](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation8VB.pdf)

> Die Animation-Steuerelement in ASP.NET AJAX Control Toolkit ist nicht nur ein Steuerelement, aber ein ganzes Framework Animationen an ein Steuerelement hinzufügen. Starten eine Animation wird im Allgemeinen durch eine Benutzerinteraktion mit dem gleichen Steuerelement ausgelöst. Es ist jedoch auch möglich, ein Steuerelement, und klicken Sie dann Animation ein anderes Steuerelement interagieren.


## <a name="overview"></a>Übersicht

Die Animation-Steuerelement in ASP.NET AJAX Control Toolkit ist nicht nur ein Steuerelement, aber ein ganzes Framework Animationen an ein Steuerelement hinzufügen. Starten eine Animation wird im Allgemeinen durch eine Benutzerinteraktion mit dem gleichen Steuerelement ausgelöst. Es ist jedoch auch möglich, ein Steuerelement, und klicken Sie dann Animation ein anderes Steuerelement interagieren.

## <a name="steps"></a>Schritte

Zunächst einmal sind die `ScriptManager` in die Seite klicken Sie dann die ASP.NET AJAX-Bibliothek wird geladen, lässt sich das Steuerelement-Toolkit verwenden:

[!code-aspx[Main](triggering-an-animation-in-another-control-vb/samples/sample1.aspx)]

Die Animation wird auf einen Bereich des Texts angewendet werden, der so aussieht:

[!code-aspx[Main](triggering-an-animation-in-another-control-vb/samples/sample2.aspx)]

Definieren Sie in der zugehörigen CSS-Klasse für den Bereich eine gute Hintergrundfarbe aus, und auch festlegen Sie eine feste Breite für den Bereich:

[!code-css[Main](triggering-an-animation-in-another-control-vb/samples/sample3.css)]

Um das Panel Animation zu starten, wird eine HTML-Schaltfläche verwendet. Beachten Sie, dass `<input type="button" />` ist über begünstigt `<asp:Button />` da wir einen Postback nicht soll, wenn der Benutzer auf diese Schaltfläche klickt.

[!code-aspx[Main](triggering-an-animation-in-another-control-vb/samples/sample4.aspx)]

Fügen Sie dann die `AnimationExtender` auf der Seite Bereitstellen einer `ID`, `TargetControlID` -Attribut und das obligatorische `runat="server"`. Es ist wichtig, legen Sie `TargetControlID` auf die ID der Schaltfläche (das Auslösen der Animation-Element), nicht auf die ID des Bereichs (Element der zu animierenden)

[!code-aspx[Main](triggering-an-animation-in-another-control-vb/samples/sample5.aspx)]

In der `<Animations>` Knoten Ort Animationen wie gewohnt. Damit sie im Bereich geändert werden können, legen Sie nicht die Schaltfläche, die `AnimationTarget` -Attribut für jedes Animationselement in `AnimationExtender`. Der Wert für `AnimationTarget` ist die ID des Bereichs, natürlich. Auf diese Weise Fall die Animationen im Bereich nicht mit der Schaltfläche mit den auslösenden sein. Hier ist die `AnimationExtender` Markup für dieses Szenario:

[!code-aspx[Main](triggering-an-animation-in-another-control-vb/samples/sample6.aspx)]

Beachten Sie die spezielle Reihenfolge, in der die einzelnen Animationen angezeigt werden. Als Erstes ruft die Schaltfläche deaktiviert, nachdem die Animation ausgeführt wird. Da gibt es keine `AnimationTarget` -Attribut in der `<EnableAction>` Element dieser Animation wird angewendet, auf das ursprüngliche Steuerelement: die Schaltfläche. Die Animationsschritte für die nächsten beiden parallelly erfolgt (`<Parallel>` Element). Beide verfügen über ihre `AnimationTarget` Attribute festgelegt werden, um `"Panel1"`, animieren daher im Bereich nicht auf die Schaltfläche.


[![Startet die Animation Bereich, klicken mit der Maus auf die Schaltfläche](triggering-an-animation-in-another-control-vb/_static/image2.png)](triggering-an-animation-in-another-control-vb/_static/image1.png)

Startet die Animation Bereich, klicken mit der Maus auf die Schaltfläche ([klicken Sie, um das Bild in voller Größe anzeigen](triggering-an-animation-in-another-control-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Zurück](disabling-actions-during-animation-vb.md)
> [Weiter](modifying-animations-from-the-server-side-vb.md)
