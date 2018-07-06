---
uid: web-forms/overview/ajax-control-toolkit/dropshadow/manipulating-dropshadow-properties-from-client-code-vb
title: Bearbeiten von DropShadow-Eigenschaften über den Clientcode (VB) | Microsoft-Dokumentation
author: wenz
description: DropShadow-Steuerelements im AJAX Control Toolkit erweitert, einen Bereich mit einem Schlagschatteneffekt. Eigenschaften dieses Extenders können auch mit JavaScript-Client geändert werden...
ms.author: aspnetcontent
ms.date: 06/02/2008
ms.assetid: 11be4211-2fb9-4e15-b6d4-2aa623d81f3e
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/dropshadow/manipulating-dropshadow-properties-from-client-code-vb
msc.type: authoredcontent
ms.openlocfilehash: b9220eecca224c2b1e0f19c972c6c2a4dc9c7d35
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2018
ms.locfileid: "37841408"
---
<a name="manipulating-dropshadow-properties-from-client-code-vb"></a>Bearbeiten von DropShadow-Eigenschaften über den Clientcode (VB)
====================
durch [Christian Wenz](https://github.com/wenz)

[Code herunterladen](http://download.microsoft.com/download/5/1/6/51652a81-500b-4f6b-88d3-617103e7941e/DropShadow2.vb.zip) oder [PDF-Datei herunterladen](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dropshadow2VB.pdf)

> DropShadow-Steuerelements im AJAX Control Toolkit erweitert, einen Bereich mit einem Schlagschatteneffekt. Eigenschaften dieses Extenders können auch mithilfe von JavaScript-Clientcode geändert werden.


## <a name="overview"></a>Übersicht

DropShadow-Steuerelements im AJAX Control Toolkit erweitert, einen Bereich mit einem Schlagschatteneffekt. Eigenschaften dieses Extenders können auch mithilfe von JavaScript-Clientcode geändert werden.

## <a name="steps"></a>Schritte

Der Code beginnt mit einem Bereich, der einige Textzeilen enthält:

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-vb/samples/sample1.aspx)]

Die zugeordnete CSS-Klasse bietet dem Bereich eine schöne Hintergrundfarbe:

[!code-css[Main](manipulating-dropshadow-properties-from-client-code-vb/samples/sample2.css)]

Die `DropShadowExtender` hinzugefügt wird, erweitern Sie den Bereich mit einem Schlagschatteneffekt, Deckkraft, die auf 50 % festgelegt:

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-vb/samples/sample3.aspx)]

Klicken Sie dann das ASP.NET AJAX `ScriptManager` -Steuerelement können Sie das Steuerelement-Toolkit funktioniert:

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-vb/samples/sample4.aspx)]

Einen anderen Bereich enthält zwei JavaScript-Links zum Festlegen der Durchlässigkeit des Schlagschattens: das Minuszeichen links verringert den Schatten Deckkraft, die plus-Verknüpfung wird vergrößert.

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-vb/samples/sample5.aspx)]

Die JavaScript-Funktion `changeOpacity()` suchen müssen Sie dann zuerst die `DropShadowExtender` Steuerelement auf der Seite. ASP.NET AJAX-definiert den `$find()` -Methode für genau diese Aufgabe. Anschließend wird die `get_Opacity()` -Methode ruft die aktuelle Durchlässigkeit, ab der `set_Opacity()` Methode festgelegt. Der JavaScript-Code fügt den aktuellen durchsichtigkeitswert klicken Sie dann in der `<label>` Element:

[!code-html[Main](manipulating-dropshadow-properties-from-client-code-vb/samples/sample6.html)]


[![Die Deckkraft wird auf der Clientseite geändert.](manipulating-dropshadow-properties-from-client-code-vb/_static/image2.png)](manipulating-dropshadow-properties-from-client-code-vb/_static/image1.png)

Die Deckkraft wird auf der Clientseite geändert ([klicken Sie, um das Bild in voller Größe anzeigen](manipulating-dropshadow-properties-from-client-code-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Vorherige](adjusting-the-z-index-of-a-dropshadow-vb.md)
