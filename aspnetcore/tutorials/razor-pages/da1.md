---
title: Aktualisieren der generierten Seiten in einer ASP.NET Core-App
author: rick-anderson
description: Erfahren Sie mehr über das Aktualisieren der generierten Seiten in einer ASP.NET Core-App.
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.date: 05/30/2018
uid: tutorials/razor-pages/da1
ms.openlocfilehash: 7633c0a40764cc18a656f0497e3280e4067cb59f
ms.sourcegitcommit: 317f9be24db600499e79d25872d743af74bd86c0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/02/2018
ms.locfileid: "48045574"
---
# <a name="update-the-generated-pages-in-an-aspnet-core-app"></a><span data-ttu-id="95e2a-103">Aktualisieren der generierten Seiten in einer ASP.NET Core-App</span><span class="sxs-lookup"><span data-stu-id="95e2a-103">Update the generated pages in an ASP.NET Core app</span></span>

<span data-ttu-id="95e2a-104">Von [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="95e2a-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="95e2a-105">Für den Anfang ist die Movie-App schon recht ansprechend, doch es gibt Raum für Verbesserungen.</span><span class="sxs-lookup"><span data-stu-id="95e2a-105">We have a good start to the movie app, but the presentation isn't ideal.</span></span> <span data-ttu-id="95e2a-106">Wir wollen die Uhrzeit nicht sehen (12:00:00 Uhr in der nachstehenden Abbildung), und **ReleaseDate** als soll **Release Date** (zwei Wörter) angezeigt werden.</span><span class="sxs-lookup"><span data-stu-id="95e2a-106">We don't want to see the time (12:00:00 AM in the image below) and **ReleaseDate** should be **Release Date** (two words).</span></span>

![In Chrome geöffnete Movie-Anwendung mit Filmdaten](sql/_static/m55.png)

## <a name="update-the-generated-code"></a><span data-ttu-id="95e2a-108">Aktualisieren des generierten Codes</span><span class="sxs-lookup"><span data-stu-id="95e2a-108">Update the generated code</span></span>

<span data-ttu-id="95e2a-109">Öffnen Sie die Datei *Models/Movie.cs*, und fügen Sie die im folgenden Code gezeigten markierten Zeilen hinzu:</span><span class="sxs-lookup"><span data-stu-id="95e2a-109">Open the *Models/Movie.cs* file and add the highlighted lines shown in the following code:</span></span>

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Models/MovieDate.cs?name=snippet_1&highlight=10-11)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie21/Models/MovieDate.cs?name=snippet_1&highlight=10-11,15)]

::: moniker-end

<span data-ttu-id="95e2a-110">Klicken Sie mit der rechten Maustaste auf eine rote Wellenlinie >  **Schnellaktionen und Refactorings**.</span><span class="sxs-lookup"><span data-stu-id="95e2a-110">Right click on a red squiggly line > **Quick Actions and Refactorings**.</span></span>

  ![Kontextmenü mit **> Schnellaktionen und Refactorings**.](da1/qa.png)

<span data-ttu-id="95e2a-112">Wählen Sie `using System.ComponentModel.DataAnnotations;` aus.</span><span class="sxs-lookup"><span data-stu-id="95e2a-112">Select `using System.ComponentModel.DataAnnotations;`</span></span>

  ![„using System.ComponentModel.DataAnnotations“ oben in der Liste](da1/da.png)

  <span data-ttu-id="95e2a-114">Visual Studio fügt `using System.ComponentModel.DataAnnotations;` hinzu.</span><span class="sxs-lookup"><span data-stu-id="95e2a-114">Visual Studio adds `using System.ComponentModel.DataAnnotations;`.</span></span>

[!INCLUDE [model1](~/includes/RP/da2.md)]

> [!div class="step-by-step"]
> <span data-ttu-id="95e2a-115">[Zurück: Arbeiten mit SQL Server LocalDB](xref:tutorials/razor-pages/sql)
> [Hinzufügen der Suche](xref:tutorials/razor-pages/search)</span><span class="sxs-lookup"><span data-stu-id="95e2a-115">[Previous: Working with SQL Server LocalDB](xref:tutorials/razor-pages/sql)
[Add search](xref:tutorials/razor-pages/search)</span></span>
