---
uid: signalr/overview/getting-started/tutorial-getting-started-with-signalr-and-mvc
title: 'Tutorial: Erste Schritte mit SignalR 2 und MVC 5 | Microsoft-Dokumentation'
author: pfletcher
description: Dieses Tutorial veranschaulicht, wie ASP.NET SignalR 2 zu verwenden, um eine chatanwendung mit Echtzeitfunktionalität erstellen. Sie fügen SignalR zu einer MVC 5-Anwendung und eine Chat-Ansicht erstellen...
ms.author: aspnetcontent
ms.date: 06/10/2014
ms.assetid: 80bfe5fb-bdfc-41fe-ac43-2132e5d69fac
msc.legacyurl: /signalr/overview/getting-started/tutorial-getting-started-with-signalr-and-mvc
msc.type: authoredcontent
ms.openlocfilehash: 4a4c013ff047f18f9d9b88595af7951577f3f200
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2018
ms.locfileid: "37838649"
---
<a name="tutorial-getting-started-with-signalr-2-and-mvc-5"></a><span data-ttu-id="75233-104">Tutorial: Erste Schritte mit SignalR 2 und MVC 5</span><span class="sxs-lookup"><span data-stu-id="75233-104">Tutorial: Getting Started with SignalR 2 and MVC 5</span></span>
====================
<span data-ttu-id="75233-105">durch [Patrick Fletcher](https://github.com/pfletcher), [Tim Teebken](https://github.com/timlt)</span><span class="sxs-lookup"><span data-stu-id="75233-105">by [Patrick Fletcher](https://github.com/pfletcher), [Tim Teebken](https://github.com/timlt)</span></span>

[<span data-ttu-id="75233-106">Abgeschlossenes Projekt herunterladen</span><span class="sxs-lookup"><span data-stu-id="75233-106">Download Completed Project</span></span>](http://code.msdn.microsoft.com/Getting-Started-with-c366b2f3)

> <span data-ttu-id="75233-107">Dieses Tutorial veranschaulicht, wie ASP.NET SignalR 2 zu verwenden, um eine chatanwendung mit Echtzeitfunktionalität erstellen.</span><span class="sxs-lookup"><span data-stu-id="75233-107">This tutorial shows how to use ASP.NET SignalR 2 to create a real-time chat application.</span></span> <span data-ttu-id="75233-108">Sie SignalR zu einer MVC 5-Anwendung hinzufügen und erstellen Sie eine Chat-Ansicht zum Senden und Meldungen angezeigt werden.</span><span class="sxs-lookup"><span data-stu-id="75233-108">You will add SignalR to an MVC 5 application and create a chat view to send and display messages.</span></span> 
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="75233-109">Softwareversionen, die in diesem Tutorial verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="75233-109">Software versions used in the tutorial</span></span>
> 
> 
> - [<span data-ttu-id="75233-110">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="75233-110">Visual Studio 2013</span></span>](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - <span data-ttu-id="75233-111">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="75233-111">.NET 4.5</span></span>
> - <span data-ttu-id="75233-112">MVC 5</span><span class="sxs-lookup"><span data-stu-id="75233-112">MVC 5</span></span>
> - <span data-ttu-id="75233-113">SignalR-Version 2</span><span class="sxs-lookup"><span data-stu-id="75233-113">SignalR version 2</span></span>
>   
> 
> 
> ## <a name="using-visual-studio-2012-with-this-tutorial"></a><span data-ttu-id="75233-114">In diesem Tutorial mithilfe von Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="75233-114">Using Visual Studio 2012 with this tutorial</span></span>
> 
> 
> <span data-ttu-id="75233-115">Um Visual Studio 2012 mit diesem Tutorial verwenden möchten, führen Sie folgende Schritte aus:</span><span class="sxs-lookup"><span data-stu-id="75233-115">To use Visual Studio 2012 with this tutorial, do the following:</span></span>
> 
> - <span data-ttu-id="75233-116">Aktualisieren Ihrer [-Paket-Manager](http://docs.nuget.org/docs/start-here/installing-nuget) auf die neueste Version.</span><span class="sxs-lookup"><span data-stu-id="75233-116">Update your [Package Manager](http://docs.nuget.org/docs/start-here/installing-nuget) to the latest version.</span></span>
> - <span data-ttu-id="75233-117">Installieren Sie die [Webplattform-Installer](https://www.microsoft.com/web/downloads/platform.aspx).</span><span class="sxs-lookup"><span data-stu-id="75233-117">Install the [Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx).</span></span>
> - <span data-ttu-id="75233-118">Suchen Sie in den Webplattform-Installer, und installieren Sie **ASP.NET und Web Tools 2013.1 für Visual Studio 2012**.</span><span class="sxs-lookup"><span data-stu-id="75233-118">In the Web Platform Installer, search for and install **ASP.NET and Web Tools 2013.1 for Visual Studio 2012**.</span></span> <span data-ttu-id="75233-119">Dadurch wird Visual Studio-Vorlagen für SignalR-Klassen wie z. B. installiert **Hub**.</span><span class="sxs-lookup"><span data-stu-id="75233-119">This will install Visual Studio templates for SignalR classes such as **Hub**.</span></span>
> - <span data-ttu-id="75233-120">Einige Vorlagen (z. B. **OWIN-Startklasse**) nicht zur Verfügung; verwenden Sie für diese stattdessen eine Klassendatei.</span><span class="sxs-lookup"><span data-stu-id="75233-120">Some templates (such as **OWIN Startup Class**) will not be available; for these, use a Class file instead.</span></span>
> 
> 
> ## <a name="tutorial-versions"></a><span data-ttu-id="75233-121">Lernprogramm-Versionen</span><span class="sxs-lookup"><span data-stu-id="75233-121">Tutorial Versions</span></span>
> 
> <span data-ttu-id="75233-122">Weitere Informationen zu früheren Versionen von SignalR, finden Sie unter [ältere Versionen von SignalR](../older-versions/index.md).</span><span class="sxs-lookup"><span data-stu-id="75233-122">For information about earlier versions of SignalR, see [SignalR Older Versions](../older-versions/index.md).</span></span>
> 
> ## <a name="questions-and-comments"></a><span data-ttu-id="75233-123">Fragen und Kommentare</span><span class="sxs-lookup"><span data-stu-id="75233-123">Questions and comments</span></span>
> 
> <span data-ttu-id="75233-124">Lassen Sie Feedback, auf wie Ihnen in diesem Tutorial gefallen hat und was wir in den Kommentaren am unteren Rand der Seite verbessern können.</span><span class="sxs-lookup"><span data-stu-id="75233-124">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="75233-125">Wenn Sie Fragen, die nicht direkt mit dem Tutorial verknüpft sind haben, können Sie sie veröffentlichen das [ASP.NET SignalR-Forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) oder [StackOverflow.com](http://stackoverflow.com/).</span><span class="sxs-lookup"><span data-stu-id="75233-125">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>


## <a name="overview"></a><span data-ttu-id="75233-126">Übersicht</span><span class="sxs-lookup"><span data-stu-id="75233-126">Overview</span></span>

<span data-ttu-id="75233-127">Dieses Tutorial führt Sie in Echtzeit-Webanwendungsentwicklung mit ASP.NET SignalR 2 und ASP.NET MVC 5.</span><span class="sxs-lookup"><span data-stu-id="75233-127">This tutorial introduces you to real-time web application development with ASP.NET SignalR 2 and ASP.NET MVC 5.</span></span> <span data-ttu-id="75233-128">Das Lernprogramm verwendet den gleichen Code der Chat-Anwendung wie den [SignalR, erste Schritte-Lernprogramm](tutorial-getting-started-with-signalr.md), aber zeigt, wie sie eine MVC 5-Anwendung hinzuzufügen.</span><span class="sxs-lookup"><span data-stu-id="75233-128">The tutorial uses the same chat application code as the [SignalR Getting Started tutorial](tutorial-getting-started-with-signalr.md), but shows how to add it to an MVC 5 application.</span></span>

<span data-ttu-id="75233-129">In diesem Thema lernen Sie die folgenden Entwicklungsaufgaben von SignalR:</span><span class="sxs-lookup"><span data-stu-id="75233-129">In this topic you will learn the following SignalR development tasks:</span></span>

- <span data-ttu-id="75233-130">Die SignalR-Bibliothek hinzufügen zu einer MVC 5-Anwendung.</span><span class="sxs-lookup"><span data-stu-id="75233-130">Adding the SignalR library to an MVC 5 application.</span></span>
- <span data-ttu-id="75233-131">Erstellen von Hub und OWIN-Startup-Klassen, die Inhalt an Clients zu verteilen.</span><span class="sxs-lookup"><span data-stu-id="75233-131">Creating hub and OWIN startup classes to push content to clients.</span></span>
- <span data-ttu-id="75233-132">Verwenden die SignalR-jQuery-Bibliothek auf einer Webseite zum Senden von Nachrichten und Updates vom Hub angezeigt.</span><span class="sxs-lookup"><span data-stu-id="75233-132">Using the SignalR jQuery library in a web page to send messages and display updates from the hub.</span></span>

<span data-ttu-id="75233-133">Der folgende Screenshot zeigt die abgeschlossene Chat-Anwendung, die in einem Browser ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="75233-133">The following screen shot shows the completed chat application running in a browser.</span></span>

![Chatinstanzen](tutorial-getting-started-with-signalr-and-mvc/_static/image1.png)

<span data-ttu-id="75233-135">Abschnitte:</span><span class="sxs-lookup"><span data-stu-id="75233-135">Sections:</span></span>

- [<span data-ttu-id="75233-136">Einrichten des Projekts</span><span class="sxs-lookup"><span data-stu-id="75233-136">Set up the Project</span></span>](#setup)
- [<span data-ttu-id="75233-137">Ausführen des Beispiels</span><span class="sxs-lookup"><span data-stu-id="75233-137">Run the Sample</span></span>](#run)
- [<span data-ttu-id="75233-138">Überprüfen Sie den Code</span><span class="sxs-lookup"><span data-stu-id="75233-138">Examine the Code</span></span>](#code)
- [<span data-ttu-id="75233-139">Nächste Schritte</span><span class="sxs-lookup"><span data-stu-id="75233-139">Next Steps</span></span>](#next)

<a id="setup"></a>

## <a name="set-up-the-project"></a><span data-ttu-id="75233-140">Einrichten des Projekts</span><span class="sxs-lookup"><span data-stu-id="75233-140">Set up the Project</span></span>

<span data-ttu-id="75233-141">Erforderliche Komponenten:</span><span class="sxs-lookup"><span data-stu-id="75233-141">Prerequisites:</span></span>

- <span data-ttu-id="75233-142">Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="75233-142">Visual Studio 2013.</span></span> <span data-ttu-id="75233-143">Wenn Sie nicht über Visual Studio verfügen, finden Sie unter [ASP.NET-Downloads](https://www.asp.net/downloads) um die kostenlose Visual Studio 2013 Express Entwicklungstool zu erhalten.</span><span class="sxs-lookup"><span data-stu-id="75233-143">If you do not have Visual Studio, see [ASP.NET Downloads](https://www.asp.net/downloads) to get the free Visual Studio 2013 Express Development Tool.</span></span>

<span data-ttu-id="75233-144">In diesem Abschnitt veranschaulicht das Erstellen einer ASP.NET MVC 5-Anwendung, die SignalR-Bibliothek hinzufügen und erstellen Sie die chatanwendung.</span><span class="sxs-lookup"><span data-stu-id="75233-144">This section shows how to create an ASP.NET MVC 5 application, add the SignalR library, and create the chat application.</span></span>

1. <span data-ttu-id="75233-145">In Visual Studio Erstellen einer c# ASP.NET-Anwendung, die auf .NET Framework 4.5 abzielt, nennen Sie sie SignalRChat, und klicken Sie auf OK.</span><span class="sxs-lookup"><span data-stu-id="75233-145">In Visual Studio, create a C# ASP.NET application that targets .NET Framework 4.5, name it SignalRChat, and click OK.</span></span>

    ![Erstellen Sie web](tutorial-getting-started-with-signalr-and-mvc/_static/image2.png)
2. <span data-ttu-id="75233-147">In der `New ASP.NET Project` Dialogfeld ein, und wählen Sie **MVC**, und klicken Sie auf **Authentifizierung ändern**.</span><span class="sxs-lookup"><span data-stu-id="75233-147">In the `New ASP.NET Project` dialog, and select **MVC**, and click **Change Authentication**.</span></span>

    ![Erstellen Sie web](tutorial-getting-started-with-signalr-and-mvc/_static/image3.png)
3. <span data-ttu-id="75233-149">Wählen Sie **keine Authentifizierung** in die **Authentifizierung ändern** Dialogfeld ein, und klicken Sie auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="75233-149">Select **No Authentication** in the **Change Authentication** dialog, and click **OK**.</span></span>

    ![Wählen Sie keine Authentifizierung](tutorial-getting-started-with-signalr-and-mvc/_static/image4.png)

    > [!NOTE]
    > <span data-ttu-id="75233-151">Wenn Sie einen anderen Authentifizierungsanbieter für Ihre Anwendung auswählen einer `Startup.cs` Klasse wird für Sie erstellt werden; Sie müssen nicht zum Erstellen eines eigenen `Startup.cs` Klasse, die in Schritt 10 unten.</span><span class="sxs-lookup"><span data-stu-id="75233-151">If you select a different authentication provider for your application, a `Startup.cs` class will be created for you; you will not need to create your own `Startup.cs` class in step 10 below.</span></span>
4. <span data-ttu-id="75233-152">Klicken Sie auf **OK** in die **neues ASP.NET-Projekt** Dialogfeld.</span><span class="sxs-lookup"><span data-stu-id="75233-152">Click **OK** in the **New ASP.NET Project** dialog.</span></span>
5. <span data-ttu-id="75233-153">Öffnen der **Tools | Bibliothekspaket-Manager | Paket-Manager-Konsole** , und führen Sie den folgenden Befehl aus.</span><span class="sxs-lookup"><span data-stu-id="75233-153">Open the **Tools | Library Package Manager | Package Manager Console** and run the following command.</span></span> <span data-ttu-id="75233-154">Dieser Schritt fügt dem Projekt hinzu einen Satz von Skriptdateien und Assemblyverweise, die SignalR-Funktionalität zu aktivieren.</span><span class="sxs-lookup"><span data-stu-id="75233-154">This step adds to the project a set of script files and assembly references that enable SignalR functionality.</span></span>

    `install-package Microsoft.AspNet.SignalR`
6. <span data-ttu-id="75233-155">In **Projektmappen-Explorer**, erweitern Sie den Ordner "Scripts".</span><span class="sxs-lookup"><span data-stu-id="75233-155">In **Solution Explorer**, expand the Scripts folder.</span></span> <span data-ttu-id="75233-156">Beachten Sie, dass die Skriptbibliotheken für SignalR zum Projekt hinzugefügt wurden.</span><span class="sxs-lookup"><span data-stu-id="75233-156">Note that script libraries for SignalR have been added to the project.</span></span>

    ![Ordner "Scripts"](tutorial-getting-started-with-signalr-and-mvc/_static/image5.png)
7. <span data-ttu-id="75233-158">In **Projektmappen-Explorer**mit der rechten Maustaste auf das Projekt, wählen Sie **hinzufügen | Neuer Ordner**, und fügen Sie einen neuen Ordner namens **Hubs**.</span><span class="sxs-lookup"><span data-stu-id="75233-158">In **Solution Explorer**, right-click the project, select **Add | New Folder**, and add a new folder named **Hubs**.</span></span>
8. <span data-ttu-id="75233-159">Mit der rechten Maustaste die **Hubs** Ordner, klicken Sie auf **hinzufügen | Neues Element**, wählen die **Visual c# | Web | SignalR** Knoten in der **installiert** wählen Sie im Bereich **SignalR-Hubklasse (v2)** im mittleren Bereich, und erstellen Sie einen neuen Hub, mit dem Namen **ChatHub.cs**.</span><span class="sxs-lookup"><span data-stu-id="75233-159">Right-click the **Hubs** folder, click **Add | New Item**, select the **Visual C# | Web | SignalR** node in the **Installed** pane, select **SignalR Hub Class (v2)** from the center pane, and create a new hub named **ChatHub.cs**.</span></span> <span data-ttu-id="75233-160">Sie verwenden diese Klasse als einen SignalR-Hub, die Nachrichten an alle Clients sendet.</span><span class="sxs-lookup"><span data-stu-id="75233-160">You will use this class as a SignalR server hub that sends messages to all clients.</span></span>

    ![Erstellen von neuen hub](tutorial-getting-started-with-signalr-and-mvc/_static/image6.png)
9. <span data-ttu-id="75233-162">Ersetzen Sie den Code in die **ChatHub** Klasse durch den folgenden Code.</span><span class="sxs-lookup"><span data-stu-id="75233-162">Replace the code in the **ChatHub** class with the following code.</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample1.cs)]
10. <span data-ttu-id="75233-163">Erstellen Sie eine neue Klasse namens "Startup.cs".</span><span class="sxs-lookup"><span data-stu-id="75233-163">Create a new class called Startup.cs.</span></span> <span data-ttu-id="75233-164">Ändern Sie den Inhalt der Datei die folgende aus.</span><span class="sxs-lookup"><span data-stu-id="75233-164">Change the contents of the file to the following.</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample2.cs)]
11. <span data-ttu-id="75233-165">Bearbeiten der `HomeController` Klasse finden Sie im **Controllers/HomeController.cs** und fügen Sie die folgende Methode der Klasse.</span><span class="sxs-lookup"><span data-stu-id="75233-165">Edit the `HomeController` class found in **Controllers/HomeController.cs** and add the following method to the class.</span></span> <span data-ttu-id="75233-166">Diese Methode gibt die **Chat** anzeigen, die Sie in einem späteren Schritt erstellen.</span><span class="sxs-lookup"><span data-stu-id="75233-166">This method returns the **Chat** view that you will create in a later step.</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample3.cs)]
12. <span data-ttu-id="75233-167">Mit der rechten Maustaste die **Views/Home** Ordner, und wählen **hinzufügen.... | Ansicht**.</span><span class="sxs-lookup"><span data-stu-id="75233-167">Right-click the **Views/Home** folder, and select **Add... | View**.</span></span>
13. <span data-ttu-id="75233-168">In der **Ansicht hinzufügen** Dialogfeld Namen der neuen Sicht **Chat**.</span><span class="sxs-lookup"><span data-stu-id="75233-168">In the **Add View** dialog, name the new view **Chat**.</span></span>

    ![Hinzufügen einer Ansicht](tutorial-getting-started-with-signalr-and-mvc/_static/image7.png)
14. <span data-ttu-id="75233-170">Ersetzen Sie den Inhalt der **Chat.cshtml** durch den folgenden Code.</span><span class="sxs-lookup"><span data-stu-id="75233-170">Replace the contents of **Chat.cshtml** with the following code.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="75233-171">Wenn Sie SignalR und anderen JavaScript-Bibliotheken zu Ihrem Visual Studio-Projekt hinzufügen, kann der Paket-Manager eine Version der SignalR-Skriptdatei installieren, als die Version, die in diesem Thema gezeigten ist.</span><span class="sxs-lookup"><span data-stu-id="75233-171">When you add SignalR and other script libraries to your Visual Studio project, the Package Manager might install a version of the SignalR script file that is more recent than the version shown in this topic.</span></span> <span data-ttu-id="75233-172">Stellen Sie sicher, dass der Verweis auf das Skript in Ihrem Code mit der Version der Skriptbibliothek in Ihrem Projekt installiert übereinstimmt.</span><span class="sxs-lookup"><span data-stu-id="75233-172">Make sure that the script reference in your code matches the version of the script library installed in your project.</span></span>

    [!code-cshtml[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample4.cshtml)]
15. <span data-ttu-id="75233-173">**Speichern Sie alle** für das Projekt.</span><span class="sxs-lookup"><span data-stu-id="75233-173">**Save All** for the project.</span></span>

<a id="run"></a>

## <a name="run-the-sample"></a><span data-ttu-id="75233-174">Ausführen des Beispiels</span><span class="sxs-lookup"><span data-stu-id="75233-174">Run the Sample</span></span>

1. <span data-ttu-id="75233-175">Drücken Sie F5, um das Projekt im Debugmodus auszuführen.</span><span class="sxs-lookup"><span data-stu-id="75233-175">Press F5 to run the project in debug mode.</span></span>
2. <span data-ttu-id="75233-176">Fügen Sie in der Adresszeile des Browsers **/Home/Chat** an die URL der Standardseite für das Projekt.</span><span class="sxs-lookup"><span data-stu-id="75233-176">In the browser address line, append **/home/chat** to the URL of the default page for the project.</span></span> <span data-ttu-id="75233-177">Die Chat-Seite lädt in eine Instanz eines Webbrowsers und aufforderungen für einen Benutzernamen ein.</span><span class="sxs-lookup"><span data-stu-id="75233-177">The Chat page loads in a browser instance and prompts for a user name.</span></span>

    ![Benutzername eingeben](tutorial-getting-started-with-signalr-and-mvc/_static/image8.png)
3. <span data-ttu-id="75233-179">Geben Sie einen Benutzernamen ein.</span><span class="sxs-lookup"><span data-stu-id="75233-179">Enter a user name.</span></span>
4. <span data-ttu-id="75233-180">Kopieren Sie die URL aus der Adresszeile des Browsers, und öffnen Sie zwei Browserinstanzen für weitere.</span><span class="sxs-lookup"><span data-stu-id="75233-180">Copy the URL from the address line of the browser and use it to open two more browser instances.</span></span> <span data-ttu-id="75233-181">Geben Sie in den einzelnen Instanzen Browser einen eindeutigen Benutzernamen ein.</span><span class="sxs-lookup"><span data-stu-id="75233-181">In each browser instance, enter a unique user name.</span></span>
5. <span data-ttu-id="75233-182">Klicken Sie in den einzelnen Instanzen Browser Fügt einen Kommentar hinzu, und klicken Sie auf **senden**.</span><span class="sxs-lookup"><span data-stu-id="75233-182">In each browser instance, add a comment and click **Send**.</span></span> <span data-ttu-id="75233-183">Die Kommentare sollte in alle Browserinstanzen angezeigt werden.</span><span class="sxs-lookup"><span data-stu-id="75233-183">The comments should display in all browser instances.</span></span>

    > [!NOTE]
    > <span data-ttu-id="75233-184">Diese einfache chatanwendung werden den Kontext der Diskussion auf dem Server nicht verwaltet werden.</span><span class="sxs-lookup"><span data-stu-id="75233-184">This simple chat application does not maintain the discussion context on the server.</span></span> <span data-ttu-id="75233-185">Der Hub sendet, Kommentare, um alle aktuellen Benutzer.</span><span class="sxs-lookup"><span data-stu-id="75233-185">The hub broadcasts comments to all current users.</span></span> <span data-ttu-id="75233-186">Benutzer, die im Chat später zusammenfügen sehen, dass Nachrichten ab dem Zeitpunkt hinzugefügt werden, dass sie aufgenommen werden.</span><span class="sxs-lookup"><span data-stu-id="75233-186">Users who join the chat later will see messages added from the time they join.</span></span>
6. <span data-ttu-id="75233-187">Der folgende Screenshot zeigt die chatanwendung, die in einem Browser ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="75233-187">The following screen shot shows the chat application running in a browser.</span></span>

    ![Chat-Browser](tutorial-getting-started-with-signalr-and-mvc/_static/image9.png)
7. <span data-ttu-id="75233-189">In **Projektmappen-Explorer**, überprüfen Sie die **Skriptdokumente** Knoten für die ausgeführte Anwendung.</span><span class="sxs-lookup"><span data-stu-id="75233-189">In **Solution Explorer**, inspect the **Script Documents** node for the running application.</span></span> <span data-ttu-id="75233-190">Dieser Knoten ist sichtbar im Debugmodus, wenn Sie Internet Explorer als Browser verwenden.</span><span class="sxs-lookup"><span data-stu-id="75233-190">This node is visible in debug mode if you are using Internet Explorer as your browser.</span></span> <span data-ttu-id="75233-191">Es gibt eine Skriptdatei namens **Hubs** , die den SignalR-Bibliothek wird zur Laufzeit dynamisch generiert.</span><span class="sxs-lookup"><span data-stu-id="75233-191">There is a script file named **hubs** that the SignalR library dynamically generates at runtime.</span></span> <span data-ttu-id="75233-192">Diese Datei, verwaltet die Kommunikation zwischen der jQuery-Skripts und serverseitigen Code.</span><span class="sxs-lookup"><span data-stu-id="75233-192">This file manages the communication between jQuery script and server-side code.</span></span> <span data-ttu-id="75233-193">Wenn Sie einen Browser als Internet Explorer verwenden, können Sie auch zugreifen, auf den dynamischen **Hubs** Datei navigieren darin direkt, beispielsweise http://mywebsite/signalr/hubs.</span><span class="sxs-lookup"><span data-stu-id="75233-193">If you use a browser other than Internet Explorer, you can also access the dynamic **hubs** file by browsing to it directly, for example http://mywebsite/signalr/hubs.</span></span>

<a id="code"></a>

## <a name="examine-the-code"></a><span data-ttu-id="75233-194">Überprüfen Sie den Code</span><span class="sxs-lookup"><span data-stu-id="75233-194">Examine the Code</span></span>

<span data-ttu-id="75233-195">Der SignalR Chat-Anwendung zeigt zwei grundlegende Aufgaben für die SignalR-Entwicklung: Erstellen eines Hubs wie das wichtigste Coordination-Objekt, auf dem Server, und mithilfe der SignalR-jQuery-Bibliothek zum Senden und Empfangen von Nachrichten.</span><span class="sxs-lookup"><span data-stu-id="75233-195">The SignalR chat application demonstrates two basic SignalR development tasks: creating a hub as the main coordination object on the server, and using the SignalR jQuery library to send and receive messages.</span></span>

### <a name="signalr-hubs"></a><span data-ttu-id="75233-196">SignalR-Hubs</span><span class="sxs-lookup"><span data-stu-id="75233-196">SignalR Hubs</span></span>

<span data-ttu-id="75233-197">Im Codebeispiel enthält die **ChatHub** Klasse leitet sich von der **Microsoft.AspNet.SignalR.Hub** Klasse.</span><span class="sxs-lookup"><span data-stu-id="75233-197">In the code sample the **ChatHub** class derives from the **Microsoft.AspNet.SignalR.Hub** class.</span></span> <span data-ttu-id="75233-198">Ableiten von der **Hub** Klasse ist eine gute Möglichkeit, eine SignalR-Anwendung zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="75233-198">Deriving from the **Hub** class is a useful way to build a SignalR application.</span></span> <span data-ttu-id="75233-199">Sie können öffentliche Methoden auf Ihrem Hub-Klasse erstellen und dann Zugriff auf diese Methoden durch einen Aufruf über Skripts auf einer Webseite.</span><span class="sxs-lookup"><span data-stu-id="75233-199">You can create public methods on your hub class and then access those methods by calling them from scripts in a web page.</span></span>

<span data-ttu-id="75233-200">Im Code Chat Clients rufen die **ChatHub.Send** Methode, um eine neue Nachricht zu senden.</span><span class="sxs-lookup"><span data-stu-id="75233-200">In the chat code, clients call the **ChatHub.Send** method to send a new message.</span></span> <span data-ttu-id="75233-201">Der Hub wiederum sendet die Nachricht an alle Clients durch den Aufruf **Clients.All.addNewMessageToPage**.</span><span class="sxs-lookup"><span data-stu-id="75233-201">The hub in turn sends the message to all clients by calling **Clients.All.addNewMessageToPage**.</span></span>

<span data-ttu-id="75233-202">Die **senden** -Methode demonstriert, mehrere Hub-Konzepte:</span><span class="sxs-lookup"><span data-stu-id="75233-202">The **Send** method demonstrates several hub concepts :</span></span>

- <span data-ttu-id="75233-203">Deklarieren Sie öffentliche Methoden für einen Hub, damit Clients, die sie aufrufen können.</span><span class="sxs-lookup"><span data-stu-id="75233-203">Declare public methods on a hub so that clients can call them.</span></span>
- <span data-ttu-id="75233-204">Verwenden der **Microsoft.AspNet.SignalR.Hub.Clients** Eigenschaft, um alle Clients zugreifen, die mit diesem Hub verbunden sind.</span><span class="sxs-lookup"><span data-stu-id="75233-204">Use the **Microsoft.AspNet.SignalR.Hub.Clients** property to access all clients connected to this hub.</span></span>
- <span data-ttu-id="75233-205">Aufrufen einer Funktion auf dem Client (z. B. die `addNewMessageToPage` Funktion) zum Aktualisieren von Clients.</span><span class="sxs-lookup"><span data-stu-id="75233-205">Call a function on the client (such as the `addNewMessageToPage` function) to update clients.</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample5.cs)]

### <a name="signalr-and-jquery"></a><span data-ttu-id="75233-206">SignalR und jQuery</span><span class="sxs-lookup"><span data-stu-id="75233-206">SignalR and jQuery</span></span>

<span data-ttu-id="75233-207">Die **Chat.cshtml** Ansicht im Codebeispiel zeigt, wie die SignalR-jQuery-Bibliothek, die zur Kommunikation mit einer SignalR-Hub verwenden.</span><span class="sxs-lookup"><span data-stu-id="75233-207">The **Chat.cshtml** view file in the code sample shows how to use the SignalR jQuery library to communicate with a SignalR hub.</span></span> <span data-ttu-id="75233-208">Die wesentlichen Aufgaben in der Code erstellen einen Verweis auf den automatisch generierten Proxy für den Hub, Deklarieren einer Funktion, die Inhalt per Pushvorgang an Clients der Server aufrufen können, und starten eine Verbindung zum Senden von Nachrichten an den Hub.</span><span class="sxs-lookup"><span data-stu-id="75233-208">The essential tasks in the code are creating a reference to the auto-generated proxy for the hub, declaring a function that the server can call to push content to clients, and starting a connection to send messages to the hub.</span></span>

<span data-ttu-id="75233-209">Der folgende Code deklariert einen Verweis auf eine hubproxy-Klasse.</span><span class="sxs-lookup"><span data-stu-id="75233-209">The following code declares a reference to a hub proxy.</span></span>

[!code-javascript[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample6.js)]

> [!NOTE]
> <span data-ttu-id="75233-210">In JavaScript wird der Verweis auf die Server-Klasse und ihre Member in Camel-Case.</span><span class="sxs-lookup"><span data-stu-id="75233-210">In JavaScript the reference to the server class and its members is in camel case.</span></span> <span data-ttu-id="75233-211">Das Codebeispiel verweist auf die C#- **ChatHub** Klasse in JavaScript als **ChatHub**.</span><span class="sxs-lookup"><span data-stu-id="75233-211">The code sample references the C# **ChatHub** class in JavaScript as **chatHub**.</span></span> <span data-ttu-id="75233-212">Wenn Sie verweisen möchten die `ChatHub` Klasse in jQuery mit herkömmlichen Pascal Groß-/Kleinschreibung wie in c#-Datei der ChatHub.cs bearbeiten.</span><span class="sxs-lookup"><span data-stu-id="75233-212">If you want to reference the `ChatHub` class in jQuery with conventional Pascal casing as you would in C#, edit the ChatHub.cs class file.</span></span> <span data-ttu-id="75233-213">Hinzufügen einer `using` Anweisung auf die `Microsoft.AspNet.SignalR.Hubs` Namespace.</span><span class="sxs-lookup"><span data-stu-id="75233-213">Add a `using` statement to reference the `Microsoft.AspNet.SignalR.Hubs` namespace.</span></span> <span data-ttu-id="75233-214">Fügen Sie dann die `HubName` -Attribut auf die `ChatHub` Klasse, zum Beispiel `[HubName("ChatHub")]`.</span><span class="sxs-lookup"><span data-stu-id="75233-214">Then add the `HubName` attribute to the `ChatHub` class, for example `[HubName("ChatHub")]`.</span></span> <span data-ttu-id="75233-215">Zum Schluss aktualisieren Sie den jQuery-Verweis auf die `ChatHub` Klasse.</span><span class="sxs-lookup"><span data-stu-id="75233-215">Finally, update your jQuery reference to the `ChatHub` class.</span></span>


<span data-ttu-id="75233-216">Der folgende Code zeigt, wie Sie eine Callback-Funktion im Skript zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="75233-216">The following code shows how to create a callback function in the script.</span></span> <span data-ttu-id="75233-217">Die hubklasse auf dem Server ruft diese Funktion, um Inhaltsupdates per Pushvorgang an jeden Client übertragen.</span><span class="sxs-lookup"><span data-stu-id="75233-217">The hub class on the server calls this function to push content updates to each client.</span></span> <span data-ttu-id="75233-218">Der Aufruf optional die `htmlEncode` Funktion zeigt die Möglichkeit, HTML codieren von Inhalt der Nachricht vor der Anzeige auf der Seite als eine Möglichkeit zum Skript-Injection verhindern.</span><span class="sxs-lookup"><span data-stu-id="75233-218">The optional call to the `htmlEncode` function shows a way to HTML encode the message content before displaying it in the page, as a way to prevent script injection.</span></span>

[!code-html[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample7.html)]

<span data-ttu-id="75233-219">Der folgende Code zeigt, wie Sie eine Verbindung mit dem Hub zu öffnen.</span><span class="sxs-lookup"><span data-stu-id="75233-219">The following code shows how to open a connection with the hub.</span></span> <span data-ttu-id="75233-220">Der Code wird die Verbindung gestartet und leitet diese dann in eine Funktion zum Behandeln der Click-Ereignis auf der **senden** auf der Seite Chat.</span><span class="sxs-lookup"><span data-stu-id="75233-220">The code starts the connection and then passes it a function to handle the click event on the **Send** button in the Chat page.</span></span>

> [!NOTE]
> <span data-ttu-id="75233-221">Dieser Ansatz wird sichergestellt, dass die Verbindung hergestellt wurde, bevor der Ereignishandler ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="75233-221">This approach ensures that the connection is established before the event handler executes.</span></span>


[!code-javascript[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample8.js)]

<a id="next"></a>

## <a name="next-steps"></a><span data-ttu-id="75233-222">Nächste Schritte</span><span class="sxs-lookup"><span data-stu-id="75233-222">Next Steps</span></span>

<span data-ttu-id="75233-223">Sie haben gelernt, dass es sich bei SignalR handelt es sich ein Framework zum Erstellen von Echtzeit-Webanwendungen.</span><span class="sxs-lookup"><span data-stu-id="75233-223">You learned that SignalR is a framework for building real-time web applications.</span></span> <span data-ttu-id="75233-224">Sie erfahren auch mehrere Aufgaben bei der Entwicklung von SignalR: wie eine ASP.NET-Anwendung SignalR hinzugefügt, wie zum Erstellen einer hubklasse und das Senden und Empfangen von Nachrichten aus dem Hub.</span><span class="sxs-lookup"><span data-stu-id="75233-224">You also learned several SignalR development tasks: how to add SignalR to an ASP.NET application, how to create a hub class, and how to send and receive messages from the hub.</span></span>

<span data-ttu-id="75233-225">Eine exemplarische Vorgehensweise, um die SignalR-beispielanwendung in Azure bereitstellen, finden Sie unter [mithilfe von SignalR mit Web-Apps in Azure App Service](../deployment/using-signalr-with-azure-web-sites.md).</span><span class="sxs-lookup"><span data-stu-id="75233-225">For a walkthrough on how to deploy the sample SignalR application to Azure, see [Using SignalR with Web Apps in Azure App Service](../deployment/using-signalr-with-azure-web-sites.md).</span></span> <span data-ttu-id="75233-226">Ausführliche Informationen dazu, wie Sie ein Visual Studio-Webprojekt auf einer Windows Azure-Website bereitstellen, finden Sie unter [ASP.NET Web-app in Azure App Service erstellen](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-get-started/).</span><span class="sxs-lookup"><span data-stu-id="75233-226">For detailed information about how to deploy a Visual Studio web project to a Windows Azure Web Site, see [Create an ASP.NET web app in Azure App Service](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-get-started/).</span></span>

<span data-ttu-id="75233-227">Erweiterte Konzepte der SignalR-Entwicklungen finden Sie unter den folgenden Websites für SignalR-Quellcode und Ressourcen:</span><span class="sxs-lookup"><span data-stu-id="75233-227">To learn more advanced SignalR developments concepts, visit the following sites for SignalR source code and resources :</span></span>

- [<span data-ttu-id="75233-228">SignalR-Projekt</span><span class="sxs-lookup"><span data-stu-id="75233-228">SignalR Project</span></span>](http://signalr.net)
- [<span data-ttu-id="75233-229">SignalR Github und Beispiele</span><span class="sxs-lookup"><span data-stu-id="75233-229">SignalR Github and Samples</span></span>](https://github.com/SignalR/SignalR)
- [<span data-ttu-id="75233-230">SignalR-Wiki</span><span class="sxs-lookup"><span data-stu-id="75233-230">SignalR Wiki</span></span>](https://github.com/SignalR/SignalR/wiki)
