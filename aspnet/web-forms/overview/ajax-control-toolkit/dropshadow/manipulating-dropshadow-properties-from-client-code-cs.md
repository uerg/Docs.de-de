---
uid: web-forms/overview/ajax-control-toolkit/dropshadow/manipulating-dropshadow-properties-from-client-code-cs
title: Bearbeiten von DropShadow-Eigenschaften über den Clientcode (c#) | Microsoft-Dokumentation
author: wenz
description: Die Bearbeitung von DataList anpassen
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: c83ca3e6-c0bf-4158-a166-40c1ab0f33da
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/dropshadow/manipulating-dropshadow-properties-from-client-code-cs
msc.type: authoredcontent
ms.openlocfilehash: e3166b9da97a0f4097566b62ba52b6d672eab78f
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/03/2018
ms.locfileid: "37377284"
---
<a name="manipulating-dropshadow-properties-from-client-code-c"></a>Bearbeiten von DropShadow-Eigenschaften über den Clientcode (c#)
====================
durch [Christian Wenz](https://github.com/wenz)

[Code herunterladen](http://download.microsoft.com/download/5/1/6/51652a81-500b-4f6b-88d3-617103e7941e/DropShadow2.cs.zip) oder [PDF-Datei herunterladen](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dropshadow2CS.pdf)

> DropShadow-Steuerelements im AJAX Control Toolkit erweitert, einen Bereich mit einem Schlagschatteneffekt. Eigenschaften dieses Extenders können auch mithilfe von JavaScript-Clientcode geändert werden.


## <a name="overview"></a>Übersicht

DropShadow-Steuerelements im AJAX Control Toolkit erweitert, einen Bereich mit einem Schlagschatteneffekt. Eigenschaften dieses Extenders können auch mithilfe von JavaScript-Clientcode geändert werden.

## <a name="steps"></a>Schritte

Der Code beginnt mit einem Bereich, der einige Textzeilen enthält:

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-cs/samples/sample1.aspx)]

Die zugeordnete CSS-Klasse bietet dem Bereich eine schöne Hintergrundfarbe:

[!code-css[Main](manipulating-dropshadow-properties-from-client-code-cs/samples/sample2.css)]

Die `DropShadowExtender` hinzugefügt wird, erweitern Sie den Bereich mit einem Schlagschatteneffekt, Deckkraft, die auf 50 % festgelegt:

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-cs/samples/sample3.aspx)]

Klicken Sie dann das ASP.NET AJAX `ScriptManager` -Steuerelement können Sie das Steuerelement-Toolkit funktioniert:

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-cs/samples/sample4.aspx)]

Einen anderen Bereich enthält zwei JavaScript-Links zum Festlegen der Durchlässigkeit des Schlagschattens: das Minuszeichen links verringert den Schatten Deckkraft, die plus-Verknüpfung wird vergrößert.

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-cs/samples/sample5.aspx)]

Die JavaScript-Funktion `changeOpacity()` suchen müssen Sie dann zuerst die `DropShadowExtender` Steuerelement auf der Seite. ASP.NET AJAX-definiert den `$find()` -Methode für genau diese Aufgabe. Anschließend wird die `get_Opacity()` -Methode ruft die aktuelle Durchlässigkeit, ab der `set_Opacity()` Methode festgelegt. Der JavaScript-Code fügt den aktuellen durchsichtigkeitswert klicken Sie dann in der `<label>` Element:

[!code-html[Main](manipulating-dropshadow-properties-from-client-code-cs/samples/sample6.html)]


[![Die Deckkraft wird auf der Clientseite geändert.](manipulating-dropshadow-properties-from-client-code-cs/_static/image2.png)](manipulating-dropshadow-properties-from-client-code-cs/_static/image1.png)

Die Deckkraft wird auf der Clientseite geändert ([klicken Sie, um das Bild in voller Größe anzeigen](manipulating-dropshadow-properties-from-client-code-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Zurück](adjusting-the-z-index-of-a-dropshadow-cs.md)
> [Weiter](adjusting-the-z-index-of-a-dropshadow-vb.md)
