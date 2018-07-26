---
title: Erste Schritte mit SignalR in ASP.NET Core
author: tdykstra
description: In diesem Tutorial erstellen Sie eine ASP.NET Core-App, die SignalR verwendet.
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 05/22/2018
uid: tutorials/signalr
ms.openlocfilehash: 83be28b30cf06eeea37e8d76b3f6444ffd9a10e8
ms.sourcegitcommit: 3ca527f27c88cfc9d04688db5499e372fbc2c775
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/17/2018
ms.locfileid: "39095490"
---
# <a name="get-started-with-signalr-on-aspnet-core"></a><span data-ttu-id="b8dbe-103">Erste Schritte mit SignalR in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="b8dbe-103">Get started with SignalR on ASP.NET Core</span></span>

<span data-ttu-id="b8dbe-104">In diesem Tutorial werden die Grundlagen zur Erstellung einer Echtzeit-App mit SignalR für ASP.NET Core beschrieben.</span><span class="sxs-lookup"><span data-stu-id="b8dbe-104">This tutorial teaches the basics of building a real-time app using SignalR for ASP.NET Core.</span></span>

   ![Lösung](signalr/_static/signalr-get-started-finished.png)

<span data-ttu-id="b8dbe-106">In diesem Tutorial werden die folgenden SignalR-Entwicklungsaufgaben veranschaulicht:</span><span class="sxs-lookup"><span data-stu-id="b8dbe-106">This tutorial demonstrates the following SignalR development tasks:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="b8dbe-107">Erstellen einer SignalR-Web-App für ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="b8dbe-107">Create a SignalR on ASP.NET Core web app.</span></span>
> * <span data-ttu-id="b8dbe-108">Erstellen eines SignalR-Hubs zur Übermittlung von Inhalten per Push an Clients</span><span class="sxs-lookup"><span data-stu-id="b8dbe-108">Create a SignalR hub to push content to clients.</span></span>
> * <span data-ttu-id="b8dbe-109">Anpassen der `Startup`-Klasse und Konfigurieren der App</span><span class="sxs-lookup"><span data-stu-id="b8dbe-109">Modify the `Startup` class and configure the app.</span></span>

<span data-ttu-id="b8dbe-110">[Anzeigen oder Herunterladen von Beispielcode](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/signalr/sample) ([Vorgehensweise zum Herunterladen](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="b8dbe-110">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/signalr/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="b8dbe-111">Erforderliche Komponenten</span><span class="sxs-lookup"><span data-stu-id="b8dbe-111">Prerequisites</span></span>

<span data-ttu-id="b8dbe-112">Installieren Sie die folgenden Softwarekomponenten:</span><span class="sxs-lookup"><span data-stu-id="b8dbe-112">Install the following software:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="b8dbe-113">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="b8dbe-113">Visual Studio</span></span>](#tab/visual-studio)

* [<span data-ttu-id="b8dbe-114">.NET Core SDK 2.1 oder höher</span><span class="sxs-lookup"><span data-stu-id="b8dbe-114">.NET Core SDK 2.1 or later</span></span>](https://www.microsoft.com/net/download/all)
* <span data-ttu-id="b8dbe-115">Version 15.7.3 oder höher von [Visual Studio 2017](https://www.visualstudio.com/downloads/) mit der Workload für **ASP.NET und Webentwicklung**</span><span class="sxs-lookup"><span data-stu-id="b8dbe-115">[Visual Studio 2017](https://www.visualstudio.com/downloads/) version 15.7.3 or later with the **ASP.NET and web development** workload</span></span>
* <span data-ttu-id="b8dbe-116">[npm](https://www.npmjs.com/get-npm) (Paket-Manager für Node.js)</span><span class="sxs-lookup"><span data-stu-id="b8dbe-116">[npm](https://www.npmjs.com/get-npm) (package manager for Node.js)</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="b8dbe-117">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="b8dbe-117">Visual Studio Code</span></span>](#tab/visual-studio-code)

* [<span data-ttu-id="b8dbe-118">.NET Core SDK 2.1 oder höher</span><span class="sxs-lookup"><span data-stu-id="b8dbe-118">.NET Core SDK 2.1 or later</span></span>](https://www.microsoft.com/net/download/all)
* [<span data-ttu-id="b8dbe-119">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="b8dbe-119">Visual Studio Code</span></span>](https://code.visualstudio.com/download)
* [<span data-ttu-id="b8dbe-120">C# für Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="b8dbe-120">C# for Visual Studio Code</span></span>](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)
* <span data-ttu-id="b8dbe-121">[npm](https://www.npmjs.com/get-npm) (Paket-Manager für Node.js)</span><span class="sxs-lookup"><span data-stu-id="b8dbe-121">[npm](https://www.npmjs.com/get-npm) (package manager for Node.js)</span></span>

-----

## <a name="create-an-aspnet-core-project-that-hosts-signalr-client-and-server"></a><span data-ttu-id="b8dbe-122">Erstellen eines ASP.NET Core-Projekts zum Hosten eines SignalR-Clients und -Servers</span><span class="sxs-lookup"><span data-stu-id="b8dbe-122">Create an ASP.NET Core project that hosts SignalR client and server</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="b8dbe-123">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="b8dbe-123">Visual Studio</span></span>](#tab/visual-studio/)

1. <span data-ttu-id="b8dbe-124">Klicken Sie zuerst auf die Menüoption **Datei** > **Neues Projekt** und anschließend auf **ASP.NET Core-Web-App**.</span><span class="sxs-lookup"><span data-stu-id="b8dbe-124">Use the **File** > **New Project** menu option and choose **ASP.NET Core Web Application**.</span></span> <span data-ttu-id="b8dbe-125">Geben Sie als Projektname *SignalRChat* ein.</span><span class="sxs-lookup"><span data-stu-id="b8dbe-125">Name the project *SignalRChat*.</span></span>

   ![Dialogfeld „Neues Projekt“ in Visual Studio](signalr/_static/signalr-new-project-dialog.png)

2. <span data-ttu-id="b8dbe-127">Klicken Sie auf **Webanwendung**, um ein Projekt mit Razor Pages zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="b8dbe-127">Select **Web Application** to create a project using Razor Pages.</span></span> <span data-ttu-id="b8dbe-128">Klicken Sie anschließend auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="b8dbe-128">Then select **OK**.</span></span> <span data-ttu-id="b8dbe-129">Obwohl SignalR auch unter älteren Versionen von .NET ausgeführt werden kann, sollte Sie darauf achten, **ASP.NET Core 2.1** als Framework aus der Dropdownliste auszuwählen.</span><span class="sxs-lookup"><span data-stu-id="b8dbe-129">Be sure that **ASP.NET Core 2.1** is selected from the framework selector, though SignalR runs on older versions of .NET.</span></span>

   ![Dialogfeld „Neues Projekt“ in Visual Studio](signalr/_static/signalr-new-project-choose-type.png)

<span data-ttu-id="b8dbe-131">Visual Studio enthält das `Microsoft.AspNetCore.SignalR`-Paket mit den zugehörigen Serverbibliotheken als Teil der **ASP.NET Core-Web-App**-Vorlage.</span><span class="sxs-lookup"><span data-stu-id="b8dbe-131">Visual Studio includes the `Microsoft.AspNetCore.SignalR` package containing its server libraries as part of its **ASP.NET Core Web Application** template.</span></span> <span data-ttu-id="b8dbe-132">Die JavaScript-Clientbibliothek für SignalR muss allerdings mit *npm* installiert werden.</span><span class="sxs-lookup"><span data-stu-id="b8dbe-132">However, the JavaScript client library for SignalR must be installed using *npm*.</span></span>

3. <span data-ttu-id="b8dbe-133">Führen Sie auf der Ebene des Projektstamms die folgenden Befehle im Fenster **Paket-Manager-Konsole** aus:</span><span class="sxs-lookup"><span data-stu-id="b8dbe-133">Run the following commands in the **Package Manager Console** window, from the project root:</span></span>

    ```console
    npm init -y
    npm install @aspnet/signalr
    ```

4. <span data-ttu-id="b8dbe-134">Erstellen Sie einen neuen Ordner mit dem Namen „signalr“ in Ihrem Projekt im Ordner *wwwroot/lib*.</span><span class="sxs-lookup"><span data-stu-id="b8dbe-134">Create a new folder named "signalr" inside the  *wwwroot/lib* folder in your project.</span></span> <span data-ttu-id="b8dbe-135">Kopieren Sie die Datei *signalr.js* aus *node_modules\\@aspnet\signalr\dist\browser* in diesen Ordner.</span><span class="sxs-lookup"><span data-stu-id="b8dbe-135">Copy the *signalr.js* file from *node_modules\\@aspnet\signalr\dist\browser* to this folder.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="b8dbe-136">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="b8dbe-136">Visual Studio Code</span></span>](#tab/visual-studio-code/)

1. <span data-ttu-id="b8dbe-137">Führen Sie über das **integrierte Terminal** den folgenden Befehl aus:</span><span class="sxs-lookup"><span data-stu-id="b8dbe-137">From the **Integrated Terminal**, run the following command:</span></span>

    ```console
    dotnet new webapp -o SignalRChat
    ```

    [!INCLUDE[](~/includes/webapp-alias-notice.md)]

2. <span data-ttu-id="b8dbe-138">Installieren Sie die JavaScript-Clientbibliothek mithilfe von *npm*.</span><span class="sxs-lookup"><span data-stu-id="b8dbe-138">Install the JavaScript client library using *npm*.</span></span>

    ```console
    npm init -y
    npm install @aspnet/signalr
    ```

3. <span data-ttu-id="b8dbe-139">Erstellen Sie einen neuen Ordner mit dem Namen „signalr“ im Ordner *lib* Ihres Projekts.</span><span class="sxs-lookup"><span data-stu-id="b8dbe-139">Create a new folder named "signalr" inside the  *lib* folder in your project.</span></span> <span data-ttu-id="b8dbe-140">Kopieren Sie die Datei *signalr.js* aus *node_modules\\@aspnet\signalr\dist\browser* in diesen Ordner.</span><span class="sxs-lookup"><span data-stu-id="b8dbe-140">Copy the *signalr.js* file from *node_modules\\@aspnet\signalr\dist\browser* to this folder.</span></span>

---

## <a name="create-the-signalr-hub"></a><span data-ttu-id="b8dbe-141">Erstellen des SignalR-Hubs</span><span class="sxs-lookup"><span data-stu-id="b8dbe-141">Create the SignalR Hub</span></span>

<span data-ttu-id="b8dbe-142">Ein Hub ist eine Klasse, die als grundlegende Pipeline verwendet wird. Mit einer solchen Pipeline können Client und Server Methoden der jeweils anderen Komponente aufrufen.</span><span class="sxs-lookup"><span data-stu-id="b8dbe-142">A hub is a class that serves as a high-level pipeline that allows the client and server to call methods on each other.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="b8dbe-143">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="b8dbe-143">Visual Studio</span></span>](#tab/visual-studio/)

1. <span data-ttu-id="b8dbe-144">Klicken Sie zuerst auf **Datei** > **Neu** > **Datei** und anschließend auf **Visual C#-Klasse**, um dem Projekt eine Klasse hinzuzufügen.</span><span class="sxs-lookup"><span data-stu-id="b8dbe-144">Add a class to the project by choosing **File** > **New** > **File** and selecting **Visual C# Class**.</span></span> <span data-ttu-id="b8dbe-145">Nennen Sie die Klasse `ChatHub`und die Datei *ChatHub.cs*.</span><span class="sxs-lookup"><span data-stu-id="b8dbe-145">Name the class `ChatHub` and the file *ChatHub.cs*.</span></span>

2. <span data-ttu-id="b8dbe-146">Stellen Sie sicher, dass Ihre Klasse von der Klasse `Microsoft.AspNetCore.SignalR.Hub` erbt.</span><span class="sxs-lookup"><span data-stu-id="b8dbe-146">Inherit from `Microsoft.AspNetCore.SignalR.Hub`.</span></span> <span data-ttu-id="b8dbe-147">Die `Hub`-Klasse enthält Eigenschaften und Ereignisse zur Verwaltung von Verbindungen und Gruppen sowie zum Senden und Empfangen von Daten.</span><span class="sxs-lookup"><span data-stu-id="b8dbe-147">The `Hub` class contains properties and events for managing connections and groups, as well as sending and receiving data.</span></span>

3. <span data-ttu-id="b8dbe-148">Erstellen Sie die Methode `SendMessage`. Diese sendet Nachrichten an alle verbundenen Chatclients.</span><span class="sxs-lookup"><span data-stu-id="b8dbe-148">Create the `SendMessage` method that sends a message to all connected chat clients.</span></span> <span data-ttu-id="b8dbe-149">Beachten Sie, dass diese ein [Task](https://msdn.microsoft.com/library/system.threading.tasks.task(v=vs.110).aspx)-Objekt zurückgibt, da SignalR asynchron arbeitet.</span><span class="sxs-lookup"><span data-stu-id="b8dbe-149">Notice it returns a [Task](https://msdn.microsoft.com/library/system.threading.tasks.task(v=vs.110).aspx), because SignalR is asynchronous.</span></span> <span data-ttu-id="b8dbe-150">Asynchroner Code erleichtert die Skalierung.</span><span class="sxs-lookup"><span data-stu-id="b8dbe-150">Asynchronous code scales better.</span></span>

   [!code-csharp[Startup](signalr/sample/Hubs/ChatHub.cs)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="b8dbe-151">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="b8dbe-151">Visual Studio Code</span></span>](#tab/visual-studio-code/)

1. <span data-ttu-id="b8dbe-152">Öffnen Sie in Visual Studio Code den Ordner *SignalRChat*.</span><span class="sxs-lookup"><span data-stu-id="b8dbe-152">Open the *SignalRChat* folder in Visual Studio Code.</span></span>

2. <span data-ttu-id="b8dbe-153">Klicken Sie im Menü auf **Datei** > **Neue Datei**, um dem Projekt eine Klasse hinzuzufügen.</span><span class="sxs-lookup"><span data-stu-id="b8dbe-153">Add a class to the project by selecting **File** > **New File** from the menu.</span></span> <span data-ttu-id="b8dbe-154">Nennen Sie die Klasse `ChatHub`und die Datei *ChatHub.cs*.</span><span class="sxs-lookup"><span data-stu-id="b8dbe-154">Name the class `ChatHub` and the file *ChatHub.cs*.</span></span>

3. <span data-ttu-id="b8dbe-155">Stellen Sie sicher, dass Ihre Klasse von der Klasse `Microsoft.AspNetCore.SignalR.Hub` erbt.</span><span class="sxs-lookup"><span data-stu-id="b8dbe-155">Inherit from `Microsoft.AspNetCore.SignalR.Hub`.</span></span> <span data-ttu-id="b8dbe-156">Die `Hub`-Klasse enthält Eigenschaften und Ereignisse zur Verwaltung von Verbindungen und Gruppen sowie zum Senden von Daten an Clients und zum Empfangen von Clientdaten.</span><span class="sxs-lookup"><span data-stu-id="b8dbe-156">The `Hub` class contains properties and events for managing connections and groups, as well as sending and receiving data to clients.</span></span>

4. <span data-ttu-id="b8dbe-157">Fügen Sie der Klasse eine `SendMessage`-Method hinzu.</span><span class="sxs-lookup"><span data-stu-id="b8dbe-157">Add a `SendMessage` method to the class.</span></span> <span data-ttu-id="b8dbe-158">Die `SendMessage`-Methode sendet Nachrichten an alle verbundenen Chatclients.</span><span class="sxs-lookup"><span data-stu-id="b8dbe-158">The `SendMessage` method sends a message to all connected chat clients.</span></span> <span data-ttu-id="b8dbe-159">Beachten Sie, dass diese ein [Task](/dotnet/api/system.threading.tasks.task)-Objekt zurückgibt, da SignalR asynchron arbeitet.</span><span class="sxs-lookup"><span data-stu-id="b8dbe-159">Notice it returns a [Task](/dotnet/api/system.threading.tasks.task), because SignalR is asynchronous.</span></span> <span data-ttu-id="b8dbe-160">Asynchroner Code erleichtert die Skalierung.</span><span class="sxs-lookup"><span data-stu-id="b8dbe-160">Asynchronous code scales better.</span></span>

   [!code-csharp[Startup](signalr/sample/Hubs/ChatHub.cs)]

-----

## <a name="configure-the-project-to-use-signalr"></a><span data-ttu-id="b8dbe-161">Konfigurieren des Projekts zur Verwendung von SignalR</span><span class="sxs-lookup"><span data-stu-id="b8dbe-161">Configure the project to use SignalR</span></span>

<span data-ttu-id="b8dbe-162">Der SignalR-Server muss zunächst konfiguriert werden, damit dieser Anforderungen an SignalR sendet.</span><span class="sxs-lookup"><span data-stu-id="b8dbe-162">The SignalR server must be configured so that it knows to pass requests to SignalR.</span></span>

1. <span data-ttu-id="b8dbe-163">Zum Konfigurieren eines SignalR-Projekts müssen Sie die Methode `Startup.ConfigureServices` des Projekts anpassen.</span><span class="sxs-lookup"><span data-stu-id="b8dbe-163">To configure a SignalR project, modify the project's `Startup.ConfigureServices` method.</span></span>

   <span data-ttu-id="b8dbe-164">`services.AddSignalR` stellt SignalR-Dienste für das [Dependency Injection](xref:fundamentals/dependency-injection)-System zur Verfügung.</span><span class="sxs-lookup"><span data-stu-id="b8dbe-164">`services.AddSignalR` makes the SignalR services available to the [dependency injection](xref:fundamentals/dependency-injection) system.</span></span>

1. <span data-ttu-id="b8dbe-165">Mit `UseSignalR` in der `Configure`-Methode konfigurieren Sie Routen zu Ihren Hubs.</span><span class="sxs-lookup"><span data-stu-id="b8dbe-165">Configure routes to your hubs with `UseSignalR` in the `Configure` method.</span></span> <span data-ttu-id="b8dbe-166">Durch `app.UseSignalR` wird SignalR der [Middlewarepipeline](xref:fundamentals/middleware/index) hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="b8dbe-166">`app.UseSignalR` adds SignalR to the [middleware](xref:fundamentals/middleware/index) pipeline.</span></span>

   [!code-csharp[Startup](signalr/sample/Startup.cs?highlight=37,57-60)]

## <a name="create-the-signalr-client-code"></a><span data-ttu-id="b8dbe-167">Erstellen des SignalR-Clientcodes</span><span class="sxs-lookup"><span data-stu-id="b8dbe-167">Create the SignalR client code</span></span>

1. <span data-ttu-id="b8dbe-168">Fügen Sie dem Ordner *wwwroot\js* eine JavaScript-Datei mit der Bezeichnung *chat.js* hinzu.</span><span class="sxs-lookup"><span data-stu-id="b8dbe-168">Add a JavaScript file, named *chat.js*, to the *wwwroot\js* folder.</span></span> <span data-ttu-id="b8dbe-169">Fügen Sie ihr folgenden Code hinzu:</span><span class="sxs-lookup"><span data-stu-id="b8dbe-169">Add the following code to it:</span></span>

   [!code-javascript[Index](signalr/sample/wwwroot/js/chat.js)]

2. <span data-ttu-id="b8dbe-170">Ersetzen Sie den Inhalt in *Pages\Index.cshtml* durch den folgenden Code:</span><span class="sxs-lookup"><span data-stu-id="b8dbe-170">Replace the content in *Pages\Index.cshtml* with the following code:</span></span>

   [!code-cshtml[Index](signalr/sample/Pages/Index.cshtml)]

   <span data-ttu-id="b8dbe-171">Durch den oben gezeigten HTML-Code werden Namens- und Nachrichtenfelder sowie eine Sendeschaltfläche angezeigt.</span><span class="sxs-lookup"><span data-stu-id="b8dbe-171">The preceding HTML displays name and message fields, and a submit button.</span></span> <span data-ttu-id="b8dbe-172">Beachten Sie, dass sich im unteren Skriptbereich ein Verweis auf SignalR und *chat.js* befindet.</span><span class="sxs-lookup"><span data-stu-id="b8dbe-172">Notice the script references at the bottom: a reference to SignalR and *chat.js*.</span></span>

## <a name="run-the-app"></a><span data-ttu-id="b8dbe-173">Ausführen der App</span><span class="sxs-lookup"><span data-stu-id="b8dbe-173">Run the app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="b8dbe-174">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="b8dbe-174">Visual Studio</span></span>](#tab/visual-studio)

1. <span data-ttu-id="b8dbe-175">Klicken Sie auf **Debuggen** > **Starten ohne Debuggen**, um einen Browser zu starten und die Website lokal zu laden.</span><span class="sxs-lookup"><span data-stu-id="b8dbe-175">Select **Debug** > **Start without debugging** to launch a browser and load the website locally.</span></span> <span data-ttu-id="b8dbe-176">Kopieren Sie die URL aus der Adressleiste.</span><span class="sxs-lookup"><span data-stu-id="b8dbe-176">Copy the URL from the address bar.</span></span>

1. <span data-ttu-id="b8dbe-177">Öffnen Sie eine andere Instanz eines beliebigen Browsers, und fügen Sie die URL in die Adressleiste ein.</span><span class="sxs-lookup"><span data-stu-id="b8dbe-177">Open another browser instance (any browser) and paste the URL in the address bar.</span></span>

1. <span data-ttu-id="b8dbe-178">Geben Sie in einem der beiden Browser einen Namen und eine Nachricht ein, und klicken sie auf **Send** (Senden).</span><span class="sxs-lookup"><span data-stu-id="b8dbe-178">Choose either browser, enter a name and message, and click the **Send** button.</span></span> <span data-ttu-id="b8dbe-179">Der Name und die Nachricht werden sofort auf beiden Seiten angezeigt.</span><span class="sxs-lookup"><span data-stu-id="b8dbe-179">The name and message are displayed on both pages instantly.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="b8dbe-180">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="b8dbe-180">Visual Studio Code</span></span>](#tab/visual-studio-code)

1. <span data-ttu-id="b8dbe-181">Drücken Sie **Debuggen** (F5), um das Programm zu erstellen und auszuführen.</span><span class="sxs-lookup"><span data-stu-id="b8dbe-181">Press **Debug** (F5) to build and run the program.</span></span> <span data-ttu-id="b8dbe-182">Durch das Ausführen des Programms wird ein Browserfenster geöffnet.</span><span class="sxs-lookup"><span data-stu-id="b8dbe-182">Running the program opens a browser window.</span></span>

1. <span data-ttu-id="b8dbe-183">Öffnen Sie ein anderes Browserfenster, und laden Sie in diesem die Website lokal.</span><span class="sxs-lookup"><span data-stu-id="b8dbe-183">Open another browser window and load the website locally in it.</span></span>

1. <span data-ttu-id="b8dbe-184">Geben Sie in einem der beiden Browser einen Namen und eine Nachricht ein, und klicken sie auf **Send** (Senden).</span><span class="sxs-lookup"><span data-stu-id="b8dbe-184">Choose either browser, enter a name and message, and click the **Send** button.</span></span> <span data-ttu-id="b8dbe-185">Der Name und die Nachricht werden sofort auf beiden Seiten angezeigt.</span><span class="sxs-lookup"><span data-stu-id="b8dbe-185">The name and message are displayed on both pages instantly.</span></span>

---

  ![Lösung](signalr/_static/signalr-get-started-finished.png)

## <a name="related-resources"></a><span data-ttu-id="b8dbe-187">Weitere Informationen</span><span class="sxs-lookup"><span data-stu-id="b8dbe-187">Related resources</span></span>

[<span data-ttu-id="b8dbe-188">Einführung in ASP.NET Core SignalR</span><span class="sxs-lookup"><span data-stu-id="b8dbe-188">Introduction to ASP.NET Core SignalR</span></span>](xref:signalr/introduction)
