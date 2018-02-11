---
title: Aktualisieren der generierten Seiten
author: rick-anderson
description: Aktualisieren Sie die generierten Seiten mit besserer Anzeige.
manager: wpickett
ms.author: riande
ms.date: 08/07/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/razor-pages/da1
ms.openlocfilehash: a1bb1ab1e4fac9c634f4048947ac3f934af3d625
ms.sourcegitcommit: 18d1dc86770f2e272d93c7e1cddfc095c5995d9e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2018
---
# <a name="update-the-generated-pages"></a><span data-ttu-id="a97fb-103">Aktualisieren der generierten Seiten</span><span class="sxs-lookup"><span data-stu-id="a97fb-103">Update the generated pages</span></span>

<span data-ttu-id="a97fb-104">Von [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="a97fb-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="a97fb-105">Für den Anfang ist die Movie-App schon recht ansprechend, doch es gibt Raum für Verbesserungen.</span><span class="sxs-lookup"><span data-stu-id="a97fb-105">We have a good start to the movie app, but the presentation isn't ideal.</span></span> <span data-ttu-id="a97fb-106">Wir wollen die Uhrzeit nicht sehen (12:00:00 Uhr in der nachstehenden Abbildung), und **ReleaseDate** als soll **Release Date** (zwei Wörter) angezeigt werden.</span><span class="sxs-lookup"><span data-stu-id="a97fb-106">We don't want to see the time (12:00:00 AM in the image below) and **ReleaseDate** should be **Release Date** (two words).</span></span>

![In Chrome geöffnete Movie-Anwendung mit Filmdaten](sql/_static/m55.png)

## <a name="update-the-generated-code"></a><span data-ttu-id="a97fb-108">Aktualisieren des generierten Codes</span><span class="sxs-lookup"><span data-stu-id="a97fb-108">Update the generated code</span></span>

<span data-ttu-id="a97fb-109">Öffnen Sie die Datei *Models/Movie.cs*, und fügen Sie die im folgenden Code gezeigten markierten Zeilen hinzu:</span><span class="sxs-lookup"><span data-stu-id="a97fb-109">Open the *Models/Movie.cs* file and add the highlighted lines shown in the following code:</span></span>

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Models/MovieDate.cs?name=snippet_1&highlight=10-11)]

<span data-ttu-id="a97fb-110">Klicken Sie mit der rechten Maustaste auf eine rote Wellenlinie > **Schnellaktionen und Refactorings**.</span><span class="sxs-lookup"><span data-stu-id="a97fb-110">Right click on a red squiggly line > ** Quick Actions and Refactorings**.</span></span>

  ![Kontextmenü mit **> Schnellaktionen und Refactorings**.](da1/qa.png)

<span data-ttu-id="a97fb-112">Wählen Sie `using System.ComponentModel.DataAnnotations;` aus.</span><span class="sxs-lookup"><span data-stu-id="a97fb-112">Select `using System.ComponentModel.DataAnnotations;`</span></span>

  ![„using System.ComponentModel.DataAnnotations“ oben in der Liste](da1/da.png)

  <span data-ttu-id="a97fb-114">Visual Studio fügt `using System.ComponentModel.DataAnnotations;` hinzu.</span><span class="sxs-lookup"><span data-stu-id="a97fb-114">Visual studio adds `using System.ComponentModel.DataAnnotations;`.</span></span>

[!INCLUDE[model1](../../includes/RP/da2.md)]

>[!div class="step-by-step"]
<span data-ttu-id="a97fb-115">[Zurück: Arbeiten mit SQL Server LocalDB](xref:tutorials/razor-pages/sql)
[Hinzufügen der Suche](xref:tutorials/razor-pages/search)</span><span class="sxs-lookup"><span data-stu-id="a97fb-115">[Previous: Working with SQL Server LocalDB](xref:tutorials/razor-pages/sql)
[Adding Search](xref:tutorials/razor-pages/search)</span></span>
