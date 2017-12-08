---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-2
title: "Teil 2: Erstellen der Domänenmodelle | Microsoft Docs"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/03/2012
ms.topic: article
ms.assetid: fe3ef85f-bdc6-4e10-9768-25aa565c01d0
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-2
msc.type: authoredcontent
ms.openlocfilehash: 5d4c7d7d02ced5a99db5b59f9e2e1adf6588208a
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="part-2-creating-the-domain-models"></a>Teil 2: Erstellen der Domänenmodelle
====================
durch [Mike Wasson](https://github.com/MikeWasson)

[Herunterladen des abgeschlossenen Projekts](http://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

## <a name="add-models"></a>Hinzufügen von Modellen

Es gibt drei Möglichkeiten, Ansatz Entity Framework:

- Database First: beginnen Sie mit einer Datenbank und Entity Framework generiert den Code.
- Model First: Sie beginnen mit einem visual Modell und Entity Framework generiert, die Datenbank und den Code.
- Code First: beginnen Sie mit Code und Entity Framework generiert die Datenbank.

Verwenden wir den Code First-Ansatz, damit wir beginnen mit unserer Domänenobjekte als POCOs (Plain-Old CLR Objects) definieren. Mit dem Code First-Ansatz benötigen Domänenobjekte nicht keinen zusätzlichen Code für die Datenbankebene, z. B. Transaktionen oder Persistenz unterstützen. (Insbesondere benötigen keinen zu Vererben der [EntityObject](https://msdn.microsoft.com/en-us/library/system.data.objects.dataclasses.entityobject.aspx) Klasse.) Datenanmerkungen können weiterhin steuern, wie Entity Framework für das Datenbankschema erstellt werden.

Da POCOs keine zusätzlichen Eigenschaften enthalten, die beschreiben, [Datenbankstatus](https://msdn.microsoft.com/en-us/library/system.data.entitystate.aspx), sie können problemlos in JSON oder XML serialisiert werden. Allerdings bedeutet dies nicht immer die Entity Framework-Modelle direkt an Clients sollten verfügbar machen, wie wir später in diesem Lernprogramm sehen werden.

Wir werden die folgenden POCOs erstellen:

- Produkt
- Reihenfolge
- OrderDetail

Um jede Klasse erstellen, und mit der rechten Maustaste im Projektmappen-Explorer die Ordner Models. Wählen Sie im Kontextmenü der **hinzufügen** und wählen Sie dann **Klasse.**

![](using-web-api-with-entity-framework-part-2/_static/image1.png)

Hinzufügen einer `Product` Klasse mit der folgenden Implementierung:

[!code-csharp[Main](using-web-api-with-entity-framework-part-2/samples/sample1.cs)]

Gemäß der Konvention Entity Framework verwendet die `Id` Eigenschaft als Primärschlüssel und ordnet sie einer Identity-Spalte in der Datenbanktabelle. Beim Erstellen einer neuen `Product` Instanz wird nicht für einen Wert festlegen `Id`, da die Datenbank den Wert generiert.

Die **ScaffoldColumn** -Attribut weist ASP.NET MVC zum Überspringen der `Id` Eigenschaft während der Generierung einer Formular-Editors. Die **erforderlich** Attribut wird verwendet, um das Modell zu überprüfen. Gibt an, dass die `Name` Eigenschaft muss eine nicht leere Zeichenfolge sein.

Hinzufügen der `Order` Klasse:

[!code-csharp[Main](using-web-api-with-entity-framework-part-2/samples/sample2.cs)]

Hinzufügen der `OrderDetail` Klasse:

[!code-csharp[Main](using-web-api-with-entity-framework-part-2/samples/sample3.cs)]

## <a name="foreign-key-relations"></a>Foreign Key-Beziehungen

Eine Bestellung enthält viele Bestelldetails, und jede Order Detail bezieht sich auf einem einzigen Produkt. Zur Darstellung dieser Beziehungen die `OrderDetail` Klasse definiert die Eigenschaften, die mit dem Namen `OrderId` und `ProductId`. Entity Framework wird abgeleitet werden, dass diese Eigenschaften Fremdschlüssel darstellen, und die Datenbank foreign Key-Einschränkungen hinzu.

![](using-web-api-with-entity-framework-part-2/_static/image2.png)

Die `Order` und `OrderDetail` Klassen umfassen auch ""-Navigationseigenschaften, die Verweise auf die verknüpften Objekte enthalten. Erhält eine Bestellung, können Sie die Produkte in der Reihenfolge navigieren, anhand der Navigationseigenschaften.

Kompilieren Sie das Projekt jetzt an. Entity Framework verwendet Reflektion, um die Eigenschaften der Modelle ermitteln, daher ist eine kompilierte Assembly So erstellen das Schema der Datenbank erforderlich.

## <a name="configure-the-media-type-formatters"></a>Konfigurieren Sie die Medientypformatierer

Ein [medientypformatierer](../../formats-and-model-binding/media-formatters.md) ist ein Objekt, das Ihre Daten serialisiert, wenn die Web-API über HTTP-Antworttext schreibt. Die integrierten Formatierer unterstützt JSON und XML-Ausgabe. Standardmäßig serialisiert beider dieser Formatierer alle Objekte nach Wert an.

Serialisierung nach Wert erstellt ein Problem auf, wenn ein Objektdiagramm Zirkuläre Verweise enthält. Dies ist genau der Fall mit der `Order` und `OrderDetail` Klassen, da jeweils einen Verweis auf die andere enthält. Der Formatierer wird führen Sie die Verweise, Schreiben jedes Objekt als Wert, und wechseln Sie im Kreis. Aus diesem Grund müssen wir das Standardverhalten zu ändern.

Erweitern Sie im Projektmappen-Explorer die App\_starten Sie die Ordner, und öffnen Sie die Datei mit dem Namen WebApiConfig.cs. Fügen Sie der `WebApiConfig` -Klasse folgenden Code hinzu:

[!code-csharp[Main](using-web-api-with-entity-framework-part-2/samples/sample4.cs?highlight=11)]

Dieser Code legt die JSON-Formatierungsprogramm Objektverweise beizubehalten und den XML-Formatierer vollständig aus der Pipeline entfernt. (Sie können konfigurieren, dass des XML-Formatierers zum Beibehalten von Objektverweisen, aber es etwas aufwendiger ist und wir brauchen nur JSON für diese Anwendung. Weitere Informationen finden Sie unter [Behandlung von Objekt-Zirkelverweise](../../formats-and-model-binding/json-and-xml-serialization.md#handling_circular_object_references).)

>[!div class="step-by-step"]
[Zurück](using-web-api-with-entity-framework-part-1.md)
[Weiter](using-web-api-with-entity-framework-part-3.md)
