---
uid: web-forms/overview/ajax-control-toolkit/animation/triggering-an-animation-in-another-control-cs
title: Auslösen einer Animation in ein anderes Steuerelement (c#) | Microsoft Docs
author: wenz
description: Animation-Steuerelement in ASP.NET AJAX-Steuerelement-Toolkit ist nicht nur ein Steuerelement, aber eine gesamte Framework Animationen an ein Steuerelement hinzufügen. Starten Sie in der Regel ein...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: e5d99c2b-d8ee-413c-80d5-c120cffb0a4c
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/triggering-an-animation-in-another-control-cs
msc.type: authoredcontent
ms.openlocfilehash: e94046ca70607e37c1b5ef57d5cedef67a236b94
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
ms.locfileid: "30868112"
---
<a name="triggering-an-animation-in-another-control-c"></a>Auslösen einer Animation in ein anderes Steuerelement (c#)
====================
durch [Christian Wenz](https://github.com/wenz)

[Herunterladen von Code](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation8.cs.zip) oder [PDF herunterladen](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation8CS.pdf)

> Animation-Steuerelement in ASP.NET AJAX-Steuerelement-Toolkit ist nicht nur ein Steuerelement, aber eine gesamte Framework Animationen an ein Steuerelement hinzufügen. Starten eine Animation wird im Allgemeinen durch eine Benutzerinteraktion mit dem gleichen Steuerelement ausgelöst. Es ist jedoch auch möglich, mit einem Steuerelement, und klicken Sie dann Animation ein anderes Steuerelement interagieren.


## <a name="overview"></a>Übersicht

Animation-Steuerelement in ASP.NET AJAX-Steuerelement-Toolkit ist nicht nur ein Steuerelement, aber eine gesamte Framework Animationen an ein Steuerelement hinzufügen. Starten eine Animation wird im Allgemeinen durch eine Benutzerinteraktion mit dem gleichen Steuerelement ausgelöst. Es ist jedoch auch möglich, mit einem Steuerelement, und klicken Sie dann Animation ein anderes Steuerelement interagieren.

## <a name="steps"></a>Schritte

Erstens sind die `ScriptManager` auf der Seite; klicken Sie dann die ASP.NET AJAX-Bibliothek geladen ist, wodurch das Steuerelement-Toolkit verwenden:

[!code-aspx[Main](triggering-an-animation-in-another-control-cs/samples/sample1.aspx)]

Die Animation wird auf einen Bereich des Texts angewendet werden, die wie folgt aussieht:

[!code-aspx[Main](triggering-an-animation-in-another-control-cs/samples/sample2.aspx)]

Definieren Sie in der zugehörigen CSS-Klasse für den Bereich eine gute Hintergrundfarbe und auch festlegen Sie eine feste Breite für den Bereich:

[!code-css[Main](triggering-an-animation-in-another-control-cs/samples/sample3.css)]

Um das Panel Animation starten, wird eine HTML-Schaltfläche verwendet. Beachten Sie, dass `<input type="button" />` ist über begünstigt `<asp:Button />` da wir einen Postback nicht soll, wenn der Benutzer auf die Schaltfläche klickt.

[!code-aspx[Main](triggering-an-animation-in-another-control-cs/samples/sample4.aspx)]

Fügen Sie dann die `AnimationExtender` auf der Seite "Bereitstellen einer `ID`, die `TargetControlID` Attribut und der Auswahlparameter `runat="server"`. Es ist wichtig, legen Sie `TargetControlID` auf die ID der Schaltfläche (die die Animation auslösen-Element), nicht auf die ID des Bereichs (Element der zu animierenden)

[!code-aspx[Main](triggering-an-animation-in-another-control-cs/samples/sample5.aspx)]

Innerhalb der `<Animations>` Knoten Stelle Animationen wie gewohnt. Um den Bereich ändern machen, nicht die Schaltfläche ist die `AnimationTarget` -Attributs für jedes Animationselement in `AnimationExtender`. Der Wert für `AnimationTarget` ist die ID des Bereichs, natürlich. Auf diese Weise geschehen Animationen mit im Bereich nicht mit der auslösenden Schaltfläche. So sieht die `AnimationExtender` Markup für dieses Szenario:

[!code-aspx[Main](triggering-an-animation-in-another-control-cs/samples/sample6.aspx)]

Beachten Sie die spezielle Reihenfolge, in der die einzelnen Animationen angezeigt werden. Erstens Ruft die Schaltfläche deaktiviert, sobald die Animation ausgeführt wird. Pflege, weil keine `AnimationTarget` Attribut in der `<EnableAction>` Element dieser Animation auf die ursprünglichen Steuerelement angewendet wird: die Schaltfläche "". Die nächsten beiden Animationsschritte erfolgt parallelly (`<Parallel>` Element). Beide verfügen über ihre `AnimationTarget` Attribute festgelegt werden, um `"Panel1"`, daher Animieren der Bereich nicht auf die Schaltfläche.


[![Klicken mit der Maus auf die Schaltfläche "" startet das Panel animation](triggering-an-animation-in-another-control-cs/_static/image2.png)](triggering-an-animation-in-another-control-cs/_static/image1.png)

Startet die Panel Animation, klicken mit der Maus auf die Schaltfläche ([klicken Sie hier, um das Bild in voller Größe angezeigt](triggering-an-animation-in-another-control-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Zurück](disabling-actions-during-animation-cs.md)
> [Weiter](modifying-animations-from-the-server-side-cs.md)
