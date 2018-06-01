---
title: Hosten von ASP.NET Core in einem Windows-Dienst
author: rick-anderson
description: Erfahren Sie, wie eine ASP.NET Core-App in einem Windows-Dienst gehostet wird.
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
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/14/2018
ms.locfileid: "34153528"
---
# <a name="host-aspnet-core-in-a-windows-service"></a><span data-ttu-id="b81fe-103">Hosten von ASP.NET Core in einem Windows-Dienst</span><span class="sxs-lookup"><span data-stu-id="b81fe-103">Host ASP.NET Core in a Windows Service</span></span>

<span data-ttu-id="b81fe-104">Von [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="b81fe-104">By [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="b81fe-105">Wenn Sie eine ASP.NET Core-App unter Windows ohne Verwendung von IIS hosten möchten, wird empfohlen, diese in einem [Windows-Dienst](/dotnet/framework/windows-services/introduction-to-windows-service-applications) auszuführen.</span><span class="sxs-lookup"><span data-stu-id="b81fe-105">The recommended way to host an ASP.NET Core app on Windows without using IIS is to run it in a [Windows Service](/dotnet/framework/windows-services/introduction-to-windows-service-applications).</span></span> <span data-ttu-id="b81fe-106">Wird die App als Windows-Dienst gehostet, kann sie nach Neustarts und Abstürzen automatisch gestartet werden, ohne dass manuelles Eingreifen erforderlich wird.</span><span class="sxs-lookup"><span data-stu-id="b81fe-106">When hosted as a Windows Service, the app can automatically start after reboots and crashes without requiring human intervention.</span></span>

<span data-ttu-id="b81fe-107">[Zeigen Sie Beispielcode an, oder laden Sie diesen herunter](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/windows-service/sample) ([Vorgehensweise zum Herunterladen](xref:tutorials/index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="b81fe-107">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/windows-service/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample)).</span></span> <span data-ttu-id="b81fe-108">Anweisungen zum Ausführen der Beispiel-App finden Sie in der Datei *README.md* des Beispiels.</span><span class="sxs-lookup"><span data-stu-id="b81fe-108">For instructions on how to run the sample app, see the sample's *README.md* file.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="b81fe-109">Erforderliche Komponenten</span><span class="sxs-lookup"><span data-stu-id="b81fe-109">Prerequisites</span></span>

* <span data-ttu-id="b81fe-110">Die App muss in der .NET Framework-Runtime ausgeführt werden.</span><span class="sxs-lookup"><span data-stu-id="b81fe-110">The app must run on the .NET Framework runtime.</span></span> <span data-ttu-id="b81fe-111">Geben Sie in der *CSPROJ*-Datei entsprechende Werte für [TargetFramework](/nuget/schema/target-frameworks) und [RuntimeIdentifier](/dotnet/articles/core/rid-catalog) an.</span><span class="sxs-lookup"><span data-stu-id="b81fe-111">In the *.csproj* file, specify appropriate values for [TargetFramework](/nuget/schema/target-frameworks) and [RuntimeIdentifier](/dotnet/articles/core/rid-catalog).</span></span> <span data-ttu-id="b81fe-112">Im Folgenden ein Beispiel:</span><span class="sxs-lookup"><span data-stu-id="b81fe-112">Here's an example:</span></span>

  [!code-xml[](windows-service/sample/AspNetCoreService.csproj?range=3-6)]

  <span data-ttu-id="b81fe-113">Verwenden Sie bei der Erstellung eines Projekts in Visual Studio die Vorlage **ASP.NET Core-Anwendung (.NET Framework)**.</span><span class="sxs-lookup"><span data-stu-id="b81fe-113">When creating a project in Visual Studio, use the **ASP.NET Core Application (.NET Framework)** template.</span></span>

* <span data-ttu-id="b81fe-114">Wenn die App Anforderungen über das Internet empfängt (nicht nur über ein internes Netzwerk), muss Sie den [HTTP.sys](xref:fundamentals/servers/httpsys)-Webserver (früher bekannt als [WebListener](xref:fundamentals/servers/weblistener) für ASP.NET Core 1.x-Apps) anstelle von [Kestrel](xref:fundamentals/servers/kestrel) verwenden.</span><span class="sxs-lookup"><span data-stu-id="b81fe-114">If the app receives requests from the Internet (not just from an internal network), it must use the [HTTP.sys](xref:fundamentals/servers/httpsys) web server (formerly known as [WebListener](xref:fundamentals/servers/weblistener) for ASP.NET Core 1.x apps) rather than [Kestrel](xref:fundamentals/servers/kestrel).</span></span> <span data-ttu-id="b81fe-115">Für die Verwendung als Reverseproxyserver mit Kestrel für Edge-Bereitstellungen wird IIS empfohlen.</span><span class="sxs-lookup"><span data-stu-id="b81fe-115">IIS is recommended for use as a reverse proxy server with Kestrel for edge deployments.</span></span> <span data-ttu-id="b81fe-116">Weitere Informationen finden Sie unter [When to use Kestrel with a reverse proxy (Verwenden von Kestrel mit einem Reverseproxy)](xref:fundamentals/servers/kestrel#when-to-use-kestrel-with-a-reverse-proxy).</span><span class="sxs-lookup"><span data-stu-id="b81fe-116">For more information, see [When to use Kestrel with a reverse proxy](xref:fundamentals/servers/kestrel#when-to-use-kestrel-with-a-reverse-proxy).</span></span>

## <a name="get-started"></a><span data-ttu-id="b81fe-117">Erste Schritte</span><span class="sxs-lookup"><span data-stu-id="b81fe-117">Get started</span></span>

<span data-ttu-id="b81fe-118">In diesem Abschnitt wird erläutert, welche minimalen Änderungen für die Einrichtung eines vorhandenen ASP.NET Core-Projekts zur Ausführung in einem Dienst erforderlich sind.</span><span class="sxs-lookup"><span data-stu-id="b81fe-118">This section explains the minimum changes required to set up an existing ASP.NET Core project to run in a service.</span></span>

1. <span data-ttu-id="b81fe-119">Installieren Sie das NuGet-Paket [Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices/).</span><span class="sxs-lookup"><span data-stu-id="b81fe-119">Install the NuGet package [Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices/).</span></span>

2. <span data-ttu-id="b81fe-120">Nehmen Sie in `Program.Main` die folgenden Änderungen vor:</span><span class="sxs-lookup"><span data-stu-id="b81fe-120">Make the following changes in `Program.Main`:</span></span>

   * <span data-ttu-id="b81fe-121">Rufen Sie `host.RunAsService` statt `host.Run` auf.</span><span class="sxs-lookup"><span data-stu-id="b81fe-121">Call `host.RunAsService` instead of `host.Run`.</span></span>

   * <span data-ttu-id="b81fe-122">Wenn der Code `UseContentRoot` aufruft, verwenden Sie anstelle von `Directory.GetCurrentDirectory()` einen Pfad zum Ort der Veröffentlichung.</span><span class="sxs-lookup"><span data-stu-id="b81fe-122">If the code calls `UseContentRoot`, use a path to the publish location instead of `Directory.GetCurrentDirectory()`.</span></span>

   # <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="b81fe-123">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="b81fe-123">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

   [!code-csharp[](windows-service/sample/Program.cs?name=ServiceOnly&highlight=3-4,7,12)]

   # <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="b81fe-124">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="b81fe-124">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

   [!code-csharp[](windows-service/sample_snapshot/Program.cs?name=ServiceOnly&highlight=3-4,8,14)]

   ---

3. <span data-ttu-id="b81fe-125">Veröffentlichen Sie die App in einem Ordner.</span><span class="sxs-lookup"><span data-stu-id="b81fe-125">Publish the app to a folder.</span></span> <span data-ttu-id="b81fe-126">Verwenden Sie [dotnet publish](/dotnet/articles/core/tools/dotnet-publish) oder ein [Visual Studio-Veröffentlichungsprofil](xref:host-and-deploy/visual-studio-publish-profiles) für die Veröffentlichung in einem Ordner.</span><span class="sxs-lookup"><span data-stu-id="b81fe-126">Use [dotnet publish](/dotnet/articles/core/tools/dotnet-publish) or a [Visual Studio publish profile](xref:host-and-deploy/visual-studio-publish-profiles) that publishes to a folder.</span></span>

4. <span data-ttu-id="b81fe-127">Führen Sie einen Test durch, indem Sie den Dienst erstellen und starten.</span><span class="sxs-lookup"><span data-stu-id="b81fe-127">Test by creating and starting the service.</span></span>

   <span data-ttu-id="b81fe-128">Öffnen Sie eine Befehlsshell mit Administratorrechten, um das Befehlszeilentool [sc.exe](https://technet.microsoft.com/library/bb490995) zum Erstellen und Starten eines Diensts zu verwenden.</span><span class="sxs-lookup"><span data-stu-id="b81fe-128">Open a command shell with administrative privileges to use the [sc.exe](https://technet.microsoft.com/library/bb490995) command-line tool to create and start a service.</span></span> <span data-ttu-id="b81fe-129">Wenn der Dienst den Namen „MyService“ trägt, in `c:\svc` veröffentlicht wird und den Namen „AspNetCoreService“ erhält, lauten die Befehle wie folgt:</span><span class="sxs-lookup"><span data-stu-id="b81fe-129">If the service is named MyService, published to `c:\svc`, and named AspNetCoreService, the commands are:</span></span>

   ```console
   sc create MyService binPath="c:\svc\aspnetcoreservice.exe"
   sc start MyService
   ```

   <span data-ttu-id="b81fe-130">Der Wert `binPath` ist der Pfad zu der ausführbaren Datei der App, der den Namen der ausführbaren Datei enthält.</span><span class="sxs-lookup"><span data-stu-id="b81fe-130">The `binPath` value is the path to the app's executable, which includes the executable file name.</span></span>

   ![Beispiel zum Erstellen und Starten im Konsolenfenster](windows-service/_static/create-start.png)

   <span data-ttu-id="b81fe-132">Navigieren Sie nach Abschluss dieser Befehle zu dem gleichen Pfad wie bei der Ausführung als Konsolen-App (standardmäßig `http://localhost:5000`):</span><span class="sxs-lookup"><span data-stu-id="b81fe-132">When these commands finish, browse to the same path as when running as a console app (by default, `http://localhost:5000`):</span></span>

   ![Ausführung in einem Dienst](windows-service/_static/running-in-service.png)

## <a name="provide-a-way-to-run-outside-of-a-service"></a><span data-ttu-id="b81fe-134">Bieten einer Möglichkeit zur Ausführung außerhalb eines Diensts</span><span class="sxs-lookup"><span data-stu-id="b81fe-134">Provide a way to run outside of a service</span></span>

<span data-ttu-id="b81fe-135">Das Testen und Debuggen ist bei der Ausführung außerhalb eines Diensts einfacher. Daher ist es üblich, dass Code hinzugefügt wird, der `RunAsService` nur unter bestimmten Bedingungen aufruft.</span><span class="sxs-lookup"><span data-stu-id="b81fe-135">It's easier to test and debug when running outside of a service, so it's customary to add code that calls `RunAsService` only under certain conditions.</span></span> <span data-ttu-id="b81fe-136">Beispielsweise kann die App als Konsolen-App mit dem Befehlszeilenargument `--console` ausgeführt werden oder wenn der Debugger angefügt wird:</span><span class="sxs-lookup"><span data-stu-id="b81fe-136">For example, the app can run as a console app with a `--console` command-line argument or if the debugger is attached:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="b81fe-137">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="b81fe-137">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

[!code-csharp[](windows-service/sample/Program.cs?name=ServiceOrConsole)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="b81fe-138">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="b81fe-138">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

[!code-csharp[](windows-service/sample_snapshot/Program.cs?name=ServiceOrConsole)]

---

## <a name="handle-stopping-and-starting-events"></a><span data-ttu-id="b81fe-139">Behandeln des Stoppens und Startens von Ereignissen</span><span class="sxs-lookup"><span data-stu-id="b81fe-139">Handle stopping and starting events</span></span>

<span data-ttu-id="b81fe-140">Nehmen Sie die folgenden zusätzlichen Änderungen vor, um Ereignisse vom Typ `OnStarting`, `OnStarted` und `OnStopping` zu verarbeiten:</span><span class="sxs-lookup"><span data-stu-id="b81fe-140">To handle `OnStarting`, `OnStarted`, and `OnStopping` events, make the following additional changes:</span></span>

1. <span data-ttu-id="b81fe-141">Erstellen Sie eine von `WebHostService` abgeleitete Klasse:</span><span class="sxs-lookup"><span data-stu-id="b81fe-141">Create a class that derives from `WebHostService`:</span></span>

   [!code-csharp[](windows-service/sample/CustomWebHostService.cs?name=NoLogging)]

2. <span data-ttu-id="b81fe-142">Erstellen Sie eine Erweiterungsmethode für `IWebHost`, die den benutzerdefinierten `WebHostService` an `ServiceBase.Run` übergibt:</span><span class="sxs-lookup"><span data-stu-id="b81fe-142">Create an extension method for `IWebHost` that passes the custom `WebHostService` to `ServiceBase.Run`:</span></span>

   [!code-csharp[](windows-service/sample/WebHostServiceExtensions.cs?name=ExtensionsClass)]

3. <span data-ttu-id="b81fe-143">Rufen Sie in `Program.Main` anstelle von `RunAsService` die neue Erweiterungsmethode `RunAsCustomService` auf:</span><span class="sxs-lookup"><span data-stu-id="b81fe-143">In `Program.Main`, call the new extension method, `RunAsCustomService`, instead of `RunAsService`:</span></span>

   # <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="b81fe-144">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="b81fe-144">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

   [!code-csharp[](windows-service/sample/Program.cs?name=HandleStopStart&highlight=24)]

   # <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="b81fe-145">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="b81fe-145">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

   [!code-csharp[](windows-service/sample_snapshot/Program.cs?name=HandleStopStart&highlight=26)]

   ---

<span data-ttu-id="b81fe-146">Wenn für den benutzerdefinierten `WebHostService`-Code ein Dienst aus der Abhängigkeitsinjektion erforderlich ist (z.B. eine Protokollierung), rufen Sie diese über die `Services`-Eigenschaft von `IWebHost` ab:</span><span class="sxs-lookup"><span data-stu-id="b81fe-146">If the custom `WebHostService` code requires a service from dependency injection (such as a logger), obtain it from the `Services` property of `IWebHost`:</span></span>

[!code-csharp[](windows-service/sample/CustomWebHostService.cs?name=Logging&highlight=7)]

## <a name="proxy-server-and-load-balancer-scenarios"></a><span data-ttu-id="b81fe-147">Proxyserver und Lastenausgleichsszenarien</span><span class="sxs-lookup"><span data-stu-id="b81fe-147">Proxy server and load balancer scenarios</span></span>

<span data-ttu-id="b81fe-148">Dienste, die mit Anforderungen aus dem Internet oder einem Unternehmensnetzwerk interagieren und hinter einem Proxy oder Lastenausgleich ausgeführt werden, erfordern möglicherweise zusätzliche Konfigurationen.</span><span class="sxs-lookup"><span data-stu-id="b81fe-148">Services that interact with requests from the Internet or a corporate network and are behind a proxy or load balancer might require additional configuration.</span></span> <span data-ttu-id="b81fe-149">Weitere Informationen hierzu feinden Sie unter [Konfigurieren von ASP.NET Core zur Verwendung mit Proxyservern und Lastenausgleich](xref:host-and-deploy/proxy-load-balancer).</span><span class="sxs-lookup"><span data-stu-id="b81fe-149">For more information, see [Configure ASP.NET Core to work with proxy servers and load balancers](xref:host-and-deploy/proxy-load-balancer).</span></span>

## <a name="acknowledgments"></a><span data-ttu-id="b81fe-150">Danksagungen</span><span class="sxs-lookup"><span data-stu-id="b81fe-150">Acknowledgments</span></span>

<span data-ttu-id="b81fe-151">Dieser Artikel wurde mithilfe von veröffentlichten Quellen geschrieben:</span><span class="sxs-lookup"><span data-stu-id="b81fe-151">This article was written with the help of published sources:</span></span>

* [<span data-ttu-id="b81fe-152">Hosten von ASP.NET Core als Windows-Dienst</span><span class="sxs-lookup"><span data-stu-id="b81fe-152">Hosting ASP.NET Core as Windows service</span></span>](https://stackoverflow.com/questions/37346383/hosting-asp-net-core-as-windows-service/37464074)
* [<span data-ttu-id="b81fe-153">Hosten Ihres ASP.NET Core in einem Windows-Dienst</span><span class="sxs-lookup"><span data-stu-id="b81fe-153">How to host your ASP.NET Core in a Windows Service</span></span>](https://dotnetthoughts.net/how-to-host-your-aspnet-core-in-a-windows-service/)
