---
title: Kompilierung und Vorkompilierung einer Razor-Ansicht
author: rick-anderson
description: "Dies ist ein Referenzdokument, in dem erklärt wird, wie Sie die Kompilierung und Vorkompilierung von MVC-Razor-Ansichten in ASP.NET Core-Anwendungen aktivieren."
manager: wpickett
ms.author: riande
ms.date: 12/13/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: mvc/views/view-compilation
ms.openlocfilehash: bd3f4470035b0375fc79aa7caa73b60ba6fc4f53
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2018
---
# <a name="razor-view-compilation-and-precompilation-in-aspnet-core"></a>Kompilierung und Vorkompilierung einer Razor-Ansicht in ASP.NET Core

Von [Rick Anderson](https://twitter.com/RickAndMSFT)

Razor-Ansichten werden zur Laufzeit kompiliert, wenn die Ansicht aufgerufen wird. ASP.NET Core 1.1.0 und höher kann optional Razor-Ansichten kompilieren und diese mit der App bereitstellen &mdash; dieser Prozess wird als Vorkompilierung bezeichnet. Die ASP.NET Core 2.x-Projektvorlagen aktivieren die Vorkompilierung standardmäßig.

> [!IMPORTANT]
> Die Vorkompilierung von Razor-Ansichten steht aktuell beim Durchführen einer [eigenständigen Bereitstellung (Self-Contained Deployment, SCD)](/dotnet/core/deploying/#self-contained-deployments-scd) in ASP.NET Core 2.0 nicht zur Verfügung. Mit Release 2.1 wird dieses Feature für SCDs verfügbar. Weitere Informationen finden Sie unter [View compilation fails when cross-compiling for Linux on Windows (Die Ansichtskompilierung schlägt bei der übergreifenden Kompilierung für Linux unter Windows fehl)](https://github.com/aspnet/MvcPrecompilation/issues/102).

Was vor der Kompilierung beachtet werden muss:

* Das Vorkompilieren von Ansichten führt dazu, dass ein kleineres Bundle veröffentlicht wird und der Start schneller erfolgt.
* Sie können Razor-Dateien nicht mehr bearbeiten, nachdem Sie Ansichten vorkompiliert haben. Die bearbeiteten Ansichten sind im veröffentlichten Bundle nicht vorhanden. 

So stellen Sie vorkompilierte Ansichten bereit:

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Wenn Ihr Projekt .NET Framework als Ziel verwendet, beziehen Sie einen Paketverweis auf [Microsoft.AspNetCore.Mvc.Razor.ViewCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.ViewCompilation/) mit ein:

```xml
<PackageReference Include="Microsoft.AspNetCore.Mvc.Razor.ViewCompilation" Version="2.0.0" PrivateAssets="All" />
```

Wenn Ihr Paket für .NET Core gedacht ist, sind keine Änderungen erforderlich.

Die ASP.NET Core 2.x-Projektvorlagen legen `MvcRazorCompileOnPublish` standardmäßig implizit auf `true` fest. Dies bedeutet, dass dieser Knoten sicher aus der *CSPROJ*-Datei entfernt werden kann. Wenn Sie das explizite Festlegen vorziehen, können Sie die `MvcRazorCompileOnPublish`-Eigenschaft auch auf `true` festlegen. Das folgende *CSPROJ*-Beispiel veranschaulicht diese Eigenschaft:

[!code-xml[Main](view-compilation\sample\MvcRazorCompileOnPublish2.csproj?highlight=5)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Legen Sie `MvcRazorCompileOnPublish` auf `true` fest, und ziehen Sie einen Paketverweis auf `Microsoft.AspNetCore.Mvc.Razor.ViewCompilation` ein. Das folgende *CSPROJ*-Beispiel veranschaulicht diese Einstellungen:

[!code-xml[Main](view-compilation\sample\MvcRazorCompileOnPublish.csproj?highlight=5,12)]

---

Bereiten Sie die App auf eine [frameworkabhängige Bereitstellung](/dotnet/core/deploying/#framework-dependent-deployments-fdd) vor, indem Sie einen Befehl wie den folgenden am Projektstamm ausführen:

```console
dotnet publish -c Release
```

Die Datei *<projektname>.PrecompiledViews.dll*, die die kompilierten Razor-Ansichten enthält, wird erzeugt, wenn die Vorkompilierung erfolgreich abgeschlossen wurde. Der unten stehende Screenshot zeigt beispielsweise den Inhalt von *Index.cshtml* innerhalb von *WebApplication1.PrecompiledViews.dll*:

![Razor-Ansichten in einer DLL](view-compilation/_static/razor-views-in-dll.png)
