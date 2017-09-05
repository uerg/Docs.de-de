---
title: Razor-Ansicht-Kompilierung und Vorkompilierung
author: rick-anderson
description: "Ein Verweis Dokument erläutert, wie die MVC Razor-Ansicht-Kompilierung und Vorkompilierung in ASP.NET Core-Anwendungen zu ermöglichen."
keywords: ASP.NET Core, Razor-Ansicht-Kompilierung, Razor vor der Kompilierung, Razor Vorkompilierung
ms.author: riande
manager: wpickett
ms.date: 08/16/2017
ms.topic: article
ms.assetid: ab4705b7-1638-1638-bc97-ea7f292fe92a
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/views/view-compilation
ms.openlocfilehash: 1395717341bfcf5441b78633ca3957630ae5d899
ms.sourcegitcommit: bd05f7ea8f87ad076ef6e8b704698ebcba5ca80c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/25/2017
---
# <a name="razor-view-compilation-and-precompilation-in-aspnet-core"></a>Razor-Ansicht-Kompilierung und Vorkompilierung in ASP.NET Core

Durch [Rick Anderson](https://twitter.com/RickAndMSFT)

Razor-Ansichten werden zur Laufzeit kompiliert, wenn die Ansicht aufgerufen wird. ASP.NET Core 1.1.0 und höheren Versionen kann optional kompilieren Razor-Ansichten und diese mit der app bereitstellen &mdash; genannten Vorkompilierung. Die Projektvorlagen für ASP.NET Core 2.x aktiviert Vorkompilierung standardmäßig.

> [!NOTE]
> Razor-Ansicht Vorkompilierung ist nicht verfügbar, wenn auf diese Weise eine [Self-Contained Bereitstellung](https://docs.microsoft.com/dotnet/core/deploying/#self-contained-deployments-scd) in ASP.NET Core-Version 2.0.0 und früher.

Vorkompilierung Aspekte:

* Vorkompilieren von Ansichten führt zu einer kleineren veröffentlichte Paket und schneller gestartet.
* Razor-Dateien kann nicht bearbeitet werden, nachdem Sie Ansichten vorkompilieren. Die bearbeiteten Sichten wird nicht in das veröffentlichte Paket vorhanden sein. 

Vorkompilierte Sichten bereitstellen:

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Wenn Ihr Projekt auf .NET Framework abzielt, enthalten einen Verweis Paket auf `Microsoft.AspNetCore.Mvc.Razor.ViewCompilation`:

```xml
<PackageReference Include="Microsoft.AspNetCore.Mvc.Razor.ViewCompilation" Version="2.0.0" PrivateAssets="All" />
```

Wenn das Projekt .NET Core als Ziel verwendet werden, sind keine Änderungen erforderlich.

Legen Sie die Projektvorlagen von ASP.NET Core 2.x implizit `MvcRazorCompileOnPublish` auf `true` standardmäßig, d. h. dieser Knoten kann sicher entfernt werden, aus der *csproj* Datei. Wenn Sie es vorziehen, die explizit sein, besteht keine Schäden in der Setting der `MvcRazorCompileOnPublish` Eigenschaft `true`. Die folgenden *csproj* Beispiel beschreibt diese Einstellung:

[!code-xml[Main](view-compilation\sample\MvcRazorCompileOnPublish2.csproj?highlight=5)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Legen Sie `MvcRazorCompileOnPublish` auf `true`, und schließen Sie einen Paket Verweis auf `Microsoft.AspNetCore.Mvc.Razor.ViewCompilation`. Die folgenden *csproj* Beispiel beschreibt diese Einstellungen:

[!code-xml[Main](view-compilation\sample\MvcRazorCompileOnPublish.csproj?highlight=5,12)]

---