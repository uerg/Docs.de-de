---
uid: web-forms/overview/ajax-control-toolkit/slider/using-the-slider-control-with-auto-postback-cs
title: Verwenden das Schieberegler-Steuerelement mit Auto-Postback (c#) | Microsoft Docs
author: wenz
description: Das Schieberegler-Steuerelement im AJAX Control Toolkit bietet einen grafische Schieberegler, der mit der Maus gesteuert werden kann. Es ist möglich, stellen die Schieberegler Autopost...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 4d85e9fb-91e6-41f2-9c13-754549b19c27
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/slider/using-the-slider-control-with-auto-postback-cs
msc.type: authoredcontent
ms.openlocfilehash: e347d20c5c2ee48e6ed801e95459af6f0bcd2667
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
ms.locfileid: "30879243"
---
<a name="using-the-slider-control-with-auto-postback-c"></a>Verwenden das Schieberegler-Steuerelement mit Auto-Postback (c#)
====================
durch [Christian Wenz](https://github.com/wenz)

[Herunterladen von Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/Slider1.cs.zip) oder [PDF herunterladen](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/slider1CS.pdf)

> Das Schieberegler-Steuerelement im AJAX Control Toolkit bietet einen grafische Schieberegler, der mit der Maus gesteuert werden kann. Es ist möglich, stellen Sie der Schieberegler Autopostback einmal dessen Wert sich ändert.


## <a name="overview"></a>Übersicht

Das Schieberegler-Steuerelement im AJAX Control Toolkit bietet einen grafische Schieberegler, der mit der Maus gesteuert werden kann. Es ist möglich, stellen Sie der Schieberegler Autopostback einmal dessen Wert sich ändert.

## <a name="steps"></a>Schritte

Um den Schieberegler nach einer Änderung automatisch postback machen, benötigen beide Textfelder Attributs `AutoPostBack="true"`: das Textfeld, das den Schieberegler selbst werden soll, und das Textfeld, das den Schieberegler Position enthält. Hier ist das erforderliche Markup für diese:

[!code-aspx[Main](using-the-slider-control-with-auto-postback-cs/samples/sample1.aspx)]

Die `SliderExtender` Steuerelement aus dem ASP.NET AJAX-Steuerelement-Toolkit weist die beiden Textfelder die Schieberegler-Funktionalität:

[!code-aspx[Main](using-the-slider-control-with-auto-postback-cs/samples/sample2.aspx)]

Zusätzliche Label-Element wird später verwendet werden, um die Benutzer über ein Postback zu informieren:

[!code-aspx[Main](using-the-slider-control-with-auto-postback-cs/samples/sample3.aspx)]

Schließlich die `ScriptManager` -Steuerelement von ASP.NET AJAX lädt die erforderlichen JavaScript für das Steuerelement Toolkit arbeiten:

[!code-aspx[Main](using-the-slider-control-with-auto-postback-cs/samples/sample4.aspx)]

Der Schieberegler ist jetzt wieder buchen; auf der Serverseite kann dieses Ereignis abgefangen und Reaktionen bewirkten:

[!code-aspx[Main](using-the-slider-control-with-auto-postback-cs/samples/sample5.aspx)]


[![Verschieben des Schiebereglers wird einen Postback ausgelöst.](using-the-slider-control-with-auto-postback-cs/_static/image2.png)](using-the-slider-control-with-auto-postback-cs/_static/image1.png)

Verschieben des Schiebereglers wird einen Postback ausgelöst ([klicken Sie hier, um das Bild in voller Größe angezeigt](using-the-slider-control-with-auto-postback-cs/_static/image3.png))


[![Das Datum der Änderung wird anschließend in die Bezeichnung geschrieben.](using-the-slider-control-with-auto-postback-cs/_static/image5.png)](using-the-slider-control-with-auto-postback-cs/_static/image4.png)

Danach wird das Datum der Änderung in die Bezeichnung geschrieben ([klicken Sie hier, um das Bild in voller Größe angezeigt](using-the-slider-control-with-auto-postback-cs/_static/image6.png))

> [!div class="step-by-step"]
> [Nächste](databinding-the-slider-control-cs.md)
