---
uid: web-forms/overview/deployment/configuring-server-environments-for-web-deployment/configuring-server-environments-for-web-deployment
title: "Konfigurieren von Serverumgebungen für Web Deploy | Microsoft Docs"
author: jrjlee
description: "In diesem Lernprogramm erfahren Sie, wie serverumgebungen unterstützen nur einem Klick oder automatisierte Website bereitstellen und die Publishing in verschiedene andere Scen eingerichtet..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 0bf0959b-4ca8-45de-bd13-b15347543b5a
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/configuring-server-environments-for-web-deployment/configuring-server-environments-for-web-deployment
msc.type: authoredcontent
ms.openlocfilehash: 8a07d283e3e4344e5513152cf760ac90481d9f4b
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="configuring-server-environments-for-web-deployment"></a>Konfigurieren von Serverumgebungen für die Bereitstellung
====================
durch [Jason Lee](https://github.com/jrjlee)

[PDF herunterladen](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> In diesem Lernprogramm erfahren Sie, wie Server-Umgebungen zu nur einem Klick oder automatisierte Unterstützung, die Bereitstellung der Website und die Veröffentlichung in verschiedenen Situationen eingerichtet. Das Lernprogramm enthält Themen, um führen Sie durch verschiedene Aufgaben, wie die Konfiguration von einem Webserver zur Unterstützung bestimmter Ansätze bei der Bereitstellung und das Einrichten einer Serverfarm Web Farm Framework (WFF) zusammen mit szenariobasierte Übersichten, die bereitstellen einen groben Anhaltspunkt für End-to-End.
> 
> Das Lernprogramm verwendet das Fabrikam, Inc.-Bereitstellungsszenario, die in beschriebenen [Web Unternehmensbereitstellung: Szenarioübersicht](../deploying-web-applications-in-enterprise-scenarios/enterprise-web-deployment-scenario-overview.md) als Bezugspunkt für Beispiele und Netzwerkinfrastruktur.
> 
> Für einen italienischen Übersetzung mit diesen Lernprogrammen, besuchen Sie [http://www.lucamorelli.it](http://www.lucamorelli.it).


Dieses Lernprogramm umfasst die folgenden Themen:

- [Auswählen des richtigen Ansatzes zur Webbereitstellung](choosing-the-right-approach-to-web-deployment.md)
- [Szenario: Konfigurieren einer Testumgebung für die Bereitstellung](scenario-configuring-a-test-environment-for-web-deployment.md)
- [Szenario: Konfigurieren einer Stagingumgebung für die Bereitstellung](scenario-configuring-a-staging-environment-for-web-deployment.md)
- [Szenario: Konfigurieren einer produktiven Umgebung für die Bereitstellung](scenario-configuring-a-production-environment-for-web-deployment.md)
- [Konfigurieren eines Webservers für Web Deploy-Veröffentlichung (Remote-Agent)](configuring-a-web-server-for-web-deploy-publishing-remote-agent.md)
- [Konfigurieren einen Webserver für das Web Deploy-Veröffentlichung (Web Deploy-Handler)](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler.md)
- [Konfigurieren eines Webservers für Web Deploy-Veröffentlichung (Offline Bereitstellung)](configuring-a-web-server-for-web-deploy-publishing-offline-deployment.md)
- [Konfigurieren einen Datenbankserver für Web Deploy-Veröffentlichung](configuring-a-database-server-for-web-deploy-publishing.md)
- [Erstellen eine Serverfarm mit Webfarmframework](creating-a-server-farm-with-the-web-farm-framework.md)
- [Konfigurieren Bereitstellungseigenschaften für eine Zielumgebung](configuring-deployment-properties-for-a-target-environment.md)

Das erste Thema [Auswählen der rechts Ansatz für die Webbereitstellung](choosing-the-right-approach-to-web-deployment.md), beschreibt die wichtigsten Ansätze Sie zum Veröffentlichen von Webanwendungen mithilfe der Internetinformationsdienste (Internet Information Services, IIS)-Webbereitstellungstool (Web Deploy können) 2.0. Außerdem ermittelt es für die Szenarien, die jeder Ansatz zuordnen. Von hier aus jedes Szenario Thema bietet einen allgemeinen Überblick über die Aufgaben, die Sie ausführen müssen und identifiziert die Themen, die, denen Sie benötigen, über arbeiten, können Sie diese Aufgaben unabhängig.

Bei Verwendung der Teilung Datei Herangehensweise beschrieben [Verständnis des Prozesses erstellen](../web-deployment-in-the-enterprise/understanding-the-build-process.md) zum Erstellen und Bereitstellen der Projektmappe das letzte Thema [Konfigurieren von Bereitstellungseigenschaften für eine Zielumgebung](configuring-deployment-properties-for-a-target-environment.md), beschreibt das umgebungsspezifische Projektdateien für die Bereitstellung in unterschiedliche zielumgebungen konfigurieren.

## <a name="key-technologies"></a>Schlüsseltechnologien

Dieses Lernprogramm konzentriert sich auf wie dieser Produkte und Technologien verwenden, um Web Deploy zu unterstützen:

- IIS 7.5
- Web Deploy 2.x
- Bereitzustellendes WFF 2.x
- IIS-Web-Verwaltungsdienst (WMSvc)

Das Lernprogramm erwähnt auch die Verwendung von Windows Server 2008 R2, SQL Server 2008 R2, ASP.NET 4.0 und ASP.NET MVC 3.

## <a name="other-tutorials-in-this-series"></a>Weitere Lernprogramme in dieser Serie

Dies bildet einen Teil einer Reihe von fünf Lernprogramme auf Unternehmensebene zur Bereitstellung. Dies sind die anderen Lernprogramme in der Reihe:

- [Bereitstellen von Webanwendungen in Enterprise-Szenarios](../deploying-web-applications-in-enterprise-scenarios/deploying-web-applications-in-enterprise-scenarios.md). Diese einführenden Inhalt enthält kontextbezogenen Hintergrund für die Reihe von Lernprogrammen. Das Szenario des Lernprogramme beschrieben und veranschaulicht, wie die Aufgaben und exemplarische Vorgehensweisen beschrieben, die in der gesamten Reihe in einen größeren Application Lifecycle Management (ALM) Prozess passen.
- [Die webbereitstellung im Unternehmen](../web-deployment-in-the-enterprise/web-deployment-in-the-enterprise.md). Dieses Lernprogramm enthält eine grundlegende Einführung in Microsoft Build Engine (MSBuild)-Projektdateien, die Publishing Web-Pipeline, Web Deploy und anderen verwandten Technologien. Es wird erläutert, wie diese Tools gemeinsam verwendet werden können, um komplexe-bereitstellungstechnologien zu verwalten.
- [Konfigurieren von Team Foundation Server für die Bereitstellung](../configuring-team-foundation-server-for-web-deployment/configuring-team-foundation-server-for-web-deployment.md). In diesem Lernprogramm wird beschrieben, wie zum Konfigurieren von Team Foundation Server (TFS) zur Unterstützung der verschiedenen Bereitstellungsszenarien, einschließlich automatisierter Bereitstellung im Rahmen eines Prozesses für die fortlaufende Integration (CI) und Bereitstellungen von bestimmte Builds manuell ausgelöst wird.
- [Erweiterte Web Unternehmensbereitstellung](../advanced-enterprise-web-deployment/advanced-enterprise-web-deployment.md). In diesem Lernprogramm wird beschrieben, wie verschiedene erweiterte Bereitstellung, wie Datenbank-Bereitstellungen für mehrere Umgebungen anpassen, Ausschließen von Dateien und Ordner von der Bereitstellung und offline-Webanwendungen, die während des Bereitstellungsvorgangs Aufgaben .

>[!div class="step-by-step"]
[Nächste](choosing-the-right-approach-to-web-deployment.md)
