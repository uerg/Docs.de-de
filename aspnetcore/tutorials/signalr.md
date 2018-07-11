---
title: Erste Schritte mit SignalR in ASP.NET Core
author: rachelappel
description: In diesem Tutorial erstellen Sie eine ASP.NET Core-App, die SignalR verwendet.
monikerRange: '>= aspnetcore-2.1'
ms.author: rachelap
ms.custom: mvc
ms.date: 05/22/2018
uid: tutorials/signalr
ms.openlocfilehash: 6b8222ee04573ca7157b4e1125ed5a4453b2b9a9
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2018
ms.locfileid: "37830554"
---
# <a name="get-started-with-signalr-on-aspnet-core"></a><span data-ttu-id="6a474-103">Erste Schritte mit SignalR in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="6a474-103">Get started with SignalR on ASP.NET Core</span></span>

<span data-ttu-id="6a474-104">Von [Rachel Appel](https://twitter.com/rachelappel)</span><span class="sxs-lookup"><span data-stu-id="6a474-104">By [Rachel Appel](https://twitter.com/rachelappel)</span></span>

<span data-ttu-id="6a474-105">In diesem Tutorial werden die Grundlagen zur Erstellung einer Echtzeit-App mit SignalR für ASP.NET Core beschrieben.</span><span class="sxs-lookup"><span data-stu-id="6a474-105">This tutorial teaches the basics of building a real-time app using SignalR for ASP.NET Core.</span></span>

   ![Lösung](signalr/_static/signalr-get-started-finished.png)

<span data-ttu-id="6a474-107">In diesem Tutorial werden die folgenden SignalR-Entwicklungsaufgaben veranschaulicht:</span><span class="sxs-lookup"><span data-stu-id="6a474-107">This tutorial demonstrates the following SignalR development tasks:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="6a474-108">Erstellen einer SignalR-Web-App für ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="6a474-108">Create a SignalR on ASP.NET Core web app.</span></span>
> * <span data-ttu-id="6a474-109">Erstellen eines SignalR-Hubs zur Übermittlung von Inhalten per Push an Clients</span><span class="sxs-lookup"><span data-stu-id="6a474-109">Create a SignalR hub to push content to clients.</span></span>
> * <span data-ttu-id="6a474-110">Anpassen der `Startup`-Klasse und Konfigurieren der App</span><span class="sxs-lookup"><span data-stu-id="6a474-110">Modify the `Startup` class and configure the app.</span></span>

<span data-ttu-id="6a474-111">[Anzeigen oder Herunterladen von Beispielcode](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/signalr/sample) ([Vorgehensweise zum Herunterladen](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="6a474-111">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/signalr/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="6a474-112">Erforderliche Komponenten</span><span class="sxs-lookup"><span data-stu-id="6a474-112">Prerequisites</span></span>

<span data-ttu-id="6a474-113">Installieren Sie die folgenden Softwarekomponenten:</span><span class="sxs-lookup"><span data-stu-id="6a474-113">Install the following software:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="6a474-114">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="6a474-114">Visual Studio</span></span>](#tab/visual-studio)

* [<span data-ttu-id="6a474-115">.NET Core SDK 2.1 oder höher</span><span class="sxs-lookup"><span data-stu-id="6a474-115">.NET Core SDK 2.1 or later</span></span>](https://www.microsoft.com/net/download/all)
* <span data-ttu-id="6a474-116">Version 15.7.3 oder höher von [Visual Studio 2017](https://www.visualstudio.com/downloads/) mit der Workload für **ASP.NET und Webentwicklung**</span><span class="sxs-lookup"><span data-stu-id="6a474-116">[Visual Studio 2017](https://www.visualstudio.com/downloads/) version 15.7.3 or later with the **ASP.NET and web development** workload</span></span>
* [<span data-ttu-id="6a474-117">npm</span><span class="sxs-lookup"><span data-stu-id="6a474-117">npm</span></span>](https://www.npmjs.com/get-npm)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="6a474-118">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="6a474-118">Visual Studio Code</span></span>](#tab/visual-studio-code)

* [<span data-ttu-id="6a474-119">.NET Core SDK 2.1 oder höher</span><span class="sxs-lookup"><span data-stu-id="6a474-119">.NET Core SDK 2.1 or later</span></span>](https://www.microsoft.com/net/download/all)
* [<span data-ttu-id="6a474-120">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="6a474-120">Visual Studio Code</span></span>](https://code.visualstudio.com/download)
* [<span data-ttu-id="6a474-121">C# für Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="6a474-121">C# for Visual Studio Code</span></span>](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)
* [<span data-ttu-id="6a474-122">npm</span><span class="sxs-lookup"><span data-stu-id="6a474-122">npm</span></span>](https://www.npmjs.com/get-npm)

-----

## <a name="create-an-aspnet-core-project-that-hosts-signalr-client-and-server"></a><span data-ttu-id="6a474-123">Erstellen eines ASP.NET Core-Projekts zum Hosten eines SignalR-Clients und -Servers</span><span class="sxs-lookup"><span data-stu-id="6a474-123">Create an ASP.NET Core project that hosts SignalR client and server</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="6a474-124">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="6a474-124">Visual Studio</span></span>](#tab/visual-studio/)

1. <span data-ttu-id="6a474-125">Klicken Sie zuerst auf die Menüoption **Datei** > **Neues Projekt** und anschließend auf **ASP.NET Core-Web-App**.</span><span class="sxs-lookup"><span data-stu-id="6a474-125">Use the **File** > **New Project** menu option and choose **ASP.NET Core Web Application**.</span></span> <span data-ttu-id="6a474-126">Geben Sie als Projektname *SignalRChat* ein.</span><span class="sxs-lookup"><span data-stu-id="6a474-126">Name the project *SignalRChat*.</span></span>

   ![Dialogfeld „Neues Projekt“ in Visual Studio](signalr/_static/signalr-new-project-dialog.png)

2. <span data-ttu-id="6a474-128">Klicken Sie auf **Webanwendung**, um ein Projekt mit Razor Pages zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="6a474-128">Select **Web Application** to create a project using Razor Pages.</span></span> <span data-ttu-id="6a474-129">Klicken Sie anschließend auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="6a474-129">Then select **OK**.</span></span> <span data-ttu-id="6a474-130">Obwohl SignalR auch unter älteren Versionen von .NET ausgeführt werden kann, sollte Sie darauf achten, **ASP.NET Core 2.1** als Framework aus der Dropdownliste auszuwählen.</span><span class="sxs-lookup"><span data-stu-id="6a474-130">Be sure that **ASP.NET Core 2.1** is selected from the framework selector, though SignalR runs on older versions of .NET.</span></span>

   ![Dialogfeld „Neues Projekt“ in Visual Studio](signalr/_static/signalr-new-project-choose-type.png)

<span data-ttu-id="6a474-132">Visual Studio enthält das `Microsoft.AspNetCore.SignalR`-Paket mit den zugehörigen Serverbibliotheken als Teil der **ASP.NET Core-Web-App**-Vorlage.</span><span class="sxs-lookup"><span data-stu-id="6a474-132">Visual Studio includes the `Microsoft.AspNetCore.SignalR` package containing its server libraries as part of its **ASP.NET Core Web Application** template.</span></span> <span data-ttu-id="6a474-133">Die JavaScript-Clientbibliothek für SignalR muss allerdings mit *npm* installiert werden.</span><span class="sxs-lookup"><span data-stu-id="6a474-133">However, the JavaScript client library for SignalR must be installed using *npm*.</span></span>

3. <span data-ttu-id="6a474-134">Führen Sie auf der Ebene des Projektstamms die folgenden Befehle im Fenster **Paket-Manager-Konsole** aus:</span><span class="sxs-lookup"><span data-stu-id="6a474-134">Run the following commands in the **Package Manager Console** window, from the project root:</span></span>

    ```console
    npm init -y
    npm install @aspnet/signalr
    ```

4. <span data-ttu-id="6a474-135">Erstellen Sie einen neuen Ordner mit dem Namen „signalr“ im Ordner *lib* Ihres Projekts.</span><span class="sxs-lookup"><span data-stu-id="6a474-135">Create a new folder named "signalr" inside the  *lib* folder in your project.</span></span> <span data-ttu-id="6a474-136">Kopieren Sie die Datei *signalr.js* aus *node_modules\\@aspnet\signalr\dist\browser* in diesen Ordner.</span><span class="sxs-lookup"><span data-stu-id="6a474-136">Copy the *signalr.js* file from *node_modules\\@aspnet\signalr\dist\browser* to this folder.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="6a474-137">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="6a474-137">Visual Studio Code</span></span>](#tab/visual-studio-code/)

1. <span data-ttu-id="6a474-138">Führen Sie über das **integrierte Terminal** den folgenden Befehl aus:</span><span class="sxs-lookup"><span data-stu-id="6a474-138">From the **Integrated Terminal**, run the following command:</span></span>

    ```console
    dotnet new webapp -o SignalRChat
    ```

    [!INCLUDE[](~/includes/webapp-alias-notice.md)]

2. <span data-ttu-id="6a474-139">Installieren Sie die JavaScript-Clientbibliothek mithilfe von *npm*.</span><span class="sxs-lookup"><span data-stu-id="6a474-139">Install the JavaScript client library using *npm*.</span></span>

    ```console
    npm init -y
    npm install @aspnet/signalr
    ```

3. <span data-ttu-id="6a474-140">Erstellen Sie einen neuen Ordner mit dem Namen „signalr“ im Ordner *lib* Ihres Projekts.</span><span class="sxs-lookup"><span data-stu-id="6a474-140">Create a new folder named "signalr" inside the  *lib* folder in your project.</span></span> <span data-ttu-id="6a474-141">Kopieren Sie die Datei *signalr.js* aus *node_modules\\@aspnet\signalr\dist\browser* in diesen Ordner.</span><span class="sxs-lookup"><span data-stu-id="6a474-141">Copy the *signalr.js* file from *node_modules\\@aspnet\signalr\dist\browser* to this folder.</span></span>

---

## <a name="create-the-signalr-hub"></a><span data-ttu-id="6a474-142">Erstellen des SignalR-Hubs</span><span class="sxs-lookup"><span data-stu-id="6a474-142">Create the SignalR Hub</span></span>

<span data-ttu-id="6a474-143">Ein Hub ist eine Klasse, die als grundlegende Pipeline verwendet wird. Mit einer solchen Pipeline können Client und Server Methoden der jeweils anderen Komponente aufrufen.</span><span class="sxs-lookup"><span data-stu-id="6a474-143">A hub is a class that serves as a high-level pipeline that allows the client and server to call methods on each other.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="6a474-144">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="6a474-144">Visual Studio</span></span>](#tab/visual-studio/)

1. <span data-ttu-id="6a474-145">Klicken Sie zuerst auf **Datei** > **Neu** > **Datei** und anschließend auf **Visual C#-Klasse**, um dem Projekt eine Klasse hinzuzufügen.</span><span class="sxs-lookup"><span data-stu-id="6a474-145">Add a class to the project by choosing **File** > **New** > **File** and selecting **Visual C# Class**.</span></span> <span data-ttu-id="6a474-146">Nennen Sie die Klasse `ChatHub`und die Datei *ChatHub.cs*.</span><span class="sxs-lookup"><span data-stu-id="6a474-146">Name the class `ChatHub` and the file *ChatHub.cs*.</span></span>

2. <span data-ttu-id="6a474-147">Stellen Sie sicher, dass Ihre Klasse von der Klasse `Microsoft.AspNetCore.SignalR.Hub` erbt.</span><span class="sxs-lookup"><span data-stu-id="6a474-147">Inherit from `Microsoft.AspNetCore.SignalR.Hub`.</span></span> <span data-ttu-id="6a474-148">Die `Hub`-Klasse enthält Eigenschaften und Ereignisse zur Verwaltung von Verbindungen und Gruppen sowie zum Senden und Empfangen von Daten.</span><span class="sxs-lookup"><span data-stu-id="6a474-148">The `Hub` class contains properties and events for managing connections and groups, as well as sending and receiving data.</span></span>

3. <span data-ttu-id="6a474-149">Erstellen Sie die Methode `SendMessage`. Diese sendet Nachrichten an alle verbundenen Chatclients.</span><span class="sxs-lookup"><span data-stu-id="6a474-149">Create the `SendMessage` method that sends a message to all connected chat clients.</span></span> <span data-ttu-id="6a474-150">Beachten Sie, dass diese ein [Task](https://msdn.microsoft.com/library/system.threading.tasks.task(v=vs.110).aspx)-Objekt zurückgibt, da SignalR asynchron arbeitet.</span><span class="sxs-lookup"><span data-stu-id="6a474-150">Notice it returns a [Task](https://msdn.microsoft.com/library/system.threading.tasks.task(v=vs.110).aspx), because SignalR is asynchronous.</span></span> <span data-ttu-id="6a474-151">Asynchroner Code erleichtert die Skalierung.</span><span class="sxs-lookup"><span data-stu-id="6a474-151">Asynchronous code scales better.</span></span>

   [!code-csharp[Startup](signalr/sample/Hubs/ChatHub.cs)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="6a474-152">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="6a474-152">Visual Studio Code</span></span>](#tab/visual-studio-code/)

1. <span data-ttu-id="6a474-153">Öffnen Sie in Visual Studio Code den Ordner *SignalRChat*.</span><span class="sxs-lookup"><span data-stu-id="6a474-153">Open the *SignalRChat* folder in Visual Studio Code.</span></span>

2. <span data-ttu-id="6a474-154">Klicken Sie im Menü auf **Datei** > **Neue Datei**, um dem Projekt eine Klasse hinzuzufügen.</span><span class="sxs-lookup"><span data-stu-id="6a474-154">Add a class to the project by selecting **File** > **New File** from the menu.</span></span> <span data-ttu-id="6a474-155">Nennen Sie die Klasse `ChatHub`und die Datei *ChatHub.cs*.</span><span class="sxs-lookup"><span data-stu-id="6a474-155">Name the class `ChatHub` and the file *ChatHub.cs*.</span></span>

3. <span data-ttu-id="6a474-156">Stellen Sie sicher, dass Ihre Klasse von der Klasse `Microsoft.AspNetCore.SignalR.Hub` erbt.</span><span class="sxs-lookup"><span data-stu-id="6a474-156">Inherit from `Microsoft.AspNetCore.SignalR.Hub`.</span></span> <span data-ttu-id="6a474-157">Die `Hub`-Klasse enthält Eigenschaften und Ereignisse zur Verwaltung von Verbindungen und Gruppen sowie zum Senden von Daten an Clients und zum Empfangen von Clientdaten.</span><span class="sxs-lookup"><span data-stu-id="6a474-157">The `Hub` class contains properties and events for managing connections and groups, as well as sending and receiving data to clients.</span></span>

4. <span data-ttu-id="6a474-158">Fügen Sie der Klasse eine `SendMessage`-Method hinzu.</span><span class="sxs-lookup"><span data-stu-id="6a474-158">Add a `SendMessage` method to the class.</span></span> <span data-ttu-id="6a474-159">Die `SendMessage`-Methode sendet Nachrichten an alle verbundenen Chatclients.</span><span class="sxs-lookup"><span data-stu-id="6a474-159">The `SendMessage` method sends a message to all connected chat clients.</span></span> <span data-ttu-id="6a474-160">Beachten Sie, dass diese ein [Task](/dotnet/api/system.threading.tasks.task)-Objekt zurückgibt, da SignalR asynchron arbeitet.</span><span class="sxs-lookup"><span data-stu-id="6a474-160">Notice it returns a [Task](/dotnet/api/system.threading.tasks.task), because SignalR is asynchronous.</span></span> <span data-ttu-id="6a474-161">Asynchroner Code erleichtert die Skalierung.</span><span class="sxs-lookup"><span data-stu-id="6a474-161">Asynchronous code scales better.</span></span>

   [!code-csharp[Startup](signalr/sample/Hubs/ChatHub.cs)]

-----

## <a name="configure-the-project-to-use-signalr"></a><span data-ttu-id="6a474-162">Konfigurieren des Projekts zur Verwendung von SignalR</span><span class="sxs-lookup"><span data-stu-id="6a474-162">Configure the project to use SignalR</span></span>

<span data-ttu-id="6a474-163">Der SignalR-Server muss zunächst konfiguriert werden, damit dieser Anforderungen an SignalR sendet.</span><span class="sxs-lookup"><span data-stu-id="6a474-163">The SignalR server must be configured so that it knows to pass requests to SignalR.</span></span>

1. <span data-ttu-id="6a474-164">Zum Konfigurieren eines SignalR-Projekts müssen Sie die Methode `Startup.ConfigureServices` des Projekts anpassen.</span><span class="sxs-lookup"><span data-stu-id="6a474-164">To configure a SignalR project, modify the project's `Startup.ConfigureServices` method.</span></span>

   <span data-ttu-id="6a474-165">`services.AddSignalR` stellt SignalR-Dienste für das [Dependency Injection](xref:fundamentals/dependency-injection)-System zur Verfügung.</span><span class="sxs-lookup"><span data-stu-id="6a474-165">`services.AddSignalR` makes the SignalR services available to the [dependency injection](xref:fundamentals/dependency-injection) system.</span></span>

1. <span data-ttu-id="6a474-166">Mit `UseSignalR` in der `Configure`-Methode konfigurieren Sie Routen zu Ihren Hubs.</span><span class="sxs-lookup"><span data-stu-id="6a474-166">Configure routes to your hubs with `UseSignalR` in the `Configure` method.</span></span> <span data-ttu-id="6a474-167">Durch `app.UseSignalR` wird SignalR der [Middlewarepipeline](xref:fundamentals/middleware/index) hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="6a474-167">`app.UseSignalR` adds SignalR to the [middleware](xref:fundamentals/middleware/index) pipeline.</span></span>

   [!code-csharp[Startup](signalr/sample/Startup.cs?highlight=37,57-60)]

## <a name="create-the-signalr-client-code"></a><span data-ttu-id="6a474-168">Erstellen des SignalR-Clientcodes</span><span class="sxs-lookup"><span data-stu-id="6a474-168">Create the SignalR client code</span></span>

1. <span data-ttu-id="6a474-169">Fügen Sie dem Ordner *wwwroot\js* eine JavaScript-Datei mit der Bezeichnung *chat.js* hinzu.</span><span class="sxs-lookup"><span data-stu-id="6a474-169">Add a JavaScript file, named *chat.js*, to the *wwwroot\js* folder.</span></span> <span data-ttu-id="6a474-170">Fügen Sie ihr folgenden Code hinzu:</span><span class="sxs-lookup"><span data-stu-id="6a474-170">Add the following code to it:</span></span>

   [!code-javascript[Index](signalr/sample/wwwroot/js/chat.js)]

2. <span data-ttu-id="6a474-171">Ersetzen Sie den Inhalt in *Pages\Index.cshtml* durch den folgenden Code:</span><span class="sxs-lookup"><span data-stu-id="6a474-171">Replace the content in *Pages\Index.cshtml* with the following code:</span></span>

   [!code-cshtml[Index](signalr/sample/Pages/Index.cshtml)]

   <span data-ttu-id="6a474-172">Durch den oben gezeigten HTML-Code werden Namens- und Nachrichtenfelder sowie eine Sendeschaltfläche angezeigt.</span><span class="sxs-lookup"><span data-stu-id="6a474-172">The preceding HTML displays name and message fields, and a submit button.</span></span> <span data-ttu-id="6a474-173">Beachten Sie, dass sich im unteren Skriptbereich ein Verweis auf SignalR und *chat.js* befindet.</span><span class="sxs-lookup"><span data-stu-id="6a474-173">Notice the script references at the bottom: a reference to SignalR and *chat.js*.</span></span>

## <a name="run-the-app"></a><span data-ttu-id="6a474-174">Ausführen der App</span><span class="sxs-lookup"><span data-stu-id="6a474-174">Run the app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="6a474-175">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="6a474-175">Visual Studio</span></span>](#tab/visual-studio)

1. <span data-ttu-id="6a474-176">Klicken Sie auf **Debuggen** > **Starten ohne Debuggen**, um einen Browser zu starten und die Website lokal zu laden.</span><span class="sxs-lookup"><span data-stu-id="6a474-176">Select **Debug** > **Start without debugging** to launch a browser and load the website locally.</span></span> <span data-ttu-id="6a474-177">Kopieren Sie die URL aus der Adressleiste.</span><span class="sxs-lookup"><span data-stu-id="6a474-177">Copy the URL from the address bar.</span></span>

1. <span data-ttu-id="6a474-178">Öffnen Sie eine andere Instanz eines beliebigen Browsers, und fügen Sie die URL in die Adressleiste ein.</span><span class="sxs-lookup"><span data-stu-id="6a474-178">Open another browser instance (any browser) and paste the URL in the address bar.</span></span>

1. <span data-ttu-id="6a474-179">Geben Sie in einem der beiden Browser einen Namen und eine Nachricht ein, und klicken sie auf **Send** (Senden).</span><span class="sxs-lookup"><span data-stu-id="6a474-179">Choose either browser, enter a name and message, and click the **Send** button.</span></span> <span data-ttu-id="6a474-180">Der Name und die Nachricht werden sofort auf beiden Seiten angezeigt.</span><span class="sxs-lookup"><span data-stu-id="6a474-180">The name and message are displayed on both pages instantly.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="6a474-181">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="6a474-181">Visual Studio Code</span></span>](#tab/visual-studio-code)

1. <span data-ttu-id="6a474-182">Drücken Sie **Debuggen** (F5), um das Programm zu erstellen und auszuführen.</span><span class="sxs-lookup"><span data-stu-id="6a474-182">Press **Debug** (F5) to build and run the program.</span></span> <span data-ttu-id="6a474-183">Durch das Ausführen des Programms wird ein Browserfenster geöffnet.</span><span class="sxs-lookup"><span data-stu-id="6a474-183">Running the program opens a browser window.</span></span>

1. <span data-ttu-id="6a474-184">Öffnen Sie ein anderes Browserfenster, und laden Sie in diesem die Website lokal.</span><span class="sxs-lookup"><span data-stu-id="6a474-184">Open another browser window and load the website locally in it.</span></span>

1. <span data-ttu-id="6a474-185">Geben Sie in einem der beiden Browser einen Namen und eine Nachricht ein, und klicken sie auf **Send** (Senden).</span><span class="sxs-lookup"><span data-stu-id="6a474-185">Choose either browser, enter a name and message, and click the **Send** button.</span></span> <span data-ttu-id="6a474-186">Der Name und die Nachricht werden sofort auf beiden Seiten angezeigt.</span><span class="sxs-lookup"><span data-stu-id="6a474-186">The name and message are displayed on both pages instantly.</span></span>

---

  ![Lösung](signalr/_static/signalr-get-started-finished.png)

## <a name="related-resources"></a><span data-ttu-id="6a474-188">Weitere Informationen</span><span class="sxs-lookup"><span data-stu-id="6a474-188">Related resources</span></span>

[<span data-ttu-id="6a474-189">Einführung in ASP.NET Core SignalR</span><span class="sxs-lookup"><span data-stu-id="6a474-189">Introduction to ASP.NET Core SignalR</span></span>](xref:signalr/introduction)
