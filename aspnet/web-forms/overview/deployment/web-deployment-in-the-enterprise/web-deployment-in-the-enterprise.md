---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/web-deployment-in-the-enterprise
title: Webbereitstellung im Unternehmen | Microsoft-Dokumentation
author: jrjlee
description: In diesem Tutorial wird beschrieben, wie viele der Herausforderungen beim erfüllen, die Sie begegnen, wenn Sie die Bereitstellung von Webanwendungen für Unternehmen, bei verwalten...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: b8283698-7b82-42a8-8d83-3aeb18ca7fcc
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/web-deployment-in-the-enterprise
msc.type: authoredcontent
ms.openlocfilehash: 07c1ea9728c0130b860c0e0a64eb0751245ff840
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/03/2018
ms.locfileid: "37371141"
---
<a name="web-deployment-in-the-enterprise"></a>Webbereitstellung im Unternehmen
====================
durch [Jason Lee](https://github.com/jrjlee)

[PDF herunterladen](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> In diesem Tutorial wird beschrieben, wie viele der Herausforderungen beim erfüllen, die auftreten müssen, wenn Sie die Bereitstellung von Webanwendungen für Unternehmen angemessenen in Umgebungen für Entwicklungs-, Test-, Staging- und produktionsumgebungen verwalten. Das Lernprogramm enthält eine referenzlösung zusammen mit einer Mischung aus konzeptionelle und aufgabenbezogene Inhalte, die Sie über die verschiedenen Aufgaben und Verfahren zu führen.
> 
> Ein italienischen Übersetzung mit diesen Lernprogrammen finden Sie unter [ http://www.lucamorelli.it ](http://www.lucamorelli.it).


## <a name="enterprise-deployment-challenges"></a>Herausforderungen bei der Bereitstellung von Enterprise

Organisationen auftreten oft diese Herausforderungen, wenn sie die Bereitstellung von Lösungen für komplexe, unternehmensweite aussehen:

- Sie müssen zum Bereitstellen von Projekten für mehrere Umgebungen, wie Entwickler oder testumgebungen, staging-Plattformen und Produktionsserver. Die Lösung muss mit anderen Konfigurationseinstellungen für jede Umgebung bereitgestellt werden.
- Sie müssen mehrere abhängige Projekte gleichzeitig im Rahmen eines einzigen Schritt oder automatisierte Build & Deployment-Prozesses bereitstellen.
- Sie müssen auf die Bereitstellung mithilfe eines automatisierten Prozesses können. Sie möchten z. B. einen fortlaufende Integration (CI)-Prozess verwenden, um Webanwendungen in einer testumgebung bereitstellen, wenn Sie neuer Code eingecheckt wird.
- Sie müssen möglicherweise steuern den Bereitstellungsprozess und legen Sie die Bereitstellung-Variablen von außerhalb von Visual Studio, wie Entwickler es unwahrscheinlich, dass die richtigen Konfigurationseinstellungen oder den notwendigen Anmeldeinformationen für jede zielumgebung verfügen.
- Schema-basierte Datenbank-Projekte bereitzustellen und vorhandene Daten bei nachfolgenden Bereitstellungen beibehalten werden sollen.
- In diesem Fall müssen Sie eine Mitgliedschaft-Datenbanken auf einer ad-hoc-Basis bereitstellen, ohne Benutzerkontodaten bereitzustellen. Sie müssen auch das Schema der bereitgestellten Membership-Datenbanken zu aktualisieren, ohne die bestehende Konto-Benutzerdaten zu verlieren.
- Sie müssen bestimmte Dateien oder Ordner ausschließen, wenn Sie Inhalte in verschiedenen zielumgebungen bereitstellen.

## <a name="overview-of-approach"></a>Übersicht über die Vorgehensweise

Dieses Lernprogramm zusammen mit anderen Tutorials dieser Reihe, verwendet dieser allgemeinen Ansatz, um die oben beschriebenen Herausforderungen zu meistern.

- **Verwenden Sie benutzerdefinierte Microsoft Build Engine (MSBuild)-Projektdateien, um der gesamte Build & Deployment-Prozess zu steuern.**
- Dadurch können Sie das Erstellen und Bereitstellen von jedes Projekt in der Projektmappe als Teil von einem einzigen Vorgang skriptfähig.
- Umgebungsspezifische Einstellungen werden mithilfe von einfachen umgebungsspezifische Projektdateien konfiguriert. Im Gegensatz zu den Visual Studio-orientierten Ansatz Projektmappenkonfigurationen und Veröffentlichen von Profilen zum Konfigurieren von Bereitstellungen für unterschiedliche Umgebungen, mit diesem Ansatz können Sie konfigurieren und verwalten den Bereitstellungsprozess außerhalb von Visual Studio. Dies bedeutet, dass Entwickler Kenntnisse in Verbindungszeichenfolgen, Dienstendpunkte, Anmeldeinformationen und andere Variablen Bereitstellung für zielumgebungen fahren Sie fort, müssen nicht.
- Die benutzerdefinierte Projektdateien können durch Team Build als Teil eines Team Foundation Server (TFS)-Workflows aufgerufen werden. Dadurch können Sie die automatische Bereitstellung für CI-Szenarien zu konfigurieren.

**Verwenden Sie die Internetinformationsdienste (Internet Information Services, IIS)-Webbereitstellungstool (Web Deploy) zum Verpacken und Bereitstellen von Web-Anwendungsprojekte.**

- Web Deploy bietet ein Framework, mit dem Sie das Packen und Bereitstellen von Inhalt Ihrer Web-Anwendung auf einem Ziel IIS-Webserver zusammen mit Abhängigkeiten, Einstellungen, Sicherheitseinstellungen und anderen Anforderungen an.
- Sie können den gesamten Packen und-Bereitstellen Prozess in Ihre benutzerdefinierte MSBuild-Projektdateien steuern. Sie können auch die Konfigurationseinstellungen verändern, die Ihr Webbereitstellungspaket, z.B. Verbindungszeichenfolgen, Dienstendpunkte und Zieldetails für IIS zu begleiten.
- Web Deploy, zusammen mit der Veröffentlichung Webpipeline, bietet viele Erweiterungspunkte, mit die Sie Ihre Bereitstellungen anpassen können. Beispielsweise ist es einfach, Ihre Web-Bereitstellungspakete unerwünschten Dateien und Ordner ausgeschlossen werden sollen.

**Verwenden Sie das Dienstprogramm VSDBCMD.exe, bereitstellen und Aktualisieren des Datenbankschemas.**

- VSDBCMD ermöglicht Ihnen das Bereitstellen von Datenbanken aus einer Datenbank-Schema-Datei (.dbschema), die generiert wird, wenn Sie ein Visual Studio-Datenbankprojekt erstellen. Im Gegensatz dazu ist Web Deploy enthaltene Funktionen für die Bereitstellung der Datenbank besser geeignet, für die Bereitstellung von vorhandenen Datenbanken aus einer lokalen SQL Server-Instanz.
- Im Gegensatz zu Visual Studio Funktionen für die Bereitstellung von Datenbankprojekten Sie können VSDBCMD differenzielle Updates für eine vorhandene Zieldatenbank bereitstellen. Dadurch können Sie alle vorhandenen Daten beibehalten, während Sie das Datenbankschema aktualisieren.
- Sie können Befehle VSDBCMD in Ihre benutzerdefinierte MSBuild-Projektdateien ausführen.

## <a name="content-map"></a>Inhaltszuordnung

Dieses Lernprogramm enthält Themen, die in vier Hauptbereiche fallen.

Diese Themen enthalten eine Einführung referenzlösung&#x2014;Contact Manager-Lösung&#x2014;und beschrieben, wie Sie es herunterladen und konfigurieren Sie ihn auf Ihrem lokalen Computer:

- [Contact Manager-Lösung](the-contact-manager-solution.md)
- [Einrichten der Contact Manager-Lösung](setting-up-the-contact-manager-solution.md)

Diese Themen führen MSBuild-Projektdateien, beschrieben, wie Sie erstellen und Verwenden benutzerdefinierter Projektdateien und Schritt für Schritt durch den Bereitstellungsprozess für die Projektmappe Contact Manager:

- [Grundlegendes zur Projektdatei](understanding-the-project-file.md)
- [Grundlegendes zum Buildprozess](understanding-the-build-process.md)

Die folgenden Themen beschreiben die Bereitstellung von Webanwendungen, einschließlich zur des erstellungs- und Packvorgangs-Prozess, wie der Buildprozess mit der Web-Publishing-Pipeline integriert, Bereitstellungsparameter ändern und Bereitstellen von Webpaketen für Ziel Umgebungen:

- [Erstellen von Webanwendungsprojekten und Paketerstellung](building-and-packaging-web-application-projects.md)
- [Konfigurieren von Parametern für die Bereitstellung von Webpaketen](configuring-parameters-for-web-package-deployment.md)
- [Bereitstellen von Webpaketen](deploying-web-packages.md)

- [Bereitstellen von Datenbankprojekten](deploying-database-projects.md) beschreibt die verschiedenen Techniken zum Bereitstellen von Visual Studio-Datenbankprojekte, sowie die vor- und Nachteile jeder Vorgehensweise können. [Erstellen und Ausführen einer Befehlsdatei Bereitstellung](creating-and-running-a-deployment-command-file.md) wird beschrieben, wie eine einfache Befehlsdatei zu erstellen, die Ihrer Bereitstellungslogik kapselt und ermöglicht Ihnen das Bereitstellen von komplexer Lösungen als Prozess Schritt für Schritt.
- Zum Schluss [Manuelles Installieren von Webpaketen](manually-installing-web-packages.md) ist das Lernprogramm abgeschlossen wird, durch die Anzeige von Webpaketen in IIS importieren.

## <a name="key-technologies"></a>Schlüsseltechnologien

In die Themen in diesem Tutorial verwenden Sie diese Technologien in erster Linie zum Verwalten von Build und Bereitstellung:

- Visual Studio 2010
- MSBuild
- IIS 7.5
- Web Deploy 2.0
- Das bereitstellungshilfsprogramm für VSDBCMD.exe-Datenbank

## <a name="other-tutorials-in-this-series"></a>Andere Tutorials in dieser Reihe

Dies ist Teil einer Reihe von fünf Lernprogrammen auf Unternehmensniveau webbereitstellung. Dies sind die anderen Tutorials der Reihe:

- [Bereitstellen von Webanwendungen in Unternehmensszenarien](../deploying-web-applications-in-enterprise-scenarios/deploying-web-applications-in-enterprise-scenarios.md). Dieser einführenden Inhalt enthält die kontextbezogenen Hintergrundinformationen zum der Tutorial-Reihe. Das tutorialszenario beschrieben und veranschaulicht, wie die Aufgaben und exemplarische Vorgehensweisen beschrieben, die in der gesamten Reihe in einem größeren Application Lifecycle Management (ALM)-Prozess passen.
- [Konfigurieren von Serverumgebungen für die Webbereitstellung](../configuring-server-environments-for-web-deployment/configuring-server-environments-for-web-deployment.md). In diesem Tutorial wird beschrieben, wie Windows Server zur Unterstützung verschiedener Bereitstellungsszenarien, einschließlich der remote-Web-Paket-Bereitstellung mithilfe der Webbereitstellungs-Agent-Dienst (der remote-Agent) oder Bereitstellen von Web-Handler und remote-Datenbank-Bereitstellung konfiguriert. Sie erhalten Anweisungen zum Auswählen der geeigneten Bereitstellungsmethode für Ihre eigene Umgebung, und es wird beschrieben, wie das Web Farm Framework (WFF) verwenden, um bereitgestellten Webanwendungen für alle Webserver in einer Serverfarm zu replizieren.
- [Konfigurieren von Team Foundation Server für die Webbereitstellung](../configuring-team-foundation-server-for-web-deployment/configuring-team-foundation-server-for-web-deployment.md). In diesem Tutorial wird beschrieben, wie zum Konfigurieren von TFS zur Unterstützung verschiedener Bereitstellungsszenarien, einschließlich der automatisierten Bereitstellung im Rahmen eines CI-Prozesses und bestimmte Builds Bereitstellungen manuell ausgelöst wird.
- [Erweiterte webbasierte Unternehmensbereitstellung](../advanced-enterprise-web-deployment/advanced-enterprise-web-deployment.md). In diesem Tutorial wird beschrieben, wie verschiedene erweiterte Bereitstellung, z. B. Anpassen von datenbankbereitstellungen für mehrere Umgebungen und Ausschließen von Dateien und Ordner von der Bereitstellung von Webanwendungen während der Bereitstellung und Aufgaben .

> [!div class="step-by-step"]
> [Nächste](the-contact-manager-solution.md)
