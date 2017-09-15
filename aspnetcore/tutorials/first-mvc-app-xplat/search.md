---
title: "Hinzufügen der Suche"
author: rick-anderson
description: "Informationen zum Hinzufügen der Suche zu einer einfachen ASP.NET Core MVC-App"
keywords: ASP.NET Core,
ms.author: riande
manager: wpickett
ms.date: 04/07/2017
ms.topic: get-started-article
ms.assetid: d69e5529-ffff-4628-855d-200206d96269
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/first-mvc-app-xplat/search
ms.openlocfilehash: 57419c697aea80a054e906c75002f5a39c512fc6
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/11/2017
---
[!INCLUDE[adding-model](../../includes/mvc-intro/search1.md)]

Hinweis: Bei SQLlite wird Groß-/Kleinschreibung beachtet, weshalb Sie nach „Ghost“ und nicht nach „Ghost“ suchen müssen.

[!INCLUDE[adding-model](../../includes/mvc-intro/search2.md)]

Ändern Sie das Tag `<form>` in der Razor-Ansicht *Views\movie\Index.cshtml* so, dass `method="get"` angegeben wird:

```html
<form asp-controller="Movies" asp-action="Index" method="get">
```

[!INCLUDE[adding-model](../../includes/mvc-intro/search3.md)]

>[!div class="step-by-step"]
[Zurück: Controllermethoden und Ansichten](controller-methods-views.md)
[Weiter: Hinzufügen eines Felds](new-field.md)  
