---
uid: web-pages/overview/getting-started/introducing-aspnet-web-pages-2/getting-started
title: "Einführung in ASP.NET Web Pages - erste Schritte | Microsoft Docs"
author: tfitzmac
description: "WebMatrix wird nicht mehr als eine integrierte Entwicklungsumgebung für ASP.NET Web Pages empfohlen. Verwenden Sie Visual Studio oder Visual Studio-Code. In dieser Anleitung ein..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/28/2015
ms.topic: article
ms.assetid: a36d3bdf-ef1b-47a4-b932-3a0cf4cad716
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/getting-started/introducing-aspnet-web-pages-2/getting-started
msc.type: authoredcontent
ms.openlocfilehash: 615ddc31d0d857e5bf9a7f372b7efcf67d185905
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="introducing-aspnet-web-pages---getting-started"></a>Einführung in ASP.NET Web Pages - erste Schritte
====================
durch [Tom FitzMacken](https://github.com/tfitzmac)

> > [!NOTE] 
> > 
> > WebMatrix wird nicht mehr als eine integrierte Entwicklungsumgebung für ASP.NET Web Pages empfohlen. Verwendung [Visual Studio](../program-asp-net-web-pages-in-visual-studio.md) oder [Visual Studio-Code](https://code.visualstudio.com/).
> 
> 
> Diese Anleitung und die Anwendung bietet Ihnen eine Übersicht der ASP.NET Web Pages (Version 2 oder höher) und Razor-Syntax, ein einfaches Framework zum Erstellen dynamischer Websites. Darüber hinaus gibt es WebMatrix, ein Tool zum Erstellen von Seiten und Websites.
> 
> **Ebene**: noch nicht mit ASP.NET Web Pages.  
> **Kenntnisse vorausgesetzt**: HTML, grundlegende cascading Stylesheets (CSS).
> 
> In der Menge der ersten Lernprogramm lernen Sie:
> 
> - Welche ASP.NET Web Pages-Technologie ist, und was für ist.
> - WebMatrix ist.
> - Zum Installieren aller erforderlichen Komponenten.
> - Vorgehensweise zum Erstellen einer Website mit WebMatrix.
>   
> 
> Features/Technologien erläutert:
> 
> - Microsoft Web Platform Installer.
> - WebMatrix.
> - *cshtml* Seiten
>   
> 
> Dieses Lernprogramm wurde ursprünglich von Mike Pope geschrieben. Tom FitzMacken, die sie für Microsoft WebMatrix 3 aktualisiert.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>In diesem Lernprogramm verwendeten Versionen der Software
> 
> 
> - ASP.NET Web Pages (Razor) 2
> - WebMatrix-3


## <a name="what-should-you-know"></a>Was wissen Sie sollten?

Es wird davon ausgegangen, dass Sie mit vertraut sind:

- **HTML**. Es ist keine Fachwissen erforderlich. Wird nicht erläutert, HTML, aber wir auch keine verwenden komplexe. Wir szenariobereitstellung erhalten Sie Links zu HTML-Lernprogrammen, in denen wir glauben, dass sie hilfreich sind.
- **Cascading Stylesheets (CSS)**. Identisch mit HTML.
- **Grundlegende Datenbank Ideen**. Wenn Sie ein Arbeitsblatt für Daten verwendet und sortiert und gefiltert, die Daten, die Kenntnisse haben sind es in der Regel für dieses Lernprogramm Satz angenommen.

Wir sind auch davon ausgegangen, dass Sie lernen die grundlegende Programmierung interessiert sind. ASP.NET Web Pages verwenden einen Programmiersprache Ihrer Wahl namens c#. Sie haben keiner Hintergrund in Programmiersprachen, nur relevante darin haben. Wenn Sie jemals JavaScript in einer Webseite, bevor Sie geschrieben haben, haben Sie ausreichend Hintergrund erhalten haben.

Beachten Sie, dass wenn Sie mit der Programmierung vertraut sind, gefunden werden kann, dass dieses Lernprogramm Anfangs festgelegt, langsam bewegt, während wir neue Programmierer alles bringen. Wie wir nach der ersten einige Lernprogramme erhalten, jedoch weniger grundlegende Programmierung erläutert werden, und Punkte entlang auf einen schnelleren Clip verschieben.

## <a name="what-do-you-need"></a>Was benötigen Sie?

Sie benötigen folgendes:

- Ein Computer, auf der Windows 8, Windows 7, Windows Server 2008 oder Windows Server 2012 ausgeführt wird.
- Eine aktive Internetverbindung.
- Administratorrechte (erforderlich für den Installationsvorgang).

## <a name="what-is-aspnet-web-pages"></a>Was ist der ASP.NET Web Pages?

ASP.NET Web Pages ist ein Framework, das Sie verwenden können, um dynamische Webseiten zu erstellen. Eine einfache HTML-Webseite ist statisch. seinen Inhalt richtet sich nach den festen HTML-Markup, das auf der Seite ist. Dynamische Seiten wie diejenigen, die Sie mit ASP.NET Web Pages erstellen können Sie den Seiteninhalt im Handumdrehen zu erstellen, mithilfe von Code.

Dynamische Seiten können Sie alle Arten von Aufgaben durchführen. Sie können bitten Sie einen Benutzer über ein Formular für die Eingabe, und ändern Sie das auf die Seite angezeigt, oder wie er aussieht. Sie können Informationen von einem Benutzer übernehmen, in einer Datenbank zu speichern und dann weiter unten aufgeführt. Sie können das Senden von e-Mails von Ihrem Standort. Sie können mit anderen Diensten im Web (z. B. ein Mapping-Dienst) interagieren und Erzeugen von Seiten, die Informationen aus diesen Quellen integrieren.

## <a name="what-is-webmatrix"></a>Was ist WebMatrix?

WebMatrix ist ein Tool, das eine Webseiten-Editor, ein Hilfsprogramm für die Datenbank, einen Webserver für Tests, Seiten und Funktionen für die Veröffentlichung Ihrer Websites mit dem Internet integriert. WebMatrix ist kostenlos, und es ist einfach zu installieren und einfach zu verwenden. (sie kann eingesetzt werden für nur einfache HTML-Seiten, sowie für andere Technologien wie PHP.)

Sie nicht tatsächlich *haben* zu WebMatrix verwenden, um das Arbeiten mit ASP.NET Web Pages. Sie können Seiten erstellen, mit einem Text-Editor, z. B. und Testen von Seiten mit einem Webserver, dem Sie können zugreifen. Allerdings erleichtert WebMatrix alle, daher diese Lernprogramme in der gesamten WebMatrix verwenden.

## <a name="about-these-tutorials"></a>Über diese Lernprogramme

Dieses Lernprogramm ist eine Einführung in die Verwendung von ASP.NET Web Pages. Es gibt 9 Lernprogramme in diesem Lernprogramm einführenden Satz insgesamt. Es handelt sich um Teil einer Reihe von Tutorial legt fest, der für das Erstellen von real, professionelle Websites von ASP.NET Web Pages-Anfänger führt.

Diese ersten Lernprogramm festgelegt konzentriert sich auf die Grundlagen zum Arbeiten mit ASP.NET Web Pages anzeigen. Wenn Sie fertig sind, können Sie mit zusätzlichen Tutorial Sätze arbeiten, die übernehmen, in dem dieses Objekt beendet und genauer zu untersuchen, die Webseiten.

Wir fahren Sie absichtlich einfach ausführlichen Erklärungen. Und wenn es etwas anzuzeigen, für dieses Lernprogramm Satz wir immer wählen, dass die Möglichkeit, die wir glauben, dass am einfachsten zu verstehen ist. Späteren Lernprogramme Mengen mehr ins Detail gehen und zeigen Ihnen, eine effizientere oder flexibler Ansätze (auch mehr Spaß). Aber diese Lernprogramme benötigen Sie zunächst mit die Grundlagen vertraut machen.

Das Lernprogramm festzulegen, das Sie soeben gestartet haben, werden diese Features von ASP.NET Web Pages behandelt:

- Einführung und alles installiert abrufen. (Das ist in diesem Lernprogramm, das Sie gerade lesen.)
- Die Grundlagen der ASP.NET Web Pages-Programmierung.
- Erstellen einer Datenbank an.
- Erstellen und Verarbeiten einer Benutzereingabeformular.
- Hinzufügen, aktualisieren und Löschen von Daten in der Datenbank.

## <a name="what-will-you-create"></a>Was werden Sie erstellen?

Dieses Lernprogramm festgelegt, und alle weiteren beziehen sich auf einer Website, in dem Sie Filme auflisten können, die Ihnen gefällt. Sie müssen möglicherweise Filme eingeben, bearbeiten und führen Sie sie. Hier sind einige der Seiten, die Sie in diesem Lernprogramm Satz erstellen. Das erste Schema zeigt den Film Angebot Seite, die Sie erstellen müssen:

![Zeigt eine Liste der Film Finshed Film-app](getting-started/_static/image1.png)

Und hier ist die Seite, die Sie neue Informationen zu Ihrer Website hinzufügen kann:

![Fertigen Film-app, die mit der Seite "Movie hinzufügen"](getting-started/_static/image2.png)

Nachfolgende Lernprogramm legt der Build für dieses festlegen und weitere Funktionen hinzufügen, z. B. Bilder hochladen, können Personen, melden Sie sich, Senden von e-Mail und die Integration in soziale Medien.

## <a name="see-this-app-running-on-azure"></a>Finden Sie unter dieser App auf Azure ausgeführt wird

Möchten Sie nicht mehr benötigen Standort ausgeführt wird, als live-Web-app angezeigt wird? Sie können eine vollständige Version der app beim Azure-Konto bereitstellen, indem Sie einfach auf die Schaltfläche mit den folgenden.

[![](http://azuredeploy.net/deploybutton.png)](https://azuredeploy.net/?WT.mc_id=deploy_azure_aspnet&repository=https://github.com/tfitzmac/WebPagesMovies)

Sie benötigen ein Azure-Konto zum Bereitstellen dieser Lösung in Azure. Wenn Sie nicht bereits über ein Konto verfügen, müssen Sie die folgenden Optionen aus:

- [Öffnen Sie ein Azure-Konto kostenlos](https://azure.microsoft.com/en-us/pricing/free-trial/?WT.mc_id=A443DD604) – Sie erhalten ein Guthaben können Sie kostenpflichtige Azure-Dienste zu testen und sogar nachdem sie verwendet werden bis können Sie das Konto beibehalten und Verwendung frei Azure-Dienste.
- [MSDN-abonnentenvorteile aktivieren](https://azure.microsoft.com/en-us/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604) -Ihr MSDN-Abonnement erhalten Sie Gutschriften jedes Monats, die Sie für kostenpflichtige Azure-Dienste verwenden können.

## <a name="installing-everything"></a>Installieren alle Elemente

Sie können alles, was mithilfe von Microsoft Web Platform Installer installieren. Aktiviert ist, installieren Sie den Installer und ihn verwenden, um alles zu installieren.

Um Webseiten zu verwenden, müssen Sie weisen mindestens Windows XP mit SP3 oder WindowsServer 2008 oder höher.

Auf der [Web Pages-Seite](../../../index.md) der ASP.NET-Website, klicken Sie auf **installieren**.

![Die Website mit ASP.NET Web &quot;Installieren von WebMatrix&quot; Schaltfläche](getting-started/_static/image3.png)

Sie werden aufgefordert, um die Lizenzbedingungen und Datenschutzbestimmungen vor der Installation von WebMatrix zu akzeptieren.

![Akzeptieren Sie Begriff, um die Installation zu starten](getting-started/_static/image4.png)

Klicken Sie auf **ausführen** um die Installation zu starten. (Wenn Sie das Installationsprogramm speichern möchten, klicken Sie auf **speichern** und führen Sie das Installationsprogramm aus dem Ordner, in dem Sie gespeichert.)

![](getting-started/_static/image5.png)

Der Webplattform-Installer wird angezeigt, die bereit zum Installieren von WebMatrix. Klicken Sie auf **Installieren**.

![](getting-started/_static/image6.png)

Während des Installationsvorgangs schließt, er auf dem Computer installieren muss und startet den Prozess. Je nachdem, was genau sein installiert muss kann der Prozess an einer beliebigen Stelle mehrere Minuten einen Augenblick dauern. Wählen Sie **ich stimme** zu den Lizenzbedingungen zustimmen.

## <a name="hello-webmatrix"></a>Hello, WebMatrix

Wenn dies erfolgt ist, kann während des Installationsvorgangs WebMatrix automatisch starten. Wenn es, in Windows aus nicht die **starten** Menü **Microsoft WebMatrix**.

Wenn Sie WebMatrix zum ersten Mal starten, erhalten Sie die Möglichkeit, die in Microsoft Azure mit Ihrem Microsoft-Konto anmelden. Durch die Anmeldung bei, erhalten Sie über Azure 10 kostenloses Web-apps. Dieser kostenlose Web-apps bieten eine einfache Möglichkeit zum Testen Ihrer apps. Wenn Sie nicht bereits über ein Azure-Konto verfügen, aber Sie ein MSDN-Abonnement verfügen, können Sie [Vorteile Ihres MSDN-Abonnements aktivieren](https://www.windowsazure.com/en-us/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604). Andernfalls können Sie in wenigen Minuten ein kostenloses Testkonto erstellen. Weitere Informationen finden Sie unter [kostenlose Azure-Testversion](https://azure.microsoft.com/free/?WT.mc_id=A443DD604).

Sie müssen nicht jetzt anmelden, um dieses Lernprogramm fortsetzen. Wenn Sie nicht jetzt anmelden, müssen Sie immer noch die Option später anzumelden. Der letzte [Thema](publishing.md) in diesem Lernprogramm Reihe wird beschrieben, wie Ihre Website in Azure bereitgestellt, daher müssen Sie sich zum Abschließen dieses Themas anmelden.

Zu diesem Zeitpunkt entweder melden Sie sich mit Ihrem Microsoft-Konto, oder wählen **nicht jetzt** in der unteren rechten Ecke.

![Anmelden](getting-started/_static/image7.png)

Um zu beginnen, Sie erstellen Sie eine leere Website und fügen Sie eine Seite hinzu. In einem späteren Lernprogramm in dieser Gruppe müssen mit einem der integrierten Websitevorlagen wiedergegeben werden.

Klicken Sie in das Startfenster auf **neu**.

![WebMatrix-Startbildschirm](getting-started/_static/image8.png)

Vorlagen sind vordefinierte Dateien und Seiten für verschiedene Arten von Websites. Um alle Vorlagen anzuzeigen, die standardmäßig verfügbar sind, wählen Sie die Vorlagenkatalog-Option.

![Wählen Sie Katalog mit Vorlagen](getting-started/_static/image9.png)

In der **Schnellstart** wählen **leere Website** aus der **ASP.NET** Gruppe, und nennen Sie die neue Website "WebPagesMovies".

![WebMatrix-Schnellstart-Fenster mit leere Websitevorlage ausgewählt](getting-started/_static/image10.png)

Klicken Sie auf **Weiter**.

Wenn Sie bei Ihrem Microsoft-Konto angemeldet haben, erhalten Sie die Möglichkeit, die Website in Azure erstellt werden. Basierend auf den Namen Ihrer Website, den Standardnamen des **WebPagesMovies.azurewebsites.net** wird vorgeschlagen; das Ausrufezeichen geben jedoch an, dass dieser Name nicht in Windows Azure verfügbar ist. Wählen Sie aus Gründen der Einfachheit, **Skip** , erstellen jetzt die Website in Azure zu umgehen. Weiter unten in dieser Serie werden wir den Standort in Azure veröffentlichen.

![Azure-Website erstellen](getting-started/_static/image11.png)

WebMatrix erstellt und öffnet die Website:

![Neue WebPagesMovies-Website in WebMatrix öffnen](getting-started/_static/image12.png)

Im oberen Bereich besteht eine Symbolleiste für den Schnellzugriff und ein Menüband. Links unten, daraufhin die Auswahl der Arbeitsbereich, in dem Sie Sie zwischen Aufgaben wechseln (**Website**, **Dateien**, **Datenbanken**, **Berichte**). Auf der rechten Seite ist im Bereich Inhalte des für den Editor, und für Berichte. Und die über den unteren gelegentlich eine Benachrichtigungsleiste für Nachrichten angezeigt.

Erfahren Sie mehr zum WebMatrix und dessen Funktionen, wie Sie diese Lernprogramme durchlaufen.

## <a name="creating-a-web-page"></a>Erstellen einer Webseite

Um mit WebMatrix und ASP.NET Web Pages vertraut sind, erstellen Sie eine einfache Seite.

Wählen Sie in der Arbeitsbereich, den **Dateien** Arbeitsbereich. In diesem Arbeitsbereich können Sie die Arbeit mit Dateien und Ordnern. Im linken Bereich werden die Dateistruktur Ihrer Website. Die Menüband-Änderungen dateibezogene Aufgaben anzeigen.

![Arbeitsbereich "Datei" in WebMatrix](getting-started/_static/image13.png)

Klicken Sie im Menüband auf den Pfeil unter **neu** , und klicken Sie dann auf **neue Datei**.

![Mithilfe der &quot;neu&quot; Befehl im Menüband eine neue Datei erstellen](getting-started/_static/image14.png)

WebMatrix zeigt eine Liste der Dateitypen. Wählen Sie **CSHTML**, und klicken Sie in der **Namen** geben "HelloWorld". Eine CSHTML-Seite ist eine ASP.NET Web Pages-Seite.

![Erstellen einer neuen CSHTML-Seite mit dem Namen HelloWorld.cshtml](getting-started/_static/image15.png)

Klicken Sie auf **OK**.

WebMatrix wird die Seite erstellt und öffnet sie im Editor.

![Die neue HelloWorld-Seite in der WebMatrix-editor](getting-started/_static/image16.png)

Wie Sie sehen können, enthält die Seite größtenteils gewöhnliche HTML-Markup, mit Ausnahme von einem Block oben, die wie folgt aussieht:

[!code-cshtml[Main](getting-started/samples/sample1.cshtml)]

Die zum Hinzufügen von Code, wie Sie in Kürze sehen.

Beachten Sie, dass die verschiedenen Teile der Seite &mdash; die Elementnamen, Attribute und Text sowie den Block oben – sind jeweils in unterschiedlichen Farben. Hierbei spricht *syntaxhervorhebung*, und Dies erleichtert, Alles klar zu halten. Es ist eine der Funktionen, die zur Bearbeitung von Webseiten in WebMatrix erleichtert.

Hinzufügen von Inhalten für die `<head>` und `<body>` Elemente wie im folgenden Beispiel. (Wenn Sie möchten, klicken Sie einfach den folgenden Block kopieren und ersetzen die gesamte vorhandene Seite mit dem folgenden Code.)

[!code-cshtml[Main](getting-started/samples/sample2.cshtml)]

In der Symbolleiste für den Schnellzugriff oder in der **Datei** Menü klicken Sie auf **speichern**.

![Speichern Sie Schaltfläche "" in der Symbolleiste für den WebMatrix Schnellzugriff](getting-started/_static/image17.png)

## <a name="testing-the-page"></a>Testen die Seite "

In der **Dateien** Arbeitsbereich mit der rechten Maustaste die *HelloWorld.cshtml* Seite, und klicken Sie dann auf **in Browser starten**.

![Ausführen einer Seite über die Schaltfläche "ausführen" im Menüband WebMatrix](getting-started/_static/image18.png)

WebMatrix startet einen integrierten Webserver (IIS Express), den Sie zum Testen von Seiten auf Ihrem Computer verwenden können. (Ohne IIS Express in WebMatrix müssten Sie Seite "Veröffentlichen" die an einen Webserver an einer beliebigen Stelle bevor Sie es testen können.) Die Seite ist in Ihrem Standardbrowser angezeigt.

![&quot;Hello World&quot; Seite im Browser ausgeführt wird](getting-started/_static/image19.png)

Beachten Sie, dass beim Testen einer Seite in WebMatrix die URL im Browser etwa `http://localhost:33651/HelloWorld.cshtml.` den Namen *"localhost"* bezieht sich auf einem lokalen Server, was bedeutet, dass die Seite von einem Webserver bereitgestellt werden, die auf Ihrem eigenen Computer befindet. Wie bereits erwähnt, enthält WebMatrix eine Web-Server-Programm mit dem Namen IIS Express, die ausgeführt wird, wenn Sie eine Seite zu starten.

Die Zahl nach *"localhost"* (z. B. *Localhost:33651*) bezieht sich auf eine *Portnummer* auf Ihrem Computer. Dies ist die Anzahl der "Channel", die für diese bestimmte Website IIS Express verwendet werden. Die Portnummer wird nach dem Zufallsprinzip aus dem Bereich von 1024 bis 65536 ausgewählt, wenn Sie Ihre Website erstellen und dieser Vorgang unterscheidet sich für jede Website, die Sie erstellen. (Beim Testen Ihrer eigenen Site wird die Nummer des Ports Befehlsbeispiel eine andere Zahl als 33561 sein.) Verwenden Sie einen anderen Port für jede Website, können IIS Express gerade dem Ihren Websites beibehalten, es kommuniziert.

Später, wenn Sie Ihre Website mit einem öffentlichen Webserver veröffentlichen daraufhin nicht mehr *"localhost"* in der URL. Nun sehen Sie eine typische URL wie `http://myhostingsite/mywebsite/HelloWorld.cshtml` oder die Seite ist. Sie erfahren mehr zum Veröffentlichen einer Websites weiter unten in diesem Lernprogramm.

## <a name="adding-some-server-side-code"></a>Hinzufügen von serverseitigen Code

Schließen Sie den Browser, und wechseln Sie zurück zur Seite in WebMatrix.

Fügen Sie eine Zeile an den Codeblock, damit sie den folgenden Code ähnelt:

[!code-cshtml[Main](getting-started/samples/sample3.cshtml)]

Dies ist ein wenig von Razor-Code. Es ist wahrscheinlich klar, dass er das aktuelle Datum und die Uhrzeit ruft und den Wert in einer *Variable* mit dem Namen `currentDateTime`. Fügen Sie weitere Informationen zu Razor-Syntax in den nächsten Lernprogrammen.

Im Hauptteil der Seite nach der `<p>Hello World!</p>` Element, fügen Sie Folgendes hinzu:

[!code-html[Main](getting-started/samples/sample4.html)]

Dieser Code Ruft den Wert ab, die Sie speichern die `currentDateTime` Variablen am Anfang und fügt es in das Markup der Seite. Die `@` Zeichen kennzeichnet die ASP.NET Web Pages-Code auf der Seite.

Führen Sie die Seite erneut (WebMatrix speichert die Änderungen für Sie vor der Ausführung der Seite "). Diesmal sehen Sie Datum und Uhrzeit auf der Seite.

![&quot;Hello World&quot; Seite im Browser mit einem dynamisch generierten Uhrzeitanzeige ausgeführt](getting-started/_static/image20.png)

Warten Sie einige Minuten, und klicken Sie dann aktualisieren Sie die Seite im Browser zu. Die Anzeige von Datum und die Uhrzeit wird aktualisiert.

Betrachten Sie die Seitenquelle im Browser aus. Es sieht wie das folgende Markup:

[!code-html[Main](getting-started/samples/sample5.html)]

Beachten Sie, dass die `@{ }` Block oben ist nicht vorhanden. Beachten Sie außerdem, dass das Datum und Uhrzeit eine tatsächliche Zeichenfolge von Zeichen zeigt (`1/18/2012 2:49:50 PM` oder einer beliebigen anderen), nicht `@currentDateTime` wie Sie in mussten der *cshtml* Seite. Was ist geschehen ist hier, dass Sie auf der Seite "ausgeführt haben, mit ASP.NET den Code (sehr wenig in diesem Fall) verarbeitet wird, die mit markiert wurde `@`. Der Code erzeugt die Ausgabe, und diese Ausgabe in die Seite eingefügt wurde.

## <a name="this-is-what-aspnet-web-pages-are-about"></a>Dies ist, was zu ASP.NET Web Pages sind

Wenn Sie lesen, dass ASP.NET Web Pages dynamischer Webinhalte erzeugt, ist hier gesehen das Konzept. Die soeben erstellten Seite enthält das gleiche HTML-Markup, dem Sie vor dem gesehen haben. Sie können auch Code enthalten, die alle Arten von Aufgaben ausführen können. In diesem Beispiel haben sie die einfache Aufgabe abrufen, das aktuelle Datum und die Uhrzeit. Wie Sie gesehen haben, können Sie Code mit dem HTML-Ausgabe auf der Seite einzufügen. Wenn ein Benutzer fordert eine *cshtml* Seite im Browser, ASP.NET verarbeitet die Bild-auf, während sie weiterhin in die Hände des Webservers befindet. ASP.NET fügt die Ausgabe des Codes (sofern vorhanden) in der Seite als HTML. Wenn der Code-Verarbeitung abgeschlossen ist, sendet ASP.NET Ergebnisseite an den Browser. Alles, was die unmittelbaren Kontrolle liegt der Browser ist HTML. So sieht ein Diagramm aus:

![Konzeptioneller Ablauf der wie ASP.NET HTML dynamisch generiert](getting-started/_static/image21.png)

Die Idee ist einfach, aber es gibt viele interessante Aufgaben, die der Code ausführen können, und es gibt viele interessante Möglichkeiten, in denen Sie HTML-Inhalt dynamisch auf der Seite hinzufügen können. Und ASP.NET *cshtml* Seiten wie jeder HTML-Seite zählen auch Code, der im Browser selbst (JavaScript und jQuery-Code) ausgeführt wird. Sie müssen alle folgenden Schritte in diesem Lernprogramm Satz und alle weiteren durchsuchen.

## <a name="coming-up-next"></a>Als Nächstes kommen

In der nächsten Lernprogramm dieser Reihe untersuchen Sie ASP.NET Web Pages-Programmierung ein wenig mehr.

## <a name="additional-resources"></a>Zusätzliche Ressourcen

[Erstellen eine ASP.NET-Website von Grund auf Neu](https://www.microsoft.com/web/post/create-an-aspnet-website-from-scratch). Dies ist ein Lernprogramm, das speziell zur Verwendung von WebMatrix (nicht ASP.NET Web Pages). Er wechselt in etwas genauer zu einigen der zusätzlichen Features von WebMatrix, die wir in diesem Lernprogramm Satz verdeckt wird.

>[!div class="step-by-step"]
[Nächste](intro-to-web-pages-programming.md)
