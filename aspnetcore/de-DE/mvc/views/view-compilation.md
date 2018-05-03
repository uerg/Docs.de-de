---
title: Kompilierung und Vorkompilierung einer Razor-Ansicht in ASP.NET Core
author: rick-anderson
description: Erfahren Sie, wie Sie die Kompilierung und Vorkompilierung einer MVC-Razor-Ansicht in ASP.NET Core-Apps aktivieren.
manager: wpickett
ms.author: riande
ms.date: 12/13/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: mvc/views/view-compilation
ms.openlocfilehash: 5d971645106a79497a9902063c7774dc6d546395
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
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

#### <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)
Wenn Ihr Projekt .NET Framework als Ziel verwendet, beziehen Sie einen Paketverweis auf [Microsoft.AspNetCore.Mvc.Razor.ViewCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.ViewCompilation/) mit ein:

```xml
<PackageReference Include="Microsoft.AspNetCore.Mvc.Razor.ViewCompilation" Version="2.0.0" PrivateAssets="All" />
```

Wenn Ihr Paket für .NET Core gedacht ist, sind keine Änderungen erforderlich.

Die ASP.NET Core 2.x-Projektvorlagen legen `MvcRazorCompileOnPublish` standardmäßig implizit auf `true` fest. Dies bedeutet, dass dieser Knoten sicher aus der *CSPROJ*-Datei entfernt werden kann. Wenn Sie das explizite Festlegen vorziehen, können Sie die `MvcRazorCompileOnPublish`-Eigenschaft auch auf `true` festlegen. Das folgende *CSPROJ*-Beispiel veranschaulicht diese Eigenschaft:

[!code-xml[](view-compilation/sample/MvcRazorCompileOnPublish2.csproj?highlight=5)]

#### <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)
Legen Sie `MvcRazorCompileOnPublish` auf `true` fest, und ziehen Sie einen Paketverweis auf `Microsoft.AspNetCore.Mvc.Razor.ViewCompilation` ein. Das folgende *CSPROJ*-Beispiel veranschaulicht diese Einstellungen:

[!code-xml[](view-compilation/sample/MvcRazorCompileOnPublish.csproj?highlight=5,12)]

Bereiten Sie die App mit dem [Veröffentlichungsbefehl der .NET Core-CLI](/dotnet/core/tools/dotnet-publish) für eine [Framework-abhängige Bereitstellung](/dotnet/core/deploying/#framework-dependent-deployments-fdd) vor. Führen Sie z. B. den folgenden Befehl auf der Stammebene des Projekts aus:

```console
dotnet publish -c Release
```

Die Datei *<projektname>.PrecompiledViews.dll*, die die kompilierten Razor-Ansichten enthält, wird erzeugt, wenn die Vorkompilierung erfolgreich abgeschlossen wurde. Der unten stehende Screenshot zeigt beispielsweise den Inhalt von *Index.cshtml* innerhalb von *WebApplication1.PrecompiledViews.dll*:

![Razor-Ansichten in einer DLL](view-compilation/_static/razor-views-in-dll.png)
