---
title: Erste Schritte mit ASP.NET Core MVC und Visual Studio
author: rick-anderson
description: Hier finden Sie Informationen zum Einstieg in ASP.NET Core MVC und Visual Studio.
ms.author: riande
ms.date: 10/07/2017
uid: tutorials/first-mvc-app/start-mvc
ms.openlocfilehash: 41f986a06ec46dc025c4e8218745b4a513e8ee2a
ms.sourcegitcommit: b2723654af4969a24545f09ebe32004cb5e84a96
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/18/2018
ms.locfileid: "46011699"
---
# <a name="get-started-with-aspnet-core-mvc-and-visual-studio"></a><span data-ttu-id="35e3b-103">Erste Schritte mit ASP.NET Core MVC und Visual Studio</span><span class="sxs-lookup"><span data-stu-id="35e3b-103">Get started with ASP.NET Core MVC and Visual Studio</span></span>

<span data-ttu-id="35e3b-104">Von [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="35e3b-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

[!INCLUDE [consider RP](~/includes/razor.md)]

<span data-ttu-id="35e3b-105">Es gibt drei Versionen dieses Tutorials:</span><span class="sxs-lookup"><span data-stu-id="35e3b-105">There are 3 versions of this tutorial:</span></span>

* <span data-ttu-id="35e3b-106">macOS: [Erstellen einer ASP.NET Core MVC-App mit Visual Studio für Mac](xref:tutorials/first-mvc-app-mac/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="35e3b-106">macOS: [Create an ASP.NET Core MVC app with Visual Studio for Mac](xref:tutorials/first-mvc-app-mac/start-mvc)</span></span>
* <span data-ttu-id="35e3b-107">Windows: [Erstellen einer ASP.NET Core MVC-App mit Visual Studio](xref:tutorials/first-mvc-app/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="35e3b-107">Windows: [Create an ASP.NET Core MVC app with Visual Studio](xref:tutorials/first-mvc-app/start-mvc)</span></span>
* <span data-ttu-id="35e3b-108">macOS, Linux und Windows: [Erstellen einer ASP.NET Core MVC-App mit Visual Studio Code](xref:tutorials/first-mvc-app-xplat/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="35e3b-108">macOS, Linux, and Windows: [Create an ASP.NET Core MVC app with Visual Studio Code](xref:tutorials/first-mvc-app-xplat/start-mvc)</span></span>

## <a name="install-visual-studio-and-net-core"></a><span data-ttu-id="35e3b-109">Installieren von Visual Studio und .NET Core</span><span class="sxs-lookup"><span data-stu-id="35e3b-109">Install Visual Studio and .NET Core</span></span>

::: moniker range=">= aspnetcore-2.1"

[!INCLUDE [](~/includes/net-core-prereqs-windows.md)]

## <a name="create-a-web-app"></a><span data-ttu-id="35e3b-110">Erstellen einer Web-App</span><span class="sxs-lookup"><span data-stu-id="35e3b-110">Create a web app</span></span>

<span data-ttu-id="35e3b-111">Klicken Sie in Visual Studio auf **Datei > Neu > Projekt**.</span><span class="sxs-lookup"><span data-stu-id="35e3b-111">From Visual Studio, select  **File > New > Project**.</span></span>

![Datei > Neu > Projekt](start-mvc/_static/alt_new_project.png)

<span data-ttu-id="35e3b-113">Schließen Sie das Dialogfeld **Neues Projekt**ab:</span><span class="sxs-lookup"><span data-stu-id="35e3b-113">Complete the **New Project** dialog:</span></span>

* <span data-ttu-id="35e3b-114">Tippen Sie im linken Bereich auf **.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="35e3b-114">In the left pane, tap **.NET Core**</span></span>
* <span data-ttu-id="35e3b-115">Tippen Sie im mittleren Bereich auf **ASP.NET Core-Webanwendung (.NET Core)**.</span><span class="sxs-lookup"><span data-stu-id="35e3b-115">In the center pane, tap **ASP.NET Core Web Application (.NET Core)**</span></span>
* <span data-ttu-id="35e3b-116">Geben Sie dem Projekt den Namen „MvcMovie“. Es ist wichtig, dem Projekt diesen Namen zu geben, sodass beim Kopieren von Code der Namespace übereinstimmt.</span><span class="sxs-lookup"><span data-stu-id="35e3b-116">Name the project "MvcMovie" (It's important to name the project "MvcMovie" so when you copy code, the namespace will match.)</span></span>
* <span data-ttu-id="35e3b-117">Tippen Sie auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="35e3b-117">Tap **OK**</span></span>

![<span data-ttu-id="35e3b-118">Dialogfeld „Neues Projekt“, .NET Core im linken Bereich, ASP.NET Core-Web</span><span class="sxs-lookup"><span data-stu-id="35e3b-118">New project dialog, .Net core in left pane, ASP.NET Core web</span></span> ](start-mvc/_static/new_project2-21.png)

<span data-ttu-id="35e3b-119">Schließen Sie das Dialogfeld **ASP.NET Core-Webanwendung (.NET Core) – MvcMovie** ab:</span><span class="sxs-lookup"><span data-stu-id="35e3b-119">Complete the **New ASP.NET Core Web Application (.NET Core) - MvcMovie** dialog:</span></span>

* <span data-ttu-id="35e3b-120">Wählen Sie im Dropdownfeld der Versionsauswahl den Eintrag **ASP.NET Core 2.1.** aus</span><span class="sxs-lookup"><span data-stu-id="35e3b-120">In the version selector drop-down box select **ASP.NET Core 2.1**</span></span>
* <span data-ttu-id="35e3b-121">Klicken Sie auf **Webanwendung (Model View Controller)**.</span><span class="sxs-lookup"><span data-stu-id="35e3b-121">Select **Web Application(Model-View-Controller)**</span></span>
* <span data-ttu-id="35e3b-122">Tippen Sie auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="35e3b-122">Tap **OK**.</span></span>

![<span data-ttu-id="35e3b-123">Dialogfeld „Neues Projekt“, .NET Core im linken Bereich, ASP.NET Core-Web</span><span class="sxs-lookup"><span data-stu-id="35e3b-123">New project dialog, .Net core in left pane, ASP.NET Core web</span></span> ](start-mvc/_static/new_project22-21.png)

<span data-ttu-id="35e3b-124">Visual Studio verwendet eine Standardvorlage für das MVC-Projekt, das Sie gerade erstellt haben.</span><span class="sxs-lookup"><span data-stu-id="35e3b-124">Visual Studio used a default template for the MVC project you just created.</span></span> <span data-ttu-id="35e3b-125">Wenn Sie einen Projektnamen eingeben und einige Optionen festlegen, funktioniert Ihre App bereits.</span><span class="sxs-lookup"><span data-stu-id="35e3b-125">You have a working app right now by entering a project name and selecting a few options.</span></span> <span data-ttu-id="35e3b-126">Dies ist ein grundlegendes Startprojekt und ein guter Einstieg.</span><span class="sxs-lookup"><span data-stu-id="35e3b-126">This is a basic starter project, and it's a good place to start,</span></span>

<span data-ttu-id="35e3b-127">Tippen Sie auf **F5**, um die App im Debugmodus auszuführen oder **STRG+F5**, um sie im Nicht-Debugmodus auszuführen.</span><span class="sxs-lookup"><span data-stu-id="35e3b-127">Tap **F5** to run the app in debug mode or **Ctrl-F5** in non-debug mode.</span></span>
<span data-ttu-id="35e3b-128"><!-- These images are also used by uid: tutorials/first-mvc-app-xplat/start-mvc -->
![Ausgeführte App](start-mvc/_static/1.png)</span><span class="sxs-lookup"><span data-stu-id="35e3b-128"><!-- These images are also used by uid: tutorials/first-mvc-app-xplat/start-mvc -->
![running app](start-mvc/_static/1.png)</span></span>

* <span data-ttu-id="35e3b-129">Visual Studio startet [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) und führt Ihre App aus.</span><span class="sxs-lookup"><span data-stu-id="35e3b-129">Visual Studio starts [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs your app.</span></span> <span data-ttu-id="35e3b-130">Beachten Sie, dass die Adressleiste `localhost:port#` und nicht etwas wie `example.com` anzeigt.</span><span class="sxs-lookup"><span data-stu-id="35e3b-130">Notice that the address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="35e3b-131">Das liegt daran, dass es sich bei `localhost` um den Standard-Hostnamen für Ihren lokalen Computer handelt.</span><span class="sxs-lookup"><span data-stu-id="35e3b-131">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="35e3b-132">Wenn in Visual Studio ein Webprojekt erstellt wird, wird für den Webserver ein zufälliger Port verwendet.</span><span class="sxs-lookup"><span data-stu-id="35e3b-132">When Visual Studio creates a web project, a random port is used for the web server.</span></span> <span data-ttu-id="35e3b-133">In der obigen Abbildung ist die Portnummer 5000.</span><span class="sxs-lookup"><span data-stu-id="35e3b-133">In the image above, the port number is 5000.</span></span> <span data-ttu-id="35e3b-134">Die URL im Browser zeigt `localhost:5000` an.</span><span class="sxs-lookup"><span data-stu-id="35e3b-134">The URL in the browser shows `localhost:5000`.</span></span> <span data-ttu-id="35e3b-135">Wenn Sie die App ausführen, wird eine andere Portnummer angezeigt.</span><span class="sxs-lookup"><span data-stu-id="35e3b-135">When you run the app, you'll see a different port number.</span></span>
* <span data-ttu-id="35e3b-136">Das Starten der App mit **STRG+F5** (Nicht-Debugmodus) ermöglicht die Änderung des Codes, das Speichern der Datei, das Aktualisieren des Browsers und das Anzeigen von Codeänderungen.</span><span class="sxs-lookup"><span data-stu-id="35e3b-136">Launching the app with **Ctrl+F5** (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="35e3b-137">Viele Entwickler bevorzugen den Nicht-Debugmodus, um die App schnell zu starten und Änderungen anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="35e3b-137">Many developers prefer to use non-debug mode to quickly launch the app and view changes.</span></span>
* <span data-ttu-id="35e3b-138">Sie können die App über das Menüelement **Debuggen** im Debugmodus oder Nicht-Debugmodus starten:</span><span class="sxs-lookup"><span data-stu-id="35e3b-138">You can launch the app in debug or non-debug mode from the **Debug** menu item:</span></span>

![Menü „Debuggen“](start-mvc/_static/debug_menu.png)

* <span data-ttu-id="35e3b-140">Sie können die App debuggen, indem Sie auf die Schaltfläche **IIS Express** tippen.</span><span class="sxs-lookup"><span data-stu-id="35e3b-140">You can debug the app by tapping the **IIS Express** button</span></span>

![IIS Express](start-mvc/_static/iis_express.png)

<span data-ttu-id="35e3b-142">Die Standardvorlage bietet Ihnen funktionierende Links für **Startseite, Info** und **Kontakt**.</span><span class="sxs-lookup"><span data-stu-id="35e3b-142">The default template gives you working **Home, About** and **Contact** links.</span></span> <span data-ttu-id="35e3b-143">Die obenstehende Browserabbildung zeigt diese Links nicht an.</span><span class="sxs-lookup"><span data-stu-id="35e3b-143">The browser image above doesn't show these links.</span></span> <span data-ttu-id="35e3b-144">Je nach Größe Ihres Browsers müssen Sie möglicherweise auf das Navigationssymbol klicken, um diese anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="35e3b-144">Depending on the size of your browser, you might need to click the navigation icon to show them.</span></span>

![Navigationssymbol oben rechts](start-mvc/_static/2.png)

<span data-ttu-id="35e3b-146">Wenn Sie die App im Debugmodus ausgeführt haben, tippen Sie **UMSCHALT+F5**, um das Debuggen zu beenden.</span><span class="sxs-lookup"><span data-stu-id="35e3b-146">If you were running in debug mode, tap **Shift-F5** to stop debugging.</span></span>

<span data-ttu-id="35e3b-147">Im nächsten Teil dieses Tutorials erfahren Sie mehr über MVC und beginnen mit dem Schreiben von Code.</span><span class="sxs-lookup"><span data-stu-id="35e3b-147">In the next part of this tutorial, we'll learn about MVC and start writing some code.</span></span>

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="35e3b-148">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="35e3b-148">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

[!INCLUDE [](~/includes/net-core-prereqs.md)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="35e3b-149">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="35e3b-149">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

<span data-ttu-id="35e3b-150">Installieren Sie Visual Studio Community 2017.</span><span class="sxs-lookup"><span data-stu-id="35e3b-150">Install Visual Studio Community 2017.</span></span> <span data-ttu-id="35e3b-151">Wählen Sie den Download „Community“ aus.</span><span class="sxs-lookup"><span data-stu-id="35e3b-151">Select the Community download.</span></span> <span data-ttu-id="35e3b-152">Überspringen Sie diesen Schritt, wenn Sie Visual Studio 2017 bereits installiert haben.</span><span class="sxs-lookup"><span data-stu-id="35e3b-152">Skip this step if you have Visual Studio 2017 installed.</span></span>

* [<span data-ttu-id="35e3b-153">Visual Studio 2017-Installationsprogramm auf der Homepage</span><span class="sxs-lookup"><span data-stu-id="35e3b-153">Visual Studio 2017 Home page installer</span></span>](https://www.visualstudio.com/)

<span data-ttu-id="35e3b-154">Führen Sie das Installationsprogramm aus, und wählen Sie die folgenden Workloads aus:</span><span class="sxs-lookup"><span data-stu-id="35e3b-154">Run the installer and select the following workloads:</span></span>

* <span data-ttu-id="35e3b-155">**ASP.NET- und Webentwicklung** (unter **Web & Cloud**)</span><span class="sxs-lookup"><span data-stu-id="35e3b-155">**ASP.NET and web development** (under **Web & Cloud**)</span></span>
* <span data-ttu-id="35e3b-156">**Plattformübergreifende .NET Core-Entwicklung** (unter **Andere Toolsets**)</span><span class="sxs-lookup"><span data-stu-id="35e3b-156">**.NET Core cross-platform development** (under **Other Toolsets**)</span></span>

![**ASP.NET- und Webentwicklung** (unter **Web & Cloud**)](start-mvc/_static/web_workload.png)

![**Plattformübergreifende .NET Core-Entwicklung** (unter **Andere Toolsets**)](start-mvc/_static/x_plat_wl.png)

---

## <a name="create-a-web-app"></a><span data-ttu-id="35e3b-159">Erstellen einer Web-App</span><span class="sxs-lookup"><span data-stu-id="35e3b-159">Create a web app</span></span>

<span data-ttu-id="35e3b-160">Klicken Sie in Visual Studio auf **Datei > Neu > Projekt**.</span><span class="sxs-lookup"><span data-stu-id="35e3b-160">From Visual Studio, select  **File > New > Project**.</span></span>

![Datei > Neu > Projekt](start-mvc/_static/alt_new_project.png)

<span data-ttu-id="35e3b-162">Schließen Sie das Dialogfeld **Neues Projekt**ab:</span><span class="sxs-lookup"><span data-stu-id="35e3b-162">Complete the **New Project** dialog:</span></span>

* <span data-ttu-id="35e3b-163">Tippen Sie im linken Bereich auf **.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="35e3b-163">In the left pane, tap **.NET Core**</span></span>
* <span data-ttu-id="35e3b-164">Tippen Sie im mittleren Bereich auf **ASP.NET Core-Webanwendung (.NET Core)**.</span><span class="sxs-lookup"><span data-stu-id="35e3b-164">In the center pane, tap **ASP.NET Core Web Application (.NET Core)**</span></span>
* <span data-ttu-id="35e3b-165">Geben Sie dem Projekt den Namen „MvcMovie“. Es ist wichtig, dem Projekt diesen Namen zu geben, sodass beim Kopieren von Code der Namespace übereinstimmt.</span><span class="sxs-lookup"><span data-stu-id="35e3b-165">Name the project "MvcMovie" (It's important to name the project "MvcMovie" so when you copy code, the namespace will match.)</span></span>
* <span data-ttu-id="35e3b-166">Tippen Sie auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="35e3b-166">Tap **OK**</span></span>

![<span data-ttu-id="35e3b-167">Dialogfeld „Neues Projekt“, .NET Core im linken Bereich, ASP.NET Core-Web</span><span class="sxs-lookup"><span data-stu-id="35e3b-167">New project dialog, .Net core in left pane, ASP.NET Core web</span></span> ](start-mvc/_static/new_project2.png)

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="35e3b-168">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="35e3b-168">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="35e3b-169">Schließen Sie das Dialogfeld **ASP.NET Core-Webanwendung (.NET Core) – MvcMovie** ab:</span><span class="sxs-lookup"><span data-stu-id="35e3b-169">Complete the **New ASP.NET Core Web Application (.NET Core) - MvcMovie** dialog:</span></span>

* <span data-ttu-id="35e3b-170">Klicken Sie im Dropdownfeld der Versionsauswahl auf **ASP.NET Core 2.–**.</span><span class="sxs-lookup"><span data-stu-id="35e3b-170">In the version selector drop-down box select **ASP.NET Core 2.-**</span></span>
* <span data-ttu-id="35e3b-171">Klicken Sie auf **Webanwendung (Model View Controller)**.</span><span class="sxs-lookup"><span data-stu-id="35e3b-171">Select **Web Application(Model-View-Controller)**</span></span>
* <span data-ttu-id="35e3b-172">Tippen Sie auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="35e3b-172">Tap **OK**.</span></span>

![<span data-ttu-id="35e3b-173">Dialogfeld „Neues Projekt“, .NET Core im linken Bereich, ASP.NET Core-Web</span><span class="sxs-lookup"><span data-stu-id="35e3b-173">New project dialog, .Net core in left pane, ASP.NET Core web</span></span> ](start-mvc/_static/new_project22.png)

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="35e3b-174">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="35e3b-174">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="35e3b-175">Schließen Sie das Dialogfeld **ASP.NET Core-Webanwendung (.NET Core) – MvcMovie** ab:</span><span class="sxs-lookup"><span data-stu-id="35e3b-175">Complete the **New ASP.NET Core Web Application (.NET Core) - MvcMovie** dialog:</span></span>

* <span data-ttu-id="35e3b-176">Tippen Sie im Dropdownfeld der Versionsauswahl auf **ASP.NET Core 1.1**.</span><span class="sxs-lookup"><span data-stu-id="35e3b-176">In the version selector drop-down box tap **ASP.NET Core 1.1**</span></span>
* <span data-ttu-id="35e3b-177">Tippen Sie auf **Webanwendung**.</span><span class="sxs-lookup"><span data-stu-id="35e3b-177">Tap **Web Application**</span></span>
* <span data-ttu-id="35e3b-178">Behalten Sie den Standardwert **Keine Authentifizierung** bei.</span><span class="sxs-lookup"><span data-stu-id="35e3b-178">Keep the default **No Authentication**</span></span>
* <span data-ttu-id="35e3b-179">Tippen Sie auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="35e3b-179">Tap **OK**.</span></span>

![Neue ASP.NET Core-Web-App](start-mvc/_static/p3.png)

---

<span data-ttu-id="35e3b-181">Visual Studio verwendet eine Standardvorlage für das MVC-Projekt, das Sie gerade erstellt haben.</span><span class="sxs-lookup"><span data-stu-id="35e3b-181">Visual Studio used a default template for the MVC project you just created.</span></span> <span data-ttu-id="35e3b-182">Wenn Sie einen Projektnamen eingeben und einige Optionen festlegen, funktioniert Ihre App bereits.</span><span class="sxs-lookup"><span data-stu-id="35e3b-182">You have a working app right now by entering a project name and selecting a few options.</span></span> <span data-ttu-id="35e3b-183">Dies ist ein grundlegendes Startprojekt und ein guter Einstieg.</span><span class="sxs-lookup"><span data-stu-id="35e3b-183">This is a basic starter project, and it's a good place to start,</span></span>

<span data-ttu-id="35e3b-184">Tippen Sie auf **F5**, um die App im Debugmodus auszuführen oder **STRG+F5**, um sie im Nicht-Debugmodus auszuführen.</span><span class="sxs-lookup"><span data-stu-id="35e3b-184">Tap **F5** to run the app in debug mode or **Ctrl-F5** in non-debug mode.</span></span>
<span data-ttu-id="35e3b-185"><!-- These images are also used by uid: tutorials/first-mvc-app-xplat/start-mvc -->
![Ausgeführte App](start-mvc/_static/1.png)</span><span class="sxs-lookup"><span data-stu-id="35e3b-185"><!-- These images are also used by uid: tutorials/first-mvc-app-xplat/start-mvc -->
![running app](start-mvc/_static/1.png)</span></span>

* <span data-ttu-id="35e3b-186">Visual Studio startet [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) und führt Ihre App aus.</span><span class="sxs-lookup"><span data-stu-id="35e3b-186">Visual Studio starts [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs your app.</span></span> <span data-ttu-id="35e3b-187">Beachten Sie, dass die Adressleiste `localhost:port#` und nicht etwas wie `example.com` anzeigt.</span><span class="sxs-lookup"><span data-stu-id="35e3b-187">Notice that the address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="35e3b-188">Das liegt daran, dass es sich bei `localhost` um den Standard-Hostnamen für Ihren lokalen Computer handelt.</span><span class="sxs-lookup"><span data-stu-id="35e3b-188">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="35e3b-189">Wenn in Visual Studio ein Webprojekt erstellt wird, wird für den Webserver ein zufälliger Port verwendet.</span><span class="sxs-lookup"><span data-stu-id="35e3b-189">When Visual Studio creates a web project, a random port is used for the web server.</span></span> <span data-ttu-id="35e3b-190">In der obigen Abbildung ist die Portnummer 5000.</span><span class="sxs-lookup"><span data-stu-id="35e3b-190">In the image above, the port number is 5000.</span></span> <span data-ttu-id="35e3b-191">Die URL im Browser zeigt `localhost:5000` an.</span><span class="sxs-lookup"><span data-stu-id="35e3b-191">The URL in the browser shows `localhost:5000`.</span></span> <span data-ttu-id="35e3b-192">Wenn Sie die App ausführen, wird eine andere Portnummer angezeigt.</span><span class="sxs-lookup"><span data-stu-id="35e3b-192">When you run the app, you'll see a different port number.</span></span>
* <span data-ttu-id="35e3b-193">Das Starten der App mit **STRG+F5** (Nicht-Debugmodus) ermöglicht die Änderung des Codes, das Speichern der Datei, das Aktualisieren des Browsers und das Anzeigen von Codeänderungen.</span><span class="sxs-lookup"><span data-stu-id="35e3b-193">Launching the app with **Ctrl+F5** (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="35e3b-194">Viele Entwickler bevorzugen den Nicht-Debugmodus, um die App schnell zu starten und Änderungen anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="35e3b-194">Many developers prefer to use non-debug mode to quickly launch the app and view changes.</span></span>
* <span data-ttu-id="35e3b-195">Sie können die App über das Menüelement **Debuggen** im Debugmodus oder Nicht-Debugmodus starten:</span><span class="sxs-lookup"><span data-stu-id="35e3b-195">You can launch the app in debug or non-debug mode from the **Debug** menu item:</span></span>

![Menü „Debuggen“](start-mvc/_static/debug_menu.png)

* <span data-ttu-id="35e3b-197">Sie können die App debuggen, indem Sie auf die Schaltfläche **IIS Express** tippen.</span><span class="sxs-lookup"><span data-stu-id="35e3b-197">You can debug the app by tapping the **IIS Express** button</span></span>

![IIS Express](start-mvc/_static/iis_express.png)

<span data-ttu-id="35e3b-199">Die Standardvorlage bietet Ihnen funktionierende Links für **Startseite, Info** und **Kontakt**.</span><span class="sxs-lookup"><span data-stu-id="35e3b-199">The default template gives you working **Home, About** and **Contact** links.</span></span> <span data-ttu-id="35e3b-200">Die obenstehende Browserabbildung zeigt diese Links nicht an.</span><span class="sxs-lookup"><span data-stu-id="35e3b-200">The browser image above doesn't show these links.</span></span> <span data-ttu-id="35e3b-201">Je nach Größe Ihres Browsers müssen Sie möglicherweise auf das Navigationssymbol klicken, um diese anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="35e3b-201">Depending on the size of your browser, you might need to click the navigation icon to show them.</span></span>

![Navigationssymbol oben rechts](start-mvc/_static/2.png)

<span data-ttu-id="35e3b-203">Wenn Sie die App im Debugmodus ausgeführt haben, tippen Sie **UMSCHALT+F5**, um das Debuggen zu beenden.</span><span class="sxs-lookup"><span data-stu-id="35e3b-203">If you were running in debug mode, tap **Shift-F5** to stop debugging.</span></span>

<span data-ttu-id="35e3b-204">Im nächsten Teil dieses Tutorials erfahren Sie mehr über MVC und beginnen mit dem Schreiben von Code.</span><span class="sxs-lookup"><span data-stu-id="35e3b-204">In the next part of this tutorial, we'll learn about MVC and start writing some code.</span></span>

::: moniker-end

> [!div class="step-by-step"]
> [<span data-ttu-id="35e3b-205">Nächste</span><span class="sxs-lookup"><span data-stu-id="35e3b-205">Next</span></span>](adding-controller.md)  
