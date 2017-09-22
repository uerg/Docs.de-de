---
title: Benutzerdefinierte wurden die Modellbindung
author: ardalis
description: Anpassen der modellbindung in ASP.NET Core MVC.
keywords: ASP.NET Core, wurden die modellbindung, benutzerdefinierten Modellbinder
ms.author: riande
manager: wpickett
ms.date: 04/10/2017
ms.topic: article
ms.assetid: ebd98159-a028-4a94-b06c-43981c79c6be
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/advanced/custom-model-binding
ms.openlocfilehash: f3fc3d624c3b79d49a886dd85ca8b19147631e39
ms.sourcegitcommit: 78d28178345a0eea91556e4cd1adad98b1446db8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/22/2017
---
# <a name="custom-model-binding"></a>Benutzerdefinierte wurden die Modellbindung

Durch [Steve Smith](https://ardalis.com/)

Wurden die modellbindung ermöglicht Controlleraktionen Modelltypen (in als Methodenargumente übergeben), sondern als HTTP-Anforderungen direkt arbeiten. Zuordnung zwischen der eingehenden Anforderung Daten- und anwendungsanforderungen Modelle wird von Modellbinder behandelt. Entwickler können die integrierten Modell Bindungsfunktionalität erweitern, durch die Implementierung von benutzerdefinierten Modellbinder (Obwohl in der Regel Sie nicht Ihren eigenen Anbieter schreiben müssen).

[Anzeigen oder Herunterladen des Beispiels von GitHub](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/advanced/custom-model-binding/)

## <a name="default-model-binder-limitations"></a>Standard-Modell Binder Einschränkungen

Die Standard-Modellbinder unterstützen die meisten allgemeinen .NET Core-Datentypen und sollte die meisten Entwickler Anforderungen erfüllen. Sie erwarten textbasierten Eingabe aus der Anforderung direkt an Modelltypen zu binden. Möglicherweise müssen Sie die Eingabe vor dem binden es zu transformieren. Wenn müssen Sie beispielsweise einen Schlüssel, der zum Nachschlagen der Modelldaten verwendet werden kann. Sie können einen benutzerdefinierten Modellbinder zum Abrufen von Daten basierend auf den Schlüssel verwenden.

## <a name="model-binding-review"></a>Modell Bindung überprüfen

Wurden die modellbindung verwendet spezifische Definitionen für die Datentypen, denen sie für ausgeführt wird. Ein *einfacher Typ* aus einer einzelnen Zeichenfolge in der Eingabe konvertiert wird. Ein *komplexen Typ* von mehrere Eingabewerte konvertiert werden. Das Framework bestimmt den Unterschied basierend auf dem Vorhandensein von einem `TypeConverter`. Es wird empfohlen, wenn Sie eine einfache haben, erstellen einen Typkonverter `string`  ->  `SomeType` Zuordnung, die keine externen Ressourcen erfordert.

Vor dem Erstellen Ihrer eigenen benutzerdefinierten Modellbinder, ist es, zu überprüfen, wie vorhandene Modell Bindern implementiert werden. Betrachten Sie die [ByteArrayModelBinder](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.modelbinding.binders.bytearraymodelbinder) die base64-codierte Zeichenfolgen in Bytearrays hinein konvertieren verwendet werden können. Bytearrays werden häufig als Dateien oder Datenbankfeldern-BLOB gespeichert.

### <a name="working-with-the-bytearraymodelbinder"></a>Arbeiten mit der ByteArrayModelBinder

Base64-codierte Zeichenfolgen können verwendet werden, um binäre Daten darzustellen. Beispielsweise kann die folgende Abbildung als Zeichenfolge codiert werden.

![Dotnet Bot](custom-model-binding/images/bot.png "Dotnet Bot")

Ein kleiner Teil die codierte Zeichenfolge wird in der folgenden Abbildung gezeigt:

![codiert ein Dotnet-Bot](custom-model-binding/images/encoded-bot.png "Dotnet Bot codiert")

Befolgen Sie die Anweisungen in der [des Beispiels Infodatei](https://github.com/aspnet/Docs/blob/master/aspnetcore/mvc/advanced/custom-model-binding/sample/CustomModelBindingSample/README.md) die base64-codierte Zeichenfolge in eine Datei zu konvertieren.

Core ASP.NET-MVC kann eine base64-codierte Zeichenfolgen und Verwenden einer `ByteArrayModelBinder` in ein Bytearray zu konvertieren. Die [ByteArrayModelBinderProvider](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.modelbinding.binders.bytearraymodelbinderprovider) implementiert [IModelBinderProvider](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.modelbinding.imodelbinderprovider) ordnet `byte[]` Argumente `ByteArrayModelBinder`:

```csharp
public IModelBinder GetBinder(ModelBinderProviderContext context)
{
    if (context == null)
    {
        throw new ArgumentNullException(nameof(context));
    }

    if (context.Metadata.ModelType == typeof(byte[]))
    {
        return new ByteArrayModelBinder();
    }

    return null;
}
```

Wenn Sie Ihre eigenen benutzerdefinierten Modellbinder zu erstellen, Sie können eigenen Dienst implementieren `IModelBinderProvider` geben, oder verwenden Sie die [ModelBinderAttribute](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.modelbinderattribute).

Das folgende Beispiel zeigt, wie Sie `ByteArrayModelBinder` konvertieren Sie eine base64-codierte Zeichenfolge in eine `byte[]` und das Ergebnis in einer Datei speichern:

[!code-csharp[Main](custom-model-binding/sample/CustomModelBindingSample/Controllers/ImageController.cs?name=post1&highlight=3)]

Können Sie eine base64-codierte Zeichenfolge an diese api-Methode, die mit einem Tool wie POST [Postman](https://www.getpostman.com/):

![Postman](custom-model-binding/images/postman.png "Postman")

Solange der Binder Anforderungsdaten auf entsprechend benannten Eigenschaften oder Argumente gebunden werden kann, erfolgreich wurden die modellbindung. Das folgende Beispiel zeigt, wie Sie `ByteArrayModelBinder` mit einem Ansichtsmodell:

[!code-csharp[Main](custom-model-binding/sample/CustomModelBindingSample/Controllers/ImageController.cs?name=post2&highlight=2)]

## <a name="custom-model-binder-sample"></a>Benutzerdefinierte Binder-modellbeispiels

In diesem Abschnitt implementieren wir einen benutzerdefinierten Modellbinder, die:

- Konvertiert eingehende Anforderungsdaten in stark typisierten Key Argumente an.
- Ruft mit Entity Framework Core die zugeordnete Entität sollen.
- Die zugeordnete Entität übergeben als Argument an die Aktionsmethode.

Das folgende Beispiel verwendet die `ModelBinder` -Attribut auf die `Author` Modell:

[!code-csharp[Main](custom-model-binding/sample/CustomModelBindingSample/Data/Author.cs?highlight=10)]

Im vorangehenden Code der `ModelBinder` Attribut gibt den Typ des `IModelBinder` verwendet werden soll, zum Binden `Author` Action-Parameter. 

Die `AuthorEntityBinder` wird verwendet, um das Binden einer `Author` Parameter durch Abrufen der Entität aus einer Datenquelle, die Verwendung von Entity Framework Core und ein `authorId`:

[!code-csharp[Main](custom-model-binding/sample/CustomModelBindingSample/Binders/AuthorEntityBinder.cs?name=demo)]

Der folgende Code zeigt, wie Sie die `AuthorEntityBinder` in einer Aktionsmethode:

[!code-csharp[Main](custom-model-binding/sample/CustomModelBindingSample/Controllers/BoundAuthorsController.cs?name=demo2&highlight=2)]

Die `ModelBinder` Attribut kann verwendet werden, um gelten die `AuthorEntityBinder` Parametern, die keine Standardkonventionen verwenden:

[!code-csharp[Main](custom-model-binding/sample/CustomModelBindingSample/Controllers/BoundAuthorsController.cs?name=demo1&highlight=2)]

In diesem Beispiel, da der Name des Arguments nicht die Standardeinstellung ist `authorId`, es wird angegeben, auf den Parameter mit `ModelBinder` Attribut. Beachten Sie, dass der Controller und der Aktionsmethode im Vergleich zum Nachschlagen der Entität in der Aktionsmethode vereinfacht werden. Die Logik zum Abrufen des Verwendung von Entity Framework Core Autors wird in den Modellbinder verschoben. Dies kann beträchtliche Vereinfachung sein, wenn Sie mehrere Methoden verfügen, die dem Autor Modell binden und helfen Ihnen beim befolgen die [TROCKENEN Prinzip](http://deviq.com/don-t-repeat-yourself/).

Können Sie anwenden der `ModelBinder` -Attribut auf einzelne Modelleigenschaften (z. B. auf einem Viewmodel) oder an die Aktionsmethodenparameter an einen bestimmten Modellbinder oder Modellname für nur den Typ oder die Aktion.

### <a name="implementing-a-modelbinderprovider"></a>Implementieren eine ModelBinderProvider

Anstatt ein Attribut anwenden, implementieren Sie `IModelBinderProvider`. Dies ist wie die Bindern integriertes Framework implementiert werden. Bei der Angabe des Typs der Binder arbeitet, geben Sie den Typ des Arguments, sie erzeugt **nicht** der Eingabe der Binder akzeptiert. Die folgenden Binder Anbieter arbeitet mit der `AuthorEntityBinder`. Beim MVCs-Auflistung von Anbietern hinzugefügt wird, müssen Sie verwenden die `ModelBinder` -Attribut `Author` oder `Author` typisierte Parameter.

[!code-csharp[Main](custom-model-binding/sample/CustomModelBindingSample/Binders/AuthorEntityBinderProvider.cs?highlight=17-20)]

> Hinweis: Der vorangehende Code gibt eine `BinderTypeModelBinder`. `BinderTypeModelBinder`fungiert als Factory für Modellbinder und abhängigkeiteneinschleusung (DI) enthält. Die `AuthorEntityBinder` erfordert DI auf EF Core zugreifen. Verwendung `BinderTypeModelBinder` Wenn Ihre Modellbinder Dienste aus DI erfordert.

Um einen benutzerdefinierten modellbinderanbieter zu verwenden, fügen Sie ihn unter `ConfigureServices`:

[!code-csharp[Main](custom-model-binding/sample/CustomModelBindingSample/Startup.cs?name=callout&highlight=5-9)]

Beim Auswerten der Modellbinder, wird die Auflistung von Anbietern in Reihenfolge untersucht. Der erste Anbieter, der einen Binder zurückgegeben wird verwendet.

Die folgende Abbildung zeigt die Modellbinder im Debugger:

![Standard Modellbinder](custom-model-binding/images/default-model-binders.png "Modellbinder Standard")

Hinzufügen von Ihrem Anbieter bis zum Ende der Auflistung möglicherweise eine integrierte Modellbinder aufgerufen werden, bevor Ihre benutzerdefinierten Binder Gelegenheit hat. In diesem Beispiel wird der benutzerdefinierte Anbieter auf den Anfang der Auflistung, um sicherzustellen, dass es dient für hinzugefügt `Author` Aktionsargumenten.

[!code-csharp[Main](custom-model-binding/sample/CustomModelBindingSample/Startup.cs?name=callout&highlight=5-9)]

## <a name="recommendations-and-best-practices"></a>Empfehlungen und bewährte Methoden

Benutzerdefinierte Modellbinder:
- Sollten nicht versuchen, Statuscodes festlegen oder Zurückgeben von Ergebnissen (zum Beispiel 404 nicht gefunden). Wenn wurden die modellbindung fehlschlägt, eine [Aktionsfilter](xref:mvc/controllers/filters) oder Logik innerhalb der Aktionsmethode selbst den Fehler behandelt werden sollen.
- Sind besonders hilfreich für die Eliminierung von sich wiederholenden Code und querschnittliche Bedenken bei Aktionsmethoden.
- Sollte in der Regel nicht zum Konvertieren einer Zeichenfolge in einen benutzerdefinierten Typ verwendet werden eine [ `TypeConverter` ](https://docs.microsoft.com//dotnet/api/system.componentmodel.typeconverter) ist in der Regel eine bessere Option.
