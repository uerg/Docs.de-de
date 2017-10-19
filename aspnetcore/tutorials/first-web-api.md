---
title: "Erstellen einer Web-API mit ASP.NET Core und Visual Studio für Windows"
author: rick-anderson
description: "Erstellen einer Web-API mit ASP.NET Core MVC und Visual Studio für Windows"
keywords: ASP.NET Core, WebAPI, Web-API, REST, HTTP, Service, HTTP-Dienst
ms.author: riande
manager: wpickett
ms.date: 08/15/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/first-web-api
ms.openlocfilehash: 617b11cd7652e393c06446c62138802e4a4e90df
ms.sourcegitcommit: 67f54fabbfa4e3942f5bfe1f8a7fdfe4a7a75358
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/19/2017
---
#<a name="create-a-web-api-with-aspnet-core-and-visual-studio-for-windows"></a><span data-ttu-id="400c8-104">Erstellen einer Web-API mit ASP.NET Core und Visual Studio für Windows</span><span class="sxs-lookup"><span data-stu-id="400c8-104">Create a web API with ASP.NET Core and Visual Studio for Windows</span></span>

<span data-ttu-id="400c8-105">Von [Rick Anderson](https://twitter.com/RickAndMSFT) und [Mike Wasson](https://github.com/mikewasson)</span><span class="sxs-lookup"><span data-stu-id="400c8-105">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Mike Wasson](https://github.com/mikewasson)</span></span>

<span data-ttu-id="400c8-106">In diesem Tutorial erstellen Sie eine Web-API zum Verwalten einer Liste von „To-Do“-Elementen.</span><span class="sxs-lookup"><span data-stu-id="400c8-106">In this tutorial, you’ll build a web API for managing a list of "to-do" items.</span></span> <span data-ttu-id="400c8-107">Sie werden keine Benutzeroberfläche erstellen.</span><span class="sxs-lookup"><span data-stu-id="400c8-107">You won’t build a UI.</span></span>

<span data-ttu-id="400c8-108">Es gibt drei Versionen dieses Tutorials:</span><span class="sxs-lookup"><span data-stu-id="400c8-108">There are 3 versions of this tutorial:</span></span>

* <span data-ttu-id="400c8-109">Windows: Web-API mit Visual Studio für Windows (dieses Tutorial)</span><span class="sxs-lookup"><span data-stu-id="400c8-109">Windows: Web API with Visual Studio for Windows (This tutorial)</span></span>
* <span data-ttu-id="400c8-110">macOS: [Web-API mit Visual Studio für Mac](xref:tutorials/first-web-api-mac)</span><span class="sxs-lookup"><span data-stu-id="400c8-110">macOS: [Web API with Visual Studio for Mac](xref:tutorials/first-web-api-mac)</span></span>
* <span data-ttu-id="400c8-111">macOS, Linux, Windows: [Web-API mit Visual Studio Code](xref:tutorials/web-api-vsc)</span><span class="sxs-lookup"><span data-stu-id="400c8-111">macOS, Linux, Windows: [Web API with Visual Studio Code](xref:tutorials/web-api-vsc)</span></span>

<!-- WARNING: The code AND images in this doc are used by uid: tutorials/web-api-vsc, tutorials/first-web-api-mac and tutorials/first-web-api. If you change any code/images in this tutorial, update uid: tutorials/web-api-vsc -->

[!INCLUDE[intro to web API](../includes/webApi/intro.md)]

## <a name="prerequisites"></a><span data-ttu-id="400c8-112">Erforderliche Komponenten</span><span class="sxs-lookup"><span data-stu-id="400c8-112">Prerequisites</span></span>

[!INCLUDE[install 2.0](../includes/install2.0.md)]

<span data-ttu-id="400c8-113">In [dieser PDF](https://github.com/aspnet/Docs/blob/master/aspnetcore/tutorials/first-web-api/_static/_webAPI.pdf) finden Sie die Version ASP.NET Core 1.1.</span><span class="sxs-lookup"><span data-stu-id="400c8-113">See [this PDF](https://github.com/aspnet/Docs/blob/master/aspnetcore/tutorials/first-web-api/_static/_webAPI.pdf) for the ASP.NET Core 1.1 version.</span></span>

## <a name="create-the-project"></a><span data-ttu-id="400c8-114">Erstellen eines Projekts</span><span class="sxs-lookup"><span data-stu-id="400c8-114">Create the project</span></span>

<span data-ttu-id="400c8-115">Klicken Sie in Visual Studio auf das Menü **Datei** > **Neu** > **Projekt**.</span><span class="sxs-lookup"><span data-stu-id="400c8-115">From Visual Studio, select **File** menu, > **New** > **Project**.</span></span>

<span data-ttu-id="400c8-116">Wählen Sie die Projektvorlage **ASP.NET Core Web Application (.NET Core)** aus.</span><span class="sxs-lookup"><span data-stu-id="400c8-116">Select the **ASP.NET Core Web Application (.NET Core)** project template.</span></span> <span data-ttu-id="400c8-117">Geben Sie dem Projekt den Namen `TodoApi`, und klicken Sie auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="400c8-117">Name the project `TodoApi` and select **OK**.</span></span>

![Dialogfeld "Neues Projekt"](first-web-api/_static/new-project.png)

<span data-ttu-id="400c8-119">Wählen Sie im Dialogfeld **ASP.NET Core-Webanwendung – TodoAPI** die Vorlage **Web-API** aus.</span><span class="sxs-lookup"><span data-stu-id="400c8-119">In the **New ASP.NET Core Web Application - TodoApi** dialog, select the **Web API** template.</span></span> <span data-ttu-id="400c8-120">Klicken Sie auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="400c8-120">Select **OK**.</span></span> <span data-ttu-id="400c8-121">Wählen Sie **nicht** **Enable Docker Support** (Docker-Unterstützung aktivieren) aus.</span><span class="sxs-lookup"><span data-stu-id="400c8-121">Do **not** select **Enable Docker Support**.</span></span>

![Dialogfeld Neue ASP.NET-Webanwendung mit ausgewählter Web-API-Projektvorlage aus ASP.NET Core-Vorlagen](first-web-api/_static/web-api-project.png)

### <a name="launch-the-app"></a><span data-ttu-id="400c8-123">Starten der App</span><span class="sxs-lookup"><span data-stu-id="400c8-123">Launch the app</span></span>

<span data-ttu-id="400c8-124">Drücken Sie in Visual Studio STRG+F5 zum Starten der App.</span><span class="sxs-lookup"><span data-stu-id="400c8-124">In Visual Studio, press CTRL+F5 to launch the app.</span></span> <span data-ttu-id="400c8-125">Visual Studio startet einen Browser und navigiert zu `http://localhost:port/api/values`, wobei es sich bei *port* um eine zufällig ausgewählte Portnummer handelt.</span><span class="sxs-lookup"><span data-stu-id="400c8-125">Visual Studio launches a browser and navigates to `http://localhost:port/api/values`, where *port* is a randomly-chosen port number.</span></span> <span data-ttu-id="400c8-126">Chrome, Microsoft Edge und Firefox zeigen Folgendes an:</span><span class="sxs-lookup"><span data-stu-id="400c8-126">Chrome, Edge, and Firefox display the following:</span></span>

```
["value1","value2"]
``` 

### <a name="add-a-model-class"></a><span data-ttu-id="400c8-127">Hinzufügen einer Modellklasse</span><span class="sxs-lookup"><span data-stu-id="400c8-127">Add a model class</span></span>

<span data-ttu-id="400c8-128">Ein Modell ist ein Objekt, das die Daten in Ihrer Anwendung darstellt.</span><span class="sxs-lookup"><span data-stu-id="400c8-128">A model is an object that represents the data in your application.</span></span> <span data-ttu-id="400c8-129">In diesem Fall ist das einzige Modell ein To-Do-Element.</span><span class="sxs-lookup"><span data-stu-id="400c8-129">In this case, the only model is a to-do item.</span></span>

<span data-ttu-id="400c8-130">Fügen Sie einen Ordner namens „Modelle“ hinzu.</span><span class="sxs-lookup"><span data-stu-id="400c8-130">Add a folder named "Models".</span></span> <span data-ttu-id="400c8-131">Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf den Projektnamen.</span><span class="sxs-lookup"><span data-stu-id="400c8-131">In Solution Explorer, right-click the project.</span></span> <span data-ttu-id="400c8-132">Klicken Sie auf **Hinzufügen** > **Neuer Ordner**.</span><span class="sxs-lookup"><span data-stu-id="400c8-132">Select **Add** > **New Folder**.</span></span> <span data-ttu-id="400c8-133">Geben Sie dem Ordner den Namen *Modelle*.</span><span class="sxs-lookup"><span data-stu-id="400c8-133">Name the folder *Models*.</span></span>

<span data-ttu-id="400c8-134">Hinweis: Die Modellklassen können überall in Ihrem Projekt platziert werden, aber der Ordner *Modelle* wird gemäß der Konvention verwendet.</span><span class="sxs-lookup"><span data-stu-id="400c8-134">Note: The model classes go anywhere in your project, but the *Models* folder is used by convention.</span></span>

<span data-ttu-id="400c8-135">Fügen Sie eine `TodoItem`-Klasse hinzu.</span><span class="sxs-lookup"><span data-stu-id="400c8-135">Add a `TodoItem` class.</span></span> <span data-ttu-id="400c8-136">Klicken Sie mit der rechten Maustaste auf den Ordner *Modelle*, und wählen Sie **Hinzufügen** > **Klasse** aus.</span><span class="sxs-lookup"><span data-stu-id="400c8-136">Right-click the *Models* folder and select **Add** > **Class**.</span></span> <span data-ttu-id="400c8-137">Benennen Sie die Klasse `TodoItem`, und klicken Sie auf **Hinzufügen**.</span><span class="sxs-lookup"><span data-stu-id="400c8-137">Name the class `TodoItem` and select **Add**.</span></span>

<span data-ttu-id="400c8-138">Ersetzen Sie den generierten Code durch den folgenden:</span><span class="sxs-lookup"><span data-stu-id="400c8-138">Replace the generated code with the following:</span></span>

[!code-csharp[Main](first-web-api/sample/TodoApi/Models/TodoItem.cs)]

<span data-ttu-id="400c8-139">Die Datenbank generiert die `Id`, wenn ein `TodoItem` erstellt wird.</span><span class="sxs-lookup"><span data-stu-id="400c8-139">The database generates the `Id` when a `TodoItem` is created.</span></span>

### <a name="create-the-database-context"></a><span data-ttu-id="400c8-140">Erstellen des Datenbankkontexts</span><span class="sxs-lookup"><span data-stu-id="400c8-140">Create the database context</span></span>

<span data-ttu-id="400c8-141">Der *Datenbankkontext* ist die Hauptklasse, die die Entity Framework-Funktionen für ein angegebenes Datenmodell koordiniert.</span><span class="sxs-lookup"><span data-stu-id="400c8-141">The *database context* is the main class that coordinates Entity Framework functionality for a given data model.</span></span> <span data-ttu-id="400c8-142">Diese Klasse wird durch Ableiten von der `Microsoft.EntityFrameworkCore.DbContext`-Klasse erstellt.</span><span class="sxs-lookup"><span data-stu-id="400c8-142">This class is created by deriving from the `Microsoft.EntityFrameworkCore.DbContext` class.</span></span>

<span data-ttu-id="400c8-143">Fügen Sie eine `TodoContext`-Klasse hinzu.</span><span class="sxs-lookup"><span data-stu-id="400c8-143">Add a `TodoContext` class.</span></span> <span data-ttu-id="400c8-144">Klicken Sie mit der rechten Maustaste auf den Ordner *Modelle*, und wählen Sie **Hinzufügen** > **Klasse** aus.</span><span class="sxs-lookup"><span data-stu-id="400c8-144">Right-click the *Models* folder and select **Add** > **Class**.</span></span> <span data-ttu-id="400c8-145">Benennen Sie die Klasse `TodoContext`, und klicken Sie auf **Hinzufügen**.</span><span class="sxs-lookup"><span data-stu-id="400c8-145">Name the class `TodoContext` and select **Add**.</span></span>

<span data-ttu-id="400c8-146">Ersetzen Sie den generierten Code durch den folgenden:</span><span class="sxs-lookup"><span data-stu-id="400c8-146">Replace the generated code with the following:</span></span>

[!code-csharp[Main](first-web-api/sample/TodoApi/Models/TodoContext.cs)]

[!INCLUDE[Register the database context](../includes/webApi/register_dbContext.md)]

### <a name="add-a-controller"></a><span data-ttu-id="400c8-147">Hinzufügen eines Controllers</span><span class="sxs-lookup"><span data-stu-id="400c8-147">Add a controller</span></span>

<span data-ttu-id="400c8-148">Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf den Ordner *Controller*.</span><span class="sxs-lookup"><span data-stu-id="400c8-148">In Solution Explorer, right-click the *Controllers* folder.</span></span> <span data-ttu-id="400c8-149">Klicken Sie auf **Hinzufügen** > **Neues Element**.</span><span class="sxs-lookup"><span data-stu-id="400c8-149">Select **Add** > **New Item**.</span></span> <span data-ttu-id="400c8-150">Wählen Sie im Dialogfeld **Neues Element hinzufügen** die Vorlage **Web API Controller Class** aus.</span><span class="sxs-lookup"><span data-stu-id="400c8-150">In the **Add New Item** dialog, select the **Web  API Controller Class** template.</span></span> <span data-ttu-id="400c8-151">Nennen Sie die Klasse `TodoController`.</span><span class="sxs-lookup"><span data-stu-id="400c8-151">Name the class `TodoController`.</span></span>

![Dialogfeld „Neues Element“ mit Controller im Suchfeld und ausgewähltem Web-API-Controller](first-web-api/_static/new_controller.png)

<span data-ttu-id="400c8-153">Ersetzen Sie den generierten Code durch den folgenden:</span><span class="sxs-lookup"><span data-stu-id="400c8-153">Replace the generated code with the following:</span></span>

[!INCLUDE[code and get todo items](../includes/webApi/getTodoItems.md)]
  
### <a name="launch-the-app"></a><span data-ttu-id="400c8-154">Starten der App</span><span class="sxs-lookup"><span data-stu-id="400c8-154">Launch the app</span></span>

<span data-ttu-id="400c8-155">Drücken Sie in Visual Studio STRG+F5 zum Starten der App.</span><span class="sxs-lookup"><span data-stu-id="400c8-155">In Visual Studio, press CTRL+F5 to launch the app.</span></span> <span data-ttu-id="400c8-156">Visual Studio startet einen Browser und navigiert zu `http://localhost:port/api/values`, wobei *port* eine zufällig ausgewählte Portnummer ist.</span><span class="sxs-lookup"><span data-stu-id="400c8-156">Visual Studio launches a browser and navigates to `http://localhost:port/api/values`, where *port* is a randomly chosen port number.</span></span> <span data-ttu-id="400c8-157">Wenn Sie Chrome, Microsoft Edge oder Firefox verwenden, werden die Daten angezeigt.</span><span class="sxs-lookup"><span data-stu-id="400c8-157">If you're using Chrome, Edge or Firefox, the data will be displayed.</span></span> <span data-ttu-id="400c8-158">Wenn Sie Internet Explorer verwenden, werden Sie aufgefordert, die Datei *values.json* zu öffnen oder zu speichern.</span><span class="sxs-lookup"><span data-stu-id="400c8-158">If you're using IE, IE will prompt to you open or save the *values.json* file.</span></span> <span data-ttu-id="400c8-159">Navigieren Sie zum `Todo`-Controller, den wir eben erstellt haben `http://localhost:port/api/todo`.</span><span class="sxs-lookup"><span data-stu-id="400c8-159">Navigate to the `Todo` controller we just created `http://localhost:port/api/todo`.</span></span>

[!INCLUDE[last part of web API](../includes/webApi/end.md)]

[!INCLUDE[next steps](../includes/webApi/next.md)]

