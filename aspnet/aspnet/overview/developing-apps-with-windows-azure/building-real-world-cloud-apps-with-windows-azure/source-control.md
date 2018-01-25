---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/source-control
title: Datenquellen-Steuerelement (Building Real-World Cloud Apps with Azure) | Microsoft Docs
author: MikeWasson
description: "Die Building Real World Cloud Apps with Azure-e-Book basiert auf einer Präsentation von Scott Guthrie entwickelt. Es wird erläutert, 13 Muster und Vorgehensweisen, die er können..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/23/2015
ms.topic: article
ms.assetid: 2a0370d3-c2fb-4bf3-88b8-aad5a736c793
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/source-control
msc.type: authoredcontent
ms.openlocfilehash: e3ce68b949199db35c18a09771d99d38562b74e9
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/24/2018
---
<a name="source-control-building-real-world-cloud-apps-with-azure"></a>Datenquellen-Steuerelements (Building Real-World Cloud Apps with Azure)
====================
durch [Mike Wasson](https://github.com/MikeWasson), [Rick Anderson](https://github.com/Rick-Anderson), [Tom Dykstra](https://github.com/tdykstra)

[Download Behebungsskript Projekt](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) oder [E-Book herunterladen](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> Die **Building Real World Cloud Apps with Azure** e-Book basiert auf einer Präsentation von Scott Guthrie entwickelt. Es wird erläutert, 13 Muster und Vorgehensweisen, die Ihnen helfen können erfolgreich ausgeführt entwickeln Web-apps für die Cloud. Weitere Informationen zu e-Book herunterladen, finden Sie unter [im Kapitel über das erste](introduction.md).


Datenquellen-Steuerelements ist entscheidend für alle Cloud-Entwicklungsprojekte, nicht nur die Team-Umgebungen. Stellen Sie wäre nicht Quellcode bearbeiten oder sogar ein Word-Dokument ohne eine Rückgängigfunktion und die automatische Sicherungen und die Datenquellen-Steuerelements können Sie diese Funktionen auf Projektebene, in denen sie noch mehr Zeit sparen, wenn etwas schief geht. Mit Cloud-Quellcodeverwaltungsdienste müssen Sie nicht mehr komplizierte Installation kümmern, und können Sie Visual Studio Online für bis zu 5 Benutzer kostenlos Datenquellen-Steuerelements.

Der erste Teil dieses Kapitel erklärt drei wichtige best Practices zu bedenken:

- [Behandeln von Automatisierungsskripts als Quellcode](#scripts) und Version Sie zusammen mit der Anwendungscode.
- [Checken Sie niemals geheime Schlüssel](#secrets) (sensible Daten wie z. B. Anmeldeinformationen) in einem Quellcoderepository.
- [Einrichten der Quelle Verzweigungen](#devops) an den DevOps-Workflow zu aktivieren.

Der Rest des Kapitels bietet einige Beispiel-Implementierungen dieser Muster in Visual Studio, Azure und Visual Studio Online:

- [Hinzufügen von Skripts zur quellcodeverwaltung in Visual Studio](#vsscripts)
- [Speichern von sensiblen Daten in Azure](#appsettings)
- [Verwenden Sie Git in Visual Studio und Visual Studio Online](#gittfs)

<a id="scripts"></a>
## <a name="treat-automation-scripts-as-source-code"></a>Behandeln von Automatisierungsskripts als Quellcode

Wenn Sie ein Cloud-Projekt arbeiten, ändern Sie Dinge häufig und schnell reagieren auf Probleme, die von unseren Kunden gemeldet werden sollen. Schnelle Reaktion auf umfasst die Verwendung des Automatisierungsskripts, wie beschrieben in der [automatisieren alles](automate-everything.md) Kapitel. Alle Skripts, mit denen Sie die Umgebung erstellt haben bereitstellen ihm, skalieren sie usw. mit Quellcode Ihrer Anwendung synchronisiert werden müssen.

Um die Skripts mit Code synchron zu halten, speichern Sie sie in Ihr Quellcodeverwaltungssystem aus. Wenn die Notwendigkeit zum Zurücksetzen von Änderungen oder einen Quick Fix an Produktionscode Dies unterscheidet sich von der Entwicklungscode vornehmen, müssen Sie anschließend zeitaufwendiges zum Auffinden, welche Einstellungen geändert haben oder die Teammitglieder Kopien der Version sein Sie müssen versuchen. Sie sind sicher, die die benötigten Skripts synchron mit der Codebasis, die Sie benötigen sie für sind, und Sie sind sicher, dass alle Teammitglieder mit den gleichen Skripts funktionieren. Klicken Sie dann, ob Sie zum Automatisieren von Tests und Bereitstellung von ein Hotfix, Produktions- oder Neuentwicklungen-Feature müssen, müssen das richtige Skript für den Code Sie, die aktualisiert werden muss.

<a id="secrets"></a>
## <a name="dont-check-in-secrets"></a>Aktivieren Sie nicht in der geheime Schlüssel

Eine Quellcoderepository ist in der Regel auf zu viele Personen dafür eine entsprechend sicheren Ort für vertrauliche Daten wie Kennwörter, zugegriffen werden kann. Wenn Skripts Geheimnisse wie Kennwörter benötigen, werden parametrisieren Sie diese Einstellungen, sodass sie nicht im Quellcode gespeichert, und speichern Ihre vertraulichen Daten, die an anderer Stelle.

Beispielsweise veröffentlichen Azure können, die Sie Dateien herunterladen, die enthalten, Einstellungen, um die Erstellung von Veröffentlichungsprofile zu automatisieren. Diese Dateien enthalten Benutzernamen und Kennwörter, die autorisiert sind, um die Azure-Dienste zu verwalten. Bei Verwendung dieser Methode zum Erstellen der Veröffentlichungsprofile, und wenn Sie diese Dateien zur Versionskontrolle einchecken, kann jede Person mit Zugriff auf Ihr Repository sehen, diese Benutzernamen und Kennwörter. Sie können das Kennwort sicher im Veröffentlichungsprofil selbst speichern, da er verschlüsselt und befindet sich im ein *. pubxml.user* Datei, die standardmäßig nicht in der quellcodeverwaltung enthalten ist.

<a id="devops"></a>
## <a name="structure-source-branches-to-facilitate-devops-workflow"></a>Struktur Quelle Verzweigungen DevOps Workflows zu erleichtern.

Wie Sie in Ihrem Repository Verzweigungen implementieren wirkt sich auf die Möglichkeit, neue Funktionen zu entwickeln, und Beheben von Problemen in der Produktion aus. Hier ist ein Muster, dass viele Mittel Teams verwenden die Größe:

![Quelle Verzweigungsstruktur](source-control/_static/image1.png)

Die hauptverzweigung entspricht immer Code, der in der Produktion ist. Verzweigungen unterhalb Master entsprechen verschiedenen Phasen in den Entwicklungslebenszyklus. Die Development-Verzweigung ist, in dem Sie neue Features zu implementieren. Für ein kleines Team möglicherweise gerade müssen Master "und" Entwicklung, aber häufig empfiehlt Personen eine staging Verzweigung zwischen Entwicklungs- und Master haben. Sie können die Staging verwenden, für die endgültige Integration testen, bevor ein Update für die Produktion verschoben wird.

Für big Teams möglicherweise separate Verzweigungen für jede neue Funktion; für eine kleinere Team müssen Sie möglicherweise "Jeder" Einchecken zur Development-Verzweigung.

Haben eine Verzweigung für jede Funktion Funktion A bereit Sie-Zusammenführung wird immer der quellcodeänderungen in die Entwicklung von Verzweigen und nach unten in der anderen Zweige der Funktion. Zusammenführung der Quellcode kann zeitaufwändig sein, und um diese Aufgabe gleichzeitig Funktionen separate zu vermeiden, implementieren einige Teams Alternative aufgerufen  *[feature schaltet](http://en.wikipedia.org/wiki/Feature_toggle)*  (auch bekannt als *feature Flags*). Dies bedeutet, dass der gesamte Code für alle Funktionen in der gleichen Verzweigung ist, aber Sie aktivieren oder Deaktivieren von einzelnen Funktionen mithilfe von Switches im Code. Nehmen Sie z. B. an Funktion A ist ein neues Feld für korrigieren app-Vorgänge und Funktion B fügt Funktionen zum Zwischenspeichern. Der Code für beide Funktionen kann in die Development-Verzweigung werden, jedoch werden nur app-Anzeige von wird das neue Feld, wenn eine Variable auf "true", und er festgelegt ist nur verwendet zwischenspeichern, wenn eine andere Variable festgelegt ist auf "true". Wenn Funktion ein nicht höher gestuft werden können, aber die Funktion B bereit ist, können Sie höher Stufen der gesamte Code bis hin zur Produktion mit dem Feature ein Schalter deaktiviert, und schalten Sie die Funktion B. Sie können Fertig stellen Funktion A und Stufen Sie ihn später mit keine Quelle Code zusammengeführt.

Und zwar unabhängig davon, ob Sie Verzweigungen oder schaltet für Funktionen verwenden, können eine Verzweigungsstruktur wie folgt auf eine Weise Agile- und wiederholbare Codes von der Entwicklung in die Produktion übertragen.

Diese Struktur ermöglicht Ihnen, schnell auf Feedback von Kunden zu reagieren. Wenn Sie schnelle Problembehebung bis hin zur Produktion vornehmen müssen, können Sie auch, die effizient agile-Methode verwenden. Sie können eine Verzweigung aus Master oder Staging erstellen und einrichten Master zusammenführen und nach-unten-Wenn er bereit ist in den Verzweigungen "Entwicklung" und "Funktion.

![Hotfix-Verzweigung](source-control/_static/image2.png)

Ohne eine Verzweigungsstruktur wie folgt mit der Trennung von Produktions- und Development-Branches konnte Sie ein Produktionsproblem in die Position des neuen Featurecode sowie die Korrektur Produktion höher stufen müssen einfügen. Der neuen Featurecode u. u. nicht vollständig getestet und bereit für die Produktion und möglicherweise müssen Sie viele Arbeitsaufgaben Rückgängigmachen von Änderungen, die nicht bereit sind. Oder Sie möglicherweise die Korrektur zum Testen von Änderungen, und rufen Sie sie bereit für die Bereitstellung zu verzögern.

Als Nächstes sehen Sie Beispiele für diese drei Muster in Visual Studio, Azure und Visual Studio Online zu implementieren. Diese sind Beispiele, die detaillierte, schrittweise Vorgehensweise-to--It-Anweisungen statt; Ausführliche Anweisungen, die alle erforderlichen Kontext bereitzustellen, finden Sie unter der [Ressourcen](#resources) Abschnitt am Ende des Kapitels.

<a id="vsscripts"></a>
## <a name="add-scripts-to-source-control-in-visual-studio"></a>Hinzufügen von Skripts zur quellcodeverwaltung in Visual Studio

Sie können Skripts zur quellcodeverwaltung in Visual Studio hinzufügen, indem Sie sie in einem Visual Studio-Projektmappenordner (vorausgesetzt, dass das Projekt in der quellcodeverwaltung ist) einschließen. Dies ist eine Methode zu diesem Zweck.

Erstellen Sie einen Ordner für die Skripts im Projektmappenordner (den gleichen Ordner mit Ihrem *sln* Datei).

![Automation-Ordner](source-control/_static/image3.png)

Kopieren Sie die Skriptdateien in den Ordner ein.

![Automatisierung Ordnerinhalt auflisten](source-control/_static/image4.png)

Fügen Sie in Visual Studio dem Projekt einen Projektmappenordner hinzu.

![Neuer Projektmappenordner Menüauswahl](source-control/_static/image5.png)

Und fügen Sie die Skriptdateien in den Projektmappenordner.

![Menüauswahl vorhandenes Element hinzufügen](source-control/_static/image6.png)

![Dialogfeld "Vorhandenes Element hinzufügen"](source-control/_static/image7.png)

Die Skriptdateien sind nun in Ihrem Projekt enthalten, und Datenquellen-Steuerelements ist ihre versionsänderungen zusammen mit den entsprechenden quellcodeänderungen nachverfolgen.

<a id="appsettings"></a>
## <a name="store-sensitive-data-in-azure"></a>Speichern von sensiblen Daten in Azure

Wenn Sie Ihre Anwendung auf ein Azure-Website ausführen, ist eine Möglichkeit zum Speichern von Anmeldeinformationen in der quellcodeverwaltung vermeiden sie in Azure speichern.

Beispielsweise speichert die Anwendung beheben in der Datei "Web.config" Datei zwei Verbindungszeichenfolgen, die Kennwörter in der Produktion und einen Schlüssel, der Zugriff auf Azure-Speicherkonto gewährt hat.

[!code-xml[Main](source-control/samples/sample1.xml?highlight=2-3,11)]

Wenn Ablegen von tatsächlichen Produktionswerten für diese Einstellungen in Ihrer *"Web.config"* Datei, oder wenn Sie sie, in Ablegen der *Web.Release.config* Datei so konfigurieren Sie eine Transformation für "Web.config", um sie während der Bereitstellung einfügen Sie müssen im Quell-Repository gespeichert werden. Bei der Eingabe von Datenbank-Verbindungszeichenfolgen in der Produktion eines Veröffentlichungsprofils, das Kennwort werden in Ihre *pubxml* Datei. (Sie ausschließen, die *pubxml* Datei aus der quellcodeverwaltung, aber dann verlieren Sie auch den Vorteil, dass die anderen bereitstellungseinstellungen freigeben.)

Azure bietet eine Alternative für die **"appSettings"** und Verbindungszeichenfolgen Abschnitte der *"Web.config"* Datei. Hier wird der relevante Teil der **Konfiguration** Registerkarte für eine Website im Azure-Verwaltungsportal:

!["appSettings" und ConnectionStrings im portal](source-control/_static/image8.png)

Beim Bereitstellen eines Projekts zu dieser Website und die Anwendung ausgeführt wird, überschreiben beliebige Werte, die Sie in Azure gespeichert haben, werden alle Werte in der Datei "Web.config".

Sie können diese Werte mit dem Verwaltungsportal oder die Skripts in Azure festlegen. Die Umgebung Automation Erstellungsskript Sie gesehen, in haben der [automatisieren alles](automate-everything.md) Kapitel einer Azure SQL-Datenbank erstellt, ruft der Speicher und SQL-Datenbank-Verbindungszeichenfolgen ab und speichert diese geheimen Informationen in den Einstellungen für Ihre Website.

[!code-powershell[Main](source-control/samples/sample2.ps1)]

[!code-powershell[Main](source-control/samples/sample3.ps1)]

Beachten Sie, dass die Skripts, sodass Istwerte auf das Quellrepository beibehalten abrufen nicht parametrisiert werden.

Wenn Sie sich lokal in der Entwicklungsumgebung ausführen, die app liest die lokale Datei "Web.config"-Datei und die Verbindung Zeichenfolge verweist auf eine LocalDB, SQL Server-Datenbank, in der *App\_Daten* Ordner des Webprojekts. Wenn Sie die app in Azure ausführen und die Anwendung versucht, diese Werte aus der Datei "Web.config" zu lesen, werden was er ruft zurück und verwendet die Werte für die Website nicht was tatsächlich in der Datei "Web.config" wird gespeichert.

<a id="gittfs"></a>
## <a name="use-git-in-visual-studio-and-visual-studio-online"></a>Verwenden Sie Git in Visual Studio und Visual Studio Online

Alle quellcodeverwaltungsumgebung können Sie um die Verzweigung DevOps-Struktur, die weiter oben vorgestellten zu implementieren. Für verteilte Teams eine [distributed Version Control System](http://en.wikipedia.org/wiki/Distributed_revision_control) (DVCS) möglicherweise funktionieren am besten; für andere Teams eine [zentralisierte System](http://en.wikipedia.org/wiki/Revision_control) möglicherweise besser geeignet.

[Git](http://git-scm.com/) ist ein DVCS, das sehr beliebte geworden ist. Wenn Sie Git für die quellcodeverwaltung verwenden, müssen Sie eine vollständige Kopie des Repositorys mit allen dessen Verlauf auf dem lokalen Computer. Viele bevorzugen, weil das einfacher ist weiterarbeiten, wenn Sie nicht mit dem Netzwerk verbunden – können Sie weiterhin führen Sie ein Commit und Rollbacks, erstellen und Verzweigungen wechseln und so weiter. Auch wenn Sie mit dem Netzwerk verbunden sind, ist es einfacher und schneller zum Erstellen von Verzweigungen und Verzweigungen wechseln, wenn alles lokal ist. Sie können auch auf lokalen Commit- oder Rollbackvorgängen vorgehen, ohne Auswirkungen auf andere Entwickler. Und Sie können in Batches Commits vor dem Senden an den Server.

[Microsoft Visual Studio Online](https://www.visualstudio.com/)(VSO), früher bekannt als Team Foundation-Dienst bietet sowohl Git und [Team Foundation-Versionskontrolle](https://msdn.microsoft.com/library/ms181237(v=vs.120).aspx) (TFVC; zentralisierte Steuerung der Quelle). Hier verwenden einige Teams bei Microsoft in der Azure-Gruppe zentrale Datenquellen-Steuerelements, einige verteilt, verwenden und einige eine Mischung aus (bei einigen Projekten zentralisierte und verteilt, die für andere Projekte) verwenden. VSO-Diensts ist für bis zu 5 Benutzer kostenlos. Sie können für eine kostenlose Plan registrieren [hier](https://go.microsoft.com/fwlink/?LinkId=307137).

Visual Studio 2013 umfasst integrierte erstrangige [Git Unterstützung](https://msdn.microsoft.com/library/hh850437.aspx); hier ist eine schnelle Demo, die Funktionsweise von.

Ein Projekt in Visual Studio 2013 geöffnet, Maustaste die Projektmappe in **Projektmappen-Explorer**, und wählen Sie **Projektmappe zur Quellcodeverwaltung hinzufügen**.

![Projektmappe zur Quellcodeverwaltung hinzufügen](source-control/_static/image9.png)

Visual Studio werden gefragt, ob Sie TFVC (zentrale Versionskontrolle) oder Git verwenden möchten.

![Source Control auswählen](source-control/_static/image10.png)

Wenn Sie Git auswählen, und klicken Sie auf **OK**, Visual Studio ein neues lokales Git-Repository im Projektmappenordner erstellt. Das neue Repository sind noch keine Dateien; Sie müssen diese im Repository hinzufügen, indem Sie ein Git-Commit. Mit der rechten Maustaste in der Projektmappe in **Projektmappen-Explorer**, und klicken Sie dann auf **Commit**.

![Commit](source-control/_static/image11.png)

Visual Studio automatisch die Stufen aller die Projektdateien für den Commit und listet diese in **Team Explorer** in der **eingeschlossene Änderungen** Bereich. (Wenn es einige nicht in der Commit enthalten sein sollen wurden, wählen Sie sie beispielsweise mit der rechten Maustaste, und klicken Sie auf **ausschließen**.)

![Team Explorer](source-control/_static/image12.png)

Geben Sie einen Commit Kommentar ein, und klicken Sie auf **Commit**, und Visual Studio wird das Commit ausgeführt, und zeigt die Commit-ID an.

![Team Explorer-Änderungen](source-control/_static/image13.png)

Jetzt, wenn Sie Code ändern, sodass es unterscheidet sich von den im Repository, können Sie einfach die Unterschiede anzeigen. Mit der rechten Maustaste eine Datei, die Sie geändert haben, wählen Sie **vergleichen mit Unmodified**, und Sie erhalten einen Vergleich anzeigen, die Änderung Ihrer uncommitted anzeigt.

![Mit nicht geänderten vergleichen](source-control/_static/image14.png)

![Diff mit Änderungen](source-control/_static/image15.png)

Welche Änderungen können Sie ganz leicht erkennen, nehmen und Sie sie einchecken.

Angenommen, müssen Sie eine Verzweigung –, können Sie dies in Visual Studio zu tun. In **Team Explorer**, klicken Sie auf **neue Verzweigung**.

![Team Explorer New Branch](source-control/_static/image16.png)

Geben Sie einen Namen für die Verzweigung, klicken Sie auf **Verzweigung erstellen**, und bei Auswahl **Auschecken Verzweigung**, Visual Studio automatisch auscheckt der neuen Verzweigung.

![Team Explorer New Branch](source-control/_static/image17.png)

Sie können jetzt nehmen Sie Änderungen an Dateien und in diese Verzweigung einchecken. Und Sie können problemlos zwischen Verzweigungen und Visual Studio automatisch synchronisiert, die die Dateien, je nachdem, was Sie zu verzweigen ausgecheckt haben. In diesem Beispiel wird die Webseite im Titel  *\_Layout.cshtml* wurde geändert in "Hotfix 1" in HotFix1 Verzweigung.

![Hotfix1 Verzweigung](source-control/_static/image18.png)

Wenn Sie in der Master zurückwechseln verzweigen, den Inhalt der  *\_Layout.cshtml* Datei automatisch, was sie in den hauptbranch werden zurückgesetzt.

![Hauptverzweigung](source-control/_static/image19.png)

Diese ein einfaches Beispiel, wie Sie schnell eine Verzweigung erstellen und zwischen Verzweigungen hin-und spiegeln. Dieses Feature ermöglicht einen hoch agilen Workflow verwenden die Verzweigungsstruktur und Automatisierungsskripts dargestellt wird, der [automatisieren alles](automate-everything.md) Kapitel. Sie können z. B. werden in die Development-Verzweigung arbeiten, Erstellen einer Hotfix-Verzweigung aus Master, wechseln Sie zu der neuen Verzweigung, nehmen Sie die Änderungen vorhanden und commit, und wechseln Sie zurück zur Development-Verzweigung und fortfahren, was Sie tun.

Hier gesehen ist, wie Sie mit einem lokalen Git-Repository in Visual Studio arbeiten. In einer teamumgebung mithilfe von Push übertragen Sie in der Regel auch ändert sich in einem gemeinsamen Repository. Visual Studio-Tools aktivieren Sie auf einem remote-Git-Repository zu verweisen. Sie können "github.com" zu diesem Zweck verwenden oder können Sie [Git in Visual Studio Online](https://msdn.microsoft.com/library/hh850437.aspx) mit allen anderen Visual Studio Online Funktionen wie z. B. Arbeitsaufgabe und fehlernachverfolgung integriert.

Dies ist nicht die einzige Möglichkeit, die einem agile Verzweigungsstrategie natürlich implementiert werden können. Sie können die gleichen agilen Workflow über eine zentrale Quellcodeverwaltungs-Repository aktivieren.

## <a name="summary"></a>Zusammenfassung

Messen Sie Ihr Quellcodeverwaltungssystem basierend auf wie schnell können Sie eine Änderung und rufen Sie die Anwendung in einer sicheren und kalkulierbare Weise live Erfolg. Wenn Sie selbst abschreckend feststellen, eine Änderung vorzunehmen, da ein oder zwei von manuellen Tests darauf erfordern, Fragen Sie sich selbst habe process-wise oder test-wise vornehmen, damit Sie diese Änderung in Minuten oder zu schlechteste nicht mehr als eine Stunde vornehmen können. Eine Strategie für die auf diese Weise wird zum Implementieren von continuous Integration und kontinuierlichen Bereitstellung, die wir in behandelt werden die [nächsten Kapitels](continuous-integration-and-continuous-delivery.md).

<a id="resources"></a>
## <a name="resources"></a>Ressourcen

Die [Visual Studio Online](https://www.visualstudio.com/) Portal bietet Dokumentation und Support Services und Sie können für ein Konto registrieren. Wenn Sie Visual Studio 2012 und Git verwenden möchten, finden Sie unter [Visual Studio-Tools für Git](https://visualstudiogallery.msdn.microsoft.com/abafc7d6-dcaa-40f4-8a5e-d6724bdb980c).

Weitere Informationen zu TFVC (zentrale Version Control) und Git (distributed Version Control) finden Sie unter den folgenden Ressourcen:

- [Welche Versionskontrollsystem sollten verwenden: TFVC oder Git?](https://msdn.microsoft.com/library/vstudio/ms181368.aspx#tfvc_or_git_summary) MSDN-Dokumentation enthält eine Tabelle, die die Unterschiede zwischen TFVC und Git.
- [Nun, möchte ich, dass Team Foundation Server und möchte ich, Git, aber dies ist eine bessere?](https://blogs.msdn.com/b/visualstudiouk/archive/2013/08/05/well-i-like-team-foundation-server-and-i-like-git-but-which-is-better.aspx) Vergleich von Git- und TFVC.

Weitere Informationen zu verzweigen Strategien finden Sie unter den folgenden Ressourcen:

- [Erstellen einer Releasepipeline mit Team Foundation Server 2012](https://msdn.microsoft.com/library/dn449957.aspx). Microsoft Patterns and Practices-Dokumentation. Finden Sie in Kapitel 6 Nähere Informationen zur Verzweigung Strategien. Befürworter-Funktion über die Funktion Verzweigungen schaltet und wenn Verzweigungen für Funktionen verwendet werden, Fürsprecher halten sie kurzlebige (Stunden oder Tage höchstens).
- [Version-Steuerelement Handbuch](https://aka.ms/vsarsolutions). Führen Sie zu verzweigen Strategien von ALM Rangers. Finden Sie auf der Registerkarte "Downloads" in der Verzweigung Strategies.pdf.
- [Softwareentwicklung mit dem Feature schaltet](https://msdn.microsoft.com/magazine/dn683796.aspx). MSDN Magazine-Artikel.
- [Funktion zum ein-/ausschalten](http://martinfowler.com/bliki/FeatureToggle.html). Einführung in die Funktion schaltet / Funktion kennzeichnet Fowlers-Blog.
- [Schaltet Vs Feature Verzweigungen Feature](http://geekswithblogs.net/Optikal/archive/2013/02/10/152069.aspx). Eine andere Blogbeitrag zu Feature-Schaltet von Dylan Smith.

Weitere Informationen zum Umgang mit vertraulichen Informationen, der nicht in quellcodeverwaltungsrepositorys beibehalten werden soll, finden Sie unter den folgenden Ressourcen:

- [Bewährte Methoden für die Bereitstellung von Kennwörtern und andere sensible Daten für ASP.NET und Azure App Service](../../../../identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure.md).
- [Azure-Websites: Wie Anwendungszeichenfolgen und Verbindung Zeichenfolgen Arbeit](https://azure.microsoft.com/blog/2013/07/17/windows-azure-web-sites-how-application-strings-and-connection-strings-work/). Erläutert die Azure-Funktion, die überschreibt `appSettings` und `connectionStrings` Daten in der *"Web.config"* Datei.
- [Benutzerdefinierte Einstellungen für Konfiguration und die Anwendung in Azure Web Sites - mit Stefan Schackow](https://azure.microsoft.com/documentation/videos/configuration-and-app-settings-of-azure-web-sites/).

Informationen zu weiteren Methoden, durch die Beibehaltung vertrauliche Informationen aus der quellcodeverwaltung finden Sie unter [ASP.NET MVC: behalten Sie Private Einstellungen von Datenquellen-Steuerelements](http://typecastexception.com/post/2014/04/06/ASPNET-MVC-Keep-Private-Settings-Out-of-Source-Control.aspx).

>[!div class="step-by-step"]
[Zurück](automate-everything.md)
[Weiter](continuous-integration-and-continuous-delivery.md)
