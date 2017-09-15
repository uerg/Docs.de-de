---
title: "Hinzufügen der Suche zu einer ASP.NET Core MVC-App"
author: rick-anderson
description: "Informationen zum Hinzufügen der Suche zu einer einfachen ASP.NET Core MVC-App"
keywords: ASP.NET Core,
ms.author: riande
manager: wpickett
ms.date: 04/07/2017
ms.topic: get-started-article
ms.assetid: d69e5529-ffff-4628-855d-200202096269
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/first-mvc-app-mac/search
ms.openlocfilehash: 8dab5293ab6a6fe65288bb230e4f39af462ba28b
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/11/2017
---
[!INCLUDE[adding-model](../../includes/mvc-intro/search1.md)]

<span data-ttu-id="88cdb-104">Hinweis: Bei SQLlite wird Groß-/Kleinschreibung beachtet, weshalb Sie nach „Ghost“ und nicht nach „Ghost“ suchen müssen.</span><span class="sxs-lookup"><span data-stu-id="88cdb-104">Note: SQLlite is case sensitive, so you'll need to search for "Ghost" and not "ghost".</span></span>

[!INCLUDE[adding-model](../../includes/mvc-intro/search2.md)]

<span data-ttu-id="88cdb-105">Ändern Sie das Tag `<form>` in der Razor-Ansicht *Views\movie\Index.cshtml* so, dass `method="get"` angegeben wird:</span><span class="sxs-lookup"><span data-stu-id="88cdb-105">Change the `<form>` tag in the *Views\movie\Index.cshtml* Razor view to specify `method="get"`:</span></span>

```html
<form asp-controller="Movies" asp-action="Index" method="get">
```

[!INCLUDE[adding-model](../../includes/mvc-intro/search3.md)]

>[!div class="step-by-step"]
<span data-ttu-id="88cdb-106">[Zurück: Controllermethoden und Ansichten](controller-methods-views.md)
[Weiter: Hinzufügen eines Felds](new-field.md)</span><span class="sxs-lookup"><span data-stu-id="88cdb-106">[Previous - Controller methods and views](controller-methods-views.md)
[Next - Add a field](new-field.md)</span></span>
