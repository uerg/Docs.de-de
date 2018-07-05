---
title: Hinzufügen der Suche zu einer ASP.NET Core MVC-App
author: rick-anderson
description: Informationen zum Hinzufügen der Suche zu einer einfachen ASP.NET Core MVC-App
ms.author: riande
ms.date: 04/07/2017
uid: tutorials/first-mvc-app-xplat/search
ms.openlocfilehash: cf4fe3806b45008f48bf5f0598057552bdcfae7c
ms.sourcegitcommit: 356c8d394aaf384c834e9c90cabab43bfe36e063
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/26/2018
ms.locfileid: "36961437"
---
[!INCLUDE [adding-model](../../includes/mvc-intro/search1.md)]

Hinweis: Bei SQLlite wird Groß-/Kleinschreibung beachtet, weshalb Sie nach „Ghost“ und nicht nach „Ghost“ suchen müssen.

[!INCLUDE [adding-model](../../includes/mvc-intro/search2.md)]

Ändern Sie das Tag `<form>` in der Razor-Ansicht *Views\movie\Index.cshtml* so, dass `method="get"` angegeben wird:

```html
<form asp-controller="Movies" asp-action="Index" method="get">
```

[!INCLUDE [adding-model](../../includes/mvc-intro/search3.md)]

> [!div class="step-by-step"]
> [Zurück: Controllermethoden und Ansichten](controller-methods-views.md)
> [Weiter: Hinzufügen eines Felds](new-field.md)  
