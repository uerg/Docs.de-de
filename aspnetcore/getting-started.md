---
title: Erste Schritte mit ASP.NET Core
author: rick-anderson
description: Kurztutorial, in dem eine einfache Hello World-App mit ASP.NET Core erstellt und ausgeführt wird.
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 05/10/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: getting-started
ms.openlocfilehash: e814277663ff5a964171a71ebb6e0f094e0ddc60
ms.sourcegitcommit: 3d071fabaf90e32906df97b08a8d00e602db25c0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/10/2018
---
# <a name="get-started-with-aspnet-core"></a><span data-ttu-id="f7c41-103">Erste Schritte mit ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="f7c41-103">Get started with ASP.NET Core</span></span>

::: moniker range=">= aspnetcore-2.0"

1. <span data-ttu-id="f7c41-104">Installieren Sie [!INCLUDE[](~/includes/net-core-sdk-download-link.md)].</span><span class="sxs-lookup"><span data-stu-id="f7c41-104">Install the [!INCLUDE[](~/includes/net-core-sdk-download-link.md)].</span></span>

2. <span data-ttu-id="f7c41-105">Erstellen Sie ein neues .NET Core-Projekt.</span><span class="sxs-lookup"><span data-stu-id="f7c41-105">Create a new .NET Core project.</span></span>

   <span data-ttu-id="f7c41-106">Öffnen Sie unter macOS und Linux ein Terminalfenster.</span><span class="sxs-lookup"><span data-stu-id="f7c41-106">On macOS and Linux, open a terminal window.</span></span> <span data-ttu-id="f7c41-107">Öffnen Sie unter Windows eine Eingabeaufforderung.</span><span class="sxs-lookup"><span data-stu-id="f7c41-107">On Windows, open a command prompt.</span></span> <span data-ttu-id="f7c41-108">Geben Sie folgenden Befehl ein:</span><span class="sxs-lookup"><span data-stu-id="f7c41-108">Enter the following command:</span></span>

    ```terminal
    dotnet new razor -o aspnetcoreapp
    ```

3. <span data-ttu-id="f7c41-109">Führen Sie die App mit den folgenden Befehlen aus:</span><span class="sxs-lookup"><span data-stu-id="f7c41-109">Run the app with the following commands:</span></span>

    ```terminal
    cd aspnetcoreapp
    dotnet run
    ```

4. <span data-ttu-id="f7c41-110">Wechseln Sie zu [http://localhost:5000](http://localhost:5000).</span><span class="sxs-lookup"><span data-stu-id="f7c41-110">Browse to [http://localhost:5000](http://localhost:5000).</span></span>

5. <span data-ttu-id="f7c41-111">Öffnen Sie *Pages/About.cshtml*, und verändern Sie die Seite so, dass sie die Meldung „Hallo Welt!“ anzeigt.</span><span class="sxs-lookup"><span data-stu-id="f7c41-111">Open *Pages/About.cshtml* and modify the page to display the message "Hello, world!</span></span> <span data-ttu-id="f7c41-112">Die Zeit auf dem Server ist @DateTime.Now" :</span><span class="sxs-lookup"><span data-stu-id="f7c41-112">The time on the server is @DateTime.Now":</span></span>

    <span data-ttu-id="f7c41-113">[!code-cshtml[](getting-started/sample/getting-started/about.cshtml?highlight=9&range=1-9)]</span><span class="sxs-lookup"><span data-stu-id="f7c41-113">[!code-cshtml[](getting-started/sample/getting-started/about.cshtml?highlight=9&range=1-9)]</span></span>

6. <span data-ttu-id="f7c41-114">Wechseln Sie zu [http://localhost:5000/About](http://localhost:5000/About), und bestätigen Sie die Änderungen.</span><span class="sxs-lookup"><span data-stu-id="f7c41-114">Browse to [http://localhost:5000/About](http://localhost:5000/About) and verify the changes.</span></span>

[!INCLUDE[next steps](~/includes/getting-started/next-steps.md)]
::: moniker-end

::: moniker range="<= aspnetcore-1.1"

1. <span data-ttu-id="f7c41-115">Installieren Sie den .NET Core **SDK Installer** für SDK 1.0.4 von der .NET Core-Seite [Alle Downloads](https://www.microsoft.com/net/download/all).</span><span class="sxs-lookup"><span data-stu-id="f7c41-115">Install the .NET Core **SDK Installer** for SDK 1.0.4 from the [.NET Core All Downloads page](https://www.microsoft.com/net/download/all).</span></span>

2. <span data-ttu-id="f7c41-116">Erstellen Sie einen Ordner für das neue .NET Core-Projekt.</span><span class="sxs-lookup"><span data-stu-id="f7c41-116">Create a folder for a new .NET Core project.</span></span>

   <span data-ttu-id="f7c41-117">Öffnen Sie unter macOS und Linux ein Terminalfenster.</span><span class="sxs-lookup"><span data-stu-id="f7c41-117">On macOS and Linux, open a terminal window.</span></span> <span data-ttu-id="f7c41-118">Öffnen Sie unter Windows eine Eingabeaufforderung.</span><span class="sxs-lookup"><span data-stu-id="f7c41-118">On Windows, open a command prompt.</span></span>

   ```terminal
   mkdir aspnetcoreapp
   cd aspnetcoreapp
   ```

3. <span data-ttu-id="f7c41-119">Wenn Sie eine höhere Version des SDK auf Ihrem Computer installiert haben, erstellen Sie die Datei *global.json*, und wählen Sie das SDK 1.0.4 aus.</span><span class="sxs-lookup"><span data-stu-id="f7c41-119">If you have installed a later SDK version on your machine, create a *global.json* file to select the 1.0.4 SDK.</span></span>

   ```json
   {
     "sdk": { "version": "1.0.4" }
   }
   ```

4. <span data-ttu-id="f7c41-120">Erstellen Sie ein neues .NET Core-Projekt.</span><span class="sxs-lookup"><span data-stu-id="f7c41-120">Create a new .NET Core project.</span></span>

   ```terminal
   dotnet new web
   ```

5. <span data-ttu-id="f7c41-121">Stellen Sie die Pakete wieder her.</span><span class="sxs-lookup"><span data-stu-id="f7c41-121">Restore the packages.</span></span>

    ```terminal
    dotnet restore
    ```

6. <span data-ttu-id="f7c41-122">Führen Sie die App aus.</span><span class="sxs-lookup"><span data-stu-id="f7c41-122">Run the app.</span></span>

   ```terminal
   dotnet run
   ```

   <span data-ttu-id="f7c41-123">Mit dem Befehl [dotnet run](/dotnet/core/tools/dotnet-run) wird die App bei Bedarf zunächst erstellt.</span><span class="sxs-lookup"><span data-stu-id="f7c41-123">The [dotnet run](/dotnet/core/tools/dotnet-run) command builds the app first, if needed.</span></span>

7. <span data-ttu-id="f7c41-124">Wechseln Sie zu `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="f7c41-124">Browse to `http://localhost:5000`.</span></span>

[!INCLUDE[next steps](~/includes/getting-started/next-steps.md)]
::: moniker-end