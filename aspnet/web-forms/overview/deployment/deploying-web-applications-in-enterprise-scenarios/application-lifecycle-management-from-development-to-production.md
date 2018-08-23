---
uid: web-forms/overview/deployment/deploying-web-applications-in-enterprise-scenarios/application-lifecycle-management-from-development-to-production
title: 'Anwendungslebenszyklus-Verwaltung: Von der Entwicklung zur Produktion | Microsoft-Dokumentation'
author: jrjlee
description: In diesem Thema wird veranschaulicht, wie ein fiktives Unternehmen die Bereitstellung von einer ASP.NET-Webanwendung über die Test-, Staging-und produktionsumgebungen als Par verwaltet...
ms.author: riande
ms.date: 05/04/2012
ms.assetid: f97a1145-6470-4bca-8f15-ccfb25fb903c
msc.legacyurl: /web-forms/overview/deployment/deploying-web-applications-in-enterprise-scenarios/application-lifecycle-management-from-development-to-production
msc.type: authoredcontent
ms.openlocfilehash: 7cb9c949936c3af73d4c904d401c36d4d83f3e18
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2018
ms.locfileid: "41837712"
---
<a name="application-lifecycle-management-from-development-to-production"></a>Anwendungslebenszyklus-Verwaltung: Von der Entwicklung zur Produktion
====================
durch [Jason Lee](https://github.com/jrjlee)

[PDF herunterladen](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> In diesem Thema wird veranschaulicht, wie ein fiktives Unternehmen die Bereitstellung von einer ASP.NET-Webanwendung über die Test-, Staging-und produktionsumgebungen als Teil eines fortlaufenden Entwicklungsprozesses verwaltet. In der gesamten Thema werden Links zu zusätzlicher Informationen und exemplarische Vorgehensweisen zum Ausführen bestimmter Aufgaben angegeben.
> 
> Das Thema dient, geben Sie eine allgemeine Übersicht über die für eine [Reihe von Tutorials](deploying-web-applications-in-enterprise-scenarios.md) für webbereitstellung im Unternehmen. Keine Sorge, wenn Sie mit einigen der hier beschriebenen Konzepte nicht vertraut&#x2014;die Lernprogramme, die Folgen bieten ausführliche Informationen zu Aufgaben und Verfahren.
> 
> > [!NOTE]
> > Forthe halber werden hier der Einfachheit nicht in diesem Thema Aktualisieren der Datenbanken im Rahmen des Bereitstellungsprozesses behandelt werden. Allerdings inkrementelle Updates an Datenbankfunktionen vornehmen, ist eine Voraussetzung für viele Szenarien der Enterprise, und finden Sie Anleitungen dazu, wie weiter unten in dieser tutorialreihe dazu. Weitere Informationen finden Sie unter [Bereitstellen von Datenbankprojekten](../web-deployment-in-the-enterprise/deploying-database-projects.md).


## <a name="overview"></a>Übersicht

Der Bereitstellungsvorgang hier basiert darauf, dass das Fabrikam, Inc.-Bereitstellungsszenario, die in beschriebenen [webbasierte Unternehmensbereitstellung: Szenarioübersicht](enterprise-web-deployment-scenario-overview.md). Lesen Sie die Übersicht über das Szenario, bevor Sie in diesem Thema untersuchen. Im Wesentlichen das Szenario untersucht, wie eine Organisation die Bereitstellung einer recht komplexen Webanwendung, verwaltet die [Contact Manager-Lösung](../web-deployment-in-the-enterprise/the-contact-manager-solution.md), über die verschiedenen Phasen einer typischen unternehmensumgebung.

Contact Manager-Lösung führt auf einer hohen Ebene diese Phasen als Teil der Entwicklung und Bereitstellung:

1. Ein Entwickler überprüft Code die in Team Foundation Server (TFS) 2010.
2. TFS erstellt den Code und führt alle Komponententests mit dem Teamprojekt verknüpft ist.
3. Die Lösung wird von TFS in der testumgebung bereitgestellt.
4. Die Developer-Team überprüft und überprüft die Lösung in der testumgebung.
5. Der staging umgebungsadministrator führt eine "Was-wäre-wenn" Bereitstellung in der Stagingumgebung bereit, um herauszufinden, ob es sich bei die Bereitstellung alle Probleme verursachen.
6. Der staging umgebungsadministrator führt eine live-Bereitstellung in der Stagingumgebung bereit.
7. Die Lösung ausgeführt wird, Benutzerakzeptanztests, die in der Stagingumgebung.
8. Die Web-Bereitstellungspakete werden in der produktionsumgebung manuell importiert.

Diese Phasen bilden einen Teil eines fortlaufenden Entwicklungszyklus.

![](application-lifecycle-management-from-development-to-production/_static/image1.png)

In der Praxis ist der Prozess etwas komplizierter, als dies, wie Sie sehen werden, wenn wir jede Phase noch ausführlicher betrachten. Fabrikam, Inc. verwendet einen anderen Ansatz zur Bereitstellung für jede zielumgebung an.

![](application-lifecycle-management-from-development-to-production/_static/image2.png)

Im weiteren Verlauf dieses Themas werden diese wichtigsten Phasen dieses Bereitstellungslebenszyklus:

- **Erforderliche Komponenten**: wie müssen Sie Ihrer Serverinfrastruktur zu konfigurieren, bevor Sie Ihrer Bereitstellungslogik platziert.
- **Anfängliche Entwicklung und Bereitstellung**: Was Sie tun, bevor Sie die Projektmappe zum ersten Mal bereitstellen müssen.
- **Bereitstellung zum Testen**: das Verpacken und Bereitstellen von Inhalten für eine Test-Umgebung automatisch, wenn ein Entwickler im neuen Code eincheckt.
- **Bereitstellung in der Stagingumgebung**: bestimmte bereitstellen builds zu einer Stagingumgebung und das "Was-wäre-wenn" führen Sie Bereitstellungen, um sicherzustellen, dass es sich bei eine Bereitstellung Probleme dadurch keine.
- **Bereitstellung in der Produktion**: Web-Pakete in einer produktionsumgebung importieren, wenn Netzwerkinfrastruktur Remotebereitstellung verhindert.

## <a name="prerequisites"></a>Erforderliche Komponenten

Die erste Aufgabe in jedem Szenario wird sichergestellt, dass es sich bei Ihrer Serverinfrastruktur erfüllt die Anforderungen Ihrer Bereitstellungstools und Techniken. In diesem Fall hat Fabrikam, Inc. seine Serverinfrastruktur wie folgt konfiguriert:

- TFS ist für eine teamprojektauflistung enthalten, die Buildcontroller und build-Agents konfiguriert. Finden Sie unter [Konfigurieren von Team Foundation Server für die automatisierte Bereitstellung](../configuring-team-foundation-server-for-web-deployment/configuring-team-foundation-server-for-web-deployment.md) für Weitere Informationen.
- Die testumgebung ist so konfiguriert, dass um remote-Bereitstellungen mithilfe der Webbereitstellungs-Agent-Dienst ("remote-Agent"), zu akzeptieren, wie in beschrieben [Szenario: Konfigurieren einer Testumgebung für die Webbereitstellung](../configuring-server-environments-for-web-deployment/scenario-configuring-a-test-environment-for-web-deployment.md) und [ Konfigurieren Sie einen Webserver für Web Deploy-Veröffentlichung (Remote-Agent)](../configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-remote-agent.md).
- Die Stagingumgebung wird konfiguriert Remotebereitstellungen mit dem Bereitstellen von Web-Handler-Endpunkt, wie in beschrieben [Szenario: Konfigurieren von einer Staging-Umgebung für die Webbereitstellung](../configuring-server-environments-for-web-deployment/scenario-configuring-a-staging-environment-for-web-deployment.md) und [einen Webserver konfigurieren für Web Deploy-Veröffentlichung (Web Deploy-Handler)](../configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler.md).
- Die produktionsumgebung ist so konfiguriert, dass Administrator manuell der Web-Bereitstellungspakete in Internet Information Services (IIS), importieren können, wie in beschrieben [Szenario: Konfigurieren einer Produktionsumgebung für die Webbereitstellung](../configuring-server-environments-for-web-deployment/scenario-configuring-a-production-environment-for-web-deployment.md) und [konfigurieren Sie einen Webserver für Web Deploy-Veröffentlichung (Offlinebereitstellung)](../configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-offline-deployment.md).

## <a name="initial-development-and-deployment"></a>Anfängliche Entwicklung und Bereitstellung

Damit das Entwicklungsteam der Fabrikam, Inc. Contact Manager-Lösung zum ersten Mal bereitstellen kann, muss sie diese Aufgaben ausführen:

- Erstellen Sie ein neues Teamprojekt in TFS ein.
- Erstellen Sie die Microsoft Build Engine (MSBuild)-Projektdateien, die die Bereitstellungslogik enthalten.
- Erstellen Sie die TFS-Build-Definitionen, die die Bereitstellungsprozesse auslösen.

### <a name="create-a-new-team-project"></a>Erstellen Sie ein neues Teamprojekt

- Die TFS-Administrator, Rob Walters, erstellt ein neues Teamprojekt für die Anwendung aus, wie in beschrieben [Erstellen eines Teamprojekts in TFS](../configuring-team-foundation-server-for-web-deployment/creating-a-team-project-in-tfs.md). Als Nächstes erstellt der Entwicklungsleiter, Matt Hink, eine Skelette-Lösung. Überprüft er seine Dateien in das neue Teamprojekt in TFS an, wie in beschrieben [Inhalt hinzufügen, in die Quellcodeverwaltung](../configuring-team-foundation-server-for-web-deployment/adding-content-to-source-control.md).

### <a name="create-the-deployment-logic"></a>Erstellen Sie die Bereitstellungslogik

Matt Hink erstellt verschiedene benutzerdefinierte MSBuild-Projektdateien, geteilten Projekt Datei beschriebene Ansatz [Grundlegendes zur Projektdatei](../web-deployment-in-the-enterprise/understanding-the-project-file.md). Matt erstellt:

- Eine Projektdatei namens *Publish.proj* , die den Bereitstellungsprozess ausgeführt wird. Diese Datei enthält MSBuild-Ziele, die die Projekte in der Projektmappe zu erstellen, Webpaketen erstellen und Bereitstellen der Pakete in einer zielumgebung für die Server an.
- Umgebungsspezifische Projektdateien, die mit dem Namen *Env-Dev.proj* und *Env-Stage.proj*. Diese enthalten Einstellungen, die bzw. für die testumgebung und die Stagingumgebung spezifisch sind, wie Verbindungszeichenfolgen, Endpunkte und die Details des Remotediensts, die das Webpaket empfangen sollen. Anleitungen zum Auswählen der richtigen Einstellungen für bestimmte zielumgebungen finden Sie unter [Konfigurieren von Bereitstellungseigenschaften für eine Zielumgebung](../configuring-server-environments-for-web-deployment/configuring-deployment-properties-for-a-target-environment.md).

Führen Sie die Bereitstellung, die ein Benutzer führt die *Publish.proj* Datei mithilfe von MSBuild oder Team Build, und gibt den Speicherort der relevanten umgebungsspezifische Projektdatei (*Env-Dev.proj* oder *Env-Stage.proj*) als Befehlszeilenargument. Die *Publish.proj* -Datei importiert anschließend die umgebungsspezifische Projektdatei, um einen vollständigen Satz von Anweisungen für jede zielumgebung Veröffentlichung zu erstellen.

> [!NOTE]
> Die Arbeitsweise dieser benutzerdefinierten Projektdateien ist unabhängig von den Mechanismus an, die, den Sie verwenden, um den MSBuild aufrufen. Beispielsweise können Sie die MSBuild-Befehlszeile direkt, wie in beschrieben [Grundlegendes zur Projektdatei](../web-deployment-in-the-enterprise/understanding-the-project-file.md). Sie können die Projektdateien aus einer Befehlsdatei ausführen, wie in beschrieben [erstellen und Ausführen einer Befehlsdatei Bereitstellung](../web-deployment-in-the-enterprise/creating-and-running-a-deployment-command-file.md). Alternativ Sie können Ausführen der Projektdateien aus einer Builddefinition in TFS unter [Erstellen einer Builddefinition, dass unterstützt die Bereitstellung](../configuring-team-foundation-server-for-web-deployment/creating-a-build-definition-that-supports-deployment.md).  
> In jedem Fall ist das Endergebnis dasselbe&#x2014;MSBuild führt die zusammengeführte Datei und Ihre Lösung in der zielumgebung bereitgestellt. Dies bietet Ihnen ein hohes Maß an Flexibilität in, wie Sie Ihre Veröffentlichungsprozess auslösen.


Nachdem er die benutzerdefinierten Projektdateien erstellt, Matt fügt sie einen Projektmappenordner hinzu, und überprüft diese in die quellcodeverwaltung.

### <a name="create-build-definitions"></a>Builddefinitionen erstellen

Als letzte auftragsvorbereitungsaufgaben zusammenarbeiten, Matt und Rob Campbell um drei Build-Definitionen für das neue Teamprojekt zu erstellen:

- **DeployToTest**. Dies erstellt die Projektmappe Contact Manager, und jedes Mal, wenn ein Eincheckvorgang erfolgt in der testumgebung bereitstellt.
- **DeployToStaging**. Dies stellt Ressourcen aus einem angegebenen vorherigen Build in der Stagingumgebung bereit, wenn ein Entwickler den Build-Warteschlangen.
- **DeployToStaging-WhatIf**. Dies führt eine "Was-wäre-wenn" Bereitstellung in der Stagingumgebung bereit, wenn ein Entwickler den Build-Warteschlangen.

Die folgenden Abschnitte enthalten weitere Details zu jeder dieser build-Definitionen.

## <a name="deployment-to-test"></a>Bereitstellung für die Testumgebung

Das Entwicklungsteam bei Fabrikam, Inc. verwaltet die Test-Umgebungen, um eine Vielzahl von Softwaretests Aktivitäten, wie die Überprüfung und Validierung, Nutzbarkeitstests, Tests der Anwendungskompatibilität, und ad-hoc- oder explorativen Tests durchzuführen.

Das Entwicklungsteam hat eine Builddefinition in TFS mit dem Namen **DeployToTest**. Diese Builddefinition verwendet einen Auslöser für continuous Integration, was, dass der Buildprozess ausgeführt wird bedeutet, jedes Mal, wenn ein Mitglied über das Entwicklungsteam der Fabrikam, Inc. einen Eincheckvorgang ausführt. Wenn ein Build ausgelöst wird, wird die Build-Definition:

- Erstellen Sie die ContactManager.sln-Projektmappe. Dies wird wiederum jedes Projekt in der Projektmappe erstellt.
- Führen Sie Komponententests in der Ordnerstruktur für Projektmappen (wenn das Projekt erfolgreich erstellt).
- Führen Sie die benutzerdefinierte Projektdateien, die während des Bereitstellungsvorgangs (wenn die Lösung erfolgreich erstellt und alle Komponententests übergibt) zu steuern.

Das Endergebnis ist, dass die Lösung erfolgreich erstellt und übergibt Komponententests, die Webpakete und andere Bereitstellungsressourcen in der testumgebung bereitgestellt werden.

![](application-lifecycle-management-from-development-to-production/_static/image3.png)

### <a name="how-does-the-deployment-process-work"></a>Wie funktioniert der Bereitstellungsprozess?

Die **DeployToTest** Definition stellt diese Argumente an MSBuild zu erstellen:


[!code-console[Main](application-lifecycle-management-from-development-to-production/samples/sample1.cmd)]


Die **DeployOnBuild = True** und **DeployTarget = Paket** Eigenschaften werden verwendet, wenn die Projekte in der Projektmappe Team Build erstellt werden. Wenn das Projekt ein Webprojekt für die Anwendung ist, weisen Sie diese Eigenschaften MSBuild zum Erstellen eines Webbereitstellungspakets für das Projekt. Die **TargetEnvPropsFile** -Eigenschaft teilt die *Publish.proj* -Datei, in die zu importierende Projektdatei umgebungsspezifische gefunden.

> [!NOTE]
> Eine ausführliche exemplarische Vorgehensweise zum Erstellen einer Builddefinition wie folgt, finden Sie unter [Erstellen einer Builddefinition, dass unterstützt die Bereitstellung](../configuring-team-foundation-server-for-web-deployment/creating-a-build-definition-that-supports-deployment.md).


Die *Publish.proj* -Datei enthält die Ziele, die jedes Projekt in der Projektmappe zu erstellen. Es enthält jedoch auch bedingten Logik, überspringt diese Ziele zu erstellen, wenn Sie die Datei in Team Build ausgeführt werden. Dadurch können Sie die zusätzliche Funktionalität nutzen, die Team Build, sich z.B. die Möglichkeit zum Ausführen von Komponententests bietet. Wenn die Projektmappe erstellen oder die Einheit fehlschlagen Tests, die *Publish.proj* Datei wird nicht ausgeführt, und die Anwendung wird nicht bereitgestellt werden.

Die Bedingungslogik erfolgt durch die Auswertung der **BuildingInTeamBuild** Eigenschaft. Dies ist eine MSBuild-Eigenschaft, die automatisch, um festgelegt wird **"true"** Verwendung Team Build zum Erstellen von Projekten.

## <a name="deployment-to-staging"></a>Bereitstellung in der Stagingumgebung

Wenn ein Build mit den Anforderungen des Teams Developer in der testumgebung erfüllt, kann das Team denselben Build in einer Stagingumgebung bereitstellen möchten. Stagingumgebungen werden in der Regel entsprechend die Eigenschaften der Produktion oder "live"-Umgebung als eng wie möglich, z. B. in Bezug auf die Server-Spezifikationen, Betriebssysteme und Software und Netzwerkkonfiguration konfiguriert. Stagingumgebungen werden häufig für Auslastungstests, Tests der Benutzerakzeptanz und größeren internen Prüfungen verwendet werden. Builds werden direkt aus dem Build-Server in der Stagingumgebung bereitgestellt.

![](application-lifecycle-management-from-development-to-production/_static/image4.png)

Die Builddefinitionen, die zum Bereitstellen der Lösung in der Stagingumgebung **DeployToStaging-WhatIf** und **DeployToStaging**, weisen diese Merkmale auf:

- Es erstellt nicht wirklich etwas. Wenn die Lösung durch Rob in der Stagingumgebung bereitgestellt wird, möchte er einen bestimmten, vorhandenen Build bereitstellen, der bereits überprüft und in der testumgebung überprüft wurde. Builddefinitionen müssen lediglich die benutzerdefinierten Projektdateien ausgeführt, die den Bereitstellungsprozess steuern.
- Wenn Rob einen Build ausgelöst wird, verwendet er die Parameter, um anzugeben, welcher Build die Ressourcen enthält, die er auf dem Buildserver bereitstellen möchte.
- Der Build-Definitionen werden nicht automatisch ausgelöst. Rob Warteschlangen manuell einen Build aus, wenn er die Lösung in der Stagingumgebung bereitstellen möchte.

Dies ist der grundsätzliche Erstellungsprozess für eine Bereitstellung in der Stagingumgebung bereit:

1. Der Stagingumgebung umgebungsadministrator, Rob Walters, Warteschlangen, einen Build mithilfe der **DeployToStaging-WhatIf** Definition zu erstellen. Rob verwendet die Build-Definition-Parameter, an welchem Build, möchte er bereitstellen.
2. Die **DeployToStaging-WhatIf** Definition führt die benutzerdefinierte Projektdateien im "Was-wäre-wenn"-Modus zu erstellen. Dies generiert Protokolldateien, als ob Rob wurde eine live-Bereitstellung wird ausgeführt, aber es in der zielumgebung keine Änderungen vornehmen.
3. Rob werden die Protokolldateien, um die Auswirkungen der Bereitstellung in der Stagingumgebung zu ermitteln. Insbesondere möchte Rob überprüfen, was hinzugefügt werden, was aktualisiert wird und welche gelöscht werden.
4. Wenn er ist zufrieden sind, dass die Bereitstellung unerwünschten Änderungen an vorhandenen Ressourcen oder Daten vornehmen, wird nicht, er fügt einen Build verwenden die **DeployToStaging** Builddefinition.
5. Die **DeployToStaging** Definition führt die benutzerdefinierte Projektdateien zu erstellen. Diese veröffentlichen Sie die Bereitstellungsressourcen auf dem primären Webserver in der Stagingumgebung.
6. Der Controller des Web Farm Framework (WFF) wird die Webserver in der Stagingumgebung synchronisiert. Dadurch wird die Anwendung auf allen Webservern in der Serverfarm verfügbar.

### <a name="how-does-the-deployment-process-work"></a>Wie funktioniert der Bereitstellungsprozess?

Die **DeployToStaging** Definition stellt diese Argumente an MSBuild zu erstellen:


[!code-console[Main](application-lifecycle-management-from-development-to-production/samples/sample2.cmd)]


Die **TargetEnvPropsFile** -Eigenschaft teilt die *Publish.proj* -Datei, in die zu importierende Projektdatei umgebungsspezifische gefunden. Die **OutputRoot** Eigenschaft überschreibt die integrierten Wert und gibt den Speicherort der Ordner "Build" mit den Ressourcen bereitgestellt werden soll. Wenn Rob den Build-Warteschlangen, verwendet er die **Parameter** Tab, um einen aktualisierten Wert für die **OutputRoot** Eigenschaft.

![](application-lifecycle-management-from-development-to-production/_static/image5.png)

> [!NOTE]
> Weitere Informationen zum Erstellen einer Builddefinition wie folgt finden Sie unter [Bereitstellen von einem bestimmten Build](../configuring-team-foundation-server-for-web-deployment/deploying-a-specific-build.md).


Die **DeployToStaging-WhatIf** Builddefinition enthält die gleiche Bereitstellungslogik wie die **DeployToStaging** Definition zu erstellen. Es enthält jedoch das zusätzliche Argument **"WhatIf" = "true"**:


[!code-console[Main](application-lifecycle-management-from-development-to-production/samples/sample3.cmd)]


In der *Publish.proj* -Datei, die **"WhatIf"** Eigenschaft gibt an, dass alle Bereitstellungsressourcen "what if"-Modus veröffentlicht werden soll. Das heißt, werden die Protokolldateien generiert, als ob die Bereitstellung jetzt mehr, aber "nothing" in der zielumgebung tatsächlich geändert wird. Dadurch können Sie ermitteln können, die Auswirkungen der vorgeschlagenen Bereitstellung&#x2014;in bestimmten, was hinzugefügt wird, was aktualisiert wird und welche gelöscht werden&#x2014;, bevor Sie tatsächlich Änderungen vornehmen.

> [!NOTE]
> Weitere Informationen zum Konfigurieren von "what if"-Bereitstellungen finden Sie unter [Durchführen einer Bereitstellung "Was-wäre-wenn"](../advanced-enterprise-web-deployment/performing-a-what-if-deployment.md).


Nachdem Sie Ihre Anwendung mit dem primären Webserver in der Stagingumgebung bereitgestellt haben, werden die WFF die Anwendung automatisch auf allen Servern in der Serverfarm synchronisiert.

> [!NOTE]
> Weitere Informationen zum Konfigurieren der WFF zum Synchronisieren von Webservern finden Sie unter [erstellen Sie eine Serverfarm mit Web Farm Framework](../configuring-server-environments-for-web-deployment/creating-a-server-farm-with-the-web-farm-framework.md).


## <a name="deployment-to-production"></a>Bereitstellung in der Produktion

Wenn ein Build in der Stagingumgebung genehmigt wurde, kann das Team der Fabrikam, Inc. die Anwendung in der produktionsumgebung veröffentlichen. Der produktionsumgebung bereit ist, in dem die Anwendung wird "live", und erreicht die Zielgruppe von Endbenutzern.

Die produktionsumgebung ist in einem Umkreisnetzwerk Internetzugriff. Dies ist isoliert, aus dem internen Netzwerk, das den Buildserver enthält. Der Produktion umgebungsadministrator Lisa Andrews, muss manuell der Web-Pakete aus dem Build-Server kopieren und in IIS auf dem primären Produktions-Web-Server importieren.

![](application-lifecycle-management-from-development-to-production/_static/image6.png)

Dies ist der grundsätzliche Erstellungsprozess für eine Bereitstellung in der produktionsumgebung bereit:

1. Das Entwicklerteam weist Claudia, dass ein Build für die Bereitstellung für die Produktion bereit ist. Das Team empfiehlt Claudia des Speicherorts für die Web-Bereitstellungspakete in der Drop-Ordner auf dem Buildserver.
2. Claudia die Webpakete auf dem Buildserver erfasst und kopiert diese auf dem primären Webserver in der produktionsumgebung.
3. Claudia wird importieren und veröffentlichen die Webpakete auf dem primären Webserver IIS-Manager verwendet.
4. Der Controller WFF synchronisiert die Webserver in der produktionsumgebung. Dadurch wird die Anwendung auf allen Webservern in der Serverfarm verfügbar.

### <a name="how-does-the-deployment-process-work"></a>Wie funktioniert der Bereitstellungsprozess?

IIS-Manager enthält ein Paket Assistenten zum Importieren, die es einfach macht, Web-Pakete auf einer IIS-Website veröffentlichen. Eine exemplarische Vorgehensweise zum Ausführen dieses Verfahrens finden Sie unter [Manuelles Installieren von Webpaketen](../web-deployment-in-the-enterprise/manually-installing-web-packages.md).

## <a name="conclusion"></a>Schlussbemerkung

In diesem Thema bereitgestellten eine Abbildung der bereitstellungs-Lebenszyklus für eine typische Enterprise-Skalierung-Webanwendung.

In diesem Thema ist Teil einer Reihe von Tutorials, die Anleitungen zu verschiedenen Aspekten der Bereitstellung von Webanwendungen bereitstellen. Es gibt viele weitere Aufgaben und Überlegungen in den einzelnen Phasen des Bereitstellungsprozesses, und es ist nicht möglich, die sie in eine einzelne Exemplarische Vorgehensweise zu behandeln, in der Praxis. Weitere Informationen finden Sie in diesen Tutorials:

- [Webbereitstellung im Unternehmen](../web-deployment-in-the-enterprise/web-deployment-in-the-enterprise.md). Dieses Tutorial bietet eine umfassende Einführung in Web Deployment-Techniken, die mithilfe von MSBuild und der IIS-Webbereitstellungstool (Web Deploy).
- [Konfigurieren von Serverumgebungen für die Webbereitstellung](../configuring-server-environments-for-web-deployment/configuring-server-environments-for-web-deployment.md). Dieses Tutorial enthält Anweisungen zum Konfigurieren von Windows Server-Umgebungen, um verschiedene Szenarien zu unterstützen.
- [Konfiguration von Team Foundation Server für die Webbereitstellung automatisierter](../configuring-team-foundation-server-for-web-deployment/configuring-team-foundation-server-for-web-deployment.md). In diesem Tutorial enthält Anleitungen dazu, wie Sie Bereitstellungslogik in TFS-Buildprozesse integrieren.
- [Erweiterte webbasierte Unternehmensbereitstellung](../advanced-enterprise-web-deployment/advanced-enterprise-web-deployment.md). In diesem Tutorial enthält Anleitungen zum Teil der komplexen Herausforderungen bei der Bereitstellung entsprechen denen Unternehmen konfrontiert.

> [!div class="step-by-step"]
> [Vorherige](enterprise-web-deployment-scenario-overview.md)
