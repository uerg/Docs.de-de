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
ms.openlocfilehash: 013ca0d149c6415b5e6825aa5a48e93ae48f6728
ms.sourcegitcommit: 0063338c2e130409081bb60fcffa0c3f190cd46a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/12/2018
---
# <a name="razor-file-cshtml-compilation-in-aspnet-core"></a>Kompilieren einer Razor-Datei (CSHTML) in ASP.NET Core

Von [Rick Anderson](https://twitter.com/RickAndMSFT)

Razor-Ansichten werden zur Laufzeit kompiliert, wenn die Ansicht aufgerufen wird. Mit ASP.NET Core 2.1.0 oder höher können Sie Ansichten zum Zeitpunkt der Erstellung und Veröffentlichung mithilfe vom [Razor SDK](/aspnetcore/mvc/razor-pages/sdk) kompilieren. Mithilfe des Tools für die Vorkompilierung können Ansichten in ASP.NET Core 1.1 und ASP.NET Core 2.0 optional zur Veröffentlichung oder Bereitstellung mit der App kompiliert werden. 



Was vor der Kompilierung beachtet werden muss:

* Das Vorkompilieren von Ansichten führt dazu, dass ein kleineres Bundle veröffentlicht wird und der Start schneller erfolgt.
* Sie können Razor-Dateien nicht mehr bearbeiten, nachdem Sie Ansichten vorkompiliert haben. Die bearbeiteten Ansichten sind im veröffentlichten Bundle nicht vorhanden. 

So stellen Sie vorkompilierte Ansichten bereit:

# <a name="aspnet-core-21tabaspnetcore21"></a>[ASP.NET Core 2.1](#tab/aspnetcore21/)
Die Kompilierung zur Erstellung und Veröffentlichung von Razor-Dateien wird standardmäßig vom Razor SDK aktiviert. Das Bearbeiten von Razor-Dateien, nachdem sie aktualisiert wurden, wird zum Zeitpunkt der Erstellung unterstützt. Standardmäßig wird nur die kompilierte *Views.dll*, ohne CSHTML-Dateien, mit Ihrer Anwendung bereitgestellt. 
    
> [!IMPORTANT]
> Das Razor SDK ist nur wirksam, wenn keine für die Vorkompilierung spezifischen Eigenschaften in Ihrer Projektdatei festgelegt sind. Das Razor SDK wird beispielsweise deaktiviert, wenn Sie in Ihrer *CSPROJ*-Datei `MvcRazorCompileOnPublish` festlegen.

# <a name="aspnet-core-20tabaspnetcore20"></a>[ASP.NET Core 2.0](#tab/aspnetcore20/)

Wenn Ihr Projekt .NET Framework als Ziel verwendet, beziehen Sie einen Paketverweis auf [Microsoft.AspNetCore.Mvc.Razor.ViewCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.ViewCompilation/) mit ein:

```xml
<PackageReference Include="Microsoft.AspNetCore.Mvc.Razor.ViewCompilation" Version="2.0.0" PrivateAssets="All" />
```

Wenn Ihr Paket für .NET Core gedacht ist, sind keine Änderungen erforderlich.

Die ASP.NET Core 2.x-Projektvorlagen legen `MvcRazorCompileOnPublish` standardmäßig implizit auf `true` fest. Dies bedeutet, dass dieser Knoten sicher aus der *CSPROJ*-Datei entfernt werden kann.
    
> [!IMPORTANT]
> Die Vorkompilierung von Razor-Ansichten steht beim Durchführen einer [eigenständigen Bereitstellung (Self-Contained Deployment, SCD)](/dotnet/core/deploying/#self-contained-deployments-scd) in ASP.NET Core 2.0 nicht zur Verfügung. 

Bereiten Sie die App mit dem [Veröffentlichungsbefehl der .NET Core-CLI](/dotnet/core/tools/dotnet-publish) für eine [Framework-abhängige Bereitstellung](/dotnet/core/deploying/#framework-dependent-deployments-fdd) vor. Führen Sie z. B. den folgenden Befehl auf der Stammebene des Projekts aus:

```console
dotnet publish -c Release
```

Die Datei *<projektname>.PrecompiledViews.dll*, die die kompilierten Razor-Ansichten enthält, wird erzeugt, wenn die Vorkompilierung erfolgreich abgeschlossen wurde. Der unten stehende Screenshot zeigt beispielsweise den Inhalt von *Index.cshtml* innerhalb von *WebApplication1.PrecompiledViews.dll*:

![Razor-Ansichten in einer DLL](view-compilation/_static/razor-views-in-dll.png)

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

Legen Sie `MvcRazorCompileOnPublish` auf `true` fest, und ziehen Sie einen Paketverweis auf `Microsoft.AspNetCore.Mvc.Razor.ViewCompilation` ein. Das folgende *CSPROJ*-Beispiel veranschaulicht diese Einstellungen:

[!code-xml[](view-compilation/sample/MvcRazorCompileOnPublish.csproj?highlight=5,12)]

---

