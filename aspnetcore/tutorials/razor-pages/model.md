---
title: Hinzufügen eines Modells zu einer App mit Razor-Seiten in ASP.NET Core
author: rick-anderson
description: Erfahren Sie, wie Sie Klassen für das Verwalten von Filmen mithilfe von Entity Framework Core (EF Core) zu einer Datenbank hinzufügen.
ms.author: riande
ms.date: 05/30/2018
uid: tutorials/razor-pages/model
ms.openlocfilehash: de82738509bb009f030a02e28904e3155088fa6a
ms.sourcegitcommit: b2723654af4969a24545f09ebe32004cb5e84a96
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/18/2018
ms.locfileid: "46011357"
---
# <a name="add-a-model-to-a-razor-pages-app-in-aspnet-core"></a><span data-ttu-id="9d8bb-103">Hinzufügen eines Modells zu einer App mit Razor-Seiten in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="9d8bb-103">Add a model to a Razor Pages app in ASP.NET Core</span></span>

::: moniker range=">= aspnetcore-2.1"

[!INCLUDE [model1](~/includes/RP/model1.md)]

## <a name="add-a-data-model"></a><span data-ttu-id="9d8bb-104">Hinzufügen eines Datenmodells</span><span class="sxs-lookup"><span data-stu-id="9d8bb-104">Add a data model</span></span>

<span data-ttu-id="9d8bb-105">Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf das Projekt **RazorPagesMovie** > **Hinzufügen** > **Neuer Ordner**.</span><span class="sxs-lookup"><span data-stu-id="9d8bb-105">In Solution Explorer, right-click the **RazorPagesMovie** project > **Add** > **New Folder**.</span></span> <span data-ttu-id="9d8bb-106">Geben Sie dem Ordner den Namen *Modelle*.</span><span class="sxs-lookup"><span data-stu-id="9d8bb-106">Name the folder *Models*.</span></span>

<span data-ttu-id="9d8bb-107">Klicken Sie mit der rechten Maustaste auf den Ordner *Modelle*.</span><span class="sxs-lookup"><span data-stu-id="9d8bb-107">Right click the *Models* folder.</span></span> <span data-ttu-id="9d8bb-108">Wählen Sie **Hinzufügen** > **Klasse** aus.</span><span class="sxs-lookup"><span data-stu-id="9d8bb-108">Select **Add** > **Class**.</span></span> <span data-ttu-id="9d8bb-109">Nennen Sie die Klasse **Movie**, und fügen Sie Ihr die folgenden Eigenschaften hinzu:</span><span class="sxs-lookup"><span data-stu-id="9d8bb-109">Name the class **Movie** and add the following properties:</span></span>

<span data-ttu-id="9d8bb-110">Ersetzen Sie den Inhalt der Klasse `Movie` durch folgenden Code:</span><span class="sxs-lookup"><span data-stu-id="9d8bb-110">Replace the contents of the `Movie` class with the following code:</span></span>

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie21/Models/Movie1.cs?name=snippet)]

## <a name="scaffold-the-movie-model"></a><span data-ttu-id="9d8bb-111">Erstellen des Gerüsts für das Filmmodell</span><span class="sxs-lookup"><span data-stu-id="9d8bb-111">Scaffold the movie model</span></span>

<span data-ttu-id="9d8bb-112">In diesem Abschnitt wird das Gerüst für das Filmmodell erstellt.</span><span class="sxs-lookup"><span data-stu-id="9d8bb-112">In this section, the movie model is scaffolded.</span></span> <span data-ttu-id="9d8bb-113">Mit dem Tool für den Gerüstbau werden Seiten für die Vorgänge „Create“ (Erstellen), „Read“ (Lesen), „Update“ (Aktualisieren) und „Delete“ (Löschen), kurz CRUD-Vorgänge, für das Filmmodell erstellt.</span><span class="sxs-lookup"><span data-stu-id="9d8bb-113">That is, the scaffolding tool produces pages for Create, Read, Update, and Delete (CRUD) operations for the movie model.</span></span>

<span data-ttu-id="9d8bb-114">Erstellen Sie den Ordner *Pages/Movies*:</span><span class="sxs-lookup"><span data-stu-id="9d8bb-114">Create a *Pages/Movies* folder:</span></span>

* <span data-ttu-id="9d8bb-115">Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf den Ordner *Pages* > **Hinzufügen** > **Neuer Ordner**.</span><span class="sxs-lookup"><span data-stu-id="9d8bb-115">In **Solution Explorer**, right click on the *Pages* folder > **Add** > **New Folder**.</span></span>
* <span data-ttu-id="9d8bb-116">Geben Sie dem Ordner den Namen *Movies*</span><span class="sxs-lookup"><span data-stu-id="9d8bb-116">Name the folder *Movies*</span></span>

<span data-ttu-id="9d8bb-117">Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf den Ordner *Pages/Movies* > **Hinzufügen** > **Neues Gerüstelement**.</span><span class="sxs-lookup"><span data-stu-id="9d8bb-117">In **Solution Explorer**, right click on the *Pages/Movies* folder > **Add** > **New Scaffolded Item**.</span></span>

![Abbildung der vorherigen Anweisungen.](model/_static/sca.png)

<span data-ttu-id="9d8bb-119">Wählen Sie im Dialogfeld **Gerüst hinzufügen** den Eintrag **Razor Pages mit Entity Framework (CRUD)** > **HINZUFÜGEN** aus.</span><span class="sxs-lookup"><span data-stu-id="9d8bb-119">In the **Add Scaffold** dialog, select **Razor Pages using Entity Framework (CRUD)** > **ADD**.</span></span>

![Abbildung der vorherigen Anweisungen.](model/_static/add_scaffold.png)

<span data-ttu-id="9d8bb-121">Vervollständigen Sie das Dialogfeld **Razor Pages mit Entity Framework (CRUD) hinzufügen**:</span><span class="sxs-lookup"><span data-stu-id="9d8bb-121">Complete the **Add Razor Pages using Entity Framework (CRUD)** dialog:</span></span>

* <span data-ttu-id="9d8bb-122">Wählen Sie in der Dropdownliste **Modellklasse** den Eintrag **Film (RazorPagesMovie.Models)** aus.</span><span class="sxs-lookup"><span data-stu-id="9d8bb-122">In the **Model class** drop down, select **Movie (RazorPagesMovie.Models)**.</span></span>
* <span data-ttu-id="9d8bb-123">Wählen Sie in der Zeile **Datenkontextklasse** das Zeichen **+** (Plus) aus, und akzeptieren Sie den generierten Namen **RazorPagesMovie.Models.RazorPagesMovieContext**.</span><span class="sxs-lookup"><span data-stu-id="9d8bb-123">In the **Data context class** row, select the **+** (plus) sign and accept the generated name **RazorPagesMovie.Models.RazorPagesMovieContext**.</span></span>
* <span data-ttu-id="9d8bb-124">Wählen Sie in der Dropdownliste **Datenkontextklasse** den Eintrag **RazorPagesMovie.Models.RazorPagesMovieContext** aus.</span><span class="sxs-lookup"><span data-stu-id="9d8bb-124">In the **Data context class** drop down, select **RazorPagesMovie.Models.RazorPagesMovieContext**</span></span>
* <span data-ttu-id="9d8bb-125">Wählen Sie **Hinzufügen** aus.</span><span class="sxs-lookup"><span data-stu-id="9d8bb-125">Select **Add**.</span></span>

![Abbildung der vorherigen Anweisungen.](model/_static/arp.png)

<span data-ttu-id="9d8bb-127">Der Gerüstprozess hat folgende Dateien erstellt und geändert:</span><span class="sxs-lookup"><span data-stu-id="9d8bb-127">The scaffold process created and changed the following files:</span></span>

### <a name="files-created"></a><span data-ttu-id="9d8bb-128">Erstellte Dateien</span><span class="sxs-lookup"><span data-stu-id="9d8bb-128">Files created</span></span>

* <span data-ttu-id="9d8bb-129">*Pages/Movies/Create.cshtml.cs* ( bzw. /Delete, /Details, /Edit, /Index).</span><span class="sxs-lookup"><span data-stu-id="9d8bb-129">*Pages/Movies* Create, Delete, Details, Edit, Index.</span></span> <span data-ttu-id="9d8bb-130">Diese Seiten werden im nächsten Tutorial ausführlich erläutert.</span><span class="sxs-lookup"><span data-stu-id="9d8bb-130">These pages are detailed in the next tutorial.</span></span>
* <span data-ttu-id="9d8bb-131">*Data/RazorPagesMovieContext.cs*</span><span class="sxs-lookup"><span data-stu-id="9d8bb-131">*Data/RazorPagesMovieContext.cs*</span></span>

### <a name="files-updates"></a><span data-ttu-id="9d8bb-132">Aktualisierte Dateien</span><span class="sxs-lookup"><span data-stu-id="9d8bb-132">Files updates</span></span>

* <span data-ttu-id="9d8bb-133">*Startup.cs:* Die Änderungen an dieser Datei werden im nächsten Abschnitt ausführlich erläutert.</span><span class="sxs-lookup"><span data-stu-id="9d8bb-133">*Startup.cs*: Changes to this file are detailed in the next section.</span></span>
* <span data-ttu-id="9d8bb-134">*appsettings.json*: Die Verbindungszeichenfolge, die zum Herstellen einer Verbindung mit einer lokalen Datenbank verwendet wird, wurde hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="9d8bb-134">*appsettings.json*: The connection string used to connect to a local database is added.</span></span>

## <a name="examine-the-context-registered-with-dependency-injection"></a><span data-ttu-id="9d8bb-135">Überprüfen des mit Dependency Injection registrierten Kontexts</span><span class="sxs-lookup"><span data-stu-id="9d8bb-135">Examine the context registered with dependency injection</span></span>

<span data-ttu-id="9d8bb-136">ASP.NET Core wird mit [Dependency Injection](xref:fundamentals/dependency-injection) erstellt.</span><span class="sxs-lookup"><span data-stu-id="9d8bb-136">ASP.NET Core is built with [dependency injection](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="9d8bb-137">Dienste (z.B. der Datenbankkontext Entity Framework Core) werden über Dependency Injection beim Anwendungsstart registriert.</span><span class="sxs-lookup"><span data-stu-id="9d8bb-137">Services (such as the EF Core DB context) are registered with dependency injection during application startup.</span></span> <span data-ttu-id="9d8bb-138">Komponenten, die diese Dienste erfordern (z.B. Razor Pages), werden von diesen Diensten über Konstruktorparameter bereitgestellt.</span><span class="sxs-lookup"><span data-stu-id="9d8bb-138">Components that require these services (such as Razor Pages) are provided these services via constructor parameters.</span></span> <span data-ttu-id="9d8bb-139">Der Konstruktorcode, der eine Datenbankkontext-Instanz abruft, wird später in diesem Tutorial erläutert.</span><span class="sxs-lookup"><span data-stu-id="9d8bb-139">The constructor code that gets a DB context instance is shown later in the tutorial.</span></span>

<span data-ttu-id="9d8bb-140">Das Gerüstbautool hat automatisch einen Datenbankkontext erstellt und diesen mit dem Dependency Injection-Container registriert.</span><span class="sxs-lookup"><span data-stu-id="9d8bb-140">The scaffolding tool automatically created a DB context and registered it with the dependency injection container.</span></span>

<span data-ttu-id="9d8bb-141">Untersuchen Sie die Methode `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="9d8bb-141">Examine the `Startup.ConfigureServices` method.</span></span> <span data-ttu-id="9d8bb-142">Die hervorgehobene Zeile wurde vom Gerüst hinzugefügt:</span><span class="sxs-lookup"><span data-stu-id="9d8bb-142">The highlighted line was added by the scaffolder:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie21/Startup.cs?name=snippet_ConfigureServices&highlight=12-13)]

<span data-ttu-id="9d8bb-143">Bei der Datenbankkontextklasse handelt es sich um die Hauptklasse, die die Entity Framework Core-Funktionen für ein angegebenes Datenmodell koordiniert.</span><span class="sxs-lookup"><span data-stu-id="9d8bb-143">The main class that coordinates EF Core functionality for a given data model is the DB context class.</span></span> <span data-ttu-id="9d8bb-144">Der Datenkontext wird von [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext) abgeleitet.</span><span class="sxs-lookup"><span data-stu-id="9d8bb-144">The data context is derived from [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext).</span></span> <span data-ttu-id="9d8bb-145">Der Datenkontext gibt an, welche Entitäten im Datenmodell enthalten sind.</span><span class="sxs-lookup"><span data-stu-id="9d8bb-145">The data context specifies which entities are included in the data model.</span></span> <span data-ttu-id="9d8bb-146">In diesem Projekt heißt die Klasse `RazorPagesMovieContext`.</span><span class="sxs-lookup"><span data-stu-id="9d8bb-146">In this project, the class is named `RazorPagesMovieContext`.</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie21/Data/RazorPagesMovieContext.cs)]

<span data-ttu-id="9d8bb-147">Der vorangehende Code erstellt eine [DbSet\<Movie>](/dotnet/api/microsoft.entityframeworkcore.dbset-1)-Eigenschaft für die Entitätenmenge.</span><span class="sxs-lookup"><span data-stu-id="9d8bb-147">The preceding code creates a [DbSet\<Movie>](/dotnet/api/microsoft.entityframeworkcore.dbset-1) property for the entity set.</span></span> <span data-ttu-id="9d8bb-148">In der Terminologie von Entity Framework entspricht eine Entitätenmenge in der Regel einer Datenbanktabelle.</span><span class="sxs-lookup"><span data-stu-id="9d8bb-148">In Entity Framework terminology, an entity set typically corresponds to a database table.</span></span> <span data-ttu-id="9d8bb-149">Entitäten entsprechen Zeilen in Tabellen.</span><span class="sxs-lookup"><span data-stu-id="9d8bb-149">An entity corresponds to a row in the table.</span></span>

<span data-ttu-id="9d8bb-150">Der Name der Verbindungszeichenfolge wird an den Kontext übergeben, indem Sie eine Methode auf einem [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions)-Objekt aufrufen.</span><span class="sxs-lookup"><span data-stu-id="9d8bb-150">The name of the connection string is passed in to the context by calling a method on a [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) object.</span></span> <span data-ttu-id="9d8bb-151">Für die lokale Entwicklung liest das [ASP.NET Core-Konfigurationssystem](xref:fundamentals/configuration/index) die Verbindungszeichenfolge aus der *appsettings.json*-Datei.</span><span class="sxs-lookup"><span data-stu-id="9d8bb-151">For local development, the [ASP.NET Core configuration system](xref:fundamentals/configuration/index) reads the connection string from the *appsettings.json* file.</span></span>

<a name="pmc"></a>
## <a name="perform-initial-migration"></a><span data-ttu-id="9d8bb-152">Ausführen der anfänglichen Migration</span><span class="sxs-lookup"><span data-stu-id="9d8bb-152">Perform initial migration</span></span>

<span data-ttu-id="9d8bb-153">In diesem Abschnitt führen Sie folgende Aktionen mit der Paket-Manager-Konsole (Package Manager Console, PMC) aus:</span><span class="sxs-lookup"><span data-stu-id="9d8bb-153">In this section, you use the Package Manager Console (PMC) to:</span></span>

* <span data-ttu-id="9d8bb-154">Fügen Sie eine anfängliche Migration hinzu.</span><span class="sxs-lookup"><span data-stu-id="9d8bb-154">Add an initial migration.</span></span>
* <span data-ttu-id="9d8bb-155">Aktualisieren Sie die Datenbank mit der anfänglichen Migration.</span><span class="sxs-lookup"><span data-stu-id="9d8bb-155">Update the database with the initial migration.</span></span>

<span data-ttu-id="9d8bb-156">Wählen Sie im Menü **Tools** die Option **NuGet-Paket-Manager** > **Paket-Manager-Konsole** aus.</span><span class="sxs-lookup"><span data-stu-id="9d8bb-156">From the **Tools** menu, select **NuGet Package Manager** > **Package Manager Console**.</span></span>

  ![PMC-Menü](../first-mvc-app/adding-model/_static/pmc.png)

<span data-ttu-id="9d8bb-158">Geben Sie in der PMC die folgenden Befehle ein:</span><span class="sxs-lookup"><span data-stu-id="9d8bb-158">In the PMC, enter the following commands:</span></span>

```PMC
Add-Migration Initial
Update-Database
```

<span data-ttu-id="9d8bb-159">Alternativ können die folgenden .NET Core-CLI-Befehle vom Projektordner aus verwendet werden:</span><span class="sxs-lookup"><span data-stu-id="9d8bb-159">Alternatively, the following .NET Core CLI commands can be used from the project folder:</span></span>

```console
dotnet ef migrations add Initial
dotnet ef database update
```

<span data-ttu-id="9d8bb-160">Die folgende Warnmeldung können Sie ignorieren, Sie werden dies in einem späteren Tutorial beheben:</span><span class="sxs-lookup"><span data-stu-id="9d8bb-160">Ignore the following warning message, you fix that in a a later tutorial:</span></span>

`Microsoft.EntityFrameworkCore.Model.Validation[30000]`

      *No type was specified for the decimal column 'Price' on entity type 'Movie'. This will cause values to be silently truncated if they do not fit in the default precision and scale. Explicitly specify the SQL server column type that can accommodate all the values using 'ForHasColumnType()'.*

<span data-ttu-id="9d8bb-161">Mit dem Befehl `Add-Migration` wird Code generiert, um das anfängliche Datenbankschema zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="9d8bb-161">The `Add-Migration` command generates code to create the initial database schema.</span></span> <span data-ttu-id="9d8bb-162">Das Schema basiert auf dem Modell, das in der Datei *Data/RazorPagesMovieContext.cs* für `RazorPagesMovieContext` angegeben ist.</span><span class="sxs-lookup"><span data-stu-id="9d8bb-162">The schema is based on the model specified in the `RazorPagesMovieContext` (In the *Data/RazorPagesMovieContext.cs* file).</span></span> <span data-ttu-id="9d8bb-163">Das Argument `Initial` wird verwendet, um die Migrationen zu benennen.</span><span class="sxs-lookup"><span data-stu-id="9d8bb-163">The `Initial` argument is used to name the migrations.</span></span> <span data-ttu-id="9d8bb-164">Sie können einen beliebigen Namen auswählen, sollten aber der Konvention zufolge einen Namen verwenden, der die Migration beschreibt.</span><span class="sxs-lookup"><span data-stu-id="9d8bb-164">You can use any name, but by convention you choose a name that describes the migration.</span></span> <span data-ttu-id="9d8bb-165">Weitere Informationen finden Sie unter [Introduction to migrations (Einführung in Migrationen)](xref:data/ef-mvc/migrations#introduction-to-migrations).</span><span class="sxs-lookup"><span data-stu-id="9d8bb-165">See [Introduction to migrations](xref:data/ef-mvc/migrations#introduction-to-migrations) for more information.</span></span>

<span data-ttu-id="9d8bb-166">Mit dem Befehl `Update-Database` führen Sie die Methode `Up` in der Datei *Migrations/{time-stamp}_InitialCreate.cs* aus, mit der die Datenbank erstellt wird.</span><span class="sxs-lookup"><span data-stu-id="9d8bb-166">The `Update-Database` command runs the `Up` method in the *Migrations/{time-stamp}_InitialCreate.cs* file, which creates the database.</span></span>

<span data-ttu-id="9d8bb-167">Wenn Sie eine Fehlermeldung erhalten, müssen Sie Folgendes tun:</span><span class="sxs-lookup"><span data-stu-id="9d8bb-167">If you get the error:</span></span>

<span data-ttu-id="9d8bb-168">SqlException: Die bei der Anmeldung angeforderte Datenbank „RazorPagesMovieContext-GUID“ kann nicht geöffnet werden.</span><span class="sxs-lookup"><span data-stu-id="9d8bb-168">SqlException: Cannot open database "RazorPagesMovieContext-GUID" requested by the login.</span></span> <span data-ttu-id="9d8bb-169">Die Anmeldung ist fehlgeschlagen.</span><span class="sxs-lookup"><span data-stu-id="9d8bb-169">The login failed.</span></span>
<span data-ttu-id="9d8bb-170">Fehler bei der Anmeldung des Benutzers „Benutzername“.</span><span class="sxs-lookup"><span data-stu-id="9d8bb-170">Login failed for user 'User-name'.</span></span>

<span data-ttu-id="9d8bb-171">Sie haben den [Migrationsschritt](#pmc) verpasst.</span><span class="sxs-lookup"><span data-stu-id="9d8bb-171">You missed the [migrations step](#pmc).</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!INCLUDE [model1](~/includes/RP/model1.md)]

## <a name="add-a-data-model"></a><span data-ttu-id="9d8bb-172">Hinzufügen eines Datenmodells</span><span class="sxs-lookup"><span data-stu-id="9d8bb-172">Add a data model</span></span>

<span data-ttu-id="9d8bb-173">Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf das Projekt **RazorPagesMovie** > **Hinzufügen** > **Neuer Ordner**.</span><span class="sxs-lookup"><span data-stu-id="9d8bb-173">In Solution Explorer, right-click the **RazorPagesMovie** project > **Add** > **New Folder**.</span></span> <span data-ttu-id="9d8bb-174">Geben Sie dem Ordner den Namen *Modelle*.</span><span class="sxs-lookup"><span data-stu-id="9d8bb-174">Name the folder *Models*.</span></span>

<span data-ttu-id="9d8bb-175">Klicken Sie mit der rechten Maustaste auf den Ordner *Modelle*.</span><span class="sxs-lookup"><span data-stu-id="9d8bb-175">Right click the *Models* folder.</span></span> <span data-ttu-id="9d8bb-176">Wählen Sie **Hinzufügen** > **Klasse** aus.</span><span class="sxs-lookup"><span data-stu-id="9d8bb-176">Select **Add** > **Class**.</span></span> <span data-ttu-id="9d8bb-177">Nennen Sie die Klasse **Movie**, und fügen Sie Ihr die folgenden Eigenschaften hinzu:</span><span class="sxs-lookup"><span data-stu-id="9d8bb-177">Name the class **Movie** and add the following properties:</span></span>

[!INCLUDE [model 2](~/includes/RP/model2.md)]

<a name="cs"></a>
### <a name="add-a-database-connection-string"></a><span data-ttu-id="9d8bb-178">Hinzufügen einer Datenbank-Verbindungszeichenfolge</span><span class="sxs-lookup"><span data-stu-id="9d8bb-178">Add a database connection string</span></span>

<span data-ttu-id="9d8bb-179">Fügen Sie der Datei *appsettings.json* eine Verbindungszeichenfolge hinzu.</span><span class="sxs-lookup"><span data-stu-id="9d8bb-179">Add a connection string to the *appsettings.json* file.</span></span>

[!code-json[](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/appsettings.json?highlight=8-10)]

<a name="reg"></a>
###  <a name="register-the-database-context"></a><span data-ttu-id="9d8bb-180">Registrieren des Datenbankkontexts</span><span class="sxs-lookup"><span data-stu-id="9d8bb-180">Register the database context</span></span>

<span data-ttu-id="9d8bb-181">Registrieren Sie den Datenbankkontext mit dem [Abhängigkeitsinjektionscontainer](xref:fundamentals/dependency-injection) in der [ConfigureServices-Methode der Startup-Klasse](xref:fundamentals/startup#the-startup-class) (*Startup.cs*):</span><span class="sxs-lookup"><span data-stu-id="9d8bb-181">Register the database context with the [dependency injection](xref:fundamentals/dependency-injection) container in the [ConfigureServices method of the Startup class](xref:fundamentals/startup#the-startup-class) (*Startup.cs*):</span></span>

[!code-csharp[](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Startup.cs?name=snippet_ConfigureServices&highlight=3-5,7-9)]

<span data-ttu-id="9d8bb-182">Erstellen Sie das Projekt, um sicherzustellen, dass keine Fehler vorliegen.</span><span class="sxs-lookup"><span data-stu-id="9d8bb-182">Build the project to verify you don't have any errors.</span></span>

<a name="pmc"></a>
## <a name="add-scaffold-tooling-and-perform-initial-migration"></a><span data-ttu-id="9d8bb-183">Hinzufügen von Tools für den Gerüstbau und Ausführen der anfänglichen Migration</span><span class="sxs-lookup"><span data-stu-id="9d8bb-183">Add scaffold tooling and perform initial migration</span></span>

<span data-ttu-id="9d8bb-184">In diesem Abschnitt führen Sie folgende Aktionen mit der Paket-Manager-Konsole (Package Manager Console, PMC) aus:</span><span class="sxs-lookup"><span data-stu-id="9d8bb-184">In this section, you use the Package Manager Console (PMC) to:</span></span>

* <span data-ttu-id="9d8bb-185">Fügen Sie das Visual Studio-Paket für die Webcodegenerierung hinzu.</span><span class="sxs-lookup"><span data-stu-id="9d8bb-185">Add the Visual Studio web code generation package.</span></span> <span data-ttu-id="9d8bb-186">Dieses Paket ist für die Ausführung der Gerüstbau-Engine erforderlich.</span><span class="sxs-lookup"><span data-stu-id="9d8bb-186">This package is required to run the scaffolding engine.</span></span>
* <span data-ttu-id="9d8bb-187">Fügen Sie eine anfängliche Migration hinzu.</span><span class="sxs-lookup"><span data-stu-id="9d8bb-187">Add an initial migration.</span></span>
* <span data-ttu-id="9d8bb-188">Aktualisieren Sie die Datenbank mit der anfänglichen Migration.</span><span class="sxs-lookup"><span data-stu-id="9d8bb-188">Update the database with the initial migration.</span></span>

<span data-ttu-id="9d8bb-189">Wählen Sie im Menü **Tools** die Option **NuGet-Paket-Manager** > **Paket-Manager-Konsole** aus.</span><span class="sxs-lookup"><span data-stu-id="9d8bb-189">From the **Tools** menu, select **NuGet Package Manager** > **Package Manager Console**.</span></span>

  ![PMC-Menü](../first-mvc-app/adding-model/_static/pmc.png)

<span data-ttu-id="9d8bb-191">Geben Sie in der PMC die folgenden Befehle ein:</span><span class="sxs-lookup"><span data-stu-id="9d8bb-191">In the PMC, enter the following commands:</span></span>

```powershell
Install-Package Microsoft.VisualStudio.Web.CodeGeneration.Design -Version 2.0.3
Add-Migration Initial
Update-Database
```

<span data-ttu-id="9d8bb-192">Alternativ können folgende Befehle der .NET Core-CLI verwendet werden:</span><span class="sxs-lookup"><span data-stu-id="9d8bb-192">Alternatively, the following .NET Core CLI commands can be used:</span></span>

```console
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet ef migrations add Initial
dotnet ef database update
```

<span data-ttu-id="9d8bb-193">Die folgende Meldung können Sie ignorieren:</span><span class="sxs-lookup"><span data-stu-id="9d8bb-193">Ignore the following message:</span></span>

    `Microsoft.EntityFrameworkCore.Model.Validation[30000]`

      *No type was specified for the decimal column 'Price' on entity type 'Movie'. This will cause values to be silently truncated if they do not fit in the default precision and scale. Explicitly specify the SQL server column type that can accommodate all the values using 'ForHasColumnType()'*

<span data-ttu-id="9d8bb-194">Dieser Fehler wird im nächsten Tutorial behoben.</span><span class="sxs-lookup"><span data-stu-id="9d8bb-194">You fix that in the next tutorial.</span></span>

<span data-ttu-id="9d8bb-195">Mit dem Befehl `Install-Package` werden die für die Gerüstbau-Engine benötigten Tools installiert.</span><span class="sxs-lookup"><span data-stu-id="9d8bb-195">The `Install-Package` command installs the tooling required to run the scaffolding engine.</span></span>

<span data-ttu-id="9d8bb-196">Mit dem Befehl `Add-Migration` wird Code generiert, um das anfängliche Datenbankschema zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="9d8bb-196">The `Add-Migration` command generates code to create the initial database schema.</span></span> <span data-ttu-id="9d8bb-197">Das Schema basiert auf dem Modell, das in der Datei *Modelle/MovieContext.cs* für `DbContext` angegeben ist.</span><span class="sxs-lookup"><span data-stu-id="9d8bb-197">The schema is based on the model specified in the `DbContext` (In the *Models/MovieContext.cs* file).</span></span> <span data-ttu-id="9d8bb-198">Das Argument `Initial` wird verwendet, um die Migrationen zu benennen.</span><span class="sxs-lookup"><span data-stu-id="9d8bb-198">The `Initial` argument is used to name the migrations.</span></span> <span data-ttu-id="9d8bb-199">Sie können einen beliebigen Namen auswählen, sollten aber der Konvention zufolge einen Namen verwenden, der die Migration beschreibt.</span><span class="sxs-lookup"><span data-stu-id="9d8bb-199">You can use any name, but by convention you choose a name that describes the migration.</span></span> <span data-ttu-id="9d8bb-200">Weitere Informationen finden Sie unter [Introduction to migrations (Einführung in Migrationen)](xref:data/ef-mvc/migrations#introduction-to-migrations).</span><span class="sxs-lookup"><span data-stu-id="9d8bb-200">See [Introduction to migrations](xref:data/ef-mvc/migrations#introduction-to-migrations) for more information.</span></span>

<span data-ttu-id="9d8bb-201">Mit dem Befehl `Update-Database` führen Sie die Methode `Up` in der Datei *Migrations/{time-stamp}_InitialCreate.cs* aus, mit der die Datenbank erstellt wird.</span><span class="sxs-lookup"><span data-stu-id="9d8bb-201">The `Update-Database` command runs the `Up` method in the *Migrations/{time-stamp}_InitialCreate.cs* file, which creates the database.</span></span>

[!INCLUDE [model 4windows](~/includes/RP/model4Win.md)]

[!INCLUDE [model 4](~/includes/RP/model4tbl.md)]

::: moniker-end

<a name="test"></a>

### <a name="test-the-app"></a><span data-ttu-id="9d8bb-202">Testen der App</span><span class="sxs-lookup"><span data-stu-id="9d8bb-202">Test the app</span></span>

* <span data-ttu-id="9d8bb-203">Führen Sie die App aus, und fügen Sie `/Movies` an die URL im Browser an (`http://localhost:port/movies`).</span><span class="sxs-lookup"><span data-stu-id="9d8bb-203">Run the app and append `/Movies` to the URL in the browser (`http://localhost:port/movies`).</span></span>
* <span data-ttu-id="9d8bb-204">Testen Sie den Link **Create** (Erstellen).</span><span class="sxs-lookup"><span data-stu-id="9d8bb-204">Test the **Create** link.</span></span>

  ![Seite „Create“](../../tutorials/razor-pages/model/_static/conan.png)

<a name="scaffold"></a>

* <span data-ttu-id="9d8bb-206">Testen Sie die Links **Edit** (Bearbeiten), **Details** und **Delete** (Löschen).</span><span class="sxs-lookup"><span data-stu-id="9d8bb-206">Test the **Edit**, **Details**, and **Delete** links.</span></span>

<span data-ttu-id="9d8bb-207">Überprüfen Sie bei der Auslösung einer SQL-Ausnahme, ob Migrationen ausgeführt wurden und die Datenbank aktualisiert wurde.</span><span class="sxs-lookup"><span data-stu-id="9d8bb-207">If you get a SQL exception, verify you have run migrations and updated the database.</span></span>

<span data-ttu-id="9d8bb-208">Im nächsten Tutorial finden Sie Erläuterungen zu den Dateien, die durch den Gerüstbau erstellt wurden.</span><span class="sxs-lookup"><span data-stu-id="9d8bb-208">The next tutorial explains the files created by scaffolding.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="9d8bb-209">[Zurück: Erste Schritte](xref:tutorials/razor-pages/razor-pages-start)
> [Weiter: Gerüstbau mit Razor-Seiten](xref:tutorials/razor-pages/page)</span><span class="sxs-lookup"><span data-stu-id="9d8bb-209">[Previous: Get Started](xref:tutorials/razor-pages/razor-pages-start)
[Next: Scaffolded Razor Pages](xref:tutorials/razor-pages/page)</span></span>
