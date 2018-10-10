---
uid: aspnet/overview/owin-and-katana/host-owin-in-an-azure-worker-role
title: Hosten von OWIN in einer Azure-Workerrolle | Microsoft-Dokumentation
author: MikeWasson
description: In diesem Tutorial wird gezeigt, wie OWIN selbst in einer Microsoft Azure-workerrolle gehostet wird. Open Web Interface for .NET (OWIN) definiert eine Abstraktion zwischen .NET Webserver...
ms.author: riande
ms.date: 04/11/2014
ms.assetid: 07aa855a-92ee-4d43-ba66-5bfd7de20ee6
msc.legacyurl: /aspnet/overview/owin-and-katana/host-owin-in-an-azure-worker-role
msc.type: authoredcontent
ms.openlocfilehash: dbf0964695dd2592d063b05c0778923edffe8e2e
ms.sourcegitcommit: a4dcca4f1cb81227c5ed3c92dc0e28be6e99447b
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/10/2018
ms.locfileid: "48910772"
---
<a name="host-owin-in-an-azure-worker-role"></a><span data-ttu-id="0d4d7-104">Hosten von OWIN in einer Azure-Workerrolle</span><span class="sxs-lookup"><span data-stu-id="0d4d7-104">Host OWIN in an Azure Worker Role</span></span>
====================
<span data-ttu-id="0d4d7-105">durch [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="0d4d7-105">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

> <span data-ttu-id="0d4d7-106">In diesem Tutorial wird gezeigt, wie OWIN selbst in einer Microsoft Azure-workerrolle gehostet wird.</span><span class="sxs-lookup"><span data-stu-id="0d4d7-106">This tutorial shows how to self-host OWIN in a Microsoft Azure worker role.</span></span>
>
> <span data-ttu-id="0d4d7-107">[Öffnen von Weboberfläche für .NET](http://owin.org/) (OWIN) definiert eine Abstraktion zwischen Webservern für .NET und Webanwendungen.</span><span class="sxs-lookup"><span data-stu-id="0d4d7-107">[Open Web Interface for .NET](http://owin.org/) (OWIN) defines an abstraction between .NET web servers and web applications.</span></span> <span data-ttu-id="0d4d7-108">OWIN entkoppelt die Webanwendung aus dem Server, wodurch OWIN ideal für eine Webanwendung in Ihrem eigenen Prozess, außerhalb von IIS Self-hosting – z. B. in einer Azure-workerrolle.</span><span class="sxs-lookup"><span data-stu-id="0d4d7-108">OWIN decouples the web application from the server, which makes OWIN ideal for self-hosting a web application in your own process, outside of IIS–for example, inside an Azure worker role.</span></span>
>
> <span data-ttu-id="0d4d7-109">In diesem Tutorial erfahren Sie, wie Sie Self-Hosting einer OWIN-Anwendung in einer Microsoft Azure-workerrolle.</span><span class="sxs-lookup"><span data-stu-id="0d4d7-109">In this tutorial, you'll learn how to self-host an OWIN applications inside a Microsoft Azure worker role.</span></span> <span data-ttu-id="0d4d7-110">Weitere Informationen zu Workerrollen finden Sie unter [Azure-Ausführungsmodelle](https://azure.microsoft.com/documentation/articles/fundamentals-application-models/#CloudServices).</span><span class="sxs-lookup"><span data-stu-id="0d4d7-110">To learn more about worker roles, see [Azure Execution Models](https://azure.microsoft.com/documentation/articles/fundamentals-application-models/#CloudServices).</span></span>
>
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="0d4d7-111">Softwareversionen, die in diesem Tutorial verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="0d4d7-111">Software versions used in the tutorial</span></span>
>
>
> - [<span data-ttu-id="0d4d7-112">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="0d4d7-112">Visual Studio 2013</span></span>](https://my.visualstudio.com/Downloads?q=visual%20studio%202013)
> - [<span data-ttu-id="0d4d7-113">Azure SDK für .NET 2.3</span><span class="sxs-lookup"><span data-stu-id="0d4d7-113">Azure SDK for .NET 2.3</span></span>](https://azure.microsoft.com/downloads/)
> - [<span data-ttu-id="0d4d7-114">Microsoft.Owin.Selfhost 2.1.0</span><span class="sxs-lookup"><span data-stu-id="0d4d7-114">Microsoft.Owin.Selfhost 2.1.0</span></span>](http://www.nuget.org/packages/Microsoft.Owin.SelfHost/2.1.0)


## <a name="create-a-microsoft-azure-project"></a><span data-ttu-id="0d4d7-115">Erstellen Sie ein Microsoft Azure-Projekt</span><span class="sxs-lookup"><span data-stu-id="0d4d7-115">Create a Microsoft Azure Project</span></span>

<span data-ttu-id="0d4d7-116">Starten Sie Visual Studio mit Administratorrechten aus.</span><span class="sxs-lookup"><span data-stu-id="0d4d7-116">Start Visual Studio with administrator privileges.</span></span> <span data-ttu-id="0d4d7-117">Administratorberechtigungen sind erforderlich, um die Anwendung lokal zu debuggen, mit dem Azure-Serveremulator.</span><span class="sxs-lookup"><span data-stu-id="0d4d7-117">Administrator privileges are needed to debug the application locally, using the Azure compute emulator.</span></span>

<span data-ttu-id="0d4d7-118">Auf der **Datei** Menü klicken Sie auf **neu**, klicken Sie dann auf **Projekt**.</span><span class="sxs-lookup"><span data-stu-id="0d4d7-118">On the **File** menu, click **New**, then click **Project**.</span></span> <span data-ttu-id="0d4d7-119">Von **installierte Vorlagen**, klicken Sie unter Visual c# **Cloud** , und klicken Sie dann auf **Windows Azure Cloud Service**.</span><span class="sxs-lookup"><span data-stu-id="0d4d7-119">From **Installed Templates**, under Visual C#, click **Cloud** and then click **Windows Azure Cloud Service**.</span></span> <span data-ttu-id="0d4d7-120">Nennen Sie das Projekt "AzureApp", und klicken Sie auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="0d4d7-120">Name the project "AzureApp" and click **OK**.</span></span>

[![](host-owin-in-an-azure-worker-role/_static/image2.png)](host-owin-in-an-azure-worker-role/_static/image1.png)

<span data-ttu-id="0d4d7-121">In der **neuen Windows Azure-Cloud-Dienst** Dialogfeld, doppelklicken Sie auf **Workerrolle**.</span><span class="sxs-lookup"><span data-stu-id="0d4d7-121">In the **New Windows Azure Cloud Service** dialog, double-click **Worker Role**.</span></span> <span data-ttu-id="0d4d7-122">Übernehmen Sie den Standardnamen ("WorkerRole1") aus.</span><span class="sxs-lookup"><span data-stu-id="0d4d7-122">Leave the default name ("WorkerRole1").</span></span> <span data-ttu-id="0d4d7-123">Dieser Schritt wird der Projektmappe eine Worker-Rolle hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="0d4d7-123">This step adds a worker role to the solution.</span></span> <span data-ttu-id="0d4d7-124">Klicken Sie auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="0d4d7-124">Click **OK**.</span></span>

[![](host-owin-in-an-azure-worker-role/_static/image4.png)](host-owin-in-an-azure-worker-role/_static/image3.png)

<span data-ttu-id="0d4d7-125">Visual Studio-Projektmappe, die erstellt wird, enthält zwei Projekte:</span><span class="sxs-lookup"><span data-stu-id="0d4d7-125">The Visual Studio solution that is created contains two projects:</span></span>

- <span data-ttu-id="0d4d7-126">&quot;AzureApp&quot; definiert die Rollen und die Konfiguration für die Azure-Anwendung.</span><span class="sxs-lookup"><span data-stu-id="0d4d7-126">&quot;AzureApp&quot; defines the roles and configuration for the Azure application.</span></span>
- <span data-ttu-id="0d4d7-127">&quot;WorkerRole1&quot; enthält den Code für die Worker-Rolle.</span><span class="sxs-lookup"><span data-stu-id="0d4d7-127">&quot;WorkerRole1&quot; contains the code for the worker role.</span></span>

<span data-ttu-id="0d4d7-128">Im Allgemeinen kann eine Azure-Anwendung mehrere Rollen enthalten, obwohl in diesem Tutorial eine einzelne Rolle verwendet.</span><span class="sxs-lookup"><span data-stu-id="0d4d7-128">In general, an Azure application can contain multiple roles, although this tutorial uses a single role.</span></span>

![](host-owin-in-an-azure-worker-role/_static/image5.png)

## <a name="add-the-owin-self-host-packages"></a><span data-ttu-id="0d4d7-129">Fügen Sie die Self-Hosting-Pakete für OWIN</span><span class="sxs-lookup"><span data-stu-id="0d4d7-129">Add the OWIN Self-Host Packages</span></span>

<span data-ttu-id="0d4d7-130">Von der **Tools** Menü klicken Sie auf **NuGet Package Manager**, klicken Sie dann auf **-Paket-Manager-Konsole**.</span><span class="sxs-lookup"><span data-stu-id="0d4d7-130">From the **Tools** menu, click **NuGet Package Manager**, then click **Package Manager Console**.</span></span>

<span data-ttu-id="0d4d7-131">Geben Sie im Fenster Paket-Manager-Konsole den folgenden Befehl aus:</span><span class="sxs-lookup"><span data-stu-id="0d4d7-131">In the Package Manager Console window, enter the following command:</span></span>

[!code-console[Main](host-owin-in-an-azure-worker-role/samples/sample1.cmd)]

## <a name="add-an-http-endpoint"></a><span data-ttu-id="0d4d7-132">Fügen Sie einen HTTP-Endpunkt hinzu.</span><span class="sxs-lookup"><span data-stu-id="0d4d7-132">Add an HTTP Endpoint</span></span>

<span data-ttu-id="0d4d7-133">Erweitern Sie im Projektmappen-Explorer das Projekt AzureApp aus.</span><span class="sxs-lookup"><span data-stu-id="0d4d7-133">In Solution Explorer, expand the AzureApp project.</span></span> <span data-ttu-id="0d4d7-134">Erweitern Sie den Knoten "Rollen", mit der rechten Maustaste WorkerRole1, und wählen **Eigenschaften**.</span><span class="sxs-lookup"><span data-stu-id="0d4d7-134">Expand the Roles node, right-click WorkerRole1, and select **Properties**.</span></span>

![](host-owin-in-an-azure-worker-role/_static/image6.png)

<span data-ttu-id="0d4d7-135">Klicken Sie auf **Endpunkte**, und klicken Sie dann auf **Add Endpoint**.</span><span class="sxs-lookup"><span data-stu-id="0d4d7-135">Click **Endpoints**, and then click **Add Endpoint**.</span></span>

<span data-ttu-id="0d4d7-136">In der **Protokoll** Dropdown-Liste, Option "http".</span><span class="sxs-lookup"><span data-stu-id="0d4d7-136">In the **Protocol** dropdown list, select "http".</span></span> <span data-ttu-id="0d4d7-137">In **öffentlicher Port** und **privater Port**, geben Sie 80.</span><span class="sxs-lookup"><span data-stu-id="0d4d7-137">In **Public Port** and **Private Port**, type 80.</span></span> <span data-ttu-id="0d4d7-138">Diese Portnummern können sich unterscheiden.</span><span class="sxs-lookup"><span data-stu-id="0d4d7-138">These port numbers can be different.</span></span> <span data-ttu-id="0d4d7-139">Der öffentliche Port ist, welche Clients verwenden, wenn sie eine Anforderung an die Rolle zu senden.</span><span class="sxs-lookup"><span data-stu-id="0d4d7-139">The public port is what clients use when they send a request to the role.</span></span>

[![](host-owin-in-an-azure-worker-role/_static/image8.png)](host-owin-in-an-azure-worker-role/_static/image7.png)

## <a name="create-the-owin-startup-class"></a><span data-ttu-id="0d4d7-140">Erstellen der OWIN-Startup-Klasse</span><span class="sxs-lookup"><span data-stu-id="0d4d7-140">Create the OWIN Startup Class</span></span>

<span data-ttu-id="0d4d7-141">Klicken Sie im Projektmappen-Explorer klicken Sie mit der rechten Maustaste auf das Projekt WorkerRole1, und wählen Sie **hinzufügen** / **Klasse** eine neue Klasse hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="0d4d7-141">In Solution Explorer, right click the WorkerRole1 project and select **Add** / **Class** to add a new class.</span></span> <span data-ttu-id="0d4d7-142">Nennen Sie die Klasse `Startup`.</span><span class="sxs-lookup"><span data-stu-id="0d4d7-142">Name the class `Startup`.</span></span>

<span data-ttu-id="0d4d7-143">Ersetzen Sie alle den Standardcode durch Folgendes:</span><span class="sxs-lookup"><span data-stu-id="0d4d7-143">Replace all of the boilerplate code with the following:</span></span>

[!code-csharp[Main](host-owin-in-an-azure-worker-role/samples/sample2.cs)]

<span data-ttu-id="0d4d7-144">Die `UseWelcomePage` Erweiterungsmethode Fügt eine einfache HTML-Seite Ihrer Anwendung, um zu überprüfen, ob die Website funktioniert.</span><span class="sxs-lookup"><span data-stu-id="0d4d7-144">The `UseWelcomePage` extension method adds a simple HTML page to your application, to verify the site is working.</span></span>

## <a name="start-the-owin-host"></a><span data-ttu-id="0d4d7-145">Starten Sie die OWIN-Host</span><span class="sxs-lookup"><span data-stu-id="0d4d7-145">Start the OWIN Host</span></span>

<span data-ttu-id="0d4d7-146">Öffnen Sie die Datei "WorkerRole.cs".</span><span class="sxs-lookup"><span data-stu-id="0d4d7-146">Open the WorkerRole.cs file.</span></span> <span data-ttu-id="0d4d7-147">Diese Klasse definiert den Code, der ausgeführt wird, wenn die workerrolle gestartet oder beendet wird.</span><span class="sxs-lookup"><span data-stu-id="0d4d7-147">This class defines the code that runs when the worker role is started and stopped.</span></span>

<span data-ttu-id="0d4d7-148">Fügen Sie die folgenden using-Anweisung:</span><span class="sxs-lookup"><span data-stu-id="0d4d7-148">Add the following using statement:</span></span>

[!code-csharp[Main](host-owin-in-an-azure-worker-role/samples/sample3.cs)]

<span data-ttu-id="0d4d7-149">Hinzufügen einer **"IDisposable"** Member der `WorkerRole` Klasse:</span><span class="sxs-lookup"><span data-stu-id="0d4d7-149">Add an **IDisposable** member to the `WorkerRole` class:</span></span>

[!code-csharp[Main](host-owin-in-an-azure-worker-role/samples/sample4.cs)]

<span data-ttu-id="0d4d7-150">In der `OnStart` -Methode, fügen Sie den folgenden Code zum Starten des Hosts hinzu:</span><span class="sxs-lookup"><span data-stu-id="0d4d7-150">In the `OnStart` method, add the following code to start the host:</span></span>

[!code-csharp[Main](host-owin-in-an-azure-worker-role/samples/sample5.cs?highlight=5)]

<span data-ttu-id="0d4d7-151">Die **WebApp.Start** Methode startet den OWIN-Host.</span><span class="sxs-lookup"><span data-stu-id="0d4d7-151">The **WebApp.Start** method starts the OWIN host.</span></span> <span data-ttu-id="0d4d7-152">Der Name des der `Startup` Klasse ist ein Typparameter der Methode.</span><span class="sxs-lookup"><span data-stu-id="0d4d7-152">The name of the `Startup` class is a type parameter to the method.</span></span> <span data-ttu-id="0d4d7-153">Gemäß der Konvention der Host Ruft die `Configure` Methode dieser Klasse.</span><span class="sxs-lookup"><span data-stu-id="0d4d7-153">By convention, the host will call the `Configure` method of this class.</span></span>

<span data-ttu-id="0d4d7-154">Überschreiben der `OnStop` zum Verwerfen der  *\_app* Instanz:</span><span class="sxs-lookup"><span data-stu-id="0d4d7-154">Override the `OnStop` to dispose of the *\_app* instance:</span></span>

[!code-csharp[Main](host-owin-in-an-azure-worker-role/samples/sample6.cs)]

<span data-ttu-id="0d4d7-155">Hier ist der vollständige Code für "WorkerRole.cs":</span><span class="sxs-lookup"><span data-stu-id="0d4d7-155">Here is the complete code for WorkerRole.cs:</span></span>

[!code-csharp[Main](host-owin-in-an-azure-worker-role/samples/sample7.cs)]

<span data-ttu-id="0d4d7-156">Erstellen Sie die Projektmappe, und drücken Sie F5, um die Anwendung lokal im Azure-Serveremulator ausführen.</span><span class="sxs-lookup"><span data-stu-id="0d4d7-156">Build the solution, and press F5 to run the application locally in the Azure Compute Emulator.</span></span> <span data-ttu-id="0d4d7-157">Abhängig von Ihren Firewalleinstellungen müssen Sie den Emulator über die Firewall zulassen.</span><span class="sxs-lookup"><span data-stu-id="0d4d7-157">Depending on your firewall settings, you might need to allow the emulator through your firewall.</span></span>

<span data-ttu-id="0d4d7-158">Der Serveremulator weist eine lokale IP-Adresse an den Endpunkt an.</span><span class="sxs-lookup"><span data-stu-id="0d4d7-158">The compute emulator assigns a local IP address to the endpoint.</span></span> <span data-ttu-id="0d4d7-159">Sie finden die IP-Adresse, indem Sie die Serveremulator-Benutzeroberfläche anzeigen.</span><span class="sxs-lookup"><span data-stu-id="0d4d7-159">You can find the IP address by viewing the Compute Emulator UI.</span></span> <span data-ttu-id="0d4d7-160">Mit der rechten Maustaste in des Emulator-Symbols im Infobereich der Taskleiste und, und wählen **Serveremulator-Benutzeroberfläche anzeigen**.</span><span class="sxs-lookup"><span data-stu-id="0d4d7-160">Right-click the emulator icon in the task bar notification area, and select **Show Compute Emulator UI**.</span></span>

[![](host-owin-in-an-azure-worker-role/_static/image10.png)](host-owin-in-an-azure-worker-role/_static/image9.png)

<span data-ttu-id="0d4d7-161">Suchen Sie die IP-Adresse unter Dienstbereitstellungen, die Bereitstellung [Id], Dienstdetails.</span><span class="sxs-lookup"><span data-stu-id="0d4d7-161">Find the IP address under Service Deployments, deployment [id], Service Details.</span></span> <span data-ttu-id="0d4d7-162">Öffnen Sie einen Webbrowser, und navigieren Sie zu http://<em>Adresse</em>, wobei <em>Adresse</em> ist die IP-Adresse vom Serveremulator; z. B. `http://127.0.0.1:80`.</span><span class="sxs-lookup"><span data-stu-id="0d4d7-162">Open a web browser and navigate to http://<em>address</em>, where <em>address</em> is the IP address assigned by the compute emulator; for example, `http://127.0.0.1:80`.</span></span> <span data-ttu-id="0d4d7-163">Der OWIN-Startseite sollte angezeigt werden:</span><span class="sxs-lookup"><span data-stu-id="0d4d7-163">You should see the OWIN welcome page:</span></span>

![](host-owin-in-an-azure-worker-role/_static/image11.png)

## <a name="deploy-to-azure"></a><span data-ttu-id="0d4d7-164">Bereitstellung in Azure</span><span class="sxs-lookup"><span data-stu-id="0d4d7-164">Deploy to Azure</span></span>

<span data-ttu-id="0d4d7-165">Für diesen Schritt müssen Sie ein Azure-Konto verfügen.</span><span class="sxs-lookup"><span data-stu-id="0d4d7-165">For this step, you must have an Azure account.</span></span> <span data-ttu-id="0d4d7-166">Wenn Sie noch nicht haben, können Sie in nur wenigen Minuten ein kostenloses Testkonto erstellen.</span><span class="sxs-lookup"><span data-stu-id="0d4d7-166">If you don't already have one, you can create a free trial account in just a couple of minutes.</span></span> <span data-ttu-id="0d4d7-167">Weitere Informationen finden Sie unter [kostenlose Microsoft Azure-Testversion](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F).</span><span class="sxs-lookup"><span data-stu-id="0d4d7-167">For details, see [Microsoft Azure Free Trial](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F).</span></span>

<span data-ttu-id="0d4d7-168">Klicken Sie im Projektmappen-Explorer das Projekt AzureApp.</span><span class="sxs-lookup"><span data-stu-id="0d4d7-168">In Solution Explorer, right-click the AzureApp project.</span></span> <span data-ttu-id="0d4d7-169">Wählen Sie **Veröffentlichen**.</span><span class="sxs-lookup"><span data-stu-id="0d4d7-169">Select **Publish**.</span></span>

![](host-owin-in-an-azure-worker-role/_static/image12.png)

<span data-ttu-id="0d4d7-170">Wenn Sie nicht beim Azure-Konto angemeldet sind, klicken Sie auf **Anmeldung**.</span><span class="sxs-lookup"><span data-stu-id="0d4d7-170">If you are not signed in to your Azure account, click **Sign In**.</span></span>

[![](host-owin-in-an-azure-worker-role/_static/image14.png)](host-owin-in-an-azure-worker-role/_static/image13.png)

<span data-ttu-id="0d4d7-171">Nachdem Sie angemeldet sind, wählen Sie ein Abonnement, und klicken Sie auf **Weiter**.</span><span class="sxs-lookup"><span data-stu-id="0d4d7-171">After you are signed in, choose a subscription and click **Next**.</span></span>

[![](host-owin-in-an-azure-worker-role/_static/image16.png)](host-owin-in-an-azure-worker-role/_static/image15.png)

<span data-ttu-id="0d4d7-172">Geben Sie einen Namen für den Clouddienst, und wählen Sie eine Region aus.</span><span class="sxs-lookup"><span data-stu-id="0d4d7-172">Enter a name for the cloud service and choose a region.</span></span> <span data-ttu-id="0d4d7-173">Klicken Sie auf **Erstellen**.</span><span class="sxs-lookup"><span data-stu-id="0d4d7-173">Click **Create**.</span></span>

![](host-owin-in-an-azure-worker-role/_static/image17.png)

<span data-ttu-id="0d4d7-174">Klicken Sie auf **Veröffentlichen**.</span><span class="sxs-lookup"><span data-stu-id="0d4d7-174">Click **Publish**.</span></span>

[![](host-owin-in-an-azure-worker-role/_static/image19.png)](host-owin-in-an-azure-worker-role/_static/image18.png)

<span data-ttu-id="0d4d7-175">Das Fenster "Azure Activity Log" zeigt den Fortschritt der Bereitstellung.</span><span class="sxs-lookup"><span data-stu-id="0d4d7-175">The Azure Activity Log window shows the progress of the deployment.</span></span> <span data-ttu-id="0d4d7-176">Wenn die app bereitgestellt wurde, navigieren Sie zur `http://appname.cloudapp.net/`, wobei *Appname* ist der Name des Cloud-Diensts.</span><span class="sxs-lookup"><span data-stu-id="0d4d7-176">When the app is deployed, browse to `http://appname.cloudapp.net/`, where *appname* is the name of your cloud service.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="0d4d7-177">Zusätzliche Ressourcen</span><span class="sxs-lookup"><span data-stu-id="0d4d7-177">Additional Resources</span></span>

- [<span data-ttu-id="0d4d7-178">Übersicht über das Katana-Projekt</span><span class="sxs-lookup"><span data-stu-id="0d4d7-178">An Overview of Project Katana</span></span>](an-overview-of-project-katana.md)
- [<span data-ttu-id="0d4d7-179">Katana-Projekt auf GitHub</span><span class="sxs-lookup"><span data-stu-id="0d4d7-179">Katana Project on GitHub</span></span>](https://github.com/aspnet/AspNetKatana/)
