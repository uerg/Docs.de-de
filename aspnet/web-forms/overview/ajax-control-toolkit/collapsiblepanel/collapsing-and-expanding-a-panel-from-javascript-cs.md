---
uid: web-forms/overview/ajax-control-toolkit/collapsiblepanel/collapsing-and-expanding-a-panel-from-javascript-cs
title: Reduzieren und erweitern ein Panel aus JavaScript (c#) | Microsoft Docs
author: wenz
description: "Das CollapsiblePanel-Steuerelement in ASP.NET AJAX-Steuerelement-Toolkit einen Bereich erweitert und bietet die Möglichkeit, den Inhalt zu reduzieren und erweitern ihn ein..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: de5500be-75e5-461a-8064-b70ae52ea6a4
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/collapsiblepanel/collapsing-and-expanding-a-panel-from-javascript-cs
msc.type: authoredcontent
ms.openlocfilehash: 666f3e212ccdd5b26b466f4672134ce751dc5dd1
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="collapsing-and-expanding-a-panel-from-javascript-c"></a>Reduzieren und erweitern ein Panel aus JavaScript (c#)
====================
durch [Christian Wenz](https://github.com/wenz)

[Herunterladen von Code](http://download.microsoft.com/download/8/a/a/8aab3c3e-de6f-463f-805c-5fda567eef6e/CollapsiblePanel1.cs.zip) oder [PDF herunterladen](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/collapsiblepanel1CS.pdf)

> Das CollapsiblePanel-Steuerelement in ASP.NET AJAX-Steuerelement-Toolkit einen Bereich erweitert und bietet die Möglichkeit, seinen Inhalt zu reduzieren und erweitern ihn erneut. Diese zwei Aktionen können auch benutzerdefinierte JavaScript-Code ausgelöst werden.


## <a name="overview"></a>Übersicht

Das CollapsiblePanel-Steuerelement in ASP.NET AJAX-Steuerelement-Toolkit einen Bereich erweitert und bietet die Möglichkeit, seinen Inhalt zu reduzieren und erweitern ihn erneut. Diese zwei Aktionen können auch benutzerdefinierte JavaScript-Code ausgelöst werden.

## <a name="steps"></a>Schritte

Zunächst erstellen Sie eine neue ASP.NET-Seite und enthalten die `ScriptManager` innerhalb der `<form>` Element. Dadurch wird geladen, die ASP.NET AJAX-Bibliothek, die vom Toolkit-Steuerelement erforderlich ist:

[!code-aspx[Main](collapsing-and-expanding-a-panel-from-javascript-cs/samples/sample1.aspx)]

Dann erstellen Sie ein Panel mit Text, damit die Auswirkungen Reduzieren/Erweitern angezeigt werden kann:

[!code-aspx[Main](collapsing-and-expanding-a-panel-from-javascript-cs/samples/sample2.aspx)]

Wie Sie sehen können, verweist im Bereich auf eine CSS-Klasse, die hier gezeigt wird (und im Grunde eine Hintergrundfarbe und Breite des Bereichs definiert):

[!code-css[Main](collapsing-and-expanding-a-panel-from-javascript-cs/samples/sample3.css)]

Die `CollapsiblePanelExtender` Steuerelement erfordert die `TargetControlID` Attribut, damit das Toolkit weiß, welche Bereich zu reduzieren oder erweitern, auf Anforderung:

[!code-aspx[Main](collapsing-and-expanding-a-panel-from-javascript-cs/samples/sample4.aspx)]

Leider der Extender derzeit nicht verfügbar macht eine bestimmte API für reduzieren oder Erweitern im Bereich, aber einige nicht dokumentierten Methoden führen. Erstens Hinzufügen von drei HTML-Schaltflächen auf der Seite klicken Sie dann die clientseitiges JavaScript zum Reduzieren oder erweitern den Inhalt des Bereichs auslösen:

[!code-aspx[Main](collapsing-and-expanding-a-panel-from-javascript-cs/samples/sample5.aspx)]

Im clientseitigen JavaScript-Code (Einstieg `<script type="text/javascript">`), die `$find()` Methode verwendet werden, muss für den Zugriff auf die `CollapsiblePanelExtender`. `$find("cpe")`Gibt einen Verweis darauf zurück. Von dort auf bestimmte Methoden lösen die Aufgabe.

Die Methode zum Öffnen von (Erweitert) wird im Bereich aufgerufen `_doOpen()`; der folgende code implementiert die `doOpen()` Funktion aufgerufen, wenn die erste Schaltfläche geklickt wird:

[!code-javascript[Main](collapsing-and-expanding-a-panel-from-javascript-cs/samples/sample6.js)]

Für das Schließen, oder reduzieren den Bereich der `_doClose()` Methode ausgeführt werden muss. Der Benutzer auf die zweite Schaltfläche klickt, wird daher folgende JavaScript-Code aufgerufen:

[!code-javascript[Main](collapsing-and-expanding-a-panel-from-javascript-cs/samples/sample7.js)]

Die dritte Schaltfläche Schaltet den Zustand des Bereichs: aus reduziert erweitert, und umgekehrt. Die `CollapsiblePanelExtender` macht die `toggle()` Methode was genau dies der Fall ist: Kehrt den Zustand des Bereichs. Aber es gibt auch ein anderer Ansatz (der intern von verwendet wird die `toggle()` Methode): der `get_Collapsed()` Methode der `CollapsiblePanelExtender()` gibt an, ob der Bereich reduziert wird. Abhängig vom Rückgabewert dieser Funktion, der Bereich ist, und klicken Sie dann entweder erweitert (`_doOpen()` Methode) oder reduzierter Form (`_doClose()`) Methode:

[!code-javascript[Main](collapsing-and-expanding-a-panel-from-javascript-cs/samples/sample8.js)]


[![Die dritte Schaltfläche ändert den Zustand des Bereichs: aus reduziert erweitert und zurück](collapsing-and-expanding-a-panel-from-javascript-cs/_static/image2.png)](collapsing-and-expanding-a-panel-from-javascript-cs/_static/image1.png)

Die dritte Schaltfläche ändert den Zustand des Bereichs: aus reduziert erweitert und zurück ([klicken Sie hier, um das Bild in voller Größe angezeigt](collapsing-and-expanding-a-panel-from-javascript-cs/_static/image3.png))

>[!div class="step-by-step"]
[Nächste](collapsing-and-expanding-a-panel-from-javascript-vb.md)
