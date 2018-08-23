---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part7
title: Hinzufügen der Überprüfung zum Modell | Microsoft-Dokumentation
author: shanselman
description: Dies ist ein Tutorial für Anfänger, die die Grundlagen von ASP.NET MVC eingeführt werden. Erstellen Sie eine einfache Webanwendung, die aus einer Datenbank liest und schreibt.
ms.author: riande
ms.date: 08/14/2010
ms.assetid: aa7b3e8e-e23d-49f1-b160-f99a7f2982bd
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part7
msc.type: authoredcontent
ms.openlocfilehash: 4c7867587ba0610f0f1c23d9a0b9fbdc4040de7c
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2018
ms.locfileid: "41837159"
---
<a name="adding-validation-to-the-model"></a>Hinzufügen der Überprüfung zum Modell
====================
durch [Scott Hanselman](https://github.com/shanselman)

> Dies ist ein Tutorial für Anfänger, die die Grundlagen von ASP.NET MVC eingeführt werden. Sie erstellen eine einfache Webanwendung, die aus einer Datenbank liest und schreibt. Besuchen Sie die [ASP.NET MVC-Informationscenter](../../../index.md) anderen ASP.NET MVC anhand von Tutorials und Beispiele finden.


In diesem Abschnitt werden wir implementieren Sie die notwendige Unterstützung für die Validierung von Benutzereingaben in unserer Anwendung zu aktivieren. Wir stellen sicher, dass unsere Datenbankinhalte immer richtig ist und hilfreiche Fehlermeldungen für Endbenutzer bereit, wenn sie versuchen, und geben Sie die Daten ungültig ist. Wir beginnen, indem die Movie-Klasse ein wenig Validierungslogik hinzugefügt.

Klicken Sie mit der rechten Maustaste auf den Modell-Ordner, und wählen Sie die Klasse hinzufügen. Benennen Sie die Movie-Klasse.

Wenn wir das Entitätsmodell Film zuvor erstellt haben, erstellt die IDE eine Movie-Klasse. Tatsächlich können die Movie-Klasse in einer Datei und Teil an einer anderen angehören. Dies ist eine partielle Klasse bezeichnet. Wir werden die Movie-Klasse aus einer anderen Datei zu erweitern.

Wir erstellen eine partielle Movie-Klasse, die auf "Buddy-Klasse" zeigt für einige Attribute, die Validierung Hinweise auf das System erhalten. Wir markieren den Titel und der Preis nach Bedarf, und auch bestehen, dass der Preis innerhalb eines bestimmten Bereichs liegen. Klicken Sie mit der rechten Maustaste auf den Ordner "Models", und wählen Sie die Klasse hinzufügen. Benennen Sie die Movie-Klasse, und klicken Sie auf die Schaltfläche "OK". Hier ist unsere teilweise Movie-Klasse-ähnlichen.

[!code-csharp[Main](getting-started-with-mvc-part7/samples/sample1.cs)]

Führen Sie die Anwendung erneut aus, und versuchen Sie, einen Film mit einem Preis über 100 einzugeben. Sie erhalten eine Fehlermeldung, nachdem Sie das Formular gesendet haben. Der Fehler abgefangen wird, auf dem Server, und es tritt auf, nachdem das Formular gesendet wird. Beachten Sie, wie integrierte HTML-Hilfsprogramme des ASP.NET intelligent genug, um die Fehlermeldung angezeigt, und behalten die Werte für uns in der Textbox-Elemente wurden:

[![CreateMovieWithValidation](getting-started-with-mvc-part7/_static/image2.png)](getting-started-with-mvc-part7/_static/image1.png)

Dies funktioniert hervorragend, aber es wäre schön, wenn wir den Benutzer auf der Clientseite sofort mitteilen könnten, bevor der Server beteiligt ruft.

Lassen Sie uns einige clientseitige Validierung mit JavaScript zu aktivieren.

## <a name="adding-client-side-validation"></a>Die clientseitige Validierung hinzufügen

Da unsere Movie-Klasse bereits einige Validierungsattribute aufweist, müssen wir nur unseren Create.aspx ansichtsvorlage einige JavaScript-Dateien hinzu, und fügen eine Codezeile aktivieren Sie die clientseitige Validierung durchgeführt werden.

In VWD wechseln Sie den Ordner Views/Film, und öffnen Sie Create.aspx.

Öffnen Sie im Projektmappen-Explorer den Ordner "Scripts", und ziehen Sie die folgenden drei Skripts innerhalb der &lt;Head&gt; Tag.

- MicrosoftAjax.js
- MicrosoftMvcValidation.js

Diese Skriptdateien in dieser Reihenfolge angezeigt werden sollen.

[!code-html[Main](getting-started-with-mvc-part7/samples/sample2.html)]

Darüber hinaus fügen Sie dieser einzelnen Zeile oberhalb der Html.BeginForm hinzu:

[!code-aspx[Main](getting-started-with-mvc-part7/samples/sample3.aspx)]

Hier ist der Code in der IDE angezeigt.

[![Filme – Microsoft Visual Web Developer 2010 Express (10)](getting-started-with-mvc-part7/_static/image4.png)](getting-started-with-mvc-part7/_static/image3.png)

Führen Sie die Anwendung und besuchen Sie /Movies/Create erneut, und klicken Sie auf erstellen, ohne Eingabe von Daten. Die Fehlermeldungen werden sofort ohne die Seite flash, ordnen wir Senden von Daten zurück an den Server. Dies ist, da ASP.NET MVC jetzt die Eingabe der sowohl der Client (JavaScript) eine Überprüfung durchgeführt wird und auf dem Server.

[![Erstellen Sie – Windows InternetExplorer](getting-started-with-mvc-part7/_static/image6.png)](getting-started-with-mvc-part7/_static/image5.png)

Dies ist gut suchen! Nun fügen Sie eine zusätzliche Spalte in der Datenbank.

> [!div class="step-by-step"]
> [Zurück](getting-started-with-mvc-part6.md)
> [Weiter](getting-started-with-mvc-part8.md)
