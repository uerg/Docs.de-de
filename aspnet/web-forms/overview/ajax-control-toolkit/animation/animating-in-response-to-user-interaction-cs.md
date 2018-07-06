---
uid: web-forms/overview/ajax-control-toolkit/animation/animating-in-response-to-user-interaction-cs
title: Animieren als Reaktion auf eine Benutzerinteraktion (c#) | Microsoft-Dokumentation
author: wenz
description: Die Animation-Steuerelement in ASP.NET AJAX Control Toolkit ist nicht nur ein Steuerelement, aber ein ganzes Framework Animationen an ein Steuerelement hinzufügen. Die Animationen können Sternen...
ms.author: aspnetcontent
ms.date: 06/02/2008
ms.assetid: ea26549d-fbbf-4973-a108-b14cd1d6de26
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/animating-in-response-to-user-interaction-cs
msc.type: authoredcontent
ms.openlocfilehash: 5e8ebf5ec7fc0875e0eb43923321513bf0a08899
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2018
ms.locfileid: "37826130"
---
<a name="animating-in-response-to-user-interaction-c"></a>Animieren als Reaktion auf eine Benutzerinteraktion (c#)
====================
durch [Christian Wenz](https://github.com/wenz)

[Code herunterladen](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation6.cs.zip) oder [PDF-Datei herunterladen](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation6CS.pdf)

> Die Animation-Steuerelement in ASP.NET AJAX Control Toolkit ist nicht nur ein Steuerelement, aber ein ganzes Framework Animationen an ein Steuerelement hinzufügen. Die Animationen können automatisch starten, oder es werden möglicherweise ausgelöst durch einen Benutzereingriff, z. B. indem Sie mit der Maus auf.


## <a name="overview"></a>Übersicht

Die Animation-Steuerelement in ASP.NET AJAX Control Toolkit ist nicht nur ein Steuerelement, aber ein ganzes Framework Animationen an ein Steuerelement hinzufügen. Die Animationen können automatisch starten, oder es werden möglicherweise ausgelöst durch einen Benutzereingriff, z. B. indem Sie mit der Maus auf.

## <a name="steps"></a>Schritte

Zunächst einmal sind die `ScriptManager` in die Seite klicken Sie dann die ASP.NET AJAX-Bibliothek wird geladen, lässt sich das Steuerelement-Toolkit verwenden:

[!code-aspx[Main](animating-in-response-to-user-interaction-cs/samples/sample1.aspx)]

Die Animation wird auf einen Bereich des Texts angewendet werden, der so aussieht:

[!code-aspx[Main](animating-in-response-to-user-interaction-cs/samples/sample2.aspx)]

Definieren Sie in der zugehörigen CSS-Klasse für den Bereich eine gute Hintergrundfarbe aus, und auch festlegen Sie eine feste Breite für den Bereich:

[!code-css[Main](animating-in-response-to-user-interaction-cs/samples/sample3.css)]

Fügen Sie dann die `AnimationExtender` auf der Seite Bereitstellen einer `ID`, `TargetControlID` -Attribut und das obligatorische `runat="server"`:

[!code-aspx[Main](animating-in-response-to-user-interaction-cs/samples/sample4.aspx)]

In der `<Animations>` Knoten stehen fünf Methoden zum Starten der Animation über eine Benutzerinteraktion (das fehlende Element ist `<OnLoad>` die wird ausgeführt, nachdem die gesamte Seite vollständig geladen wurde):

- `<OnClick>` (Mausklick auf das Steuerelement)
- `<OnHoverOut>` (Mauszeiger das Steuerelement)
- `<OnHoverOver>` (Maus bewegt wird, auf ein Steuerelement, das Beenden der `<OnHoverOut>` Animation)
- `<OnMouseOut>` (Mauszeiger ein Steuerelement)
- `<OnMouseOver>` (Maus bewegt wird, auf ein Steuerelement, das nicht beendet die `<OnMouseOut>` Animation)

In diesem Szenario `<OnClick>` verwendet wird. Klickt der Benutzer im Bereich angepasst wird und zur gleichen Zeit ausgeblendet wird.

[!code-aspx[Main](animating-in-response-to-user-interaction-cs/samples/sample5.aspx)]


[![Startet die Animation, klicken mit der Maus](animating-in-response-to-user-interaction-cs/_static/image2.png)](animating-in-response-to-user-interaction-cs/_static/image1.png)

Startet die Animation, klicken mit der Maus ([klicken Sie, um das Bild in voller Größe anzeigen](animating-in-response-to-user-interaction-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Zurück](picking-one-animation-out-of-a-list-cs.md)
> [Weiter](disabling-actions-during-animation-cs.md)
