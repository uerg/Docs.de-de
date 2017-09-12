---
title: Benutzerdefinierte wurden die Modellbindung
author: ardalis
description: Anpassen der modellbindung in ASP.NET Core MVC.
keywords: ASP.NET Core, wurden die modellbindung, benutzerdefinierten Modellbinder
ms.author: riande
manager: wpickett
ms.date: 4/10/2017
ms.topic: article
ms.assetid: ebd98159-a028-4a94-b06c-43981c79c6be
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/advanced/custom-model-binding
ms.openlocfilehash: 2b95073bc0972908d0c0b2158a036e4374c7df4d
ms.sourcegitcommit: 9cdbfd0d670d70b9c354216aabee260c52dad5ee
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/12/2017
---
# <a name="custom-model-binding"></a><span data-ttu-id="36a25-104">Benutzerdefinierte wurden die Modellbindung</span><span class="sxs-lookup"><span data-stu-id="36a25-104">Custom Model Binding</span></span>

<span data-ttu-id="36a25-105">Durch [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="36a25-105">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="36a25-106">Wurden die modellbindung ermöglicht Controlleraktionen Modelltypen (in als Methodenargumente übergeben), sondern als HTTP-Anforderungen direkt arbeiten.</span><span class="sxs-lookup"><span data-stu-id="36a25-106">Model binding allows controller actions to work directly with model types (passed in as method arguments), rather than HTTP requests.</span></span> <span data-ttu-id="36a25-107">Zuordnung zwischen der eingehenden Anforderung Daten- und anwendungsanforderungen Modelle wird von Modellbinder behandelt.</span><span class="sxs-lookup"><span data-stu-id="36a25-107">Mapping between incoming request data and application models is handled by model binders.</span></span> <span data-ttu-id="36a25-108">Entwickler können die integrierten Modell Bindungsfunktionalität erweitern, durch die Implementierung von benutzerdefinierten Modellbinder (Obwohl in der Regel Sie nicht Ihren eigenen Anbieter schreiben müssen).</span><span class="sxs-lookup"><span data-stu-id="36a25-108">Developers can extend the built-in model binding functionality by implementing custom model binders (though typically, you don't need to write your own provider).</span></span>

[<span data-ttu-id="36a25-109">Anzeigen oder Herunterladen des Beispiels von GitHub</span><span class="sxs-lookup"><span data-stu-id="36a25-109">View or download sample from GitHub</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/advanced/custom-model-binding/)

## <a name="default-model-binder-limitations"></a><span data-ttu-id="36a25-110">Standard-Modell Binder Einschränkungen</span><span class="sxs-lookup"><span data-stu-id="36a25-110">Default model binder limitations</span></span>

<span data-ttu-id="36a25-111">Die Standard-Modellbinder unterstützen die meisten allgemeinen .NET Core-Datentypen und sollte die meisten Entwickler Anforderungen erfüllen.</span><span class="sxs-lookup"><span data-stu-id="36a25-111">The default model binders support most of the common .NET Core data types and should meet most developers\` needs.</span></span> <span data-ttu-id="36a25-112">Sie erwarten textbasierten Eingabe aus der Anforderung direkt an Modelltypen zu binden.</span><span class="sxs-lookup"><span data-stu-id="36a25-112">They expect to bind text-based input from the request directly to model types.</span></span> <span data-ttu-id="36a25-113">Möglicherweise müssen Sie die Eingabe vor dem binden es zu transformieren.</span><span class="sxs-lookup"><span data-stu-id="36a25-113">You might need to transform the input prior to binding it.</span></span> <span data-ttu-id="36a25-114">Wenn müssen Sie beispielsweise einen Schlüssel, der zum Nachschlagen der Modelldaten verwendet werden kann.</span><span class="sxs-lookup"><span data-stu-id="36a25-114">For example, when you have a key that can be used to look up model data.</span></span> <span data-ttu-id="36a25-115">Sie können einen benutzerdefinierten Modellbinder zum Abrufen von Daten basierend auf den Schlüssel verwenden.</span><span class="sxs-lookup"><span data-stu-id="36a25-115">You can use a custom model binder to fetch data based on the key.</span></span>

## <a name="model-binding-review"></a><span data-ttu-id="36a25-116">Modell Bindung überprüfen</span><span class="sxs-lookup"><span data-stu-id="36a25-116">Model binding review</span></span>

<span data-ttu-id="36a25-117">Wurden die modellbindung verwendet spezifische Definitionen für die Datentypen, denen sie für ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="36a25-117">Model binding uses specific definitions for the types it operates on.</span></span> <span data-ttu-id="36a25-118">Ein *einfacher Typ* aus einer einzelnen Zeichenfolge in der Eingabe konvertiert wird.</span><span class="sxs-lookup"><span data-stu-id="36a25-118">A *simple type* is converted from a single string in the input.</span></span> <span data-ttu-id="36a25-119">Ein *komplexen Typ* von mehrere Eingabewerte konvertiert werden.</span><span class="sxs-lookup"><span data-stu-id="36a25-119">A *complex type* is converted from multiple input values.</span></span> <span data-ttu-id="36a25-120">Das Framework bestimmt den Unterschied basierend auf dem Vorhandensein von einem `TypeConverter`.</span><span class="sxs-lookup"><span data-stu-id="36a25-120">The framework determines the difference based on the existence of a `TypeConverter`.</span></span> <span data-ttu-id="36a25-121">Es wird empfohlen, wenn Sie eine einfache haben, erstellen einen Typkonverter `string`  ->  `SomeType` Zuordnung, die keine externen Ressourcen erfordert.</span><span class="sxs-lookup"><span data-stu-id="36a25-121">We recommended you create a type converter if you have a simple `string` -> `SomeType` mapping that doesn't require external resources.</span></span>

<span data-ttu-id="36a25-122">Vor dem Erstellen Ihrer eigenen benutzerdefinierten Modellbinder, ist es, zu überprüfen, wie vorhandene Modell Bindern implementiert werden.</span><span class="sxs-lookup"><span data-stu-id="36a25-122">Before creating your own custom model binder, it's worth reviewing how existing model binders are implemented.</span></span> <span data-ttu-id="36a25-123">Betrachten Sie die [ByteArrayModelBinder](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.modelbinding.binders.bytearraymodelbinder) die base64-codierte Zeichenfolgen in Bytearrays hinein konvertieren verwendet werden können.</span><span class="sxs-lookup"><span data-stu-id="36a25-123">Consider the [ByteArrayModelBinder](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.modelbinding.binders.bytearraymodelbinder) which can be used to convert base64-encoded strings into byte arrays.</span></span> <span data-ttu-id="36a25-124">Bytearrays werden häufig als Dateien oder Datenbankfeldern-BLOB gespeichert.</span><span class="sxs-lookup"><span data-stu-id="36a25-124">The byte arrays are often stored as files or database BLOB fields.</span></span>

### <a name="working-with-the-bytearraymodelbinder"></a><span data-ttu-id="36a25-125">Arbeiten mit der ByteArrayModelBinder</span><span class="sxs-lookup"><span data-stu-id="36a25-125">Working with the ByteArrayModelBinder</span></span>

<span data-ttu-id="36a25-126">Base64-codierte Zeichenfolgen können verwendet werden, um binäre Daten darzustellen.</span><span class="sxs-lookup"><span data-stu-id="36a25-126">Base64-encoded strings can be used to represent binary data.</span></span> <span data-ttu-id="36a25-127">Beispielsweise kann die folgende Abbildung als Zeichenfolge codiert werden.</span><span class="sxs-lookup"><span data-stu-id="36a25-127">For example, the following image can be encoded as a string.</span></span>

<span data-ttu-id="36a25-128">![Dotnet Bot](custom-model-binding/images/bot.png "Dotnet Bot")</span><span class="sxs-lookup"><span data-stu-id="36a25-128">![dotnet bot](custom-model-binding/images/bot.png "dotnet bot")</span></span>

<span data-ttu-id="36a25-129">Ein kleiner Teil die codierte Zeichenfolge wird in der folgenden Abbildung gezeigt:</span><span class="sxs-lookup"><span data-stu-id="36a25-129">A small portion of the encoded string is shown in the following image:</span></span>

<span data-ttu-id="36a25-130">![codiert ein Dotnet-Bot](custom-model-binding/images/encoded-bot.png "Dotnet Bot codiert")</span><span class="sxs-lookup"><span data-stu-id="36a25-130">![dotnet bot encoded](custom-model-binding/images/encoded-bot.png "dotnet bot encoded")</span></span>

<span data-ttu-id="36a25-131">Befolgen Sie die Anweisungen in der [des Beispiels Infodatei](https://github.com/aspnet/Docs/blob/master/aspnetcore/mvc/advanced/custom-model-binding/sample/CustomModelBindingSample/README.md) die base64-codierte Zeichenfolge in eine Datei zu konvertieren.</span><span class="sxs-lookup"><span data-stu-id="36a25-131">Follow the instructions in the [sample's README](https://github.com/aspnet/Docs/blob/master/aspnetcore/mvc/advanced/custom-model-binding/sample/CustomModelBindingSample/README.md) to convert the base64-encoded string into a file.</span></span>

<span data-ttu-id="36a25-132">Core ASP.NET-MVC kann eine base64-codierte Zeichenfolgen und Verwenden einer `ByteArrayModelBinder` in ein Bytearray zu konvertieren.</span><span class="sxs-lookup"><span data-stu-id="36a25-132">ASP.NET Core MVC can take a base64-encoded strings and use a `ByteArrayModelBinder` to convert it into a byte array.</span></span> <span data-ttu-id="36a25-133">Die [ByteArrayModelBinderProvider](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.modelbinding.binders.bytearraymodelbinderprovider) implementiert [IModelBinderProvider](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.modelbinding.imodelbinderprovider) ordnet `byte[]` Argumente `ByteArrayModelBinder`:</span><span class="sxs-lookup"><span data-stu-id="36a25-133">The [ByteArrayModelBinderProvider](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.modelbinding.binders.bytearraymodelbinderprovider) which implements [IModelBinderProvider](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.modelbinding.imodelbinderprovider) maps `byte[]` arguments to `ByteArrayModelBinder`:</span></span>

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

<span data-ttu-id="36a25-134">Wenn Sie Ihre eigenen benutzerdefinierten Modellbinder zu erstellen, Sie können eigenen Dienst implementieren `IModelBinderProvider` geben, oder verwenden Sie die [ModelBinderAttribute](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.modelbinderattribute).</span><span class="sxs-lookup"><span data-stu-id="36a25-134">When creating your own custom model binder, you can implement your own `IModelBinderProvider` type, or use the [ModelBinderAttribute](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.modelbinderattribute).</span></span>

<span data-ttu-id="36a25-135">Das folgende Beispiel zeigt, wie Sie `ByteArrayModelBinder` konvertieren Sie eine base64-codierte Zeichenfolge in eine `byte[]` und das Ergebnis in einer Datei speichern:</span><span class="sxs-lookup"><span data-stu-id="36a25-135">The following example shows how to use `ByteArrayModelBinder` to convert a base64-encoded string to a `byte[]` and save the result to a file:</span></span>

<span data-ttu-id="36a25-136">[!code-csharp[Main](custom-model-binding/sample/CustomModelBindingSample/Controllers/ImageController.cs?name=post1&highlight=3)]</span><span class="sxs-lookup"><span data-stu-id="36a25-136">[!code-csharp[Main](custom-model-binding/sample/CustomModelBindingSample/Controllers/ImageController.cs?name=post1&highlight=3)]</span></span>

<span data-ttu-id="36a25-137">Können Sie eine base64-codierte Zeichenfolge an diese api-Methode, die mit einem Tool wie POST [Postman](https://www.getpostman.com/):</span><span class="sxs-lookup"><span data-stu-id="36a25-137">You can POST a base64-encoded string to this api method using a tool like [Postman](https://www.getpostman.com/):</span></span>

<span data-ttu-id="36a25-138">![Postman](custom-model-binding/images/postman.png "Postman")</span><span class="sxs-lookup"><span data-stu-id="36a25-138">![postman](custom-model-binding/images/postman.png "postman")</span></span>

<span data-ttu-id="36a25-139">Solange der Binder Anforderungsdaten auf entsprechend benannten Eigenschaften oder Argumente gebunden werden kann, erfolgreich wurden die modellbindung.</span><span class="sxs-lookup"><span data-stu-id="36a25-139">As long as the binder can bind request data to appropriately named properties or arguments, model binding will succeed.</span></span> <span data-ttu-id="36a25-140">Das folgende Beispiel zeigt, wie Sie `ByteArrayModelBinder` mit einem Ansichtsmodell:</span><span class="sxs-lookup"><span data-stu-id="36a25-140">The following example shows how to use `ByteArrayModelBinder` with a view model:</span></span>

<span data-ttu-id="36a25-141">[!code-csharp[Main](custom-model-binding/sample/CustomModelBindingSample/Controllers/ImageController.cs?name=post2&highlight=2)]</span><span class="sxs-lookup"><span data-stu-id="36a25-141">[!code-csharp[Main](custom-model-binding/sample/CustomModelBindingSample/Controllers/ImageController.cs?name=post2&highlight=2)]</span></span>

## <a name="custom-model-binder-sample"></a><span data-ttu-id="36a25-142">Benutzerdefinierte Binder-modellbeispiels</span><span class="sxs-lookup"><span data-stu-id="36a25-142">Custom model binder sample</span></span>

<span data-ttu-id="36a25-143">In diesem Abschnitt implementieren wir einen benutzerdefinierten Modellbinder, die:</span><span class="sxs-lookup"><span data-stu-id="36a25-143">In this section we'll implement a custom model binder that:</span></span>

- <span data-ttu-id="36a25-144">Konvertiert eingehende Anforderungsdaten in stark typisierten Key Argumente an.</span><span class="sxs-lookup"><span data-stu-id="36a25-144">Converts incoming request data into strongly typed key arguments.</span></span>
- <span data-ttu-id="36a25-145">Ruft mit Entity Framework Core die zugeordnete Entität sollen.</span><span class="sxs-lookup"><span data-stu-id="36a25-145">Uses Entity Framework Core to fetch the associated entity.</span></span>
- <span data-ttu-id="36a25-146">Die zugeordnete Entität übergeben als Argument an die Aktionsmethode.</span><span class="sxs-lookup"><span data-stu-id="36a25-146">Passes the associated entity as an argument to the action method.</span></span>

<span data-ttu-id="36a25-147">Das folgende Beispiel verwendet die `ModelBinder` -Attribut auf die `Author` Modell:</span><span class="sxs-lookup"><span data-stu-id="36a25-147">The following sample uses the `ModelBinder` attribute on the `Author` model:</span></span>

<span data-ttu-id="36a25-148">[!code-csharp[Main](custom-model-binding/sample/CustomModelBindingSample/Data/Author.cs?highlight=10)]</span><span class="sxs-lookup"><span data-stu-id="36a25-148">[!code-csharp[Main](custom-model-binding/sample/CustomModelBindingSample/Data/Author.cs?highlight=10)]</span></span>

<span data-ttu-id="36a25-149">Im vorangehenden Code der `ModelBinder` Attribut gibt den Typ des `IModelBinder` verwendet werden soll, zum Binden `Author` Action-Parameter.</span><span class="sxs-lookup"><span data-stu-id="36a25-149">In the preceding code, the `ModelBinder` attribute specifies the type of `IModelBinder` that should be used to bind `Author` action parameters.</span></span> 

<span data-ttu-id="36a25-150">Die `AuthorEntityBinder` wird verwendet, um das Binden einer `Author` Parameter durch Abrufen der Entität aus einer Datenquelle, die Verwendung von Entity Framework Core und ein `authorId`:</span><span class="sxs-lookup"><span data-stu-id="36a25-150">The `AuthorEntityBinder` is used to bind an `Author` parameter by fetching the entity from a data source using Entity Framework Core and an `authorId`:</span></span>

<span data-ttu-id="36a25-151">[!code-csharp[Main](custom-model-binding/sample/CustomModelBindingSample/Binders/AuthorEntityBinder.cs?name=demo)]</span><span class="sxs-lookup"><span data-stu-id="36a25-151">[!code-csharp[Main](custom-model-binding/sample/CustomModelBindingSample/Binders/AuthorEntityBinder.cs?name=demo)]</span></span>

<span data-ttu-id="36a25-152">Der folgende Code zeigt, wie Sie die `AuthorEntityBinder` in einer Aktionsmethode:</span><span class="sxs-lookup"><span data-stu-id="36a25-152">The following code shows how to use the `AuthorEntityBinder` in an action method:</span></span>

<span data-ttu-id="36a25-153">[!code-csharp[Main](custom-model-binding/sample/CustomModelBindingSample/Controllers/BoundAuthorsController.cs?name=demo2&highlight=2)]</span><span class="sxs-lookup"><span data-stu-id="36a25-153">[!code-csharp[Main](custom-model-binding/sample/CustomModelBindingSample/Controllers/BoundAuthorsController.cs?name=demo2&highlight=2)]</span></span>

<span data-ttu-id="36a25-154">Die `ModelBinder` Attribut kann verwendet werden, um gelten die `AuthorEntityBinder` Parametern, die keine Standardkonventionen verwenden:</span><span class="sxs-lookup"><span data-stu-id="36a25-154">The `ModelBinder` attribute can be used to apply the `AuthorEntityBinder` to parameters that do not use default conventions:</span></span>

<span data-ttu-id="36a25-155">[!code-csharp[Main](custom-model-binding/sample/CustomModelBindingSample/Controllers/BoundAuthorsController.cs?name=demo1&highlight=2)]</span><span class="sxs-lookup"><span data-stu-id="36a25-155">[!code-csharp[Main](custom-model-binding/sample/CustomModelBindingSample/Controllers/BoundAuthorsController.cs?name=demo1&highlight=2)]</span></span>

<span data-ttu-id="36a25-156">In diesem Beispiel, da der Name des Arguments nicht die Standardeinstellung ist `authorId`, es wird angegeben, auf den Parameter mit `ModelBinder` Attribut.</span><span class="sxs-lookup"><span data-stu-id="36a25-156">In this example, since the name of the argument is not the default `authorId`, it's specified on the parameter using `ModelBinder` attribute.</span></span> <span data-ttu-id="36a25-157">Beachten Sie, dass der Controller und der Aktionsmethode im Vergleich zum Nachschlagen der Entität in der Aktionsmethode vereinfacht werden.</span><span class="sxs-lookup"><span data-stu-id="36a25-157">Note that both the controller and action method are simplified compared to looking up the entity in the action method.</span></span> <span data-ttu-id="36a25-158">Die Logik zum Abrufen des Verwendung von Entity Framework Core Autors wird in den Modellbinder verschoben.</span><span class="sxs-lookup"><span data-stu-id="36a25-158">The logic to fetch the author using Entity Framework Core is moved to the model binder.</span></span> <span data-ttu-id="36a25-159">Dies kann beträchtliche Vereinfachung sein, wenn Sie mehrere Methoden verfügen, die dem Autor Modell binden und helfen Ihnen beim befolgen die [TROCKENEN Prinzip](http://deviq.com/don-t-repeat-yourself/).</span><span class="sxs-lookup"><span data-stu-id="36a25-159">This can be considerable simplification when you have several methods that bind to the author model, and can help you to follow the [DRY principle](http://deviq.com/don-t-repeat-yourself/).</span></span>

<span data-ttu-id="36a25-160">Können Sie anwenden der `ModelBinder` -Attribut auf einzelne Modelleigenschaften (z. B. auf einem Viewmodel) oder an die Aktionsmethodenparameter an einen bestimmten Modellbinder oder Modellname für nur den Typ oder die Aktion.</span><span class="sxs-lookup"><span data-stu-id="36a25-160">You can apply the `ModelBinder` attribute to individual model properties (such as on a viewmodel) or to action method parameters to specify a certain model binder or model name for just that type or action.</span></span>

### <a name="implementing-a-modelbinderprovider"></a><span data-ttu-id="36a25-161">Implementieren eine ModelBinderProvider</span><span class="sxs-lookup"><span data-stu-id="36a25-161">Implementing a ModelBinderProvider</span></span>

<span data-ttu-id="36a25-162">Anstatt ein Attribut anwenden, implementieren Sie `IModelBinderProvider`.</span><span class="sxs-lookup"><span data-stu-id="36a25-162">Instead of applying an attribute, you can implement `IModelBinderProvider`.</span></span> <span data-ttu-id="36a25-163">Dies ist wie die Bindern integriertes Framework implementiert werden.</span><span class="sxs-lookup"><span data-stu-id="36a25-163">This is how the built-in framework binders are implemented.</span></span> <span data-ttu-id="36a25-164">Bei der Angabe des Typs der Binder arbeitet, geben Sie den Typ des Arguments, sie erzeugt **nicht** der Eingabe der Binder akzeptiert.</span><span class="sxs-lookup"><span data-stu-id="36a25-164">When you specify the type your binder operates on, you specify the type of argument it produces, **not** the input your binder accepts.</span></span> <span data-ttu-id="36a25-165">Die folgenden Binder Anbieter arbeitet mit der `AuthorEntityBinder`.</span><span class="sxs-lookup"><span data-stu-id="36a25-165">The following binder provider works with the `AuthorEntityBinder`.</span></span> <span data-ttu-id="36a25-166">Beim MVCs-Auflistung von Anbietern hinzugefügt wird, müssen Sie verwenden die `ModelBinder` -Attribut `Author` oder `Author` typisierte Parameter.</span><span class="sxs-lookup"><span data-stu-id="36a25-166">When it's added to MVC's collection of providers, you don't need to use the `ModelBinder` attribute on `Author` or `Author` typed parameters.</span></span>

<span data-ttu-id="36a25-167">[!code-csharp[Main](custom-model-binding/sample/CustomModelBindingSample/Binders/AuthorEntityBinderProvider.cs?highlight=17-20)]</span><span class="sxs-lookup"><span data-stu-id="36a25-167">[!code-csharp[Main](custom-model-binding/sample/CustomModelBindingSample/Binders/AuthorEntityBinderProvider.cs?highlight=17-20)]</span></span>

> <span data-ttu-id="36a25-168">Hinweis: Der vorangehende Code gibt eine `BinderTypeModelBinder`.</span><span class="sxs-lookup"><span data-stu-id="36a25-168">Note: The preceding code returns a `BinderTypeModelBinder`.</span></span> <span data-ttu-id="36a25-169">`BinderTypeModelBinder`fungiert als Factory für Modellbinder und abhängigkeiteneinschleusung (DI) enthält.</span><span class="sxs-lookup"><span data-stu-id="36a25-169">`BinderTypeModelBinder` acts as a factory for model binders and provides dependency injection (DI).</span></span> <span data-ttu-id="36a25-170">Die `AuthorEntityBinder` erfordert DI auf EF Core zugreifen.</span><span class="sxs-lookup"><span data-stu-id="36a25-170">The `AuthorEntityBinder` requires DI to access EF Core.</span></span> <span data-ttu-id="36a25-171">Verwendung `BinderTypeModelBinder` Wenn Ihre Modellbinder Dienste aus DI erfordert.</span><span class="sxs-lookup"><span data-stu-id="36a25-171">Use `BinderTypeModelBinder` if your model binder requires services from DI.</span></span>

<span data-ttu-id="36a25-172">Um einen benutzerdefinierten modellbinderanbieter zu verwenden, fügen Sie ihn unter `ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="36a25-172">To use a custom model binder provider, add it in `ConfigureServices`:</span></span>

<span data-ttu-id="36a25-173">[!code-csharp[Main](custom-model-binding/sample/CustomModelBindingSample/Startup.cs?name=callout&highlight=5-9)]</span><span class="sxs-lookup"><span data-stu-id="36a25-173">[!code-csharp[Main](custom-model-binding/sample/CustomModelBindingSample/Startup.cs?name=callout&highlight=5-9)]</span></span>

<span data-ttu-id="36a25-174">Beim Auswerten der Modellbinder, wird die Auflistung von Anbietern in Reihenfolge untersucht.</span><span class="sxs-lookup"><span data-stu-id="36a25-174">When evaluating model binders, the collection of providers is examined in order.</span></span> <span data-ttu-id="36a25-175">Der erste Anbieter, der einen Binder zurückgegeben wird verwendet.</span><span class="sxs-lookup"><span data-stu-id="36a25-175">The first provider that returns a binder is used.</span></span>

<span data-ttu-id="36a25-176">Die folgende Abbildung zeigt die Modellbinder im Debugger:</span><span class="sxs-lookup"><span data-stu-id="36a25-176">The following image shows the default model binders from the debugger.</span></span>

<span data-ttu-id="36a25-177">![Standard Modellbinder](custom-model-binding/images/default-model-binders.png "Modellbinder Standard")</span><span class="sxs-lookup"><span data-stu-id="36a25-177">![default model binders](custom-model-binding/images/default-model-binders.png "default model binders")</span></span>

<span data-ttu-id="36a25-178">Hinzufügen von Ihrem Anbieter bis zum Ende der Auflistung möglicherweise eine integrierte Modellbinder aufgerufen werden, bevor Ihre benutzerdefinierten Binder Gelegenheit hat.</span><span class="sxs-lookup"><span data-stu-id="36a25-178">Adding your provider to the end of the collection may result in a built-in model binder being called before your custom binder has a chance.</span></span> <span data-ttu-id="36a25-179">In diesem Beispiel wird der benutzerdefinierte Anbieter auf den Anfang der Auflistung, um sicherzustellen, dass es dient für hinzugefügt `Author` Aktionsargumenten.</span><span class="sxs-lookup"><span data-stu-id="36a25-179">In this example, the custom provider is added to the beginning of the collection to ensure it is used for `Author` action arguments.</span></span>

<span data-ttu-id="36a25-180">[!code-csharp[Main](custom-model-binding/sample/CustomModelBindingSample/Startup.cs?name=callout&highlight=5-9)]</span><span class="sxs-lookup"><span data-stu-id="36a25-180">[!code-csharp[Main](custom-model-binding/sample/CustomModelBindingSample/Startup.cs?name=callout&highlight=5-9)]</span></span>

## <a name="recommendations-and-best-practices"></a><span data-ttu-id="36a25-181">Empfehlungen und bewährte Methoden</span><span class="sxs-lookup"><span data-stu-id="36a25-181">Recommendations and best practices</span></span>

<span data-ttu-id="36a25-182">Benutzerdefinierte Modellbinder:</span><span class="sxs-lookup"><span data-stu-id="36a25-182">Custom model binders:</span></span>
- <span data-ttu-id="36a25-183">Sollten nicht versuchen, Statuscodes festlegen oder Zurückgeben von Ergebnissen (zum Beispiel 404 nicht gefunden).</span><span class="sxs-lookup"><span data-stu-id="36a25-183">Should not attempt to set status codes or return results (for example, 404 Not Found).</span></span> <span data-ttu-id="36a25-184">Wenn wurden die modellbindung fehlschlägt, eine [Aktionsfilter](xref:mvc/controllers/filters) oder Logik innerhalb der Aktionsmethode selbst den Fehler behandelt werden sollen.</span><span class="sxs-lookup"><span data-stu-id="36a25-184">If model binding fails, an [action filter](xref:mvc/controllers/filters) or logic within the action method itself should handle the failure.</span></span>
- <span data-ttu-id="36a25-185">Sind besonders hilfreich für die Eliminierung von sich wiederholenden Code und querschnittliche Bedenken bei Aktionsmethoden.</span><span class="sxs-lookup"><span data-stu-id="36a25-185">Are most useful for eliminating repetitive code and cross-cutting concerns from action methods.</span></span>
- <span data-ttu-id="36a25-186">Sollte in der Regel nicht zum Konvertieren einer Zeichenfolge in einen benutzerdefinierten Typ verwendet werden eine [ `TypeConverter` ](https://docs.microsoft.com//dotnet/api/system.componentmodel.typeconverter) ist in der Regel eine bessere Option.</span><span class="sxs-lookup"><span data-stu-id="36a25-186">Typically should not be used to convert a string into a custom type, a [`TypeConverter`](https://docs.microsoft.com//dotnet/api/system.componentmodel.typeconverter) is usually a better option.</span></span>
