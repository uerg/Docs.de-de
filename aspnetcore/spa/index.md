---
title: Verwenden der Projektvorlagen für Einzelseitenanwendungen (SPAs) mit ASP.NET Core
author: SteveSandersonMS
description: Erfahren Sie, wie Sie die Projektvorlagen für die Einzelseitenanwendung (Single-Page Application, SPA) von ASP.NET Core installieren und sich mit ihr vertraut machen.
manager: wpickett
monikerRange: '>= aspnetcore-2.0'
ms.author: scaddie
ms.custom: mvc
ms.date: 02/21/2018
ms.devlang: csharp
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: spa/index
ms.openlocfilehash: f7bb23e9001c7606c3e622bf4575a4debec56ccd
ms.sourcegitcommit: 466300d32f8c33e64ee1b419a2cbffe702863cdf
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/27/2018
---
# <a name="use-the-single-page-application-templates-with-aspnet-core"></a>Verwenden der Projektvorlagen für Einzelseitenanwendungen (SPAs) mit ASP.NET Core

::: moniker range="= aspnetcore-2.0"

> [!NOTE]
> Das veröffentlichte .NET Core 2.0.x SDK umfasst ältere Projektvorlagen für Angular, React und React mit Redux. Diese Dokumentation bezieht sich nicht auf diese älteren Projektvorlagen. Sie gilt für die neuesten Projektvorlagen für Angular, React und React mit Redux, die manuell in ASP.NET Core 2.0 installiert werden können. Die Vorlagen sind standardmäßig in ASP.NET Core 2.1 enthalten.

::: moniker-end

## <a name="prerequisites"></a>Erforderliche Komponenten

* [!INCLUDE [](~/includes/net-core-sdk-download-link.md)]
* [Node.js](https://nodejs.org), Version 6 oder höher

## <a name="installation"></a>Installation

::: moniker range=">= aspnetcore-2.1"

Die Vorlagen sind bereits mit ASP.NET Core 2.1 installiert.

::: moniker-end

::: moniker range="= aspnetcore-2.0"

Wenn Sie über ASP.NET Core 2.0 verfügen, führen Sie den folgenden Befehl aus, um die aktualisierten ASP.NET Core-Vorlagen für Angular, React und React mit Redux zu installieren:

```console
dotnet new --install Microsoft.DotNet.Web.Spa.ProjectTemplates::2.0.0
```

::: moniker-end

## <a name="use-the-templates"></a>Verwenden der Vorlagen

* [Verwenden der Angular-Projektvorlage](xref:spa/angular)
* [Verwenden der React-Projektvorlage](xref:spa/react)
* [Verwenden der React-Redux-Projektvorlage](xref:spa/react-with-redux)
