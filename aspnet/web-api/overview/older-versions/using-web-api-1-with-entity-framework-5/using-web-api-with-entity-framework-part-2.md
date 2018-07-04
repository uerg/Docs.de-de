---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-2
title: 'Teil 2: Erstellen der Domänenmodelle | Microsoft-Dokumentation'
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/03/2012
ms.topic: article
ms.assetid: fe3ef85f-bdc6-4e10-9768-25aa565c01d0
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-2
msc.type: authoredcontent
ms.openlocfilehash: 9f379c92f7795c9fe10f7860b72188a8cfc1b6d2
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/03/2018
ms.locfileid: "37379724"
---
<a name="part-2-creating-the-domain-models"></a>Teil 2: Erstellen der Domänenmodelle
====================
durch [Mike Wasson](https://github.com/MikeWasson)

[Abgeschlossenes Projekt herunterladen](http://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

## <a name="add-models"></a>Hinzufügen von Modellen

Es gibt drei Möglichkeiten, um den Ansatz von Entity Framework:

- Database First: beginnen Sie mit einer Datenbank und Entity Framework generiert den Code.
- Model First: beginnen Sie mit einem visual Modell und Entity Framework generiert die Datenbank und den Code.
- Code First: beginnen Sie mit Code und Entity Framework generiert die Datenbank.

Wir verwenden die Code First-Ansatz, damit unsere Domänenobjekte als POCOs (Plain-Old CLR Objects) definieren wir zunächst. Mit dem Code-First-Ansatz erforderlich nicht Domänenobjekte zusätzlichen Code zur Unterstützung der Datenbankebene, z. B. Transaktionen oder Persistenz. (Insbesondere, sie müssen nicht das erben die ["EntityObject"](https://msdn.microsoft.com/library/system.data.objects.dataclasses.entityobject.aspx) Klasse.) Sie können weiterhin datenanmerkungen verwenden, um zu steuern, wie Entity Framework für das Schema der Datenbank erstellt.

Da POCOs keine zusätzlichen Eigenschaften enthalten, die beschreiben, [Datenbankstatus](https://msdn.microsoft.com/library/system.data.entitystate.aspx), sie können ganz einfach JSON oder XML serialisiert werden. Allerdings bedeutet dies nicht immer die Entity Framework-Modelle direkt an die Clients sollten verfügbar machen, wie wir später in diesem Lernprogramm sehen werden.

Wir erstellen die folgenden POCOs:

- Produkt
- Reihenfolge
- OrderDetail

Um jede Klasse zu erstellen, und mit der rechten Maustaste im Projektmappen-Explorer den Ordner "Models". Wählen Sie im Kontextmenü des **hinzufügen** und wählen Sie dann **Klasse.**

![](using-web-api-with-entity-framework-part-2/_static/image1.png)

Hinzufügen einer `Product` Klasse durch die folgende Implementierung:

[!code-csharp[Main](using-web-api-with-entity-framework-part-2/samples/sample1.cs)]

Gemäß der Konvention verwendet Entity Framework die `Id` Eigenschaft als Primärschlüssel und ordnet es zu einer Identity-Spalte in der Datenbanktabelle. Beim Erstellen einer neuen `Product` -Instanz wird nicht legen Sie einen Wert für `Id`, da die Datenbank den Wert generiert.

Die **ScaffoldColumn** Attribut teilt ASP.NET MVC zum Überspringen der `Id` Eigenschaft, wenn ein Formular-Editors zu generieren. Die **erforderlich** Attribut wird verwendet, um das Modell überprüft. Es gibt an, dass die `Name` -Eigenschaft muss eine nicht leere Zeichenfolge sein.

Hinzufügen der `Order` Klasse:

[!code-csharp[Main](using-web-api-with-entity-framework-part-2/samples/sample2.cs)]

Hinzufügen der `OrderDetail` Klasse:

[!code-csharp[Main](using-web-api-with-entity-framework-part-2/samples/sample3.cs)]

## <a name="foreign-key-relations"></a>Foreign Key-Beziehungen

Ein Auftrag enthält viele Auftragsdetails, und jede auftragsdetailbereich bezieht sich auf einem einzigen Produkt. Zur Darstellung dieser Beziehungen, die `OrderDetail` -Klasse definiert Eigenschaften, die mit dem Namen `OrderId` und `ProductId`. Entitätsframework folgert, dass diese Eigenschaften Fremdschlüssel darstellen, und der Datenbank foreign Key-Einschränkungen hinzugefügt werden.

![](using-web-api-with-entity-framework-part-2/_static/image2.png)

Die `Order` und `OrderDetail` Klassen umfassen auch "Navigation"-Eigenschaften, die Verweise auf die verknüpften Objekte enthalten. Wenn einen Auftrag, können Sie auf die Produkte in der Reihenfolge navigieren, anhand der Navigationseigenschaften.

Kompilieren Sie das Projekt jetzt. Entitätsframework verwendet Reflektion, um die Eigenschaften der Modelle, zu ermitteln, deshalb sie eine kompilierte Assembly ein, um das Datenbankschema zu erstellen müssen.

## <a name="configure-the-media-type-formatters"></a>Der Medientypformatierer konfigurieren

Ein [medientypformatierer](../../formats-and-model-binding/media-formatters.md) ein Objekt, das Ihre Daten serialisiert werden, wenn die Web-API-HTTP-Antworttext geschrieben wird. Die integrierten Formatierungsprogrammen unterstützen JSON- und XML-Ausgabe. Standardmäßig serialisiert werden beide dieser Formatierer alle Objekte nach Wert.

Serialisierung nach Wert führt zu einem Problem, wenn ein Objektdiagramm Zirkuläre Verweise enthält. Das ist genau der Fall mit der `Order` und `OrderDetail` Klassen, da Sie jeweils einen Verweis auf die andere enthält. Der Formatierer wird führen Sie die Verweise, Schreiben jedes Objekt als Wert und wechseln Sie im Kreis. Aus diesem Grund müssen wir das Standardverhalten zu ändern.

Erweitern Sie im Projektmappen-Explorer die App\_starten Sie die Ordner, und öffnen Sie die Datei mit dem Namen WebApiConfig.cs. Fügen Sie der `WebApiConfig` -Klasse folgenden Code hinzu:

[!code-csharp[Main](using-web-api-with-entity-framework-part-2/samples/sample4.cs?highlight=11)]

Dieser Code legt das JSON-Formatierungsprogramm Objektverweise beizubehalten, und den XML-Formatierer vollständig aus der Pipeline entfernt. (Sie können konfigurieren, dass des XML-Formatierers zum Beibehalten von Objektverweisen, aber es ist etwas mehr Arbeit, und wir brauchen nur JSON für diese Anwendung. Weitere Informationen finden Sie unter [zirkuläre Objektverweise behandeln](../../formats-and-model-binding/json-and-xml-serialization.md#handling_circular_object_references).)

> [!div class="step-by-step"]
> [Zurück](using-web-api-with-entity-framework-part-1.md)
> [Weiter](using-web-api-with-entity-framework-part-3.md)
