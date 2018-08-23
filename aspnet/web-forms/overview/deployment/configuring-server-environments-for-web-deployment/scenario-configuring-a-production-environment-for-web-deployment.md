---
uid: web-forms/overview/deployment/configuring-server-environments-for-web-deployment/scenario-configuring-a-production-environment-for-web-deployment
title: 'Szenario: Konfigurieren einer Produktionsumgebung für die Webbereitstellung | Microsoft-Dokumentation'
author: jrjlee
description: In diesem Thema wird beschrieben, einem typischen Bereitstellungsszenario für eine produktionsumgebung und erläutert, die Aufgaben, die Sie erledigen, um eine ähnliche einrichten müssen...
ms.author: riande
ms.date: 05/04/2012
ms.assetid: 2e861511-450e-4752-a61e-4a01933f9b6e
msc.legacyurl: /web-forms/overview/deployment/configuring-server-environments-for-web-deployment/scenario-configuring-a-production-environment-for-web-deployment
msc.type: authoredcontent
ms.openlocfilehash: 3627d18e1b102c574726282780239464be04c04b
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2018
ms.locfileid: "41831273"
---
<a name="scenario-configuring-a-production-environment-for-web-deployment"></a>Szenario: Konfigurieren einer Produktionsumgebung für die Webbereitstellung
====================
durch [Jason Lee](https://github.com/jrjlee)

[PDF herunterladen](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> In diesem Thema wird beschrieben, einem typischen Bereitstellungsszenario für eine produktionsumgebung und erläutert, die Aufgaben, die Sie ausführen, um eine ähnliche Umgebung einrichten müssen.


Die produktionsumgebung ist das endgültige Ziel für eine Webanwendung oder eine Website. Zu diesem Zeitpunkt Ihre Anwendung wurde durch Tests in einer Stagingumgebung bereitgestellt wurde und ist "online geschaltet." Die Merkmale einer produktionsumgebung können gemäß der Art und den Zweck Ihrer Web-Inhalte, die Größe Ihrer Organisation, Ihre Zielgruppe und viele andere Faktoren variieren. In einem Unternehmen-Szenario kann die produktionsumgebung diese Merkmale aufweisen:

- Die Umgebung besteht aus mehreren Webservern auf der Lastenausgleich und eine oder mehrere Datenbankserver häufig mit Failoverclustering und datenbankspiegelung.
- Wenn die Umgebung über Internetzugriff verfügt, ist es wahrscheinlich aus dem internen Netzwerk getrennt werden. Möglicherweise in einem anderen Subnetz in einem Umkreisnetzwerk, möglicherweise in einer anderen Domäne, und möglicherweise in einer ganz anderen Netzwerkinfrastruktur.
- Entwickler und Build-Server-Prozesskonten sind sehr unwahrscheinlich ist, auf den Produktionsservern die über Administratorrechte verfügen.
- Änderungen an Anwendungen werden auf die seltener als Test- oder staging-Bereitstellungen bereitgestellt.

> [!NOTE]
> Horizontales Skalieren einer datenbankbereitstellung auf mehreren Servern ist, würde den Rahmen dieses Tutorials. Weitere Informationen zu diesem Bereich, finden Sie in [SQL Server-Onlinedokumentation](https://technet.microsoft.com/library/ms130214.aspx).


Z. B. in unserer [lernprogrammszenario](../deploying-web-applications-in-enterprise-scenarios/enterprise-web-deployment-scenario-overview.md), ein Team Build-Server enthält die Build-Definitionen, mit denen Benutzer die Projektmappe Contact Manager erstellen und in einer Stagingumgebung in einem einzigen Schritt bereitstellen. Wenn die Anwendung bereit für die Bereitstellung zur Produktion, aufgrund der Einschränkungen von sicherheitsanforderungen und der Netzwerkinfrastruktur, ist muss der Produktion umgebungsadministrator manuell kopieren des Pakets auf einem Produktionswebserver und importieren Es wird über Internet Information Services (IIS) Manager.

![](scenario-configuring-a-production-environment-for-web-deployment/_static/image1.png)

## <a name="solution-overview"></a>Übersicht über die Lösung

In diesem Szenario können Sie diese Fakten aus einer Analyse der bereitstellungsanforderungen hergeleitet werden:

- Sie können nicht aufgrund von sicherheitseinschränkungen und die Netzwerkkonfiguration die produktionsumgebung zur Unterstützung von nur einem Klick oder automatisierte Bereitstellung konfigurieren. Offline-Bereitstellung ist die einzige Möglichkeit in diesem Szenario.
- Die produktionsumgebung enthält mehrere Webserver, damit Sie das Web Farm Framework (WFF) verwenden können, um eine Serverfarm zu erstellen. Mit diesem Ansatz muss des Administrators nur den import der Anwendung auf einem Web-Server (der primäre Server) und WFF wird die Bereitstellung auf allen anderen Webservern in der produktionsumgebung replizieren.

In diesen Themen enthalten alle Informationen, die Sie benötigen, um diese Aufgaben ausführen:

- [Erstellen Sie eine Serverfarm mit Webfarmframework](configuring-a-database-server-for-web-deploy-publishing.md). In diesem Thema wird beschrieben, wie zum Erstellen und konfigurieren eine Serverfarm mit WFF, sodass Web Platform-Produkte und Komponenten, Konfigurationseinstellungen und Websites und Anwendungen über mehrere Lastenausgleich Webserver hinweg repliziert werden.
- [Konfigurieren Sie einen Webserver für Web Deploy-Veröffentlichung (Offlinebereitstellung)](configuring-a-web-server-for-web-deploy-publishing-offline-deployment.md). Dieses Thema beschreibt die Erstellung von einem Webserver, mit dem Administratoren importieren und Bereitstellen von Webpaketen manuell einen bereinigten Build von Windows Server 2008 R2 ab.
- [Konfigurieren eines Datenbankservers aus, für Web Deploy-Veröffentlichung](configuring-a-database-server-for-web-deploy-publishing.md). Dieses Thema beschreibt, wie Sie einen Datenbankserver zur Unterstützung von Remotezugriff und -Bereitstellung, beginnend bei einer Standardinstallation von SQL Server 2008 R2 zu konfigurieren.

## <a name="further-reading"></a>Weiterführende Themen

Anleitungen zum Konfigurieren einer testumgebung für die typischen Entwicklers finden Sie unter [Szenario: Konfigurieren einer Testumgebung für die Webbereitstellung](scenario-configuring-a-test-environment-for-web-deployment.md). Anleitungen zum Konfigurieren einer typischen staging-Umgebung finden Sie unter [Szenario: Konfigurieren von einer Staging-Umgebung für die Webbereitstellung](scenario-configuring-a-staging-environment-for-web-deployment.md).

> [!div class="step-by-step"]
> [Zurück](scenario-configuring-a-staging-environment-for-web-deployment.md)
> [Weiter](configuring-a-web-server-for-web-deploy-publishing-remote-agent.md)
