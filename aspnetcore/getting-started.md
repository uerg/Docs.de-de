---
title: Erste Schritte mit ASP.NET Core 2.0
author: rick-anderson
description: "Kurztutorial, in dem eine einfache Hello World-App mit ASP.NET Core erstellt und ausgeführt wird."
keywords: ASP.NET Core, Tutorial, erste Schritte
ms.author: riande
manager: wpickett
ms.date: 08/07/2017
ms.topic: get-started-article
ms.assetid: 73543e9d-d9d5-47d6-9664-17a9beea6cd3
ms.technology: aspnet
ms.prod: asp.net-core
uid: getting-started
ms.openlocfilehash: 3399df3958093da9117b013736b1cb370fd6beb2
ms.sourcegitcommit: 297ee5d2f3b3b24eb8a2c4a25195c9e2973cb91b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/14/2017
---
# <a name="getting-started-with-aspnet-core"></a><span data-ttu-id="e3acc-104">Erste Schritte mit ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="e3acc-104">Getting Started with ASP.NET Core</span></span>

> [!NOTE]
> <span data-ttu-id="e3acc-105">Diese Anweisungen gelten für die neueste Version von ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="e3acc-105">These instructions are for the latest version of ASP.NET Core.</span></span> <span data-ttu-id="e3acc-106">Suchen Sie zum Einstieg eine frühere Version?</span><span class="sxs-lookup"><span data-stu-id="e3acc-106">Looking to get started with an earlier version?</span></span> <span data-ttu-id="e3acc-107">Siehe die [Version 1.1 dieses Tutorials](xref:getting-started-1.1).</span><span class="sxs-lookup"><span data-stu-id="e3acc-107">See [the 1.1 version of this tutorial](xref:getting-started-1.1).</span></span>

1. <span data-ttu-id="e3acc-108">Installieren Sie [.NET Core](https://microsoft.com/net/core/).</span><span class="sxs-lookup"><span data-stu-id="e3acc-108">Install [.NET Core](https://microsoft.com/net/core/).</span></span>

2. <span data-ttu-id="e3acc-109">Erstellen Sie ein neues .NET Core-Projekt.</span><span class="sxs-lookup"><span data-stu-id="e3acc-109">Create a new .NET Core project.</span></span>

   <span data-ttu-id="e3acc-110">Öffnen Sie unter macOS und Linux ein Terminalfenster.</span><span class="sxs-lookup"><span data-stu-id="e3acc-110">On macOS and Linux, open a terminal window.</span></span> <span data-ttu-id="e3acc-111">Öffnen Sie unter Windows eine Eingabeaufforderung.</span><span class="sxs-lookup"><span data-stu-id="e3acc-111">On Windows, open a command prompt.</span></span>

   ```terminal
   mkdir aspnetcoreapp
   cd aspnetcoreapp
   dotnet new web
   ```
    
4. <span data-ttu-id="e3acc-112">Führen Sie die App aus.</span><span class="sxs-lookup"><span data-stu-id="e3acc-112">Run the app.</span></span>

   <span data-ttu-id="e3acc-113">Mit dem Befehl `dotnet run` wird die App bei Bedarf zunächst erstellt.</span><span class="sxs-lookup"><span data-stu-id="e3acc-113">The `dotnet run` command builds the app first if needed.</span></span>

   ```terminal
   dotnet run
   ```

5. <span data-ttu-id="e3acc-114">Wechseln Sie zu `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="e3acc-114">Browse to `http://localhost:5000`</span></span>

### <a name="next-steps"></a><span data-ttu-id="e3acc-115">Nächste Schritte</span><span class="sxs-lookup"><span data-stu-id="e3acc-115">Next steps</span></span>

<span data-ttu-id="e3acc-116">Tutorials für die ersten Schritte finden Sie unter [ASP.NET Core-Tutorials](tutorials/index.md).</span><span class="sxs-lookup"><span data-stu-id="e3acc-116">For getting-started tutorials, see [ASP.NET Core Tutorials](tutorials/index.md)</span></span>

<span data-ttu-id="e3acc-117">Eine Einführung in ASP.NET Core-Konzepte und -Architektur finden Sie unter [Einführung in ASP.NET Core](index.md) und [ASP.NET Core – Grundlagen](fundamentals/index.md).</span><span class="sxs-lookup"><span data-stu-id="e3acc-117">For an introduction to ASP.NET Core concepts and architecture, see [ASP.NET Core Introduction](index.md) and [ASP.NET Core Fundamentals](fundamentals/index.md).</span></span>

<span data-ttu-id="e3acc-118">Eine ASP.NET Core-App kann die .NET Core oder .NET Framework-Basisklassenbibliothek und -Laufzeit verwenden.</span><span class="sxs-lookup"><span data-stu-id="e3acc-118">An ASP.NET Core app can use the .NET Core or .NET Framework Base Class Library and runtime.</span></span> <span data-ttu-id="e3acc-119">Weitere Informationen finden Sie unter [Wahl zwischen .NET Core und .NET Framework](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server).</span><span class="sxs-lookup"><span data-stu-id="e3acc-119">For more information, see [Choosing between .NET Core and .NET Framework](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server).</span></span>
