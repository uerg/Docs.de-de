---
uid: signalr/overview/performance/scaleout-with-redis
title: SignalR mit horizontaler Skalierung mit Redis | Microsoft Docs
author: MikeWasson
description: "Versionen der Software verwendete in diesem Thema Visual Studio 2013 .NET 4.5 SignalR Version 2-Vorgängerversionen des in diesem Thema Informationen zu früheren Versionen von..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2014
ms.topic: article
ms.assetid: 6ecd08c1-e364-4cd7-ad4c-806521911585
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/performance/scaleout-with-redis
msc.type: authoredcontent
ms.openlocfilehash: 2ef161f35e69ef4a754d2740199166ee48c3fbab
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/24/2018
---
<a name="signalr-scaleout-with-redis"></a><span data-ttu-id="99084-103">SignalR mit horizontaler Skalierung mit Redis</span><span class="sxs-lookup"><span data-stu-id="99084-103">SignalR Scaleout with Redis</span></span>
====================
<span data-ttu-id="99084-104">durch [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="99084-104">by [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)</span></span>

> ## <a name="software-versions-used-in-this-topic"></a><span data-ttu-id="99084-105">In diesem Thema verwendeten Versionen der Software</span><span class="sxs-lookup"><span data-stu-id="99084-105">Software versions used in this topic</span></span>
> 
> 
> - [<span data-ttu-id="99084-106">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="99084-106">Visual Studio 2013</span></span>](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - <span data-ttu-id="99084-107">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="99084-107">.NET 4.5</span></span>
> - <span data-ttu-id="99084-108">SignalR Version 2</span><span class="sxs-lookup"><span data-stu-id="99084-108">SignalR version 2</span></span>
>   
> 
> 
> ## <a name="previous-versions-of-this-topic"></a><span data-ttu-id="99084-109">Frühere Versionen dieses Themas</span><span class="sxs-lookup"><span data-stu-id="99084-109">Previous versions of this topic</span></span>
> 
> <span data-ttu-id="99084-110">Informationen zu früheren Versionen von SignalR finden Sie unter [ältere Versionen von SignalR](../older-versions/index.md).</span><span class="sxs-lookup"><span data-stu-id="99084-110">For information about earlier versions of SignalR, see [SignalR Older Versions](../older-versions/index.md).</span></span>
> 
> ## <a name="questions-and-comments"></a><span data-ttu-id="99084-111">Fragen und Kommentare</span><span class="sxs-lookup"><span data-stu-id="99084-111">Questions and comments</span></span>
> 
> <span data-ttu-id="99084-112">Lassen Sie Sie Feedback auf wie in diesem Lernprogramm mögen und was wir in den Kommentaren am unteren Rand der Seite verbessern können.</span><span class="sxs-lookup"><span data-stu-id="99084-112">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="99084-113">Wenn Sie Fragen, die nicht direkt mit dem Lernprogramm verknüpft sind haben, bereitstellen können, die [ASP.NET SignalR-Forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) oder [StackOverflow.com](http://stackoverflow.com/).</span><span class="sxs-lookup"><span data-stu-id="99084-113">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>


<span data-ttu-id="99084-114">In diesem Lernprogramm verwenden Sie [Redis](http://redis.io/) zum Verteilen von Nachrichten auf einer SignalR-Anwendung, die in zwei separaten IIS-Instanzen bereitgestellt wird.</span><span class="sxs-lookup"><span data-stu-id="99084-114">In this tutorial, you will use [Redis](http://redis.io/) to distribute messages across a SignalR application that is deployed on two separate IIS instances.</span></span>

<span data-ttu-id="99084-115">Redis ist ein in-Memory-Schlüssel / Wert-Speicher.</span><span class="sxs-lookup"><span data-stu-id="99084-115">Redis is an in-memory key-value store.</span></span> <span data-ttu-id="99084-116">Außerdem wird ein Nachrichtensystem mit einem Veröffentlichen/Abonnieren-Modell unterstützt.</span><span class="sxs-lookup"><span data-stu-id="99084-116">It also supports a messaging system with a publish/subscribe model.</span></span> <span data-ttu-id="99084-117">Der Rückwand SignalR Redis verwendet Pub/Sub-das Feature zum Weiterleiten von Nachrichten an andere Server.</span><span class="sxs-lookup"><span data-stu-id="99084-117">The SignalR Redis backplane uses the pub/sub feature to forward messages to other servers.</span></span>

![](scaleout-with-redis/_static/image1.png)

<span data-ttu-id="99084-118">Für dieses Lernprogramm verwenden Sie drei Server:</span><span class="sxs-lookup"><span data-stu-id="99084-118">For this tutorial, you will use three servers:</span></span>

- <span data-ttu-id="99084-119">Zwei Server unter Windows, die Sie zum Bereitstellen einer SignalR-Anwendung verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="99084-119">Two servers running Windows, which you will use to deploy a SignalR application.</span></span>
- <span data-ttu-id="99084-120">Ein Server mit Linux, die Sie zum Ausführen von Redis verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="99084-120">One server running Linux, which you will use to run Redis.</span></span> <span data-ttu-id="99084-121">Die Screenshots in diesem Lernprogramm verwendet ich Ubuntu 12.04 TLS.</span><span class="sxs-lookup"><span data-stu-id="99084-121">For the screenshots in this tutorial, I used Ubuntu 12.04 TLS.</span></span>

<span data-ttu-id="99084-122">Wenn Sie nicht über die drei physischen Servern für die Verwendung verfügen, können Sie virtuelle Computer auf Hyper-V erstellen.</span><span class="sxs-lookup"><span data-stu-id="99084-122">If you don't have three physical servers to use, you can create VMs on Hyper-V.</span></span> <span data-ttu-id="99084-123">Eine weitere Option ist für das Erstellen von virtuellen Computern in Azure.</span><span class="sxs-lookup"><span data-stu-id="99084-123">Another option is to create VMs on Azure.</span></span>

<span data-ttu-id="99084-124">Obwohl dieses Lernprogramm die offizielle Redis-Implementierung verwendet wird, es gibt auch eine [Windows Port von Redis](https://github.com/MSOpenTech/redis) aus MSOpenTech.</span><span class="sxs-lookup"><span data-stu-id="99084-124">Although this tutorial uses the official Redis implementation, there is also a [Windows port of Redis](https://github.com/MSOpenTech/redis) from MSOpenTech.</span></span> <span data-ttu-id="99084-125">Einrichtung und Konfiguration voneinander abweichen, aber ansonsten die Schritte sind identisch.</span><span class="sxs-lookup"><span data-stu-id="99084-125">Setup and configuration are different, but otherwise the steps are the same.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="99084-126">SignalR mit horizontaler Skalierung mit Redis unterstützt Redis-Cluster nicht.</span><span class="sxs-lookup"><span data-stu-id="99084-126">SignalR scaleout with Redis does not support Redis clusters.</span></span>


## <a name="overview"></a><span data-ttu-id="99084-127">Übersicht</span><span class="sxs-lookup"><span data-stu-id="99084-127">Overview</span></span>

<span data-ttu-id="99084-128">Bevor wir auf die ausführliches Lernprogramm erhalten, ist hier ein schnellen Überblick darüber, was Sie tun können.</span><span class="sxs-lookup"><span data-stu-id="99084-128">Before we get to the detailed tutorial, here is a quick overview of what you will do.</span></span>

1. <span data-ttu-id="99084-129">Installieren Sie Redis, und starten Sie den Redis-Server.</span><span class="sxs-lookup"><span data-stu-id="99084-129">Install Redis and start the Redis server.</span></span>
2. <span data-ttu-id="99084-130">Ihre Anwendung diese NuGet-Pakete hinzugefügt werden:</span><span class="sxs-lookup"><span data-stu-id="99084-130">Add these NuGet packages to your application:</span></span> 

    - [<span data-ttu-id="99084-131">Microsoft.AspNet.SignalR</span><span class="sxs-lookup"><span data-stu-id="99084-131">Microsoft.AspNet.SignalR</span></span>](http://nuget.org/packages/Microsoft.AspNet.SignalR)
    - [<span data-ttu-id="99084-132">Microsoft.AspNet.SignalR.Redis</span><span class="sxs-lookup"><span data-stu-id="99084-132">Microsoft.AspNet.SignalR.Redis</span></span>](http://nuget.org/packages/Microsoft.AspNet.SignalR.Redis)
3. <span data-ttu-id="99084-133">Erstellen einer SignalR-Anwendung.</span><span class="sxs-lookup"><span data-stu-id="99084-133">Create a SignalR application.</span></span>
4. <span data-ttu-id="99084-134">Fügen Sie den folgenden Code, um tritt an der Rückwand zu konfigurieren:</span><span class="sxs-lookup"><span data-stu-id="99084-134">Add the following code to Startup.cs to configure the backplane:</span></span> 

    [!code-csharp[Main](scaleout-with-redis/samples/sample1.cs)]

## <a name="ubuntu-on-hyper-v"></a><span data-ttu-id="99084-135">Ubuntu in Hyper-V</span><span class="sxs-lookup"><span data-stu-id="99084-135">Ubuntu on Hyper-V</span></span>

<span data-ttu-id="99084-136">Verwenden die Windows Hyper-V, können Sie problemlos eine Ubuntu VM unter Windows Server erstellen.</span><span class="sxs-lookup"><span data-stu-id="99084-136">Using Windows Hyper-V, you can easily create an Ubuntu VM on Windows Server.</span></span>

<span data-ttu-id="99084-137">Herunterladen der Ubuntu-ISO-Datei von [http://www.ubuntu.com](http://www.ubuntu.com/).</span><span class="sxs-lookup"><span data-stu-id="99084-137">Download the Ubuntu ISO from [http://www.ubuntu.com](http://www.ubuntu.com/).</span></span>

<span data-ttu-id="99084-138">Fügen Sie einen neuen virtuellen Computer in Hyper-V hinzu.</span><span class="sxs-lookup"><span data-stu-id="99084-138">In Hyper-V, add a new VM.</span></span> <span data-ttu-id="99084-139">In der **virtuelle Festplatte verbinden** Schritt wählen **erstellen Sie eine virtuelle Festplatte**.</span><span class="sxs-lookup"><span data-stu-id="99084-139">In the **Connect Virtual Hard Disk** step, select **Create a virtual hard disk**.</span></span>

![](scaleout-with-redis/_static/image2.png)

<span data-ttu-id="99084-140">In der **Installationsoptionen** Schritt wählen **Imagedatei (.iso)**, klicken Sie auf **Durchsuchen**, und navigieren Sie zur Installation Ubuntu ISO.</span><span class="sxs-lookup"><span data-stu-id="99084-140">In the **Installation Options** step, select **Image file (.iso)**, click **Browse**, and browse to the Ubuntu installation ISO.</span></span>

![](scaleout-with-redis/_static/image3.png)

## <a name="install-redis"></a><span data-ttu-id="99084-141">Installieren von Redis</span><span class="sxs-lookup"><span data-stu-id="99084-141">Install Redis</span></span>

<span data-ttu-id="99084-142">Führen Sie die Schritte unter [http://redis.io/download](http://redis.io/download) herunterladen und Redis erstellen.</span><span class="sxs-lookup"><span data-stu-id="99084-142">Follow the steps at [http://redis.io/download](http://redis.io/download) to download and build Redis.</span></span>

[!code-console[Main](scaleout-with-redis/samples/sample2.cmd)]

<span data-ttu-id="99084-143">Dies erstellt die Redis-Binärdateien die `src` Verzeichnis.</span><span class="sxs-lookup"><span data-stu-id="99084-143">This builds the Redis binaries in the `src` directory.</span></span>

<span data-ttu-id="99084-144">Standardmäßig ist Redis kein Kennwort erforderlich.</span><span class="sxs-lookup"><span data-stu-id="99084-144">By default, Redis does not require a password.</span></span> <span data-ttu-id="99084-145">Bearbeiten, um ein Kennwort festzulegen, die `redis.conf` Datei, die sich im Stammverzeichnis des Quellcodes befindet.</span><span class="sxs-lookup"><span data-stu-id="99084-145">To set a password, edit the `redis.conf` file, which is located in the root directory of the source code.</span></span> <span data-ttu-id="99084-146">(Stellen Sie eine Sicherungskopie der Datei aus, bevor Sie diese bearbeiten,!) Fügen Sie die folgende Anweisung zum `redis.conf`:</span><span class="sxs-lookup"><span data-stu-id="99084-146">(Make a backup copy of the file before you edit it!) Add the following directive to `redis.conf`:</span></span>

[!code-powershell[Main](scaleout-with-redis/samples/sample3.ps1)]

<span data-ttu-id="99084-147">Starten Sie jetzt den Redis-Server:</span><span class="sxs-lookup"><span data-stu-id="99084-147">Now start the Redis server:</span></span>

[!code-css[Main](scaleout-with-redis/samples/sample4.css)]

![](scaleout-with-redis/_static/image4.png)

<span data-ttu-id="99084-148">Geöffneten Port 6379, also den Standardport, der Redis lauscht.</span><span class="sxs-lookup"><span data-stu-id="99084-148">Open port 6379, which is the default port that Redis listens on.</span></span> <span data-ttu-id="99084-149">(Sie können die Portnummer in der Konfigurationsdatei ändern.)</span><span class="sxs-lookup"><span data-stu-id="99084-149">(You can change the port number in the configuration file.)</span></span>

## <a name="create-the-signalr-application"></a><span data-ttu-id="99084-150">Die SignalR-Anwendung erstellen</span><span class="sxs-lookup"><span data-stu-id="99084-150">Create the SignalR Application</span></span>

<span data-ttu-id="99084-151">Erstellen einer SignalR-Anwendung entweder mit diesen Lernprogrammen:</span><span class="sxs-lookup"><span data-stu-id="99084-151">Create a SignalR application by following either of these tutorials:</span></span>

- [<span data-ttu-id="99084-152">Erste Schritte mit SignalR 2.0</span><span class="sxs-lookup"><span data-stu-id="99084-152">Getting Started with SignalR 2.0</span></span>](../getting-started/tutorial-getting-started-with-signalr.md)
- [<span data-ttu-id="99084-153">Erste Schritte mit SignalR 2.0 und MVC 5</span><span class="sxs-lookup"><span data-stu-id="99084-153">Getting Started with SignalR 2.0 and MVC 5</span></span>](../getting-started/tutorial-getting-started-with-signalr-and-mvc.md)

<span data-ttu-id="99084-154">Ändern Sie als Nächstes die chatanwendung zur Unterstützung von Warteschlangen für horizontale Skalierung mit Redis.</span><span class="sxs-lookup"><span data-stu-id="99084-154">Next, we'll modify the chat application to support scaleout with Redis.</span></span> <span data-ttu-id="99084-155">Fügen Sie zuerst das SignalR.Redis NuGet-Paket zum Projekt hinzu.</span><span class="sxs-lookup"><span data-stu-id="99084-155">First, add the SignalR.Redis NuGet package to your project.</span></span> <span data-ttu-id="99084-156">In Visual Studio aus der **Tools** klicken Sie im Menü **Bibliothekspaket-Manager**, und wählen Sie dann **Package Manager Console**.</span><span class="sxs-lookup"><span data-stu-id="99084-156">In Visual Studio, from the **Tools** menu, select **Library Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="99084-157">Geben Sie im Fenster Paket-Manager-Konsole den folgenden Befehl aus:</span><span class="sxs-lookup"><span data-stu-id="99084-157">In the Package Manager Console window, enter the following command:</span></span>

[!code-powershell[Main](scaleout-with-redis/samples/sample5.ps1)]

<span data-ttu-id="99084-158">Öffnen Sie als Nächstes die Startup.cs-Datei.</span><span class="sxs-lookup"><span data-stu-id="99084-158">Next, open the Startup.cs file.</span></span> <span data-ttu-id="99084-159">Fügen Sie folgenden Code, der **Konfiguration** Methode:</span><span class="sxs-lookup"><span data-stu-id="99084-159">Add the following code to the **Configuration** method:</span></span>

[!code-csharp[Main](scaleout-with-redis/samples/sample6.cs)]

- <span data-ttu-id="99084-160">"Server" ist der Name des Servers, der Redis ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="99084-160">"server" is the name of the server that is running Redis.</span></span>
- <span data-ttu-id="99084-161">*Port* ist die Nummer des Ports</span><span class="sxs-lookup"><span data-stu-id="99084-161">*port* is the port number</span></span>
- <span data-ttu-id="99084-162">"Kennwort" ist das Kennwort, das Sie in der Datei redis.conf definiert.</span><span class="sxs-lookup"><span data-stu-id="99084-162">"password" is the password that you defined in the redis.conf file.</span></span>
- <span data-ttu-id="99084-163">"AppName" ist eine Zeichenfolge.</span><span class="sxs-lookup"><span data-stu-id="99084-163">"AppName" is any string.</span></span> <span data-ttu-id="99084-164">SignalR erstellt einen Redis Pub/Sub-Kanal mit diesem Namen.</span><span class="sxs-lookup"><span data-stu-id="99084-164">SignalR creates a Redis pub/sub channel with this name.</span></span>

<span data-ttu-id="99084-165">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="99084-165">For example:</span></span>

[!code-csharp[Main](scaleout-with-redis/samples/sample7.cs)]

## <a name="deploy-and-run-the-application"></a><span data-ttu-id="99084-166">Bereitstellen und Ausführen der Anwendung</span><span class="sxs-lookup"><span data-stu-id="99084-166">Deploy and Run the Application</span></span>

<span data-ttu-id="99084-167">Vorbereiten der Windows Server-Instanzen, die SignalR-Anwendung bereitstellen.</span><span class="sxs-lookup"><span data-stu-id="99084-167">Prepare your Windows Server instances to deploy the SignalR application.</span></span>

<span data-ttu-id="99084-168">Fügen Sie die IIS-Rolle hinzu.</span><span class="sxs-lookup"><span data-stu-id="99084-168">Add the IIS role.</span></span> <span data-ttu-id="99084-169">Schließen Sie "Anwendungsentwicklung"-Funktionen, einschließlich des WebSocket-Protokolls.</span><span class="sxs-lookup"><span data-stu-id="99084-169">Include "Application Development" features, including the WebSocket Protocol.</span></span>

![](scaleout-with-redis/_static/image5.png)

<span data-ttu-id="99084-170">Außerdem enthalten Sie den Verwaltungsdienst (unter "Verwaltungstools" aufgeführt).</span><span class="sxs-lookup"><span data-stu-id="99084-170">Also include the Management Service (listed under "Management Tools").</span></span>

![](scaleout-with-redis/_static/image6.png)

<span data-ttu-id="99084-171">**Installieren Sie Web Deploy 3.0.**</span><span class="sxs-lookup"><span data-stu-id="99084-171">**Install Web Deploy 3.0.**</span></span> <span data-ttu-id="99084-172">Wenn Sie die IIS-Manager ausführen, werden Sie zum Installieren von Microsoft-Webplattform aufgefordert, oder Sie können [Herunterladen der Intstaller](https://go.microsoft.com/fwlink/?LinkId=255386).</span><span class="sxs-lookup"><span data-stu-id="99084-172">When you run IIS Manager, it will prompt you to install Microsoft Web Platform, or you can [download the intstaller](https://go.microsoft.com/fwlink/?LinkId=255386).</span></span> <span data-ttu-id="99084-173">Klicken Sie in der Webplattform-Installer Web Deploy suchen Sie, und installieren Sie Web Deploy 3.0</span><span class="sxs-lookup"><span data-stu-id="99084-173">In the Platform Installer, search for Web Deploy and install Web Deploy 3.0</span></span>

![](scaleout-with-redis/_static/image7.png)

<span data-ttu-id="99084-174">Überprüfen Sie, dass die Web-Management-Dienst ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="99084-174">Check that the Web Management Service is running.</span></span> <span data-ttu-id="99084-175">Wenn dies nicht der Fall ist, starten Sie den Dienst.</span><span class="sxs-lookup"><span data-stu-id="99084-175">If not, start the service.</span></span> <span data-ttu-id="99084-176">(Wenn Webverwaltungsdienst in der Liste der Windows-Dienste nicht angezeigt wird, stellen Sie sicher, dass Sie den Management-Dienst installiert, wenn Sie die IIS-Rolle hinzugefügt.)</span><span class="sxs-lookup"><span data-stu-id="99084-176">(If you don't see Web Management Service in the list of Windows services, make sure that you installed the Management Service when you added the IIS role.)</span></span>

<span data-ttu-id="99084-177">Standardmäßig lauscht der Webverwaltungsdienst an TCP-Port 8172.</span><span class="sxs-lookup"><span data-stu-id="99084-177">By default, the Web Management Service listens on TCP port 8172.</span></span> <span data-ttu-id="99084-178">Erstellen Sie in Windows-Firewall eine neue eingehende Regel zum Zulassen von TCP-Datenverkehr über Port 8172.</span><span class="sxs-lookup"><span data-stu-id="99084-178">In Windows Firewall, create a new inbound rule to allow TCP traffic on port 8172.</span></span> <span data-ttu-id="99084-179">Weitere Informationen finden Sie unter [Konfigurieren von Firewallregeln](https://technet.microsoft.com/library/dd448559(WS.10).aspx).</span><span class="sxs-lookup"><span data-stu-id="99084-179">For more information, see [Configuring Firewall Rules](https://technet.microsoft.com/library/dd448559(WS.10).aspx).</span></span> <span data-ttu-id="99084-180">(Wenn Sie die VMs in Azure hosten, können Sie direkt im Azure-Portal dazu.</span><span class="sxs-lookup"><span data-stu-id="99084-180">(If you are hosting the VMs on Azure, you can do this directly in the Azure portal.</span></span> <span data-ttu-id="99084-181">Finden Sie unter [Einrichten von Endpunkten für einen virtuellen Computer](https://azure.microsoft.com/documentation/articles/virtual-machines-set-up-endpoints/).)</span><span class="sxs-lookup"><span data-stu-id="99084-181">See [How to Set Up Endpoints to a Virtual Machine](https://azure.microsoft.com/documentation/articles/virtual-machines-set-up-endpoints/).)</span></span>

<span data-ttu-id="99084-182">Jetzt sind Sie bereit für die Bereitstellung von Visual Studio-Projekt vom Entwicklungscomputer auf den Server.</span><span class="sxs-lookup"><span data-stu-id="99084-182">Now you are ready to deploy the Visual Studio project from your development machine to the server.</span></span> <span data-ttu-id="99084-183">Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste in der Projektmappe, und klicken Sie auf **veröffentlichen**.</span><span class="sxs-lookup"><span data-stu-id="99084-183">In Solution Explorer, right-click the solution and click **Publish**.</span></span>

<span data-ttu-id="99084-184">Ausführlichere Dokumentation zu Web Deploy finden Sie unter [ASP.NET-Bereitstellung für Visual Studio und ASP.NET Web](../../../whitepapers/aspnet-web-deployment-content-map.md).</span><span class="sxs-lookup"><span data-stu-id="99084-184">For more detailed documentation about web deployment, see [Web Deployment Content Map for Visual Studio and ASP.NET](../../../whitepapers/aspnet-web-deployment-content-map.md).</span></span>

<span data-ttu-id="99084-185">Wenn Sie die Anwendung mit zwei Servern bereitstellen, können jede Instanz in einem separaten Browserfenster zu öffnen und sehen, dass jeder SignalR-Nachrichten aus den anderen erhält.</span><span class="sxs-lookup"><span data-stu-id="99084-185">If you deploy the application to two servers, you can open each instance in a separate browser window and see that they each receive SignalR messages from the other.</span></span> <span data-ttu-id="99084-186">(Selbstverständlich würden in einer produktionsumgebung die beiden Servern hinter einem Lastenausgleich sit.)</span><span class="sxs-lookup"><span data-stu-id="99084-186">(Of course, in a production environment, the two servers would sit behind a load balancer.)</span></span>

![](scaleout-with-redis/_static/image8.png)

<span data-ttu-id="99084-187">Wenn Sie wissen möchten, finden Sie unter den Nachrichten, die mit Redis, gesendet werden, können Sie die **Redis-Cli** Client, der mit Redis installiert wird.</span><span class="sxs-lookup"><span data-stu-id="99084-187">If you're curious to see the messages that are sent to Redis, you can use the **redis-cli** client, which installs with Redis.</span></span>

[!code-xml[Main](scaleout-with-redis/samples/sample8.xml)]

![](scaleout-with-redis/_static/image9.png)
