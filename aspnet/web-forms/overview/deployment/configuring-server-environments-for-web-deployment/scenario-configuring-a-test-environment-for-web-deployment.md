---
uid: web-forms/overview/deployment/configuring-server-environments-for-web-deployment/scenario-configuring-a-test-environment-for-web-deployment
title: 'Szenario: Konfigurieren einer Testumgebung für die Bereitstellung | Microsoft Docs'
author: jrjlee
description: In diesem Thema wird beschrieben, ein Webdienst-Bereitstellungsszenario für Entwickler oder testumgebungen und erläutert, die Aufgaben, die Sie zum Einrichten einer Si durchführen müssen...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 44a22ac7-1fc7-4174-b946-c6129fb6a19b
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/configuring-server-environments-for-web-deployment/scenario-configuring-a-test-environment-for-web-deployment
msc.type: authoredcontent
ms.openlocfilehash: 2976be642815e715ac19bd9db34485cf5474cb32
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
---
<a name="scenario-configuring-a-test-environment-for-web-deployment"></a>Szenario: Konfigurieren einer Testumgebung für die Bereitstellung
====================
durch [Jason Lee](https://github.com/jrjlee)

[PDF herunterladen](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> In diesem Thema wird beschrieben, ein Webdienst-Bereitstellungsszenario für Entwickler oder testumgebungen und erläutert, die Aufgaben, die Sie ausführen, um eine ähnliche Umgebung einrichten müssen.


Wenn Entwickler in Webanwendungen arbeiten zu können, sind sie häufig Zugriff auf eine Server-Umgebung gewährt, die sie verwenden können, um Änderungen an ihren Anwendungen in einer realistischen testen. Diese Art von Entwicklungs-oder testumgebung weist normalerweise folgende Merkmale auf:

- Die Umgebung besteht aus einem einzelnen Webserver und ein einzelner Datenbankserver.
- Die Entwickler werden in der Regel Administratorrechte auf den Servern, mit denen sie die Umgebung die Anforderungen ihrer Anwendungen konfigurieren können.
- Änderungen an Anwendungen in regelmäßigen Abständen, bereitgestellt werden, damit die Umgebung zur Unterstützung von einstufiger muss oder automatisierte Bereitstellung.

Z. B. in unserer [lernprogrammszenario](../deploying-web-applications-in-enterprise-scenarios/enterprise-web-deployment-scenario-overview.md), Matt Hink ist ein Entwickler bei Fabrikam, Inc. Matt für die Projektmappe Contact Manager arbeitet und regelmäßig Änderungen in einer testumgebung bereitgestellt werden muss. Matt ist ein Administrator auf dem Test-Webserver und dem Testserver für die Datenbank. Zunächst muss Matt, damit die Projektmappe direkt in der testumgebung bereitgestellt werden.

![](scenario-configuring-a-test-environment-for-web-deployment/_static/image1.png)

Wenn Arbeitsaufgaben verknüpfen im Verlauf der Arbeit und Entwickler im Team, der Kontakt-Manager, die Lösung für die fortlaufende Integration (CI) in Team Foundation Server (TFS) konfiguriert ist. Wenn ein Entwickler Inhalt eincheckt, sollten Team Build erstellen Sie die Projektmappe, Ausführen von Komponententests und automatisch die Projektmappe bereitstellen, in der testumgebung.

![](scenario-configuring-a-test-environment-for-web-deployment/_static/image2.png)

## <a name="solution-overview"></a>Lösungsübersicht

Die testumgebung einstufiger unterstützen muss, oder automatisierte Bereitstellung auf einem Remotecomputer befindet, müssen Sie eine Auswahl von zwei Hauptansätze. Sie haben folgende Möglichkeiten:

- Konfigurieren des Test-Servers zur Unterstützung einer Bereitstellung mit der Webbereitstellungs-Agent-Dienst (der "remote-Agent").
- Konfigurieren Sie den Test-Webserver für die Bereitstellung der Web Deploy-Handler zu unterstützen.

> [!NOTE]
> Sie können auch [Web Deploy bei Bedarf](https://technet.microsoft.com/library/ee517345(WS.10).aspx) ("temp Agent"). Dies ähnelt der remote-Agent-Ansatz hinsichtlich der Anforderungen und Einschränkungen.


In diesem Fall müssen die Entwickler verfügen über Administratorrechte auf dem Zielserver, und die testumgebung unterliegt nicht der strenge sicherheitseinschränkungen, daher ist die erste Wahl so konfigurieren Sie die Test-Web-Server zur Unterstützung einer Bereitstellung mit dem remote-Agent. Dies ist weniger komplex und erfordert weniger Erstkonfiguration als beim Bereitstellen von Web-Handler-Ansatz. Sie müssen auch Ihr Datenbankserver zur Unterstützung von Remotezugriff und die Bereitstellung konfigurieren.

Die folgenden Themen enthalten alle Informationen, die Sie benötigen, um diese Aufgaben ausführen:

- [Konfigurieren Sie einen Webserver für Web Deploy-Veröffentlichung (Remote-Agent)](configuring-a-web-server-for-web-deploy-publishing-remote-agent.md). In diesem Thema wird beschrieben, wie einen Webserver zu erstellen, der von Web Deploy veröffentlichen, indem Sie den remote-Agent-Ansatz, beginnend mit einem bereinigten Build von Windows Server 2008 R2 unterstützt wird.
- [Konfigurieren eines Datenbankservers für Web Deploy-Veröffentlichung](configuring-a-database-server-for-web-deploy-publishing.md). Dieses Thema beschreibt, wie einen Datenbankserver zur Unterstützung von Remotezugriff und Bereitstellung, beginnend bei einer Standardinstallation von SQL Server 2008 R2 zu konfigurieren.

## <a name="further-reading"></a>Weiterführende Themen

Anleitungen zum Konfigurieren einer typischen Stagingumgebung finden Sie unter [Szenario: Konfigurieren einer Staging-Umgebung für die Bereitstellung](scenario-configuring-a-staging-environment-for-web-deployment.md). Anleitungen zum Konfigurieren einer typischen produktionsumgebung finden Sie unter [Szenario: Konfigurieren einer produktiven Umgebung für die Bereitstellung von](scenario-configuring-a-production-environment-for-web-deployment.md).

> [!div class="step-by-step"]
> [Zurück](choosing-the-right-approach-to-web-deployment.md)
> [Weiter](scenario-configuring-a-staging-environment-for-web-deployment.md)
