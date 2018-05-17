---
title: Erste Schritte mit ASP.NET Core
author: rick-anderson
description: Kurztutorial, in dem eine einfache Hello World-App mit ASP.NET Core erstellt und ausgeführt wird.
manager: wpickett
ms.author: riande
ms.date: 10/18/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: getting-started
ms.openlocfilehash: c2f18c69901a5a6503314d508a776e6985872681
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
---
# <a name="get-started-with-aspnet-core"></a><span data-ttu-id="ba886-103">Erste Schritte mit ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="ba886-103">Get Started with ASP.NET Core</span></span>

> [!NOTE]
> <span data-ttu-id="ba886-104">Diese Anweisungen gelten für die neueste Version von ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="ba886-104">These instructions are for the latest version of ASP.NET Core.</span></span> <span data-ttu-id="ba886-105">Die Version 1.1. dieses Dokuments finden Sie unter [Erste Schritte mit ASP.NET Core 1.1](xref:getting-started-1.1).</span><span class="sxs-lookup"><span data-stu-id="ba886-105">See [Getting Started with ASP.NET Core 1.1](xref:getting-started-1.1) for the 1.1 version of this document.</span></span>

1. <span data-ttu-id="ba886-106">Installieren Sie [!INCLUDE [](~/includes/net-core-sdk-download-link.md)].</span><span class="sxs-lookup"><span data-stu-id="ba886-106">Install the [!INCLUDE [](~/includes/net-core-sdk-download-link.md)].</span></span>

2. <span data-ttu-id="ba886-107">Erstellen Sie ein neues .NET Core-Projekt.</span><span class="sxs-lookup"><span data-stu-id="ba886-107">Create a new .NET Core project.</span></span>

   <span data-ttu-id="ba886-108">Öffnen Sie unter macOS und Linux ein Terminalfenster.</span><span class="sxs-lookup"><span data-stu-id="ba886-108">On macOS and Linux, open a terminal window.</span></span> <span data-ttu-id="ba886-109">Öffnen Sie unter Windows eine Eingabeaufforderung.</span><span class="sxs-lookup"><span data-stu-id="ba886-109">On Windows, open a command prompt.</span></span> <span data-ttu-id="ba886-110">Geben Sie folgenden Befehl ein:</span><span class="sxs-lookup"><span data-stu-id="ba886-110">Enter the following command:</span></span>

    ```terminal
    dotnet new razor -o aspnetcoreapp
    ```
    
3. <span data-ttu-id="ba886-111">Führen Sie die App aus.</span><span class="sxs-lookup"><span data-stu-id="ba886-111">Run the app.</span></span>

    <span data-ttu-id="ba886-112">Verwenden Sie die folgenden Befehle, um die App auszuführen:</span><span class="sxs-lookup"><span data-stu-id="ba886-112">Use the following commands to run the app:</span></span>

    ```terminal
    cd aspnetcoreapp
    dotnet run
    ```

4. <span data-ttu-id="ba886-113">Wechseln Sie zu [http://localhost:5000](http://localhost:5000).</span><span class="sxs-lookup"><span data-stu-id="ba886-113">Browse to [http://localhost:5000](http://localhost:5000)</span></span>

5. <span data-ttu-id="ba886-114">Öffnen Sie <em>Pages/About.cshtml</em>, und verändern Sie die Seite so, dass sie die Meldung „Hallo Welt!“ anzeigt.</span><span class="sxs-lookup"><span data-stu-id="ba886-114">Open <em>Pages/About.cshtml</em> and modify the page to display the message "Hello, world!</span></span> <span data-ttu-id="ba886-115">Die Zeit auf dem Server beträgt @DateTime.Now ":</span><span class="sxs-lookup"><span data-stu-id="ba886-115">The time on the server is @DateTime.Now ":</span></span>

    [!code-html[](getting-started/sample/getting-started/about.cshtml?highlight=9&range=1-9)]

6. <span data-ttu-id="ba886-116">Wechseln Sie zu [http://localhost:5000/About](http://localhost:5000/About), und bestätigen Sie die Änderungen.</span><span class="sxs-lookup"><span data-stu-id="ba886-116">Browse to [http://localhost:5000/About](http://localhost:5000/About) and verify the changes.</span></span>

### <a name="next-steps"></a><span data-ttu-id="ba886-117">Nächste Schritte</span><span class="sxs-lookup"><span data-stu-id="ba886-117">Next steps</span></span>

<span data-ttu-id="ba886-118">Tutorials für die ersten Schritte finden Sie unter [ASP.NET Core-Tutorials](tutorials/index.md).</span><span class="sxs-lookup"><span data-stu-id="ba886-118">For getting-started tutorials, see [ASP.NET Core Tutorials](tutorials/index.md)</span></span>

<span data-ttu-id="ba886-119">Eine Einführung in ASP.NET Core-Konzepte und -Architektur finden Sie unter [Einführung in ASP.NET Core](index.md) und [ASP.NET Core – Grundlagen](fundamentals/index.md).</span><span class="sxs-lookup"><span data-stu-id="ba886-119">For an introduction to ASP.NET Core concepts and architecture, see [ASP.NET Core Introduction](index.md) and [ASP.NET Core Fundamentals](fundamentals/index.md).</span></span>

<span data-ttu-id="ba886-120">Eine ASP.NET Core-App kann die .NET Core oder .NET Framework-Basisklassenbibliothek und -Laufzeit verwenden.</span><span class="sxs-lookup"><span data-stu-id="ba886-120">An ASP.NET Core app can use the .NET Core or .NET Framework Base Class Library and runtime.</span></span> <span data-ttu-id="ba886-121">Weitere Informationen finden Sie unter [Wahl zwischen .NET Core und .NET Framework](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server).</span><span class="sxs-lookup"><span data-stu-id="ba886-121">For more information, see [Choosing between .NET Core and .NET Framework](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server).</span></span>
