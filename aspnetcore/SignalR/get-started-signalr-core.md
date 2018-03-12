---
title: "Erste Schritte mit SignalR für ASP.NET Core"
author: rachelappel
ms.author: rachelap
description: "In diesem Lernprogramm erstellen Sie eine app mit SignalR für ASP.NET Core."
manager: wpickett
ms.date: 03/06/2018
ms.topic: tutorial
ms.technology: dotnet-signalr
ms.prod: aspnet-core
ms.custom: mvc
uid: signalr/get-started-signalr-core
ms.openlocfilehash: 4afb9785fc3d0f472226a745537acbc77adefb4c
ms.sourcegitcommit: 9622bdc6326c28c3322c70000468a80ef21ad376
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/12/2018
---
# <a name="tutorial-get-started-with-signalr-for-aspnet-core"></a><span data-ttu-id="989d9-103">Tutorial: Erste Schritte mit SignalR für ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="989d9-103">Tutorial: Get started with SignalR for ASP.NET Core</span></span>

<span data-ttu-id="989d9-104">Von [Rachel Appel](https://twitter.com/rachelappel)</span><span class="sxs-lookup"><span data-stu-id="989d9-104">By [Rachel Appel](https://twitter.com/rachelappel)</span></span>

<span data-ttu-id="989d9-105">Dieses Lernprogramm zeigt die Grundlagen der Erstellung einer Echtzeit-app mit SignalR für ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="989d9-105">This tutorial teaches the basics of building a real-time app using SignalR for ASP.NET Core.</span></span>

   ![Lösung](get-started-signalr-core/_static/signalr-get-started-finished.png)

<span data-ttu-id="989d9-107">[Anzeigen oder Herunterladen von Beispielcode](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/get-started-signalr-core/sample/) ([Vorgehensweise zum Herunterladen](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="989d9-107">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/get-started-signalr-core/sample/) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="989d9-108">Dieses Lernprogramm veranschaulicht die folgenden SignalR Entwicklungsaufgaben:</span><span class="sxs-lookup"><span data-stu-id="989d9-108">This tutorial demonstrates the following SignalR development tasks:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="989d9-109">Erstellen Sie eine ASP.NET Core-Web-app.</span><span class="sxs-lookup"><span data-stu-id="989d9-109">Create an ASP.NET Core web app.</span></span>
> * <span data-ttu-id="989d9-110">Erstellen eines SignalR-Hubs, um Inhalt an Clients zu verteilen.</span><span class="sxs-lookup"><span data-stu-id="989d9-110">Create a SignalR hub to push content to clients.</span></span>
> * <span data-ttu-id="989d9-111">Verwenden Sie die SignalR JavaScript-Bibliothek zum Senden von Nachrichten und Updates vom Hub angezeigt.</span><span class="sxs-lookup"><span data-stu-id="989d9-111">Use the SignalR JavaScript library to send messages and display updates from the hub.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="989d9-112">Erforderliche Komponenten</span><span class="sxs-lookup"><span data-stu-id="989d9-112">Prerequisites</span></span>

<span data-ttu-id="989d9-113">Installieren Sie die folgende Software:</span><span class="sxs-lookup"><span data-stu-id="989d9-113">Install the following software:</span></span>

* <span data-ttu-id="989d9-114">[.NET Core 2.1.0 Vorschau 1 SDK](https://www.microsoft.com/net/download/dotnet-core/sdk-2.1.300-preview1) oder höher</span><span class="sxs-lookup"><span data-stu-id="989d9-114">[.NET Core 2.1.0 Preview 1 SDK](https://www.microsoft.com/net/download/dotnet-core/sdk-2.1.300-preview1) or later</span></span>
* <span data-ttu-id="989d9-115">[Visual Studio-2017](https://www.visualstudio.com/downloads/) Version 15.6 oder höher, mit der arbeitsauslastung für ASP.NET und Entwicklung</span><span class="sxs-lookup"><span data-stu-id="989d9-115">[Visual Studio 2017](https://www.visualstudio.com/downloads/) version 15.6 or later with the ASP.NET and web development workload</span></span>
* [<span data-ttu-id="989d9-116">npm</span><span class="sxs-lookup"><span data-stu-id="989d9-116">npm</span></span>](https://www.npmjs.com/get-npm)

## <a name="create-an-aspnet-core-project-that-hosts-signalr-client-and-server"></a><span data-ttu-id="989d9-117">Erstellen Sie ein Projekt von ASP.NET Core, die SignalR-Client und Server hostet</span><span class="sxs-lookup"><span data-stu-id="989d9-117">Create an ASP.NET Core project that hosts SignalR client and server</span></span>

1. <span data-ttu-id="989d9-118">Verwenden der **Datei** > **neues Projekt** Menü aus, und wählen Sie **ASP.NET-Webanwendung für Core**.</span><span class="sxs-lookup"><span data-stu-id="989d9-118">Use the **File** > **New Project** menu option and choose **ASP.NET Core Web Application**.</span></span> <span data-ttu-id="989d9-119">Benennen Sie das Projekt mit `SignalRChat`.</span><span class="sxs-lookup"><span data-stu-id="989d9-119">Name the project `SignalRChat`.</span></span>

  ![Dialogfeld "Neues Projekt" in Visual Studio](get-started-signalr-core/_static/signalr-new-project-dialog.png)

2. <span data-ttu-id="989d9-121">Wählen Sie **Webanwendung** zum Erstellen eines Projekts mithilfe von Razor-Seiten.</span><span class="sxs-lookup"><span data-stu-id="989d9-121">Select **Web Application** to create a project using Razor Pages.</span></span> <span data-ttu-id="989d9-122">Wählen Sie dann **Ok**.</span><span class="sxs-lookup"><span data-stu-id="989d9-122">Then select **Ok**.</span></span> <span data-ttu-id="989d9-123">Achten Sie darauf, **ASP.NET Core 2.1** aus dem Framework Selektor ausgewählt ist, obwohl SignalR unter älteren Versionen von .NET ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="989d9-123">Be sure that **ASP.NET Core 2.1** is selected from the framework selector, though SignalR runs on older versions of .NET.</span></span>

  ![Dialogfeld "Neues Projekt" in Visual Studio](get-started-signalr-core/_static/signalr-new-project-choose-type.png)

  <span data-ttu-id="989d9-125">Die Bibliotheken, die SignalR serverseitigen Code hosten, sind in der Projektvorlage enthalten.</span><span class="sxs-lookup"><span data-stu-id="989d9-125">The libraries that host SignalR server-side code are included in the project template.</span></span> <span data-ttu-id="989d9-126">Installieren Sie die clientseitiges JavaScript separat mit [Npm](https://www.npmjs.com/).</span><span class="sxs-lookup"><span data-stu-id="989d9-126">Install the client-side JavaScript separately with [npm](https://www.npmjs.com/).</span></span>

  ```console
   npm install @aspnet/signalr
  ```

3. <span data-ttu-id="989d9-127">Kopieren der *signalr.js* aus *Node_modules\\ @aspnet\signalr\dist\browser*  auf die *Wwwroot\lib* Ordner des Projekts.</span><span class="sxs-lookup"><span data-stu-id="989d9-127">Copy the *signalr.js* from *node_modules\\@aspnet\signalr\dist\browser* to the *wwwroot\lib* folder in your project.</span></span>

## <a name="create-the-signalr-hub"></a><span data-ttu-id="989d9-128">Erstellen von SignalR-Hubs.</span><span class="sxs-lookup"><span data-stu-id="989d9-128">Create the SignalR Hub</span></span>

<span data-ttu-id="989d9-129">Ein Hub ist eine Klasse, die als eine allgemeine Pipeline dient, die zum Aufrufen von Methoden voneinander Client und Server ermöglicht.</span><span class="sxs-lookup"><span data-stu-id="989d9-129">A hub is a class that serves as a high-level pipeline that allows the client and server to call methods on each other.</span></span>

1. <span data-ttu-id="989d9-130">Fügen Sie eine Klasse zum Projekt durch Auswahl **Datei** > **neu** > **Datei** auswählen und **Visual C#-Klasse**.</span><span class="sxs-lookup"><span data-stu-id="989d9-130">Add a class to the project by choosing **File** > **New** > **File** and selecting **Visual C# Class**.</span></span> 

1. <span data-ttu-id="989d9-131">Erben von `Microsoft.AspNetCore.SignalR.Hub`.</span><span class="sxs-lookup"><span data-stu-id="989d9-131">Inherit from `Microsoft.AspNetCore.SignalR.Hub`.</span></span> <span data-ttu-id="989d9-132">Die `Hub` Klasse enthält Eigenschaften und Ereignisse für die Verwaltung von Verbindungen und Gruppen sowie senden und Empfangen von Daten.</span><span class="sxs-lookup"><span data-stu-id="989d9-132">The `Hub` class contains properties and events for managing connections and groups, as well as sending and receiving data.</span></span>

1. <span data-ttu-id="989d9-133">Erstellen der `Send` -Methode, die eine Nachricht an alle verbundenen Chat-Clients sendet.</span><span class="sxs-lookup"><span data-stu-id="989d9-133">Create the `Send` method that sends a message to all connected chat clients.</span></span> <span data-ttu-id="989d9-134">Beachten Sie, es gibt eine `Task`, da SignalR asynchron ist.</span><span class="sxs-lookup"><span data-stu-id="989d9-134">Notice it returns a `Task`, because SignalR is asynchronous.</span></span> <span data-ttu-id="989d9-135">Asynchronen Code skaliert besser.</span><span class="sxs-lookup"><span data-stu-id="989d9-135">Asynchronous code scales better.</span></span>

  [!code-csharp[Startup](get-started-signalr-core/sample/Hubs/ChatHub.cs?range=7-14)]

## <a name="configure-the-project-to-use-signalr"></a><span data-ttu-id="989d9-136">Konfigurieren Sie das Projekt zur Verwendung von SignalR</span><span class="sxs-lookup"><span data-stu-id="989d9-136">Configure the project to use SignalR</span></span>

<span data-ttu-id="989d9-137">Die SignalR-Server muss konfiguriert werden, damit dieser weiß, dass Anforderungen an SignalR übergeben.</span><span class="sxs-lookup"><span data-stu-id="989d9-137">The SignalR server must be configured so that it knows to pass requests to SignalR.</span></span>

1. <span data-ttu-id="989d9-138">Ändern Sie zum Konfigurieren einer SignalR-Projekts die `ConfigureServices` Methode von der Anwendungsverzeichnis `Startup` Klasse durch Einfügen eines Aufrufs in `services.AddSignalR`.</span><span class="sxs-lookup"><span data-stu-id="989d9-138">To configure a SignalR project, modify the `ConfigureServices` method of the application's `Startup` class by inserting a call to `services.AddSignalR`.</span></span>

  <span data-ttu-id="989d9-139">`services.AddSignalR` Fügt der SignalR als Teil der [ASP.NET Core Middleware](xref:fundamentals/middleware/index) Pipeline.</span><span class="sxs-lookup"><span data-stu-id="989d9-139">`services.AddSignalR` adds SignalR as part of the [ASP.NET Core middleware](xref:fundamentals/middleware/index) pipeline.</span></span>

1. <span data-ttu-id="989d9-140">Konfigurieren von Routen zu Ihrer Verwendung Hubs `UseSignalR`.</span><span class="sxs-lookup"><span data-stu-id="989d9-140">Configure routes to your hubs using `UseSignalR`.</span></span>

  [!code-csharp[Startup](get-started-signalr-core/sample/Startup.cs?highlight=22,40-43)]

## <a name="create-the-signalr-client-code"></a><span data-ttu-id="989d9-141">Erstellen Sie den SignalR-Clientcode</span><span class="sxs-lookup"><span data-stu-id="989d9-141">Create the SignalR client code</span></span>

1. <span data-ttu-id="989d9-142">Ersetzen Sie den Inhalt in *Pages\Index.cshtml* durch den folgenden Code:</span><span class="sxs-lookup"><span data-stu-id="989d9-142">Replace the content in *Pages\Index.cshtml* with the following code:</span></span>

  [!code-cshtml[Index](get-started-signalr-core/sample/Pages/Index.cshtml)]

  <span data-ttu-id="989d9-143">Die vorangehende HTML zeigt Name und Nachrichtenfelder sowie eine Schaltfläche "Absenden".</span><span class="sxs-lookup"><span data-stu-id="989d9-143">The preceding HTML displays name and message fields, and a submit button.</span></span> <span data-ttu-id="989d9-144">Beachten Sie die Skriptverweise im unteren Bereich: ein Verweis auf SignalR und *chat.js*.</span><span class="sxs-lookup"><span data-stu-id="989d9-144">Notice the script references at the bottom: a reference to SignalR and *chat.js*.</span></span>

1. <span data-ttu-id="989d9-145">Fügen Sie eine JavaScript-Datei, um die *Wwwroot\js* Ordner mit dem Namen *chat.js* und fügen Sie den folgenden Code hinzu:</span><span class="sxs-lookup"><span data-stu-id="989d9-145">Add a JavaScript file to the *wwwroot\js* folder named *chat.js* and add the following code to it:</span></span>

  [!code-javascript[Index](get-started-signalr-core/sample/wwwroot/js/chat.js)]

## <a name="run-the-app"></a><span data-ttu-id="989d9-146">Ausführen der App</span><span class="sxs-lookup"><span data-stu-id="989d9-146">Run the app</span></span>

1. <span data-ttu-id="989d9-147">Wählen Sie **Debuggen** > **Starten ohne Debuggen** starten Sie einen Browser, und laden die Website lokal.</span><span class="sxs-lookup"><span data-stu-id="989d9-147">Select **Debug** > **Start without debugging** to launch a browser and load the website locally.</span></span> <span data-ttu-id="989d9-148">Kopieren Sie die URL aus der Adressleiste ein.</span><span class="sxs-lookup"><span data-stu-id="989d9-148">Copy the URL from the address bar.</span></span>

1. <span data-ttu-id="989d9-149">Öffnen Sie eine andere Browserinstanz (alle Browser), und fügen Sie die URL in der Adressleiste.</span><span class="sxs-lookup"><span data-stu-id="989d9-149">Open another browser instance (any browser) and paste the URL in the address bar.</span></span>

1. <span data-ttu-id="989d9-150">Wählen Sie entweder Browser, geben Sie einen Namen und eine Nachricht, und klicken Sie auf die **senden** Schaltfläche.</span><span class="sxs-lookup"><span data-stu-id="989d9-150">Choose either browser, enter a name and message, and click the **Send** button.</span></span> <span data-ttu-id="989d9-151">Der Name und die Meldung werden sofort auf beiden Seiten angezeigt.</span><span class="sxs-lookup"><span data-stu-id="989d9-151">The name and message are displayed on both pages instantly.</span></span>

  ![Lösung](get-started-signalr-core/_static/signalr-get-started-finished.png)
