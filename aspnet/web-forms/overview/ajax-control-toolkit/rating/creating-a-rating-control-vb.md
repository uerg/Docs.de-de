---
uid: web-forms/overview/ajax-control-toolkit/rating/creating-a-rating-control-vb
title: Erstellen eines Steuerelements Bewertung (VB) | Microsoft Docs
author: wenz
description: Viele Websites bieten Benutzern die Rate Artikel oder Elemente von e-Commerce-Community-Sites. Dies erfordert in der Regel einige Codierungsaufwand, aber wir haben die...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 6d0d70f4-725e-4258-8ae8-24a6ba1ddbf7
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/rating/creating-a-rating-control-vb
msc.type: authoredcontent
ms.openlocfilehash: ff3394865e084c5a24e7e79469a4a7d26aabb552
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="creating-a-rating-control-vb"></a>Erstellen eines Steuerelements Bewertung (VB)
====================
durch [Christian Wenz](https://github.com/wenz)

[Herunterladen von Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/rating0.vb.zip) oder [PDF herunterladen](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/rating0VB.pdf)

> Viele Websites bieten Benutzern die Rate Artikel oder Elemente von e-Commerce-Community-Sites. Dies erfordert in der Regel einige Codierungsaufwand allerdings haben wir die Control-Toolkit zu Verfügung.


## <a name="overview"></a>Übersicht

Viele Websites bieten Benutzern die Rate Artikel oder Elemente von e-Commerce-Community-Sites. Dies erfordert in der Regel einige Codierungsaufwand allerdings haben wir die Control-Toolkit zu Verfügung.

## <a name="steps"></a>Schritte

Erstens, Sie benötigen (mindestens) zwei Arten von Images: eine für eine ausgefüllte Bewertung Element und eine für ein Element leer Bewertung. Eine Bewertung-Element ist in der Regel einen Stern oder einem Smiley. In diesem Szenario finden Sie drei Dateien, smiley.png und empty.png und Smiley done.png im Rahmen des Downloads Source Code für dieses Lernprogramm.

Anschließend erstellen Sie eine neue ASP.NET-Datei, und starten Sie mit dem Hinzufügen einer `ScriptManager` Steuerelement darauf:

[!code-aspx[Main](creating-a-rating-control-vb/samples/sample1.aspx)]

Fügen Sie dann die `Rating` Steuerelement aus dem ASP.NET AJAX-Steuerelement-Toolkit. Die folgenden Attribute müssen für dieses Beispiel festgelegt werden:

- `CurrentRating`die erste Bewertung verwendet werden
- `MaxRating`die maximale Bewertung für
- `EmptyStarCssClass`die CSS-Klasse verwenden, wenn ein Element Bewertung (Stern) leer ist.
- `FilledStarCssClass`die CSS-Klasse verwenden, wenn ein Element Bewertung (Stern) ausgefüllt wird
- `StarCssClass`die CSS-Klasse, die für einen sichtbaren Stat verwendet
- `WaitingStarCssClass`die CSS-Klasse verwenden, während Bewertungssterne zurück an den Server gesendet wird

Und hier ist das Markup erstellt ein Steuerelement für die Bewertung mit fünf Elementen (Smileys), von denen keine anfänglich ausgefüllt wird:

[!code-aspx[Main](creating-a-rating-control-vb/samples/sample2.aspx)]

Die drei referenzierten CSS-Klassen müssen nun die entsprechenden Bilddateien anzeigen also erleichtert, Verwendung von CSS:

[!code-css[Main](creating-a-rating-control-vb/samples/sample3.css)]

Stellen Sie sicher, dass Sie die Breite und Höhe der drei Images bereitstellen, andernfalls kann die Anzeige etwas daran aussehen.

Schließlich sollte das Ergebnis der Bewertung werden dem Benutzer angezeigten (oder mindestens in einer Datenbank gespeichert). So fügen Sie eine Bezeichnung für die Ausgabe von SMS und eine Schaltfläche "Absenden" Zurücksenden der Bewertung Formular an den Server hinzu:

[!code-aspx[Main](creating-a-rating-control-vb/samples/sample4.aspx)]

In den serverseitigen Code Zugriff auf das Steuerelement Bewertung über seine `ID` , und klicken Sie dann Zugriff auf seine `CurrentRating` Eigenschaft, die die Anzahl der Elemente ausgewählten Bewertung in unserem Beispiel einen Wert zwischen 0 und 5 ist.

[!code-aspx[Main](creating-a-rating-control-vb/samples/sample5.aspx)]

Speichern Sie die Seite, und Laden Sie sie in Ihren Browser. Wenn Sie auf die Elemente (Anfangs leer) Bewertung zeigen, ein JavaScript-Effekt tritt: die Bewertung ändert. Wenn Sie für den Satz von Sternen klicken, wird die Bewertung des aktuellen beibehalten. Bei der Übermittlung des Formulars wird der serverseitige Code schließlich ausgewählte Bewertung ausgegeben.


[![Erstellen ein Bewertungssystem mit minimalem Codeeinsatz](creating-a-rating-control-vb/_static/image2.png)](creating-a-rating-control-vb/_static/image1.png)

Erstellen ein Bewertungssystem mit minimalem Codeeinsatz ([klicken Sie hier, um das Bild in voller Größe angezeigt](creating-a-rating-control-vb/_static/image3.png))

>[!div class="step-by-step"]
[Zurück](creating-a-rating-control-cs.md)
