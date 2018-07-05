---
uid: web-forms/overview/ajax-control-toolkit/animation/picking-one-animation-out-of-a-list-vb
title: Auswählen einer Animation aus einer Liste (VB) | Microsoft-Dokumentation
author: wenz
description: Die Animation-Steuerelement in ASP.NET AJAX Control Toolkit ist nicht nur ein Steuerelement, aber ein ganzes Framework Animationen an ein Steuerelement hinzufügen. Das Framework auch zulassen...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 81ba9116-d485-40c0-8ff6-7e9ae23e0a0c
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/picking-one-animation-out-of-a-list-vb
msc.type: authoredcontent
ms.openlocfilehash: 561f05e96888962cfe576963ce3905b171203939
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/03/2018
ms.locfileid: "37387740"
---
<a name="picking-one-animation-out-of-a-list-vb"></a>Auswählen einer Animation aus einer Liste (VB)
====================
durch [Christian Wenz](https://github.com/wenz)

[Code herunterladen](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation5.vb.zip) oder [PDF-Datei herunterladen](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation5VB.pdf)

> Die Animation-Steuerelement in ASP.NET AJAX Control Toolkit ist nicht nur ein Steuerelement, aber ein ganzes Framework Animationen an ein Steuerelement hinzufügen. Das Framework ermöglicht es auch den Programmierer, wählen Sie eine Animation aus einer Liste von Animationen, je nach der Auswertung von JavaScript-Code.


## <a name="overview"></a>Übersicht

Die Animation-Steuerelement in ASP.NET AJAX Control Toolkit ist nicht nur ein Steuerelement, aber ein ganzes Framework Animationen an ein Steuerelement hinzufügen. Das Framework ermöglicht es auch den Programmierer, wählen Sie eine Animation aus einer Liste von Animationen, je nach der Auswertung von JavaScript-Code.

## <a name="steps"></a>Schritte

Zunächst einmal sind die `ScriptManager` in die Seite klicken Sie dann die ASP.NET AJAX-Bibliothek wird geladen, lässt sich das Steuerelement-Toolkit verwenden:

[!code-aspx[Main](picking-one-animation-out-of-a-list-vb/samples/sample1.aspx)]

Die Animation wird auf einen Bereich des Texts angewendet werden, der so aussieht:

[!code-aspx[Main](picking-one-animation-out-of-a-list-vb/samples/sample2.aspx)]

Definieren Sie in der zugehörigen CSS-Klasse für den Bereich eine gute Hintergrundfarbe aus, und auch festlegen Sie eine feste Breite für den Bereich:

[!code-css[Main](picking-one-animation-out-of-a-list-vb/samples/sample3.css)]

Fügen Sie dann die `AnimationExtender` auf der Seite Bereitstellen einer `ID`, `TargetControlID` -Attribut und das obligatorische `runat="server":`

[!code-aspx[Main](picking-one-animation-out-of-a-list-vb/samples/sample4.aspx)]

In der `<Animations>` Knoten verwenden `<OnLoad>` die Animationen ausgeführt werden, nachdem die Seite vollständig geladen wurde. Statt in einem regulären-Animationen die `<Case>` Element ins Spiel. Der Wert seines Attributs SelectScript wird ausgewertet; der Rückgabewert muss numerische sein. Je nach dieser Anzahl, eines der Subanimation in &lt;Fall&gt; ausgeführt wird. Wenn SelectScript 2 ergibt, führt das Steuerelement-Toolkit z. B. dritte Animation in &lt;Fall&gt; (Zählung von 0 beginnt).

Das folgende Markup definiert drei Subanimation: Ändern Sie die Breite der Größe, ändern Sie die Höhe der Größe und ausgeblendet wird. Der JavaScript-Code (`Math.floor(3 * Math.random())`) wählt anschließend eine Zahl zwischen 0 und 2, sodass einer der drei Animationen ausgeführt wird:

[!code-aspx[Main](picking-one-animation-out-of-a-list-vb/samples/sample5.aspx)]


[![Einer der drei möglichen Animationen: Ruft der Bereich ab, größere](picking-one-animation-out-of-a-list-vb/_static/image2.png)](picking-one-animation-out-of-a-list-vb/_static/image1.png)

Einer der drei möglichen Animationen: Ruft der Bereich ab, größere ([klicken Sie, um das Bild in voller Größe anzeigen](picking-one-animation-out-of-a-list-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Zurück](animation-depending-on-a-condition-vb.md)
> [Weiter](animating-in-response-to-user-interaction-vb.md)
