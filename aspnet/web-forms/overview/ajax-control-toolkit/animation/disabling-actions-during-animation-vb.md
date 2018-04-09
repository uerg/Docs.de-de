---
uid: web-forms/overview/ajax-control-toolkit/animation/disabling-actions-during-animation-vb
title: Deaktivieren Aktionen während der Animation (VB) | Microsoft Docs
author: wenz
description: Animation-Steuerelement in ASP.NET AJAX-Steuerelement-Toolkit ist nicht nur ein Steuerelement, aber eine gesamte Framework Animationen an ein Steuerelement hinzufügen. Darüber hinaus wird die Aktion unterstützt...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: a86c0276-6481-46ee-8b4f-8c2009399ee9
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/disabling-actions-during-animation-vb
msc.type: authoredcontent
ms.openlocfilehash: 9e2a0517800e90788bb67c1d75482a3d9340674b
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
---
<a name="disabling-actions-during-animation-vb"></a>Deaktivieren Aktionen während der Animation (VB)
====================
durch [Christian Wenz](https://github.com/wenz)

[Herunterladen von Code](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation7.vb.zip) oder [PDF herunterladen](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation7VB.pdf)

> Animation-Steuerelement in ASP.NET AJAX-Steuerelement-Toolkit ist nicht nur ein Steuerelement, aber eine gesamte Framework Animationen an ein Steuerelement hinzufügen. Darüber hinaus unterstützt Aktionen wie Mausklicks. Wenn ein Mausklick eine Animation gestartet wird, ist es jedoch wünschenswert, Mausklicks während der Animation deaktivieren.


## <a name="overview"></a>Übersicht

Animation-Steuerelement in ASP.NET AJAX-Steuerelement-Toolkit ist nicht nur ein Steuerelement, aber eine gesamte Framework Animationen an ein Steuerelement hinzufügen. Darüber hinaus unterstützt Aktionen wie Mausklicks. Wenn ein Mausklick eine Animation gestartet wird, ist es jedoch wünschenswert, Mausklicks während der Animation deaktivieren.

## <a name="steps"></a>Schritte

Erstens sind die `ScriptManager` auf der Seite; klicken Sie dann die ASP.NET AJAX-Bibliothek geladen ist, wodurch das Steuerelement-Toolkit verwenden:

[!code-aspx[Main](disabling-actions-during-animation-vb/samples/sample1.aspx)]

Die Animation wird in eine HTML-Schaltfläche wie folgt angewendet:

[!code-aspx[Main](disabling-actions-during-animation-vb/samples/sample2.aspx)]

Beachten Sie, dass ein HTML-Steuerelement, statt ein Websteuerelement verwendet wird, da wir nicht, dass die Schaltfläche zum Erstellen eines Postbacks möchten. Es werden nur die Animation des clientseitigen für uns starten.

Fügen Sie dann die `AnimationExtender` auf der Seite "Bereitstellen einer `ID`, die `TargetControlID` Attribut und der Auswahlparameter `runat="server"`:

[!code-aspx[Main](disabling-actions-during-animation-vb/samples/sample3.aspx)]

Innerhalb der `<Animations>` Knoten `<OnClick>` wird von der richtigen Elemente für die Maus zu behandeln. Allerdings kann der geklickt während der Animation. Die `<EnableAction>` Element dieser kümmern. Festlegen von `Enabled="false"` deaktiviert die Schaltfläche mit den im Rahmen der Animation. Da es mehrere einzelne Animationen (deaktivieren die Schaltfläche und die tatsächliche Animationen), verwenden die `<Parallel>` Element ist erforderlich, um Kleben Sie zusammen in einem einzelnen Animationen. Hier wird das vollständige Markup für `AnimationExtender`:

[!code-aspx[Main](disabling-actions-during-animation-vb/samples/sample4.aspx)]

Es wäre auch möglich, auf die Schaltfläche zum nach der Animation, verwenden das folgende XML-Element am Ende der Liste erneut zu aktivieren:

[!code-xml[Main](disabling-actions-during-animation-vb/samples/sample5.xml)]

Jedoch im vorliegenden Szenario nutzlos seit der Schaltfläche wäre dies ausgeblendet und nicht am Ende der Animation sichtbar ist.


[![Die Schaltfläche ist deaktiviert, sobald die Animation ausgeführt wird.](disabling-actions-during-animation-vb/_static/image2.png)](disabling-actions-during-animation-vb/_static/image1.png)

Die Schaltfläche ist deaktiviert, sobald die Animation ausgeführt wird ([klicken Sie hier, um das Bild in voller Größe angezeigt](disabling-actions-during-animation-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Zurück](animating-in-response-to-user-interaction-vb.md)
> [Weiter](triggering-an-animation-in-another-control-vb.md)
