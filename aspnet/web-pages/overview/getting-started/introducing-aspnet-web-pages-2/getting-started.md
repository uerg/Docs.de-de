---
uid: web-pages/overview/getting-started/introducing-aspnet-web-pages-2/getting-started
title: Einführung in ASP.NET Web Pages - erste Schritte | Microsoft-Dokumentation
author: tfitzmac
description: WebMatrix wird nicht mehr als eine integrierte Entwicklungsumgebung für ASP.NET Web Pages empfohlen. Verwenden Sie Visual Studio oder Visual Studio-Code. Diese Anleitung ein...
ms.author: aspnetcontent
ms.date: 05/28/2015
ms.assetid: a36d3bdf-ef1b-47a4-b932-3a0cf4cad716
msc.legacyurl: /web-pages/overview/getting-started/introducing-aspnet-web-pages-2/getting-started
msc.type: authoredcontent
ms.openlocfilehash: b4f554d2bf8bf564fd69239fcc7cc605158c83c3
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2018
ms.locfileid: "37825036"
---
<a name="introducing-aspnet-web-pages---getting-started"></a>Einführung in ASP.NET Web Pages - erste Schritte
====================
durch [Tom FitzMacken](https://github.com/tfitzmac)

> > [!NOTE] 
> > 
> > WebMatrix wird nicht mehr als eine integrierte Entwicklungsumgebung für ASP.NET Web Pages empfohlen. Verwendung [Visual Studio](../program-asp-net-web-pages-in-visual-studio.md) oder [Visual Studio-Code](https://code.visualstudio.com/).
> 
> 
> Dieser Leitfaden und die Anwendung bietet Ihnen eine Übersicht der ASP.NET Web Pages (Version 2 oder höher) und die Razor-Syntax, ein schlankes Framework zum Erstellen dynamischer Websites. Er führt außerdem WebMatrix, ein Tool zum Erstellen von Seiten und Websites.
> 
> **Ebene**: noch nicht mit ASP.NET Web Pages.  
> **Kenntnisse davon ausgegangen, dass**: HTML, grundlegende cascading Stylesheets (CSS).
> 
> Im ersten Tutorial der Reihe lernen Sie:
> 
> - Welche Technologie ASP.NET Web Pages ist, und wozu es dient.
> - Was WebMatrix ist.
> - Wie um alles zu installieren.
> - So erstellen Sie eine Website mit WebMatrix.
>   
> 
> Features/Technologien erläutert:
> 
> - Microsoft-Webplattform-Installer.
> - WebMatrix.
> - *cshtml* Seiten
>   
> 
> In diesem Tutorial wurde ursprünglich von Mike Pope geschrieben. Tom FitzMacken, die sie für Microsoft WebMatrix 3 aktualisiert.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Softwareversionen, die in diesem Tutorial verwendet werden.
> 
> 
> - ASP.NET Web Pages (Razor) 2
> - WebMatrix 3


## <a name="what-should-you-know"></a>Sollten Sie wissen?

Es wird davon ausgegangen, dass Sie mit vertraut sind:

- **HTML**. Es ist keine detaillierten Kenntnisse erforderlich. Wird nicht erläutert, HTML, aber wir auch nicht verwenden, komplexe Funktionen. Wir gebe enthält Links zu HTML-Tutorials, in dem wir denken, dass sie hilfreich sind.
- **Cascading Stylesheets (CSS)**. Identisch mit HTML.
- **Basic-Datenbank-Ideen**. Wenn Sie eine Tabelle für Daten verwendet und sortiert und gefiltert, die Daten, die Kenntnisse haben sind wir in der Regel für diese tutorialreihe davon.

Außerdem wird es davon ausgegangen, dass Sie die basic-Programmierung interessiert sind. ASP.NET Web Pages verwenden die Programmiersprache c#. Sie müssen keine Hintergrund in die Programmierung, nur ein Interesse haben. Wenn Sie jemals JavaScript auf einer Webseite, bevor Sie geschrieben haben, haben Sie viele des Hintergrunds.

Beachten Sie, dass wenn Sie mit der Programmierung vertraut sind, finden Sie möglicherweise, dass in diesem Tutorial legen Sie zunächst langsam bewegt wird, während wir beschleunigen Anfänger bringen. Wie wir über die ersten Paar Lernprogramme eingehen, jedoch weniger basic-Programmierung erläutert werden, und Dinge werden an einen schnelleren Clip entlang verschoben.

## <a name="what-do-you-need"></a>Was benötigen Sie?

Sie benötigen folgendes:

- Ein Computer, auf der Windows 8, Windows 7, Windows Server 2008 oder Windows Server 2012 ausgeführt wird.
- Eine aktive Internetverbindung.
- Administratorberechtigungen (für den Installationsvorgang erforderlich).

## <a name="what-is-aspnet-web-pages"></a>Was ist ASP.NET Web Pages?

ASP.NET Web Pages ist ein Framework, das Sie zum Erstellen dynamischer Webseiten verwenden können. Eine einfache HTML-Webseite ist statisch. der Inhalt wird durch das festen HTML-Markup bestimmt, die auf der Seite. Dynamische Seiten wie jene, die Sie mit ASP.NET Web Pages erstellen können Sie den Seiteninhalt dynamisch zu erstellen, mithilfe von Code.

Dynamische Seiten können Sie viele Aufgaben ausführen. Sie können bitten Sie einen Benutzer über ein Formular für die Eingabe, und ändern Sie das auf die Seite angezeigt oder wie er aussieht. Sie können Informationen von einem Benutzer nutzen, speichern Sie ihn in einer Datenbank und Listen Sie es später noch mal. Sie können e-Mail-Adresse von Ihrem Standort senden. Sie können mit anderen Diensten im Internet (z. B. ein Mapping-Dienst) zu interagieren und erstellen Seiten, die Informationen aus diesen Quellen integrieren.

## <a name="what-is-webmatrix"></a>Was ist die WebMatrix?

WebMatrix ist ein Tool, das integriert werden, einem Webseiteneditor, ein Datenbankdienstprogramm, einen Webserver für Tests, Seiten und Features für die Veröffentlichung Ihrer Websites mit dem Internet. WebMatrix ist kostenlos, und es ist einfach zu installieren und einfach zu verwenden. (Es funktioniert auch, für die einfach HTML-Seiten sowie für andere Technologien wie PHP.)

Sie nicht die eigentliche *haben* zu WebMatrix verwenden, um die Arbeit mit ASP.NET Web Pages. Sie können Seiten erstellen, mit einem Text-Editor, z. B. und Testen von Seiten mit einem Webserver, dem Sie können zugreifen. Allerdings vereinfacht WebMatrix alles sehr einfach, daher in diesen Tutorials WebMatrix während des gesamten verwenden.

## <a name="about-these-tutorials"></a>Informationen zu diesen Tutorials

Diese tutorialreihe ist eine Einführung in ASP.NET Web Pages verwenden. Es gibt 9 Tutorials in dieser einführende Tutorial insgesamt. Sie ist Teil einer Reihe von Tutorials legt, der von ASP.NET Web Pages-Anfänger bis zum Erstellen von Websites für echte, professionell aussehende führt.

In diesem ersten Tutorial legen er konzentriert sich auf zeigt Ihnen die Grundlagen der Arbeit mit ASP.NET Web Pages. Wenn Sie fertig sind, können Sie mit weiteren Tutorials arbeiten, die weitermachen, wo diese endet und, die Webseiten genauer untersuchen.

Gehen wir absichtlich einfach der ausführliche erläuterungen. Und wenn wir etwas zeigen, für diese tutorialreihe wir immer wählen Sie die Möglichkeit, die wir denken am einfachsten zu verstehen ist. Späteren Tutorial mehr ins Detail gehen und zeigen Ihnen, eine effizientere oder eine flexiblere Methoden (auch mehr Spaß). Aber diese Tutorials benötigen Sie zunächst mit die Grundlagen vertraut machen.

Der tutorialreihe, die Sie gerade gestartet haben, behandelt diese Features von ASP.NET Web Pages:

- Einführung und erste alle Komponenten installiert sein. (Das ist in diesem Tutorial aus, die, das Sie lesen.)
- Die Grundlagen der Programmierung von ASP.NET Web Pages.
- Erstellen einer Datenbank an.
- Erstellen und Verarbeiten von einem Benutzer geben Sie ein Formular.
- Hinzufügen, aktualisieren und Löschen von Daten in der Datenbank.

## <a name="what-will-you-create"></a>Was werden Sie erstellen?

Legen Sie in diesem Tutorial aus, und alle weiteren beziehen sich auf das eine Website, in dem Sie Filme auflisten können, die Ihnen gefällt. Sie werden Filme eingeben, bearbeiten und auflisten können. Hier sind einige der Seiten, die Sie in diesen Tutorials erstellen. Der erste Screenshot zeigt den Film vorstellungsseite, die Sie erstellen:

![Hat-Film-app mit einer Movie-Liste](getting-started/_static/image1.png)

Und hier ist die Seite, die Sie neue Informationen zu Ihrer Website hinzufügen kann:

![Fertige Film-app, die mit der Seite "Film hinzufügen"](getting-started/_static/image2.png)

Nachfolgende Lernprogramm legt Build dazu legen, und fügen weitere Funktionen, z. B. das Hochladen von Bildern, lassen Personen, melden Sie sich, Senden von e-Mails und Integrieren von sozialen Medien.

## <a name="see-this-app-running-on-azure"></a>Finden Sie unter dieser App in Azure ausführen

Möchten Sie die fertige Website als live-Web-app ausgeführt wird, finden Sie unter? Sie können eine vollständige Version der app beim Azure-Konto bereitstellen, indem Sie einfach die folgende Schaltfläche.

[![](http://azuredeploy.net/deploybutton.png)](https://azuredeploy.net/?WT.mc_id=deploy_azure_aspnet&repository=https://github.com/tfitzmac/WebPagesMovies)

Sie benötigen ein Azure-Konto zum Bereitstellen dieser Lösung in Azure. Wenn Sie nicht bereits über ein Konto verfügen, müssen Sie die folgenden Optionen aus:

- [Öffnen Sie ein Azure-Konto kostenlos](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A443DD604) – Sie erhalten ein Guthaben zum Ausprobieren Zahlungspflichtiger Azure-Dienste nutzen können, und auch nach dem sie verwendet werden, bis können Sie das Konto behalten und kostenlose Azure-Dienste.
- [MSDN-abonnentenvorteile aktivieren](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604) -Ihr MSDN-Abonnement ein monatliches Guthaben, die Sie für zahlungspflichtige Azure-Dienste verwenden können.

## <a name="installing-everything"></a>Installieren alle Elemente

Sie können alles, was mithilfe von Microsoft Web Platform Installer installieren. Aktiviert ist, Sie installieren Sie den Installer, und dann verwenden, um alles zu installieren.

Um Webseiten zu verwenden, müssen Sie mindestens aufweisen Windows XP mit SP3 oder WindowsServer 2008 oder höher.

Auf der [Web Pages-Seite](../../../index.md) der ASP.NET-Website, klicken Sie auf **installieren**.

![ASP.NET Web Site mit &quot;Installieren von WebMatrix&quot; Schaltfläche](getting-started/_static/image3.png)

Sie werden aufgefordert, um die Lizenzbedingungen und Datenschutzbestimmungen vor der Installation von WebMatrix zu akzeptieren.

![Akzeptieren Sie die Laufzeit, um die Installation zu beginnen](getting-started/_static/image4.png)

Klicken Sie auf **ausführen** um die Installation zu starten. (Wenn Sie das Installationsprogramm zu speichern möchten, klicken Sie auf **speichern** und führen Sie den Installer aus dem Ordner, in dem Sie sie gespeichert haben.)

![](getting-started/_static/image5.png)

Der Webplattform-Installer angezeigt wird, bereit zum Installieren von WebMatrix. Klicken Sie auf **Installieren**.

![](getting-started/_static/image6.png)

Während des Installationsvorgangs herausfindet, was es auf Ihrem Computer installiert hat, und startet den Prozess. Je nachdem, was genau hat installiert werden kann der Prozess in wenigen Augenblicken mehrere Minuten dauern. Wählen Sie **akzeptieren** zu den Lizenzbedingungen zustimmen.

## <a name="hello-webmatrix"></a>Hallo, WebMatrix

Wenn dies abgeschlossen ist, kann während des Installationsvorgangs WebMatrix automatisch gestartet. Wenn nicht der Fall, in Windows, von ist das **starten** Menü **Microsoft WebMatrix**.

Wenn Sie WebMatrix zum ersten Mal starten, erhalten Sie Gelegenheit, sich mit Ihrem Microsoft-Konto in Microsoft Azure anzumelden. Indem Sie sich anmelden, erhalten Sie 10 kostenlose Web-apps über Azure. Diese kostenlosen Web-apps bieten eine praktische Möglichkeit zum Testen Ihrer apps. Wenn Sie nicht bereits über ein Azure-Konto verfügen, aber Sie ein MSDN-Abonnement müssen, können Sie [aktivieren Sie Ihr MSDN-abonnementleistungen](https://www.windowsazure.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604). Andernfalls können Sie in wenigen Minuten ein kostenloses Testkonto erstellen. Weitere Informationen finden Sie unter [kostenlose Azure-Testversion](https://azure.microsoft.com/free/?WT.mc_id=A443DD604).

Sie müssen keinen jetzt bei anmelden, um dieses Tutorial fortsetzen. Wenn Sie keine Anmeldung jetzt, müssen Sie weiterhin die Option später anzumelden. Die letzte [Thema](publishing.md) Reihe wird in diesem Tutorial beschrieben, wie Ihre Website in Azure bereitstellen; Sie müssen daher anmelden, um dieses Thema abzuschließen.

An diesem Punkt entweder melden Sie sich mit Ihrem Microsoft- oder Select **nicht jetzt** in der unteren rechten Ecke.

![Anmelden](getting-started/_static/image7.png)

Um zu beginnen, Sie erstellen eine leere Website und fügen Sie eine Seite. In einem späteren Tutorial in dieser Gruppe müssen Sie mit einem der integrierten Websitevorlagen wiedergeben.

Klicken Sie im Startfenster auf **neu**.

![WebMatrix-Startbildschirm](getting-started/_static/image8.png)

Vorlagen sind vordefinierte Dateien und Seiten für unterschiedliche Arten von Websites. Um alle Vorlagen anzuzeigen, die standardmäßig verfügbar sind, wählen Sie die Vorlagenkatalog-Option.

![Vorlagenkatalog auswählen](getting-started/_static/image9.png)

In der **Schnellstart** wählen Sie im Fenster **leere Website** aus der **ASP.NET** Gruppe, und nennen Sie die neue Website "WebPagesMovies".

![WebMatrix-Schnellstart-Fenster mit leere Websitevorlage ausgewählt](getting-started/_static/image10.png)

Klicken Sie auf **Weiter**.

Wenn Sie sich mit Ihrem Microsoft-Konto angemeldet haben, werden Sie die Möglichkeit zum Erstellen der Website auf Azure erhalten. Basierend auf den Namen Ihrer Website ein, den Standardnamen des **WebPagesMovies.azurewebsites.net** empfohlen; allerdings das Ausrufezeichen gibt an, dass dieser Name nicht in Windows Azure verfügbar ist. Wählen Sie aus Gründen der Einfachheit **überspringen** , erstellen jetzt die Website in Azure zu umgehen. Weiter unten in dieser Reihe werden wir die Website in Azure veröffentlichen.

![Erstellen von Azure-Website](getting-started/_static/image11.png)

WebMatrix erstellt und öffnet die Website:

![Öffnen Sie die neue WebPagesMovies-Website in WebMatrix](getting-started/_static/image12.png)

Im oberen Bereich besteht eine Symbolleiste für den Schnellzugriff und ein Menüband. Unten links, daraufhin wird die arbeitsbereichsauswahl, in dem Sie wechseln zwischen Aufgaben (**Site**, **Dateien**, **Datenbanken**, **Berichte**). Auf der rechten Seite ist im Bereich Inhalte des für den Editor sowie für Berichte. Und am unteren gelegentlich sehen Sie eine Benachrichtigungsleiste für Nachrichten.

Sie erfahren mehr über WebMatrix und der zugehörigen Funktionen, die Sie, wie Sie in diesen Tutorials durcharbeiten.

## <a name="creating-a-web-page"></a>Erstellen einer Webseite

Um mit WebMatrix und ASP.NET Web Pages vertraut sind, erstellen Sie eine einfache Seite.

Wählen Sie in der arbeitsbereichsauswahl den **Dateien** Arbeitsbereich. Diesem Arbeitsbereich können Sie die Arbeit mit Dateien und Ordner. Im linken Bereich werden die Dateistruktur Ihrer Website. Die Menüband-Änderungen Aufgaben im Zusammenhang mit Datei angezeigt werden soll.

![Datei-Arbeitsbereich in WebMatrix](getting-started/_static/image13.png)

Klicken Sie im Menüband auf den Pfeil unter **neu** , und klicken Sie dann auf **neue Datei**.

![Mithilfe der &quot;neu&quot; -Befehl in der Multifunktionsleiste, um eine neue Datei erstellen](getting-started/_static/image14.png)

WebMatrix wird eine Liste der Dateitypen angezeigt. Wählen Sie **CSHTML**, und klicken Sie in der **Namen** geben "HelloWorld". Eine CSHTML-Seite ist eine ASP.NET Web Pages-Seite.

![Erstellen einer neuen CSHTML-Seite mit dem Namen HelloWorld.cshtml](getting-started/_static/image15.png)

Klicken Sie auf **OK**.

WebMatrix erstellt die Seite, und es im Editor geöffnet.

![Die neue HelloWorld-Seite in der WebMatrix-editor](getting-started/_static/image16.png)

Wie Sie sehen, enthält die Seite größtenteils normale HTML-Markup, mit Ausnahme von einem Block an der Spitze, die folgendermaßen aussieht:

[!code-cshtml[Main](getting-started/samples/sample1.cshtml)]

Die zum Hinzufügen von Code, wie Sie bald sehen werden.

Beachten Sie, dass die verschiedenen Teile der Seite &mdash; Elementnamen, Attribute und Text sowie der Block an der Spitze – werden alle in verschiedenen Farben. Dies wird als bezeichnet *syntaxhervorhebung*, und erleichtert es, Alles klar zu halten. Es ist eine der Funktionen, die für die Arbeit mit Web Pages in WebMatrix erleichtert.

Hinzufügen von Inhalt für die `<head>` und `<body>` Elemente wie im folgenden Beispiel. (Wenn Sie möchten, klicken Sie einfach kopieren Sie den folgenden Block und die gesamte vorhandene Seite durch den folgenden Code ersetzen.)

[!code-cshtml[Main](getting-started/samples/sample2.cshtml)]

In der Symbolleiste für den Schnellzugriff oder in der **Datei** Menü klicken Sie auf **speichern**.

![Speichern Sie Schaltfläche "" in WebMatrix die Symbolleiste für Schnellzugriff](getting-started/_static/image17.png)

## <a name="testing-the-page"></a>Testen der Seite

In der **Dateien** Arbeitsbereich mit der rechten Maustaste die *HelloWorld.cshtml* Seite, und klicken Sie dann auf **in Browser starten**.

![Ausführen einer Seite mit der Schaltfläche "ausführen" im Menüband WebMatrix](getting-started/_static/image18.png)

WebMatrix startet einen integrierten Webserver (IIS Express), den Sie, zum Testen von Seiten auf Ihrem Computer verwenden können. (Ohne IIS Express in WebMatrix müssten Sie veröffentlichen, um die Seite auf einem Webserver an einer beliebigen Stelle Sie getestet werden konnte.) Die Seite wird im Standardbrowser angezeigt.

![&quot;Hallo Welt&quot; Seite im Browser ausgeführt wird](getting-started/_static/image19.png)

Beachten Sie, dass beim Testen einer Seite in WebMatrix die URL im Browser etwa `http://localhost:33651/HelloWorld.cshtml.` den Namen *"localhost"* bezieht sich auf einem lokalen Server, was bedeutet, dass die Seite von einem Webserver bereitgestellt wird, die auf Ihrem eigenen Computer befindet. Wie bereits erwähnt, enthält WebMatrix ein Web-Server-Programm mit dem Namen IIS Express, die ausgeführt wird, wenn Sie eine Seite starten.

Die Zahl nach *"localhost"* (z. B. *Localhost:33651*) bezieht sich auf eine *Portnummer* auf Ihrem Computer. Dies ist die Anzahl der "Channel", die für diese bestimmte Website IIS Express verwendet werden. Die Portnummer wird nach dem Zufallsprinzip aus dem Bereich von 1024 bis 65536 ausgewählt, wenn Sie Ihre Website erstellen, und sie unterscheidet sich für jeden Standort, die Sie erstellen. (Wenn Sie Ihre eigene Website testen, wird die Nummer des Ports mit großer Wahrscheinlichkeit eine andere Zahl als 33561 sein.) Verwenden Sie einen anderen Port für jede Website, können IIS Express gerade die Ihrer Websites bewahren, es kommuniziert.

Später, wenn Sie einen öffentlichen Webserver Ihrer Website veröffentlichen daraufhin nicht mehr *"localhost"* in der URL. An diesem Punkt sehen Sie eine typische URL wie `http://myhostingsite/mywebsite/HelloWorld.cshtml` oder beliebige die Seite ist. Sie erfahren mehr zum Veröffentlichen einer Websites weiter unten in diesem Tutorial-Reihe.

## <a name="adding-some-server-side-code"></a>Hinzufügen von serverseitigen Code

Schließen Sie den Browser, und wechseln Sie zurück zur Seite in WebMatrix.

Fügen Sie eine Zeile an den Codeblock, damit sie sich wie der folgende Code aussieht:

[!code-cshtml[Main](getting-started/samples/sample3.cshtml)]

Dies ist ein wenig mit Razor-Code. Es ist wahrscheinlich klar, dass er das aktuelle Datum und die Uhrzeit ruft und den Wert in eine *Variable* mit dem Namen `currentDateTime`. Lesen Sie weitere Informationen zu Razor-Syntax im nächsten Tutorial.

Im Hauptteil der Seite nach der `<p>Hello World!</p>` -Element, fügen Sie Folgendes hinzu:

[!code-html[Main](getting-started/samples/sample4.html)]

Dieser Code Ruft den Wert ab, die Sie für die Bereitstellen der `currentDateTime` -Variable oben und fügt es in das Markup der Seite. Die `@` Zeichen kennzeichnet, die ASP.NET Web Pages-Code auf der Seite.

Führen Sie die Seite erneut (WebMatrix speichert die Änderungen für Sie vor der Ausführung der Seite "). Dieses Mal sehen, wenn Sie das Datum und die Uhrzeit auf der Seite.

![&quot;Hallo Welt&quot; Seite im Browser mit einem dynamisch generierten Zeitanzeige ausführen](getting-started/_static/image20.png)

Warten Sie einige Minuten, und klicken Sie dann aktualisieren Sie die Seite im Browser zu. Die Anzeige von Datum und Uhrzeit wird aktualisiert.

Betrachten Sie den Quellcode der Seite im Browser aus. Es sieht aus wie das folgende Markup:

[!code-html[Main](getting-started/samples/sample5.html)]

Beachten Sie, dass die `@{ }` Block an der Spitze nicht vorhanden ist. Beachten Sie außerdem, dass das Datum und Uhrzeit eine tatsächliche Zeichenfolge zeigt (`1/18/2012 2:49:50 PM` bzw. der jeweiligen) und nicht `@currentDateTime` wie Sie in mussten der *.cshtml* Seite. Was passiert hier ist, wenn Sie die Seite ausgeführt haben, mit ASP.NET den Code (nur wenig in diesem Fall) verarbeitet wird, die mit `@`. Der Code erzeugt die Ausgabe, und diese Ausgabe in die Seite eingefügt wurde.

## <a name="this-is-what-aspnet-web-pages-are-about"></a>Dies ist, was zu ASP.NET Web Pages sind

Wenn Sie gelesen werden, dass ASP.NET Web Pages dynamische Webinhalte erzeugt, ist was Sie hier gesehen haben die Idee. Die Seite, die Sie gerade erstellt haben, enthält dasselbe HTML-Markup, dem Sie vor dem gesehen haben. Sie können auch Code enthält, die alle möglichen Aufgaben ausführen können. In diesem Beispiel haben sie die einfache Aufgabe abrufen, das aktuelle Datum und die Uhrzeit. Wie Sie gesehen haben, können Sie Code mit der HTML-Code, um die Ausgabe auf der Seite einzufügen. Wenn ein Benutzer anfordert, eine *.cshtml* Seite im Browser, ASP.NET verarbeitet die Bild-auf, während sie weiterhin in die Hände des Webservers befindet. ASP.NET fügt die Ausgabe des Codes (sofern vorhanden) in der Seite im HTML-Format. Wenn die Verarbeitung der Code abgeschlossen ist, sendet ASP.NET die entsprechende Seite an den Browser. Alles, was der Browser jemals ruft ist HTML. Es folgt ein Diagramm aus:

![Konzeptioneller Ablauf der wie ASP.NET HTML dynamisch generiert](getting-started/_static/image21.png)

Die Idee ist einfach, aber es gibt viele interessante Aufgaben, die der Code ausführen können, und es gibt viele interessante Möglichkeiten, die in denen Sie auf der Seite dynamisch HTML-Inhalt hinzufügen können. Und ASP.NET *.cshtml* Seiten, wie alle HTML-Seite können auch enthalten Code, der in den Browser selbst (JavaScript und jQuery-Code) ausgeführt wird. Sie müssen alle diese Dinge in dieser tutorialreihe und alle weiteren durchsuchen.

## <a name="coming-up-next"></a>Als Nächstes kommen

Im nächsten Tutorial dieser Reihe untersuchen Sie die ASP.NET Web Pages Programmieren ein bisschen mehr.

## <a name="additional-resources"></a>Zusätzliche Ressourcen

[Erstellen eine ASP.NET-Website von Grund auf Neu](https://www.microsoft.com/web/post/create-an-aspnet-website-from-scratch). Dies ist ein Tutorial, das speziell zur Verwendung von WebMatrix (nicht ASP.NET Web Pages). Danach wird in ein wenig genauer Informationen zu den zusätzlichen Features des WebMatrix, die wir nicht in diesen Tutorials erläutert.

> [!div class="step-by-step"]
> [Nächste](intro-to-web-pages-programming.md)
