---
title: Verwenden der React-Redux-Projektvorlage mit ASP.NET Core
author: SteveSandersonMS
description: Erfahren Sie, wie Sie sich mit der Projektvorlage für die Einzelseitenanwendung (Single-Page Application, SPA) von ASP.NET Core für React-Redux und create-react-app vertraut machen.
manager: wpickett
monikerRange: '>= aspnetcore-2.0'
ms.author: scaddie
ms.custom: mvc
ms.date: 02/21/2018
ms.devlang: csharp
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: spa/react-with-redux
ms.openlocfilehash: 7ec4f6d53a4723ace087b1dc256de7845cb44cc6
ms.sourcegitcommit: 466300d32f8c33e64ee1b419a2cbffe702863cdf
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/27/2018
ms.locfileid: "34555221"
---
# <a name="use-the-react-with-redux-project-template-with-aspnet-core"></a>Verwenden der React-Redux-Projektvorlage mit ASP.NET Core

::: moniker range="= aspnetcore-2.0"

> [!NOTE]
> Diese Dokumentation befasst sich nicht mit der in ASP.NET Core 2.0 enthaltenen React-Redux-Projektvorlage. Sie befasst sich mit der neueren React-Redux-Vorlage, die manuell aktualisiert werden kann. Die Vorlage ist standardmäßig in ASP.NET Core 2.1 enthalten.

::: moniker-end

Die aktualisierte React-Redux-Projektvorlage stellt einen geeigneten Anfangspunkt für ASP.NET Core-Apps dar, die React-, Redux- und CRA-Konventionen ([create-react-app](https://github.com/facebookincubator/create-react-app)) für die Implementierung einer umfangreichen, clientseitigen Benutzerschnittstelle (User Interface, UI) verwenden.

Mit Ausnahme des Befehls für die Projekterstellung sind sämtliche Informationen zur React-Redux-Vorlage mit denen zur React-Vorlage identisch. Führen Sie zum Erstellen dieses Projekttyps `dotnet new reactredux` anstelle von `dotnet new react` aus. Weitere Informationen zu den Funktionen beider React-basierten Vorlagen finden Sie in der [Dokumentation zu React-Vorlagen](xref:spa/react).
