---
title: Erstellen einer Web-API mit ASP.NET Core und Visual Studio Code
author: rick-anderson
description: Erstellen einer Web-API unter macOS, Linux oder Windows mit ASP.NET Core MVC und Visual Studio Code
keywords: ASP.NET Core, WebAPI, Web API, REST, Mac, Linux,HTTP, Dienst, HTTP-Dienst, Visual Studio Code
ms.author: riande
manager: wpickett
ms.date: 5/24/2017
ms.topic: get-started-article
ms.assetid: 830b4bf5-dd14-423e-9f59-764a6f13a8f6
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/web-api-vsc
ms.openlocfilehash: 17687e38aae066bdab4663268a2af54f20a6ad75
ms.sourcegitcommit: 9cdbfd0d670d70b9c354216aabee260c52dad5ee
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/12/2017
---
# <a name="create-a-web-api-with-aspnet-core-mvc-and-visual-studio-code-on-linux-macos-and-windows"></a><span data-ttu-id="f4e49-104">Erstellen einer Web-API mit ASP.NET Core MVC und Visual Studio Code unter macOS Linux und Windows</span><span class="sxs-lookup"><span data-stu-id="f4e49-104">Create a Web API with ASP.NET Core MVC and Visual Studio Code on Linux, macOS, and Windows</span></span>

<span data-ttu-id="f4e49-105">Von [Rick Anderson](https://twitter.com/RickAndMSFT) und [Mike Wasson](https://github.com/mikewasson)</span><span class="sxs-lookup"><span data-stu-id="f4e49-105">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Mike Wasson](https://github.com/mikewasson)</span></span>

<span data-ttu-id="f4e49-106">In diesem Tutorial erstellen Sie eine Web-API zum Verwalten einer Liste von „To-Do“-Elementen.</span><span class="sxs-lookup"><span data-stu-id="f4e49-106">In this tutorial, you’ll build a web API for managing a list of "to-do" items.</span></span> <span data-ttu-id="f4e49-107">Sie werden keine Benutzeroberfläche erstellen.</span><span class="sxs-lookup"><span data-stu-id="f4e49-107">You won’t build a UI.</span></span>

<span data-ttu-id="f4e49-108">Es gibt drei Versionen dieses Tutorials:</span><span class="sxs-lookup"><span data-stu-id="f4e49-108">There are 3 versions of this tutorial:</span></span>

* <span data-ttu-id="f4e49-109">macOS, Linux, Windows: Web-API mit Visual Studio Code (dieses Tutorial)</span><span class="sxs-lookup"><span data-stu-id="f4e49-109">macOS, Linux, Windows: Web API with Visual Studio Code (This tutorial)</span></span>
* <span data-ttu-id="f4e49-110">macOS: [Web-API mit Visual Studio für Mac](xref:tutorials/first-web-api-mac)</span><span class="sxs-lookup"><span data-stu-id="f4e49-110">macOS: [Web API with Visual Studio for Mac](xref:tutorials/first-web-api-mac)</span></span>
* <span data-ttu-id="f4e49-111">Windows: [Web-API mit Visual Studio für Windows](xref:tutorials/first-web-api)</span><span class="sxs-lookup"><span data-stu-id="f4e49-111">Windows: [Web API with Visual Studio for Windows](xref:tutorials/first-web-api)</span></span>

<!-- WARNING: The code AND images in this doc are used by uid: tutorials/web-api-vsc, tutorials/first-web-api-mac and tutorials/first-web-api. If you change any code/images in this tutorial, update uid: tutorials/web-api-vsc -->

[!INCLUDE[template files](../includes/webApi/intro.md)]

## <a name="set-up-your-development-environment"></a><span data-ttu-id="f4e49-112">Einrichten der Entwicklungsumgebung</span><span class="sxs-lookup"><span data-stu-id="f4e49-112">Set up your development environment</span></span>

<span data-ttu-id="f4e49-113">Sie müssen Folgendes herunterladen und installieren:</span><span class="sxs-lookup"><span data-stu-id="f4e49-113">Download and install:</span></span>
- [<span data-ttu-id="f4e49-114">.NET Core</span><span class="sxs-lookup"><span data-stu-id="f4e49-114">.NET Core</span></span>](https://www.microsoft.com/net/core)
- [<span data-ttu-id="f4e49-115">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="f4e49-115">Visual Studio Code</span></span>](https://code.visualstudio.com)
- <span data-ttu-id="f4e49-116">Visual Studio Code [C#-Erweiterung](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)</span><span class="sxs-lookup"><span data-stu-id="f4e49-116">Visual Studio Code [C# extension](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)</span></span>

## <a name="create-the-project"></a><span data-ttu-id="f4e49-117">Erstellen eines Projekts</span><span class="sxs-lookup"><span data-stu-id="f4e49-117">Create the project</span></span>

<span data-ttu-id="f4e49-118">Führen Sie in einer Konsole die folgenden Befehle aus:</span><span class="sxs-lookup"><span data-stu-id="f4e49-118">From a console, run the following commands:</span></span>

```console
mkdir TodoApi
cd TodoApi
dotnet new webapi
```

<span data-ttu-id="f4e49-119">Öffnen Sie den Ordner *TodoApi* in Visual Studio Code, und wählen Sie die Datei *Startup.cs* aus.</span><span class="sxs-lookup"><span data-stu-id="f4e49-119">Open the *TodoApi* folder in Visual Studio Code (VS Code) and select the *Startup.cs* file.</span></span>

- <span data-ttu-id="f4e49-120">Klicken Sie auf **Ja**, wenn die **Warnung** „Required assets to build and debug are missing from 'TodoApi'. Add them?“ („Die erforderlichen Objekte für die Erstellung und das Debugging sind in 'TodoApi' nicht vorhanden.</span><span class="sxs-lookup"><span data-stu-id="f4e49-120">Select **Yes** to the **Warn** message "Required assets to build and debug are missing from 'TodoApi'.</span></span> <span data-ttu-id="f4e49-121">Sollen Sie hinzugefügt werden?“) angezeigt wird.</span><span class="sxs-lookup"><span data-stu-id="f4e49-121">Add them?"</span></span>
- <span data-ttu-id="f4e49-122">Klicken Sie bei der **Infomeldung** „There are unresolved depedencies“ („Es bestehen ungelöste Abhängigkeiten“) auf **Wiederherstellen**.</span><span class="sxs-lookup"><span data-stu-id="f4e49-122">Select **Restore** to the **Info** message "There are unresolved dependencies".</span></span>

<!-- uid: tutorials/first-mvc-app-xplat/start-mvc uses the pic below. If you change it, make sure it's consistent -->

![VS Code mit der Warnung „Required assets to build and debug are missing from „MvcMovie“. Add them?“ („Die erforderlichen Objekte für die Erstellung und das Debugging sind in 'TodoApi' nicht vorhanden.](web-api-vsc/_static/vsc_restore.png)

<span data-ttu-id="f4e49-126">Drücken Sie **Debuggen** (F5), um das Programm zu erstellen und auszuführen.</span><span class="sxs-lookup"><span data-stu-id="f4e49-126">Press **Debug** (F5) to build and run the program.</span></span> <span data-ttu-id="f4e49-127">Navigieren Sie in einem Browser zu „http://localhost:5000/api/values“.</span><span class="sxs-lookup"><span data-stu-id="f4e49-127">In a browser navigate to http://localhost:5000/api/values .</span></span> <span data-ttu-id="f4e49-128">Folgendes wird angezeigt:</span><span class="sxs-lookup"><span data-stu-id="f4e49-128">The following is displayed:</span></span>

`["value1","value2"]`

<span data-ttu-id="f4e49-129">In der [Visual Studio Code-Hilfe](#visual-studio-code-help) finden Sie Tipps zum Arbeiten mit VS Code.</span><span class="sxs-lookup"><span data-stu-id="f4e49-129">See [Visual Studio Code help](#visual-studio-code-help) for tips on using VS Code.</span></span>

## <a name="add-support-for-entity-framework-core"></a><span data-ttu-id="f4e49-130">Hinzufügen der Unterstützung für Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="f4e49-130">Add support for Entity Framework Core</span></span>

<span data-ttu-id="f4e49-131">Bearbeiten Sie die Datei *TodoApi.csproj* so, dass der Datenbankanbieter [Entity Framework Core InMemory](https://docs.microsoft.com/ef/core/providers/in-memory/) installiert wird.</span><span class="sxs-lookup"><span data-stu-id="f4e49-131">Edit the *TodoApi.csproj* file to install the [Entity Framework Core InMemory](https://docs.microsoft.com/ef/core/providers/in-memory/) database provider.</span></span> <span data-ttu-id="f4e49-132">Dieser Datenbankanbieter ermöglicht, dass Entity Framework Core mit einer In-Memory Database verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="f4e49-132">This database provider allows Entity Framework Core to be used with an in-memory database.</span></span>

<span data-ttu-id="f4e49-133">[!code-xml[Main](web-api-vsc/sample/TodoApi/TodoApi.csproj?highlight=12)]</span><span class="sxs-lookup"><span data-stu-id="f4e49-133">[!code-xml[Main](web-api-vsc/sample/TodoApi/TodoApi.csproj?highlight=12)]</span></span>

<span data-ttu-id="f4e49-134">Führen Sie `dotnet restore` zum Herunterladen und Installieren des Datenbankanbieters „EF Core InMemory“ aus.</span><span class="sxs-lookup"><span data-stu-id="f4e49-134">Run `dotnet restore` to download and install the EF Core InMemory DB provider.</span></span> <span data-ttu-id="f4e49-135">Sie können `dotnet restore` im Terminal ausführen oder `⌘⇧P` (macOS) oder `Ctrl+Shift+P` (Linux) in VS Code und dann **.NET** eingeben.</span><span class="sxs-lookup"><span data-stu-id="f4e49-135">You can run `dotnet restore` from the terminal or enter `⌘⇧P` (macOS) or `Ctrl+Shift+P` (Linux) in VS Code and then type **.NET**.</span></span> <span data-ttu-id="f4e49-136">Wählen Sie **.NET: Pakete wiederherstellen** aus.</span><span class="sxs-lookup"><span data-stu-id="f4e49-136">Select **.NET: Restore Packages**.</span></span>

## <a name="add-a-model-class"></a><span data-ttu-id="f4e49-137">Hinzufügen einer Modellklasse</span><span class="sxs-lookup"><span data-stu-id="f4e49-137">Add a model class</span></span>

<span data-ttu-id="f4e49-138">Ein Modell ist ein Objekt, das die Daten in Ihrer Anwendung darstellt.</span><span class="sxs-lookup"><span data-stu-id="f4e49-138">A model is an object that represents the data in your application.</span></span> <span data-ttu-id="f4e49-139">In diesem Fall ist das einzige Modell ein To-Do-Element.</span><span class="sxs-lookup"><span data-stu-id="f4e49-139">In this case, the only model is a to-do item.</span></span>

<span data-ttu-id="f4e49-140">Fügen Sie einen Ordner namens *Models* hinzu.</span><span class="sxs-lookup"><span data-stu-id="f4e49-140">Add a folder named *Models*.</span></span> <span data-ttu-id="f4e49-141">Hinweis: Sie können Modellklassen überall in Ihrem Projekt unterbringen, aber gemäß Konvention wird der Ordner *Models* verwendet.</span><span class="sxs-lookup"><span data-stu-id="f4e49-141">You can put model classes anywhere in your project, but the *Models* folder is used by convention.</span></span>

<span data-ttu-id="f4e49-142">Fügen Sie eine `TodoItem`-Klasse mit dem folgenden Code hinzu:</span><span class="sxs-lookup"><span data-stu-id="f4e49-142">Add a `TodoItem` class with the following code:</span></span>

<span data-ttu-id="f4e49-143">[!code-csharp[Main](first-web-api/sample/TodoApi/Models/TodoItem.cs)]</span><span class="sxs-lookup"><span data-stu-id="f4e49-143">[!code-csharp[Main](first-web-api/sample/TodoApi/Models/TodoItem.cs)]</span></span>

<span data-ttu-id="f4e49-144">Die Datenbank generiert die `Id`, wenn ein `TodoItem` erstellt wird.</span><span class="sxs-lookup"><span data-stu-id="f4e49-144">The database generates the `Id` when a `TodoItem` is created.</span></span>

## <a name="create-the-database-context"></a><span data-ttu-id="f4e49-145">Erstellen des Datenbankkontexts</span><span class="sxs-lookup"><span data-stu-id="f4e49-145">Create the database context</span></span>

<span data-ttu-id="f4e49-146">Der *Datenbankkontext* ist die Hauptklasse, die die Entity Framework-Funktionen für ein angegebenes Datenmodell koordiniert.</span><span class="sxs-lookup"><span data-stu-id="f4e49-146">The *database context* is the main class that coordinates Entity Framework functionality for a given data model.</span></span> <span data-ttu-id="f4e49-147">Sie können diese Klasse durch Ableiten von der `Microsoft.EntityFrameworkCore.DbContext`-Klasse erstellen.</span><span class="sxs-lookup"><span data-stu-id="f4e49-147">You create this class by deriving from the `Microsoft.EntityFrameworkCore.DbContext` class.</span></span>

<span data-ttu-id="f4e49-148">Fügen Sie eine `TodoContext`-Klasse zum Ordner *Models* hinzu:</span><span class="sxs-lookup"><span data-stu-id="f4e49-148">Add a `TodoContext` class in the *Models* folder:</span></span>

<span data-ttu-id="f4e49-149">[!code-csharp[Main](first-web-api/sample/TodoApi/Models/TodoContext.cs)]</span><span class="sxs-lookup"><span data-stu-id="f4e49-149">[!code-csharp[Main](first-web-api/sample/TodoApi/Models/TodoContext.cs)]</span></span>

[!INCLUDE[Register the database context](../includes/webApi/register_dbContext.md)]

## <a name="add-a-controller"></a><span data-ttu-id="f4e49-150">Hinzufügen eines Controllers</span><span class="sxs-lookup"><span data-stu-id="f4e49-150">Add a controller</span></span>

<span data-ttu-id="f4e49-151">Erstellen Sie im Ordner *Controllers* eine Klasse namens `TodoController`.</span><span class="sxs-lookup"><span data-stu-id="f4e49-151">In the *Controllers* folder, create a class named `TodoController`.</span></span> <span data-ttu-id="f4e49-152">Fügen Sie den folgenden Code hinzu:</span><span class="sxs-lookup"><span data-stu-id="f4e49-152">Add the following code:</span></span>

[!INCLUDE[code and get todo items](../includes/webApi/getTodoItems.md)]

### <a name="launch-the-app"></a><span data-ttu-id="f4e49-153">Starten der App</span><span class="sxs-lookup"><span data-stu-id="f4e49-153">Launch the app</span></span>

<span data-ttu-id="f4e49-154">Drücken Sie in Visual Studio Code F5, um die App zu starten.</span><span class="sxs-lookup"><span data-stu-id="f4e49-154">In VS Code, press F5 to launch the app.</span></span> <span data-ttu-id="f4e49-155">Navigieren Sie zu „http://localhost:5000/api/todo“ (zum zuvor erstellten Controller `Todo`).</span><span class="sxs-lookup"><span data-stu-id="f4e49-155">Navigate to  http://localhost:5000/api/todo   (The `Todo` controller we just created).</span></span>

[!INCLUDE[last part of web API](../includes/webApi/end.md)]

## <a name="visual-studio-code-help"></a><span data-ttu-id="f4e49-156">Hilfe zu Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="f4e49-156">Visual Studio Code help</span></span>

- [<span data-ttu-id="f4e49-157">Erste Schritte</span><span class="sxs-lookup"><span data-stu-id="f4e49-157">Getting started</span></span>](https://code.visualstudio.com/docs)
- [<span data-ttu-id="f4e49-158">Debuggen</span><span class="sxs-lookup"><span data-stu-id="f4e49-158">Debugging</span></span>](https://code.visualstudio.com/docs/editor/debugging)
- [<span data-ttu-id="f4e49-159">Integriertes Terminal</span><span class="sxs-lookup"><span data-stu-id="f4e49-159">Integrated terminal</span></span>](https://code.visualstudio.com/docs/editor/integrated-terminal)
- [<span data-ttu-id="f4e49-160">Tastenkombinationen</span><span class="sxs-lookup"><span data-stu-id="f4e49-160">Keyboard shortcuts</span></span>](https://code.visualstudio.com/docs/getstarted/keybindings#_keyboard-shortcuts-reference)

  - [<span data-ttu-id="f4e49-161">Mac-Tastenkombinationen</span><span class="sxs-lookup"><span data-stu-id="f4e49-161">Mac keyboard shortcuts</span></span>](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-macos.pdf)
  - [<span data-ttu-id="f4e49-162">Linux-Tastenkombinationen</span><span class="sxs-lookup"><span data-stu-id="f4e49-162">Linux keyboard shortcuts</span></span>](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-linux.pdf)
  - [<span data-ttu-id="f4e49-163">Windows-Tastenkombinationen</span><span class="sxs-lookup"><span data-stu-id="f4e49-163">Windows keyboard shortcuts</span></span>](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-windows.pdf)

[!INCLUDE[next steps](../includes/webApi/next.md)]


