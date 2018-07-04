---
uid: web-forms/overview/deployment/configuring-server-environments-for-web-deployment/scenario-configuring-a-test-environment-for-web-deployment
title: 'Szenario: Konfigurieren einer Testumgebung für die Webbereitstellung | Microsoft-Dokumentation'
author: jrjlee
description: In diesem Thema wird beschrieben, ein typisches Web-Bereitstellungsszenario für Entwickler oder testumgebungen und erläutert, die Aufgaben, die Sie zum Einrichten eines servicevorfalls durchführen müssen...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 44a22ac7-1fc7-4174-b946-c6129fb6a19b
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/deployment/configuring-server-environments-for-web-deployment/scenario-configuring-a-test-environment-for-web-deployment
msc.type: authoredcontent
ms.openlocfilehash: f8636fab82b63edab50fc13ae32f4dd536f133ff
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/03/2018
ms.locfileid: "37382493"
---
<a name="scenario-configuring-a-test-environment-for-web-deployment"></a>Szenario: Konfigurieren einer Testumgebung für die Webbereitstellung
====================
durch [Jason Lee](https://github.com/jrjlee)

[PDF herunterladen](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> In diesem Thema wird beschrieben, ein typisches Web-Bereitstellungsszenario für Entwickler oder testumgebungen und erläutert, die Aufgaben, die Sie ausführen, um eine ähnliche Umgebung einrichten müssen.


Wenn Entwickler auf Webanwendungen arbeiten zu können, sind sie häufig Zugriff auf eine Server-Umgebung gewährt, die sie verwenden können, um Änderungen für ihre Anwendungen in einer realistischen Umgebung zu testen. Diese Art von Entwicklungs-oder testumgebung hat in der Regel folgende Eigenschaften:

- Die Umgebung besteht aus einem einzelnen Web-Server und einem einzelnen Datenbankserver.
- Die Entwickler werden in der Regel Administratorrechte auf den Servern, auf die Umgebung die Anforderungen ihrer Anwendungen konfigurieren zu können.
- Änderungen an Anwendungen in regelmäßigen Abständen, bereitgestellt werden, damit die Umgebung zur Unterstützung von einem Schritt muss oder die automatisierte Bereitstellung.

Z. B. in unserer [lernprogrammszenario](../deploying-web-applications-in-enterprise-scenarios/enterprise-web-deployment-scenario-overview.md), Matt Hink ist Entwickler bei Fabrikam, Inc. Matt arbeitet an der Contact Manager-Lösung und muss regelmäßig Änderungen in einer testumgebung bereitzustellen. Matt ist ein Administrator auf dem Test-Webserver und dem Testserver für die Datenbank. Anfangs muss Matt, um die Lösung direkt in die testumgebung bereitstellen zu können.

![](scenario-configuring-a-test-environment-for-web-deployment/_static/image1.png)

Funktionsweise von verknüpfen verläuft und mehr Entwickler im Team, das Contact Manager-Lösung für continuous Integration (CI) in Team Foundation Server (TFS) konfiguriert ist. Wenn Entwickler Inhalte eincheckt, sollte Team Build erstellen Sie die Projektmappe, alle Komponententests ausführen und automatisch die Lösung bereitstellen, in die testumgebung.

![](scenario-configuring-a-test-environment-for-web-deployment/_static/image2.png)

## <a name="solution-overview"></a>Übersicht über die Lösung

Die testumgebung muss Schritt für Schritt zu unterstützen oder die automatisierte Bereitstellung von einem Remotecomputer befindet, müssen Sie eine Auswahl von zwei Hauptansätze. Sie haben folgende Möglichkeiten:

- Konfigurieren des Test-Servers zur Unterstützung der Bereitstellung mithilfe der Webbereitstellungs-Agent-Dienst ("remote-Agent").
- Konfigurieren Sie den Test-Webserver für die Bereitstellung der Web Deploy-Handler zu unterstützen.

> [!NOTE]
> Außerdem können Sie [Web Deploy bei Bedarf](https://technet.microsoft.com/library/ee517345(WS.10).aspx) ("temporäre Agent" bezeichnet). Dies ist vergleichbar mit dem remote-Agent-Ansatz in Bezug auf die Anforderungen und Einschränkungen.


In diesem Fall die Entwickler über Administratorrechte verfügen, auf dem Zielserver, und die testumgebung unterliegt nicht der strenge sicherheitseinschränkungen, daher ist die logische Wahl Testwebserver zur Unterstützung einer Bereitstellung mit dem remote-Agent zu konfigurieren. Dies ist weniger komplex und erfordert weniger anfängliche Konfiguration als der Ansatz WebHandler bereitstellen. Sie müssen auch so konfigurieren Sie Ihren Datenbankserver zur Unterstützung von Remotezugriff und Bereitstellung.

In diesen Themen enthalten alle Informationen, die Sie benötigen, um diese Aufgaben ausführen:

- [Konfigurieren Sie einen Webserver für Web Deploy-Veröffentlichung (Remote-Agent)](configuring-a-web-server-for-web-deploy-publishing-remote-agent.md). In diesem Thema wird beschrieben, wie Sie einen Webserver zu erstellen, der von Web Deploy veröffentlichen, verwenden den remote-Agent-Ansatz, beginnend mit einen bereinigten Build von Windows Server 2008 R2 unterstützt.
- [Konfigurieren eines Datenbankservers aus, für Web Deploy-Veröffentlichung](configuring-a-database-server-for-web-deploy-publishing.md). Dieses Thema beschreibt, wie Sie einen Datenbankserver zur Unterstützung von Remotezugriff und -Bereitstellung, beginnend bei einer Standardinstallation von SQL Server 2008 R2 zu konfigurieren.

## <a name="further-reading"></a>Weiterführende Themen

Anleitungen zum Konfigurieren einer typischen staging-Umgebung finden Sie unter [Szenario: Konfigurieren von einer Staging-Umgebung für die Webbereitstellung](scenario-configuring-a-staging-environment-for-web-deployment.md). Anleitungen zum Konfigurieren einer typischen produktionsumgebung finden Sie unter [Szenario: Konfigurieren einer Produktionsumgebung für die Webbereitstellung](scenario-configuring-a-production-environment-for-web-deployment.md).

> [!div class="step-by-step"]
> [Zurück](choosing-the-right-approach-to-web-deployment.md)
> [Weiter](scenario-configuring-a-staging-environment-for-web-deployment.md)
