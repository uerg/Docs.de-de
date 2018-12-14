---
title: Erste Schritte mit ASP.NET Core SignalR
author: tdykstra
description: In diesem Tutorial erstellen Sie eine Chat-App, die ASP.NET Core SignalR verwendet.
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 11/30/2018
uid: tutorials/signalr
ms.openlocfilehash: c52041b34d6c9d1d8f06f980c900b805a0933293
ms.sourcegitcommit: 9bb58d7c8dad4bbd03419bcc183d027667fefa20
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/04/2018
ms.locfileid: "52861980"
---
# <a name="tutorial-get-started-with-aspnet-core-signalr"></a><span data-ttu-id="a17d4-103">Tutorial: Erste Schritte mit ASP.NET Core SignalR</span><span class="sxs-lookup"><span data-stu-id="a17d4-103">Tutorial: Get started with ASP.NET Core SignalR</span></span>

<span data-ttu-id="a17d4-104">In diesem Tutorial werden die Grundlagen zur Erstellung einer Echtzeit-App mit SignalR beschrieben.</span><span class="sxs-lookup"><span data-stu-id="a17d4-104">This tutorial teaches the basics of building a real-time app using SignalR.</span></span> <span data-ttu-id="a17d4-105">Sie lernen Folgendes:</span><span class="sxs-lookup"><span data-stu-id="a17d4-105">You learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="a17d4-106">Erstellen Sie ein Webprojekt.</span><span class="sxs-lookup"><span data-stu-id="a17d4-106">Create a web project.</span></span>
> * <span data-ttu-id="a17d4-107">Hinzufügen der SignalR-Clientbibliothek</span><span class="sxs-lookup"><span data-stu-id="a17d4-107">Add the SignalR client library.</span></span>
> * <span data-ttu-id="a17d4-108">Erstellen eines SignalR-Hubs</span><span class="sxs-lookup"><span data-stu-id="a17d4-108">Create a SignalR hub.</span></span>
> * <span data-ttu-id="a17d4-109">Konfigurieren des Projekts zur Verwendung von SignalR</span><span class="sxs-lookup"><span data-stu-id="a17d4-109">Configure the project to use SignalR.</span></span>
> * <span data-ttu-id="a17d4-110">Hinzufügen von Code, mit dem Nachrichten von jedem Client an alle verbundene Clients gesendet werden.</span><span class="sxs-lookup"><span data-stu-id="a17d4-110">Add code that sends messages from any client to all connected clients.</span></span>

<span data-ttu-id="a17d4-111">Am Ende verfügen Sie über eine funktionierende Chat-App:</span><span class="sxs-lookup"><span data-stu-id="a17d4-111">At the end, you'll have a working chat app:</span></span>

![SignalR-Beispiel-App](signalr/_static/signalr-get-started-finished.png)

<span data-ttu-id="a17d4-113">[Zeigen Sie Beispielcode an, oder laden Sie diesen herunter](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/signalr/sample) ([Vorgehensweise zum Herunterladen](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="a17d4-113">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/signalr/sample) ([how to download](xref:index#how-to-download-a-sample)).</span></span>

> [!NOTE]
> <span data-ttu-id="a17d4-114">Wir testen gerade eine vorgeschlagene neue Struktur für das ASP.NET Core-Inhaltsverzeichnis.</span><span class="sxs-lookup"><span data-stu-id="a17d4-114">We’re testing the usability of a proposed new structure for the ASP.NET Core table of contents.</span></span>  <span data-ttu-id="a17d4-115">Falls Sie einige Minuten Zeit haben, um einen Test durchzuführen, in dem Sie sieben unterschiedliche Artikel im aktuellen und vorgeschlagene Inhaltsverzeichnis finden sollen, [klicken Sie hier, um daran teilzunehmen](https://dpk4xbh5.optimalworkshop.com/treejack/aa11wn82).</span><span class="sxs-lookup"><span data-stu-id="a17d4-115">If you have a few minutes to try an exercise of finding 7 different topics in the current or proposed table of contents, please [click here to participate in the study](https://dpk4xbh5.optimalworkshop.com/treejack/aa11wn82).</span></span>


[!INCLUDE [|Prerequisites](~/includes/net-core-prereqs-all-2.2.md)]

## <a name="create-a-web-project"></a><span data-ttu-id="a17d4-116">Erstellen eines Webprojekts</span><span class="sxs-lookup"><span data-stu-id="a17d4-116">Create a web project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="a17d4-117">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a17d4-117">Visual Studio</span></span>](#tab/visual-studio/)

* <span data-ttu-id="a17d4-118">Klicken Sie im Menü auf **Datei > Neues Projekt**.</span><span class="sxs-lookup"><span data-stu-id="a17d4-118">From the menu, select **File > New Project**.</span></span>

* <span data-ttu-id="a17d4-119">Wählen Sie im Dialogfeld **Neues Projekt** **Installiert > Visual C# > Web > ASP.NET Core-Webanwendung** aus.</span><span class="sxs-lookup"><span data-stu-id="a17d4-119">In the **New Project** dialog, select **Installed > Visual C# > Web > ASP.NET Core Web Application**.</span></span> <span data-ttu-id="a17d4-120">Geben Sie als Projektname *SignalRChat* ein.</span><span class="sxs-lookup"><span data-stu-id="a17d4-120">Name the project *SignalRChat*.</span></span>

  ![Dialogfeld „Neues Projekt“ in Visual Studio](signalr/_static/signalr-new-project-dialog.png)

* <span data-ttu-id="a17d4-122">Klicken Sie auf **Webanwendung**, um ein Projekt zu erstellen, das Razor Pages verwendet.</span><span class="sxs-lookup"><span data-stu-id="a17d4-122">Select **Web Application** to create a project that uses Razor Pages.</span></span>

* <span data-ttu-id="a17d4-123">Wählen Sie **.NET Core** als Zielframework sowie **ASP.NET Core 2.2** aus, und klicken Sie auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="a17d4-123">Select a target framework of **.NET Core**, select **ASP.NET Core 2.2**, and click **OK**.</span></span>

  ![Dialogfeld „Neues Projekt“ in Visual Studio](signalr/_static/signalr-new-project-choose-type.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="a17d4-125">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="a17d4-125">Visual Studio Code</span></span>](#tab/visual-studio-code/)

* <span data-ttu-id="a17d4-126">Öffnen Sie das [integrierte Terminal](https://code.visualstudio.com/docs/editor/integrated-terminal) in dem Ordner, in dem der neue Projektordner erstellt wird.</span><span class="sxs-lookup"><span data-stu-id="a17d4-126">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal) to the folder in which the new project folder will be created.</span></span>

* <span data-ttu-id="a17d4-127">Führen Sie die folgenden Befehle aus:</span><span class="sxs-lookup"><span data-stu-id="a17d4-127">Run the following commands:</span></span>

   ```console
   dotnet new webapp -o SignalRChat
   code -r SignalRChat
   ```

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="a17d4-128">Visual Studio für Mac</span><span class="sxs-lookup"><span data-stu-id="a17d4-128">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="a17d4-129">Klicken Sie im Menü auf **Datei > Neue Projektmappe**.</span><span class="sxs-lookup"><span data-stu-id="a17d4-129">From the menu, select **File > New Solution**.</span></span>

* <span data-ttu-id="a17d4-130">Klicken Sie auf **.NET Core > App > ASP.NET Core-Web-App** (Wählen Sie nicht **ASP.NET Core-Web-App (MVC)** aus).</span><span class="sxs-lookup"><span data-stu-id="a17d4-130">Select **.NET Core > App > ASP.NET Core Web App** (Don't select **ASP.NET Core Web App (MVC)**).</span></span>

* <span data-ttu-id="a17d4-131">Klicken Sie auf **Weiter**.</span><span class="sxs-lookup"><span data-stu-id="a17d4-131">Select **Next**.</span></span>

* <span data-ttu-id="a17d4-132">Nennen Sie das Projekt *SignalRChat*, und wählen Sie dann **Erstellen** aus.</span><span class="sxs-lookup"><span data-stu-id="a17d4-132">Name the project *SignalRChat*, and then select **Create**.</span></span>

---

## <a name="add-the-signalr-client-library"></a><span data-ttu-id="a17d4-133">Hinzufügen der SignalR-Clientbibliothek</span><span class="sxs-lookup"><span data-stu-id="a17d4-133">Add the SignalR client library</span></span>

<span data-ttu-id="a17d4-134">Die SignalR-Serverbibliothek ist im Metapaket `Microsoft.AspNetCore.App` enthalten.</span><span class="sxs-lookup"><span data-stu-id="a17d4-134">The SignalR server library is included in the `Microsoft.AspNetCore.App` metapackage.</span></span> <span data-ttu-id="a17d4-135">Die JavaScript-Clientbibliothek ist nicht automatisch im Projekt enthalten.</span><span class="sxs-lookup"><span data-stu-id="a17d4-135">The JavaScript client library isn't automatically included in the project.</span></span> <span data-ttu-id="a17d4-136">In diesem Tutorial verwenden Sie den Bibliotheks-Manager (LibMan), um die Clientbibliothek von *unpkg* abzurufen.</span><span class="sxs-lookup"><span data-stu-id="a17d4-136">For this tutorial, you use Library Manager (LibMan) to get the client library from *unpkg*.</span></span> <span data-ttu-id="a17d4-137">unpkg ist ein CDN (Content Delivery Network), mit dem Sie alles bereitstellen können, was im npm (Node.js-Paket-Manager) zu finden ist.</span><span class="sxs-lookup"><span data-stu-id="a17d4-137">unpkg is a content delivery network (CDN)) that can deliver anything found in npm, the Node.js package manager.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="a17d4-138">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a17d4-138">Visual Studio</span></span>](#tab/visual-studio/)

* <span data-ttu-id="a17d4-139">Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf das Projekt, und wählen Sie **Hinzufügen** > **Client-Side Library** (Clientseitige Bibliothek) aus.</span><span class="sxs-lookup"><span data-stu-id="a17d4-139">In **Solution Explorer**, right-click the project, and select **Add** > **Client-Side Library**.</span></span>

* <span data-ttu-id="a17d4-140">Wählen Sie **unpkg** im Dialogfeld **Add Client-Side Library** (Clientseitige Bibliothek hinzufügen) als **Anbieter** aus.</span><span class="sxs-lookup"><span data-stu-id="a17d4-140">In the **Add Client-Side Library** dialog, for **Provider** select **unpkg**.</span></span> 

* <span data-ttu-id="a17d4-141">Geben Sie `@aspnet/signalr@1` für die **Bibliothek** ein, und wählen Sie die neueste Version aus, die keine Vorschauversion ist.</span><span class="sxs-lookup"><span data-stu-id="a17d4-141">For **Library**, enter `@aspnet/signalr@1`, and select the latest version that isn't preview.</span></span>

  ![Dialogfeld „Clientseitige Bibliothek hinzufügen“: Auswählen der Bibliothek](signalr/_static/libman1.png)

* <span data-ttu-id="a17d4-143">Klicken Sie auf **Choose specific files** (Spezifische Dateien auswählen), erweitern Sie den Ordner *dist/browser*, und wählen Sie *signalr.js* und *signalr.min.js* aus.</span><span class="sxs-lookup"><span data-stu-id="a17d4-143">Select **Choose specific files**, expand the *dist/browser* folder, and select *signalr.js* and *signalr.min.js*.</span></span>

* <span data-ttu-id="a17d4-144">Legen Sie *wwwroot/lib/signalr/* als **Zielspeicherort** fest, und klicken Sie auf **Installieren**.</span><span class="sxs-lookup"><span data-stu-id="a17d4-144">Set **Target Location** to *wwwroot/lib/signalr/*, and select **Install**.</span></span>

  ![Dialogfeld „Clientseitige Bibliothek hinzufügen“: Auswählen der Dateien und des Zielspeicherorts](signalr/_static/libman2.png)

  <span data-ttu-id="a17d4-146">Der Ordner *wwwroot/lib/signalr* wird von LibMan erstellt, und die ausgewählten Dateien werden in ihn hineinkopiert.</span><span class="sxs-lookup"><span data-stu-id="a17d4-146">LibMan creates a *wwwroot/lib/signalr* folder and copies the selected files to it.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="a17d4-147">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="a17d4-147">Visual Studio Code</span></span>](#tab/visual-studio-code/)

* <span data-ttu-id="a17d4-148">Führen Sie den folgenden Befehl über das integrierte Terminal aus, um LibMan zu installieren.</span><span class="sxs-lookup"><span data-stu-id="a17d4-148">In the integrated terminal, run the following command to install LibMan.</span></span>

  ```console
  dotnet tool install -g Microsoft.Web.LibraryManager.Cli
  ```

* <span data-ttu-id="a17d4-149">Führen Sie den folgenden Befehl aus, um die SignalR-Clientbibliothek mit LibMan abzurufen.</span><span class="sxs-lookup"><span data-stu-id="a17d4-149">Run the following command to get the SignalR client library by using LibMan.</span></span> <span data-ttu-id="a17d4-150">Es kann einige Sekunden dauern, bis die Ausgabe angezeigt wird.</span><span class="sxs-lookup"><span data-stu-id="a17d4-150">You might have to wait a few seconds before seeing output.</span></span>

  ```console
  libman install @aspnet/signalr -p unpkg -d wwwroot/lib/signalr --files dist/browser/signalr.js --files dist/browser/signalr.min.js
  ```

  <span data-ttu-id="a17d4-151">Die Parameter legen folgende Optionen fest:</span><span class="sxs-lookup"><span data-stu-id="a17d4-151">The parameters specify the following options:</span></span>
  * <span data-ttu-id="a17d4-152">Die Verwendung des Anbieters „unpkg“.</span><span class="sxs-lookup"><span data-stu-id="a17d4-152">Use the unpkg provider.</span></span>
  * <span data-ttu-id="a17d4-153">Das Kopieren der Dateien in den Zielordner *wwwroot/lib/signalr*.</span><span class="sxs-lookup"><span data-stu-id="a17d4-153">Copy files to the *wwwroot/lib/signalr* destination.</span></span>
  * <span data-ttu-id="a17d4-154">Das Kopieren von ausschließlich den angegebenen Dateien.</span><span class="sxs-lookup"><span data-stu-id="a17d4-154">Copy only the specified files.</span></span>

  <span data-ttu-id="a17d4-155">Die Ausgabe sieht wie folgt aus:</span><span class="sxs-lookup"><span data-stu-id="a17d4-155">The output looks like the following example:</span></span>

  ```console
  wwwroot/lib/signalr/dist/browser/signalr.js written to disk
  wwwroot/lib/signalr/dist/browser/signalr.min.js written to disk
  Installed library "@aspnet/signalr@1.0.3" to "wwwroot/lib/signalr"
  ```

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="a17d4-156">Visual Studio für Mac</span><span class="sxs-lookup"><span data-stu-id="a17d4-156">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="a17d4-157">Führen Sie den folgenden Befehl über das **Terminal** aus, um LibMan zu installieren.</span><span class="sxs-lookup"><span data-stu-id="a17d4-157">In the **Terminal**, run the following command to install LibMan.</span></span>

  ```console
  dotnet tool install -g Microsoft.Web.LibraryManager.Cli
  ```

* <span data-ttu-id="a17d4-158">Navigieren Sie zum Projektordner (der die Datei *SignalRChat.csproj* enthält).</span><span class="sxs-lookup"><span data-stu-id="a17d4-158">Navigate to the project folder (the one that contains the *SignalRChat.csproj* file).</span></span>

* <span data-ttu-id="a17d4-159">Führen Sie den folgenden Befehl aus, um die SignalR-Clientbibliothek mit LibMan abzurufen.</span><span class="sxs-lookup"><span data-stu-id="a17d4-159">Run the following command to get the SignalR client library by using LibMan.</span></span>

  ```console
  libman install @aspnet/signalr -p unpkg -d wwwroot/lib/signalr --files dist/browser/signalr.js --files dist/browser/signalr.min.js
  ```

  <span data-ttu-id="a17d4-160">Die Parameter legen folgende Optionen fest:</span><span class="sxs-lookup"><span data-stu-id="a17d4-160">The parameters specify the following options:</span></span>
  * <span data-ttu-id="a17d4-161">Die Verwendung des Anbieters „unpkg“.</span><span class="sxs-lookup"><span data-stu-id="a17d4-161">Use the unpkg provider.</span></span>
  * <span data-ttu-id="a17d4-162">Das Kopieren der Dateien in den Zielordner *wwwroot/lib/signalr*.</span><span class="sxs-lookup"><span data-stu-id="a17d4-162">Copy files to the *wwwroot/lib/signalr* destination.</span></span>
  * <span data-ttu-id="a17d4-163">Das Kopieren von ausschließlich den angegebenen Dateien.</span><span class="sxs-lookup"><span data-stu-id="a17d4-163">Copy only the specified files.</span></span>

  <span data-ttu-id="a17d4-164">Die Ausgabe sieht wie folgt aus:</span><span class="sxs-lookup"><span data-stu-id="a17d4-164">The output looks like the following example:</span></span>

  ```console
  wwwroot/lib/signalr/dist/browser/signalr.js written to disk
  wwwroot/lib/signalr/dist/browser/signalr.min.js written to disk
  Installed library "@aspnet/signalr@1.0.3" to "wwwroot/lib/signalr"
  ```

---

## <a name="create-a-signalr-hub"></a><span data-ttu-id="a17d4-165">Erstellen eines SignalR-Hubs</span><span class="sxs-lookup"><span data-stu-id="a17d4-165">Create a SignalR hub</span></span>

<span data-ttu-id="a17d4-166">Ein *Hub* ist eine Klasse, die als grundlegende Pipeline verwendet wird, mit der Client/Server-Kommunikation verarbeitet wird.</span><span class="sxs-lookup"><span data-stu-id="a17d4-166">A *hub* is a class that serves as a high-level pipeline that handles client-server communication.</span></span>

* <span data-ttu-id="a17d4-167">Erstellen Sie im SignalRChat-Projektordner einen *Hubs*-Ordner.</span><span class="sxs-lookup"><span data-stu-id="a17d4-167">In the SignalRChat project folder, create a *Hubs* folder.</span></span>

* <span data-ttu-id="a17d4-168">Erstellen Sie im *Hubs*-Ordner eine *ChatHub.cs*-Datei mit folgendem Code:</span><span class="sxs-lookup"><span data-stu-id="a17d4-168">In the *Hubs* folder, create a *ChatHub.cs* file with the following code:</span></span>

  [!code-csharp[Startup](signalr/sample/Hubs/ChatHub.cs)]

  <span data-ttu-id="a17d4-169">Die `ChatHub`-Klasse erbt von der `Hub`-Klasse von SignalR.</span><span class="sxs-lookup"><span data-stu-id="a17d4-169">The `ChatHub` class inherits from the SignalR `Hub` class.</span></span> <span data-ttu-id="a17d4-170">Die `Hub`-Klasse verwaltet Verbindungen, Gruppen und Messaging.</span><span class="sxs-lookup"><span data-stu-id="a17d4-170">The `Hub` class manages connections, groups, and messaging.</span></span>

  <span data-ttu-id="a17d4-171">Die `SendMessage`-Methode kann von jedem verbundenen Client aufgerufen werden.</span><span class="sxs-lookup"><span data-stu-id="a17d4-171">The `SendMessage` method can be called by any connected client.</span></span> <span data-ttu-id="a17d4-172">Die empfangenen Nachrichten werden an alle Clients gesendet.</span><span class="sxs-lookup"><span data-stu-id="a17d4-172">It sends the received message to all clients.</span></span> <span data-ttu-id="a17d4-173">SignalR-Code ist asynchron, damit die maximale Skalierbarkeit gewährleistet werden kann.</span><span class="sxs-lookup"><span data-stu-id="a17d4-173">SignalR code is asynchronous to provide maximum scalability.</span></span>

## <a name="configure-signalr"></a><span data-ttu-id="a17d4-174">Konfigurieren von SignalR</span><span class="sxs-lookup"><span data-stu-id="a17d4-174">Configure SignalR</span></span>

<span data-ttu-id="a17d4-175">Der SignalR-Server muss zunächst konfiguriert werden, um Anforderungen an SignalR zu senden.</span><span class="sxs-lookup"><span data-stu-id="a17d4-175">The SignalR server must be configured to pass SignalR requests to SignalR.</span></span>

* <span data-ttu-id="a17d4-176">Fügen Sie der *Startup.cs*-Datei den folgenden hervorgehobenen Code zu.</span><span class="sxs-lookup"><span data-stu-id="a17d4-176">Add the following highlighted code to the *Startup.cs* file.</span></span>

  [!code-csharp[Startup](signalr/sample/Startup.cs?highlight=7,33,52-55)]

  <span data-ttu-id="a17d4-177">Durch diese Änderungen wird SignalR zum Dependency Injection-System von ASP.NET Core sowie der Middlewarepipeline hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="a17d4-177">These changes add SignalR to the ASP.NET Core dependency injection system and the middleware pipeline.</span></span>

## <a name="add-signalr-client-code"></a><span data-ttu-id="a17d4-178">Hinzufügen des SignalR-Clientcodes</span><span class="sxs-lookup"><span data-stu-id="a17d4-178">Add SignalR client code</span></span>

* <span data-ttu-id="a17d4-179">Ersetzen Sie den Inhalt in *Pages\Index.cshtml* durch den folgenden Code:</span><span class="sxs-lookup"><span data-stu-id="a17d4-179">Replace the content in *Pages\Index.cshtml* with the following code:</span></span>

  [!code-cshtml[Index](signalr/sample/Pages/Index.cshtml)]

  <span data-ttu-id="a17d4-180">Der vorangehende Code:</span><span class="sxs-lookup"><span data-stu-id="a17d4-180">The preceding code:</span></span>

  * <span data-ttu-id="a17d4-181">erstellt Textfelder für den Namen und den Nachrichtentext sowie eine Senden-Schaltfläche</span><span class="sxs-lookup"><span data-stu-id="a17d4-181">Creates text boxes for name and message text, and a submit button.</span></span>
  * <span data-ttu-id="a17d4-182">erstellt eine Liste mit `id="messagesList"` zum Anzeigen von Nachrichten, die vom SignalR-Hub empfangen werden</span><span class="sxs-lookup"><span data-stu-id="a17d4-182">Creates a list with `id="messagesList"` for displaying messages that are received from the SignalR hub.</span></span>
  * <span data-ttu-id="a17d4-183">schließt Skriptverweise sowie den *chat.js*-Anwendungscode, den Sie im folgenden Schritt erstellen, in SignalR ein</span><span class="sxs-lookup"><span data-stu-id="a17d4-183">Includes script references to SignalR and the *chat.js* application code that you create in the next step.</span></span>

* <span data-ttu-id="a17d4-184">Erstellen Sie im *wwwroot/js*-Ordner eine *chat.js*-Datei mit folgendem Code:</span><span class="sxs-lookup"><span data-stu-id="a17d4-184">In the *wwwroot/js* folder, create a *chat.js* file with the following code:</span></span>

  [!code-javascript[Index](signalr/sample/wwwroot/js/chat.js)]

  <span data-ttu-id="a17d4-185">Der vorangehende Code:</span><span class="sxs-lookup"><span data-stu-id="a17d4-185">The preceding code:</span></span>

  * <span data-ttu-id="a17d4-186">erstellt und startet eine Verbindung</span><span class="sxs-lookup"><span data-stu-id="a17d4-186">Creates and starts a connection.</span></span>
  * <span data-ttu-id="a17d4-187">fügt einen Handler zur Senden-Schaltfläche hinzu, der Nachrichten an den Hub sendet</span><span class="sxs-lookup"><span data-stu-id="a17d4-187">Adds to the submit button a handler that sends messages to the hub.</span></span>
  * <span data-ttu-id="a17d4-188">fügt einen Handler zum Verbindungsobjekt hinzu, der Nachrichten vom Hub empfängt und diese der Liste hinzufügt</span><span class="sxs-lookup"><span data-stu-id="a17d4-188">Adds to the connection object a handler that receives messages from the hub and adds them to the list.</span></span>

## <a name="run-the-app"></a><span data-ttu-id="a17d4-189">Ausführen der App</span><span class="sxs-lookup"><span data-stu-id="a17d4-189">Run the app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="a17d4-190">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a17d4-190">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="a17d4-191">Drücken Sie **STRG+F5**, um die App ohne Debugging zu starten.</span><span class="sxs-lookup"><span data-stu-id="a17d4-191">Press **CTRL+F5** to run the app without debugging.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="a17d4-192">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="a17d4-192">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="a17d4-193">Führen Sie über das integrierte Terminal den folgenden Befehl aus:</span><span class="sxs-lookup"><span data-stu-id="a17d4-193">In the integrated terminal, run the following command:</span></span>

  ```console
  dotnet run -p SignalRChat.csproj
  ```
  
# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="a17d4-194">Visual Studio für Mac</span><span class="sxs-lookup"><span data-stu-id="a17d4-194">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="a17d4-195">Wählen Sie im Menü **Ausführen > Starten ohne Debuggen** aus.</span><span class="sxs-lookup"><span data-stu-id="a17d4-195">From the menu, select **Run > Start Without Debugging**.</span></span>

---

* <span data-ttu-id="a17d4-196">Kopieren Sie die URL aus der Adressleiste, öffnen Sie eine neue Browserinstanz oder Registerkarte, und fügen Sie die URL in die Adressleiste ein.</span><span class="sxs-lookup"><span data-stu-id="a17d4-196">Copy the URL from the address bar, open another browser instance or tab, and paste the URL in the address bar.</span></span>

* <span data-ttu-id="a17d4-197">Geben Sie in einem der beiden Browser einen Namen und eine Nachricht ein, und klicken sie auf **Nachricht senden**.</span><span class="sxs-lookup"><span data-stu-id="a17d4-197">Choose either browser, enter a name and message, and select the **Send Message** button.</span></span>

  <span data-ttu-id="a17d4-198">Der Name und die Nachricht werden sofort auf beiden Seiten angezeigt.</span><span class="sxs-lookup"><span data-stu-id="a17d4-198">The name and message are displayed on both pages instantly.</span></span>

  ![SignalR-Beispiel-App](signalr/_static/signalr-get-started-finished.png)

> [!TIP]
> <span data-ttu-id="a17d4-200">Wenn die App nicht funktioniert, öffnen Sie die Browser-Entwicklungstools (F12), und wechseln Sie zur Konsole.</span><span class="sxs-lookup"><span data-stu-id="a17d4-200">If the app doesn't work, open your browser developer tools (F12) and go to the console.</span></span> <span data-ttu-id="a17d4-201">Möglicherweise werden Fehler in Bezug auf Ihren HTML- und JavaScript-Code angezeigt.</span><span class="sxs-lookup"><span data-stu-id="a17d4-201">You might see errors related to your HTML and JavaScript code.</span></span> <span data-ttu-id="a17d4-202">Nehmen wir an, dass Sie z.B. *signalr.js* in einen anderen Ordner als vorgeschrieben platziert haben.</span><span class="sxs-lookup"><span data-stu-id="a17d4-202">For example, suppose you put *signalr.js* in a different folder than directed.</span></span> <span data-ttu-id="a17d4-203">In diesem Fall funktioniert der Verweis auf diese Datei nicht, und Ihnen wird der Fehler 404 in der Konsole angezeigt.</span><span class="sxs-lookup"><span data-stu-id="a17d4-203">In that case the reference to that file won't work and you'll see a 404 error in the console.</span></span>
> <span data-ttu-id="a17d4-204">![Fehler „signalr.js nicht gefunden“](signalr/_static/f12-console.png)</span><span class="sxs-lookup"><span data-stu-id="a17d4-204">![signalr.js not found error](signalr/_static/f12-console.png)</span></span>

## <a name="next-steps"></a><span data-ttu-id="a17d4-205">Nächste Schritte</span><span class="sxs-lookup"><span data-stu-id="a17d4-205">Next steps</span></span>

<span data-ttu-id="a17d4-206">In diesem Tutorial haben Sie gelernt, wie die folgenden Aufgaben ausgeführt werden:</span><span class="sxs-lookup"><span data-stu-id="a17d4-206">In this tutorial, you learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="a17d4-207">Erstellen Sie ein Web-App-Projekt.</span><span class="sxs-lookup"><span data-stu-id="a17d4-207">Create a web app project.</span></span>
> * <span data-ttu-id="a17d4-208">Hinzufügen der SignalR-Clientbibliothek</span><span class="sxs-lookup"><span data-stu-id="a17d4-208">Add the SignalR client library.</span></span>
> * <span data-ttu-id="a17d4-209">Erstellen eines SignalR-Hubs</span><span class="sxs-lookup"><span data-stu-id="a17d4-209">Create a SignalR hub.</span></span>
> * <span data-ttu-id="a17d4-210">Konfigurieren des Projekts zur Verwendung von SignalR</span><span class="sxs-lookup"><span data-stu-id="a17d4-210">Configure the project to use SignalR.</span></span>
> * <span data-ttu-id="a17d4-211">Hinzufügen von Code, der den Hub verwendet, um Nachrichten von einem beliebigen Client an alle verbundenen Clients zu senden</span><span class="sxs-lookup"><span data-stu-id="a17d4-211">Add code that uses the hub to send messages from any client to all connected clients.</span></span>

<span data-ttu-id="a17d4-212">Weitere Informationen zu SignalR finden Sie in der folgenden Einführung:</span><span class="sxs-lookup"><span data-stu-id="a17d4-212">To learn more about SignalR, see the introduction:</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="a17d4-213">Einführung in ASP.NET Core SignalR</span><span class="sxs-lookup"><span data-stu-id="a17d4-213">Introduction to ASP.NET Core SignalR</span></span>](xref:signalr/introduction)
