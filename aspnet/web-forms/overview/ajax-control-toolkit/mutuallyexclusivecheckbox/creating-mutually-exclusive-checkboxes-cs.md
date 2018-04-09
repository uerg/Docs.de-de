---
uid: web-forms/overview/ajax-control-toolkit/mutuallyexclusivecheckbox/creating-mutually-exclusive-checkboxes-cs
title: Erstellen sich gegenseitig ausschließende Kontrollkästchen (c#) | Microsoft Docs
author: wenz
description: 'Wenn nur eine der Optionen ausgewählt werden kann, werden in der Regel Optionsfelder verwendet. Obwohl ein Nachteil besteht: Nachdem ein Optionsfeld in einer Gruppe ausgewählt ist,...'
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 8e11b813-ba0d-4c29-b0f8-f65db6dbef1e
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/mutuallyexclusivecheckbox/creating-mutually-exclusive-checkboxes-cs
msc.type: authoredcontent
ms.openlocfilehash: c3a5abe7d02ace16f4aaad8d4adfbd0cba8e84ef
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
---
<a name="creating-mutually-exclusive-checkboxes-c"></a>Erstellen sich gegenseitig ausschließende Kontrollkästchen (c#)
====================
durch [Christian Wenz](https://github.com/wenz)

[Herunterladen von Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/MutuallyExclusiveCheckBox0.cs.zip) oder [PDF herunterladen](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/mutuallyexclusivecheckbox0CS.pdf)

> Wenn nur eine der Optionen ausgewählt werden kann, werden in der Regel Optionsfelder verwendet. Obwohl ein Nachteil besteht: Nachdem ein Optionsfeld in einer Gruppe ausgewählt ist, ist es nicht möglich, alle Optionsfelder deaktivieren. Kontrollkästchen kann zu einem beliebigen Zeitpunkt aufgehoben werden, sind jedoch nicht gegenseitig. Dieses Lernprogramm bietet das Beste beider Ansätze: Kontrollkästchen, die sich gegenseitig ausschließende sind.


## <a name="overview"></a>Übersicht

Wenn nur eine der Optionen ausgewählt werden kann, werden in der Regel Optionsfelder verwendet. Obwohl ein Nachteil besteht: Nachdem ein Optionsfeld in einer Gruppe ausgewählt ist, ist es nicht möglich, alle Optionsfelder deaktivieren. Kontrollkästchen kann zu einem beliebigen Zeitpunkt aufgehoben werden, sind jedoch nicht gegenseitig. Dieses Lernprogramm bietet das Beste beider Ansätze: Kontrollkästchen, die sich gegenseitig ausschließende sind.

## <a name="steps"></a>Schritte

Das ASP.NET AJAX-Steuerelement-Toolkit enthält die MutuallyExclusiveCheckBox Extender. Weisen Sie alle Kontrollkästchen auf einen Gruppennamen Programmierer können sich (`Key` Attribut). Alle Kontrollkästchen innerhalb der gleichen Gruppe darf nur eine gleichzeitig ausgewählt werden.

Beginnen wir mit zwei Kontrollkästchen auf eine neue ASP.NET-Seite ablegen. Es können weitere, aber zwei davon reicht aus, um das Prinzip veranschaulichen:

[!code-aspx[Main](creating-mutually-exclusive-checkboxes-cs/samples/sample1.aspx)]

Für beide Kontrollkästchen muss ein MutuallyExclusiveCheckBoxExtender-Steuerelement auf der Seite abgelegt werden. Beide Schlüsselattribute müssen den gleichen Wert wie der Wert sein die Attribute des HTML-Radio Button-Elemente mit der Gruppe "bezeichnen, in dem sie angehören identisch sein müssen. Die TargetControlID-Eigenschaft des Extenders verweist auf die ID des Kontrollkästchens auf.

[!code-aspx[Main](creating-mutually-exclusive-checkboxes-cs/samples/sample2.aspx)]

Darüber hinaus enthalten die ASP.NET AJAX `ScriptManager` das durch alle Elemente eines ASP.NET AJAX-Steuerelement-Toolkit erforderlich ist:

[!code-aspx[Main](creating-mutually-exclusive-checkboxes-cs/samples/sample3.aspx)]

Speichern und führen Sie die Seite: Sie aktivieren bzw. deaktivieren Sie beide Kontrollkästchen können jedoch zu keinem Zeitpunkt können beide Kontrollkästchen aktiviert werden.


[![Nur ein Kontrollkästchen kann zu einem Zeitpunkt überprüft werden](creating-mutually-exclusive-checkboxes-cs/_static/image2.png)](creating-mutually-exclusive-checkboxes-cs/_static/image1.png)

Zu einem Zeitpunkt kann nur ein Kontrollkästchen aktiviert werden ([klicken Sie hier, um das Bild in voller Größe angezeigt](creating-mutually-exclusive-checkboxes-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Nächste](creating-mutually-exclusive-checkboxes-vb.md)
