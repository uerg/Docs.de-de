---
title: "Hinzufügen eines Modells zu einer App mit Razor-Seiten in ASP.NET Core"
author: rick-anderson
description: "Hinzufügen eines Modells zu einer App mit Razor-Seiten in ASP.NET Core"
manager: wpickett
ms.author: riande
ms.date: 07/27/2017
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/razor-pages/model
ms.openlocfilehash: 0ce7693bfdc37d930488304b329dbcd533a5ec1d
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2018
---
# <a name="adding-a-model-to-a-razor-pages-app"></a><span data-ttu-id="b6a67-103">Hinzufügen eines Modells zu einer App mit Razor-Seiten</span><span class="sxs-lookup"><span data-stu-id="b6a67-103">Adding a model to a Razor Pages app</span></span>

[!INCLUDE[model1](../../includes/RP/model1.md)]

## <a name="add-a-data-model"></a><span data-ttu-id="b6a67-104">Hinzufügen eines Datenmodells</span><span class="sxs-lookup"><span data-stu-id="b6a67-104">Add a data model</span></span>

<span data-ttu-id="b6a67-105">Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf das Projekt **RazorPagesMovie** > **Hinzufügen** > **Neuer Ordner**.</span><span class="sxs-lookup"><span data-stu-id="b6a67-105">In Solution Explorer, right-click the **RazorPagesMovie** project > **Add** > **New Folder**.</span></span> <span data-ttu-id="b6a67-106">Geben Sie dem Ordner den Namen *Modelle*.</span><span class="sxs-lookup"><span data-stu-id="b6a67-106">Name the folder *Models*.</span></span>

<span data-ttu-id="b6a67-107">Klicken Sie mit der rechten Maustaste auf den Ordner *Modelle*.</span><span class="sxs-lookup"><span data-stu-id="b6a67-107">Right click the *Models* folder.</span></span> <span data-ttu-id="b6a67-108">Wählen Sie **Hinzufügen** > **Klasse** aus.</span><span class="sxs-lookup"><span data-stu-id="b6a67-108">Select **Add** > **Class**.</span></span> <span data-ttu-id="b6a67-109">Nennen Sie die Klasse **Movie**, und fügen Sie Ihr die folgenden Eigenschaften hinzu:</span><span class="sxs-lookup"><span data-stu-id="b6a67-109">Name the class **Movie** and add the following properties:</span></span>

[!INCLUDE[model 2](../../includes/RP/model2.md)]

<a name="cs"></a>
### <a name="add-a-database-connection-string"></a><span data-ttu-id="b6a67-110">Hinzufügen einer Datenbank-Verbindungszeichenfolge</span><span class="sxs-lookup"><span data-stu-id="b6a67-110">Add a database connection string</span></span>

<span data-ttu-id="b6a67-111">Fügen Sie der Datei *appsettings.json* eine Verbindungszeichenfolge hinzu.</span><span class="sxs-lookup"><span data-stu-id="b6a67-111">Add a connection string to the *appsettings.json* file.</span></span>

[!code-json[Main](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/appsettings.json?highlight=8-10)]

<a name="reg"></a>
###  <a name="register-the-database-context"></a><span data-ttu-id="b6a67-112">Registrieren des Datenbankkontexts</span><span class="sxs-lookup"><span data-stu-id="b6a67-112">Register the database context</span></span>

<span data-ttu-id="b6a67-113">Registrieren Sie den Datenbankkontext mit dem [Abhängigkeitsinjektionscontainer](xref:fundamentals/dependency-injection) in der Datei *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="b6a67-113">Register the database context with the [dependency injection](xref:fundamentals/dependency-injection) container in the *Startup.cs* file.</span></span>

[!code-csharp[Main](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Startup.cs?name=snippet_ConfigureServices&highlight=3-5,7-9)]

<span data-ttu-id="b6a67-114">Erstellen Sie das Projekt, um sicherzustellen, dass keine Fehler vorliegen.</span><span class="sxs-lookup"><span data-stu-id="b6a67-114">Build the project to verify you don't have any errors.</span></span>

<a name="pmc"></a>
## <a name="add-scaffold-tooling-and-perform-initial-migration"></a><span data-ttu-id="b6a67-115">Hinzufügen von Tools für den Gerüstbau und Ausführen der anfänglichen Migration</span><span class="sxs-lookup"><span data-stu-id="b6a67-115">Add scaffold tooling and perform initial migration</span></span>

<span data-ttu-id="b6a67-116">In diesem Abschnitt führen Sie folgende Aktionen mit der Paket-Manager-Konsole (Package Manager Console, PMC) aus:</span><span class="sxs-lookup"><span data-stu-id="b6a67-116">In this section, you use the Package Manager Console (PMC) to:</span></span>

* <span data-ttu-id="b6a67-117">Fügen Sie das Visual Studio-Paket für die Webcodegenerierung hinzu.</span><span class="sxs-lookup"><span data-stu-id="b6a67-117">Add the Visual Studio web code generation package.</span></span> <span data-ttu-id="b6a67-118">Dieses Paket ist für die Ausführung des Gerüstbaumoduls erforderlich.</span><span class="sxs-lookup"><span data-stu-id="b6a67-118">This package is required to run the scaffolding engine.</span></span>
* <span data-ttu-id="b6a67-119">Fügen Sie eine anfängliche Migration hinzu.</span><span class="sxs-lookup"><span data-stu-id="b6a67-119">Add an initial migration.</span></span>
* <span data-ttu-id="b6a67-120">Aktualisieren Sie die Datenbank mit der anfänglichen Migration.</span><span class="sxs-lookup"><span data-stu-id="b6a67-120">Update the database with the initial migration.</span></span>

<span data-ttu-id="b6a67-121">Öffnen Sie das Menü **Extras**, und wählen Sie **NuGet-Paket-Manager** > **Paket-Manager-Konsole** aus.</span><span class="sxs-lookup"><span data-stu-id="b6a67-121">From the **Tools** menu, select **NuGet Package Manager** > **Package Manager Console**.</span></span>

  ![PMC-Menü](../first-mvc-app/adding-model/_static/pmc.png)

<span data-ttu-id="b6a67-123">Geben Sie in der PMC die folgenden Befehle ein:</span><span class="sxs-lookup"><span data-stu-id="b6a67-123">In the PMC, enter the following commands:</span></span>

```powershell
Install-Package Microsoft.VisualStudio.Web.CodeGeneration.Design
Add-Migration Initial
Update-Database
```

<span data-ttu-id="b6a67-124">Mit dem Befehl `Install-Package` werden die für das Gerüstbaumodul benötigten Tools installiert.</span><span class="sxs-lookup"><span data-stu-id="b6a67-124">The `Install-Package` command installs the tooling required to run the scaffolding engine.</span></span>

<span data-ttu-id="b6a67-125">Mit dem Befehl `Add-Migration` wird Code generiert, um das anfängliche Datenbankschema zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="b6a67-125">The `Add-Migration` command generates code to create the initial database schema.</span></span> <span data-ttu-id="b6a67-126">Das Schema basiert auf dem Modell, das in der Datei *Modelle/MovieContext.cs* für `DbContext` angegeben ist.</span><span class="sxs-lookup"><span data-stu-id="b6a67-126">The schema is based on the model specified in the `DbContext` (In the *Models/MovieContext.cs* file).</span></span> <span data-ttu-id="b6a67-127">Das Argument `Initial` wird verwendet, um die Migrationen zu benennen.</span><span class="sxs-lookup"><span data-stu-id="b6a67-127">The `Initial` argument is used to name the migrations.</span></span> <span data-ttu-id="b6a67-128">Sie können einen beliebigen Namen auswählen, sollten aber der Konvention zufolge einen Namen verwenden, der die Migration beschreibt.</span><span class="sxs-lookup"><span data-stu-id="b6a67-128">You can use any name, but by convention you choose a name that describes the migration.</span></span> <span data-ttu-id="b6a67-129">Weitere Informationen finden Sie unter [Introduction to migrations (Einführung in Migrationen)](xref:data/ef-mvc/migrations#introduction-to-migrations).</span><span class="sxs-lookup"><span data-stu-id="b6a67-129">See [Introduction to migrations](xref:data/ef-mvc/migrations#introduction-to-migrations) for more information.</span></span>

<span data-ttu-id="b6a67-130">Mit dem Befehl `Update-Database` führen Sie in der Datei *Migrations/\<time-stamp>_InitialCreate.cs* die Methode `Up` aus, mit der die Datenbank erstellt wird.</span><span class="sxs-lookup"><span data-stu-id="b6a67-130">The `Update-Database` command runs the `Up` method in the *Migrations/\<time-stamp>_InitialCreate.cs* file, which creates the database.</span></span>

[!INCLUDE[model 4windows](../../includes/RP/model4Win.md)]

[!INCLUDE[model 4](../../includes/RP/model4tbl.md)]

<a name="test"></a>
### <a name="test-the-app"></a><span data-ttu-id="b6a67-131">Testen der App</span><span class="sxs-lookup"><span data-stu-id="b6a67-131">Test the app</span></span>

* <span data-ttu-id="b6a67-132">Führen Sie die App aus, und fügen Sie `/Movies` an die URL im Browser an (`http://localhost:port/movies`).</span><span class="sxs-lookup"><span data-stu-id="b6a67-132">Run the app and append `/Movies` to the URL in the browser (`http://localhost:port/movies`).</span></span>
* <span data-ttu-id="b6a67-133">Testen Sie den Link **Create** (Erstellen).</span><span class="sxs-lookup"><span data-stu-id="b6a67-133">Test the **Create** link.</span></span>

 ![Seite „Create“](../../tutorials/razor-pages/model/_static/conan.png)

<a name="scaffold"></a>

* <span data-ttu-id="b6a67-135">Testen Sie die Links **Edit** (Bearbeiten), **Details** und **Delete** (Löschen).</span><span class="sxs-lookup"><span data-stu-id="b6a67-135">Test the **Edit**, **Details**, and **Delete** links.</span></span>

<span data-ttu-id="b6a67-136">Wenn eine SQL-Ausnahme ausgelöst wird, stellen Sie sicher, dass Migrationen ausgeführt wurden und die Datenbank aktualisiert wurde:</span><span class="sxs-lookup"><span data-stu-id="b6a67-136">If you get a SQL exception, verify you have run migrations and updated the database:</span></span>

<span data-ttu-id="b6a67-137">Im nächsten Tutorial finden Sie Erläuterungen zu den Dateien, die durch den Gerüstbau erstellt wurden.</span><span class="sxs-lookup"><span data-stu-id="b6a67-137">The next tutorial explains the files created by scaffolding.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="b6a67-138">[Vorheriges Tutorial: Getting Started (Erste Schritte)](xref:tutorials/razor-pages/razor-pages-start)
[Nächstes Tutorial: Scaffolded Razor Pages (Razor-Seiten mit erfolgtem Gerüstbau)](xref:tutorials/razor-pages/page)</span><span class="sxs-lookup"><span data-stu-id="b6a67-138">[Previous: Getting Started](xref:tutorials/razor-pages/razor-pages-start)
[Next: Scaffolded Razor Pages](xref:tutorials/razor-pages/page)</span></span>    
