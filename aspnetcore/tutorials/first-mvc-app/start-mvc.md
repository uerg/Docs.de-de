---
title: Erste Schritte mit ASP.NET Core MVC und Visual Studio
author: rick-anderson
description: Erste Schritte mit ASP.NET Core MVC und Visual Studio
keywords: ASP.NET Core, MVC
ms.author: riande
manager: wpickett
ms.date: 08/07/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/first-mvc-app/start-mvc
ms.openlocfilehash: 6e233a0114967405a67b05365e0125620441efb4
ms.sourcegitcommit: 78d28178345a0eea91556e4cd1adad98b1446db8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/22/2017
---
# <a name="getting-started-with-aspnet-core-mvc-and-visual-studio"></a><span data-ttu-id="881fd-104">Erste Schritte mit ASP.NET Core MVC und Visual Studio</span><span class="sxs-lookup"><span data-stu-id="881fd-104">Getting started with ASP.NET Core MVC and Visual Studio</span></span>

<span data-ttu-id="881fd-105">Von [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="881fd-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="881fd-106">Dieses Tutorial vermittelt Ihnen die Grundlagen zum Erstellen einer ASP.NET Core MVC-Web-App mithilfe von [Visual Studio 2017](https://www.visualstudio.com/).</span><span class="sxs-lookup"><span data-stu-id="881fd-106">This tutorial will teach you the basics of building an ASP.NET Core MVC web app using [Visual Studio 2017](https://www.visualstudio.com/).</span></span> [!INCLUDE[consider RP](../../includes/razor.md)]

<span data-ttu-id="881fd-107">Es gibt drei Versionen von diesem Tutorial:</span><span class="sxs-lookup"><span data-stu-id="881fd-107">There are 3 versions of this tutorial:</span></span>

* <span data-ttu-id="881fd-108">macOS: [Erstellen einer ASP.NET Core MVC-App mit Visual Studio für Mac](xref:tutorials/first-mvc-app-mac/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="881fd-108">macOS: [Create an ASP.NET Core MVC app with Visual Studio for Mac](xref:tutorials/first-mvc-app-mac/start-mvc)</span></span>
* <span data-ttu-id="881fd-109">Windows: [Erstellen einer ASP.NET Core MVC-App mit Visual Studio](xref:tutorials/first-mvc-app/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="881fd-109">Windows: [Create an ASP.NET Core MVC app with Visual Studio](xref:tutorials/first-mvc-app/start-mvc)</span></span>
* <span data-ttu-id="881fd-110">macOS, Linux und Windows: [Erstellen einer ASP.NET Core MVC-App mit Visual Studio Code](xref:tutorials/first-mvc-app-xplat/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="881fd-110">macOS, Linux, and Windows: [Create an ASP.NET Core MVC app with Visual Studio Code](xref:tutorials/first-mvc-app-xplat/start-mvc)</span></span>

<span data-ttu-id="881fd-111">Die Version dieses Tutorials für Visual Studio 2015 finden Sie unter [Visual Studio 2015-Version der ASP.NET Core-Dokumentation im PDF-Format](https://github.com/aspnet/Docs/blob/master/aspnetcore/common/_static/aspnet-core-project-json.pdf).</span><span class="sxs-lookup"><span data-stu-id="881fd-111">For the Visual Studio 2015 version of this tutorial, see the [VS 2015 version of ASP.NET Core documentation in PDF format](https://github.com/aspnet/Docs/blob/master/aspnetcore/common/_static/aspnet-core-project-json.pdf).</span></span>

## <a name="install-visual-studio-and-net-core"></a><span data-ttu-id="881fd-112">Installieren von Visual Studio und .NET Core</span><span class="sxs-lookup"><span data-stu-id="881fd-112">Install Visual Studio and .NET Core</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="881fd-113">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="881fd-113">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

[!INCLUDE[install 2.0](../../includes/install2.0.md)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="881fd-114">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="881fd-114">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="881fd-115">Installieren Sie Visual Studio Community 2017.</span><span class="sxs-lookup"><span data-stu-id="881fd-115">Install Visual Studio Community 2017.</span></span> <span data-ttu-id="881fd-116">Wählen Sie den Download „Community“ aus.</span><span class="sxs-lookup"><span data-stu-id="881fd-116">Select the Community download.</span></span> <span data-ttu-id="881fd-117">Überspringen Sie diesen Schritt, wenn Sie Visual Studio 2017 bereits installiert haben.</span><span class="sxs-lookup"><span data-stu-id="881fd-117">Skip this step if you have Visual Studio 2017 installed.</span></span>

* [<span data-ttu-id="881fd-118">Visual Studio 2017-Installationsprogramm auf der Homepage</span><span class="sxs-lookup"><span data-stu-id="881fd-118">Visual Studio 2017 Home page installer</span></span>](https://www.visualstudio.com/)

<span data-ttu-id="881fd-119">Führen Sie das Installationsprogramm aus, und wählen Sie die folgenden Workloads aus:</span><span class="sxs-lookup"><span data-stu-id="881fd-119">Run the installer and select the following workloads:</span></span>

* <span data-ttu-id="881fd-120">**ASP.NET- und Webentwicklung** (unter **Web & Cloud**)</span><span class="sxs-lookup"><span data-stu-id="881fd-120">**ASP.NET and web development** (under **Web & Cloud**)</span></span>
* <span data-ttu-id="881fd-121">**Plattformübergreifende .NET Core-Entwicklung** (unter **Andere Toolsets**)</span><span class="sxs-lookup"><span data-stu-id="881fd-121">**.NET Core cross-platform development** (under **Other Toolsets**)</span></span>

![**ASP.NET- und Webentwicklung** (unter **Web & Cloud**)](start-mvc/_static/web_workload.png)

![**Plattformübergreifende .NET Core-Entwicklung** (unter **Andere Toolsets**)](start-mvc/_static/x_plat_wl.png)

---

## <a name="create-a-web-app"></a><span data-ttu-id="881fd-124">Erstellen einer Web-App</span><span class="sxs-lookup"><span data-stu-id="881fd-124">Create a web app</span></span>

<span data-ttu-id="881fd-125">Klicken Sie in Visual Studio auf **Datei > Neu > Projekt**.</span><span class="sxs-lookup"><span data-stu-id="881fd-125">From Visual Studio, select  **File > New > Project**.</span></span>

![Datei > Neu > Projekt](start-mvc/_static/alt_new_project.png)

<span data-ttu-id="881fd-127">Schließen Sie das Dialogfeld **Neues Projekt**ab:</span><span class="sxs-lookup"><span data-stu-id="881fd-127">Complete the **New Project** dialog:</span></span>

* <span data-ttu-id="881fd-128">Tippen Sie im linken Bereich auf **.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="881fd-128">In the left pane, tap **.NET Core**</span></span>
* <span data-ttu-id="881fd-129">Tippen Sie im mittleren Bereich auf **ASP.NET Core-Webanwendung (.NET Core)**.</span><span class="sxs-lookup"><span data-stu-id="881fd-129">In the center pane, tap **ASP.NET Core Web Application (.NET Core)**</span></span>
* <span data-ttu-id="881fd-130">Geben Sie dem Projekt den Namen „MvcMovie“. Es ist wichtig, dem Projekt diesen Namen zu geben, sodass beim Kopieren von Code der Namespace übereinstimmt.</span><span class="sxs-lookup"><span data-stu-id="881fd-130">Name the project "MvcMovie" (It's important to name the project "MvcMovie" so when you copy code, the namespace will match.)</span></span>
* <span data-ttu-id="881fd-131">Tippen Sie auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="881fd-131">Tap **OK**</span></span>

![<span data-ttu-id="881fd-132">Dialogfeld „Neues Projekt“, .NET Core im linken Bereich, ASP.NET Core-Web</span><span class="sxs-lookup"><span data-stu-id="881fd-132">New project dialog, .Net core in left pane, ASP.NET Core web</span></span> ](start-mvc/_static/new_project2.png)


# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="881fd-133">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="881fd-133">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="881fd-134">Schließen Sie das Dialogfeld **ASP.NET Core-Webanwendung (.NET Core) – MvcMovie** ab:</span><span class="sxs-lookup"><span data-stu-id="881fd-134">Complete the **New ASP.NET Core Web Application (.NET Core) - MvcMovie** dialog:</span></span>

* <span data-ttu-id="881fd-135">Klicken Sie im Dropdownfeld der Versionsauswahl auf **ASP.NET Core 2.–**.</span><span class="sxs-lookup"><span data-stu-id="881fd-135">In the version selector drop-down box select **ASP.NET Core 2.-**</span></span>
* <span data-ttu-id="881fd-136">Klicken Sie auf **Webanwendung (Model View Controller)**.</span><span class="sxs-lookup"><span data-stu-id="881fd-136">Select **Web Application(Model-View-Controller)**</span></span>
* <span data-ttu-id="881fd-137">Tippen Sie auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="881fd-137">Tap **OK**.</span></span>

![<span data-ttu-id="881fd-138">Dialogfeld „Neues Projekt“, .NET Core im linken Bereich, ASP.NET Core-Web</span><span class="sxs-lookup"><span data-stu-id="881fd-138">New project dialog, .Net core in left pane, ASP.NET Core web</span></span> ](start-mvc/_static/new_project22.png)

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="881fd-139">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="881fd-139">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="881fd-140">Schließen Sie das Dialogfeld **ASP.NET Core-Webanwendung (.NET Core) – MvcMovie** ab:</span><span class="sxs-lookup"><span data-stu-id="881fd-140">Complete the **New ASP.NET Core Web Application (.NET Core) - MvcMovie** dialog:</span></span>

* <span data-ttu-id="881fd-141">Tippen Sie im Dropdownfeld der Versionsauswahl auf **ASP.NET Core 1.1**.</span><span class="sxs-lookup"><span data-stu-id="881fd-141">In the version selector drop-down box tap **ASP.NET Core 1.1**</span></span>
* <span data-ttu-id="881fd-142">Tippen Sie auf **Webanwendung**.</span><span class="sxs-lookup"><span data-stu-id="881fd-142">Tap **Web Application**</span></span>
* <span data-ttu-id="881fd-143">Behalten Sie den Standardwert **Keine Authentifizierung** bei.</span><span class="sxs-lookup"><span data-stu-id="881fd-143">Keep the default **No Authentication**</span></span>
* <span data-ttu-id="881fd-144">Tippen Sie auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="881fd-144">Tap **OK**.</span></span>

![Neue ASP.NET Core-Web-App](start-mvc/_static/p3.png)

---

<span data-ttu-id="881fd-146">Visual Studio verwendet eine Standardvorlage für das MVC-Projekt, das Sie gerade erstellt haben.</span><span class="sxs-lookup"><span data-stu-id="881fd-146">Visual Studio used a default template for the MVC project you just created.</span></span> <span data-ttu-id="881fd-147">Wenn Sie einen Projektnamen eingeben und einige Optionen festlegen, funktioniert Ihre App bereits.</span><span class="sxs-lookup"><span data-stu-id="881fd-147">You have a working app right now by entering a project name and selecting a few options.</span></span> <span data-ttu-id="881fd-148">Dies ist ein einfaches Startprojekt und ein guter Einstieg.</span><span class="sxs-lookup"><span data-stu-id="881fd-148">This is a simple starter project, and it's a good place to start,</span></span>

<span data-ttu-id="881fd-149">Tippen Sie auf **F5**, um die App im Debugmodus auszuführen oder **STRG+F5**, um sie im Nicht-Debugmodus auszuführen.</span><span class="sxs-lookup"><span data-stu-id="881fd-149">Tap **F5** to run the app in debug mode or **Ctrl-F5** in non-debug mode.</span></span>
<!-- These images are also used by uid: tutorials/first-mvc-app-xplat/start-mvc -->
![Ausgeführte App](start-mvc/_static/1.png)

* <span data-ttu-id="881fd-151">Visual Studio startet [IIS Express](https://docs.microsoft.com/iis/extensions/introduction-to-iis-express/iis-express-overview) und führt Ihre App aus.</span><span class="sxs-lookup"><span data-stu-id="881fd-151">Visual Studio starts [IIS Express](https://docs.microsoft.com/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs your app.</span></span> <span data-ttu-id="881fd-152">Beachten Sie, dass die Adressleiste `localhost:port#` und nicht etwas wie `example.com` anzeigt.</span><span class="sxs-lookup"><span data-stu-id="881fd-152">Notice that the address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="881fd-153">Das liegt daran, dass es sich bei `localhost` um den Standard-Hostnamen für Ihren lokalen Computer handelt.</span><span class="sxs-lookup"><span data-stu-id="881fd-153">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="881fd-154">Wenn in Visual Studio ein Webprojekt erstellt wird, wird für den Webserver ein zufälliger Port verwendet.</span><span class="sxs-lookup"><span data-stu-id="881fd-154">When Visual Studio creates a web project, a random port is used for the web server.</span></span> <span data-ttu-id="881fd-155">In der obigen Abbildung ist die Portnummer 5000.</span><span class="sxs-lookup"><span data-stu-id="881fd-155">In the image above, the port number is 5000.</span></span> <span data-ttu-id="881fd-156">Wenn Sie die App ausführen, wird eine andere Portnummer angezeigt.</span><span class="sxs-lookup"><span data-stu-id="881fd-156">When you run the app, you'll see a different port number.</span></span>
* <span data-ttu-id="881fd-157">Das Starten der App mit **STRG+F5** (Nicht-Debugmodus) ermöglicht die Änderung des Codes, das Speichern der Datei, das Aktualisieren des Browsers und das Anzeigen von Codeänderungen.</span><span class="sxs-lookup"><span data-stu-id="881fd-157">Launching the app with **Ctrl+F5** (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="881fd-158">Viele Entwickler bevorzugen den Nicht-Debugmodus, um die App schnell zu starten und Änderungen anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="881fd-158">Many developers prefer to use non-debug mode to quickly launch the app and view changes.</span></span>
* <span data-ttu-id="881fd-159">Sie können die App über das Menüelement **Debuggen** im Debugmodus oder Nicht-Debugmodus starten:</span><span class="sxs-lookup"><span data-stu-id="881fd-159">You can launch the app in debug or non-debug mode from the **Debug** menu item:</span></span>

![Menü „Debuggen“](start-mvc/_static/debug_menu.png)

* <span data-ttu-id="881fd-161">Sie können die App debuggen, indem Sie auf die Schaltfläche **IIS Express** tippen.</span><span class="sxs-lookup"><span data-stu-id="881fd-161">You can debug the app by tapping the **IIS Express** button</span></span>

![IIS Express](start-mvc/_static/iis_express.png)

<span data-ttu-id="881fd-163">Die Standardvorlage bietet Ihnen funktionierende Links für **Startseite, Info** und **Kontakt**.</span><span class="sxs-lookup"><span data-stu-id="881fd-163">The default template gives you working **Home, About** and **Contact** links.</span></span> <span data-ttu-id="881fd-164">Die obenstehende Browserabbildung zeigt diese Links nicht an.</span><span class="sxs-lookup"><span data-stu-id="881fd-164">The browser image above doesn't show these links.</span></span> <span data-ttu-id="881fd-165">Je nach Größe Ihres Browsers müssen Sie möglicherweise auf das Navigationssymbol klicken, um diese anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="881fd-165">Depending on the size of your browser, you might need to click the navigation icon to show them.</span></span>

![Navigationssymbol oben rechts](start-mvc/_static/2.png)

<span data-ttu-id="881fd-167">Wenn Sie die App im Debugmodus ausgeführt haben, tippen Sie **UMSCHALT+F5**, um das Debuggen zu beenden.</span><span class="sxs-lookup"><span data-stu-id="881fd-167">If you were running in debug mode, tap **Shift-F5** to stop debugging.</span></span>

<span data-ttu-id="881fd-168">Im nächsten Teil dieses Tutorials erfahren Sie mehr über MVC und beginnen mit dem Schreiben von Code.</span><span class="sxs-lookup"><span data-stu-id="881fd-168">In the next part of this tutorial, we'll learn about MVC and start writing some code.</span></span>

>[!div class="step-by-step"]
[<span data-ttu-id="881fd-169">Nächste</span><span class="sxs-lookup"><span data-stu-id="881fd-169">Next</span></span>](adding-controller.md)  
