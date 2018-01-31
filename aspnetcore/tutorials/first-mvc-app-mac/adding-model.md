---
title: "Hinzufügen eines Modells zu einer ASP.NET Core MVC-App"
author: rick-anderson
description: "Fügen Sie ein Modell zu einer einfachen ASP.NET Core-App hinzu."
manager: wpickett
ms.author: riande
ms.date: 09/22/2017
ms.devlang: csharp
ms.prod: .net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/first-mvc-app-mac/adding-model
ms.openlocfilehash: bf4d5d289266b585cbdfbb70c7482620fd4ced54
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2018
---
[!INCLUDE[adding-model](../../includes/mvc-intro/adding-model1.md)]

* <span data-ttu-id="7a2db-103">Klicken Sie mit der rechten Maustaste auf den Ordner *Modelle*, und klicken Sie auf **Hinzufügen** > **Neue Datei**.</span><span class="sxs-lookup"><span data-stu-id="7a2db-103">Right-click the *Models* folder, and then select **Add** > **New File**.</span></span> 
* <span data-ttu-id="7a2db-104">Führen Sie im Dialogfeld **Neue Datei** folgende Aktionen aus:</span><span class="sxs-lookup"><span data-stu-id="7a2db-104">In the **New File** dialog:</span></span>

  * <span data-ttu-id="7a2db-105">Klicken Sie im linken Bereich auf **Allgemein**.</span><span class="sxs-lookup"><span data-stu-id="7a2db-105">Select **General** in the left pane.</span></span>
  * <span data-ttu-id="7a2db-106">Klicken Sie im mittleren Bereich auf **Leere Klasse**.</span><span class="sxs-lookup"><span data-stu-id="7a2db-106">Select **Empty Class** in the center pain.</span></span>
  * <span data-ttu-id="7a2db-107">Geben Sie der Klasse den Namen **Film**, und wählen sie **Neu** aus.</span><span class="sxs-lookup"><span data-stu-id="7a2db-107">Name the class **Movie** and select **New**.</span></span>

<span data-ttu-id="7a2db-108">Fügen Sie der Klasse `Movie` die folgenden Eigenschaften hinzu:</span><span class="sxs-lookup"><span data-stu-id="7a2db-108">Add the following properties to the `Movie` class:</span></span>

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/MovieNoEF.cs?name=snippet_1)]

<span data-ttu-id="7a2db-109">Die Datenbank benötigt das Feld `ID` für den primären Schlüssel.</span><span class="sxs-lookup"><span data-stu-id="7a2db-109">The `ID` field is required by the database for the primary key.</span></span>

<span data-ttu-id="7a2db-110">Erstellen Sie das Projekt, um sicherzustellen, dass keine Fehler vorliegen.</span><span class="sxs-lookup"><span data-stu-id="7a2db-110">Build the project to verify you don't have any errors.</span></span> <span data-ttu-id="7a2db-111">Sie verfügen jetzt über ein **Modell** in Ihrer **MVC**-App.</span><span class="sxs-lookup"><span data-stu-id="7a2db-111">You now have a **M**odel in your **M**VC app.</span></span>

## <a name="prepare-the-project-for-scaffolding"></a><span data-ttu-id="7a2db-112">Vorbereiten des Projekts für den Gerüstbau</span><span class="sxs-lookup"><span data-stu-id="7a2db-112">Prepare the project for scaffolding</span></span>

- <span data-ttu-id="7a2db-113">Klicken Sie mit der rechten Maustaste auf die Projektdatei, und wählen Sie dann **Extras > Datei bearbeiten** aus.</span><span class="sxs-lookup"><span data-stu-id="7a2db-113">Right click on the project file, and then select **Tools > Edit File**.</span></span>

  ![Screenshot für oben genannten Schritt](adding-model/_static/1.png)

- <span data-ttu-id="7a2db-115">Fügen Sie die folgenden hervorgehobenen NuGet-Pakete in die Datei *MvcMovie.csproj* ein:</span><span class="sxs-lookup"><span data-stu-id="7a2db-115">Add the following highlighted NuGet packages to the *MvcMovie.csproj* file:</span></span>
             
  [!code-csharp[Main](../first-mvc-app-xplat/start-mvc/sample/MvcMovie/MvcMovie.csproj?highlight=7,10)]

- <span data-ttu-id="7a2db-116">Speichern Sie die Datei.</span><span class="sxs-lookup"><span data-stu-id="7a2db-116">Save the file.</span></span>

- <span data-ttu-id="7a2db-117">Erstellen sie eine *Models/MvcMovieContext.cs*-Datei, und fügen Sie die folgende Klasse des Typs `MvcMovieContext` hinzu: [!code-csharp[Main](../../tutorials/first-mvc-app-xplat/start-mvc/sample/MvcMovie/Models/MvcMovieContext.cs)]</span><span class="sxs-lookup"><span data-stu-id="7a2db-117">Create a *Models/MvcMovieContext.cs* file and add the following `MvcMovieContext` class:  [!code-csharp[Main](../../tutorials/first-mvc-app-xplat/start-mvc/sample/MvcMovie/Models/MvcMovieContext.cs)]</span></span>
   
- <span data-ttu-id="7a2db-118">Öffnen Sie die Datei *Startup.cs*, und fügen Sie zwei Usings hinzu: [!code-csharp[Main](../../tutorials/first-mvc-app-xplat/start-mvc/sample/MvcMovie/Startup.cs?name=snippet1&highlight=1,2)]</span><span class="sxs-lookup"><span data-stu-id="7a2db-118">Open the *Startup.cs* file and add two usings:  [!code-csharp[Main](../../tutorials/first-mvc-app-xplat/start-mvc/sample/MvcMovie/Startup.cs?name=snippet1&highlight=1,2)]</span></span>

- <span data-ttu-id="7a2db-119">Fügen Sie den Datenbankkontext zur Datei *Startup.cs* hinzu:</span><span class="sxs-lookup"><span data-stu-id="7a2db-119">Add the database context to the *Startup.cs* file:</span></span>

   [!code-csharp[Main](../../tutorials/first-mvc-app-xplat/start-mvc/sample/MvcMovie/Startup.cs?name=snippet2&highlight=6-7)]

  <span data-ttu-id="7a2db-120">Dadurch weiß das Entity Framework, welche Modellklassen im Datenmodell enthalten sind.</span><span class="sxs-lookup"><span data-stu-id="7a2db-120">This tells Entity Framework which model classes are included in the data model.</span></span> <span data-ttu-id="7a2db-121">Sie definieren eine *Entitätenmenge* von Filmobjekten, die in der Datenbank als Tabelle namens „Film“ dargestellt wird.</span><span class="sxs-lookup"><span data-stu-id="7a2db-121">You're defining one *entity set* of Movie objects, which will be represented in the database as a Movie table.</span></span>

- <span data-ttu-id="7a2db-122">Erstellen Sie das Projekt, um sicherzustellen, dass keine Fehler vorliegen.</span><span class="sxs-lookup"><span data-stu-id="7a2db-122">Build the project to verify there are no errors.</span></span>

## <a name="scaffold-the-moviecontroller"></a><span data-ttu-id="7a2db-123">Erstellen des Gerüsts für „MovieController“</span><span class="sxs-lookup"><span data-stu-id="7a2db-123">Scaffold the MovieController</span></span>

<span data-ttu-id="7a2db-124">Öffnen Sie im Projektordner ein Terminalfenster, und führen Sie die folgenden Befehle aus:</span><span class="sxs-lookup"><span data-stu-id="7a2db-124">Open a terminal window in the project folder and run the following commands:</span></span>

```
dotnet restore
dotnet aspnet-codegenerator controller -name MoviesController -m Movie -dc MvcMovieContext --relativeFolderPath Controllers --useDefaultLayout --referenceScriptLibraries 
```
<span data-ttu-id="7a2db-125">Wenn Sie die Fehlermeldung `No executable found matching command "dotnet-aspnet-codegenerator", verify` erhalten, trifft Folgendes zu:</span><span class="sxs-lookup"><span data-stu-id="7a2db-125">If you get the error `No executable found matching command "dotnet-aspnet-codegenerator", verify`:</span></span>

 * <span data-ttu-id="7a2db-126">Sie befinden sich im Projektverzeichnis.</span><span class="sxs-lookup"><span data-stu-id="7a2db-126">You are in the project directory.</span></span> <span data-ttu-id="7a2db-127">Hier befinden sich die Dateien *Program.cs*, *Startup.cs* und *.csproj*.</span><span class="sxs-lookup"><span data-stu-id="7a2db-127">The project directory has the *Program.cs*, *Startup.cs* and *.csproj* files.</span></span>
 * <span data-ttu-id="7a2db-128">Sie verwenden die dotnet-Version 1.1 oder höher.</span><span class="sxs-lookup"><span data-stu-id="7a2db-128">Your dotnet version is 1.1 or higher.</span></span> <span data-ttu-id="7a2db-129">Führen Sie `dotnet` aus, um Informationen über die Version zu erhalten.</span><span class="sxs-lookup"><span data-stu-id="7a2db-129">Run `dotnet` to get the version.</span></span>
 * <span data-ttu-id="7a2db-130">Sie haben der Datei [MvcMovie.csproj](#prepare-the-project-for-scaffolding) das Element `<DotNetCliToolReference>` hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="7a2db-130">You have added the `<DotNetCliToolReference>` element to the [MvcMovie.csproj file](#prepare-the-project-for-scaffolding).</span></span>
 
<!--
> [!NOTE]
> If you get an error when the scaffolding command runs, see [issue 444 in the scaffolding repository](https://github.com/aspnet/scaffolding/issues/444) for a workaround.
-->

<span data-ttu-id="7a2db-131">Das Gerüstbaumodul erstellt Folgendes:</span><span class="sxs-lookup"><span data-stu-id="7a2db-131">The scaffolding engine creates the following:</span></span>

* <span data-ttu-id="7a2db-132">Einen Filmcontroller (*Controllers/MoviesController.cs*)</span><span class="sxs-lookup"><span data-stu-id="7a2db-132">A movies controller (*Controllers/MoviesController.cs*)</span></span>
* <span data-ttu-id="7a2db-133">Razor-Ansichtsdateien für Seiten der Typen „Erstellen“, „Löschen“, „Details“ und „Index“ (*Views/Movies/\*.cshtml*)</span><span class="sxs-lookup"><span data-stu-id="7a2db-133">Razor view files for Create, Delete, Details, Edit and Index pages (*Views/Movies/\*.cshtml*)</span></span>

<span data-ttu-id="7a2db-134">Die automatische Erstellung von [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete)-Aktionsmethoden und Ansichten (create, read update and delete – Erstellen, Lesen, Aktualisieren und Löschen) wird als *Gerüstbau* bezeichnet.</span><span class="sxs-lookup"><span data-stu-id="7a2db-134">The automatic creation of [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) (create, read, update, and delete) action methods and views is known as *scaffolding*.</span></span> <span data-ttu-id="7a2db-135">Bald verfügen Sie über eine voll funktionsfähige Webanwendung, mit der Sie eine Filmdatenbank verwalten können.</span><span class="sxs-lookup"><span data-stu-id="7a2db-135">You'll soon have a fully functional web application that lets you manage a movie database.</span></span>

### <a name="add-the-files-to-visual-studio"></a><span data-ttu-id="7a2db-136">Hinzufügen der Dateien zu Visual Studio</span><span class="sxs-lookup"><span data-stu-id="7a2db-136">Add the files to Visual Studio</span></span>

* <span data-ttu-id="7a2db-137">Fügen Sie dem Visual Studio-Projekt die Datei *MovieController.cs* hinzu:</span><span class="sxs-lookup"><span data-stu-id="7a2db-137">Add the *MovieController.cs* file to the Visual Studio project:</span></span>

  * <span data-ttu-id="7a2db-138">Klicken Sie mit der rechten Maustaste auf den Ordner *Controller*, und wählen Sie **Hinzufügen > Dateien hinzufügen** aus.</span><span class="sxs-lookup"><span data-stu-id="7a2db-138">Right-click on the *Controllers* folder and select **Add > Add Files**.</span></span>
  * <span data-ttu-id="7a2db-139">Wählen Sie die Datei *MovieController.cs* aus.</span><span class="sxs-lookup"><span data-stu-id="7a2db-139">Select the *MovieController.cs* file.</span></span>

* <span data-ttu-id="7a2db-140">Fügen Sie den Ordner *Filme* und die Ansichten hinzu:</span><span class="sxs-lookup"><span data-stu-id="7a2db-140">Add the *Movies* folder and views:</span></span>

  * <span data-ttu-id="7a2db-141">Klicken Sie mit der rechten Maustaste auf den Ordner *Ansichten*, und wählen Sie **Hinzufügen > Vorhandenen Ordner hinzufügen** aus.</span><span class="sxs-lookup"><span data-stu-id="7a2db-141">Right-click on the *Views* folder and select **Add > Add Existing Folder**.</span></span>
  * <span data-ttu-id="7a2db-142">Navigieren Sie zum Ordner *Ansichten*, und wählen Sie *Ansichten\Filme* und anschließend **Öffnen** aus.</span><span class="sxs-lookup"><span data-stu-id="7a2db-142">Navigate to the *Views* folder, select *Views\Movies*, and then select **Open**.</span></span>
  * <span data-ttu-id="7a2db-143">Wählen Sie im Dialogfeld **Select files to add from Movies** (Aus „Filme“ hinzuzufügende Dateien auswählen) **Alle einschließen** aus, und klicken Sie dann auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="7a2db-143">In the **Select files to add from Movies** dialog, select **Include All**, and then **OK**.</span></span>

[!INCLUDE[adding-model 2x](../../includes/mvc-intro/adding-model2xp.md)]

[!INCLUDE[adding-model](../../includes/mvc-intro/adding-model3.md)]

<span data-ttu-id="7a2db-144">Sie verfügen jetzt über eine Datenbank und Seiten zum Anzeigen, Bearbeiten, Aktualisieren und Löschen von Daten.</span><span class="sxs-lookup"><span data-stu-id="7a2db-144">You now have a database and pages to display, edit, update and delete data.</span></span> <span data-ttu-id="7a2db-145">Im nächsten Tutorial werden wir mit dieser Datenbank arbeiten.</span><span class="sxs-lookup"><span data-stu-id="7a2db-145">In the next tutorial, we'll work with the database.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="7a2db-146">Zusätzliche Ressourcen</span><span class="sxs-lookup"><span data-stu-id="7a2db-146">Additional resources</span></span>

* [<span data-ttu-id="7a2db-147">Taghilfsprogramme</span><span class="sxs-lookup"><span data-stu-id="7a2db-147">Tag Helpers</span></span>](xref:mvc/views/tag-helpers/intro)
* [<span data-ttu-id="7a2db-148">Globalisierung und Lokalisierung</span><span class="sxs-lookup"><span data-stu-id="7a2db-148">Globalization and localization</span></span>](xref:fundamentals/localization)

>[!div class="step-by-step"]
<span data-ttu-id="7a2db-149">[Vorheriges Tutorial: Hinzufügen einer Ansicht](adding-view.md)
[Nächstes Tutorial: Arbeiten mit SQL](working-with-sql.md)</span><span class="sxs-lookup"><span data-stu-id="7a2db-149">[Previous Adding a View](adding-view.md)
[Next Working with SQL](working-with-sql.md)</span></span>  
