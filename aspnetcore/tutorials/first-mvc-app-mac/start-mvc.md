---
title: "Erste Schritte mit ASP.NET Core MVC und Visual Studio für Mac"
author: rick-anderson
description: Erste Schritte mit ASP.NET Core MVC und Visual Studio
ms.author: riande
manager: wpickett
ms.date: 8/23/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/first-mvc-app-mac/start-mvc
ms.openlocfilehash: 51c0477c40885d3aaaa7562f8baba0a94cb4f920
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/19/2018
---
# <a name="getting-started-with-aspnet-core-mvc-and-visual-studio-for-mac"></a><span data-ttu-id="5441b-103">Erste Schritte mit ASP.NET Core MVC und Visual Studio für Mac</span><span class="sxs-lookup"><span data-stu-id="5441b-103">Getting started with ASP.NET Core MVC and Visual Studio for Mac</span></span>

<span data-ttu-id="5441b-104">Von [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="5441b-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="5441b-105">Dieses Tutorial vermittelt Ihnen die Grundlagen zum Erstellen einer ASP.NET Core MVC-Web-App mithilfe von [Visual Studio für Mac](https://www.visualstudio.com/vs/visual-studio-mac/).</span><span class="sxs-lookup"><span data-stu-id="5441b-105">This tutorial teaches you the basics of building an ASP.NET Core MVC web app using [Visual Studio for Mac](https://www.visualstudio.com/vs/visual-studio-mac/).</span></span> 

[!INCLUDE[consider RP](../../includes/razor.md)]

<span data-ttu-id="5441b-106">Es gibt drei Versionen dieses Tutorials:</span><span class="sxs-lookup"><span data-stu-id="5441b-106">There are 3 versions of this tutorial:</span></span>

* <span data-ttu-id="5441b-107">macOS: [Erstellen einer ASP.NET Core MVC-App mit Visual Studio für Mac](xref:tutorials/first-mvc-app-mac/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="5441b-107">macOS: [Build an ASP.NET Core MVC app with Visual Studio for Mac](xref:tutorials/first-mvc-app-mac/start-mvc)</span></span>
* <span data-ttu-id="5441b-108">Windows: [Erstellen einer ASP.NET Core MVC-App mit Visual Studio](xref:tutorials/first-mvc-app/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="5441b-108">Windows: [Build an ASP.NET Core MVC app with Visual Studio](xref:tutorials/first-mvc-app/start-mvc)</span></span>
* <span data-ttu-id="5441b-109">Linux, macOS und Windows: [Erstellen einer ASP.NET Core MVC-App mit Visual Studio Code](xref:tutorials/first-mvc-app-xplat/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="5441b-109">Linux, macOS, and Windows: [Build an ASP.NET Core MVC app with Visual Studio Code](xref:tutorials/first-mvc-app-xplat/start-mvc)</span></span>

## <a name="prerequisites"></a><span data-ttu-id="5441b-110">Erforderliche Komponenten</span><span class="sxs-lookup"><span data-stu-id="5441b-110">Prerequisites</span></span>

<span data-ttu-id="5441b-111">Dieses Tutorial erfordert das [.NET Core 2.0.0 SDK](https://www.microsoft.com/net/core) oder höher.</span><span class="sxs-lookup"><span data-stu-id="5441b-111">This tutorial requires the [.NET Core 2.0.0 SDK](https://www.microsoft.com/net/core) or later.</span></span>

<span data-ttu-id="5441b-112">Installieren Sie Folgendes:</span><span class="sxs-lookup"><span data-stu-id="5441b-112">Install the following:</span></span>

- <span data-ttu-id="5441b-113">[.NET Core 2.0.0 SDK](https://www.microsoft.com/net/core) oder höher</span><span class="sxs-lookup"><span data-stu-id="5441b-113">[.NET Core 2.0.0 SDK](https://www.microsoft.com/net/core) or later</span></span>
- [<span data-ttu-id="5441b-114">Visual Studio für Mac</span><span class="sxs-lookup"><span data-stu-id="5441b-114">Visual Studio for Mac</span></span>](https://www.visualstudio.com/vs/visual-studio-mac/)

## <a name="create-a-web-app"></a><span data-ttu-id="5441b-115">Erstellen einer Web-App</span><span class="sxs-lookup"><span data-stu-id="5441b-115">Create a web app</span></span>

<span data-ttu-id="5441b-116">Klicken Sie in Visual Studio auf **Datei > Neue Projektmappe**.</span><span class="sxs-lookup"><span data-stu-id="5441b-116">From Visual Studio, select **File > New Solution**.</span></span>

![Neue Projektmappe in macOS](../first-web-api-mac/_static/sln.png)

<span data-ttu-id="5441b-118">Klicken Sie auf **.NET Core-App > ASP.NET Core > Web-App > Weiter**.</span><span class="sxs-lookup"><span data-stu-id="5441b-118">Select **.NET Core App >  ASP.NET Core > Web App > Next**.</span></span>

![Dialogfeld „Neue Projektmappe“ in macOS](start-mvc/1.png)

<span data-ttu-id="5441b-120">Nennen Sie das Projekt **MvcMovie**, und wählen Sie dann **Erstellen** aus.</span><span class="sxs-lookup"><span data-stu-id="5441b-120">Name the project **MvcMovie**, and then select **Create**.</span></span>

![Dialogfeld „Neue Projektmappe“ in macOS](start-mvc/2.png)

### <a name="launch-the-app"></a><span data-ttu-id="5441b-122">Starten der App</span><span class="sxs-lookup"><span data-stu-id="5441b-122">Launch the app</span></span>

<span data-ttu-id="5441b-123">Klicken Sie in Visual Studio auf **Ausführen > Ohne Debugging starten**, um die App zu starten.</span><span class="sxs-lookup"><span data-stu-id="5441b-123">In Visual Studio, select **Run > Start Without Debugging** to launch the app.</span></span> <span data-ttu-id="5441b-124">Visual Studio startet [Kestrel](xref:fundamentals/servers/index#kestrel) und einen Browser und navigiert zu `http://localhost:port`, wobei es sich bei *port* um eine zufällig ausgewählte Portnummer handelt.</span><span class="sxs-lookup"><span data-stu-id="5441b-124">Visual Studio starts [Kestrel](xref:fundamentals/servers/index#kestrel), launches a browser, and navigates to `http://localhost:port`, where *port* is a randomly chosen port number.</span></span>

![Browser mit einem neuen Projekt](start-mvc/b1.png)

* <span data-ttu-id="5441b-126">Die Adressleiste zeigt `localhost:port#` an, nicht `example.com`.</span><span class="sxs-lookup"><span data-stu-id="5441b-126">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="5441b-127">Das liegt daran, dass es sich bei `localhost` um den Standardhostnamen für Ihren lokalen Computer handelt.</span><span class="sxs-lookup"><span data-stu-id="5441b-127">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="5441b-128">Wenn in Visual Studio ein Webprojekt erstellt wird, wird für den Webserver ein zufälliger Port verwendet.</span><span class="sxs-lookup"><span data-stu-id="5441b-128">When Visual Studio creates a web project, a random port is used for the web server.</span></span> <span data-ttu-id="5441b-129">Wenn Sie die App ausführen, wird eine andere Portnummer angezeigt.</span><span class="sxs-lookup"><span data-stu-id="5441b-129">When you run the app, you'll see a different port number.</span></span>
* <span data-ttu-id="5441b-130">Sie können die App über das Menü **Ausführen** im Debugmodus oder Nicht-Debugmodus starten.</span><span class="sxs-lookup"><span data-stu-id="5441b-130">You can launch the app in debug or non-debug mode from the **Run** menu.</span></span>

<span data-ttu-id="5441b-131">Die Standardvorlage bietet Ihnen Links für **Startseite, Info** und **Kontakt**.</span><span class="sxs-lookup"><span data-stu-id="5441b-131">The default template gives you **Home, About** and **Contact** links.</span></span> <span data-ttu-id="5441b-132">Die obenstehende Browserabbildung zeigt diese Links nicht an.</span><span class="sxs-lookup"><span data-stu-id="5441b-132">The browser image above doesn't show these links.</span></span> <span data-ttu-id="5441b-133">Je nach Größe Ihres Browsers müssen Sie möglicherweise auf das Navigationssymbol klicken, um diese anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="5441b-133">Depending on the size of your browser, you might need to click the navigation icon to show them.</span></span>

![Browser mit einem neuen Projekt](start-mvc/b2.png)

<span data-ttu-id="5441b-135">Im nächsten Teil dieses Tutorials erfahren Sie mehr über MVC und beginnen mit dem Schreiben von Code.</span><span class="sxs-lookup"><span data-stu-id="5441b-135">In the next part of this tutorial, you learn about MVC and start writing some code.</span></span>

>[!div class="step-by-step"]
[<span data-ttu-id="5441b-136">Nächste</span><span class="sxs-lookup"><span data-stu-id="5441b-136">Next</span></span>](adding-controller.md)  
