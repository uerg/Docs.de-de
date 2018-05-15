---
uid: signalr/overview/performance/using-signalr-performance-counters-in-an-azure-web-role
title: SignalR-Leistungsindikatoren in einer Azure-Webrolle mit | Microsoft Docs
author: guardrex
description: Informationen zum Installieren und Verwenden von SignalR-Leistungsindikatoren in einer Azure-Webrolle.
keywords: ASP.NET,SignalR,Performance Zähler, Azure-Webrolle
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/11/2017
ms.topic: article
ms.assetid: 2a127d3b-21ed-4cc9-bec0-cdab4e742a25
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/performance/using-signalr-performance-counters-in-an-azure-web-role
msc.type: authoredcontent
ms.openlocfilehash: 2f6c6feb030fc17f95e7862c39029569f3d8c5dc
ms.sourcegitcommit: d8aa1d314891e981460b5e5c912afb730adbb3ad
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/05/2018
---
# <a name="using-signalr-performance-counters-in-an-azure-web-role"></a><span data-ttu-id="e8ba1-104">Mithilfe von SignalR-Leistungsindikatoren in einer Azure-Webrolle</span><span class="sxs-lookup"><span data-stu-id="e8ba1-104">Using SignalR performance counters in an Azure Web Role</span></span>

<span data-ttu-id="e8ba1-105">Von [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="e8ba1-105">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="e8ba1-106">SignalR-Leistungsindikatoren werden verwendet, um die Leistung Ihrer app in einer Azure-Webrolle zu überwachen.</span><span class="sxs-lookup"><span data-stu-id="e8ba1-106">SignalR performance counters are used to monitor your app's performance in an Azure Web Role.</span></span> <span data-ttu-id="e8ba1-107">Die Leistungsindikatoren werden von Microsoft Azure-Diagnose erfasst.</span><span class="sxs-lookup"><span data-stu-id="e8ba1-107">The counters are captured by Microsoft Azure Diagnostics.</span></span> <span data-ttu-id="e8ba1-108">Installation von SignalR-Leistungsindikatoren in Azure mit *signalr.exe*, das gleiche Tool zur eigenständigen oder den lokalen apps.</span><span class="sxs-lookup"><span data-stu-id="e8ba1-108">You install SignalR performance counters on Azure with *signalr.exe*, the same tool used for standalone or on-premises apps.</span></span> <span data-ttu-id="e8ba1-109">Da Azure-Rollen vorübergehenden sind, konfigurieren Sie eine app zum Installieren und Registrieren von SignalR-Leistungsindikatoren beim Start.</span><span class="sxs-lookup"><span data-stu-id="e8ba1-109">Since Azure roles are transient, you configure an app to install and register SignalR performance counters upon startup.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="e8ba1-110">Erforderliche Komponenten</span><span class="sxs-lookup"><span data-stu-id="e8ba1-110">Prerequisites</span></span>

* [<span data-ttu-id="e8ba1-111">Visual Studio 2015</span><span class="sxs-lookup"><span data-stu-id="e8ba1-111">Visual Studio 2015</span></span>](https://www.visualstudio.com/vs/visual-studio-express/)
* <span data-ttu-id="e8ba1-112">[Microsoft Azure SDK für Visual Studio 2015 (VS2015)](https://azure.microsoft.com/downloads/) **Hinweis: Starten Sie den Computer nach der Installation des SDK.**</span><span class="sxs-lookup"><span data-stu-id="e8ba1-112">[Microsoft Azure SDK for Visual Studio 2015 (VS2015)](https://azure.microsoft.com/downloads/) **Note: Restart your machine after installing the SDK.**</span></span>
* <span data-ttu-id="e8ba1-113">Microsoft Azure-Abonnement: um ein kostenloses Azure-Testkonto registrieren, finden Sie unter [kostenlose Azure-Testversion](https://azure.microsoft.com/free/).</span><span class="sxs-lookup"><span data-stu-id="e8ba1-113">Microsoft Azure subscription: To sign up for a free Azure trial account, see [Azure Free Trial](https://azure.microsoft.com/free/).</span></span>

## <a name="creating-an-azure-web-role-application-that-exposes-signalr-performance-counters"></a><span data-ttu-id="e8ba1-114">Erstellen einer Anwendung Azure-Webrolle, macht SignalR-Leistungsindikatoren</span><span class="sxs-lookup"><span data-stu-id="e8ba1-114">Creating an Azure Web Role application that exposes SignalR performance counters</span></span>

1. <span data-ttu-id="e8ba1-115">Open Visual Studio 2015.</span><span class="sxs-lookup"><span data-stu-id="e8ba1-115">Open Visual Studio 2015.</span></span>

2. <span data-ttu-id="e8ba1-116">Wählen Sie in Visual Studio 2015 **Datei** > **neu** > **Projekt**.</span><span class="sxs-lookup"><span data-stu-id="e8ba1-116">In Visual Studio 2015, select **File** > **New** > **Project**.</span></span>

3. <span data-ttu-id="e8ba1-117">In der **Vorlagen** im Bereich der **neues Projekt** Fenster unter der **Visual C#-** Knoten, wählen die **Cloud** Knoten, und wählen Sie die **Azure-Cloud-Dienst** Vorlage.</span><span class="sxs-lookup"><span data-stu-id="e8ba1-117">In the **Templates** pane of the **New Project** window under the **Visual C#** node, select the **Cloud** node and select the **Azure Cloud Service** template.</span></span> <span data-ttu-id="e8ba1-118">Benennen Sie die app **SignalRPerfCounters** , und wählen Sie **OK**.</span><span class="sxs-lookup"><span data-stu-id="e8ba1-118">Name the app **SignalRPerfCounters** and select **OK**.</span></span>

   ![Neue Cloud-Anwendung](using-signalr-performance-counters-in-an-azure-web-role/_static/image1.png)
    
4. <span data-ttu-id="e8ba1-120">In der **neuen Microsoft Azure-Cloud-Dienst** wählen Sie im Dialogfeld **ASP.NET-Webrolle** , und wählen Sie die >, um die Rolle zum Projekt hinzuzufügen.</span><span class="sxs-lookup"><span data-stu-id="e8ba1-120">In the **New Microsoft Azure Cloud Service** dialog, select **ASP.NET Web Role** and select the > button to add the role to the project.</span></span> <span data-ttu-id="e8ba1-121">Klicken Sie auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="e8ba1-121">Select **OK**.</span></span>

   ![ASP.NET Web-Rolle hinzufügen](using-signalr-performance-counters-in-an-azure-web-role/_static/image2.png)
    
5. <span data-ttu-id="e8ba1-123">In der **neue ASP.NET-Webanwendung - WebRole1** wählen Sie im Dialogfeld die **MVC** Vorlage, und wählen Sie dann **OK**.</span><span class="sxs-lookup"><span data-stu-id="e8ba1-123">In the **New ASP.NET Web Application - WebRole1** dialog, select the **MVC** template, and then select **OK**.</span></span>

   ![Hinzufügen von MVC und Web-APIs](using-signalr-performance-counters-in-an-azure-web-role/_static/image3.png)
    
6. <span data-ttu-id="e8ba1-125">In **Projektmappen-Explorer**öffnen die *"Diagnostics.wadcfgx"* Datei unter **WebRole1**.</span><span class="sxs-lookup"><span data-stu-id="e8ba1-125">In **Solution Explorer**, open the *diagnostics.wadcfgx* file under **WebRole1**.</span></span>

   ![Projektmappen-Explorer "Diagnostics.wadcfgx"](using-signalr-performance-counters-in-an-azure-web-role/_static/image4.png)
    
7. <span data-ttu-id="e8ba1-127">Speichern Sie die Datei, und Ersetzen Sie den Inhalt der Datei mit der folgenden Konfiguration:</span><span class="sxs-lookup"><span data-stu-id="e8ba1-127">Replace the contents of the file with the following configuration and save the file:</span></span>

   [!code-xml[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample1.xml)]
    
8. <span data-ttu-id="e8ba1-128">Öffnen der **Package Manager Console** aus **Tools** > **NuGet Package Manager**.</span><span class="sxs-lookup"><span data-stu-id="e8ba1-128">Open the **Package Manager Console** from **Tools** > **NuGet Package Manager**.</span></span> <span data-ttu-id="e8ba1-129">Geben Sie die folgenden Befehle, um die neueste Version der SignalR und des SignalR-Hilfsprogramme-Pakets installieren:</span><span class="sxs-lookup"><span data-stu-id="e8ba1-129">Enter the following commands to install the latest version of SignalR and the SignalR utilities package:</span></span>

   [!code-powershell[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample2.ps1)]
    
9. <span data-ttu-id="e8ba1-130">Konfigurieren Sie die app aus, um die SignalR-Leistungsindikatoren in der Rolleninstanz zu installieren, wenn er startet oder wiederverwendet wird.</span><span class="sxs-lookup"><span data-stu-id="e8ba1-130">Configure the app to install the SignalR performance counters into the role instance when it starts up or recycles.</span></span> <span data-ttu-id="e8ba1-131">In **Projektmappen-Explorer**, mit der rechten Maustaste auf die **WebRole1** Projekt, und wählen Sie **hinzufügen** > **neuer Ordner**.</span><span class="sxs-lookup"><span data-stu-id="e8ba1-131">In **Solution Explorer**, right-click on the **WebRole1** project and select **Add** > **New Folder**.</span></span> <span data-ttu-id="e8ba1-132">Nennen Sie diesen Ordner *Start*.</span><span class="sxs-lookup"><span data-stu-id="e8ba1-132">Name the new folder *Startup*.</span></span>

   ![Startordner hinzufügen](using-signalr-performance-counters-in-an-azure-web-role/_static/image5.png)
    
10. <span data-ttu-id="e8ba1-134">Kopieren der *signalr.exe* Datei (hinzugefügt, mit der **Microsoft.AspNet.SignalR.Utils** Paket) von \<Projektordner > / SignalRPerfCounters/packages/Microsoft.AspNet.SignalR.Utils.\< Version > / tools für die *Start* Ordner, die Sie im vorherigen Schritt erstellt haben.</span><span class="sxs-lookup"><span data-stu-id="e8ba1-134">Copy the *signalr.exe* file (added with the **Microsoft.AspNet.SignalR.Utils** package) from \<project folder>/SignalRPerfCounters/packages/Microsoft.AspNet.SignalR.Utils.\<version>/tools to the *Startup* folder you created in the previous step.</span></span>

11. <span data-ttu-id="e8ba1-135">In **Projektmappen-Explorer**, mit der rechten Maustaste die *Start* Ordner, und wählen **hinzufügen** > **vorhandenes Element**.</span><span class="sxs-lookup"><span data-stu-id="e8ba1-135">In **Solution Explorer**, right-click the *Startup* folder and select **Add** > **Existing Item**.</span></span> <span data-ttu-id="e8ba1-136">Wählen Sie im daraufhin angezeigten Dialogfeld *signalr.exe* , und wählen Sie **hinzufügen**.</span><span class="sxs-lookup"><span data-stu-id="e8ba1-136">In the dialog that appears, select *signalr.exe* and select **Add**.</span></span>

    ![Fügen Sie signalr.exe Projekt hinzu](using-signalr-performance-counters-in-an-azure-web-role/_static/image6.png)
    
12. <span data-ttu-id="e8ba1-138">Mit der rechten Maustaste auf die *Start* neu erstellten Ordner.</span><span class="sxs-lookup"><span data-stu-id="e8ba1-138">Right-click on the *Startup* folder you created.</span></span> <span data-ttu-id="e8ba1-139">Klicken Sie auf **Hinzufügen** > **Neues Element**.</span><span class="sxs-lookup"><span data-stu-id="e8ba1-139">Select **Add** > **New Item**.</span></span> <span data-ttu-id="e8ba1-140">Wählen Sie die **allgemeine** Knoten **Textdatei**, und nennen Sie das neue Element *SignalRPerfCounterInstall.cmd*.</span><span class="sxs-lookup"><span data-stu-id="e8ba1-140">Select the **General** node, select **Text File**, and name the new item *SignalRPerfCounterInstall.cmd*.</span></span> <span data-ttu-id="e8ba1-141">Diese Befehlsdatei installiert SignalR-Leistungsindikatoren in der Rolle "Web".</span><span class="sxs-lookup"><span data-stu-id="e8ba1-141">This command file will install the SignalR performance counters into the web role.</span></span>

    ![Erstellen von SignalR Performance Counter-Batch-Installationsdatei](using-signalr-performance-counters-in-an-azure-web-role/_static/image7.png)
     
13. <span data-ttu-id="e8ba1-143">Wenn Visual Studio erstellt die *SignalRPerfCounterInstall.cmd* Datei, es wird automatisch geöffnet im Hauptfenster.</span><span class="sxs-lookup"><span data-stu-id="e8ba1-143">When Visual Studio creates the *SignalRPerfCounterInstall.cmd* file, it will automatically open in the main window.</span></span> <span data-ttu-id="e8ba1-144">Ersetzen Sie den Inhalt der Datei mit dem folgenden Skript speichern Sie und schließen Sie die Datei.</span><span class="sxs-lookup"><span data-stu-id="e8ba1-144">Replace the contents of the file with the following script, then save and close the file.</span></span> <span data-ttu-id="e8ba1-145">Dieses Skript führt *signalr.exe*, der die Rolleninstanz die SignalR-Leistungsindikatoren hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="e8ba1-145">This script executes *signalr.exe*, which adds the SignalR performance counters to the role instance.</span></span>

    [!code-console[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample3.cmd)]
    
14. <span data-ttu-id="e8ba1-146">Wählen Sie die *signalr.exe* Datei **Projektmappen-Explorer**.</span><span class="sxs-lookup"><span data-stu-id="e8ba1-146">Select the *signalr.exe* file in **Solution Explorer**.</span></span> <span data-ttu-id="e8ba1-147">In der Datei **Eigenschaften**legen **in Ausgabeverzeichnis kopieren** auf **immer kopieren**.</span><span class="sxs-lookup"><span data-stu-id="e8ba1-147">In the file's **Properties**, set **Copy to Output Directory** to **Copy Always**.</span></span>

    ![Legen Sie in Ausgabeverzeichnis kopieren immer kopieren](using-signalr-performance-counters-in-an-azure-web-role/_static/image8.png)
    
15. <span data-ttu-id="e8ba1-149">Wiederholen Sie den vorherigen Schritt für die *SignalRPerfCounterInstall.cmd* Datei.</span><span class="sxs-lookup"><span data-stu-id="e8ba1-149">Repeat the previous step for the *SignalRPerfCounterInstall.cmd* file.</span></span>

    
16. <span data-ttu-id="e8ba1-150">Mit der rechten Maustaste auf die *SignalRPerfCounterInstall.cmd* Datei, und wählen Sie **Öffnen mit**.</span><span class="sxs-lookup"><span data-stu-id="e8ba1-150">Right-click on the *SignalRPerfCounterInstall.cmd* file and select **Open With**.</span></span> <span data-ttu-id="e8ba1-151">Wählen Sie im daraufhin angezeigten Dialogfeld **Binär-Editor** , und wählen Sie **OK**.</span><span class="sxs-lookup"><span data-stu-id="e8ba1-151">In the dialog that appears, select **Binary Editor** and select **OK**.</span></span>

    ![Mit dem Binär-Editor öffnen](using-signalr-performance-counters-in-an-azure-web-role/_static/image9.png)
    
17. <span data-ttu-id="e8ba1-153">Klicken Sie im Binär-Editor wählen Sie alle führenden Bytes in der Datei, und löschen Sie sie.</span><span class="sxs-lookup"><span data-stu-id="e8ba1-153">In the binary editor, select any leading bytes in the file and delete them.</span></span> <span data-ttu-id="e8ba1-154">Speichern und schließen Sie die Datei.</span><span class="sxs-lookup"><span data-stu-id="e8ba1-154">Save and close the file.</span></span>

    ![Führende Bytes löschen](using-signalr-performance-counters-in-an-azure-web-role/_static/image10.png)
    
18. <span data-ttu-id="e8ba1-156">Open *"Servicedefinition.csdef"* und fügen Sie eine Startaufgabe, die ausgeführt wird die *SignalrPerfCounterInstall.cmd* Datei, wenn der Dienst wird gestartet:</span><span class="sxs-lookup"><span data-stu-id="e8ba1-156">Open *ServiceDefinition.csdef* and add a startup task that executes the *SignalrPerfCounterInstall.cmd* file when the service starts up:</span></span>

    [!code-xml[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample4.xml?highlight=4-7)]
    
19. <span data-ttu-id="e8ba1-157">Open `Views/Shared/_Layout.cshtml` und jQuery-Paket-Skript am Ende der Datei entfernen.</span><span class="sxs-lookup"><span data-stu-id="e8ba1-157">Open `Views/Shared/_Layout.cshtml` and remove the jQuery bundle script from the end of the file.</span></span>

    [!code-cshtml[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample5.cshtml)]
    
20. <span data-ttu-id="e8ba1-158">Hinzufügen eines JavaScript-Clients, die ständig aufruft die `increment` Methode auf dem Server.</span><span class="sxs-lookup"><span data-stu-id="e8ba1-158">Add a JavaScript client that continuously calls the `increment` method on the server.</span></span> <span data-ttu-id="e8ba1-159">Open `Views/Home/Index.cshtml` und Ersetzen Sie den Inhalt durch folgenden Code:</span><span class="sxs-lookup"><span data-stu-id="e8ba1-159">Open `Views/Home/Index.cshtml` and replace the contents with the following code:</span></span>

    [!code-cshtml[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample6.cshtml)]
    
21. <span data-ttu-id="e8ba1-160">Erstellen eines neuen Ordners in der **WebRole1** Projekt mit dem Namen *Hubs*.</span><span class="sxs-lookup"><span data-stu-id="e8ba1-160">Create a new folder in the **WebRole1** project named *Hubs*.</span></span> <span data-ttu-id="e8ba1-161">Mit der rechten Maustaste die *Hubs* Ordner **Projektmappen-Explorer**Option **Web** > **SignalR**, und wählen Sie  **SignalR-Hub-Klasse (v2)**.</span><span class="sxs-lookup"><span data-stu-id="e8ba1-161">Right-click the *Hubs* folder in **Solution Explorer**, select **Web** > **SignalR**, and select **SignalR Hub Class (v2)**.</span></span> <span data-ttu-id="e8ba1-162">Name des neuen Hubs *MyHub.cs* , und wählen Sie **hinzufügen**.</span><span class="sxs-lookup"><span data-stu-id="e8ba1-162">Name the new hub *MyHub.cs* and select **Add**.</span></span>

    ![Hinzufügen von SignalR-Hub-Klasse in den Hubs-Ordner, klicken Sie im Dialogfeld "Neues Element hinzufügen"](using-signalr-performance-counters-in-an-azure-web-role/_static/image13.png)

22. <span data-ttu-id="e8ba1-164">*MyHub.cs* wird im Hauptfenster automatisch geöffnet.</span><span class="sxs-lookup"><span data-stu-id="e8ba1-164">*MyHub.cs* will automatically open in the main window.</span></span> <span data-ttu-id="e8ba1-165">Ersetzen Sie den Inhalt durch den folgenden Code, und klicken Sie dann speichern Sie und schließen Sie die Datei:</span><span class="sxs-lookup"><span data-stu-id="e8ba1-165">Replace the contents with the following code, then save and close the file:</span></span>

    [!code-csharp[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample7.cs)]
    
23. <span data-ttu-id="e8ba1-166">*[Crank.exe](signalr-connection-density-testing-with-crank.md)*  eine Verbindung Dichte Testtool mit SignalR Codebasis bereitgestellt wird.</span><span class="sxs-lookup"><span data-stu-id="e8ba1-166">*[Crank.exe](signalr-connection-density-testing-with-crank.md)* is a connection density testing tool provided with the SignalR codebase.</span></span> <span data-ttu-id="e8ba1-167">Da tatsächlich eine dauerhafte Verbindung erforderlich ist, fügen Sie eine auf Ihrer Website für die Verwendung beim Testen.</span><span class="sxs-lookup"><span data-stu-id="e8ba1-167">Since Crank requires a persistent connection, you add one to your site for use when testing.</span></span> <span data-ttu-id="e8ba1-168">Fügen Sie einen neuen Ordner auf dem **WebRole1** Projekt mit der Bezeichnung *PersistentConnections*.</span><span class="sxs-lookup"><span data-stu-id="e8ba1-168">Add a new folder to the **WebRole1** project called *PersistentConnections*.</span></span> <span data-ttu-id="e8ba1-169">Mit der rechten Maustaste in diesem Ordner, und wählen Sie **hinzufügen** > **Klasse**.</span><span class="sxs-lookup"><span data-stu-id="e8ba1-169">Right-click this folder and select **Add** > **Class**.</span></span> <span data-ttu-id="e8ba1-170">Nennen Sie die neue Klassendatei *MyPersistentConnections.cs* , und wählen Sie **hinzufügen**.</span><span class="sxs-lookup"><span data-stu-id="e8ba1-170">Name the new class file *MyPersistentConnections.cs* and select **Add**.</span></span>

24. <span data-ttu-id="e8ba1-171">Visual Studio öffnet die *MyPersistentConnections.cs* Datei im Hauptfenster.</span><span class="sxs-lookup"><span data-stu-id="e8ba1-171">Visual Studio will open the *MyPersistentConnections.cs* file in the main window.</span></span> <span data-ttu-id="e8ba1-172">Ersetzen Sie den Inhalt durch den folgenden Code, und klicken Sie dann speichern Sie und schließen Sie die Datei:</span><span class="sxs-lookup"><span data-stu-id="e8ba1-172">Replace the contents with the following code, then save and close the file:</span></span>

    [!code-csharp[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample8.cs)]
    
25. <span data-ttu-id="e8ba1-173">Mithilfe der `Startup` -Klasse, die SignalR-Objekte zu starten, wenn OWIN wird gestartet.</span><span class="sxs-lookup"><span data-stu-id="e8ba1-173">Using the `Startup` class, the SignalR objects start when OWIN starts up.</span></span> <span data-ttu-id="e8ba1-174">Öffnen oder erstellen Sie *Startup.cs* und Ersetzen Sie den Inhalt durch folgenden Code:</span><span class="sxs-lookup"><span data-stu-id="e8ba1-174">Open or create *Startup.cs* and replace the contents with the following code:</span></span>

    [!code-csharp[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample9.cs)]
    
    <span data-ttu-id="e8ba1-175">Im obigen Code die `OwinStartup` Attribut markiert, dieser Klasse, um OWIN zu starten.</span><span class="sxs-lookup"><span data-stu-id="e8ba1-175">In the code above, the `OwinStartup` attribute marks this class to start OWIN.</span></span> <span data-ttu-id="e8ba1-176">Die `Configuration` Methode startet SignalR.</span><span class="sxs-lookup"><span data-stu-id="e8ba1-176">The `Configuration` method starts SignalR.</span></span>
    
26. <span data-ttu-id="e8ba1-177">Testen Sie Ihre Anwendung in Microsoft Azure-Emulator durch Drücken von **F5**.</span><span class="sxs-lookup"><span data-stu-id="e8ba1-177">Test your application in the Microsoft Azure Emulator by pressing **F5**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="e8ba1-178">Treten eine **FileLoadException** am **MapSignalR**, ändern Sie die bindungsumleitungen in *"Web.config"* , der dem folgenden:</span><span class="sxs-lookup"><span data-stu-id="e8ba1-178">If you encounter a **FileLoadException** at **MapSignalR**, change the binding redirects in *web.config* to the following:</span></span>

    [!code-xml[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample12.xml?highlight=3,7)]
    
27. <span data-ttu-id="e8ba1-179">Warten Sie ca. eine Minute.</span><span class="sxs-lookup"><span data-stu-id="e8ba1-179">Wait about one minute.</span></span> <span data-ttu-id="e8ba1-180">Öffnen Sie das Cloud-Explorer-Toolfenster in Visual Studio (**Ansicht** > **Cloud Explorer**), und erweitern Sie den Pfad `(Local)/Storage Accounts/(Development)/Tables`.</span><span class="sxs-lookup"><span data-stu-id="e8ba1-180">Open the Cloud Explorer tool window in Visual Studio (**View** > **Cloud Explorer**) and expand the path `(Local)/Storage Accounts/(Development)/Tables`.</span></span> <span data-ttu-id="e8ba1-181">Doppelklicken Sie auf **WADPerformanceCountersTable**.</span><span class="sxs-lookup"><span data-stu-id="e8ba1-181">Double-click **WADPerformanceCountersTable**.</span></span> <span data-ttu-id="e8ba1-182">SignalR-Leistungsindikatoren in den Tabellendaten sollte angezeigt werden.</span><span class="sxs-lookup"><span data-stu-id="e8ba1-182">You should see SignalR counters in the table data.</span></span> <span data-ttu-id="e8ba1-183">Wenn die Tabelle nicht angezeigt wird, müssen Sie möglicherweise Ihre Azure-Speicher Anmeldeinformationen erneut einzugeben.</span><span class="sxs-lookup"><span data-stu-id="e8ba1-183">If you don't see the table, you may need to re-enter your Azure Storage credentials.</span></span> <span data-ttu-id="e8ba1-184">Sie müssen auswählen der **aktualisieren** Schaltfläche, um die Tabelle in finden Sie unter **Cloud Explorer** , oder wählen Sie die **aktualisieren** Schaltfläche im Fenster Tabelle öffnen, um Daten in der Tabelle finden Sie unter.</span><span class="sxs-lookup"><span data-stu-id="e8ba1-184">You may need to select the **Refresh** button to see the table in **Cloud Explorer** or select the **Refresh** button in the open table window to see data in the table.</span></span>

    ![Auswählen von der WAD-Leistungsindikatortabelle im Cloud-Explorer von Visual Studio](using-signalr-performance-counters-in-an-azure-web-role/_static/image11.png)

    ![Zeigt die Leistungsindikatoren gesammelt, in der WAD-Leistungsindikatortabelle](using-signalr-performance-counters-in-an-azure-web-role/_static/image12.png)
    
28. <span data-ttu-id="e8ba1-187">Aktualisieren Sie zum Testen der Anwendungsstatus in der Cloud Die **ServiceConfiguration.Cloud.cscfg** und legen Sie die `Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString` auf eine gültige Verbindungszeichenfolge für den Azure Storage-Konto.</span><span class="sxs-lookup"><span data-stu-id="e8ba1-187">To test your application in the cloud, update the **ServiceConfiguration.Cloud.cscfg** file and set the `Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString` to a valid Azure Storage account connection string.</span></span>

    [!code-xml[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample10.xml)]

29. <span data-ttu-id="e8ba1-188">Bereitstellen Sie die Anwendung auf Ihrem Azure-Abonnement.</span><span class="sxs-lookup"><span data-stu-id="e8ba1-188">Deploy the application to your Azure subscription.</span></span> <span data-ttu-id="e8ba1-189">Weitere Informationen zum Bereitstellen einer Anwendung in Azure finden Sie unter [zum Erstellen und Bereitstellen eines Cloud-Diensts](https://docs.microsoft.com/azure/cloud-services/cloud-services-how-to-create-deploy).</span><span class="sxs-lookup"><span data-stu-id="e8ba1-189">For details on how to deploy an application to Azure, see [How to Create and Deploy a Cloud Service](https://docs.microsoft.com/azure/cloud-services/cloud-services-how-to-create-deploy).</span></span>

30. <span data-ttu-id="e8ba1-190">Warten Sie einige Minuten.</span><span class="sxs-lookup"><span data-stu-id="e8ba1-190">Wait a few minutes.</span></span> <span data-ttu-id="e8ba1-191">In **Cloud Explorer**, suchen Sie das Speicherkonto, das Sie oben konfiguriert, und suchen die `WADPerformanceCountersTable` Tabelle darin.</span><span class="sxs-lookup"><span data-stu-id="e8ba1-191">In **Cloud Explorer**, locate the storage account you configured above and find the `WADPerformanceCountersTable` table in it.</span></span> <span data-ttu-id="e8ba1-192">SignalR-Leistungsindikatoren in den Tabellendaten sollte angezeigt werden.</span><span class="sxs-lookup"><span data-stu-id="e8ba1-192">You should see SignalR counters in the table data.</span></span> <span data-ttu-id="e8ba1-193">Wenn die Tabelle nicht angezeigt wird, müssen Sie möglicherweise Ihre Azure-Speicher Anmeldeinformationen erneut einzugeben.</span><span class="sxs-lookup"><span data-stu-id="e8ba1-193">If you don't see the table, you may need to re-enter your Azure Storage credentials.</span></span> <span data-ttu-id="e8ba1-194">Sie müssen auswählen der **aktualisieren** Schaltfläche, um die Tabelle in finden Sie unter **Cloud Explorer** , oder wählen Sie die **aktualisieren** Schaltfläche im Fenster Tabelle öffnen, um Daten in der Tabelle finden Sie unter.</span><span class="sxs-lookup"><span data-stu-id="e8ba1-194">You may need to select the **Refresh** button to see the table in **Cloud Explorer** or select the **Refresh** button in the open table window to see data in the table.</span></span>

<span data-ttu-id="e8ba1-195">Besonderen Dank an [Martin Richard](https://social.msdn.microsoft.com/profile/Martin+Richard) für den ursprünglichen Inhalt in diesem Lernprogramm verwendet.</span><span class="sxs-lookup"><span data-stu-id="e8ba1-195">Special thanks to [Martin Richard](https://social.msdn.microsoft.com/profile/Martin+Richard) for the original content used in this tutorial.</span></span>
