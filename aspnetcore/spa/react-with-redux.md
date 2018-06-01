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
# <a name="use-the-react-with-redux-project-template-with-aspnet-core"></a><span data-ttu-id="9a18b-103">Verwenden der React-Redux-Projektvorlage mit ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="9a18b-103">Use the React-with-Redux project template with ASP.NET Core</span></span>

::: moniker range="= aspnetcore-2.0"

> [!NOTE]
> <span data-ttu-id="9a18b-104">Diese Dokumentation befasst sich nicht mit der in ASP.NET Core 2.0 enthaltenen React-Redux-Projektvorlage.</span><span class="sxs-lookup"><span data-stu-id="9a18b-104">This documentation isn't about the React-with-Redux project template included in ASP.NET Core 2.0.</span></span> <span data-ttu-id="9a18b-105">Sie befasst sich mit der neueren React-Redux-Vorlage, die manuell aktualisiert werden kann.</span><span class="sxs-lookup"><span data-stu-id="9a18b-105">It's about the newer React-with-Redux template to which you can update manually.</span></span> <span data-ttu-id="9a18b-106">Die Vorlage ist standardmäßig in ASP.NET Core 2.1 enthalten.</span><span class="sxs-lookup"><span data-stu-id="9a18b-106">The template is included in ASP.NET Core 2.1 by default.</span></span>

::: moniker-end

<span data-ttu-id="9a18b-107">Die aktualisierte React-Redux-Projektvorlage stellt einen geeigneten Anfangspunkt für ASP.NET Core-Apps dar, die React-, Redux- und CRA-Konventionen ([create-react-app](https://github.com/facebookincubator/create-react-app)) für die Implementierung einer umfangreichen, clientseitigen Benutzerschnittstelle (User Interface, UI) verwenden.</span><span class="sxs-lookup"><span data-stu-id="9a18b-107">The updated React-with-Redux project template provides a convenient starting point for ASP.NET Core apps using React, Redux, and [create-react-app](https://github.com/facebookincubator/create-react-app) (CRA) conventions to implement a rich, client-side user interface (UI).</span></span>

<span data-ttu-id="9a18b-108">Mit Ausnahme des Befehls für die Projekterstellung sind sämtliche Informationen zur React-Redux-Vorlage mit denen zur React-Vorlage identisch.</span><span class="sxs-lookup"><span data-stu-id="9a18b-108">With the exception of the project creation command, all information about the React-with-Redux template is the same as the React template.</span></span> <span data-ttu-id="9a18b-109">Führen Sie zum Erstellen dieses Projekttyps `dotnet new reactredux` anstelle von `dotnet new react` aus.</span><span class="sxs-lookup"><span data-stu-id="9a18b-109">To create this project type, run `dotnet new reactredux` instead of `dotnet new react`.</span></span> <span data-ttu-id="9a18b-110">Weitere Informationen zu den Funktionen beider React-basierten Vorlagen finden Sie in der [Dokumentation zu React-Vorlagen](xref:spa/react).</span><span class="sxs-lookup"><span data-stu-id="9a18b-110">For more information about the functionality common to both React-based templates, see [React template documentation](xref:spa/react).</span></span>
