---
uid: signalr/overview/older-versions/scaleout-with-redis
title: SignalR mit horizontaler Skalierung mit Redis (SignalR 1.x) | Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/01/2013
ms.topic: article
ms.assetid: 6abecf80-8ffa-41ba-b0d9-1d9edbe7687b
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/older-versions/scaleout-with-redis
msc.type: authoredcontent
ms.openlocfilehash: 8376c6537d693841a621158358cc8f69cda0a1d6
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/24/2018
ms.locfileid: "28035730"
---
<a name="signalr-scaleout-with-redis-signalr-1x"></a>SignalR mit horizontaler Skalierung mit Redis (SignalR 1.x)
====================
durch [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)

In diesem Lernprogramm verwenden Sie [Redis](http://redis.io/) zum Verteilen von Nachrichten auf einer SignalR-Anwendung, die in zwei separaten IIS-Instanzen bereitgestellt wird.

Redis ist ein in-Memory-Schlüssel / Wert-Speicher. Außerdem wird ein Nachrichtensystem mit einem Veröffentlichen/Abonnieren-Modell unterstützt. Der Rückwand SignalR Redis verwendet Pub/Sub-das Feature zum Weiterleiten von Nachrichten an andere Server.

![](scaleout-with-redis/_static/image1.png)

Für dieses Lernprogramm verwenden Sie drei Server:

- Zwei Server unter Windows, die Sie zum Bereitstellen einer SignalR-Anwendung verwendet werden.
- Ein Server mit Linux, die Sie zum Ausführen von Redis verwendet werden. Die Screenshots in diesem Lernprogramm verwendet ich Ubuntu 12.04 TLS.

Wenn Sie nicht über die drei physischen Servern für die Verwendung verfügen, können Sie virtuelle Computer auf Hyper-V erstellen. Eine weitere Option ist für das Erstellen von virtuellen Computern in Azure.

Obwohl dieses Lernprogramm die offizielle Redis-Implementierung verwendet wird, es gibt auch eine [Windows Port von Redis](https://github.com/MSOpenTech/redis) aus MSOpenTech. Einrichtung und Konfiguration voneinander abweichen, aber ansonsten die Schritte sind identisch.

> [!NOTE] 
> 
> SignalR mit horizontaler Skalierung mit Redis unterstützt Redis-Cluster nicht.


## <a name="overview"></a>Übersicht

Bevor wir auf die ausführliches Lernprogramm erhalten, ist hier ein schnellen Überblick darüber, was Sie tun können.

1. Installieren Sie Redis, und starten Sie den Redis-Server.
2. Ihre Anwendung diese NuGet-Pakete hinzugefügt werden: 

    - [Microsoft.AspNet.SignalR](http://nuget.org/packages/Microsoft.AspNet.SignalR)
    - [Microsoft.AspNet.SignalR.Redis](http://nuget.org/packages/Microsoft.AspNet.SignalR.Redis)
3. Erstellen einer SignalR-Anwendung.
4. Fügen Sie den folgenden Code, um "Global.asax" So konfigurieren Sie die Backplane: 

    [!code-csharp[Main](scaleout-with-redis/samples/sample1.cs)]

## <a name="ubuntu-on-hyper-v"></a>Ubuntu in Hyper-V

Verwenden die Windows Hyper-V, können Sie problemlos eine Ubuntu VM unter Windows Server erstellen.

Herunterladen der Ubuntu-ISO-Datei von [http://www.ubuntu.com](http://www.ubuntu.com/).

Fügen Sie einen neuen virtuellen Computer in Hyper-V hinzu. In der **virtuelle Festplatte verbinden** Schritt wählen **erstellen Sie eine virtuelle Festplatte**.

![](scaleout-with-redis/_static/image2.png)

In der **Installationsoptionen** Schritt wählen **Imagedatei (.iso)**, klicken Sie auf **Durchsuchen**, und navigieren Sie zur Installation Ubuntu ISO.

![](scaleout-with-redis/_static/image3.png)

## <a name="install-redis"></a>Installieren von Redis

Führen Sie die Schritte unter [http://redis.io/download](http://redis.io/download) herunterladen und Redis erstellen.

[!code-console[Main](scaleout-with-redis/samples/sample2.cmd)]

Dies erstellt die Redis-Binärdateien die `src` Verzeichnis.

Standardmäßig ist Redis kein Kennwort erforderlich. Bearbeiten, um ein Kennwort festzulegen, die `redis.conf` Datei, die sich im Stammverzeichnis des Quellcodes befindet. (Stellen Sie eine Sicherungskopie der Datei aus, bevor Sie diese bearbeiten,!) Fügen Sie die folgende Anweisung zum `redis.conf`:

[!code-powershell[Main](scaleout-with-redis/samples/sample3.ps1)]

Starten Sie jetzt den Redis-Server:

[!code-css[Main](scaleout-with-redis/samples/sample4.css)]

![](scaleout-with-redis/_static/image4.png)

Geöffneten Port 6379, also den Standardport, der Redis lauscht. (Sie können die Portnummer in der Konfigurationsdatei ändern.)

## <a name="create-the-signalr-application"></a>Die SignalR-Anwendung erstellen

Erstellen einer SignalR-Anwendung entweder mit diesen Lernprogrammen:

- [Erste Schritte mit SignalR](../getting-started/tutorial-getting-started-with-signalr.md)
- [Erste Schritte mit SignalR und MVC 4](tutorial-getting-started-with-signalr-and-mvc-4.md)

Ändern Sie als Nächstes die chatanwendung zur Unterstützung von Warteschlangen für horizontale Skalierung mit Redis. Fügen Sie zuerst das SignalR.Redis NuGet-Paket zum Projekt hinzu. In Visual Studio aus der **Tools** klicken Sie im Menü **Bibliothekspaket-Manager**, und wählen Sie dann **Package Manager Console**. Geben Sie im Fenster Paket-Manager-Konsole den folgenden Befehl aus:

[!code-powershell[Main](scaleout-with-redis/samples/sample5.ps1)]

Als Nächstes öffnen Sie die Datei "Global.asax". Fügen Sie folgenden Code, der **Anwendung\_starten** Methode:

[!code-csharp[Main](scaleout-with-redis/samples/sample6.cs)]

- "Server" ist der Name des Servers, der Redis ausgeführt wird.
- *Port* ist die Nummer des Ports
- "Kennwort" ist das Kennwort, das Sie in der Datei redis.conf definiert.
- "AppName" ist eine Zeichenfolge. SignalR erstellt einen Redis Pub/Sub-Kanal mit diesem Namen.

Zum Beispiel:

[!code-csharp[Main](scaleout-with-redis/samples/sample7.cs)]

## <a name="deploy-and-run-the-application"></a>Bereitstellen und Ausführen der Anwendung

Vorbereiten der Windows Server-Instanzen, die SignalR-Anwendung bereitstellen.

Fügen Sie die IIS-Rolle hinzu. Schließen Sie "Anwendungsentwicklung"-Funktionen, einschließlich des WebSocket-Protokolls.

![](scaleout-with-redis/_static/image5.png)

Außerdem enthalten Sie den Verwaltungsdienst (unter "Verwaltungstools" aufgeführt).

![](scaleout-with-redis/_static/image6.png)

**Installieren Sie Web Deploy 3.0.** Wenn Sie die IIS-Manager ausführen, werden Sie zum Installieren von Microsoft-Webplattform aufgefordert, oder Sie können [Herunterladen der Intstaller](https://go.microsoft.com/fwlink/?LinkId=255386). Klicken Sie in der Webplattform-Installer Web Deploy suchen Sie, und installieren Sie Web Deploy 3.0

![](scaleout-with-redis/_static/image7.png)

Überprüfen Sie, dass die Web-Management-Dienst ausgeführt wird. Wenn dies nicht der Fall ist, starten Sie den Dienst. (Wenn Webverwaltungsdienst in der Liste der Windows-Dienste nicht angezeigt wird, stellen Sie sicher, dass Sie den Management-Dienst installiert, wenn Sie die IIS-Rolle hinzugefügt.)

Standardmäßig lauscht der Webverwaltungsdienst an TCP-Port 8172. Erstellen Sie in Windows-Firewall eine neue eingehende Regel zum Zulassen von TCP-Datenverkehr über Port 8172. Weitere Informationen finden Sie unter [Konfigurieren von Firewallregeln](https://technet.microsoft.com/library/dd448559(WS.10).aspx). (Wenn Sie die VMs in Azure hosten, können Sie direkt im Azure-Portal dazu. Finden Sie unter [Einrichten von Endpunkten für einen virtuellen Computer](https://azure.microsoft.com/documentation/articles/virtual-machines-set-up-endpoints/).)

Jetzt sind Sie bereit für die Bereitstellung von Visual Studio-Projekt vom Entwicklungscomputer auf den Server. Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste in der Projektmappe, und klicken Sie auf **veröffentlichen**.

Ausführlichere Dokumentation zu Web Deploy finden Sie unter [ASP.NET-Bereitstellung für Visual Studio und ASP.NET Web](../../../whitepapers/aspnet-web-deployment-content-map.md).

Wenn Sie die Anwendung mit zwei Servern bereitstellen, können jede Instanz in einem separaten Browserfenster zu öffnen und sehen, dass jeder SignalR-Nachrichten aus den anderen erhält. (Selbstverständlich würden in einer produktionsumgebung die beiden Servern hinter einem Lastenausgleich sit.)

![](scaleout-with-redis/_static/image8.png)

Wenn Sie wissen möchten, finden Sie unter den Nachrichten, die mit Redis, gesendet werden, können Sie die **Redis-Cli** Client, der mit Redis installiert wird.

[!code-xml[Main](scaleout-with-redis/samples/sample8.xml)]

![](scaleout-with-redis/_static/image9.png)
