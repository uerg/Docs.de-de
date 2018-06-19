---
uid: web-forms/overview/ajax-control-toolkit/animation/dynamically-controlling-updatepanel-animations-cs
title: UpdatePanel-Animationen (c#) dynamisch steuern | Microsoft Docs
author: wenz
description: Animation-Steuerelement in ASP.NET AJAX-Steuerelement-Toolkit ist nicht nur ein Steuerelement, aber eine gesamte Framework Animationen an ein Steuerelement hinzufügen. Für den Inhalt einer...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 5138b8fe-98ff-4e73-a00b-e263fc3ff11d
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/dynamically-controlling-updatepanel-animations-cs
msc.type: authoredcontent
ms.openlocfilehash: 78401ee35027040dffea50c295c752dd182e75a1
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
ms.locfileid: "30868606"
---
<a name="dynamically-controlling-updatepanel-animations-c"></a>UpdatePanel-Animationen dynamisch steuern (c#)
====================
durch [Christian Wenz](https://github.com/wenz)

[Herunterladen von Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/UpdatePanelAnimation2.cs.zip) oder [PDF herunterladen](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/updatepanelanimation2CS.pdf)

> Animation-Steuerelement in ASP.NET AJAX-Steuerelement-Toolkit ist nicht nur ein Steuerelement, aber eine gesamte Framework Animationen an ein Steuerelement hinzufügen. Für den Inhalt eines UpdatePanel, ein spezieller Extender vorhanden ist, die sich stark auf die Animationsframework stützt: UpdatePanelAnimation. Sie können auch zusammen mit UpdatePanel Trigger arbeiten.


## <a name="overview"></a>Übersicht

Animation-Steuerelement in ASP.NET AJAX-Steuerelement-Toolkit ist nicht nur ein Steuerelement, aber eine gesamte Framework Animationen an ein Steuerelement hinzufügen. Für den Inhalt einer `UpdatePanel`, ein spezieller Extender vorhanden ist, die sich stark auf die Animationsframework stützt: `UpdatePanelAnimation`. Es kann auch zusammen mit arbeiten `UpdatePanel` Trigger.

## <a name="steps"></a>Schritte

Der erste Schritt besteht wie gewohnt enthalten die `ScriptManager` auf der Seite, damit die ASP.NET AJAX-Bibliothek geladen wird und die Control-Toolkit verwendet werden können:


[!code-aspx[Main](dynamically-controlling-updatepanel-animations-cs/samples/sample1.aspx)]

Die Animation in diesem Szenario wird an eine Anzeige der aktuellen Uhrzeit angewendet werden. Diese Informationen kann geschrieben werden, in eine Bezeichnung mit der `Page_Load()` -Methode, oder (der Einfachheit halber) die folgenden Inlinecode verwendet wird:


[!code-aspx[Main](dynamically-controlling-updatepanel-animations-cs/samples/sample2.aspx)]

Darüber hinaus wird eine Schaltfläche zum Auslösen der Zeit aktualisiert erstellt:


[!code-aspx[Main](dynamically-controlling-updatepanel-animations-cs/samples/sample3.aspx)]

Dieser Code wird dann in eingefügt die `<ContentTemplate>` Teil einer `UpdatePanel` Element. Im Bereich `UpdateMode` Attribut muss festgelegt werden, um `"Conditional"`, da nur Trigger möglicherweise den Inhalt des Bereichs aktualisiert. In der `<Triggers>` Teil der `UpdatePanel`, asynchroner Postbacks Trigger erstellt und gebunden werden, um die `Click` -Ereignis der Schaltfläche. Daher, wenn der Benutzer auf die Schaltfläche klickt der `UpdatePanel` aktualisiert wird. Hier wird das Markup für die `UpdatePanel` Steuerelement:


[!code-aspx[Main](dynamically-controlling-updatepanel-animations-cs/samples/sample4.aspx)]

Schließlich die `UpdatePanelAnimationExtender` muss so konfiguriert werden: Legen Sie die `TargetControlID` -Attribut auf die ID des Bereichs, und definieren Sie eine Animation in der Extender. Ausblenden im ist sinnvoll, die einen gute visual Schwerpunkt auf die aktualisierte Uhrzeit erstellt. Das Extendermarkup kann dann wie folgt aussehen:


[!code-aspx[Main](dynamically-controlling-updatepanel-animations-cs/samples/sample5.aspx)]

Führen Sie die Datei im Browser. Wenn Sie auf die Schaltfläche klicken, wird die aktuelle Uhrzeit im Bereich für die Dauer von einer Sekunde immer einblenden angezeigt.


[![Die aktuelle Uhrzeit ist einblenden](dynamically-controlling-updatepanel-animations-cs/_static/image2.png)](dynamically-controlling-updatepanel-animations-cs/_static/image1.png)

Die aktuelle Uhrzeit ist einblenden ([klicken Sie hier, um das Bild in voller Größe angezeigt](dynamically-controlling-updatepanel-animations-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Zurück](animating-an-updatepanel-control-cs.md)
> [Weiter](adding-animation-to-a-control-vb.md)
