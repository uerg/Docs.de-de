---
uid: web-forms/overview/deployment/configuring-server-environments-for-web-deployment/configuring-server-environments-for-web-deployment
title: Konfigurieren von Serverumgebungen für die Webbereitstellung | Microsoft-Dokumentation
author: jrjlee
description: Dieses Tutorial zeigt Ihnen, wie Server-Umgebungen unterstützen nur einem Klick oder automatisierte, websitebereitstellung und Veröffentlichung in verschiedene andere Scen eingerichtet...
ms.author: aspnetcontent
ms.date: 05/04/2012
ms.assetid: 0bf0959b-4ca8-45de-bd13-b15347543b5a
msc.legacyurl: /web-forms/overview/deployment/configuring-server-environments-for-web-deployment/configuring-server-environments-for-web-deployment
msc.type: authoredcontent
ms.openlocfilehash: df40cc9d16b9e6b3fe0b659b405106e68cee62b5
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2018
ms.locfileid: "37835562"
---
<a name="configuring-server-environments-for-web-deployment"></a>Konfigurieren von Serverumgebungen für die Webbereitstellung
====================
durch [Jason Lee](https://github.com/jrjlee)

[PDF herunterladen](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Dieses Tutorial zeigt Ihnen, wie Sie Server-Umgebungen unterstützen nur einem Klick oder automatisierte, websitebereitstellung und Veröffentlichung in verschiedene andere Szenarien einrichten. Das Tutorial enthält Themen, führen Sie durch Ausführen verschiedener Aufgaben, wie das Konfigurieren eines Webservers zur Unterstützung bestimmter Ansätze bei der Bereitstellung und zum Einrichten einer Web Farm Framework (WFF)-Serverfarm, zusammen mit auf Szenarien basierende Übersichten, die bereitstellen End-to-End-Leitfaden auf höherer Ebene.
> 
> Das Lernprogramm verwendet das Fabrikam, Inc.-Bereitstellungsszenario, die in beschriebenen [webbasierte Unternehmensbereitstellung: Szenarioübersicht](../deploying-web-applications-in-enterprise-scenarios/enterprise-web-deployment-scenario-overview.md) als Bezugspunkt für Beispiele und Netzwerkinfrastruktur.
> 
> Ein italienischen Übersetzung mit diesen Lernprogrammen finden Sie unter [ http://www.lucamorelli.it ](http://www.lucamorelli.it).


In diesem Lernprogramm umfasst folgende Themen:

- [Auswählen der richtigen Vorgehensweise zur Webbereitstellung](choosing-the-right-approach-to-web-deployment.md)
- [Szenario: Konfigurieren einer Testumgebung für die Webbereitstellung](scenario-configuring-a-test-environment-for-web-deployment.md)
- [Szenario: Konfigurieren einer Stagingumgebung für die Webbereitstellung](scenario-configuring-a-staging-environment-for-web-deployment.md)
- [Szenario: Konfigurieren einer Produktionsumgebung für die Webbereitstellung](scenario-configuring-a-production-environment-for-web-deployment.md)
- [Konfigurieren eines Webservers für die Web Deploy-Veröffentlichung (Remote-Agent)](configuring-a-web-server-for-web-deploy-publishing-remote-agent.md)
- [Konfigurieren eines Webservers für die Web Deploy-Veröffentlichung (Web Deploy-Handler)](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler.md)
- [Konfigurieren eines Webservers für die Web Deploy-Veröffentlichung (Offlinebereitstellung)](configuring-a-web-server-for-web-deploy-publishing-offline-deployment.md)
- [Konfigurieren eines Datenbankservers für die Web Deploy-Veröffentlichung](configuring-a-database-server-for-web-deploy-publishing.md)
- [Erstellen einer Serverfarm mit dem Webfarmframework](creating-a-server-farm-with-the-web-farm-framework.md)
- [Konfigurieren von Bereitstellungseigenschaften für eine Zielumgebung](configuring-deployment-properties-for-a-target-environment.md)

Das erste Thema [Entscheidung zur Webbereitstellung rechts](choosing-the-right-approach-to-web-deployment.md), beschreibt die wichtigsten Ansätze Sie das Veröffentlichen von Webanwendungen mithilfe der Internetinformationsdienste (Internet Information Services, IIS)-Webbereitstellungstool (Web Deploy können) 2.0. Ebenfalls aufgeführt sind die Szenarien, die auf jeden Ansatz zuordnen. Von hier aus jedes Szenario Thema bietet einen Überblick über die Aufgaben, die Sie durchführen müssen, und identifiziert die Themen über arbeiten, können Sie das Ausführen dieser Aufgaben benötigen.

Bei Verwendung in beschriebenen Ansatz der geteilten Projekt Datei [Verständnis des Prozesses erstellen](../web-deployment-in-the-enterprise/understanding-the-build-process.md) zum Erstellen und Bereitstellen Ihrer Lösung, die das letzte Thema, [Konfigurieren von Bereitstellungseigenschaften für eine Zielumgebung](configuring-deployment-properties-for-a-target-environment.md), beschreibt, wie umgebungsspezifisches Projektdateien für die Bereitstellung in unterschiedliche zielumgebungen zu konfigurieren.

## <a name="key-technologies"></a>Schlüsseltechnologien

In diesem Tutorial geht es um die Produkte und Technologien zu verwenden, um die webbereitstellung zu unterstützen:

- IIS 7.5
- Web Deploy-2.x
- WFF 2.x
- IIS-Web-Verwaltungsdienst (WMSvc) verwenden.

Das Tutorial betrifft auch für die Verwendung von Windows Server 2008 R2, SQL Server 2008 R2, ASP.NET 4.0 und ASP.NET MVC 3.

## <a name="other-tutorials-in-this-series"></a>Andere Tutorials in dieser Reihe

Dies ist Teil einer Reihe von fünf Lernprogrammen auf Unternehmensniveau webbereitstellung. Dies sind die anderen Tutorials der Reihe:

- [Bereitstellen von Webanwendungen in Unternehmensszenarien](../deploying-web-applications-in-enterprise-scenarios/deploying-web-applications-in-enterprise-scenarios.md). Dieser einführenden Inhalt enthält die kontextbezogenen Hintergrundinformationen zum der Tutorial-Reihe. Das tutorialszenario beschrieben und veranschaulicht, wie die Aufgaben und exemplarische Vorgehensweisen beschrieben, die in der gesamten Reihe in einem größeren Application Lifecycle Management (ALM)-Prozess passen.
- [Webbereitstellung im Unternehmen](../web-deployment-in-the-enterprise/web-deployment-in-the-enterprise.md). Dieses Tutorial bietet eine grundlegende Einführung in Projektdateien Microsoft Build Engine (MSBuild), die Veröffentlichung Webpipeline, Web Deploy und andere verwandten Technologien. Es wird erläutert, wie diese Tools zusammen verwenden werden können, um komplexe Bereitstellungsprozesse zu verwalten.
- [Konfigurieren von Team Foundation Server für die Webbereitstellung](../configuring-team-foundation-server-for-web-deployment/configuring-team-foundation-server-for-web-deployment.md). In diesem Tutorial wird beschrieben, wie so konfigurieren Sie Team Foundation Server (TFS) zur Unterstützung verschiedener Bereitstellungsszenarien, einschließlich der automatisierten Bereitstellung im Rahmen einer continuous Integration (CI) und bestimmte Builds Bereitstellungen manuell ausgelöst wird.
- [Erweiterte webbasierte Unternehmensbereitstellung](../advanced-enterprise-web-deployment/advanced-enterprise-web-deployment.md). In diesem Tutorial wird beschrieben, wie verschiedene erweiterte Bereitstellung, z. B. Anpassen von datenbankbereitstellungen für mehrere Umgebungen und Ausschließen von Dateien und Ordner von der Bereitstellung von Webanwendungen während der Bereitstellung und Aufgaben .

> [!div class="step-by-step"]
> [Nächste](choosing-the-right-approach-to-web-deployment.md)
