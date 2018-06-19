---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part6
title: Hinzufügen einer Create-Methode, und erstellen Sie die Ansicht | Microsoft Docs
author: shanselman
description: Dies ist ein Anfänger-Lernprogramm, das die Grundlagen von ASP.NET MVC. Erstellen Sie eine einfache Web-Anwendung, die Lese- und Schreibvorgänge aus einer Datenbank.
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/14/2010
ms.topic: article
ms.assetid: a3a90963-0286-4fa0-9b3d-c230cc18b0a3
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part6
msc.type: authoredcontent
ms.openlocfilehash: 48e656a0c394b9db5baaec9c557ec38c4020d41b
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/10/2018
ms.locfileid: "30867982"
---
<a name="adding-a-create-method-and-create-view"></a>Hinzufügen einer Create-Methode und die Ansicht erstellen
====================
durch [Scott Hanselman](https://github.com/shanselman)

> Dies ist ein Anfänger-Lernprogramm, das die Grundlagen von ASP.NET MVC. Erstellen Sie eine einfache Web-Anwendung, die Lese- und Schreibvorgänge aus einer Datenbank. Besuchen Sie die [ASP.NET MVC-Trainingscenter](../../../index.md) andere ASP.NET-MVC Lernprogramme und Beispiele finden.


In diesem Abschnitt werden wir implementieren die Unterstützung notwendig, um Benutzern das Erstellen neuer Filmen in der Datenbank zu ermöglichen. Dazu müssen wir die Filme/erstellen URL-Aktion implementieren.

Implementieren die URL Filme/erstellen erfolgt in zwei Schritten. Wenn ein Benutzer zunächst die URL Filme/erstellen besucht möchten wir sie einem HTML-Formular anzuzeigen, die sie zur Eingabe eines neuen Films ausfüllen können. Klicken Sie dann, wenn der Benutzer sendet das Formular und Beiträge, die die Daten an den Server zu sichern, möchten wir den bereitgestellten Inhalt abrufen und speichern Sie ihn in die Datenbank.

Diese beiden Schritte innerhalb von zwei Create()--Methoden implementieren wir in unserer MoviesController-Klasse. Zeigt eine Methode der &lt;Formular&gt; , dass der Benutzer zum Erstellen eines neuen Films ausfüllen soll. Die zweite Methode verarbeitet die bereitgestellten Daten verarbeiten, wenn der Benutzer sendet die &lt;Formular&gt; zurück an den Server, und speichern Sie einen neuen Film innerhalb der Datenbank.

Im folgenden finden Sie der Code zur unsere MoviesController Klasse zur Implementierung dieser Funktion hinzufügen müssen:

[!code-csharp[Main](getting-started-with-mvc-part6/samples/sample1.cs)]

Der obige Code enthält alle des Codes, die wir in unserer Controller müssen.

Nun implementieren wir die Create View-Vorlage, mit dem wir ein Formular für den Benutzer angezeigt wird. Wir mit der rechten Maustaste in der ersten Create-Methode fort und wählen "Ansicht hinzufügen", um die Ansichtenvorlage für unsere Film-Formular erstellen.

Wir wählen, die wir zur navigieren Sie zu der Vorlage anzeigen ein "Films" übergeben, als die Datenklasse anzeigen und darauf hinweisen, dass "Vorlage"Create"erstellen" werden soll.

[![Fügen Sie die Ansicht](getting-started-with-mvc-part6/_static/image2.png)](getting-started-with-mvc-part6/_static/image1.png)

Nachdem Sie die Schaltfläche "hinzufügen" klicken, wird \Movies\Create.aspx Ansichtenvorlage für Sie erstellt werden. Da wir "Erstellen" in der Dropdownliste "Inhalt anzeigen" ausgewählt haben, Gerüstbau"das Dialogfeld" Ansicht hinzufügen"automatisch" einige Standardinhalt für uns. Das Gerüst erstellt eine HTML &lt;Formular&gt;, ein Platz für Validierungsfehler zu Nachrichten, und da Gerüstbau zu Filmen bekannt ist, erstellt Bezeichnung und Felder für jede Eigenschaft der Klasse.

[!code-aspx[Main](getting-started-with-mvc-part6/samples/sample2.aspx)]

Die Datenbank automatisch einem Film eine ID gewährt hat, sehen wir entfernen, da diese Felder, Referenzmodell. Unsere Erstellungsansicht-ID: Entfernen Sie die 7 Zeilen nach &lt;Legende&gt;Felder&lt;/legend&gt; wie wir nicht möchten, dass das Feld "ID" zeigen.

Nehmen wir jetzt erstellen Sie einen neuen Film und der Datenbank hinzuzufügen. Wir führen Sie dies durch die Anwendung erneut ausführen und besuchen Sie die "/ Filme" URL, und klicken Sie auf "Erstellen" verknüpfen, um einen neuen Film hinzuzufügen.

[![Create - Windows Internet Explorer](getting-started-with-mvc-part6/_static/image4.png)](getting-started-with-mvc-part6/_static/image3.png)

Wenn wir die Schaltfläche "erstellen" klicken, müssen wir die Daten auf diesem Formular an die /Movies/Create-Methode, die soeben erstellt wurde, zurück (über HTTP POST) gebucht. Das System wird genau wie beim ist das System automatisch dauerte den Parameter "NumTimes" und "Name" aus der URL und diese an einen Parameter in einer Methode weiter oben zugeordnet wird nehmen die Felder aus einer POST-Anforderung und Zuordnen eines Objekts. In diesem Fall werden die Werte von Feldern im HTML-Format, z. B. "ReleaseDate" und "Title" automatisch in die entsprechenden Eigenschaften einer neuen Instanz eines Films abgelegt werden.

Sehen wir uns die zweite Erstellungsmethode aus unserem MoviesController erneut. Beachten Sie, wie sie eine "Movie"-Objekt als Argument akzeptiert:

[!code-csharp[Main](getting-started-with-mvc-part6/samples/sample3.cs)]

Dieses Movie-Objekt klicken Sie dann auf die Version [HttpPost] unsere erstellen Aktionsmethode übergeben wurde, und wir es in der Datenbank gespeichert und anschließend den Benutzer umgeleitet, an die Aktionsmethode Index(), die das gespeicherte Ergebnis in der Filmliste angezeigt:

[![Film List – Windows Internet Explorer](getting-started-with-mvc-part6/_static/image6.png)](getting-started-with-mvc-part6/_static/image5.png)

Wir werden nicht überprüft, ob unsere Filme richtig angegeben, obwohl sind und die Datenbank wird nicht es uns ermöglichen, einen Film mit ohne Titel zu speichern. Es wäre schön, wenn wir den Benutzer informieren könnte, der vor der Datenbank einen Fehler ausgelöst hat. Wir müssen dies als Nächstes durchführen, indem validierungsunterstützung unsere Anwendung hinzufügen.

> [!div class="step-by-step"]
> [Zurück](getting-started-with-mvc-part5.md)
> [Weiter](getting-started-with-mvc-part7.md)
