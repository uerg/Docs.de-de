---
title: ASP.NET Core MVC mit EF-Kern - Migrationen - 4 von 10
author: tdykstra
description: "In diesem Lernprogramm beginnen Sie mit der EF-Core-Migrationen-Funktion für die Verwaltung von datenmodelländerungen in einer ASP.NET-MVC-Anwendung Core."
ms.author: tdykstra
manager: wpickett
ms.date: 03/15/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: data/ef-mvc/migrations
ms.openlocfilehash: 9081ddd14e6ed9192c6bd8ce8b265d14dbca7e23
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/19/2018
---
# <a name="migrations---ef-core-with-aspnet-core-mvc-tutorial-4-of-10"></a><span data-ttu-id="1df67-103">Migrationen - EF-Core mit ASP.NET Core MVC-Lernprogramm (4 von 10)</span><span class="sxs-lookup"><span data-stu-id="1df67-103">Migrations - EF Core with ASP.NET Core MVC tutorial (4 of 10)</span></span>

<span data-ttu-id="1df67-104">Durch [Tom Dykstra](https://github.com/tdykstra) und [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="1df67-104">By [Tom Dykstra](https://github.com/tdykstra) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="1df67-105">Die Contoso-University Beispielwebanwendung veranschaulicht, wie ASP.NET Core MVC-Webanwendungen, die mit Entity Framework Core und Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="1df67-105">The Contoso University sample web application demonstrates how to create ASP.NET Core MVC web applications using Entity Framework Core and Visual Studio.</span></span> <span data-ttu-id="1df67-106">Informationen über die Reihe von Lernprogrammen finden Sie unter [im ersten Lernprogramm, in der Reihe](intro.md).</span><span class="sxs-lookup"><span data-stu-id="1df67-106">For information about the tutorial series, see [the first tutorial in the series](intro.md).</span></span>

<span data-ttu-id="1df67-107">In diesem Lernprogramm beginnen Sie mit der EF-Core-Migrationen-Funktion für die Verwaltung von datenmodelländerungen.</span><span class="sxs-lookup"><span data-stu-id="1df67-107">In this tutorial, you start using the EF Core migrations feature for managing data model changes.</span></span> <span data-ttu-id="1df67-108">In späteren Lernprogrammen fügen Sie weitere Migrationen, wenn Sie das Datenmodell ändern.</span><span class="sxs-lookup"><span data-stu-id="1df67-108">In later tutorials, you'll add more migrations as you change the data model.</span></span>

## <a name="introduction-to-migrations"></a><span data-ttu-id="1df67-109">Einführung in die Migrationen</span><span class="sxs-lookup"><span data-stu-id="1df67-109">Introduction to migrations</span></span>

<span data-ttu-id="1df67-110">Wenn Sie eine neue Anwendung entwickeln, Ihr Datenmodell ändert sich häufig, und jedes Mal das Modell ändert, ruft nicht synchron mit der Datenbank ab.</span><span class="sxs-lookup"><span data-stu-id="1df67-110">When you develop a new application, your data model changes frequently, and each time the model changes, it gets out of sync with the database.</span></span> <span data-ttu-id="1df67-111">Mit diesen Lernprogrammen wird gestartet, durch die Konfiguration des Entity Framework zum Erstellen der Datenbank, wenn er nicht vorhanden ist.</span><span class="sxs-lookup"><span data-stu-id="1df67-111">You started these tutorials by configuring the Entity Framework to create the database if it doesn't exist.</span></span> <span data-ttu-id="1df67-112">Klicken Sie dann jedes Mal, die Sie ändern das Datenmodell – hinzufügen, entfernen, Entitätsklassen oder ändern die DbContext-Klasse können Sie die Datenbank löschen und EF ein neues Zertifikat an, das das Modell entspricht und startet ihn mit Testdaten erstellt haben.</span><span class="sxs-lookup"><span data-stu-id="1df67-112">Then each time you change the data model -- add, remove, or change entity classes or change your DbContext class -- you can delete the database and EF creates a new one that matches the model, and seeds it with test data.</span></span>

<span data-ttu-id="1df67-113">Synchronisieren der Datenbank mit dem Datenmodell diese Methode funktioniert gut, bis Sie die Anwendung bis hin zur Produktion bereitstellen.</span><span class="sxs-lookup"><span data-stu-id="1df67-113">This method of keeping the database in sync with the data model works well until you deploy the application to production.</span></span> <span data-ttu-id="1df67-114">Wenn die Anwendung in der produktionsumgebung ausgeführt wird, es in der Regel, die Daten, die Sie beibehalten möchten gespeichert sind, und Sie nicht alles, was bei jedem verlieren möchten, nehmen Sie eine Änderung wie z. B. das Hinzufügen einer neuen Spalte.</span><span class="sxs-lookup"><span data-stu-id="1df67-114">When the application is running in production it is usually storing data that you want to keep, and you don't want to lose everything each time you make a change such as adding a new column.</span></span> <span data-ttu-id="1df67-115">Das Feature EF Core Migrationen löst dieses Problem durch Aktivieren von EF zum Aktualisieren des Datenbankschemas statt eine neue Datenbank erstellen.</span><span class="sxs-lookup"><span data-stu-id="1df67-115">The EF Core Migrations feature solves this problem by enabling EF to update the database schema instead of creating  a new database.</span></span>

## <a name="entity-framework-core-nuget-packages-for-migrations"></a><span data-ttu-id="1df67-116">NuGet-Pakete für Migrationen in Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="1df67-116">Entity Framework Core NuGet packages for migrations</span></span>

<span data-ttu-id="1df67-117">Um mit Migrationen arbeiten, können Sie die **Package Manager Console** (PMC) oder die Befehlszeilenschnittstelle (CLI).</span><span class="sxs-lookup"><span data-stu-id="1df67-117">To work with migrations, you can use the **Package Manager Console** (PMC) or the command-line interface (CLI).</span></span>  <span data-ttu-id="1df67-118">Diese Lernprogramme veranschaulichen, wie CLI-Befehlen.</span><span class="sxs-lookup"><span data-stu-id="1df67-118">These tutorials show how to use CLI commands.</span></span> <span data-ttu-id="1df67-119">Informationen zu der Systemmonitor ist am [Ende dieses Lernprogramms](#pmc).</span><span class="sxs-lookup"><span data-stu-id="1df67-119">Information about the PMC is at [the end of this tutorial](#pmc).</span></span>

<span data-ttu-id="1df67-120">Die EF-Tools für die Befehlszeilenschnittstelle (CLI) werden unter [Microsoft.EntityFrameworkCore.Tools.DotNet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools.DotNet) bereitgestellt.</span><span class="sxs-lookup"><span data-stu-id="1df67-120">The EF tools for the command-line interface (CLI) are provided in [Microsoft.EntityFrameworkCore.Tools.DotNet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools.DotNet).</span></span> <span data-ttu-id="1df67-121">Um dieses Paket zu installieren, fügen sie der `DotNetCliToolReference` Sammlung in der *csproj* Datei wie gezeigt.</span><span class="sxs-lookup"><span data-stu-id="1df67-121">To install this package, add it to the `DotNetCliToolReference` collection in the *.csproj* file, as shown.</span></span> <span data-ttu-id="1df67-122">**Hinweis:** Sie müssen dieses Paket installieren, indem Sie die *CSPROJ*-Datei bearbeiten. Sie können den `install-package`-Befehl oder die Paket-Manager-GUI nicht verwenden.</span><span class="sxs-lookup"><span data-stu-id="1df67-122">**Note:** You have to install this package by editing the *.csproj* file; you can't use the `install-package` command or the package manager GUI.</span></span> <span data-ttu-id="1df67-123">Können Sie bearbeiten die *csproj* Datei mit der rechten Maustaste auf den Projektnamen im **Projektmappen-Explorer** auswählen und **ContosoUniversity.csproj bearbeiten**.</span><span class="sxs-lookup"><span data-stu-id="1df67-123">You can edit the *.csproj* file by right-clicking the project name in **Solution Explorer** and selecting **Edit ContosoUniversity.csproj**.</span></span>

[!code-xml[](intro/samples/cu/ContosoUniversity.csproj?range=12-15&highlight=2)]
  
<span data-ttu-id="1df67-124">(Die Versionsnummern in diesem Beispiel wurden beim Erstellen des Lernprogramms geschrieben wurde.)</span><span class="sxs-lookup"><span data-stu-id="1df67-124">(The version numbers in this example were current when the tutorial was written.)</span></span> 

## <a name="change-the-connection-string"></a><span data-ttu-id="1df67-125">Ändern der Verbindungszeichenfolge</span><span class="sxs-lookup"><span data-stu-id="1df67-125">Change the connection string</span></span>

<span data-ttu-id="1df67-126">In der *appsettings.json* Datei, ändern Sie den Namen der Datenbank in der Verbindungszeichenfolge ContosoUniversity2 oder einen anderen Namen, die Sie verwenden, die Sie nicht auf dem Computer verwendet haben.</span><span class="sxs-lookup"><span data-stu-id="1df67-126">In the *appsettings.json* file, change the name of the database in the connection string to ContosoUniversity2 or some other name that you haven't used on the computer you're using.</span></span>

[!code-json[Main](intro/samples/cu/appsettings2.json?range=1-4)]

<span data-ttu-id="1df67-127">Diese Änderung richtet das Projekt ein, damit die erste Migration eine neue Datenbank erstellt wird.</span><span class="sxs-lookup"><span data-stu-id="1df67-127">This change sets up the project so that the first migration will create a new database.</span></span> <span data-ttu-id="1df67-128">Dies ist nicht erforderlich für erste Schritte mit Migrationen, aber Sie werden später angezeigt, daher ist es eine gute Idee bleiben.</span><span class="sxs-lookup"><span data-stu-id="1df67-128">This isn't required for getting started with migrations, but you'll see later why it's a good idea.</span></span>

> [!NOTE]
> <span data-ttu-id="1df67-129">Als Alternative zum Ändern des Datenbanknamens können Sie die Datenbank löschen.</span><span class="sxs-lookup"><span data-stu-id="1df67-129">As an alternative to changing the database name, you can delete the database.</span></span> <span data-ttu-id="1df67-130">Verwendung **Objekt-Explorer von SQL Server** (SSOX) oder die `database drop` CLI-Befehl:</span><span class="sxs-lookup"><span data-stu-id="1df67-130">Use **SQL Server Object Explorer** (SSOX) or the `database drop` CLI command:</span></span>
> ```console
> dotnet ef database drop
> ```
> <span data-ttu-id="1df67-131">Im folgende Abschnitt wird erläutert, wie CLI-Befehle ausgeführt werden.</span><span class="sxs-lookup"><span data-stu-id="1df67-131">The following section explains how to run CLI commands.</span></span>

## <a name="create-an-initial-migration"></a><span data-ttu-id="1df67-132">Erstellen einer anfänglichen Migrations</span><span class="sxs-lookup"><span data-stu-id="1df67-132">Create an initial migration</span></span>

<span data-ttu-id="1df67-133">Speichern Sie die Änderungen zu, und erstellen Sie das Projekt.</span><span class="sxs-lookup"><span data-stu-id="1df67-133">Save your changes and build the project.</span></span> <span data-ttu-id="1df67-134">Klicken Sie dann öffnen Sie ein Befehlsfenster, und navigieren Sie zum Projektordner.</span><span class="sxs-lookup"><span data-stu-id="1df67-134">Then open a command window and navigate to the project folder.</span></span> <span data-ttu-id="1df67-135">Hier ist eine schnelle Möglichkeit hierzu:</span><span class="sxs-lookup"><span data-stu-id="1df67-135">Here's a quick way to do that:</span></span>

* <span data-ttu-id="1df67-136">In **Projektmappen-Explorer**mit der rechten Maustaste auf das Projekt, und wählen Sie **im Datei-Explorer öffnen** aus dem Kontextmenü.</span><span class="sxs-lookup"><span data-stu-id="1df67-136">In **Solution Explorer**, right-click the project and choose **Open in File Explorer** from the context menu.</span></span>

  ![Öffnen Sie im Datei-Explorer-Menüelement](migrations/_static/open-in-file-explorer.png)

* <span data-ttu-id="1df67-138">Geben Sie "Cmd" in der Adressleiste ein, und drücken Sie die EINGABETASTE.</span><span class="sxs-lookup"><span data-stu-id="1df67-138">Enter "cmd" in the address bar and press Enter.</span></span>

  ![Befehlsfenster öffnen](migrations/_static/open-command-window.png)

<span data-ttu-id="1df67-140">Geben Sie im Befehlsfenster folgenden Befehl ein:</span><span class="sxs-lookup"><span data-stu-id="1df67-140">Enter the following command in the command window:</span></span>

```console
dotnet ef migrations add InitialCreate
```

<span data-ttu-id="1df67-141">Die Ausgabe wie folgt im Befehlsfenster angezeigt:</span><span class="sxs-lookup"><span data-stu-id="1df67-141">You see output like the following in the command window:</span></span>

```console
info: Microsoft.AspNetCore.DataProtection.KeyManagement.XmlKeyManager[0]
      User profile is available. Using 'C:\Users\username\AppData\Local\ASP.NET\DataProtection-Keys' as key repository and Windows DPAPI to encrypt keys at rest.
info: Microsoft.EntityFrameworkCore.Infrastructure[100403]
      Entity Framework Core 2.0.0-rtm-26452 initialized 'SchoolContext' using provider 'Microsoft.EntityFrameworkCore.SqlServer' with options: None
Done. To undo this action, use 'ef migrations remove'
```

> [!NOTE]
> <span data-ttu-id="1df67-142">Wenn Sie eine Fehlermeldung *keine ausführbare Datei gefunden übereinstimmenden Befehl "Dotnet-Ef"*, finden Sie unter [diesem Blogbeitrag](http://thedatafarm.com/data-access/no-executable-found-matching-command-dotnet-ef/) für Hilfe zur Problembehandlung.</span><span class="sxs-lookup"><span data-stu-id="1df67-142">If you see an error message *No executable found matching command "dotnet-ef"*, see [this blog post](http://thedatafarm.com/data-access/no-executable-found-matching-command-dotnet-ef/) for help troubleshooting.</span></span>

<span data-ttu-id="1df67-143">Wenn Sie eine Fehlermeldung angezeigt "*... die Datei kann nicht zugegriffen werden. ContosoUniversity.dll, da sie von einem anderen Prozess verwendet wird.* ", suchen Sie das Symbol" IIS Express "in der Windows-Taskleiste der rechten Maustaste darauf klicken, und klicken Sie auf **ContosoUniversity > Stop Standort**.</span><span class="sxs-lookup"><span data-stu-id="1df67-143">If you see an error message "*cannot access the file ... ContosoUniversity.dll because it is being used by another process.*", find the IIS Express icon in the Windows System Tray, and right-click it, then click **ContosoUniversity > Stop Site**.</span></span>

## <a name="examine-the-up-and-down-methods"></a><span data-ttu-id="1df67-144">Überprüfen Sie die nach-oben und nach-unten Sie-Methoden</span><span class="sxs-lookup"><span data-stu-id="1df67-144">Examine the Up and Down methods</span></span>

<span data-ttu-id="1df67-145">Wenn Sie die Ausführung der `migrations add` Befehl EF generiert den Code, der die Datenbank von Grund auf neu erstellt wird.</span><span class="sxs-lookup"><span data-stu-id="1df67-145">When you executed the `migrations add` command, EF generated the code that will create the database from scratch.</span></span> <span data-ttu-id="1df67-146">Dieser Code befindet sich in der *Migrationen* Ordner, in der Datei mit dem Namen  *\<Zeitstempel > _InitialCreate.cs*.</span><span class="sxs-lookup"><span data-stu-id="1df67-146">This code is in the *Migrations* folder, in the file named *\<timestamp>_InitialCreate.cs*.</span></span> <span data-ttu-id="1df67-147">Die `Up` Methode der `InitialCreate` Klasse erstellt, die Datenbanktabellen, die die Daten Modell Entitätenmengen, entsprechen und die `Down` Methode löscht, wie im folgenden Beispiel gezeigt.</span><span class="sxs-lookup"><span data-stu-id="1df67-147">The `Up` method of the `InitialCreate` class creates the database tables that correspond to the data model entity sets, and the `Down` method deletes them, as shown in the following example.</span></span>

[!code-csharp[Main](intro/samples/cu/Migrations/20170215220724_InitialCreate.cs?range=92-118)]

<span data-ttu-id="1df67-148">Migrationen Aufrufe der `Up` Methode für die Implementierung des Datenmodells für eine Migration.</span><span class="sxs-lookup"><span data-stu-id="1df67-148">Migrations calls the `Up` method to implement the data model changes for a migration.</span></span> <span data-ttu-id="1df67-149">Bei der Eingabe eines Befehls ein Rollback der Updates, die Migrationen Aufrufe der `Down` Methode.</span><span class="sxs-lookup"><span data-stu-id="1df67-149">When you enter a command to roll back the update, Migrations calls the `Down` method.</span></span>

<span data-ttu-id="1df67-150">Dieser Code ist für den anfänglichen Migration, die erstellt wurde, wenn Sie eingegeben haben die `migrations add InitialCreate` Befehl.</span><span class="sxs-lookup"><span data-stu-id="1df67-150">This code is for the initial migration that was created when you entered the `migrations add InitialCreate` command.</span></span> <span data-ttu-id="1df67-151">Der Namensparameter der Migration (im Beispiel "InitialCreate") wird verwendet, für den Dateinamen und kann beliebig sein.</span><span class="sxs-lookup"><span data-stu-id="1df67-151">The migration name parameter ("InitialCreate" in the example) is used for the file name and can be whatever you want.</span></span> <span data-ttu-id="1df67-152">Es wird empfohlen, wählen ein Wort oder Ausdruck, die zusammengefasst, was bei der Migration durchgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="1df67-152">It's best to choose a word or phrase that summarizes what is being done in the migration.</span></span> <span data-ttu-id="1df67-153">Sie können z. B. eine spätere Migrationen "AddDepartmentTable" nennen.</span><span class="sxs-lookup"><span data-stu-id="1df67-153">For example, you might name a later migration "AddDepartmentTable".</span></span>

<span data-ttu-id="1df67-154">Wenn Sie die anfängliche Migration erstellt, wenn die Datenbank bereits vorhanden ist, wird die Erstellung Datenbankcode generiert, aber keinen ausführen, da die Datenbank bereits das Datenmodell entspricht.</span><span class="sxs-lookup"><span data-stu-id="1df67-154">If you created the initial migration when the database already exists, the database creation code is generated but it doesn't have to run because the database already matches the data model.</span></span> <span data-ttu-id="1df67-155">Während der Bereitstellung der app in eine andere Umgebung, in dem die Datenbank ist nicht vorhanden, noch dieser Code wird ausgeführt, um Ihre Datenbank zu erstellen, daher ist es eine gute Idee, zuerst zu testen.</span><span class="sxs-lookup"><span data-stu-id="1df67-155">When you deploy the app to another environment where the database doesn't exist yet, this code will run to create your database, so it's a good idea to test it first.</span></span> <span data-ttu-id="1df67-156">Warum ist Sie den Namen der Datenbank in der Verbindungszeichenfolge zuvor--geändert, sodass Migrationen eine neue von Grund auf neu erstellen können.</span><span class="sxs-lookup"><span data-stu-id="1df67-156">That's why you changed the name of the database in the connection string earlier -- so that migrations can create a new one from scratch.</span></span>

## <a name="examine-the-data-model-snapshot"></a><span data-ttu-id="1df67-157">Überprüfen Sie die datenmomentaufnahme für das Modell</span><span class="sxs-lookup"><span data-stu-id="1df67-157">Examine the data model snapshot</span></span>

<span data-ttu-id="1df67-158">Migrationen erstellt außerdem eine *Momentaufnahme* des aktuellen Datenbankschemas in *Migrations/SchoolContextModelSnapshot.cs*.</span><span class="sxs-lookup"><span data-stu-id="1df67-158">Migrations also creates a *snapshot* of the current database schema in *Migrations/SchoolContextModelSnapshot.cs*.</span></span> <span data-ttu-id="1df67-159">Dieses Codes sieht z. B. aus:</span><span class="sxs-lookup"><span data-stu-id="1df67-159">Here's what that code looks like:</span></span>

[!code-csharp[Main](intro/samples/cu/Migrations/SchoolContextModelSnapshot1.cs?name=snippet_Truncate)]

<span data-ttu-id="1df67-160">Da das Schema der aktuellen Datenbank im Code dargestellt wird, verwendet nicht EF Kern, für die Interaktion mit der Datenbank, um Migrationen zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="1df67-160">Because the current database schema is represented in code, EF Core doesn't have to interact with the database to create migrations.</span></span> <span data-ttu-id="1df67-161">Wenn Sie eine Migration hinzufügen, bestimmt EF an, was durch Vergleichen Datenmodell der Datenbankmomentaufnahme-Datei geändert.</span><span class="sxs-lookup"><span data-stu-id="1df67-161">When you add a migration, EF determines what changed by comparing the data model to the snapshot file.</span></span> <span data-ttu-id="1df67-162">EF interagiert mit der Datenbank nur, wenn sie zum Aktualisieren der Datenbank hat.</span><span class="sxs-lookup"><span data-stu-id="1df67-162">EF interacts with the database only when it has to update the database.</span></span> 

<span data-ttu-id="1df67-163">Die Datenbankmomentaufnahme-Datei muss mit der Migrationen synchron gehalten werden, die sie erstellt haben, damit Sie eine Migration nicht entfernen können, indem Sie einfach das Löschen der Datei mit dem Namen  *\<Zeitstempel > _\<Migrationname > .cs*.</span><span class="sxs-lookup"><span data-stu-id="1df67-163">The snapshot file has to be kept in sync with the migrations that create it, so you can't remove a migration just by deleting the file named  *\<timestamp>_\<migrationname>.cs*.</span></span> <span data-ttu-id="1df67-164">Wenn Sie diese Datei löschen, werden die übrigen Migrationen werden nicht mit der Datenbankmomentaufnahme-Datei synchron.</span><span class="sxs-lookup"><span data-stu-id="1df67-164">If you delete that file, the remaining migrations will be out of sync with the database snapshot file.</span></span> <span data-ttu-id="1df67-165">Um der letzten Migration löschen, die Sie hinzugefügt haben, verwenden die [Dotnet Ef Migrationen entfernen](https://docs.microsoft.com/ef/core/miscellaneous/cli/dotnet#dotnet-ef-migrations-remove) Befehl.</span><span class="sxs-lookup"><span data-stu-id="1df67-165">To delete the last migration that you added, use the [dotnet ef migrations remove](https://docs.microsoft.com/ef/core/miscellaneous/cli/dotnet#dotnet-ef-migrations-remove) command.</span></span>

## <a name="apply-the-migration-to-the-database"></a><span data-ttu-id="1df67-166">Die Migration auf die Datenbank anwenden.</span><span class="sxs-lookup"><span data-stu-id="1df67-166">Apply the migration to the database</span></span>

<span data-ttu-id="1df67-167">Geben Sie in das Befehlsfenster den folgenden Befehl an der Datenbank und die Tabellen erstellt werden.</span><span class="sxs-lookup"><span data-stu-id="1df67-167">In the command window, enter the following command to create the database and tables in it.</span></span>

```console
dotnet ef database update
```

<span data-ttu-id="1df67-168">Die Ausgabe des Befehls ähnelt der `migrations add` Befehl, mit dem Unterschied, dass Protokolle anzuzeigen, für die SQL-, die die Datenbank eingerichtet Befehle.</span><span class="sxs-lookup"><span data-stu-id="1df67-168">The output from the command is similar to the `migrations add` command, except that you see logs for the SQL commands that set up the database.</span></span> <span data-ttu-id="1df67-169">Die meisten Protokolle werden in der folgenden Beispielausgabe weggelassen.</span><span class="sxs-lookup"><span data-stu-id="1df67-169">Most of the logs are omitted in the following sample output.</span></span> <span data-ttu-id="1df67-170">Wenn Sie nicht diese Detailebene in Protokollnachrichten anzeigen möchten, können, ändern Sie die Protokollebene in den *"appSettings". Development.JSON* Datei.</span><span class="sxs-lookup"><span data-stu-id="1df67-170">If you prefer not to see this level of detail in log messages, you can change the log level in the *appsettings.Development.json* file.</span></span> <span data-ttu-id="1df67-171">Weitere Informationen finden Sie unter [Einführung in die Protokollierung](xref:fundamentals/logging/index).</span><span class="sxs-lookup"><span data-stu-id="1df67-171">For more information, see [Introduction to logging](xref:fundamentals/logging/index).</span></span>

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

<span data-ttu-id="1df67-172">Verwendung **Objekt-Explorer von SQL Server** , die Datenbank zu überprüfen, wie Sie im ersten Lernprogramm ausgeführt haben.</span><span class="sxs-lookup"><span data-stu-id="1df67-172">Use **SQL Server Object Explorer** to inspect the database as you did in the first tutorial.</span></span>  <span data-ttu-id="1df67-173">Sie werden das Hinzufügen einer Tabelle __EFMigrationsHistory bemerken, die überwacht die Migrationen auf die Datenbank angewendet wurden.</span><span class="sxs-lookup"><span data-stu-id="1df67-173">You'll notice the addition of an __EFMigrationsHistory table that keeps track of which migrations have been applied to the database.</span></span> <span data-ttu-id="1df67-174">Zeigen Sie die Daten in dieser Tabelle, und sehen Sie eine Zeile für die erste Migration.</span><span class="sxs-lookup"><span data-stu-id="1df67-174">View the data in that table and you'll see one row for the first migration.</span></span> <span data-ttu-id="1df67-175">(Das letzte Protokoll im vorherigen Beispiel der CLI-Ausgabe zeigt die INSERT-Anweisung, die diese Zeile erstellt.)</span><span class="sxs-lookup"><span data-stu-id="1df67-175">(The last log in the preceding CLI output example shows the INSERT statement that creates this row.)</span></span>

<span data-ttu-id="1df67-176">Führen Sie die Anwendung aus, um sicherzustellen, dass alles noch funktioniert gleichermaßen wie vor.</span><span class="sxs-lookup"><span data-stu-id="1df67-176">Run the application to verify that everything still works the same as before.</span></span>

![Indexseite für Studenten](migrations/_static/students-index.png)

<a id="pmc"></a>
## <a name="command-line-interface-cli-vs-package-manager-console-pmc"></a><span data-ttu-id="1df67-178">Im Vergleich zur Befehlszeilenschnittstelle (CLI) Paket-Manager-Konsole (PMC)</span><span class="sxs-lookup"><span data-stu-id="1df67-178">Command-line interface (CLI) vs. Package Manager Console (PMC)</span></span>

<span data-ttu-id="1df67-179">Der EF-Tools für die Verwaltung von Migrationen aus .NET Core-CLI-Befehlen oder PowerShell-Cmdlets in der Visual Studio verfügbar ist **Package Manager Console** (PMC) Fenster.</span><span class="sxs-lookup"><span data-stu-id="1df67-179">The EF tooling for managing migrations is available from .NET Core CLI commands or from PowerShell cmdlets in the Visual Studio **Package Manager Console** (PMC) window.</span></span> <span data-ttu-id="1df67-180">Dieses Lernprogramm zeigt, wie die CLI, aber Sie können die PMC verwenden, falls gewünscht.</span><span class="sxs-lookup"><span data-stu-id="1df67-180">This tutorial shows how to use the CLI, but you can use the PMC if you prefer.</span></span>

<span data-ttu-id="1df67-181">Der EF-Befehle für die PMC-Befehle werden in der [Microsoft.EntityFrameworkCore.Tools](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools) Paket.</span><span class="sxs-lookup"><span data-stu-id="1df67-181">The EF commands for the PMC commands are in the [Microsoft.EntityFrameworkCore.Tools](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools) package.</span></span> <span data-ttu-id="1df67-182">Dieses Paket ist bereits in enthalten die [Microsoft.AspNetCore.All](xref:fundamentals/metapackage) Metapackage, daher ist es nicht, es zu installieren.</span><span class="sxs-lookup"><span data-stu-id="1df67-182">This package is already included in the [Microsoft.AspNetCore.All](xref:fundamentals/metapackage) metapackage, so you don't have to install it.</span></span>

<span data-ttu-id="1df67-183">**Wichtig:** Dies ist nicht das gleiche Paket mit dem Sie für die CLI, indem Sie die Bearbeitung Installieren der *csproj* Datei.</span><span class="sxs-lookup"><span data-stu-id="1df67-183">**Important:** This is not the same package as the one you install for the CLI by editing the *.csproj* file.</span></span> <span data-ttu-id="1df67-184">Der Name dieser endet `Tools`, im Gegensatz zu den CLI-Paketnamen die endet in `Tools.DotNet`.</span><span class="sxs-lookup"><span data-stu-id="1df67-184">The name of this one ends in `Tools`, unlike the CLI package name which ends in `Tools.DotNet`.</span></span>

<span data-ttu-id="1df67-185">Weitere Informationen zu CLI-Befehlen finden Sie unter [.NET Core CLI](https://docs.microsoft.com/ef/core/miscellaneous/cli/dotnet).</span><span class="sxs-lookup"><span data-stu-id="1df67-185">For more information about the CLI commands, see [.NET Core CLI](https://docs.microsoft.com/ef/core/miscellaneous/cli/dotnet).</span></span> 

<span data-ttu-id="1df67-186">Weitere Informationen zu den PMC-Befehlen finden Sie unter [Package Manager Console (Visual Studio)](https://docs.microsoft.com/ef/core/miscellaneous/cli/powershell).</span><span class="sxs-lookup"><span data-stu-id="1df67-186">For more information about the PMC commands, see [Package Manager Console (Visual Studio)](https://docs.microsoft.com/ef/core/miscellaneous/cli/powershell).</span></span>

## <a name="summary"></a><span data-ttu-id="1df67-187">Zusammenfassung</span><span class="sxs-lookup"><span data-stu-id="1df67-187">Summary</span></span>

<span data-ttu-id="1df67-188">In diesem Lernprogramm haben Sie gesehen, wie zum Erstellen und Anwenden der ersten Migrations.</span><span class="sxs-lookup"><span data-stu-id="1df67-188">In this tutorial, you've seen how to create and apply your first migration.</span></span> <span data-ttu-id="1df67-189">In den nächsten Lernprogrammen beginnen Sie erweiterte Themen ansehen, erweitern Sie das Datenmodell.</span><span class="sxs-lookup"><span data-stu-id="1df67-189">In the next tutorial, you'll begin looking at more advanced topics by expanding the data model.</span></span> <span data-ttu-id="1df67-190">Nebenbei erstellen und Anwenden von zusätzliche Migrationen.</span><span class="sxs-lookup"><span data-stu-id="1df67-190">Along the way you'll create and apply additional migrations.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="1df67-191">[Zurück](sort-filter-page.md)
[Weiter](complex-data-model.md)</span><span class="sxs-lookup"><span data-stu-id="1df67-191">[Previous](sort-filter-page.md)
[Next](complex-data-model.md)</span></span>  
