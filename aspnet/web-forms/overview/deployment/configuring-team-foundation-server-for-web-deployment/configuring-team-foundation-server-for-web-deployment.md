---
uid: web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/configuring-team-foundation-server-for-web-deployment
title: Team Foundation Server für die Bereitstellung konfigurieren | Microsoft-Dokumentation
author: jrjlee
description: In diesem Tutorial erfahren Sie, wie Team Foundation Server (TFS) 2010 zum Erstellen von Lösungen und Bereitstellen von Web-Inhalte in verschiedenen zielumgebungen zu konfigurieren. Dies...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: ff55233a-e795-4007-a4fc-861fe1bb590b
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/configuring-team-foundation-server-for-web-deployment
msc.type: authoredcontent
ms.openlocfilehash: 6430a96a8e430a8a30d062ec22868de829680806
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/03/2018
ms.locfileid: "37365340"
---
<a name="configuring-team-foundation-server-for-web-deployment"></a>Team Foundation Server für die Bereitstellung konfigurieren
====================
durch [Jason Lee](https://github.com/jrjlee)

[PDF herunterladen](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> In diesem Tutorial erfahren Sie, wie Team Foundation Server (TFS) 2010 zum Erstellen von Lösungen und Bereitstellen von Web-Inhalte in verschiedenen zielumgebungen zu konfigurieren. Dies schließt die fortlaufende Integration (CI) Szenarien, in dem Sie Inhalte automatisch bereitstellen jedes Mal, wenn ein Entwickler eine Änderung vornimmt. Er kann auch Szenarien manueller Trigger enthalten, in denen ein Administrator für die Bereitstellung eines bestimmten Builds in einer Stagingumgebung auslösen, nachdem der Build wurde überprüft und validiert, in der testumgebung möchten möglicherweise. Die Themen in diesem Tutorial führt Sie durch den gesamten Konfigurationsprozess, einschließlich:
> 
> - Vorgehensweise: Erstellen Sie ein neues Teamprojekt in TFS.
> - Inhalte zur quellcodeverwaltung hinzufügen
> - Vorgehensweise: konfigurieren ein Buildservers zur Unterstützung von CI und Bereitstellung.
> - Erstellen eine Builddefinition, die Bereitstellungslogik enthält.
> - So konfigurieren Sie Berechtigungen für die automatisierte Bereitstellung.
> 
> Ein italienischen Übersetzung mit diesen Lernprogrammen finden Sie unter [ http://www.lucamorelli.it ](http://www.lucamorelli.it).


In diesem Tutorial wird davon ausgegangen, dass die Installation von TFS 2010 und eine teamprojektauflistung im Rahmen der anfänglichen Konfiguration erstellt. Die [Installationshandbuch zu Team Foundation für Visual Studio 2010](https://go.microsoft.com/?linkid=9805132) bietet eine umfassende Anleitung zu diesen Aufgaben.

## <a name="context"></a>Kontext

Dies ist Teil einer Reihe von Tutorials, die basierend auf den Anforderungen des Enterprise-Bereitstellung für das fiktive Unternehmen mit dem Namen Fabrikam, Inc. Dieser tutorialreihe verwendet eine beispiellösung&#x2014;der [Contact Manager](../web-deployment-in-the-enterprise/the-contact-manager-solution.md) Lösung&#x2014;zur Darstellung einer Webanwendung mit einem realistischen Maß an Komplexität, einschließlich einer ASP.NET MVC 3-Anwendung, eine Windows-Kommunikation Foundation (WCF)-Dienst und ein Datenbankprojekt.

Die Methode für die Bereitstellung das Kernstück des in diesen Tutorials basiert auf den geteilten Projekt Dateiansatz beschrieben, die [Verständnis des Prozesses erstellen](../web-deployment-in-the-enterprise/understanding-the-build-process.md), in dem der Buildprozess durch gesteuert wird zwei Projektdateien&#x2014;enthält Erstellen Sie die Anweisungen, die für jede zielumgebung, und enthält umgebungsspezifische Build & Deployment-Einstellungen gelten. Zur Erstellungszeit wird die umgebungsspezifischen-Projektdatei in die Unabhängigkeit von der Umgebung Projektdatei, um einen vollständigen Satz von einrichtungsanweisungen bilden zusammengeführt.

## <a name="scenario-overview"></a>Übersicht über das Szenario

Das allgemeine Szenario für diese Tutorials finden Sie im [webbasierte Unternehmensbereitstellung: Szenarioübersicht](../deploying-web-applications-in-enterprise-scenarios/enterprise-web-deployment-scenario-overview.md). Es wird empfohlen, in diesem Thema lesen, bevor Sie mit diesem Tutorial beginnen.

## <a name="how-to-use-this-tutorial"></a>Verwendung dieses Tutorials

Wenn das erste Mal haben Sie die in diesem Tutorial beschriebenen Aufgaben ausgeführt, oder wenn Sie den beispiellösung mit Beispielen folgen möchten, sollten Sie den Lernprogrammthemen in der Reihenfolge durcharbeiten. Alternativ können Sie einzelne Themen als Leitfaden für bestimmte Aufgaben verwenden. In diesem Lernprogramm umfasst folgende Themen:

- [Erstellen eines Teamprojekts in TFS](creating-a-team-project-in-tfs.md). Ein Teamprojekt ist die grundlegende Einheit für quellcodeverwaltung, prozessverwaltung und Builds in TFS. Sie müssen ein Teamprojekt erstellen, bevor Sie Inhalt an Datenquellen-Steuerelement, oder Erstellen von Builddefinitionen hinzufügen können.
- [Hinzufügen von Inhalten zur Quellcodeverwaltung](adding-content-to-source-control.md). Nachdem Sie ein Teamprojekt erstellt haben, können Sie beginnen, Hinzufügen von Inhalten zur quellcodeverwaltung. Sie müssen Ihre Projekte und Projektmappen, zusammen mit externen Abhängigkeiten, hinzufügen, bevor Sie Builds konfigurieren können.
- [Konfigurieren eine TFS-Buildserver für die Webbereitstellung](configuring-a-tfs-build-server-for-web-deployment.md). Wenn Sie Ihre Inhalte der Team-Projekt erstellen möchten, müssen Sie einen Buildserver konfigurieren zu können. In den meisten Fällen sollte dies auf einem separaten Computer aus der TFS-Installation. Um einen Buildserver konfigurieren zu können, müssen Sie installieren und Konfigurieren der TFS-Build-Diensts, Visual Studio 2010 installieren, erstellen Buildcontroller und build-Agents, installieren Sie Produkte oder Komponenten, die Ihr Code benötigt, um erfolgreich erstellt werden, und installieren Sie die Internetinformationsdienste (IIS)-Webbereitstellungstool (Web Deploy).
- [Erstellen eine Builddefinition, unterstützt die Bereitstellung](creating-a-build-definition-that-supports-deployment.md). Bevor Sie beginnen können, queuing oder Builds in TFS auslösen, müssen Sie mindestens eine Builddefinition für das Teamprojekt zu erstellen. Die Build-Definition definiert alle Aspekte des Builds, einschließlich, was in den Build enthalten sein sollen und was den Build auslösen sollen, in dem die Buildausgaben Team Build senden soll. Sie können eine Builddefinition zum Ausführen von benutzerdefinierter Microsoft Build Engine (MSBuild)-Projektdateien, konfigurieren, die Sie Bereitstellungslogik in Ihren automatisierten Builds einschließen können.
- [Bereitstellen eines bestimmten Builds](deploying-a-specific-build.md). In vielen Szenarien sollten Sie einen bestimmten Build, anstatt den aktuellen Build, in einer zielumgebung bereitgestellt. In diesem Fall können Sie eine Builddefinition konfigurieren, die Inhalte aus einem bestimmten Ablageordner bereitstellt.
- [Konfigurieren von Berechtigungen für Team-Buildbereitstellung](configuring-permissions-for-team-build-deployment.md). Wenn der Builddienst wird zum Bereitstellen von Inhalt als Teil einer automatisierten Buildprozess zu, müssen Sie verschiedene Berechtigungen für den Build Service-Konto für alle Ziel-Webserver und Datenbankserver zu gewähren.

## <a name="key-technologies"></a>Schlüsseltechnologien

In diesem Tutorial geht es um die Produkte und Technologien zu verwenden, um automatische Builds und webbereitstellung zu unterstützen:

- Visual Studio Team Foundation Server 2010
- Teambuild und MSBuild
- Web Deploy

Das Tutorial betrifft auch bei der Verwendung von Windows Server 2008 R2, IIS 7.5, SQL Server 2008 R2, ASP.NET 4.0 und ASP.NET MVC 3.

## <a name="other-tutorials-in-this-series"></a>Andere Tutorials in dieser Reihe

Dies ist Teil einer Reihe von fünf Lernprogrammen auf Unternehmensniveau webbereitstellung. Dies sind die anderen Tutorials der Reihe:

- [Bereitstellen von Webanwendungen in Unternehmensszenarien](../deploying-web-applications-in-enterprise-scenarios/deploying-web-applications-in-enterprise-scenarios.md). Dieser einführenden Inhalt enthält die kontextbezogenen Hintergrundinformationen zum der Tutorial-Reihe. Das tutorialszenario beschrieben und veranschaulicht, wie die Aufgaben und exemplarische Vorgehensweisen beschrieben, die in der gesamten Reihe in einem größeren Application Lifecycle Management (ALM)-Prozess passen.
- [Webbereitstellung im Unternehmen](../web-deployment-in-the-enterprise/web-deployment-in-the-enterprise.md). Dieses Tutorial bietet eine grundlegende Einführung in MSBuild-Projektdateien, die Web Publishing-Pipeline (WPP), Web Deploy und andere verwandten Technologien. Es wird erläutert, wie diese Tools zusammen verwenden werden können, um komplexe Bereitstellungsprozesse zu verwalten.
- [Konfigurieren von Serverumgebungen für die Webbereitstellung](../configuring-server-environments-for-web-deployment/configuring-server-environments-for-web-deployment.md). In diesem Tutorial wird beschrieben, wie Windows Server zur Unterstützung verschiedener Bereitstellungsszenarien, einschließlich der remote-Web-Paket-Bereitstellung mithilfe der Webbereitstellungs-Agent-Dienst (der remote-Agent) oder Bereitstellen von Web-Handler und remote-Datenbank-Bereitstellung konfiguriert. Sie erhalten Anweisungen zum Auswählen der geeigneten Bereitstellungsmethode für Ihre eigene Umgebung, und es wird beschrieben, wie das Web Farm Framework (WFF) verwenden, um bereitgestellten Webanwendungen für alle Webserver in einer Serverfarm zu replizieren.
- [Erweiterte webbasierte Unternehmensbereitstellung](../advanced-enterprise-web-deployment/advanced-enterprise-web-deployment.md). In diesem Tutorial wird beschrieben, wie verschiedene erweiterte Bereitstellung, z. B. Anpassen von datenbankbereitstellungen für mehrere Umgebungen und Ausschließen von Dateien und Ordner von der Bereitstellung von Webanwendungen während der Bereitstellung und Aufgaben .

> [!div class="step-by-step"]
> [Nächste](creating-a-team-project-in-tfs.md)
