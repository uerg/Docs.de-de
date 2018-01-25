---
uid: web-api/overview/formats-and-model-binding/parameter-binding-in-aspnet-web-api
title: Parameterbindung in ASP.NET Web-API | Microsoft Docs
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/11/2013
ms.topic: article
ms.assetid: e42c8388-04ed-4341-9fdb-41b1b4c06320
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/formats-and-model-binding/parameter-binding-in-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: 5aa532137436922519c86246ebfa834910ac0d86
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/24/2018
---
<a name="parameter-binding-in-aspnet-web-api"></a><span data-ttu-id="7d3ab-102">Parameterbindung in ASP.NET Web-API</span><span class="sxs-lookup"><span data-stu-id="7d3ab-102">Parameter Binding in ASP.NET Web API</span></span>
====================
<span data-ttu-id="7d3ab-103">durch [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="7d3ab-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="7d3ab-104">Beim Web-API auf einem Domänencontroller eine Methode aufruft, müssen sie Werte für die Parameter, einen so genannten festgelegt *Bindung*.</span><span class="sxs-lookup"><span data-stu-id="7d3ab-104">When Web API calls a method on a controller, it must set values for the parameters, a process called *binding*.</span></span> <span data-ttu-id="7d3ab-105">In diesem Artikel wird beschrieben, wie Web-API-Parameter gebunden und Anpassung des Bindungsvorgangs.</span><span class="sxs-lookup"><span data-stu-id="7d3ab-105">This article describes how Web API binds parameters, and how you can customize the binding process.</span></span>

<span data-ttu-id="7d3ab-106">Standardmäßig verwendet die Web-API die folgenden Regeln zum Binden von Parametern:</span><span class="sxs-lookup"><span data-stu-id="7d3ab-106">By default, Web API uses the following rules to bind parameters:</span></span>

- <span data-ttu-id="7d3ab-107">Wenn der Parameter ein "simple"-Typ ist, versucht Web-API zum Abrufen des Werts aus dem URI.</span><span class="sxs-lookup"><span data-stu-id="7d3ab-107">If the parameter is a "simple" type, Web API tries to get the value from the URI.</span></span> <span data-ttu-id="7d3ab-108">Zu einfachen Typen gehören .NET [Grundtypen](https://msdn.microsoft.com/library/system.type.isprimitive.aspx) (**Int**, **Bool**, **doppelte**usw.), plus **TimeSpan**, **"DateTime"**, **Guid**, **decimal**, und **Zeichenfolge**, *plus* alle Geben Sie einen Typkonverter, der aus einer Zeichenfolge konvertiert werden kann.</span><span class="sxs-lookup"><span data-stu-id="7d3ab-108">Simple types include the .NET [primitive types](https://msdn.microsoft.com/library/system.type.isprimitive.aspx) (**int**, **bool**, **double**, and so forth), plus **TimeSpan**, **DateTime**, **Guid**, **decimal**, and **string**, *plus* any type with a type converter that can convert from a string.</span></span> <span data-ttu-id="7d3ab-109">(Weitere Informationen zu einem späteren Zeitpunkt den Einsatz von Typkonvertern.)</span><span class="sxs-lookup"><span data-stu-id="7d3ab-109">(More about type converters later.)</span></span>
- <span data-ttu-id="7d3ab-110">Verwenden Sie für komplexe Typen, Web-API zum Lesen des Werts aus dem Nachrichtentext versucht, eine [medientypformatierer](media-formatters.md).</span><span class="sxs-lookup"><span data-stu-id="7d3ab-110">For complex types, Web API tries to read the value from the message body, using a [media-type formatter](media-formatters.md).</span></span>

<span data-ttu-id="7d3ab-111">Hier ist z. B. eine typische Web-API-Controller-Methode:</span><span class="sxs-lookup"><span data-stu-id="7d3ab-111">For example, here is a typical Web API controller method:</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample1.cs)]

<span data-ttu-id="7d3ab-112">Die *Id* Parameter ist ein &quot;einfache&quot; eingeben, damit der Web-API versucht, den Wert aus der Anforderungs-URI abzurufen.</span><span class="sxs-lookup"><span data-stu-id="7d3ab-112">The *id* parameter is a &quot;simple&quot; type, so Web API tries to get the value from the request URI.</span></span> <span data-ttu-id="7d3ab-113">Die *Element* Parameter ist ein komplexer Typ, damit Web-API einen medientypformatierer verwendet, um den Wert aus dem Anforderungstext zu lesen.</span><span class="sxs-lookup"><span data-stu-id="7d3ab-113">The *item* parameter is a complex type, so Web API uses a media-type formatter to read the value from the request body.</span></span>

<span data-ttu-id="7d3ab-114">Um einen Wert aus dem URI zu erhalten, sucht Web-API in der Routendaten und der URI-Abfragezeichenfolge.</span><span class="sxs-lookup"><span data-stu-id="7d3ab-114">To get a value from the URI, Web API looks in the route data and the URI query string.</span></span> <span data-ttu-id="7d3ab-115">Die Routendaten werden aufgefüllt, wenn das routing System den URI analysiert und ihn mit einer Route vergleicht.</span><span class="sxs-lookup"><span data-stu-id="7d3ab-115">The route data is populated when the routing system parses the URI and matches it to a route.</span></span> <span data-ttu-id="7d3ab-116">Weitere Informationen finden Sie unter [Routing und Aktionsauswahl](../web-api-routing-and-actions/routing-and-action-selection.md).</span><span class="sxs-lookup"><span data-stu-id="7d3ab-116">For more information, see [Routing and Action Selection](../web-api-routing-and-actions/routing-and-action-selection.md).</span></span>

<span data-ttu-id="7d3ab-117">Im Rest dieses Artikels zeige ich, wie Sie dem modellbindungsprozess anpassen können.</span><span class="sxs-lookup"><span data-stu-id="7d3ab-117">In the rest of this article, I'll show how you can customize the model binding process.</span></span> <span data-ttu-id="7d3ab-118">Für komplexe Typen, sollten Sie jedoch nach Möglichkeit medientypformatierer.</span><span class="sxs-lookup"><span data-stu-id="7d3ab-118">For complex types, however, consider using media-type formatters whenever possible.</span></span> <span data-ttu-id="7d3ab-119">Ein Hauptprinzip von HTTP ist, dass Ressourcen im Nachrichtentext, mithilfe der inhaltsaushandlung an die Darstellung der Ressource gesendet werden.</span><span class="sxs-lookup"><span data-stu-id="7d3ab-119">A key principle of HTTP is that resources are sent in the message body, using content negotiation to specify the representation of the resource.</span></span> <span data-ttu-id="7d3ab-120">Medientypformatierer wurden für genau diesen Zweck konzipiert.</span><span class="sxs-lookup"><span data-stu-id="7d3ab-120">Media-type formatters were designed for exactly this purpose.</span></span>

## <a name="using-fromuri"></a><span data-ttu-id="7d3ab-121">Verwenden [FromUri]</span><span class="sxs-lookup"><span data-stu-id="7d3ab-121">Using [FromUri]</span></span>

<span data-ttu-id="7d3ab-122">Um Web-API zum Lesen eines komplexen Typs aus dem URI zu erzwingen, Hinzufügen der **[FromUri]** -Attribut für den Parameter.</span><span class="sxs-lookup"><span data-stu-id="7d3ab-122">To force Web API to read a complex type from the URI, add the **[FromUri]** attribute to the parameter.</span></span> <span data-ttu-id="7d3ab-123">Das folgende Beispiel definiert eine `GeoPoint` Typs führen, sowie einen Controllermethode, die Ruft die `GeoPoint` aus dem URI.</span><span class="sxs-lookup"><span data-stu-id="7d3ab-123">The following example defines a `GeoPoint` type, along with a controller method that gets the `GeoPoint` from the URI.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample2.cs)]

<span data-ttu-id="7d3ab-124">Der Client kann die Breiten- und Längengrad Werte in der Abfragezeichenfolge abgelegt und Web-API verwenden sie zum Erstellen einer `GeoPoint`.</span><span class="sxs-lookup"><span data-stu-id="7d3ab-124">The client can put the Latitude and Longitude values in the query string and Web API will use them to construct a `GeoPoint`.</span></span> <span data-ttu-id="7d3ab-125">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="7d3ab-125">For example:</span></span>

`http://localhost/api/values/?Latitude=47.678558&Longitude=-122.130989`

## <a name="using-frombody"></a><span data-ttu-id="7d3ab-126">Verwenden [FromBody]</span><span class="sxs-lookup"><span data-stu-id="7d3ab-126">Using [FromBody]</span></span>

<span data-ttu-id="7d3ab-127">Um Web-API zum Lesen von eines einfachen Typs aus dem Anforderungstext zu erzwingen, Hinzufügen der **[FromBody]** -Attribut für den Parameter:</span><span class="sxs-lookup"><span data-stu-id="7d3ab-127">To force Web API to read a simple type from the request body, add the **[FromBody]** attribute to the parameter:</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample3.cs)]

<span data-ttu-id="7d3ab-128">In diesem Beispiel wird Web-API einen medientypformatierer verwenden, lesen Sie den Wert der *Namen* aus dem Anforderungstext.</span><span class="sxs-lookup"><span data-stu-id="7d3ab-128">In this example, Web API will use a media-type formatter to read the value of *name* from the request body.</span></span> <span data-ttu-id="7d3ab-129">Hier ist eine Beispiel für Client-Anforderung.</span><span class="sxs-lookup"><span data-stu-id="7d3ab-129">Here is an example client request.</span></span>

[!code-console[Main](parameter-binding-in-aspnet-web-api/samples/sample4.cmd)]

<span data-ttu-id="7d3ab-130">Wenn ein Parameter [FromBody] verfügt, verwendet Web-API Content-Type-Headers, um ein Formatierungsprogramm auszuwählen.</span><span class="sxs-lookup"><span data-stu-id="7d3ab-130">When a parameter has [FromBody], Web API uses the Content-Type header to select a formatter.</span></span> <span data-ttu-id="7d3ab-131">In diesem Beispiel wird der Inhaltstyp &quot;Application/Json&quot; und der Anforderungstext ist eine unformatierte JSON-Zeichenfolge (kein JSON-Objekt).</span><span class="sxs-lookup"><span data-stu-id="7d3ab-131">In this example, the content type is &quot;application/json&quot; and the request body is a raw JSON string (not a JSON object).</span></span>

<span data-ttu-id="7d3ab-132">Über höchstens einen Parameter aus dem Nachrichtentext lesen darf.</span><span class="sxs-lookup"><span data-stu-id="7d3ab-132">At most one parameter is allowed to read from the message body.</span></span> <span data-ttu-id="7d3ab-133">Daher wird dies nicht funktioniert:</span><span class="sxs-lookup"><span data-stu-id="7d3ab-133">So this will not work:</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample5.cs)]

<span data-ttu-id="7d3ab-134">Der Grund für diese Regel ist, dass der Anforderungstext in einem nicht gepufferten Datenstrom gespeichert werden kann, die nur einmal gelesen werden kann.</span><span class="sxs-lookup"><span data-stu-id="7d3ab-134">The reason for this rule is that the request body might be stored in a non-buffered stream that can only be read once.</span></span>

## <a name="type-converters"></a><span data-ttu-id="7d3ab-135">Typkonverter</span><span class="sxs-lookup"><span data-stu-id="7d3ab-135">Type Converters</span></span>

<span data-ttu-id="7d3ab-136">Sie können die Web-API, die eine Klasse als ein einfacher Typ behandelt werden sollen (damit, dass Web-API versucht, sie aus dem URI Binden) vornehmen, durch das Erstellen einer **TypeConverter** und bietet eine zeichenfolgenkonvertierung.</span><span class="sxs-lookup"><span data-stu-id="7d3ab-136">You can make Web API treat a class as a simple type (so that Web API will try to bind it from the URI) by creating a **TypeConverter** and providing a string conversion.</span></span>

<span data-ttu-id="7d3ab-137">Der folgende code zeigt eine `GeoPoint` -Klasse, die einen geographischen Punkt darstellt zuzüglich einer **TypeConverter** , die aus Zeichenfolgen konvertiert `GeoPoint` Instanzen.</span><span class="sxs-lookup"><span data-stu-id="7d3ab-137">The following code shows a `GeoPoint` class that represents a geographical point, plus a **TypeConverter** that converts from strings to `GeoPoint` instances.</span></span> <span data-ttu-id="7d3ab-138">Die `GeoPoint` Klasse ergänzt wird, mit einem **[TypeConverter]** -Attribut den Typkonverter angeben.</span><span class="sxs-lookup"><span data-stu-id="7d3ab-138">The `GeoPoint` class is decorated with a **[TypeConverter]** attribute to specify the type converter.</span></span> <span data-ttu-id="7d3ab-139">(In diesem Beispiel wurde von Mike Stall des Blogbeitrag Inspiriertes [Gewusst wie: Binden an benutzerdefinierten Objekte in Aktion Signaturen in MVC/WebAPI](https://blogs.msdn.com/b/jmstall/archive/2012/04/20/how-to-bind-to-custom-objects-in-action-signatures-in-mvc-webapi.aspx).)</span><span class="sxs-lookup"><span data-stu-id="7d3ab-139">(This example was inspired by Mike Stall's blog post [How to bind to custom objects in action signatures in MVC/WebAPI](https://blogs.msdn.com/b/jmstall/archive/2012/04/20/how-to-bind-to-custom-objects-in-action-signatures-in-mvc-webapi.aspx).)</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample6.cs)]

<span data-ttu-id="7d3ab-140">Nachdem Web-API behandelt, `GeoPoint` als ein einfacher Typ, d. h. es wird versucht, binden `GeoPoint` Parameter aus dem URI.</span><span class="sxs-lookup"><span data-stu-id="7d3ab-140">Now Web API will treat `GeoPoint` as a simple type, meaning it will try to bind `GeoPoint` parameters from the URI.</span></span> <span data-ttu-id="7d3ab-141">Sie müssen nicht enthalten sein **[FromUri]** für den Parameter.</span><span class="sxs-lookup"><span data-stu-id="7d3ab-141">You don't need to include **[FromUri]** on the parameter.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample7.cs)]

<span data-ttu-id="7d3ab-142">Der Client kann die Methode mit einem URI wie folgt aufrufen:</span><span class="sxs-lookup"><span data-stu-id="7d3ab-142">The client can invoke the method with a URI like this:</span></span>

`http://localhost/api/values/?location=47.678558,-122.130989`

## <a name="model-binders"></a><span data-ttu-id="7d3ab-143">Modellbinder</span><span class="sxs-lookup"><span data-stu-id="7d3ab-143">Model Binders</span></span>

<span data-ttu-id="7d3ab-144">Erstellen Sie einen benutzerdefinierten Modellbinder ist eine flexiblere Option als einen Typkonverter werden.</span><span class="sxs-lookup"><span data-stu-id="7d3ab-144">A more flexible option than a type converter is to create a custom model binder.</span></span> <span data-ttu-id="7d3ab-145">Mit einem Modellbinder haben Sie Zugriff auf die HTTP-Anforderung und die Beschreibung der Aktion sowie die unformatierte Werte von den Routendaten.</span><span class="sxs-lookup"><span data-stu-id="7d3ab-145">With a model binder, you have access to things like the HTTP request, the action description, and the raw values from the route data.</span></span>

<span data-ttu-id="7d3ab-146">Um einen Modellbinder zu erstellen, implementieren die **IModelBinder** Schnittstelle.</span><span class="sxs-lookup"><span data-stu-id="7d3ab-146">To create a model binder, implement the **IModelBinder** interface.</span></span> <span data-ttu-id="7d3ab-147">Diese Schnittstelle definiert eine einzelne Methode **BindModel**:</span><span class="sxs-lookup"><span data-stu-id="7d3ab-147">This interface defines a single method, **BindModel**:</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample8.cs)]

<span data-ttu-id="7d3ab-148">Hier ist ein Modellbinder für `GeoPoint` Objekte.</span><span class="sxs-lookup"><span data-stu-id="7d3ab-148">Here is a model binder for `GeoPoint` objects.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample9.cs)]

<span data-ttu-id="7d3ab-149">Ein Modellbinder ruft unformatierte Eingabewerten aus einer *Wertanbieter*.</span><span class="sxs-lookup"><span data-stu-id="7d3ab-149">A model binder gets raw input values from a *value provider*.</span></span> <span data-ttu-id="7d3ab-150">Dieser Entwurf trennt werden zwei verschiedene Funktionen:</span><span class="sxs-lookup"><span data-stu-id="7d3ab-150">This design separates two distinct functions:</span></span>

- <span data-ttu-id="7d3ab-151">Der Wertanbieter die HTTP-Anforderung akzeptiert und ein Wörterbuch von Schlüssel-Wert-Paaren aufgefüllt.</span><span class="sxs-lookup"><span data-stu-id="7d3ab-151">The value provider takes the HTTP request and populates a dictionary of key-value pairs.</span></span>
- <span data-ttu-id="7d3ab-152">Der Modellbinder verwendet dieses Wörterbuch aufweist und zum Auffüllen des Modells.</span><span class="sxs-lookup"><span data-stu-id="7d3ab-152">The model binder uses this dictionary to populate the model.</span></span>

<span data-ttu-id="7d3ab-153">Der Standard-Wertanbieter in Web-API ruft die Routendaten und der Abfragezeichenfolge Werte ab.</span><span class="sxs-lookup"><span data-stu-id="7d3ab-153">The default value provider in Web API gets values from the route data and the query string.</span></span> <span data-ttu-id="7d3ab-154">Wenn der URI ist z. B. `http://localhost/api/values/1?location=48,-122`, Wertanbieter erstellt der folgenden Schlüssel-Wert-Paaren:</span><span class="sxs-lookup"><span data-stu-id="7d3ab-154">For example, if the URI is `http://localhost/api/values/1?location=48,-122`, the value provider creates the following key-value pairs:</span></span>

- <span data-ttu-id="7d3ab-155">id = &quot;1&quot;</span><span class="sxs-lookup"><span data-stu-id="7d3ab-155">id = &quot;1&quot;</span></span>
- <span data-ttu-id="7d3ab-156">Speicherort = &quot;48,122&quot;</span><span class="sxs-lookup"><span data-stu-id="7d3ab-156">location = &quot;48,122&quot;</span></span>

<span data-ttu-id="7d3ab-157">(Ich verwende die Standardvorlage für die Route, also vorausgesetzt &quot;api / {Controller} / {Id}&quot;.)</span><span class="sxs-lookup"><span data-stu-id="7d3ab-157">(I'm assuming the default route template, which is &quot;api/{controller}/{id}&quot;.)</span></span>

<span data-ttu-id="7d3ab-158">Der Name des Parameters gebunden wird gespeichert, der **ModelBindingContext.ModelName** Eigenschaft.</span><span class="sxs-lookup"><span data-stu-id="7d3ab-158">The name of the parameter to bind is stored in the **ModelBindingContext.ModelName** property.</span></span> <span data-ttu-id="7d3ab-159">Der Modellbinder sucht nach einem Schlüssel mit diesem Wert im Wörterbuch.</span><span class="sxs-lookup"><span data-stu-id="7d3ab-159">The model binder looks for a key with this value in the dictionary.</span></span> <span data-ttu-id="7d3ab-160">Wenn der Wert vorhanden ist, und kann, in konvertiert werden einem `GeoPoint`, der Modellbinder weist den gebundenen Wert, der die **ModelBindingContext.Model** Eigenschaft.</span><span class="sxs-lookup"><span data-stu-id="7d3ab-160">If the value exists and can be converted into a `GeoPoint`, the model binder assigns the bound value to the **ModelBindingContext.Model** property.</span></span>

<span data-ttu-id="7d3ab-161">Beachten Sie, dass die modellbindung nicht auf eine einfache typkonvertierung beschränkt ist.</span><span class="sxs-lookup"><span data-stu-id="7d3ab-161">Notice that the model binder is not limited to a simple type conversion.</span></span> <span data-ttu-id="7d3ab-162">In diesem Beispiel der Modellbinder sucht zuerst in eine Tabelle mit den bekannten Speicherorten, und wenn dies fehlschlägt, verwendet typkonvertierung.</span><span class="sxs-lookup"><span data-stu-id="7d3ab-162">In this example, the model binder first looks in a table of known locations, and if that fails, it uses type conversion.</span></span>

<span data-ttu-id="7d3ab-163">**Den Modellbinder festlegen**</span><span class="sxs-lookup"><span data-stu-id="7d3ab-163">**Setting the Model Binder**</span></span>

<span data-ttu-id="7d3ab-164">Es gibt mehrere Möglichkeiten, einen Modellbinder festzulegen.</span><span class="sxs-lookup"><span data-stu-id="7d3ab-164">There are several ways to set a model binder.</span></span> <span data-ttu-id="7d3ab-165">Sie können fügen Sie zunächst eine **[ModelBinder]** -Attribut für den Parameter.</span><span class="sxs-lookup"><span data-stu-id="7d3ab-165">First, you can add a **[ModelBinder]** attribute to the parameter.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample10.cs)]

<span data-ttu-id="7d3ab-166">Sie können auch Hinzufügen einer **[ModelBinder]** -Attribut auf den Typ.</span><span class="sxs-lookup"><span data-stu-id="7d3ab-166">You can also add a **[ModelBinder]** attribute to the type.</span></span> <span data-ttu-id="7d3ab-167">Web-API wird den angegebene Modellbinder für alle Parameter dieses Typs verwendet.</span><span class="sxs-lookup"><span data-stu-id="7d3ab-167">Web API will use the specified model binder for all parameters of that type.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample11.cs)]

<span data-ttu-id="7d3ab-168">Schließlich können Sie einen Modellbinder Anbieter zum Hinzufügen der **HttpConfiguration**.</span><span class="sxs-lookup"><span data-stu-id="7d3ab-168">Finally, you can add a model-binder provider to the **HttpConfiguration**.</span></span> <span data-ttu-id="7d3ab-169">Ein Modellbinder Anbieter ist einfach eine Factoryklasse, die einen Modellbinder erstellt.</span><span class="sxs-lookup"><span data-stu-id="7d3ab-169">A model-binder provider is simply a factory class that creates a model binder.</span></span> <span data-ttu-id="7d3ab-170">Sie können einen Anbieter erstellen, durch Ableiten von der [ModelBinderProvider](https://msdn.microsoft.com/library/system.web.http.modelbinding.modelbinderprovider.aspx) Klasse.</span><span class="sxs-lookup"><span data-stu-id="7d3ab-170">You can create a provider by deriving from the [ModelBinderProvider](https://msdn.microsoft.com/library/system.web.http.modelbinding.modelbinderprovider.aspx) class.</span></span> <span data-ttu-id="7d3ab-171">Wenn Ihre Modellbinder ein einzelnes Typs behandelt, es ist jedoch einfacher mithilfe die integrierten **SimpleModelBinderProvider**, die für diesen Zweck dient.</span><span class="sxs-lookup"><span data-stu-id="7d3ab-171">However, if your model binder handles a single type, it's easier to use the built-in **SimpleModelBinderProvider**, which is designed for this purpose.</span></span> <span data-ttu-id="7d3ab-172">Dies wird im folgenden Code veranschaulicht.</span><span class="sxs-lookup"><span data-stu-id="7d3ab-172">The following code shows how to do this.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample12.cs)]

<span data-ttu-id="7d3ab-173">Mit einem Anbieter modellbindungs-Sie benötigen Sie weitere hinzufügen der **[ModelBinder]** -Attribut für den Parameter, Web-API zu informieren, dass es einen Modellbinder und nicht auf einem medientypformatierer verwendet werden sollen.</span><span class="sxs-lookup"><span data-stu-id="7d3ab-173">With a model-binding provider, you still need to add the **[ModelBinder]** attribute to the parameter, to tell Web API that it should use a model binder and not a media-type formatter.</span></span> <span data-ttu-id="7d3ab-174">Jetzt müssen Sie jedoch nicht den Typ der modellbindung im Attribut angeben:</span><span class="sxs-lookup"><span data-stu-id="7d3ab-174">But now you don't need to specify the type of model binder in the attribute:</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample13.cs)]

## <a name="value-providers"></a><span data-ttu-id="7d3ab-175">Wertanbieter</span><span class="sxs-lookup"><span data-stu-id="7d3ab-175">Value Providers</span></span>

<span data-ttu-id="7d3ab-176">Ich schon gesagt habe, ein Modellbinder von einem Wertanbieter Ruft Werte ab.</span><span class="sxs-lookup"><span data-stu-id="7d3ab-176">I mentioned that a model binder gets values from a value provider.</span></span> <span data-ttu-id="7d3ab-177">Um einen benutzerdefinierten Wertanbieter zu schreiben, implementieren die **IValueProvider** Schnittstelle.</span><span class="sxs-lookup"><span data-stu-id="7d3ab-177">To write a custom value provider, implement the **IValueProvider** interface.</span></span> <span data-ttu-id="7d3ab-178">Hier ist ein Beispiel, das Werte aus den Cookies in der Anforderung abruft:</span><span class="sxs-lookup"><span data-stu-id="7d3ab-178">Here is an example that pulls values from the cookies in the request:</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample14.cs)]

<span data-ttu-id="7d3ab-179">Sie müssen auch eine Wertanbieterfactory zu erstellen, durch Ableiten von der **ValueProviderFactory** Klasse.</span><span class="sxs-lookup"><span data-stu-id="7d3ab-179">You also need to create a value provider factory by deriving from the **ValueProviderFactory** class.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample15.cs)]

<span data-ttu-id="7d3ab-180">Hinzufügen der Wertanbieterfactory auf die **HttpConfiguration** wie folgt.</span><span class="sxs-lookup"><span data-stu-id="7d3ab-180">Add the value provider factory to the **HttpConfiguration** as follows.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample16.cs)]

<span data-ttu-id="7d3ab-181">Web-API verfasst aller Wertanbieter also wenn aufruft, ein Modellbinder **ValueProvider.GetValue**, der Modellbinder erhält den Wert aus der ersten Wertanbieter, die daraus resultieren kann.</span><span class="sxs-lookup"><span data-stu-id="7d3ab-181">Web API composes all of the value providers, so when a model binder calls **ValueProvider.GetValue**, the model binder receives the value from the first value provider that is able to produce it.</span></span>

<span data-ttu-id="7d3ab-182">Sie können alternativ die Wertanbieterfactory Ebene der Parameter festlegen, mit der **ValueProvider** -Attribut wie folgt:</span><span class="sxs-lookup"><span data-stu-id="7d3ab-182">Alternatively, you can set the value provider factory at the parameter level by using the **ValueProvider** attribute, as follows:</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample17.cs)]

<span data-ttu-id="7d3ab-183">Dies teilt Web-API, mit der angegebenen Wertanbieterfactory wurden die modellbindung verwenden und nicht auf eine der anderen registrierten Wertanbieter verwenden.</span><span class="sxs-lookup"><span data-stu-id="7d3ab-183">This tells Web API to use model binding with the specified value provider factory, and not to use any of the other registered value providers.</span></span>

## <a name="httpparameterbinding"></a><span data-ttu-id="7d3ab-184">HttpParameterBinding</span><span class="sxs-lookup"><span data-stu-id="7d3ab-184">HttpParameterBinding</span></span>

<span data-ttu-id="7d3ab-185">Modellbinder entsprechen einer bestimmten Instanz eines allgemeineren Mechanismus.</span><span class="sxs-lookup"><span data-stu-id="7d3ab-185">Model binders are a specific instance of a more general mechanism.</span></span> <span data-ttu-id="7d3ab-186">Bei Betrachtung der **[ModelBinder]** -Attribut, sehen Sie, dass es von der abstrakten abgeleitet **ParameterBindingAttribute** Klasse.</span><span class="sxs-lookup"><span data-stu-id="7d3ab-186">If you look at the **[ModelBinder]** attribute, you will see that it derives from the abstract **ParameterBindingAttribute** class.</span></span> <span data-ttu-id="7d3ab-187">Diese Klasse definiert eine einzelne Methode **GetBinding**, welche gibt eine **HttpParameterBinding** Objekt:</span><span class="sxs-lookup"><span data-stu-id="7d3ab-187">This class defines a single method, **GetBinding**, which returns an **HttpParameterBinding** object:</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample18.cs)]

<span data-ttu-id="7d3ab-188">Ein **HttpParameterBinding** ist verantwortlich für einen Parameter auf einen Wert zu binden.</span><span class="sxs-lookup"><span data-stu-id="7d3ab-188">An **HttpParameterBinding** is responsible for binding a parameter to a value.</span></span> <span data-ttu-id="7d3ab-189">Im Fall von **[ModelBinder]**, das Attribut gibt eine **HttpParameterBinding** Implementierung, die verwendet ein **IModelBinder** die tatsächliche Bindung ausführen.</span><span class="sxs-lookup"><span data-stu-id="7d3ab-189">In the case of **[ModelBinder]**, the attribute returns an **HttpParameterBinding** implementation that uses an **IModelBinder** to perform the actual binding.</span></span> <span data-ttu-id="7d3ab-190">Sie können auch implementieren Ihre eigenen **HttpParameterBinding**.</span><span class="sxs-lookup"><span data-stu-id="7d3ab-190">You can also implement your own **HttpParameterBinding**.</span></span>

<span data-ttu-id="7d3ab-191">Nehmen wir beispielsweise an, die Sie ETags aus abrufen möchten `if-match` und `if-none-match` Header in der Anforderung.</span><span class="sxs-lookup"><span data-stu-id="7d3ab-191">For example, suppose you want to get ETags from `if-match` and `if-none-match` headers in the request.</span></span> <span data-ttu-id="7d3ab-192">Wir beginnen, durch die Definition einer Klasse zum Darstellen des ETags.</span><span class="sxs-lookup"><span data-stu-id="7d3ab-192">We'll start by defining a class to represent ETags.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample19.cs)]

<span data-ttu-id="7d3ab-193">Definieren wir eine Enumeration angeben, ob das ETag des abzurufenden auch die `if-match` Header oder der `if-none-match` Header.</span><span class="sxs-lookup"><span data-stu-id="7d3ab-193">We'll also define an enumeration to indicate whether to get the ETag from the `if-match` header or the `if-none-match` header.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample20.cs)]

<span data-ttu-id="7d3ab-194">Hier ist ein **HttpParameterBinding** , ruft das ETag aus den gewünschten Headertyp als ab und bindet sie an einen Parameter vom Typ ETag:</span><span class="sxs-lookup"><span data-stu-id="7d3ab-194">Here is an **HttpParameterBinding** that gets the ETag from the desired header and binds it to a parameter of type ETag:</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample21.cs)]

<span data-ttu-id="7d3ab-195">Die **ExecuteBindingAsync** -Methode führt die Bindung.</span><span class="sxs-lookup"><span data-stu-id="7d3ab-195">The **ExecuteBindingAsync** method does the binding.</span></span> <span data-ttu-id="7d3ab-196">Fügen Sie innerhalb dieser Methode den gebundenen Parameterwert die **ActionArgument** Wörterbuch in das **HttpActionContext**.</span><span class="sxs-lookup"><span data-stu-id="7d3ab-196">Within this method, add the bound parameter value to the **ActionArgument** dictionary in the **HttpActionContext**.</span></span>

> [!NOTE]
> <span data-ttu-id="7d3ab-197">Wenn Ihre **ExecuteBindingAsync** Methode liest den Text der Anforderungsnachricht, überschreiben die **WillReadBody** -Eigenschaft auf "true" zurückgeben.</span><span class="sxs-lookup"><span data-stu-id="7d3ab-197">If your **ExecuteBindingAsync** method reads the body of the request message, override the **WillReadBody** property to return true.</span></span> <span data-ttu-id="7d3ab-198">Der Anforderungstext ist möglicherweise, dass ein ungepufferten Datenstrom, der nur einmal gelesen werden kann, sodass Web-API einer Regel, dass höchstens einer Bindung erzwingt den Nachrichtentext lesen kann.</span><span class="sxs-lookup"><span data-stu-id="7d3ab-198">The request body might be an unbuffered stream that can only be read once, so Web API enforces a rule that at most one binding can read the message body.</span></span>


<span data-ttu-id="7d3ab-199">Anwenden ein benutzerdefinierten **HttpParameterBinding**, können Sie ein Attribut, das von abgeleitet ist definieren **ParameterBindingAttribute**.</span><span class="sxs-lookup"><span data-stu-id="7d3ab-199">To apply a custom **HttpParameterBinding**, you can define an attribute that derives from **ParameterBindingAttribute**.</span></span> <span data-ttu-id="7d3ab-200">Für `ETagParameterBinding`, definieren wir zwei Attribute: eine für `if-match` Kopf- und eine für `if-none-match` Header.</span><span class="sxs-lookup"><span data-stu-id="7d3ab-200">For `ETagParameterBinding`, we'll define two attributes, one for `if-match` headers and one for `if-none-match` headers.</span></span> <span data-ttu-id="7d3ab-201">Beide werden von einer abstrakten Klasse abgeleitet.</span><span class="sxs-lookup"><span data-stu-id="7d3ab-201">Both derive from an abstract base class.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample22.cs)]

<span data-ttu-id="7d3ab-202">Hier ist eine Controllermethode, verwendet die `[IfNoneMatch]` Attribut.</span><span class="sxs-lookup"><span data-stu-id="7d3ab-202">Here is a controller method that uses the `[IfNoneMatch]` attribute.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample23.cs)]

<span data-ttu-id="7d3ab-203">Neben **ParameterBindingAttribute**, es ist eine andere Hook zum Hinzufügen eines benutzerdefiniertes **HttpParameterBinding**.</span><span class="sxs-lookup"><span data-stu-id="7d3ab-203">Besides **ParameterBindingAttribute**, there is another hook for adding a custom **HttpParameterBinding**.</span></span> <span data-ttu-id="7d3ab-204">Auf der **HttpConfiguration** -Objekt, das **ParameterBindingRules** Eigenschaft ist eine Sammlung von Anomymous Funktionen des Typs (**HttpParameterDescriptor**  - &gt; **HttpParameterBinding**).</span><span class="sxs-lookup"><span data-stu-id="7d3ab-204">On the **HttpConfiguration** object, the **ParameterBindingRules** property is a collection of anomymous functions of type (**HttpParameterDescriptor** -&gt; **HttpParameterBinding**).</span></span> <span data-ttu-id="7d3ab-205">Sie können z. B. eine Regel, ein ETag-Parameter für eine GET-Methode verwendet, hinzufügen `ETagParameterBinding` mit `if-none-match`:</span><span class="sxs-lookup"><span data-stu-id="7d3ab-205">For example, you could add a rule that any ETag parameter on a GET method uses `ETagParameterBinding` with `if-none-match`:</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample24.cs)]

<span data-ttu-id="7d3ab-206">Die Funktion zurückgeben sollte `null` für Parameter, die die Bindung, in denen nicht anwendbar ist.</span><span class="sxs-lookup"><span data-stu-id="7d3ab-206">The function should return `null` for parameters where the binding is not applicable.</span></span>

## <a name="iactionvaluebinder"></a><span data-ttu-id="7d3ab-207">IActionValueBinder</span><span class="sxs-lookup"><span data-stu-id="7d3ab-207">IActionValueBinder</span></span>

<span data-ttu-id="7d3ab-208">Der gesamte parameterbindung Prozess wird gesteuert, von einem Dienst austauschbare **IActionValueBinder**.</span><span class="sxs-lookup"><span data-stu-id="7d3ab-208">The entire parameter-binding process is controlled by a pluggable service, **IActionValueBinder**.</span></span> <span data-ttu-id="7d3ab-209">Die standardmäßige Implementierung des **IActionValueBinder** bewirkt Folgendes:</span><span class="sxs-lookup"><span data-stu-id="7d3ab-209">The default implementation of **IActionValueBinder** does the following:</span></span>

1. <span data-ttu-id="7d3ab-210">Suchen Sie nach einer **ParameterBindingAttribute** für den Parameter.</span><span class="sxs-lookup"><span data-stu-id="7d3ab-210">Look for a **ParameterBindingAttribute** on the parameter.</span></span> <span data-ttu-id="7d3ab-211">Dies schließt **[FromBody]**, **[FromUri]**, und **[ModelBinder]**, oder benutzerdefinierte Attribute.</span><span class="sxs-lookup"><span data-stu-id="7d3ab-211">This includes **[FromBody]**, **[FromUri]**, and **[ModelBinder]**, or custom attributes.</span></span>
2. <span data-ttu-id="7d3ab-212">Andernfalls, suchen Sie im **HttpConfiguration.ParameterBindingRules** für eine Funktion, eine Wert ungleich Null zurückgibt **HttpParameterBinding**.</span><span class="sxs-lookup"><span data-stu-id="7d3ab-212">Otherwise, look in **HttpConfiguration.ParameterBindingRules** for a function that returns a non-null **HttpParameterBinding**.</span></span>
3. <span data-ttu-id="7d3ab-213">Verwenden Sie andernfalls die Standardregeln, die ich zuvor beschrieben.</span><span class="sxs-lookup"><span data-stu-id="7d3ab-213">Otherwise, use the default rules that I described previously.</span></span> 

    - <span data-ttu-id="7d3ab-214">Wenn der Parametertyp ist "einfach" oder einen Typkonverter, binden Sie aus dem URI.</span><span class="sxs-lookup"><span data-stu-id="7d3ab-214">If the parameter type is "simple"or has a type converter, bind from the URI.</span></span> <span data-ttu-id="7d3ab-215">Dies entspricht dem Platzieren der **[FromUri]** Attribut für den Parameter.</span><span class="sxs-lookup"><span data-stu-id="7d3ab-215">This is equivalent to putting the **[FromUri]** attribute on the parameter.</span></span>
    - <span data-ttu-id="7d3ab-216">Andernfalls versucht, den Parameter aus dem Nachrichtentext lesen.</span><span class="sxs-lookup"><span data-stu-id="7d3ab-216">Otherwise, try to read the parameter from the message body.</span></span> <span data-ttu-id="7d3ab-217">Dies entspricht dem platzieren **[FromBody]** für den Parameter.</span><span class="sxs-lookup"><span data-stu-id="7d3ab-217">This is equivalent to putting **[FromBody]** on the parameter.</span></span>

<span data-ttu-id="7d3ab-218">Falls gewünscht, konnte Sie ersetzt die gesamte **IActionValueBinder** Service durch eine benutzerdefinierte Implementierung.</span><span class="sxs-lookup"><span data-stu-id="7d3ab-218">If you wanted, you could replace the entire **IActionValueBinder** service with a custom implementation.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="7d3ab-219">Zusätzliche Ressourcen</span><span class="sxs-lookup"><span data-stu-id="7d3ab-219">Additional Resources</span></span>

[<span data-ttu-id="7d3ab-220">Benutzerdefinierte Parameter Bindung-Beispiel</span><span class="sxs-lookup"><span data-stu-id="7d3ab-220">Custom Parameter Binding Sample</span></span>](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/CustomParameterBinding/ReadMe.txt)

<span data-ttu-id="7d3ab-221">Mike Stall wurde zu Web-API-parameterbindung eine gute Reihe von Blogbeiträgen geschrieben:</span><span class="sxs-lookup"><span data-stu-id="7d3ab-221">Mike Stall wrote a good series of blog posts about Web API parameter binding:</span></span>

- [<span data-ttu-id="7d3ab-222">Wie funktioniert Web-API Parameterbindung</span><span class="sxs-lookup"><span data-stu-id="7d3ab-222">How Web API does Parameter Binding</span></span>](https://blogs.msdn.com/b/jmstall/archive/2012/04/16/how-webapi-does-parameter-binding.aspx)
- [<span data-ttu-id="7d3ab-223">MVC-Stil parameterbindung für Web-API</span><span class="sxs-lookup"><span data-stu-id="7d3ab-223">MVC Style parameter binding for Web API</span></span>](https://blogs.msdn.com/b/jmstall/archive/2012/04/18/mvc-style-parameter-binding-for-webapi.aspx)
- [<span data-ttu-id="7d3ab-224">Gewusst wie: Binden an benutzerdefinierten Objekte in Aktion Signaturen in MVC/Web-API</span><span class="sxs-lookup"><span data-stu-id="7d3ab-224">How to bind to custom objects in action signatures in MVC/Web API</span></span>](https://blogs.msdn.com/b/jmstall/archive/2012/04/20/how-to-bind-to-custom-objects-in-action-signatures-in-mvc-webapi.aspx)
- [<span data-ttu-id="7d3ab-225">Vorgehensweise: Erstellen Sie einen benutzerdefinierten Wertanbieter in Web-API</span><span class="sxs-lookup"><span data-stu-id="7d3ab-225">How to create a custom value provider in Web API</span></span>](https://blogs.msdn.com/b/jmstall/archive/2012/04/23/how-to-create-a-custom-value-provider-in-webapi.aspx)
- [<span data-ttu-id="7d3ab-226">Web-API-parameterbindung hinter den Kulissen</span><span class="sxs-lookup"><span data-stu-id="7d3ab-226">Web API Parameter binding under the hood</span></span>](https://blogs.msdn.com/b/jmstall/archive/2012/05/11/webapi-parameter-binding-under-the-hood.aspx)
