---
title: Kompilierung und Vorkompilierung einer Razor-Datei in ASP.NET Core
author: rick-anderson
description: Erfahren Sie mehr über die Vorteile der Vorkompilierung von Razor-Dateien und über das Erreichen der Vorkompilierung einer Razor-Datei in einer ASP.NET Core-App.
monikerRange: '>= aspnetcore-1.1'
ms.author: riande
ms.custom: mvc
ms.date: 05/17/2018
uid: mvc/views/view-compilation
ms.openlocfilehash: 9355d467ca819ea8c6292963b31367ad5ca36d55
ms.sourcegitcommit: 661d30492d5ef7bbca4f7e709f40d8f3309d2dac
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/10/2018
ms.locfileid: "37938536"
---
# <a name="razor-file-compilation-in-aspnet-core"></a>Kompilieren einer Razor-Datei in ASP.NET Core

Von [Rick Anderson](https://twitter.com/RickAndMSFT)

::: moniker range="= aspnetcore-1.1"
Eine Razor-Datei wird zur Laufzeit kompiliert, wenn die zugeordnete MVC-Ansicht aufgerufen wird. Die Veröffentlichung von Razor-Dateien zum Zeitpunkt der Erstellung wird nicht unterstützt. Razor-Dateien können optional zur Veröffentlichung kompiliert und mit der App über das Vorkompilierungstool bereitgestellt werden.
::: moniker-end
::: moniker range="= aspnetcore-2.0"
Eine Razor-Datei wird zur Laufzeit kompiliert, wenn die zugeordnete Razor-Seite oder MVC-Ansicht aufgerufen wird. Die Veröffentlichung von Razor-Dateien zum Zeitpunkt der Erstellung wird nicht unterstützt. Razor-Dateien können optional zur Veröffentlichung kompiliert und mit der App über das Vorkompilierungstool bereitgestellt werden.
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
Eine Razor-Datei wird zur Laufzeit kompiliert, wenn die zugeordnete Razor-Seite oder MVC-Ansicht aufgerufen wird. Razor-Dateien werden mit dem [Razor SDK](xref:razor-pages/sdk) zur Erstellung und zur Veröffentlichung kompiliert.
::: moniker-end

## <a name="precompilation-considerations"></a>Was vor der Kompilierung beachtet werden muss

Im Folgenden werden Nebeneffekte der Vorkompilierung von Razor-Dateien aufgeführt:

* Kleineres veröffentlichtes Paket
* Schnellere Startzeit
* Razor-Dateien können nicht bearbeitet werden. Der zugehörige Inhalt fehlt im veröffentlichten Paket.

## <a name="deploy-precompiled-files"></a>Bereitstellen vorkompilierter Dateien

::: moniker range=">= aspnetcore-2.1"
Die Kompilierung zur Erstellung und Veröffentlichung von Razor-Dateien wird standardmäßig vom Razor SDK aktiviert. Das Bearbeiten von Razor-Dateien, nachdem sie aktualisiert wurden, wird zum Zeitpunkt der Erstellung unterstützt. Standardmäßig wird nur die kompilierte Datei *Views.dll*, ohne *CSHTML*-Dateien, mit Ihrer App bereitgestellt.

> [!IMPORTANT]
> Das Razor SDK ist nur dann wirksam, wenn keine für die Vorkompilierung spezifischen Eigenschaften in der Projektdatei festgelegt sind. Durch Festlegen der Eigenschaft `MvcRazorCompileOnPublish` der *CSPROJ*-Datei auf `true` wird das Razor SDK beispielsweise deaktiviert.
::: moniker-end

::: moniker range="= aspnetcore-2.0"
Wenn Ihr Projekt .NET Framework als Ziel verwendet, installieren Sie das NuGet-Paket [Microsoft.AspNetCore.Mvc.Razor.ViewCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.ViewCompilation/):

[!code-xml[](view-compilation/sample/DotNetFrameworkProject.csproj?name=snippet_ViewCompilationPackage)]

Wenn Ihr Paket für .NET Core gedacht ist, sind keine Änderungen erforderlich.

Mit den Projektvorlagen von ASP.NET Core 2.x wird die Eigenschaft `MvcRazorCompileOnPublish` standardmäßig explizit auf `true` festgelegt. Dieses Element kann folglich sicher aus der *CSPROJ*-Datei entfernt werden.

> [!IMPORTANT]
> Die Vorkompilierung von Razor-Dateien steht aktuell beim Durchführen einer [eigenständigen Bereitstellung (Self-Contained Deployment, SCD)](/dotnet/core/deploying/#self-contained-deployments-scd) in ASP.NET Core 2.0 nicht zur Verfügung.
::: moniker-end

::: moniker range="= aspnetcore-1.1"
Legen Sie die Eigenschaft `MvcRazorCompileOnPublish` auf `true` fest und installieren Sie das NuGet-Paket [Microsoft.AspNetCore.Mvc.Razor.ViewCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.ViewCompilation/). Das folgende *CSPROJ*-Beispiel veranschaulicht diese Einstellungen:

[!code-xml[](view-compilation/sample/MvcRazorCompileOnPublish.csproj?highlight=4,10)]
::: moniker-end

::: moniker range="<= aspnetcore-2.0"
Bereiten Sie die App mit dem [Veröffentlichungsbefehl der .NET Core-CLI](/dotnet/core/tools/dotnet-publish) für eine [Framework-abhängige Bereitstellung](/dotnet/core/deploying/#framework-dependent-deployments-fdd) vor. Führen Sie z. B. den folgenden Befehl auf der Stammebene des Projekts aus:

```console
dotnet publish -c Release
```

Die Datei *<project_name>.PrecompiledViews.dll*, welche die kompilierten Razor-Dateien enthält, wird erzeugt, wenn die Vorkompilierung erfolgreich abgeschlossen wurde. Im unten stehenden Screenshot wird beispielsweise der Inhalt von *Index.cshtml* in *WebApplication1.PrecompiledViews.dll* dargestellt:

![Razor-Ansichten in einer DLL](view-compilation/_static/razor-views-in-dll.png)
::: moniker-end

## <a name="additional-resources"></a>Zusätzliche Ressourcen

::: moniker range="= aspnetcore-1.1"
* <xref:mvc/views/overview>
::: moniker-end

::: moniker range="= aspnetcore-2.0"
* <xref:razor-pages/index>
* <xref:mvc/views/overview>
::: moniker-end

::: moniker range=">= aspnetcore-2.1"
* <xref:razor-pages/index>
* <xref:mvc/views/overview>
* <xref:razor-pages/sdk>
::: moniker-end
