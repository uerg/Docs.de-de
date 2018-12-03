---
title: Erstellen einer Web-API mit ASP.NET Core und Visual Studio
author: rick-anderson
description: Erstellen einer Web-API mit ASP.NET Core MVC und Visual Studio für Windows
ms.author: riande
ms.custom: mvc
ms.date: 05/17/2018
uid: tutorials/first-web-api
ms.openlocfilehash: eb23d5376742e04712f714263815582fddc5d18e
ms.sourcegitcommit: e9b99854b0a8021dafabee0db5e1338067f250a9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/28/2018
ms.locfileid: "52450696"
---
# <a name="create-a-web-api-with-aspnet-core-and-visual-studio"></a><span data-ttu-id="d8627-103">Erstellen einer Web-API mit ASP.NET Core und Visual Studio</span><span class="sxs-lookup"><span data-stu-id="d8627-103">Create a Web API with ASP.NET Core and Visual Studio</span></span>

<span data-ttu-id="d8627-104">Von [Rick Anderson](https://twitter.com/RickAndMSFT) und [Mike Wasson](https://github.com/mikewasson)</span><span class="sxs-lookup"><span data-stu-id="d8627-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Mike Wasson](https://github.com/mikewasson)</span></span>

<span data-ttu-id="d8627-105">In diesem Tutorial wird eine Web-API zum Verwalten einer Liste von Aufgabenelementen erstellt.</span><span class="sxs-lookup"><span data-stu-id="d8627-105">This tutorial builds a web API for managing a list of "to-do" items.</span></span> <span data-ttu-id="d8627-106">Es wird keine Benutzeroberfläche (UI) erstellt.</span><span class="sxs-lookup"><span data-stu-id="d8627-106">A user interface (UI) isn't created.</span></span>

<span data-ttu-id="d8627-107">Es gibt drei Versionen dieses Tutorials:</span><span class="sxs-lookup"><span data-stu-id="d8627-107">There are three versions of this tutorial:</span></span>

* <span data-ttu-id="d8627-108">Windows: Web-API mit Visual Studio für Windows (dieses Tutorial, siehe [Videoversion](https://www.youtube.com/watch?v=TTkhEyGBfAk))</span><span class="sxs-lookup"><span data-stu-id="d8627-108">Windows: Web API with Visual Studio on Windows (This tutorial, see the [video version](https://www.youtube.com/watch?v=TTkhEyGBfAk))</span></span>
* <span data-ttu-id="d8627-109">macOS: [Web-API mit Visual Studio für Mac](xref:tutorials/first-web-api-mac)</span><span class="sxs-lookup"><span data-stu-id="d8627-109">macOS: [Web API with Visual Studio for Mac](xref:tutorials/first-web-api-mac)</span></span>
* <span data-ttu-id="d8627-110">macOS, Linux, Windows: [Web-API mit Visual Studio Code](xref:tutorials/web-api-vsc)</span><span class="sxs-lookup"><span data-stu-id="d8627-110">macOS, Linux, Windows: [Web API with Visual Studio Code](xref:tutorials/web-api-vsc)</span></span>

<!-- WARNING: The code AND images in this doc are used by uid: tutorials/web-api-vsc, tutorials/first-web-api-mac and tutorials/first-web-api. If you change any code/images in this tutorial, update uid: tutorials/web-api-vsc -->

> [!NOTE]
> <span data-ttu-id="d8627-111">Wir testen gerade eine vorgeschlagene neue Struktur für das ASP.NET Core-Inhaltsverzeichnis.</span><span class="sxs-lookup"><span data-stu-id="d8627-111">We’re testing the usability of a proposed new structure for the ASP.NET Core table of contents.</span></span>  <span data-ttu-id="d8627-112">Falls Sie einige Minuten Zeit haben, um einen Test durchzuführen, in dem Sie sieben unterschiedliche Artikel im aktuellen und vorgeschlagene Inhaltsverzeichnis finden sollen, [klicken Sie hier, um daran teilzunehmen](https://dpk4xbh5.optimalworkshop.com/treejack/rps16hd5).</span><span class="sxs-lookup"><span data-stu-id="d8627-112">If you have a few minutes to try an exercise of finding 7 different topics in the current or proposed table of contents, please [click here to participate in the study](https://dpk4xbh5.optimalworkshop.com/treejack/rps16hd5).</span></span>

[!INCLUDE[intro to web API](../includes/webApi/intro.md)]

## <a name="prerequisites"></a><span data-ttu-id="d8627-113">Erforderliche Komponenten</span><span class="sxs-lookup"><span data-stu-id="d8627-113">Prerequisites</span></span>

[!INCLUDE[](~/includes/net-core-prereqs-windows.md)]

## <a name="create-the-project"></a><span data-ttu-id="d8627-114">Erstellen eines Projekts</span><span class="sxs-lookup"><span data-stu-id="d8627-114">Create the project</span></span>

<span data-ttu-id="d8627-115">Führen Sie in Visual Studio die folgenden Schritte aus:</span><span class="sxs-lookup"><span data-stu-id="d8627-115">Follow these steps in Visual Studio:</span></span>

* <span data-ttu-id="d8627-116">Wählen Sie im Menü **Datei** den Befehl **Neu** > **Projekt** aus.</span><span class="sxs-lookup"><span data-stu-id="d8627-116">From the **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="d8627-117">Wählen Sie die Vorlage **ASP.NET Core-Webanwendung** aus.</span><span class="sxs-lookup"><span data-stu-id="d8627-117">Select the **ASP.NET Core Web Application** template.</span></span> <span data-ttu-id="d8627-118">Geben Sie dem Projekt den Namen *TodoApi*, und klicken Sie auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="d8627-118">Name the project *TodoApi* and click **OK**.</span></span>
* <span data-ttu-id="d8627-119">Wählen Sie im Dialogfeld **ASP.NET Core-Webanwendung – TodoAPI** die ASP.NET Core-Version aus.</span><span class="sxs-lookup"><span data-stu-id="d8627-119">In the **New ASP.NET Core Web Application - TodoApi** dialog, choose the ASP.NET Core version.</span></span> <span data-ttu-id="d8627-120">Wählen Sie die Vorlage **API** aus, und klicken Sie auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="d8627-120">Select the **API** template and click **OK**.</span></span> <span data-ttu-id="d8627-121">Wählen Sie **nicht** **Enable Docker Support** (Docker-Unterstützung aktivieren) aus.</span><span class="sxs-lookup"><span data-stu-id="d8627-121">Do **not** select **Enable Docker Support**.</span></span>

### <a name="launch-the-app"></a><span data-ttu-id="d8627-122">Starten der App</span><span class="sxs-lookup"><span data-stu-id="d8627-122">Launch the app</span></span>

<span data-ttu-id="d8627-123">Drücken Sie in Visual Studio STRG+F5 zum Starten der App.</span><span class="sxs-lookup"><span data-stu-id="d8627-123">In Visual Studio, press CTRL+F5 to launch the app.</span></span> <span data-ttu-id="d8627-124">Visual Studio startet einen Browser und navigiert zu `http://localhost:<port>/api/values`, wobei es sich bei `<port>` um eine zufällig ausgewählte Portnummer handelt.</span><span class="sxs-lookup"><span data-stu-id="d8627-124">Visual Studio launches a browser and navigates to `http://localhost:<port>/api/values`, where `<port>` is a randomly chosen port number.</span></span> <span data-ttu-id="d8627-125">Chrome, Microsoft Edge und Firefox zeigen die folgende Ausgabe an:</span><span class="sxs-lookup"><span data-stu-id="d8627-125">Chrome, Microsoft Edge, and Firefox display the following output:</span></span>

```json
["value1","value2"]
```

<span data-ttu-id="d8627-126">Wenn Sie Internet Explorer verwenden, werden Sie dazu aufgefordert, eine *values.json*-Datei zu speichern.</span><span class="sxs-lookup"><span data-stu-id="d8627-126">If using Internet Explorer, you'll be prompted to save a *values.json* file.</span></span>

### <a name="add-a-model-class"></a><span data-ttu-id="d8627-127">Hinzufügen einer Modellklasse</span><span class="sxs-lookup"><span data-stu-id="d8627-127">Add a model class</span></span>

<span data-ttu-id="d8627-128">Ein Modell ist ein Objekt, das die Daten in der App darstellt.</span><span class="sxs-lookup"><span data-stu-id="d8627-128">A model is an object representing the data in the app.</span></span> <span data-ttu-id="d8627-129">In diesem Fall ist das einzige Modell ein To-do-Element.</span><span class="sxs-lookup"><span data-stu-id="d8627-129">In this case, the only model is a to-do item.</span></span>

<span data-ttu-id="d8627-130">Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf den Projektnamen.</span><span class="sxs-lookup"><span data-stu-id="d8627-130">In Solution Explorer, right-click the project.</span></span> <span data-ttu-id="d8627-131">Klicken Sie auf **Hinzufügen** > **Neuer Ordner**.</span><span class="sxs-lookup"><span data-stu-id="d8627-131">Select **Add** > **New Folder**.</span></span> <span data-ttu-id="d8627-132">Geben Sie dem Ordner den Namen *Modelle*.</span><span class="sxs-lookup"><span data-stu-id="d8627-132">Name the folder *Models*.</span></span>

> [!NOTE]
> <span data-ttu-id="d8627-133">Die Modellklassen können überall im Projekt gespeichert werden.</span><span class="sxs-lookup"><span data-stu-id="d8627-133">The model classes can go anywhere in the project.</span></span> <span data-ttu-id="d8627-134">Der Ordner *Modelle* wird gemäß Konvention für Modellklassen verwendet.</span><span class="sxs-lookup"><span data-stu-id="d8627-134">The *Models* folder is used by convention for model classes.</span></span>

<span data-ttu-id="d8627-135">Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf den Ordner *Modelle*, und wählen Sie anschließend **Hinzufügen** > **Klasse** aus.</span><span class="sxs-lookup"><span data-stu-id="d8627-135">In Solution Explorer, right-click the *Models* folder and select **Add** > **Class**.</span></span> <span data-ttu-id="d8627-136">Nennen Sie die Klasse *TodoItem*, und klicken Sie auf **Hinzufügen**.</span><span class="sxs-lookup"><span data-stu-id="d8627-136">Name the class *TodoItem* and click **Add**.</span></span>

<span data-ttu-id="d8627-137">Aktualisieren Sie die `TodoItem`-Klasse mit dem folgenden Code:</span><span class="sxs-lookup"><span data-stu-id="d8627-137">Update the `TodoItem` class with the following code:</span></span>

[!code-csharp[](first-web-api/samples/2.0/TodoApi/Models/TodoItem.cs)]

<span data-ttu-id="d8627-138">Die Datenbank generiert die `Id`, wenn ein `TodoItem` erstellt wird.</span><span class="sxs-lookup"><span data-stu-id="d8627-138">The database generates the `Id` when a `TodoItem` is created.</span></span>

### <a name="create-the-database-context"></a><span data-ttu-id="d8627-139">Erstellen des Datenbankkontexts</span><span class="sxs-lookup"><span data-stu-id="d8627-139">Create the database context</span></span>

<span data-ttu-id="d8627-140">Der *Datenbankkontext* ist die Hauptklasse, die die Entity Framework-Funktionen für ein angegebenes Datenmodell koordiniert.</span><span class="sxs-lookup"><span data-stu-id="d8627-140">The *database context* is the main class that coordinates Entity Framework functionality for a given data model.</span></span> <span data-ttu-id="d8627-141">Diese Klasse wird durch Ableiten von der `Microsoft.EntityFrameworkCore.DbContext`-Klasse erstellt.</span><span class="sxs-lookup"><span data-stu-id="d8627-141">This class is created by deriving from the `Microsoft.EntityFrameworkCore.DbContext` class.</span></span>

<span data-ttu-id="d8627-142">Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf den Ordner *Modelle*, und wählen Sie anschließend **Hinzufügen** > **Klasse** aus.</span><span class="sxs-lookup"><span data-stu-id="d8627-142">In Solution Explorer, right-click the *Models* folder and select **Add** > **Class**.</span></span> <span data-ttu-id="d8627-143">Nennen Sie die Klasse *TodoContext*, und klicken Sie auf **Hinzufügen**.</span><span class="sxs-lookup"><span data-stu-id="d8627-143">Name the class *TodoContext* and click **Add**.</span></span>

<span data-ttu-id="d8627-144">Ersetzen Sie die Klasse durch den folgenden Code:</span><span class="sxs-lookup"><span data-stu-id="d8627-144">Replace the class with the following code:</span></span>

[!code-csharp[](first-web-api/samples/2.0/TodoApi/Models/TodoContext.cs)]

[!INCLUDE[Register the database context](../includes/webApi/register_dbContext.md)]

### <a name="add-a-controller"></a><span data-ttu-id="d8627-145">Hinzufügen eines Controllers</span><span class="sxs-lookup"><span data-stu-id="d8627-145">Add a controller</span></span>

<span data-ttu-id="d8627-146">Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf den Ordner *Controller*.</span><span class="sxs-lookup"><span data-stu-id="d8627-146">In Solution Explorer, right-click the *Controllers* folder.</span></span> <span data-ttu-id="d8627-147">Klicken Sie auf **Hinzufügen** > **Neues Element**.</span><span class="sxs-lookup"><span data-stu-id="d8627-147">Select **Add** > **New Item**.</span></span> <span data-ttu-id="d8627-148">Wählen Sie im Dialogfeld **Neues Element hinzufügen** die Vorlage **API-Controllerklasse** aus.</span><span class="sxs-lookup"><span data-stu-id="d8627-148">In the **Add New Item** dialog, select the **API Controller Class** template.</span></span> <span data-ttu-id="d8627-149">Nennen Sie die Klasse *TodoController*, und klicken Sie auf **Hinzufügen**.</span><span class="sxs-lookup"><span data-stu-id="d8627-149">Name the class *TodoController*, and click **Add**.</span></span>

![Dialogfeld „Neues Element“ mit Controller im Suchfeld und ausgewähltem Web-API-Controller](first-web-api/_static/new_controller.png)

<span data-ttu-id="d8627-151">Ersetzen Sie die Klasse durch den folgenden Code:</span><span class="sxs-lookup"><span data-stu-id="d8627-151">Replace the class with the following code:</span></span>

[!INCLUDE[code and get todo items](../includes/webApi/getTodoItems.md)]

### <a name="launch-the-app"></a><span data-ttu-id="d8627-152">Starten der App</span><span class="sxs-lookup"><span data-stu-id="d8627-152">Launch the app</span></span>

<span data-ttu-id="d8627-153">Drücken Sie in Visual Studio STRG+F5 zum Starten der App.</span><span class="sxs-lookup"><span data-stu-id="d8627-153">In Visual Studio, press CTRL+F5 to launch the app.</span></span> <span data-ttu-id="d8627-154">Visual Studio startet einen Browser und navigiert zu `http://localhost:<port>/api/values`, wobei es sich bei `<port>` um eine zufällig ausgewählte Portnummer handelt.</span><span class="sxs-lookup"><span data-stu-id="d8627-154">Visual Studio launches a browser and navigates to `http://localhost:<port>/api/values`, where `<port>` is a randomly chosen port number.</span></span> <span data-ttu-id="d8627-155">Navigieren Sie zum `Todo`-Controller auf `http://localhost:<port>/api/todo`.</span><span class="sxs-lookup"><span data-stu-id="d8627-155">Navigate to the `Todo` controller at `http://localhost:<port>/api/todo`.</span></span>

[!INCLUDE[last part of web API](../includes/webApi/end.md)]

[!INCLUDE[jQuery](../includes/webApi/add-jquery.md)]

[!INCLUDE[next steps](../includes/webApi/next.md)]
