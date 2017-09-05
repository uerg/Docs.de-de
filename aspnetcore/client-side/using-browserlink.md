---
title: Browserlink in ASP.NET Core
author: ncarandini
description: Ein Visual Studio-Feature, das links von der Entwicklungsumgebung mit mindestens einem Webbrowser
keywords: ASP.NET Core, browserlink, CSS-Synchronisierung
ms.author: riande
manager: wpickett
ms.date: 12/28/2016
ms.topic: article
ms.assetid: 11813d4c-3f8a-445a-b23b-e4a57d001abc
ms.technology: aspnet
ms.prod: asp.net-core
uid: client-side/using-browserlink
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: b2ff38288cee3e9ca42a07c219521bb79a00a359
ms.sourcegitcommit: 4e84d8bf5f404bb77f3d41665cf7e7374fc39142
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/05/2017
---
# <a name="introduction-to-browser-link-in-aspnet-core"></a><span data-ttu-id="d9f36-104">Einführung in ASP.NET Core Browserlink</span><span class="sxs-lookup"><span data-stu-id="d9f36-104">Introduction to Browser Link in ASP.NET Core</span></span> 

<span data-ttu-id="d9f36-105">Durch [Nicolò Carandini](https://github.com/ncarandini), [Mike Wasson](https://github.com/MikeWasson), und [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="d9f36-105">By [Nicolò Carandini](https://github.com/ncarandini), [Mike Wasson](https://github.com/MikeWasson), and [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="d9f36-106">Browserlink ist ein Feature in Visual Studio, der einen Kommunikationskanal zwischen der Entwicklungs- und eine oder mehrere Webbrowsern erstellt.</span><span class="sxs-lookup"><span data-stu-id="d9f36-106">Browser Link is a feature in Visual Studio that creates a communication channel between the development environment and one or more web browsers.</span></span> <span data-ttu-id="d9f36-107">Browser-Link, um Ihre Webanwendung gleichzeitig in mehreren Browsern aktualisieren können Sie eignet sich für browserübergreifende Tests.</span><span class="sxs-lookup"><span data-stu-id="d9f36-107">You can use Browser Link to refresh your web application in several browsers at once, which is useful for cross-browser testing.</span></span>

## <a name="browser-link-setup"></a><span data-ttu-id="d9f36-108">Setup für Browser Link</span><span class="sxs-lookup"><span data-stu-id="d9f36-108">Browser Link setup</span></span>

<span data-ttu-id="d9f36-109">Die ASP.NET Core **Webanwendung** -Projektvorlagen in Visual Studio 2015 und höher enthalten alles, was für Browserlink benötigen.</span><span class="sxs-lookup"><span data-stu-id="d9f36-109">The ASP.NET Core **Web Application** project templates in Visual Studio 2015 and later include everything needed for Browser Link.</span></span>

<span data-ttu-id="d9f36-110">Browser-Link zu einem Projekt hinzugefügt, die Sie erstellt mithilfe der ASP.NET Core **leere** oder **Web-API** Vorlage, gehen Sie folgendermaßen vor:</span><span class="sxs-lookup"><span data-stu-id="d9f36-110">To add Browser Link to a project that you created by using the ASP.NET Core **Empty** or **Web API** template, follow these steps:</span></span>

1. <span data-ttu-id="d9f36-111">Hinzufügen der *Microsoft.VisualStudio.Web.BrowserLink.Loader* Paket</span><span class="sxs-lookup"><span data-stu-id="d9f36-111">Add the *Microsoft.VisualStudio.Web.BrowserLink.Loader* package</span></span> 
2. <span data-ttu-id="d9f36-112">Hinzufügen von Konfigurationscode in die *Startup.cs* Datei.</span><span class="sxs-lookup"><span data-stu-id="d9f36-112">Add configuration code in the *Startup.cs* file.</span></span>

### <a name="add-the-package"></a><span data-ttu-id="d9f36-113">Fügen Sie dem Paket hinzu</span><span class="sxs-lookup"><span data-stu-id="d9f36-113">Add the package</span></span>

<span data-ttu-id="d9f36-114">Da dies eine Visual Studio-Funktion ist, ist die einfachste Möglichkeit, das Paket hinzuzufügen, öffnen Sie die **Package Manager Console** (**Ansicht > Weitere Fenster > Package Manager Console**), und führen Sie den folgenden Befehl aus:</span><span class="sxs-lookup"><span data-stu-id="d9f36-114">Since this is a Visual Studio feature, the easiest way to add the package is to open the **Package Manager Console** (**View > Other Windows > Package Manager Console**) and run the following command:</span></span>

```console
install-package Microsoft.VisualStudio.Web.BrowserLink.Loader
```

<span data-ttu-id="d9f36-115">Alternativ können Sie **NuGet Package Manager**.</span><span class="sxs-lookup"><span data-stu-id="d9f36-115">Alternatively, you can use **NuGet Package Manager**.</span></span>  <span data-ttu-id="d9f36-116">Mit der rechten Maustaste in des Namens des Projekts **Projektmappen-Explorer**, und wählen Sie **NuGet-Pakete verwalten**.</span><span class="sxs-lookup"><span data-stu-id="d9f36-116">Right-click the project name in **Solution Explorer**, and choose **Manage NuGet Packages**.</span></span> 

![Öffnen von NuGet-Paket-Manager](using-browserlink/_static/open-nuget-package-manager.png)

<span data-ttu-id="d9f36-118">Klicken Sie dann suchen Sie und installieren Sie das Paket.</span><span class="sxs-lookup"><span data-stu-id="d9f36-118">Then find and install the package.</span></span>

![Paket mit NuGet-Paket-Manager hinzufügen](using-browserlink/_static/add-package-with-nuget-package-manager.png)

### <a name="add-configuration-code"></a><span data-ttu-id="d9f36-120">Fügen Sie Konfigurationscode hinzu.</span><span class="sxs-lookup"><span data-stu-id="d9f36-120">Add configuration code</span></span>

<span data-ttu-id="d9f36-121">Öffnen der *Startup.cs* Datei, und klicken Sie in der `Configure` Methode fügen Sie den folgenden Code hinzu:</span><span class="sxs-lookup"><span data-stu-id="d9f36-121">Open the *Startup.cs* file, and in the `Configure` method add the following code:</span></span>

```csharp
app.UseBrowserLink();
```

<span data-ttu-id="d9f36-122">Dieser Code in der Regel befindet sich innerhalb einer `if` blockieren, die nur in der Entwicklungsumgebung Browserlink ermöglicht, wie hier gezeigt:</span><span class="sxs-lookup"><span data-stu-id="d9f36-122">Usually that code is inside an `if` block that enables Browser Link only in the Development environment, as shown here:</span></span>

<span data-ttu-id="d9f36-123">[!code-csharp[Main](./using-browserlink/sample/BrowserLinkSample/src/BrowserLinkSample/Startup.cs?highlight=1,4&range=40-44)]</span><span class="sxs-lookup"><span data-stu-id="d9f36-123">[!code-csharp[Main](./using-browserlink/sample/BrowserLinkSample/src/BrowserLinkSample/Startup.cs?highlight=1,4&range=40-44)]</span></span>

<span data-ttu-id="d9f36-124">Weitere Informationen finden Sie unter [arbeiten mit mehreren Umgebungen](../fundamentals/environments.md).</span><span class="sxs-lookup"><span data-stu-id="d9f36-124">For more information, see [Working with Multiple Environments](../fundamentals/environments.md).</span></span>

## <a name="how-to-use-browser-link"></a><span data-ttu-id="d9f36-125">Gewusst wie: Verwenden von Browserlink</span><span class="sxs-lookup"><span data-stu-id="d9f36-125">How to use Browser Link</span></span>

<span data-ttu-id="d9f36-126">Wenn Sie eine ASP.NET Core-Projekt geöffnet haben, zeigt Visual Studio das Symbolleistensteuerelement Browserlink neben der **Debugziel** Toolbar-Steuerelement:</span><span class="sxs-lookup"><span data-stu-id="d9f36-126">When you have an ASP.NET Core project open, Visual Studio shows the Browser Link toolbar control next to the **Debug Target** toolbar control:</span></span>

![Browser Link Dropdown-Menü](using-browserlink/_static/browserLink-dropdown-menu.png)

<span data-ttu-id="d9f36-128">Aus dem Browserlink-Toolbar-Steuerelement können Sie folgende Aktionen ausführen:</span><span class="sxs-lookup"><span data-stu-id="d9f36-128">From the Browser Link toolbar control, you can:</span></span>

- <span data-ttu-id="d9f36-129">Aktualisieren Sie die Webanwendung gleichzeitig in mehreren Browsern</span><span class="sxs-lookup"><span data-stu-id="d9f36-129">Refresh the web application in several browsers at once</span></span>
- <span data-ttu-id="d9f36-130">Öffnen der **Browserlink-Dashboard**</span><span class="sxs-lookup"><span data-stu-id="d9f36-130">Open the **Browser Link Dashboard**</span></span>
- <span data-ttu-id="d9f36-131">Aktivieren oder deaktivieren Sie **Browserlink**</span><span class="sxs-lookup"><span data-stu-id="d9f36-131">Enable or disable **Browser Link**</span></span>
- <span data-ttu-id="d9f36-132">Aktivieren Sie oder deaktivieren Sie die automatische CSS-Synchronisierung</span><span class="sxs-lookup"><span data-stu-id="d9f36-132">Enable or disable CSS Auto-Sync</span></span>

> [!NOTE]
> <span data-ttu-id="d9f36-133">Einige Visual Studio-Plug-ins, vor allem Kontrollvorgänge *Web Erweiterung Pack 2015* und *Web Erweiterung Pack 2017*, bieten erweiterte Funktionalität für Browserlink, aber einige zusätzlichen Funktionen, die mit ASP funktionieren nicht. NET Core-Projekte.</span><span class="sxs-lookup"><span data-stu-id="d9f36-133">Some Visual Studio plug-ins, most notably *Web Extension Pack 2015* and *Web Extension Pack 2017*, offer extended functionality for Browser Link, but some of the additional features don't work with ASP.NET Core projects.</span></span>

## <a name="refresh-the-web-application-in-several-browsers-at-once"></a><span data-ttu-id="d9f36-134">Aktualisieren Sie die Webanwendung gleichzeitig in mehreren Browsern</span><span class="sxs-lookup"><span data-stu-id="d9f36-134">Refresh the web application in several browsers at once</span></span>

<span data-ttu-id="d9f36-135">Verwenden Sie zum Auswählen eines einzelnen Web-Browsers zum Starten, wenn das Projekt starten das Dropdown-Menü in der **Debugziel** Toolbar-Steuerelement:</span><span class="sxs-lookup"><span data-stu-id="d9f36-135">To choose a single web browser to launch when starting the project, use the drop-down menu in the **Debug Target** toolbar control:</span></span>

![F5-Dropdownmenü](using-browserlink/_static/debug-target-dropdown-menu.png)

<span data-ttu-id="d9f36-137">Zum Öffnen von mehreren Browsern gleichzeitig wählen **durchsuchen mit...**  aus der gleichen Dropdownliste aus.</span><span class="sxs-lookup"><span data-stu-id="d9f36-137">To open multiple browsers at once, choose **Browse with...** from the same drop-down.</span></span>  <span data-ttu-id="d9f36-138">Halten Sie die STRG-Taste auf die Browsern auswählen, werden soll, und klicken Sie dann auf **Durchsuchen**:</span><span class="sxs-lookup"><span data-stu-id="d9f36-138">Hold down the CTRL key to select the browsers you want, and then click **Browse**:</span></span>

![Viele Browsern gleichzeitig öffnen](using-browserlink/_static/open-many-browsers-at-once.png)

<span data-ttu-id="d9f36-140">Hier ist ein Beispiel Screenshot der Visual Studio mit die Indexansicht öffnen und zwei geöffneten Browser:</span><span class="sxs-lookup"><span data-stu-id="d9f36-140">Here's a sample screenshot showing Visual Studio with the Index view open and two open browsers:</span></span>

![Synchronisieren Sie mit zwei Browser-Beispiel](using-browserlink/_static/sync-with-two-browsers-example.png)

<span data-ttu-id="d9f36-142">Zeigen Sie auf dem Browserlink-Symbolleisten-Steuerelement auf den Browsern, die dem Projekt verbunden sind, finden Sie unter:</span><span class="sxs-lookup"><span data-stu-id="d9f36-142">Hover over the Browser Link toolbar control to see the browsers that are connected to the project:</span></span>

![Zeigen Sie Tipp](using-browserlink/_static/hoover-tip.png)

<span data-ttu-id="d9f36-144">Ändern die Indexansicht und allen verbundenen Browsern werden aktualisiert, wenn Sie auf die Aktualisierungsschaltfläche klicken Browserlink:</span><span class="sxs-lookup"><span data-stu-id="d9f36-144">Change the Index view, and all connected browsers are updated when you click the Browser Link refresh button:</span></span>

![Browser-Synchronisierung auf Änderungen](using-browserlink/_static/browsers-sync-to-changes.png)

<span data-ttu-id="d9f36-146">Browserlink funktioniert auch mit Browsern, die Sie außerhalb von Visual Studio starten, und navigieren Sie zu der URL der Anwendung.</span><span class="sxs-lookup"><span data-stu-id="d9f36-146">Browser Link also works with browsers that you launch from outside Visual Studio and navigate to the application URL.</span></span>

### <a name="the-browser-link-dashboard"></a><span data-ttu-id="d9f36-147">Die Browserlink-Dashboard</span><span class="sxs-lookup"><span data-stu-id="d9f36-147">The Browser Link Dashboard</span></span>

<span data-ttu-id="d9f36-148">Öffnen Sie die Browserlink-Dashboard aus dem Browserlink-Dropdown-Menü zum Verwalten einer Verbindung mit geöffneten Browser:</span><span class="sxs-lookup"><span data-stu-id="d9f36-148">Open the Browser Link Dashboard from the Browser Link drop down menu to manage the connection with open browsers:</span></span>

![Open-Browserslink-dashboard](using-browserlink/_static/open-browserlink-dashboard.png)

<span data-ttu-id="d9f36-150">Wenn keine Browser verbunden ist, können Sie beginnen, nicht Debuggen Sitzung auf die _in Browser anzeigen_ Link:</span><span class="sxs-lookup"><span data-stu-id="d9f36-150">If no browser is connected, you can start a non debugging session clicking the _View in Browser_ link:</span></span>

![Browserlink-Dashboard-ohne-Verbindungen](using-browserlink/_static/browserlink-dashboard-no-connections.png)

<span data-ttu-id="d9f36-152">Andernfalls werden die verbundenen Browser, durch den Pfad zu der Seite angezeigt, die jedem Browser angezeigt wird:</span><span class="sxs-lookup"><span data-stu-id="d9f36-152">Otherwise, the connected browsers are shown, with the path to the page that each browser is showing:</span></span>

![Browserlink-Dashboard-zwei-Verbindungen](using-browserlink/_static/browserlink-dashboard-two-connections.png)

<span data-ttu-id="d9f36-154">Wenn Sie möchten, können Sie auf einen aufgelisteten Browsernamen dieser einzelnen Browsersitzung aktualisieren klicken.</span><span class="sxs-lookup"><span data-stu-id="d9f36-154">If you like, you can click on a listed browser name to refresh that single browser.</span></span>

### <a name="enable-or-disable-browser-link"></a><span data-ttu-id="d9f36-155">Aktivieren oder Deaktivieren von Browserlink</span><span class="sxs-lookup"><span data-stu-id="d9f36-155">Enable or disable Browser Link</span></span>

<span data-ttu-id="d9f36-156">Wenn Sie Browserlink erneut aktivieren nach dem deaktivieren, müssen Sie aktualisieren die Browser, um sie erneut eine Verbindung herstellen.</span><span class="sxs-lookup"><span data-stu-id="d9f36-156">When you re-enable Browser Link after disabling it, you have to refresh the browsers to reconnect them.</span></span>

### <a name="enable-or-disable-css-auto-sync"></a><span data-ttu-id="d9f36-157">Aktivieren Sie oder deaktivieren Sie die automatische CSS-Synchronisierung</span><span class="sxs-lookup"><span data-stu-id="d9f36-157">Enable or disable CSS Auto-Sync</span></span>

<span data-ttu-id="d9f36-158">Wenn die automatische CSS-Synchronisierung aktiviert ist, werden die verbundenen Browsern automatisch aktualisiert, wenn Sie für jede Änderung an der CSS-Dateien vornehmen.</span><span class="sxs-lookup"><span data-stu-id="d9f36-158">When CSS Auto-Sync is enabled, connected browsers are automatically refreshed when you make any change to CSS files.</span></span>

## <a name="how-does-it-work"></a><span data-ttu-id="d9f36-159">Wie funktioniert das?</span><span class="sxs-lookup"><span data-stu-id="d9f36-159">How does it work?</span></span>

<span data-ttu-id="d9f36-160">Browserlink verwendet SignalR, um einen Kommunikationskanal zwischen Visual Studio und den Browser zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="d9f36-160">Browser Link uses SignalR to create a communication channel between Visual Studio and the browser.</span></span> <span data-ttu-id="d9f36-161">Wenn Browserlink aktiviert ist, verhält sich Visual Studio als SignalR-Server, dem mehrere Clients (Browser), eine Verbindung herstellen können.</span><span class="sxs-lookup"><span data-stu-id="d9f36-161">When Browser Link is enabled, Visual Studio acts as a SignalR server that multiple clients (browsers) can connect to.</span></span> <span data-ttu-id="d9f36-162">Browserlink registriert auch eine middlewarekomponente in der ASP.NET-Pipeline für die Anforderung.</span><span class="sxs-lookup"><span data-stu-id="d9f36-162">Browser Link also registers a middleware component in the ASP.NET request pipeline.</span></span> <span data-ttu-id="d9f36-163">Diese Komponente fügt spezielle `<script>` Verweise in jede Seitenanforderung vom Server.</span><span class="sxs-lookup"><span data-stu-id="d9f36-163">This component injects special `<script>` references into every page request from the server.</span></span> <span data-ttu-id="d9f36-164">Sehen Sie dazu die Skriptverweise **Quelltext anzeigen** im Browser, und scrollen bis zum Ende der `<body>` tag-Inhalte:</span><span class="sxs-lookup"><span data-stu-id="d9f36-164">You can see the script references by selecting **View source** in the browser and scrolling to the end of the `<body>` tag content:</span></span>

```javascript
    <!-- Visual Studio Browser Link -->
    <script type="application/json" id="__browserLink_initializationData">
        {"requestId":"a717d5a07c1741949a7cefd6fa2bad08","requestMappingFromServer":false}
    </script>
    <script type="text/javascript" src="http://localhost:54139/b6e36e429d034f578ebccd6a79bf19bf/browserLink" async="async"></script>
    <!-- End Browser Link -->
</body>
```

<span data-ttu-id="d9f36-165">Die Quelldateien werden nicht geändert.</span><span class="sxs-lookup"><span data-stu-id="d9f36-165">Your source files are not modified.</span></span> <span data-ttu-id="d9f36-166">Die Komponente fügt die Skriptverweise dynamisch.</span><span class="sxs-lookup"><span data-stu-id="d9f36-166">The middleware component injects the script references dynamically.</span></span> 

<span data-ttu-id="d9f36-167">Da der Browser clientseitigen Code alle JavaScript ist, funktioniert es in allen Browsern, die SignalR unterstützt, ohne dass einen beliebigen Browser-Plug-in.</span><span class="sxs-lookup"><span data-stu-id="d9f36-167">Because the browser-side code is all JavaScript, it works on all browsers that SignalR supports, without requiring any browser plug-in.</span></span>
