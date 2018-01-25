---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/deploying-web-packages
title: Bereitstellen von Webpaketen | Microsoft Docs
author: jrjlee
description: "In diesem Thema wird beschrieben, wie Sie Web-Bereitstellungspakete mit einem Remoteserver veröffentlichen können, mit dem Internet Information Services (IIS)-Webbereitstellungstool (Web..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: a5c5eed2-8683-40a5-a2e1-35c9f8d17c29
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/deploying-web-packages
msc.type: authoredcontent
ms.openlocfilehash: cd2bfa07262155b68ac4605fc7e9748d276d3193
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/24/2018
---
<a name="deploying-web-packages"></a>Bereitstellen von Webpaketen
====================
durch [Jason Lee](https://github.com/jrjlee)

[PDF herunterladen](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> In diesem Thema wird beschrieben, wie Sie Web-Bereitstellungspakete mit einem Remoteserver veröffentlichen können, mithilfe der Internetinformationsdienste (Internet Information Services, IIS)-Webbereitstellungstool (Web Deploy) 2.0.
> 
> Es gibt zwei Hauptmethoden, in denen Sie eine Web-Paket mit einem Remoteserver bereitstellen können:
> 
> - Sie können das Befehlszeilenprogramm "MSDeploy.exe" direkt verwenden.
> - Können Sie ausführen, die *[Projektname].deploy.cmd* -Datei, die während des Erstellungsprozesses generiert.
> 
> Das Endergebnis ist identisch, unabhängig davon, welchen, die Ansatz Sie verwenden. Im Wesentlichen alle der *. deploy.cmd* Datei ist MSDeploy.exe mit einigen vorher festgelegten Werten ausgeführt, damit Sie nicht so viele Informationen geben an, um das Paket bereitstellen müssen. Dadurch wird die Bereitstellung vereinfacht. Andererseits, MSDeploy.exe direkt mit viel mehr Flexibilität über genau wie das Paket bereitgestellt wird.
> 
> Ansatz hängt von verschiedenen Faktoren ab, einschließlich wie viel Kontrolle über den Bereitstellungsprozess sind erforderlich und gibt an, ob Sie Web Deploy Remote-Agents-Dienst oder der Handler für die Web-Bereitstellung als Ziel bestimmen. In diesem Thema wird erläutert, wie jeder Ansatz zu verwenden und identifiziert, wenn jeder Ansatz geeignet ist.
> 
> Aufgaben und exemplarische Vorgehensweisen in diesem Thema wird davon ausgegangen, die aus:
> 
> - Sie erstellt haben, und Ihre Webanwendung verpackt, wie in beschrieben [erstellen und Packen Webanwendungsprojekte](building-and-packaging-web-application-projects.md).
> - Verändert wurde die *SetParameters.xml* um die richtigen Parameterwerte für Ihre zielumgebung zu verbessern, wie in beschrieben [Konfigurieren von Parametern für die Bereitstellung von Paketen](configuring-parameters-for-web-package-deployment.md).


Ausführen der [*Projektname*]*. deploy.cmd* Datei ist die einfachste Möglichkeit zum Bereitstellen eines Webpakets. Insbesondere das Verwenden der *. deploy.cmd* Datei bietet folgende Vorteile gegenüber MSDeploy.exe direkt:

- Sie müssen den Speicherort des Webbereitstellungspaket & #x 2014; angeben der *. deploy.cmd* Datei bereits bekannt ist.
- Sie müssen den Speicherort der an die *SetParameters.xml* Datei & #x 2014; die *. deploy.cmd* Datei bereits bekannt, wo es ist.
- Sie müssen nicht angeben von Quelle und Ziel MSDeploy-Anbieter & #x 2014; die *. deploy.cmd* Datei bereits weiß, welche Werte verwendet.
- Sie müssen nicht angeben von MSDeploy Vorgang Einstellungen & #x 2014; die *. deploy.cmd* Datei fügt die häufigsten erforderlichen Werte automatisch an den MSDeploy.exe-Befehl.

Vor der Verwendung der *. deploy.cmd* Datei zum Bereitstellen eines Webpakets sollten Sie sicherstellen, dass:

- Die *. deploy.cmd* Datei, die [*Projektname*]. *SetParameters.xml* Datei und die Web-Paket ([*Projektname*]. *ZIP*) befinden sich im gleichen Ordner.
- Web Deploy (MSDeploy.exe) ist installiert, auf dem Computer mit der *. deploy.cmd* Datei.

Die *. deploy.cmd* Datei unterstützt verschiedene Befehlszeilenoptionen stehen zur Verfügung. Wenn Sie die Datei an einer Eingabeaufforderung ausführen, ist dies die grundlegende Syntax:


[!code-console[Main](deploying-web-packages/samples/sample1.cmd)]


Geben Sie einen **/t** Flag oder ein **/y** Flag, um anzugeben, ob Sie eine Testversion ausführen oder eine aktive Bereitstellung bzw. ausführen möchten (verwenden Sie nicht beide Flags im selben Befehl). Diese Tabelle erläutert den Zweck der einzelnen Flags.

| Flag | Beschreibung |
| --- | --- |
| **/T** | Ruft MSDeploy.exe mit der **"-WhatIf"** Flag, das einer Testversion ausführen angibt. Anstelle der Bereitstellung des Pakets, erstellt es einen Bericht von Was geschieht, wenn das Paket bereitgestellt werden konnten. |
| **/Y** | Ruft die MSDeploy.exe ohne die **"-WhatIf"** Flag. Dadurch wird das Paket auf dem lokalen Computer oder der angegebene Zielserver bereitgestellt. |
| **/M** | Gibt den Zielserver name oder die Dienst-URL. Weitere Informationen zu den Werten, die Sie hier angeben können, finden Sie unter der **Endpunkt Überlegungen** Abschnitt dieses Themas. Wenn Sie weglassen der **/m** Flag, das Paket wird auf dem lokalen Computer bereitgestellt werden. |
| **/A** | Gibt den Authentifizierungstyp, den MSDeploy.exe verwenden sollten, um die Bereitstellung auszuführen. Mögliche Werte sind **NTLM** und **grundlegende**. Wenn Sie weglassen der **/a** Flag der Authentifizierungstyp standardmäßig **NTLM** für die Bereitstellung auf dem Web bereitstellen Remote-Agent-Dienst und **grundlegende** für die Bereitstellung in der Web Deploy Handler. |
| **/U** | Gibt den Benutzernamen an. Dies gilt nur, wenn Sie Standardauthentifizierung verwenden. |
| **/P** | Gibt das Kennwort an. Dies gilt nur, wenn Sie Standardauthentifizierung verwenden. |
| **/L** | Gibt an, dass das Paket mit der lokalen IIS Express-Instanz bereitgestellt werden soll. |
| **/G** | Gibt an, dass das Paket bereitgestellt wird, mithilfe der [TempAgent anbietereinstellung](https://technet.microsoft.com/library/ee517345(WS.10).aspx). Wenn Sie weglassen der **/g** -Flag wird als Standardwert **"false"**. |

> [!NOTE]
> Jedes Mal, wenn während des Erstellungsprozesses ein Webpaket erstellt, erstellt er auch eine Datei namens *[Projektname] ".deploy"-"Readme.txt"* , die diese Bereitstellungsoptionen erläutert.


Zusätzlich zu diesen Flags können Sie Einstellungen für Web Deploy Vorgang als zusätzliche angeben *. deploy.cmd* Parameter. Alle zusätzlichen Einstellungen, die Sie angeben, werden einfach an den zugrunde liegenden MSDeploy.exe-Befehl durch übergeben. Weitere Informationen zu diesen Einstellungen finden Sie unter [Web Deploy Vorgang Settings](https://technet.microsoft.com/library/dd569089(WS.10).aspx).

Angenommen, Sie möchten das Webanwendungsprojekt ContactManager.Mvc in einer testumgebung bereitstellen, durch Ausführen der *. deploy.cmd* Datei. Die testumgebung zur Verwendung des Diensts Web Deploy Remote-Agents konfiguriert ist, wie in beschrieben [konfigurieren Sie einen Webserver für bereitstellen Webveröffentlichung (Remote-Agent)](../configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-remote-agent.md). Um die Webanwendung bereitzustellen, müssen Sie die nächsten Schritte ausführen.

**Bereitstellen einer Anwendung mithilfe der. Datei deploy.cmd**

1. Erstellen und Packen Sie das Webanwendungsprojekt, wie in beschrieben [erstellen und Packen Webanwendungsprojekte](building-and-packaging-web-application-projects.md).
2. Ändern der *ContactManager.Mvc.SetParameters.xml* Datei, die die richtigen Parameterwerte für Ihre testumgebung enthalten, wie in beschrieben [Konfigurieren von Parametern für die Bereitstellung von Paketen](configuring-parameters-for-web-package-deployment.md).
3. Öffnen Sie ein Eingabeaufforderungsfenster, und navigieren Sie zum Speicherort der der *ContactManager.Mvc.deploy.cmd* Datei.
4. Geben Sie diesen Befehl, und drücken Sie dann die EINGABETASTE:

    [!code-console[Main](deploying-web-packages/samples/sample2.cmd)]

In diesem Beispiel:

- Die **/y** Flag gibt an, dass das Paket tatsächlich bereitgestellt werden soll, anstatt auf diese Weise einer Testversion ausführen.
- Die **/m** Flag gibt an, dass Sie das Paket auf dem Server mit dem Namen TESTWEB1 bereitstellen möchten. Dieser Wert wird MSDeploy.exe versucht, das Paket an den Webserver Bereitstellen von Remote-Agent-Dienst unter http://TESTWEB1/MSDeployAgentService bereitgestellt.
- Die **/a** Flag gibt an, dass NTLM-Authentifizierung verwendet werden soll. Daher müssen Sie einen Benutzernamen und ein Kennwort angeben.

Zur Veranschaulichung Verwendung der *. deploy.cmd* Datei vereinfacht die Bereitstellung, sehen Sie sich den MSDeploy.exe-Befehl, die generiert und ausgeführt, wenn Sie ausführen, ruft *ContactManager.Mvc.deploy.cmd* verwenden die oben angezeigten Optionen.


[!code-console[Main](deploying-web-packages/samples/sample3.cmd)]


Weitere Informationen zur Verwendung der *. deploy.cmd* Datei zum Bereitstellen einer Web-Paket finden [wie: Installieren Sie eine Bereitstellung Paket mithilfe der Datei deploy.cmd](https://msdn.microsoft.com/library/ff356104.aspx).

## <a name="using-msdeployexe"></a>Verwenden von MSDeploy.exe

Obwohl durch die Verwendung der *. deploy.cmd* Datei in der Regel vereinfacht die Bereitstellung, es gibt einige Situationen, wenn ist es empfehlenswert, MSDeploy.exe direkt zu verwenden. Zum Beispiel:

- Wenn Sie an den Webdienst bereitstellen Handler als Benutzer ohne Administratorrechte bereitstellen möchten, können keine der *. deploy.cmd* Datei. Dies ist aufgrund eines Fehlers in Web Deploy 2.0, wie beschrieben unter **Endpunkt Überlegungen**.
- Wenn Sie manuell zwischen verschiedenen wechseln möchten *SetParameters.xml* Dateien an verschiedenen Standorten, möglicherweise bevorzugen Sie MSDeploy.exe direkt verwenden.
- Wenn Sie mehrere MSDeploy.exe Befehlszeilenargumente überschreiben möchten, empfiehlt es sich, MSDeploy.exe direkt zu verwenden.

Wenn Sie auf "MSDeploy.exe" verwenden, müssen Sie drei wichtige Arten von Informationen bereitstellen:

- Ein **– Quelle** Parameter, der angibt, in dem die Daten stammen.
- Ein **– Dest** Parameter, der angibt, wo die Daten an.
- Ein **– Verb** Parameter, der angibt der [Vorgang](https://technet.microsoft.com/library/dd568989(WS.10).aspx) Sie ausführen möchten.

Verwendet MSDeploy.exe [Web Deploy-Anbieter](https://technet.microsoft.com/library/dd569040(WS.10).aspx) Quell- und Zielserver um Daten zu verarbeiten. Web Deploy umfasst zahlreiche von Anbietern, die den Bereich von Anwendungen und Datenquellen, funktioniert er mit & #x 2014 darstellen; es gibt z. B.-Anbieter für SQL Server-Datenbanken, IIS-Webservern, Zertifikate, global Assembly Cache (GAC)-Assemblys, verschiedene verschiedene Konfigurationsdateien und viele andere Typen von Daten. Sowohl die **– Quelle** Parameter und die **– Dest** Parameter muss einen Anbieter angeben, in der Form **– Quelle**: [*ProviderName*] = [*Speicherort*]. Wenn Sie eine Web-Paket auf einer IIS-Website bereitstellen, sollten Sie diese Werte verwenden:

- Die **– Quelle** Anbieter befindet sich immer [Paket](https://technet.microsoft.com/library/dd569019(WS.10).aspx). Zum Beispiel:

    [!code-console[Main](deploying-web-packages/samples/sample4.cmd)]
- Die **– Dest** Anbieter befindet sich immer [Auto](https://technet.microsoft.com/library/dd569016(WS.10).aspx). Zum Beispiel:

    [!code-console[Main](deploying-web-packages/samples/sample5.cmd)]
- Die **– Verb** ist immer **Sync**.

    [!code-console[Main](deploying-web-packages/samples/sample6.cmd)]

Darüber hinaus, benötigen Sie an verschiedene andere [anbieterspezifische Einstellungen](https://technet.microsoft.com/library/dd569001(WS.10).aspx) und allgemeine [Vorgang Einstellungen](https://technet.microsoft.com/library/dd569089(WS.10).aspx). Nehmen Sie z. B. an, dass die ContactManager.Mvc-Webanwendung in einer Stagingumgebung bereitgestellt werden soll. Die Bereitstellung wird als Ziel der Web-Handler bereitstellen und Standardauthentifizierung verwenden. Um die Webanwendung bereitzustellen, müssen Sie die nächsten Schritte ausführen.

**Bereitstellen eine Webanwendung mit MSDeploy.exe**

1. Erstellen und Packen Sie das Webanwendungsprojekt, wie in beschrieben [erstellen und Packen Webanwendungsprojekte](building-and-packaging-web-application-projects.md).
2. Ändern der *ContactManager.Mvc.SetParameters.xml* Datei, die die richtigen Parameterwerte für Ihre Stagingumgebung enthalten, wie in beschrieben [Konfigurieren von Parametern für die Bereitstellung von Paketen](configuring-parameters-for-web-package-deployment.md).
3. Öffnen Sie ein Eingabeaufforderungsfenster, und navigieren Sie zum Speicherort der MSDeploy.exe. Dies ist in der Regel zum %PROGRAMFILES%\IIS\Microsoft Web bereitstellen V2\msdeploy.exe.
4. Geben Sie diesen Befehl, und drücken Sie dann die EINGABETASTE (die Zeilenumbrüche ignorieren):

    [!code-console[Main](deploying-web-packages/samples/sample7.cmd)]

In diesem Beispiel:

- Die **– Quelle** Parameter gibt die **Paket** Anbieter und gibt den Speicherort des Webpakets.
- Die **– Dest** Parameter gibt die **Auto** Anbieter. Die **ComputerName** Einstellung bietet die Dienst-URL der Web-Handler bereitstellen, auf dem Zielserver. Die **Authtype** Einstellung gibt an, dass Sie die Standardauthentifizierung verwenden möchten, und daher Sie angeben müssen einer **Benutzername** und ein **Kennwort**. Schließlich die **IncludeAcls = "False"** Einstellung gibt an, dass Sie nicht die Zugriffssteuerungslisten (ACLs) der Dateien in der Source-Webanwendung auf den Zielserver kopieren möchten.
- Die **– Verb: Sync** Argument gibt an, dass den Quellinhalt auf dem Zielserver repliziert werden sollen.
- Die **– DisableLink** Argumente anzugeben, dass Anwendungspools, Konfiguration des virtuellen Verzeichnisses oder Secure Sockets Layer (SSL) Zertifikate auf dem Zielserver repliziert werden soll. Weitere Informationen finden Sie unter [Web bereitstellen Linkerweiterungen](https://technet.microsoft.com/library/dd569028(WS.10).aspx).
- Die **– SetParamFile** Parameter gibt die Position eines der *SetParameters.xml* Datei.
- Die **-AllowUntrusted** gibt an, dass Web Deploy-SSL-Zertifikate akzeptiert werden sollen, die nicht von einer vertrauenswürdigen Zertifizierungsstelle ausgestellt wurden. Wenn Sie an den Handler für die Web-Bereitstellung bereitstellen, und Sie ein selbst signiertes Zertifikat verwendet haben, um die Dienst-URL zu sichern, müssen Sie diesen Schalter.

## <a name="automating-web-package-deployment"></a>Automatisieren der Bereitstellung von Paketen

In vielen Unternehmensszenarien sollten Sie Ihre Webpakete als Teil eines größeren einstufiger oder automatisierte Bereitstellung bereitstellen. Unabhängig davon, ob Sie zum Bereitstellen von Ihrer Webpaketen durch Ausführen der *. deploy.cmd* Datei oder MSDeploy.exe direkt verwenden, können Sie Ihre Befehle zu parametrisieren und rufen sie ein Ziel in einer Microsoft Build Engine (MSBuild) die Projektdatei.

In der Beispielprojektmappe Contact Manager sehen Sie sich die **PublishWebPackages** als Ziel der *Publish.proj* Datei. Dieses Ziel wird einmal ausgeführt, für die einzelnen *. deploy.cmd* Datei, die eine Liste der mit dem Namen identifizierte **PublishPackages**. Das Ziel verwendet Eigenschaften und Metadaten von Elementen ein breites Spektrum an Argumentwerte für jede erstellen *. deploy.cmd* Datei und dann verwendet, die **Exec** Aufgabe zum Ausführen des Befehls.


[!code-xml[Main](deploying-web-packages/samples/sample8.xml)]


> [!NOTE]
> Eine umfassendere Übersicht über das Projektmodell-Datei in der Beispielprojektmappe und eine Einführung in benutzerdefinierte Projektdateien im Allgemeinen finden Sie unter [verstehen die Projektdatei](understanding-the-project-file.md) und [Verständnis des Build-Prozesses](understanding-the-build-process.md).


## <a name="endpoint-considerations"></a>Endpunkt-Überlegungen

Unabhängig davon, ob Sie Ihre Webpaket bereitgestellt werden durch Ausführen der *. deploy.cmd* Datei oder MSDeploy.exe direkt verwenden, müssen Sie einen Computernamen oder einen Dienstendpunkt für Ihre Bereitstellung angeben.

Wenn die Ziel-Webserver für die Bereitstellung über das Web Bereitstellen von Remote-Agent-Dienst konfiguriert ist, geben Sie die Ziel-URL als Ziel.


[!code-console[Main](deploying-web-packages/samples/sample9.cmd)]


Alternativ können Sie den Servernamen, die allein als Ziel angeben und Web Deploy remote-Agent-Dienst-URL abgeleitet wird.


[!code-console[Main](deploying-web-packages/samples/sample10.cmd)]


Wenn die Ziel-Webserver für die Bereitstellung mit dem Web bereitstellen Handler konfiguriert ist, müssen Sie die Endpunktadresse des IIS Web-Verwaltungsdiensts (WMSvc) als Ziel angeben. Standardmäßig weist diese Form auf:


[!code-console[Main](deploying-web-packages/samples/sample11.cmd)]


Sie können diese Endpunkte, die entweder Ziel der *. deploy.cmd* Datei- oder MSDeploy.exe direkt. Jedoch, wenn Sie als Benutzer ohne Administratorrechte an den Handler für die Web-Bereitstellung bereitstellen, wie in beschrieben möchten [konfigurieren Sie einen Webserver für bereitstellen Webveröffentlichung (Web bereitstellen Handler)](../configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler.md), müssen Sie die Adresse des Dienstendpunkts eine Abfragezeichenfolge hinzufügen.


[!code-console[Main](deploying-web-packages/samples/sample12.cmd)]


Dies ist, da der Benutzer ohne Administratorrechte auf Serverebene IIS nicht zugreifen. Er hat nur Zugriff auf eine bestimmte IIS-Website. Zum Zeitpunkt des Dokuments aufgrund eines Fehlers im Web veröffentlichen Pipeline (WPP), kann nicht ausführen der *. deploy.cmd* -Datendatei mithilfe einer Endpunktadresse, die eine Abfragezeichenfolge enthält. In diesem Szenario müssen Sie Ihre Webpaket MSDeploy.exe direkt mit bereitstellen.

> [!NOTE]
> Weitere Informationen auf dem Web Bereitstellen von Remote-Agent-Dienst und der Web-Handler bereitstellen können, finden Sie unter [Auswählen der rechts Ansatz für die Webbereitstellung](../configuring-server-environments-for-web-deployment/choosing-the-right-approach-to-web-deployment.md). Anleitung zum Konfigurieren Ihrer umgebungsspezifische Projektdateien mit diesen Endpunkten bereitstellen, finden Sie unter [Konfigurieren von Bereitstellungseigenschaften für eine Zielumgebung](../configuring-server-environments-for-web-deployment/configuring-deployment-properties-for-a-target-environment.md).


## <a name="authentication-considerations"></a>Überlegungen zur Authentifizierung

Unabhängig davon, ob Sie Ihre Webpaket bereitgestellt werden durch Ausführen der *. deploy.cmd* Datei oder MSDeploy.exe direkt verwenden, müssen Sie einen Authentifizierungstyp angeben. Web Deploy akzeptiert zwei mögliche Werte: **NTLM** oder **grundlegende**. Wenn Sie Standardauthentifizierung angeben, müssen Sie auch einen Benutzernamen und ein Kennwort angeben. Es gibt verschiedene Faktoren bedenken Sie bei der Auswahl eines Authentifizierungstyps müssen Sie ein:

- Wenn Sie mit dem Web Bereitstellen von Remote-Agent-Dienst bereitstellen, müssen Sie die NTLM-Authentifizierung verwenden. Der remote-Agent-Dienst akzeptiert keine Anmeldeinformationen für Standardauthentifizierung.
- Wenn Sie an den Handler für die Web-Bereitstellung bereitstellen, können Sie entweder NTLM oder die Standardauthentifizierung verwenden. Die Standardeinstellung ist die Standardauthentifizierung. Zwar Standardauthentifizierung Benutzernamen und Kennwörter im nur-Text übertragen werden benötigt, werden Ihre Anmeldeinformationen geschützt, da der Handler für die Web-Bereitstellung immer SSL-Verschlüsselung verwendet.
- Wenn Ihre Web-Paket eine Datenbank enthält und den Webserver und Datenbankserver separate Computer sind, kann nicht zum Bereitstellen der Datenbank mithilfe von NTLM-Authentifizierung aufgrund der [NTLM "Doppel-Hop"-Einschränkung](https://go.microsoft.com/?linkid=9805120). Sie müssen entweder SQL Server-Anmeldeinformationen verwenden, in der Verbindungszeichenfolge für die Bereitstellung oder Anmeldeinformationen für Standardauthentifizierung für Web Deploy. Dieses Problem wird ausführlich in [Mitgliedschaft-Datenbanken bereitstellen, um Unternehmensumgebungen](../advanced-enterprise-web-deployment/deploying-membership-databases-to-enterprise-environments.md).

## <a name="conclusion"></a>Schlussbemerkung

In diesem Thema beschrieben, wie Sie eine Web-Paket entweder durch Ausführen von bereitstellen können die *. deploy.cmd* Datei oder direkt mit MSDeploy.exe. Diese erläutert, wenn jeder Ansatz kann angemessen sein, und es beschrieben, wie Sie parametrisieren und führen Sie einen Bereitstellungsbefehl als Teil eines größeren einstufiger oder automatisierten Build-Prozesses.

## <a name="further-reading"></a>Weiterführende Themen

Anleitungen zum Erstellen und ein Webbereitstellungspaket parametrisieren finden Sie unter [erstellen und Packen Webanwendungsprojekte](building-and-packaging-web-application-projects.md) und [Konfigurieren von Parametern für die Bereitstellung von Paketen](configuring-parameters-for-web-package-deployment.md). Anleitungen zum Erstellen und Bereitstellen von Webpaketen aus einer Instanz von Team Foundation Server (TFS) finden Sie unter [Konfigurieren von Team Foundation Server für die automatisierte Bereitstellung](../configuring-team-foundation-server-for-web-deployment/configuring-team-foundation-server-for-web-deployment.md). Informationen zum Anpassen und Problembehandlung für den Bereitstellungsprozess finden Sie unter [Ausschließen von Dateien und Ordnern über Bereitstellung](../advanced-enterprise-web-deployment/excluding-files-and-folders-from-deployment.md).

>[!div class="step-by-step"]
[Zurück](configuring-parameters-for-web-package-deployment.md)
[Weiter](deploying-database-projects.md)
