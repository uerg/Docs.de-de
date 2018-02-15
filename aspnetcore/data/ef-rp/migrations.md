---
title: 'Razor-Seiten mit EF Core: Migrationen (4 von 8)'
author: rick-anderson
description: "In diesem Tutorial verwenden Sie zunächst das EF Core-Migrationsfeature für die Verwaltung von Datenmodelländerungen in einer ASP.NET Core MVC-App."
manager: wpickett
ms.author: riande
ms.date: 10/15/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: data/ef-rp/migrations
ms.openlocfilehash: e89d95702cb94556bc6e5dc73253c51acaa11578
ms.sourcegitcommit: 18d1dc86770f2e272d93c7e1cddfc095c5995d9e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/31/2018
---
# <a name="migrations---ef-core-with-razor-pages-tutorial-4-of-8"></a><span data-ttu-id="453f9-103">Migrationen: Tutorial zu EF-Core mit Razor-Seiten (4 von 8)</span><span class="sxs-lookup"><span data-stu-id="453f9-103">Migrations - EF Core with Razor Pages tutorial (4 of 8)</span></span>

<span data-ttu-id="453f9-104">Von [Tom Dykstra](https://github.com/tdykstra), [Jon P. Smith](https://twitter.com/thereformedprog) und [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="453f9-104">By [Tom Dykstra](https://github.com/tdykstra), [Jon P Smith](https://twitter.com/thereformedprog), and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

[!INCLUDE[about the series](../../includes/RP-EF/intro.md)]

<span data-ttu-id="453f9-105">In diesem Tutorial wird das EF Core-Migrationsfeature zur Verwaltung von Datenmodelländerungen verwendet.</span><span class="sxs-lookup"><span data-stu-id="453f9-105">In this tutorial, the EF Core migrations feature for managing data model changes is used.</span></span>

<span data-ttu-id="453f9-106">Wenn nicht zu lösende Probleme auftreten, laden Sie die [abgeschlossene Anwendung für diese Phase](
https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part4-migrations) herunter.</span><span class="sxs-lookup"><span data-stu-id="453f9-106">If you run into problems you can't solve, download the [completed app for this stage](
https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part4-migrations).</span></span>

<span data-ttu-id="453f9-107">Wenn eine neue App entwickelt wird, ändert sich das Datenmodell häufig.</span><span class="sxs-lookup"><span data-stu-id="453f9-107">When a new app is developed, the data model changes frequently.</span></span> <span data-ttu-id="453f9-108">Nach jeder Modelländerung ist das Modell nicht mehr synchron mit der Datenbank.</span><span class="sxs-lookup"><span data-stu-id="453f9-108">Each time the model changes, the model gets out of sync with the database.</span></span> <span data-ttu-id="453f9-109">In diesem Tutorial wurde zunächst Entity Framework für die Erstellung der Datenbank konfiguriert, falls diese nicht vorhanden ist.</span><span class="sxs-lookup"><span data-stu-id="453f9-109">This tutorial started by configuring the Entity Framework to create the database if it doesn't exist.</span></span> <span data-ttu-id="453f9-110">Bei jeder Datenmodelländerung:</span><span class="sxs-lookup"><span data-stu-id="453f9-110">Each time the data model changes:</span></span>

* <span data-ttu-id="453f9-111">Wird die Datenbank gelöscht.</span><span class="sxs-lookup"><span data-stu-id="453f9-111">The DB is dropped.</span></span>
* <span data-ttu-id="453f9-112">Erstellt EF eine neue, dem Modell entsprechende Datenbank.</span><span class="sxs-lookup"><span data-stu-id="453f9-112">EF creates a new one that matches the model.</span></span>
* <span data-ttu-id="453f9-113">Füllt die App die Datenbank mit Testdaten auf.</span><span class="sxs-lookup"><span data-stu-id="453f9-113">The app seeds the DB with test data.</span></span>

<span data-ttu-id="453f9-114">Dieser Ansatz, bei dem die Datenbank mit dem Datenmodell synchron gehalten wird, funktioniert so lange, bis Sie die App für die Produktion bereitstellen.</span><span class="sxs-lookup"><span data-stu-id="453f9-114">This approach to keeping the DB in sync with the data model works well until you deploy the app to production.</span></span> <span data-ttu-id="453f9-115">Wenn die App in der Produktionsumgebung ausgeführt wird, speichert sie in der Regel Daten, die verwaltet werden müssen.</span><span class="sxs-lookup"><span data-stu-id="453f9-115">When the app is running in production, it's usually storing data that needs to be maintained.</span></span> <span data-ttu-id="453f9-116">Jedes Mal, wenn eine Änderung vorgenommen wird (z.B. wenn eine neue Spalte hinzugefügt wird), kann die App nicht mit einer Testdatenbank starten.</span><span class="sxs-lookup"><span data-stu-id="453f9-116">The app can't start with a test DB each time a change is made (such as adding a new column).</span></span> <span data-ttu-id="453f9-117">Mit der EF Core-Migrationsfeature wird dieses Problem gelöst, indem EF Core das Datenbankschema aktualisiert, anstatt eine neue Datenbank zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="453f9-117">The EF Core Migrations feature solves this problem by enabling EF Core to update the DB schema instead of creating a new DB.</span></span>

<span data-ttu-id="453f9-118">Statt die Datenbank bei den Datenmodelländerungen zu löschen und neu zu erstellen, wird bei der Migration eine Aktualisierung des Schemas und eine Beibehaltung vorhandener Daten angestrebt.</span><span class="sxs-lookup"><span data-stu-id="453f9-118">Rather than dropping and recreating the DB when the data model changes, migrations updates the schema and retains existing data.</span></span>

## <a name="entity-framework-core-nuget-packages-for-migrations"></a><span data-ttu-id="453f9-119">NuGet-Pakete für Migrationen in Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="453f9-119">Entity Framework Core NuGet packages for migrations</span></span>

<span data-ttu-id="453f9-120">Verwenden Sie für die Arbeit mit Migrationen die **Paket-Manager-Konsole** (Package Manager Console, PMC) oder die Befehlszeilenschnittstelle (Command-Line Interface, CLI).</span><span class="sxs-lookup"><span data-stu-id="453f9-120">To work with migrations, use the **Package Manager Console** (PMC) or the command-line interface (CLI).</span></span> <span data-ttu-id="453f9-121">In diesen Tutorials wird die Verwendungsweise von CLI-Befehlen erläutert.</span><span class="sxs-lookup"><span data-stu-id="453f9-121">These tutorials show how to use CLI commands.</span></span> <span data-ttu-id="453f9-122">Informationen zur PMC finden Sie am [Ende dieses Tutorials](#pmc).</span><span class="sxs-lookup"><span data-stu-id="453f9-122">Information about the PMC is at [the end of this tutorial](#pmc).</span></span>

<span data-ttu-id="453f9-123">Die EF Core-Tools für die Befehlszeilenschnittstelle (Command-Line Interface, CLI) werden unter [Microsoft.EntityFrameworkCore.Tools.DotNet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools.DotNet) bereitgestellt.</span><span class="sxs-lookup"><span data-stu-id="453f9-123">The EF Core tools for the command-line interface (CLI) are provided in [Microsoft.EntityFrameworkCore.Tools.DotNet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools.DotNet).</span></span> <span data-ttu-id="453f9-124">Fügen Sie das Paket zum Installieren wie dargestellt zu der `DotNetCliToolReference`-Auflistung in der *.csproj*-Datei hinzu.</span><span class="sxs-lookup"><span data-stu-id="453f9-124">To install this package, add it to the `DotNetCliToolReference` collection in the *.csproj* file, as shown.</span></span> <span data-ttu-id="453f9-125">**Hinweis:** Dieses Paket muss durch Bearbeitung der *.csproj*-Datei installiert werden.</span><span class="sxs-lookup"><span data-stu-id="453f9-125">**Note:** This package must be installed by editing the *.csproj* file.</span></span> <span data-ttu-id="453f9-126">Dieses Paket kann nicht mit dem Befehl `install-package` oder über die grafische Benutzeroberfläche des Paket-Managers installiert werden.</span><span class="sxs-lookup"><span data-stu-id="453f9-126">The`install-package` command or the package manager GUI cannot be used to install this package.</span></span> <span data-ttu-id="453f9-127">Bearbeiten Sie die *.csproj*-Datei, indem Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf den Projektnamen klicken und **Edit ContosoUniversity.csproj** („ContosoUniversity.csproj“ bearbeiten) auswählen.</span><span class="sxs-lookup"><span data-stu-id="453f9-127">Edit the *.csproj* file by right-clicking the project name in **Solution Explorer** and selecting **Edit ContosoUniversity.csproj**.</span></span>

<span data-ttu-id="453f9-128">Das folgende Markup zeigt die aktualisierte *.csproj*-Datei mit hervorgehobenen EF Core-CLI-Tools an:</span><span class="sxs-lookup"><span data-stu-id="453f9-128">The following markup shows the updated *.csproj* file with the EF Core CLI tools highlighted:</span></span>

[!code-xml[](intro/samples/cu/ContosoUniversity.csproj?highlight=12)]
  
<span data-ttu-id="453f9-129">Die Versionsnummern im vorgehenden Beispiel waren zum Zeitpunkt der Verfassung des Tutorials aktuell.</span><span class="sxs-lookup"><span data-stu-id="453f9-129">The version numbers in the preceding example were current when the tutorial was written.</span></span> <span data-ttu-id="453f9-130">Verwenden Sie die gleiche Version für die EF Core-CLI-Tools, die auch in den anderen Paketen gefunden wurde.</span><span class="sxs-lookup"><span data-stu-id="453f9-130">Use the same version for the EF Core CLI tools found in the other packages.</span></span>

## <a name="change-the-connection-string"></a><span data-ttu-id="453f9-131">Ändern der Verbindungszeichenfolge</span><span class="sxs-lookup"><span data-stu-id="453f9-131">Change the connection string</span></span>

<span data-ttu-id="453f9-132">Ändern Sie in der Datei *appsettings.json* den Namen der Datenbank in der Verbindungszeichenfolge in „ContosoUniversity2“.</span><span class="sxs-lookup"><span data-stu-id="453f9-132">In the *appsettings.json* file, change the name of the DB in the connection string to ContosoUniversity2.</span></span>

[!code-json[Main](intro/samples/cu/appsettings2.json?range=1-4)]

<span data-ttu-id="453f9-133">Durch das Ändern des Datenbanknamens in der Verbindungszeichenfolge wird bei der ersten Migration eine neue Datenbank erstellt.</span><span class="sxs-lookup"><span data-stu-id="453f9-133">Changing the DB name in the connection string causes the first migration to create a new DB.</span></span> <span data-ttu-id="453f9-134">Es wird eine neue Datenbank erstellt, da keine Datenbank mit diesem Namen vorhanden ist.</span><span class="sxs-lookup"><span data-stu-id="453f9-134">A new DB is created because one with that name doesn't exist.</span></span> <span data-ttu-id="453f9-135">Die Verbindungszeichenfolge muss für die ersten Schritte mit Migrationen nicht geändert werden.</span><span class="sxs-lookup"><span data-stu-id="453f9-135">Changing the connection string isn't required for getting started with migrations.</span></span>

<span data-ttu-id="453f9-136">Alternativ zum Ändern des Datenbanknamens kann die Datenbank auch gelöscht werden.</span><span class="sxs-lookup"><span data-stu-id="453f9-136">An alternative to changing the DB name is deleting the DB.</span></span> <span data-ttu-id="453f9-137">Verwenden Sie den **SQL Server-Objekt-Explorer** (SSOX) oder den CLI-Befehl `database drop`:</span><span class="sxs-lookup"><span data-stu-id="453f9-137">Use **SQL Server Object Explorer** (SSOX) or the `database drop` CLI command:</span></span>

 ```console
 dotnet ef database drop
 ```

<span data-ttu-id="453f9-138">Im folgenden Abschnitt wird erläutert, wie CLI-Befehle ausgeführt werden.</span><span class="sxs-lookup"><span data-stu-id="453f9-138">The following section explains how to run CLI commands.</span></span>

## <a name="create-an-initial-migration"></a><span data-ttu-id="453f9-139">Erstellen einer ursprünglichen Migration</span><span class="sxs-lookup"><span data-stu-id="453f9-139">Create an initial migration</span></span>

<span data-ttu-id="453f9-140">Erstellen Sie das Projekt.</span><span class="sxs-lookup"><span data-stu-id="453f9-140">Build the project.</span></span>

<span data-ttu-id="453f9-141">Öffnen Sie ein Befehlsfenster, und navigieren Sie zu dem Projektordner.</span><span class="sxs-lookup"><span data-stu-id="453f9-141">Open a command window and navigate to the project folder.</span></span> <span data-ttu-id="453f9-142">Der Projektordner enthält die Datei *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="453f9-142">The project folder contains the *Startup.cs* file.</span></span>

<span data-ttu-id="453f9-143">Geben Sie im Befehlsfenster Folgendes ein:</span><span class="sxs-lookup"><span data-stu-id="453f9-143">Enter the following in the command window:</span></span>

```console
dotnet ef migrations add InitialCreate
```

<span data-ttu-id="453f9-144">Im Befehlsfenster werden Informationen angezeigt, die in etwa wie folgt aussehen:</span><span class="sxs-lookup"><span data-stu-id="453f9-144">The command window displays information similar to the following:</span></span>

```console
info: Microsoft.AspNetCore.DataProtection.KeyManagement.XmlKeyManager[0]
      User profile is available. Using 'C:\Users\username\AppData\Local\ASP.NET\DataProtection-Keys' as key repository and Windows DPAPI to encrypt keys at rest.
info: Microsoft.EntityFrameworkCore.Infrastructure[100403]
      Entity Framework Core 2.0.0-rtm-26452 initialized 'SchoolContext' using provider 'Microsoft.EntityFrameworkCore.SqlServer' with options: None
Done. To undo this action, use 'ef migrations remove'
```

<span data-ttu-id="453f9-145">Wenn die Migration fehlschlägt und die Nachricht „*Auf die Datei „ContosoUniversity.dll“ kann nicht zugegriffen werden, da sie von einem anderen Prozess verwendet wird.*“</span><span class="sxs-lookup"><span data-stu-id="453f9-145">If the migration fails with the message "*cannot access the file ... ContosoUniversity.dll because it is being used by another process.*"</span></span> <span data-ttu-id="453f9-146">angezeigt wird:</span><span class="sxs-lookup"><span data-stu-id="453f9-146">is displayed:</span></span>

* <span data-ttu-id="453f9-147">Beenden Sie IIS Express.</span><span class="sxs-lookup"><span data-stu-id="453f9-147">Stop IIS Express.</span></span>

   * <span data-ttu-id="453f9-148">Beenden Sie Visual Studio, und führen Sie einen Neustart durch, oder</span><span class="sxs-lookup"><span data-stu-id="453f9-148">Exit and restart Visual Studio, or</span></span>
   * <span data-ttu-id="453f9-149">Suchen Sie in der Windows-Taskleiste nach dem Symbol „IIS Express“.</span><span class="sxs-lookup"><span data-stu-id="453f9-149">Find the IIS Express icon in the Windows System Tray.</span></span>
   * <span data-ttu-id="453f9-150">Klicken Sie mit der rechten Maustaste auf das Symbol „IIS Express“, und klicken Sie anschließend auf **ContosoUniversity > Website beenden**.</span><span class="sxs-lookup"><span data-stu-id="453f9-150">Right-click the IIS Express icon, and then click **ContosoUniversity > Stop Site**.</span></span>

<span data-ttu-id="453f9-151">Wenn die Fehlermeldung „Fehler beim Buildvorgang.“</span><span class="sxs-lookup"><span data-stu-id="453f9-151">If the error message "Build failed."</span></span> <span data-ttu-id="453f9-152">angezeigt wird, führen Sie den Befehl erneut aus.</span><span class="sxs-lookup"><span data-stu-id="453f9-152">is displayed, run the command again.</span></span> <span data-ttu-id="453f9-153">Hinterlassen Sie am Ende dieses Tutorials eine Notiz, wenn Ihnen diese Fehlermeldung angezeigt wird.</span><span class="sxs-lookup"><span data-stu-id="453f9-153">If you get this error, leave a note at the bottom of this tutorial.</span></span>

### <a name="examine-the-up-and-down-methods"></a><span data-ttu-id="453f9-154">Überprüfen der Methoden „Up“ und „Down“</span><span class="sxs-lookup"><span data-stu-id="453f9-154">Examine the Up and Down methods</span></span>

<span data-ttu-id="453f9-155">Der EF Core-Befehl `migrations add` hat Code generiert, aus dem die Datenbank erstellt werden soll.</span><span class="sxs-lookup"><span data-stu-id="453f9-155">The EF Core command `migrations add` generated code to create the DB from.</span></span> <span data-ttu-id="453f9-156">Dieser Migrationscode ist in der Datei *Migrations\<timestamp>_InitialCreate.cs* enthalten.</span><span class="sxs-lookup"><span data-stu-id="453f9-156">This migrations code is in the *Migrations\<timestamp>_InitialCreate.cs* file.</span></span> <span data-ttu-id="453f9-157">Die Methode `Up` der Klasse `InitialCreate` erstellt die Datenbanktabellen, die den Datenmodellentitätenmengen entsprechen.</span><span class="sxs-lookup"><span data-stu-id="453f9-157">The `Up` method of the `InitialCreate` class creates the DB tables that correspond to the data model entity sets.</span></span> <span data-ttu-id="453f9-158">Die Methode `Down` löscht diese, wie im folgenden Beispiel dargestellt:</span><span class="sxs-lookup"><span data-stu-id="453f9-158">The `Down` method deletes them, as shown in the following example:</span></span>

[!code-csharp[Main](intro/samples/cu/Migrations/20171026010210_InitialCreate.cs?range=8-24,77-)]

<span data-ttu-id="453f9-159">Die Migrationsfunktion ruft die Methode `Up` auf, um die Datenmodelländerungen für eine Migration zu implementieren.</span><span class="sxs-lookup"><span data-stu-id="453f9-159">Migrations calls the `Up` method to implement the data model changes for a migration.</span></span> <span data-ttu-id="453f9-160">Wenn Sie einen Befehl eingeben, um ein Rollback für das Update auszuführen, ruft die Migrationsfunktion die Methode `Down` auf.</span><span class="sxs-lookup"><span data-stu-id="453f9-160">When you enter a command to roll back the update, migrations calls the `Down` method.</span></span>

<span data-ttu-id="453f9-161">Der vorangehende Code ist für die ursprüngliche Migration bestimmt.</span><span class="sxs-lookup"><span data-stu-id="453f9-161">The preceding code is for the initial migration.</span></span> <span data-ttu-id="453f9-162">Dieser Code wurde bei der Ausführung des Befehls `migrations add InitialCreate` erstellt.</span><span class="sxs-lookup"><span data-stu-id="453f9-162">That code was created when the `migrations add InitialCreate` command was run.</span></span> <span data-ttu-id="453f9-163">Der Parameter für den Migrationsnamen (in dem Beispiel „InitialCreate“) wird für den Dateinamen verwendet.</span><span class="sxs-lookup"><span data-stu-id="453f9-163">The migration name parameter ("InitialCreate" in the example) is used for the file name.</span></span> <span data-ttu-id="453f9-164">Der Migrationsname kann ein beliebiger gültiger Dateiname sein.</span><span class="sxs-lookup"><span data-stu-id="453f9-164">The migration name can be any valid file name.</span></span> <span data-ttu-id="453f9-165">Es wird empfohlen, ein Wort oder einen Ausdruck auszuwählen, durch das bzw. den der Hintergrund der Migration widergespiegelt wird.</span><span class="sxs-lookup"><span data-stu-id="453f9-165">It's best to choose a word or phrase that summarizes what is being done in the migration.</span></span> <span data-ttu-id="453f9-166">Eine Migration, bei der eine Tabelle mit Fachbereichen hinzugefügt wurde, könnte beispielsweise als „AddDepartmentTable“ bezeichnet werden.</span><span class="sxs-lookup"><span data-stu-id="453f9-166">For example, a migration that added a department table might be called "AddDepartmentTable."</span></span>

<span data-ttu-id="453f9-167">Wenn die ursprüngliche Migration erstellt wird und die Datenbank beendet wird:</span><span class="sxs-lookup"><span data-stu-id="453f9-167">If the initial migration is created and the DB exits:</span></span>

* <span data-ttu-id="453f9-168">Wird der Code für die Datenbankerstellung generiert.</span><span class="sxs-lookup"><span data-stu-id="453f9-168">The DB creation code is generated.</span></span>
* <span data-ttu-id="453f9-169">Muss der Code für die Datenbankerstellung nicht ausgeführt werden, da die Datenbank bereits mit dem Datenmodell übereinstimmt.</span><span class="sxs-lookup"><span data-stu-id="453f9-169">The DB creation code doesn't need to run because the DB already matches the data model.</span></span> <span data-ttu-id="453f9-170">Wenn der Code für die Datenbankerstellung ausgeführt wird, werden keine Änderungen vorgenommen, da die Datenbank bereits mit dem Datenmodell übereinstimmt.</span><span class="sxs-lookup"><span data-stu-id="453f9-170">If the The DB creation code is run, it doesn't make any changes because the DB already matches the data model.</span></span>

<span data-ttu-id="453f9-171">Wenn die App in einer neuen Umgebung bereitgestellt wird, muss der Code für die Datenbankerstellung ausgeführt werden, damit die Datenbank erstellt wird.</span><span class="sxs-lookup"><span data-stu-id="453f9-171">When the app is deployed to a new environment, the DB creation code must be run to create the DB.</span></span>

<span data-ttu-id="453f9-172">Zuvor wurde die Verbindungszeichenfolge geändert, damit ein neuer Name für die Datenbank verwendet werden kann.</span><span class="sxs-lookup"><span data-stu-id="453f9-172">Previously the connection string was changed to use a new name for the DB.</span></span> <span data-ttu-id="453f9-173">Die angegebene Datenbank ist nicht vorhanden, daher wird die Datenbank bei der Migration erstellt.</span><span class="sxs-lookup"><span data-stu-id="453f9-173">The specified DB doesn't exist, so migrations creates the DB.</span></span>

### <a name="examine-the-data-model-snapshot"></a><span data-ttu-id="453f9-174">Momentaufnahme: Überprüfen des Datenmodells</span><span class="sxs-lookup"><span data-stu-id="453f9-174">Examine the data model snapshot</span></span>

<span data-ttu-id="453f9-175">Bei der Migration wird in *Migrations/SchoolContextModelSnapshot.cs* eine *Momentaufnahme* des aktuellen Datenbankschemas erstellt:</span><span class="sxs-lookup"><span data-stu-id="453f9-175">Migrations creates a *snapshot* of the current DB schema in *Migrations/SchoolContextModelSnapshot.cs*:</span></span>

[!code-csharp[Main](intro/samples/cu/Migrations/SchoolContextModelSnapshot1.cs?name=snippet_Truncate)]

<span data-ttu-id="453f9-176">Da das aktuelle Datenbankschema in Code dargestellt wird, muss EF Core zur Erstellung von Migrationen nicht mit der Datenbank interagieren.</span><span class="sxs-lookup"><span data-stu-id="453f9-176">Because the current DB schema is represented in code, EF Core doesn't have to interact with the DB to create migrations.</span></span> <span data-ttu-id="453f9-177">Wenn Sie eine Migration hinzufügen, bestimmt EF Core die vorgenommenen Änderungen, indem es das Datenmodell mit der Momentaufnahmedatei vergleicht.</span><span class="sxs-lookup"><span data-stu-id="453f9-177">When you add a migration, EF Core determines what changed by comparing the data model to the snapshot file.</span></span> <span data-ttu-id="453f9-178">EF Core interagiert nur dann mit der Datenbank, wenn diese aktualisiert werden muss.</span><span class="sxs-lookup"><span data-stu-id="453f9-178">EF Core interacts with the DB only when it has to update the DB.</span></span>

<span data-ttu-id="453f9-179">Die Momentaufnahmedatei muss mit den Migrationen synchron sein, über die diese erstellt wurde.</span><span class="sxs-lookup"><span data-stu-id="453f9-179">The snapshot file must be in sync with the migrations that created it.</span></span> <span data-ttu-id="453f9-180">Eine Migration kann nicht durch Löschen der Datei *\<timestamp>_\<migrationname>.cs* entfernt werden.</span><span class="sxs-lookup"><span data-stu-id="453f9-180">A migration can't be removed by deleting the file named *\<timestamp>_\<migrationname>.cs*.</span></span> <span data-ttu-id="453f9-181">Wenn diese Datei gelöscht wird, sind die übrigen Migrationen mit der Momentaufnahmedatei der Datenbank nicht synchron.</span><span class="sxs-lookup"><span data-stu-id="453f9-181">If that file is deleted, the remaining migrations are out of sync with the DB snapshot file.</span></span> <span data-ttu-id="453f9-182">Verwenden Sie den Befehl [dotnet ef migrations remove](https://docs.microsoft.com/ef/core/miscellaneous/cli/dotnet#dotnet-ef-migrations-remove), um die letzte hinzugefügte Migration zu löschen.</span><span class="sxs-lookup"><span data-stu-id="453f9-182">To delete the last migration added, use the [dotnet ef migrations remove](https://docs.microsoft.com/ef/core/miscellaneous/cli/dotnet#dotnet-ef-migrations-remove) command.</span></span>

## <a name="remove-ensurecreated"></a><span data-ttu-id="453f9-183">Entfernen von EnsureCreated</span><span class="sxs-lookup"><span data-stu-id="453f9-183">Remove EnsureCreated</span></span>

<span data-ttu-id="453f9-184">Zu einem frühen Entwicklungszeitpunkt wurde der Befehl `EnsureCreated` verwendet.</span><span class="sxs-lookup"><span data-stu-id="453f9-184">For early development, the `EnsureCreated` command was used.</span></span> <span data-ttu-id="453f9-185">In diesem Tutorial werden Migrationen verwendet.</span><span class="sxs-lookup"><span data-stu-id="453f9-185">In this tutorial, migrations is used.</span></span> <span data-ttu-id="453f9-186">Der Befehl `EnsureCreated` weist folgende Einschränkungen auf:</span><span class="sxs-lookup"><span data-stu-id="453f9-186">`EnsureCreated` has the following limitations:</span></span>

* <span data-ttu-id="453f9-187">Er umgeht Migrationen und erstellt die Datenbank und das Schema.</span><span class="sxs-lookup"><span data-stu-id="453f9-187">Bypasses migrations and creates the DB and schema.</span></span>
* <span data-ttu-id="453f9-188">Er erstellt keine Migrationstabelle.</span><span class="sxs-lookup"><span data-stu-id="453f9-188">Doesn't create a migrations table.</span></span>
* <span data-ttu-id="453f9-189">Er kann *nicht* mit Migrationen verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="453f9-189">Can *not* be used with migrations.</span></span>
* <span data-ttu-id="453f9-190">Er dient zum Testen oder schnellen Erstellen von Prototypen an Positionen, an denen die Datenbank häufig gelöscht und neu erstellt wird.</span><span class="sxs-lookup"><span data-stu-id="453f9-190">Is designed for testing or rapid prototyping where the DB is dropped and re-created frequently.</span></span>

<span data-ttu-id="453f9-191">Entfernen Sie die folgende Zeile aus `DbInitializer`:</span><span class="sxs-lookup"><span data-stu-id="453f9-191">Remove the following line from `DbInitializer`:</span></span>

```csharp
context.Database.EnsureCreated();
```

## <a name="apply-the-migration-to-the-db-in-development"></a><span data-ttu-id="453f9-192">Anwenden der Migration auf die Datenbank in der Entwicklung</span><span class="sxs-lookup"><span data-stu-id="453f9-192">Apply the migration to the DB in development</span></span>

<span data-ttu-id="453f9-193">Geben Sie im Befehlsfenster Folgendes ein, um die Datenbank und Tabellen zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="453f9-193">In the command window, enter the following to create the DB and tables.</span></span>

```console
dotnet ef database update
```

<span data-ttu-id="453f9-194">Hinweis: Wenn der Befehl `update` den Fehler „Fehler beim Buildvorgang.“ zurückgibt:</span><span class="sxs-lookup"><span data-stu-id="453f9-194">Note: If the `update` command returns the error "Build failed.":</span></span>

* <span data-ttu-id="453f9-195">Führen Sie den Befehl erneut aus.</span><span class="sxs-lookup"><span data-stu-id="453f9-195">Run the command again.</span></span>
* <span data-ttu-id="453f9-196">Schlägt er erneut fehl, beenden Sie Visual Studio, und führen Sie anschließend den Befehl `update` aus.</span><span class="sxs-lookup"><span data-stu-id="453f9-196">If it fails again, exit Visual Studio and then run the `update` command.</span></span>
* <span data-ttu-id="453f9-197">Hinterlassen Sie unten auf der Seite eine Nachricht.</span><span class="sxs-lookup"><span data-stu-id="453f9-197">Leave a message at the bottom of the page.</span></span>

<span data-ttu-id="453f9-198">Die Ausgabe des Befehls ähnelt der des Befehls `migrations add`.</span><span class="sxs-lookup"><span data-stu-id="453f9-198">The output from the command is similar to the `migrations add` command output.</span></span> <span data-ttu-id="453f9-199">Im vorangehenden Befehl werden Protokolle für die SQL-Befehle angezeigt, mit denen die Datenbank eingerichtet wurde.</span><span class="sxs-lookup"><span data-stu-id="453f9-199">In the preceding command, logs for the SQL commands that set up the DB are displayed.</span></span> <span data-ttu-id="453f9-200">In der folgenden Beispielausgabe werden die meisten Protokolle ausgelassen:</span><span class="sxs-lookup"><span data-stu-id="453f9-200">Most of the logs are omitted in the following sample output:</span></span>

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

<span data-ttu-id="453f9-201">Wenn diese Detailebene in Protokollnachrichten nicht angezeigt werden soll, können Sie die Protokollebene in der Datei *appsettings.Development.json* ändern.</span><span class="sxs-lookup"><span data-stu-id="453f9-201">To reduce the level of detail in log messages, can change the log levels in the *appsettings.Development.json* file.</span></span> <span data-ttu-id="453f9-202">Weitere Informationen finden Sie unter [Einführung in die Protokollierung](xref:fundamentals/logging/index).</span><span class="sxs-lookup"><span data-stu-id="453f9-202">For more information, see [Introduction to logging](xref:fundamentals/logging/index).</span></span>

<span data-ttu-id="453f9-203">Verwenden Sie den **SQL Server-Objekt-Explorer** zur Untersuchung der Datenbank.</span><span class="sxs-lookup"><span data-stu-id="453f9-203">Use **SQL Server Object Explorer** to inspect the DB.</span></span> <span data-ttu-id="453f9-204">Beachten Sie die zusätzliche Tabelle`__EFMigrationsHistory`.</span><span class="sxs-lookup"><span data-stu-id="453f9-204">Notice the addition of an `__EFMigrationsHistory` table.</span></span> <span data-ttu-id="453f9-205">In der Tabelle `__EFMigrationsHistory` wird nachverfolgt, welche Migrationen auf die Datenbank angewendet wurden.</span><span class="sxs-lookup"><span data-stu-id="453f9-205">The `__EFMigrationsHistory` table keeps track of which migrations have been applied to the DB.</span></span> <span data-ttu-id="453f9-206">Wenn Sie die Daten in der Tabelle `__EFMigrationsHistory` anzeigen, wird eine Zeile für die erste Migration angezeigt.</span><span class="sxs-lookup"><span data-stu-id="453f9-206">View the data in the `__EFMigrationsHistory` table, it shows one row for the first migration.</span></span> <span data-ttu-id="453f9-207">Im letzten Protokoll im vorherigen Beispiel der CLI-Ausgabe wird die INSERT-Anweisung angezeigt, mit der diese Zeile erstellt wird.</span><span class="sxs-lookup"><span data-stu-id="453f9-207">The last log in the preceding CLI output example shows the INSERT statement that creates this row.</span></span>

<span data-ttu-id="453f9-208">Führen Sie die App aus, und überprüfen Sie, ob alles funktioniert.</span><span class="sxs-lookup"><span data-stu-id="453f9-208">Run the app and verify that everything works.</span></span>

## <a name="appling-migrations-in-production"></a><span data-ttu-id="453f9-209">Anwenden von Migrationen in der Produktionsumgebung</span><span class="sxs-lookup"><span data-stu-id="453f9-209">Appling migrations in production</span></span>

<span data-ttu-id="453f9-210">Es wird empfohlen, dass Produktions-Apps [Database.Migrate](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.relationaldatabasefacadeextensions.migrate?view=efcore-2.0#Microsoft_EntityFrameworkCore_RelationalDatabaseFacadeExtensions_Migrate_Microsoft_EntityFrameworkCore_Infrastructure_DatabaseFacade_) beim Anwendungsstart **nicht** aufrufen.</span><span class="sxs-lookup"><span data-stu-id="453f9-210">We recommend production apps should **not** call [Database.Migrate](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.relationaldatabasefacadeextensions.migrate?view=efcore-2.0#Microsoft_EntityFrameworkCore_RelationalDatabaseFacadeExtensions_Migrate_Microsoft_EntityFrameworkCore_Infrastructure_DatabaseFacade_) at application startup.</span></span> <span data-ttu-id="453f9-211">`Migrate` sollte in der Serverfarm nicht über eine App aufgerufen werden.</span><span class="sxs-lookup"><span data-stu-id="453f9-211">`Migrate` shouldn't be called from an app in server farm.</span></span> <span data-ttu-id="453f9-212">Beispielsweise wenn die App über eine Cloud mit horizontaler Skalierung bereitgestellt wurde (mehrere Instanzen der App werden ausgeführt).</span><span class="sxs-lookup"><span data-stu-id="453f9-212">For example, if the app has been cloud deployed with scale-out (multiple instances of the app are running).</span></span>

<span data-ttu-id="453f9-213">Die Datenbankmigration sollte im Rahmen der Bereitstellung und auf kontrollierte Weise erfolgen.</span><span class="sxs-lookup"><span data-stu-id="453f9-213">Database migration should be done as part of deployment, and in a controlled way.</span></span> <span data-ttu-id="453f9-214">Zu den Ansätzen für die Migration von Produktionsdatenbanken zählen die folgenden:</span><span class="sxs-lookup"><span data-stu-id="453f9-214">Production database migration approaches include:</span></span>

* <span data-ttu-id="453f9-215">Verwendung von Migrationen zur Erstellung von SQL-Skripts und Verwendung der SQL-Skripts bei der Bereitstellung.</span><span class="sxs-lookup"><span data-stu-id="453f9-215">Using migrations to create SQL scripts and using the SQL scripts in deployment.</span></span>
* <span data-ttu-id="453f9-216">Ausführen von `dotnet ef database update` über eine kontrollierte Umgebung.</span><span class="sxs-lookup"><span data-stu-id="453f9-216">Running `dotnet ef database update` from a controlled environment.</span></span>

<span data-ttu-id="453f9-217">EF Core kann über die Tabelle `__MigrationsHistory` sehen, ob Migrationen ausgeführt werden müssen.</span><span class="sxs-lookup"><span data-stu-id="453f9-217">EF Core uses the `__MigrationsHistory` table to see if any migrations need to run.</span></span> <span data-ttu-id="453f9-218">Wenn die Datenbank auf dem neuesten Stand ist, wird keine Migration ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="453f9-218">If the DB is up to date, no migration is run.</span></span>

<a id="pmc"></a>
## <a name="command-line-interface-cli-vs-package-manager-console-pmc"></a><span data-ttu-id="453f9-219">Die Befehlszeilenschnittstelle (Command-line Interface, CLI) im Vergleich mit der Paket-Manager-Konsole (Package Manager Console, PMC) im Vergleich</span><span class="sxs-lookup"><span data-stu-id="453f9-219">Command-line interface (CLI) vs. Package Manager Console (PMC)</span></span>

<span data-ttu-id="453f9-220">Die EF Core-Tools für die Verwaltung von Migrationen sind verfügbar über:</span><span class="sxs-lookup"><span data-stu-id="453f9-220">The EF Core tooling for managing migrations is available from:</span></span>

* <span data-ttu-id="453f9-221">.NET Core-CLI-Befehle.</span><span class="sxs-lookup"><span data-stu-id="453f9-221">.NET Core CLI commands.</span></span>
* <span data-ttu-id="453f9-222">Die PowerShell-Cmdlets im Visual Studio-Fenster **Paket-Manager-Konsole** (Package Manager Console, PMC).</span><span class="sxs-lookup"><span data-stu-id="453f9-222">The PowerShell cmdlets in the Visual Studio **Package Manager Console** (PMC) window.</span></span>

<span data-ttu-id="453f9-223">In diesem Tutorial wird die Verwendungsweise der CLI erläutert. Einige Entwickler bevorzugen die Verwendung der PMC.</span><span class="sxs-lookup"><span data-stu-id="453f9-223">This tutorial shows how to use the CLI, some developers prefer using the PMC.</span></span>

<span data-ttu-id="453f9-224">Die EF Core-Befehle für die PMC sind im Paket [Microsoft.EntityFrameworkCore.Tools](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools) enthalten.</span><span class="sxs-lookup"><span data-stu-id="453f9-224">The EF Core commands for the PMC are in the [Microsoft.EntityFrameworkCore.Tools](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools) package.</span></span> <span data-ttu-id="453f9-225">Dieses Paket ist im Metapaket [Microsoft.AspNetCore.All](xref:fundamentals/metapackage) enthalten. Eine Installation ist daher nicht erforderlich.</span><span class="sxs-lookup"><span data-stu-id="453f9-225">This package is included in the [Microsoft.AspNetCore.All](xref:fundamentals/metapackage) metapackage, so you don't have to install it.</span></span>

<span data-ttu-id="453f9-226">**Wichtig:** Dieses Paket ist nicht mit dem Paket identisch, das Sie für die CLI durch Bearbeitung der Datei *.csproj* installiert haben.</span><span class="sxs-lookup"><span data-stu-id="453f9-226">**Important:** This isn't the same package as the one you install for the CLI by editing the *.csproj* file.</span></span> <span data-ttu-id="453f9-227">Der Name dieses Pakets endet mit `Tools`, im Gegensatz zum Namen des CLI-Pakets, der mit `Tools.DotNet` endet.</span><span class="sxs-lookup"><span data-stu-id="453f9-227">The name of this one ends in `Tools`, unlike the CLI package name which ends in `Tools.DotNet`.</span></span>

<span data-ttu-id="453f9-228">Weitere Informationen zu CLI-Befehlen finden Sie unter [.NET Core-CLI](https://docs.microsoft.com/ef/core/miscellaneous/cli/dotnet).</span><span class="sxs-lookup"><span data-stu-id="453f9-228">For more information about the CLI commands, see [.NET Core CLI](https://docs.microsoft.com/ef/core/miscellaneous/cli/dotnet).</span></span>

<span data-ttu-id="453f9-229">Weitere Informationen zu den PMC-Befehlen finden Sie unter [Paket-Manager-Konsole (Visual Studio)](https://docs.microsoft.com/ef/core/miscellaneous/cli/powershell).</span><span class="sxs-lookup"><span data-stu-id="453f9-229">For more information about the PMC commands, see [Package Manager Console (Visual Studio)](https://docs.microsoft.com/ef/core/miscellaneous/cli/powershell).</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="453f9-230">Problembehandlung</span><span class="sxs-lookup"><span data-stu-id="453f9-230">Troubleshooting</span></span>

<span data-ttu-id="453f9-231">Laden Sie die [abgeschlossene App für diese Phase](
https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part4-migrations) herunter.</span><span class="sxs-lookup"><span data-stu-id="453f9-231">Download the [completed app for this stage](
https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part4-migrations).</span></span>

<span data-ttu-id="453f9-232">Die App generiert folgende Ausnahme:</span><span class="sxs-lookup"><span data-stu-id="453f9-232">The app generates the following exception:</span></span>

```text
`SqlException: Cannot open database "ContosoUniversity" requested by the login.
The login failed.
Login failed for user 'user name'.
```

<span data-ttu-id="453f9-233">Lösung: Führen Sie `dotnet ef database update` aus.</span><span class="sxs-lookup"><span data-stu-id="453f9-233">Solution: Run `dotnet ef database update`</span></span>

<span data-ttu-id="453f9-234">Wenn der Befehl `update` den Fehler „Fehler beim Buildvorgang“ zurückgibt:</span><span class="sxs-lookup"><span data-stu-id="453f9-234">If the `update` command returns the error "Build failed.":</span></span>

* <span data-ttu-id="453f9-235">Führen Sie den Befehl erneut aus.</span><span class="sxs-lookup"><span data-stu-id="453f9-235">Run the command again.</span></span>
* <span data-ttu-id="453f9-236">Hinterlassen Sie unten auf der Seite eine Nachricht.</span><span class="sxs-lookup"><span data-stu-id="453f9-236">Leave a message at the bottom of the page.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="453f9-237">[Zurück](xref:data/ef-rp/sort-filter-page)
[Weiter](xref:data/ef-rp/complex-data-model)</span><span class="sxs-lookup"><span data-stu-id="453f9-237">[Previous](xref:data/ef-rp/sort-filter-page)
[Next](xref:data/ef-rp/complex-data-model)</span></span>
