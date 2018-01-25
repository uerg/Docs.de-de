---
uid: signalr/overview/older-versions/scaleout-with-windows-azure-service-bus
title: SignalR mit horizontaler Skalierung mit Azure Servicebus (SignalR 1.x) | Microsoft Docs
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/01/2013
ms.topic: article
ms.assetid: 501db899-e68c-49ff-81b2-1dc561bfe908
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/older-versions/scaleout-with-windows-azure-service-bus
msc.type: authoredcontent
ms.openlocfilehash: b48a7b04701b69f68a492c0f7e08da4a37a92a48
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/24/2018
---
<a name="signalr-scaleout-with-azure-service-bus-signalr-1x"></a><span data-ttu-id="1c16b-102">SignalR mit horizontaler Skalierung mit Azure Servicebus (SignalR 1.x)</span><span class="sxs-lookup"><span data-stu-id="1c16b-102">SignalR Scaleout with Azure Service Bus (SignalR 1.x)</span></span>
====================
<span data-ttu-id="1c16b-103">durch [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="1c16b-103">by [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)</span></span>

<span data-ttu-id="1c16b-104">In diesem Lernprogramm werden Sie eine SignalR-Anwendung in einer Windows Azure-Webrolle, mit der Service Bus-Rückwandplatine zum Verteilen von Nachrichten für jede Rolleninstanz bereitstellen.</span><span class="sxs-lookup"><span data-stu-id="1c16b-104">In this tutorial, you will deploy a SignalR application to a Windows Azure Web Role, using the Service Bus backplane to distribute messages to each role instance.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image1.png)

<span data-ttu-id="1c16b-105">Erforderliche Komponenten:</span><span class="sxs-lookup"><span data-stu-id="1c16b-105">Prerequisites:</span></span>

- <span data-ttu-id="1c16b-106">Ein Windows Azure-Konto.</span><span class="sxs-lookup"><span data-stu-id="1c16b-106">A Windows Azure account.</span></span>
- <span data-ttu-id="1c16b-107">Die [Windows Azure SDK](https://go.microsoft.com/fwlink/?linkid=254364&amp;clcid=0x409).</span><span class="sxs-lookup"><span data-stu-id="1c16b-107">The [Windows Azure SDK](https://go.microsoft.com/fwlink/?linkid=254364&amp;clcid=0x409).</span></span>
- <span data-ttu-id="1c16b-108">Visual Studio 2012.</span><span class="sxs-lookup"><span data-stu-id="1c16b-108">Visual Studio 2012.</span></span>

<span data-ttu-id="1c16b-109">Die Servicebus-Rückwandplatine ist kompatibel mit [Service Bus for Windows Server](https://msdn.microsoft.com/library/windowsazure/dn282144.aspx), Version 1.1.</span><span class="sxs-lookup"><span data-stu-id="1c16b-109">The service bus backplane is also compatible with [Service Bus for Windows Server](https://msdn.microsoft.com/library/windowsazure/dn282144.aspx), version 1.1.</span></span> <span data-ttu-id="1c16b-110">Allerdings ist es nicht mit Version 1.0 der Service Bus for Windows Server kompatibel.</span><span class="sxs-lookup"><span data-stu-id="1c16b-110">However, it is not compatible with version 1.0 of Service Bus for Windows Server.</span></span>

## <a name="pricing"></a><span data-ttu-id="1c16b-111">Preise</span><span class="sxs-lookup"><span data-stu-id="1c16b-111">Pricing</span></span>

<span data-ttu-id="1c16b-112">Die Service Bus-Rückwandplatine werden Themen zum Senden von Nachrichten verwendet.</span><span class="sxs-lookup"><span data-stu-id="1c16b-112">The Service Bus backplane uses topics to send messages.</span></span> <span data-ttu-id="1c16b-113">Die neuesten Preisinformationen finden Sie unter [Service Bus](https://azure.microsoft.com/pricing/details/service-bus/).</span><span class="sxs-lookup"><span data-stu-id="1c16b-113">For the latest pricing information, see [Service Bus](https://azure.microsoft.com/pricing/details/service-bus/).</span></span> <span data-ttu-id="1c16b-114">Zum Zeitpunkt der Erstellung dieses Dokuments können Sie für weniger als 1 $ 1.000.000 Nachrichten pro Monat senden.</span><span class="sxs-lookup"><span data-stu-id="1c16b-114">At the time of this writing, you can send 1,000,000 messages per month for less than $1.</span></span> <span data-ttu-id="1c16b-115">Der Rückwand sendet eine Servicebus-Nachricht für jeden Aufruf einer SignalR-Hub-Methode.</span><span class="sxs-lookup"><span data-stu-id="1c16b-115">The backplane sends a service bus message for each invocation of a SignalR hub method.</span></span> <span data-ttu-id="1c16b-116">Es gibt auch einige Steuerungsnachrichten für Verbindungen, Trennvorgänge beitreten oder verlassen Gruppen und So weiter.</span><span class="sxs-lookup"><span data-stu-id="1c16b-116">There are also some control messages for connections, disconnections, joining or leaving groups, and so forth.</span></span> <span data-ttu-id="1c16b-117">In den meisten Anwendungen wird die Mehrheit der dem Volumen des Nachrichtenverkehrs hubmethodenaufrufe sein.</span><span class="sxs-lookup"><span data-stu-id="1c16b-117">In most applications, the majority of the message traffic will be hub method invocations.</span></span>

## <a name="overview"></a><span data-ttu-id="1c16b-118">Übersicht</span><span class="sxs-lookup"><span data-stu-id="1c16b-118">Overview</span></span>

<span data-ttu-id="1c16b-119">Bevor wir auf die ausführliches Lernprogramm erhalten, ist hier ein schnellen Überblick darüber, was Sie tun können.</span><span class="sxs-lookup"><span data-stu-id="1c16b-119">Before we get to the detailed tutorial, here is a quick overview of what you will do.</span></span>

1. <span data-ttu-id="1c16b-120">Verwenden Sie die Windows Azure-Portal, um einen neuen Service Bus-Namespace zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="1c16b-120">Use the Windows Azure portal to create a new Service Bus namespace.</span></span>
2. <span data-ttu-id="1c16b-121">Ihre Anwendung diese NuGet-Pakete hinzugefügt werden:</span><span class="sxs-lookup"><span data-stu-id="1c16b-121">Add these NuGet packages to your application:</span></span> 

    - [<span data-ttu-id="1c16b-122">Microsoft.AspNet.SignalR</span><span class="sxs-lookup"><span data-stu-id="1c16b-122">Microsoft.AspNet.SignalR</span></span>](http://nuget.org/packages/Microsoft.AspNet.SignalR)
    - [<span data-ttu-id="1c16b-123">Microsoft.AspNet.SignalR.ServiceBus</span><span class="sxs-lookup"><span data-stu-id="1c16b-123">Microsoft.AspNet.SignalR.ServiceBus</span></span>](http://www.nuget.org/packages/SignalR.WindowsAzureServiceBus)
3. <span data-ttu-id="1c16b-124">Erstellen einer SignalR-Anwendung.</span><span class="sxs-lookup"><span data-stu-id="1c16b-124">Create a SignalR application.</span></span>
4. <span data-ttu-id="1c16b-125">Fügen Sie den folgenden Code, um "Global.asax" So konfigurieren Sie die Backplane:</span><span class="sxs-lookup"><span data-stu-id="1c16b-125">Add the following code to Global.asax to configure the backplane:</span></span> 

    [!code-csharp[Main](scaleout-with-windows-azure-service-bus/samples/sample1.cs)]

<span data-ttu-id="1c16b-126">Wählen Sie für jede Anwendung einen anderen Wert für "Nameihrerapp" ein.</span><span class="sxs-lookup"><span data-stu-id="1c16b-126">For each application, pick a different value for "YourAppName".</span></span> <span data-ttu-id="1c16b-127">Verwenden Sie nicht den gleichen Wert für mehrere Anwendungen.</span><span class="sxs-lookup"><span data-stu-id="1c16b-127">Do not use the same value across multiple applications.</span></span>

## <a name="create-the-azure-services"></a><span data-ttu-id="1c16b-128">Erstellen der Azure-Dienste</span><span class="sxs-lookup"><span data-stu-id="1c16b-128">Create the Azure Services</span></span>

<span data-ttu-id="1c16b-129">Erstellen Sie einen Cloud-Dienst aus, wie beschrieben in [zum Erstellen und Bereitstellen eines Cloud-Diensts](https://docs.microsoft.com/azure/cloud-services/cloud-services-how-to-create-deploy).</span><span class="sxs-lookup"><span data-stu-id="1c16b-129">Create a Cloud Service, as described in [How to Create and Deploy a Cloud Service](https://docs.microsoft.com/azure/cloud-services/cloud-services-how-to-create-deploy).</span></span> <span data-ttu-id="1c16b-130">Führen Sie die Schritte im Abschnitt "Vorgehensweise: erstellen ein Cloud-Diensts verwenden der Schnellerfassung".</span><span class="sxs-lookup"><span data-stu-id="1c16b-130">Follow the steps in the section "How to: Create a cloud service using Quick Create".</span></span> <span data-ttu-id="1c16b-131">Für dieses Lernprogramm müssen Sie sich nicht um ein Zertifikat hochzuladen.</span><span class="sxs-lookup"><span data-stu-id="1c16b-131">For this tutorial, you do not need to upload a certificate.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image2.png)

<span data-ttu-id="1c16b-132">Erstellen Sie einen neuen Service Bus-Namespace ein, wie beschrieben in [wie auf verwenden Service Bus-Themen/-Abonnements](https://docs.microsoft.com/azure/service-bus-messaging/service-bus-dotnet-how-to-use-topics-subscriptions).</span><span class="sxs-lookup"><span data-stu-id="1c16b-132">Create a new Service Bus namespace, as described in [How to Use Service Bus Topics/Subscriptions](https://docs.microsoft.com/azure/service-bus-messaging/service-bus-dotnet-how-to-use-topics-subscriptions).</span></span> <span data-ttu-id="1c16b-133">Führen Sie die Schritte im Abschnitt "Erstellen einer Dienst-Namespace" aus.</span><span class="sxs-lookup"><span data-stu-id="1c16b-133">Follow the steps in the section "Create a Service Namespace".</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image3.png)

> [!NOTE]
> <span data-ttu-id="1c16b-134">Stellen Sie sicher, dass die gleiche Region für den Clouddienst und Service Bus-Namespace.</span><span class="sxs-lookup"><span data-stu-id="1c16b-134">Make sure to select the same region for the cloud service and the Service Bus namespace.</span></span>


## <a name="create-the-visual-studio-project"></a><span data-ttu-id="1c16b-135">Visual Studio-Projekt erstellen</span><span class="sxs-lookup"><span data-stu-id="1c16b-135">Create the Visual Studio Project</span></span>

<span data-ttu-id="1c16b-136">Starten Sie Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="1c16b-136">Start Visual Studio.</span></span> <span data-ttu-id="1c16b-137">Aus der **Datei** Menü klicken Sie auf **neues Projekt**.</span><span class="sxs-lookup"><span data-stu-id="1c16b-137">From the **File** menu, click **New Project**.</span></span>

<span data-ttu-id="1c16b-138">In der **neues Projekt** Dialogfeld erweitern Sie **Visual C#-**.</span><span class="sxs-lookup"><span data-stu-id="1c16b-138">In the **New Project** dialog box, expand **Visual C#**.</span></span> <span data-ttu-id="1c16b-139">Klicken Sie unter **installierte Vorlagen**Option **Cloud** und wählen Sie dann **Windows Azure-Cloud-Dienst**.</span><span class="sxs-lookup"><span data-stu-id="1c16b-139">Under **Installed Templates**, select **Cloud** and then select **Windows Azure Cloud Service**.</span></span> <span data-ttu-id="1c16b-140">Behalten Sie die Standardeinstellung .NET Framework 4.5.</span><span class="sxs-lookup"><span data-stu-id="1c16b-140">Keep the default .NET Framework 4.5.</span></span> <span data-ttu-id="1c16b-141">Benennen Sie die Anwendung ChatService, und klicken Sie auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="1c16b-141">Name the application ChatService and click **OK**.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image4.png)

<span data-ttu-id="1c16b-142">In der **neuen Windows Azure-Cloud-Dienst** Dialogfeld Wählen Sie ASP.NET MVC 4-Webrolle.</span><span class="sxs-lookup"><span data-stu-id="1c16b-142">In the **New Windows Azure Cloud Service** dialog, select ASP.NET MVC 4 Web Role.</span></span> <span data-ttu-id="1c16b-143">Klicken Sie auf den Pfeil nach rechts (**&gt;**), die der Projektmappe hinzuzufügen.</span><span class="sxs-lookup"><span data-stu-id="1c16b-143">Click the right-arrow button (**&gt;**) to add the role to your solution.</span></span>

<span data-ttu-id="1c16b-144">Bewegen Sie die Maus über die neue Rolle, also das Stiftsymbol angezeigt.</span><span class="sxs-lookup"><span data-stu-id="1c16b-144">Hover the mouse over the new role, so the pencil icon visible.</span></span> <span data-ttu-id="1c16b-145">Klicken Sie auf dieses Symbol, um die Rolle umbenennen.</span><span class="sxs-lookup"><span data-stu-id="1c16b-145">Click this icon to rename the role.</span></span> <span data-ttu-id="1c16b-146">Benennen Sie die Rolle "SignalRChat", und klicken Sie auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="1c16b-146">Name the role "SignalRChat" and click **OK**.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image5.png)

<span data-ttu-id="1c16b-147">In der **neues ASP.NET MVC 4-Projekt** Assistenten **Internetanwendung**.</span><span class="sxs-lookup"><span data-stu-id="1c16b-147">In the **New ASP.NET MVC 4 Project** wizard, select **Internet Application**.</span></span> <span data-ttu-id="1c16b-148">Klicken Sie auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="1c16b-148">Click **OK**.</span></span> <span data-ttu-id="1c16b-149">Der Assistent erstellt zwei Projekte:</span><span class="sxs-lookup"><span data-stu-id="1c16b-149">The project wizard creates two projects:</span></span>

- <span data-ttu-id="1c16b-150">ChatService: Dieses Projekt ist das Windows Azure-Anwendung.</span><span class="sxs-lookup"><span data-stu-id="1c16b-150">ChatService: This project is the Windows Azure application.</span></span> <span data-ttu-id="1c16b-151">Er definiert den Azure-Rollen und weitere Konfigurationsoptionen.</span><span class="sxs-lookup"><span data-stu-id="1c16b-151">It defines the Azure roles and other configuration options.</span></span>
- <span data-ttu-id="1c16b-152">SignalRChat: Dieses Projekt ist das ASP.NET MVC 4-Projekt.</span><span class="sxs-lookup"><span data-stu-id="1c16b-152">SignalRChat: This project is your ASP.NET MVC 4 project.</span></span>

## <a name="create-the-signalr-chat-application"></a><span data-ttu-id="1c16b-153">Die SignalR-Chat-Anwendung erstellen</span><span class="sxs-lookup"><span data-stu-id="1c16b-153">Create the SignalR Chat Application</span></span>

<span data-ttu-id="1c16b-154">Um die chatanwendung zu erstellen, führen Sie die Schritte im Lernprogramm [erste Schritte mit SignalR und MVC 4](tutorial-getting-started-with-signalr-and-mvc-4.md).</span><span class="sxs-lookup"><span data-stu-id="1c16b-154">To create the chat application, follow the steps in the tutorial [Getting Started with SignalR and MVC 4](tutorial-getting-started-with-signalr-and-mvc-4.md).</span></span>

<span data-ttu-id="1c16b-155">Installieren Sie die erforderlichen Bibliotheken mithilfe von NuGet.</span><span class="sxs-lookup"><span data-stu-id="1c16b-155">Use NuGet to install the required libraries.</span></span> <span data-ttu-id="1c16b-156">Aus der **Tools** klicken Sie im Menü **Bibliothekspaket-Manager**, und wählen Sie dann **Package Manager Console**.</span><span class="sxs-lookup"><span data-stu-id="1c16b-156">From the **Tools** menu, select **Library Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="1c16b-157">In der **Package Manager Console** Fenster, geben Sie die folgenden Befehle:</span><span class="sxs-lookup"><span data-stu-id="1c16b-157">In the **Package Manager Console** window, enter the following commands:</span></span>

[!code-powershell[Main](scaleout-with-windows-azure-service-bus/samples/sample2.ps1)]

<span data-ttu-id="1c16b-158">Verwenden der `-ProjectName` Option zum Installieren der Pakete anstelle der Windows Azure-Projekt das ASP.NET MVC-Projekt zur Verfügung.</span><span class="sxs-lookup"><span data-stu-id="1c16b-158">Use the `-ProjectName` option to install the packages to the ASP.NET MVC project, rather than the Windows Azure project.</span></span>

## <a name="configure-the-backplane"></a><span data-ttu-id="1c16b-159">Konfigurieren der Rückwand</span><span class="sxs-lookup"><span data-stu-id="1c16b-159">Configure the Backplane</span></span>

<span data-ttu-id="1c16b-160">Fügen Sie in Ihrer Anwendung Global.asax-Datei den folgenden Code hinzu:</span><span class="sxs-lookup"><span data-stu-id="1c16b-160">In your application's Global.asax file, add the following code:</span></span>

[!code-csharp[Main](scaleout-with-windows-azure-service-bus/samples/sample3.cs)]

<span data-ttu-id="1c16b-161">Nun müssen Sie die Servicebus-Verbindungszeichenfolge abzurufen.</span><span class="sxs-lookup"><span data-stu-id="1c16b-161">Now you need to get your service bus connection string.</span></span> <span data-ttu-id="1c16b-162">Wählen Sie im Azure-Portal den Servicebus-Namespace, den Sie erstellt haben, und klicken Sie auf das Symbol "Zugriffsschlüssel".</span><span class="sxs-lookup"><span data-stu-id="1c16b-162">In the Azure portal, select the service bus namespace that you created and click the Access Key icon.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image6.png)

<span data-ttu-id="1c16b-163">Kopieren Sie die Verbindungszeichenfolge in die Zwischenablage, und fügen Sie ihn in die *"ConnectionString"* Variable.</span><span class="sxs-lookup"><span data-stu-id="1c16b-163">Copy the connection string to the clipboard, then paste it into the *connectionString* variable.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image7.png)

[!code-csharp[Main](scaleout-with-windows-azure-service-bus/samples/sample4.cs)]

## <a name="deploy-to-azure"></a><span data-ttu-id="1c16b-164">In Azure bereitstellen</span><span class="sxs-lookup"><span data-stu-id="1c16b-164">Deploy to Azure</span></span>

<span data-ttu-id="1c16b-165">Erweitern Sie im Projektmappen-Explorer die **Rollen** Ordner im Projekt ChatService.</span><span class="sxs-lookup"><span data-stu-id="1c16b-165">In Solution Explorer, expand the **Roles** folder inside the ChatService project.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image8.png)

<span data-ttu-id="1c16b-166">Mit der rechten Maustaste in der Rolle "SignalRChat", und wählen Sie **Eigenschaften**.</span><span class="sxs-lookup"><span data-stu-id="1c16b-166">Right-click the SignalRChat role and select **Properties**.</span></span> <span data-ttu-id="1c16b-167">Wählen Sie die **Konfiguration** Registerkarte. Klicken Sie unter **Instanzen** 2 auswählen.</span><span class="sxs-lookup"><span data-stu-id="1c16b-167">Select the **Configuration** tab. Under **Instances** select 2.</span></span> <span data-ttu-id="1c16b-168">Sie können auch die Größe des virtuellen Computers festlegen, um **sehr klein**.</span><span class="sxs-lookup"><span data-stu-id="1c16b-168">You can also set the VM size to **Extra Small**.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image9.png)

<span data-ttu-id="1c16b-169">Speichern Sie die Änderungen.</span><span class="sxs-lookup"><span data-stu-id="1c16b-169">Save the changes.</span></span>

<span data-ttu-id="1c16b-170">Im Projektmappen-Explorer mit der rechten Maustaste des Projekts ChatService.</span><span class="sxs-lookup"><span data-stu-id="1c16b-170">In Solution Explorer, right-click the ChatService project.</span></span> <span data-ttu-id="1c16b-171">Wählen Sie **Veröffentlichen**.</span><span class="sxs-lookup"><span data-stu-id="1c16b-171">Select **Publish**.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image10.png)

<span data-ttu-id="1c16b-172">Wenn dies Ihre erste Zeit Veröffentlichungsvorgang in Windows Azure ist, müssen Sie Ihre Anmeldeinformationen herunterladen.</span><span class="sxs-lookup"><span data-stu-id="1c16b-172">If this is your first time publishing to Windows Azure, you must download your credentials.</span></span> <span data-ttu-id="1c16b-173">In der **veröffentlichen** -Assistenten klicken Sie auf "Sign in to download Credentials".</span><span class="sxs-lookup"><span data-stu-id="1c16b-173">In the **Publish** wizard, click "Sign in to download credentials".</span></span> <span data-ttu-id="1c16b-174">Dadurch werden Sie aufgefordert, sich bei Windows Azure-Portal anmelden und eine Datei mit veröffentlichungseinstellungen herunterladen.</span><span class="sxs-lookup"><span data-stu-id="1c16b-174">This will prompt you to sign into the Windows Azure portal and download a publish settings file.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image11.png)

<span data-ttu-id="1c16b-175">Klicken Sie auf **Import** , und wählen Sie die heruntergeladene Datei mit den veröffentlichungseinstellungen.</span><span class="sxs-lookup"><span data-stu-id="1c16b-175">Click **Import** and select the publish settings file that you downloaded.</span></span>

<span data-ttu-id="1c16b-176">Klicken Sie auf **Weiter**.</span><span class="sxs-lookup"><span data-stu-id="1c16b-176">Click **Next**.</span></span> <span data-ttu-id="1c16b-177">In der **Veröffentlichungseinstellungen** Dialogfeld unter **Cloud-Dienst**, wählen Sie den Clouddienst, die Sie zuvor erstellt haben.</span><span class="sxs-lookup"><span data-stu-id="1c16b-177">In the **Publish Settings** dialog, under **Cloud Service**, select the cloud service that you created earlier.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image12.png)

<span data-ttu-id="1c16b-178">Klicken Sie auf **Veröffentlichen**.</span><span class="sxs-lookup"><span data-stu-id="1c16b-178">Click **Publish**.</span></span> <span data-ttu-id="1c16b-179">Es dauert einige Minuten, bis die Anwendung bereitstellen und starten die virtuellen Computern.</span><span class="sxs-lookup"><span data-stu-id="1c16b-179">It can take a few minutes to deploy the application and start the VMs.</span></span>

<span data-ttu-id="1c16b-180">Beim Ausführen der chatanwendung kommunizieren die Rolleninstanzen jetzt über Azure Service Bus mithilfe eines Service Bus-Themas.</span><span class="sxs-lookup"><span data-stu-id="1c16b-180">Now when you run the chat application, the role instances communicate through Azure Service Bus, using a Service Bus topic.</span></span> <span data-ttu-id="1c16b-181">Ein Thema ist eine Warteschlange, die mehrere Abonnenten ermöglicht.</span><span class="sxs-lookup"><span data-stu-id="1c16b-181">A topic is a message queue that allows multiple subscribers.</span></span>

<span data-ttu-id="1c16b-182">Der Rückwand erstellt automatisch das Thema und die Abonnements.</span><span class="sxs-lookup"><span data-stu-id="1c16b-182">The backplane automatically creates the topic and the subscriptions.</span></span> <span data-ttu-id="1c16b-183">Um das Abonnement und die Nachrichtenaktivität anzuzeigen, öffnen Sie das Azure Portal, wählen Sie den Service Bus-Namespace, und klicken Sie auf "Themen".</span><span class="sxs-lookup"><span data-stu-id="1c16b-183">To see the subscriptions and message activity, open the Azure portal, select the Service Bus namespace, and click on "Topics".</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image13.png)

<span data-ttu-id="1c16b-184">Dafür können einige Minuten, bis die Nachrichtenaktivität im Dashboard angezeigt werden.</span><span class="sxs-lookup"><span data-stu-id="1c16b-184">It make take a few minutes for the message activity to show up in the dashboard.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image14.png)

<span data-ttu-id="1c16b-185">SignalR verwaltet die Lebensdauer des Themas.</span><span class="sxs-lookup"><span data-stu-id="1c16b-185">SignalR manages the topic lifetime.</span></span> <span data-ttu-id="1c16b-186">Solange die Anwendung bereitgestellt wurde, versuchen Sie nicht manuell Themen löschen oder Ändern von Einstellungen für das Thema.</span><span class="sxs-lookup"><span data-stu-id="1c16b-186">As long as your application is deployed, don't try to manually delete topics or change settings on the topic.</span></span>
