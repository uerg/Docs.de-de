---
uid: web-forms/overview/deployment/configuring-server-environments-for-web-deployment/scenario-configuring-a-staging-environment-for-web-deployment
title: "Szenario: Konfigurieren einer Stagingumgebung für Web Deploy | Microsoft Docs"
author: jrjlee
description: "In diesem Thema wird beschrieben, ein Webdienst-Bereitstellungsszenario für eine Stagingumgebung und erläutert, die Aufgaben, die Sie ausführen, um eine ähnliche Env einrichten müssen..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 5a8e49b7-5317-4125-b107-7e2466b47bb3
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/configuring-server-environments-for-web-deployment/scenario-configuring-a-staging-environment-for-web-deployment
msc.type: authoredcontent
ms.openlocfilehash: 683a0cf88225fee762e82925afe3785a2defd5bf
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/24/2018
---
<a name="scenario-configuring-a-staging-environment-for-web-deployment"></a>Szenario: Konfigurieren einer Stagingumgebung für die Bereitstellung
====================
durch [Jason Lee](https://github.com/jrjlee)

[PDF herunterladen](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> In diesem Thema wird beschrieben, ein Webdienst-Bereitstellungsszenario für eine Stagingumgebung und erläutert, die Aufgaben, die Sie ausführen, um eine ähnliche Umgebung einrichten müssen.


Viele Organisationen verwenden Stagingumgebungen, um Updates für Webanwendungen oder Websites in der Vorschau anzeigen. Dadurch Personen innerhalb Ihrer Organisation eine Möglichkeit, untersuchen und überprüfen die neue Funktionen oder Inhalte, bevor die Website "geht", oder in anderen Worten: in einer produktionsumgebung bereitgestellt wird. Die Stagingumgebung dient die produktionsumgebung so genau wie möglich zu replizieren, um eine realistische Vorschau bereitzustellen. Diese Art von Stagingumgebung weist normalerweise folgende Merkmale auf:

- Die Umgebung besteht aus mehreren Lastenausgleich Webserver und eine oder mehrere Datenbankserver, häufig mit Failoverclustering und datenbankspiegelung.
- Anwendungen können manuell von einem Entwicklungsteam oder automatisch von einem Team Build-Server bereitgestellt werden.
- Die Benutzer oder Prozesskonten, die Bereitstellung von Anwendungen entsprechen wahrscheinlich nicht auf den staging-Servern über Administratorrechte verfügen.
- Änderungen an Anwendungen in regelmäßigen Abständen, bereitgestellt werden, damit die Umgebung zur Unterstützung von einstufiger muss oder automatisierte Bereitstellung.

> [!NOTE]
> Dezentrales Skalieren einer datenbankbereitstellung auf mehreren Servern ist nicht Gegenstand dieses Lernprogramm. Weitere Informationen zu diesem Bereich, finden Sie in [SQL Server-Onlinedokumentation](https://technet.microsoft.com/library/ms130214.aspx).


Z. B. in unserer [lernprogrammszenario](../deploying-web-applications-in-enterprise-scenarios/enterprise-web-deployment-scenario-overview.md), Team Foundation Server (TFS) verwaltet die Kontakt-Manager-Lösung. Die TFS-Administrator Rob Walters, hat eine Builddefinition erstellt, mit dem Entwickler, die eine Bereitstellung in der Stagingumgebung nach Bedarf auszulösen.

![](scenario-configuring-a-staging-environment-for-web-deployment/_static/image1.png)

Beachten Sie, dass in den meisten Fällen Sie unbedingt wird nicht den neuesten Build in der Stagingumgebung bereitstellen möchten. Stattdessen können Sie viel wahrscheinlicher für einen bestimmten Build bereitstellen, der bereits Validierung und Überprüfung in der testumgebung entwurfsänderungen möchten.

## <a name="solution-overview"></a>Lösungsübersicht

In diesem Szenario können Sie diese Fakten aus eine Analyse der bereitstellungsanforderungen ableiten:

- Der Benutzer oder Prozess-Konto, das die Bereitstellung führt wird nicht Administratorrechte auf den staging-Servern, damit die Web-Stagingserver Nichtadministrator-Bereitstellung unterstützen. Daher müssen Sie die Web-Stagingserver Verwendung der Web-Handler bereitstellen anstelle des remote-Agenten zu konfigurieren.
- Die Stagingumgebung enthält mehrere Webserver, aber er nur einem Klick oder automatisierte Bereitstellung zu unterstützen, daher Sie das Web Farm Framework (WFF) zu verwenden, um eine Serverfarm erstellen müssen muss. Bei dieser Vorgehensweise können Sie eine Anwendung mit einem Web-Server (dem primären Server) bereitstellen und WFF wird die Bereitstellung für alle anderen Webserver in der Stagingumgebung repliziert.
- Der Benutzer oder das Prozesskonto, die die Bereitstellung führt muss Berechtigungen zum Erstellen von Datenbanken verfügen. Daher müssen Sie das Konto zum Hinzufügen der **Dbcreator** -Serverrolle auf dem Datenbankserver, zusätzlich zum Konfigurieren des Datenbankserver zur Unterstützung von Remotezugriff und Bereitstellung.

Die folgenden Themen enthalten alle Informationen, die Sie benötigen, um diese Aufgaben ausführen:

- [Erstellen Sie eine Serverfarm mit Webfarmframework](creating-a-server-farm-with-the-web-farm-framework.md). Dieses Thema beschreibt, wie zum Erstellen und Konfigurieren einer Serverfarm WFF, sodass Web Platform Produkte und Komponenten, Konfigurationseinstellungen und Websites und Anwendungen auf mehrere Load Balancing Webserver repliziert werden.
- [Konfigurieren Sie einen Webserver für Web Deploy-Veröffentlichung (Web Deploy-Handler)](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler.md). In diesem Thema wird beschrieben, wie einen Webserver zu erstellen, der von Web Deploy veröffentlichen, indem Sie den remote-Agent-Ansatz, beginnend mit einem bereinigten Build von Windows Server 2008 R2 unterstützt wird.
- [Konfigurieren eines Datenbankservers für Web Deploy-Veröffentlichung](configuring-a-database-server-for-web-deploy-publishing.md). Dieses Thema beschreibt, wie einen Datenbankserver zur Unterstützung von Remotezugriff und Bereitstellung, beginnend bei einer Standardinstallation von SQL Server 2008 R2 zu konfigurieren.

## <a name="further-reading"></a>Weiterführende Themen

Anleitungen zum Konfigurieren einer testumgebung typische Entwickler finden Sie unter [Szenario: Konfigurieren einer Umgebung testen für Webbereitstellung](scenario-configuring-a-test-environment-for-web-deployment.md). Anleitungen zum Konfigurieren einer typischen produktionsumgebung finden Sie unter [Szenario: Konfigurieren einer produktiven Umgebung für die Bereitstellung von](scenario-configuring-a-production-environment-for-web-deployment.md).

>[!div class="step-by-step"]
[Zurück](scenario-configuring-a-test-environment-for-web-deployment.md)
[Weiter](scenario-configuring-a-production-environment-for-web-deployment.md)
