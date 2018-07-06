---
uid: web-pages/overview/performance-and-traffic/bundling-and-minifying-assets-in-an-aspnet-web-pages-razor-site
title: Bündeln und Minimieren der Objekte in einer ASP.NET Web Pages (Razor) Standort | Microsoft-Dokumentation
author: microsoft
description: Bündelung und Minimierung werden Methoden zum Beschleunigen Ihrer Site. Bündeln können kombinieren Sie mehrere Dateien für JavaScript (.js) oder mehrere cascading Stylesheet (...)
ms.author: aspnetcontent
ms.date: 06/21/2012
ms.assetid: 8906f1e9-4b66-4a03-8e8a-9e9debf8ed91
msc.legacyurl: /web-pages/overview/performance-and-traffic/bundling-and-minifying-assets-in-an-aspnet-web-pages-razor-site
msc.type: authoredcontent
ms.openlocfilehash: 5799c4a3ae1c36636e0e485361f0f55e62ec7ec7
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2018
ms.locfileid: "37813439"
---
<a name="bundling-and-minifying-assets-in-an-aspnet-web-pages-razor-site"></a><span data-ttu-id="f32ec-104">Bündeln und Minimieren der Objekte in einer ASP.NET Web Pages (Razor)-Website</span><span class="sxs-lookup"><span data-stu-id="f32ec-104">Bundling and Minifying Assets in an ASP.NET Web Pages (Razor) Site</span></span>
====================
<span data-ttu-id="f32ec-105">durch [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="f32ec-105">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="f32ec-106">Bündelung und Minimierung werden Methoden zum Beschleunigen Ihrer Site.</span><span class="sxs-lookup"><span data-stu-id="f32ec-106">Bundling and minification are ways to make your site faster.</span></span> <span data-ttu-id="f32ec-107">Bündeln können Sie mehrere JavaScript kombinieren (*js*) Dateien oder mehrere cascading Stylesheet (*CSS*) Dateien, damit sie als eine Einheit, anstatt jeweils einzeln heruntergeladen werden können.</span><span class="sxs-lookup"><span data-stu-id="f32ec-107">Bundling lets you combine multiple JavaScript (*.js*) files or multiple cascading style sheet (*.css*) files so that they can be downloaded as a unit, rather than one at a time.</span></span> <span data-ttu-id="f32ec-108">Staucht, Leerzeichen und führt andere Arten von Komprimierung, um den heruntergeladenen Dateien möglichst klein eine möglich machen.</span><span class="sxs-lookup"><span data-stu-id="f32ec-108">Minification squeezes out whitespace and performs other types of compression to make the downloaded files as small a possible.</span></span>
> 
> > [!NOTE]
> > <span data-ttu-id="f32ec-109">Die RC-Version von ASP.NET Web Pages 2 unterstützt keine Bündelung und Minimierung, da das Paket, das die erforderlichen Elemente enthält noch nicht in Microsoft WebMatrix verfügbar ist.</span><span class="sxs-lookup"><span data-stu-id="f32ec-109">The RC release of ASP.NET Web Pages 2 does not support bundling and minification because the package that contains the required elements is not yet available in Microsoft WebMatrix.</span></span> <span data-ttu-id="f32ec-110">Wir entschuldigen uns für diese Unannehmlichkeit.</span><span class="sxs-lookup"><span data-stu-id="f32ec-110">We apologize for this inconvenience.</span></span> <span data-ttu-id="f32ec-111">Das Paket wird erwartet, in der endgültigen Version von ASP.NET Web Pages 2 und WebMatrix 2 verfügbar sein.</span><span class="sxs-lookup"><span data-stu-id="f32ec-111">The package is expected to be available in the final release of ASP.NET Web Pages 2 and WebMatrix 2.</span></span>
