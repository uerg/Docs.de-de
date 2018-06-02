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
ms.openlocfilehash: 880abd87805990baf8dd977c340a60582e54d2df
ms.sourcegitcommit: a0b6319c36f41cdce76ea334372f6e14fc66507e
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/02/2018
ms.locfileid: "34729498"
---
# <a name="get-started-with-signalr-on-aspnet-core"></a><span data-ttu-id="5e6d0-103">Erste Schritte mit SignalR für ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="5e6d0-103">Get started with SignalR on ASP.NET Core</span></span>

<span data-ttu-id="5e6d0-104">Von [Rachel Appel](https://twitter.com/rachelappel)</span><span class="sxs-lookup"><span data-stu-id="5e6d0-104">By [Rachel Appel](https://twitter.com/rachelappel)</span></span>

<span data-ttu-id="5e6d0-105">Dieses Lernprogramm zeigt die Grundlagen der Erstellung einer Echtzeit-app mit SignalR für ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="5e6d0-105">This tutorial teaches the basics of building a real-time app using SignalR for ASP.NET Core.</span></span>

   ![Lösung](get-started/_static/signalr-get-started-finished.png)

<span data-ttu-id="5e6d0-107">Dieses Lernprogramm veranschaulicht die folgenden SignalR Entwicklungsaufgaben:</span><span class="sxs-lookup"><span data-stu-id="5e6d0-107">This tutorial demonstrates the following SignalR development tasks:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="5e6d0-108">Erstellen Sie eine SignalR für ASP.NET Core Web-app.</span><span class="sxs-lookup"><span data-stu-id="5e6d0-108">Create a SignalR on ASP.NET Core web app.</span></span>
> * <span data-ttu-id="5e6d0-109">Erstellen eines SignalR-Hubs, um Inhalt an Clients zu verteilen.</span><span class="sxs-lookup"><span data-stu-id="5e6d0-109">Create a SignalR hub to push content to clients.</span></span>
> * <span data-ttu-id="5e6d0-110">Ändern der `Startup` -Klasse und die app konfigurieren.</span><span class="sxs-lookup"><span data-stu-id="5e6d0-110">Modify the `Startup` class and configure the app.</span></span>

<span data-ttu-id="5e6d0-111">[Anzeigen oder Herunterladen von Beispielcode](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/get-started/sample/) ([Vorgehensweise zum Herunterladen](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="5e6d0-111">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/get-started/sample/) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

# <a name="prerequisites"></a><span data-ttu-id="5e6d0-112">Erforderliche Komponenten</span><span class="sxs-lookup"><span data-stu-id="5e6d0-112">Prerequisites</span></span>

<span data-ttu-id="5e6d0-113">Installieren Sie die folgende Software:</span><span class="sxs-lookup"><span data-stu-id="5e6d0-113">Install the following software:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="5e6d0-114">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="5e6d0-114">Visual Studio</span></span>](#tab/visual-studio)

* [<span data-ttu-id="5e6d0-115">.NET Core SDK 2.1 oder höher</span><span class="sxs-lookup"><span data-stu-id="5e6d0-115">.NET Core SDK 2.1 or later</span></span>](https://www.microsoft.com/net/download/all)
* <span data-ttu-id="5e6d0-116">[Visual Studio-2017](https://www.visualstudio.com/downloads/) 15.7 oder höher mit der **ASP.NET und zur Webentwicklung** arbeitsauslastung</span><span class="sxs-lookup"><span data-stu-id="5e6d0-116">[Visual Studio 2017](https://www.visualstudio.com/downloads/) version 15.7 or later with the **ASP.NET and web development** workload</span></span>
* [<span data-ttu-id="5e6d0-117">npm</span><span class="sxs-lookup"><span data-stu-id="5e6d0-117">npm</span></span>](https://www.npmjs.com/get-npm)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="5e6d0-118">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="5e6d0-118">Visual Studio Code</span></span>](#tab/visual-studio-code)

* [<span data-ttu-id="5e6d0-119">.NET Core SDK 2.1 oder höher</span><span class="sxs-lookup"><span data-stu-id="5e6d0-119">.NET Core SDK 2.1 or later</span></span>](https://www.microsoft.com/net/download/all)
* [<span data-ttu-id="5e6d0-120">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="5e6d0-120">Visual Studio Code</span></span>](https://code.visualstudio.com/download)
* [<span data-ttu-id="5e6d0-121">C# für Visual Studio-Code</span><span class="sxs-lookup"><span data-stu-id="5e6d0-121">C# for Visual Studio Code</span></span>](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)
* [<span data-ttu-id="5e6d0-122">npm</span><span class="sxs-lookup"><span data-stu-id="5e6d0-122">npm</span></span>](https://www.npmjs.com/get-npm)

-----

## <a name="create-an-aspnet-core-project-that-hosts-signalr-client-and-server"></a><span data-ttu-id="5e6d0-123">Erstellen Sie ein Projekt von ASP.NET Core, die SignalR-Client und Server hostet</span><span class="sxs-lookup"><span data-stu-id="5e6d0-123">Create an ASP.NET Core project that hosts SignalR client and server</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="5e6d0-124">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="5e6d0-124">Visual Studio</span></span>](#tab/visual-studio/)

1. <span data-ttu-id="5e6d0-125">Verwenden der **Datei** > **neues Projekt** Menü aus, und wählen Sie **ASP.NET-Webanwendung für Core**.</span><span class="sxs-lookup"><span data-stu-id="5e6d0-125">Use the **File** > **New Project** menu option and choose **ASP.NET Core Web Application**.</span></span> <span data-ttu-id="5e6d0-126">Nennen Sie das Projekt *SignalRChat*.</span><span class="sxs-lookup"><span data-stu-id="5e6d0-126">Name the project *SignalRChat*.</span></span>

   ![Dialogfeld "Neues Projekt" in Visual Studio](get-started/_static/signalr-new-project-dialog.png)

2. <span data-ttu-id="5e6d0-128">Wählen Sie **Webanwendung** zum Erstellen eines Projekts mithilfe von Razor-Seiten.</span><span class="sxs-lookup"><span data-stu-id="5e6d0-128">Select **Web Application** to create a project using Razor Pages.</span></span> <span data-ttu-id="5e6d0-129">Wählen Sie dann **OK**.</span><span class="sxs-lookup"><span data-stu-id="5e6d0-129">Then select **OK**.</span></span> <span data-ttu-id="5e6d0-130">Achten Sie darauf, **ASP.NET Core 2.1** aus dem Framework Selektor ausgewählt ist, obwohl SignalR unter älteren Versionen von .NET ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="5e6d0-130">Be sure that **ASP.NET Core 2.1** is selected from the framework selector, though SignalR runs on older versions of .NET.</span></span>

   ![Dialogfeld "Neues Projekt" in Visual Studio](get-started/_static/signalr-new-project-choose-type.png)

<span data-ttu-id="5e6d0-132">Visual Studio enthält die `Microsoft.AspNetCore.SignalR` -Paket mit der zugehörigen Serverbibliotheken als Teil seiner **ASP.NET-Webanwendung für Core** Vorlage.</span><span class="sxs-lookup"><span data-stu-id="5e6d0-132">Visual Studio includes the `Microsoft.AspNetCore.SignalR` package containing its server libraries as part of its **ASP.NET Core Web Application** template.</span></span> <span data-ttu-id="5e6d0-133">Allerdings die JavaScript-Clientbibliothek für SignalR muss installiert werden mit *Npm*.</span><span class="sxs-lookup"><span data-stu-id="5e6d0-133">However, the JavaScript client library for SignalR must be installed using *npm*.</span></span>

3. <span data-ttu-id="5e6d0-134">Führen die folgenden Befehle der **Package Manager Console** Fenster aus dem Projektstamm:</span><span class="sxs-lookup"><span data-stu-id="5e6d0-134">Run the following commands in the **Package Manager Console** window, from the project root:</span></span>

    ```console
    npm init -y
    npm install @aspnet/signalr
    ```     

4. <span data-ttu-id="5e6d0-135">Erstellen Sie einen neuen Ordner namens "Signalr" innerhalb der *Lib* Ordner des Projekts.</span><span class="sxs-lookup"><span data-stu-id="5e6d0-135">Create a new folder named "signalr" inside the  *lib* folder in your project.</span></span> <span data-ttu-id="5e6d0-136">Kopieren Sie dann die *signalr.js* aus der Datei *Node_modules\\ @aspnet\signalr\dist\browser*  in diesen Ordner.</span><span class="sxs-lookup"><span data-stu-id="5e6d0-136">Then copy the *signalr.js* file from *node_modules\\@aspnet\signalr\dist\browser* to this folder.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="5e6d0-137">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="5e6d0-137">Visual Studio Code</span></span>](#tab/visual-studio-code/)

1. <span data-ttu-id="5e6d0-138">Aus der **integrierte Terminaldienste**, führen Sie den folgenden Befehl:</span><span class="sxs-lookup"><span data-stu-id="5e6d0-138">From the **Integrated Terminal**, run the following command:</span></span>

    ```console
    dotnet new webapp -o SignalRChat
    ```

2. <span data-ttu-id="5e6d0-139">Installieren Sie die JavaScript-Client Library verwenden *Npm*.</span><span class="sxs-lookup"><span data-stu-id="5e6d0-139">Install the JavaScript client library using *npm*.</span></span>

    ```console
    npm init -y
    npm install @aspnet/signalr
    ```

3. <span data-ttu-id="5e6d0-140">Kopieren der *signalr.js* aus der Datei *Node_modules\\ @aspnet\signalr\dist\browser*  auf die *Lib* Ordner des Projekts.</span><span class="sxs-lookup"><span data-stu-id="5e6d0-140">Copy the *signalr.js* file from *node_modules\\@aspnet\signalr\dist\browser* to the *lib* folder in your project.</span></span>

-----

## <a name="create-the-signalr-hub"></a><span data-ttu-id="5e6d0-141">Erstellen von SignalR-Hubs.</span><span class="sxs-lookup"><span data-stu-id="5e6d0-141">Create the SignalR Hub</span></span>

<span data-ttu-id="5e6d0-142">Ein Hub ist eine Klasse, die als eine allgemeine Pipeline dient, die zum Aufrufen von Methoden voneinander Client und Server ermöglicht.</span><span class="sxs-lookup"><span data-stu-id="5e6d0-142">A hub is a class that serves as a high-level pipeline that allows the client and server to call methods on each other.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="5e6d0-143">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="5e6d0-143">Visual Studio</span></span>](#tab/visual-studio/)

1. <span data-ttu-id="5e6d0-144">Fügen Sie eine Klasse zum Projekt durch Auswahl **Datei** > **neu** > **Datei** auswählen und **Visual C#-Klasse**.</span><span class="sxs-lookup"><span data-stu-id="5e6d0-144">Add a class to the project by choosing **File** > **New** > **File** and selecting **Visual C# Class**.</span></span> <span data-ttu-id="5e6d0-145">Nennen Sie die Datei *ChatHub*.</span><span class="sxs-lookup"><span data-stu-id="5e6d0-145">Name the file *ChatHub*.</span></span> 

2. <span data-ttu-id="5e6d0-146">Erben von `Microsoft.AspNetCore.SignalR.Hub`.</span><span class="sxs-lookup"><span data-stu-id="5e6d0-146">Inherit from `Microsoft.AspNetCore.SignalR.Hub`.</span></span> <span data-ttu-id="5e6d0-147">Die `Hub` Klasse enthält Eigenschaften und Ereignisse für die Verwaltung von Verbindungen und Gruppen sowie senden und Empfangen von Daten.</span><span class="sxs-lookup"><span data-stu-id="5e6d0-147">The `Hub` class contains properties and events for managing connections and groups, as well as sending and receiving data.</span></span>

3. <span data-ttu-id="5e6d0-148">Erstellen der `SendMessage` -Methode, die eine Nachricht an alle verbundenen Chat-Clients sendet.</span><span class="sxs-lookup"><span data-stu-id="5e6d0-148">Create the `SendMessage` method that sends a message to all connected chat clients.</span></span> <span data-ttu-id="5e6d0-149">Beachten Sie, es gibt eine [Aufgabe](https://msdn.microsoft.com/library/system.threading.tasks.task(v=vs.110).aspx), da SignalR asynchron ist.</span><span class="sxs-lookup"><span data-stu-id="5e6d0-149">Notice it returns a [Task](https://msdn.microsoft.com/library/system.threading.tasks.task(v=vs.110).aspx), because SignalR is asynchronous.</span></span> <span data-ttu-id="5e6d0-150">Asynchronen Code skaliert besser.</span><span class="sxs-lookup"><span data-stu-id="5e6d0-150">Asynchronous code scales better.</span></span>

   [!code-csharp[Startup](get-started/sample/Hubs/ChatHub.cs)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="5e6d0-151">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="5e6d0-151">Visual Studio Code</span></span>](#tab/visual-studio-code/)

1. <span data-ttu-id="5e6d0-152">Öffnen der *SignalRChat* Ordner in Visual Studio-Code.</span><span class="sxs-lookup"><span data-stu-id="5e6d0-152">Open the *SignalRChat* folder in Visual Studio Code.</span></span>

2. <span data-ttu-id="5e6d0-153">Fügen Sie eine Klasse zum Projekt dazu **Datei** > **neue Datei** aus dem Menü.</span><span class="sxs-lookup"><span data-stu-id="5e6d0-153">Add a class to the project by selecting **File** > **New File** from the menu.</span></span>

3. <span data-ttu-id="5e6d0-154">Erben von `Microsoft.AspNetCore.SignalR.Hub`.</span><span class="sxs-lookup"><span data-stu-id="5e6d0-154">Inherit from `Microsoft.AspNetCore.SignalR.Hub`.</span></span> <span data-ttu-id="5e6d0-155">Die `Hub` Klasse enthält Eigenschaften und Ereignisse für die Verwaltung von Verbindungen und Gruppen sowie senden und Empfangen von Daten an Clients.</span><span class="sxs-lookup"><span data-stu-id="5e6d0-155">The `Hub` class contains properties and events for managing connections and groups, as well as sending and receiving data to clients.</span></span>

4. <span data-ttu-id="5e6d0-156">Fügen Sie der Klasse eine `SendMessage`-Method hinzu.</span><span class="sxs-lookup"><span data-stu-id="5e6d0-156">Add a `SendMessage` method to the class.</span></span> <span data-ttu-id="5e6d0-157">Die `SendMessage` Methode sendet eine Nachricht an alle verbundenen Chat-Clients.</span><span class="sxs-lookup"><span data-stu-id="5e6d0-157">The `SendMessage` method sends a message to all connected chat clients.</span></span> <span data-ttu-id="5e6d0-158">Beachten Sie, es gibt eine [Aufgabe](/dotnet/api/system.threading.tasks.task), da SignalR asynchron ist.</span><span class="sxs-lookup"><span data-stu-id="5e6d0-158">Notice it returns a [Task](/dotnet/api/system.threading.tasks.task), because SignalR is asynchronous.</span></span> <span data-ttu-id="5e6d0-159">Asynchronen Code skaliert besser.</span><span class="sxs-lookup"><span data-stu-id="5e6d0-159">Asynchronous code scales better.</span></span>

   [!code-csharp[Startup](get-started/sample/Hubs/ChatHub.cs?range=6-12)]

-----

## <a name="configure-the-project-to-use-signalr"></a><span data-ttu-id="5e6d0-160">Konfigurieren Sie das Projekt zur Verwendung von SignalR</span><span class="sxs-lookup"><span data-stu-id="5e6d0-160">Configure the project to use SignalR</span></span>

<span data-ttu-id="5e6d0-161">Die SignalR-Server muss konfiguriert werden, damit dieser weiß, dass Anforderungen an SignalR übergeben.</span><span class="sxs-lookup"><span data-stu-id="5e6d0-161">The SignalR server must be configured so that it knows to pass requests to SignalR.</span></span>

1. <span data-ttu-id="5e6d0-162">Ändern Sie des Projekts zum Konfigurieren einer SignalR-Projekts `Startup.ConfigureServices` Methode.</span><span class="sxs-lookup"><span data-stu-id="5e6d0-162">To configure a SignalR project, modify the project's `Startup.ConfigureServices` method.</span></span>

   <span data-ttu-id="5e6d0-163">`services.AddSignalR` Fügt der SignalR als Teil der [Middleware](xref:fundamentals/middleware/index) Pipeline.</span><span class="sxs-lookup"><span data-stu-id="5e6d0-163">`services.AddSignalR` adds SignalR as part of the [middleware](xref:fundamentals/middleware/index) pipeline.</span></span>

2. <span data-ttu-id="5e6d0-164">Konfigurieren von Routen zu Ihrer Verwendung Hubs `UseSignalR`.</span><span class="sxs-lookup"><span data-stu-id="5e6d0-164">Configure routes to your hubs using `UseSignalR`.</span></span>


   [!code-csharp[Startup](get-started/sample/Startup.cs?highlight=37,57-60)]


## <a name="create-the-signalr-client-code"></a><span data-ttu-id="5e6d0-165">Erstellen Sie den SignalR-Clientcode</span><span class="sxs-lookup"><span data-stu-id="5e6d0-165">Create the SignalR client code</span></span>

1. <span data-ttu-id="5e6d0-166">Ersetzen Sie den Inhalt in *Pages\Index.cshtml* durch den folgenden Code:</span><span class="sxs-lookup"><span data-stu-id="5e6d0-166">Replace the content in *Pages\Index.cshtml* with the following code:</span></span>

   [!code-cshtml[Index](get-started/sample/Pages/Index.cshtml)]

   <span data-ttu-id="5e6d0-167">Die vorangehende HTML zeigt Name und Nachrichtenfelder sowie eine Schaltfläche "Absenden".</span><span class="sxs-lookup"><span data-stu-id="5e6d0-167">The preceding HTML displays name and message fields, and a submit button.</span></span> <span data-ttu-id="5e6d0-168">Beachten Sie die Skriptverweise im unteren Bereich: ein Verweis auf SignalR und *chat.js*.</span><span class="sxs-lookup"><span data-stu-id="5e6d0-168">Notice the script references at the bottom: a reference to SignalR and *chat.js*.</span></span>

2. <span data-ttu-id="5e6d0-169">Fügen Sie eine JavaScript-Datei, mit dem Namen *chat.js*, zu der *Wwwroot\js* Ordner.</span><span class="sxs-lookup"><span data-stu-id="5e6d0-169">Add a JavaScript file, named *chat.js*, to the *wwwroot\js* folder.</span></span> <span data-ttu-id="5e6d0-170">Fügen Sie ihr folgenden Code hinzu:</span><span class="sxs-lookup"><span data-stu-id="5e6d0-170">Add the following code to it:</span></span>

   [!code-javascript[Index](get-started/sample/wwwroot/js/chat.js)]

## <a name="run-the-app"></a><span data-ttu-id="5e6d0-171">Ausführen der App</span><span class="sxs-lookup"><span data-stu-id="5e6d0-171">Run the app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="5e6d0-172">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="5e6d0-172">Visual Studio</span></span>](#tab/visual-studio)

1. <span data-ttu-id="5e6d0-173">Wählen Sie **Debuggen** > **Starten ohne Debuggen** starten Sie einen Browser, und laden die Website lokal.</span><span class="sxs-lookup"><span data-stu-id="5e6d0-173">Select **Debug** > **Start without debugging** to launch a browser and load the website locally.</span></span> <span data-ttu-id="5e6d0-174">Kopieren Sie die URL aus der Adressleiste ein.</span><span class="sxs-lookup"><span data-stu-id="5e6d0-174">Copy the URL from the address bar.</span></span>

1. <span data-ttu-id="5e6d0-175">Öffnen Sie eine andere Browserinstanz (alle Browser), und fügen Sie die URL in der Adressleiste.</span><span class="sxs-lookup"><span data-stu-id="5e6d0-175">Open another browser instance (any browser) and paste the URL in the address bar.</span></span>

1. <span data-ttu-id="5e6d0-176">Wählen Sie entweder Browser, geben Sie einen Namen und eine Nachricht, und klicken Sie auf die **senden** Schaltfläche.</span><span class="sxs-lookup"><span data-stu-id="5e6d0-176">Choose either browser, enter a name and message, and click the **Send** button.</span></span> <span data-ttu-id="5e6d0-177">Der Name und die Meldung werden sofort auf beiden Seiten angezeigt.</span><span class="sxs-lookup"><span data-stu-id="5e6d0-177">The name and message are displayed on both pages instantly.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="5e6d0-178">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="5e6d0-178">Visual Studio Code</span></span>](#tab/visual-studio-code)

1. <span data-ttu-id="5e6d0-179">Drücken Sie **Debuggen** (F5), um das Programm zu erstellen und auszuführen.</span><span class="sxs-lookup"><span data-stu-id="5e6d0-179">Press **Debug** (F5) to build and run the program.</span></span> <span data-ttu-id="5e6d0-180">Das Programm ausführen, wird ein Browserfenster geöffnet.</span><span class="sxs-lookup"><span data-stu-id="5e6d0-180">Running the program opens a browser window.</span></span>

1. <span data-ttu-id="5e6d0-181">Öffnen Sie einen anderen Browserfenster, und Laden Sie die Website lokal in.</span><span class="sxs-lookup"><span data-stu-id="5e6d0-181">Open another browser window and load the website locally in it.</span></span>

1. <span data-ttu-id="5e6d0-182">Wählen Sie entweder Browser, geben Sie einen Namen und eine Nachricht, und klicken Sie auf die **senden** Schaltfläche.</span><span class="sxs-lookup"><span data-stu-id="5e6d0-182">Choose either browser, enter a name and message, and click the **Send** button.</span></span> <span data-ttu-id="5e6d0-183">Der Name und die Meldung werden sofort auf beiden Seiten angezeigt.</span><span class="sxs-lookup"><span data-stu-id="5e6d0-183">The name and message are displayed on both pages instantly.</span></span>

---

  ![Lösung](get-started/_static/signalr-get-started-finished.png)

## <a name="related-resources"></a><span data-ttu-id="5e6d0-185">Weitere Informationen</span><span class="sxs-lookup"><span data-stu-id="5e6d0-185">Related resources</span></span>

[<span data-ttu-id="5e6d0-186">Einführung in ASP.NET Core SignalR</span><span class="sxs-lookup"><span data-stu-id="5e6d0-186">Introduction to ASP.NET Core SignalR</span></span>](introduction.md)
