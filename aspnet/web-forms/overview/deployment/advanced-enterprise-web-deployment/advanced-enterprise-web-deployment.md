---
uid: web-forms/overview/deployment/advanced-enterprise-web-deployment/advanced-enterprise-web-deployment
title: Erweiterte webbasierte Unternehmensbereitstellung | Microsoft-Dokumentation
author: jrjlee
description: In diesem Tutorial erfahren Sie, wie Sie verschiedene Aufgaben ausführen, die erforderlich oder wünschenswert ist, eine Vielzahl von Bereitstellungsszenarios. Für eine italienische Translati...
ms.author: riande
ms.date: 05/04/2012
ms.assetid: 7dcaba80-f2ec-4db3-ad98-daadc3afdb49
msc.legacyurl: /web-forms/overview/deployment/advanced-enterprise-web-deployment/advanced-enterprise-web-deployment
msc.type: authoredcontent
ms.openlocfilehash: 34f1bf3bc2c37afc66f458a60a29fe5ce8f6c018
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2018
ms.locfileid: "41832345"
---
<a name="advanced-enterprise-web-deployment"></a>Erweiterte webbasierte Unternehmensbereitstellung
====================
durch [Jason Lee](https://github.com/jrjlee)

[PDF herunterladen](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> In diesem Tutorial erfahren Sie, wie Sie verschiedene Aufgaben ausführen, die erforderlich oder wünschenswert ist, eine Vielzahl von Bereitstellungsszenarios.
> 
> Ein italienischen Übersetzung mit diesen Lernprogrammen finden Sie unter [ http://www.lucamorelli.it ](http://www.lucamorelli.it).


Dies ist Teil einer Reihe von Tutorials, die auf der Basis der bereitstellungsanforderungen Enterprise ein fiktives Unternehmen, die mit dem Namen Fabrikam, Inc. Dieser tutorialreihe verwendet eine beispiellösung&#x2014;der [Contact Manager](../web-deployment-in-the-enterprise/the-contact-manager-solution.md) Lösung&#x2014;zur Darstellung einer Webanwendung mit einem realistischen Maß an Komplexität, einschließlich einer ASP.NET MVC 3-Anwendung, eine Windows-Kommunikation Foundation (WCF)-Dienst und ein Datenbankprojekt.

Die Methode für die Bereitstellung das Kernstück des in diesen Tutorials basiert auf den geteilten Projekt Dateiansatz beschrieben, die [Verständnis des Prozesses erstellen](../web-deployment-in-the-enterprise/understanding-the-build-process.md), in dem der Buildprozess durch gesteuert wird zwei Projektdateien&#x2014;enthält Erstellen Sie die Anweisungen, die für jede zielumgebung, und enthält umgebungsspezifische Build & Deployment-Einstellungen gelten. Zur Erstellungszeit wird die umgebungsspezifischen-Projektdatei in die Unabhängigkeit von der Umgebung Projektdatei, um einen vollständigen Satz von einrichtungsanweisungen bilden zusammengeführt.

## <a name="scenario-overview"></a>Übersicht über das Szenario

Das allgemeine Szenario für diese Tutorials finden Sie im [webbasierte Unternehmensbereitstellung: Szenarioübersicht](../deploying-web-applications-in-enterprise-scenarios/enterprise-web-deployment-scenario-overview.md). Es wird empfohlen, in diesem Thema lesen, bevor Sie mit diesem Tutorial beginnen.

## <a name="how-to-use-this-tutorial"></a>Verwendung dieses Tutorials

- Jede in den Themen in diesem Tutorial ist in sich geschlossen und löst eine spezielle Herausforderung oder ein Problem, das in den Bereitstellungsszenarios auftritt. Sie müssen nicht über diese Themen, in einer bestimmten Reihenfolge arbeiten. In diesem Tutorial wird jedoch einige erweiterten Aufgaben behandelt. Daher ist es möglich, Sie sollten sich vertraut machen mit den Konzepten und Techniken, die die [Webbereitstellung im Unternehmen](../web-deployment-in-the-enterprise/web-deployment-in-the-enterprise.md) Tutorial erfahren Sie, um den größten Nutzen aus diesen Inhalt zu erhalten.
- In diesem Lernprogramm umfasst folgende Themen:
- [Durchführen einer Bereitstellung "Was-wäre-wenn"](performing-a-what-if-deployment.md). In vielen Szenarien sollten Sie die Auswirkungen der vorgeschlagenen Bereitstellung in einer zielumgebung oder jeglichen vorhandenen Inhalt zu ermitteln, bevor Sie tatsächlich Änderungen vornehmen. In diesem Thema wird beschrieben, wie bei der Ausführung einer Bereitstellung "what if", um die Protokolldateien und Update von Datenbankskripts zu generieren, als wenn Sie Inhalte in einer zielumgebung bereitgestellt haben, ohne tatsächlich Änderungen vorzunehmen. Analysieren diese Ressourcen können Sie potenzielle Probleme im Voraus über eine live-Bereitstellung erkennen können.
- [Anpassen von Datenbankbereitstellungen für mehrere Umgebungen](customizing-database-deployments-for-multiple-environments.md). Wenn Sie ein Datenbankprojekt an mehrere Ziele bereitstellen, werden Sie häufig die Bereitstellungseigenschaften für jede zielumgebung anpassen möchten. Beispielsweise würde in testumgebungen Sie in der Regel die Datenbank bei jeder Bereitstellung, neu erstellen in Staging-oder produktionsumgebung Sie damit inkrementelle Updates für Ihre Daten zu erhalten sehr viel wahrscheinlicher wäre. In diesem Thema wird beschrieben, wie Sie diese eigenschaftenänderungen in Ihrer Bereitstellungslogik integrieren können, erstellen Sie eine Konfigurationsdatei umgebungsspezifische-Bereitstellung (.sqldeployment) für jede zielumgebung.
- Bereitstellen von Datenbankrollenmitgliedschaften in Testumgebungen. Wenn Sie eine Datenbank bei jeder Bereitstellung neu erstellen&#x2014;angenommen, Sie als Teil einer continuous Integration (CI), erstellen und Bereitstellen in einer testumgebung&#x2014;benötigten, in der Regel so konfigurieren Sie Datenbank-Rollenmitgliedschaften jedes Mal. Beispielsweise müssen Sie in der Regel Gewähren von Berechtigungen für die Identität des Anwendungspools der Webanwendung zugeordnet. In diesem Thema wird beschrieben, wie Sie diesen Prozess automatisieren, indem Sie eine SQL­Skript nach der Bereitstellung Ihrer Bereitstellungslogik hinzufügen.
- [Bereitstellen von Datenbankrollenmitgliedschaften in Unternehmensumgebungen](deploying-membership-databases-to-enterprise-environments.md). ASP.NET Datenbankrollenmitgliedschaften haben verschiedene Merkmale, die während des Bereitstellungsvorgangs erschweren können. Z. B. wird eine Schema only-Bereitstellung die Datenbank in einen nicht betriebsfähigen Zustand belassen. In den meisten Szenarien ist es besser, eine Mitgliedschaftsdatenbank direkt in jede zielumgebung zu erstellen. Jedoch wenn Sie zum Bereitstellen einer Mitgliedschaftsdatenbank verfügen, in diesem Thema werden einige beschrieben die Ansätze, die Sie verwenden können, um die unvermeidlichen Herausforderungen im Zusammenhang zu erfüllen.
- [Ausschließen von Dateien und Ordner von der Bereitstellung](excluding-files-and-folders-from-deployment.md). In einigen Szenarios sollten Sie den Inhalt Ihrer Webpakets für bestimmte zielumgebungen anpassen. Beispielsweise empfiehlt es sich um die vollständige Versionen von JavaScript-Bibliotheken enthalten, bei der Bereitstellung in einer testumgebung, um clientseitiges Debuggen zu unterstützen, aber die minimierte Versionen der Bibliotheken verwenden, wenn Sie in einer Staging- oder produktionsumgebung bereitstellen. In diesem Thema wird beschrieben, wie Sie bestimmte Dateien und Ordner aus der Paketerstellungsprozess ausschließen können.
- [Offlineschalten von Webanwendungen mit Web bereitstellen](taking-web-applications-offline-with-web-deploy.md). Wenn Sie Lösungen in einer Staging- oder produktionsumgebung bereitstellen, sollten Sie häufig Ihre Webanwendungen für die Dauer des Bereitstellungsprozesses erfolgen soll. In diesem Thema wird beschrieben, wie Sie hinzufügen können eine *App\_offline.htm* -Datei in Ihre Webanwendung zu Beginn des Bereitstellungsprozesses, und entfernen Sie sie am Ende. Während der *App\_offline.htm* Datei vorhanden ist, alle Benutzer, die an die Webanwendung durchsuchen werden automatisch zur der *App\_offline.htm* Datei.
- [Ausführen von Windows PowerShell-Skripts aus MSBuild](running-windows-powershell-scripts-from-msbuild-project-files.md). Viele Szenarien erfordern eine komplexere Aktionen nach der Bereitstellung, wie benutzerdefinierte Ereignisquellen in der Registrierung hinzufügen oder Konfigurieren der Replikation zwischen SQL Server-Instanzen. Diese Aktionen werden häufig über Windows PowerShell-Skripts ausgeführt werden. In diesem Thema wird beschrieben, wie Windows PowerShell-Skripts aus einer Projektdatei Microsoft Build Engine (MSBuild) als Teil des Build & Deployment-Prozesses ausgeführt wird.
- [Problembehandlung des Verpackungsprozesses](troubleshooting-the-packaging-process.md). Die Web Publishing Pipeline (WPP) definiert eine MSBuild-Eigenschaft, die mit dem Namen **EnablePackageProcessLoggingAndAssert** , Sie verwenden können, um ausführliche Informationen über den Prozess der paketerstellung für Webanwendungsprojekte zu generieren. In diesem Thema wird beschrieben, was bewirkt, dass die Eigenschaft und Ihre Verwendung.

## <a name="key-technologies"></a>Schlüsseltechnologien

In diesem Tutorial geht es um die Produkte und Technologien zu verwenden, um automatische Builds und webbereitstellung zu unterstützen:

- Visual Studio 2010 und Team Foundation Server (TFS) 2010
- MSBuild und TFS Teambuild
- Internetinformationsdienste (IIS) 7.5
- IIS-Webbereitstellungstool (Web Deploy) 2.1
- Das bereitstellungshilfsprogramm für VSDBCMD.exe-Datenbank

## <a name="other-tutorials-in-this-series"></a>Andere Tutorials in dieser Reihe

Dies ist Teil einer Reihe von fünf Lernprogrammen auf Unternehmensniveau webbereitstellung. Dies sind die anderen Tutorials der Reihe:

- [Bereitstellen von Webanwendungen in Unternehmensszenarien](../deploying-web-applications-in-enterprise-scenarios/deploying-web-applications-in-enterprise-scenarios.md). Dieser einführenden Inhalt enthält die kontextbezogenen Hintergrundinformationen zum der Tutorial-Reihe. Das tutorialszenario beschrieben und veranschaulicht, wie die Aufgaben und exemplarische Vorgehensweisen beschrieben, die in der gesamten Reihe in einem größeren Application Lifecycle Management (ALM)-Prozess passen.
- [Webbereitstellung im Unternehmen](../web-deployment-in-the-enterprise/web-deployment-in-the-enterprise.md). Dieses Tutorial bietet eine grundlegende Einführung in MSBuild-Projektdateien, die Apps, Web Deploy und andere verwandten Technologien. Es wird erläutert, wie diese Tools zusammen verwenden werden können, um komplexe Bereitstellungsprozesse zu verwalten.
- [Konfigurieren von Serverumgebungen für die Webbereitstellung](../configuring-server-environments-for-web-deployment/configuring-server-environments-for-web-deployment.md). In diesem Tutorial wird beschrieben, wie Windows Server zur Unterstützung verschiedener Bereitstellungsszenarien, einschließlich der remote-Web-Paket-Bereitstellung mithilfe der Webbereitstellungs-Agent-Dienst (der remote-Agent) oder Bereitstellen von Web-Handler und remote-Datenbank-Bereitstellung konfiguriert. Sie erhalten Anweisungen zum Auswählen der geeigneten Bereitstellungsmethode für Ihre eigene Umgebung, und es wird beschrieben, wie das Web Farm Framework (WFF) verwenden, um bereitgestellten Webanwendungen für alle Webserver in einer Serverfarm zu replizieren.
- [Konfigurieren von Team Foundation Server für die Webbereitstellung](../configuring-team-foundation-server-for-web-deployment/configuring-team-foundation-server-for-web-deployment.md). In diesem Tutorial wird beschrieben, wie zum Konfigurieren von TFS zur Unterstützung verschiedener Bereitstellungsszenarien, einschließlich der automatisierten Bereitstellung im Rahmen eines CI-Prozesses und bestimmte Builds Bereitstellungen manuell ausgelöst wird.

> [!div class="step-by-step"]
> [Nächste](performing-a-what-if-deployment.md)
