---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-2
title: Hinzufügen von Modellen und Controller | Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/16/2014
ms.topic: article
ms.assetid: 88908ff8-51a9-40eb-931c-a8139128b680
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-2
msc.type: authoredcontent
ms.openlocfilehash: 015bb9698d81387d03ea8f9629316fb53232e708
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
---
<a name="add-models-and-controllers"></a>Hinzufügen von Modellen und Controllern
====================
durch [Mike Wasson](https://github.com/MikeWasson)

[Herunterladen des abgeschlossenen Projekts](https://github.com/MikeWasson/BookService)

In diesem Abschnitt fügen Sie ein Modellklassen, die die Datenbankentitäten zu definieren. Dann können Sie Web-API-Controller hinzufügen, die CRUD-Vorgänge für die Entitäten ausführen.

## <a name="add-model-classes"></a>Hinzufügen von Modellklassen

In diesem Lernprogramm erstellen wir die Datenbank mithilfe des "Code First" Ansatzes zu Entity Framework (EF). Mit Code First Schreiben von C#-Klassen, die entsprechen an den Datenbanktabellen und EF erstellt die Datenbank. (Weitere Informationen finden Sie unter [Ansätze zur Entity Framework-Entwicklung](https://msdn.microsoft.com/library/ms178359%28v=vs.110%29.aspx#dbfmfcf).)

Beginnen wir unsere Domänenobjekte als POCOs (Plain-Old CLR Objects) definieren. Wir werden die folgenden POCOs erstellen:

- Autor
- Adressbuch

Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf Ordner Models. Wählen Sie **hinzufügen**, und wählen Sie dann **Klasse**. Nennen Sie die Klasse `Author`.

![](part-2/_static/image1.png)

Ersetzen Sie alle Standardcode in Author.cs durch den folgenden Code ein.

[!code-csharp[Main](part-2/samples/sample1.cs)]

Fügen Sie eine andere Klasse, die mit dem Namen `Book`, durch den folgenden Code.

[!code-csharp[Main](part-2/samples/sample2.cs)]

Entity Framework werden diese Modelle verwenden, um Tabellen zu erstellen. Für jedes Modell die `Id` Eigenschaft werden die Primärschlüsselspalte der Datenbanktabelle.

In der Book-Klasse die `AuthorId` definiert einen Fremdschlüssel in der `Author` Tabelle. (Aus Gründen der Einfachheit, habe ich vorausgesetzt, dass jedes Buch der Autor ein einzelnes hat.) Die Book-Klasse enthält außerdem eine Navigationseigenschaft, die verwandte `Author`. Können Sie den Zugriff auf die Navigationseigenschaft `Author` im Code. Ich mehr über Navigationseigenschaften in Teil 4, sagen Sie [Behandlung von Entitätsbeziehungen](part-4.md).

## <a name="add-web-api-controllers"></a>Hinzufügen von Web-API-Controller

In diesem Abschnitt fügen wir Web-API-Controller, die CRUD-Vorgänge zu unterstützen (erstellen, lesen, aktualisieren und löschen). Controller werden Entity Framework verwendet, für die Kommunikation mit der Datenbankebene.

Erstens können Sie die Datei Controllers/ValuesController.cs löschen. Diese Datei enthält einen Beispiel für Web-API-Controller, jedoch ist dies nicht erforderlich für dieses Lernprogramm.

![](part-2/_static/image2.png)

Als Nächstes erstellen Sie das Projekt. Das Gerüst für die Web-API verwendet Reflektion, um die Modellklassen zu finden, damit sie die kompilierte Assembly benötigt.

Im Projektmappen-Explorer mit der rechten Maustaste des Ordners Controllers. Wählen Sie **hinzufügen**, und wählen Sie dann **Controller**.

![](part-2/_static/image3.png)

In der **Gerüst hinzufügen** wählen Sie im Dialogfeld "Web-API 2-Controller mit Aktionen unter Verwendung von Entity Framework". Klicken Sie auf **Hinzufügen**.

![](part-2/_static/image4.png)

In der **Controller hinzufügen** Dialogfeld, gehen Sie folgendermaßen vor:

1. In der **Modellschemas** Dropdownliste, wählen die `Author` Klasse. (Wenn Sie in der Dropdownliste aufgeführt nicht angezeigt wird, stellen Sie sicher, dass Sie das Projekt erstellt.)
2. Überprüfen Sie "Async-Controlleraktionen verwenden".
3. Lassen Sie den Namen des Controllers als &quot;AuthorsController&quot;.
4. Klicken Sie auf Pluszeichen (+) neben Schaltfläche **Datenkontextklasse**.

![](part-2/_static/image5.png)

In der **neuen Datenkontext** Dialogfeld, lassen Sie den Standardnamen, und klicken Sie auf **hinzufügen**.

![](part-2/_static/image6.png)

Klicken Sie auf **hinzufügen** zum Abschließen der **Controller hinzufügen** Dialogfeld. Das Dialogfeld fügt zwei Klassen zu Ihrem Projekt hinzu:

- `AuthorsController` definiert einen Web-API-Controller. Der Controller implementiert die REST-API, mit denen Clients CRUD-Vorgänge in der Liste der Autoren ausführen.
- `BookServiceContext` verwaltet von Entitätsobjekten während der Laufzeit, Auffüllen mit Daten aus einer Datenbank, Nachverfolgen von Änderungen und Beibehalten von Daten in die Datenbank Objekte enthält. Es erbt von `DbContext`.

![](part-2/_static/image7.png)

Nun erstellen Sie das Projekt erneut. Kehren Sie nun über die gleichen Schritte zum Hinzufügen von eines API-Controllers für `Book` Entitäten. Wählen Sie dieses Mal `Book` für die Modellklasse, und wählen Sie die vorhandene `BookServiceContext` Datenkontextklasse-Klasse. (Erstellen Sie keinen neuen Datenkontext.) Klicken Sie auf **hinzufügen** Controller hinzufügen.

![](part-2/_static/image8.png)

> [!div class="step-by-step"]
> [Zurück](part-1.md)
> [Weiter](part-3.md)
