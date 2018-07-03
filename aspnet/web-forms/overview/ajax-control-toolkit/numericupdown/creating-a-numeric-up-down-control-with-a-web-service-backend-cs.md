---
uid: web-forms/overview/ajax-control-toolkit/numericupdown/creating-a-numeric-up-down-control-with-a-web-service-backend-cs
title: Erstellen eine numerische hoch-/Herunterskalieren-Steuerelement mit einer Web-Dienst-Back-End (c#) | Microsoft-Dokumentation
author: wenz
description: Anstatt einen Benutzer aus, geben Sie einen Wert in ein Kontrollkästchen, kann ein numerischen UpAndDown-Steuerelement (das unter Windows und anderen Betriebssystemen vorhanden) als weitere c Beweisen...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: c99bbc72-d4de-41ed-92a4-9a4632368363
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/numericupdown/creating-a-numeric-up-down-control-with-a-web-service-backend-cs
msc.type: authoredcontent
ms.openlocfilehash: f9f9bd6b565ae4309694df64d24aac2a03238930
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/03/2018
ms.locfileid: "37389310"
---
<a name="creating-a-numeric-updown-control-with-a-web-service-backend-c"></a>Erstellen einen numerischen UpAndDown-Steuerelement mit einer Web-Dienst-Back-End (c#)
====================
durch [Christian Wenz](https://github.com/wenz)

[Code herunterladen](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/numericupdown1.cs.zip) oder [PDF-Datei herunterladen](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/numericupdown1CS.pdf)

> Geben Sie einen Wert in das Kontrollkästchen für einen Benutzer, anstatt ein numerischer hoch-/Herunterskalieren-Steuerelement (das unter Windows und anderen Betriebssystemen vorhanden) kann sich als mehr vertraut. Standardmäßig werden das NumericUpDown-Steuerelement immer erhöht oder verringert einen Wert von 1, aber ein Webdienst, erweist sich als flexibler.


## <a name="overview"></a>Übersicht

Geben Sie einen Wert in das Kontrollkästchen für einen Benutzer, anstatt ein numerischer hoch-/Herunterskalieren-Steuerelement (das unter Windows und anderen Betriebssystemen vorhanden) kann sich als mehr vertraut. In der Standardeinstellung die `NumericUpDown` Steuerelement immer erhöht oder verringert einen Wert von 1, aber ein Webdienst, erweist sich als flexibler.

## <a name="steps"></a>Schritte

Das ASP.NET AJAX Control Toolkit enthält die `NumericUpDown` Extender automatisch zwei Schaltflächen zu einem Textfeld hinzugefügt: eine zum Erhöhen des Werts, eine zum Verringern der es. Das Steuerelement unterstützt jedoch auch ein Aufruf des Webdiensts (oder eine Seite-Methodenaufruf). Wenn das hoch- oder Herunterskalieren Schaltfläche geklickt wird, die JavaScript-Code eine Verbindung mit dem Webserver her und führt es eine Methode. Die Signatur der Methode ist den folgenden Ausdruck:

[!code-csharp[Main](creating-a-numeric-up-down-control-with-a-web-service-backend-cs/samples/sample1.cs)]

Die `current` Argument ist der aktuelle Wert in das Textfeld; die `tag` -Attribut ist zusätzlicher Kontext-Daten, die als eine Eigenschaft festgelegt werden können die `NumericUpDown` Extender (ist jedoch nicht erforderlich).

In diesem Beispiel die numerische hoch-/Herunterskalieren Steuerelement sollte nur zulassen Werte, die Potenzen von 2: 1, 2, 4, 8, 16, 32, 64 und So weiter. Aus diesem Grund muss die Methode ausgeführt wird, wenn der Benutzer den Wert erhöhen möchte double den alten Wert; die andere Methode muss durch zwei Teilen. Hier ist also die vollständige Webdienst:

[!code-aspx[Main](creating-a-numeric-up-down-control-with-a-web-service-backend-cs/samples/sample2.aspx)]

Abschließend erstellen Sie eine neue ASP.NET-Seite. Wie üblich, müssen Sie eine `ScriptManager` -Steuerelement, ein `TextBox` Steuerelement und ein `NumericUpDownExtender` Steuerelement. In letzterem Fall müssen Sie die Web-Service-Informationen bereitstellen:

- `ServiceDownMethod` Name des der aus web-Methode oder die Seite Methode
- `ServiceDownPath` Pfad auf den Webdienst mit der nach-unten-Service-Methode Lassen Sie bei Verwendung eine Seitenmethode
- `ServiceUpMethod` Name des oben-Webmethode oder die Seite Methode
- `ServiceUpPath` Pfad auf den Webdienst mit der Sie Dienstmethode Lassen Sie bei Verwendung eine Seitenmethode

Hier ist das vollständige Markup für die Seite:

[!code-aspx[Main](creating-a-numeric-up-down-control-with-a-web-service-backend-cs/samples/sample3.aspx)]

Wenn Sie auf die Seite ausführen, beachten Sie, wie der Wert in das Textfeld immer verdoppelt sich, beim Klicken Sie auf die Schaltfläche "oben", und wird beim Klicken auf die untere Schaltfläche halbiert.


[![Nur Zahlen, die eine Potenz von 2 angezeigt werden.](creating-a-numeric-up-down-control-with-a-web-service-backend-cs/_static/image2.png)](creating-a-numeric-up-down-control-with-a-web-service-backend-cs/_static/image1.png)

Nur Zahlen, die eine Potenz von 2 angezeigt werden ([klicken Sie, um das Bild in voller Größe anzeigen](creating-a-numeric-up-down-control-with-a-web-service-backend-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Nächste](creating-a-numeric-up-down-control-with-a-web-service-backend-vb.md)
