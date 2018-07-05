---
title: Hinzufügen der Suche zu einer ASP.NET Core MVC-App
author: rick-anderson
description: Informationen zum Hinzufügen der Suche zu einer einfachen ASP.NET Core MVC-App
ms.author: riande
ms.date: 03/07/2017
uid: tutorials/first-mvc-app/search
ms.openlocfilehash: d0581142b505323385feba441b707d29b3f6db5d
ms.sourcegitcommit: 356c8d394aaf384c834e9c90cabab43bfe36e063
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/26/2018
ms.locfileid: "36960839"
---
[!INCLUDE [adding-model](~/includes/mvc-intro/search1.md)]

Mit dem Befehl **rename** können Sie den Parameter `searchString` schnell in `id` umbenennen. Klicken Sie mit der rechten Maustaste auf `searchString` **> Umbenennen**.

![Kontextmenü](search/_static/rename.png)

Die Umbenennungsziele sind hervorgehoben.

![Code-Editor mit der in der gesamten „Index ActionResult“-Methode hervorgehobenen Variablen](search/_static/rename2.png)

Ändern Sie den Parameter in `id` und alle Vorkommen von `searchString` in `id`.

![Code-Editor mit in „id“ geänderter Variable](search/_static/rename3.png)

[!INCLUDE [adding-model](~/includes/mvc-intro/search2.md)]

Beachten Sie, wie IntelliSense uns hilft, das Markup zu aktualisieren.

![IntelliSense-Kontextmenü mit in der Liste der Attribute für das Formularelement ausgewählten Methode](search/_static/int_m.png)

![IntelliSense-Kontextmenü mit ausgewähltem „get“ in der Liste der Methodenattributwerte](search/_static/int_get.png)

Beachten Sie die unterschiedliche Schriftart im Tag `<form>`. Diese besondere Schriftart gibt an, dass das Tag von [Taghilfsprogrammen](~/mvc/views/tag-helpers/intro.md) unterstützt wird.

![Formulartag mit violettem Text](search/_static/th_font.png)

[!INCLUDE [adding-model](~/includes/mvc-intro/search3.md)]

> [!div class="step-by-step"]
> [Zurück](controller-methods-views.md)
> [Weiter](new-field.md)  
