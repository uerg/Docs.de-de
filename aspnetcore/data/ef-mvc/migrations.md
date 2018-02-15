---
title: 'ASP.NET Core MVC mit Entity Framework Core (EF Core): Migrationen (4 von 10)'
author: tdykstra
description: "In diesem Tutorial verwenden Sie zunächst die EF Core-Migrationsfeatures für die Verwaltung von Datenmodelländerungen in einer ASP.NET Core MVC-Anwendung."
manager: wpickett
ms.author: tdykstra
ms.date: 03/15/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: data/ef-mvc/migrations
ms.openlocfilehash: fd466af8a73bf4c568fafe7e7fdcaa82021624da
ms.sourcegitcommit: 18d1dc86770f2e272d93c7e1cddfc095c5995d9e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/31/2018
---
# <a name="migrations---ef-core-with-aspnet-core-mvc-tutorial-4-of-10"></a><span data-ttu-id="4a575-103">Migrationen: Tutorial zu EF Core mit ASP.NET Core MVC (4 von 10)</span><span class="sxs-lookup"><span data-stu-id="4a575-103">Migrations - EF Core with ASP.NET Core MVC tutorial (4 of 10)</span></span>

<span data-ttu-id="4a575-104">Von [Tom Dykstra](https://github.com/tdykstra) und [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="4a575-104">By [Tom Dykstra](https://github.com/tdykstra) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="4a575-105">Die Contoso University-Beispielwebanwendung veranschaulicht, wie ASP.NET Core MVC-Webanwendungen mit Entity Framework Core und Visual Studio erstellt werden.</span><span class="sxs-lookup"><span data-stu-id="4a575-105">The Contoso University sample web application demonstrates how to create ASP.NET Core MVC web applications using Entity Framework Core and Visual Studio.</span></span> <span data-ttu-id="4a575-106">Informationen zu dieser Tutorialreihe finden Sie im [ersten Tutorial der Reihe](intro.md).</span><span class="sxs-lookup"><span data-stu-id="4a575-106">For information about the tutorial series, see [the first tutorial in the series](intro.md).</span></span>

<span data-ttu-id="4a575-107">In diesem Tutorial verwenden Sie zunächst die EF Core-Migrationsfeature für die Verwaltung von Datenmodelländerungen.</span><span class="sxs-lookup"><span data-stu-id="4a575-107">In this tutorial, you start using the EF Core migrations feature for managing data model changes.</span></span> <span data-ttu-id="4a575-108">In späteren Tutorials fügen Sie weitere Migrationen hinzu, wenn Sie das Datenmodell ändern.</span><span class="sxs-lookup"><span data-stu-id="4a575-108">In later tutorials, you'll add more migrations as you change the data model.</span></span>

## <a name="introduction-to-migrations"></a><span data-ttu-id="4a575-109">Einführung in die Migrationsfunktion</span><span class="sxs-lookup"><span data-stu-id="4a575-109">Introduction to migrations</span></span>

<span data-ttu-id="4a575-110">Wenn Sie eine neue Anwendung entwickeln, ändert sich Ihr Datenmodell häufig. Jedes Mal, wenn das Datenmodell geändert wird, ist es nicht mehr synchron mit der Datenbank.</span><span class="sxs-lookup"><span data-stu-id="4a575-110">When you develop a new application, your data model changes frequently, and each time the model changes, it gets out of sync with the database.</span></span> <span data-ttu-id="4a575-111">Sie haben zu Beginn dieser Tutorials Entity Framework für die Erstellung der Datenbank konfiguriert, sofern diese nicht vorhanden war.</span><span class="sxs-lookup"><span data-stu-id="4a575-111">You started these tutorials by configuring the Entity Framework to create the database if it doesn't exist.</span></span> <span data-ttu-id="4a575-112">Anschließend können Sie bei jeder Datenmodelländerung (beim Hinzufügen, Entfernen oder Ändern von Entitätsklassen oder beim Ändern Ihrer DbContext-Klasse) die Datenbank löschen. EF erstellt dann eine neue Datenbank, die dem Modell entspricht, und startet diese mit Testdaten.</span><span class="sxs-lookup"><span data-stu-id="4a575-112">Then each time you change the data model -- add, remove, or change entity classes or change your DbContext class -- you can delete the database and EF creates a new one that matches the model, and seeds it with test data.</span></span>

<span data-ttu-id="4a575-113">Diese Methode, bei der die Datenbank mit dem Datenmodell synchron gehalten wird, funktioniert so lange, bis Sie die Anwendung für die Produktion bereitstellen.</span><span class="sxs-lookup"><span data-stu-id="4a575-113">This method of keeping the database in sync with the data model works well until you deploy the application to production.</span></span> <span data-ttu-id="4a575-114">Wenn sich die Anwendung in der Produktion befindet, speichert sie in der Regel die Daten, die Sie weiter verwenden möchten, da nicht bei jeder Änderung, wie z.B. dem Hinzufügen einer neuen Spalte, alle Daten verloren gehen sollen.</span><span class="sxs-lookup"><span data-stu-id="4a575-114">When the application is running in production it's usually storing data that you want to keep, and you don't want to lose everything each time you make a change such as adding a new column.</span></span> <span data-ttu-id="4a575-115">Mit der EF Core-Migrationsfeature wird dieses Problem gelöst, indem EF das Datenbankschema aktualisiert, statt eine neue Datenbank zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="4a575-115">The EF Core Migrations feature solves this problem by enabling EF to update the database schema instead of creating  a new database.</span></span>

## <a name="entity-framework-core-nuget-packages-for-migrations"></a><span data-ttu-id="4a575-116">NuGet-Pakete für Migrationen in Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="4a575-116">Entity Framework Core NuGet packages for migrations</span></span>

<span data-ttu-id="4a575-117">Für die Arbeit mit Migrationen können Sie die **Paket-Manager-Konsole** (Package Manager Console, PMC) oder die Befehlszeilenschnittstelle (Command-Line Interface, CLI) verwenden.</span><span class="sxs-lookup"><span data-stu-id="4a575-117">To work with migrations, you can use the **Package Manager Console** (PMC) or the command-line interface (CLI).</span></span>  <span data-ttu-id="4a575-118">In diesen Tutorials wird die Verwendungsweise von CLI-Befehlen erläutert.</span><span class="sxs-lookup"><span data-stu-id="4a575-118">These tutorials show how to use CLI commands.</span></span> <span data-ttu-id="4a575-119">Informationen zur PMC finden Sie am [Ende dieses Tutorials](#pmc).</span><span class="sxs-lookup"><span data-stu-id="4a575-119">Information about the PMC is at [the end of this tutorial](#pmc).</span></span>

<span data-ttu-id="4a575-120">Die EF-Tools für die Befehlszeilenschnittstelle (CLI) werden unter [Microsoft.EntityFrameworkCore.Tools.DotNet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools.DotNet) bereitgestellt.</span><span class="sxs-lookup"><span data-stu-id="4a575-120">The EF tools for the command-line interface (CLI) are provided in [Microsoft.EntityFrameworkCore.Tools.DotNet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools.DotNet).</span></span> <span data-ttu-id="4a575-121">Fügen Sie das Paket zum Installieren wie dargestellt zu der `DotNetCliToolReference`-Sammlung in der *CSPROJ*-Datei hinzu.</span><span class="sxs-lookup"><span data-stu-id="4a575-121">To install this package, add it to the `DotNetCliToolReference` collection in the *.csproj* file, as shown.</span></span> <span data-ttu-id="4a575-122">**Hinweis:** Sie müssen dieses Paket installieren, indem Sie die *CSPROJ*-Datei bearbeiten. Sie können den `install-package`-Befehl oder die Paket-Manager-GUI nicht verwenden.</span><span class="sxs-lookup"><span data-stu-id="4a575-122">**Note:** You have to install this package by editing the *.csproj* file; you can't use the `install-package` command or the package manager GUI.</span></span> <span data-ttu-id="4a575-123">Sie können die *CSPROJ*-Datei bearbeiten, indem Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf den Projektnamen klicken und dann auf **„ContosoUniversity.csproj“ bearbeiten** klicken.</span><span class="sxs-lookup"><span data-stu-id="4a575-123">You can edit the *.csproj* file by right-clicking the project name in **Solution Explorer** and selecting **Edit ContosoUniversity.csproj**.</span></span>

[!code-xml[](intro/samples/cu/ContosoUniversity.csproj?range=12-15&highlight=2)]
  
<span data-ttu-id="4a575-124">(Die Versionsnummern in diesem Beispiel waren zum Zeitpunkt der Verfassung des Tutorials aktuell.)</span><span class="sxs-lookup"><span data-stu-id="4a575-124">(The version numbers in this example were current when the tutorial was written.)</span></span> 

## <a name="change-the-connection-string"></a><span data-ttu-id="4a575-125">Ändern der Verbindungszeichenfolge</span><span class="sxs-lookup"><span data-stu-id="4a575-125">Change the connection string</span></span>

<span data-ttu-id="4a575-126">Ändern Sie in der Datei *appsettings.json* in der Verbindungszeichenfolge den Namen der Datenbank in „ContosoUniversity2“ oder in einen anderen Namen, den Sie auf Ihrem Computer noch nicht verwendet haben.</span><span class="sxs-lookup"><span data-stu-id="4a575-126">In the *appsettings.json* file, change the name of the database in the connection string to ContosoUniversity2 or some other name that you haven't used on the computer you're using.</span></span>

[!code-json[Main](intro/samples/cu/appsettings2.json?range=1-4)]

<span data-ttu-id="4a575-127">Durch diese Änderung wird das Projekt eingerichtet, sodass bei der ersten Migration eine neue Datenbank erstellt wird.</span><span class="sxs-lookup"><span data-stu-id="4a575-127">This change sets up the project so that the first migration will create a new database.</span></span> <span data-ttu-id="4a575-128">Dies ist für die ersten Schritte mit Migrationen nicht erforderlich, aber Sie werden später feststellen, weshalb es sich empfiehlt, entsprechend vorzugehen.</span><span class="sxs-lookup"><span data-stu-id="4a575-128">This isn't required for getting started with migrations, but you'll see later why it's a good idea.</span></span>

> [!NOTE]
> <span data-ttu-id="4a575-129">Alternativ zum Ändern des Datenbanknamens können Sie die Datenbank auch löschen.</span><span class="sxs-lookup"><span data-stu-id="4a575-129">As an alternative to changing the database name, you can delete the database.</span></span> <span data-ttu-id="4a575-130">Verwenden Sie den **SQL Server-Objekt-Explorer** (SSOX) oder den CLI-Befehl `database drop`:</span><span class="sxs-lookup"><span data-stu-id="4a575-130">Use **SQL Server Object Explorer** (SSOX) or the `database drop` CLI command:</span></span>
> ```console
> dotnet ef database drop
> ```
> <span data-ttu-id="4a575-131">Im folgenden Abschnitt wird erläutert, wie CLI-Befehle ausgeführt werden.</span><span class="sxs-lookup"><span data-stu-id="4a575-131">The following section explains how to run CLI commands.</span></span>

## <a name="create-an-initial-migration"></a><span data-ttu-id="4a575-132">Erstellen einer ursprünglichen Migration</span><span class="sxs-lookup"><span data-stu-id="4a575-132">Create an initial migration</span></span>

<span data-ttu-id="4a575-133">Speichern Sie Ihre Änderungen, und erstellen Sie das Projekt.</span><span class="sxs-lookup"><span data-stu-id="4a575-133">Save your changes and build the project.</span></span> <span data-ttu-id="4a575-134">Öffnen Sie anschließend ein Befehlsfenster, und navigieren Sie zu dem Projektordner.</span><span class="sxs-lookup"><span data-stu-id="4a575-134">Then open a command window and navigate to the project folder.</span></span> <span data-ttu-id="4a575-135">Sie können diesen Vorgang auf folgende Weise beschleunigen:</span><span class="sxs-lookup"><span data-stu-id="4a575-135">Here's a quick way to do that:</span></span>

* <span data-ttu-id="4a575-136">Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf das Projekt, und wählen Sie aus dem Kontextmenü die Option **In Datei-Explorer öffnen** aus.</span><span class="sxs-lookup"><span data-stu-id="4a575-136">In **Solution Explorer**, right-click the project and choose **Open in File Explorer** from the context menu.</span></span>

  ![Menüelement „In Datei-Explorer öffnen“](migrations/_static/open-in-file-explorer.png)

* <span data-ttu-id="4a575-138">Geben Sie in die Adressleiste „cmd“ ein, und drücken Sie die EINGABETASTE.</span><span class="sxs-lookup"><span data-stu-id="4a575-138">Enter "cmd" in the address bar and press Enter.</span></span>

  ![Öffnen des Befehlsfensters](migrations/_static/open-command-window.png)

<span data-ttu-id="4a575-140">Geben Sie im Befehlsfenster folgenden Befehl ein:</span><span class="sxs-lookup"><span data-stu-id="4a575-140">Enter the following command in the command window:</span></span>

```console
dotnet ef migrations add InitialCreate
```

<span data-ttu-id="4a575-141">Im Befehlsfenster wird Ihnen eine Ausgabe wie die folgende angezeigt:</span><span class="sxs-lookup"><span data-stu-id="4a575-141">You see output like the following in the command window:</span></span>

```console
info: Microsoft.AspNetCore.DataProtection.KeyManagement.XmlKeyManager[0]
      User profile is available. Using 'C:\Users\username\AppData\Local\ASP.NET\DataProtection-Keys' as key repository and Windows DPAPI to encrypt keys at rest.
info: Microsoft.EntityFrameworkCore.Infrastructure[100403]
      Entity Framework Core 2.0.0-rtm-26452 initialized 'SchoolContext' using provider 'Microsoft.EntityFrameworkCore.SqlServer' with options: None
Done. To undo this action, use 'ef migrations remove'
```

> [!NOTE]
> <span data-ttu-id="4a575-142">Wenn Ihnen die Fehlermeldung *Es wurde keine ausführbare Datei gefunden, die dem Befehl „dotnet-ef“ entspricht.* angezeigt wird, finden Sie unter [diesem Blogbeitrag](http://thedatafarm.com/data-access/no-executable-found-matching-command-dotnet-ef/) Hilfe zur Problembehandlung.</span><span class="sxs-lookup"><span data-stu-id="4a575-142">If you see an error message *No executable found matching command "dotnet-ef"*, see [this blog post](http://thedatafarm.com/data-access/no-executable-found-matching-command-dotnet-ef/) for help troubleshooting.</span></span>

<span data-ttu-id="4a575-143">Wenn Ihnen die Fehlermeldung *Auf die Datei „ContosoUniversity.dll“ kann nicht zugegriffen werden, da sie von einem anderen Prozess verwendet wird.* angezeigt wird, suchen Sie in der Windows-Taskleiste nach dem Symbol „IIS Express“, klicken Sie mit der rechten Maustaste darauf, und klicken Sie anschließend auf **ContosoUniversity > Website beenden**.</span><span class="sxs-lookup"><span data-stu-id="4a575-143">If you see an error message "*cannot access the file ... ContosoUniversity.dll because it is being used by another process.*", find the IIS Express icon in the Windows System Tray, and right-click it, then click **ContosoUniversity > Stop Site**.</span></span>

## <a name="examine-the-up-and-down-methods"></a><span data-ttu-id="4a575-144">Überprüfen der Methoden „Up“ und „Down“</span><span class="sxs-lookup"><span data-stu-id="4a575-144">Examine the Up and Down methods</span></span>

<span data-ttu-id="4a575-145">Wenn Sie den Befehl `migrations add` ausgeführt haben, hat EF den Code generiert, mit dem die Datenbank von Grund auf neu erstellt wird.</span><span class="sxs-lookup"><span data-stu-id="4a575-145">When you executed the `migrations add` command, EF generated the code that will create the database from scratch.</span></span> <span data-ttu-id="4a575-146">Dieser Code befindet sich im Ordner *Migrationen* in der Datei *\<timestamp>_InitialCreate.cs*.</span><span class="sxs-lookup"><span data-stu-id="4a575-146">This code is in the *Migrations* folder, in the file named *\<timestamp>_InitialCreate.cs*.</span></span> <span data-ttu-id="4a575-147">Mit der Methode `Up` der `InitialCreate`-Klasse werden die Datenbanktabellen erstellt, die den Datenmodellentitäten entsprechen, und mit der Methode `Down` werden die Datenbanktabellen gelöscht (siehe folgendes Beispiel).</span><span class="sxs-lookup"><span data-stu-id="4a575-147">The `Up` method of the `InitialCreate` class creates the database tables that correspond to the data model entity sets, and the `Down` method deletes them, as shown in the following example.</span></span>

[!code-csharp[Main](intro/samples/cu/Migrations/20170215220724_InitialCreate.cs?range=92-118)]

<span data-ttu-id="4a575-148">Die Migrationsfunktion ruft die Methode `Up` auf, um die Datenmodelländerungen für eine Migration zu implementieren.</span><span class="sxs-lookup"><span data-stu-id="4a575-148">Migrations calls the `Up` method to implement the data model changes for a migration.</span></span> <span data-ttu-id="4a575-149">Wenn Sie einen Befehl eingeben, um ein Rollback für das Update auszuführen, ruft die Migrationsfunktion die Methode `Down` auf.</span><span class="sxs-lookup"><span data-stu-id="4a575-149">When you enter a command to roll back the update, Migrations calls the `Down` method.</span></span>

<span data-ttu-id="4a575-150">Dieser Code ist für die ursprüngliche Migration vorgesehen, die bei der Eingabe des Befehls `migrations add InitialCreate` erstellt wurde.</span><span class="sxs-lookup"><span data-stu-id="4a575-150">This code is for the initial migration that was created when you entered the `migrations add InitialCreate` command.</span></span> <span data-ttu-id="4a575-151">Der Parameter für den Migrationsnamen (in dem Beispiel „InitialCreate“) wird für den Dateinamen verwendet und kann einen beliebigen Namen haben.</span><span class="sxs-lookup"><span data-stu-id="4a575-151">The migration name parameter ("InitialCreate" in the example) is used for the file name and can be whatever you want.</span></span> <span data-ttu-id="4a575-152">Es wird empfohlen, ein Wort oder einen Ausdruck auszuwählen, durch das bzw. den der Hintergrund der Migration widergespiegelt wird.</span><span class="sxs-lookup"><span data-stu-id="4a575-152">It's best to choose a word or phrase that summarizes what is being done in the migration.</span></span> <span data-ttu-id="4a575-153">Sie können einer späteren Migration beispielsweise den Namen „AddDepartmentTable“ geben.</span><span class="sxs-lookup"><span data-stu-id="4a575-153">For example, you might name a later migration "AddDepartmentTable".</span></span>

<span data-ttu-id="4a575-154">Wenn Sie die ursprüngliche Migration erstellt haben, als die Datenbank bereits vorhanden war, wird der Code für die Datenbankerstellung zwar generiert, allerdings muss er nicht ausgeführt werden, da die Datenbank bereits dem Datenmodell entspricht.</span><span class="sxs-lookup"><span data-stu-id="4a575-154">If you created the initial migration when the database already exists, the database creation code is generated but it doesn't have to run because the database already matches the data model.</span></span> <span data-ttu-id="4a575-155">Wenn Sie die App in einer anderen Umgebung bereitstellen, in der die Datenbank noch nicht vorhanden ist, wird dieser Code ausgeführt, um Ihre Datenbank zu erstellen. Daher sollte dieser zunächst getestet werden.</span><span class="sxs-lookup"><span data-stu-id="4a575-155">When you deploy the app to another environment where the database doesn't exist yet, this code will run to create your database, so it's a good idea to test it first.</span></span> <span data-ttu-id="4a575-156">Aus diesem Grund haben Sie den Namen der Datenbank zuvor in der Verbindungszeichenfolge geändert – damit eine Datenbank bei Migrationen von Grund auf neu erstellt werden kann.</span><span class="sxs-lookup"><span data-stu-id="4a575-156">That's why you changed the name of the database in the connection string earlier -- so that migrations can create a new one from scratch.</span></span>

## <a name="examine-the-data-model-snapshot"></a><span data-ttu-id="4a575-157">Momentaufnahme: Überprüfen des Datenmodells</span><span class="sxs-lookup"><span data-stu-id="4a575-157">Examine the data model snapshot</span></span>

<span data-ttu-id="4a575-158">Die Migrationsfunktion erstellt unter *Migrations/SchoolContextModelSnapshot.cs* auch eine *Momentaufnahme* des aktuellen Datenbankschemas.</span><span class="sxs-lookup"><span data-stu-id="4a575-158">Migrations also creates a *snapshot* of the current database schema in *Migrations/SchoolContextModelSnapshot.cs*.</span></span> <span data-ttu-id="4a575-159">Dieser Code sieht folgendermaßen aus:</span><span class="sxs-lookup"><span data-stu-id="4a575-159">Here's what that code looks like:</span></span>

[!code-csharp[Main](intro/samples/cu/Migrations/SchoolContextModelSnapshot1.cs?name=snippet_Truncate)]

<span data-ttu-id="4a575-160">Da das aktuelle Datenbankschema in Code dargestellt wird, muss EF Core für die Erstellung von Migrationen nicht mit der Datenbank interagieren.</span><span class="sxs-lookup"><span data-stu-id="4a575-160">Because the current database schema is represented in code, EF Core doesn't have to interact with the database to create migrations.</span></span> <span data-ttu-id="4a575-161">Wenn Sie eine Migration hinzufügen, bestimmt EF die vorgenommenen Änderungen, indem das Datenmodell mit der Momentaufnahmedatei verglichen wird.</span><span class="sxs-lookup"><span data-stu-id="4a575-161">When you add a migration, EF determines what changed by comparing the data model to the snapshot file.</span></span> <span data-ttu-id="4a575-162">EF interagiert nur dann mit der Datenbank, wenn diese aktualisiert werden muss.</span><span class="sxs-lookup"><span data-stu-id="4a575-162">EF interacts with the database only when it has to update the database.</span></span> 

<span data-ttu-id="4a575-163">Die Momentaufnahmedatei muss mit den Migrationen synchron gehalten werden, die diese erstellt haben, damit Sie eine Migration einfach durch das Löschen der Datei *\<timestamp>_\<migrationname>.cs* entfernen können.</span><span class="sxs-lookup"><span data-stu-id="4a575-163">The snapshot file has to be kept in sync with the migrations that create it, so you can't remove a migration just by deleting the file named  *\<timestamp>_\<migrationname>.cs*.</span></span> <span data-ttu-id="4a575-164">Wenn Sie diese Datei löschen, sind die übrigen Migrationen mit der Datei, die die Datenbankmomentaufnahme enthält, nicht mehr synchron.</span><span class="sxs-lookup"><span data-stu-id="4a575-164">If you delete that file, the remaining migrations will be out of sync with the database snapshot file.</span></span> <span data-ttu-id="4a575-165">Verwenden Sie den Befehl [dotnet ef migrations remove](https://docs.microsoft.com/ef/core/miscellaneous/cli/dotnet#dotnet-ef-migrations-remove), um die letzte von Ihnen hinzugefügte Migration zu löschen.</span><span class="sxs-lookup"><span data-stu-id="4a575-165">To delete the last migration that you added, use the [dotnet ef migrations remove](https://docs.microsoft.com/ef/core/miscellaneous/cli/dotnet#dotnet-ef-migrations-remove) command.</span></span>

## <a name="apply-the-migration-to-the-database"></a><span data-ttu-id="4a575-166">Anwenden der Migration auf die Datenbank</span><span class="sxs-lookup"><span data-stu-id="4a575-166">Apply the migration to the database</span></span>

<span data-ttu-id="4a575-167">Geben Sie folgenden Befehl in das Befehlsfenster ein, um die Datenbank mit den darin enthaltenen Tabellen zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="4a575-167">In the command window, enter the following command to create the database and tables in it.</span></span>

```console
dotnet ef database update
```

<span data-ttu-id="4a575-168">Die Ausgabe des Befehls ähnelt der des Befehls `migrations add`. Der Unterschied besteht darin, dass Ihnen Protokolle für die SQL-Befehle angezeigt werden, mit denen die Datenbank eingerichtet wird.</span><span class="sxs-lookup"><span data-stu-id="4a575-168">The output from the command is similar to the `migrations add` command, except that you see logs for the SQL commands that set up the database.</span></span> <span data-ttu-id="4a575-169">In der folgenden Beispielausgabe werden die meisten Protokolle ausgelassen.</span><span class="sxs-lookup"><span data-stu-id="4a575-169">Most of the logs are omitted in the following sample output.</span></span> <span data-ttu-id="4a575-170">Wenn diese Detailebene in Protokollnachrichten nicht angezeigt werden soll, können Sie die Protokollebene in der Datei *appsettings.Development.json* ändern.</span><span class="sxs-lookup"><span data-stu-id="4a575-170">If you prefer not to see this level of detail in log messages, you can change the log level in the *appsettings.Development.json* file.</span></span> <span data-ttu-id="4a575-171">Weitere Informationen finden Sie unter [Introduction to Logging (Einführung in die Protokollierung)](xref:fundamentals/logging/index).</span><span class="sxs-lookup"><span data-stu-id="4a575-171">For more information, see [Introduction to logging](xref:fundamentals/logging/index).</span></span>

```text
info: Microsoft.AspNetCore.DataProtection.KeyManagement.XmlKeyManager[0]
      User profile is available. Using 'C:\Users\username\AppData\Local\ASP.NET\DataProtection-Keys' as key repository and Windows DPAPI to encrypt keys at rest.
info: Microsoft.EntityFrameworkCore.Infrastructure[100403]
      Entity Framework Core 2.0.0-rtm-26452 initialized 'SchoolContext' using provider 'Microsoft.EntityFrameworkCore.SqlServer' with options: None
info: Microsoft.EntityFrameworkCore.Database.Command[200101]
      Executed DbCommand (467ms) [Parameters=[], CommandType='Text', CommandTimeout='60']
      CREATE DATABASE [ContosoUniversity2];
info: Microsoft.EntityFrameworkCore.Database.Command[200101]
      Executed DbCommand (20ms) [Parameters=[], CommandType='Text', CommandTimeout='30']
      CREATE TABLE [__EFMigrationsHistory] (
          [MigrationId] nvarchar(150) NOT NULL,
          [ProductVersion] nvarchar(32) NOT NULL,
          CONSTRAINT [PK___EFMigrationsHistory] PRIMARY KEY ([MigrationId])
      );

<logs omitted for brevity>

info: Microsoft.EntityFrameworkCore.Database.Command[200101]
      Executed DbCommand (3ms) [Parameters=[], CommandType='Text', CommandTimeout='30']
      INSERT INTO [__EFMigrationsHistory] ([MigrationId], [ProductVersion])
      VALUES (N'20170816151242_InitialCreate', N'2.0.0-rtm-26452');
Done.
```

<span data-ttu-id="4a575-172">Verwenden Sie den **SQL Server-Objekt-Explorer**, um die Datenbank wie im ersten Tutorial zu untersuchen.</span><span class="sxs-lookup"><span data-stu-id="4a575-172">Use **SQL Server Object Explorer** to inspect the database as you did in the first tutorial.</span></span>  <span data-ttu-id="4a575-173">Sie werden feststellen, dass die Tabelle „__EFMigrationsHistory“ hinzugefügt wurde, in der nachverfolgt wird, welche Migrationen auf die Datenbank angewendet worden sind.</span><span class="sxs-lookup"><span data-stu-id="4a575-173">You'll notice the addition of an __EFMigrationsHistory table that keeps track of which migrations have been applied to the database.</span></span> <span data-ttu-id="4a575-174">Wenn Sie die Daten in dieser Tabelle anzeigen, ist eine Zeile für die erste Migration zu sehen.</span><span class="sxs-lookup"><span data-stu-id="4a575-174">View the data in that table and you'll see one row for the first migration.</span></span> <span data-ttu-id="4a575-175">(Im letzten Protokoll im vorgehenden Beispiel der CLI-Ausgabe wird die INSERT-Anweisung angezeigt, mit der diese Zeile erstellt wird.)</span><span class="sxs-lookup"><span data-stu-id="4a575-175">(The last log in the preceding CLI output example shows the INSERT statement that creates this row.)</span></span>

<span data-ttu-id="4a575-176">Führen Sie die Anwendung aus, um zu überprüfen, ob alles wie gewohnt funktioniert.</span><span class="sxs-lookup"><span data-stu-id="4a575-176">Run the application to verify that everything still works the same as before.</span></span>

![Indexseite „Studenten“](migrations/_static/students-index.png)

<a id="pmc"></a>
## <a name="command-line-interface-cli-vs-package-manager-console-pmc"></a><span data-ttu-id="4a575-178">Die Befehlszeilenschnittstelle (Command-line Interface, CLI) im Vergleich mit der Paket-Manager-Konsole (Package Manager Console, PMC)</span><span class="sxs-lookup"><span data-stu-id="4a575-178">Command-line interface (CLI) vs. Package Manager Console (PMC)</span></span>

<span data-ttu-id="4a575-179">Die EF-Tools für die Verwaltung von Migrationen sind über .NET Core-CLI-Befehle oder über PowerShell-Cmdlets im Visual Studio-Fenster **Paket-Manager-Console** (PMC) verfügbar.</span><span class="sxs-lookup"><span data-stu-id="4a575-179">The EF tooling for managing migrations is available from .NET Core CLI commands or from PowerShell cmdlets in the Visual Studio **Package Manager Console** (PMC) window.</span></span> <span data-ttu-id="4a575-180">In diesem Tutorial wird die Verwendungsweise der CLI erläutert. Sie können aber auch die PMC verwenden, wenn Sie diese bevorzugen.</span><span class="sxs-lookup"><span data-stu-id="4a575-180">This tutorial shows how to use the CLI, but you can use the PMC if you prefer.</span></span>

<span data-ttu-id="4a575-181">Die EF-Befehle für die PMC-Befehle sind im Paket [Microsoft.EntityFrameworkCore.Tools](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools) enthalten.</span><span class="sxs-lookup"><span data-stu-id="4a575-181">The EF commands for the PMC commands are in the [Microsoft.EntityFrameworkCore.Tools](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools) package.</span></span> <span data-ttu-id="4a575-182">Dieses Paket ist bereits im Metapaket [Microsoft.AspNetCore.All](xref:fundamentals/metapackage) enthalten. Eine Installation ist daher nicht erforderlich.</span><span class="sxs-lookup"><span data-stu-id="4a575-182">This package is already included in the [Microsoft.AspNetCore.All](xref:fundamentals/metapackage) metapackage, so you don't have to install it.</span></span>

<span data-ttu-id="4a575-183">**Wichtig:** Dieses Paket ist nicht mit dem Paket identisch, das Sie für die CLI durch Bearbeitung der *CSPROJ*-Datei installiert haben.</span><span class="sxs-lookup"><span data-stu-id="4a575-183">**Important:** This isn't the same package as the one you install for the CLI by editing the *.csproj* file.</span></span> <span data-ttu-id="4a575-184">Der Name dieses Pakets endet mit `Tools`, im Gegensatz zum Namen des CLI-Pakets, der mit `Tools.DotNet` endet.</span><span class="sxs-lookup"><span data-stu-id="4a575-184">The name of this one ends in `Tools`, unlike the CLI package name which ends in `Tools.DotNet`.</span></span>

<span data-ttu-id="4a575-185">Weitere Informationen zu CLI-Befehlen finden Sie unter [.NET Core-CLI](https://docs.microsoft.com/ef/core/miscellaneous/cli/dotnet).</span><span class="sxs-lookup"><span data-stu-id="4a575-185">For more information about the CLI commands, see [.NET Core CLI](https://docs.microsoft.com/ef/core/miscellaneous/cli/dotnet).</span></span> 

<span data-ttu-id="4a575-186">Weitere Informationen zu den PMC-Befehlen finden Sie unter [Paket-Manager-Konsole (Visual Studio)](https://docs.microsoft.com/ef/core/miscellaneous/cli/powershell).</span><span class="sxs-lookup"><span data-stu-id="4a575-186">For more information about the PMC commands, see [Package Manager Console (Visual Studio)](https://docs.microsoft.com/ef/core/miscellaneous/cli/powershell).</span></span>

## <a name="summary"></a><span data-ttu-id="4a575-187">Zusammenfassung</span><span class="sxs-lookup"><span data-stu-id="4a575-187">Summary</span></span>

<span data-ttu-id="4a575-188">In diesem Tutorial haben Sie gelernt, wie Sie Ihre erste Migration erstellen und anwenden.</span><span class="sxs-lookup"><span data-stu-id="4a575-188">In this tutorial, you've seen how to create and apply your first migration.</span></span> <span data-ttu-id="4a575-189">Im nächsten Tutorial befassen Sie sich mit erweiterten Themen, indem Sie das Datenmodell erweitern.</span><span class="sxs-lookup"><span data-stu-id="4a575-189">In the next tutorial, you'll begin looking at more advanced topics by expanding the data model.</span></span> <span data-ttu-id="4a575-190">Dabei werden Sie weitere Migrationen erstellen und anwenden.</span><span class="sxs-lookup"><span data-stu-id="4a575-190">Along the way you'll create and apply additional migrations.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="4a575-191">[Zurück](sort-filter-page.md)
[Weiter](complex-data-model.md)</span><span class="sxs-lookup"><span data-stu-id="4a575-191">[Previous](sort-filter-page.md)
[Next](complex-data-model.md)</span></span>  
