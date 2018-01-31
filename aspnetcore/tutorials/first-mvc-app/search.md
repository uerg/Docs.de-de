---
title: "Hinzufügen der Suche"
author: rick-anderson
description: "Informationen zum Hinzufügen der Suche zu einer einfachen ASP.NET Core MVC-App"
manager: wpickett
ms.author: riande
ms.date: 03/07/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/first-mvc-app/search
ms.openlocfilehash: 3ab9086275ec4c3651383c4c845e40db55f67f4c
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2018
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
