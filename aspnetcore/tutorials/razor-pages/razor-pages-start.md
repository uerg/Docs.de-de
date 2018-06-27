---
title: Erste Schritte mit Razor Pages in ASP.NET Core
author: rick-anderson
description: Erfahren Sie Grundlegendes zur Erstellung einer ASP.NET Core-Web-App mit Razor-Seiten. Razor-Seiten werden für Web-Workloads in ASP.NET Core empfohlen.
manager: wpickett
ms.author: riande
ms.date: 5/30/2018
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/razor-pages/razor-pages-start
ms.openlocfilehash: d7cdf7c8fac3b2ac1e526c6eeee8205068964ec9
ms.sourcegitcommit: 545ff5a632e2281035c1becec1f99137298e4f5c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/01/2018
ms.locfileid: "34582816"
---
# <a name="get-started-with-razor-pages-in-aspnet-core"></a><span data-ttu-id="ee660-104">Erste Schritte mit Razor Pages in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="ee660-104">Get started with Razor Pages in ASP.NET Core</span></span>

<span data-ttu-id="ee660-105">Von [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="ee660-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="ee660-106">Es wird empfohlen, die Version ASP.NET Core 2.1 dieses Tutorials zu verwenden.</span><span class="sxs-lookup"><span data-stu-id="ee660-106">We recommend you follow the ASP.NET Core 2.1 version of this tutorial.</span></span> <span data-ttu-id="ee660-107">Es ist **deutlich** einfacher, dieser Version zu folgen; darüber hinaus behandelt diese mehr Features.</span><span class="sxs-lookup"><span data-stu-id="ee660-107">It's **much** easier to follow and covers more features.</span></span> <span data-ttu-id="ee660-108">Wählen Sie in der Versionsauswahl **ASP.NET Core 2.1** aus.</span><span class="sxs-lookup"><span data-stu-id="ee660-108">Select **ASP.NET Core 2.1** in the version selector.</span></span>

![Versionsauswahl im Inhaltsverzeichnis](razor-pages-start/_static/v21.png)

::: moniker-end

<span data-ttu-id="ee660-110">In diesem Tutorial lernen Sie Grundlegendes zur Erstellung einer ASP.NET Core-Webapp mit Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="ee660-110">This tutorial teaches the basics of building an ASP.NET Core Razor Pages web app.</span></span> <span data-ttu-id="ee660-111">Razor Pages sind der empfohlene Weg für die Erstellung von Benutzeroberflächen für Web-Apps in ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="ee660-111">Razor Pages is the recommended way to build UI for web apps in ASP.NET Core.</span></span>

<span data-ttu-id="ee660-112">Es gibt drei Versionen dieses Tutorials:</span><span class="sxs-lookup"><span data-stu-id="ee660-112">There are three versions of this tutorial:</span></span>

* <span data-ttu-id="ee660-113">Windows: Dieses Tutorial</span><span class="sxs-lookup"><span data-stu-id="ee660-113">Windows: This tutorial</span></span>
* <span data-ttu-id="ee660-114">macOS: [Erste Schritte mit Razor-Seiten in ASP.NET Core mit Visual Studio für Mac](xref:tutorials/razor-pages-mac/razor-pages-start)</span><span class="sxs-lookup"><span data-stu-id="ee660-114">MacOS: [Get started with Razor Pages with Visual Studio for Mac](xref:tutorials/razor-pages-mac/razor-pages-start)</span></span>
* <span data-ttu-id="ee660-115">macOS, Linux und Windows: [Erste Schritte mit Razor-Seiten in ASP.NET Core mit Visual Studio Code](xref:tutorials/razor-pages-vsc/razor-pages-start)</span><span class="sxs-lookup"><span data-stu-id="ee660-115">macOS, Linux, and Windows: [Get started with ASP.NET Core Razor Pages in Visual Studio Code](xref:tutorials/razor-pages-vsc/razor-pages-start)</span></span>

<span data-ttu-id="ee660-116">[Anzeigen oder Herunterladen von Beispielcode](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample) ([Vorgehensweise zum Herunterladen](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="ee660-116">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

::: moniker range=">= aspnetcore-2.1"

## <a name="prerequisites"></a><span data-ttu-id="ee660-117">Erforderliche Komponenten</span><span class="sxs-lookup"><span data-stu-id="ee660-117">Prerequisites</span></span>

<span data-ttu-id="ee660-118">[!INCLUDE [](~/includes/net-core-prereqs-windows.md) [](~/includes/net-core-prereqs-windows.md)]</span><span class="sxs-lookup"><span data-stu-id="ee660-118">[!INCLUDE [](~/includes/net-core-prereqs-windows.md) [](~/includes/net-core-prereqs-windows.md)]</span></span>

## <a name="create-a-razor-web-app"></a><span data-ttu-id="ee660-119">Erstellen einer Razor-Web-App</span><span class="sxs-lookup"><span data-stu-id="ee660-119">Create a Razor web app</span></span>

* <span data-ttu-id="ee660-120">Wählen Sie in Visual Studio im Menü **Datei** die Option **Neu** > **Projekt** aus.</span><span class="sxs-lookup"><span data-stu-id="ee660-120">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="ee660-121">Erstellen Sie eine neue ASP.NET Core-Webanwendung.</span><span class="sxs-lookup"><span data-stu-id="ee660-121">Create a new ASP.NET Core Web Application.</span></span> <span data-ttu-id="ee660-122">Nennen Sie das Projekt **RazorPagesMovie**.</span><span class="sxs-lookup"><span data-stu-id="ee660-122">Name the project **RazorPagesMovie**.</span></span> <span data-ttu-id="ee660-123">Es ist wichtig, den Namen *RazorPagesMovie* zu verwenden, damit die Namespaces übereinstimmen, wenn Sie Code kopieren/einfügen.</span><span class="sxs-lookup"><span data-stu-id="ee660-123">It's important to name the project *RazorPagesMovie* so the namespaces will match when you copy/paste code.</span></span>
 <span data-ttu-id="ee660-124">![neue ASP.NET Core-Webanwendung](razor-pages-start/_static/np_2.1.png)</span><span class="sxs-lookup"><span data-stu-id="ee660-124">![new ASP.NET Core Web Application](razor-pages-start/_static/np_2.1.png)</span></span>
* <span data-ttu-id="ee660-125">Wählen Sie in der Dropdownliste **ASP.NET Core 2.1** und anschließend **Webanwendung** aus.</span><span class="sxs-lookup"><span data-stu-id="ee660-125">Select **ASP.NET Core 2.1** in the dropdown, and then select **Web Application**.</span></span>

 ![neue ASP.NET Core-Webanwendung](razor-pages-start/_static/np_2_2.1.png)

<span data-ttu-id="ee660-127">Die Visual Studio-Vorlage erstellt ein Startprojekt:</span><span class="sxs-lookup"><span data-stu-id="ee660-127">The Visual Studio template creates a starter project:</span></span>

![Projektmappen-Explorer](razor-pages-start/_static/se2.1.png)

<span data-ttu-id="ee660-129">Drücken Sie **F5**, um die App im Debugmodus auszuführen, oder **STRG + F5**, um die App ohne Anfügen des Debuggers auszuführen.</span><span class="sxs-lookup"><span data-stu-id="ee660-129">Press **F5** to run the app in debug mode or **Ctrl-F5** to run without attaching the debugger.</span></span> <span data-ttu-id="ee660-130">Wählen Sie **Akzeptieren** aus, um der Nachverfolgung zuzustimmen.</span><span class="sxs-lookup"><span data-stu-id="ee660-130">Select **Accept** to consent to tracking.</span></span> <span data-ttu-id="ee660-131">Diese App verfolgt keine personenbezogenen Informationen nach.</span><span class="sxs-lookup"><span data-stu-id="ee660-131">This app doesn't track personal information.</span></span> <span data-ttu-id="ee660-132">Der generierte Vorlagencode enthält Objekte, die bei der Erfüllung der [Datenschutz-Grundverordnung (DSGVO)](xref:security/gdpr) als Unterstützung dienen sollen.</span><span class="sxs-lookup"><span data-stu-id="ee660-132">The template generated code includes assets to help meet [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

![Start- oder Indexseite](razor-pages-start/_static/homeGDPR.png)

<span data-ttu-id="ee660-134">Die folgende Abbildung zeigt die App, nachdem die Nachverfolgung akzeptiert wurde:</span><span class="sxs-lookup"><span data-stu-id="ee660-134">The following image shows the app after accepting tracking:</span></span>

![Start- oder Indexseite](razor-pages-start/_static/home2.1.png)

* <span data-ttu-id="ee660-136">Visual Studio startet [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) und führt die App aus.</span><span class="sxs-lookup"><span data-stu-id="ee660-136">Visual Studio starts [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs the app.</span></span> <span data-ttu-id="ee660-137">Die Adressleiste zeigt `localhost:port#` an, nicht `example.com`.</span><span class="sxs-lookup"><span data-stu-id="ee660-137">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="ee660-138">Das liegt daran, dass es sich bei `localhost` um den Standardhostnamen für Ihren lokalen Computer handelt.</span><span class="sxs-lookup"><span data-stu-id="ee660-138">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="ee660-139">„Localhost“ dient nur Webanforderungen vom lokalen Computer.</span><span class="sxs-lookup"><span data-stu-id="ee660-139">Localhost only serves web requests from the local computer.</span></span> <span data-ttu-id="ee660-140">Wenn in Visual Studio ein Webprojekt erstellt wird, wird für den Webserver ein zufälliger Port verwendet.</span><span class="sxs-lookup"><span data-stu-id="ee660-140">When Visual Studio creates a web project, a random port is used for the web server.</span></span> <span data-ttu-id="ee660-141">In der vorherigen Abbildung ist die Nummer des Ports 5000.</span><span class="sxs-lookup"><span data-stu-id="ee660-141">In the preceding image, the port number is 5000.</span></span> <span data-ttu-id="ee660-142">Wenn Sie die App ausführen, wird eine andere Portnummer angezeigt.</span><span class="sxs-lookup"><span data-stu-id="ee660-142">When you run the app, you'll see a different port number.</span></span>
* <span data-ttu-id="ee660-143">Das Starten der App mit **STRG+F5** (Nicht-Debugmodus) ermöglicht die Änderung des Codes, das Speichern der Datei, das Aktualisieren des Browsers und das Anzeigen von Codeänderungen.</span><span class="sxs-lookup"><span data-stu-id="ee660-143">Launching the app with **Ctrl+F5** (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="ee660-144">Viele Entwickler bevorzugen den Nicht-Debugmodus, um die App schnell zu starten und Änderungen anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="ee660-144">Many developers prefer to use non-debug mode to quickly launch the app and view changes.</span></span>

[!INCLUDE [razor-pages-start](~/includes/RP/2.1/razor-pages-start.md)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

## <a name="prerequisites"></a><span data-ttu-id="ee660-145">Erforderliche Komponenten</span><span class="sxs-lookup"><span data-stu-id="ee660-145">Prerequisites</span></span>

<span data-ttu-id="ee660-146">[!INCLUDE [](~/includes/net-core-prereqs-windows.md) [](~/includes/net-core-prereqs-windows.md)]</span><span class="sxs-lookup"><span data-stu-id="ee660-146">[!INCLUDE [](~/includes/net-core-prereqs-windows.md) [](~/includes/net-core-prereqs-windows.md)]</span></span>

## <a name="create-a-razor-web-app"></a><span data-ttu-id="ee660-147">Erstellen einer Razor-Web-App</span><span class="sxs-lookup"><span data-stu-id="ee660-147">Create a Razor web app</span></span>

* <span data-ttu-id="ee660-148">Wählen Sie in Visual Studio im Menü **Datei** die Option **Neu** > **Projekt** aus.</span><span class="sxs-lookup"><span data-stu-id="ee660-148">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="ee660-149">Erstellen Sie eine neue ASP.NET Core-Webanwendung.</span><span class="sxs-lookup"><span data-stu-id="ee660-149">Create a new ASP.NET Core Web Application.</span></span> <span data-ttu-id="ee660-150">Nennen Sie das Projekt **RazorPagesMovie**.</span><span class="sxs-lookup"><span data-stu-id="ee660-150">Name the project **RazorPagesMovie**.</span></span> <span data-ttu-id="ee660-151">Es ist wichtig, den Namen *RazorPagesMovie* zu verwenden, damit die Namespaces übereinstimmen, wenn Sie Code kopieren/einfügen.</span><span class="sxs-lookup"><span data-stu-id="ee660-151">It's important to name the project *RazorPagesMovie* so the namespaces will match when you copy/paste code.</span></span>
  <span data-ttu-id="ee660-152">![neue ASP.NET Core-Webanwendung](../../mvc/razor-pages/index/_static/np.png)</span><span class="sxs-lookup"><span data-stu-id="ee660-152">![new ASP.NET Core Web Application](../../mvc/razor-pages/index/_static/np.png)</span></span>
* <span data-ttu-id="ee660-153">Wählen Sie in der Dropdownliste **ASP.NET Core 2.0** aus, und klicken Sie anschließend auf **Webanwendung**.</span><span class="sxs-lookup"><span data-stu-id="ee660-153">Select **ASP.NET Core 2.0** in the dropdown, and then select **Web Application**.</span></span>

  [!INCLUDE [install 2.0](~/includes/dotnetcore-on-dotnetfx-vs.md)]

<span data-ttu-id="ee660-154">Die Visual Studio-Vorlage erstellt ein Startprojekt:</span><span class="sxs-lookup"><span data-stu-id="ee660-154">The Visual Studio template creates a starter project:</span></span>

![Projektmappen-Explorer](razor-pages-start/_static/se.png)

<span data-ttu-id="ee660-156">Drücken Sie **F5**, um die App im Debugmodus auszuführen, oder **STRG+F5** zur Ausführung ohne Anfügen des Debuggers.</span><span class="sxs-lookup"><span data-stu-id="ee660-156">Press **F5** to run the app in debug mode or **Ctrl-F5** to run without attaching the debugger</span></span>

![Start- oder Indexseite](razor-pages-start/_static/home.png)

* <span data-ttu-id="ee660-158">Visual Studio startet [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) und führt Ihre App aus.</span><span class="sxs-lookup"><span data-stu-id="ee660-158">Visual Studio starts [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs your app.</span></span> <span data-ttu-id="ee660-159">Die Adressleiste zeigt `localhost:port#` an, nicht `example.com`.</span><span class="sxs-lookup"><span data-stu-id="ee660-159">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="ee660-160">Das liegt daran, dass es sich bei `localhost` um den Standardhostnamen für Ihren lokalen Computer handelt.</span><span class="sxs-lookup"><span data-stu-id="ee660-160">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="ee660-161">„Localhost“ dient nur Webanforderungen vom lokalen Computer.</span><span class="sxs-lookup"><span data-stu-id="ee660-161">Localhost only serves web requests from the local computer.</span></span> <span data-ttu-id="ee660-162">Wenn in Visual Studio ein Webprojekt erstellt wird, wird für den Webserver ein zufälliger Port verwendet.</span><span class="sxs-lookup"><span data-stu-id="ee660-162">When Visual Studio creates a web project, a random port is used for the web server.</span></span> <span data-ttu-id="ee660-163">In der vorherigen Abbildung ist die Nummer des Ports 5000.</span><span class="sxs-lookup"><span data-stu-id="ee660-163">In the preceding image, the port number is 5000.</span></span> <span data-ttu-id="ee660-164">Wenn Sie die App ausführen, wird eine andere Portnummer angezeigt.</span><span class="sxs-lookup"><span data-stu-id="ee660-164">When you run the app, you'll see a different port number.</span></span>
* <span data-ttu-id="ee660-165">Das Starten der App mit **STRG+F5** (Nicht-Debugmodus) ermöglicht die Änderung des Codes, das Speichern der Datei, das Aktualisieren des Browsers und das Anzeigen von Codeänderungen.</span><span class="sxs-lookup"><span data-stu-id="ee660-165">Launching the app with **Ctrl+F5** (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="ee660-166">Viele Entwickler bevorzugen den Nicht-Debugmodus, um die App schnell zu starten und Änderungen anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="ee660-166">Many developers prefer to use non-debug mode to quickly launch the app and view changes.</span></span>

[!INCLUDE [razor-pages-start](~/includes/RP/2.1/razor-pages-start.md)]

::: moniker-end

> [!div class="step-by-step"]
> [<span data-ttu-id="ee660-167">Nächstes Tutorial: Adding a model to a Razor Pages app in ASP.NET Core with Visual Studio for Mac (Hinzufügen eines Modells zu einer App mit Razor Pages in ASP.NET Core mit Visual Studio für Mac)</span><span class="sxs-lookup"><span data-stu-id="ee660-167">Next: Adding a model</span></span>](xref:tutorials/razor-pages/model)
