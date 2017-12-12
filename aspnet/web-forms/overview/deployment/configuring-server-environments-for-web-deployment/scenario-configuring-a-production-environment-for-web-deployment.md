---
uid: web-forms/overview/deployment/configuring-server-environments-for-web-deployment/scenario-configuring-a-production-environment-for-web-deployment
title: "Szenario: Eine Produktionsumgebung für die Bereitstellung konfigurieren | Microsoft Docs"
author: jrjlee
description: "In diesem Thema wird beschrieben, ein Webdienst-Bereitstellungsszenario für eine produktionsumgebung und erläutert, die Aufgaben, die Sie ausführen, um ein ähnliches einrichten müssen..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 2e861511-450e-4752-a61e-4a01933f9b6e
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/configuring-server-environments-for-web-deployment/scenario-configuring-a-production-environment-for-web-deployment
msc.type: authoredcontent
ms.openlocfilehash: d5574ee353ff41205e9029e4aa5d139a5aa0e959
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="scenario-configuring-a-production-environment-for-web-deployment"></a>Szenario: Konfigurieren einer produktiven Umgebung für die Bereitstellung
====================
durch [Jason Lee](https://github.com/jrjlee)

[PDF herunterladen](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> In diesem Thema wird beschrieben, ein Webdienst-Bereitstellungsszenario für eine produktionsumgebung und erläutert, die Aufgaben, die Sie ausführen, um eine ähnliche Umgebung einrichten müssen.


Die produktionsumgebung ist das endgültige Ziel für eine Webanwendung oder einer Website. Zu diesem Zeitpunkt die Anwendung wurde durch Tests in einer Stagingumgebung bereitgestellt wurde und ist jetzt kann es losgehen "live." Die Merkmale einer produktiven Umgebung können entsprechend den Natur und Zweck der Webinhalte, die Größe des Unternehmens, Ihre Zielgruppe und viele andere Faktoren stark variieren. In einem Szenario mit Enterprise-Ebene kann die produktionsumgebung diese Merkmale haben:

- Die Umgebung besteht aus mehreren Lastenausgleich Webserver und eine oder mehrere Datenbankserver, häufig mit Failoverclustering und datenbankspiegelung.
- Wenn die Umgebung über Internetzugriff verfügt, ist es wahrscheinlich aus dem internen Netzwerk getrennt werden. Möglicherweise in einem anderen Subnetz in einem Umkreisnetzwerk, möglicherweise in einer anderen Domäne und kann es sich um eine vollkommen andere Netzwerkinfrastruktur handeln.
- Entwickler und Build-Server-Prozesskonten werden mit hoher Wahrscheinlichkeit keine Administratorrechte auf den Produktionsservern.
- Änderungen an Anwendungen sind auf die seltener als Test- oder stagingbereitstellungen bereitgestellt.

> [!NOTE]
> Dezentrales Skalieren einer datenbankbereitstellung auf mehreren Servern ist nicht Gegenstand dieses Lernprogramm. Weitere Informationen zu diesem Bereich, finden Sie in [SQL Server-Onlinedokumentation](https://technet.microsoft.com/en-us/library/ms130214.aspx).


Z. B. in unserer [lernprogrammszenario](../deploying-web-applications-in-enterprise-scenarios/enterprise-web-deployment-scenario-overview.md), Team Build-Server enthält alle Builddefinitionen, mit denen Benutzer die Projektmappe Contact Manager erstellt und in einer Stagingumgebung in einem einzigen Schritt bereitgestellt haben. Sobald die Anwendung bis hin zur Produktion, aufgrund der Einschränkungen von sicherheitsanforderungen und die Netzwerkinfrastruktur bereitgestellt werden kann muss der Administrator der Produktion-Umgebung manuell kopieren Sie das Webpaket auf einem Produktionsserver für Web und importieren es über Internet Information Services (IIS) Manager.

![](scenario-configuring-a-production-environment-for-web-deployment/_static/image1.png)

## <a name="solution-overview"></a>Lösungsübersicht

In diesem Szenario können Sie diese Fakten aus eine Analyse der bereitstellungsanforderungen ableiten:

- Aufgrund von sicherheitsbeschränkungen und die Netzwerkkonfiguration kann nicht die produktionsumgebung zur Unterstützung von nur einem Klick oder automatisierte Bereitstellung konfiguriert werden. Offline-Bereitstellung ist die einzige realisierbare Ansatz in diesem Szenario.
- Die produktionsumgebung umfasst mehrere Webserver, damit Sie Web Farm Framework (WFF) verwenden können, um eine Serverfarm zu erstellen. Bei diesem Ansatz muss der Administrator nur die Anwendung auf einem Web-Server (dem primären Server) importieren und WFF wird die Bereitstellung für alle anderen Webserver in der produktionsumgebung repliziert.

Die folgenden Themen enthalten alle Informationen, die Sie benötigen, um diese Aufgaben ausführen:

- [Erstellen Sie eine Serverfarm mit Webfarmframework](configuring-a-database-server-for-web-deploy-publishing.md). Dieses Thema beschreibt, wie zum Erstellen und Konfigurieren einer Serverfarm WFF, sodass Web Platform Produkte und Komponenten, Konfigurationseinstellungen und Websites und Anwendungen auf mehrere Load Balancing Webserver repliziert werden.
- [Konfigurieren Sie einen Webserver für Web Deploy-Veröffentlichung (Bereitstellung Offline)](configuring-a-web-server-for-web-deploy-publishing-offline-deployment.md). Dieses Thema beschreibt das Erstellen eines Webservers, mit der können Administratoren importieren und Bereitstellen von Webpaketen manuell, beginnend mit einem bereinigten Build von Windows Server 2008 R2.
- [Konfigurieren eines Datenbankservers für Web Deploy-Veröffentlichung](configuring-a-database-server-for-web-deploy-publishing.md). Dieses Thema beschreibt, wie einen Datenbankserver zur Unterstützung von Remotezugriff und Bereitstellung, beginnend bei einer Standardinstallation von SQL Server 2008 R2 zu konfigurieren.

## <a name="further-reading"></a>Weiterführende Themen

Anleitungen zum Konfigurieren einer testumgebung typische Entwickler finden Sie unter [Szenario: Konfigurieren einer Umgebung testen für Webbereitstellung](scenario-configuring-a-test-environment-for-web-deployment.md). Anleitungen zum Konfigurieren einer typischen Stagingumgebung finden Sie unter [Szenario: Konfigurieren einer Staging-Umgebung für die Bereitstellung](scenario-configuring-a-staging-environment-for-web-deployment.md).

>[!div class="step-by-step"]
[Zurück](scenario-configuring-a-staging-environment-for-web-deployment.md)
[Weiter](configuring-a-web-server-for-web-deploy-publishing-remote-agent.md)
