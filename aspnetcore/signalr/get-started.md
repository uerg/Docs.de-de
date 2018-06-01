---
title: Erste Schritte mit SignalR für ASP.NET Core
author: rachelappel
description: In diesem Lernprogramm erstellen Sie eine app mit SignalR für ASP.NET Core.
manager: wpickett
monikerRange: '>= aspnetcore-2.1'
ms.author: rachelap
ms.custom: mvc
ms.date: 05/22/2018
ms.prod: aspnet-core
ms.topic: tutorial
ms.technology: aspnet
uid: signalr/get-started
ms.openlocfilehash: eb14fbf42f5c18ccdc3ca42af8fd8bcfaa15c623
ms.sourcegitcommit: 545ff5a632e2281035c1becec1f99137298e4f5c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/31/2018
ms.locfileid: "34688587"
---
# <a name="get-started-with-signalr-on-aspnet-core"></a><span data-ttu-id="89d7e-103">Erste Schritte mit SignalR für ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="89d7e-103">Get started with SignalR on ASP.NET Core</span></span>

<span data-ttu-id="89d7e-104">Von [Rachel Appel](https://twitter.com/rachelappel)</span><span class="sxs-lookup"><span data-stu-id="89d7e-104">By [Rachel Appel](https://twitter.com/rachelappel)</span></span>

<span data-ttu-id="89d7e-105">Dieses Lernprogramm zeigt die Grundlagen der Erstellung einer Echtzeit-app mit SignalR für ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="89d7e-105">This tutorial teaches the basics of building a real-time app using SignalR for ASP.NET Core.</span></span>

   ![Lösung](get-started/_static/signalr-get-started-finished.png)

<span data-ttu-id="89d7e-107">Dieses Lernprogramm veranschaulicht die folgenden SignalR Entwicklungsaufgaben:</span><span class="sxs-lookup"><span data-stu-id="89d7e-107">This tutorial demonstrates the following SignalR development tasks:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="89d7e-108">Erstellen Sie eine SignalR für ASP.NET Core Web-app.</span><span class="sxs-lookup"><span data-stu-id="89d7e-108">Create a SignalR on ASP.NET Core web app.</span></span>
> * <span data-ttu-id="89d7e-109">Erstellen eines SignalR-Hubs, um Inhalt an Clients zu verteilen.</span><span class="sxs-lookup"><span data-stu-id="89d7e-109">Create a SignalR hub to push content to clients.</span></span>
> * <span data-ttu-id="89d7e-110">Ändern der `Startup` -Klasse und die app konfigurieren.</span><span class="sxs-lookup"><span data-stu-id="89d7e-110">Modify the `Startup` class and configure the app.</span></span>

<span data-ttu-id="89d7e-111">[Anzeigen oder Herunterladen von Beispielcode](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/get-started/sample/) ([Vorgehensweise zum Herunterladen](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="89d7e-111">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/get-started/sample/) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

# <a name="prerequisites"></a><span data-ttu-id="89d7e-112">Erforderliche Komponenten</span><span class="sxs-lookup"><span data-stu-id="89d7e-112">Prerequisites</span></span>

<span data-ttu-id="89d7e-113">Installieren Sie die folgende Software:</span><span class="sxs-lookup"><span data-stu-id="89d7e-113">Install the following software:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="89d7e-114">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="89d7e-114">Visual Studio</span></span>](#tab/visual-studio)

* [<span data-ttu-id="89d7e-115">.NET Core SDK 2.1 oder höher</span><span class="sxs-lookup"><span data-stu-id="89d7e-115">.NET Core SDK 2.1 or later</span></span>](https://www.microsoft.com/net/download/all)
* <span data-ttu-id="89d7e-116">[Visual Studio-2017](https://www.visualstudio.com/downloads/) 15.7 oder höher mit der **ASP.NET und zur Webentwicklung** arbeitsauslastung</span><span class="sxs-lookup"><span data-stu-id="89d7e-116">[Visual Studio 2017](https://www.visualstudio.com/downloads/) version 15.7 or later with the **ASP.NET and web development** workload</span></span>
* [<span data-ttu-id="89d7e-117">npm</span><span class="sxs-lookup"><span data-stu-id="89d7e-117">npm</span></span>](https://www.npmjs.com/get-npm)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="89d7e-118">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="89d7e-118">Visual Studio Code</span></span>](#tab/visual-studio-code)

* [<span data-ttu-id="89d7e-119">.NET Core SDK 2.1 oder höher</span><span class="sxs-lookup"><span data-stu-id="89d7e-119">.NET Core SDK 2.1 or later</span></span>](https://www.microsoft.com/net/download/all)
* [<span data-ttu-id="89d7e-120">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="89d7e-120">Visual Studio Code</span></span>](https://code.visualstudio.com/download)
* [<span data-ttu-id="89d7e-121">C# für Visual Studio-Code</span><span class="sxs-lookup"><span data-stu-id="89d7e-121">C# for Visual Studio Code</span></span>](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)
* [<span data-ttu-id="89d7e-122">npm</span><span class="sxs-lookup"><span data-stu-id="89d7e-122">npm</span></span>](https://www.npmjs.com/get-npm)

-----

## <a name="create-an-aspnet-core-project-that-hosts-signalr-client-and-server"></a><span data-ttu-id="89d7e-123">Erstellen Sie ein Projekt von ASP.NET Core, die SignalR-Client und Server hostet</span><span class="sxs-lookup"><span data-stu-id="89d7e-123">Create an ASP.NET Core project that hosts SignalR client and server</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="89d7e-124">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="89d7e-124">Visual Studio</span></span>](#tab/visual-studio/)

1. <span data-ttu-id="89d7e-125">Verwenden der **Datei** > **neues Projekt** Menü aus, und wählen Sie **ASP.NET-Webanwendung für Core**.</span><span class="sxs-lookup"><span data-stu-id="89d7e-125">Use the **File** > **New Project** menu option and choose **ASP.NET Core Web Application**.</span></span> <span data-ttu-id="89d7e-126">Nennen Sie das Projekt *SignalRChat*.</span><span class="sxs-lookup"><span data-stu-id="89d7e-126">Name the project *SignalRChat*.</span></span>

   ![Dialogfeld "Neues Projekt" in Visual Studio](get-started/_static/signalr-new-project-dialog.png)

2. <span data-ttu-id="89d7e-128">Wählen Sie **Webanwendung** zum Erstellen eines Projekts mithilfe von Razor-Seiten.</span><span class="sxs-lookup"><span data-stu-id="89d7e-128">Select **Web Application** to create a project using Razor Pages.</span></span> <span data-ttu-id="89d7e-129">Wählen Sie dann **OK**.</span><span class="sxs-lookup"><span data-stu-id="89d7e-129">Then select **OK**.</span></span> <span data-ttu-id="89d7e-130">Achten Sie darauf, **ASP.NET Core 2.1** aus dem Framework Selektor ausgewählt ist, obwohl SignalR unter älteren Versionen von .NET ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="89d7e-130">Be sure that **ASP.NET Core 2.1** is selected from the framework selector, though SignalR runs on older versions of .NET.</span></span>

   ![Dialogfeld "Neues Projekt" in Visual Studio](get-started/_static/signalr-new-project-choose-type.png)

<span data-ttu-id="89d7e-132">Visual Studio enthält die `Microsoft.AspNetCore.SignalR` -Paket mit der zugehörigen Serverbibliotheken als Teil seiner **ASP.NET-Webanwendung für Core** Vorlage.</span><span class="sxs-lookup"><span data-stu-id="89d7e-132">Visual Studio includes the `Microsoft.AspNetCore.SignalR` package containing its server libraries as part of its **ASP.NET Core Web Application** template.</span></span> <span data-ttu-id="89d7e-133">Allerdings die JavaScript-Clientbibliothek für SignalR muss installiert werden mit *Npm*.</span><span class="sxs-lookup"><span data-stu-id="89d7e-133">However, the JavaScript client library for SignalR must be installed using *npm*.</span></span>

3. <span data-ttu-id="89d7e-134">Führen die folgenden Befehle der **Package Manager Console** Fenster aus dem Projektstamm:</span><span class="sxs-lookup"><span data-stu-id="89d7e-134">Run the following commands in the **Package Manager Console** window, from the project root:</span></span>

    ```console
    npm init -y
    npm install @aspnet/signalr
    ```     

4. <span data-ttu-id="89d7e-135">Kopieren der *signalr.js* aus der Datei *Node_modules\\ @aspnet\signalr\dist\browser*  auf die *Lib* Ordner des Projekts.</span><span class="sxs-lookup"><span data-stu-id="89d7e-135">Copy the *signalr.js* file from *node_modules\\@aspnet\signalr\dist\browser* to the *lib* folder in your project.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="89d7e-136">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="89d7e-136">Visual Studio Code</span></span>](#tab/visual-studio-code/)

1. <span data-ttu-id="89d7e-137">Aus der **integrierte Terminaldienste**, führen Sie den folgenden Befehl:</span><span class="sxs-lookup"><span data-stu-id="89d7e-137">From the **Integrated Terminal**, run the following command:</span></span>

    ```console
    dotnet new webapp -o SignalRChat
    ```

2. <span data-ttu-id="89d7e-138">Installieren Sie die JavaScript-Client Library verwenden *Npm*.</span><span class="sxs-lookup"><span data-stu-id="89d7e-138">Install the JavaScript client library using *npm*.</span></span>

    ```console
    npm init -y
    npm install @aspnet/signalr
    ```

3. <span data-ttu-id="89d7e-139">Kopieren der *signalr.js* aus der Datei *Node_modules\\ @aspnet\signalr\dist\browser*  auf die *Lib* Ordner des Projekts.</span><span class="sxs-lookup"><span data-stu-id="89d7e-139">Copy the *signalr.js* file from *node_modules\\@aspnet\signalr\dist\browser* to the *lib* folder in your project.</span></span>

-----

## <a name="create-the-signalr-hub"></a><span data-ttu-id="89d7e-140">Erstellen von SignalR-Hubs.</span><span class="sxs-lookup"><span data-stu-id="89d7e-140">Create the SignalR Hub</span></span>

<span data-ttu-id="89d7e-141">Ein Hub ist eine Klasse, die als eine allgemeine Pipeline dient, die zum Aufrufen von Methoden voneinander Client und Server ermöglicht.</span><span class="sxs-lookup"><span data-stu-id="89d7e-141">A hub is a class that serves as a high-level pipeline that allows the client and server to call methods on each other.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="89d7e-142">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="89d7e-142">Visual Studio</span></span>](#tab/visual-studio/)

1. <span data-ttu-id="89d7e-143">Fügen Sie eine Klasse zum Projekt durch Auswahl **Datei** > **neu** > **Datei** auswählen und **Visual C#-Klasse**.</span><span class="sxs-lookup"><span data-stu-id="89d7e-143">Add a class to the project by choosing **File** > **New** > **File** and selecting **Visual C# Class**.</span></span>

2. <span data-ttu-id="89d7e-144">Erben von `Microsoft.AspNetCore.SignalR.Hub`.</span><span class="sxs-lookup"><span data-stu-id="89d7e-144">Inherit from `Microsoft.AspNetCore.SignalR.Hub`.</span></span> <span data-ttu-id="89d7e-145">Die `Hub` Klasse enthält Eigenschaften und Ereignisse für die Verwaltung von Verbindungen und Gruppen sowie senden und Empfangen von Daten.</span><span class="sxs-lookup"><span data-stu-id="89d7e-145">The `Hub` class contains properties and events for managing connections and groups, as well as sending and receiving data.</span></span>

3. <span data-ttu-id="89d7e-146">Erstellen der `SendMessage` -Methode, die eine Nachricht an alle verbundenen Chat-Clients sendet.</span><span class="sxs-lookup"><span data-stu-id="89d7e-146">Create the `SendMessage` method that sends a message to all connected chat clients.</span></span> <span data-ttu-id="89d7e-147">Beachten Sie, es gibt eine [Aufgabe](https://msdn.microsoft.com/library/system.threading.tasks.task(v=vs.110).aspx), da SignalR asynchron ist.</span><span class="sxs-lookup"><span data-stu-id="89d7e-147">Notice it returns a [Task](https://msdn.microsoft.com/library/system.threading.tasks.task(v=vs.110).aspx), because SignalR is asynchronous.</span></span> <span data-ttu-id="89d7e-148">Asynchronen Code skaliert besser.</span><span class="sxs-lookup"><span data-stu-id="89d7e-148">Asynchronous code scales better.</span></span>

   [!code-csharp[Startup](get-started/sample/Hubs/ChatHub.cs)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="89d7e-149">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="89d7e-149">Visual Studio Code</span></span>](#tab/visual-studio-code/)

1. <span data-ttu-id="89d7e-150">Öffnen der *SignalRChat* Ordner in Visual Studio-Code.</span><span class="sxs-lookup"><span data-stu-id="89d7e-150">Open the *SignalRChat* folder in Visual Studio Code.</span></span>

2. <span data-ttu-id="89d7e-151">Fügen Sie eine Klasse zum Projekt dazu **Datei** > **neue Datei** aus dem Menü.</span><span class="sxs-lookup"><span data-stu-id="89d7e-151">Add a class to the project by selecting **File** > **New File** from the menu.</span></span>

3. <span data-ttu-id="89d7e-152">Erben von `Microsoft.AspNetCore.SignalR.Hub`.</span><span class="sxs-lookup"><span data-stu-id="89d7e-152">Inherit from `Microsoft.AspNetCore.SignalR.Hub`.</span></span> <span data-ttu-id="89d7e-153">Die `Hub` Klasse enthält Eigenschaften und Ereignisse für die Verwaltung von Verbindungen und Gruppen sowie senden und Empfangen von Daten an Clients.</span><span class="sxs-lookup"><span data-stu-id="89d7e-153">The `Hub` class contains properties and events for managing connections and groups, as well as sending and receiving data to clients.</span></span>

4. <span data-ttu-id="89d7e-154">Fügen Sie der Klasse eine `SendMessage`-Method hinzu.</span><span class="sxs-lookup"><span data-stu-id="89d7e-154">Add a `SendMessage` method to the class.</span></span> <span data-ttu-id="89d7e-155">Die `SendMessage` Methode sendet eine Nachricht an alle verbundenen Chat-Clients.</span><span class="sxs-lookup"><span data-stu-id="89d7e-155">The `SendMessage` method sends a message to all connected chat clients.</span></span> <span data-ttu-id="89d7e-156">Beachten Sie, es gibt eine [Aufgabe](/dotnet/api/system.threading.tasks.task), da SignalR asynchron ist.</span><span class="sxs-lookup"><span data-stu-id="89d7e-156">Notice it returns a [Task](/dotnet/api/system.threading.tasks.task), because SignalR is asynchronous.</span></span> <span data-ttu-id="89d7e-157">Asynchronen Code skaliert besser.</span><span class="sxs-lookup"><span data-stu-id="89d7e-157">Asynchronous code scales better.</span></span>

   [!code-csharp[Startup](get-started/sample/Hubs/ChatHub.cs?range=6-12)]

-----

## <a name="configure-the-project-to-use-signalr"></a><span data-ttu-id="89d7e-158">Konfigurieren Sie das Projekt zur Verwendung von SignalR</span><span class="sxs-lookup"><span data-stu-id="89d7e-158">Configure the project to use SignalR</span></span>

<span data-ttu-id="89d7e-159">Die SignalR-Server muss konfiguriert werden, damit dieser weiß, dass Anforderungen an SignalR übergeben.</span><span class="sxs-lookup"><span data-stu-id="89d7e-159">The SignalR server must be configured so that it knows to pass requests to SignalR.</span></span>

1. <span data-ttu-id="89d7e-160">Ändern Sie des Projekts zum Konfigurieren einer SignalR-Projekts `Startup.ConfigureServices` Methode.</span><span class="sxs-lookup"><span data-stu-id="89d7e-160">To configure a SignalR project, modify the project's `Startup.ConfigureServices` method.</span></span>

   <span data-ttu-id="89d7e-161">`services.AddSignalR` Fügt der SignalR als Teil der [Middleware](xref:fundamentals/middleware/index) Pipeline.</span><span class="sxs-lookup"><span data-stu-id="89d7e-161">`services.AddSignalR` adds SignalR as part of the [middleware](xref:fundamentals/middleware/index) pipeline.</span></span>

2. <span data-ttu-id="89d7e-162">Konfigurieren von Routen zu Ihrer Verwendung Hubs `UseSignalR`.</span><span class="sxs-lookup"><span data-stu-id="89d7e-162">Configure routes to your hubs using `UseSignalR`.</span></span>


   [!code-csharp[Startup](get-started/sample/Startup.cs?highlight=37,57-60)]


## <a name="create-the-signalr-client-code"></a><span data-ttu-id="89d7e-163">Erstellen Sie den SignalR-Clientcode</span><span class="sxs-lookup"><span data-stu-id="89d7e-163">Create the SignalR client code</span></span>

1. <span data-ttu-id="89d7e-164">Ersetzen Sie den Inhalt in *Pages\Index.cshtml* durch den folgenden Code:</span><span class="sxs-lookup"><span data-stu-id="89d7e-164">Replace the content in *Pages\Index.cshtml* with the following code:</span></span>

   [!code-cshtml[Index](get-started/sample/Pages/Index.cshtml)]

   <span data-ttu-id="89d7e-165">Die vorangehende HTML zeigt Name und Nachrichtenfelder sowie eine Schaltfläche "Absenden".</span><span class="sxs-lookup"><span data-stu-id="89d7e-165">The preceding HTML displays name and message fields, and a submit button.</span></span> <span data-ttu-id="89d7e-166">Beachten Sie die Skriptverweise im unteren Bereich: ein Verweis auf SignalR und *chat.js*.</span><span class="sxs-lookup"><span data-stu-id="89d7e-166">Notice the script references at the bottom: a reference to SignalR and *chat.js*.</span></span>

2. <span data-ttu-id="89d7e-167">Fügen Sie eine JavaScript-Datei, mit dem Namen *chat.js*, zu der *Wwwroot\js* Ordner.</span><span class="sxs-lookup"><span data-stu-id="89d7e-167">Add a JavaScript file, named *chat.js*, to the *wwwroot\js* folder.</span></span> <span data-ttu-id="89d7e-168">Fügen Sie ihr folgenden Code hinzu:</span><span class="sxs-lookup"><span data-stu-id="89d7e-168">Add the following code to it:</span></span>

   [!code-javascript[Index](get-started/sample/wwwroot/js/chat.js)]

## <a name="run-the-app"></a><span data-ttu-id="89d7e-169">Ausführen der App</span><span class="sxs-lookup"><span data-stu-id="89d7e-169">Run the app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="89d7e-170">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="89d7e-170">Visual Studio</span></span>](#tab/visual-studio)

1. <span data-ttu-id="89d7e-171">Wählen Sie **Debuggen** > **Starten ohne Debuggen** starten Sie einen Browser, und laden die Website lokal.</span><span class="sxs-lookup"><span data-stu-id="89d7e-171">Select **Debug** > **Start without debugging** to launch a browser and load the website locally.</span></span> <span data-ttu-id="89d7e-172">Kopieren Sie die URL aus der Adressleiste ein.</span><span class="sxs-lookup"><span data-stu-id="89d7e-172">Copy the URL from the address bar.</span></span>

1. <span data-ttu-id="89d7e-173">Öffnen Sie eine andere Browserinstanz (alle Browser), und fügen Sie die URL in der Adressleiste.</span><span class="sxs-lookup"><span data-stu-id="89d7e-173">Open another browser instance (any browser) and paste the URL in the address bar.</span></span>

1. <span data-ttu-id="89d7e-174">Wählen Sie entweder Browser, geben Sie einen Namen und eine Nachricht, und klicken Sie auf die **senden** Schaltfläche.</span><span class="sxs-lookup"><span data-stu-id="89d7e-174">Choose either browser, enter a name and message, and click the **Send** button.</span></span> <span data-ttu-id="89d7e-175">Der Name und die Meldung werden sofort auf beiden Seiten angezeigt.</span><span class="sxs-lookup"><span data-stu-id="89d7e-175">The name and message are displayed on both pages instantly.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="89d7e-176">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="89d7e-176">Visual Studio Code</span></span>](#tab/visual-studio-code)

1. <span data-ttu-id="89d7e-177">Drücken Sie **Debuggen** (F5), um das Programm zu erstellen und auszuführen.</span><span class="sxs-lookup"><span data-stu-id="89d7e-177">Press **Debug** (F5) to build and run the program.</span></span> <span data-ttu-id="89d7e-178">Das Programm ausführen, wird ein Browserfenster geöffnet.</span><span class="sxs-lookup"><span data-stu-id="89d7e-178">Running the program opens a browser window.</span></span>

1. <span data-ttu-id="89d7e-179">Öffnen Sie einen anderen Browserfenster, und Laden Sie die Website lokal in.</span><span class="sxs-lookup"><span data-stu-id="89d7e-179">Open another browser window and load the website locally in it.</span></span>

1. <span data-ttu-id="89d7e-180">Wählen Sie entweder Browser, geben Sie einen Namen und eine Nachricht, und klicken Sie auf die **senden** Schaltfläche.</span><span class="sxs-lookup"><span data-stu-id="89d7e-180">Choose either browser, enter a name and message, and click the **Send** button.</span></span> <span data-ttu-id="89d7e-181">Der Name und die Meldung werden sofort auf beiden Seiten angezeigt.</span><span class="sxs-lookup"><span data-stu-id="89d7e-181">The name and message are displayed on both pages instantly.</span></span>

---

  ![Lösung](get-started/_static/signalr-get-started-finished.png)

## <a name="related-resources"></a><span data-ttu-id="89d7e-183">Weitere Informationen</span><span class="sxs-lookup"><span data-stu-id="89d7e-183">Related resources</span></span>

[<span data-ttu-id="89d7e-184">Einführung in ASP.NET Core SignalR</span><span class="sxs-lookup"><span data-stu-id="89d7e-184">Introduction to ASP.NET Core SignalR</span></span>](introduction.md)
