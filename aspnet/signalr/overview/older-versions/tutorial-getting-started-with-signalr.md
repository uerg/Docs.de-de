---
uid: signalr/overview/older-versions/tutorial-getting-started-with-signalr
title: 'Lernprogramm: Erste Schritte mit SignalR 1.x | Microsoft Docs'
author: pfletcher
description: Verwenden Sie ASP.NET SignalR eine Echtzeit-Chat-Anwendung in eine HTML-Seite erstellt.
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2013
ms.topic: article
ms.assetid: fdc3599a-5217-44c1-951f-0eec9812dce7
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/older-versions/tutorial-getting-started-with-signalr
msc.type: authoredcontent
ms.openlocfilehash: ce4953a0abf64af28ef4dbc5a62bb2d989343d99
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/24/2018
---
<a name="tutorial-getting-started-with-signalr-1x"></a><span data-ttu-id="abf19-103">Lernprogramm: Erste Schritte mit SignalR 1.x</span><span class="sxs-lookup"><span data-stu-id="abf19-103">Tutorial: Getting Started with SignalR 1.x</span></span>
====================
<span data-ttu-id="abf19-104">durch [Patrick Fletcher](https://github.com/pfletcher), [Tim Teebken](https://github.com/timlt)</span><span class="sxs-lookup"><span data-stu-id="abf19-104">by [Patrick Fletcher](https://github.com/pfletcher), [Tim Teebken](https://github.com/timlt)</span></span>

> <span data-ttu-id="abf19-105">Dieses Tutorial zeigt, wie Sie mithilfe von SignalR eine Chatanwendung mit Echtzeitfunktionalität erstellen.</span><span class="sxs-lookup"><span data-stu-id="abf19-105">This tutorial shows how to use SignalR to create a real-time chat application.</span></span> <span data-ttu-id="abf19-106">Sie werden eine leere ASP.NET-Webanwendung SignalR hinzu, und erstellen eine HTML-Seite zum Senden und Meldungen angezeigt werden.</span><span class="sxs-lookup"><span data-stu-id="abf19-106">You will add SignalR to an empty ASP.NET web application and create an HTML page to send and display messages.</span></span>


## <a name="overview"></a><span data-ttu-id="abf19-107">Übersicht</span><span class="sxs-lookup"><span data-stu-id="abf19-107">Overview</span></span>

<span data-ttu-id="abf19-108">Dieses Lernprogramm führt SignalR Entwicklung erfahren, wie eine einfache browserbasierte chatanwendung erstellt.</span><span class="sxs-lookup"><span data-stu-id="abf19-108">This tutorial introduces SignalR development by showing how to build a simple browser-based chat application.</span></span> <span data-ttu-id="abf19-109">Sie werden eine leere ASP.NET-Webanwendung SignalR-Bibliothek hinzugefügt, erstellen Sie eine hubklasse zum Senden von Nachrichten für Clients und erstellen eine HTML-Seite, mit dem Benutzer, senden und Empfangen von Nachrichten.</span><span class="sxs-lookup"><span data-stu-id="abf19-109">You will add the SignalR library to an empty ASP.NET web application, create a hub class for sending messages to clients, and create an HTML page that lets users send and receive chat messages.</span></span> <span data-ttu-id="abf19-110">Ein ähnliches Lernprogramm, das zum Erstellen einer chatanwendung in MVC 4 veranschaulicht eine MVC-Ansicht verwenden, finden Sie unter [erste Schritte mit SignalR und MVC 4](index.md).</span><span class="sxs-lookup"><span data-stu-id="abf19-110">For a similar tutorial that shows how to create a chat application in MVC 4 using an MVC view, see [Getting Started with SignalR and MVC 4](index.md).</span></span>

> [!NOTE]
> <span data-ttu-id="abf19-111">Dieses Lernprogramm verwendet die Releaseversion (1.x) der SignalR.</span><span class="sxs-lookup"><span data-stu-id="abf19-111">This tutorial uses the release (1.x) version of SignalR.</span></span> <span data-ttu-id="abf19-112">Weitere Informationen zu Änderungen zwischen SignalR 1.x und 2.0 finden Sie unter [aktualisieren SignalR 1.x Projekte](../releases/upgrading-signalr-1x-projects-to-20.md).</span><span class="sxs-lookup"><span data-stu-id="abf19-112">For details on changes between SignalR 1.x and 2.0, see [Upgrading SignalR 1.x Projects](../releases/upgrading-signalr-1x-projects-to-20.md).</span></span>

<span data-ttu-id="abf19-113">SignalR ist eine Open Source-.NET Bibliothek zum Erstellen von Webanwendungen, die Live Benutzerinteraktionen oder Daten in Echtzeit-Updates erfordern.</span><span class="sxs-lookup"><span data-stu-id="abf19-113">SignalR is an open-source .NET library for building web applications that require live user interaction or real-time data updates.</span></span> <span data-ttu-id="abf19-114">Beispiele sind soziale Anwendungen, mehrere Benutzer Spiele, Business-Zusammenarbeit und Neuigkeiten, Wetter oder finanzielle Aktualisierung von Anwendungen.</span><span class="sxs-lookup"><span data-stu-id="abf19-114">Examples include social applications, multiuser games, business collaboration, and news, weather, or financial update applications.</span></span> <span data-ttu-id="abf19-115">Diese werden häufig in Echtzeit Anwendungen bezeichnet.</span><span class="sxs-lookup"><span data-stu-id="abf19-115">These are often called real-time applications.</span></span>

<span data-ttu-id="abf19-116">SignalR vereinfacht das Erstellen von Anwendungen in Echtzeit.</span><span class="sxs-lookup"><span data-stu-id="abf19-116">SignalR simplifies the process of building real-time applications.</span></span> <span data-ttu-id="abf19-117">Er enthält eine ASP.NET-Serverbibliothek und eine JavaScript-Client-Bibliothek, damit sie leichter verwalten von Client / Server-Verbindungen und Inhaltsupdates an Clients übertragen.</span><span class="sxs-lookup"><span data-stu-id="abf19-117">It includes an ASP.NET server library and a JavaScript client library to make it easier to manage client-server connections and push content updates to clients.</span></span> <span data-ttu-id="abf19-118">Sie können eine vorhandene ASP.NET-Anwendung nach Echtzeit-Funktionen nutzen zu können die SignalR-Bibliothek hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="abf19-118">You can add the SignalR library to an existing ASP.NET application to gain real-time functionality.</span></span>

<span data-ttu-id="abf19-119">Im Lernprogramm wird die folgenden SignalR Entwicklungsaufgaben veranschaulicht:</span><span class="sxs-lookup"><span data-stu-id="abf19-119">The tutorial demonstrates the following SignalR development tasks:</span></span>

- <span data-ttu-id="abf19-120">Eine ASP.NET-Webanwendung hinzugefügt SignalR-Bibliothek.</span><span class="sxs-lookup"><span data-stu-id="abf19-120">Adding the SignalR library to an ASP.NET web application.</span></span>
- <span data-ttu-id="abf19-121">Erstellen einer hubklasse, um Inhalt an Clients zu verteilen.</span><span class="sxs-lookup"><span data-stu-id="abf19-121">Creating a hub class to push content to clients.</span></span>
- <span data-ttu-id="abf19-122">Verwenden die SignalR-jQuery-Bibliothek auf einer Webseite zum Senden von Nachrichten und Updates vom Hub angezeigt.</span><span class="sxs-lookup"><span data-stu-id="abf19-122">Using the SignalR jQuery library in a web page to send messages and display updates from the hub.</span></span>

<span data-ttu-id="abf19-123">Der folgende Screenshot zeigt die chatanwendung, die in einem Browser ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="abf19-123">The following screen shot shows the chat application running in a browser.</span></span> <span data-ttu-id="abf19-124">Jeder neuer Benutzer kann senden Kommentare und Kommentare hinzugefügt, nachdem der Benutzer über den Chat verknüpft.</span><span class="sxs-lookup"><span data-stu-id="abf19-124">Each new user can post comments and see comments added after the user joins the chat.</span></span>

![Chatinstanzen](tutorial-getting-started-with-signalr/_static/image1.png)

<span data-ttu-id="abf19-126">Abschnitte:</span><span class="sxs-lookup"><span data-stu-id="abf19-126">Sections:</span></span>

- [<span data-ttu-id="abf19-127">Richten Sie das Projekt</span><span class="sxs-lookup"><span data-stu-id="abf19-127">Set up the Project</span></span>](#setup)
- [<span data-ttu-id="abf19-128">Führen Sie das Beispiel</span><span class="sxs-lookup"><span data-stu-id="abf19-128">Run the Sample</span></span>](#run)
- [<span data-ttu-id="abf19-129">Überprüfen Sie den Code</span><span class="sxs-lookup"><span data-stu-id="abf19-129">Examine the Code</span></span>](#code)
- [<span data-ttu-id="abf19-130">Nächste Schritte</span><span class="sxs-lookup"><span data-stu-id="abf19-130">Next Steps</span></span>](#next)

<a id="setup"></a>

## <a name="set-up-the-project"></a><span data-ttu-id="abf19-131">Richten Sie das Projekt</span><span class="sxs-lookup"><span data-stu-id="abf19-131">Set up the Project</span></span>

<span data-ttu-id="abf19-132">Fügen Sie in diesem Abschnitt wird gezeigt, wie eine leere ASP.NET-Webanwendung erstellt SignalR hinzu, und erstellen Sie die chatanwendung.</span><span class="sxs-lookup"><span data-stu-id="abf19-132">This section shows how to create an empty ASP.NET web application, add SignalR, and create the chat application.</span></span>

<span data-ttu-id="abf19-133">Erforderliche Komponenten:</span><span class="sxs-lookup"><span data-stu-id="abf19-133">Prerequisites:</span></span>

- <span data-ttu-id="abf19-134">Visual Studio 2010 SP1 oder 2012.</span><span class="sxs-lookup"><span data-stu-id="abf19-134">Visual Studio 2010 SP1 or 2012.</span></span> <span data-ttu-id="abf19-135">Wenn Sie nicht über Visual Studio verfügen, finden Sie unter [ASP.NET Downloads](https://www.asp.net/downloads) das kostenlose Visual Studio 2012 Express Entwicklungstool abgerufen.</span><span class="sxs-lookup"><span data-stu-id="abf19-135">If you do not have Visual Studio, see [ASP.NET Downloads](https://www.asp.net/downloads) to get the free Visual Studio 2012 Express Development Tool.</span></span>
- <span data-ttu-id="abf19-136">[Microsoft ASP.NET und Tools 2012.2](https://go.microsoft.com/fwlink/?LinkId=279941).</span><span class="sxs-lookup"><span data-stu-id="abf19-136">[Microsoft ASP.NET and Web Tools 2012.2](https://go.microsoft.com/fwlink/?LinkId=279941).</span></span> <span data-ttu-id="abf19-137">Für Visual Studio 2012 fügt dieses Installationsprogramm neue ASP.NET-Funktionen einschließlich SignalR-Vorlagen zu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="abf19-137">For Visual Studio 2012, this installer adds new ASP.NET features including SignalR templates to Visual Studio.</span></span> <span data-ttu-id="abf19-138">Für Visual Studio 2010 SP1 in einem Installationsprogramm ist nicht verfügbar, aber Sie können das Lernprogramm abschließen, indem die SignalR-NuGet-Paket installieren, wie in den setupschritten beschrieben.</span><span class="sxs-lookup"><span data-stu-id="abf19-138">For Visual Studio 2010 SP1, an installer is not available but you can complete the tutorial by installing the SignalR NuGet package as described in the setup steps.</span></span>

<span data-ttu-id="abf19-139">Die folgenden Schritte verwenden Visual Studio 2012, um eine leere ASP.NET-Webanwendung erstellen und Hinzufügen der SignalR-Bibliothek:</span><span class="sxs-lookup"><span data-stu-id="abf19-139">The following steps use Visual Studio 2012 to create an ASP.NET Empty Web Application and add the SignalR library:</span></span>

1. <span data-ttu-id="abf19-140">Erstellen Sie in Visual Studio eine leere ASP.NET-Webanwendung.</span><span class="sxs-lookup"><span data-stu-id="abf19-140">In Visual Studio create an ASP.NET Empty Web Application.</span></span>

    ![Erstellen Sie leeres web](tutorial-getting-started-with-signalr/_static/image2.png)
2. <span data-ttu-id="abf19-142">Öffnen der **Package Manager Console** dazu **Tools | Bibliothek-Paket-Manager | Paket-Manager-Konsole**.</span><span class="sxs-lookup"><span data-stu-id="abf19-142">Open the **Package Manager Console** by selecting **Tools | Library Package Manager | Package Manager Console**.</span></span> <span data-ttu-id="abf19-143">Geben Sie den folgenden Befehl in das Konsolenfenster aus:</span><span class="sxs-lookup"><span data-stu-id="abf19-143">Enter the following command into the console window:</span></span>

    `Install-Package Microsoft.AspNet.SignalR -Version 1.1.3`

    <span data-ttu-id="abf19-144">Dieser Befehl installiert die neueste Version von SignalR 1.x.</span><span class="sxs-lookup"><span data-stu-id="abf19-144">This command installs the latest version of SignalR 1.x.</span></span>
3. <span data-ttu-id="abf19-145">In **Projektmappen-Explorer**mit der rechten Maustaste auf das Projekt, wählen Sie **hinzufügen | Klasse**.</span><span class="sxs-lookup"><span data-stu-id="abf19-145">In **Solution Explorer**, right-click the project, select **Add | Class**.</span></span> <span data-ttu-id="abf19-146">Benennen Sie die neue Klasse **ChatHub**.</span><span class="sxs-lookup"><span data-stu-id="abf19-146">Name the new class **ChatHub**.</span></span>
4. <span data-ttu-id="abf19-147">In **Projektmappen-Explorer** erweitern Sie den Knoten des Skripts.</span><span class="sxs-lookup"><span data-stu-id="abf19-147">In **Solution Explorer** expand the Scripts node.</span></span> <span data-ttu-id="abf19-148">Skriptbibliotheken für jQuery und SignalR werden in das Projekt angezeigt.</span><span class="sxs-lookup"><span data-stu-id="abf19-148">Script libraries for jQuery and SignalR are visible in the project.</span></span>

    ![Verweise auf](tutorial-getting-started-with-signalr/_static/image3.png)
5. <span data-ttu-id="abf19-150">Ersetzen Sie den Code in der **ChatHub** Klasse durch den folgenden Code.</span><span class="sxs-lookup"><span data-stu-id="abf19-150">Replace the code in the **ChatHub** class with the following code.</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample1.cs)]
6. <span data-ttu-id="abf19-151">In **Projektmappen-Explorer**Sie mit der rechten Maustaste des Projekts, und klicken Sie auf **hinzufügen | Neues Element**.</span><span class="sxs-lookup"><span data-stu-id="abf19-151">In **Solution Explorer**, right-click the project, then click **Add | New Item**.</span></span> <span data-ttu-id="abf19-152">In der **neues Element hinzufügen** wählen Sie im Dialogfeld **Globale Anwendungsklasse** , und klicken Sie auf **hinzufügen**.</span><span class="sxs-lookup"><span data-stu-id="abf19-152">In the **Add New Item** dialog, select **Global Application Class** and click **Add**.</span></span>

    ![Globale hinzufügen](tutorial-getting-started-with-signalr/_static/image4.png)
7. <span data-ttu-id="abf19-154">Fügen Sie die folgenden `using` Anweisungen nach der bereitgestellten `using` Anweisungen in der Global.asax.cs-Klasse.</span><span class="sxs-lookup"><span data-stu-id="abf19-154">Add the following `using` statements after the provided `using` statements in the Global.asax.cs class.</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample2.cs)]
8. <span data-ttu-id="abf19-155">Fügen Sie die folgende Codezeile in der `Application_Start` -Methode der globalen Klasse um die Standardroute für SignalR-Hubs zu registrieren.</span><span class="sxs-lookup"><span data-stu-id="abf19-155">Add the following line of code in the `Application_Start` method of the Global class to register the default route for SignalR hubs.</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample3.cs)]
9. <span data-ttu-id="abf19-156">In **Projektmappen-Explorer**Sie mit der rechten Maustaste des Projekts, und klicken Sie auf **hinzufügen | Neues Element**.</span><span class="sxs-lookup"><span data-stu-id="abf19-156">In **Solution Explorer**, right-click the project, then click **Add | New Item**.</span></span> <span data-ttu-id="abf19-157">In der **neues Element hinzufügen** Dialogfeld, wählen Sie Html-Seite, und klicken Sie auf **hinzufügen**.</span><span class="sxs-lookup"><span data-stu-id="abf19-157">In the **Add New Item** dialog, select Html Page and click **Add**.</span></span>
10. <span data-ttu-id="abf19-158">In **Projektmappen-Explorer**mit der rechten Maustaste auf das HTML-Seite, die Sie gerade erstellt haben, und klicken Sie auf **als Startseite festlegen**.</span><span class="sxs-lookup"><span data-stu-id="abf19-158">In **Solution Explorer**, right-click the HTML page you just created and click **Set as Start Page**.</span></span>
11. <span data-ttu-id="abf19-159">Ersetzen Sie den Standardcode in der HTML-Seite mit den folgenden Code ein.</span><span class="sxs-lookup"><span data-stu-id="abf19-159">Replace the default code in the HTML page with the following code.</span></span>

    [!code-html[Main](tutorial-getting-started-with-signalr/samples/sample4.html)]
12. <span data-ttu-id="abf19-160">**Speichern Sie alle** für das Projekt.</span><span class="sxs-lookup"><span data-stu-id="abf19-160">**Save All** for the project.</span></span>

<a id="run"></a>

## <a name="run-the-sample"></a><span data-ttu-id="abf19-161">Führen Sie das Beispiel</span><span class="sxs-lookup"><span data-stu-id="abf19-161">Run the Sample</span></span>

1. <span data-ttu-id="abf19-162">Drücken Sie F5, um das Projekt im Debugmodus auszuführen.</span><span class="sxs-lookup"><span data-stu-id="abf19-162">Press F5 to run the project in debug mode.</span></span> <span data-ttu-id="abf19-163">Die HTML-Seite lädt in einen Browserinstanz und fordert Sie auf einen Benutzernamen ein.</span><span class="sxs-lookup"><span data-stu-id="abf19-163">The HTML page loads in a browser instance and prompts for a user name.</span></span>

    ![Benutzername eingeben](tutorial-getting-started-with-signalr/_static/image5.png)
2. <span data-ttu-id="abf19-165">Geben Sie einen Benutzernamen ein.</span><span class="sxs-lookup"><span data-stu-id="abf19-165">Enter a user name.</span></span>
3. <span data-ttu-id="abf19-166">Kopieren Sie die URL aus der Adresszeile des Browsers ein, und verwenden Sie, um zwei weitere Browserinstanzen öffnen.</span><span class="sxs-lookup"><span data-stu-id="abf19-166">Copy the URL from the address line of the browser and use it to open two more browser instances.</span></span> <span data-ttu-id="abf19-167">Geben Sie in den einzelnen Instanzen Browser einen eindeutigen Benutzernamen aus.</span><span class="sxs-lookup"><span data-stu-id="abf19-167">In each browser instance, enter a unique user name.</span></span>
4. <span data-ttu-id="abf19-168">Klicken Sie in den einzelnen Instanzen Browser einen Kommentar hinzufügen, und klicken Sie auf **senden**.</span><span class="sxs-lookup"><span data-stu-id="abf19-168">In each browser instance, add a comment and click **Send**.</span></span> <span data-ttu-id="abf19-169">Die Kommentare sollten in alle Browserinstanzen angezeigt werden.</span><span class="sxs-lookup"><span data-stu-id="abf19-169">The comments should display in all browser instances.</span></span>

    > [!NOTE]
    > <span data-ttu-id="abf19-170">Diese einfache chatanwendung behält den Kontext der Diskussion auf dem Server.</span><span class="sxs-lookup"><span data-stu-id="abf19-170">This simple chat application does not maintain the discussion context on the server.</span></span> <span data-ttu-id="abf19-171">Der Hub, Kommentare, um alle aktuellen Benutzer sendet.</span><span class="sxs-lookup"><span data-stu-id="abf19-171">The hub broadcasts comments to all current users.</span></span> <span data-ttu-id="abf19-172">Benutzer, die den Chat später beizutreten sehen, dass Nachrichten ab dem Zeitpunkt hinzugefügt werden, dass sie verknüpfen.</span><span class="sxs-lookup"><span data-stu-id="abf19-172">Users who join the chat later will see messages added from the time they join.</span></span>

    <span data-ttu-id="abf19-173">Der folgende Screenshot zeigt die chatanwendung, die in drei Browserinstanzen, die aktualisiert werden, wenn eine Instanz eine Nachricht sendet ausgeführt wird:</span><span class="sxs-lookup"><span data-stu-id="abf19-173">The following screen shot shows the chat application running in three browser instances, all of which are updated when one instance sends a message:</span></span>

    ![Chat-Browser](tutorial-getting-started-with-signalr/_static/image6.png)
5. <span data-ttu-id="abf19-175">In **Projektmappen-Explorer**, überprüfen Sie die **Skriptdokumente** Knoten für die ausgeführte Anwendung.</span><span class="sxs-lookup"><span data-stu-id="abf19-175">In **Solution Explorer**, inspect the **Script Documents** node for the running application.</span></span> <span data-ttu-id="abf19-176">Es wird eine Skriptdatei mit dem Namen **Hubs** , die die SignalR-Bibliothek wird zur Laufzeit dynamisch generiert.</span><span class="sxs-lookup"><span data-stu-id="abf19-176">There is a script file named **hubs** that the SignalR library dynamically generates at runtime.</span></span> <span data-ttu-id="abf19-177">Diese Datei wird die Kommunikation zwischen jQuery-Skripts und serverseitigen Code verwaltet.</span><span class="sxs-lookup"><span data-stu-id="abf19-177">This file manages the communication between jQuery script and server-side code.</span></span>

    ![Generierte Hub-Skript](tutorial-getting-started-with-signalr/_static/image7.png)

<a id="code"></a>

## <a name="examine-the-code"></a><span data-ttu-id="abf19-179">Überprüfen Sie den Code</span><span class="sxs-lookup"><span data-stu-id="abf19-179">Examine the Code</span></span>

<span data-ttu-id="abf19-180">Die SignalR-Chat-Anwendung zeigt zwei grundlegende SignalR Entwicklungsaufgaben: Erstellen eines Hubs wie das Hauptfenster Koordinierung-Objekt, auf dem Server und mithilfe der SignalR-jQuery-Bibliothek zum Senden und Empfangen von Nachrichten.</span><span class="sxs-lookup"><span data-stu-id="abf19-180">The SignalR chat application demonstrates two basic SignalR development tasks: creating a hub as the main coordination object on the server, and using the SignalR jQuery library to send and receive messages.</span></span>

### <a name="signalr-hubs"></a><span data-ttu-id="abf19-181">SignalR Hubs</span><span class="sxs-lookup"><span data-stu-id="abf19-181">SignalR Hubs</span></span>

<span data-ttu-id="abf19-182">Im Codebeispiel den **ChatHub** Klasse leitet sich von der **Microsoft.AspNet.SignalR.Hub** Klasse.</span><span class="sxs-lookup"><span data-stu-id="abf19-182">In the code sample the **ChatHub** class derives from the **Microsoft.AspNet.SignalR.Hub** class.</span></span> <span data-ttu-id="abf19-183">Ableiten von der **Hub** Klasse ist eine hilfreiche Möglichkeit zum Erstellen einer SignalR-Anwendung.</span><span class="sxs-lookup"><span data-stu-id="abf19-183">Deriving from the **Hub** class is a useful way to build a SignalR application.</span></span> <span data-ttu-id="abf19-184">Sie können öffentliche Methoden in der hubklasse erstellen und dann Zugriff auf diese Methoden durch Aufruf aus jQuery-Skripts auf einer Webseite.</span><span class="sxs-lookup"><span data-stu-id="abf19-184">You can create public methods on your hub class and then access those methods by calling them from jQuery scripts in a web page.</span></span>

<span data-ttu-id="abf19-185">Rufen Sie in den Code Chat Clients die **ChatHub.Send** Methode, um eine neue Nachricht zu senden.</span><span class="sxs-lookup"><span data-stu-id="abf19-185">In the chat code, clients call the **ChatHub.Send** method to send a new message.</span></span> <span data-ttu-id="abf19-186">Der Hub sendet wiederum die Nachricht an alle Clients durch den Aufruf **Clients.All.broadcastMessage**.</span><span class="sxs-lookup"><span data-stu-id="abf19-186">The hub in turn sends the message to all clients by calling **Clients.All.broadcastMessage**.</span></span>

<span data-ttu-id="abf19-187">Die **senden** einige Konzepte der Hub-Methode veranschaulicht:</span><span class="sxs-lookup"><span data-stu-id="abf19-187">The **Send** method demonstrates several hub concepts :</span></span>

- <span data-ttu-id="abf19-188">Öffentliche Methoden für einen Hub deklariert werden, damit Clients, die sie aufrufen können.</span><span class="sxs-lookup"><span data-stu-id="abf19-188">Declare public methods on a hub so that clients can call them.</span></span>
- <span data-ttu-id="abf19-189">Verwenden der **Microsoft.AspNet.SignalR.Hub.Clients** dynamische Eigenschaft auf alle Clients, die mit diesem Hub verbunden sind.</span><span class="sxs-lookup"><span data-stu-id="abf19-189">Use the **Microsoft.AspNet.SignalR.Hub.Clients** dynamic property to access all clients connected to this hub.</span></span>
- <span data-ttu-id="abf19-190">Aufrufen einer jQuery-Funktion auf dem Client (z. B. die `broadcastMessage` Funktion) so aktualisieren Sie Clients.</span><span class="sxs-lookup"><span data-stu-id="abf19-190">Call a jQuery function on the client (such as the `broadcastMessage` function) to update clients.</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample5.cs)]

### <a name="signalr-and-jquery"></a><span data-ttu-id="abf19-191">SignalR und jQuery</span><span class="sxs-lookup"><span data-stu-id="abf19-191">SignalR and jQuery</span></span>

<span data-ttu-id="abf19-192">Die HTML-Seite im Codebeispiel zeigt, wie die SignalR-jQuery-Bibliothek für die Kommunikation mit SignalR-Hubs.</span><span class="sxs-lookup"><span data-stu-id="abf19-192">The HTML page in the code sample shows how to use the SignalR jQuery library to communicate with a SignalR hub.</span></span> <span data-ttu-id="abf19-193">Die Durchführung wichtiger Aufgaben im Code deklarieren ein Proxys für den Hub, Deklarieren einer Funktion, die der Server auf Push-Inhalte für Clients aufrufen können, und starten eine Verbindung zum Senden von Nachrichten an den Hub zu verweisen.</span><span class="sxs-lookup"><span data-stu-id="abf19-193">The essential tasks in the code are declaring a proxy to reference the hub, declaring a function that the server can call to push content to clients, and starting a connection to send messages to the hub.</span></span>

<span data-ttu-id="abf19-194">Der folgende Code deklariert einen Proxy für einen Hub.</span><span class="sxs-lookup"><span data-stu-id="abf19-194">The following code declares a proxy for a hub.</span></span>

[!code-javascript[Main](tutorial-getting-started-with-signalr/samples/sample6.js)]

> [!NOTE]
> <span data-ttu-id="abf19-195">In jQuery wird der Verweis auf die Server-Klasse und ihre Member in Camel-Case.</span><span class="sxs-lookup"><span data-stu-id="abf19-195">In jQuery the reference to the server class and its members is in camel case.</span></span> <span data-ttu-id="abf19-196">Im Codebeispiel verweist auf die C#- **ChatHub** Klasse in jQuery als **ChatHub**.</span><span class="sxs-lookup"><span data-stu-id="abf19-196">The code sample references the C# **ChatHub** class in jQuery as **chatHub**.</span></span>


<span data-ttu-id="abf19-197">Der folgende Code stammt, wie Sie eine Rückruffunktion in das Skript erstellen.</span><span class="sxs-lookup"><span data-stu-id="abf19-197">The following code is how you create a callback function in the script.</span></span> <span data-ttu-id="abf19-198">Die hubklasse, auf dem Server ruft diese Funktion um Inhaltsupdates an jeden Client per Push übertragen.</span><span class="sxs-lookup"><span data-stu-id="abf19-198">The hub class on the server calls this function to push content updates to each client.</span></span> <span data-ttu-id="abf19-199">Die beiden Zeilen an, dass HTML-Codierung den Inhalt vor der Anzeige sind optional, und zeigen eine einfache Möglichkeit zum Skript-Injection verhindern.</span><span class="sxs-lookup"><span data-stu-id="abf19-199">The two lines that HTML encode the content before displaying it are optional and show a simple way to prevent script injection.</span></span>

[!code-html[Main](tutorial-getting-started-with-signalr/samples/sample7.html)]

<span data-ttu-id="abf19-200">Der folgende Code zeigt, wie Sie eine Verbindung mit dem Hub zu öffnen.</span><span class="sxs-lookup"><span data-stu-id="abf19-200">The following code shows how to open a connection with the hub.</span></span> <span data-ttu-id="abf19-201">Der Code wird die Verbindung gestartet und übergibt diese eine Funktion, um das Click-Ereignis zu behandeln, auf die **senden** Schaltfläche in der HTML-Seite.</span><span class="sxs-lookup"><span data-stu-id="abf19-201">The code starts the connection and then passes it a function to handle the click event on the **Send** button in the HTML page.</span></span>

> [!NOTE]
> <span data-ttu-id="abf19-202">Dieser Ansatz wird sichergestellt, dass die Verbindung hergestellt wird, bevor der Ereignishandler ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="abf19-202">This approach insures that the connection is established before the event handler executes.</span></span>


[!code-javascript[Main](tutorial-getting-started-with-signalr/samples/sample8.js)]

<a id="next"></a>

## <a name="next-steps"></a><span data-ttu-id="abf19-203">Nächste Schritte</span><span class="sxs-lookup"><span data-stu-id="abf19-203">Next Steps</span></span>

<span data-ttu-id="abf19-204">Sie erfahren, dass SignalR ein Framework zur Erstellung von Echtzeit-Webanwendungen.</span><span class="sxs-lookup"><span data-stu-id="abf19-204">You learned that SignalR is a framework for building real-time web applications.</span></span> <span data-ttu-id="abf19-205">Sie haben auch erfahren, mehrere SignalR Entwicklungsaufgaben: zum Hinzufügen von SignalR an eine ASP.NET-Anwendung zum Erstellen einer hubklasse und zum Senden und Empfangen von Nachrichten über den Hub.</span><span class="sxs-lookup"><span data-stu-id="abf19-205">You also learned several SignalR development tasks: how to add SignalR to an ASP.NET application, how to create a hub class, and how to send and receive messages from the hub.</span></span>

<span data-ttu-id="abf19-206">Sie können die beispielanwendung in diesem Lernprogramm oder andere SignalR-Anwendungen verfügbar über das Internet, indem sie mit einem Hostinganbieter bereitstellen.</span><span class="sxs-lookup"><span data-stu-id="abf19-206">You can make the sample application in this tutorial or other SignalR applications available over the Internet by deploying them to a hosting provider.</span></span> <span data-ttu-id="abf19-207">Microsoft bietet kostenlose Webhosting für bis zu 10 Websites im ein kostenloses [Windows Azure-Testkonto](https://www.windowsazure.com/pricing/free-trial/?WT.mc_id=A443DD604).</span><span class="sxs-lookup"><span data-stu-id="abf19-207">Microsoft offers free web hosting for up to 10 web sites in a free [Windows Azure trial account](https://www.windowsazure.com/pricing/free-trial/?WT.mc_id=A443DD604).</span></span> <span data-ttu-id="abf19-208">Eine exemplarische Vorgehensweise zum Bereitstellen der SignalR-beispielanwendung finden Sie unter [SignalR erste Schritte Beispiel als ein Windows Azure-Website veröffentlichen](https://blogs.msdn.com/b/timlee/archive/2013/02/27/deploy-the-signalr-getting-started-sample-as-a-windows-azure-web-site.aspx).</span><span class="sxs-lookup"><span data-stu-id="abf19-208">For a walkthrough on how to deploy the sample SignalR application, see [Publish the SignalR Getting Started Sample as a Windows Azure Web Site](https://blogs.msdn.com/b/timlee/archive/2013/02/27/deploy-the-signalr-getting-started-sample-as-a-windows-azure-web-site.aspx).</span></span> <span data-ttu-id="abf19-209">Ausführliche Informationen dazu, wie Sie ein Visual Studio-Webprojekt auf einer Windows Azure-Website bereitstellen, finden Sie unter [Bereitstellung einer ASP.NET-Anwendung auf einem Windows Azure-Website](https://docs.microsoft.com/azure/app-service-web/app-service-web-get-started-dotnet).</span><span class="sxs-lookup"><span data-stu-id="abf19-209">For detailed information about how to deploy a Visual Studio web project to a Windows Azure Web Site, see [Deploying an ASP.NET Application to a Windows Azure Web Site](https://docs.microsoft.com/azure/app-service-web/app-service-web-get-started-dotnet).</span></span> <span data-ttu-id="abf19-210">(Hinweis: die WebSocket-Transport wird derzeit nicht unterstützt für Windows Azure-Websites.</span><span class="sxs-lookup"><span data-stu-id="abf19-210">(Note: The WebSocket transport is not currently supported for Windows Azure Web Sites.</span></span> <span data-ttu-id="abf19-211">Bei der WebSocket-Transport ist nicht verfügbar, SignalR verwendet die andere verfügbare Transporte wie beschrieben im Abschnitt Transporte der [Einführung zu SignalR Thema](index.md).)</span><span class="sxs-lookup"><span data-stu-id="abf19-211">When WebSocket transport is not available, SignalR uses the other available transports as described in the Transports section of the [Introduction to SignalR topic](index.md).)</span></span>

<span data-ttu-id="abf19-212">Erweiterte Konzepte von SignalR-Entwicklungen finden Sie auf den folgenden Websites für SignalR-Quellcode und Ressourcen:</span><span class="sxs-lookup"><span data-stu-id="abf19-212">To learn more advanced SignalR developments concepts, visit the following sites for SignalR source code and resources:</span></span>

- [<span data-ttu-id="abf19-213">SignalR-Projekt</span><span class="sxs-lookup"><span data-stu-id="abf19-213">SignalR Project</span></span>](http://signalr.net)
- [<span data-ttu-id="abf19-214">SignalR Github und Beispiele</span><span class="sxs-lookup"><span data-stu-id="abf19-214">SignalR Github and Samples</span></span>](https://github.com/SignalR/SignalR)
- [<span data-ttu-id="abf19-215">SignalR Wiki</span><span class="sxs-lookup"><span data-stu-id="abf19-215">SignalR Wiki</span></span>](https://github.com/SignalR/SignalR/wiki)
