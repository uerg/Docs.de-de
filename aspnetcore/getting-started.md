---
title: Erste Schritte mit ASP.NET Core 2.0
author: rick-anderson
description: "Kurztutorial, in dem eine einfache Hello World-App mit ASP.NET Core erstellt und ausgeführt wird."
keywords: ASP.NET Core, Tutorial, erste Schritte
ms.author: riande
manager: wpickett
ms.date: 08/30/2017
ms.topic: get-started-article
ms.assetid: 73543e9d-d9d5-47d6-9664-17a9beea6cd3
ms.technology: aspnet
ms.prod: asp.net-core
uid: getting-started
ms.openlocfilehash: c81e1328fda6d1652ab937bd580be2342924d241
ms.sourcegitcommit: 9cdbfd0d670d70b9c354216aabee260c52dad5ee
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/12/2017
---
# <a name="getting-started-with-aspnet-core"></a><span data-ttu-id="a931d-104">Erste Schritte mit ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="a931d-104">Getting Started with ASP.NET Core</span></span>

> [!NOTE]
> <span data-ttu-id="a931d-105">Diese Anweisungen gelten für die neueste Version von ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="a931d-105">These instructions are for the latest version of ASP.NET Core.</span></span> <span data-ttu-id="a931d-106">Suchen Sie zum Einstieg eine frühere Version?</span><span class="sxs-lookup"><span data-stu-id="a931d-106">Looking to get started with an earlier version?</span></span> <span data-ttu-id="a931d-107">Siehe die [Version 1.1 dieses Tutorials](xref:getting-started-1.1).</span><span class="sxs-lookup"><span data-stu-id="a931d-107">See [the 1.1 version of this tutorial](xref:getting-started-1.1).</span></span>

1. <span data-ttu-id="a931d-108">Installieren Sie [.NET Core](https://www.microsoft.com/net/core/).</span><span class="sxs-lookup"><span data-stu-id="a931d-108">Install [.NET Core](https://www.microsoft.com/net/core/).</span></span>

2. <span data-ttu-id="a931d-109">Erstellen Sie ein neues .NET Core-Projekt.</span><span class="sxs-lookup"><span data-stu-id="a931d-109">Create a new .NET Core project.</span></span>

   <span data-ttu-id="a931d-110">Öffnen Sie unter macOS und Linux ein Terminalfenster.</span><span class="sxs-lookup"><span data-stu-id="a931d-110">On macOS and Linux, open a terminal window.</span></span> <span data-ttu-id="a931d-111">Öffnen Sie unter Windows eine Eingabeaufforderung.</span><span class="sxs-lookup"><span data-stu-id="a931d-111">On Windows, open a command prompt.</span></span>

    ```terminal
    dotnet new razor -o aspnetcoreapp
    ```
    
4. <span data-ttu-id="a931d-112">Führen Sie die App aus.</span><span class="sxs-lookup"><span data-stu-id="a931d-112">Run the app.</span></span>

    <span data-ttu-id="a931d-113">Verwenden Sie die folgenden Befehle, um die App auszuführen:</span><span class="sxs-lookup"><span data-stu-id="a931d-113">Use the following commands to run the app:</span></span>

    ```terminal
    cd aspnetcoreapp
    dotnet run
    ```

5. <span data-ttu-id="a931d-114">Wechseln Sie dann zu [http://localhost:5000](http://localhost:5000).</span><span class="sxs-lookup"><span data-stu-id="a931d-114">Browse to [http://localhost:5000](http://localhost:5000)</span></span>

6. <span data-ttu-id="a931d-115">Öffnen Sie *Pages/About.cshtml*, und verändern Sie die Seite so, dass sie die Meldung „Hallo Welt!“ anzeigt.</span><span class="sxs-lookup"><span data-stu-id="a931d-115">Open *Pages/About.cshtml* and modify the page to display the message "Hello, world!</span></span> <span data-ttu-id="a931d-116">Die Zeit auf dem Server ist @DateTime.Now" :</span><span class="sxs-lookup"><span data-stu-id="a931d-116">The time on the server is @DateTime.Now":</span></span>

    <span data-ttu-id="a931d-117">[!code-html[Main](getting-started/sample/getting-started/about.cshtml?highlight=9&range=1-9)]</span><span class="sxs-lookup"><span data-stu-id="a931d-117">[!code-html[Main](getting-started/sample/getting-started/about.cshtml?highlight=9&range=1-9)]</span></span>

7. <span data-ttu-id="a931d-118">Navigieren Sie zu [http://localhost:5000/About](http://localhost:5000/About), und überprüfen Sie die Änderungen.</span><span class="sxs-lookup"><span data-stu-id="a931d-118">Browse to [http://localhost:5000/About](http://localhost:5000/About) and verify the changes.</span></span>

### <a name="next-steps"></a><span data-ttu-id="a931d-119">Nächste Schritte</span><span class="sxs-lookup"><span data-stu-id="a931d-119">Next steps</span></span>

<span data-ttu-id="a931d-120">Tutorials für die ersten Schritte finden Sie unter [ASP.NET Core-Tutorials](tutorials/index.md).</span><span class="sxs-lookup"><span data-stu-id="a931d-120">For getting-started tutorials, see [ASP.NET Core Tutorials](tutorials/index.md)</span></span>

<span data-ttu-id="a931d-121">Eine Einführung in ASP.NET Core-Konzepte und -Architektur finden Sie unter [Einführung in ASP.NET Core](index.md) und [ASP.NET Core – Grundlagen](fundamentals/index.md).</span><span class="sxs-lookup"><span data-stu-id="a931d-121">For an introduction to ASP.NET Core concepts and architecture, see [ASP.NET Core Introduction](index.md) and [ASP.NET Core Fundamentals](fundamentals/index.md).</span></span>

<span data-ttu-id="a931d-122">Eine ASP.NET Core-App kann die .NET Core oder .NET Framework-Basisklassenbibliothek und -Laufzeit verwenden.</span><span class="sxs-lookup"><span data-stu-id="a931d-122">An ASP.NET Core app can use the .NET Core or .NET Framework Base Class Library and runtime.</span></span> <span data-ttu-id="a931d-123">Weitere Informationen finden Sie unter [Wahl zwischen .NET Core und .NET Framework](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server).</span><span class="sxs-lookup"><span data-stu-id="a931d-123">For more information, see [Choosing between .NET Core and .NET Framework](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server).</span></span>
