---
title: "Einführung in ASP.NET Core MVC unter Mac, Linux und Windows"
author: rick-anderson
description: Erste Schritte mit ASP.NET Core MVC und Visual Studio Code unter Mac, Linux und Windows
keywords: ASP.NET Core, MVC, VS Code, Visual Studio Code, Mac, Linux, Windows
ms.author: riande
manager: wpickett
ms.date: 07/07/2017
ms.topic: get-started-article
ms.assetid: 1d18b589-1638-4dc6-1638-fb0f41998d78
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/first-mvc-app-xplat/start-mvc
ms.openlocfilehash: 82ce5bbc695b190759ac2c05cdceebb5f7854eb7
ms.sourcegitcommit: e6a8f171f26fab1b2195a2d7f14e7d258a2e690e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/23/2017
---
# <a name="getting-started-with-aspnet-core-mvc--on-mac-linux-or-windows"></a><span data-ttu-id="8fef5-104">Erste Schritte mit ASP.NET Core MVC unter Mac, Linux oder Windows</span><span class="sxs-lookup"><span data-stu-id="8fef5-104">Getting started with ASP.NET Core MVC  on Mac, Linux, or Windows</span></span>

<span data-ttu-id="8fef5-105">Von [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="8fef5-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="8fef5-106">Dieses Tutorial vermittelt Ihnen die Grundlagen zum Erstellen einer ASP.NET Core MVC-Web-App mithilfe von [Visual Studio Code](https://code.visualstudio.com) (VS Code).</span><span class="sxs-lookup"><span data-stu-id="8fef5-106">This tutorial will teach you the basics of building an ASP.NET Core MVC web app using [Visual Studio Code](https://code.visualstudio.com) (VS Code).</span></span> <span data-ttu-id="8fef5-107">Das Tutorial setzt voraus, dass Sie mit VS Code vertraut sind.</span><span class="sxs-lookup"><span data-stu-id="8fef5-107">The tutorial assumes familarity with VS Code.</span></span> <span data-ttu-id="8fef5-108">Weitere Informationen finden Sie unter [Erste Schritte mit VS Code](https://code.visualstudio.com/docs) und [Hilfe zu Visual Studio Code](#visual-studio-code-help).</span><span class="sxs-lookup"><span data-stu-id="8fef5-108">See [Getting started with VS Code](https://code.visualstudio.com/docs) and [Visual Studio Code help](#visual-studio-code-help) for more information.</span></span> [!INCLUDE[consider RP](../../includes/razor.md)]

<span data-ttu-id="8fef5-109">Es gibt drei Versionen dieses Tutorials:</span><span class="sxs-lookup"><span data-stu-id="8fef5-109">There are 3 versions of this tutorial:</span></span>

* <span data-ttu-id="8fef5-110">macOS: [Erstellen einer ASP.NET Core MVC-App mit Visual Studio für Mac](xref:tutorials/first-mvc-app-mac/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="8fef5-110">macOS: [Create an ASP.NET Core MVC app with Visual Studio for Mac](xref:tutorials/first-mvc-app-mac/start-mvc)</span></span>
* <span data-ttu-id="8fef5-111">Windows: [Erstellen einer ASP.NET Core MVC-App mit Visual Studio](xref:tutorials/first-mvc-app/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="8fef5-111">Windows: [Create an ASP.NET Core MVC app with Visual Studio](xref:tutorials/first-mvc-app/start-mvc)</span></span>
* <span data-ttu-id="8fef5-112">macOS, Linux und Windows: [Erstellen einer ASP.NET Core MVC-App mit Visual Studio Code](xref:tutorials/first-mvc-app-xplat/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="8fef5-112">macOS, Linux, and Windows: [Create an ASP.NET Core MVC app with Visual Studio Code](xref:tutorials/first-mvc-app-xplat/start-mvc)</span></span> 

## <a name="install-vs-code-and-net-core"></a><span data-ttu-id="8fef5-113">Installieren von VS-Code und .NET Core</span><span class="sxs-lookup"><span data-stu-id="8fef5-113">Install VS Code and .NET Core</span></span>

<span data-ttu-id="8fef5-114">Dieses Tutorial erfordert das [.NET Core 2.0.0 SDK](https://dot.net/core) oder höher.</span><span class="sxs-lookup"><span data-stu-id="8fef5-114">This tutorial requires the [.NET Core 2.0.0 SDK](https://dot.net/core) or later.</span></span> <span data-ttu-id="8fef5-115">In [dieser PDF](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/first-mvc-app-mac/start-mvc/8-23-17.pdf) finden Sie die Version ASP.NET Core 1.1.</span><span class="sxs-lookup"><span data-stu-id="8fef5-115">See [the pdf](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/first-mvc-app-mac/start-mvc/8-23-17.pdf) for the ASP.NET Core 1.1 version.</span></span>

<span data-ttu-id="8fef5-116">Installieren Sie Folgendes:</span><span class="sxs-lookup"><span data-stu-id="8fef5-116">Install the following:</span></span>

* <span data-ttu-id="8fef5-117">[.NET Core 2.0.0 SDK](https://dot.net/core) oder höher</span><span class="sxs-lookup"><span data-stu-id="8fef5-117">[.NET Core 2.0.0 SDK](https://dot.net/core) or later.</span></span>
* [<span data-ttu-id="8fef5-118">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="8fef5-118">Visual Studio Code</span></span>](https://code.visualstudio.com)
* <span data-ttu-id="8fef5-119">[C#-Erweiterung](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp) für VS Code</span><span class="sxs-lookup"><span data-stu-id="8fef5-119">VS Code [C# extension](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)</span></span> 

## <a name="create-a-web-app-with-dotnet"></a><span data-ttu-id="8fef5-120">Erstellen einer Web-App mit dotnet</span><span class="sxs-lookup"><span data-stu-id="8fef5-120">Create a web app with dotnet</span></span>

<span data-ttu-id="8fef5-121">Führen Sie über ein Terminal die folgenden Befehle aus:</span><span class="sxs-lookup"><span data-stu-id="8fef5-121">From a terminal, run the following commands:</span></span>

```console
mkdir MvcMovie
cd MvcMovie
dotnet new mvc
```

<span data-ttu-id="8fef5-122">Öffnen den Ordner *MvcMovie* in Visual Studio Code (VS Code), und wählen Sie die Datei *Startup.cs* aus.</span><span class="sxs-lookup"><span data-stu-id="8fef5-122">Open the *MvcMovie* folder in Visual Studio Code (VS Code) and select the *Startup.cs* file.</span></span>

- <span data-ttu-id="8fef5-123">Klicken Sie auf **Ja**, wenn die **Warnung** „Required assets to build and debug are missing from „MvcMovie“. Add them?“ („Die erforderlichen Objekte für die Erstellung und das Debugging sind in „MvcMovie“ nicht vorhanden.</span><span class="sxs-lookup"><span data-stu-id="8fef5-123">Select **Yes** to the **Warn** message "Required assets to build and debug are missing from 'MvcMovie'.</span></span> <span data-ttu-id="8fef5-124">Sollen Sie hinzugefügt werden?“) angezeigt wird.</span><span class="sxs-lookup"><span data-stu-id="8fef5-124">Add them?"</span></span>
- <span data-ttu-id="8fef5-125">Klicken Sie in der **Infomeldung** „There are unresolved depedencies“ („Es bestehen ungelöste Abhängigkeiten“) auf **Wiederherstellen**.</span><span class="sxs-lookup"><span data-stu-id="8fef5-125">Select **Restore** to the **Info** message "There are unresolved dependencies".</span></span>

![VS Code mit der Warnung „Required assets to build and debug are missing from „MvcMovie“. Add them?“ („Die erforderlichen Objekte für die Erstellung und das Debugging sind in „MvcMovie“ nicht vorhanden.](../web-api-vsc/_static/vsc_restore.png)

<span data-ttu-id="8fef5-129">Drücken Sie **Debuggen** (F5), um das Programm zu erstellen und auszuführen.</span><span class="sxs-lookup"><span data-stu-id="8fef5-129">Press **Debug** (F5) to build and run the program.</span></span>

![Ausgeführte App](../first-mvc-app/start-mvc/_static/1.png)

<span data-ttu-id="8fef5-131">VS Code startet den [Kestrel](xref:fundamentals/servers/kestrel)-Webserver und führt Ihre App aus.</span><span class="sxs-lookup"><span data-stu-id="8fef5-131">VS Code starts the [Kestrel](xref:fundamentals/servers/kestrel) web server and runs your app.</span></span> <span data-ttu-id="8fef5-132">Beachten Sie, dass die Adressleiste `localhost:5000` und nicht etwas wie `example.com` anzeigt.</span><span class="sxs-lookup"><span data-stu-id="8fef5-132">Notice that the address bar shows `localhost:5000` and not something like `example.com`.</span></span> <span data-ttu-id="8fef5-133">Das liegt daran, dass es sich bei `localhost` um den Standardhostnamen für Ihren lokalen Computer handelt.</span><span class="sxs-lookup"><span data-stu-id="8fef5-133">That's because `localhost` is the standard hostname for your local computer.</span></span>

<span data-ttu-id="8fef5-134">Die Standardvorlage bietet Ihnen funktionierende Links für **Startseite, Info** und **Kontakt**.</span><span class="sxs-lookup"><span data-stu-id="8fef5-134">The default template gives you working **Home, About** and **Contact** links.</span></span> <span data-ttu-id="8fef5-135">Die obenstehende Browserabbildung zeigt diese Links nicht an.</span><span class="sxs-lookup"><span data-stu-id="8fef5-135">The browser image above doesn't show these links.</span></span> <span data-ttu-id="8fef5-136">Je nach Größe Ihres Browsers müssen Sie möglicherweise auf das Navigationssymbol klicken, um diese anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="8fef5-136">Depending on the size of your browser, you might need to click the navigation icon to show them.</span></span>

![Navigationssymbol oben rechts](../first-mvc-app/start-mvc/_static/2.png)

<span data-ttu-id="8fef5-138">Im nächsten Teil dieses Tutorials erfahren Sie mehr über MVC und beginnen mit dem Schreiben von Code.</span><span class="sxs-lookup"><span data-stu-id="8fef5-138">In the next part of this tutorial, we'll learn about MVC and start writing some code.</span></span>

## <a name="visual-studio-code-help"></a><span data-ttu-id="8fef5-139">Hilfe zu Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="8fef5-139">Visual Studio Code help</span></span>

- [<span data-ttu-id="8fef5-140">Erste Schritte</span><span class="sxs-lookup"><span data-stu-id="8fef5-140">Getting started</span></span>](https://code.visualstudio.com/docs)
- [<span data-ttu-id="8fef5-141">Debuggen</span><span class="sxs-lookup"><span data-stu-id="8fef5-141">Debugging</span></span>](https://code.visualstudio.com/docs/editor/debugging)
- [<span data-ttu-id="8fef5-142">Integriertes Terminal</span><span class="sxs-lookup"><span data-stu-id="8fef5-142">Integrated terminal</span></span>](https://code.visualstudio.com/docs/editor/integrated-terminal)
- [<span data-ttu-id="8fef5-143">Tastenkombinationen</span><span class="sxs-lookup"><span data-stu-id="8fef5-143">Keyboard shortcuts</span></span>](https://code.visualstudio.com/docs/getstarted/keybindings#_keyboard-shortcuts-reference)

  - [<span data-ttu-id="8fef5-144">Mac-Tastenkombinationen</span><span class="sxs-lookup"><span data-stu-id="8fef5-144">Mac keyboard shortcuts</span></span>](https://go.microsoft.com/fwlink/?linkid=832143)
  - [<span data-ttu-id="8fef5-145">Linux-Tastenkombinationen</span><span class="sxs-lookup"><span data-stu-id="8fef5-145">Linux keyboard shortcuts</span></span>](https://go.microsoft.com/fwlink/?linkid=832144)
  - [<span data-ttu-id="8fef5-146">Windows-Tastenkombinationen</span><span class="sxs-lookup"><span data-stu-id="8fef5-146">Windows keyboard shortcuts</span></span>](https://go.microsoft.com/fwlink/?linkid=832145)

>[!div class="step-by-step"]
[<span data-ttu-id="8fef5-147">Nächstes Tutorial: Hinzufügen eines Controllers</span><span class="sxs-lookup"><span data-stu-id="8fef5-147">Next - Add a controller</span></span>](adding-controller.md)
