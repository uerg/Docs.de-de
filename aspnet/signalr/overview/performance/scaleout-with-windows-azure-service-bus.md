---
uid: signalr/overview/performance/scaleout-with-windows-azure-service-bus
title: SignalR mit horizontaler Skalierung mit Azure Servicebus | Microsoft Docs
author: MikeWasson
description: "Versionen der Software verwendet in diesem Thema Visual Studio 2013 .NET 4.5 SignalR Version 2 frühere Versionen dieser Thema für SignalR 1.x-Version dieses Themas..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2014
ms.topic: article
ms.assetid: ce1305f9-30fd-49e3-bf38-d0a78dfb06c3
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/performance/scaleout-with-windows-azure-service-bus
msc.type: authoredcontent
ms.openlocfilehash: 857fc8baa61549e2fabbb8da012b1fa23950237d
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="signalr-scaleout-with-azure-service-bus"></a><span data-ttu-id="fd3c8-103">SignalR mit horizontaler Skalierung mit Azure Servicebus</span><span class="sxs-lookup"><span data-stu-id="fd3c8-103">SignalR Scaleout with Azure Service Bus</span></span>
====================
<span data-ttu-id="fd3c8-104">durch [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="fd3c8-104">by [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)</span></span>

<span data-ttu-id="fd3c8-105">In diesem Lernprogramm werden Sie eine SignalR-Anwendung in einer Windows Azure-Webrolle, mit der Service Bus-Rückwandplatine zum Verteilen von Nachrichten für jede Rolleninstanz bereitstellen.</span><span class="sxs-lookup"><span data-stu-id="fd3c8-105">In this tutorial, you will deploy a SignalR application to a Windows Azure Web Role, using the Service Bus backplane to distribute messages to each role instance.</span></span> <span data-ttu-id="fd3c8-106">(Sie können auch mit die Service Bus-Rückwandplatine [web-apps in Azure App Service](https://docs.microsoft.com/azure/app-service-web/).)</span><span class="sxs-lookup"><span data-stu-id="fd3c8-106">(You can also use the Service Bus backplane with [web apps in Azure App Service](https://docs.microsoft.com/azure/app-service-web/).)</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image1.png)

<span data-ttu-id="fd3c8-107">Erforderliche Komponenten:</span><span class="sxs-lookup"><span data-stu-id="fd3c8-107">Prerequisites:</span></span>

- <span data-ttu-id="fd3c8-108">Ein Windows Azure-Konto.</span><span class="sxs-lookup"><span data-stu-id="fd3c8-108">A Windows Azure account.</span></span>
- <span data-ttu-id="fd3c8-109">Die [Windows Azure SDK](https://go.microsoft.com/fwlink/?linkid=254364&amp;clcid=0x409).</span><span class="sxs-lookup"><span data-stu-id="fd3c8-109">The [Windows Azure SDK](https://go.microsoft.com/fwlink/?linkid=254364&amp;clcid=0x409).</span></span>
- <span data-ttu-id="fd3c8-110">Visual Studio 2012 oder 2013.</span><span class="sxs-lookup"><span data-stu-id="fd3c8-110">Visual Studio 2012 or 2013.</span></span>

<span data-ttu-id="fd3c8-111">Die Servicebus-Rückwandplatine ist kompatibel mit [Service Bus for Windows Server](https://msdn.microsoft.com/en-us/library/windowsazure/dn282144.aspx), Version 1.1.</span><span class="sxs-lookup"><span data-stu-id="fd3c8-111">The service bus backplane is also compatible with [Service Bus for Windows Server](https://msdn.microsoft.com/en-us/library/windowsazure/dn282144.aspx), version 1.1.</span></span> <span data-ttu-id="fd3c8-112">Allerdings ist es nicht mit Version 1.0 der Service Bus for Windows Server kompatibel.</span><span class="sxs-lookup"><span data-stu-id="fd3c8-112">However, it is not compatible with version 1.0 of Service Bus for Windows Server.</span></span>

## <a name="pricing"></a><span data-ttu-id="fd3c8-113">Preise</span><span class="sxs-lookup"><span data-stu-id="fd3c8-113">Pricing</span></span>

<span data-ttu-id="fd3c8-114">Die Service Bus-Rückwandplatine werden Themen zum Senden von Nachrichten verwendet.</span><span class="sxs-lookup"><span data-stu-id="fd3c8-114">The Service Bus backplane uses topics to send messages.</span></span> <span data-ttu-id="fd3c8-115">Die neuesten Preisinformationen finden Sie unter [Service Bus](https://azure.microsoft.com/pricing/details/service-bus/).</span><span class="sxs-lookup"><span data-stu-id="fd3c8-115">For the latest pricing information, see [Service Bus](https://azure.microsoft.com/pricing/details/service-bus/).</span></span> <span data-ttu-id="fd3c8-116">Zum Zeitpunkt der Erstellung dieses Dokuments können Sie für weniger als 1 $ 1.000.000 Nachrichten pro Monat senden.</span><span class="sxs-lookup"><span data-stu-id="fd3c8-116">At the time of this writing, you can send 1,000,000 messages per month for less than $1.</span></span> <span data-ttu-id="fd3c8-117">Der Rückwand sendet eine Servicebus-Nachricht für jeden Aufruf einer SignalR-Hub-Methode.</span><span class="sxs-lookup"><span data-stu-id="fd3c8-117">The backplane sends a service bus message for each invocation of a SignalR hub method.</span></span> <span data-ttu-id="fd3c8-118">Es gibt auch einige Steuerungsnachrichten für Verbindungen, Trennvorgänge beitreten oder verlassen Gruppen und So weiter.</span><span class="sxs-lookup"><span data-stu-id="fd3c8-118">There are also some control messages for connections, disconnections, joining or leaving groups, and so forth.</span></span> <span data-ttu-id="fd3c8-119">In den meisten Anwendungen wird die Mehrheit der dem Volumen des Nachrichtenverkehrs hubmethodenaufrufe sein.</span><span class="sxs-lookup"><span data-stu-id="fd3c8-119">In most applications, the majority of the message traffic will be hub method invocations.</span></span>

## <a name="overview"></a><span data-ttu-id="fd3c8-120">Übersicht</span><span class="sxs-lookup"><span data-stu-id="fd3c8-120">Overview</span></span>

<span data-ttu-id="fd3c8-121">Bevor wir auf die ausführliches Lernprogramm erhalten, ist hier ein schnellen Überblick darüber, was Sie tun können.</span><span class="sxs-lookup"><span data-stu-id="fd3c8-121">Before we get to the detailed tutorial, here is a quick overview of what you will do.</span></span>

1. <span data-ttu-id="fd3c8-122">Verwenden Sie die Windows Azure-Portal, um einen neuen Service Bus-Namespace zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="fd3c8-122">Use the Windows Azure portal to create a new Service Bus namespace.</span></span>
2. <span data-ttu-id="fd3c8-123">Ihre Anwendung diese NuGet-Pakete hinzugefügt werden:</span><span class="sxs-lookup"><span data-stu-id="fd3c8-123">Add these NuGet packages to your application:</span></span> 

    - [<span data-ttu-id="fd3c8-124">Microsoft.AspNet.SignalR</span><span class="sxs-lookup"><span data-stu-id="fd3c8-124">Microsoft.AspNet.SignalR</span></span>](http://nuget.org/packages/Microsoft.AspNet.SignalR)
    - [<span data-ttu-id="fd3c8-125">Microsoft.AspNet.SignalR.ServiceBus</span><span class="sxs-lookup"><span data-stu-id="fd3c8-125">Microsoft.AspNet.SignalR.ServiceBus</span></span>](http://www.nuget.org/packages/SignalR.WindowsAzureServiceBus)
3. <span data-ttu-id="fd3c8-126">Erstellen einer SignalR-Anwendung.</span><span class="sxs-lookup"><span data-stu-id="fd3c8-126">Create a SignalR application.</span></span>
4. <span data-ttu-id="fd3c8-127">Fügen Sie den folgenden Code, um tritt an der Rückwand zu konfigurieren:</span><span class="sxs-lookup"><span data-stu-id="fd3c8-127">Add the following code to Startup.cs to configure the backplane:</span></span> 

    [!code-csharp[Main](scaleout-with-windows-azure-service-bus/samples/sample1.cs)]

<span data-ttu-id="fd3c8-128">Dieser Code dient zum Konfigurieren der Rückwand mit den Standardwerten für [TopicCount](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.servicebusscaleoutconfiguration.topiccount(v=vs.118).aspx) und [MaxQueueLength](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.messaging.scaleoutconfiguration.maxqueuelength(v=vs.118).aspx).</span><span class="sxs-lookup"><span data-stu-id="fd3c8-128">This code configures the backplane with the default values for [TopicCount](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.servicebusscaleoutconfiguration.topiccount(v=vs.118).aspx) and [MaxQueueLength](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.messaging.scaleoutconfiguration.maxqueuelength(v=vs.118).aspx).</span></span> <span data-ttu-id="fd3c8-129">Informationen zum Ändern dieser Werte finden Sie unter [SignalR-Leistung: Warteschlangen für horizontale Skalierung Metriken](signalr-performance.md#scaleout_metrics).</span><span class="sxs-lookup"><span data-stu-id="fd3c8-129">For information on changing these values, see [SignalR Performance: Scaleout Metrics](signalr-performance.md#scaleout_metrics).</span></span>

<span data-ttu-id="fd3c8-130">Wählen Sie für jede Anwendung einen anderen Wert für "Nameihrerapp" ein.</span><span class="sxs-lookup"><span data-stu-id="fd3c8-130">For each application, pick a different value for "YourAppName".</span></span> <span data-ttu-id="fd3c8-131">Verwenden Sie nicht den gleichen Wert für mehrere Anwendungen.</span><span class="sxs-lookup"><span data-stu-id="fd3c8-131">Do not use the same value across multiple applications.</span></span>

## <a name="create-the-azure-services"></a><span data-ttu-id="fd3c8-132">Erstellen der Azure-Dienste</span><span class="sxs-lookup"><span data-stu-id="fd3c8-132">Create the Azure Services</span></span>

<span data-ttu-id="fd3c8-133">Erstellen Sie einen Cloud-Dienst aus, wie beschrieben in [zum Erstellen und Bereitstellen eines Cloud-Diensts](https://docs.microsoft.com/azure/cloud-services/cloud-services-how-to-create-deploy).</span><span class="sxs-lookup"><span data-stu-id="fd3c8-133">Create a Cloud Service, as described in [How to Create and Deploy a Cloud Service](https://docs.microsoft.com/azure/cloud-services/cloud-services-how-to-create-deploy).</span></span> <span data-ttu-id="fd3c8-134">Führen Sie die Schritte im Abschnitt "Vorgehensweise: erstellen ein Cloud-Diensts verwenden der Schnellerfassung".</span><span class="sxs-lookup"><span data-stu-id="fd3c8-134">Follow the steps in the section "How to: Create a cloud service using Quick Create".</span></span> <span data-ttu-id="fd3c8-135">Für dieses Lernprogramm müssen Sie sich nicht um ein Zertifikat hochzuladen.</span><span class="sxs-lookup"><span data-stu-id="fd3c8-135">For this tutorial, you do not need to upload a certificate.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image2.png)

<span data-ttu-id="fd3c8-136">Erstellen Sie einen neuen Service Bus-Namespace ein, wie beschrieben in [wie auf verwenden Service Bus-Themen/-Abonnements](https://docs.microsoft.com/azure/service-bus-messaging/service-bus-dotnet-how-to-use-topics-subscriptions).</span><span class="sxs-lookup"><span data-stu-id="fd3c8-136">Create a new Service Bus namespace, as described in [How to Use Service Bus Topics/Subscriptions](https://docs.microsoft.com/azure/service-bus-messaging/service-bus-dotnet-how-to-use-topics-subscriptions).</span></span> <span data-ttu-id="fd3c8-137">Führen Sie die Schritte im Abschnitt "Erstellen einer Dienst-Namespace" aus.</span><span class="sxs-lookup"><span data-stu-id="fd3c8-137">Follow the steps in the section "Create a Service Namespace".</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image3.png)

> [!NOTE]
> <span data-ttu-id="fd3c8-138">Stellen Sie sicher, dass die gleiche Region für den Clouddienst und Service Bus-Namespace.</span><span class="sxs-lookup"><span data-stu-id="fd3c8-138">Make sure to select the same region for the cloud service and the Service Bus namespace.</span></span>


## <a name="create-the-visual-studio-project"></a><span data-ttu-id="fd3c8-139">Visual Studio-Projekt erstellen</span><span class="sxs-lookup"><span data-stu-id="fd3c8-139">Create the Visual Studio Project</span></span>

<span data-ttu-id="fd3c8-140">Starten Sie Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="fd3c8-140">Start Visual Studio.</span></span> <span data-ttu-id="fd3c8-141">Aus der **Datei** Menü klicken Sie auf **neues Projekt**.</span><span class="sxs-lookup"><span data-stu-id="fd3c8-141">From the **File** menu, click **New Project**.</span></span>

<span data-ttu-id="fd3c8-142">In der **neues Projekt** Dialogfeld erweitern Sie **Visual C#-**.</span><span class="sxs-lookup"><span data-stu-id="fd3c8-142">In the **New Project** dialog box, expand **Visual C#**.</span></span> <span data-ttu-id="fd3c8-143">Klicken Sie unter **installierte Vorlagen**Option **Cloud** und wählen Sie dann **Windows Azure-Cloud-Dienst**.</span><span class="sxs-lookup"><span data-stu-id="fd3c8-143">Under **Installed Templates**, select **Cloud** and then select **Windows Azure Cloud Service**.</span></span> <span data-ttu-id="fd3c8-144">Behalten Sie die Standardeinstellung .NET Framework 4.5.</span><span class="sxs-lookup"><span data-stu-id="fd3c8-144">Keep the default .NET Framework 4.5.</span></span> <span data-ttu-id="fd3c8-145">Benennen Sie die Anwendung ChatService, und klicken Sie auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="fd3c8-145">Name the application ChatService and click **OK**.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image4.png)

<span data-ttu-id="fd3c8-146">In der **neuen Windows Azure-Cloud-Dienst** ASP.NET-Webrolle wählen Sie im Dialogfeld.</span><span class="sxs-lookup"><span data-stu-id="fd3c8-146">In the **New Windows Azure Cloud Service** dialog, select ASP.NET Web Role.</span></span> <span data-ttu-id="fd3c8-147">Klicken Sie auf den Pfeil nach rechts (**&gt;**), die der Projektmappe hinzuzufügen.</span><span class="sxs-lookup"><span data-stu-id="fd3c8-147">Click the right-arrow button (**&gt;**) to add the role to your solution.</span></span>

<span data-ttu-id="fd3c8-148">Bewegen Sie die Maus über die neue Rolle, also das Stiftsymbol angezeigt.</span><span class="sxs-lookup"><span data-stu-id="fd3c8-148">Hover the mouse over the new role, so the pencil icon visible.</span></span> <span data-ttu-id="fd3c8-149">Klicken Sie auf dieses Symbol, um die Rolle umbenennen.</span><span class="sxs-lookup"><span data-stu-id="fd3c8-149">Click this icon to rename the role.</span></span> <span data-ttu-id="fd3c8-150">Benennen Sie die Rolle "SignalRChat", und klicken Sie auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="fd3c8-150">Name the role "SignalRChat" and click **OK**.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image5.png)

<span data-ttu-id="fd3c8-151">In der **neues ASP.NET-Projekt** wählen Sie im Dialogfeld **MVC**, und klicken Sie auf OK.</span><span class="sxs-lookup"><span data-stu-id="fd3c8-151">In the **New ASP.NET Project** dialog, select **MVC**, and click OK.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image6.png)

<span data-ttu-id="fd3c8-152">Der Assistent erstellt zwei Projekte:</span><span class="sxs-lookup"><span data-stu-id="fd3c8-152">The project wizard creates two projects:</span></span>

- <span data-ttu-id="fd3c8-153">ChatService: Dieses Projekt ist das Windows Azure-Anwendung.</span><span class="sxs-lookup"><span data-stu-id="fd3c8-153">ChatService: This project is the Windows Azure application.</span></span> <span data-ttu-id="fd3c8-154">Er definiert den Azure-Rollen und weitere Konfigurationsoptionen.</span><span class="sxs-lookup"><span data-stu-id="fd3c8-154">It defines the Azure roles and other configuration options.</span></span>
- <span data-ttu-id="fd3c8-155">SignalRChat: Dieses Projekt ist das ASP.NET MVC 5-Projekt.</span><span class="sxs-lookup"><span data-stu-id="fd3c8-155">SignalRChat: This project is your ASP.NET MVC 5 project.</span></span>

## <a name="create-the-signalr-chat-application"></a><span data-ttu-id="fd3c8-156">Die SignalR-Chat-Anwendung erstellen</span><span class="sxs-lookup"><span data-stu-id="fd3c8-156">Create the SignalR Chat Application</span></span>

<span data-ttu-id="fd3c8-157">Um die chatanwendung zu erstellen, führen Sie die Schritte im Lernprogramm [erste Schritte mit SignalR und MVC 5](../getting-started/tutorial-getting-started-with-signalr-and-mvc.md).</span><span class="sxs-lookup"><span data-stu-id="fd3c8-157">To create the chat application, follow the steps in the tutorial [Getting Started with SignalR and MVC 5](../getting-started/tutorial-getting-started-with-signalr-and-mvc.md).</span></span>

<span data-ttu-id="fd3c8-158">Installieren Sie die erforderlichen Bibliotheken mithilfe von NuGet.</span><span class="sxs-lookup"><span data-stu-id="fd3c8-158">Use NuGet to install the required libraries.</span></span> <span data-ttu-id="fd3c8-159">Aus der **Tools** klicken Sie im Menü **Bibliothekspaket-Manager**, und wählen Sie dann **Package Manager Console**.</span><span class="sxs-lookup"><span data-stu-id="fd3c8-159">From the **Tools** menu, select **Library Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="fd3c8-160">In der **Package Manager Console** Fenster, geben Sie die folgenden Befehle:</span><span class="sxs-lookup"><span data-stu-id="fd3c8-160">In the **Package Manager Console** window, enter the following commands:</span></span>

[!code-powershell[Main](scaleout-with-windows-azure-service-bus/samples/sample2.ps1)]

<span data-ttu-id="fd3c8-161">Verwenden der `-ProjectName` Option zum Installieren der Pakete anstelle der Windows Azure-Projekt das ASP.NET MVC-Projekt zur Verfügung.</span><span class="sxs-lookup"><span data-stu-id="fd3c8-161">Use the `-ProjectName` option to install the packages to the ASP.NET MVC project, rather than the Windows Azure project.</span></span>

## <a name="configure-the-backplane"></a><span data-ttu-id="fd3c8-162">Konfigurieren der Rückwand</span><span class="sxs-lookup"><span data-stu-id="fd3c8-162">Configure the Backplane</span></span>

<span data-ttu-id="fd3c8-163">Fügen Sie in Ihrer Anwendung Startup.cs Datei den folgenden Code hinzu:</span><span class="sxs-lookup"><span data-stu-id="fd3c8-163">In your application's Startup.cs file, add the following code:</span></span>

[!code-csharp[Main](scaleout-with-windows-azure-service-bus/samples/sample3.cs)]

<span data-ttu-id="fd3c8-164">Nun müssen Sie die Servicebus-Verbindungszeichenfolge abzurufen.</span><span class="sxs-lookup"><span data-stu-id="fd3c8-164">Now you need to get your service bus connection string.</span></span> <span data-ttu-id="fd3c8-165">Wählen Sie im Azure-Portal den Servicebus-Namespace, den Sie erstellt haben, und klicken Sie auf das Symbol "Zugriffsschlüssel".</span><span class="sxs-lookup"><span data-stu-id="fd3c8-165">In the Azure portal, select the service bus namespace that you created and click the Access Key icon.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image7.png)

<span data-ttu-id="fd3c8-166">Kopieren Sie die Verbindungszeichenfolge in die Zwischenablage, und fügen Sie ihn in die *"ConnectionString"* Variable.</span><span class="sxs-lookup"><span data-stu-id="fd3c8-166">Copy the connection string to the clipboard, then paste it into the *connectionString* variable.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image8.png)

[!code-csharp[Main](scaleout-with-windows-azure-service-bus/samples/sample4.cs)]

## <a name="deploy-to-azure"></a><span data-ttu-id="fd3c8-167">In Azure bereitstellen</span><span class="sxs-lookup"><span data-stu-id="fd3c8-167">Deploy to Azure</span></span>

<span data-ttu-id="fd3c8-168">Erweitern Sie im Projektmappen-Explorer die **Rollen** Ordner im Projekt ChatService.</span><span class="sxs-lookup"><span data-stu-id="fd3c8-168">In Solution Explorer, expand the **Roles** folder inside the ChatService project.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image9.png)

<span data-ttu-id="fd3c8-169">Mit der rechten Maustaste in der Rolle "SignalRChat", und wählen Sie **Eigenschaften**.</span><span class="sxs-lookup"><span data-stu-id="fd3c8-169">Right-click the SignalRChat role and select **Properties**.</span></span> <span data-ttu-id="fd3c8-170">Wählen Sie die **Konfiguration** Registerkarte. Klicken Sie unter **Instanzen** 2 auswählen.</span><span class="sxs-lookup"><span data-stu-id="fd3c8-170">Select the **Configuration** tab. Under **Instances** select 2.</span></span> <span data-ttu-id="fd3c8-171">Sie können auch die Größe des virtuellen Computers festlegen, um **sehr klein**.</span><span class="sxs-lookup"><span data-stu-id="fd3c8-171">You can also set the VM size to **Extra Small**.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image10.png)

<span data-ttu-id="fd3c8-172">Speichern Sie die Änderungen.</span><span class="sxs-lookup"><span data-stu-id="fd3c8-172">Save the changes.</span></span>

<span data-ttu-id="fd3c8-173">Im Projektmappen-Explorer mit der rechten Maustaste des Projekts ChatService.</span><span class="sxs-lookup"><span data-stu-id="fd3c8-173">In Solution Explorer, right-click the ChatService project.</span></span> <span data-ttu-id="fd3c8-174">Wählen Sie **Veröffentlichen**.</span><span class="sxs-lookup"><span data-stu-id="fd3c8-174">Select **Publish**.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image11.png)

<span data-ttu-id="fd3c8-175">Wenn dies Ihre erste Zeit Veröffentlichungsvorgang in Windows Azure ist, müssen Sie Ihre Anmeldeinformationen herunterladen.</span><span class="sxs-lookup"><span data-stu-id="fd3c8-175">If this is your first time publishing to Windows Azure, you must download your credentials.</span></span> <span data-ttu-id="fd3c8-176">In der **veröffentlichen** -Assistenten klicken Sie auf "Sign in to download Credentials".</span><span class="sxs-lookup"><span data-stu-id="fd3c8-176">In the **Publish** wizard, click "Sign in to download credentials".</span></span> <span data-ttu-id="fd3c8-177">Dadurch werden Sie aufgefordert, sich bei Windows Azure-Portal anmelden und eine Datei mit veröffentlichungseinstellungen herunterladen.</span><span class="sxs-lookup"><span data-stu-id="fd3c8-177">This will prompt you to sign into the Windows Azure portal and download a publish settings file.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image12.png)

<span data-ttu-id="fd3c8-178">Klicken Sie auf **Import** , und wählen Sie die heruntergeladene Datei mit den veröffentlichungseinstellungen.</span><span class="sxs-lookup"><span data-stu-id="fd3c8-178">Click **Import** and select the publish settings file that you downloaded.</span></span>

<span data-ttu-id="fd3c8-179">Klicken Sie auf **Weiter**.</span><span class="sxs-lookup"><span data-stu-id="fd3c8-179">Click **Next**.</span></span> <span data-ttu-id="fd3c8-180">In der **Veröffentlichungseinstellungen** Dialogfeld unter **Cloud-Dienst**, wählen Sie den Clouddienst, die Sie zuvor erstellt haben.</span><span class="sxs-lookup"><span data-stu-id="fd3c8-180">In the **Publish Settings** dialog, under **Cloud Service**, select the cloud service that you created earlier.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image13.png)

<span data-ttu-id="fd3c8-181">Klicken Sie auf **Veröffentlichen**.</span><span class="sxs-lookup"><span data-stu-id="fd3c8-181">Click **Publish**.</span></span> <span data-ttu-id="fd3c8-182">Es dauert einige Minuten, bis die Anwendung bereitstellen und starten die virtuellen Computern.</span><span class="sxs-lookup"><span data-stu-id="fd3c8-182">It can take a few minutes to deploy the application and start the VMs.</span></span>

<span data-ttu-id="fd3c8-183">Beim Ausführen der chatanwendung kommunizieren die Rolleninstanzen jetzt über Azure Service Bus mithilfe eines Service Bus-Themas.</span><span class="sxs-lookup"><span data-stu-id="fd3c8-183">Now when you run the chat application, the role instances communicate through Azure Service Bus, using a Service Bus topic.</span></span> <span data-ttu-id="fd3c8-184">Ein Thema ist eine Warteschlange, die mehrere Abonnenten ermöglicht.</span><span class="sxs-lookup"><span data-stu-id="fd3c8-184">A topic is a message queue that allows multiple subscribers.</span></span>

<span data-ttu-id="fd3c8-185">Der Rückwand erstellt automatisch das Thema und die Abonnements.</span><span class="sxs-lookup"><span data-stu-id="fd3c8-185">The backplane automatically creates the topic and the subscriptions.</span></span> <span data-ttu-id="fd3c8-186">Um das Abonnement und die Nachrichtenaktivität anzuzeigen, öffnen Sie das Azure Portal, wählen Sie den Service Bus-Namespace, und klicken Sie auf "Themen".</span><span class="sxs-lookup"><span data-stu-id="fd3c8-186">To see the subscriptions and message activity, open the Azure portal, select the Service Bus namespace, and click on "Topics".</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image14.png)

<span data-ttu-id="fd3c8-187">Dafür können einige Minuten, bis die Nachrichtenaktivität im Dashboard angezeigt werden.</span><span class="sxs-lookup"><span data-stu-id="fd3c8-187">It make take a few minutes for the message activity to show up in the dashboard.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image15.png)

<span data-ttu-id="fd3c8-188">SignalR verwaltet die Lebensdauer des Themas.</span><span class="sxs-lookup"><span data-stu-id="fd3c8-188">SignalR manages the topic lifetime.</span></span> <span data-ttu-id="fd3c8-189">Solange die Anwendung bereitgestellt wurde, versuchen Sie nicht manuell Themen löschen oder Ändern von Einstellungen für das Thema.</span><span class="sxs-lookup"><span data-stu-id="fd3c8-189">As long as your application is deployed, don't try to manually delete topics or change settings on the topic.</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="fd3c8-190">Problembehandlung</span><span class="sxs-lookup"><span data-stu-id="fd3c8-190">Troubleshooting</span></span>

<span data-ttu-id="fd3c8-191">**System.InvalidOperationException "Die einzige unterstützte IsolationLevel ist"IsolationLevel.Serializable"."**</span><span class="sxs-lookup"><span data-stu-id="fd3c8-191">**System.InvalidOperationException "The only supported IsolationLevel is 'IsolationLevel.Serializable'."**</span></span>

<span data-ttu-id="fd3c8-192">Dieser Fehler kann auftreten, wenn die Transaktionsebene für einen Vorgang auf einen anderen Wert, außer festgelegt ist `Serializable`.</span><span class="sxs-lookup"><span data-stu-id="fd3c8-192">This error can occur if the transaction level for an operation is set to something other than `Serializable`.</span></span> <span data-ttu-id="fd3c8-193">Stellen Sie sicher, dass keine Vorgänge mit anderen Ebenen für die Transaktion ausgeführt werden.</span><span class="sxs-lookup"><span data-stu-id="fd3c8-193">Verify that no operations are being performed with other transaction levels.</span></span>
