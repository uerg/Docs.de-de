---
uid: mvc/overview/older-versions-1/controllers-and-routing/adding-dynamic-content-to-a-cached-page-cs
title: Hinzufügen von dynamischen Inhalt zu einer zwischengespeicherten Seite (c#) | Microsoft Docs
author: microsoft
description: Informationen Sie zum dynamischen und zwischengespeicherte Inhalte auf der gleichen Seite mischen. Nach der Zwischenspeicher können Sie dynamische Inhalte wie Banner Ankündigungen o anzeigen...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/27/2009
ms.topic: article
ms.assetid: 2ddd4407-d143-4a94-877c-21771bfb97a6
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/adding-dynamic-content-to-a-cached-page-cs
msc.type: authoredcontent
ms.openlocfilehash: 9f91cc07bc531cfb3edf577ab79e91fd94a57a3c
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
---
<a name="adding-dynamic-content-to-a-cached-page-c"></a>Hinzufügen von dynamischen Inhalt zu einer zwischengespeicherten Seite (c#)
====================
durch [Microsoft](https://github.com/microsoft)

> Informationen Sie zum dynamischen und zwischengespeicherte Inhalte auf der gleichen Seite mischen. Nach der Zwischenspeicher können Sie dynamische Inhalte, z. B. Banner Ankündigungen oder Nachrichtenelemente innerhalb einer Seite eine Ausgabe wurde hat zwischengespeichert.


Durch Zwischenspeichern der Ausgabe nutzen, können Sie die Leistung einer ASP.NET MVC-Anwendung deutlich verbessern. Anstelle von Neugenerieren eine Seite jedes Mal, wenn, das die Seite angefordert wird, kann die Seite einmal generiert und für mehrere Benutzer im Arbeitsspeicher zwischengespeichert werden.

Es ist jedoch ein Problem. Was geschieht, wenn müssen Sie dynamische Inhalte auf der Seite anzeigen? Angenommen Sie, dass ein Werbebanner auf der Seite angezeigt werden soll. Sie möchten schließlich nicht die Werbebanner zwischengespeichert werden, sodass jeder Benutzer die selbe Ankündigung angezeigt wird. Auf diese Weise würde nicht Sie Geld machen!

Glücklicherweise ist eine einfache Lösung. Profitieren Sie von einer Funktion von ASP.NET-Framework aufgerufen, *cache nach der Ersetzung*. Nach der Zwischenspeicher ermöglicht Ihnen, dynamische Inhalte auf einer Seite ersetzen, die im Arbeitsspeicher zwischengespeichert wurde.


Wenn Cache eine Seite mit dem Attribut [OutputCache] ausgegeben wird, wird die Seite in der Regel wird auf dem Server und der Client (Webbrowser) zwischengespeichert. Wenn Sie nach der Zwischenspeicher verwenden, wird eine Seite nur auf dem Server zwischengespeichert.


#### <a name="using-post-cache-substitution"></a>Verwenden nach der Zwischenspeicher

Verwenden nach der Zwischenspeicher sind zwei Schritte erforderlich. Zunächst müssen Sie eine Methode definieren, die eine Zeichenfolge zurückgibt, die die dynamische Inhalte darstellt, die in der zwischengespeicherten Seite angezeigt werden soll. Anschließend rufen Sie die HttpResponse.WriteSubstitution()-Methode, um den dynamischen Inhalt in die Seite einfügen.

Stellen Sie sich vor, z. B., dass nach dem Zufallsprinzip verschiedenen Nachrichtenelemente in einer zwischengespeicherten Seite angezeigt werden soll. Die Klasse im Codebeispiel 1 stellt eine einzelne Methode, mit dem Namen RenderNews(), die nach dem Zufallsprinzip ein Newselement aus einer Liste von drei Nachrichtenelemente zurückgibt.

**1 – Models\News.cs auflisten**

[!code-csharp[Main](adding-dynamic-content-to-a-cached-page-cs/samples/sample1.cs)]

Um nach der Zwischenspeicher nutzen zu können, rufen Sie die HttpResponse.WriteSubstitution()-Methode. Die WriteSubstitution()-Methode richtet den Code, um einen Bereich der zwischengespeicherten Seite mit dynamischen Inhalten zu ersetzen. Die WriteSubstitution()-Methode wird verwendet, zum Anzeigen des zufälligen News-Elements in der Ansicht 2 aufgelistet.

**2 – Views\Home\Index.aspx auflisten**

[!code-aspx[Main](adding-dynamic-content-to-a-cached-page-cs/samples/sample2.aspx)]

Die RenderNews-Methode wird an die WriteSubstitution()-Methode übergeben. Beachten Sie, dass die RenderNews-Methode nicht aufgerufen wird (es gibt keine Klammern). Stattdessen wird ein Verweis auf die Methode an WriteSubstitution() übergeben.

Die Indexansicht wird zwischengespeichert. Die Sicht wird von der Controller im Codebeispiel 3 zurückgegeben. Beachten Sie, dass die Index()-Aktion mit einem [OutputCache]-Attribut ergänzt wird, die bewirkt, dass die Indexansicht 60 Sekunden lang zwischengespeichert werden soll.

**3 – Controllers\HomeController. cs auflisten**

[!code-csharp[Main](adding-dynamic-content-to-a-cached-page-cs/samples/sample3.cs)]

Obwohl die Indexansicht zwischengespeichert wird, werden verschiedene zufällige Newsgroupbeiträgen angezeigt, wenn Sie die Indexseite anfordern. Wenn Sie die Indexseite anfordern, ändert die von der Seite angezeigte Uhrzeit nicht 60 Sekunden (siehe Abbildung 1). Die Tatsache, die die Zeit nicht ändert beweist, dass die Seite zwischengespeichert wird. Allerdings wird der Inhalt durch der WriteSubstitution() – das zufällige Newselement – ändert mit jeder Anforderung eingefügt.

**Abbildung 1 – dynamische Nachrichtenelemente in einer zwischengespeicherten Seite Räumen**

![clip_image002](adding-dynamic-content-to-a-cached-page-cs/_static/image1.jpg)

#### <a name="using-post-cache-substitution-in-helper-methods"></a>Verwenden nach der Zwischenspeicher in Hilfsmethoden

Eine einfachere Möglichkeit, nach der Zwischenspeicher nutzen ist, den Aufruf der Methode WriteSubstitution() innerhalb einer benutzerdefinierten Hilfsmethode zu kapseln. Dieser Ansatz wird durch die Hilfsmethode auflisten 4 veranschaulicht.

**4 – AdHelper.cs auflisten**

[!code-csharp[Main](adding-dynamic-content-to-a-cached-page-cs/samples/sample4.cs)]

Auflisten von 4 enthält eine statische Klasse, die zwei Methoden verfügbar macht: RenderBanner() und RenderBannerInternal(). Die RenderBanner() Methode darstellt, die tatsächliche Hilfsmethode. Diese Methode erweitert die standardmäßige ASP.NET MVC HtmlHelper-Klasse, damit Sie Html.RenderBanner() in einer Ansicht genau wie alle anderen Hilfsmethode aufrufen können.

Die RenderBanner()-Methode ruft die HttpResponse.WriteSubstitution()-Methode, die die RenderBannerInternal()-Methode der WriteSubsitution()-Methode übergeben.

Die RenderBannerInternal()-Methode ist eine private Methode. Diese Methode wird nicht als eine Hilfsmethode verfügbar gemacht werden. Die RenderBannerInternal()-Methode gibt ein Banner Ankündigung Bild nach dem Zufallsprinzip aus einer Liste mit drei Banner Ankündigung Bilder.

Die geänderte Indexansicht auflisten 5 wird veranschaulicht, wie Sie die RenderBanner()-Hilfsmethode. Beachten Sie, dass ein zusätzliches &lt;% @ Import %&gt; Richtlinie finden Sie am oberen Rand der Ansicht, um den MvcApplication1.Helpers-Namespace zu importieren. Wenn Sie keine dieses Namespace zu importieren, wird nicht die RenderBanner() Methode als Methode für die Html-Eigenschaft angezeigt.

**Auflisten von 5 – Views\Home\Index.aspx (mit RenderBanner()-Methode)**

[!code-aspx[Main](adding-dynamic-content-to-a-cached-page-cs/samples/sample5.aspx)]

Wenn Sie auf die Seite gerendert, die von der Sicht im Codebeispiel 5 anfordern, wird mit jeder Anforderung verschiedene Werbebanner angezeigt (siehe Abbildung 2). Die Seite wird zwischengespeichert, aber die Banner Ankündigung wird dynamisch durch die Hilfsmethode RenderBanner() eingefügt.

**Abbildung 2 – die Indexansicht Werbebanner zufälliges anzeigen**

![clip_image004](adding-dynamic-content-to-a-cached-page-cs/_static/image2.jpg)

#### <a name="summary"></a>Zusammenfassung

In diesem Lernprogramm wird erläutert, wie Sie Inhalte in einer zwischengespeicherten Seite dynamisch aktualisieren können. Sie haben gelernt, wie Sie die HttpResponse.WriteSubstitution()-Methode verwenden, um in einer zwischengespeicherten Seite eingefügt werden dynamische Inhalte zu ermöglichen. Außerdem haben Sie gelernt, den Aufruf der Methode WriteSubstitution() innerhalb einer HTML-Hilfsmethode kapseln.

Nutzen Sie die Vorteile des Zwischenspeicherns, wann immer möglich – sie können eine drastische Auswirkungen auf die Leistung Ihrer Webanwendungen verfügen. Wie in diesem Lernprogramm erläutert wird, können Sie nutzen zwischenspeichern, selbst wenn dynamische Inhalte zu Ihren Seiten anzuzeigen müssen.

## 

## 

> [!div class="step-by-step"]
> [Zurück](improving-performance-with-output-caching-cs.md)
> [Weiter](creating-a-controller-cs.md)
