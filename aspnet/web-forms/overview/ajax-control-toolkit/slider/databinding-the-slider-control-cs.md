---
uid: web-forms/overview/ajax-control-toolkit/slider/databinding-the-slider-control-cs
title: DataBinding das Schieberegler-Steuerelement (c#) | Microsoft Docs
author: wenz
description: Das Schieberegler-Steuerelement im AJAX Control Toolkit bietet einen grafische Schieberegler, der mit der Maus gesteuert werden kann. Es ist möglich, binden Sie die aktuelle Position...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: b7f77869-aa1d-4025-924f-622c57112db6
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/slider/databinding-the-slider-control-cs
msc.type: authoredcontent
ms.openlocfilehash: 7644c991cd88868235511ba372be1f5b47c68fea
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
ms.locfileid: "30870582"
---
<a name="databinding-the-slider-control-c"></a>DataBinding das Schieberegler-Steuerelement (c#)
====================
durch [Christian Wenz](https://github.com/wenz)

[Herunterladen von Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/Slider0.cs.zip) oder [PDF herunterladen](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/slider0CS.pdf)

> Das Schieberegler-Steuerelement im AJAX Control Toolkit bietet einen grafische Schieberegler, der mit der Maus gesteuert werden kann. Es ist möglich, die aktuelle Position des Schiebereglers auf ein anderes ASP.NET-Steuerelement zu binden.


## <a name="overview"></a>Übersicht

Das Schieberegler-Steuerelement im AJAX Control Toolkit bietet einen grafische Schieberegler, der mit der Maus gesteuert werden kann. Es ist möglich, die aktuelle Position des Schiebereglers auf ein anderes ASP.NET-Steuerelement zu binden.

## <a name="steps"></a>Schritte

Um die Funktionalität von ASP.NET AJAX und das Steuerelement-Toolkit aktivieren die `ScriptManager` Steuerelement an einer beliebigen Stelle auf der Seite versetzt werden muss (jedoch innerhalb der `<form>` Element):

[!code-aspx[Main](databinding-the-slider-control-cs/samples/sample1.aspx)]

Als Nächstes fügen Sie zwei `TextBox` Steuerelemente auf der Seite. Eine wird in einem grafischen Schieberegler transformiert werden, und der andere Controller wird die Position des Schiebereglers halten.

[!code-aspx[Main](databinding-the-slider-control-cs/samples/sample2.aspx)]

Der nächste Schritt ist bereits im letzten Schritt. Die `SliderExtender` Steuerelement aus dem ASP.NET AJAX-Steuerelement-Toolkit stellt einen Schieberegler aus dem ersten Textfeld und das zweite Textfeld automatisch aktualisiert, wenn Änderungen positionieren Sie der Schieberegler. Damit dies funktioniert die `SliderExtender`des `TargetControlID` Attribut muss festgelegt werden, auf die ID der im ersten Textfeld; die `BoundControlID` Attribut muss festgelegt werden, um die ID des zweiten Textfeld.

[!code-aspx[Main](databinding-the-slider-control-cs/samples/sample3.aspx)]

Wie Sie im Browser sehen können, funktioniert die Datenbindung in beide Richtungen: Position des Schiebereglers aktualisiert einen neuen Wert in das Textfeld eingeben. Wenn Sie das zweite Textfeld schreibgeschützt vornehmen, können Sie das Textfeld "einen unzureichenden Schutz hinzufügen, damit es erschwert, sich für den Benutzer so aktualisieren Sie manuell auf den Wert vorhanden ist.


[![Schieberegler und Textfeld sind synchronisiert.](databinding-the-slider-control-cs/_static/image2.png)](databinding-the-slider-control-cs/_static/image1.png)

Schieberegler und Textfeld sind synchron ([klicken Sie hier, um das Bild in voller Größe angezeigt](databinding-the-slider-control-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Zurück](using-the-slider-control-with-auto-postback-cs.md)
> [Weiter](using-the-slider-control-with-auto-postback-vb.md)
