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
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
# <a name="custom-model-binding"></a><span data-ttu-id="ff06e-104">Benutzerdefinierte wurden die Modellbindung</span><span class="sxs-lookup"><span data-stu-id="ff06e-104">Custom Model Binding</span></span>

<span data-ttu-id="ff06e-105">Durch [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="ff06e-105">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="ff06e-106">Wurden die modellbindung ermöglicht Controlleraktionen Modelltypen (in als Methodenargumente übergeben), sondern als HTTP-Anforderungen direkt arbeiten.</span><span class="sxs-lookup"><span data-stu-id="ff06e-106">Model binding allows controller actions to work directly with model types (passed in as method arguments), rather than HTTP requests.</span></span> <span data-ttu-id="ff06e-107">Zuordnung zwischen der eingehenden Anforderung Daten- und anwendungsanforderungen Modelle wird von Modellbinder behandelt.</span><span class="sxs-lookup"><span data-stu-id="ff06e-107">Mapping between incoming request data and application models is handled by model binders.</span></span> <span data-ttu-id="ff06e-108">Entwickler können die integrierten Modell Bindungsfunktionalität erweitern, durch die Implementierung von benutzerdefinierten Modellbinder (Obwohl in der Regel Sie nicht Ihren eigenen Anbieter schreiben müssen).</span><span class="sxs-lookup"><span data-stu-id="ff06e-108">Developers can extend the built-in model binding functionality by implementing custom model binders (though typically, you don't need to write your own provider).</span></span>

[<span data-ttu-id="ff06e-109">Anzeigen oder Herunterladen des Beispiels von GitHub</span><span class="sxs-lookup"><span data-stu-id="ff06e-109">View or download sample from GitHub</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/advanced/custom-model-binding/)

## <a name="default-model-binder-limitations"></a><span data-ttu-id="ff06e-110">Standard-Modell Binder Einschränkungen</span><span class="sxs-lookup"><span data-stu-id="ff06e-110">Default model binder limitations</span></span>

<span data-ttu-id="ff06e-111">Die Standard-Modellbinder unterstützen die meisten allgemeinen .NET Core-Datentypen und sollte die meisten Entwickler Anforderungen erfüllen.</span><span class="sxs-lookup"><span data-stu-id="ff06e-111">The default model binders support most of the common .NET Core data types and should meet most developers\` needs.</span></span> <span data-ttu-id="ff06e-112">Sie erwarten textbasierten Eingabe aus der Anforderung direkt an Modelltypen zu binden.</span><span class="sxs-lookup"><span data-stu-id="ff06e-112">They expect to bind text-based input from the request directly to model types.</span></span> <span data-ttu-id="ff06e-113">Möglicherweise müssen Sie die Eingabe vor dem binden es zu transformieren.</span><span class="sxs-lookup"><span data-stu-id="ff06e-113">You might need to transform the input prior to binding it.</span></span> <span data-ttu-id="ff06e-114">Wenn müssen Sie beispielsweise einen Schlüssel, der zum Nachschlagen der Modelldaten verwendet werden kann.</span><span class="sxs-lookup"><span data-stu-id="ff06e-114">For example, when you have a key that can be used to look up model data.</span></span> <span data-ttu-id="ff06e-115">Sie können einen benutzerdefinierten Modellbinder zum Abrufen von Daten basierend auf den Schlüssel verwenden.</span><span class="sxs-lookup"><span data-stu-id="ff06e-115">You can use a custom model binder to fetch data based on the key.</span></span>

## <a name="model-binding-review"></a><span data-ttu-id="ff06e-116">Modell Bindung überprüfen</span><span class="sxs-lookup"><span data-stu-id="ff06e-116">Model binding review</span></span>

<span data-ttu-id="ff06e-117">Wurden die modellbindung verwendet spezifische Definitionen für die Datentypen, denen sie für ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="ff06e-117">Model binding uses specific definitions for the types it operates on.</span></span> <span data-ttu-id="ff06e-118">Ein *einfacher Typ* aus einer einzelnen Zeichenfolge in der Eingabe konvertiert wird.</span><span class="sxs-lookup"><span data-stu-id="ff06e-118">A *simple type* is converted from a single string in the input.</span></span> <span data-ttu-id="ff06e-119">Ein *komplexen Typ* von mehrere Eingabewerte konvertiert werden.</span><span class="sxs-lookup"><span data-stu-id="ff06e-119">A *complex type* is converted from multiple input values.</span></span> <span data-ttu-id="ff06e-120">Das Framework bestimmt den Unterschied basierend auf dem Vorhandensein von einem `TypeConverter`.</span><span class="sxs-lookup"><span data-stu-id="ff06e-120">The framework determines the difference based on the existence of a `TypeConverter`.</span></span> <span data-ttu-id="ff06e-121">Es wird empfohlen, wenn Sie eine einfache haben, erstellen einen Typkonverter `string`  ->  `SomeType` Zuordnung, die keine externen Ressourcen erfordert.</span><span class="sxs-lookup"><span data-stu-id="ff06e-121">We recommended you create a type converter if you have a simple `string` -> `SomeType` mapping that doesn't require external resources.</span></span>

<span data-ttu-id="ff06e-122">Vor dem Erstellen Ihrer eigenen benutzerdefinierten Modellbinder, ist es, zu überprüfen, wie vorhandene Modell Bindern implementiert werden.</span><span class="sxs-lookup"><span data-stu-id="ff06e-122">Before creating your own custom model binder, it's worth reviewing how existing model binders are implemented.</span></span> <span data-ttu-id="ff06e-123">Betrachten Sie die [ByteArrayModelBinder](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.modelbinding.binders.bytearraymodelbinder) die base64-codierte Zeichenfolgen in Bytearrays hinein konvertieren verwendet werden können.</span><span class="sxs-lookup"><span data-stu-id="ff06e-123">Consider the [ByteArrayModelBinder](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.modelbinding.binders.bytearraymodelbinder) which can be used to convert base64-encoded strings into byte arrays.</span></span> <span data-ttu-id="ff06e-124">Bytearrays werden häufig als Dateien oder Datenbankfeldern-BLOB gespeichert.</span><span class="sxs-lookup"><span data-stu-id="ff06e-124">The byte arrays are often stored as files or database BLOB fields.</span></span>

### <a name="working-with-the-bytearraymodelbinder"></a><span data-ttu-id="ff06e-125">Arbeiten mit der ByteArrayModelBinder</span><span class="sxs-lookup"><span data-stu-id="ff06e-125">Working with the ByteArrayModelBinder</span></span>

<span data-ttu-id="ff06e-126">Base64-codierte Zeichenfolgen können verwendet werden, um binäre Daten darzustellen.</span><span class="sxs-lookup"><span data-stu-id="ff06e-126">Base64-encoded strings can be used to represent binary data.</span></span> <span data-ttu-id="ff06e-127">Beispielsweise kann die folgende Abbildung als Zeichenfolge codiert werden.</span><span class="sxs-lookup"><span data-stu-id="ff06e-127">For example, the following image can be encoded as a string.</span></span>

<span data-ttu-id="ff06e-128">![Dotnet Bot](custom-model-binding/images/bot.png "Dotnet Bot")</span><span class="sxs-lookup"><span data-stu-id="ff06e-128">![dotnet bot](custom-model-binding/images/bot.png "dotnet bot")</span></span>

<span data-ttu-id="ff06e-129">Ein kleiner Teil die codierte Zeichenfolge wird in der folgenden Abbildung gezeigt:</span><span class="sxs-lookup"><span data-stu-id="ff06e-129">A small portion of the encoded string is shown in the following image:</span></span>

<span data-ttu-id="ff06e-130">![codiert ein Dotnet-Bot](custom-model-binding/images/encoded-bot.png "Dotnet Bot codiert")</span><span class="sxs-lookup"><span data-stu-id="ff06e-130">![dotnet bot encoded](custom-model-binding/images/encoded-bot.png "dotnet bot encoded")</span></span>

<span data-ttu-id="ff06e-131">Befolgen Sie die Anweisungen in der [des Beispiels Infodatei](https://github.com/aspnet/Docs/blob/master/aspnetcore/mvc/advanced/custom-model-binding/sample/CustomModelBindingSample/README.md) die base64-codierte Zeichenfolge in eine Datei zu konvertieren.</span><span class="sxs-lookup"><span data-stu-id="ff06e-131">Follow the instructions in the [sample's README](https://github.com/aspnet/Docs/blob/master/aspnetcore/mvc/advanced/custom-model-binding/sample/CustomModelBindingSample/README.md) to convert the base64-encoded string into a file.</span></span>

<span data-ttu-id="ff06e-132">Core ASP.NET-MVC kann eine base64-codierte Zeichenfolgen und Verwenden einer `ByteArrayModelBinder` in ein Bytearray zu konvertieren.</span><span class="sxs-lookup"><span data-stu-id="ff06e-132">ASP.NET Core MVC can take a base64-encoded strings and use a `ByteArrayModelBinder` to convert it into a byte array.</span></span> <span data-ttu-id="ff06e-133">Die [ByteArrayModelBinderProvider](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.modelbinding.binders.bytearraymodelbinderprovider) implementiert [IModelBinderProvider](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.modelbinding.imodelbinderprovider) ordnet `byte[]` Argumente `ByteArrayModelBinder`:</span><span class="sxs-lookup"><span data-stu-id="ff06e-133">The [ByteArrayModelBinderProvider](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.modelbinding.binders.bytearraymodelbinderprovider) which implements [IModelBinderProvider](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.modelbinding.imodelbinderprovider) maps `byte[]` arguments to `ByteArrayModelBinder`:</span></span>

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

<span data-ttu-id="ff06e-134">Wenn Sie Ihre eigenen benutzerdefinierten Modellbinder zu erstellen, Sie können eigenen Dienst implementieren `IModelBinderProvider` geben, oder verwenden Sie die [ModelBinderAttribute](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.modelbinderattribute).</span><span class="sxs-lookup"><span data-stu-id="ff06e-134">When creating your own custom model binder, you can implement your own `IModelBinderProvider` type, or use the [ModelBinderAttribute](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.modelbinderattribute).</span></span>

<span data-ttu-id="ff06e-135">Das folgende Beispiel zeigt, wie Sie `ByteArrayModelBinder` konvertieren Sie eine base64-codierte Zeichenfolge in eine `byte[]` und das Ergebnis in einer Datei speichern:</span><span class="sxs-lookup"><span data-stu-id="ff06e-135">The following example shows how to use `ByteArrayModelBinder` to convert a base64-encoded string to a `byte[]` and save the result to a file:</span></span>

[!code-csharp[Main](custom-model-binding/sample/CustomModelBindingSample/Controllers/ImageController.cs?name=post1&highlight=3)]

<span data-ttu-id="ff06e-136">Können Sie eine base64-codierte Zeichenfolge an diese api-Methode, die mit einem Tool wie POST [Postman](https://www.getpostman.com/):</span><span class="sxs-lookup"><span data-stu-id="ff06e-136">You can POST a base64-encoded string to this api method using a tool like [Postman](https://www.getpostman.com/):</span></span>

<span data-ttu-id="ff06e-137">![Postman](custom-model-binding/images/postman.png "Postman")</span><span class="sxs-lookup"><span data-stu-id="ff06e-137">![postman](custom-model-binding/images/postman.png "postman")</span></span>

<span data-ttu-id="ff06e-138">Solange der Binder Anforderungsdaten auf entsprechend benannten Eigenschaften oder Argumente gebunden werden kann, erfolgreich wurden die modellbindung.</span><span class="sxs-lookup"><span data-stu-id="ff06e-138">As long as the binder can bind request data to appropriately named properties or arguments, model binding will succeed.</span></span> <span data-ttu-id="ff06e-139">Das folgende Beispiel zeigt, wie Sie `ByteArrayModelBinder` mit einem Ansichtsmodell:</span><span class="sxs-lookup"><span data-stu-id="ff06e-139">The following example shows how to use `ByteArrayModelBinder` with a view model:</span></span>

[!code-csharp[Main](custom-model-binding/sample/CustomModelBindingSample/Controllers/ImageController.cs?name=post2&highlight=2)]

## <a name="custom-model-binder-sample"></a><span data-ttu-id="ff06e-140">Benutzerdefinierte Binder-modellbeispiels</span><span class="sxs-lookup"><span data-stu-id="ff06e-140">Custom model binder sample</span></span>

<span data-ttu-id="ff06e-141">In diesem Abschnitt implementieren wir einen benutzerdefinierten Modellbinder, die:</span><span class="sxs-lookup"><span data-stu-id="ff06e-141">In this section we'll implement a custom model binder that:</span></span>

- <span data-ttu-id="ff06e-142">Konvertiert eingehende Anforderungsdaten in stark typisierten Key Argumente an.</span><span class="sxs-lookup"><span data-stu-id="ff06e-142">Converts incoming request data into strongly typed key arguments.</span></span>
- <span data-ttu-id="ff06e-143">Ruft mit Entity Framework Core die zugeordnete Entität sollen.</span><span class="sxs-lookup"><span data-stu-id="ff06e-143">Uses Entity Framework Core to fetch the associated entity.</span></span>
- <span data-ttu-id="ff06e-144">Die zugeordnete Entität übergeben als Argument an die Aktionsmethode.</span><span class="sxs-lookup"><span data-stu-id="ff06e-144">Passes the associated entity as an argument to the action method.</span></span>

<span data-ttu-id="ff06e-145">Das folgende Beispiel verwendet die `ModelBinder` -Attribut auf die `Author` Modell:</span><span class="sxs-lookup"><span data-stu-id="ff06e-145">The following sample uses the `ModelBinder` attribute on the `Author` model:</span></span>

[!code-csharp[Main](custom-model-binding/sample/CustomModelBindingSample/Data/Author.cs?highlight=10)]

<span data-ttu-id="ff06e-146">Im vorangehenden Code der `ModelBinder` Attribut gibt den Typ des `IModelBinder` verwendet werden soll, zum Binden `Author` Action-Parameter.</span><span class="sxs-lookup"><span data-stu-id="ff06e-146">In the preceding code, the `ModelBinder` attribute specifies the type of `IModelBinder` that should be used to bind `Author` action parameters.</span></span> 

<span data-ttu-id="ff06e-147">Die `AuthorEntityBinder` wird verwendet, um das Binden einer `Author` Parameter durch Abrufen der Entität aus einer Datenquelle, die Verwendung von Entity Framework Core und ein `authorId`:</span><span class="sxs-lookup"><span data-stu-id="ff06e-147">The `AuthorEntityBinder` is used to bind an `Author` parameter by fetching the entity from a data source using Entity Framework Core and an `authorId`:</span></span>

[!code-csharp[Main](custom-model-binding/sample/CustomModelBindingSample/Binders/AuthorEntityBinder.cs?name=demo)]

<span data-ttu-id="ff06e-148">Der folgende Code zeigt, wie Sie die `AuthorEntityBinder` in einer Aktionsmethode:</span><span class="sxs-lookup"><span data-stu-id="ff06e-148">The following code shows how to use the `AuthorEntityBinder` in an action method:</span></span>

[!code-csharp[Main](custom-model-binding/sample/CustomModelBindingSample/Controllers/BoundAuthorsController.cs?name=demo2&highlight=2)]

<span data-ttu-id="ff06e-149">Die `ModelBinder` Attribut kann verwendet werden, um gelten die `AuthorEntityBinder` Parametern, die keine Standardkonventionen verwenden:</span><span class="sxs-lookup"><span data-stu-id="ff06e-149">The `ModelBinder` attribute can be used to apply the `AuthorEntityBinder` to parameters that do not use default conventions:</span></span>

[!code-csharp[Main](custom-model-binding/sample/CustomModelBindingSample/Controllers/BoundAuthorsController.cs?name=demo1&highlight=2)]

<span data-ttu-id="ff06e-150">In diesem Beispiel, da der Name des Arguments nicht die Standardeinstellung ist `authorId`, es wird angegeben, auf den Parameter mit `ModelBinder` Attribut.</span><span class="sxs-lookup"><span data-stu-id="ff06e-150">In this example, since the name of the argument is not the default `authorId`, it's specified on the parameter using `ModelBinder` attribute.</span></span> <span data-ttu-id="ff06e-151">Beachten Sie, dass der Controller und der Aktionsmethode im Vergleich zum Nachschlagen der Entität in der Aktionsmethode vereinfacht werden.</span><span class="sxs-lookup"><span data-stu-id="ff06e-151">Note that both the controller and action method are simplified compared to looking up the entity in the action method.</span></span> <span data-ttu-id="ff06e-152">Die Logik zum Abrufen des Verwendung von Entity Framework Core Autors wird in den Modellbinder verschoben.</span><span class="sxs-lookup"><span data-stu-id="ff06e-152">The logic to fetch the author using Entity Framework Core is moved to the model binder.</span></span> <span data-ttu-id="ff06e-153">Dies kann beträchtliche Vereinfachung sein, wenn Sie mehrere Methoden verfügen, die dem Autor Modell binden und helfen Ihnen beim befolgen die [TROCKENEN Prinzip](http://deviq.com/don-t-repeat-yourself/).</span><span class="sxs-lookup"><span data-stu-id="ff06e-153">This can be considerable simplification when you have several methods that bind to the author model, and can help you to follow the [DRY principle](http://deviq.com/don-t-repeat-yourself/).</span></span>

<span data-ttu-id="ff06e-154">Können Sie anwenden der `ModelBinder` -Attribut auf einzelne Modelleigenschaften (z. B. auf einem Viewmodel) oder an die Aktionsmethodenparameter an einen bestimmten Modellbinder oder Modellname für nur den Typ oder die Aktion.</span><span class="sxs-lookup"><span data-stu-id="ff06e-154">You can apply the `ModelBinder` attribute to individual model properties (such as on a viewmodel) or to action method parameters to specify a certain model binder or model name for just that type or action.</span></span>

### <a name="implementing-a-modelbinderprovider"></a><span data-ttu-id="ff06e-155">Implementieren eine ModelBinderProvider</span><span class="sxs-lookup"><span data-stu-id="ff06e-155">Implementing a ModelBinderProvider</span></span>

<span data-ttu-id="ff06e-156">Anstatt ein Attribut anwenden, implementieren Sie `IModelBinderProvider`.</span><span class="sxs-lookup"><span data-stu-id="ff06e-156">Instead of applying an attribute, you can implement `IModelBinderProvider`.</span></span> <span data-ttu-id="ff06e-157">Dies ist wie die Bindern integriertes Framework implementiert werden.</span><span class="sxs-lookup"><span data-stu-id="ff06e-157">This is how the built-in framework binders are implemented.</span></span> <span data-ttu-id="ff06e-158">Bei der Angabe des Typs der Binder arbeitet, geben Sie den Typ des Arguments, sie erzeugt **nicht** der Eingabe der Binder akzeptiert.</span><span class="sxs-lookup"><span data-stu-id="ff06e-158">When you specify the type your binder operates on, you specify the type of argument it produces, **not** the input your binder accepts.</span></span> <span data-ttu-id="ff06e-159">Die folgenden Binder Anbieter arbeitet mit der `AuthorEntityBinder`.</span><span class="sxs-lookup"><span data-stu-id="ff06e-159">The following binder provider works with the `AuthorEntityBinder`.</span></span> <span data-ttu-id="ff06e-160">Beim MVCs-Auflistung von Anbietern hinzugefügt wird, müssen Sie verwenden die `ModelBinder` -Attribut `Author` oder `Author` typisierte Parameter.</span><span class="sxs-lookup"><span data-stu-id="ff06e-160">When it's added to MVC's collection of providers, you don't need to use the `ModelBinder` attribute on `Author` or `Author` typed parameters.</span></span>

[!code-csharp[Main](custom-model-binding/sample/CustomModelBindingSample/Binders/AuthorEntityBinderProvider.cs?highlight=17-20)]

> <span data-ttu-id="ff06e-161">Hinweis: Der vorangehende Code gibt eine `BinderTypeModelBinder`.</span><span class="sxs-lookup"><span data-stu-id="ff06e-161">Note: The preceding code returns a `BinderTypeModelBinder`.</span></span> <span data-ttu-id="ff06e-162">`BinderTypeModelBinder`fungiert als Factory für Modellbinder und abhängigkeiteneinschleusung (DI) enthält.</span><span class="sxs-lookup"><span data-stu-id="ff06e-162">`BinderTypeModelBinder` acts as a factory for model binders and provides dependency injection (DI).</span></span> <span data-ttu-id="ff06e-163">Die `AuthorEntityBinder` erfordert DI auf EF Core zugreifen.</span><span class="sxs-lookup"><span data-stu-id="ff06e-163">The `AuthorEntityBinder` requires DI to access EF Core.</span></span> <span data-ttu-id="ff06e-164">Verwendung `BinderTypeModelBinder` Wenn Ihre Modellbinder Dienste aus DI erfordert.</span><span class="sxs-lookup"><span data-stu-id="ff06e-164">Use `BinderTypeModelBinder` if your model binder requires services from DI.</span></span>

<span data-ttu-id="ff06e-165">Um einen benutzerdefinierten modellbinderanbieter zu verwenden, fügen Sie ihn unter `ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="ff06e-165">To use a custom model binder provider, add it in `ConfigureServices`:</span></span>

[!code-csharp[Main](custom-model-binding/sample/CustomModelBindingSample/Startup.cs?name=callout&highlight=5-9)]

<span data-ttu-id="ff06e-166">Beim Auswerten der Modellbinder, wird die Auflistung von Anbietern in Reihenfolge untersucht.</span><span class="sxs-lookup"><span data-stu-id="ff06e-166">When evaluating model binders, the collection of providers is examined in order.</span></span> <span data-ttu-id="ff06e-167">Der erste Anbieter, der einen Binder zurückgegeben wird verwendet.</span><span class="sxs-lookup"><span data-stu-id="ff06e-167">The first provider that returns a binder is used.</span></span>

<span data-ttu-id="ff06e-168">Die folgende Abbildung zeigt die Modellbinder im Debugger:</span><span class="sxs-lookup"><span data-stu-id="ff06e-168">The following image shows the default model binders from the debugger.</span></span>

<span data-ttu-id="ff06e-169">![Standard Modellbinder](custom-model-binding/images/default-model-binders.png "Modellbinder Standard")</span><span class="sxs-lookup"><span data-stu-id="ff06e-169">![default model binders](custom-model-binding/images/default-model-binders.png "default model binders")</span></span>

<span data-ttu-id="ff06e-170">Hinzufügen von Ihrem Anbieter bis zum Ende der Auflistung möglicherweise eine integrierte Modellbinder aufgerufen werden, bevor Ihre benutzerdefinierten Binder Gelegenheit hat.</span><span class="sxs-lookup"><span data-stu-id="ff06e-170">Adding your provider to the end of the collection may result in a built-in model binder being called before your custom binder has a chance.</span></span> <span data-ttu-id="ff06e-171">In diesem Beispiel wird der benutzerdefinierte Anbieter auf den Anfang der Auflistung, um sicherzustellen, dass es dient für hinzugefügt `Author` Aktionsargumenten.</span><span class="sxs-lookup"><span data-stu-id="ff06e-171">In this example, the custom provider is added to the beginning of the collection to ensure it is used for `Author` action arguments.</span></span>

[!code-csharp[Main](custom-model-binding/sample/CustomModelBindingSample/Startup.cs?name=callout&highlight=5-9)]

## <a name="recommendations-and-best-practices"></a><span data-ttu-id="ff06e-172">Empfehlungen und bewährte Methoden</span><span class="sxs-lookup"><span data-stu-id="ff06e-172">Recommendations and best practices</span></span>

<span data-ttu-id="ff06e-173">Benutzerdefinierte Modellbinder:</span><span class="sxs-lookup"><span data-stu-id="ff06e-173">Custom model binders:</span></span>
- <span data-ttu-id="ff06e-174">Sollten nicht versuchen, Statuscodes festlegen oder Zurückgeben von Ergebnissen (zum Beispiel 404 nicht gefunden).</span><span class="sxs-lookup"><span data-stu-id="ff06e-174">Should not attempt to set status codes or return results (for example, 404 Not Found).</span></span> <span data-ttu-id="ff06e-175">Wenn wurden die modellbindung fehlschlägt, eine [Aktionsfilter](xref:mvc/controllers/filters) oder Logik innerhalb der Aktionsmethode selbst den Fehler behandelt werden sollen.</span><span class="sxs-lookup"><span data-stu-id="ff06e-175">If model binding fails, an [action filter](xref:mvc/controllers/filters) or logic within the action method itself should handle the failure.</span></span>
- <span data-ttu-id="ff06e-176">Sind besonders hilfreich für die Eliminierung von sich wiederholenden Code und querschnittliche Bedenken bei Aktionsmethoden.</span><span class="sxs-lookup"><span data-stu-id="ff06e-176">Are most useful for eliminating repetitive code and cross-cutting concerns from action methods.</span></span>
- <span data-ttu-id="ff06e-177">Sollte in der Regel nicht zum Konvertieren einer Zeichenfolge in einen benutzerdefinierten Typ verwendet werden eine [ `TypeConverter` ](https://docs.microsoft.com//dotnet/api/system.componentmodel.typeconverter) ist in der Regel eine bessere Option.</span><span class="sxs-lookup"><span data-stu-id="ff06e-177">Typically should not be used to convert a string into a custom type, a [`TypeConverter`](https://docs.microsoft.com//dotnet/api/system.componentmodel.typeconverter) is usually a better option.</span></span>
