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
ms.openlocfilehash: 2ddcbfc30a839ed8dd72a632644bf73dcea777ac
ms.sourcegitcommit: b02db6da115e55140da91b67355aaf56aae1703f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/11/2017
---
# <a name="developing-aspnet-core-apps-using-dotnet-watch"></a>Entwickeln von ASP.NET Core-Apps mit dotnet watch


Von [Rick Anderson](https://twitter.com/RickAndMSFT) und [Victor Hurdugaci](https://twitter.com/victorhurdugaci)

`dotnet watch` ist ein Tool, das den Befehl `dotnet` ausführt, wenn sich Quelldateien ändern. Eine Dateiänderung kann beispielsweise eine Kompilierung, Tests oder Bereitstellung auslösen.

In diesem Tutorial verwenden wir eine vorhandene Web-API-App mit zwei Endpunkten: der eine gibt eine Summe, der andere ein Produkt zurück. Die Produktmethode enthält einen Fehler, den wir im Rahmen dieses Tutorials beheben müssen.

Laden Sie die [Beispiel-App](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/dotnet-watch/sample) herunter. Sie enthält zwei Projekte: `WebApp` (eine Web-App) und `WebAppTests` (Komponententests für die Web-App).

Navigieren Sie in einer Konsole zum Ordner „WebApp“, und führen Sie die folgenden Befehle aus:

- `dotnet restore`
- `dotnet run`

Die Konsolenausgabe zeigt Meldungen ähnlich der folgenden (die angibt, dass die App ausgeführt wird und auf Anforderungen wartet):

```console
$ dotnet run
Hosting environment: Production
Content root path: C:/Docs/aspnetcore/tutorials/dotnet-watch/sample/WebApp
Now listening on: http://localhost:5000
Application started. Press Ctrl+C to shut down.
```

Wenn Sie in einem Webbrowser zu `http://localhost:5000/api/math/sum?a=4&b=5` wechseln, sollten Sie das Ergebnis `9` sehen.

Navigieren Sie zur Produkt-API (`http://localhost:5000/api/math/product?a=4&b=5`), die `9` und nicht wie erwartet `20` ausgibt. Dies korrigieren wir später im Tutorial.

## <a name="add-dotnet-watch-to-a-project"></a>Hinzufügen von `dotnet watch` zu einem Projekt

- Fügen Sie `Microsoft.DotNet.Watcher.Tools` der Datei *.csproj* hinzu:
 ```xml
 <ItemGroup>
   <DotNetCliToolReference Include="Microsoft.DotNet.Watcher.Tools" Version="2.0.0" />
 </ItemGroup> 
 ```

- Führen Sie `dotnet restore` aus.

## <a name="running-dotnet-commands-using-dotnet-watch"></a>Ausführen von `dotnet`-Befehlen mit `dotnet watch`

Alle `dotnet`-Befehle können mit `dotnet watch` ausgeführt werden, wie z.B.:

| Befehl | Befehl mit watch |
| ---- | ----- |
| dotnet run | dotnet watch run |
| dotnet run -f net451 | dotnet watch run -f net451 |
| dotnet run -f net451 -- --arg1 | dotnet watch run -f net451 -- --arg1 |
| dotnet test | dotnet watch test |

Führen Sie `dotnet watch run` im Ordner `WebApp` aus. Die Konsolenausgabe gibt an, dass `watch` gestartet wurde.

## <a name="making-changes-with-dotnet-watch"></a>Vornehmen von Änderungen mit `dotnet watch`

Stellen Sie sicher, dass `dotnet watch` ausgeführt wird.

Korrigieren Sie den Fehler in der `Product`-Methode von `MathController` so, dass das Produkt und nicht die Summe zurückgegeben wird.

```csharp
public static int Product(int a, int b)
{
  return a * b;
} 
```

Speichern Sie die Datei. Die Konsolenausgabe zeigt Meldungen, die angeben, dass `dotnet watch` eine Dateiänderung erkannt und die App neu gestartet hat.

Vergewissern Sie sich, dass `http://localhost:5000/api/math/product?a=4&b=5` das richtige Ergebnis zurückgibt.

## <a name="running-tests-using-dotnet-watch"></a>Ausführen von Tests mit `dotnet watch`

- Ändern Sie `Product`-Methode von `MathController` so, dass wieder die Summe zurückgegeben wird, und speichern Sie die Datei.
- Navigieren Sie in einem Befehlsfenster zum Ordner `WebAppTests`.
- Ausführen von `dotnet restore`
- Führen Sie `dotnet watch test` aus. Sie erkennen in der Ausgabe, dass ein Test fehlgeschlagen ist und watcher auf Dateiänderungen wartet:

 ```console
 Total tests: 2. Passed: 1. Failed: 1. Skipped: 0.
 Test Run Failed.
  ```
- Ändern Sie den Code der `Product`-Methode so, dass das Produkt zurückgegeben wird. Speichern Sie die Datei.

`dotnet watch` erkennt die Dateiänderung und führt die Tests erneut aus. Die Konsolenausgabe zeigt, dass die Tests bestanden wurden.

## <a name="dotnet-watch-in-github"></a>dotnet-watch in GitHub

dotnet-watch ist Teil des GitHub-Repositorys [DotNetTools](https://github.com/aspnet/DotNetTools/tree/dev/src/Microsoft.DotNet.Watcher.Tools).

Der Abschnitt [MSBuild](https://github.com/aspnet/DotNetTools/blob/dev/src/Microsoft.DotNet.Watcher.Tools/README.md#msbuild) in der [Infodatei zu dotnet-watch](https://github.com/aspnet/DotNetTools/blob/dev/src/Microsoft.DotNet.Watcher.Tools/README.md) erläutert, wie dotnet-watch in der überwachten MSBuild-Projektdatei konfiguriert werden kann. Die [Infodatei zu dotnet-watch](https://github.com/aspnet/DotNetTools/blob/dev/src/Microsoft.DotNet.Watcher.Tools/README.md) enthält Informationen zu dotnet-watch, die in diesem Tutorial nicht behandelt werden.
