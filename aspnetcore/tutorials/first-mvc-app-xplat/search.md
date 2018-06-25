---
title: Hinzufügen der Suche
author: rick-anderson
description: Informationen zum Hinzufügen der Suche zu einer einfachen ASP.NET Core MVC-App
ms.author: riande
ms.date: 04/07/2017
uid: tutorials/first-mvc-app-xplat/search
ms.openlocfilehash: dc84eb38c0487d90451979ec9572bf1641571357
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/20/2018
ms.locfileid: "36276005"
---
[!INCLUDE [adding-model](../../includes/mvc-intro/search1.md)]

<span data-ttu-id="2423a-103">Hinweis: Bei SQLlite wird Groß-/Kleinschreibung beachtet, weshalb Sie nach „Ghost“ und nicht nach „Ghost“ suchen müssen.</span><span class="sxs-lookup"><span data-stu-id="2423a-103">Note: SQLlite is case sensitive, so you'll need to search for "Ghost" and not "ghost".</span></span>

[!INCLUDE [adding-model](../../includes/mvc-intro/search2.md)]

<span data-ttu-id="2423a-104">Ändern Sie das Tag `<form>` in der Razor-Ansicht *Views\movie\Index.cshtml* so, dass `method="get"` angegeben wird:</span><span class="sxs-lookup"><span data-stu-id="2423a-104">Change the `<form>` tag in the *Views\movie\Index.cshtml* Razor view to specify `method="get"`:</span></span>

```html
<form asp-controller="Movies" asp-action="Index" method="get">
```

[!INCLUDE [adding-model](../../includes/mvc-intro/search3.md)]

> [!div class="step-by-step"]
> <span data-ttu-id="2423a-105">[Zurück: Controllermethoden und Ansichten](controller-methods-views.md)
> [Weiter: Hinzufügen eines Felds](new-field.md)</span><span class="sxs-lookup"><span data-stu-id="2423a-105">[Previous - Controller methods and views](controller-methods-views.md)
[Next - Add a field](new-field.md)</span></span>  
