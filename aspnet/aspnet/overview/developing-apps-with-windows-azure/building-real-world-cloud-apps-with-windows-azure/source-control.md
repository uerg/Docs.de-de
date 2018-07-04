---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/source-control
title: Datenquellen-Steuerelement (erstellen realer Cloud-Apps mit Azure) | Microsoft-Dokumentation
author: MikeWasson
description: Die Building Real World Cloud Apps mit Azure-e-Book basiert auf einer Präsentation von Scott Guthrie entwickelt wurde. Es wird erläutert, 13 Muster und Vorgehensweisen, die er können...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/23/2015
ms.topic: article
ms.assetid: 2a0370d3-c2fb-4bf3-88b8-aad5a736c793
ms.technology: ''
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/source-control
msc.type: authoredcontent
ms.openlocfilehash: 12c695b65a21452fdc4a31e821854253bccec0d7
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/03/2018
ms.locfileid: "37368974"
---
<a name="source-control-building-real-world-cloud-apps-with-azure"></a>Datenquellen-Steuerelement (erstellen realer Cloud-Apps mit Azure)
====================
durch [Mike Wasson](https://github.com/MikeWasson), [Rick Anderson](https://github.com/Rick-Anderson), [Tom Dykstra](https://github.com/tdykstra)

[Download korrigieren Projekt](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) oder [E-Book herunterladen](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> Die **Building Real World Cloud Apps mit Azure** e-Book basiert darauf, dass eine Präsentation von Scott Guthrie entwickelt wurde. Es wird erläutert, 13 Muster und Methoden, die Ihnen helfen können, werden erfolgreiche Entwicklung von Web-apps für die Cloud. Weitere Informationen zu e-Book, finden Sie unter [im ersten Kapitel](introduction.md).


Datenquellen-Steuerelement ist entscheidend für alle Cloud-Entwicklungsprojekte, nicht nur für teamumgebungen. Sie wäre nicht vorstellen, Quellcode zu bearbeiten, oder sogar ein Word-Dokument ohne eine Rückgängigfunktion und die automatischen Sicherungen und die Datenquellen-Steuerelement können Sie diese Funktionen auf Projektebene, in denen sie noch mehr Zeit sparen können, wenn etwas schief geht. Mit Cloud dateiquellcode-Verwaltungsdienste müssen Sie nicht mehr komplizierte Einrichtung kümmern und können Sie Visual Studio Online quellcodeverwaltung für bis zu 5 Benutzer kostenlos.

Der erste Teil dieses Kapitels erläutert die drei wichtige bewährte Methoden zu berücksichtigen:

- [Behandeln von Automation-Skripts als Quellcode](#scripts) und Version Sie zusammen mit Ihrem Anwendungscode.
- [Checken Sie niemals Geheimnisse](#secrets) (sensible Daten wie z. B. Anmeldeinformationen) in ein Quellcoderepository.
- [Einrichten des quellbranches](#devops) den DevOps-Workflow zu aktivieren.

Der Rest des Kapitels erhalten einige beispielimplementierungen dieser Muster in Visual Studio, Azure und Visual Studio Online:

- [Hinzufügen von Skripts zu quellcodeverwaltung in Visual Studio](#vsscripts)
- [Sensible Daten im Azure Store](#appsettings)
- [Verwenden Sie Git in Visual Studio und Visual Studio Online](#gittfs)

<a id="scripts"></a>
## <a name="treat-automation-scripts-as-source-code"></a>Behandeln von Automation-Skripts als Quellcode

Wenn Sie ein Cloud-Projekt arbeiten Sie Dinge häufig ändern, und schnell reagieren auf Probleme, die von Ihren Kunden gemeldet werden sollen. Schnelle Reaktion auf umfasst mithilfe von Automatisierungsskripts, wie unter der [automatisieren alles](automate-everything.md) Kapitel. Alle Skripts, mit denen Sie Ihre Umgebung erstellen, bereitstellen, skalieren sie usw. mit Quellcode der Anwendung synchronisiert werden müssen.

Um Skripts mit Code synchron zu halten, speichern Sie sie in Ihr Quellcodeverwaltungssystem ein. Wenn Sie müssen zum Zurücksetzen von Änderungen, oder stellen eine schnelle Lösung an den Produktionscode entwickeln von Code abweicht, müssen Sie dann Zeit dabei vergeuden, nachzuverfolgen, welche Einstellungen geändert haben oder die Teammitglieder Kopien der Version verfügen, die Sie benötigen. Sie sind sicher, die die benötigten Skripts synchron mit der Codebasis, die Sie benötigen diese Angaben für sind, und Sie sind sicher, dass alle Teammitglieder mit den gleichen Skripts funktionieren. Klicken Sie dann, ob Sie testen und Bereitstellen der ein Hotfix, Produktion oder Entwicklung neuer Features automatisieren möchten, müssen das richtige Skript für den Code Sie, die aktualisiert werden muss.

<a id="secrets"></a>
## <a name="dont-check-in-secrets"></a>Überprüfen Sie mit der nicht im Geheimnisse

Ein Quellcoderepository ist in der Regel auf zu viele Entwickler dafür eine angemessen sichere Ort für vertrauliche Daten wie Kennwörter, zugreifen kann. Wenn Skripts Geheimnisse wie Kennwörter benötigen, parametrisieren Sie die Einstellungen, damit sie nicht im Quellcode gespeichert, und speichern Ihre Geheimnisse, die an anderer Stelle.

Z. B. Veröffentlichen mit Azure können, die Sie Dateien herunterladen, die enthalten, Einstellungen, um die Erstellung von veröffentlichungsprofilen automatisieren. Diese Dateien enthalten Benutzernamen und Kennwörter, die autorisiert sind, um Ihre Azure-Dienste zu verwalten. Bei Verwendung dieser Methode zum Erstellen von veröffentlichungsprofilen, und wenn Sie diese Dateien in die quellcodeverwaltung einchecken, werden jeder mit Zugriff auf Ihr Repository sehen diese Benutzernamen und Kennwörter. Sie können das Kennwort sicher im Veröffentlichungsprofil selbst speichern, da er verschlüsselt und befindet sich im eine *. pubxml.user* -Datei, die standardmäßig nicht in der quellcodeverwaltung enthalten ist.

<a id="devops"></a>
## <a name="structure-source-branches-to-facilitate-devops-workflow"></a>Struktur des quellbranches um DevOps-Workflow zu erleichtern.

Wie Sie Branches in Ihrem Repository implementieren wirkt sich auf Ihre Fähigkeit, sowohl neue Features entwickeln, und Beheben von Problemen in der Produktion aus. Hier ist ein Muster, dass die Größe der Medium viele Teams verwenden:

![Quelle branchstruktur](source-control/_static/image1.png)

Der master-Branch entspricht immer Code, der in der Produktion ist. Branches unterhalb Master entsprechen verschiedenen Phasen im Lebenszyklus Entwicklung. Der Development-Branch ist, in dem Sie die neuen Features implementiert. Für ein kleines Team können Sie nur Master und -Entwicklung haben, aber es häufig wird empfohlen, Personen eine stagingverzweigung zwischen Entwicklung und Master. Sie können Staging verwenden, für die endgültige Integration zu testen, bevor ein Update für die Produktion verschoben wird.

Für große Teams, die möglicherweise separaten Branches für jede neue Funktion; für ein kleineres Teams müssen Sie möglicherweise alle Benutzer auf dem Development-Branch einchecken.

Wenn Sie einen Branch für jede Funktion Feature A bereit besteht seine Änderungen am Quellcode nach oben in die Entwicklung branch und nach-unten in den anderen funktionszweigen. Dieses Quellcodes Zusammenführung kann sehr zeitaufwändig sein, und um die Arbeit und dennoch die Sicherheit Features separate zu vermeiden, implementieren einige Teams Alternative aufgerufen *[featuretoggles](http://en.wikipedia.org/wiki/Feature_toggle)* (auch bekannt als *featureflags*). Dies bedeutet, dass der gesamte Code für alle Features befindet sich in derselben Filiale, aber Sie aktivieren oder Deaktivieren von einzelnen Funktionen mithilfe von Switches im Code. Nehmen wir beispielsweise an Feature A ist ein neues Feld für die Aufgaben-app-Fix It und Feature B fügt Funktionen zum Zwischenspeichern hinzu. Der Code für beide Funktionen kann in der Development-Branch werden, nur Anzeige der app wird das neue Feld, wenn eine Variable auf "true", und es festgelegt ist nur verwenden jedoch zwischenspeichern, wenn eine andere Variable festgelegt ist auf "true". Wenn Feature A nicht höher gestuft werden, aber die Feature B bereit ist, können Sie höher Stufen der gesamte Code in der Produktion mit dem Feature A-Schalter aus, und schalten Sie das Feature B. Sie können-Funktion A abgeschlossen und Stufen es später noch Mal, alles ohne Source Code zusammenführen.

Und zwar unabhängig davon, ob Sie Branches oder schaltet für Funktionen verwenden, kann eine Verzweigungsstruktur wie folgt Sie Ihren Code von der Entwicklung in der produktionsumgebung auf agile und wiederholbare Weise zu übertragen.

Diese Struktur ermöglicht Ihnen, schnell auf Kundenfeedback zu reagieren. Wenn Sie eine schnelle Lösung für die Produktion vornehmen müssen, auch möglich, die effizient auf agile Weise. Sie können eine Verzweigung vom Master oder Staging erstellen, und wenn er bereit ist, um in den masterbranch zusammenführen und nach-unten in der Entwicklung und Feature-Branches.

![Hotfix-branch](source-control/_static/image2.png)

Ohne eine Verzweigungsstruktur wie folgt mit der Trennung von Produktions- und entwicklungsbranches können Sie ein Produktionsproblem an der Position der neuen Featurecode sowie die Korrektur für die Produktion heraufstufen zuordnen. Der neue Code für die Funktion möglicherweise nicht vollständig getestet und bereit zur Produktion, und möglicherweise müssen Sie eine Menge Arbeit Zurückziehen von Änderungen, die nicht bereit sind. Oder Sie möglicherweise die Korrektur, um die Änderungen zu testen, und Regen sie bereit für die Bereitstellung verzögern.

Als Nächstes sehen Sie Beispiele für diese drei Muster in Visual Studio, Azure und Visual Studio Online zu implementieren. Dies sind Beispiele für statt-Schritt-how-to--It-Anweisungen; Detaillierte Anweisungen, die alle von den erforderlichen Kontext bereitstellen, finden Sie unter den [Ressourcen](#resources) Abschnitt am Ende des Kapitels.

<a id="vsscripts"></a>
## <a name="add-scripts-to-source-control-in-visual-studio"></a>Hinzufügen von Skripts zu quellcodeverwaltung in Visual Studio

Sie können Skripts zur quellcodeverwaltung in Visual Studio hinzufügen, dazu können Sie sie in einem Visual Studio-Projektmappenordner (vorausgesetzt, dass Ihr Projekt in der quellcodeverwaltung ist). Hier ist eine Möglichkeit dafür.

Erstellen Sie einen Ordner für die Skripts in Ihrem Projektordner (den gleichen Ordner, die Ihre *sln* Datei).

![Ordner "Automatisierung"](source-control/_static/image3.png)

Kopieren Sie die Skriptdateien in den Ordner an.

![Automation Ordnerinhalt auflisten](source-control/_static/image4.png)

Fügen Sie in Visual Studio dem Projekt einen Projektmappenordner hinzu.

![Neuen Projektmappenordner Menüauswahl](source-control/_static/image5.png)

Und fügen Sie die Skriptdateien in den Projektmappenordner hinzu.

![Menüauswahl vorhandenes Element hinzufügen](source-control/_static/image6.png)

![Dialogfeld "Vorhandenes Element hinzufügen"](source-control/_static/image7.png)

Die Skriptdateien befinden sich nun in Ihrem Projekt und quellcodeverwaltung verfolgt ihre Änderungen an der Typsystemversion zusammen mit den entsprechenden Änderungen am Quellcode.

<a id="appsettings"></a>
## <a name="store-sensitive-data-in-azure"></a>Sensible Daten im Azure Store

Wenn Sie Ihre Anwendung in einer Azure-Website ausführen, werden eine Möglichkeit zum Vermeiden der Speicherung von Anmeldeinformationen in der quellcodeverwaltung in Azure zu speichern, die stattdessen.

Beispielsweise speichert die Fix It-Anwendung in der Web.config-Datei zwei Verbindungszeichenfolgen, die Kennwörter in der Produktion und einen Schlüssel, der Ihrem Azure Storage-Konto Zugriff hat.

[!code-xml[Main](source-control/samples/sample1.xml?highlight=2-3,11)]

Wenn Sie die eigentliche Produktion-Werte für diese Einstellungen im ablegen Ihrer *"Web.config"* -Datei, oder wenn Sie sie, in Platzieren der *Datei "Web.Release.config"* Datei so konfigurieren Sie eine Web.config-Transformation, um sie während der Bereitstellung einfügen Sie müssen im Quell-Repository gespeichert werden. Wenn Sie die Datenbank-Verbindungszeichenfolgen in der Produktion geben Sie das Veröffentlichungsprofil, das Kennwort werden in Ihrem *pubxml* Datei. (Können Sie ausschließen, die *pubxml* Datei aus der quellcodeverwaltung, aber dann verlieren Sie den Vorteil, dass die anderen bereitstellungseinstellungen freigeben.)

Azure bietet Ihnen eine Alternative für die **"appSettings"** und Verbindungszeichenfolgen Teile der *"Web.config"* Datei. Hier ist der relevante Teil der **Konfiguration** Registerkarte für eine Website im Azure-Verwaltungsportal:

!["appSettings" und ConnectionStrings im portal](source-control/_static/image8.png)

Wenn Sie ein Projekt auf die Anwendung ausgeführt wird und dieser Website bereitstellen, überschreiben beliebige Werte, die Sie in Azure gespeichert haben, werden alle Werte in der Datei "Web.config".

Sie können diese Werte in Azure mit dem Verwaltungsportal oder Skripts festlegen. Die Umgebung erstellen Automatisierungsskripts, die Sie gesehen, in haben der [automatisieren alles](automate-everything.md) Kapitel erstellt eine Azure SQL-Datenbank, ruft Sie ab, den Speicher und SQL-Datenbank-Verbindungszeichenfolgen und speichert diese Informationen in den Einstellungen für Ihre Website.

[!code-powershell[Main](source-control/samples/sample2.ps1)]

[!code-powershell[Main](source-control/samples/sample3.ps1)]

Beachten Sie, dass die Skripts, damit an die Quell-Repository tatsächliche Werte beibehalten werden nicht parametrisiert werden.

Wenn Sie lokal in Ihrer Entwicklungsumgebung ausführen, die app liest die lokale Datei "Web.config"-Datei und der Verbindung angegebene Verbindungszeichenfolge verweist auf die SQL Server LocalDB-Datenbank in der *App\_Daten* Ordner des Webprojekts. Wenn Sie die app in Azure ausführen und die app versucht, diese Werte aus der Datei "Web.config" zu lesen, sind was erhält zurück und wird verwendet, die Werte, die für die Website, nicht was tatsächlich in der Datei "Web.config" wird gespeichert ist.

<a id="gittfs"></a>
## <a name="use-git-in-visual-studio-and-visual-studio-online"></a>Verwenden Sie Git in Visual Studio und Visual Studio Online

Sie können der quellcodeverwaltungsumgebung verwenden, um DevOps-Verzweigungsstruktur, die zuvor gezeigte zu implementieren. Für verteilte Teams einen [verteilte Versionskontrollsystem](http://en.wikipedia.org/wiki/Distributed_revision_control) (DVCS) funktioniert am besten; für andere Teams eine [ein zentralisiertes System](http://en.wikipedia.org/wiki/Revision_control) besser funktioniert.

[Git](http://git-scm.com/) ist einem DVCS, das ist sehr beliebt geworden. Wenn Sie Git für die quellcodeverwaltung verwenden, müssen Sie eine vollständige Kopie des Repositorys mit dem gesamten Verlauf auf dem lokalen Computer. Viele Benutzer bevorzugen, weil es einfacher ist weiterarbeiten, wenn Sie nicht mit dem Netzwerk verbunden sind – Sie können Sie weiterhin commits und Rollbacks, erstellen und zwischen Branches wechseln und so weiter. Auch wenn Sie mit dem Netzwerk verbunden sind, ist es einfacher und schneller, erstellen Sie Branches und zwischen Branches wechseln, wenn alles lokal ist. Sie können auch lokale Commits und Rollbacks durchführen, ohne Auswirkungen auf andere Entwickler. Und Sie können Commits vor dem Senden an den Server batch.

[Microsoft Visual Studio Online](https://www.visualstudio.com/)(VSO), früher bekannt als Team Foundation Service, Git bietet und [Team Foundation Version Control](https://msdn.microsoft.com/library/ms181237(v=vs.120).aspx) (TFVC; zentrale quellcodeverwaltung). Hier verwenden einige Teams bei Microsoft im Azure-zentrale quellcodeverwaltung, einige verwenden, verteilt, und einige Verwenden einer Kombination aus (bei einigen Projekten zentralisiert und für andere Projekte verteilt). Der Visual Studio Online-Dienst ist für bis zu 5 Benutzer kostenlos. Sie können für einen kostenlosen Plan registrieren [hier](https://go.microsoft.com/fwlink/?LinkId=307137).

Visual Studio 2013 bietet erstklassige integrierte [Git-Unterstützung](https://msdn.microsoft.com/library/hh850437.aspx); hier ist eine kurze Demo, wie es geht.

Ein Projekt in Visual Studio 2013 geöffnet, mit der rechten Maustaste der Projektmappe in **Projektmappen-Explorer**, und wählen Sie **Projektmappe zur Quellcodeverwaltung hinzufügen**.

![Projektmappe zur Quellcodeverwaltung hinzufügen](source-control/_static/image9.png)

Visual Studio gefragt werden, wenn Sie TFVC (zentralisierte Versionskontrolle) oder Git verwenden möchten.

![Wählen Sie die Datenquellen-Steuerelement](source-control/_static/image10.png)

Wenn Sie Git wählen, und klicken Sie auf **OK**, Visual Studio erstellt ein neues lokales Git-Repository in Ihrem Projektmappenordner. Das neue Repository sind keine Dateien noch; Sie müssen diese in das Repository hinzufügen, indem Sie ein Git-Commit. Mit der rechten Maustaste in der Lösung in **Projektmappen-Explorer**, und klicken Sie dann auf **Commit**.

![Commit](source-control/_static/image11.png)

Visual Studio automatisch alle Projektdateien für den Commit stellt es vorab bereit und listet diese in **Team Explorer** in die **eingeschlossene Änderungen** Bereich. (Würde es einige nicht im Commit enthalten sein sollen, können Sie auswählen, mit der rechten Maustaste, und klicken Sie auf **ausschließen**.)

![Team Explorer](source-control/_static/image12.png)

Geben Sie einen commitkommentar, und klicken Sie auf **Commit**, Visual Studio führt den Commit und zeigt die Commit-ID.

![Team Explorer-Änderungen](source-control/_static/image13.png)

Jetzt, wenn Sie Code ändern, sodass sie unterscheidet sich von den im Repository, können Sie einfach die Unterschiede anzeigen. Mit der rechten Maustaste eine Datei, die Sie geändert haben, wählen Sie **vergleichen mit Unmodified**, und Sie erhalten einen Vergleich anzeigen, die Ihre nicht gespeicherte Änderungen anzeigt.

![Mit unveränderter Version vergleichen](source-control/_static/image14.png)

![Diff mit Änderungen](source-control/_static/image15.png)

Sie können Änderungen leicht erkennen, Sie machen, und checken Sie sie.

Angenommen, müssen Sie einen Branch – können Sie dies in Visual Studio zu tun. In **Team Explorer**, klicken Sie auf **neuen Branch**.

![Neuen Branch für Team Explorer](source-control/_static/image16.png)

Geben Sie einen Branch ein, klicken Sie auf **Verzweigung erstellen**, und wenn Sie ausgewählt **Branch Auschecken**, überprüft Visual Studio automatisch den neuen Branch.

![Neuen Branch für Team Explorer](source-control/_static/image17.png)

Sie können nun Änderungen an Dateien vornehmen und Einchecken für diesen Branch. Und Sie können problemlos Wechseln zwischen Branches und Visual Studio automatisch Synchronisierungen, die die Dateien, je nachdem, was Sie branch ausgecheckt haben. In diesem Beispiel wird die Webseite im Titel  *\_Layout.cshtml* wurde geändert in "Hotfix 1" in HotFix1 Branch.

![Hotfix1 branch](source-control/_static/image18.png)

Wenn Sie zurück zum Master wechseln branch, den Inhalt der  *\_Layout.cshtml* Datei automatisch zurückgesetzt, was sie in der master-Branch werden.

![Master-branch](source-control/_static/image19.png)

Diese ein einfaches Beispiel, wie Sie schnell eine Verzweigung erstellen und kippen zwischen Branches wechseln. Dieses Feature ermöglicht einen äußerst agilen Workflow, die mithilfe von die branchstruktur und Automatisierungsskripts angezeigt, der [automatisieren alles](automate-everything.md) Kapitel. Sie können z. B. werden in der Development-Branch arbeiten, erstellen Sie einen Hotfix Branch abseits des masterbranches, wechseln Sie zu den neuen Branch, Ihre Änderungen daran vornehmen und committet haben, und wechseln Sie dann zu dem Development-Branch und fortgesetzt, wo Sie aufgehört haben.

Was Sie hier gesehen haben ist, wie Sie mit einem lokalen Git-Repository in Visual Studio arbeiten. In einer teamumgebung push Sie in der Regel auch Änderungen zu einem gemeinsamen Repository. Visual Studio-Tools können auch Sie auf einem remote-Git-Repository zu verweisen. Können Sie zu diesem Zweck "github.com", oder Sie können [Git in Visual Studio Online](https://msdn.microsoft.com/library/hh850437.aspx) mit allen anderen Visual Studio Online Funktionen wie z. B. Arbeitselement und nachverfolgung von Programmfehlern integriert.

Dies ist nicht die einzige Möglichkeit, die Sie eine agile Verzweigungsstrategie, natürlich implementieren können. Sie können den gleichen agilen-Workflow mit einem zentralen Quellcodeverwaltungs-Repository aktivieren.

## <a name="summary"></a>Zusammenfassung

Messen Sie den Erfolg von Ihr Quellcodeverwaltungssystem basierend auf wie schnell Sie können eine Änderung vornehmen und sie auf eine Weise sicheres und berechenbares live. Wenn Sie sich eine Änderung vornehmen, da Sie müssen lediglich eine oder zwei manuelle Tests auf abschreckend, können Sie sich Fragen habe process-wise oder test-wise, damit Sie innerhalb von Minuten oder am schlechtesten, die nicht mehr als eine Stunde, die diese Änderung vornehmen können. Eine Strategie zum Ausführen, wird zum Implementieren von continuous Integration und continuous Delivery, die wir im behandeln werde die [im nächsten Kapitel](continuous-integration-and-continuous-delivery.md).

<a id="resources"></a>
## <a name="resources"></a>Ressourcen

Die [Visual Studio Online](https://www.visualstudio.com/) -Portal bietet die Dokumentation und Support Services, und Sie können für ein Konto registrieren. Wenn Sie Visual Studio 2012 und Git verwenden möchten, finden Sie unter [Visual Studio-Tools für Git](https://visualstudiogallery.msdn.microsoft.com/abafc7d6-dcaa-40f4-8a5e-d6724bdb980c).

Weitere Informationen zu TFVC (zentralisierte Versionskontrolle) und Git (verteilte Versionskontrolle) finden Sie unter den folgenden Ressourcen:

- [Welches Versionskontrollsystem sollte ich verwenden: TFVC oder Git?](https://msdn.microsoft.com/library/vstudio/ms181368.aspx#tfvc_or_git_summary) MSDN-Dokumentation enthält eine Tabelle, die Zusammenfassung der Unterschiede zwischen TFVC und Git.
- [Auch wie ich Team Foundation Server und z.B. ich wie Git verwenden, aber das ist besser?](https://blogs.msdn.com/b/visualstudiouk/archive/2013/08/05/well-i-like-team-foundation-server-and-i-like-git-but-which-is-better.aspx) Vergleich von Git- und TFVC.

Weitere Informationen zu verzweigungsstrategien finden Sie unter den folgenden Ressourcen:

- [Erstellung einer Versionspipeline mit Team Foundation Server 2012](https://msdn.microsoft.com/library/dn449957.aspx). Microsoft Patterns and Practices-Dokumentation. Finden Sie in Kapitel 6, Informationen zu verzweigungsstrategien. Befürworter-Funktion über featurebranches schaltet und Verzweigungen für Funktionen verwendet, advocates sorgen kurzlebige (Stunden oder Tage lang höchstens).
- [Version Control Handbuch](https://aka.ms/vsarsolutions). Leitfaden Sie zu verzweigungsstrategien von ALM Rangers. Finden Sie auf der Registerkarte "Downloads" in der Verzweigung Strategies.pdf.
- [Softwareentwicklung mit Funktionsschaltern](https://msdn.microsoft.com/magazine/dn683796.aspx). MSDN Magazine-Artikel.
- [Aktivieren/Deaktivieren Features](http://martinfowler.com/bliki/FeatureToggle.html). Einführung in die Funktion schaltet / featureflags Blog von Martin.
- [Feature Toggles Vs Featurebranches](http://geekswithblogs.net/Optikal/archive/2013/02/10/152069.aspx). Einem anderen Blogbeitrag zu funktionsschaltern von Dylan Smith.

Weitere Informationen zum Umgang mit vertraulichen Informationen, der nicht in Repositorys zur quellcodeverwaltung aufbewahrt werden sollen, finden Sie unter den folgenden Ressourcen:

- [Bewährte Methoden für die Bereitstellung von Kennwörtern und anderen vertraulichen Daten in ASP.NET und Azure App Service](../../../../identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure.md).
- [Azure-Websites: Wie Anwendungs- und Verbindung Zeichenfolgen arbeiten](https://azure.microsoft.com/blog/2013/07/17/windows-azure-web-sites-how-application-strings-and-connection-strings-work/). Erläutert die Azure-Funktion, die überschreibt `appSettings` und `connectionStrings` Daten in die *"Web.config"* Datei.
- [Benutzerdefinierte Konfiguration und Anwendung von Einstellungen in Azure Websites - mit Stefan Schackow](https://azure.microsoft.com/documentation/videos/configuration-and-app-settings-of-azure-web-sites/).

Weitere Informationen zu anderen Methoden zum Schützen von vertraulichen Informationen aus der quellcodeverwaltung finden Sie unter [ASP.NET MVC: behalten Sie privaten Einstellungen von Quellcodeverwaltung](http://typecastexception.com/post/2014/04/06/ASPNET-MVC-Keep-Private-Settings-Out-of-Source-Control.aspx).

> [!div class="step-by-step"]
> [Zurück](automate-everything.md)
> [Weiter](continuous-integration-and-continuous-delivery.md)
