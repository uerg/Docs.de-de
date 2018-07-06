---
uid: web-forms/overview/ajax-control-toolkit/nobot/fighting-bots-vb
title: Abwehren von Bots (VB) | Microsoft-Dokumentation
author: wenz
description: Automatisierte Bots Stuck, Blogs und anderen Websites mit Spam, Kommentar ohne Eingreifen des Benutzers senden. Das Steuerelement "nobot" in der ASP.NET AJAX-Con...
ms.author: aspnetcontent
ms.date: 06/02/2008
ms.assetid: e9803150-452d-4521-97e3-d75d5599383c
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/nobot/fighting-bots-vb
msc.type: authoredcontent
ms.openlocfilehash: e79a973f721c1feeddb00ecbf9d6a76786afb4bb
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2018
ms.locfileid: "37833442"
---
<a name="fighting-bots-vb"></a>Abwehren von Bots (VB)
====================
durch [Christian Wenz](https://github.com/wenz)

[Code herunterladen](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/NoBot0.vb.zip) oder [PDF-Datei herunterladen](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/nobot0VB.pdf)

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

[!code-aspx[Main](fighting-bots-vb/samples/sample1.aspx)]

Klicken Sie dann wie gewohnt stellen Sie sicher, dass die `ScriptManager` auf der Seite, damit die ASP.NET AJAX-Bibliothek geladen wird und das Steuerelement-Toolkit verwendet werden kann:

[!code-aspx[Main](fighting-bots-vb/samples/sample2.aspx)]

Da der Großteil der Überprüfungen `NoBot` Aktionen auftreten, auf dem Server, müssen Sie das Ergebnis des diese Überprüfungen zu überprüfen. Dies ist möglich durch Aufrufen von `NoBot`des `IsValid()` Methode. Es hat ein Argument (als ein `out` Parameter /`ByRef` Parameter) vom Typ `NoBotState`. Die Zeichenfolgendarstellung enthält den Grund an, wenn die Überprüfung ein Fehler auftritt und `Valid` andernfalls. Der folgende Code gibt eine Meldung gemäß `NoBot`des führen:

[!code-aspx[Main](fighting-bots-vb/samples/sample3.aspx)]

Schließlich benötigen Sie ein Formular zum Senden und ein Label-Element die Meldung ausgegeben, und Sie sind fertig!

[!code-aspx[Main](fighting-bots-vb/samples/sample4.aspx)]

Wenn Sie dieses Skript ausführen und Deaktivieren von JavaScript oder senden Sie das Formular in den ersten zwei Sekunden oder senden Sie das Formular sieben Mal innerhalb von 30 Sekunden, erhalten Sie eine Fehlermeldung angezeigt. Verwenden Sie dieses Steuerelement jedoch bedacht, da nur ungefähr 90 bis 95 % der Benutzer über die JavaScript-aktivierte verfügen, daher 5 bis 10 % der Benutzer nicht `NoBot`des testen.


[![Diese Fehlermeldung kann durch einen Bot verursacht werden](fighting-bots-vb/_static/image2.png)](fighting-bots-vb/_static/image1.png)

Diese Fehlermeldung kann auftreten, durch einen Bot ([klicken Sie, um das Bild in voller Größe anzeigen](fighting-bots-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Vorherige](fighting-bots-cs.md)
