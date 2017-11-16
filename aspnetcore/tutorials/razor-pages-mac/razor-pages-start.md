---
title: Erste Schritte mit Razor-Seiten mit ASP.NET Core unter Mac
author: rick-anderson
description: "Erster Schritte bei Razor-Seiten in ASP.NET Core mithilfe von Visual Studio für Mac"
keywords: "ASP.NET Core, Razor-Seiten, Gerüstbau, Entity Framework Core, EF, EF Core, Datenbank, Mac, macOS, Visual Studio für Mac"
ms.author: riande
manager: wpickett
ms.date: 07/27/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: aspnet-core
uid: tutorials/razor-pages-mac/razor-pages-start
ms.openlocfilehash: 9431e8160011d1485925db5cc4f9f83bf7381e97
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
# <a name="getting-started-with-razor-pages-in-aspnet-core-with-visual-studio-for-mac"></a><span data-ttu-id="1a8fa-104">Erste Schritte mit Razor-Seiten in ASP.NET Core mit Visual Studio für Mac</span><span class="sxs-lookup"><span data-stu-id="1a8fa-104">Getting started with Razor Pages in ASP.NET Core with Visual Studio for Mac</span></span>

<span data-ttu-id="1a8fa-105">Von [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="1a8fa-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="1a8fa-106">In diesem Tutorial lernen Sie Grundlegendes zur Erstellung einer ASP.NET Core-Webapp mit Razor-Seiten.</span><span class="sxs-lookup"><span data-stu-id="1a8fa-106">This tutorial teaches the basics of building an ASP.NET Core Razor Pages web app.</span></span> <span data-ttu-id="1a8fa-107">Wir empfehlen Ihnen, zuerst das Tutorial [Introduction to Razor Pages (Einführung in Razor-Seiten)](xref:mvc/razor-pages/index) durchzuarbeiten, bevor Sie mit diesem Tutorial beginnen.</span><span class="sxs-lookup"><span data-stu-id="1a8fa-107">We recommend you complete [Introduction to Razor Pages](xref:mvc/razor-pages/index) before starting this tutorial.</span></span> <span data-ttu-id="1a8fa-108">Razor-Seiten sind der empfohlene Weg für die Erstellung von Benutzeroberflächen für Webanwendungen in ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="1a8fa-108">Razor Pages is the recommended way to build UI for web applications in ASP.NET Core.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="1a8fa-109">Erforderliche Komponenten</span><span class="sxs-lookup"><span data-stu-id="1a8fa-109">Prerequisites</span></span>

<span data-ttu-id="1a8fa-110">Installieren Sie Folgendes:</span><span class="sxs-lookup"><span data-stu-id="1a8fa-110">Install the following:</span></span>

* <span data-ttu-id="1a8fa-111">[.NET Core 2.0.0 SDK](https://www.microsoft.com/net/core) oder höher</span><span class="sxs-lookup"><span data-stu-id="1a8fa-111">[.NET Core 2.0.0 SDK](https://www.microsoft.com/net/core) or later</span></span>
* [<span data-ttu-id="1a8fa-112">Visual Studio für Mac</span><span class="sxs-lookup"><span data-stu-id="1a8fa-112">Visual Studio for Mac</span></span>](https://www.visualstudio.com/vs/visual-studio-mac/)

## <a name="create-a-razor-web-app"></a><span data-ttu-id="1a8fa-113">Erstellen einer Razor-Web-App</span><span class="sxs-lookup"><span data-stu-id="1a8fa-113">Create a Razor web app</span></span>

<span data-ttu-id="1a8fa-114">Führen Sie über ein Terminal die folgenden Befehle aus:</span><span class="sxs-lookup"><span data-stu-id="1a8fa-114">From a terminal, run the following commands:</span></span>

```console
dotnet new razor -o RazorPagesMovie
cd RazorPagesMovie
dotnet run
```

<span data-ttu-id="1a8fa-115">Diese Befehle verwenden die [.NET Core-CLI](https://docs.microsoft.com/dotnet/core/tools/dotnet), um ein Projekt mit Razor-Seiten zu erstellen und auszuführen.</span><span class="sxs-lookup"><span data-stu-id="1a8fa-115">The preceding commands use the [.NET Core CLI](https://docs.microsoft.com/dotnet/core/tools/dotnet) to create and run a Razor Pages project.</span></span> <span data-ttu-id="1a8fa-116">Öffnen Sie in einem Browser „http://localhost:5000“, um sich die Anwendung anzeigen zu lassen.</span><span class="sxs-lookup"><span data-stu-id="1a8fa-116">Open a browser to http://localhost:5000 to view the application.</span></span>

![Start- oder Indexseite](../razor-pages/razor-pages-start/_static/home.png)

[!INCLUDE[razor-pages-start](../../includes/RP/razor-pages-start.md)]

## <a name="open-the-project"></a><span data-ttu-id="1a8fa-118">Öffnen des Projekts</span><span class="sxs-lookup"><span data-stu-id="1a8fa-118">Open the project</span></span>

<span data-ttu-id="1a8fa-119">Drücken Sie STRG+C, um die Anwendung zu beenden.</span><span class="sxs-lookup"><span data-stu-id="1a8fa-119">Press Ctrl+C to shut down the application.</span></span>

<span data-ttu-id="1a8fa-120">Klicken Sie in Visual Studio auf **Datei > Öffnen**, und wählen Sie dann die Datei *RazorPagesMovie.csproj* aus.</span><span class="sxs-lookup"><span data-stu-id="1a8fa-120">From Visual Studio, select **File > Open**, and then select the *RazorPagesMovie.csproj* file.</span></span>

### <a name="launch-the-app"></a><span data-ttu-id="1a8fa-121">Starten der App</span><span class="sxs-lookup"><span data-stu-id="1a8fa-121">Launch the app</span></span>

<span data-ttu-id="1a8fa-122">Klicken Sie in Visual Studio auf **Ausführen > Ohne Debugging starten**, um die App zu starten.</span><span class="sxs-lookup"><span data-stu-id="1a8fa-122">In Visual Studio, select **Run > Start Without Debugging** to launch the app.</span></span> <span data-ttu-id="1a8fa-123">Visual Studio startet dann [IIS Express](https://docs.microsoft.com/iis/extensions/introduction-to-iis-express/iis-express-overview) und einen Browser und navigiert zu `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="1a8fa-123">Visual Studio starts [IIS Express](https://docs.microsoft.com/iis/extensions/introduction-to-iis-express/iis-express-overview), launches a browser, and navigates to `http://localhost:5000`.</span></span>

<span data-ttu-id="1a8fa-124">Im nächsten Tutorial wird dem Projekt ein Modell hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="1a8fa-124">In the next tutorial, we add a model to the project.</span></span>

>[!div class="step-by-step"]
[<span data-ttu-id="1a8fa-125">Weiter: Hinzufügen eines Modells</span><span class="sxs-lookup"><span data-stu-id="1a8fa-125">Next: Adding a model</span></span>](xref:tutorials/razor-pages-mac/model)
