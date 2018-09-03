---
title: Hosten von ASP.NET Core in einem Windows-Dienst
author: guardrex
description: Erfahren Sie, wie eine ASP.NET Core-App in einem Windows-Dienst gehostet wird.
ms.author: tdykstra
ms.custom: mvc
ms.date: 06/04/2018
uid: host-and-deploy/windows-service
ms.openlocfilehash: 68afe77b05a717cffecc32188f18e9fde208b81f
ms.sourcegitcommit: 3ca20ed63bf1469f4365f0c1fbd00c98a3191c84
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/17/2018
ms.locfileid: "41751591"
---
# <a name="host-aspnet-core-in-a-windows-service"></a><span data-ttu-id="596da-103">Hosten von ASP.NET Core in einem Windows-Dienst</span><span class="sxs-lookup"><span data-stu-id="596da-103">Host ASP.NET Core in a Windows Service</span></span>

<span data-ttu-id="596da-104">Von [Luke Latham](https://github.com/guardrex) und [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="596da-104">By [Luke Latham](https://github.com/guardrex) and [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="596da-105">Eine ASP.NET Core-App kann unter Windows gehostet werden, ohne IIS als [Windows-Dienst](/dotnet/framework/windows-services/introduction-to-windows-service-applications) zu verwenden.</span><span class="sxs-lookup"><span data-stu-id="596da-105">An ASP.NET Core app can be hosted on Windows without using IIS as a [Windows Service](/dotnet/framework/windows-services/introduction-to-windows-service-applications).</span></span> <span data-ttu-id="596da-106">Wird die App als Windows-Dienst gehostet, kann sie nach Neustarts und Abstürzen automatisch gestartet werden, ohne dass manuelles Eingreifen erforderlich wird.</span><span class="sxs-lookup"><span data-stu-id="596da-106">When hosted as a Windows Service, the app can automatically start after reboots and crashes without requiring human intervention.</span></span>

<span data-ttu-id="596da-107">[Anzeigen oder Herunterladen von Beispielcode](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/windows-service/samples) ([Vorgehensweise zum Herunterladen](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="596da-107">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/windows-service/samples) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="get-started"></a><span data-ttu-id="596da-108">Erste Schritte</span><span class="sxs-lookup"><span data-stu-id="596da-108">Get started</span></span>

<span data-ttu-id="596da-109">Die folgenden minimalen Änderungen sind für die Einrichtung eines vorhandenen ASP.NET Core-Projekts zur Ausführung in einem Dienst erforderlich:</span><span class="sxs-lookup"><span data-stu-id="596da-109">The following minimum changes are required to set up an existing ASP.NET Core project to run in a service:</span></span>

1. <span data-ttu-id="596da-110">In der Projektdatei:</span><span class="sxs-lookup"><span data-stu-id="596da-110">In the project file:</span></span>

   1. <span data-ttu-id="596da-111">Bestätigen Sie das Vorhandensein des Runtime-Bezeichners, oder fügen Sie diesen der **\<Eigenschaftengruppe>** hinzu, die das Zielframework enthält:</span><span class="sxs-lookup"><span data-stu-id="596da-111">Confirm the presence of the runtime identifier or add it to the **\<PropertyGroup>** that contains the target framework:</span></span>

      ::: moniker range=">= aspnetcore-2.1"

      ```xml
      <PropertyGroup>
        <TargetFramework>netcoreapp2.1</TargetFramework>
        <RuntimeIdentifier>win7-x64</RuntimeIdentifier>
      </PropertyGroup>
      ```

      ::: moniker-end

      ::: moniker range="= aspnetcore-2.0"

      ```xml
      <PropertyGroup>
        <TargetFramework>netcoreapp2.0</TargetFramework>
        <RuntimeIdentifier>win7-x64</RuntimeIdentifier>
      </PropertyGroup>
      ```

      ::: moniker-end

      ::: moniker range="< aspnetcore-2.0"

      ```xml
      <PropertyGroup>
        <TargetFramework>netcoreapp1.1</TargetFramework>
        <RuntimeIdentifier>win7-x64</RuntimeIdentifier>
      </PropertyGroup>
      ```

      ::: moniker-end

   1. <span data-ttu-id="596da-112">Fügen Sie einen Paketverweis auf [Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices/) hinzu.</span><span class="sxs-lookup"><span data-stu-id="596da-112">Add a package reference for [Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices/).</span></span>

1. <span data-ttu-id="596da-113">Nehmen Sie in `Program.Main` die folgenden Änderungen vor:</span><span class="sxs-lookup"><span data-stu-id="596da-113">Make the following changes in `Program.Main`:</span></span>

   * <span data-ttu-id="596da-114">Rufen Sie [host.RunAsService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostwindowsserviceextensions.runasservice) anstelle von `host.Run` auf.</span><span class="sxs-lookup"><span data-stu-id="596da-114">Call [host.RunAsService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostwindowsserviceextensions.runasservice) instead of `host.Run`.</span></span>

   * <span data-ttu-id="596da-115">Rufen Sie [UseContentRoot](xref:fundamentals/host/web-host#content-root) auf, und verwenden Sie anstelle von `Directory.GetCurrentDirectory()` einen Pfad zum veröffentlichten Speicherort der App.</span><span class="sxs-lookup"><span data-stu-id="596da-115">Call [UseContentRoot](xref:fundamentals/host/web-host#content-root) and use a path to the app's published location instead of `Directory.GetCurrentDirectory()`.</span></span>

     ::: moniker range=">= aspnetcore-2.0"

     [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/Program.cs?name=ServiceOnly&highlight=8-9,16)]

     ::: moniker-end

     ::: moniker range="< aspnetcore-2.0"

     [!code-csharp[](windows-service/samples_snapshot/1.x/AspNetCoreService/Program.cs?name=ServiceOnly&highlight=3-4,8,13)]

     ::: moniker-end

1. <span data-ttu-id="596da-116">Veröffentlichen Sie die App.</span><span class="sxs-lookup"><span data-stu-id="596da-116">Publish the app.</span></span> <span data-ttu-id="596da-117">Verwenden Sie [dotnet publish](/dotnet/articles/core/tools/dotnet-publish) oder ein [Visual Studio-Veröffentlichungsprofil](xref:host-and-deploy/visual-studio-publish-profiles).</span><span class="sxs-lookup"><span data-stu-id="596da-117">Use [dotnet publish](/dotnet/articles/core/tools/dotnet-publish) or a [Visual Studio publish profile](xref:host-and-deploy/visual-studio-publish-profiles).</span></span> <span data-ttu-id="596da-118">Wählen Sie **FolderProfile** aus, wenn Sie Visual Studio verwenden.</span><span class="sxs-lookup"><span data-stu-id="596da-118">When using a Visual Studio, select the **FolderProfile**.</span></span>

   <span data-ttu-id="596da-119">Führen Sie zum Veröffentlichen der Beispiel-App über die Befehlszeile den folgenden Befehl in einem Konsolenfenster des Projektordners aus:</span><span class="sxs-lookup"><span data-stu-id="596da-119">To publish the sample app from the command line, run the following command in a console window from the project folder:</span></span>

   ```console
   dotnet publish --configuration Release
   ```

1. <span data-ttu-id="596da-120">Verwenden Sie das Befehlszeilentool [sc.exe](https://technet.microsoft.com/library/bb490995), um den Dienst zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="596da-120">Use the [sc.exe](https://technet.microsoft.com/library/bb490995) command-line tool to create the service.</span></span> <span data-ttu-id="596da-121">Der Wert `binPath` ist der Pfad zu der ausführbaren Datei der App, der den Namen der ausführbaren Datei enthält.</span><span class="sxs-lookup"><span data-stu-id="596da-121">The `binPath` value is the path to the app's executable, which includes the executable file name.</span></span> <span data-ttu-id="596da-122">**Das Leerzeichen zwischen dem Gleichheitszeichen und dem Anführungszeichen am Anfang des Pfads ist erforderlich.**</span><span class="sxs-lookup"><span data-stu-id="596da-122">**The space between the equal sign and the quote character at the start of the path is required.**</span></span>

   ```console
   sc create <SERVICE_NAME> binPath= "<PATH_TO_SERVICE_EXECUTABLE>"
   ```

   <span data-ttu-id="596da-123">Verwenden Sie zum Erstellen eines Diensts, der im Projektordner veröffentlicht wird, den Pfad zum *Veröffentlichungsordner*.</span><span class="sxs-lookup"><span data-stu-id="596da-123">For a service published in the project folder, use the path to the *publish* folder to create the service.</span></span> <span data-ttu-id="596da-124">Im folgenden Beispiel:</span><span class="sxs-lookup"><span data-stu-id="596da-124">In the following example:</span></span>

   * <span data-ttu-id="596da-125">Das Projekt befindet sich im `c:\my_services\AspNetCoreService`-Ordner.</span><span class="sxs-lookup"><span data-stu-id="596da-125">The project resides in the `c:\my_services\AspNetCoreService` folder.</span></span>
   * <span data-ttu-id="596da-126">Das Projekt wird in der `Release`-Konfiguration veröffentlicht.</span><span class="sxs-lookup"><span data-stu-id="596da-126">The project is published in `Release` configuration.</span></span>
   * <span data-ttu-id="596da-127">Der Zielframeworkmoniker (Target Framework Moniker, TFM) ist `netcoreapp2.1`.</span><span class="sxs-lookup"><span data-stu-id="596da-127">The Target Framework Moniker (TFM) is `netcoreapp2.1`.</span></span>
   * <span data-ttu-id="596da-128">Der Runtimebezeichner (RID) ist `win7-x64`.</span><span class="sxs-lookup"><span data-stu-id="596da-128">The Runtime Identifer (RID) is `win7-x64`.</span></span>
   * <span data-ttu-id="596da-129">Der Name der ausführbaren App-Datei lautet *AspNetCoreService.exe*.</span><span class="sxs-lookup"><span data-stu-id="596da-129">The app executable is named *AspNetCoreService.exe*.</span></span>
   * <span data-ttu-id="596da-130">Der Dienst heißt **MyService**.</span><span class="sxs-lookup"><span data-stu-id="596da-130">The service is named **MyService**.</span></span>

   <span data-ttu-id="596da-131">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="596da-131">Example:</span></span>

   ```console
   sc create MyService binPath= "c:\my_services\AspNetCoreService\bin\Release\netcoreapp2.1\win7-x64\publish\AspNetCoreService.exe"
   ```
   
   > [!IMPORTANT]
   > <span data-ttu-id="596da-132">Achten Sie auf das Leerzeichen zwischen dem `binPath=`-Argument und seinem Wert.</span><span class="sxs-lookup"><span data-stu-id="596da-132">Make sure the space is present between the `binPath=` argument and its value.</span></span>
   
   <span data-ttu-id="596da-133">So veröffentlichen und starten Sie den Dienst aus einem anderen Ordner:</span><span class="sxs-lookup"><span data-stu-id="596da-133">To publish and start the service from a different folder:</span></span>
   
      1. <span data-ttu-id="596da-134">Verwenden Sie die Option [--output &lt;OUTPUT_DIRECTORY&gt;](/dotnet/core/tools/dotnet-publish#options) im Befehl `dotnet publish`.</span><span class="sxs-lookup"><span data-stu-id="596da-134">Use the [--output &lt;OUTPUT_DIRECTORY&gt;](/dotnet/core/tools/dotnet-publish#options) option on the `dotnet publish` command.</span></span> <span data-ttu-id="596da-135">Wenn Sie Visual Studio verwenden, konfigurieren Sie den **Zielspeicherort** auf der Seite **FolderProfile** der publish-Eigenschaft, bevor Sie auf die Schaltfläche **Veröffentlichen** klicken.</span><span class="sxs-lookup"><span data-stu-id="596da-135">If using Visual Studio, configure the **Target Location** in the **FolderProfile** publish property page before selecting the **Publish** button.</span></span>
   1. <span data-ttu-id="596da-136">Erstellen Sie den Dienst mit dem Befehl `sc.exe`, indem Sie den Pfad des Ausgabeordners verwenden.</span><span class="sxs-lookup"><span data-stu-id="596da-136">Create the service with the `sc.exe` command using the output folder path.</span></span> <span data-ttu-id="596da-137">Fügen Sie den Namen der ausführbaren Datei des Diensts in dem Pfad hinzu, der für `binPath` bereitgestellt wird.</span><span class="sxs-lookup"><span data-stu-id="596da-137">Include the name of the service's executable in the path provided to `binPath`.</span></span>

1. <span data-ttu-id="596da-138">Starten Sie den Dienst mithilfe des Befehls `sc start <SERVICE_NAME>`.</span><span class="sxs-lookup"><span data-stu-id="596da-138">Start the service with the `sc start <SERVICE_NAME>` command.</span></span>

   <span data-ttu-id="596da-139">Verwenden Sie zum Starten des Diensts der Beispiel-App den folgenden Befehl:</span><span class="sxs-lookup"><span data-stu-id="596da-139">To start the sample app service, use the following command:</span></span>

   ```console
   sc start MyService
   ```

   <span data-ttu-id="596da-140">Das Starten des Diensts dauert ein paar Sekunden.</span><span class="sxs-lookup"><span data-stu-id="596da-140">The command takes a few seconds to start the service.</span></span>

1. <span data-ttu-id="596da-141">Der `sc query <SERVICE_NAME>`-Befehl kann dazu verwendet werden, den Status des Diensts zu überprüfen und zu bestimmen:</span><span class="sxs-lookup"><span data-stu-id="596da-141">The `sc query <SERVICE_NAME>` command can be used to check the status of the service to determine its status:</span></span>

   * `START_PENDING`
   * `RUNNING`
   * `STOP_PENDING`
   * `STOPPED`

   <span data-ttu-id="596da-142">Verwenden Sie den folgenden Befehl, um den Status des Diensts der Beispiel-App zu überprüfen:</span><span class="sxs-lookup"><span data-stu-id="596da-142">Use the following command to check the status of the sample app service:</span></span>

   ```console
   sc query MyService
   ```

1. <span data-ttu-id="596da-143">Befindet sich der Dienst im Status `RUNNING`, und ist der Dienst gleichzeitig eine Web-App, rufen Sie die App über ihren Pfad auf (Standardpfad ist `http://localhost:5000`, der umgeleitet wird zu `https://localhost:5001`, wenn [Middleware für HTTPS-Umleitung](xref:security/enforcing-ssl) verwendet wird).</span><span class="sxs-lookup"><span data-stu-id="596da-143">When the service is in the `RUNNING` state and if the service is a web app, browse the app at its path (by default, `http://localhost:5000`, which redirects to `https://localhost:5001` when using [HTTPS Redirection Middleware](xref:security/enforcing-ssl)).</span></span>

   <span data-ttu-id="596da-144">Rufen Sie die App für den Dienst der Beispiel-App über `http://localhost:5000` auf.</span><span class="sxs-lookup"><span data-stu-id="596da-144">For the sample app service, browse the app at `http://localhost:5000`.</span></span>

1. <span data-ttu-id="596da-145">Verwenden Sie zum Beenden des Diensts den Befehl `sc stop <SERVICE_NAME>`.</span><span class="sxs-lookup"><span data-stu-id="596da-145">Stop the service with the `sc stop <SERVICE_NAME>` command.</span></span>

   <span data-ttu-id="596da-146">Verwenden Sie zum Beenden des Diensts der Beispiel-App den folgenden Befehl:</span><span class="sxs-lookup"><span data-stu-id="596da-146">The following command stops the sample app service:</span></span>

   ```console
   sc stop MyService
   ```

1. <span data-ttu-id="596da-147">Deinstallieren Sie den Dienst nach einer kurzen Verzögerung zum Beenden des Diensts mit dem Befehl `sc delete <SERVICE_NAME>`.</span><span class="sxs-lookup"><span data-stu-id="596da-147">After a short delay to stop a service, uninstall the service with the `sc delete <SERVICE_NAME>` command.</span></span>

   <span data-ttu-id="596da-148">Überprüfen Sie den Status des Diensts der Beispiel-App:</span><span class="sxs-lookup"><span data-stu-id="596da-148">Check the status of the sample app service:</span></span>

   ```console
   sc query MyService
   ```

   <span data-ttu-id="596da-149">Befindet sich der Dienst der Beispiel-App im Status `STOPPED`, verwenden Sie zum Deinstallieren des Diensts der Beispiel-App den folgenden Befehl:</span><span class="sxs-lookup"><span data-stu-id="596da-149">When the sample app service is in the `STOPPED` state, use the following command to uninstall the sample app service:</span></span>

   ```console
   sc delete MyService
   ```

## <a name="provide-a-way-to-run-outside-of-a-service"></a><span data-ttu-id="596da-150">Bieten einer Möglichkeit zur Ausführung außerhalb eines Diensts</span><span class="sxs-lookup"><span data-stu-id="596da-150">Provide a way to run outside of a service</span></span>

<span data-ttu-id="596da-151">Das Testen und Debuggen ist bei der Ausführung außerhalb eines Diensts einfacher. Daher ist es üblich, dass Code hinzugefügt wird, der `RunAsService` nur unter bestimmten Bedingungen aufruft.</span><span class="sxs-lookup"><span data-stu-id="596da-151">It's easier to test and debug when running outside of a service, so it's customary to add code that calls `RunAsService` only under certain conditions.</span></span> <span data-ttu-id="596da-152">Beispielsweise kann die App als Konsolen-App mit dem Befehlszeilenargument `--console` ausgeführt werden oder wenn der Debugger angefügt wird:</span><span class="sxs-lookup"><span data-stu-id="596da-152">For example, the app can run as a console app with a `--console` command-line argument or if the debugger is attached:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](windows-service/samples/2.x/AspNetCoreService/Program.cs?name=ServiceOrConsole)]

<span data-ttu-id="596da-153">Da für die Konfiguration von ASP.NET Core Name/Wert-Paare für Befehlszeilenargumente erforderlich sind, wird der `--console`-Schalter entfernt, bevor die Argumente an [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) übergeben werden.</span><span class="sxs-lookup"><span data-stu-id="596da-153">Because ASP.NET Core configuration requires name-value pairs for command-line arguments, the `--console` switch is removed before the arguments are passed to [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder).</span></span>

> [!NOTE]
> <span data-ttu-id="596da-154">`isService` wird nicht von `Main` an `CreateWebHostBuilder` übergeben. Die Signatur von `CreateWebHostBuilder` muss `CreateWebHostBuilder(string[])` sein, damit die [Integrationstests](xref:test/integration-tests) korrekt ablaufen.</span><span class="sxs-lookup"><span data-stu-id="596da-154">`isService` isn't passed from `Main` into `CreateWebHostBuilder` because the signature of `CreateWebHostBuilder` must be `CreateWebHostBuilder(string[])` in order for [integration testing](xref:test/integration-tests) to work properly.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](windows-service/samples_snapshot/1.x/AspNetCoreService/Program.cs?name=ServiceOrConsole)]

::: moniker-end

## <a name="handle-stopping-and-starting-events"></a><span data-ttu-id="596da-155">Behandeln des Stoppens und Startens von Ereignissen</span><span class="sxs-lookup"><span data-stu-id="596da-155">Handle stopping and starting events</span></span>

<span data-ttu-id="596da-156">Nehmen Sie zum Behandeln von [OnStarting](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstarting)-, [OnStarted](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstarted)- und [OnStopping](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstopping)-Ereignissen die folgenden zusätzlichen Änderungen vor:</span><span class="sxs-lookup"><span data-stu-id="596da-156">To handle [OnStarting](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstarting), [OnStarted](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstarted), and [OnStopping](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstopping) events, make the following additional changes:</span></span>

1. <span data-ttu-id="596da-157">Erstellen Sie eine von [WebHostService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice) abgeleitete Klasse:</span><span class="sxs-lookup"><span data-stu-id="596da-157">Create a class that derives from [WebHostService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice):</span></span>

   [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/CustomWebHostService.cs?name=NoLogging)]

2. <span data-ttu-id="596da-158">Erstellen Sie eine Erweiterungsmethode für [IWebHost](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost), die den benutzerdefinierten `WebHostService` an [ServiceBase.Run](/dotnet/api/system.serviceprocess.servicebase.run) übergibt:</span><span class="sxs-lookup"><span data-stu-id="596da-158">Create an extension method for [IWebHost](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost) that passes the custom `WebHostService` to [ServiceBase.Run](/dotnet/api/system.serviceprocess.servicebase.run):</span></span>

   [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/WebHostServiceExtensions.cs?name=ExtensionsClass)]

3. <span data-ttu-id="596da-159">Rufen Sie in `Program.Main` anstelle von [RunAsService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostwindowsserviceextensions.runasservice) die neue Erweiterungsmethode `RunAsCustomService` auf:</span><span class="sxs-lookup"><span data-stu-id="596da-159">In `Program.Main`, call the new extension method, `RunAsCustomService`, instead of [RunAsService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostwindowsserviceextensions.runasservice):</span></span>

   ::: moniker range=">= aspnetcore-2.0"

   [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/Program.cs?name=HandleStopStart&highlight=17)]

   > [!NOTE]
   > <span data-ttu-id="596da-160">`isService` wird nicht von `Main` an `CreateWebHostBuilder` übergeben. Die Signatur von `CreateWebHostBuilder` muss `CreateWebHostBuilder(string[])` sein, damit die [Integrationstests](xref:test/integration-tests) korrekt ablaufen.</span><span class="sxs-lookup"><span data-stu-id="596da-160">`isService` isn't passed from `Main` into `CreateWebHostBuilder` because the signature of `CreateWebHostBuilder` must be `CreateWebHostBuilder(string[])` in order for [integration testing](xref:test/integration-tests) to work properly.</span></span>

   ::: moniker-end

   ::: moniker range="< aspnetcore-2.0"

   [!code-csharp[](windows-service/samples_snapshot/1.x/AspNetCoreService/Program.cs?name=HandleStopStart&highlight=27)]

   ::: moniker-end

<span data-ttu-id="596da-161">Wenn für den benutzerdefinierten `WebHostService`-Code ein Dienst aus der Abhängigkeitsinjektion erforderlich ist (z.B. eine Protokollierung), rufen Sie diese über die [IWebHost.Services](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost.services)-Eigenschaft ab:</span><span class="sxs-lookup"><span data-stu-id="596da-161">If the custom `WebHostService` code requires a service from dependency injection (such as a logger), obtain it from the [IWebHost.Services](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost.services) property:</span></span>

[!code-csharp[](windows-service/samples/2.x/AspNetCoreService/CustomWebHostService.cs?name=Logging&highlight=7-8)]

## <a name="proxy-server-and-load-balancer-scenarios"></a><span data-ttu-id="596da-162">Proxyserver und Lastenausgleichsszenarien</span><span class="sxs-lookup"><span data-stu-id="596da-162">Proxy server and load balancer scenarios</span></span>

<span data-ttu-id="596da-163">Dienste, die mit Anforderungen aus dem Internet oder einem Unternehmensnetzwerk interagieren und hinter einem Proxy oder Lastenausgleich ausgeführt werden, erfordern möglicherweise zusätzliche Konfigurationen.</span><span class="sxs-lookup"><span data-stu-id="596da-163">Services that interact with requests from the Internet or a corporate network and are behind a proxy or load balancer might require additional configuration.</span></span> <span data-ttu-id="596da-164">Weitere Informationen finden Sie unter <xref:host-and-deploy/proxy-load-balancer>.</span><span class="sxs-lookup"><span data-stu-id="596da-164">For more information, see <xref:host-and-deploy/proxy-load-balancer>.</span></span>

## <a name="configure-https"></a><span data-ttu-id="596da-165">HTTPS konfigurieren</span><span class="sxs-lookup"><span data-stu-id="596da-165">Configure HTTPS</span></span>

<span data-ttu-id="596da-166">Geben Sie einen [Kestrel-Server-HTTPS-Endpunktkonfiguration](xref:fundamentals/servers/kestrel#endpoint-configuration) ein.</span><span class="sxs-lookup"><span data-stu-id="596da-166">Specify a [Kestrel server HTTPS endpoint configuration](xref:fundamentals/servers/kestrel#endpoint-configuration).</span></span>

## <a name="current-directory-and-content-root"></a><span data-ttu-id="596da-167">Aktuelles Verzeichnis und Inhaltsstammverzeichnis</span><span class="sxs-lookup"><span data-stu-id="596da-167">Current directory and content root</span></span>

<span data-ttu-id="596da-168">Das aktuelle Arbeitsverzeichnis, das durch Aufrufen von `Directory.GetCurrentDirectory()` für einen Windows-Dienst zurückgegeben wird, ist der Ordner *C:\WINDOWS\system32*.</span><span class="sxs-lookup"><span data-stu-id="596da-168">The current working directory returned by calling `Directory.GetCurrentDirectory()` for a Windows Service is the *C:\WINDOWS\system32* folder.</span></span> <span data-ttu-id="596da-169">Der Ordner *system32* ist kein geeigneter Speicherort für die Dateien eines Diensts (z.B. Einstellungsdateien).</span><span class="sxs-lookup"><span data-stu-id="596da-169">The *system32* folder isn't a suitable location to store a service's files (for example, settings files).</span></span> <span data-ttu-id="596da-170">Verwenden Sie einen der folgenden Ansätze, um die Ressourcen und Einstellungsdateien eines Diensts mit [FileConfigurationExtensions.SetBasePath](/dotnet/api/microsoft.extensions.configuration.fileconfigurationextensions.setbasepath) zu verwalten und auf diese zuzugreifen, wenn Sie [IConfigurationBuilder](/dotnet/api/microsoft.extensions.configuration.iconfigurationbuilder) verwenden:</span><span class="sxs-lookup"><span data-stu-id="596da-170">Use one of the following approaches to maintain and access a service's assets and settings files with [FileConfigurationExtensions.SetBasePath](/dotnet/api/microsoft.extensions.configuration.fileconfigurationextensions.setbasepath) when using an [IConfigurationBuilder](/dotnet/api/microsoft.extensions.configuration.iconfigurationbuilder):</span></span>

* <span data-ttu-id="596da-171">Verwenden Sie den Inhaltsstammpfad.</span><span class="sxs-lookup"><span data-stu-id="596da-171">Use the content root path.</span></span> <span data-ttu-id="596da-172">`IHostingEnvironment.ContentRootPath` entspricht dem gleichen Pfad, der für das Argument `binPath` bereitgestellt wird, wenn der Dienst erstellt wird.</span><span class="sxs-lookup"><span data-stu-id="596da-172">The `IHostingEnvironment.ContentRootPath` is the same path provided to the `binPath` argument when the service is created.</span></span> <span data-ttu-id="596da-173">Anstatt `Directory.GetCurrentDirectory()` zum Erstellen von Pfaden zu Einstellungsdateien zu verwenden, verwenden Sie den Inhaltsstammpfad, und verwalten Sie die Dateien im Inhaltsstammverzeichnis der App.</span><span class="sxs-lookup"><span data-stu-id="596da-173">Instead of using `Directory.GetCurrentDirectory()` to create paths to settings files, use the content root path and maintain the files in the app's content root.</span></span>
* <span data-ttu-id="596da-174">Speichern Sie die Dateien an einem geeigneten Speicherort auf dem Datenträger.</span><span class="sxs-lookup"><span data-stu-id="596da-174">Store the files in a suitable location on disk.</span></span> <span data-ttu-id="596da-175">Geben Sie mit `SetBasePath` einen absoluten Pfad zu dem Ordner an, der die Dateien enthält.</span><span class="sxs-lookup"><span data-stu-id="596da-175">Specify an absolute path with `SetBasePath` to the folder containing the files.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="596da-176">Zusätzliche Ressourcen</span><span class="sxs-lookup"><span data-stu-id="596da-176">Additional resources</span></span>

* <span data-ttu-id="596da-177">[Kestrel-Endpunktkonfiguration](xref:fundamentals/servers/kestrel#endpoint-configuration) (einschließlich HTTPS-Konfiguration und Unterstützung für SNI)</span><span class="sxs-lookup"><span data-stu-id="596da-177">[Kestrel endpoint configuration](xref:fundamentals/servers/kestrel#endpoint-configuration) (includes HTTPS configuration and SNI support)</span></span>
* <xref:fundamentals/host/web-host>
