---
title: Verwenden der Projektvorlagen für Einzelseitenanwendungen (SPAs) mit ASP.NET Core
author: SteveSandersonMS
description: Erfahren Sie, wie Sie die Projektvorlagen für die Einzelseitenanwendung (Single-Page Application, SPA) von ASP.NET Core installieren und sich mit ihr vertraut machen.
monikerRange: '>= aspnetcore-2.0'
ms.author: scaddie
ms.custom: mvc
ms.date: 02/21/2018
uid: spa/index
ms.openlocfilehash: ab164ae5d2df47739ec04b32cd21835ffdf9f87f
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/20/2018
ms.locfileid: "36291443"
---
# <a name="use-the-single-page-application-templates-with-aspnet-core"></a><span data-ttu-id="b7d91-103">Verwenden der Projektvorlagen für Einzelseitenanwendungen (SPAs) mit ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="b7d91-103">Use the Single Page Application templates with ASP.NET Core</span></span>

::: moniker range="= aspnetcore-2.0"

> [!NOTE]
> <span data-ttu-id="b7d91-104">Das veröffentlichte .NET Core 2.0.x SDK umfasst ältere Projektvorlagen für Angular, React und React mit Redux.</span><span class="sxs-lookup"><span data-stu-id="b7d91-104">The released .NET Core 2.0.x SDK includes older project templates for Angular, React, and React with Redux.</span></span> <span data-ttu-id="b7d91-105">Diese Dokumentation bezieht sich nicht auf diese älteren Projektvorlagen.</span><span class="sxs-lookup"><span data-stu-id="b7d91-105">This documentation isn't about those older project templates.</span></span> <span data-ttu-id="b7d91-106">Sie gilt für die neuesten Projektvorlagen für Angular, React und React mit Redux, die manuell in ASP.NET Core 2.0 installiert werden können.</span><span class="sxs-lookup"><span data-stu-id="b7d91-106">This documentation is for the latest Angular, React, and React with Redux templates, which can be installed manually into ASP.NET Core 2.0.</span></span> <span data-ttu-id="b7d91-107">Die Vorlagen sind standardmäßig in ASP.NET Core 2.1 enthalten.</span><span class="sxs-lookup"><span data-stu-id="b7d91-107">The templates are included by default with ASP.NET Core 2.1.</span></span>

::: moniker-end

## <a name="prerequisites"></a><span data-ttu-id="b7d91-108">Erforderliche Komponenten</span><span class="sxs-lookup"><span data-stu-id="b7d91-108">Prerequisites</span></span>

* [!INCLUDE [](~/includes/net-core-sdk-download-link.md)]
* <span data-ttu-id="b7d91-109">[Node.js](https://nodejs.org), Version 6 oder höher</span><span class="sxs-lookup"><span data-stu-id="b7d91-109">[Node.js](https://nodejs.org), version 6 or later</span></span>

## <a name="installation"></a><span data-ttu-id="b7d91-110">Installation</span><span class="sxs-lookup"><span data-stu-id="b7d91-110">Installation</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="b7d91-111">Die Vorlagen sind bereits mit ASP.NET Core 2.1 installiert.</span><span class="sxs-lookup"><span data-stu-id="b7d91-111">The templates are already installed with ASP.NET Core 2.1.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="b7d91-112">Wenn Sie über ASP.NET Core 2.0 verfügen, führen Sie den folgenden Befehl aus, um die aktualisierten ASP.NET Core-Vorlagen für Angular, React und React mit Redux zu installieren:</span><span class="sxs-lookup"><span data-stu-id="b7d91-112">If you have ASP.NET Core 2.0, run the following command to install the updated ASP.NET Core templates for Angular, React, and React with Redux:</span></span>

```console
dotnet new --install Microsoft.DotNet.Web.Spa.ProjectTemplates::2.0.0
```

::: moniker-end

## <a name="use-the-templates"></a><span data-ttu-id="b7d91-113">Verwenden der Vorlagen</span><span class="sxs-lookup"><span data-stu-id="b7d91-113">Use the templates</span></span>

* [<span data-ttu-id="b7d91-114">Verwenden der Angular-Projektvorlage</span><span class="sxs-lookup"><span data-stu-id="b7d91-114">Use the Angular project template</span></span>](xref:spa/angular)
* [<span data-ttu-id="b7d91-115">Verwenden der React-Projektvorlage</span><span class="sxs-lookup"><span data-stu-id="b7d91-115">Use the React project template</span></span>](xref:spa/react)
* [<span data-ttu-id="b7d91-116">Verwenden der React-Redux-Projektvorlage</span><span class="sxs-lookup"><span data-stu-id="b7d91-116">Use the React with Redux project template</span></span>](xref:spa/react-with-redux)
