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
ms.openlocfilehash: 5b54c77ff9e019b1d550aa687923077a3e9ba5c2
ms.sourcegitcommit: 78d28178345a0eea91556e4cd1adad98b1446db8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/22/2017
---
# <a name="host-an-aspnet-core-app-in-a-windows-service"></a><span data-ttu-id="7266d-104">Hosten Sie eine ASP.NET Core-app in einem Windowsdienst</span><span class="sxs-lookup"><span data-stu-id="7266d-104">Host an ASP.NET Core app in a Windows Service</span></span>

<span data-ttu-id="7266d-105">Durch [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="7266d-105">By [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="7266d-106">Die empfohlene Methode zum Hosten einer ASP.NET Core-app unter Windows bei Verwendung von IIS nicht ist, führen Sie es in einem [Windowsdienst](https://docs.microsoft.com/dotnet/framework/windows-services/introduction-to-windows-service-applications).</span><span class="sxs-lookup"><span data-stu-id="7266d-106">The recommended way to host an ASP.NET Core app on Windows when you don't use IIS is to run it in a [Windows Service](https://docs.microsoft.com/dotnet/framework/windows-services/introduction-to-windows-service-applications).</span></span> <span data-ttu-id="7266d-107">Auf diese Weise können sie automatisch nach Neustarts und Abstürzen, starten, ohne sich im Wartezustand.</span><span class="sxs-lookup"><span data-stu-id="7266d-107">That way it can automatically start after reboots and crashes, without waiting for someone to log in.</span></span>

<span data-ttu-id="7266d-108">[Anzeigen oder Herunterladen von Beispielcode](https://github.com/aspnet/Docs/tree/master/aspnetcore/hosting/windows-service/sample) finden Sie unter der [Arbeitsschritte](#next-steps) Abschnitt Anleitungen für die Ausführung.</span><span class="sxs-lookup"><span data-stu-id="7266d-108">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/hosting/windows-service/sample) See the [Next Steps](#next-steps) section for instructions on how to run it.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="7266d-109">Erforderliche Komponenten</span><span class="sxs-lookup"><span data-stu-id="7266d-109">Prerequisites</span></span>

* <span data-ttu-id="7266d-110">In der .NET Framework-Laufzeit muss die app auszuführen.</span><span class="sxs-lookup"><span data-stu-id="7266d-110">The app must run on the .NET framework runtime.</span></span>  <span data-ttu-id="7266d-111">In der *csproj* Datei, geben Sie entsprechende Werte für [TargetFramework](https://docs.microsoft.com/nuget/schema/target-frameworks) und [RuntimeIdentifier](https://docs.microsoft.com/dotnet/articles/core/rid-catalog).</span><span class="sxs-lookup"><span data-stu-id="7266d-111">In the *.csproj* file, specify appropriate values for [TargetFramework](https://docs.microsoft.com/nuget/schema/target-frameworks) and [RuntimeIdentifier](https://docs.microsoft.com/dotnet/articles/core/rid-catalog).</span></span> <span data-ttu-id="7266d-112">Im Folgenden ein Beispiel:</span><span class="sxs-lookup"><span data-stu-id="7266d-112">Here's an example:</span></span>

  [!code-xml[](windows-service/sample/AspNetCoreService.csproj?range=3-6)]

  <span data-ttu-id="7266d-113">Verwenden Sie beim Erstellen eines Projekts in Visual Studio die **Core ASP.NET-Anwendung ((.NET Framework)** Vorlage.</span><span class="sxs-lookup"><span data-stu-id="7266d-113">When creating a project in Visual Studio, use the **ASP.NET Core Application (.NET Framework)** template.</span></span>

* <span data-ttu-id="7266d-114">Wenn die app aus dem Internet (nicht nur über ein internes Netzwerk) die Anforderungen erhalten werden, verwenden sie die [WebListener](xref:fundamentals/servers/weblistener) Webserver statt [Kestrel](xref:fundamentals/servers/kestrel).</span><span class="sxs-lookup"><span data-stu-id="7266d-114">If the app will get requests from the internet (not just from an internal network), it must use the [WebListener](xref:fundamentals/servers/weblistener) web server rather than [Kestrel](xref:fundamentals/servers/kestrel).</span></span>  <span data-ttu-id="7266d-115">Kestrel muss mit IIS für Edge-Bereitstellungen verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="7266d-115">Kestrel must be used with IIS for edge deployments.</span></span>  <span data-ttu-id="7266d-116">Weitere Informationen finden Sie unter [When to use Kestrel with a reverse proxy (Verwenden von Kestrel mit einem Reverseproxy)](xref:fundamentals/servers/kestrel#when-to-use-kestrel-with-a-reverse-proxy).</span><span class="sxs-lookup"><span data-stu-id="7266d-116">For more information, see [When to use Kestrel with a reverse proxy](xref:fundamentals/servers/kestrel#when-to-use-kestrel-with-a-reverse-proxy).</span></span>

## <a name="getting-started"></a><span data-ttu-id="7266d-117">Erste Schritte</span><span class="sxs-lookup"><span data-stu-id="7266d-117">Getting started</span></span>

<span data-ttu-id="7266d-118">In diesem Abschnitt wird erläutert, die minimale Änderungen erforderlich, um ein vorhandenes Projekt für ASP.NET Core einrichten, um in einem Dienst ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="7266d-118">This section explains the minimum changes required to set up an existing ASP.NET Core project to run in a service.</span></span>

* <span data-ttu-id="7266d-119">Installieren Sie das NuGet-Paket [Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices/).</span><span class="sxs-lookup"><span data-stu-id="7266d-119">Install the NuGet package [Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices/).</span></span>

* <span data-ttu-id="7266d-120">Nehmen Sie die folgenden Änderungen in `Program.Main`:</span><span class="sxs-lookup"><span data-stu-id="7266d-120">Make the following changes in `Program.Main`:</span></span>
  
  * <span data-ttu-id="7266d-121">Rufen Sie `host.RunAsService` anstelle von `host.Run`.</span><span class="sxs-lookup"><span data-stu-id="7266d-121">Call `host.RunAsService` instead of `host.Run`.</span></span>
  
  * <span data-ttu-id="7266d-122">Wenn Sie Ihren Code aufruft `UseContentRoot`, verwenden Sie einen Pfad an den Veröffentlichungsort anstelle von`Directory.GetCurrentDirectory()`</span><span class="sxs-lookup"><span data-stu-id="7266d-122">If your code calls `UseContentRoot`, use a path to the publish location instead of `Directory.GetCurrentDirectory()`</span></span> 
  
  [!code-csharp[](windows-service/sample/Program.cs?name=ServiceOnly&highlight=3-4,8,14)]

* <span data-ttu-id="7266d-123">Veröffentlichen Sie die Anwendung in einen Ordner.</span><span class="sxs-lookup"><span data-stu-id="7266d-123">Publish the application to a folder.</span></span>

  <span data-ttu-id="7266d-124">Verwendung [Dotnet veröffentlichen](https://docs.microsoft.com/dotnet/articles/core/tools/dotnet-publish) oder ein [Visual Studio das Veröffentlichungsprofil](xref:publishing/web-publishing-vs) , die in einen Ordner veröffentlicht.</span><span class="sxs-lookup"><span data-stu-id="7266d-124">Use [dotnet publish](https://docs.microsoft.com/dotnet/articles/core/tools/dotnet-publish) or a [Visual Studio publish profile](xref:publishing/web-publishing-vs) that publishes to a folder.</span></span>

* <span data-ttu-id="7266d-125">Testen von erstellen und den Dienst zu starten.</span><span class="sxs-lookup"><span data-stu-id="7266d-125">Test by creating and starting the service.</span></span>

  <span data-ttu-id="7266d-126">Öffnen Sie eine Administrator-Eingabeaufforderung mit dem [sc.exe](https://technet.microsoft.com/library/bb490995) Befehlszeilentool zum Erstellen und Starten eines Diensts.</span><span class="sxs-lookup"><span data-stu-id="7266d-126">Open an administrator command prompt window to use the [sc.exe](https://technet.microsoft.com/library/bb490995) command-line tool to create and start a service.</span></span>  
  
  <span data-ttu-id="7266d-127">Wenn Sie den Dienst MyService benennen, Veröffentlichen Ihrer app zu `c:\svc`, und die app selbst heißt AspNetCoreService, würde die Befehle wie folgt aussehen:</span><span class="sxs-lookup"><span data-stu-id="7266d-127">If you name your service MyService, you publish your app to `c:\svc`, and the app itself is named AspNetCoreService, the commands would look like this:</span></span>

  ```console
  sc create MyService binPath="C:\Svc\AspNetCoreService.exe"
  sc start MyService
  ```
  <span data-ttu-id="7266d-128">Die `binPath` Wert ist der Pfad zu Ihrer app ausführbare Datei, einschließlich der ausführbaren Datei selbst.</span><span class="sxs-lookup"><span data-stu-id="7266d-128">The `binPath` value is the path to your app's executable, including the executable filename itself.</span></span>

  ![Konsolenfenster erstellen und starten Beispiel](windows-service/_static/create-start.png)

  <span data-ttu-id="7266d-130">Wenn diese Befehle abgeschlossen haben, können Sie navigieren Sie zu dem gleichen Pfad wie beim als eine Konsolen-app ausführen (standardmäßig `http://localhost:5000`)</span><span class="sxs-lookup"><span data-stu-id="7266d-130">When these commands finish, you can browse to the same path as when you run as a console app (by default, `http://localhost:5000`)</span></span>

  ![In einem Dienst ausgeführt wird](windows-service/_static/running-in-service.png)


## <a name="provide-a-way-to-run-outside-of-a-service"></a><span data-ttu-id="7266d-132">Bieten Sie eine Möglichkeit, die außerhalb eines Diensts ausführen</span><span class="sxs-lookup"><span data-stu-id="7266d-132">Provide a way to run outside of a service</span></span>

<span data-ttu-id="7266d-133">Es ist einfacher, testen und Debuggen, wenn Sie außerhalb von einem Dienst ausführen, daher ist es üblich, So fügen Sie Code hinzu, die aufruft `host.RunAsService` nur unter bestimmten Bedingungen.</span><span class="sxs-lookup"><span data-stu-id="7266d-133">It's easier to test and debug when you're running outside of a service, so it's customary to add code that calls `host.RunAsService` only under certain conditions.</span></span>  <span data-ttu-id="7266d-134">Beispielsweise konnte Ausführen als eine Konsolen-app, wenn Sie erhalten eine `--console` Befehlszeilenargument oder wenn der Debugger angefügt ist.</span><span class="sxs-lookup"><span data-stu-id="7266d-134">For example, you could run as a console app if you get a `--console` command-line argument or if the debugger is attached.</span></span>

[!code-csharp[](windows-service/sample/Program.cs?name=ServiceOrConsole)]

## <a name="handle-stopping-and-starting-events"></a><span data-ttu-id="7266d-135">Behandeln von Ereignissen starten und beenden</span><span class="sxs-lookup"><span data-stu-id="7266d-135">Handle stopping and starting events</span></span>

<span data-ttu-id="7266d-136">Wenn Sie behandeln möchten `OnStarting`, `OnStarted`, und `OnStopping` Ereignisse, die folgenden zusätzliche Änderungen vornehmen:</span><span class="sxs-lookup"><span data-stu-id="7266d-136">If you want to handle `OnStarting`, `OnStarted`, and `OnStopping` events, make the following additional changes:</span></span>

* <span data-ttu-id="7266d-137">Erstellen Sie eine von der `WebHostService`-Klasse abgeleitete Klasse.</span><span class="sxs-lookup"><span data-stu-id="7266d-137">Create a class that derives from `WebHostService`.</span></span>

  [!code-csharp[](windows-service/sample/CustomWebHostService.cs?name=NoLogging)]

* <span data-ttu-id="7266d-138">Erstellen Sie eine Erweiterungsmethode für `IWebHost` , die Ihre benutzerdefinierte übergibt `WebHostService` auf `ServiceBase.Run`.</span><span class="sxs-lookup"><span data-stu-id="7266d-138">Create an extension method for `IWebHost` that passes your custom `WebHostService` to `ServiceBase.Run`.</span></span>

  [!code-csharp[](windows-service/sample/WebHostServiceExtensions.cs?name=ExtensionsClass)]

* <span data-ttu-id="7266d-139">In `Program.Main` Änderung rufen Sie die neue Erweiterungsmethode anstelle von `host.RunAsService`.</span><span class="sxs-lookup"><span data-stu-id="7266d-139">In `Program.Main` change call the new extension method instead of `host.RunAsService`.</span></span>

  [!code-csharp[](windows-service/sample/Program.cs?name=HandleStopStart&highlight=26)]

<span data-ttu-id="7266d-140">Wenn Ihre benutzerdefinierte `WebHostService` Code muss einen Dienst von abhängigkeiteneinschleusung (z. B. eine Protokollierung) abrufen, können Sie es aus der `Services` Eigenschaft `IWebHost`.</span><span class="sxs-lookup"><span data-stu-id="7266d-140">If your custom `WebHostService` code needs to get a service from dependency injection (such as a logger), you can get it from the `Services` property of `IWebHost`.</span></span>

[!code-csharp[](windows-service/sample/CustomWebHostService.cs?name=Logging&highlight=7)]

## <a name="next-steps"></a><span data-ttu-id="7266d-141">Nächste Schritte</span><span class="sxs-lookup"><span data-stu-id="7266d-141">Next steps</span></span>

<span data-ttu-id="7266d-142">Die [beispielanwendung](https://github.com/aspnet/Docs/tree/master/aspnetcore/hosting/windows-service/sample) , die begleitet dieser Artikel ist eine einfache MVC-Web-app, die wie gezeigt in vorherigen Codebeispiele geändert wurde.</span><span class="sxs-lookup"><span data-stu-id="7266d-142">The [sample application](https://github.com/aspnet/Docs/tree/master/aspnetcore/hosting/windows-service/sample) that accompanies this article is a simple MVC web app that has been modified as shown in preceding code examples.</span></span>  <span data-ttu-id="7266d-143">Um es in einem Dienst auszuführen, führen Sie die folgenden Schritte aus:</span><span class="sxs-lookup"><span data-stu-id="7266d-143">To run it in a service, do the following steps:</span></span>

* <span data-ttu-id="7266d-144">Veröffentlichen in *c:\svc*.</span><span class="sxs-lookup"><span data-stu-id="7266d-144">Publish to *c:\svc*.</span></span>

* <span data-ttu-id="7266d-145">Öffnen Sie eine Administratorfenster.</span><span class="sxs-lookup"><span data-stu-id="7266d-145">Open an administrator window.</span></span>

* <span data-ttu-id="7266d-146">Geben Sie die folgenden Befehle aus:</span><span class="sxs-lookup"><span data-stu-id="7266d-146">Enter the following commands:</span></span>

  ```console
  sc create MyService binPath="c:\svc\aspnetcoreservice.exe"
  sc start MyService
  ```

  * <span data-ttu-id="7266d-147">Wechseln Sie in einem Browser zu http://localhost: 5000, um sicherzustellen, dass er ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="7266d-147">In a browser, go to http://localhost:5000 to verify that it's running.</span></span>

<span data-ttu-id="7266d-148">Wenn die app nicht wie erwartet, bei der Ausführung in einem Dienst gestartet, ist eine schnelle Möglichkeit, Fehlermeldungen zugänglich sind zum Hinzufügen eines Anbieters für die Protokollierung wie dem [Anbieter Windows-Ereignisprotokoll](xref:fundamentals/logging#eventlog).</span><span class="sxs-lookup"><span data-stu-id="7266d-148">If the app doesn't start up as expected when running in a service, a quick way to make error messages accessible is to add a logging provider such as the [Windows EventLog provider](xref:fundamentals/logging#eventlog).</span></span>

## <a name="acknowledgments"></a><span data-ttu-id="7266d-149">Bestätigungen</span><span class="sxs-lookup"><span data-stu-id="7266d-149">Acknowledgments</span></span>

<span data-ttu-id="7266d-150">In diesem Artikel wurde mit Hilfe von Quellen geschrieben, die bereits veröffentlicht wurden.</span><span class="sxs-lookup"><span data-stu-id="7266d-150">This article was written with the help of sources that were already published.</span></span> <span data-ttu-id="7266d-151">Das früheste und besonders hilfreich, diese wurden diese:</span><span class="sxs-lookup"><span data-stu-id="7266d-151">The earliest and most useful of them were these:</span></span>

* [<span data-ttu-id="7266d-152">Hosten von ASP.NET Core als Windows-Dienst</span><span class="sxs-lookup"><span data-stu-id="7266d-152">Hosting ASP.NET Core as Windows service</span></span>](https://stackoverflow.com/questions/37346383/hosting-asp-net-core-as-windows-service/37464074)
* [<span data-ttu-id="7266d-153">Wie Ihre ASP.NET Core in einem Windows-Dienst gehostet wird.</span><span class="sxs-lookup"><span data-stu-id="7266d-153">How to host your ASP.NET Core in a Windows Service</span></span>](https://dotnetthoughts.net/how-to-host-your-aspnet-core-in-a-windows-service/)
