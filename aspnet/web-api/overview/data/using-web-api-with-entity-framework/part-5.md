---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-5
title: Erstellen Sie die Datenübertragungsobjekte (DTOs) | Microsoft-Dokumentation
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/16/2014
ms.topic: article
ms.assetid: 0fd07176-b74b-48f0-9fac-0f02e3ffa213
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-5
msc.type: authoredcontent
ms.openlocfilehash: 6a066f1aca909afc2956e2026d9025cb87f10b1f
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/03/2018
ms.locfileid: "37399924"
---
<a name="create-data-transfer-objects-dtos"></a>Erstellen Sie die Datenübertragungsobjekte (DTOs)
====================
durch [Mike Wasson](https://github.com/MikeWasson)

[Abgeschlossenes Projekt herunterladen](https://github.com/MikeWasson/BookService)

Die Datenbankentitäten an den Client, macht rechts nun unsere Web-API verfügbar. Der Client empfängt Daten, die direkt von Datenbanktabellen zugeordnet. Allerdings ist, die nicht immer eine gute Idee. Manchmal möchten Sie die Form der Daten zu ändern, die an den Client gesendet. Auf diese Weise können Sie z. B. folgende Vorgänge durchführen:

- Zirkuläre Verweise entfernen (Siehe vorheriger Abschnitt).
- Blenden Sie bestimmte Eigenschaften, die Clients sollten nicht anzeigen.
- Lassen Sie einige Eigenschaften Weg, um die Nutzlastgröße zu reduzieren.
- Vereinfachen von Objektdiagrammen, die geschachtelte Objekte, um die bequemer für Clients zu enthalten.
- Vermeiden Sie "overposting" Sicherheitsrisiken. (Finden Sie unter [Modellvalidierung](../../formats-and-model-binding/model-validation-in-aspnet-web-api.md) eine Erläuterung der overposting.)
- Entkoppeln von Ihrer Dienstebene aus der Datenbankebene.

Um dies zu erreichen, können Sie definieren eine *Datenübertragungsobjekt* (DTO). Ein DTO ist ein Objekt, das definiert, wie die Daten über das Netzwerk gesendet werden. Sehen wir uns an, wie dies mit der Entität Buch funktioniert. Fügen Sie in den Ordner "Models" zwei DTO-Klassen hinzu:

[!code-csharp[Main](part-5/samples/sample1.cs)]

Die `BookDetailDTO` Klasse enthält alle Eigenschaften aus dem Buch-Modell, außer dass `AuthorName` ist eine Zeichenfolge, die der Name des Autors enthalten wird. Die `BookDTO` -Klasse enthält eine Teilmenge der Eigenschaften von `BookDetailDTO`.

Ersetzen Sie als Nächstes die beiden GET-Methoden in der `BooksController` Klasse, die mit Versionen, die DTOs zurückgeben. Wir verwenden die LINQ **wählen** -Anweisung von buchentitäten in DTOs zu konvertieren.

[!code-csharp[Main](part-5/samples/sample2.cs)]

Hier ist die SQL-generiert, durch die neue `GetBooks` Methode. Sie sehen, dass EF LINQ übersetzt **wählen** in eine SQL-SELECT-Anweisung.

[!code-sql[Main](part-5/samples/sample3.sql)]

Ändern Sie abschließend die `PostBook` Methode, um ein DTO zurückzugeben.

[!code-csharp[Main](part-5/samples/sample4.cs)]

> [!NOTE]
> In diesem Tutorial konvertieren zu DTOs manuell im Code wir. Eine weitere Möglichkeit ist die Verwendung von einer Bibliothek wie [AutoMapper](http://automapper.org/) , die die Konvertierung automatisch verarbeitet.
> 
> [!div class="step-by-step"]
> [Zurück](part-4.md)
> [Weiter](part-6.md)
