---
title: Erste Schritte mit Razor-Seiten in ASP.NET Core mit Visual Studio für Mac unter macOS
author: rick-anderson
description: Hier finden Sie Informationen zum Einstieg in Razor-Seiten in ASP.NET Core mit Visual Studio für Mac.
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.date: 07/27/2017
uid: tutorials/razor-pages-mac/razor-pages-start
ms.openlocfilehash: c2d2038a77a67d4e955856756f73e18e31f13a5d
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/20/2018
ms.locfileid: "36272051"
---
# <a name="get-started-with-razor-pages-in-aspnet-core-on-macos-with-visual-studio-for-mac"></a><span data-ttu-id="08ad8-103">Erste Schritte mit Razor-Seiten in ASP.NET Core mit Visual Studio für Mac unter macOS</span><span class="sxs-lookup"><span data-stu-id="08ad8-103">Get started with Razor Pages in ASP.NET Core on macOS with Visual Studio for Mac</span></span>

<span data-ttu-id="08ad8-104">Von [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="08ad8-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="08ad8-105">In diesem Tutorial lernen Sie Grundlegendes zur Erstellung einer ASP.NET Core-Webapp mit Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="08ad8-105">This tutorial teaches the basics of building an ASP.NET Core Razor Pages web app.</span></span> <span data-ttu-id="08ad8-106">Es wird empfohlen, zuerst das Tutorial [Einführung in Razor Pages](xref:razor-pages/index) zu lesen, bevor Sie mit diesem Tutorial beginnen.</span><span class="sxs-lookup"><span data-stu-id="08ad8-106">We recommend you review [Introduction to Razor Pages](xref:razor-pages/index) before starting this tutorial.</span></span> <span data-ttu-id="08ad8-107">Razor Pages sind der empfohlene Weg für die Erstellung von Benutzeroberflächen für Webanwendungen in ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="08ad8-107">Razor Pages is the recommended way to build UI for web applications in ASP.NET Core.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="08ad8-108">Erforderliche Komponenten</span><span class="sxs-lookup"><span data-stu-id="08ad8-108">Prerequisites</span></span>

[!INCLUDE [](~/includes/net-core-prereqs-macos.md)]

## <a name="create-a-razor-web-app"></a><span data-ttu-id="08ad8-109">Erstellen einer Razor-Web-App</span><span class="sxs-lookup"><span data-stu-id="08ad8-109">Create a Razor web app</span></span>

<span data-ttu-id="08ad8-110">Führen Sie über ein Terminal die folgenden Befehle aus:</span><span class="sxs-lookup"><span data-stu-id="08ad8-110">From a terminal, run the following commands:</span></span>

::: moniker range=">= aspnetcore-2.1"

```console
dotnet new webapp -o RazorPagesMovie
cd RazorPagesMovie
dotnet run
```

[!INCLUDE[](~/includes/webapp-alias-notice.md)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

```console
dotnet new razor -o RazorPagesMovie
cd RazorPagesMovie
dotnet run
```

::: moniker-end

<span data-ttu-id="08ad8-112">Diese Befehle verwenden die [.NET Core-CLI](https://docs.microsoft.com/dotnet/core/tools/dotnet), um ein Projekt mit Razor Pages zu erstellen und auszuführen.</span><span class="sxs-lookup"><span data-stu-id="08ad8-112">The preceding commands use the [.NET Core CLI](https://docs.microsoft.com/dotnet/core/tools/dotnet) to create and run a Razor Pages project.</span></span> <span data-ttu-id="08ad8-113">Öffnen Sie http://localhost:5000 in einem Browser, um sich die Anwendung anzeigen zu lassen.</span><span class="sxs-lookup"><span data-stu-id="08ad8-113">Open a browser to http://localhost:5000 to view the application.</span></span>

![Start- oder Indexseite](../razor-pages/razor-pages-start/_static/home.png)

[!INCLUDE [razor-pages-start](../../includes/RP/razor-pages-start.md)]

## <a name="open-the-project"></a><span data-ttu-id="08ad8-115">Öffnen des Projekts</span><span class="sxs-lookup"><span data-stu-id="08ad8-115">Open the project</span></span>

<span data-ttu-id="08ad8-116">Drücken Sie STRG+C, um die Anwendung zu beenden.</span><span class="sxs-lookup"><span data-stu-id="08ad8-116">Press Ctrl+C to shut down the application.</span></span>

<span data-ttu-id="08ad8-117">Klicken Sie in Visual Studio auf **Datei > Öffnen**, und wählen Sie dann die Datei *RazorPagesMovie.csproj* aus.</span><span class="sxs-lookup"><span data-stu-id="08ad8-117">From Visual Studio, select **File > Open**, and then select the *RazorPagesMovie.csproj* file.</span></span>

### <a name="launch-the-app"></a><span data-ttu-id="08ad8-118">Starten der App</span><span class="sxs-lookup"><span data-stu-id="08ad8-118">Launch the app</span></span>

<span data-ttu-id="08ad8-119">Klicken Sie in Visual Studio auf **Ausführen > Ohne Debugging starten**, um die App zu starten.</span><span class="sxs-lookup"><span data-stu-id="08ad8-119">In Visual Studio, select **Run > Start Without Debugging** to launch the app.</span></span> <span data-ttu-id="08ad8-120">Visual Studio startet dann [Kestrel](xref:fundamentals/servers/kestrel) und einen Browser und navigiert zu `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="08ad8-120">Visual Studio starts [Kestrel](xref:fundamentals/servers/kestrel), launches a browser, and navigates to `http://localhost:5000`.</span></span>

<span data-ttu-id="08ad8-121">Im nächsten Tutorial wird dem Projekt ein Modell hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="08ad8-121">In the next tutorial, we add a model to the project.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="08ad8-122">Weiter: Hinzufügen eines Modells</span><span class="sxs-lookup"><span data-stu-id="08ad8-122">Next: Adding a model</span></span>](xref:tutorials/razor-pages-mac/model)
