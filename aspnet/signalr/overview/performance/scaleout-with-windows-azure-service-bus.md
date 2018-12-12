---
uid: signalr/overview/performance/scaleout-with-windows-azure-service-bus
title: Horizontale Skalierung in SignalR mit Azure Servicebus | Microsoft-Dokumentation
author: MikeWasson
description: Version der Software verwendet in diesem Thema Visual Studio 2013 .NET 4.5 SignalR-Version 2 frühere Versionen dieser Thema für die SignalR 1.x-Version dieses Themas...
ms.author: riande
ms.date: 06/10/2014
ms.assetid: ce1305f9-30fd-49e3-bf38-d0a78dfb06c3
msc.legacyurl: /signalr/overview/performance/scaleout-with-windows-azure-service-bus
msc.type: authoredcontent
ms.openlocfilehash: 5cdb9b5eb6d3f5ebd5c96e4b0d89926c18bddadd
ms.sourcegitcommit: 74e3be25ea37b5fc8b4b433b0b872547b4b99186
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 12/12/2018
ms.locfileid: "53287610"
---
<a name="signalr-scaleout-with-azure-service-bus"></a><span data-ttu-id="bb743-103">Horizontale Skalierung in SignalR mit Azure Servicebus</span><span class="sxs-lookup"><span data-stu-id="bb743-103">SignalR Scaleout with Azure Service Bus</span></span>
====================
<span data-ttu-id="bb743-104">durch [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="bb743-104">by [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)</span></span>

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

<span data-ttu-id="bb743-105">In diesem Tutorial stellen Sie eine SignalR-Anwendung für eine Windows Azure-Webrolle, mit der Service Bus-Rückwandplatine zum Verteilen von Nachrichten an alle Rolleninstanzen bereit.</span><span class="sxs-lookup"><span data-stu-id="bb743-105">In this tutorial, you will deploy a SignalR application to a Windows Azure Web Role, using the Service Bus backplane to distribute messages to each role instance.</span></span> <span data-ttu-id="bb743-106">(Sie können auch die Service Bus-Backplane [web-apps in Azure App Service](https://docs.microsoft.com/azure/app-service-web/).)</span><span class="sxs-lookup"><span data-stu-id="bb743-106">(You can also use the Service Bus backplane with [web apps in Azure App Service](https://docs.microsoft.com/azure/app-service-web/).)</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image1.png)

<span data-ttu-id="bb743-107">Erforderliche Komponenten:</span><span class="sxs-lookup"><span data-stu-id="bb743-107">Prerequisites:</span></span>

- <span data-ttu-id="bb743-108">Ein Windows Azure-Konto.</span><span class="sxs-lookup"><span data-stu-id="bb743-108">A Windows Azure account.</span></span>
- <span data-ttu-id="bb743-109">Die [Windows Azure SDK](https://go.microsoft.com/fwlink/?linkid=254364&amp;clcid=0x409).</span><span class="sxs-lookup"><span data-stu-id="bb743-109">The [Windows Azure SDK](https://go.microsoft.com/fwlink/?linkid=254364&amp;clcid=0x409).</span></span>
- <span data-ttu-id="bb743-110">Visual Studio 2012 oder 2013.</span><span class="sxs-lookup"><span data-stu-id="bb743-110">Visual Studio 2012 or 2013.</span></span>

<span data-ttu-id="bb743-111">Die Service Bus-Rückwandplatine ist auch mit kompatibel [Service Bus für Windows Server](https://msdn.microsoft.com/library/windowsazure/dn282144.aspx), Version 1.1.</span><span class="sxs-lookup"><span data-stu-id="bb743-111">The service bus backplane is also compatible with [Service Bus for Windows Server](https://msdn.microsoft.com/library/windowsazure/dn282144.aspx), version 1.1.</span></span> <span data-ttu-id="bb743-112">Es ist jedoch nicht kompatibel mit Version 1.0 von Service Bus für Windows Server.</span><span class="sxs-lookup"><span data-stu-id="bb743-112">However, it is not compatible with version 1.0 of Service Bus for Windows Server.</span></span>

## <a name="pricing"></a><span data-ttu-id="bb743-113">Preise</span><span class="sxs-lookup"><span data-stu-id="bb743-113">Pricing</span></span>

<span data-ttu-id="bb743-114">Die Service Bus-Rückwandplatine werden Themen zum Senden von Nachrichten verwendet.</span><span class="sxs-lookup"><span data-stu-id="bb743-114">The Service Bus backplane uses topics to send messages.</span></span> <span data-ttu-id="bb743-115">Aktuelle Preisinformationen, finden Sie unter [Service Bus](https://azure.microsoft.com/pricing/details/service-bus/).</span><span class="sxs-lookup"><span data-stu-id="bb743-115">For the latest pricing information, see [Service Bus](https://azure.microsoft.com/pricing/details/service-bus/).</span></span> <span data-ttu-id="bb743-116">Zum Zeitpunkt der Erstellung dieses Dokuments können Sie für weniger als $1 1.000.000 Nachrichten pro Monat senden.</span><span class="sxs-lookup"><span data-stu-id="bb743-116">At the time of this writing, you can send 1,000,000 messages per month for less than $1.</span></span> <span data-ttu-id="bb743-117">Der Rückwand sendet eine Service Bus-Nachricht für jeden Aufruf einer SignalR-Hub-Methode.</span><span class="sxs-lookup"><span data-stu-id="bb743-117">The backplane sends a service bus message for each invocation of a SignalR hub method.</span></span> <span data-ttu-id="bb743-118">Es gibt auch einige Steuerungsnachrichten für Verbindungen, Trennvorgänge, verknüpfen oder eine verlassen Gruppen und So weiter.</span><span class="sxs-lookup"><span data-stu-id="bb743-118">There are also some control messages for connections, disconnections, joining or leaving groups, and so forth.</span></span> <span data-ttu-id="bb743-119">In den meisten Fällen werden die meisten dem Volumen des Nachrichtenverkehrs hubmethodenaufrufe.</span><span class="sxs-lookup"><span data-stu-id="bb743-119">In most applications, the majority of the message traffic will be hub method invocations.</span></span>

## <a name="overview"></a><span data-ttu-id="bb743-120">Übersicht</span><span class="sxs-lookup"><span data-stu-id="bb743-120">Overview</span></span>

<span data-ttu-id="bb743-121">Bevor wir mit dem ausführlichen Tutorial eingehen, sieht ein schnellen Überblick darüber, welche Aktionen Sie ausgeführt werden.</span><span class="sxs-lookup"><span data-stu-id="bb743-121">Before we get to the detailed tutorial, here is a quick overview of what you will do.</span></span>

1. <span data-ttu-id="bb743-122">Verwenden Sie Windows Azure-Portal, um einen neuen Service Bus-Namespace zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="bb743-122">Use the Windows Azure portal to create a new Service Bus namespace.</span></span>
2. <span data-ttu-id="bb743-123">Fügen Sie diese NuGet-Pakete für Ihre Anwendung hinzu:</span><span class="sxs-lookup"><span data-stu-id="bb743-123">Add these NuGet packages to your application:</span></span> 

    - [<span data-ttu-id="bb743-124">Microsoft.AspNet.SignalR</span><span class="sxs-lookup"><span data-stu-id="bb743-124">Microsoft.AspNet.SignalR</span></span>](http://nuget.org/packages/Microsoft.AspNet.SignalR)
    - <span data-ttu-id="bb743-125">[Microsoft.AspNet.SignalR.ServiceBus3](https://www.nuget.org/packages/Microsoft.AspNet.SignalR.ServiceBus3) oder [Microsoft.AspNet.SignalR.ServiceBus](https://www.nuget.org/packages/Microsoft.AspNet.SignalR.ServiceBus)</span><span class="sxs-lookup"><span data-stu-id="bb743-125">[Microsoft.AspNet.SignalR.ServiceBus3](https://www.nuget.org/packages/Microsoft.AspNet.SignalR.ServiceBus3) or [Microsoft.AspNet.SignalR.ServiceBus](https://www.nuget.org/packages/Microsoft.AspNet.SignalR.ServiceBus)</span></span>
3. <span data-ttu-id="bb743-126">Erstellen einer SignalR-Anwendung.</span><span class="sxs-lookup"><span data-stu-id="bb743-126">Create a SignalR application.</span></span>
4. <span data-ttu-id="bb743-127">Fügen Sie den folgenden Code, "Startup.cs" So konfigurieren Sie die Rückwandplatine:</span><span class="sxs-lookup"><span data-stu-id="bb743-127">Add the following code to Startup.cs to configure the backplane:</span></span> 

    [!code-csharp[Main](scaleout-with-windows-azure-service-bus/samples/sample1.cs)]

<span data-ttu-id="bb743-128">Dieser Code konfiguriert die Rückwandplatine mit den Standardwerten für [TopicCount](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.servicebusscaleoutconfiguration.topiccount(v=vs.118).aspx) und [MaxQueueLength](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.messaging.scaleoutconfiguration.maxqueuelength(v=vs.118).aspx).</span><span class="sxs-lookup"><span data-stu-id="bb743-128">This code configures the backplane with the default values for [TopicCount](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.servicebusscaleoutconfiguration.topiccount(v=vs.118).aspx) and [MaxQueueLength](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.messaging.scaleoutconfiguration.maxqueuelength(v=vs.118).aspx).</span></span> <span data-ttu-id="bb743-129">Informationen zum Ändern dieser Werte finden Sie unter [SignalR-Leistung: Horizontale Skalierung Metriken](signalr-performance.md#scaleout_metrics).</span><span class="sxs-lookup"><span data-stu-id="bb743-129">For information on changing these values, see [SignalR Performance: Scaleout Metrics](signalr-performance.md#scaleout_metrics).</span></span>

<span data-ttu-id="bb743-130">Wählen Sie für jede Anwendung einen anderen Wert für "Nameihrerapp" ein.</span><span class="sxs-lookup"><span data-stu-id="bb743-130">For each application, pick a different value for "YourAppName".</span></span> <span data-ttu-id="bb743-131">Verwenden Sie nicht den gleichen Wert für mehrere Anwendungen.</span><span class="sxs-lookup"><span data-stu-id="bb743-131">Do not use the same value across multiple applications.</span></span>

## <a name="create-the-azure-services"></a><span data-ttu-id="bb743-132">Erstellen Sie die Azure-Dienste</span><span class="sxs-lookup"><span data-stu-id="bb743-132">Create the Azure Services</span></span>

<span data-ttu-id="bb743-133">Erstellen Sie einen Cloud-Dienst aus, wie im [erstellen und Bereitstellen eines Clouddiensts](https://docs.microsoft.com/azure/cloud-services/cloud-services-how-to-create-deploy).</span><span class="sxs-lookup"><span data-stu-id="bb743-133">Create a Cloud Service, as described in [How to Create and Deploy a Cloud Service](https://docs.microsoft.com/azure/cloud-services/cloud-services-how-to-create-deploy).</span></span> <span data-ttu-id="bb743-134">Führen Sie die Schritte im Abschnitt "Vorgehensweise: Erstellen Sie einen Cloud-Dienst mithilfe der Schnellerfassung".</span><span class="sxs-lookup"><span data-stu-id="bb743-134">Follow the steps in the section "How to: Create a cloud service using Quick Create".</span></span> <span data-ttu-id="bb743-135">Für dieses Tutorial müssen Sie nicht, ein Zertifikat hochzuladen.</span><span class="sxs-lookup"><span data-stu-id="bb743-135">For this tutorial, you do not need to upload a certificate.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image2.png)

<span data-ttu-id="bb743-136">Erstellen Sie einen neuen Service Bus-Namespace, wie im [wie, verwendet Service Bus Themen/-Abonnements](https://docs.microsoft.com/azure/service-bus-messaging/service-bus-dotnet-how-to-use-topics-subscriptions).</span><span class="sxs-lookup"><span data-stu-id="bb743-136">Create a new Service Bus namespace, as described in [How to Use Service Bus Topics/Subscriptions](https://docs.microsoft.com/azure/service-bus-messaging/service-bus-dotnet-how-to-use-topics-subscriptions).</span></span> <span data-ttu-id="bb743-137">Führen Sie die Schritte im Abschnitt "Erstellen einer Dienst-Namespace" aus.</span><span class="sxs-lookup"><span data-stu-id="bb743-137">Follow the steps in the section "Create a Service Namespace".</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image3.png)

> [!NOTE]
> <span data-ttu-id="bb743-138">Stellen Sie sicher, dass die gleiche Region für den Clouddienst und Service Bus-Namespace.</span><span class="sxs-lookup"><span data-stu-id="bb743-138">Make sure to select the same region for the cloud service and the Service Bus namespace.</span></span>


## <a name="create-the-visual-studio-project"></a><span data-ttu-id="bb743-139">Visual Studio-Projekt erstellen</span><span class="sxs-lookup"><span data-stu-id="bb743-139">Create the Visual Studio Project</span></span>

<span data-ttu-id="bb743-140">Starten Sie Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="bb743-140">Start Visual Studio.</span></span> <span data-ttu-id="bb743-141">Von der **Datei** Menü klicken Sie auf **neues Projekt**.</span><span class="sxs-lookup"><span data-stu-id="bb743-141">From the **File** menu, click **New Project**.</span></span>

<span data-ttu-id="bb743-142">In der **neues Projekt** Dialogfeld erweitern Sie **Visual C#-**.</span><span class="sxs-lookup"><span data-stu-id="bb743-142">In the **New Project** dialog box, expand **Visual C#**.</span></span> <span data-ttu-id="bb743-143">Klicken Sie unter **installierte Vorlagen**Option **Cloud** und wählen Sie dann **Windows Azure Cloud Service**.</span><span class="sxs-lookup"><span data-stu-id="bb743-143">Under **Installed Templates**, select **Cloud** and then select **Windows Azure Cloud Service**.</span></span> <span data-ttu-id="bb743-144">Behalten Sie die standardmäßigen .NET Framework 4.5.</span><span class="sxs-lookup"><span data-stu-id="bb743-144">Keep the default .NET Framework 4.5.</span></span> <span data-ttu-id="bb743-145">Nennen Sie die Anwendung ChatService, und klicken Sie auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="bb743-145">Name the application ChatService and click **OK**.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image4.png)

<span data-ttu-id="bb743-146">In der **neuen Windows Azure-Cloud-Dienst** im Dialogfeld auf ASP.NET Web-Rolle.</span><span class="sxs-lookup"><span data-stu-id="bb743-146">In the **New Windows Azure Cloud Service** dialog, select ASP.NET Web Role.</span></span> <span data-ttu-id="bb743-147">Klicken Sie auf den Pfeil nach rechts-Schaltfläche (**&gt;**) um die Rolle der Projektmappe hinzuzufügen.</span><span class="sxs-lookup"><span data-stu-id="bb743-147">Click the right-arrow button (**&gt;**) to add the role to your solution.</span></span>

<span data-ttu-id="bb743-148">Zeigen Sie auf den die neue Rolle, also das Stiftsymbol angezeigt.</span><span class="sxs-lookup"><span data-stu-id="bb743-148">Hover the mouse over the new role, so the pencil icon visible.</span></span> <span data-ttu-id="bb743-149">Klicken Sie auf dieses Symbol, um die Rolle umbenennen.</span><span class="sxs-lookup"><span data-stu-id="bb743-149">Click this icon to rename the role.</span></span> <span data-ttu-id="bb743-150">Benennen Sie die Rolle "SignalRChat", und klicken Sie auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="bb743-150">Name the role "SignalRChat" and click **OK**.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image5.png)

<span data-ttu-id="bb743-151">In der **neues ASP.NET-Projekt** wählen Sie im Dialogfeld **MVC**, und klicken Sie auf OK.</span><span class="sxs-lookup"><span data-stu-id="bb743-151">In the **New ASP.NET Project** dialog, select **MVC**, and click OK.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image6.png)

<span data-ttu-id="bb743-152">Der Assistent erstellt zwei Projekte:</span><span class="sxs-lookup"><span data-stu-id="bb743-152">The project wizard creates two projects:</span></span>

- <span data-ttu-id="bb743-153">ChatService: Dieses Projekt ist das Windows Azure-Anwendung.</span><span class="sxs-lookup"><span data-stu-id="bb743-153">ChatService: This project is the Windows Azure application.</span></span> <span data-ttu-id="bb743-154">Es definiert die Azure-Rollen und andere Konfigurationsoptionen.</span><span class="sxs-lookup"><span data-stu-id="bb743-154">It defines the Azure roles and other configuration options.</span></span>
- <span data-ttu-id="bb743-155">SignalRChat: Dieses Projekt ist das ASP.NET MVC 5-Projekt.</span><span class="sxs-lookup"><span data-stu-id="bb743-155">SignalRChat: This project is your ASP.NET MVC 5 project.</span></span>

## <a name="create-the-signalr-chat-application"></a><span data-ttu-id="bb743-156">Erstellen Sie die SignalR Chat-Anwendung</span><span class="sxs-lookup"><span data-stu-id="bb743-156">Create the SignalR Chat Application</span></span>

<span data-ttu-id="bb743-157">Um die chatanwendung zu erstellen, führen Sie die Schritte in diesem Tutorial [erste Schritte mit SignalR und MVC 5](../getting-started/tutorial-getting-started-with-signalr-and-mvc.md).</span><span class="sxs-lookup"><span data-stu-id="bb743-157">To create the chat application, follow the steps in the tutorial [Getting Started with SignalR and MVC 5](../getting-started/tutorial-getting-started-with-signalr-and-mvc.md).</span></span>

<span data-ttu-id="bb743-158">Verwenden Sie NuGet, um die erforderlichen Bibliotheken zu installieren.</span><span class="sxs-lookup"><span data-stu-id="bb743-158">Use NuGet to install the required libraries.</span></span> <span data-ttu-id="bb743-159">Von der **Tools** , wählen Sie im Menü **NuGet Package Manager**, und wählen Sie dann **-Paket-Manager-Konsole**.</span><span class="sxs-lookup"><span data-stu-id="bb743-159">From the **Tools** menu, select **NuGet Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="bb743-160">In der **-Paket-Manager-Konsole** Fenster, geben Sie die folgenden Befehle aus:</span><span class="sxs-lookup"><span data-stu-id="bb743-160">In the **Package Manager Console** window, enter the following commands:</span></span>

[!code-powershell[Main](scaleout-with-windows-azure-service-bus/samples/sample2.ps1)]

<span data-ttu-id="bb743-161">Verwenden der `-ProjectName` Option aus, um die Pakete auf das ASP.NET MVC-Projekt, und nicht auf Windows Azure-Projekt zu installieren.</span><span class="sxs-lookup"><span data-stu-id="bb743-161">Use the `-ProjectName` option to install the packages to the ASP.NET MVC project, rather than the Windows Azure project.</span></span>

## <a name="configure-the-backplane"></a><span data-ttu-id="bb743-162">Konfigurieren der Rückwandplatine</span><span class="sxs-lookup"><span data-stu-id="bb743-162">Configure the Backplane</span></span>

<span data-ttu-id="bb743-163">Fügen Sie in der Datei "Startup.cs" Ihrer Anwendung den folgenden Code hinzu:</span><span class="sxs-lookup"><span data-stu-id="bb743-163">In your application's Startup.cs file, add the following code:</span></span>

[!code-csharp[Main](scaleout-with-windows-azure-service-bus/samples/sample3.cs)]

<span data-ttu-id="bb743-164">Nun müssen Sie Ihre Service Bus-Verbindungszeichenfolge zu erhalten.</span><span class="sxs-lookup"><span data-stu-id="bb743-164">Now you need to get your service bus connection string.</span></span> <span data-ttu-id="bb743-165">Wählen Sie im Azure-Portal die Service Bus-Namespace, den Sie erstellt haben, und klicken Sie auf das Symbol "Zugriffsschlüssel".</span><span class="sxs-lookup"><span data-stu-id="bb743-165">In the Azure portal, select the service bus namespace that you created and click the Access Key icon.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image7.png)

<span data-ttu-id="bb743-166">Kopieren Sie die Verbindungszeichenfolge in die Zwischenablage, und fügen Sie ihn in das *"ConnectionString"* Variable.</span><span class="sxs-lookup"><span data-stu-id="bb743-166">Copy the connection string to the clipboard, then paste it into the *connectionString* variable.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image8.png)

[!code-csharp[Main](scaleout-with-windows-azure-service-bus/samples/sample4.cs)]

## <a name="deploy-to-azure"></a><span data-ttu-id="bb743-167">Bereitstellung in Azure</span><span class="sxs-lookup"><span data-stu-id="bb743-167">Deploy to Azure</span></span>

<span data-ttu-id="bb743-168">Erweitern Sie im Projektmappen-Explorer die **Rollen** Ordner innerhalb des Projekts ChatService.</span><span class="sxs-lookup"><span data-stu-id="bb743-168">In Solution Explorer, expand the **Roles** folder inside the ChatService project.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image9.png)

<span data-ttu-id="bb743-169">Mit der rechten Maustaste in der Rolle "SignalRChat", und wählen Sie **Eigenschaften**.</span><span class="sxs-lookup"><span data-stu-id="bb743-169">Right-click the SignalRChat role and select **Properties**.</span></span> <span data-ttu-id="bb743-170">Wählen Sie die **Konfiguration** Registerkarte. Klicken Sie unter **Instanzen** 2 auswählen.</span><span class="sxs-lookup"><span data-stu-id="bb743-170">Select the **Configuration** tab. Under **Instances** select 2.</span></span> <span data-ttu-id="bb743-171">Sie können auch die Größe des virtuellen Computers festlegen, um **sehr klein**.</span><span class="sxs-lookup"><span data-stu-id="bb743-171">You can also set the VM size to **Extra Small**.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image10.png)

<span data-ttu-id="bb743-172">Speichern Sie die Änderungen.</span><span class="sxs-lookup"><span data-stu-id="bb743-172">Save the changes.</span></span>

<span data-ttu-id="bb743-173">Klicken Sie im Projektmappen-Explorer das Projekt ChatService.</span><span class="sxs-lookup"><span data-stu-id="bb743-173">In Solution Explorer, right-click the ChatService project.</span></span> <span data-ttu-id="bb743-174">Wählen Sie **Veröffentlichen**.</span><span class="sxs-lookup"><span data-stu-id="bb743-174">Select **Publish**.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image11.png)

<span data-ttu-id="bb743-175">Wenn dies Ihre erste Zeit mit der Veröffentlichung in Windows Azure ist, müssen Sie Ihre Anmeldeinformationen herunterladen.</span><span class="sxs-lookup"><span data-stu-id="bb743-175">If this is your first time publishing to Windows Azure, you must download your credentials.</span></span> <span data-ttu-id="bb743-176">In der **veröffentlichen** -Assistenten klicken Sie auf "Sign in to download Credentials".</span><span class="sxs-lookup"><span data-stu-id="bb743-176">In the **Publish** wizard, click "Sign in to download credentials".</span></span> <span data-ttu-id="bb743-177">Dadurch werden Sie aufgefordert, sich bei Windows Azure-Portal anzumelden, und eine Datei mit veröffentlichungseinstellungen herunterladen.</span><span class="sxs-lookup"><span data-stu-id="bb743-177">This will prompt you to sign into the Windows Azure portal and download a publish settings file.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image12.png)

<span data-ttu-id="bb743-178">Klicken Sie auf **Import** , und wählen Sie die von Ihnen heruntergeladene Datei mit den veröffentlichungseinstellungen.</span><span class="sxs-lookup"><span data-stu-id="bb743-178">Click **Import** and select the publish settings file that you downloaded.</span></span>

<span data-ttu-id="bb743-179">Klicken Sie auf **Weiter**.</span><span class="sxs-lookup"><span data-stu-id="bb743-179">Click **Next**.</span></span> <span data-ttu-id="bb743-180">In der **Veröffentlichungseinstellungen** Dialogfeld unter **Clouddienst**, wählen Sie den Clouddienst, die Sie zuvor erstellt haben.</span><span class="sxs-lookup"><span data-stu-id="bb743-180">In the **Publish Settings** dialog, under **Cloud Service**, select the cloud service that you created earlier.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image13.png)

<span data-ttu-id="bb743-181">Klicken Sie auf **Veröffentlichen**.</span><span class="sxs-lookup"><span data-stu-id="bb743-181">Click **Publish**.</span></span> <span data-ttu-id="bb743-182">Es dauert einige Minuten, bis die Anwendung bereitstellen und starten die virtuellen Computer.</span><span class="sxs-lookup"><span data-stu-id="bb743-182">It can take a few minutes to deploy the application and start the VMs.</span></span>

<span data-ttu-id="bb743-183">Wenn Sie die chatanwendung ausführen, kommunizieren die Rolleninstanzen jetzt über Azure Service Bus, ein Service Bus-Thema mit.</span><span class="sxs-lookup"><span data-stu-id="bb743-183">Now when you run the chat application, the role instances communicate through Azure Service Bus, using a Service Bus topic.</span></span> <span data-ttu-id="bb743-184">Ein Thema ist eine Warteschlange, die mehrere Abonnenten ermöglicht.</span><span class="sxs-lookup"><span data-stu-id="bb743-184">A topic is a message queue that allows multiple subscribers.</span></span>

<span data-ttu-id="bb743-185">Der Rückwand wird automatisch erstellt, das Thema und die Abonnements.</span><span class="sxs-lookup"><span data-stu-id="bb743-185">The backplane automatically creates the topic and the subscriptions.</span></span> <span data-ttu-id="bb743-186">Um die Abonnements und die Nachrichtenaktivität anzuzeigen, öffnen Sie das Azure-Portal, wählen Sie den Service Bus-Namespace, und klicken Sie auf "Themen".</span><span class="sxs-lookup"><span data-stu-id="bb743-186">To see the subscriptions and message activity, open the Azure portal, select the Service Bus namespace, and click on "Topics".</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image14.png)

<span data-ttu-id="bb743-187">Sie nehmen einige Minuten, bis die Aktivität auf dem Dashboard angezeigt werden.</span><span class="sxs-lookup"><span data-stu-id="bb743-187">It make take a few minutes for the message activity to show up in the dashboard.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image15.png)

<span data-ttu-id="bb743-188">SignalR verwaltet die Lebensdauer des Themas.</span><span class="sxs-lookup"><span data-stu-id="bb743-188">SignalR manages the topic lifetime.</span></span> <span data-ttu-id="bb743-189">Solange Ihre Anwendung bereitgestellt wird, versuchen Sie nicht manuell löschen von Themen oder Ändern der Einstellungen für das Thema.</span><span class="sxs-lookup"><span data-stu-id="bb743-189">As long as your application is deployed, don't try to manually delete topics or change settings on the topic.</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="bb743-190">Problembehandlung</span><span class="sxs-lookup"><span data-stu-id="bb743-190">Troubleshooting</span></span>

<span data-ttu-id="bb743-191">**System.InvalidOperationException "Die einzige unterstützte IsolationLevel-Eigenschaft ist"IsolationLevel.Serializable"."**</span><span class="sxs-lookup"><span data-stu-id="bb743-191">**System.InvalidOperationException "The only supported IsolationLevel is 'IsolationLevel.Serializable'."**</span></span>

<span data-ttu-id="bb743-192">Dieser Fehler kann auftreten, wenn die Transaktionsebene für einen Vorgang auf einen anderen Wert, außer festgelegt ist `Serializable`.</span><span class="sxs-lookup"><span data-stu-id="bb743-192">This error can occur if the transaction level for an operation is set to something other than `Serializable`.</span></span> <span data-ttu-id="bb743-193">Stellen Sie sicher, dass keine Vorgänge mit anderen Ebenen für die Transaktion ausgeführt werden.</span><span class="sxs-lookup"><span data-stu-id="bb743-193">Verify that no operations are being performed with other transaction levels.</span></span>
