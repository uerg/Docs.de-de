---
title: "Hinzufügen der Suche"
author: rick-anderson
description: "Informationen zum Hinzufügen der Suche zu einer einfachen ASP.NET Core MVC-App"
keywords: ASP.NET Core,
ms.author: riande
manager: wpickett
ms.date: 03/07/2017
ms.topic: get-started-article
ms.assetid: d69e5529-8ef6-4628-855d-200206d962b9
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/first-mvc-app/search
ms.openlocfilehash: f811cd0063404872b0e993a99e8a92cc354064ce
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
[!INCLUDE[adding-model](../../includes/mvc-intro/search1.md)]

Mit dem Befehl **rename** können Sie den Parameter `searchString` schnell in `id` umbenennen. Klicken Sie mit der rechten Maustaste auf `searchString` **> Umbenennen**.

![Kontextmenü](search/_static/rename.png)

Die Umbenennungsziele sind hervorgehoben.

![Code-Editor mit der in der gesamten „Index ActionResult“-Methode hervorgehobenen Variablen](search/_static/rename2.png)

Ändern Sie den Parameter in `id` und alle Vorkommen von `searchString` in `id`.

![Code-Editor mit in „id“ geänderter Variable](search/_static/rename3.png)

[!INCLUDE[adding-model](../../includes/mvc-intro/search2.md)]

Beachten Sie, wie IntelliSense uns hilft, das Markup zu aktualisieren.

![IntelliSense-Kontextmenü mit in der Liste der Attribute für das Formularelement ausgewählten Methode](search/_static/int_m.png)

![IntelliSense-Kontextmenü mit ausgewähltem „get“ in der Liste der Methodenattributwerte](search/_static/int_get.png)

Beachten Sie die unterschiedliche Schriftart im Tag `<form>`. Diese besondere Schriftart gibt an, dass das Tag von [Taghilfsprogrammen](../../mvc/views/tag-helpers/intro.md) unterstützt wird.

![Formulartag mit violettem Text](search/_static/th_font.png)

[!INCLUDE[adding-model](../../includes/mvc-intro/search3.md)]

>[!div class="step-by-step"]
[Zurück](controller-methods-views.md)
[Weiter](new-field.md)  
