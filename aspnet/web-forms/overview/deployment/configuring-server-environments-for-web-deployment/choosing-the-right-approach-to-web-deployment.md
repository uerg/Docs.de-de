---
uid: web-forms/overview/deployment/configuring-server-environments-for-web-deployment/choosing-the-right-approach-to-web-deployment
title: "Auswählen des richtigen Ansatzes zu Web Deploy | Microsoft Docs"
author: jrjlee
description: "Wenn Sie mit dem Internet Information Services (IIS) Webbereitstellungstool (Web Deploy) 2.0 oder höher arbeiten, stehen die drei wichtigsten Ansätze, die Sie, zum Abrufen verwenden können..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 787a53fd-9901-4a11-9d58-61e0509cda45
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/configuring-server-environments-for-web-deployment/choosing-the-right-approach-to-web-deployment
msc.type: authoredcontent
ms.openlocfilehash: b77aa37160f3822f58908866e44497aea3d3bdc8
ms.sourcegitcommit: 493a215355576cfa481773365de021bcf04bb9c7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/15/2018
---
<a name="choosing-the-right-approach-to-web-deployment"></a>Auswählen des richtigen Ansatzes zur Webbereitstellung
====================
durch [Jason Lee](https://github.com/jrjlee)

[PDF herunterladen](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Wenn Sie mit dem Internet Information Services (IIS) Webbereitstellungstool (Web Deploy) 2.0 oder höher arbeiten, stehen drei wichtigsten Ansätze, die Sie zum Abrufen Ihrer App Webanwendungen auf einem Webserver verwenden können. Sie können entweder:
> 
> - Bereitstellen die Anwendung von einem Remotestandort durch Angeben der *Webbereitstellungs-Agent-Dienst* (auch bekannt als die "remote-Agent") auf dem Zielserver.
> - Bereitstellen der Anwendung von einem Remotestandort mithilfe von Web Deploy bei Bedarf (auch bekannt als "temp Agent").
> - Bereitstellen die Anwendung von einem Remotestandort durch Angeben der *IIS Web bereitstellen Handler* auf dem Zielserver.
> - Bereitstellen der Anwendung manuell das Paket auf den Zielserver kopieren und importieren, wird über den IIS-Manager.
> 
> Wie Sie die Ziel-Webserver konfigurieren hängt von der Vorgehensweise bei der Bereitstellung, die Sie verwenden möchten. Dieses Thema hilft Ihnen bei der Entscheidung, welcher Ansatz zur Bereitstellung für Sie geeignet ist.


Diese Tabelle zeigt die wichtigsten vor- und Nachteile jeder Ansatzes Bereitstellung zusammen mit den Szenarien, die i. d. r. jeder Ansatz entsprechend.

| Ansatz | Vorteile | Nachteile | Typische Szenarien |
| --- | --- | --- | --- |
| Remote-Agenten | Es ist einfach einrichten. Es eignet sich für regelmäßige Updates auf Webanwendungen und Inhalt. | Der Benutzer muss ein Administrator auf dem Zielserver sein. Benutzer kann keine alternative Anmeldeinformationen angeben. | Entwicklungsumgebungen. Testumgebungen. |
| Temp-Agent | Es ist nicht erforderlich, Web Deploy auf dem Zielcomputer installieren. Die neueste Version von Web Deploy wird automatisch verwendet. | Der Benutzer muss ein Administrator auf dem Zielserver sein. Benutzer kann keine alternative Anmeldeinformationen angeben. | Entwicklungsumgebungen. Testumgebungen. |
| Web Deploy-Handler | Benutzer ohne Administratorrechte können Inhalte bereitstellen. Es eignet sich für regelmäßige Updates auf Webanwendungen und Inhalt. | Es ist viel komplexere einrichten. | Stagingumgebungen. Intranet-produktionsumgebungen. Gehostete Umgebungen. |
| Offline-Bereitstellung | Es ist sehr einfach einrichten. Er ist für von netzwerkisolierten Umgebungen geeignet. | Der Serveradministrator muss manuell kopieren und importieren das Paket jedem. | Internetzugriff von produktionsumgebungen. Isolierte Netzwerk. |
  

## <a name="using-the-remote-agent"></a>Verwenden den Remote-Agenten

Wenn Sie Web Deploy mit den Standardeinstellungen auf einem Zielserver installieren, wird der Webbereitstellungs-Agent-Dienst (der "remote-Agent") automatisch installiert und gestartet. Standardmäßig macht der remote-Agent einen HTTP-Endpunkt unter dieser Adresse:


[!code-console[Main](choosing-the-right-approach-to-web-deployment/samples/sample1.cmd)]


> [!NOTE]
> Ersetzen Sie [*Server*] mit den Computernamen des Webservers, eine IP-Adresse für den Webserver oder ein Hostname, auflöst auf Ihren Webserver.


Serveradministratoren können Webpakete von einem Remotestandort, wie ein Entwicklercomputer oder einem Build-Server bereitstellen, durch Angeben dieser Endpunktadresse. Angenommen Sie, dass Matt Hink bei Fabrikam, Inc. das Webanwendungsprojekt ContactManager.Mvc auf seinem Entwicklercomputer erstellt hat. Während des Erstellungsprozesses generiert eine Web-Paket zusammen mit einem *. deploy.cmd* Datei, die die Web Deploy-Befehle enthält, die zum Installieren des Pakets erforderlich sind. Wenn Matt ein Serveradministrator auf dem Server TESTWEB1 ist, kann er die Webanwendung mit dem Test-Webserver bereit, durch Ausführen dieses Befehls auf einem Entwicklercomputer:


[!code-console[Main](choosing-the-right-approach-to-web-deployment/samples/sample2.cmd)]


Tatsächliche tatsächlich kann die Web Deploy ausführbare Datei die Endpunktadresse des remote-Agents ableiten, wenn Sie den Namen des Computers angeben, damit Matt nur Folgendes eingeben muss:


[!code-console[Main](choosing-the-right-approach-to-web-deployment/samples/sample3.cmd)]


> [!NOTE]
> Weitere Informationen zu Web Deploy Befehlszeilensyntax und *. deploy.cmd* finden Sie unter [Vorgehensweise: Installieren Sie eine Bereitstellung Paket mithilfe der Datei deploy.cmd](https://msdn.microsoft.com/library/ff356104.aspx).


Der remote-Agent bietet eine einfache Methode zum Bereitstellen von Inhalt von einem Remotestandort aus, und dieser Ansatz kann umgesetzt, problemlos mit nur einem Klick oder automatisierte Bereitstellung. Allerdings muss der Benutzer, der Bereitstellungsbefehl ausführt, auch ein Domänenadministrator oder ein Mitglied der lokalen Administratorgruppe auf dem Zielserver. Darüber hinaus unterstützt der remote-Agenten Standardauthentifizierung nicht damit alternative Anmeldeinformationen in der Befehlszeile übergeben werden kann.

Der remote-Agent stellt hilfreich bei der Bereitstellung in Entwicklungs- oder Testszenarien, in denen es ist nicht ungewöhnlich, dass Entwickler Hauptadministrator Kontrolle über eine Test-Server-Umgebung, und Anwendungen werden in der Regel neu erstellt und erneuten sehr häufig. Dieser Ansatz ist jedoch in der Regel weniger hinsichtlich Staging-oder produktionsumgebung.

Eine End-to-End-Beispiel für ein Szenario, bei dem den remote-Agent-Ansatz verwendet, finden Sie unter [Szenario: Konfigurieren einer Testumgebung für die Bereitstellung von](scenario-configuring-a-test-environment-for-web-deployment.md).

## <a name="using-the-temp-agent"></a>Mithilfe des Temp-Agents

Der temporäre Agent Ansatz für die Bereitstellung ähnelt der remote-Agent-Ansatz. Jedoch im Gegensatz zu den Ansatz der remote-Agenten, müssen Sie Web Deploy auf dem Ziel-Webserver zu installieren. Wenn Sie die Bereitstellung durchführen, Web Deploy stattdessen wird eine temporäre Version der webbereitstellungs-Agent-Dienst auf dem Zielserver installiert und wird Hiermit können Sie Ihre Inhalte unter IIS bereitzustellen. Wenn die Bereitstellung abgeschlossen ist, werden alle temporäre Dateien entfernt.

Wenn Sie die temp-Agent-anbietereinstellung verwenden, fügen Sie möchten die **/g** Flag auf Ihre Bereitstellung:


[!code-console[Main](choosing-the-right-approach-to-web-deployment/samples/sample4.cmd)]


> [!NOTE]
> Sie können die temp-Agent verwenden, wenn der webbereitstellungs-Agent-Dienst auf dem Zielcomputer installiert ist, auch wenn der Dienst nicht ausgeführt wird.


Der Vorteil dieses Ansatzes ist, dass Sie keine Installationen von Web Deploy auf Ihrem Zielserver verwalten müssen. Darüber hinaus müssen Sie sicherstellen, dass die Quell- und Ziel-Computer die gleiche Version von Web Deploy ausgeführt werden. Allerdings noch dadurch erschwert dieser Ansatz principal dieselben Einschränkungen wie bei der remote-Agent-Ansatz, nämlich, dass Sie ein lokaler Administrator auf dem Zielserver sein müssen, um Inhalte bereitstellen, und nur die NTLM-Authentifizierung wird unterstützt. Die temp-Agent-Ansatz erfordert auch viel mehr Erstkonfiguration der zielumgebung verbunden.

Weitere Informationen zur Verwendung des temp-Agents finden Sie unter [Vorgehensweise: Installieren Sie eine Bereitstellung Paket mithilfe der Datei deploy.cmd](https://msdn.microsoft.com/library/ff356104.aspx) und [Web Deploy bei Bedarf](https://technet.microsoft.com/library/ee517345(WS.10).aspx).

## <a name="using-the-web-deploy-handler"></a>Verwenden von der Web Deploy-Handler

Für IIS 7 oder höher bietet Web Deploy eine alternative Bereitstellungsansatz über den IIS-Handler zum Bereitstellen von Web aus. Der Web-bereitstellen-Handler ist eng mit dem IIS-Web-Verwaltungsdienst (WMSvc), integriert, das entworfen wurde, um Benutzern das Verwalten von IIS-Websites von Remotestandorten aus ermöglichen.

Standardmäßig macht der remote-Agent einen HTTP-Endpunkt unter dieser Adresse:


[!code-console[Main](choosing-the-right-approach-to-web-deployment/samples/sample5.cmd)]


> [!NOTE]
> Ersetzen Sie [*Server*] mit den Computernamen des Webservers, eine IP-Adresse für den Webserver oder ein Hostname, auflöst auf Ihren Webserver.


Die großer Vorteil den Handler für die Web-Bereitstellung über die remote-Agent und der temp-Agent ist, dass Sie IIS so ermöglichen Nichtadministratoren zum Bereitstellen von Anwendungen und Inhalt auf bestimmte IIS-Websites konfigurieren können. Der Web-Handler bereitstellen unterstützt auch die Standardauthentifizierung, damit Sie alternative Anmeldeinformationen als Parameter in Ihre Befehle Web Deploy bereitstellen können. Die wichtigste Nachteil ist, dass der Web-Handler bereitstellen anfänglich viel schwieriger zu einrichten und konfigurieren.

Im Fall von nicht-Administratorbenutzer wird der Web-Verwaltungsdienst (WMSvc) nur den Benutzer für die Verbindung zu IIS ermöglicht die Verwendung einer Websiteebene Verbindung, statt eine Verbindung auf Serverebene. Um eine bestimmte Website zuzugreifen, können Sie eine standortspezifische Abfragezeichenfolge in der Endpunktadresse einschließen:


[!code-console[Main](choosing-the-right-approach-to-web-deployment/samples/sample6.cmd)]


Angenommen Sie, dass ein Buildprozess konfiguriert ist, um automatisch eine Webanwendung in einer Stagingumgebung nach jedem erfolgreichen Build bereitstellen. Wenn Sie den remote-Agent-Ansatz verwendet haben, müssen Sie der Build-Prozess-ID einen Administrator auf Ihrem Zielserver vornehmen. Im Gegensatz dazu kann mit dem Web bereitstellen Handler Ansatz zu einem Benutzer ohne Administratorrechte & #x 2014 führen; **FABRIKAM\stagingdeployer** in diesem Fall & #x 2014; die Berechtigung für einen bestimmten IIS-Website und während des Erstellungsprozesses kann diese Anmeldeinformationen zum Bereitstellen des Webpakets angeben.


[!code-console[Main](choosing-the-right-approach-to-web-deployment/samples/sample7.cmd)]


> [!NOTE]
> Weitere Informationen zu Befehlszeilenoperationen Web Deploy und deren Syntax finden Sie unter [bereitstellen Befehlszeile Webverweis](https://technet.microsoft.com/library/dd568991(v=ws.10).aspx). Weitere Informationen zur Verwendung der *. deploy.cmd* finden Sie unter [wie: Installieren Sie eine Bereitstellung Paket mithilfe der Datei deploy.cmd](https://msdn.microsoft.com/library/ff356104.aspx).


Der Web-bereitstellen-Handler stellt hilfreich bei der Bereitstellung in den staging-Umgebungen, gehostete Umgebungen und intranetbasierte produktionsumgebungen, in denen Remotezugriff auf dem Server verfügbar ist jedoch die Anmeldeinformationen des Administrators sind.

Eine End-to-End-Beispiel für ein Szenario, das Bereitstellen von Web-Handler-Ansatz verwendet, finden Sie unter [Szenario: Konfigurieren einer Stagingumgebung für die Bereitstellung von](scenario-configuring-a-staging-environment-for-web-deployment.md).

## <a name="using-offline-deployment"></a>Mithilfe der Offline-Bereitstellung

In einigen Fällen ist es nicht möglich oder praktikabel ist, Anwendungen und Inhalte von einem Remotestandort aus auf einer IIS-Website bereitstellen. Z. B. möglicherweise die Quell- und Ziel-Computer in isolierten Netzwerken oder Netzwerksegmenten oder Firewall-Richtlinie kann nicht remote Zugriff zulassen.

In solchen Szenarien können Sie weiterhin das Verpacken und Veröffentlichen von Funktionen des Web Deploy verwenden; Sie können nicht nur von einem Remotestandort aus verwenden. Stattdessen muss ein Administrator auf dem Zielserver kopieren Sie das Webpaket auf dem Server und über den IIS-Manager zu importieren.

![](choosing-the-right-approach-to-web-deployment/_static/image1.png)

Der offline-Bereitstellungsansatz ist in der Regel in produktionsumgebungen Internetzugriff, auf dem Server in einem Umkreisnetzwerk Verbindungen mit Computern im internen Netzwerk beschränkt ist möglicherweise nützlich.

Eine End-to-End-Beispiel für ein Szenario, bei dem die von offline-Bereitstellungsansatz verwendet, finden Sie unter [Szenario: Konfigurieren einer produktiven Umgebung für die Bereitstellung von](scenario-configuring-a-production-environment-for-web-deployment.md).

## <a name="further-reading"></a>Weiterführende Themen

Weitere Informationen zu Befehlszeilenoperationen Web Deploy und deren Syntax finden Sie unter [bereitstellen Befehlszeile Webverweis](https://technet.microsoft.com/library/dd568991(v=ws.10).aspx). Weitere Informationen zur Verwendung der *. deploy.cmd* finden Sie unter [wie: Installieren Sie eine Bereitstellung Paket mithilfe der Datei deploy.cmd](https://msdn.microsoft.com/library/ff356104.aspx).

Allgemeineren Leitfaden für die verschiedenen Möglichkeiten, in dem Sie Webpaketen von einem Remotecomputer bereitstellen können, finden Sie unter [mithilfe von Web Deploy über eine Remoteverbindung](https://technet.microsoft.com/library/ee461175(WS.10).aspx). Weitere Informationen zur Verwendung von Web Deploy bei Bedarf finden Sie unter [Web Deploy bei Bedarf](https://technet.microsoft.com/library/ee517345(WS.10).aspx).

>[!div class="step-by-step"]
[Zurück](configuring-server-environments-for-web-deployment.md)
[Weiter](scenario-configuring-a-test-environment-for-web-deployment.md)
