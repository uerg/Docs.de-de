---
title: Erste Schritte mit Razor-Seiten in ASP.NET Core mit Visual Studio Code
author: rick-anderson
description: Erste Schritte mit Razor-Seiten in ASP.NET Core mit Visual Studio Code
keywords: "ASP.NET Core, Razor-Seiten, Gerüstbau, Entity Framework Core, EF, EF Core, Datenbank, Mac, macOS, Visual Studio Code, Code"
ms.author: riande
manager: wpickett
ms.date: 8/27/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: aspnet-core
uid: tutorials/razor-pages-vsc/razor-pages-start
ms.openlocfilehash: aa39de71addb2499af6d322db6da0ec635c54970
ms.sourcegitcommit: d9ec19e5452af83648074db5d96c0a0f4f9e7f9a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/29/2017
---
# <a name="getting-started-with-razor-pages-in-aspnet-core-with-visual-studio-code"></a><span data-ttu-id="3109c-104">Erste Schritte mit Razor-Seiten in ASP.NET Core mit Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="3109c-104">Getting started with Razor Pages in ASP.NET Core with Visual Studio Code</span></span>

<span data-ttu-id="3109c-105">Von [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="3109c-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="3109c-106">In diesem Tutorial lernen Sie Grundlegendes zur Erstellung einer ASP.NET Core-Webapp mit Razor-Seiten.</span><span class="sxs-lookup"><span data-stu-id="3109c-106">This tutorial teaches the basics of building an ASP.NET Core Razor Pages web app.</span></span> <span data-ttu-id="3109c-107">Wir empfehlen Ihnen, zuerst das Tutorial [Introduction to Razor Pages (Einführung in Razor-Seiten)](xref:mvc/razor-pages/index) durchzuarbeiten, bevor Sie mit diesem Tutorial beginnen.</span><span class="sxs-lookup"><span data-stu-id="3109c-107">We recommend you complete [Introduction to Razor Pages](xref:mvc/razor-pages/index) before starting this tutorial.</span></span> <span data-ttu-id="3109c-108">Razor-Seiten stellen die empfohlene Methode für die Erstellung der Benutzeroberfläche für Webanwendungen in ASP.NET Core dar.</span><span class="sxs-lookup"><span data-stu-id="3109c-108">Razor Pages is the recommended way to build UI for web applications in ASP.NET Core.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="3109c-109">Erforderliche Komponenten</span><span class="sxs-lookup"><span data-stu-id="3109c-109">Prerequisites</span></span>

<span data-ttu-id="3109c-110">Installieren Sie Folgendes:</span><span class="sxs-lookup"><span data-stu-id="3109c-110">Install the following:</span></span>

* <span data-ttu-id="3109c-111">[.NET Core 2.0.0 SDK](https://dot.net/core) oder höher</span><span class="sxs-lookup"><span data-stu-id="3109c-111">[.NET Core 2.0.0 SDK](https://dot.net/core) or later</span></span>
* [<span data-ttu-id="3109c-112">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="3109c-112">Visual Studio Code</span></span>](https://code.visualstudio.com)
* <span data-ttu-id="3109c-113">Visual Studio Code [C#-Erweiterung](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)</span><span class="sxs-lookup"><span data-stu-id="3109c-113">VS Code [C# extension](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)</span></span> 

## <a name="create-a-razor-web-app"></a><span data-ttu-id="3109c-114">Erstellen einer Razor-Web-App</span><span class="sxs-lookup"><span data-stu-id="3109c-114">Create a Razor web app</span></span>

<span data-ttu-id="3109c-115">Führen Sie über ein Terminal die folgenden Befehle aus:</span><span class="sxs-lookup"><span data-stu-id="3109c-115">From a terminal, run the following commands:</span></span>

```console
dotnet new razor -o RazorPagesMovie
cd RazorPagesMovie
dotnet run
```

<span data-ttu-id="3109c-116">Diese Befehle verwenden die [.NET Core-CLI](https://docs.microsoft.com/dotnet/core/tools/dotnet), um ein Projekt mit Razor-Seiten zu erstellen und auszuführen.</span><span class="sxs-lookup"><span data-stu-id="3109c-116">The preceding commands use the [.NET Core CLI](https://docs.microsoft.com/dotnet/core/tools/dotnet) to create and run a Razor Pages project.</span></span> <span data-ttu-id="3109c-117">Öffnen Sie in einem Browser „http://localhost:5000“, um sich die Anwendung anzeigen zu lassen.</span><span class="sxs-lookup"><span data-stu-id="3109c-117">Open a browser to http://localhost:5000 to view the application.</span></span>

![Start- oder Indexseite](../razor-pages/razor-pages-start/_static/home.png)

[!INCLUDE[razor-pages-start](../../includes/RP/razor-pages-start.md)]

## <a name="open-the-project"></a><span data-ttu-id="3109c-119">Öffnen des Projekts</span><span class="sxs-lookup"><span data-stu-id="3109c-119">Open the project</span></span>

<span data-ttu-id="3109c-120">Drücken Sie STRG+C, um die Anwendung zu beenden.</span><span class="sxs-lookup"><span data-stu-id="3109c-120">Press Ctrl+C to shut down the application.</span></span>

<span data-ttu-id="3109c-121">Klicken Sie in Visual Studio Code (VS Code) auf **Datei > Ordner öffnen**, und wählen Sie dann den Ordner *RazorPagesMovie* aus.</span><span class="sxs-lookup"><span data-stu-id="3109c-121">From Visual Studio Code (VS Code), select **File > Open Folder**, and then select the *RazorPagesMovie* folder.</span></span>

- <span data-ttu-id="3109c-122">Klicken Sie auf **Ja**, wenn die **Warnung** „Required assets to build and debug are missing from 'RazorPagesMovie'. Add them?“ („Die erforderlichen Objekte für die Erstellung und das Debugging sind in 'RazorPagesMovie' nicht vorhanden.</span><span class="sxs-lookup"><span data-stu-id="3109c-122">Select **Yes** to the **Warn** message "Required assets to build and debug are missing from 'RazorPagesMovie'.</span></span> <span data-ttu-id="3109c-123">Sollen Sie hinzugefügt werden?“) angezeigt wird.</span><span class="sxs-lookup"><span data-stu-id="3109c-123">Add them?"</span></span>
- <span data-ttu-id="3109c-124">Klicken Sie bei der **Infomeldung** „There are unresolved depedencies“ („Es bestehen ungelöste Abhängigkeiten“) auf **Wiederherstellen**.</span><span class="sxs-lookup"><span data-stu-id="3109c-124">Select **Restore** to the **Info** message "There are unresolved dependencies".</span></span>

### <a name="launch-the-app"></a><span data-ttu-id="3109c-125">Starten der App</span><span class="sxs-lookup"><span data-stu-id="3109c-125">Launch the app</span></span>

<span data-ttu-id="3109c-126">Drücken Sie STRG+F5, um die App ohne Debugging zu starten.</span><span class="sxs-lookup"><span data-stu-id="3109c-126">Press Ctrl+F5 to start the app without debugging.</span></span> <span data-ttu-id="3109c-127">Alternativ können Sie auch über das Menü **Debuggen** die Option **Starten ohne Debugging** auswählen.</span><span class="sxs-lookup"><span data-stu-id="3109c-127">Alternatively, from the **Debug** menu, select **Start Without Debugging**.</span></span>

<span data-ttu-id="3109c-128">Im nächsten Tutorial fügen wir dem Projekt ein Modell hinzu.</span><span class="sxs-lookup"><span data-stu-id="3109c-128">In the next tutorial, we add a model to the project.</span></span> 

>[!div class="step-by-step"]
[<span data-ttu-id="3109c-129">Nächstes Tutorial: Adding a model to a Razor Pages app in ASP.NET Core with Visual Studio for Mac (Hinzufügen eines Modells zu einer App mit Razor-Seiten in ASP.NET Core mit Visual Studio für Mac)</span><span class="sxs-lookup"><span data-stu-id="3109c-129">Next: Adding a model</span></span>](xref:tutorials/razor-pages-vsc/model)  
