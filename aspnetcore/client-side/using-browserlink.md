---
title: Browserlink in ASP.NET Core
author: ncarandini
description: "Erklärt, wie Browserlink eine Visual Studio-Funktion, die die Entwicklungsumgebung mit mindestens einem Webbrowser verknüpft."
ms.author: riande
manager: wpickett
ms.date: 09/22/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: client-side/using-browserlink
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: d5db65c268923e96c45b034639437fc3496ccac1
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/19/2018
---
# <a name="browser-link-in-aspnet-core"></a><span data-ttu-id="8d009-103">Browserlink in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="8d009-103">Browser Link in ASP.NET Core</span></span> 

<span data-ttu-id="8d009-104">Durch [Nicolò Carandini](https://github.com/ncarandini), [Mike Wasson](https://github.com/MikeWasson), und [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="8d009-104">By [Nicolò Carandini](https://github.com/ncarandini), [Mike Wasson](https://github.com/MikeWasson), and [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="8d009-105">Browserlink ist ein Feature in Visual Studio, der einen Kommunikationskanal zwischen der Entwicklungs- und eine oder mehrere Webbrowsern erstellt.</span><span class="sxs-lookup"><span data-stu-id="8d009-105">Browser Link is a feature in Visual Studio that creates a communication channel between the development environment and one or more web browsers.</span></span> <span data-ttu-id="8d009-106">Browser-Link, um Ihre Webanwendung gleichzeitig in mehreren Browsern aktualisieren können Sie eignet sich für browserübergreifende Tests.</span><span class="sxs-lookup"><span data-stu-id="8d009-106">You can use Browser Link to refresh your web application in several browsers at once, which is useful for cross-browser testing.</span></span>

## <a name="browser-link-setup"></a><span data-ttu-id="8d009-107">Setup für Browser Link</span><span class="sxs-lookup"><span data-stu-id="8d009-107">Browser Link setup</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="8d009-108">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="8d009-108">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="8d009-109">Die ASP.NET Core 2.x **Webanwendung**, **leere**, und **Web-API** Vorlage Projekte verwenden die [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All/) Meta-Paket, das eine Paket-Referenz für enthält [Microsoft.VisualStudio.Web.BrowserLink](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.BrowserLink/).</span><span class="sxs-lookup"><span data-stu-id="8d009-109">The ASP.NET Core 2.x **Web Application**, **Empty**, and **Web API** template projects use the [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All/) meta-package, which contains a package reference for [Microsoft.VisualStudio.Web.BrowserLink](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.BrowserLink/).</span></span> <span data-ttu-id="8d009-110">Verwenden Sie deshalb die `Microsoft.AspNetCore.All` metapaket erfordert keine weitere Aktion um Browserlink für die Verwendung verfügbar zu machen.</span><span class="sxs-lookup"><span data-stu-id="8d009-110">Therefore, using the `Microsoft.AspNetCore.All` meta-package requires no further action to make Browser Link available for use.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="8d009-111">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="8d009-111">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="8d009-112">Die ASP.NET Core 1.x **Webanwendung** -Projektvorlage enthält einen Paket-Verweis für die [Microsoft.VisualStudio.Web.BrowserLink](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.BrowserLink/) Paket.</span><span class="sxs-lookup"><span data-stu-id="8d009-112">The ASP.NET Core 1.x **Web Application** project template has a package reference for the [Microsoft.VisualStudio.Web.BrowserLink](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.BrowserLink/) package.</span></span> <span data-ttu-id="8d009-113">Die **leere** oder **Web-API** -Vorlagenprojekten erfordern, dass Sie einen Verweis Paket hinzufügen `Microsoft.VisualStudio.Web.BrowserLink`.</span><span class="sxs-lookup"><span data-stu-id="8d009-113">The **Empty** or **Web API** template projects require you to add a package reference to `Microsoft.VisualStudio.Web.BrowserLink`.</span></span>

<span data-ttu-id="8d009-114">Da dies eine Visual Studio-Funktion, die einfachste Möglichkeit darin besteht, das Paket zum Hinzufügen einer **leere** oder **Web-API** Vorlagenprojekt besteht darin, öffnen die **Package Manager Console** (**Ansicht** > **Weitere Fenster** > **Package Manager Console**), und führen Sie den folgenden Befehl aus:</span><span class="sxs-lookup"><span data-stu-id="8d009-114">Since this is a Visual Studio feature, the easiest way to add the package to an **Empty** or **Web API** template project is to open the **Package Manager Console** (**View** > **Other Windows** > **Package Manager Console**) and run the following command:</span></span>

```console
install-package Microsoft.VisualStudio.Web.BrowserLink
```

<span data-ttu-id="8d009-115">Alternativ können Sie **NuGet Package Manager**.</span><span class="sxs-lookup"><span data-stu-id="8d009-115">Alternatively, you can use **NuGet Package Manager**.</span></span> <span data-ttu-id="8d009-116">Mit der rechten Maustaste in des Namens des Projekts **Projektmappen-Explorer** , und wählen Sie **NuGet-Pakete verwalten**:</span><span class="sxs-lookup"><span data-stu-id="8d009-116">Right-click the project name in **Solution Explorer** and choose **Manage NuGet Packages**:</span></span>

![Öffnen von NuGet-Paket-Manager](using-browserlink/_static/open-nuget-package-manager.png)

<span data-ttu-id="8d009-118">Suchen Sie und installieren Sie das Paket:</span><span class="sxs-lookup"><span data-stu-id="8d009-118">Find and install the package:</span></span>

![Paket mit NuGet-Paket-Manager hinzufügen](using-browserlink/_static/add-package-with-nuget-package-manager.png)

---

### <a name="configuration"></a><span data-ttu-id="8d009-120">Konfiguration</span><span class="sxs-lookup"><span data-stu-id="8d009-120">Configuration</span></span>

<span data-ttu-id="8d009-121">In der `Configure` Methode der *Startup.cs* Datei:</span><span class="sxs-lookup"><span data-stu-id="8d009-121">In the `Configure` method of the *Startup.cs* file:</span></span>

```csharp
app.UseBrowserLink();
```

<span data-ttu-id="8d009-122">Der Code in der Regel befindet sich innerhalb einer `if` Block, der nur die Browser-Link in der Entwicklungsumgebung ermöglicht, wie hier gezeigt:</span><span class="sxs-lookup"><span data-stu-id="8d009-122">Usually the code is inside an `if` block that only enables Browser Link in the Development environment, as shown here:</span></span>

```csharp
if (env.IsDevelopment())
{
    app.UseDeveloperExceptionPage();
    app.UseBrowserLink();
}
```

<span data-ttu-id="8d009-123">Weitere Informationen finden Sie unter [Working with Multiple Environments (Verwenden von mehreren Umgebungen)](xref:fundamentals/environments).</span><span class="sxs-lookup"><span data-stu-id="8d009-123">For more information, see [Working with Multiple Environments](xref:fundamentals/environments).</span></span>

## <a name="how-to-use-browser-link"></a><span data-ttu-id="8d009-124">Gewusst wie: Verwenden von Browserlink</span><span class="sxs-lookup"><span data-stu-id="8d009-124">How to use Browser Link</span></span>

<span data-ttu-id="8d009-125">Wenn Sie eine ASP.NET Core-Projekt geöffnet haben, zeigt Visual Studio das Symbolleistensteuerelement Browserlink neben der **Debugziel** Toolbar-Steuerelement:</span><span class="sxs-lookup"><span data-stu-id="8d009-125">When you have an ASP.NET Core project open, Visual Studio shows the Browser Link toolbar control next to the **Debug Target** toolbar control:</span></span>

![Browser Link Dropdown-Menü](using-browserlink/_static/browserLink-dropdown-menu.png)

<span data-ttu-id="8d009-127">Aus dem Browserlink-Toolbar-Steuerelement können Sie folgende Aktionen ausführen:</span><span class="sxs-lookup"><span data-stu-id="8d009-127">From the Browser Link toolbar control, you can:</span></span>

* <span data-ttu-id="8d009-128">Die Webanwendung in verschiedenen Browsern gleichzeitig zu aktualisieren.</span><span class="sxs-lookup"><span data-stu-id="8d009-128">Refresh the web application in several browsers at once.</span></span>
* <span data-ttu-id="8d009-129">Öffnen der **Browserlink-Dashboard**.</span><span class="sxs-lookup"><span data-stu-id="8d009-129">Open the **Browser Link Dashboard**.</span></span>
* <span data-ttu-id="8d009-130">Aktivieren oder deaktivieren Sie **Browserlink**.</span><span class="sxs-lookup"><span data-stu-id="8d009-130">Enable or disable **Browser Link**.</span></span> <span data-ttu-id="8d009-131">Hinweis: Browserlink ist in Visual Studio 2017 (15.3) standardmäßig deaktiviert.</span><span class="sxs-lookup"><span data-stu-id="8d009-131">Note: Browser Link is disabled by default in Visual Studio 2017 (15.3).</span></span>
* <span data-ttu-id="8d009-132">Aktivieren oder deaktivieren Sie [automatische CSS-Synchronisierung](#enable-or-disable-css-auto-sync).</span><span class="sxs-lookup"><span data-stu-id="8d009-132">Enable or disable [CSS Auto-Sync](#enable-or-disable-css-auto-sync).</span></span>

> [!NOTE]
> <span data-ttu-id="8d009-133">Einige Visual Studio-Plug-ins, vor allem Kontrollvorgänge *Web Erweiterung Pack 2015* und *Web Erweiterung Pack 2017*, bieten erweiterte Funktionalität für Browserlink, aber einige zusätzlichen Funktionen, die mit ASP funktionieren nicht. NET Core-Projekte.</span><span class="sxs-lookup"><span data-stu-id="8d009-133">Some Visual Studio plug-ins, most notably *Web Extension Pack 2015* and *Web Extension Pack 2017*, offer extended functionality for Browser Link, but some of the additional features don't work with ASP.NET Core projects.</span></span>

## <a name="refresh-the-web-application-in-several-browsers-at-once"></a><span data-ttu-id="8d009-134">Aktualisieren Sie die Webanwendung gleichzeitig in mehreren Browsern</span><span class="sxs-lookup"><span data-stu-id="8d009-134">Refresh the web application in several browsers at once</span></span>

<span data-ttu-id="8d009-135">Verwenden Sie zum Auswählen eines einzelnen Web-Browsers zum Starten, wenn das Projekt starten das Dropdown-Menü in der **Debugziel** Toolbar-Steuerelement:</span><span class="sxs-lookup"><span data-stu-id="8d009-135">To choose a single web browser to launch when starting the project, use the drop-down menu in the **Debug Target** toolbar control:</span></span>

![F5-Dropdownmenü](using-browserlink/_static/debug-target-dropdown-menu.png)

<span data-ttu-id="8d009-137">Zum Öffnen von mehreren Browsern gleichzeitig wählen **durchsuchen mit...**  aus der gleichen Dropdownliste aus.</span><span class="sxs-lookup"><span data-stu-id="8d009-137">To open multiple browsers at once, choose **Browse with...** from the same drop-down.</span></span> <span data-ttu-id="8d009-138">Halten Sie die STRG-Taste auf die Browsern auswählen, werden soll, und klicken Sie dann auf **Durchsuchen**:</span><span class="sxs-lookup"><span data-stu-id="8d009-138">Hold down the CTRL key to select the browsers you want, and then click **Browse**:</span></span>

![Viele Browsern gleichzeitig öffnen](using-browserlink/_static/open-many-browsers-at-once.png)

<span data-ttu-id="8d009-140">Hier ist ein Screenshot der Visual Studio mit die Indexansicht öffnen und zwei geöffneten Browser:</span><span class="sxs-lookup"><span data-stu-id="8d009-140">Here's a screenshot showing Visual Studio with the Index view open and two open browsers:</span></span>

![Synchronisieren Sie mit zwei Browser-Beispiel](using-browserlink/_static/sync-with-two-browsers-example.png)

<span data-ttu-id="8d009-142">Zeigen Sie auf dem Browserlink-Symbolleisten-Steuerelement auf den Browsern, die dem Projekt verbunden sind, finden Sie unter:</span><span class="sxs-lookup"><span data-stu-id="8d009-142">Hover over the Browser Link toolbar control to see the browsers that are connected to the project:</span></span>

![Zeigen Sie Tipp](using-browserlink/_static/hoover-tip.png)

<span data-ttu-id="8d009-144">Ändern die Indexansicht und allen verbundenen Browsern werden aktualisiert, wenn Sie auf die Aktualisierungsschaltfläche klicken Browserlink:</span><span class="sxs-lookup"><span data-stu-id="8d009-144">Change the Index view, and all connected browsers are updated when you click the Browser Link refresh button:</span></span>

![Browser-Synchronisierung auf Änderungen](using-browserlink/_static/browsers-sync-to-changes.png)

<span data-ttu-id="8d009-146">Browserlink funktioniert auch mit Browsern, die Sie außerhalb von Visual Studio starten, und navigieren Sie zu der URL der Anwendung.</span><span class="sxs-lookup"><span data-stu-id="8d009-146">Browser Link also works with browsers that you launch from outside Visual Studio and navigate to the application URL.</span></span>

### <a name="the-browser-link-dashboard"></a><span data-ttu-id="8d009-147">Die Browserlink-Dashboard</span><span class="sxs-lookup"><span data-stu-id="8d009-147">The Browser Link Dashboard</span></span>

<span data-ttu-id="8d009-148">Öffnen Sie die Browserlink-Dashboard aus dem Browserlink-Dropdown-Menü zum Verwalten einer Verbindung mit geöffneten Browser:</span><span class="sxs-lookup"><span data-stu-id="8d009-148">Open the Browser Link Dashboard from the Browser Link drop down menu to manage the connection with open browsers:</span></span>

![open-browserslink-dashboard](using-browserlink/_static/open-browserlink-dashboard.png)

<span data-ttu-id="8d009-150">Wenn keine Browser verbunden ist, können Sie eine nicht-Debugsitzung starten, durch Auswählen der *in Browser anzeigen* Link:</span><span class="sxs-lookup"><span data-stu-id="8d009-150">If no browser is connected, you can start a non-debugging session by selecting the *View in Browser* link:</span></span>

![browserlink-dashboard-no-connections](using-browserlink/_static/browserlink-dashboard-no-connections.png)

<span data-ttu-id="8d009-152">Andernfalls werden die verbundenen Browser durch den Pfad zu der Seite angezeigt, die jedem Browser angezeigt wird:</span><span class="sxs-lookup"><span data-stu-id="8d009-152">Otherwise, the connected browsers are shown with the path to the page that each browser is showing:</span></span>

![browserlink-dashboard-two-connections](using-browserlink/_static/browserlink-dashboard-two-connections.png)

<span data-ttu-id="8d009-154">Wenn Sie möchten, können Sie auf einen aufgelisteten Browsernamen dieser einzelnen Browsersitzung aktualisieren klicken.</span><span class="sxs-lookup"><span data-stu-id="8d009-154">If you like, you can click on a listed browser name to refresh that single browser.</span></span>

### <a name="enable-or-disable-browser-link"></a><span data-ttu-id="8d009-155">Aktivieren oder Deaktivieren von Browserlink</span><span class="sxs-lookup"><span data-stu-id="8d009-155">Enable or disable Browser Link</span></span>

<span data-ttu-id="8d009-156">Wenn Sie Browserlink erneut aktivieren nach dem deaktivieren, müssen Sie die Browser, um sie erneut eine Verbindung herstellen aktualisieren.</span><span class="sxs-lookup"><span data-stu-id="8d009-156">When you re-enable Browser Link after disabling it, you must refresh the browsers to reconnect them.</span></span>

### <a name="enable-or-disable-css-auto-sync"></a><span data-ttu-id="8d009-157">Aktivieren Sie oder deaktivieren Sie die automatische CSS-Synchronisierung</span><span class="sxs-lookup"><span data-stu-id="8d009-157">Enable or disable CSS Auto-Sync</span></span>

<span data-ttu-id="8d009-158">Wenn die automatische CSS-Synchronisierung aktiviert ist, werden die verbundenen Browsern automatisch aktualisiert, wenn Sie für jede Änderung an der CSS-Dateien vornehmen.</span><span class="sxs-lookup"><span data-stu-id="8d009-158">When CSS Auto-Sync is enabled, connected browsers are automatically refreshed when you make any change to CSS files.</span></span>

## <a name="how-does-it-work"></a><span data-ttu-id="8d009-159">Wie funktioniert das?</span><span class="sxs-lookup"><span data-stu-id="8d009-159">How does it work?</span></span>

<span data-ttu-id="8d009-160">Browserlink verwendet SignalR, um einen Kommunikationskanal zwischen Visual Studio und den Browser zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="8d009-160">Browser Link uses SignalR to create a communication channel between Visual Studio and the browser.</span></span> <span data-ttu-id="8d009-161">Wenn Browserlink aktiviert ist, verhält sich Visual Studio als SignalR-Server, dem mehrere Clients (Browser), eine Verbindung herstellen können.</span><span class="sxs-lookup"><span data-stu-id="8d009-161">When Browser Link is enabled, Visual Studio acts as a SignalR server that multiple clients (browsers) can connect to.</span></span> <span data-ttu-id="8d009-162">Browserlink registriert auch eine middlewarekomponente in der ASP.NET-Pipeline für die Anforderung.</span><span class="sxs-lookup"><span data-stu-id="8d009-162">Browser Link also registers a middleware component in the ASP.NET request pipeline.</span></span> <span data-ttu-id="8d009-163">Diese Komponente fügt spezielle `<script>` Verweise in jede Seitenanforderung vom Server.</span><span class="sxs-lookup"><span data-stu-id="8d009-163">This component injects special `<script>` references into every page request from the server.</span></span> <span data-ttu-id="8d009-164">Sehen Sie dazu die Skriptverweise **Quelltext anzeigen** im Browser, und scrollen bis zum Ende der `<body>` tag-Inhalte:</span><span class="sxs-lookup"><span data-stu-id="8d009-164">You can see the script references by selecting **View source** in the browser and scrolling to the end of the `<body>` tag content:</span></span>

```javascript
    <!-- Visual Studio Browser Link -->
    <script type="application/json" id="__browserLink_initializationData">
        {"requestId":"a717d5a07c1741949a7cefd6fa2bad08","requestMappingFromServer":false}
    </script>
    <script type="text/javascript" src="http://localhost:54139/b6e36e429d034f578ebccd6a79bf19bf/browserLink" async="async"></script>
    <!-- End Browser Link -->
</body>
```

<span data-ttu-id="8d009-165">Die Quelldateien werden nicht geändert.</span><span class="sxs-lookup"><span data-stu-id="8d009-165">Your source files aren't modified.</span></span> <span data-ttu-id="8d009-166">Die Komponente fügt die Skriptverweise dynamisch.</span><span class="sxs-lookup"><span data-stu-id="8d009-166">The middleware component injects the script references dynamically.</span></span> 

<span data-ttu-id="8d009-167">Da der Browser clientseitigen Code alle JavaScript ist, funktioniert es in allen Browsern, die SignalR unterstützt werden, ohne dass einen Browser-Plug-in.</span><span class="sxs-lookup"><span data-stu-id="8d009-167">Because the browser-side code is all JavaScript, it works on all browsers that SignalR supports without requiring a browser plug-in.</span></span>
