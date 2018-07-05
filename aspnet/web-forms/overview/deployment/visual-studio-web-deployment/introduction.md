---
uid: web-forms/overview/deployment/visual-studio-web-deployment/introduction
title: 'ASP.NET-webbereitstellung mithilfe von Visual Studio: Einführung | Microsoft-Dokumentation'
author: tdykstra
description: Dieser tutorialreihe erfahren Sie, wie zum Bereitstellen von web-Anwendung in Azure App Service-Web-Apps oder von Drittanbietern Hostinganbieter durch (veröffentlichen) aus einer ASP.NET-Anwendung mit V...
ms.author: aspnetcontent
ms.date: 02/15/2013
ms.assetid: 24ad086d-865e-433c-9ac9-05f1a553da16
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/introduction
msc.type: authoredcontent
ms.openlocfilehash: 11e6f6a695b4e98cf5d061ca3c3197922e971644
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2018
ms.locfileid: "37828350"
---
<a name="aspnet-web-deployment-using-visual-studio-introduction"></a>ASP.NET-webbereitstellung mithilfe von Visual Studio: Einführung
====================
durch [Tom Dykstra](https://github.com/tdykstra)

[Startprojekt herunterladen](http://go.microsoft.com/fwlink/p/?LinkId=282627)

> Dieser tutorialreihe erfahren Sie, wie zum Bereitstellen von web-Anwendung in Azure App Service-Web-Apps oder von Drittanbietern Hostinganbieter durch (veröffentlichen) aus einer ASP.NET-Anwendung mithilfe von Visual Studio 2012 mit dem Azure SDK für .NET. Die meisten der Verfahren sind ähnlich wie für Visual Studio 2013.
> 
> Sie entwickeln eine Webanwendung, um es für Personen, die über das Internet verfügbar machen. Jedoch Web Programmierung Lernprogramme in der Regel beendet wird, unmittelbar nach dem sie Sie auf Ihrem Entwicklungscomputer funktioniert abrufen angezeigt haben. In dieser tutorialreihe beginnt, in dem die anderen lassen: Sie haben eine Web-app erstellt, getestet, und es ist einsatzbereit. Ausblick In diesen Tutorials gezeigt, wie in IIS auf dem lokalen Entwicklungscomputer zum Testen und dann in Azure oder einem Drittanbieter-Hostinganbieter für Staging und Produktion zuerst bereitstellen. Die beispielanwendung, die Sie bereitstellen ist ein Webanwendungsprojekt, die das Entity Framework, SQL Server und dem ASP.NET-Mitgliedschaftssystem verwendet. Die beispielanwendung verwendet die ASP.NET Web Forms, aber die gezeigten Verfahren gelten auch für ASP.NET MVC und Web-API.
> 
> In diesen Tutorials wird davon ausgegangen, dass Sie wissen, wie mit ASP.NET Core in Visual Studio arbeiten. Falls nicht, ein guter Ausgangspunkt ist ein [ASP.NET Web Forms Lernprogramm](../../older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-1.md) oder [Lernprogramm zu ASP.NET MVC](../../../../mvc/overview/older-versions/getting-started-with-aspnet-mvc4/intro-to-aspnet-mvc-4.md).
> 
> Wenn Sie Fragen, die nicht direkt mit dem Tutorial verknüpft sind haben, können Sie sie veröffentlichen das [ASP.NET und bereitstellungsforum für](https://forums.asp.net/26.aspx/1?Configuration+and+Deployment) oder [StackOverflow](http://stackoverflow.com).
> 
> Dieser Inhalt ist auch verfügbar als ein kostenloses e-Book in [der TechNet-E-Book-Galerie](https://social.technet.microsoft.com/wiki/contents/articles/11608.e-book-gallery-for-microsoft-technologies.aspx#ASPNETWebDeploymentusingVisualStudio).


## <a name="overview"></a>Übersicht

Diese Lernprogramme führen Sie durch die Bereitstellung einer ASP.NET-Webanwendung, die SQL Server-Datenbanken enthält. Sie müssen zuerst in IIS auf dem lokalen Entwicklungscomputer zum Testen und dann in der Web-Apps in Azure App Service und Azure SQL-Datenbank für Staging und Produktion bereitstellen. Erfahren Sie, wie mithilfe von Visual Studio-One-Click-Veröffentlichung bereitstellen, und erfahren Sie, wie das Bereitstellen über die Befehlszeile.

Die Anzahl der Tutorials kann es sich um den Bereitstellungsprozess könnte entmutigend erscheinen. Die grundlegenden Verfahren sind in der Tat einfach. Allerdings in realen Situationen müssen häufig zusätzliche Bereitstellungsaufgaben ausführen – z. B. Berechtigungen für Ordner auf dem Zielserver festlegen. Wir haben einige dieser zusätzlichen Aufgaben, in der Hoffnung dargestellt, die Informationen in den Tutorials lassen nicht, die erfolgreiche Bereitstellung einer echten Anwendung verhindern könnten.

Die Lernprogramme nacheinander ausführen sollen, und jeder Teil baut auf den vorherigen Teil. Sie können Teile, die für Ihre Situation relevanten nicht überspringen, müssen dann aber möglicherweise anpassen, die Verfahren in späteren Tutorials.

## <a name="intended-audience"></a>Zielgruppe

In den Tutorials dienen ASP.NET von Entwickler, die in Umgebungen arbeiten, in denen:

- Die produktionsumgebung ist Azure App Service-Web-Apps oder einem Drittanbieter-hosting-Anbieter.
- Bereitstellung ist nicht auf eine continuous Integration-Prozesses beschränkt, aber es kann direkt aus Visual Studio ausgeführt werden.

Bereitstellung von [quellcodeverwaltung](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/source-control.md) mit einer [continuous Delivery](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/continuous-integration-and-continuous-delivery.md) Prozess wird nicht behandelt, in diesen Tutorials mit Ausnahme von einem Lernprogramm, das zeigt, wie Sie über die Befehlszeile bereitstellen. Informationen zu continuous Delivery finden Sie unter den folgenden Ressourcen:

- [Continuous Integration und Continuous Delivery (erstellen realer Cloud-Apps mit Windows Azure)](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/continuous-integration-and-continuous-delivery.md)
- [Bereitstellen einer Web-Apps in Azure App Service](https://azure.microsoft.com/documentation/articles/web-sites-deploy/)
- [Bereitstellen von Webanwendungen in Unternehmensszenarien](../deploying-web-applications-in-enterprise-scenarios/deploying-web-applications-in-enterprise-scenarios.md) (ältere Satz Lernprogramme für Visual Studio 2010, die immer nützliche Informationen für unternehmensumgebungen noch geschrieben.)

## <a name="using-a-third-party-hosting-provider"></a>Verwenden von einem Drittanbieter-hosting-Anbieter

In den Tutorials führen Sie durch den Prozess der ein Azure-Konto einrichten und Bereitstellen der Anwendung für Web-Apps in Azure App Service für Staging und Produktion. Allerdings können Sie die gleichen grundlegenden Verfahren für die Bereitstellung in einer Drittanbieter-hosting-Anbieter Ihrer Wahl aus. In den Tutorials zu Azure über eindeutige Prozessen wechseln, sie erläutern, und welche Unterschiede, die Sie, bei einem Hostanbieter von Drittanbietern erwarten können empfehlen.

## <a name="deploying-web-app-projects"></a>Bereitstellen von Web-app-Projekte

Die beispielanwendung, die Sie herunterladen und bereitstellen, die in diesen Tutorials wird ein Visual Studio-Webanwendungsprojekt. Aber wenn Sie die neueste Version installieren [Web Publish Update für Visual Studio](https://msdn.microsoft.com/tr-tr/library/jj161045.aspx), können Sie die gleichen Bereitstellungsmethoden und Tools für Web-app-Projekte.

## <a name="deploying-aspnet-mvc-projects"></a>Bereitstellen von ASP.NET MVC-Projekten

Die beispielanwendung ist ein ASP.NET Web Forms-Projekt, aber alles, was Sie erfahren, wie Sie gilt auch für ASP.NET MVC. Ein Visual Studio-MVC-Projekt ist lediglich eine andere Form der Webanwendungsprojekt. Der einzige Unterschied ist, wenn Sie bei einem Hostinganbieter bereitstellen, die ASP.NET MVC oder Ihr Zielversion nicht unterstützt, müssen Sie sicherstellen, dass Sie die entsprechende installiert haben ([MVC 3](http://nuget.org/packages/AspNetMvc/3.0.20105.0), [MVC 4](http://www.nuget.org/packages/Microsoft.AspNet.Mvc/4.0.30506.0) oder [MVC 5](http://www.nuget.org/packages/Microsoft.AspNet.Mvc)) NuGet-Paket in Ihrem Projekt.

## <a name="programming-language"></a>Programmiersprache

Die beispielanwendung verwendet c# aber die Tutorials erfordern keine Kenntnisse von c# und Bereitstellungstechnik angezeigt, indem Sie die Lernprogramme sind nicht sprachspezifische.

## <a name="database-deployment-methods"></a>Datenbank-Bereitstellungsmethoden

Es gibt drei Möglichkeiten, die Sie zusammen in Visual Studio-webbereitstellung mit SQL Server-Datenbank bereitstellen können:

- Entity Framework Code First-Migrationen
- Der DbDacFx Web Deploy-Anbieter
- Die DbFullSql Web Deploy-Anbieter

In diesem Tutorial verwenden Sie die ersten beiden dieser Methoden. Die DbFullSql Web Deploy-Anbieter ist eine ältere Methode, die mit Ausnahme von einige spezifischen Szenarien, z. B. Migrieren von SQL Server Compact in SQL Server nicht mehr empfohlen wird.

Die in diesem Tutorial gezeigten Methoden sind für SQL Server-Datenbanken keine SQL Server Compact. Informationen zum Bereitstellen einer SQL Server Compact-Datenbank finden Sie unter [Visual Studio Web-Bereitstellung mit SQL Server Compact](../../older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-introduction-1-of-12.md).

Die Methoden, die in diesem Tutorial gezeigt erfordern die Verwendung der Web Deploy-Veröffentlichungsmethode. Wenn Sie lieber eine andere Methode, z. B. FTP, Dateisystem oder FPSE veröffentlichen, finden Sie unter [Bereitstellen einer Datenbank getrennt von der Bereitstellung von Webanwendungen](https://go.microsoft.com/fwlink/p/?LinkId=282413#databaseseparate) in den Einstieg in die Webbereitstellung für Visual Studio und ASP.NET.

### <a name="entity-framework-code-first-migrations"></a>Entity Framework Code First-Migrationen

In Entity Framework, Version 4.3 führte Microsoft Code First-Migrationen. Code First-Migrationen automatisiert die inkrementellen Änderungen zu einem Datenmodell und das Weitergeben von diesen Änderungen an der Datenbank. In früheren Versionen von Code First können Sie in der Regel das Entity Framework, löschen und Neuerstellen der Datenbank jedes Mal, die Sie das Datenmodell ändern. Dies ist ein Problem bei der Entwicklung nicht, da Testdaten einfach neu erstellt, aber in der Produktion in der Regel das Datenbankschema zu aktualisieren, ohne Löschen der Datenbank soll. Das Migrationsfeature ermöglicht Code First die Datenbank zu aktualisieren, ohne Sie zu löschen und neu zu erstellen. Sie können Code First automatisch entscheiden, wie Sie die erforderlichen schemaänderungen vorzunehmen, oder Sie Code schreiben, der die Änderungen angepasst. Eine Einführung in Code First-Migrationen, finden Sie unter [Code First-Migrationen](https://msdn.microsoft.com/library/hh770484.aspx).

Wenn Sie ein Webprojekt bereitstellen, können Visual Studio automatisieren den Prozess der Bereitstellung einer Datenbank, die von Code First-Migrationen verwaltet wird. Wenn Sie das Veröffentlichungsprofil erstellen, wählen Sie ein Kontrollkästchen mit der Bezeichnung Code First-Migrationen ausführen (wird beim Anwendungsstart ausgeführt). Diese Einstellung bewirkt, dass der Prozess der automatisch der Web.config-Datei der Anwendung auf dem Zielserver so konfigurieren, dass Code First verwendet die `MigrateDatabaseToLatestVersion` datenbankinitialisiererklasse.

Visual Studio führt keine Aktion mit der Datenbank während der Bereitstellung aus. Wenn die bereitgestellte Anwendung zum ersten Mal nach der Bereitstellung auf die Datenbank zugreift, Code First automatisch wird die Datenbank erstellt oder aktualisiert das Datenbankschema auf die neueste Version. Wenn die Anwendung eine Migrationen Seed-Methode implementiert wird, führt die Methode auf, nachdem die Datenbank wird erstellt, oder das Schema wird aktualisiert.

In diesem Tutorial verwenden Sie Code First-Migrationen zum Bereitstellen der Anwendungsdatenbank.

### <a name="the-dbdacfx-web-deploy-provider"></a>Der DbDacFx Web Deploy-Anbieter

Für eine SQL Server-Datenbank, die nicht von Entity Framework Code First verwaltet wird, können Sie ein Kontrollkästchen auswählen, das Aktualisieren einer Datenbank bezeichnet wird, wenn Sie das Veröffentlichungsprofil konfigurieren. Während der ersten Bereitstellung erstellt der DbDacFx-Anbieter Tabellen und andere Datenbankobjekte in die Zieldatenbank entsprechend die Quelldatenbank an. Bei nachfolgenden Bereitstellungen bestimmt der Anbieter, was zwischen den Datenbanken Quell- und Zielserver unterscheidet, und aktualisiert das Schema der Zieldatenbank entsprechend die Quelldatenbank. Standardmäßig wird nicht der Anbieter keine Änderungen vornehmen, die dazu führen, dass Daten verloren gehen, z. B. wenn eine Tabelle oder Spalte gelöscht wird.

Diese Methode ist nicht automatisiert die Bereitstellung von Daten in Datenbanktabellen, aber Sie können Skripts ausführen, und Konfigurieren von Visual Studio, um sie auszuführen, während der Bereitstellung erstellen. Ein weiterer Grund zum Ausführen von Skripts während der Bereitstellung werden schemaänderungen vornehmen, die automatisch durchgeführt werden kann, da sie zu Datenverlust führen würde.

In diesem Tutorial verwenden Sie den DbDacFx-Anbieter zum Bereitstellen der ASP.NET-Mitgliedschaftsdatenbank.

## <a name="troubleshooting-during-this-tutorial"></a>In diesem Tutorial zur Problembehandlung

Wenn während der Bereitstellung ein Fehler auftritt, oder die bereitgestellte Website nicht ordnungsgemäß ausgeführt wird, Bereitstellen nicht die Fehlermeldungen immer eine offensichtliche Lösung. Lassen sich mit einigen gängigen Problemszenarien eine [Verweis Problembehandlungsseite](troubleshooting.md) verfügbar ist. Wenn Sie eine Fehlermeldung erhalten, oder etwas funktioniert nicht, wie Sie die Tutorials durchgehen, achten Sie darauf, dass Sie auf der Seite "Problembehandlung" überprüfen.

## <a name="comments-welcome"></a>Kommentare freuen.

Kommentare zu den Lernprogrammen sind Willkommen, und bei der Aktualisierung des Tutorials wird bemüht versucht, berücksichtigt der Konto-Korrekturen oder Vorschläge für Verbesserungen, die im Tutorial Kommentare bereitgestellt werden.

<a id="prerequisites"></a>

## <a name="prerequisites"></a>Erforderliche Komponenten

In diesem Tutorial wurde für die folgenden Produkte geschrieben:

- Windows 8 oder Windows 7.
- Visual Studio 2012 oder Visual Studio-2012 Express für Web mit [das neueste Update](https://go.microsoft.com/fwlink/?LinkId=272486).
- [Azure SDK für Visual Studio 2012](https://go.microsoft.com/fwlink/?LinkId=254364)

Sie können das Tutorial mit Visual Studio 2010 SP1 oder Visual Studio 2013, aber einige Screenshots unterscheiden und einige Funktionen unterscheiden.

Wenn Sie Visual Studio 2013 verwenden, installieren Sie [Azure SDK für Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkID=324322).

Wenn Sie Visual Studio 2010 SP1 verwenden, installieren Sie die folgende Software:

- [Azure SDK für Visual Studio 2010](https://go.microsoft.com/fwlink/?LinkID=254269)
- [SQL Server Express LocalDB](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=SQLLocalDBOnly_11_0)
- [SQL Server-Datentools](https://msdn.microsoft.com/library/hh500335.aspx).

Je nachdem, wie viele der SDK-Abhängigkeiten Sie bereits auf Ihrem Computer verfügen kann das Installieren des Azure SDK längere Zeit von einigen Minuten bis zu einer halben Stunde oder länger dauern. Sie benötigen das Azure SDK, auch wenn Sie planen, mit einem Drittanbieter-Hostinganbieter anstelle von in Azure veröffentlichen, da das SDK die neuesten Updates für Visual Studio Web umfasst Funktionen zu veröffentlichen.

> [!NOTE]
> In diesem Tutorial wurde mit dem Azure SDK-Version 1.8.1 geschrieben. Seit damals wurden die neuere Versionen mit zusätzlichen Funktionen zur Verfügung gestellt. Die Lernprogramme wurden aktualisiert, um ganz zu schweigen diese Funktionen und Links zu Ressourcen, die Weitere Informationen dazu zu erhalten.


Die Anweisungen und Screenshots basieren auf Windows 8, aber in den Tutorials werden die Unterschiede für Windows 7 erläutert.

Eine andere Software ist erforderlich, um das Tutorial abzuschließen, aber Sie müssen keine, die noch nicht installiert haben. Das Tutorial führt Sie durch die Schritte für die Installation bei Bedarf.

## <a name="download-the-sample-application"></a>Herunterladen der beispielanwendung

Die Anwendung, die Sie bereitstellen, wird mit dem Namen Contoso University und wurde bereits für Sie erstellt. Dies ist eine vereinfachte Version einer Universität-Website, die anhand der Contoso University-Anwendung, die in beschriebenen lose der [-Lernprogramme für Entity Framework auf der ASP.NET-Website](https://asp.net/entity-framework/tutorials).

Wenn Sie die erforderlichen Komponenten installiert haben, laden die [Webanwendung der Contoso University](https://go.microsoft.com/fwlink/p/?LinkId=282627). Die *ZIP* -Datei enthält mehrere Versionen des Projekts. Um durch die Schritte des Tutorials zu arbeiten, beginnen Sie mit dem Projekt im Ordner "c#". Um anzuzeigen, wie das Projekt am Ende der Lernprogramme aussieht, öffnen Sie das Projekt im Ordner "ContosoUniversity-End".

Um das Projekt für die Verwendung durch die Schritte des Tutorials zu vorzubereiten, führen Sie die folgenden Schritte aus:

1. Speichern Sie die Projektmappendateien ContosoUniversity aus dem C#-Ordner in einem Ordner namens ContosoUniversity in den Ordner, die Sie für die Arbeit mit Visual Studio-Projekten verwenden.

    Standardmäßig ist dies für Visual Studio 2012 den folgenden Ordner:

    `C:\Users\<username>\Documents\Visual Studio 2012\Projects`

    (Für die Screenshots in diesem Tutorial, den Ordner des Projekts befindet sich im Stammverzeichnis auf dem `C`: Laufwerk.)
2. Starten Sie Visual Studio, und öffnen Sie das Projekt.
3. In **Projektmappen-Explorer**mit der rechten Maustaste auf die Projektmappe, und klicken Sie auf **EnableNuGet Paketwiederherstellung**.
4. Erstellen Sie die Projektmappe.
5. Wenn Sie Kompilierungsfehler erhalten, manuell Wiederherstellen der NuGet-Pakete:

    1. In **Projektmappen-Explorer**mit der rechten Maustaste auf die Projektmappe, und klicken Sie dann auf **NuGet-Pakete für Projektmappe verwalten**.
    2. Am oberen Rand der **NuGet-Pakete verwalten** Dialogfeld Sie sehen **einige NuGet-Pakete sind von dieser Lösung fehlt. Klicken Sie auf diese Option, um wiederherzustellen.** Klicken Sie auf die **wiederherstellen** Schaltfläche.
    3. Generieren Sie die Projektmappe neu.
6. Drücken Sie STRG + F5, um die Anwendung auszuführen.

    Die Anwendung wird mit der Contoso University-Startseite geöffnet.

    ![Auf der Startseite Dev](introduction/_static/image1.png)

    (Es gibt möglicherweise eine Wartezeit beim Start von Visual Studio die SQL Server Express LocalDB-Instanz, und erhalten Sie möglicherweise einen Timeoutfehler Wenn, die zu lange dauert. In diesem Fall einfach starten Sie das Projekt erneut.)

Die Websiteseiten stehen in der Menüleiste, und können Sie die folgenden Funktionen ausführen:

- Anzeigen von studentenstatistiken (die Seite "Info").
- Zeigen Sie an, bearbeiten Sie, löschen Sie und fügen Sie der Schüler/Studenten hinzu.
- Anzeigen und Bearbeiten von Kursen.
- Anzeigen und Bearbeiten von Dozenten.
- Anzeigen und Bearbeiten von Abteilungen.

Folgenden finden Sie Screenshots der einige repräsentative Seiten.

![Schüler/Studenten Seite Dev](introduction/_static/image2.png)

![Entwicklung für Schüler/Studenten-Seite hinzufügen](introduction/_static/image3.png)

## <a name="review-application-features-that-affect-deployment"></a>Überprüfen Sie die Features für die Anwendung, die Bereitstellung auswirken.

Die folgenden Funktionen von der Anwendung beeinträchtigen, zur Bereitstellung oder was Sie tun, um die sie bereitstellen müssen. Jedes davon wird in den folgenden Tutorials der Reihe ausführlicher erläutert.

- Contoso University verwendet eine SQL Server-Datenbank zum Speichern von Anwendungsdaten, z. B. "Student" und "Instructor". Die Datenbank enthält eine Mischung von Test- und Produktionsdaten, und wenn Sie in der produktionsumgebung bereitstellen, müssen Sie die Testdaten ausschließen.
- Die Anwendung verwendet, das ASP.NET-Mitgliedschaftssystem, die Benutzerkontoinformationen in SQL Server-Datenbank gespeichert wird. Die Anwendung definiert einen Benutzer mit Administratorrechten, wer Zugriff auf eingeschränkte Informationen hat. Sie müssen die Mitgliedschaftsdatenbank ohne Testkonten, aber mit einem Administratorkonto bereitstellen.
- Die Anwendung verwendet einen Drittanbieter-Fehler, Protokollierung und berichterstellung Hilfsprogramm. Dieses Hilfsprogramm wird in einer Assembly bereitgestellt, die mit der Anwendung bereitgestellt werden muss.
- Das Dienstprogramm zum Protokollieren der Fehler Schreibt Fehlerinformationen in XML-Dateien in einem Dateiordner. Sie müssen sicherstellen, dass das Konto, das unter ASP.NET ausgeführt, auf der bereitgestellten Website wird über die Schreibberechtigung für diesen Ordner, und Sie diesen Ordner von der Bereitstellung ausgeschlossen müssen. (Andernfalls Fehler Protokolldaten aus der testumgebung für die Produktion bereitgestellt werden können und/oder Produktion Fehlerprotokolldateien können gelöscht werden.)
- Die Anwendung enthält einige Einstellungen, die geändert werden kann, müssen in der bereitgestellten *"Web.config"* Datei abhängig von der zielumgebung (Test, Staging oder Produktion) und andere Einstellungen, die abhängig von der Build geändert werden müssen Konfiguration (Debug oder Release).
- Visual Studio-Projektmappe enthält ein Klassenbibliotheksprojekt. Nur die Assembly, die von diesem Projekt generiert bereitgestellt werden sollen, nicht auf das Projekt selbst.

## <a name="summary"></a>Zusammenfassung

In diesem ersten Tutorial der Reihe, die Sie das Visual Studio-Beispielprojekt heruntergeladen haben und überprüft die Websitefunktionen, die beeinflussen, wie Sie die Anwendung bereitstellen. In den folgenden Tutorials vorbereitet Sie für die Bereitstellung durch einrichten, dass einige dieser Dinge automatisch behandelt werden. Andere kümmert Sie manuell.

> [!div class="step-by-step"]
> [Nächste](preparing-databases.md)
