---
title: Host in einem Windowsdienst
author: tdykstra
description: Erfahren Sie, wie eine ASP.NET Core-Anwendung in einem Windows-Dienst zu hosten.
keywords: Hosten von ASP.NET Core, Windows-Dienst
ms.author: tdykstra
manager: wpickett
ms.date: 03/30/2017
ms.topic: article
ms.assetid: d9a65066-d7cb-47df-b046-64629c4d2c6f
ms.technology: aspnet
ms.prod: aspnet-core
uid: hosting/windows-service
ms.openlocfilehash: ca3b98f0b0405fcd5751cb7d9bc7a40257739084
ms.sourcegitcommit: 732cd2684246e49e796836596643a8d37e20c46d
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/01/2017
---
# <a name="host-an-aspnet-core-app-in-a-windows-service"></a><span data-ttu-id="16856-104">Hosten Sie eine ASP.NET Core-app in einem Windowsdienst</span><span class="sxs-lookup"><span data-stu-id="16856-104">Host an ASP.NET Core app in a Windows Service</span></span>

<span data-ttu-id="16856-105">Durch [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="16856-105">By [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="16856-106">Die empfohlene Methode zum Hosten einer ASP.NET Core-app unter Windows bei Verwendung von IIS nicht ist, führen Sie es in einem [Windowsdienst](https://docs.microsoft.com/dotnet/framework/windows-services/introduction-to-windows-service-applications).</span><span class="sxs-lookup"><span data-stu-id="16856-106">The recommended way to host an ASP.NET Core app on Windows when you don't use IIS is to run it in a [Windows Service](https://docs.microsoft.com/dotnet/framework/windows-services/introduction-to-windows-service-applications).</span></span> <span data-ttu-id="16856-107">Auf diese Weise können sie automatisch nach Neustarts und Abstürzen, starten, ohne sich im Wartezustand.</span><span class="sxs-lookup"><span data-stu-id="16856-107">That way it can automatically start after reboots and crashes, without waiting for someone to log in.</span></span>

<span data-ttu-id="16856-108">[Anzeigen oder Herunterladen von Beispielcode](https://github.com/aspnet/Docs/tree/master/aspnetcore/hosting/windows-service/sample) ([zum Herunterladen von](xref:tutorials/index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="16856-108">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/hosting/windows-service/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample)).</span></span> <span data-ttu-id="16856-109">Finden Sie unter der [Arbeitsschritte](#next-steps) Abschnitt Anleitungen für die Ausführung.</span><span class="sxs-lookup"><span data-stu-id="16856-109">See the [Next Steps](#next-steps) section for instructions on how to run it.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="16856-110">Erforderliche Komponenten</span><span class="sxs-lookup"><span data-stu-id="16856-110">Prerequisites</span></span>

* <span data-ttu-id="16856-111">In der .NET Framework-Laufzeit muss die app auszuführen.</span><span class="sxs-lookup"><span data-stu-id="16856-111">The app must run on the .NET framework runtime.</span></span>  <span data-ttu-id="16856-112">In der *csproj* Datei, geben Sie entsprechende Werte für [TargetFramework](https://docs.microsoft.com/nuget/schema/target-frameworks) und [RuntimeIdentifier](https://docs.microsoft.com/dotnet/articles/core/rid-catalog).</span><span class="sxs-lookup"><span data-stu-id="16856-112">In the *.csproj* file, specify appropriate values for [TargetFramework](https://docs.microsoft.com/nuget/schema/target-frameworks) and [RuntimeIdentifier](https://docs.microsoft.com/dotnet/articles/core/rid-catalog).</span></span> <span data-ttu-id="16856-113">Im Folgenden ein Beispiel:</span><span class="sxs-lookup"><span data-stu-id="16856-113">Here's an example:</span></span>

  [!code-xml[](windows-service/sample/AspNetCoreService.csproj?range=3-6)]

  <span data-ttu-id="16856-114">Verwenden Sie beim Erstellen eines Projekts in Visual Studio die **Core ASP.NET-Anwendung ((.NET Framework)** Vorlage.</span><span class="sxs-lookup"><span data-stu-id="16856-114">When creating a project in Visual Studio, use the **ASP.NET Core Application (.NET Framework)** template.</span></span>

* <span data-ttu-id="16856-115">Wenn die app aus dem Internet (nicht nur über ein internes Netzwerk) die Anforderungen erhalten werden, verwenden sie die [WebListener](xref:fundamentals/servers/weblistener) Webserver statt [Kestrel](xref:fundamentals/servers/kestrel).</span><span class="sxs-lookup"><span data-stu-id="16856-115">If the app will get requests from the internet (not just from an internal network), it must use the [WebListener](xref:fundamentals/servers/weblistener) web server rather than [Kestrel](xref:fundamentals/servers/kestrel).</span></span>  <span data-ttu-id="16856-116">Kestrel muss mit IIS für Edge-Bereitstellungen verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="16856-116">Kestrel must be used with IIS for edge deployments.</span></span>  <span data-ttu-id="16856-117">Weitere Informationen finden Sie unter [When to use Kestrel with a reverse proxy (Verwenden von Kestrel mit einem Reverseproxy)](xref:fundamentals/servers/kestrel#when-to-use-kestrel-with-a-reverse-proxy).</span><span class="sxs-lookup"><span data-stu-id="16856-117">For more information, see [When to use Kestrel with a reverse proxy](xref:fundamentals/servers/kestrel#when-to-use-kestrel-with-a-reverse-proxy).</span></span>

## <a name="getting-started"></a><span data-ttu-id="16856-118">Erste Schritte</span><span class="sxs-lookup"><span data-stu-id="16856-118">Getting started</span></span>

<span data-ttu-id="16856-119">In diesem Abschnitt wird erläutert, die minimale Änderungen erforderlich, um ein vorhandenes Projekt für ASP.NET Core einrichten, um in einem Dienst ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="16856-119">This section explains the minimum changes required to set up an existing ASP.NET Core project to run in a service.</span></span>

* <span data-ttu-id="16856-120">Installieren Sie das NuGet-Paket [Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices/).</span><span class="sxs-lookup"><span data-stu-id="16856-120">Install the NuGet package [Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices/).</span></span>

* <span data-ttu-id="16856-121">Nehmen Sie die folgenden Änderungen in `Program.Main`:</span><span class="sxs-lookup"><span data-stu-id="16856-121">Make the following changes in `Program.Main`:</span></span>
  
  * <span data-ttu-id="16856-122">Rufen Sie `host.RunAsService` anstelle von `host.Run`.</span><span class="sxs-lookup"><span data-stu-id="16856-122">Call `host.RunAsService` instead of `host.Run`.</span></span>
  
  * <span data-ttu-id="16856-123">Wenn Sie Ihren Code aufruft `UseContentRoot`, verwenden Sie einen Pfad an den Veröffentlichungsort anstelle von`Directory.GetCurrentDirectory()`</span><span class="sxs-lookup"><span data-stu-id="16856-123">If your code calls `UseContentRoot`, use a path to the publish location instead of `Directory.GetCurrentDirectory()`</span></span> 
  
  [!code-csharp[](windows-service/sample/Program.cs?name=ServiceOnly&highlight=3-4,8,14)]

* <span data-ttu-id="16856-124">Veröffentlichen Sie die Anwendung in einen Ordner.</span><span class="sxs-lookup"><span data-stu-id="16856-124">Publish the application to a folder.</span></span>

  <span data-ttu-id="16856-125">Verwendung [Dotnet veröffentlichen](https://docs.microsoft.com/dotnet/articles/core/tools/dotnet-publish) oder ein [Visual Studio das Veröffentlichungsprofil](xref:publishing/web-publishing-vs) , die in einen Ordner veröffentlicht.</span><span class="sxs-lookup"><span data-stu-id="16856-125">Use [dotnet publish](https://docs.microsoft.com/dotnet/articles/core/tools/dotnet-publish) or a [Visual Studio publish profile](xref:publishing/web-publishing-vs) that publishes to a folder.</span></span>

* <span data-ttu-id="16856-126">Testen von erstellen und den Dienst zu starten.</span><span class="sxs-lookup"><span data-stu-id="16856-126">Test by creating and starting the service.</span></span>

  <span data-ttu-id="16856-127">Öffnen Sie eine Administrator-Eingabeaufforderung mit dem [sc.exe](https://technet.microsoft.com/library/bb490995) Befehlszeilentool zum Erstellen und Starten eines Diensts.</span><span class="sxs-lookup"><span data-stu-id="16856-127">Open an administrator command prompt window to use the [sc.exe](https://technet.microsoft.com/library/bb490995) command-line tool to create and start a service.</span></span>  
  
  <span data-ttu-id="16856-128">Wenn Sie den Dienst MyService benennen, Veröffentlichen Ihrer app zu `c:\svc`, und die app selbst heißt AspNetCoreService, würde die Befehle wie folgt aussehen:</span><span class="sxs-lookup"><span data-stu-id="16856-128">If you name your service MyService, you publish your app to `c:\svc`, and the app itself is named AspNetCoreService, the commands would look like this:</span></span>

  ```console
  sc create MyService binPath="C:\Svc\AspNetCoreService.exe"
  sc start MyService
  ```
  <span data-ttu-id="16856-129">Die `binPath` Wert ist der Pfad zu Ihrer app ausführbare Datei, einschließlich der ausführbaren Datei selbst.</span><span class="sxs-lookup"><span data-stu-id="16856-129">The `binPath` value is the path to your app's executable, including the executable filename itself.</span></span>

  ![Konsolenfenster erstellen und starten Beispiel](windows-service/_static/create-start.png)

  <span data-ttu-id="16856-131">Wenn diese Befehle abgeschlossen haben, können Sie navigieren Sie zu dem gleichen Pfad wie beim als eine Konsolen-app ausführen (standardmäßig `http://localhost:5000`)</span><span class="sxs-lookup"><span data-stu-id="16856-131">When these commands finish, you can browse to the same path as when you run as a console app (by default, `http://localhost:5000`)</span></span>

  ![In einem Dienst ausgeführt wird](windows-service/_static/running-in-service.png)


## <a name="provide-a-way-to-run-outside-of-a-service"></a><span data-ttu-id="16856-133">Bieten Sie eine Möglichkeit, die außerhalb eines Diensts ausführen</span><span class="sxs-lookup"><span data-stu-id="16856-133">Provide a way to run outside of a service</span></span>

<span data-ttu-id="16856-134">Es ist einfacher, testen und Debuggen, wenn Sie außerhalb von einem Dienst ausführen, daher ist es üblich, So fügen Sie Code hinzu, die aufruft `host.RunAsService` nur unter bestimmten Bedingungen.</span><span class="sxs-lookup"><span data-stu-id="16856-134">It's easier to test and debug when you're running outside of a service, so it's customary to add code that calls `host.RunAsService` only under certain conditions.</span></span>  <span data-ttu-id="16856-135">Beispielsweise konnte Ausführen als eine Konsolen-app, wenn Sie erhalten eine `--console` Befehlszeilenargument oder wenn der Debugger angefügt ist.</span><span class="sxs-lookup"><span data-stu-id="16856-135">For example, you could run as a console app if you get a `--console` command-line argument or if the debugger is attached.</span></span>

[!code-csharp[](windows-service/sample/Program.cs?name=ServiceOrConsole)]

## <a name="handle-stopping-and-starting-events"></a><span data-ttu-id="16856-136">Behandeln von Ereignissen starten und beenden</span><span class="sxs-lookup"><span data-stu-id="16856-136">Handle stopping and starting events</span></span>

<span data-ttu-id="16856-137">Wenn Sie behandeln möchten `OnStarting`, `OnStarted`, und `OnStopping` Ereignisse, die folgenden zusätzliche Änderungen vornehmen:</span><span class="sxs-lookup"><span data-stu-id="16856-137">If you want to handle `OnStarting`, `OnStarted`, and `OnStopping` events, make the following additional changes:</span></span>

* <span data-ttu-id="16856-138">Erstellen Sie eine von der `WebHostService`-Klasse abgeleitete Klasse.</span><span class="sxs-lookup"><span data-stu-id="16856-138">Create a class that derives from `WebHostService`.</span></span>

  [!code-csharp[](windows-service/sample/CustomWebHostService.cs?name=NoLogging)]

* <span data-ttu-id="16856-139">Erstellen Sie eine Erweiterungsmethode für `IWebHost` , die Ihre benutzerdefinierte übergibt `WebHostService` auf `ServiceBase.Run`.</span><span class="sxs-lookup"><span data-stu-id="16856-139">Create an extension method for `IWebHost` that passes your custom `WebHostService` to `ServiceBase.Run`.</span></span>

  [!code-csharp[](windows-service/sample/WebHostServiceExtensions.cs?name=ExtensionsClass)]

* <span data-ttu-id="16856-140">In `Program.Main` Änderung rufen Sie die neue Erweiterungsmethode anstelle von `host.RunAsService`.</span><span class="sxs-lookup"><span data-stu-id="16856-140">In `Program.Main` change call the new extension method instead of `host.RunAsService`.</span></span>

  [!code-csharp[](windows-service/sample/Program.cs?name=HandleStopStart&highlight=26)]

<span data-ttu-id="16856-141">Wenn Ihre benutzerdefinierte `WebHostService` Code muss einen Dienst von abhängigkeiteneinschleusung (z. B. eine Protokollierung) abrufen, können Sie es aus der `Services` Eigenschaft `IWebHost`.</span><span class="sxs-lookup"><span data-stu-id="16856-141">If your custom `WebHostService` code needs to get a service from dependency injection (such as a logger), you can get it from the `Services` property of `IWebHost`.</span></span>

[!code-csharp[](windows-service/sample/CustomWebHostService.cs?name=Logging&highlight=7)]

## <a name="next-steps"></a><span data-ttu-id="16856-142">Nächste Schritte</span><span class="sxs-lookup"><span data-stu-id="16856-142">Next steps</span></span>

<span data-ttu-id="16856-143">Die [beispielanwendung](https://github.com/aspnet/Docs/tree/master/aspnetcore/hosting/windows-service/sample) , die begleitet dieser Artikel ist eine einfache MVC-Web-app, die wie gezeigt in vorherigen Codebeispiele geändert wurde.</span><span class="sxs-lookup"><span data-stu-id="16856-143">The [sample application](https://github.com/aspnet/Docs/tree/master/aspnetcore/hosting/windows-service/sample) that accompanies this article is a simple MVC web app that has been modified as shown in preceding code examples.</span></span>  <span data-ttu-id="16856-144">Um es in einem Dienst auszuführen, führen Sie die folgenden Schritte aus:</span><span class="sxs-lookup"><span data-stu-id="16856-144">To run it in a service, do the following steps:</span></span>

* <span data-ttu-id="16856-145">Veröffentlichen in *c:\svc*.</span><span class="sxs-lookup"><span data-stu-id="16856-145">Publish to *c:\svc*.</span></span>

* <span data-ttu-id="16856-146">Öffnen Sie eine Administratorfenster.</span><span class="sxs-lookup"><span data-stu-id="16856-146">Open an administrator window.</span></span>

* <span data-ttu-id="16856-147">Geben Sie die folgenden Befehle aus:</span><span class="sxs-lookup"><span data-stu-id="16856-147">Enter the following commands:</span></span>

  ```console
  sc create MyService binPath="c:\svc\aspnetcoreservice.exe"
  sc start MyService
  ```

  * <span data-ttu-id="16856-148">Wechseln Sie in einem Browser zu http://localhost: 5000, um sicherzustellen, dass er ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="16856-148">In a browser, go to http://localhost:5000 to verify that it's running.</span></span>

<span data-ttu-id="16856-149">Wenn die app nicht wie erwartet, bei der Ausführung in einem Dienst gestartet, ist eine schnelle Möglichkeit, Fehlermeldungen zugänglich sind zum Hinzufügen eines Anbieters für die Protokollierung wie dem [Anbieter Windows-Ereignisprotokoll](xref:fundamentals/logging#eventlog).</span><span class="sxs-lookup"><span data-stu-id="16856-149">If the app doesn't start up as expected when running in a service, a quick way to make error messages accessible is to add a logging provider such as the [Windows EventLog provider](xref:fundamentals/logging#eventlog).</span></span>

## <a name="acknowledgments"></a><span data-ttu-id="16856-150">Bestätigungen</span><span class="sxs-lookup"><span data-stu-id="16856-150">Acknowledgments</span></span>

<span data-ttu-id="16856-151">In diesem Artikel wurde mit Hilfe von Quellen geschrieben, die bereits veröffentlicht wurden.</span><span class="sxs-lookup"><span data-stu-id="16856-151">This article was written with the help of sources that were already published.</span></span> <span data-ttu-id="16856-152">Das früheste und besonders hilfreich, diese wurden diese:</span><span class="sxs-lookup"><span data-stu-id="16856-152">The earliest and most useful of them were these:</span></span>

* [<span data-ttu-id="16856-153">Hosten von ASP.NET Core als Windows-Dienst</span><span class="sxs-lookup"><span data-stu-id="16856-153">Hosting ASP.NET Core as Windows service</span></span>](https://stackoverflow.com/questions/37346383/hosting-asp-net-core-as-windows-service/37464074)
* [<span data-ttu-id="16856-154">Wie Ihre ASP.NET Core in einem Windows-Dienst gehostet wird.</span><span class="sxs-lookup"><span data-stu-id="16856-154">How to host your ASP.NET Core in a Windows Service</span></span>](https://dotnetthoughts.net/how-to-host-your-aspnet-core-in-a-windows-service/)
