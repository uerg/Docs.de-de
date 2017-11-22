---
title: "Erste Schritte mit ASP.NET Core MVC und Visual Studio für Mac"
author: rick-anderson
description: Erste Schritte mit ASP.NET Core MVC und Visual Studio
keywords: "ASP.NET Core, MVC, Visual Studio für Mac, Entity Framework"
ms.author: riande
manager: wpickett
ms.date: 8/23/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/first-mvc-app-mac/start-mvc
ms.openlocfilehash: 387d7a91ae7d58cbc293c04039017df1dd208c82
ms.sourcegitcommit: f017f940a164dbaf84307410c78eb14e0f3ac811
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/15/2017
---
# <a name="getting-started-with-aspnet-core-mvc-and-visual-studio-for-mac"></a><span data-ttu-id="6c0c5-104">Erste Schritte mit ASP.NET Core MVC und Visual Studio für Mac</span><span class="sxs-lookup"><span data-stu-id="6c0c5-104">Getting started with ASP.NET Core MVC and Visual Studio for Mac</span></span>

<span data-ttu-id="6c0c5-105">Von [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="6c0c5-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="6c0c5-106">Dieses Tutorial vermittelt Ihnen die Grundlagen zum Erstellen einer ASP.NET Core MVC-Web-App mithilfe von [Visual Studio für Mac](https://www.visualstudio.com/vs/visual-studio-mac/).</span><span class="sxs-lookup"><span data-stu-id="6c0c5-106">This tutorial teaches you the basics of building an ASP.NET Core MVC web app using [Visual Studio for Mac](https://www.visualstudio.com/vs/visual-studio-mac/).</span></span> 

[!INCLUDE[consider RP](../../includes/razor.md)]

<span data-ttu-id="6c0c5-107">Es gibt drei Versionen dieses Tutorials:</span><span class="sxs-lookup"><span data-stu-id="6c0c5-107">There are 3 versions of this tutorial:</span></span>

* <span data-ttu-id="6c0c5-108">macOS: [Erstellen einer ASP.NET Core MVC-App mit Visual Studio für Mac](xref:tutorials/first-mvc-app-mac/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="6c0c5-108">macOS: [Build an ASP.NET Core MVC app with Visual Studio for Mac](xref:tutorials/first-mvc-app-mac/start-mvc)</span></span>
* <span data-ttu-id="6c0c5-109">Windows: [Erstellen einer ASP.NET Core MVC-App mit Visual Studio](xref:tutorials/first-mvc-app/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="6c0c5-109">Windows: [Build an ASP.NET Core MVC app with Visual Studio](xref:tutorials/first-mvc-app/start-mvc)</span></span>
* <span data-ttu-id="6c0c5-110">Linux, macOS und Windows: [Erstellen einer ASP.NET Core MVC-App mit Visual Studio Code](xref:tutorials/first-mvc-app-xplat/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="6c0c5-110">Linux, macOS, and Windows: [Build an ASP.NET Core MVC app with Visual Studio Code](xref:tutorials/first-mvc-app-xplat/start-mvc)</span></span>

## <a name="prerequisites"></a><span data-ttu-id="6c0c5-111">Erforderliche Komponenten</span><span class="sxs-lookup"><span data-stu-id="6c0c5-111">Prerequisites</span></span>

<span data-ttu-id="6c0c5-112">Dieses Tutorial erfordert das [.NET Core 2.0.0 SDK](https://www.microsoft.com/net/core) oder höher.</span><span class="sxs-lookup"><span data-stu-id="6c0c5-112">This tutorial requires the [.NET Core 2.0.0 SDK](https://www.microsoft.com/net/core) or later.</span></span> <span data-ttu-id="6c0c5-113">In [dieser PDF](https://github.com/aspnet/Docs/blob/master/aspnetcore/tutorials/first-mvc-app-mac/start-mvc/8-23-17.pdf) finden Sie die Version ASP.NET Core 1.1.</span><span class="sxs-lookup"><span data-stu-id="6c0c5-113">See [the pdf](https://github.com/aspnet/Docs/blob/master/aspnetcore/tutorials/first-mvc-app-mac/start-mvc/8-23-17.pdf) for the ASP.NET Core 1.1 version.</span></span>

<span data-ttu-id="6c0c5-114">Installieren Sie Folgendes:</span><span class="sxs-lookup"><span data-stu-id="6c0c5-114">Install the following:</span></span>

- <span data-ttu-id="6c0c5-115">[.NET Core 2.0.0 SDK](https://www.microsoft.com/net/core) oder höher</span><span class="sxs-lookup"><span data-stu-id="6c0c5-115">[.NET Core 2.0.0 SDK](https://www.microsoft.com/net/core) or later</span></span>
- [<span data-ttu-id="6c0c5-116">Visual Studio für Mac</span><span class="sxs-lookup"><span data-stu-id="6c0c5-116">Visual Studio for Mac</span></span>](https://www.visualstudio.com/vs/visual-studio-mac/)

## <a name="create-a-web-app"></a><span data-ttu-id="6c0c5-117">Erstellen einer Web-App</span><span class="sxs-lookup"><span data-stu-id="6c0c5-117">Create a web app</span></span>

<span data-ttu-id="6c0c5-118">Klicken Sie in Visual Studio auf **Datei > Neue Projektmappe**.</span><span class="sxs-lookup"><span data-stu-id="6c0c5-118">From Visual Studio, select **File > New Solution**.</span></span>

![Neue Projektmappe in macOS](../first-web-api-mac/_static/sln.png)

<span data-ttu-id="6c0c5-120">Klicken Sie auf **.NET Core-App > ASP.NET Core > Web-App > Weiter**.</span><span class="sxs-lookup"><span data-stu-id="6c0c5-120">Select **.NET Core App >  ASP.NET Core > Web App > Next**.</span></span>

![Dialogfeld „Neue Projektmappe“ in macOS](start-mvc/1.png)

<span data-ttu-id="6c0c5-122">Nennen Sie das Projekt **MvcMovie**, und wählen Sie dann **Erstellen** aus.</span><span class="sxs-lookup"><span data-stu-id="6c0c5-122">Name the project **MvcMovie**, and then select **Create**.</span></span>

![Dialogfeld „Neue Projektmappe“ in macOS](start-mvc/2.png)

### <a name="launch-the-app"></a><span data-ttu-id="6c0c5-124">Starten der App</span><span class="sxs-lookup"><span data-stu-id="6c0c5-124">Launch the app</span></span>

<span data-ttu-id="6c0c5-125">Klicken Sie in Visual Studio auf **Ausführen > Ohne Debugging starten**, um die App zu starten.</span><span class="sxs-lookup"><span data-stu-id="6c0c5-125">In Visual Studio, select **Run > Start Without Debugging** to launch the app.</span></span> <span data-ttu-id="6c0c5-126">Visual Studio startet [Kestrel](xref:fundamentals/servers/index#Kestrel) und einen Browser und navigiert zu `http://localhost:port`, wobei es sich bei *port* um eine zufällig ausgewählte Portnummer handelt.</span><span class="sxs-lookup"><span data-stu-id="6c0c5-126">Visual Studio starts [Kestrel](xref:fundamentals/servers/index#Kestrel), launches a browser, and navigates to `http://localhost:port`, where *port* is a randomly chosen port number.</span></span>

![Browser mit einem neuen Projekt](start-mvc/b1.png)

* <span data-ttu-id="6c0c5-128">Die Adressleiste zeigt `localhost:port#` an, nicht `example.com`.</span><span class="sxs-lookup"><span data-stu-id="6c0c5-128">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="6c0c5-129">Das liegt daran, dass es sich bei `localhost` um den Standardhostnamen für Ihren lokalen Computer handelt.</span><span class="sxs-lookup"><span data-stu-id="6c0c5-129">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="6c0c5-130">Wenn in Visual Studio ein Webprojekt erstellt wird, wird für den Webserver ein zufälliger Port verwendet.</span><span class="sxs-lookup"><span data-stu-id="6c0c5-130">When Visual Studio creates a web project, a random port is used for the web server.</span></span> <span data-ttu-id="6c0c5-131">Wenn Sie die App ausführen, wird eine andere Portnummer angezeigt.</span><span class="sxs-lookup"><span data-stu-id="6c0c5-131">When you run the app, you'll see a different port number.</span></span>
* <span data-ttu-id="6c0c5-132">Sie können die App über das Menü **Ausführen** im Debugmodus oder Nicht-Debugmodus starten.</span><span class="sxs-lookup"><span data-stu-id="6c0c5-132">You can launch the app in debug or non-debug mode from the **Run** menu.</span></span>

<span data-ttu-id="6c0c5-133">Die Standardvorlage bietet Ihnen Links für **Startseite, Info** und **Kontakt**.</span><span class="sxs-lookup"><span data-stu-id="6c0c5-133">The default template gives you **Home, About** and **Contact** links.</span></span> <span data-ttu-id="6c0c5-134">Die obenstehende Browserabbildung zeigt diese Links nicht an.</span><span class="sxs-lookup"><span data-stu-id="6c0c5-134">The browser image above doesn't show these links.</span></span> <span data-ttu-id="6c0c5-135">Je nach Größe Ihres Browsers müssen Sie möglicherweise auf das Navigationssymbol klicken, um diese anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="6c0c5-135">Depending on the size of your browser, you might need to click the navigation icon to show them.</span></span>

![Browser mit einem neuen Projekt](start-mvc/b2.png)

<span data-ttu-id="6c0c5-137">Im nächsten Teil dieses Tutorials erfahren Sie mehr über MVC und beginnen mit dem Schreiben von Code.</span><span class="sxs-lookup"><span data-stu-id="6c0c5-137">In the next part of this tutorial, you learn about MVC and start writing some code.</span></span>

>[!div class="step-by-step"]
[<span data-ttu-id="6c0c5-138">Nächste</span><span class="sxs-lookup"><span data-stu-id="6c0c5-138">Next</span></span>](adding-controller.md)  
