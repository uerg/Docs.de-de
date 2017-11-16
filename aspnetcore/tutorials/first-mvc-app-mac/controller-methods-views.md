---
title: Controllermethoden und Ansichten in einer ASP.NET Core MVC-App
author: rick-anderson
description: Arbeiten mit Controllermethoden, Ansichten und DataAnnotations
keywords: ASP.NET Core,
ms.author: riande
manager: wpickett
ms.date: 04/07/2017
ms.topic: get-started-article
ms.assetid: c7313211-babe-babe-babe-8e72603cc0ce
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/first-mvc-app-mac/controller-methods-views
ms.openlocfilehash: 846a78a09cdde0cfdbcf35bb899c1822ebe8fea2
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
# <a name="controller-methods-and-views-in-an-aspnet-core-mvc-app"></a>Controllermethoden und Ansichten in einer ASP.NET Core MVC-App

Von [Rick Anderson](https://twitter.com/RickAndMSFT)

Die „movie“-App ist schon recht ansprechend, doch die Präsentation ist nicht ideal. Wir wollen die Uhrzeit nicht sehen (12:00:00 Uhr in der nachstehenden Abbildung), und **ReleaseDate** soll als zwei Wörter angezeigt werden.

![Indexansicht: „ReleaseDate“ ist ein Wort (ohne Leerzeichen), und bei jedem Veröffentlichungsdatum wird die Uhrzeit 12: 00 Uhr angezeigt](../../tutorials/first-mvc-app/working-with-sql/_static/m55.png)

Öffnen Sie die Datei *Models/Movie.cs*, und fügen Sie die nachfolgend gezeigten markierten Zeilen hinzu:

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/MovieDate.cs?name=snippet_1&highlight=2,11-12)]

Erstellen Sie die App, und führen Sie sie aus.

<!-- include start
![MVC Movie application open browser showing movie data](../../tutorials/first-mvc-app/working-with-sql/_static/m55.png)

 -->

[!INCLUDE[adding-model](../../includes/mvc-intro/controller-methods-views.md)]

>[!div class="step-by-step"]
[Zurück: Arbeiten mit SQLite](working-with-sql.md)
[Weiter: Hinzufügen der Suche](search.md)
