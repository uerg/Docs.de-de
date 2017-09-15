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
# <a name="controller-methods-and-views"></a><span data-ttu-id="13d98-104">Controllermethoden und -ansichten</span><span class="sxs-lookup"><span data-stu-id="13d98-104">Controller methods and views</span></span>

<span data-ttu-id="13d98-105">Von [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="13d98-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="13d98-106">Die „movie“-App ist schon recht ansprechend, doch die Präsentation ist nicht ideal.</span><span class="sxs-lookup"><span data-stu-id="13d98-106">We have a good start to the movie app, but the presentation is not ideal.</span></span> <span data-ttu-id="13d98-107">Wir wollen die Uhrzeit nicht sehen (12:00:00 Uhr in der nachstehenden Abbildung), und **ReleaseDate** soll als zwei Wörter angezeigt werden.</span><span class="sxs-lookup"><span data-stu-id="13d98-107">We don't want to see the time (12:00:00 AM in the image below) and **ReleaseDate** should be two words.</span></span>

![Indexansicht: „ReleaseDate“ ist ein Wort (ohne Leerzeichen), und bei jedem Veröffentlichungsdatum wird die Uhrzeit 12: 00 Uhr angezeigt](working-with-sql/_static/m55.png)

<span data-ttu-id="13d98-109">Öffnen Sie die Datei *Models/Movie.cs*, und fügen Sie die nachfolgend gezeigten markierten Zeilen hinzu:</span><span class="sxs-lookup"><span data-stu-id="13d98-109">Open the *Models/Movie.cs* file and add the highlighted lines shown below:</span></span>

<span data-ttu-id="13d98-110">[!code-csharp[Main](start-mvc/sample/MvcMovie/Models/MovieDateWithExtraUsings.cs?name=snippet_1&highlight=13-14)]</span><span class="sxs-lookup"><span data-stu-id="13d98-110">[!code-csharp[Main](start-mvc/sample/MvcMovie/Models/MovieDateWithExtraUsings.cs?name=snippet_1&highlight=13-14)]</span></span>

<span data-ttu-id="13d98-111">Klicken Sie mit der rechten Maustaste auf eine rote Wellenlinie **> Schnellaktionen und Refactorings**.</span><span class="sxs-lookup"><span data-stu-id="13d98-111">Right click on a red squiggly line **> Quick Actions and Refactorings**.</span></span>

  ![Kontextmenü mit **> Schnellaktionen und Refactorings**.](controller-methods-views/_static/qa.png)


<span data-ttu-id="13d98-113">Tippen Sie auf `using System.ComponentModel.DataAnnotations;`</span><span class="sxs-lookup"><span data-stu-id="13d98-113">Tap `using System.ComponentModel.DataAnnotations;`</span></span>

  ![„using System.ComponentModel.DataAnnotations“ oben in der Liste](controller-methods-views/_static/da.png)

  <span data-ttu-id="13d98-115">Visual Studio fügt `using System.ComponentModel.DataAnnotations;` hinzu.</span><span class="sxs-lookup"><span data-stu-id="13d98-115">Visual studio adds `using System.ComponentModel.DataAnnotations;`.</span></span>

<span data-ttu-id="13d98-116">Entfernen Sie nun die `using`-Anweisungen, die nicht benötigt werden.</span><span class="sxs-lookup"><span data-stu-id="13d98-116">Let's remove the `using` statements that are not needed.</span></span> <span data-ttu-id="13d98-117">Sie werden standardmäßig in einer hellgrauen Schrift angezeigt.</span><span class="sxs-lookup"><span data-stu-id="13d98-117">They show up by default in a light grey font.</span></span> <span data-ttu-id="13d98-118">Klicken Sie mit der rechten Maus auf die Datei *Movie.cs* > **Using-Direktiven entfernen und sortieren**.</span><span class="sxs-lookup"><span data-stu-id="13d98-118">Right click anywhere in the *Movie.cs* file **> Remove and Sort Usings**.</span></span>

![Using-Direktiven entfernen und sortieren](controller-methods-views/_static/rm.png)

<span data-ttu-id="13d98-120">Der aktualisierte Code:</span><span class="sxs-lookup"><span data-stu-id="13d98-120">The updated code:</span></span>

<span data-ttu-id="13d98-121">[!code-csharp[Main](./start-mvc/sample/MvcMovie/Models/MovieDate.cs?name=snippet_1)]</span><span class="sxs-lookup"><span data-stu-id="13d98-121">[!code-csharp[Main](./start-mvc/sample/MvcMovie/Models/MovieDate.cs?name=snippet_1)]</span></span>

<!-- include start -->

[!INCLUDE[adding-model](../../includes/mvc-intro/controller-methods-views.md)]

>[!div class="step-by-step"]
<span data-ttu-id="13d98-122">[Zurück](working-with-sql.md)
[Weiter](search.md)</span><span class="sxs-lookup"><span data-stu-id="13d98-122">[Previous](working-with-sql.md)
[Next](search.md)</span></span>  
