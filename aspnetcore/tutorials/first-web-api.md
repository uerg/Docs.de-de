---
title: "Erstellen einer Web-API mit ASP.NET Core und Visual Studio für Windows"
author: rick-anderson
description: "Erstellen einer Web-API mit ASP.NET Core MVC und Visual Studio für Windows"
keywords: ASP.NET Core, WebAPI, Web-API, REST, HTTP, Service, HTTP-Dienst
ms.author: riande
manager: wpickett
ms.date: 8/15/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/first-web-api
ms.openlocfilehash: c57c73c6f9c60874ef88749b838ed1cc1d353ead
ms.sourcegitcommit: 7fef13045e98d716c589a2982613dad261694a65
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2017
---
#<a name="create-a-web-api-with-aspnet-core-and-visual-studio-for-windows"></a><span data-ttu-id="59cc2-104">Erstellen einer Web-API mit ASP.NET Core und Visual Studio für Windows</span><span class="sxs-lookup"><span data-stu-id="59cc2-104">Create a web API with ASP.NET Core and Visual Studio for Windows</span></span>

<span data-ttu-id="59cc2-105">Von [Rick Anderson](https://twitter.com/RickAndMSFT) und [Mike Wasson](https://github.com/mikewasson)</span><span class="sxs-lookup"><span data-stu-id="59cc2-105">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Mike Wasson](https://github.com/mikewasson)</span></span>

<span data-ttu-id="59cc2-106">In diesem Tutorial erstellen Sie eine Web-API zum Verwalten einer Liste von „To-Do“-Elementen.</span><span class="sxs-lookup"><span data-stu-id="59cc2-106">In this tutorial, you’ll build a web API for managing a list of "to-do" items.</span></span> <span data-ttu-id="59cc2-107">Sie werden keine Benutzeroberfläche erstellen.</span><span class="sxs-lookup"><span data-stu-id="59cc2-107">You won’t build a UI.</span></span>

<span data-ttu-id="59cc2-108">Es gibt drei Versionen dieses Tutorials:</span><span class="sxs-lookup"><span data-stu-id="59cc2-108">There are 3 versions of this tutorial:</span></span>

* <span data-ttu-id="59cc2-109">Windows: Web-API mit Visual Studio für Windows (dieses Tutorial)</span><span class="sxs-lookup"><span data-stu-id="59cc2-109">Windows: Web API with Visual Studio for Windows (This tutorial)</span></span>
* <span data-ttu-id="59cc2-110">macOS: [Web-API mit Visual Studio für Mac](xref:tutorials/first-web-api-mac)</span><span class="sxs-lookup"><span data-stu-id="59cc2-110">macOS: [Web API with Visual Studio for Mac](xref:tutorials/first-web-api-mac)</span></span>
* <span data-ttu-id="59cc2-111">macOS, Linux, Windows: [Web-API mit Visual Studio Code](xref:tutorials/web-api-vsc)</span><span class="sxs-lookup"><span data-stu-id="59cc2-111">macOS, Linux, Windows: [Web API with Visual Studio Code](xref:tutorials/web-api-vsc)</span></span>

<!-- WARNING: The code AND images in this doc are used by uid: tutorials/web-api-vsc, tutorials/first-web-api-mac and tutorials/first-web-api. If you change any code/images in this tutorial, update uid: tutorials/web-api-vsc -->

[!INCLUDE[intro to web API](../includes/webApi/intro.md)]

## <a name="prerequisites"></a><span data-ttu-id="59cc2-112">Erforderliche Komponenten</span><span class="sxs-lookup"><span data-stu-id="59cc2-112">Prerequisites</span></span>

[!INCLUDE[install 2.0](../includes/install2.0.md)]

<span data-ttu-id="59cc2-113">In [dieser PDF](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/first-web-api/_static/_webAPI.pdf) finden Sie die Version ASP.NET Core 1.1.</span><span class="sxs-lookup"><span data-stu-id="59cc2-113">See [this PDF](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/first-web-api/_static/_webAPI.pdf) for the ASP.NET Core 1.1 version.</span></span>

## <a name="create-the-project"></a><span data-ttu-id="59cc2-114">Erstellen eines Projekts</span><span class="sxs-lookup"><span data-stu-id="59cc2-114">Create the project</span></span>

<span data-ttu-id="59cc2-115">Klicken Sie in Visual Studio auf das Menü **Datei** > **Neu** > **Projekt**.</span><span class="sxs-lookup"><span data-stu-id="59cc2-115">From Visual Studio, select **File** menu, > **New** > **Project**.</span></span>

<span data-ttu-id="59cc2-116">Wählen Sie die Projektvorlage **ASP.NET Core Web Application (.NET Core)** aus.</span><span class="sxs-lookup"><span data-stu-id="59cc2-116">Select the **ASP.NET Core Web Application (.NET Core)** project template.</span></span> <span data-ttu-id="59cc2-117">Geben Sie dem Projekt den Namen `TodoApi`, und klicken Sie auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="59cc2-117">Name the project `TodoApi` and select **OK**.</span></span>

![Dialogfeld "Neues Projekt"](first-web-api/_static/new-project.png)

<span data-ttu-id="59cc2-119">Wählen Sie im Dialogfeld **ASP.NET Core-Webanwendung – TodoAPI** die Vorlage **Web-API** aus.</span><span class="sxs-lookup"><span data-stu-id="59cc2-119">In the **New ASP.NET Core Web Application - TodoApi** dialog, select the **Web API** template.</span></span> <span data-ttu-id="59cc2-120">Klicken Sie auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="59cc2-120">Select **OK**.</span></span> <span data-ttu-id="59cc2-121">Wählen Sie **nicht** **Enable Docker Support** (Docker-Unterstützung aktivieren) aus.</span><span class="sxs-lookup"><span data-stu-id="59cc2-121">Do **not** select **Enable Docker Support**.</span></span>

![Dialogfeld Neue ASP.NET-Webanwendung mit ausgewählter Web-API-Projektvorlage aus ASP.NET Core-Vorlagen](first-web-api/_static/web-api-project.png)

### <a name="launch-the-app"></a><span data-ttu-id="59cc2-123">Starten der App</span><span class="sxs-lookup"><span data-stu-id="59cc2-123">Launch the app</span></span>

<span data-ttu-id="59cc2-124">Drücken Sie in Visual Studio STRG+F5 zum Starten der App.</span><span class="sxs-lookup"><span data-stu-id="59cc2-124">In Visual Studio, press CTRL+F5 to launch the app.</span></span> <span data-ttu-id="59cc2-125">Visual Studio startet einen Browser und navigiert zu `http://localhost:port/api/values`, wobei es sich bei *port* um eine zufällig ausgewählte Portnummer handelt.</span><span class="sxs-lookup"><span data-stu-id="59cc2-125">Visual Studio launches a browser and navigates to `http://localhost:port/api/values`, where *port* is a randomly-chosen port number.</span></span> <span data-ttu-id="59cc2-126">Chrome, Edge und Firefox zeigen Folgendes an:</span><span class="sxs-lookup"><span data-stu-id="59cc2-126">Chrome, Edge, and Firefox display the following:</span></span>

```
["value1","value2"]
``` 

### <a name="add-a-model-class"></a><span data-ttu-id="59cc2-127">Hinzufügen einer Modellklasse</span><span class="sxs-lookup"><span data-stu-id="59cc2-127">Add a model class</span></span>

<span data-ttu-id="59cc2-128">Ein Modell ist ein Objekt, das die Daten in Ihrer Anwendung darstellt.</span><span class="sxs-lookup"><span data-stu-id="59cc2-128">A model is an object that represents the data in your application.</span></span> <span data-ttu-id="59cc2-129">In diesem Fall ist das einzige Modell ein To-Do-Element.</span><span class="sxs-lookup"><span data-stu-id="59cc2-129">In this case, the only model is a to-do item.</span></span>

<span data-ttu-id="59cc2-130">Fügen Sie einen Ordner namens „Modelle“ hinzu.</span><span class="sxs-lookup"><span data-stu-id="59cc2-130">Add a folder named "Models".</span></span> <span data-ttu-id="59cc2-131">Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf den Projektnamen.</span><span class="sxs-lookup"><span data-stu-id="59cc2-131">In Solution Explorer, right-click the project.</span></span> <span data-ttu-id="59cc2-132">Klicken Sie auf **Hinzufügen** > **Neuer Ordner**.</span><span class="sxs-lookup"><span data-stu-id="59cc2-132">Select **Add** > **New Folder**.</span></span> <span data-ttu-id="59cc2-133">Geben Sie dem Ordner den Namen *Modelle*.</span><span class="sxs-lookup"><span data-stu-id="59cc2-133">Name the folder *Models*.</span></span>

<span data-ttu-id="59cc2-134">Hinweis: Die Modellklassen können überall in Ihrem Projekt platziert werden, aber der Ordner *Modelle* wird gemäß der Konvention verwendet.</span><span class="sxs-lookup"><span data-stu-id="59cc2-134">Note: The model classes go anywhere in your project, but the *Models* folder is used by convention.</span></span>

<span data-ttu-id="59cc2-135">Fügen Sie eine `TodoItem`-Klasse hinzu.</span><span class="sxs-lookup"><span data-stu-id="59cc2-135">Add a `TodoItem` class.</span></span> <span data-ttu-id="59cc2-136">Klicken Sie mit der rechten Maustaste auf den Ordner *Modelle*, und wählen Sie **Hinzufügen** > **Klasse** aus.</span><span class="sxs-lookup"><span data-stu-id="59cc2-136">Right-click the *Models* folder and select **Add** > **Class**.</span></span> <span data-ttu-id="59cc2-137">Benennen Sie die Klasse `TodoItem`, und klicken Sie auf **Hinzufügen**.</span><span class="sxs-lookup"><span data-stu-id="59cc2-137">Name the class `TodoItem` and select **Add**.</span></span>

<span data-ttu-id="59cc2-138">Ersetzen Sie den generierten Code durch den folgenden:</span><span class="sxs-lookup"><span data-stu-id="59cc2-138">Replace the generated code with the following:</span></span>

<span data-ttu-id="59cc2-139">[!code-csharp[Main](first-web-api/sample/TodoApi/Models/TodoItem.cs)]</span><span class="sxs-lookup"><span data-stu-id="59cc2-139">[!code-csharp[Main](first-web-api/sample/TodoApi/Models/TodoItem.cs)]</span></span>

<span data-ttu-id="59cc2-140">Die Datenbank generiert die `Id`, wenn ein `TodoItem` erstellt wird.</span><span class="sxs-lookup"><span data-stu-id="59cc2-140">The database generates the `Id` when a `TodoItem` is created.</span></span>

### <a name="create-the-database-context"></a><span data-ttu-id="59cc2-141">Erstellen des Datenbankkontexts</span><span class="sxs-lookup"><span data-stu-id="59cc2-141">Create the database context</span></span>

<span data-ttu-id="59cc2-142">Der *Datenbankkontext* ist die Hauptklasse, die die Entity Framework-Funktionen für ein angegebenes Datenmodell koordiniert.</span><span class="sxs-lookup"><span data-stu-id="59cc2-142">The *database context* is the main class that coordinates Entity Framework functionality for a given data model.</span></span> <span data-ttu-id="59cc2-143">Diese Klasse wird durch Ableiten von der `Microsoft.EntityFrameworkCore.DbContext`-Klasse erstellt.</span><span class="sxs-lookup"><span data-stu-id="59cc2-143">This class is created by deriving from the `Microsoft.EntityFrameworkCore.DbContext` class.</span></span>

<span data-ttu-id="59cc2-144">Fügen Sie eine `TodoContext`-Klasse hinzu.</span><span class="sxs-lookup"><span data-stu-id="59cc2-144">Add a `TodoContext` class.</span></span> <span data-ttu-id="59cc2-145">Klicken Sie mit der rechten Maustaste auf den Ordner *Modelle*, und wählen Sie **Hinzufügen** > **Klasse** aus.</span><span class="sxs-lookup"><span data-stu-id="59cc2-145">Right-click the *Models* folder and select **Add** > **Class**.</span></span> <span data-ttu-id="59cc2-146">Benennen Sie die Klasse `TodoContext`, und klicken Sie auf **Hinzufügen**.</span><span class="sxs-lookup"><span data-stu-id="59cc2-146">Name the class `TodoContext` and select **Add**.</span></span>

<span data-ttu-id="59cc2-147">Ersetzen Sie den generierten Code durch den folgenden:</span><span class="sxs-lookup"><span data-stu-id="59cc2-147">Replace the generated code with the following:</span></span>

<span data-ttu-id="59cc2-148">[!code-csharp[Main](first-web-api/sample/TodoApi/Models/TodoContext.cs)]</span><span class="sxs-lookup"><span data-stu-id="59cc2-148">[!code-csharp[Main](first-web-api/sample/TodoApi/Models/TodoContext.cs)]</span></span>

[!INCLUDE[Register the database context](../includes/webApi/register_dbContext.md)]

### <a name="add-a-controller"></a><span data-ttu-id="59cc2-149">Hinzufügen eines Controllers</span><span class="sxs-lookup"><span data-stu-id="59cc2-149">Add a controller</span></span>

<span data-ttu-id="59cc2-150">Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf den Ordner *Controller*.</span><span class="sxs-lookup"><span data-stu-id="59cc2-150">In Solution Explorer, right-click the *Controllers* folder.</span></span> <span data-ttu-id="59cc2-151">Klicken Sie auf **Hinzufügen** > **Neues Element**.</span><span class="sxs-lookup"><span data-stu-id="59cc2-151">Select **Add** > **New Item**.</span></span> <span data-ttu-id="59cc2-152">Wählen Sie im Dialogfeld **Neues Element hinzufügen** die Vorlage **Web API Controller Class** aus.</span><span class="sxs-lookup"><span data-stu-id="59cc2-152">In the **Add New Item** dialog, select the **Web  API Controller Class** template.</span></span> <span data-ttu-id="59cc2-153">Nennen Sie die Klasse `TodoController`.</span><span class="sxs-lookup"><span data-stu-id="59cc2-153">Name the class `TodoController`.</span></span>

![Dialogfeld „Neues Element“ mit Controller im Suchfeld und ausgewähltem Web-API-Controller](first-web-api/_static/new_controller.png)

<span data-ttu-id="59cc2-155">Ersetzen Sie den generierten Code durch den folgenden:</span><span class="sxs-lookup"><span data-stu-id="59cc2-155">Replace the generated code with the following:</span></span>

[!INCLUDE[code and get todo items](../includes/webApi/getTodoItems.md)]
  
### <a name="launch-the-app"></a><span data-ttu-id="59cc2-156">Starten der App</span><span class="sxs-lookup"><span data-stu-id="59cc2-156">Launch the app</span></span>

<span data-ttu-id="59cc2-157">Drücken Sie in Visual Studio STRG+F5 zum Starten der App.</span><span class="sxs-lookup"><span data-stu-id="59cc2-157">In Visual Studio, press CTRL+F5 to launch the app.</span></span> <span data-ttu-id="59cc2-158">Visual Studio startet einen Browser und navigiert zu `http://localhost:port/api/values`, wobei *port* eine zufällig ausgewählte Portnummer ist.</span><span class="sxs-lookup"><span data-stu-id="59cc2-158">Visual Studio launches a browser and navigates to `http://localhost:port/api/values`, where *port* is a randomly chosen port number.</span></span> <span data-ttu-id="59cc2-159">Wenn Sie Chrome, Edge oder Firefox verwenden, werden die Daten angezeigt.</span><span class="sxs-lookup"><span data-stu-id="59cc2-159">If you're using Chrome, Edge or Firefox, the data will be displayed.</span></span> <span data-ttu-id="59cc2-160">Wenn Sie Internet Explorer verwenden, werden Sie aufgefordert, die Datei *values.json* zu öffnen oder zu speichern.</span><span class="sxs-lookup"><span data-stu-id="59cc2-160">If you're using IE, IE will prompt to you open or save the *values.json* file.</span></span> <span data-ttu-id="59cc2-161">Navigieren Sie zum `Todo`-Controller, den wir eben erstellt haben `http://localhost:port/api/todo`.</span><span class="sxs-lookup"><span data-stu-id="59cc2-161">Navigate to the `Todo` controller we just created `http://localhost:port/api/todo`.</span></span>

[!INCLUDE[last part of web API](../includes/webApi/end.md)]

[!INCLUDE[next steps](../includes/webApi/next.md)]

