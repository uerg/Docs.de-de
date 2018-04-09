---
uid: mvc/overview/older-versions-1/security/preventing-javascript-injection-attacks-cs
title: Verhindern von JavaScript-Injection-Angriffe (c#) | Microsoft Docs
author: StephenWalther
description: Verhindern, dass JavaScript-Injection-Angriffe und Cross-Site Scripting-Angriffe zu vermeiden Sie. In diesem Lernprogramm wird Stephen Walther erläutert, wie Sie de auf einfache Weise...
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/19/2008
ms.topic: article
ms.assetid: d0136da6-81a4-4815-b002-baa84744c09e
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/security/preventing-javascript-injection-attacks-cs
msc.type: authoredcontent
ms.openlocfilehash: fbec58c009640164d908db5a45557c9e50041173
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
---
<a name="preventing-javascript-injection-attacks-c"></a>Verhindern von JavaScript-Injection-Angriffe (c#)
====================
durch [Stephen Walther](https://github.com/StephenWalther)

[PDF herunterladen](http://download.microsoft.com/download/8/4/8/84843d8d-1575-426c-bcb5-9d0c42e51416/ASPNET_MVC_Tutorial_06_CS.pdf)

> Verhindern, dass JavaScript-Injection-Angriffe und Cross-Site Scripting-Angriffe zu vermeiden Sie. In diesem Lernprogramm wird Stephen Walther erläutert, wie Sie einfach diese Arten von Angriffen durch HTML-Codierung Ihrer Inhalte zunichte machen können.


Ziel dieses Lernprogramms wird erläutert, wie Sie JavaScript-Injection-Angriffen in Ihre ASP.NET MVC-Anwendungen verhindern können. In diesem Lernprogramm wird erläutert, zwei Ansätze zum Schutz Ihrer Website für einen JavaScript-Injection-Angriff. Erfahren Sie, wie Sie JavaScript-Injection-Angriffe zu verhindern, indem Sie Verschlüsseln von Daten, die Sie anzeigen. Sie erfahren außerdem, wie Sie JavaScript-Injection-Angriffe zu verhindern, indem Sie Verschlüsseln von Daten, die Sie akzeptieren.

## <a name="what-is-a-javascript-injection-attack"></a>Was ist ein JavaScript-Injection-Angriff?

Wenn Sie Benutzereingaben akzeptieren und das erneute die Benutzereingabe anzeigen, öffnen Sie Ihre Website für JavaScript-Injection-Angriffe. Sehen wir uns eine konkrete Anwendung, die für JavaScript-Injection-Angriffe geöffnet ist.

Stellen Sie sich vor, dass Sie eine Kunden-Feedback-Website erstellt haben (siehe Abbildung 1). Kunden können finden Sie auf der Website, und geben Sie Feedback zu ihrer Erfahrung mit Ihren Produkten. Wenn ein Kunde ihr Feedback sendet, wird das Feedback auf der Seite "Feedback" erneut.


[![Kunden-Feedback-Website](preventing-javascript-injection-attacks-cs/_static/image2.png)](preventing-javascript-injection-attacks-cs/_static/image1.png)

**Abbildung 01**: Kunden-Feedback-Website ([klicken Sie hier, um das Bild in voller Größe angezeigt](preventing-javascript-injection-attacks-cs/_static/image3.png))


Die Kunden-Feedback-Website verwendet das `controller` 1 aufgelistet. Dies `controller` enthält zwei Aktionen, die mit dem Namen `Index()` und `Create()`.

**Auflisten von 1 – `HomeController.cs`**

[!code-csharp[Main](preventing-javascript-injection-attacks-cs/samples/sample1.cs)]

Die `Index()` Methode zeigt den `Index` anzeigen. Diese Methode übergibt alle vorherigen Kundenfeedback auf die `Index` Ansicht durch Abrufen von Feedback aus der Datenbank (mithilfe einer LINQ to SQL-Abfrage).

Die `Create()` Methode erstellt ein neues Feedbackelement und der Datenbank hinzugefügt. Die Meldung, die der Kunde in der Form gibt wird zum Übergeben der `Create()` Methode im Message-Parameter. Ein Feedbackelement erstellt wird und die Nachricht wird des Feedbackelement zugewiesen `Message` Eigenschaft. Das Feedback-Element wird mit der Datenbank übermittelt die `DataContext.SubmitChanges()` -Methodenaufruf. Schließlich wird der Besucher umgeleitet, an die `Index` anzeigen, in dem alle Feedback, das angezeigt wird.

Die `Index` Ansicht im Codebeispiel 2 enthalten ist.

**Auflisten von 2 – `Index.aspx`**

[!code-aspx[Main](preventing-javascript-injection-attacks-cs/samples/sample2.aspx)]

Die `Index` Sicht hat zwei Abschnitte. Der obere Abschnitt enthält die tatsächlichen Kunden-Feedback-Formular. Der untere Abschnitt enthält eine For... Each-Schleife, die alle vorherigen Kunden Feedback Elemente durchläuft und zeigt die EntryDate und Nachricht Eigenschaften für jedes Feedbackelement.

Die Kunden-Feedback-Website ist eine einfache Website. Leider ist die Website öffnen, um JavaScript-Injection-Angriffe.

Stellen Sie sich vor, dass Sie den folgenden Text in der Kunden-Feedback-Formular eingeben:

[!code-html[Main](preventing-javascript-injection-attacks-cs/samples/sample3.html)]

Dieser Text stellt eine JavaScript-Skript, das eine Warnung Meldungsfeld wird angezeigt. Nachdem ein Benutzer mit diesem Skript an das Feedback übermittelt zu bilden, die Nachricht <em>Boo!</em> wird angezeigt, wenn jeder Benutzer, die Kunden-Feedback-Website in der Zukunft besucht (siehe Abbildung 2).


[![JavaScript-Injection](preventing-javascript-injection-attacks-cs/_static/image5.png)](preventing-javascript-injection-attacks-cs/_static/image4.png)

**Abbildung 02**: JavaScript-Injection ([klicken Sie hier, um das Bild in voller Größe angezeigt](preventing-javascript-injection-attacks-cs/_static/image6.png))


Nun kann Ihre erste Antwort für JavaScript-Injection-Angriffe Apathy sein. Glauben, dass JavaScript-Injection-Angriffe einfach ein sind *Verunstaltung* Angriff. Sie gehen davon aus, dass niemand nichts tatsächlich bösartige durch Ausführen eines Commits für eine JavaScript-Injection-Angriff ausführen können.

Leider ein Hacker kann tragen einige really, tatsächlich bösartige Dinge von Räumen JavaScript in einer Website. Sie können einen JavaScript-Injection-Angriff verwenden, einen Cross-Site-Skripting (XSS)-Angriff durchführen. Bei einem Angriff siteübergreifende vertrauliche Benutzerinformationen zu stehlen und an eine andere Website gesendet.

Ein Hacker kann z. B. einen JavaScript-Injection-Angriff verwenden, die Werte der Browsercookies von anderen Benutzern zu stehlen. Wenn vertraulicher Informationen – z. B. Kennwörter, Kreditkartennummern und Sozialversicherungsnummern – in der Browsercookies gespeichert ist, klicken Sie dann können ein Hacker einen JavaScript-Injection-Angriff Sie um diese Informationen zu stehlen. Oder, wenn ein Benutzer vertraulichen Informationen in ein Formularfeld in eine Seite, die mit einem JavaScript-Angriff gefährdet ist eingibt, der Hacker kann die eingefügte JavaScript verwenden, ziehen Sie die Daten des Formulars und einer anderen Website gesendet.

*Sie werden abschreckend*. Nehmen Sie JavaScript-Injection-Angriffe ernst, und schützen Sie vertrauliche Informationen des Benutzers. In den nächsten beiden Abschnitten besprechen wir zwei Techniken, die Sie verwenden können, um Ihre ASP.NET MVC-Anwendungen vom JavaScript-Injection-Angriffen zu schützen.

## <a name="approach-1-html-encode-in-the-view"></a>Ansatz #1: HTML-Codierung in der Ansicht

Eine einfache Methode, mit der JavaScript-Injection-Angriffe verhindert in HTML ist codieren alle Daten, die von Websitebenutzern eingegeben werden, wenn Sie die Daten in einer Ansicht anzuzeigen. Die aktualisierte `Index` Ansicht im Codebeispiel 3 folgt diesem Ansatz.

**Auflisten von 3 – `Index.aspx` (HTML-codiert)**

[!code-aspx[Main](preventing-javascript-injection-attacks-cs/samples/sample4.aspx)]

Beachten Sie, dass der Wert des `feedback.Message` ist HTML-codiert werden, bevor der Wert durch den folgenden Code angezeigt wird:

[!code-aspx[Main](preventing-javascript-injection-attacks-cs/samples/sample5.aspx)]

Welche Aktion er ausführt Mittelwert in HTML zu codieren eine Zeichenfolge? Wenn Sie HTML eine Zeichenfolge zu codieren, gefährliche Zeichen wie z. B. `<` und `>` werden von HTML-Entitätsverweisen, z. B. ersetzt `&lt;` und `&gt;`. Daher die Zeichenfolge `<script>alert("Boo!")</script>` ist HTML-codiert ist, ruft er konvertiert `&lt;script&gt;alert(&quot;Boo!&quot;)&lt;/script&gt;`. Die codierte Zeichenfolge führt nicht mehr als ein JavaScript-Skript, wenn vom Browser interpretiert. Rufen Sie stattdessen die Seite "ignorieren" in Abbildung 3.


[![JavaScript-Angriff wird außer Kraft gesetzt](preventing-javascript-injection-attacks-cs/_static/image8.png)](preventing-javascript-injection-attacks-cs/_static/image7.png)

**Abbildung 03**: JavaScript-Angriff wird außer Kraft gesetzt ([klicken Sie hier, um das Bild in voller Größe angezeigt](preventing-javascript-injection-attacks-cs/_static/image9.png))


Beachten Sie, dass in der `Index` anzeigen in 3 auflisten nur den Wert der `feedback.Message` codiert ist. Der Wert des `feedback.EntryDate` ist nicht codiert. Sie müssen nur von einem Benutzer eingegeben Daten codieren. Da der Wert des EntryDate im Controller generiert wurde, Sie nicht HTML-erforderlich dieser Wert Codierung.

## <a name="approach-2-html-encode-in-the-controller"></a>Vorgehensweise #2: HTML-Codierung im Controller

Anstelle von HTML-Codierung von Daten, wenn Sie die Daten in einer Ansicht anzeigen, können Sie HTML codieren der Daten, kurz bevor die Daten in die Datenbank zu senden. Dieser zweite Ansatz in der die `controller` 4 auflisten.

**Auflisten von 4 – `HomeController.cs` (HTML-codiert)**

[!code-csharp[Main](preventing-javascript-injection-attacks-cs/samples/sample6.cs)]

Beachten Sie, dass der Wert der Nachricht HTML ist-codiert vor der Übermittlung der des Werts in der Datenbank innerhalb der `Create()` Aktion. Wenn die Nachricht in der Ansicht angezeigt wird, wird die Nachricht ist HTML-codiert, und JavaScript in der Nachricht eingefügt wird nicht ausgeführt.

In der Regel sollten Sie die erste Methode, die in diesem Lernprogramm erläutert wird, über der zweite Ansatz bevorzugen. Das Problem bei dieser zweite Ansatz ist, dass Sie mit HTML-codierte Daten in der Datenbank am Ende. Das heißt, werden Ihre Datenbankdaten mit lustige suchen Zeichen Fehler auftreten.

Warum ist dies das fehlerhafte? Wenn Sie Daten in der Datenbank in einen anderen Wert als eine Webseite anzeigen müssen, werden Sie Probleme haben. Beispielsweise können Sie die Daten in einer Windows Forms-Anwendung einfach nicht mehr anzeigen.

## <a name="summary"></a>Zusammenfassung

Der Zweck dieses Tutorials konnten Sie über die Aussicht einen JavaScript-Injection-Angriff erschrecken. In diesem Lernprogramm erläuterten zwei Ansätze zum Schutz Ihrer ASP.NET MVC-Anwendungen vor Injection-Angriffen JavaScript: können Sie entweder HTML codieren Benutzer übermittelt Daten in der Ansicht, oder Sie können HTML codieren Benutzer übermittelt Daten im Controller.

> [!div class="step-by-step"]
> [Zurück](authenticating-users-with-windows-authentication-cs.md)
> [Weiter](authenticating-users-with-forms-authentication-vb.md)
