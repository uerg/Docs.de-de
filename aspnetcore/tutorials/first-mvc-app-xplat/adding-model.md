---
title: "Hinzufügen eines Modells zu einer ASP.NET Core MVC-App."
author: rick-anderson
description: "Fügen Sie ein Modell zu einer einfachen ASP.NET Core-App hinzu."
manager: wpickett
ms.author: riande
ms.date: 09/18/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/first-mvc-app-xplat/adding-model
ms.openlocfilehash: 81511b05a3cc11a58b93452d3c6e5305e7ee4357
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2018
---
[!INCLUDE[adding-model1](../../includes/mvc-intro/adding-model1.md)]

* <span data-ttu-id="fe5d8-103">Fügen Sie zum Ordner *Modelle* eine Klasse mit dem Namen *Movie.cs* hinzu.</span><span class="sxs-lookup"><span data-stu-id="fe5d8-103">Add a class to the *Models* folder named *Movie.cs*.</span></span>
* <span data-ttu-id="fe5d8-104">Fügen Sie der Datei *Models/Movie.cs* den folgenden Code hinzu:</span><span class="sxs-lookup"><span data-stu-id="fe5d8-104">Add the following code to the *Models/Movie.cs* file:</span></span>

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/MovieNoEF.cs?name=snippet_1)]

<span data-ttu-id="fe5d8-105">Die Datenbank benötigt das Feld `ID` für den primären Schlüssel.</span><span class="sxs-lookup"><span data-stu-id="fe5d8-105">The `ID` field is required by the database for the primary key.</span></span> 

<span data-ttu-id="fe5d8-106">Erstellen Sie die App, um sicherzustellen, dass keine Fehler auftreten und Sie ein **M**odell zu Ihrer **M**VC-App hinzugefügt haben.</span><span class="sxs-lookup"><span data-stu-id="fe5d8-106">Build the app to verify you don't have any errors, and you've finally added a **M**odel to your **M**VC app.</span></span>

## <a name="prepare-the-project-for-scaffolding"></a><span data-ttu-id="fe5d8-107">Vorbereiten des Projekts für den Gerüstbau</span><span class="sxs-lookup"><span data-stu-id="fe5d8-107">Prepare the project for scaffolding</span></span>

- <span data-ttu-id="fe5d8-108">Fügen Sie die folgenden hervorgehobenen NuGet-Pakete in die Datei *MvcMovie.csproj* ein:</span><span class="sxs-lookup"><span data-stu-id="fe5d8-108">Add the following highlighted NuGet packages to the *MvcMovie.csproj* file:</span></span>
             
   [!code-csharp[Main](start-mvc/sample/MvcMovie/MvcMovie.csproj?highlight=7,10)]

- <span data-ttu-id="fe5d8-109">Speichern Sie die Datei, und klicken Sie bei der **Info-Meldung** „There are unresolved dependencies“ (Es bestehen ungelöste Abhängigkeiten) auf **Wiederherstellen**.</span><span class="sxs-lookup"><span data-stu-id="fe5d8-109">Save the file and select **Restore** to the **Info** message "There are unresolved dependencies".</span></span>
- <span data-ttu-id="fe5d8-110">Erstellen sie eine *Models/MvcMovieContext.cs*-Datei, und fügen Sie die folgende Klasse des Typs `MvcMovieContext` hinzu:</span><span class="sxs-lookup"><span data-stu-id="fe5d8-110">Create a *Models/MvcMovieContext.cs* file and add the following `MvcMovieContext` class:</span></span>

   [!code-csharp[Main](start-mvc/sample/MvcMovie/Models/MvcMovieContext.cs)]
   
- <span data-ttu-id="fe5d8-111">Öffnen Sie die Datei *Startup.cs*, und fügen Sie zwei Usings hinzu:</span><span class="sxs-lookup"><span data-stu-id="fe5d8-111">Open the *Startup.cs* file and add two usings:</span></span>

   [!code-csharp[Main](start-mvc/sample/MvcMovie/Startup.cs?name=snippet1&highlight=1,2)]

- <span data-ttu-id="fe5d8-112">Fügen Sie den Datenbankkontext zur Datei *Startup.cs* hinzu:</span><span class="sxs-lookup"><span data-stu-id="fe5d8-112">Add the database context to the *Startup.cs* file:</span></span>

   [!code-csharp[Main](start-mvc/sample/MvcMovie/Startup.cs?name=snippet2&highlight=6-7)]

  <span data-ttu-id="fe5d8-113">Dadurch weiß das Entity Framework, welche Modellklassen im Datenmodell enthalten sind.</span><span class="sxs-lookup"><span data-stu-id="fe5d8-113">This tells Entity Framework which model classes are included in the data model.</span></span> <span data-ttu-id="fe5d8-114">Sie definieren eine *Entitätenmenge* von Filmobjekten, die in der Datenbank als Tabelle namens „Film“ dargestellt wird.</span><span class="sxs-lookup"><span data-stu-id="fe5d8-114">You're defining one *entity set* of Movie objects, which will be represented in the database as a Movie table.</span></span>

- <span data-ttu-id="fe5d8-115">Erstellen Sie das Projekt, um sicherzustellen, dass keine Fehler vorliegen.</span><span class="sxs-lookup"><span data-stu-id="fe5d8-115">Build the project to verify there are no errors.</span></span>

## <a name="scaffold-the-moviecontroller"></a><span data-ttu-id="fe5d8-116">Erstellen des Gerüsts für „MovieController“</span><span class="sxs-lookup"><span data-stu-id="fe5d8-116">Scaffold the MovieController</span></span>

<span data-ttu-id="fe5d8-117">Öffnen Sie im Projektordner ein Terminalfenster, und führen Sie die folgenden Befehle aus:</span><span class="sxs-lookup"><span data-stu-id="fe5d8-117">Open a terminal window in the project folder and run the following commands:</span></span>

```
dotnet restore
dotnet aspnet-codegenerator controller -name MoviesController -m Movie -dc MvcMovieContext --relativeFolderPath Controllers --useDefaultLayout --referenceScriptLibraries 
```
<span data-ttu-id="fe5d8-118">Das Gerüstbaumodul erstellt Folgendes:</span><span class="sxs-lookup"><span data-stu-id="fe5d8-118">The scaffolding engine creates the following:</span></span>

* <span data-ttu-id="fe5d8-119">Einen Filmcontroller (*Controllers/MoviesController.cs*)</span><span class="sxs-lookup"><span data-stu-id="fe5d8-119">A movies controller (*Controllers/MoviesController.cs*)</span></span>
* <span data-ttu-id="fe5d8-120">Razor-Ansichtsdateien für Seiten der Typen „Erstellen“, „Löschen“, „Details“ und „Index“ (*Views/Movies/\*.cshtml*)</span><span class="sxs-lookup"><span data-stu-id="fe5d8-120">Razor view files for Create, Delete, Details, Edit and Index pages (*Views/Movies/\*.cshtml*)</span></span>

<span data-ttu-id="fe5d8-121">Die automatische Erstellung von [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete)-Aktionsmethoden und Ansichten (create, read update and delete – Erstellen, Lesen, Aktualisieren und Löschen) wird als *Gerüstbau* bezeichnet.</span><span class="sxs-lookup"><span data-stu-id="fe5d8-121">The automatic creation of [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) (create, read, update, and delete) action methods and views is known as *scaffolding*.</span></span> <span data-ttu-id="fe5d8-122">Bald haben Sie eine voll funktionsfähige Webanwendung, mit der Sie eine Filmdatenbank verwalten können.</span><span class="sxs-lookup"><span data-stu-id="fe5d8-122">You'll soon have a fully functional web application that lets you manage a movie database.</span></span>

[!INCLUDE[adding-model 2x](../../includes/mvc-intro/adding-model2xp.md)]

[!INCLUDE[adding-model](../../includes/mvc-intro/adding-model3.md)]

<span data-ttu-id="fe5d8-123">Sie verfügen jetzt über eine Datenbank und Seiten zum Anzeigen, Bearbeiten, Aktualisieren und Löschen von Dateien.</span><span class="sxs-lookup"><span data-stu-id="fe5d8-123">You now have a database and pages to display, edit, update and delete data.</span></span> <span data-ttu-id="fe5d8-124">Im nächsten Tutorial werden wir mit dieser Datenbank arbeiten.</span><span class="sxs-lookup"><span data-stu-id="fe5d8-124">In the next tutorial, we'll work with the database.</span></span>

### <a name="additional-resources"></a><span data-ttu-id="fe5d8-125">Zusätzliche Ressourcen</span><span class="sxs-lookup"><span data-stu-id="fe5d8-125">Additional resources</span></span>

* [<span data-ttu-id="fe5d8-126">Taghilfsprogramme</span><span class="sxs-lookup"><span data-stu-id="fe5d8-126">Tag Helpers</span></span>](xref:mvc/views/tag-helpers/intro)
* [<span data-ttu-id="fe5d8-127">Globalization and localization (Globalisierung und Lokalisierung)</span><span class="sxs-lookup"><span data-stu-id="fe5d8-127">Globalization and localization</span></span>](xref:fundamentals/localization)

>[!div class="step-by-step"]
<span data-ttu-id="fe5d8-128">[Vorheriges Tutorial: Adding a View (Hinzufügen einer Ansicht)](adding-view.md)
[Nächstes Tutorial: Working with SQLite (Arbeiten mit SQLite)](working-with-sql.md)</span><span class="sxs-lookup"><span data-stu-id="fe5d8-128">[Previous - Add a view](adding-view.md)
[Next - Working with SQLite](working-with-sql.md)</span></span>
