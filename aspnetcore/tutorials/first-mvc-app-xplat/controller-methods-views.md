---
title: Controllermethoden und Ansichten in ASP.NET Core
author: rick-anderson
description: Erfahren Sie mehr über das Arbeiten mit Controllermethoden, Ansichten und DataAnnotation-Objekten in ASP.NET Core.
ms.author: riande
ms.date: 04/07/2017
uid: tutorials/first-mvc-app-xplat/controller-methods-views
ms.openlocfilehash: 7cf42807eaba356cd090a08bba9357c3ec237087
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/20/2018
ms.locfileid: "36278439"
---
# <a name="controller-methods-and-views-in-aspnet-core"></a>Controllermethoden und Ansichten in ASP.NET Core

Von [Rick Anderson](https://twitter.com/RickAndMSFT)

Für den Anfang ist die Movie-App schon recht ansprechend, doch es gibt Raum für Verbesserungen. Wir wollen die Uhrzeit nicht sehen (12:00:00 Uhr in der nachstehenden Abbildung), und **ReleaseDate** soll als zwei Wörter angezeigt werden.

![Indexansicht: „ReleaseDate“ ist ein Wort (ohne Leerzeichen), und bei jedem Veröffentlichungsdatum wird die Uhrzeit 12: 00 Uhr angezeigt](../../tutorials/first-mvc-app/working-with-sql/_static/m55.png)

Öffnen Sie die Datei *Models/Movie.cs*, und fügen Sie die nachfolgend gezeigten markierten Zeilen hinzu:

[!code-csharp[](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/MovieDate.cs?name=snippet_1&highlight=2,11-12)]

Erstellen Sie die App, und führen Sie sie aus.

<!-- include start
![MVC Movie application open browser showing movie data](../../tutorials/first-mvc-app/working-with-sql/_static/m55.png)

 -->

[!INCLUDE [adding-model](../../includes/mvc-intro/controller-methods-views.md)]

> [!div class="step-by-step"]
> [Zurück: Arbeiten mit SQLite](working-with-sql.md)
> [Weiter: Hinzufügen der Suche](search.md)  
