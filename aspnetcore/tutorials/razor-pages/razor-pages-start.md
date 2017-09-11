---
title: Erste Schritte mit Razor-Seiten mit ASP.NET Core
author: rick-anderson
description: Erste Schritte mit Razor-Seiten mit ASP.NET Core
keywords: ASP.NET Core, Razor-Seiten, Razor, MVC
ms.author: riande
manager: wpickett
ms.date: 8/27/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: aspnet-core
uid: tutorials/razor-pages/razor-pages-start
ms.openlocfilehash: c22ee2554992d15df2f6b92ee5da6805ab80b73a
ms.sourcegitcommit: d9ec19e5452af83648074db5d96c0a0f4f9e7f9a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/29/2017
---
# <a name="getting-started-with-razor-pages-in-aspnet-core"></a><span data-ttu-id="f0fd8-104">Erste Schritte mit Razor-Seiten mit ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="f0fd8-104">Getting started with Razor Pages in ASP.NET Core</span></span>

<span data-ttu-id="f0fd8-105">Von [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="f0fd8-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="f0fd8-106">In diesem Tutorial lernen Sie grundlegendes zur Erstellung einer ASP.NET Core-Web-App mit Razor-Seiten.</span><span class="sxs-lookup"><span data-stu-id="f0fd8-106">This tutorial teaches the basics of building an ASP.NET Core Razor Pages web app.</span></span> <span data-ttu-id="f0fd8-107">Wir empfehlen Ihnen, zuerst das Tutorial [Introduction to Razor Pages (Einführung in Razor-Seiten)](xref:mvc/razor-pages/index) durchzuarbeiten, bevor Sie mit diesem Tutorial beginnen.</span><span class="sxs-lookup"><span data-stu-id="f0fd8-107">We recommend you complete [Introduction to Razor Pages](xref:mvc/razor-pages/index) before starting this tutorial.</span></span> <span data-ttu-id="f0fd8-108">Razor-Pages sind der empfohlene Weg für die Erstellung von Webanwendungs-UI in ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="f0fd8-108">Razor Pages is the recommended way to build UI for web applications in ASP.NET Core.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="f0fd8-109">Erforderliche Komponenten</span><span class="sxs-lookup"><span data-stu-id="f0fd8-109">Prerequisites</span></span>

[!INCLUDE[install 2.0](../../includes/install2.0.md)]

## <a name="create-a-razor-web-app"></a><span data-ttu-id="f0fd8-110">Erstellen einer Razor-Web-App</span><span class="sxs-lookup"><span data-stu-id="f0fd8-110">Create a Razor web app</span></span>

* <span data-ttu-id="f0fd8-111">Klicken Sie in Visual Studio im Menü **Datei** auf **Neu > Projekt**.</span><span class="sxs-lookup"><span data-stu-id="f0fd8-111">From the Visual Studio **File** menu, select **New > Project**.</span></span>
* <span data-ttu-id="f0fd8-112">Erstellen Sie eine neue ASP.NET Core-Webanwendung.</span><span class="sxs-lookup"><span data-stu-id="f0fd8-112">Create a new ASP.NET Core Web Application.</span></span> <span data-ttu-id="f0fd8-113">Nennen Sie das Projekt **RazorPagesMovie**.</span><span class="sxs-lookup"><span data-stu-id="f0fd8-113">Name the project **RazorPagesMovie**.</span></span> <span data-ttu-id="f0fd8-114">Es ist wichtig, den Namen *RazorPagesMovie* zu verwenden, damit die Namespaces übereinstimmen, wenn Sie Code kopieren/einfügen.</span><span class="sxs-lookup"><span data-stu-id="f0fd8-114">It's important to name the project *RazorPagesMovie* so the namespaces will match when you copy/paste code.</span></span>
 <span data-ttu-id="f0fd8-115">![neue ASP.NET Core-Webanwendung](../../mvc/razor-pages/index/_static/np.png)</span><span class="sxs-lookup"><span data-stu-id="f0fd8-115">![new ASP.NET Core Web Application](../../mvc/razor-pages/index/_static/np.png)</span></span>
* <span data-ttu-id="f0fd8-116">Wählen Sie in der Dropdownliste **ASP.NET Core 2.0** aus, und klicken Sie anschließend auf **Webanwendung**.</span><span class="sxs-lookup"><span data-stu-id="f0fd8-116">Select **ASP.NET Core 2.0** in the dropdown, and then select **Web Application**.</span></span>
 <span data-ttu-id="f0fd8-117">![Webanwendung (Razor-Seiten)](../../mvc/razor-pages/index/_static/np2.png)</span><span class="sxs-lookup"><span data-stu-id="f0fd8-117">![Web Application (Razor Pages)](../../mvc/razor-pages/index/_static/np2.png)</span></span>

<span data-ttu-id="f0fd8-118">Die Visual Studio-Vorlage erstellt ein Startprojekt:</span><span class="sxs-lookup"><span data-stu-id="f0fd8-118">The Visual Studio template creates a starter project:</span></span>

![Projektmappen-Explorer](razor-pages-start/_static/se.png)

<span data-ttu-id="f0fd8-120">Drücken Sie **F5**, um die App im Debugmodus auszuführen, oder **STRG+F5** zur Ausführung ohne Anfügen des Debuggers.</span><span class="sxs-lookup"><span data-stu-id="f0fd8-120">Press **F5** to run the app in debug mode or **Ctrl-F5** to run without attaching the debugger</span></span>

![Seite „Home“ oder „Index“](razor-pages-start/_static/home.png)

* <span data-ttu-id="f0fd8-122">Visual Studio startet [IIS Express](https://docs.microsoft.com/iis/extensions/introduction-to-iis-express/iis-express-overview) und führt Ihre App aus.</span><span class="sxs-lookup"><span data-stu-id="f0fd8-122">Visual Studio starts [IIS Express](https://docs.microsoft.com/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs your app.</span></span> <span data-ttu-id="f0fd8-123">Die Adressleiste zeigt `localhost:port#` an, nicht `example.com`.</span><span class="sxs-lookup"><span data-stu-id="f0fd8-123">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="f0fd8-124">Das liegt daran, dass es sich bei `localhost` um den Standard-Hostnamen für Ihren lokalen Computer handelt.</span><span class="sxs-lookup"><span data-stu-id="f0fd8-124">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="f0fd8-125">„Localhost“ dient nur Webanforderungen vom lokalen Computer.</span><span class="sxs-lookup"><span data-stu-id="f0fd8-125">Localhost only serves web requests from the local computer.</span></span> <span data-ttu-id="f0fd8-126">Wenn in Visual Studio ein Webprojekt erstellt wird, wird für den Webserver ein zufälliger Port verwendet.</span><span class="sxs-lookup"><span data-stu-id="f0fd8-126">When Visual Studio creates a web project, a random port is used for the web server.</span></span> <span data-ttu-id="f0fd8-127">In der vorherigen Abbildung ist die Nummer des Ports 5000.</span><span class="sxs-lookup"><span data-stu-id="f0fd8-127">In the preceding image, the port number is 5000.</span></span> <span data-ttu-id="f0fd8-128">Wenn Sie die App ausführen, wird eine andere Portnummer angezeigt.</span><span class="sxs-lookup"><span data-stu-id="f0fd8-128">When you run the app, you'll see a different port number.</span></span>
* <span data-ttu-id="f0fd8-129">Das Starten der Anwendung mit **STRG+F5** (Nicht-Debugmodus) ermöglicht die Änderung des Codes, das Speichern der Datei, das Aktualisieren des Browsers und das Anzeigen von Codeänderungen.</span><span class="sxs-lookup"><span data-stu-id="f0fd8-129">Launching the app with **Ctrl+F5** (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="f0fd8-130">Viele Entwickler bevorzugen den Nicht-Debugmodus, um die App schnell zu starten und Änderungen anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="f0fd8-130">Many developers prefer to use non-debug mode to quickly launch the app and view changes.</span></span>

[!INCLUDE[razor-pages-start](../../includes/RP/razor-pages-start.md)]

>[!div class="step-by-step"]
[<span data-ttu-id="f0fd8-131">Nächstes Tutorial: Adding a model to a Razor Pages app in ASP.NET Core with Visual Studio for Mac (Hinzufügen eines Modells zu einer App mit Razor-Seiten in ASP.NET Core mit Visual Studio für Mac)</span><span class="sxs-lookup"><span data-stu-id="f0fd8-131">Next: Adding a model</span></span>](xref:tutorials/razor-pages/modelz)  
