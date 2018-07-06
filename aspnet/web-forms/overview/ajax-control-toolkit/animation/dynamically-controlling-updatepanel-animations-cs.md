---
uid: web-forms/overview/ajax-control-toolkit/animation/dynamically-controlling-updatepanel-animations-cs
title: Dynamisches Steuern von UpdatePanel-Animationen (c#) | Microsoft-Dokumentation
author: wenz
description: Die Animation-Steuerelement in ASP.NET AJAX Control Toolkit ist nicht nur ein Steuerelement, aber ein ganzes Framework Animationen an ein Steuerelement hinzufügen. Für den Inhalt einer...
ms.author: aspnetcontent
ms.date: 06/02/2008
ms.assetid: 5138b8fe-98ff-4e73-a00b-e263fc3ff11d
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/dynamically-controlling-updatepanel-animations-cs
msc.type: authoredcontent
ms.openlocfilehash: c55381548b00866c4e4fc9244392c00af201f62a
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2018
ms.locfileid: "37822602"
---
<a name="dynamically-controlling-updatepanel-animations-c"></a>Dynamisches Steuern von UpdatePanel-Animationen (c#)
====================
durch [Christian Wenz](https://github.com/wenz)

[Code herunterladen](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/UpdatePanelAnimation2.cs.zip) oder [PDF-Datei herunterladen](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/updatepanelanimation2CS.pdf)

> Die Animation-Steuerelement in ASP.NET AJAX Control Toolkit ist nicht nur ein Steuerelement, aber ein ganzes Framework Animationen an ein Steuerelement hinzufügen. Für den Inhalt von einem UpdatePanel-Steuerelement, ein spezieller Extender vorhanden ist, die stark auf die Animationsframework basieren: UpdatePanelAnimation. Sie können auch zusammen mit UpdatePanel-Triggern arbeiten.


## <a name="overview"></a>Übersicht

Die Animation-Steuerelement in ASP.NET AJAX Control Toolkit ist nicht nur ein Steuerelement, aber ein ganzes Framework Animationen an ein Steuerelement hinzufügen. Für den Inhalt einer `UpdatePanel`, ein spezieller Extender vorhanden ist, die sehr auf die Animationsframework basiert: `UpdatePanelAnimation`. Sie können auch arbeiten zusammen mit `UpdatePanel` Trigger.

## <a name="steps"></a>Schritte

Der erste Schritt ist wie üblich, enthalten die `ScriptManager` auf der Seite, damit die ASP.NET AJAX-Bibliothek geladen wird und das Steuerelement-Toolkit verwendet werden kann:


[!code-aspx[Main](dynamically-controlling-updatepanel-animations-cs/samples/sample1.aspx)]

Die Animation in diesem Szenario wird an eine Anzeige der aktuellen Zeit angewendet werden. Diese Informationen kann geschrieben werden, in eine Bezeichnung mit der `Page_Load()` -Methode, oder (aus Gründen der Einfachheit) den folgenden Inlinecode verwendet wird:


[!code-aspx[Main](dynamically-controlling-updatepanel-animations-cs/samples/sample2.aspx)]

Darüber hinaus wird eine Schaltfläche ausgelöst wird, aktualisieren die Zeit erstellt:


[!code-aspx[Main](dynamically-controlling-updatepanel-animations-cs/samples/sample3.aspx)]

Dieser Code wird dann in eingefügt der `<ContentTemplate>` Teil einer `UpdatePanel` Element. Der Bereich `UpdateMode` Attribut muss festgelegt werden, um `"Conditional"`, da nur Trigger, den Inhalt des Bereichs aktualisieren können. In der `<Triggers>` Teil der `UpdatePanel`, asynchroner Postbacktrigger erstellt und gebunden werden, um die `Click` -Ereignis der Schaltfläche. Also, wenn der Benutzer auf die Schaltfläche klickt der `UpdatePanel` aktualisiert wird. Dies ist das Markup für die `UpdatePanel` Steuerelement:


[!code-aspx[Main](dynamically-controlling-updatepanel-animations-cs/samples/sample4.aspx)]

Schließlich die `UpdatePanelAnimationExtender` muss so konfiguriert werden: Legen Sie die `TargetControlID` -Attribut auf die ID des Bereichs, und definieren Sie eine Animation in der Extender. Überblenden in macht Sinn, die einen gute visual Schwerpunkt auf die aktualisierte Zeit erstellt. Das Extendermarkup kann dann wie folgt aussehen:


[!code-aspx[Main](dynamically-controlling-updatepanel-animations-cs/samples/sample5.aspx)]

Führen Sie die Datei im Browser. Wenn Sie auf die Schaltfläche klicken, wird die aktuelle Uhrzeit im Bereich immer für die Dauer von einer Sekunde mit im Verlauf angezeigt.


[![Die aktuelle Uhrzeit ist mit im Verlauf.](dynamically-controlling-updatepanel-animations-cs/_static/image2.png)](dynamically-controlling-updatepanel-animations-cs/_static/image1.png)

Die aktuelle Uhrzeit wird eingeblendet, ([klicken Sie, um das Bild in voller Größe anzeigen](dynamically-controlling-updatepanel-animations-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Zurück](animating-an-updatepanel-control-cs.md)
> [Weiter](adding-animation-to-a-control-vb.md)
