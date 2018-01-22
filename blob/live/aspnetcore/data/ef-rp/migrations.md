---
title: Razor-Seiten mit EF-Kern - Migrationen - 4 von 8
author: rick-anderson
description: "In diesem Lernprogramm beginnen Sie mit der EF-Core-Migrationen-Funktion für die Verwaltung von datenmodelländerungen in einer ASP.NET-MVC-Anwendung Core."
ms.author: riande
manager: wpickett
ms.date: 10/15/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: data/ef-rp/migrations
ms.openlocfilehash: 26fbda99b0c1dfa2d09cf387e43f3123c58215f8
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/19/2018
---
# <a name="migrations---ef-core-with-razor-pages-tutorial-4-of-8"></a><span data-ttu-id="c64d2-103">Migrationen - EF-Core mit Razor-Seiten Lernprogramm (4 von 8)</span><span class="sxs-lookup"><span data-stu-id="c64d2-103">Migrations - EF Core with Razor Pages tutorial (4 of 8)</span></span>

<span data-ttu-id="c64d2-104">Durch [Tom Dykstra](https://github.com/tdykstra), [Jon P Smith](https://twitter.com/thereformedprog), und [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="c64d2-104">By [Tom Dykstra](https://github.com/tdykstra), [Jon P Smith](https://twitter.com/thereformedprog), and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

[!INCLUDE[about the series](../../includes/RP-EF/intro.md)]

<span data-ttu-id="c64d2-105">In diesem Lernprogramm wird das EF Hauptfeature-Migrationen für die Verwaltung von datenmodelländerungen verwendet.</span><span class="sxs-lookup"><span data-stu-id="c64d2-105">In this tutorial, the EF Core migrations feature for managing data model changes is used.</span></span>

<span data-ttu-id="c64d2-106">Wenn Probleme können nicht zu lösen auftreten, laden Sie die [abgeschlossene Anwendung für diese Stufe](
https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part4-migrations).</span><span class="sxs-lookup"><span data-stu-id="c64d2-106">If you run into problems you can't solve, download the [completed app for this stage](
https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part4-migrations).</span></span>

<span data-ttu-id="c64d2-107">Wenn eine neue app entwickelt wird, werden Änderungen in die Daten häufig modellieren.</span><span class="sxs-lookup"><span data-stu-id="c64d2-107">When a new app is developed, the data model changes frequently.</span></span> <span data-ttu-id="c64d2-108">Jedes Mal Ruft das Modell ändert, das Modell nicht synchron mit der Datenbank.</span><span class="sxs-lookup"><span data-stu-id="c64d2-108">Each time the model changes, the model gets out of sync with the database.</span></span> <span data-ttu-id="c64d2-109">Dieses Lernprogramm gestartet, indem Sie die Konfiguration des Entity Framework zum Erstellen der Datenbank, wenn er nicht vorhanden ist.</span><span class="sxs-lookup"><span data-stu-id="c64d2-109">This tutorial started by configuring the Entity Framework to create the database if it doesn't exist.</span></span> <span data-ttu-id="c64d2-110">Jedes Mal, die das Datenmodell ändert:</span><span class="sxs-lookup"><span data-stu-id="c64d2-110">Each time the data model changes:</span></span>

* <span data-ttu-id="c64d2-111">Die Datenbank wird gelöscht.</span><span class="sxs-lookup"><span data-stu-id="c64d2-111">The DB is dropped.</span></span>
* <span data-ttu-id="c64d2-112">EF erstellt ein neues Konto, das das Modell entspricht.</span><span class="sxs-lookup"><span data-stu-id="c64d2-112">EF creates a new one that matches the model.</span></span>
* <span data-ttu-id="c64d2-113">Die app startet die Datenbank mit der Testdaten.</span><span class="sxs-lookup"><span data-stu-id="c64d2-113">The app seeds the DB with test data.</span></span>

<span data-ttu-id="c64d2-114">Dieser Ansatz zum Synchronisieren der Datenbank mit dem Datenmodell funktioniert gut, bis Sie die app in die Produktion bereitstellen.</span><span class="sxs-lookup"><span data-stu-id="c64d2-114">This approach to keeping the DB in sync with the data model works well until you deploy the app to production.</span></span> <span data-ttu-id="c64d2-115">Wenn die app in der produktionsumgebung ausgeführt wird, wird es in der Regel Daten speichern, die beibehalten werden muss.</span><span class="sxs-lookup"><span data-stu-id="c64d2-115">When the app is running in production, it is usually storing data that needs to be maintained.</span></span> <span data-ttu-id="c64d2-116">Die app kann nicht in einem DB-Test starten jedes Mal eine Änderung vorgenommen wird, (z. B. das Hinzufügen einer neuen Spalte).</span><span class="sxs-lookup"><span data-stu-id="c64d2-116">The app can't start with a test DB each time a change is made (such as adding a new column).</span></span> <span data-ttu-id="c64d2-117">Das Feature EF Core Migrationen löst dieses Problem durch Aktivieren von EF Core zum Aktualisieren des Schemas DB, anstatt eine neue Datenbank erstellen.</span><span class="sxs-lookup"><span data-stu-id="c64d2-117">The EF Core Migrations feature solves this problem by enabling EF Core to update the DB schema instead of creating a new DB.</span></span>

<span data-ttu-id="c64d2-118">Anstatt löschen und Neuerstellen der Datenbank, wenn das Datenmodell ändert, werden Migrationen aktualisiert das Schema und behält die vorhandene Daten.</span><span class="sxs-lookup"><span data-stu-id="c64d2-118">Rather than dropping and recreating the DB when the data model changes, migrations updates the schema and retains existing data.</span></span>

## <a name="entity-framework-core-nuget-packages-for-migrations"></a><span data-ttu-id="c64d2-119">NuGet-Pakete für Migrationen in Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="c64d2-119">Entity Framework Core NuGet packages for migrations</span></span>

<span data-ttu-id="c64d2-120">Verwenden Sie zum Arbeiten mit Migrationen der **Package Manager Console** (PMC) oder die Befehlszeilenschnittstelle (CLI).</span><span class="sxs-lookup"><span data-stu-id="c64d2-120">To work with migrations, use the **Package Manager Console** (PMC) or the command-line interface (CLI).</span></span> <span data-ttu-id="c64d2-121">Diese Lernprogramme veranschaulichen, wie CLI-Befehlen.</span><span class="sxs-lookup"><span data-stu-id="c64d2-121">These tutorials show how to use CLI commands.</span></span> <span data-ttu-id="c64d2-122">Informationen zu der Systemmonitor ist am [Ende dieses Lernprogramms](#pmc).</span><span class="sxs-lookup"><span data-stu-id="c64d2-122">Information about the PMC is at [the end of this tutorial](#pmc).</span></span>

<span data-ttu-id="c64d2-123">Der EF-Core-Tools für die Befehlszeilenschnittstelle (CLI) finden Sie unter [Microsoft.EntityFrameworkCore.Tools.DotNet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools.DotNet).</span><span class="sxs-lookup"><span data-stu-id="c64d2-123">The EF Core tools for the command-line interface (CLI) are provided in [Microsoft.EntityFrameworkCore.Tools.DotNet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools.DotNet).</span></span> <span data-ttu-id="c64d2-124">Um dieses Paket zu installieren, fügen sie der `DotNetCliToolReference` Sammlung in der *csproj* Datei wie gezeigt.</span><span class="sxs-lookup"><span data-stu-id="c64d2-124">To install this package, add it to the `DotNetCliToolReference` collection in the *.csproj* file, as shown.</span></span> <span data-ttu-id="c64d2-125">**Hinweis:** dieses Paket muss installiert werden, indem Sie bearbeiten die *csproj* Datei.</span><span class="sxs-lookup"><span data-stu-id="c64d2-125">**Note:** This package must be installed by editing the *.csproj* file.</span></span> <span data-ttu-id="c64d2-126">Die`install-package` Befehl oder die Paket-Manager-GUI nicht zum Installieren dieses Pakets verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="c64d2-126">The`install-package` command or the package manager GUI cannot be used to install this package.</span></span> <span data-ttu-id="c64d2-127">Bearbeiten der *csproj* Datei mit der rechten Maustaste auf den Projektnamen im **Projektmappen-Explorer** auswählen und **ContosoUniversity.csproj bearbeiten**.</span><span class="sxs-lookup"><span data-stu-id="c64d2-127">Edit the *.csproj* file by right-clicking the project name in **Solution Explorer** and selecting **Edit ContosoUniversity.csproj**.</span></span>

<span data-ttu-id="c64d2-128">Das folgende Markup zeigt die aktualisierte *csproj* Datei mit hervorgehobenen EF Core CLI-Tools:</span><span class="sxs-lookup"><span data-stu-id="c64d2-128">The following markup shows the updated *.csproj* file with the EF Core CLI tools highlighted:</span></span>

[!code-xml[](intro/samples/cu/ContosoUniversity.csproj?highlight=12)]
  
<span data-ttu-id="c64d2-129">Die Versionsnummern im vorherigen Beispiel wurden beim Erstellen des Lernprogramms geschrieben wurde.</span><span class="sxs-lookup"><span data-stu-id="c64d2-129">The version numbers in the preceding example were current when the tutorial was written.</span></span> <span data-ttu-id="c64d2-130">Verwenden Sie die gleiche Version, für die EF Core CLI-Tools, die in die anderen Pakete gefunden.</span><span class="sxs-lookup"><span data-stu-id="c64d2-130">Use the same version for the EF Core CLI tools found in the other packages.</span></span>

## <a name="change-the-connection-string"></a><span data-ttu-id="c64d2-131">Ändern der Verbindungszeichenfolge</span><span class="sxs-lookup"><span data-stu-id="c64d2-131">Change the connection string</span></span>

<span data-ttu-id="c64d2-132">In der *appsettings.json* Datei, ändern Sie den Namen der Datenbank in der Verbindungszeichenfolge auf ContosoUniversity2.</span><span class="sxs-lookup"><span data-stu-id="c64d2-132">In the *appsettings.json* file, change the name of the DB in the connection string to ContosoUniversity2.</span></span>

[!code-json[Main](intro/samples/cu/appsettings2.json?range=1-4)]

<span data-ttu-id="c64d2-133">Ändern der DB-Name in der Verbindungszeichenfolge bewirkt, dass die erste Migration zu eine neue Datenbank zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="c64d2-133">Changing the DB name in the connection string causes the first migration to create a new DB.</span></span> <span data-ttu-id="c64d2-134">Eine neue Datenbank wird erstellt, da eine mit diesem Namen nicht vorhanden ist.</span><span class="sxs-lookup"><span data-stu-id="c64d2-134">A new DB is created because one with that name does not exist.</span></span> <span data-ttu-id="c64d2-135">Die Verbindungszeichenfolge ändern, ist nicht erforderlich für erste Schritte mit Migrationen.</span><span class="sxs-lookup"><span data-stu-id="c64d2-135">Changing the connection string isn't required for getting started with migrations.</span></span>

<span data-ttu-id="c64d2-136">Eine Alternative zum Ändern der DB-Name ist die Datenbank gelöscht.</span><span class="sxs-lookup"><span data-stu-id="c64d2-136">An alternative to changing the DB name is deleting the DB.</span></span> <span data-ttu-id="c64d2-137">Verwendung **Objekt-Explorer von SQL Server** (SSOX) oder die `database drop` CLI-Befehl:</span><span class="sxs-lookup"><span data-stu-id="c64d2-137">Use **SQL Server Object Explorer** (SSOX) or the `database drop` CLI command:</span></span>

 ```console
 dotnet ef database drop
 ```

<span data-ttu-id="c64d2-138">Im folgende Abschnitt wird erläutert, wie CLI-Befehle ausgeführt werden.</span><span class="sxs-lookup"><span data-stu-id="c64d2-138">The following section explains how to run CLI commands.</span></span>

## <a name="create-an-initial-migration"></a><span data-ttu-id="c64d2-139">Erstellen einer anfänglichen Migrations</span><span class="sxs-lookup"><span data-stu-id="c64d2-139">Create an initial migration</span></span>

<span data-ttu-id="c64d2-140">Erstellen Sie das Projekt.</span><span class="sxs-lookup"><span data-stu-id="c64d2-140">Build the project.</span></span>

<span data-ttu-id="c64d2-141">Öffnen Sie ein Befehlsfenster, und navigieren Sie zum Projektordner.</span><span class="sxs-lookup"><span data-stu-id="c64d2-141">Open a command window and navigate to the project folder.</span></span> <span data-ttu-id="c64d2-142">Enthält der Projektordner der *Startup.cs* Datei.</span><span class="sxs-lookup"><span data-stu-id="c64d2-142">The project folder contains the *Startup.cs* file.</span></span>

<span data-ttu-id="c64d2-143">Geben Sie im Fenster Eingabeaufforderung Folgendes ein:</span><span class="sxs-lookup"><span data-stu-id="c64d2-143">Enter the following in the command window:</span></span>

```console
dotnet ef migrations add InitialCreate
```

<span data-ttu-id="c64d2-144">Das Befehlsfenster werden Informationen, die etwa wie folgt angezeigt:</span><span class="sxs-lookup"><span data-stu-id="c64d2-144">The command window displays information similar to the following:</span></span>

```console
info: Microsoft.AspNetCore.DataProtection.KeyManagement.XmlKeyManager[0]
      User profile is available. Using 'C:\Users\username\AppData\Local\ASP.NET\DataProtection-Keys' as key repository and Windows DPAPI to encrypt keys at rest.
info: Microsoft.EntityFrameworkCore.Infrastructure[100403]
      Entity Framework Core 2.0.0-rtm-26452 initialized 'SchoolContext' using provider 'Microsoft.EntityFrameworkCore.SqlServer' with options: None
Done. To undo this action, use 'ef migrations remove'
```

<span data-ttu-id="c64d2-145">Bei einem mit der Meldung Migrationsfehler "*... die Datei kann nicht zugegriffen werden. ContosoUniversity.dll, da sie von einem anderen Prozess verwendet wird.* "</span><span class="sxs-lookup"><span data-stu-id="c64d2-145">If the migration fails with the message "*cannot access the file ... ContosoUniversity.dll because it is being used by another process.*"</span></span> <span data-ttu-id="c64d2-146">wird angezeigt:</span><span class="sxs-lookup"><span data-stu-id="c64d2-146">is displayed:</span></span>

* <span data-ttu-id="c64d2-147">Beenden von IIS Express.</span><span class="sxs-lookup"><span data-stu-id="c64d2-147">Stop IIS Express.</span></span>

   * <span data-ttu-id="c64d2-148">Beenden und starten Sie Visual Studio oder</span><span class="sxs-lookup"><span data-stu-id="c64d2-148">Exit and restart Visual Studio, or</span></span>
   * <span data-ttu-id="c64d2-149">Suchen Sie das Symbol "IIS Express" in der Windows-Taskleiste.</span><span class="sxs-lookup"><span data-stu-id="c64d2-149">Find the IIS Express icon in the Windows System Tray.</span></span>
   * <span data-ttu-id="c64d2-150">Maustaste auf das Symbol "IIS Express", und klicken Sie dann auf **ContosoUniversity > Standort beenden**.</span><span class="sxs-lookup"><span data-stu-id="c64d2-150">Right-click the IIS Express icon, and then click **ContosoUniversity > Stop Site**.</span></span>

<span data-ttu-id="c64d2-151">Wenn die Fehlermeldung "Fehler beim Erstellen des."</span><span class="sxs-lookup"><span data-stu-id="c64d2-151">If the error message "Build failed."</span></span> <span data-ttu-id="c64d2-152">wird angezeigt, führen den Befehl erneut aus.</span><span class="sxs-lookup"><span data-stu-id="c64d2-152">is displayed, run the command again.</span></span> <span data-ttu-id="c64d2-153">Wenn Sie diesen Fehler erhalten, lassen Sie sich am Ende dieses Lernprogramms.</span><span class="sxs-lookup"><span data-stu-id="c64d2-153">If you get this error, leave a note at the bottom of this tutorial.</span></span>

### <a name="examine-the-up-and-down-methods"></a><span data-ttu-id="c64d2-154">Überprüfen Sie die nach-oben und nach-unten Sie-Methoden</span><span class="sxs-lookup"><span data-stu-id="c64d2-154">Examine the Up and Down methods</span></span>

<span data-ttu-id="c64d2-155">Der EF-Core-Befehl `migrations add` generiert Code zum Erstellen der Datenbank aus.</span><span class="sxs-lookup"><span data-stu-id="c64d2-155">The EF Core command `migrations add` generated code to create the DB from.</span></span> <span data-ttu-id="c64d2-156">Dieser Code Migrationen befindet sich in der *Migrationen\<Zeitstempel > _InitialCreate.cs* Datei.</span><span class="sxs-lookup"><span data-stu-id="c64d2-156">This migrations code is in the *Migrations\<timestamp>_InitialCreate.cs* file.</span></span> <span data-ttu-id="c64d2-157">Die `Up` Methode der `InitialCreate` Klasse erstellt, die DB-Tabellen, die die Daten Modell Entitätenmengen entsprechen.</span><span class="sxs-lookup"><span data-stu-id="c64d2-157">The `Up` method of the `InitialCreate` class creates the DB tables that correspond to the data model entity sets.</span></span> <span data-ttu-id="c64d2-158">Die `Down` Methode löscht, wie im folgenden Beispiel gezeigt:</span><span class="sxs-lookup"><span data-stu-id="c64d2-158">The `Down` method deletes them, as shown in the following example:</span></span>

[!code-csharp[Main](intro/samples/cu/Migrations/20171026010210_InitialCreate.cs?range=8-24,77-)]

<span data-ttu-id="c64d2-159">Migrationen Aufrufe der `Up` Methode für die Implementierung des Datenmodells für eine Migration.</span><span class="sxs-lookup"><span data-stu-id="c64d2-159">Migrations calls the `Up` method to implement the data model changes for a migration.</span></span> <span data-ttu-id="c64d2-160">Bei der Eingabe eines Befehls ein Rollback der Updates, die Migrationen Aufrufe der `Down` Methode.</span><span class="sxs-lookup"><span data-stu-id="c64d2-160">When you enter a command to roll back the update, migrations calls the `Down` method.</span></span>

<span data-ttu-id="c64d2-161">Der vorhergehende Code ist der anfänglichen Migration.</span><span class="sxs-lookup"><span data-stu-id="c64d2-161">The preceding code is for the initial migration.</span></span> <span data-ttu-id="c64d2-162">Dieser Code wurde erstellt, wenn die `migrations add InitialCreate` -Befehl ausgeführt wurde.</span><span class="sxs-lookup"><span data-stu-id="c64d2-162">That code was created when the `migrations add InitialCreate` command was run.</span></span> <span data-ttu-id="c64d2-163">Der Namensparameter der Migration (im Beispiel "InitialCreate") wird für den Dateinamen verwendet.</span><span class="sxs-lookup"><span data-stu-id="c64d2-163">The migration name parameter ("InitialCreate" in the example) is used for the file name.</span></span> <span data-ttu-id="c64d2-164">Der migrationsname kann einen beliebigen gültigen Dateinamen.</span><span class="sxs-lookup"><span data-stu-id="c64d2-164">The migration name can be any valid file name.</span></span> <span data-ttu-id="c64d2-165">Es wird empfohlen, wählen ein Wort oder Ausdruck, die zusammengefasst, was bei der Migration durchgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="c64d2-165">It's best to choose a word or phrase that summarizes what is being done in the migration.</span></span> <span data-ttu-id="c64d2-166">Beispielsweise könnte eine Migration, die Department-Tabelle hinzugefügt "AddDepartmentTable." aufgerufen werden</span><span class="sxs-lookup"><span data-stu-id="c64d2-166">For example, a migration that added a department table might be called "AddDepartmentTable."</span></span>

<span data-ttu-id="c64d2-167">Wenn die anfängliche Migration erstellt wird und die Datenbank beendet wird:</span><span class="sxs-lookup"><span data-stu-id="c64d2-167">If the initial migration is created and the DB exits:</span></span>

* <span data-ttu-id="c64d2-168">Der DB-Erstellung-Code wird generiert.</span><span class="sxs-lookup"><span data-stu-id="c64d2-168">The DB creation code is generated.</span></span>
* <span data-ttu-id="c64d2-169">Der DB-Erstellung Code muss nicht ausgeführt werden, da die Datenbank bereits das Datenmodell übereinstimmt.</span><span class="sxs-lookup"><span data-stu-id="c64d2-169">The DB creation code doesn't need to run because the DB already matches the data model.</span></span> <span data-ttu-id="c64d2-170">Wenn der DB-Erstellung Code ausgeführt wird, nicht der Änderungen vorgenommen werden, da die Datenbank bereits das Datenmodell übereinstimmt.</span><span class="sxs-lookup"><span data-stu-id="c64d2-170">If the The DB creation code is run, it doesn't make any changes because the DB already matches the data model.</span></span>

<span data-ttu-id="c64d2-171">Wenn die app zu einer neuen Umgebung bereitgestellt wird, muss die DB-Erstellung Code ausgeführt werden, um die Datenbank zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="c64d2-171">When the app is deployed to a new environment, the DB creation code must be run to create the DB.</span></span>

<span data-ttu-id="c64d2-172">Zuvor wurde die Verbindungszeichenfolge geändert, um einen neuen Namen für die Datenbank zu verwenden.</span><span class="sxs-lookup"><span data-stu-id="c64d2-172">Previously the connection string was changed to use a new name for the DB.</span></span> <span data-ttu-id="c64d2-173">Die angegebene Datenbank ist nicht vorhanden, damit Migrationen erstellt die Datenbank.</span><span class="sxs-lookup"><span data-stu-id="c64d2-173">The specified DB doesn't exist, so migrations creates the DB.</span></span>

### <a name="examine-the-data-model-snapshot"></a><span data-ttu-id="c64d2-174">Überprüfen Sie die datenmomentaufnahme für das Modell</span><span class="sxs-lookup"><span data-stu-id="c64d2-174">Examine the data model snapshot</span></span>

<span data-ttu-id="c64d2-175">Migrationen erstellt eine *Momentaufnahme* des aktuellen DB-Schemas in *Migrations/SchoolContextModelSnapshot.cs*:</span><span class="sxs-lookup"><span data-stu-id="c64d2-175">Migrations creates a *snapshot* of the current DB schema in *Migrations/SchoolContextModelSnapshot.cs*:</span></span>

[!code-csharp[Main](intro/samples/cu/Migrations/SchoolContextModelSnapshot1.cs?name=snippet_Truncate)]

<span data-ttu-id="c64d2-176">Da das Schema der aktuellen im Code dargestellt wird, verwendet nicht EF Kern, für die Interaktion mit der Datenbank, um Migrationen zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="c64d2-176">Because the current DB schema is represented in code, EF Core doesn't have to interact with the DB to create migrations.</span></span> <span data-ttu-id="c64d2-177">Wenn Sie eine Migration hinzufügen, bestimmt EF Core Änderungen durch das Datenmodell auf der Snapshot-Datei vergleichen.</span><span class="sxs-lookup"><span data-stu-id="c64d2-177">When you add a migration, EF Core determines what changed by comparing the data model to the snapshot file.</span></span> <span data-ttu-id="c64d2-178">EF Core interagiert mit der Datenbank, nur bei die Datenbank zu aktualisieren.</span><span class="sxs-lookup"><span data-stu-id="c64d2-178">EF Core interacts with the DB only when it has to update the DB.</span></span>

<span data-ttu-id="c64d2-179">Der Datenbankmomentaufnahme-Datei muss mit die Migrationen synchron sein, der Sie erstellt.</span><span class="sxs-lookup"><span data-stu-id="c64d2-179">The snapshot file must be in sync with the migrations that created it.</span></span> <span data-ttu-id="c64d2-180">Eine Migration kann nicht entfernt werden, durch Löschen der Datei mit dem Namen  *\<Zeitstempel > _\<Migrationname > .cs*.</span><span class="sxs-lookup"><span data-stu-id="c64d2-180">A migration can't be removed by deleting the file named *\<timestamp>_\<migrationname>.cs*.</span></span> <span data-ttu-id="c64d2-181">Wenn diese Datei gelöscht wird, werden die übrigen Migrationen werden nicht synchron mit der Snapshot-Datenbankdatei.</span><span class="sxs-lookup"><span data-stu-id="c64d2-181">If that file is deleted, the remaining migrations are out of sync with the DB snapshot file.</span></span> <span data-ttu-id="c64d2-182">Verwenden Sie zum Löschen der letzten Migrations hinzugefügt der [Dotnet Ef Migrationen entfernen](https://docs.microsoft.com/ef/core/miscellaneous/cli/dotnet#dotnet-ef-migrations-remove) Befehl.</span><span class="sxs-lookup"><span data-stu-id="c64d2-182">To delete the last migration added, use the [dotnet ef migrations remove](https://docs.microsoft.com/ef/core/miscellaneous/cli/dotnet#dotnet-ef-migrations-remove) command.</span></span>

## <a name="remove-ensurecreated"></a><span data-ttu-id="c64d2-183">EnsureCreated entfernen</span><span class="sxs-lookup"><span data-stu-id="c64d2-183">Remove EnsureCreated</span></span>

<span data-ttu-id="c64d2-184">Bei frühen-Bereitstellung müssen die `EnsureCreated` -Befehl wurde verwendet.</span><span class="sxs-lookup"><span data-stu-id="c64d2-184">For early development, the `EnsureCreated` command was used.</span></span> <span data-ttu-id="c64d2-185">In diesem Lernprogramm wird die Migrationen verwendet.</span><span class="sxs-lookup"><span data-stu-id="c64d2-185">In this tutorial, migrations is used.</span></span> <span data-ttu-id="c64d2-186">`EnsureCreated`verfügt über die folgenden Limatitions aus:</span><span class="sxs-lookup"><span data-stu-id="c64d2-186">`EnsureCreated` has the following limatitions:</span></span>

* <span data-ttu-id="c64d2-187">Migrationen umgangen wird und die DB- und das Schema erstellt.</span><span class="sxs-lookup"><span data-stu-id="c64d2-187">Bypasses migrations and creates the DB and schema.</span></span>
* <span data-ttu-id="c64d2-188">Eine Tabelle Migrationen wird nicht erstellt werden.</span><span class="sxs-lookup"><span data-stu-id="c64d2-188">Does not create a migrations table.</span></span>
* <span data-ttu-id="c64d2-189">Können *nicht* mit Migrationen verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="c64d2-189">Can *not* be used with migrations.</span></span>
* <span data-ttu-id="c64d2-190">Dient zum Testen oder schnellen Prototyping, in dem die Datenbank gelöscht und neu erstellt häufig.</span><span class="sxs-lookup"><span data-stu-id="c64d2-190">Is designed for testing or rapid prototyping where the DB is dropped and re-created frequently.</span></span>

<span data-ttu-id="c64d2-191">Entfernen Sie die folgende Zeile aus `DbInitializer`:</span><span class="sxs-lookup"><span data-stu-id="c64d2-191">Remove the following line from `DbInitializer`:</span></span>

```csharp
context.Database.EnsureCreated();
```

## <a name="apply-the-migration-to-the-db-in-development"></a><span data-ttu-id="c64d2-192">Übernehmen Sie die Migration mit der Datenbank in der Entwicklung</span><span class="sxs-lookup"><span data-stu-id="c64d2-192">Apply the migration to the DB in development</span></span>

<span data-ttu-id="c64d2-193">Geben Sie Folgendes ein, um die DB- und Tabellen zu erstellen, in das Befehlsfenster.</span><span class="sxs-lookup"><span data-stu-id="c64d2-193">In the command window, enter the following to create the DB and tables.</span></span>

```console
dotnet ef database update
```

<span data-ttu-id="c64d2-194">Hinweis: Wenn die `update` Befehl gibt dem Fehler "Fehler beim Buildvorgang.":</span><span class="sxs-lookup"><span data-stu-id="c64d2-194">Note: If the `update` command returns the error "Build failed.":</span></span>

* <span data-ttu-id="c64d2-195">Führen Sie den Befehl erneut aus.</span><span class="sxs-lookup"><span data-stu-id="c64d2-195">Run the command again.</span></span>
* <span data-ttu-id="c64d2-196">Wenn es wieder fehlschlägt, beenden Sie Visual Studio, und führen Sie dann die `update` Befehl.</span><span class="sxs-lookup"><span data-stu-id="c64d2-196">If it fails again, exit Visual Studio and then run the `update` command.</span></span>
* <span data-ttu-id="c64d2-197">Lassen Sie eine Nachricht am unteren Rand der Seite.</span><span class="sxs-lookup"><span data-stu-id="c64d2-197">Leave a message at the bottom of the page.</span></span>

<span data-ttu-id="c64d2-198">Die Ausgabe des Befehls ähnelt der `migrations add` Befehl Ausgabe.</span><span class="sxs-lookup"><span data-stu-id="c64d2-198">The output from the command is similar to the `migrations add` command output.</span></span> <span data-ttu-id="c64d2-199">In den vorherigen Befehl werden die Protokolle für die SQL-Befehle, mit dem die Datenbank eingerichtet angezeigt.</span><span class="sxs-lookup"><span data-stu-id="c64d2-199">In the preceding command, logs for the SQL commands that set up the DB are displayed.</span></span> <span data-ttu-id="c64d2-200">Die meisten Protokolle werden in der folgenden Beispielausgabe ausgelassen:</span><span class="sxs-lookup"><span data-stu-id="c64d2-200">Most of the logs are omitted in the following sample output:</span></span>

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

<span data-ttu-id="c64d2-201">Zum Reduzieren der Detailebene in Protokollnachrichten enthalten, kann die Protokollierungsstufen im Ändern der *"appSettings". Development.JSON* Datei.</span><span class="sxs-lookup"><span data-stu-id="c64d2-201">To reduce the level of detail in log messages, can change the log levels in the *appsettings.Development.json* file.</span></span> <span data-ttu-id="c64d2-202">Weitere Informationen finden Sie unter [Einführung in die Protokollierung](xref:fundamentals/logging/index).</span><span class="sxs-lookup"><span data-stu-id="c64d2-202">For more information, see [Introduction to logging](xref:fundamentals/logging/index).</span></span>

<span data-ttu-id="c64d2-203">Verwendung **Objekt-Explorer von SQL Server** , die Datenbank zu überprüfen.</span><span class="sxs-lookup"><span data-stu-id="c64d2-203">Use **SQL Server Object Explorer** to inspect the DB.</span></span> <span data-ttu-id="c64d2-204">Beachten Sie das Hinzufügen einer `__EFMigrationsHistory` Tabelle.</span><span class="sxs-lookup"><span data-stu-id="c64d2-204">Notice the addition of an `__EFMigrationsHistory` table.</span></span> <span data-ttu-id="c64d2-205">Die `__EFMigrationsHistory` Tabelle der nachverfolgt welche Migrationen auf die Datenbank angewendet wurden.</span><span class="sxs-lookup"><span data-stu-id="c64d2-205">The `__EFMigrationsHistory` table keeps track of which migrations have been applied to the DB.</span></span> <span data-ttu-id="c64d2-206">Zeigen Sie die Daten in der `__EFMigrationsHistory` Tabelle wird eine Zeile für die erste Migration gezeigt.</span><span class="sxs-lookup"><span data-stu-id="c64d2-206">View the data in the `__EFMigrationsHistory` table, it shows one row for the first migration.</span></span> <span data-ttu-id="c64d2-207">Das letzte Protokoll im vorherigen Beispiel der CLI-Ausgabe zeigt die INSERT-Anweisung, die diese Zeile erstellt.</span><span class="sxs-lookup"><span data-stu-id="c64d2-207">The last log in the preceding CLI output example shows the INSERT statement that creates this row.</span></span>

<span data-ttu-id="c64d2-208">Führen Sie die app, und stellen Sie sicher, dass alles funktioniert.</span><span class="sxs-lookup"><span data-stu-id="c64d2-208">Run the app and verify that everything works.</span></span>

## <a name="appling-migrations-in-production"></a><span data-ttu-id="c64d2-209">Appling Migrationen in der Produktion</span><span class="sxs-lookup"><span data-stu-id="c64d2-209">Appling migrations in production</span></span>

<span data-ttu-id="c64d2-210">Es wird empfohlen, sollte der Produktion apps **nicht** Aufrufen [Database.Migrate](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.relationaldatabasefacadeextensions.migrate?view=efcore-2.0#Microsoft_EntityFrameworkCore_RelationalDatabaseFacadeExtensions_Migrate_Microsoft_EntityFrameworkCore_Infrastructure_DatabaseFacade_) beim Anwendungsstart.</span><span class="sxs-lookup"><span data-stu-id="c64d2-210">We recommend production apps should **not** call [Database.Migrate](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.relationaldatabasefacadeextensions.migrate?view=efcore-2.0#Microsoft_EntityFrameworkCore_RelationalDatabaseFacadeExtensions_Migrate_Microsoft_EntityFrameworkCore_Infrastructure_DatabaseFacade_) at application startup.</span></span> <span data-ttu-id="c64d2-211">`Migrate`sollte nicht von einer app in der Serverfarm aufgerufen werden.</span><span class="sxs-lookup"><span data-stu-id="c64d2-211">`Migrate` should not be called from an app in server farm.</span></span> <span data-ttu-id="c64d2-212">Beispielsweise, wenn die app wurde Cloud mit horizontaler Skalierung (mehrere Instanzen der app ausgeführt werden) bereitgestellt.</span><span class="sxs-lookup"><span data-stu-id="c64d2-212">For example, if the app has been cloud deployed with scale-out (multiple instances of the app are running).</span></span>

<span data-ttu-id="c64d2-213">Datenbankmigration sollte im Rahmen der Bereitstellung, und klicken Sie auf kontrollierte Weise erfolgen.</span><span class="sxs-lookup"><span data-stu-id="c64d2-213">Database migration should be done as part of deployment, and in a controlled way.</span></span> <span data-ttu-id="c64d2-214">Produktion Datenbank Migrationsansätze gehören:</span><span class="sxs-lookup"><span data-stu-id="c64d2-214">Production database migration approaches include:</span></span>

* <span data-ttu-id="c64d2-215">Migrationen zum SQL-Skripts erstellen und verwenden die SQL-Skripts in der Bereitstellung.</span><span class="sxs-lookup"><span data-stu-id="c64d2-215">Using migrations to create SQL scripts and using the SQL scripts in deployment.</span></span>
* <span data-ttu-id="c64d2-216">Ausführen `dotnet ef database update` aus einer kontrollierten Umgebung.</span><span class="sxs-lookup"><span data-stu-id="c64d2-216">Running `dotnet ef database update` from a controlled environment.</span></span>

<span data-ttu-id="c64d2-217">EF Kern verwendet die `__MigrationsHistory` Tabelle, um festzustellen, ob alle Migrationen ausführen müssen.</span><span class="sxs-lookup"><span data-stu-id="c64d2-217">EF Core uses the `__MigrationsHistory` table to see if any migrations need to run.</span></span> <span data-ttu-id="c64d2-218">Wenn die Datenbank auf dem neuesten Stand ist, wird keine Migration ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="c64d2-218">If the DB is up to date, no migration is run.</span></span>

<a id="pmc"></a>
## <a name="command-line-interface-cli-vs-package-manager-console-pmc"></a><span data-ttu-id="c64d2-219">Im Vergleich zur Befehlszeilenschnittstelle (CLI) Paket-Manager-Konsole (PMC)</span><span class="sxs-lookup"><span data-stu-id="c64d2-219">Command-line interface (CLI) vs. Package Manager Console (PMC)</span></span>

<span data-ttu-id="c64d2-220">Die Tools zum Verwalten von Migrationen EF Core steht aus:</span><span class="sxs-lookup"><span data-stu-id="c64d2-220">The EF Core tooling for managing migrations is available from:</span></span>

* <span data-ttu-id="c64d2-221">.NET Core-CLI-Befehlen.</span><span class="sxs-lookup"><span data-stu-id="c64d2-221">.NET Core CLI commands.</span></span>
* <span data-ttu-id="c64d2-222">Die PowerShell-Cmdlets in der Visual Studio **Package Manager Console** (PMC) Fenster.</span><span class="sxs-lookup"><span data-stu-id="c64d2-222">The PowerShell cmdlets in the Visual Studio **Package Manager Console** (PMC) window.</span></span>

<span data-ttu-id="c64d2-223">Dieses Lernprogramm zeigt, wie die CLI, einige Entwickler bevorzugen, mit der Systemmonitor.</span><span class="sxs-lookup"><span data-stu-id="c64d2-223">This tutorial shows how to use the CLI, some developers prefer using the PMC.</span></span>

<span data-ttu-id="c64d2-224">Die EF Kernbefehle für die PMC befinden sich in der [Microsoft.EntityFrameworkCore.Tools](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools) Paket.</span><span class="sxs-lookup"><span data-stu-id="c64d2-224">The EF Core commands for the PMC are in the [Microsoft.EntityFrameworkCore.Tools](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools) package.</span></span> <span data-ttu-id="c64d2-225">Dieses Paket ist in enthalten die [Microsoft.AspNetCore.All](xref:fundamentals/metapackage) Metapackage, daher ist es nicht, es zu installieren.</span><span class="sxs-lookup"><span data-stu-id="c64d2-225">This package is included in the [Microsoft.AspNetCore.All](xref:fundamentals/metapackage) metapackage, so you don't have to install it.</span></span>

<span data-ttu-id="c64d2-226">**Wichtig:** Dies ist nicht das gleiche Paket mit dem Sie für die CLI, indem Sie die Bearbeitung Installieren der *csproj* Datei.</span><span class="sxs-lookup"><span data-stu-id="c64d2-226">**Important:** This is not the same package as the one you install for the CLI by editing the *.csproj* file.</span></span> <span data-ttu-id="c64d2-227">Der Name dieser endet `Tools`, im Gegensatz zu den CLI-Paketnamen die endet in `Tools.DotNet`.</span><span class="sxs-lookup"><span data-stu-id="c64d2-227">The name of this one ends in `Tools`, unlike the CLI package name which ends in `Tools.DotNet`.</span></span>

<span data-ttu-id="c64d2-228">Weitere Informationen zu CLI-Befehlen finden Sie unter [.NET Core CLI](https://docs.microsoft.com/ef/core/miscellaneous/cli/dotnet).</span><span class="sxs-lookup"><span data-stu-id="c64d2-228">For more information about the CLI commands, see [.NET Core CLI](https://docs.microsoft.com/ef/core/miscellaneous/cli/dotnet).</span></span>

<span data-ttu-id="c64d2-229">Weitere Informationen zu den PMC-Befehlen finden Sie unter [Package Manager Console (Visual Studio)](https://docs.microsoft.com/ef/core/miscellaneous/cli/powershell).</span><span class="sxs-lookup"><span data-stu-id="c64d2-229">For more information about the PMC commands, see [Package Manager Console (Visual Studio)](https://docs.microsoft.com/ef/core/miscellaneous/cli/powershell).</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="c64d2-230">Problembehandlung</span><span class="sxs-lookup"><span data-stu-id="c64d2-230">Troubleshooting</span></span>

<span data-ttu-id="c64d2-231">Herunterladen der [abgeschlossene Anwendung für diese Stufe](
https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part4-migrations).</span><span class="sxs-lookup"><span data-stu-id="c64d2-231">Download the [completed app for this stage](
https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part4-migrations).</span></span>

<span data-ttu-id="c64d2-232">Die app wird die folgende Ausnahme generiert:</span><span class="sxs-lookup"><span data-stu-id="c64d2-232">The app generates the following exception:</span></span>

```text
`SqlException: Cannot open database "ContosoUniversity" requested by the login.
The login failed.
Login failed for user 'user name'.
```

<span data-ttu-id="c64d2-233">Lösung: Ausführen`dotnet ef database update`</span><span class="sxs-lookup"><span data-stu-id="c64d2-233">Solution: Run `dotnet ef database update`</span></span>

<span data-ttu-id="c64d2-234">Wenn die `update` Befehl gibt dem Fehler "Fehler beim Buildvorgang.":</span><span class="sxs-lookup"><span data-stu-id="c64d2-234">If the `update` command returns the error "Build failed.":</span></span>

* <span data-ttu-id="c64d2-235">Führen Sie den Befehl erneut aus.</span><span class="sxs-lookup"><span data-stu-id="c64d2-235">Run the command again.</span></span>
* <span data-ttu-id="c64d2-236">Lassen Sie eine Nachricht am unteren Rand der Seite.</span><span class="sxs-lookup"><span data-stu-id="c64d2-236">Leave a message at the bottom of the page.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="c64d2-237">[Zurück](xref:data/ef-rp/sort-filter-page)
[Weiter](xref:data/ef-rp/complex-data-model)</span><span class="sxs-lookup"><span data-stu-id="c64d2-237">[Previous](xref:data/ef-rp/sort-filter-page)
[Next](xref:data/ef-rp/complex-data-model)</span></span>
