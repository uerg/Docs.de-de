---
title: Hinzufügen der Suche zu einer ASP.NET Core MVC-App
author: rick-anderson
description: Informationen zum Hinzufügen der Suche zu einer einfachen ASP.NET Core MVC-App
manager: wpickett
ms.author: riande
ms.date: 04/07/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/first-mvc-app-mac/search
ms.openlocfilehash: 3f648bc6c6d095b9fe8b6ac65bf5f51741938ed1
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
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
