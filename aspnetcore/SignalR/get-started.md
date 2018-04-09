---
title: Erste Schritte mit SignalR für ASP.NET Core
author: rachelappel
description: In diesem Lernprogramm erstellen Sie eine app mit SignalR für ASP.NET Core.
manager: wpickett
ms.author: rachelap
ms.custom: mvc
ms.date: 03/16/2018
ms.prod: aspnet-core
ms.topic: tutorial
ms.technology: aspnet
uid: signalr/get-started
ms.openlocfilehash: cf120d535c85c7871f5b1f27039018ea2405b9cb
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
---
# <a name="get-started-with-signalr-on-aspnet-core"></a><span data-ttu-id="43afd-103">Erste Schritte mit SignalR für ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="43afd-103">Get started with SignalR on ASP.NET Core</span></span>

<span data-ttu-id="43afd-104">Von [Rachel Appel](https://twitter.com/rachelappel)</span><span class="sxs-lookup"><span data-stu-id="43afd-104">By [Rachel Appel](https://twitter.com/rachelappel)</span></span>

[!INCLUDE [Version notice](../includes/signalr-version-notice.md)]

<span data-ttu-id="43afd-105">Dieses Lernprogramm zeigt die Grundlagen der Erstellung einer Echtzeit-app mit SignalR für ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="43afd-105">This tutorial teaches the basics of building a real-time app using SignalR for ASP.NET Core.</span></span>

   ![Lösung](get-started/_static/signalr-get-started-finished.png)

<span data-ttu-id="43afd-107">Dieses Lernprogramm veranschaulicht die folgenden SignalR Entwicklungsaufgaben:</span><span class="sxs-lookup"><span data-stu-id="43afd-107">This tutorial demonstrates the following SignalR development tasks:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="43afd-108">Erstellen Sie eine SignalR für ASP.NET Core Web-app.</span><span class="sxs-lookup"><span data-stu-id="43afd-108">Create a SignalR on ASP.NET Core web app.</span></span>
> * <span data-ttu-id="43afd-109">Erstellen eines SignalR-Hubs, um Inhalt an Clients zu verteilen.</span><span class="sxs-lookup"><span data-stu-id="43afd-109">Create a SignalR hub to push content to clients.</span></span>
> * <span data-ttu-id="43afd-110">Ändern der `Startup` -Klasse und die app konfigurieren.</span><span class="sxs-lookup"><span data-stu-id="43afd-110">Modify the `Startup` class and configure the app.</span></span>

<span data-ttu-id="43afd-111">[Anzeigen oder Herunterladen von Beispielcode](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/get-started/sample/) ([Vorgehensweise zum Herunterladen](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="43afd-111">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/get-started/sample/) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

# <a name="prerequisites"></a><span data-ttu-id="43afd-112">Erforderliche Komponenten</span><span class="sxs-lookup"><span data-stu-id="43afd-112">Prerequisites</span></span>

<span data-ttu-id="43afd-113">Installieren Sie die folgende Software:</span><span class="sxs-lookup"><span data-stu-id="43afd-113">Install the following software:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="43afd-114">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="43afd-114">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="43afd-115">[.NET Core 2.1.0 Vorschau 1 SDK](https://www.microsoft.com/net/download/dotnet-core/sdk-2.1.300-preview1) oder höher</span><span class="sxs-lookup"><span data-stu-id="43afd-115">[.NET Core 2.1.0 Preview 1 SDK](https://www.microsoft.com/net/download/dotnet-core/sdk-2.1.300-preview1) or later</span></span>
* <span data-ttu-id="43afd-116">[Visual Studio-2017](https://www.visualstudio.com/downloads/) Version 15.6 oder höher, mit der **ASP.NET und zur Webentwicklung** arbeitsauslastung</span><span class="sxs-lookup"><span data-stu-id="43afd-116">[Visual Studio 2017](https://www.visualstudio.com/downloads/) version 15.6 or later with the **ASP.NET and web development** workload</span></span>
* [<span data-ttu-id="43afd-117">npm</span><span class="sxs-lookup"><span data-stu-id="43afd-117">npm</span></span>](https://www.npmjs.com/get-npm)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="43afd-118">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="43afd-118">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="43afd-119">[.NET Core 2.1.0 Vorschau 1 SDK](https://www.microsoft.com/net/download/dotnet-core/sdk-2.1.300-preview1) oder höher</span><span class="sxs-lookup"><span data-stu-id="43afd-119">[.NET Core 2.1.0 Preview 1 SDK](https://www.microsoft.com/net/download/dotnet-core/sdk-2.1.300-preview1) or later</span></span>
* [<span data-ttu-id="43afd-120">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="43afd-120">Visual Studio Code</span></span>](https://code.visualstudio.com/download) 
* [<span data-ttu-id="43afd-121">C# für Visual Studio-Code</span><span class="sxs-lookup"><span data-stu-id="43afd-121">C# for Visual Studio Code</span></span>](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)
* [<span data-ttu-id="43afd-122">npm</span><span class="sxs-lookup"><span data-stu-id="43afd-122">npm</span></span>](https://www.npmjs.com/get-npm)

-----

## <a name="create-an-aspnet-core-project-that-hosts-signalr-client-and-server"></a><span data-ttu-id="43afd-123">Erstellen Sie ein Projekt von ASP.NET Core, die SignalR-Client und Server hostet</span><span class="sxs-lookup"><span data-stu-id="43afd-123">Create an ASP.NET Core project that hosts SignalR client and server</span></span>

#### <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="43afd-124">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="43afd-124">Visual Studio</span></span>](#tab/visual-studio/)
1. <span data-ttu-id="43afd-125">Verwenden der **Datei** > **neues Projekt** Menü aus, und wählen Sie **ASP.NET-Webanwendung für Core**.</span><span class="sxs-lookup"><span data-stu-id="43afd-125">Use the **File** > **New Project** menu option and choose **ASP.NET Core Web Application**.</span></span> <span data-ttu-id="43afd-126">Nennen Sie das Projekt *SignalRChat*.</span><span class="sxs-lookup"><span data-stu-id="43afd-126">Name the project *SignalRChat*.</span></span>

   ![Dialogfeld "Neues Projekt" in Visual Studio](get-started/_static/signalr-new-project-dialog.png)

2. <span data-ttu-id="43afd-128">Wählen Sie **Webanwendung** zum Erstellen eines Projekts mithilfe von Razor-Seiten.</span><span class="sxs-lookup"><span data-stu-id="43afd-128">Select **Web Application** to create a project using Razor Pages.</span></span> <span data-ttu-id="43afd-129">Wählen Sie dann **OK**.</span><span class="sxs-lookup"><span data-stu-id="43afd-129">Then select **OK**.</span></span> <span data-ttu-id="43afd-130">Achten Sie darauf, **ASP.NET Core 2.1** aus dem Framework Selektor ausgewählt ist, obwohl SignalR unter älteren Versionen von .NET ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="43afd-130">Be sure that **ASP.NET Core 2.1** is selected from the framework selector, though SignalR runs on older versions of .NET.</span></span>

   ![Dialogfeld "Neues Projekt" in Visual Studio](get-started/_static/signalr-new-project-choose-type.png)

3. <span data-ttu-id="43afd-132">Mit der rechten Maustaste des Projekts im **Projektmappen-Explorer** > **hinzufügen** > **neues Element** > **Npm-Konfigurationsdatei** .</span><span class="sxs-lookup"><span data-stu-id="43afd-132">Right-click the project in **Solution Explorer** > **Add** > **New Item** > **npm Configuration File**.</span></span> <span data-ttu-id="43afd-133">Nennen Sie die Datei *"Package.JSON"*.</span><span class="sxs-lookup"><span data-stu-id="43afd-133">Name the file *package.json*.</span></span>

4. <span data-ttu-id="43afd-134">Führen den folgenden Befehl der **Package Manager Console** Fenster aus dem Projektstamm:</span><span class="sxs-lookup"><span data-stu-id="43afd-134">Run the following command in the **Package Manager Console** window, from the project root:</span></span>

    ```console
      npm install @aspnet/signalr
    ```
5. <span data-ttu-id="43afd-135">Kopieren der <em>signalr.js</em> aus der Datei <em>Node_modules\\ @aspnet\signalr\dist\browser</em>  auf die <em>Wwwroot\lib</em> Ordner des Projekts.</span><span class="sxs-lookup"><span data-stu-id="43afd-135">Copy the <em>signalr.js</em> file from <em>node_modules\\@aspnet\signalr\dist\browser</em> to the <em>wwwroot\lib</em> folder in your project.</span></span>

#### <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="43afd-136">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="43afd-136">Visual Studio Code</span></span>](#tab/visual-studio-code/)
1. <span data-ttu-id="43afd-137">Aus der **integrierte Terminaldienste**, führen Sie den folgenden Befehl:</span><span class="sxs-lookup"><span data-stu-id="43afd-137">From the **Integrated Terminal**, run the following command:</span></span>

    ```console
      dotnet new razor -o SignalRChat
    ```

2. <span data-ttu-id="43afd-138">Installieren Sie die JavaScript-Client Library verwenden *Npm*.</span><span class="sxs-lookup"><span data-stu-id="43afd-138">Install the JavaScript client library using *npm*.</span></span>

    ```
      npm init -y
      npm install @aspnet/signalr
    ```

* * *
## <a name="create-the-signalr-hub"></a><span data-ttu-id="43afd-139">Erstellen von SignalR-Hubs.</span><span class="sxs-lookup"><span data-stu-id="43afd-139">Create the SignalR Hub</span></span>

<span data-ttu-id="43afd-140">Ein Hub ist eine Klasse, die als eine allgemeine Pipeline dient, die zum Aufrufen von Methoden voneinander Client und Server ermöglicht.</span><span class="sxs-lookup"><span data-stu-id="43afd-140">A hub is a class that serves as a high-level pipeline that allows the client and server to call methods on each other.</span></span>

#### <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="43afd-141">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="43afd-141">Visual Studio</span></span>](#tab/visual-studio/)
1. <span data-ttu-id="43afd-142">Fügen Sie eine Klasse zum Projekt durch Auswahl **Datei** > **neu** > **Datei** auswählen und **Visual C#-Klasse**.</span><span class="sxs-lookup"><span data-stu-id="43afd-142">Add a class to the project by choosing **File** > **New** > **File** and selecting **Visual C# Class**.</span></span>

2. <span data-ttu-id="43afd-143">Erben von `Microsoft.AspNetCore.SignalR.Hub`.</span><span class="sxs-lookup"><span data-stu-id="43afd-143">Inherit from `Microsoft.AspNetCore.SignalR.Hub`.</span></span> <span data-ttu-id="43afd-144">Die `Hub` Klasse enthält Eigenschaften und Ereignisse für die Verwaltung von Verbindungen und Gruppen sowie senden und Empfangen von Daten.</span><span class="sxs-lookup"><span data-stu-id="43afd-144">The `Hub` class contains properties and events for managing connections and groups, as well as sending and receiving data.</span></span>

3. <span data-ttu-id="43afd-145">Erstellen der `SendMessage` -Methode, die eine Nachricht an alle verbundenen Chat-Clients sendet.</span><span class="sxs-lookup"><span data-stu-id="43afd-145">Create the `SendMessage` method that sends a message to all connected chat clients.</span></span> <span data-ttu-id="43afd-146">Beachten Sie, es gibt eine [Aufgabe](https://msdn.microsoft.com/en-us/library/system.threading.tasks.task(v=vs.110).aspx), da SignalR asynchron ist.</span><span class="sxs-lookup"><span data-stu-id="43afd-146">Notice it returns a [Task](https://msdn.microsoft.com/en-us/library/system.threading.tasks.task(v=vs.110).aspx), because SignalR is asynchronous.</span></span> <span data-ttu-id="43afd-147">Asynchronen Code skaliert besser.</span><span class="sxs-lookup"><span data-stu-id="43afd-147">Asynchronous code scales better.</span></span>

   [!code-csharp[Startup](get-started/sample/Hubs/ChatHub.cs?range=7-14)]

#### <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="43afd-148">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="43afd-148">Visual Studio Code</span></span>](#tab/visual-studio-code/)
1. <span data-ttu-id="43afd-149">Öffnen der *SignalRChat* Ordner in Visual Studio-Code.</span><span class="sxs-lookup"><span data-stu-id="43afd-149">Open the *SignalRChat* folder in Visual Studio Code.</span></span>

2. <span data-ttu-id="43afd-150">Fügen Sie eine Klasse zum Projekt dazu **Datei** > **neue Datei** aus dem Menü.</span><span class="sxs-lookup"><span data-stu-id="43afd-150">Add a class to the project by selecting **File** > **New File** from the menu.</span></span>

3. <span data-ttu-id="43afd-151">Erben von `Microsoft.AspNetCore.SignalR.Hub`.</span><span class="sxs-lookup"><span data-stu-id="43afd-151">Inherit from `Microsoft.AspNetCore.SignalR.Hub`.</span></span> <span data-ttu-id="43afd-152">Die `Hub` Klasse enthält Eigenschaften und Ereignisse für die Verwaltung von Verbindungen und Gruppen sowie senden und Empfangen von Daten an Clients.</span><span class="sxs-lookup"><span data-stu-id="43afd-152">The `Hub` class contains properties and events for managing connections and groups, as well as sending and receiving data to clients.</span></span>

4. <span data-ttu-id="43afd-153">Fügen Sie der Klasse eine `SendMessage`-Method hinzu.</span><span class="sxs-lookup"><span data-stu-id="43afd-153">Add a `SendMessage` method to the class.</span></span> <span data-ttu-id="43afd-154">Die `SendMessage` Methode sendet eine Nachricht an alle verbundenen Chat-Clients.</span><span class="sxs-lookup"><span data-stu-id="43afd-154">The `SendMessage` method sends a message to all connected chat clients.</span></span> <span data-ttu-id="43afd-155">Beachten Sie, es gibt eine [Aufgabe](/dotnet/api/system.threading.tasks.task), da SignalR asynchron ist.</span><span class="sxs-lookup"><span data-stu-id="43afd-155">Notice it returns a [Task](/dotnet/api/system.threading.tasks.task), because SignalR is asynchronous.</span></span> <span data-ttu-id="43afd-156">Asynchronen Code skaliert besser.</span><span class="sxs-lookup"><span data-stu-id="43afd-156">Asynchronous code scales better.</span></span>

   [!code-csharp[Startup](get-started/sample/Hubs/ChatHub.cs?range=7-14)]

* * *
## <a name="configure-the-project-to-use-signalr"></a><span data-ttu-id="43afd-157">Konfigurieren Sie das Projekt zur Verwendung von SignalR</span><span class="sxs-lookup"><span data-stu-id="43afd-157">Configure the project to use SignalR</span></span>

<span data-ttu-id="43afd-158">Die SignalR-Server muss konfiguriert werden, damit dieser weiß, dass Anforderungen an SignalR übergeben.</span><span class="sxs-lookup"><span data-stu-id="43afd-158">The SignalR server must be configured so that it knows to pass requests to SignalR.</span></span>

1. <span data-ttu-id="43afd-159">Ändern Sie des Projekts zum Konfigurieren einer SignalR-Projekts `Startup.ConfigureServices` Methode.</span><span class="sxs-lookup"><span data-stu-id="43afd-159">To configure a SignalR project, modify the project's `Startup.ConfigureServices` method.</span></span>

   <span data-ttu-id="43afd-160">`services.AddSignalR` Fügt der SignalR als Teil der [Middleware](xref:fundamentals/middleware/index) Pipeline.</span><span class="sxs-lookup"><span data-stu-id="43afd-160">`services.AddSignalR` adds SignalR as part of the [middleware](xref:fundamentals/middleware/index) pipeline.</span></span>

2. <span data-ttu-id="43afd-161">Konfigurieren von Routen zu Ihrer Verwendung Hubs `UseSignalR`.</span><span class="sxs-lookup"><span data-stu-id="43afd-161">Configure routes to your hubs using `UseSignalR`.</span></span>

   [!code-csharp[Startup](get-started/sample/Startup.cs?highlight=22,40-43)]

## <a name="create-the-signalr-client-code"></a><span data-ttu-id="43afd-162">Erstellen Sie den SignalR-Clientcode</span><span class="sxs-lookup"><span data-stu-id="43afd-162">Create the SignalR client code</span></span>

1. <span data-ttu-id="43afd-163">Ersetzen Sie den Inhalt in *Pages\Index.cshtml* durch den folgenden Code:</span><span class="sxs-lookup"><span data-stu-id="43afd-163">Replace the content in *Pages\Index.cshtml* with the following code:</span></span>

   [!code-cshtml[Index](get-started/sample/Pages/Index.cshtml)]

   <span data-ttu-id="43afd-164">Die vorangehende HTML zeigt Name und Nachrichtenfelder sowie eine Schaltfläche "Absenden".</span><span class="sxs-lookup"><span data-stu-id="43afd-164">The preceding HTML displays name and message fields, and a submit button.</span></span> <span data-ttu-id="43afd-165">Beachten Sie die Skriptverweise im unteren Bereich: ein Verweis auf SignalR und *chat.js*.</span><span class="sxs-lookup"><span data-stu-id="43afd-165">Notice the script references at the bottom: a reference to SignalR and *chat.js*.</span></span>

2. <span data-ttu-id="43afd-166">Fügen Sie eine JavaScript-Datei, mit dem Namen *chat.js*, zu der *Wwwroot\js* Ordner.</span><span class="sxs-lookup"><span data-stu-id="43afd-166">Add a JavaScript file, named *chat.js*, to the *wwwroot\js* folder.</span></span> <span data-ttu-id="43afd-167">Fügen Sie ihr folgenden Code hinzu:</span><span class="sxs-lookup"><span data-stu-id="43afd-167">Add the following code to it:</span></span>

   [!code-javascript[Index](get-started/sample/wwwroot/js/chat.js)]

## <a name="run-the-app"></a><span data-ttu-id="43afd-168">Ausführen der App</span><span class="sxs-lookup"><span data-stu-id="43afd-168">Run the app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="43afd-169">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="43afd-169">Visual Studio</span></span>](#tab/visual-studio)

1. <span data-ttu-id="43afd-170">Wählen Sie **Debuggen** > **Starten ohne Debuggen** starten Sie einen Browser, und laden die Website lokal.</span><span class="sxs-lookup"><span data-stu-id="43afd-170">Select **Debug** > **Start without debugging** to launch a browser and load the website locally.</span></span> <span data-ttu-id="43afd-171">Kopieren Sie die URL aus der Adressleiste ein.</span><span class="sxs-lookup"><span data-stu-id="43afd-171">Copy the URL from the address bar.</span></span>

1. <span data-ttu-id="43afd-172">Öffnen Sie eine andere Browserinstanz (alle Browser), und fügen Sie die URL in der Adressleiste.</span><span class="sxs-lookup"><span data-stu-id="43afd-172">Open another browser instance (any browser) and paste the URL in the address bar.</span></span>

1. <span data-ttu-id="43afd-173">Wählen Sie entweder Browser, geben Sie einen Namen und eine Nachricht, und klicken Sie auf die **senden** Schaltfläche.</span><span class="sxs-lookup"><span data-stu-id="43afd-173">Choose either browser, enter a name and message, and click the **Send** button.</span></span> <span data-ttu-id="43afd-174">Der Name und die Meldung werden sofort auf beiden Seiten angezeigt.</span><span class="sxs-lookup"><span data-stu-id="43afd-174">The name and message are displayed on both pages instantly.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="43afd-175">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="43afd-175">Visual Studio Code</span></span>](#tab/visual-studio-code)

1. <span data-ttu-id="43afd-176">Drücken Sie **Debuggen** (F5), um das Programm zu erstellen und auszuführen.</span><span class="sxs-lookup"><span data-stu-id="43afd-176">Press **Debug** (F5) to build and run the program.</span></span> <span data-ttu-id="43afd-177">Das Programm ausführen, wird ein Browserfenster geöffnet.</span><span class="sxs-lookup"><span data-stu-id="43afd-177">Running the program opens a browser window.</span></span>

1. <span data-ttu-id="43afd-178">Öffnen Sie einen anderen Browserfenster, und Laden Sie die Website lokal in.</span><span class="sxs-lookup"><span data-stu-id="43afd-178">Open another browser window and load the website locally in it.</span></span>

1. <span data-ttu-id="43afd-179">Wählen Sie entweder Browser, geben Sie einen Namen und eine Nachricht, und klicken Sie auf die **senden** Schaltfläche.</span><span class="sxs-lookup"><span data-stu-id="43afd-179">Choose either browser, enter a name and message, and click the **Send** button.</span></span> <span data-ttu-id="43afd-180">Der Name und die Meldung werden sofort auf beiden Seiten angezeigt.</span><span class="sxs-lookup"><span data-stu-id="43afd-180">The name and message are displayed on both pages instantly.</span></span>

-----

  ![Lösung](get-started/_static/signalr-get-started-finished.png)

## <a name="related-resources"></a><span data-ttu-id="43afd-182">Weitere Informationen</span><span class="sxs-lookup"><span data-stu-id="43afd-182">Related resources</span></span>

[<span data-ttu-id="43afd-183">Einführung in ASP.NET Core SignalR</span><span class="sxs-lookup"><span data-stu-id="43afd-183">Introduction to ASP.NET Core SignalR</span></span>](introduction.md)