---
uid: web-forms/overview/deployment/deploying-web-applications-in-enterprise-scenarios/deploying-web-applications-in-enterprise-scenarios
title: Bereitstellen von Webanwendungen in Unternehmensszenarien mit Visual Studio 2010 | Microsoft-Dokumentation
author: jrjlee
description: Diese Reihe von Tutorials beschreibt Tools und Techniken, die Sie zum Bereitstellen von Webanwendungen in verschiedenen Unternehmensszenarien verwenden können. Es wird erläutert, wie optimal zu nutzen...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/03/2012
ms.topic: article
ms.assetid: 48cfe378-d62a-48c6-a4db-6be3cead6898
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/deployment/deploying-web-applications-in-enterprise-scenarios/deploying-web-applications-in-enterprise-scenarios
msc.type: authoredcontent
ms.openlocfilehash: 8412000e150f59911bb38f0147b1a487bef60c18
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/03/2018
ms.locfileid: "37376837"
---
<a name="deploying-web-applications-in-enterprise-scenarios-using-visual-studio-2010"></a>Bereitstellen von Webanwendungen in Unternehmensszenarien mit Visual Studio 2010
====================
durch [Jason Lee](https://github.com/jrjlee)

[PDF herunterladen](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Diese Reihe von Tutorials beschreibt Tools und Techniken, die Sie zum Bereitstellen von Webanwendungen in verschiedenen Unternehmensszenarien verwenden können. Es wird erläutert, wie am besten von Technologien wie Visual Studio 2010, Microsoft Build Engine (MSBuild), Internet Information Services (IIS) 7.5, das IIS-Webbereitstellungstool (Web Deploy), das Web Farm Framework (WFF) und Hilfsprogramme wie VSDBCMD.exe zu nutzen vereinfachen und den Bereitstellungsprozess zu verwalten. Sie enthält Übersichten und aufgabenbezogene Leitfäden, die Sie bei der:
> 
> - Überprüfen Sie, und herzustellen Sie die Anforderungen für die Bereitstellung für eine Webanwendung für Unternehmen.
> - Konfigurieren Sie die Test-, Staging- und produktionsumgebungen-webserverumgebungen zur Unterstützung von Web-Bereitstellung.
> - Konfigurieren Sie Team Foundation Server (TFS) continuous (CI) Integrationsprozesse, um automatisierte webbereitstellung zu unterstützen.
> - Stellen Sie unternehmensweite Webanwendungen unterschiedliche Umgebungen mit unterschiedlichen Anforderungen und Einschränkungen bereit.
> - Bereitstellen von Änderungen für Webanwendungen, die in verschiedenen serverumgebungen ausgeführt werden.
> 
> > [!NOTE]
> > Während in diesen Tutorials wird die Verwendung von TFS als ein CI-Server beschrieben, ist die Anleitung auf einem beliebigen CI-Server ganz einfach angepasst. Umfassende Kenntnisse über TFS zu verstehen und nutzen die Tutorials erforderlich nicht.
> 
> 
> Ein italienischen Übersetzung mit diesen Lernprogrammen finden Sie unter [ http://www.lucamorelli.it ](http://www.lucamorelli.it).


## <a name="about-the-authors"></a>Über die Autoren

Jason Lee ist leitender Technologe mit [Inhalt Master](http://www.contentmaster.com/) , in denen er arbeitet mit Microsoft-Produkten und Technologien, insbesondere für SharePoint und ASP.NET, seit einigen Jahren. Jason verfügt über einen PhD in der Datenverarbeitung und befindet sich derzeit MCPD und MCTS-Zertifizierung. Lesen Sie Jasons technischen Blog unter [www.jrjlee.com](http://www.jrjlee.com/).

Benjamin Curry ist leitender Technologe mit [Inhalt Master](http://www.contentmaster.com/) hat, die Whitepaper, SDK-Dokumentation, PowerPoint-Präsentationen und Kurse mit Schulungsleiter und onlineschulungen während seiner beruflichen Laufbahn geschrieben. Eine ursprüngliche Member des ASP.NET-Teams Dokumentation, hat er mit der Microsoft-webtechnologien für mehr als einem Jahrzehnt gearbeitet.

## <a name="target-audience"></a>Zielgruppe

Diese Reihe von Lernprogrammen ist für ASP.NET Web Application-Entwickler und Lösungsarchitekten, die Visual Studio 2010 verwenden, um unternehmensweite Webanwendungen zu erstellen. Um den größtmöglichen Nutzen aus dem Inhalt zu erhalten, sollten Sie mit Visual Studio 2010 vertraut sein und haben grundlegende Kenntnisse von TFS, zusammen mit Berücksichtigung der Plattform Microsoft-webtechnologien wie ASP.NET MVC 3, Windows Communication Foundation (WCF), IIS, SQL Server und Visual Studio-Datenbankprojekte. Sie müssen jedoch nicht mit Bereitstellungstools und-Technologien vertraut sein, oder müssen wissen, wie zum Einrichten von CI-Systemen zu können.

## <a name="requirements"></a>Anforderungen

Führen die exemplarischen Vorgehensweisen aus, und führen Sie die Aufgaben, die in diesen Tutorials beschreiben, müssen Sie zum Installieren der Software auf Ihrem Entwicklungscomputer:

- Visual Studio 2010 Premium oder Ultimate Edition mit Servicepack 1
- .NET Framework 4.0
- .NET Framework 3.5 mit Servicepack 1
- ASP.NET MVC 3.0
- IIS 7.5 Express
- SQL Server Express 2008 R2

Um die in diesen exemplarischen Vorgehensweisen beschriebenen Schritte zur Bereitstellung auszuführen, müssen Sie Zugriff auf beispielumgebungen für die Bereitstellung von Web-Anwendung. Für optimale Ergebnisse sollten diese Umgebungen Ihrer Organisation Unternehmen Bereitstellungsmuster widerspiegeln. Anschließend können Sie die exemplarischen Vorgehensweisen in dieser Dokumentation die bereitstellungsumgebungen und Anforderungen Ihrer Organisation entsprechend ändern.

## <a name="series-contents"></a>Inhalt der Reihe

In diesem einführenden Abschnitt besteht aus zwei weiteren Themen. Diese sind darauf ausgelegt, um einen breiteren Kontext für die Lernprogramme bereitzustellen, die folgen:

- [Webbasierte Unternehmensbereitstellung: Szenarioübersicht](enterprise-web-deployment-scenario-overview.md). In diesem Thema wird beschrieben, die jeweils den Tutorials dieser Serie bildet. Das Szenario konzentriert sich auf das Application Lifecycle Management (ALM)-Anforderungen für das fiktive Unternehmen mit dem Namen Fabrikam, Inc., wie sie eine Webanwendung für Unternehmen entwickelt.
- [Anwendungslebenszyklus-Verwaltung: Von der Entwicklung zur Produktion](application-lifecycle-management-from-development-to-production.md). Dieses Thema enthält eine Übersicht auf hoher Ebene, End-to-End über einen Prozess. Es wird veranschaulicht, wie eine unternehmensweite ASP.NET-Webanwendung über die Test-, Staging-und produktionsumgebungen im Rahmen einer ständigen Entwicklung von Fabrikam, Inc. bewegt wird.

Die Serie umfasst vier Tutorial Sätze. Jede konzentriert sich auf verschiedene Aspekte der Webentwicklung:

- [Webbereitstellung im Unternehmen](../web-deployment-in-the-enterprise/web-deployment-in-the-enterprise.md). Dieses Tutorial bietet eine grundlegende Einführung in MSBuild-Projektdateien, die Veröffentlichung Webpipeline, Web Deploy und andere verwandten Technologien. Es wird erläutert, wie diese Tools zusammen verwenden werden können, um komplexe Bereitstellungsprozesse zu verwalten.
- [Konfigurieren von Serverumgebungen für die Webbereitstellung](../configuring-server-environments-for-web-deployment/configuring-server-environments-for-web-deployment.md). In diesem Tutorial wird beschrieben, wie Windows Server zur Unterstützung verschiedener Bereitstellungsszenarien, einschließlich der remote-Web-Paket-Bereitstellung mithilfe der Webbereitstellungs-Agent-Dienst ("remote-Agent") oder Bereitstellen von Web-Handler und remote-Datenbank-Bereitstellung konfiguriert. Sie erhalten Anweisungen zum Auswählen der geeigneten Bereitstellungsmethode für Ihre eigene Umgebung, und es wird beschrieben, wie die WFF verwenden, um bereitgestellten Webanwendungen für alle Webserver in einer Serverfarm zu replizieren.
- [Konfigurieren von Team Foundation Server für die Webbereitstellung](../configuring-team-foundation-server-for-web-deployment/configuring-team-foundation-server-for-web-deployment.md). In diesem Tutorial wird beschrieben, wie zum Konfigurieren von TFS zur Unterstützung verschiedener Bereitstellungsszenarien, einschließlich der automatisierten Bereitstellung im Rahmen eines CI-Prozesses und bestimmte Builds Bereitstellungen manuell ausgelöst wird.
- [Erweiterte webbasierte Unternehmensbereitstellung](../advanced-enterprise-web-deployment/advanced-enterprise-web-deployment.md). In diesem Tutorial wird beschrieben, wie verschiedene erweiterte Bereitstellung, z. B. Anpassen von datenbankbereitstellungen für mehrere Umgebungen und Ausschließen von Dateien und Ordner von der Bereitstellung von Webanwendungen während der Bereitstellung und Aufgaben .

## <a name="where-to-start"></a>Wo Sie anfangen

Diese Reihe von Lernprogrammen verwendet eine beispiellösung mit einem realistischen Maß an Komplexität, zusammen mit der ein fiktives Unternehmen-Bereitstellungsszenario, um eine referenzimplementierung bereitzustellen und geben Sie die Aufgaben und exemplarische Vorgehensweisen ein allgemeiner Kontext. Im nächsten Thema, [webbasierte Unternehmensbereitstellung: Szenarioübersicht](enterprise-web-deployment-scenario-overview.md), führt das Szenario und die Projektmappe. Von dort aus können Sie arbeiten, über die Lernprogramme und Themen, die Ihren Anforderungen am ehesten entsprechen.

> [!div class="step-by-step"]
> [Nächste](enterprise-web-deployment-scenario-overview.md)
