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
ms.openlocfilehash: 30e0d07bdfbd16a475e03c1a21cdd10220bd1630
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/11/2017
---
# <a name="developing-aspnet-core-apps-using-dotnet-watch"></a><span data-ttu-id="b4a82-104">Entwickeln von ASP.NET Core-Apps mit dotnet watch</span><span class="sxs-lookup"><span data-stu-id="b4a82-104">Developing ASP.NET Core apps using dotnet watch</span></span>


<span data-ttu-id="b4a82-105">Von [Rick Anderson](https://twitter.com/RickAndMSFT) und [Victor Hurdugaci](https://twitter.com/victorhurdugaci)</span><span class="sxs-lookup"><span data-stu-id="b4a82-105">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Victor Hurdugaci](https://twitter.com/victorhurdugaci)</span></span>

<span data-ttu-id="b4a82-106">`dotnet watch` ist ein Tool, das den Befehl `dotnet` ausführt, wenn sich Quelldateien ändern.</span><span class="sxs-lookup"><span data-stu-id="b4a82-106">`dotnet watch` is a tool that runs a `dotnet` command when source files change.</span></span> <span data-ttu-id="b4a82-107">Eine Dateiänderung kann beispielsweise eine Kompilierung, Tests oder Bereitstellung auslösen.</span><span class="sxs-lookup"><span data-stu-id="b4a82-107">For example, a file change can trigger compilation, tests, or deployment.</span></span>

<span data-ttu-id="b4a82-108">In diesem Tutorial verwenden wir eine vorhandene Web-API-App mit zwei Endpunkten: der eine gibt eine Summe, der andere ein Produkt zurück.</span><span class="sxs-lookup"><span data-stu-id="b4a82-108">In this tutorial we use an existing Web API app with two endpoints: one that returns a sum and one that returns a product.</span></span> <span data-ttu-id="b4a82-109">Die Produktmethode enthält einen Fehler, den wir im Rahmen dieses Tutorials beheben müssen.</span><span class="sxs-lookup"><span data-stu-id="b4a82-109">The product method contains a bug that we'll fix as part of this tutorial.</span></span>

<span data-ttu-id="b4a82-110">Laden Sie die [Beispiel-App](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/dotnet-watch/sample) herunter.</span><span class="sxs-lookup"><span data-stu-id="b4a82-110">Download the [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/dotnet-watch/sample).</span></span> <span data-ttu-id="b4a82-111">Sie enthält zwei Projekte: `WebApp` (eine Web-App) und `WebAppTests` (Komponententests für die Web-App).</span><span class="sxs-lookup"><span data-stu-id="b4a82-111">It contains two projects, `WebApp` (a web app) and `WebAppTests` (unit tests for the web app).</span></span>

<span data-ttu-id="b4a82-112">Navigieren Sie in einer Konsole zum Ordner „WebApp“, und führen Sie die folgenden Befehle aus:</span><span class="sxs-lookup"><span data-stu-id="b4a82-112">In a console, navigate to the WebApp folder and run the following commands:</span></span>

- `dotnet restore`
- `dotnet run`

<span data-ttu-id="b4a82-113">Die Konsolenausgabe zeigt Meldungen ähnlich der folgenden (die angibt, dass die App ausgeführt wird und auf Anforderungen wartet):</span><span class="sxs-lookup"><span data-stu-id="b4a82-113">The console output will show messages similar to the following (indicating that the app is running and waiting for requests):</span></span>

```console
$ dotnet run
Hosting environment: Production
Content root path: C:/Docs/aspnetcore/tutorials/dotnet-watch/sample/WebApp
Now listening on: http://localhost:5000
Application started. Press Ctrl+C to shut down.
```

<span data-ttu-id="b4a82-114">Wenn Sie in einem Webbrowser zu `http://localhost:5000/api/math/sum?a=4&b=5` wechseln, sollten Sie das Ergebnis `9` sehen.</span><span class="sxs-lookup"><span data-stu-id="b4a82-114">In a web browser, navigate to `http://localhost:5000/api/math/sum?a=4&b=5`, you should see the result `9`.</span></span>

<span data-ttu-id="b4a82-115">Navigieren Sie zur Produkt-API (`http://localhost:5000/api/math/product?a=4&b=5`), die `9` und nicht wie erwartet `20` ausgibt.</span><span class="sxs-lookup"><span data-stu-id="b4a82-115">Navigate to the product API (`http://localhost:5000/api/math/product?a=4&b=5`), it returns `9`, not `20` as you'd expect.</span></span> <span data-ttu-id="b4a82-116">Dies korrigieren wir später im Tutorial.</span><span class="sxs-lookup"><span data-stu-id="b4a82-116">We'll fix that later in the tutorial.</span></span>

## <a name="add-dotnet-watch-to-a-project"></a><span data-ttu-id="b4a82-117">Hinzufügen von `dotnet watch` zu einem Projekt</span><span class="sxs-lookup"><span data-stu-id="b4a82-117">Add `dotnet watch` to a project</span></span>

- <span data-ttu-id="b4a82-118">Fügen Sie `Microsoft.DotNet.Watcher.Tools` der Datei *.csproj* hinzu:</span><span class="sxs-lookup"><span data-stu-id="b4a82-118">Add `Microsoft.DotNet.Watcher.Tools` to the *.csproj* file:</span></span>
 ```xml
 <ItemGroup>
   <DotNetCliToolReference Include="Microsoft.DotNet.Watcher.Tools" Version="1.0.0" />
 </ItemGroup> 
 ```

- <span data-ttu-id="b4a82-119">Führen Sie `dotnet restore` aus.</span><span class="sxs-lookup"><span data-stu-id="b4a82-119">Run `dotnet restore`.</span></span>

## <a name="running-dotnet-commands-using-dotnet-watch"></a><span data-ttu-id="b4a82-120">Ausführen von `dotnet`-Befehlen mit `dotnet watch`</span><span class="sxs-lookup"><span data-stu-id="b4a82-120">Running `dotnet` commands using `dotnet watch`</span></span>

<span data-ttu-id="b4a82-121">Alle `dotnet`-Befehle können mit `dotnet watch` ausgeführt werden, wie z.B.:</span><span class="sxs-lookup"><span data-stu-id="b4a82-121">Any `dotnet` command can be run with `dotnet watch`, for example:</span></span>

| <span data-ttu-id="b4a82-122">Befehl</span><span class="sxs-lookup"><span data-stu-id="b4a82-122">Command</span></span> | <span data-ttu-id="b4a82-123">Befehl mit watch</span><span class="sxs-lookup"><span data-stu-id="b4a82-123">Command with watch</span></span> |
| ---- | ----- |
| <span data-ttu-id="b4a82-124">dotnet run</span><span class="sxs-lookup"><span data-stu-id="b4a82-124">dotnet run</span></span> | <span data-ttu-id="b4a82-125">dotnet watch run</span><span class="sxs-lookup"><span data-stu-id="b4a82-125">dotnet watch run</span></span> |
| <span data-ttu-id="b4a82-126">dotnet run -f net451</span><span class="sxs-lookup"><span data-stu-id="b4a82-126">dotnet run -f net451</span></span> | <span data-ttu-id="b4a82-127">dotnet watch run -f net451</span><span class="sxs-lookup"><span data-stu-id="b4a82-127">dotnet watch run -f net451</span></span> |
| <span data-ttu-id="b4a82-128">dotnet run -f net451 -- --arg1</span><span class="sxs-lookup"><span data-stu-id="b4a82-128">dotnet run -f net451 -- --arg1</span></span> | <span data-ttu-id="b4a82-129">dotnet watch run -f net451 -- --arg1</span><span class="sxs-lookup"><span data-stu-id="b4a82-129">dotnet watch run -f net451 -- --arg1</span></span> |
| <span data-ttu-id="b4a82-130">dotnet test</span><span class="sxs-lookup"><span data-stu-id="b4a82-130">dotnet test</span></span> | <span data-ttu-id="b4a82-131">dotnet watch test</span><span class="sxs-lookup"><span data-stu-id="b4a82-131">dotnet watch test</span></span> |

<span data-ttu-id="b4a82-132">Führen Sie `dotnet watch run` im Ordner `WebApp` aus.</span><span class="sxs-lookup"><span data-stu-id="b4a82-132">Run `dotnet watch run` in the `WebApp` folder.</span></span> <span data-ttu-id="b4a82-133">Die Konsolenausgabe gibt an, dass `watch` gestartet wurde.</span><span class="sxs-lookup"><span data-stu-id="b4a82-133">The console output will indicate `watch` has started.</span></span>

## <a name="making-changes-with-dotnet-watch"></a><span data-ttu-id="b4a82-134">Vornehmen von Änderungen mit `dotnet watch`</span><span class="sxs-lookup"><span data-stu-id="b4a82-134">Making changes with `dotnet watch`</span></span>

<span data-ttu-id="b4a82-135">Stellen Sie sicher, dass `dotnet watch` ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="b4a82-135">Make sure `dotnet watch` is running.</span></span>

<span data-ttu-id="b4a82-136">Korrigieren Sie den Fehler in der `Product`-Methode von `MathController` so, dass das Produkt und nicht die Summe zurückgegeben wird.</span><span class="sxs-lookup"><span data-stu-id="b4a82-136">Fix the bug in the `Product` method of the `MathController` so it returns the product and not the sum.</span></span>

```csharp
public static int Product(int a, int b)
{
  return a * b;
} 
```

<span data-ttu-id="b4a82-137">Speichern Sie die Datei.</span><span class="sxs-lookup"><span data-stu-id="b4a82-137">Save the file.</span></span> <span data-ttu-id="b4a82-138">Die Konsolenausgabe zeigt Meldungen, die angeben, dass `dotnet watch` eine Dateiänderung erkannt und die App neu gestartet hat.</span><span class="sxs-lookup"><span data-stu-id="b4a82-138">The console output will show messages indicating that `dotnet watch` detected a file change and restarted the app.</span></span>

<span data-ttu-id="b4a82-139">Vergewissern Sie sich, dass `http://localhost:5000/api/math/product?a=4&b=5` das richtige Ergebnis zurückgibt.</span><span class="sxs-lookup"><span data-stu-id="b4a82-139">Verify `http://localhost:5000/api/math/product?a=4&b=5` returns the correct result.</span></span>

## <a name="running-tests-using-dotnet-watch"></a><span data-ttu-id="b4a82-140">Ausführen von Tests mit `dotnet watch`</span><span class="sxs-lookup"><span data-stu-id="b4a82-140">Running tests using `dotnet watch`</span></span>

- <span data-ttu-id="b4a82-141">Ändern Sie `Product`-Methode von `MathController` so, dass wieder die Summe zurückgegeben wird, und speichern Sie die Datei.</span><span class="sxs-lookup"><span data-stu-id="b4a82-141">Change the `Product` method of the `MathController` back to returning the sum and save the file.</span></span>
- <span data-ttu-id="b4a82-142">Navigieren Sie in einem Befehlsfenster zum Ordner `WebAppTests`.</span><span class="sxs-lookup"><span data-stu-id="b4a82-142">In a command window, naviagate to the `WebAppTests` folder.</span></span>
- <span data-ttu-id="b4a82-143">Ausführen von `dotnet restore`</span><span class="sxs-lookup"><span data-stu-id="b4a82-143">Run `dotnet restore`</span></span>
- <span data-ttu-id="b4a82-144">Führen Sie `dotnet watch test` aus.</span><span class="sxs-lookup"><span data-stu-id="b4a82-144">Run `dotnet watch test`.</span></span> <span data-ttu-id="b4a82-145">Sie erkennen in der Ausgabe, dass ein Test fehlgeschlagen ist und watcher auf Dateiänderungen wartet:</span><span class="sxs-lookup"><span data-stu-id="b4a82-145">You see output indicating that a test failed and that watcher is waiting for file changes:</span></span>

 ```console
 Total tests: 2. Passed: 1. Failed: 1. Skipped: 0.
 Test Run Failed.
  ```
- <span data-ttu-id="b4a82-146">Ändern Sie den Code der `Product`-Methode so, dass das Produkt zurückgegeben wird.</span><span class="sxs-lookup"><span data-stu-id="b4a82-146">Fix the `Product` method code so it returns the product.</span></span> <span data-ttu-id="b4a82-147">Speichern Sie die Datei.</span><span class="sxs-lookup"><span data-stu-id="b4a82-147">Save the file.</span></span>

<span data-ttu-id="b4a82-148">`dotnet watch` erkennt die Dateiänderung und führt die Tests erneut aus.</span><span class="sxs-lookup"><span data-stu-id="b4a82-148">`dotnet watch` detects the file change and reruns the tests.</span></span> <span data-ttu-id="b4a82-149">Die Konsolenausgabe zeigt, dass die Tests bestanden wurden.</span><span class="sxs-lookup"><span data-stu-id="b4a82-149">The console output will show the tests passed.</span></span>

## <a name="dotnet-watch-in-github"></a><span data-ttu-id="b4a82-150">dotnet-watch in GitHub</span><span class="sxs-lookup"><span data-stu-id="b4a82-150">dotnet-watch in GitHub</span></span>

<span data-ttu-id="b4a82-151">dotnet-watch ist Teil des GitHub-Repositorys [DotNetTools](https://github.com/aspnet/DotNetTools/tree/dev/src/Microsoft.DotNet.Watcher.Tools).</span><span class="sxs-lookup"><span data-stu-id="b4a82-151">dotnet-watch is part of the GitHub [DotNetTools repository](https://github.com/aspnet/DotNetTools/tree/dev/src/Microsoft.DotNet.Watcher.Tools).</span></span>

<span data-ttu-id="b4a82-152">Der Abschnitt [MSBuild](https://github.com/aspnet/DotNetTools/blob/dev/src/Microsoft.DotNet.Watcher.Tools/README.md#msbuild) in der [Infodatei zu dotnet-watch](https://github.com/aspnet/DotNetTools/blob/dev/src/Microsoft.DotNet.Watcher.Tools/README.md) erläutert, wie dotnet-watch in der überwachten MSBuild-Projektdatei konfiguriert werden kann.</span><span class="sxs-lookup"><span data-stu-id="b4a82-152">The [MSBuild section](https://github.com/aspnet/DotNetTools/blob/dev/src/Microsoft.DotNet.Watcher.Tools/README.md#msbuild) of the [dotnet-watch ReadMe](https://github.com/aspnet/DotNetTools/blob/dev/src/Microsoft.DotNet.Watcher.Tools/README.md) explains how dotnet-watch can be configured from the MSBuild project file being watched.</span></span> <span data-ttu-id="b4a82-153">Die [Infodatei zu dotnet-watch](https://github.com/aspnet/DotNetTools/blob/dev/src/Microsoft.DotNet.Watcher.Tools/README.md) enthält Informationen zu dotnet-watch, die in diesem Tutorial nicht behandelt werden.</span><span class="sxs-lookup"><span data-stu-id="b4a82-153">The [dotnet-watch ReadMe](https://github.com/aspnet/DotNetTools/blob/dev/src/Microsoft.DotNet.Watcher.Tools/README.md) contains information on dotnet-watch not covered in this tutorial.</span></span>
