---
uid: signalr/overview/getting-started/tutorial-getting-started-with-signalr
title: 'Lernprogramm: Erste Schritte mit SignalR 2 | Microsoft Docs'
author: pfletcher
description: "Dieses Tutorial zeigt, wie Sie mithilfe von SignalR eine Chatanwendung mit Echtzeitfunktionalität erstellen. Sie werden eine leere ASP.NET-Webanwendung SignalR hinzu, und erstellen eine HTML-Pa..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2014
ms.topic: article
ms.assetid: a8b3b778-f009-4369-85c7-e90f9878d8b4
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/getting-started/tutorial-getting-started-with-signalr
msc.type: authoredcontent
ms.openlocfilehash: 8be851f5a2b1cca39f5f8f284ff1c002c486d7e8
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/24/2018
---
<a name="tutorial-getting-started-with-signalr-2"></a><span data-ttu-id="646d9-104">Lernprogramm: Erste Schritte mit SignalR 2</span><span class="sxs-lookup"><span data-stu-id="646d9-104">Tutorial: Getting Started with SignalR 2</span></span>
====================
<span data-ttu-id="646d9-105">durch [Patrick Fletcher](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="646d9-105">by [Patrick Fletcher](https://github.com/pfletcher)</span></span>

[<span data-ttu-id="646d9-106">Herunterladen des abgeschlossenen Projekts</span><span class="sxs-lookup"><span data-stu-id="646d9-106">Download Completed Project</span></span>](http://code.msdn.microsoft.com/SignalR-Getting-Started-b9d18aa9)

> <span data-ttu-id="646d9-107">Dieses Tutorial zeigt, wie Sie mithilfe von SignalR eine Chatanwendung mit Echtzeitfunktionalität erstellen.</span><span class="sxs-lookup"><span data-stu-id="646d9-107">This tutorial shows how to use SignalR to create a real-time chat application.</span></span> <span data-ttu-id="646d9-108">Sie werden eine leere ASP.NET-Webanwendung SignalR hinzu, und erstellen eine HTML-Seite zum Senden und Meldungen angezeigt werden.</span><span class="sxs-lookup"><span data-stu-id="646d9-108">You will add SignalR to an empty ASP.NET web application and create an HTML page to send and display messages.</span></span> 
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="646d9-109">In diesem Lernprogramm verwendeten Versionen der Software</span><span class="sxs-lookup"><span data-stu-id="646d9-109">Software versions used in the tutorial</span></span>
> 
> 
> - [<span data-ttu-id="646d9-110">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="646d9-110">Visual Studio 2013</span></span>](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - <span data-ttu-id="646d9-111">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="646d9-111">.NET 4.5</span></span>
> - <span data-ttu-id="646d9-112">SignalR Version 2</span><span class="sxs-lookup"><span data-stu-id="646d9-112">SignalR version 2</span></span>
>   
> 
> 
> ## <a name="using-visual-studio-2012-with-this-tutorial"></a><span data-ttu-id="646d9-113">Verwenden von Visual Studio 2012 mit diesem Lernprogramm</span><span class="sxs-lookup"><span data-stu-id="646d9-113">Using Visual Studio 2012 with this tutorial</span></span>
> 
> 
> <span data-ttu-id="646d9-114">Um Visual Studio 2012 mit diesem Lernprogramm verwenden, führen Sie folgende Schritte aus:</span><span class="sxs-lookup"><span data-stu-id="646d9-114">To use Visual Studio 2012 with this tutorial, do the following:</span></span>
> 
> - <span data-ttu-id="646d9-115">Update der [Package Manager](http://docs.nuget.org/docs/start-here/installing-nuget) auf die neueste Version.</span><span class="sxs-lookup"><span data-stu-id="646d9-115">Update your [Package Manager](http://docs.nuget.org/docs/start-here/installing-nuget) to the latest version.</span></span>
> - <span data-ttu-id="646d9-116">Installieren der [Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx).</span><span class="sxs-lookup"><span data-stu-id="646d9-116">Install the [Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx).</span></span>
> - <span data-ttu-id="646d9-117">Suchen Sie in den Webplattform-Installer, und installieren Sie **ASP.NET und 2013.1 von Web Tools für Visual Studio 2012**.</span><span class="sxs-lookup"><span data-stu-id="646d9-117">In the Web Platform Installer, search for and install **ASP.NET and Web Tools 2013.1 for Visual Studio 2012**.</span></span> <span data-ttu-id="646d9-118">Hierbei werden z. B. Visual Studio-Vorlagen für SignalR-Klassen installiert **Hub**.</span><span class="sxs-lookup"><span data-stu-id="646d9-118">This will install Visual Studio templates for SignalR classes such as **Hub**.</span></span>
> - <span data-ttu-id="646d9-119">Einige Vorlagen (z. B. **OWIN-Startklasse**) ist nicht verfügbar; verwenden Sie für diese stattdessen eine Klassendatei.</span><span class="sxs-lookup"><span data-stu-id="646d9-119">Some templates (such as **OWIN Startup Class**) will not be available; for these, use a Class file instead.</span></span>
> 
> 
> ## <a name="tutorial-versions"></a><span data-ttu-id="646d9-120">Lernprogramm-Versionen</span><span class="sxs-lookup"><span data-stu-id="646d9-120">Tutorial versions</span></span>
> 
> <span data-ttu-id="646d9-121">Informationen zu früheren Versionen von SignalR finden Sie unter [ältere Versionen von SignalR](../older-versions/index.md).</span><span class="sxs-lookup"><span data-stu-id="646d9-121">For information about earlier versions of SignalR, see [SignalR Older Versions](../older-versions/index.md).</span></span>
> 
> ## <a name="questions-and-comments"></a><span data-ttu-id="646d9-122">Fragen und Kommentare</span><span class="sxs-lookup"><span data-stu-id="646d9-122">Questions and comments</span></span>
> 
> <span data-ttu-id="646d9-123">Lassen Sie Sie Feedback auf wie in diesem Lernprogramm mögen und was wir in den Kommentaren am unteren Rand der Seite verbessern können.</span><span class="sxs-lookup"><span data-stu-id="646d9-123">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="646d9-124">Wenn Sie Fragen, die nicht direkt mit dem Lernprogramm verknüpft sind haben, bereitstellen können, die [ASP.NET SignalR-Forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) oder [StackOverflow.com](http://stackoverflow.com/).</span><span class="sxs-lookup"><span data-stu-id="646d9-124">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>


## <a name="overview"></a><span data-ttu-id="646d9-125">Übersicht</span><span class="sxs-lookup"><span data-stu-id="646d9-125">Overview</span></span>

<span data-ttu-id="646d9-126">Dieses Lernprogramm führt SignalR Entwicklung erfahren, wie eine einfache browserbasierte chatanwendung erstellt.</span><span class="sxs-lookup"><span data-stu-id="646d9-126">This tutorial introduces SignalR development by showing how to build a simple browser-based chat application.</span></span> <span data-ttu-id="646d9-127">Sie werden eine leere ASP.NET-Webanwendung SignalR-Bibliothek hinzugefügt, erstellen Sie eine hubklasse zum Senden von Nachrichten für Clients und erstellen eine HTML-Seite, mit dem Benutzer, senden und Empfangen von Nachrichten.</span><span class="sxs-lookup"><span data-stu-id="646d9-127">You will add the SignalR library to an empty ASP.NET web application, create a hub class for sending messages to clients, and create an HTML page that lets users send and receive chat messages.</span></span> <span data-ttu-id="646d9-128">Ein ähnliches Lernprogramm, das zum Erstellen einer chatanwendung in MVC 5 veranschaulicht eine MVC-Ansicht verwenden, finden Sie unter [erste Schritte mit SignalR-2 und MVC 5](tutorial-getting-started-with-signalr-and-mvc.md).</span><span class="sxs-lookup"><span data-stu-id="646d9-128">For a similar tutorial that shows how to create a chat application in MVC 5 using an MVC view, see [Getting Started with SignalR 2 and MVC 5](tutorial-getting-started-with-signalr-and-mvc.md).</span></span>

> [!NOTE]
> <span data-ttu-id="646d9-129">Dieses Lernprogramm veranschaulicht das Erstellen von SignalR-Anwendungen in Version 2.</span><span class="sxs-lookup"><span data-stu-id="646d9-129">This tutorial demonstrates how to create SignalR applications in version 2.</span></span> <span data-ttu-id="646d9-130">Weitere Informationen zu Änderungen zwischen SignalR 1.x und 2, finden Sie unter [aktualisieren SignalR 1.x Projekte](../releases/upgrading-signalr-1x-projects-to-20.md) und [Versionshinweisen zu Visual Studio 2013](../../../visual-studio/overview/2013/release-notes.md#TOC13).</span><span class="sxs-lookup"><span data-stu-id="646d9-130">For details on changes between SignalR 1.x and 2, see [Upgrading SignalR 1.x Projects](../releases/upgrading-signalr-1x-projects-to-20.md) and [Visual Studio 2013 Release Notes](../../../visual-studio/overview/2013/release-notes.md#TOC13).</span></span>

<span data-ttu-id="646d9-131">SignalR ist eine Open Source-.NET Bibliothek zum Erstellen von Webanwendungen, die Live Benutzerinteraktionen oder Daten in Echtzeit-Updates erfordern.</span><span class="sxs-lookup"><span data-stu-id="646d9-131">SignalR is an open-source .NET library for building web applications that require live user interaction or real-time data updates.</span></span> <span data-ttu-id="646d9-132">Beispiele sind soziale Anwendungen, mehrere Benutzer Spiele, Business-Zusammenarbeit und Neuigkeiten, Wetter oder finanzielle Aktualisierung von Anwendungen.</span><span class="sxs-lookup"><span data-stu-id="646d9-132">Examples include social applications, multiuser games, business collaboration, and news, weather, or financial update applications.</span></span> <span data-ttu-id="646d9-133">Diese werden häufig in Echtzeit Anwendungen bezeichnet.</span><span class="sxs-lookup"><span data-stu-id="646d9-133">These are often called real-time applications.</span></span>

<span data-ttu-id="646d9-134">SignalR vereinfacht das Erstellen von Anwendungen in Echtzeit.</span><span class="sxs-lookup"><span data-stu-id="646d9-134">SignalR simplifies the process of building real-time applications.</span></span> <span data-ttu-id="646d9-135">Er enthält eine ASP.NET-Serverbibliothek und eine JavaScript-Client-Bibliothek, damit sie leichter verwalten von Client / Server-Verbindungen und Inhaltsupdates an Clients übertragen.</span><span class="sxs-lookup"><span data-stu-id="646d9-135">It includes an ASP.NET server library and a JavaScript client library to make it easier to manage client-server connections and push content updates to clients.</span></span> <span data-ttu-id="646d9-136">Sie können eine vorhandene ASP.NET-Anwendung nach Echtzeit-Funktionen nutzen zu können die SignalR-Bibliothek hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="646d9-136">You can add the SignalR library to an existing ASP.NET application to gain real-time functionality.</span></span>

<span data-ttu-id="646d9-137">Im Lernprogramm wird die folgenden SignalR Entwicklungsaufgaben veranschaulicht:</span><span class="sxs-lookup"><span data-stu-id="646d9-137">The tutorial demonstrates the following SignalR development tasks:</span></span>

- <span data-ttu-id="646d9-138">Eine ASP.NET-Webanwendung hinzugefügt SignalR-Bibliothek.</span><span class="sxs-lookup"><span data-stu-id="646d9-138">Adding the SignalR library to an ASP.NET web application.</span></span>
- <span data-ttu-id="646d9-139">Erstellen einer hubklasse, um Inhalt an Clients zu verteilen.</span><span class="sxs-lookup"><span data-stu-id="646d9-139">Creating a hub class to push content to clients.</span></span>
- <span data-ttu-id="646d9-140">Erstellen eine OWIN-Start-Klasse zum Konfigurieren der Anwendung.</span><span class="sxs-lookup"><span data-stu-id="646d9-140">Creating an OWIN startup class to configure the application.</span></span>
- <span data-ttu-id="646d9-141">Verwenden die SignalR-jQuery-Bibliothek auf einer Webseite zum Senden von Nachrichten und Updates vom Hub angezeigt.</span><span class="sxs-lookup"><span data-stu-id="646d9-141">Using the SignalR jQuery library in a web page to send messages and display updates from the hub.</span></span>

<span data-ttu-id="646d9-142">Der folgende Screenshot zeigt die chatanwendung, die in einem Browser ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="646d9-142">The following screen shot shows the chat application running in a browser.</span></span> <span data-ttu-id="646d9-143">Jeder neuer Benutzer kann senden Kommentare und Kommentare hinzugefügt, nachdem der Benutzer über den Chat verknüpft.</span><span class="sxs-lookup"><span data-stu-id="646d9-143">Each new user can post comments and see comments added after the user joins the chat.</span></span>

![Chatinstanzen](tutorial-getting-started-with-signalr/_static/image1.png)

<span data-ttu-id="646d9-145">Abschnitte:</span><span class="sxs-lookup"><span data-stu-id="646d9-145">Sections:</span></span>

- [<span data-ttu-id="646d9-146">Richten Sie das Projekt</span><span class="sxs-lookup"><span data-stu-id="646d9-146">Set up the Project</span></span>](#setup)
- [<span data-ttu-id="646d9-147">Führen Sie das Beispiel</span><span class="sxs-lookup"><span data-stu-id="646d9-147">Run the Sample</span></span>](#run)
- [<span data-ttu-id="646d9-148">Überprüfen Sie den Code</span><span class="sxs-lookup"><span data-stu-id="646d9-148">Examine the Code</span></span>](#code)
- [<span data-ttu-id="646d9-149">Nächste Schritte</span><span class="sxs-lookup"><span data-stu-id="646d9-149">Next Steps</span></span>](#next)

<a id="setup"></a>

## <a name="set-up-the-project"></a><span data-ttu-id="646d9-150">Richten Sie das Projekt</span><span class="sxs-lookup"><span data-stu-id="646d9-150">Set up the Project</span></span>

<span data-ttu-id="646d9-151">Fügen Sie in diesem Abschnitt wird gezeigt, wie Visual Studio 2013 und SignalR Version 2 Erstellen Sie eine leere ASP.NET-Webanwendung mit SignalR hinzu, und erstellen Sie die chatanwendung.</span><span class="sxs-lookup"><span data-stu-id="646d9-151">This section shows how to use Visual Studio 2013 and SignalR version 2 to create an empty ASP.NET web application, add SignalR, and create the chat application.</span></span>

<span data-ttu-id="646d9-152">Erforderliche Komponenten:</span><span class="sxs-lookup"><span data-stu-id="646d9-152">Prerequisites:</span></span>

- <span data-ttu-id="646d9-153">Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="646d9-153">Visual Studio 2013.</span></span> <span data-ttu-id="646d9-154">Wenn Sie nicht über Visual Studio verfügen, finden Sie unter [ASP.NET Downloads](https://www.asp.net/downloads) das kostenlose Visual Studio 2013 Express Entwicklungstool abgerufen.</span><span class="sxs-lookup"><span data-stu-id="646d9-154">If you do not have Visual Studio, see [ASP.NET Downloads](https://www.asp.net/downloads) to get the free Visual Studio 2013 Express Development Tool.</span></span>

<span data-ttu-id="646d9-155">Verwenden Sie die folgenden Schritte aus Visual Studio 2013 erstellen eine leere ASP.NET-Webanwendung und Hinzufügen der SignalR-Bibliothek:</span><span class="sxs-lookup"><span data-stu-id="646d9-155">The following steps use Visual Studio 2013 to create an ASP.NET Empty Web Application and add the SignalR library:</span></span>

1. <span data-ttu-id="646d9-156">Erstellen Sie in Visual Studio eine ASP.NET-Webanwendung.</span><span class="sxs-lookup"><span data-stu-id="646d9-156">In Visual Studio, create an ASP.NET Web Application.</span></span>

    ![Erstellen von web](tutorial-getting-started-with-signalr/_static/image2.png)
2. <span data-ttu-id="646d9-158">In der **neues ASP.NET-Projekt** Fenster verlassen **leere** ausgewählt, und klicken Sie auf **Projekt erstellen**.</span><span class="sxs-lookup"><span data-stu-id="646d9-158">In the **New ASP.NET Project** window, leave **Empty** selected and click **Create Project**.</span></span>

    ![Erstellen Sie leeres web](tutorial-getting-started-with-signalr/_static/image3.png)
3. <span data-ttu-id="646d9-160">In **Projektmappen-Explorer**mit der rechten Maustaste auf das Projekt, wählen Sie **hinzufügen | SignalR-Hub-Klasse (v2)**.</span><span class="sxs-lookup"><span data-stu-id="646d9-160">In **Solution Explorer**, right-click the project, select **Add | SignalR Hub Class (v2)**.</span></span> <span data-ttu-id="646d9-161">Benennen Sie die Klasse **ChatHub.cs** und dem Projekt hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="646d9-161">Name the class **ChatHub.cs** and add it to the project.</span></span> <span data-ttu-id="646d9-162">In diesem Schritt wird die **ChatHub** -Klasse und fügt Sie dem Projekt eine Reihe von Skriptdateien und Assemblyverweisen, die SignalR unterstützen.</span><span class="sxs-lookup"><span data-stu-id="646d9-162">This step creates the **ChatHub** class and adds to the project a set of script files and assembly references that support SignalR.</span></span>

    > [!NOTE]
    > <span data-ttu-id="646d9-163">Sie können auch SignalR zu einem Projekt hinzufügen, indem Sie öffnen die **Tools | Bibliothek-Paket-Manager | Paket-Manager-Konsole** und einen Befehl ausführen:</span><span class="sxs-lookup"><span data-stu-id="646d9-163">You can also add SignalR to a project by opening the **Tools | Library Package Manager | Package Manager Console** and running a command:</span></span>

    `install-package Microsoft.AspNet.SignalR`

    <span data-ttu-id="646d9-164">Wenn Sie die Konsole verwenden, um SignalR hinzuzufügen, erstellen Sie die SignalR-Hub-Klasse als separater Schritt nach dem Hinzufügen von SignalR.</span><span class="sxs-lookup"><span data-stu-id="646d9-164">If you use the console to add SignalR, create the SignalR hub class as a separate step after you add SignalR.</span></span>

    > [!NOTE]
    > <span data-ttu-id="646d9-165">Bei Verwendung von Visual Studio 2012 die **SignalR-Hub-Klasse (v2)** Vorlage ist nicht verfügbar.</span><span class="sxs-lookup"><span data-stu-id="646d9-165">If you are using Visual Studio 2012, the **SignalR Hub Class (v2)** template will not be available.</span></span> <span data-ttu-id="646d9-166">Sie können ein einfachen hinzufügen **Klasse** aufgerufen `ChatHub` stattdessen.</span><span class="sxs-lookup"><span data-stu-id="646d9-166">You can add a plain **Class** called `ChatHub` instead.</span></span>
4. <span data-ttu-id="646d9-167">In **Projektmappen-Explorer**, erweitern Sie den Knoten des Skripts.</span><span class="sxs-lookup"><span data-stu-id="646d9-167">In **Solution Explorer**, expand the Scripts node.</span></span> <span data-ttu-id="646d9-168">Skriptbibliotheken für jQuery und SignalR werden in das Projekt angezeigt.</span><span class="sxs-lookup"><span data-stu-id="646d9-168">Script libraries for jQuery and SignalR are visible in the project.</span></span>
5. <span data-ttu-id="646d9-169">Ersetzen Sie den Code in der neuen **ChatHub** Klasse durch den folgenden Code.</span><span class="sxs-lookup"><span data-stu-id="646d9-169">Replace the code in the new **ChatHub** class with the following code.</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample1.cs)]
6. <span data-ttu-id="646d9-170">In **Projektmappen-Explorer**Sie mit der rechten Maustaste des Projekts, und klicken Sie auf **hinzufügen | OWIN-Startklasse**.</span><span class="sxs-lookup"><span data-stu-id="646d9-170">In **Solution Explorer**, right-click the project, then click **Add | OWIN Startup Class**.</span></span> <span data-ttu-id="646d9-171">Benennen Sie die neue Klasse `Startup` , und klicken Sie auf OK.</span><span class="sxs-lookup"><span data-stu-id="646d9-171">Name the new class `Startup` and click OK.</span></span>

    > [!NOTE]
    > <span data-ttu-id="646d9-172">Bei Verwendung von Visual Studio 2012 die **OWIN-Startklasse** Vorlage ist nicht verfügbar.</span><span class="sxs-lookup"><span data-stu-id="646d9-172">If you are using Visual Studio 2012, the **OWIN Startup Class** template will not be available.</span></span> <span data-ttu-id="646d9-173">Sie können ein einfachen hinzufügen **Klasse** aufgerufen `Startup` stattdessen.</span><span class="sxs-lookup"><span data-stu-id="646d9-173">You can add a plain **Class** called `Startup` instead.</span></span>
7. <span data-ttu-id="646d9-174">Ändern Sie den Inhalt der neuen Startklasse folgt.</span><span class="sxs-lookup"><span data-stu-id="646d9-174">Change the contents of the new Startup class to the following.</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample2.cs)]
8. <span data-ttu-id="646d9-175">In **Projektmappen-Explorer**Sie mit der rechten Maustaste des Projekts, und klicken Sie auf **hinzufügen | HTML-Seite**.</span><span class="sxs-lookup"><span data-stu-id="646d9-175">In **Solution Explorer**, right-click the project, then click **Add | HTML Page**.</span></span> <span data-ttu-id="646d9-176">Nennen Sie die neue Seite `index.html`.</span><span class="sxs-lookup"><span data-stu-id="646d9-176">Name the new page `index.html`.</span></span>
    >[!NOTE]
    ><span data-ttu-id="646d9-177">Sie müssen möglicherweise die Versionsnummern für die Verweise auf Bibliotheken JQuery und SignalR ändern</span><span class="sxs-lookup"><span data-stu-id="646d9-177">You might need to change the version numbers for the references to JQuery and SignalR libraries</span></span>
9. <span data-ttu-id="646d9-178">In **Projektmappen-Explorer**mit der rechten Maustaste auf das HTML-Seite, die Sie gerade erstellt haben, und klicken Sie auf **als Startseite festlegen**.</span><span class="sxs-lookup"><span data-stu-id="646d9-178">In **Solution Explorer**, right-click the HTML page you just created and click **Set as Start Page**.</span></span>
10. <span data-ttu-id="646d9-179">Ersetzen Sie den Standardcode in der HTML-Seite mit den folgenden Code ein.</span><span class="sxs-lookup"><span data-stu-id="646d9-179">Replace the default code in the HTML page with the following code.</span></span>

    > [!NOTE]
    > <span data-ttu-id="646d9-180">Eine höhere Version der SignalR-Skripts kann von den Paket-Manager installiert werden.</span><span class="sxs-lookup"><span data-stu-id="646d9-180">A later version of the SignalR scripts may be installed by the package manager.</span></span> <span data-ttu-id="646d9-181">Stellen Sie sicher, dass die Versionen der Skriptdateien im Projekt die folgenden Skriptverweise entsprechen (sie werden anders, wenn Sie SignalR mithilfe von NuGet, anstatt einen Hub hinzugefügt.)</span><span class="sxs-lookup"><span data-stu-id="646d9-181">Verify that the script references below correspond to the versions of the script files in the project (they will be different if you added SignalR using NuGet rather than adding a hub.)</span></span>

    [!code-html[Main](tutorial-getting-started-with-signalr/samples/sample3.html)]
11. <span data-ttu-id="646d9-182">**Speichern Sie alle** für das Projekt.</span><span class="sxs-lookup"><span data-stu-id="646d9-182">**Save All** for the project.</span></span>

<a id="run"></a>

## <a name="run-the-sample"></a><span data-ttu-id="646d9-183">Führen Sie das Beispiel</span><span class="sxs-lookup"><span data-stu-id="646d9-183">Run the Sample</span></span>

1. <span data-ttu-id="646d9-184">Drücken Sie F5, um das Projekt im Debugmodus auszuführen.</span><span class="sxs-lookup"><span data-stu-id="646d9-184">Press F5 to run the project in debug mode.</span></span> <span data-ttu-id="646d9-185">Die HTML-Seite lädt in einen Browserinstanz und fordert Sie auf einen Benutzernamen ein.</span><span class="sxs-lookup"><span data-stu-id="646d9-185">The HTML page loads in a browser instance and prompts for a user name.</span></span>

    ![Benutzername eingeben](tutorial-getting-started-with-signalr/_static/image4.png)
2. <span data-ttu-id="646d9-187">Geben Sie einen Benutzernamen ein.</span><span class="sxs-lookup"><span data-stu-id="646d9-187">Enter a user name.</span></span>
3. <span data-ttu-id="646d9-188">Kopieren Sie die URL aus der Adresszeile des Browsers ein, und verwenden Sie, um zwei weitere Browserinstanzen öffnen.</span><span class="sxs-lookup"><span data-stu-id="646d9-188">Copy the URL from the address line of the browser and use it to open two more browser instances.</span></span> <span data-ttu-id="646d9-189">Geben Sie in den einzelnen Instanzen Browser einen eindeutigen Benutzernamen aus.</span><span class="sxs-lookup"><span data-stu-id="646d9-189">In each browser instance, enter a unique user name.</span></span>
4. <span data-ttu-id="646d9-190">Klicken Sie in den einzelnen Instanzen Browser einen Kommentar hinzufügen, und klicken Sie auf **senden**.</span><span class="sxs-lookup"><span data-stu-id="646d9-190">In each browser instance, add a comment and click **Send**.</span></span> <span data-ttu-id="646d9-191">Die Kommentare sollten in alle Browserinstanzen angezeigt werden.</span><span class="sxs-lookup"><span data-stu-id="646d9-191">The comments should display in all browser instances.</span></span>

    > [!NOTE]
    > <span data-ttu-id="646d9-192">Diese einfache chatanwendung behält den Kontext der Diskussion auf dem Server.</span><span class="sxs-lookup"><span data-stu-id="646d9-192">This simple chat application does not maintain the discussion context on the server.</span></span> <span data-ttu-id="646d9-193">Der Hub, Kommentare, um alle aktuellen Benutzer sendet.</span><span class="sxs-lookup"><span data-stu-id="646d9-193">The hub broadcasts comments to all current users.</span></span> <span data-ttu-id="646d9-194">Benutzer, die den Chat später beizutreten sehen, dass Nachrichten ab dem Zeitpunkt hinzugefügt werden, dass sie verknüpfen.</span><span class="sxs-lookup"><span data-stu-id="646d9-194">Users who join the chat later will see messages added from the time they join.</span></span>

    <span data-ttu-id="646d9-195">Der folgende Screenshot zeigt die chatanwendung, die in drei Browserinstanzen, die aktualisiert werden, wenn eine Instanz eine Nachricht sendet ausgeführt wird:</span><span class="sxs-lookup"><span data-stu-id="646d9-195">The following screen shot shows the chat application running in three browser instances, all of which are updated when one instance sends a message:</span></span>

    ![Chat-Browser](tutorial-getting-started-with-signalr/_static/image5.png)
5. <span data-ttu-id="646d9-197">In **Projektmappen-Explorer**, überprüfen Sie die **Skriptdokumente** Knoten für die ausgeführte Anwendung.</span><span class="sxs-lookup"><span data-stu-id="646d9-197">In **Solution Explorer**, inspect the **Script Documents** node for the running application.</span></span> <span data-ttu-id="646d9-198">Es wird eine Skriptdatei mit dem Namen **Hubs** , die die SignalR-Bibliothek wird zur Laufzeit dynamisch generiert.</span><span class="sxs-lookup"><span data-stu-id="646d9-198">There is a script file named **hubs** that the SignalR library dynamically generates at runtime.</span></span> <span data-ttu-id="646d9-199">Diese Datei wird die Kommunikation zwischen jQuery-Skripts und serverseitigen Code verwaltet.</span><span class="sxs-lookup"><span data-stu-id="646d9-199">This file manages the communication between jQuery script and server-side code.</span></span>

    ![](tutorial-getting-started-with-signalr/_static/image6.png)

<a id="code"></a>

## <a name="examine-the-code"></a><span data-ttu-id="646d9-200">Überprüfen Sie den Code</span><span class="sxs-lookup"><span data-stu-id="646d9-200">Examine the Code</span></span>

<span data-ttu-id="646d9-201">Die SignalR-Chat-Anwendung zeigt zwei grundlegende SignalR Entwicklungsaufgaben: Erstellen eines Hubs wie das Hauptfenster Koordinierung-Objekt, auf dem Server und mithilfe der SignalR-jQuery-Bibliothek zum Senden und Empfangen von Nachrichten.</span><span class="sxs-lookup"><span data-stu-id="646d9-201">The SignalR chat application demonstrates two basic SignalR development tasks: creating a hub as the main coordination object on the server, and using the SignalR jQuery library to send and receive messages.</span></span>

### <a name="signalr-hubs"></a><span data-ttu-id="646d9-202">SignalR Hubs</span><span class="sxs-lookup"><span data-stu-id="646d9-202">SignalR Hubs</span></span>

<span data-ttu-id="646d9-203">Im Codebeispiel den **ChatHub** Klasse leitet sich von der **Microsoft.AspNet.SignalR.Hub** Klasse.</span><span class="sxs-lookup"><span data-stu-id="646d9-203">In the code sample the **ChatHub** class derives from the **Microsoft.AspNet.SignalR.Hub** class.</span></span> <span data-ttu-id="646d9-204">Ableiten von der **Hub** Klasse ist eine hilfreiche Möglichkeit zum Erstellen einer SignalR-Anwendung.</span><span class="sxs-lookup"><span data-stu-id="646d9-204">Deriving from the **Hub** class is a useful way to build a SignalR application.</span></span> <span data-ttu-id="646d9-205">Sie können öffentliche Methoden in der hubklasse erstellen und dann Zugriff auf diese Methoden durch Aufruf aus Skripts auf einer Webseite.</span><span class="sxs-lookup"><span data-stu-id="646d9-205">You can create public methods on your hub class and then access those methods by calling them from scripts in a web page.</span></span>

<span data-ttu-id="646d9-206">Rufen Sie in den Code Chat Clients die **ChatHub.Send** Methode, um eine neue Nachricht zu senden.</span><span class="sxs-lookup"><span data-stu-id="646d9-206">In the chat code, clients call the **ChatHub.Send** method to send a new message.</span></span> <span data-ttu-id="646d9-207">Der Hub sendet wiederum die Nachricht an alle Clients durch den Aufruf **Clients.All.broadcastMessage**.</span><span class="sxs-lookup"><span data-stu-id="646d9-207">The hub in turn sends the message to all clients by calling **Clients.All.broadcastMessage**.</span></span>

<span data-ttu-id="646d9-208">Die **senden** einige Konzepte der Hub-Methode veranschaulicht:</span><span class="sxs-lookup"><span data-stu-id="646d9-208">The **Send** method demonstrates several hub concepts :</span></span>

- <span data-ttu-id="646d9-209">Öffentliche Methoden für einen Hub deklariert werden, damit Clients, die sie aufrufen können.</span><span class="sxs-lookup"><span data-stu-id="646d9-209">Declare public methods on a hub so that clients can call them.</span></span>
- <span data-ttu-id="646d9-210">Verwenden der **Microsoft.AspNet.SignalR.Hub.Clients** dynamische Eigenschaft auf alle Clients, die mit diesem Hub verbunden sind.</span><span class="sxs-lookup"><span data-stu-id="646d9-210">Use the **Microsoft.AspNet.SignalR.Hub.Clients** dynamic property to access all clients connected to this hub.</span></span>
- <span data-ttu-id="646d9-211">Aufrufen einer Funktion auf dem Client (z. B. die `broadcastMessage` Funktion) so aktualisieren Sie Clients.</span><span class="sxs-lookup"><span data-stu-id="646d9-211">Call a function on the client (such as the `broadcastMessage` function) to update clients.</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample4.cs)]

### <a name="signalr-and-jquery"></a><span data-ttu-id="646d9-212">SignalR und jQuery</span><span class="sxs-lookup"><span data-stu-id="646d9-212">SignalR and jQuery</span></span>

<span data-ttu-id="646d9-213">Die HTML-Seite im Codebeispiel zeigt, wie die SignalR-jQuery-Bibliothek für die Kommunikation mit SignalR-Hubs.</span><span class="sxs-lookup"><span data-stu-id="646d9-213">The HTML page in the code sample shows how to use the SignalR jQuery library to communicate with a SignalR hub.</span></span> <span data-ttu-id="646d9-214">Die Durchführung wichtiger Aufgaben im Code deklarieren ein Proxys für den Hub, Deklarieren einer Funktion, die der Server auf Push-Inhalte für Clients aufrufen können, und starten eine Verbindung zum Senden von Nachrichten an den Hub zu verweisen.</span><span class="sxs-lookup"><span data-stu-id="646d9-214">The essential tasks in the code are declaring a proxy to reference the hub, declaring a function that the server can call to push content to clients, and starting a connection to send messages to the hub.</span></span>

<span data-ttu-id="646d9-215">Der folgende Code deklariert einen Verweis auf einen hubproxy.</span><span class="sxs-lookup"><span data-stu-id="646d9-215">The following code declares a reference to a hub proxy.</span></span>

[!code-javascript[Main](tutorial-getting-started-with-signalr/samples/sample5.js)]

> [!NOTE]
> <span data-ttu-id="646d9-216">In JavaScript wird der Verweis auf die Server-Klasse und ihre Member in Camel-Case.</span><span class="sxs-lookup"><span data-stu-id="646d9-216">In JavaScript the reference to the server class and its members is in camel case.</span></span> <span data-ttu-id="646d9-217">Im Codebeispiel verweist auf die C#- **ChatHub** Klasse in JavaScript als **ChatHub**.</span><span class="sxs-lookup"><span data-stu-id="646d9-217">The code sample references the C# **ChatHub** class in JavaScript as **chatHub**.</span></span>


<span data-ttu-id="646d9-218">Der folgende Code stammt, wie Sie eine Rückruffunktion in das Skript erstellen.</span><span class="sxs-lookup"><span data-stu-id="646d9-218">The following code is how you create a callback function in the script.</span></span> <span data-ttu-id="646d9-219">Die hubklasse, auf dem Server ruft diese Funktion um Inhaltsupdates an jeden Client per Push übertragen.</span><span class="sxs-lookup"><span data-stu-id="646d9-219">The hub class on the server calls this function to push content updates to each client.</span></span> <span data-ttu-id="646d9-220">Die beiden Zeilen an, dass HTML-Codierung den Inhalt vor der Anzeige sind optional, und zeigen eine einfache Möglichkeit zum Skript-Injection verhindern.</span><span class="sxs-lookup"><span data-stu-id="646d9-220">The two lines that HTML encode the content before displaying it are optional and show a simple way to prevent script injection.</span></span>

[!code-html[Main](tutorial-getting-started-with-signalr/samples/sample6.html)]

<span data-ttu-id="646d9-221">Der folgende Code zeigt, wie Sie eine Verbindung mit dem Hub zu öffnen.</span><span class="sxs-lookup"><span data-stu-id="646d9-221">The following code shows how to open a connection with the hub.</span></span> <span data-ttu-id="646d9-222">Der Code wird die Verbindung gestartet und übergibt diese eine Funktion, um das Click-Ereignis zu behandeln, auf die **senden** Schaltfläche in der HTML-Seite.</span><span class="sxs-lookup"><span data-stu-id="646d9-222">The code starts the connection and then passes it a function to handle the click event on the **Send** button in the HTML page.</span></span>

> [!NOTE]
> <span data-ttu-id="646d9-223">Dieser Ansatz wird sichergestellt, dass die Verbindung hergestellt wird, bevor der Ereignishandler ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="646d9-223">This approach ensures that the connection is established before the event handler executes.</span></span>


[!code-javascript[Main](tutorial-getting-started-with-signalr/samples/sample7.js)]

<a id="next"></a>

## <a name="next-steps"></a><span data-ttu-id="646d9-224">Nächste Schritte</span><span class="sxs-lookup"><span data-stu-id="646d9-224">Next Steps</span></span>

<span data-ttu-id="646d9-225">Sie erfahren, dass SignalR ein Framework zur Erstellung von Echtzeit-Webanwendungen.</span><span class="sxs-lookup"><span data-stu-id="646d9-225">You learned that SignalR is a framework for building real-time web applications.</span></span> <span data-ttu-id="646d9-226">Sie haben auch erfahren, mehrere SignalR Entwicklungsaufgaben: zum Hinzufügen von SignalR an eine ASP.NET-Anwendung zum Erstellen einer hubklasse und zum Senden und Empfangen von Nachrichten über den Hub.</span><span class="sxs-lookup"><span data-stu-id="646d9-226">You also learned several SignalR development tasks: how to add SignalR to an ASP.NET application, how to create a hub class, and how to send and receive messages from the hub.</span></span>

<span data-ttu-id="646d9-227">Eine exemplarische Vorgehensweise, die SignalR-beispielanwendung in Azure bereitzustellen, finden Sie unter [SignalR mit Web-Apps in Azure App Service mithilfe von](../deployment/using-signalr-with-azure-web-sites.md).</span><span class="sxs-lookup"><span data-stu-id="646d9-227">For a walkthrough on how to deploy the sample SignalR application to Azure, see [Using SignalR with Web Apps in Azure App Service](../deployment/using-signalr-with-azure-web-sites.md).</span></span> <span data-ttu-id="646d9-228">Ausführliche Informationen dazu, wie Sie ein Visual Studio-Webprojekt auf einer Windows Azure-Website bereitstellen, finden Sie unter [erstellen eine ASP.NET Web-app in Azure App Service](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-get-started/).</span><span class="sxs-lookup"><span data-stu-id="646d9-228">For detailed information about how to deploy a Visual Studio web project to a Windows Azure Web Site, see [Create an ASP.NET web app in Azure App Service](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-get-started/).</span></span>

<span data-ttu-id="646d9-229">Erweiterte Konzepte von SignalR-Entwicklungen finden Sie auf den folgenden Websites für SignalR-Quellcode und Ressourcen:</span><span class="sxs-lookup"><span data-stu-id="646d9-229">To learn more advanced SignalR developments concepts, visit the following sites for SignalR source code and resources:</span></span>

- [<span data-ttu-id="646d9-230">SignalR-Projekt</span><span class="sxs-lookup"><span data-stu-id="646d9-230">SignalR Project</span></span>](http://signalr.net)
- [<span data-ttu-id="646d9-231">SignalR Github und Beispiele</span><span class="sxs-lookup"><span data-stu-id="646d9-231">SignalR Github and Samples</span></span>](https://github.com/SignalR/SignalR)
- [<span data-ttu-id="646d9-232">SignalR Wiki</span><span class="sxs-lookup"><span data-stu-id="646d9-232">SignalR Wiki</span></span>](https://github.com/SignalR/SignalR/wiki)
