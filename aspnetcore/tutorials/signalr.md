---
title: 'Tutorial: Erste Schritte mit SignalR unter ASP.NET Core'
author: tdykstra
description: In diesem Tutorial erstellen Sie eine Chat-App, die SignalR für ASP.NET Core verwendet.
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 08/20/2018
uid: tutorials/signalr
ms.openlocfilehash: a2573e2817a2d8921954264ca17bc3a7e2a010a8
ms.sourcegitcommit: 847cc1de5526ff42a7303491e6336c2dbdb45de4
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/27/2018
ms.locfileid: "43055831"
---
# <a name="tutorial-get-started-with-signalr-on-aspnet-core"></a><span data-ttu-id="8e2a6-103">Tutorial: Erste Schritte mit SignalR unter ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="8e2a6-103">Tutorial: Get started with SignalR on ASP.NET Core</span></span>

<span data-ttu-id="8e2a6-104">In diesem Tutorial werden die Grundlagen zur Erstellung einer Echtzeit-App mit SignalR beschrieben.</span><span class="sxs-lookup"><span data-stu-id="8e2a6-104">This tutorial teaches the basics of building a real-time app using SignalR.</span></span> <span data-ttu-id="8e2a6-105">Sie lernen Folgendes:</span><span class="sxs-lookup"><span data-stu-id="8e2a6-105">You learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="8e2a6-106">Erstellen einer Web-App, die SignalR für ASP.NET Core verwendet</span><span class="sxs-lookup"><span data-stu-id="8e2a6-106">Create a web app that uses SignalR on ASP.NET Core.</span></span>
> * <span data-ttu-id="8e2a6-107">Erstellen eines SignalR-Hubs auf dem Server</span><span class="sxs-lookup"><span data-stu-id="8e2a6-107">Create a SignalR hub on the server.</span></span>
> * <span data-ttu-id="8e2a6-108">Herstellen einer Verbindung zwischen JavaScript-Clients und dem SignalR-Hub</span><span class="sxs-lookup"><span data-stu-id="8e2a6-108">Connect to the SignalR hub from JavaScript clients.</span></span>
> * <span data-ttu-id="8e2a6-109">Verwenden des Hubs zum Senden von Nachrichten von jedem Client an alle verbundenen Clients</span><span class="sxs-lookup"><span data-stu-id="8e2a6-109">Use the hub to send messages from any client to all connected clients.</span></span>

<span data-ttu-id="8e2a6-110">Am Ende verfügen Sie über eine funktionierende Chat-App:</span><span class="sxs-lookup"><span data-stu-id="8e2a6-110">At the end, you'll have a working chat app:</span></span>

![SignalR-Beispiel-App](signalr/_static/signalr-get-started-finished.png)

<span data-ttu-id="8e2a6-112">[Zeigen Sie Beispielcode an, oder laden Sie diesen herunter](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/signalr/sample) ([Vorgehensweise zum Herunterladen](xref:tutorials/index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="8e2a6-112">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/signalr/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample)).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="8e2a6-113">Erforderliche Komponenten</span><span class="sxs-lookup"><span data-stu-id="8e2a6-113">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="8e2a6-114">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="8e2a6-114">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="8e2a6-115">[Version 15.7.3 oder höher von Visual Studio 2017](https://www.visualstudio.com/downloads/) mit der Workload für **ASP.NET und Webentwicklung**</span><span class="sxs-lookup"><span data-stu-id="8e2a6-115">[Visual Studio 2017 version 15.7.3 or later](https://www.visualstudio.com/downloads/) with the **ASP.NET and web development** workload</span></span>
* [<span data-ttu-id="8e2a6-116">.NET Core SDK 2.1 oder höher</span><span class="sxs-lookup"><span data-stu-id="8e2a6-116">.NET Core SDK 2.1 or later</span></span>](https://www.microsoft.com/net/download/all)
* <span data-ttu-id="8e2a6-117">[npm](https://www.npmjs.com/get-npm) (Paket-Manager für Node.js, der für die JavaScript-Clientbibliothek für SignalR verwendet wird.)</span><span class="sxs-lookup"><span data-stu-id="8e2a6-117">[npm](https://www.npmjs.com/get-npm) (Package manager for Node.js, used for the SignalR JavaScript client library.)</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="8e2a6-118">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="8e2a6-118">Visual Studio Code</span></span>](#tab/visual-studio-code)

* [<span data-ttu-id="8e2a6-119">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="8e2a6-119">Visual Studio Code</span></span>](https://code.visualstudio.com/download)
* [<span data-ttu-id="8e2a6-120">.NET Core SDK 2.1 oder höher</span><span class="sxs-lookup"><span data-stu-id="8e2a6-120">.NET Core SDK 2.1 or later</span></span>](https://www.microsoft.com/net/download/all)
* [<span data-ttu-id="8e2a6-121">C# für Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="8e2a6-121">C# for Visual Studio Code</span></span>](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)
* <span data-ttu-id="8e2a6-122">[npm](https://www.npmjs.com/get-npm) (Paket-Manager für Node.js, der für die JavaScript-Clientbibliothek für SignalR verwendet wird.)</span><span class="sxs-lookup"><span data-stu-id="8e2a6-122">[npm](https://www.npmjs.com/get-npm) (Package manager for Node.js, used for the SignalR JavaScript client library.)</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="8e2a6-123">Visual Studio für Mac</span><span class="sxs-lookup"><span data-stu-id="8e2a6-123">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* [<span data-ttu-id="8e2a6-124">Visual Studio für Mac Version 7.5.4 oder höher</span><span class="sxs-lookup"><span data-stu-id="8e2a6-124">Visual Studio for Mac version 7.5.4 or later</span></span>](https://www.visualstudio.com/downloads/)
* <span data-ttu-id="8e2a6-125">[.NET Core SDK 2.1 oder höher](https://www.microsoft.com/net/download/all) (in der Visual Studio-Installation enthalten)</span><span class="sxs-lookup"><span data-stu-id="8e2a6-125">[.NET Core SDK 2.1 or later](https://www.microsoft.com/net/download/all) (included in the Visual Studio install)</span></span>
* <span data-ttu-id="8e2a6-126">[npm](https://www.npmjs.com/get-npm) (Paket-Manager für Node.js, der für die JavaScript-Clientbibliothek für SignalR verwendet wird.)</span><span class="sxs-lookup"><span data-stu-id="8e2a6-126">[npm](https://www.npmjs.com/get-npm) (Package manager for Node.js, used for the SignalR JavaScript client library.)</span></span>

---

## <a name="create-the-project"></a><span data-ttu-id="8e2a6-127">Erstellen eines Projekts</span><span class="sxs-lookup"><span data-stu-id="8e2a6-127">Create the project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="8e2a6-128">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="8e2a6-128">Visual Studio</span></span>](#tab/visual-studio/)

* <span data-ttu-id="8e2a6-129">Klicken Sie im Menü auf **Datei > Neues Projekt**.</span><span class="sxs-lookup"><span data-stu-id="8e2a6-129">From the menu, select **File > New Project**.</span></span>

* <span data-ttu-id="8e2a6-130">Wählen Sie im Dialogfeld **Neues Projekt** **Installiert > Visual C# > Web > ASP.NET Core-Webanwendung** aus.</span><span class="sxs-lookup"><span data-stu-id="8e2a6-130">In the **New Project** dialog, select **Installed > Visual C# > Web > ASP.NET Core Web Application**.</span></span> <span data-ttu-id="8e2a6-131">Geben Sie als Projektname *SignalRChat* ein.</span><span class="sxs-lookup"><span data-stu-id="8e2a6-131">Name the project *SignalRChat*.</span></span>

  ![Dialogfeld „Neues Projekt“ in Visual Studio](signalr/_static/signalr-new-project-dialog.png)

* <span data-ttu-id="8e2a6-133">Klicken Sie auf **Webanwendung**, um ein Projekt zu erstellen, das Razor Pages verwendet.</span><span class="sxs-lookup"><span data-stu-id="8e2a6-133">Select **Web Application** to create a project that uses Razor Pages.</span></span>

* <span data-ttu-id="8e2a6-134">Wählen Sie ein Zielframework von **.NET Core** aus, und klicken Sie anschließend auf **ASP.NET Core 2.1** und **OK**.</span><span class="sxs-lookup"><span data-stu-id="8e2a6-134">Select a target framework of **.NET Core**, select **ASP.NET Core 2.1**, and click **OK**.</span></span>

  ![Dialogfeld „Neues Projekt“ in Visual Studio](signalr/_static/signalr-new-project-choose-type.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="8e2a6-136">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="8e2a6-136">Visual Studio Code</span></span>](#tab/visual-studio-code/)

* <span data-ttu-id="8e2a6-137">Öffnen Sie einen Ordner, den Sie für ein neues Projekt verwenden können.</span><span class="sxs-lookup"><span data-stu-id="8e2a6-137">Open a folder that you can use for a new project.</span></span>

* <span data-ttu-id="8e2a6-138">Führen Sie über das **integrierte Terminal** den folgenden Befehl aus:</span><span class="sxs-lookup"><span data-stu-id="8e2a6-138">In the **Integrated Terminal**, run the following command:</span></span>

   ```console
   dotnet new webapp -o SignalRChat
   ```

   [!INCLUDE[](~/includes/webapp-alias-notice.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="8e2a6-139">Visual Studio für Mac</span><span class="sxs-lookup"><span data-stu-id="8e2a6-139">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="8e2a6-140">Klicken Sie im Menü auf **Datei > Neue Projektmappe**.</span><span class="sxs-lookup"><span data-stu-id="8e2a6-140">From the menu, select **File > New Solution**.</span></span>

* <span data-ttu-id="8e2a6-141">Klicken Sie auf **.NET Core > App > ASP.NET Core-Web-App** (Wählen Sie nicht **ASP.NET Core-Web-App (MVC)** aus).</span><span class="sxs-lookup"><span data-stu-id="8e2a6-141">Select **.NET Core > App > ASP.NET Core Web App** (Don't select **ASP.NET Core Web App (MVC)**).</span></span>

* <span data-ttu-id="8e2a6-142">Klicken Sie auf **Weiter**.</span><span class="sxs-lookup"><span data-stu-id="8e2a6-142">Select **Next**.</span></span>

* <span data-ttu-id="8e2a6-143">Nennen Sie das Projekt *SignalRChat*, und wählen Sie dann **Erstellen** aus.</span><span class="sxs-lookup"><span data-stu-id="8e2a6-143">Name the project *SignalRChat*, and then select **Create**.</span></span>

---

## <a name="add-the-signalr-client-library"></a><span data-ttu-id="8e2a6-144">Hinzufügen der SignalR-Clientbibliothek</span><span class="sxs-lookup"><span data-stu-id="8e2a6-144">Add the SignalR client library</span></span>

<span data-ttu-id="8e2a6-145">Die SignalR-Serverbibliothek ist im [Metapaket Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) enthalten.</span><span class="sxs-lookup"><span data-stu-id="8e2a6-145">The SignalR server library is included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span> <span data-ttu-id="8e2a6-146">Sie müssen die JavaScript-Clientbibliothek jedoch über [npm, den Node.js-Paket-Manager](https://www.npmjs.com/get-npm) abrufen.</span><span class="sxs-lookup"><span data-stu-id="8e2a6-146">But you have to get the JavaScript client library from [npm, the Node.js package manager](https://www.npmjs.com/get-npm).</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="8e2a6-147">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="8e2a6-147">Visual Studio</span></span>](#tab/visual-studio/)

* <span data-ttu-id="8e2a6-148">Wechseln Sie in der **Paket-Manager-Konsole** (PMC) zum Projektordner (derjenige, der die Datei *SignalRChat.csproj* enthält).</span><span class="sxs-lookup"><span data-stu-id="8e2a6-148">In **Package Manager Console** (PMC), change to the project folder (the one that contains the *SignalRChat.csproj* file).</span></span>

  ```console
  cd SignalRChat
  ```

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="8e2a6-149">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="8e2a6-149">Visual Studio Code</span></span>](#tab/visual-studio-code/)

2. <span data-ttu-id="8e2a6-150">Wechseln Sie zum neuen Projektordner.</span><span class="sxs-lookup"><span data-stu-id="8e2a6-150">Change to the new project folder.</span></span>

  ```console
  cd SignalRChat
  ``` 

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="8e2a6-151">Visual Studio für Mac</span><span class="sxs-lookup"><span data-stu-id="8e2a6-151">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="8e2a6-152">Navigieren Sie im **Terminal** zum Projektordner (derjenige, der die Datei \*SignalRChat.csproj enthält).</span><span class="sxs-lookup"><span data-stu-id="8e2a6-152">In the **Terminal**, navigate to the project folder (the one that contains the *SignalRChat.csproj* file).</span></span>

---

* <span data-ttu-id="8e2a6-153">Führen Sie den npm-Initialisierer aus, um eine *package.json*-Datei zu erstellen:</span><span class="sxs-lookup"><span data-stu-id="8e2a6-153">Run the npm initializer to create a *package.json* file:</span></span>

  ```console
  npm init -y
  ```

  <span data-ttu-id="8e2a6-154">Dieser Befehl erzeugt eine Ausgabe wie die folgende:</span><span class="sxs-lookup"><span data-stu-id="8e2a6-154">The command creates output similar to the following example:</span></span>

  ```console
  Wrote to C:\tmp\SignalRChat\package.json:
  {
    "name": "SignalRChat",
    "version": "1.0.0",
    "description": "",
    "main": "index.js",
    "scripts": {
      "test": "echo \"Error: no test specified\" && exit 1"
    },
    "keywords": [],
    "author": "",
    "license": "ISC"0
  }
  ```

* <span data-ttu-id="8e2a6-155">Installieren Sie das Clientbibliothekspaket:</span><span class="sxs-lookup"><span data-stu-id="8e2a6-155">Install the client library package:</span></span>

  ```console
  npm install @aspnet/signalr
  ```

  <span data-ttu-id="8e2a6-156">Dieser Befehl erzeugt eine Ausgabe wie die folgende:</span><span class="sxs-lookup"><span data-stu-id="8e2a6-156">The command creates output similar to the following example:</span></span>

  ```
  npm notice created a lockfile as package-lock.json. You should commit this file.
  npm WARN signalrchat@1.0.0 No description
  npm WARN signalrchat@1.0.0 No repository field.

  + @aspnet/signalr@1.0.2
  added 1 package in 0.98s
  ```

<span data-ttu-id="8e2a6-157">Über den `npm install`-Befehl wurde die JavaScript-Clientbibliothek in einen Unterordner unter *node_modules* geladen.</span><span class="sxs-lookup"><span data-stu-id="8e2a6-157">The `npm install` command downloaded the JavaScript client library to a subfolder under *node_modules*.</span></span> <span data-ttu-id="8e2a6-158">Kopieren Sie sie von dort in einen Ordner unter *wwwroot*, auf den Sie von der Chat-App-Webseite verweisen können.</span><span class="sxs-lookup"><span data-stu-id="8e2a6-158">Copy it from there to a folder under *wwwroot* that you can reference from the chat app web page.</span></span>

* <span data-ttu-id="8e2a6-159">Erstellen Sie einen *signalr*-Ordner unter *wwwroot/lib*.</span><span class="sxs-lookup"><span data-stu-id="8e2a6-159">Create a *signalr* folder in *wwwroot/lib*.</span></span>

* <span data-ttu-id="8e2a6-160">Kopieren Sie die *signalr.js*-Datei von *node_modules/@aspnet/signalr/dist/browser* in den neuen *wwwroot/lib/signalr*-Ordner.</span><span class="sxs-lookup"><span data-stu-id="8e2a6-160">Copy the *signalr.js* file from *node_modules/@aspnet/signalr/dist/browser* to the new *wwwroot/lib/signalr* folder.</span></span>

## <a name="create-the-signalr-hub"></a><span data-ttu-id="8e2a6-161">Erstellen des SignalR-Hubs</span><span class="sxs-lookup"><span data-stu-id="8e2a6-161">Create the SignalR hub</span></span>

<span data-ttu-id="8e2a6-162">Ein [Hub](xref:signalr/hubs) ist eine Klasse, die als grundlegende Pipeline verwendet wird, mit der Client/Server-Kommunikation verarbeitet wird.</span><span class="sxs-lookup"><span data-stu-id="8e2a6-162">A [hub](xref:signalr/hubs) is a class that serves as a high-level pipeline that handles client-server communication.</span></span>

* <span data-ttu-id="8e2a6-163">Erstellen Sie im SignalRChat-Projektordner einen *Hubs*-Ordner.</span><span class="sxs-lookup"><span data-stu-id="8e2a6-163">In the SignalRChat project folder, create a *Hubs* folder.</span></span>

* <span data-ttu-id="8e2a6-164">Erstellen Sie im *Hubs*-Ordner eine *ChatHub.cs*-Datei mit folgendem Code:</span><span class="sxs-lookup"><span data-stu-id="8e2a6-164">In the *Hubs* folder, create a *ChatHub.cs* file with the following code:</span></span>

  [!code-csharp[Startup](signalr/sample/Hubs/ChatHub.cs)]

  <span data-ttu-id="8e2a6-165">Die `ChatHub`-Klasse erbt von der [Hub](/dotnet/api/microsoft.aspnetcore.signalr.hub)-Klasse von SignalR.</span><span class="sxs-lookup"><span data-stu-id="8e2a6-165">The `ChatHub` class inherits from the SignalR [Hub](/dotnet/api/microsoft.aspnetcore.signalr.hub) class.</span></span> <span data-ttu-id="8e2a6-166">Die `Hub`-Klasse verwaltet Verbindungen, Gruppen und Messaging.</span><span class="sxs-lookup"><span data-stu-id="8e2a6-166">The `Hub` class manages connections, groups, and messaging.</span></span>

  <span data-ttu-id="8e2a6-167">Die `SendMessage`-Methode kann von jedem verbundenen Client aufgerufen werden.</span><span class="sxs-lookup"><span data-stu-id="8e2a6-167">The `SendMessage` method can be called by any connected client.</span></span> <span data-ttu-id="8e2a6-168">Die empfangenen Nachrichten werden an alle Clients gesendet.</span><span class="sxs-lookup"><span data-stu-id="8e2a6-168">It sends the received message to all clients.</span></span> <span data-ttu-id="8e2a6-169">SignalR-Code ist asynchron, damit die maximale Skalierbarkeit gewährleistet werden kann.</span><span class="sxs-lookup"><span data-stu-id="8e2a6-169">SignalR code is asynchronous to provide maximum scalability.</span></span>

## <a name="configure-the-project-to-use-signalr"></a><span data-ttu-id="8e2a6-170">Konfigurieren des Projekts zur Verwendung von SignalR</span><span class="sxs-lookup"><span data-stu-id="8e2a6-170">Configure the project to use SignalR</span></span>

<span data-ttu-id="8e2a6-171">Der SignalR-Server muss zunächst konfiguriert werden, um Anforderungen an SignalR zu senden.</span><span class="sxs-lookup"><span data-stu-id="8e2a6-171">The SignalR server must be configured to pass SignalR requests to SignalR.</span></span>

* <span data-ttu-id="8e2a6-172">Fügen Sie der *Startup.cs*-Datei den folgenden hervorgehobenen Code zu.</span><span class="sxs-lookup"><span data-stu-id="8e2a6-172">Add the following highlighted code to the *Startup.cs* file.</span></span>

  [!code-csharp[Startup](signalr/sample/Startup.cs?highlight=7,33,52-55)]

  <span data-ttu-id="8e2a6-173">Durch diese Änderungen wird SignalR zum [Dependency Injection](xref:fundamentals/dependency-injection)-System sowie der [Middlewarepipeline](xref:fundamentals/middleware/index) hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="8e2a6-173">These changes add SignalR to the [dependency injection](xref:fundamentals/dependency-injection) system and the [middleware](xref:fundamentals/middleware/index) pipeline.</span></span>

## <a name="create-the-signalr-client-code"></a><span data-ttu-id="8e2a6-174">Erstellen des SignalR-Clientcodes</span><span class="sxs-lookup"><span data-stu-id="8e2a6-174">Create the SignalR client code</span></span>

* <span data-ttu-id="8e2a6-175">Ersetzen Sie den Inhalt in *Pages\Index.cshtml* durch Folgendes:</span><span class="sxs-lookup"><span data-stu-id="8e2a6-175">Replace the content in *Pages\Index.cshtml* with the following:</span></span>

  [!code-cshtml[Index](signalr/sample/Pages/Index.cshtml)]

  <span data-ttu-id="8e2a6-176">Der vorangehende Code:</span><span class="sxs-lookup"><span data-stu-id="8e2a6-176">The preceding code:</span></span>

  * <span data-ttu-id="8e2a6-177">erstellt Textfelder für den Namen und den Nachrichtentext sowie eine Senden-Schaltfläche</span><span class="sxs-lookup"><span data-stu-id="8e2a6-177">Creates text boxes for name and message text, and a submit button.</span></span>
  * <span data-ttu-id="8e2a6-178">erstellt eine Liste mit `id="messagesList"` zum Anzeigen von Nachrichten, die vom SignalR-Hub empfangen werden</span><span class="sxs-lookup"><span data-stu-id="8e2a6-178">Creates a list with `id="messagesList"` for displaying messages that are received from the SignalR hub.</span></span>
  * <span data-ttu-id="8e2a6-179">schließt Skriptverweise sowie den *chat.js*-Anwendungscode, den Sie im folgenden Schritt erstellen, in SignalR ein</span><span class="sxs-lookup"><span data-stu-id="8e2a6-179">Includes script references to SignalR and the *chat.js* application code that you create in the next step.</span></span>

* <span data-ttu-id="8e2a6-180">Erstellen Sie im *wwwroot/js*-Ordner eine *chat.js*-Datei mit folgendem Code:</span><span class="sxs-lookup"><span data-stu-id="8e2a6-180">In the *wwwroot/js* folder, create a *chat.js* file with the following code:</span></span>

  [!code-javascript[Index](signalr/sample/wwwroot/js/chat.js)]

  <span data-ttu-id="8e2a6-181">Der vorangehende Code:</span><span class="sxs-lookup"><span data-stu-id="8e2a6-181">The preceding code:</span></span>

  * <span data-ttu-id="8e2a6-182">erstellt und startet eine Verbindung</span><span class="sxs-lookup"><span data-stu-id="8e2a6-182">Creates and starts a connection.</span></span>
  * <span data-ttu-id="8e2a6-183">fügt einen Handler zur Senden-Schaltfläche hinzu, der Nachrichten an den Hub sendet</span><span class="sxs-lookup"><span data-stu-id="8e2a6-183">Adds to the submit button a handler that sends messages to the hub.</span></span>
  * <span data-ttu-id="8e2a6-184">fügt einen Handler zum Verbindungsobjekt hinzu, der Nachrichten vom Hub empfängt und diese der Liste hinzufügt</span><span class="sxs-lookup"><span data-stu-id="8e2a6-184">Adds to the connection object a handler that receives messages from the hub and adds them to the list.</span></span>

## <a name="run-the-app"></a><span data-ttu-id="8e2a6-185">Ausführen der App</span><span class="sxs-lookup"><span data-stu-id="8e2a6-185">Run the app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="8e2a6-186">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="8e2a6-186">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="8e2a6-187">Drücken Sie **STRG+F5**, um die App ohne Debugging zu starten.</span><span class="sxs-lookup"><span data-stu-id="8e2a6-187">Press **CTRL+F5** to run the app without debugging.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="8e2a6-188">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="8e2a6-188">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="8e2a6-189">Drücken Sie **STRG+F5**, um die App ohne Debugging zu starten.</span><span class="sxs-lookup"><span data-stu-id="8e2a6-189">Press **CTRL+F5** to run the app without debugging.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="8e2a6-190">Visual Studio für Mac</span><span class="sxs-lookup"><span data-stu-id="8e2a6-190">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="8e2a6-191">Wählen Sie im Menü **Ausführen > Starten ohne Debuggen** aus.</span><span class="sxs-lookup"><span data-stu-id="8e2a6-191">From the menu, select **Run > Start Without Debugging**.</span></span>

---

* <span data-ttu-id="8e2a6-192">Kopieren Sie die URL aus der Adressleiste, öffnen Sie eine neue Browserinstanz oder Registerkarte, und fügen Sie die URL in die Adressleiste ein.</span><span class="sxs-lookup"><span data-stu-id="8e2a6-192">Copy the URL from the address bar, open another browser instance or tab, and paste the URL in the address bar.</span></span>

* <span data-ttu-id="8e2a6-193">Geben Sie in einem der beiden Browser einen Namen und eine Nachricht ein, und klicken sie auf **Send** (Senden).</span><span class="sxs-lookup"><span data-stu-id="8e2a6-193">Choose either browser, enter a name and message, and select the **Send** button.</span></span>

  <span data-ttu-id="8e2a6-194">Der Name und die Nachricht werden sofort auf beiden Seiten angezeigt.</span><span class="sxs-lookup"><span data-stu-id="8e2a6-194">The name and message are displayed on both pages instantly.</span></span>

  ![SignalR-Beispiel-App](signalr/_static/signalr-get-started-finished.png)

> [!TIP]
> <span data-ttu-id="8e2a6-196">Wenn die App nicht funktioniert, öffnen Sie die Browser-Entwicklungstools (F12), und wechseln Sie zur Konsole.</span><span class="sxs-lookup"><span data-stu-id="8e2a6-196">If the app doesn't work, open your browser developer tools (F12) and go to the console.</span></span> <span data-ttu-id="8e2a6-197">Möglicherweise werden Fehler in Bezug auf Ihren HTML- und JavaScript-Code angezeigt.</span><span class="sxs-lookup"><span data-stu-id="8e2a6-197">You might see errors related to your HTML and JavaScript code.</span></span> <span data-ttu-id="8e2a6-198">Nehmen wir an, dass Sie z.B. *signalr.js* in einen anderen Ordner als vorgeschrieben platziert haben.</span><span class="sxs-lookup"><span data-stu-id="8e2a6-198">For example, suppose you put *signalr.js* in a different folder than directed.</span></span> <span data-ttu-id="8e2a6-199">In diesem Fall funktioniert der Verweis auf diese Datei nicht, und Ihnen wird der Fehler 404 in der Konsole angezeigt.</span><span class="sxs-lookup"><span data-stu-id="8e2a6-199">In that case the reference to that file won't work and you'll see a 404 error in the console.</span></span>
> <span data-ttu-id="8e2a6-200">![Fehler „signalr.js nicht gefunden“](signalr/_static/f12-console.png)</span><span class="sxs-lookup"><span data-stu-id="8e2a6-200">![signalr.js not found error](signalr/_static/f12-console.png)</span></span>

## <a name="next-steps"></a><span data-ttu-id="8e2a6-201">Nächste Schritte</span><span class="sxs-lookup"><span data-stu-id="8e2a6-201">Next steps</span></span>

<span data-ttu-id="8e2a6-202">Wenn Sie möchten, dass Clients eine Verbindung mit einer SignalR-App von unterschiedlichen Domänen herstellt, müssen Sie die Ressourcenfreigabe zwischen verschiedenen Ursprüngen aktivieren (Cross-Origin Resource Sharing, CORS).</span><span class="sxs-lookup"><span data-stu-id="8e2a6-202">If you want clients to connect to a SignalR app from different domains, you have to enable Cross-Origin Resource Sharing (CORS).</span></span> <span data-ttu-id="8e2a6-203">Weiter Informationen finden Sie unter [Cross-origin resource sharing (Ressourcenfreigabe zwischen verschiedenen Ursprüngen)](xref:signalr/security?view=aspnetcore-2.1#cross-origin-resource-sharing).</span><span class="sxs-lookup"><span data-stu-id="8e2a6-203">For more information, see [Cross-origin resource sharing](xref:signalr/security?view=aspnetcore-2.1#cross-origin-resource-sharing).</span></span>

<span data-ttu-id="8e2a6-204">Weitere Informationen zu SignalR, Hubs sowie JavaScript-Clients finden Sie in folgenden Artikeln:</span><span class="sxs-lookup"><span data-stu-id="8e2a6-204">To learn more about SignalR, hubs, and JavaScript clients, see these resources:</span></span>

* [<span data-ttu-id="8e2a6-205">Introduction to SignalR for ASP.NET Core (Einführung in SignalR für ASP.NET Core)</span><span class="sxs-lookup"><span data-stu-id="8e2a6-205">Introduction to SignalR for ASP.NET Core</span></span>](xref:signalr/introduction)
* [<span data-ttu-id="8e2a6-206">Verwenden von Hubs in SignalR für ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="8e2a6-206">Use hubs in SignalR for ASP.NET Core</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="8e2a6-207">ASP.NET Core SignalR JavaScript client (JavaScript-Client in SignalR für ASP.NET Core)</span><span class="sxs-lookup"><span data-stu-id="8e2a6-207">ASP.NET Core SignalR JavaScript client</span></span>](xref:signalr/javascript-client)
