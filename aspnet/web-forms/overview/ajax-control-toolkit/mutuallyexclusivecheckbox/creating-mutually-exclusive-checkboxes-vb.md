---
uid: web-forms/overview/ajax-control-toolkit/mutuallyexclusivecheckbox/creating-mutually-exclusive-checkboxes-vb
title: "Erstellen sich gegenseitig ausschließende Kontrollkästchen (VB) | Microsoft Docs"
author: wenz
description: "Wenn nur eine der Optionen ausgewählt werden kann, werden in der Regel Optionsfelder verwendet. Obwohl ein Nachteil besteht: Nachdem ein Optionsfeld in einer Gruppe ausgewählt ist,..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: e9dd1d5a-a1db-4114-981d-6a91acb1d709
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/mutuallyexclusivecheckbox/creating-mutually-exclusive-checkboxes-vb
msc.type: authoredcontent
ms.openlocfilehash: 023ca0b145de8147a98e78f4dba20846dc344f06
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="creating-mutually-exclusive-checkboxes-vb"></a>Erstellen sich gegenseitig ausschließende Kontrollkästchen (VB)
====================
durch [Christian Wenz](https://github.com/wenz)

[Herunterladen von Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/MutuallyExclusiveCheckBox0.vb.zip) oder [PDF herunterladen](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/mutuallyexclusivecheckbox0VB.pdf)

> Wenn nur eine der Optionen ausgewählt werden kann, werden in der Regel Optionsfelder verwendet. Obwohl ein Nachteil besteht: Nachdem ein Optionsfeld in einer Gruppe ausgewählt ist, ist es nicht möglich, alle Optionsfelder deaktivieren. Kontrollkästchen kann zu einem beliebigen Zeitpunkt aufgehoben werden, sind jedoch nicht gegenseitig. Dieses Lernprogramm bietet das Beste beider Ansätze: Kontrollkästchen, die sich gegenseitig ausschließende sind.


## <a name="overview"></a>Übersicht

Wenn nur eine der Optionen ausgewählt werden kann, werden in der Regel Optionsfelder verwendet. Obwohl ein Nachteil besteht: Nachdem ein Optionsfeld in einer Gruppe ausgewählt ist, ist es nicht möglich, alle Optionsfelder deaktivieren. Kontrollkästchen kann zu einem beliebigen Zeitpunkt aufgehoben werden, sind jedoch nicht gegenseitig. Dieses Lernprogramm bietet das Beste beider Ansätze: Kontrollkästchen, die sich gegenseitig ausschließende sind.

## <a name="steps"></a>Schritte

Das ASP.NET AJAX-Steuerelement-Toolkit enthält die MutuallyExclusiveCheckBox Extender. Weisen Sie alle Kontrollkästchen auf einen Gruppennamen Programmierer können sich (`Key` Attribut). Alle Kontrollkästchen innerhalb der gleichen Gruppe darf nur eine gleichzeitig ausgewählt werden.

Beginnen wir mit zwei Kontrollkästchen auf eine neue ASP.NET-Seite ablegen. Es können weitere, aber zwei davon reicht aus, um das Prinzip veranschaulichen:

[!code-aspx[Main](creating-mutually-exclusive-checkboxes-vb/samples/sample1.aspx)]

Für beide Kontrollkästchen muss ein MutuallyExclusiveCheckBoxExtender-Steuerelement auf der Seite abgelegt werden. Beide Schlüsselattribute müssen den gleichen Wert wie der Wert sein die Attribute des HTML-Radio Button-Elemente mit der Gruppe "bezeichnen, in dem sie angehören identisch sein müssen. Die TargetControlID-Eigenschaft des Extenders verweist auf die ID des Kontrollkästchens auf.

[!code-aspx[Main](creating-mutually-exclusive-checkboxes-vb/samples/sample2.aspx)]

Darüber hinaus enthalten die ASP.NET AJAX `ScriptManager` das durch alle Elemente eines ASP.NET AJAX-Steuerelement-Toolkit erforderlich ist:

[!code-aspx[Main](creating-mutually-exclusive-checkboxes-vb/samples/sample3.aspx)]

Speichern und führen Sie die Seite: Sie aktivieren bzw. deaktivieren Sie beide Kontrollkästchen können jedoch zu keinem Zeitpunkt können beide Kontrollkästchen aktiviert werden.


[![Nur ein Kontrollkästchen kann zu einem Zeitpunkt überprüft werden](creating-mutually-exclusive-checkboxes-vb/_static/image2.png)](creating-mutually-exclusive-checkboxes-vb/_static/image1.png)

Zu einem Zeitpunkt kann nur ein Kontrollkästchen aktiviert werden ([klicken Sie hier, um das Bild in voller Größe angezeigt](creating-mutually-exclusive-checkboxes-vb/_static/image3.png))

>[!div class="step-by-step"]
[Zurück](creating-mutually-exclusive-checkboxes-cs.md)
