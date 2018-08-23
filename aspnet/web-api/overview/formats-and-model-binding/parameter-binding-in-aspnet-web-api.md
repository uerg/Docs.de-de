---
uid: web-api/overview/formats-and-model-binding/parameter-binding-in-aspnet-web-api
title: Parameterbindung in der ASP.NET Web-API | Microsoft-Dokumentation
author: MikeWasson
description: ''
ms.author: riande
ms.date: 07/11/2013
ms.assetid: e42c8388-04ed-4341-9fdb-41b1b4c06320
msc.legacyurl: /web-api/overview/formats-and-model-binding/parameter-binding-in-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: 4d29f087cd658faf1fadb0d9a85e9f32c03a2b3f
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2018
ms.locfileid: "41838030"
---
<a name="parameter-binding-in-aspnet-web-api"></a><span data-ttu-id="11129-102">Parameterbindung in der ASP.NET Web-API</span><span class="sxs-lookup"><span data-stu-id="11129-102">Parameter Binding in ASP.NET Web API</span></span>
====================
<span data-ttu-id="11129-103">durch [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="11129-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="11129-104">Bei Web-API eine Methode auf einem Controller aufruft, müssen sie festlegen, dass Werte für die Parameter, ein so genanntes *Bindung*.</span><span class="sxs-lookup"><span data-stu-id="11129-104">When Web API calls a method on a controller, it must set values for the parameters, a process called *binding*.</span></span> <span data-ttu-id="11129-105">In diesem Artikel wird beschrieben, wie Web-API-Parameter gebunden wird und wie Sie den Bindungsprozess anpassen können.</span><span class="sxs-lookup"><span data-stu-id="11129-105">This article describes how Web API binds parameters, and how you can customize the binding process.</span></span>

<span data-ttu-id="11129-106">Web-API verwendet standardmäßig die folgenden Regeln, um Parameter zu binden:</span><span class="sxs-lookup"><span data-stu-id="11129-106">By default, Web API uses the following rules to bind parameters:</span></span>

- <span data-ttu-id="11129-107">Wenn der Parameter "einfach" ein, versucht Web-API, den Wert aus dem URI abzurufen.</span><span class="sxs-lookup"><span data-stu-id="11129-107">If the parameter is a "simple" type, Web API tries to get the value from the URI.</span></span> <span data-ttu-id="11129-108">Einfache Typen enthalten, die .NET [primitive Typen](https://msdn.microsoft.com/library/system.type.isprimitive.aspx) (**Int**, **"bool"**, **doppelte**usw.), plus **TimeSpan**, **"DateTime"**, **Guid**, **decimal**, und **Zeichenfolge**, *plus* alle Geben Sie mit einem Typkonverter, der aus einer Zeichenfolge konvertieren kann.</span><span class="sxs-lookup"><span data-stu-id="11129-108">Simple types include the .NET [primitive types](https://msdn.microsoft.com/library/system.type.isprimitive.aspx) (**int**, **bool**, **double**, and so forth), plus **TimeSpan**, **DateTime**, **Guid**, **decimal**, and **string**, *plus* any type with a type converter that can convert from a string.</span></span> <span data-ttu-id="11129-109">(Weitere Informationen zu Typkonvertern weiter unten.)</span><span class="sxs-lookup"><span data-stu-id="11129-109">(More about type converters later.)</span></span>
- <span data-ttu-id="11129-110">Verwenden Sie für komplexe Typen, Web-API versucht, den Wert aus dem Nachrichtentext zu lesen, eine [medientypformatierer](media-formatters.md).</span><span class="sxs-lookup"><span data-stu-id="11129-110">For complex types, Web API tries to read the value from the message body, using a [media-type formatter](media-formatters.md).</span></span>

<span data-ttu-id="11129-111">Hier ist z. B. eine typische Web-API-Controller-Methode:</span><span class="sxs-lookup"><span data-stu-id="11129-111">For example, here is a typical Web API controller method:</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample1.cs)]

<span data-ttu-id="11129-112">Die *Id* -Parameter ist ein &quot;einfache&quot; eingeben, damit die Web-API versucht, den Wert aus der Anforderungs-URI abzurufen.</span><span class="sxs-lookup"><span data-stu-id="11129-112">The *id* parameter is a &quot;simple&quot; type, so Web API tries to get the value from the request URI.</span></span> <span data-ttu-id="11129-113">Die *Element* Parameter ist ein komplexer Typ, damit die Web-API einen medientypformatierer verwendet, um den Wert aus dem Anforderungstext lesen.</span><span class="sxs-lookup"><span data-stu-id="11129-113">The *item* parameter is a complex type, so Web API uses a media-type formatter to read the value from the request body.</span></span>

<span data-ttu-id="11129-114">Rufen Sie einen Wert aus dem URI sieht Web-API in den Routendaten und der URI-Abfragezeichenfolge.</span><span class="sxs-lookup"><span data-stu-id="11129-114">To get a value from the URI, Web API looks in the route data and the URI query string.</span></span> <span data-ttu-id="11129-115">Die Routendaten werden aufgefüllt, wenn das Routingsystem den URI analysiert und ihn mit einer Route vergleicht.</span><span class="sxs-lookup"><span data-stu-id="11129-115">The route data is populated when the routing system parses the URI and matches it to a route.</span></span> <span data-ttu-id="11129-116">Weitere Informationen finden Sie unter [Routing- und Aktionsauswahl](../web-api-routing-and-actions/routing-and-action-selection.md).</span><span class="sxs-lookup"><span data-stu-id="11129-116">For more information, see [Routing and Action Selection](../web-api-routing-and-actions/routing-and-action-selection.md).</span></span>

<span data-ttu-id="11129-117">In der Rest dieses Artikels zeige ich, wie Sie den modellbindungsprozess anpassen können.</span><span class="sxs-lookup"><span data-stu-id="11129-117">In the rest of this article, I'll show how you can customize the model binding process.</span></span> <span data-ttu-id="11129-118">Für komplexe Typen, sollten Sie jedoch nach Möglichkeit medientypformatierer.</span><span class="sxs-lookup"><span data-stu-id="11129-118">For complex types, however, consider using media-type formatters whenever possible.</span></span> <span data-ttu-id="11129-119">Ein Hauptprinzip von HTTP ist, dass Ressourcen im Nachrichtentext, mit der Aushandlung von Inhalten an die Darstellung der Ressource gesendet werden.</span><span class="sxs-lookup"><span data-stu-id="11129-119">A key principle of HTTP is that resources are sent in the message body, using content negotiation to specify the representation of the resource.</span></span> <span data-ttu-id="11129-120">Medientypformatierer wurden für genau diesen Zweck entwickelt.</span><span class="sxs-lookup"><span data-stu-id="11129-120">Media-type formatters were designed for exactly this purpose.</span></span>

## <a name="using-fromuri"></a><span data-ttu-id="11129-121">Verwenden [FromUri]</span><span class="sxs-lookup"><span data-stu-id="11129-121">Using [FromUri]</span></span>

<span data-ttu-id="11129-122">Um Web-API zum Lesen eines komplexen Typs aus dem URI zu erzwingen, Hinzufügen der **[FromUri]** -Attribut auf den Parameter.</span><span class="sxs-lookup"><span data-stu-id="11129-122">To force Web API to read a complex type from the URI, add the **[FromUri]** attribute to the parameter.</span></span> <span data-ttu-id="11129-123">Das folgende Beispiel definiert eine `GeoPoint` Typs führen, sowie eine Controllermethode, die Ruft die `GeoPoint` aus dem URI.</span><span class="sxs-lookup"><span data-stu-id="11129-123">The following example defines a `GeoPoint` type, along with a controller method that gets the `GeoPoint` from the URI.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample2.cs)]

<span data-ttu-id="11129-124">Der Client kann die Werte für Breiten- und Längengrad platzieren, in der Abfragezeichenfolge und Web-API verwenden sie zum Erstellen einer `GeoPoint`.</span><span class="sxs-lookup"><span data-stu-id="11129-124">The client can put the Latitude and Longitude values in the query string and Web API will use them to construct a `GeoPoint`.</span></span> <span data-ttu-id="11129-125">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="11129-125">For example:</span></span>

`http://localhost/api/values/?Latitude=47.678558&Longitude=-122.130989`

## <a name="using-frombody"></a><span data-ttu-id="11129-126">Verwenden [FromBody]</span><span class="sxs-lookup"><span data-stu-id="11129-126">Using [FromBody]</span></span>

<span data-ttu-id="11129-127">Um Web-API, um einen einfachen Typ aus dem Anforderungstext lesen zu erzwingen, Hinzufügen der **[FromBody]** -Attribut auf den Parameter:</span><span class="sxs-lookup"><span data-stu-id="11129-127">To force Web API to read a simple type from the request body, add the **[FromBody]** attribute to the parameter:</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample3.cs)]

<span data-ttu-id="11129-128">In diesem Beispiel-Web-API verwendet einen medientypformatierer, lesen den Wert der *Namen* aus dem Anforderungstext.</span><span class="sxs-lookup"><span data-stu-id="11129-128">In this example, Web API will use a media-type formatter to read the value of *name* from the request body.</span></span> <span data-ttu-id="11129-129">Hier ist eine beispielanforderung für den Client.</span><span class="sxs-lookup"><span data-stu-id="11129-129">Here is an example client request.</span></span>

[!code-console[Main](parameter-binding-in-aspnet-web-api/samples/sample4.cmd)]

<span data-ttu-id="11129-130">Wenn ein Parameter [FromBody] verfügt, verwendet Web-API den Content-Type-Header, um einen Formatierer auszuwählen.</span><span class="sxs-lookup"><span data-stu-id="11129-130">When a parameter has [FromBody], Web API uses the Content-Type header to select a formatter.</span></span> <span data-ttu-id="11129-131">In diesem Beispiel wird der Inhaltstyp &quot;Application/Json&quot; und der Anforderungstext ist eine unformatierte JSON-Zeichenfolge (nicht in ein JSON-Objekt).</span><span class="sxs-lookup"><span data-stu-id="11129-131">In this example, the content type is &quot;application/json&quot; and the request body is a raw JSON string (not a JSON object).</span></span>

<span data-ttu-id="11129-132">Höchstens einen Parameter aus dem Nachrichtentext lesen kann.</span><span class="sxs-lookup"><span data-stu-id="11129-132">At most one parameter is allowed to read from the message body.</span></span> <span data-ttu-id="11129-133">Daher wird dies nicht funktioniert:</span><span class="sxs-lookup"><span data-stu-id="11129-133">So this will not work:</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample5.cs)]

<span data-ttu-id="11129-134">Der Grund für diese Regel ist, dass der Hauptteil der Anforderung in einem nicht gepufferten Stream gespeichert werden kann, die nur einmal gelesen werden kann.</span><span class="sxs-lookup"><span data-stu-id="11129-134">The reason for this rule is that the request body might be stored in a non-buffered stream that can only be read once.</span></span>

## <a name="type-converters"></a><span data-ttu-id="11129-135">Typkonverter</span><span class="sxs-lookup"><span data-stu-id="11129-135">Type Converters</span></span>

<span data-ttu-id="11129-136">Sie können die Web-API, die eine Klasse als ein einfacher Typ behandelt werden sollen (sodass Web-API versucht, aus dem URI bindet, bindet es) vornehmen, durch das Erstellen einer **TypeConverter** und eine zeichenfolgenkonvertierung.</span><span class="sxs-lookup"><span data-stu-id="11129-136">You can make Web API treat a class as a simple type (so that Web API will try to bind it from the URI) by creating a **TypeConverter** and providing a string conversion.</span></span>

<span data-ttu-id="11129-137">Der folgende code zeigt eine `GeoPoint` -Klasse, die einen geografischen Punkt, darstellt sowie **TypeConverter** , der von Zeichenfolgen, die konvertiert `GeoPoint` Instanzen.</span><span class="sxs-lookup"><span data-stu-id="11129-137">The following code shows a `GeoPoint` class that represents a geographical point, plus a **TypeConverter** that converts from strings to `GeoPoint` instances.</span></span> <span data-ttu-id="11129-138">Die `GeoPoint` Klasse ergänzt wird, mit einem **[TypeConverter]** -Attribut den Typkonverter angegeben.</span><span class="sxs-lookup"><span data-stu-id="11129-138">The `GeoPoint` class is decorated with a **[TypeConverter]** attribute to specify the type converter.</span></span> <span data-ttu-id="11129-139">(In diesem Beispiel wurde dadurch inspiriert, Blogbeitrag von Mike Stall des [Gewusst wie: Binden an benutzerdefinierte Objekte in aktionssignaturen in MVC/WebAPI](https://blogs.msdn.com/b/jmstall/archive/2012/04/20/how-to-bind-to-custom-objects-in-action-signatures-in-mvc-webapi.aspx).)</span><span class="sxs-lookup"><span data-stu-id="11129-139">(This example was inspired by Mike Stall's blog post [How to bind to custom objects in action signatures in MVC/WebAPI](https://blogs.msdn.com/b/jmstall/archive/2012/04/20/how-to-bind-to-custom-objects-in-action-signatures-in-mvc-webapi.aspx).)</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample6.cs)]

<span data-ttu-id="11129-140">Jetzt Web-API behandelt `GeoPoint` als einen einfachen Typ, d. h. es wird versucht, binden `GeoPoint` Parameter aus dem URI.</span><span class="sxs-lookup"><span data-stu-id="11129-140">Now Web API will treat `GeoPoint` as a simple type, meaning it will try to bind `GeoPoint` parameters from the URI.</span></span> <span data-ttu-id="11129-141">Sie müssen nicht einschließen **[FromUri]** für den Parameter.</span><span class="sxs-lookup"><span data-stu-id="11129-141">You don't need to include **[FromUri]** on the parameter.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample7.cs)]

<span data-ttu-id="11129-142">Der Client kann die Methode mit einem URI wie folgt aufrufen:</span><span class="sxs-lookup"><span data-stu-id="11129-142">The client can invoke the method with a URI like this:</span></span>

`http://localhost/api/values/?location=47.678558,-122.130989`

## <a name="model-binders"></a><span data-ttu-id="11129-143">Modellbinder</span><span class="sxs-lookup"><span data-stu-id="11129-143">Model Binders</span></span>

<span data-ttu-id="11129-144">Eine Option für mehr Flexibilität als ein Typkonverter ist, um einen benutzerdefinierten Modellbinder zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="11129-144">A more flexible option than a type converter is to create a custom model binder.</span></span> <span data-ttu-id="11129-145">Mit einem Modellbinder haben Sie Zugriff auf Dinge wie die HTTP-Anforderung, die Beschreibung der Aktion und die unformatierte Werte aus den Routendaten.</span><span class="sxs-lookup"><span data-stu-id="11129-145">With a model binder, you have access to things like the HTTP request, the action description, and the raw values from the route data.</span></span>

<span data-ttu-id="11129-146">Um einen Modellbinder erstellen, implementieren die **IModelBinder** Schnittstelle.</span><span class="sxs-lookup"><span data-stu-id="11129-146">To create a model binder, implement the **IModelBinder** interface.</span></span> <span data-ttu-id="11129-147">Diese Schnittstelle definiert eine einzelne Methode, **BindModel**:</span><span class="sxs-lookup"><span data-stu-id="11129-147">This interface defines a single method, **BindModel**:</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample8.cs)]

<span data-ttu-id="11129-148">Hier ist eine modellbindung für `GeoPoint` Objekte.</span><span class="sxs-lookup"><span data-stu-id="11129-148">Here is a model binder for `GeoPoint` objects.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample9.cs)]

<span data-ttu-id="11129-149">Ein Modellbinder Ruft die Rohdaten der Eingabewerten aus einer *Wertanbieter*.</span><span class="sxs-lookup"><span data-stu-id="11129-149">A model binder gets raw input values from a *value provider*.</span></span> <span data-ttu-id="11129-150">Dieser Entwurf trennt werden zwei verschiedene Funktionen:</span><span class="sxs-lookup"><span data-stu-id="11129-150">This design separates two distinct functions:</span></span>

- <span data-ttu-id="11129-151">Der Wertanbieter die HTTP-Anforderung akzeptiert und füllt ein Wörterbuch von Schlüssel-Wert-Paaren.</span><span class="sxs-lookup"><span data-stu-id="11129-151">The value provider takes the HTTP request and populates a dictionary of key-value pairs.</span></span>
- <span data-ttu-id="11129-152">Die modellbindung verwendet dieses Wörterbuch zum Auffüllen des Modells.</span><span class="sxs-lookup"><span data-stu-id="11129-152">The model binder uses this dictionary to populate the model.</span></span>

<span data-ttu-id="11129-153">Der Standardanbieter-Wert in der Web-API ruft Werte aus den Routendaten und die Abfragezeichenfolge ab.</span><span class="sxs-lookup"><span data-stu-id="11129-153">The default value provider in Web API gets values from the route data and the query string.</span></span> <span data-ttu-id="11129-154">Wenn der URI ist z. B. `http://localhost/api/values/1?location=48,-122`, der Wertanbieter erstellt die folgenden Schlüssel-Wert-Paare:</span><span class="sxs-lookup"><span data-stu-id="11129-154">For example, if the URI is `http://localhost/api/values/1?location=48,-122`, the value provider creates the following key-value pairs:</span></span>

- <span data-ttu-id="11129-155">id = &quot;1&quot;</span><span class="sxs-lookup"><span data-stu-id="11129-155">id = &quot;1&quot;</span></span>
- <span data-ttu-id="11129-156">Standort = &quot;48,122&quot;</span><span class="sxs-lookup"><span data-stu-id="11129-156">location = &quot;48,122&quot;</span></span>

<span data-ttu-id="11129-157">(Ich gehe davon aus der Standardvorlage für die Route, handelt es sich &quot;api / {Controller} / {Id}&quot;.)</span><span class="sxs-lookup"><span data-stu-id="11129-157">(I'm assuming the default route template, which is &quot;api/{controller}/{id}&quot;.)</span></span>

<span data-ttu-id="11129-158">Der Name des Parameters, der Bindung wird gespeichert, der **ModelBindingContext.ModelName** Eigenschaft.</span><span class="sxs-lookup"><span data-stu-id="11129-158">The name of the parameter to bind is stored in the **ModelBindingContext.ModelName** property.</span></span> <span data-ttu-id="11129-159">Die modellbindung sucht nach einem Schlüssel mit diesem Wert im Wörterbuch.</span><span class="sxs-lookup"><span data-stu-id="11129-159">The model binder looks for a key with this value in the dictionary.</span></span> <span data-ttu-id="11129-160">Wenn der Wert vorhanden ist, und kann, in konvertiert werden einem `GeoPoint`, die modellbindung weist den gebundenen Wert der **ModelBindingContext.Model** Eigenschaft.</span><span class="sxs-lookup"><span data-stu-id="11129-160">If the value exists and can be converted into a `GeoPoint`, the model binder assigns the bound value to the **ModelBindingContext.Model** property.</span></span>

<span data-ttu-id="11129-161">Beachten Sie, dass die modellbindung nicht auf eine einfache typkonvertierung beschränkt ist.</span><span class="sxs-lookup"><span data-stu-id="11129-161">Notice that the model binder is not limited to a simple type conversion.</span></span> <span data-ttu-id="11129-162">In diesem Beispiel die modellbindung sucht zuerst in eine Tabelle mit bekannten Speicherorten und wenn dies fehlschlägt, verwendet es typkonvertierung.</span><span class="sxs-lookup"><span data-stu-id="11129-162">In this example, the model binder first looks in a table of known locations, and if that fails, it uses type conversion.</span></span>

<span data-ttu-id="11129-163">**Festlegen der Modellbindung**</span><span class="sxs-lookup"><span data-stu-id="11129-163">**Setting the Model Binder**</span></span>

<span data-ttu-id="11129-164">Es gibt mehrere Möglichkeiten, einen Modellbinder festzulegen.</span><span class="sxs-lookup"><span data-stu-id="11129-164">There are several ways to set a model binder.</span></span> <span data-ttu-id="11129-165">Sie können zunächst Hinzufügen einer **[ModelBinder]** -Attribut auf den Parameter.</span><span class="sxs-lookup"><span data-stu-id="11129-165">First, you can add a **[ModelBinder]** attribute to the parameter.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample10.cs)]

<span data-ttu-id="11129-166">Sie können auch Hinzufügen einer **[ModelBinder]** -Attribut auf den Typ.</span><span class="sxs-lookup"><span data-stu-id="11129-166">You can also add a **[ModelBinder]** attribute to the type.</span></span> <span data-ttu-id="11129-167">Web-API wird den angegebene Modellbinder für alle Parameter dieses Typs verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="11129-167">Web API will use the specified model binder for all parameters of that type.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample11.cs)]

<span data-ttu-id="11129-168">Schließlich können Sie einen modellbinderanbieter zum Hinzufügen der **HttpConfiguration**.</span><span class="sxs-lookup"><span data-stu-id="11129-168">Finally, you can add a model-binder provider to the **HttpConfiguration**.</span></span> <span data-ttu-id="11129-169">Ein modellbinderanbieter ist einfach eine Factoryklasse, die eine modellbindung erstellt.</span><span class="sxs-lookup"><span data-stu-id="11129-169">A model-binder provider is simply a factory class that creates a model binder.</span></span> <span data-ttu-id="11129-170">Sie können einen Anbieter erstellen, durch Ableiten von der [ModelBinderProvider](https://msdn.microsoft.com/library/system.web.http.modelbinding.modelbinderprovider.aspx) Klasse.</span><span class="sxs-lookup"><span data-stu-id="11129-170">You can create a provider by deriving from the [ModelBinderProvider](https://msdn.microsoft.com/library/system.web.http.modelbinding.modelbinderprovider.aspx) class.</span></span> <span data-ttu-id="11129-171">Wenn Ihre modellbindung einen einzelnen Typ behandelt wird, ist es jedoch einfacher mithilfe die integrierten **SimpleModelBinderProvider**, die für diesen Zweck dient.</span><span class="sxs-lookup"><span data-stu-id="11129-171">However, if your model binder handles a single type, it's easier to use the built-in **SimpleModelBinderProvider**, which is designed for this purpose.</span></span> <span data-ttu-id="11129-172">Dies wird im folgenden Code veranschaulicht.</span><span class="sxs-lookup"><span data-stu-id="11129-172">The following code shows how to do this.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample12.cs)]

<span data-ttu-id="11129-173">Mit einem modellbindungs-Anbieter müssen dennoch hinzufügen der **[ModelBinder]** -Attribut auf den Parameter, um Web-API-mitzuteilen, dass eine modellbindung und nicht auf einem medientypformatierer verwendet werden soll.</span><span class="sxs-lookup"><span data-stu-id="11129-173">With a model-binding provider, you still need to add the **[ModelBinder]** attribute to the parameter, to tell Web API that it should use a model binder and not a media-type formatter.</span></span> <span data-ttu-id="11129-174">Jetzt müssen Sie jedoch nicht auf das Attribut den Typ der modellbindung angeben:</span><span class="sxs-lookup"><span data-stu-id="11129-174">But now you don't need to specify the type of model binder in the attribute:</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample13.cs)]

## <a name="value-providers"></a><span data-ttu-id="11129-175">Wertanbieter</span><span class="sxs-lookup"><span data-stu-id="11129-175">Value Providers</span></span>

<span data-ttu-id="11129-176">Ich habe erwähnt, dass eine modellbindung von einem Wertanbieter Ruft Werte ab.</span><span class="sxs-lookup"><span data-stu-id="11129-176">I mentioned that a model binder gets values from a value provider.</span></span> <span data-ttu-id="11129-177">Um einen benutzerdefinierten Wertanbieter zu schreiben, implementieren die **IValueProvider** Schnittstelle.</span><span class="sxs-lookup"><span data-stu-id="11129-177">To write a custom value provider, implement the **IValueProvider** interface.</span></span> <span data-ttu-id="11129-178">Hier ist ein Beispiel, das Werte aus den Cookies in der Anforderung abruft:</span><span class="sxs-lookup"><span data-stu-id="11129-178">Here is an example that pulls values from the cookies in the request:</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample14.cs)]

<span data-ttu-id="11129-179">Sie müssen auch eine Wertanbieter-Factory zu erstellen, durch Ableiten von der **ValueProviderFactory** Klasse.</span><span class="sxs-lookup"><span data-stu-id="11129-179">You also need to create a value provider factory by deriving from the **ValueProviderFactory** class.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample15.cs)]

<span data-ttu-id="11129-180">Hinzufügen der Wertanbieter-Factory, die **HttpConfiguration** wie folgt.</span><span class="sxs-lookup"><span data-stu-id="11129-180">Add the value provider factory to the **HttpConfiguration** as follows.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample16.cs)]

<span data-ttu-id="11129-181">Web-API erstellt alle von den Wertanbietern, also wenn ein Modellbinder aufruft **ValueProvider.GetValue**, die modellbindung empfängt den Wert aus der ersten Wertanbieter, die sie erzeugt werden kann.</span><span class="sxs-lookup"><span data-stu-id="11129-181">Web API composes all of the value providers, so when a model binder calls **ValueProvider.GetValue**, the model binder receives the value from the first value provider that is able to produce it.</span></span>

<span data-ttu-id="11129-182">Sie können alternativ die Wertanbieter-Factory auf der Ebene der Parameter festlegen, mit der **ValueProvider** Attribut wie folgt:</span><span class="sxs-lookup"><span data-stu-id="11129-182">Alternatively, you can set the value provider factory at the parameter level by using the **ValueProvider** attribute, as follows:</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample17.cs)]

<span data-ttu-id="11129-183">Dadurch wird die Web-API mit modellbindung mit dem angegebenen Wertanbieter-Factory und nicht auf den registrierten Wertanbietern verwendet.</span><span class="sxs-lookup"><span data-stu-id="11129-183">This tells Web API to use model binding with the specified value provider factory, and not to use any of the other registered value providers.</span></span>

## <a name="httpparameterbinding"></a><span data-ttu-id="11129-184">HttpParameterBinding</span><span class="sxs-lookup"><span data-stu-id="11129-184">HttpParameterBinding</span></span>

<span data-ttu-id="11129-185">Modellbinder entsprechen einer bestimmten Instanz eines allgemeineren Mechanismus.</span><span class="sxs-lookup"><span data-stu-id="11129-185">Model binders are a specific instance of a more general mechanism.</span></span> <span data-ttu-id="11129-186">Bei Betrachtung der **[ModelBinder]** -Attribut, sehen Sie, dass es von der abstrakten abgeleitetes **ParameterBindingAttribute** Klasse.</span><span class="sxs-lookup"><span data-stu-id="11129-186">If you look at the **[ModelBinder]** attribute, you will see that it derives from the abstract **ParameterBindingAttribute** class.</span></span> <span data-ttu-id="11129-187">Diese Klasse definiert eine einzelne Methode, **GetBinding**, gibt ein **HttpParameterBinding** Objekt:</span><span class="sxs-lookup"><span data-stu-id="11129-187">This class defines a single method, **GetBinding**, which returns an **HttpParameterBinding** object:</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample18.cs)]

<span data-ttu-id="11129-188">Ein **HttpParameterBinding** ist verantwortlich für die ein Parameter auf einen Wert gebunden.</span><span class="sxs-lookup"><span data-stu-id="11129-188">An **HttpParameterBinding** is responsible for binding a parameter to a value.</span></span> <span data-ttu-id="11129-189">Im Fall von **[ModelBinder]**, gibt das Attribut ein **HttpParameterBinding** -Implementierung, verwendet eine **IModelBinder** , führen Sie die tatsächliche Bindung.</span><span class="sxs-lookup"><span data-stu-id="11129-189">In the case of **[ModelBinder]**, the attribute returns an **HttpParameterBinding** implementation that uses an **IModelBinder** to perform the actual binding.</span></span> <span data-ttu-id="11129-190">Sie können auch implementieren Ihre eigenen **HttpParameterBinding**.</span><span class="sxs-lookup"><span data-stu-id="11129-190">You can also implement your own **HttpParameterBinding**.</span></span>

<span data-ttu-id="11129-191">Nehmen wir beispielsweise an, die Sie ETags aus abrufen möchten `if-match` und `if-none-match` Header in der Anforderung.</span><span class="sxs-lookup"><span data-stu-id="11129-191">For example, suppose you want to get ETags from `if-match` and `if-none-match` headers in the request.</span></span> <span data-ttu-id="11129-192">Wir beginnen mit dem Definieren einer Klasse zur Darstellung von ETags.</span><span class="sxs-lookup"><span data-stu-id="11129-192">We'll start by defining a class to represent ETags.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample19.cs)]

<span data-ttu-id="11129-193">Wir definieren auch eine Enumeration, um anzugeben, ob das ETag abgerufen werden. die `if-match` Header oder der `if-none-match` Header.</span><span class="sxs-lookup"><span data-stu-id="11129-193">We'll also define an enumeration to indicate whether to get the ETag from the `if-match` header or the `if-none-match` header.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample20.cs)]

<span data-ttu-id="11129-194">Hier ist ein **HttpParameterBinding** , ruft das ETag ab, aus dem gewünschten Header und bindet ihn an einen Parameter vom Typ ETag:</span><span class="sxs-lookup"><span data-stu-id="11129-194">Here is an **HttpParameterBinding** that gets the ETag from the desired header and binds it to a parameter of type ETag:</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample21.cs)]

<span data-ttu-id="11129-195">Die **ExecuteBindingAsync** Methode führt die Bindung.</span><span class="sxs-lookup"><span data-stu-id="11129-195">The **ExecuteBindingAsync** method does the binding.</span></span> <span data-ttu-id="11129-196">Fügen Sie innerhalb dieser Methode wird den gebundene Parameter-Wert, der **ActionArgument** Wörterbuch in der **HttpActionContext**.</span><span class="sxs-lookup"><span data-stu-id="11129-196">Within this method, add the bound parameter value to the **ActionArgument** dictionary in the **HttpActionContext**.</span></span>

> [!NOTE]
> <span data-ttu-id="11129-197">Wenn Ihre **ExecuteBindingAsync** Methode liest den Text der Anforderungsnachricht, überschreiben die **WillReadBody** -Eigenschaft "true" zurückgeben.</span><span class="sxs-lookup"><span data-stu-id="11129-197">If your **ExecuteBindingAsync** method reads the body of the request message, override the **WillReadBody** property to return true.</span></span> <span data-ttu-id="11129-198">Der Anforderungstext ist möglicherweise, dass ein nicht gepufferten Datenstream, der nur einmal gelesen werden kann, sodass Web-API einer Regel, dass höchstens eine Bindung setzt den Nachrichtentext lesen kann.</span><span class="sxs-lookup"><span data-stu-id="11129-198">The request body might be an unbuffered stream that can only be read once, so Web API enforces a rule that at most one binding can read the message body.</span></span>


<span data-ttu-id="11129-199">Anwenden ein benutzerdefinierten **HttpParameterBinding**, Sie können definieren, ein Attribut, das von abgeleitet ist **ParameterBindingAttribute**.</span><span class="sxs-lookup"><span data-stu-id="11129-199">To apply a custom **HttpParameterBinding**, you can define an attribute that derives from **ParameterBindingAttribute**.</span></span> <span data-ttu-id="11129-200">Für `ETagParameterBinding`, definieren wir zwei Attribute: eine für `if-match` Header und eine für `if-none-match` Header.</span><span class="sxs-lookup"><span data-stu-id="11129-200">For `ETagParameterBinding`, we'll define two attributes, one for `if-match` headers and one for `if-none-match` headers.</span></span> <span data-ttu-id="11129-201">Beide werden von einer abstrakten Klasse abgeleitet.</span><span class="sxs-lookup"><span data-stu-id="11129-201">Both derive from an abstract base class.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample22.cs)]

<span data-ttu-id="11129-202">Hier ist eine Controllermethode, verwendet der `[IfNoneMatch]` Attribut.</span><span class="sxs-lookup"><span data-stu-id="11129-202">Here is a controller method that uses the `[IfNoneMatch]` attribute.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample23.cs)]

<span data-ttu-id="11129-203">Darüber hinaus **ParameterBindingAttribute**, es wird eine andere Hook für das Hinzufügen eines benutzerdefiniertes **HttpParameterBinding**.</span><span class="sxs-lookup"><span data-stu-id="11129-203">Besides **ParameterBindingAttribute**, there is another hook for adding a custom **HttpParameterBinding**.</span></span> <span data-ttu-id="11129-204">Auf der **HttpConfiguration** -Objekt, das **ParameterBindingRules** Eigenschaft ist eine Sammlung von Anomymous Funktionen des Typs (**HttpParameterDescriptor**  - &gt; **HttpParameterBinding**).</span><span class="sxs-lookup"><span data-stu-id="11129-204">On the **HttpConfiguration** object, the **ParameterBindingRules** property is a collection of anomymous functions of type (**HttpParameterDescriptor** -&gt; **HttpParameterBinding**).</span></span> <span data-ttu-id="11129-205">Sie können beispielsweise eine Regel, ein ETag-Parameter für eine GET-Methode verwendet, hinzufügen `ETagParameterBinding` mit `if-none-match`:</span><span class="sxs-lookup"><span data-stu-id="11129-205">For example, you could add a rule that any ETag parameter on a GET method uses `ETagParameterBinding` with `if-none-match`:</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample24.cs)]

<span data-ttu-id="11129-206">Die Funktion zurückgeben soll `null` für Parameter, die die Bindung, in denen nicht anwendbar ist.</span><span class="sxs-lookup"><span data-stu-id="11129-206">The function should return `null` for parameters where the binding is not applicable.</span></span>

## <a name="iactionvaluebinder"></a><span data-ttu-id="11129-207">IActionValueBinder</span><span class="sxs-lookup"><span data-stu-id="11129-207">IActionValueBinder</span></span>

<span data-ttu-id="11129-208">Der gesamte Prozess für die bindenden Parameter wird gesteuert, von einem Dienst austauschbare **IActionValueBinder**.</span><span class="sxs-lookup"><span data-stu-id="11129-208">The entire parameter-binding process is controlled by a pluggable service, **IActionValueBinder**.</span></span> <span data-ttu-id="11129-209">Die standardmäßige Implementierung des **IActionValueBinder** bewirkt Folgendes:</span><span class="sxs-lookup"><span data-stu-id="11129-209">The default implementation of **IActionValueBinder** does the following:</span></span>

1. <span data-ttu-id="11129-210">Suchen Sie nach einer **ParameterBindingAttribute** für den Parameter.</span><span class="sxs-lookup"><span data-stu-id="11129-210">Look for a **ParameterBindingAttribute** on the parameter.</span></span> <span data-ttu-id="11129-211">Dies schließt **[FromBody]**, **[FromUri]**, und **[ModelBinder]**, oder benutzerdefinierte Attribute.</span><span class="sxs-lookup"><span data-stu-id="11129-211">This includes **[FromBody]**, **[FromUri]**, and **[ModelBinder]**, or custom attributes.</span></span>
2. <span data-ttu-id="11129-212">Suchen Sie andernfalls **HttpConfiguration.ParameterBindingRules** für eine Funktion, eine Wert ungleich Null zurückgibt **HttpParameterBinding**.</span><span class="sxs-lookup"><span data-stu-id="11129-212">Otherwise, look in **HttpConfiguration.ParameterBindingRules** for a function that returns a non-null **HttpParameterBinding**.</span></span>
3. <span data-ttu-id="11129-213">Verwenden Sie andernfalls die Standardregeln, die ich zuvor beschrieben.</span><span class="sxs-lookup"><span data-stu-id="11129-213">Otherwise, use the default rules that I described previously.</span></span> 

    - <span data-ttu-id="11129-214">Wenn der Parametertyp ist "einfach" oder einen Typkonverter, binden Sie aus dem URI.</span><span class="sxs-lookup"><span data-stu-id="11129-214">If the parameter type is "simple"or has a type converter, bind from the URI.</span></span> <span data-ttu-id="11129-215">Dies entspricht dem Platzieren der **[FromUri]** Attribut für den Parameter.</span><span class="sxs-lookup"><span data-stu-id="11129-215">This is equivalent to putting the **[FromUri]** attribute on the parameter.</span></span>
    - <span data-ttu-id="11129-216">Versuchen Sie andernfalls den Parameter aus dem Nachrichtentext zu lesen.</span><span class="sxs-lookup"><span data-stu-id="11129-216">Otherwise, try to read the parameter from the message body.</span></span> <span data-ttu-id="11129-217">Dies ist äquivalent zum Einfügen von **[FromBody]** für den Parameter.</span><span class="sxs-lookup"><span data-stu-id="11129-217">This is equivalent to putting **[FromBody]** on the parameter.</span></span>

<span data-ttu-id="11129-218">Falls gewünscht, können Sie die gesamte ersetzen **IActionValueBinder** mit einer benutzerdefinierten Implementierung.</span><span class="sxs-lookup"><span data-stu-id="11129-218">If you wanted, you could replace the entire **IActionValueBinder** service with a custom implementation.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="11129-219">Zusätzliche Ressourcen</span><span class="sxs-lookup"><span data-stu-id="11129-219">Additional Resources</span></span>

[<span data-ttu-id="11129-220">Beispiel einer benutzerdefinierten Parameter</span><span class="sxs-lookup"><span data-stu-id="11129-220">Custom Parameter Binding Sample</span></span>](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/CustomParameterBinding/ReadMe.txt)

<span data-ttu-id="11129-221">Mike Stall habe eine gute Reihe von Blogbeiträgen zur Bindung der Web-API-Parameter:</span><span class="sxs-lookup"><span data-stu-id="11129-221">Mike Stall wrote a good series of blog posts about Web API parameter binding:</span></span>

- [<span data-ttu-id="11129-222">Wie Web-API binden?</span><span class="sxs-lookup"><span data-stu-id="11129-222">How Web API does Parameter Binding</span></span>](https://blogs.msdn.com/b/jmstall/archive/2012/04/16/how-webapi-does-parameter-binding.aspx)
- [<span data-ttu-id="11129-223">MVC-Stil parameterbindung für Web-API</span><span class="sxs-lookup"><span data-stu-id="11129-223">MVC Style parameter binding for Web API</span></span>](https://blogs.msdn.com/b/jmstall/archive/2012/04/18/mvc-style-parameter-binding-for-webapi.aspx)
- [<span data-ttu-id="11129-224">Gewusst wie: Binden an benutzerdefinierte Objekte in aktionssignaturen in der MVC-Web-API</span><span class="sxs-lookup"><span data-stu-id="11129-224">How to bind to custom objects in action signatures in MVC/Web API</span></span>](https://blogs.msdn.com/b/jmstall/archive/2012/04/20/how-to-bind-to-custom-objects-in-action-signatures-in-mvc-webapi.aspx)
- [<span data-ttu-id="11129-225">Vorgehensweise: erstellen ein benutzerdefinierten Wertanbieters in Web-API</span><span class="sxs-lookup"><span data-stu-id="11129-225">How to create a custom value provider in Web API</span></span>](https://blogs.msdn.com/b/jmstall/archive/2012/04/23/how-to-create-a-custom-value-provider-in-webapi.aspx)
- [<span data-ttu-id="11129-226">Web-API-parameterbindung im Hintergrund</span><span class="sxs-lookup"><span data-stu-id="11129-226">Web API Parameter binding under the hood</span></span>](https://blogs.msdn.com/b/jmstall/archive/2012/05/11/webapi-parameter-binding-under-the-hood.aspx)
