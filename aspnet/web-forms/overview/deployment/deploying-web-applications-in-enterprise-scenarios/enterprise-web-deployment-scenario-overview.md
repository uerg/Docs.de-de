---
uid: web-forms/overview/deployment/deploying-web-applications-in-enterprise-scenarios/enterprise-web-deployment-scenario-overview
title: 'Webbasierte Unternehmensbereitstellung: Szenarioübersicht | Microsoft-Dokumentation'
author: jrjlee
description: Diese Reihe von Lernprogrammen verwendet eine beispiellösung mit einem realistischen Maß an Komplexität, zusammen mit der ein fiktives Unternehmen-Bereitstellungsszenario, ref-elelement bereitstellen...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/03/2012
ms.topic: article
ms.assetid: aa862153-4cd8-4e33-beeb-abf502c6664f
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/deployment/deploying-web-applications-in-enterprise-scenarios/enterprise-web-deployment-scenario-overview
msc.type: authoredcontent
ms.openlocfilehash: 47bd0a0f7b71a6069d573f1454583cdb832f6b4c
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/03/2018
ms.locfileid: "37366076"
---
<a name="enterprise-web-deployment-scenario-overview"></a>Webbasierte Unternehmensbereitstellung: Szenarioübersicht
====================
durch [Jason Lee](https://github.com/jrjlee)

[PDF herunterladen](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Diese Reihe von Lernprogrammen verwendet eine beispiellösung mit einem realistischen Maß an Komplexität, zusammen mit der ein fiktives Unternehmen-Bereitstellungsszenario, um eine referenzimplementierung bereitzustellen und geben Sie die Aufgaben und exemplarische Vorgehensweisen ein allgemeiner Kontext. In diesem Thema wird beschrieben, die das Szenario des Lernprogramms und führt die beispiellösung.


## <a name="scenario-description"></a>Beschreibung des Szenarios

Fabrikam, Inc., einem fiktiven Unternehmen, ist das Erstellen einer lösungs, mit dem remote Vertriebsteams, speichern und Abrufen von Informationen aus einer Web-Schnittstelle.

Der Application Lifecycle Management (ALM)-Prozesse bei Fabrikam, Inc. erfordern die Lösung bis zu drei serverumgebungen in verschiedenen Phasen der Entwicklung von Softwareupdates bereitgestellt werden:

- Test- oder "Sandkasten" entwicklerumgebung verwenden.
- Ein intranetbasierter Stagingumgebung bereit.
- Eine Internetverbindung-produktionsumgebung.

Jede dieser Umgebungen verfügt über andere Konfiguration und sicherheitsanforderungen und jeweils individuelle Bereitstellung Herausforderungen stellt.

### <a name="the-fabrikam-inc-server-infrastructure"></a>Der Fabrikam, Inc. Server-Infrastruktur

Dies ist die allgemeine Entwicklung und Bereitstellung von Infrastruktur bei Fabrikam, Inc.

![](enterprise-web-deployment-scenario-overview/_static/image1.png)

Der Arbeitsstationen von Entwicklern, die Source-Control-Infrastruktur, die Entwickler-testumgebung und der Stagingumgebung bereit, die alle im Intranet Netzwerk innerhalb der Fabrikam.net-Domäne befinden. Die produktionsumgebung befindet sich in einem Umkreisnetzwerk (auch bekannt als DMZ, demilitarisierte Zone und überwachtes Subnetz bezeichnet), die aus dem Intranetnetzwerk durch eine Firewall getrennt ist. Dies ist ein häufiges Bereitstellungsszenario: Ihre Internet verbundenen Webserver i. d. r. isoliert, von Ihrer internen Server-Infrastruktur durch die Verwendung von Firewalls oder Gatewayservern.

In diesem Beispiel:

- Ein Team Foundation Server (TFS) 2010-Server mit einem separaten Build-Server bietet die quellcodeverwaltung und continuous Integration (CI)-Funktionalität.
- Die Developer-testumgebung umfasst einen Internet Information Services (IIS) 7.5-Web-Server und einem SQL Server 2008 R2-Datenbankserver.
- Die produktionsumgebung enthält mehrere IIS 7.5-Webservern, die von einem Web Farm Framework (WFF) Controllerserver, zusammen mit einem SQL Server 2008 R2-Datenbank-Server synchronisiert. In der Praxis kann der Datenbankserver verwenden Failoverclustering oder Spiegelung zur Verbesserung der Skalierbarkeit und Verfügbarkeit.
- Die Stagingumgebung wurde entwickelt, um die Konfiguration der produktionsumgebung soweit wie möglich zu replizieren.
- Die Firewall- und Richtlinien für die Domänenisolation erlauben keine direkten, automatisierte Bereitstellung über das Intranet mit dem Umkreisnetzwerk.

Die Konfiguration der einzelnen Umgebungen wird ausführlicher beschrieben, in diesem zweiten Tutorial [Konfigurieren von Server-Umgebungen für die Webbereitstellung](../configuring-server-environments-for-web-deployment/configuring-server-environments-for-web-deployment.md).

### <a name="team-roles-for-alm"></a>Für ALM-Rollen

Diese Benutzer sind beim Erstellen, verwalten, erstellen und Veröffentlichen der Projektmappe Contact Manager beteiligt:

- Matt Hink ist ein Entwickler bei Fabrikam, Inc. Er ist Teil des Teams, die die Kontakt-Manager-Lösung mithilfe von Visual Studio 2010 entwickelt. Matt hat vollständige Administratorrechte auf den Servern in der testumgebung für Entwickler, die ihm die Umgebung, um seine Anforderungen konfigurieren können. Er hat auch den Benutzerzugriff auf die Visual Studio 2010-TFS-Instanz, in dem er den Quellcode für die Projektmappe Contact Manager speichert.
- Rob Walters ist ein Server-Administrator für das Entwicklungsteam der Fabrikam, Inc. an. Rob hat Administratorzugriff auf dem TFS-Server, sodass er alle Aspekte von TFS und Team Build konfigurieren kann. Rob Campbell ist auch hat Administratorzugriff auf den Test und staging Webserver und fungiert als Datenbankadministrator (DBA) für die Datenbankserver in der Test- und Stagingumgebungen. Rob wurde Team Build auf dem TFS-Server zum Ausführen dieser Aufgaben konfiguriert:

    - Erstellen und Ausführen von Komponententests für die Anwendung bei jedem einer Datei mit TFS Einchecken. Dies wird CI bezeichnet.
    - Bereitstellen Sie die Kontakt-Manager-Anwendung in der testumgebung automatisch, sobald die Anwendung Komponententests übergibt. Dies umfasst das Veröffentlichen von der Datenbank auf dem Testserver auf der ersten Bereitstellung und alle Updates für die Datenbank nach der erstmaligen Bereitstellung.
    - Bereitstellen Sie die Kontakt-Manager-Anwendung in der Stagingumgebung bereit, in einem Prozess Schritt für Schritt.
    - Erstellen eines Webpakets, das eine Web-Server-Administrator und ein DBA zum Veröffentlichen der Anwendung in der produktionsumgebung verwenden können.
- Lisa Andrews ist ein Server-Administrator für die Bereitstellung von Anwendungen auf den Produktionsservern Fabrikam, Inc. verantwortlich. Sie verfügt über Lesezugriff auf die Freigabe, auf dem TFS Team Build das Webbereitstellungspaket speichert, nachdem sie die Kontakt-Manager-Anwendung erstellt wurde. Sie hat auch Administratorzugriff auf die Produktions-Webservern, damit sie die Anwendung für die Produktion bereitstellen kann. Darüber hinaus fungiert er als DBA, Datenbanken und Datenbank-Updates mit dem Datenbankserver in der produktionsumgebung bereitstellt.

<a id="_The_Contact_Manager"></a>

### <a name="the-contact-manager-solution"></a>Contact Manager-Lösung

Die Projektmappe Contact Manager dient, damit registrierte, angemeldeten Benutzer hinzufügen und Kontaktinformationen über eine Weboberfläche bearbeiten. Contact Manager-Lösung besteht aus vier einzelne Projekte:

![](enterprise-web-deployment-scenario-overview/_static/image2.png)

- **ContactManager.Mvc**. Dies ist ein ASP.NET MVC3-Webanwendungsprojekt, das den Einstiegspunkt für die Projektmappe darstellt. Es bietet einige Funktionen der einfachen Web-Anwendung, wie Benutzern die Möglichkeit zum Erstellen und Anzeigen von Details für sicherheitskontakt bereitstellen. Die Anwendung verwendet einen Windows Communication Foundation (WCF)-Dienst zum Verwalten von Kontakten und eine Datenbank für ASP.NET-Anwendungsdienste zum Verwalten der Authentifizierung und Autorisierung.
- **ContactManager.Database**. Dies ist ein Visual Studio 2010-Datenbankprojekt. Das Projekt definiert das Schema für eine Datenbank, speichert Informationen wenden Sie sich an.
- **ContactManager.Service**. Dies ist ein WCF-Webdienstprojekt. Der WCF macht ein Endpunkt, der Aufrufern ermöglicht, führen Sie zu erstellen, abrufen, aktualisieren und löschen (CRUD) für die Kontakt-Manager-Datenbank. Der Dienst basiert auf der Contact Manager-Datenbank und die ContactManager.Common.dll-Assembly.
- **ContactManager.Common**. Dies ist ein Klassenbibliotheksprojekt. Der WCF-Dienst basiert auf Typen, die in dieser Assembly definiert.

Eine vollständige Überprüfung der Projektmappe und die Anforderungen für die Bereitstellung finden Sie im ersten Tutorial dieser Reihe [Webbereitstellung im Unternehmen](../web-deployment-in-the-enterprise/web-deployment-in-the-enterprise.md).

<a id="_Deployment_Tasks"></a>

## <a name="deployment-tasks"></a>Bereitstellungsaufgaben

Es gibt mehrere unterschiedliche Aufgaben zum Bereitstellen von Anwendungen für unterschiedliche Umgebungen in einer großen Organisation. Dies sind die wichtigsten Aufgaben, die Themen in den Tutorials:

![](enterprise-web-deployment-scenario-overview/_static/image3.png)

Hier ist eine Liste der einzelnen Schritte im Bereitstellungsprozess aus der Perspektive der Benutzer weiter oben in diesem Dokument beschrieben:

1. Alle Mitglieder des Teams überprüfen die Contact Manager-Lösung in Visual Studio 2010, um wichtige bereitstellungsanforderungen und Probleme zu ermitteln.
2. Matt Hink bereitstellen Contact Manager-Lösung direkt aus der Entwicklerarbeitsstation in der Developer-testumgebung, um einen ersten Test Logik, die Bereitstellung durchzuführen.
3. Matt Hink fügt die Anwendung in die quellcodeverwaltung in TFS hinzu.
4. Rob Walters erstellt verschiedene Build-Definitionen für die Projektmappe Contact Manager in Team Build. Eine Builddefinition wird CI verwendet, um die Lösung in die testumgebung für die Entwickler bereitzustellen, wenn ein Benutzer im neuen Code eincheckt. Eine weitere Builddefinition kann Benutzer lösen Sie Bereitstellungen in der Stagingumgebung nach Bedarf.
5. Jedes Mal, wenn ein Benutzer im neuen Code eincheckt, Team Build automatisch die Komponenten der Lösung erstellt, führt Komponententests und die Lösung in der Developer-testumgebung, wenn der Build erfolgreich war und die Komponententests bestanden bereitgestellt.
6. Wenn ein Benutzer eine Bereitstellung in der Stagingumgebung ausgelöst wird, wird die Lösung wird verpackt und in einem Prozess Schritt für Schritt bereitgestellt. Dieser Prozess generiert auch ein Paket für die manuelle Bereitstellung in der produktionsumgebung bereit.
7. Lisa Andrews stellt die Anwendung in der produktionsumgebung bereit, indem Sie manuell importieren des Webpakets, das in Schritt 6 erstellt haben.

### <a name="key-deployment-issues"></a>Wichtige Bereitstellungsproblemen

Markieren Sie Contact Manager-Lösung und Fabrikam, Inc. Szenario verschiedene häufig auftretende Probleme und Herausforderungen, die möglicherweise auftreten, wenn Sie komplexe, unternehmensweite Lösungen bereitstellen. Zum Beispiel:

- Sie müssen zum Bereitstellen von Projekten für mehrere Umgebungen, wie Entwickler oder testumgebungen, staging-Plattformen und Produktionsserver. Die Lösung muss mit anderen Konfigurationseinstellungen für jede Umgebung bereitgestellt werden.
- Sie müssen mehrere abhängige Projekte gleichzeitig im Rahmen eines einzigen Schritt oder automatisierte Build & Deployment-Prozesses bereitstellen.
- Sie müssen auf die Bereitstellung mithilfe eines automatisierten Prozesses können. Sie möchten z. B. einen CI-Prozess zu verwenden, um das Bereitstellen von Webanwendungen in einer Stagingumgebung, wenn Sie neuer Code eingecheckt wird.
- Sie müssen möglicherweise steuern den Bereitstellungsprozess und legen Sie die Bereitstellung-Variablen von außerhalb von Visual Studio, wie Entwickler es unwahrscheinlich, dass die richtigen Konfigurationseinstellungen oder den notwendigen Anmeldeinformationen für jede zielumgebung verfügen.
- Schema-basierte Datenbank-Projekte bereitzustellen und vorhandene Daten bei nachfolgenden Bereitstellungen beibehalten werden sollen.
- In diesem Fall müssen Sie eine Mitgliedschaft-Datenbanken auf einer ad-hoc-Basis bereitstellen, ohne Benutzerkontodaten bereitzustellen. Sie müssen auch das Schema der bereitgestellten Membership-Datenbanken zu aktualisieren, ohne die bestehende Konto-Benutzerdaten zu verlieren.
- Sie müssen bestimmte Dateien oder Ordner ausschließen, wenn Sie Inhalte in verschiedenen zielumgebungen bereitstellen.

Darüber hinaus löst die Bereitstellung zu verwalten, wenn Updates häufige und inkrementelle sind Sie einige zusätzlichen Herausforderungen. Zum Beispiel:

- Sie können Komponententests ausführen, jedes Mal, wenn ein Entwickler im neuen Code eincheckt. Sie möchten nur zum Bereitstellen der Lösung, wenn der Code die Komponententests bestanden.
- Wenn Sie eine Webanwendung in einer Umgebung Staging- oder produktionsumgebung bereitstellen, möchten Sie Umleiten von Benutzern ein *app\_offline.htm* -Datei für die Dauer des Bereitstellungsprozesses.
- Möchten Sie die Bereitstellungsaufgaben zu melden. Während des Bereitstellungsvorgangs sollten e-Mail-Benachrichtigungen der erfolgreichen oder fehlgeschlagenen Bereitstellungen an festgelegte Empfänger senden.
- Wenn es sich bei eine automatisierte Bereitstellung ein Fehler auftritt, sollte während des Bereitstellungsvorgangs wiederholen Sie die aktuelle Bereitstellung oder das vorherige Webpaket stattdessen bereitstellen.

> [!div class="step-by-step"]
> [Zurück](deploying-web-applications-in-enterprise-scenarios.md)
> [Weiter](application-lifecycle-management-from-development-to-production.md)
