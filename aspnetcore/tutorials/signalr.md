---
title: Erste Schritte mit ASP.NET Core SignalR
author: tdykstra
description: In diesem Tutorial erstellen Sie eine Chat-App, die ASP.NET Core SignalR verwendet.
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 11/13/2018
uid: tutorials/signalr
ms.openlocfilehash: 190717dc6e6f9f2766ba92aa7472f4cdea9b6827
ms.sourcegitcommit: e7fafb153b9de7595c2558a0133f8d1c33a3bddb
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/28/2018
ms.locfileid: "52458529"
---
# <a name="tutorial-get-started-with-aspnet-core-signalr"></a><span data-ttu-id="e3d3e-103">Tutorial: Erste Schritte mit ASP.NET Core SignalR</span><span class="sxs-lookup"><span data-stu-id="e3d3e-103">Tutorial: Get started with ASP.NET Core SignalR</span></span>

<span data-ttu-id="e3d3e-104">In diesem Tutorial werden die Grundlagen zur Erstellung einer Echtzeit-App mit SignalR beschrieben.</span><span class="sxs-lookup"><span data-stu-id="e3d3e-104">This tutorial teaches the basics of building a real-time app using SignalR.</span></span> <span data-ttu-id="e3d3e-105">Sie lernen Folgendes:</span><span class="sxs-lookup"><span data-stu-id="e3d3e-105">You learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="e3d3e-106">Erstellen Sie ein Webprojekt.</span><span class="sxs-lookup"><span data-stu-id="e3d3e-106">Create a web project.</span></span>
> * <span data-ttu-id="e3d3e-107">Hinzufügen der SignalR-Clientbibliothek</span><span class="sxs-lookup"><span data-stu-id="e3d3e-107">Add the SignalR client library.</span></span>
> * <span data-ttu-id="e3d3e-108">Erstellen eines SignalR-Hubs</span><span class="sxs-lookup"><span data-stu-id="e3d3e-108">Create a SignalR hub.</span></span>
> * <span data-ttu-id="e3d3e-109">Konfigurieren des Projekts zur Verwendung von SignalR</span><span class="sxs-lookup"><span data-stu-id="e3d3e-109">Configure the project to use SignalR.</span></span>
> * <span data-ttu-id="e3d3e-110">Hinzufügen von Code, mit dem Nachrichten von jedem Client an alle verbundene Clients gesendet werden.</span><span class="sxs-lookup"><span data-stu-id="e3d3e-110">Add code that sends messages from any client to all connected clients.</span></span>

<span data-ttu-id="e3d3e-111">Am Ende verfügen Sie über eine funktionierende Chat-App:</span><span class="sxs-lookup"><span data-stu-id="e3d3e-111">At the end, you'll have a working chat app:</span></span>

![SignalR-Beispiel-App](signalr/_static/signalr-get-started-finished.png)

<span data-ttu-id="e3d3e-113">[Zeigen Sie Beispielcode an, oder laden Sie diesen herunter](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/signalr/sample) ([Vorgehensweise zum Herunterladen](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="e3d3e-113">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/signalr/sample) ([how to download](xref:index#how-to-download-a-sample)).</span></span>

> [!NOTE]
> <span data-ttu-id="e3d3e-114">Wir testen gerade eine vorgeschlagene neue Struktur für das ASP.NET Core-Inhaltsverzeichnis.</span><span class="sxs-lookup"><span data-stu-id="e3d3e-114">We’re testing the usability of a proposed new structure for the ASP.NET Core table of contents.</span></span>  <span data-ttu-id="e3d3e-115">Falls Sie einige Minuten Zeit haben, um einen Test durchzuführen, in dem Sie sieben unterschiedliche Artikel im aktuellen und vorgeschlagene Inhaltsverzeichnis finden sollen, [klicken Sie hier, um daran teilzunehmen](https://dpk4xbh5.optimalworkshop.com/treejack/rps16hd5).</span><span class="sxs-lookup"><span data-stu-id="e3d3e-115">If you have a few minutes to try an exercise of finding 7 different topics in the current or proposed table of contents, please [click here to participate in the study](https://dpk4xbh5.optimalworkshop.com/treejack/rps16hd5).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="e3d3e-116">Erforderliche Komponenten</span><span class="sxs-lookup"><span data-stu-id="e3d3e-116">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="e3d3e-117">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e3d3e-117">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="e3d3e-118">[Version 15.8 von Visual Studio 2017 oder höher](https://www.visualstudio.com/downloads/) mit der Workload **ASP.NET und Webentwicklung**</span><span class="sxs-lookup"><span data-stu-id="e3d3e-118">[Visual Studio 2017 version 15.8 or later](https://www.visualstudio.com/downloads/) with the **ASP.NET and web development** workload</span></span>
* [<span data-ttu-id="e3d3e-119">.NET Core SDK 2.1 oder höher</span><span class="sxs-lookup"><span data-stu-id="e3d3e-119">.NET Core SDK 2.1 or later</span></span>](https://www.microsoft.com/net/download/all)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="e3d3e-120">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="e3d3e-120">Visual Studio Code</span></span>](#tab/visual-studio-code)

* [<span data-ttu-id="e3d3e-121">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="e3d3e-121">Visual Studio Code</span></span>](https://code.visualstudio.com/download)
* [<span data-ttu-id="e3d3e-122">.NET Core SDK 2.1 oder höher</span><span class="sxs-lookup"><span data-stu-id="e3d3e-122">.NET Core SDK 2.1 or later</span></span>](https://www.microsoft.com/net/download/all)
* [<span data-ttu-id="e3d3e-123">C# für Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="e3d3e-123">C# for Visual Studio Code</span></span>](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="e3d3e-124">Visual Studio für Mac</span><span class="sxs-lookup"><span data-stu-id="e3d3e-124">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* [<span data-ttu-id="e3d3e-125">Visual Studio für Mac Version 7.5.4 oder höher</span><span class="sxs-lookup"><span data-stu-id="e3d3e-125">Visual Studio for Mac version 7.5.4 or later</span></span>](https://www.visualstudio.com/downloads/)
* <span data-ttu-id="e3d3e-126">[.NET Core SDK 2.1 oder höher](https://www.microsoft.com/net/download/all) (in der Visual Studio-Installation enthalten)</span><span class="sxs-lookup"><span data-stu-id="e3d3e-126">[.NET Core SDK 2.1 or later](https://www.microsoft.com/net/download/all) (included in the Visual Studio install)</span></span>

---

## <a name="create-a-web-project"></a><span data-ttu-id="e3d3e-127">Erstellen eines Webprojekts</span><span class="sxs-lookup"><span data-stu-id="e3d3e-127">Create a web project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="e3d3e-128">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e3d3e-128">Visual Studio</span></span>](#tab/visual-studio/)

* <span data-ttu-id="e3d3e-129">Klicken Sie im Menü auf **Datei > Neues Projekt**.</span><span class="sxs-lookup"><span data-stu-id="e3d3e-129">From the menu, select **File > New Project**.</span></span>

* <span data-ttu-id="e3d3e-130">Wählen Sie im Dialogfeld **Neues Projekt** **Installiert > Visual C# > Web > ASP.NET Core-Webanwendung** aus.</span><span class="sxs-lookup"><span data-stu-id="e3d3e-130">In the **New Project** dialog, select **Installed > Visual C# > Web > ASP.NET Core Web Application**.</span></span> <span data-ttu-id="e3d3e-131">Geben Sie als Projektname *SignalRChat* ein.</span><span class="sxs-lookup"><span data-stu-id="e3d3e-131">Name the project *SignalRChat*.</span></span>

  ![Dialogfeld „Neues Projekt“ in Visual Studio](signalr/_static/signalr-new-project-dialog.png)

* <span data-ttu-id="e3d3e-133">Klicken Sie auf **Webanwendung**, um ein Projekt zu erstellen, das Razor Pages verwendet.</span><span class="sxs-lookup"><span data-stu-id="e3d3e-133">Select **Web Application** to create a project that uses Razor Pages.</span></span>

* <span data-ttu-id="e3d3e-134">Wählen Sie ein Zielframework von **.NET Core** aus, und klicken Sie anschließend auf **ASP.NET Core 2.1** und **OK**.</span><span class="sxs-lookup"><span data-stu-id="e3d3e-134">Select a target framework of **.NET Core**, select **ASP.NET Core 2.1**, and click **OK**.</span></span>

  ![Dialogfeld „Neues Projekt“ in Visual Studio](signalr/_static/signalr-new-project-choose-type.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="e3d3e-136">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="e3d3e-136">Visual Studio Code</span></span>](#tab/visual-studio-code/)

* <span data-ttu-id="e3d3e-137">Öffnen Sie das [integrierte Terminal](https://code.visualstudio.com/docs/editor/integrated-terminal) in dem Ordner, in dem der neue Projektordner erstellt wird.</span><span class="sxs-lookup"><span data-stu-id="e3d3e-137">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal) to the folder in which the new project folder will be created.</span></span>

* <span data-ttu-id="e3d3e-138">Führen Sie die folgenden Befehle aus:</span><span class="sxs-lookup"><span data-stu-id="e3d3e-138">Run the following commands:</span></span>

   ```console
   dotnet new webapp -o SignalRChat
   code -r SignalRChat
   ```

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="e3d3e-139">Visual Studio für Mac</span><span class="sxs-lookup"><span data-stu-id="e3d3e-139">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="e3d3e-140">Klicken Sie im Menü auf **Datei > Neue Projektmappe**.</span><span class="sxs-lookup"><span data-stu-id="e3d3e-140">From the menu, select **File > New Solution**.</span></span>

* <span data-ttu-id="e3d3e-141">Klicken Sie auf **.NET Core > App > ASP.NET Core-Web-App** (Wählen Sie nicht **ASP.NET Core-Web-App (MVC)** aus).</span><span class="sxs-lookup"><span data-stu-id="e3d3e-141">Select **.NET Core > App > ASP.NET Core Web App** (Don't select **ASP.NET Core Web App (MVC)**).</span></span>

* <span data-ttu-id="e3d3e-142">Klicken Sie auf **Weiter**.</span><span class="sxs-lookup"><span data-stu-id="e3d3e-142">Select **Next**.</span></span>

* <span data-ttu-id="e3d3e-143">Nennen Sie das Projekt *SignalRChat*, und wählen Sie dann **Erstellen** aus.</span><span class="sxs-lookup"><span data-stu-id="e3d3e-143">Name the project *SignalRChat*, and then select **Create**.</span></span>

---

## <a name="add-the-signalr-client-library"></a><span data-ttu-id="e3d3e-144">Hinzufügen der SignalR-Clientbibliothek</span><span class="sxs-lookup"><span data-stu-id="e3d3e-144">Add the SignalR client library</span></span>

<span data-ttu-id="e3d3e-145">Die SignalR-Serverbibliothek ist im Metapaket `Microsoft.AspNetCore.App` enthalten.</span><span class="sxs-lookup"><span data-stu-id="e3d3e-145">The SignalR server library is included in the `Microsoft.AspNetCore.App` metapackage.</span></span> <span data-ttu-id="e3d3e-146">Die JavaScript-Clientbibliothek ist nicht automatisch im Projekt enthalten.</span><span class="sxs-lookup"><span data-stu-id="e3d3e-146">The JavaScript client library isn't automatically included in the project.</span></span> <span data-ttu-id="e3d3e-147">In diesem Tutorial verwenden Sie den Bibliotheks-Manager (LibMan), um die Clientbibliothek von *unpkg* abzurufen.</span><span class="sxs-lookup"><span data-stu-id="e3d3e-147">For this tutorial, you use Library Manager (LibMan) to get the client library from *unpkg*.</span></span> <span data-ttu-id="e3d3e-148">unpkg ist ein CDN (Content Delivery Network), mit dem Sie alles bereitstellen können, was im npm (Node.js-Paket-Manager) zu finden ist.</span><span class="sxs-lookup"><span data-stu-id="e3d3e-148">unpkg is a content delivery network (CDN)) that can deliver anything found in npm, the Node.js package manager.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="e3d3e-149">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e3d3e-149">Visual Studio</span></span>](#tab/visual-studio/)

* <span data-ttu-id="e3d3e-150">Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf das Projekt, und wählen Sie **Hinzufügen** > **Client-Side Library** (Clientseitige Bibliothek) aus.</span><span class="sxs-lookup"><span data-stu-id="e3d3e-150">In **Solution Explorer**, right-click the project, and select **Add** > **Client-Side Library**.</span></span>

* <span data-ttu-id="e3d3e-151">Wählen Sie **unpkg** im Dialogfeld **Add Client-Side Library** (Clientseitige Bibliothek hinzufügen) als **Anbieter** aus.</span><span class="sxs-lookup"><span data-stu-id="e3d3e-151">In the **Add Client-Side Library** dialog, for **Provider** select **unpkg**.</span></span> 

* <span data-ttu-id="e3d3e-152">Geben Sie `@aspnet/signalr@1` für die **Bibliothek** ein, und wählen Sie die neueste Version aus, die keine Vorschauversion ist.</span><span class="sxs-lookup"><span data-stu-id="e3d3e-152">For **Library**, enter `@aspnet/signalr@1`, and select the latest version that isn't preview.</span></span>

  ![Dialogfeld „Clientseitige Bibliothek hinzufügen“: Auswählen der Bibliothek](signalr/_static/libman1.png)

* <span data-ttu-id="e3d3e-154">Klicken Sie auf **Choose specific files** (Spezifische Dateien auswählen), erweitern Sie den Ordner *dist/browser*, und wählen Sie *signalr.js* und *signalr.min.js* aus.</span><span class="sxs-lookup"><span data-stu-id="e3d3e-154">Select **Choose specific files**, expand the *dist/browser* folder, and select *signalr.js* and *signalr.min.js*.</span></span>

* <span data-ttu-id="e3d3e-155">Legen Sie *wwwroot/lib/signalr/* als **Zielspeicherort** fest, und klicken Sie auf **Installieren**.</span><span class="sxs-lookup"><span data-stu-id="e3d3e-155">Set **Target Location** to *wwwroot/lib/signalr/*, and select **Install**.</span></span>

  ![Dialogfeld „Clientseitige Bibliothek hinzufügen“: Auswählen der Dateien und des Zielspeicherorts](signalr/_static/libman2.png)

  <span data-ttu-id="e3d3e-157">Der Ordner *wwwroot/lib/signalr* wird von LibMan erstellt, und die ausgewählten Dateien werden in ihn hineinkopiert.</span><span class="sxs-lookup"><span data-stu-id="e3d3e-157">LibMan creates a *wwwroot/lib/signalr* folder and copies the selected files to it.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="e3d3e-158">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="e3d3e-158">Visual Studio Code</span></span>](#tab/visual-studio-code/)

* <span data-ttu-id="e3d3e-159">Führen Sie den folgenden Befehl über das integrierte Terminal aus, um LibMan zu installieren.</span><span class="sxs-lookup"><span data-stu-id="e3d3e-159">In the integrated terminal, run the following command to install LibMan.</span></span>

  ```console
  dotnet tool install -g Microsoft.Web.LibraryManager.Cli
  ```

* <span data-ttu-id="e3d3e-160">Führen Sie den folgenden Befehl aus, um die SignalR-Clientbibliothek mit LibMan abzurufen.</span><span class="sxs-lookup"><span data-stu-id="e3d3e-160">Run the following command to get the SignalR client library by using LibMan.</span></span> <span data-ttu-id="e3d3e-161">Es kann einige Sekunden dauern, bis die Ausgabe angezeigt wird.</span><span class="sxs-lookup"><span data-stu-id="e3d3e-161">You might have to wait a few seconds before seeing output.</span></span>

  ```console
  libman install @aspnet/signalr -p unpkg -d wwwroot/lib/signalr --files dist/browser/signalr.js --files dist/browser/signalr.min.js
  ```

  <span data-ttu-id="e3d3e-162">Die Parameter legen folgende Optionen fest:</span><span class="sxs-lookup"><span data-stu-id="e3d3e-162">The parameters specify the following options:</span></span>
  * <span data-ttu-id="e3d3e-163">Die Verwendung des Anbieters „unpkg“.</span><span class="sxs-lookup"><span data-stu-id="e3d3e-163">Use the unpkg provider.</span></span>
  * <span data-ttu-id="e3d3e-164">Das Kopieren der Dateien in den Zielordner *wwwroot/lib/signalr*.</span><span class="sxs-lookup"><span data-stu-id="e3d3e-164">Copy files to the *wwwroot/lib/signalr* destination.</span></span>
  * <span data-ttu-id="e3d3e-165">Das Kopieren von ausschließlich den angegebenen Dateien.</span><span class="sxs-lookup"><span data-stu-id="e3d3e-165">Copy only the specified files.</span></span>

  <span data-ttu-id="e3d3e-166">Die Ausgabe sieht wie folgt aus:</span><span class="sxs-lookup"><span data-stu-id="e3d3e-166">The output looks like the following example:</span></span>

  ```console
  wwwroot/lib/signalr/dist/browser/signalr.js written to disk
  wwwroot/lib/signalr/dist/browser/signalr.min.js written to disk
  Installed library "@aspnet/signalr@1.0.3" to "wwwroot/lib/signalr"
  ```

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="e3d3e-167">Visual Studio für Mac</span><span class="sxs-lookup"><span data-stu-id="e3d3e-167">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="e3d3e-168">Führen Sie den folgenden Befehl über das **Terminal** aus, um LibMan zu installieren.</span><span class="sxs-lookup"><span data-stu-id="e3d3e-168">In the **Terminal**, run the following command to install LibMan.</span></span>

  ```console
  dotnet tool install -g Microsoft.Web.LibraryManager.Cli
  ```

* <span data-ttu-id="e3d3e-169">Navigieren Sie zum Projektordner (der die Datei *SignalRChat.csproj* enthält).</span><span class="sxs-lookup"><span data-stu-id="e3d3e-169">Navigate to the project folder (the one that contains the *SignalRChat.csproj* file).</span></span>

* <span data-ttu-id="e3d3e-170">Führen Sie den folgenden Befehl aus, um die SignalR-Clientbibliothek mit LibMan abzurufen.</span><span class="sxs-lookup"><span data-stu-id="e3d3e-170">Run the following command to get the SignalR client library by using LibMan.</span></span>

  ```console
  libman install @aspnet/signalr -p unpkg -d wwwroot/lib/signalr --files dist/browser/signalr.js --files dist/browser/signalr.min.js
  ```

  <span data-ttu-id="e3d3e-171">Die Parameter legen folgende Optionen fest:</span><span class="sxs-lookup"><span data-stu-id="e3d3e-171">The parameters specify the following options:</span></span>
  * <span data-ttu-id="e3d3e-172">Die Verwendung des Anbieters „unpkg“.</span><span class="sxs-lookup"><span data-stu-id="e3d3e-172">Use the unpkg provider.</span></span>
  * <span data-ttu-id="e3d3e-173">Das Kopieren der Dateien in den Zielordner *wwwroot/lib/signalr*.</span><span class="sxs-lookup"><span data-stu-id="e3d3e-173">Copy files to the *wwwroot/lib/signalr* destination.</span></span>
  * <span data-ttu-id="e3d3e-174">Das Kopieren von ausschließlich den angegebenen Dateien.</span><span class="sxs-lookup"><span data-stu-id="e3d3e-174">Copy only the specified files.</span></span>

  <span data-ttu-id="e3d3e-175">Die Ausgabe sieht wie folgt aus:</span><span class="sxs-lookup"><span data-stu-id="e3d3e-175">The output looks like the following example:</span></span>

  ```console
  wwwroot/lib/signalr/dist/browser/signalr.js written to disk
  wwwroot/lib/signalr/dist/browser/signalr.min.js written to disk
  Installed library "@aspnet/signalr@1.0.3" to "wwwroot/lib/signalr"
  ```

---

## <a name="create-a-signalr-hub"></a><span data-ttu-id="e3d3e-176">Erstellen eines SignalR-Hubs</span><span class="sxs-lookup"><span data-stu-id="e3d3e-176">Create a SignalR hub</span></span>

<span data-ttu-id="e3d3e-177">Ein *Hub* ist eine Klasse, die als grundlegende Pipeline verwendet wird, mit der Client/Server-Kommunikation verarbeitet wird.</span><span class="sxs-lookup"><span data-stu-id="e3d3e-177">A *hub* is a class that serves as a high-level pipeline that handles client-server communication.</span></span>

* <span data-ttu-id="e3d3e-178">Erstellen Sie im SignalRChat-Projektordner einen *Hubs*-Ordner.</span><span class="sxs-lookup"><span data-stu-id="e3d3e-178">In the SignalRChat project folder, create a *Hubs* folder.</span></span>

* <span data-ttu-id="e3d3e-179">Erstellen Sie im *Hubs*-Ordner eine *ChatHub.cs*-Datei mit folgendem Code:</span><span class="sxs-lookup"><span data-stu-id="e3d3e-179">In the *Hubs* folder, create a *ChatHub.cs* file with the following code:</span></span>

  [!code-csharp[Startup](signalr/sample/Hubs/ChatHub.cs)]

  <span data-ttu-id="e3d3e-180">Die `ChatHub`-Klasse erbt von der `Hub`-Klasse von SignalR.</span><span class="sxs-lookup"><span data-stu-id="e3d3e-180">The `ChatHub` class inherits from the SignalR `Hub` class.</span></span> <span data-ttu-id="e3d3e-181">Die `Hub`-Klasse verwaltet Verbindungen, Gruppen und Messaging.</span><span class="sxs-lookup"><span data-stu-id="e3d3e-181">The `Hub` class manages connections, groups, and messaging.</span></span>

  <span data-ttu-id="e3d3e-182">Die `SendMessage`-Methode kann von jedem verbundenen Client aufgerufen werden.</span><span class="sxs-lookup"><span data-stu-id="e3d3e-182">The `SendMessage` method can be called by any connected client.</span></span> <span data-ttu-id="e3d3e-183">Die empfangenen Nachrichten werden an alle Clients gesendet.</span><span class="sxs-lookup"><span data-stu-id="e3d3e-183">It sends the received message to all clients.</span></span> <span data-ttu-id="e3d3e-184">SignalR-Code ist asynchron, damit die maximale Skalierbarkeit gewährleistet werden kann.</span><span class="sxs-lookup"><span data-stu-id="e3d3e-184">SignalR code is asynchronous to provide maximum scalability.</span></span>

## <a name="configure-signalr"></a><span data-ttu-id="e3d3e-185">Konfigurieren von SignalR</span><span class="sxs-lookup"><span data-stu-id="e3d3e-185">Configure SignalR</span></span>

<span data-ttu-id="e3d3e-186">Der SignalR-Server muss zunächst konfiguriert werden, um Anforderungen an SignalR zu senden.</span><span class="sxs-lookup"><span data-stu-id="e3d3e-186">The SignalR server must be configured to pass SignalR requests to SignalR.</span></span>

* <span data-ttu-id="e3d3e-187">Fügen Sie der *Startup.cs*-Datei den folgenden hervorgehobenen Code zu.</span><span class="sxs-lookup"><span data-stu-id="e3d3e-187">Add the following highlighted code to the *Startup.cs* file.</span></span>

  [!code-csharp[Startup](signalr/sample/Startup.cs?highlight=7,33,52-55)]

  <span data-ttu-id="e3d3e-188">Durch diese Änderungen wird SignalR zum Dependency Injection-System von ASP.NET Core sowie der Middlewarepipeline hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="e3d3e-188">These changes add SignalR to the ASP.NET Core dependency injection system and the middleware pipeline.</span></span>

## <a name="add-signalr-client-code"></a><span data-ttu-id="e3d3e-189">Hinzufügen des SignalR-Clientcodes</span><span class="sxs-lookup"><span data-stu-id="e3d3e-189">Add SignalR client code</span></span>

* <span data-ttu-id="e3d3e-190">Ersetzen Sie den Inhalt in *Pages\Index.cshtml* durch den folgenden Code:</span><span class="sxs-lookup"><span data-stu-id="e3d3e-190">Replace the content in *Pages\Index.cshtml* with the following code:</span></span>

  [!code-cshtml[Index](signalr/sample/Pages/Index.cshtml)]

  <span data-ttu-id="e3d3e-191">Der vorangehende Code:</span><span class="sxs-lookup"><span data-stu-id="e3d3e-191">The preceding code:</span></span>

  * <span data-ttu-id="e3d3e-192">erstellt Textfelder für den Namen und den Nachrichtentext sowie eine Senden-Schaltfläche</span><span class="sxs-lookup"><span data-stu-id="e3d3e-192">Creates text boxes for name and message text, and a submit button.</span></span>
  * <span data-ttu-id="e3d3e-193">erstellt eine Liste mit `id="messagesList"` zum Anzeigen von Nachrichten, die vom SignalR-Hub empfangen werden</span><span class="sxs-lookup"><span data-stu-id="e3d3e-193">Creates a list with `id="messagesList"` for displaying messages that are received from the SignalR hub.</span></span>
  * <span data-ttu-id="e3d3e-194">schließt Skriptverweise sowie den *chat.js*-Anwendungscode, den Sie im folgenden Schritt erstellen, in SignalR ein</span><span class="sxs-lookup"><span data-stu-id="e3d3e-194">Includes script references to SignalR and the *chat.js* application code that you create in the next step.</span></span>

* <span data-ttu-id="e3d3e-195">Erstellen Sie im *wwwroot/js*-Ordner eine *chat.js*-Datei mit folgendem Code:</span><span class="sxs-lookup"><span data-stu-id="e3d3e-195">In the *wwwroot/js* folder, create a *chat.js* file with the following code:</span></span>

  [!code-javascript[Index](signalr/sample/wwwroot/js/chat.js)]

  <span data-ttu-id="e3d3e-196">Der vorangehende Code:</span><span class="sxs-lookup"><span data-stu-id="e3d3e-196">The preceding code:</span></span>

  * <span data-ttu-id="e3d3e-197">erstellt und startet eine Verbindung</span><span class="sxs-lookup"><span data-stu-id="e3d3e-197">Creates and starts a connection.</span></span>
  * <span data-ttu-id="e3d3e-198">fügt einen Handler zur Senden-Schaltfläche hinzu, der Nachrichten an den Hub sendet</span><span class="sxs-lookup"><span data-stu-id="e3d3e-198">Adds to the submit button a handler that sends messages to the hub.</span></span>
  * <span data-ttu-id="e3d3e-199">fügt einen Handler zum Verbindungsobjekt hinzu, der Nachrichten vom Hub empfängt und diese der Liste hinzufügt</span><span class="sxs-lookup"><span data-stu-id="e3d3e-199">Adds to the connection object a handler that receives messages from the hub and adds them to the list.</span></span>

## <a name="run-the-app"></a><span data-ttu-id="e3d3e-200">Ausführen der App</span><span class="sxs-lookup"><span data-stu-id="e3d3e-200">Run the app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="e3d3e-201">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e3d3e-201">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="e3d3e-202">Drücken Sie **STRG+F5**, um die App ohne Debugging zu starten.</span><span class="sxs-lookup"><span data-stu-id="e3d3e-202">Press **CTRL+F5** to run the app without debugging.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="e3d3e-203">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="e3d3e-203">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="e3d3e-204">Führen Sie über das integrierte Terminal den folgenden Befehl aus:</span><span class="sxs-lookup"><span data-stu-id="e3d3e-204">In the integrated terminal, run the following command:</span></span>

  ```console
  dotnet run -p SignalRChat.csproj
  ```
  
# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="e3d3e-205">Visual Studio für Mac</span><span class="sxs-lookup"><span data-stu-id="e3d3e-205">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="e3d3e-206">Wählen Sie im Menü **Ausführen > Starten ohne Debuggen** aus.</span><span class="sxs-lookup"><span data-stu-id="e3d3e-206">From the menu, select **Run > Start Without Debugging**.</span></span>

---

* <span data-ttu-id="e3d3e-207">Kopieren Sie die URL aus der Adressleiste, öffnen Sie eine neue Browserinstanz oder Registerkarte, und fügen Sie die URL in die Adressleiste ein.</span><span class="sxs-lookup"><span data-stu-id="e3d3e-207">Copy the URL from the address bar, open another browser instance or tab, and paste the URL in the address bar.</span></span>

* <span data-ttu-id="e3d3e-208">Geben Sie in einem der beiden Browser einen Namen und eine Nachricht ein, und klicken sie auf **Nachricht senden**.</span><span class="sxs-lookup"><span data-stu-id="e3d3e-208">Choose either browser, enter a name and message, and select the **Send Message** button.</span></span>

  <span data-ttu-id="e3d3e-209">Der Name und die Nachricht werden sofort auf beiden Seiten angezeigt.</span><span class="sxs-lookup"><span data-stu-id="e3d3e-209">The name and message are displayed on both pages instantly.</span></span>

  ![SignalR-Beispiel-App](signalr/_static/signalr-get-started-finished.png)

> [!TIP]
> <span data-ttu-id="e3d3e-211">Wenn die App nicht funktioniert, öffnen Sie die Browser-Entwicklungstools (F12), und wechseln Sie zur Konsole.</span><span class="sxs-lookup"><span data-stu-id="e3d3e-211">If the app doesn't work, open your browser developer tools (F12) and go to the console.</span></span> <span data-ttu-id="e3d3e-212">Möglicherweise werden Fehler in Bezug auf Ihren HTML- und JavaScript-Code angezeigt.</span><span class="sxs-lookup"><span data-stu-id="e3d3e-212">You might see errors related to your HTML and JavaScript code.</span></span> <span data-ttu-id="e3d3e-213">Nehmen wir an, dass Sie z.B. *signalr.js* in einen anderen Ordner als vorgeschrieben platziert haben.</span><span class="sxs-lookup"><span data-stu-id="e3d3e-213">For example, suppose you put *signalr.js* in a different folder than directed.</span></span> <span data-ttu-id="e3d3e-214">In diesem Fall funktioniert der Verweis auf diese Datei nicht, und Ihnen wird der Fehler 404 in der Konsole angezeigt.</span><span class="sxs-lookup"><span data-stu-id="e3d3e-214">In that case the reference to that file won't work and you'll see a 404 error in the console.</span></span>
> <span data-ttu-id="e3d3e-215">![Fehler „signalr.js nicht gefunden“](signalr/_static/f12-console.png)</span><span class="sxs-lookup"><span data-stu-id="e3d3e-215">![signalr.js not found error](signalr/_static/f12-console.png)</span></span>

## <a name="next-steps"></a><span data-ttu-id="e3d3e-216">Nächste Schritte</span><span class="sxs-lookup"><span data-stu-id="e3d3e-216">Next steps</span></span>

<span data-ttu-id="e3d3e-217">In diesem Tutorial haben Sie gelernt, wie die folgenden Aufgaben ausgeführt werden:</span><span class="sxs-lookup"><span data-stu-id="e3d3e-217">In this tutorial, you learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="e3d3e-218">Erstellen Sie ein Web-App-Projekt.</span><span class="sxs-lookup"><span data-stu-id="e3d3e-218">Create a web app project.</span></span>
> * <span data-ttu-id="e3d3e-219">Hinzufügen der SignalR-Clientbibliothek</span><span class="sxs-lookup"><span data-stu-id="e3d3e-219">Add the SignalR client library.</span></span>
> * <span data-ttu-id="e3d3e-220">Erstellen eines SignalR-Hubs</span><span class="sxs-lookup"><span data-stu-id="e3d3e-220">Create a SignalR hub.</span></span>
> * <span data-ttu-id="e3d3e-221">Konfigurieren des Projekts zur Verwendung von SignalR</span><span class="sxs-lookup"><span data-stu-id="e3d3e-221">Configure the project to use SignalR.</span></span>
> * <span data-ttu-id="e3d3e-222">Hinzufügen von Code, der den Hub verwendet, um Nachrichten von einem beliebigen Client an alle verbundenen Clients zu senden</span><span class="sxs-lookup"><span data-stu-id="e3d3e-222">Add code that uses the hub to send messages from any client to all connected clients.</span></span>

<span data-ttu-id="e3d3e-223">Weitere Informationen zu SignalR finden Sie in der folgenden Einführung:</span><span class="sxs-lookup"><span data-stu-id="e3d3e-223">To learn more about SignalR, see the introduction:</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="e3d3e-224">Einführung in ASP.NET Core SignalR</span><span class="sxs-lookup"><span data-stu-id="e3d3e-224">Introduction to ASP.NET Core SignalR</span></span>](xref:signalr/introduction)
