---
title: Erste Schritte mit ASP.NET Core 2.0
author: rick-anderson
description: "Kurztutorial, in dem eine einfache Hello World-App mit ASP.NET Core erstellt und ausgeführt wird."
manager: wpickett
ms.author: riande
ms.date: 10/18/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: getting-started
ms.openlocfilehash: eb1fd748029743ca6991927cc95b612ed1975338
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2018
---
# <a name="get-started-with-aspnet-core"></a><span data-ttu-id="96df4-103">Erste Schritte mit ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="96df4-103">Get Started with ASP.NET Core</span></span>

> [!NOTE]
> <span data-ttu-id="96df4-104">Diese Anweisungen gelten für die neueste Version von ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="96df4-104">These instructions are for the latest version of ASP.NET Core.</span></span> <span data-ttu-id="96df4-105">Suchen Sie zum Einstieg eine frühere Version?</span><span class="sxs-lookup"><span data-stu-id="96df4-105">Looking to get started with an earlier version?</span></span> <span data-ttu-id="96df4-106">Siehe die [Version 1.1 dieses Tutorials](xref:getting-started-1.1).</span><span class="sxs-lookup"><span data-stu-id="96df4-106">See [the 1.1 version of this tutorial](xref:getting-started-1.1).</span></span>

1. <span data-ttu-id="96df4-107">Installieren Sie [.NET Core](https://www.microsoft.com/net/core/).</span><span class="sxs-lookup"><span data-stu-id="96df4-107">Install [.NET Core](https://www.microsoft.com/net/core/).</span></span>

2. <span data-ttu-id="96df4-108">Erstellen Sie ein neues .NET Core-Projekt.</span><span class="sxs-lookup"><span data-stu-id="96df4-108">Create a new .NET Core project.</span></span>

   <span data-ttu-id="96df4-109">Öffnen Sie unter macOS und Linux ein Terminalfenster.</span><span class="sxs-lookup"><span data-stu-id="96df4-109">On macOS and Linux, open a terminal window.</span></span> <span data-ttu-id="96df4-110">Öffnen Sie unter Windows eine Eingabeaufforderung.</span><span class="sxs-lookup"><span data-stu-id="96df4-110">On Windows, open a command prompt.</span></span> <span data-ttu-id="96df4-111">Geben Sie folgenden Befehl ein:</span><span class="sxs-lookup"><span data-stu-id="96df4-111">Enter the following command:</span></span>

    ```terminal
    dotnet new razor -o aspnetcoreapp
    ```
    
4. <span data-ttu-id="96df4-112">Führen Sie die App aus.</span><span class="sxs-lookup"><span data-stu-id="96df4-112">Run the app.</span></span>

    <span data-ttu-id="96df4-113">Verwenden Sie die folgenden Befehle, um die App auszuführen:</span><span class="sxs-lookup"><span data-stu-id="96df4-113">Use the following commands to run the app:</span></span>

    ```terminal
    cd aspnetcoreapp
    dotnet run
    ```

5. <span data-ttu-id="96df4-114">Wechseln Sie dann zu [http://localhost:5000](http://localhost:5000).</span><span class="sxs-lookup"><span data-stu-id="96df4-114">Browse to [http://localhost:5000](http://localhost:5000)</span></span>

6. <span data-ttu-id="96df4-115">Öffnen Sie *Pages/About.cshtml*, und verändern Sie die Seite so, dass sie die Meldung „Hallo Welt!“ anzeigt.</span><span class="sxs-lookup"><span data-stu-id="96df4-115">Open *Pages/About.cshtml* and modify the page to display the message "Hello, world!</span></span> <span data-ttu-id="96df4-116">Die Zeit auf dem Server beträgt @DateTime.Now ":</span><span class="sxs-lookup"><span data-stu-id="96df4-116">The time on the server is @DateTime.Now ":</span></span>

    [!code-html[Main](getting-started/sample/getting-started/about.cshtml?highlight=9&range=1-9)]

7. <span data-ttu-id="96df4-117">Navigieren Sie zu [http://localhost:5000/About](http://localhost:5000/About), und überprüfen Sie die Änderungen.</span><span class="sxs-lookup"><span data-stu-id="96df4-117">Browse to [http://localhost:5000/About](http://localhost:5000/About) and verify the changes.</span></span>

### <a name="next-steps"></a><span data-ttu-id="96df4-118">Nächste Schritte</span><span class="sxs-lookup"><span data-stu-id="96df4-118">Next steps</span></span>

<span data-ttu-id="96df4-119">Tutorials für die ersten Schritte finden Sie unter [ASP.NET Core-Tutorials](tutorials/index.md).</span><span class="sxs-lookup"><span data-stu-id="96df4-119">For getting-started tutorials, see [ASP.NET Core Tutorials](tutorials/index.md)</span></span>

<span data-ttu-id="96df4-120">Eine Einführung in ASP.NET Core-Konzepte und -Architektur finden Sie unter [Einführung in ASP.NET Core](index.md) und [ASP.NET Core – Grundlagen](fundamentals/index.md).</span><span class="sxs-lookup"><span data-stu-id="96df4-120">For an introduction to ASP.NET Core concepts and architecture, see [ASP.NET Core Introduction](index.md) and [ASP.NET Core Fundamentals](fundamentals/index.md).</span></span>

<span data-ttu-id="96df4-121">Eine ASP.NET Core-App kann die .NET Core oder .NET Framework-Basisklassenbibliothek und -Laufzeit verwenden.</span><span class="sxs-lookup"><span data-stu-id="96df4-121">An ASP.NET Core app can use the .NET Core or .NET Framework Base Class Library and runtime.</span></span> <span data-ttu-id="96df4-122">Weitere Informationen finden Sie unter [Wahl zwischen .NET Core und .NET Framework](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server).</span><span class="sxs-lookup"><span data-stu-id="96df4-122">For more information, see [Choosing between .NET Core and .NET Framework](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server).</span></span>
