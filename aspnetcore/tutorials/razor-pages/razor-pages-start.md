---
title: Erste Schritte mit Razor Pages in ASP.NET Core
author: rick-anderson
description: Erfahren Sie Grundlegendes zur Erstellung einer ASP.NET Core-Web-App mit Razor-Seiten. Razor-Seiten werden für Web-Workloads in ASP.NET Core empfohlen.
ms.author: riande
ms.date: 05/30/2018
uid: tutorials/razor-pages/razor-pages-start
ms.openlocfilehash: e317b49f2ad33c392de33bc32a87f67bb8cb72a0
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/20/2018
ms.locfileid: "36278043"
---
# <a name="get-started-with-razor-pages-in-aspnet-core"></a><span data-ttu-id="ef50d-104">Erste Schritte mit Razor Pages in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="ef50d-104">Get started with Razor Pages in ASP.NET Core</span></span>

<span data-ttu-id="ef50d-105">Von [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="ef50d-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="ef50d-106">Es wird empfohlen, die Version ASP.NET Core 2.1 dieses Tutorials zu verwenden.</span><span class="sxs-lookup"><span data-stu-id="ef50d-106">We recommend you follow the ASP.NET Core 2.1 version of this tutorial.</span></span> <span data-ttu-id="ef50d-107">Es ist **deutlich** einfacher, dieser Version zu folgen; darüber hinaus behandelt diese mehr Features.</span><span class="sxs-lookup"><span data-stu-id="ef50d-107">It's **much** easier to follow and covers more features.</span></span> <span data-ttu-id="ef50d-108">Wählen Sie in der Versionsauswahl **ASP.NET Core 2.1** aus.</span><span class="sxs-lookup"><span data-stu-id="ef50d-108">Select **ASP.NET Core 2.1** in the version selector.</span></span>

![Versionsauswahl im Inhaltsverzeichnis](razor-pages-start/_static/v21.png)

::: moniker-end

<span data-ttu-id="ef50d-110">In diesem Tutorial lernen Sie Grundlegendes zur Erstellung einer ASP.NET Core-Webapp mit Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="ef50d-110">This tutorial teaches the basics of building an ASP.NET Core Razor Pages web app.</span></span> <span data-ttu-id="ef50d-111">Razor Pages sind der empfohlene Weg für die Erstellung von Benutzeroberflächen für Web-Apps in ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="ef50d-111">Razor Pages is the recommended way to build UI for web apps in ASP.NET Core.</span></span>

<span data-ttu-id="ef50d-112">Es gibt drei Versionen dieses Tutorials:</span><span class="sxs-lookup"><span data-stu-id="ef50d-112">There are three versions of this tutorial:</span></span>

* <span data-ttu-id="ef50d-113">Windows: Dieses Tutorial</span><span class="sxs-lookup"><span data-stu-id="ef50d-113">Windows: This tutorial</span></span>
* <span data-ttu-id="ef50d-114">macOS: [Erste Schritte mit Razor-Seiten in ASP.NET Core mit Visual Studio für Mac](xref:tutorials/razor-pages-mac/razor-pages-start)</span><span class="sxs-lookup"><span data-stu-id="ef50d-114">MacOS: [Get started with Razor Pages with Visual Studio for Mac](xref:tutorials/razor-pages-mac/razor-pages-start)</span></span>
* <span data-ttu-id="ef50d-115">macOS, Linux und Windows: [Erste Schritte mit Razor-Seiten in ASP.NET Core mit Visual Studio Code](xref:tutorials/razor-pages-vsc/razor-pages-start)</span><span class="sxs-lookup"><span data-stu-id="ef50d-115">macOS, Linux, and Windows: [Get started with ASP.NET Core Razor Pages in Visual Studio Code](xref:tutorials/razor-pages-vsc/razor-pages-start)</span></span>

<span data-ttu-id="ef50d-116">[Anzeigen oder Herunterladen von Beispielcode](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample) ([Vorgehensweise zum Herunterladen](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="ef50d-116">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

::: moniker range=">= aspnetcore-2.1"

## <a name="prerequisites"></a><span data-ttu-id="ef50d-117">Erforderliche Komponenten</span><span class="sxs-lookup"><span data-stu-id="ef50d-117">Prerequisites</span></span>

<span data-ttu-id="ef50d-118">[!INCLUDE [](~/includes/net-core-prereqs-windows.md) [](~/includes/net-core-prereqs-windows.md)]</span><span class="sxs-lookup"><span data-stu-id="ef50d-118">[!INCLUDE [](~/includes/net-core-prereqs-windows.md) [](~/includes/net-core-prereqs-windows.md)]</span></span>

## <a name="create-a-razor-web-app"></a><span data-ttu-id="ef50d-119">Erstellen einer Razor-Web-App</span><span class="sxs-lookup"><span data-stu-id="ef50d-119">Create a Razor web app</span></span>

* <span data-ttu-id="ef50d-120">Wählen Sie in Visual Studio im Menü **Datei** die Option **Neu** > **Projekt** aus.</span><span class="sxs-lookup"><span data-stu-id="ef50d-120">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="ef50d-121">Erstellen Sie eine neue ASP.NET Core-Webanwendung.</span><span class="sxs-lookup"><span data-stu-id="ef50d-121">Create a new ASP.NET Core Web Application.</span></span> <span data-ttu-id="ef50d-122">Nennen Sie das Projekt **RazorPagesMovie**.</span><span class="sxs-lookup"><span data-stu-id="ef50d-122">Name the project **RazorPagesMovie**.</span></span> <span data-ttu-id="ef50d-123">Es ist wichtig, den Namen *RazorPagesMovie* zu verwenden, damit die Namespaces übereinstimmen, wenn Sie Code kopieren/einfügen.</span><span class="sxs-lookup"><span data-stu-id="ef50d-123">It's important to name the project *RazorPagesMovie* so the namespaces will match when you copy/paste code.</span></span>
 <span data-ttu-id="ef50d-124">![neue ASP.NET Core-Webanwendung](razor-pages-start/_static/np_2.1.png)</span><span class="sxs-lookup"><span data-stu-id="ef50d-124">![new ASP.NET Core Web Application](razor-pages-start/_static/np_2.1.png)</span></span>
* <span data-ttu-id="ef50d-125">Wählen Sie in der Dropdownliste **ASP.NET Core 2.1** und anschließend **Webanwendung** aus.</span><span class="sxs-lookup"><span data-stu-id="ef50d-125">Select **ASP.NET Core 2.1** in the dropdown, and then select **Web Application**.</span></span>

 ![neue ASP.NET Core-Webanwendung](razor-pages-start/_static/np_2_2.1.png)

<span data-ttu-id="ef50d-127">Die Visual Studio-Vorlage erstellt ein Startprojekt:</span><span class="sxs-lookup"><span data-stu-id="ef50d-127">The Visual Studio template creates a starter project:</span></span>

![Projektmappen-Explorer](razor-pages-start/_static/se2.1.png)

<span data-ttu-id="ef50d-129">Drücken Sie **F5**, um die App im Debugmodus auszuführen, oder **STRG + F5**, um die App ohne Anfügen des Debuggers auszuführen.</span><span class="sxs-lookup"><span data-stu-id="ef50d-129">Press **F5** to run the app in debug mode or **Ctrl-F5** to run without attaching the debugger.</span></span> <span data-ttu-id="ef50d-130">Wählen Sie **Akzeptieren** aus, um der Nachverfolgung zuzustimmen.</span><span class="sxs-lookup"><span data-stu-id="ef50d-130">Select **Accept** to consent to tracking.</span></span> <span data-ttu-id="ef50d-131">Diese App verfolgt keine personenbezogenen Informationen nach.</span><span class="sxs-lookup"><span data-stu-id="ef50d-131">This app doesn't track personal information.</span></span> <span data-ttu-id="ef50d-132">Der generierte Vorlagencode enthält Objekte, die bei der Erfüllung der [Datenschutz-Grundverordnung (DSGVO)](xref:security/gdpr) als Unterstützung dienen sollen.</span><span class="sxs-lookup"><span data-stu-id="ef50d-132">The template generated code includes assets to help meet [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

![Start- oder Indexseite](razor-pages-start/_static/homeGDPR.png)

<span data-ttu-id="ef50d-134">Die folgende Abbildung zeigt die App, nachdem die Nachverfolgung akzeptiert wurde:</span><span class="sxs-lookup"><span data-stu-id="ef50d-134">The following image shows the app after accepting tracking:</span></span>

![Start- oder Indexseite](razor-pages-start/_static/home2.1.png)

* <span data-ttu-id="ef50d-136">Visual Studio startet [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) und führt die App aus.</span><span class="sxs-lookup"><span data-stu-id="ef50d-136">Visual Studio starts [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs the app.</span></span> <span data-ttu-id="ef50d-137">Die Adressleiste zeigt `localhost:port#` an, nicht `example.com`.</span><span class="sxs-lookup"><span data-stu-id="ef50d-137">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="ef50d-138">Das liegt daran, dass es sich bei `localhost` um den Standardhostnamen für Ihren lokalen Computer handelt.</span><span class="sxs-lookup"><span data-stu-id="ef50d-138">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="ef50d-139">„Localhost“ dient nur Webanforderungen vom lokalen Computer.</span><span class="sxs-lookup"><span data-stu-id="ef50d-139">Localhost only serves web requests from the local computer.</span></span> <span data-ttu-id="ef50d-140">Wenn in Visual Studio ein Webprojekt erstellt wird, wird für den Webserver ein zufälliger Port verwendet.</span><span class="sxs-lookup"><span data-stu-id="ef50d-140">When Visual Studio creates a web project, a random port is used for the web server.</span></span> <span data-ttu-id="ef50d-141">In der vorherigen Abbildung ist die Nummer des Ports 5000.</span><span class="sxs-lookup"><span data-stu-id="ef50d-141">In the preceding image, the port number is 5000.</span></span> <span data-ttu-id="ef50d-142">Wenn Sie die App ausführen, wird eine andere Portnummer angezeigt.</span><span class="sxs-lookup"><span data-stu-id="ef50d-142">When you run the app, you'll see a different port number.</span></span>
* <span data-ttu-id="ef50d-143">Das Starten der App mit **STRG+F5** (Nicht-Debugmodus) ermöglicht die Änderung des Codes, das Speichern der Datei, das Aktualisieren des Browsers und das Anzeigen von Codeänderungen.</span><span class="sxs-lookup"><span data-stu-id="ef50d-143">Launching the app with **Ctrl+F5** (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="ef50d-144">Viele Entwickler bevorzugen den Nicht-Debugmodus, um die App schnell zu starten und Änderungen anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="ef50d-144">Many developers prefer to use non-debug mode to quickly launch the app and view changes.</span></span>

[!INCLUDE [razor-pages-start](~/includes/RP/2.1/razor-pages-start.md)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

## <a name="prerequisites"></a><span data-ttu-id="ef50d-145">Erforderliche Komponenten</span><span class="sxs-lookup"><span data-stu-id="ef50d-145">Prerequisites</span></span>

<span data-ttu-id="ef50d-146">[!INCLUDE [](~/includes/net-core-prereqs-windows.md) [](~/includes/net-core-prereqs-windows.md)]</span><span class="sxs-lookup"><span data-stu-id="ef50d-146">[!INCLUDE [](~/includes/net-core-prereqs-windows.md) [](~/includes/net-core-prereqs-windows.md)]</span></span>

## <a name="create-a-razor-web-app"></a><span data-ttu-id="ef50d-147">Erstellen einer Razor-Web-App</span><span class="sxs-lookup"><span data-stu-id="ef50d-147">Create a Razor web app</span></span>

* <span data-ttu-id="ef50d-148">Wählen Sie in Visual Studio im Menü **Datei** die Option **Neu** > **Projekt** aus.</span><span class="sxs-lookup"><span data-stu-id="ef50d-148">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="ef50d-149">Erstellen Sie eine neue ASP.NET Core-Webanwendung.</span><span class="sxs-lookup"><span data-stu-id="ef50d-149">Create a new ASP.NET Core Web Application.</span></span> <span data-ttu-id="ef50d-150">Nennen Sie das Projekt **RazorPagesMovie**.</span><span class="sxs-lookup"><span data-stu-id="ef50d-150">Name the project **RazorPagesMovie**.</span></span> <span data-ttu-id="ef50d-151">Es ist wichtig, den Namen *RazorPagesMovie* zu verwenden, damit die Namespaces übereinstimmen, wenn Sie Code kopieren/einfügen.</span><span class="sxs-lookup"><span data-stu-id="ef50d-151">It's important to name the project *RazorPagesMovie* so the namespaces will match when you copy/paste code.</span></span>
  <span data-ttu-id="ef50d-152">![neue ASP.NET Core-Webanwendung](../../razor-pages/index/_static/np.png)</span><span class="sxs-lookup"><span data-stu-id="ef50d-152">![new ASP.NET Core Web Application](../../razor-pages/index/_static/np.png)</span></span>
* <span data-ttu-id="ef50d-153">Wählen Sie in der Dropdownliste **ASP.NET Core 2.0** aus, und klicken Sie anschließend auf **Webanwendung**.</span><span class="sxs-lookup"><span data-stu-id="ef50d-153">Select **ASP.NET Core 2.0** in the dropdown, and then select **Web Application**.</span></span>

  [!INCLUDE [install 2.0](~/includes/dotnetcore-on-dotnetfx-vs.md)]

<span data-ttu-id="ef50d-154">Die Visual Studio-Vorlage erstellt ein Startprojekt:</span><span class="sxs-lookup"><span data-stu-id="ef50d-154">The Visual Studio template creates a starter project:</span></span>

![Projektmappen-Explorer](razor-pages-start/_static/se.png)

<span data-ttu-id="ef50d-156">Drücken Sie **F5**, um die App im Debugmodus auszuführen, oder **STRG+F5** zur Ausführung ohne Anfügen des Debuggers.</span><span class="sxs-lookup"><span data-stu-id="ef50d-156">Press **F5** to run the app in debug mode or **Ctrl-F5** to run without attaching the debugger</span></span>

![Start- oder Indexseite](razor-pages-start/_static/home.png)

* <span data-ttu-id="ef50d-158">Visual Studio startet [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) und führt Ihre App aus.</span><span class="sxs-lookup"><span data-stu-id="ef50d-158">Visual Studio starts [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs your app.</span></span> <span data-ttu-id="ef50d-159">Die Adressleiste zeigt `localhost:port#` an, nicht `example.com`.</span><span class="sxs-lookup"><span data-stu-id="ef50d-159">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="ef50d-160">Das liegt daran, dass es sich bei `localhost` um den Standardhostnamen für Ihren lokalen Computer handelt.</span><span class="sxs-lookup"><span data-stu-id="ef50d-160">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="ef50d-161">„Localhost“ dient nur Webanforderungen vom lokalen Computer.</span><span class="sxs-lookup"><span data-stu-id="ef50d-161">Localhost only serves web requests from the local computer.</span></span> <span data-ttu-id="ef50d-162">Wenn in Visual Studio ein Webprojekt erstellt wird, wird für den Webserver ein zufälliger Port verwendet.</span><span class="sxs-lookup"><span data-stu-id="ef50d-162">When Visual Studio creates a web project, a random port is used for the web server.</span></span> <span data-ttu-id="ef50d-163">In der vorherigen Abbildung ist die Nummer des Ports 5000.</span><span class="sxs-lookup"><span data-stu-id="ef50d-163">In the preceding image, the port number is 5000.</span></span> <span data-ttu-id="ef50d-164">Wenn Sie die App ausführen, wird eine andere Portnummer angezeigt.</span><span class="sxs-lookup"><span data-stu-id="ef50d-164">When you run the app, you'll see a different port number.</span></span>
* <span data-ttu-id="ef50d-165">Das Starten der App mit **STRG+F5** (Nicht-Debugmodus) ermöglicht die Änderung des Codes, das Speichern der Datei, das Aktualisieren des Browsers und das Anzeigen von Codeänderungen.</span><span class="sxs-lookup"><span data-stu-id="ef50d-165">Launching the app with **Ctrl+F5** (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="ef50d-166">Viele Entwickler bevorzugen den Nicht-Debugmodus, um die App schnell zu starten und Änderungen anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="ef50d-166">Many developers prefer to use non-debug mode to quickly launch the app and view changes.</span></span>

[!INCLUDE [razor-pages-start](~/includes/RP/2.1/razor-pages-start.md)]

::: moniker-end

> [!div class="step-by-step"]
> [<span data-ttu-id="ef50d-167">Nächstes Tutorial: Adding a model to a Razor Pages app in ASP.NET Core with Visual Studio for Mac (Hinzufügen eines Modells zu einer App mit Razor Pages in ASP.NET Core mit Visual Studio für Mac)</span><span class="sxs-lookup"><span data-stu-id="ef50d-167">Next: Adding a model</span></span>](xref:tutorials/razor-pages/model)
