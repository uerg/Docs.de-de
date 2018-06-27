---
title: Erste Schritte mit ASP.NET Core MVC und Visual Studio
author: rick-anderson
description: Hier finden Sie Informationen zum Einstieg in ASP.NET Core MVC und Visual Studio.
manager: wpickett
ms.author: riande
ms.date: 10/07/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/first-mvc-app/start-mvc
ms.openlocfilehash: 3272700c7739778a6a341ae8ee424fd69605ca53
ms.sourcegitcommit: 43bd79667bbdc8a07bd39fb4cd6f7ad3e70212fb
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/04/2018
ms.locfileid: "34729716"
---
# <a name="get-started-with-aspnet-core-mvc-and-visual-studio"></a><span data-ttu-id="52da8-103">Erste Schritte mit ASP.NET Core MVC und Visual Studio</span><span class="sxs-lookup"><span data-stu-id="52da8-103">Get started with ASP.NET Core MVC and Visual Studio</span></span>

<span data-ttu-id="52da8-104">Von [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="52da8-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

[!INCLUDE [consider RP](~/includes/razor.md)]

<span data-ttu-id="52da8-105">Es gibt drei Versionen dieses Tutorials:</span><span class="sxs-lookup"><span data-stu-id="52da8-105">There are 3 versions of this tutorial:</span></span>

* <span data-ttu-id="52da8-106">macOS: [Erstellen einer ASP.NET Core MVC-App mit Visual Studio für Mac](xref:tutorials/first-mvc-app-mac/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="52da8-106">macOS: [Create an ASP.NET Core MVC app with Visual Studio for Mac](xref:tutorials/first-mvc-app-mac/start-mvc)</span></span>
* <span data-ttu-id="52da8-107">Windows: [Erstellen einer ASP.NET Core MVC-App mit Visual Studio](xref:tutorials/first-mvc-app/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="52da8-107">Windows: [Create an ASP.NET Core MVC app with Visual Studio](xref:tutorials/first-mvc-app/start-mvc)</span></span>
* <span data-ttu-id="52da8-108">macOS, Linux und Windows: [Erstellen einer ASP.NET Core MVC-App mit Visual Studio Code](xref:tutorials/first-mvc-app-xplat/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="52da8-108">macOS, Linux, and Windows: [Create an ASP.NET Core MVC app with Visual Studio Code](xref:tutorials/first-mvc-app-xplat/start-mvc)</span></span>

## <a name="install-visual-studio-and-net-core"></a><span data-ttu-id="52da8-109">Installieren von Visual Studio und .NET Core</span><span class="sxs-lookup"><span data-stu-id="52da8-109">Install Visual Studio and .NET Core</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="52da8-110">[!INCLUDE [](~/includes/net-core-prereqs-windows.md) [](~/includes/net-core-prereqs-windows.md)]</span><span class="sxs-lookup"><span data-stu-id="52da8-110">[!INCLUDE [](~/includes/net-core-prereqs-windows.md) [](~/includes/net-core-prereqs-windows.md)]</span></span>

## <a name="create-a-web-app"></a><span data-ttu-id="52da8-111">Erstellen einer Web-App</span><span class="sxs-lookup"><span data-stu-id="52da8-111">Create a web app</span></span>

<span data-ttu-id="52da8-112">Klicken Sie in Visual Studio auf **Datei > Neu > Projekt**.</span><span class="sxs-lookup"><span data-stu-id="52da8-112">From Visual Studio, select  **File > New > Project**.</span></span>

![Datei > Neu > Projekt](start-mvc/_static/alt_new_project.png)

<span data-ttu-id="52da8-114">Schließen Sie das Dialogfeld **Neues Projekt**ab:</span><span class="sxs-lookup"><span data-stu-id="52da8-114">Complete the **New Project** dialog:</span></span>

* <span data-ttu-id="52da8-115">Tippen Sie im linken Bereich auf **.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="52da8-115">In the left pane, tap **.NET Core**</span></span>
* <span data-ttu-id="52da8-116">Tippen Sie im mittleren Bereich auf **ASP.NET Core-Webanwendung (.NET Core)**.</span><span class="sxs-lookup"><span data-stu-id="52da8-116">In the center pane, tap **ASP.NET Core Web Application (.NET Core)**</span></span>
* <span data-ttu-id="52da8-117">Geben Sie dem Projekt den Namen „MvcMovie“. Es ist wichtig, dem Projekt diesen Namen zu geben, sodass beim Kopieren von Code der Namespace übereinstimmt.</span><span class="sxs-lookup"><span data-stu-id="52da8-117">Name the project "MvcMovie" (It's important to name the project "MvcMovie" so when you copy code, the namespace will match.)</span></span>
* <span data-ttu-id="52da8-118">Tippen Sie auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="52da8-118">Tap **OK**</span></span>

![<span data-ttu-id="52da8-119">Dialogfeld „Neues Projekt“, .NET Core im linken Bereich, ASP.NET Core-Web</span><span class="sxs-lookup"><span data-stu-id="52da8-119">New project dialog, .Net core in left pane, ASP.NET Core web</span></span> ](start-mvc/_static/new_project2-21.png)

<span data-ttu-id="52da8-120">Schließen Sie das Dialogfeld **ASP.NET Core-Webanwendung (.NET Core) – MvcMovie** ab:</span><span class="sxs-lookup"><span data-stu-id="52da8-120">Complete the **New ASP.NET Core Web Application (.NET Core) - MvcMovie** dialog:</span></span>

* <span data-ttu-id="52da8-121">Wählen Sie im Dropdownfeld der Versionsauswahl den Eintrag **ASP.NET Core 2.1.** aus</span><span class="sxs-lookup"><span data-stu-id="52da8-121">In the version selector drop-down box select **ASP.NET Core 2.1**</span></span>
* <span data-ttu-id="52da8-122">Klicken Sie auf **Webanwendung (Model View Controller)**.</span><span class="sxs-lookup"><span data-stu-id="52da8-122">Select **Web Application(Model-View-Controller)**</span></span>
* <span data-ttu-id="52da8-123">Tippen Sie auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="52da8-123">Tap **OK**.</span></span>

![<span data-ttu-id="52da8-124">Dialogfeld „Neues Projekt“, .NET Core im linken Bereich, ASP.NET Core-Web</span><span class="sxs-lookup"><span data-stu-id="52da8-124">New project dialog, .Net core in left pane, ASP.NET Core web</span></span> ](start-mvc/_static/new_project22-21.png)

<span data-ttu-id="52da8-125">Visual Studio verwendet eine Standardvorlage für das MVC-Projekt, das Sie gerade erstellt haben.</span><span class="sxs-lookup"><span data-stu-id="52da8-125">Visual Studio used a default template for the MVC project you just created.</span></span> <span data-ttu-id="52da8-126">Wenn Sie einen Projektnamen eingeben und einige Optionen festlegen, funktioniert Ihre App bereits.</span><span class="sxs-lookup"><span data-stu-id="52da8-126">You have a working app right now by entering a project name and selecting a few options.</span></span> <span data-ttu-id="52da8-127">Dies ist ein grundlegendes Startprojekt und ein guter Einstieg.</span><span class="sxs-lookup"><span data-stu-id="52da8-127">This is a basic starter project, and it's a good place to start,</span></span>

<span data-ttu-id="52da8-128">Tippen Sie auf **F5**, um die App im Debugmodus auszuführen oder **STRG+F5**, um sie im Nicht-Debugmodus auszuführen.</span><span class="sxs-lookup"><span data-stu-id="52da8-128">Tap **F5** to run the app in debug mode or **Ctrl-F5** in non-debug mode.</span></span>
<!-- These images are also used by uid: tutorials/first-mvc-app-xplat/start-mvc -->
![Ausgeführte App](start-mvc/_static/1.png)

* <span data-ttu-id="52da8-130">Visual Studio startet [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) und führt Ihre App aus.</span><span class="sxs-lookup"><span data-stu-id="52da8-130">Visual Studio starts [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs your app.</span></span> <span data-ttu-id="52da8-131">Beachten Sie, dass die Adressleiste `localhost:port#` und nicht etwas wie `example.com` anzeigt.</span><span class="sxs-lookup"><span data-stu-id="52da8-131">Notice that the address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="52da8-132">Das liegt daran, dass es sich bei `localhost` um den Standard-Hostnamen für Ihren lokalen Computer handelt.</span><span class="sxs-lookup"><span data-stu-id="52da8-132">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="52da8-133">Wenn in Visual Studio ein Webprojekt erstellt wird, wird für den Webserver ein zufälliger Port verwendet.</span><span class="sxs-lookup"><span data-stu-id="52da8-133">When Visual Studio creates a web project, a random port is used for the web server.</span></span> <span data-ttu-id="52da8-134">In der obigen Abbildung ist die Portnummer 5000.</span><span class="sxs-lookup"><span data-stu-id="52da8-134">In the image above, the port number is 5000.</span></span> <span data-ttu-id="52da8-135">Die URL im Browser zeigt `localhost:5000` an.</span><span class="sxs-lookup"><span data-stu-id="52da8-135">The URL in the browser shows `localhost:5000`.</span></span> <span data-ttu-id="52da8-136">Wenn Sie die App ausführen, wird eine andere Portnummer angezeigt.</span><span class="sxs-lookup"><span data-stu-id="52da8-136">When you run the app, you'll see a different port number.</span></span>
* <span data-ttu-id="52da8-137">Das Starten der App mit **STRG+F5** (Nicht-Debugmodus) ermöglicht die Änderung des Codes, das Speichern der Datei, das Aktualisieren des Browsers und das Anzeigen von Codeänderungen.</span><span class="sxs-lookup"><span data-stu-id="52da8-137">Launching the app with **Ctrl+F5** (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="52da8-138">Viele Entwickler bevorzugen den Nicht-Debugmodus, um die App schnell zu starten und Änderungen anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="52da8-138">Many developers prefer to use non-debug mode to quickly launch the app and view changes.</span></span>
* <span data-ttu-id="52da8-139">Sie können die App über das Menüelement **Debuggen** im Debugmodus oder Nicht-Debugmodus starten:</span><span class="sxs-lookup"><span data-stu-id="52da8-139">You can launch the app in debug or non-debug mode from the **Debug** menu item:</span></span>

![Menü „Debuggen“](start-mvc/_static/debug_menu.png)

* <span data-ttu-id="52da8-141">Sie können die App debuggen, indem Sie auf die Schaltfläche **IIS Express** tippen.</span><span class="sxs-lookup"><span data-stu-id="52da8-141">You can debug the app by tapping the **IIS Express** button</span></span>

![IIS Express](start-mvc/_static/iis_express.png)

<span data-ttu-id="52da8-143">Die Standardvorlage bietet Ihnen funktionierende Links für **Startseite, Info** und **Kontakt**.</span><span class="sxs-lookup"><span data-stu-id="52da8-143">The default template gives you working **Home, About** and **Contact** links.</span></span> <span data-ttu-id="52da8-144">Die obenstehende Browserabbildung zeigt diese Links nicht an.</span><span class="sxs-lookup"><span data-stu-id="52da8-144">The browser image above doesn't show these links.</span></span> <span data-ttu-id="52da8-145">Je nach Größe Ihres Browsers müssen Sie möglicherweise auf das Navigationssymbol klicken, um diese anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="52da8-145">Depending on the size of your browser, you might need to click the navigation icon to show them.</span></span>

![Navigationssymbol oben rechts](start-mvc/_static/2.png)

<span data-ttu-id="52da8-147">Wenn Sie die App im Debugmodus ausgeführt haben, tippen Sie **UMSCHALT+F5**, um das Debuggen zu beenden.</span><span class="sxs-lookup"><span data-stu-id="52da8-147">If you were running in debug mode, tap **Shift-F5** to stop debugging.</span></span>

<span data-ttu-id="52da8-148">Im nächsten Teil dieses Tutorials erfahren Sie mehr über MVC und beginnen mit dem Schreiben von Code.</span><span class="sxs-lookup"><span data-stu-id="52da8-148">In the next part of this tutorial, we'll learn about MVC and start writing some code.</span></span>

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="52da8-149">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="52da8-149">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

<span data-ttu-id="52da8-150">[!INCLUDE [](~/includes/net-core-prereqs.md) [](~/includes/net-core-prereqs.md)]</span><span class="sxs-lookup"><span data-stu-id="52da8-150">[!INCLUDE [](~/includes/net-core-prereqs.md) [](~/includes/net-core-prereqs.md)]</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="52da8-151">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="52da8-151">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

<span data-ttu-id="52da8-152">Installieren Sie Visual Studio Community 2017.</span><span class="sxs-lookup"><span data-stu-id="52da8-152">Install Visual Studio Community 2017.</span></span> <span data-ttu-id="52da8-153">Wählen Sie den Download „Community“ aus.</span><span class="sxs-lookup"><span data-stu-id="52da8-153">Select the Community download.</span></span> <span data-ttu-id="52da8-154">Überspringen Sie diesen Schritt, wenn Sie Visual Studio 2017 bereits installiert haben.</span><span class="sxs-lookup"><span data-stu-id="52da8-154">Skip this step if you have Visual Studio 2017 installed.</span></span>

* [<span data-ttu-id="52da8-155">Visual Studio 2017-Installationsprogramm auf der Homepage</span><span class="sxs-lookup"><span data-stu-id="52da8-155">Visual Studio 2017 Home page installer</span></span>](https://www.visualstudio.com/)

<span data-ttu-id="52da8-156">Führen Sie das Installationsprogramm aus, und wählen Sie die folgenden Workloads aus:</span><span class="sxs-lookup"><span data-stu-id="52da8-156">Run the installer and select the following workloads:</span></span>

* <span data-ttu-id="52da8-157">**ASP.NET- und Webentwicklung** (unter **Web & Cloud**)</span><span class="sxs-lookup"><span data-stu-id="52da8-157">**ASP.NET and web development** (under **Web & Cloud**)</span></span>
* <span data-ttu-id="52da8-158">**Plattformübergreifende .NET Core-Entwicklung** (unter **Andere Toolsets**)</span><span class="sxs-lookup"><span data-stu-id="52da8-158">**.NET Core cross-platform development** (under **Other Toolsets**)</span></span>

![**ASP.NET- und Webentwicklung** (unter **Web & Cloud**)](start-mvc/_static/web_workload.png)

![**Plattformübergreifende .NET Core-Entwicklung** (unter **Andere Toolsets**)](start-mvc/_static/x_plat_wl.png)

---

## <a name="create-a-web-app"></a><span data-ttu-id="52da8-161">Erstellen einer Web-App</span><span class="sxs-lookup"><span data-stu-id="52da8-161">Create a web app</span></span>

<span data-ttu-id="52da8-162">Klicken Sie in Visual Studio auf **Datei > Neu > Projekt**.</span><span class="sxs-lookup"><span data-stu-id="52da8-162">From Visual Studio, select  **File > New > Project**.</span></span>

![Datei > Neu > Projekt](start-mvc/_static/alt_new_project.png)

<span data-ttu-id="52da8-164">Schließen Sie das Dialogfeld **Neues Projekt**ab:</span><span class="sxs-lookup"><span data-stu-id="52da8-164">Complete the **New Project** dialog:</span></span>

* <span data-ttu-id="52da8-165">Tippen Sie im linken Bereich auf **.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="52da8-165">In the left pane, tap **.NET Core**</span></span>
* <span data-ttu-id="52da8-166">Tippen Sie im mittleren Bereich auf **ASP.NET Core-Webanwendung (.NET Core)**.</span><span class="sxs-lookup"><span data-stu-id="52da8-166">In the center pane, tap **ASP.NET Core Web Application (.NET Core)**</span></span>
* <span data-ttu-id="52da8-167">Geben Sie dem Projekt den Namen „MvcMovie“. Es ist wichtig, dem Projekt diesen Namen zu geben, sodass beim Kopieren von Code der Namespace übereinstimmt.</span><span class="sxs-lookup"><span data-stu-id="52da8-167">Name the project "MvcMovie" (It's important to name the project "MvcMovie" so when you copy code, the namespace will match.)</span></span>
* <span data-ttu-id="52da8-168">Tippen Sie auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="52da8-168">Tap **OK**</span></span>

![<span data-ttu-id="52da8-169">Dialogfeld „Neues Projekt“, .NET Core im linken Bereich, ASP.NET Core-Web</span><span class="sxs-lookup"><span data-stu-id="52da8-169">New project dialog, .Net core in left pane, ASP.NET Core web</span></span> ](start-mvc/_static/new_project2.png)

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="52da8-170">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="52da8-170">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="52da8-171">Schließen Sie das Dialogfeld **ASP.NET Core-Webanwendung (.NET Core) – MvcMovie** ab:</span><span class="sxs-lookup"><span data-stu-id="52da8-171">Complete the **New ASP.NET Core Web Application (.NET Core) - MvcMovie** dialog:</span></span>

* <span data-ttu-id="52da8-172">Klicken Sie im Dropdownfeld der Versionsauswahl auf **ASP.NET Core 2.–**.</span><span class="sxs-lookup"><span data-stu-id="52da8-172">In the version selector drop-down box select **ASP.NET Core 2.-**</span></span>
* <span data-ttu-id="52da8-173">Klicken Sie auf **Webanwendung (Model View Controller)**.</span><span class="sxs-lookup"><span data-stu-id="52da8-173">Select **Web Application(Model-View-Controller)**</span></span>
* <span data-ttu-id="52da8-174">Tippen Sie auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="52da8-174">Tap **OK**.</span></span>

![<span data-ttu-id="52da8-175">Dialogfeld „Neues Projekt“, .NET Core im linken Bereich, ASP.NET Core-Web</span><span class="sxs-lookup"><span data-stu-id="52da8-175">New project dialog, .Net core in left pane, ASP.NET Core web</span></span> ](start-mvc/_static/new_project22.png)

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="52da8-176">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="52da8-176">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="52da8-177">Schließen Sie das Dialogfeld **ASP.NET Core-Webanwendung (.NET Core) – MvcMovie** ab:</span><span class="sxs-lookup"><span data-stu-id="52da8-177">Complete the **New ASP.NET Core Web Application (.NET Core) - MvcMovie** dialog:</span></span>

* <span data-ttu-id="52da8-178">Tippen Sie im Dropdownfeld der Versionsauswahl auf **ASP.NET Core 1.1**.</span><span class="sxs-lookup"><span data-stu-id="52da8-178">In the version selector drop-down box tap **ASP.NET Core 1.1**</span></span>
* <span data-ttu-id="52da8-179">Tippen Sie auf **Webanwendung**.</span><span class="sxs-lookup"><span data-stu-id="52da8-179">Tap **Web Application**</span></span>
* <span data-ttu-id="52da8-180">Behalten Sie den Standardwert **Keine Authentifizierung** bei.</span><span class="sxs-lookup"><span data-stu-id="52da8-180">Keep the default **No Authentication**</span></span>
* <span data-ttu-id="52da8-181">Tippen Sie auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="52da8-181">Tap **OK**.</span></span>

![Neue ASP.NET Core-Web-App](start-mvc/_static/p3.png)

---

<span data-ttu-id="52da8-183">Visual Studio verwendet eine Standardvorlage für das MVC-Projekt, das Sie gerade erstellt haben.</span><span class="sxs-lookup"><span data-stu-id="52da8-183">Visual Studio used a default template for the MVC project you just created.</span></span> <span data-ttu-id="52da8-184">Wenn Sie einen Projektnamen eingeben und einige Optionen festlegen, funktioniert Ihre App bereits.</span><span class="sxs-lookup"><span data-stu-id="52da8-184">You have a working app right now by entering a project name and selecting a few options.</span></span> <span data-ttu-id="52da8-185">Dies ist ein grundlegendes Startprojekt und ein guter Einstieg.</span><span class="sxs-lookup"><span data-stu-id="52da8-185">This is a basic starter project, and it's a good place to start,</span></span>

<span data-ttu-id="52da8-186">Tippen Sie auf **F5**, um die App im Debugmodus auszuführen oder **STRG+F5**, um sie im Nicht-Debugmodus auszuführen.</span><span class="sxs-lookup"><span data-stu-id="52da8-186">Tap **F5** to run the app in debug mode or **Ctrl-F5** in non-debug mode.</span></span>
<!-- These images are also used by uid: tutorials/first-mvc-app-xplat/start-mvc -->
![Ausgeführte App](start-mvc/_static/1.png)

* <span data-ttu-id="52da8-188">Visual Studio startet [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) und führt Ihre App aus.</span><span class="sxs-lookup"><span data-stu-id="52da8-188">Visual Studio starts [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs your app.</span></span> <span data-ttu-id="52da8-189">Beachten Sie, dass die Adressleiste `localhost:port#` und nicht etwas wie `example.com` anzeigt.</span><span class="sxs-lookup"><span data-stu-id="52da8-189">Notice that the address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="52da8-190">Das liegt daran, dass es sich bei `localhost` um den Standard-Hostnamen für Ihren lokalen Computer handelt.</span><span class="sxs-lookup"><span data-stu-id="52da8-190">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="52da8-191">Wenn in Visual Studio ein Webprojekt erstellt wird, wird für den Webserver ein zufälliger Port verwendet.</span><span class="sxs-lookup"><span data-stu-id="52da8-191">When Visual Studio creates a web project, a random port is used for the web server.</span></span> <span data-ttu-id="52da8-192">In der obigen Abbildung ist die Portnummer 5000.</span><span class="sxs-lookup"><span data-stu-id="52da8-192">In the image above, the port number is 5000.</span></span> <span data-ttu-id="52da8-193">Die URL im Browser zeigt `localhost:5000` an.</span><span class="sxs-lookup"><span data-stu-id="52da8-193">The URL in the browser shows `localhost:5000`.</span></span> <span data-ttu-id="52da8-194">Wenn Sie die App ausführen, wird eine andere Portnummer angezeigt.</span><span class="sxs-lookup"><span data-stu-id="52da8-194">When you run the app, you'll see a different port number.</span></span>
* <span data-ttu-id="52da8-195">Das Starten der App mit **STRG+F5** (Nicht-Debugmodus) ermöglicht die Änderung des Codes, das Speichern der Datei, das Aktualisieren des Browsers und das Anzeigen von Codeänderungen.</span><span class="sxs-lookup"><span data-stu-id="52da8-195">Launching the app with **Ctrl+F5** (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="52da8-196">Viele Entwickler bevorzugen den Nicht-Debugmodus, um die App schnell zu starten und Änderungen anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="52da8-196">Many developers prefer to use non-debug mode to quickly launch the app and view changes.</span></span>
* <span data-ttu-id="52da8-197">Sie können die App über das Menüelement **Debuggen** im Debugmodus oder Nicht-Debugmodus starten:</span><span class="sxs-lookup"><span data-stu-id="52da8-197">You can launch the app in debug or non-debug mode from the **Debug** menu item:</span></span>

![Menü „Debuggen“](start-mvc/_static/debug_menu.png)

* <span data-ttu-id="52da8-199">Sie können die App debuggen, indem Sie auf die Schaltfläche **IIS Express** tippen.</span><span class="sxs-lookup"><span data-stu-id="52da8-199">You can debug the app by tapping the **IIS Express** button</span></span>

![IIS Express](start-mvc/_static/iis_express.png)

<span data-ttu-id="52da8-201">Die Standardvorlage bietet Ihnen funktionierende Links für **Startseite, Info** und **Kontakt**.</span><span class="sxs-lookup"><span data-stu-id="52da8-201">The default template gives you working **Home, About** and **Contact** links.</span></span> <span data-ttu-id="52da8-202">Die obenstehende Browserabbildung zeigt diese Links nicht an.</span><span class="sxs-lookup"><span data-stu-id="52da8-202">The browser image above doesn't show these links.</span></span> <span data-ttu-id="52da8-203">Je nach Größe Ihres Browsers müssen Sie möglicherweise auf das Navigationssymbol klicken, um diese anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="52da8-203">Depending on the size of your browser, you might need to click the navigation icon to show them.</span></span>

![Navigationssymbol oben rechts](start-mvc/_static/2.png)

<span data-ttu-id="52da8-205">Wenn Sie die App im Debugmodus ausgeführt haben, tippen Sie **UMSCHALT+F5**, um das Debuggen zu beenden.</span><span class="sxs-lookup"><span data-stu-id="52da8-205">If you were running in debug mode, tap **Shift-F5** to stop debugging.</span></span>

<span data-ttu-id="52da8-206">Im nächsten Teil dieses Tutorials erfahren Sie mehr über MVC und beginnen mit dem Schreiben von Code.</span><span class="sxs-lookup"><span data-stu-id="52da8-206">In the next part of this tutorial, we'll learn about MVC and start writing some code.</span></span>

::: moniker-end
> [!div class="step-by-step"]
> [<span data-ttu-id="52da8-207">Nächste</span><span class="sxs-lookup"><span data-stu-id="52da8-207">Next</span></span>](adding-controller.md)  
