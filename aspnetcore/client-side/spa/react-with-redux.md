---
title: Verwenden der React-Redux-Projektvorlage mit ASP.NET Core
author: SteveSandersonMS
description: Erfahren Sie, wie Sie sich mit der Projektvorlage für die Einzelseitenanwendung (Single-Page Application, SPA) von ASP.NET Core für React-Redux und create-react-app vertraut machen.
monikerRange: '>= aspnetcore-2.0'
ms.author: scaddie
ms.custom: mvc
ms.date: 02/21/2018
uid: spa/react-with-redux
ms.openlocfilehash: dab3d20865250aae548bff4614e631dd7c73b46f
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/20/2018
ms.locfileid: "36291483"
---
# <a name="use-the-react-with-redux-project-template-with-aspnet-core"></a><span data-ttu-id="60402-103">Verwenden der React-Redux-Projektvorlage mit ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="60402-103">Use the React-with-Redux project template with ASP.NET Core</span></span>

::: moniker range="= aspnetcore-2.0"

> [!NOTE]
> <span data-ttu-id="60402-104">Diese Dokumentation befasst sich nicht mit der in ASP.NET Core 2.0 enthaltenen React-Redux-Projektvorlage.</span><span class="sxs-lookup"><span data-stu-id="60402-104">This documentation isn't about the React-with-Redux project template included in ASP.NET Core 2.0.</span></span> <span data-ttu-id="60402-105">Es geht hier um die neuere React-with-Redux-Vorlage, auf die Sie manuell aktualisieren können.</span><span class="sxs-lookup"><span data-stu-id="60402-105">It's about the newer React-with-Redux template to which you can update manually.</span></span> <span data-ttu-id="60402-106">Die Vorlage ist standardmäßig in ASP.NET Core 2.1 enthalten.</span><span class="sxs-lookup"><span data-stu-id="60402-106">The template is included in ASP.NET Core 2.1 by default.</span></span>

::: moniker-end

<span data-ttu-id="60402-107">Die aktualisierte React-with-Redux-Projektvorlage stellt einen nützlichen Ausgangspunkt für ASP.NET Core-Apps mit Redux, React und [CRA-Konventionen ](https://github.com/facebookincubator/create-react-app) (Create-React-App) dar, um eine umfassende clientseitige Benutzeroberfläche (UI) zu implementieren.
</span><span class="sxs-lookup"><span data-stu-id="60402-107">The updated React-with-Redux project template provides a convenient starting point for ASP.NET Core apps using React, Redux, and [create-react-app](https://github.com/facebookincubator/create-react-app) (CRA) conventions to implement a rich, client-side user interface (UI).</span></span>

<span data-ttu-id="60402-108">Mit Ausnahme des Befehls zur Projekterstellung sind alle Informationen zur React-with-Redux-Vorlage identisch mit jenen zur React-Vorlage.</span><span class="sxs-lookup"><span data-stu-id="60402-108">With the exception of the project creation command, all information about the React-with-Redux template is the same as the React template.</span></span> <span data-ttu-id="60402-109">Führen Sie zum Erstellen dieses Projekttyps `dotnet new reactredux` anstelle von `dotnet new react` aus.</span><span class="sxs-lookup"><span data-stu-id="60402-109">To create this project type, run `dotnet new reactredux` instead of `dotnet new react`.</span></span> <span data-ttu-id="60402-110">Weitere Informationen zu den Funktionen für die beiden React-basierten Vorlagen finden Sie in der [Dokumentation zur React-Vorlage(](xref:spa/react).</span><span class="sxs-lookup"><span data-stu-id="60402-110">For more information about the functionality common to both React-based templates, see [React template documentation](xref:spa/react).</span></span>
