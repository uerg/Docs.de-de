---
uid: web-forms/overview/ajax-control-toolkit/dropshadow/adjusting-the-z-index-of-a-dropshadow-cs
title: Anpassen der Z-Index von einem DropShadow (c#) | Microsoft Docs
author: wenz
description: "DropShadow-Steuerelements in der AJAX-Steuerelement-Toolkit erweitert einen Bereich mit Schatten. Jedoch diese Schatten manchmal mit anderen Steuerelementen für Insta steht in Konflikt..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 14133833-e518-4347-87b9-6b6f71f14a77
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/dropshadow/adjusting-the-z-index-of-a-dropshadow-cs
msc.type: authoredcontent
ms.openlocfilehash: 161f02daa5dd1f0e21853c1b7c1a65c1a9aa5d03
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="adjusting-the-z-index-of-a-dropshadow-c"></a>Anpassen der Z-Index von einem DropShadow (c#)
====================
durch [Christian Wenz](https://github.com/wenz)

[Herunterladen von Code](http://download.microsoft.com/download/5/1/6/51652a81-500b-4f6b-88d3-617103e7941e/DropShadow1.cs.zip) oder [PDF herunterladen](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dropshadow1CS.pdf)

> DropShadow-Steuerelements in der AJAX-Steuerelement-Toolkit erweitert einen Bereich mit Schatten. Jedoch diese Schatten manchmal mit anderen Steuerelementen, z. B. das ASP.NET Menüsteuerelement steht in Konflikt. Wenn ein Menüeintrag gestartet wurde, wird er hinter der Schatten angezeigt wird.


## <a name="overview"></a>Übersicht

DropShadow-Steuerelements in der AJAX-Steuerelement-Toolkit erweitert einen Bereich mit Schatten. Jedoch diese Schatten manchmal mit anderen Steuerelementen, z. B. das ASP.NET Menüsteuerelement steht in Konflikt. Wenn ein Menüeintrag gestartet wurde, wird er hinter der Schatten angezeigt wird.

## <a name="steps"></a>Schritte

Der Code beginnt mit dem Bildschirm selbst, genug Text enthält, sodass Bereich genug Text für den Effekt sichtbar sein enthält:

[!code-aspx[Main](adjusting-the-z-index-of-a-dropshadow-cs/samples/sample1.aspx)]

Einem anderen Bereich befindet sich direkt vor der `panelShadow` Bereich. Enthält ein Menü mit horizontalen Ausrichtung, sodass über Menüeinträge im angezeigt werden (oder vielmehr: unter) die `dropShadow` Bereich):

[!code-aspx[Main](adjusting-the-z-index-of-a-dropshadow-cs/samples/sample2.aspx)]

Anschließend wird die `DropShadowExtender` wird hinzugefügt, zum Erweitern der `panelShadow` Bereich mit einem Schlagschatteneffekt:

[!code-aspx[Main](adjusting-the-z-index-of-a-dropshadow-cs/samples/sample3.aspx)]

Schließlich ASP.NET AJAX `ScriptManager` Steuerelement ermöglicht das Toolkit Steuerelement funktioniert:

[!code-aspx[Main](adjusting-the-z-index-of-a-dropshadow-cs/samples/sample4.aspx)]

Wenn Sie dieses Skript ausführen, werden die Menüeinträge im unterhalb der Systemsteuerung angezeigt. Klicken Sie im Menü die CSS-Klasse verwendet, jedoch `panel` , in dem Sie lassen, definieren Sie zwei Dinge Elemente vor anderen Fensterbereich angezeigt:

- Relative Positionierung
- Eine positive Z-index

[!code-css[Main](adjusting-the-z-index-of-a-dropshadow-cs/samples/sample5.css)]

Anschließend wird die `DropShadowExtender` Steuerelement keine Konflikte mehr mit den Menu-Steuerelement.


[![Vorher: Die Menüeintrag ist nicht sichtbar](adjusting-the-z-index-of-a-dropshadow-cs/_static/image2.png)](adjusting-the-z-index-of-a-dropshadow-cs/_static/image1.png)

Vorher: Ist der Menüeintrag nicht sichtbar ([klicken Sie hier, um das Bild in voller Größe angezeigt](adjusting-the-z-index-of-a-dropshadow-cs/_static/image3.png))


[![Nach: Der Menüeintrag im angezeigt wird](adjusting-the-z-index-of-a-dropshadow-cs/_static/image5.png)](adjusting-the-z-index-of-a-dropshadow-cs/_static/image4.png)

Nach: Wird angezeigt, den Menüeintrag ([klicken Sie hier, um das Bild in voller Größe angezeigt](adjusting-the-z-index-of-a-dropshadow-cs/_static/image6.png))

>[!div class="step-by-step"]
[Nächste](manipulating-dropshadow-properties-from-client-code-cs.md)
