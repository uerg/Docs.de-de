---
uid: web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/configuring-team-foundation-server-for-web-deployment
title: "Konfigurieren von Team Foundation Server für Web Deploy | Microsoft Docs"
author: jrjlee
description: "In diesem Lernprogramm wird gezeigt, wie zum Konfigurieren von Team Foundation Server (TFS) 2010 zum Erstellen von Lösungen und Webinhalte auf verschiedenen zielumgebungen bereitstellen. Dies..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: ff55233a-e795-4007-a4fc-861fe1bb590b
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/configuring-team-foundation-server-for-web-deployment
msc.type: authoredcontent
ms.openlocfilehash: 72f60841a1381380c0ea6167077420f960180dc7
ms.sourcegitcommit: 493a215355576cfa481773365de021bcf04bb9c7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/15/2018
---
<a name="configuring-team-foundation-server-for-web-deployment"></a>Konfigurieren von Team Foundation Server für die Bereitstellung
====================
durch [Jason Lee](https://github.com/jrjlee)

[PDF herunterladen](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> In diesem Lernprogramm wird gezeigt, wie zum Konfigurieren von Team Foundation Server (TFS) 2010 zum Erstellen von Lösungen und Webinhalte auf verschiedenen zielumgebungen bereitstellen. Dies schließt die fortlaufende Integration (CI)-Szenarien, in der Sie Inhalt automatisch bereitstellen jedes Mal, wenn ein Entwickler eine Änderung vornimmt. Er kann auch manuelle triggerszenarios enthalten, in denen Bereitstellung eines bestimmten Builds in einer Stagingumgebung auslösen, nachdem der Build wurde überprüft und validiert, in der testumgebung für ein Administrator möchte kann. Die Themen in diesem Lernprogramm führt Sie durch den gesamten Konfigurationsprozess, einschließlich:
> 
> - Vorgehensweise: Erstellen Sie ein neues Teamprojekt in TFS.
> - Das Hinzufügen von Inhalt zur quellcodeverwaltung.
> - Wie Sie einen Build-Server zur Unterstützung von CI und Bereitstellung konfigurieren.
> - Wie Sie eine Builddefinition erstellen, die Bereitstellung Logik enthält.
> - Gewusst wie: Konfigurieren von Berechtigungen für die automatisierte Bereitstellung.
> 
> Für einen italienischen Übersetzung mit diesen Lernprogrammen, besuchen Sie [ http://www.lucamorelli.it ](http://www.lucamorelli.it).


In diesem Lernprogramm wird davon ausgegangen, dass Sie TFS 2010 installiert und eine Teamprojektsammlung im Rahmen der Erstkonfiguration erstellt haben. Die [Team Foundation-Installationshandbuch für Visual Studio 2010](https://go.microsoft.com/?linkid=9805132) enthält umfassende Anleitungen zu diesen Aufgaben.

## <a name="context"></a>Kontext

Dies ist Teil einer Reihe von Lernprogrammen, die basierend auf den Anforderungen des Enterprise-Bereitstellung eines fiktiven Unternehmens mit dem Namen Fabrikam, Inc. Diese Reihe von Lernprogrammen verwendet eine Beispielprojektmappe & #x 2014; die [Vorgesetzten Kontakts](../web-deployment-in-the-enterprise/the-contact-manager-solution.md) Lösung & #x 2014; zum Darstellen einer Webanwendung mit einer realistischen Maß an Komplexität, einschließlich einer ASP.NET MVC 3-Anwendung, eine Windows Communication Foundation (WCF)-Dienst, und ein Datenbankprojekt.

Die Bereitstellungsmethode das Herzstück mit diesen Lernprogrammen basiert auf der Teilung Datei Herangehensweise beschrieben [Verständnis des Build-Prozesses](../web-deployment-in-the-enterprise/understanding-the-build-process.md), in dem durch der Buildprozess gesteuert wird Projekt zwei Dateien & #x 2014; enthält Erstellen Sie für jede zielumgebung und enthält umgebungsspezifische Einstellungen für Build- und Bereitstellungsprozess geltenden Anweisungen, an. Zur Buildzeit ist die Unabhängigkeit von der Umgebung-Projektdatei, einen vollständigen Satz von Buildanweisungen bilden die Projektdatei umgebungsspezifische zusammengeführt.

## <a name="scenario-overview"></a>Übersicht über das Szenario

Das allgemeine Szenario für diesen Lernprogrammen wird beschrieben, [Web Unternehmensbereitstellung: Szenarioübersicht](../deploying-web-applications-in-enterprise-scenarios/enterprise-web-deployment-scenario-overview.md). Es wird empfohlen, dass Sie in diesem Thema behandelt, bevor Sie auf diesem Lernprogramm beginnen.

## <a name="how-to-use-this-tutorial"></a>Verwendung dieses Tutorials

Wenn dies das erste Mal ist haben Sie die in diesem Lernprogramm beschriebenen Aufgaben ausgeführt, oder wenn Sie die Beispiele zur Verwendung der Beispielprojektmappe folgen soll, sollten Sie über die Lernprogrammthemen in Reihenfolge arbeiten. Alternativ können Sie einzelne Themen als Leitfaden für bestimmte Aufgaben verwenden. Dieses Lernprogramm umfasst die folgenden Themen:

- [Erstellen ein Teamprojekt in TFS](creating-a-team-project-in-tfs.md). Ein Teamprojekt wird die grundlegende Einheit für die quellcodeverwaltung, Prozessmanagement und Builds in TFS. Sie müssen ein Teamprojekt erstellen, bevor Sie Inhalt an Datenquellen-Steuerelement, oder erstellen Sie Builddefinitionen hinzufügen können.
- [Hinzufügen von Inhalt zur Quellcodeverwaltung](adding-content-to-source-control.md). Wenn Sie ein Teamprojekt erstellt haben, können Sie beginnen, Hinzufügen von Inhalt zur quellcodeverwaltung. Sie müssen die Projekte und Projektmappen, zusammen mit etwaigen externe Abhängigkeiten hinzufügen, bevor Sie Builds konfigurieren können.
- [Konfigurieren eine TFS-Buildserver für Webbereitstellung](configuring-a-tfs-build-server-for-web-deployment.md). Wenn Sie Ihrem Team projizieren von Inhalten erstellen möchten, müssen Sie einen Buildserver zu konfigurieren. In den meisten Fällen sollte dies auf einem separaten Computer aus der TFS-Installation. Um einem Build-Server zu konfigurieren, müssen Sie installieren und Konfigurieren des TFS-Build-Diensts, Visual Studio 2010, erstellen Buildcontroller und build-Agents, Produkte oder Komponenten, die Ihren Code benötigt, um erfolgreich erstellt und installiert die Internetinformationsdienste (IIS) Web Webbereitstellungstool (Web Deploy).
- [Erstellen einer Builddefinition, unterstützt die Bereitstellung von](creating-a-build-definition-that-supports-deployment.md). Bevor Sie beginnen können, queuing oder Builds in TFS auslösen, müssen Sie mindestens eine Builddefinition für das Teamprojekt zu erstellen. Die Builddefinition definiert jeden Aspekt des Builds, einschließlich, was in den Build enthalten sein soll, wodurch den Build ausgelöst werden soll und, in denen die Ausgaben des builddebugvorgangs Team Build senden soll. Sie können eine Builddefinition zum Ausführen von benutzerdefinierter Microsoft Build Engine (MSBuild)-Projektdateien, konfigurieren, die Bereitstellung Logik in automatisierten Builds enthalten können.
- [Bereitstellen von einem bestimmten Build](deploying-a-specific-build.md). In einer Vielzahl von Szenarien sollten Sie den neuesten Build, statt einen bestimmten Build in einer zielumgebung bereitgestellt. In diesem Fall können Sie eine Builddefinition konfigurieren, die Inhalte von einem bestimmten Ablageordner bereitstellt.
- [Konfigurieren von Berechtigungen für Team Build-Bereitstellung](configuring-permissions-for-team-build-deployment.md). Wenn der Builddienst zum Bereitstellen von Inhalt im Rahmen einer automatisierten Build ist, müssen Sie das Build-Dienstkonto auf einem beliebigen Ziel-Webserver und Datenbankserver verschiedene Berechtigungen erteilen.

## <a name="key-technologies"></a>Schlüsseltechnologien

Dieses Lernprogramm konzentriert sich auf wie dieser Produkte und Technologien verwenden, um automatisierte Build- und Web Deploy zu unterstützen:

- Visual Studio Team Foundation Server 2010
- Teambuild und MSBuild
- Web Deploy

Das Lernprogramm erwähnt auch die Verwendung von Windows Server 2008 R2, IIS 7.5, SQL Server 2008 R2, ASP.NET 4.0 und ASP.NET MVC 3.

## <a name="other-tutorials-in-this-series"></a>Weitere Lernprogramme in dieser Serie

Dies bildet einen Teil einer Reihe von fünf Lernprogramme auf Unternehmensebene zur Bereitstellung. Dies sind die anderen Lernprogramme in der Reihe:

- [Bereitstellen von Webanwendungen in Enterprise-Szenarios](../deploying-web-applications-in-enterprise-scenarios/deploying-web-applications-in-enterprise-scenarios.md). Diese einführenden Inhalt enthält kontextbezogenen Hintergrund für die Reihe von Lernprogrammen. Das Szenario des Lernprogramme beschrieben und veranschaulicht, wie die Aufgaben und exemplarische Vorgehensweisen beschrieben, die in der gesamten Reihe in einen größeren Application Lifecycle Management (ALM) Prozess passen.
- [Die webbereitstellung im Unternehmen](../web-deployment-in-the-enterprise/web-deployment-in-the-enterprise.md). Dieses Lernprogramm enthält eine grundlegende Einführung in MSBuild-Projektdateien, die Publishing Web Pipeline (WPP), Web Deploy und anderen verwandten Technologien. Es wird erläutert, wie diese Tools gemeinsam verwendet werden können, um komplexe-bereitstellungstechnologien zu verwalten.
- [Konfigurieren von Serverumgebungen für die Bereitstellung](../configuring-server-environments-for-web-deployment/configuring-server-environments-for-web-deployment.md). In diesem Lernprogramm beschreibt, wie Windows-Servern zum unterstützen verschiedene Bereitstellungsszenarien, einschließlich remote-Web-paketbereitstellung mithilfe der Webbereitstellungs-Agent-Dienst (der remote-Agent) oder Bereitstellen von Web-Handler und die Bereitstellung des Remotezugriffs zu konfigurieren. Sie erhalten Anweisungen zum Auswählen der geeigneten Bereitstellungsmethode für Ihre Umgebung, und es wird beschrieben, wie der Web Farm Framework (WFF) verwenden, um bereitgestellter Webanwendungen über alle Webserver in einer Serverfarm zu replizieren.
- [Erweiterte Web Unternehmensbereitstellung](../advanced-enterprise-web-deployment/advanced-enterprise-web-deployment.md). In diesem Lernprogramm wird beschrieben, wie verschiedene erweiterte Bereitstellung, wie Datenbank-Bereitstellungen für mehrere Umgebungen anpassen, Ausschließen von Dateien und Ordner von der Bereitstellung und offline-Webanwendungen, die während des Bereitstellungsvorgangs Aufgaben .

>[!div class="step-by-step"]
[Nächste](creating-a-team-project-in-tfs.md)
