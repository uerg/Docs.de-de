---
title: "Hinzufügen eines Modells zu einer ASP.NET Core MVC-App"
author: rick-anderson
description: "Fügen Sie ein Modell zu einer einfachen ASP.NET Core-App hinzu."
manager: wpickett
ms.author: riande
ms.date: 12/8/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/first-mvc-app/adding-model
ms.openlocfilehash: 1819aff0e6ae68ad3c609466e52fcb6510fe1dcd
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2018
---
[!INCLUDE[adding-model](../../includes/mvc-intro/adding-model1.md)]

<span data-ttu-id="6189c-103">Hinweis: Die ASP.NET Core 2.0-Vorlagen enthalten den Ordner *Modelle*.</span><span class="sxs-lookup"><span data-stu-id="6189c-103">Note: The ASP.NET Core 2.0 templates contain the *Models* folder.</span></span>

<span data-ttu-id="6189c-104">Klicken Sie mit der rechten Maustaste auf den Ordner *Modelle*>**Hinzufügen** > **Klasse**.</span><span class="sxs-lookup"><span data-stu-id="6189c-104">Right-click the *Models* folder > **Add** > **Class**.</span></span> <span data-ttu-id="6189c-105">Nennen Sie die Klasse **Movie**, und fügen Sie Ihr die folgenden Eigenschaften hinzu:</span><span class="sxs-lookup"><span data-stu-id="6189c-105">Name the class **Movie** and add the following properties:</span></span>

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/MovieNoEF.cs?name=snippet_1)]

<span data-ttu-id="6189c-106">Die Datenbank benötigt das Feld `ID` für den primären Schlüssel.</span><span class="sxs-lookup"><span data-stu-id="6189c-106">The `ID` field is required by the database for the primary key.</span></span> 

<span data-ttu-id="6189c-107">Erstellen Sie das Projekt, um sicherzustellen, dass keine Fehler vorliegen.</span><span class="sxs-lookup"><span data-stu-id="6189c-107">Build the project to verify you don't have any errors.</span></span> <span data-ttu-id="6189c-108">Sie verfügen jetzt über ein **Modell** in Ihrer **MVC**-App.</span><span class="sxs-lookup"><span data-stu-id="6189c-108">You now have a **M**odel in your **M**VC app.</span></span>

## <a name="scaffolding-a-controller"></a><span data-ttu-id="6189c-109">Gerüstbau für einen Controller</span><span class="sxs-lookup"><span data-stu-id="6189c-109">Scaffolding a controller</span></span>

<span data-ttu-id="6189c-110">Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf den Ordner *Controller* > **Hinzufügen > Controller**.</span><span class="sxs-lookup"><span data-stu-id="6189c-110">In **Solution Explorer**, right-click the *Controllers* folder **> Add > Controller**.</span></span>

![Screenshot für oben genannten Schritt](adding-model/_static/add_controller.png)

<span data-ttu-id="6189c-112">Wenn das Dialogfeld **MVC-Abhängigkeiten hinzufügen** angezeigt wird, gehen Sie wie folgt vor:</span><span class="sxs-lookup"><span data-stu-id="6189c-112">If the **Add MVC Dependencies** dialog appears:</span></span>

* <span data-ttu-id="6189c-113">[Aktualisieren Sie Visual Studio auf die neuste Version.](https://www.visualstudio.com/downloads/)</span><span class="sxs-lookup"><span data-stu-id="6189c-113">[Update Visual Studio to the latest version](https://www.visualstudio.com/downloads/).</span></span> <span data-ttu-id="6189c-114">Dieses Dialogfeld wird in allen Visual Studio Versionen vor Version 15.5 angezeigt.</span><span class="sxs-lookup"><span data-stu-id="6189c-114">Visual Studio versions prior to 15.5 show this dialog.</span></span>
* <span data-ttu-id="6189c-115">Wenn Sie kein Update ausführen können, klicken Sie auf **Hinzufügen**, und führen Sie die Schritte zum Hinzufügen eines Controllers erneut aus.</span><span class="sxs-lookup"><span data-stu-id="6189c-115">If you can't update, select **ADD**, and then follow the add controller steps again.</span></span>

<span data-ttu-id="6189c-116">Tippen Sie im Dialogfeld **Gerüst hinzufügen** auf **MVC-Controller mit Ansichten unter Verwendung von Entity Framework > Hinzufügen**.</span><span class="sxs-lookup"><span data-stu-id="6189c-116">In the **Add Scaffold** dialog, tap **MVC Controller with views, using Entity Framework > Add**.</span></span>

![Dialogfeld „Gerüst hinzufügen“](adding-model/_static/add_scaffold2.png)

<span data-ttu-id="6189c-118">Bearbeiten Sie das Dialogfeld **Controller hinzufügen**:</span><span class="sxs-lookup"><span data-stu-id="6189c-118">Complete the **Add Controller** dialog:</span></span>

* <span data-ttu-id="6189c-119">**Modellklasse:** *Movie (MvcMovie.Models)*</span><span class="sxs-lookup"><span data-stu-id="6189c-119">**Model class:** *Movie (MvcMovie.Models)*</span></span>
* <span data-ttu-id="6189c-120">**Datenkontextklasse:** Wählen Sie das Symbol **+** aus, und fügen Sie den Standardtyp **MvcMovie.Models.MvcMovieContext** hinzu</span><span class="sxs-lookup"><span data-stu-id="6189c-120">**Data context class:** Select the **+** icon and add the default **MvcMovie.Models.MvcMovieContext**</span></span>

![Datenkontext hinzufügen](adding-model/_static/dc.png)

* <span data-ttu-id="6189c-122">**Ansichten:** Lassen Sie den Standardwert der einzelnen Optionen aktiviert</span><span class="sxs-lookup"><span data-stu-id="6189c-122">**Views:** Keep the default of each option checked</span></span>
* <span data-ttu-id="6189c-123">**Controllername:** Behalten Sie den Standardwert *MoviesController* bei</span><span class="sxs-lookup"><span data-stu-id="6189c-123">**Controller name:** Keep the default *MoviesController*</span></span>
* <span data-ttu-id="6189c-124">Tippen Sie auf **Hinzufügen**.</span><span class="sxs-lookup"><span data-stu-id="6189c-124">Tap **Add**</span></span>

![Dialogfeld „Controller hinzufügen“](adding-model/_static/add_controller2.png)

<span data-ttu-id="6189c-126">Visual Studio erstellt Folgendes:</span><span class="sxs-lookup"><span data-stu-id="6189c-126">Visual Studio creates:</span></span>

* <span data-ttu-id="6189c-127">Eine Entity Framework Core-[Datenbankkontext-Klasse](xref:data/ef-mvc/intro#create-the-database-context) (*Data/MvcMovieContext.cs*)</span><span class="sxs-lookup"><span data-stu-id="6189c-127">An Entity Framework Core [database context class](xref:data/ef-mvc/intro#create-the-database-context) (*Data/MvcMovieContext.cs*)</span></span>
* <span data-ttu-id="6189c-128">Einen Movies-Controller (*Controllers/MoviesController.cs*)</span><span class="sxs-lookup"><span data-stu-id="6189c-128">A movies controller (*Controllers/MoviesController.cs*)</span></span>
* <span data-ttu-id="6189c-129">Razor-Ansichtsdateien für die Seiten „Erstellen“, „Löschen“, „Details“ „Bearbeiten“ und „Index“ (*Views/Movies/&ast;.cshtml*)</span><span class="sxs-lookup"><span data-stu-id="6189c-129">Razor view files for Create, Delete, Details, Edit, and Index pages (*Views/Movies/&ast;.cshtml*)</span></span>

<span data-ttu-id="6189c-130">Die automatische Erstellung des Datenbankkontexts und der [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete)-Aktionsmethoden (create, read, update and delete – Erstellen, Lesen, Aktualisieren und Löschen) und Ansichten wird als *Gerüstbau* bezeichnet.</span><span class="sxs-lookup"><span data-stu-id="6189c-130">The automatic creation of the database context and [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) (create, read, update, and delete) action methods and views is known as *scaffolding*.</span></span> <span data-ttu-id="6189c-131">Bald verfügen Sie über eine voll funktionsfähige Webanwendung, mit der Sie eine Filmdatenbank verwalten können.</span><span class="sxs-lookup"><span data-stu-id="6189c-131">You'll soon have a fully functional web application that lets you manage a movie database.</span></span>

<span data-ttu-id="6189c-132">Wenn Sie die App ausführen und auf den Link **Mvc Movie** klicken, erhalten Sie eine Fehlermeldung wie die Folgende:</span><span class="sxs-lookup"><span data-stu-id="6189c-132">If you run the app and click on the **Mvc Movie** link, you get an error similar to the following:</span></span>

```
An unhandled exception occurred while processing the request.

SqlException: Cannot open database "MvcMovieContext-<GUID removed>" requested by the login. The login failed.
Login failed for user 'Rick'.

System.Data.SqlClient.SqlInternalConnectionTds..ctor(DbConnectionPoolIdentity identity, SqlConnectionString 
```

<span data-ttu-id="6189c-133">Sie müssen die Datenbank erstellen und verwenden dazu das EF Core-Feature [Migrationen](xref:data/ef-mvc/migrations).</span><span class="sxs-lookup"><span data-stu-id="6189c-133">You need to create the database, and you'll use the EF Core [Migrations](xref:data/ef-mvc/migrations) feature to do that.</span></span> <span data-ttu-id="6189c-134">Mit „Migrationen“ können Sie eine Datenbank erstellen, die mit Ihrem Datenmodell übereinstimmt, und das Datenbankschema aktualisieren, wenn sich Ihr Datenmodell ändert.</span><span class="sxs-lookup"><span data-stu-id="6189c-134">Migrations lets you create a database that matches your data model and update the database schema when your data model changes.</span></span>

## <a name="add-ef-tooling-and-perform-initial-migration"></a><span data-ttu-id="6189c-135">Hinzufügen von EF-Tools und Ausführen der anfänglichen Migration</span><span class="sxs-lookup"><span data-stu-id="6189c-135">Add EF tooling and perform initial migration</span></span>

<span data-ttu-id="6189c-136">In diesem Abschnitt führen Sie folgende Aktionen mit der Paket-Manager-Konsole (Package Manager Console, PMC) aus:</span><span class="sxs-lookup"><span data-stu-id="6189c-136">In this section you'll use the Package Manager Console (PMC) to:</span></span>

* <span data-ttu-id="6189c-137">Fügen Sie das Entity Framework Core-Toolpaket hinzu.</span><span class="sxs-lookup"><span data-stu-id="6189c-137">Add the Entity Framework Core Tools package.</span></span> <span data-ttu-id="6189c-138">Dieses Paket ist erforderlich, um Migrationen hinzuzufügen und die Datenbank zu aktualisieren.</span><span class="sxs-lookup"><span data-stu-id="6189c-138">This package is required to add migrations and update the database.</span></span>
* <span data-ttu-id="6189c-139">Fügen Sie eine anfängliche Migration hinzu.</span><span class="sxs-lookup"><span data-stu-id="6189c-139">Add an initial migration.</span></span>
* <span data-ttu-id="6189c-140">Aktualisieren Sie die Datenbank mit der anfänglichen Migration.</span><span class="sxs-lookup"><span data-stu-id="6189c-140">Update the database with the initial migration.</span></span>

<span data-ttu-id="6189c-141">Öffnen Sie das Menü **Extras**. Wählen Sie dort **NuGet-Paket-Manager > Paket-Manager-Konsole** aus.</span><span class="sxs-lookup"><span data-stu-id="6189c-141">From the **Tools** menu, select **NuGet Package Manager > Package Manager Console**.</span></span>

<!-- following image shared with uid: tutorials/razor-pages/model -->
  ![PMC-Menü](adding-model/_static/pmc.png)

<span data-ttu-id="6189c-143">Geben Sie in der PMC die folgenden Befehle ein:</span><span class="sxs-lookup"><span data-stu-id="6189c-143">In the PMC, enter the following commands:</span></span>

``` PMC
Install-Package Microsoft.EntityFrameworkCore.Tools
Add-Migration Initial
Update-Database
```

<span data-ttu-id="6189c-144">**Hinweis:** Wenn ein Fehler mit dem Befehl `Install-Package` ausgegeben wird, öffnen Sie den NuGet-Paket-Manager, und suchen Sie nach dem `Microsoft.EntityFrameworkCore.Tools`-Paket.</span><span class="sxs-lookup"><span data-stu-id="6189c-144">**Note:** If you receive an error with the `Install-Package` command, open NuGet Package Manager and search for the `Microsoft.EntityFrameworkCore.Tools` package.</span></span> <span data-ttu-id="6189c-145">Dadurch können Sie das Paket installieren oder überprüfen, ob es bereits installiert ist.</span><span class="sxs-lookup"><span data-stu-id="6189c-145">This allows you to install the package or check if it's already installed.</span></span> <span data-ttu-id="6189c-146">Nutzen Sie bei Problemen mit PMC alternativ den [CLI-Ansatz](#cli).</span><span class="sxs-lookup"><span data-stu-id="6189c-146">Alternatively, see the [CLI approach](#cli) if you have problems with the PMC.</span></span>

<span data-ttu-id="6189c-147">Mit dem Befehl `Add-Migration` wird Code erstellt, um das anfängliche Datenbankschema zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="6189c-147">The `Add-Migration` command creates code to create the initial database schema.</span></span> <span data-ttu-id="6189c-148">Das Schema basiert auf dem Modell, das in der Datei *Data/MvcMovieContext.cs* für `DbContext` angegeben ist.</span><span class="sxs-lookup"><span data-stu-id="6189c-148">The schema is based on the model specified in the `DbContext`(In the *Data/MvcMovieContext.cs* file).</span></span> <span data-ttu-id="6189c-149">Das Argument `Initial` wird verwendet, um die Migrationen zu benennen.</span><span class="sxs-lookup"><span data-stu-id="6189c-149">The `Initial` argument is used to name the migrations.</span></span> <span data-ttu-id="6189c-150">Sie können einen beliebigen Namen auswählen, sollten aber der Konvention zufolge einen Namen verwenden, der die Migration beschreibt.</span><span class="sxs-lookup"><span data-stu-id="6189c-150">You can use any name, but by convention you choose a name that describes the migration.</span></span> <span data-ttu-id="6189c-151">Weitere Informationen finden Sie unter [Introduction to migrations (Einführung in Migrationen)](xref:data/ef-mvc/migrations#introduction-to-migrations).</span><span class="sxs-lookup"><span data-stu-id="6189c-151">See [Introduction to migrations](xref:data/ef-mvc/migrations#introduction-to-migrations) for more information.</span></span>

<span data-ttu-id="6189c-152">Mit dem Befehl `Update-Database` führen Sie in der Datei *Migrations/\<time-stamp>_Initial.cs* die Methode `Up` aus, mit der die Datenbank erstellt wird.</span><span class="sxs-lookup"><span data-stu-id="6189c-152">The `Update-Database` command runs the `Up` method in the *Migrations/\<time-stamp>_Initial.cs* file, which creates the database.</span></span>

<a name="cli"></a> <span data-ttu-id="6189c-153">Sie können die vorherigen Schritte über die Befehlszeilenschnittstelle (CLI) statt mit PMC ausführen:</span><span class="sxs-lookup"><span data-stu-id="6189c-153">You can perform the preceeding steps using the command-line interface (CLI) rather than the PMC:</span></span>

* <span data-ttu-id="6189c-154">Fügen Sie [EF Core-Tools](xref:data/ef-mvc/migrations#entity-framework-core-nuget-packages-for-migrations) zur *CSPROJ*-Datei hinzu.</span><span class="sxs-lookup"><span data-stu-id="6189c-154">Add [EF Core tooling](xref:data/ef-mvc/migrations#entity-framework-core-nuget-packages-for-migrations) to the *.csproj* file.</span></span>
* <span data-ttu-id="6189c-155">Führen Sie die folgenden Befehle über die Konsole aus (im Projektverzeichnis):</span><span class="sxs-lookup"><span data-stu-id="6189c-155">Run the following commands from the console (in the project directory):</span></span>

  ```console
  dotnet ef migrations add Initial
  dotnet ef database update
  ```     
  

[!INCLUDE[adding-model](../../includes/mvc-intro/adding-model3.md)]

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Startup.cs?name=ConfigureServices&highlight=6-7)]

[!INCLUDE[adding-model](../../includes/mvc-intro/adding-model4.md)]

![IntelliSense-Kontextmenü für ein Modellelement mit den verfügbaren Eigenschaften für ID, Preis, Veröffentlichungsdatum und Titel](adding-model/_static/ints.png)

## <a name="additional-resources"></a><span data-ttu-id="6189c-157">Zusätzliche Ressourcen</span><span class="sxs-lookup"><span data-stu-id="6189c-157">Additional resources</span></span>

* [<span data-ttu-id="6189c-158">Taghilfsprogramme</span><span class="sxs-lookup"><span data-stu-id="6189c-158">Tag Helpers</span></span>](xref:mvc/views/tag-helpers/intro)
* [<span data-ttu-id="6189c-159">Globalisierung und Lokalisierung</span><span class="sxs-lookup"><span data-stu-id="6189c-159">Globalization and localization</span></span>](xref:fundamentals/localization)

>[!div class="step-by-step"]
<span data-ttu-id="6189c-160">[Vorheriges Tutorial: Hinzufügen einer Ansicht](adding-view.md)
[Nächstes Tutorial: Arbeiten mit SQL](working-with-sql.md)</span><span class="sxs-lookup"><span data-stu-id="6189c-160">[Previous Adding a View](adding-view.md)
[Next Working with SQL](working-with-sql.md)</span></span>  
