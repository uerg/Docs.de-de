---
uid: web-forms/overview/ajax-control-toolkit/dropshadow/manipulating-dropshadow-properties-from-client-code-vb
title: Bearbeiten von DropShadow-Eigenschaften aus dem Clientcode (VB) | Microsoft Docs
author: wenz
description: DropShadow-Steuerelements in der AJAX-Steuerelement-Toolkit erweitert einen Bereich mit Schatten. Eigenschaften von diesen Extender können auch mithilfe der Client JavaScrip geändert werden...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 11be4211-2fb9-4e15-b6d4-2aa623d81f3e
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/dropshadow/manipulating-dropshadow-properties-from-client-code-vb
msc.type: authoredcontent
ms.openlocfilehash: b5b024811ea511e67ce180169de9f0b7e3ef51d9
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
ms.locfileid: "30870907"
---
<a name="manipulating-dropshadow-properties-from-client-code-vb"></a>Bearbeiten von DropShadow-Eigenschaften aus dem Clientcode (VB)
====================
durch [Christian Wenz](https://github.com/wenz)

[Herunterladen von Code](http://download.microsoft.com/download/5/1/6/51652a81-500b-4f6b-88d3-617103e7941e/DropShadow2.vb.zip) oder [PDF herunterladen](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dropshadow2VB.pdf)

> DropShadow-Steuerelements in der AJAX-Steuerelement-Toolkit erweitert einen Bereich mit Schatten. Eigenschaften von diesen Extender können auch mit Client-JavaScript-Code geändert werden.


## <a name="overview"></a>Übersicht

DropShadow-Steuerelements in der AJAX-Steuerelement-Toolkit erweitert einen Bereich mit Schatten. Eigenschaften von diesen Extender können auch mit Client-JavaScript-Code geändert werden.

## <a name="steps"></a>Schritte

Der Code beginnt mit einem Bereich, die einige Textzeilen enthält:

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-vb/samples/sample1.aspx)]

Zugeordnete CSS-Klasse bietet im Bereich eine gute Hintergrundfarbe:

[!code-css[Main](manipulating-dropshadow-properties-from-client-code-vb/samples/sample2.css)]

Die `DropShadowExtender` wird hinzugefügt, um den Bereich mit einem Schlagschatteneffekt Deckkraft auf 50 % zu erweitern:

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-vb/samples/sample3.aspx)]

Klicken Sie dann, die ASP.NET AJAX `ScriptManager` Steuerelement ermöglicht das Toolkit Steuerelement funktioniert:

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-vb/samples/sample4.aspx)]

Einen anderen Bereich enthält zwei JavaScript-Links für die Durchlässigkeit des ablageschattens: minus Link verringert die Deckkraft des Schattens, plus Link erhöht.

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-vb/samples/sample5.aspx)]

Die JavaScript-Funktion `changeOpacity()` suchen müssen Sie dann zuerst die `DropShadowExtender` Steuerelement auf der Seite. ASP.NET AJAX-definiert die `$find()` Methode für genau diese Aufgabe. Anschließend wird die `get_Opacity()` Methode ruft die aktuellen Deckkraft, ab der `set_Opacity()` Methode legt sie fest. Der JavaScript-Code fügt dann der aktuelle Wert für die Deckkraft in der `<label>` Element:

[!code-html[Main](manipulating-dropshadow-properties-from-client-code-vb/samples/sample6.html)]


[![Die Deckkraft wird auf der Clientseite geändert.](manipulating-dropshadow-properties-from-client-code-vb/_static/image2.png)](manipulating-dropshadow-properties-from-client-code-vb/_static/image1.png)

Die Deckkraft wird auf der Clientseite geändert ([klicken Sie hier, um das Bild in voller Größe angezeigt](manipulating-dropshadow-properties-from-client-code-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Vorherige](adjusting-the-z-index-of-a-dropshadow-vb.md)
