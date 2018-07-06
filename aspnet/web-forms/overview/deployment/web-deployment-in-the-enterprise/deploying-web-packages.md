---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/deploying-web-packages
title: Bereitstellen von Webpaketen | Microsoft-Dokumentation
author: jrjlee
description: In diesem Thema wird beschrieben, wie Sie Web-Bereitstellungspakete mit einem Remoteserver veröffentlichen können, mit dem Internet Information Services (IIS)-Webbereitstellungstool (Web...
ms.author: aspnetcontent
ms.date: 05/04/2012
ms.assetid: a5c5eed2-8683-40a5-a2e1-35c9f8d17c29
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/deploying-web-packages
msc.type: authoredcontent
ms.openlocfilehash: 44c3772263cbebaf03e1393542fda32467a97cd8
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2018
ms.locfileid: "37825264"
---
<a name="deploying-web-packages"></a>Bereitstellen von Webpaketen
====================
durch [Jason Lee](https://github.com/jrjlee)

[PDF herunterladen](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> In diesem Thema wird beschrieben, wie Sie Web-Bereitstellungspakete mit einem Remoteserver veröffentlichen können, mithilfe der Internetinformationsdienste (Internet Information Services, IIS)-Webbereitstellungstool (Web Deploy) 2.0.
> 
> Es gibt zwei Hauptmethoden, die in denen Sie eine Web-Paket mit einem Remoteserver bereitstellen können:
> 
> - Sie können das MSDeploy.exe-Befehlszeile-Hilfsprogramm direkt verwenden.
> - Sie können ausführen, die *[Projektname].deploy.cmd* -Datei, die während des Buildprozesses werden.
> 
> Das Endergebnis ist identisch unabhängig davon, welchen Ansatz Sie. Im Wesentlichen alle die *. "Deploy.cmd"* Datei ist die Ausführung von MSDeploy.exe mit einigen vordefinierten Werten, sodass Sie nicht so viele Informationen bereitstellen, um das Paket bereitstellen. Dies vereinfacht den Bereitstellungsprozess. Auf der anderen Seite MSDeploy.exe direkt mit viel mehr Flexibilität über genau wie das Paket bereitgestellt wird.
> 
> Welchen Ansatz Sie verwenden, hängt von verschiedenen Faktoren ab, einschließlich wie viel Kontrolle über den Bereitstellungsprozess sind erforderlich und gibt an, ob Sie mit der Web Deploy Remote-Agents-Dienst oder der Handler für die Web-Bereitstellung abzielen. In diesem Thema wird erläutert, wie jeder Ansatz verwenden, und gibt an, wann jeder Ansatz geeignet ist.
> 
> Die Aufgaben und exemplarische Vorgehensweisen in diesem Thema wird angenommen, dass:
> 
> - Haben Sie erstellt und verpackt Sie Ihre Webanwendung zu, wie in beschrieben [erstellen und Verpacken von Webanwendungsprojekten](building-and-packaging-web-application-projects.md).
> - Verändert die *"SetParameters.xml"* Datei, um die richtigen Werte für Ihre zielumgebung, bereitzustellen, wie in beschrieben [Konfigurieren von Parametern für die Bereitstellung von Paket](configuring-parameters-for-web-package-deployment.md).


Ausführen der [*Projektname*]*. "Deploy.cmd"* Datei ist die einfachste Möglichkeit, ein Webpaket bereitgestellt werden. Insbesondere das Verwenden der *. "Deploy.cmd"* Datei bietet die folgenden Vorteile gegenüber der Verwendung von MSDeploy.exe direkt:

- Sie müssen nicht angeben des Speicherorts der Webbereitstellungspaket&#x2014;der *. "Deploy.cmd"* Datei bereits weiß, wo es ist.
- Sie müssen nicht den Speicherort der an die *"SetParameters.xml"* Datei&#x2014;der *. "Deploy.cmd"* Datei bereits weiß, wo es ist.
- Sie müssen nicht MS Deploy-Anbieter für Quelle und Ziel angeben&#x2014;der *. "Deploy.cmd"* Datei bereits weiß, welche Werte verwendet.
- Müssen Sie keine Einstellungen für den MSDeploy angeben&#x2014;der *. "Deploy.cmd"* Datei fügt die häufigsten erforderlichen Werte automatisch an den MSDeploy.exe-Befehl.

Vor der Verwendung der *. "Deploy.cmd"* Datei zum Bereitstellen eines Webpakets, sollten Sie sicherstellen, dass:

- Die *. "Deploy.cmd"* -Datei, die [*Projektname*]. *"SetParameters.xml"* -Datei und das Webpaket ([*Projektname*]. *ZIP*) befinden sich im gleichen Ordner.
- Web Deploy (MSDeploy.exe) installiert ist, auf dem Computer mit der *. "Deploy.cmd"* Datei.

Die *. "Deploy.cmd"* Datei unterstützt verschiedene Befehlszeilenoptionen. Wenn Sie die Datei an einer Eingabeaufforderung ausführen, ist dies die grundlegende Syntax:


[!code-console[Main](deploying-web-packages/samples/sample1.cmd)]


Geben Sie einen **/t /** Flag oder **/y** Flag, um anzugeben, ob Sie eine Testversion ausführen oder einer aktiven Bereitstellung bzw. ausführen möchten (verwenden Sie nicht beide Flags im selben Befehl). Diese Tabelle erläutert den Zweck der einzelnen dieser Flags.

| Flag | Beschreibung |
| --- | --- |
| **/T** | Ruft die MSDeploy.exe mit der **– Whatif** Flag, das ein Testlaufs angibt. Anstatt die Bereitstellung des Pakets an, erstellt ein Bericht von Was geschieht, wenn Sie das Paket bereitgestellt wurde. |
| **/ Y** | Ruft die MSDeploy.exe ohne die **– Whatif** Flag. Dadurch wird das Paket auf dem lokalen Computer oder der angegebene Zielserver bereitgestellt. |
| **/M** | Gibt den Zielserver name oder die Dienst-URL. Weitere Informationen zu den Werten, die Sie hier angeben können, finden Sie unter den **Endpunkt Überlegungen** in diesem Thema. Wenn Sie weglassen der **/m** Flag, das Paket wird auf dem lokalen Computer bereitgestellt werden. |
| **/ A** | Gibt den Authentifizierungstyp an, dem MSDeploy.exe zur Durchführung der Bereitstellung verwenden soll. Mögliche Werte sind **NTLM** und **grundlegende**. Wenn Sie weglassen der **/a** Flag der Authentifizierungstyp standardmäßig **NTLM** für die Bereitstellung auf den Dienst für die Web Deploy Remote-Agents und zu **grundlegende** für die Bereitstellung in der Web Deploy -Handler. |
| **/U** | Gibt den Benutzernamen an. Dies gilt nur, wenn Sie die Standardauthentifizierung verwenden. |
| **/P** | Gibt das Kennwort an. Dies gilt nur, wenn Sie die Standardauthentifizierung verwenden. |
| **/L** | Gibt an, dass das Paket an die lokale IIS Express-Instanz bereitgestellt werden soll. |
| **/G** | Gibt an, dass das Paket bereitgestellt wird, mit der [TempAgent anbietereinstellung](https://technet.microsoft.com/library/ee517345(WS.10).aspx). Wenn Sie weglassen der **/g** Flag, das als Standardwert **"false"**. |

> [!NOTE]
> Jedes Mal, wenn der Buildprozess ein Webpaket erstellt wird, erstellt er auch eine Datei namens *[Projektname] ".deploy"-"Readme.txt"* , die diese Bereitstellungsoptionen erläutert.


Sie können Einstellungen für den Web Deploy zusätzlich zu diesen Flags die, angeben, als zusätzliche *. "Deploy.cmd"* Parameter. Alle zusätzlichen Einstellungen, die Sie angeben, werden einfach an den zugrunde liegenden MSDeploy.exe-Befehl übergeben. Weitere Informationen zu diesen Einstellungen finden Sie unter [Web Deploy-Vorgang Settings](https://technet.microsoft.com/library/dd569089(WS.10).aspx).

Angenommen, Sie möchten das ContactManager.Mvc Webanwendungsprojekt in einer testumgebung bereitstellen, mit der *. "Deploy.cmd"* Datei. Die testumgebung zur Verwendung des Diensts Web Deploy Remote-Agents konfiguriert ist, wie in beschrieben [Konfigurieren eines Webservers für Bereitstellen von Webpublishing (Remote-Agent)](../configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-remote-agent.md). Um die Webanwendung bereitzustellen, müssen Sie die nächsten Schritte ausführen.

**Bereitstellen einer Webanwendung mit den. Datei "deploy.cmd"**

1. Erstellen und Packen Sie das Webanwendungsprojekt, siehe [erstellen und Verpacken von Webanwendungsprojekten](building-and-packaging-web-application-projects.md).
2. Ändern der *ContactManager.Mvc.SetParameters.xml* -Datei die richtigen Parameterwerte für Ihre testumgebung, wie in beschrieben [Konfigurieren von Parametern für die Bereitstellung von Paket](configuring-parameters-for-web-package-deployment.md).
3. Öffnen Sie ein Eingabeaufforderungsfenster, und navigieren Sie zu den Speicherort der *ContactManager.Mvc.deploy.cmd* Datei.
4. Geben Sie folgenden Befehl aus, und drücken Sie dann die EINGABETASTE:

    [!code-console[Main](deploying-web-packages/samples/sample2.cmd)]

In diesem Beispiel:

- Die **/y** Flag gibt an, dass Sie das Paket tatsächlich bereitstellen möchten, anstatt die Ausführen einer Testversion ausführen.
- Die **/m** Flag gibt an, dass Sie das Paket auf dem Server, die mit dem Namen TESTWEB1 bereitstellen möchten. Dieser Wert MSDeploy.exe versucht, das Paket an den Dienst für Web Deploy Remote-Agents unter Bereitstellung http://TESTWEB1/MSDeployAgentService.
- Die **/a** Flag gibt an, dass Sie die NTLM-Authentifizierung verwenden möchten. Daher müssen Sie einen Benutzernamen und ein Kennwort angeben.

Zu veranschaulichen, wie sich durch Verwendung der *. "Deploy.cmd"* Datei vereinfacht den Bereitstellungsprozess, sehen Sie sich den MSDeploy.exe-Befehl, die generiert und ausgeführt, wenn Sie ausführen, ruft *ContactManager.Mvc.deploy.cmd* verwenden die obigen Optionen.


[!code-console[Main](deploying-web-packages/samples/sample3.cmd)]


Weitere Informationen zur Verwendung der *. "Deploy.cmd"* Datei zum Bereitstellen einer Webpakets finden Sie unter [wie: Installieren eines Bereitstellung Pakets mithilfe der Datei "Deploy.cmd"](https://msdn.microsoft.com/library/ff356104.aspx).

## <a name="using-msdeployexe"></a>Verwenden von MSDeploy.exe

Obwohl durch die Verwendung der *. "Deploy.cmd"* Datei in der Regel vereinfacht den Bereitstellungsprozess, es gibt einige Situationen, wenn sie lieber MSDeploy.exe direkt zu verwenden ist. Zum Beispiel:

- Wenn Sie auf den Web bereitstellen Handler als Benutzer ohne Administratorrechte bereitstellen möchten, können keine der *. "Deploy.cmd"* Datei. Dies ist aufgrund eines Fehlers in Web Deploy 2.0, wie unter beschrieben **Endpunkt Überlegungen**.
- Wenn Sie manuell zwischen verschiedenen wechseln möchten *"SetParameters.xml"* Dateien an unterschiedlichen Standorten befinden, Sie können MSDeploy.exe direkt verwenden möchten.
- Wenn Sie mehrere MSDeploy.exe-Befehlszeilenargumenten außer Kraft setzen möchten, empfiehlt es sich, MSDeploy.exe direkt zu verwenden.

Wenn Sie auf "MSDeploy.exe" verwenden, müssen Sie drei wichtige Arten von Informationen angeben:

- Ein **– Quelle** Parameter, der angibt, woher die Daten stammen.
- Ein **– Dest** Parameter, der angibt, in dem Ihre Daten zu übertragen wird.
- Ein **– Verb** Parameter, der angibt der [Vorgang](https://technet.microsoft.com/library/dd568989(WS.10).aspx) Sie ausführen möchten.

Verwendet MSDeploy.exe [Web Deploy-Anbieter](https://technet.microsoft.com/library/dd569040(WS.10).aspx) Quell- und Zielserver um Daten zu verarbeiten. Web Deploy umfasst zahlreiche von Anbietern, die die Auswahl von Anwendungen und Daten darstellen, es funktioniert mit&#x2014;z. B. Anbieter für SQL Server-Datenbanken "," IIS-Webserver "," Zertifikate "," global Assembly Cache (GAC)-Assemblys, es gibt verschiedene andere Konfigurationsdateien, und viele andere Arten von Daten. Sowohl die **– Quelle** Parameter und die **– Dest** Parameter muss einen Anbieter angeben, in der Form **– Quelle**: [*ProviderName*] = [*Speicherort*]. Wenn Sie ein Webpaket in einer IIS-Website bereitstellen möchten, sollten Sie diese Werte verwenden:

- Die **– Quelle** Anbieter wird stets [Paket](https://technet.microsoft.com/library/dd569019(WS.10).aspx). Zum Beispiel:

    [!code-console[Main](deploying-web-packages/samples/sample4.cmd)]
- Die **– Dest** Anbieter wird stets [automatisch](https://technet.microsoft.com/library/dd569016(WS.10).aspx). Zum Beispiel:

    [!code-console[Main](deploying-web-packages/samples/sample5.cmd)]
- Die **– Verb** ist immer **Sync**.

    [!code-console[Main](deploying-web-packages/samples/sample6.cmd)]

Darüber hinaus benötigten an verschiedenen anderen [anbieterspezifische Einstellungen](https://technet.microsoft.com/library/dd569001(WS.10).aspx) und allgemeine [Einstellungen für den](https://technet.microsoft.com/library/dd569089(WS.10).aspx). Nehmen wir beispielsweise an, dass Sie die ContactManager.Mvc-Webanwendung in einer Stagingumgebung bereitstellen möchten. Die Bereitstellung wird als Ziel der Web-Handler bereitstellen und muss die Standardauthentifizierung verwenden. Um die Webanwendung bereitzustellen, müssen Sie die nächsten Schritte ausführen.

**Bereitstellen eine Webanwendung mithilfe von MSDeploy.exe**

1. Erstellen und Packen Sie das Webanwendungsprojekt, siehe [erstellen und Verpacken von Webanwendungsprojekten](building-and-packaging-web-application-projects.md).
2. Ändern der *ContactManager.Mvc.SetParameters.xml* -Datei die richtigen Parameterwerte für Ihre Stagingumgebung, siehe [Konfigurieren von Parametern für die Bereitstellung von Paket](configuring-parameters-for-web-package-deployment.md).
3. Öffnen Sie ein Eingabeaufforderungsfenster, und navigieren Sie zum Speicherort der MSDeploy.exe. Dies ist in der Regel am %PROGRAMFILES%\IIS\Microsoft Web bereitstellen V2\msdeploy.exe.
4. Geben Sie folgenden Befehl aus, und drücken Sie dann die EINGABETASTE (ignorieren Sie die Zeilenumbrüche):

    [!code-console[Main](deploying-web-packages/samples/sample7.cmd)]

In diesem Beispiel:

- Die **– Quelle** Parameter gibt die **Paket** Anbieter und gibt den Speicherort des Webpakets.
- Die **– Dest** Parameter gibt die **automatisch** Anbieter. Die **ComputerName** Einstellung bietet die Dienst-URL für den Handler für die Web-Bereitstellung auf dem Zielserver. Die **Authtype** Einstellung gibt an, dass Sie die Standardauthentifizierung verwenden möchten, und daher Sie angeben müssen einer **Benutzername** und **Kennwort**. Zum Schluss die **IncludeAcls = "False"** Einstellung gibt an, dass Sie nicht die Zugriffssteuerungslisten (ACLs) der Dateien in der Source-Webanwendung auf den Zielserver kopieren möchten.
- Die **– Verb: Synchronisierung** Argument gibt an, dass den Quellinhalt auf dem Zielserver repliziert werden soll.
- Die **– DisableLink** Argumente anzugeben, dass Sie nicht, um Anwendungspools, Konfiguration des virtuellen Verzeichnisses oder Secure Sockets Layer (SSL) Zertifikate auf dem Zielserver zu replizieren möchten. Weitere Informationen finden Sie unter [bereitstellen Link Webdiensterweiterungen](https://technet.microsoft.com/library/dd569028(WS.10).aspx).
- Die **– SetParamFile** Parameter gibt die Position der *"SetParameters.xml"* Datei.
- Die **AllowUntrusted** Option gibt an, dass Web Deploy-SSL-Zertifikate akzeptieren soll, die nicht von einer vertrauenswürdigen Zertifizierungsstelle ausgestellt wurden. Wenn Sie auf den Handler für die Web-Bereitstellung bereitstellen, und Sie ein selbst signiertes Zertifikat verwendet haben, um die Dienst-URL zu sichern, müssen Sie diese Option enthalten.

## <a name="automating-web-package-deployment"></a>Automatisieren der Bereitstellung der Web-Paket

In vielen Unternehmen konzipiert sollten Sie Ihre Webpakete als Teil eines größeren Schritt für Schritt oder einer automatisierten Bereitstellung bereitzustellen. Unabhängig davon, ob Sie zum Bereitstellen Ihrer Web-Pakete durch Ausführen der *. "Deploy.cmd"* Datei oder MSDeploy.exe direkt verwenden, können Sie Ihre Befehle zu parametrisieren und rufen Sie diese über ein Ziel in einer Microsoft Build Engine (MSBuild) die Projektdatei.

In der Contact Manager-beispiellösung, sehen Sie sich die **PublishWebPackages** im Ziel die *Publish.proj* Datei. Dieses Ziel wird einmal ausgeführt, für die einzelnen *. "Deploy.cmd"* Datei, die eine Liste mit dem Namen identifizierte **PublishPackages**. Das Ziel verwendet Eigenschaften und Elementmetadaten, erstellen Sie eine vollständige Gruppe von Argumentwerten für jedes *. "Deploy.cmd"* -Datei und dann verwendet, die **Exec** Aufgabe zum Ausführen des Befehls.


[!code-xml[Main](deploying-web-packages/samples/sample8.xml)]


> [!NOTE]
> Eine umfassendere Übersicht des Projektmodells-Datei in die Projektmappe, und eine Einführung in benutzerdefinierte Projektdateien im Allgemeinen finden Sie unter [Grundlegendes zur Projektdatei](understanding-the-project-file.md) und [Verständnis des Prozesses erstellen](understanding-the-build-process.md).


## <a name="endpoint-considerations"></a>Endpunkt-Überlegungen

Unabhängig davon, ob Sie Ihre Web-Paket bereitstellen, mit der *. "Deploy.cmd"* Datei oder MSDeploy.exe direkt verwenden, müssen Sie einen Computernamen oder einen Dienstendpunkt für Ihre Bereitstellung angeben.

Wenn die Ziel-Web-Server für die Bereitstellung, die mit dem Web Bereitstellen von Remote-Agent-Dienst konfiguriert ist, geben Sie die Ziel-URL als Ziel.


[!code-console[Main](deploying-web-packages/samples/sample9.cmd)]


Alternativ können Sie den Servernamen, die nur als Ziel angeben, und Web Deploy wird die remote-Agent-Dienst-URL abgeleitet.


[!code-console[Main](deploying-web-packages/samples/sample10.cmd)]


Wenn die Ziel-Web-Server mit der Web-bereitstellen-Handler für die Bereitstellung konfiguriert ist, müssen Sie die Endpunktadresse des den IIS-Web-Verwaltungsdienst (WMSvc) als Ziel angeben. Standardmäßig weist diese der Form auf:


[!code-console[Main](deploying-web-packages/samples/sample11.cmd)]


Sie können diese Endpunkte, die entweder Ziel der *. "Deploy.cmd"* Datei- oder MSDeploy.exe direkt. Jedoch wenn Sie als Benutzer ohne Administratorrechte auf den Handler für die Web-Bereitstellung bereitstellen, siehe möchten [Konfigurieren eines Webservers für Bereitstellen von Webpublishing (Web bereitstellen Handler)](../configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler.md), müssen Sie eine Abfragezeichenfolge an die Dienst-Endpunktadresse hinzufügen.


[!code-console[Main](deploying-web-packages/samples/sample12.cmd)]


Dies ist, da der Benutzer ohne Administratorrechte auf Serverebene IIS keine Zugriffsberechtigung besitzt. Er hat nur Zugriff auf eine bestimmte IIS-Website. Sie können nicht zum Zeitpunkt der geschrieben wird, aufgrund eines Fehlers in der Web Publishing Pipeline (WPP), Ausführen der *. "Deploy.cmd"* mithilfe einer Endpunktadresse, die eine Abfragezeichenfolge enthält die Datei. In diesem Szenario müssen Sie Ihre Webpaket MSDeploy.exe direkt mit bereitstellen.

> [!NOTE]
> Weitere Informationen zu den Web Deploy Remote-Agents-Dienst und der Handler für die Web-Bereitstellung, finden Sie unter [Entscheidung zur Webbereitstellung rechts](../configuring-server-environments-for-web-deployment/choosing-the-right-approach-to-web-deployment.md). Anleitungen zum Konfigurieren Ihrer umgebungsspezifische Projektdateien, die diesen Endpunkten bereitstellen, finden Sie unter [Konfigurieren von Bereitstellungseigenschaften für eine Zielumgebung](../configuring-server-environments-for-web-deployment/configuring-deployment-properties-for-a-target-environment.md).


## <a name="authentication-considerations"></a>Überlegungen zur Authentifizierung

Unabhängig davon, ob Sie Ihre Web-Paket bereitstellen, mit der *. "Deploy.cmd"* Datei oder MSDeploy.exe direkt verwenden, müssen Sie einen Authentifizierungstyp angeben. Web Deploy akzeptiert zwei mögliche Werte: **NTLM** oder **grundlegende**. Wenn Sie die Standardauthentifizierung angeben, müssen Sie auch einen Benutzernamen und ein Kennwort angeben. Es gibt verschiedene Faktoren zu beachten, wenn Sie einen anderen Authentifizierungstyp auswählen:

- Wenn Sie mit dem Web Bereitstellen von Remote-Agent-Dienst bereitstellen, müssen Sie NTLM-Authentifizierung verwenden. Der remote-Agent-Dienst akzeptiert keine standardmäßigen Authentifizierungsanmeldeinformationen.
- Wenn Sie auf den Handler für die Web-Bereitstellung bereitstellen, können Sie NTLM oder Standardauthentifizierung verwenden. Die Standardeinstellung ist die Standardauthentifizierung. Zwar Benutzernamen und Kennwörter unverschlüsselt als nur-Text-Standardauthentifizierung verwendet, werden Ihre Anmeldeinformationen geschützt, da der Handler für die Web-Bereitstellung immer SSL-Verschlüsselung verwendet.
- Wenn Ihre Web-Paket eine Datenbank enthält und den Webserver und Datenbankserver separate Computer sind, nicht möglich zum Bereitstellen der Datenbank mithilfe von NTLM-Authentifizierung aufgrund der [NTLM "doppelhop" Einschränkung](https://go.microsoft.com/?linkid=9805120). Sie müssen SQL Server-Anmeldeinformationen in der Verbindungszeichenfolge für die Bereitstellung verwenden, oder geben Sie Anmeldeinformationen für Standardauthentifizierung zu Web Deploy. Dieses Problem wird ausführlicher beschrieben [Bereitstellen von Datenbankrollenmitgliedschaften in Enterprise-Umgebungen](../advanced-enterprise-web-deployment/deploying-membership-databases-to-enterprise-environments.md).

## <a name="conclusion"></a>Schlussbemerkung

In diesem Thema beschrieben, wie Sie eine Web-Paket entweder mit Bereitstellen der *. "Deploy.cmd"* Datei oder direkt mithilfe von MSDeploy.exe. Er erläutert, wenn jeder Ansatz kann sinnvoll sein, und es wurde beschrieben, wie Sie parametrisieren können, und führen Sie einen Bereitstellungsbefehl als Teil eines größeren einstufiger oder automatisierte Build-Prozesses.

## <a name="further-reading"></a>Weiterführende Themen

Anweisungen zum Erstellen und einem Webbereitstellungspaket parametrisieren, finden Sie unter [erstellen und Verpacken von Webanwendungsprojekten](building-and-packaging-web-application-projects.md) und [Konfigurieren von Parametern für die Bereitstellung von Paket](configuring-parameters-for-web-package-deployment.md). Anweisungen zum Erstellen und Bereitstellen von Webpaketen aus einer Instanz von Team Foundation Server (TFS), finden Sie unter [Konfigurieren von Team Foundation Server für die automatisierte Bereitstellung](../configuring-team-foundation-server-for-web-deployment/configuring-team-foundation-server-for-web-deployment.md). Informationen zum Anpassen und den Bereitstellungsprozess zu beheben, finden Sie unter [Ausschließen von Dateien und Ordner von der Bereitstellung](../advanced-enterprise-web-deployment/excluding-files-and-folders-from-deployment.md).

> [!div class="step-by-step"]
> [Zurück](configuring-parameters-for-web-package-deployment.md)
> [Weiter](deploying-database-projects.md)
