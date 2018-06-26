---
title: Erstellen einer Web-API mit ASP.NET Core und Visual Studio für Windows
author: rick-anderson
description: Erstellen einer Web-API mit ASP.NET Core MVC und Visual Studio für Windows
ms.author: riande
ms.custom: mvc
ms.date: 05/17/2018
uid: tutorials/first-web-api
ms.openlocfilehash: 3da22cbbbe0db7771656997a13587521e182fb2a
ms.sourcegitcommit: 79b756ea03eae77a716f500ef88253ee9b1464d2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/25/2018
ms.locfileid: "36277399"
---
# <a name="create-a-web-api-with-aspnet-core-and-visual-studio-for-windows"></a><span data-ttu-id="99357-103">Erstellen einer Web-API mit ASP.NET Core und Visual Studio für Windows</span><span class="sxs-lookup"><span data-stu-id="99357-103">Create a Web API with ASP.NET Core and Visual Studio for Windows</span></span>

<span data-ttu-id="99357-104">Von [Rick Anderson](https://twitter.com/RickAndMSFT) und [Mike Wasson](https://github.com/mikewasson)</span><span class="sxs-lookup"><span data-stu-id="99357-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Mike Wasson](https://github.com/mikewasson)</span></span>

<span data-ttu-id="99357-105">In diesem Tutorial wird eine Web-API zum Verwalten einer Liste von Aufgabenelementen erstellt.</span><span class="sxs-lookup"><span data-stu-id="99357-105">This tutorial builds a web API for managing a list of "to-do" items.</span></span> <span data-ttu-id="99357-106">Es wird keine Benutzeroberfläche (UI) erstellt.</span><span class="sxs-lookup"><span data-stu-id="99357-106">A user interface (UI) isn't created.</span></span>

<span data-ttu-id="99357-107">Es gibt drei Versionen dieses Tutorials:</span><span class="sxs-lookup"><span data-stu-id="99357-107">There are three versions of this tutorial:</span></span>

* <span data-ttu-id="99357-108">Windows: Web-API mit Visual Studio für Windows (dieses Tutorial)</span><span class="sxs-lookup"><span data-stu-id="99357-108">Windows: Web API with Visual Studio for Windows (This tutorial)</span></span>
* <span data-ttu-id="99357-109">macOS: [Web-API mit Visual Studio für Mac](xref:tutorials/first-web-api-mac)</span><span class="sxs-lookup"><span data-stu-id="99357-109">macOS: [Web API with Visual Studio for Mac](xref:tutorials/first-web-api-mac)</span></span>
* <span data-ttu-id="99357-110">macOS, Linux, Windows: [Web-API mit Visual Studio Code](xref:tutorials/web-api-vsc)</span><span class="sxs-lookup"><span data-stu-id="99357-110">macOS, Linux, Windows: [Web API with Visual Studio Code](xref:tutorials/web-api-vsc)</span></span>

<!-- WARNING: The code AND images in this doc are used by uid: tutorials/web-api-vsc, tutorials/first-web-api-mac and tutorials/first-web-api. If you change any code/images in this tutorial, update uid: tutorials/web-api-vsc -->

[!INCLUDE[intro to web API](../includes/webApi/intro.md)]

## <a name="prerequisites"></a><span data-ttu-id="99357-111">Erforderliche Komponenten</span><span class="sxs-lookup"><span data-stu-id="99357-111">Prerequisites</span></span>

[!INCLUDE[](~/includes/net-core-prereqs-windows.md)]

## <a name="create-the-project"></a><span data-ttu-id="99357-112">Erstellen eines Projekts</span><span class="sxs-lookup"><span data-stu-id="99357-112">Create the project</span></span>

<span data-ttu-id="99357-113">Führen Sie in Visual Studio die folgenden Schritte aus:</span><span class="sxs-lookup"><span data-stu-id="99357-113">Follow these steps in Visual Studio:</span></span>

* <span data-ttu-id="99357-114">Wählen Sie im Menü **Datei** den Befehl **Neu** > **Projekt** aus.</span><span class="sxs-lookup"><span data-stu-id="99357-114">From the **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="99357-115">Wählen Sie die Vorlage **ASP.NET Core-Webanwendung** aus.</span><span class="sxs-lookup"><span data-stu-id="99357-115">Select the **ASP.NET Core Web Application** template.</span></span> <span data-ttu-id="99357-116">Geben Sie dem Projekt den Namen *TodoApi*, und klicken Sie auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="99357-116">Name the project *TodoApi* and click **OK**.</span></span>
* <span data-ttu-id="99357-117">Wählen Sie im Dialogfeld **ASP.NET Core-Webanwendung – TodoAPI** die ASP.NET Core-Version aus.</span><span class="sxs-lookup"><span data-stu-id="99357-117">In the **New ASP.NET Core Web Application - TodoApi** dialog, choose the ASP.NET Core version.</span></span> <span data-ttu-id="99357-118">Wählen Sie die Vorlage **API** aus, und klicken Sie auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="99357-118">Select the **API** template and click **OK**.</span></span> <span data-ttu-id="99357-119">Wählen Sie **nicht** **Enable Docker Support** (Docker-Unterstützung aktivieren) aus.</span><span class="sxs-lookup"><span data-stu-id="99357-119">Do **not** select **Enable Docker Support**.</span></span>

### <a name="launch-the-app"></a><span data-ttu-id="99357-120">Starten der App</span><span class="sxs-lookup"><span data-stu-id="99357-120">Launch the app</span></span>

<span data-ttu-id="99357-121">Drücken Sie in Visual Studio STRG+F5 zum Starten der App.</span><span class="sxs-lookup"><span data-stu-id="99357-121">In Visual Studio, press CTRL+F5 to launch the app.</span></span> <span data-ttu-id="99357-122">Visual Studio startet einen Browser und navigiert zu `http://localhost:<port>/api/values`, wobei es sich bei `<port>` um eine zufällig ausgewählte Portnummer handelt.</span><span class="sxs-lookup"><span data-stu-id="99357-122">Visual Studio launches a browser and navigates to `http://localhost:<port>/api/values`, where `<port>` is a randomly chosen port number.</span></span> <span data-ttu-id="99357-123">Chrome, Microsoft Edge und Firefox zeigen die folgende Ausgabe an:</span><span class="sxs-lookup"><span data-stu-id="99357-123">Chrome, Microsoft Edge, and Firefox display the following output:</span></span>

```json
["value1","value2"]
```

<span data-ttu-id="99357-124">Wenn Sie Internet Explorer verwenden, werden Sie dazu aufgefordert, eine *values.json*-Datei zu speichern.</span><span class="sxs-lookup"><span data-stu-id="99357-124">If using Internet Explorer, you'll be prompted to save a *values.json* file.</span></span>

### <a name="add-a-model-class"></a><span data-ttu-id="99357-125">Hinzufügen einer Modellklasse</span><span class="sxs-lookup"><span data-stu-id="99357-125">Add a model class</span></span>

<span data-ttu-id="99357-126">Ein Modell ist ein Objekt, das die Daten in der App darstellt.</span><span class="sxs-lookup"><span data-stu-id="99357-126">A model is an object representing the data in the app.</span></span> <span data-ttu-id="99357-127">In diesem Fall ist das einzige Modell ein To-do-Element.</span><span class="sxs-lookup"><span data-stu-id="99357-127">In this case, the only model is a to-do item.</span></span>

<span data-ttu-id="99357-128">Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf den Projektnamen.</span><span class="sxs-lookup"><span data-stu-id="99357-128">In Solution Explorer, right-click the project.</span></span> <span data-ttu-id="99357-129">Klicken Sie auf **Hinzufügen** > **Neuer Ordner**.</span><span class="sxs-lookup"><span data-stu-id="99357-129">Select **Add** > **New Folder**.</span></span> <span data-ttu-id="99357-130">Geben Sie dem Ordner den Namen *Modelle*.</span><span class="sxs-lookup"><span data-stu-id="99357-130">Name the folder *Models*.</span></span>

> [!NOTE]
> <span data-ttu-id="99357-131">Die Modellklassen können überall im Projekt gespeichert werden.</span><span class="sxs-lookup"><span data-stu-id="99357-131">The model classes can go anywhere in the project.</span></span> <span data-ttu-id="99357-132">Der Ordner *Modelle* wird gemäß Konvention für Modellklassen verwendet.</span><span class="sxs-lookup"><span data-stu-id="99357-132">The *Models* folder is used by convention for model classes.</span></span>

<span data-ttu-id="99357-133">Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf den Ordner *Modelle*, und wählen Sie anschließend **Hinzufügen** > **Klasse** aus.</span><span class="sxs-lookup"><span data-stu-id="99357-133">In Solution Explorer, right-click the *Models* folder and select **Add** > **Class**.</span></span> <span data-ttu-id="99357-134">Nennen Sie die Klasse *TodoItem*, und klicken Sie auf **Hinzufügen**.</span><span class="sxs-lookup"><span data-stu-id="99357-134">Name the class *TodoItem* and click **Add**.</span></span>

<span data-ttu-id="99357-135">Aktualisieren Sie die `TodoItem`-Klasse mit dem folgenden Code:</span><span class="sxs-lookup"><span data-stu-id="99357-135">Update the `TodoItem` class with the following code:</span></span>

[!code-csharp[](first-web-api/samples/2.0/TodoApi/Models/TodoItem.cs)]

<span data-ttu-id="99357-136">Die Datenbank generiert die `Id`, wenn ein `TodoItem` erstellt wird.</span><span class="sxs-lookup"><span data-stu-id="99357-136">The database generates the `Id` when a `TodoItem` is created.</span></span>

### <a name="create-the-database-context"></a><span data-ttu-id="99357-137">Erstellen des Datenbankkontexts</span><span class="sxs-lookup"><span data-stu-id="99357-137">Create the database context</span></span>

<span data-ttu-id="99357-138">Der *Datenbankkontext* ist die Hauptklasse, die die Entity Framework-Funktionen für ein angegebenes Datenmodell koordiniert.</span><span class="sxs-lookup"><span data-stu-id="99357-138">The *database context* is the main class that coordinates Entity Framework functionality for a given data model.</span></span> <span data-ttu-id="99357-139">Diese Klasse wird durch Ableiten von der `Microsoft.EntityFrameworkCore.DbContext`-Klasse erstellt.</span><span class="sxs-lookup"><span data-stu-id="99357-139">This class is created by deriving from the `Microsoft.EntityFrameworkCore.DbContext` class.</span></span>

<span data-ttu-id="99357-140">Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf den Ordner *Modelle*, und wählen Sie anschließend **Hinzufügen** > **Klasse** aus.</span><span class="sxs-lookup"><span data-stu-id="99357-140">In Solution Explorer, right-click the *Models* folder and select **Add** > **Class**.</span></span> <span data-ttu-id="99357-141">Nennen Sie die Klasse *TodoContext*, und klicken Sie auf **Hinzufügen**.</span><span class="sxs-lookup"><span data-stu-id="99357-141">Name the class *TodoContext* and click **Add**.</span></span>

<span data-ttu-id="99357-142">Ersetzen Sie die Klasse durch den folgenden Code:</span><span class="sxs-lookup"><span data-stu-id="99357-142">Replace the class with the following code:</span></span>

[!code-csharp[](first-web-api/samples/2.0/TodoApi/Models/TodoContext.cs)]

[!INCLUDE[Register the database context](../includes/webApi/register_dbContext.md)]

### <a name="add-a-controller"></a><span data-ttu-id="99357-143">Hinzufügen eines Controllers</span><span class="sxs-lookup"><span data-stu-id="99357-143">Add a controller</span></span>

<span data-ttu-id="99357-144">Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf den Ordner *Controller*.</span><span class="sxs-lookup"><span data-stu-id="99357-144">In Solution Explorer, right-click the *Controllers* folder.</span></span> <span data-ttu-id="99357-145">Klicken Sie auf **Hinzufügen** > **Neues Element**.</span><span class="sxs-lookup"><span data-stu-id="99357-145">Select **Add** > **New Item**.</span></span> <span data-ttu-id="99357-146">Wählen Sie im Dialogfeld **Neues Element hinzufügen** die Vorlage **API-Controllerklasse** aus.</span><span class="sxs-lookup"><span data-stu-id="99357-146">In the **Add New Item** dialog, select the **API Controller Class** template.</span></span> <span data-ttu-id="99357-147">Nennen Sie die Klasse *TodoController*, und klicken Sie auf **Hinzufügen**.</span><span class="sxs-lookup"><span data-stu-id="99357-147">Name the class *TodoController*, and click **Add**.</span></span>

![Dialogfeld „Neues Element“ mit Controller im Suchfeld und ausgewähltem Web-API-Controller](first-web-api/_static/new_controller.png)

<span data-ttu-id="99357-149">Ersetzen Sie die Klasse durch den folgenden Code:</span><span class="sxs-lookup"><span data-stu-id="99357-149">Replace the class with the following code:</span></span>

[!INCLUDE[code and get todo items](../includes/webApi/getTodoItems.md)]

### <a name="launch-the-app"></a><span data-ttu-id="99357-150">Starten der App</span><span class="sxs-lookup"><span data-stu-id="99357-150">Launch the app</span></span>

<span data-ttu-id="99357-151">Drücken Sie in Visual Studio STRG+F5 zum Starten der App.</span><span class="sxs-lookup"><span data-stu-id="99357-151">In Visual Studio, press CTRL+F5 to launch the app.</span></span> <span data-ttu-id="99357-152">Visual Studio startet einen Browser und navigiert zu `http://localhost:<port>/api/values`, wobei es sich bei `<port>` um eine zufällig ausgewählte Portnummer handelt.</span><span class="sxs-lookup"><span data-stu-id="99357-152">Visual Studio launches a browser and navigates to `http://localhost:<port>/api/values`, where `<port>` is a randomly chosen port number.</span></span> <span data-ttu-id="99357-153">Navigieren Sie zum `Todo`-Controller auf `http://localhost:<port>/api/todo`.</span><span class="sxs-lookup"><span data-stu-id="99357-153">Navigate to the `Todo` controller at `http://localhost:<port>/api/todo`.</span></span>

[!INCLUDE[last part of web API](../includes/webApi/end.md)]

[!INCLUDE[jQuery](../includes/webApi/add-jquery.md)]

[!INCLUDE[next steps](../includes/webApi/next.md)]
