---
uid: web-forms/overview/ajax-control-toolkit/animation/changing-an-animation-using-client-side-code-vb
title: Ändern einer Animation mit einer clientseitigen Code (VB) | Microsoft Docs
author: wenz
description: Animation-Steuerelement in ASP.NET AJAX-Steuerelement-Toolkit ist nicht nur ein Steuerelement, aber eine gesamte Framework Animationen an ein Steuerelement hinzufügen. Es können auch die Animation...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: a7fe5de5-a964-4780-ae5e-70821dfb50a0
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/changing-an-animation-using-client-side-code-vb
msc.type: authoredcontent
ms.openlocfilehash: 7f9b72576cc3a9e91827cfb40983821704621060
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
ms.locfileid: "30879152"
---
<a name="changing-an-animation-using-client-side-code-vb"></a>Ändern einer Animation mit einer clientseitigen Code (VB)
====================
durch [Christian Wenz](https://github.com/wenz)

[Herunterladen von Code](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation11.vb.zip) oder [PDF herunterladen](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation11VB.pdf)

> Animation-Steuerelement in ASP.NET AJAX-Steuerelement-Toolkit ist nicht nur ein Steuerelement, aber eine gesamte Framework Animationen an ein Steuerelement hinzufügen. Die Animation kann auch mit benutzerdefinierten clientseitigen JavaScript-Code geändert werden.


## <a name="overview"></a>Übersicht

Animation-Steuerelement in ASP.NET AJAX-Steuerelement-Toolkit ist nicht nur ein Steuerelement, aber eine gesamte Framework Animationen an ein Steuerelement hinzufügen. Die Animation kann auch mit benutzerdefinierten clientseitigen JavaScript-Code geändert werden.

## <a name="steps"></a>Schritte

Erstens sind die `ScriptManager` auf der Seite; klicken Sie dann die ASP.NET AJAX-Bibliothek geladen ist, wodurch das Steuerelement-Toolkit verwenden:

[!code-aspx[Main](changing-an-animation-using-client-side-code-vb/samples/sample1.aspx)]

Die Animation wird auf einen Bereich des Texts angewendet werden, die wie folgt aussieht:

[!code-aspx[Main](changing-an-animation-using-client-side-code-vb/samples/sample2.aspx)]

Definieren Sie in der zugehörigen CSS-Klasse für den Bereich eine gute Hintergrundfarbe und auch festlegen Sie eine feste Breite für den Bereich:

[!code-css[Main](changing-an-animation-using-client-side-code-vb/samples/sample3.css)]

Die tatsächliche Animation wird durch eine HTML-Schaltfläche gestartet:

[!code-aspx[Main](changing-an-animation-using-client-side-code-vb/samples/sample4.aspx)]

Fügen Sie dann die `AnimationExtender` auf der Seite "Bereitstellen einer `ID`, die `TargetControlID` Attribut und der Auswahlparameter `runat="server"`:

[!code-aspx[Main](changing-an-animation-using-client-side-code-vb/samples/sample5.aspx)]

Beachten Sie, dass es keine `<Animations>` Knoten innerhalb der `AnimationExtender` Steuerelement. Benutzerdefinierte JavaScript-Code dient zum Bereitstellen von Animationen mit dem Steuerelement verwendet werden soll.

Wie bei der Server-API von `AnimationExtender`, es ist keine einfache Möglichkeit, eine Animation der Extender noch zuweisen. Mit den verschiedenen Ereignissen registriert jedoch die Extender zur Verfügung stellt mehrere Methoden zum Lesen und Schreiben von Animationen (`OnClick`, `OnLoad`usw.). Hier einige Beispiele:

- `get_OnClick()`
- `set_OnClick()`
- `get_OnLoad()`
- `set_OnLoad()`
- `...`

Das Format des Rückgabewerts der der `get_*()` Funktionen und das Format des Arguments für die `set_*()` Funktionen ist eine JSON-Zeichenfolge, die eine objektdarstellung wäre dies das XML-Markup bereitstellen. Aktuell besteht keine Möglichkeit, um ein Objekt im, aber es ist möglich, ein Objekt aus einer angegebenen Animation lesen (`get_OnXXXBehavior()` Methoden).

Hier ist eine JSON-Zeichenfolge (ohne die begrenzenden Anführungszeichen und ordentlich formatierte), die eine Animation, die ausgelöst wird, indem Sie die Schaltfläche darstellt, jedoch im Bereich durch Ändern der Größe und gleichzeitig ausblenden animieren:

[!code-json[Main](changing-an-animation-using-client-side-code-vb/samples/sample6.json)]

Der folgende JavaScript-Code weist diese JSON descripting auf die `OnClick` Animation des aktuellen Extenders und führt sie:

[!code-html[Main](changing-an-animation-using-client-side-code-vb/samples/sample7.html)]


[![Die Animation wird sofort ausgeführt, ohne einen Mausklick (und mit wenig Markup)](changing-an-animation-using-client-side-code-vb/_static/image2.png)](changing-an-animation-using-client-side-code-vb/_static/image1.png)

Die Animation wird sofort ausgeführt, ohne einen Mausklick (und mit wenig Markup) ([klicken Sie hier, um das Bild in voller Größe angezeigt](changing-an-animation-using-client-side-code-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Zurück](executing-animations-using-client-side-code-vb.md)
> [Weiter](animating-an-updatepanel-control-vb.md)
