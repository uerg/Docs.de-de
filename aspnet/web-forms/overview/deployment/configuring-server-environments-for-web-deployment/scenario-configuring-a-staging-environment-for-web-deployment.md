---
uid: web-forms/overview/deployment/configuring-server-environments-for-web-deployment/scenario-configuring-a-staging-environment-for-web-deployment
title: 'Szenario: Konfigurieren einer Stagingumgebung für die Webbereitstellung | Microsoft-Dokumentation'
author: jrjlee
description: In diesem Thema wird beschrieben, ein typisches Web-Bereitstellungsszenario für einer Stagingumgebung bereit und erläutert, die Aufgaben, die Sie ausführen, um eine ähnliche Env einrichten möchten...
ms.author: aspnetcontent
ms.date: 05/04/2012
ms.assetid: 5a8e49b7-5317-4125-b107-7e2466b47bb3
msc.legacyurl: /web-forms/overview/deployment/configuring-server-environments-for-web-deployment/scenario-configuring-a-staging-environment-for-web-deployment
msc.type: authoredcontent
ms.openlocfilehash: 9c0f12645f10820996a03c232d20ee87031aefaa
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2018
ms.locfileid: "37820800"
---
<a name="scenario-configuring-a-staging-environment-for-web-deployment"></a>Szenario: Konfigurieren einer Stagingumgebung für die Webbereitstellung
====================
durch [Jason Lee](https://github.com/jrjlee)

[PDF herunterladen](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> In diesem Thema wird beschrieben, ein typisches Web-Bereitstellungsszenario für einer Stagingumgebung bereit und erläutert, die Aufgaben, die Sie ausführen, um eine ähnliche Umgebung einrichten müssen.


Viele Organisationen verwenden Stagingumgebungen, um eine Vorschau der Updates für Webanwendungen und Websites anzuzeigen. Dies gibt Personen innerhalb der Organisation die Gelegenheit, untersuchen und überprüfen die neue Funktionen oder Inhalte, bevor die Website "live geschaltet" oder in anderen Worten: in einer produktionsumgebung bereitgestellt wird. Die Stagingumgebung soll die produktionsumgebung soweit wie möglich zu replizieren, um eine realistische Vorschau bereitzustellen. Diese Art von Stagingumgebung hat in der Regel folgende Eigenschaften:

- Die Umgebung besteht aus mehreren Webservern auf der Lastenausgleich und eine oder mehrere Datenbankserver häufig mit Failoverclustering und datenbankspiegelung.
- Anwendungen können manuell von einem Entwicklungsteam oder automatisch von einem Team Build-Server bereitgestellt werden.
- Die Benutzer oder Prozesskonten, die Bereitstellung von Anwendungen sind wahrscheinlich nicht über Administratorrechte auf dem Stagingserver verfügen.
- Änderungen an Anwendungen in regelmäßigen Abständen, bereitgestellt werden, damit die Umgebung zur Unterstützung von einem Schritt muss oder die automatisierte Bereitstellung.

> [!NOTE]
> Horizontales Skalieren einer datenbankbereitstellung auf mehreren Servern ist, würde den Rahmen dieses Tutorials. Weitere Informationen zu diesem Bereich, finden Sie in [SQL Server-Onlinedokumentation](https://technet.microsoft.com/library/ms130214.aspx).


Z. B. in unserer [lernprogrammszenario](../deploying-web-applications-in-enterprise-scenarios/enterprise-web-deployment-scenario-overview.md), Team Foundation Server (TFS) verwaltet die Kontakt-Manager-Lösung. Der TFS-Administrator, Rob Walters, hat es sich um eine bestimmte Builddefinition erstellt, mit dem Entwickler, die eine Bereitstellung in der Stagingumgebung nach Bedarf auszulösen.

![](scenario-configuring-a-staging-environment-for-web-deployment/_static/image1.png)

Beachten Sie, dass in den meisten Fällen Sie unbedingt wird nicht den neuesten Build in der Stagingumgebung bereitstellen möchten. Stattdessen können Sie sehr viel wahrscheinlicher zu einen bestimmten Build bereitgestellt, der bereits, Validierung und Überprüfung in der testumgebung vorgenommen wurden werden soll.

## <a name="solution-overview"></a>Übersicht über die Lösung

In diesem Szenario können Sie diese Fakten aus einer Analyse der bereitstellungsanforderungen hergeleitet werden:

- Der Benutzer oder Prozess Konto, das die Bereitstellung ausführt wird nicht über Administratorrechte verfügen auf den staging-Servern, damit die staging Webserver nicht-Administrator-Bereitstellung unterstützen müssen. Daher müssen Sie so konfigurieren Sie die staging-Webservern, um den Handler für die Web-Bereitstellung anstelle der remote-Agent zu verwenden.
- Die Stagingumgebung enthält mehrere Webserver, aber nur einem Klick oder automatisierte Bereitstellung unterstützt, daher Sie die Web Farm Framework (WFF) zu verwenden, um eine Serverfarm erstellen müssen muss. Mit diesem Ansatz können Sie eine Anwendung auf einem Webserver (der primäre Server) bereitstellen und WFF wird die Bereitstellung auf allen anderen Webservern in der Stagingumgebung replizieren.
- Das Benutzer- oder Prozess an, die die Bereitstellung ausführt, benötigen Berechtigungen zum Erstellen von Datenbanken. Daher müssen Sie zum Hinzufügen des Kontos, der **Dbcreator** -Serverrolle auf dem Datenbankserver, neben der Konfiguration für des Datenbankserver zur Unterstützung von Remotezugriff und Bereitstellung.

In diesen Themen enthalten alle Informationen, die Sie benötigen, um diese Aufgaben ausführen:

- [Erstellen Sie eine Serverfarm mit Webfarmframework](creating-a-server-farm-with-the-web-farm-framework.md). In diesem Thema wird beschrieben, wie zum Erstellen und konfigurieren eine Serverfarm mit WFF, sodass Web Platform-Produkte und Komponenten, Konfigurationseinstellungen und Websites und Anwendungen über mehrere Lastenausgleich Webserver hinweg repliziert werden.
- [Konfigurieren Sie einen Webserver für Web Deploy-Veröffentlichung (Web Deploy-Handler)](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler.md). In diesem Thema wird beschrieben, wie Sie einen Webserver zu erstellen, der von Web Deploy veröffentlichen, verwenden den remote-Agent-Ansatz, beginnend mit einen bereinigten Build von Windows Server 2008 R2 unterstützt.
- [Konfigurieren eines Datenbankservers aus, für Web Deploy-Veröffentlichung](configuring-a-database-server-for-web-deploy-publishing.md). Dieses Thema beschreibt, wie Sie einen Datenbankserver zur Unterstützung von Remotezugriff und -Bereitstellung, beginnend bei einer Standardinstallation von SQL Server 2008 R2 zu konfigurieren.

## <a name="further-reading"></a>Weiterführende Themen

Anleitungen zum Konfigurieren einer testumgebung für die typischen Entwicklers finden Sie unter [Szenario: Konfigurieren einer Testumgebung für die Webbereitstellung](scenario-configuring-a-test-environment-for-web-deployment.md). Anleitungen zum Konfigurieren einer typischen produktionsumgebung finden Sie unter [Szenario: Konfigurieren einer Produktionsumgebung für die Webbereitstellung](scenario-configuring-a-production-environment-for-web-deployment.md).

> [!div class="step-by-step"]
> [Zurück](scenario-configuring-a-test-environment-for-web-deployment.md)
> [Weiter](scenario-configuring-a-production-environment-for-web-deployment.md)
