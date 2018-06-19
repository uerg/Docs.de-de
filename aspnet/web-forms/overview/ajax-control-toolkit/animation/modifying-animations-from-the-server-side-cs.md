---
uid: web-forms/overview/ajax-control-toolkit/animation/modifying-animations-from-the-server-side-cs
title: Ändern von Animationen von der Serverseite (c#) | Microsoft Docs
author: wenz
description: Animation-Steuerelement in ASP.NET AJAX-Steuerelement-Toolkit ist nicht nur ein Steuerelement, aber eine gesamte Framework Animationen an ein Steuerelement hinzufügen. Es kann auch die Animationen an...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: b0abec39-a1c9-422d-ba9a-ef16f6185af8
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/modifying-animations-from-the-server-side-cs
msc.type: authoredcontent
ms.openlocfilehash: 6946875552c885ffb1f2a2eb7e728b85d7dd3973
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
ms.locfileid: "30869009"
---
<a name="modifying-animations-from-the-server-side-c"></a>Ändern von Animationen von der Serverseite (c#)
====================
durch [Christian Wenz](https://github.com/wenz)

[Herunterladen von Code](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation9.cs.zip) oder [PDF herunterladen](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation9CS.pdf)

> Animation-Steuerelement in ASP.NET AJAX-Steuerelement-Toolkit ist nicht nur ein Steuerelement, aber eine gesamte Framework Animationen an ein Steuerelement hinzufügen. Die Animationen möglicherweise auch auf der Serverseite geändert werden


## <a name="overview"></a>Übersicht

Animation-Steuerelement in ASP.NET AJAX-Steuerelement-Toolkit ist nicht nur ein Steuerelement, aber eine gesamte Framework Animationen an ein Steuerelement hinzufügen. Die Animationen möglicherweise auch auf der Serverseite geändert werden

## <a name="steps"></a>Schritte

Erstens sind die `ScriptManager` auf der Seite; klicken Sie dann die ASP.NET AJAX-Bibliothek geladen ist, wodurch das Steuerelement-Toolkit verwenden:

[!code-aspx[Main](modifying-animations-from-the-server-side-cs/samples/sample1.aspx)]

Die Animation wird auf einen Bereich des Texts angewendet werden, die wie folgt aussieht:

[!code-aspx[Main](modifying-animations-from-the-server-side-cs/samples/sample2.aspx)]

Definieren Sie in der zugehörigen CSS-Klasse für den Bereich eine gute Hintergrundfarbe und auch festlegen Sie eine feste Breite für den Bereich:

[!code-css[Main](modifying-animations-from-the-server-side-cs/samples/sample3.css)]

Der Rest des Codes auf der Serverseite ausgeführt wird und Markup nicht verwendet. Stattdessen wird Code zum Erstellen der `AnimationExtender` Steuerelement:

[!code-aspx[Main](modifying-animations-from-the-server-side-cs/samples/sample4.aspx)]

Das Steuerelement-Toolkit bietet derzeit jedoch kein API-Zugriff, um die einzelnen Animationen erstellen. Es ist jedoch möglich ist, legen Sie die `AnimationExtender`die Animationen-Eigenschaft, um eine Zeichenfolge enthält, des XML-Markups verwendet, wenn die Animationen deklarativ zuweisen. Um den XML-Code erstellen, die nicht enthalten darf der `<Animations>` können Sie die .NET Framework-XML-Element unterstützen, oder geben Sie wie im folgenden Code nur die Zeichenfolge:

[!code-css[Main](modifying-animations-from-the-server-side-cs/samples/sample5.css)]

Fügen Sie schließlich die `AnimationExtender` innerhalb der aktuellen Seite steuern die `<form runat="server">` -Element, und stellen Sie sicher, dass die Animation enthalten ist und ausgeführt wird:

[!code-html[Main](modifying-animations-from-the-server-side-cs/samples/sample6.html)]


[![Die Animation wird mit serverseitiger C#/VB-Code erstellt.](modifying-animations-from-the-server-side-cs/_static/image2.png)](modifying-animations-from-the-server-side-cs/_static/image1.png)

Die Animation wird mithilfe von serverseitiger C#/VB-Code erstellt ([klicken Sie hier, um das Bild in voller Größe angezeigt](modifying-animations-from-the-server-side-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Zurück](triggering-an-animation-in-another-control-cs.md)
> [Weiter](executing-animations-using-client-side-code-cs.md)
