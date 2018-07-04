---
uid: web-forms/overview/deployment/configuring-server-environments-for-web-deployment/choosing-the-right-approach-to-web-deployment
title: Auswählen der richtigen Vorgehensweise zur Webbereitstellung | Microsoft-Dokumentation
author: jrjlee
description: Wenn Sie mit der Internet Information Services (IIS)-Webbereitstellungstool (Web Deploy) 2.0 oder höher arbeiten, stehen die drei wichtigsten Ansätze, die Sie verwenden können, um zu erhalten...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 787a53fd-9901-4a11-9d58-61e0509cda45
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/deployment/configuring-server-environments-for-web-deployment/choosing-the-right-approach-to-web-deployment
msc.type: authoredcontent
ms.openlocfilehash: f1c3a09d9e960a53a25e0dd0b953224204a9ba7b
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/03/2018
ms.locfileid: "37367744"
---
<a name="choosing-the-right-approach-to-web-deployment"></a>Auswählen der richtigen Vorgehensweise zur Webbereitstellung
====================
durch [Jason Lee](https://github.com/jrjlee)

[PDF herunterladen](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Wenn Sie mit der Internet Information Services (IIS)-Webbereitstellungstool (Web Deploy) 2.0 oder höher arbeiten, stehen die drei wichtigsten Ansätze, die Sie verwenden können, um Ihre App-Pakete Webanwendungen auf einem Webserver zu erhalten. Sie können entweder:
> 
> - Bereitstellen die Anwendung von einem Remotestandort aus auf die *Webbereitstellungs-Agent-Dienst* (auch bekannt als "remote-Agent") auf dem Zielserver.
> - Bereitstellen Sie die Anwendung von einem Remotestandort aus mithilfe von Web Deploy bei Bedarf (auch bekannt als "temporäre Agent" bezeichnet).
> - Bereitstellen die Anwendung von einem Remotestandort aus auf die *IIS Web bereitstellen Handler* auf dem Zielserver.
> - Stellen Sie die Anwendung manuell kopieren des Pakets auf den Zielserver, und sie über IIS-Manager zu importieren.
> 
> Wie Sie Ihre Ziel-Webserver konfigurieren hängt die Ansatz zur Bereitstellung, die Sie verwenden möchten. In diesem Thema helfen Ihnen bei der Entscheidung, welcher Ansatz zur Bereitstellung für Sie geeignet ist.


Diese Tabelle zeigt die wichtigsten vor- und Nachteile der einzelnen Bereitstellungsansatz, zusammen mit den Szenarien, die i. d. r. jeder Ansatz entsprechen.

| Ansatz | Vorteile | Nachteile | Typische Szenarien |
| --- | --- | --- | --- |
| Remote-Agent | Es ist einfach einzurichten. Es eignet sich für regelmäßige Updates auf Webanwendungen und Inhalt. | Der Benutzer muss ein Administrator auf dem Zielserver sein. der Benutzer kann nicht auf alternative Anmeldeinformationen angeben. | Entwicklungsumgebungen. Testumgebungen. |
| Temp-Agent | Es ist nicht erforderlich, Web Deploy auf dem Zielcomputer installieren. Die neueste Version von Web Deploy wird automatisch verwendet. | Der Benutzer muss ein Administrator auf dem Zielserver sein. Der Benutzer kann nicht auf alternative Anmeldeinformationen angeben. | Entwicklungsumgebungen. Testumgebungen. |
| Web Deploy-Handler | Benutzer ohne Administratorrechte können Inhalte bereitstellen. Es eignet sich für regelmäßige Updates auf Webanwendungen und Inhalt. | Es ist sehr viel komplizierter eingerichtet wird. | Stagingumgebungen. Intranet-produktionsumgebungen. Gehostete Umgebungen. |
| Offline-Bereitstellung | Es ist sehr einfach einrichten. Es ist für isolierte Umgebungen geeignet. | Der Serveradministrator muss manuell kopieren und importieren das Webpaket jedes Mal. | Internetzugriff von produktionsumgebungen. Isolierten netzwerkumgebungen. |
  

## <a name="using-the-remote-agent"></a>Verwenden den Remote-Agent

Wenn Sie Web Deploy unter Verwendung der Standardeinstellungen auf einem Zielserver installieren, wird der Webbereitstellungs-Agent-Dienst ("remote-Agent") automatisch installiert und gestartet. Standardmäßig stellt der remote-Agent einen HTTP-Endpunkt unter dieser Adresse:


[!code-console[Main](choosing-the-right-approach-to-web-deployment/samples/sample1.cmd)]


> [!NOTE]
> Ersetzen Sie [*Server*] mit den Computernamen des Webservers, eine IP-Adresse für den Webserver oder einen Hostnamen, die aufgelöst wird auf Ihren Webserver.


Server-Administratoren können Web-Pakete von einem Remotestandort befindet, wie einem Entwicklungscomputer oder einem Buildserver bereitstellen, durch Angeben dieser Endpunktadresse. Nehmen wir beispielsweise an, dass Matt Hink bei Fabrikam, Inc. das ContactManager.Mvc Webanwendungsprojekt auf seinem Entwicklercomputer erstellt wurde. Während des Buildprozesses werden ein Webpaket, zusammen mit einem *. "Deploy.cmd"* Datei, die die Web Deploy-Befehle enthält, die zum Installieren des Pakets erforderlich sind. Ist Matt ein Serveradministrator auf dem Server TESTWEB1, kann er die Webanwendung mit dem Test-Webserver bereit, durch Ausführen dieses Befehls auf seinem Entwicklercomputer:


[!code-console[Main](choosing-the-right-approach-to-web-deployment/samples/sample2.cmd)]


Tatsächliche tatsächlich kann die Web Deploy-ausführbare Datei die Endpunktadresse des remote-Agent ableiten, wenn Sie den Namen des Computers angeben, damit Matt nur ein solches muss:


[!code-console[Main](choosing-the-right-approach-to-web-deployment/samples/sample3.cmd)]


> [!NOTE]
> Weitere Informationen zu Web Deploy-Befehlszeilensyntax und *. "Deploy.cmd"* finden Sie unter [Vorgehensweise: Installieren eines Bereitstellung Pakets mithilfe der Datei "Deploy.cmd"](https://msdn.microsoft.com/library/ff356104.aspx).


Der remote-Agent bietet eine einfache Möglichkeit zum Bereitstellen von Inhalt von einem Remotestandort aus, und dieser Ansatz kann funktionieren gut mit nur einem Klick oder automatisierte Bereitstellung. Allerdings muss der Benutzer, die der Bereitstellungsbefehl führt auch entweder ein Domänenadministrator oder ein Mitglied der lokalen Administratorengruppe auf dem Zielserver. Darüber hinaus unterstützt der remote-Agent über eine Standardauthentifizierung, deshalb Sie alternative Anmeldeinformationen in der Befehlszeile übergeben können.

Der remote-Agent stellt hilfreich bei der Bereitstellung in Entwicklungs- oder Testszenarien, in denen es ist nicht ungewöhnlich, dass Entwickler Hauptadministrator Kontrolle über eine testumgebung für die Server, und Anwendungen werden in der Regel neu erstellt und erneut bereitgestellt, sehr bereit. häufig. Dieser Ansatz ist jedoch in der Regel weniger akzeptabel ist, für die Staging-oder produktionsumgebungen.

Ein End-to-End-Beispiel für ein Szenario, den remote-Agent-Ansatz verwendet, werden soll, finden Sie unter [Szenario: Konfigurieren einer Testumgebung für die Webbereitstellung](scenario-configuring-a-test-environment-for-web-deployment.md).

## <a name="using-the-temp-agent"></a>Mithilfe des Temp-Agents

Der temporäre-Agent-Ansatz für die Bereitstellung ist ähnlich wie bei der remote-Agent. Jedoch im Gegensatz zu den remote-Agent-Ansatz müssen Sie Web Deploy auf dem Ziel-Webserver zu installieren. Stattdessen Web Deploy beim Ausführen der bereitstellungs wird eine temporäre Version der webbereitstellungs-Agent-Dienst auf dem Zielserver installiert und Hiermit können Sie Ihre Inhalte unter IIS bereitzustellen. Wenn die Bereitstellung abgeschlossen ist, werden alle temporäre Dateien entfernt.

Wenn Sie verwenden möchten, verwenden Sie den temporären Agentanbieter, die festlegen, fügen Sie der **/g** Flag, um Ihre Bereitstellungsbefehl:


[!code-console[Main](choosing-the-right-approach-to-web-deployment/samples/sample4.cmd)]


> [!NOTE]
> Sie können nicht den temp-Agent verwenden, wenn der webbereitstellungs-Agent-Dienst auf dem Zielcomputer installiert ist, auch wenn der Dienst nicht ausgeführt wird.


Der Vorteil dieses Ansatzes ist, dass Sie nicht warten von Installationen von Web Deploy auf Ihrem Zielserver müssen. Darüber hinaus müssen Sie sicherstellen, dass die Quelle und Ziel-Computer die gleiche Version von Web Deploy ausgeführt werden. Allerdings unterliegt dieser Ansatz principal dieselben Einschränkungen wie bei den Ansatz der remote-Agent, nämlich, dass Sie ein lokaler Administrator auf dem Zielserver sein müssen, um Inhalte bereitzustellen, und nur die NTLM-Authentifizierung wird unterstützt. Der temporäre-Agent-Ansatz erfordert auch viel mehr Erstkonfiguration der zielumgebung.

Weitere Informationen zur Verwendung von des temp-Agents finden Sie unter [Vorgehensweise: Installieren eines Bereitstellung Pakets mithilfe der Datei "Deploy.cmd"](https://msdn.microsoft.com/library/ff356104.aspx) und [Web Deploy bei Bedarf](https://technet.microsoft.com/library/ee517345(WS.10).aspx).

## <a name="using-the-web-deploy-handler"></a>Über die Web Deploy-Handler

Für IIS 7 oder höher bietet Web Deploy einen anderer Bereitstellungsansatz über den IIS-Web-bereitstellen-Ereignishandler aus. Der Handler für die Web-bereitstellen, ist eng mit dem IIS-Web-Verwaltungsdienst (WMSvc), integriert werden, die konzipiert wurde, ermöglichen Benutzern das IIS-Websites von Remotestandorten aus zu verwalten.

Standardmäßig stellt der remote-Agent einen HTTP-Endpunkt unter dieser Adresse:


[!code-console[Main](choosing-the-right-approach-to-web-deployment/samples/sample5.cmd)]


> [!NOTE]
> Ersetzen Sie [*Server*] mit den Computernamen des Webservers, eine IP-Adresse für den Webserver oder einen Hostnamen, die aufgelöst wird auf Ihren Webserver.


Der große Vorteil der Handler für die Web-Bereitstellung über den remote-Agent und den temporären-Agent ist, dass Sie IIS so Benutzer ohne Administratorrechte für die Bereitstellung von Anwendungen und Inhalte auf bestimmte IIS-Websites konfigurieren können. Der Handler für die Web-Bereitstellung unterstützt auch Standardauthentifizierung, damit Sie andere Anmeldeinformationen als Parameter in Ihrer Web Deploy-Befehlen bereitstellen können. Die großen Nachteil ist, dass die Web-bereitstellen-Handler anfänglich wesentlich komplizierter einrichten und konfigurieren.

Bei Benutzern ohne Administratorrechte lässt den Webverwaltungsdienst (WMSvc) den Benutzer eine Verbindung zu IIS herstellen nur über eine Verbindung Workflowdokumentbibliothek auf Siteebene, statt über eine Verbindung auf Serverebene. Für den Zugriff auf eine bestimmte Website, können Sie eine standortspezifischen Abfragezeichenfolge in der Endpunktadresse enthält:


[!code-console[Main](choosing-the-right-approach-to-web-deployment/samples/sample6.cmd)]


Nehmen wir beispielsweise an, dass ein Buildprozess konfiguriert ist, um eine Webanwendung in einer Stagingumgebung nach jedem erfolgreichen Build automatisch bereitzustellen. Wenn Sie den remote-Agent-Ansatz verwendet, würde müssen Sie der Build-Prozess-ID einen Administrator auf Ihrem Zielserver. Im Gegensatz dazu mit dem Ansatz WebHandler bereitstellen Sie einen Benutzer ohne Administratorrechte erhalten&#x2014;**FABRIKAM\stagingdeployer** in diesem Fall&#x2014;Berechtigung, eine bestimmte IIS-Website und des Buildprozesses bieten diese Anmeldeinformationen für das Webpaket bereitzustellen.


[!code-console[Main](choosing-the-right-approach-to-web-deployment/samples/sample7.cmd)]


> [!NOTE]
> Weitere Informationen zu Web Deploy Befehlszeileneingaben und deren Syntax finden Sie unter [Bereitstellen über die Befehlszeile Webverweis](https://technet.microsoft.com/library/dd568991(v=ws.10).aspx). Weitere Informationen zur Verwendung der *. "Deploy.cmd"* finden Sie unter [wie: Installieren eines Bereitstellung Pakets mithilfe der Datei "Deploy.cmd"](https://msdn.microsoft.com/library/ff356104.aspx).


Die Web-bereitstellen-Handler stellt die hilfreich bei der Bereitstellung in staging-Umgebungen, gehostete Umgebungen und intranetbasierte produktionsumgebungen, in denen Remotezugriff auf dem Server ist verfügbar, aber Administratoranmeldeinformationen sind nicht bereit.

Ein End-to-End-Beispiel für ein Szenario, den Bereitstellen von Web-Handler Ansatz verwendet, werden soll, finden Sie unter [Szenario: Konfigurieren einer Stagingumgebung für die Webbereitstellung](scenario-configuring-a-staging-environment-for-web-deployment.md).

## <a name="using-offline-deployment"></a>Mithilfe der Offline-Bereitstellung

In einigen Fällen ist es nicht möglich oder praktikabel ist, die Bereitstellung von Anwendungen und Inhalte auf einer IIS-Website von einem Remotestandort aus. Klicken Sie z. B. Quell-und Ziel möglicherweise in isolierten Netzwerken oder Netzwerksegmenten, oder -Firewall-Richtlinie kann nicht remote-Zugriff zulassen.

In Szenarien wie diese können Sie weiterhin das Verpacken und veröffentlichen die Funktionen von Web Deploy verwenden. Sie können nicht nur von einem Remotestandort aus verwenden. Stattdessen muss ein Administrator auf dem Zielserver kopieren des Pakets auf dem Server, und importieren es über IIS-Manager.

![](choosing-the-right-approach-to-web-deployment/_static/image1.png)

Die offlinebereitstellung-Ansatz ist in der Regel in Internetzugriff produktionsumgebungen, in dem Server in einem Umkreisnetzwerk Verbindungen mit Computern im internen Netzwerk beschränkt ist möglicherweise nützlich.

Ein End-to-End-Beispiel für ein Szenario, das die offlinebereitstellung-Ansatz verwendet werden soll, finden Sie unter [Szenario: Konfigurieren einer Produktionsumgebung für die Webbereitstellung](scenario-configuring-a-production-environment-for-web-deployment.md).

## <a name="further-reading"></a>Weiterführende Themen

Weitere Informationen zu Web Deploy Befehlszeileneingaben und deren Syntax finden Sie unter [Bereitstellen über die Befehlszeile Webverweis](https://technet.microsoft.com/library/dd568991(v=ws.10).aspx). Weitere Informationen zur Verwendung der *. "Deploy.cmd"* finden Sie unter [wie: Installieren eines Bereitstellung Pakets mithilfe der Datei "Deploy.cmd"](https://msdn.microsoft.com/library/ff356104.aspx).

Eine allgemeine Anleitung mit den verschiedenen Vorgehensweisen, in dem Sie Web-Pakete von einem Remotecomputer bereitstellen können, finden Sie unter [mithilfe von Web Deploy über eine Remoteverbindung](https://technet.microsoft.com/library/ee461175(WS.10).aspx). Weitere Informationen zur Verwendung von Web Deploy bei Bedarf finden Sie unter [Web Deploy bei Bedarf](https://technet.microsoft.com/library/ee517345(WS.10).aspx).

> [!div class="step-by-step"]
> [Zurück](configuring-server-environments-for-web-deployment.md)
> [Weiter](scenario-configuring-a-test-environment-for-web-deployment.md)
