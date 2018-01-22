---
title: Erste Schritte mit ASP.NET Core 1.1
author: rick-anderson
description: "Kurztutorial, in dem eine einfache Hello World-App mit ASP.NET Core 1.1 erstellt und ausgeführt wird."
ms.author: riande
manager: wpickett
ms.date: 08/07/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: getting-started-1.1
ms.openlocfilehash: 7f178618508e1a1e9c49d8ace619b9f942998dae
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/19/2018
---
# <a name="getting-started-with-aspnet-core-11"></a><span data-ttu-id="75733-103">Erste Schritte mit ASP.NET Core 1.1</span><span class="sxs-lookup"><span data-stu-id="75733-103">Getting Started with ASP.NET Core 1.1</span></span>

> [!NOTE]
> <span data-ttu-id="75733-104">Diese Anweisungen gelten für ASP.NET Core 1.1.</span><span class="sxs-lookup"><span data-stu-id="75733-104">These instructions are for ASP.NET Core 1.1.</span></span> <span data-ttu-id="75733-105">Suchen Sie die neueste Version?</span><span class="sxs-lookup"><span data-stu-id="75733-105">Looking for the latest version?</span></span> <span data-ttu-id="75733-106">Siehe [die aktuelle Version dieses Tutorials](xref:getting-started).</span><span class="sxs-lookup"><span data-stu-id="75733-106">See [the current version of this tutorial](xref:getting-started).</span></span>

1. <span data-ttu-id="75733-107">Installieren Sie den .NET Core **SDK Installer** für SDK 1.0.4 von der [Downloadseite für .NET Core 1.0.5 und 1.1.2 SDK 1.0.4](https://github.com/dotnet/core/blob/master/release-notes/download-archives/1.0.5-download.md).</span><span class="sxs-lookup"><span data-stu-id="75733-107">Install the .NET Core **SDK Installer** for SDK 1.0.4 from the [.NET Core 1.0.5 & 1.1.2 SDK 1.0.4 downloads page](https://github.com/dotnet/core/blob/master/release-notes/download-archives/1.0.5-download.md).</span></span>

2. <span data-ttu-id="75733-108">Erstellen Sie einen Ordner für das neue .NET Core-Projekt.</span><span class="sxs-lookup"><span data-stu-id="75733-108">Create a folder for a new .NET Core project.</span></span>

   <span data-ttu-id="75733-109">Öffnen Sie unter macOS und Linux ein Terminalfenster.</span><span class="sxs-lookup"><span data-stu-id="75733-109">On macOS and Linux, open a terminal window.</span></span> <span data-ttu-id="75733-110">Öffnen Sie unter Windows eine Eingabeaufforderung.</span><span class="sxs-lookup"><span data-stu-id="75733-110">On Windows, open a command prompt.</span></span>

   ```terminal
   mkdir aspnetcoreapp
   cd aspnetcoreapp
   ```

2. <span data-ttu-id="75733-111">Wenn Sie eine höhere Version des SDK auf Ihrem Computer installiert haben, erstellen Sie die Datei *global.json*, und wählen Sie das SDK 1.0.4 aus.</span><span class="sxs-lookup"><span data-stu-id="75733-111">If you have installed a later SDK version on your machine, create a *global.json* file to select the 1.0.4 SDK.</span></span>

   ```json
   {
     "sdk": { "version": "1.0.4" }
   }
   ```

2. <span data-ttu-id="75733-112">Erstellen Sie ein neues .NET Core-Projekt.</span><span class="sxs-lookup"><span data-stu-id="75733-112">Create a new .NET Core project.</span></span>

   ```terminal
   dotnet new web
   ```
   
3.  <span data-ttu-id="75733-113">Stellen Sie die Pakete wieder her.</span><span class="sxs-lookup"><span data-stu-id="75733-113">Restore the packages.</span></span>

    ```terminal
    dotnet restore
    ```

4. <span data-ttu-id="75733-114">Führen Sie die App aus.</span><span class="sxs-lookup"><span data-stu-id="75733-114">Run the app.</span></span>

   <span data-ttu-id="75733-115">Mit dem Befehl `dotnet run` wird die App bei Bedarf zunächst erstellt.</span><span class="sxs-lookup"><span data-stu-id="75733-115">The `dotnet run` command builds the app first if needed.</span></span>

   ```terminal
   dotnet run
   ```

5. <span data-ttu-id="75733-116">Wechseln Sie zu `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="75733-116">Browse to `http://localhost:5000`</span></span>

<!-- H3 to avoid a single-entry internal TOC -->
### <a name="next-steps"></a><span data-ttu-id="75733-117">Nächste Schritte</span><span class="sxs-lookup"><span data-stu-id="75733-117">Next steps</span></span>

<span data-ttu-id="75733-118">Tutorials für die ersten Schritte finden Sie unter [ASP.NET Core-Tutorials](tutorials/index.md).</span><span class="sxs-lookup"><span data-stu-id="75733-118">For getting-started tutorials, see [ASP.NET Core Tutorials](tutorials/index.md)</span></span>

<span data-ttu-id="75733-119">Eine Einführung in ASP.NET Core-Konzepte und -Architektur finden Sie unter [Einführung in ASP.NET Core](index.md) und [ASP.NET Core – Grundlagen](fundamentals/index.md).</span><span class="sxs-lookup"><span data-stu-id="75733-119">For an introduction to ASP.NET Core concepts and architecture, see [ASP.NET Core Introduction](index.md) and [ASP.NET Core Fundamentals](fundamentals/index.md).</span></span>

<span data-ttu-id="75733-120">Eine ASP.NET Core-App kann die .NET Core oder .NET Framework-Basisklassenbibliothek und -Laufzeit verwenden.</span><span class="sxs-lookup"><span data-stu-id="75733-120">An ASP.NET Core app can use the .NET Core or .NET Framework Base Class Library and runtime.</span></span> <span data-ttu-id="75733-121">Weitere Informationen finden Sie unter [Wahl zwischen .NET Core und .NET Framework](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server).</span><span class="sxs-lookup"><span data-stu-id="75733-121">For more information, see [Choosing between .NET Core and .NET Framework](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server).</span></span>
