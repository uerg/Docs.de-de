---
uid: web-forms/overview/ajax-control-toolkit/accordion/dynamically-adding-an-accordion-pane-vb
title: Dynamisches Hinzufügen von einem des Accordion-Bereichs (VB) | Microsoft-Dokumentation
author: wenz
description: Das ' Accordion '-Steuerelement im AJAX Control Toolkit bietet mehrere Bereiche und ermöglicht dem Benutzer eine von ihnen zu einem Zeitpunkt angezeigt. Bereiche werden in der Regel deklariert, w...
ms.author: aspnetcontent
ms.date: 06/02/2008
ms.assetid: fae968c9-1902-487d-b053-86a46dd52c3f
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/accordion/dynamically-adding-an-accordion-pane-vb
msc.type: authoredcontent
ms.openlocfilehash: 1fdb95ee1ee93bc011a257e4e21c876dbbc7d2a9
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2018
ms.locfileid: "37820025"
---
<a name="dynamically-adding-an-accordion-pane-vb"></a>Dynamisches Hinzufügen von einem des Accordion-Bereichs (VB)
====================
durch [Christian Wenz](https://github.com/wenz)

[Code herunterladen](http://download.microsoft.com/download/5/6/d/56d50cef-2011-4c8f-9891-7edc6dc57df9/Accordion2.vb.zip) oder [PDF-Datei herunterladen](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/accordion2VB.pdf)

> Das ' Accordion '-Steuerelement im AJAX Control Toolkit bietet mehrere Bereiche und ermöglicht dem Benutzer eine von ihnen zu einem Zeitpunkt angezeigt. Bereiche werden in der Regel auf der Seite selbst deklariert, aber von serverseitigem Code kann verwendet werden, um das gleiche Ergebnis zu erzielen.


## <a name="overview"></a>Übersicht

Das ' Accordion '-Steuerelement im AJAX Control Toolkit bietet mehrere Bereiche und ermöglicht dem Benutzer eine von ihnen zu einem Zeitpunkt angezeigt. Bereiche werden in der Regel auf der Seite selbst deklariert, aber von serverseitigem Code kann verwendet werden, um das gleiche Ergebnis zu erzielen.

## <a name="steps"></a>Schritte

Das Steuerelement ' Accordion ' macht alle wichtige Eigenschaften serverseitiger Code verfügbar. Unter anderem die `Panes` Eigenschaft erhalten Sie Zugriff auf die Auflistung der Bereiche, die die Akkordeon bilden. Jeder Bereich es vom Typ wird `AccordionPane`. Daher ist es einfach, diese einen Bereich zu erstellen:

[!code-vb[Main](dynamically-adding-an-accordion-pane-vb/samples/sample1.vb)]

Die `HeaderContainer` Eigenschaft `AccordionPane` ermöglicht den Zugriff auf die ASP.NET-Steuerelemente in den Headerbereich des Bereichs; der `ContentContainer` Eigenschaft `AccordionPane` führt den gleichen Wert für den Content-Abschnitt des Bereichs. Dadurch können ASP-Code, um die Bereiche Inhalt hinzuzufügen:

[!code-vb[Main](dynamically-adding-an-accordion-pane-vb/samples/sample2.vb)]

Abschließend muss die Pane(s) hinzugefügt werden, um die `Panes` Auflistung von der ' Accordion ':

[!code-vb[Main](dynamically-adding-an-accordion-pane-vb/samples/sample3.vb)]

Hier ist ein vollständige serverseitigen Code, der zwei Bereiche ein Akkordeon-Steuerelement hinzufügt:

[!code-aspx[Main](dynamically-adding-an-accordion-pane-vb/samples/sample4.aspx)]

Das einzige Element ist nicht vorhanden ist, die ' Accordion ' selbst, auf die das Vorhandensein von ASP.NET `ScriptManager` Steuerelement:

[!code-aspx[Main](dynamically-adding-an-accordion-pane-vb/samples/sample5.aspx)]

Um das Beispiel abgeschlossen haben, geben Sie die beiden CSS-Klassen, die im Steuerelement ' Accordion ' auf die verwiesen wird. Informationen zum Schriftschnitt für den Browser:

[!code-css[Main](dynamically-adding-an-accordion-pane-vb/samples/sample6.css)]


[![Die Daten in der ' Accordion ' wurde von serverseitigem Code dynamisch hinzugefügt.](dynamically-adding-an-accordion-pane-vb/_static/image2.png)](dynamically-adding-an-accordion-pane-vb/_static/image1.png)

Die Daten in der ' Accordion ' wurde vom serverseitigen Code dynamisch hinzugefügt ([klicken Sie, um das Bild in voller Größe anzeigen](dynamically-adding-an-accordion-pane-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Vorherige](databinding-to-an-accordion-vb.md)
