---
title: Controllermethoden und -ansichten
author: rick-anderson
description: Arbeiten mit Controllermethoden, Ansichten und DataAnnotations
keywords: ASP.NET Core,
ms.author: riande
manager: wpickett
ms.date: 04/07/2017
ms.topic: get-started-article
ms.assetid: c7313211-bb71-4adf-babe-8e72603cc0ce
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/first-mvc-app-xplat/controller-methods-views
ms.openlocfilehash: b72fb5077d3bd577cd6282e7a6231a0806ee1fd2
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
# <a name="controller-methods-and-views"></a><span data-ttu-id="53870-104">Controllermethoden und -ansichten</span><span class="sxs-lookup"><span data-stu-id="53870-104">Controller methods and views</span></span>

<span data-ttu-id="53870-105">Von [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="53870-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="53870-106">Die „movie“-App ist schon recht ansprechend, doch die Präsentation ist nicht ideal.</span><span class="sxs-lookup"><span data-stu-id="53870-106">We have a good start to the movie app, but the presentation is not ideal.</span></span> <span data-ttu-id="53870-107">Wir wollen die Uhrzeit nicht sehen (12:00:00 Uhr in der nachstehenden Abbildung), und **ReleaseDate** soll als zwei Wörter angezeigt werden.</span><span class="sxs-lookup"><span data-stu-id="53870-107">We don't want to see the time (12:00:00 AM in the image below) and **ReleaseDate** should be two words.</span></span>

![Indexansicht: „ReleaseDate“ ist ein Wort (ohne Leerzeichen), und bei jedem Veröffentlichungsdatum wird die Uhrzeit 12: 00 Uhr angezeigt](../../tutorials/first-mvc-app/working-with-sql/_static/m55.png)

<span data-ttu-id="53870-109">Öffnen Sie die Datei *Models/Movie.cs*, und fügen Sie die nachfolgend gezeigten markierten Zeilen hinzu:</span><span class="sxs-lookup"><span data-stu-id="53870-109">Open the *Models/Movie.cs* file and add the highlighted lines shown below:</span></span>

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/MovieDate.cs?name=snippet_1&highlight=2,11-12)]

<span data-ttu-id="53870-110">Erstellen Sie die App, und führen Sie sie aus.</span><span class="sxs-lookup"><span data-stu-id="53870-110">Build and run the app.</span></span>

<!-- include start
![MVC Movie application open browser showing movie data](../../tutorials/first-mvc-app/working-with-sql/_static/m55.png)

 -->

[!INCLUDE[adding-model](../../includes/mvc-intro/controller-methods-views.md)]

>[!div class="step-by-step"]
<span data-ttu-id="53870-111">[Zurück: Arbeiten mit SQLite](working-with-sql.md)
[Weiter: Hinzufügen der Suche](search.md)</span><span class="sxs-lookup"><span data-stu-id="53870-111">[Previous - Working with SQLite](working-with-sql.md)
[Next - Add search](search.md)</span></span>  
