---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part3
title: "Hinzufügen einer Ansicht | Microsoft Docs"
author: shanselman
description: "Dies ist ein Anfänger-Lernprogramm, das die Grundlagen von ASP.NET MVC. Erstellen Sie eine einfache Web-Anwendung, die Lese- und Schreibvorgänge aus einer Datenbank."
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/14/2010
ms.topic: article
ms.assetid: e8f1515c-c277-47ff-a23e-224118f13f02
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part3
msc.type: authoredcontent
ms.openlocfilehash: 8725d054861c857ceac10a42b0cc3f2afe056aea
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2018
---
<a name="adding-a-view"></a>Hinzufügen einer Ansicht
====================
durch [Scott Hanselman](https://github.com/shanselman)

> Dies ist ein Anfänger-Lernprogramm, das die Grundlagen von ASP.NET MVC. Erstellen Sie eine einfache Web-Anwendung, die Lese- und Schreibvorgänge aus einer Datenbank. Besuchen Sie die [ASP.NET MVC-Trainingscenter](../../../index.md) andere ASP.NET-MVC Lernprogramme und Beispiele finden.


In diesem Abschnitt werden wir betrachten, wie wir unsere HelloWorldController-Klasse, die eine Vorlagendatei Ansicht verwenden, um Generieren von HTML-Antworten an den Client ordnungsgemäß zu kapseln haben kann.

Beginnen wir unsere Index-Methode mit einer Vorlage anzeigen. Unsere Methode wird aufgerufen, Index, und in der HelloWorldController werden kann. Unsere Index()-Methode wird zurzeit eine Zeichenfolge mit einer Meldung, die innerhalb der Controllerklasse hartcodiert ist zurück.

[!code-csharp[Main](getting-started-with-mvc-part3/samples/sample1.cs)]

Jetzt ändern wir die Index-Methode, die stattdessen wie folgt aussehen:

[!code-csharp[Main](getting-started-with-mvc-part3/samples/sample2.cs)]

Fügen Sie eine Ansichtenvorlage wir nun unsere Projekt, die wir für unsere Index()-Methode verwenden können. Zu diesem Zweck mit der rechten Maustaste des Mauszeigers an einer beliebigen Stelle in der Mitte der Index-Methode, und klicken Sie auf Ansicht hinzufügen...

![Bild](getting-started-with-mvc-part3/_static/image1.png)

Hierdurch wird das Dialogfeld "Ansicht hinzufügen" angezeigt, die uns bietet einige Optionen wie wir soll beim Erstellen einer Ansichtenvorlage, die von unseren Index-Methode verwendet werden kann. Vorerst nicht geändert, und klicken Sie einfach auf die Schaltfläche "hinzufügen".

[![View-Dialogfeld "hinzufügen"](getting-started-with-mvc-part3/_static/image3.png)](getting-started-with-mvc-part3/_static/image2.png)

Nachdem Sie auf "hinzufügen" klicken, werden ein neuer Ordner und eine neue Datei im Projektmappenordner, angezeigt, wie hier zu sehen. Ich habe jetzt einen HelloWorld Unterordner Ansichten, und eine Index.aspx-Datei in diesem Ordner.

[![solutionexplorerwithindex](getting-started-with-mvc-part3/_static/image5.png)](getting-started-with-mvc-part3/_static/image4.png)

Die neue Indexdatei ist auch bereits geöffnet ist und für die Bearbeitung. Hinzufügen von Text unter der ersten &lt;h2&gt;Index&lt;/h2&gt; wie "Hello World".

[!code-html[Main](getting-started-with-mvc-part3/samples/sample3.html)]

Führen Sie die Anwendung, und besuchen Sie [ `http://localhost:xx/HelloWorld` ](http://localhostxx) erneut in Ihrem Browser. Die Index-Methode in unserer Controller in diesem Beispiel haben nicht Aufgaben mehr ausführen, aber "return View()" die ergaben, dass wir eine Vorlage-Datei verwenden, um eine Antwort zurück an den Client zu rendern wollten aufgerufen hat. Da wir den Namen der Vorlagendatei anzeigen, verwenden Sie nicht explizit angegeben wurde, verwendet ASP.NET MVC die Ansichtsdatei Index.aspx innerhalb des Ordners \Views\HelloWorld verwenden. Jetzt sehen wir die Zeichenfolge, die wir in unserer Ansicht hartcodiert.

[![Index – Windows Internet Explorer](getting-started-with-mvc-part3/_static/image7.png)](getting-started-with-mvc-part3/_static/image6.png)

Sieht ziemlich guten. Beachten Sie jedoch, dass der Browser Titel sagt "Index" und besagt, dass große Titel auf der Seite "Meine MVC-Anwendung." Wir ändern die.

### <a name="changing-views-and-master-pages"></a>Ändern von Sichten und Masterseiten

Zunächst sehen wir ändern Sie den Text "Meine MVC-Anwendung." Dieser Text wird freigegeben und auf jeder Seite angezeigt. Anscheinend tatsächlich nur einmal in der getesteten Codes, obwohl er auf jeder Seite in dieser app ist. Wechseln Sie zu dem Ordner /Views/Shared im Projektmappen-Explorer, und öffnen Sie die Datei Site.Master. Diese Datei heißt Masterseite, und der freigegebenen "Shell", die unsere anderen Seiten zu verwenden ist.

Beachten Sie, die besagt, ContentPlaceholder "MainContent dass" Text in dieser Datei.

[!code-aspx[Main](getting-started-with-mvc-part3/samples/sample4.aspx)]

Der Platzhalter ist, in dem alle Seiten, die Sie erstellen, in die Masterseite "umschlossene" angezeigt werden. Versuchen Sie es ändern des Titels, und klicken Sie dann die Anwendung ausführen und mehrere Seiten besuchen. Sie werden bemerken, dass Ihr eine Änderung auf mehreren Seiten angezeigt wird.

[!code-html[Main](getting-started-with-mvc-part3/samples/sample5.html)]

Jetzt jeder Seite der primären Überschrift - hat, ist, die H1 - "Meine MVC-Film-Anwendung." Verarbeitet die weißen Text am oberen vorhanden, die für alle Seiten freigegeben wird.

So sieht die Site.Master in seiner Gesamtheit mit unserem Titel geändert:

[!code-aspx[Main](getting-started-with-mvc-part3/samples/sample6.aspx)]

Jetzt sehen wir ändern Sie den Titel der Indexseite.

Öffnen Sie /HelloWorld/Index.aspx. Es gibt zwei Orte ändern. Zunächst wird der Titel angezeigt, die in der Titelleiste des Browsers, und klicken Sie dann auf den sekundären Header -, der ebenfalls H2 - ist. Ich stellen jeweils geringfügig, damit Sie, welche viel Code sehen können welcher Teil der app ändert.

[!code-aspx[Main](getting-started-with-mvc-part3/samples/sample7.aspx)]

Ausführen der app, und besuchen Sie /Movies. Beachten Sie, dass der Titel des Browsers, die Überschrift der primären und sekundären Überschriften geändert haben. Es ist einfach, Ihrer Ansicht big Änderungen in Ihrer app mit kleinere Änderungen vornehmen.

[![Movie List - Windows Internet Explorer](getting-started-with-mvc-part3/_static/image9.png)](getting-started-with-mvc-part3/_static/image8.png)

Unsere wenig "Daten" (in diesem Fall die "Hello World!" Nachricht) war schwierig zwar codiert. Wir haben V (Views), und wir haben C (Domänencontroller), aber noch keine M (Modell). Wir werden in Kürze exemplarisch erklärt, wie eine Datenbank erstellen und das Modelldaten daraus abrufen.

## <a name="passing-a-viewmodel"></a>Übergeben ein ViewModel

Bevor wir mit einer Datenbank aufrufen und erörtern, Modelle, jedoch, zunächst sprechen wir über "ViewModels." Hierbei handelt es sich um Objekte, die darstellen, welche Ansichtenvorlage zum Rendern einer HTML-Antwort an den Client erfordert. Sie werden i. d. r. durch eine Controllerklasse an eine Ansichtenvorlage übergeben und sollten nur die Daten, die die Vorlage anzeigen erfordert- und nicht mehr enthalten.

Zuvor mit unserem HelloWorld-Beispiel unsere Welcome() Aktionsmethode gedauert hat einen Namen und einen NumTimes-Parameter, und an den Browser ausgeben. Anstatt des Controllers direkt rendern, diese Antwort weiterhin nutzen zu können, stellen stattdessen eine kleine Klasse für die bereitzustellenden diese Daten dann über übergeben, um eine Vorlage anzeigen, um die HTML-Antwort, indem es wieder zu rendern. Auf diese Weise der Controller hat Bedenken hinsichtlich aufpassen und die Ansicht aus einem anderen – aktivieren uns klare "Trennung von Anliegen" innerhalb der Anwendung zu verwalten.

Zurück zur Datei HelloWorldController.cs und Hinzufügen einer neuen "WelcomeViewModel"-Klasse, und ändern Sie die Willkommensseite Methode im Controller. Hier ist die vollständige HelloWorldController.cs mit der neuen Klasse in der gleichen Datei.

[!code-csharp[Main](getting-started-with-mvc-part3/samples/sample8.cs)]

Obwohl er auf mehrere Zeilen ist, ist unsere Willkommen Methode wirklich nur zwei codeanweisungen. Die erste Anweisung verpackt unsere zwei Parameter in ein ViewModel-Objekt, und der zweite übergibt das resultierende Objekt in der Ansicht.

Nun benötigen wir eine Vorlage Willkommen anzeigen. Klicken Sie mit der mit der rechten Maustaste in die Willkommen-Methode aus, und wählen Sie die Ansicht hinzufügen. Dieses Mal wir überprüfen "Stark typisierte Ansicht erstellen" und unsere WelcomeViewModel-Klasse aus der Dropdownliste auswählen. Diese neue Ansicht wird nur WelcomeViewModels und keine anderen Typen von Objekten zu verstehen.

> *Hinweis: Sie müssen kompiliert haben, einmal nach dem Hinzufügen der WelcomeViewModel für in der Dropdownliste angezeigt.*


Hier ist wie Ihr Dialogfeld Ansicht hinzufügen aussehen soll. Klicken Sie auf die Schaltfläche "hinzufügen". ![Das Hinzufügen eines Eingekreister anzeigen](getting-started-with-mvc-part3/_static/image10.png)

Fügen Sie diesen Code unter der &lt;h2&gt; in Ihre neue Welcome.aspx. Stellen eine Schleife und wir begrüßen so häufig wie der Benutzer besagt, dass wir sollten!

[!code-aspx[Main](getting-started-with-mvc-part3/samples/sample9.aspx)]

Beachten Sie außerdem, während Sie, die Eingaben sind, da diese Ansicht zu den WelcomeViewModel angewiesen (sie sind verheiratet, denken Sie daran?), dass wir hilfreich Intellisense erhalten jedes Mal wir unsere Model-Objekts, wie im Screenshot unten angezeigt verweisen:

[![NumTime-Quellcode](getting-started-with-mvc-part3/_static/image12.png)](getting-started-with-mvc-part3/_static/image11.png)

Führen Sie die Anwendung, und besuchen Sie `http://localhost:xx/HelloWorld/Welcome?name=Scott&numtimes=4` erneut aus. Nun wir Daten aus der URL einnehmen, es in unserer Controller automatisch übergeben, unser Controller und die Daten in einem ViewModel Pakete und wird das Objekt in unserer Ansicht übergeben. Die Sicht als dem Benutzer die Daten als HTML angezeigt.

[![Willkommen – Windows Internet Explorer](getting-started-with-mvc-part3/_static/image14.png)](getting-started-with-mvc-part3/_static/image13.png)

Also wurde, die eine Art von einem "M" für das Modell, aber nicht die Art der Datenbank. Was wir gelernt haben, und erstellen Sie eine Datenbank von Filmen sehen wir uns.

>[!div class="step-by-step"]
[Zurück](getting-started-with-mvc-part2.md)
[Weiter](getting-started-with-mvc-part4.md)
