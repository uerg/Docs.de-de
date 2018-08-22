---
uid: mvc/overview/older-versions-1/controllers-and-routing/adding-dynamic-content-to-a-cached-page-vb
title: Hinzufügen von dynamischen Inhalten zu einer zwischengespeicherten Seite (VB) | Microsoft-Dokumentation
author: microsoft
description: Erfahren Sie, wie dynamische und zwischengespeicherte Inhalte auf der gleichen Seite zu kombinieren. Ersetzung nach dem Zwischenspeichern können Sie dynamischen Inhalt, z. B. Banner Ankündigungen o anzuzeigen...
ms.author: riande
ms.date: 01/27/2009
ms.assetid: 68acd884-fb57-4486-a1be-aaa93e380780
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/adding-dynamic-content-to-a-cached-page-vb
msc.type: authoredcontent
ms.openlocfilehash: 121a3a35c8255f1423d7008930315f76bbb8e8f9
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2018
ms.locfileid: "41825899"
---
<a name="adding-dynamic-content-to-a-cached-page-vb"></a>Hinzufügen von dynamischen Inhalten zu einer zwischengespeicherten Seite (VB)
====================
durch [Microsoft](https://github.com/microsoft)

> Erfahren Sie, wie dynamische und zwischengespeicherte Inhalte auf der gleichen Seite zu kombinieren. Ersetzung nach dem Zwischenspeichern können Sie zum Anzeigen dynamischen Inhalts, z. B. Banner Ankündigungen oder neuen Elementen auf einer Seite, die Ausgabe wurden zwischengespeichert.


Durch Zwischenspeichern der Ausgabe nutzen, können Sie die Leistung von ASP.NET MVC-Anwendungen erheblich verbessern. Anstelle eine Seite wird jedes Mal, wenn, das die Seite angefordert wird, erneut generiert, kann die Seite einmal generiert und im Speicher für mehrere Benutzer zwischengespeichert werden.

Aber es liegt ein Problem. Was geschieht, wenn Sie dynamischen Inhalt auf der Seite anzeigen müssen? Angenommen Sie, dass Sie eine Ankündigung Banner auf der Seite angezeigt werden soll. Sie sollten nicht die Ankündigung Banner zwischengespeichert werden, damit jeder Benutzer, die diese Ankündigung erkennt. Sie wäre nicht auf diese Weise Geld verdienen!

Glücklicherweise ist gibt es eine einfache Lösung. Profitieren Sie ein Feature von ASP.NET-Framework namens *nach dem Zwischenspeichern Ersetzung*. Ersetzung nach dem Zwischenspeichern können Sie dynamischen Inhalt auf einer Seite zu ersetzen, die im Arbeitsspeicher zwischengespeichert wurde.


In der Regel bei der Ausgabe Zwischenspeichern eine Seite mit den &lt;OutputCache&gt; -Attribut, die Seite wird auf dem Server und der Client (dem Webbrowser) zwischengespeichert. Wenn Sie Ersetzungen nach dem Zwischenspeichern verwenden, wird eine Seite nur auf dem Server zwischengespeichert.


#### <a name="using-post-cache-substitution"></a>Verwenden von Ersetzungen nach dem Zwischenspeichern

Mithilfe von Ersetzungen nach dem Zwischenspeichern sind zwei Schritte erforderlich. Zunächst müssen Sie eine Methode definieren, die eine Zeichenfolge zurückgibt, die den dynamischen Inhalt darstellt, den in die zwischengespeicherte Seite angezeigt werden soll. Als Nächstes rufen Sie die HttpResponse.WriteSubstitution()-Methode, um den dynamischen Inhalt in die Seite einfügen.

Stellen Sie sich vor, z. B., dass nach dem Zufallsprinzip Nachrichten aus verschiedenen Elementen in einer zwischengespeicherten Seite angezeigt werden soll. Die Klasse in Codebeispiel 1 stellt eine einzelne Methode, mit dem Namen RenderNews(), die nach dem Zufallsprinzip eine Nachrichten-Element aus einer Liste mit drei Nachrichtenelemente zurückgibt.

**Codebeispiel 1 – Models\News.vb**

[!code-vb[Main](adding-dynamic-content-to-a-cached-page-vb/samples/sample1.vb)]

Um Ersetzungen nach dem Zwischenspeichern nutzen zu können, rufen Sie die HttpResponse.WriteSubstitution()-Methode. Die Methode WriteSubstitution() Richtet den Code, um eine Region aus, der die zwischengespeicherte Seite mit dynamischen Inhalten zu ersetzen. Die WriteSubstitution()-Methode dient zum Anzeigen des zufälligen News-Elements in der Ansicht in Liste 2.

**Codebeispiel 2 – Views\Home\Index.aspx**

[!code-aspx[Main](adding-dynamic-content-to-a-cached-page-vb/samples/sample2.aspx)]

Die RenderNews-Methode wird an die WriteSubstitution()-Methode übergeben. Beachten Sie, dass die RenderNews-Methode nicht aufgerufen wird. Stattdessen wird ein Verweis auf die Methode an WriteSubstitution() mithilfe der AddressOf-Operator übergeben.

Ansicht "Index" wird zwischengespeichert. Die Ansicht wird durch den Controller in Programmausdruck 3 zurückgegeben. Beachten Sie, die mit die Index()-Aktion ergänzt wird ein &lt;OutputCache&gt; -Attribut, das bewirkt, dass die Ansicht "Index" 60 Sekunden lang zwischengespeichert werden.

**Codebeispiel 3 – Controllers\HomeController.vb**

[!code-vb[Main](adding-dynamic-content-to-a-cached-page-vb/samples/sample3.vb)]

Obwohl die Ansicht "Index" zwischengespeichert wird, werden Nachrichten aus verschiedenen zufällige-Elemente angezeigt, wenn Sie die Indexseite anfordern. Wenn Sie die Indexseite anfordern, ändert die von der Seite angezeigte Uhrzeit nicht 60 Sekunden (siehe Abbildung 1). Die Tatsache, die die Zeit nicht ändert beweist, dass die Seite zwischengespeichert wird. Jedoch der Inhalt wird durch die WriteSubstitution()-Methode – die zufällige News-Artikel – Änderungen bei jeder Anforderung eingefügt.

**Abbildung 1 – dynamische Nachrichtenelemente in einer zwischengespeicherten Seite einfügen**

![clip_image002](adding-dynamic-content-to-a-cached-page-vb/_static/image1.jpg)

#### <a name="using-post-cache-substitution-in-helper-methods"></a>Verwenden von Ersetzungen nach dem Zwischenspeichern in Helper-Methoden

Eine einfachere Möglichkeit zum Nutzen von Ersetzungen nach dem Zwischenspeichern ist der Aufruf der Methode WriteSubstitution() innerhalb einer benutzerdefinierten Hilfsmethoden-Methode zu kapseln. Dieser Ansatz ist mit der Hilfsmethode in Listing 4 dargestellt.

**Programmausdruck 4 – Helpers\AdHelper.vb**

[!code-vb[Main](adding-dynamic-content-to-a-cached-page-vb/samples/sample4.vb)]

Programmausdruck 4 enthält ein Visual Basic-Modul, das zwei Methoden zur Verfügung stellt: RenderBanner() und RenderBannerInternal(). Die RenderBanner()-Methode stellt die tatsächliche Hilfsmethode dar. Diese Methode erweitert die standardmäßige ASP.NET MVC HtmlHelper-Klasse, sodass Sie Html.RenderBanner() in einer Ansicht wie jede andere Helper-Methode aufrufen können.

Die RenderBanner()-Methode ruft die HttpResponse.WriteSubstitution()-Methode, die die RenderBannerInternal()-Methode der WriteSubsitution()-Methode übergeben.

Die RenderBannerInternal()-Methode ist eine private Methode. Diese Methode wird nicht als eine Hilfsmethode verfügbar gemacht werden. Die RenderBannerInternal()-Methode gibt eine Bannerbild Ankündigungen nach dem Zufallsprinzip aus einer Liste mit drei Images der Banner-Ankündigung.

Geänderte Ansicht "Index" in Listing 5 wird veranschaulicht, wie Sie die RenderBanner()-Hilfsmethode. Beachten Sie, dass ein zusätzliches &lt;% @ Import %&gt; Richtlinie finden Sie am oberen Rand der Ansicht, um den MvcApplication1.Helpers-Namespace zu importieren. Wenn Sie nicht diesen Namespace zu importieren, wird nicht die RenderBanner()-Methode als Methode für die Html-Eigenschaft angezeigt.

**Programmausdruck 5 – Views\Home\Index.aspx (mit RenderBanner()-Methode)**

[!code-aspx[Main](adding-dynamic-content-to-a-cached-page-vb/samples/sample5.aspx)]

Wenn Sie die Seite der Sicht in Listing 5 anfordern, wird bei jeder Anforderung unterschiedliche Werbebanner angezeigt (siehe Abbildung 2). Die Seite wird zwischengespeichert, aber die Banner Ankündigung wird dynamisch mit der Hilfsmethode RenderBanner() eingefügt.

**Abbildung 2 – Anzeigen von zufälligen Werbebanner Ansicht "Index"**

![clip_image004](adding-dynamic-content-to-a-cached-page-vb/_static/image2.jpg)

#### <a name="summary"></a>Zusammenfassung

In diesem Tutorial wird erläutert, wie Sie Inhalte in einer zwischengespeicherten Seite dynamisch aktualisieren können. Sie haben gelernt, wie Sie die HttpResponse.WriteSubstitution()-Methode verwenden, um dynamische Inhalte in einer zwischengespeicherten Seite eingefügt werden zu ermöglichen. Außerdem haben Sie gelernt, den Aufruf der Methode WriteSubstitution() innerhalb einer HTML-Hilfsprogramm-Methode zu kapseln.

Nutzen Sie die Vorteile des Zwischenspeicherns an, wann immer möglich ist – er kann drastische Auswirkungen auf die Leistung Ihrer Webanwendungen haben. Wie in diesem Tutorial erläutert wird, können Sie nutzen zwischenspeichern, auch wenn dynamischen Inhalte auf Ihren Seiten angezeigt werden müssen.

> [!div class="step-by-step"]
> [Zurück](improving-performance-with-output-caching-vb.md)
> [Weiter](creating-a-controller-vb.md)
