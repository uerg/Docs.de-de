---
title: "Hinzufügen der Suche"
author: rick-anderson
description: "Informationen zum Hinzufügen der Suche zu einer einfachen ASP.NET Core MVC-App"
ms.author: riande
manager: wpickett
ms.date: 03/07/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/first-mvc-app/search
ms.openlocfilehash: aaa00a9eedda73a2c66da30b5ace3b5b5a7760b2
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/19/2018
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
