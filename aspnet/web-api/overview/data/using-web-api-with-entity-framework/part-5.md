---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-5
title: Erstellen Sie die Datenübertragungsobjekte (DTOs) | Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/16/2014
ms.topic: article
ms.assetid: 0fd07176-b74b-48f0-9fac-0f02e3ffa213
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-5
msc.type: authoredcontent
ms.openlocfilehash: 35e01f959072b96204de3e2ce3d507635720e110
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
---
<a name="create-data-transfer-objects-dtos"></a>Erstellen Sie die Datenübertragungsobjekte (DTOs)
====================
durch [Mike Wasson](https://github.com/MikeWasson)

[Herunterladen des abgeschlossenen Projekts](https://github.com/MikeWasson/BookService)

Rechts nun unsere Web-API macht Datenbankentitäten an den Client. Der Client empfängt Daten, die direkt an den Datenbanktabellen zugeordnet. Allerdings ist, die nicht immer empfehlenswert. In einigen Fällen möchten Sie die Form der Daten zu ändern, die an den Client gesendet. Auf diese Weise können Sie z. B. folgende Vorgänge durchführen:

- Zirkuläre Verweise entfernen (Siehe vorherige Abschnitt).
- Blenden Sie bestimmte Eigenschaften aus, die Clients nicht anzeigen.
- Lassen Sie einige Eigenschaften, um die Nutzlastgröße zu reduzieren.
- Vereinfachen von Objektdiagrammen, die geschachtelten Objekte, um sie für Clients erleichtern enthalten.
- Vermeiden Sie "stark Posten von Beiträgen" Sicherheitsrisiken. (Siehe [Modellvalidierung](../../formats-and-model-binding/model-validation-in-aspnet-web-api.md) Nähere Informationen zu senden.)
- Entkoppeln Sie die Dienstebene aus der Datenbankebene.

Um dies zu erreichen, können Sie definieren eine *Datenübertragungsobjekt* (DTO). Ein DTO ist ein Objekt, das definiert, wie die Daten über das Netzwerk gesendet werden. Sehen wir, die Funktionsweise der Book-Entität. Fügen Sie im Ordner Models zwei DTO Klassen hinzu:

[!code-csharp[Main](part-5/samples/sample1.cs)]

Die `BookDetailDTO` Klasse enthält alle Eigenschaften aus dem Buch-Modell, außer dass `AuthorName` ist eine Zeichenfolge, die den Namen des Autors enthalten soll. Die `BookDTO` Klasse enthält eine Teilmenge der Eigenschaften von `BookDetailDTO`.

Als Nächstes ersetzen Sie die beiden GET-Methoden in der `BooksController` -Klasse, mit Versionen, die DTOs zurückgeben. Wir verwenden die LINQ **wählen** Anweisung Buch von Entitäten in DTOs zu konvertieren.

[!code-csharp[Main](part-5/samples/sample2.cs)]

Hier wird die generiert wird, durch die neue SQL `GetBooks` Methode. Beachten Sie, dass EF LINQ übersetzt **wählen** in eine SQL SELECT-Anweisung.

[!code-sql[Main](part-5/samples/sample3.sql)]

Ändern Sie abschließend die `PostBook` Methode, um ein DTO zurückzugeben.

[!code-csharp[Main](part-5/samples/sample4.cs)]

> [!NOTE]
> In diesem Lernprogramm möchten wir in DTOs manuell im Code konvertieren. Eine andere Möglichkeit ist die Verwendung eine Bibliothek wie [AutoMapper](http://automapper.org/) , die die Konvertierung automatisch verarbeitet.
> 
> [!div class="step-by-step"]
> [Zurück](part-4.md)
> [Weiter](part-6.md)
