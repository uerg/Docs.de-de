---
uid: web-pages/overview/getting-started/introducing-aspnet-web-pages-2/publishing
title: 'Einführung in ASP.NET Web Pages: Veröffentlichen einer Website mit WebMatrix | Microsoft-Dokumentation'
author: tfitzmac
description: Dieses Tutorial ist die letzte Folge der tutorialreihe, die ASP.NET Web Pages und Microsoft WebMatrix eingeführt werden. Es wird erläutert, wie zum Veröffentlichen Ihrer Website t...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/28/2015
ms.topic: article
ms.assetid: 7e85c70e-1a88-4408-8b3d-29611c7713ed
ms.technology: dotnet-webpages
msc.legacyurl: /web-pages/overview/getting-started/introducing-aspnet-web-pages-2/publishing
msc.type: authoredcontent
ms.openlocfilehash: c6fa7692527b7aa65e93cd57ed5bd56f42e54bd6
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/03/2018
ms.locfileid: "37373486"
---
<a name="introducing-aspnet-web-pages---publishing-a-site-by-using-webmatrix"></a>Einführung in ASP.NET Web Pages - Veröffentlichen einer Website mit WebMatrix
====================
durch [Tom FitzMacken](https://github.com/tfitzmac)

> Dieses Tutorial ist die letzte Folge der tutorialreihe, die ASP.NET Web Pages und Microsoft WebMatrix eingeführt werden. Es wird erläutert, wie Ihre Website mit dem Internet zu veröffentlichen, damit andere Benutzer damit arbeiten können. Es wird vorausgesetzt, Sie haben die Reihe über [für ASP.NET Web Pages-Websites Erstellen einer konsistenten aussehen](https://go.microsoft.com/fwlink/?LinkId=251585).
> 
> Sie erfahren, wie zum Veröffentlichen Ihrer Website verwenden können:
> 
> - Microsoft Azure
> - Web-Hosting-Unternehmen


## <a name="about-publishing-your-site"></a>Zum Veröffentlichen Ihrer Website

Bisher haben Sie Ihre Arbeit auf einem lokalen Computer, z. B. Ihre Seiten testen fertig. Zum Ausführen Ihrer<em>.cshtml</em> Seiten Sie verwendet haben, den Webserver, die in WebMatrix, d. h. IIS Express integriert ist. Aber natürlich niemand können finden Sie auf der Website, die Sie außer dass Sie erstellt haben. Damit um andere Benutzer die Arbeit mit Ihrer Website zu können, müssen Sie sie mit dem Internet zu veröffentlichen.

Wenn Sie bereits Zugriff auf einen öffentlichen Webserver verfügen, Veröffentlichung bedeutet, dass Sie ein Konto mit einem *Cloudplattform* oder *Hostinganbieter*. Eine Cloud-Plattform, z. B. Microsoft Azure stellt bei Bedarf Infrastruktur bereit, für Ihre Anwendungen. Ein hosting-Anbieter ist ein Unternehmen, die öffentlich zugänglichen Webserver besitzt und wird, die zu mieten Sie, Speicherplatz für Ihre Website. Hosten von Plänen aus wenige Euro im Monat ausgeführt (oder sogar kostenlos) für kleine Standorte mit vielen Hunderten von Euro im Monat für umfangreiche kommerzielle Websites.

> [!NOTE]
> Möglicherweise haben Sie Zugriff auf einen öffentlichen Webserver über die Internetdienstanbieter (ISP), mit denen Sie zu Hause Internetdienst zu erhalten. Allerdings muss der Hostinganbieter ASP.NET Web Pages unterstützt. Viele ISPs nicht, aber es lohnt sich immer überprüfen.


In diesem Tutorial haben erhalten wir einen Überblick über die Informationen zum Veröffentlichen Sie. Es ist nicht praktikabel ist, geben Sie die genauen Details für alle Elemente, da ein wenig unterscheidet sich der Vorgang für alle Hostinganbieter. Aber Sie erhalten einen guten Überblick darüber, wie der Prozess ausgeführt.

Dieses Lernprogramm enthält vier Abschnitte:

1. [Die Standardseite einrichten](#defaultpage)
2. Veröffentlichen (Wählen Sie eine der folgenden)  
 a. [Veröffentlichen Sie Ihre Website in Microsoft Azure](#azure)  
 b. [Veröffentlichen Sie Ihre Website in ein Webhostingunternehmen](#host)
3. [Aktualisieren der Live-Websites: Erneutes Veröffentlichen](#update)

<a id="defaultpage"></a>
## <a name="setting-up-the-default-page"></a>Die Standardseite einrichten

Wenn ein Benutzer auf die Basisadresse für Ihre Website navigiert, wird die Standardseite für die Website, die dem Benutzer angezeigt. Z. B. wenn als Standardseite für die Website unter www.contoso.com "default.htm" festgelegt ist, und dann weiter navigieren <strong>www.contoso.com</strong> ist identisch mit der Navigation zu <strong>www.contoso.com/Default.htm</strong>.

Ihre Website derzeit verwendet **Default.cshtml** wie die Standardseite. Diese Seite ist für die voreingestellte Seite in Ordnung, aber in diesem Tutorial nicht hinzugefügt haben alle Inhalte der Seite, damit sie eine leere Seite angezeigt wird. Öffnen Sie Default.cshtml, und Ersetzen Sie den Inhalt durch den folgenden Code.

[!code-cshtml[Main](publishing/samples/sample1.cshtml)]

Nachdem Sie Ihre Website für die Veröffentlichung bereit ist. Zunächst sehen Sie wie die Website in Azure bereitstellen, und klicken Sie dann in einem Webhostinganbieter bereitgestellt. Beide funktionieren Option für Ihre Website, und Sie müssen nur eine der Optionen für die Bereitstellung ausführen.

<a id="azure"></a>
## <a name="publishing-your-site-to-microsoft-azure"></a>Veröffentlichen Sie Ihre Website in Microsoft Azure

In diesem Tutorial wird zuerst Ihre Website in Microsoft Azure bereitstellen, veranschaulichen. Anmeldung mit einem Microsoft-Konto, können Sie bis zu 10 kostenlose Websites in Azure erstellen. Diese kostenlosen Websites bieten eine praktische Möglichkeit zum Testen von Ihrer Websites. Sie können diese Beispielwebsite, später, um zu vermeiden, verwenden all Ihre kostenlosen Websites jederzeit löschen. Sie können in nur wenigen Minuten ein kostenloses Testkonto erstellen. Weitere Informationen finden Sie unter [kostenlose Azure-Testversion](https://azure.microsoft.com/free/?WT.mc_id=A443DD604).

Klicken Sie in WebMatrix im Menüband auf die **veröffentlichen** Schaltfläche.

!['Publish' Schaltfläche im Menüband von WebMatrix](publishing/_static/image1.png)

Die **Veröffentlichen Ihrer Website** Dialogfeld wird angezeigt. Wenn Sie nicht mit Ihrem Microsoft-Konto angemeldet haben, kann das Dialogfeld enthält einen **erste Schritte mit Azure** Link. Klicken Sie auf diesen Link.

![Ihre Website veröffentlichen](publishing/_static/image2.png)

Wenn Sie sich nicht um ein Microsoft-Konto angemeldet haben, erhalten Sie die Möglichkeit, melden Sie sich erneut. Sie müssen ein Microsoft-Konto melden Sie sich auf Ihre Website in Azure zu veröffentlichen.

![Anmelden](publishing/_static/image3.png)

Nach der Anmeldung bei Ihrem Azure-Konto, enthält das Dialogfeld Links zum Erstellen einer neuen Website in Azure oder eine Verbindung mit einer Ihrer vorhandenen Websites in Azure.

![Neue Website erstellen](publishing/_static/image4.png)

Wählen Sie **Erstellen eines neuen Standorts**.

Wenn Sie Ihr Projekt mit dem Namen **WebPagesMovies**, der Standardnamen für die Website werden **webpagesmovies.azurewebsites.net**. Dieser Standardname ist wahrscheinlich nicht verfügbar ist, wie durch das rote Ausrufezeichen angezeigt.

![Name der Standard-Website](publishing/_static/image5.png)

Ändern Sie den Standortnamen in etwas, das verfügbar ist, und wählen Sie einen Speicherort, der Nähe Ihres eigenen Standorts ist.

![geänderte Standortname](publishing/_static/image6.png)

Klicken Sie auf **OK**.

WebMatrix Performss einen Test, um zu bestimmen, ob der Server mit Ihrer Website kompatibel ist.

![Testen der Kompatibilität](publishing/_static/image7.png)

Wählen Sie **weiterhin**.

Die Ergebnisse von den Kompatibilitätstest werden angezeigt.

![Ergebnis der Kompatibilität](publishing/_static/image8.png)

Wählen Sie **weiterhin**.

WebMatrix zeigt die Dateien und Datenbanken, die auf der Website veröffentlicht werden soll. Da dies beim ersten Sie die Website veröffentlichen ist, werden alle Dateien aufgeführt. Deaktivieren Sie eine Datei, die nicht veröffentlicht werden. In nachfolgenden Veröffentlichungen werden nur die geänderten Dateien angezeigt. Finden Sie unter [Aktualisieren der Live-Websites: Erneutes Veröffentlichen von](#update).

![Vorschau veröffentlichen](publishing/_static/image9.png)

Wählen Sie **weiterhin**.

Nachdem die Website in Azure bereitgestellt wurde, wird eine Meldung angezeigt, der angibt, dass die Bereitstellung abgeschlossen ist.

![Veröffentlichung abgeschlossen](publishing/_static/image10.png)

Eine Website und Datenbank in Azure veröffentlicht wurden, und sind jetzt frei verfügbar. Klicken Sie auf den Link in die Meldung, die Veröffentlichung abgeschlossen ist, und sehen Sie nun Ihrer bereitgestellten Website. Sie oder ein Benutzer mit Zugriff auf das Internet hinzufügen oder Ändern von Einträgen in der Datenbank.

![](publishing/_static/image11.png)

<a id="host"></a>
## <a name="publishing-your-site-to-a-web-hosting-company"></a>Veröffentlichen Sie Ihre Website in ein Webhostingunternehmen

Wenn Sie nicht in Azure veröffentlichen möchten, können Sie stattdessen Ihre Website auf einen Webhostinganbieter veröffentlichen.

Klicken Sie auf die **Webhosting suchen** Link.

![Schaltfläche "Webhosting suchen", "im Dialogfeld" Veröffentlichungseinstellungen "](publishing/_static/image12.png)

Sie gelangen zu einer Seite auf der Microsoft-Website, in der Hostinganbieter aufgeführt, die ASP.NET unterstützen.

![Auf Microsoft-Website, in der Hostinganbieter aufgeführt.](publishing/_static/image13.png)

Natürlich kann es schwierig sein, wissen nun genau welche hostingfeatures langfristig erforderlich sein können. Hier sind einige Dinge zu bedenken:

- Für die Zwecke des Standorts WebPagesMovies müssen Sie ein separates Add-on für SQL Server verfügen, die oft zusätzliche Kosten. Auf der Website verwenden SQL Server Compact Edition, Sie die ist in sich geschlossen. Allerdings benötigen Sie möglicherweise SQL Server-Zugriff für einige Aufgaben für die zukünftige-Website, die Sie ausführen. Wenn Sie, dass Sie möglicherweise glauben, stellen Sie sicher, dass Sie SQL Server-Funktion später hinzufügen können.
- Überprüfen Sie, ob der Hostinganbieter der Web Deploy-publishing-Protokoll unterstützt. Sie veröffentlichen, indem Sie mithilfe von FTP-Protokoll, aber es ist einfacher, Web Deploy verwendet werden.

Einige Websites bieten einen kostenlosen Testzeitraum. Eine kostenlose Testversion ist eine gute Möglichkeit zum Testen der Veröffentlichung, und hosten, während Sie weiterhin mit WebMatrix und ASP.NET Web Pages experimentieren können.

Wählen Sie eine, die Ihnen gefallen. Für dieses Tutorial ausgewählt wir DiscountASP.NET, weil während wir das Tutorial erstellt haben, dieses Unternehmen verwendet eine heraufstufung haben, mit die Benutzer eine Website, die einige Monate lang kostenlos hosten können.

> [!NOTE]
> Unsere Wahl eines Hostinganbieters für dieses Tutorial sollten nicht als Empfehlung der jeweiligen Unternehmen über andere interpretiert werden. Aber wir hatten, um eine Abbildung auszuwählen, und DiscountASP.NET ist eines der vielen Unternehmen, die ASP.NET Web Pages und Web Deploy-Protokoll für die Veröffentlichung unterstützt.


In der Regel nach dem Sie mit dem hosting-Anbieter registriert haben, sendet des Unternehmens Sie eine e-Mail, die einen Benutzernamen und Kennwort, die URL der Web-Server und So weiter enthält. Wenn der Hostinganbieter Web Deploy-Protokoll unterstützt, können sie senden Sie eine Datei mit veröffentlichungseinstellungen oder können Sie eine herunterladen. Eine Datei mit veröffentlichungseinstellungen vereinfacht den Prozess für Sie.

Wenn Sie sich registriert haben und bereit für die Veröffentlichung, klicken Sie auf die **veröffentlichen** Schaltfläche im Menüband WebMatrix. Die **Veröffentlichungseinstellungen** Dialogfeld wird angezeigt.

Wenn der Hostinganbieter Sie eine Datei mit veröffentlichungseinstellungen gesendet haben, klicken Sie auf die **Importieren der veröffentlichungseinstellungen** verknüpfen, und importieren Sie die Datei. Wenn Sie nicht über eine Datei mit veröffentlichungseinstellungen verfügen, geben Sie in den Feldern, unter Verwendung der Werte, die der Hostinganbieter Sie per e-Mail gesendet. Dabei handelt es sich die **Veröffentlichungseinstellungen** das Dialogfeld sieht z. B. Wenn Sie fertig sind:

![Veröffentlichungseinstellungen ausgefüllt, die im Dialogfeld "Veröffentlichungseinstellungen"](publishing/_static/image14.png)

Klicken Sie auf **überprüft, ob Verbindung**. Wenn alles in Ordnung ist, wird das Dialogfeld meldet **erfolgreich verbunden**, was bedeutet, dass mit der hosting-Anbieter-Server kommunizieren kann.

![Nachricht erfolgreich, wenn veröffentlichen Einstellungen richtig sind.](publishing/_static/image15.png)

Wenn ein Problem vorliegt, führt WebMatrix empfiehlt es sich, Ihnen mitteilen, was das Problem ist:

![Fehlermeldung, wenn ein Problem mit den veröffentlichungseinstellungen](publishing/_static/image16.png)

Klicken Sie auf **speichern** zum Speichern der Einstellungen. WebMatrix bietet zum Ausführen eines Tests aus, um sicherzustellen, dass er ordnungsgemäß mit der hosting-Site kommunizieren kann:

![Nachricht, die zum Ausführen eines Tests des Veröffentlichungsvorgangs Angebot](publishing/_static/image17.png)

Klicken Sie auf **Ja**. WebMatrix lädt einige Beispieldateien an den Hostinganbieter ein. Wenn das Testen der Kompatibilität von fertig ist, meldet WebMatrix die Ergebnisse an:

![Testergebnisse veröffentlichen](publishing/_static/image18.png)

Wenn Sie fertig sind, fahren Sie fort, und klicken Sie auf **Weiter** tatsächlichen den Veröffentlichungsprozess zu starten. WebMatrix ermittelt, welche Dateien befinden sich in Ihre Website und befinden sich bereits auf dem Host (momentan keine) und bietet Ihnen eine Vorschau des Veröffentlichungsprozesses:

![Vorschau, welche Dateien, die der Veröffentlichungsprozess hochladen](publishing/_static/image19.png)

Die Liste der zu veröffentlichenden Dateien enthält, die Webseiten, die Sie z. B. erstellt haben *Movies.cshtml*. Die Liste enthält auch die Dateien für Hilfsmethoden, die Sie installiert haben, die Dateien in SQL Server Compact Edition für Ihre Datenbank ausführen und so weiter. Daher der ersten Veröffentlichungsprozess können beträchtlich sein.

Klicken Sie auf **Weiter**. WebMatrix kopiert die Dateien auf der hosting-Anbieter-Server. Wenn dies abgeschlossen ist, werden die Ergebnisse in der Statusleiste gemeldet:

![Meldung in der Statusleiste, wenn der Veröffentlichungsprozess erfolgreich abgeschlossen wurde](publishing/_static/image20.png)

Um Ihre live-Website anzuzeigen, klicken Sie auf den Link in der Statusleiste angezeigt. Hinzufügen *Filme* an die URL, und Sie sehen die *Movies.cshtml* -Datei, die Sie erstellt haben:

![Der live-Website mit der Seite "Movies"](publishing/_static/image21.png)

<a id="update"></a>
## <a name="updating-the-live-site-republishing"></a>Aktualisieren der Live-Websites: Erneutes Veröffentlichen

Nachdem Sie Ihre Website (zu Azure oder ein Webhostingunternehmen) veröffentlicht haben, sind zwei Kopien &mdash; die Version auf Ihrem Computer und die Version auf den Dienstanbieter. Sie sollten das Entwickeln des Standorts fortsetzen (Wenn nichts anderes, als Teil der nächsten Tutorial). Wenn Sie dies tun, müssen Sie Ihre Website erneut zu veröffentlichen, um Änderungen an den Dienstanbieter auf Ihrem Computer zu kopieren. Der Veröffentlichungsprozess in WebMatrix kann bestimmen, welche Dateien auf Ihrer Website geändert haben und nur die Dateien veröffentlichen.

Um anzuzeigen, wie funktioniert das erneute veröffentlichen, öffnen Sie die *Movies.cshtml* site, eine kleine Änderung vornehmen und speichern Sie die Datei. Ändern Sie z. B. den Titel in `Movies - Updated`.

Klicken Sie auf die **veröffentlichen** Schaltfläche im Menüband. WebMatrix wird bestimmt, was geändert wird, und zeigt eine Vorschau der Dateien, die sie veröffentlicht werden sollen.

![Das Dialogfeld "Veröffentlichen" mit der geänderten bereit Dateien, für die wiederveröffentlichung](publishing/_static/image22.png)

> [!IMPORTANT] 
> 
> Standardmäßig veröffentlicht WebMatrix Ihrer Datenbank (*.sdf* Datei) nur beim ersten Verwenden Sie die Website veröffentlichen. Sobald Ihre Website veröffentlicht wird, und Personen mit der Website interagieren, hat die Datenbank auf der live-Website in der Regel echte Daten von der Website. Sie müssen sehr darauf achten, nicht das Überschreiben der live-Datenbank mit der *.sdf* Datei, die auf dem Computer, die in der Regel nur für die Testdaten enthält. Deshalb die Warnung **Veröffentlichung überschreibt alle Remotedatenbanken**, und warum sich das Kontrollkästchen für *WebPagesMovies.sdf* ist standardmäßig deaktiviert.


Klicken Sie auf **Weiter**. WebMatrix die geänderten Dateien veröffentlicht, und es wird gezeigt, eine Erfolgsmeldung, wie es beim ersten, die Sie veröffentlicht.

Wechseln Sie zu der live-Website (Sie können den Link in der Success-Nachricht klicken, wenn es immer noch angezeigt werden) und stellen Sie sicher, dass die Änderung veröffentlicht wurde.

> [!TIP] 
> 
> **Bearbeiten von Dateien per Remotezugriff**
> 
> Als Alternative zum Ändern Ihrer Website und dann erneut veröffentlichen können Sie die remote-Dateien direkt in WebMatrix bearbeiten. Öffnen Sie eine Datei, die auf den Dienstanbieter ist, und WebMatrix lädt eine Kopie der Datei bearbeiten, in diesem Szenario. Jedes Mal, wenn Sie die Datei speichern, werden die Änderungen von WebMatrix an den Standort gesendet.
> 
> Remote bearbeiten, ist eine einfache Möglichkeit, um Ihre live-Website zu ändern. Allerdings werden nicht auf diese Weise vorgenommene Änderungen mit den Dateien an Ihrem lokalen Standort synchronisiert. Um die lokalen Dateien mit dem Remotestandort zu synchronisieren, können Sie die Remotedateien herunterzuladen. Dieser Prozess funktioniert ähnlich wie die Veröffentlichung, außer in umgekehrter Reihenfolge.
> 
> Es wird nicht mehr über die Remote-Bearbeitung und Remote-Download-Funktionen von WebMatrix hier beschrieben. Sie sind sehr nützlich, wenn mehrere Personen am selben Standort auf verschiedenen Computern zu arbeiten müssen. Weitere Informationen finden Sie unter [veröffentlichen und Bearbeiten von einem Remotestandort mit WebMatrix 2 Beta](https://go.microsoft.com/fwlink/?LinkId=251591).


## <a name="additional-resources"></a>Zusätzliche Ressourcen

- [ASP.NET WebMatrix ASP.NET Web Pages-Forum](https://forums.asp.net/1224.aspx/1?WebMatrix+and+ASP+NET+Web+Pages), Fragen, die hervorragend zum Posten und Antworten.

> [!div class="step-by-step"]
> [Vorherige](layouts.md)
