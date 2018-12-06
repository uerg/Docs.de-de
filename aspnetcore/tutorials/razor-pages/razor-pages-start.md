---
title: Erste Schritte mit Razor Pages in ASP.NET Core
author: rick-anderson
description: Diese Reihe von Tutorials zeigt, wie Sie Razor Pages in ASP.NET Core verwenden können. Erfahren Sie, wie Sie ein Modell erstellen, Code für Razor-Seiten generieren, Entity Framework Core und SQL Server für den Datenzugriff verwenden, Suchfunktionen hinzufügen, Eingabeüberprüfung hinzufügen und Migrationen verwenden, um das Modell zu aktualisieren.
ms.author: riande
ms.date: 05/30/2018
uid: tutorials/razor-pages/razor-pages-start
ms.openlocfilehash: ca5b134aaa0a9218bc3b0466c3448bd41e8cef8b
ms.sourcegitcommit: e9b99854b0a8021dafabee0db5e1338067f250a9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/28/2018
ms.locfileid: "52450735"
---
# <a name="tutorial-get-started-with-razor-pages-in-aspnet-core"></a><span data-ttu-id="c0949-104">Tutorial: Erste Schritte mit Razor Pages in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="c0949-104">Tutorial: Get started with Razor Pages in ASP.NET Core</span></span>

<span data-ttu-id="c0949-105">Von [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="c0949-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="c0949-106">Es wird empfohlen, die Version ASP.NET Core 2.1 dieses Tutorials zu verwenden.</span><span class="sxs-lookup"><span data-stu-id="c0949-106">We recommend you follow the ASP.NET Core 2.1 version of this tutorial.</span></span> <span data-ttu-id="c0949-107">Es ist **deutlich** einfacher, dieser Version zu folgen; darüber hinaus behandelt diese mehr Features.</span><span class="sxs-lookup"><span data-stu-id="c0949-107">It's **much** easier to follow and covers more features.</span></span> <span data-ttu-id="c0949-108">Wählen Sie in der Versionsauswahl **ASP.NET Core 2.1** aus.</span><span class="sxs-lookup"><span data-stu-id="c0949-108">Select **ASP.NET Core 2.1** in the version selector.</span></span>

![Versionsauswahl im Inhaltsverzeichnis](razor-pages-start/_static/v21.png)

::: moniker-end

<span data-ttu-id="c0949-110">In diesem Tutorial lernen Sie Grundlegendes zur Erstellung einer ASP.NET Core-Webapp mit Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="c0949-110">This tutorial teaches the basics of building an ASP.NET Core Razor Pages web app.</span></span> <span data-ttu-id="c0949-111">Razor Pages sind der empfohlene Weg für die Erstellung von Benutzeroberflächen für Web-Apps in ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="c0949-111">Razor Pages is the recommended way to build UI for web apps in ASP.NET Core.</span></span>

<span data-ttu-id="c0949-112">Es gibt drei Versionen dieses Tutorials:</span><span class="sxs-lookup"><span data-stu-id="c0949-112">There are three versions of this tutorial:</span></span>

* <span data-ttu-id="c0949-113">Windows: Dieses Tutorial</span><span class="sxs-lookup"><span data-stu-id="c0949-113">Windows: This tutorial</span></span>
* <span data-ttu-id="c0949-114">macOS: [Erste Schritte mit Razor Pages in ASP.NET Core mit Visual Studio für Mac](xref:tutorials/razor-pages-mac/razor-pages-start)</span><span class="sxs-lookup"><span data-stu-id="c0949-114">macOS: [Get started with Razor Pages with Visual Studio for Mac](xref:tutorials/razor-pages-mac/razor-pages-start)</span></span>
* <span data-ttu-id="c0949-115">macOS, Linux und Windows: [Erste Schritte mit Razor-Seiten in ASP.NET Core mit Visual Studio Code](xref:tutorials/razor-pages-vsc/razor-pages-start)</span><span class="sxs-lookup"><span data-stu-id="c0949-115">macOS, Linux, and Windows: [Get started with ASP.NET Core Razor Pages in Visual Studio Code](xref:tutorials/razor-pages-vsc/razor-pages-start)</span></span>

<span data-ttu-id="c0949-116">[Anzeigen oder Herunterladen von Beispielcode](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample) ([Vorgehensweise zum Herunterladen](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="c0949-116">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

> [!NOTE]
> <span data-ttu-id="c0949-117">Wir testen gerade eine vorgeschlagene neue Struktur für das ASP.NET Core-Inhaltsverzeichnis.</span><span class="sxs-lookup"><span data-stu-id="c0949-117">We’re testing the usability of a proposed new structure for the ASP.NET Core table of contents.</span></span>  <span data-ttu-id="c0949-118">Falls Sie einige Minuten Zeit haben, um einen Test durchzuführen, in dem Sie sieben unterschiedliche Artikel im aktuellen und vorgeschlagene Inhaltsverzeichnis finden sollen, [klicken Sie hier, um daran teilzunehmen](https://dpk4xbh5.optimalworkshop.com/treejack/rps16hd5).</span><span class="sxs-lookup"><span data-stu-id="c0949-118">If you have a few minutes to try an exercise of finding 7 different topics in the current or proposed table of contents, please [click here to participate in the study](https://dpk4xbh5.optimalworkshop.com/treejack/rps16hd5).</span></span>

::: moniker range=">= aspnetcore-2.1"

## <a name="prerequisites"></a><span data-ttu-id="c0949-119">Erforderliche Komponenten</span><span class="sxs-lookup"><span data-stu-id="c0949-119">Prerequisites</span></span>

[!INCLUDE [Prerequisites](~/includes/net-core-prereqs-windows.md)]

## <a name="create-a-razor-web-app"></a><span data-ttu-id="c0949-120">Erstellen einer Razor-Web-App</span><span class="sxs-lookup"><span data-stu-id="c0949-120">Create a Razor web app</span></span>

* <span data-ttu-id="c0949-121">Wählen Sie in Visual Studio im Menü **Datei** die Option **Neu** > **Projekt** aus.</span><span class="sxs-lookup"><span data-stu-id="c0949-121">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="c0949-122">Erstellen Sie eine neue ASP.NET Core-Webanwendung.</span><span class="sxs-lookup"><span data-stu-id="c0949-122">Create a new ASP.NET Core Web Application.</span></span> <span data-ttu-id="c0949-123">Nennen Sie das Projekt **RazorPagesMovie**.</span><span class="sxs-lookup"><span data-stu-id="c0949-123">Name the project **RazorPagesMovie**.</span></span> <span data-ttu-id="c0949-124">Es ist wichtig, den Namen *RazorPagesMovie* zu verwenden, damit die Namespaces übereinstimmen, wenn Sie Code kopieren/einfügen.</span><span class="sxs-lookup"><span data-stu-id="c0949-124">It's important to name the project *RazorPagesMovie* so the namespaces will match when you copy/paste code.</span></span>
 <span data-ttu-id="c0949-125">![neue ASP.NET Core-Webanwendung](razor-pages-start/_static/np_2.1.png)</span><span class="sxs-lookup"><span data-stu-id="c0949-125">![new ASP.NET Core Web Application](razor-pages-start/_static/np_2.1.png)</span></span>
* <span data-ttu-id="c0949-126">Wählen Sie in der Dropdownliste **ASP.NET Core 2.1** und anschließend **Webanwendung** aus.</span><span class="sxs-lookup"><span data-stu-id="c0949-126">Select **ASP.NET Core 2.1** in the dropdown, and then select **Web Application**.</span></span>

 ![neue ASP.NET Core-Webanwendung](razor-pages-start/_static/np_2_2.1.png)

<span data-ttu-id="c0949-128">Die Visual Studio-Vorlage erstellt ein Startprojekt:</span><span class="sxs-lookup"><span data-stu-id="c0949-128">The Visual Studio template creates a starter project:</span></span>

![Projektmappen-Explorer](razor-pages-start/_static/se2.1.png)

<span data-ttu-id="c0949-130">Drücken Sie **F5**, um die App im Debugmodus auszuführen, oder **STRG + F5**, um die App ohne Anfügen des Debuggers auszuführen.</span><span class="sxs-lookup"><span data-stu-id="c0949-130">Press **F5** to run the app in debug mode or **Ctrl-F5** to run without attaching the debugger.</span></span> <span data-ttu-id="c0949-131">Wählen Sie **Akzeptieren** aus, um der Nachverfolgung zuzustimmen.</span><span class="sxs-lookup"><span data-stu-id="c0949-131">Select **Accept** to consent to tracking.</span></span> <span data-ttu-id="c0949-132">Diese App verfolgt keine personenbezogenen Informationen nach.</span><span class="sxs-lookup"><span data-stu-id="c0949-132">This app doesn't track personal information.</span></span> <span data-ttu-id="c0949-133">Der generierte Vorlagencode enthält Objekte, die bei der Erfüllung der [Datenschutz-Grundverordnung (DSGVO)](xref:security/gdpr) als Unterstützung dienen sollen.</span><span class="sxs-lookup"><span data-stu-id="c0949-133">The template generated code includes assets to help meet [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

![Start- oder Indexseite](razor-pages-start/_static/homeGDPR.png)

<span data-ttu-id="c0949-135">Die folgende Abbildung zeigt die App, nachdem die Nachverfolgung akzeptiert wurde:</span><span class="sxs-lookup"><span data-stu-id="c0949-135">The following image shows the app after accepting tracking:</span></span>

![Start- oder Indexseite](razor-pages-start/_static/home2.1.png)

* <span data-ttu-id="c0949-137">Visual Studio startet [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) und führt die App aus.</span><span class="sxs-lookup"><span data-stu-id="c0949-137">Visual Studio starts [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs the app.</span></span> <span data-ttu-id="c0949-138">Die Adressleiste zeigt `localhost:port#` an, nicht `example.com`.</span><span class="sxs-lookup"><span data-stu-id="c0949-138">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="c0949-139">Das liegt daran, dass es sich bei `localhost` um den Standardhostnamen für Ihren lokalen Computer handelt.</span><span class="sxs-lookup"><span data-stu-id="c0949-139">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="c0949-140">„Localhost“ dient nur Webanforderungen vom lokalen Computer.</span><span class="sxs-lookup"><span data-stu-id="c0949-140">Localhost only serves web requests from the local computer.</span></span> <span data-ttu-id="c0949-141">Wenn in Visual Studio ein Webprojekt erstellt wird, wird für den Webserver ein zufälliger Port verwendet.</span><span class="sxs-lookup"><span data-stu-id="c0949-141">When Visual Studio creates a web project, a random port is used for the web server.</span></span> <span data-ttu-id="c0949-142">In der vorherigen Abbildung ist die Portnummer 5001.</span><span class="sxs-lookup"><span data-stu-id="c0949-142">In the preceding image, the port number is 5001.</span></span> <span data-ttu-id="c0949-143">Wenn Sie die App ausführen, wird eine andere Portnummer angezeigt.</span><span class="sxs-lookup"><span data-stu-id="c0949-143">When you run the app, you'll see a different port number.</span></span>
* <span data-ttu-id="c0949-144">Das Starten der App mit **STRG+F5** (Nicht-Debugmodus) ermöglicht die Änderung des Codes, das Speichern der Datei, das Aktualisieren des Browsers und das Anzeigen von Codeänderungen.</span><span class="sxs-lookup"><span data-stu-id="c0949-144">Launching the app with **Ctrl+F5** (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="c0949-145">Viele Entwickler bevorzugen den Nicht-Debugmodus, um die App schnell zu starten und Änderungen anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="c0949-145">Many developers prefer to use non-debug mode to quickly launch the app and view changes.</span></span>

[!INCLUDE [razor-pages-start](~/includes/RP/2.1/razor-pages-start.md)]

[!INCLUDE [F7](~/includes/RP/F7.md)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

## <a name="prerequisites"></a><span data-ttu-id="c0949-146">Erforderliche Komponenten</span><span class="sxs-lookup"><span data-stu-id="c0949-146">Prerequisites</span></span>

[!INCLUDE [Prerequisites](~/includes/net-core-prereqs-windows.md)]

## <a name="create-a-razor-web-app"></a><span data-ttu-id="c0949-147">Erstellen einer Razor-Web-App</span><span class="sxs-lookup"><span data-stu-id="c0949-147">Create a Razor web app</span></span>

* <span data-ttu-id="c0949-148">Wählen Sie in Visual Studio im Menü **Datei** die Option **Neu** > **Projekt** aus.</span><span class="sxs-lookup"><span data-stu-id="c0949-148">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="c0949-149">Erstellen Sie eine neue ASP.NET Core-Webanwendung.</span><span class="sxs-lookup"><span data-stu-id="c0949-149">Create a new ASP.NET Core Web Application.</span></span> <span data-ttu-id="c0949-150">Nennen Sie das Projekt **RazorPagesMovie**.</span><span class="sxs-lookup"><span data-stu-id="c0949-150">Name the project **RazorPagesMovie**.</span></span> <span data-ttu-id="c0949-151">Es ist wichtig, den Namen *RazorPagesMovie* zu verwenden, damit die Namespaces übereinstimmen, wenn Sie Code kopieren/einfügen.</span><span class="sxs-lookup"><span data-stu-id="c0949-151">It's important to name the project *RazorPagesMovie* so the namespaces will match when you copy/paste code.</span></span>
  <span data-ttu-id="c0949-152">![neue ASP.NET Core-Webanwendung](../../razor-pages/index/_static/np.png)</span><span class="sxs-lookup"><span data-stu-id="c0949-152">![new ASP.NET Core Web Application](../../razor-pages/index/_static/np.png)</span></span>
* <span data-ttu-id="c0949-153">Wählen Sie in der Dropdownliste **ASP.NET Core 2.0** aus, und klicken Sie anschließend auf **Webanwendung**.</span><span class="sxs-lookup"><span data-stu-id="c0949-153">Select **ASP.NET Core 2.0** in the dropdown, and then select **Web Application**.</span></span>

  [!INCLUDE [install 2.0](~/includes/dotnetcore-on-dotnetfx-vs.md)]

<span data-ttu-id="c0949-154">Die Visual Studio-Vorlage erstellt ein Startprojekt:</span><span class="sxs-lookup"><span data-stu-id="c0949-154">The Visual Studio template creates a starter project:</span></span>

![Projektmappen-Explorer](razor-pages-start/_static/se.png)

<span data-ttu-id="c0949-156">Drücken Sie **F5**, um die App im Debugmodus auszuführen, oder **STRG+F5** zur Ausführung ohne Anfügen des Debuggers.</span><span class="sxs-lookup"><span data-stu-id="c0949-156">Press **F5** to run the app in debug mode or **Ctrl-F5** to run without attaching the debugger</span></span>

![Start- oder Indexseite](razor-pages-start/_static/home.png)

* <span data-ttu-id="c0949-158">Visual Studio startet [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) und führt Ihre App aus.</span><span class="sxs-lookup"><span data-stu-id="c0949-158">Visual Studio starts [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs your app.</span></span> <span data-ttu-id="c0949-159">Die Adressleiste zeigt `localhost:port#` an, nicht `example.com`.</span><span class="sxs-lookup"><span data-stu-id="c0949-159">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="c0949-160">Das liegt daran, dass es sich bei `localhost` um den Standardhostnamen für Ihren lokalen Computer handelt.</span><span class="sxs-lookup"><span data-stu-id="c0949-160">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="c0949-161">„Localhost“ dient nur Webanforderungen vom lokalen Computer.</span><span class="sxs-lookup"><span data-stu-id="c0949-161">Localhost only serves web requests from the local computer.</span></span> <span data-ttu-id="c0949-162">Wenn in Visual Studio ein Webprojekt erstellt wird, wird für den Webserver ein zufälliger Port verwendet.</span><span class="sxs-lookup"><span data-stu-id="c0949-162">When Visual Studio creates a web project, a random port is used for the web server.</span></span> <span data-ttu-id="c0949-163">In der vorherigen Abbildung ist die Nummer des Ports 5000.</span><span class="sxs-lookup"><span data-stu-id="c0949-163">In the preceding image, the port number is 5000.</span></span> <span data-ttu-id="c0949-164">Wenn Sie die App ausführen, wird eine andere Portnummer angezeigt.</span><span class="sxs-lookup"><span data-stu-id="c0949-164">When you run the app, you'll see a different port number.</span></span>
* <span data-ttu-id="c0949-165">Das Starten der App mit **STRG+F5** (Nicht-Debugmodus) ermöglicht die Änderung des Codes, das Speichern der Datei, das Aktualisieren des Browsers und das Anzeigen von Codeänderungen.</span><span class="sxs-lookup"><span data-stu-id="c0949-165">Launching the app with **Ctrl+F5** (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="c0949-166">Viele Entwickler bevorzugen den Nicht-Debugmodus, um die App schnell zu starten und Änderungen anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="c0949-166">Many developers prefer to use non-debug mode to quickly launch the app and view changes.</span></span>

[!INCLUDE [razor-pages-start](~/includes/RP/razor-pages-start.md)]

::: moniker-end

> [!div class="step-by-step"]
> [<span data-ttu-id="c0949-167">Nächstes Tutorial: Adding a model to a Razor Pages app in ASP.NET Core with Visual Studio for Mac (Hinzufügen eines Modells zu einer App mit Razor Pages in ASP.NET Core mit Visual Studio für Mac)</span><span class="sxs-lookup"><span data-stu-id="c0949-167">Next: Adding a model</span></span>](xref:tutorials/razor-pages/model)
