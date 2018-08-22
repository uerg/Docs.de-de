---
uid: mvc/overview/older-versions-1/security/preventing-javascript-injection-attacks-cs
title: Verhindern von Injection-Angriffen für JavaScript (c#) | Microsoft-Dokumentation
author: StephenWalther
description: Verhindern Sie, dass JavaScript-Injection-Angriffe und Cross-Site Scripting-Angriffe informieren möchten. In diesem Tutorial erläutert Stephen Walther an, wie Sie de auf einfache Weise...
ms.author: riande
ms.date: 08/19/2008
ms.assetid: d0136da6-81a4-4815-b002-baa84744c09e
msc.legacyurl: /mvc/overview/older-versions-1/security/preventing-javascript-injection-attacks-cs
msc.type: authoredcontent
ms.openlocfilehash: 77d0f0346e9eff756cd74c64c310918f3c367ab1
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2018
ms.locfileid: "41834751"
---
<a name="preventing-javascript-injection-attacks-c"></a>Verhindern von Injection-Angriffen für JavaScript (c#)
====================
durch [Stephen Walther](https://github.com/StephenWalther)

[PDF herunterladen](http://download.microsoft.com/download/8/4/8/84843d8d-1575-426c-bcb5-9d0c42e51416/ASPNET_MVC_Tutorial_06_CS.pdf)

> Verhindern Sie, dass JavaScript-Injection-Angriffe und Cross-Site Scripting-Angriffe informieren möchten. In diesem Tutorial erläutert Stephen Walther an, wie Sie einfach diese Arten von Angriffen durch HTML-Codierung Ihrer Inhalte zunichte machen können.


Das Ziel in diesem Tutorial wird beschrieben, wie Sie JavaScript-Injection-Angriffen in ASP.NET MVC-Anwendungen verhindern können. In diesem Tutorial werden zwei Ansätze zur Verteidigung Ihrer Website für einen JavaScript-Injection-Angriff erläutert. Erfahren Sie, wie Sie JavaScript-Injection-Angriffe zu verhindern, indem Sie die Codierung der Daten, die angezeigt werden. Außerdem erfahren Sie, wie Sie JavaScript-Injection-Angriffe zu verhindern, indem Sie die Codierung der Daten, die Sie akzeptieren.

## <a name="what-is-a-javascript-injection-attack"></a>Was ist ein JavaScript-Injection-Angriff?

Wenn Sie Benutzereingaben akzeptieren und das erneute die Benutzereingabe anzeigen, öffnen Sie Ihre Website für JavaScript-Injection-Angriffe. Betrachten Sie eine konkrete Anwendung, die für JavaScript-Injection-Angriffe geöffnet ist.

Stellen Sie sich vor, dass Sie eine Kunden-Feedback-Website erstellt haben (siehe Abbildung 1). Kunden können finden Sie auf der Website, und geben Sie Feedback an den Erfahrungen, die Ihre Produkte verwenden. Wenn ein Kunde ihr Feedback übermittelt, wird das Feedback auf der Seite "Feedback" erneut angezeigt.


[![Kunden-Feedback-Website](preventing-javascript-injection-attacks-cs/_static/image2.png)](preventing-javascript-injection-attacks-cs/_static/image1.png)

**Abbildung 01**: Kunden-Feedback-Website ([klicken Sie, um das Bild in voller Größe anzeigen](preventing-javascript-injection-attacks-cs/_static/image3.png))


Die Kunden-Feedback-Website verwendet die `controller` in Codebeispiel 1. Dies `controller` enthält zwei Aktionen, die mit dem Namen `Index()` und `Create()`.

**Codebeispiel 1: `HomeController.cs`**

[!code-csharp[Main](preventing-javascript-injection-attacks-cs/samples/sample1.cs)]

Die `Index()` Methode zeigt die `Index` anzeigen. Diese Methode gibt alle von der vorherigen Feedback von Kunden, die `Index` Ansicht durch Abrufen des Feedbacks aus der Datenbank (mithilfe einer LINQ to SQL-Abfrage).

Die `Create()` Methode erstellt ein neues Feedbackelement und fügt es der Datenbank hinzu. Die Meldung, die der Kunde in der Form gibt übergeben wird, um die `Create()` -Methode in der Meldungsparameter. Ein Feedbackelement erstellt wird und die Nachricht wird des Feedbackelement zugewiesen `Message` Eigenschaft. Das Feedbackelement wird übermittelt, für die Datenbank mit der `DataContext.SubmitChanges()` Methodenaufruf. Schließlich wird der Besucher umgeleitet, an die `Index` anzeigen, in denen alle das Feedback wird angezeigt.

Die `Index` Ansicht im Codebeispiel 2 enthalten ist.

**Codebeispiel 2: `Index.aspx`**

[!code-aspx[Main](preventing-javascript-injection-attacks-cs/samples/sample2.aspx)]

Die `Index` Ansicht besteht aus zwei Abschnitten. Der oberste Abschnitt enthält die tatsächliche Kunden-Feedback-Formular. Der untere Abschnitt enthält eine For... Each-Schleife, die alle vorherigen Customer feedbackelemente durchläuft und zeigt die EntryDate und Nachricht Eigenschaften für jedes Feedbackelement.

Die Kunden-Feedback-Website ist eine einfache Website. Leider ist die Website für JavaScript-Injection-Angriffe geöffnet.

Stellen Sie sich, dass Sie den folgenden Text in das Kunden-Feedback-Formular eingeben:

[!code-html[Main](preventing-javascript-injection-attacks-cs/samples/sample3.html)]

Dieser Text stellt ein JavaScript-Skript, das eine Warnmeldung angezeigt. Nachdem ein Benutzer dieses Skript an das Feedback übermittelt zu bilden, die Nachricht <em>Boo!</em> wird angezeigt, wenn jeder Benutzer, die Kunden-Feedback-Website in der Zukunft besucht (siehe Abbildung 2).


[![Einschleusung von JavaScript](preventing-javascript-injection-attacks-cs/_static/image5.png)](preventing-javascript-injection-attacks-cs/_static/image4.png)

**Abbildung 02**: JavaScript-Injection ([klicken Sie, um das Bild in voller Größe anzeigen](preventing-javascript-injection-attacks-cs/_static/image6.png))


Nun kann Ihre erste Reaktion auf JavaScript-Injection-Angriffen kann das zu apathie sein. Zunächst vermuten, dass die JavaScript-Injection-Angriffe einfach eine Art von sind *Verunstaltung* Angriff. Sie gehen davon aus, dass niemand alles wirklich böse ausführen können, indem ein Commit für einen JavaScript-Injection-Angriff.

Leider ein Hacker möglich, einige wirklich sehr schlecht Dinge durch Einfügen von JavaScript in einer Website. Sie können einen JavaScript-Injection-Angriff verwenden, Cross-Site Scripting (XSS) auszuführen. Bei einem Angriff Cross-Site Scripting, wenn Sie vertrauliche Benutzer Daten stehlen und an eine andere Website gesendet.

Beispielsweise kann ein Hacker einen JavaScript-Injection-Angriff verwenden, um die Werte der Cookies im Browser von anderen Benutzern zu stehlen. Wenn vertraulicher Informationen zugreifen, beispielsweise Kennwörter, Kreditkartennummern und Sozialversicherungsnummern – in die Cookies im Browser gespeichert sind, klicken Sie dann können ein Hacker einen JavaScript-Injection-Angriff Sie um diese Informationen zu stehlen. Wenn ein Benutzer vertraulichen Informationen in ein Formularfeld in eine Seite, die bei einem JavaScript-Angriff gefährdet ist eingibt, klicken Sie dann der Hacker kann auch den injizierten JavaScript-Code zum Abrufen von Daten aus dem Formular und eine andere Website an.

*Seien Sie abschreckend*. Nehmen Sie JavaScript-Injection-Angriffen ernst zu, und schützen Sie vertrauliche Informationen des Benutzers. In den nächsten beiden Abschnitten werden zwei Techniken, mit denen Sie Ihre ASP.NET MVC-Anwendungen von JavaScript-Injection-Angriffen zu schützen.

## <a name="approach-1-html-encode-in-the-view"></a>Ansatz #1: HTML-Codierung in der Ansicht

Eine einfache Methode zum Verhindern von JavaScript-Injection-Angriffen in HTML codieren Sie alle Daten, die von Benutzern der Website eingegeben werden, wenn Sie die Daten in einer Sicht erneut anzeigen. Die aktualisierte `Index` Ansicht in Programmausdruck 3 folgt diesem Ansatz.

**Codebeispiel 3 – `Index.aspx` (HTML-codiert)**

[!code-aspx[Main](preventing-javascript-injection-attacks-cs/samples/sample4.aspx)]

Beachten Sie, dass der Wert des `feedback.Message` ist HTML-codiert werden, bevor der Wert durch den folgenden Code angezeigt wird:

[!code-aspx[Main](preventing-javascript-injection-attacks-cs/samples/sample5.aspx)]

Funktionsweise Mittelwert in HTML codieren eine Zeichenfolge? Wenn Sie HTML eine Zeichenfolge zu codieren, gefährliche Zeichen wie z. B. `<` und `>` durch Verweise auf HTML-Entitäten wie z. B. ersetzt werden `&lt;` und `&gt;`. Dies der Fall bei der Zeichenfolge `<script>alert("Boo!")</script>` HTML-codiert, es konvertiert `&lt;script&gt;alert(&quot;Boo!&quot;)&lt;/script&gt;`. Die codierte Zeichenfolge führt nicht mehr als einem JavaScript-Skript, wenn von einem Browser interpretiert. Stattdessen erhalten Sie in Abbildung 3 die harmlose Seite an.


[![JavaScript-Angriff](preventing-javascript-injection-attacks-cs/_static/image8.png)](preventing-javascript-injection-attacks-cs/_static/image7.png)

**Abbildung 03**: keine JavaScript-Angriff ([klicken Sie, um das Bild in voller Größe anzeigen](preventing-javascript-injection-attacks-cs/_static/image9.png))


Beachten Sie, dass in der `Index` anzeigen in Programmausdruck 3 nur den Wert der `feedback.Message` codiert ist. Der Wert des `feedback.EntryDate` ist nicht codiert. Sie müssen nur von einem Benutzer eingegeben Daten codieren. Da der Wert des EntryDate im Controller generiert wurde, müssen Sie nicht in HTML dieses Werts codieren.

## <a name="approach-2-html-encode-in-the-controller"></a>Ansatz #2: HTML-Codierung im Controller

Anstelle von HTML codieren von Daten, wenn Sie die Daten in einer Ansicht anzeigen, können Sie HTML codieren Sie die Daten ein, kurz bevor die Daten in der Datenbank zu übermitteln. Dieser zweite Ansatz wird verwendet, in der die `controller` in Listing 4.

**Programmausdruck 4 – `HomeController.cs` (HTML-codiert)**

[!code-csharp[Main](preventing-javascript-injection-attacks-cs/samples/sample6.cs)]

Beachten Sie, dass der Wert der Nachricht HTML ist-codiert werden, bevor der Wert, an die Datenbank in übermittelt wird der `Create()` Aktion. Wenn die Nachricht in der Ansicht erneut angezeigt wird, wird die Nachricht wird HTML-codiert, und JavaScript in der Nachricht eingefügt wird nicht ausgeführt.

In der Regel sollten Sie die erste Methode, die in diesem Tutorial erläutert wird, über dieser zweite Ansatz bevorzugen. Das Problem bei dieser zweite Ansatz ist, dass Sie HTML-codierte Daten in der Datenbank erhalten. Das heißt, werden Ihre Datenbankdaten mit lustige aussehende Zeichen verschmutzte.

Warum dies schlecht ist? Wenn Sie Daten in der Datenbank in einen anderen Wert als eine Webseite anzeigen müssen, klicken Sie dann haben Sie Probleme. Beispielsweise können Sie nicht mehr einfach die Daten in einer Windows Forms-Anwendung anzeigen.

## <a name="summary"></a>Zusammenfassung

Der Zweck dieses Lernprogramms bestand darin, Sie über die Aussicht einen JavaScript-Injection-Angriff abschrecken. In diesem Tutorial erläutert zwei Ansätze zur Verteidigung von ASP.NET MVC-Anwendungen für JavaScript-Injection-Angriffen: können Sie entweder HTML codieren Benutzer übermittelt Daten in der Ansicht, oder Sie können HTML codieren Benutzer übermittelt Daten in den Controller.

> [!div class="step-by-step"]
> [Zurück](authenticating-users-with-windows-authentication-cs.md)
> [Weiter](authenticating-users-with-forms-authentication-vb.md)
