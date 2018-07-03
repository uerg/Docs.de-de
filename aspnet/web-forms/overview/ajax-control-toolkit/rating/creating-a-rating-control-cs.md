---
uid: web-forms/overview/ajax-control-toolkit/rating/creating-a-rating-control-cs
title: Erstellen eines Bewertungssteuerelements (c#) | Microsoft-Dokumentation
author: wenz
description: Viele Websites bieten e-Commerce, Community-Sites, die Benutzer auf Rate Artikel oder Elemente. Dies in der Regel erfordert einigen Aufwand beim Codeschreiben, aber wir haben die...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 969fb28f-2bff-4fc4-b24a-27f5e2534a37
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/rating/creating-a-rating-control-cs
msc.type: authoredcontent
ms.openlocfilehash: 7be954a73c6c08bca9992aacf6ad529bc61c9247
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/03/2018
ms.locfileid: "37389585"
---
<a name="creating-a-rating-control-c"></a>Erstellen eines Bewertungssteuerelements (c#)
====================
durch [Christian Wenz](https://github.com/wenz)

[Code herunterladen](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/rating0.cs.zip) oder [PDF-Datei herunterladen](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/rating0CS.pdf)

> Viele Websites bieten e-Commerce, Community-Sites, die Benutzer auf Rate Artikel oder Elemente. Dies in der Regel erfordert einigen Aufwand beim Codeschreiben, aber wir müssen das Steuerelement-Toolkit unsere Freigabe.


## <a name="overview"></a>Übersicht

Viele Websites bieten e-Commerce, Community-Sites, die Benutzer auf Rate Artikel oder Elemente. Dies in der Regel erfordert einigen Aufwand beim Codeschreiben, aber wir müssen das Steuerelement-Toolkit unsere Freigabe.

## <a name="steps"></a>Schritte

Als Erstes benötigen Sie (mindestens) zwei Arten von Images: eine für einen ausgefüllten Bewertung Element und eine für ein Element leer Bewertung. Ein Element für die Bewertung ist normalerweise ein Stern- oder einem Smiley. In diesem Szenario finden Sie drei Dateien, smiley.png und empty.png und Smiley-done.png als Teil der Source-Code-Downloads für dieses Tutorial.

Anschließend erstellen Sie eine neue ASP.NET-Datei, und beginnen mit dem Hinzufügen einer `ScriptManager` -Steuerelement hinzu:

[!code-aspx[Main](creating-a-rating-control-cs/samples/sample1.aspx)]

Fügen Sie dann die `Rating` Steuerelement von ASP.NET AJAX Control Toolkit. Die folgenden Attribute für dieses Beispiel festgelegt werden müssen:

- `CurrentRating` die erste Bewertung verwendet werden
- `MaxRating` die maximale Bewertung
- `EmptyStarCssClass` die CSS-Klasse verwenden, wenn ein Element der Bewertung (Star) leer ist.
- `FilledStarCssClass` zu verwenden, wenn ein Element der Bewertung (Star) ausgefüllt wird, die CSS-Klasse
- `StarCssClass` die CSS-Klasse, die für einen sichtbaren Stat verwendet
- `WaitingStarCssClass` die CSS-Klasse verwenden, während eine Bewertung von Daten an den Server gesendet wird

Und hier ist das Markup der einem Steuerelement für Bewertungen mit fünf erstellt Elemente (Smileys), von denen keine anfänglich ausgefüllt wird:

[!code-aspx[Main](creating-a-rating-control-cs/samples/sample2.aspx)]

Die drei referenzierten CSS-Klassen müssen jetzt zeigen die entsprechenden Bilddateien, dies ist ganz einfach mithilfe von CSS:

[!code-css[Main](creating-a-rating-control-cs/samples/sample3.css)]

Stellen Sie sicher, dass Sie in die Breite und Höhe der drei Images bereitstellen, andernfalls kann die Anzeige ein wenig dachte aussehen.

Schließlich sollte das Ergebnis der Bewertung werden dem Benutzer angezeigt (oder zumindest in einer Datenbank gespeichert). So fügen Sie eine Bezeichnung für die Ausgabe eine SMS-Nachricht und eine Schaltfläche "Senden" Zurücksenden der Bewertung Formular an den Server hinzu:

[!code-aspx[Main](creating-a-rating-control-cs/samples/sample4.aspx)]

Zugriff auf das Steuerelement für Bewertungen über, in den serverseitigen Code, dessen `ID` und rufen Sie die `CurrentRating` Eigenschaft, die die Anzahl der ausgewählten Bewertung Elemente, in unserem Beispiel ein Wert zwischen 0 und 5 ist.

[!code-aspx[Main](creating-a-rating-control-cs/samples/sample5.aspx)]

Speichern Sie die Seite, und Laden Sie sie in Ihrem Browser. Wenn Sie auf die Elemente (anfänglich leer) Bewertung zeigen, ein JavaScript-Effekt tritt: die Änderungen für die Bewertung. Wenn Sie auf den Satz von Sternen klicken, wird die aktuelle Bewertung beibehalten. Wenn Sie das Formular übermitteln, gibt der serverseitige Code schließlich ausgewählte Bewertung aus.


[![Erstellen ein Bewertungssystem mit minimalem code](creating-a-rating-control-cs/_static/image2.png)](creating-a-rating-control-cs/_static/image1.png)

Erstellen ein Bewertungssystem mit minimalem Code ([klicken Sie, um das Bild in voller Größe anzeigen](creating-a-rating-control-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Nächste](creating-a-rating-control-vb.md)
