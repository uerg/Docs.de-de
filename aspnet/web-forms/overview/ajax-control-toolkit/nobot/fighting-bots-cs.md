---
uid: web-forms/overview/ajax-control-toolkit/nobot/fighting-bots-cs
title: Abwehren von Bots (c#) | Microsoft-Dokumentation
author: wenz
description: Automatisierte Bots Stuck, Blogs und anderen Websites mit Spam, Kommentar ohne Eingreifen des Benutzers senden. Das Steuerelement "nobot" in der ASP.NET AJAX-Con...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 0a1917e0-884a-4576-8e93-9ed660faae51
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/nobot/fighting-bots-cs
msc.type: authoredcontent
ms.openlocfilehash: 41e8fda5138e4a94e7b8c4af0a5c2bd68e50e9e1
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/03/2018
ms.locfileid: "37402719"
---
<a name="fighting-bots-c"></a>Abwehren von Bots (c#)
====================
durch [Christian Wenz](https://github.com/wenz)

[Code herunterladen](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/NoBot0.cs.zip) oder [PDF-Datei herunterladen](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/nobot0CS.pdf)

> Automatisierte Bots Stuck, Blogs und anderen Websites mit Spam, Kommentar ohne Eingreifen des Benutzers senden. Das Steuerelement "nobot" in der ASP.NET AJAX Control Toolkit können diese Bots zu bekämpfen.


## <a name="overview"></a>Übersicht

Automatisierte Bots Stuck, Blogs und anderen Websites mit Spam, Kommentar ohne Eingreifen des Benutzers senden. Das Steuerelement "nobot" in der ASP.NET AJAX Control Toolkit können diese Bots zu bekämpfen.

## <a name="steps"></a>Schritte

Ein gebräuchliches Verfahren zur Bots zunichte machen werden CAPTCHAs vollständig automatisierte öffentliche Turing-Test zu verwenden, um Computer und Menschen auseinander anzugeben. Ein Turing-Test war ursprünglich ein Test eine Person, in denen entscheiden, ob ein Kommunikationspartner für eine Person oder ein Computer ist erforderlich. Im Web besteht ein CAPTCHA eines Bildes mit einigen verzerrten Buchstaben darauf in der Regel aus. Die Idee ist, dass nur einem Benutzer, der die Buchstaben auf dem Image lesen kann, während die OCR-Algorithmen fehl.

Es gibt verschiedene vor- und Nachteile dieses Ansatzes, aber eine genauere Beschreibung ist, würde den Rahmen dieses Tutorials. Es ist jedoch ein Steuerelement in ASP.NET AJAX Control Toolkit bietet einen ähnlichen Ansatz: `NoBot`. Es ist einfacher, als ein CAPTCHA zu überwinden und ist sehr einfach zu verwenden und umfassen extrem gut auf Websites wie Blogs, in denen gilt dies erfolgreich, wenn die meisten Versuche Spam, werden außer Kraft gesetzt, die die `NoBot` Steuerelement möglich.

`NoBot` fängt das Postback des aktuellen ASP.NET Web Form an, wenn mindestens eine der folgenden Bedingungen erfüllt ist:

- Der Browser ein Fehler auftritt, ein JavaScript-Rätsel lösen (z. B. wenn JavaScript deaktiviert ist)
- Der Benutzer das Formular, um schnell gesendet hat
- Die Client-IP-Adresse übermittelt das Formular zu häufig in einem bestimmten Zeitraum an.

Um zu prüfen, ob diese Bedingungen, die `NoBot` Steuerelement erfordert diese Attribute (alle optional):

- `ResponseMinimumDelaySeconds` minimale Anzahl von Sekunden zwischen postbacks
- `CutoffWindowSeconds` die Länge des Zeitfensters, in dem Measures von Postbacks über eine IP-Adresse sind
- `CutoffMaximumInstances` maximale Anzahl von Sekunden pro Zeitintervall

Die folgende Markup-Anforderungen, mindestens zwei Sekunden verstreichen zwischen Postbacks und dass es nur fünf Postbacks innerhalb eines Zeitraums von 30 Sekunden maximal:

[!code-aspx[Main](fighting-bots-cs/samples/sample1.aspx)]

Klicken Sie dann wie gewohnt stellen Sie sicher, dass die `ScriptManager` auf der Seite, damit die ASP.NET AJAX-Bibliothek geladen wird und das Steuerelement-Toolkit verwendet werden kann:

[!code-aspx[Main](fighting-bots-cs/samples/sample2.aspx)]

Da der Großteil der Überprüfungen `NoBot` Aktionen auftreten, auf dem Server, müssen Sie das Ergebnis des diese Überprüfungen zu überprüfen. Dies ist möglich durch Aufrufen von `NoBot`des `IsValid()` Methode. Es hat ein Argument (als ein `out` Parameter /`ByRef` Parameter) vom Typ `NoBotState`. Die Zeichenfolgendarstellung enthält den Grund an, wenn die Überprüfung ein Fehler auftritt und `Valid` andernfalls. Der folgende Code gibt eine Meldung gemäß `NoBot`des führen:

[!code-aspx[Main](fighting-bots-cs/samples/sample3.aspx)]

Schließlich benötigen Sie ein Formular zum Senden und ein Label-Element die Meldung ausgegeben, und Sie sind fertig!

[!code-aspx[Main](fighting-bots-cs/samples/sample4.aspx)]

Wenn Sie dieses Skript ausführen und Deaktivieren von JavaScript oder senden Sie das Formular in den ersten zwei Sekunden oder senden Sie das Formular sieben Mal innerhalb von 30 Sekunden, erhalten Sie eine Fehlermeldung angezeigt. Verwenden Sie dieses Steuerelement jedoch bedacht, da nur ungefähr 90 bis 95 % der Benutzer über die JavaScript-aktivierte verfügen, daher 5 bis 10 % der Benutzer nicht `NoBot`des testen.


[![Diese Fehlermeldung kann durch einen Bot verursacht werden](fighting-bots-cs/_static/image2.png)](fighting-bots-cs/_static/image1.png)

Diese Fehlermeldung kann auftreten, durch einen Bot ([klicken Sie, um das Bild in voller Größe anzeigen](fighting-bots-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Nächste](fighting-bots-vb.md)
