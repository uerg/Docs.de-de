---
uid: web-pages/overview/performance-and-traffic/bundling-and-minifying-assets-in-an-aspnet-web-pages-razor-site
title: Bundling und Minimierung mit Objekten in einer ASP.NET Web Pages (Razor) Website | Microsoft Docs
author: microsoft
description: Bundling und Minimierung werden Methoden zum Beschleunigen Ihrer Website. M bündeln können kombinieren Sie mehrere JavaScript (. js)-Dateien oder mehrere cascading Stylesheet (...)
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/21/2012
ms.topic: article
ms.assetid: 8906f1e9-4b66-4a03-8e8a-9e9debf8ed91
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/performance-and-traffic/bundling-and-minifying-assets-in-an-aspnet-web-pages-razor-site
msc.type: authoredcontent
ms.openlocfilehash: df00b5c9e741e429011143d04121df97c1930602
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
ms.locfileid: "26528549"
---
<a name="bundling-and-minifying-assets-in-an-aspnet-web-pages-razor-site"></a><span data-ttu-id="e2a26-104">Bundling und Minimierung mit Anlagen an einem Standort der ASP.NET Web Pages (Razor)</span><span class="sxs-lookup"><span data-stu-id="e2a26-104">Bundling and Minifying Assets in an ASP.NET Web Pages (Razor) Site</span></span>
====================
<span data-ttu-id="e2a26-105">durch [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="e2a26-105">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="e2a26-106">Bundling und Minimierung werden Methoden zum Beschleunigen Ihrer Website.</span><span class="sxs-lookup"><span data-stu-id="e2a26-106">Bundling and minification are ways to make your site faster.</span></span> <span data-ttu-id="e2a26-107">Bundling können Sie mehrere JavaScript kombinieren (*js*) Dateien oder mehrere cascading Stylesheet (*CSS*) Dateien, damit sie als eine Einheit, statt jeweils einzeln heruntergeladen werden können.</span><span class="sxs-lookup"><span data-stu-id="e2a26-107">Bundling lets you combine multiple JavaScript (*.js*) files or multiple cascading style sheet (*.css*) files so that they can be downloaded as a unit, rather than one at a time.</span></span> <span data-ttu-id="e2a26-108">Minimierung staucht, Leerzeichen und führt andere Arten von datenkomprimierung, um den heruntergeladenen Dateien als kleine ein mögliches vornehmen.</span><span class="sxs-lookup"><span data-stu-id="e2a26-108">Minification squeezes out whitespace and performs other types of compression to make the downloaded files as small a possible.</span></span>
> 
> > [!NOTE]
> > <span data-ttu-id="e2a26-109">Die RC-Version von ASP.NET Web Pages 2 unterstützt keine Bündelung und Minimierung, da das Paket, das die erforderlichen Elemente enthält noch nicht in Microsoft WebMatrix verfügbar ist.</span><span class="sxs-lookup"><span data-stu-id="e2a26-109">The RC release of ASP.NET Web Pages 2 does not support bundling and minification because the package that contains the required elements is not yet available in Microsoft WebMatrix.</span></span> <span data-ttu-id="e2a26-110">Wir entschuldigen uns für die Unannehmlichkeiten.</span><span class="sxs-lookup"><span data-stu-id="e2a26-110">We apologize for this inconvenience.</span></span> <span data-ttu-id="e2a26-111">Das Paket muss in der endgültigen Version von ASP.NET Web Pages 2 und WebMatrix 2 verfügbar sein.</span><span class="sxs-lookup"><span data-stu-id="e2a26-111">The package is expected to be available in the final release of ASP.NET Web Pages 2 and WebMatrix 2.</span></span>
