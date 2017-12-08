---
uid: web-forms/overview/deployment/deploying-web-applications-in-enterprise-scenarios/application-lifecycle-management-from-development-to-production
title: 'Anwendungslebenszyklus-Verwaltung: Von der Entwicklung bis hin zur Produktion | Microsoft Docs'
author: jrjlee
description: "In diesem Thema wird veranschaulicht, wie ein fiktives Unternehmen für die Bereitstellung von eine ASP.NET-Webanwendung durch Test-, Staging-und produktionsumgebungen als Par verwaltet..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: f97a1145-6470-4bca-8f15-ccfb25fb903c
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/deploying-web-applications-in-enterprise-scenarios/application-lifecycle-management-from-development-to-production
msc.type: authoredcontent
ms.openlocfilehash: f7ffff1c3434ce98c70265e4bf64047fd44252d0
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="application-lifecycle-management-from-development-to-production"></a>Anwendungslebenszyklus-Verwaltung: Von der Entwicklung bis hin zur Produktion
====================
durch [Jason Lee](https://github.com/jrjlee)

[PDF herunterladen](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> In diesem Thema wird veranschaulicht, wie ein fiktives Unternehmen für die Bereitstellung von eine ASP.NET-Webanwendung durch Test-, Staging-und produktionsumgebungen als Teil eines Prozesses kontinuierliches entwickeln verwaltet. In der gesamten Thema sind Links zu zusätzlicher Informationen und Vorgehensweisen zum Ausführen bestimmter Aufgaben bereitgestellt.
> 
> Das Thema dient zum bieten einer allgemeinen Übersicht über die für eine [Reihe von Lernprogrammen](deploying-web-applications-in-enterprise-scenarios.md) auf webbereitstellung im Unternehmen. Keine Sorge, wenn Sie nicht mit einigen der hier beschriebenen Konzepte & #x 2014 vertraut sind, die Lernprogramme, die Folgen enthalten ausführliche Informationen für alle diese Aufgaben und Verfahren.
> 
> > [!NOTE]
> > Forthe der Einfachheit halber in diesem Thema nicht aktualisieren Datenbanken im Rahmen des Bereitstellungsprozesses erläutert. Allerdings inkrementelle Updates auf Datenbankfunktionen ist eine Voraussetzung für viele Szenarien der Enterprise und Anleitungen finden Sie auf der erläutert, wie dies weiter unten in diesem Lernprogramm ausgeführt. Weitere Informationen finden Sie unter [Datenbankprojekte bereitstellen](../web-deployment-in-the-enterprise/deploying-database-projects.md).


## <a name="overview"></a>Übersicht

Der Bereitstellungsvorgang hier basiert auf der Fabrikam, Inc.-Bereitstellungsszenario, die in beschriebenen [Web Unternehmensbereitstellung: Szenarioübersicht](enterprise-web-deployment-scenario-overview.md). Lesen Sie die Übersicht über das Szenario, bevor Sie untersuchen, das in diesem Thema. Im Wesentlichen das Szenario untersucht, wie eine Organisation die Bereitstellung einer relativ komplex Webanwendung verwaltet die [Kontakt-Manager-Lösung](../web-deployment-in-the-enterprise/the-contact-manager-solution.md), über die verschiedenen Phasen in einer typischen unternehmensumgebung.

Auf hoher Ebene durchläuft diese Phasen als Teil des Entwicklungs- und Bereitstellungsprozess die Kontakt-Manager-Lösung:

1. Ein Entwickler überprüft Code in Team Foundation Server (TFS) 2010.
2. TFS erstellt den Code und alle Komponententests mit dem Teamprojekt verbunden sind.
3. TFS wird die Projektmappe in der testumgebung bereitgestellt.
4. Das Entwicklerteam überprüft und überprüft die Lösung in der testumgebung.
5. Der staging-Umgebung-Administrator führt eine "Was-wäre-wenn" Bereitstellung in der Stagingumgebung, um festzustellen, ob es sich bei die Bereitstellung Probleme verursacht.
6. Der staging-Umgebung-Administrator führt eine aktive Bereitstellung in der Stagingumgebung.
7. Die Lösung unterliegt Benutzerakzeptanztests in der Stagingumgebung testen.
8. Die Web-Bereitstellungspakete werden in der produktionsumgebung manuell importiert.

Diese Phasen bilden zum Teil einer fortlaufenden Entwicklungszyklus.

![](application-lifecycle-management-from-development-to-production/_static/image1.png)

In der Praxis ist der Prozess etwas komplizierter ist, als, wie Sie sehen, wenn wir in den einzelnen Phasen ausführlich betrachten. Fabrikam, Inc. verwendet einen anderen Ansatz für die Bereitstellung für jede zielumgebung.

![](application-lifecycle-management-from-development-to-production/_static/image2.png)

Im weiteren Verlauf dieses Themas überprüft diese Schlüssel Phasen dieses Bereitstellungslebenszyklus:

- **Erforderliche Komponenten**: wie müssen Sie Ihrer Serverinfrastruktur konfigurieren, bevor Sie Ihre Bereitstellung Logik Direktes Einfügen.
- **Entwicklung und Bereitstellung der ersten**: Was Sie tun, bevor Sie die Projektmappe zum ersten Mal bereitstellen müssen.
- **Bereitstellung zum Testen**: das Verpacken und Bereitstellen von Inhalten für eine testumgebung automatisch, wenn ein Entwickler im neuen Code eincheckt.
- **Bereitstellung in die Stagingumgebung**: Bereitstellen von bestimmten erstellt einer Stagingumgebung und zum Ausführen "Was-wäre-wenn-Bereitstellungen, um sicherzustellen, dass eine Bereitstellung Probleme auftreten, wird nicht.
- **Bereitstellung bis hin zur Produktion**: Webpaketen in einer produktionsumgebung zu importieren, wenn Netzwerkinfrastruktur Remotebereitstellung verhindert.

## <a name="prerequisites"></a>Erforderliche Komponenten

Die erste Aufgabe in jedem Bereitstellungsszenario wird sichergestellt, dass der Server-Infrastruktur die Anforderungen Ihrer Bereitstellungstools und Techniken entspricht. In diesem Fall hat Fabrikam, Inc. die Serverinfrastruktur wie folgt konfiguriert:

- TFS ist so konfiguriert, dass eine Teamprojektsammlung umfasst, Buildcontroller und build-Agents. Finden Sie unter [Konfigurieren von Team Foundation Server für die automatisierte Bereitstellung](../configuring-team-foundation-server-for-web-deployment/configuring-team-foundation-server-for-web-deployment.md) für Weitere Informationen.
- Die testumgebung ist so konfiguriert, dass Remotebereitstellungen mithilfe der Webbereitstellungs-Agent-Dienst (der "remote-Agent"), akzeptiert, wie in beschrieben [Szenario: Konfigurieren einer Umgebung testen für Webbereitstellung](../configuring-server-environments-for-web-deployment/scenario-configuring-a-test-environment-for-web-deployment.md) und [ Konfigurieren Sie einen Webserver für Web Deploy-Veröffentlichung (Remote-Agent)](../configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-remote-agent.md).
- Die Stagingumgebung ist so konfiguriert, dass unter Verwendung des Endpunkts Web bereitstellen Handler Remotebereitstellungen akzeptiert, wie in beschrieben [Szenario: Konfigurieren einer Staging-Umgebung für die Bereitstellung](../configuring-server-environments-for-web-deployment/scenario-configuring-a-staging-environment-for-web-deployment.md) und [einen Webserver konfigurieren für das Web Deploy-Veröffentlichung (Web Deploy-Handler)](../configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler.md).
- Die produktionsumgebung ist so konfiguriert, dass Administrator manuell Web-Bereitstellungspakete in Internet Information Services (IIS) importieren können, wie in beschrieben [Szenario: Konfigurieren einer produktiven Umgebung für die Bereitstellung](../configuring-server-environments-for-web-deployment/scenario-configuring-a-production-environment-for-web-deployment.md) und [konfigurieren Sie einen Webserver für Web Deploy-Veröffentlichung (Bereitstellung Offline)](../configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-offline-deployment.md).

## <a name="initial-development-and-deployment"></a>Anfängliche Entwicklung und Bereitstellung

Das Entwicklungsteam Fabrikam, Inc. die Projektmappe Contact Manager zum ersten Mal bereitstellen kann, muss es diese Aufgaben ausführen:

- Erstellen Sie ein neues Teamprojekt in TFS.
- Erstellen Sie die Microsoft Build Engine (MSBuild)-Projektdateien, die die Logik für die Bereitstellung enthalten.
- Erstellen Sie die TFS-Builddefinitionen, die die-bereitstellungstechnologien auslösen.

### <a name="create-a-new-team-project"></a>Erstellen eines neuen Teamprojekts

- Die TFS-Administrator Rob Walters, erstellt ein neues Teamprojekt für die Anwendung aus, wie in beschrieben [erstellen ein Teamprojekt in TFS](../configuring-team-foundation-server-for-web-deployment/creating-a-team-project-in-tfs.md). Als Nächstes erstellt der Entwicklungsleiter Matt Hink, eine Skelette-Lösung. Checkt seine Dateien in das neue Teamprojekt in TFS, wie in beschrieben [Inhalt hinzufügen, zur Quellcodeverwaltung](../configuring-team-foundation-server-for-web-deployment/adding-content-to-source-control.md).

### <a name="create-the-deployment-logic"></a>Erstellen Sie die Logik für die Bereitstellung

Matt Hink erstellt verschiedene benutzerdefinierte MSBuild-Projektdateien, mit dem Split Projekt Dateiansatz beschrieben [verstehen die Projektdatei](../web-deployment-in-the-enterprise/understanding-the-project-file.md). Matt erstellt:

- Eine Projektdatei mit dem Namen *Publish.proj* , die den Bereitstellungsvorgang ausführen. Diese Datei enthält die MSBuild-Ziele, die Projekte in der Projektmappe zu erstellen, Webpakete erstellen und Bereitstellen der Pakete in ein Ziel-Server-Umgebung.
- Umgebungsspezifische Projektdateien, die mit dem Namen *Env Dev.proj* und *Env Stage.proj*. Diese enthalten Einstellungen, die für die testumgebung und der Stagingumgebung spezifisch sind bzw. wie Verbindungszeichenfolgen, die Dienstendpunkte und die Details des Remotediensts an, die das Paket erhalten. Hilfestellung bei der Auswahl der richtigen Einstellungen für bestimmte zielumgebungen finden Sie unter [Konfigurieren von Bereitstellungseigenschaften für eine Zielumgebung](../configuring-server-environments-for-web-deployment/configuring-deployment-properties-for-a-target-environment.md).

Um die Bereitstellung ausgeführt wird, ein Benutzer führt die *Publish.proj* Datei mithilfe von MSBuild oder Team Build und gibt den Speicherort der Projektdatei von relevanten umgebungsspezifische (*Env Dev.proj* oder *Env Stage.proj*) als Befehlszeilenargument. Die *Publish.proj* Datei importiert Sie danach die umgebungsspezifische Projektdatei, um einen vollständigen Satz von Anweisungen für jedes zielumgebung veröffentlichen zu erstellen.

> [!NOTE]
> Die Funktionsweise dieser benutzerdefinierten Projektdateien ist unabhängig von der Mechanismus, mit dem MSBuild aufrufen. Beispielsweise können Sie die MSBuild-Befehlszeile direkt, wie in beschrieben [verstehen die Projektdatei](../web-deployment-in-the-enterprise/understanding-the-project-file.md). Sie können die Projektdateien über eine Befehlsdatei ausführen, wie in beschrieben [erstellen und Ausführen einer Befehlsdatei Bereitstellung](../web-deployment-in-the-enterprise/creating-and-running-a-deployment-command-file.md). Alternativ können Ausführen der Projektdateien aus einer Builddefinition in TFS, wie in beschrieben [Erstellen einer Builddefinition, unterstützt die Bereitstellung](../configuring-team-foundation-server-for-web-deployment/creating-a-build-definition-that-supports-deployment.md).  
> In jedem Fall ist das Endergebnis dasselbe & #x 2014; MSBuild führt die zusammengeführte Projektdatei und die Projektmappe in der zielumgebung bereitgestellt. Dies bietet Ihnen ein hohes Maß an Flexibilität in wie die publishing Prozess ausgelöst.


Sobald er die benutzerdefinierte Projektdateien erstellt, Matt Projektmappenordner hinzugefügt, und in die quellcodeverwaltung überprüft werden.

### <a name="create-build-definitions"></a>Builddefinitionen erstellen

Als eine Aufgabe finalen Vorbereitung zusammenarbeiten, Matt und Rob um drei Builddefinitionen für das neue Teamprojekt zu erstellen:

- **DeployToTest**. Die Projektmappe Contact Manager erstellt und jedes Mal, wenn eine Überprüfung erfolgt, in der testumgebung bereitgestellt.
- **DeployToStaging**. Dadurch wird die Ressourcen aus einem angegebenen vorherigen Build in der Stagingumgebung bereitgestellt, wenn ein Entwickler den Build Warteschlangen.
- **"DeployToStaging-WhatIf"**. Dies führt eine "Was-wäre-wenn" Bereitstellung in der Stagingumgebung, wenn ein Entwickler den Build Warteschlangen.

Die folgenden Abschnitte enthalten weitere Details zu jeder dieser Builddefinitionen.

## <a name="deployment-to-test"></a>Bereitstellung für Tests

Das Entwicklungsteam an Fabrikam, Inc. verwaltet testumgebungen, um eine Vielzahl von Aktivitäten, wie die Überprüfung und Validierung, Nutzbarkeitstests Kompatibilitätstests und ad-hoc- oder unsystematische Tests Testen von Software durchführen.

Das Entwicklungsteam hat eine Builddefinition in TFS mit dem Namen **DeployToTest**. Diese Builddefinition verwendet einen Auslöser für continuous Integration, was, dass der Buildprozess ausgeführt wird bedeutet, jedes Mal, wenn ein Mitglied des Entwicklungsteams Fabrikam, Inc. einen Eincheckvorgang ausführt. Wenn ein Build ausgelöst wird, wird die Builddefinition aus:

- Erstellen Sie die Projektmappe ContactManager.sln. Dies wird wiederum jedes Projekt innerhalb der Projektmappe erstellt.
- Führen Sie Komponententests in der Projektmappenordnerstruktur (wenn das Projekt erfolgreich erstellt wurde).
- Führen Sie die benutzerdefinierte Projektdateien, die den Bereitstellungsprozess (wenn die Projektmappe wurde erfolgreich erstellt und alle Komponententests übergibt) zu steuern.

Das Endergebnis ist, dass die Projektmappe wurde erfolgreich erstellt und übergibt Komponententests, die Webpakete und andere Bereitstellungsressourcen in der testumgebung bereitgestellt werden.

![](application-lifecycle-management-from-development-to-production/_static/image3.png)

### <a name="how-does-the-deployment-process-work"></a>Wie funktioniert der Bereitstellungsprozess?

Die **DeployToTest** Definition stellt diese Argumente in MSBuild zu erstellen:


[!code-console[Main](application-lifecycle-management-from-development-to-production/samples/sample1.cmd)]


Die **DeployOnBuild = "true"** und **DeployTarget = Paket** Eigenschaften werden verwendet, wenn das Team Build Projekte innerhalb der Projektmappe erstellt. Wenn das Projekt ein Webprojekt für die Anwendung ist, weisen Sie diese Eigenschaften MSBuild zum Erstellen eines Webbereitstellungspakets für das Projekt. Die **TargetEnvPropsFile** -Eigenschaft teilt die *Publish.proj* -Datei, in die zu importierende Projektdatei umgebungsspezifische gefunden.

> [!NOTE]
> Eine ausführliche exemplarische Vorgehensweise zum Erstellen einer Builddefinition wie folgt, finden Sie unter [Erstellen einer Builddefinition, unterstützt die Bereitstellung](../configuring-team-foundation-server-for-web-deployment/creating-a-build-definition-that-supports-deployment.md).


Die *Publish.proj* -Datei enthält die Ziele, die jedes Projekt in der Projektmappe zu erstellen. Es enthält jedoch auch bedingten Logik, überspringt diese Ziele zu erstellen, wenn Sie die Datei in Team Build ausführen. Dadurch können Sie zusätzliche Funktionen nutzen, die Team Build, sich z. B. die Fähigkeit zum Ausführen von Komponententests bietet. Wenn die Erstellung der Projektmappe oder die Einheit ausfallen, testet der *Publish.proj* Datei wird nicht ausgeführt werden und die Anwendung wird nicht bereitgestellt werden.

Die Bedingungslogik erfolgt durch das Auswerten der **BuildingInTeamBuild** Eigenschaft. Dies ist eine MSBuild-Eigenschaft, die automatisch, um festgelegt wird **"true"** Verwendung Team Build zum Erstellen von Projekten.

## <a name="deployment-to-staging"></a>Bereitstellung in die Stagingumgebung

Wenn ein Build alle Anforderungen für das Team Developer in der testumgebung entspricht, kann das Team denselben Build in einer Stagingumgebung bereitstellen möchten. Stagingumgebungen sind in der Regel entsprechend konfiguriert die Eigenschaften der Produktion oder "live" Umgebung als eng wie möglich, z. B. im Hinblick auf die Server-Spezifikationen, Betriebssysteme und Software und Netzwerkkonfiguration. Stagingumgebungen werden häufig für Auslastungstests, Benutzerakzeptanztests und umfassenderen interne Reviews verwendet. Builds werden direkt aus dem Build-Server in der Stagingumgebung bereitgestellt.

![](application-lifecycle-management-from-development-to-production/_static/image4.png)

Die Builddefinitionen, die zum Bereitstellen der Lösung in der Stagingumgebung **DeployToStaging-WhatIf** und **DeployToStaging**, weisen diese Merkmale auf:

- Diese erstellen nicht tatsächlich etwas. Beim Rob die Lösung in der Stagingumgebung bereitstellen, möchte er einen bestimmten, vorhandenen Build bereitstellen, der bereits überprüft und in der testumgebung überprüft wurde. Alle Builddefinitionen müssen nur die benutzerdefinierten Projektdateien ausgeführt, die den Bereitstellungsprozess zu steuern.
- Wenn Rob einen Build ausgelöst wird, verwendet er die Buildparameter, um anzugeben, welchem Build die Ressourcen enthält, die er aus dem Build-Server bereitstellen möchte.
- Alle Builddefinitionen werden nicht automatisch ausgelöst. Rob Warteschlangen manuell einen Build aus, wenn er die Lösung für die Stagingumgebung bereitstellen möchte.

Dies ist der grundsätzliche Erstellungsprozess für eine Bereitstellung in der Stagingumgebung:

1. Staging Umgebung Administrator Rob Walters, Warteschlangen, einem Build mit der **DeployToStaging-WhatIf** Definition zu erstellen. Rob verwendet die Build-Definition-Parameter, an welchem Build, möchte er bereitstellen.
2. Die **DeployToStaging-WhatIf** Definition ausgeführt wird die benutzerdefinierte Projektdateien im "Was-wäre-wenn"-Modus zu erstellen. Dadurch wird die Protokolldateien generiert, als ob Rob wurde eine aktive Bereitstellung ausführen, jedoch nicht tatsächlich alle Änderungen in der zielumgebung.
3. Rob prüft die Protokolldateien, um die Auswirkungen der Bereitstellung in der Stagingumgebung zu ermitteln. Rob möchte insbesondere, überprüfen Sie, was hinzugefügt werden, was aktualisiert wird und welche gelöscht werden.
4. Wenn Rob wird erfüllt, dass die Bereitstellung unerwünschten Änderungen an vorhandenen Ressourcen oder Daten vornehmen, wird nicht, er Warteschlangen ein Build mit der **DeployToStaging** Builddefinition.
5. Die **DeployToStaging** Definition ausgeführt wird die benutzerdefinierte Projektdateien zu erstellen. Die Ressourcen zur Bereitstellung veröffentlichen auf dem primären Webserver in der Stagingumgebung.
6. Der Web Farm Framework (WFF)-Controller wird die Webserver in der Stagingumgebung synchronisiert. Dadurch wird die Anwendung auf allen Web-Servern in der Farm verfügbar.

### <a name="how-does-the-deployment-process-work"></a>Wie funktioniert der Bereitstellungsprozess?

Die **DeployToStaging** Definition stellt diese Argumente in MSBuild zu erstellen:


[!code-console[Main](application-lifecycle-management-from-development-to-production/samples/sample2.cmd)]


Die **TargetEnvPropsFile** -Eigenschaft teilt die *Publish.proj* -Datei, in die zu importierende Projektdatei umgebungsspezifische gefunden. Die **OutputRoot** Eigenschaft überschreibt den integrierten Wert und gibt den Speicherort des Ordners "Build", die die Ressourcen enthält, bereitgestellt werden soll. Wenn Rob Build Warteschlangen, verwendet er die **Parameter** Tab, um einen aktualisierten Wert für die **OutputRoot** Eigenschaft.

![](application-lifecycle-management-from-development-to-production/_static/image5.png)

> [!NOTE]
> Weitere Informationen zum Erstellen einer Builddefinition wie folgt, finden Sie unter [Bereitstellen einer bestimmten Build](../configuring-team-foundation-server-for-web-deployment/deploying-a-specific-build.md).


Die **DeployToStaging-WhatIf** Builddefinition enthält die gleiche Bereitstellung Logik wie die **DeployToStaging** Definition zu erstellen. Es beinhaltet jedoch das zusätzliche Argument **"WhatIf" = "true"**:


[!code-console[Main](application-lifecycle-management-from-development-to-production/samples/sample3.cmd)]


Innerhalb der *Publish.proj* Datei, die **"WhatIf"** Eigenschaft gibt an, dass alle Ressourcen zur Bereitstellung "Was-wäre-wenn"-Modus veröffentlicht werden soll. Das heißt, werden Protokolldateien generiert, als wäre die Bereitstellung im Voraus nicht hatte, aber nichts tatsächlich in der zielumgebung geändert wird. Auf diese Weise können Sie die Auswirkungen eines vorgeschlagenen Bereitstellung & #x 2014 bewertet, insbesondere, was wird abrufen hinzugefügt haben, was aktualisiert wird, und welche gelöscht werden & #x 2014; bevor Sie Änderungen tatsächlich vornehmen.

> [!NOTE]
> Weitere Informationen zum Konfigurieren von "Was-wäre-wenn-Bereitstellungen finden Sie unter [Ausführen einer"Was-wäre-wenn"Bereitstellung](../advanced-enterprise-web-deployment/performing-a-what-if-deployment.md).


Nachdem Sie Ihre Anwendung mit dem primären Webserver in der Stagingumgebung bereitgestellt haben, wird die WFF automatisch die Anwendung auf allen Servern in der Serverfarm synchronisiert werden.

> [!NOTE]
> Weitere Informationen zum Konfigurieren der WFF zum Synchronisieren von Webservern finden Sie unter [erstellen Sie eine Serverfarm mit Web Farm Framework](../configuring-server-environments-for-web-deployment/creating-a-server-farm-with-the-web-farm-framework.md).


## <a name="deployment-to-production"></a>Bereitstellung bis hin zur Produktion

Wenn ein Build in der Stagingumgebung genehmigt wurde, kann das Fabrikam, Inc.-Team die Anwendung in der produktionsumgebung veröffentlichen. Die produktionsumgebung ist, in dem die Anwendung "live" geht, und die Zielgruppe der Endbenutzer erreicht.

Die produktionsumgebung wird in einem Umkreisnetzwerk Internetzugriff. Dies ist aus dem internen Netzwerk, das den Build-Server enthält isoliert. Der Produktion Umgebung Administrator Lisa Andrews, muss manuell die Web-Bereitstellungspakete aus dem Build-Server kopieren und in IIS auf dem primären Produktions-Web-Server importieren.

![](application-lifecycle-management-from-development-to-production/_static/image6.png)

Dies ist der grundsätzliche Erstellungsprozess für eine Bereitstellung in der produktionsumgebung:

1. Das Team Developer Member-Claudia, ein Build für die Bereitstellung bis hin zur Produktion bereit ist. Das Team Member-Claudia Speicherort in der Web-Pakete innerhalb des Drop-Ordners auf dem Buildserver.
2. Claudia die Webpakete auf dem Buildserver erfasst und kopiert diese auf dem primären Webserver in der produktionsumgebung.
3. Claudia wird zum Importieren und veröffentlichen die Webpakete auf dem primären Webserver IIS-Manager verwendet.
4. Der Controller WFF synchronisiert Webserver in der produktionsumgebung. Dadurch wird die Anwendung auf allen Web-Servern in der Farm verfügbar.

### <a name="how-does-the-deployment-process-work"></a>Wie funktioniert der Bereitstellungsprozess?

IIS-Manager enthält einen Import Anwendungs-Paket-Assistenten, der zum Veröffentlichen von Webpakete in einer IIS-Website ermöglicht. Eine exemplarische Vorgehensweise, um dieses Verfahren ausführen, finden Sie unter [Webpaketen manuell installieren](../web-deployment-in-the-enterprise/manually-installing-web-packages.md).

## <a name="conclusion"></a>Schlussfolgerung

In diesem Thema bereitgestellten eine Abbildung des Bereitstellungslebenszyklus für eine typische Enterprise-Skalierung-Webanwendung.

Dieses Thema ist Teil einer Reihe von Lernprogrammen, die Anleitungen auf verschiedene Aspekte der Bereitstellung von Webanwendungen bereit. Klicken Sie in der Praxis stehen viele weitere Tasks und Überlegungen in den einzelnen Phasen des Bereitstellungsprozesses, und es ist nicht möglich, diese in eine einzelne Exemplarische Vorgehensweise abzudecken. Weitere Informationen finden Sie in diesen Lernprogrammen:

- [Die webbereitstellung im Unternehmen](../web-deployment-in-the-enterprise/web-deployment-in-the-enterprise.md). Dieses Lernprogramm enthält eine umfassende Einführung in Web-Bereitstellungsverfahren mithilfe von MSBuild und der IIS-Webbereitstellungstool (Web Deploy).
- [Konfigurieren von Serverumgebungen für die Bereitstellung](../configuring-server-environments-for-web-deployment/configuring-server-environments-for-web-deployment.md). Dieses Lernprogramm bietet Hinweise zum Konfigurieren von Windows Server-Umgebungen, um verschiedene Bereitstellungsszenarien zu unterstützen.
- [Konfigurieren von Team Foundation Server für automatisierte Webbereitstellung](../configuring-team-foundation-server-for-web-deployment/configuring-team-foundation-server-for-web-deployment.md). Dieses Lernprogramm bietet Hinweise zum Bereitstellung Logik in TFS Buildprozesse zu integrieren.
- [Erweiterte Web Unternehmensbereitstellung](../advanced-enterprise-web-deployment/advanced-enterprise-web-deployment.md). Dieses Lernprogramm bietet Hinweise zum erfüllen einige der komplexeren Herausforderungen der Bereitstellung dieser Organisationen Gesicht.

>[!div class="step-by-step"]
[Zurück](enterprise-web-deployment-scenario-overview.md)
