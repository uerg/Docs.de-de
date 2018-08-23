---
uid: web-forms/overview/ajax-control-toolkit/accordion/dynamically-adding-an-accordion-pane-cs
title: Dynamisches Hinzufügen von einem des Accordion-Bereichs (c#) | Microsoft-Dokumentation
author: wenz
description: Das ' Accordion '-Steuerelement im AJAX Control Toolkit bietet mehrere Bereiche und ermöglicht dem Benutzer eine von ihnen zu einem Zeitpunkt angezeigt. Bereiche werden in der Regel deklariert, w...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 66d88cfa-f26f-46b1-ad52-1c9e03c04a48
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/accordion/dynamically-adding-an-accordion-pane-cs
msc.type: authoredcontent
ms.openlocfilehash: 16cefadb7086a658b20558526757f9229a43fcc9
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2018
ms.locfileid: "41832758"
---
<a name="dynamically-adding-an-accordion-pane-c"></a>Dynamisches Hinzufügen von einem des Accordion-Bereichs (c#)
====================
durch [Christian Wenz](https://github.com/wenz)

[Code herunterladen](http://download.microsoft.com/download/5/6/d/56d50cef-2011-4c8f-9891-7edc6dc57df9/Accordion2.cs.zip) oder [PDF-Datei herunterladen](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/accordion2CS.pdf)

> Das ' Accordion '-Steuerelement im AJAX Control Toolkit bietet mehrere Bereiche und ermöglicht dem Benutzer eine von ihnen zu einem Zeitpunkt angezeigt. Bereiche werden in der Regel auf der Seite selbst deklariert, aber von serverseitigem Code kann verwendet werden, um das gleiche Ergebnis zu erzielen.


## <a name="overview"></a>Übersicht

Das ' Accordion '-Steuerelement im AJAX Control Toolkit bietet mehrere Bereiche und ermöglicht dem Benutzer eine von ihnen zu einem Zeitpunkt angezeigt. Bereiche werden in der Regel auf der Seite selbst deklariert, aber von serverseitigem Code kann verwendet werden, um das gleiche Ergebnis zu erzielen.

## <a name="steps"></a>Schritte

Das Steuerelement ' Accordion ' macht alle wichtige Eigenschaften serverseitiger Code verfügbar. Unter anderem die `Panes` Eigenschaft erhalten Sie Zugriff auf die Auflistung der Bereiche, die die Akkordeon bilden. Jeder Bereich es vom Typ wird `AccordionPane`. Daher ist es einfach, diese einen Bereich zu erstellen:

[!code-csharp[Main](dynamically-adding-an-accordion-pane-cs/samples/sample1.cs)]

Die `HeaderContainer` Eigenschaft `AccordionPane` ermöglicht den Zugriff auf die ASP.NET-Steuerelemente in den Headerbereich des Bereichs; der `ContentContainer` Eigenschaft `AccordionPane` führt den gleichen Wert für den Content-Abschnitt des Bereichs. Dadurch können ASP-Code, um die Bereiche Inhalt hinzuzufügen:

[!code-csharp[Main](dynamically-adding-an-accordion-pane-cs/samples/sample2.cs)]

Abschließend muss die Pane(s) hinzugefügt werden, um die `Panes` Auflistung von der ' Accordion ':

[!code-csharp[Main](dynamically-adding-an-accordion-pane-cs/samples/sample3.cs)]

Hier ist ein vollständige serverseitigen Code, der zwei Bereiche ein Akkordeon-Steuerelement hinzufügt:

[!code-aspx[Main](dynamically-adding-an-accordion-pane-cs/samples/sample4.aspx)]

Das einzige Element ist nicht vorhanden ist, die ' Accordion ' selbst, auf die das Vorhandensein von ASP.NET `ScriptManager` Steuerelement:

[!code-aspx[Main](dynamically-adding-an-accordion-pane-cs/samples/sample5.aspx)]

Um das Beispiel abgeschlossen haben, geben Sie die beiden CSS-Klassen, die im Steuerelement ' Accordion ' auf die verwiesen wird. Informationen zum Schriftschnitt für den Browser:

[!code-css[Main](dynamically-adding-an-accordion-pane-cs/samples/sample6.css)]


[![Die Daten in der ' Accordion ' wurde von serverseitigem Code dynamisch hinzugefügt.](dynamically-adding-an-accordion-pane-cs/_static/image2.png)](dynamically-adding-an-accordion-pane-cs/_static/image1.png)

Die Daten in der ' Accordion ' wurde vom serverseitigen Code dynamisch hinzugefügt ([klicken Sie, um das Bild in voller Größe anzeigen](dynamically-adding-an-accordion-pane-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Zurück](databinding-to-an-accordion-cs.md)
> [Weiter](databinding-to-an-accordion-vb.md)
