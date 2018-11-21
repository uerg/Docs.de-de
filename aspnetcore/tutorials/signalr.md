---
title: Erste Schritte mit ASP.NET Core SignalR
author: tdykstra
description: In diesem Tutorial erstellen Sie eine Chat-App, die ASP.NET Core SignalR verwendet.
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 11/13/2018
uid: tutorials/signalr
ms.openlocfilehash: 8916b3659250c1bcbbc2dc9b3d466586f98bcc7e
ms.sourcegitcommit: d3392f688cfebc1f25616da7489664d69c6ee330
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/16/2018
ms.locfileid: "51818381"
---
# <a name="tutorial-get-started-with-aspnet-core-signalr"></a><span data-ttu-id="95040-103">Tutorial: Erste Schritte mit ASP.NET Core SignalR</span><span class="sxs-lookup"><span data-stu-id="95040-103">Tutorial: Get started with ASP.NET Core SignalR</span></span>

<span data-ttu-id="95040-104">In diesem Tutorial werden die Grundlagen zur Erstellung einer Echtzeit-App mit SignalR beschrieben.</span><span class="sxs-lookup"><span data-stu-id="95040-104">This tutorial teaches the basics of building a real-time app using SignalR.</span></span> <span data-ttu-id="95040-105">Sie lernen Folgendes:</span><span class="sxs-lookup"><span data-stu-id="95040-105">You learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="95040-106">Erstellen Sie ein Webprojekt.</span><span class="sxs-lookup"><span data-stu-id="95040-106">Create a web project.</span></span>
> * <span data-ttu-id="95040-107">Hinzufügen der SignalR-Clientbibliothek</span><span class="sxs-lookup"><span data-stu-id="95040-107">Add the SignalR client library.</span></span>
> * <span data-ttu-id="95040-108">Erstellen eines SignalR-Hubs</span><span class="sxs-lookup"><span data-stu-id="95040-108">Create a SignalR hub.</span></span>
> * <span data-ttu-id="95040-109">Konfigurieren des Projekts zur Verwendung von SignalR</span><span class="sxs-lookup"><span data-stu-id="95040-109">Configure the project to use SignalR.</span></span>
> * <span data-ttu-id="95040-110">Hinzufügen von Code, mit dem Nachrichten von jedem Client an alle verbundene Clients gesendet werden.</span><span class="sxs-lookup"><span data-stu-id="95040-110">Add code that sends messages from any client to all connected clients.</span></span>

<span data-ttu-id="95040-111">Am Ende verfügen Sie über eine funktionierende Chat-App:</span><span class="sxs-lookup"><span data-stu-id="95040-111">At the end, you'll have a working chat app:</span></span>

![SignalR-Beispiel-App](signalr/_static/signalr-get-started-finished.png)

<span data-ttu-id="95040-113">[Zeigen Sie Beispielcode an, oder laden Sie diesen herunter](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/signalr/sample) ([Vorgehensweise zum Herunterladen](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="95040-113">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/signalr/sample) ([how to download](xref:index#how-to-download-a-sample)).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="95040-114">Erforderliche Komponenten</span><span class="sxs-lookup"><span data-stu-id="95040-114">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="95040-115">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="95040-115">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="95040-116">[Version 15.8 von Visual Studio 2017 oder höher](https://www.visualstudio.com/downloads/) mit der Workload **ASP.NET und Webentwicklung**</span><span class="sxs-lookup"><span data-stu-id="95040-116">[Visual Studio 2017 version 15.8 or later](https://www.visualstudio.com/downloads/) with the **ASP.NET and web development** workload</span></span>
* [<span data-ttu-id="95040-117">.NET Core SDK 2.1 oder höher</span><span class="sxs-lookup"><span data-stu-id="95040-117">.NET Core SDK 2.1 or later</span></span>](https://www.microsoft.com/net/download/all)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="95040-118">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="95040-118">Visual Studio Code</span></span>](#tab/visual-studio-code)

* [<span data-ttu-id="95040-119">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="95040-119">Visual Studio Code</span></span>](https://code.visualstudio.com/download)
* [<span data-ttu-id="95040-120">.NET Core SDK 2.1 oder höher</span><span class="sxs-lookup"><span data-stu-id="95040-120">.NET Core SDK 2.1 or later</span></span>](https://www.microsoft.com/net/download/all)
* [<span data-ttu-id="95040-121">C# für Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="95040-121">C# for Visual Studio Code</span></span>](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="95040-122">Visual Studio für Mac</span><span class="sxs-lookup"><span data-stu-id="95040-122">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* [<span data-ttu-id="95040-123">Visual Studio für Mac Version 7.5.4 oder höher</span><span class="sxs-lookup"><span data-stu-id="95040-123">Visual Studio for Mac version 7.5.4 or later</span></span>](https://www.visualstudio.com/downloads/)
* <span data-ttu-id="95040-124">[.NET Core SDK 2.1 oder höher](https://www.microsoft.com/net/download/all) (in der Visual Studio-Installation enthalten)</span><span class="sxs-lookup"><span data-stu-id="95040-124">[.NET Core SDK 2.1 or later](https://www.microsoft.com/net/download/all) (included in the Visual Studio install)</span></span>

---

## <a name="create-a-web-project"></a><span data-ttu-id="95040-125">Erstellen eines Webprojekts</span><span class="sxs-lookup"><span data-stu-id="95040-125">Create a web project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="95040-126">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="95040-126">Visual Studio</span></span>](#tab/visual-studio/)

* <span data-ttu-id="95040-127">Klicken Sie im Menü auf **Datei > Neues Projekt**.</span><span class="sxs-lookup"><span data-stu-id="95040-127">From the menu, select **File > New Project**.</span></span>

* <span data-ttu-id="95040-128">Wählen Sie im Dialogfeld **Neues Projekt** **Installiert > Visual C# > Web > ASP.NET Core-Webanwendung** aus.</span><span class="sxs-lookup"><span data-stu-id="95040-128">In the **New Project** dialog, select **Installed > Visual C# > Web > ASP.NET Core Web Application**.</span></span> <span data-ttu-id="95040-129">Geben Sie als Projektname *SignalRChat* ein.</span><span class="sxs-lookup"><span data-stu-id="95040-129">Name the project *SignalRChat*.</span></span>

  ![Dialogfeld „Neues Projekt“ in Visual Studio](signalr/_static/signalr-new-project-dialog.png)

* <span data-ttu-id="95040-131">Klicken Sie auf **Webanwendung**, um ein Projekt zu erstellen, das Razor Pages verwendet.</span><span class="sxs-lookup"><span data-stu-id="95040-131">Select **Web Application** to create a project that uses Razor Pages.</span></span>

* <span data-ttu-id="95040-132">Wählen Sie ein Zielframework von **.NET Core** aus, und klicken Sie anschließend auf **ASP.NET Core 2.1** und **OK**.</span><span class="sxs-lookup"><span data-stu-id="95040-132">Select a target framework of **.NET Core**, select **ASP.NET Core 2.1**, and click **OK**.</span></span>

  ![Dialogfeld „Neues Projekt“ in Visual Studio](signalr/_static/signalr-new-project-choose-type.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="95040-134">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="95040-134">Visual Studio Code</span></span>](#tab/visual-studio-code/)

* <span data-ttu-id="95040-135">Öffnen Sie das [integrierte Terminal](https://code.visualstudio.com/docs/editor/integrated-terminal) in dem Ordner, in dem der neue Projektordner erstellt wird.</span><span class="sxs-lookup"><span data-stu-id="95040-135">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal) to the folder in which the new project folder will be created.</span></span>

* <span data-ttu-id="95040-136">Führen Sie die folgenden Befehle aus:</span><span class="sxs-lookup"><span data-stu-id="95040-136">Run the following commands:</span></span>

   ```console
   dotnet new webapp -o SignalRChat
   code -r SignalRChat
   ```

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="95040-137">Visual Studio für Mac</span><span class="sxs-lookup"><span data-stu-id="95040-137">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="95040-138">Klicken Sie im Menü auf **Datei > Neue Projektmappe**.</span><span class="sxs-lookup"><span data-stu-id="95040-138">From the menu, select **File > New Solution**.</span></span>

* <span data-ttu-id="95040-139">Klicken Sie auf **.NET Core > App > ASP.NET Core-Web-App** (Wählen Sie nicht **ASP.NET Core-Web-App (MVC)** aus).</span><span class="sxs-lookup"><span data-stu-id="95040-139">Select **.NET Core > App > ASP.NET Core Web App** (Don't select **ASP.NET Core Web App (MVC)**).</span></span>

* <span data-ttu-id="95040-140">Klicken Sie auf **Weiter**.</span><span class="sxs-lookup"><span data-stu-id="95040-140">Select **Next**.</span></span>

* <span data-ttu-id="95040-141">Nennen Sie das Projekt *SignalRChat*, und wählen Sie dann **Erstellen** aus.</span><span class="sxs-lookup"><span data-stu-id="95040-141">Name the project *SignalRChat*, and then select **Create**.</span></span>

---

## <a name="add-the-signalr-client-library"></a><span data-ttu-id="95040-142">Hinzufügen der SignalR-Clientbibliothek</span><span class="sxs-lookup"><span data-stu-id="95040-142">Add the SignalR client library</span></span>

<span data-ttu-id="95040-143">Die SignalR-Serverbibliothek ist im Metapaket `Microsoft.AspNetCore.App` enthalten.</span><span class="sxs-lookup"><span data-stu-id="95040-143">The SignalR server library is included in the `Microsoft.AspNetCore.App` metapackage.</span></span> <span data-ttu-id="95040-144">Die JavaScript-Clientbibliothek ist nicht automatisch im Projekt enthalten.</span><span class="sxs-lookup"><span data-stu-id="95040-144">The JavaScript client library isn't automatically included in the project.</span></span> <span data-ttu-id="95040-145">In diesem Tutorial verwenden Sie den Bibliotheks-Manager (LibMan), um die Clientbibliothek von *unpkg* abzurufen.</span><span class="sxs-lookup"><span data-stu-id="95040-145">For this tutorial, you use Library Manager (LibMan) to get the client library from *unpkg*.</span></span> <span data-ttu-id="95040-146">unpkg ist ein CDN (Content Delivery Network), mit dem Sie alles bereitstellen können, was im npm (Node.js-Paket-Manager) zu finden ist.</span><span class="sxs-lookup"><span data-stu-id="95040-146">unpkg is a content delivery network (CDN)) that can deliver anything found in npm, the Node.js package manager.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="95040-147">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="95040-147">Visual Studio</span></span>](#tab/visual-studio/)

* <span data-ttu-id="95040-148">Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf das Projekt, und wählen Sie **Hinzufügen** > **Client-Side Library** (Clientseitige Bibliothek) aus.</span><span class="sxs-lookup"><span data-stu-id="95040-148">In **Solution Explorer**, right-click the project, and select **Add** > **Client-Side Library**.</span></span>

* <span data-ttu-id="95040-149">Wählen Sie **unpkg** im Dialogfeld **Add Client-Side Library** (Clientseitige Bibliothek hinzufügen) als **Anbieter** aus.</span><span class="sxs-lookup"><span data-stu-id="95040-149">In the **Add Client-Side Library** dialog, for **Provider** select **unpkg**.</span></span> 

* <span data-ttu-id="95040-150">Geben Sie `@aspnet/signalr@1` für die **Bibliothek** ein, und wählen Sie die neueste Version aus, die keine Vorschauversion ist.</span><span class="sxs-lookup"><span data-stu-id="95040-150">For **Library**, enter `@aspnet/signalr@1`, and select the latest version that isn't preview.</span></span>

  ![Dialogfeld „Clientseitige Bibliothek hinzufügen“: Auswählen der Bibliothek](signalr/_static/libman1.png)

* <span data-ttu-id="95040-152">Klicken Sie auf **Choose specific files** (Spezifische Dateien auswählen), erweitern Sie den Ordner *dist/browser*, und wählen Sie *signalr.js* und *signalr.min.js* aus.</span><span class="sxs-lookup"><span data-stu-id="95040-152">Select **Choose specific files**, expand the *dist/browser* folder, and select *signalr.js* and *signalr.min.js*.</span></span>

* <span data-ttu-id="95040-153">Legen Sie *wwwroot/lib/signalr/* als **Zielspeicherort** fest, und klicken Sie auf **Installieren**.</span><span class="sxs-lookup"><span data-stu-id="95040-153">Set **Target Location** to *wwwroot/lib/signalr/*, and select **Install**.</span></span>

  ![Dialogfeld „Clientseitige Bibliothek hinzufügen“: Auswählen der Dateien und des Zielspeicherorts](signalr/_static/libman2.png)

  <span data-ttu-id="95040-155">Der Ordner *wwwroot/lib/signalr* wird von LibMan erstellt, und die ausgewählten Dateien werden in ihn hineinkopiert.</span><span class="sxs-lookup"><span data-stu-id="95040-155">LibMan creates a *wwwroot/lib/signalr* folder and copies the selected files to it.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="95040-156">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="95040-156">Visual Studio Code</span></span>](#tab/visual-studio-code/)

* <span data-ttu-id="95040-157">Führen Sie den folgenden Befehl über das integrierte Terminal aus, um LibMan zu installieren.</span><span class="sxs-lookup"><span data-stu-id="95040-157">In the integrated terminal, run the following command to install LibMan.</span></span>

  ```console
  dotnet tool install -g Microsoft.Web.LibraryManager.Cli
  ```

* <span data-ttu-id="95040-158">Führen Sie den folgenden Befehl aus, um die SignalR-Clientbibliothek mit LibMan abzurufen.</span><span class="sxs-lookup"><span data-stu-id="95040-158">Run the following command to get the SignalR client library by using LibMan.</span></span> <span data-ttu-id="95040-159">Es kann einige Sekunden dauern, bis die Ausgabe angezeigt wird.</span><span class="sxs-lookup"><span data-stu-id="95040-159">You might have to wait a few seconds before seeing output.</span></span>

  ```console
  libman install @aspnet/signalr -p unpkg -d wwwroot/lib/signalr --files dist/browser/signalr.js --files dist/browser/signalr.min.js
  ```

  <span data-ttu-id="95040-160">Die Parameter legen folgende Optionen fest:</span><span class="sxs-lookup"><span data-stu-id="95040-160">The parameters specify the following options:</span></span>
  * <span data-ttu-id="95040-161">Die Verwendung des Anbieters „unpkg“.</span><span class="sxs-lookup"><span data-stu-id="95040-161">Use the unpkg provider.</span></span>
  * <span data-ttu-id="95040-162">Das Kopieren der Dateien in den Zielordner *wwwroot/lib/signalr*.</span><span class="sxs-lookup"><span data-stu-id="95040-162">Copy files to the *wwwroot/lib/signalr* destination.</span></span>
  * <span data-ttu-id="95040-163">Das Kopieren von ausschließlich den angegebenen Dateien.</span><span class="sxs-lookup"><span data-stu-id="95040-163">Copy only the specified files.</span></span>

  <span data-ttu-id="95040-164">Die Ausgabe sieht wie folgt aus:</span><span class="sxs-lookup"><span data-stu-id="95040-164">The output looks like the following example:</span></span>

  ```console
  wwwroot/lib/signalr/dist/browser/signalr.js written to disk
  wwwroot/lib/signalr/dist/browser/signalr.min.js written to disk
  Installed library "@aspnet/signalr@1.0.3" to "wwwroot/lib/signalr"
  ```

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="95040-165">Visual Studio für Mac</span><span class="sxs-lookup"><span data-stu-id="95040-165">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="95040-166">Führen Sie den folgenden Befehl über das **Terminal** aus, um LibMan zu installieren.</span><span class="sxs-lookup"><span data-stu-id="95040-166">In the **Terminal**, run the following command to install LibMan.</span></span>

  ```console
  dotnet tool install -g Microsoft.Web.LibraryManager.Cli
  ```

* <span data-ttu-id="95040-167">Navigieren Sie zum Projektordner (der die Datei *SignalRChat.csproj* enthält).</span><span class="sxs-lookup"><span data-stu-id="95040-167">Navigate to the project folder (the one that contains the *SignalRChat.csproj* file).</span></span>

* <span data-ttu-id="95040-168">Führen Sie den folgenden Befehl aus, um die SignalR-Clientbibliothek mit LibMan abzurufen.</span><span class="sxs-lookup"><span data-stu-id="95040-168">Run the following command to get the SignalR client library by using LibMan.</span></span>

  ```console
  libman install @aspnet/signalr -p unpkg -d wwwroot/lib/signalr --files dist/browser/signalr.js --files dist/browser/signalr.min.js
  ```

  <span data-ttu-id="95040-169">Die Parameter legen folgende Optionen fest:</span><span class="sxs-lookup"><span data-stu-id="95040-169">The parameters specify the following options:</span></span>
  * <span data-ttu-id="95040-170">Die Verwendung des Anbieters „unpkg“.</span><span class="sxs-lookup"><span data-stu-id="95040-170">Use the unpkg provider.</span></span>
  * <span data-ttu-id="95040-171">Das Kopieren der Dateien in den Zielordner *wwwroot/lib/signalr*.</span><span class="sxs-lookup"><span data-stu-id="95040-171">Copy files to the *wwwroot/lib/signalr* destination.</span></span>
  * <span data-ttu-id="95040-172">Das Kopieren von ausschließlich den angegebenen Dateien.</span><span class="sxs-lookup"><span data-stu-id="95040-172">Copy only the specified files.</span></span>

  <span data-ttu-id="95040-173">Die Ausgabe sieht wie folgt aus:</span><span class="sxs-lookup"><span data-stu-id="95040-173">The output looks like the following example:</span></span>

  ```console
  wwwroot/lib/signalr/dist/browser/signalr.js written to disk
  wwwroot/lib/signalr/dist/browser/signalr.min.js written to disk
  Installed library "@aspnet/signalr@1.0.3" to "wwwroot/lib/signalr"
  ```

---

## <a name="create-a-signalr-hub"></a><span data-ttu-id="95040-174">Erstellen eines SignalR-Hubs</span><span class="sxs-lookup"><span data-stu-id="95040-174">Create a SignalR hub</span></span>

<span data-ttu-id="95040-175">Ein *Hub* ist eine Klasse, die als grundlegende Pipeline verwendet wird, mit der Client/Server-Kommunikation verarbeitet wird.</span><span class="sxs-lookup"><span data-stu-id="95040-175">A *hub* is a class that serves as a high-level pipeline that handles client-server communication.</span></span>

* <span data-ttu-id="95040-176">Erstellen Sie im SignalRChat-Projektordner einen *Hubs*-Ordner.</span><span class="sxs-lookup"><span data-stu-id="95040-176">In the SignalRChat project folder, create a *Hubs* folder.</span></span>

* <span data-ttu-id="95040-177">Erstellen Sie im *Hubs*-Ordner eine *ChatHub.cs*-Datei mit folgendem Code:</span><span class="sxs-lookup"><span data-stu-id="95040-177">In the *Hubs* folder, create a *ChatHub.cs* file with the following code:</span></span>

  [!code-csharp[Startup](signalr/sample/Hubs/ChatHub.cs)]

  <span data-ttu-id="95040-178">Die `ChatHub`-Klasse erbt von der `Hub`-Klasse von SignalR.</span><span class="sxs-lookup"><span data-stu-id="95040-178">The `ChatHub` class inherits from the SignalR `Hub` class.</span></span> <span data-ttu-id="95040-179">Die `Hub`-Klasse verwaltet Verbindungen, Gruppen und Messaging.</span><span class="sxs-lookup"><span data-stu-id="95040-179">The `Hub` class manages connections, groups, and messaging.</span></span>

  <span data-ttu-id="95040-180">Die `SendMessage`-Methode kann von jedem verbundenen Client aufgerufen werden.</span><span class="sxs-lookup"><span data-stu-id="95040-180">The `SendMessage` method can be called by any connected client.</span></span> <span data-ttu-id="95040-181">Die empfangenen Nachrichten werden an alle Clients gesendet.</span><span class="sxs-lookup"><span data-stu-id="95040-181">It sends the received message to all clients.</span></span> <span data-ttu-id="95040-182">SignalR-Code ist asynchron, damit die maximale Skalierbarkeit gewährleistet werden kann.</span><span class="sxs-lookup"><span data-stu-id="95040-182">SignalR code is asynchronous to provide maximum scalability.</span></span>

## <a name="configure-signalr"></a><span data-ttu-id="95040-183">Konfigurieren von SignalR</span><span class="sxs-lookup"><span data-stu-id="95040-183">Configure SignalR</span></span>

<span data-ttu-id="95040-184">Der SignalR-Server muss zunächst konfiguriert werden, um Anforderungen an SignalR zu senden.</span><span class="sxs-lookup"><span data-stu-id="95040-184">The SignalR server must be configured to pass SignalR requests to SignalR.</span></span>

* <span data-ttu-id="95040-185">Fügen Sie der *Startup.cs*-Datei den folgenden hervorgehobenen Code zu.</span><span class="sxs-lookup"><span data-stu-id="95040-185">Add the following highlighted code to the *Startup.cs* file.</span></span>

  [!code-csharp[Startup](signalr/sample/Startup.cs?highlight=7,33,52-55)]

  <span data-ttu-id="95040-186">Durch diese Änderungen wird SignalR zum Dependency Injection-System von ASP.NET Core sowie der Middlewarepipeline hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="95040-186">These changes add SignalR to the ASP.NET Core dependency injection system and the middleware pipeline.</span></span>

## <a name="add-signalr-client-code"></a><span data-ttu-id="95040-187">Hinzufügen des SignalR-Clientcodes</span><span class="sxs-lookup"><span data-stu-id="95040-187">Add SignalR client code</span></span>

* <span data-ttu-id="95040-188">Ersetzen Sie den Inhalt in *Pages\Index.cshtml* durch den folgenden Code:</span><span class="sxs-lookup"><span data-stu-id="95040-188">Replace the content in *Pages\Index.cshtml* with the following code:</span></span>

  [!code-cshtml[Index](signalr/sample/Pages/Index.cshtml)]

  <span data-ttu-id="95040-189">Der vorangehende Code:</span><span class="sxs-lookup"><span data-stu-id="95040-189">The preceding code:</span></span>

  * <span data-ttu-id="95040-190">erstellt Textfelder für den Namen und den Nachrichtentext sowie eine Senden-Schaltfläche</span><span class="sxs-lookup"><span data-stu-id="95040-190">Creates text boxes for name and message text, and a submit button.</span></span>
  * <span data-ttu-id="95040-191">erstellt eine Liste mit `id="messagesList"` zum Anzeigen von Nachrichten, die vom SignalR-Hub empfangen werden</span><span class="sxs-lookup"><span data-stu-id="95040-191">Creates a list with `id="messagesList"` for displaying messages that are received from the SignalR hub.</span></span>
  * <span data-ttu-id="95040-192">schließt Skriptverweise sowie den *chat.js*-Anwendungscode, den Sie im folgenden Schritt erstellen, in SignalR ein</span><span class="sxs-lookup"><span data-stu-id="95040-192">Includes script references to SignalR and the *chat.js* application code that you create in the next step.</span></span>

* <span data-ttu-id="95040-193">Erstellen Sie im *wwwroot/js*-Ordner eine *chat.js*-Datei mit folgendem Code:</span><span class="sxs-lookup"><span data-stu-id="95040-193">In the *wwwroot/js* folder, create a *chat.js* file with the following code:</span></span>

  [!code-javascript[Index](signalr/sample/wwwroot/js/chat.js)]

  <span data-ttu-id="95040-194">Der vorangehende Code:</span><span class="sxs-lookup"><span data-stu-id="95040-194">The preceding code:</span></span>

  * <span data-ttu-id="95040-195">erstellt und startet eine Verbindung</span><span class="sxs-lookup"><span data-stu-id="95040-195">Creates and starts a connection.</span></span>
  * <span data-ttu-id="95040-196">fügt einen Handler zur Senden-Schaltfläche hinzu, der Nachrichten an den Hub sendet</span><span class="sxs-lookup"><span data-stu-id="95040-196">Adds to the submit button a handler that sends messages to the hub.</span></span>
  * <span data-ttu-id="95040-197">fügt einen Handler zum Verbindungsobjekt hinzu, der Nachrichten vom Hub empfängt und diese der Liste hinzufügt</span><span class="sxs-lookup"><span data-stu-id="95040-197">Adds to the connection object a handler that receives messages from the hub and adds them to the list.</span></span>

## <a name="run-the-app"></a><span data-ttu-id="95040-198">Ausführen der App</span><span class="sxs-lookup"><span data-stu-id="95040-198">Run the app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="95040-199">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="95040-199">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="95040-200">Drücken Sie **STRG+F5**, um die App ohne Debugging zu starten.</span><span class="sxs-lookup"><span data-stu-id="95040-200">Press **CTRL+F5** to run the app without debugging.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="95040-201">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="95040-201">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="95040-202">Führen Sie über das integrierte Terminal den folgenden Befehl aus:</span><span class="sxs-lookup"><span data-stu-id="95040-202">In the integrated terminal, run the following command:</span></span>

  ```console
  dotnet run -p SignalRChat
  ```
  
# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="95040-203">Visual Studio für Mac</span><span class="sxs-lookup"><span data-stu-id="95040-203">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="95040-204">Wählen Sie im Menü **Ausführen > Starten ohne Debuggen** aus.</span><span class="sxs-lookup"><span data-stu-id="95040-204">From the menu, select **Run > Start Without Debugging**.</span></span>

---

* <span data-ttu-id="95040-205">Kopieren Sie die URL aus der Adressleiste, öffnen Sie eine neue Browserinstanz oder Registerkarte, und fügen Sie die URL in die Adressleiste ein.</span><span class="sxs-lookup"><span data-stu-id="95040-205">Copy the URL from the address bar, open another browser instance or tab, and paste the URL in the address bar.</span></span>

* <span data-ttu-id="95040-206">Geben Sie in einem der beiden Browser einen Namen und eine Nachricht ein, und klicken sie auf **Nachricht senden**.</span><span class="sxs-lookup"><span data-stu-id="95040-206">Choose either browser, enter a name and message, and select the **Send Message** button.</span></span>

  <span data-ttu-id="95040-207">Der Name und die Nachricht werden sofort auf beiden Seiten angezeigt.</span><span class="sxs-lookup"><span data-stu-id="95040-207">The name and message are displayed on both pages instantly.</span></span>

  ![SignalR-Beispiel-App](signalr/_static/signalr-get-started-finished.png)

> [!TIP]
> <span data-ttu-id="95040-209">Wenn die App nicht funktioniert, öffnen Sie die Browser-Entwicklungstools (F12), und wechseln Sie zur Konsole.</span><span class="sxs-lookup"><span data-stu-id="95040-209">If the app doesn't work, open your browser developer tools (F12) and go to the console.</span></span> <span data-ttu-id="95040-210">Möglicherweise werden Fehler in Bezug auf Ihren HTML- und JavaScript-Code angezeigt.</span><span class="sxs-lookup"><span data-stu-id="95040-210">You might see errors related to your HTML and JavaScript code.</span></span> <span data-ttu-id="95040-211">Nehmen wir an, dass Sie z.B. *signalr.js* in einen anderen Ordner als vorgeschrieben platziert haben.</span><span class="sxs-lookup"><span data-stu-id="95040-211">For example, suppose you put *signalr.js* in a different folder than directed.</span></span> <span data-ttu-id="95040-212">In diesem Fall funktioniert der Verweis auf diese Datei nicht, und Ihnen wird der Fehler 404 in der Konsole angezeigt.</span><span class="sxs-lookup"><span data-stu-id="95040-212">In that case the reference to that file won't work and you'll see a 404 error in the console.</span></span>
> <span data-ttu-id="95040-213">![Fehler „signalr.js nicht gefunden“](signalr/_static/f12-console.png)</span><span class="sxs-lookup"><span data-stu-id="95040-213">![signalr.js not found error](signalr/_static/f12-console.png)</span></span>

## <a name="next-steps"></a><span data-ttu-id="95040-214">Nächste Schritte</span><span class="sxs-lookup"><span data-stu-id="95040-214">Next steps</span></span>

<span data-ttu-id="95040-215">In diesem Tutorial haben Sie gelernt, wie die folgenden Aufgaben ausgeführt werden:</span><span class="sxs-lookup"><span data-stu-id="95040-215">In this tutorial, you learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="95040-216">Erstellen Sie ein Web-App-Projekt.</span><span class="sxs-lookup"><span data-stu-id="95040-216">Create a web app project.</span></span>
> * <span data-ttu-id="95040-217">Hinzufügen der SignalR-Clientbibliothek</span><span class="sxs-lookup"><span data-stu-id="95040-217">Add the SignalR client library.</span></span>
> * <span data-ttu-id="95040-218">Erstellen eines SignalR-Hubs</span><span class="sxs-lookup"><span data-stu-id="95040-218">Create a SignalR hub.</span></span>
> * <span data-ttu-id="95040-219">Konfigurieren des Projekts zur Verwendung von SignalR</span><span class="sxs-lookup"><span data-stu-id="95040-219">Configure the project to use SignalR.</span></span>
> * <span data-ttu-id="95040-220">Hinzufügen von Code, der den Hub verwendet, um Nachrichten von einem beliebigen Client an alle verbundenen Clients zu senden</span><span class="sxs-lookup"><span data-stu-id="95040-220">Add code that uses the hub to send messages from any client to all connected clients.</span></span>

<span data-ttu-id="95040-221">Weitere Informationen zu SignalR finden Sie in der folgenden Einführung:</span><span class="sxs-lookup"><span data-stu-id="95040-221">To learn more about SignalR, see the introduction:</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="95040-222">Einführung in ASP.NET Core SignalR</span><span class="sxs-lookup"><span data-stu-id="95040-222">Introduction to ASP.NET Core SignalR</span></span>](xref:signalr/introduction)
