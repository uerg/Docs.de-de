---
title: Hinzufügen der Suche zu einer ASP.NET Core MVC-App
author: rick-anderson
description: Informationen zum Hinzufügen der Suche zu einer einfachen ASP.NET Core MVC-App
ms.author: riande
ms.date: 03/07/2017
uid: tutorials/first-mvc-app/search
ms.openlocfilehash: d0581142b505323385feba441b707d29b3f6db5d
ms.sourcegitcommit: b8a2f14bf8dd346d7592977642b610bbcb0b0757
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2018
ms.locfileid: "38216195"
---
[!INCLUDE [adding-model](~/includes/mvc-intro/search1.md)]

<span data-ttu-id="7e499-103">Mit dem Befehl **rename** können Sie den Parameter `searchString` schnell in `id` umbenennen.</span><span class="sxs-lookup"><span data-stu-id="7e499-103">You can quickly rename the `searchString` parameter to `id` with the **rename** command.</span></span> <span data-ttu-id="7e499-104">Klicken Sie mit der rechten Maustaste auf `searchString` **> Umbenennen**.</span><span class="sxs-lookup"><span data-stu-id="7e499-104">Right click on `searchString` **> Rename**.</span></span>

![Kontextmenü](search/_static/rename.png)

<span data-ttu-id="7e499-106">Die Umbenennungsziele sind hervorgehoben.</span><span class="sxs-lookup"><span data-stu-id="7e499-106">The rename targets are highlighted.</span></span>

![Code-Editor mit der in der gesamten „Index ActionResult“-Methode hervorgehobenen Variablen](search/_static/rename2.png)

<span data-ttu-id="7e499-108">Ändern Sie den Parameter in `id` und alle Vorkommen von `searchString` in `id`.</span><span class="sxs-lookup"><span data-stu-id="7e499-108">Change the parameter to `id` and all occurrences of `searchString` change to `id`.</span></span>

![Code-Editor mit in „id“ geänderter Variable](search/_static/rename3.png)

[!INCLUDE [adding-model](~/includes/mvc-intro/search2.md)]

<span data-ttu-id="7e499-110">Beachten Sie, wie IntelliSense uns hilft, das Markup zu aktualisieren.</span><span class="sxs-lookup"><span data-stu-id="7e499-110">Notice how intelliSense helps us update the markup.</span></span>

![IntelliSense-Kontextmenü mit in der Liste der Attribute für das Formularelement ausgewählten Methode](search/_static/int_m.png)

![IntelliSense-Kontextmenü mit ausgewähltem „get“ in der Liste der Methodenattributwerte](search/_static/int_get.png)

<span data-ttu-id="7e499-113">Beachten Sie die unterschiedliche Schriftart im Tag `<form>`.</span><span class="sxs-lookup"><span data-stu-id="7e499-113">Notice the distinctive font in the `<form>` tag.</span></span> <span data-ttu-id="7e499-114">Diese besondere Schriftart gibt an, dass das Tag von [Taghilfsprogrammen](~/mvc/views/tag-helpers/intro.md) unterstützt wird.</span><span class="sxs-lookup"><span data-stu-id="7e499-114">That distinctive font indicates the tag is supported by [Tag Helpers](~/mvc/views/tag-helpers/intro.md).</span></span>

![Formulartag mit violettem Text](search/_static/th_font.png)

[!INCLUDE [adding-model](~/includes/mvc-intro/search3.md)]

> [!div class="step-by-step"]
> <span data-ttu-id="7e499-116">[Zurück](controller-methods-views.md)
> [Weiter](new-field.md)</span><span class="sxs-lookup"><span data-stu-id="7e499-116">[Previous](controller-methods-views.md)
[Next](new-field.md)</span></span>  
