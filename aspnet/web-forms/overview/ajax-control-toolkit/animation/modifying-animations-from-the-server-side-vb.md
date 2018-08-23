---
uid: web-forms/overview/ajax-control-toolkit/animation/modifying-animations-from-the-server-side-vb
title: Ändern von Animationen von Serverseite (VB) | Microsoft-Dokumentation
author: wenz
description: Die Animation-Steuerelement in ASP.NET AJAX Control Toolkit ist nicht nur ein Steuerelement, aber ein ganzes Framework Animationen an ein Steuerelement hinzufügen. Es kann auch die Animationen...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: addcf4aa-340a-460b-9c64-506424a1f725
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/modifying-animations-from-the-server-side-vb
msc.type: authoredcontent
ms.openlocfilehash: ef32c8f4846b18f11d816a64a3e4292b67b232e9
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2018
ms.locfileid: "41831339"
---
<a name="modifying-animations-from-the-server-side-vb"></a>Ändern von Animationen von Serverseite (VB)
====================
durch [Christian Wenz](https://github.com/wenz)

[Code herunterladen](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation9.vb.zip) oder [PDF-Datei herunterladen](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation9VB.pdf)

> Die Animation-Steuerelement in ASP.NET AJAX Control Toolkit ist nicht nur ein Steuerelement, aber ein ganzes Framework Animationen an ein Steuerelement hinzufügen. Die Animationen können auch auf der Serverseite geändert werden


## <a name="overview"></a>Übersicht

Die Animation-Steuerelement in ASP.NET AJAX Control Toolkit ist nicht nur ein Steuerelement, aber ein ganzes Framework Animationen an ein Steuerelement hinzufügen. Die Animationen können auch auf der Serverseite geändert werden

## <a name="steps"></a>Schritte

Zunächst einmal sind die `ScriptManager` in die Seite klicken Sie dann die ASP.NET AJAX-Bibliothek wird geladen, lässt sich das Steuerelement-Toolkit verwenden:

[!code-aspx[Main](modifying-animations-from-the-server-side-vb/samples/sample1.aspx)]

Die Animation wird auf einen Bereich des Texts angewendet werden, der so aussieht:

[!code-aspx[Main](modifying-animations-from-the-server-side-vb/samples/sample2.aspx)]

Definieren Sie in der zugehörigen CSS-Klasse für den Bereich eine gute Hintergrundfarbe aus, und auch festlegen Sie eine feste Breite für den Bereich:

[!code-css[Main](modifying-animations-from-the-server-side-vb/samples/sample3.css)]

Der Rest des Codes wird auf der Serverseite ausgeführt und verwendet keine Markup; Stattdessen wird Code zum Erstellen der `AnimationExtender` Steuerelement:

[!code-aspx[Main](modifying-animations-from-the-server-side-vb/samples/sample4.aspx)]

Das Steuerelement-Toolkit bietet derzeit jedoch keine API-Zugriff, um die einzelnen Animationen zu erstellen. Es ist jedoch möglich, legen Sie die `AnimationExtender`die Animations-Eigenschaft in eine Zeichenfolge enthält, des XML-Markups verwendet werden, wenn Sie die Animationen deklarativ zuweisen. Um den XML-Code zu erstellen, die nicht enthalten, muss die `<Animations>` Element können Sie die .NET Framework XML unterstützt, oder, wie im folgenden Code ein, geben Sie einfach die Zeichenfolge:

[!code-vb[Main](modifying-animations-from-the-server-side-vb/samples/sample5.vb)]

Fügen Sie abschließend die `AnimationExtender` die Steuerung an die aktuelle Seite innerhalb der `<form runat="server">` -Element, um sicherzustellen, dass die Animation enthalten ist und ausgeführt wird:

[!code-vb[Main](modifying-animations-from-the-server-side-vb/samples/sample6.vb)]


[![Die Animation wird mithilfe von serverseitiger C#/VB-Code erstellt.](modifying-animations-from-the-server-side-vb/_static/image2.png)](modifying-animations-from-the-server-side-vb/_static/image1.png)

Die Animation wird erstellt, mit serverseitiger C#/VB-Code ([klicken Sie, um das Bild in voller Größe anzeigen](modifying-animations-from-the-server-side-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Zurück](triggering-an-animation-in-another-control-vb.md)
> [Weiter](executing-animations-using-client-side-code-vb.md)
