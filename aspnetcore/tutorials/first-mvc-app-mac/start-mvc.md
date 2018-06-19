---
title: Erste Schritte mit ASP.NET Core MVC und Visual Studio für Mac
author: rick-anderson
description: Hier finden Sie Informationen zum Einstieg in ASP.NET Core MVC und Visual Studio.
manager: wpickett
ms.author: riande
ms.date: 8/23/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: tutorials/first-mvc-app-mac/start-mvc
ms.openlocfilehash: ffa620f07251c52c785672d8fbeefacac31ed4c1
ms.sourcegitcommit: 24c32648ab0c6f0be15333d7c23c1bf680858c43
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/21/2018
ms.locfileid: "30896639"
---
# <a name="get-started-with-aspnet-core-mvc-and-visual-studio-for-mac"></a><span data-ttu-id="4ee27-103">Erste Schritte mit ASP.NET Core MVC und Visual Studio für Mac</span><span class="sxs-lookup"><span data-stu-id="4ee27-103">Get started with ASP.NET Core MVC and Visual Studio for Mac</span></span>

<span data-ttu-id="4ee27-104">Von [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="4ee27-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="4ee27-105">Dieses Tutorial vermittelt Ihnen die Grundlagen zum Erstellen einer ASP.NET Core MVC-Web-App mithilfe von [Visual Studio für Mac](https://www.visualstudio.com/vs/visual-studio-mac/).</span><span class="sxs-lookup"><span data-stu-id="4ee27-105">This tutorial teaches you the basics of building an ASP.NET Core MVC web app using [Visual Studio for Mac](https://www.visualstudio.com/vs/visual-studio-mac/).</span></span> 

[!INCLUDE [consider RP](../../includes/razor.md)]

<span data-ttu-id="4ee27-106">Es gibt drei Versionen dieses Tutorials:</span><span class="sxs-lookup"><span data-stu-id="4ee27-106">There are 3 versions of this tutorial:</span></span>

* <span data-ttu-id="4ee27-107">macOS: [Erstellen einer ASP.NET Core MVC-App mit Visual Studio für Mac](xref:tutorials/first-mvc-app-mac/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="4ee27-107">macOS: [Build an ASP.NET Core MVC app with Visual Studio for Mac](xref:tutorials/first-mvc-app-mac/start-mvc)</span></span>
* <span data-ttu-id="4ee27-108">Windows: [Erstellen einer ASP.NET Core MVC-App mit Visual Studio](xref:tutorials/first-mvc-app/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="4ee27-108">Windows: [Build an ASP.NET Core MVC app with Visual Studio](xref:tutorials/first-mvc-app/start-mvc)</span></span>
* <span data-ttu-id="4ee27-109">Linux, macOS und Windows: [Erstellen einer ASP.NET Core MVC-App mit Visual Studio Code](xref:tutorials/first-mvc-app-xplat/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="4ee27-109">Linux, macOS, and Windows: [Build an ASP.NET Core MVC app with Visual Studio Code](xref:tutorials/first-mvc-app-xplat/start-mvc)</span></span>

## <a name="prerequisites"></a><span data-ttu-id="4ee27-110">Erforderliche Komponenten</span><span class="sxs-lookup"><span data-stu-id="4ee27-110">Prerequisites</span></span>

[!INCLUDE [](~/includes/net-core-prereqs-macos.md)]

## <a name="create-a-web-app"></a><span data-ttu-id="4ee27-111">Erstellen einer Web-App</span><span class="sxs-lookup"><span data-stu-id="4ee27-111">Create a web app</span></span>

<span data-ttu-id="4ee27-112">Klicken Sie in Visual Studio auf **Datei > Neue Projektmappe**.</span><span class="sxs-lookup"><span data-stu-id="4ee27-112">From Visual Studio, select **File > New Solution**.</span></span>

![Neue Projektmappe in macOS](../first-web-api-mac/_static/sln.png)

<span data-ttu-id="4ee27-114">Klicken Sie auf **.NET Core-App > ASP.NET Core > Web-App > Weiter**.</span><span class="sxs-lookup"><span data-stu-id="4ee27-114">Select **.NET Core App >  ASP.NET Core > Web App > Next**.</span></span>

![Dialogfeld „Neue Projektmappe“ in macOS](start-mvc/1.png)

<span data-ttu-id="4ee27-116">Nennen Sie das Projekt **MvcMovie**, und wählen Sie dann **Erstellen** aus.</span><span class="sxs-lookup"><span data-stu-id="4ee27-116">Name the project **MvcMovie**, and then select **Create**.</span></span>

![Dialogfeld „Neue Projektmappe“ in macOS](start-mvc/2.png)

### <a name="launch-the-app"></a><span data-ttu-id="4ee27-118">Starten der App</span><span class="sxs-lookup"><span data-stu-id="4ee27-118">Launch the app</span></span>

<span data-ttu-id="4ee27-119">Klicken Sie in Visual Studio auf **Ausführen > Ohne Debugging starten**, um die App zu starten.</span><span class="sxs-lookup"><span data-stu-id="4ee27-119">In Visual Studio, select **Run > Start Without Debugging** to launch the app.</span></span> <span data-ttu-id="4ee27-120">Visual Studio startet [Kestrel](xref:fundamentals/servers/index#kestrel) und einen Browser und navigiert zu `http://localhost:port`, wobei es sich bei *port* um eine zufällig ausgewählte Portnummer handelt.</span><span class="sxs-lookup"><span data-stu-id="4ee27-120">Visual Studio starts [Kestrel](xref:fundamentals/servers/index#kestrel), launches a browser, and navigates to `http://localhost:port`, where *port* is a randomly chosen port number.</span></span>

![Browser mit einem neuen Projekt](start-mvc/b1.png)

* <span data-ttu-id="4ee27-122">Die Adressleiste zeigt `localhost:port#` an, nicht `example.com`.</span><span class="sxs-lookup"><span data-stu-id="4ee27-122">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="4ee27-123">Das liegt daran, dass es sich bei `localhost` um den Standardhostnamen für Ihren lokalen Computer handelt.</span><span class="sxs-lookup"><span data-stu-id="4ee27-123">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="4ee27-124">Wenn in Visual Studio ein Webprojekt erstellt wird, wird für den Webserver ein zufälliger Port verwendet.</span><span class="sxs-lookup"><span data-stu-id="4ee27-124">When Visual Studio creates a web project, a random port is used for the web server.</span></span> <span data-ttu-id="4ee27-125">Wenn Sie die App ausführen, wird eine andere Portnummer angezeigt.</span><span class="sxs-lookup"><span data-stu-id="4ee27-125">When you run the app, you'll see a different port number.</span></span>
* <span data-ttu-id="4ee27-126">Sie können die App über das Menü **Ausführen** im Debugmodus oder Nicht-Debugmodus starten.</span><span class="sxs-lookup"><span data-stu-id="4ee27-126">You can launch the app in debug or non-debug mode from the **Run** menu.</span></span>

<span data-ttu-id="4ee27-127">Die Standardvorlage bietet Ihnen Links für **Startseite, Info** und **Kontakt**.</span><span class="sxs-lookup"><span data-stu-id="4ee27-127">The default template gives you **Home, About** and **Contact** links.</span></span> <span data-ttu-id="4ee27-128">Die obenstehende Browserabbildung zeigt diese Links nicht an.</span><span class="sxs-lookup"><span data-stu-id="4ee27-128">The browser image above doesn't show these links.</span></span> <span data-ttu-id="4ee27-129">Je nach Größe Ihres Browsers müssen Sie möglicherweise auf das Navigationssymbol klicken, um diese anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="4ee27-129">Depending on the size of your browser, you might need to click the navigation icon to show them.</span></span>

![Browser mit einem neuen Projekt](start-mvc/b2.png)

<span data-ttu-id="4ee27-131">Im nächsten Teil dieses Tutorials erfahren Sie mehr über MVC und beginnen mit dem Schreiben von Code.</span><span class="sxs-lookup"><span data-stu-id="4ee27-131">In the next part of this tutorial, you learn about MVC and start writing some code.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="4ee27-132">Nächste</span><span class="sxs-lookup"><span data-stu-id="4ee27-132">Next</span></span>](adding-controller.md)  
