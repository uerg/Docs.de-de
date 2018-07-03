---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part3
title: Hinzufügen einer Ansicht | Microsoft-Dokumentation
author: shanselman
description: Dies ist ein Tutorial für Anfänger, die die Grundlagen von ASP.NET MVC eingeführt werden. Erstellen Sie eine einfache Webanwendung, die aus einer Datenbank liest und schreibt.
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/14/2010
ms.topic: article
ms.assetid: e8f1515c-c277-47ff-a23e-224118f13f02
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part3
msc.type: authoredcontent
ms.openlocfilehash: 942d329cf0451101a24da4c3facd38b2813653d1
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/03/2018
ms.locfileid: "37365671"
---
<a name="adding-a-view"></a>Hinzufügen einer Ansicht
====================
durch [Scott Hanselman](https://github.com/shanselman)

> Dies ist ein Tutorial für Anfänger, die die Grundlagen von ASP.NET MVC eingeführt werden. Sie erstellen eine einfache Webanwendung, die aus einer Datenbank liest und schreibt. Besuchen Sie die [ASP.NET MVC-Informationscenter](../../../index.md) anderen ASP.NET MVC anhand von Tutorials und Beispiele finden.


In diesem Abschnitt werden wir sehen uns an, wie wir unsere HelloWorldController-Klasse, die eine ansichtsvorlagendatei verwenden, um Generieren von HTML-Antworten zurück an einen Client sauber zu kapseln haben können.

Beginnen Sie mit der eine ansichtsvorlage mit unseren Index-Methode. Unsere Methode wird aufgerufen, Index, und klicken Sie in der HelloWorldController werden kann. Derzeit gibt die Index()-Methode eine Zeichenfolge mit einer Meldung, die innerhalb der Controllerklasse hartcodiert ist.

[!code-csharp[Main](getting-started-with-mvc-part3/samples/sample1.cs)]

Jetzt ändern wir die Index-Methode, die stattdessen wie folgt aussehen:

[!code-csharp[Main](getting-started-with-mvc-part3/samples/sample2.cs)]

Nun fügen Sie eine ansichtsvorlage unser Projekt, das wir für unsere Index()-Methode verwenden können. Zu diesem Zweck mit der rechten Maustaste, mit der Maus an einer beliebigen Stelle in der Mitte der Index-Methode, und klicken Sie auf die Ansicht hinzufügen...

![Bild](getting-started-with-mvc-part3/_static/image1.png)

Dadurch wird das Dialogfeld "Ansicht hinzufügen" angezeigt, bietet uns einige Optionen für die wie wir möchten eine ansichtsvorlage zu erstellen, die durch die Index-Methode verwendet werden können. Vorerst keine Änderungen Sie aus, und klicken Sie einfach auf die Schaltfläche "hinzufügen".

[![Ansicht-Dialogfeld "hinzufügen"](getting-started-with-mvc-part3/_static/image3.png)](getting-started-with-mvc-part3/_static/image2.png)

Nachdem Sie auf "hinzufügen" klicken, werden ein neuer Ordner und eine neue Datei im Projektmappenordner, angezeigt, wie hier zu sehen. Ich habe jetzt einen HelloWorld-Ordner unter Ansichten, und eine Index.aspx-Datei in diesem Ordner.

[![solutionexplorerwithindex](getting-started-with-mvc-part3/_static/image5.png)](getting-started-with-mvc-part3/_static/image4.png)

Der neue Index-Datei ist auch bereits geöffnet ist und für die Bearbeitung. Hinzufügen von Text unterhalb der ersten &lt;h2&gt;Index&lt;/h2&gt; wie "Hello World".

[!code-html[Main](getting-started-with-mvc-part3/samples/sample3.html)]

Führen Sie die Anwendung, und besuchen Sie [ `http://localhost:xx/HelloWorld` ](http://localhostxx) erneut in Ihrem Browser. Die Index-Methode im Controller in diesem Beispiel keinen arbeiten durchführen, aber aufgerufen haben, "return View()" Dies wird angegeben, dass wir eine ansichtsvorlagendatei zum Rendern einer Antwort zurück an den Client verwenden möchten. Da wir den Namen der ansichtsvorlagendatei mit nicht explizit angegeben hat, standardmäßig ASP.NET MVC die Ansichtsdatei Index.aspx im Ordner "\Views\HelloWorld". Sie sehen, ist die Zeichenfolge, die wir in der Ansicht hartcodiert.

[![Index – Windows InternetExplorer](getting-started-with-mvc-part3/_static/image7.png)](getting-started-with-mvc-part3/_static/image6.png)

Das sieht recht gut. Beachten Sie jedoch, dass im Browser auf die Titel sagt, "Index" und der große Titel auf der Seite "Meine MVC-Anwendung.", sagt Diese ändern.

### <a name="changing-views-and-master-pages"></a>Ändern von Ansichten und Masterseiten

Zuerst ändern wir den Text "Meine MVC-Anwendung." Dieser Text wird freigegeben und auf jeder Seite angezeigt wird. Er wird tatsächlich in nur an einer Stelle in unserem Code, auch wenn es auf jeder Seite in unserer app ist. Wechseln Sie zu dem Ordner "/Views/Shared" im Projektmappen-Explorer, und öffnen Sie die Datei Site.Master. Diese Datei heißt einer Masterseite aus, und die freigegebenen "Shell", mit denen unsere anderen Seiten angezeigt werden kann.

Beachten Sie, die besagt ContentPlaceholder "MainContent" Text in dieser Datei ein.

[!code-aspx[Main](getting-started-with-mvc-part3/samples/sample4.aspx)]

Der Platzhalter ist, in dem alle Seiten, die Sie erstellen, "umschlossenen" auf der Masterseite angezeigt werden. Versuchen Sie es den Titel ändern, und klicken Sie dann die Anwendung ausführen, und finden Sie auf mehreren Seiten. Sie werden feststellen, dass die eine Änderung auf mehreren Seiten angezeigt wird.

[!code-html[Main](getting-started-with-mvc-part3/samples/sample5.html)]

Jetzt hat jede Seite die primäre Überschrift - ist, die H1 - von "Meine MVC-Film-Anwendung." Verarbeitet den weißen Text oben, die alle Seiten gemeinsam verwendet wird.

Hier ist die Site.Master in seiner Gesamtheit mit unserer Titel wurde geändert:

[!code-aspx[Main](getting-started-with-mvc-part3/samples/sample6.aspx)]

Jetzt ändern wir den Titel der Indexseite.

Öffnen Sie /HelloWorld/Index.aspx. Es gibt zwei Stellen zu ändern. Zunächst wird der Titel, die in den Titel der Browser, und klicken Sie dann auf die sekundäre Kopfzeile –, die H2 - sowie ist. Ich nehme sie jede geringfügig, damit Sie, welche Codeabschnitt sehen können welcher Teil der app ändert.

[!code-aspx[Main](getting-started-with-mvc-part3/samples/sample7.aspx)]

Führen Sie die app aus, und besuchen Sie /Movies. Beachten Sie, dass der Titel des Browsers, der die primäre Überschrift und die sekundären Überschriften geändert haben. Es ist einfach, umfassende Änderungen in Ihrer app mit kleinen Änderungen in Ihrer Ansicht anzeigen.

[![Filmliste – Windows InternetExplorer](getting-started-with-mvc-part3/_static/image9.png)](getting-started-with-mvc-part3/_static/image8.png)

Unsere wenig "Daten" (in diesem Fall die "Hello World!" Nachricht) war einfach schwierig jedoch codiert. Wir haben V (Ansichten), und wir C (Controller), aber noch keine M (Modell). In Kürze begleiten wir erläutert, wie eine Datenbank erstellen und Modellieren von Daten daraus abrufen.

## <a name="passing-a-viewmodel"></a>Übergeben ein "ViewModels"

Bevor wir mit einer Datenbank und über Modelle sprechen, jedoch zunächst sprechen wir über "ViewModels". Hierbei handelt es sich um Objekte, die darstellen, was eine ansichtsvorlage benötigt, um eine HTML-Antwort zurück an einen Client zu rendern. Sie werden in der Regel erstellt und durch eine Controllerklasse übergeben wird, um eine ansichtsvorlage und sollten nur die Daten, die die ansichtsvorlage benötigt – und nicht mehr enthalten.

Zuvor mit unserem HelloWorld-Beispiel, unsere Welcome() Action-Methode hat einen Namen und einen NumTimes-Parameter, und Ausgabe an den Browser. Anstatt den Controller diese Antwort direkt rendern weiterhin zu lassen, stattdessen lassen Sie uns eine kleine Klasse auf diese Daten enthalten, und übergibt diese dann über an eine ansichtsvorlage wieder verwenden, die HTML-Antwort gerendert. Auf diese Weise der Controller eine Sache, und die ansichtsvorlage umfasst eine andere – ermöglicht uns, die klare "Trennung von Anliegen" in unserer Anwendung zu verwalten.

Zurück zu der Datei HelloWorldController.cs und fügen Sie eine neue "WelcomeViewModel"-Klasse, und ändern Sie die Willkommensseite Methode im Controller. Hier ist die vollständige HelloWorldController.cs mit der neuen Klasse in der gleichen Datei.

[!code-csharp[Main](getting-started-with-mvc-part3/samples/sample8.cs)]

Obwohl es in mehrere Zeilen aufgeteilt ist, ist unsere willkommene-Methode wirklich nur zwei codeanweisungen. Die erste Anweisung fasst unsere zwei Parameter in ein "ViewModel"-Objekt, und der zweite übergibt das resultierende Objekt auf der Ansicht.

Nun benötigen wir eine ansichtsvorlage! Klicken Sie mit der rechten Maustaste in der Willkommen-Methode, und wählen Sie die Ansicht hinzufügen. Diesmal wir überprüfen "Stark typisierte Ansicht erstellen" und wählen aus der Dropdown-Liste unserer WelcomeViewModel-Klasse. Diese neue Ansicht wird lediglich zu WelcomeViewModels und keine anderen Arten von Objekten kennen.

> *Hinweis: Sie müssen kompiliert haben, einmal nach dem Hinzufügen Ihrer WelcomeViewModel für in der Dropdown-Liste angezeigt wird.*


Hier ist Ihre Ansicht hinzufügen-Dialogfeld sollte folgendermaßen aussehen. Klicken Sie auf die Schaltfläche "hinzufügen". ![Fügen Sie, dass die Ansicht rot markiert.](getting-started-with-mvc-part3/_static/image10.png)

Fügen Sie diesen Code unter der &lt;h2&gt; in Ihre neue Welcome.aspx. Wir stellen eine Schleife und begrüßen so oft wie der Benutzer meldet, dass wir sollten!

[!code-aspx[Main](getting-started-with-mvc-part3/samples/sample9.aspx)]

Beachten Sie außerdem, während Sie, die Eingaben sind, da wir diese Ansicht zu den WelcomeViewModel mitgeteilt (sie sind verheiratet, denken Sie daran?), dass wir nützliche Intellisense erhalten jedes Mal verweisen wir auf unsere Model-Objekts, wie im Screenshot unten:

[![NumTime-Quellcode](getting-started-with-mvc-part3/_static/image12.png)](getting-started-with-mvc-part3/_static/image11.png)

Führen Sie die Anwendung, und besuchen Sie `http://localhost:xx/HelloWorld/Welcome?name=Scott&numtimes=4` erneut aus. Nachdem wir Daten aus der URL nimmt er automatisch den Controller übergeben wird, der Controller die Daten in ein "ViewModel" verpackt und wird das Objekt auf unsere Ansicht übergeben. Die Ansicht als die Daten im HTML-Format dem Benutzer angezeigt.

[![Willkommen – Windows InternetExplorer](getting-started-with-mvc-part3/_static/image14.png)](getting-started-with-mvc-part3/_static/image13.png)

Das war also eine Art von einem "M" für Modell, aber nicht der Art Datenbank. Sehen wir uns, was wir gelernt haben, und erstellen Sie eine Datenbank von Filmen.

> [!div class="step-by-step"]
> [Zurück](getting-started-with-mvc-part2.md)
> [Weiter](getting-started-with-mvc-part4.md)
