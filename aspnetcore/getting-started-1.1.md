---
title: Erste Schritte mit ASP.NET Core 1.1
author: rick-anderson
description: "Kurztutorial, in dem eine einfache Hello World-App mit ASP.NET Core 1.1 erstellt und ausgeführt wird."
keywords: ASP.NET Core, Tutorial, erste Schritte
ms.author: riande
manager: wpickett
ms.date: 08/07/2017
ms.topic: get-started-article
ms.assetid: 73543e9d-d9d5-47d6-9664-17a9beea6cd3
ms.technology: aspnet
ms.prod: asp.net-core
uid: getting-started-1.1
ms.openlocfilehash: e8fd9ef60ebc1cff6ca0e03000ea50eebff0a9f9
ms.sourcegitcommit: 297ee5d2f3b3b24eb8a2c4a25195c9e2973cb91b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/14/2017
---
# <a name="getting-started-with-aspnet-core-11"></a><span data-ttu-id="e4519-104">Erste Schritte mit ASP.NET Core 1.1</span><span class="sxs-lookup"><span data-stu-id="e4519-104">Getting Started with ASP.NET Core 1.1</span></span>

> [!NOTE]
> <span data-ttu-id="e4519-105">Diese Anweisungen gelten für ASP.NET Core 1.1.</span><span class="sxs-lookup"><span data-stu-id="e4519-105">These instructions are for ASP.NET Core 1.1.</span></span> <span data-ttu-id="e4519-106">Suchen Sie die neueste Version?</span><span class="sxs-lookup"><span data-stu-id="e4519-106">Looking for the latest version?</span></span> <span data-ttu-id="e4519-107">Siehe [die aktuelle Version dieses Tutorials](xref:getting-started).</span><span class="sxs-lookup"><span data-stu-id="e4519-107">See [the current version of this tutorial](xref:getting-started).</span></span>

1. <span data-ttu-id="e4519-108">Installieren Sie den .NET Core **SDK Installer** für SDK 1.0.4 von der [Downloadseite für .NET Core 1.0.5 und 1.1.2 SDK 1.0.4](https://github.com/dotnet/core/blob/master/release-notes/download-archives/1.0.5-download.md).</span><span class="sxs-lookup"><span data-stu-id="e4519-108">Install the .NET Core **SDK Installer** for SDK 1.0.4 from the [.NET Core 1.0.5 & 1.1.2 SDK 1.0.4 downloads page](https://github.com/dotnet/core/blob/master/release-notes/download-archives/1.0.5-download.md).</span></span>

2. <span data-ttu-id="e4519-109">Erstellen Sie einen Ordner für das neue .NET Core-Projekt.</span><span class="sxs-lookup"><span data-stu-id="e4519-109">Create a folder for a new .NET Core project.</span></span>

   <span data-ttu-id="e4519-110">Öffnen Sie unter macOS und Linux ein Terminalfenster.</span><span class="sxs-lookup"><span data-stu-id="e4519-110">On macOS and Linux, open a terminal window.</span></span> <span data-ttu-id="e4519-111">Öffnen Sie unter Windows eine Eingabeaufforderung.</span><span class="sxs-lookup"><span data-stu-id="e4519-111">On Windows, open a command prompt.</span></span>

   ```terminal
   mkdir aspnetcoreapp
   cd aspnetcoreapp
   ```

2. <span data-ttu-id="e4519-112">Wenn Sie eine höhere Version des SDK auf Ihrem Computer installiert haben, erstellen Sie die Datei *global.json*, und wählen Sie das SDK 1.0.4 aus.</span><span class="sxs-lookup"><span data-stu-id="e4519-112">If you have installed a later SDK version on your machine, create a *global.json* file to select the 1.0.4 SDK.</span></span>

   ```json
   {
     "sdk": { "version": "1.0.4" }
   }
   ```

2. <span data-ttu-id="e4519-113">Erstellen Sie ein neues .NET Core-Projekt.</span><span class="sxs-lookup"><span data-stu-id="e4519-113">Create a new .NET Core project.</span></span>

   ```terminal
   dotnet new web
   ```
   
3.  <span data-ttu-id="e4519-114">Stellen Sie die Pakete wieder her.</span><span class="sxs-lookup"><span data-stu-id="e4519-114">Restore the packages.</span></span>

    ```terminal
    dotnet restore
    ```

4. <span data-ttu-id="e4519-115">Führen Sie die App aus.</span><span class="sxs-lookup"><span data-stu-id="e4519-115">Run the app.</span></span>

   <span data-ttu-id="e4519-116">Mit dem Befehl `dotnet run` wird die App bei Bedarf zunächst erstellt.</span><span class="sxs-lookup"><span data-stu-id="e4519-116">The `dotnet run` command builds the app first if needed.</span></span>

   ```terminal
   dotnet run
   ```

5. <span data-ttu-id="e4519-117">Wechseln Sie zu `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="e4519-117">Browse to `http://localhost:5000`</span></span>

<!-- H3 to avoid a single-entry internal TOC -->
### <a name="next-steps"></a><span data-ttu-id="e4519-118">Nächste Schritte</span><span class="sxs-lookup"><span data-stu-id="e4519-118">Next steps</span></span>

<span data-ttu-id="e4519-119">Tutorials für die ersten Schritte finden Sie unter [ASP.NET Core-Tutorials](tutorials/index.md).</span><span class="sxs-lookup"><span data-stu-id="e4519-119">For getting-started tutorials, see [ASP.NET Core Tutorials](tutorials/index.md)</span></span>

<span data-ttu-id="e4519-120">Eine Einführung in ASP.NET Core-Konzepte und -Architektur finden Sie unter [Einführung in ASP.NET Core](index.md) und [ASP.NET Core – Grundlagen](fundamentals/index.md).</span><span class="sxs-lookup"><span data-stu-id="e4519-120">For an introduction to ASP.NET Core concepts and architecture, see [ASP.NET Core Introduction](index.md) and [ASP.NET Core Fundamentals](fundamentals/index.md).</span></span>

<span data-ttu-id="e4519-121">Eine ASP.NET Core-App kann die .NET Core oder .NET Framework-Basisklassenbibliothek und -Laufzeit verwenden.</span><span class="sxs-lookup"><span data-stu-id="e4519-121">An ASP.NET Core app can use the .NET Core or .NET Framework Base Class Library and runtime.</span></span> <span data-ttu-id="e4519-122">Weitere Informationen finden Sie unter [Wahl zwischen .NET Core und .NET Framework](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server).</span><span class="sxs-lookup"><span data-stu-id="e4519-122">For more information, see [Choosing between .NET Core and .NET Framework](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server).</span></span>
