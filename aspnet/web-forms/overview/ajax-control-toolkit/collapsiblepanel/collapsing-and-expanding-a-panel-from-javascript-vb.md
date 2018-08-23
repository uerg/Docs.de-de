---
uid: web-forms/overview/ajax-control-toolkit/collapsiblepanel/collapsing-and-expanding-a-panel-from-javascript-vb
title: Reduzieren und Erweitern eines Bereichs über JavaScript (VB) | Microsoft-Dokumentation
author: wenz
description: Das CollapsiblePanel-Steuerelement in ASP.NET AJAX Control Toolkit einen Bereich erweitert und bietet die Möglichkeit, seinen Inhalt zu reduzieren und erweitern ihn ein...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 298789b4-2964-49f5-a0a8-d4dbeb9ff2c2
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/collapsiblepanel/collapsing-and-expanding-a-panel-from-javascript-vb
msc.type: authoredcontent
ms.openlocfilehash: 1f80a6979966a887db0557b4f1b98570e10b1ab7
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2018
ms.locfileid: "41832210"
---
<a name="collapsing-and-expanding-a-panel-from-javascript-vb"></a>Reduzieren und Erweitern eines Bereichs über JavaScript (VB)
====================
durch [Christian Wenz](https://github.com/wenz)

[Code herunterladen](http://download.microsoft.com/download/8/a/a/8aab3c3e-de6f-463f-805c-5fda567eef6e/CollapsiblePanel1.vb.zip) oder [PDF-Datei herunterladen](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/collapsiblepanel1VB.pdf)

> Das CollapsiblePanel-Steuerelement in ASP.NET AJAX Control Toolkit einen Bereich erweitert und bietet die Möglichkeit, dessen Inhalt reduzieren und erweitern es noch mal. Diese beiden Aktionen können auch mit benutzerdefinierten JavaScript-Code ausgelöst werden.


## <a name="overview"></a>Übersicht

Das CollapsiblePanel-Steuerelement in ASP.NET AJAX Control Toolkit einen Bereich erweitert und bietet die Möglichkeit, dessen Inhalt reduzieren und erweitern es noch mal. Diese beiden Aktionen können auch mit benutzerdefinierten JavaScript-Code ausgelöst werden.

## <a name="steps"></a>Schritte

Zunächst erstellen Sie eine neue ASP.NET-Seite und enthalten die `ScriptManager` innerhalb der `<form>` Element. Dies lädt die ASP.NET AJAX-Bibliothek, die für das Steuerelement-Toolkit erforderlich ist:

[!code-aspx[Main](collapsing-and-expanding-a-panel-from-javascript-vb/samples/sample1.aspx)]

Erstellen Sie Sie dann ein Panel mit Text, damit die Auswirkungen Reduzieren/Erweitern angezeigt werden:

[!code-aspx[Main](collapsing-and-expanding-a-panel-from-javascript-vb/samples/sample2.aspx)]

Wie Sie sehen können, verweist der Bereich eine CSS-Klasse, die wird hier gezeigt (und werden im Grunde eine Hintergrundfarbe und des Bereichs Breite definiert):

[!code-css[Main](collapsing-and-expanding-a-panel-from-javascript-vb/samples/sample3.css)]

Die `CollapsiblePanelExtender` Steuerelement erfordert die `TargetControlID` Attribut, damit das Toolkit weiß, welchen Bereich reduzieren oder Erweitern auf Anfrage:

[!code-aspx[Main](collapsing-and-expanding-a-panel-from-javascript-vb/samples/sample4.aspx)]

Leider wird der Extender derzeit macht eine bestimmte API für den Bereich zu erweitern oder reduzieren, aber einige nicht dokumentierten Methoden erfolgt. Fügen Sie zunächst auf die Seite, die dann auslösen, wird die clientseitige JavaScript-Code zum Reduzieren oder erweitern den Inhalt des Bereichs drei HTML-Schaltflächen hinzu:

[!code-aspx[Main](collapsing-and-expanding-a-panel-from-javascript-vb/samples/sample5.aspx)]

Im clientseitigen JavaScript-Code (Einstieg `<script type="text/javascript">`), wird die `$find()` -Methode verwendet werden, muss für den Zugriff auf die `CollapsiblePanelExtender`. `$find("cpe")` Gibt einen Verweis darauf zurück. Von dort auf bestimmte Methoden lösen die aktuelle Aufgabe.

Die Methode zum Öffnen von (Erweitert) im Bereich heißt `_doOpen()`; der folgende code implementiert die `doOpen()` Funktion aufgerufen, wenn die erste Schaltfläche geklickt wird:

[!code-javascript[Main](collapsing-and-expanding-a-panel-from-javascript-vb/samples/sample6.js)]

Zum Schließen, oder reduzieren im Bereich der `_doClose()` -Methode ausgeführt werden muss. Wenn der Benutzer auf der zweiten Schaltfläche klickt, wird also der folgende JavaScript-Code aufgerufen:

[!code-javascript[Main](collapsing-and-expanding-a-panel-from-javascript-vb/samples/sample7.js)]

Die dritte Schaltfläche Schaltet den Zustand des Bereichs: aus reduziert, um erweitert, und umgekehrt. Die `CollapsiblePanelExtender` macht die `toggle()` -Methode die genau diese Lücke: Kehrt den Zustand des Bereichs. Aber es gibt auch ein anderer Ansatz (wird intern von verwendet die `toggle()` Methode): der `get_Collapsed()` -Methode der der `CollapsiblePanelExtender()` gibt an, ob der Bereich reduziert wird oder nicht. Je nach den von dieser Funktion zurückgegebenen Wert, der Bereich ist, und klicken Sie dann entweder erweitert (`_doOpen()` Methode) oder reduziert (`_doClose()`) Methode:

[!code-javascript[Main](collapsing-and-expanding-a-panel-from-javascript-vb/samples/sample8.js)]


[![Die dritte Schaltfläche ändert den Zustand des Bereichs: aus reduziert erweiterte und zurück](collapsing-and-expanding-a-panel-from-javascript-vb/_static/image2.png)](collapsing-and-expanding-a-panel-from-javascript-vb/_static/image1.png)

Die dritte Schaltfläche ändert den Zustand des Bereichs: aus reduziert erweiterte und zurück ([klicken Sie, um das Bild in voller Größe anzeigen](collapsing-and-expanding-a-panel-from-javascript-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Vorherige](collapsing-and-expanding-a-panel-from-javascript-cs.md)
