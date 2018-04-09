---
uid: web-forms/overview/ajax-control-toolkit/nobot/fighting-bots-vb
title: Abwehren von Bots (VB) | Microsoft Docs
author: wenz
description: Automatisierte Bots Stuck Blogs und anderen Websites mit Spam, Kommentar Formen ohne Eingreifen des Benutzers senden. Das NoBot-Steuerelement in der ASP.NET AJAX-Con...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: e9803150-452d-4521-97e3-d75d5599383c
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/nobot/fighting-bots-vb
msc.type: authoredcontent
ms.openlocfilehash: 35d5984ac7ff3422bab07a759c93fef3914a22f7
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
---
<a name="fighting-bots-vb"></a>Abwehren von Bots (VB)
====================
durch [Christian Wenz](https://github.com/wenz)

[Herunterladen von Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/NoBot0.vb.zip) oder [PDF herunterladen](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/nobot0VB.pdf)

> Automatisierte Bots Stuck Blogs und anderen Websites mit Spam, Kommentar Formen ohne Eingreifen des Benutzers senden. Das NoBot-Steuerelement in ASP.NET AJAX-Steuerelement-Toolkit können diese Bots bekämpfen.


## <a name="overview"></a>Übersicht

Automatisierte Bots Stuck Blogs und anderen Websites mit Spam, Kommentar Formen ohne Eingreifen des Benutzers senden. Das NoBot-Steuerelement in ASP.NET AJAX-Steuerelement-Toolkit können diese Bots bekämpfen.

## <a name="steps"></a>Schritte

Ein gebräuchliches Verfahren zur Bots zunichte machen werden CAPTCHAs vollständig automatisierte öffentliche Turing Test verwenden, um Computer und Menschen auseinander. Ein Test Turing wurde ursprünglich einen Test eine Person, zu entscheiden, ob ein Kommunikationspartner ein menschlicher oder ein Computer ist erforderlich. Im Web besteht ein CAPTCHA eines Bildes mit einigen verzerrten Buchstaben darauf in der Regel aus. Die Idee dabei ist, dass nur ein Benutzer die Buchstaben für das Abbild lesen kann, während OCR Algorithmen fehl.

Es gibt mehrere vor- und Nachteile dieser Vorgehensweise, aber eine Erläuterung dieser sprengt des Rahmen dieses lehrprogramms sprengen. Es ist jedoch ein Steuerelement in ASP.NET AJAX Control Toolkit, die einen ähnlichen Ansatz bereitstellt: `NoBot`. Es ist einfacher, als ein CAPTCHA zu umgehen, aber ist sehr einfach zu verwenden und Flugpreise sehr gut auf Websites wie Blogs, in denen es Erfolg berücksichtigt ist, wenn die meisten Versuche Spam, sind wird außer Kraft gesetzt, die die `NoBot` Steuerelement möglich.

`NoBot` fängt das Postback von der aktuellen ASP.NET Web Form ab, wenn mindestens eine der folgenden Bedingungen erfüllt ist:

- Der Browser eine JavaScript-Rätsel lösen fehlschlägt (z. B. wenn JavaScript deaktiviert ist)
- Der Benutzer übermittelt das Formular, um schnelle
- Die Client-IP-Adresse übermittelt das Formular zu häufig in einem bestimmten Zeitraum an.

Um diese Bedingungen zu überprüfen der `NoBot` Steuerelement erfordert diese Attribute (alle von ihnen optional):

- `ResponseMinimumDelaySeconds` minimale Anzahl der Sekunden zwischen postbacks
- `CutoffWindowSeconds` die Länge des Zeitintervalls, in denen Postbacks aus einem IP-Measures sind
- `CutoffMaximumInstances` maximale Anzahl der Sekunden pro Zeitintervall

Das folgende Markup Forderungen, mindestens zwei Sekunden verstreichen zwischen Postbacks und es sind nur fünf Postbacks oder weniger in einem Intervall von 30 Sekunden:

[!code-aspx[Main](fighting-bots-vb/samples/sample1.aspx)]

Klicken Sie dann wie üblich stellen Sie sicherstellen, dass die `ScriptManager` auf der Seite, damit die ASP.NET AJAX-Bibliothek geladen wird und die Control-Toolkit verwendet werden kann:

[!code-aspx[Main](fighting-bots-vb/samples/sample2.aspx)]

Da die meisten der Überprüfungen `NoBot` ist jedoch auf der Serverseite auftreten, müssen Sie das Ergebnis von diese Überprüfungen zu überprüfen. Dies kann geschehen, indem Aufrufen `NoBot`des `IsValid()` Methode. Es wurde ein Argument (wie ein `out` Parameter /`ByRef` Parameter) vom Typ `NoBotState`. Wenn die Überprüfung fehlschlägt, enthält seine Zeichenfolgendarstellung die Ursache und `Valid` andernfalls. Der folgende Code gibt eine Nachricht entsprechend dem `NoBot`des führen:

[!code-aspx[Main](fighting-bots-vb/samples/sample3.aspx)]

Schließlich benötigen Sie ein Formular zum Senden und ein Label-Element, um die Meldung ausgegeben, und Sie sind fertig!

[!code-aspx[Main](fighting-bots-vb/samples/sample4.aspx)]

Wenn Sie dieses Skript ausführen, und Deaktivieren von JavaScript oder des Formulars innerhalb der ersten zwei Sekunden senden oder des Formulars sieben Mal innerhalb von 30 Sekunden senden, erhalten Sie eine Fehlermeldung angezeigt. Verwenden Sie dieses Steuerelement jedoch mit Bedacht, da JavaScript aktiviert haben, nur ca. 90-95 % der Benutzer, daher 5 bis 10 % der Benutzer fehl `NoBot`des testen.


[![Diese Fehlermeldung kann durch einen Bot wurde vorliegt](fighting-bots-vb/_static/image2.png)](fighting-bots-vb/_static/image1.png)

Diese Fehlermeldung kann durch einen Bot verursacht worden sein ([klicken Sie hier, um das Bild in voller Größe angezeigt](fighting-bots-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Vorherige](fighting-bots-cs.md)
