---
title: Host in einem Windowsdienst
author: tdykstra
description: Erfahren Sie, wie eine ASP.NET Core-Anwendung in einem Windows-Dienst zu hosten.
manager: wpickett
ms.author: tdykstra
ms.custom: mvc
ms.date: 03/30/2017
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/windows-service
ms.openlocfilehash: beda34dbd613f6ffe0afa207ab57dd6ebbc489ee
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2018
---
# <a name="host-an-aspnet-core-app-in-a-windows-service"></a><span data-ttu-id="4573c-103">Hosten Sie eine ASP.NET Core-app in einem Windowsdienst</span><span class="sxs-lookup"><span data-stu-id="4573c-103">Host an ASP.NET Core app in a Windows Service</span></span>

<span data-ttu-id="4573c-104">Durch [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="4573c-104">By [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="4573c-105">Die empfohlene Methode, um eine ASP.NET Core-app unter Windows zu hosten, ohne mit IIS ist die Ausführung in einem [Windowsdienst](https://docs.microsoft.com/dotnet/framework/windows-services/introduction-to-windows-service-applications).</span><span class="sxs-lookup"><span data-stu-id="4573c-105">The recommended way to host an ASP.NET Core app on Windows without using IIS is to run it in a [Windows Service](https://docs.microsoft.com/dotnet/framework/windows-services/introduction-to-windows-service-applications).</span></span> <span data-ttu-id="4573c-106">Auf diese Weise können sie automatisch nach Neustarts und Abstürzen, starten, ohne sich im Wartezustand.</span><span class="sxs-lookup"><span data-stu-id="4573c-106">That way it can automatically start after reboots and crashes, without waiting for someone to log in.</span></span>

<span data-ttu-id="4573c-107">[Anzeigen oder Herunterladen von Beispielcode](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/windows-service/sample) ([zum Herunterladen von](xref:tutorials/index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="4573c-107">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/windows-service/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample)).</span></span> <span data-ttu-id="4573c-108">Finden Sie unter der [Arbeitsschritte](#next-steps) Abschnitt Anleitungen für die Ausführung.</span><span class="sxs-lookup"><span data-stu-id="4573c-108">See the [Next Steps](#next-steps) section for instructions on how to run it.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="4573c-109">Erforderliche Komponenten</span><span class="sxs-lookup"><span data-stu-id="4573c-109">Prerequisites</span></span>

* <span data-ttu-id="4573c-110">In der .NET Framework-Laufzeit muss die app auszuführen.</span><span class="sxs-lookup"><span data-stu-id="4573c-110">The app must run on the .NET Framework runtime.</span></span>  <span data-ttu-id="4573c-111">In der *csproj* Datei, geben Sie entsprechende Werte für [TargetFramework](https://docs.microsoft.com/nuget/schema/target-frameworks) und [RuntimeIdentifier](https://docs.microsoft.com/dotnet/articles/core/rid-catalog).</span><span class="sxs-lookup"><span data-stu-id="4573c-111">In the *.csproj* file, specify appropriate values for [TargetFramework](https://docs.microsoft.com/nuget/schema/target-frameworks) and [RuntimeIdentifier](https://docs.microsoft.com/dotnet/articles/core/rid-catalog).</span></span> <span data-ttu-id="4573c-112">Im Folgenden ein Beispiel:</span><span class="sxs-lookup"><span data-stu-id="4573c-112">Here's an example:</span></span>

  [!code-xml[](windows-service/sample/AspNetCoreService.csproj?range=3-6)]

  <span data-ttu-id="4573c-113">Verwenden Sie beim Erstellen eines Projekts in Visual Studio die **Core ASP.NET-Anwendung ((.NET Framework)** Vorlage.</span><span class="sxs-lookup"><span data-stu-id="4573c-113">When creating a project in Visual Studio, use the **ASP.NET Core Application (.NET Framework)** template.</span></span>

* <span data-ttu-id="4573c-114">Wenn die app aus dem Internet (nicht nur über ein internes Netzwerk) Anforderungen empfängt, verwenden sie die [WebListener](xref:fundamentals/servers/weblistener) Webserver statt [Kestrel](xref:fundamentals/servers/kestrel).</span><span class="sxs-lookup"><span data-stu-id="4573c-114">If the app receives requests from the Internet (not just from an internal network), it must use the [WebListener](xref:fundamentals/servers/weblistener) web server rather than [Kestrel](xref:fundamentals/servers/kestrel).</span></span>  <span data-ttu-id="4573c-115">Kestrel muss mit IIS für Edge-Bereitstellungen verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="4573c-115">Kestrel must be used with IIS for edge deployments.</span></span>  <span data-ttu-id="4573c-116">Weitere Informationen finden Sie unter [When to use Kestrel with a reverse proxy (Verwenden von Kestrel mit einem Reverseproxy)](xref:fundamentals/servers/kestrel#when-to-use-kestrel-with-a-reverse-proxy).</span><span class="sxs-lookup"><span data-stu-id="4573c-116">For more information, see [When to use Kestrel with a reverse proxy](xref:fundamentals/servers/kestrel#when-to-use-kestrel-with-a-reverse-proxy).</span></span>

## <a name="getting-started"></a><span data-ttu-id="4573c-117">Erste Schritte</span><span class="sxs-lookup"><span data-stu-id="4573c-117">Getting started</span></span>

<span data-ttu-id="4573c-118">In diesem Abschnitt wird erläutert, die minimale Änderungen erforderlich, um ein vorhandenes Projekt für ASP.NET Core einrichten, um in einem Dienst ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="4573c-118">This section explains the minimum changes required to set up an existing ASP.NET Core project to run in a service.</span></span>

* <span data-ttu-id="4573c-119">Installieren Sie das NuGet-Paket [Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices/).</span><span class="sxs-lookup"><span data-stu-id="4573c-119">Install the NuGet package [Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices/).</span></span>

* <span data-ttu-id="4573c-120">Nehmen Sie die folgenden Änderungen in `Program.Main`:</span><span class="sxs-lookup"><span data-stu-id="4573c-120">Make the following changes in `Program.Main`:</span></span>
  
  * <span data-ttu-id="4573c-121">Rufen Sie `host.RunAsService` anstelle von `host.Run`.</span><span class="sxs-lookup"><span data-stu-id="4573c-121">Call `host.RunAsService` instead of `host.Run`.</span></span>
  
  * <span data-ttu-id="4573c-122">Wenn der Code ruft `UseContentRoot`, verwenden Sie einen Pfad an den Veröffentlichungsort anstelle von`Directory.GetCurrentDirectory()`</span><span class="sxs-lookup"><span data-stu-id="4573c-122">If the code calls `UseContentRoot`, use a path to the publish location instead of `Directory.GetCurrentDirectory()`</span></span> 
  
  [!code-csharp[](windows-service/sample/Program.cs?name=ServiceOnly&highlight=3-4,8,14)]

* <span data-ttu-id="4573c-123">Veröffentlichen Sie die Anwendung in einen Ordner.</span><span class="sxs-lookup"><span data-stu-id="4573c-123">Publish the application to a folder.</span></span>

  <span data-ttu-id="4573c-124">Verwendung [Dotnet veröffentlichen](https://docs.microsoft.com/dotnet/articles/core/tools/dotnet-publish) oder ein [Visual Studio das Veröffentlichungsprofil](xref:host-and-deploy/visual-studio-publish-profiles) , die in einen Ordner veröffentlicht.</span><span class="sxs-lookup"><span data-stu-id="4573c-124">Use [dotnet publish](https://docs.microsoft.com/dotnet/articles/core/tools/dotnet-publish) or a [Visual Studio publish profile](xref:host-and-deploy/visual-studio-publish-profiles) that publishes to a folder.</span></span>

* <span data-ttu-id="4573c-125">Testen von erstellen und den Dienst zu starten.</span><span class="sxs-lookup"><span data-stu-id="4573c-125">Test by creating and starting the service.</span></span>

  <span data-ttu-id="4573c-126">Öffnen Sie eine Administrator-Eingabeaufforderung mit dem [sc.exe](https://technet.microsoft.com/library/bb490995) Befehlszeilentool zum Erstellen und Starten eines Diensts.</span><span class="sxs-lookup"><span data-stu-id="4573c-126">Open an administrator command prompt window to use the [sc.exe](https://technet.microsoft.com/library/bb490995) command-line tool to create and start a service.</span></span>  
  
  <span data-ttu-id="4573c-127">Wenn der Dienst "MyService" benannt wird, veröffentlichen Sie die app `c:\svc`, und die app selbst heißt AspNetCoreService, würde die Befehle wie folgt aussehen:</span><span class="sxs-lookup"><span data-stu-id="4573c-127">If the service is named MyService, publish the app to `c:\svc`, and the app itself is named AspNetCoreService, the commands would look like this:</span></span>

  ```console
  sc create MyService binPath="C:\Svc\AspNetCoreService.exe"
  sc start MyService
  ```

  <span data-ttu-id="4573c-128">Die `binPath` Wert ist der Pfad zur ausführbaren Datei der der app, einschließlich der ausführbaren Datei selbst.</span><span class="sxs-lookup"><span data-stu-id="4573c-128">The `binPath` value is the path to the app's executable, including the executable filename itself.</span></span>

  ![Konsolenfenster erstellen und starten Beispiel](windows-service/_static/create-start.png)

  <span data-ttu-id="4573c-130">Wenn diese Befehle abgeschlossen haben, navigieren Sie zu dem gleichen Pfad wie bei Ausführung als eine Konsolen-app (standardmäßig `http://localhost:5000`)</span><span class="sxs-lookup"><span data-stu-id="4573c-130">When these commands finish, browse to the same path as when running as a console app (by default, `http://localhost:5000`)</span></span>

  ![In einem Dienst ausgeführt wird](windows-service/_static/running-in-service.png)


## <a name="provide-a-way-to-run-outside-of-a-service"></a><span data-ttu-id="4573c-132">Bieten Sie eine Möglichkeit, die außerhalb eines Diensts ausführen</span><span class="sxs-lookup"><span data-stu-id="4573c-132">Provide a way to run outside of a service</span></span>

<span data-ttu-id="4573c-133">Es ist einfacher, testen und Debuggen außerhalb von einem Dienst ausgeführt wird, daher ist es üblich, So fügen Sie Code hinzu, die aufruft `host.RunAsService` nur unter bestimmten Bedingungen.</span><span class="sxs-lookup"><span data-stu-id="4573c-133">It's easier to test and debug when running outside of a service, so it's customary to add code that calls `host.RunAsService` only under certain conditions.</span></span>  <span data-ttu-id="4573c-134">Beispielsweise kann die app auszuführen, als eine Konsolen-app mit einer `--console` Befehlszeilenargument oder wenn der Debugger angefügt ist.</span><span class="sxs-lookup"><span data-stu-id="4573c-134">For example, the app can run as a console app with a `--console` command-line argument or if the debugger is attached.</span></span>

[!code-csharp[](windows-service/sample/Program.cs?name=ServiceOrConsole)]

## <a name="handle-stopping-and-starting-events"></a><span data-ttu-id="4573c-135">Behandeln von Ereignissen starten und beenden</span><span class="sxs-lookup"><span data-stu-id="4573c-135">Handle stopping and starting events</span></span>

<span data-ttu-id="4573c-136">Behandeln `OnStarting`, `OnStarted`, und `OnStopping` Ereignisse, die folgenden zusätzliche Änderungen vornehmen:</span><span class="sxs-lookup"><span data-stu-id="4573c-136">To handle `OnStarting`, `OnStarted`, and `OnStopping` events, make the following additional changes:</span></span>

* <span data-ttu-id="4573c-137">Erstellen Sie eine von der `WebHostService`-Klasse abgeleitete Klasse.</span><span class="sxs-lookup"><span data-stu-id="4573c-137">Create a class that derives from `WebHostService`.</span></span>

  [!code-csharp[](windows-service/sample/CustomWebHostService.cs?name=NoLogging)]

* <span data-ttu-id="4573c-138">Erstellen Sie eine Erweiterungsmethode für `IWebHost` , die die benutzerdefinierte übergibt `WebHostService` auf `ServiceBase.Run`.</span><span class="sxs-lookup"><span data-stu-id="4573c-138">Create an extension method for `IWebHost` that passes the custom `WebHostService` to `ServiceBase.Run`.</span></span>

  [!code-csharp[](windows-service/sample/WebHostServiceExtensions.cs?name=ExtensionsClass)]

* <span data-ttu-id="4573c-139">In `Program.Main` Änderung rufen Sie die neue Erweiterungsmethode anstelle von `host.RunAsService`.</span><span class="sxs-lookup"><span data-stu-id="4573c-139">In `Program.Main` change call the new extension method instead of `host.RunAsService`.</span></span>

  [!code-csharp[](windows-service/sample/Program.cs?name=HandleStopStart&highlight=26)]

<span data-ttu-id="4573c-140">Wenn die benutzerdefinierte `WebHostService` auszugehen zum Abrufen eines Diensts vom abhängigkeiteneinschleusung (z. B. eine Protokollierung), können Sie es aus der `Services` Eigenschaft `IWebHost`.</span><span class="sxs-lookup"><span data-stu-id="4573c-140">If the custom `WebHostService` code needs to get a service from dependency injection (such as a logger), get it from the `Services` property of `IWebHost`.</span></span>

[!code-csharp[](windows-service/sample/CustomWebHostService.cs?name=Logging&highlight=7)]

## <a name="next-steps"></a><span data-ttu-id="4573c-141">Nächste Schritte</span><span class="sxs-lookup"><span data-stu-id="4573c-141">Next steps</span></span>

<span data-ttu-id="4573c-142">Die [beispielanwendung](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/windows-service/sample) , die begleitet dieser Artikel ist eine einfache MVC-Web-app, die wie gezeigt in vorherigen Codebeispiele geändert wurde.</span><span class="sxs-lookup"><span data-stu-id="4573c-142">The [sample application](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/windows-service/sample) that accompanies this article is a simple MVC web app that has been modified as shown in preceding code examples.</span></span>  <span data-ttu-id="4573c-143">Um es in einem Dienst auszuführen, führen Sie die folgenden Schritte aus:</span><span class="sxs-lookup"><span data-stu-id="4573c-143">To run it in a service, do the following steps:</span></span>

* <span data-ttu-id="4573c-144">Veröffentlichen in *c:\svc*.</span><span class="sxs-lookup"><span data-stu-id="4573c-144">Publish to *c:\svc*.</span></span>

* <span data-ttu-id="4573c-145">Öffnen Sie eine Administratorfenster.</span><span class="sxs-lookup"><span data-stu-id="4573c-145">Open an administrator window.</span></span>

* <span data-ttu-id="4573c-146">Geben Sie die folgenden Befehle aus:</span><span class="sxs-lookup"><span data-stu-id="4573c-146">Enter the following commands:</span></span>

  ```console
  sc create MyService binPath="c:\svc\aspnetcoreservice.exe"
  sc start MyService
  ```

  * <span data-ttu-id="4573c-147">Wechseln Sie in einem Browser zu http://localhost: 5000, um sicherzustellen, dass er ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="4573c-147">In a browser, go to http://localhost:5000 to verify that it's running.</span></span>

<span data-ttu-id="4573c-148">Wenn die app nicht wie erwartet, bei der Ausführung in einem Dienst gestartet, ist eine schnelle Möglichkeit, Fehlermeldungen zugänglich sind zum Hinzufügen eines Anbieters für die Protokollierung wie dem [Anbieter Windows-Ereignisprotokoll](xref:fundamentals/logging/index#eventlog).</span><span class="sxs-lookup"><span data-stu-id="4573c-148">If the app doesn't start up as expected when running in a service, a quick way to make error messages accessible is to add a logging provider such as the [Windows EventLog provider](xref:fundamentals/logging/index#eventlog).</span></span>

## <a name="acknowledgments"></a><span data-ttu-id="4573c-149">Bestätigungen</span><span class="sxs-lookup"><span data-stu-id="4573c-149">Acknowledgments</span></span>

<span data-ttu-id="4573c-150">In diesem Artikel wurde mit Hilfe von Quellen geschrieben, die bereits veröffentlicht wurden.</span><span class="sxs-lookup"><span data-stu-id="4573c-150">This article was written with the help of sources that were already published.</span></span> <span data-ttu-id="4573c-151">Das früheste und besonders hilfreich, diese wurden diese:</span><span class="sxs-lookup"><span data-stu-id="4573c-151">The earliest and most useful of them were these:</span></span>

* [<span data-ttu-id="4573c-152">Hosten von ASP.NET Core als Windows-Dienst</span><span class="sxs-lookup"><span data-stu-id="4573c-152">Hosting ASP.NET Core as Windows service</span></span>](https://stackoverflow.com/questions/37346383/hosting-asp-net-core-as-windows-service/37464074)
* [<span data-ttu-id="4573c-153">Wie Ihre ASP.NET Core in einem Windows-Dienst gehostet wird.</span><span class="sxs-lookup"><span data-stu-id="4573c-153">How to host your ASP.NET Core in a Windows Service</span></span>](https://dotnetthoughts.net/how-to-host-your-aspnet-core-in-a-windows-service/)
