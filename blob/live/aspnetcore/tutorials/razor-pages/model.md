---
title: "Hinzufügen eines Modells zu einer App mit Razor-Seiten in ASP.NET Core"
author: rick-anderson
description: "Hinzufügen eines Modells zu einer App mit Razor-Seiten in ASP.NET Core"
keywords: ASP.NET Core, Razor-Seiten, Razor, MVC
ms.author: riande
manager: wpickett
ms.date: 07/27/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: aspnet-core
uid: tutorials/razor-pages/model
ms.openlocfilehash: 38f27a1d5ca80cec4b7bc43c3d5473fc829f1b05
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
# <a name="adding-a-model-to-a-razor-pages-app"></a><span data-ttu-id="37c3c-104">Hinzufügen eines Modells zu einer App mit Razor-Seiten</span><span class="sxs-lookup"><span data-stu-id="37c3c-104">Adding a model to a Razor Pages app</span></span>

[!INCLUDE[model1](../../includes/RP/model1.md)]

## <a name="add-a-data-model"></a><span data-ttu-id="37c3c-105">Hinzufügen eines Datenmodells</span><span class="sxs-lookup"><span data-stu-id="37c3c-105">Add a data model</span></span>

<span data-ttu-id="37c3c-106">Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf das Projekt **RazorPagesMovie** > **Hinzufügen** > **Neuer Ordner**.</span><span class="sxs-lookup"><span data-stu-id="37c3c-106">In Solution Explorer, right-click the **RazorPagesMovie** project > **Add** > **New Folder**.</span></span> <span data-ttu-id="37c3c-107">Geben Sie dem Ordner den Namen *Modelle*.</span><span class="sxs-lookup"><span data-stu-id="37c3c-107">Name the folder *Models*.</span></span>

<span data-ttu-id="37c3c-108">Klicken Sie mit der rechten Maustaste auf den Ordner *Modelle*.</span><span class="sxs-lookup"><span data-stu-id="37c3c-108">Right click the *Models* folder.</span></span> <span data-ttu-id="37c3c-109">Wählen Sie **Hinzufügen** > **Klasse** aus.</span><span class="sxs-lookup"><span data-stu-id="37c3c-109">Select **Add** > **Class**.</span></span> <span data-ttu-id="37c3c-110">Nennen Sie die Klasse **Movie**, und fügen Sie Ihr die folgenden Eigenschaften hinzu:</span><span class="sxs-lookup"><span data-stu-id="37c3c-110">Name the class **Movie** and add the following properties:</span></span>

[!INCLUDE[model 2](../../includes/RP/model2.md)]

<a name="cs"></a>
### <a name="add-a-database-connection-string"></a><span data-ttu-id="37c3c-111">Hinzufügen einer Datenbank-Verbindungszeichenfolge</span><span class="sxs-lookup"><span data-stu-id="37c3c-111">Add a database connection string</span></span>

<span data-ttu-id="37c3c-112">Fügen Sie der Datei *appsettings.json* eine Verbindungszeichenfolge hinzu.</span><span class="sxs-lookup"><span data-stu-id="37c3c-112">Add a connection string to the *appsettings.json* file.</span></span>

[!code-json[Main](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/appsettings.json?highlight=8-10)]

<a name="reg"></a>
###  <a name="register-the-database-context"></a><span data-ttu-id="37c3c-113">Registrieren des Datenbankkontexts</span><span class="sxs-lookup"><span data-stu-id="37c3c-113">Register the database context</span></span>

<span data-ttu-id="37c3c-114">Registrieren Sie den Datenbankkontext mit dem [Abhängigkeitsinjektionscontainer](xref:fundamentals/dependency-injection) in der Datei *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="37c3c-114">Register the database context with the [dependency injection](xref:fundamentals/dependency-injection) container in the *Startup.cs* file.</span></span>

[!code-csharp[Main](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Startup.cs?name=snippet_ConfigureServices&highlight=3-6)]

<span data-ttu-id="37c3c-115">Erstellen Sie das Projekt, um sicherzustellen, dass keine Fehler vorliegen.</span><span class="sxs-lookup"><span data-stu-id="37c3c-115">Build the project to verify you don't have any errors.</span></span>

<a name="pmc"></a>
## <a name="add-scaffold-tooling-and-perform-initial-migration"></a><span data-ttu-id="37c3c-116">Hinzufügen von Tools für den Gerüstbau und Ausführen der anfänglichen Migration</span><span class="sxs-lookup"><span data-stu-id="37c3c-116">Add scaffold tooling and perform initial migration</span></span>

<span data-ttu-id="37c3c-117">In diesem Abschnitt führen Sie folgende Aktionen mit der Paket-Manager-Konsole (Package Manager Console, PMC) aus:</span><span class="sxs-lookup"><span data-stu-id="37c3c-117">In this section, you use the Package Manager Console (PMC) to:</span></span>

* <span data-ttu-id="37c3c-118">Fügen Sie das Visual Studio-Paket für die Webcodegenerierung hinzu.</span><span class="sxs-lookup"><span data-stu-id="37c3c-118">Add the Visual Studio web code generation package.</span></span> <span data-ttu-id="37c3c-119">Dieses Paket ist für die Ausführung des Gerüstbaumoduls erforderlich.</span><span class="sxs-lookup"><span data-stu-id="37c3c-119">This package is required to run the scaffolding engine.</span></span>
* <span data-ttu-id="37c3c-120">Fügen Sie eine anfängliche Migration hinzu.</span><span class="sxs-lookup"><span data-stu-id="37c3c-120">Add an initial migration.</span></span>
* <span data-ttu-id="37c3c-121">Aktualisieren Sie die Datenbank mit der anfänglichen Migration.</span><span class="sxs-lookup"><span data-stu-id="37c3c-121">Update the database with the initial migration.</span></span>

<span data-ttu-id="37c3c-122">Öffnen Sie das Menü **Extras**, und wählen Sie **NuGet-Paket-Manager** > **Paket-Manager-Konsole** aus.</span><span class="sxs-lookup"><span data-stu-id="37c3c-122">From the **Tools** menu, select **NuGet Package Manager** > **Package Manager Console**.</span></span>

  ![PMC-Menü](../first-mvc-app/adding-model/_static/pmc.png)

<span data-ttu-id="37c3c-124">Geben Sie in der PMC die folgenden Befehle ein:</span><span class="sxs-lookup"><span data-stu-id="37c3c-124">In the PMC, enter the following commands:</span></span>

```powershell
Install-Package Microsoft.VisualStudio.Web.CodeGeneration.Design -Version 2.0.0
Add-Migration Initial
Update-Database
```

<span data-ttu-id="37c3c-125">Mit dem Befehl `Install-Package` werden die für das Gerüstbaumodul benötigten Tools installiert.</span><span class="sxs-lookup"><span data-stu-id="37c3c-125">The `Install-Package` command installs the tooling required to run the scaffolding engine.</span></span>

<span data-ttu-id="37c3c-126">Mit dem Befehl `Add-Migration` wird Code generiert, um das anfängliche Datenbankschema zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="37c3c-126">The `Add-Migration` command generates code to create the initial database schema.</span></span> <span data-ttu-id="37c3c-127">Das Schema basiert auf dem Modell, das in der Datei *Modelle/MovieContext.cs* für `DbContext` angegeben ist.</span><span class="sxs-lookup"><span data-stu-id="37c3c-127">The schema is based on the model specified in the `DbContext` (In the *Models/MovieContext.cs* file).</span></span> <span data-ttu-id="37c3c-128">Das Argument `Initial` wird verwendet, um die Migrationen zu benennen.</span><span class="sxs-lookup"><span data-stu-id="37c3c-128">The `Initial` argument is used to name the migrations.</span></span> <span data-ttu-id="37c3c-129">Sie können einen beliebigen Namen auswählen, sollten aber der Konvention zufolge einen Namen verwenden, der die Migration beschreibt.</span><span class="sxs-lookup"><span data-stu-id="37c3c-129">You can use any name, but by convention you choose a name that describes the migration.</span></span> <span data-ttu-id="37c3c-130">Weitere Informationen finden Sie unter [Introduction to migrations (Einführung in Migrationen)](xref:data/ef-mvc/migrations#introduction-to-migrations).</span><span class="sxs-lookup"><span data-stu-id="37c3c-130">See [Introduction to migrations](xref:data/ef-mvc/migrations#introduction-to-migrations) for more information.</span></span>

<span data-ttu-id="37c3c-131">Mit dem Befehl `Update-Database` führen Sie in der Datei *Migrations/\<time-stamp>_InitialCreate.cs* die Methode `Up` aus, mit der die Datenbank erstellt wird.</span><span class="sxs-lookup"><span data-stu-id="37c3c-131">The `Update-Database` command runs the `Up` method in the *Migrations/\<time-stamp>_InitialCreate.cs* file, which creates the database.</span></span>

[!INCLUDE[model 4windows](../../includes/RP/model4Win.md)]

[!INCLUDE[model 4](../../includes/RP/model4tbl.md)]

<a name="test"></a>
### <a name="test-the-app"></a><span data-ttu-id="37c3c-132">Testen der App</span><span class="sxs-lookup"><span data-stu-id="37c3c-132">Test the app</span></span>

* <span data-ttu-id="37c3c-133">Führen Sie die App aus, und fügen Sie `/Movies` an die URL im Browser an (`http://localhost:port/movies`).</span><span class="sxs-lookup"><span data-stu-id="37c3c-133">Run the app and append `/Movies` to the URL in the browser (`http://localhost:port/movies`).</span></span>
* <span data-ttu-id="37c3c-134">Testen Sie den Link **Create** (Erstellen).</span><span class="sxs-lookup"><span data-stu-id="37c3c-134">Test the **Create** link.</span></span>

 ![Seite „Create“](../../tutorials/razor-pages/model/_static/conan.png)

<a name="scaffold"></a>

* <span data-ttu-id="37c3c-136">Testen Sie die Links **Edit** (Bearbeiten), **Details** und **Delete** (Löschen).</span><span class="sxs-lookup"><span data-stu-id="37c3c-136">Test the **Edit**, **Details**, and **Delete** links.</span></span>

<span data-ttu-id="37c3c-137">Wenn eine SQL-Ausnahme ausgelöst wird, stellen Sie sicher, dass Migrationen ausgeführt wurden und die Datenbank aktualisiert wurde:</span><span class="sxs-lookup"><span data-stu-id="37c3c-137">If you get a SQL exception, verify you have run migrations and updated the database:</span></span>

<span data-ttu-id="37c3c-138">Im nächsten Tutorial finden Sie Erläuterungen zu den Dateien, die durch den Gerüstbau erstellt wurden.</span><span class="sxs-lookup"><span data-stu-id="37c3c-138">The next tutorial explains the files created by scaffolding.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="37c3c-139">[Vorheriges Tutorial: Getting Started (Erste Schritte)](xref:tutorials/razor-pages/razor-pages-start)
[Nächstes Tutorial: Scaffolded Razor Pages (Razor-Seiten mit erfolgtem Gerüstbau)](xref:tutorials/razor-pages/page)</span><span class="sxs-lookup"><span data-stu-id="37c3c-139">[Previous: Getting Started](xref:tutorials/razor-pages/razor-pages-start)
[Next: Scaffolded Razor Pages](xref:tutorials/razor-pages/page)</span></span>    
