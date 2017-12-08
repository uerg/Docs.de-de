---
uid: web-api/overview/hosting-aspnet-web-api/host-aspnet-web-api-in-an-azure-worker-role
title: Hosten von ASP.NET Web API 2 in einer Azure-Workerrolle | Microsoft Docs
author: MikeWasson
description: "Dieses Lernprogramm zeigt, wie zum Hosten von ASP.NET Web-API in einer Azure-Workerrolle mit OWIN Selbsthosting der Web-API-Framework. Öffnen Sie die Weboberfläche für .NET (OWIN) de..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/02/2014
ms.topic: article
ms.assetid: 6980ee2e-d6b0-4a08-8fb6-ab96362dd0e3
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/hosting-aspnet-web-api/host-aspnet-web-api-in-an-azure-worker-role
msc.type: authoredcontent
ms.openlocfilehash: 326c4a4e274dbc1aa6e09f1d07c4d135e4304484
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="host-aspnet-web-api-2-in-an-azure-worker-role"></a><span data-ttu-id="eef0e-104">Hosten von ASP.NET Web API 2 in einer Azure-Workerrolle</span><span class="sxs-lookup"><span data-stu-id="eef0e-104">Host ASP.NET Web API 2 in an Azure Worker Role</span></span>
====================
<span data-ttu-id="eef0e-105">durch [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="eef0e-105">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

> <span data-ttu-id="eef0e-106">Dieses Lernprogramm zeigt, wie zum Hosten von ASP.NET Web-API in einer Azure-Workerrolle mit OWIN Selbsthosting der Web-API-Framework.</span><span class="sxs-lookup"><span data-stu-id="eef0e-106">This tutorial shows how to host ASP.NET Web API in an Azure Worker Role, using OWIN to self-host the Web API framework.</span></span>
> 
> <span data-ttu-id="eef0e-107">[Öffnen Sie die Weboberfläche für .NET](http://owin.org/) (OWIN) definiert eine Abstraktion zwischen .NET Webservern und Webanwendungen.</span><span class="sxs-lookup"><span data-stu-id="eef0e-107">[Open Web Interface for .NET](http://owin.org/) (OWIN) defines an abstraction between .NET web servers and web applications.</span></span> <span data-ttu-id="eef0e-108">OWIN entkoppelt die Webanwendung aus dem Server, wodurch OWIN ideal für eine Webanwendung in Ihrem eigenen Prozess, außerhalb von IIS-Self-hosting – z. B. in einer Azure-workerrolle.</span><span class="sxs-lookup"><span data-stu-id="eef0e-108">OWIN decouples the web application from the server, which makes OWIN ideal for self-hosting a web application in your own process, outside of IIS–for example, inside an Azure worker role.</span></span>
> 
> <span data-ttu-id="eef0e-109">In diesem Lernprogramm verwenden Sie das Paket Microsoft.Owin.Host.HttpListener stellt einem HTTP-Server, die verwendet werden, um Selbsthosting OWIN-Anwendungen.</span><span class="sxs-lookup"><span data-stu-id="eef0e-109">In this tutorial, you'll use the Microsoft.Owin.Host.HttpListener package, which provides an HTTP server that be used to self-host OWIN applications.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="eef0e-110">In diesem Lernprogramm verwendeten Versionen der Software</span><span class="sxs-lookup"><span data-stu-id="eef0e-110">Software versions used in the tutorial</span></span>
> 
> 
> - [<span data-ttu-id="eef0e-111">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="eef0e-111">Visual Studio 2013</span></span>](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - <span data-ttu-id="eef0e-112">Web-API 2</span><span class="sxs-lookup"><span data-stu-id="eef0e-112">Web API 2</span></span>
> - [<span data-ttu-id="eef0e-113">Azure SDK für .NET 2.3</span><span class="sxs-lookup"><span data-stu-id="eef0e-113">Azure SDK for .NET 2.3</span></span>](https://azure.microsoft.com/en-us/downloads/)


## <a name="create-a-microsoft-azure-project"></a><span data-ttu-id="eef0e-114">Erstellen Sie ein Microsoft Azure-Projekt</span><span class="sxs-lookup"><span data-stu-id="eef0e-114">Create a Microsoft Azure Project</span></span>

<span data-ttu-id="eef0e-115">Starten Sie Visual Studio mit Administratorrechten aus.</span><span class="sxs-lookup"><span data-stu-id="eef0e-115">Start Visual Studio with administrator privileges.</span></span> <span data-ttu-id="eef0e-116">Administratorrechte sind erforderlich, um die Anwendung lokal debuggen, mit dem Azure-Serveremulator.</span><span class="sxs-lookup"><span data-stu-id="eef0e-116">Administrator privileges are needed to debug the application locally, using the Azure compute emulator.</span></span>

<span data-ttu-id="eef0e-117">Auf der **Datei** Menü klicken Sie auf **neu**, klicken Sie dann auf **Projekt**.</span><span class="sxs-lookup"><span data-stu-id="eef0e-117">On the **File** menu, click **New**, then click **Project**.</span></span> <span data-ttu-id="eef0e-118">Von **installierte Vorlagen**, klicken Sie unter Visual C#- **Cloud** , und klicken Sie dann auf **Windows Azure-Cloud-Dienst**.</span><span class="sxs-lookup"><span data-stu-id="eef0e-118">From **Installed Templates**, under Visual C#, click **Cloud** and then click **Windows Azure Cloud Service**.</span></span> <span data-ttu-id="eef0e-119">Nennen Sie das Projekt "AzureApp", und klicken Sie auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="eef0e-119">Name the project "AzureApp" and click **OK**.</span></span>

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image2.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image1.png)

<span data-ttu-id="eef0e-120">In der **neuen Windows Azure-Cloud-Dienst** Dialogfeld, doppelklicken Sie auf **Workerrolle**.</span><span class="sxs-lookup"><span data-stu-id="eef0e-120">In the **New Windows Azure Cloud Service** dialog, double-click **Worker Role**.</span></span> <span data-ttu-id="eef0e-121">Lassen Sie den Standardnamen ("WorkerRole1").</span><span class="sxs-lookup"><span data-stu-id="eef0e-121">Leave the default name ("WorkerRole1").</span></span> <span data-ttu-id="eef0e-122">Dieser Schritt wird der Projektmappe eine workerrolle hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="eef0e-122">This step adds a worker role to the solution.</span></span> <span data-ttu-id="eef0e-123">Klicken Sie auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="eef0e-123">Click **OK**.</span></span>

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image4.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image3.png)

<span data-ttu-id="eef0e-124">Visual Studio-Projektmappe, die erstellt wird, enthält zwei Projekte:</span><span class="sxs-lookup"><span data-stu-id="eef0e-124">The Visual Studio solution that is created contains two projects:</span></span>

- <span data-ttu-id="eef0e-125">&quot;AzureApp&quot; definiert die Rollen und die Konfiguration für die Azure-Anwendung.</span><span class="sxs-lookup"><span data-stu-id="eef0e-125">&quot;AzureApp&quot; defines the roles and configuration for the Azure application.</span></span>
- <span data-ttu-id="eef0e-126">&quot;WorkerRole1&quot; enthält den Code für die workerrolle.</span><span class="sxs-lookup"><span data-stu-id="eef0e-126">&quot;WorkerRole1&quot; contains the code for the worker role.</span></span>

<span data-ttu-id="eef0e-127">Im Allgemeinen kann eine Azure-Anwendung mehrere Rollen enthalten, obwohl in diesem Lernprogramm eine einzige Rolle verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="eef0e-127">In general, an Azure application can contain multiple roles, although this tutorial uses a single role.</span></span>

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image5.png)

## <a name="add-the-web-api-and-owin-packages"></a><span data-ttu-id="eef0e-128">Hinzufügen der Web-API und die OWIN-Pakete</span><span class="sxs-lookup"><span data-stu-id="eef0e-128">Add the Web API and OWIN Packages</span></span>

<span data-ttu-id="eef0e-129">Aus der **Tools** Menü klicken Sie auf **Bibliothekspaket-Manager**, klicken Sie dann auf **Package Manager Console**.</span><span class="sxs-lookup"><span data-stu-id="eef0e-129">From the **Tools** menu, click **Library Package Manager**, then click **Package Manager Console**.</span></span>

<span data-ttu-id="eef0e-130">Geben Sie im Fenster Paket-Manager-Konsole den folgenden Befehl aus:</span><span class="sxs-lookup"><span data-stu-id="eef0e-130">In the Package Manager Console window, enter the following command:</span></span>

[!code-console[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample1.cmd)]

## <a name="add-an-http-endpoint"></a><span data-ttu-id="eef0e-131">Fügen Sie einen HTTP-Endpunkt</span><span class="sxs-lookup"><span data-stu-id="eef0e-131">Add an HTTP Endpoint</span></span>

<span data-ttu-id="eef0e-132">Erweitern Sie im Projektmappen-Explorer das Projekt AzureApp.</span><span class="sxs-lookup"><span data-stu-id="eef0e-132">In Solution Explorer, expand the AzureApp project.</span></span> <span data-ttu-id="eef0e-133">Erweitern Sie den Knoten Rollen, mit der rechten Maustaste WorkerRole1, und wählen **Eigenschaften**.</span><span class="sxs-lookup"><span data-stu-id="eef0e-133">Expand the Roles node, right-click WorkerRole1, and select **Properties**.</span></span>

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image6.png)

<span data-ttu-id="eef0e-134">Klicken Sie auf **Endpunkte**, und klicken Sie dann auf **Add Endpoint**.</span><span class="sxs-lookup"><span data-stu-id="eef0e-134">Click **Endpoints**, and then click **Add Endpoint**.</span></span>

<span data-ttu-id="eef0e-135">In der **Protokoll** Dropdown-Liste, wählen Sie "http".</span><span class="sxs-lookup"><span data-stu-id="eef0e-135">In the **Protocol** dropdown list, select "http".</span></span> <span data-ttu-id="eef0e-136">In **öffentlicher Port** und **privater Port**, geben den Wert 80.</span><span class="sxs-lookup"><span data-stu-id="eef0e-136">In **Public Port** and **Private Port**, type 80.</span></span> <span data-ttu-id="eef0e-137">Diese Portnummern können unterschiedlich sein.</span><span class="sxs-lookup"><span data-stu-id="eef0e-137">These port numbers can be different.</span></span> <span data-ttu-id="eef0e-138">Der öffentliche Port wird von Clients verwendet, beim Versand einer Anforderungs in die Rolle.</span><span class="sxs-lookup"><span data-stu-id="eef0e-138">The public port is what clients use when they send a request to the role.</span></span>

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image8.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image7.png)

## <a name="configure-web-api-for-self-host"></a><span data-ttu-id="eef0e-139">Konfigurieren von Web-API für Selbsthosting</span><span class="sxs-lookup"><span data-stu-id="eef0e-139">Configure Web API for Self-Host</span></span>

<span data-ttu-id="eef0e-140">Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste, klicken Sie auf das Projekt WorkerRole1, und wählen **hinzufügen** / **Klasse** So fügen Sie eine neue Klasse hinzu.</span><span class="sxs-lookup"><span data-stu-id="eef0e-140">In Solution Explorer, right click the WorkerRole1 project and select **Add** / **Class** to add a new class.</span></span> <span data-ttu-id="eef0e-141">Nennen Sie die Klasse `Startup`.</span><span class="sxs-lookup"><span data-stu-id="eef0e-141">Name the class `Startup`.</span></span>

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image9.png)

<span data-ttu-id="eef0e-142">Ersetzen Sie alle in dieser Datei den Standardcode durch Folgendes:</span><span class="sxs-lookup"><span data-stu-id="eef0e-142">Replace all of the boilerplate code in this file with the following:</span></span>

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample2.cs)]

## <a name="add-a-web-api-controller"></a><span data-ttu-id="eef0e-143">Fügen Sie eine Web-API-Controller hinzu</span><span class="sxs-lookup"><span data-stu-id="eef0e-143">Add a Web API Controller</span></span>

<span data-ttu-id="eef0e-144">Als Nächstes fügen Sie eine Web-API-Controllerklasse hinzu.</span><span class="sxs-lookup"><span data-stu-id="eef0e-144">Next, add a Web API controller class.</span></span> <span data-ttu-id="eef0e-145">Mit der rechten Maustaste des WorkerRole1-Projekts, und wählen Sie **hinzufügen** / **Klasse**.</span><span class="sxs-lookup"><span data-stu-id="eef0e-145">Right-click the WorkerRole1 project and select **Add** / **Class**.</span></span> <span data-ttu-id="eef0e-146">Benennen Sie die Klasse TestController.</span><span class="sxs-lookup"><span data-stu-id="eef0e-146">Name the class TestController.</span></span> <span data-ttu-id="eef0e-147">Ersetzen Sie alle in dieser Datei den Standardcode durch Folgendes:</span><span class="sxs-lookup"><span data-stu-id="eef0e-147">Replace all of the boilerplate code in this file with the following:</span></span>

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample3.cs)]

<span data-ttu-id="eef0e-148">Der Einfachheit halber definiert diesen Controller nur zwei GET-Methoden, die nur-Text zurückgeben.</span><span class="sxs-lookup"><span data-stu-id="eef0e-148">For simplicity, this controller just defines two GET methods that return plain text.</span></span>

## <a name="start-the-owin-host"></a><span data-ttu-id="eef0e-149">Starten Sie den OWIN-Host</span><span class="sxs-lookup"><span data-stu-id="eef0e-149">Start the OWIN Host</span></span>

<span data-ttu-id="eef0e-150">Öffnen der Datei WorkerRole.cs hinzu.</span><span class="sxs-lookup"><span data-stu-id="eef0e-150">Open the WorkerRole.cs file.</span></span> <span data-ttu-id="eef0e-151">Diese Klasse definiert den Code, der ausgeführt wird, wenn die workerrolle gestartet oder beendet wird.</span><span class="sxs-lookup"><span data-stu-id="eef0e-151">This class defines the code that runs when the worker role is started and stopped.</span></span>

<span data-ttu-id="eef0e-152">Fügen Sie die folgende Anweisung:</span><span class="sxs-lookup"><span data-stu-id="eef0e-152">Add the following using statement:</span></span>

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample4.cs)]

<span data-ttu-id="eef0e-153">Hinzufügen einer **IDisposable** Member der `WorkerRole` Klasse:</span><span class="sxs-lookup"><span data-stu-id="eef0e-153">Add an **IDisposable** member to the `WorkerRole` class:</span></span>

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample5.cs)]

<span data-ttu-id="eef0e-154">In der `OnStart` -Methode, fügen Sie den folgenden Code zum Starten des Hosts hinzu:</span><span class="sxs-lookup"><span data-stu-id="eef0e-154">In the `OnStart` method, add the following code to start the host:</span></span>

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample6.cs?highlight=5)]

<span data-ttu-id="eef0e-155">Die **WebApp.Start** Methode startet den OWIN-Host.</span><span class="sxs-lookup"><span data-stu-id="eef0e-155">The **WebApp.Start** method starts the OWIN host.</span></span> <span data-ttu-id="eef0e-156">Der Name des der `Startup` Klasse ist ein Typparameter der Methode.</span><span class="sxs-lookup"><span data-stu-id="eef0e-156">The name of the `Startup` class is a type parameter to the method.</span></span> <span data-ttu-id="eef0e-157">Gemäß der Konvention werden der Host Ruft die `Configure` Methode dieser Klasse.</span><span class="sxs-lookup"><span data-stu-id="eef0e-157">By convention, the host will call the `Configure` method of this class.</span></span>

<span data-ttu-id="eef0e-158">Überschreiben Sie die `OnStop` Löschen der  *\_app* Instanz:</span><span class="sxs-lookup"><span data-stu-id="eef0e-158">Override the `OnStop` to dispose of the *\_app* instance:</span></span>

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample7.cs)]

<span data-ttu-id="eef0e-159">Hier wird der vollständige Code für WorkerRole.cs:</span><span class="sxs-lookup"><span data-stu-id="eef0e-159">Here is the complete code for WorkerRole.cs:</span></span>

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample8.cs)]

<span data-ttu-id="eef0e-160">Erstellen Sie die Projektmappe, und drücken Sie F5, um die Anwendung lokal im Azure-Serveremulator auszuführen.</span><span class="sxs-lookup"><span data-stu-id="eef0e-160">Build the solution, and press F5 to run the application locally in the Azure Compute Emulator.</span></span> <span data-ttu-id="eef0e-161">Abhängig von Ihren Firewalleinstellungen müssen Sie möglicherweise den Emulator über die Firewall zulassen.</span><span class="sxs-lookup"><span data-stu-id="eef0e-161">Depending on your firewall settings, you might need to allow the emulator through your firewall.</span></span>

> [!NOTE]
> <span data-ttu-id="eef0e-162">Wenn Sie eine der folgenden vergleichbare Ausnahme angezeigt werden, finden Sie unter [diesem Blogbeitrag](https://blogs.msdn.com/b/praburaj/archive/2013/11/20/fileloadexception-on-microsoft-owin-when-running-on-worker-role.aspx) für dieses Problem zu umgehen.</span><span class="sxs-lookup"><span data-stu-id="eef0e-162">If you get an exception like the following, please see [this blog post](https://blogs.msdn.com/b/praburaj/archive/2013/11/20/fileloadexception-on-microsoft-owin-when-running-on-worker-role.aspx) for a workaround.</span></span> <span data-ttu-id="eef0e-163">"Konnte nicht geladen werden, Datei oder Assembly" "Microsoft.owin", Version = 2.0.2.0, Culture = Neutral, PublicKeyToken = 31bf3856ad364e35' oder eine ihrer Abhängigkeiten.</span><span class="sxs-lookup"><span data-stu-id="eef0e-163">"Could not load file or assembly 'Microsoft.Owin, Version=2.0.2.0, Culture=neutral, PublicKeyToken=31bf3856ad364e35' or one of its dependencies.</span></span> <span data-ttu-id="eef0e-164">Das manifest der gefunden Assemblydefinition entspricht nicht der Assemblyverweis verweist.</span><span class="sxs-lookup"><span data-stu-id="eef0e-164">The located assembly's manifest definition does not match the assembly reference.</span></span> <span data-ttu-id="eef0e-165">(Ausnahme von HRESULT: 0x80131040) "</span><span class="sxs-lookup"><span data-stu-id="eef0e-165">(Exception from HRESULT: 0x80131040)"</span></span>


<span data-ttu-id="eef0e-166">Der Serveremulator weist eine lokale IP-Adresse an den Endpunkt an.</span><span class="sxs-lookup"><span data-stu-id="eef0e-166">The compute emulator assigns a local IP address to the endpoint.</span></span> <span data-ttu-id="eef0e-167">Sie können die IP-Adresse finden, indem Sie den Serveremulator-Benutzeroberfläche anzeigen.</span><span class="sxs-lookup"><span data-stu-id="eef0e-167">You can find the IP address by viewing the Compute Emulator UI.</span></span> <span data-ttu-id="eef0e-168">Maustaste auf das Symbol "Emulator" in den Infobereich der Taskleiste, und wählen Sie **Serveremulator-Benutzeroberfläche anzeigen**.</span><span class="sxs-lookup"><span data-stu-id="eef0e-168">Right-click the emulator icon in the task bar notification area, and select **Show Compute Emulator UI**.</span></span>

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image11.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image10.png)

<span data-ttu-id="eef0e-169">Suchen Sie die IP-Adresse unter Dienstbereitstellungen, Bereitstellung [Id], Dienstdetails.</span><span class="sxs-lookup"><span data-stu-id="eef0e-169">Find the IP address under Service Deployments, deployment [id], Service Details.</span></span> <span data-ttu-id="eef0e-170">Öffnen Sie einen Webbrowser, und navigieren Sie zu http://*Adresse*/Test/1, in dem *Adresse* ist die IP-Adresse zugewiesen, die vom Serveremulator; z. B. `http://127.0.0.1:80/test/1`.</span><span class="sxs-lookup"><span data-stu-id="eef0e-170">Open a web browser and navigate to http://*address*/test/1, where *address* is the IP address assigned by the compute emulator; for example, `http://127.0.0.1:80/test/1`.</span></span> <span data-ttu-id="eef0e-171">Die Antwort aus dem Web-API-Controller sollte angezeigt werden:</span><span class="sxs-lookup"><span data-stu-id="eef0e-171">You should see the response from the Web API controller:</span></span>

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image12.png)

## <a name="deploy-to-azure"></a><span data-ttu-id="eef0e-172">In Azure bereitstellen</span><span class="sxs-lookup"><span data-stu-id="eef0e-172">Deploy to Azure</span></span>

<span data-ttu-id="eef0e-173">Für diesen Schritt müssen Sie ein Azure-Konto verfügen.</span><span class="sxs-lookup"><span data-stu-id="eef0e-173">For this step, you must have an Azure account.</span></span> <span data-ttu-id="eef0e-174">Wenn Sie einen noch nicht haben, können Sie in wenigen Minuten ein kostenloses Testkonto erstellen.</span><span class="sxs-lookup"><span data-stu-id="eef0e-174">If you don't already have one, you can create a free trial account in just a couple of minutes.</span></span> <span data-ttu-id="eef0e-175">Weitere Informationen finden Sie unter [kostenlosen Microsoft Azure-Testversion](https://azure.microsoft.com/en-us/pricing/free-trial/?WT.mc_id=A261C142F).</span><span class="sxs-lookup"><span data-stu-id="eef0e-175">For details, see [Microsoft Azure Free Trial](https://azure.microsoft.com/en-us/pricing/free-trial/?WT.mc_id=A261C142F).</span></span>

<span data-ttu-id="eef0e-176">Im Projektmappen-Explorer mit der rechten Maustaste des Projekts AzureApp.</span><span class="sxs-lookup"><span data-stu-id="eef0e-176">In Solution Explorer, right-click the AzureApp project.</span></span> <span data-ttu-id="eef0e-177">Wählen Sie **Veröffentlichen**.</span><span class="sxs-lookup"><span data-stu-id="eef0e-177">Select **Publish**.</span></span>

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image13.png)

<span data-ttu-id="eef0e-178">Wenn Sie nicht beim Azure-Konto angemeldet sind, klicken Sie auf **anmelden**.</span><span class="sxs-lookup"><span data-stu-id="eef0e-178">If you are not signed in to your Azure account, click **Sign In**.</span></span>

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image15.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image14.png)

<span data-ttu-id="eef0e-179">Nachdem Sie sich angemeldet haben, wählen Sie ein Abonnement, und klicken Sie auf **Weiter**.</span><span class="sxs-lookup"><span data-stu-id="eef0e-179">After you are signed in, choose a subscription and click **Next**.</span></span>

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image17.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image16.png)

<span data-ttu-id="eef0e-180">Geben Sie einen Namen für den Clouddienst, und wählen Sie eine Region aus.</span><span class="sxs-lookup"><span data-stu-id="eef0e-180">Enter a name for the cloud service and choose a region.</span></span> <span data-ttu-id="eef0e-181">Klicken Sie auf **Erstellen**.</span><span class="sxs-lookup"><span data-stu-id="eef0e-181">Click **Create**.</span></span>

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image18.png)

<span data-ttu-id="eef0e-182">Klicken Sie auf **Veröffentlichen**.</span><span class="sxs-lookup"><span data-stu-id="eef0e-182">Click **Publish**.</span></span>

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image20.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image19.png)

<span data-ttu-id="eef0e-183">Das Azure-Aktivitätsprotokoll-Fenster wird der Fortschritt der Bereitstellung.</span><span class="sxs-lookup"><span data-stu-id="eef0e-183">The Azure Activity Log window shows the progress of the deployment.</span></span> <span data-ttu-id="eef0e-184">Wenn die app bereitgestellt wurde, navigieren Sie zu http://appname.cloudapp.net/test/1.</span><span class="sxs-lookup"><span data-stu-id="eef0e-184">When the app is deployed, browse to http://appname.cloudapp.net/test/1.</span></span>

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image21.png)

## <a name="additional-resources"></a><span data-ttu-id="eef0e-185">Zusätzliche Ressourcen</span><span class="sxs-lookup"><span data-stu-id="eef0e-185">Additional Resources</span></span>

- [<span data-ttu-id="eef0e-186">Eine Übersicht über Project Katana</span><span class="sxs-lookup"><span data-stu-id="eef0e-186">An Overview of Project Katana</span></span>](../../../aspnet/overview/owin-and-katana/an-overview-of-project-katana.md)
- [<span data-ttu-id="eef0e-187">Katana-Projekt auf GitHub</span><span class="sxs-lookup"><span data-stu-id="eef0e-187">Katana Project on GitHub</span></span>](https://github.com/aspnet/AspNetKatana)
