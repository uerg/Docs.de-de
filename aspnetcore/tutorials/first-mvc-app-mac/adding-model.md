---
title: "Hinzufügen eines Modells zu einer ASP.NET MVC Core-App"
author: rick-anderson
description: "Hinzufügen eines Modells zu einer einfachen ASP.NET Core-App"
keywords: "ASP.NET Core, MVC, Gerüst, Gerüstbau"
ms.author: riande
manager: wpickett
ms.date: 03/30/2017
ms.topic: get-started-article
ms.assetid: 8dc28498-eeee-1638-b903-b593059e9f39
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/first-mvc-app-mac/adding-model
ms.openlocfilehash: 4a158802a19011cbb45da1b3ca43d67706efe4cd
ms.sourcegitcommit: d9e2c99c837078fcac0e315025f8fbfbd45ea6e8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/28/2017
---
[!INCLUDE[adding-model](../../includes/mvc-intro/adding-model1.md)]

* <span data-ttu-id="499c9-104">Klicken Sie mit der rechten Maustaste auf den Ordner *Modelle*, und wählen Sie **Hinzufügen** > **Neue Datei** aus.</span><span class="sxs-lookup"><span data-stu-id="499c9-104">Right-click the *Models* folder, and then select **Add** > **New File**.</span></span> 
* <span data-ttu-id="499c9-105">Führen Sie im Dialogfeld **Neue Datei** folgende Aktionen aus:</span><span class="sxs-lookup"><span data-stu-id="499c9-105">In the **New File** dialog:</span></span>

  * <span data-ttu-id="499c9-106">Wählen sie im linken Bereich **Allgemein** aus.</span><span class="sxs-lookup"><span data-stu-id="499c9-106">Select **General** in the left pane.</span></span>
  * <span data-ttu-id="499c9-107">Wählen Sie im mittleren Bereich **Leere Klasse** aus.</span><span class="sxs-lookup"><span data-stu-id="499c9-107">Select **Empty Class** in the center pain.</span></span>
  * <span data-ttu-id="499c9-108">Geben Sie der Klasse den Namen **Film**, und wählen sie **Neu** aus.</span><span class="sxs-lookup"><span data-stu-id="499c9-108">Name the class **Movie** and select **New**.</span></span>

<span data-ttu-id="499c9-109">Fügen Sie der Klasse `Movie` die folgenden Eigenschaften hinzu:</span><span class="sxs-lookup"><span data-stu-id="499c9-109">Add the following properties to the `Movie` class:</span></span>

<span data-ttu-id="499c9-110">[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/MovieNoEF.cs?name=snippet_1)]</span><span class="sxs-lookup"><span data-stu-id="499c9-110">[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/MovieNoEF.cs?name=snippet_1)]</span></span>

<span data-ttu-id="499c9-111">Die Datenbank benötigt das Feld `ID` für den primären Schlüssel.</span><span class="sxs-lookup"><span data-stu-id="499c9-111">The `ID` field is required by the database for the primary key.</span></span>

<span data-ttu-id="499c9-112">Erstellen Sie das Projekt, um sicherzustellen, dass keine Fehler vorliegen.</span><span class="sxs-lookup"><span data-stu-id="499c9-112">Build the project to verify you don't have any errors.</span></span> <span data-ttu-id="499c9-113">Sie verfügen jetzt über ein **Modell** in Ihrer **MVC**-App.</span><span class="sxs-lookup"><span data-stu-id="499c9-113">You now have a **M**odel in your **M**VC app.</span></span>

## <a name="prepare-the-project-for-scaffolding"></a><span data-ttu-id="499c9-114">Vorbereiten des Projekts für den Gerüstbau</span><span class="sxs-lookup"><span data-stu-id="499c9-114">Prepare the project for scaffolding</span></span>

- <span data-ttu-id="499c9-115">Klicken Sie mit der rechten Maustaste auf die Projektdatei, und wählen Sie dann **Extras > Datei bearbeiten** aus.</span><span class="sxs-lookup"><span data-stu-id="499c9-115">Right click on the project file, and then select **Tools > Edit File**.</span></span>

  ![Screenshot für oben genannten Schritt](adding-model/_static/1.png)

- <span data-ttu-id="499c9-117">Fügen Sie die folgenden, hervorgehobenen NuGet-Pakete in die Datei *MvcMovie.csproj* ein:</span><span class="sxs-lookup"><span data-stu-id="499c9-117">Add the following highlighted NuGet packages to the *MvcMovie.csproj* file:</span></span>
             
  <span data-ttu-id="499c9-118">[!code-csharp[Main](../first-mvc-app-xplat/start-mvc/sample/MvcMovie/MvcMovie.csproj?highlight=7,10)]</span><span class="sxs-lookup"><span data-stu-id="499c9-118">[!code-csharp[Main](../first-mvc-app-xplat/start-mvc/sample/MvcMovie/MvcMovie.csproj?highlight=7,10)]</span></span>

- <span data-ttu-id="499c9-119">Speichern Sie die Datei.</span><span class="sxs-lookup"><span data-stu-id="499c9-119">Save the file.</span></span>

- <span data-ttu-id="499c9-120">Erstellen sie eine *Models/MvcMovieContext.cs*-Datei, und fügen Sie die folgende `MvcMovieContext`-Klasse hinzu: [!code-csharp[Main](../../tutorials/first-mvc-app-xplat/start-mvc/sample/MvcMovie/Models/MvcMovieContext.cs)]</span><span class="sxs-lookup"><span data-stu-id="499c9-120">Create a *Models/MvcMovieContext.cs* file and add the following `MvcMovieContext` class:  [!code-csharp[Main](../../tutorials/first-mvc-app-xplat/start-mvc/sample/MvcMovie/Models/MvcMovieContext.cs)]</span></span>
   
- <span data-ttu-id="499c9-121">Öffnen Sie die Datei *Startup.cs*, und fügen Sie zwei Usings hinzu: [!code-csharp[Main](../../tutorials/first-mvc-app-xplat/start-mvc/sample/MvcMovie/Startup.cs?name=snippet1&highlight=1,2)]</span><span class="sxs-lookup"><span data-stu-id="499c9-121">Open the *Startup.cs* file and add two usings:  [!code-csharp[Main](../../tutorials/first-mvc-app-xplat/start-mvc/sample/MvcMovie/Startup.cs?name=snippet1&highlight=1,2)]</span></span>

- <span data-ttu-id="499c9-122">Fügen Sie den Datenbankkontext zur Datei *Startup.cs* hinzu:</span><span class="sxs-lookup"><span data-stu-id="499c9-122">Add the database context to the *Startup.cs* file:</span></span>

   <span data-ttu-id="499c9-123">[!code-csharp[Main](../../tutorials/first-mvc-app-xplat/start-mvc/sample/MvcMovie/Startup.cs?name=snippet2&highlight=6-7)]</span><span class="sxs-lookup"><span data-stu-id="499c9-123">[!code-csharp[Main](../../tutorials/first-mvc-app-xplat/start-mvc/sample/MvcMovie/Startup.cs?name=snippet2&highlight=6-7)]</span></span>

  <span data-ttu-id="499c9-124">Damit weiß Entity Framework, welche Modellklassen im Datenmodell enthalten sind.</span><span class="sxs-lookup"><span data-stu-id="499c9-124">This tells Entity Framework which model classes are included in the data model.</span></span> <span data-ttu-id="499c9-125">Sie definieren eine *Entitätenmenge* von Filmobjekten, die in der Datenbank als Tabelle namens „Film“ dargestellt wird.</span><span class="sxs-lookup"><span data-stu-id="499c9-125">You're defining one *entity set* of Movie objects, which will be represented in the database as a Movie table.</span></span>

- <span data-ttu-id="499c9-126">Erstellen Sie das Projekt, um sicherzustellen, dass keine Fehler vorliegen.</span><span class="sxs-lookup"><span data-stu-id="499c9-126">Build the project to verify there are no errors.</span></span>

## <a name="scaffold-the-moviecontroller"></a><span data-ttu-id="499c9-127">Erstellen des Gerüsts für MovieController</span><span class="sxs-lookup"><span data-stu-id="499c9-127">Scaffold the MovieController</span></span>

<span data-ttu-id="499c9-128">Öffnen Sie im Projektordner ein Terminalfenster, und führen Sie die folgenden Befehle aus:</span><span class="sxs-lookup"><span data-stu-id="499c9-128">Open a terminal window in the project folder and run the following commands:</span></span>

```
dotnet restore
dotnet aspnet-codegenerator controller -name MoviesController -m Movie -dc MvcMovieContext --relativeFolderPath Controllers --useDefaultLayout --referenceScriptLibraries 
```
<span data-ttu-id="499c9-129">Wenn Sie die Fehlermeldung `No executable found matching command "dotnet-aspnet-codegenerator", verify` erhalten, trifft Folgendes zu:</span><span class="sxs-lookup"><span data-stu-id="499c9-129">If you get the error `No executable found matching command "dotnet-aspnet-codegenerator", verify`:</span></span>

 * <span data-ttu-id="499c9-130">Sie befinden sich im Projektverzeichnis.</span><span class="sxs-lookup"><span data-stu-id="499c9-130">You are in the project directory.</span></span> <span data-ttu-id="499c9-131">Hier befinden sich die Dateien *Program.cs*, *Startup.cs* und *.csproj*.</span><span class="sxs-lookup"><span data-stu-id="499c9-131">The project directory has the *Program.cs*, *Startup.cs* and *.csproj* files.</span></span>
 * <span data-ttu-id="499c9-132">Sie verwenden die dotnet-Version 1.1 oder höher.</span><span class="sxs-lookup"><span data-stu-id="499c9-132">Your dotnet version is 1.1 or higher.</span></span> <span data-ttu-id="499c9-133">Führen Sie `dotnet` aus, um Informationen über die Version zu erhalten.</span><span class="sxs-lookup"><span data-stu-id="499c9-133">Run `dotnet` to get the version.</span></span>
 * <span data-ttu-id="499c9-134">Sie haben der Datei [MvcMovie.csproj](#prepare-the-project-for-scaffolding) das Element `<DotNetCliToolReference>` hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="499c9-134">You have added the `<DotNetCliToolReference>` elment to the [MvcMovie.csproj file](#prepare-the-project-for-scaffolding).</span></span>
 
<!--
> [!NOTE]
> If you get an error when the scaffolding command runs, see [issue 444 in the scaffolding repository](https://github.com/aspnet/scaffolding/issues/444) for a workaround.
-->

<span data-ttu-id="499c9-135">Das Gerüstbaumodul erstellt Folgendes:</span><span class="sxs-lookup"><span data-stu-id="499c9-135">The scaffolding engine creates the following:</span></span>

* <span data-ttu-id="499c9-136">Einen Filmcontroller (*Controllers/MoviesController.cs*)</span><span class="sxs-lookup"><span data-stu-id="499c9-136">A movies controller (*Controllers/MoviesController.cs*)</span></span>
* <span data-ttu-id="499c9-137">Razor-Ansichtsdateien für Seiten der Typen „Erstellen“, „Löschen“, „Details“ und „Index“ (*Views/Movies/\*.cshtml*)</span><span class="sxs-lookup"><span data-stu-id="499c9-137">Razor view files for Create, Delete, Details, Edit and Index pages (*Views/Movies/\*.cshtml*)</span></span>

<span data-ttu-id="499c9-138">Die automatische Erstellung von [CRUD](https://en.wikipedia.org/wiki/Create,_read,_update_and_delete)-Aktionsmethoden und Ansichten (create, read update and delete – Erstellen, Lesen, Aktualisieren und Löschen) wird als *Gerüstbau* bezeichnet.</span><span class="sxs-lookup"><span data-stu-id="499c9-138">The automatic creation of [CRUD](https://en.wikipedia.org/wiki/Create,_read,_update_and_delete) (create, read, update, and delete) action methods and views is known as *scaffolding*.</span></span> <span data-ttu-id="499c9-139">Bald verfügen Sie über eine voll funktionsfähige Webanwendung, mit der Sie eine Filmdatenbank verwalten können.</span><span class="sxs-lookup"><span data-stu-id="499c9-139">You'll soon have a fully functional web application that lets you manage a movie database.</span></span>

### <a name="add-the-files-to-visual-studio"></a><span data-ttu-id="499c9-140">Hinzufügen der Dateien zu Visual Studio</span><span class="sxs-lookup"><span data-stu-id="499c9-140">Add the files to Visual Studio</span></span>

* <span data-ttu-id="499c9-141">Fügen Sie dem Visual Studio-Projekt die Datei *MovieController.cs* hinzu:</span><span class="sxs-lookup"><span data-stu-id="499c9-141">Add the *MovieController.cs* file to the Visual Studio project:</span></span>

  * <span data-ttu-id="499c9-142">Klicken Sie mit der rechten Maustaste auf den Ordner *Controller*, und wählen Sie **Hinzufügen > Dateien hinzufügen** aus.</span><span class="sxs-lookup"><span data-stu-id="499c9-142">Right-click on the *Controllers* folder and select **Add > Add Files**.</span></span>
  * <span data-ttu-id="499c9-143">Wählen Sie die Datei *MovieController.cs* aus.</span><span class="sxs-lookup"><span data-stu-id="499c9-143">Select the *MovieController.cs* file.</span></span>

* <span data-ttu-id="499c9-144">Fügen Sie den Ordner *Filme* und die Ansichten hinzu:</span><span class="sxs-lookup"><span data-stu-id="499c9-144">Add the *Movies* folder and views:</span></span>

  * <span data-ttu-id="499c9-145">Klicken Sie mit der rechten Maustaste auf den Ordner *Ansichten*, und wählen Sie **Hinzufügen > Vorhandenen Ordner hinzufügen** aus.</span><span class="sxs-lookup"><span data-stu-id="499c9-145">Right-click on the *Views* folder and select **Add > Add Existing Folder**.</span></span>
  * <span data-ttu-id="499c9-146">Navigieren Sie zum Ordner *Ansichten*, und wählen Sie *Ansichten\Filme* und anschließend **Öffnen** aus.</span><span class="sxs-lookup"><span data-stu-id="499c9-146">Navigate to the *Views* folder, select *Views\Movies*, and then select **Open**.</span></span>
  * <span data-ttu-id="499c9-147">Wählen Sie im Dialogfeld **Select files to add from Movies** (Aus „Filme“ hinzuzufügende Dateien auswählen) **Alle einschließen** aus, und klicken Sie dann auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="499c9-147">In the **Select files to add from Movies** dialog, select **Include All**, and then **OK**.</span></span>

[!INCLUDE[adding-model 2x](../../includes/mvc-intro/adding-model2xp.md)]

[!INCLUDE[adding-model](../../includes/mvc-intro/adding-model3.md)]

<span data-ttu-id="499c9-148">Sie verfügen jetzt über eine Datenbank und Seiten zum Anzeigen, Bearbeiten, Aktualisieren und Löschen von Daten.</span><span class="sxs-lookup"><span data-stu-id="499c9-148">You now have a database and pages to display, edit, update and delete data.</span></span> <span data-ttu-id="499c9-149">Im nächsten Tutorial werden wir mit dieser Datenbank arbeiten.</span><span class="sxs-lookup"><span data-stu-id="499c9-149">In the next tutorial, we'll work with the database.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="499c9-150">Zusätzliche Ressourcen</span><span class="sxs-lookup"><span data-stu-id="499c9-150">Additional resources</span></span>

* [<span data-ttu-id="499c9-151">Tag Helpers (Tag-Hilfsprogramme)</span><span class="sxs-lookup"><span data-stu-id="499c9-151">Tag Helpers</span></span>](xref:mvc/views/tag-helpers/intro)
* [<span data-ttu-id="499c9-152">Globalization and localization (Globalisierung und Lokalisierung)</span><span class="sxs-lookup"><span data-stu-id="499c9-152">Globalization and localization</span></span>](xref:fundamentals/localization)

>[!div class="step-by-step"]
<span data-ttu-id="499c9-153">[Vorheriges Tutorial: Adding a View (Hinzufügen einer Ansicht)](adding-view.md)
[Nächstes Tutorial: Working with SQL (Arbeiten mit SQL)](working-with-sql.md)</span><span class="sxs-lookup"><span data-stu-id="499c9-153">[Previous Adding a View](adding-view.md)
[Next Working with SQL](working-with-sql.md)</span></span>  
