---
uid: web-forms/overview/ajax-control-toolkit/accordion/dynamically-adding-an-accordion-pane-vb
title: "Dynamisches Hinzufügen von einem Akkordeon Bereich (VB) | Microsoft Docs"
author: wenz
description: "Das Steuerelement ' Accordion ' im AJAX-Steuerelement-Toolkit enthält mehrere Bereiche und ermöglicht es dem Benutzer eines davon zu einem Zeitpunkt angezeigt. Bereiche werden in der Regel w deklariert..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: fae968c9-1902-487d-b053-86a46dd52c3f
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/accordion/dynamically-adding-an-accordion-pane-vb
msc.type: authoredcontent
ms.openlocfilehash: 6adc0b7985e5ba3da1684b645ae1b71b5112594a
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="dynamically-adding-an-accordion-pane-vb"></a>Dynamisches Hinzufügen von einem Akkordeon Bereich (VB)
====================
durch [Christian Wenz](https://github.com/wenz)

[Herunterladen von Code](http://download.microsoft.com/download/5/6/d/56d50cef-2011-4c8f-9891-7edc6dc57df9/Accordion2.vb.zip) oder [PDF herunterladen](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/accordion2VB.pdf)

> Das Steuerelement ' Accordion ' im AJAX-Steuerelement-Toolkit enthält mehrere Bereiche und ermöglicht es dem Benutzer eines davon zu einem Zeitpunkt angezeigt. Bereiche werden in der Regel auf der Seite selbst deklariert, aber serverseitigen Code kann verwendet werden, um das gleiche Ergebnis zu erzielen.


## <a name="overview"></a>Übersicht

Das Steuerelement ' Accordion ' im AJAX-Steuerelement-Toolkit enthält mehrere Bereiche und ermöglicht es dem Benutzer eines davon zu einem Zeitpunkt angezeigt. Bereiche werden in der Regel auf der Seite selbst deklariert, aber serverseitigen Code kann verwendet werden, um das gleiche Ergebnis zu erzielen.

## <a name="steps"></a>Schritte

Das Steuerelement ' Accordion ' macht alle wichtige Eigenschaften mit serverseitigen Code verfügbar. Unter anderem die `Panes` Eigenschaft gewährt Zugriff auf die Auflistung der Bereiche, die die ' Accordion ' bilden. Jeder Bereich es vom Typ ist `AccordionPane`. Es ist daher einfach einen Bereich im erstellen:

[!code-vb[Main](dynamically-adding-an-accordion-pane-vb/samples/sample1.vb)]

Die `HeaderContainer` Eigenschaft `AccordionPane` ermöglicht den Zugriff auf den ASP.NET-Steuerelementen innerhalb der Headerabschnitt eines Bereich; die `ContentContainer` Eigenschaft `AccordionPane` erfüllt den gleichen Wert für den Inhaltsabschnitt des Bereichs. Dadurch können ASP-Code, um die Bereiche Inhalt hinzuzufügen:

[!code-vb[Main](dynamically-adding-an-accordion-pane-vb/samples/sample2.vb)]

Schließlich muss die Pane(s) hinzugefügt werden, um die `Panes` Auflistung von der ' Accordion ':

[!code-vb[Main](dynamically-adding-an-accordion-pane-vb/samples/sample3.vb)]

Hier ist ein vollständige serverseitigen Code, der zwei Bereiche auf das Steuerelement ' Accordion ' hinzufügt:

[!code-aspx[Main](dynamically-adding-an-accordion-pane-vb/samples/sample4.aspx)]

Das einzige fehlende Element ist das ' Accordion ' selbst, abhängig von der Anwesenheit von ASP.NET `ScriptManager` Steuerelement:

[!code-aspx[Main](dynamically-adding-an-accordion-pane-vb/samples/sample5.aspx)]

Um das Beispiel fertig sind, enthalten die beiden CSS-Klassen verwiesen wird, in das Steuerelement ' Accordion ' Formatinformationen für den Browser:

[!code-css[Main](dynamically-adding-an-accordion-pane-vb/samples/sample6.css)]


[![Die Daten in der ' Accordion ' wurde vom serverseitigen Code dynamisch hinzugefügt.](dynamically-adding-an-accordion-pane-vb/_static/image2.png)](dynamically-adding-an-accordion-pane-vb/_static/image1.png)

Die Daten in der ' Accordion ' wurde vom serverseitigen Code dynamisch hinzugefügt ([klicken Sie hier, um das Bild in voller Größe angezeigt](dynamically-adding-an-accordion-pane-vb/_static/image3.png))

>[!div class="step-by-step"]
[Zurück](databinding-to-an-accordion-vb.md)
