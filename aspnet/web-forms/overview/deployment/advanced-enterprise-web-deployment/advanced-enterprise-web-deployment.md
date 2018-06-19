---
uid: web-forms/overview/deployment/advanced-enterprise-web-deployment/advanced-enterprise-web-deployment
title: Erweiterte Enterprise Web Deploy | Microsoft Docs
author: jrjlee
description: In diesem Lernprogramm wird gezeigt, wie verschiedene Aufgaben ausführen, die erforderlich oder wünschenswert ist, in eine Vielzahl von Szenarien für Enterprise-Bereitstellung sind. Für eine italienische Translati...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 7dcaba80-f2ec-4db3-ad98-daadc3afdb49
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/advanced-enterprise-web-deployment/advanced-enterprise-web-deployment
msc.type: authoredcontent
ms.openlocfilehash: 892e494b6fde994c4d04952382e4d618d73cad5c
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
ms.locfileid: "30879932"
---
<a name="advanced-enterprise-web-deployment"></a>Erweiterte Unternehmensbereitstellung für Web
====================
durch [Jason Lee](https://github.com/jrjlee)

[PDF herunterladen](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> In diesem Lernprogramm wird gezeigt, wie verschiedene Aufgaben ausführen, die erforderlich oder wünschenswert ist, in eine Vielzahl von Szenarien für Enterprise-Bereitstellung sind.
> 
> Für einen italienischen Übersetzung mit diesen Lernprogrammen, besuchen Sie [ http://www.lucamorelli.it ](http://www.lucamorelli.it).


Dies ist Teil einer Reihe von Lernprogrammen, die auf der Basis der Enterprise-bereitstellungsanforderungen eines fiktiven Unternehmens mit dem Namen Fabrikam, Inc. Dieses Lernprogramm Zeichenreihe verwendet eine beispiellösung&#x2014;der [Vorgesetzten Kontakts](../web-deployment-in-the-enterprise/the-contact-manager-solution.md) Lösung&#x2014;zur Darstellung einer Webanwendung mit einer realistischen Maß an Komplexität, einschließlich einer ASP.NET MVC 3-Anwendung, einen Windows Communication Foundation (WCF)-Dienst, und ein Datenbankprojekt.

Die Bereitstellungsmethode das Herzstück mit diesen Lernprogrammen basiert auf der Teilung Datei Herangehensweise beschrieben [Verständnis des Build-Prozesses](../web-deployment-in-the-enterprise/understanding-the-build-process.md), in dem durch der Buildprozess gesteuert wird zwei Projektdateien&#x2014;enthält Erstellen Sie für jede zielumgebung und enthält umgebungsspezifische Einstellungen für Build- und Bereitstellungsprozess geltenden Anweisungen, an. Zur Buildzeit ist die Unabhängigkeit von der Umgebung-Projektdatei, einen vollständigen Satz von Buildanweisungen bilden die Projektdatei umgebungsspezifische zusammengeführt.

## <a name="scenario-overview"></a>Übersicht über das Szenario

Das allgemeine Szenario für diesen Lernprogrammen wird beschrieben, [Web Unternehmensbereitstellung: Szenarioübersicht](../deploying-web-applications-in-enterprise-scenarios/enterprise-web-deployment-scenario-overview.md). Es wird empfohlen, dass Sie in diesem Thema behandelt, bevor Sie auf diesem Lernprogramm beginnen.

## <a name="how-to-use-this-tutorial"></a>Verwendung dieses Tutorials

- Jede in den Themen in diesem Lernprogramm ist in sich geschlossen und Adressen eine besondere Herausforderung oder ein Problem, das in Bereitstellungsszenarios auftritt. Sie müssen nicht über die folgenden Themen in einer bestimmten Reihenfolge zu arbeiten. Dieses Lernprogramm umfasst jedoch einige erweiterten Aufgaben. Daher ist es möglich, Sie sollten sich mit vertraut machen die Konzepte und Techniken, die die [Webbereitstellung im Unternehmen](../web-deployment-in-the-enterprise/web-deployment-in-the-enterprise.md) Lernprogramm behandelt wird, um den größten nutzen diesen Inhalt zugreifen zu können.
- Dieses Lernprogramm umfasst die folgenden Themen:
- [Ausführen einer "Was-wäre-wenn" Bereitstellung](performing-a-what-if-deployment.md). In einer Vielzahl von Szenarien sollten Sie die Auswirkungen einer vorgeschlagenen Bereitstellung in einer zielumgebung oder jeglichen vorhandenen Inhalt zu ermitteln, bevor Sie Änderungen tatsächlich vornehmen zu können. In diesem Thema wird beschrieben, wie Sie eine "Was-wäre-wenn" Bereitstellung zum Generieren von Protokolldateien und Update Datenbankskripts, als ob Sie Inhalte in einer zielumgebung bereitgestellt hatten, ohne jegliche Änderungen tatsächlich vornehmen ausführen können. Analysieren diese Ressourcen hilft Ihnen, potenzielle Probleme im Voraus über eine aktive Bereitstellung.
- [Anpassen von Datenbank-Bereitstellungen für mehrere Umgebungen](customizing-database-deployments-for-multiple-environments.md). Wenn Sie ein Datenbankprojekt an mehrere Ziele bereitstellen, müssen Sie häufig die Bereitstellungseigenschaften für jedes zielumgebung anpassen möchten. Beispielsweise würde in testumgebungen Sie in der Regel die Datenbank bei jeder Bereitstellung neu erstellen hingegen in Staging-oder produktionsumgebung Sie viel wahrscheinlicher damit inkrementelle Updates für Ihre Daten zu erhalten werden würde. In diesem Thema wird beschrieben, wie Sie diese eigenschaftsänderungen in Ihre Bereitstellung Logik integrieren können, durch das Erstellen einer Konfigurationsdatei umgebungsspezifische Bereitstellung (.sqldeployment) für jede zielumgebung.
- Bereitstellen von Datenbank-Rollenmitgliedschaften für Testumgebungen aus. Wenn Sie eine Datenbank bei jeder Bereitstellung neu erstellen&#x2014;z. B. als Teil einer continuous Integration (CI) erstellen und die in einer testumgebung bereitstellen&#x2014;in der Regel müssen Sie Rollenmitgliedschaften für die Datenbank jedes Mal konfigurieren. Beispielsweise müssen Sie in der Regel Erteilen von Berechtigungen für die Identität des Anwendungspools der Webanwendung zugeordnet. In diesem Thema wird beschrieben, wie dieser Vorgang automatisiert werden können, indem Ihre Logik für die Bereitstellung einer SQL­Skript nach der Bereitstellung hinzugefügt.
- [Bereitstellen von Datenbanken Mitgliedschaft in Enterprise-Umgebungen](deploying-membership-databases-to-enterprise-environments.md). ASP.NET Membership Datenbanken haben verschiedene Eigenschaften, die den Bereitstellungsprozess erschweren können. Beispielsweise wird eine Schema only-Bereitstellung die Datenbank in einen nicht betriebsfähigen Zustand belassen. In den meisten Szenarien ist es vorzuziehen, um eine Mitgliedschaftsdatenbank direkt in jedes zielumgebung zu erstellen. Jedoch, wenn Sie eine Mitgliedschaftsdatenbank bereitgestellt haben, wird in diesem Thema einige der Ansätze, die Sie verwenden können, auf den inhärenten Auflagen erfüllt werden können.
- [Ausschließen von Dateien und Ordner von der Bereitstellung](excluding-files-and-folders-from-deployment.md). In einigen Szenarien möchten Sie den Inhalt des Webpakets auf bestimmte zielumgebungen anzupassen. Sie möchten z. B. Vollversionen von JavaScript-Bibliotheken enthalten, bei der Bereitstellung in einer testumgebung, clientseitiges Debuggen aber verkleinerte Versionen der Bibliotheken verwenden, wenn Sie in einer Staging- oder produktionsumgebung bereitstellen. In diesem Thema wird beschrieben, wie Sie bestimmte Dateien und Ordner aus der Paketerstellungsprozess ausschließen können.
- [Erstellen von Webanwendungen Offline mit Web Deploy](taking-web-applications-offline-with-web-deploy.md). Wenn Sie Lösungen in einer Staging- oder produktionsumgebung bereitstellen, möchten Sie häufig Ihre Webanwendungen für die Dauer des Bereitstellungsprozesses erfolgen soll. In diesem Thema wird erläutert, wie Sie hinzufügen, können ein *App\_offline.htm* Datei wird in der Webanwendung zu Beginn des Bereitstellungsprozesses und entfernen Sie sie am Ende. Während der *App\_offline.htm* Datei ist vorhanden, die Benutzer, die an die Web-Anwendung durchsuchen werden automatisch umgeleitet, auf die *App\_offline.htm* Datei.
- [Ausführen von Windows PowerShell-Skripts von MSBuild](running-windows-powershell-scripts-from-msbuild-project-files.md). Viele Szenarien erfordern komplexere Aktionen nach der Bereitstellung, wie benutzerdefinierte Ereignisquellen zur Registrierung hinzufügen oder Konfigurieren der Replikation zwischen SQL Server-Instanzen. Diese Aktionen werden oft über Windows PowerShell-Skripts ausgeführt werden. In diesem Thema wird beschrieben, wie Windows PowerShell-Skripts aus einer Projektdatei Microsoft Build Engine (MSBuild) als Teil des Prozesses Build- und Bereitstellungsprozess ausgeführt wird.
- [Problembehandlung des Verpackungsprozesses](troubleshooting-the-packaging-process.md). Die Publishing Web Pipeline (WPP) definiert eine MSBuild-Eigenschaft, die mit dem Namen **EnablePackageProcessLoggingAndAssert** , Sie verwenden können, um ausführliche Informationen zu des Verpackungsprozesses für Webanwendungsprojekte zu generieren. In diesem Thema wird beschrieben, was bewirkt, dass die Eigenschaft und Ihre Verwendung.

## <a name="key-technologies"></a>Schlüsseltechnologien

Dieses Lernprogramm konzentriert sich auf wie dieser Produkte und Technologien verwenden, um automatisierte Build- und Web Deploy zu unterstützen:

- Visual Studio 2010 und Team Foundation Server (TFS 2010)
- MSBuild und TFS-Teambuild
- Internet Information Services (IIS) 7.5
- IIS Web Webbereitstellungstool (Web Deploy) 2.1
- Das bereitstellungshilfsprogramm für VSDBCMD.exe-Datenbank

## <a name="other-tutorials-in-this-series"></a>Weitere Lernprogramme in dieser Serie

Dies bildet einen Teil einer Reihe von fünf Lernprogramme auf Unternehmensebene zur Bereitstellung. Dies sind die anderen Lernprogramme in der Reihe:

- [Bereitstellen von Webanwendungen in Enterprise-Szenarios](../deploying-web-applications-in-enterprise-scenarios/deploying-web-applications-in-enterprise-scenarios.md). Diese einführenden Inhalt enthält kontextbezogenen Hintergrund für die Reihe von Lernprogrammen. Das Szenario des Lernprogramme beschrieben und veranschaulicht, wie die Aufgaben und exemplarische Vorgehensweisen beschrieben, die in der gesamten Reihe in einen größeren Application Lifecycle Management (ALM) Prozess passen.
- [Die webbereitstellung im Unternehmen](../web-deployment-in-the-enterprise/web-deployment-in-the-enterprise.md). Dieses Lernprogramm enthält eine grundlegende Einführung in MSBuild-Projektdateien, die WPP, Web Deploy und anderen verwandten Technologien. Es wird erläutert, wie diese Tools gemeinsam verwendet werden können, um komplexe-bereitstellungstechnologien zu verwalten.
- [Konfigurieren von Serverumgebungen für die Bereitstellung](../configuring-server-environments-for-web-deployment/configuring-server-environments-for-web-deployment.md). In diesem Lernprogramm beschreibt, wie Windows-Servern zum unterstützen verschiedene Bereitstellungsszenarien, einschließlich remote-Web-paketbereitstellung mithilfe der Webbereitstellungs-Agent-Dienst (der remote-Agent) oder Bereitstellen von Web-Handler und die Bereitstellung des Remotezugriffs zu konfigurieren. Sie erhalten Anweisungen zum Auswählen der geeigneten Bereitstellungsmethode für Ihre Umgebung, und es wird beschrieben, wie der Web Farm Framework (WFF) verwenden, um bereitgestellter Webanwendungen über alle Webserver in einer Serverfarm zu replizieren.
- [Konfigurieren von Team Foundation Server für die Bereitstellung](../configuring-team-foundation-server-for-web-deployment/configuring-team-foundation-server-for-web-deployment.md). In diesem Lernprogramm wird beschrieben, wie zum Konfigurieren von TFS, sodass unterstützen verschiedene Bereitstellungsszenarien, einschließlich automatisierter Bereitstellung im Rahmen des CI-Prozess und Bereitstellungen von bestimmte Builds manuell ausgelöst wird.

> [!div class="step-by-step"]
> [Nächste](performing-a-what-if-deployment.md)
