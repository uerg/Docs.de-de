---
title: Erste Schritte mit ASP.NET Core MVC und Visual Studio
author: rick-anderson
description: Erste Schritte mit ASP.NET Core MVC und Visual Studio
keywords: ASP.NET Core, MVC
ms.author: riande
manager: wpickett
ms.date: 10/07/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/first-mvc-app/start-mvc
ms.openlocfilehash: 72852678a296107e6a2a146ed2324335a6e870ee
ms.sourcegitcommit: 77b8025c30ec2fd46d85ee2a2b497c44435d3009
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/12/2018
---
# <a name="getting-started-with-aspnet-core-mvc-and-visual-studio"></a><span data-ttu-id="ed2f1-104">Erste Schritte mit ASP.NET Core MVC und Visual Studio</span><span class="sxs-lookup"><span data-stu-id="ed2f1-104">Getting started with ASP.NET Core MVC and Visual Studio</span></span>

<span data-ttu-id="ed2f1-105">Von [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="ed2f1-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

[!INCLUDE[consider RP](../../includes/razor.md)]

<span data-ttu-id="ed2f1-106">Es gibt drei Versionen dieses Tutorials:</span><span class="sxs-lookup"><span data-stu-id="ed2f1-106">There are 3 versions of this tutorial:</span></span>

* <span data-ttu-id="ed2f1-107">macOS: [Erstellen einer ASP.NET Core MVC-App mit Visual Studio für Mac](xref:tutorials/first-mvc-app-mac/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="ed2f1-107">macOS: [Create an ASP.NET Core MVC app with Visual Studio for Mac](xref:tutorials/first-mvc-app-mac/start-mvc)</span></span>
* <span data-ttu-id="ed2f1-108">Windows: [Erstellen einer ASP.NET Core MVC-App mit Visual Studio](xref:tutorials/first-mvc-app/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="ed2f1-108">Windows: [Create an ASP.NET Core MVC app with Visual Studio](xref:tutorials/first-mvc-app/start-mvc)</span></span>
* <span data-ttu-id="ed2f1-109">macOS, Linux und Windows: [Erstellen einer ASP.NET Core MVC-App mit Visual Studio Code](xref:tutorials/first-mvc-app-xplat/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="ed2f1-109">macOS, Linux, and Windows: [Create an ASP.NET Core MVC app with Visual Studio Code](xref:tutorials/first-mvc-app-xplat/start-mvc)</span></span>

## <a name="install-visual-studio-and-net-core"></a><span data-ttu-id="ed2f1-110">Installieren von Visual Studio und .NET Core</span><span class="sxs-lookup"><span data-stu-id="ed2f1-110">Install Visual Studio and .NET Core</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="ed2f1-111">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="ed2f1-111">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

[!INCLUDE[install 2.0](../../includes/install2.0.md)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="ed2f1-112">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="ed2f1-112">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="ed2f1-113">Installieren Sie Visual Studio Community 2017.</span><span class="sxs-lookup"><span data-stu-id="ed2f1-113">Install Visual Studio Community 2017.</span></span> <span data-ttu-id="ed2f1-114">Wählen Sie den Download „Community“ aus.</span><span class="sxs-lookup"><span data-stu-id="ed2f1-114">Select the Community download.</span></span> <span data-ttu-id="ed2f1-115">Überspringen Sie diesen Schritt, wenn Sie Visual Studio 2017 bereits installiert haben.</span><span class="sxs-lookup"><span data-stu-id="ed2f1-115">Skip this step if you have Visual Studio 2017 installed.</span></span>

* [<span data-ttu-id="ed2f1-116">Visual Studio 2017-Installationsprogramm auf der Homepage</span><span class="sxs-lookup"><span data-stu-id="ed2f1-116">Visual Studio 2017 Home page installer</span></span>](https://www.visualstudio.com/)

<span data-ttu-id="ed2f1-117">Führen Sie das Installationsprogramm aus, und wählen Sie die folgenden Workloads aus:</span><span class="sxs-lookup"><span data-stu-id="ed2f1-117">Run the installer and select the following workloads:</span></span>

* <span data-ttu-id="ed2f1-118">**ASP.NET- und Webentwicklung** (unter **Web & Cloud**)</span><span class="sxs-lookup"><span data-stu-id="ed2f1-118">**ASP.NET and web development** (under **Web & Cloud**)</span></span>
* <span data-ttu-id="ed2f1-119">**Plattformübergreifende .NET Core-Entwicklung** (unter **Andere Toolsets**)</span><span class="sxs-lookup"><span data-stu-id="ed2f1-119">**.NET Core cross-platform development** (under **Other Toolsets**)</span></span>

![**ASP.NET- und Webentwicklung** (unter **Web & Cloud**)](start-mvc/_static/web_workload.png)

![**Plattformübergreifende .NET Core-Entwicklung** (unter **Andere Toolsets**)](start-mvc/_static/x_plat_wl.png)

---

## <a name="create-a-web-app"></a><span data-ttu-id="ed2f1-122">Erstellen einer Web-App</span><span class="sxs-lookup"><span data-stu-id="ed2f1-122">Create a web app</span></span>

<span data-ttu-id="ed2f1-123">Klicken Sie in Visual Studio auf **Datei > Neu > Projekt**.</span><span class="sxs-lookup"><span data-stu-id="ed2f1-123">From Visual Studio, select  **File > New > Project**.</span></span>

![Datei > Neu > Projekt](start-mvc/_static/alt_new_project.png)

<span data-ttu-id="ed2f1-125">Schließen Sie das Dialogfeld **Neues Projekt**ab:</span><span class="sxs-lookup"><span data-stu-id="ed2f1-125">Complete the **New Project** dialog:</span></span>

* <span data-ttu-id="ed2f1-126">Tippen Sie im linken Bereich auf **.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="ed2f1-126">In the left pane, tap **.NET Core**</span></span>
* <span data-ttu-id="ed2f1-127">Tippen Sie im mittleren Bereich auf **ASP.NET Core-Webanwendung (.NET Core)**.</span><span class="sxs-lookup"><span data-stu-id="ed2f1-127">In the center pane, tap **ASP.NET Core Web Application (.NET Core)**</span></span>
* <span data-ttu-id="ed2f1-128">Geben Sie dem Projekt den Namen „MvcMovie“. Es ist wichtig, dem Projekt diesen Namen zu geben, sodass beim Kopieren von Code der Namespace übereinstimmt.</span><span class="sxs-lookup"><span data-stu-id="ed2f1-128">Name the project "MvcMovie" (It's important to name the project "MvcMovie" so when you copy code, the namespace will match.)</span></span>
* <span data-ttu-id="ed2f1-129">Tippen Sie auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="ed2f1-129">Tap **OK**</span></span>

![<span data-ttu-id="ed2f1-130">Dialogfeld „Neues Projekt“, .NET Core im linken Bereich, ASP.NET Core-Web</span><span class="sxs-lookup"><span data-stu-id="ed2f1-130">New project dialog, .Net core in left pane, ASP.NET Core web</span></span> ](start-mvc/_static/new_project2.png)


# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="ed2f1-131">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="ed2f1-131">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="ed2f1-132">Schließen Sie das Dialogfeld **ASP.NET Core-Webanwendung (.NET Core) – MvcMovie** ab:</span><span class="sxs-lookup"><span data-stu-id="ed2f1-132">Complete the **New ASP.NET Core Web Application (.NET Core) - MvcMovie** dialog:</span></span>

* <span data-ttu-id="ed2f1-133">Klicken Sie im Dropdownfeld der Versionsauswahl auf **ASP.NET Core 2.–**.</span><span class="sxs-lookup"><span data-stu-id="ed2f1-133">In the version selector drop-down box select **ASP.NET Core 2.-**</span></span>
* <span data-ttu-id="ed2f1-134">Klicken Sie auf **Webanwendung (Model View Controller)**.</span><span class="sxs-lookup"><span data-stu-id="ed2f1-134">Select **Web Application(Model-View-Controller)**</span></span>
* <span data-ttu-id="ed2f1-135">Tippen Sie auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="ed2f1-135">Tap **OK**.</span></span>

![<span data-ttu-id="ed2f1-136">Dialogfeld „Neues Projekt“, .NET Core im linken Bereich, ASP.NET Core-Web</span><span class="sxs-lookup"><span data-stu-id="ed2f1-136">New project dialog, .Net core in left pane, ASP.NET Core web</span></span> ](start-mvc/_static/new_project22.png)

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="ed2f1-137">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="ed2f1-137">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="ed2f1-138">Schließen Sie das Dialogfeld **ASP.NET Core-Webanwendung (.NET Core) – MvcMovie** ab:</span><span class="sxs-lookup"><span data-stu-id="ed2f1-138">Complete the **New ASP.NET Core Web Application (.NET Core) - MvcMovie** dialog:</span></span>

* <span data-ttu-id="ed2f1-139">Tippen Sie im Dropdownfeld der Versionsauswahl auf **ASP.NET Core 1.1**.</span><span class="sxs-lookup"><span data-stu-id="ed2f1-139">In the version selector drop-down box tap **ASP.NET Core 1.1**</span></span>
* <span data-ttu-id="ed2f1-140">Tippen Sie auf **Webanwendung**.</span><span class="sxs-lookup"><span data-stu-id="ed2f1-140">Tap **Web Application**</span></span>
* <span data-ttu-id="ed2f1-141">Behalten Sie den Standardwert **Keine Authentifizierung** bei.</span><span class="sxs-lookup"><span data-stu-id="ed2f1-141">Keep the default **No Authentication**</span></span>
* <span data-ttu-id="ed2f1-142">Tippen Sie auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="ed2f1-142">Tap **OK**.</span></span>

![Neue ASP.NET Core-Web-App](start-mvc/_static/p3.png)

---

<span data-ttu-id="ed2f1-144">Visual Studio verwendet eine Standardvorlage für das MVC-Projekt, das Sie gerade erstellt haben.</span><span class="sxs-lookup"><span data-stu-id="ed2f1-144">Visual Studio used a default template for the MVC project you just created.</span></span> <span data-ttu-id="ed2f1-145">Wenn Sie einen Projektnamen eingeben und einige Optionen festlegen, funktioniert Ihre App bereits.</span><span class="sxs-lookup"><span data-stu-id="ed2f1-145">You have a working app right now by entering a project name and selecting a few options.</span></span> <span data-ttu-id="ed2f1-146">Dies ist ein einfaches Startprojekt und ein guter Einstieg.</span><span class="sxs-lookup"><span data-stu-id="ed2f1-146">This is a simple starter project, and it's a good place to start,</span></span>

<span data-ttu-id="ed2f1-147">Tippen Sie auf **F5**, um die App im Debugmodus auszuführen oder **STRG+F5**, um sie im Nicht-Debugmodus auszuführen.</span><span class="sxs-lookup"><span data-stu-id="ed2f1-147">Tap **F5** to run the app in debug mode or **Ctrl-F5** in non-debug mode.</span></span>
<!-- These images are also used by uid: tutorials/first-mvc-app-xplat/start-mvc -->
![Ausgeführte App](start-mvc/_static/1.png)

* <span data-ttu-id="ed2f1-149">Visual Studio startet [IIS Express](https://docs.microsoft.com/iis/extensions/introduction-to-iis-express/iis-express-overview) und führt Ihre App aus.</span><span class="sxs-lookup"><span data-stu-id="ed2f1-149">Visual Studio starts [IIS Express](https://docs.microsoft.com/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs your app.</span></span> <span data-ttu-id="ed2f1-150">Beachten Sie, dass die Adressleiste `localhost:port#` und nicht etwas wie `example.com` anzeigt.</span><span class="sxs-lookup"><span data-stu-id="ed2f1-150">Notice that the address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="ed2f1-151">Das liegt daran, dass es sich bei `localhost` um den Standard-Hostnamen für Ihren lokalen Computer handelt.</span><span class="sxs-lookup"><span data-stu-id="ed2f1-151">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="ed2f1-152">Wenn in Visual Studio ein Webprojekt erstellt wird, wird für den Webserver ein zufälliger Port verwendet.</span><span class="sxs-lookup"><span data-stu-id="ed2f1-152">When Visual Studio creates a web project, a random port is used for the web server.</span></span> <span data-ttu-id="ed2f1-153">In der obigen Abbildung ist die Portnummer 5000.</span><span class="sxs-lookup"><span data-stu-id="ed2f1-153">In the image above, the port number is 5000.</span></span> <span data-ttu-id="ed2f1-154">Die URL im Browser zeigt `localhost:5000` an.</span><span class="sxs-lookup"><span data-stu-id="ed2f1-154">The URL in the browser shows `localhost:5000`.</span></span> <span data-ttu-id="ed2f1-155">Wenn Sie die App ausführen, wird eine andere Portnummer angezeigt.</span><span class="sxs-lookup"><span data-stu-id="ed2f1-155">When you run the app, you'll see a different port number.</span></span>
* <span data-ttu-id="ed2f1-156">Das Starten der App mit **STRG+F5** (Nicht-Debugmodus) ermöglicht die Änderung des Codes, das Speichern der Datei, das Aktualisieren des Browsers und das Anzeigen von Codeänderungen.</span><span class="sxs-lookup"><span data-stu-id="ed2f1-156">Launching the app with **Ctrl+F5** (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="ed2f1-157">Viele Entwickler bevorzugen den Nicht-Debugmodus, um die App schnell zu starten und Änderungen anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="ed2f1-157">Many developers prefer to use non-debug mode to quickly launch the app and view changes.</span></span>
* <span data-ttu-id="ed2f1-158">Sie können die App über das Menüelement **Debuggen** im Debugmodus oder Nicht-Debugmodus starten:</span><span class="sxs-lookup"><span data-stu-id="ed2f1-158">You can launch the app in debug or non-debug mode from the **Debug** menu item:</span></span>

![Menü „Debuggen“](start-mvc/_static/debug_menu.png)

* <span data-ttu-id="ed2f1-160">Sie können die App debuggen, indem Sie auf die Schaltfläche **IIS Express** tippen.</span><span class="sxs-lookup"><span data-stu-id="ed2f1-160">You can debug the app by tapping the **IIS Express** button</span></span>

![IIS Express](start-mvc/_static/iis_express.png)

<span data-ttu-id="ed2f1-162">Die Standardvorlage bietet Ihnen funktionierende Links für **Startseite, Info** und **Kontakt**.</span><span class="sxs-lookup"><span data-stu-id="ed2f1-162">The default template gives you working **Home, About** and **Contact** links.</span></span> <span data-ttu-id="ed2f1-163">Die obenstehende Browserabbildung zeigt diese Links nicht an.</span><span class="sxs-lookup"><span data-stu-id="ed2f1-163">The browser image above doesn't show these links.</span></span> <span data-ttu-id="ed2f1-164">Je nach Größe Ihres Browsers müssen Sie möglicherweise auf das Navigationssymbol klicken, um diese anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="ed2f1-164">Depending on the size of your browser, you might need to click the navigation icon to show them.</span></span>

![Navigationssymbol oben rechts](start-mvc/_static/2.png)

<span data-ttu-id="ed2f1-166">Wenn Sie die App im Debugmodus ausgeführt haben, tippen Sie **UMSCHALT+F5**, um das Debuggen zu beenden.</span><span class="sxs-lookup"><span data-stu-id="ed2f1-166">If you were running in debug mode, tap **Shift-F5** to stop debugging.</span></span>

<span data-ttu-id="ed2f1-167">Im nächsten Teil dieses Tutorials erfahren Sie mehr über MVC und beginnen mit dem Schreiben von Code.</span><span class="sxs-lookup"><span data-stu-id="ed2f1-167">In the next part of this tutorial, we'll learn about MVC and start writing some code.</span></span>

>[!div class="step-by-step"]
[<span data-ttu-id="ed2f1-168">Nächste</span><span class="sxs-lookup"><span data-stu-id="ed2f1-168">Next</span></span>](adding-controller.md)  
