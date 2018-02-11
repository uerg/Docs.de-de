---
title: Erstellen einer Web-API mit ASP.NET Core und Visual Studio Code
author: rick-anderson
description: Erstellen einer Web-API unter macOS, Linux oder Windows mit ASP.NET Core MVC und Visual Studio Code
manager: wpickett
ms.author: riande
ms.date: 09/22/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/web-api-vsc
ms.openlocfilehash: 44566c4014400aa2ca3d512eeaa226637b5f0b97
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2018
---
# <a name="create-a-web-api-with-aspnet-core-mvc-and-visual-studio-code-on-linux-macos-and-windows"></a><span data-ttu-id="98e12-103">Erstellen einer Web-API mit ASP.NET Core MVC und Visual Studio Code unter macOS Linux und Windows</span><span class="sxs-lookup"><span data-stu-id="98e12-103">Create a Web API with ASP.NET Core MVC and Visual Studio Code on Linux, macOS, and Windows</span></span>

<span data-ttu-id="98e12-104">Von [Rick Anderson](https://twitter.com/RickAndMSFT) und [Mike Wasson](https://github.com/mikewasson)</span><span class="sxs-lookup"><span data-stu-id="98e12-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Mike Wasson](https://github.com/mikewasson)</span></span>

<span data-ttu-id="98e12-105">In diesem Tutorial wird eine Web-API zum Verwalten einer Liste von Aufgabenelementen erstellt.</span><span class="sxs-lookup"><span data-stu-id="98e12-105">In this tutorial, build a web API for managing a list of "to-do" items.</span></span> <span data-ttu-id="98e12-106">Es wird keine Benutzeroberfläche erstellt.</span><span class="sxs-lookup"><span data-stu-id="98e12-106">A UI isn't constructed.</span></span>

<span data-ttu-id="98e12-107">Es gibt drei Versionen dieses Tutorials:</span><span class="sxs-lookup"><span data-stu-id="98e12-107">There are 3 versions of this tutorial:</span></span>

* <span data-ttu-id="98e12-108">macOS, Linux, Windows: Web-API mit Visual Studio Code (dieses Tutorial)</span><span class="sxs-lookup"><span data-stu-id="98e12-108">macOS, Linux, Windows: Web API with Visual Studio Code (This tutorial)</span></span>
* <span data-ttu-id="98e12-109">macOS: [Web-API mit Visual Studio für Mac](xref:tutorials/first-web-api-mac)</span><span class="sxs-lookup"><span data-stu-id="98e12-109">macOS: [Web API with Visual Studio for Mac](xref:tutorials/first-web-api-mac)</span></span>
* <span data-ttu-id="98e12-110">Windows: [Web-API mit Visual Studio für Windows](xref:tutorials/first-web-api)</span><span class="sxs-lookup"><span data-stu-id="98e12-110">Windows: [Web API with Visual Studio for Windows](xref:tutorials/first-web-api)</span></span>

<!-- WARNING: The code AND images in this doc are used by uid: tutorials/web-api-vsc, tutorials/first-web-api-mac and tutorials/first-web-api. If you change any code/images in this tutorial, update uid: tutorials/web-api-vsc -->

[!INCLUDE[template files](../includes/webApi/intro.md)]

## <a name="set-up-your-development-environment"></a><span data-ttu-id="98e12-111">Einrichten der Entwicklungsumgebung</span><span class="sxs-lookup"><span data-stu-id="98e12-111">Set up your development environment</span></span>

<span data-ttu-id="98e12-112">Sie müssen Folgendes herunterladen und installieren:</span><span class="sxs-lookup"><span data-stu-id="98e12-112">Download and install:</span></span>
- <span data-ttu-id="98e12-113">mindestens [.NET Core 2.0.0 SDK](https://www.microsoft.com/net/core)</span><span class="sxs-lookup"><span data-stu-id="98e12-113">[.NET Core 2.0.0 SDK](https://www.microsoft.com/net/core) or later.</span></span>
- [<span data-ttu-id="98e12-114">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="98e12-114">Visual Studio Code</span></span>](https://code.visualstudio.com)
- <span data-ttu-id="98e12-115">Visual Studio Code [C#-Erweiterung](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)</span><span class="sxs-lookup"><span data-stu-id="98e12-115">Visual Studio Code [C# extension](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)</span></span>

## <a name="create-the-project"></a><span data-ttu-id="98e12-116">Erstellen eines Projekts</span><span class="sxs-lookup"><span data-stu-id="98e12-116">Create the project</span></span>

<span data-ttu-id="98e12-117">Führen Sie in einer Konsole die folgenden Befehle aus:</span><span class="sxs-lookup"><span data-stu-id="98e12-117">From a console, run the following commands:</span></span>

```console
mkdir TodoApi
cd TodoApi
dotnet new webapi
```

<span data-ttu-id="98e12-118">Öffnen Sie den Ordner *TodoApi* in Visual Studio Code, und wählen Sie die Datei *Startup.cs* aus.</span><span class="sxs-lookup"><span data-stu-id="98e12-118">Open the *TodoApi* folder in Visual Studio Code (VS Code) and select the *Startup.cs* file.</span></span>

- <span data-ttu-id="98e12-119">Klicken Sie auf **Ja**, wenn die **Warnung** „Required assets to build and debug are missing from 'TodoApi'. Add them?“ („Die erforderlichen Objekte für die Erstellung und das Debugging sind in 'TodoApi' nicht vorhanden.</span><span class="sxs-lookup"><span data-stu-id="98e12-119">Select **Yes** to the **Warn** message "Required assets to build and debug are missing from 'TodoApi'.</span></span> <span data-ttu-id="98e12-120">Sollen Sie hinzugefügt werden?“) angezeigt wird.</span><span class="sxs-lookup"><span data-stu-id="98e12-120">Add them?"</span></span>
- <span data-ttu-id="98e12-121">Klicken Sie bei der **Infomeldung** „There are unresolved depedencies“ („Es bestehen ungelöste Abhängigkeiten“) auf **Wiederherstellen**.</span><span class="sxs-lookup"><span data-stu-id="98e12-121">Select **Restore** to the **Info** message "There are unresolved dependencies".</span></span>

<!-- uid: tutorials/first-mvc-app-xplat/start-mvc uses the pic below. If you change it, make sure it's consistent -->

![VS Code mit der Warnung „Required assets to build and debug are missing from „MvcMovie“. Add them?“ („Die erforderlichen Objekte für die Erstellung und das Debugging sind in 'TodoApi' nicht vorhanden.](web-api-vsc/_static/vsc_restore.png)

<span data-ttu-id="98e12-125">Drücken Sie **Debuggen** (F5), um das Programm zu erstellen und auszuführen.</span><span class="sxs-lookup"><span data-stu-id="98e12-125">Press **Debug** (F5) to build and run the program.</span></span> <span data-ttu-id="98e12-126">Navigieren Sie in einem Browser zu „http://localhost:5000/api/values“.</span><span class="sxs-lookup"><span data-stu-id="98e12-126">In a browser navigate to http://localhost:5000/api/values .</span></span> <span data-ttu-id="98e12-127">Folgendes wird angezeigt:</span><span class="sxs-lookup"><span data-stu-id="98e12-127">The following is displayed:</span></span>

`["value1","value2"]`

<span data-ttu-id="98e12-128">In der [Visual Studio Code-Hilfe](#visual-studio-code-help) finden Sie Tipps zum Arbeiten mit VS Code.</span><span class="sxs-lookup"><span data-stu-id="98e12-128">See [Visual Studio Code help](#visual-studio-code-help) for tips on using VS Code.</span></span>

## <a name="add-support-for-entity-framework-core"></a><span data-ttu-id="98e12-129">Hinzufügen der Unterstützung für Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="98e12-129">Add support for Entity Framework Core</span></span>

<span data-ttu-id="98e12-130">Durch das Erstellen eines neuen Projekts in .NET Core 2.0 wird der Anbieter „Microsoft.AspNetCore.All“ zur Datei *TodoApi.csproj* hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="98e12-130">Creating a new project in .NET Core 2.0 adds the 'Microsoft.AspNetCore.All' provider in the *TodoApi.csproj* file.</span></span> <span data-ttu-id="98e12-131">Der Datenbankanbieter [Entity Framework Core InMemory](https://docs.microsoft.com/ef/core/providers/in-memory/) muss nicht separat installiert werden.</span><span class="sxs-lookup"><span data-stu-id="98e12-131">There's no need to install the [Entity Framework Core InMemory](https://docs.microsoft.com/ef/core/providers/in-memory/) database provider separately.</span></span> <span data-ttu-id="98e12-132">Dieser Datenbankanbieter ermöglicht, dass Entity Framework Core mit einer In-Memory Database verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="98e12-132">This database provider allows Entity Framework Core to be used with an in-memory database.</span></span>

[!code-xml[Main](web-api-vsc/sample/TodoApi/TodoApi.csproj?highlight=12)]

## <a name="add-a-model-class"></a><span data-ttu-id="98e12-133">Hinzufügen einer Modellklasse</span><span class="sxs-lookup"><span data-stu-id="98e12-133">Add a model class</span></span>

<span data-ttu-id="98e12-134">Ein Modell ist ein Objekt, das die Daten in Ihrer Anwendung darstellt.</span><span class="sxs-lookup"><span data-stu-id="98e12-134">A model is an object that represents the data in your application.</span></span> <span data-ttu-id="98e12-135">In diesem Fall ist das einzige Modell ein To-do-Element.</span><span class="sxs-lookup"><span data-stu-id="98e12-135">In this case, the only model is a to-do item.</span></span>

<span data-ttu-id="98e12-136">Fügen Sie einen Ordner namens *Models* hinzu.</span><span class="sxs-lookup"><span data-stu-id="98e12-136">Add a folder named *Models*.</span></span> <span data-ttu-id="98e12-137">Hinweis: Sie können Modellklassen überall in Ihrem Projekt unterbringen, aber gemäß Konvention wird der Ordner *Models* verwendet.</span><span class="sxs-lookup"><span data-stu-id="98e12-137">You can put model classes anywhere in your project, but the *Models* folder is used by convention.</span></span>

<span data-ttu-id="98e12-138">Fügen Sie eine `TodoItem`-Klasse mit dem folgenden Code hinzu:</span><span class="sxs-lookup"><span data-stu-id="98e12-138">Add a `TodoItem` class with the following code:</span></span>

[!code-csharp[Main](first-web-api/sample/TodoApi/Models/TodoItem.cs)]

<span data-ttu-id="98e12-139">Die Datenbank generiert die `Id`, wenn ein `TodoItem` erstellt wird.</span><span class="sxs-lookup"><span data-stu-id="98e12-139">The database generates the `Id` when a `TodoItem` is created.</span></span>

## <a name="create-the-database-context"></a><span data-ttu-id="98e12-140">Erstellen des Datenbankkontexts</span><span class="sxs-lookup"><span data-stu-id="98e12-140">Create the database context</span></span>

<span data-ttu-id="98e12-141">Der *Datenbankkontext* ist die Hauptklasse, die die Entity Framework-Funktionen für ein angegebenes Datenmodell koordiniert.</span><span class="sxs-lookup"><span data-stu-id="98e12-141">The *database context* is the main class that coordinates Entity Framework functionality for a given data model.</span></span> <span data-ttu-id="98e12-142">Sie können diese Klasse durch Ableiten von der `Microsoft.EntityFrameworkCore.DbContext`-Klasse erstellen.</span><span class="sxs-lookup"><span data-stu-id="98e12-142">You create this class by deriving from the `Microsoft.EntityFrameworkCore.DbContext` class.</span></span>

<span data-ttu-id="98e12-143">Fügen Sie eine `TodoContext`-Klasse zum Ordner *Models* hinzu:</span><span class="sxs-lookup"><span data-stu-id="98e12-143">Add a `TodoContext` class in the *Models* folder:</span></span>

[!code-csharp[Main](first-web-api/sample/TodoApi/Models/TodoContext.cs)]

[!INCLUDE[Register the database context](../includes/webApi/register_dbContext.md)]

## <a name="add-a-controller"></a><span data-ttu-id="98e12-144">Hinzufügen eines Controllers</span><span class="sxs-lookup"><span data-stu-id="98e12-144">Add a controller</span></span>

<span data-ttu-id="98e12-145">Erstellen Sie im Ordner *Controllers* eine Klasse namens `TodoController`.</span><span class="sxs-lookup"><span data-stu-id="98e12-145">In the *Controllers* folder, create a class named `TodoController`.</span></span> <span data-ttu-id="98e12-146">Fügen Sie den folgenden Code hinzu:</span><span class="sxs-lookup"><span data-stu-id="98e12-146">Add the following code:</span></span>

[!INCLUDE[code and get todo items](../includes/webApi/getTodoItems.md)]

### <a name="launch-the-app"></a><span data-ttu-id="98e12-147">Starten der App</span><span class="sxs-lookup"><span data-stu-id="98e12-147">Launch the app</span></span>

<span data-ttu-id="98e12-148">Drücken Sie in Visual Studio Code F5, um die App zu starten.</span><span class="sxs-lookup"><span data-stu-id="98e12-148">In VS Code, press F5 to launch the app.</span></span> <span data-ttu-id="98e12-149">Navigieren Sie zu „http://localhost:5000/api/todo“ (zum zuvor erstellten Controller `Todo`).</span><span class="sxs-lookup"><span data-stu-id="98e12-149">Navigate to  http://localhost:5000/api/todo   (The `Todo` controller we just created).</span></span>

[!INCLUDE[last part of web API](../includes/webApi/end.md)]

## <a name="visual-studio-code-help"></a><span data-ttu-id="98e12-150">Hilfe zu Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="98e12-150">Visual Studio Code help</span></span>

- [<span data-ttu-id="98e12-151">Erste Schritte</span><span class="sxs-lookup"><span data-stu-id="98e12-151">Getting started</span></span>](https://code.visualstudio.com/docs)
- [<span data-ttu-id="98e12-152">Debuggen</span><span class="sxs-lookup"><span data-stu-id="98e12-152">Debugging</span></span>](https://code.visualstudio.com/docs/editor/debugging)
- [<span data-ttu-id="98e12-153">Integriertes Terminal</span><span class="sxs-lookup"><span data-stu-id="98e12-153">Integrated terminal</span></span>](https://code.visualstudio.com/docs/editor/integrated-terminal)
- [<span data-ttu-id="98e12-154">Tastenkombinationen</span><span class="sxs-lookup"><span data-stu-id="98e12-154">Keyboard shortcuts</span></span>](https://code.visualstudio.com/docs/getstarted/keybindings#_keyboard-shortcuts-reference)

  - [<span data-ttu-id="98e12-155">Mac-Tastenkombinationen</span><span class="sxs-lookup"><span data-stu-id="98e12-155">Mac keyboard shortcuts</span></span>](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-macos.pdf)
  - [<span data-ttu-id="98e12-156">Linux-Tastenkombinationen</span><span class="sxs-lookup"><span data-stu-id="98e12-156">Linux keyboard shortcuts</span></span>](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-linux.pdf)
  - [<span data-ttu-id="98e12-157">Windows-Tastenkombinationen</span><span class="sxs-lookup"><span data-stu-id="98e12-157">Windows keyboard shortcuts</span></span>](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-windows.pdf)

[!INCLUDE[next steps](../includes/webApi/next.md)]


