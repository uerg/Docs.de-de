---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-2
title: Hinzufügen von Modellen und Controllern | Microsoft-Dokumentation
author: MikeWasson
description: ''
ms.author: riande
ms.date: 06/16/2014
ms.assetid: 88908ff8-51a9-40eb-931c-a8139128b680
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-2
msc.type: authoredcontent
ms.openlocfilehash: 162ef2cd4ba11040e1bc938617a36495489ba5bc
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2018
ms.locfileid: "41834862"
---
<a name="add-models-and-controllers"></a>Hinzufügen von Modellen und Controllern
====================
durch [Mike Wasson](https://github.com/MikeWasson)

[Abgeschlossenes Projekt herunterladen](https://github.com/MikeWasson/BookService)

In diesem Abschnitt fügen Sie ViewModel-Klassen, die die Datenbankentitäten zu definieren. Dann können Sie Web-API-Controller hinzufügen, die CRUD-Vorgänge für diese Entitäten ausführen.

## <a name="add-model-classes"></a>Hinzufügen von Modellklassen

In diesem Tutorial erstellen wir die Datenbank mithilfe des "Code First"-Ansatzes zu Entity Framework (EF). Klicken Sie mit Code First Sie schreiben C#-Klassen, die entsprechen, Datenbanktabellen, und EF erstellt die Datenbank. (Weitere Informationen finden Sie unter [Entity Framework-Webentwicklungsansätzen](https://msdn.microsoft.com/library/ms178359%28v=vs.110%29.aspx#dbfmfcf).)

Definieren zunächst wir unser Domänenobjekte als POCOs (Plain-Old CLR Objects). Wir erstellen die folgenden POCOs:

- Autor
- Buch

Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf den Ordner "Models". Wählen Sie **hinzufügen**, und wählen Sie dann **Klasse**. Nennen Sie die Klasse `Author`.

![](part-2/_static/image1.png)

Ersetzen Sie alle die Codebausteine in Author.cs durch den folgenden Code.

[!code-csharp[Main](part-2/samples/sample1.cs)]

Fügen Sie eine andere Klasse, die mit dem Namen `Book`, durch den folgenden Code.

[!code-csharp[Main](part-2/samples/sample2.cs)]

Entitätsframework verwendet diese Modelle zum Erstellen von Tabellen. Für jedes Modell das `Id` Eigenschaft werden die Primärschlüsselspalte der Datenbanktabelle.

In der Book-Klasse die `AuthorId` definiert einen Fremdschlüssel in der `Author` Tabelle. (Der Einfachheit halber habe ich vorausgesetzt, dass jedes Buch einen einzelnen Autor hat.) Die Buch-Klasse enthält auch eine Navigationseigenschaft für den zugehörigen `Author`. Können Sie den entsprechenden Zugriff auf die Navigationseigenschaft `Author` im Code. Weitere Informationen zu Navigationseigenschaften in Teil 4, sagen [Verarbeiten von Entitätsbeziehungen](part-4.md).

## <a name="add-web-api-controllers"></a>Hinzufügen von Web-API-Controllern

In diesem Abschnitt fügen wir Web-API-Controller, die CRUD-Vorgänge zu unterstützen (erstellen, lesen, aktualisieren und löschen). Controller werden Entity Framework verwenden, um mit der Datenbankebene kommunizieren zu können.

Erstens können Sie die Datei Controllers/ValuesController.cs löschen. Diese Datei enthält einen Beispiel-Web-API-Controller, aber nicht für dieses Tutorial erforderlich.

![](part-2/_static/image2.png)

Als Nächstes erstellen Sie das Projekt ein. Das Web-API-Gerüst verwendet Reflektion, um die Modellklassen gefunden, daher muss die kompilierte Assembly festgelegt.

Klicken Sie im Projektmappen-Explorer den Ordner "Controllers". Wählen Sie **hinzufügen**, und wählen Sie dann **Controller**.

![](part-2/_static/image3.png)

In der **Gerüst hinzufügen** wählen Sie im Dialogfeld "Web-API 2-Controller mit Aktionen unter Verwendung von Entitätsframework". Klicken Sie auf **Hinzufügen**.

![](part-2/_static/image4.png)

In der **Controller hinzufügen** Dialogfeld, gehen Sie folgendermaßen vor:

1. In der **Modellklasse** Dropdownliste, wählen die `Author` Klasse. (Wenn Sie nicht, dass es in der Dropdownliste aufgeführt sehen, stellen Sie sicher, dass Sie das Projekt erstellt.)
2. Überprüfen Sie "Async-Controlleraktionen verwenden".
3. Lassen Sie den Namen der Controller als &quot;"authorscontroller"&quot;.
4. Klicken Sie auf Pluszeichen (+) neben Schaltfläche **Datenkontextklasse**.

![](part-2/_static/image5.png)

In der **neuer Datenkontext** Dialogfeld, übernehmen Sie den Standardnamen, und klicken Sie auf **hinzufügen**.

![](part-2/_static/image6.png)

Klicken Sie auf **hinzufügen** zum Abschließen der **Controller hinzufügen** Dialogfeld. Das Dialogfeld fügt zwei Klassen zu Ihrem Projekt hinzu:

- `AuthorsController` definiert einen Web-API-Controller. Der Controller implementiert die REST-API, die Clients zum Ausführen von CRUD-Vorgänge in der Liste der Autoren verwenden.
- `BookServiceContext` Entitätsobjekte verwaltet während der Laufzeit, einschließlich ausfüllenden Objekten mit Daten aus einer Datenbank, Nachverfolgen von Änderungen und Beibehalten von Daten in der Datenbank. Sie erbt von `DbContext`.

![](part-2/_static/image7.png)

An diesem Punkt erstellen Sie das Projekt erneut. Kehren Sie nun über die gleichen Schritte zum Hinzufügen von eines API-Controllers für `Book` Entitäten. Wählen Sie dieses Mal `Book` für die Model-Klasse, und wählen Sie die vorhandene `BookServiceContext` -Klasse für das die Datenkontextklasse. (Erstellen Sie keinen neuen Datenkontext.) Klicken Sie auf **hinzufügen** Controller hinzufügen.

![](part-2/_static/image8.png)

> [!div class="step-by-step"]
> [Zurück](part-1.md)
> [Weiter](part-3.md)
