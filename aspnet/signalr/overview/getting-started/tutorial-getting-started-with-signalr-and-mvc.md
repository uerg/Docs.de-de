---
uid: signalr/overview/getting-started/tutorial-getting-started-with-signalr-and-mvc
title: 'Lernprogramm: Erste Schritte mit SignalR 2 und MVC 5 | Microsoft Docs'
author: pfletcher
description: Dieses Lernprogramm zeigt, wie ASP.NET SignalR 2 eine Echtzeit-Chat-Anwendung erstellen. Fügen SignalR zu einer MVC 5-Anwendung und erstellen Sie eine Ansicht Chat...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2014
ms.topic: article
ms.assetid: 80bfe5fb-bdfc-41fe-ac43-2132e5d69fac
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/getting-started/tutorial-getting-started-with-signalr-and-mvc
msc.type: authoredcontent
ms.openlocfilehash: ca0471114da7363c5df9d459308708e7ab4f8b84
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/24/2018
---
<a name="tutorial-getting-started-with-signalr-2-and-mvc-5"></a><span data-ttu-id="30509-104">Lernprogramm: Erste Schritte mit SignalR 2 und MVC 5</span><span class="sxs-lookup"><span data-stu-id="30509-104">Tutorial: Getting Started with SignalR 2 and MVC 5</span></span>
====================
<span data-ttu-id="30509-105">durch [Patrick Fletcher](https://github.com/pfletcher), [Tim Teebken](https://github.com/timlt)</span><span class="sxs-lookup"><span data-stu-id="30509-105">by [Patrick Fletcher](https://github.com/pfletcher), [Tim Teebken](https://github.com/timlt)</span></span>

[<span data-ttu-id="30509-106">Herunterladen des abgeschlossenen Projekts</span><span class="sxs-lookup"><span data-stu-id="30509-106">Download Completed Project</span></span>](http://code.msdn.microsoft.com/Getting-Started-with-c366b2f3)

> <span data-ttu-id="30509-107">Dieses Lernprogramm zeigt, wie ASP.NET SignalR 2 eine Echtzeit-Chat-Anwendung erstellen.</span><span class="sxs-lookup"><span data-stu-id="30509-107">This tutorial shows how to use ASP.NET SignalR 2 to create a real-time chat application.</span></span> <span data-ttu-id="30509-108">Sie werden einer MVC 5-Anwendung SignalR hinzu, und erstellen eine Chat-Ansicht zum Senden und Meldungen angezeigt werden.</span><span class="sxs-lookup"><span data-stu-id="30509-108">You will add SignalR to an MVC 5 application and create a chat view to send and display messages.</span></span> 
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="30509-109">In diesem Lernprogramm verwendeten Versionen der Software</span><span class="sxs-lookup"><span data-stu-id="30509-109">Software versions used in the tutorial</span></span>
> 
> 
> - [<span data-ttu-id="30509-110">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="30509-110">Visual Studio 2013</span></span>](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - <span data-ttu-id="30509-111">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="30509-111">.NET 4.5</span></span>
> - <span data-ttu-id="30509-112">MVC 5</span><span class="sxs-lookup"><span data-stu-id="30509-112">MVC 5</span></span>
> - <span data-ttu-id="30509-113">SignalR Version 2</span><span class="sxs-lookup"><span data-stu-id="30509-113">SignalR version 2</span></span>
>   
> 
> 
> ## <a name="using-visual-studio-2012-with-this-tutorial"></a><span data-ttu-id="30509-114">Verwenden von Visual Studio 2012 mit diesem Lernprogramm</span><span class="sxs-lookup"><span data-stu-id="30509-114">Using Visual Studio 2012 with this tutorial</span></span>
> 
> 
> <span data-ttu-id="30509-115">Um Visual Studio 2012 mit diesem Lernprogramm verwenden, führen Sie folgende Schritte aus:</span><span class="sxs-lookup"><span data-stu-id="30509-115">To use Visual Studio 2012 with this tutorial, do the following:</span></span>
> 
> - <span data-ttu-id="30509-116">Update der [Package Manager](http://docs.nuget.org/docs/start-here/installing-nuget) auf die neueste Version.</span><span class="sxs-lookup"><span data-stu-id="30509-116">Update your [Package Manager](http://docs.nuget.org/docs/start-here/installing-nuget) to the latest version.</span></span>
> - <span data-ttu-id="30509-117">Installieren der [Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx).</span><span class="sxs-lookup"><span data-stu-id="30509-117">Install the [Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx).</span></span>
> - <span data-ttu-id="30509-118">Suchen Sie in den Webplattform-Installer, und installieren Sie **ASP.NET und 2013.1 von Web Tools für Visual Studio 2012**.</span><span class="sxs-lookup"><span data-stu-id="30509-118">In the Web Platform Installer, search for and install **ASP.NET and Web Tools 2013.1 for Visual Studio 2012**.</span></span> <span data-ttu-id="30509-119">Hierbei werden z. B. Visual Studio-Vorlagen für SignalR-Klassen installiert **Hub**.</span><span class="sxs-lookup"><span data-stu-id="30509-119">This will install Visual Studio templates for SignalR classes such as **Hub**.</span></span>
> - <span data-ttu-id="30509-120">Einige Vorlagen (z. B. **OWIN-Startklasse**) ist nicht verfügbar; verwenden Sie für diese stattdessen eine Klassendatei.</span><span class="sxs-lookup"><span data-stu-id="30509-120">Some templates (such as **OWIN Startup Class**) will not be available; for these, use a Class file instead.</span></span>
> 
> 
> ## <a name="tutorial-versions"></a><span data-ttu-id="30509-121">Lernprogramm-Versionen</span><span class="sxs-lookup"><span data-stu-id="30509-121">Tutorial Versions</span></span>
> 
> <span data-ttu-id="30509-122">Informationen zu früheren Versionen von SignalR finden Sie unter [ältere Versionen von SignalR](../older-versions/index.md).</span><span class="sxs-lookup"><span data-stu-id="30509-122">For information about earlier versions of SignalR, see [SignalR Older Versions](../older-versions/index.md).</span></span>
> 
> ## <a name="questions-and-comments"></a><span data-ttu-id="30509-123">Fragen und Kommentare</span><span class="sxs-lookup"><span data-stu-id="30509-123">Questions and comments</span></span>
> 
> <span data-ttu-id="30509-124">Lassen Sie Sie Feedback auf wie in diesem Lernprogramm mögen und was wir in den Kommentaren am unteren Rand der Seite verbessern können.</span><span class="sxs-lookup"><span data-stu-id="30509-124">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="30509-125">Wenn Sie Fragen, die nicht direkt mit dem Lernprogramm verknüpft sind haben, bereitstellen können, die [ASP.NET SignalR-Forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) oder [StackOverflow.com](http://stackoverflow.com/).</span><span class="sxs-lookup"><span data-stu-id="30509-125">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>


## <a name="overview"></a><span data-ttu-id="30509-126">Übersicht</span><span class="sxs-lookup"><span data-stu-id="30509-126">Overview</span></span>

<span data-ttu-id="30509-127">Dieses Lernprogramm führt Sie in Echtzeit-Anwendungsentwicklung ASP.NET SignalR 2 mit ASP.NET MVC 5.</span><span class="sxs-lookup"><span data-stu-id="30509-127">This tutorial introduces you to real-time web application development with ASP.NET SignalR 2 and ASP.NET MVC 5.</span></span> <span data-ttu-id="30509-128">Das Lernprogramm verwendet den gleichen Chat-Anwendungscode als die [SignalR Getting Started Tutorial](tutorial-getting-started-with-signalr.md), jedoch wird gezeigt, wie sie einer MVC 5-Anwendung hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="30509-128">The tutorial uses the same chat application code as the [SignalR Getting Started tutorial](tutorial-getting-started-with-signalr.md), but shows how to add it to an MVC 5 application.</span></span>

<span data-ttu-id="30509-129">In diesem Thema erfahren Sie die folgenden SignalR Entwicklungsaufgaben:</span><span class="sxs-lookup"><span data-stu-id="30509-129">In this topic you will learn the following SignalR development tasks:</span></span>

- <span data-ttu-id="30509-130">Einer MVC 5-Anwendung hinzugefügt die SignalR-Bibliothek.</span><span class="sxs-lookup"><span data-stu-id="30509-130">Adding the SignalR library to an MVC 5 application.</span></span>
- <span data-ttu-id="30509-131">Erstellen Hub- und OWIN-Start-Klassen, die Inhalt an Clients zu verteilen.</span><span class="sxs-lookup"><span data-stu-id="30509-131">Creating hub and OWIN startup classes to push content to clients.</span></span>
- <span data-ttu-id="30509-132">Verwenden die SignalR-jQuery-Bibliothek auf einer Webseite zum Senden von Nachrichten und Updates vom Hub angezeigt.</span><span class="sxs-lookup"><span data-stu-id="30509-132">Using the SignalR jQuery library in a web page to send messages and display updates from the hub.</span></span>

<span data-ttu-id="30509-133">Der folgende Screenshot zeigt die abgeschlossenen chatanwendung, die in einem Browser ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="30509-133">The following screen shot shows the completed chat application running in a browser.</span></span>

![Chatinstanzen](tutorial-getting-started-with-signalr-and-mvc/_static/image1.png)

<span data-ttu-id="30509-135">Abschnitte:</span><span class="sxs-lookup"><span data-stu-id="30509-135">Sections:</span></span>

- [<span data-ttu-id="30509-136">Richten Sie das Projekt</span><span class="sxs-lookup"><span data-stu-id="30509-136">Set up the Project</span></span>](#setup)
- [<span data-ttu-id="30509-137">Führen Sie das Beispiel</span><span class="sxs-lookup"><span data-stu-id="30509-137">Run the Sample</span></span>](#run)
- [<span data-ttu-id="30509-138">Überprüfen Sie den Code</span><span class="sxs-lookup"><span data-stu-id="30509-138">Examine the Code</span></span>](#code)
- [<span data-ttu-id="30509-139">Nächste Schritte</span><span class="sxs-lookup"><span data-stu-id="30509-139">Next Steps</span></span>](#next)

<a id="setup"></a>

## <a name="set-up-the-project"></a><span data-ttu-id="30509-140">Richten Sie das Projekt</span><span class="sxs-lookup"><span data-stu-id="30509-140">Set up the Project</span></span>

<span data-ttu-id="30509-141">Erforderliche Komponenten:</span><span class="sxs-lookup"><span data-stu-id="30509-141">Prerequisites:</span></span>

- <span data-ttu-id="30509-142">Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="30509-142">Visual Studio 2013.</span></span> <span data-ttu-id="30509-143">Wenn Sie nicht über Visual Studio verfügen, finden Sie unter [ASP.NET Downloads](https://www.asp.net/downloads) das kostenlose Visual Studio 2013 Express Entwicklungstool abgerufen.</span><span class="sxs-lookup"><span data-stu-id="30509-143">If you do not have Visual Studio, see [ASP.NET Downloads](https://www.asp.net/downloads) to get the free Visual Studio 2013 Express Development Tool.</span></span>

<span data-ttu-id="30509-144">In diesem Abschnitt veranschaulicht das Erstellen einer ASP.NET MVC 5-Anwendung, die SignalR-Bibliothek hinzufügen und die chatanwendung erstellen.</span><span class="sxs-lookup"><span data-stu-id="30509-144">This section shows how to create an ASP.NET MVC 5 application, add the SignalR library, and create the chat application.</span></span>

1. <span data-ttu-id="30509-145">Klicken Sie in Visual Studio Erstellen einer c# ASP.NET-Anwendung mit der Zielversion .NET Framework 4.5, nennen Sie sie SignalRChat, und klicken Sie auf OK.</span><span class="sxs-lookup"><span data-stu-id="30509-145">In Visual Studio, create a C# ASP.NET application that targets .NET Framework 4.5, name it SignalRChat, and click OK.</span></span>

    ![Erstellen von web](tutorial-getting-started-with-signalr-and-mvc/_static/image2.png)
2. <span data-ttu-id="30509-147">In der `New ASP.NET Project` Dialogfeld, und wählen Sie **MVC**, und klicken Sie auf **Authentifizierung ändern**.</span><span class="sxs-lookup"><span data-stu-id="30509-147">In the `New ASP.NET Project` dialog, and select **MVC**, and click **Change Authentication**.</span></span>

    ![Erstellen von web](tutorial-getting-started-with-signalr-and-mvc/_static/image3.png)
3. <span data-ttu-id="30509-149">Wählen Sie **keine Authentifizierung** in der **Authentifizierung ändern** Dialogfeld ein, und klicken Sie auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="30509-149">Select **No Authentication** in the **Change Authentication** dialog, and click **OK**.</span></span>

    ![Wählen Sie die keine Authentifizierung](tutorial-getting-started-with-signalr-and-mvc/_static/image4.png)

    > [!NOTE]
    > <span data-ttu-id="30509-151">Bei Auswahl einen anderen Authentifizierungsanbieter für Ihre Anwendung eine `Startup.cs` Klasse wird für Sie erstellt werden; Sie müssen keine Erstellen eigener `Startup.cs` Klasse in Schritt 10 unten.</span><span class="sxs-lookup"><span data-stu-id="30509-151">If you select a different authentication provider for your application, a `Startup.cs` class will be created for you; you will not need to create your own `Startup.cs` class in step 10 below.</span></span>
4. <span data-ttu-id="30509-152">Klicken Sie auf **OK** in der **neues ASP.NET-Projekt** Dialogfeld.</span><span class="sxs-lookup"><span data-stu-id="30509-152">Click **OK** in the **New ASP.NET Project** dialog.</span></span>
5. <span data-ttu-id="30509-153">Öffnen der **Tools | Bibliothek-Paket-Manager | Paket-Manager-Konsole** und führen Sie den folgenden Befehl.</span><span class="sxs-lookup"><span data-stu-id="30509-153">Open the **Tools | Library Package Manager | Package Manager Console** and run the following command.</span></span> <span data-ttu-id="30509-154">Dieser Schritt fügt dem Projekt eine Reihe von Skriptdateien und Assemblyverweisen, die SignalR-Funktionen zu aktivieren.</span><span class="sxs-lookup"><span data-stu-id="30509-154">This step adds to the project a set of script files and assembly references that enable SignalR functionality.</span></span>

    `install-package Microsoft.AspNet.SignalR`
6. <span data-ttu-id="30509-155">In **Projektmappen-Explorer**, erweitern Sie den Ordner "Skripts".</span><span class="sxs-lookup"><span data-stu-id="30509-155">In **Solution Explorer**, expand the Scripts folder.</span></span> <span data-ttu-id="30509-156">Beachten Sie, dass das Projekt Skriptbibliotheken für SignalR hinzugefügt wurde.</span><span class="sxs-lookup"><span data-stu-id="30509-156">Note that script libraries for SignalR have been added to the project.</span></span>

    ![Ordner "Skripts"](tutorial-getting-started-with-signalr-and-mvc/_static/image5.png)
7. <span data-ttu-id="30509-158">In **Projektmappen-Explorer**mit der rechten Maustaste auf das Projekt, wählen Sie **hinzufügen | Neuer Ordner**, und fügen Sie einen neuen Ordner namens **Hubs**.</span><span class="sxs-lookup"><span data-stu-id="30509-158">In **Solution Explorer**, right-click the project, select **Add | New Folder**, and add a new folder named **Hubs**.</span></span>
8. <span data-ttu-id="30509-159">Mit der rechten Maustaste die **Hubs** Ordner, klicken Sie auf **hinzufügen | Neues Element**, wählen die **Visual C# | Web | SignalR** Knoten in der **installiert** klicken Sie im Bereich **SignalR-Hub-Klasse (v2)** aus der Mitte, und erstellen Sie einen neuen Hub mit dem Namen **ChatHub.cs**.</span><span class="sxs-lookup"><span data-stu-id="30509-159">Right-click the **Hubs** folder, click **Add | New Item**, select the **Visual C# | Web | SignalR** node in the **Installed** pane, select **SignalR Hub Class (v2)** from the center pane, and create a new hub named **ChatHub.cs**.</span></span> <span data-ttu-id="30509-160">Sie verwenden diese Klasse als einen SignalR-Hub, die Nachrichten für alle Clients sendet.</span><span class="sxs-lookup"><span data-stu-id="30509-160">You will use this class as a SignalR server hub that sends messages to all clients.</span></span>

    ![Erstellen des neuen Hubs](tutorial-getting-started-with-signalr-and-mvc/_static/image6.png)
9. <span data-ttu-id="30509-162">Ersetzen Sie den Code in der **ChatHub** Klasse durch den folgenden Code.</span><span class="sxs-lookup"><span data-stu-id="30509-162">Replace the code in the **ChatHub** class with the following code.</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample1.cs)]
10. <span data-ttu-id="30509-163">Erstellen Sie eine neue Klasse namens Startup.cs.</span><span class="sxs-lookup"><span data-stu-id="30509-163">Create a new class called Startup.cs.</span></span> <span data-ttu-id="30509-164">Ändern Sie den Inhalt der Datei in den folgenden.</span><span class="sxs-lookup"><span data-stu-id="30509-164">Change the contents of the file to the following.</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample2.cs)]
11. <span data-ttu-id="30509-165">Bearbeiten der `HomeController` Klasse gefunden **Controllers/HomeController.cs** und fügen Sie die folgende Methode der Klasse.</span><span class="sxs-lookup"><span data-stu-id="30509-165">Edit the `HomeController` class found in **Controllers/HomeController.cs** and add the following method to the class.</span></span> <span data-ttu-id="30509-166">Diese Methode gibt die **Chat** anzeigen, die Sie in einem späteren Schritt erstellen.</span><span class="sxs-lookup"><span data-stu-id="30509-166">This method returns the **Chat** view that you will create in a later step.</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample3.cs)]
12. <span data-ttu-id="30509-167">Mit der rechten Maustaste die **Ansichten/Start** Ordner, und wählen **hinzufügen... | Ansicht**.</span><span class="sxs-lookup"><span data-stu-id="30509-167">Right-click the **Views/Home** folder, and select **Add... | View**.</span></span>
13. <span data-ttu-id="30509-168">In der **Ansicht hinzufügen** im Dialogfeld die Namen der neuen Sicht **Chat**.</span><span class="sxs-lookup"><span data-stu-id="30509-168">In the **Add View** dialog, name the new view **Chat**.</span></span>

    ![Hinzufügen einer Ansicht](tutorial-getting-started-with-signalr-and-mvc/_static/image7.png)
14. <span data-ttu-id="30509-170">Ersetzen Sie den Inhalt des **Chat.cshtml** durch den folgenden Code.</span><span class="sxs-lookup"><span data-stu-id="30509-170">Replace the contents of **Chat.cshtml** with the following code.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="30509-171">Wenn Sie Visual Studio-Projekts SignalR und andere Skriptbibliotheken hinzugefügt haben, könnten die Paket-Manager eine Version der SignalR-Skriptdatei installieren, die aktueller als die Version, die in diesem Thema dargestellt sind.</span><span class="sxs-lookup"><span data-stu-id="30509-171">When you add SignalR and other script libraries to your Visual Studio project, the Package Manager might install a version of the SignalR script file that is more recent than the version shown in this topic.</span></span> <span data-ttu-id="30509-172">Stellen Sie sicher, dass im Code wird der Verweis auf das Skript der Version der Skriptbibliothek, die in Ihrem Projekt installiert entspricht.</span><span class="sxs-lookup"><span data-stu-id="30509-172">Make sure that the script reference in your code matches the version of the script library installed in your project.</span></span>

    [!code-cshtml[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample4.cshtml)]
15. <span data-ttu-id="30509-173">**Speichern Sie alle** für das Projekt.</span><span class="sxs-lookup"><span data-stu-id="30509-173">**Save All** for the project.</span></span>

<a id="run"></a>

## <a name="run-the-sample"></a><span data-ttu-id="30509-174">Führen Sie das Beispiel</span><span class="sxs-lookup"><span data-stu-id="30509-174">Run the Sample</span></span>

1. <span data-ttu-id="30509-175">Drücken Sie F5, um das Projekt im Debugmodus auszuführen.</span><span class="sxs-lookup"><span data-stu-id="30509-175">Press F5 to run the project in debug mode.</span></span>
2. <span data-ttu-id="30509-176">Fügen Sie in der Adresszeile des Browsers **/Home/Chat** an die URL der Standardseite für das Projekt.</span><span class="sxs-lookup"><span data-stu-id="30509-176">In the browser address line, append **/home/chat** to the URL of the default page for the project.</span></span> <span data-ttu-id="30509-177">Chat-Seite lädt in einen Browserinstanz und fordert Sie auf einen Benutzernamen ein.</span><span class="sxs-lookup"><span data-stu-id="30509-177">The Chat page loads in a browser instance and prompts for a user name.</span></span>

    ![Benutzername eingeben](tutorial-getting-started-with-signalr-and-mvc/_static/image8.png)
3. <span data-ttu-id="30509-179">Geben Sie einen Benutzernamen ein.</span><span class="sxs-lookup"><span data-stu-id="30509-179">Enter a user name.</span></span>
4. <span data-ttu-id="30509-180">Kopieren Sie die URL aus der Adresszeile des Browsers ein, und verwenden Sie, um zwei weitere Browserinstanzen öffnen.</span><span class="sxs-lookup"><span data-stu-id="30509-180">Copy the URL from the address line of the browser and use it to open two more browser instances.</span></span> <span data-ttu-id="30509-181">Geben Sie in den einzelnen Instanzen Browser einen eindeutigen Benutzernamen aus.</span><span class="sxs-lookup"><span data-stu-id="30509-181">In each browser instance, enter a unique user name.</span></span>
5. <span data-ttu-id="30509-182">Klicken Sie in den einzelnen Instanzen Browser einen Kommentar hinzufügen, und klicken Sie auf **senden**.</span><span class="sxs-lookup"><span data-stu-id="30509-182">In each browser instance, add a comment and click **Send**.</span></span> <span data-ttu-id="30509-183">Die Kommentare sollten in alle Browserinstanzen angezeigt werden.</span><span class="sxs-lookup"><span data-stu-id="30509-183">The comments should display in all browser instances.</span></span>

    > [!NOTE]
    > <span data-ttu-id="30509-184">Diese einfache chatanwendung behält den Kontext der Diskussion auf dem Server.</span><span class="sxs-lookup"><span data-stu-id="30509-184">This simple chat application does not maintain the discussion context on the server.</span></span> <span data-ttu-id="30509-185">Der Hub, Kommentare, um alle aktuellen Benutzer sendet.</span><span class="sxs-lookup"><span data-stu-id="30509-185">The hub broadcasts comments to all current users.</span></span> <span data-ttu-id="30509-186">Benutzer, die den Chat später beizutreten sehen, dass Nachrichten ab dem Zeitpunkt hinzugefügt werden, dass sie verknüpfen.</span><span class="sxs-lookup"><span data-stu-id="30509-186">Users who join the chat later will see messages added from the time they join.</span></span>
6. <span data-ttu-id="30509-187">Der folgende Screenshot zeigt die chatanwendung, die in einem Browser ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="30509-187">The following screen shot shows the chat application running in a browser.</span></span>

    ![Chat-Browser](tutorial-getting-started-with-signalr-and-mvc/_static/image9.png)
7. <span data-ttu-id="30509-189">In **Projektmappen-Explorer**, überprüfen Sie die **Skriptdokumente** Knoten für die ausgeführte Anwendung.</span><span class="sxs-lookup"><span data-stu-id="30509-189">In **Solution Explorer**, inspect the **Script Documents** node for the running application.</span></span> <span data-ttu-id="30509-190">Dieser Knoten ist sichtbar, im Debugmodus befindet, bei Verwendung von Internet Explorer als Ihren Browser.</span><span class="sxs-lookup"><span data-stu-id="30509-190">This node is visible in debug mode if you are using Internet Explorer as your browser.</span></span> <span data-ttu-id="30509-191">Es wird eine Skriptdatei mit dem Namen **Hubs** , die die SignalR-Bibliothek wird zur Laufzeit dynamisch generiert.</span><span class="sxs-lookup"><span data-stu-id="30509-191">There is a script file named **hubs** that the SignalR library dynamically generates at runtime.</span></span> <span data-ttu-id="30509-192">Diese Datei wird die Kommunikation zwischen jQuery-Skripts und serverseitigen Code verwaltet.</span><span class="sxs-lookup"><span data-stu-id="30509-192">This file manages the communication between jQuery script and server-side code.</span></span> <span data-ttu-id="30509-193">Wenn Sie einen Browser als Internet Explorer verwenden, können Sie auch den dynamischen zugreifen **Hubs** Datei zu suchen und ihn direkt, z. B. http://mywebsite/signalr/hubs.</span><span class="sxs-lookup"><span data-stu-id="30509-193">If you use a browser other than Internet Explorer, you can also access the dynamic **hubs** file by browsing to it directly, for example http://mywebsite/signalr/hubs.</span></span>

<a id="code"></a>

## <a name="examine-the-code"></a><span data-ttu-id="30509-194">Überprüfen Sie den Code</span><span class="sxs-lookup"><span data-stu-id="30509-194">Examine the Code</span></span>

<span data-ttu-id="30509-195">Die SignalR-Chat-Anwendung zeigt zwei grundlegende SignalR Entwicklungsaufgaben: Erstellen eines Hubs wie das Hauptfenster Koordinierung-Objekt, auf dem Server und mithilfe der SignalR-jQuery-Bibliothek zum Senden und Empfangen von Nachrichten.</span><span class="sxs-lookup"><span data-stu-id="30509-195">The SignalR chat application demonstrates two basic SignalR development tasks: creating a hub as the main coordination object on the server, and using the SignalR jQuery library to send and receive messages.</span></span>

### <a name="signalr-hubs"></a><span data-ttu-id="30509-196">SignalR Hubs</span><span class="sxs-lookup"><span data-stu-id="30509-196">SignalR Hubs</span></span>

<span data-ttu-id="30509-197">Im Codebeispiel den **ChatHub** Klasse leitet sich von der **Microsoft.AspNet.SignalR.Hub** Klasse.</span><span class="sxs-lookup"><span data-stu-id="30509-197">In the code sample the **ChatHub** class derives from the **Microsoft.AspNet.SignalR.Hub** class.</span></span> <span data-ttu-id="30509-198">Ableiten von der **Hub** Klasse ist eine hilfreiche Möglichkeit zum Erstellen einer SignalR-Anwendung.</span><span class="sxs-lookup"><span data-stu-id="30509-198">Deriving from the **Hub** class is a useful way to build a SignalR application.</span></span> <span data-ttu-id="30509-199">Sie können öffentliche Methoden in der hubklasse erstellen und dann Zugriff auf diese Methoden durch Aufruf aus Skripts auf einer Webseite.</span><span class="sxs-lookup"><span data-stu-id="30509-199">You can create public methods on your hub class and then access those methods by calling them from scripts in a web page.</span></span>

<span data-ttu-id="30509-200">Rufen Sie in den Code Chat Clients die **ChatHub.Send** Methode, um eine neue Nachricht zu senden.</span><span class="sxs-lookup"><span data-stu-id="30509-200">In the chat code, clients call the **ChatHub.Send** method to send a new message.</span></span> <span data-ttu-id="30509-201">Der Hub sendet wiederum die Nachricht an alle Clients durch den Aufruf **Clients.All.addNewMessageToPage**.</span><span class="sxs-lookup"><span data-stu-id="30509-201">The hub in turn sends the message to all clients by calling **Clients.All.addNewMessageToPage**.</span></span>

<span data-ttu-id="30509-202">Die **senden** einige Konzepte der Hub-Methode veranschaulicht:</span><span class="sxs-lookup"><span data-stu-id="30509-202">The **Send** method demonstrates several hub concepts :</span></span>

- <span data-ttu-id="30509-203">Öffentliche Methoden für einen Hub deklariert werden, damit Clients, die sie aufrufen können.</span><span class="sxs-lookup"><span data-stu-id="30509-203">Declare public methods on a hub so that clients can call them.</span></span>
- <span data-ttu-id="30509-204">Verwenden der **Microsoft.AspNet.SignalR.Hub.Clients** Eigenschaft auf alle Clients, die mit diesem Hub verbunden sind.</span><span class="sxs-lookup"><span data-stu-id="30509-204">Use the **Microsoft.AspNet.SignalR.Hub.Clients** property to access all clients connected to this hub.</span></span>
- <span data-ttu-id="30509-205">Aufrufen einer Funktion auf dem Client (z. B. die `addNewMessageToPage` Funktion) so aktualisieren Sie Clients.</span><span class="sxs-lookup"><span data-stu-id="30509-205">Call a function on the client (such as the `addNewMessageToPage` function) to update clients.</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample5.cs)]

### <a name="signalr-and-jquery"></a><span data-ttu-id="30509-206">SignalR und jQuery</span><span class="sxs-lookup"><span data-stu-id="30509-206">SignalR and jQuery</span></span>

<span data-ttu-id="30509-207">Die **Chat.cshtml** Sichtdatei im Codebeispiel zeigt, wie die SignalR-jQuery-Bibliothek für die Kommunikation mit SignalR-Hubs.</span><span class="sxs-lookup"><span data-stu-id="30509-207">The **Chat.cshtml** view file in the code sample shows how to use the SignalR jQuery library to communicate with a SignalR hub.</span></span> <span data-ttu-id="30509-208">Die Durchführung wichtiger Aufgaben im Code werden einen Verweis auf den automatisch generierten Proxy für den Hub Deklarieren einer Funktion, die der Server auf Push-Inhalte für Clients aufrufen können, und starten eine Verbindung zum Senden von Nachrichten an den Hub erstellt.</span><span class="sxs-lookup"><span data-stu-id="30509-208">The essential tasks in the code are creating a reference to the auto-generated proxy for the hub, declaring a function that the server can call to push content to clients, and starting a connection to send messages to the hub.</span></span>

<span data-ttu-id="30509-209">Der folgende Code deklariert einen Verweis auf einen hubproxy.</span><span class="sxs-lookup"><span data-stu-id="30509-209">The following code declares a reference to a hub proxy.</span></span>

[!code-javascript[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample6.js)]

> [!NOTE]
> <span data-ttu-id="30509-210">In JavaScript wird der Verweis auf die Server-Klasse und ihre Member in Camel-Case.</span><span class="sxs-lookup"><span data-stu-id="30509-210">In JavaScript the reference to the server class and its members is in camel case.</span></span> <span data-ttu-id="30509-211">Im Codebeispiel verweist auf die C#- **ChatHub** Klasse in JavaScript als **ChatHub**.</span><span class="sxs-lookup"><span data-stu-id="30509-211">The code sample references the C# **ChatHub** class in JavaScript as **chatHub**.</span></span> <span data-ttu-id="30509-212">Wenn Sie verweisen möchten die `ChatHub` Klasse in jQuery mit konventionellen Pascal Groß-/Kleinschreibung wie in C#-Klassendatei ChatHub.cs bearbeiten.</span><span class="sxs-lookup"><span data-stu-id="30509-212">If you want to reference the `ChatHub` class in jQuery with conventional Pascal casing as you would in C#, edit the ChatHub.cs class file.</span></span> <span data-ttu-id="30509-213">Hinzufügen einer `using` -Anweisung verweisen die `Microsoft.AspNet.SignalR.Hubs` Namespace.</span><span class="sxs-lookup"><span data-stu-id="30509-213">Add a `using` statement to reference the `Microsoft.AspNet.SignalR.Hubs` namespace.</span></span> <span data-ttu-id="30509-214">Fügen Sie dann die `HubName` -Attribut auf die `ChatHub` Klasse, zum Beispiel `[HubName("ChatHub")]`.</span><span class="sxs-lookup"><span data-stu-id="30509-214">Then add the `HubName` attribute to the `ChatHub` class, for example `[HubName("ChatHub")]`.</span></span> <span data-ttu-id="30509-215">Aktualisieren Sie schließlich den jQuery-Verweis auf die `ChatHub` Klasse.</span><span class="sxs-lookup"><span data-stu-id="30509-215">Finally, update your jQuery reference to the `ChatHub` class.</span></span>


<span data-ttu-id="30509-216">Der folgende Code zeigt, wie eine Rückruffunktion in das Skript erstellt wird.</span><span class="sxs-lookup"><span data-stu-id="30509-216">The following code shows how to create a callback function in the script.</span></span> <span data-ttu-id="30509-217">Die hubklasse, auf dem Server ruft diese Funktion um Inhaltsupdates an jeden Client per Push übertragen.</span><span class="sxs-lookup"><span data-stu-id="30509-217">The hub class on the server calls this function to push content updates to each client.</span></span> <span data-ttu-id="30509-218">Der optionale Aufruf der `htmlEncode` Funktion zeigt eine Möglichkeit, HTML zu codieren den Nachrichteninhalt vor der Anzeige auf der Seite als eine Möglichkeit, Skript-Injection verhindern.</span><span class="sxs-lookup"><span data-stu-id="30509-218">The optional call to the `htmlEncode` function shows a way to HTML encode the message content before displaying it in the page, as a way to prevent script injection.</span></span>

[!code-html[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample7.html)]

<span data-ttu-id="30509-219">Der folgende Code zeigt, wie Sie eine Verbindung mit dem Hub zu öffnen.</span><span class="sxs-lookup"><span data-stu-id="30509-219">The following code shows how to open a connection with the hub.</span></span> <span data-ttu-id="30509-220">Der Code wird die Verbindung gestartet und übergibt diese eine Funktion, um das Click-Ereignis zu behandeln, auf die **senden** auf der Seite Chat.</span><span class="sxs-lookup"><span data-stu-id="30509-220">The code starts the connection and then passes it a function to handle the click event on the **Send** button in the Chat page.</span></span>

> [!NOTE]
> <span data-ttu-id="30509-221">Dieser Ansatz wird sichergestellt, dass die Verbindung hergestellt wird, bevor der Ereignishandler ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="30509-221">This approach ensures that the connection is established before the event handler executes.</span></span>


[!code-javascript[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample8.js)]

<a id="next"></a>

## <a name="next-steps"></a><span data-ttu-id="30509-222">Nächste Schritte</span><span class="sxs-lookup"><span data-stu-id="30509-222">Next Steps</span></span>

<span data-ttu-id="30509-223">Sie erfahren, dass SignalR ein Framework zur Erstellung von Echtzeit-Webanwendungen.</span><span class="sxs-lookup"><span data-stu-id="30509-223">You learned that SignalR is a framework for building real-time web applications.</span></span> <span data-ttu-id="30509-224">Sie haben auch erfahren, mehrere SignalR Entwicklungsaufgaben: zum Hinzufügen von SignalR an eine ASP.NET-Anwendung zum Erstellen einer hubklasse und zum Senden und Empfangen von Nachrichten über den Hub.</span><span class="sxs-lookup"><span data-stu-id="30509-224">You also learned several SignalR development tasks: how to add SignalR to an ASP.NET application, how to create a hub class, and how to send and receive messages from the hub.</span></span>

<span data-ttu-id="30509-225">Eine exemplarische Vorgehensweise, die SignalR-beispielanwendung in Azure bereitzustellen, finden Sie unter [SignalR mit Web-Apps in Azure App Service mithilfe von](../deployment/using-signalr-with-azure-web-sites.md).</span><span class="sxs-lookup"><span data-stu-id="30509-225">For a walkthrough on how to deploy the sample SignalR application to Azure, see [Using SignalR with Web Apps in Azure App Service](../deployment/using-signalr-with-azure-web-sites.md).</span></span> <span data-ttu-id="30509-226">Ausführliche Informationen dazu, wie Sie ein Visual Studio-Webprojekt auf einer Windows Azure-Website bereitstellen, finden Sie unter [erstellen eine ASP.NET Web-app in Azure App Service](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-get-started/).</span><span class="sxs-lookup"><span data-stu-id="30509-226">For detailed information about how to deploy a Visual Studio web project to a Windows Azure Web Site, see [Create an ASP.NET web app in Azure App Service](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-get-started/).</span></span>

<span data-ttu-id="30509-227">Erweiterte Konzepte von SignalR-Entwicklungen finden Sie auf den folgenden Websites für SignalR-Quellcode und Ressourcen:</span><span class="sxs-lookup"><span data-stu-id="30509-227">To learn more advanced SignalR developments concepts, visit the following sites for SignalR source code and resources :</span></span>

- [<span data-ttu-id="30509-228">SignalR-Projekt</span><span class="sxs-lookup"><span data-stu-id="30509-228">SignalR Project</span></span>](http://signalr.net)
- [<span data-ttu-id="30509-229">SignalR Github und Beispiele</span><span class="sxs-lookup"><span data-stu-id="30509-229">SignalR Github and Samples</span></span>](https://github.com/SignalR/SignalR)
- [<span data-ttu-id="30509-230">SignalR Wiki</span><span class="sxs-lookup"><span data-stu-id="30509-230">SignalR Wiki</span></span>](https://github.com/SignalR/SignalR/wiki)
