---
uid: web-forms/overview/ajax-control-toolkit/animation/animating-an-updatepanel-control-vb
title: Animieren eines UpdatePanel-Steuerelements (VB) | Microsoft Docs
author: wenz
description: Animation-Steuerelement in ASP.NET AJAX-Steuerelement-Toolkit ist nicht nur ein Steuerelement, aber eine gesamte Framework Animationen an ein Steuerelement hinzufügen. Für den Inhalt einer...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 4c306a2c-92b6-4904-b70b-365b847334fe
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/animating-an-updatepanel-control-vb
msc.type: authoredcontent
ms.openlocfilehash: 2c1114b74fd152a4ea85aa10850860f75573adee
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
---
<a name="animating-an-updatepanel-control-vb"></a>Animieren eines UpdatePanel-Steuerelements (VB)
====================
durch [Christian Wenz](https://github.com/wenz)

[Herunterladen von Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/UpdatePanelAnimation1.vb.zip) oder [PDF herunterladen](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/updatepanelanimation1VB.pdf)

> Animation-Steuerelement in ASP.NET AJAX-Steuerelement-Toolkit ist nicht nur ein Steuerelement, aber eine gesamte Framework Animationen an ein Steuerelement hinzufügen. Für den Inhalt eines UpdatePanel, ein spezieller Extender vorhanden ist, die sich stark auf die Animationsframework stützt: UpdatePanelAnimation. In diesem Lernprogramm wird gezeigt, wie solche eine Animation für UpdatePanel eingerichtet wird.


## <a name="overview"></a>Übersicht

Animation-Steuerelement in ASP.NET AJAX-Steuerelement-Toolkit ist nicht nur ein Steuerelement, aber eine gesamte Framework Animationen an ein Steuerelement hinzufügen. Für den Inhalt einer `UpdatePanel`, ein spezieller Extender vorhanden ist, die sich stark auf die Animationsframework stützt: `UpdatePanelAnimation`. In diesem Lernprogramm wird gezeigt, wie solche eine Animation für Einrichten einer `UpdatePanel`.

## <a name="steps"></a>Schritte

Der erste Schritt besteht wie gewohnt enthalten die `ScriptManager` auf der Seite, damit die ASP.NET AJAX-Bibliothek geladen wird und die Control-Toolkit verwendet werden können:

[!code-aspx[Main](animating-an-updatepanel-control-vb/samples/sample1.aspx)]

Die Animation in diesem Szenario gelten für eine ASP.NET `Wizard` Websteuerelement in den ein `UpdatePanel`. Drei (beliebigen) Schritte bieten genügend Optionen Postbacks auslösen:

[!code-aspx[Main](animating-an-updatepanel-control-vb/samples/sample2.aspx)]

Das Markup für die `UpdatePanelAnimationExtender` Steuerelement sind sehr ähnlich, um das Markup, das für die `AnimationExtender`. In der `TargetControlID` Attribut wir bieten die `ID` von der `UpdatePanel` zu animierende; innerhalb der `UpdatePanelAnimationExtender` -Steuerelement, die `<Animations>` -Element enthält XML-Markup für Animationen. Allerdings gibt es einen Unterschied: die Menge der Ereignisse und Ereignishandler ist im Vergleich zur beschränkt `AnimationExtender`. Für `UpdatePanels`, nur zwei davon vorhanden:

- `<OnUpdated>` Wenn die UpdatePanel aktualisiert wurde
- `<OnUpdating>` Beim Starten der UpdatePanel aktualisieren

In diesem Szenario wird der neue Inhalt der `UpdatePanel` (nach dem Postback) eingeblendet wird. Dies ist die notwendige Markup für diese:

[!code-aspx[Main](animating-an-updatepanel-control-vb/samples/sample3.aspx)]

Jetzt bei jedem asynchronen in UpdatePanel Postback Einblenden der neue Inhalt des Bereichs reibungslos.


[![Der nächste Assistentenschritt wird im ausblenden](animating-an-updatepanel-control-vb/_static/image2.png)](animating-an-updatepanel-control-vb/_static/image1.png)

Der nächste Assistentenschritt wird im ausblenden ([klicken Sie hier, um das Bild in voller Größe angezeigt](animating-an-updatepanel-control-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Zurück](changing-an-animation-using-client-side-code-vb.md)
> [Weiter](dynamically-controlling-updatepanel-animations-vb.md)
