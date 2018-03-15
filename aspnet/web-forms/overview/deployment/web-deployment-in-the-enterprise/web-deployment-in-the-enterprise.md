---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/web-deployment-in-the-enterprise
title: Die webbereitstellung im Unternehmen | Microsoft Docs
author: jrjlee
description: "In diesem Lernprogramm beschreibt, wie viele der Herausforderungen erfüllen, die Sie beim Verwalten der Bereitstellung von Enterprise-Skalierung von Webanwendungen auf entwickl auftreten werden..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: b8283698-7b82-42a8-8d83-3aeb18ca7fcc
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/web-deployment-in-the-enterprise
msc.type: authoredcontent
ms.openlocfilehash: 6210d01f65bcadf8ae4209e372d5aac68861bd7a
ms.sourcegitcommit: 493a215355576cfa481773365de021bcf04bb9c7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/15/2018
---
<a name="web-deployment-in-the-enterprise"></a>Webbereitstellung im Unternehmen
====================
durch [Jason Lee](https://github.com/jrjlee)

[PDF herunterladen](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> In diesem Lernprogramm beschreibt, wie viele der Herausforderungen erfüllen, die Sie treffen, wenn Sie die Bereitstellung von Enterprise-Skalierung-Webanwendungen für die Entwicklung, Test, Staging-und produktionsumgebungen verwalten. Das Lernprogramm enthält eine referenzlösung zusammen mit einer Mischung aus konzeptionelle und aufgabenspezifische Inhalt führt Sie durch verschiedene allgemeine Aufgaben und Verfahren.
> 
> Für einen italienischen Übersetzung mit diesen Lernprogrammen, besuchen Sie [ http://www.lucamorelli.it ](http://www.lucamorelli.it).


## <a name="enterprise-deployment-challenges"></a>Enterprise-Herausforderung

Organisationen auftreten dieser Herausforderung häufig, wenn sie ansehen, um die Bereitstellung der komplexen, unternehmensweite Lösungen zu verwalten:

- Sie müssen in der Lage, Bereitstellen von Projekten in mehreren Umgebungen, wie Entwickler oder testumgebungen, staging-Plattformen und Produktionsservern. Die Lösung muss mit verschiedenen Konfigurationseinstellungen für jede Umgebung bereitgestellt werden.
- Sie müssen mehrere abhängige Projekte gleichzeitig als Teil eines einstufiger Prozesses oder eines automatisierten Build- und Bereitstellungsprozess bereitstellen.
- In diesem Fall müssen Sie eine Bereitstellung des Laufwerks aus einen automatisierten Prozess können. Sie möchten z. B. einen Prozess für die fortlaufende Integration (CI) zu verwenden, um die Webanwendungen in einer testumgebung bereitstellen, wenn Sie neuer Code eingecheckt wird.
- Sie müssen in der Lage, steuern den Bereitstellungsprozess und Festlegen von Bereitstellung Variablen von außerhalb von Visual Studio, wie Entwickler es unwahrscheinlich, dass die richtigen Konfigurationseinstellungen oder den notwendigen Anmeldeinformationen für jede zielumgebung haben, sind.
- Sie müssen schemabasierte Datenbankprojekte bereitstellen und vorhandene Daten bei nachfolgenden Bereitstellungen beibehalten.
- In diesem Fall müssen Sie eine Mitgliedschaft Datenbanken auf einer ad-hoc-Basis bereitstellen, ohne Benutzerkontodaten bereitzustellen. Sie müssen möglicherweise auch das Schema der bereitgestellten Mitgliedschaft Datenbanken zu aktualisieren, ohne Datenverlust vorhandene Konto.
- In diesem Fall müssen Sie eine bestimmte Dateien oder Ordner ausschließen, beim Bereitstellen von Inhalt für unterschiedliche zielumgebungen.

## <a name="overview-of-approach"></a>Übersicht über Vorgehensweise

Dieses Lernprogramm zusammen mit den anderen Lernprogrammen in dieser Serie verwendet diesen allgemeinen Ansatz zur der oben beschriebenen Auflagen erfüllt werden können.

- **Verwenden Sie benutzerdefinierte Microsoft Build Engine (MSBuild)-Projektdateien, um den Gesamtprozess Build- und Bereitstellungsprozess steuern.**
- Dadurch können Sie das Erstellen und Bereitstellen von jedes Projekt in der Projektmappe als Teil eines einzelnen, skriptfähige-Vorgangs.
- Umgebungsspezifische Einstellungen werden mithilfe von einfachen umgebungsspezifische Projektdateien konfiguriert. Im Gegensatz zu den Visual Studio – zentriertes Ansatzes der Verwendung von Projektmappenkonfigurationen und veröffentlichungsprofilen Bereitstellungen für unterschiedliche Umgebungen zu konfigurieren, mit diesem Ansatz können Sie konfigurieren und verwalten den Bereitstellungsprozess außerhalb von Visual Studio. Dies bedeutet, dass Entwickler Kenntnisse der Verbindungszeichenfolgen, Dienstendpunkte, Anmeldeinformationen und andere Variablen Bereitstellung für zielumgebungen wechseln müssen, nicht.
- Die benutzerdefinierte Projektdateien können vom Team Build als Teil eines Workflows für Team Foundation Server (TFS) aufgerufen werden. Dadurch können Sie die automatische Bereitstellung für CI-Szenarien zu konfigurieren.

**Verwenden Sie die Internetinformationsdienste (Internet Information Services, IIS)-Webbereitstellungstool (Web Deploy) zum Packen und Bereitstellen von Webanwendungsprojekte.**

- Web Deploy bietet ein Framework, mit dem Sie das Packen und Bereitstellen Ihrer Anwendung Webinhalte zu einem Zielserver IIS Web, zusammen mit Abhängigkeiten, Einstellungen, Sicherheitseinstellungen und anderen Anforderungen an.
- Sie können den gesamten Packen und-Bereitstellen Prozess in Ihre benutzerdefinierte MSBuild-Projektdateien steuern. Sie können auch die Konfigurationseinstellungen bearbeiten, die Ihre Webbereitstellungspaket wie Verbindungszeichenfolgen, die Dienstendpunkte und IIS Ziel Details offen.
- Web Deploy, zusammen mit der Web-Publishing-Pipeline bietet viele Erweiterungspunkte, mit die Sie Ihre Bereitstellungen anpassen können. Beispielsweise ist es einfach, Ihre Web-Bereitstellungspakete nicht benötigte Dateien und Ordner ausgeschlossen.

**Verwenden Sie das Dienstprogramm VSDBCMD.exe, bereitstellen und Aktualisieren des Datenbankschemas.**

- VSDBCMD ermöglicht Ihnen die Bereitstellung von Datenbanken aus einer Datenbank-Schemadatei (.dbschema), die generiert wird, wenn Sie ein Visual Studio-Datenbankprojekt erstellen. Im Gegensatz dazu ist die Datenbank neue Bereitstellungsfunktion in Web Deploy enthalten bis zur Bereitstellung von vorhandenen Datenbanken aus einer lokalen SQL Server-Instanz besser geeignet.
- Im Gegensatz zu Visual Studio-Funktionalität für das Bereitstellen von Datenbankprojekten können mit VSDBCMD differenzielle Updates in einer vorhandenen Zieldatenbank bereitzustellen. Dadurch können Sie alle vorhandenen Daten beibehalten, während Sie das Datenbankschema aktualisieren.
- Sie können in Ihre benutzerdefinierte MSBuild-Projektdateien VSDBCMD Befehle ausführen.

## <a name="content-map"></a>Inhaltszuordnung

Dieses Lernprogramm enthält Themen, die in vier Hauptbereiche fallen.

Diese Themen stellen Sie vor der referenzlösung & #x 2014; die Kontakt-Manager-Lösung & #x 2014; und beschrieben, wie die Software hier herunterladen und auf dem lokalen Computer zu konfigurieren:

- [Contact Manager-Lösung](the-contact-manager-solution.md)
- [Einrichten der Contact Manager-Lösung](setting-up-the-contact-manager-solution.md)

Diese Themen einführen von MSBuild-Projektdateien, beschreiben, wie Sie erstellen und Verwenden von benutzerdefinierten Projektdateien und führt durch den Bereitstellungsprozess für die Projektmappe Contact Manager:

- [Grundlegendes zur Projektdatei](understanding-the-project-file.md)
- [Grundlegendes zum Buildprozess](understanding-the-build-process.md)

Die folgenden Themen beschreiben die Bereitstellung von Webanwendungen, einschließlich wie der Build und Verpacken-Prozess, wie während des Erstellungsprozesses in die Publishing Web-Pipeline integriert Bereitstellungsparameter ändern und Bereitstellen von Webpaketen auf Ziel Umgebungen:

- [Erstellen von Webanwendungsprojekten und Paketerstellung](building-and-packaging-web-application-projects.md)
- [Konfigurieren von Parametern für die Bereitstellung von Webpaketen](configuring-parameters-for-web-package-deployment.md)
- [Bereitstellen von Webpaketen](deploying-web-packages.md)

- [Bereitstellen von Datenbankprojekten](deploying-database-projects.md) beschreibt die verschiedenen Techniken zum Bereitstellen von Visual Studio-Datenbankprojekte, zusammen mit den vor- und Nachteile jeder Vorgehensweise können. [Erstellen und Ausführen einer Befehlsdatei Bereitstellung](creating-and-running-a-deployment-command-file.md) wird beschrieben, wie eine einfache Befehlsdatei zu erstellen, kapselt die Logik für die Bereitstellung und ermöglicht Ihnen das Bereitstellen von komplexer Lösungen als einzelnen Schritten bestehender Prozess.
- Schließlich [Webpaketen manuell installieren](manually-installing-web-packages.md) ist das Lernprogramm abgeschlossen, indem Sie Webpaketen in IIS importieren angezeigt.

## <a name="key-technologies"></a>Schlüsseltechnologien

Die Themen in diesem Lernprogramm verwenden dieser Technologien in erster Linie um Build- und Bereitstellungsprozess zu verwalten:

- Visual Studio 2010
- MSBuild
- IIS 7.5
- Web Deploy 2.0
- Das bereitstellungshilfsprogramm für VSDBCMD.exe-Datenbank

## <a name="other-tutorials-in-this-series"></a>Weitere Lernprogramme in dieser Serie

Dies bildet einen Teil einer Reihe von fünf Lernprogramme auf Unternehmensebene zur Bereitstellung. Dies sind die anderen Lernprogramme in der Reihe:

- [Bereitstellen von Webanwendungen in Enterprise-Szenarios](../deploying-web-applications-in-enterprise-scenarios/deploying-web-applications-in-enterprise-scenarios.md). Diese einführenden Inhalt enthält kontextbezogenen Hintergrund für die Reihe von Lernprogrammen. Das Szenario des Lernprogramme beschrieben und veranschaulicht, wie die Aufgaben und exemplarische Vorgehensweisen beschrieben, die in der gesamten Reihe in einen größeren Application Lifecycle Management (ALM) Prozess passen.
- [Konfigurieren von Serverumgebungen für die Bereitstellung](../configuring-server-environments-for-web-deployment/configuring-server-environments-for-web-deployment.md). In diesem Lernprogramm beschreibt, wie Windows-Servern zum unterstützen verschiedene Bereitstellungsszenarien, einschließlich remote-Web-paketbereitstellung mithilfe der Webbereitstellungs-Agent-Dienst (der remote-Agent) oder Bereitstellen von Web-Handler und die Bereitstellung des Remotezugriffs zu konfigurieren. Sie erhalten Anweisungen zum Auswählen der geeigneten Bereitstellungsmethode für Ihre Umgebung, und es wird beschrieben, wie der Web Farm Framework (WFF) verwenden, um bereitgestellter Webanwendungen über alle Webserver in einer Serverfarm zu replizieren.
- [Konfigurieren von Team Foundation Server für die Bereitstellung](../configuring-team-foundation-server-for-web-deployment/configuring-team-foundation-server-for-web-deployment.md). In diesem Lernprogramm wird beschrieben, wie zum Konfigurieren von TFS, sodass unterstützen verschiedene Bereitstellungsszenarien, einschließlich automatisierter Bereitstellung im Rahmen des CI-Prozess und Bereitstellungen von bestimmte Builds manuell ausgelöst wird.
- [Erweiterte Web Unternehmensbereitstellung](../advanced-enterprise-web-deployment/advanced-enterprise-web-deployment.md). In diesem Lernprogramm wird beschrieben, wie verschiedene erweiterte Bereitstellung, wie Datenbank-Bereitstellungen für mehrere Umgebungen anpassen, Ausschließen von Dateien und Ordner von der Bereitstellung und offline-Webanwendungen, die während des Bereitstellungsvorgangs Aufgaben .

>[!div class="step-by-step"]
[Nächste](the-contact-manager-solution.md)
