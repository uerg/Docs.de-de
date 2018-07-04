---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-introduction-1-of-12
title: 'Bereitstellen einer ASP.NET-Webanwendung mit SQL Server Compact mit Visual Studio: Einführung in – 1 von 12 | Microsoft-Dokumentation'
author: tdykstra
description: In dieser tutorialreihe erfahren Sie, wie zum Bereitstellen einer ASP.NET-Anwendung (veröffentlichen) Webanwendungsprojekt, die eine SQL Server Compact-Datenbank enthält, mithilfe von Visual Stu...
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/17/2011
ms.topic: article
ms.assetid: a2d7f33b-8c4a-4b48-9fb1-9139cf9b9878
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-introduction-1-of-12
msc.type: authoredcontent
ms.openlocfilehash: 4a790fa72568caafdb2fab5efd9f334919c23719
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/03/2018
ms.locfileid: "37398270"
---
<a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-introduction---1-of-12"></a>Bereitstellen einer ASP.NET-Webanwendung mit SQL Server Compact mit Visual Studio: Einführung in – 1 von 12
====================
durch [Tom Dykstra](https://github.com/tdykstra)

[Startprojekt herunterladen](http://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> In dieser tutorialreihe erfahren Sie, wie zum Bereitstellen einer ASP.NET-Anwendung (veröffentlichen) Webanwendungsprojekt, das eine SQL Server Compact-Datenbank mithilfe von Visual Studio 2012 RC oder Visual Studio Express 2012 RC für Web enthält. Sie können auch Visual Studio 2010 verwenden, wenn Sie die Web Publish Update installieren.
> 
> Ein Lernprogramm, das zeigt, Bereitstellungsfunktionen, die nach der RC-Version von Visual Studio 2012 eingeführt wurden, zeigt, wie zum Bereitstellen von SQL Server-Editionen als SQL Server Compact und zeigt, wie Sie in Azure App Service-Web-Apps bereitstellen, finden Sie unter [ASP.NET-webbereitstellung Mithilfe von Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md).
> 
> Diese Lernprogramme begleiten Sie zuerst IIS auf dem lokalen Entwicklungscomputer zum Testen, und dann zu einem Hostinganbieter von Drittanbietern bereitstellen. Die Anwendung, die Sie bereitstellen werden einer-Datenbank und einer ASP.NET-Mitgliedschaftsdatenbank verwendet. Beginnen Sie mithilfe von SQL Server Compact und SQL Server Compact bereitstellen und späteren Tutorials wird veranschaulicht, wie Sie Änderungen in der Datenbank bereitstellen und Migrieren zu SQL Server.
> 
> In den Tutorials wird davon ausgegangen, dass Sie wissen, wie mit ASP.NET Core in Visual Studio arbeiten. Falls nicht, ein guter Ausgangspunkt ist ein [ASP.NET Web Forms Lernprogramm](../tailspin-spyworks/tailspin-spyworks-part-1.md) oder [Lernprogramm zu ASP.NET MVC](../../../../mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md).
> 
> Wenn Sie Fragen, die nicht direkt mit dem Tutorial verknüpft sind haben, können Sie sie veröffentlichen das [ASP.NET bereitstellungsforum](https://forums.asp.net/26.aspx/1?Configuration+and+Deployment).


## <a name="overview"></a>Übersicht

Diese Lernprogramme begleiten Sie zuerst IIS auf dem lokalen Entwicklungscomputer zum Testen, und dann zu einem Hostinganbieter von Drittanbietern bereitstellen. Die Anwendung, die Sie bereitstellen werden einer-Datenbank und einer ASP.NET-Mitgliedschaftsdatenbank verwendet. Beginnen Sie mithilfe von SQL Server Compact und SQL Server Compact bereitstellen und späteren Tutorials wird veranschaulicht, wie Sie Änderungen in der Datenbank bereitstellen und Migrieren zu SQL Server.

Die Anzahl der Lernprogramme – 11 in allen plus der Problembehandlungsseite – möglicherweise ist die Bereitstellung könnte entmutigend erscheinen. Die grundlegenden Verfahren für die Bereitstellung von einem Standort besteht in der Tat einen relativ kleinen Teil der tutorialreihe. In realen Situationen benötigen Sie jedoch häufig Informationen über zusätzliche kleine, jedoch wichtige Aspekte der Bereitstellung – Festlegen von Ordnerberechtigungen z. B. auf dem Zielserver. Wir haben viele zusätzliche Verfahren in den Tutorials mit der Hoffnung, die Informationen in den Tutorials, die erfolgreiche Bereitstellung einer echten Anwendung verhindern möglicherweise lassen nicht einbezogen.

Die Lernprogramme nacheinander ausführen sollen, und jeder Teil baut auf den vorherigen Teil. Sie können jedoch Teile überspringen, die für Ihre Situation nicht relevant. (Überspringen die Teile müssen Sie möglicherweise die Verfahren in späteren Tutorials passen.)

## <a name="intended-audience"></a>Zielgruppe

In den Tutorials dienen ASP.NET von Entwickler, die in kleinen Unternehmen oder in anderen Umgebungen arbeiten, in denen:

- Ein continuous Integration-Prozesses (automatisierte Builds und -Bereitstellung) wird nicht verwendet.
- Die produktionsumgebung ist eine Drittanbieter-hosting-Anbieter.
- Eine Person füllt in der Regel mehrere Rollen (die gleiche Person entwickelt, getestet und bereitgestellt).

In unternehmensumgebungen ausgeführt werden es kommt häufiger vor, um continuous Integrationsprozesse zu implementieren und die produktionsumgebung in der Regel von den Servern des Unternehmens gehostet wird. Auch in der Regel führen Sie verschiedene Personen verschiedene Rollen. Weitere Informationen zu Enterprise-Bereitstellung, finden Sie unter [Bereitstellen von Webanwendungen in Unternehmensszenarien](../../deployment/deploying-web-applications-in-enterprise-scenarios/deploying-web-applications-in-enterprise-scenarios.md).

Organisationen aller Größen können auch Webanwendungen in Azure bereitstellen, und die meisten der in diesen Lernprogrammen vorgestellten Verfahren gelten auch für Azure App Services-Web-Apps. Eine Einführung in Azure, finden Sie unter [ https://azure.microsoft.com ](https://azure.microsoft.com).

## <a name="the-hosting-provider-shown-in-the-tutorials"></a>Der Hostinganbieter, die in den Tutorials gezeigt

In den Tutorials führen Sie durch die das Einrichten eines Kontos mit einem Hostinganbieter und Bereitstellen der Anwendung an den hosting-Anbieter. Einem bestimmten Hostinganbieter wurde ausgewählt, sodass in den Tutorials veranschaulichen, der alle Funktionen nutzen, der Bereitstellung in einer live-Website konnte. Jede Hostingunternehmen verfügt über verschiedene Funktionen, und die benutzerfreundlichkeit der Bereitstellung von auf ihren Servern variiert ein wenig; in diesem Tutorial beschriebene Prozess ist jedoch meist für den gesamten Prozess.

Der hosting-Anbieter verwendet, die für dieses Tutorial Cytanium.com, ist einer von vielen, die verfügbar sind stellt, und seine Verwendung in diesem Lernprogramm keine Befürwortung oder Empfehlung.

## <a name="deploying-web-site-projects"></a>Bereitstellen von Websiteprojekten

Contoso University ist ein Visual Studio-Webanwendungsprojekt. Die meisten der Bereitstellungsmethoden und Tools, die in diesem Tutorial gezeigten gelten nicht für [Websiteprojekte](https://msdn.microsoft.com/library/dd547590.aspx). Informationen zum Bereitstellen von Website-Projekten finden Sie unter [ASP.NET die ASP.NET-Bereitstellung](https://msdn.microsoft.com/library/bb386521.aspx#deployment_for_web_site_projects).

## <a name="deploying-aspnet-mvc-projects"></a>Bereitstellen von ASP.NET MVC-Projekte

Für dieses Tutorial, die Sie Bereitstellen einer ASP.NET Web Forms-Projekt, aber alles, was Sie erfahren, wie Sie gilt auch für ASP.NET MVC. Ein Visual Studio-MVC-Projekt ist lediglich eine andere Form der Webanwendungsprojekt. Der einzige Unterschied ist, wenn Sie bei einem Hostinganbieter bereitstellen, die ASP.NET MVC oder Ihr Zielversion nicht unterstützt, müssen Sie sicherstellen, dass Sie die entsprechende installiert haben ([MVC 3](http://nuget.org/packages/AspNetMvc/3.0.20105.0) oder [MVC 4](http://nuget.org/packages/aspnetmvc)) NuGet-Paket in Ihrem Projekt.

## <a name="programming-language"></a>Programmiersprache

Die beispielanwendung verwendet c# aber die Tutorials erfordern keine Kenntnisse von c# und Bereitstellungstechnik angezeigt, indem Sie die Lernprogramme sind nicht sprachspezifische.

## <a name="troubleshooting-during-this-tutorial"></a>In diesem Tutorial zur Problembehandlung

Stellen Sie eine Lösung, wenn ein Fehler, während der Bereitstellung auftritt, oder wenn die bereitgestellte Website nicht ordnungsgemäß ausgeführt wird, nicht immer die Fehlermeldungen. Lassen sich mit einigen gängigen Problemszenarien eine [Verweis Problembehandlungsseite](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md) verfügbar ist. Wenn Sie eine Fehlermeldung erhalten, oder etwas funktioniert nicht, wie Sie die Tutorials durchgehen, achten Sie darauf, dass Sie auf der Seite "Problembehandlung" überprüfen.

## <a name="comments-welcome"></a>Kommentare Willkommen

Kommentare zu den Lernprogrammen sind Willkommen, und bei der Aktualisierung des Tutorials wird bemüht versucht, berücksichtigt der Konto-Korrekturen oder Vorschläge für Verbesserungen, die im Tutorial Kommentare bereitgestellt werden.

## <a name="prerequisites"></a>Erforderliche Komponenten

Bevor Sie beginnen, stellen Sie sicher, dass Sie Windows 7 oder höher und eine der folgenden Produkte auf Ihrem Computer installiert:

- [Visual Studio 2010 SP1](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack)
- [Visual Web Developer Express 2010 SP1](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VWD2010SP1Pack)
- [Visual Studio 2012 RC oder Visual Studio Express 2012 RC für Web](https://go.microsoft.com/fwlink/?LinkId=240162)

Wenn Sie Visual Studio 2010 SP1 oder Visual Web Developer Express 2010 SP1 verfügen, installieren Sie auch die folgenden Produkte:

- [Azure SDK für .NET (VS 2010 SP1)](https://go.microsoft.com/fwlink/?LinkID=208120) (einschließlich der Web Publish Update)
- [Microsoft Visual Studio 2010 SP1 Tools für SQLServer Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCEVSTools)

Eine andere Software ist erforderlich, um das Tutorial abzuschließen, aber Sie müssen keine, die noch geladen haben. Das Tutorial führt Sie durch die Schritte für die Installation bei Bedarf.

## <a name="downloading-the-sample-application"></a>Die Beispielanwendung herunterladen

Die Anwendung, die Sie bereitstellen heißt Contoso University und bereits für Sie erstellt wurde. Dies ist eine vereinfachte Version einer Universität-Website, die anhand der Contoso University-Anwendung, die in beschriebenen lose der [-Lernprogramme für Entity Framework auf der ASP.NET-Website](https://asp.net/entity-framework/tutorials).

Wenn Sie die erforderlichen Komponenten installiert haben, laden die [Webanwendung der Contoso University](https://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b). Die *ZIP* -Datei enthält mehrere Versionen des Projekts und eine PDF-Datei, die alle 12 Tutorials enthält. Um die Schritte des Tutorials zu arbeiten, beginnen Sie mit ContosoUniversity – Anfang. Um anzuzeigen, wie das Projekt am Ende der Lernprogramme aussieht, öffnen Sie ContosoUniversity-End. Um anzuzeigen, wie das Projekt vor der Migration auf die Vollversion von SQL Server in Lernprogramm 10 aussieht, öffnen Sie ContosoUniversity-AfterTutorial09.

Speichern Sie zur Vorbereitung auf den Schritten des Tutorials durcharbeiten, ContosoUniversity-Begin, auf den Ordner, die Sie für die Arbeit mit Visual Studio-Projekten verwenden. Standardmäßig ist dies im folgenden Ordner:

`C:\Users\<username>\Documents\Visual Studio 2012\Projects`

(Für die Screenshots in diesem Tutorial, den Ordner des Projekts befindet sich im Stammverzeichnis auf dem `C`: Laufwerk.)

Starten Sie Visual Studio, öffnen Sie das Projekt, und drücken Sie STRG + F5, um es auszuführen.

[![Home_Page](deployment-to-a-hosting-provider-introduction-1-of-12/_static/image2.png)](deployment-to-a-hosting-provider-introduction-1-of-12/_static/image1.png)

Die Websiteseiten stehen in der Menüleiste, und können Sie die folgenden Funktionen ausführen:

- Anzeigen von studentenstatistiken (die Seite "Info").
- Zeigen Sie an, bearbeiten Sie, löschen Sie und fügen Sie der Schüler/Studenten hinzu.
- Anzeigen und Bearbeiten von Kursen.
- Anzeigen und Bearbeiten von Dozenten.
- Anzeigen und Bearbeiten von Abteilungen.

Folgenden finden Sie Screenshots der einige repräsentative Seiten.

[![Students_Page](deployment-to-a-hosting-provider-introduction-1-of-12/_static/image4.png)](deployment-to-a-hosting-provider-introduction-1-of-12/_static/image3.png)

[![Add_Students_Page](deployment-to-a-hosting-provider-introduction-1-of-12/_static/image6.png)](deployment-to-a-hosting-provider-introduction-1-of-12/_static/image5.png)

## <a name="reviewing-application-features-that-affect-deployment"></a>Überprüfen von Anwendungsfunktionen, die Bereitstellung auswirken.

Die folgenden Funktionen von der Anwendung beeinträchtigen, zur Bereitstellung oder was Sie tun, um die sie bereitstellen müssen. Jedes davon wird in den folgenden Tutorials der Reihe ausführlicher erläutert.

- Contoso University verwendet eine SQL Server Compact-Datenbank zum Speichern von Anwendungsdaten, z. B. "Student" und "Instructor". Die Datenbank enthält eine Mischung von Test- und Produktionsdaten, und wenn Sie in der produktionsumgebung bereitstellen, müssen Sie die Testdaten ausschließen. Weiter unten in der tutorialreihe werden Sie von SQL Server Compact in SQL Server migrieren.
- Die Anwendung verwendet, das ASP.NET-Mitgliedschaftssystem, die Benutzerkontoinformationen in einer SQL Server Compact-Datenbank gespeichert wird. Die Anwendung definiert einen Benutzer mit Administratorrechten, wer Zugriff auf eingeschränkte Informationen hat. Sie müssen die Mitgliedschaftsdatenbank ohne Testkonten, aber mit einem Administratorkonto bereitstellen.
- Da die Datenbank und die Mitgliedschaftsdatenbank als die Datenbank-Engine SQL Server Compact verwenden, müssen Sie die Datenbank-Engine für den Hostinganbieter sowie die Datenbanken selbst bereitstellen.
- Die Anwendung verwendet ASP.NET universal Mitgliedschaftsanbieter, damit das Mitgliedschaftssystem seine Daten in einer SQL Server Compact-Datenbank speichern kann. Die Assembly, die die universelle Gruppenmitgliedschaft-Anbieter enthält, muss mit der Anwendung bereitgestellt werden.
- Die Anwendung verwendet Entity Framework 5.0, den Zugriff auf Daten in der Datenbank. Die Assembly mit Entity Framework 5.0 muss mit der Anwendung bereitgestellt werden.
- Die Anwendung verwendet einen Drittanbieter-Fehler, Protokollierung und berichterstellung Hilfsprogramm. Dieses Hilfsprogramm wird in einer Assembly bereitgestellt, die mit der Anwendung bereitgestellt werden muss.
- Das Dienstprogramm zum Protokollieren der Fehler Schreibt Fehlerinformationen in XML-Dateien in einem Dateiordner. Sie müssen sicherstellen, dass das Konto, das unter ASP.NET ausgeführt, auf der bereitgestellten Website wird über die Schreibberechtigung für diesen Ordner, und Sie diesen Ordner von der Bereitstellung ausgeschlossen müssen. (Andernfalls Fehler Protokolldaten aus der testumgebung für die Produktion bereitgestellt werden können und/oder Produktion Fehlerprotokolldateien können gelöscht werden.)
- Die Anwendung enthält einige Einstellungen, die geändert werden kann, müssen in der bereitgestellten *"Web.config"* Datei abhängig von der zielumgebung (Test oder Produktion) und andere Einstellungen, die abhängig von der Build geändert werden müssen Konfiguration (Debug oder Release).
- Visual Studio-Projektmappe enthält ein Klassenbibliotheksprojekt. Nur die Assembly, die von diesem Projekt generiert bereitgestellt werden sollen, nicht auf das Projekt selbst.

In diesem ersten Tutorial der Reihe, die Sie das Visual Studio-Beispielprojekt heruntergeladen haben und überprüft die Websitefunktionen, die beeinflussen, wie Sie die Anwendung bereitstellen. In den folgenden Tutorials vorbereitet Sie für die Bereitstellung durch einrichten, dass einige dieser Dinge automatisch behandelt werden. Andere kümmert Sie manuell.

> [!div class="step-by-step"]
> [Nächste](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12.md)
