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
# <a name="update-the-generated-pages-in-an-aspnet-core-app"></a>Aktualisieren der generierten Seiten in einer ASP.NET Core-App

Von [Rick Anderson](https://twitter.com/RickAndMSFT)

Für den Anfang ist die Movie-App schon recht ansprechend, doch es gibt Raum für Verbesserungen. Wir wollen die Uhrzeit nicht sehen (12:00:00 Uhr in der nachstehenden Abbildung), und **ReleaseDate** als soll **Release Date** (zwei Wörter) angezeigt werden.

![In Chrome geöffnete Movie-Anwendung mit Filmdaten](sql/_static/m55.png)

## <a name="update-the-generated-code"></a>Aktualisieren des generierten Codes

Öffnen Sie die Datei *Models/Movie.cs*, und fügen Sie die im folgenden Code gezeigten markierten Zeilen hinzu:

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Models/MovieDate.cs?name=snippet_1&highlight=10-11)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie21/Models/MovieDate.cs?name=snippet_1&highlight=10-11,15)]

::: moniker-end

Klicken Sie mit der rechten Maustaste auf eine rote Wellenlinie >  **Schnellaktionen und Refactorings**.

  ![Kontextmenü mit **> Schnellaktionen und Refactorings**.](da1/qa.png)

Wählen Sie `using System.ComponentModel.DataAnnotations;` aus.

  ![„using System.ComponentModel.DataAnnotations“ oben in der Liste](da1/da.png)

  Visual Studio fügt `using System.ComponentModel.DataAnnotations;` hinzu.

[!INCLUDE [model1](~/includes/RP/da2.md)]

> [!div class="step-by-step"]
> [Zurück: Arbeiten mit SQL Server LocalDB](xref:tutorials/razor-pages/sql)
> [Hinzufügen der Suche](xref:tutorials/razor-pages/search)
