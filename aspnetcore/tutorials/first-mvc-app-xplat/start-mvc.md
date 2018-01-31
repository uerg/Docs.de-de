---
title: "Einführung in ASP.NET Core MVC unter Mac, Linux und Windows"
author: rick-anderson
description: Erste Schritte mit ASP.NET Core MVC und Visual Studio Code unter Mac, Linux und Windows
manager: wpickett
ms.author: riande
ms.date: 07/07/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/first-mvc-app-xplat/start-mvc
ms.openlocfilehash: 4771555b66f328a819f17a32eb3959f9ecf33d44
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2018
---
# <a name="getting-started-with-aspnet-core-mvc--on-mac-linux-or-windows"></a><span data-ttu-id="2b25c-103">Erste Schritte mit ASP.NET Core MVC unter Mac, Linux oder Windows</span><span class="sxs-lookup"><span data-stu-id="2b25c-103">Getting started with ASP.NET Core MVC  on Mac, Linux, or Windows</span></span>

<span data-ttu-id="2b25c-104">Von [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="2b25c-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="2b25c-105">Dieses Tutorial vermittelt Ihnen die Grundlagen zum Erstellen einer ASP.NET Core MVC-Web-App mithilfe von [Visual Studio Code](https://code.visualstudio.com) (VS Code).</span><span class="sxs-lookup"><span data-stu-id="2b25c-105">This tutorial will teach you the basics of building an ASP.NET Core MVC web app using [Visual Studio Code](https://code.visualstudio.com) (VS Code).</span></span> <span data-ttu-id="2b25c-106">Das Tutorial setzt voraus, dass Sie mit VS Code vertraut sind.</span><span class="sxs-lookup"><span data-stu-id="2b25c-106">The tutorial assumes familarity with VS Code.</span></span> <span data-ttu-id="2b25c-107">Weitere Informationen finden Sie unter [Erste Schritte mit VS Code](https://code.visualstudio.com/docs) und [Hilfe zu Visual Studio Code](#visual-studio-code-help).</span><span class="sxs-lookup"><span data-stu-id="2b25c-107">See [Getting started with VS Code](https://code.visualstudio.com/docs) and [Visual Studio Code help](#visual-studio-code-help) for more information.</span></span> 

[!INCLUDE[consider RP](../../includes/razor.md)]

<span data-ttu-id="2b25c-108">Es gibt drei Versionen dieses Tutorials:</span><span class="sxs-lookup"><span data-stu-id="2b25c-108">There are 3 versions of this tutorial:</span></span>

* <span data-ttu-id="2b25c-109">macOS: [Erstellen einer ASP.NET Core MVC-App mit Visual Studio für Mac](xref:tutorials/first-mvc-app-mac/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="2b25c-109">macOS: [Create an ASP.NET Core MVC app with Visual Studio for Mac](xref:tutorials/first-mvc-app-mac/start-mvc)</span></span>
* <span data-ttu-id="2b25c-110">Windows: [Erstellen einer ASP.NET Core MVC-App mit Visual Studio](xref:tutorials/first-mvc-app/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="2b25c-110">Windows: [Create an ASP.NET Core MVC app with Visual Studio](xref:tutorials/first-mvc-app/start-mvc)</span></span>
* <span data-ttu-id="2b25c-111">macOS, Linux und Windows: [Erstellen einer ASP.NET Core MVC-App mit Visual Studio Code](xref:tutorials/first-mvc-app-xplat/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="2b25c-111">macOS, Linux, and Windows: [Create an ASP.NET Core MVC app with Visual Studio Code](xref:tutorials/first-mvc-app-xplat/start-mvc)</span></span> 

## <a name="install-vs-code-and-net-core"></a><span data-ttu-id="2b25c-112">Installieren von VS-Code und .NET Core</span><span class="sxs-lookup"><span data-stu-id="2b25c-112">Install VS Code and .NET Core</span></span>

<span data-ttu-id="2b25c-113">Dieses Tutorial erfordert das [.NET Core 2.0.0 SDK](https://www.microsoft.com/net/core) oder höher.</span><span class="sxs-lookup"><span data-stu-id="2b25c-113">This tutorial requires the [.NET Core 2.0.0 SDK](https://www.microsoft.com/net/core) or later.</span></span> <span data-ttu-id="2b25c-114">In [dieser PDF](https://github.com/aspnet/Docs/blob/master/aspnetcore/tutorials/first-mvc-app-mac/start-mvc/8-23-17.pdf) finden Sie die Version ASP.NET Core 1.1.</span><span class="sxs-lookup"><span data-stu-id="2b25c-114">See [the pdf](https://github.com/aspnet/Docs/blob/master/aspnetcore/tutorials/first-mvc-app-mac/start-mvc/8-23-17.pdf) for the ASP.NET Core 1.1 version.</span></span>

<span data-ttu-id="2b25c-115">Installieren Sie Folgendes:</span><span class="sxs-lookup"><span data-stu-id="2b25c-115">Install the following:</span></span>

* <span data-ttu-id="2b25c-116">mindestens [.NET Core 2.0.0 SDK](https://www.microsoft.com/net/core)</span><span class="sxs-lookup"><span data-stu-id="2b25c-116">[.NET Core 2.0.0 SDK](https://www.microsoft.com/net/core) or later.</span></span>
* [<span data-ttu-id="2b25c-117">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="2b25c-117">Visual Studio Code</span></span>](https://code.visualstudio.com)
* <span data-ttu-id="2b25c-118">[C#-Erweiterung](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp) für VS Code</span><span class="sxs-lookup"><span data-stu-id="2b25c-118">VS Code [C# extension](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)</span></span> 

## <a name="create-a-web-app-with-dotnet"></a><span data-ttu-id="2b25c-119">Erstellen einer Web-App mit dotnet</span><span class="sxs-lookup"><span data-stu-id="2b25c-119">Create a web app with dotnet</span></span>

<span data-ttu-id="2b25c-120">Führen Sie über ein Terminal die folgenden Befehle aus:</span><span class="sxs-lookup"><span data-stu-id="2b25c-120">From a terminal, run the following commands:</span></span>

```console
mkdir MvcMovie
cd MvcMovie
dotnet new mvc
```

<span data-ttu-id="2b25c-121">Öffnen den Ordner *MvcMovie* in Visual Studio Code (VS Code), und wählen Sie die Datei *Startup.cs* aus.</span><span class="sxs-lookup"><span data-stu-id="2b25c-121">Open the *MvcMovie* folder in Visual Studio Code (VS Code) and select the *Startup.cs* file.</span></span>

- <span data-ttu-id="2b25c-122">Klicken Sie auf **Ja**, wenn die **Warnung** „Required assets to build and debug are missing from „MvcMovie“. Add them?“ („Die erforderlichen Objekte für die Erstellung und das Debugging sind in „MvcMovie“ nicht vorhanden.</span><span class="sxs-lookup"><span data-stu-id="2b25c-122">Select **Yes** to the **Warn** message "Required assets to build and debug are missing from 'MvcMovie'.</span></span> <span data-ttu-id="2b25c-123">Sollen Sie hinzugefügt werden?“) angezeigt wird.</span><span class="sxs-lookup"><span data-stu-id="2b25c-123">Add them?"</span></span>
- <span data-ttu-id="2b25c-124">Klicken Sie in der **Infomeldung** „There are unresolved depedencies“ („Es bestehen ungelöste Abhängigkeiten“) auf **Wiederherstellen**.</span><span class="sxs-lookup"><span data-stu-id="2b25c-124">Select **Restore** to the **Info** message "There are unresolved dependencies".</span></span>

![VS Code mit der Warnung „Required assets to build and debug are missing from „MvcMovie“. Add them?“ („Die erforderlichen Objekte für die Erstellung und das Debugging sind in „MvcMovie“ nicht vorhanden.](../web-api-vsc/_static/vsc_restore.png)

<span data-ttu-id="2b25c-128">Drücken Sie **Debuggen** (F5), um das Programm zu erstellen und auszuführen.</span><span class="sxs-lookup"><span data-stu-id="2b25c-128">Press **Debug** (F5) to build and run the program.</span></span>

![Ausgeführte App](../first-mvc-app/start-mvc/_static/1.png)

<span data-ttu-id="2b25c-130">VS Code startet den [Kestrel](xref:fundamentals/servers/kestrel)-Webserver und führt Ihre App aus.</span><span class="sxs-lookup"><span data-stu-id="2b25c-130">VS Code starts the [Kestrel](xref:fundamentals/servers/kestrel) web server and runs your app.</span></span> <span data-ttu-id="2b25c-131">Beachten Sie, dass die Adressleiste `localhost:5000` und nicht etwas wie `example.com` anzeigt.</span><span class="sxs-lookup"><span data-stu-id="2b25c-131">Notice that the address bar shows `localhost:5000` and not something like `example.com`.</span></span> <span data-ttu-id="2b25c-132">Das liegt daran, dass es sich bei `localhost` um den Standardhostnamen für Ihren lokalen Computer handelt.</span><span class="sxs-lookup"><span data-stu-id="2b25c-132">That's because `localhost` is the standard hostname for your local computer.</span></span>

<span data-ttu-id="2b25c-133">Die Standardvorlage bietet Ihnen funktionierende Links für **Startseite, Info** und **Kontakt**.</span><span class="sxs-lookup"><span data-stu-id="2b25c-133">The default template gives you working **Home, About** and **Contact** links.</span></span> <span data-ttu-id="2b25c-134">Die obenstehende Browserabbildung zeigt diese Links nicht an.</span><span class="sxs-lookup"><span data-stu-id="2b25c-134">The browser image above doesn't show these links.</span></span> <span data-ttu-id="2b25c-135">Je nach Größe Ihres Browsers müssen Sie möglicherweise auf das Navigationssymbol klicken, um diese anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="2b25c-135">Depending on the size of your browser, you might need to click the navigation icon to show them.</span></span>

![Navigationssymbol oben rechts](../first-mvc-app/start-mvc/_static/2.png)

<span data-ttu-id="2b25c-137">Im nächsten Teil dieses Tutorials erfahren Sie mehr über MVC und beginnen mit dem Schreiben von Code.</span><span class="sxs-lookup"><span data-stu-id="2b25c-137">In the next part of this tutorial, we'll learn about MVC and start writing some code.</span></span>

## <a name="visual-studio-code-help"></a><span data-ttu-id="2b25c-138">Hilfe zu Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="2b25c-138">Visual Studio Code help</span></span>

- [<span data-ttu-id="2b25c-139">Erste Schritte</span><span class="sxs-lookup"><span data-stu-id="2b25c-139">Getting started</span></span>](https://code.visualstudio.com/docs)
- [<span data-ttu-id="2b25c-140">Debuggen</span><span class="sxs-lookup"><span data-stu-id="2b25c-140">Debugging</span></span>](https://code.visualstudio.com/docs/editor/debugging)
- [<span data-ttu-id="2b25c-141">Integriertes Terminal</span><span class="sxs-lookup"><span data-stu-id="2b25c-141">Integrated terminal</span></span>](https://code.visualstudio.com/docs/editor/integrated-terminal)
- [<span data-ttu-id="2b25c-142">Tastenkombinationen</span><span class="sxs-lookup"><span data-stu-id="2b25c-142">Keyboard shortcuts</span></span>](https://code.visualstudio.com/docs/getstarted/keybindings#_keyboard-shortcuts-reference)

  - [<span data-ttu-id="2b25c-143">Mac-Tastenkombinationen</span><span class="sxs-lookup"><span data-stu-id="2b25c-143">Mac keyboard shortcuts</span></span>](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-macos.pdf)
  - [<span data-ttu-id="2b25c-144">Linux-Tastenkombinationen</span><span class="sxs-lookup"><span data-stu-id="2b25c-144">Linux keyboard shortcuts</span></span>](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-linux.pdf)
  - [<span data-ttu-id="2b25c-145">Windows-Tastenkombinationen</span><span class="sxs-lookup"><span data-stu-id="2b25c-145">Windows keyboard shortcuts</span></span>](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-windows.pdf)

>[!div class="step-by-step"]
[<span data-ttu-id="2b25c-146">Nächstes Tutorial: Hinzufügen eines Controllers</span><span class="sxs-lookup"><span data-stu-id="2b25c-146">Next - Add a controller</span></span>](adding-controller.md)
