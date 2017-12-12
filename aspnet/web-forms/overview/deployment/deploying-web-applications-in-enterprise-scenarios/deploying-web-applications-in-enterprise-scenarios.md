---
uid: web-forms/overview/deployment/deploying-web-applications-in-enterprise-scenarios/deploying-web-applications-in-enterprise-scenarios
title: Bereitstellen von Webanwendungen in Enterprise-Szenarios, die mit Visual Studio 2010 | Microsoft Docs
author: jrjlee
description: "Diese Reihe von Lernprogrammen beschreibt Tools und Techniken, die Sie zum Bereitstellen von Webanwendungen in verschiedenen Unternehmensszenarien verwenden können. Es wird erläutert, wie optimal nutzen..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/03/2012
ms.topic: article
ms.assetid: 48cfe378-d62a-48c6-a4db-6be3cead6898
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/deploying-web-applications-in-enterprise-scenarios/deploying-web-applications-in-enterprise-scenarios
msc.type: authoredcontent
ms.openlocfilehash: 99bab371dd34b30f3554843e49bbec7f57c3f96c
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="deploying-web-applications-in-enterprise-scenarios-using-visual-studio-2010"></a>Bereitstellen von Webanwendungen in Enterprise-Szenarios, die mit Visual Studio 2010
====================
durch [Jason Lee](https://github.com/jrjlee)

[PDF herunterladen](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Diese Reihe von Lernprogrammen beschreibt Tools und Techniken, die Sie zum Bereitstellen von Webanwendungen in verschiedenen Unternehmensszenarien verwenden können. Es wird erläutert, wie am besten Technologien wie Visual Studio 2010, Microsoft Build Engine (MSBuild), Internet Information Services (IIS) 7.5, das IIS-Webbereitstellungstool (Web Deploy), Web Farm Framework (WFF) und Dienstprogramme wie VSDBCMD.exe zum Verwenden von zu vereinfachen Sie, und verwalten Sie den Bereitstellungsprozess. Sie enthält Übersichten und aufgabenorientierte Anleitungen, die Ihnen dabei helfen wird:
> 
> - Überprüfen Sie, und stellen Sie die Anforderungen für die Bereitstellung für eine Web-Anwendung von Enterprise-Skalierung her.
> - Konfigurieren Sie Tests, Staging und Produktion webserverumgebungen zur Unterstützung einer webbereitstellung.
> - Konfigurieren von Team Foundation Server (TFS) die fortlaufende Integration (CI) Prozesse, um automatisierte Web Deploy unterstützt.
> - Bereitstellen von Enterprise-Skalierung Webanwendungen auf verschiedene serverumgebungen mit unterschiedlichen Anforderungen und Einschränkungen.
> - Bereitstellen von Änderungen für Webanwendungen, die in anderen Server-Umgebungen ausgeführt werden.
> 
> > [!NOTE]
> > Während diese Lernprogramme für die Verwendung von TFS als CI-Server die Beschreibung ist die Anweisungen auf den CI leicht angepasst. Ein fundiertes Wissen TFS zu verstehen und nutzen die Lernprogramme erforderlich nicht.
> 
> 
> Für einen italienischen Übersetzung mit diesen Lernprogrammen, besuchen Sie [http://www.lucamorelli.it](http://www.lucamorelli.it).


## <a name="about-the-authors"></a>Über die Autoren

Jason Lee ist principal Technologist mit [Inhalt Master](http://www.contentmaster.com/) , in denen er arbeitet seit mit Microsoft-Produkten und Technologien, insbesondere in SharePoint und ASP.NET seit einigen Jahren. Jason besitzt einen PhD berechnen und ist zurzeit MCPD und MCTS zertifiziert. Erfahren Sie, Jasons technischen Blog unter [www.jrjlee.com](http://www.jrjlee.com/).

Benjamin Curry ist principal Technologist mit [Inhalt Master](http://www.contentmaster.com/) hat, die Whitepapers, SDK-Dokumentation, PowerPoint-Präsentationen und Trainer und online-Training Kurse während seiner Career geschrieben. Eine ursprüngliche Mitglied ASP.NET-Dokumentationsteam, hat er mit Microsoft Web-Technologien für über einem Jahrzehnt über gearbeitet.

## <a name="target-audience"></a>Zielgruppe

Dieser Satz Lernprogramme sind für ASP.NET Web-Anwendungsentwickler und Lösungsarchitekten, die Visual Studio 2010 verwenden, um unternehmensweite Webanwendungen zu erstellen. Um den größtmöglichen Nutzen aus dem Inhalt zu erhalten, sollten Sie mit Visual Studio 2010 vertraut sein, und haben grundlegende Kenntnisse von TFS, zusammen mit Berücksichtigung der Microsoft Web Platform-Technologien wie ASP.NET MVC 3, Windows Communication Foundation (WCF), IIS, SQL Server und Visual Studio-Datenbankprojekte. Sie müssen jedoch nicht mit Bereitstellungstools und Technologien vertraut sein, oder müssen wissen, wie zum Einrichten von CI-Systemen zu können.

## <a name="requirements"></a>Anforderungen

Die exemplarischen Vorgehensweisen, und führen Sie die Aufgaben, die diese Lernprogramme zu beschreiben, müssen Sie diese Software auf Ihrem Computer zu installieren:

- Visual Studio 2010 Premium oder Ultimate Edition mit Servicepack 1
- .NET Framework 4.0
- .NET Framework 3.5 mit Servicepack 1
- ASP.NET MVC 3.0
- IIS 7.5 Express
- SQLServer Express 2008 R2

Um die in diesen exemplarischen Vorgehensweisen beschriebenen Schritte zur Bereitstellung auszuführen, müssen Sie zum Beispiel Web Application-bereitstellungsumgebungen zugreifen. Optimale Ergebnisse zu erzielen sollte diese Umgebungen Bereitstellungsmuster für Ihre Organisation Unternehmen entsprechen. Sie können dann die exemplarischen Vorgehensweisen in dieser Dokumentation die bereitstellungsumgebungen und die Anforderungen Ihrer Organisation entsprechend ändern.

## <a name="series-contents"></a>Inhalt der Reihe

In diesem Abschnitt einführende besteht aus zwei weiteren Themen. Diese sind darauf ausgelegt, befolgen Sie, die Lernprogramme einen größeren Kontext bereit:

- [Web-Unternehmensbereitstellung: Szenarioübersicht](enterprise-web-deployment-scenario-overview.md). Dieses Thema beschreibt das Szenario, das jeweils die Lernprogramme in dieser Serie unterstützt. Das Szenario konzentriert sich auf die Application Lifecycle Management (ALM) für das fiktive Unternehmen mit dem Namen Fabrikam, Inc., wie sie eine Enterprise-Skalierung Web-Anwendung entwickelt.
- [Anwendungslebenszyklus-Verwaltung: Von der Entwicklung bis hin zur Produktion](application-lifecycle-management-from-development-to-production.md). Dieses Thema enthält eine allgemeine, für den End-to-End-Übersicht über einen Bereitstellungsprozess. Es wird veranschaulicht, wie eine Enterprise-Skalierung ASP.NET-Webanwendung über die Test-, Staging-und produktionsumgebungen als Teil eines Prozesses kontinuierliches Entwickeln von Fabrikam, Inc. bewegt wird.

Die Serie umfasst vier Tutorial Sätze. Jede konzentriert sich auf verschiedene Aspekte der Bereitstellung von:

- [Die webbereitstellung im Unternehmen](../web-deployment-in-the-enterprise/web-deployment-in-the-enterprise.md). Dieses Lernprogramm enthält eine grundlegende Einführung in MSBuild-Projektdateien, die Publishing Web-Pipeline, Web Deploy und anderen verwandten Technologien. Es wird erläutert, wie diese Tools gemeinsam verwendet werden können, um komplexe-bereitstellungstechnologien zu verwalten.
- [Konfigurieren von Serverumgebungen für die Bereitstellung](../configuring-server-environments-for-web-deployment/configuring-server-environments-for-web-deployment.md). In diesem Lernprogramm beschreibt, wie Windows-Servern zum unterstützen verschiedene Bereitstellungsszenarien, einschließlich remote-Web-paketbereitstellung mithilfe der Webbereitstellungs-Agent-Dienst (der "remote-Agent") oder Bereitstellen von Web-Handler und die Bereitstellung des Remotezugriffs zu konfigurieren. Sie erhalten Anweisungen zum Auswählen der geeigneten Bereitstellungsmethode für Ihre Umgebung, und es wird beschrieben, wie die WFF mithilfe bereitgestellter Webanwendungen über alle Webserver in einer Serverfarm repliziert.
- [Konfigurieren von Team Foundation Server für die Bereitstellung](../configuring-team-foundation-server-for-web-deployment/configuring-team-foundation-server-for-web-deployment.md). In diesem Lernprogramm wird beschrieben, wie zum Konfigurieren von TFS, sodass unterstützen verschiedene Bereitstellungsszenarien, einschließlich automatisierter Bereitstellung im Rahmen des CI-Prozess und Bereitstellungen von bestimmte Builds manuell ausgelöst wird.
- [Erweiterte Web Unternehmensbereitstellung](../advanced-enterprise-web-deployment/advanced-enterprise-web-deployment.md). In diesem Lernprogramm wird beschrieben, wie verschiedene erweiterte Bereitstellung, wie Datenbank-Bereitstellungen für mehrere Umgebungen anpassen, Ausschließen von Dateien und Ordner von der Bereitstellung und offline-Webanwendungen, die während des Bereitstellungsvorgangs Aufgaben .

## <a name="where-to-start"></a>Wo Sie beginnen

Diese Reihe von Lernprogrammen verwendet eine beispiellösung mit einer realistischen Maß an Komplexität, zusammen mit einem fiktiven Unternehmen-Bereitstellungsszenario, um eine referenzimplementierung bereitzustellen und um die Aufgaben und exemplarische Vorgehensweisen für einen allgemeinen Kontext zu gewähren. Im nächsten Thema [Web Unternehmensbereitstellung: Szenarioübersicht](enterprise-web-deployment-scenario-overview.md), werden das Szenario und der Beispielprojektmappe eingeführt. Von dort aus können Sie arbeiten über den Lernprogrammen und Themen, die Ihren Anforderungen am ehesten entsprechen.

>[!div class="step-by-step"]
[Nächste](enterprise-web-deployment-scenario-overview.md)
