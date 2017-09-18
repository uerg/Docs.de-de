---
title: "Hinzufügen eines Modells"
author: rick-anderson
description: "Hinzufügen eines Modells zu einer einfachen ASP.NET Core-App"
keywords: ASP.NET Core
ms.author: riande
manager: wpickett
ms.date: 03/30/2017
ms.topic: get-started-article
ms.assetid: 8dc28498-eeee-4666-b903-b593059e9f39
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/first-mvc-app-xplat/adding-model
ms.openlocfilehash: c68466a645687b6fe0193e9deec2f32632e6f0e7
ms.sourcegitcommit: 9cdbfd0d670d70b9c354216aabee260c52dad5ee
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/12/2017
---
[!INCLUDE[adding-model1](../../includes/mvc-intro/adding-model1.md)]

* <span data-ttu-id="b9080-104">Fügen Sie eine Klasse zum Ordner *Modelle* mit dem Namen *Movie.cs* hinzu.</span><span class="sxs-lookup"><span data-stu-id="b9080-104">Add a class to the *Models* folder named *Movie.cs*.</span></span>
* <span data-ttu-id="b9080-105">Fügen Sie der Datei *Models/Movie.cs* den folgenden Code hinzu:</span><span class="sxs-lookup"><span data-stu-id="b9080-105">Add the following code to the *Models/Movie.cs* file:</span></span>

<span data-ttu-id="b9080-106">[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/MovieNoEF.cs?name=snippet_1)]</span><span class="sxs-lookup"><span data-stu-id="b9080-106">[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/MovieNoEF.cs?name=snippet_1)]</span></span>

<span data-ttu-id="b9080-107">Die Datenbank benötigt das Feld `ID` für den primären Schlüssel.</span><span class="sxs-lookup"><span data-stu-id="b9080-107">The `ID` field is required by the database for the primary key.</span></span> 

<span data-ttu-id="b9080-108">Erstellen Sie die App, um sicherzustellen, dass keine Fehler auftreten und Sie ein **M**odell zu Ihrer **M**VC-App hinzugefügt haben.</span><span class="sxs-lookup"><span data-stu-id="b9080-108">Build the app to verify you don't have any errors, and you've finally added a **M**odel to your **M**VC app.</span></span>

## <a name="prepare-the-project-for-scaffolding"></a><span data-ttu-id="b9080-109">Vorbereiten des Projekts für den Gerüstbau</span><span class="sxs-lookup"><span data-stu-id="b9080-109">Prepare the project for scaffolding</span></span>

- <span data-ttu-id="b9080-110">Fügen Sie die folgenden hervorgehobenen NuGet-Pakete in die Datei *MvcMovie.csproj* ein:</span><span class="sxs-lookup"><span data-stu-id="b9080-110">Add the following highlighted NuGet packages to the *MvcMovie.csproj* file:</span></span>
             
   <span data-ttu-id="b9080-111">[!code-csharp[Main](start-mvc/sample/MvcMovie/MvcMovie.csproj?highlight=7,10)]</span><span class="sxs-lookup"><span data-stu-id="b9080-111">[!code-csharp[Main](start-mvc/sample/MvcMovie/MvcMovie.csproj?highlight=7,10)]</span></span>

- <span data-ttu-id="b9080-112">Speichern Sie die Datei, und klicken Sie bei der **Info-Meldung** „There are unresolved dependencies“ (Es bestehen ungelöste Abhängigkeiten) auf **Wiederherstellen**.</span><span class="sxs-lookup"><span data-stu-id="b9080-112">Save the file and select **Restore** to the **Info** message "There are unresolved dependencies".</span></span>
- <span data-ttu-id="b9080-113">Erstellen sie eine *Models/MvcMovieContext.cs*-Datei, und fügen Sie die folgende Klasse des Typs `MvcMovieContext` hinzu:</span><span class="sxs-lookup"><span data-stu-id="b9080-113">Create a *Models/MvcMovieContext.cs* file and add the following `MvcMovieContext` class:</span></span>

   <span data-ttu-id="b9080-114">[!code-csharp[Main](start-mvc/sample/MvcMovie/Models/MvcMovieContext.cs)]</span><span class="sxs-lookup"><span data-stu-id="b9080-114">[!code-csharp[Main](start-mvc/sample/MvcMovie/Models/MvcMovieContext.cs)]</span></span>
   
- <span data-ttu-id="b9080-115">Öffnen Sie die Datei *Startup.cs*, und fügen Sie zwei Usings hinzu:</span><span class="sxs-lookup"><span data-stu-id="b9080-115">Open the *Startup.cs* file and add two usings:</span></span>

   <span data-ttu-id="b9080-116">[!code-csharp[Main](start-mvc/sample/MvcMovie/Startup.cs?name=snippet1&highlight=1,2)]</span><span class="sxs-lookup"><span data-stu-id="b9080-116">[!code-csharp[Main](start-mvc/sample/MvcMovie/Startup.cs?name=snippet1&highlight=1,2)]</span></span>

- <span data-ttu-id="b9080-117">Fügen Sie den Datenbankkontext zur Datei *Startup.cs* hinzu:</span><span class="sxs-lookup"><span data-stu-id="b9080-117">Add the database context to the *Startup.cs* file:</span></span>

   <span data-ttu-id="b9080-118">[!code-csharp[Main](start-mvc/sample/MvcMovie/Startup.cs?name=snippet2&highlight=6-7)]</span><span class="sxs-lookup"><span data-stu-id="b9080-118">[!code-csharp[Main](start-mvc/sample/MvcMovie/Startup.cs?name=snippet2&highlight=6-7)]</span></span>

  <span data-ttu-id="b9080-119">Dadurch weiß das Entity Framework, welche Modellklassen im Datenmodell enthalten sind.</span><span class="sxs-lookup"><span data-stu-id="b9080-119">This tells Entity Framework which model classes are included in the data model.</span></span> <span data-ttu-id="b9080-120">Sie definieren eine *Entitätenmenge* von Filmobjekten, die in der Datenbank als Tabelle namens „Film“ dargestellt wird.</span><span class="sxs-lookup"><span data-stu-id="b9080-120">You're defining one *entity set* of Movie objects, which will be represented in the database as a Movie table.</span></span>

- <span data-ttu-id="b9080-121">Erstellen Sie das Projekt, um sicherzustellen, dass keine Fehler vorliegen.</span><span class="sxs-lookup"><span data-stu-id="b9080-121">Build the project to verify there are no errors.</span></span>

## <a name="scaffold-the-moviecontroller"></a><span data-ttu-id="b9080-122">Erstellen des Gerüsts für „MovieController“</span><span class="sxs-lookup"><span data-stu-id="b9080-122">Scaffold the MovieController</span></span>

<span data-ttu-id="b9080-123">Öffnen Sie im Projektordner ein Terminalfenster, und führen Sie die folgenden Befehle aus:</span><span class="sxs-lookup"><span data-stu-id="b9080-123">Open a terminal window in the project folder and run the following commands:</span></span>

```
dotnet restore
dotnet aspnet-codegenerator controller -name MoviesController -m Movie -dc MvcMovieContext --relativeFolderPath Controllers --useDefaultLayout --referenceScriptLibraries 
```

> [!NOTE]
> <span data-ttu-id="b9080-124">Wenn Sie eine Fehlermeldung erhalten, wenn der Gerüstbau-Befehl ausgeführt wird, finden Sie unter [issue 444 in the scaffolding repository](https://github.com/aspnet/scaffolding/issues/444) (Problem 444 im Gerüstbau-Repository) eine Möglichkeit zur Umgehung des Problems.</span><span class="sxs-lookup"><span data-stu-id="b9080-124">If you get an error when the scaffolding command runs, see [issue 444 in the scaffolding repository](https://github.com/aspnet/scaffolding/issues/444) for a workaround.</span></span>

<span data-ttu-id="b9080-125">Das Gerüstbau-Modul erstellt folgende Dinge:</span><span class="sxs-lookup"><span data-stu-id="b9080-125">The scaffolding engine creates the following:</span></span>

* <span data-ttu-id="b9080-126">Einen Filmcontroller (*Controllers/MoviesController.cs*)</span><span class="sxs-lookup"><span data-stu-id="b9080-126">A movies controller (*Controllers/MoviesController.cs*)</span></span>
* <span data-ttu-id="b9080-127">Razor-Ansichtsdateien für Seiten der Typen „Erstellen“, „Löschen“, „Details“ und „Index“ (*Views/Movies/\*.cshtml*)</span><span class="sxs-lookup"><span data-stu-id="b9080-127">Razor view files for Create, Delete, Details, Edit and Index pages (*Views/Movies/\*.cshtml*)</span></span>

<span data-ttu-id="b9080-128">Die automatische Erstellung von [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete)-Aktionsmethoden und Ansichten (create, read update and delete – Erstellen, Lesen, Aktualisieren und Löschen) wird als *Gerüstbau* bezeichnet.</span><span class="sxs-lookup"><span data-stu-id="b9080-128">The automatic creation of [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) (create, read, update, and delete) action methods and views is known as *scaffolding*.</span></span> <span data-ttu-id="b9080-129">Bald haben Sie eine voll funktionsfähige Webanwendung, mit der Sie eine Filmdatenbank verwalten können.</span><span class="sxs-lookup"><span data-stu-id="b9080-129">You'll soon have a fully functional web application that lets you manage a movie database.</span></span>

[!INCLUDE[adding-model 2x](../../includes/mvc-intro/adding-model2xp.md)]

[!INCLUDE[adding-model](../../includes/mvc-intro/adding-model3.md)]

<span data-ttu-id="b9080-130">Sie verfügen jetzt über eine Datenbank und Seiten zum Anzeigen, Bearbeiten, Aktualisieren und Löschen von Dateien.</span><span class="sxs-lookup"><span data-stu-id="b9080-130">You now have a database and pages to display, edit, update and delete data.</span></span> <span data-ttu-id="b9080-131">Im nächsten Tutorial werden wir mit dieser Datenbank arbeiten.</span><span class="sxs-lookup"><span data-stu-id="b9080-131">In the next tutorial, we'll work with the database.</span></span>

### <a name="additional-resources"></a><span data-ttu-id="b9080-132">Zusätzliche Ressourcen</span><span class="sxs-lookup"><span data-stu-id="b9080-132">Additional resources</span></span>

* [<span data-ttu-id="b9080-133">Taghilfsprogramme</span><span class="sxs-lookup"><span data-stu-id="b9080-133">Tag Helpers</span></span>](xref:mvc/views/tag-helpers/intro)
* [<span data-ttu-id="b9080-134">Globalization and localization (Globalisierung und Lokalisierung)</span><span class="sxs-lookup"><span data-stu-id="b9080-134">Globalization and localization</span></span>](xref:fundamentals/localization)

>[!div class="step-by-step"]
<span data-ttu-id="b9080-135">[Vorheriges Tutorial: Adding a View (Hinzufügen einer Ansicht)](adding-view.md)
[Nächstes Tutorial: Working with SQLite (Arbeiten mit SQLite)](working-with-sql.md)</span><span class="sxs-lookup"><span data-stu-id="b9080-135">[Previous - Add a view](adding-view.md)
[Next - Working with SQLite](working-with-sql.md)</span></span>
