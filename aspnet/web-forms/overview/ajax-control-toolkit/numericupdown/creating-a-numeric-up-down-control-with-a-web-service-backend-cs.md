---
uid: web-forms/overview/ajax-control-toolkit/numericupdown/creating-a-numeric-up-down-control-with-a-web-service-backend-cs
title: Erstellen eine numerische nach oben/unten Steuerelement mit einer Web-Service-Backend (c#) | Microsoft Docs
author: wenz
description: Anstatt einen Benutzer aus, geben Sie einen Wert in ein Kontrollkästchen konnte eine numerische-Steuerelement (das unter Windows und anderen Betriebssystemen vorhanden ist) nach oben/unten als weitere c nachweisen...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: c99bbc72-d4de-41ed-92a4-9a4632368363
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/numericupdown/creating-a-numeric-up-down-control-with-a-web-service-backend-cs
msc.type: authoredcontent
ms.openlocfilehash: 942902bdba93fe4fef8a9122403c6d5c62e6123c
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
ms.locfileid: "30868814"
---
<a name="creating-a-numeric-updown-control-with-a-web-service-backend-c"></a>Erstellen einen numerischen Steuerelements nach oben/unten mit einem Web Service Back-End-(c#)
====================
durch [Christian Wenz](https://github.com/wenz)

[Herunterladen von Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/numericupdown1.cs.zip) oder [PDF herunterladen](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/numericupdown1CS.pdf)

> Anstatt einen Benutzer einen Wert in ein Kontrollkästchen geben numerische nach oben/unten-Steuerelement (das unter Windows und anderen Betriebssystemen vorhanden ist) kann sich als mehr vertraut. Wird standardmäßig das NumericUpDown-Steuerelement immer erhöht oder verringert einen Wert von 1, aber ein Webdienst wird bewiesen, dass mehr Flexibilität.


## <a name="overview"></a>Übersicht

Anstatt einen Benutzer einen Wert in ein Kontrollkästchen geben numerische nach oben/unten-Steuerelement (das unter Windows und anderen Betriebssystemen vorhanden ist) kann sich als mehr vertraut. Wird standardmäßig die `NumericUpDown` Steuerelement immer erhöht oder verringert einen Wert von 1, aber ein Webdienst wird bewiesen, dass mehr Flexibilität.

## <a name="steps"></a>Schritte

Das ASP.NET AJAX-Steuerelement-Toolkit enthält die `NumericUpDown` Extender automatisch zwei Schaltflächen zu einem Textfeld hinzugefügt: eine zum Erhöhen des Objektwerts, eine zum Verringern der es. Das Steuerelement unterstützt jedoch auch ein Aufruf des Webdiensts (oder Methodenaufruf für die Seite "). Wenn die oben oder unten Schaltfläche geklickt wird, die JavaScript-Code eine Verbindung mit dem Webserver her und führt eine Methode vorhanden. Die Signatur der Methode ist die folgende:

[!code-csharp[Main](creating-a-numeric-up-down-control-with-a-web-service-backend-cs/samples/sample1.cs)]

Der `current` Argument ist der aktuelle Wert im Textfeld; der `tag` Attribut ist zusätzliche Kontextdaten, die als Eigenschaft festgelegt werden, können die `NumericUpDown` Extender (ist jedoch nicht erforderlich).

Für dieses Beispiel soll der numerischen Wert nach oben/unten Steuerelement Werte, die Potenzen von 2 sind nur zulassen: 1, 2, 4, 8, 16, 32, 64 und So weiter. Aus diesem Grund muss die Methode ausgeführt wird, wenn der Benutzer zum Erhöhen des möchte double den alten Wert; die andere Methode muss durch zwei Teilen. Hier ist also der vollständige Webdienst:

[!code-aspx[Main](creating-a-numeric-up-down-control-with-a-web-service-backend-cs/samples/sample2.aspx)]

Abschließend erstellen Sie eine neue ASP.NET-Seite. Wie üblich, Sie müssen eine `ScriptManager` -Steuerelement, ein `TextBox` Steuerelement und ein `NumericUpDownExtender` Steuerelement. Für die letztgenannte Aufgabe müssen Sie die Webdienstinformationen angeben:

- `ServiceDownMethod` Name des der aus-Webmethode oder Seite Methode
- `ServiceDownPath` der Pfad zu den Webdienst mit der nach-unten Dienstmethode; Lassen Sie bei Verwendung von einer Seitenmethode
- `ServiceUpMethod` Name der auf--Webmethode oder Seite Methode
- `ServiceUpPath` der Pfad zu den Webdienst mit dem Stand Dienstmethode; Lassen Sie bei Verwendung von einer Seitenmethode

Hier ist das vollständige Markup für die Seite an:

[!code-aspx[Main](creating-a-numeric-up-down-control-with-a-web-service-backend-cs/samples/sample3.aspx)]

Wenn Sie auf der Seite "ausführen, beachten Sie, wie der Wert in das Textfeld immer verdoppelt wird, wenn Sie klicken Sie auf die Schaltfläche" oben "und werden beim Klicken auf die Schaltfläche mit den niedrigeren halbiert.


[![Nur Ziffern, die eine Potenz von 2 angezeigt werden.](creating-a-numeric-up-down-control-with-a-web-service-backend-cs/_static/image2.png)](creating-a-numeric-up-down-control-with-a-web-service-backend-cs/_static/image1.png)

Nur Ziffern, die eine Potenz von 2 angezeigt werden ([klicken Sie hier, um das Bild in voller Größe angezeigt](creating-a-numeric-up-down-control-with-a-web-service-backend-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Nächste](creating-a-numeric-up-down-control-with-a-web-service-backend-vb.md)
