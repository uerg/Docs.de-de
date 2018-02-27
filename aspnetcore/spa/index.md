---
title: "Verwenden der Projektvorlagen für Einzelseitenanwendungen (SPAs)"
author: SteveSandersonMS
description: "Erfahren Sie, wie Sie die Projektvorlagen für die Einzelseitenanwendung (Single-Page Application, SPA) von ASP.NET Core installieren und sich mit ihr vertraut machen."
manager: wpickett
ms.author: scaddie
ms.custom: mvc
ms.date: 02/21/2018
ms.devlang: csharp
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: spa/index
ms.openlocfilehash: 63b56de101199e9ea0d66d89d2dd7288e47902f6
ms.sourcegitcommit: 49fb3b7669b504d35edad34db8285e56b958a9fc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/23/2018
---
# <a name="use-the-single-page-application-templates"></a>Verwenden der Projektvorlagen für Einzelseitenanwendungen (SPAs)

> [!NOTE]
> Das veröffentlichte .NET Core 2.0.x SDK umfasst ältere Projektvorlagen für Angular, React und React mit Redux. Diese Dokumentation bezieht sich nicht auf diese älteren Projektvorlagen. Sie gilt für die neuesten Projektvorlagen für Angular, React und React mit Redux, die manuell in ASP.NET Core 2.0 installiert werden können. Die Vorlagen sind standardmäßig in ASP.NET Core 2.1 enthalten.

## <a name="prerequisites"></a>Erforderliche Komponenten

* [.NET Core SDK](https://www.microsoft.com/net/download), Version 2.0.0 oder höher
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
