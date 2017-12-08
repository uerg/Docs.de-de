---
uid: signalr/overview/deployment/using-signalr-with-azure-web-sites
title: Web-Apps in Azure App Service SignalR mit | Microsoft Docs
author: pfletcher
description: "Dieses Dokument beschreibt, wie eine SignalR-Anwendung zu konfigurieren, die auf Microsoft Azure ausgeführt wird. Versionen der Software, die im Lernprogramm verwendet werden, Visual Studio 2013 oder Vis...."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/01/2015
ms.topic: article
ms.assetid: 2a7517a0-b88c-4162-ade3-9bf6ca7062fd
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/deployment/using-signalr-with-azure-web-sites
msc.type: authoredcontent
ms.openlocfilehash: 414701159b4e1fa3da9597503b14281a1e9991de
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="using-signalr-with-web-apps-in-azure-app-service"></a>Verwenden von SignalR mit Web-Apps in Azure App Service
====================
durch [Patrick Fletcher](https://github.com/pfletcher)

> Dieses Dokument beschreibt, wie eine SignalR-Anwendung zu konfigurieren, die auf Microsoft Azure ausgeführt wird.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>In diesem Lernprogramm verwendeten Versionen der Software
> 
> 
> - [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads) oder Visual Studio 2012
> - .NET 4.5
> - SignalR Version 2
> - Azure SDK 2.3 für Visual Studio 2013 oder 2012
>   
> 
> 
> ## <a name="questions-and-comments"></a>Fragen und Kommentare
> 
> Lassen Sie Sie Feedback auf wie in diesem Lernprogramm mögen und was wir in den Kommentaren am unteren Rand der Seite verbessern können. Wenn Sie Fragen, die nicht direkt mit dem Lernprogramm verknüpft sind haben, bereitstellen können, die [ASP.NET SignalR-Forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR), [StackOverflow.com](http://stackoverflow.com/), oder die [Microsoft Azure-Foren](https://social.msdn.microsoft.com/Forums/windowsazure/en-US/home?category=windowsazureplatform).


## <a name="table-of-contents"></a>Inhaltsverzeichnis

- [Introduction (Einführung)](#introduction)
- [Bereitstellen einer SignalR-Web-App in Azure App Service](#deploying)
- [Aktivierung von WebSockets in Azure App Service](#websocket)
- [Mithilfe der Azure-Redis-Cache-Rückwandplatine](#backplane)
- [Nächste Schritte](#nextsteps)

<a id="introduction"></a>
## <a name="introduction"></a>Einführung

ASP.NET SignalR kann verwendet werden, um ein neues Maß an Interaktivität zwischen Servern und Web oder .NET Clients zu bringen. Wenn in Azure gehostet, SignalR-Anwendungen nutzen der hoch verfügbare, skalierbare und leistungsstarke Umgebung, die Ausführung in der Cloud bereitstellt.

<a id="deploying"></a>
## <a name="deploying-a-signalr-web-app-to-azure-app-service"></a>Bereitstellen einer SignalR-Web-App in Azure App Service

SignalR hinzufügen nicht bestimmten Komplikationen bis zur Bereitstellung von einer Anwendung in Azure im Vergleich zur Bereitstellung auf einem lokalen Server. Eine Anwendung, die SignalR verwendet in Azure gehostet werden kann, ohne Änderungen in der Konfiguration oder andere Einstellungen (obwohl WebSockets-Unterstützung finden Sie unter [Aktivierung von WebSockets auf Azure App Service](#websocket) unten.) In diesem Lernprogramm stellen Sie die Anwendung in der [Lernprogramm für erste Schritte](../getting-started/tutorial-getting-started-with-signalr.md) in Azure.

**Erforderliche Komponenten**

- Visual Studio 2013. Wenn Sie Visual Studio nicht haben, ist Visual Studio-2013 Express für Web in der Installation des Azure SDK enthalten.
- [Azure SDK 2.3 für Visual Studio 2013](https://go.microsoft.com/fwlink/?linkid=324322&clcid=0x409) oder [Azure SDK 2.3 für Visual Studio 2012](https://go.microsoft.com/fwlink/p/?linkid=323511).
- Um dieses Lernprogramm abzuschließen, benötigen Sie ein Azure-Abonnement. Sie können [Ihrem MSDN-abonnentenvorteile aktivieren](https://azure.microsoft.com/en-us/pricing/member-offers/msdn-benefits-details/), oder [registrieren Sie sich für ein Testabonnement](https://azure.microsoft.com/en-us/pricing/free-trial/).

### <a name="deploying-a-signalr-web-app-to-azure"></a>Bereitstellen einer SignalR-Web-app in Azure

1. Führen Sie die [Lernprogramm für erste Schritte](../getting-started/tutorial-getting-started-with-signalr.md), oder Herunterladen der fertige Projekt aus [Code Gallery](https://code.msdn.microsoft.com/SignalR-Getting-Started-b9d18aa9).
2. Wählen Sie in Visual Studio **erstellen**, **veröffentlichen SignalR Chat**.
3. Wählen Sie im Dialogfeld "Web veröffentlichen" "Windows Azure-Websites".

    ![Wählen Sie die Azure-Websites](using-signalr-with-azure-web-sites/_static/image1.png)
4. Wenn Sie Ihr Microsoft-Konto angemeldet sind, klicken Sie auf **anmelden...**  in das Dialogfeld "Wählen Sie vorhandene Web Site", und melden Sie sich.

    ![Wählen Sie die vorhandene Website](using-signalr-with-azure-web-sites/_static/image2.png)    ![Melden Sie sich bei Azure](using-signalr-with-azure-web-sites/_static/image3.png)
5. Klicken Sie im Dialogfeld "Wählen Sie vorhandene Web Site" **neu**.

    ![Neue Website](using-signalr-with-azure-web-sites/_static/image4.png)
6. Geben Sie im Dialogfeld "Website in Windows Azure erstellen" einen eindeutigen Anwendungsnamen ein. Wählen Sie in der Region-Dropdownliste die Region, die Sie am nächsten. Klicken Sie auf **Erstellen**.

    ![Erstellen Sie die Website in Azure](using-signalr-with-azure-web-sites/_static/image5.png)
7. Klicken Sie im Dialogfeld "Web veröffentlichen" auf **veröffentlichen**.

    ![Website veröffentlichen](using-signalr-with-azure-web-sites/_static/image6.png)
8. Wenn die app Veröffentlichung abgeschlossen wurde, wird die SignalR-Chat-Anwendung in Azure App Service-Web-Apps gehostet in einem Browser geöffnet.

    ![Website, die in einem Browser öffnen](using-signalr-with-azure-web-sites/_static/image7.png)

<a id="websocket"></a>
### <a name="enabling-websockets-on-azure-app-service-web-apps"></a>Aktivierung von WebSockets auf Azure App Service-Web-Apps

WebSockets muss explizit in Ihre Web-app aktiviert werden, um in einer SignalR-Anwendung verwendet werden. andernfalls, die anderen Protokolle verwendet werden (finden Sie unter [Transporte und Zugriffe](../getting-started/introduction-to-signalr.md#transports) Einzelheiten).

Um WebSockets auf Azure App Service-Web-Apps zu verwenden, aktivieren sie im Abschnitt "Konfiguration" der Web-app. Zu diesem Zweck öffnen Sie Ihre Web-app in der [Azure-Verwaltungsportal](https://manage.windowsazure.com/), und wählen Sie konfigurieren.

![Registerkarte „Konfigurieren“](using-signalr-with-azure-web-sites/_static/image8.png)

Am oberen Rand der Seite "Konfiguration" Stellen Sie sicher, dass .NET 4.5 für Ihre Web-app verwendet wird.

![.NET Framework Version 4.5-Einstellung](using-signalr-with-azure-web-sites/_static/image9.png)

Auf der Seite "Konfiguration" in der **WebSockets** wählen Sie die Einstellung **auf**.

![WebSockets-Einstellung: auf](using-signalr-with-azure-web-sites/_static/image10.png)

Wählen Sie am unteren Rand der Seite "Konfiguration", **speichern** zum Speichern der Änderungen.

![Speichern von Einstellungen](using-signalr-with-azure-web-sites/_static/image11.png)

<a id="backplane"></a>
## <a name="using-the-azure-redis-cache-backplane"></a>Mithilfe der Azure-Redis-Cache-Rückwandplatine

Wenn Sie mehrere Instanzen für Ihre Web-app verwenden, und die Benutzer dieser Instanzen müssen miteinander interagieren, (, z. B. Chat-Nachrichten, die in einer Instanz erstellt wurde mit anderen Instanzen verbundenen Benutzer erreichen können), wird die [Azure-Redis-Cache Rückwand](../performance/scaleout-with-redis.md) muss in der Anwendung implementiert werden.

<a id="nextsteps"></a>
## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu Web-Apps in Azure App Service, finden Sie unter [-Web-Apps – Übersicht](https://azure.microsoft.com/en-us/documentation/articles/app-service-web-overview/).
