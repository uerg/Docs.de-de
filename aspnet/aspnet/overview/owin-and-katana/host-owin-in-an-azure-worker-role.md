---
uid: aspnet/overview/owin-and-katana/host-owin-in-an-azure-worker-role
title: Hosten in einer Azure-Workerrolle OWIN | Microsoft Docs
author: MikeWasson
description: "Dieses Lernprogramm zeigt, wie OWIN selbst in einer Microsoft Azure-workerrolle hosten. Open Web-Schnittstelle für .NET (OWIN) definiert eine Abstraktion zwischen .NET Webserver..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/11/2014
ms.topic: article
ms.assetid: 07aa855a-92ee-4d43-ba66-5bfd7de20ee6
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/owin-and-katana/host-owin-in-an-azure-worker-role
msc.type: authoredcontent
ms.openlocfilehash: 8c0fdfdf60ff3bde34b6869adf3f8693b4d9615d
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/24/2018
---
<a name="host-owin-in-an-azure-worker-role"></a><span data-ttu-id="3b197-104">Host in einer Azure-Workerrolle OWIN</span><span class="sxs-lookup"><span data-stu-id="3b197-104">Host OWIN in an Azure Worker Role</span></span>
====================
<span data-ttu-id="3b197-105">durch [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="3b197-105">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

> <span data-ttu-id="3b197-106">Dieses Lernprogramm zeigt, wie OWIN selbst in einer Microsoft Azure-workerrolle hosten.</span><span class="sxs-lookup"><span data-stu-id="3b197-106">This tutorial shows how to self-host OWIN in a Microsoft Azure worker role.</span></span>
> 
> <span data-ttu-id="3b197-107">[Öffnen Sie die Weboberfläche für .NET](http://owin.org/) (OWIN) definiert eine Abstraktion zwischen .NET Webservern und Webanwendungen.</span><span class="sxs-lookup"><span data-stu-id="3b197-107">[Open Web Interface for .NET](http://owin.org/) (OWIN) defines an abstraction between .NET web servers and web applications.</span></span> <span data-ttu-id="3b197-108">OWIN entkoppelt die Webanwendung aus dem Server, wodurch OWIN ideal für eine Webanwendung in Ihrem eigenen Prozess, außerhalb von IIS-Self-hosting – z. B. in einer Azure-workerrolle.</span><span class="sxs-lookup"><span data-stu-id="3b197-108">OWIN decouples the web application from the server, which makes OWIN ideal for self-hosting a web application in your own process, outside of IIS–for example, inside an Azure worker role.</span></span>
> 
> <span data-ttu-id="3b197-109">In diesem Lernprogramm erfahren Sie, wie einer OWIN-Anwendung in eine Microsoft Azure-Worker-Rolle selbst gehostet.</span><span class="sxs-lookup"><span data-stu-id="3b197-109">In this tutorial, you'll learn how to self-host an OWIN applications inside a Microsoft Azure worker role.</span></span> <span data-ttu-id="3b197-110">Weitere Informationen zu Workerrollen finden Sie unter [Azure-Ausführungsmodelle](https://azure.microsoft.com/documentation/articles/fundamentals-application-models/#CloudServices).</span><span class="sxs-lookup"><span data-stu-id="3b197-110">To learn more about worker roles, see [Azure Execution Models](https://azure.microsoft.com/documentation/articles/fundamentals-application-models/#CloudServices).</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="3b197-111">In diesem Lernprogramm verwendeten Versionen der Software</span><span class="sxs-lookup"><span data-stu-id="3b197-111">Software versions used in the tutorial</span></span>
> 
> 
> - [<span data-ttu-id="3b197-112">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="3b197-112">Visual Studio 2013</span></span>](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - [<span data-ttu-id="3b197-113">Azure SDK für .NET 2.3</span><span class="sxs-lookup"><span data-stu-id="3b197-113">Azure SDK for .NET 2.3</span></span>](https://azure.microsoft.com/downloads/)
> - [<span data-ttu-id="3b197-114">Microsoft.Owin.Selfhost 2.1.0</span><span class="sxs-lookup"><span data-stu-id="3b197-114">Microsoft.Owin.Selfhost 2.1.0</span></span>](http://www.nuget.org/packages/Microsoft.Owin.SelfHost/2.1.0)


## <a name="create-a-microsoft-azure-project"></a><span data-ttu-id="3b197-115">Erstellen Sie ein Microsoft Azure-Projekt</span><span class="sxs-lookup"><span data-stu-id="3b197-115">Create a Microsoft Azure Project</span></span>

<span data-ttu-id="3b197-116">Starten Sie Visual Studio mit Administratorrechten aus.</span><span class="sxs-lookup"><span data-stu-id="3b197-116">Start Visual Studio with administrator privileges.</span></span> <span data-ttu-id="3b197-117">Administratorrechte sind erforderlich, um die Anwendung lokal debuggen, mit dem Azure-Serveremulator.</span><span class="sxs-lookup"><span data-stu-id="3b197-117">Administrator privileges are needed to debug the application locally, using the Azure compute emulator.</span></span>

<span data-ttu-id="3b197-118">Auf der **Datei** Menü klicken Sie auf **neu**, klicken Sie dann auf **Projekt**.</span><span class="sxs-lookup"><span data-stu-id="3b197-118">On the **File** menu, click **New**, then click **Project**.</span></span> <span data-ttu-id="3b197-119">Von **installierte Vorlagen**, klicken Sie unter Visual C#- **Cloud** , und klicken Sie dann auf **Windows Azure-Cloud-Dienst**.</span><span class="sxs-lookup"><span data-stu-id="3b197-119">From **Installed Templates**, under Visual C#, click **Cloud** and then click **Windows Azure Cloud Service**.</span></span> <span data-ttu-id="3b197-120">Nennen Sie das Projekt "AzureApp", und klicken Sie auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="3b197-120">Name the project "AzureApp" and click **OK**.</span></span>

[![](host-owin-in-an-azure-worker-role/_static/image2.png)](host-owin-in-an-azure-worker-role/_static/image1.png)

<span data-ttu-id="3b197-121">In der **neuen Windows Azure-Cloud-Dienst** Dialogfeld, doppelklicken Sie auf **Workerrolle**.</span><span class="sxs-lookup"><span data-stu-id="3b197-121">In the **New Windows Azure Cloud Service** dialog, double-click **Worker Role**.</span></span> <span data-ttu-id="3b197-122">Lassen Sie den Standardnamen ("WorkerRole1").</span><span class="sxs-lookup"><span data-stu-id="3b197-122">Leave the default name ("WorkerRole1").</span></span> <span data-ttu-id="3b197-123">Dieser Schritt wird der Projektmappe eine workerrolle hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="3b197-123">This step adds a worker role to the solution.</span></span> <span data-ttu-id="3b197-124">Klicken Sie auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="3b197-124">Click **OK**.</span></span>

[![](host-owin-in-an-azure-worker-role/_static/image4.png)](host-owin-in-an-azure-worker-role/_static/image3.png)

<span data-ttu-id="3b197-125">Visual Studio-Projektmappe, die erstellt wird, enthält zwei Projekte:</span><span class="sxs-lookup"><span data-stu-id="3b197-125">The Visual Studio solution that is created contains two projects:</span></span>

- <span data-ttu-id="3b197-126">&quot;AzureApp&quot; definiert die Rollen und die Konfiguration für die Azure-Anwendung.</span><span class="sxs-lookup"><span data-stu-id="3b197-126">&quot;AzureApp&quot; defines the roles and configuration for the Azure application.</span></span>
- <span data-ttu-id="3b197-127">&quot;WorkerRole1&quot; enthält den Code für die workerrolle.</span><span class="sxs-lookup"><span data-stu-id="3b197-127">&quot;WorkerRole1&quot; contains the code for the worker role.</span></span>

<span data-ttu-id="3b197-128">Im Allgemeinen kann eine Azure-Anwendung mehrere Rollen enthalten, obwohl in diesem Lernprogramm eine einzige Rolle verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="3b197-128">In general, an Azure application can contain multiple roles, although this tutorial uses a single role.</span></span>

![](host-owin-in-an-azure-worker-role/_static/image5.png)

## <a name="add-the-owin-self-host-packages"></a><span data-ttu-id="3b197-129">Hinzufügen der Pakete für die OWIN-Selbsthosting</span><span class="sxs-lookup"><span data-stu-id="3b197-129">Add the OWIN Self-Host Packages</span></span>

<span data-ttu-id="3b197-130">Aus der **Tools** Menü klicken Sie auf **Bibliothekspaket-Manager**, klicken Sie dann auf **Package Manager Console**.</span><span class="sxs-lookup"><span data-stu-id="3b197-130">From the **Tools** menu, click **Library Package Manager**, then click **Package Manager Console**.</span></span>

<span data-ttu-id="3b197-131">Geben Sie im Fenster Paket-Manager-Konsole den folgenden Befehl aus:</span><span class="sxs-lookup"><span data-stu-id="3b197-131">In the Package Manager Console window, enter the following command:</span></span>

[!code-console[Main](host-owin-in-an-azure-worker-role/samples/sample1.cmd)]

## <a name="add-an-http-endpoint"></a><span data-ttu-id="3b197-132">Fügen Sie einen HTTP-Endpunkt</span><span class="sxs-lookup"><span data-stu-id="3b197-132">Add an HTTP Endpoint</span></span>

<span data-ttu-id="3b197-133">Erweitern Sie im Projektmappen-Explorer das Projekt AzureApp.</span><span class="sxs-lookup"><span data-stu-id="3b197-133">In Solution Explorer, expand the AzureApp project.</span></span> <span data-ttu-id="3b197-134">Erweitern Sie den Knoten Rollen, mit der rechten Maustaste WorkerRole1, und wählen **Eigenschaften**.</span><span class="sxs-lookup"><span data-stu-id="3b197-134">Expand the Roles node, right-click WorkerRole1, and select **Properties**.</span></span>

![](host-owin-in-an-azure-worker-role/_static/image6.png)

<span data-ttu-id="3b197-135">Klicken Sie auf **Endpunkte**, und klicken Sie dann auf **Add Endpoint**.</span><span class="sxs-lookup"><span data-stu-id="3b197-135">Click **Endpoints**, and then click **Add Endpoint**.</span></span>

<span data-ttu-id="3b197-136">In der **Protokoll** Dropdown-Liste, wählen Sie "http".</span><span class="sxs-lookup"><span data-stu-id="3b197-136">In the **Protocol** dropdown list, select "http".</span></span> <span data-ttu-id="3b197-137">In **öffentlicher Port** und **privater Port**, geben den Wert 80.</span><span class="sxs-lookup"><span data-stu-id="3b197-137">In **Public Port** and **Private Port**, type 80.</span></span> <span data-ttu-id="3b197-138">Diese Portnummern können unterschiedlich sein.</span><span class="sxs-lookup"><span data-stu-id="3b197-138">These port numbers can be different.</span></span> <span data-ttu-id="3b197-139">Der öffentliche Port wird von Clients verwendet, beim Versand einer Anforderungs in die Rolle.</span><span class="sxs-lookup"><span data-stu-id="3b197-139">The public port is what clients use when they send a request to the role.</span></span>

[![](host-owin-in-an-azure-worker-role/_static/image8.png)](host-owin-in-an-azure-worker-role/_static/image7.png)

## <a name="create-the-owin-startup-class"></a><span data-ttu-id="3b197-140">Erstellen Sie die OWIN-Start-Klasse</span><span class="sxs-lookup"><span data-stu-id="3b197-140">Create the OWIN Startup Class</span></span>

<span data-ttu-id="3b197-141">Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste, klicken Sie auf das Projekt WorkerRole1, und wählen **hinzufügen** / **Klasse** So fügen Sie eine neue Klasse hinzu.</span><span class="sxs-lookup"><span data-stu-id="3b197-141">In Solution Explorer, right click the WorkerRole1 project and select **Add** / **Class** to add a new class.</span></span> <span data-ttu-id="3b197-142">Nennen Sie die Klasse `Startup`.</span><span class="sxs-lookup"><span data-stu-id="3b197-142">Name the class `Startup`.</span></span>

<span data-ttu-id="3b197-143">Ersetzen Sie alle den Standardcode durch Folgendes:</span><span class="sxs-lookup"><span data-stu-id="3b197-143">Replace all of the boilerplate code with the following:</span></span>

[!code-csharp[Main](host-owin-in-an-azure-worker-role/samples/sample2.cs)]

<span data-ttu-id="3b197-144">Die `UseWelcomePage` Erweiterungsmethode Fügt eine einfache HTML-Seite für Ihre Anwendung, um zu überprüfen, ob der Standort funktioniert.</span><span class="sxs-lookup"><span data-stu-id="3b197-144">The `UseWelcomePage` extension method adds a simple HTML page to your application, to verify the site is working.</span></span>

## <a name="start-the-owin-host"></a><span data-ttu-id="3b197-145">Starten Sie den OWIN-Host</span><span class="sxs-lookup"><span data-stu-id="3b197-145">Start the OWIN Host</span></span>

<span data-ttu-id="3b197-146">Öffnen der Datei WorkerRole.cs hinzu.</span><span class="sxs-lookup"><span data-stu-id="3b197-146">Open the WorkerRole.cs file.</span></span> <span data-ttu-id="3b197-147">Diese Klasse definiert den Code, der ausgeführt wird, wenn die workerrolle gestartet oder beendet wird.</span><span class="sxs-lookup"><span data-stu-id="3b197-147">This class defines the code that runs when the worker role is started and stopped.</span></span>

<span data-ttu-id="3b197-148">Fügen Sie die folgende Anweisung:</span><span class="sxs-lookup"><span data-stu-id="3b197-148">Add the following using statement:</span></span>

[!code-csharp[Main](host-owin-in-an-azure-worker-role/samples/sample3.cs)]

<span data-ttu-id="3b197-149">Hinzufügen einer **IDisposable** Member der `WorkerRole` Klasse:</span><span class="sxs-lookup"><span data-stu-id="3b197-149">Add an **IDisposable** member to the `WorkerRole` class:</span></span>

[!code-csharp[Main](host-owin-in-an-azure-worker-role/samples/sample4.cs)]

<span data-ttu-id="3b197-150">In der `OnStart` -Methode, fügen Sie den folgenden Code zum Starten des Hosts hinzu:</span><span class="sxs-lookup"><span data-stu-id="3b197-150">In the `OnStart` method, add the following code to start the host:</span></span>

[!code-csharp[Main](host-owin-in-an-azure-worker-role/samples/sample5.cs?highlight=5)]

<span data-ttu-id="3b197-151">Die **WebApp.Start** Methode startet den OWIN-Host.</span><span class="sxs-lookup"><span data-stu-id="3b197-151">The **WebApp.Start** method starts the OWIN host.</span></span> <span data-ttu-id="3b197-152">Der Name des der `Startup` Klasse ist ein Typparameter der Methode.</span><span class="sxs-lookup"><span data-stu-id="3b197-152">The name of the `Startup` class is a type parameter to the method.</span></span> <span data-ttu-id="3b197-153">Gemäß der Konvention werden der Host Ruft die `Configure` Methode dieser Klasse.</span><span class="sxs-lookup"><span data-stu-id="3b197-153">By convention, the host will call the `Configure` method of this class.</span></span>

<span data-ttu-id="3b197-154">Überschreiben Sie die `OnStop` Löschen der  *\_app* Instanz:</span><span class="sxs-lookup"><span data-stu-id="3b197-154">Override the `OnStop` to dispose of the *\_app* instance:</span></span>

[!code-csharp[Main](host-owin-in-an-azure-worker-role/samples/sample6.cs)]

<span data-ttu-id="3b197-155">Hier wird der vollständige Code für WorkerRole.cs:</span><span class="sxs-lookup"><span data-stu-id="3b197-155">Here is the complete code for WorkerRole.cs:</span></span>

[!code-csharp[Main](host-owin-in-an-azure-worker-role/samples/sample7.cs)]

<span data-ttu-id="3b197-156">Erstellen Sie die Projektmappe, und drücken Sie F5, um die Anwendung lokal im Azure-Serveremulator auszuführen.</span><span class="sxs-lookup"><span data-stu-id="3b197-156">Build the solution, and press F5 to run the application locally in the Azure Compute Emulator.</span></span> <span data-ttu-id="3b197-157">Abhängig von Ihren Firewalleinstellungen müssen Sie möglicherweise den Emulator über die Firewall zulassen.</span><span class="sxs-lookup"><span data-stu-id="3b197-157">Depending on your firewall settings, you might need to allow the emulator through your firewall.</span></span>

<span data-ttu-id="3b197-158">Der Serveremulator weist eine lokale IP-Adresse an den Endpunkt an.</span><span class="sxs-lookup"><span data-stu-id="3b197-158">The compute emulator assigns a local IP address to the endpoint.</span></span> <span data-ttu-id="3b197-159">Sie können die IP-Adresse finden, indem Sie den Serveremulator-Benutzeroberfläche anzeigen.</span><span class="sxs-lookup"><span data-stu-id="3b197-159">You can find the IP address by viewing the Compute Emulator UI.</span></span> <span data-ttu-id="3b197-160">Maustaste auf das Symbol "Emulator" in den Infobereich der Taskleiste, und wählen Sie **Serveremulator-Benutzeroberfläche anzeigen**.</span><span class="sxs-lookup"><span data-stu-id="3b197-160">Right-click the emulator icon in the task bar notification area, and select **Show Compute Emulator UI**.</span></span>

[![](host-owin-in-an-azure-worker-role/_static/image10.png)](host-owin-in-an-azure-worker-role/_static/image9.png)

<span data-ttu-id="3b197-161">Suchen Sie die IP-Adresse unter Dienstbereitstellungen, Bereitstellung [Id], Dienstdetails.</span><span class="sxs-lookup"><span data-stu-id="3b197-161">Find the IP address under Service Deployments, deployment [id], Service Details.</span></span> <span data-ttu-id="3b197-162">Öffnen Sie einen Webbrowser, und navigieren Sie zu http://*Adresse*, wobei *Adresse* ist die IP-Adresse zugewiesen, die vom Serveremulator; z. B. `http://127.0.0.1:80`.</span><span class="sxs-lookup"><span data-stu-id="3b197-162">Open a web browser and navigate to http://*address*, where *address* is the IP address assigned by the compute emulator; for example, `http://127.0.0.1:80`.</span></span> <span data-ttu-id="3b197-163">Die OWIN-Willkommensseite sollte angezeigt werden:</span><span class="sxs-lookup"><span data-stu-id="3b197-163">You should see the OWIN welcome page:</span></span>

![](host-owin-in-an-azure-worker-role/_static/image11.png)

## <a name="deploy-to-azure"></a><span data-ttu-id="3b197-164">In Azure bereitstellen</span><span class="sxs-lookup"><span data-stu-id="3b197-164">Deploy to Azure</span></span>

<span data-ttu-id="3b197-165">Für diesen Schritt müssen Sie ein Azure-Konto verfügen.</span><span class="sxs-lookup"><span data-stu-id="3b197-165">For this step, you must have an Azure account.</span></span> <span data-ttu-id="3b197-166">Wenn Sie einen noch nicht haben, können Sie in wenigen Minuten ein kostenloses Testkonto erstellen.</span><span class="sxs-lookup"><span data-stu-id="3b197-166">If you don't already have one, you can create a free trial account in just a couple of minutes.</span></span> <span data-ttu-id="3b197-167">Weitere Informationen finden Sie unter [kostenlosen Microsoft Azure-Testversion](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F).</span><span class="sxs-lookup"><span data-stu-id="3b197-167">For details, see [Microsoft Azure Free Trial](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F).</span></span>

<span data-ttu-id="3b197-168">Im Projektmappen-Explorer mit der rechten Maustaste des Projekts AzureApp.</span><span class="sxs-lookup"><span data-stu-id="3b197-168">In Solution Explorer, right-click the AzureApp project.</span></span> <span data-ttu-id="3b197-169">Wählen Sie **Veröffentlichen**.</span><span class="sxs-lookup"><span data-stu-id="3b197-169">Select **Publish**.</span></span>

![](host-owin-in-an-azure-worker-role/_static/image12.png)

<span data-ttu-id="3b197-170">Wenn Sie nicht beim Azure-Konto angemeldet sind, klicken Sie auf **anmelden**.</span><span class="sxs-lookup"><span data-stu-id="3b197-170">If you are not signed in to your Azure account, click **Sign In**.</span></span>

[![](host-owin-in-an-azure-worker-role/_static/image14.png)](host-owin-in-an-azure-worker-role/_static/image13.png)

<span data-ttu-id="3b197-171">Nachdem Sie sich angemeldet haben, wählen Sie ein Abonnement, und klicken Sie auf **Weiter**.</span><span class="sxs-lookup"><span data-stu-id="3b197-171">After you are signed in, choose a subscription and click **Next**.</span></span>

[![](host-owin-in-an-azure-worker-role/_static/image16.png)](host-owin-in-an-azure-worker-role/_static/image15.png)

<span data-ttu-id="3b197-172">Geben Sie einen Namen für den Clouddienst, und wählen Sie eine Region aus.</span><span class="sxs-lookup"><span data-stu-id="3b197-172">Enter a name for the cloud service and choose a region.</span></span> <span data-ttu-id="3b197-173">Klicken Sie auf **Erstellen**.</span><span class="sxs-lookup"><span data-stu-id="3b197-173">Click **Create**.</span></span>

![](host-owin-in-an-azure-worker-role/_static/image17.png)

<span data-ttu-id="3b197-174">Klicken Sie auf **Veröffentlichen**.</span><span class="sxs-lookup"><span data-stu-id="3b197-174">Click **Publish**.</span></span>

[![](host-owin-in-an-azure-worker-role/_static/image19.png)](host-owin-in-an-azure-worker-role/_static/image18.png)

<span data-ttu-id="3b197-175">Das Azure-Aktivitätsprotokoll-Fenster wird der Fortschritt der Bereitstellung.</span><span class="sxs-lookup"><span data-stu-id="3b197-175">The Azure Activity Log window shows the progress of the deployment.</span></span> <span data-ttu-id="3b197-176">Wenn die app bereitgestellt wurde, navigieren Sie zu `http://appname.cloudapp.net/`, wobei *Appname* ist der Name des Cloud-Diensts.</span><span class="sxs-lookup"><span data-stu-id="3b197-176">When the app is deployed, browse to `http://appname.cloudapp.net/`, where *appname* is the name of your cloud service.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="3b197-177">Zusätzliche Ressourcen</span><span class="sxs-lookup"><span data-stu-id="3b197-177">Additional Resources</span></span>

- [<span data-ttu-id="3b197-178">Übersicht über das Katana-Projekt</span><span class="sxs-lookup"><span data-stu-id="3b197-178">An Overview of Project Katana</span></span>](an-overview-of-project-katana.md)
- [<span data-ttu-id="3b197-179">Katana-Projekt auf GitHub</span><span class="sxs-lookup"><span data-stu-id="3b197-179">Katana Project on GitHub</span></span>](https://github.com/aspnet/AspNetKatana/)
