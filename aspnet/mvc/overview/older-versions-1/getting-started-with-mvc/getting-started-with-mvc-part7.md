---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part7
title: "Hinzufügen einer Validierung zum Modell | Microsoft Docs"
author: shanselman
description: "Dies ist ein Anfänger-Lernprogramm, das die Grundlagen von ASP.NET MVC. Erstellen Sie eine einfache Web-Anwendung, die Lese- und Schreibvorgänge aus einer Datenbank."
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/14/2010
ms.topic: article
ms.assetid: aa7b3e8e-e23d-49f1-b160-f99a7f2982bd
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part7
msc.type: authoredcontent
ms.openlocfilehash: 25c939bc8121589f91914e553d56e8f0975115b2
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="adding-validation-to-the-model"></a>Hinzufügen einer Validierung zum Modell
====================
durch [Scott Hanselman](https://github.com/shanselman)

> Dies ist ein Anfänger-Lernprogramm, das die Grundlagen von ASP.NET MVC. Erstellen Sie eine einfache Web-Anwendung, die Lese- und Schreibvorgänge aus einer Datenbank. Besuchen Sie die [ASP.NET MVC-Trainingscenter](../../../index.md) andere ASP.NET-MVC Lernprogramme und Beispiele finden.


In diesem Abschnitt werden wir implementieren die Unterstützung für die Validierung von Benutzereingaben innerhalb der Anwendung aktivieren. Wir müssen sicherstellen, dass unsere Datenbankinhalte immer richtig ist und hilfreiche Fehlermeldungen für Endbenutzer bereitzustellen, wenn sie versuchen, und geben Sie die Filmdaten, die was nicht zulässig ist. Wir beginnen, indem Sie ein wenig Validierungslogik auf die Film-Klasse hinzufügen.

Klicken Sie mit der rechten Maustaste auf den Ordner Modell, und wählen Sie die Klasse hinzufügen. Benennen Sie Ihre Film-Klasse.

Wenn wir das Entitätsmodell Film vorher erstellt haben, erstellt die IDE eine Film-Klasse. In der Tat kann Teil der Film-Klasse in eine Datei und in einer anderen. Dies ist eine partielle Klasse aufgerufen. Wir werden die Film-Klasse aus einer anderen Datei erweitern.

Wir erstellen eine partielle Film-Klasse, verweist auf eine "Buddy-Klasse" für einige Attribute, die Überprüfung Hinweise mit dem System bereitgestellt wird. Wir Titel und Preis nach Bedarf zu markieren, und außerdem bestehen, dass der Preis innerhalb eines bestimmten Bereichs darstellen. Klicken Sie mit der mit der rechten Maustaste auf den Ordner Models, und wählen Sie die Klasse hinzufügen. Benennen Sie Ihre Film-Klasse, und klicken Sie auf die Schaltfläche "OK". Hier ist unsere partielle Film Klasse-ähnlichen.

[!code-csharp[Main](getting-started-with-mvc-part7/samples/sample1.cs)]

Führen Sie die Anwendung erneut aus, und versuchen Sie, einen Film mit einem Preis über 100 eingeben. Nachdem Sie das Formular gesendet haben, erhalten Sie einen Fehler. Der Fehler abgefangen wird, auf der Serverseite und tritt nach dem Bereitstellen des Formulars gesendet wird. Beachten Sie, wie die integrierten HTML-Hilfsmethoden des ASP.NET intelligent genug, um die Fehlermeldung angezeigt und verwalten Sie die Werte für uns innerhalb der Textbox-Elemente wurden:

[![CreateMovieWithValidation](getting-started-with-mvc-part7/_static/image2.png)](getting-started-with-mvc-part7/_static/image1.png)

Dies funktioniert hervorragend, aber es wäre schön, wenn den Benutzer auf der Clientseite sofort mitgeteilt konnte vor der Server beteiligt ruft.

Wir einige clientseitige Validierung mit JavaScript zu aktivieren.

## <a name="adding-client-side-validation"></a>Die clientseitige Validierung hinzufügen

Da unsere Film-Klasse bereits einige Validierungsattribute aufweist, müssen wir nur unsere Create.aspx Ansicht Vorlage einige JavaScript-Dateien hinzu, und fügen eine Codezeile aktivieren Sie die clientseitige Überprüfung stattfinden.

Wechseln Sie von innerhalb VWD unsere Ansichten/Film-Ordner, und öffnen Sie Create.aspx.

Öffnen Sie den Skriptordner im Projektmappen-Explorer, und ziehen Sie die folgenden drei Skripts innerhalb der &lt;Head&gt; Tag.

- MicrosoftAjax.js
- MicrosoftMvcValidation.js

Diese Skriptdateien werden in dieser Reihenfolge angezeigt werden sollen.

[!code-html[Main](getting-started-with-mvc-part7/samples/sample2.html)]

Darüber hinaus fügen Sie dieser einzelnen Zeile oberhalb der Html.BeginForm hinzu:

[!code-aspx[Main](getting-started-with-mvc-part7/samples/sample3.aspx)]

Hier ist der Code in der IDE angezeigt.

[![Filme – Microsoft Visual Web Developer 2010 Express (10)](getting-started-with-mvc-part7/_static/image4.png)](getting-started-with-mvc-part7/_static/image3.png)

Führen Sie die Anwendung und besuchen Sie /Movies/Create erneut, und klicken Sie auf erstellen, ohne Eingabe von Daten. Die Fehlermeldungen werden sofort ohne die Seite flash, ordnen wir Senden von Daten alle fort, bis der Server. Grund hierfür ist ASP.NET MVC jetzt die Eingabe der sowohl der Client (JavaScript) überprüft wird und auf dem Server.

[![Create - Windows Internet Explorer](getting-started-with-mvc-part7/_static/image6.png)](getting-started-with-mvc-part7/_static/image5.png)

Dies ist gute! Nehmen wir jetzt eine zusätzliche Spalte mit der Datenbank hinzufügen.

>[!div class="step-by-step"]
[Zurück](getting-started-with-mvc-part6.md)
[Weiter](getting-started-with-mvc-part8.md)
