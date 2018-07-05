---
uid: web-forms/overview/ajax-control-toolkit/animation/modifying-animations-from-the-server-side-cs
title: Ändern von Animationen von Serverseite (c#) | Microsoft-Dokumentation
author: wenz
description: Die Animation-Steuerelement in ASP.NET AJAX Control Toolkit ist nicht nur ein Steuerelement, aber ein ganzes Framework Animationen an ein Steuerelement hinzufügen. Es kann auch die Animationen...
ms.author: aspnetcontent
ms.date: 06/02/2008
ms.assetid: b0abec39-a1c9-422d-ba9a-ef16f6185af8
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/modifying-animations-from-the-server-side-cs
msc.type: authoredcontent
ms.openlocfilehash: 8954f025873ea553fde26c6e2330ce6e5be2b539
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2018
ms.locfileid: "37804053"
---
<a name="modifying-animations-from-the-server-side-c"></a>Ändern von Animationen von Serverseite (c#)
====================
durch [Christian Wenz](https://github.com/wenz)

[Code herunterladen](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation9.cs.zip) oder [PDF-Datei herunterladen](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation9CS.pdf)

> Die Animation-Steuerelement in ASP.NET AJAX Control Toolkit ist nicht nur ein Steuerelement, aber ein ganzes Framework Animationen an ein Steuerelement hinzufügen. Die Animationen können auch auf der Serverseite geändert werden


## <a name="overview"></a>Übersicht

Die Animation-Steuerelement in ASP.NET AJAX Control Toolkit ist nicht nur ein Steuerelement, aber ein ganzes Framework Animationen an ein Steuerelement hinzufügen. Die Animationen können auch auf der Serverseite geändert werden

## <a name="steps"></a>Schritte

Zunächst einmal sind die `ScriptManager` in die Seite klicken Sie dann die ASP.NET AJAX-Bibliothek wird geladen, lässt sich das Steuerelement-Toolkit verwenden:

[!code-aspx[Main](modifying-animations-from-the-server-side-cs/samples/sample1.aspx)]

Die Animation wird auf einen Bereich des Texts angewendet werden, der so aussieht:

[!code-aspx[Main](modifying-animations-from-the-server-side-cs/samples/sample2.aspx)]

Definieren Sie in der zugehörigen CSS-Klasse für den Bereich eine gute Hintergrundfarbe aus, und auch festlegen Sie eine feste Breite für den Bereich:

[!code-css[Main](modifying-animations-from-the-server-side-cs/samples/sample3.css)]

Der Rest des Codes wird auf der Serverseite ausgeführt und verwendet keine Markup; Stattdessen wird Code zum Erstellen der `AnimationExtender` Steuerelement:

[!code-aspx[Main](modifying-animations-from-the-server-side-cs/samples/sample4.aspx)]

Das Steuerelement-Toolkit bietet derzeit jedoch keine API-Zugriff, um die einzelnen Animationen zu erstellen. Es ist jedoch möglich, legen Sie die `AnimationExtender`die Animations-Eigenschaft in eine Zeichenfolge enthält, des XML-Markups verwendet werden, wenn Sie die Animationen deklarativ zuweisen. Um den XML-Code zu erstellen, die nicht enthalten, muss die `<Animations>` Element können Sie die .NET Framework XML unterstützt, oder, wie im folgenden Code ein, geben Sie einfach die Zeichenfolge:

[!code-css[Main](modifying-animations-from-the-server-side-cs/samples/sample5.css)]

Fügen Sie abschließend die `AnimationExtender` die Steuerung an die aktuelle Seite innerhalb der `<form runat="server">` -Element, um sicherzustellen, dass die Animation enthalten ist und ausgeführt wird:

[!code-html[Main](modifying-animations-from-the-server-side-cs/samples/sample6.html)]


[![Die Animation wird mithilfe von serverseitiger C#/VB-Code erstellt.](modifying-animations-from-the-server-side-cs/_static/image2.png)](modifying-animations-from-the-server-side-cs/_static/image1.png)

Die Animation wird erstellt, mit serverseitiger C#/VB-Code ([klicken Sie, um das Bild in voller Größe anzeigen](modifying-animations-from-the-server-side-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Zurück](triggering-an-animation-in-another-control-cs.md)
> [Weiter](executing-animations-using-client-side-code-cs.md)
