---
title: Controllermethoden und -ansichten
author: rick-anderson
description: Arbeiten mit Controllermethoden, Ansichten und DataAnnotations
keywords: ASP.NET Core,
ms.author: riande
manager: wpickett
ms.date: 03/07/2017
ms.topic: get-started-article
ms.assetid: c7313211-b271-4adf-bab8-8e72603cc0ce
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/first-mvc-app/controller-methods-views
ms.openlocfilehash: 8960853438d3227de3ef7c50936626149d8d5997
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/11/2017
---
# <a name="controller-methods-and-views"></a>Controllermethoden und -ansichten

Von [Rick Anderson](https://twitter.com/RickAndMSFT)

Die „movie“-App ist schon recht ansprechend, doch die Präsentation ist nicht ideal. Wir wollen die Uhrzeit nicht sehen (12:00:00 Uhr in der nachstehenden Abbildung), und **ReleaseDate** soll als zwei Wörter angezeigt werden.

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
