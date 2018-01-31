---
title: Controllermethoden und -ansichten
author: rick-anderson
description: Arbeiten mit Controllermethoden, Ansichten und DataAnnotations
ms.author: riande
manager: wpickett
ms.date: 03/07/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/first-mvc-app/controller-methods-views
ms.openlocfilehash: e279b6c2852f1aea49685381ccaa5f7854f7c418
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/24/2018
---
# <a name="controller-methods-and-views"></a>Controllermethoden und -ansichten

Von [Rick Anderson](https://twitter.com/RickAndMSFT)

Für den Anfang ist die Movie-App schon recht ansprechend, doch es gibt Raum für Verbesserungen. Wir wollen die Uhrzeit nicht sehen (12:00:00 Uhr in der nachstehenden Abbildung), und **ReleaseDate** soll als zwei Wörter angezeigt werden.

![Indexansicht: „ReleaseDate“ ist ein Wort (ohne Leerzeichen), und bei jedem Veröffentlichungsdatum wird die Uhrzeit 12: 00 Uhr angezeigt](working-with-sql/_static/m55.png)

Öffnen Sie die Datei *Models/Movie.cs*, und fügen Sie die nachfolgend gezeigten markierten Zeilen hinzu:

[!code-csharp[Main](start-mvc/sample/MvcMovie/Models/MovieDateWithExtraUsings.cs?name=snippet_1&highlight=13-14)]

Klicken Sie mit der rechten Maustaste auf eine rote Wellenlinie **> Schnellaktionen und Refactorings**.

  ![Kontextmenü mit **> Schnellaktionen und Refactorings**.](controller-methods-views/_static/qa.png)


Tippen Sie auf `using System.ComponentModel.DataAnnotations;`

  ![„using System.ComponentModel.DataAnnotations“ oben in der Liste](controller-methods-views/_static/da.png)

  Visual Studio fügt `using System.ComponentModel.DataAnnotations;` hinzu.

Entfernen Sie nun die `using`-Anweisungen, die nicht benötigt werden. Sie werden standardmäßig in einer hellgrauen Schrift angezeigt. Klicken Sie mit der rechten Maus auf die Datei *Movie.cs* > **Using-Direktiven entfernen und sortieren**.

![Using-Direktiven entfernen und sortieren](controller-methods-views/_static/rm.png)

Der aktualisierte Code:

[!code-csharp[Main](./start-mvc/sample/MvcMovie/Models/MovieDate.cs?name=snippet_1)]

<!-- include start -->

[!INCLUDE[adding-model](../../includes/mvc-intro/controller-methods-views.md)]

>[!div class="step-by-step"]
[Zurück](working-with-sql.md)
[Weiter](search.md)  
