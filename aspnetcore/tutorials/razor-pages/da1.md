---
title: Aktualisieren der generierten Seiten in einer ASP.NET Core-App
author: rick-anderson
description: Erfahren Sie mehr über das Aktualisieren der generierten Seiten in einer ASP.NET Core-App.
manager: wpickett
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.date: 08/07/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/razor-pages/da1
ms.openlocfilehash: 5c188799b7a42bcd5e9d5eab8dfe8cdad8002fe5
ms.sourcegitcommit: c79fd3592f444d58e17518914f8873d0a11219c0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/18/2018
---
# <a name="update-the-generated-pages-in-an-aspnet-core-app"></a>Aktualisieren der generierten Seiten in einer ASP.NET Core-App

Von [Rick Anderson](https://twitter.com/RickAndMSFT)

Für den Anfang ist die Movie-App schon recht ansprechend, doch es gibt Raum für Verbesserungen. Wir wollen die Uhrzeit nicht sehen (12:00:00 Uhr in der nachstehenden Abbildung), und **ReleaseDate** als soll **Release Date** (zwei Wörter) angezeigt werden.

![In Chrome geöffnete Movie-Anwendung mit Filmdaten](sql/_static/m55.png)

## <a name="update-the-generated-code"></a>Aktualisieren des generierten Codes

Öffnen Sie die Datei *Models/Movie.cs*, und fügen Sie die im folgenden Code gezeigten markierten Zeilen hinzu:

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie/Models/MovieDate.cs?name=snippet_1&highlight=10-11)]

Klicken Sie mit der rechten Maustaste auf eine rote Wellenlinie > **Schnellaktionen und Refactorings**.

  ![Kontextmenü mit **> Schnellaktionen und Refactorings**.](da1/qa.png)

Wählen Sie `using System.ComponentModel.DataAnnotations;` aus.

  ![„using System.ComponentModel.DataAnnotations“ oben in der Liste](da1/da.png)

  Visual Studio fügt `using System.ComponentModel.DataAnnotations;` hinzu.

[!INCLUDE [model1](../../includes/RP/da2.md)]

> [!div class="step-by-step"]
> [Zurück: Arbeiten mit SQL Server LocalDB](xref:tutorials/razor-pages/sql)
> [Hinzufügen der Suche](xref:tutorials/razor-pages/search)
