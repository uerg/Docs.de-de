---
uid: web-forms/overview/deployment/deploying-web-applications-in-enterprise-scenarios/enterprise-web-deployment-scenario-overview
title: 'Web-Unternehmensbereitstellung: Szenarioübersicht | Microsoft Docs'
author: jrjlee
description: Diese Lernprogramme verwendet einer beispiellösung mit einer realistischen Maß an Komplexität, zusammen mit einem fiktiven Unternehmen Bereitstellungsszenario Ref bereitstellen...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/03/2012
ms.topic: article
ms.assetid: aa862153-4cd8-4e33-beeb-abf502c6664f
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/deploying-web-applications-in-enterprise-scenarios/enterprise-web-deployment-scenario-overview
msc.type: authoredcontent
ms.openlocfilehash: 20f6e206d6aa4bebb4936246468f5ada0e213236
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
---
<a name="enterprise-web-deployment-scenario-overview"></a>Web-Unternehmensbereitstellung: Szenarioübersicht
====================
durch [Jason Lee](https://github.com/jrjlee)

[PDF herunterladen](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Diese Reihe von Lernprogrammen verwendet eine beispiellösung mit einer realistischen Maß an Komplexität, zusammen mit einem fiktiven Unternehmen-Bereitstellungsszenario, um eine referenzimplementierung bereitzustellen und um die Aufgaben und exemplarische Vorgehensweisen für einen allgemeinen Kontext zu gewähren. In diesem Thema wird beschrieben, die das Szenario des Lernprogramme und führt die beispiellösung.


## <a name="scenario-description"></a>Beschreibung des Szenarios

Fabrikam, Inc., einem fiktiven Unternehmen, ist das Erstellen einer Lösung, mit dem remote-Vertriebsteams speichern und Abrufen von Informationen von einer Webschnittstelle.

Application Lifecycle Management (ALM) Prozesse an Fabrikam, Inc. erfordern die Lösung in drei Server-Umgebungen in verschiedenen Phasen der Entwicklung von Softwareupdates bereitgestellt werden:

- Eine Test- oder "Sandkasten" entwicklerumgebung.
- Eine Stagingumgebung intranetbasierte.
- Ein Internetzugriff-produktionsumgebung.

Jede dieser Umgebungen verfügt über unterschiedliche Konfigurations- und sicherheitsanforderungen und jede eindeutige Herausforderung stellt.

### <a name="the-fabrikam-inc-server-infrastructure"></a>The Fabrikam, Inc. Server-Infrastruktur

Dies ist die allgemeine Entwicklung und Bereitstellung Infrastruktur bei Fabrikam, Inc.

![](enterprise-web-deployment-scenario-overview/_static/image1.png)

Die Developer-Arbeitsstationen, die Infrastruktur des Steuerelements, die Umgebung für Entwickler und der Stagingumgebung, die alle im Intranet Netzwerk innerhalb der Fabrikam.net-Domäne befinden. Die produktionsumgebung befindet sich in einem Umkreisnetzwerk (auch bekannt als DMZ, demilitarisierte Zone und überwachtes Subnetz bezeichnet), die aus dem Intranetnetzwerk durch eine Firewall getrennt ist. Dies ist ein gängiges Bereitstellungsszenario: Webserver Internetzugriff i. d. r. isoliert, aus der internen Server-Infrastruktur durch die Verwendung von Firewalls oder Gatewayserver.

In diesem Beispiel:

- Ein Team Foundation Server (TFS) 2010-Server mit einem separaten Build-Server bietet quellcodeverwaltung und die fortlaufende Integration (CI)-Funktionen.
- Die Developer-testumgebung umfasst einen Internet Information Services (IIS) 7.5-Web-Server und einem SQL Server 2008 R2-Datenbankserver.
- Die produktionsumgebung umfasst mehrere IIS 7.5-Webservern synchronisiert, indem ein Web Farm Framework (WFF)-Controller-Server zusammen mit einem SQL Server 2008 R2-Datenbankserver. In der Praxis kann der Datenbankserver verwenden Failoverclustering oder Spiegelung zur Verbesserung der Skalierbarkeit und Verfügbarkeit.
- Die Stagingumgebung wurde entwickelt, um die Konfiguration der produktionsumgebung soweit wie möglich zu replizieren.
- Die Firewall- und Netzwerkeinstellungen Isolation Richtlinien zulassen direkter und automatisierte Bereitstellung aus dem Intranet mit dem Umkreisnetzwerk nicht.

Die Konfiguration dieser Umgebungen wird ausführlicher beschrieben, in der zweiten Lernprogramm [Konfigurieren von Server-Umgebungen für die Bereitstellung](../configuring-server-environments-for-web-deployment/configuring-server-environments-for-web-deployment.md).

### <a name="team-roles-for-alm"></a>Rollen für ALM

Diese Benutzer werden beim Erstellen, verwalten, erstellen und Veröffentlichen der Projektmappe Contact Manager beteiligt:

- Matt Hink ist ein Web-Anwendung-Entwickler bei Fabrikam, Inc. Er ist Teil des Teams, die die Kontakt-Manager-Lösung mithilfe von Visual Studio 2010 entwickelt. Matt hat vollständige Administratorrechte auf den Servern in der testumgebung für Entwickler, die ihm die Umgebung entsprechend seinen Bedürfnissen konfigurieren können. Er verfügt auch über Benutzerzugriff auf die Visual Studio 2010-TFS-Instanz, in dem er den Quellcode für die Projektmappe Contact Manager speichert.
- Rob Walters ist ein Server-Administrator für das Entwicklungsteam Fabrikam, Inc. an. Rob hat administrativen Zugriff auf die TFS-Server, sodass er alle Aspekte von TFS und Team Build konfigurieren kann. Rob auch verfügt über Verwaltungszugriff auf den Test und staging Webservern und fungiert als Datenbankadministrator (DBA) für den datenbanservern in die Test- und Stagingumgebungen. Rob wurde auf dem TFS-Server zum Ausführen dieser Aufgaben Team Build konfiguriert:

    - Erstellen und Ausführen von Komponententests für die Anwendung, wenn ein Benutzer eine Datei mit TFS eincheckt. Hierbei spricht CI.
    - Bereitstellen Sie die Kontakt-Manager-Anwendung in der testumgebung automatisch, sobald die Anwendung Komponententests übergibt. Dies schließt die Datenbank auf dem Testserver auf der ursprünglichen Bereitstellung und alle Updates für die Datenbank nach der erstmaligen Bereitstellung veröffentlichen.
    - Stellen Sie die Kontakt-Manager-Anwendung in der Stagingumgebung in einem einzigen Schritt Prozess bereit.
    - Erstellen Sie eine Webpaket, das ein Web-Server-Administrator und ein DBA verwenden, um die Anwendung in der produktionsumgebung zu veröffentlichen.
- Lisa Andrews ist ein Serveradministrator für die Bereitstellung von Anwendungen auf der Fabrikam, Inc. Produktionsservern verantwortlich. Sie verfügt über Lesezugriff auf die Freigabe, in dem die TFS-Team Build Webbereitstellungspaket speichert, sobald er die Kontakt-Manager-Anwendung erstellt wird. Sie muss außerdem administrativen Zugriff auf die Produktions-Webservern, so, dass sie die Anwendung für die Produktion bereitstellen kann. Darüber hinaus fungiert sie als der Datenbankadministrator, die Datenbanken und Datenbankupdates an den Datenbankserver in der produktionsumgebung bereitstellen.

<a id="_The_Contact_Manager"></a>

### <a name="the-contact-manager-solution"></a>Die Kontakt-Manager-Lösung

Die Projektmappe Contact Manager dient, damit registrierte, angemeldeten Benutzer hinzufügen und Kontaktinformationen über eine Weboberfläche bearbeiten. Die Kontakt-Manager-Lösung besteht aus vier einzelne Projekte:

![](enterprise-web-deployment-scenario-overview/_static/image2.png)

- **ContactManager.Mvc**. Dies ist eine ASP.NET MVC3-Webanwendungsprojekt, die den Einstiegspunkt für die Projektmappe darstellt. Er bietet einige grundlegende Web Application-Funktionen, wie Benutzern die Möglichkeit zum Erstellen und Anzeigen von Details des Kontakts. Die Anwendung verwendet einen Windows Communication Foundation (WCF)-Dienst zum Verwalten von Kontakten und eine Datenbank für ASP.NET-Anwendungsdienste zum Verwalten der Authentifizierung und Autorisierung.
- **ContactManager.Database**. Dies ist ein Visual Studio 2010-Datenbankprojekt. Das Projekt definiert das Schema für eine Datenbank, speichert Informationen wenden Sie sich an.
- **ContactManager.Service**. Dies ist ein WCF-Webdienstprojekt. Die WCF macht ein Endpunkt, der Aufrufern ermöglicht, führen zu erstellen, abrufen, aktualisieren und Löschvorgänge (CRUD) für die Kontakt-Manager-Datenbank. Der Dienst hängt davon ab, der Kontakt-Manager-Datenbank und die ContactManager.Common.dll-Assembly.
- **ContactManager.Common**. Dies ist ein Klassenbibliotheksprojekt. Der WCF-Dienst basiert auf in dieser Assembly definierten Typen.

Eine vollständige Überprüfung der Lösung und die Anforderungen für die Bereitstellung finden Sie im ersten Lernprogramm dieser Reihe [Webbereitstellung im Unternehmen](../web-deployment-in-the-enterprise/web-deployment-in-the-enterprise.md).

<a id="_Deployment_Tasks"></a>

## <a name="deployment-tasks"></a>Bereitstellungsaufgaben

Bereitstellen von Anwendungen für unterschiedliche Umgebungen in einem großen Unternehmen stehen mehrere verschiedene Aufgaben. Dies sind die wichtigsten Aufgaben, die die Lernprogramme behandeln:

![](enterprise-web-deployment-scenario-overview/_static/image3.png)

Hier ist eine Liste der einzelnen Schritte im Bereitstellungsprozess aus der Perspektive der Benutzer, die weiter oben in diesem Dokument beschrieben:

1. Alle Mitglieder des Teams überprüfen Sie die Kontakt-Manager-Projektmappe in Visual Studio 2010, um wichtige bereitstellungsanforderungen und Probleme zu bestimmen.
2. Matt Hink können die Projektmappe Contact Manager direkt von der Developer-Arbeitsstation in der Developer-testumgebung für die Durchführung von einem ersten Test Logik, die Bereitstellung bereitstellen.
3. Matt Hink fügt die Anwendung zur quellcodeverwaltung in TFS.
4. Rob Walters erstellt verschiedene Builddefinitionen für die Projektmappe Contact Manager in Team Build. Eine Builddefinition verwendet CI zum Bereitstellen der Lösung in der testumgebung für Entwickler, wenn ein Benutzer im neuen Code eincheckt. Eine weitere Builddefinition kann Benutzer Trigger Bereitstellungen in der Stagingumgebung nach Bedarf.
5. Jedes Mal, wenn ein Benutzer im neuen Code eincheckt, Team Build automatisch erstellt die Komponenten der Lösung, Komponententests und stellt die Lösung bereit, um die Umgebung für Entwickler, wenn die Erstellung erfolgreich war und die Komponententests bestanden.
6. Wenn ein Benutzer eine Bereitstellung in der Stagingumgebung auslöst, wird die Projektmappe verpackt und in einem einzigen Schritt Prozess bereitgestellt. Dieser Prozess generiert außerdem ein Paket für die manuelle Bereitstellung in der produktionsumgebung.
7. Lisa Andrews stellt die Anwendung in der produktionsumgebung bereit, indem Sie manuell importieren der Web-Paket, das in Schritt 6 erstellt haben.

### <a name="key-deployment-issues"></a>Wichtige Probleme bei

Markieren Sie die Projektmappe Contact Manager und der Fabrikam, Inc. Szenario verschiedene häufig auftretende Probleme und Herausforderungen, die möglicherweise auftreten, wenn Sie komplexe, unternehmensweite Lösungen bereitstellen. Zum Beispiel:

- Sie müssen in der Lage, Bereitstellen von Projekten in mehreren Umgebungen, wie Entwickler oder testumgebungen, staging-Plattformen und Produktionsservern. Die Lösung muss mit verschiedenen Konfigurationseinstellungen für jede Umgebung bereitgestellt werden.
- Sie müssen mehrere abhängige Projekte gleichzeitig als Teil eines einstufiger Prozesses oder eines automatisierten Build- und Bereitstellungsprozess bereitstellen.
- In diesem Fall müssen Sie eine Bereitstellung des Laufwerks aus einen automatisierten Prozess können. Angenommen, möchten Sie einen CI-Prozess zu verwenden, um die Webanwendungen in einer Stagingumgebung bereitstellen, wenn Sie neuer Code eingecheckt wird.
- Sie müssen in der Lage, steuern den Bereitstellungsprozess und Festlegen von Bereitstellung Variablen von außerhalb von Visual Studio, wie Entwickler es unwahrscheinlich, dass die richtigen Konfigurationseinstellungen oder den notwendigen Anmeldeinformationen für jede zielumgebung haben, sind.
- Sie müssen schemabasierte Datenbankprojekte bereitstellen und vorhandene Daten bei nachfolgenden Bereitstellungen beibehalten.
- In diesem Fall müssen Sie eine Mitgliedschaft Datenbanken auf einer ad-hoc-Basis bereitstellen, ohne Benutzerkontodaten bereitzustellen. Sie müssen möglicherweise auch das Schema der bereitgestellten Mitgliedschaft Datenbanken zu aktualisieren, ohne Datenverlust vorhandene Konto.
- In diesem Fall müssen Sie eine bestimmte Dateien oder Ordner ausschließen, beim Bereitstellen von Inhalt für unterschiedliche zielumgebungen.

Darüber hinaus löst die Bereitstellung verwalten, wenn häufige und inkrementellen Updates um eine weitere Herausforderung dar. Zum Beispiel:

- Zum Ausführen von Komponententests jedes Mal, wenn ein Entwickler im neuen Code eincheckt. Sie möchten nur die Projektmappe bereitstellen, wenn der Code die Komponententests übergibt.
- Wenn Sie eine Webanwendung in einer Staging- oder Produktionsserver Umgebung bereitstellen, die Sie Benutzer umleiten möchten eine *app\_offline.htm* -Datei für die Dauer des Bereitstellungsprozesses.
- Bereitstellungsaktivitäten protokolliert werden soll. Der Bereitstellungsprozess sollte e-Mail-Benachrichtigungen der erfolgreiche oder fehlgeschlagene Bereitstellungen an festgelegte Empfänger senden.
- Ausfall eine automatisierte Bereitstellung sollten der Bereitstellungsprozess wiederholen Sie die aktuelle Bereitstellung oder der vorherigen Webpaket stattdessen bereitstellen.

> [!div class="step-by-step"]
> [Zurück](deploying-web-applications-in-enterprise-scenarios.md)
> [Weiter](application-lifecycle-management-from-development-to-production.md)
