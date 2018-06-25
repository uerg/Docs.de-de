---
title: Einführung in ASP.NET Core MVC unter macOS, Linux und Windows
author: rick-anderson
description: Hier finden Sie Informationen zum Einstieg in ASP.NET Core MVC und Visual Studio Code unter macOS, Linux und Windows.
ms.author: riande
ms.date: 07/07/2017
uid: tutorials/first-mvc-app-xplat/start-mvc
ms.openlocfilehash: b25ee29541e345a3bf6700b67d992409c02b183a
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/20/2018
ms.locfileid: "36275274"
---
# <a name="introduction-to-aspnet-core-mvc-on-macos-linux-or-windows"></a><span data-ttu-id="ed1ee-103">Einführung in ASP.NET Core MVC unter macOS, Linux und Windows</span><span class="sxs-lookup"><span data-stu-id="ed1ee-103">Introduction to ASP.NET Core MVC on macOS, Linux, or Windows</span></span>

<span data-ttu-id="ed1ee-104">Von [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="ed1ee-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="ed1ee-105">Dieses Tutorial vermittelt Ihnen die Grundlagen zum Erstellen einer ASP.NET Core MVC-Web-App mithilfe von [Visual Studio Code](https://code.visualstudio.com) (VS Code).</span><span class="sxs-lookup"><span data-stu-id="ed1ee-105">This tutorial will teach you the basics of building an ASP.NET Core MVC web app using [Visual Studio Code](https://code.visualstudio.com) (VS Code).</span></span> <span data-ttu-id="ed1ee-106">Das Tutorial setzt voraus, dass Sie mit VS Code vertraut sind.</span><span class="sxs-lookup"><span data-stu-id="ed1ee-106">The tutorial assumes familarity with VS Code.</span></span> <span data-ttu-id="ed1ee-107">Weitere Informationen finden Sie unter [Erste Schritte mit VS Code](https://code.visualstudio.com/docs) und [Hilfe zu Visual Studio Code](#visual-studio-code-help).</span><span class="sxs-lookup"><span data-stu-id="ed1ee-107">See [Getting started with VS Code](https://code.visualstudio.com/docs) and [Visual Studio Code help](#visual-studio-code-help) for more information.</span></span> 

[!INCLUDE [consider RP](../../includes/razor.md)]

<span data-ttu-id="ed1ee-108">Es gibt drei Versionen dieses Tutorials:</span><span class="sxs-lookup"><span data-stu-id="ed1ee-108">There are 3 versions of this tutorial:</span></span>

* <span data-ttu-id="ed1ee-109">macOS: [Erstellen einer ASP.NET Core MVC-App mit Visual Studio für Mac](xref:tutorials/first-mvc-app-mac/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="ed1ee-109">macOS: [Create an ASP.NET Core MVC app with Visual Studio for Mac](xref:tutorials/first-mvc-app-mac/start-mvc)</span></span>
* <span data-ttu-id="ed1ee-110">Windows: [Erstellen einer ASP.NET Core MVC-App mit Visual Studio](xref:tutorials/first-mvc-app/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="ed1ee-110">Windows: [Create an ASP.NET Core MVC app with Visual Studio](xref:tutorials/first-mvc-app/start-mvc)</span></span>
* <span data-ttu-id="ed1ee-111">macOS, Linux und Windows: [Erstellen einer ASP.NET Core MVC-App mit Visual Studio Code](xref:tutorials/first-mvc-app-xplat/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="ed1ee-111">macOS, Linux, and Windows: [Create an ASP.NET Core MVC app with Visual Studio Code](xref:tutorials/first-mvc-app-xplat/start-mvc)</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="ed1ee-112">Erforderliche Komponenten</span><span class="sxs-lookup"><span data-stu-id="ed1ee-112">Prerequisites</span></span>

[!INCLUDE [](~/includes/net-core-prereqs-vscode.md)]

## <a name="create-a-web-app-with-dotnet"></a><span data-ttu-id="ed1ee-113">Erstellen einer Web-App mit dotnet</span><span class="sxs-lookup"><span data-stu-id="ed1ee-113">Create a web app with dotnet</span></span>

<span data-ttu-id="ed1ee-114">Führen Sie über ein Terminal die folgenden Befehle aus:</span><span class="sxs-lookup"><span data-stu-id="ed1ee-114">From a terminal, run the following commands:</span></span>

```console
mkdir MvcMovie
cd MvcMovie
dotnet new mvc
```

<span data-ttu-id="ed1ee-115">Öffnen den Ordner *MvcMovie* in Visual Studio Code (VS Code), und wählen Sie die Datei *Startup.cs* aus.</span><span class="sxs-lookup"><span data-stu-id="ed1ee-115">Open the *MvcMovie* folder in Visual Studio Code (VS Code) and select the *Startup.cs* file.</span></span>

- <span data-ttu-id="ed1ee-116">Klicken Sie auf **Ja**, wenn die **Warnung** „Required assets to build and debug are missing from „MvcMovie“. Add them?“ („Die erforderlichen Objekte für die Erstellung und das Debugging sind in „MvcMovie“ nicht vorhanden.</span><span class="sxs-lookup"><span data-stu-id="ed1ee-116">Select **Yes** to the **Warn** message "Required assets to build and debug are missing from 'MvcMovie'.</span></span> <span data-ttu-id="ed1ee-117">Sollen Sie hinzugefügt werden?“) angezeigt wird.</span><span class="sxs-lookup"><span data-stu-id="ed1ee-117">Add them?"</span></span>
- <span data-ttu-id="ed1ee-118">Klicken Sie in der **Infomeldung** „There are unresolved depedencies“ („Es bestehen ungelöste Abhängigkeiten“) auf **Wiederherstellen**.</span><span class="sxs-lookup"><span data-stu-id="ed1ee-118">Select **Restore** to the **Info** message "There are unresolved dependencies".</span></span>

![VS Code mit der Warnung „Required assets to build and debug are missing from „MvcMovie“. Add them?“ („Die erforderlichen Objekte für die Erstellung und das Debugging sind in „MvcMovie“ nicht vorhanden.](../web-api-vsc/_static/vsc_restore.png)

<span data-ttu-id="ed1ee-122">Drücken Sie **Debuggen** (F5), um das Programm zu erstellen und auszuführen.</span><span class="sxs-lookup"><span data-stu-id="ed1ee-122">Press **Debug** (F5) to build and run the program.</span></span>

![Ausgeführte App](../first-mvc-app/start-mvc/_static/1.png)

<span data-ttu-id="ed1ee-124">VS Code startet den [Kestrel](xref:fundamentals/servers/kestrel)-Webserver und führt Ihre App aus.</span><span class="sxs-lookup"><span data-stu-id="ed1ee-124">VS Code starts the [Kestrel](xref:fundamentals/servers/kestrel) web server and runs your app.</span></span> <span data-ttu-id="ed1ee-125">Beachten Sie, dass die Adressleiste `localhost:5000` und nicht etwas wie `example.com` anzeigt.</span><span class="sxs-lookup"><span data-stu-id="ed1ee-125">Notice that the address bar shows `localhost:5000` and not something like `example.com`.</span></span> <span data-ttu-id="ed1ee-126">Das liegt daran, dass es sich bei `localhost` um den Standardhostnamen für Ihren lokalen Computer handelt.</span><span class="sxs-lookup"><span data-stu-id="ed1ee-126">That's because `localhost` is the standard hostname for your local computer.</span></span>

<span data-ttu-id="ed1ee-127">Die Standardvorlage bietet Ihnen funktionierende Links für **Startseite, Info** und **Kontakt**.</span><span class="sxs-lookup"><span data-stu-id="ed1ee-127">The default template gives you working **Home, About** and **Contact** links.</span></span> <span data-ttu-id="ed1ee-128">Die obenstehende Browserabbildung zeigt diese Links nicht an.</span><span class="sxs-lookup"><span data-stu-id="ed1ee-128">The browser image above doesn't show these links.</span></span> <span data-ttu-id="ed1ee-129">Je nach Größe Ihres Browsers müssen Sie möglicherweise auf das Navigationssymbol klicken, um diese anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="ed1ee-129">Depending on the size of your browser, you might need to click the navigation icon to show them.</span></span>

![Navigationssymbol oben rechts](../first-mvc-app/start-mvc/_static/2.png)

<span data-ttu-id="ed1ee-131">Im nächsten Teil dieses Tutorials erfahren Sie mehr über MVC und beginnen mit dem Schreiben von Code.</span><span class="sxs-lookup"><span data-stu-id="ed1ee-131">In the next part of this tutorial, we'll learn about MVC and start writing some code.</span></span>

## <a name="visual-studio-code-help"></a><span data-ttu-id="ed1ee-132">Hilfe zu Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="ed1ee-132">Visual Studio Code help</span></span>

- [<span data-ttu-id="ed1ee-133">Erste Schritte</span><span class="sxs-lookup"><span data-stu-id="ed1ee-133">Getting started</span></span>](https://code.visualstudio.com/docs)
- [<span data-ttu-id="ed1ee-134">Debuggen</span><span class="sxs-lookup"><span data-stu-id="ed1ee-134">Debugging</span></span>](https://code.visualstudio.com/docs/editor/debugging)
- [<span data-ttu-id="ed1ee-135">Integriertes Terminal</span><span class="sxs-lookup"><span data-stu-id="ed1ee-135">Integrated terminal</span></span>](https://code.visualstudio.com/docs/editor/integrated-terminal)
- [<span data-ttu-id="ed1ee-136">Tastenkombinationen</span><span class="sxs-lookup"><span data-stu-id="ed1ee-136">Keyboard shortcuts</span></span>](https://code.visualstudio.com/docs/getstarted/keybindings#_keyboard-shortcuts-reference)

  - [<span data-ttu-id="ed1ee-137">macOS-Tastenkombinationen</span><span class="sxs-lookup"><span data-stu-id="ed1ee-137">macOS keyboard shortcuts</span></span>](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-macos.pdf)
  - [<span data-ttu-id="ed1ee-138">Linux-Tastenkombinationen</span><span class="sxs-lookup"><span data-stu-id="ed1ee-138">Linux keyboard shortcuts</span></span>](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-linux.pdf)
  - [<span data-ttu-id="ed1ee-139">Windows-Tastenkombinationen</span><span class="sxs-lookup"><span data-stu-id="ed1ee-139">Windows keyboard shortcuts</span></span>](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-windows.pdf)

> [!div class="step-by-step"]
> [<span data-ttu-id="ed1ee-140">Nächstes Tutorial: Hinzufügen eines Controllers</span><span class="sxs-lookup"><span data-stu-id="ed1ee-140">Next - Add a controller</span></span>](adding-controller.md)
