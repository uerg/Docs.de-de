---
uid: signalr/overview/older-versions/scaleout-with-redis
title: SignalR mit horizontaler Skalierung mit Redis (SignalR 1.x) | Microsoft Docs
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/01/2013
ms.topic: article
ms.assetid: 6abecf80-8ffa-41ba-b0d9-1d9edbe7687b
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/older-versions/scaleout-with-redis
msc.type: authoredcontent
ms.openlocfilehash: c0d6fd421dad02298326d1975ae68d1e7cc78d8c
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="signalr-scaleout-with-redis-signalr-1x"></a><span data-ttu-id="d8792-102">SignalR mit horizontaler Skalierung mit Redis (SignalR 1.x)</span><span class="sxs-lookup"><span data-stu-id="d8792-102">SignalR Scaleout with Redis (SignalR 1.x)</span></span>
====================
<span data-ttu-id="d8792-103">durch [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="d8792-103">by [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)</span></span>

<span data-ttu-id="d8792-104">In diesem Lernprogramm verwenden Sie [Redis](http://redis.io/) zum Verteilen von Nachrichten auf einer SignalR-Anwendung, die in zwei separaten IIS-Instanzen bereitgestellt wird.</span><span class="sxs-lookup"><span data-stu-id="d8792-104">In this tutorial, you will use [Redis](http://redis.io/) to distribute messages across a SignalR application that is deployed on two separate IIS instances.</span></span>

<span data-ttu-id="d8792-105">Redis ist ein in-Memory-Schlüssel / Wert-Speicher.</span><span class="sxs-lookup"><span data-stu-id="d8792-105">Redis is an in-memory key-value store.</span></span> <span data-ttu-id="d8792-106">Außerdem wird ein Nachrichtensystem mit einem Veröffentlichen/Abonnieren-Modell unterstützt.</span><span class="sxs-lookup"><span data-stu-id="d8792-106">It also supports a messaging system with a publish/subscribe model.</span></span> <span data-ttu-id="d8792-107">Der Rückwand SignalR Redis verwendet Pub/Sub-das Feature zum Weiterleiten von Nachrichten an andere Server.</span><span class="sxs-lookup"><span data-stu-id="d8792-107">The SignalR Redis backplane uses the pub/sub feature to forward messages to other servers.</span></span>

![](scaleout-with-redis/_static/image1.png)

<span data-ttu-id="d8792-108">Für dieses Lernprogramm verwenden Sie drei Server:</span><span class="sxs-lookup"><span data-stu-id="d8792-108">For this tutorial, you will use three servers:</span></span>

- <span data-ttu-id="d8792-109">Zwei Server unter Windows, die Sie zum Bereitstellen einer SignalR-Anwendung verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="d8792-109">Two servers running Windows, which you will use to deploy a SignalR application.</span></span>
- <span data-ttu-id="d8792-110">Ein Server mit Linux, die Sie zum Ausführen von Redis verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="d8792-110">One server running Linux, which you will use to run Redis.</span></span> <span data-ttu-id="d8792-111">Die Screenshots in diesem Lernprogramm verwendet ich Ubuntu 12.04 TLS.</span><span class="sxs-lookup"><span data-stu-id="d8792-111">For the screenshots in this tutorial, I used Ubuntu 12.04 TLS.</span></span>

<span data-ttu-id="d8792-112">Wenn Sie nicht über die drei physischen Servern für die Verwendung verfügen, können Sie virtuelle Computer auf Hyper-V erstellen.</span><span class="sxs-lookup"><span data-stu-id="d8792-112">If you don't have three physical servers to use, you can create VMs on Hyper-V.</span></span> <span data-ttu-id="d8792-113">Eine weitere Option ist für das Erstellen von virtuellen Computern in Azure.</span><span class="sxs-lookup"><span data-stu-id="d8792-113">Another option is to create VMs on Azure.</span></span>

<span data-ttu-id="d8792-114">Obwohl dieses Lernprogramm die offizielle Redis-Implementierung verwendet wird, es gibt auch eine [Windows Port von Redis](https://github.com/MSOpenTech/redis) aus MSOpenTech.</span><span class="sxs-lookup"><span data-stu-id="d8792-114">Although this tutorial uses the official Redis implementation, there is also a [Windows port of Redis](https://github.com/MSOpenTech/redis) from MSOpenTech.</span></span> <span data-ttu-id="d8792-115">Einrichtung und Konfiguration voneinander abweichen, aber ansonsten die Schritte sind identisch.</span><span class="sxs-lookup"><span data-stu-id="d8792-115">Setup and configuration are different, but otherwise the steps are the same.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="d8792-116">SignalR mit horizontaler Skalierung mit Redis unterstützt Redis-Cluster nicht.</span><span class="sxs-lookup"><span data-stu-id="d8792-116">SignalR scaleout with Redis does not support Redis clusters.</span></span>


## <a name="overview"></a><span data-ttu-id="d8792-117">Übersicht</span><span class="sxs-lookup"><span data-stu-id="d8792-117">Overview</span></span>

<span data-ttu-id="d8792-118">Bevor wir auf die ausführliches Lernprogramm erhalten, ist hier ein schnellen Überblick darüber, was Sie tun können.</span><span class="sxs-lookup"><span data-stu-id="d8792-118">Before we get to the detailed tutorial, here is a quick overview of what you will do.</span></span>

1. <span data-ttu-id="d8792-119">Installieren Sie Redis, und starten Sie den Redis-Server.</span><span class="sxs-lookup"><span data-stu-id="d8792-119">Install Redis and start the Redis server.</span></span>
2. <span data-ttu-id="d8792-120">Ihre Anwendung diese NuGet-Pakete hinzugefügt werden:</span><span class="sxs-lookup"><span data-stu-id="d8792-120">Add these NuGet packages to your application:</span></span> 

    - [<span data-ttu-id="d8792-121">Microsoft.AspNet.SignalR</span><span class="sxs-lookup"><span data-stu-id="d8792-121">Microsoft.AspNet.SignalR</span></span>](http://nuget.org/packages/Microsoft.AspNet.SignalR)
    - [<span data-ttu-id="d8792-122">Microsoft.AspNet.SignalR.Redis</span><span class="sxs-lookup"><span data-stu-id="d8792-122">Microsoft.AspNet.SignalR.Redis</span></span>](http://nuget.org/packages/Microsoft.AspNet.SignalR.Redis)
3. <span data-ttu-id="d8792-123">Erstellen einer SignalR-Anwendung.</span><span class="sxs-lookup"><span data-stu-id="d8792-123">Create a SignalR application.</span></span>
4. <span data-ttu-id="d8792-124">Fügen Sie den folgenden Code, um "Global.asax" So konfigurieren Sie die Backplane:</span><span class="sxs-lookup"><span data-stu-id="d8792-124">Add the following code to Global.asax to configure the backplane:</span></span> 

    [!code-csharp[Main](scaleout-with-redis/samples/sample1.cs)]

## <a name="ubuntu-on-hyper-v"></a><span data-ttu-id="d8792-125">Ubuntu in Hyper-V</span><span class="sxs-lookup"><span data-stu-id="d8792-125">Ubuntu on Hyper-V</span></span>

<span data-ttu-id="d8792-126">Verwenden die Windows Hyper-V, können Sie problemlos eine Ubuntu VM unter Windows Server erstellen.</span><span class="sxs-lookup"><span data-stu-id="d8792-126">Using Windows Hyper-V, you can easily create an Ubuntu VM on Windows Server.</span></span>

<span data-ttu-id="d8792-127">Herunterladen der Ubuntu-ISO-Datei von [http://www.ubuntu.com](http://www.ubuntu.com/).</span><span class="sxs-lookup"><span data-stu-id="d8792-127">Download the Ubuntu ISO from [http://www.ubuntu.com](http://www.ubuntu.com/).</span></span>

<span data-ttu-id="d8792-128">Fügen Sie einen neuen virtuellen Computer in Hyper-V hinzu.</span><span class="sxs-lookup"><span data-stu-id="d8792-128">In Hyper-V, add a new VM.</span></span> <span data-ttu-id="d8792-129">In der **virtuelle Festplatte verbinden** Schritt wählen **erstellen Sie eine virtuelle Festplatte**.</span><span class="sxs-lookup"><span data-stu-id="d8792-129">In the **Connect Virtual Hard Disk** step, select **Create a virtual hard disk**.</span></span>

![](scaleout-with-redis/_static/image2.png)

<span data-ttu-id="d8792-130">In der **Installationsoptionen** Schritt wählen **Imagedatei (.iso)**, klicken Sie auf **Durchsuchen**, und navigieren Sie zur Installation Ubuntu ISO.</span><span class="sxs-lookup"><span data-stu-id="d8792-130">In the **Installation Options** step, select **Image file (.iso)**, click **Browse**, and browse to the Ubuntu installation ISO.</span></span>

![](scaleout-with-redis/_static/image3.png)

## <a name="install-redis"></a><span data-ttu-id="d8792-131">Installieren von Redis</span><span class="sxs-lookup"><span data-stu-id="d8792-131">Install Redis</span></span>

<span data-ttu-id="d8792-132">Führen Sie die Schritte unter [http://redis.io/download](http://redis.io/download) herunterladen und Redis erstellen.</span><span class="sxs-lookup"><span data-stu-id="d8792-132">Follow the steps at [http://redis.io/download](http://redis.io/download) to download and build Redis.</span></span>

[!code-console[Main](scaleout-with-redis/samples/sample2.cmd)]

<span data-ttu-id="d8792-133">Dies erstellt die Redis-Binärdateien die `src` Verzeichnis.</span><span class="sxs-lookup"><span data-stu-id="d8792-133">This builds the Redis binaries in the `src` directory.</span></span>

<span data-ttu-id="d8792-134">Standardmäßig ist Redis kein Kennwort erforderlich.</span><span class="sxs-lookup"><span data-stu-id="d8792-134">By default, Redis does not require a password.</span></span> <span data-ttu-id="d8792-135">Bearbeiten, um ein Kennwort festzulegen, die `redis.conf` Datei, die sich im Stammverzeichnis des Quellcodes befindet.</span><span class="sxs-lookup"><span data-stu-id="d8792-135">To set a password, edit the `redis.conf` file, which is located in the root directory of the source code.</span></span> <span data-ttu-id="d8792-136">(Stellen Sie eine Sicherungskopie der Datei aus, bevor Sie diese bearbeiten,!) Fügen Sie die folgende Anweisung zum `redis.conf`:</span><span class="sxs-lookup"><span data-stu-id="d8792-136">(Make a backup copy of the file before you edit it!) Add the following directive to `redis.conf`:</span></span>

[!code-powershell[Main](scaleout-with-redis/samples/sample3.ps1)]

<span data-ttu-id="d8792-137">Starten Sie jetzt den Redis-Server:</span><span class="sxs-lookup"><span data-stu-id="d8792-137">Now start the Redis server:</span></span>

[!code-css[Main](scaleout-with-redis/samples/sample4.css)]

![](scaleout-with-redis/_static/image4.png)

<span data-ttu-id="d8792-138">Geöffneten Port 6379, also den Standardport, der Redis lauscht.</span><span class="sxs-lookup"><span data-stu-id="d8792-138">Open port 6379, which is the default port that Redis listens on.</span></span> <span data-ttu-id="d8792-139">(Sie können die Portnummer in der Konfigurationsdatei ändern.)</span><span class="sxs-lookup"><span data-stu-id="d8792-139">(You can change the port number in the configuration file.)</span></span>

## <a name="create-the-signalr-application"></a><span data-ttu-id="d8792-140">Die SignalR-Anwendung erstellen</span><span class="sxs-lookup"><span data-stu-id="d8792-140">Create the SignalR Application</span></span>

<span data-ttu-id="d8792-141">Erstellen einer SignalR-Anwendung entweder mit diesen Lernprogrammen:</span><span class="sxs-lookup"><span data-stu-id="d8792-141">Create a SignalR application by following either of these tutorials:</span></span>

- [<span data-ttu-id="d8792-142">Erste Schritte mit SignalR</span><span class="sxs-lookup"><span data-stu-id="d8792-142">Getting Started with SignalR</span></span>](../getting-started/tutorial-getting-started-with-signalr.md)
- [<span data-ttu-id="d8792-143">Erste Schritte mit SignalR und MVC 4</span><span class="sxs-lookup"><span data-stu-id="d8792-143">Getting Started with SignalR and MVC 4</span></span>](tutorial-getting-started-with-signalr-and-mvc-4.md)

<span data-ttu-id="d8792-144">Ändern Sie als Nächstes die chatanwendung zur Unterstützung von Warteschlangen für horizontale Skalierung mit Redis.</span><span class="sxs-lookup"><span data-stu-id="d8792-144">Next, we'll modify the chat application to support scaleout with Redis.</span></span> <span data-ttu-id="d8792-145">Fügen Sie zuerst das SignalR.Redis NuGet-Paket zum Projekt hinzu.</span><span class="sxs-lookup"><span data-stu-id="d8792-145">First, add the SignalR.Redis NuGet package to your project.</span></span> <span data-ttu-id="d8792-146">In Visual Studio aus der **Tools** klicken Sie im Menü **Bibliothekspaket-Manager**, und wählen Sie dann **Package Manager Console**.</span><span class="sxs-lookup"><span data-stu-id="d8792-146">In Visual Studio, from the **Tools** menu, select **Library Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="d8792-147">Geben Sie im Fenster Paket-Manager-Konsole den folgenden Befehl aus:</span><span class="sxs-lookup"><span data-stu-id="d8792-147">In the Package Manager Console window, enter the following command:</span></span>

[!code-powershell[Main](scaleout-with-redis/samples/sample5.ps1)]

<span data-ttu-id="d8792-148">Als Nächstes öffnen Sie die Datei "Global.asax".</span><span class="sxs-lookup"><span data-stu-id="d8792-148">Next, open the Global.asax file.</span></span> <span data-ttu-id="d8792-149">Fügen Sie folgenden Code, der **Anwendung\_starten** Methode:</span><span class="sxs-lookup"><span data-stu-id="d8792-149">Add the following code to the **Application\_Start** method:</span></span>

[!code-csharp[Main](scaleout-with-redis/samples/sample6.cs)]

- <span data-ttu-id="d8792-150">"Server" ist der Name des Servers, der Redis ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="d8792-150">"server" is the name of the server that is running Redis.</span></span>
- <span data-ttu-id="d8792-151">*Port* ist die Nummer des Ports</span><span class="sxs-lookup"><span data-stu-id="d8792-151">*port* is the port number</span></span>
- <span data-ttu-id="d8792-152">"Kennwort" ist das Kennwort, das Sie in der Datei redis.conf definiert.</span><span class="sxs-lookup"><span data-stu-id="d8792-152">"password" is the password that you defined in the redis.conf file.</span></span>
- <span data-ttu-id="d8792-153">"AppName" ist eine Zeichenfolge.</span><span class="sxs-lookup"><span data-stu-id="d8792-153">"AppName" is any string.</span></span> <span data-ttu-id="d8792-154">SignalR erstellt einen Redis Pub/Sub-Kanal mit diesem Namen.</span><span class="sxs-lookup"><span data-stu-id="d8792-154">SignalR creates a Redis pub/sub channel with this name.</span></span>

<span data-ttu-id="d8792-155">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="d8792-155">For example:</span></span>

[!code-csharp[Main](scaleout-with-redis/samples/sample7.cs)]

## <a name="deploy-and-run-the-application"></a><span data-ttu-id="d8792-156">Bereitstellen und Ausführen der Anwendung</span><span class="sxs-lookup"><span data-stu-id="d8792-156">Deploy and Run the Application</span></span>

<span data-ttu-id="d8792-157">Vorbereiten der Windows Server-Instanzen, die SignalR-Anwendung bereitstellen.</span><span class="sxs-lookup"><span data-stu-id="d8792-157">Prepare your Windows Server instances to deploy the SignalR application.</span></span>

<span data-ttu-id="d8792-158">Fügen Sie die IIS-Rolle hinzu.</span><span class="sxs-lookup"><span data-stu-id="d8792-158">Add the IIS role.</span></span> <span data-ttu-id="d8792-159">Schließen Sie "Anwendungsentwicklung"-Funktionen, einschließlich des WebSocket-Protokolls.</span><span class="sxs-lookup"><span data-stu-id="d8792-159">Include "Application Development" features, including the WebSocket Protocol.</span></span>

![](scaleout-with-redis/_static/image5.png)

<span data-ttu-id="d8792-160">Außerdem enthalten Sie den Verwaltungsdienst (unter "Verwaltungstools" aufgeführt).</span><span class="sxs-lookup"><span data-stu-id="d8792-160">Also include the Management Service (listed under "Management Tools").</span></span>

![](scaleout-with-redis/_static/image6.png)

<span data-ttu-id="d8792-161">**Installieren Sie Web Deploy 3.0.**</span><span class="sxs-lookup"><span data-stu-id="d8792-161">**Install Web Deploy 3.0.**</span></span> <span data-ttu-id="d8792-162">Wenn Sie die IIS-Manager ausführen, werden Sie zum Installieren von Microsoft-Webplattform aufgefordert, oder Sie können [Herunterladen der Intstaller](https://go.microsoft.com/fwlink/?LinkId=255386).</span><span class="sxs-lookup"><span data-stu-id="d8792-162">When you run IIS Manager, it will prompt you to install Microsoft Web Platform, or you can [download the intstaller](https://go.microsoft.com/fwlink/?LinkId=255386).</span></span> <span data-ttu-id="d8792-163">Klicken Sie in der Webplattform-Installer Web Deploy suchen Sie, und installieren Sie Web Deploy 3.0</span><span class="sxs-lookup"><span data-stu-id="d8792-163">In the Platform Installer, search for Web Deploy and install Web Deploy 3.0</span></span>

![](scaleout-with-redis/_static/image7.png)

<span data-ttu-id="d8792-164">Überprüfen Sie, dass die Web-Management-Dienst ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="d8792-164">Check that the Web Management Service is running.</span></span> <span data-ttu-id="d8792-165">Wenn dies nicht der Fall ist, starten Sie den Dienst.</span><span class="sxs-lookup"><span data-stu-id="d8792-165">If not, start the service.</span></span> <span data-ttu-id="d8792-166">(Wenn Webverwaltungsdienst in der Liste der Windows-Dienste nicht angezeigt wird, stellen Sie sicher, dass Sie den Management-Dienst installiert, wenn Sie die IIS-Rolle hinzugefügt.)</span><span class="sxs-lookup"><span data-stu-id="d8792-166">(If you don't see Web Management Service in the list of Windows services, make sure that you installed the Management Service when you added the IIS role.)</span></span>

<span data-ttu-id="d8792-167">Standardmäßig lauscht der Webverwaltungsdienst an TCP-Port 8172.</span><span class="sxs-lookup"><span data-stu-id="d8792-167">By default, the Web Management Service listens on TCP port 8172.</span></span> <span data-ttu-id="d8792-168">Erstellen Sie in Windows-Firewall eine neue eingehende Regel zum Zulassen von TCP-Datenverkehr über Port 8172.</span><span class="sxs-lookup"><span data-stu-id="d8792-168">In Windows Firewall, create a new inbound rule to allow TCP traffic on port 8172.</span></span> <span data-ttu-id="d8792-169">Weitere Informationen finden Sie unter [Konfigurieren von Firewallregeln](https://technet.microsoft.com/en-us/library/dd448559(WS.10).aspx).</span><span class="sxs-lookup"><span data-stu-id="d8792-169">For more information, see [Configuring Firewall Rules](https://technet.microsoft.com/en-us/library/dd448559(WS.10).aspx).</span></span> <span data-ttu-id="d8792-170">(Wenn Sie die VMs in Azure hosten, können Sie direkt im Azure-Portal dazu.</span><span class="sxs-lookup"><span data-stu-id="d8792-170">(If you are hosting the VMs on Azure, you can do this directly in the Azure portal.</span></span> <span data-ttu-id="d8792-171">Finden Sie unter [Einrichten von Endpunkten für einen virtuellen Computer](https://azure.microsoft.com/en-us/documentation/articles/virtual-machines-set-up-endpoints/).)</span><span class="sxs-lookup"><span data-stu-id="d8792-171">See [How to Set Up Endpoints to a Virtual Machine](https://azure.microsoft.com/en-us/documentation/articles/virtual-machines-set-up-endpoints/).)</span></span>

<span data-ttu-id="d8792-172">Jetzt sind Sie bereit für die Bereitstellung von Visual Studio-Projekt vom Entwicklungscomputer auf den Server.</span><span class="sxs-lookup"><span data-stu-id="d8792-172">Now you are ready to deploy the Visual Studio project from your development machine to the server.</span></span> <span data-ttu-id="d8792-173">Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste in der Projektmappe, und klicken Sie auf **veröffentlichen**.</span><span class="sxs-lookup"><span data-stu-id="d8792-173">In Solution Explorer, right-click the solution and click **Publish**.</span></span>

<span data-ttu-id="d8792-174">Ausführlichere Dokumentation zu Web Deploy finden Sie unter [ASP.NET-Bereitstellung für Visual Studio und ASP.NET Web](../../../whitepapers/aspnet-web-deployment-content-map.md).</span><span class="sxs-lookup"><span data-stu-id="d8792-174">For more detailed documentation about web deployment, see [Web Deployment Content Map for Visual Studio and ASP.NET](../../../whitepapers/aspnet-web-deployment-content-map.md).</span></span>

<span data-ttu-id="d8792-175">Wenn Sie die Anwendung mit zwei Servern bereitstellen, können jede Instanz in einem separaten Browserfenster zu öffnen und sehen, dass jeder SignalR-Nachrichten aus den anderen erhält.</span><span class="sxs-lookup"><span data-stu-id="d8792-175">If you deploy the application to two servers, you can open each instance in a separate browser window and see that they each receive SignalR messages from the other.</span></span> <span data-ttu-id="d8792-176">(Selbstverständlich würden in einer produktionsumgebung die beiden Servern hinter einem Lastenausgleich sit.)</span><span class="sxs-lookup"><span data-stu-id="d8792-176">(Of course, in a production environment, the two servers would sit behind a load balancer.)</span></span>

![](scaleout-with-redis/_static/image8.png)

<span data-ttu-id="d8792-177">Wenn Sie wissen möchten, finden Sie unter den Nachrichten, die mit Redis, gesendet werden, können Sie die **Redis-Cli** Client, der mit Redis installiert wird.</span><span class="sxs-lookup"><span data-stu-id="d8792-177">If you're curious to see the messages that are sent to Redis, you can use the **redis-cli** client, which installs with Redis.</span></span>

[!code-xml[Main](scaleout-with-redis/samples/sample8.xml)]

![](scaleout-with-redis/_static/image9.png)
