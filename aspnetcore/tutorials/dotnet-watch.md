---
title: Entwickeln von ASP.NET Core-Apps mit dotnet watch
author: rick-anderson
description: Demonstriert die Verwendung von dotnet watch.
keywords: ASP.NET Core, Verwenden von dotnet watch
ms.author: riande
manager: wpickett
ms.date: 03/09/2017
ms.topic: article
ms.assetid: 563ffb3f-d369-4aa5-bf0a-7300b4e7832c
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/dotnet-watch
ms.openlocfilehash: 6a8943619e6174dbcd3d901b7bb736ba5d3af95d
ms.sourcegitcommit: 78d28178345a0eea91556e4cd1adad98b1446db8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/22/2017
---
# <a name="developing-aspnet-core-apps-using-dotnet-watch"></a><span data-ttu-id="20a37-104">Entwickeln von ASP.NET Core-Apps mit dotnet watch</span><span class="sxs-lookup"><span data-stu-id="20a37-104">Developing ASP.NET Core apps using dotnet watch</span></span>


<span data-ttu-id="20a37-105">Von [Rick Anderson](https://twitter.com/RickAndMSFT) und [Victor Hurdugaci](https://twitter.com/victorhurdugaci)</span><span class="sxs-lookup"><span data-stu-id="20a37-105">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Victor Hurdugaci](https://twitter.com/victorhurdugaci)</span></span>

<span data-ttu-id="20a37-106">`dotnet watch` ist ein Tool, das den Befehl `dotnet` ausführt, wenn sich Quelldateien ändern.</span><span class="sxs-lookup"><span data-stu-id="20a37-106">`dotnet watch` is a tool that runs a `dotnet` command when source files change.</span></span> <span data-ttu-id="20a37-107">Eine Dateiänderung kann beispielsweise eine Kompilierung, Tests oder Bereitstellung auslösen.</span><span class="sxs-lookup"><span data-stu-id="20a37-107">For example, a file change can trigger compilation, tests, or deployment.</span></span>

<span data-ttu-id="20a37-108">In diesem Tutorial verwenden wir eine vorhandene Web-API-App mit zwei Endpunkten: der eine gibt eine Summe, der andere ein Produkt zurück.</span><span class="sxs-lookup"><span data-stu-id="20a37-108">In this tutorial, we use an existing Web API app with two endpoints: one that returns a sum and one that returns a product.</span></span> <span data-ttu-id="20a37-109">Die Produktmethode enthält einen Fehler, den wir im Rahmen dieses Tutorials beheben müssen.</span><span class="sxs-lookup"><span data-stu-id="20a37-109">The product method contains a bug that we'll fix as part of this tutorial.</span></span>

<span data-ttu-id="20a37-110">Laden Sie die [Beispiel-App](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/dotnet-watch/sample) herunter.</span><span class="sxs-lookup"><span data-stu-id="20a37-110">Download the [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/dotnet-watch/sample).</span></span> <span data-ttu-id="20a37-111">Sie enthält zwei Projekte: `WebApp` (eine Web-App) und `WebAppTests` (Komponententests für die Web-App).</span><span class="sxs-lookup"><span data-stu-id="20a37-111">It contains two projects, `WebApp` (a web app) and `WebAppTests` (unit tests for the web app).</span></span>

<span data-ttu-id="20a37-112">Navigieren Sie in einer Konsole zum Ordner „WebApp“, und führen Sie die folgenden Befehle aus:</span><span class="sxs-lookup"><span data-stu-id="20a37-112">In a console, navigate to the WebApp folder and run the following commands:</span></span>

- `dotnet restore`
- `dotnet run`

<span data-ttu-id="20a37-113">Die Konsolenausgabe zeigt Meldungen ähnlich der folgenden (die angibt, dass die App ausgeführt wird und auf Anforderungen wartet):</span><span class="sxs-lookup"><span data-stu-id="20a37-113">The console output will show messages similar to the following (indicating that the app is running and waiting for requests):</span></span>

```console
$ dotnet run
Hosting environment: Production
Content root path: C:/Docs/aspnetcore/tutorials/dotnet-watch/sample/WebApp
Now listening on: http://localhost:5000
Application started. Press Ctrl+C to shut down.
```

<span data-ttu-id="20a37-114">Wenn Sie in einem Webbrowser zu `http://localhost:5000/api/math/sum?a=4&b=5` wechseln, sollten Sie das Ergebnis `9` sehen.</span><span class="sxs-lookup"><span data-stu-id="20a37-114">In a web browser, navigate to `http://localhost:5000/api/math/sum?a=4&b=5`, you should see the result `9`.</span></span>

<span data-ttu-id="20a37-115">Navigieren Sie zur Produkt-API (`http://localhost:5000/api/math/product?a=4&b=5`), die `9` und nicht wie erwartet `20` ausgibt.</span><span class="sxs-lookup"><span data-stu-id="20a37-115">Navigate to the product API (`http://localhost:5000/api/math/product?a=4&b=5`), it returns `9`, not `20` as you'd expect.</span></span> <span data-ttu-id="20a37-116">Dies korrigieren wir später im Tutorial.</span><span class="sxs-lookup"><span data-stu-id="20a37-116">We'll fix that later in the tutorial.</span></span>

## <a name="add-dotnet-watch-to-a-project"></a><span data-ttu-id="20a37-117">Hinzufügen von `dotnet watch` zu einem Projekt</span><span class="sxs-lookup"><span data-stu-id="20a37-117">Add `dotnet watch` to a project</span></span>

- <span data-ttu-id="20a37-118">Fügen Sie `Microsoft.DotNet.Watcher.Tools` der Datei *.csproj* hinzu:</span><span class="sxs-lookup"><span data-stu-id="20a37-118">Add `Microsoft.DotNet.Watcher.Tools` to the *.csproj* file:</span></span>
 ```xml
 <ItemGroup>
   <DotNetCliToolReference Include="Microsoft.DotNet.Watcher.Tools" Version="2.0.0" />
 </ItemGroup> 
 ```

- <span data-ttu-id="20a37-119">Führen Sie `dotnet restore` aus.</span><span class="sxs-lookup"><span data-stu-id="20a37-119">Run `dotnet restore`.</span></span>

## <a name="running-dotnet-commands-using-dotnet-watch"></a><span data-ttu-id="20a37-120">Ausführen von `dotnet`-Befehlen mit `dotnet watch`</span><span class="sxs-lookup"><span data-stu-id="20a37-120">Running `dotnet` commands using `dotnet watch`</span></span>

<span data-ttu-id="20a37-121">Alle `dotnet`-Befehle können mit `dotnet watch` ausgeführt werden, wie z.B.:</span><span class="sxs-lookup"><span data-stu-id="20a37-121">Any `dotnet` command can be run with `dotnet watch`, for example:</span></span>

| <span data-ttu-id="20a37-122">Befehl</span><span class="sxs-lookup"><span data-stu-id="20a37-122">Command</span></span> | <span data-ttu-id="20a37-123">Befehl mit watch</span><span class="sxs-lookup"><span data-stu-id="20a37-123">Command with watch</span></span> |
| ---- | ----- |
| <span data-ttu-id="20a37-124">dotnet run</span><span class="sxs-lookup"><span data-stu-id="20a37-124">dotnet run</span></span> | <span data-ttu-id="20a37-125">dotnet watch run</span><span class="sxs-lookup"><span data-stu-id="20a37-125">dotnet watch run</span></span> |
| <span data-ttu-id="20a37-126">dotnet run -f net451</span><span class="sxs-lookup"><span data-stu-id="20a37-126">dotnet run -f net451</span></span> | <span data-ttu-id="20a37-127">dotnet watch run -f net451</span><span class="sxs-lookup"><span data-stu-id="20a37-127">dotnet watch run -f net451</span></span> |
| <span data-ttu-id="20a37-128">dotnet run -f net451 -- --arg1</span><span class="sxs-lookup"><span data-stu-id="20a37-128">dotnet run -f net451 -- --arg1</span></span> | <span data-ttu-id="20a37-129">dotnet watch run -f net451 -- --arg1</span><span class="sxs-lookup"><span data-stu-id="20a37-129">dotnet watch run -f net451 -- --arg1</span></span> |
| <span data-ttu-id="20a37-130">dotnet test</span><span class="sxs-lookup"><span data-stu-id="20a37-130">dotnet test</span></span> | <span data-ttu-id="20a37-131">dotnet watch test</span><span class="sxs-lookup"><span data-stu-id="20a37-131">dotnet watch test</span></span> |

<span data-ttu-id="20a37-132">Führen Sie `dotnet watch run` im Ordner `WebApp` aus.</span><span class="sxs-lookup"><span data-stu-id="20a37-132">Run `dotnet watch run` in the `WebApp` folder.</span></span> <span data-ttu-id="20a37-133">Die Konsolenausgabe gibt an, dass `watch` gestartet wurde.</span><span class="sxs-lookup"><span data-stu-id="20a37-133">The console output will indicate `watch` has started.</span></span>

## <a name="making-changes-with-dotnet-watch"></a><span data-ttu-id="20a37-134">Vornehmen von Änderungen mit `dotnet watch`</span><span class="sxs-lookup"><span data-stu-id="20a37-134">Making changes with `dotnet watch`</span></span>

<span data-ttu-id="20a37-135">Stellen Sie sicher, dass `dotnet watch` ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="20a37-135">Make sure `dotnet watch` is running.</span></span>

<span data-ttu-id="20a37-136">Korrigieren Sie den Fehler in der `Product`-Methode von `MathController` so, dass das Produkt und nicht die Summe zurückgegeben wird.</span><span class="sxs-lookup"><span data-stu-id="20a37-136">Fix the bug in the `Product` method of the `MathController` so it returns the product and not the sum.</span></span>

```csharp
public static int Product(int a, int b)
{
  return a * b;
} 
```

<span data-ttu-id="20a37-137">Speichern Sie die Datei.</span><span class="sxs-lookup"><span data-stu-id="20a37-137">Save the file.</span></span> <span data-ttu-id="20a37-138">Die Konsolenausgabe zeigt Meldungen, die angeben, dass `dotnet watch` eine Dateiänderung erkannt und die App neu gestartet hat.</span><span class="sxs-lookup"><span data-stu-id="20a37-138">The console output will show messages indicating that `dotnet watch` detected a file change and restarted the app.</span></span>

<span data-ttu-id="20a37-139">Vergewissern Sie sich, dass `http://localhost:5000/api/math/product?a=4&b=5` das richtige Ergebnis zurückgibt.</span><span class="sxs-lookup"><span data-stu-id="20a37-139">Verify `http://localhost:5000/api/math/product?a=4&b=5` returns the correct result.</span></span>

## <a name="running-tests-using-dotnet-watch"></a><span data-ttu-id="20a37-140">Ausführen von Tests mit `dotnet watch`</span><span class="sxs-lookup"><span data-stu-id="20a37-140">Running tests using `dotnet watch`</span></span>

- <span data-ttu-id="20a37-141">Ändern Sie `Product`-Methode von `MathController` so, dass wieder die Summe zurückgegeben wird, und speichern Sie die Datei.</span><span class="sxs-lookup"><span data-stu-id="20a37-141">Change the `Product` method of the `MathController` back to returning the sum and save the file.</span></span>
- <span data-ttu-id="20a37-142">Navigieren Sie in einem Befehlsfenster zum Ordner `WebAppTests`.</span><span class="sxs-lookup"><span data-stu-id="20a37-142">In a command window, naviagate to the `WebAppTests` folder.</span></span>
- <span data-ttu-id="20a37-143">Ausführen von `dotnet restore`</span><span class="sxs-lookup"><span data-stu-id="20a37-143">Run `dotnet restore`</span></span>
- <span data-ttu-id="20a37-144">Führen Sie `dotnet watch test` aus.</span><span class="sxs-lookup"><span data-stu-id="20a37-144">Run `dotnet watch test`.</span></span> <span data-ttu-id="20a37-145">Sie erkennen in der Ausgabe, dass ein Test fehlgeschlagen ist und watcher auf Dateiänderungen wartet:</span><span class="sxs-lookup"><span data-stu-id="20a37-145">You see output indicating that a test failed and that watcher is waiting for file changes:</span></span>

 ```console
 Total tests: 2. Passed: 1. Failed: 1. Skipped: 0.
 Test Run Failed.
  ```
- <span data-ttu-id="20a37-146">Ändern Sie den Code der `Product`-Methode so, dass das Produkt zurückgegeben wird.</span><span class="sxs-lookup"><span data-stu-id="20a37-146">Fix the `Product` method code so it returns the product.</span></span> <span data-ttu-id="20a37-147">Speichern Sie die Datei.</span><span class="sxs-lookup"><span data-stu-id="20a37-147">Save the file.</span></span>

<span data-ttu-id="20a37-148">`dotnet watch` erkennt die Dateiänderung und führt die Tests erneut aus.</span><span class="sxs-lookup"><span data-stu-id="20a37-148">`dotnet watch` detects the file change and reruns the tests.</span></span> <span data-ttu-id="20a37-149">Die Konsolenausgabe zeigt, dass die Tests bestanden wurden.</span><span class="sxs-lookup"><span data-stu-id="20a37-149">The console output will show the tests passed.</span></span>

## <a name="dotnet-watch-in-github"></a><span data-ttu-id="20a37-150">dotnet-watch in GitHub</span><span class="sxs-lookup"><span data-stu-id="20a37-150">dotnet-watch in GitHub</span></span>

<span data-ttu-id="20a37-151">dotnet-watch ist Teil des GitHub-Repositorys [DotNetTools](https://github.com/aspnet/DotNetTools/tree/dev/src/Microsoft.DotNet.Watcher.Tools).</span><span class="sxs-lookup"><span data-stu-id="20a37-151">dotnet-watch is part of the GitHub [DotNetTools repository](https://github.com/aspnet/DotNetTools/tree/dev/src/Microsoft.DotNet.Watcher.Tools).</span></span>

<span data-ttu-id="20a37-152">Der Abschnitt [MSBuild](https://github.com/aspnet/DotNetTools/blob/dev/src/Microsoft.DotNet.Watcher.Tools/README.md#msbuild) in der [Infodatei zu dotnet-watch](https://github.com/aspnet/DotNetTools/blob/dev/src/Microsoft.DotNet.Watcher.Tools/README.md) erläutert, wie dotnet-watch in der überwachten MSBuild-Projektdatei konfiguriert werden kann.</span><span class="sxs-lookup"><span data-stu-id="20a37-152">The [MSBuild section](https://github.com/aspnet/DotNetTools/blob/dev/src/Microsoft.DotNet.Watcher.Tools/README.md#msbuild) of the [dotnet-watch ReadMe](https://github.com/aspnet/DotNetTools/blob/dev/src/Microsoft.DotNet.Watcher.Tools/README.md) explains how dotnet-watch can be configured from the MSBuild project file being watched.</span></span> <span data-ttu-id="20a37-153">Die [Infodatei zu dotnet-watch](https://github.com/aspnet/DotNetTools/blob/dev/src/Microsoft.DotNet.Watcher.Tools/README.md) enthält Informationen zu dotnet-watch, die in diesem Tutorial nicht behandelt werden.</span><span class="sxs-lookup"><span data-stu-id="20a37-153">The [dotnet-watch ReadMe](https://github.com/aspnet/DotNetTools/blob/dev/src/Microsoft.DotNet.Watcher.Tools/README.md) contains information on dotnet-watch not covered in this tutorial.</span></span>
