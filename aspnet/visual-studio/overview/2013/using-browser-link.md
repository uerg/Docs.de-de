---
uid: visual-studio/overview/2013/using-browser-link
title: Verwenden einer Browserverknüpfung in Visual Studio 2013 | Microsoft-Dokumentation
author: MikeWasson
description: ''
ms.author: riande
ms.date: 10/04/2013
ms.assetid: 46cbfe20-b4dc-449b-9016-80657dd44fbe
msc.legacyurl: /visual-studio/overview/2013/using-browser-link
msc.type: authoredcontent
ms.openlocfilehash: f470aa7e425d16aec3f67d2a0ebb664a3e7eac41
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2018
ms.locfileid: "41832720"
---
<a name="using-browser-link-in-visual-studio-2013"></a><span data-ttu-id="994e2-102">Verwenden einer Browserverknüpfung in Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="994e2-102">Using Browser Link in Visual Studio 2013</span></span>
====================
<span data-ttu-id="994e2-103">durch [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="994e2-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="994e2-104">Browserverknüpfung ist ein neues Feature in Visual Studio 2013, der einen Kommunikationskanal zwischen der Entwicklungs- und eine oder mehrere Webbrowser erstellt.</span><span class="sxs-lookup"><span data-stu-id="994e2-104">Browser Link is a new feature in Visual Studio 2013 that creates a communication channel between the development environment and one or more web browsers.</span></span> <span data-ttu-id="994e2-105">Sie können Browser Link, um Ihre Webanwendung in mehreren Browsern gleichzeitig zu aktualisieren, dies ist hilfreich für browserübergreifende Tests verwenden.</span><span class="sxs-lookup"><span data-stu-id="994e2-105">You can use Browser Link to refresh your web application in several browsers at once, which is useful for cross-browser testing.</span></span>

- [<span data-ttu-id="994e2-106">Aufrufbrowser aktualisieren</span><span class="sxs-lookup"><span data-stu-id="994e2-106">Browser Refresh</span></span>](#browser-refresh)
- [<span data-ttu-id="994e2-107">Die Browserlink-Dashboard anzeigen</span><span class="sxs-lookup"><span data-stu-id="994e2-107">Viewing the Browser Link Dashboard</span></span>](#dashboard)
- [<span data-ttu-id="994e2-108">Aktivieren von Browserlink, um statische HTML-Dateien</span><span class="sxs-lookup"><span data-stu-id="994e2-108">Enabling Browser Link for Static HTML Files</span></span>](#static-html)
- [<span data-ttu-id="994e2-109">Deaktivieren von Browserlink</span><span class="sxs-lookup"><span data-stu-id="994e2-109">Disabling Browser Link</span></span>](#disabling)
- [<span data-ttu-id="994e2-110">Wie funktioniert es?</span><span class="sxs-lookup"><span data-stu-id="994e2-110">How Does It Work?</span></span>](#how-it-works)

<a id="browser-refresh"></a>
## <a name="browser-refresh"></a><span data-ttu-id="994e2-111">Aufrufbrowser aktualisieren</span><span class="sxs-lookup"><span data-stu-id="994e2-111">Browser Refresh</span></span>

<span data-ttu-id="994e2-112">Mit den Browser zu aktualisieren können Sie mehrere Browser aktualisieren, die in Visual Studio über Browser Link verbunden sind.</span><span class="sxs-lookup"><span data-stu-id="994e2-112">With Browser Refresh, you can refresh multiple browsers that are connected to Visual Studio through Browser Link.</span></span>

<span data-ttu-id="994e2-113">Um den Browser aktualisieren, verwenden zu können, müssen Sie zuerst erstellen Sie eine ASP.NET-Anwendung mit einer der Projektvorlagen.</span><span class="sxs-lookup"><span data-stu-id="994e2-113">To use Browser Refresh, first create an ASP.NET application, using any of the project templates.</span></span> <span data-ttu-id="994e2-114">Debuggen der Anwendung durch Drücken von F5 oder durch Klicken auf das Pfeilsymbol in der Symbolleiste:</span><span class="sxs-lookup"><span data-stu-id="994e2-114">Debug the application by pressing F5 or clicking the arrow icon in the toolbar:</span></span>

![](using-browser-link/_static/image1.png)

<span data-ttu-id="994e2-115">Sie können auch die Dropdownliste zur Auswahl eines bestimmten Browsers zum Debuggen verwenden.</span><span class="sxs-lookup"><span data-stu-id="994e2-115">You can also use the dropdown to select a specific browser for debugging.</span></span>

![](using-browser-link/_static/image2.png)

<span data-ttu-id="994e2-116">Wählen Sie zum Debuggen mit mehreren Browsern **Browserauswahl**.</span><span class="sxs-lookup"><span data-stu-id="994e2-116">To debug with multiple browsers, select **Browse With**.</span></span> <span data-ttu-id="994e2-117">In der **Browserauswahl** Dialogfeld, halten Sie die STRG-Taste mehrere Browser auswählen.</span><span class="sxs-lookup"><span data-stu-id="994e2-117">In the **Browse With** dialog, hold down the CTRL key to select more than one browser.</span></span> <span data-ttu-id="994e2-118">Klicken Sie auf **Durchsuchen** zum Debuggen mit den ausgewählten Browsern.</span><span class="sxs-lookup"><span data-stu-id="994e2-118">Click **Browse** to debug with the selected browsers.</span></span> <span data-ttu-id="994e2-119">Browserverknüpfung funktioniert auch, wenn Sie einen Browser außerhalb von Visual Studio startet, und navigieren Sie zu der Anwendungs-URL.</span><span class="sxs-lookup"><span data-stu-id="994e2-119">Browser Link also works if you launch a browser from outside Visual Studio and navigate to the application URL.</span></span>

![](using-browser-link/_static/image3.png)

<span data-ttu-id="994e2-120">Die Browserlink-Steuerelemente befinden sich in der Dropdownliste mit dem kreisförmigen Pfeil-Symbol.</span><span class="sxs-lookup"><span data-stu-id="994e2-120">The Browser Link controls are located in the dropdown with the circular arrow icon.</span></span> <span data-ttu-id="994e2-121">Das Symbol "Pfeil" ist die **aktualisieren** Schaltfläche.</span><span class="sxs-lookup"><span data-stu-id="994e2-121">The arrow icon is the **Refresh** button.</span></span>

![](using-browser-link/_static/image4.png)

<span data-ttu-id="994e2-122">Um anzuzeigen, welche Browser verbunden sind, zeigen Sie auf den die **aktualisieren** Schaltfläche während des Debuggens.</span><span class="sxs-lookup"><span data-stu-id="994e2-122">To see which browsers are connected, hover the mouse over the **Refresh** button while debugging.</span></span> <span data-ttu-id="994e2-123">Die verbundenen Browser werden in einer QuickInfo-Fenster angezeigt.</span><span class="sxs-lookup"><span data-stu-id="994e2-123">The connected browsers are shown in a ToolTip window.</span></span>

![](using-browser-link/_static/image5.png)

<span data-ttu-id="994e2-124">Um die verbundenen Browser zu aktualisieren, klicken Sie auf die **aktualisieren** Schaltfläche oder drücken Sie STRG + ALT + Eingabe.</span><span class="sxs-lookup"><span data-stu-id="994e2-124">To refresh the connected browsers, click the **Refresh** button or press CTRL+ALT+ENTER.</span></span> <span data-ttu-id="994e2-125">Der folgende Screenshot zeigt beispielsweise ein ASP.NET-Projekt, den ich erstellt, mit der MVC 5-Projektvorlage.</span><span class="sxs-lookup"><span data-stu-id="994e2-125">For example, the following screenshot shows an ASP.NET project, which I created using the MVC 5 project template.</span></span> <span data-ttu-id="994e2-126">Sie sehen, dass die Anwendung in beiden Browsern oben ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="994e2-126">You can see the application running in two browsers at the top.</span></span> <span data-ttu-id="994e2-127">Unten ist das Projekt in Visual Studio geöffnet.</span><span class="sxs-lookup"><span data-stu-id="994e2-127">At the bottom, the project is open in Visual Studio.</span></span>

![](using-browser-link/_static/image6.png)

<span data-ttu-id="994e2-128">In Visual Studio geändert ich die &lt;h1&gt; Überschrift auf der Startseite:</span><span class="sxs-lookup"><span data-stu-id="994e2-128">In Visual Studio, I changed the &lt;h1&gt; heading for the home page:</span></span>

![](using-browser-link/_static/image7.png)

<span data-ttu-id="994e2-129">Ich beim Klicken auf die **aktualisieren** Schaltfläche, die Änderung in beide Browserfenster angezeigt:</span><span class="sxs-lookup"><span data-stu-id="994e2-129">When I clicked the **Refresh** button, the change appeared in both browser windows:</span></span>

![](using-browser-link/_static/image8.png)

<span data-ttu-id="994e2-130">**Notizen**</span><span class="sxs-lookup"><span data-stu-id="994e2-130">**Notes**</span></span>

- <span data-ttu-id="994e2-131">Legen Sie zum Aktivieren des Browserlinks `debug=true` in die [ &lt;Kompilierung&gt; ](https://msdn.microsoft.com/library/s10awwz0(v=vs.85).aspx) Element in der Datei Web.config des Projekts.</span><span class="sxs-lookup"><span data-stu-id="994e2-131">To enable Browser Link, set `debug=true` in the [&lt;compilation&gt;](https://msdn.microsoft.com/library/s10awwz0(v=vs.85).aspx) element in the project's Web.config file.</span></span>
- <span data-ttu-id="994e2-132">Die Anwendung muss auf "localhost" ausgeführt werden.</span><span class="sxs-lookup"><span data-stu-id="994e2-132">The application must be running on localhost.</span></span>
- <span data-ttu-id="994e2-133">Die Anwendung muss .NET 4.0 oder höher verweisen können.</span><span class="sxs-lookup"><span data-stu-id="994e2-133">The application must target .NET 4.0 or later.</span></span>

<a id="dashboard"></a>
## <a name="viewing-the-browser-link-dashboard"></a><span data-ttu-id="994e2-134">Die Browserlink-Dashboard anzeigen</span><span class="sxs-lookup"><span data-stu-id="994e2-134">Viewing the Browser Link Dashboard</span></span>

<span data-ttu-id="994e2-135">Die Browserlink-Dashboard zeigt Informationen zu den Browserlink-Verbindungen.</span><span class="sxs-lookup"><span data-stu-id="994e2-135">The Browser Link dashboard shows information about the Browser Link connections.</span></span> <span data-ttu-id="994e2-136">Um das Dashboard anzuzeigen, wählen Sie im Dropdownmenü mit den Browser Link (den kleinen Pfeil neben der **aktualisieren** Schaltfläche).</span><span class="sxs-lookup"><span data-stu-id="994e2-136">To view the dashboard, select the Browser Link dropdown menu (the small arrow next to the **Refresh** button).</span></span> <span data-ttu-id="994e2-137">Klicken Sie dann auf **Browserlink-Dashboard**.</span><span class="sxs-lookup"><span data-stu-id="994e2-137">Then click **Browser Link Dashboard**.</span></span>

![](using-browser-link/_static/image9.png)

<span data-ttu-id="994e2-138">Die verbundenen Browser und die URL, zu dem jeweiligen Browser navigiert ist, werden im Dashboard aufgeführt.</span><span class="sxs-lookup"><span data-stu-id="994e2-138">The dashboard lists the connected Browsers and the URL to which each browser has navigated.</span></span>

![](using-browser-link/_static/image10.png)

<span data-ttu-id="994e2-139">Die **Voraussetzungen** Abschnitt wird gezeigt, alle erforderlichen Schritte zum Aktivieren von Browser Link für das Projekt.</span><span class="sxs-lookup"><span data-stu-id="994e2-139">The **Prerequisites** section shows any steps needed to enable Browser Link for that project.</span></span> <span data-ttu-id="994e2-140">Der folgende Screenshot zeigt beispielsweise ein Projekt, in denen "debug" auf "false" in der Datei "Web.config" festgelegt ist.</span><span class="sxs-lookup"><span data-stu-id="994e2-140">For example, the following screenshot shows a project where "debug" is set to false in the Web.config file.</span></span>

![](using-browser-link/_static/image11.png)

<a id="static-html"></a>
## <a name="enabling-browser-link-for-static-html-files"></a><span data-ttu-id="994e2-141">Aktivieren von Browserlink, um statische HTML-Dateien</span><span class="sxs-lookup"><span data-stu-id="994e2-141">Enabling Browser Link for Static HTML Files</span></span>

<span data-ttu-id="994e2-142">Fügen Sie Folgendes zum Aktivieren von Browser Link für statische HTML-Dateien zu Ihrer Datei "Web.config".</span><span class="sxs-lookup"><span data-stu-id="994e2-142">To enable Browser Link for static HTML files, add the following to your Web.config file.</span></span>

[!code-xml[Main](using-browser-link/samples/sample1.xml)]

<span data-ttu-id="994e2-143">Zur Verbesserung der Leistung entfernen Sie diese Einstellung, wenn Sie Ihr Projekt veröffentlichen.</span><span class="sxs-lookup"><span data-stu-id="994e2-143">For performance reasons, remove this setting when you publish your project.</span></span>

<a id="disabling"></a>
## <a name="disabling-browser-link"></a><span data-ttu-id="994e2-144">Deaktivieren von Browserlink</span><span class="sxs-lookup"><span data-stu-id="994e2-144">Disabling Browser Link</span></span>

<span data-ttu-id="994e2-145">Browserverknüpfung ist standardmäßig aktiviert.</span><span class="sxs-lookup"><span data-stu-id="994e2-145">Browser Link is enabled by default.</span></span> <span data-ttu-id="994e2-146">Es gibt mehrere Möglichkeiten, um ihn zu deaktivieren:</span><span class="sxs-lookup"><span data-stu-id="994e2-146">There are several ways to disable it:</span></span>

- <span data-ttu-id="994e2-147">Deaktivieren Sie im Dropdownmenü mit den Browserlink **Aktivieren des Browserlinks**.</span><span class="sxs-lookup"><span data-stu-id="994e2-147">In the Browser Link dropdown menu, uncheck **Enable Browser Link**.</span></span> 

    ![](using-browser-link/_static/image12.png)
- <span data-ttu-id="994e2-148">Fügen Sie in der Datei "Web.config" einen Schlüssel namens "Vs: EnableBrowserLink" durch den Wert "False" im Abschnitt "appSettings" hinzu.</span><span class="sxs-lookup"><span data-stu-id="994e2-148">In the Web.config file, add a key named "vs:EnableBrowserLink" with the value "false" in the appSettings section.</span></span> 

    [!code-xml[Main](using-browser-link/samples/sample2.xml)]
- <span data-ttu-id="994e2-149">Festlegen von Debug-in der Datei "Web.config" auf "false".</span><span class="sxs-lookup"><span data-stu-id="994e2-149">In the Web.config file, set debug to false.</span></span> 

    [!code-xml[Main](using-browser-link/samples/sample3.xml)]

<a id="how-it-works"></a>
## <a name="how-does-it-work"></a><span data-ttu-id="994e2-150">Wie funktioniert es?</span><span class="sxs-lookup"><span data-stu-id="994e2-150">How Does It Work?</span></span>

<span data-ttu-id="994e2-151">Browserverknüpfung verwendet [SignalR](../../../signalr/index.md) um einen Kommunikationskanal zwischen Visual Studio und den Browser zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="994e2-151">Browser Link uses [SignalR](../../../signalr/index.md) to create a communication channel between Visual Studio and the browser.</span></span> <span data-ttu-id="994e2-152">Wenn Browser Link aktiviert ist, fungiert Visual Studio als SignalR-Server, dem mehrere Clients (Browser) herstellen können.</span><span class="sxs-lookup"><span data-stu-id="994e2-152">When Browser Link is enabled, Visual Studio acts as a SignalR server that multiple clients (browsers) can connect to.</span></span> <span data-ttu-id="994e2-153">Browserverknüpfung registriert ein HTTP-Modul auch mit ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="994e2-153">Browser Link also registers an HTTP module with ASP.NET.</span></span> <span data-ttu-id="994e2-154">Dieses Modul fügt spezielle &lt;Skript&gt; verweisen in jede Anforderung einer Seite vom Server.</span><span class="sxs-lookup"><span data-stu-id="994e2-154">This module injects special &lt;script&gt; references into every page request from the server.</span></span> <span data-ttu-id="994e2-155">Sie können die Skriptverweise sehen, indem Sie Sie im Browser "Quelltext anzeigen" auswählen.</span><span class="sxs-lookup"><span data-stu-id="994e2-155">You can see the script references by selecting "View source" in the browser.</span></span>

![](using-browser-link/_static/image13.png)

<span data-ttu-id="994e2-156">Die Quelldateien werden nicht geändert werden.</span><span class="sxs-lookup"><span data-stu-id="994e2-156">Your source files are not modified.</span></span> <span data-ttu-id="994e2-157">Das HTTP-Modul fügt die Skriptverweise dynamisch.</span><span class="sxs-lookup"><span data-stu-id="994e2-157">The HTTP module injects the script references dynamically.</span></span>

<span data-ttu-id="994e2-158">Da der Browser-Side-Code alle JavaScript ist, funktioniert in allen Browsern, [SignalR unterstützt](../../../signalr/overview/getting-started/supported-platforms.md), ohne dass einen beliebigen Browser-Plug-in.</span><span class="sxs-lookup"><span data-stu-id="994e2-158">Because the browser-side code is all JavaScript, it works on all browsers that [SignalR supports](../../../signalr/overview/getting-started/supported-platforms.md), without requiring any browser plug-in.</span></span>
