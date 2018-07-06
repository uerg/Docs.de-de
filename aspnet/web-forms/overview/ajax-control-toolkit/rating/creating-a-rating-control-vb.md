---
uid: web-forms/overview/ajax-control-toolkit/rating/creating-a-rating-control-vb
title: Erstellen eines Bewertungssteuerelements (VB) | Microsoft-Dokumentation
author: wenz
description: Viele Websites bieten e-Commerce, Community-Sites, die Benutzer auf Rate Artikel oder Elemente. Dies in der Regel erfordert einigen Aufwand beim Codeschreiben, aber wir haben die...
ms.author: aspnetcontent
ms.date: 06/02/2008
ms.assetid: 6d0d70f4-725e-4258-8ae8-24a6ba1ddbf7
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/rating/creating-a-rating-control-vb
msc.type: authoredcontent
ms.openlocfilehash: cb8bd8ab400bf5633b8218e724ad86cf7271699d
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2018
ms.locfileid: "37811972"
---
<a name="creating-a-rating-control-vb"></a>Erstellen eines Bewertungssteuerelements (VB)
====================
durch [Christian Wenz](https://github.com/wenz)

[Code herunterladen](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/rating0.vb.zip) oder [PDF-Datei herunterladen](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/rating0VB.pdf)

> Viele Websites bieten e-Commerce, Community-Sites, die Benutzer auf Rate Artikel oder Elemente. Dies in der Regel erfordert einigen Aufwand beim Codeschreiben, aber wir müssen das Steuerelement-Toolkit unsere Freigabe.


## <a name="overview"></a>Übersicht

Viele Websites bieten e-Commerce, Community-Sites, die Benutzer auf Rate Artikel oder Elemente. Dies in der Regel erfordert einigen Aufwand beim Codeschreiben, aber wir müssen das Steuerelement-Toolkit unsere Freigabe.

## <a name="steps"></a>Schritte

Als Erstes benötigen Sie (mindestens) zwei Arten von Images: eine für einen ausgefüllten Bewertung Element und eine für ein Element leer Bewertung. Ein Element für die Bewertung ist normalerweise ein Stern- oder einem Smiley. In diesem Szenario finden Sie drei Dateien, smiley.png und empty.png und Smiley-done.png als Teil der Source-Code-Downloads für dieses Tutorial.

Anschließend erstellen Sie eine neue ASP.NET-Datei, und beginnen mit dem Hinzufügen einer `ScriptManager` -Steuerelement hinzu:

[!code-aspx[Main](creating-a-rating-control-vb/samples/sample1.aspx)]

Fügen Sie dann die `Rating` Steuerelement von ASP.NET AJAX Control Toolkit. Die folgenden Attribute für dieses Beispiel festgelegt werden müssen:

- `CurrentRating` die erste Bewertung verwendet werden
- `MaxRating` die maximale Bewertung
- `EmptyStarCssClass` die CSS-Klasse verwenden, wenn ein Element der Bewertung (Star) leer ist.
- `FilledStarCssClass` zu verwenden, wenn ein Element der Bewertung (Star) ausgefüllt wird, die CSS-Klasse
- `StarCssClass` die CSS-Klasse, die für einen sichtbaren Stat verwendet
- `WaitingStarCssClass` die CSS-Klasse verwenden, während eine Bewertung von Daten an den Server gesendet wird

Und hier ist das Markup der einem Steuerelement für Bewertungen mit fünf erstellt Elemente (Smileys), von denen keine anfänglich ausgefüllt wird:

[!code-aspx[Main](creating-a-rating-control-vb/samples/sample2.aspx)]

Die drei referenzierten CSS-Klassen müssen jetzt zeigen die entsprechenden Bilddateien, dies ist ganz einfach mithilfe von CSS:

[!code-css[Main](creating-a-rating-control-vb/samples/sample3.css)]

Stellen Sie sicher, dass Sie in die Breite und Höhe der drei Images bereitstellen, andernfalls kann die Anzeige ein wenig dachte aussehen.

Schließlich sollte das Ergebnis der Bewertung werden dem Benutzer angezeigt (oder zumindest in einer Datenbank gespeichert). So fügen Sie eine Bezeichnung für die Ausgabe eine SMS-Nachricht und eine Schaltfläche "Senden" Zurücksenden der Bewertung Formular an den Server hinzu:

[!code-aspx[Main](creating-a-rating-control-vb/samples/sample4.aspx)]

Zugriff auf das Steuerelement für Bewertungen über, in den serverseitigen Code, dessen `ID` und rufen Sie die `CurrentRating` Eigenschaft, die die Anzahl der ausgewählten Bewertung Elemente, in unserem Beispiel ein Wert zwischen 0 und 5 ist.

[!code-aspx[Main](creating-a-rating-control-vb/samples/sample5.aspx)]

Speichern Sie die Seite, und Laden Sie sie in Ihrem Browser. Wenn Sie auf die Elemente (anfänglich leer) Bewertung zeigen, ein JavaScript-Effekt tritt: die Änderungen für die Bewertung. Wenn Sie auf den Satz von Sternen klicken, wird die aktuelle Bewertung beibehalten. Wenn Sie das Formular übermitteln, gibt der serverseitige Code schließlich ausgewählte Bewertung aus.


[![Erstellen ein Bewertungssystem mit minimalem code](creating-a-rating-control-vb/_static/image2.png)](creating-a-rating-control-vb/_static/image1.png)

Erstellen ein Bewertungssystem mit minimalem Code ([klicken Sie, um das Bild in voller Größe anzeigen](creating-a-rating-control-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Vorherige](creating-a-rating-control-cs.md)
