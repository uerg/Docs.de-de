---
title: "Hinzufügen eines Modells zu einer Razor-Seiten-App mit Visual Studio für Mac"
author: rick-anderson
description: "Hinzufügen eines Modells zu einer Razor-Seiten-App in ASP.NET Core mithilfe von Visual Studio für Mac"
keywords: ASP.NET Core, Razor-Seiten, Razor, MVC
ms.author: riande
manager: wpickett
ms.date: 08/27/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: aspnet-core
uid: tutorials/razor-pages-vsc/model
ms.openlocfilehash: 445c13542bd4a7561eb6aa509b1aabe47cb45468
ms.sourcegitcommit: 67f54fabbfa4e3942f5bfe1f8a7fdfe4a7a75358
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/19/2017
---
# <a name="adding-a-model-to-a-razor-pages-app-in-aspnet-core-with-visual-studio-for-mac"></a><span data-ttu-id="90e9a-104">Hinzufügen eines Modells zu einer Razor-Seiten-App in ASP.NET Core mit Visual Studio für Mac</span><span class="sxs-lookup"><span data-stu-id="90e9a-104">Adding a model to a Razor Pages app in ASP.NET Core with Visual Studio for Mac</span></span>

[!INCLUDE[model1](../../includes/RP/model1.md)]

## <a name="add-a-data-model"></a><span data-ttu-id="90e9a-105">Hinzufügen eines Datenmodells</span><span class="sxs-lookup"><span data-stu-id="90e9a-105">Add a data model</span></span>

* <span data-ttu-id="90e9a-106">Fügen Sie einen Ordner namens *Modelle* hinzu.</span><span class="sxs-lookup"><span data-stu-id="90e9a-106">Add a folder named *Models*.</span></span>
* <span data-ttu-id="90e9a-107">Fügen Sie zum Ordner *Modelle* eine Klasse mit dem Namen *Movie.cs* hinzu.</span><span class="sxs-lookup"><span data-stu-id="90e9a-107">Add a class to the *Models* folder named *Movie.cs*.</span></span>
* <span data-ttu-id="90e9a-108">Fügen Sie der Datei *Models/Movie.cs* den folgenden Code hinzu:</span><span class="sxs-lookup"><span data-stu-id="90e9a-108">Add the following code to the *Models/Movie.cs* file:</span></span>

[!INCLUDE[model 2](../../includes/RP/model2.md)]
[!INCLUDE[model 2a](../../includes/RP/model2a.md)]

[!code-csharp[Main](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Startup.cs?name=snippet_ConfigureServices2&highlight=3-6)]

<span data-ttu-id="90e9a-109">Erstellen Sie das Projekt, um sicherzustellen, dass keine Fehler vorliegen.</span><span class="sxs-lookup"><span data-stu-id="90e9a-109">Build the project to verify you don't have any errors.</span></span>

### <a name="entity-framework-core-nuget-packages-for-migrations"></a><span data-ttu-id="90e9a-110">NuGet-Pakete für Migrationen in Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="90e9a-110">Entity Framework Core NuGet packages for migrations</span></span>

<span data-ttu-id="90e9a-111">Die EF-Tools für die Befehlszeilenschnittstelle (CLI) werden unter [Microsoft.EntityFrameworkCore.Tools.DotNet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools.DotNet) bereitgestellt.</span><span class="sxs-lookup"><span data-stu-id="90e9a-111">The EF tools for the command-line interface (CLI) are provided in [Microsoft.EntityFrameworkCore.Tools.DotNet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools.DotNet).</span></span> <span data-ttu-id="90e9a-112">Fügen Sie das Paket zum Installieren zu der `DotNetCliToolReference`-Sammlung in der *CSPROJ*-Datei hinzu.</span><span class="sxs-lookup"><span data-stu-id="90e9a-112">To install this package, add it to the `DotNetCliToolReference` collection in the *.csproj* file.</span></span> <span data-ttu-id="90e9a-113">**Hinweis:** Sie müssen dieses Paket installieren, indem Sie die *CSPROJ*-Datei bearbeiten. Sie können den `install-package`-Befehl oder die Paket-Manager-GUI nicht verwenden.</span><span class="sxs-lookup"><span data-stu-id="90e9a-113">**Note:** You have to install this package by editing the *.csproj* file; you can't use the `install-package` command or the package manager GUI.</span></span>

<span data-ttu-id="90e9a-114">Bearbeiten Sie die Datei *RazorPagesMovie.csproj*:</span><span class="sxs-lookup"><span data-stu-id="90e9a-114">Edit the *RazorPagesMovie.csproj* file:</span></span>

* <span data-ttu-id="90e9a-115">Klicken Sie auf **Datei > Datei öffnen**, und wählen Sie dann die Datei *RazorPagesMovie.csproj* aus.</span><span class="sxs-lookup"><span data-stu-id="90e9a-115">Select **File > Open File**, and then select the *RazorPagesMovie.csproj* file.</span></span>
* <span data-ttu-id="90e9a-116">Fügen Sie `<DotNetCliToolReference Include="Microsoft.EntityFrameworkCore.Tools.DotNet" Version="2.0.0" />` wie im folgenden Code hervorgehoben hinzu:</span><span class="sxs-lookup"><span data-stu-id="90e9a-116">Add `<DotNetCliToolReference Include="Microsoft.EntityFrameworkCore.Tools.DotNet" Version="2.0.0" />` as highlighted in the following code:</span></span>

[!code-xml[](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/RazorPagesMovie.cli.csproj?highlight=10)]
[!INCLUDE[model 3](../../includes/RP/model3.md)]

<a name="scaffold"></a>
### <a name="scaffold-the-movie-model"></a><span data-ttu-id="90e9a-117">Aufbauen des Filmmodells</span><span class="sxs-lookup"><span data-stu-id="90e9a-117">Scaffold the Movie model</span></span>

* <span data-ttu-id="90e9a-118">Öffnen Sie ein Befehlsfenster im Projektverzeichnis (das Verzeichnis mit den Dateien *Program.cs*, *Startup.cs*, und *CSPROJ*).</span><span class="sxs-lookup"><span data-stu-id="90e9a-118">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="90e9a-119">Führen Sie den folgenden Befehl aus:</span><span class="sxs-lookup"><span data-stu-id="90e9a-119">Run the following command:</span></span>

<span data-ttu-id="90e9a-120">**Hinweis: Führen Sie den folgenden Befehl unter Windows aus. Für MacOS und Linux gilt der nächste Befehl.**</span><span class="sxs-lookup"><span data-stu-id="90e9a-120">**Note: Run the following command on Windows. For MacOS and Linux, see the next command**</span></span>

  ```console
  dotnet aspnet-codegenerator razorpage -m Movie -dc MovieContext -udl -outDir Pages\Movies --referenceScriptLibraries
  ```

* <span data-ttu-id="90e9a-121">Führen Sie unter macOS und Linux den folgenden Befehl aus:</span><span class="sxs-lookup"><span data-stu-id="90e9a-121">On MacOS and Linux, run the following command:</span></span>

  ```console
  dotnet aspnet-codegenerator razorpage -m Movie -dc MovieContext -udl -outDir Pages/Movies --referenceScriptLibraries
  ```

<span data-ttu-id="90e9a-122">Wenn Sie eine Fehlermeldung erhalten:</span><span class="sxs-lookup"><span data-stu-id="90e9a-122">If you get the error:</span></span>
  ```
  The process cannot access the file 
 'RazorPagesMovie/bin/Debug/netcoreapp2.0/RazorPagesMovie.dll' 
  because it is being used by another process.
  ```

<span data-ttu-id="90e9a-123">Beenden Sie Visual Studio, und führen Sie den Befehl erneut aus.</span><span class="sxs-lookup"><span data-stu-id="90e9a-123">Exit Visual Studio and run the command again.</span></span>

[!INCLUDE[model 4](../../includes/RP/model4.md)]<span data-ttu-id="90e9a-124"> Im nächsten Tutorial finden Sie Erläuterungen zu den Dateien, die durch den Gerüstbau erstellt werden.</span><span class="sxs-lookup"><span data-stu-id="90e9a-124"> The next tutorial explains the files created by scaffolding.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="90e9a-125">[Zurück: Erste Schritte](xref:tutorials/razor-pages-vsc/razor-pages-start)
[Weiter: Gerüstbau mit Razor-Seiten](xref:tutorials/razor-pages/page)</span><span class="sxs-lookup"><span data-stu-id="90e9a-125">[Previous: Getting Started](xref:tutorials/razor-pages-vsc/razor-pages-start)
[Next: Scaffolded Razor Pages](xref:tutorials/razor-pages/page)</span></span>
