---
title: "Erstellen einer Web-API mit ASP.NET Core und Visual Studio für Windows"
author: rick-anderson
description: "Erstellen einer Web-API mit ASP.NET Core MVC und Visual Studio für Windows"
ms.author: riande
manager: wpickett
ms.date: 08/15/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/first-web-api
ms.openlocfilehash: 234dbf73f9751ad4f995d6e7471d94f060fb15bf
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/19/2018
---
#<a name="create-a-web-api-with-aspnet-core-and-visual-studio-for-windows"></a><span data-ttu-id="683f5-103">Erstellen einer Web-API mit ASP.NET Core und Visual Studio für Windows</span><span class="sxs-lookup"><span data-stu-id="683f5-103">Create a web API with ASP.NET Core and Visual Studio for Windows</span></span>

<span data-ttu-id="683f5-104">Von [Rick Anderson](https://twitter.com/RickAndMSFT) und [Mike Wasson](https://github.com/mikewasson)</span><span class="sxs-lookup"><span data-stu-id="683f5-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Mike Wasson](https://github.com/mikewasson)</span></span>

<span data-ttu-id="683f5-105">In diesem Tutorial wird eine Web-API zum Verwalten einer Liste von Aufgabenelementen erstellt.</span><span class="sxs-lookup"><span data-stu-id="683f5-105">This tutorial builds a web API for managing a list of "to-do" items.</span></span> <span data-ttu-id="683f5-106">Eine Benutzeroberfläche (UI) wird nicht erstellt.</span><span class="sxs-lookup"><span data-stu-id="683f5-106">A user interface (UI) is not created.</span></span>

<span data-ttu-id="683f5-107">Es gibt drei Versionen dieses Tutorials:</span><span class="sxs-lookup"><span data-stu-id="683f5-107">There are 3 versions of this tutorial:</span></span>

* <span data-ttu-id="683f5-108">Windows: Web-API mit Visual Studio für Windows (dieses Tutorial)</span><span class="sxs-lookup"><span data-stu-id="683f5-108">Windows: Web API with Visual Studio for Windows (This tutorial)</span></span>
* <span data-ttu-id="683f5-109">macOS: [Web-API mit Visual Studio für Mac](xref:tutorials/first-web-api-mac)</span><span class="sxs-lookup"><span data-stu-id="683f5-109">macOS: [Web API with Visual Studio for Mac](xref:tutorials/first-web-api-mac)</span></span>
* <span data-ttu-id="683f5-110">macOS, Linux, Windows: [Web-API mit Visual Studio Code](xref:tutorials/web-api-vsc)</span><span class="sxs-lookup"><span data-stu-id="683f5-110">macOS, Linux, Windows: [Web API with Visual Studio Code](xref:tutorials/web-api-vsc)</span></span>

<!-- WARNING: The code AND images in this doc are used by uid: tutorials/web-api-vsc, tutorials/first-web-api-mac and tutorials/first-web-api. If you change any code/images in this tutorial, update uid: tutorials/web-api-vsc -->

[!INCLUDE[intro to web API](../includes/webApi/intro.md)]

## <a name="prerequisites"></a><span data-ttu-id="683f5-111">Erforderliche Komponenten</span><span class="sxs-lookup"><span data-stu-id="683f5-111">Prerequisites</span></span>

[!INCLUDE[install 2.0](../includes/install2.0.md)]

<span data-ttu-id="683f5-112">In [dieser PDF](https://github.com/aspnet/Docs/blob/master/aspnetcore/tutorials/first-web-api/_static/_webAPI.pdf) finden Sie die Version ASP.NET Core 1.1.</span><span class="sxs-lookup"><span data-stu-id="683f5-112">See [this PDF](https://github.com/aspnet/Docs/blob/master/aspnetcore/tutorials/first-web-api/_static/_webAPI.pdf) for the ASP.NET Core 1.1 version.</span></span>

## <a name="create-the-project"></a><span data-ttu-id="683f5-113">Erstellen eines Projekts</span><span class="sxs-lookup"><span data-stu-id="683f5-113">Create the project</span></span>

<span data-ttu-id="683f5-114">Klicken Sie in Visual Studio auf das Menü **Datei** > **Neu** > **Projekt**.</span><span class="sxs-lookup"><span data-stu-id="683f5-114">From Visual Studio, select **File** menu, > **New** > **Project**.</span></span>

<span data-ttu-id="683f5-115">Wählen Sie die Projektvorlage **ASP.NET Core Web Application (.NET Core)** aus.</span><span class="sxs-lookup"><span data-stu-id="683f5-115">Select the **ASP.NET Core Web Application (.NET Core)** project template.</span></span> <span data-ttu-id="683f5-116">Geben Sie dem Projekt den Namen `TodoApi`, und klicken Sie auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="683f5-116">Name the project `TodoApi` and select **OK**.</span></span>

![Dialogfeld "Neues Projekt"](first-web-api/_static/new-project.png)

<span data-ttu-id="683f5-118">Wählen Sie im Dialogfeld **ASP.NET Core-Webanwendung – TodoAPI** die Vorlage **Web-API** aus.</span><span class="sxs-lookup"><span data-stu-id="683f5-118">In the **New ASP.NET Core Web Application - TodoApi** dialog, select the **Web API** template.</span></span> <span data-ttu-id="683f5-119">Klicken Sie auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="683f5-119">Select **OK**.</span></span> <span data-ttu-id="683f5-120">Wählen Sie **nicht** **Enable Docker Support** (Docker-Unterstützung aktivieren) aus.</span><span class="sxs-lookup"><span data-stu-id="683f5-120">Do **not** select **Enable Docker Support**.</span></span>

![Dialogfeld Neue ASP.NET-Webanwendung mit ausgewählter Web-API-Projektvorlage aus ASP.NET Core-Vorlagen](first-web-api/_static/web-api-project.png)

### <a name="launch-the-app"></a><span data-ttu-id="683f5-122">Starten der App</span><span class="sxs-lookup"><span data-stu-id="683f5-122">Launch the app</span></span>

<span data-ttu-id="683f5-123">Drücken Sie in Visual Studio STRG+F5 zum Starten der App.</span><span class="sxs-lookup"><span data-stu-id="683f5-123">In Visual Studio, press CTRL+F5 to launch the app.</span></span> <span data-ttu-id="683f5-124">Visual Studio startet einen Browser und navigiert zu `http://localhost:port/api/values`, wobei *port* eine zufällig ausgewählte Portnummer ist.</span><span class="sxs-lookup"><span data-stu-id="683f5-124">Visual Studio launches a browser and navigates to `http://localhost:port/api/values`, where *port* is a randomly chosen port number.</span></span> <span data-ttu-id="683f5-125">Chrome, Edge und Firefox zeigen die folgende Ausgabe an:</span><span class="sxs-lookup"><span data-stu-id="683f5-125">Chrome, Microsoft Edge, and Firefox display the following output:</span></span>

```
["value1","value2"]
```

### <a name="add-a-model-class"></a><span data-ttu-id="683f5-126">Hinzufügen einer Modellklasse</span><span class="sxs-lookup"><span data-stu-id="683f5-126">Add a model class</span></span>

<span data-ttu-id="683f5-127">Ein Modell ist ein Objekt, das die Daten in der App darstellt.</span><span class="sxs-lookup"><span data-stu-id="683f5-127">A model is an object that represents the data in the app.</span></span> <span data-ttu-id="683f5-128">In diesem Fall ist das einzige Modell ein To-do-Element.</span><span class="sxs-lookup"><span data-stu-id="683f5-128">In this case, the only model is a to-do item.</span></span>

<span data-ttu-id="683f5-129">Fügen Sie einen Ordner namens „Modelle“ hinzu.</span><span class="sxs-lookup"><span data-stu-id="683f5-129">Add a folder named "Models".</span></span> <span data-ttu-id="683f5-130">Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf den Projektnamen.</span><span class="sxs-lookup"><span data-stu-id="683f5-130">In Solution Explorer, right-click the project.</span></span> <span data-ttu-id="683f5-131">Klicken Sie auf **Hinzufügen** > **Neuer Ordner**.</span><span class="sxs-lookup"><span data-stu-id="683f5-131">Select **Add** > **New Folder**.</span></span> <span data-ttu-id="683f5-132">Geben Sie dem Ordner den Namen *Modelle*.</span><span class="sxs-lookup"><span data-stu-id="683f5-132">Name the folder *Models*.</span></span>

<span data-ttu-id="683f5-133">Hinweis: Die Modellklassen können überall im Projekt gespeichert werden.</span><span class="sxs-lookup"><span data-stu-id="683f5-133">Note: The model classes go anywhere in the project.</span></span> <span data-ttu-id="683f5-134">Der Ordner *Modelle* wird gemäß Konvention für Modellklassen verwendet.</span><span class="sxs-lookup"><span data-stu-id="683f5-134">The *Models* folder is used by convention for model classes.</span></span>

<span data-ttu-id="683f5-135">Fügen Sie eine `TodoItem`-Klasse hinzu.</span><span class="sxs-lookup"><span data-stu-id="683f5-135">Add a `TodoItem` class.</span></span> <span data-ttu-id="683f5-136">Klicken Sie mit der rechten Maustaste auf den Ordner *Modelle*, und wählen Sie **Hinzufügen** > **Klasse** aus.</span><span class="sxs-lookup"><span data-stu-id="683f5-136">Right-click the *Models* folder and select **Add** > **Class**.</span></span> <span data-ttu-id="683f5-137">Benennen Sie die Klasse `TodoItem`, und klicken Sie auf **Hinzufügen**.</span><span class="sxs-lookup"><span data-stu-id="683f5-137">Name the class `TodoItem` and select **Add**.</span></span>

<span data-ttu-id="683f5-138">Aktualisieren Sie die `TodoItem`-Klasse mit dem folgenden Code:</span><span class="sxs-lookup"><span data-stu-id="683f5-138">Update the `TodoItem` class with the following code:</span></span>

[!code-csharp[Main](first-web-api/sample/TodoApi/Models/TodoItem.cs)]

<span data-ttu-id="683f5-139">Die Datenbank generiert die `Id`, wenn ein `TodoItem` erstellt wird.</span><span class="sxs-lookup"><span data-stu-id="683f5-139">The database generates the `Id` when a `TodoItem` is created.</span></span>

### <a name="create-the-database-context"></a><span data-ttu-id="683f5-140">Erstellen des Datenbankkontexts</span><span class="sxs-lookup"><span data-stu-id="683f5-140">Create the database context</span></span>

<span data-ttu-id="683f5-141">Der *Datenbankkontext* ist die Hauptklasse, die die Entity Framework-Funktionen für ein angegebenes Datenmodell koordiniert.</span><span class="sxs-lookup"><span data-stu-id="683f5-141">The *database context* is the main class that coordinates Entity Framework functionality for a given data model.</span></span> <span data-ttu-id="683f5-142">Diese Klasse wird durch Ableiten von der `Microsoft.EntityFrameworkCore.DbContext`-Klasse erstellt.</span><span class="sxs-lookup"><span data-stu-id="683f5-142">This class is created by deriving from the `Microsoft.EntityFrameworkCore.DbContext` class.</span></span>

<span data-ttu-id="683f5-143">Fügen Sie eine `TodoContext`-Klasse hinzu.</span><span class="sxs-lookup"><span data-stu-id="683f5-143">Add a `TodoContext` class.</span></span> <span data-ttu-id="683f5-144">Klicken Sie mit der rechten Maustaste auf den Ordner *Modelle*, und wählen Sie **Hinzufügen** > **Klasse** aus.</span><span class="sxs-lookup"><span data-stu-id="683f5-144">Right-click the *Models* folder and select **Add** > **Class**.</span></span> <span data-ttu-id="683f5-145">Benennen Sie die Klasse `TodoContext`, und klicken Sie auf **Hinzufügen**.</span><span class="sxs-lookup"><span data-stu-id="683f5-145">Name the class `TodoContext` and select **Add**.</span></span>

<span data-ttu-id="683f5-146">Ersetzen Sie die Klasse durch den folgenden Code:</span><span class="sxs-lookup"><span data-stu-id="683f5-146">Replace the class with the following code:</span></span>

[!code-csharp[Main](first-web-api/sample/TodoApi/Models/TodoContext.cs)]

[!INCLUDE[Register the database context](../includes/webApi/register_dbContext.md)]

### <a name="add-a-controller"></a><span data-ttu-id="683f5-147">Hinzufügen eines Controllers</span><span class="sxs-lookup"><span data-stu-id="683f5-147">Add a controller</span></span>

<span data-ttu-id="683f5-148">Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf den Ordner *Controller*.</span><span class="sxs-lookup"><span data-stu-id="683f5-148">In Solution Explorer, right-click the *Controllers* folder.</span></span> <span data-ttu-id="683f5-149">Klicken Sie auf **Hinzufügen** > **Neues Element**.</span><span class="sxs-lookup"><span data-stu-id="683f5-149">Select **Add** > **New Item**.</span></span> <span data-ttu-id="683f5-150">Wählen Sie im Dialogfeld **Neues Element hinzufügen** die Vorlage **Web-API-Controllerklasse** aus.</span><span class="sxs-lookup"><span data-stu-id="683f5-150">In the **Add New Item** dialog, select the **Web API Controller Class** template.</span></span> <span data-ttu-id="683f5-151">Nennen Sie die Klasse `TodoController`.</span><span class="sxs-lookup"><span data-stu-id="683f5-151">Name the class `TodoController`.</span></span>

![Dialogfeld „Neues Element“ mit Controller im Suchfeld und ausgewähltem Web-API-Controller](first-web-api/_static/new_controller.png)

<span data-ttu-id="683f5-153">Ersetzen Sie die Klasse durch den folgenden Code:</span><span class="sxs-lookup"><span data-stu-id="683f5-153">Replace the class with the following code:</span></span>

[!INCLUDE[code and get todo items](../includes/webApi/getTodoItems.md)]

### <a name="launch-the-app"></a><span data-ttu-id="683f5-154">Starten der App</span><span class="sxs-lookup"><span data-stu-id="683f5-154">Launch the app</span></span>

<span data-ttu-id="683f5-155">Drücken Sie in Visual Studio STRG+F5 zum Starten der App.</span><span class="sxs-lookup"><span data-stu-id="683f5-155">In Visual Studio, press CTRL+F5 to launch the app.</span></span> <span data-ttu-id="683f5-156">Visual Studio startet einen Browser und navigiert zu `http://localhost:port/api/values`, wobei *port* eine zufällig ausgewählte Portnummer ist.</span><span class="sxs-lookup"><span data-stu-id="683f5-156">Visual Studio launches a browser and navigates to `http://localhost:port/api/values`, where *port* is a randomly chosen port number.</span></span> <span data-ttu-id="683f5-157">Navigieren Sie zum `Todo`-Controller auf `http://localhost:port/api/todo`.</span><span class="sxs-lookup"><span data-stu-id="683f5-157">Navigate to the `Todo` controller at `http://localhost:port/api/todo`.</span></span>

[!INCLUDE[last part of web API](../includes/webApi/end.md)]

[!INCLUDE[next steps](../includes/webApi/next.md)]

