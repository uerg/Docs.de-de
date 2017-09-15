---
title: "Hinzufügen eines Modells zu einer ASP.NET Core MVC-App"
author: rick-anderson
description: "Fügen Sie ein Modell zu einer einfachen ASP.NET Core-App hinzu."
keywords: ASP.NET Core,
ms.author: riande
manager: wpickett
ms.date: 03/30/2017
ms.topic: get-started-article
ms.assetid: 8dc28498-00ee-4d66-b903-b593059e9f39
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/first-mvc-app/adding-model
ms.openlocfilehash: 8d0bebc22e1cfdc6d9b213d0c3159a7dab988020
ms.sourcegitcommit: 0bd3f6ec577c648dd777877e97572ec2da1b36c4
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/22/2017
---
[!INCLUDE[adding-model](../../includes/mvc-intro/adding-model1.md)]

<span data-ttu-id="01190-104">Hinweis: Die ASP.NET Core 2.0-Vorlagen enthalten den Ordner *Modelle*.</span><span class="sxs-lookup"><span data-stu-id="01190-104">Note: The ASP.NET Core 2.0 templates contain the *Models* folder.</span></span>

<span data-ttu-id="01190-105">Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf das Projekt **MvcMovie** > **Hinzufügen** > **Neuer Ordner**.</span><span class="sxs-lookup"><span data-stu-id="01190-105">In Solution Explorer, right click the **MvcMovie** project > **Add** > **New Folder**.</span></span> <span data-ttu-id="01190-106">Geben Sie dem Ordner den Namen *Modelle*.</span><span class="sxs-lookup"><span data-stu-id="01190-106">Name the folder *Models*.</span></span>

<span data-ttu-id="01190-107">Klicken Sie mit der rechten Maustaste auf den Ordner *Modelle* > **Hinzufügen** > **Klasse**.</span><span class="sxs-lookup"><span data-stu-id="01190-107">Right click the *Models* folder > **Add** > **Class**.</span></span> <span data-ttu-id="01190-108">Nennen Sie die Klasse **Movie**, und fügen Sie Ihr die folgenden Eigenschaften hinzu:</span><span class="sxs-lookup"><span data-stu-id="01190-108">Name the class **Movie** and add the following properties:</span></span>

<span data-ttu-id="01190-109">[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/MovieNoEF.cs?name=snippet_1)]</span><span class="sxs-lookup"><span data-stu-id="01190-109">[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/MovieNoEF.cs?name=snippet_1)]</span></span>

<span data-ttu-id="01190-110">Die Datenbank benötigt das Feld `ID` für den primären Schlüssel.</span><span class="sxs-lookup"><span data-stu-id="01190-110">The `ID` field is required by the database for the primary key.</span></span> 

<span data-ttu-id="01190-111">Erstellen Sie das Projekt, um sicherzustellen, dass keine Fehler vorliegen.</span><span class="sxs-lookup"><span data-stu-id="01190-111">Build the project to verify you don't have any errors.</span></span> <span data-ttu-id="01190-112">Sie verfügen jetzt über ein **Modell** in Ihrer **MVC**-App.</span><span class="sxs-lookup"><span data-stu-id="01190-112">You now have a **M**odel in your **M**VC app.</span></span>

## <a name="scaffolding-a-controller"></a><span data-ttu-id="01190-113">Gerüstbau für einen Controller</span><span class="sxs-lookup"><span data-stu-id="01190-113">Scaffolding a controller</span></span>

<span data-ttu-id="01190-114">Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf den Ordner *Controller* > **Hinzufügen > Controller**.</span><span class="sxs-lookup"><span data-stu-id="01190-114">In **Solution Explorer**, right-click the *Controllers* folder **> Add > Controller**.</span></span>

![Screenshot für oben genannten Schritt](adding-model/_static/add_controller.png)

<span data-ttu-id="01190-116">Wählen Sie im Dialogfeld **MVC-Abhängigkeiten hinzufügen** die Option **Mindestens erforderliche Abhängigkeiten** und dann **Hinzufügen** aus.</span><span class="sxs-lookup"><span data-stu-id="01190-116">In the **Add MVC Dependencies** dialog, select **Minimal Dependencies**, and select **Add**.</span></span>

![Screenshot für oben genannten Schritt](adding-model/_static/add_depend.png)

<span data-ttu-id="01190-118">Visual Studio fügt die für das Gerüst eines Controllers erforderlichen Abhängigkeiten hinzu, aber der Controller selbst wird nicht erstellt.</span><span class="sxs-lookup"><span data-stu-id="01190-118">Visual Studio adds the dependencies needed to scaffold a controller, but the controller itself is not created.</span></span> <span data-ttu-id="01190-119">Der nächste Aufruf von **> Hinzufügen > Controller** erstellt den Controller.</span><span class="sxs-lookup"><span data-stu-id="01190-119">The next invoke of **> Add > Controller** creates the controller.</span></span> 

<span data-ttu-id="01190-120">Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf den Ordner *Controller* > **Hinzufügen > Controller**.</span><span class="sxs-lookup"><span data-stu-id="01190-120">In **Solution Explorer**, right-click the *Controllers* folder **> Add > Controller**.</span></span>

![Screenshot für oben genannten Schritt](adding-model/_static/add_controller.png)

<span data-ttu-id="01190-122">Tippen Sie im Dialogfeld **Gerüst hinzufügen** auf **MVC-Controller mit Ansichten unter Verwendung von Entity Framework > Hinzufügen**.</span><span class="sxs-lookup"><span data-stu-id="01190-122">In the **Add Scaffold** dialog, tap **MVC Controller with views, using Entity Framework > Add**.</span></span>

![Dialogfeld „Gerüst hinzufügen“](adding-model/_static/add_scaffold2.png)

<span data-ttu-id="01190-124">Bearbeiten Sie das Dialogfeld **Controller hinzufügen**:</span><span class="sxs-lookup"><span data-stu-id="01190-124">Complete the **Add Controller** dialog:</span></span>

* <span data-ttu-id="01190-125">**Modellklasse:** *Movie (MvcMovie.Models)*</span><span class="sxs-lookup"><span data-stu-id="01190-125">**Model class:** *Movie (MvcMovie.Models)*</span></span>
* <span data-ttu-id="01190-126">**Datenkontextklasse:** Wählen Sie das Symbol **+** aus, und fügen Sie den Standardtyp **MvcMovie.Models.MvcMovieContext** hinzu</span><span class="sxs-lookup"><span data-stu-id="01190-126">**Data context class:** Select the **+** icon and add the default **MvcMovie.Models.MvcMovieContext**</span></span>

![Datenkontext hinzufügen](adding-model/_static/dc.png)

* <span data-ttu-id="01190-128">**Ansichten:** Lassen Sie den Standardwert der einzelnen Optionen aktiviert</span><span class="sxs-lookup"><span data-stu-id="01190-128">**Views:** Keep the default of each option checked</span></span>
* <span data-ttu-id="01190-129">**Controllername:** Behalten Sie den Standardwert *MoviesController* bei</span><span class="sxs-lookup"><span data-stu-id="01190-129">**Controller name:** Keep the default *MoviesController*</span></span>
* <span data-ttu-id="01190-130">Tippen Sie auf **Hinzufügen**.</span><span class="sxs-lookup"><span data-stu-id="01190-130">Tap **Add**</span></span>

![Dialogfeld „Controller hinzufügen“](adding-model/_static/add_controller2.png)

<span data-ttu-id="01190-132">Visual Studio erstellt Folgendes:</span><span class="sxs-lookup"><span data-stu-id="01190-132">Visual Studio creates:</span></span>

* <span data-ttu-id="01190-133">Eine Entity Framework Core-[Datenbankkontext-Klasse](xref:data/ef-mvc/intro#create-the-database-context) (*Data/MvcMovieContext.cs*)</span><span class="sxs-lookup"><span data-stu-id="01190-133">An Entity Framework Core [database context class](xref:data/ef-mvc/intro#create-the-database-context) (*Data/MvcMovieContext.cs*)</span></span>
* <span data-ttu-id="01190-134">Einen Movies-Controller (*Controllers/MoviesController.cs*)</span><span class="sxs-lookup"><span data-stu-id="01190-134">A movies controller (*Controllers/MoviesController.cs*)</span></span>
* <span data-ttu-id="01190-135">Razor-Ansichtsdateien für Seiten der Typen „Erstellen“, „Löschen“, „Details“ „Bearbeiten“ und „Index“ (*Views/Movies/&ast;.cshtml*)</span><span class="sxs-lookup"><span data-stu-id="01190-135">Razor view files for Create, Delete, Details, Edit and Index pages (*Views/Movies/&ast;.cshtml*)</span></span>

<span data-ttu-id="01190-136">Die automatische Erstellung des Datenbankkontexts und der [CRUD](https://en.wikipedia.org/wiki/Create,_read,_update_and_delete)-Aktionsmethoden (create, read, update and delete – Erstellen, Lesen, Aktualisieren und Löschen) und Ansichten wird als *Gerüstbau* bezeichnet.</span><span class="sxs-lookup"><span data-stu-id="01190-136">The automatic creation of the database context and [CRUD](https://en.wikipedia.org/wiki/Create,_read,_update_and_delete) (create, read, update, and delete) action methods and views is known as *scaffolding*.</span></span> <span data-ttu-id="01190-137">Bald verfügen Sie über eine voll funktionsfähige Webanwendung, mit der Sie eine Filmdatenbank verwalten können.</span><span class="sxs-lookup"><span data-stu-id="01190-137">You'll soon have a fully functional web application that lets you manage a movie database.</span></span>

<span data-ttu-id="01190-138">Wenn Sie die App auszuführen und auf den Link **Mvc Movie** klicken, erhalten Sie eine Fehlermeldung wie die folgende:</span><span class="sxs-lookup"><span data-stu-id="01190-138">If you run the app and click on the **Mvc Movie** link, you'll get an error similar to the following:</span></span>

```
An unhandled exception occurred while processing the request.

SqlException: Cannot open database "MvcMovieContext-<GUID removed>" requested by the login. The login failed.
Login failed for user 'Rick'.

System.Data.SqlClient.SqlInternalConnectionTds..ctor(DbConnectionPoolIdentity identity, SqlConnectionString 
```

<span data-ttu-id="01190-139">Sie müssen die Datenbank erstellen und verwenden dazu das EF Core-Feature [Migrationen](xref:data/ef-mvc/migrations).</span><span class="sxs-lookup"><span data-stu-id="01190-139">You need to create the database, and you'll use the EF Core [Migrations](xref:data/ef-mvc/migrations) feature to do that.</span></span> <span data-ttu-id="01190-140">Mit „Migrationen“ können Sie eine Datenbank erstellen, die mit Ihrem Datenmodell übereinstimmt, und das Datenbankschema aktualisieren, wenn sich Ihr Datenmodell ändert.</span><span class="sxs-lookup"><span data-stu-id="01190-140">Migrations lets you create a database that matches your data model and update the database schema when your data model changes.</span></span>

## <a name="add-ef-tooling-and-perform-initial-migration"></a><span data-ttu-id="01190-141">Hinzufügen von EF-Tools und Ausführen der anfänglichen Migration</span><span class="sxs-lookup"><span data-stu-id="01190-141">Add EF tooling and perform initial migration</span></span>

<span data-ttu-id="01190-142">In diesem Abschnitt führen Sie folgende Aktionen mit der Paket-Manager-Konsole (Package Manager Console, PMC) aus:</span><span class="sxs-lookup"><span data-stu-id="01190-142">In this section you'll use the Package Manager Console (PMC) to:</span></span>

* <span data-ttu-id="01190-143">Fügen Sie das Entity Framework Core-Toolpaket hinzu.</span><span class="sxs-lookup"><span data-stu-id="01190-143">Add the Entity Framework Core Tools package.</span></span> <span data-ttu-id="01190-144">Dieses Paket ist erforderlich, um Migrationen hinzuzufügen und die Datenbank zu aktualisieren.</span><span class="sxs-lookup"><span data-stu-id="01190-144">This package is required to add migrations and update the database.</span></span>
* <span data-ttu-id="01190-145">Fügen Sie eine anfängliche Migration hinzu.</span><span class="sxs-lookup"><span data-stu-id="01190-145">Add an initial migration.</span></span>
* <span data-ttu-id="01190-146">Aktualisieren Sie die Datenbank mit der anfänglichen Migration.</span><span class="sxs-lookup"><span data-stu-id="01190-146">Update the database with the initial migration.</span></span>

<span data-ttu-id="01190-147">Öffnen Sie das Menü **Extras**. Wählen Sie dort **NuGet-Paket-Manager > Paket-Manager-Konsole** aus.</span><span class="sxs-lookup"><span data-stu-id="01190-147">From the **Tools** menu, select **NuGet Package Manager > Package Manager Console**.</span></span>

<!-- following image shared with uid: tutorials/razor-pages/model -->
  ![PMC-Menü](adding-model/_static/pmc.png)

<span data-ttu-id="01190-149">Geben Sie in der PMC die folgenden Befehle ein:</span><span class="sxs-lookup"><span data-stu-id="01190-149">In the PMC, enter the following commands:</span></span>

``` PMC
Install-Package Microsoft.EntityFrameworkCore.Tools
Add-Migration Initial
Update-Database
```

<span data-ttu-id="01190-150">Hinweis: Nutzen Sie bei Problemen mit PMC den [CLI-Ansatz](#cli).</span><span class="sxs-lookup"><span data-stu-id="01190-150">Note: See the [CLI approach](#cli) if you have problems with the PMC.</span></span>

<span data-ttu-id="01190-151">Mit dem Befehl `Add-Migration` wird Code erstellt, um das anfängliche Datenbankschema zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="01190-151">The `Add-Migration` command creates code to create the initial database schema.</span></span> <span data-ttu-id="01190-152">Das Schema basiert auf dem Modell, das in der Datei *Data/MvcMovieContext.cs* für `DbContext` angegeben ist.</span><span class="sxs-lookup"><span data-stu-id="01190-152">The schema is based on the model specified in the `DbContext`(In the *Data/MvcMovieContext.cs* file).</span></span> <span data-ttu-id="01190-153">Das Argument `Initial` wird verwendet, um die Migrationen zu benennen.</span><span class="sxs-lookup"><span data-stu-id="01190-153">The `Initial` argument is used to name the migrations.</span></span> <span data-ttu-id="01190-154">Sie können einen beliebigen Namen auswählen, sollten aber der Konvention zufolge einen Namen verwenden, der die Migration beschreibt.</span><span class="sxs-lookup"><span data-stu-id="01190-154">You can use any name, but by convention you choose a name that describes the migration.</span></span> <span data-ttu-id="01190-155">Weitere Informationen finden Sie unter [Introduction to migrations (Einführung in Migrationen)](xref:data/ef-mvc/migrations#introduction-to-migrations).</span><span class="sxs-lookup"><span data-stu-id="01190-155">See [Introduction to migrations](xref:data/ef-mvc/migrations#introduction-to-migrations) for more information.</span></span>

<span data-ttu-id="01190-156">Mit dem Befehl `Update-Database` führen Sie in der Datei *Migrations/\<time-stamp>_InitialCreate.cs* die Methode `Up` aus, mit der die Datenbank erstellt wird.</span><span class="sxs-lookup"><span data-stu-id="01190-156">The `Update-Database` command runs the `Up` method in the *Migrations/\<time-stamp>_InitialCreate.cs* file, which creates the database.</span></span>

<span data-ttu-id="01190-157"><a name="cli"></a> Sie können die vorherigen Schritte über die Befehlszeilenschnittstelle (CLI) statt mit PMC ausführen:</span><span class="sxs-lookup"><span data-stu-id="01190-157"><a name="cli"></a> You can perform the preceeding steps using the command-line interface (CLI) rather than the PMC:</span></span>

* <span data-ttu-id="01190-158">Fügen Sie [EF Core-Tools](xref:data/ef-mvc/migrations#entity-framework-core-nuget-packages-for-migrations) zur *CSPROJ*-Datei hinzu.</span><span class="sxs-lookup"><span data-stu-id="01190-158">Add [EF Core tooling](xref:data/ef-mvc/migrations#entity-framework-core-nuget-packages-for-migrations) to the *.csproj* file.</span></span>
* <span data-ttu-id="01190-159">Führen Sie die folgenden Befehle über die Konsole aus (im Projektverzeichnis):</span><span class="sxs-lookup"><span data-stu-id="01190-159">Run the following commands from the console (in the project directory):</span></span>

  ```console
  dotnet ef migrations add InitialCreate
  dotnet ef database update
  ```     
  

[!INCLUDE[adding-model](../../includes/mvc-intro/adding-model3.md)]

<span data-ttu-id="01190-160">[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Startup.cs?name=ConfigureServices&highlight=6-7)]</span><span class="sxs-lookup"><span data-stu-id="01190-160">[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Startup.cs?name=ConfigureServices&highlight=6-7)]</span></span>

[!INCLUDE[adding-model](../../includes/mvc-intro/adding-model4.md)]

![IntelliSense-Kontextmenü für ein Modellelement mit den verfügbaren Eigenschaften für ID, Preis, Veröffentlichungsdatum und Titel](adding-model/_static/ints.png)

## <a name="additional-resources"></a><span data-ttu-id="01190-162">Zusätzliche Ressourcen</span><span class="sxs-lookup"><span data-stu-id="01190-162">Additional resources</span></span>

* [<span data-ttu-id="01190-163">Taghilfsprogramme</span><span class="sxs-lookup"><span data-stu-id="01190-163">Tag Helpers</span></span>](xref:mvc/views/tag-helpers/intro)
* [<span data-ttu-id="01190-164">Globalisierung und Lokalisierung</span><span class="sxs-lookup"><span data-stu-id="01190-164">Globalization and localization</span></span>](xref:fundamentals/localization)

>[!div class="step-by-step"]
<span data-ttu-id="01190-165">[Vorheriges Tutorial: Hinzufügen einer Ansicht](adding-view.md)
[Nächstes Tutorial: Arbeiten mit SQL](working-with-sql.md)</span><span class="sxs-lookup"><span data-stu-id="01190-165">[Previous Adding a View](adding-view.md)
[Next Working with SQL](working-with-sql.md)</span></span>  
