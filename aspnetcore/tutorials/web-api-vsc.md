---
title: Erstellen einer Web-API mit ASP.NET Core und Visual Studio Code
description: Erstellen einer Web-API unter macOS, Linux oder Windows mit ASP.NET Core MVC und Visual Studio Code
author: rick-anderson
ms.author: riande
ms.date: 09/22/2017
ms.topic: get-started-article
ms.prod: asp.net-core
ms.technology: aspnet
keywords: ASP.NET Core, WebAPI, Web-API, REST, Mac, Linux, HTTP, Service, HTTP-Dienst, VS Code
manager: wpickett
ms.assetid: 830b4bf5-dd14-423e-9f59-764a6f13a8f6
uid: tutorials/web-api-vsc
ms.openlocfilehash: e09943b2f810d04456a65589976aa07065a9f010
ms.sourcegitcommit: e6bcd56a4b11e20ff55df004971f9ed384937342
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/22/2017
---
# <a name="create-a-web-api-with-aspnet-core-mvc-and-visual-studio-code-on-linux-macos-and-windows"></a><span data-ttu-id="dde77-104">Erstellen einer Web-API mit ASP.NET Core MVC und Visual Studio Code unter macOS Linux und Windows</span><span class="sxs-lookup"><span data-stu-id="dde77-104">Create a Web API with ASP.NET Core MVC and Visual Studio Code on Linux, macOS, and Windows</span></span>

<span data-ttu-id="dde77-105">Von [Rick Anderson](https://twitter.com/RickAndMSFT) und [Mike Wasson](https://github.com/mikewasson)</span><span class="sxs-lookup"><span data-stu-id="dde77-105">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Mike Wasson](https://github.com/mikewasson)</span></span>

<span data-ttu-id="dde77-106">In diesem Tutorial erstellen Sie eine Web-API zum Verwalten einer Liste von „To-Do“-Elementen.</span><span class="sxs-lookup"><span data-stu-id="dde77-106">In this tutorial, you’ll build a web API for managing a list of "to-do" items.</span></span> <span data-ttu-id="dde77-107">Sie werden keine Benutzeroberfläche erstellen.</span><span class="sxs-lookup"><span data-stu-id="dde77-107">You won’t build a UI.</span></span>

<span data-ttu-id="dde77-108">Es gibt drei Versionen dieses Tutorials:</span><span class="sxs-lookup"><span data-stu-id="dde77-108">There are 3 versions of this tutorial:</span></span>

* <span data-ttu-id="dde77-109">macOS, Linux, Windows: Web-API mit Visual Studio Code (dieses Tutorial)</span><span class="sxs-lookup"><span data-stu-id="dde77-109">macOS, Linux, Windows: Web API with Visual Studio Code (This tutorial)</span></span>
* <span data-ttu-id="dde77-110">macOS: [Web-API mit Visual Studio für Mac](xref:tutorials/first-web-api-mac)</span><span class="sxs-lookup"><span data-stu-id="dde77-110">macOS: [Web API with Visual Studio for Mac](xref:tutorials/first-web-api-mac)</span></span>
* <span data-ttu-id="dde77-111">Windows: [Web-API mit Visual Studio für Windows](xref:tutorials/first-web-api)</span><span class="sxs-lookup"><span data-stu-id="dde77-111">Windows: [Web API with Visual Studio for Windows](xref:tutorials/first-web-api)</span></span>

<!-- WARNING: The code AND images in this doc are used by uid: tutorials/web-api-vsc, tutorials/first-web-api-mac and tutorials/first-web-api. If you change any code/images in this tutorial, update uid: tutorials/web-api-vsc -->

[!INCLUDE[template files](../includes/webApi/intro.md)]

## <a name="set-up-your-development-environment"></a><span data-ttu-id="dde77-112">Einrichten der Entwicklungsumgebung</span><span class="sxs-lookup"><span data-stu-id="dde77-112">Set up your development environment</span></span>

<span data-ttu-id="dde77-113">Sie müssen Folgendes herunterladen und installieren:</span><span class="sxs-lookup"><span data-stu-id="dde77-113">Download and install:</span></span>
- [<span data-ttu-id="dde77-114">.NET Core</span><span class="sxs-lookup"><span data-stu-id="dde77-114">.NET Core</span></span>](https://www.microsoft.com/net/core)
- [<span data-ttu-id="dde77-115">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="dde77-115">Visual Studio Code</span></span>](https://code.visualstudio.com)
- <span data-ttu-id="dde77-116">Visual Studio Code [C#-Erweiterung](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)</span><span class="sxs-lookup"><span data-stu-id="dde77-116">Visual Studio Code [C# extension](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)</span></span>

## <a name="create-the-project"></a><span data-ttu-id="dde77-117">Erstellen eines Projekts</span><span class="sxs-lookup"><span data-stu-id="dde77-117">Create the project</span></span>

<span data-ttu-id="dde77-118">Führen Sie in einer Konsole die folgenden Befehle aus:</span><span class="sxs-lookup"><span data-stu-id="dde77-118">From a console, run the following commands:</span></span>

```console
mkdir TodoApi
cd TodoApi
dotnet new webapi
```

<span data-ttu-id="dde77-119">Öffnen Sie den Ordner *TodoApi* in Visual Studio Code, und wählen Sie die Datei *Startup.cs* aus.</span><span class="sxs-lookup"><span data-stu-id="dde77-119">Open the *TodoApi* folder in Visual Studio Code (VS Code) and select the *Startup.cs* file.</span></span>

- <span data-ttu-id="dde77-120">Klicken Sie auf **Ja**, wenn die **Warnung** „Required assets to build and debug are missing from 'TodoApi'. Add them?“ („Die erforderlichen Objekte für die Erstellung und das Debugging sind in 'TodoApi' nicht vorhanden.</span><span class="sxs-lookup"><span data-stu-id="dde77-120">Select **Yes** to the **Warn** message "Required assets to build and debug are missing from 'TodoApi'.</span></span> <span data-ttu-id="dde77-121">Sollen Sie hinzugefügt werden?“) angezeigt wird.</span><span class="sxs-lookup"><span data-stu-id="dde77-121">Add them?"</span></span>
- <span data-ttu-id="dde77-122">Klicken Sie bei der **Infomeldung** „There are unresolved depedencies“ („Es bestehen ungelöste Abhängigkeiten“) auf **Wiederherstellen**.</span><span class="sxs-lookup"><span data-stu-id="dde77-122">Select **Restore** to the **Info** message "There are unresolved dependencies".</span></span>

<!-- uid: tutorials/first-mvc-app-xplat/start-mvc uses the pic below. If you change it, make sure it's consistent -->

![VS Code mit der Warnung „Required assets to build and debug are missing from „MvcMovie“. Add them?“ („Die erforderlichen Objekte für die Erstellung und das Debugging sind in 'TodoApi' nicht vorhanden.](web-api-vsc/_static/vsc_restore.png)

<span data-ttu-id="dde77-126">Drücken Sie **Debuggen** (F5), um das Programm zu erstellen und auszuführen.</span><span class="sxs-lookup"><span data-stu-id="dde77-126">Press **Debug** (F5) to build and run the program.</span></span> <span data-ttu-id="dde77-127">Navigieren Sie in einem Browser zu „http://localhost:5000/api/values“.</span><span class="sxs-lookup"><span data-stu-id="dde77-127">In a browser navigate to http://localhost:5000/api/values .</span></span> <span data-ttu-id="dde77-128">Folgendes wird angezeigt:</span><span class="sxs-lookup"><span data-stu-id="dde77-128">The following is displayed:</span></span>

`["value1","value2"]`

<span data-ttu-id="dde77-129">In der [Visual Studio Code-Hilfe](#visual-studio-code-help) finden Sie Tipps zum Arbeiten mit VS Code.</span><span class="sxs-lookup"><span data-stu-id="dde77-129">See [Visual Studio Code help](#visual-studio-code-help) for tips on using VS Code.</span></span>

## <a name="add-support-for-entity-framework-core"></a><span data-ttu-id="dde77-130">Hinzufügen der Unterstützung für Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="dde77-130">Add support for Entity Framework Core</span></span>

<span data-ttu-id="dde77-131">Bearbeiten Sie die Datei *TodoApi.csproj* so, dass der Datenbankanbieter [Entity Framework Core InMemory](https://docs.microsoft.com/ef/core/providers/in-memory/) installiert wird.</span><span class="sxs-lookup"><span data-stu-id="dde77-131">Edit the *TodoApi.csproj* file to install the [Entity Framework Core InMemory](https://docs.microsoft.com/ef/core/providers/in-memory/) database provider.</span></span> <span data-ttu-id="dde77-132">Dieser Datenbankanbieter ermöglicht, dass Entity Framework Core mit einer In-Memory Database verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="dde77-132">This database provider allows Entity Framework Core to be used with an in-memory database.</span></span>

[!code-xml[Main](web-api-vsc/sample/TodoApi/TodoApi.csproj?highlight=12)]

<span data-ttu-id="dde77-133">Führen Sie `dotnet restore` zum Herunterladen und Installieren des Datenbankanbieters „EF Core InMemory“ aus.</span><span class="sxs-lookup"><span data-stu-id="dde77-133">Run `dotnet restore` to download and install the EF Core InMemory DB provider.</span></span> <span data-ttu-id="dde77-134">Sie können `dotnet restore` im Terminal ausführen oder `⌘⇧P` (macOS) oder `Ctrl+Shift+P` (Linux) in VS Code und dann **.NET** eingeben.</span><span class="sxs-lookup"><span data-stu-id="dde77-134">You can run `dotnet restore` from the terminal or enter `⌘⇧P` (macOS) or `Ctrl+Shift+P` (Linux) in VS Code and then type **.NET**.</span></span> <span data-ttu-id="dde77-135">Wählen Sie **.NET: Pakete wiederherstellen** aus.</span><span class="sxs-lookup"><span data-stu-id="dde77-135">Select **.NET: Restore Packages**.</span></span>

## <a name="add-a-model-class"></a><span data-ttu-id="dde77-136">Hinzufügen einer Modellklasse</span><span class="sxs-lookup"><span data-stu-id="dde77-136">Add a model class</span></span>

<span data-ttu-id="dde77-137">Ein Modell ist ein Objekt, das die Daten in Ihrer Anwendung darstellt.</span><span class="sxs-lookup"><span data-stu-id="dde77-137">A model is an object that represents the data in your application.</span></span> <span data-ttu-id="dde77-138">In diesem Fall ist das einzige Modell ein To-Do-Element.</span><span class="sxs-lookup"><span data-stu-id="dde77-138">In this case, the only model is a to-do item.</span></span>

<span data-ttu-id="dde77-139">Fügen Sie einen Ordner namens *Models* hinzu.</span><span class="sxs-lookup"><span data-stu-id="dde77-139">Add a folder named *Models*.</span></span> <span data-ttu-id="dde77-140">Hinweis: Sie können Modellklassen überall in Ihrem Projekt unterbringen, aber gemäß Konvention wird der Ordner *Models* verwendet.</span><span class="sxs-lookup"><span data-stu-id="dde77-140">You can put model classes anywhere in your project, but the *Models* folder is used by convention.</span></span>

<span data-ttu-id="dde77-141">Fügen Sie eine `TodoItem`-Klasse mit dem folgenden Code hinzu:</span><span class="sxs-lookup"><span data-stu-id="dde77-141">Add a `TodoItem` class with the following code:</span></span>

[!code-csharp[Main](first-web-api/sample/TodoApi/Models/TodoItem.cs)]

<span data-ttu-id="dde77-142">Die Datenbank generiert die `Id`, wenn ein `TodoItem` erstellt wird.</span><span class="sxs-lookup"><span data-stu-id="dde77-142">The database generates the `Id` when a `TodoItem` is created.</span></span>

## <a name="create-the-database-context"></a><span data-ttu-id="dde77-143">Erstellen des Datenbankkontexts</span><span class="sxs-lookup"><span data-stu-id="dde77-143">Create the database context</span></span>

<span data-ttu-id="dde77-144">Der *Datenbankkontext* ist die Hauptklasse, die die Entity Framework-Funktionen für ein angegebenes Datenmodell koordiniert.</span><span class="sxs-lookup"><span data-stu-id="dde77-144">The *database context* is the main class that coordinates Entity Framework functionality for a given data model.</span></span> <span data-ttu-id="dde77-145">Sie können diese Klasse durch Ableiten von der `Microsoft.EntityFrameworkCore.DbContext`-Klasse erstellen.</span><span class="sxs-lookup"><span data-stu-id="dde77-145">You create this class by deriving from the `Microsoft.EntityFrameworkCore.DbContext` class.</span></span>

<span data-ttu-id="dde77-146">Fügen Sie eine `TodoContext`-Klasse zum Ordner *Models* hinzu:</span><span class="sxs-lookup"><span data-stu-id="dde77-146">Add a `TodoContext` class in the *Models* folder:</span></span>

[!code-csharp[Main](first-web-api/sample/TodoApi/Models/TodoContext.cs)]

[!INCLUDE[Register the database context](../includes/webApi/register_dbContext.md)]

## <a name="add-a-controller"></a><span data-ttu-id="dde77-147">Hinzufügen eines Controllers</span><span class="sxs-lookup"><span data-stu-id="dde77-147">Add a controller</span></span>

<span data-ttu-id="dde77-148">Erstellen Sie im Ordner *Controllers* eine Klasse namens `TodoController`.</span><span class="sxs-lookup"><span data-stu-id="dde77-148">In the *Controllers* folder, create a class named `TodoController`.</span></span> <span data-ttu-id="dde77-149">Fügen Sie den folgenden Code hinzu:</span><span class="sxs-lookup"><span data-stu-id="dde77-149">Add the following code:</span></span>

[!INCLUDE[code and get todo items](../includes/webApi/getTodoItems.md)]

### <a name="launch-the-app"></a><span data-ttu-id="dde77-150">Starten der App</span><span class="sxs-lookup"><span data-stu-id="dde77-150">Launch the app</span></span>

<span data-ttu-id="dde77-151">Drücken Sie in Visual Studio Code F5, um die App zu starten.</span><span class="sxs-lookup"><span data-stu-id="dde77-151">In VS Code, press F5 to launch the app.</span></span> <span data-ttu-id="dde77-152">Navigieren Sie zu „http://localhost:5000/api/todo“ (zum zuvor erstellten Controller `Todo`).</span><span class="sxs-lookup"><span data-stu-id="dde77-152">Navigate to  http://localhost:5000/api/todo   (The `Todo` controller we just created).</span></span>

[!INCLUDE[last part of web API](../includes/webApi/end.md)]

## <a name="visual-studio-code-help"></a><span data-ttu-id="dde77-153">Hilfe zu Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="dde77-153">Visual Studio Code help</span></span>

- [<span data-ttu-id="dde77-154">Erste Schritte</span><span class="sxs-lookup"><span data-stu-id="dde77-154">Getting started</span></span>](https://code.visualstudio.com/docs)
- [<span data-ttu-id="dde77-155">Debuggen</span><span class="sxs-lookup"><span data-stu-id="dde77-155">Debugging</span></span>](https://code.visualstudio.com/docs/editor/debugging)
- [<span data-ttu-id="dde77-156">Integriertes Terminal</span><span class="sxs-lookup"><span data-stu-id="dde77-156">Integrated terminal</span></span>](https://code.visualstudio.com/docs/editor/integrated-terminal)
- [<span data-ttu-id="dde77-157">Tastenkombinationen</span><span class="sxs-lookup"><span data-stu-id="dde77-157">Keyboard shortcuts</span></span>](https://code.visualstudio.com/docs/getstarted/keybindings#_keyboard-shortcuts-reference)

  - [<span data-ttu-id="dde77-158">Mac-Tastenkombinationen</span><span class="sxs-lookup"><span data-stu-id="dde77-158">Mac keyboard shortcuts</span></span>](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-macos.pdf)
  - [<span data-ttu-id="dde77-159">Linux-Tastenkombinationen</span><span class="sxs-lookup"><span data-stu-id="dde77-159">Linux keyboard shortcuts</span></span>](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-linux.pdf)
  - [<span data-ttu-id="dde77-160">Windows-Tastenkombinationen</span><span class="sxs-lookup"><span data-stu-id="dde77-160">Windows keyboard shortcuts</span></span>](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-windows.pdf)

[!INCLUDE[next steps](../includes/webApi/next.md)]


