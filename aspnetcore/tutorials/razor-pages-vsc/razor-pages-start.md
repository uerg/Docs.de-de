---
title: Erste Schritte mit Razor-Seiten in ASP.NET Core mit Visual Studio Code
author: rick-anderson
description: Erste Schritte mit Razor-Seiten in ASP.NET Core mit Visual Studio Code
keywords: "ASP.NET Core, Razor-Seiten, Gerüstbau, Entity Framework Core, EF, EF Core, Datenbank, Mac, macOS, Visual Studio Code, Code"
ms.author: riande
manager: wpickett
ms.date: 08/27/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: aspnet-core
uid: tutorials/razor-pages-vsc/razor-pages-start
ms.openlocfilehash: 1b9dff14fa98314601fa44aa229aef6b73bb79d0
ms.sourcegitcommit: 67f54fabbfa4e3942f5bfe1f8a7fdfe4a7a75358
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/19/2017
---
# <a name="getting-started-with-razor-pages-in-aspnet-core-with-visual-studio-code"></a><span data-ttu-id="05470-104">Erste Schritte mit Razor-Seiten in ASP.NET Core mit Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="05470-104">Getting started with Razor Pages in ASP.NET Core with Visual Studio Code</span></span>

<span data-ttu-id="05470-105">Von [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="05470-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="05470-106">In diesem Tutorial lernen Sie Grundlegendes zur Erstellung einer ASP.NET Core-Webapp mit Razor-Seiten.</span><span class="sxs-lookup"><span data-stu-id="05470-106">This tutorial teaches the basics of building an ASP.NET Core Razor Pages web app.</span></span> <span data-ttu-id="05470-107">Wir empfehlen Ihnen, zuerst das Tutorial [Introduction to Razor Pages (Einführung in Razor-Seiten)](xref:mvc/razor-pages/index) durchzuarbeiten, bevor Sie mit diesem Tutorial beginnen.</span><span class="sxs-lookup"><span data-stu-id="05470-107">We recommend you complete [Introduction to Razor Pages](xref:mvc/razor-pages/index) before starting this tutorial.</span></span> <span data-ttu-id="05470-108">Razor-Seiten sind der empfohlene Weg für die Erstellung von Benutzeroberflächen für Webanwendungen in ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="05470-108">Razor Pages is the recommended way to build UI for web applications in ASP.NET Core.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="05470-109">Erforderliche Komponenten</span><span class="sxs-lookup"><span data-stu-id="05470-109">Prerequisites</span></span>

<span data-ttu-id="05470-110">Installieren Sie Folgendes:</span><span class="sxs-lookup"><span data-stu-id="05470-110">Install the following:</span></span>

* <span data-ttu-id="05470-111">[.NET Core 2.0.0 SDK](https://www.microsoft.com/net/core) oder höher</span><span class="sxs-lookup"><span data-stu-id="05470-111">[.NET Core 2.0.0 SDK](https://www.microsoft.com/net/core) or later</span></span>
* [<span data-ttu-id="05470-112">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="05470-112">Visual Studio Code</span></span>](https://code.visualstudio.com)
* <span data-ttu-id="05470-113">[C#-Erweiterung](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp) für VS Code</span><span class="sxs-lookup"><span data-stu-id="05470-113">VS Code [C# extension](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)</span></span> 

## <a name="create-a-razor-web-app"></a><span data-ttu-id="05470-114">Erstellen einer Razor-Web-App</span><span class="sxs-lookup"><span data-stu-id="05470-114">Create a Razor web app</span></span>

<span data-ttu-id="05470-115">Führen Sie über ein Terminal die folgenden Befehle aus:</span><span class="sxs-lookup"><span data-stu-id="05470-115">From a terminal, run the following commands:</span></span>

```console
dotnet new razor -o RazorPagesMovie
cd RazorPagesMovie
dotnet run
```

<span data-ttu-id="05470-116">Diese Befehle verwenden die [.NET Core-CLI](https://docs.microsoft.com/dotnet/core/tools/dotnet), um ein Projekt mit Razor-Seiten zu erstellen und auszuführen.</span><span class="sxs-lookup"><span data-stu-id="05470-116">The preceding commands use the [.NET Core CLI](https://docs.microsoft.com/dotnet/core/tools/dotnet) to create and run a Razor Pages project.</span></span> <span data-ttu-id="05470-117">Öffnen Sie in einem Browser „http://localhost:5000“, um sich die Anwendung anzeigen zu lassen.</span><span class="sxs-lookup"><span data-stu-id="05470-117">Open a browser to http://localhost:5000 to view the application.</span></span>

![Start- oder Indexseite](../razor-pages/razor-pages-start/_static/home.png)

[!INCLUDE[razor-pages-start](../../includes/RP/razor-pages-start.md)]

## <a name="open-the-project"></a><span data-ttu-id="05470-119">Öffnen des Projekts</span><span class="sxs-lookup"><span data-stu-id="05470-119">Open the project</span></span>

<span data-ttu-id="05470-120">Drücken Sie STRG+C, um die Anwendung zu beenden.</span><span class="sxs-lookup"><span data-stu-id="05470-120">Press Ctrl+C to shut down the application.</span></span>

<span data-ttu-id="05470-121">Klicken Sie in Visual Studio Code (VS Code) auf **Datei > Ordner öffnen**, und wählen Sie dann den Ordner *RazorPagesMovie* aus.</span><span class="sxs-lookup"><span data-stu-id="05470-121">From Visual Studio Code (VS Code), select **File > Open Folder**, and then select the *RazorPagesMovie* folder.</span></span>

- <span data-ttu-id="05470-122">Klicken Sie auf **Ja**, wenn die **Warnung** „Required assets to build and debug are missing from 'RazorPagesMovie'. Add them?“ („Die erforderlichen Objekte für die Erstellung und das Debugging sind in 'RazorPagesMovie' nicht vorhanden.</span><span class="sxs-lookup"><span data-stu-id="05470-122">Select **Yes** to the **Warn** message "Required assets to build and debug are missing from 'RazorPagesMovie'.</span></span> <span data-ttu-id="05470-123">Sollen Sie hinzugefügt werden?“) angezeigt wird.</span><span class="sxs-lookup"><span data-stu-id="05470-123">Add them?"</span></span>
- <span data-ttu-id="05470-124">Klicken Sie bei der **Infomeldung** „There are unresolved depedencies“ („Es bestehen ungelöste Abhängigkeiten“) auf **Wiederherstellen**.</span><span class="sxs-lookup"><span data-stu-id="05470-124">Select **Restore** to the **Info** message "There are unresolved dependencies".</span></span>

### <a name="launch-the-app"></a><span data-ttu-id="05470-125">Starten der App</span><span class="sxs-lookup"><span data-stu-id="05470-125">Launch the app</span></span>

<span data-ttu-id="05470-126">Drücken Sie STRG+F5, um die App ohne Debugging zu starten.</span><span class="sxs-lookup"><span data-stu-id="05470-126">Press Ctrl+F5 to start the app without debugging.</span></span> <span data-ttu-id="05470-127">Alternativ können Sie auch über das Menü **Debuggen** die Option **Starten ohne Debugging** auswählen.</span><span class="sxs-lookup"><span data-stu-id="05470-127">Alternatively, from the **Debug** menu, select **Start Without Debugging**.</span></span>

<span data-ttu-id="05470-128">Im nächsten Tutorial fügen wir dem Projekt ein Modell hinzu.</span><span class="sxs-lookup"><span data-stu-id="05470-128">In the next tutorial, we add a model to the project.</span></span> 

>[!div class="step-by-step"]
[<span data-ttu-id="05470-129">Weiter: Hinzufügen eines Modells</span><span class="sxs-lookup"><span data-stu-id="05470-129">Next: Adding a model</span></span>](xref:tutorials/razor-pages-vsc/model)  
