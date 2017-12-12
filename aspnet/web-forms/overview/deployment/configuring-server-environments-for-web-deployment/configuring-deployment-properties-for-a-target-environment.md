---
uid: web-forms/overview/deployment/configuring-server-environments-for-web-deployment/configuring-deployment-properties-for-a-target-environment
title: "Konfigurieren Bereitstellungseigenschaften für eine Zielumgebung | Microsoft Docs"
author: jrjlee
description: Dieses Thema beschreibt die Umgebung-spezifische Eigenschaften zu konfigurieren, um die Projektmappe Contact Manager in einer bestimmten zielumgebung bereitstellen...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: b5b86e03-b8ed-46e6-90fa-e1da88ef34e9
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/configuring-server-environments-for-web-deployment/configuring-deployment-properties-for-a-target-environment
msc.type: authoredcontent
ms.openlocfilehash: f27b8376b332ff21185be0fd5c00ced7d40a20bd
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="configuring-deployment-properties-for-a-target-environment"></a>Konfigurieren Bereitstellungseigenschaften für eine Zielumgebung
====================
durch [Jason Lee](https://github.com/jrjlee)

[PDF herunterladen](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> In diesem Thema wird beschrieben, wie Umgebung-spezifische Eigenschaften zu konfigurieren, um die Projektmappe Contact Manager in einer bestimmten zielumgebung bereitgestellt wird.


Dieses Thema ist Teil einer Reihe von Lernprogrammen, die auf der Basis der Enterprise-bereitstellungsanforderungen eines fiktiven Unternehmens mit dem Namen Fabrikam, Inc. Diese Reihe von Lernprogrammen verwendet eine Beispielprojektmappe & #x 2014; die [Vorgesetzten Kontakts](../web-deployment-in-the-enterprise/the-contact-manager-solution.md) Lösung & #x 2014; zum Darstellen einer Webanwendung mit einer realistischen Maß an Komplexität, einschließlich einer ASP.NET MVC 3-Anwendung, eine Windows Communication Foundation (WCF)-Dienst, und ein Datenbankprojekt.

Die Bereitstellungsmethode das Herzstück mit diesen Lernprogrammen basiert auf der Teilung Datei Herangehensweise beschrieben [Verständnis des Build-Prozesses](../web-deployment-in-the-enterprise/understanding-the-build-process.md), in dem durch der Buildprozess gesteuert wird Projekt zwei Dateien & #x 2014; enthält Erstellen Sie für jede zielumgebung und enthält umgebungsspezifische Einstellungen für Build- und Bereitstellungsprozess geltenden Anweisungen, an. Zur Buildzeit ist die Unabhängigkeit von der Umgebung-Projektdatei, einen vollständigen Satz von Buildanweisungen bilden die Projektdatei umgebungsspezifische zusammengeführt.

## <a name="process-overview"></a>Übersicht über das

Die Projektdatei, die Sie verwenden möchten, erstellen und Bereitstellen der Projektmappe Contact Manager ist auf zwei physischen Dateien aufteilen:

- Universal enthält die Einstellungen und Anweisungen erstellen (die *Publish.proj* Datei).
- Enthält umgebungsspezifische Einstellungen erstellen (*Env Dev.proj*, *Env Stage.proj*usw.).

Beim Erstellen die entsprechenden umgebungsspezifische-Projektdatei der Universal zusammengeführt *Publish.proj* Datei, um einen vollständigen Satz von Buildanweisungen zu bilden. Sie können die Bereitstellung auf bestimmte zielumgebungen konfigurieren, durch Erstellen oder Anpassen der umgebungsspezifischen Projektdateien mit Einstellungen, die Ihre eigenen Bereitstellungsszenario beschreiben.

Große Anzahl dieser Werte werden durch die Konfiguration der zielumgebung & #x 2014 bestimmt; insbesondere, ob die Ziel-Webserver ist so konfiguriert, der Webbereitstellungs-Agent-Dienst (der remote-Agent) oder der Handler für die Web-Bereitstellung verwenden. Weitere Informationen zu diesen Vorgehensweisen sowie Anleitungen zum Auswählen des richtigen Ansatzes für Ihre Umgebung finden Sie unter [Auswählen der rechts Ansatz für die Webbereitstellung](choosing-the-right-approach-to-web-deployment.md).

Die [Kontakt-Manager-Szenario](../deploying-web-applications-in-enterprise-scenarios/enterprise-web-deployment-scenario-overview.md) erfordert zwei umgebungsspezifische Projektdateien:

- Bereitstellung in einer testumgebung für Entwickler (*Env Dev.proj*). Die Umgebung für Entwickler ist so konfiguriert, dass Remotebereitstellungen mithilfe des remote-Agents akzeptiert, wie in beschrieben [Szenario: Konfigurieren einer Umgebung testen für Webbereitstellung](scenario-configuring-a-test-environment-for-web-deployment.md). Diese Datei muss den RemoteAgent Endpunktadresse sowie standortspezifische Einstellungen wie Verbindungszeichenfolgen und Dienstendpunkte enthalten.
- Bereitstellung in einer Stagingumgebung (*Env Stage.proj*). Die Stagingumgebung ist so konfiguriert, dass mit dem Web bereitstellen Handler Remotebereitstellungen akzeptiert, wie in beschrieben [Szenario: Konfigurieren einer Staging-Umgebung für die Bereitstellung](scenario-configuring-a-staging-environment-for-web-deployment.md). Diese Datei muss die Endpunktadresse Web bereitstellen Handler sowie standortspezifische Einstellungen wie Verbindungszeichenfolgen und Dienstendpunkte.

Es ist wichtig um zu beachten, dass die Einstellungen, die Sie in der Projektdatei umgebungsspezifische konfigurieren, die keine Auswirkung auf den Inhalt der Web-Paket selbst & #x 2014; stattdessen sie steuern, wie das Paket bereitgestellt wird und welche Parameterwerte bei bereitgestellt werden die Paket extrahiert wird. Sie importieren die Web-Paket in die produktionsumgebung, wie in beschrieben, manuell [Szenario: Konfigurieren einer produktiven Umgebung für die Bereitstellung von](scenario-configuring-a-production-environment-for-web-deployment.md) und [Webpaketen manuell installieren](../web-deployment-in-the-enterprise/manually-installing-web-packages.md), so genau, welche Einstellungen keine Rolle spielt verwendet Sie in der Projektdatei umgebungsspezifische, wenn Sie das Paket generiert. Internetinformationsdienste (Internet Information Services, IIS) Manager werden Sie aufgefordert für eine beliebige parametrisierte Werte, wie z. B. Verbindungszeichenfolgen und Dienstendpunkte, wenn Sie das Paket importieren.

Um die Kontakt-Manager-Lösung für Ihre eigenen zielumgebung bereitzustellen, können Sie diese Datei anpassen oder Sie als Vorlage zu verwenden und erstellen eine eigene Datei.

**Umgebungsspezifische bereitstellungseinstellungen für die Projektmappe Contact Manager konfigurieren**

1. Öffnen Sie die ContactManager-WCF-Projektmappe in Visual Studio 2010.
2. In der **Projektmappen-Explorer** Fenster, erweitern Sie die **veröffentlichen** Ordner, erweitern Sie die **EnvConfig** Ordner, und doppelklicken Sie dann auf **Env-Dev.proj**.

    ![](configuring-deployment-properties-for-a-target-environment/_static/image1.png)
3. Ersetzen Sie die Eigenschaftswerte in die *Env Dev.proj* Datei mit den richtigen Werten für Ihre eigenen testumgebung.

    > [!NOTE]
    > Die Tabelle, die von dieser Prozedur folgt enthält weitere Informationen für jede dieser Eigenschaften.
4. Speichern Sie Ihre Arbeit, und schließen Sie dann die *Env Dev.proj* Datei.

## <a name="choosing-the-right-deployment-properties"></a>Auswählen der richtigen Bereitstellungseigenschaften

Diese Tabelle beschreibt den Zweck der einzelnen Eigenschaften in der Projektdatei von Beispiel umgebungsspezifische *Env Dev.proj*, und enthält hilfreiche Informationen für die Werte sollten Sie bereitstellen.

| Eigenschaftenname | Details |
| --- | --- |
| **MSDeployComputerName** den Namen des Web-Server oder-Dienst Zielendpunkts. | Wenn Sie an den remote-Agent-Dienst auf dem Ziel-Webserver bereitstellen, können Sie den Namen des Zielcomputers angeben (z. B. **TESTWEB1** oder **TESTWEB1.fabrikam.net**), oder Sie können die Remote angeben Agent-Endpunkt (z. B. `http://TESTWEB1/MSDEPLOYAGENTSERVICE`). Die Bereitstellung funktioniert die gleiche Weise wie in jedem Fall. Wenn Sie zum Bereitstellen von-Ereignishandler auf dem Ziel-Webserver bereitstellen, sollten Sie geben Sie den Dienstendpunkt und schließen Sie den Namen der IIS-Website als ein Abfragezeichenfolgen-Parameter (z. B. `https://STAGEWEB1:8172/MSDeploy.axd?site=DemoSite`). |
| **MSDeployAuth** die Methode für die Web Deploy verwenden soll, um mit dem Remotecomputer zu authentifizieren. | Dies sollte festgelegt werden, um **NTLM** oder **grundlegende**. Verwenden Sie in der Regel **NTLM** , wenn Sie mit dem remote-Agent-Dienst bereitstellen und **grundlegende** , wenn Sie an den Handler für die Web-Bereitstellung bereitstellen. Wenn Sie Standardauthentifizierung verwenden, müssen Sie auch angeben, den Benutzernamen und das Kennwort, das IIS-Webbereitstellungstool (Web Deploy) für den Identitätswechsel sollte zum Ausführen der Bereitstellung. In diesem Beispiel werden diese Werte bereitgestellt, über die **MSDeployUsername** und **MSDeployPassword** Eigenschaften. Wenn Sie die NTLM-Authentifizierung verwenden, können Sie diese Eigenschaften weggelassen, da leer lassen. |
| **MSDeployUsername** Wenn Sie Standardauthentifizierung verwenden, Web Deploy verwendet dieses Konto auf dem Remotecomputer. | Dies sollte Folgendes Format *Domäne*\*Benutzername * (z. B. **FABRIKAM\matt**). Dieser Wert wird nur verwendet, wenn Sie die Standardauthentifizierung angeben. Wenn Sie die NTLM-Authentifizierung verwenden, kann die Eigenschaft ausgelassen werden. Wenn kein Wert bereitgestellt wird, wird es ignoriert. |
| **MSDeployPassword** Wenn Sie Standardauthentifizierung verwenden, Web Deploy verwendet dieses Kennwort auf dem Remotecomputer. | Dies ist das Kennwort für das Benutzerkonto, das Sie, in angegeben der **MSDeployUsername** Eigenschaft. Dieser Wert wird nur verwendet, wenn Sie die Standardauthentifizierung angeben. Wenn Sie die NTLM-Authentifizierung verwenden, kann die Eigenschaft ausgelassen werden. Wenn kein Wert bereitgestellt wird, wird es ignoriert. |
| **ContactManagerIisPath** die IIS-Pfad auf dem die Kontakt-Manager-MVC-Anwendung bereitgestellt werden soll. | Dies sollte der Pfad sein, wie er im IIS-Manager im Formular angezeigt wird [*Name des IIS-Website*] / [*Web**Anwendungsname*]. Denken Sie daran, dass die IIS-Website muss vorhanden sein, bevor Sie die Anwendung bereitstellen. Wenn Sie eine IIS-Website mit dem Namen DemoSite erstellt haben, können Sie z. B. den IIS-Pfad für die MVC-Anwendung als DemoSite/ContactManager angeben. |
| **ContactManagerServiceIisPath** die IIS-Pfad auf dem den Kontakt-Manager-WCF-Dienst bereitgestellt werden soll. | Z. B. Wenn Sie eine IIS-Website mit dem Namen DemoSite erstellt haben, geben Sie den IIS-Pfad für den WCF-Dienst als **DemoSite/ContactManagerService**. |
| **ContactManagerTargetUrl** die URL, an dem der WCF-Dienst erreicht werden kann. | Dieser Vorgang dauert das Formular [*Stamm-URL des IIS-Website*] / [*dienstanwendungsname*] / [*Dienstendpunkt*]. Z. B. Wenn Sie eine IIS-Website auf Port 85 erstellt haben, die URL wäre das Formular `http://localhost:85/ContactManagerService/ContactService.svc`. Denken Sie daran, dass die MVC-Anwendung und den WCF-Dienst auf dem gleichen Server bereitgestellt werden. Diese URL wird daher immer nur auf dem Computer zugegriffen auf dem er installiert ist. Aus diesem Grund ist es besser, "localhost" oder die IP-Adresse, statt den Computernamen oder einen Hostheader in der URL verwendet. Wenn Sie den Computernamen oder einen Hostheader verwenden die [Loopback Kontrollkästchen](https://go.microsoft.com/?linkid=9805131) Sicherheitsfunktion in IIS möglicherweise blockiert die URL und Rückgabewerte ein **HTTP 401.1 - nicht autorisiert** Fehler. |
| **CmDatabaseConnectionString** die Verbindungszeichenfolge für den Datenbankserver. | Die Verbindungszeichenfolge bestimmt sowohl die Anmeldeinformationen, die VSDBCMD verwendet wird, wenden Sie sich an den Datenbankserver und die Datenbank und die Anmeldeinformationen für der Webanwendungspool für Server, wenden Sie sich an den Datenbankserver und interagieren mit der Datenbank erstellen. Im Wesentlichen haben Sie zwei Optionen hier. Können Sie angeben, **integrierte Sicherheit = "true"**, in diesem Fall die integrierte Windows-Authentifizierung verwendet wird: **Datenquelle = TESTDB1; Integrated Security = "true"** In diesem Fall die Datenbank mithilfe erstellt die Anmeldeinformationen des Benutzers, der die ausführbare VSDBCMD ausgeführt, und die Anwendung werden die Datenbank mit der Identität des Computerkontos der Web-Server zugreifen. Alternativ können Sie den Benutzernamen und das Kennwort eines Kontos für die SQL Server angeben. In diesem Fall werden die SQL Server-Anmeldeinformationen verwendet, VSDBCMD zum Erstellen der Datenbank und den Anwendungspool für die Interaktion mit der Datenbank: **Data Source = TESTDB1; Benutzer-Id = ASqlUser; Kennwort = Pa$ $w0rd** exemplarischen Vorgehensweisen in diesem Thema wird davon ausgegangen, dass Sie integrierte Windows-Authentifizierung verwenden. |
| **CmTargetDatabase** den Namen die Datenbank zu gewähren, Sie auf dem Datenbankserver erstellen, werden sollen. | Der von Ihnen hier bereitgestellten Wert wird als Parameter an den Befehl VSDBCMD hinzugefügt. Es wird auch verwendet, um eine vollständige Verbindungszeichenfolge zu erstellen, die der Anwendungspool auf dem Webserver verwenden kann, um die Interaktion mit der Datenbank. |
  

Diesen Beispielen wird gezeigt, wie Sie diese Eigenschaften für bestimmte Szenarien konfigurieren können.

### <a name="example-1x2014deployment-to-the-remote-agent-service"></a>Beispiel 1 & #x 2014; Bereitstellung mit dem Remote-Agent-Dienst

In diesem Beispiel:

- Sie sind mit dem remote-Agent-Dienst auf TESTWEB1 bereitstellen.
- Sie können Web Deploy, um die NTLM-Authentifizierung verwendet festzuhalten. Web Deploy wird ausgeführt, mit den Anmeldeinformationen, die Sie zum Aufrufen von Microsoft Build Engine (MSBuild) verwendet.
- Sie können mithilfe der integrierten Authentifizierung Bereitstellen der **ContactManager** TESTDB1 Datenbank. Die Datenbank wird mit den Anmeldeinformationen, die Sie zum Aufrufen von MSBuild verwendet bereitgestellt werden.


[!code-xml[Main](configuring-deployment-properties-for-a-target-environment/samples/sample1.xml)]


### <a name="example-2x2014deployment-to-the-web-deploy-handler-endpoint"></a>Beispiel 2 & #x 2014; Bereitstellung im Web Handler Endpunkt bereitstellen

In diesem Beispiel:

- Sie können zum Bereitstellen von Web-Handler Dienstendpunkt auf STAGEWEB1 bereitstellen.
- Sie können Web Deploy, um die Standardauthentifizierung verwenden festzuhalten.
- Geben Sie, dass Web Deploy auf dem Remotecomputer das FABRIKAM\stagingdeployer-Konto den Identitätswechsel verwenden soll.
- Verwenden Sie SQL Server-Authentifizierung zum Bereitstellen der **ContactManager** STAGEDB1 Datenbank.


[!code-xml[Main](configuring-deployment-properties-for-a-target-environment/samples/sample2.xml)]


## <a name="conclusion"></a>Schlussfolgerung

An diesem Punkt werden die Projektdateien zum Erstellen und Bereitstellen der Kontakt-Manager-Lösungen für eine oder mehrere zielumgebungen vollständig konfiguriert.

Um diese Projektdateien als Teil eines Bereitstellungsprozesses einstufiger, wiederholbare verwenden zu können, müssen Sie zum Ausführen der *Publish.proj* mithilfe von MSBuild-Datei und den Speicherort der Projektdatei umgebungsspezifische als Parameter übergeben. Dies ist auf verschiedene Weise möglich:

- Eine Übersicht über MSBuild und eine Einführung in benutzerdefinierte Projektdateien, finden Sie unter [verstehen die Projektdatei](../web-deployment-in-the-enterprise/understanding-the-project-file.md).
- Informationen zum Formulieren von MSBuild-Befehl, die die benutzerdefinierte Projektdateien ausgeführt wird, finden Sie unter [Bereitstellen von Webpaketen](../web-deployment-in-the-enterprise/deploying-web-packages.md).
- Informationen wie die MSBuild-Befehle in einer Befehlsdatei für einstufiger, wiederholbare Bereitstellungen integriert, finden Sie unter [erstellen und Ausführen einer Befehlsdatei Bereitstellung](../web-deployment-in-the-enterprise/creating-and-running-a-deployment-command-file.md).
- Informationen, die benutzerdefinierte Projektdateien aus Team Build auszuführen, finden Sie unter [Erstellen einer Builddefinition, unterstützt die Bereitstellung](../configuring-team-foundation-server-for-web-deployment/creating-a-build-definition-that-supports-deployment.md).

>[!div class="step-by-step"]
[Zurück](creating-a-server-farm-with-the-web-farm-framework.md)
