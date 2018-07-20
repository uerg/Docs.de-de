---
uid: web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/configuring-permissions-for-team-build-deployment
title: Konfigurieren von Berechtigungen für Team-Buildbereitstellung | Microsoft-Dokumentation
author: jrjlee
description: In diesem Thema wird beschrieben, wie so konfigurieren Sie Berechtigungen zum Aktivieren des Buildservers zum Bereitstellen von Inhalt auf Webserver und Datenbankserver als Teil eines automatisierten b wird...
ms.author: aspnetcontent
ms.date: 05/04/2012
ms.assetid: 2488a91e-b0a8-465a-b874-3233f724b56b
msc.legacyurl: /web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/configuring-permissions-for-team-build-deployment
msc.type: authoredcontent
ms.openlocfilehash: b577d887b4a4476b6796ae9f1df538d16eededa3
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2018
ms.locfileid: "37820359"
---
<a name="configuring-permissions-for-team-build-deployment"></a>Konfigurieren von Berechtigungen für Team-Buildbereitstellung
====================
durch [Jason Lee](https://github.com/jrjlee)

[PDF herunterladen](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Dieses Thema beschreibt, wie Sie Berechtigungen konfigurieren, um Ihre Build-Server zum Bereitstellen von Inhalt auf Webserver und Datenbankserver als Teil einer automatisierten Buildprozess zu aktivieren.


In diesem Thema ist Teil einer Reihe von Tutorials, die auf der Basis der bereitstellungsanforderungen Enterprise ein fiktives Unternehmen, die mit dem Namen Fabrikam, Inc. Dieser tutorialreihe verwendet eine beispiellösung&#x2014;der [Contact Manager-Lösung](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;zur Darstellung einer Webanwendung mit einem realistischen Maß an Komplexität, einschließlich einer ASP.NET MVC 3-Anwendung, eine Windows-Kommunikation Foundation (WCF)-Dienst und ein Datenbankprojekt.

Die Methode für die Bereitstellung das Kernstück des in diesen Tutorials basiert auf den geteilten Projekt Dateiansatz beschrieben, die [Grundlegendes zur Projektdatei](../web-deployment-in-the-enterprise/understanding-the-project-file.md), in dem der Buildprozess durch gesteuert wird zwei Projektdateien&#x2014;enthält Erstellen Sie die Anweisungen, die für jede zielumgebung, und enthält umgebungsspezifische Build & Deployment-Einstellungen gelten. Zur Erstellungszeit wird die umgebungsspezifischen-Projektdatei in die Unabhängigkeit von der Umgebung Projektdatei, um einen vollständigen Satz von einrichtungsanweisungen bilden zusammengeführt.

## <a name="task-overview"></a>Übersicht über den Task

Wenn Sie den Team Foundation Server (TFS) 2010-Builddienst installieren, geben Sie die Identität, die mit der den Dienst ausgeführt werden soll. Standardmäßig ist dies das Netzwerkdienstkonto. Alternativ können Sie den Builddienst ausgeführt, unter Verwendung eines Domänenkontos konfigurieren.

Alle Bereitstellungsaufgaben, die erfordern Windows-Authentifizierung, und dass Sie planen, zur Automatisierung mithilfe von Team Build, werden mit der Dienstidentität für den Build ausgeführt. Daher müssen Sie der builddienstidentität alle erforderlichen Berechtigungen für Ihre Webserver und Ihrer Datenbankserver aufzurüsten gewähren.

> [!NOTE]
> Das Netzwerkdienstkonto verwendet das Computerkonto, andere Computer zu authentifizieren. Computerkonten haben die Form * [Domänenname]\[Machine-Name] ***$**&#x2014;z. B. **FABRIKAM\TFSBUILD$**. Wenn der Builddienst ausgeführt wird, mit der Identität Network Service, sollten Sie daher alle erforderlichen Berechtigungen, der die computerkontoidentität für Ihren Buildserver gewähren.


## <a name="configuring-web-server-permissions"></a>Konfigurieren von Webserver-Berechtigungen

Siehe [Entscheidung zur Webbereitstellung rechts](../configuring-server-environments-for-web-deployment/choosing-the-right-approach-to-web-deployment.md), es gibt zwei Hauptansätze können, wenn Sie Web-Pakete auf einem remote-Web-Server bereitstellen möchten:

- Bereitstellen die Anwendung von einem Remotestandort aus auf die *Webbereitstellungs-Agent-Dienst* (auch bekannt als die remote-Agent) auf dem Zielserver.
- Bereitstellen die Anwendung von einem Remotestandort aus auf die *Internet Information Services* (*IIS) Bereitstellen von Web-Handler* auf dem Zielserver.

Der remote-Agent hat zwei wichtige Einschränkungen in diesem Fall:

- Der remote-Agent unterstützt nur NTLM-Authentifizierung. Das heißt, muss die Bereitstellung verwenden die builddienstidentität&#x2014;nicht möglich, ein anderes Konto den Identitätswechsel.
- Um den remote-Agent zu verwenden, muss das Konto, das die Bereitstellung führt ein Administrator auf dem Zielserver sein.

Zusammen stellen diese beiden Einschränkungen den remote-Agent-Ansatz nicht wünschenswert, dass eine automatische Bereitstellung von Team Build. Um diesen Ansatz verwenden, müssten Sie den Builddienst, der einen Administrator auf jedem Zielserver für die Web-Konto zu machen.

Im Gegensatz dazu, der WebHandler-bereitstellen-Ansatz bietet verschiedene Vorteile:

- Die Web-bereitstellen-Handler unterstützt die grundlegende Authentifizierung über HTTPS und unter dem Sie die Anmeldeinformationen eines alternativen Kontos auf der IIS-Webbereitstellungstool (Web Deploy) zu übergeben können.
- Sie können die Ziel-Webserver zum Zulassen von nicht-Administratoren zum Bereitstellen von Inhalt auf bestimmte IIS-Websites, die mithilfe des Web-bereitstellen-Handlers konfigurieren.

Daher ist es deutlich besser als Ziel der Web-bereitstellen-Handler beim Automatisieren von Web-Paket-Bereitstellung von Team Build. Dies ist die empfohlene Vorgehensweise:

1. Erstellen Sie Konto mit niedrigen Berechtigungen für die Bereitstellung.
2. Konfigurieren der Web-bereitstellen-Handler und erteilen Sie dem Konto die erforderlichen Berechtigungen zum Bereitstellen von Inhalt auf einer bestimmten IIS-Website, siehe [Konfigurieren eines Webservers für Bereitstellen von Webpublishing (Web bereitstellen Handler)](../configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler.md).
3. Aufrufen von Web Deploy und als Ziel der Web-bereitstellen-Handler, unter Verwendung der Standardauthentifizierung und die Angabe der Anmeldeinformationen des Domänenkontos Sie erstellt haben, um die Bereitstellung durchzuführen.

In der [Contact Manager](../web-deployment-in-the-enterprise/the-contact-manager-solution.md) Beispielprojektmappe, geben Sie den Authentifizierungstyp (basic oder NTLM), die Web Deploy-Anmeldeinformationen ein, und die Endpunktadresse (remote-Agent oder Bereitstellen von Web-Handler) in der Umgebung spezifischen-Projektdatei. Diese Werte werden verwendet, um zu formulieren, und führen Sie einen Web Deploy-Befehl aus, wenn die Projektdatei ausgeführt wird. Weitere Informationen finden Sie unter [Bereitstellen von Webpaketen](../web-deployment-in-the-enterprise/deploying-web-packages.md).

Weitere Informationen zum Konfigurieren der bereitstellen Webhandler, einschließlich Informationen zum Konfigurieren von Berechtigungen finden Sie unter [Konfigurieren eines Webservers für Bereitstellen von Webpublishing (Web bereitstellen Handler)](../configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler.md). Weitere Informationen zum Konfigurieren des remote-Agents finden Sie unter [Konfigurieren eines Webservers für Bereitstellen von Webpublishing (Remote-Agent)](../configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-remote-agent.md).

## <a name="configuring-database-server-permissions"></a>Konfigurieren von Datenbank-Server-Berechtigungen

Zum Bereitstellen einer Datenbank in SQL Server müssen Sie folgende Aktionen ausführen:

- Erstellen Sie einen Anmeldenamen für das Bereitstellen von Konto, für die SQL Server-Instanz.
- Gewähren Sie der Anmeldung **DBCreator** Berechtigungen für die SQL Server-Instanz.
- Fügen Sie nach der ersten Bereitstellung die Anmeldung bei der **Db\_Besitzer** Rolle in der Zieldatenbank. Dies ist erforderlich, da für nachfolgende Bereitstellungen Sie können eine vorhandene Datenbank ändern, anstatt eine neue Datenbank erstellen.

Sie können auf eine SQL Server-Instanz, die mit NTLM-Authentifizierung oder SQL Server-Authentifizierung authentifizieren:

- Wenn Sie die NTLM-Authentifizierung verwenden, müssen Sie die oben beschriebenen Build Service-Konto Berechtigungen zu erteilen.
- Wenn Sie SQL Server-Authentifizierung verwenden, müssen Sie die oben beschriebenen, auf dem SQL Server-Dienstkonto Berechtigungen gewähren. Außerdem müssen Sie den SQL Server-Benutzernamen und das Kennwort in der Verbindungszeichenfolge enthalten, die Sie verwenden, um die Datenbank bereitstellen.

Detaillierte Informationen zum Konfigurieren von Berechtigungen für die datenbankbereitstellung finden Sie unter [Konfigurieren eines Datenbankservers für die Webveröffentlichung bereitstellen](../configuring-server-environments-for-web-deployment/configuring-a-database-server-for-web-deploy-publishing.md).

## <a name="conclusion"></a>Schlussbemerkung

An diesem Punkt sollten Sie die Berechtigungen, die erforderlich sind, zusammen mit der Authentifizierungsoptionen, öffnen Sie bei einer Web-Anwendung und Datenbank-Bereitstellungen von Team Build Automatisierung verstehen. Sie sollten auch die erforderlichen Berechtigungen für IIS-Webserver und SQL Server-Datenbank-Server implementieren können.

## <a name="further-reading"></a>Weiterführende Themen

Weitere Informationen zum Konfigurieren von Windows Server-Umgebungen um Remotebereitstellungen zu unterstützen, finden Sie unter [Konfigurieren von Server-Umgebungen für die Webbereitstellung](../configuring-server-environments-for-web-deployment/configuring-server-environments-for-web-deployment.md).

> [!div class="step-by-step"]
> [Vorherige](deploying-a-specific-build.md)
