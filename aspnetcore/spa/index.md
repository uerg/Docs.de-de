---
title: Verwenden der Projektvorlagen für Einzelseitenanwendungen (SPAs) mit ASP.NET Core
author: SteveSandersonMS
description: Erfahren Sie, wie Sie die Projektvorlagen für die Einzelseitenanwendung (Single-Page Application, SPA) von ASP.NET Core installieren und sich mit ihr vertraut machen.
manager: wpickett
ms.author: scaddie
ms.custom: mvc
ms.date: 02/21/2018
ms.devlang: csharp
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: spa/index
ms.openlocfilehash: eda4817de007f3c3184b2ba6ed6c97989ff17da5
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
---
# <a name="use-the-single-page-application-templates-with-aspnet-core"></a>Verwenden der Projektvorlagen für Einzelseitenanwendungen (SPAs) mit ASP.NET Core

> [!NOTE]
> Das veröffentlichte .NET Core 2.0.x SDK umfasst ältere Projektvorlagen für Angular, React und React mit Redux. Diese Dokumentation bezieht sich nicht auf diese älteren Projektvorlagen. Sie gilt für die neuesten Projektvorlagen für Angular, React und React mit Redux, die manuell in ASP.NET Core 2.0 installiert werden können. Die Vorlagen sind standardmäßig in ASP.NET Core 2.1 enthalten.

## <a name="prerequisites"></a>Erforderliche Komponenten

* [!INCLUDE [](~/includes/net-core-sdk-download-link.md)]
* [Node.js](https://nodejs.org), Version 6 oder höher

## <a name="installation"></a>Installation

Wenn Sie über ASP.NET Core 2.0 verfügen, führen Sie den folgenden Befehl aus, um die aktualisierten ASP.NET Core-Vorlagen für Angular, React und React mit Redux zu installieren:

```console
dotnet new --install Microsoft.DotNet.Web.Spa.ProjectTemplates::2.0.0
```

## <a name="use-the-templates"></a>Verwenden der Vorlagen

- [Verwenden der Angular-Projektvorlage](xref:spa/angular)
- [Verwenden der React-Projektvorlage](xref:spa/react)
- [Verwenden der React-Redux-Projektvorlage](xref:spa/react-with-redux)
