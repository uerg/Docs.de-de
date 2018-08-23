---
uid: web-forms/overview/ajax-control-toolkit/slider/databinding-the-slider-control-vb
title: Datenbindung des Schieberegler-Steuerelements (VB) | Microsoft-Dokumentation
author: wenz
description: Das Schieberegler-Steuerelement im AJAX Control Toolkit stellt einen grafischen Schieberegler, der mit der Maus kontrolliert werden können. Es ist möglich, binden Sie die aktuelle Position...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 4f3ba53f-d166-422d-b29c-403348057836
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/slider/databinding-the-slider-control-vb
msc.type: authoredcontent
ms.openlocfilehash: 284a650d1b63ceed887deb3ddd639fe9fd27019f
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2018
ms.locfileid: "41838688"
---
<a name="databinding-the-slider-control-vb"></a>Datenbindung des Schieberegler-Steuerelements (VB)
====================
durch [Christian Wenz](https://github.com/wenz)

[Code herunterladen](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/Slider0.vb.zip) oder [PDF-Datei herunterladen](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/slider0VB.pdf)

> Das Schieberegler-Steuerelement im AJAX Control Toolkit stellt einen grafischen Schieberegler, der mit der Maus kontrolliert werden können. Es ist möglich, die die aktuelle Position des Schiebereglers an ein anderes ASP.NET-Steuerelement zu binden.


## <a name="overview"></a>Übersicht

Das Schieberegler-Steuerelement im AJAX Control Toolkit stellt einen grafischen Schieberegler, der mit der Maus kontrolliert werden können. Es ist möglich, die die aktuelle Position des Schiebereglers an ein anderes ASP.NET-Steuerelement zu binden.

## <a name="steps"></a>Schritte

Um die Funktionalität von ASP.NET AJAX und das Steuerelement-Toolkit, aktivieren die `ScriptManager` Steuerelement an einer beliebigen Stelle auf der Seite platziert werden muss (jedoch innerhalb der `<form>` Element):

[!code-aspx[Main](databinding-the-slider-control-vb/samples/sample1.aspx)]

Als Nächstes fügen Sie zwei `TextBox` Steuerelemente auf der Seite. Transformiert eine in einen grafischen Schieberegler, und der andere Controller wird die Position des Schiebereglers aufzunehmen.

[!code-aspx[Main](databinding-the-slider-control-vb/samples/sample2.aspx)]

Der nächste Schritt ist bereits im letzten Schritt. Die `SliderExtender` Steuerelement von ASP.NET AJAX Control Toolkit stellt einen Schieberegler aus dem ersten Textfeld, und das zweite Textfeld automatisch aktualisiert, wenn der Schieberegler positionieren Änderungen. Damit dies funktioniert die `SliderExtender`des `TargetControlID` Attribut muss festgelegt werden, auf die ID des ersten Textfelds; die `BoundControlID` Attribut muss auf die ID des im zweiten Textfeld festgelegt werden.

[!code-aspx[Main](databinding-the-slider-control-vb/samples/sample3.aspx)]

Wie Sie im Browser sehen können, funktioniert die Datenbindung in beide Richtungen: einen neuen Wert in das Textfeld eingeben, aktualisiert die Position der des Schiebereglers. Wenn Sie das zweite Textfeld schreibgeschützt machen, können Sie einen unzureichenden Schutz auf das Textfeld hinzufügen, sodass es schwieriger für den Benutzer manuell auf den Wert hier aktualisieren.


[![Schieberegler und Textfeld werden synchronisiert.](databinding-the-slider-control-vb/_static/image2.png)](databinding-the-slider-control-vb/_static/image1.png)

Schieberegler und Textfeld synchronisiert werden ([klicken Sie, um das Bild in voller Größe anzeigen](databinding-the-slider-control-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Vorherige](using-the-slider-control-with-auto-postback-vb.md)
