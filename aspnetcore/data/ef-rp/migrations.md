---
title: Razor-Seiten mit EF-Kern - Migrationen - 4 von 8
author: rick-anderson
description: "In diesem Lernprogramm beginnen Sie mit der EF-Core-Migrationen-Funktion für die Verwaltung von datenmodelländerungen in einer ASP.NET-MVC-Anwendung Core."
keywords: ASP.NET Core, Entity Framework Core, Migrationen
ms.author: riande
manager: wpickett
ms.date: 10/15/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: data/ef-rp/migrations
ms.openlocfilehash: 8549fc522bcd05a5755a449cd6d4b655f00d998b
ms.sourcegitcommit: 6e46abd65973dea796d364a514de9ec2e3e1c1ed
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 12/02/2017
---
# <a name="migrations---ef-core-with-razor-pages-tutorial-4-of-8"></a><span data-ttu-id="2fa6f-104">Migrationen - EF-Core mit Razor-Seiten Lernprogramm (4 von 8)</span><span class="sxs-lookup"><span data-stu-id="2fa6f-104">Migrations - EF Core with Razor Pages tutorial (4 of 8)</span></span>

<span data-ttu-id="2fa6f-105">Durch [Tom Dykstra](https://github.com/tdykstra), [Jon P Smith](https://twitter.com/thereformedprog), und [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="2fa6f-105">By [Tom Dykstra](https://github.com/tdykstra), [Jon P Smith](https://twitter.com/thereformedprog), and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

[!INCLUDE[about the series](../../includes/RP-EF/intro.md)]

<span data-ttu-id="2fa6f-106">In diesem Lernprogramm wird das EF Hauptfeature-Migrationen für die Verwaltung von datenmodelländerungen verwendet.</span><span class="sxs-lookup"><span data-stu-id="2fa6f-106">In this tutorial, the EF Core migrations feature for managing data model changes is used.</span></span>

<span data-ttu-id="2fa6f-107">Wenn Probleme können nicht zu lösen auftreten, laden Sie die [abgeschlossene Anwendung für diese Stufe](
https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part4-migrations).</span><span class="sxs-lookup"><span data-stu-id="2fa6f-107">If you run into problems you can't solve, download the [completed app for this stage](
https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part4-migrations).</span></span>

<span data-ttu-id="2fa6f-108">Wenn eine neue app entwickelt wird, werden Änderungen in die Daten häufig modellieren.</span><span class="sxs-lookup"><span data-stu-id="2fa6f-108">When a new app is developed, the data model changes frequently.</span></span> <span data-ttu-id="2fa6f-109">Jedes Mal Ruft das Modell ändert, das Modell nicht synchron mit der Datenbank.</span><span class="sxs-lookup"><span data-stu-id="2fa6f-109">Each time the model changes, the model gets out of sync with the database.</span></span> <span data-ttu-id="2fa6f-110">Dieses Lernprogramm gestartet, indem Sie die Konfiguration des Entity Framework zum Erstellen der Datenbank, wenn er nicht vorhanden ist.</span><span class="sxs-lookup"><span data-stu-id="2fa6f-110">This tutorial started by configuring the Entity Framework to create the database if it doesn't exist.</span></span> <span data-ttu-id="2fa6f-111">Jedes Mal, die das Datenmodell ändert:</span><span class="sxs-lookup"><span data-stu-id="2fa6f-111">Each time the data model changes:</span></span>

* <span data-ttu-id="2fa6f-112">Die Datenbank wird gelöscht.</span><span class="sxs-lookup"><span data-stu-id="2fa6f-112">The DB is dropped.</span></span>
* <span data-ttu-id="2fa6f-113">EF erstellt ein neues Konto, das das Modell entspricht.</span><span class="sxs-lookup"><span data-stu-id="2fa6f-113">EF creates a new one that matches the model.</span></span>
* <span data-ttu-id="2fa6f-114">Die app startet die Datenbank mit der Testdaten.</span><span class="sxs-lookup"><span data-stu-id="2fa6f-114">The app seeds the DB with test data.</span></span>

<span data-ttu-id="2fa6f-115">Dieser Ansatz zum Synchronisieren der Datenbank mit dem Datenmodell funktioniert gut, bis Sie die app in die Produktion bereitstellen.</span><span class="sxs-lookup"><span data-stu-id="2fa6f-115">This approach to keeping the DB in sync with the data model works well until you deploy the app to production.</span></span> <span data-ttu-id="2fa6f-116">Wenn die app in der produktionsumgebung ausgeführt wird, wird es in der Regel Daten speichern, die beibehalten werden muss.</span><span class="sxs-lookup"><span data-stu-id="2fa6f-116">When the app is running in production, it is usually storing data that needs to be maintained.</span></span> <span data-ttu-id="2fa6f-117">Die app kann nicht in einem DB-Test starten jedes Mal eine Änderung vorgenommen wird, (z. B. das Hinzufügen einer neuen Spalte).</span><span class="sxs-lookup"><span data-stu-id="2fa6f-117">The app can't start with a test DB each time a change is made (such as adding a new column).</span></span> <span data-ttu-id="2fa6f-118">Das Feature EF Core Migrationen löst dieses Problem durch Aktivieren von EF Core zum Aktualisieren des Schemas DB, anstatt eine neue Datenbank erstellen.</span><span class="sxs-lookup"><span data-stu-id="2fa6f-118">The EF Core Migrations feature solves this problem by enabling EF Core to update the DB schema instead of creating a new DB.</span></span>

<span data-ttu-id="2fa6f-119">Anstatt löschen und Neuerstellen der Datenbank, wenn das Datenmodell ändert, werden Migrationen aktualisiert das Schema und behält die vorhandene Daten.</span><span class="sxs-lookup"><span data-stu-id="2fa6f-119">Rather than dropping and recreating the DB when the data model changes, migrations updates the schema and retains existing data.</span></span>

## <a name="entity-framework-core-nuget-packages-for-migrations"></a><span data-ttu-id="2fa6f-120">NuGet-Pakete für Migrationen in Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="2fa6f-120">Entity Framework Core NuGet packages for migrations</span></span>

<span data-ttu-id="2fa6f-121">Verwenden Sie zum Arbeiten mit Migrationen der **Package Manager Console** (PMC) oder die Befehlszeilenschnittstelle (CLI).</span><span class="sxs-lookup"><span data-stu-id="2fa6f-121">To work with migrations, use the **Package Manager Console** (PMC) or the command-line interface (CLI).</span></span> <span data-ttu-id="2fa6f-122">Diese Lernprogramme veranschaulichen, wie CLI-Befehlen.</span><span class="sxs-lookup"><span data-stu-id="2fa6f-122">These tutorials show how to use CLI commands.</span></span> <span data-ttu-id="2fa6f-123">Informationen zu der Systemmonitor ist am [Ende dieses Lernprogramms](#pmc).</span><span class="sxs-lookup"><span data-stu-id="2fa6f-123">Information about the PMC is at [the end of this tutorial](#pmc).</span></span>

<span data-ttu-id="2fa6f-124">Der EF-Core-Tools für die Befehlszeilenschnittstelle (CLI) finden Sie unter [Microsoft.EntityFrameworkCore.Tools.DotNet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools.DotNet).</span><span class="sxs-lookup"><span data-stu-id="2fa6f-124">The EF Core tools for the command-line interface (CLI) are provided in [Microsoft.EntityFrameworkCore.Tools.DotNet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools.DotNet).</span></span> <span data-ttu-id="2fa6f-125">Um dieses Paket zu installieren, fügen sie der `DotNetCliToolReference` Sammlung in der *csproj* Datei wie gezeigt.</span><span class="sxs-lookup"><span data-stu-id="2fa6f-125">To install this package, add it to the `DotNetCliToolReference` collection in the *.csproj* file, as shown.</span></span> <span data-ttu-id="2fa6f-126">**Hinweis:** dieses Paket muss installiert werden, indem Sie bearbeiten die *csproj* Datei.</span><span class="sxs-lookup"><span data-stu-id="2fa6f-126">**Note:** This package must be installed by editing the *.csproj* file.</span></span> <span data-ttu-id="2fa6f-127">Die`install-package` Befehl oder die Paket-Manager-GUI nicht zum Installieren dieses Pakets verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="2fa6f-127">The`install-package` command or the package manager GUI cannot be used to install this package.</span></span> <span data-ttu-id="2fa6f-128">Bearbeiten der *csproj* Datei mit der rechten Maustaste auf den Projektnamen im **Projektmappen-Explorer** auswählen und **ContosoUniversity.csproj bearbeiten**.</span><span class="sxs-lookup"><span data-stu-id="2fa6f-128">Edit the *.csproj* file by right-clicking the project name in **Solution Explorer** and selecting **Edit ContosoUniversity.csproj**.</span></span>

<span data-ttu-id="2fa6f-129">Das folgende Markup zeigt die aktualisierte *csproj* Datei mit hervorgehobenen EF Core CLI-Tools:</span><span class="sxs-lookup"><span data-stu-id="2fa6f-129">The following markup shows the updated *.csproj* file with the EF Core CLI tools highlighted:</span></span>

[!code-xml[](intro/samples/cu/ContosoUniversity.csproj?highlight=12)]
  
<span data-ttu-id="2fa6f-130">Die Versionsnummern im vorherigen Beispiel wurden beim Erstellen des Lernprogramms geschrieben wurde.</span><span class="sxs-lookup"><span data-stu-id="2fa6f-130">The version numbers in the preceding example were current when the tutorial was written.</span></span> <span data-ttu-id="2fa6f-131">Verwenden Sie die gleiche Version, für die EF Core CLI-Tools, die in die anderen Pakete gefunden.</span><span class="sxs-lookup"><span data-stu-id="2fa6f-131">Use the same version for the EF Core CLI tools found in the other packages.</span></span>

## <a name="change-the-connection-string"></a><span data-ttu-id="2fa6f-132">Ändern der Verbindungszeichenfolge</span><span class="sxs-lookup"><span data-stu-id="2fa6f-132">Change the connection string</span></span>

<span data-ttu-id="2fa6f-133">In der *appsettings.json* Datei, ändern Sie den Namen der Datenbank in der Verbindungszeichenfolge auf ContosoUniversity2.</span><span class="sxs-lookup"><span data-stu-id="2fa6f-133">In the *appsettings.json* file, change the name of the DB in the connection string to ContosoUniversity2.</span></span>

[!code-json[Main](intro/samples/cu/appsettings2.json?range=1-4)]

<span data-ttu-id="2fa6f-134">Ändern der DB-Name in der Verbindungszeichenfolge bewirkt, dass die erste Migration zu eine neue Datenbank zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="2fa6f-134">Changing the DB name in the connection string causes the first migration to create a new DB.</span></span> <span data-ttu-id="2fa6f-135">Eine neue Datenbank wird erstellt, da eine mit diesem Namen nicht vorhanden ist.</span><span class="sxs-lookup"><span data-stu-id="2fa6f-135">A new DB is created because one with that name does not exist.</span></span> <span data-ttu-id="2fa6f-136">Die Verbindungszeichenfolge ändern, ist nicht erforderlich für erste Schritte mit Migrationen.</span><span class="sxs-lookup"><span data-stu-id="2fa6f-136">Changing the connection string isn't required for getting started with migrations.</span></span>

<span data-ttu-id="2fa6f-137">Eine Alternative zum Ändern der DB-Name ist die Datenbank gelöscht.</span><span class="sxs-lookup"><span data-stu-id="2fa6f-137">An alternative to changing the DB name is deleting the DB.</span></span> <span data-ttu-id="2fa6f-138">Verwendung **Objekt-Explorer von SQL Server** (SSOX) oder die `database drop` CLI-Befehl:</span><span class="sxs-lookup"><span data-stu-id="2fa6f-138">Use **SQL Server Object Explorer** (SSOX) or the `database drop` CLI command:</span></span>

 ```console
 dotnet ef database drop
 ```

<span data-ttu-id="2fa6f-139">Im folgende Abschnitt wird erläutert, wie CLI-Befehle ausgeführt werden.</span><span class="sxs-lookup"><span data-stu-id="2fa6f-139">The following section explains how to run CLI commands.</span></span>

## <a name="create-an-initial-migration"></a><span data-ttu-id="2fa6f-140">Erstellen einer anfänglichen Migrations</span><span class="sxs-lookup"><span data-stu-id="2fa6f-140">Create an initial migration</span></span>

<span data-ttu-id="2fa6f-141">Erstellen Sie das Projekt.</span><span class="sxs-lookup"><span data-stu-id="2fa6f-141">Build the project.</span></span>

<span data-ttu-id="2fa6f-142">Öffnen Sie ein Befehlsfenster, und navigieren Sie zum Projektordner.</span><span class="sxs-lookup"><span data-stu-id="2fa6f-142">Open a command window and navigate to the project folder.</span></span> <span data-ttu-id="2fa6f-143">Enthält der Projektordner der *Startup.cs* Datei.</span><span class="sxs-lookup"><span data-stu-id="2fa6f-143">The project folder contains the *Startup.cs* file.</span></span>

<span data-ttu-id="2fa6f-144">Geben Sie im Fenster Eingabeaufforderung Folgendes ein:</span><span class="sxs-lookup"><span data-stu-id="2fa6f-144">Enter the following in the command window:</span></span>

```console
dotnet ef migrations add InitialCreate
```

<span data-ttu-id="2fa6f-145">Das Befehlsfenster werden Informationen, die etwa wie folgt angezeigt:</span><span class="sxs-lookup"><span data-stu-id="2fa6f-145">The command window displays information similar to the following:</span></span>

```console
info: Microsoft.AspNetCore.DataProtection.KeyManagement.XmlKeyManager[0]
      User profile is available. Using 'C:\Users\username\AppData\Local\ASP.NET\DataProtection-Keys' as key repository and Windows DPAPI to encrypt keys at rest.
info: Microsoft.EntityFrameworkCore.Infrastructure[100403]
      Entity Framework Core 2.0.0-rtm-26452 initialized 'SchoolContext' using provider 'Microsoft.EntityFrameworkCore.SqlServer' with options: None
Done. To undo this action, use 'ef migrations remove'
```

<span data-ttu-id="2fa6f-146">Bei einem mit der Meldung Migrationsfehler "*... die Datei kann nicht zugegriffen werden. ContosoUniversity.dll, da sie von einem anderen Prozess verwendet wird.* "</span><span class="sxs-lookup"><span data-stu-id="2fa6f-146">If the migration fails with the message "*cannot access the file ... ContosoUniversity.dll because it is being used by another process.*"</span></span> <span data-ttu-id="2fa6f-147">wird angezeigt:</span><span class="sxs-lookup"><span data-stu-id="2fa6f-147">is displayed:</span></span>

* <span data-ttu-id="2fa6f-148">Beenden von IIS Express.</span><span class="sxs-lookup"><span data-stu-id="2fa6f-148">Stop IIS Express.</span></span>

   * <span data-ttu-id="2fa6f-149">Beenden und starten Sie Visual Studio oder</span><span class="sxs-lookup"><span data-stu-id="2fa6f-149">Exit and restart Visual Studio, or</span></span>
   * <span data-ttu-id="2fa6f-150">Suchen Sie das Symbol "IIS Express" in der Windows-Taskleiste.</span><span class="sxs-lookup"><span data-stu-id="2fa6f-150">Find the IIS Express icon in the Windows System Tray.</span></span>
   * <span data-ttu-id="2fa6f-151">Maustaste auf das Symbol "IIS Express", und klicken Sie dann auf **ContosoUniversity > Standort beenden**.</span><span class="sxs-lookup"><span data-stu-id="2fa6f-151">Right-click the IIS Express icon, and then click **ContosoUniversity > Stop Site**.</span></span>

<span data-ttu-id="2fa6f-152">Wenn die Fehlermeldung "Fehler beim Erstellen des."</span><span class="sxs-lookup"><span data-stu-id="2fa6f-152">If the error message "Build failed."</span></span> <span data-ttu-id="2fa6f-153">wird angezeigt, führen den Befehl erneut aus.</span><span class="sxs-lookup"><span data-stu-id="2fa6f-153">is displayed, run the command again.</span></span> <span data-ttu-id="2fa6f-154">Wenn Sie diesen Fehler erhalten, lassen Sie sich am Ende dieses Lernprogramms.</span><span class="sxs-lookup"><span data-stu-id="2fa6f-154">If you get this error, leave a note at the bottom of this tutorial.</span></span>

### <a name="examine-the-up-and-down-methods"></a><span data-ttu-id="2fa6f-155">Überprüfen Sie die nach-oben und nach-unten Sie-Methoden</span><span class="sxs-lookup"><span data-stu-id="2fa6f-155">Examine the Up and Down methods</span></span>

<span data-ttu-id="2fa6f-156">Der EF-Core-Befehl `migrations add` generiert Code zum Erstellen der Datenbank aus.</span><span class="sxs-lookup"><span data-stu-id="2fa6f-156">The EF Core command `migrations add` generated code to create the DB from.</span></span> <span data-ttu-id="2fa6f-157">Dieser Code Migrationen befindet sich in der *Migrationen\<Zeitstempel > _InitialCreate.cs* Datei.</span><span class="sxs-lookup"><span data-stu-id="2fa6f-157">This migrations code is in the *Migrations\<timestamp>_InitialCreate.cs* file.</span></span> <span data-ttu-id="2fa6f-158">Die `Up` Methode der `InitialCreate` Klasse erstellt, die DB-Tabellen, die die Daten Modell Entitätenmengen entsprechen.</span><span class="sxs-lookup"><span data-stu-id="2fa6f-158">The `Up` method of the `InitialCreate` class creates the DB tables that correspond to the data model entity sets.</span></span> <span data-ttu-id="2fa6f-159">Die `Down` Methode löscht, wie im folgenden Beispiel gezeigt:</span><span class="sxs-lookup"><span data-stu-id="2fa6f-159">The `Down` method deletes them, as shown in the following example:</span></span>

[!code-csharp[Main](intro/samples/cu/Migrations/20171026010210_InitialCreate.cs?range=8-24,77-)]

<span data-ttu-id="2fa6f-160">Migrationen Aufrufe der `Up` Methode für die Implementierung des Datenmodells für eine Migration.</span><span class="sxs-lookup"><span data-stu-id="2fa6f-160">Migrations calls the `Up` method to implement the data model changes for a migration.</span></span> <span data-ttu-id="2fa6f-161">Bei der Eingabe eines Befehls ein Rollback der Updates, die Migrationen Aufrufe der `Down` Methode.</span><span class="sxs-lookup"><span data-stu-id="2fa6f-161">When you enter a command to roll back the update, migrations calls the `Down` method.</span></span>

<span data-ttu-id="2fa6f-162">Der vorhergehende Code ist der anfänglichen Migration.</span><span class="sxs-lookup"><span data-stu-id="2fa6f-162">The preceding code is for the initial migration.</span></span> <span data-ttu-id="2fa6f-163">Dieser Code wurde erstellt, wenn die `migrations add InitialCreate` -Befehl ausgeführt wurde.</span><span class="sxs-lookup"><span data-stu-id="2fa6f-163">That code was created when the `migrations add InitialCreate` command was run.</span></span> <span data-ttu-id="2fa6f-164">Der Namensparameter der Migration (im Beispiel "InitialCreate") wird für den Dateinamen verwendet.</span><span class="sxs-lookup"><span data-stu-id="2fa6f-164">The migration name parameter ("InitialCreate" in the example) is used for the file name.</span></span> <span data-ttu-id="2fa6f-165">Der migrationsname kann einen beliebigen gültigen Dateinamen.</span><span class="sxs-lookup"><span data-stu-id="2fa6f-165">The migration name can be any valid file name.</span></span> <span data-ttu-id="2fa6f-166">Es wird empfohlen, wählen ein Wort oder Ausdruck, die zusammengefasst, was bei der Migration durchgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="2fa6f-166">It's best to choose a word or phrase that summarizes what is being done in the migration.</span></span> <span data-ttu-id="2fa6f-167">Beispielsweise könnte eine Migration, die Department-Tabelle hinzugefügt "AddDepartmentTable." aufgerufen werden</span><span class="sxs-lookup"><span data-stu-id="2fa6f-167">For example, a migration that added a department table might be called "AddDepartmentTable."</span></span>

<span data-ttu-id="2fa6f-168">Wenn die anfängliche Migration erstellt wird und die Datenbank beendet wird:</span><span class="sxs-lookup"><span data-stu-id="2fa6f-168">If the initial migration is created and the DB exits:</span></span>

* <span data-ttu-id="2fa6f-169">Der DB-Erstellung-Code wird generiert.</span><span class="sxs-lookup"><span data-stu-id="2fa6f-169">The DB creation code is generated.</span></span>
* <span data-ttu-id="2fa6f-170">Der DB-Erstellung Code muss nicht ausgeführt werden, da die Datenbank bereits das Datenmodell übereinstimmt.</span><span class="sxs-lookup"><span data-stu-id="2fa6f-170">The DB creation code doesn't need to run because the DB already matches the data model.</span></span> <span data-ttu-id="2fa6f-171">Wenn der DB-Erstellung Code ausgeführt wird, nicht der Änderungen vorgenommen werden, da die Datenbank bereits das Datenmodell übereinstimmt.</span><span class="sxs-lookup"><span data-stu-id="2fa6f-171">If the The DB creation code is run, it doesn't make any changes because the DB already matches the data model.</span></span>

<span data-ttu-id="2fa6f-172">Wenn die app zu einer neuen Umgebung bereitgestellt wird, muss die DB-Erstellung Code ausgeführt werden, um die Datenbank zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="2fa6f-172">When the app is deployed to a new environment, the DB creation code must be run to create the DB.</span></span>

<span data-ttu-id="2fa6f-173">Zuvor wurde die Verbindungszeichenfolge geändert, um einen neuen Namen für die Datenbank zu verwenden.</span><span class="sxs-lookup"><span data-stu-id="2fa6f-173">Previously the connection string was changed to use a new name for the DB.</span></span> <span data-ttu-id="2fa6f-174">Die angegebene Datenbank ist nicht vorhanden, damit Migrationen erstellt die Datenbank.</span><span class="sxs-lookup"><span data-stu-id="2fa6f-174">The specified DB doesn't exist, so migrations creates the DB.</span></span>

### <a name="examine-the-data-model-snapshot"></a><span data-ttu-id="2fa6f-175">Überprüfen Sie die datenmomentaufnahme für das Modell</span><span class="sxs-lookup"><span data-stu-id="2fa6f-175">Examine the data model snapshot</span></span>

<span data-ttu-id="2fa6f-176">Migrationen erstellt eine *Momentaufnahme* des aktuellen DB-Schemas in *Migrations/SchoolContextModelSnapshot.cs*:</span><span class="sxs-lookup"><span data-stu-id="2fa6f-176">Migrations creates a *snapshot* of the current DB schema in *Migrations/SchoolContextModelSnapshot.cs*:</span></span>

[!code-csharp[Main](intro/samples/cu/Migrations/SchoolContextModelSnapshot1.cs?name=snippet_Truncate)]

<span data-ttu-id="2fa6f-177">Da das Schema der aktuellen im Code dargestellt wird, verwendet nicht EF Kern, für die Interaktion mit der Datenbank, um Migrationen zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="2fa6f-177">Because the current DB schema is represented in code, EF Core doesn't have to interact with the DB to create migrations.</span></span> <span data-ttu-id="2fa6f-178">Wenn Sie eine Migration hinzufügen, bestimmt EF Core Änderungen durch das Datenmodell auf der Snapshot-Datei vergleichen.</span><span class="sxs-lookup"><span data-stu-id="2fa6f-178">When you add a migration, EF Core determines what changed by comparing the data model to the snapshot file.</span></span> <span data-ttu-id="2fa6f-179">EF Core interagiert mit der Datenbank, nur bei die Datenbank zu aktualisieren.</span><span class="sxs-lookup"><span data-stu-id="2fa6f-179">EF Core interacts with the DB only when it has to update the DB.</span></span>

<span data-ttu-id="2fa6f-180">Der Datenbankmomentaufnahme-Datei muss mit die Migrationen synchron sein, der Sie erstellt.</span><span class="sxs-lookup"><span data-stu-id="2fa6f-180">The snapshot file must be in sync with the migrations that created it.</span></span> <span data-ttu-id="2fa6f-181">Eine Migration kann nicht entfernt werden, durch Löschen der Datei mit dem Namen  *\<Zeitstempel > _\<Migrationname > .cs*.</span><span class="sxs-lookup"><span data-stu-id="2fa6f-181">A migration can't be removed by deleting the file named *\<timestamp>_\<migrationname>.cs*.</span></span> <span data-ttu-id="2fa6f-182">Wenn diese Datei gelöscht wird, werden die übrigen Migrationen werden nicht synchron mit der Snapshot-Datenbankdatei.</span><span class="sxs-lookup"><span data-stu-id="2fa6f-182">If that file is deleted, the remaining migrations are out of sync with the DB snapshot file.</span></span> <span data-ttu-id="2fa6f-183">Verwenden Sie zum Löschen der letzten Migrations hinzugefügt der [Dotnet Ef Migrationen entfernen](https://docs.microsoft.com/ef/core/miscellaneous/cli/dotnet#dotnet-ef-migrations-remove) Befehl.</span><span class="sxs-lookup"><span data-stu-id="2fa6f-183">To delete the last migration added, use the [dotnet ef migrations remove](https://docs.microsoft.com/ef/core/miscellaneous/cli/dotnet#dotnet-ef-migrations-remove) command.</span></span>

## <a name="remove-ensurecreated"></a><span data-ttu-id="2fa6f-184">EnsureCreated entfernen</span><span class="sxs-lookup"><span data-stu-id="2fa6f-184">Remove EnsureCreated</span></span>

<span data-ttu-id="2fa6f-185">Bei frühen-Bereitstellung müssen die `EnsureCreated` -Befehl wurde verwendet.</span><span class="sxs-lookup"><span data-stu-id="2fa6f-185">For early development, the `EnsureCreated` command was used.</span></span> <span data-ttu-id="2fa6f-186">In diesem Lernprogramm wird die Migrationen verwendet.</span><span class="sxs-lookup"><span data-stu-id="2fa6f-186">In this tutorial, migrations is used.</span></span> <span data-ttu-id="2fa6f-187">`EnsureCreated`verfügt über die folgenden Limatitions aus:</span><span class="sxs-lookup"><span data-stu-id="2fa6f-187">`EnsureCreated` has the following limatitions:</span></span>

* <span data-ttu-id="2fa6f-188">Migrationen umgangen wird und die DB- und das Schema erstellt.</span><span class="sxs-lookup"><span data-stu-id="2fa6f-188">Bypasses migrations and creates the DB and schema.</span></span>
* <span data-ttu-id="2fa6f-189">Eine Tabelle Migrationen wird nicht erstellt werden.</span><span class="sxs-lookup"><span data-stu-id="2fa6f-189">Does not create a migrations table.</span></span>
* <span data-ttu-id="2fa6f-190">Können *nicht* mit Migrationen verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="2fa6f-190">Can *not* be used with migrations.</span></span>
* <span data-ttu-id="2fa6f-191">Dient zum Testen oder schnellen Prototyping, in dem die Datenbank gelöscht und neu erstellt häufig.</span><span class="sxs-lookup"><span data-stu-id="2fa6f-191">Is designed for testing or rapid prototyping where the DB is dropped and re-created frequently.</span></span>

<span data-ttu-id="2fa6f-192">Entfernen Sie die folgende Zeile aus `DbInitializer`:</span><span class="sxs-lookup"><span data-stu-id="2fa6f-192">Remove the following line from `DbInitializer`:</span></span>

```csharp
context.Database.EnsureCreated();
```

## <a name="apply-the-migration-to-the-db-in-development"></a><span data-ttu-id="2fa6f-193">Übernehmen Sie die Migration mit der Datenbank in der Entwicklung</span><span class="sxs-lookup"><span data-stu-id="2fa6f-193">Apply the migration to the DB in development</span></span>

<span data-ttu-id="2fa6f-194">Geben Sie Folgendes ein, um die DB- und Tabellen zu erstellen, in das Befehlsfenster.</span><span class="sxs-lookup"><span data-stu-id="2fa6f-194">In the command window, enter the following to create the DB and tables.</span></span>

```console
dotnet ef database update
```

<span data-ttu-id="2fa6f-195">Hinweis: Wenn die `update` Befehl gibt dem Fehler "Fehler beim Buildvorgang.":</span><span class="sxs-lookup"><span data-stu-id="2fa6f-195">Note: If the `update` command returns the error "Build failed.":</span></span>

* <span data-ttu-id="2fa6f-196">Führen Sie den Befehl erneut aus.</span><span class="sxs-lookup"><span data-stu-id="2fa6f-196">Run the command again.</span></span>
* <span data-ttu-id="2fa6f-197">Wenn es wieder fehlschlägt, beenden Sie Visual Studio, und führen Sie dann die `update` Befehl.</span><span class="sxs-lookup"><span data-stu-id="2fa6f-197">If it fails again, exit Visual Studio and then run the `update` command.</span></span>
* <span data-ttu-id="2fa6f-198">Lassen Sie eine Nachricht am unteren Rand der Seite.</span><span class="sxs-lookup"><span data-stu-id="2fa6f-198">Leave a message at the bottom of the page.</span></span>

<span data-ttu-id="2fa6f-199">Die Ausgabe des Befehls ähnelt der `migrations add` Befehl Ausgabe.</span><span class="sxs-lookup"><span data-stu-id="2fa6f-199">The output from the command is similar to the `migrations add` command output.</span></span> <span data-ttu-id="2fa6f-200">In den vorherigen Befehl werden die Protokolle für die SQL-Befehle, mit dem die Datenbank eingerichtet angezeigt.</span><span class="sxs-lookup"><span data-stu-id="2fa6f-200">In the preceding command, logs for the SQL commands that set up the DB are displayed.</span></span> <span data-ttu-id="2fa6f-201">Die meisten Protokolle werden in der folgenden Beispielausgabe ausgelassen:</span><span class="sxs-lookup"><span data-stu-id="2fa6f-201">Most of the logs are omitted in the following sample output:</span></span>

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

<span data-ttu-id="2fa6f-202">Zum Reduzieren der Detailebene in Protokollnachrichten enthalten, kann die Protokollierungsstufen im Ändern der *"appSettings". Development.JSON* Datei.</span><span class="sxs-lookup"><span data-stu-id="2fa6f-202">To reduce the level of detail in log messages, can change the log levels in the *appsettings.Development.json* file.</span></span> <span data-ttu-id="2fa6f-203">Weitere Informationen finden Sie unter [Einführung in die Protokollierung](xref:fundamentals/logging/index).</span><span class="sxs-lookup"><span data-stu-id="2fa6f-203">For more information, see [Introduction to logging](xref:fundamentals/logging/index).</span></span>

<span data-ttu-id="2fa6f-204">Verwendung **Objekt-Explorer von SQL Server** , die Datenbank zu überprüfen.</span><span class="sxs-lookup"><span data-stu-id="2fa6f-204">Use **SQL Server Object Explorer** to inspect the DB.</span></span> <span data-ttu-id="2fa6f-205">Beachten Sie das Hinzufügen einer `__EFMigrationsHistory` Tabelle.</span><span class="sxs-lookup"><span data-stu-id="2fa6f-205">Notice the addition of an `__EFMigrationsHistory` table.</span></span> <span data-ttu-id="2fa6f-206">Die `__EFMigrationsHistory` Tabelle der nachverfolgt welche Migrationen auf die Datenbank angewendet wurden.</span><span class="sxs-lookup"><span data-stu-id="2fa6f-206">The `__EFMigrationsHistory` table keeps track of which migrations have been applied to the DB.</span></span> <span data-ttu-id="2fa6f-207">Zeigen Sie die Daten in der `__EFMigrationsHistory` Tabelle wird eine Zeile für die erste Migration gezeigt.</span><span class="sxs-lookup"><span data-stu-id="2fa6f-207">View the data in the `__EFMigrationsHistory` table, it shows one row for the first migration.</span></span> <span data-ttu-id="2fa6f-208">Das letzte Protokoll im vorherigen Beispiel der CLI-Ausgabe zeigt die INSERT-Anweisung, die diese Zeile erstellt.</span><span class="sxs-lookup"><span data-stu-id="2fa6f-208">The last log in the preceding CLI output example shows the INSERT statement that creates this row.</span></span>

<span data-ttu-id="2fa6f-209">Führen Sie die app, und stellen Sie sicher, dass alles funktioniert.</span><span class="sxs-lookup"><span data-stu-id="2fa6f-209">Run the app and verify that everything works.</span></span>

## <a name="appling-migrations-in-production"></a><span data-ttu-id="2fa6f-210">Appling Migrationen in der Produktion</span><span class="sxs-lookup"><span data-stu-id="2fa6f-210">Appling migrations in production</span></span>

<span data-ttu-id="2fa6f-211">Es wird empfohlen, sollte der Produktion apps **nicht** Aufrufen [Database.Migrate](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.relationaldatabasefacadeextensions.migrate?view=efcore-2.0#Microsoft_EntityFrameworkCore_RelationalDatabaseFacadeExtensions_Migrate_Microsoft_EntityFrameworkCore_Infrastructure_DatabaseFacade_) beim Anwendungsstart.</span><span class="sxs-lookup"><span data-stu-id="2fa6f-211">We recommend production apps should **not** call [Database.Migrate](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.relationaldatabasefacadeextensions.migrate?view=efcore-2.0#Microsoft_EntityFrameworkCore_RelationalDatabaseFacadeExtensions_Migrate_Microsoft_EntityFrameworkCore_Infrastructure_DatabaseFacade_) at application startup.</span></span> <span data-ttu-id="2fa6f-212">`Migrate`sollte nicht von einer app in der Serverfarm aufgerufen werden.</span><span class="sxs-lookup"><span data-stu-id="2fa6f-212">`Migrate` should not be called from an app in server farm.</span></span> <span data-ttu-id="2fa6f-213">Beispielsweise, wenn die app wurde Cloud mit horizontaler Skalierung (mehrere Instanzen der app ausgeführt werden) bereitgestellt.</span><span class="sxs-lookup"><span data-stu-id="2fa6f-213">For example, if the app has been cloud deployed with scale-out (multiple instances of the app are running).</span></span>

<span data-ttu-id="2fa6f-214">Datenbankmigration sollte im Rahmen der Bereitstellung, und klicken Sie auf kontrollierte Weise erfolgen.</span><span class="sxs-lookup"><span data-stu-id="2fa6f-214">Database migration should be done as part of deployment, and in a controlled way.</span></span> <span data-ttu-id="2fa6f-215">Produktion Datenbank Migrationsansätze gehören:</span><span class="sxs-lookup"><span data-stu-id="2fa6f-215">Production database migration approaches include:</span></span>

* <span data-ttu-id="2fa6f-216">Migrationen zum SQL-Skripts erstellen und verwenden die SQL-Skripts in der Bereitstellung.</span><span class="sxs-lookup"><span data-stu-id="2fa6f-216">Using migrations to create SQL scripts and using the SQL scripts in deployment.</span></span>
* <span data-ttu-id="2fa6f-217">Ausführen `dotnet ef database update` aus einer kontrollierten Umgebung.</span><span class="sxs-lookup"><span data-stu-id="2fa6f-217">Running `dotnet ef database update` from a controlled environment.</span></span>

<span data-ttu-id="2fa6f-218">EF Kern verwendet die `__MigrationsHistory` Tabelle, um festzustellen, ob alle Migrationen ausführen müssen.</span><span class="sxs-lookup"><span data-stu-id="2fa6f-218">EF Core uses the `__MigrationsHistory` table to see if any migrations need to run.</span></span> <span data-ttu-id="2fa6f-219">Wenn die Datenbank auf dem neuesten Stand ist, wird keine Migration ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="2fa6f-219">If the DB is up to date, no migration is run.</span></span>

<a id="pmc"></a>
## <a name="command-line-interface-cli-vs-package-manager-console-pmc"></a><span data-ttu-id="2fa6f-220">Im Vergleich zur Befehlszeilenschnittstelle (CLI) Paket-Manager-Konsole (PMC)</span><span class="sxs-lookup"><span data-stu-id="2fa6f-220">Command-line interface (CLI) vs. Package Manager Console (PMC)</span></span>

<span data-ttu-id="2fa6f-221">Die Tools zum Verwalten von Migrationen EF Core steht aus:</span><span class="sxs-lookup"><span data-stu-id="2fa6f-221">The EF Core tooling for managing migrations is available from:</span></span>

* <span data-ttu-id="2fa6f-222">.NET Core-CLI-Befehlen.</span><span class="sxs-lookup"><span data-stu-id="2fa6f-222">.NET Core CLI commands.</span></span>
* <span data-ttu-id="2fa6f-223">Die PowerShell-Cmdlets in der Visual Studio **Package Manager Console** (PMC) Fenster.</span><span class="sxs-lookup"><span data-stu-id="2fa6f-223">The PowerShell cmdlets in the Visual Studio **Package Manager Console** (PMC) window.</span></span>

<span data-ttu-id="2fa6f-224">Dieses Lernprogramm zeigt, wie die CLI, einige Entwickler bevorzugen, mit der Systemmonitor.</span><span class="sxs-lookup"><span data-stu-id="2fa6f-224">This tutorial shows how to use the CLI, some developers prefer using the PMC.</span></span>

<span data-ttu-id="2fa6f-225">Die EF Kernbefehle für die PMC befinden sich in der [Microsoft.EntityFrameworkCore.Tools](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools) Paket.</span><span class="sxs-lookup"><span data-stu-id="2fa6f-225">The EF Core commands for the PMC are in the [Microsoft.EntityFrameworkCore.Tools](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools) package.</span></span> <span data-ttu-id="2fa6f-226">Dieses Paket ist in enthalten die [Microsoft.AspNetCore.All](xref:fundamentals/metapackage) Metapackage, daher ist es nicht, es zu installieren.</span><span class="sxs-lookup"><span data-stu-id="2fa6f-226">This package is included in the [Microsoft.AspNetCore.All](xref:fundamentals/metapackage) metapackage, so you don't have to install it.</span></span>

<span data-ttu-id="2fa6f-227">**Wichtig:** Dies ist nicht das gleiche Paket mit dem Sie für die CLI, indem Sie die Bearbeitung Installieren der *csproj* Datei.</span><span class="sxs-lookup"><span data-stu-id="2fa6f-227">**Important:** This is not the same package as the one you install for the CLI by editing the *.csproj* file.</span></span> <span data-ttu-id="2fa6f-228">Der Name dieser endet `Tools`, im Gegensatz zu den CLI-Paketnamen die endet in `Tools.DotNet`.</span><span class="sxs-lookup"><span data-stu-id="2fa6f-228">The name of this one ends in `Tools`, unlike the CLI package name which ends in `Tools.DotNet`.</span></span>

<span data-ttu-id="2fa6f-229">Weitere Informationen zu CLI-Befehlen finden Sie unter [.NET Core CLI](https://docs.microsoft.com/ef/core/miscellaneous/cli/dotnet).</span><span class="sxs-lookup"><span data-stu-id="2fa6f-229">For more information about the CLI commands, see [.NET Core CLI](https://docs.microsoft.com/ef/core/miscellaneous/cli/dotnet).</span></span>

<span data-ttu-id="2fa6f-230">Weitere Informationen zu den PMC-Befehlen finden Sie unter [Package Manager Console (Visual Studio)](https://docs.microsoft.com/ef/core/miscellaneous/cli/powershell).</span><span class="sxs-lookup"><span data-stu-id="2fa6f-230">For more information about the PMC commands, see [Package Manager Console (Visual Studio)](https://docs.microsoft.com/ef/core/miscellaneous/cli/powershell).</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="2fa6f-231">Problembehandlung</span><span class="sxs-lookup"><span data-stu-id="2fa6f-231">Troubleshooting</span></span>

<span data-ttu-id="2fa6f-232">Herunterladen der [abgeschlossene Anwendung für diese Stufe](
https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part4-migrations).</span><span class="sxs-lookup"><span data-stu-id="2fa6f-232">Download the [completed app for this stage](
https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part4-migrations).</span></span>

<span data-ttu-id="2fa6f-233">Die app wird die folgende Ausnahme generiert:</span><span class="sxs-lookup"><span data-stu-id="2fa6f-233">The app generates the following exception:</span></span>

```text
`SqlException: Cannot open database "ContosoUniversity" requested by the login.
The login failed.
Login failed for user 'user name'.
```

<span data-ttu-id="2fa6f-234">Lösung: Ausführen`dotnet ef database update`</span><span class="sxs-lookup"><span data-stu-id="2fa6f-234">Solution: Run `dotnet ef database update`</span></span>

<span data-ttu-id="2fa6f-235">Wenn die `update` Befehl gibt dem Fehler "Fehler beim Buildvorgang.":</span><span class="sxs-lookup"><span data-stu-id="2fa6f-235">If the `update` command returns the error "Build failed.":</span></span>

* <span data-ttu-id="2fa6f-236">Führen Sie den Befehl erneut aus.</span><span class="sxs-lookup"><span data-stu-id="2fa6f-236">Run the command again.</span></span>
* <span data-ttu-id="2fa6f-237">Lassen Sie eine Nachricht am unteren Rand der Seite.</span><span class="sxs-lookup"><span data-stu-id="2fa6f-237">Leave a message at the bottom of the page.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="2fa6f-238">[Zurück](xref:data/ef-rp/sort-filter-page)
[Weiter](xref:data/ef-rp/complex-data-model)</span><span class="sxs-lookup"><span data-stu-id="2fa6f-238">[Previous](xref:data/ef-rp/sort-filter-page)
[Next](xref:data/ef-rp/complex-data-model)</span></span>
