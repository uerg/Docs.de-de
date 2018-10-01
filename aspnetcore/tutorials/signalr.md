---
title: 'Tutorial: Erste Schritte mit SignalR unter ASP.NET Core'
author: tdykstra
description: In diesem Tutorial erstellen Sie eine Chat-App, die SignalR für ASP.NET Core verwendet.
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 08/31/2018
uid: tutorials/signalr
ms.openlocfilehash: 6f93d6dc664f68425ef0fa0d02f9011e4875bc33
ms.sourcegitcommit: 9bdba90b2c97a4016188434657194b2d7027d6e3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/27/2018
ms.locfileid: "47402132"
---
# <a name="tutorial-get-started-with-signalr-on-aspnet-core"></a><span data-ttu-id="03aa4-103">Tutorial: Erste Schritte mit SignalR unter ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="03aa4-103">Tutorial: Get started with SignalR on ASP.NET Core</span></span>

<span data-ttu-id="03aa4-104">In diesem Tutorial werden die Grundlagen zur Erstellung einer Echtzeit-App mit SignalR beschrieben.</span><span class="sxs-lookup"><span data-stu-id="03aa4-104">This tutorial teaches the basics of building a real-time app using SignalR.</span></span> <span data-ttu-id="03aa4-105">Sie lernen Folgendes:</span><span class="sxs-lookup"><span data-stu-id="03aa4-105">You learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="03aa4-106">Erstellen einer Web-App, die SignalR für ASP.NET Core verwendet</span><span class="sxs-lookup"><span data-stu-id="03aa4-106">Create a web app that uses SignalR on ASP.NET Core.</span></span>
> * <span data-ttu-id="03aa4-107">Erstellen eines SignalR-Hubs auf dem Server</span><span class="sxs-lookup"><span data-stu-id="03aa4-107">Create a SignalR hub on the server.</span></span>
> * <span data-ttu-id="03aa4-108">Herstellen einer Verbindung zwischen JavaScript-Clients und dem SignalR-Hub</span><span class="sxs-lookup"><span data-stu-id="03aa4-108">Connect to the SignalR hub from JavaScript clients.</span></span>
> * <span data-ttu-id="03aa4-109">Verwenden des Hubs zum Senden von Nachrichten von jedem Client an alle verbundenen Clients</span><span class="sxs-lookup"><span data-stu-id="03aa4-109">Use the hub to send messages from any client to all connected clients.</span></span>

<span data-ttu-id="03aa4-110">Am Ende verfügen Sie über eine funktionierende Chat-App:</span><span class="sxs-lookup"><span data-stu-id="03aa4-110">At the end, you'll have a working chat app:</span></span>

![SignalR-Beispiel-App](signalr/_static/signalr-get-started-finished.png)

<span data-ttu-id="03aa4-112">[Zeigen Sie Beispielcode an, oder laden Sie diesen herunter](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/signalr/sample) ([Vorgehensweise zum Herunterladen](xref:tutorials/index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="03aa4-112">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/signalr/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample)).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="03aa4-113">Erforderliche Komponenten</span><span class="sxs-lookup"><span data-stu-id="03aa4-113">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="03aa4-114">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="03aa4-114">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="03aa4-115">[Version 15.8 von Visual Studio 2017 oder höher](https://www.visualstudio.com/downloads/) mit der Workload **ASP.NET und Webentwicklung**</span><span class="sxs-lookup"><span data-stu-id="03aa4-115">[Visual Studio 2017 version 15.8 or later](https://www.visualstudio.com/downloads/) with the **ASP.NET and web development** workload</span></span>
* [<span data-ttu-id="03aa4-116">.NET Core SDK 2.1 oder höher</span><span class="sxs-lookup"><span data-stu-id="03aa4-116">.NET Core SDK 2.1 or later</span></span>](https://www.microsoft.com/net/download/all)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="03aa4-117">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="03aa4-117">Visual Studio Code</span></span>](#tab/visual-studio-code)

* [<span data-ttu-id="03aa4-118">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="03aa4-118">Visual Studio Code</span></span>](https://code.visualstudio.com/download)
* [<span data-ttu-id="03aa4-119">.NET Core SDK 2.1 oder höher</span><span class="sxs-lookup"><span data-stu-id="03aa4-119">.NET Core SDK 2.1 or later</span></span>](https://www.microsoft.com/net/download/all)
* [<span data-ttu-id="03aa4-120">C# für Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="03aa4-120">C# for Visual Studio Code</span></span>](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="03aa4-121">Visual Studio für Mac</span><span class="sxs-lookup"><span data-stu-id="03aa4-121">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* [<span data-ttu-id="03aa4-122">Visual Studio für Mac Version 7.5.4 oder höher</span><span class="sxs-lookup"><span data-stu-id="03aa4-122">Visual Studio for Mac version 7.5.4 or later</span></span>](https://www.visualstudio.com/downloads/)
* <span data-ttu-id="03aa4-123">[.NET Core SDK 2.1 oder höher](https://www.microsoft.com/net/download/all) (in der Visual Studio-Installation enthalten)</span><span class="sxs-lookup"><span data-stu-id="03aa4-123">[.NET Core SDK 2.1 or later](https://www.microsoft.com/net/download/all) (included in the Visual Studio install)</span></span>

---

## <a name="create-the-project"></a><span data-ttu-id="03aa4-124">Erstellen eines Projekts</span><span class="sxs-lookup"><span data-stu-id="03aa4-124">Create the project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="03aa4-125">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="03aa4-125">Visual Studio</span></span>](#tab/visual-studio/)

* <span data-ttu-id="03aa4-126">Klicken Sie im Menü auf **Datei > Neues Projekt**.</span><span class="sxs-lookup"><span data-stu-id="03aa4-126">From the menu, select **File > New Project**.</span></span>

* <span data-ttu-id="03aa4-127">Wählen Sie im Dialogfeld **Neues Projekt** **Installiert > Visual C# > Web > ASP.NET Core-Webanwendung** aus.</span><span class="sxs-lookup"><span data-stu-id="03aa4-127">In the **New Project** dialog, select **Installed > Visual C# > Web > ASP.NET Core Web Application**.</span></span> <span data-ttu-id="03aa4-128">Geben Sie als Projektname *SignalRChat* ein.</span><span class="sxs-lookup"><span data-stu-id="03aa4-128">Name the project *SignalRChat*.</span></span>

  ![Dialogfeld „Neues Projekt“ in Visual Studio](signalr/_static/signalr-new-project-dialog.png)

* <span data-ttu-id="03aa4-130">Klicken Sie auf **Webanwendung**, um ein Projekt zu erstellen, das Razor Pages verwendet.</span><span class="sxs-lookup"><span data-stu-id="03aa4-130">Select **Web Application** to create a project that uses Razor Pages.</span></span>

* <span data-ttu-id="03aa4-131">Wählen Sie ein Zielframework von **.NET Core** aus, und klicken Sie anschließend auf **ASP.NET Core 2.1** und **OK**.</span><span class="sxs-lookup"><span data-stu-id="03aa4-131">Select a target framework of **.NET Core**, select **ASP.NET Core 2.1**, and click **OK**.</span></span>

  ![Dialogfeld „Neues Projekt“ in Visual Studio](signalr/_static/signalr-new-project-choose-type.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="03aa4-133">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="03aa4-133">Visual Studio Code</span></span>](#tab/visual-studio-code/)

* <span data-ttu-id="03aa4-134">Öffnen Sie einen Ordner, den Sie für ein neues Projekt verwenden können.</span><span class="sxs-lookup"><span data-stu-id="03aa4-134">Open a folder that you can use for a new project.</span></span>

* <span data-ttu-id="03aa4-135">Führen Sie über das **integrierte Terminal** den folgenden Befehl aus:</span><span class="sxs-lookup"><span data-stu-id="03aa4-135">In the **Integrated Terminal**, run the following command:</span></span>

   ```console
   dotnet new webapp -o SignalRChat
   ```

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="03aa4-136">Visual Studio für Mac</span><span class="sxs-lookup"><span data-stu-id="03aa4-136">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="03aa4-137">Klicken Sie im Menü auf **Datei > Neue Projektmappe**.</span><span class="sxs-lookup"><span data-stu-id="03aa4-137">From the menu, select **File > New Solution**.</span></span>

* <span data-ttu-id="03aa4-138">Klicken Sie auf **.NET Core > App > ASP.NET Core-Web-App** (Wählen Sie nicht **ASP.NET Core-Web-App (MVC)** aus).</span><span class="sxs-lookup"><span data-stu-id="03aa4-138">Select **.NET Core > App > ASP.NET Core Web App** (Don't select **ASP.NET Core Web App (MVC)**).</span></span>

* <span data-ttu-id="03aa4-139">Klicken Sie auf **Weiter**.</span><span class="sxs-lookup"><span data-stu-id="03aa4-139">Select **Next**.</span></span>

* <span data-ttu-id="03aa4-140">Nennen Sie das Projekt *SignalRChat*, und wählen Sie dann **Erstellen** aus.</span><span class="sxs-lookup"><span data-stu-id="03aa4-140">Name the project *SignalRChat*, and then select **Create**.</span></span>

---

## <a name="add-the-signalr-client-library"></a><span data-ttu-id="03aa4-141">Hinzufügen der SignalR-Clientbibliothek</span><span class="sxs-lookup"><span data-stu-id="03aa4-141">Add the SignalR client library</span></span>

<span data-ttu-id="03aa4-142">Die SignalR-Serverbibliothek ist im [Metapaket Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) enthalten.</span><span class="sxs-lookup"><span data-stu-id="03aa4-142">The SignalR server library is included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span> <span data-ttu-id="03aa4-143">Die JavaScript-Clientbibliothek ist nicht automatisch im Projekt enthalten.</span><span class="sxs-lookup"><span data-stu-id="03aa4-143">The JavaScript client library isn't automatically included in the project.</span></span> <span data-ttu-id="03aa4-144">In diesem Tutorial verwenden Sie den [Bibliotheks-Manager (LibMan)](xref:client-side/libman/index), um die Clientbibliothek von *unpkg* abzurufen.</span><span class="sxs-lookup"><span data-stu-id="03aa4-144">For this tutorial, you use [Library Manager (LibMan)](xref:client-side/libman/index) to get the client library from *unpkg*.</span></span> <span data-ttu-id="03aa4-145">[unpkg](https://unpkg.com/#/) ist ein [CDN](https://wikipedia.org/wiki/Content_delivery_network) (Content Delivery Network), mit dem Sie alles bereitstellen können, was im [npm (Node.js-Paket-Manager)](https://www.npmjs.com/get-npm) zu finden ist.</span><span class="sxs-lookup"><span data-stu-id="03aa4-145">[unpkg](https://unpkg.com/#/) is a [content delivery network](https://wikipedia.org/wiki/Content_delivery_network) that can deliver anything found in [npm, the Node.js package manager](https://www.npmjs.com/get-npm).</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="03aa4-146">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="03aa4-146">Visual Studio</span></span>](#tab/visual-studio/)

* <span data-ttu-id="03aa4-147">Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf das Projekt, und wählen Sie **Hinzufügen** > **Client-Side Library** (Clientseitige Bibliothek) aus.</span><span class="sxs-lookup"><span data-stu-id="03aa4-147">In **Solution Explorer**, right-click the project, and select **Add** > **Client-Side Library**.</span></span>

* <span data-ttu-id="03aa4-148">Wählen Sie **unpkg** im Dialogfeld **Add Client-Side Library** (Clientseitige Bibliothek hinzufügen) als **Anbieter** aus.</span><span class="sxs-lookup"><span data-stu-id="03aa4-148">In the **Add Client-Side Library** dialog, for **Provider** select **unpkg**.</span></span> 

* <span data-ttu-id="03aa4-149">Geben Sie _@aspnet/signalr@1_ für die **Bibliothek** ein, und wählen Sie die neuste Version aus, die keine Vorschauversion ist.</span><span class="sxs-lookup"><span data-stu-id="03aa4-149">For **Library**, enter _@aspnet/signalr@1_, and select the latest version that isn't preview.</span></span>

  ![Dialogfeld „Clientseitige Bibliothek hinzufügen“: Auswählen der Bibliothek](signalr/_static/libman1.png)

* <span data-ttu-id="03aa4-151">Klicken Sie auf **Choose specific files** (Spezifische Dateien auswählen), erweitern Sie den Ordner *dist/browser*, und wählen Sie *signalr.js* und *signalr.min.js* aus.</span><span class="sxs-lookup"><span data-stu-id="03aa4-151">Select **Choose specific files**, expand the *dist/browser* folder, and select *signalr.js* and *signalr.min.js*.</span></span>

* <span data-ttu-id="03aa4-152">Legen Sie *wwwroot/lib/signalr/* als **Zielspeicherort** fest, und klicken Sie auf **Installieren**.</span><span class="sxs-lookup"><span data-stu-id="03aa4-152">Set **Target Location** to *wwwroot/lib/signalr/*, and select **Install**.</span></span>

  ![Dialogfeld „Clientseitige Bibliothek hinzufügen“: Auswählen der Dateien und des Zielspeicherorts](signalr/_static/libman2.png)

  <span data-ttu-id="03aa4-154">Der Ordner *wwwroot/lib/signalr/* wird von [LibMan](xref:client-side/libman/index) erstellt, und die ausgewählten Dateien werden in ihn hineinkopiert.</span><span class="sxs-lookup"><span data-stu-id="03aa4-154">[LibMan](xref:client-side/libman/index) creates a *wwwroot/lib/signalr* folder and copies the selected files to it.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="03aa4-155">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="03aa4-155">Visual Studio Code</span></span>](#tab/visual-studio-code/)

* <span data-ttu-id="03aa4-156">Führen Sie den folgenden Befehl über das **integrierte Terminal** aus, um LibMan zu installieren.</span><span class="sxs-lookup"><span data-stu-id="03aa4-156">In the **Integrated Terminal**, run the following command to install LibMan.</span></span>

  ```console
  dotnet tool install -g Microsoft.Web.LibraryManager.Cli
  ```

* <span data-ttu-id="03aa4-157">Navigieren Sie zum Projektordner (der die Datei *SignalRChat.csproj* enthält).</span><span class="sxs-lookup"><span data-stu-id="03aa4-157">Navigate to the project folder (the one that contains the *SignalRChat.csproj* file).</span></span>

* <span data-ttu-id="03aa4-158">Führen Sie den folgenden Befehl aus, um die SignalR-Clientbibliothek mit LibMan abzurufen.</span><span class="sxs-lookup"><span data-stu-id="03aa4-158">Run the following command to get the SignalR client library by using LibMan.</span></span> <span data-ttu-id="03aa4-159">Es kann einige Sekunden dauern, bis die Ausgabe angezeigt wird.</span><span class="sxs-lookup"><span data-stu-id="03aa4-159">You might have to wait a few seconds before seeing output.</span></span>

  ```console
  libman install @aspnet/signalr -p unpkg -d wwwroot/lib/signalr --files dist/browser/signalr.js --files dist/browser/signalr.min.js
  ```

  <span data-ttu-id="03aa4-160">Die Parameter legen folgende Optionen fest:</span><span class="sxs-lookup"><span data-stu-id="03aa4-160">The parameters specify the following options:</span></span>
  * <span data-ttu-id="03aa4-161">Die Verwendung des Anbieters „unpkg“.</span><span class="sxs-lookup"><span data-stu-id="03aa4-161">Use the unpkg provider.</span></span>
  * <span data-ttu-id="03aa4-162">Das Kopieren der Dateien in den Zielordner *wwwroot/lib/signalr*.</span><span class="sxs-lookup"><span data-stu-id="03aa4-162">Copy files to the *wwwroot/lib/signalr* destination.</span></span>
  * <span data-ttu-id="03aa4-163">Das Kopieren von ausschließlich den angegebenen Dateien.</span><span class="sxs-lookup"><span data-stu-id="03aa4-163">Copy only the specified files.</span></span>

  <span data-ttu-id="03aa4-164">Die Ausgabe sieht wie folgt aus:</span><span class="sxs-lookup"><span data-stu-id="03aa4-164">The output looks like the following example:</span></span>

  ```console
  wwwroot/lib/signalr/dist/browser/signalr.js written to disk
  wwwroot/lib/signalr/dist/browser/signalr.min.js written to disk
  Installed library "@aspnet/signalr@1.0.3" to "wwwroot/lib/signalr"
  ```

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="03aa4-165">Visual Studio für Mac</span><span class="sxs-lookup"><span data-stu-id="03aa4-165">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="03aa4-166">Führen Sie den folgenden Befehl über das **Terminal** aus, um LibMan zu installieren.</span><span class="sxs-lookup"><span data-stu-id="03aa4-166">In the **Terminal**, run the following command to install LibMan.</span></span>

  ```console
  dotnet tool install -g Microsoft.Web.LibraryManager.Cli
  ```

* <span data-ttu-id="03aa4-167">Navigieren Sie zum Projektordner (der die Datei *SignalRChat.csproj* enthält).</span><span class="sxs-lookup"><span data-stu-id="03aa4-167">Navigate to the project folder (the one that contains the *SignalRChat.csproj* file).</span></span>

* <span data-ttu-id="03aa4-168">Führen Sie den folgenden Befehl aus, um die SignalR-Clientbibliothek mit LibMan abzurufen.</span><span class="sxs-lookup"><span data-stu-id="03aa4-168">Run the following command to get the SignalR client library by using LibMan.</span></span>

  ```console
  libman install @aspnet/signalr -p unpkg -d wwwroot/lib/signalr --files dist/browser/signalr.js --files dist/browser/signalr.min.js
  ```

  <span data-ttu-id="03aa4-169">Die Parameter legen folgende Optionen fest:</span><span class="sxs-lookup"><span data-stu-id="03aa4-169">The parameters specify the following options:</span></span>
  * <span data-ttu-id="03aa4-170">Die Verwendung des Anbieters „unpkg“.</span><span class="sxs-lookup"><span data-stu-id="03aa4-170">Use the unpkg provider.</span></span>
  * <span data-ttu-id="03aa4-171">Das Kopieren der Dateien in den Zielordner *wwwroot/lib/signalr*.</span><span class="sxs-lookup"><span data-stu-id="03aa4-171">Copy files to the *wwwroot/lib/signalr* destination.</span></span>
  * <span data-ttu-id="03aa4-172">Das Kopieren von ausschließlich den angegebenen Dateien.</span><span class="sxs-lookup"><span data-stu-id="03aa4-172">Copy only the specified files.</span></span>

  <span data-ttu-id="03aa4-173">Die Ausgabe sieht wie folgt aus:</span><span class="sxs-lookup"><span data-stu-id="03aa4-173">The output looks like the following example:</span></span>

  ```console
  wwwroot/lib/signalr/dist/browser/signalr.js written to disk
  wwwroot/lib/signalr/dist/browser/signalr.min.js written to disk
  Installed library "@aspnet/signalr@1.0.3" to "wwwroot/lib/signalr"
  ```

---

## <a name="create-the-signalr-hub"></a><span data-ttu-id="03aa4-174">Erstellen des SignalR-Hubs</span><span class="sxs-lookup"><span data-stu-id="03aa4-174">Create the SignalR hub</span></span>

<span data-ttu-id="03aa4-175">Ein [Hub](xref:signalr/hubs) ist eine Klasse, die als grundlegende Pipeline verwendet wird, mit der Client/Server-Kommunikation verarbeitet wird.</span><span class="sxs-lookup"><span data-stu-id="03aa4-175">A [hub](xref:signalr/hubs) is a class that serves as a high-level pipeline that handles client-server communication.</span></span>

* <span data-ttu-id="03aa4-176">Erstellen Sie im SignalRChat-Projektordner einen *Hubs*-Ordner.</span><span class="sxs-lookup"><span data-stu-id="03aa4-176">In the SignalRChat project folder, create a *Hubs* folder.</span></span>

* <span data-ttu-id="03aa4-177">Erstellen Sie im *Hubs*-Ordner eine *ChatHub.cs*-Datei mit folgendem Code:</span><span class="sxs-lookup"><span data-stu-id="03aa4-177">In the *Hubs* folder, create a *ChatHub.cs* file with the following code:</span></span>

  [!code-csharp[Startup](signalr/sample/Hubs/ChatHub.cs)]

  <span data-ttu-id="03aa4-178">Die `ChatHub`-Klasse erbt von der [Hub](/dotnet/api/microsoft.aspnetcore.signalr.hub)-Klasse von SignalR.</span><span class="sxs-lookup"><span data-stu-id="03aa4-178">The `ChatHub` class inherits from the SignalR [Hub](/dotnet/api/microsoft.aspnetcore.signalr.hub) class.</span></span> <span data-ttu-id="03aa4-179">Die `Hub`-Klasse verwaltet Verbindungen, Gruppen und Messaging.</span><span class="sxs-lookup"><span data-stu-id="03aa4-179">The `Hub` class manages connections, groups, and messaging.</span></span>

  <span data-ttu-id="03aa4-180">Die `SendMessage`-Methode kann von jedem verbundenen Client aufgerufen werden.</span><span class="sxs-lookup"><span data-stu-id="03aa4-180">The `SendMessage` method can be called by any connected client.</span></span> <span data-ttu-id="03aa4-181">Die empfangenen Nachrichten werden an alle Clients gesendet.</span><span class="sxs-lookup"><span data-stu-id="03aa4-181">It sends the received message to all clients.</span></span> <span data-ttu-id="03aa4-182">SignalR-Code ist asynchron, damit die maximale Skalierbarkeit gewährleistet werden kann.</span><span class="sxs-lookup"><span data-stu-id="03aa4-182">SignalR code is asynchronous to provide maximum scalability.</span></span>

## <a name="configure-the-project-to-use-signalr"></a><span data-ttu-id="03aa4-183">Konfigurieren des Projekts zur Verwendung von SignalR</span><span class="sxs-lookup"><span data-stu-id="03aa4-183">Configure the project to use SignalR</span></span>

<span data-ttu-id="03aa4-184">Der SignalR-Server muss zunächst konfiguriert werden, um Anforderungen an SignalR zu senden.</span><span class="sxs-lookup"><span data-stu-id="03aa4-184">The SignalR server must be configured to pass SignalR requests to SignalR.</span></span>

* <span data-ttu-id="03aa4-185">Fügen Sie der *Startup.cs*-Datei den folgenden hervorgehobenen Code zu.</span><span class="sxs-lookup"><span data-stu-id="03aa4-185">Add the following highlighted code to the *Startup.cs* file.</span></span>

  [!code-csharp[Startup](signalr/sample/Startup.cs?highlight=7,33,52-55)]

  <span data-ttu-id="03aa4-186">Durch diese Änderungen wird SignalR zum [Dependency Injection](xref:fundamentals/dependency-injection)-System sowie der [Middlewarepipeline](xref:fundamentals/middleware/index) hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="03aa4-186">These changes add SignalR to the [dependency injection](xref:fundamentals/dependency-injection) system and the [middleware](xref:fundamentals/middleware/index) pipeline.</span></span>

## <a name="create-the-signalr-client-code"></a><span data-ttu-id="03aa4-187">Erstellen des SignalR-Clientcodes</span><span class="sxs-lookup"><span data-stu-id="03aa4-187">Create the SignalR client code</span></span>

* <span data-ttu-id="03aa4-188">Ersetzen Sie den Inhalt in *Pages\Index.cshtml* durch den folgenden Code:</span><span class="sxs-lookup"><span data-stu-id="03aa4-188">Replace the content in *Pages\Index.cshtml* with the following code:</span></span>

  [!code-cshtml[Index](signalr/sample/Pages/Index.cshtml)]

  <span data-ttu-id="03aa4-189">Der vorangehende Code:</span><span class="sxs-lookup"><span data-stu-id="03aa4-189">The preceding code:</span></span>

  * <span data-ttu-id="03aa4-190">erstellt Textfelder für den Namen und den Nachrichtentext sowie eine Senden-Schaltfläche</span><span class="sxs-lookup"><span data-stu-id="03aa4-190">Creates text boxes for name and message text, and a submit button.</span></span>
  * <span data-ttu-id="03aa4-191">erstellt eine Liste mit `id="messagesList"` zum Anzeigen von Nachrichten, die vom SignalR-Hub empfangen werden</span><span class="sxs-lookup"><span data-stu-id="03aa4-191">Creates a list with `id="messagesList"` for displaying messages that are received from the SignalR hub.</span></span>
  * <span data-ttu-id="03aa4-192">schließt Skriptverweise sowie den *chat.js*-Anwendungscode, den Sie im folgenden Schritt erstellen, in SignalR ein</span><span class="sxs-lookup"><span data-stu-id="03aa4-192">Includes script references to SignalR and the *chat.js* application code that you create in the next step.</span></span>

* <span data-ttu-id="03aa4-193">Erstellen Sie im *wwwroot/js*-Ordner eine *chat.js*-Datei mit folgendem Code:</span><span class="sxs-lookup"><span data-stu-id="03aa4-193">In the *wwwroot/js* folder, create a *chat.js* file with the following code:</span></span>

  [!code-javascript[Index](signalr/sample/wwwroot/js/chat.js)]

  <span data-ttu-id="03aa4-194">Der vorangehende Code:</span><span class="sxs-lookup"><span data-stu-id="03aa4-194">The preceding code:</span></span>

  * <span data-ttu-id="03aa4-195">erstellt und startet eine Verbindung</span><span class="sxs-lookup"><span data-stu-id="03aa4-195">Creates and starts a connection.</span></span>
  * <span data-ttu-id="03aa4-196">fügt einen Handler zur Senden-Schaltfläche hinzu, der Nachrichten an den Hub sendet</span><span class="sxs-lookup"><span data-stu-id="03aa4-196">Adds to the submit button a handler that sends messages to the hub.</span></span>
  * <span data-ttu-id="03aa4-197">fügt einen Handler zum Verbindungsobjekt hinzu, der Nachrichten vom Hub empfängt und diese der Liste hinzufügt</span><span class="sxs-lookup"><span data-stu-id="03aa4-197">Adds to the connection object a handler that receives messages from the hub and adds them to the list.</span></span>

## <a name="run-the-app"></a><span data-ttu-id="03aa4-198">Ausführen der App</span><span class="sxs-lookup"><span data-stu-id="03aa4-198">Run the app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="03aa4-199">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="03aa4-199">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="03aa4-200">Drücken Sie **STRG+F5**, um die App ohne Debugging zu starten.</span><span class="sxs-lookup"><span data-stu-id="03aa4-200">Press **CTRL+F5** to run the app without debugging.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="03aa4-201">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="03aa4-201">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="03aa4-202">Drücken Sie **STRG+F5**, um die App ohne Debugging zu starten.</span><span class="sxs-lookup"><span data-stu-id="03aa4-202">Press **CTRL+F5** to run the app without debugging.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="03aa4-203">Visual Studio für Mac</span><span class="sxs-lookup"><span data-stu-id="03aa4-203">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="03aa4-204">Wählen Sie im Menü **Ausführen > Starten ohne Debuggen** aus.</span><span class="sxs-lookup"><span data-stu-id="03aa4-204">From the menu, select **Run > Start Without Debugging**.</span></span>

---

* <span data-ttu-id="03aa4-205">Kopieren Sie die URL aus der Adressleiste, öffnen Sie eine neue Browserinstanz oder Registerkarte, und fügen Sie die URL in die Adressleiste ein.</span><span class="sxs-lookup"><span data-stu-id="03aa4-205">Copy the URL from the address bar, open another browser instance or tab, and paste the URL in the address bar.</span></span>

* <span data-ttu-id="03aa4-206">Geben Sie in einem der beiden Browser einen Namen und eine Nachricht ein, und klicken sie auf **Send** (Senden).</span><span class="sxs-lookup"><span data-stu-id="03aa4-206">Choose either browser, enter a name and message, and select the **Send** button.</span></span>

  <span data-ttu-id="03aa4-207">Der Name und die Nachricht werden sofort auf beiden Seiten angezeigt.</span><span class="sxs-lookup"><span data-stu-id="03aa4-207">The name and message are displayed on both pages instantly.</span></span>

  ![SignalR-Beispiel-App](signalr/_static/signalr-get-started-finished.png)

> [!TIP]
> <span data-ttu-id="03aa4-209">Wenn die App nicht funktioniert, öffnen Sie die Browser-Entwicklungstools (F12), und wechseln Sie zur Konsole.</span><span class="sxs-lookup"><span data-stu-id="03aa4-209">If the app doesn't work, open your browser developer tools (F12) and go to the console.</span></span> <span data-ttu-id="03aa4-210">Möglicherweise werden Fehler in Bezug auf Ihren HTML- und JavaScript-Code angezeigt.</span><span class="sxs-lookup"><span data-stu-id="03aa4-210">You might see errors related to your HTML and JavaScript code.</span></span> <span data-ttu-id="03aa4-211">Nehmen wir an, dass Sie z.B. *signalr.js* in einen anderen Ordner als vorgeschrieben platziert haben.</span><span class="sxs-lookup"><span data-stu-id="03aa4-211">For example, suppose you put *signalr.js* in a different folder than directed.</span></span> <span data-ttu-id="03aa4-212">In diesem Fall funktioniert der Verweis auf diese Datei nicht, und Ihnen wird der Fehler 404 in der Konsole angezeigt.</span><span class="sxs-lookup"><span data-stu-id="03aa4-212">In that case the reference to that file won't work and you'll see a 404 error in the console.</span></span>
> <span data-ttu-id="03aa4-213">![Fehler „signalr.js nicht gefunden“](signalr/_static/f12-console.png)</span><span class="sxs-lookup"><span data-stu-id="03aa4-213">![signalr.js not found error](signalr/_static/f12-console.png)</span></span>

## <a name="next-steps"></a><span data-ttu-id="03aa4-214">Nächste Schritte</span><span class="sxs-lookup"><span data-stu-id="03aa4-214">Next steps</span></span>

<span data-ttu-id="03aa4-215">Wenn Sie möchten, dass Clients eine Verbindung mit einer SignalR-App von unterschiedlichen Domänen herstellt, müssen Sie die Ressourcenfreigabe zwischen verschiedenen Ursprüngen aktivieren (Cross-Origin Resource Sharing, CORS).</span><span class="sxs-lookup"><span data-stu-id="03aa4-215">If you want clients to connect to a SignalR app from different domains, you have to enable Cross-Origin Resource Sharing (CORS).</span></span> <span data-ttu-id="03aa4-216">Weiter Informationen finden Sie unter [Cross-origin resource sharing (Ressourcenfreigabe zwischen verschiedenen Ursprüngen)](xref:signalr/security?view=aspnetcore-2.1#cross-origin-resource-sharing).</span><span class="sxs-lookup"><span data-stu-id="03aa4-216">For more information, see [Cross-origin resource sharing](xref:signalr/security?view=aspnetcore-2.1#cross-origin-resource-sharing).</span></span>

<span data-ttu-id="03aa4-217">Weitere Informationen zu SignalR, Hubs sowie JavaScript-Clients finden Sie in folgenden Artikeln:</span><span class="sxs-lookup"><span data-stu-id="03aa4-217">To learn more about SignalR, hubs, and JavaScript clients, see these resources:</span></span>

* [<span data-ttu-id="03aa4-218">Introduction to SignalR for ASP.NET Core (Einführung in SignalR für ASP.NET Core)</span><span class="sxs-lookup"><span data-stu-id="03aa4-218">Introduction to SignalR for ASP.NET Core</span></span>](xref:signalr/introduction)
* [<span data-ttu-id="03aa4-219">Verwenden von Hubs in SignalR für ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="03aa4-219">Use hubs in SignalR for ASP.NET Core</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="03aa4-220">ASP.NET Core SignalR JavaScript client (JavaScript-Client in SignalR für ASP.NET Core)</span><span class="sxs-lookup"><span data-stu-id="03aa4-220">ASP.NET Core SignalR JavaScript client</span></span>](xref:signalr/javascript-client)
