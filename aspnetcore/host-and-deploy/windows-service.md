---
title: Hosten von ASP.NET Core in einem Windowsdienst
author: rick-anderson
description: Erfahren Sie, wie eine ASP.NET Core-app in einem Windows-Dienst zu hosten.
manager: wpickett
ms.author: tdykstra
ms.custom: mvc
ms.date: 01/30/2018
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/windows-service
ms.openlocfilehash: 29f83ee585c73aeb57a09f70ea8e28650c05ce69
ms.sourcegitcommit: a19261eb82b948af6e4a1664fcfb8dabb16150e3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/14/2018
---
# <a name="host-aspnet-core-in-a-windows-service"></a><span data-ttu-id="5e27f-103">Hosten von ASP.NET Core in einem Windowsdienst</span><span class="sxs-lookup"><span data-stu-id="5e27f-103">Host ASP.NET Core in a Windows Service</span></span>

<span data-ttu-id="5e27f-104">Von [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="5e27f-104">By [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="5e27f-105">Die empfohlene Methode, um eine ASP.NET Core-app unter Windows zu hosten, ohne mit IIS ist die Ausführung in einem [Windowsdienst](/dotnet/framework/windows-services/introduction-to-windows-service-applications).</span><span class="sxs-lookup"><span data-stu-id="5e27f-105">The recommended way to host an ASP.NET Core app on Windows without using IIS is to run it in a [Windows Service](/dotnet/framework/windows-services/introduction-to-windows-service-applications).</span></span> <span data-ttu-id="5e27f-106">Wenn als Windows-Dienst gehostet wird, kann die app automatisch nach dem Start neu gestartet und ohne Benutzereingriff stürzt ab.</span><span class="sxs-lookup"><span data-stu-id="5e27f-106">When hosted as a Windows Service, the app can automatically start after reboots and crashes without requiring human intervention.</span></span>

<span data-ttu-id="5e27f-107">[Zeigen Sie Beispielcode an, oder laden Sie diesen herunter](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/windows-service/sample) ([Vorgehensweise zum Herunterladen](xref:tutorials/index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="5e27f-107">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/windows-service/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample)).</span></span> <span data-ttu-id="5e27f-108">Anweisungen zum Ausführen der Beispiel-app finden Sie im Beispiels *README.md* Datei.</span><span class="sxs-lookup"><span data-stu-id="5e27f-108">For instructions on how to run the sample app, see the sample's *README.md* file.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="5e27f-109">Erforderliche Komponenten</span><span class="sxs-lookup"><span data-stu-id="5e27f-109">Prerequisites</span></span>

* <span data-ttu-id="5e27f-110">In der .NET Framework-Laufzeit muss die app auszuführen.</span><span class="sxs-lookup"><span data-stu-id="5e27f-110">The app must run on the .NET Framework runtime.</span></span> <span data-ttu-id="5e27f-111">In der *csproj* Datei, geben Sie entsprechende Werte für [TargetFramework](/nuget/schema/target-frameworks) und [RuntimeIdentifier](/dotnet/articles/core/rid-catalog).</span><span class="sxs-lookup"><span data-stu-id="5e27f-111">In the *.csproj* file, specify appropriate values for [TargetFramework](/nuget/schema/target-frameworks) and [RuntimeIdentifier](/dotnet/articles/core/rid-catalog).</span></span> <span data-ttu-id="5e27f-112">Im Folgenden ein Beispiel:</span><span class="sxs-lookup"><span data-stu-id="5e27f-112">Here's an example:</span></span>

  [!code-xml[](windows-service/sample/AspNetCoreService.csproj?range=3-6)]

  <span data-ttu-id="5e27f-113">Verwenden Sie beim Erstellen eines Projekts in Visual Studio die **Core ASP.NET-Anwendung ((.NET Framework)** Vorlage.</span><span class="sxs-lookup"><span data-stu-id="5e27f-113">When creating a project in Visual Studio, use the **ASP.NET Core Application (.NET Framework)** template.</span></span>

* <span data-ttu-id="5e27f-114">Wenn die app aus dem Internet (nicht nur über ein internes Netzwerk) Anforderungen empfängt, verwenden sie die [HTTP.sys](xref:fundamentals/servers/httpsys) Webserver (früher bekannt als [WebListener](xref:fundamentals/servers/weblistener) für ASP.NET Core 1.x-apps) anstatt [Kestrel](xref:fundamentals/servers/kestrel).</span><span class="sxs-lookup"><span data-stu-id="5e27f-114">If the app receives requests from the Internet (not just from an internal network), it must use the [HTTP.sys](xref:fundamentals/servers/httpsys) web server (formerly known as [WebListener](xref:fundamentals/servers/weblistener) for ASP.NET Core 1.x apps) rather than [Kestrel](xref:fundamentals/servers/kestrel).</span></span> <span data-ttu-id="5e27f-115">IIS ist für die Verwendung als reverse-Proxy-Server mit Kestrel für Edge-Bereitstellungen empfohlen.</span><span class="sxs-lookup"><span data-stu-id="5e27f-115">IIS is recommended for use as a reverse proxy server with Kestrel for edge deployments.</span></span> <span data-ttu-id="5e27f-116">Weitere Informationen finden Sie unter [When to use Kestrel with a reverse proxy (Verwenden von Kestrel mit einem Reverseproxy)](xref:fundamentals/servers/kestrel#when-to-use-kestrel-with-a-reverse-proxy).</span><span class="sxs-lookup"><span data-stu-id="5e27f-116">For more information, see [When to use Kestrel with a reverse proxy](xref:fundamentals/servers/kestrel#when-to-use-kestrel-with-a-reverse-proxy).</span></span>

## <a name="get-started"></a><span data-ttu-id="5e27f-117">Erste Schritte</span><span class="sxs-lookup"><span data-stu-id="5e27f-117">Get started</span></span>

<span data-ttu-id="5e27f-118">In diesem Abschnitt wird erläutert, die minimale Änderungen erforderlich, um ein vorhandenes Projekt für ASP.NET Core einrichten, um in einem Dienst ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="5e27f-118">This section explains the minimum changes required to set up an existing ASP.NET Core project to run in a service.</span></span>

1. <span data-ttu-id="5e27f-119">Installieren Sie das NuGet-Paket [Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices/).</span><span class="sxs-lookup"><span data-stu-id="5e27f-119">Install the NuGet package [Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices/).</span></span>

2. <span data-ttu-id="5e27f-120">Nehmen Sie die folgenden Änderungen in `Program.Main`:</span><span class="sxs-lookup"><span data-stu-id="5e27f-120">Make the following changes in `Program.Main`:</span></span>

   * <span data-ttu-id="5e27f-121">Rufen Sie `host.RunAsService` anstelle von `host.Run`.</span><span class="sxs-lookup"><span data-stu-id="5e27f-121">Call `host.RunAsService` instead of `host.Run`.</span></span>

   * <span data-ttu-id="5e27f-122">Wenn der Code ruft `UseContentRoot`, verwenden Sie einen Pfad an den Veröffentlichungsort anstelle von `Directory.GetCurrentDirectory()`.</span><span class="sxs-lookup"><span data-stu-id="5e27f-122">If the code calls `UseContentRoot`, use a path to the publish location instead of `Directory.GetCurrentDirectory()`.</span></span>

   # <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="5e27f-123">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="5e27f-123">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

   [!code-csharp[](windows-service/sample/Program.cs?name=ServiceOnly&highlight=3-4,7,12)]

   # <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="5e27f-124">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="5e27f-124">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

   [!code-csharp[](windows-service/sample_snapshot/Program.cs?name=ServiceOnly&highlight=3-4,8,14)]

   ---

3. <span data-ttu-id="5e27f-125">Veröffentlichen Sie die app in einem Ordner.</span><span class="sxs-lookup"><span data-stu-id="5e27f-125">Publish the app to a folder.</span></span> <span data-ttu-id="5e27f-126">Verwendung [Dotnet veröffentlichen](/dotnet/articles/core/tools/dotnet-publish) oder ein [Visual Studio das Veröffentlichungsprofil](xref:host-and-deploy/visual-studio-publish-profiles) , die in einen Ordner veröffentlicht.</span><span class="sxs-lookup"><span data-stu-id="5e27f-126">Use [dotnet publish](/dotnet/articles/core/tools/dotnet-publish) or a [Visual Studio publish profile](xref:host-and-deploy/visual-studio-publish-profiles) that publishes to a folder.</span></span>

4. <span data-ttu-id="5e27f-127">Testen von erstellen und den Dienst zu starten.</span><span class="sxs-lookup"><span data-stu-id="5e27f-127">Test by creating and starting the service.</span></span>

   <span data-ttu-id="5e27f-128">Öffnen Sie eine Befehlsshell mit Administratorrechten verwendet die [sc.exe](https://technet.microsoft.com/library/bb490995) Befehlszeilentool zum Erstellen und Starten eines Diensts.</span><span class="sxs-lookup"><span data-stu-id="5e27f-128">Open a command shell with administrative privileges to use the [sc.exe](https://technet.microsoft.com/library/bb490995) command-line tool to create and start a service.</span></span> <span data-ttu-id="5e27f-129">Wenn der Dienst "MyService" benannt ist, veröffentlicht `c:\svc`, und mit dem Namen AspNetCoreService, die Befehle sind:</span><span class="sxs-lookup"><span data-stu-id="5e27f-129">If the service is named MyService, published to `c:\svc`, and named AspNetCoreService, the commands are:</span></span>

   ```console
   sc create MyService binPath="c:\svc\aspnetcoreservice.exe"
   sc start MyService
   ```

   <span data-ttu-id="5e27f-130">Die `binPath` Wert ist der Pfad zur ausführbaren Datei die app, die den Namen der ausführbaren Datei enthält.</span><span class="sxs-lookup"><span data-stu-id="5e27f-130">The `binPath` value is the path to the app's executable, which includes the executable file name.</span></span>

   ![Konsolenfenster erstellen und starten Beispiel](windows-service/_static/create-start.png)

   <span data-ttu-id="5e27f-132">Wenn diese Befehle abgeschlossen haben, navigieren Sie zu dem gleichen Pfad wie bei Ausführung als eine Konsolen-app (standardmäßig `http://localhost:5000`):</span><span class="sxs-lookup"><span data-stu-id="5e27f-132">When these commands finish, browse to the same path as when running as a console app (by default, `http://localhost:5000`):</span></span>

   ![In einem Dienst ausgeführt wird](windows-service/_static/running-in-service.png)

## <a name="provide-a-way-to-run-outside-of-a-service"></a><span data-ttu-id="5e27f-134">Bieten Sie eine Möglichkeit, die außerhalb eines Diensts ausführen</span><span class="sxs-lookup"><span data-stu-id="5e27f-134">Provide a way to run outside of a service</span></span>

<span data-ttu-id="5e27f-135">Es ist einfacher, testen und Debuggen außerhalb von einem Dienst ausgeführt wird, daher ist es üblich, So fügen Sie Code hinzu, die aufruft `RunAsService` nur unter bestimmten Bedingungen.</span><span class="sxs-lookup"><span data-stu-id="5e27f-135">It's easier to test and debug when running outside of a service, so it's customary to add code that calls `RunAsService` only under certain conditions.</span></span> <span data-ttu-id="5e27f-136">Beispielsweise kann die app auszuführen, als eine Konsolen-app mit einer `--console` Befehlszeilenargument oder wenn der Debugger angefügt ist:</span><span class="sxs-lookup"><span data-stu-id="5e27f-136">For example, the app can run as a console app with a `--console` command-line argument or if the debugger is attached:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="5e27f-137">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="5e27f-137">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

[!code-csharp[](windows-service/sample/Program.cs?name=ServiceOrConsole)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="5e27f-138">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="5e27f-138">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

[!code-csharp[](windows-service/sample_snapshot/Program.cs?name=ServiceOrConsole)]

---

## <a name="handle-stopping-and-starting-events"></a><span data-ttu-id="5e27f-139">Behandeln von Ereignissen starten und beenden</span><span class="sxs-lookup"><span data-stu-id="5e27f-139">Handle stopping and starting events</span></span>

<span data-ttu-id="5e27f-140">Behandeln `OnStarting`, `OnStarted`, und `OnStopping` Ereignisse, die folgenden zusätzliche Änderungen vornehmen:</span><span class="sxs-lookup"><span data-stu-id="5e27f-140">To handle `OnStarting`, `OnStarted`, and `OnStopping` events, make the following additional changes:</span></span>

1. <span data-ttu-id="5e27f-141">Erstellen Sie eine Klasse, die abgeleitet `WebHostService`:</span><span class="sxs-lookup"><span data-stu-id="5e27f-141">Create a class that derives from `WebHostService`:</span></span>

   [!code-csharp[](windows-service/sample/CustomWebHostService.cs?name=NoLogging)]

2. <span data-ttu-id="5e27f-142">Erstellen Sie eine Erweiterungsmethode für `IWebHost` , die die benutzerdefinierte übergibt `WebHostService` auf `ServiceBase.Run`:</span><span class="sxs-lookup"><span data-stu-id="5e27f-142">Create an extension method for `IWebHost` that passes the custom `WebHostService` to `ServiceBase.Run`:</span></span>

   [!code-csharp[](windows-service/sample/WebHostServiceExtensions.cs?name=ExtensionsClass)]

3. <span data-ttu-id="5e27f-143">In `Program.Main`, rufen Sie die neue Erweiterungsmethode `RunAsCustomService`, anstelle von `RunAsService`:</span><span class="sxs-lookup"><span data-stu-id="5e27f-143">In `Program.Main`, call the new extension method, `RunAsCustomService`, instead of `RunAsService`:</span></span>

   # <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="5e27f-144">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="5e27f-144">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

   [!code-csharp[](windows-service/sample/Program.cs?name=HandleStopStart&highlight=24)]

   # <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="5e27f-145">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="5e27f-145">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

   [!code-csharp[](windows-service/sample_snapshot/Program.cs?name=HandleStopStart&highlight=26)]

   ---

<span data-ttu-id="5e27f-146">Wenn die benutzerdefinierte `WebHostService` Code erfordert einen Dienst aus abhängigkeiteneinschleusung (z. B. eine Protokollierung), erhalten sie über die `Services` Eigenschaft `IWebHost`:</span><span class="sxs-lookup"><span data-stu-id="5e27f-146">If the custom `WebHostService` code requires a service from dependency injection (such as a logger), obtain it from the `Services` property of `IWebHost`:</span></span>

[!code-csharp[](windows-service/sample/CustomWebHostService.cs?name=Logging&highlight=7)]

## <a name="proxy-server-and-load-balancer-scenarios"></a><span data-ttu-id="5e27f-147">Proxyserver und Lastenausgleichsszenarien</span><span class="sxs-lookup"><span data-stu-id="5e27f-147">Proxy server and load balancer scenarios</span></span>

<span data-ttu-id="5e27f-148">Dienste, die Anforderungen aus dem Internet oder einem Unternehmensnetzwerk interagieren und werden hinter einem Proxy oder den load Balancer möglicherweise zusätzliche Konfiguration erforderlich ist.</span><span class="sxs-lookup"><span data-stu-id="5e27f-148">Services that interact with requests from the Internet or a corporate network and are behind a proxy or load balancer might require additional configuration.</span></span> <span data-ttu-id="5e27f-149">Weitere Informationen hierzu feinden Sie unter [Konfigurieren von ASP.NET Core zur Verwendung mit Proxyservern und Lastenausgleich](xref:host-and-deploy/proxy-load-balancer).</span><span class="sxs-lookup"><span data-stu-id="5e27f-149">For more information, see [Configure ASP.NET Core to work with proxy servers and load balancers](xref:host-and-deploy/proxy-load-balancer).</span></span>

## <a name="acknowledgments"></a><span data-ttu-id="5e27f-150">Danksagungen</span><span class="sxs-lookup"><span data-stu-id="5e27f-150">Acknowledgments</span></span>

<span data-ttu-id="5e27f-151">In diesem Artikel wurde mithilfe von veröffentlichten Datenquellen geschrieben:</span><span class="sxs-lookup"><span data-stu-id="5e27f-151">This article was written with the help of published sources:</span></span>

* [<span data-ttu-id="5e27f-152">Hosten von ASP.NET Core als Windows-Dienst</span><span class="sxs-lookup"><span data-stu-id="5e27f-152">Hosting ASP.NET Core as Windows service</span></span>](https://stackoverflow.com/questions/37346383/hosting-asp-net-core-as-windows-service/37464074)
* [<span data-ttu-id="5e27f-153">Wie Ihre ASP.NET Core in einem Windows-Dienst gehostet wird.</span><span class="sxs-lookup"><span data-stu-id="5e27f-153">How to host your ASP.NET Core in a Windows Service</span></span>](https://dotnetthoughts.net/how-to-host-your-aspnet-core-in-a-windows-service/)
