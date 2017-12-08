---
uid: visual-studio/overview/2013/using-browser-link
title: Browserlink in Visual Studio 2013 mit | Microsoft Docs
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/04/2013
ms.topic: article
ms.assetid: 46cbfe20-b4dc-449b-9016-80657dd44fbe
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /visual-studio/overview/2013/using-browser-link
msc.type: authoredcontent
ms.openlocfilehash: 14f67d81a5b460da591b8fb27fedf53d228e7717
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="using-browser-link-in-visual-studio-2013"></a><span data-ttu-id="b1984-102">Mithilfe der Browserlink in Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="b1984-102">Using Browser Link in Visual Studio 2013</span></span>
====================
<span data-ttu-id="b1984-103">durch [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="b1984-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="b1984-104">Browserlink ist ein neues Feature in Visual Studio 2013, der einen Kommunikationskanal zwischen der Entwicklungs- und eine oder mehrere Webbrowsern erstellt.</span><span class="sxs-lookup"><span data-stu-id="b1984-104">Browser Link is a new feature in Visual Studio 2013 that creates a communication channel between the development environment and one or more web browsers.</span></span> <span data-ttu-id="b1984-105">Browser-Link, um Ihre Webanwendung gleichzeitig in mehreren Browsern aktualisieren können Sie eignet sich für browserübergreifende Tests.</span><span class="sxs-lookup"><span data-stu-id="b1984-105">You can use Browser Link to refresh your web application in several browsers at once, which is useful for cross-browser testing.</span></span>

- [<span data-ttu-id="b1984-106">Browser aktualisieren</span><span class="sxs-lookup"><span data-stu-id="b1984-106">Browser Refresh</span></span>](#browser-refresh)
- [<span data-ttu-id="b1984-107">Die Browserlink-Dashboard anzeigen</span><span class="sxs-lookup"><span data-stu-id="b1984-107">Viewing the Browser Link Dashboard</span></span>](#dashboard)
- [<span data-ttu-id="b1984-108">Browserlink für für statische HTML-Dateien aktivieren</span><span class="sxs-lookup"><span data-stu-id="b1984-108">Enabling Browser Link for Static HTML Files</span></span>](#static-html)
- [<span data-ttu-id="b1984-109">Deaktivieren von Browserlink</span><span class="sxs-lookup"><span data-stu-id="b1984-109">Disabling Browser Link</span></span>](#disabling)
- [<span data-ttu-id="b1984-110">Wie funktioniert es?</span><span class="sxs-lookup"><span data-stu-id="b1984-110">How Does It Work?</span></span>](#how-it-works)

<a id="browser-refresh"></a>
## <a name="browser-refresh"></a><span data-ttu-id="b1984-111">Browser aktualisieren</span><span class="sxs-lookup"><span data-stu-id="b1984-111">Browser Refresh</span></span>

<span data-ttu-id="b1984-112">Mit den Browser zu aktualisieren können Sie mehreren Browsern aktualisieren, die mit Visual Studio mit Browser Link verbunden sind.</span><span class="sxs-lookup"><span data-stu-id="b1984-112">With Browser Refresh, you can refresh multiple browsers that are connected to Visual Studio through Browser Link.</span></span>

<span data-ttu-id="b1984-113">Verwendung von Browser zu aktualisieren, müssen Sie zunächst erstellen Sie eine ASP.NET-Anwendung mit einer der Projektvorlagen.</span><span class="sxs-lookup"><span data-stu-id="b1984-113">To use Browser Refresh, first create an ASP.NET application, using any of the project templates.</span></span> <span data-ttu-id="b1984-114">Debuggen der Anwendung durch Drücken von F5 oder klicken Sie auf das Pfeilsymbol in der Symbolleiste:</span><span class="sxs-lookup"><span data-stu-id="b1984-114">Debug the application by pressing F5 or clicking the arrow icon in the toolbar:</span></span>

![](using-browser-link/_static/image1.png)

<span data-ttu-id="b1984-115">Sie können auch die Dropdownliste zur Auswahl eines bestimmten Browsers zum Debuggen verwenden.</span><span class="sxs-lookup"><span data-stu-id="b1984-115">You can also use the dropdown to select a specific browser for debugging.</span></span>

![](using-browser-link/_static/image2.png)

<span data-ttu-id="b1984-116">Wählen Sie zum Debuggen mit mehreren Browsern **Browserauswahl**.</span><span class="sxs-lookup"><span data-stu-id="b1984-116">To debug with multiple browsers, select **Browse With**.</span></span> <span data-ttu-id="b1984-117">In der **Browserauswahl** Dialogfeld, halten die STRG-Taste, um mehr als ein Browser auszuwählen.</span><span class="sxs-lookup"><span data-stu-id="b1984-117">In the **Browse With** dialog, hold down the CTRL key to select more than one browser.</span></span> <span data-ttu-id="b1984-118">Klicken Sie auf **Durchsuchen** mit den ausgewählten Browsern zu debuggen.</span><span class="sxs-lookup"><span data-stu-id="b1984-118">Click **Browse** to debug with the selected browsers.</span></span> <span data-ttu-id="b1984-119">Browserlink funktioniert auch, wenn Sie einen Browser außerhalb von Visual Studio starten und navigieren Sie zu der URL der Anwendung.</span><span class="sxs-lookup"><span data-stu-id="b1984-119">Browser Link also works if you launch a browser from outside Visual Studio and navigate to the application URL.</span></span>

![](using-browser-link/_static/image3.png)

<span data-ttu-id="b1984-120">Die Browserlink-Steuerelemente befinden sich in der Dropdownliste mit den zirkuläre Pfeilsymbol.</span><span class="sxs-lookup"><span data-stu-id="b1984-120">The Browser Link controls are located in the dropdown with the circular arrow icon.</span></span> <span data-ttu-id="b1984-121">Das Symbol "Pfeil" ist die **aktualisieren** Schaltfläche.</span><span class="sxs-lookup"><span data-stu-id="b1984-121">The arrow icon is the **Refresh** button.</span></span>

![](using-browser-link/_static/image4.png)

<span data-ttu-id="b1984-122">Um festzustellen, welche Browser verbunden sind, zeigen Sie die Maus auf die **aktualisieren** Schaltfläche während des Debuggens.</span><span class="sxs-lookup"><span data-stu-id="b1984-122">To see which browsers are connected, hover the mouse over the **Refresh** button while debugging.</span></span> <span data-ttu-id="b1984-123">Die verbundenen Browser werden in einer QuickInfo-Fenster angezeigt.</span><span class="sxs-lookup"><span data-stu-id="b1984-123">The connected browsers are shown in a ToolTip window.</span></span>

![](using-browser-link/_static/image5.png)

<span data-ttu-id="b1984-124">Um die verbundenen Browser zu aktualisieren, klicken Sie auf die **aktualisieren** klicken, oder drücken Sie STRG + ALT + EINGABETASTE.</span><span class="sxs-lookup"><span data-stu-id="b1984-124">To refresh the connected browsers, click the **Refresh** button or press CTRL+ALT+ENTER.</span></span> <span data-ttu-id="b1984-125">Das folgende Bildschirmfoto zeigt z. B. ein ASP.NET-Projekt, die ich mit der MVC 5-Projektvorlage erstellt.</span><span class="sxs-lookup"><span data-stu-id="b1984-125">For example, the following screenshot shows an ASP.NET project, which I created using the MVC 5 project template.</span></span> <span data-ttu-id="b1984-126">Sie können die Anwendung ausgeführt wird, in zwei Browsern oben sehen.</span><span class="sxs-lookup"><span data-stu-id="b1984-126">You can see the application running in two browsers at the top.</span></span> <span data-ttu-id="b1984-127">Unten ist das Projekt in Visual Studio geöffnet.</span><span class="sxs-lookup"><span data-stu-id="b1984-127">At the bottom, the project is open in Visual Studio.</span></span>

![](using-browser-link/_static/image6.png)

<span data-ttu-id="b1984-128">In Visual Studio geändert ich die &lt;h1&gt; Überschrift auf der Startseite ":</span><span class="sxs-lookup"><span data-stu-id="b1984-128">In Visual Studio, I changed the &lt;h1&gt; heading for the home page:</span></span>

![](using-browser-link/_static/image7.png)

<span data-ttu-id="b1984-129">Ich beim Klicken auf die **aktualisieren** Schaltfläche, die Änderung wurde in beide Browserfenster angezeigt:</span><span class="sxs-lookup"><span data-stu-id="b1984-129">When I clicked the **Refresh** button, the change appeared in both browser windows:</span></span>

![](using-browser-link/_static/image8.png)

<span data-ttu-id="b1984-130">**Notizen**</span><span class="sxs-lookup"><span data-stu-id="b1984-130">**Notes**</span></span>

- <span data-ttu-id="b1984-131">Legen Sie zum Aktivieren der Browserlink `debug=true` in der [ &lt;Kompilierung&gt; ](https://msdn.microsoft.com/en-us/library/s10awwz0(v=vs.85).aspx) Element in der Datei Web.config des Projekts.</span><span class="sxs-lookup"><span data-stu-id="b1984-131">To enable Browser Link, set `debug=true` in the [&lt;compilation&gt;](https://msdn.microsoft.com/en-us/library/s10awwz0(v=vs.85).aspx) element in the project's Web.config file.</span></span>
- <span data-ttu-id="b1984-132">Die Anwendung muss auf "localhost" ausgeführt werden.</span><span class="sxs-lookup"><span data-stu-id="b1984-132">The application must be running on localhost.</span></span>
- <span data-ttu-id="b1984-133">Die Anwendung muss .NET 4.0 oder höher abzielen.</span><span class="sxs-lookup"><span data-stu-id="b1984-133">The application must target .NET 4.0 or later.</span></span>

<a id="dashboard"></a>
## <a name="viewing-the-browser-link-dashboard"></a><span data-ttu-id="b1984-134">Die Browserlink-Dashboard anzeigen</span><span class="sxs-lookup"><span data-stu-id="b1984-134">Viewing the Browser Link Dashboard</span></span>

<span data-ttu-id="b1984-135">Die Browserlink-Dashboard zeigt Informationen zu den Browserlink-Verbindungen.</span><span class="sxs-lookup"><span data-stu-id="b1984-135">The Browser Link dashboard shows information about the Browser Link connections.</span></span> <span data-ttu-id="b1984-136">Wählen Sie zum Anzeigen eines Dashboards, die Browserlink-Dropdownmenü (den kleinen Pfeil neben der **aktualisieren** Schaltfläche).</span><span class="sxs-lookup"><span data-stu-id="b1984-136">To view the dashboard, select the Browser Link dropdown menu (the small arrow next to the **Refresh** button).</span></span> <span data-ttu-id="b1984-137">Klicken Sie dann auf **Browserlink-Dashboard**.</span><span class="sxs-lookup"><span data-stu-id="b1984-137">Then click **Browser Link Dashboard**.</span></span>

![](using-browser-link/_static/image9.png)

<span data-ttu-id="b1984-138">Das Dashboard Listet den verbundenen Browsern und die URL, zu dem jeweiligen Browser navigiert ist.</span><span class="sxs-lookup"><span data-stu-id="b1984-138">The dashboard lists the connected Browsers and the URL to which each browser has navigated.</span></span>

![](using-browser-link/_static/image10.png)

<span data-ttu-id="b1984-139">Die **Voraussetzungen** Abschnitt zeigt alle Schritte zum Aktivieren der Browserlink für dieses Projekt.</span><span class="sxs-lookup"><span data-stu-id="b1984-139">The **Prerequisites** section shows any steps needed to enable Browser Link for that project.</span></span> <span data-ttu-id="b1984-140">Das folgende Bildschirmfoto zeigt z. B. ein Projekt, "debug" auf "false" in der Datei "Web.config" festgelegt ist.</span><span class="sxs-lookup"><span data-stu-id="b1984-140">For example, the following screenshot shows a project where "debug" is set to false in the Web.config file.</span></span>

![](using-browser-link/_static/image11.png)

<a id="static-html"></a>
## <a name="enabling-browser-link-for-static-html-files"></a><span data-ttu-id="b1984-141">Browserlink für für statische HTML-Dateien aktivieren</span><span class="sxs-lookup"><span data-stu-id="b1984-141">Enabling Browser Link for Static HTML Files</span></span>

<span data-ttu-id="b1984-142">Um Browserlink für statische HTML-Dateien zu aktivieren, fügen Sie Folgendes in die Datei "Web.config" ein.</span><span class="sxs-lookup"><span data-stu-id="b1984-142">To enable Browser Link for static HTML files, add the following to your Web.config file.</span></span>

[!code-xml[Main](using-browser-link/samples/sample1.xml)]

<span data-ttu-id="b1984-143">Entfernen Sie diese Einstellung, wenn Sie Ihr Projekt veröffentlichen, aus Gründen der Leistung.</span><span class="sxs-lookup"><span data-stu-id="b1984-143">For performance reasons, remove this setting when you publish your project.</span></span>

<a id="disabling"></a>
## <a name="disabling-browser-link"></a><span data-ttu-id="b1984-144">Deaktivieren von Browserlink</span><span class="sxs-lookup"><span data-stu-id="b1984-144">Disabling Browser Link</span></span>

<span data-ttu-id="b1984-145">Browserlink ist standardmäßig aktiviert.</span><span class="sxs-lookup"><span data-stu-id="b1984-145">Browser Link is enabled by default.</span></span> <span data-ttu-id="b1984-146">Es gibt mehrere Möglichkeiten, um ihn zu deaktivieren:</span><span class="sxs-lookup"><span data-stu-id="b1984-146">There are several ways to disable it:</span></span>

- <span data-ttu-id="b1984-147">Deaktivieren Sie auf dem Browserlink-Dropdown-Menü **Browserlink aktivieren**.</span><span class="sxs-lookup"><span data-stu-id="b1984-147">In the Browser Link dropdown menu, uncheck **Enable Browser Link**.</span></span> 

    ![](using-browser-link/_static/image12.png)
- <span data-ttu-id="b1984-148">Fügen Sie einen Schlüssel namens "Vs: EnableBrowserLink" mit dem Wert "False" im Abschnitt "appSettings" in der Datei "Web.config" hinzu.</span><span class="sxs-lookup"><span data-stu-id="b1984-148">In the Web.config file, add a key named "vs:EnableBrowserLink" with the value "false" in the appSettings section.</span></span> 

    [!code-xml[Main](using-browser-link/samples/sample2.xml)]
- <span data-ttu-id="b1984-149">Festlegen von Debug-in der Datei "Web.config" auf "false".</span><span class="sxs-lookup"><span data-stu-id="b1984-149">In the Web.config file, set debug to false.</span></span> 

    [!code-xml[Main](using-browser-link/samples/sample3.xml)]

<a id="how-it-works"></a>
## <a name="how-does-it-work"></a><span data-ttu-id="b1984-150">Wie funktioniert es?</span><span class="sxs-lookup"><span data-stu-id="b1984-150">How Does It Work?</span></span>

<span data-ttu-id="b1984-151">Browser Replikationsverkehr [SignalR](../../../signalr/index.md) um einen Kommunikationskanal zwischen Visual Studio und den Browser zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="b1984-151">Browser Link uses [SignalR](../../../signalr/index.md) to create a communication channel between Visual Studio and the browser.</span></span> <span data-ttu-id="b1984-152">Wenn Browserlink aktiviert ist, verhält sich Visual Studio als SignalR-Server, dem mehrere Clients (Browser), eine Verbindung herstellen können.</span><span class="sxs-lookup"><span data-stu-id="b1984-152">When Browser Link is enabled, Visual Studio acts as a SignalR server that multiple clients (browsers) can connect to.</span></span> <span data-ttu-id="b1984-153">Browserlink registriert auch ein HTTP-Modul mit ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="b1984-153">Browser Link also registers an HTTP module with ASP.NET.</span></span> <span data-ttu-id="b1984-154">Dieses Modul fügt spezielle &lt;Skript&gt; Verweise in jede Seitenanforderung vom Server.</span><span class="sxs-lookup"><span data-stu-id="b1984-154">This module injects special &lt;script&gt; references into every page request from the server.</span></span> <span data-ttu-id="b1984-155">Sie können die Skriptverweise anzeigen, indem Sie Sie im Browser "Quelltext anzeigen" auswählen.</span><span class="sxs-lookup"><span data-stu-id="b1984-155">You can see the script references by selecting "View source" in the browser.</span></span>

![](using-browser-link/_static/image13.png)

<span data-ttu-id="b1984-156">Die Quelldateien werden nicht geändert.</span><span class="sxs-lookup"><span data-stu-id="b1984-156">Your source files are not modified.</span></span> <span data-ttu-id="b1984-157">Das HTTP-Modul fügt die Skriptverweise dynamisch.</span><span class="sxs-lookup"><span data-stu-id="b1984-157">The HTTP module injects the script references dynamically.</span></span>

<span data-ttu-id="b1984-158">Da der Browser clientseitigen Code alle JavaScript ist, funktioniert in allen Browsern, [SignalR unterstützt](../../../signalr/overview/getting-started/supported-platforms.md), ohne dass einen beliebigen Browser-Plug-in.</span><span class="sxs-lookup"><span data-stu-id="b1984-158">Because the browser-side code is all JavaScript, it works on all browsers that [SignalR supports](../../../signalr/overview/getting-started/supported-platforms.md), without requiring any browser plug-in.</span></span>
