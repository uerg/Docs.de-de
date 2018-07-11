---
title: Hinzufügen eines Modells zu einer Razor-Seiten-App in ASP.NET Core mit Visual Studio Code
author: rick-anderson
description: Erfahren Sie, wie Sie ein Modell mithilfe von Visual Studio Code zu einer Razor-Seiten-App in ASP.NET Core hinzufügen.
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.date: 08/27/2017
uid: tutorials/razor-pages-vsc/model
ms.openlocfilehash: 3552b541c43375aef43838800855ec63e7fed372
ms.sourcegitcommit: ea7ec8d47f94cfb8e008d771f647f86bbb4baa44
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/06/2018
ms.locfileid: "37894042"
---
# <a name="add-a-model-to-an-aspnet-core-razor-pages-app-with-visual-studio-code"></a><span data-ttu-id="f62ff-103">Hinzufügen eines Modells zu einer Razor-Seiten-App in ASP.NET Core mit Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="f62ff-103">Add a model to an ASP.NET Core Razor Pages app with Visual Studio Code</span></span>

[!INCLUDE [model1](../../includes/RP/model1.md)]

## <a name="add-a-data-model"></a><span data-ttu-id="f62ff-104">Hinzufügen eines Datenmodells</span><span class="sxs-lookup"><span data-stu-id="f62ff-104">Add a data model</span></span>

* <span data-ttu-id="f62ff-105">Fügen Sie einen Ordner namens *Modelle* hinzu.</span><span class="sxs-lookup"><span data-stu-id="f62ff-105">Add a folder named *Models*.</span></span>
* <span data-ttu-id="f62ff-106">Fügen Sie zum Ordner *Modelle* eine Klasse mit dem Namen *Movie.cs* hinzu.</span><span class="sxs-lookup"><span data-stu-id="f62ff-106">Add a class to the *Models* folder named *Movie.cs*.</span></span>
* <span data-ttu-id="f62ff-107">Fügen Sie der Datei *Models/Movie.cs* den folgenden Code hinzu:</span><span class="sxs-lookup"><span data-stu-id="f62ff-107">Add the following code to the *Models/Movie.cs* file:</span></span>

[!INCLUDE [model 2](../../includes/RP/model2.md)]

[!INCLUDE [model 2a](../../includes/RP/model2a.md)]

[!code-csharp[](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Startup.cs?name=snippet_ConfigureServices2&highlight=3-4)]

<span data-ttu-id="f62ff-108">Fügen Sie am Anfang der Datei *Startup.cs* die folgenden `using`-Anweisungen ein.</span><span class="sxs-lookup"><span data-stu-id="f62ff-108">Add the following `using` statements at the top of *Startup.cs*:</span></span>

```csharp
using RazorPagesMovie.Models;
using Microsoft.EntityFrameworkCore;
```

<span data-ttu-id="f62ff-109">Erstellen Sie das Projekt, um sicherzustellen, dass keine Fehler vorliegen.</span><span class="sxs-lookup"><span data-stu-id="f62ff-109">Build the project to verify you don't have any errors.</span></span>

### <a name="entity-framework-core-nuget-packages-for-migrations"></a><span data-ttu-id="f62ff-110">NuGet-Pakete für Migrationen in Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="f62ff-110">Entity Framework Core NuGet packages for migrations</span></span>

<span data-ttu-id="f62ff-111">Die EF-Tools für die Befehlszeilenschnittstelle (CLI) werden unter [Microsoft.EntityFrameworkCore.Tools.DotNet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools.DotNet) bereitgestellt.</span><span class="sxs-lookup"><span data-stu-id="f62ff-111">The EF tools for the command-line interface (CLI) are provided in [Microsoft.EntityFrameworkCore.Tools.DotNet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools.DotNet).</span></span> <span data-ttu-id="f62ff-112">Fügen Sie das Paket zum Installieren zu der `DotNetCliToolReference`-Sammlung in der *CSPROJ*-Datei hinzu.</span><span class="sxs-lookup"><span data-stu-id="f62ff-112">To install this package, add it to the `DotNetCliToolReference` collection in the *.csproj* file.</span></span> <span data-ttu-id="f62ff-113">**Hinweis:** Sie müssen dieses Paket installieren, indem Sie die *CSPROJ*-Datei bearbeiten. Sie können den `install-package`-Befehl oder die Paket-Manager-GUI nicht verwenden.</span><span class="sxs-lookup"><span data-stu-id="f62ff-113">**Note:** You have to install this package by editing the *.csproj* file; you can't use the `install-package` command or the package manager GUI.</span></span>

<span data-ttu-id="f62ff-114">Bearbeiten Sie die Datei *RazorPagesMovie.csproj*:</span><span class="sxs-lookup"><span data-stu-id="f62ff-114">Edit the *RazorPagesMovie.csproj* file:</span></span>

* <span data-ttu-id="f62ff-115">Klicken Sie auf **Datei** > **Datei öffnen**, und wählen Sie anschließend die Datei *RazorPagesMovie.csproj* aus.</span><span class="sxs-lookup"><span data-stu-id="f62ff-115">Select **File** > **Open File**, and then select the *RazorPagesMovie.csproj* file.</span></span>
* <span data-ttu-id="f62ff-116">Fügen Sie der zweiten **\<ItemGroup>** einen Toolverweis für `Microsoft.EntityFrameworkCore.Tools.DotNet` hinzu:</span><span class="sxs-lookup"><span data-stu-id="f62ff-116">Add tool reference for `Microsoft.EntityFrameworkCore.Tools.DotNet` to the second **\<ItemGroup>**:</span></span>

[!code-xml[](../../tutorials/razor-pages/razor-pages-start/snapshot_cli_sample/RazorPagesMovie/RazorPagesMovie.cli.csproj)]

[!INCLUDE [model 3](../../includes/RP/model3.md)]

<a name="scaffold"></a>
### <a name="scaffold-the-movie-model"></a><span data-ttu-id="f62ff-117">Aufbauen des Filmmodells</span><span class="sxs-lookup"><span data-stu-id="f62ff-117">Scaffold the Movie model</span></span>

* <span data-ttu-id="f62ff-118">Öffnen Sie ein Befehlsfenster im Projektverzeichnis (das Verzeichnis mit den Dateien *Program.cs*, *Startup.cs*, und *CSPROJ*).</span><span class="sxs-lookup"><span data-stu-id="f62ff-118">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="f62ff-119">Führen Sie den folgenden Befehl aus:</span><span class="sxs-lookup"><span data-stu-id="f62ff-119">Run the following command:</span></span>

<span data-ttu-id="f62ff-120">**Hinweis: Führen Sie den folgenden Befehl unter Windows aus. Für MacOS und Linux gilt der nächste Befehl.**</span><span class="sxs-lookup"><span data-stu-id="f62ff-120">**Note: Run the following command on Windows. For MacOS and Linux, see the next command**</span></span>

  ```console
  dotnet aspnet-codegenerator razorpage -m Movie -dc MovieContext -udl -outDir Pages\Movies --referenceScriptLibraries
  ```

* <span data-ttu-id="f62ff-121">Führen Sie unter macOS und Linux den folgenden Befehl aus:</span><span class="sxs-lookup"><span data-stu-id="f62ff-121">On MacOS and Linux, run the following command:</span></span>

  ```console
  dotnet aspnet-codegenerator razorpage -m Movie -dc MovieContext -udl -outDir Pages/Movies --referenceScriptLibraries
  ```

<span data-ttu-id="f62ff-122">Wenn Sie eine Fehlermeldung erhalten:</span><span class="sxs-lookup"><span data-stu-id="f62ff-122">If you get the error:</span></span>
  ```
  The process cannot access the file 
 'RazorPagesMovie/bin/Debug/netcoreapp2.0/RazorPagesMovie.dll' 
  because it is being used by another process.
  ```

<span data-ttu-id="f62ff-123">Beenden Sie Visual Studio, und führen Sie den Befehl erneut aus.</span><span class="sxs-lookup"><span data-stu-id="f62ff-123">Exit Visual Studio and run the command again.</span></span>

[!INCLUDE [model 4](../../includes/RP/model4.md)]

<span data-ttu-id="f62ff-124">Im nächsten Tutorial finden Sie Erläuterungen zu den Dateien, die durch den Gerüstbau erstellt wurden.</span><span class="sxs-lookup"><span data-stu-id="f62ff-124">The next tutorial explains the files created by scaffolding.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="f62ff-125">[Zurück: Erste Schritte](xref:tutorials/razor-pages-vsc/razor-pages-start)
> [Weiter: Gerüstbau mit Razor-Seiten](xref:tutorials/razor-pages-vsc/page)</span><span class="sxs-lookup"><span data-stu-id="f62ff-125">[Previous: Get Started](xref:tutorials/razor-pages-vsc/razor-pages-start)
[Next: Scaffolded Razor Pages](xref:tutorials/razor-pages-vsc/page)</span></span>
