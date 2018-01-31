---
title: Controllermethoden und -ansichten
author: rick-anderson
description: Arbeiten mit Controllermethoden, Ansichten und DataAnnotations
manager: wpickett
ms.author: riande
ms.date: 03/07/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/first-mvc-app/controller-methods-views
ms.openlocfilehash: 200f02f9815966653b3b46918737c60d11f11d5a
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2018
---
# <a name="controller-methods-and-views"></a><span data-ttu-id="5164c-103">Controllermethoden und -ansichten</span><span class="sxs-lookup"><span data-stu-id="5164c-103">Controller methods and views</span></span>

<span data-ttu-id="5164c-104">Von [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="5164c-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="5164c-105">Für den Anfang ist die Movie-App schon recht ansprechend, doch es gibt Raum für Verbesserungen.</span><span class="sxs-lookup"><span data-stu-id="5164c-105">We have a good start to the movie app, but the presentation isn't ideal.</span></span> <span data-ttu-id="5164c-106">Wir wollen die Uhrzeit nicht sehen (12:00:00 Uhr in der nachstehenden Abbildung), und **ReleaseDate** soll als zwei Wörter angezeigt werden.</span><span class="sxs-lookup"><span data-stu-id="5164c-106">We don't want to see the time (12:00:00 AM in the image below) and **ReleaseDate** should be two words.</span></span>

![Indexansicht: „ReleaseDate“ ist ein Wort (ohne Leerzeichen), und bei jedem Veröffentlichungsdatum wird die Uhrzeit 12: 00 Uhr angezeigt](working-with-sql/_static/m55.png)

<span data-ttu-id="5164c-108">Öffnen Sie die Datei *Models/Movie.cs*, und fügen Sie die nachfolgend gezeigten markierten Zeilen hinzu:</span><span class="sxs-lookup"><span data-stu-id="5164c-108">Open the *Models/Movie.cs* file and add the highlighted lines shown below:</span></span>

[!code-csharp[Main](start-mvc/sample/MvcMovie/Models/MovieDateWithExtraUsings.cs?name=snippet_1&highlight=13-14)]

<span data-ttu-id="5164c-109">Klicken Sie mit der rechten Maustaste auf eine rote Wellenlinie **> Schnellaktionen und Refactorings**.</span><span class="sxs-lookup"><span data-stu-id="5164c-109">Right click on a red squiggly line **> Quick Actions and Refactorings**.</span></span>

  ![Kontextmenü mit **> Schnellaktionen und Refactorings**.](controller-methods-views/_static/qa.png)


<span data-ttu-id="5164c-111">Tippen Sie auf `using System.ComponentModel.DataAnnotations;`</span><span class="sxs-lookup"><span data-stu-id="5164c-111">Tap `using System.ComponentModel.DataAnnotations;`</span></span>

  ![„using System.ComponentModel.DataAnnotations“ oben in der Liste](controller-methods-views/_static/da.png)

  <span data-ttu-id="5164c-113">Visual Studio fügt `using System.ComponentModel.DataAnnotations;` hinzu.</span><span class="sxs-lookup"><span data-stu-id="5164c-113">Visual studio adds `using System.ComponentModel.DataAnnotations;`.</span></span>

<span data-ttu-id="5164c-114">Entfernen Sie nun die `using`-Anweisungen, die nicht benötigt werden.</span><span class="sxs-lookup"><span data-stu-id="5164c-114">Let's remove the `using` statements that are not needed.</span></span> <span data-ttu-id="5164c-115">Sie werden standardmäßig in einer hellgrauen Schrift angezeigt.</span><span class="sxs-lookup"><span data-stu-id="5164c-115">They show up by default in a light grey font.</span></span> <span data-ttu-id="5164c-116">Klicken Sie mit der rechten Maus auf die Datei *Movie.cs* > **Using-Direktiven entfernen und sortieren**.</span><span class="sxs-lookup"><span data-stu-id="5164c-116">Right click anywhere in the *Movie.cs* file **> Remove and Sort Usings**.</span></span>

![Using-Direktiven entfernen und sortieren](controller-methods-views/_static/rm.png)

<span data-ttu-id="5164c-118">Der aktualisierte Code:</span><span class="sxs-lookup"><span data-stu-id="5164c-118">The updated code:</span></span>

[!code-csharp[Main](./start-mvc/sample/MvcMovie/Models/MovieDate.cs?name=snippet_1)]

<!-- include start -->

[!INCLUDE[adding-model](../../includes/mvc-intro/controller-methods-views.md)]

>[!div class="step-by-step"]
<span data-ttu-id="5164c-119">[Zurück](working-with-sql.md)
[Weiter](search.md)</span><span class="sxs-lookup"><span data-stu-id="5164c-119">[Previous](working-with-sql.md)
[Next](search.md)</span></span>  
