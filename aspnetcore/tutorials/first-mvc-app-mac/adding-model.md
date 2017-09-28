---
title: "Hinzufügen eines Modells zu einer ASP.NET MVC Core-App"
author: rick-anderson
description: "Fügen Sie ein Modell zu einer einfachen ASP.NET Core-App hinzu."
keywords: "ASP.NET Core,MVC,Gerüst,Gerüstbau"
ms.author: riande
manager: wpickett
ms.devlang: csharp
ms.date: 09/22/2017
ms.topic: get-started-article
ms.assetid: 8dc28498-eeee-1638-b903-b593059e9f39
ms.technology: aspnet
ms.prod: .net-core
uid: tutorials/first-mvc-app-mac/adding-model
ms.openlocfilehash: 0e2c47c7d9903d6d300fa293dd0530a2f65f77d8
ms.sourcegitcommit: e6bcd56a4b11e20ff55df004971f9ed384937342
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/22/2017
---
[!INCLUDE[adding-model](../../includes/mvc-intro/adding-model1.md)]

* <span data-ttu-id="abd24-104">Klicken Sie mit der rechten Maustaste auf den Ordner *Modelle*, und klicken Sie auf **Hinzufügen** > **Neue Datei**.</span><span class="sxs-lookup"><span data-stu-id="abd24-104">Right-click the *Models* folder, and then select **Add** > **New File**.</span></span> 
* <span data-ttu-id="abd24-105">Führen Sie im Dialogfeld **Neue Datei** folgende Aktionen aus:</span><span class="sxs-lookup"><span data-stu-id="abd24-105">In the **New File** dialog:</span></span>

  * <span data-ttu-id="abd24-106">Klicken Sie im linken Bereich auf **Allgemein**.</span><span class="sxs-lookup"><span data-stu-id="abd24-106">Select **General** in the left pane.</span></span>
  * <span data-ttu-id="abd24-107">Klicken Sie im mittleren Bereich auf **Leere Klasse**.</span><span class="sxs-lookup"><span data-stu-id="abd24-107">Select **Empty Class** in the center pain.</span></span>
  * <span data-ttu-id="abd24-108">Geben Sie der Klasse den Namen **Film**, und wählen sie **Neu** aus.</span><span class="sxs-lookup"><span data-stu-id="abd24-108">Name the class **Movie** and select **New**.</span></span>

<span data-ttu-id="abd24-109">Fügen Sie der Klasse `Movie` die folgenden Eigenschaften hinzu:</span><span class="sxs-lookup"><span data-stu-id="abd24-109">Add the following properties to the `Movie` class:</span></span>

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/MovieNoEF.cs?name=snippet_1)]

<span data-ttu-id="abd24-110">Die Datenbank benötigt das Feld `ID` für den primären Schlüssel.</span><span class="sxs-lookup"><span data-stu-id="abd24-110">The `ID` field is required by the database for the primary key.</span></span>

<span data-ttu-id="abd24-111">Erstellen Sie das Projekt, um sicherzustellen, dass keine Fehler vorliegen.</span><span class="sxs-lookup"><span data-stu-id="abd24-111">Build the project to verify you don't have any errors.</span></span> <span data-ttu-id="abd24-112">Sie verfügen jetzt über ein **Modell** in Ihrer **MVC**-App.</span><span class="sxs-lookup"><span data-stu-id="abd24-112">You now have a **M**odel in your **M**VC app.</span></span>

## <a name="prepare-the-project-for-scaffolding"></a><span data-ttu-id="abd24-113">Vorbereiten des Projekts für den Gerüstbau</span><span class="sxs-lookup"><span data-stu-id="abd24-113">Prepare the project for scaffolding</span></span>

- <span data-ttu-id="abd24-114">Klicken Sie mit der rechten Maustaste auf die Projektdatei, und wählen Sie dann **Extras > Datei bearbeiten** aus.</span><span class="sxs-lookup"><span data-stu-id="abd24-114">Right click on the project file, and then select **Tools > Edit File**.</span></span>

  ![Screenshot für oben genannten Schritt](adding-model/_static/1.png)

- <span data-ttu-id="abd24-116">Fügen Sie die folgenden hervorgehobenen NuGet-Pakete in die Datei *MvcMovie.csproj* ein:</span><span class="sxs-lookup"><span data-stu-id="abd24-116">Add the following highlighted NuGet packages to the *MvcMovie.csproj* file:</span></span>
             
  [!code-csharp[Main](../first-mvc-app-xplat/start-mvc/sample/MvcMovie/MvcMovie.csproj?highlight=7,10)]

- <span data-ttu-id="abd24-117">Speichern Sie die Datei.</span><span class="sxs-lookup"><span data-stu-id="abd24-117">Save the file.</span></span>

- <span data-ttu-id="abd24-118">Erstellen sie eine *Models/MvcMovieContext.cs*-Datei, und fügen Sie die folgende Klasse des Typs `MvcMovieContext` hinzu: [!code-csharp[Main](../../tutorials/first-mvc-app-xplat/start-mvc/sample/MvcMovie/Models/MvcMovieContext.cs)]</span><span class="sxs-lookup"><span data-stu-id="abd24-118">Create a *Models/MvcMovieContext.cs* file and add the following `MvcMovieContext` class:  [!code-csharp[Main](../../tutorials/first-mvc-app-xplat/start-mvc/sample/MvcMovie/Models/MvcMovieContext.cs)]</span></span>
   
- <span data-ttu-id="abd24-119">Öffnen Sie die Datei *Startup.cs*, und fügen Sie zwei Usings hinzu: [!code-csharp[Main](../../tutorials/first-mvc-app-xplat/start-mvc/sample/MvcMovie/Startup.cs?name=snippet1&highlight=1,2)]</span><span class="sxs-lookup"><span data-stu-id="abd24-119">Open the *Startup.cs* file and add two usings:  [!code-csharp[Main](../../tutorials/first-mvc-app-xplat/start-mvc/sample/MvcMovie/Startup.cs?name=snippet1&highlight=1,2)]</span></span>

- <span data-ttu-id="abd24-120">Fügen Sie den Datenbankkontext zur Datei *Startup.cs* hinzu:</span><span class="sxs-lookup"><span data-stu-id="abd24-120">Add the database context to the *Startup.cs* file:</span></span>

   [!code-csharp[Main](../../tutorials/first-mvc-app-xplat/start-mvc/sample/MvcMovie/Startup.cs?name=snippet2&highlight=6-7)]

  <span data-ttu-id="abd24-121">Dadurch weiß das Entity Framework, welche Modellklassen im Datenmodell enthalten sind.</span><span class="sxs-lookup"><span data-stu-id="abd24-121">This tells Entity Framework which model classes are included in the data model.</span></span> <span data-ttu-id="abd24-122">Sie definieren eine *Entitätenmenge* von Filmobjekten, die in der Datenbank als Tabelle namens „Film“ dargestellt wird.</span><span class="sxs-lookup"><span data-stu-id="abd24-122">You're defining one *entity set* of Movie objects, which will be represented in the database as a Movie table.</span></span>

- <span data-ttu-id="abd24-123">Erstellen Sie das Projekt, um sicherzustellen, dass keine Fehler vorliegen.</span><span class="sxs-lookup"><span data-stu-id="abd24-123">Build the project to verify there are no errors.</span></span>

## <a name="scaffold-the-moviecontroller"></a><span data-ttu-id="abd24-124">Erstellen des Gerüsts für „MovieController“</span><span class="sxs-lookup"><span data-stu-id="abd24-124">Scaffold the MovieController</span></span>

<span data-ttu-id="abd24-125">Öffnen Sie im Projektordner ein Terminalfenster, und führen Sie die folgenden Befehle aus:</span><span class="sxs-lookup"><span data-stu-id="abd24-125">Open a terminal window in the project folder and run the following commands:</span></span>

```
dotnet restore
dotnet aspnet-codegenerator controller -name MoviesController -m Movie -dc MvcMovieContext --relativeFolderPath Controllers --useDefaultLayout --referenceScriptLibraries 
```
<span data-ttu-id="abd24-126">Wenn Sie die Fehlermeldung `No executable found matching command "dotnet-aspnet-codegenerator", verify` erhalten, trifft Folgendes zu:</span><span class="sxs-lookup"><span data-stu-id="abd24-126">If you get the error `No executable found matching command "dotnet-aspnet-codegenerator", verify`:</span></span>

 * <span data-ttu-id="abd24-127">Sie befinden sich im Projektverzeichnis.</span><span class="sxs-lookup"><span data-stu-id="abd24-127">You are in the project directory.</span></span> <span data-ttu-id="abd24-128">Hier befinden sich die Dateien *Program.cs*, *Startup.cs* und *.csproj*.</span><span class="sxs-lookup"><span data-stu-id="abd24-128">The project directory has the *Program.cs*, *Startup.cs* and *.csproj* files.</span></span>
 * <span data-ttu-id="abd24-129">Sie verwenden die dotnet-Version 1.1 oder höher.</span><span class="sxs-lookup"><span data-stu-id="abd24-129">Your dotnet version is 1.1 or higher.</span></span> <span data-ttu-id="abd24-130">Führen Sie `dotnet` aus, um Informationen über die Version zu erhalten.</span><span class="sxs-lookup"><span data-stu-id="abd24-130">Run `dotnet` to get the version.</span></span>
 * <span data-ttu-id="abd24-131">Sie haben der Datei [MvcMovie.csproj](#prepare-the-project-for-scaffolding) das Element `<DotNetCliToolReference>` hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="abd24-131">You have added the `<DotNetCliToolReference>` elment to the [MvcMovie.csproj file](#prepare-the-project-for-scaffolding).</span></span>
 
<!--
> [!NOTE]
> If you get an error when the scaffolding command runs, see [issue 444 in the scaffolding repository](https://github.com/aspnet/scaffolding/issues/444) for a workaround.
-->

<span data-ttu-id="abd24-132">Das Gerüstbaumodul erstellt Folgendes:</span><span class="sxs-lookup"><span data-stu-id="abd24-132">The scaffolding engine creates the following:</span></span>

* <span data-ttu-id="abd24-133">Einen Filmcontroller (*Controllers/MoviesController.cs*)</span><span class="sxs-lookup"><span data-stu-id="abd24-133">A movies controller (*Controllers/MoviesController.cs*)</span></span>
* <span data-ttu-id="abd24-134">Razor-Ansichtsdateien für Seiten der Typen „Erstellen“, „Löschen“, „Details“ und „Index“ (*Views/Movies/\*.cshtml*)</span><span class="sxs-lookup"><span data-stu-id="abd24-134">Razor view files for Create, Delete, Details, Edit and Index pages (*Views/Movies/\*.cshtml*)</span></span>

<span data-ttu-id="abd24-135">Die automatische Erstellung von [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete)-Aktionsmethoden und Ansichten (create, read update and delete – Erstellen, Lesen, Aktualisieren und Löschen) wird als *Gerüstbau* bezeichnet.</span><span class="sxs-lookup"><span data-stu-id="abd24-135">The automatic creation of [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) (create, read, update, and delete) action methods and views is known as *scaffolding*.</span></span> <span data-ttu-id="abd24-136">Bald verfügen Sie über eine voll funktionsfähige Webanwendung, mit der Sie eine Filmdatenbank verwalten können.</span><span class="sxs-lookup"><span data-stu-id="abd24-136">You'll soon have a fully functional web application that lets you manage a movie database.</span></span>

### <a name="add-the-files-to-visual-studio"></a><span data-ttu-id="abd24-137">Hinzufügen der Dateien zu Visual Studio</span><span class="sxs-lookup"><span data-stu-id="abd24-137">Add the files to Visual Studio</span></span>

* <span data-ttu-id="abd24-138">Fügen Sie dem Visual Studio-Projekt die Datei *MovieController.cs* hinzu:</span><span class="sxs-lookup"><span data-stu-id="abd24-138">Add the *MovieController.cs* file to the Visual Studio project:</span></span>

  * <span data-ttu-id="abd24-139">Klicken Sie mit der rechten Maustaste auf den Ordner *Controller*, und wählen Sie **Hinzufügen > Dateien hinzufügen** aus.</span><span class="sxs-lookup"><span data-stu-id="abd24-139">Right-click on the *Controllers* folder and select **Add > Add Files**.</span></span>
  * <span data-ttu-id="abd24-140">Wählen Sie die Datei *MovieController.cs* aus.</span><span class="sxs-lookup"><span data-stu-id="abd24-140">Select the *MovieController.cs* file.</span></span>

* <span data-ttu-id="abd24-141">Fügen Sie den Ordner *Filme* und die Ansichten hinzu:</span><span class="sxs-lookup"><span data-stu-id="abd24-141">Add the *Movies* folder and views:</span></span>

  * <span data-ttu-id="abd24-142">Klicken Sie mit der rechten Maustaste auf den Ordner *Ansichten*, und wählen Sie **Hinzufügen > Vorhandenen Ordner hinzufügen** aus.</span><span class="sxs-lookup"><span data-stu-id="abd24-142">Right-click on the *Views* folder and select **Add > Add Existing Folder**.</span></span>
  * <span data-ttu-id="abd24-143">Navigieren Sie zum Ordner *Ansichten*, und wählen Sie *Ansichten\Filme* und anschließend **Öffnen** aus.</span><span class="sxs-lookup"><span data-stu-id="abd24-143">Navigate to the *Views* folder, select *Views\Movies*, and then select **Open**.</span></span>
  * <span data-ttu-id="abd24-144">Wählen Sie im Dialogfeld **Select files to add from Movies** (Aus „Filme“ hinzuzufügende Dateien auswählen) **Alle einschließen** aus, und klicken Sie dann auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="abd24-144">In the **Select files to add from Movies** dialog, select **Include All**, and then **OK**.</span></span>

[!INCLUDE[adding-model 2x](../../includes/mvc-intro/adding-model2xp.md)]

[!INCLUDE[adding-model](../../includes/mvc-intro/adding-model3.md)]

<span data-ttu-id="abd24-145">Sie verfügen jetzt über eine Datenbank und Seiten zum Anzeigen, Bearbeiten, Aktualisieren und Löschen von Daten.</span><span class="sxs-lookup"><span data-stu-id="abd24-145">You now have a database and pages to display, edit, update and delete data.</span></span> <span data-ttu-id="abd24-146">Im nächsten Tutorial werden wir mit dieser Datenbank arbeiten.</span><span class="sxs-lookup"><span data-stu-id="abd24-146">In the next tutorial, we'll work with the database.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="abd24-147">Zusätzliche Ressourcen</span><span class="sxs-lookup"><span data-stu-id="abd24-147">Additional resources</span></span>

* [<span data-ttu-id="abd24-148">Taghilfsprogramme</span><span class="sxs-lookup"><span data-stu-id="abd24-148">Tag Helpers</span></span>](xref:mvc/views/tag-helpers/intro)
* [<span data-ttu-id="abd24-149">Globalisierung und Lokalisierung</span><span class="sxs-lookup"><span data-stu-id="abd24-149">Globalization and localization</span></span>](xref:fundamentals/localization)

>[!div class="step-by-step"]
<span data-ttu-id="abd24-150">[Vorheriges Tutorial: Hinzufügen einer Ansicht](adding-view.md)
[Nächstes Tutorial: Arbeiten mit SQL](working-with-sql.md)</span><span class="sxs-lookup"><span data-stu-id="abd24-150">[Previous Adding a View](adding-view.md)
[Next Working with SQL](working-with-sql.md)</span></span>  
