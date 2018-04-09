---
uid: web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/configuring-permissions-for-team-build-deployment
title: Konfigurieren von Berechtigungen für Team Build-Bereitstellung | Microsoft Docs
author: jrjlee
description: In diesem Thema wird beschrieben, wie so konfigurieren Sie Berechtigungen so aktivieren Sie die Build-Server zum Bereitstellen von Inhalt an den Webserver und Datenbankserver als Teil eines automatisierten b wird...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 2488a91e-b0a8-465a-b874-3233f724b56b
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/configuring-permissions-for-team-build-deployment
msc.type: authoredcontent
ms.openlocfilehash: 4698349d664816ec49475bbfe71fb32af79ea96d
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
---
<a name="configuring-permissions-for-team-build-deployment"></a>Konfigurieren von Berechtigungen für Team Build-Bereitstellung
====================
durch [Jason Lee](https://github.com/jrjlee)

[PDF herunterladen](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Dieses Thema beschreibt das Konfigurieren von Berechtigungen für die Build-Server zum Bereitstellen von Inhalt an den Webserver und Datenbankserver als Teil eines automatisierten Build-Prozesses zu aktivieren.


Dieses Thema ist Teil einer Reihe von Lernprogrammen, die auf der Basis der Enterprise-bereitstellungsanforderungen eines fiktiven Unternehmens mit dem Namen Fabrikam, Inc. Dieses Lernprogramm Zeichenreihe verwendet eine beispiellösung&#x2014;der [Kontakt-Manager-Lösung](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;zur Darstellung einer Webanwendung mit einer realistischen Maß an Komplexität, einschließlich einer ASP.NET MVC 3-Anwendung, einen Windows Communication Foundation (WCF)-Dienst, und ein Datenbankprojekt.

Die Bereitstellungsmethode das Herzstück mit diesen Lernprogrammen basiert auf in beschriebene Ansatz der Teilung Projekt Datei [verstehen die Projektdatei](../web-deployment-in-the-enterprise/understanding-the-project-file.md), in dem durch der Buildprozess gesteuert wird zwei Projektdateien&#x2014;enthält Erstellen Sie für jede zielumgebung und enthält umgebungsspezifische Einstellungen für Build- und Bereitstellungsprozess geltenden Anweisungen, an. Zur Buildzeit ist die Unabhängigkeit von der Umgebung-Projektdatei, einen vollständigen Satz von Buildanweisungen bilden die Projektdatei umgebungsspezifische zusammengeführt.

## <a name="task-overview"></a>Übersicht über den Task

Wenn Sie den Team Foundation Server (TFS) 2010-Builddienst installieren, geben Sie die Identität, mit der den Dienst ausgeführt werden soll. Standardmäßig ist dies das Netzwerkdienstkonto verfügt. Alternativ können Sie die Build-Dienst ausgeführt wird, unter Verwendung eines Domänenkontos konfigurieren.

Alle Bereitstellungsaufgaben, die erfordern Windows-Authentifizierung, und Sie mit Team Build, automatisieren möchten werden mit der Dienstidentität Build ausgeführt. Daher müssen Sie der Build-Dienstidentität alle erforderlichen Berechtigungen für Ihre Webserver und Ihrer Datenbankserver aufzurüsten gewähren.

> [!NOTE]
> Das Netzwerkdienstkonto verwendet das Konto "Machine", um auf anderen Computern zu authentifizieren. Computerkonten haben die Form * [Domänenname]\[Computername] ***$**&#x2014;z. B. **FABRIKAM\TFSBUILD$**. Wenn der Builddienst ausgeführt wird, mit der Identität Network Service, sollten Sie daher alle erforderlichen Berechtigungen für die computerkontoidentität für Ihren Buildserver gewähren.


## <a name="configuring-web-server-permissions"></a>Konfigurieren von Webserverberechtigungen

Wie in beschrieben [Auswählen der rechts Ansatz für die Webbereitstellung](../configuring-server-environments-for-web-deployment/choosing-the-right-approach-to-web-deployment.md), es gibt zwei Hauptansätze können Webpakete auf einem Remotewebserver bereitgestellt werden soll:

- Bereitstellen die Anwendung von einem Remotestandort durch Angeben der *Webbereitstellungs-Agent-Dienst* (auch bekannt als der remote-Agent) auf dem Zielserver.
- Bereitstellen die Anwendung von einem Remotestandort durch Angeben der *Internetinformationsdienste (IIS)* (*IIS) Bereitstellen von Web-Handler* auf dem Zielserver.

Der remote-Agent hat in diesem Fall zwei wichtige Einschränkungen:

- Der remote-Agent unterstützt nur die NTLM-Authentifizierung. In anderen Worten muss die Bereitstellung die Build-Dienstidentität verwenden&#x2014;kann ein anderes Konto nicht imitieren.
- Um den remote-Agenten zu verwenden, muss das Konto, das die Bereitstellung führt ein Administrator auf dem Zielserver sein.

Zusammen bilden diese zwei Einschränkungen den remote-Agent-Ansatz nicht wünschenswert, dass eine automatische Bereitstellung von Team Build. Um diesen Ansatz verwenden, müssten Sie den Builddienst ein Administratorkonto auf dem Zielserver Web zu machen.

Der Handler für die Web-bereitstellen-Ansatz bietet im Gegensatz dazu verschiedenen Vorteile:

- Der Web-Handler bereitstellen unterstützt die Standardauthentifizierung über HTTPS, dadurch können Sie die Anmeldeinformationen eines anderen Kontos auf dem IIS-Webbereitstellungstool (Web Deploy) übergeben.
- Dient zum Konfigurieren von Ziel-Webserver, damit Benutzer ohne Administratorrechte zum Bereitstellen von Inhalt auf bestimmte IIS-Websites, die mit der Web-Handler bereitstellen können.

Daher ist es deutlich gegenüber der Web-Handler bereitstellen als Ziel bei der Bereitstellung von Web-Paketen aus Team Build zu automatisieren. Dies ist die empfohlene Vorgehensweise:

1. Erstellen Sie ein gering privilegiertes Domänenkonto für die Bereitstellung zu verwenden.
2. Konfigurieren der Web-Handler bereitstellen, und erteilen Sie dem Konto die erforderlichen Berechtigungen zum Bereitstellen von Inhalt zu einer bestimmten IIS-Website, wie in beschrieben [Konfigurieren von einem Webserver für bereitstellen Webveröffentlichung (Web bereitstellen Handler)](../configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler.md).
3. Aufrufen von Web Deploy und als Ziel der Web-Handler bereitstellen, unter Verwendung der Standardauthentifizierung und die Angabe der Anmeldeinformationen des Domänenkontos an Sie erstellt haben, um die Bereitstellung auszuführen.

In der [Vorgesetzten Kontakts](../web-deployment-in-the-enterprise/the-contact-manager-solution.md) Beispielprojektmappe, geben Sie den Authentifizierungstyp (grundlegende oder NTLM), die Web Deploy-Anmeldeinformationen, und die Endpunktadresse (remote-Agent oder Bereitstellen von Web-Handler) in der Projektdatei umgebungsspezifische. Diese Werte werden verwendet, um zu formulieren und führen Sie einen Web Deploy-Befehl aus, wenn die Projektdatei ausgeführt wird. Weitere Informationen finden Sie unter [Bereitstellen von Webpaketen](../web-deployment-in-the-enterprise/deploying-web-packages.md).

Weitere Informationen zum Konfigurieren der Web bereitstellen Handler, einschließlich Informationen zum Konfigurieren von Berechtigungen finden Sie unter [Konfigurieren von einem Webserver für bereitstellen Webveröffentlichung (Web bereitstellen Handler)](../configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler.md). Weitere Informationen zum Konfigurieren des remote-Agents finden Sie unter [Konfigurieren von einem Webserver für bereitstellen Webveröffentlichung (Remote-Agent)](../configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-remote-agent.md).

## <a name="configuring-database-server-permissions"></a>Konfigurieren von Server-Datenbankberechtigungen

Um eine Datenbank für SQL Server bereitzustellen, müssen Sie folgende Aktionen ausführen:

- Erstellen Sie eine Anmeldung für das Konto bereitstellen, auf SQL Server-Instanz.
- Gewähren Sie der Anmeldung **DBCreator** Berechtigungen für die SQL Server-Instanz.
- Fügen Sie nach der ersten Bereitstellung die Anmeldung bei der **Db\_Besitzer** Rolle in der Zieldatenbank. Dies ist erforderlich, da für spätere Bereitstellungen, Sie sind eine vorhandene Datenbank ändern, anstatt eine neue Datenbank erstellen.

Sie können mit einer SQL Server-Instanz, die mithilfe von NTLM-Authentifizierung oder SQL Server-Authentifizierung authentifizieren:

- Wenn Sie die NTLM-Authentifizierung verwenden, müssen Sie die oben beschriebenen, für das Build-Dienstkonto Berechtigungen gewähren.
- Wenn Sie SQL Server-Authentifizierung verwenden, müssen Sie die oben beschriebenen, auf dem SQL Server-Dienstkonto Berechtigungen gewähren. Sie müssen auch den SQL Server-Benutzernamen und das Kennwort in der Verbindungszeichenfolge enthalten, die Sie zum Bereitstellen der Datenbank verwenden.

Schrittweise aufgebaute Details zum Konfigurieren von Berechtigungen für die Bereitstellung finden Sie unter [konfigurieren einen Datenbankserver für die Webveröffentlichung bereitstellen](../configuring-server-environments-for-web-deployment/configuring-a-database-server-for-web-deploy-publishing.md).

## <a name="conclusion"></a>Schlussbemerkung

An diesem Punkt sollten Sie verstehen, die Berechtigungen, die erforderlich sein können, zusammen mit der Authentifizierungsoptionen, öffnen Sie bei der Web-Anwendung und Datenbank-Bereitstellungen von Team Build zu automatisieren. Sie sollten auch die erforderlichen Berechtigungen für IIS-Webserver und SQL Server-Datenbankserver implementiert sein.

## <a name="further-reading"></a>Weiterführende Themen

Weitere Informationen zum Konfigurieren von Windows Server-Umgebungen zur Unterstützung der Remotebereitstellung finden Sie unter [Konfigurieren von Server-Umgebungen für die Bereitstellung](../configuring-server-environments-for-web-deployment/configuring-server-environments-for-web-deployment.md).

> [!div class="step-by-step"]
> [Vorherige](deploying-a-specific-build.md)
