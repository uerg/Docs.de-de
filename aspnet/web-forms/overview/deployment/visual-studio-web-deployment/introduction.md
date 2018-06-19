---
uid: web-forms/overview/deployment/visual-studio-web-deployment/introduction
title: 'ASP.NET Web-Bereitstellung mit Visual Studio: Einführung | Microsoft Docs'
author: tdykstra
description: Diese Reihe von Lernprogrammen wird gezeigt, wie bereitstellen web-Anwendung in Azure App Service-Web-Apps oder Drittanbieter-Hostinganbieter durch (veröffentlichen) aus einer ASP.NET-Anwendung mithilfe von V...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/15/2013
ms.topic: article
ms.assetid: 24ad086d-865e-433c-9ac9-05f1a553da16
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/introduction
msc.type: authoredcontent
ms.openlocfilehash: 1ff4cc3b0fa6ce7e6cdc833a0c2f7fea2050c4e6
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
ms.locfileid: "30890683"
---
<a name="aspnet-web-deployment-using-visual-studio-introduction"></a>ASP.NET Web-Bereitstellung mit Visual Studio: Einführung
====================
durch [Tom Dykstra](https://github.com/tdykstra)

[Startprojekt herunterladen](http://go.microsoft.com/fwlink/p/?LinkId=282627)

> Diese Reihe von Lernprogrammen wird gezeigt, wie bereitstellen web-Anwendung in Azure App Service-Web-Apps oder Drittanbieter-Hostinganbieter durch (veröffentlichen) aus einer ASP.NET-Anwendung mithilfe von Visual Studio 2012 mit dem Azure SDK für .NET. Die meisten Vorgehensweisen sind für Visual Studio 2013 ähnlich.
> 
> Sie entwickeln eine Webanwendung, damit er für Personen, die über das Internet verfügbar zu machen. Jedoch Web programming Lernprogramme in der Regel beendet, wenn unmittelbar nach der sie etwas arbeiten auf Ihrem Entwicklungscomputer abrufen angezeigt haben. Diese Reihe von Lernprogrammen beginnt, in dem die anderen deaktiviert lassen: Sie haben eine Web-app erstellt, getestet, und es kann losgehen. Ausblick Diese Lernprogramme veranschaulichen das zuerst mit IIS auf Ihrem lokalen Computer zum Testen und dann in Azure oder eine Drittanbieter-hosting-Anbieter für das Staging und Produktion bereitstellen. Die beispielanwendung, die Sie die Bereitstellung ist ein Webanwendungsprojekt, die das Entity Framework, SQL Server und dem ASP.NET-Mitgliedschaftssystem verwendet. Die beispielanwendung verwendet die ASP.NET Web Forms, aber beschriebenen Verfahren gelten auch für ASP.NET MVC und Web-API.
> 
> Diese Lernprogramme setzen voraus, dass Sie wissen, dass zum Arbeiten mit ASP.NET in Visual Studio. Wenn Sie dies nicht tun, ist eine gute Möglichkeit zum eine [Lernprogramm zu ASP.NET Web Forms-Grundlagen](../../older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-1.md) oder ein [ASP.NET MVC-Lernprogramm](../../../../mvc/overview/older-versions/getting-started-with-aspnet-mvc4/intro-to-aspnet-mvc-4.md).
> 
> Wenn Sie Fragen, die nicht direkt mit dem Lernprogramm verknüpft sind haben, bereitstellen können, die [ASP.NET bereitstellungsforum](https://forums.asp.net/26.aspx/1?Configuration+and+Deployment) oder [StackOverflow](http://stackoverflow.com).
> 
> Dieser Inhalt ist auch verfügbar als kostenlose e-Book in [der TechNet-E-Book-Galerie](https://social.technet.microsoft.com/wiki/contents/articles/11608.e-book-gallery-for-microsoft-technologies.aspx#ASPNETWebDeploymentusingVisualStudio).


## <a name="overview"></a>Übersicht

Diese Lernprogramme begleiten Sie durch Bereitstellen einer ASP.NET-WEBANWENDUNG zwischen, die SQL Server-Datenbanken enthält. Sie müssen zuerst IIS auf Ihrem lokalen Computer zum Testen und dann auf Web-Apps in Azure App Service und Azure SQL-Datenbank für Staging und Produktion bereitstellen. Sehen Sie, wie Sie mithilfe von Visual Studio One-Click-Veröffentlichung bereitstellen, und sehen Sie, wie Sie mithilfe der Befehlszeile bereitstellen.

Die Anzahl der Lernprogramme ggf. den Bereitstellungsprozess entmutigend stellen. In der Tat sind die grundlegenden Verfahren einfach. Allerdings in realen Situationen häufig müssen Sie zusätzliche Aufgaben ausgeführt werden – z. B. Berechtigungen für Ordner festlegen, auf dem Zielserver. Wir haben einige dieser zusätzlichen Aufgaben, in der Hoffnung, dass veranschaulicht die Lernprogramme Informationen auslassen nicht, die Sie für die erfolgreiche Bereitstellung einer realen Anwendung verhindern könnten.

Die Lernprogramme werden nacheinander ausgeführt, und jeder Teil baut auf den vorherigen Teil. Sie können fortfahren, Teile, die für Ihre Situation nicht relevant sind, müssen dann aber möglicherweise, passen Sie die Verfahren in späteren Lernprogrammen.

## <a name="intended-audience"></a>Beabsichtigte Zielgruppe

Die Lernprogramme ASP.NET-Entwickler, die in Umgebungen arbeiten sollen, in denen:

- Die produktionsumgebung ist Azure App Service-Web-Apps oder einem Drittanbieter-Hostinganbieter.
- Bereitstellung ist nicht an einen Prozess für die fortlaufende Integration begrenzt jedoch möglicherweise direkt in Visual Studio vorgenommen werden.

Bereitstellung von [quellcodeverwaltung](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/source-control.md) mithilfe einer [fortlaufende Übermittlung](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/continuous-integration-and-continuous-delivery.md) Prozess fällt nicht in diesen Lernprogrammen außer ein Lernprogramm, das zeigt, wie Sie über die Befehlszeile bereitstellen. Informationen zur kontinuierlichen Bereitstellung finden Sie unter den folgenden Ressourcen:

- [Fortlaufende Integration und kontinuierlichen Bereitstellung (Building Real-World Cloud-Apps mit Windows Azure)](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/continuous-integration-and-continuous-delivery.md)
- [Bereitstellen einer Web-app in Azure App Service](https://azure.microsoft.com/documentation/articles/web-sites-deploy/)
- [Bereitstellen von Webanwendungen in Enterprise-Szenarios](../deploying-web-applications-in-enterprise-scenarios/deploying-web-applications-in-enterprise-scenarios.md) (einen älteren Satz Lernprogramme, die für Visual Studio 2010, die nützliche Informationen zum Enterprise-Umgebungen wird noch geschrieben.)

## <a name="using-a-third-party-hosting-provider"></a>Verwenden einen Drittanbieter-hosting-Anbieter

Die Lernprogramme gelangen Sie durch den Prozess ein Azure-Konto einrichten und Bereitstellen der Anwendung zu Web-Apps in Azure App Service für das Staging und Produktion. Allerdings können Sie die gleichen grundlegenden Verfahren verwenden, für die Bereitstellung von einem Drittanbieter-Hostinganbieter Ihrer Wahl. Die Lernprogramme zu Azure über Prozesse, die eindeutig gelangen, sie erläutert werden, und erläutern welche Unterschiede, die Sie bei einem Hostinganbieter eines Drittanbieters erwarten können.

## <a name="deploying-web-app-projects"></a>Bereitstellen von Web-app-Projekte

Die beispielanwendung, die Sie herunterladen und bereitstellen, die für diese Lernprogramme ist eine Visual Studio-Webanwendungsprojekt. Allerdings bei der Installation der neuesten [Web veröffentlichen Update für Visual Studio](https://msdn.microsoft.com/tr-tr/library/jj161045.aspx), können Sie die gleichen Bereitstellungsmethoden und Tools für die Web-app-Projekte.

## <a name="deploying-aspnet-mvc-projects"></a>Bereitstellen von ASP.NET MVC-Projekte

Die beispielanwendung ist eine ASP.NET Web Forms-Projekt, aber alles Sie erfahren, wie Sie gilt auch für ASP.NET MVC. Ein Visual Studio-MVC-Projekt wird gerade eine andere Form der Webanwendungsprojekt. Der einzige Unterschied ist, dass wenn Sie mit einem Hostinganbieter bereitstellen, die ASP.NET MVC oder die Zielversion des Zertifikats nicht unterstützt, Sie müssen sicherstellen, dass Sie die entsprechende installiert haben ([MVC 3](http://nuget.org/packages/AspNetMvc/3.0.20105.0), [MVC 4](http://www.nuget.org/packages/Microsoft.AspNet.Mvc/4.0.30506.0) oder [MVC 5](http://www.nuget.org/packages/Microsoft.AspNet.Mvc)) NuGet-Paket im Projekt.

## <a name="programming-language"></a>Programmiersprache

Die beispielanwendung verwendet c# jedoch die Lernprogramme erfordern keine Kenntnisse über C#- und Verfahren zur anwendungsbereitstellung angezeigt, indem Sie die Lernprogramme sind nicht sprachspezifisch.

## <a name="database-deployment-methods"></a>Datenbank-Bereitstellungsmethoden

Es gibt drei Möglichkeiten, eine SQL Server-Datenbank zusammen mit der webbereitstellung in Visual Studio bereitgestellt werden kann:

- Entity Framework Code First-Migrationen
- Der DbDacFx Web Deploy-Anbieter
- DbFullSql Web Deploy-Anbieter

In diesem Lernprogramm verwenden Sie die ersten beiden dieser Methoden. DbFullSql Web Deploy-Anbieter ist ein legacy-Methode, die mit Ausnahme von einigen bestimmten Szenarien, z. B. Migrieren von SQL Server Compact mit SQL Server nicht mehr empfohlen wird.

In diesem Lernprogramm erläuterten Methoden sind für SQL Server-Datenbanken, die nicht SQL Server Compact. Informationen zum Bereitstellen einer SQL Server Compact-Datenbank finden Sie unter [Visual Studio Web Bereitstellung mit SQL Server Compact](../../older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-introduction-1-of-12.md).

In diesem Lernprogramm erläuterten Methoden erfordern die Verwendung von der Web Deploy Veröffentlichungsmethode. Wenn Sie lieber eine andere Methode, z. B. FTP-, Dateisystem oder FPSE veröffentlichen finden Sie unter [Bereitstellen einer Datenbank getrennt von der Bereitstellung von Webanwendungen](https://go.microsoft.com/fwlink/p/?LinkId=282413#databaseseparate) in der Web Content Bereitstellungskarte für Visual Studio und ASP.NET.

### <a name="entity-framework-code-first-migrations"></a>Entity Framework Code First-Migrationen

In Entity Framework, Version 4.3 führte Microsoft Code First-Migrationen. Code First-Migrationen automatisiert den Erstellungsprozess von inkrementellen Änderungen mit einem Datenmodell und das Weitergeben von diesen Änderungen an der Datenbank. In früheren Versionen von Code First können Sie in der Regel das Entity Framework löschen und Neuerstellen der Datenbank bei jeder Änderung des Datenmodells. Dies ist kein Problem in der Entwicklung, da aus Testdaten einfach neu erstellt besteht, aber in der Produktion in der Regel das Datenbankschema zu aktualisieren, ohne Löschen der Datenbank werden möchten. Migrationen dieser Funktion können Code First zum Aktualisieren der Datenbank ohne gelöscht und neu erstellt wird. Lassen Sie Code First automatisch entscheiden, wie Sie die erforderlichen schemaänderungen vorzunehmen, oder Sie können Code schreiben, der die Änderungen passt. Eine Einführung in die Code First-Migrationen werden soll, finden Sie unter [Code First-Migrationen](https://msdn.microsoft.com/library/hh770484.aspx).

Wenn Sie ein Webprojekt bereitstellen, können Visual Studio automatisieren, den Prozess der Bereitstellung einer Datenbank, die von Code First-Migrationen verwaltet wird. Wenn Sie das Veröffentlichungsprofil erstellen, wählen Sie ein Kontrollkästchen mit der Bezeichnung führen Sie Code First-Migrationen (wird beim Anwendungsstart ausgeführt). Diese Einstellung bewirkt, dass der Bereitstellungsprozess automatisch der Anwendungsdatei "Web.config" auf dem Zielserver so konfigurieren, dass Code First verwendet die `MigrateDatabaseToLatestVersion` Initialisiererklasse.

Visual Studio ist nicht mit der Datenbank während des Bereitstellungsvorgangs keine Wirkung. Wenn die bereitgestellte Anwendung zum ersten Mal nach der Bereitstellung auf die Datenbank zugreift, Code First automatisch erstellt die Datenbank oder das Datenbankschema auf die neueste Version aktualisiert. Wenn die Anwendung eine Migrationen Seed-Methode implementiert, wird die Methode ausgeführt, nachdem die Datenbank wird erstellt das Schema aktualisiert.

In diesem Lernprogramm verwenden Sie Code First-Migrationen zum Bereitstellen der Anwendungsdatenbank.

### <a name="the-dbdacfx-web-deploy-provider"></a>Der DbDacFx Web Deploy-Anbieter

Für eine SQL Server-Datenbank, die nicht vom Entity Framework Code First verwaltet wird, können Sie ein Kontrollkästchen auswählen, mit der Bezeichnung Datenbank aktualisieren, wenn Sie das Veröffentlichungsprofil konfigurieren. Während der ersten Bereitstellung erstellt der DbDacFx-Anbieter Tabellen und andere Datenbankobjekte in der Zieldatenbank entsprechend die Quelldatenbank an. Bei nachfolgenden Bereitstellungen der Anbieter bestimmt, was sich zwischen den Quell- und Zielschemas Datenbanken unterscheidet, und das Schema der Zieldatenbank entsprechend die Quelldatenbank aktualisiert. Standardmäßig wird nicht der Anbieter Änderungen vornehmen, die dazu führen, dass Daten verloren gehen, z. B. wenn eine Tabelle oder Spalte gelöscht wird.

Diese Methode ist nicht Automatisieren der Bereitstellung von Daten in Datenbanktabellen, aber Sie können zu Skripts erstellen, ausführen und Konfigurieren von Visual Studio sie während der Bereitstellung ausgeführt. Ein weiterer Grund für die Skripts während der Bereitstellung ausgeführt wird, schemaänderungen vorzunehmen, die automatisch durchgeführt werden kann, da sie zu Datenverlust führen würde.

In diesem Lernprogramm verwenden Sie den DbDacFx-Anbieter die ASP.NET-Mitgliedschaftsdatenbank bereitstellen.

## <a name="troubleshooting-during-this-tutorial"></a>Problembehandlung im Verlauf dieses Lernprogramms

Wenn ein Fehler tritt auf, während der Bereitstellung, oder wenn die bereitgestellte Website nicht ordnungsgemäß ausgeführt wird, nicht immer die Fehlermeldungen eine offensichtliche Lösung zur Verfügung. Lassen sich mit einigen allgemeinen Problemszenarios, einer [Problembehandlung Referenzseite](troubleshooting.md) verfügbar ist. Wenn Sie eine Fehlermeldung erhalten, oder etwas funktioniert nicht, wenn Sie die Lernprogramme absolvieren, achten Sie darauf, dass Sie auf der Seite "Problembehandlung" zu überprüfen.

## <a name="comments-welcome"></a>Willkommen beim Kommentare

Kommentare zu den Lernprogrammen sind Willkommen, und bei der Aktualisierung des Lernprogramms bemüht vorgenommen werden berücksichtigt Konto Korrekturen oder Vorschläge für Verbesserungen, die im Lernprogramm Kommentare bereitgestellt werden.

<a id="prerequisites"></a>

## <a name="prerequisites"></a>Erforderliche Komponenten

Dieses Lernprogramm wurde für die folgenden Produkte geschrieben:

- Windows 8 oder Windows 7.
- Visual Studio 2012 oder Visual Studio-2012 Express für das Web mit [das neueste Update](https://go.microsoft.com/fwlink/?LinkId=272486).
- [Azure SDK für Visual Studio 2012](https://go.microsoft.com/fwlink/?LinkId=254364)

Führen Sie das Lernprogramm mithilfe von Visual Studio 2010 SP1 oder Visual Studio 2013, aber einige Screenshots unterscheiden und einige Funktionen unterscheiden.

Wenn Sie Visual Studio 2013 verwenden, installieren Sie [Azure SDK für Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkID=324322).

Wenn Sie Visual Studio 2010 SP1 verwenden, installieren Sie die folgende Software:

- [Azure SDK für Visual Studio 2010](https://go.microsoft.com/fwlink/?LinkID=254269)
- [SQL Server Express LocalDB](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=SQLLocalDBOnly_11_0)
- [SQL Server Datatools](https://msdn.microsoft.com/library/hh500335.aspx).

Je nach Anzahl der SDK-Abhängigkeiten bereits auf dem Computer konnte das Azure SDK installieren, von einigen Minuten bis zu eine halbe Stunde oder länger dauert. Sie benötigen das Azure SDK, selbst wenn Sie planen, auf Drittanbieter-hosting-Anbieter anstelle von Azure zu veröffentlichen, da das SDK umfasst die neueste Updates für Visual Studio Web veröffentlichen Funktionen.

> [!NOTE]
> Dieses Lernprogramm wurde mit dem Azure SDK-Version 1.8.1 geschrieben. Seitdem wurden neuere Versionen mit zusätzlichen Funktionen zur Verfügung gestellt. Die Lernprogramme wurden aktualisiert, sodass Erwähnen Sie diese Funktionen und Links zu Ressourcen, die Weitere Informationen zu diesen haben.


Die Anweisungen und Screenshots basieren auf Windows 8, aber die Lernprogramme erläutern die Unterschiede für Windows 7.

Andere Software ist erforderlich, um das Lernprogramm abgeschlossen haben, jedoch nicht enthalten, die noch nicht installiert sind. Das Lernprogramm führt Sie durch die Schritte für die sie bei Bedarf installieren.

## <a name="download-the-sample-application"></a>Laden Sie die beispielanwendung

Die Anwendung, die Sie die Bereitstellung ist mit dem Namen Contoso University und wurde bereits für Sie erstellt. Es ist eine vereinfachte Version einer Website Universität, lose anhand der University Contoso-Anwendung beschrieben, die der [Entity Framework-Lernprogramme auf der ASP.NET-Website](https://asp.net/entity-framework/tutorials).

Wenn Sie die erforderlichen Komponenten installiert haben, laden die [Contoso University Webanwendung](https://go.microsoft.com/fwlink/p/?LinkId=282627). Die *ZIP* -Datei enthält mehrere Versionen des Projekts. Um die Schritte des Lernprogramms zu arbeiten, starten Sie mit dem Projekt den Ordner "c#". Um anzuzeigen, wie das Projekt am Ende der Lernprogramme aussieht, öffnen Sie das Projekt im Ordner "ContosoUniversity-End".

Um das Projekt für die Arbeit durch die Schritte des Lernprogramms vorzubereiten, führen Sie die folgenden Schritte aus:

1. Speichern Sie die Projektmappendateien ContosoUniversity aus dem C#-Ordner in einem Ordner namens ContosoUniversity in den Ordner, die Sie für die Arbeit mit Visual Studio-Projekte verwenden.

    Standardmäßig ist dies für Visual Studio 2012 den folgenden Ordner:

    `C:\Users\<username>\Documents\Visual Studio 2012\Projects`

    (Für die Screenshots in diesem Lernprogramm, der Projektordner befindet sich im Stammverzeichnis auf den `C`: Laufwerk.)
2. Starten Sie Visual Studio, und öffnen Sie das Projekt.
3. In **Projektmappen-Explorer**mit der rechten Maustaste auf die Projektmappe, und klicken Sie auf **EnableNuGet Paketwiederherstellung**.
4. Erstellen Sie die Projektmappe.
5. Wenn Sie Kompilierungsfehler erhalten haben, stellen Sie manuell die NuGet-Pakete:

    1. In **Projektmappen-Explorer**mit der rechten Maustaste auf die Projektmappe, und klicken Sie dann auf **NuGet-Pakete für Projektmappe verwalten**.
    2. Am oberen Rand der **NuGet-Pakete verwalten** Dialogfeld Sie sehen **einige NuGet-Pakete fehlen in dieser Lösung. Klicken Sie auf diese Option, um wiederherzustellen.** Klicken Sie auf die **wiederherstellen** Schaltfläche.
    3. Generieren Sie die Projektmappe neu.
6. Drücken Sie STRG + F5, um die Anwendung auszuführen.

    Die Anwendung, die zur Contoso-University-Startseite wird geöffnet.

    ![Auf der Startseite Dev](introduction/_static/image1.png)

    (Es gibt möglicherweise eine Wartezeit beim Start von Visual Studio die SQL Server Express LocalDB-Instanz, und Sie erhalten möglicherweise einen Timeoutfehler Wenn, die zu lange dauert. In diesem Fall nur starten Sie das Projekt erneut.)

Die Webseiten Tastenkombinationen sind in der Menüleiste und können Sie die folgenden Funktionen ausführen:

- Student-Statistiken (Seite "Info") angezeigt.
- Anzeigen, bearbeiten, löschen und Studenten hinzufügen.
- Anzeigen und Bearbeiten von Kurse.
- Anzeigen und Bearbeiten von Lehrkräfte.
- Anzeigen und Bearbeiten von Abteilungen.

Screenshots der Seiten, die einige repräsentative Es folgen.

![Studenten Seite Dev](introduction/_static/image2.png)

![Studenten Seite Dev hinzufügen](introduction/_static/image3.png)

## <a name="review-application-features-that-affect-deployment"></a>Überprüfen Sie die Anwendungsfunktionen, die Bereitstellung beeinflussen

Auswirkungen auf die folgenden Funktionen der Anwendung, zur Bereitstellung oder was Sie tun können, um die Bereitstellung haben. Jedes davon wird in den folgenden Lernprogrammen in der Reihe ausführlicher erläutert.

- Contoso University verwendet eine SQL Server-Datenbank zum Speichern von Anwendungsdaten wie Student "und" Instructor-Namen. Die Datenbank enthält eine Mischung von Testdaten und die Produktionsdaten und während der Bereitstellung bis hin zur Produktion, müssen Sie die Testdaten ausschließen.
- Die Anwendung verwendet die ASP.NET-Mitgliedschaftssystem, die Benutzerkontoinformationen in einer SQL Server-Datenbank gespeichert. Die Anwendung definiert ein Administrator mit Zugriff auf eingeschränkte Informationen. Sie müssen zum Bereitstellen der Mitgliedschaftsdatenbank ohne Testkonten, aber mit einem lokalen Administratorkonto an.
- Die Anwendung verwendet einen Drittanbieter-Fehler, Protokollierung und-berichterstellung Hilfsprogramm. Dieses Dienstprogramm wird in einer Assembly bereitgestellt, das mit der Anwendung bereitgestellt werden müssen.
- Das Dienstprogramm zum Protokollieren der Fehler Schreibt Fehlerinformationen in XML-Dateien in einem Dateiordner. Sie müssen sicherstellen, dass das Konto, das unter ASP.NET ausgeführt, in der bereitgestellten Website wird ist die Schreibberechtigung für diesen Ordner, und Sie diesen Ordner aus der Bereitstellung ausgeschlossen. (Andernfalls Fehler Protokolldaten aus der testumgebung möglicherweise bis hin zur Produktion bereitgestellt werden und/oder Produktion Fehlerprotokolldateien möglicherweise gelöscht.)
- Die Anwendung enthält einige Einstellungen, die geändert werden müssen, die in der bereitgestellten *"Web.config"* Datei abhängig von der zielumgebung (Test-, Staging- oder produktionsumgebung) und andere Einstellungen, die abhängig von der Build geändert werden müssen Konfiguration (Debug oder Release).
- Visual Studio-Projektmappe enthält ein Klassenbibliotheksprojekt. Nur die Assembly, die diesem Projekt generierten bereitgestellt werden soll, nicht auf das Projekt selbst.

## <a name="summary"></a>Zusammenfassung

In diesem ersten Lernprogramm in der Reihe haben Sie das Beispielprojekt für Visual Studio heruntergeladen und überprüft Websitefunktionen, die beeinflussen, wie Sie die Anwendung bereitstellen. In den folgenden Lernprogrammen vorbereiten Sie für die Bereitstellung durch das einrichten, dass einige der folgenden Schritte automatisch behandelt werden. Andere kümmern Sie manuell.

> [!div class="step-by-step"]
> [Nächste](preparing-databases.md)
