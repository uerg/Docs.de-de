---
title: Hosten von ASP.NET Core in einem Windows-Dienst
author: guardrex
description: Erfahren Sie, wie eine ASP.NET Core-App in einem Windows-Dienst gehostet wird.
ms.author: tdykstra
ms.custom: mvc
ms.date: 06/04/2018
uid: host-and-deploy/windows-service
ms.openlocfilehash: 0149039f69539b7c69d7ba45efcf09d80ffcba79
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/20/2018
ms.locfileid: "36275097"
---
# <a name="host-aspnet-core-in-a-windows-service"></a><span data-ttu-id="c9c76-103">Hosten von ASP.NET Core in einem Windows-Dienst</span><span class="sxs-lookup"><span data-stu-id="c9c76-103">Host ASP.NET Core in a Windows Service</span></span>

<span data-ttu-id="c9c76-104">Von [Luke Latham](https://github.com/guardrex) und [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="c9c76-104">By [Luke Latham](https://github.com/guardrex) and [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="c9c76-105">Eine ASP.NET Core-App kann unter Windows gehostet werden, ohne IIS als [Windows-Dienst](/dotnet/framework/windows-services/introduction-to-windows-service-applications) zu verwenden.</span><span class="sxs-lookup"><span data-stu-id="c9c76-105">An ASP.NET Core app can be hosted on Windows without using IIS as a [Windows Service](/dotnet/framework/windows-services/introduction-to-windows-service-applications).</span></span> <span data-ttu-id="c9c76-106">Wird die App als Windows-Dienst gehostet, kann sie nach Neustarts und Abstürzen automatisch gestartet werden, ohne dass manuelles Eingreifen erforderlich wird.</span><span class="sxs-lookup"><span data-stu-id="c9c76-106">When hosted as a Windows Service, the app can automatically start after reboots and crashes without requiring human intervention.</span></span>

<span data-ttu-id="c9c76-107">[Anzeigen oder Herunterladen von Beispielcode](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/windows-service/sample) ([Vorgehensweise zum Herunterladen](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="c9c76-107">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/windows-service/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="get-started"></a><span data-ttu-id="c9c76-108">Erste Schritte</span><span class="sxs-lookup"><span data-stu-id="c9c76-108">Get started</span></span>

<span data-ttu-id="c9c76-109">Die folgenden minimalen Änderungen sind für die Einrichtung eines vorhandenen ASP.NET Core-Projekts zur Ausführung in einem Dienst erforderlich:</span><span class="sxs-lookup"><span data-stu-id="c9c76-109">The following minimum changes are required to set up an existing ASP.NET Core project to run in a service:</span></span>

1. <span data-ttu-id="c9c76-110">In der Projektdatei:</span><span class="sxs-lookup"><span data-stu-id="c9c76-110">In the project file:</span></span>

   1. <span data-ttu-id="c9c76-111">Bestätigen Sie das Vorhandensein des Runtime-Bezeichners, oder fügen Sie diesen der **\<Eigenschaftengruppe>** hinzu, die das Zielframework enthält:</span><span class="sxs-lookup"><span data-stu-id="c9c76-111">Confirm the presence of the runtime identifier or add it to the **\<PropertyGroup>** that contains the target framework:</span></span>
      ```xml
      <PropertyGroup>
        <TargetFramework>netcoreapp2.1</TargetFramework>
        <RuntimeIdentifier>win7-x64</RuntimeIdentifier>
      </PropertyGroup>
      ```
   1. <span data-ttu-id="c9c76-112">Fügen Sie einen Paketverweis auf [Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices/) hinzu.</span><span class="sxs-lookup"><span data-stu-id="c9c76-112">Add a package reference for [Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices/).</span></span>

1. <span data-ttu-id="c9c76-113">Nehmen Sie in `Program.Main` die folgenden Änderungen vor:</span><span class="sxs-lookup"><span data-stu-id="c9c76-113">Make the following changes in `Program.Main`:</span></span>

   * <span data-ttu-id="c9c76-114">Rufen Sie [host.RunAsService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostwindowsserviceextensions.runasservice) anstelle von `host.Run` auf.</span><span class="sxs-lookup"><span data-stu-id="c9c76-114">Call [host.RunAsService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostwindowsserviceextensions.runasservice) instead of `host.Run`.</span></span>

   * <span data-ttu-id="c9c76-115">Wenn der Code `UseContentRoot` aufruft, verwenden Sie anstelle von `Directory.GetCurrentDirectory()` einen Pfad zum veröffentlichten Speicherort der App.</span><span class="sxs-lookup"><span data-stu-id="c9c76-115">If the code calls `UseContentRoot`, use a path to the app's published location instead of `Directory.GetCurrentDirectory()`.</span></span>

     ::: moniker range=">= aspnetcore-2.0"

     <span data-ttu-id="c9c76-116">[!code-csharp[](windows-service/sample/Program.cs?name=ServiceOnly&highlight=3-4,7,11)]</span><span class="sxs-lookup"><span data-stu-id="c9c76-116">[!code-csharp[](windows-service/sample/Program.cs?name=ServiceOnly&highlight=3-4,7,11)]</span></span>

     ::: moniker-end

     ::: moniker range="< aspnetcore-2.0"

     <span data-ttu-id="c9c76-117">[!code-csharp[](windows-service/sample_snapshot/Program.cs?name=ServiceOnly&highlight=3-4,8,13)]</span><span class="sxs-lookup"><span data-stu-id="c9c76-117">[!code-csharp[](windows-service/sample_snapshot/Program.cs?name=ServiceOnly&highlight=3-4,8,13)]</span></span>

     ::: moniker-end

1. <span data-ttu-id="c9c76-118">Veröffentlichen Sie die App in einem Ordner.</span><span class="sxs-lookup"><span data-stu-id="c9c76-118">Publish the app to a folder.</span></span> <span data-ttu-id="c9c76-119">Verwenden Sie [dotnet publish](/dotnet/articles/core/tools/dotnet-publish) oder ein [Visual Studio-Veröffentlichungsprofil](xref:host-and-deploy/visual-studio-publish-profiles) für die Veröffentlichung in einem Ordner.</span><span class="sxs-lookup"><span data-stu-id="c9c76-119">Use [dotnet publish](/dotnet/articles/core/tools/dotnet-publish) or a [Visual Studio publish profile](xref:host-and-deploy/visual-studio-publish-profiles) that publishes to a folder.</span></span>

   <span data-ttu-id="c9c76-120">Führen Sie zum Veröffentlichen der Beispiel-App über die Befehlszeile den folgenden Befehl in einem Konsolenfenster des Projektordners aus:</span><span class="sxs-lookup"><span data-stu-id="c9c76-120">To publish the sample app from the command line, run the following command in a console window from the project folder:</span></span>

   ```console
   dotnet publish --configuration Release --output c:\svc
   ```

1. <span data-ttu-id="c9c76-121">Verwenden Sie das Befehlszeilentool [sc.exe](https://technet.microsoft.com/library/bb490995), um den Dienst (`sc create <SERVICE_NAME> binPath= "<PATH_TO_SERVICE_EXECUTABLE>"`) zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="c9c76-121">Use the [sc.exe](https://technet.microsoft.com/library/bb490995) command-line tool to create the service (`sc create <SERVICE_NAME> binPath= "<PATH_TO_SERVICE_EXECUTABLE>"`).</span></span> <span data-ttu-id="c9c76-122">Der Wert `binPath` ist der Pfad zu der ausführbaren Datei der App, der den Namen der ausführbaren Datei enthält.</span><span class="sxs-lookup"><span data-stu-id="c9c76-122">The `binPath` value is the path to the app's executable, which includes the executable file name.</span></span> <span data-ttu-id="c9c76-123">**Das Leerzeichen zwischen dem Gleichheitszeichen und den beginnenden Anführungszeichen am Anfang des Pfads ist erforderlich.**</span><span class="sxs-lookup"><span data-stu-id="c9c76-123">**The space between the equal sign and the quote character that starts the path is required.**</span></span>

   <span data-ttu-id="c9c76-124">Der Dienst für die Beispiel-App und den folgenden Befehl besitzt die folgenden Eigenschaften:</span><span class="sxs-lookup"><span data-stu-id="c9c76-124">For the sample app and command that follows, the service is:</span></span>

   * <span data-ttu-id="c9c76-125">Name: **MyService**</span><span class="sxs-lookup"><span data-stu-id="c9c76-125">Named **MyService**.</span></span>
   * <span data-ttu-id="c9c76-126">Veröffentlicht im Ordner *c:\\svc*</span><span class="sxs-lookup"><span data-stu-id="c9c76-126">Published to *c:\\svc* folder.</span></span>
   * <span data-ttu-id="c9c76-127">Verfügt über eine ausführbare App-Datei mit dem Namen *AspNetCoreService.exe*</span><span class="sxs-lookup"><span data-stu-id="c9c76-127">Has an app executable named *AspNetCoreService.exe*.</span></span>

   <span data-ttu-id="c9c76-128">Öffnen Sie eine Befehlsshell mit Administratorrechten, und führen Sie den folgenden Befehl aus:</span><span class="sxs-lookup"><span data-stu-id="c9c76-128">Open a command shell with administrative privileges and run the following command:</span></span>

   ```console
   sc create MyService binPath= "c:\svc\aspnetcoreservice.exe"
   ```

   <span data-ttu-id="c9c76-129">**Achten Sie auf das Leerzeichen zwischen dem `binPath=`-Argument und seinem Wert.**</span><span class="sxs-lookup"><span data-stu-id="c9c76-129">**Make sure the space is present between the `binPath=` argument and its value.**</span></span>

1. <span data-ttu-id="c9c76-130">Starten Sie den Dienst mithilfe des Befehls `sc start <SERVICE_NAME>`.</span><span class="sxs-lookup"><span data-stu-id="c9c76-130">Start the service with the `sc start <SERVICE_NAME>` command.</span></span>

   <span data-ttu-id="c9c76-131">Verwenden Sie zum Starten des Diensts der Beispiel-App den folgenden Befehl:</span><span class="sxs-lookup"><span data-stu-id="c9c76-131">To start the sample app service, use the following command:</span></span>

   ```console
   sc start MyService
   ```

   <span data-ttu-id="c9c76-132">Das Starten des Diensts dauert ein paar Sekunden.</span><span class="sxs-lookup"><span data-stu-id="c9c76-132">The command takes a few seconds to start the service.</span></span>

1. <span data-ttu-id="c9c76-133">Der `sc query <SERVICE_NAME>`-Befehl kann dazu verwendet werden, den Status des Diensts zu überprüfen und zu bestimmen:</span><span class="sxs-lookup"><span data-stu-id="c9c76-133">The `sc query <SERVICE_NAME>` command can be used to check the status of the service to determine its status:</span></span>

   * `START_PENDING`
   * `RUNNING`
   * `STOP_PENDING`
   * `STOPPED`

   <span data-ttu-id="c9c76-134">Verwenden Sie den folgenden Befehl, um den Status des Diensts der Beispiel-App zu überprüfen:</span><span class="sxs-lookup"><span data-stu-id="c9c76-134">Use the following command to check the status of the sample app service:</span></span>

   ```console
   sc query MyService
   ```

1. <span data-ttu-id="c9c76-135">Befindet sich der Dienst im Status `RUNNING`, und ist der Dienst gleichzeitig eine Web-App, rufen Sie die App über ihren Pfad auf (Standardpfad ist `http://localhost:5000`, der umgeleitet wird zu `https://localhost:5001`, wenn [Middleware für HTTPS-Umleitung](xref:security/enforcing-ssl) verwendet wird).</span><span class="sxs-lookup"><span data-stu-id="c9c76-135">When the service is in the `RUNNING` state and if the service is a web app, browse the app at its path (by default, `http://localhost:5000`, which redirects to `https://localhost:5001` when using [HTTPS Redirection Middleware](xref:security/enforcing-ssl)).</span></span>

   <span data-ttu-id="c9c76-136">Rufen Sie die App für den Dienst der Beispiel-App über `http://localhost:5000` auf.</span><span class="sxs-lookup"><span data-stu-id="c9c76-136">For the sample app service, browse the app at `http://localhost:5000`.</span></span>

1. <span data-ttu-id="c9c76-137">Verwenden Sie zum Beenden des Diensts den Befehl `sc stop <SERVICE_NAME>`.</span><span class="sxs-lookup"><span data-stu-id="c9c76-137">Stop the service with the `sc stop <SERVICE_NAME>` command.</span></span>

   <span data-ttu-id="c9c76-138">Verwenden Sie zum Beenden des Diensts der Beispiel-App den folgenden Befehl:</span><span class="sxs-lookup"><span data-stu-id="c9c76-138">The following command stops the sample app service:</span></span>

   ```console
   sc stop MyService
   ```

1. <span data-ttu-id="c9c76-139">Deinstallieren Sie den Dienst nach einer kurzen Verzögerung zum Beenden des Diensts mit dem Befehl `sc delete <SERVICE_NAME>`.</span><span class="sxs-lookup"><span data-stu-id="c9c76-139">After a short delay to stop a service, uninstall the service with the `sc delete <SERVICE_NAME>` command.</span></span>

   <span data-ttu-id="c9c76-140">Überprüfen Sie den Status des Diensts der Beispiel-App:</span><span class="sxs-lookup"><span data-stu-id="c9c76-140">Check the status of the sample app service:</span></span>

   ```console
   sc query MyService
   ```

   <span data-ttu-id="c9c76-141">Befindet sich der Dienst der Beispiel-App im Status `STOPPED`, verwenden Sie zum Deinstallieren des Diensts der Beispiel-App den folgenden Befehl:</span><span class="sxs-lookup"><span data-stu-id="c9c76-141">When the sample app service is in the `STOPPED` state, use the following command to uninstall the sample app service:</span></span>

   ```console
   sc delete MyService
   ```

## <a name="provide-a-way-to-run-outside-of-a-service"></a><span data-ttu-id="c9c76-142">Bieten einer Möglichkeit zur Ausführung außerhalb eines Diensts</span><span class="sxs-lookup"><span data-stu-id="c9c76-142">Provide a way to run outside of a service</span></span>

<span data-ttu-id="c9c76-143">Das Testen und Debuggen ist bei der Ausführung außerhalb eines Diensts einfacher. Daher ist es üblich, dass Code hinzugefügt wird, der `RunAsService` nur unter bestimmten Bedingungen aufruft.</span><span class="sxs-lookup"><span data-stu-id="c9c76-143">It's easier to test and debug when running outside of a service, so it's customary to add code that calls `RunAsService` only under certain conditions.</span></span> <span data-ttu-id="c9c76-144">Beispielsweise kann die App als Konsolen-App mit dem Befehlszeilenargument `--console` ausgeführt werden oder wenn der Debugger angefügt wird:</span><span class="sxs-lookup"><span data-stu-id="c9c76-144">For example, the app can run as a console app with a `--console` command-line argument or if the debugger is attached:</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="c9c76-145">[!code-csharp[](windows-service/sample/Program.cs?name=ServiceOrConsole)]</span><span class="sxs-lookup"><span data-stu-id="c9c76-145">[!code-csharp[](windows-service/sample/Program.cs?name=ServiceOrConsole)]</span></span>

<span data-ttu-id="c9c76-146">Da für die Konfiguration von ASP.NET Core Name/Wert-Paare für Befehlszeilenargumente erforderlich sind, wird der `--console`-Schalter entfernt, bevor die Argumente an [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) übergeben werden.</span><span class="sxs-lookup"><span data-stu-id="c9c76-146">Because ASP.NET Core configuration requires name-value pairs for command-line arguments, the `--console` switch is removed before the arguments are passed to [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="c9c76-147">[!code-csharp[](windows-service/sample_snapshot/Program.cs?name=ServiceOrConsole)]</span><span class="sxs-lookup"><span data-stu-id="c9c76-147">[!code-csharp[](windows-service/sample_snapshot/Program.cs?name=ServiceOrConsole)]</span></span>

::: moniker-end

## <a name="handle-stopping-and-starting-events"></a><span data-ttu-id="c9c76-148">Behandeln des Stoppens und Startens von Ereignissen</span><span class="sxs-lookup"><span data-stu-id="c9c76-148">Handle stopping and starting events</span></span>

<span data-ttu-id="c9c76-149">Nehmen Sie zum Behandeln von [OnStarting](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstarting)-, [OnStarted](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstarted)- und [OnStopping](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstopping)-Ereignissen die folgenden zusätzlichen Änderungen vor:</span><span class="sxs-lookup"><span data-stu-id="c9c76-149">To handle [OnStarting](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstarting), [OnStarted](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstarted), and [OnStopping](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstopping) events, make the following additional changes:</span></span>

1. <span data-ttu-id="c9c76-150">Erstellen Sie eine von [WebHostService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice) abgeleitete Klasse:</span><span class="sxs-lookup"><span data-stu-id="c9c76-150">Create a class that derives from [WebHostService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice):</span></span>

   [!code-csharp[](windows-service/sample/CustomWebHostService.cs?name=NoLogging)]

2. <span data-ttu-id="c9c76-151">Erstellen Sie eine Erweiterungsmethode für [IWebHost](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost), die den benutzerdefinierten `WebHostService` an [ServiceBase.Run](/dotnet/api/system.serviceprocess.servicebase.run) übergibt:</span><span class="sxs-lookup"><span data-stu-id="c9c76-151">Create an extension method for [IWebHost](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost) that passes the custom `WebHostService` to [ServiceBase.Run](/dotnet/api/system.serviceprocess.servicebase.run):</span></span>

   [!code-csharp[](windows-service/sample/WebHostServiceExtensions.cs?name=ExtensionsClass)]

3. <span data-ttu-id="c9c76-152">Rufen Sie in `Program.Main` anstelle von [RunAsService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostwindowsserviceextensions.runasservice) die neue Erweiterungsmethode `RunAsCustomService` auf:</span><span class="sxs-lookup"><span data-stu-id="c9c76-152">In `Program.Main`, call the new extension method, `RunAsCustomService`, instead of [RunAsService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostwindowsserviceextensions.runasservice):</span></span>

   ::: moniker range=">= aspnetcore-2.0"

   <span data-ttu-id="c9c76-153">[!code-csharp[](windows-service/sample/Program.cs?name=HandleStopStart&highlight=27)]</span><span class="sxs-lookup"><span data-stu-id="c9c76-153">[!code-csharp[](windows-service/sample/Program.cs?name=HandleStopStart&highlight=27)]</span></span>

   ::: moniker-end

   ::: moniker range="< aspnetcore-2.0"

   <span data-ttu-id="c9c76-154">[!code-csharp[](windows-service/sample_snapshot/Program.cs?name=HandleStopStart&highlight=27)]</span><span class="sxs-lookup"><span data-stu-id="c9c76-154">[!code-csharp[](windows-service/sample_snapshot/Program.cs?name=HandleStopStart&highlight=27)]</span></span>

   ::: moniker-end

<span data-ttu-id="c9c76-155">Wenn für den benutzerdefinierten `WebHostService`-Code ein Dienst aus der Abhängigkeitsinjektion erforderlich ist (z.B. eine Protokollierung), rufen Sie diese über die [IWebHost.Services](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost.services)-Eigenschaft ab:</span><span class="sxs-lookup"><span data-stu-id="c9c76-155">If the custom `WebHostService` code requires a service from dependency injection (such as a logger), obtain it from the [IWebHost.Services](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost.services) property:</span></span>

[!code-csharp[](windows-service/sample/CustomWebHostService.cs?name=Logging&highlight=7)]

## <a name="proxy-server-and-load-balancer-scenarios"></a><span data-ttu-id="c9c76-156">Proxyserver und Lastenausgleichsszenarien</span><span class="sxs-lookup"><span data-stu-id="c9c76-156">Proxy server and load balancer scenarios</span></span>

<span data-ttu-id="c9c76-157">Dienste, die mit Anforderungen aus dem Internet oder einem Unternehmensnetzwerk interagieren und hinter einem Proxy oder Lastenausgleich ausgeführt werden, erfordern möglicherweise zusätzliche Konfigurationen.</span><span class="sxs-lookup"><span data-stu-id="c9c76-157">Services that interact with requests from the Internet or a corporate network and are behind a proxy or load balancer might require additional configuration.</span></span> <span data-ttu-id="c9c76-158">Weitere Informationen hierzu feinden Sie unter [Konfigurieren von ASP.NET Core zur Verwendung mit Proxyservern und Lastenausgleich](xref:host-and-deploy/proxy-load-balancer).</span><span class="sxs-lookup"><span data-stu-id="c9c76-158">For more information, see [Configure ASP.NET Core to work with proxy servers and load balancers](xref:host-and-deploy/proxy-load-balancer).</span></span>

## <a name="kestrel-endpoint-configuration"></a><span data-ttu-id="c9c76-159">Kestrel-Endpunktkonfiguration</span><span class="sxs-lookup"><span data-stu-id="c9c76-159">Kestrel endpoint configuration</span></span>

<span data-ttu-id="c9c76-160">Informationen zur Kestrel-Endpunktkonfiguration, einschließlich HTTPS-Konfiguration und Unterstützung der Servernamensanzeige, finden Sie unter [Kestrel endpoint configuration (Kestrel-Endpunktkonfiguration)](xref:fundamentals/servers/kestrel#endpoint-configuration).</span><span class="sxs-lookup"><span data-stu-id="c9c76-160">For information on Kestrel endpoint configuration, including HTTPS configuration and SNI support, see [Kestrel endpoint configuration](xref:fundamentals/servers/kestrel#endpoint-configuration).</span></span>
