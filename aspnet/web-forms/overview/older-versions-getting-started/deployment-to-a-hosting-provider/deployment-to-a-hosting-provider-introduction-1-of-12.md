---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-introduction-1-of-12
title: "Bereitstellen einer ASP.NET-Webanwendung mit SQL Server Compact mit Visual Studio: Einführung - 1, 12 | Microsoft Docs"
author: tdykstra
description: "Diese Reihe von Lernprogrammen wird gezeigt, wie das Bereitstellen einer ASP.NET-Anwendung (veröffentlichen) Webanwendungsprojekt, die eine SQL Server Compact-Datenbank enthält, mithilfe von Visual das..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/17/2011
ms.topic: article
ms.assetid: a2d7f33b-8c4a-4b48-9fb1-9139cf9b9878
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-introduction-1-of-12
msc.type: authoredcontent
ms.openlocfilehash: a0f38c83bd9231dbd37d3d505c90316af521b336
ms.sourcegitcommit: 016f4d58663bcd442930227022de23fb3abee0b3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/12/2018
---
<a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-introduction---1-of-12"></a>Bereitstellen einer ASP.NET-Webanwendung mit SQL Server Compact mit Visual Studio: Einführung - 1, 12
====================
Durch [Tom Dykstra](https://github.com/tdykstra)

[Startprojekt herunterladen](http://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> Diese Reihe von Lernprogrammen wird gezeigt, wie das Bereitstellen einer ASP.NET-Anwendung (veröffentlichen) Webanwendungsprojekt, die eine SQL Server Compact-Datenbank mithilfe von Visual Studio 2012 RC oder Visual Studio Express 2012 RC für das Web enthält. Sie können auch Visual Studio 2010 verwenden, wenn Sie das Web veröffentlichen Update installieren.
> 
> Ein Lernprogramm, die die Bereitstellungsfeatures nach der RC-Version von Visual Studio 2012 eingeführt wurde, wird gezeigt, wie SQL Server-Editionen als SQL Server Compact bereitstellen, und zeigt die Vorgehensweise beim Bereitstellen in Azure App Service-Web-Apps, finden Sie unter [ASP.NET Web Deploy Mithilfe von Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md).
> 
> Diese Lernprogramme begleiten Sie zunächst IIS auf Ihrem lokalen Computer zum Testen und dann mit einem Drittanbieter-Hostinganbieter bereitstellen. Die Anwendung, die Sie die Bereitstellung verwendet eine dienstanwendungs-Datenbank und einer ASP.NET-Mitgliedschaftsdatenbank. Beginnen Sie mithilfe von SQL Server Compact und Bereitstellung für SQL Server Compact und höher Tutorials erfahren Sie, wie Änderungen an der Datenbank bereitstellen und Migrieren zu SQL Server.
> 
> Die Lernprogramme setzen voraus, dass Sie wissen, dass zum Arbeiten mit ASP.NET in Visual Studio. Wenn Sie dies nicht tun, ist eine gute Möglichkeit zum eine [Lernprogramm zu ASP.NET Web Forms-Grundlagen](../tailspin-spyworks/tailspin-spyworks-part-1.md) oder ein [ASP.NET MVC-Lernprogramm](../../../../mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md).
> 
> Wenn Sie Fragen, die nicht direkt mit dem Lernprogramm verknüpft sind haben, bereitstellen können, die [ASP.NET bereitstellungsforum](https://forums.asp.net/26.aspx/1?Configuration+and+Deployment).


## <a name="overview"></a>Übersicht

Diese Lernprogramme begleiten Sie zunächst IIS auf Ihrem lokalen Computer zum Testen und dann mit einem Drittanbieter-Hostinganbieter bereitstellen. Die Anwendung, die Sie die Bereitstellung verwendet eine dienstanwendungs-Datenbank und einer ASP.NET-Mitgliedschaftsdatenbank. Beginnen Sie mithilfe von SQL Server Compact und Bereitstellung für SQL Server Compact und höher Tutorials erfahren Sie, wie Änderungen an der Datenbank bereitstellen und Migrieren zu SQL Server.

Die Anzahl der Lernprogramme – 11 in allen plus eine Problembehandlung Seite – möglicherweise stellen Sie den Bereitstellungsprozess entmutigend. In der Tat bilden die grundlegenden Verfahren für die Bereitstellung von einem Standort eine relativ kleine Teil des Lernprogramms. Allerdings in realen Situationen häufig benötigen Sie Informationen über zusätzliche kleine, jedoch wichtige Aspekte der Bereitstellung – z. B. Berechtigungen für Ordner festlegen, auf dem Zielserver. Wir haben viele dieser zusätzlichen Techniken in den Lernprogrammen, die Möglichkeit zur eingefügt, die die Lernprogramme Informationen auslassen nicht, die Sie für die erfolgreiche Bereitstellung einer realen Anwendung verhindern könnten.

Die Lernprogramme werden nacheinander ausgeführt, und jeder Teil baut auf den vorherigen Teil. Sie können jedoch Teile überspringen, die für Ihre Situation nicht relevant sind. (Überspringen Teile müssen Sie möglicherweise die Prozeduren in späteren Lernprogrammen passen.)

## <a name="intended-audience"></a>Beabsichtigte Zielgruppe

Die Lernprogramme ASP.NET-Entwickler, die kleinen Organisationen oder anderen Umgebungen arbeiten sollen, in denen:

- Ein fortlaufende Integration-Prozess (automatisierte Builds und Bereitstellung) wird nicht verwendet.
- Die produktionsumgebung wird ein Drittanbieter-Hostinganbieter.
- Eine Person füllt in der Regel mehrere Rollen (dieselbe Person entwickelt, getestet und bereitgestellt).

In unternehmensumgebungen sowie mehr in der Regel die fortlaufende Integration Prozesse implementiert wird, und die produktionsumgebung in der Regel von den Servern des Unternehmens gehostet wird. Auch normalerweise führen verschiedene Personen unterschiedliche Rollen. Informationen zum Enterprise-Bereitstellung finden Sie unter [Bereitstellen von Webanwendungen in Enterprise-Szenarios](../../deployment/deploying-web-applications-in-enterprise-scenarios/deploying-web-applications-in-enterprise-scenarios.md).

Organisationen können auch Webanwendungen in Azure bereitstellen, und die meisten der in diesen Lernprogrammen vorgestellten Verfahren gelten auch für Azure-App Services-Web-Apps. Eine Einführung in Azure finden Sie unter [https://azure.microsoft.com](https://azure.microsoft.com).

## <a name="the-hosting-provider-shown-in-the-tutorials"></a>In den Lernprogrammen vorgestellten Hostinganbieter

Die Lernprogramme gelangen Sie durch das Einrichten eines Kontos mit einem Hostinganbieter und Bereitstellen der Anwendung für diese Hostinganbieter. Einem bestimmten Hostingunternehmen wurde gewählt, damit die Lernprogramme zu veranschaulichen die vollständige Benutzeroberfläche für eine live-Website bereitgestellt konnte. Jede Hostingunternehmen bietet verschiedene Funktionen und der die Erfahrung der Bereitstellung auf ihren Servern in einem gewissen; Allerdings ist das Verfahren in diesem Lernprogramm beschrieben typisch für das generelle Verfahren.

Der hosting-Anbieter verwendet für dieses Lernprogramm Cytanium.com, ist eins von vielen, die verfügbar sind, und seine Verwendung in diesem Lernprogramm stellt keine Befürwortung oder Empfehlung dar.

## <a name="deploying-web-site-projects"></a>Bereitstellen von Website-Projekten

Contoso University ist eine Visual Studio-Webanwendungsprojekt. Die meisten der Bereitstellungsmethoden und Tools, die in diesem Lernprogramm demonstriert gelten nicht für [Websiteprojekte](https://msdn.microsoft.com/library/dd547590.aspx). Informationen zum Bereitstellen von Website-Projekten finden Sie unter [ASP.NET Deployment Content Map](https://msdn.microsoft.com/library/bb386521.aspx#deployment_for_web_site_projects).

## <a name="deploying-aspnet-mvc-projects"></a>Bereitstellen von ASP.NET MVC-Projekte

Für dieses Lernprogramm Sie ein ASP.NET Web Forms-Projekt bereitstellen, aber alles Sie erfahren, wie Sie gilt auch für ASP.NET MVC. Ein Visual Studio-MVC-Projekt wird gerade eine andere Form der Webanwendungsprojekt. Der einzige Unterschied ist, dass wenn Sie mit einem Hostinganbieter bereitstellen, die ASP.NET MVC oder die Zielversion des Zertifikats nicht unterstützt, Sie müssen sicherstellen, dass Sie die entsprechende installiert haben ([MVC 3](http://nuget.org/packages/AspNetMvc/3.0.20105.0) oder [MVC 4](http://nuget.org/packages/aspnetmvc)) NuGet-Paket im Projekt.

## <a name="programming-language"></a>Programmiersprache

Die beispielanwendung verwendet c# jedoch die Lernprogramme erfordern keine Kenntnisse über C#- und Verfahren zur anwendungsbereitstellung angezeigt, indem Sie die Lernprogramme sind nicht sprachspezifisch.

## <a name="troubleshooting-during-this-tutorial"></a>Problembehandlung im Verlauf dieses Lernprogramms

Wenn ein Fehler tritt auf, während der Bereitstellung, oder wenn die bereitgestellte Website nicht ordnungsgemäß ausgeführt wird, nicht immer die Fehlermeldungen Lösung zur Verfügung. Lassen sich mit einigen allgemeinen Problemszenarios, einer [Problembehandlung Referenzseite](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md) verfügbar ist. Wenn Sie eine Fehlermeldung erhalten, oder etwas funktioniert nicht, wenn Sie die Lernprogramme absolvieren, achten Sie darauf, dass Sie auf der Seite "Problembehandlung" zu überprüfen.

## <a name="comments-welcome"></a>Kommentare Willkommen

Kommentare zu den Lernprogrammen sind Willkommen, und bei der Aktualisierung des Lernprogramms bemüht vorgenommen werden berücksichtigt Konto Korrekturen oder Vorschläge für Verbesserungen, die im Lernprogramm Kommentare bereitgestellt werden.

## <a name="prerequisites"></a>Erforderliche Komponenten

Bevor Sie beginnen, stellen Sie sicher, dass Sie Windows 7 oder höher verfügen und eines der folgenden Produkte auf dem Computer installiert:

- [Visual Studio 2010 SP1](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack)
- [Visual Web Developer Express 2010 SP1](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VWD2010SP1Pack)
- [Visual Studio 2012 RC oder Visual Studio Express 2012 RC für das Web](https://go.microsoft.com/fwlink/?LinkId=240162)

Wenn Sie Visual Studio 2010 SP1 oder Visual Web Developer Express 2010 SP1 verfügen, installieren Sie auch die folgenden Produkte:

- [Azure SDK für .NET (VS 2010 SP1)](https://go.microsoft.com/fwlink/?LinkID=208120) (einschließlich der Aktualisierung der Web veröffentlichen)
- [Microsoft Visual Studio 2010 SP1 Tools for SQLServer Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCEVSTools)

Andere Software ist erforderlich, um das Lernprogramm abgeschlossen haben, jedoch nicht enthalten sind, die noch geladen. Das Lernprogramm führt Sie durch die Schritte für die sie bei Bedarf installieren.

## <a name="downloading-the-sample-application"></a>Herunterladen der Beispielanwendung

Die Anwendung, die Sie die Bereitstellung ist mit dem Namen Contoso University und wurde bereits für Sie erstellt. Es ist eine vereinfachte Version einer Website Universität, lose anhand der University Contoso-Anwendung beschrieben, die der [Entity Framework-Lernprogramme auf der ASP.NET-Website](https://asp.net/entity-framework/tutorials).

Wenn Sie die erforderlichen Komponenten installiert haben, laden die [Contoso University Webanwendung](https://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b). Die *ZIP* -Datei enthält mehrere Versionen des Projekts und PDF-Datei, die alle 12 Lernprogramme enthält. Wenn durch die Schritte des Lernprogramms arbeiten möchten, beginnen Sie mit ContosoUniversity beginnen. Öffnen Sie zum Anzeigen des Projekts am Ende der Lernprogramme illustriert ContosoUniversity-End. Öffnen Sie ContosoUniversity AfterTutorial09 aus, um anzuzeigen, wie das Projekt vor der Migration auf die Vollversion von SQL Server im Lernprogramm 10 aussieht.

Speichern Sie ContosoUniversity Begin zum Vorbereiten der durch die Schritte des Lernprogramms zu arbeiten, auf den Ordner, die Sie für die Arbeit mit Visual Studio-Projekte verwenden. Standardmäßig ist dies die folgenden Ordner:

`C:\Users\<username>\Documents\Visual Studio 2012\Projects`

(Für die Screenshots in diesem Lernprogramm, der Projektordner befindet sich im Stammverzeichnis auf den `C`: Laufwerk.)

Starten Sie Visual Studio, öffnen Sie das Projekt, und drücken Sie STRG + F5, um es auszuführen.

[![Home_page](deployment-to-a-hosting-provider-introduction-1-of-12/_static/image2.png)](deployment-to-a-hosting-provider-introduction-1-of-12/_static/image1.png)

Die Webseiten Tastenkombinationen sind in der Menüleiste und können Sie die folgenden Funktionen ausführen:

- Student-Statistiken (Seite "Info") angezeigt.
- Anzeigen, bearbeiten, löschen und Studenten hinzufügen.
- Anzeigen und Bearbeiten von Kurse.
- Anzeigen und Bearbeiten von Lehrkräfte.
- Anzeigen und Bearbeiten von Abteilungen.

Screenshots der Seiten, die einige repräsentative Es folgen.

[![Students_Page](deployment-to-a-hosting-provider-introduction-1-of-12/_static/image4.png)](deployment-to-a-hosting-provider-introduction-1-of-12/_static/image3.png)

[![Add_Students_Page](deployment-to-a-hosting-provider-introduction-1-of-12/_static/image6.png)](deployment-to-a-hosting-provider-introduction-1-of-12/_static/image5.png)

## <a name="reviewing-application-features-that-affect-deployment"></a>Überprüfen von Anwendungsfunktionen, die Bereitstellung auswirken.

Auswirkungen auf die folgenden Funktionen der Anwendung, zur Bereitstellung oder was Sie tun können, um die Bereitstellung haben. Jedes davon wird in den folgenden Lernprogrammen in der Reihe ausführlicher erläutert.

- Contoso University verwendet eine SQL Server Compact-Datenbank zum Speichern von Anwendungsdaten wie Student "und" Instructor-Namen. Die Datenbank enthält eine Mischung von Testdaten und die Produktionsdaten und während der Bereitstellung bis hin zur Produktion, müssen Sie die Testdaten ausschließen. Weiter unten in der Reihe von Lernprogrammen werden Sie von SQL Server Compact zu SQL Server migrieren.
- Die Anwendung verwendet die ASP.NET-Mitgliedschaftssystem, die Benutzerkontoinformationen in einer SQL Server Compact-Datenbank gespeichert. Die Anwendung definiert ein Administrator mit Zugriff auf eingeschränkte Informationen. Sie müssen zum Bereitstellen der Mitgliedschaftsdatenbank ohne Testkonten, aber mit einem Administratorkonto an.
- Da die dienstanwendungs-Datenbank und der Mitgliedschaftsdatenbank als das Datenbankmodul SQL Server Compact verwenden, müssen Sie das Datenbankmodul auf dem Hostinganbieter als auch von den Datenbanken selbst bereitstellen.
- Die Anwendung wird universal ASP.NET-Mitgliedschaftsanbietern verwendet, sodass das Mitgliedschaftssystem seine Daten in einer SQL Server Compact-Datenbank speichern kann. Die Assembly, die universelle Gruppenmitgliedschaft-Anbieter enthält, muss mit der Anwendung bereitgestellt werden.
- Die Anwendung verwendet die Entity Framework 5.0 Zugriff auf Daten in der Anwendungsdatenbank. Die Assembly mit Entity Framework 5.0 muss mit der Anwendung bereitgestellt werden.
- Die Anwendung verwendet einen Drittanbieter-Fehler, Protokollierung und-berichterstellung Hilfsprogramm. Dieses Dienstprogramm wird in einer Assembly bereitgestellt, das mit der Anwendung bereitgestellt werden müssen.
- Das Dienstprogramm zum Protokollieren der Fehler Schreibt Fehlerinformationen in XML-Dateien in einem Dateiordner. Sie müssen sicherstellen, dass das Konto, das unter ASP.NET ausgeführt, in der bereitgestellten Website wird ist die Schreibberechtigung für diesen Ordner, und Sie diesen Ordner aus der Bereitstellung ausgeschlossen. (Andernfalls Fehler Protokolldaten aus der testumgebung möglicherweise bis hin zur Produktion bereitgestellt werden und/oder Produktion Fehlerprotokolldateien möglicherweise gelöscht.)
- Die Anwendung enthält einige Einstellungen, die geändert werden müssen, die in der bereitgestellten *"Web.config"* Datei abhängig von der zielumgebung (Test oder Produktion) und andere Einstellungen, die abhängig von der Build geändert werden müssen Konfiguration (Debug oder Release).
- Visual Studio-Projektmappe enthält ein Klassenbibliotheksprojekt. Nur die Assembly, die diesem Projekt generierten bereitgestellt werden soll, nicht auf das Projekt selbst.

In diesem ersten Lernprogramm in der Reihe haben Sie das Beispielprojekt für Visual Studio heruntergeladen und überprüft Websitefunktionen, die beeinflussen, wie Sie die Anwendung bereitstellen. In den folgenden Lernprogrammen vorbereiten Sie für die Bereitstellung durch das einrichten, dass einige der folgenden Schritte automatisch behandelt werden. Andere kümmern Sie manuell.

>[!div class="step-by-step"]
[Nächste](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12.md)
