---
title: Controllermethoden und Ansichten in ASP.NET Core
author: rick-anderson
description: Erfahren Sie mehr über das Arbeiten mit Controllermethoden, Ansichten und DataAnnotation-Objekten in ASP.NET Core.
manager: wpickett
ms.author: riande
ms.date: 03/07/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/first-mvc-app/controller-methods-views
ms.openlocfilehash: 6fe0a0e71079bebcbd3a76abee0f2917f562e766
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
---
# <a name="controller-methods-and-views-in-aspnet-core"></a>Controllermethoden und Ansichten in ASP.NET Core

Von [Rick Anderson](https://twitter.com/RickAndMSFT)

Für den Anfang ist die Movie-App schon recht ansprechend, doch es gibt Raum für Verbesserungen. Wir wollen die Uhrzeit nicht sehen (12:00:00 Uhr in der nachstehenden Abbildung), und **ReleaseDate** soll als zwei Wörter angezeigt werden.

![Indexansicht: „ReleaseDate“ ist ein Wort (ohne Leerzeichen), und bei jedem Veröffentlichungsdatum wird die Uhrzeit 12: 00 Uhr angezeigt](working-with-sql/_static/m55.png)

Öffnen Sie die Datei *Models/Movie.cs*, und fügen Sie die nachfolgend gezeigten markierten Zeilen hinzu:

[!code-csharp[](start-mvc/sample/MvcMovie/Models/MovieDateWithExtraUsings.cs?name=snippet_1&highlight=13-14)]

Klicken Sie mit der rechten Maustaste auf eine rote Wellenlinie **> Schnellaktionen und Refactorings**.

  ![Kontextmenü mit **> Schnellaktionen und Refactorings**.](controller-methods-views/_static/qa.png)


Tippen Sie auf `using System.ComponentModel.DataAnnotations;`

  ![„using System.ComponentModel.DataAnnotations“ oben in der Liste](controller-methods-views/_static/da.png)

  Visual Studio fügt `using System.ComponentModel.DataAnnotations;` hinzu.

Entfernen Sie nun die `using`-Anweisungen, die nicht benötigt werden. Sie werden standardmäßig in einer hellgrauen Schrift angezeigt. Klicken Sie mit der rechten Maus auf die Datei *Movie.cs* > **Using-Direktiven entfernen und sortieren**.

![Using-Direktiven entfernen und sortieren](controller-methods-views/_static/rm.png)

Der aktualisierte Code:

[!code-csharp[](./start-mvc/sample/MvcMovie/Models/MovieDate.cs?name=snippet_1)]

<!-- include start -->

[!INCLUDE [adding-model](../../includes/mvc-intro/controller-methods-views.md)]

> [!div class="step-by-step"]
> [Zurück](working-with-sql.md)
> [Weiter](search.md)  
