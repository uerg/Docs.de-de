---
title: Erste Schritte mit ASP.NET Core 1.1
author: rick-anderson
description: "Kurztutorial, in dem eine einfache Hello World-App mit ASP.NET Core 1.1 erstellt und ausgeführt wird."
manager: wpickett
ms.author: riande
ms.date: 08/07/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: getting-started-1.1
ms.openlocfilehash: 895e91efbba931923540e4cd182862cbc1851585
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2018
---
# <a name="getting-started-with-aspnet-core-11"></a><span data-ttu-id="5530b-103">Erste Schritte mit ASP.NET Core 1.1</span><span class="sxs-lookup"><span data-stu-id="5530b-103">Getting Started with ASP.NET Core 1.1</span></span>

> [!NOTE]
> <span data-ttu-id="5530b-104">Diese Anweisungen gelten für ASP.NET Core 1.1.</span><span class="sxs-lookup"><span data-stu-id="5530b-104">These instructions are for ASP.NET Core 1.1.</span></span> <span data-ttu-id="5530b-105">Suchen Sie die neueste Version?</span><span class="sxs-lookup"><span data-stu-id="5530b-105">Looking for the latest version?</span></span> <span data-ttu-id="5530b-106">Siehe [die aktuelle Version dieses Tutorials](xref:getting-started).</span><span class="sxs-lookup"><span data-stu-id="5530b-106">See [the current version of this tutorial](xref:getting-started).</span></span>

1. <span data-ttu-id="5530b-107">Installieren Sie den .NET Core **SDK Installer** für SDK 1.0.4 von der [Downloadseite für .NET Core 1.0.5 und 1.1.2 SDK 1.0.4](https://github.com/dotnet/core/blob/master/release-notes/download-archives/1.0.5-download.md).</span><span class="sxs-lookup"><span data-stu-id="5530b-107">Install the .NET Core **SDK Installer** for SDK 1.0.4 from the [.NET Core 1.0.5 & 1.1.2 SDK 1.0.4 downloads page](https://github.com/dotnet/core/blob/master/release-notes/download-archives/1.0.5-download.md).</span></span>

2. <span data-ttu-id="5530b-108">Erstellen Sie einen Ordner für das neue .NET Core-Projekt.</span><span class="sxs-lookup"><span data-stu-id="5530b-108">Create a folder for a new .NET Core project.</span></span>

   <span data-ttu-id="5530b-109">Öffnen Sie unter macOS und Linux ein Terminalfenster.</span><span class="sxs-lookup"><span data-stu-id="5530b-109">On macOS and Linux, open a terminal window.</span></span> <span data-ttu-id="5530b-110">Öffnen Sie unter Windows eine Eingabeaufforderung.</span><span class="sxs-lookup"><span data-stu-id="5530b-110">On Windows, open a command prompt.</span></span>

   ```terminal
   mkdir aspnetcoreapp
   cd aspnetcoreapp
   ```

2. <span data-ttu-id="5530b-111">Wenn Sie eine höhere Version des SDK auf Ihrem Computer installiert haben, erstellen Sie die Datei *global.json*, und wählen Sie das SDK 1.0.4 aus.</span><span class="sxs-lookup"><span data-stu-id="5530b-111">If you have installed a later SDK version on your machine, create a *global.json* file to select the 1.0.4 SDK.</span></span>

   ```json
   {
     "sdk": { "version": "1.0.4" }
   }
   ```

2. <span data-ttu-id="5530b-112">Erstellen Sie ein neues .NET Core-Projekt.</span><span class="sxs-lookup"><span data-stu-id="5530b-112">Create a new .NET Core project.</span></span>

   ```terminal
   dotnet new web
   ```
   
3.  <span data-ttu-id="5530b-113">Stellen Sie die Pakete wieder her.</span><span class="sxs-lookup"><span data-stu-id="5530b-113">Restore the packages.</span></span>

    ```terminal
    dotnet restore
    ```

4. <span data-ttu-id="5530b-114">Führen Sie die App aus.</span><span class="sxs-lookup"><span data-stu-id="5530b-114">Run the app.</span></span>

   <span data-ttu-id="5530b-115">Mit dem Befehl `dotnet run` wird die App bei Bedarf zunächst erstellt.</span><span class="sxs-lookup"><span data-stu-id="5530b-115">The `dotnet run` command builds the app first if needed.</span></span>

   ```terminal
   dotnet run
   ```

5. <span data-ttu-id="5530b-116">Wechseln Sie zu `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="5530b-116">Browse to `http://localhost:5000`</span></span>

<!-- H3 to avoid a single-entry internal TOC -->
### <a name="next-steps"></a><span data-ttu-id="5530b-117">Nächste Schritte</span><span class="sxs-lookup"><span data-stu-id="5530b-117">Next steps</span></span>

<span data-ttu-id="5530b-118">Tutorials für die ersten Schritte finden Sie unter [ASP.NET Core-Tutorials](tutorials/index.md).</span><span class="sxs-lookup"><span data-stu-id="5530b-118">For getting-started tutorials, see [ASP.NET Core Tutorials](tutorials/index.md)</span></span>

<span data-ttu-id="5530b-119">Eine Einführung in ASP.NET Core-Konzepte und -Architektur finden Sie unter [Einführung in ASP.NET Core](index.md) und [ASP.NET Core – Grundlagen](fundamentals/index.md).</span><span class="sxs-lookup"><span data-stu-id="5530b-119">For an introduction to ASP.NET Core concepts and architecture, see [ASP.NET Core Introduction](index.md) and [ASP.NET Core Fundamentals](fundamentals/index.md).</span></span>

<span data-ttu-id="5530b-120">Eine ASP.NET Core-App kann die .NET Core oder .NET Framework-Basisklassenbibliothek und -Laufzeit verwenden.</span><span class="sxs-lookup"><span data-stu-id="5530b-120">An ASP.NET Core app can use the .NET Core or .NET Framework Base Class Library and runtime.</span></span> <span data-ttu-id="5530b-121">Weitere Informationen finden Sie unter [Wahl zwischen .NET Core und .NET Framework](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server).</span><span class="sxs-lookup"><span data-stu-id="5530b-121">For more information, see [Choosing between .NET Core and .NET Framework](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server).</span></span>
