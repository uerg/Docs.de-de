---
uid: web-pages/overview/getting-started/introducing-aspnet-web-pages-2/publishing
title: Einführung in ASP.NET Web Pages - Veröffentlichen einer Website mit WebMatrix | Microsoft Docs
author: tfitzmac
description: Dieses Lernprogramm ist der letzte Teil in der Tutorial Menge, die ASP.NET Web Pages und Microsoft WebMatrix eingeführt werden. Es wird erläutert, wie Ihre Website t veröffentlichen...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/28/2015
ms.topic: article
ms.assetid: 7e85c70e-1a88-4408-8b3d-29611c7713ed
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/getting-started/introducing-aspnet-web-pages-2/publishing
msc.type: authoredcontent
ms.openlocfilehash: 7b9bffac5cc72e1bea3f1b211cc03be2ccb8e499
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
---
<a name="introducing-aspnet-web-pages---publishing-a-site-by-using-webmatrix"></a>Einführung in ASP.NET Web Pages - Veröffentlichen einer Website mit WebMatrix
====================
durch [Tom FitzMacken](https://github.com/tfitzmac)

> Dieses Lernprogramm ist der letzte Teil in der Tutorial Menge, die ASP.NET Web Pages und Microsoft WebMatrix eingeführt werden. Es wird erläutert, wie Ihre Website im Internet veröffentlichen, damit andere Benutzer damit arbeiten können. Es wird vorausgesetzt, Sie haben die Reihe über abgeschlossen [für ASP.NET Web Pages-Websites erstellen ein konsistentes Aussehen](https://go.microsoft.com/fwlink/?LinkId=251585).
> 
> Sie erfahren, wie zum Veröffentlichen Ihrer Website verwendet:
> 
> - Microsoft Azure
> - Web-Hostinganbieter


## <a name="about-publishing-your-site"></a>Zum Veröffentlichen Ihrer Website

Bis jetzt haben Sie Ihre Arbeit auf einem lokalen Computer, z. B. Ihre Seiten testen. Zum Ausführen Ihrer<em>cshtml</em> Seiten die, Sie verwendet haben, den Webserver, die in WebMatrix, nämlich IIS Express integriert ist. Aber natürlich kein können finden Sie unter der Website, die Sie außer dass Sie erstellt haben. Damit um andere Benutzer mit Ihrer Website arbeiten zu können, müssen Sie ihn mit dem Internet zu veröffentlichen.

Sofern nicht bereits Zugriff auf einen öffentlichen Webserver veröffentlichen bedeutet, dass Sie ein Konto mit einem *Cloudplattform* oder ein *Hostinganbieter*. Eine Cloud-Plattform, z. B. Microsoft Azure bietet bei Bedarf-Infrastruktur für Ihre Anwendungen. Hosting-Anbieter ist ein Unternehmen, die öffentlich zugängliche Webserver besitzt und wird, die Sie vermieten, Speicherplatz, der für Ihre Website. Hosten von Plänen von wenige Dollar pro Monat ausführen (oder sogar kostenlos) für kleine Standorte zu vielen Hunderten Dollar pro Monat für hohes Volumen kommerziellen Websites.

> [!NOTE]
> Möglicherweise haben Sie Zugriff auf einen öffentlichen Webserver über die Internetdienstanbieter (ISP), mit denen Sie zu Hause Internetdienst abrufen. Allerdings muss Ihrem Hostinganbieter ASP.NET Web Pages unterstützen. Viele Internetdienstanbieter nicht, aber es lohnt sich immer überprüfen.


In diesem Lernprogramm fügen wir einen Überblick über die Vorgehensweise beim Veröffentlichen und erteilen. Es ist nicht praktikabel ist, geben Sie die genauen Details für alles, da der Vorgang für jede Hostinganbieter etwas unterscheidet sich. Aber Sie erhalten eine Vorstellung der Funktionsweise des Prozesses.

Dieses Lernprogramm enthält vier Teilbereiche:

1. [Die Standardseite einrichten](#defaultpage)
2. Veröffentlichen (Wählen Sie eine der folgenden)  
 a. [Veröffentlichen Sie Ihre Website in Microsoft Azure](#azure)  
 b. [Veröffentlichen Ihrer Website zu einem Webhostingdienst Unternehmen](#host)
3. [Aktualisieren die Live-Standort: Erneutes Veröffentlichen](#update)

<a id="defaultpage"></a>
## <a name="setting-up-the-default-page"></a>Die Standardseite einrichten

Wenn ein Benutzer auf die Basisadresse für Ihre Website navigiert, wird die Standardseite für Ihre Website für den Benutzer angezeigt. Beispielsweise, wenn als Standardseite für den Standort am www.contoso.com "default.htm" festgelegt ist, und dann weiter navigieren <strong>www.contoso.com</strong> ist identisch mit Navigieren zu <strong>www.contoso.com/Default.htm</strong>.

Die Website derzeit verwendet **Default.cshtml** als die Standardseite. Diese Seite ist gut für die Standardseite, aber in diesem Lernprogramm nicht hinzugefügt haben alle Inhalte, die diese Seite, damit sie eine leere Seite angezeigt wird. Öffnen Sie Default.cshtml, und Ersetzen Sie den Inhalt durch folgenden Code.

[!code-cshtml[Main](publishing/samples/sample1.cshtml)]

Nun ist Ihr Standort für die Veröffentlichung bereit. Zuerst sehen Sie wie die Website in Azure bereitstellen und es zu einem Webhostingdienst Unternehmen bereitstellen. Beide funktionieren Option für Ihre Website, und Sie müssen nur eine der Optionen für die Bereitstellung ausführen.

<a id="azure"></a>
## <a name="publishing-your-site-to-microsoft-azure"></a>Veröffentlichen Sie Ihre Website in Microsoft Azure

Dieses Lernprogramm zeigt Sie zunächst wie Ihrer Website in Microsoft Azure bereitgestellt wird. Durch Anmelden mit einem Microsoft-Konto, können Sie bis zu 10 kostenlosen Websites in Azure erstellen. Diese kostenlosen Websites bieten eine einfache Möglichkeit, Ihre Standorte zu testen. Sie können diesen Standort Beispiel weiter unten, um zu vermeiden, indem Sie alle Ihre kostenlosen Websites verwenden immer löschen. Sie können in wenigen Minuten ein kostenloses Testkonto erstellen. Weitere Informationen finden Sie unter [kostenlose Azure-Testversion](https://azure.microsoft.com/free/?WT.mc_id=A443DD604).

Klicken Sie in der WebMatrix-Menüband auf die **veröffentlichen** Schaltfläche.

![WebMatrix-Menüband die Schaltfläche "Veröffentlichen"](publishing/_static/image1.png)

Die **Veröffentlichen Ihrer Website** Dialogfeld wird angezeigt. Wenn Sie nicht mit Ihrem Microsoft-Konto angemeldet haben, wird das Dialogfeld enthält einen **erste Schritte mit Azure** Link. Klicken Sie auf diesen Link.

![Veröffentlichen Sie Ihrer Website](publishing/_static/image2.png)

Wenn Sie nicht mit einem Microsoft-Konto angemeldet haben, erhalten Sie die Möglichkeit, melden Sie sich erneut. Ihre Website in Azure veröffentlichen, müssen Sie sich mit einem Microsoft-Konto anmelden.

![Anmelden](publishing/_static/image3.png)

Nach dem Anmelden bei Ihrem Microsoft-Konto, enthält das Dialogfeld Links, um einen neuen Standort in Azure erstellen oder Herstellen einer Verbindung mit einer vorhandenen Websites in Azure.

![Neue Website erstellen](publishing/_static/image4.png)

Wählen Sie **Erstellen eines neuen Standorts**.

Wenn Sie das Projekt mit dem Namen **WebPagesMovies**, werden der Standardname für Ihre Website **webpagesmovies.azurewebsites.net**. Dieser Standardname ist sehr wahrscheinlich nicht verfügbar ist, wie durch rotes Ausrufezeichen angezeigt.

![Name der Standard-Website](publishing/_static/image5.png)

Ändern Sie den Namen der Website auf einen anderen Wert, der verfügbar ist, und wählen Sie einen Speicherort, der in der Nähe Ihres Speicherorts ist.

![geänderte Standortname](publishing/_static/image6.png)

Klicken Sie auf **OK**.

WebMatrix Performss einen Test, um zu bestimmen, ob der Server mit Ihrer Website kompatibel ist.

![Testen der Kompatibilität](publishing/_static/image7.png)

Wählen Sie **weiterhin**.

Die Ergebnisse von den Kompatibilitätstest werden angezeigt.

![Kompatibilität Ergebnis](publishing/_static/image8.png)

Wählen Sie **weiterhin**.

WebMatrix zeigt die Dateien und Datenbanken, die auf der Website veröffentlicht werden. Da dies beim ersten Sie den Standort veröffentlichen ist, werden alle Dateien aufgeführt. Deaktivieren Sie eine Datei, die nicht zur Veröffentlichung bereit ist. In den nachfolgenden Publikationen werden nur die geänderten Dateien angezeigt. Finden Sie unter [Live-Standort zu aktualisieren: Erneutes Veröffentlichen von](#update).

![veröffentlichungsvorschau](publishing/_static/image9.png)

Wählen Sie **weiterhin**.

Nachdem die Website in Azure bereitgestellt wurde, wird eine Meldung angezeigt, der angibt, dass die Bereitstellung abgeschlossen wurde.

![vollständige veröffentlichen](publishing/_static/image10.png)

Des Standorts und der Datenbank auf Azure veröffentlicht wurden, und sind nun für die Öffentlichkeit verfügbar. Klicken Sie auf den Link in der Meldung, die Veröffentlichung abgeschlossen ist, und sehen Sie nun Ihre Website bereitgestellt. Sie oder jede Person mit Zugriff auf das Internet hinzufügen oder Ändern von Datensätzen in der Datenbank.

![](publishing/_static/image11.png)

<a id="host"></a>
## <a name="publishing-your-site-to-a-web-hosting-company"></a>Veröffentlichen Ihrer Website zu einem Webhostingdienst Unternehmen

Wenn Sie nicht in Azure veröffentlichen möchten, können Sie stattdessen Ihre Website auf einen Webhostinganbieter veröffentlichen.

Klicken Sie auf die **Webhosting suchen** Link.

![Schaltfläche "Webhosting suchen", im Dialogfeld Veröffentlichungseinstellungen](publishing/_static/image12.png)

Sie wechseln auf eine Seite auf der Microsoft-Website, unter dem Hostinganbieter aufgeführt, die ASP.NET unterstützen.

![Seite für Microsoft-Website, die Hostinganbieter Listet](publishing/_static/image13.png)

Natürlich kann es schwierig sein, wissen jetzt genau welche hosting Funktionen langfristig erforderlich sein können. Hier sind einige Dinge zu bedenken:

- Für die Zwecke des Standorts WebPagesMovies müssen Sie eine separate-Add-On für SQL Server verfügen, die häufig zusätzliche Kosten. An Ihrem Standort verwenden Sie SQL Server Compact Edition, der eigenständig ist. Allerdings benötigen Sie möglicherweise SQL Server-Zugriff für zukünftige Website Arbeit, die Sie erledigen. Wenn Sie, dass Sie möglicherweise glauben, stellen Sie sicher, dass Sie SQL Server-Funktion später hinzufügen können.
- Überprüfen Sie, ob der Hostinganbieter die publishing Web Deploy-Protokoll unterstützt. Können Sie mithilfe von FTP-Protokolls veröffentlichen, aber es ist einfacher, Web Deploy verwendet werden.

Einige Websites bieten einen kostenlose Testzeitraum. Eine kostenlose Testversion ist eine gute Möglichkeit, die Publishing versuchen Sie es und hostet, während Sie weiterhin mit WebMatrix und ASP.NET Web Pages experimentieren sind.

Wählen Sie eine, die Ihnen gefällt. Für dieses Lernprogramm ausgewählt wir DiscountASP.NET, da während wir das Lernprogramm erstellt haben, dieses Unternehmens mit eine Höherstufung enthielt, mit denen Sie Personen, die einen Standort für einige Monate kostenlos hosten können.

> [!NOTE]
> Unsere Wahl eines Hostinganbieters für dieses Lernprogramm sollte nicht als Billigung von der jeweiligen Unternehmen über andere interpretiert werden. Aber wir hatten, um eine zu Illustrationszwecken auszuwählen, und DiscountASP.NET ist eines der vielen Unternehmen, die ASP.NET Web Pages und Web Deploy-Protokoll für die Veröffentlichung unterstützt.


In der Regel, nachdem Sie sich mit dem hosting-Anbieter registriert haben, sendet das Unternehmen Ihnen eine e-Mail, die einen Benutzernamen und Kennwort, die URL des Webservers usw. enthält. Hostinganbieter Web Deploy-Protokoll unterstützt, können sie senden Sie eine Datei mit veröffentlichungseinstellungen oder können Sie eine herunterladen. Eine Datei mit veröffentlichungseinstellungen erleichtert den Prozess für Sie.

Wenn Sie sich registriert haben und für die Veröffentlichung bereit sind, klicken Sie auf die **veröffentlichen** Schaltfläche im Menüband WebMatrix. Die **Veröffentlichungseinstellungen** Dialogfeld wird angezeigt.

Wenn der hosting-Anbieter eine Datei mit veröffentlichungseinstellungen gesendet, klicken Sie auf die **veröffentlichungseinstellungen importieren** verknüpfen und importieren Sie die Datei. Wenn Sie nicht über eine Datei mit veröffentlichungseinstellungen verfügen, füllen Sie in den Feldern unter Verwendung der Werte, die per e-Mail Hostinganbieter gesendete. Hier wird die **Veröffentlichungseinstellungen** das Dialogfeld sieht z. B. Wenn Sie fertig sind:

![Veröffentlichungseinstellungen ausgefüllt, die im Dialogfeld "Veröffentlichungseinstellungen"](publishing/_static/image14.png)

Klicken Sie auf **überprüft, ob Verbindung**. Wenn alles in Ordnung ist, wird das Dialogfeld meldet **erfolgreich verbunden**, was bedeutet, dass mit der hosting-Anbieter-Server kommunizieren kann.

![Nachricht erfolgreich, wenn veröffentlichen Einstellungen richtig sind.](publishing/_static/image15.png)

Wenn ein Problem vorliegt, führt WebMatrix empfiehlt es sich, Ihnen mitteilen, was das Problem ist:

![Fehlermeldung, wenn es liegt ein Problem mit den veröffentlichungseinstellungen](publishing/_static/image16.png)

Klicken Sie auf **speichern** zum Speichern der Einstellungen. WebMatrix bietet zum Ausführen eines Tests aus, um sicherzustellen, dass er ordnungsgemäß mit der hosting-Website kommunizieren kann:

![Meldung, die zum Ausführen eines Tests des Veröffentlichungsvorgangs Angebot](publishing/_static/image17.png)

Klicken Sie auf **Ja**. WebMatrix lädt einige Beispieldateien mit dem Hostinganbieter hoch. Wenn der Kompatibilitätstest abgeschlossen ist, werden die Ergebnisse von WebMatrix meldet:

![Die publishing Testergebnisse](publishing/_static/image18.png)

Wenn Sie bereit sind, fahren Sie fort, und klicken Sie auf **Fortfahren** tatsächlichen den Veröffentlichungsprozess zu starten. WebMatrix Abbildungen, welche Dateien sind an Ihrem Standort und befinden sich bereits auf dem Hostserver (zurzeit keine) und bietet Ihnen eine Vorschau des Veröffentlichungsprozesses:

![Vorschau, welche Dateien, die der Veröffentlichungsprozess hochladen](publishing/_static/image19.png)

Die Liste der Dateien veröffentlichen enthält Webseiten, die Sie z. B. die erstellte *Movies.cshtml*. Die Liste enthält auch die Dateien für Hilfsprogramme, die Sie installiert haben, die Dateien in SQL Server Compact Edition für Ihre Datenbank ausführen und so weiter. Daher der ersten Vorgang veröffentlichen kann erhebliche sein.

Klicken Sie auf **Weiter**. WebMatrix kopiert die Dateien auf dem Hostinganbieter Server. Wenn dies erfolgt ist, werden die Ergebnisse in der Statusleiste gemeldet:

![Statusmeldung Leiste an, wenn der Veröffentlichungsprozess erfolgreich abgeschlossen wurde](publishing/_static/image20.png)

Um die live-Standort anzuzeigen, klicken Sie auf den Link in der Statusleiste. Hinzufügen *Filme* an die URL, und sehen Sie die *Movies.cshtml* -Datei, die Sie erstellt haben:

![Die live-Standort mit der Seite "Filme"](publishing/_static/image21.png)

<a id="update"></a>
## <a name="updating-the-live-site-republishing"></a>Aktualisieren die Live-Standort: Erneutes Veröffentlichen

Nachdem Sie Ihre Website (in Azure oder einem Webhostinganbieter) veröffentlicht haben, befinden sich zwei Kopien &mdash; die Version auf Ihrem Computer und die Version auf den Dienstanbieter. Sie möchten wahrscheinlich die Entwicklung der Website fortsetzen (Wenn nichts anderes als Teil des nächsten Lernprogramms). Wenn Sie dies tun, müssen Sie Ihre Website erneut veröffentlichen, um Änderungen an den Dienstanbieter von Ihrem Computer zu kopieren. Der Veröffentlichungsprozess in WebMatrix kann bestimmen, welche Dateien geändert wurden, auf der Website und veröffentlichen nur jene Dateien.

Um zu sehen, wie funktioniert das erneute veröffentlichen, öffnen Sie die *Movies.cshtml* -Website stellen einige kleine Änderung, und speichern Sie die Datei. Ändern Sie den Titel, z. B. `Movies - Updated`.

Klicken Sie auf die **veröffentlichen** Schaltfläche im Menüband. WebMatrix wird bestimmt, was geändert wird, und zeigt eine Vorschau der Dateien, die sie veröffentlicht werden sollen.

![Im Dialogfeld "Veröffentlichen" mit der geänderten bereit Dateien für das erneute veröffentlichen](publishing/_static/image22.png)

> [!IMPORTANT] 
> 
> Standardmäßig veröffentlicht WebMatrix Datenbankansicht (*.sdf* Datei) nur beim ersten Verwenden Sie die Website veröffentlichen. Nach Ihrer Website veröffentlicht wird, und Benutzer mit der Website interagieren, gelten für die Datenbank am Standort live in der Regel echte Daten von der Website. Müssen Sie sehr darauf achten, nicht überschreiben die live-Datenbank mit der *.sdf* Datei, die auf Ihrem Computer wird in der Regel nur die Testdaten enthält. Daher Sie sehen, dass die Warnung **Veröffentlichung überschreibt alle Remotedatenbanken**, und warum das Kontrollkästchen für *WebPagesMovies.sdf* ist standardmäßig deaktiviert.


Klicken Sie auf **Weiter**. WebMatrix veröffentlicht die geänderten Dateien und Sie eine Erfolgsmeldung zeigt, wie es erstmalig, die Sie veröffentlicht haben.

Wechseln Sie zu der live-Standort (Sie können den Link in der bestätigungsmeldung klicken, wenn sie weiterhin angezeigt wird) und stellen Sie sicher, dass die Änderung veröffentlicht wurde.

> [!TIP] 
> 
> **Bearbeiten von Remote-Dateien**
> 
> Als Alternative zum Ändern Ihrer Website und anschließend erneutes veröffentlichen können Sie die remote-Dateien direkt in WebMatrix bearbeiten. In diesem Szenario Öffnen einer Datei, die auf den Dienstanbieter ist, und WebMatrix lädt eine Kopie des Zertifikats für die Sie bearbeiten. Jedes Mal, wenn Sie die Datei speichern, werden die Änderungen von WebMatrix an den Standort gesendet.
> 
> Bearbeiten von Remote ist eine einfache Möglichkeit, um die live-Standort zu ändern. Allerdings werden nicht die vorgenommenen Änderungen auf diese Weise mit den Dateien an Ihrem lokalen Standort synchronisiert. Um den lokalen Dateien mit dem Remotestandort zu synchronisieren, können Sie die Remotedateien herunterladen. Dieser Prozess funktioniert ähnlich wie die Publishing, außer in umgekehrter Reihenfolge.
> 
> Es wird nicht mehr über die Remote-Bearbeitung und Remote-Download-Funktionen von WebMatrix hier beschreiben. Sie sind sehr nützlich, wenn mehrere Personen auf derselben Website auf verschiedenen Computern arbeiten. Weitere Informationen finden Sie unter [veröffentlichen und Bearbeiten von einem Remotestandort mit WebMatrix 2 Beta](https://go.microsoft.com/fwlink/?LinkId=251591).


## <a name="additional-resources"></a>Zusätzliche Ressourcen

- [ASP.NET WebMatrix ASP.NET Web Pages-Forum](https://forums.asp.net/1224.aspx/1?WebMatrix+and+ASP+NET+Web+Pages), ein hervorragender Ausgangspunkt veröffentlichen Fragen und Antworten.

> [!div class="step-by-step"]
> [Vorherige](layouts.md)
