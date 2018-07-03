---
uid: web-forms/overview/ajax-control-toolkit/animation/triggering-an-animation-in-another-control-cs
title: Auslösen einer Animation in einem anderen Steuerelement (c#) | Microsoft-Dokumentation
author: wenz
description: Die Animation-Steuerelement in ASP.NET AJAX Control Toolkit ist nicht nur ein Steuerelement, aber ein ganzes Framework Animationen an ein Steuerelement hinzufügen. Starten Sie in der Regel ein...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: e5d99c2b-d8ee-413c-80d5-c120cffb0a4c
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/triggering-an-animation-in-another-control-cs
msc.type: authoredcontent
ms.openlocfilehash: 37473217062b48a1e7829efcf07015be12e0c650
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/03/2018
ms.locfileid: "37375118"
---
<a name="triggering-an-animation-in-another-control-c"></a>Auslösen einer Animation in einem anderen Steuerelement (c#)
====================
durch [Christian Wenz](https://github.com/wenz)

[Code herunterladen](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation8.cs.zip) oder [PDF-Datei herunterladen](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation8CS.pdf)

> Die Animation-Steuerelement in ASP.NET AJAX Control Toolkit ist nicht nur ein Steuerelement, aber ein ganzes Framework Animationen an ein Steuerelement hinzufügen. Starten eine Animation wird im Allgemeinen durch eine Benutzerinteraktion mit dem gleichen Steuerelement ausgelöst. Es ist jedoch auch möglich, ein Steuerelement, und klicken Sie dann Animation ein anderes Steuerelement interagieren.


## <a name="overview"></a>Übersicht

Die Animation-Steuerelement in ASP.NET AJAX Control Toolkit ist nicht nur ein Steuerelement, aber ein ganzes Framework Animationen an ein Steuerelement hinzufügen. Starten eine Animation wird im Allgemeinen durch eine Benutzerinteraktion mit dem gleichen Steuerelement ausgelöst. Es ist jedoch auch möglich, ein Steuerelement, und klicken Sie dann Animation ein anderes Steuerelement interagieren.

## <a name="steps"></a>Schritte

Zunächst einmal sind die `ScriptManager` in die Seite klicken Sie dann die ASP.NET AJAX-Bibliothek wird geladen, lässt sich das Steuerelement-Toolkit verwenden:

[!code-aspx[Main](triggering-an-animation-in-another-control-cs/samples/sample1.aspx)]

Die Animation wird auf einen Bereich des Texts angewendet werden, der so aussieht:

[!code-aspx[Main](triggering-an-animation-in-another-control-cs/samples/sample2.aspx)]

Definieren Sie in der zugehörigen CSS-Klasse für den Bereich eine gute Hintergrundfarbe aus, und auch festlegen Sie eine feste Breite für den Bereich:

[!code-css[Main](triggering-an-animation-in-another-control-cs/samples/sample3.css)]

Um das Panel Animation zu starten, wird eine HTML-Schaltfläche verwendet. Beachten Sie, dass `<input type="button" />` ist über begünstigt `<asp:Button />` da wir einen Postback nicht soll, wenn der Benutzer auf diese Schaltfläche klickt.

[!code-aspx[Main](triggering-an-animation-in-another-control-cs/samples/sample4.aspx)]

Fügen Sie dann die `AnimationExtender` auf der Seite Bereitstellen einer `ID`, `TargetControlID` -Attribut und das obligatorische `runat="server"`. Es ist wichtig, legen Sie `TargetControlID` auf die ID der Schaltfläche (das Auslösen der Animation-Element), nicht auf die ID des Bereichs (Element der zu animierenden)

[!code-aspx[Main](triggering-an-animation-in-another-control-cs/samples/sample5.aspx)]

In der `<Animations>` Knoten Ort Animationen wie gewohnt. Damit sie im Bereich geändert werden können, legen Sie nicht die Schaltfläche, die `AnimationTarget` -Attribut für jedes Animationselement in `AnimationExtender`. Der Wert für `AnimationTarget` ist die ID des Bereichs, natürlich. Auf diese Weise Fall die Animationen im Bereich nicht mit der Schaltfläche mit den auslösenden sein. Hier ist die `AnimationExtender` Markup für dieses Szenario:

[!code-aspx[Main](triggering-an-animation-in-another-control-cs/samples/sample6.aspx)]

Beachten Sie die spezielle Reihenfolge, in der die einzelnen Animationen angezeigt werden. Als Erstes ruft die Schaltfläche deaktiviert, nachdem die Animation ausgeführt wird. Da gibt es keine `AnimationTarget` -Attribut in der `<EnableAction>` Element dieser Animation wird angewendet, auf das ursprüngliche Steuerelement: die Schaltfläche. Die Animationsschritte für die nächsten beiden parallelly erfolgt (`<Parallel>` Element). Beide verfügen über ihre `AnimationTarget` Attribute festgelegt werden, um `"Panel1"`, animieren daher im Bereich nicht auf die Schaltfläche.


[![Startet die Animation Bereich, klicken mit der Maus auf die Schaltfläche](triggering-an-animation-in-another-control-cs/_static/image2.png)](triggering-an-animation-in-another-control-cs/_static/image1.png)

Startet die Animation Bereich, klicken mit der Maus auf die Schaltfläche ([klicken Sie, um das Bild in voller Größe anzeigen](triggering-an-animation-in-another-control-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Zurück](disabling-actions-during-animation-cs.md)
> [Weiter](modifying-animations-from-the-server-side-cs.md)
