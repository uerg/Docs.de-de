---
title: Hinzufügen eines Modells zu einer Razor-Seiten-App in ASP.NET Core mit Visual Studio für Mac
author: rick-anderson
description: Erfahren Sie, wie Sie ein Modell mithilfe von Visual Studio für Mac zu einer Razor-Seiten-App in ASP.NET Core hinzufügen.
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.date: 08/27/2017
uid: tutorials/razor-pages-mac/model
ms.openlocfilehash: 3ca6c9b9988b8335116b7248c6c4a89997d02b14
ms.sourcegitcommit: b8a2f14bf8dd346d7592977642b610bbcb0b0757
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2018
ms.locfileid: "38186757"
---
# <a name="add-a-model-to-an-aspnet-core-razor-pages-app-with-visual-studio-for-mac"></a><span data-ttu-id="ca9b6-103">Hinzufügen eines Modells zu einer Razor-Seiten-App in ASP.NET Core mit Visual Studio für Mac</span><span class="sxs-lookup"><span data-stu-id="ca9b6-103">Add a model to an ASP.NET Core Razor Pages app with Visual Studio for Mac</span></span>

[!INCLUDE [model1](../../includes/RP/model1.md)]

## <a name="add-a-data-model"></a><span data-ttu-id="ca9b6-104">Hinzufügen eines Datenmodells</span><span class="sxs-lookup"><span data-stu-id="ca9b6-104">Add a data model</span></span>

* <span data-ttu-id="ca9b6-105">Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf das Projekt**RazorPagesMovie**, und klicken Sie dann auf **Hinzufügen** > **Neuer Ordner**.</span><span class="sxs-lookup"><span data-stu-id="ca9b6-105">In Solution Explorer, right-click the **RazorPagesMovie** project, and then select **Add** > **New Folder**.</span></span> <span data-ttu-id="ca9b6-106">Geben Sie dem Ordner den Namen *Modelle*.</span><span class="sxs-lookup"><span data-stu-id="ca9b6-106">Name the folder *Models*.</span></span>
* <span data-ttu-id="ca9b6-107">Klicken Sie mit der rechten Maustaste auf den Ordner *Modelle*, und klicken Sie auf **Hinzufügen** > **Neue Datei**.</span><span class="sxs-lookup"><span data-stu-id="ca9b6-107">Right-click the *Models* folder, and then select **Add** > **New File**.</span></span>
* <span data-ttu-id="ca9b6-108">Führen Sie im Dialogfeld **Neue Datei** folgende Aktionen aus:</span><span class="sxs-lookup"><span data-stu-id="ca9b6-108">In the **New File** dialog:</span></span>

  * <span data-ttu-id="ca9b6-109">Klicken Sie im linken Bereich auf **Allgemein**.</span><span class="sxs-lookup"><span data-stu-id="ca9b6-109">Select **General** in the left pane.</span></span>
  * <span data-ttu-id="ca9b6-110">Klicken Sie im mittleren Bereich auf **Leere Klasse**.</span><span class="sxs-lookup"><span data-stu-id="ca9b6-110">Select **Empty Class** in the center pain.</span></span>
  * <span data-ttu-id="ca9b6-111">Geben Sie der Klasse den Namen **Film**, und klicken Sie auf **Neu**.</span><span class="sxs-lookup"><span data-stu-id="ca9b6-111">Name the class **Movie** and select **New**.</span></span>

[!INCLUDE [model 2](../../includes/RP/model2.md)]

[!INCLUDE [model 2a](../../includes/RP/model2a.md)]

[!code-csharp[](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Startup.cs?name=snippet_ConfigureServices2&highlight=3-6)]

<span data-ttu-id="ca9b6-112">Klicken Sie mit der rechten Maustaste auf eine rote Wellenlinie, z.B. `MovieContext` in der Zeile `services.AddDbContext<MovieContext>(options =>`.</span><span class="sxs-lookup"><span data-stu-id="ca9b6-112">Right click on a red squiggly line, for example `MovieContext` in the line `services.AddDbContext<MovieContext>(options =>`.</span></span> <span data-ttu-id="ca9b6-113">Klicken Sie auf **Quick Fix > using RazorPagesMovie.Models;** (Schnelle Problembehebung > Verwenden von RazorPagesMovie.Models;).</span><span class="sxs-lookup"><span data-stu-id="ca9b6-113">Select **Quick Fix > using RazorPagesMovie.Models;**.</span></span> <span data-ttu-id="ca9b6-114">Visual Studio fügt die „using“-Anweisung hinzu.</span><span class="sxs-lookup"><span data-stu-id="ca9b6-114">Visual studio adds the using statement.</span></span>

<span data-ttu-id="ca9b6-115">Erstellen Sie das Projekt, um sicherzustellen, dass keine Fehler vorliegen.</span><span class="sxs-lookup"><span data-stu-id="ca9b6-115">Build the project to verify you don't have any errors.</span></span>

![Seite „Erstellen“](model/red.png)

### <a name="entity-framework-core-nuget-packages-for-migrations"></a><span data-ttu-id="ca9b6-117">NuGet-Pakete für Migrationen in Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="ca9b6-117">Entity Framework Core NuGet packages for migrations</span></span>

<span data-ttu-id="ca9b6-118">Die EF-Tools für die Befehlszeilenschnittstelle (CLI) werden unter [Microsoft.EntityFrameworkCore.Tools.DotNet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools.DotNet) bereitgestellt.</span><span class="sxs-lookup"><span data-stu-id="ca9b6-118">The EF tools for the command-line interface (CLI) are provided in [Microsoft.EntityFrameworkCore.Tools.DotNet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools.DotNet).</span></span> <span data-ttu-id="ca9b6-119">Klicken Sie auf den Link [Microsoft.EntityFrameworkCore.Tools.DotNet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools.DotNet), um die Versionsnummer abzufragen, die verwendet werden soll.</span><span class="sxs-lookup"><span data-stu-id="ca9b6-119">Click on the [Microsoft.EntityFrameworkCore.Tools.DotNet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools.DotNet) link to get the version number to use.</span></span> <span data-ttu-id="ca9b6-120">Fügen Sie das Paket zum Installieren zu der `DotNetCliToolReference`-Sammlung in der *CSPROJ*-Datei hinzu.</span><span class="sxs-lookup"><span data-stu-id="ca9b6-120">To install this package, add it to the `DotNetCliToolReference` collection in the *.csproj* file.</span></span> <span data-ttu-id="ca9b6-121">**Hinweis:** Sie müssen dieses Paket installieren, indem Sie die *CSPROJ*-Datei bearbeiten. Sie können den `install-package`-Befehl oder die Paket-Manager-GUI nicht verwenden.</span><span class="sxs-lookup"><span data-stu-id="ca9b6-121">**Note:** You have to install this package by editing the *.csproj* file; you can't use the `install-package` command or the package manager GUI.</span></span>

<span data-ttu-id="ca9b6-122">So bearbeiten Sie eine *CSPROJ*-Datei:</span><span class="sxs-lookup"><span data-stu-id="ca9b6-122">To edit a *.csproj* file:</span></span>

* <span data-ttu-id="ca9b6-123">Klicken Sie auf **Datei**  > **Öffnen**, und wählen Sie dann die *CSPROJ*-Datei aus.</span><span class="sxs-lookup"><span data-stu-id="ca9b6-123">Select **File** > **Open**, and then select the *.csproj* file.</span></span>
* <span data-ttu-id="ca9b6-124">Klicken Sie auf **Optionen**.</span><span class="sxs-lookup"><span data-stu-id="ca9b6-124">Select **Options**.</span></span>
* <span data-ttu-id="ca9b6-125">Ändern Sie **Öffnen mit** zu **Quellcode-Editor**.</span><span class="sxs-lookup"><span data-stu-id="ca9b6-125">Change **Open with** to **Source Code Editor**.</span></span>

![Bearbeiten der CSPROJ-Datei](model/csproj.png)

<span data-ttu-id="ca9b6-127">Fügen Sie dann der zweiten **\<ItemGroup>** den Toolverweis `Microsoft.EntityFrameworkCore.Tools.DotNet` hinzu:</span><span class="sxs-lookup"><span data-stu-id="ca9b6-127">Add the `Microsoft.EntityFrameworkCore.Tools.DotNet` tool reference to the second **\<ItemGroup>**.:</span></span>

[!code-xml[](../../tutorials/razor-pages/razor-pages-start/snapshot_cli_sample/RazorPagesMovie/RazorPagesMovie.cli.csproj?highlight=10)]

<span data-ttu-id="ca9b6-128">Die Versionsnummern, die im folgenden Code angezeigt werden, waren zu dem Zeitpunkt korrekt, als der Code geschrieben wurde.</span><span class="sxs-lookup"><span data-stu-id="ca9b6-128">The version numbers shown in the following code were correct at the time of writing.</span></span>

[!INCLUDE [model3](../../includes/RP/model3.md)]

[!INCLUDE [model 4x](../../includes/RP/model4x.md)]

[!INCLUDE [model 4 exit](../../includes/RP/model4exit.md)]

[!INCLUDE [model 4](../../includes/RP/model4.md)]

### <a name="add-the-pagesmovies-files-to-the-project"></a><span data-ttu-id="ca9b6-129">Hinzufügen der Seiten/Filmdateien zum Projekt</span><span class="sxs-lookup"><span data-stu-id="ca9b6-129">Add the Pages/Movies files to the project</span></span>

* <span data-ttu-id="ca9b6-130">Klicken Sie in Visual Studio mit der rechten Maustaste auf den Ordner *Seiten*, und klicken Sie auf **Hinzufügen > Vorhandenen Ordner hinzufügen**.</span><span class="sxs-lookup"><span data-stu-id="ca9b6-130">In Visual Studio, Right-click the *Pages* folder and select **Add > Add existing Folder**.</span></span>
* <span data-ttu-id="ca9b6-131">Wählen Sie den Ordner *Filme* aus.</span><span class="sxs-lookup"><span data-stu-id="ca9b6-131">Select the *Movies* folder.</span></span>
* <span data-ttu-id="ca9b6-132">Klicken Sie im Dialogfeld *Dateien zum Einschließen in das Projekt auswählen* auf **Alle einschließen**.</span><span class="sxs-lookup"><span data-stu-id="ca9b6-132">In the *Choose files to include in the project* dialog, select **Include All**.</span></span>

<span data-ttu-id="ca9b6-133">Im nächsten Tutorial finden Sie Erläuterungen zu den Dateien, die durch den Gerüstbau erstellt wurden.</span><span class="sxs-lookup"><span data-stu-id="ca9b6-133">The next tutorial explains the files created by scaffolding.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="ca9b6-134">[Zurück: Erste Schritte](xref:tutorials/razor-pages-mac/razor-pages-start)
> [Weiter: Gerüstbau mit Razor-Seiten](xref:tutorials/razor-pages-mac/page)</span><span class="sxs-lookup"><span data-stu-id="ca9b6-134">[Previous: Get Started](xref:tutorials/razor-pages-mac/razor-pages-start)
[Next: Scaffolded Razor Pages](xref:tutorials/razor-pages-mac/page)</span></span>
