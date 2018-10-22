---
title: Erstellen einer Web-API mit ASP.NET Core und Visual Studio Code
author: rick-anderson
description: Erstellen einer Web-API unter macOS, Linux oder Windows mit ASP.NET Core MVC und Visual Studio Code
ms.author: riande
ms.custom: mvc
ms.date: 07/30/2018
uid: tutorials/web-api-vsc
ms.openlocfilehash: b8e5c8b7d3dc04513997997d903295853dd1ff46
ms.sourcegitcommit: 6e6002de467cd135a69e5518d4ba9422d693132a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/16/2018
ms.locfileid: "49348428"
---
# <a name="create-a-web-api-with-aspnet-core-and-visual-studio-code"></a><span data-ttu-id="d54d2-103">Erstellen einer Web-API mit ASP.NET Core und Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="d54d2-103">Create a Web API with ASP.NET Core and Visual Studio Code</span></span>

<span data-ttu-id="d54d2-104">Von [Rick Anderson](https://twitter.com/RickAndMSFT) und [Mike Wasson](https://github.com/mikewasson)</span><span class="sxs-lookup"><span data-stu-id="d54d2-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Mike Wasson](https://github.com/mikewasson)</span></span>

<span data-ttu-id="d54d2-105">In diesem Tutorial wird eine Web-API zum Verwalten einer Liste von Aufgabenelementen erstellt.</span><span class="sxs-lookup"><span data-stu-id="d54d2-105">In this tutorial, build a web API for managing a list of "to-do" items.</span></span> <span data-ttu-id="d54d2-106">Es wird keine Benutzeroberfläche erstellt.</span><span class="sxs-lookup"><span data-stu-id="d54d2-106">A UI isn't constructed.</span></span>

<span data-ttu-id="d54d2-107">Es gibt drei Versionen dieses Tutorials:</span><span class="sxs-lookup"><span data-stu-id="d54d2-107">There are three versions of this tutorial:</span></span>

* <span data-ttu-id="d54d2-108">macOS, Linux, Windows: Web-API mit Visual Studio Code (dieses Tutorial)</span><span class="sxs-lookup"><span data-stu-id="d54d2-108">macOS, Linux, Windows: Web API with Visual Studio Code (This tutorial)</span></span>
* <span data-ttu-id="d54d2-109">macOS: [Web-API mit Visual Studio für Mac](xref:tutorials/first-web-api-mac)</span><span class="sxs-lookup"><span data-stu-id="d54d2-109">macOS: [Web API with Visual Studio for Mac](xref:tutorials/first-web-api-mac)</span></span>
* <span data-ttu-id="d54d2-110">Windows: [Web-API mit Visual Studio für Windows](xref:tutorials/first-web-api)</span><span class="sxs-lookup"><span data-stu-id="d54d2-110">Windows: [Web API with Visual Studio for Windows](xref:tutorials/first-web-api)</span></span>

<!-- WARNING: The code AND images in this doc are used by uid: tutorials/web-api-vsc, tutorials/first-web-api-mac and tutorials/first-web-api. If you change any code/images in this tutorial, update uid: tutorials/web-api-vsc -->

[!INCLUDE[template files](../includes/webApi/intro.md)]

## <a name="prerequisites"></a><span data-ttu-id="d54d2-111">Erforderliche Komponenten</span><span class="sxs-lookup"><span data-stu-id="d54d2-111">Prerequisites</span></span>

[!INCLUDE[prerequisites](~/includes/net-core-prereqs-vscode.md)]

<span data-ttu-id="d54d2-112">In der [Visual Studio Code-Hilfe](#visual-studio-code-help) finden Sie Tipps zum Arbeiten mit VS Code.</span><span class="sxs-lookup"><span data-stu-id="d54d2-112">See [Visual Studio Code help](#visual-studio-code-help) for tips on using VS Code.</span></span>

## <a name="create-the-project"></a><span data-ttu-id="d54d2-113">Erstellen eines Projekts</span><span class="sxs-lookup"><span data-stu-id="d54d2-113">Create the project</span></span>

<span data-ttu-id="d54d2-114">Führen Sie in einer Konsole die folgenden Befehle aus:</span><span class="sxs-lookup"><span data-stu-id="d54d2-114">From a console, run the following commands:</span></span>

```console
dotnet new webapi -o TodoApi
code TodoApi
```

<span data-ttu-id="d54d2-115">Der Ordner *TodoApi* wird in Visual Studio Code (VS Code) geöffnet.</span><span class="sxs-lookup"><span data-stu-id="d54d2-115">The *TodoApi* folder opens in Visual Studio Code (VS Code).</span></span> <span data-ttu-id="d54d2-116">Wählen Sie die Datei *Startup.cs* aus.</span><span class="sxs-lookup"><span data-stu-id="d54d2-116">Select the *Startup.cs* file.</span></span>

* <span data-ttu-id="d54d2-117">Klicken Sie auf **Ja**, wenn die **Warnung** „Required assets to build and debug are missing from 'TodoApi'. Add them?“ („Die erforderlichen Objekte für die Erstellung und das Debugging sind in 'TodoApi' nicht vorhanden.</span><span class="sxs-lookup"><span data-stu-id="d54d2-117">Select **Yes** to the **Warn** message "Required assets to build and debug are missing from 'TodoApi'.</span></span> <span data-ttu-id="d54d2-118">Sollen Sie hinzugefügt werden?“) angezeigt wird.</span><span class="sxs-lookup"><span data-stu-id="d54d2-118">Add them?"</span></span>
* <span data-ttu-id="d54d2-119">Klicken Sie bei der **Infomeldung** „There are unresolved depedencies“ („Es bestehen ungelöste Abhängigkeiten“) auf **Wiederherstellen**.</span><span class="sxs-lookup"><span data-stu-id="d54d2-119">Select **Restore** to the **Info** message "There are unresolved dependencies".</span></span>

<!-- uid: tutorials/first-mvc-app-xplat/start-mvc uses the pic below. If you change it, make sure it's consistent -->

![VS Code mit der Warnung „Required assets to build and debug are missing from „MvcMovie“. Add them?“ („Die erforderlichen Objekte für die Erstellung und das Debugging sind in 'TodoApi' nicht vorhanden.](web-api-vsc/_static/vsc_restore.png)

<span data-ttu-id="d54d2-123">Drücken Sie **Debuggen** (F5), um das Programm zu erstellen und auszuführen.</span><span class="sxs-lookup"><span data-stu-id="d54d2-123">Press **Debug** (F5) to build and run the program.</span></span> <span data-ttu-id="d54d2-124">Navigieren Sie in einem Browser zu http://localhost:5000/api/values.</span><span class="sxs-lookup"><span data-stu-id="d54d2-124">In a browser, navigate to http://localhost:5000/api/values.</span></span> <span data-ttu-id="d54d2-125">Die folgende Ausgabe wird angezeigt:</span><span class="sxs-lookup"><span data-stu-id="d54d2-125">The following output is displayed:</span></span>

```json
["value1","value2"]
```



## <a name="add-support-for-entity-framework-core"></a><span data-ttu-id="d54d2-126">Hinzufügen der Unterstützung für Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="d54d2-126">Add support for Entity Framework Core</span></span>

:::moniker range=">= aspnetcore-2.1"

<span data-ttu-id="d54d2-127">Durch das Erstellen eines neuen Projekts in ASP.NET Core 2.1 oder höher wird der Paketverweis [Microsoft.AspNetCore.App](https://www.nuget.org/packages/Microsoft.AspNetCore.App) der Datei *TodoApi.csproj* hinzugefügt:</span><span class="sxs-lookup"><span data-stu-id="d54d2-127">Creating a new project in ASP.NET Core 2.1 or later adds the [Microsoft.AspNetCore.App](https://www.nuget.org/packages/Microsoft.AspNetCore.App) package reference to the *TodoApi.csproj* file:</span></span>

[!code-xml[](first-web-api/samples/2.1/TodoApi/TodoApi.csproj?name=snippet_Metapackage&highlight=2)]

:::moniker-end

:::moniker range="<= aspnetcore-2.0"

<span data-ttu-id="d54d2-128">Durch das Erstellen eines neuen Projekts in ASP.NET Core 2.0 wird der Paketverweis [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All) zur Datei *TodoApi.csproj* hinzugefügt:</span><span class="sxs-lookup"><span data-stu-id="d54d2-128">Creating a new project in ASP.NET Core 2.0 adds the [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All) package reference to the *TodoApi.csproj* file:</span></span>

[!code-xml[](first-web-api/samples/2.0/TodoApi/TodoApi.csproj?name=snippet_Metapackage&highlight=2)]

:::moniker-end

<span data-ttu-id="d54d2-129">Der Datenbankanbieter [Entity Framework Core InMemory](/ef/core/providers/in-memory/) muss nicht separat installiert werden.</span><span class="sxs-lookup"><span data-stu-id="d54d2-129">There's no need to install the [Entity Framework Core InMemory](/ef/core/providers/in-memory/) database provider separately.</span></span> <span data-ttu-id="d54d2-130">Dieser Datenbankanbieter ermöglicht, dass Entity Framework Core mit einer In-Memory Database verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="d54d2-130">This database provider allows Entity Framework Core to be used with an in-memory database.</span></span>

## <a name="add-a-model-class"></a><span data-ttu-id="d54d2-131">Hinzufügen einer Modellklasse</span><span class="sxs-lookup"><span data-stu-id="d54d2-131">Add a model class</span></span>

<span data-ttu-id="d54d2-132">Ein Modell ist ein Objekt, das die Daten in Ihrer App darstellt.</span><span class="sxs-lookup"><span data-stu-id="d54d2-132">A model is an object representing the data in your app.</span></span> <span data-ttu-id="d54d2-133">In diesem Fall ist das einzige Modell ein To-do-Element.</span><span class="sxs-lookup"><span data-stu-id="d54d2-133">In this case, the only model is a to-do item.</span></span>

<span data-ttu-id="d54d2-134">Fügen Sie einen Ordner namens *Models* hinzu.</span><span class="sxs-lookup"><span data-stu-id="d54d2-134">Add a folder named *Models*.</span></span> <span data-ttu-id="d54d2-135">Hinweis: Sie können Modellklassen überall in Ihrem Projekt unterbringen, aber gemäß Konvention wird der Ordner *Models* verwendet.</span><span class="sxs-lookup"><span data-stu-id="d54d2-135">You can put model classes anywhere in your project, but the *Models* folder is used by convention.</span></span>

<span data-ttu-id="d54d2-136">Fügen Sie eine `TodoItem`-Klasse mit dem folgenden Code hinzu:</span><span class="sxs-lookup"><span data-stu-id="d54d2-136">Add a `TodoItem` class with the following code:</span></span>

[!code-csharp[](first-web-api/samples/2.0/TodoApi/Models/TodoItem.cs)]

<span data-ttu-id="d54d2-137">Die Datenbank generiert die `Id`, wenn ein `TodoItem` erstellt wird.</span><span class="sxs-lookup"><span data-stu-id="d54d2-137">The database generates the `Id` when a `TodoItem` is created.</span></span>

## <a name="create-the-database-context"></a><span data-ttu-id="d54d2-138">Erstellen des Datenbankkontexts</span><span class="sxs-lookup"><span data-stu-id="d54d2-138">Create the database context</span></span>

<span data-ttu-id="d54d2-139">Der *Datenbankkontext* ist die Hauptklasse, die die Entity Framework-Funktionen für ein angegebenes Datenmodell koordiniert.</span><span class="sxs-lookup"><span data-stu-id="d54d2-139">The *database context* is the main class that coordinates Entity Framework functionality for a given data model.</span></span> <span data-ttu-id="d54d2-140">Sie können diese Klasse durch Ableiten von der `Microsoft.EntityFrameworkCore.DbContext`-Klasse erstellen.</span><span class="sxs-lookup"><span data-stu-id="d54d2-140">You create this class by deriving from the `Microsoft.EntityFrameworkCore.DbContext` class.</span></span>

<span data-ttu-id="d54d2-141">Fügen Sie eine `TodoContext`-Klasse zum Ordner *Models* hinzu:</span><span class="sxs-lookup"><span data-stu-id="d54d2-141">Add a `TodoContext` class in the *Models* folder:</span></span>

[!code-csharp[](first-web-api/samples/2.0/TodoApi/Models/TodoContext.cs)]

[!INCLUDE[Register the database context](../includes/webApi/register_dbContext.md)]

## <a name="add-a-controller"></a><span data-ttu-id="d54d2-142">Hinzufügen eines Controllers</span><span class="sxs-lookup"><span data-stu-id="d54d2-142">Add a controller</span></span>

<span data-ttu-id="d54d2-143">Erstellen Sie im Ordner *Controllers* eine Klasse namens `TodoController`.</span><span class="sxs-lookup"><span data-stu-id="d54d2-143">In the *Controllers* folder, create a class named `TodoController`.</span></span> <span data-ttu-id="d54d2-144">Ersetzen Sie den Inhalt durch folgenden Code:</span><span class="sxs-lookup"><span data-stu-id="d54d2-144">Replace its contents with the following code:</span></span>

[!INCLUDE[code and get todo items](../includes/webApi/getTodoItems.md)]

### <a name="launch-the-app"></a><span data-ttu-id="d54d2-145">Starten der App</span><span class="sxs-lookup"><span data-stu-id="d54d2-145">Launch the app</span></span>

<span data-ttu-id="d54d2-146">Drücken Sie in Visual Studio Code F5, um die App zu starten.</span><span class="sxs-lookup"><span data-stu-id="d54d2-146">In VS Code, press F5 to launch the app.</span></span> <span data-ttu-id="d54d2-147">Navigieren Sie zu http://localhost:5000/api/todo (der `Todo`-Controller, den wir eben erstellt haben).</span><span class="sxs-lookup"><span data-stu-id="d54d2-147">Navigate to http://localhost:5000/api/todo (the `Todo` controller we created).</span></span>

[!INCLUDE[jQuery](../includes/webApi/add-jquery.md)]

[!INCLUDE[last part of web API](../includes/webApi/end.md)]

## <a name="visual-studio-code-help"></a><span data-ttu-id="d54d2-148">Hilfe zu Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="d54d2-148">Visual Studio Code help</span></span>

* [<span data-ttu-id="d54d2-149">Erste Schritte</span><span class="sxs-lookup"><span data-stu-id="d54d2-149">Getting started</span></span>](https://code.visualstudio.com/docs)
* [<span data-ttu-id="d54d2-150">Debuggen</span><span class="sxs-lookup"><span data-stu-id="d54d2-150">Debugging</span></span>](https://code.visualstudio.com/docs/editor/debugging)
* [<span data-ttu-id="d54d2-151">Integriertes Terminal</span><span class="sxs-lookup"><span data-stu-id="d54d2-151">Integrated terminal</span></span>](https://code.visualstudio.com/docs/editor/integrated-terminal)
* [<span data-ttu-id="d54d2-152">Tastenkombinationen</span><span class="sxs-lookup"><span data-stu-id="d54d2-152">Keyboard shortcuts</span></span>](https://code.visualstudio.com/docs/getstarted/keybindings#_keyboard-shortcuts-reference)

  * [<span data-ttu-id="d54d2-153">macOS-Tastenkombinationen</span><span class="sxs-lookup"><span data-stu-id="d54d2-153">macOS keyboard shortcuts</span></span>](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-macos.pdf)
  * [<span data-ttu-id="d54d2-154">Linux-Tastenkombinationen</span><span class="sxs-lookup"><span data-stu-id="d54d2-154">Linux keyboard shortcuts</span></span>](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-linux.pdf)
  * [<span data-ttu-id="d54d2-155">Windows-Tastenkombinationen</span><span class="sxs-lookup"><span data-stu-id="d54d2-155">Windows keyboard shortcuts</span></span>](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-windows.pdf)

[!INCLUDE[next steps](../includes/webApi/next.md)]
