---
title: "Formatieren von Antwortdaten in ASP.NET-MVC für Core"
author: ardalis
description: Informationen Sie zum Formatieren von Antwortdaten in ASP.NET-MVC-Core.
keywords: ASP.NET Core, Antwortdaten, IOutputFormatter, IActionResult
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: c056df45-d013-4826-91a1-4a092bae1ea5
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/models/formatting
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: abc125a093ff2cd5a38a537ecdfc795ff03e23f7
ms.sourcegitcommit: d1d8071d4093bf2444b5ae19d6e45c3d187e338b
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/19/2017
---
# <a name="introduction-to-formatting-response-data-in-aspnet-core-mvc"></a><span data-ttu-id="263e8-104">Einführung in die Antwort Formatierungsdaten in ASP.NET-MVC für Core</span><span class="sxs-lookup"><span data-stu-id="263e8-104">Introduction to formatting response data in ASP.NET Core MVC</span></span>

<span data-ttu-id="263e8-105">Durch [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="263e8-105">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="263e8-106">ASP.NET Core MVC verfügt über integrierte Unterstützung für die Formatierung von Antwortdaten, die mithilfe von fester Formaten oder als Antwort auf Client-Spezifikationen.</span><span class="sxs-lookup"><span data-stu-id="263e8-106">ASP.NET Core MVC has built-in support for formatting response data, using fixed formats or in response to client specifications.</span></span>

<span data-ttu-id="263e8-107">[Anzeigen oder Herunterladen des Beispiels aus GitHub](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/models/formatting/sample).</span><span class="sxs-lookup"><span data-stu-id="263e8-107">[View or download sample from GitHub](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/models/formatting/sample).</span></span>

## <a name="format-specific-action-results"></a><span data-ttu-id="263e8-108">Format-spezifische Aktionsergebnisse</span><span class="sxs-lookup"><span data-stu-id="263e8-108">Format-Specific Action Results</span></span>

<span data-ttu-id="263e8-109">Einige Aktionsergebnistypen sind spezifisch für ein bestimmtes Format, z. B. `JsonResult` und `ContentResult`.</span><span class="sxs-lookup"><span data-stu-id="263e8-109">Some action result types are specific to a particular format, such as `JsonResult` and `ContentResult`.</span></span> <span data-ttu-id="263e8-110">Aktionen können bestimmte Ergebnisse zurückgeben, die immer in einer bestimmten Weise formatiert werden.</span><span class="sxs-lookup"><span data-stu-id="263e8-110">Actions can return specific results that are always formatted in a particular manner.</span></span> <span data-ttu-id="263e8-111">Zurückgeben von z. B. eine `JsonResult` JSON-formatierte Daten, unabhängig von Clienteinstellungen zurück.</span><span class="sxs-lookup"><span data-stu-id="263e8-111">For example, returning a `JsonResult` will return JSON-formatted data, regardless of client preferences.</span></span> <span data-ttu-id="263e8-112">Ebenso Zurückgeben einer `ContentResult` Zeichenfolge im Format von nur-Text-Daten (wie eine Zeichenfolge zurückzugeben) zurück.</span><span class="sxs-lookup"><span data-stu-id="263e8-112">Likewise, returning a `ContentResult` will return plain-text-formatted string data (as will simply returning a string).</span></span>

> [!NOTE]
> <span data-ttu-id="263e8-113">Eine Aktion ist nicht erforderlich, um keinen bestimmten Typ zurückgeben; MVC unterstützt Objekt-Rückgabewert.</span><span class="sxs-lookup"><span data-stu-id="263e8-113">An action isn't required to return any particular type; MVC supports any object return value.</span></span> <span data-ttu-id="263e8-114">Wenn eine Aktion gibt eine `IActionResult` Implementierung und den Controller erbt von `Controller`, Entwickler haben viele Hilfsmethoden, die viele der Optionen für.</span><span class="sxs-lookup"><span data-stu-id="263e8-114">If an action returns an `IActionResult` implementation and the controller inherits from `Controller`, developers have many helper methods corresponding to many of the choices.</span></span> <span data-ttu-id="263e8-115">Ergebnisse von Aktionen, die Objekte zurückgeben, sind nicht `IActionResult` Typen serialisiert, die mit dem entsprechenden `IOutputFormatter` Implementierung.</span><span class="sxs-lookup"><span data-stu-id="263e8-115">Results from actions that return objects that are not `IActionResult` types will be serialized using the appropriate `IOutputFormatter` implementation.</span></span>

<span data-ttu-id="263e8-116">Um Daten in einem bestimmten Format von einem Controller zurückzugeben, die von erben die `Controller` Basisklasse, verwenden Sie die integrierte Hilfsmethode `Json` zurückzugebenden JSON und `Content` für nur-Text.</span><span class="sxs-lookup"><span data-stu-id="263e8-116">To return data in a specific format from a controller that inherits from the `Controller` base class, use the built-in helper method `Json` to return JSON and `Content` for plain text.</span></span> <span data-ttu-id="263e8-117">Die Aktionsmethode sollte die bestimmten Ergebnistyp zurückgeben (z. B. `JsonResult`) oder `IActionResult`.</span><span class="sxs-lookup"><span data-stu-id="263e8-117">Your action method should return either the specific result type (for instance, `JsonResult`) or `IActionResult`.</span></span>

<span data-ttu-id="263e8-118">Zurückgeben von JSON-formatierte Daten:</span><span class="sxs-lookup"><span data-stu-id="263e8-118">Returning JSON-formatted data:</span></span>

[!code-csharp[Main](./formatting/sample/Controllers/Api/AuthorsController.cs?highlight=3,5&range=21-26)]

<span data-ttu-id="263e8-119">Beispielantwort aus dieser Aktion:</span><span class="sxs-lookup"><span data-stu-id="263e8-119">Sample response from this action:</span></span>

![Registerkarte "Netzwerk" der Developer Tools in Microsoft Edge mit den Inhaltstyp der Antwort ist Application/json](formatting/_static/json-response.png)

<span data-ttu-id="263e8-121">Beachten Sie, dass der Inhaltstyp der Antwort `application/json`, die sowohl in der Liste der Anforderungen über das Netzwerk und im Antwortheader Abschnitt gezeigt.</span><span class="sxs-lookup"><span data-stu-id="263e8-121">Note that the content type of the response is `application/json`, shown both in the list of network requests and in the Response Headers section.</span></span> <span data-ttu-id="263e8-122">Beachten Sie außerdem die Liste der Optionen, die vom Browser im Accept-Header im Anforderungsheader Abschnitt (in diesem Fall Microsoft Edge) angezeigt.</span><span class="sxs-lookup"><span data-stu-id="263e8-122">Also note the list of options presented by the browser (in this case, Microsoft Edge) in the Accept header in the Request Headers section.</span></span> <span data-ttu-id="263e8-123">Das aktuelle Verfahren ist dieser Header wird ignoriert; Es obeying wird nachfolgend erläutert.</span><span class="sxs-lookup"><span data-stu-id="263e8-123">The current technique is ignoring this header; obeying it is discussed below.</span></span>

<span data-ttu-id="263e8-124">Nur-Text formatiert Daten zurückgeben möchten, verwenden `ContentResult` und `Content` Hilfsprogramm:</span><span class="sxs-lookup"><span data-stu-id="263e8-124">To return plain text formatted data, use `ContentResult` and the `Content` helper:</span></span>

[!code-csharp[Main](./formatting/sample/Controllers/Api/AuthorsController.cs?highlight=3,5&range=47-52)]

<span data-ttu-id="263e8-125">Eine Antwort von dieser Aktion:</span><span class="sxs-lookup"><span data-stu-id="263e8-125">A response from this action:</span></span>

![Registerkarte "Netzwerk" der Developer Tools in Microsoft Edge mit den Inhaltstyp der Antwort ist Text/plain](formatting/_static/text-response.png)

<span data-ttu-id="263e8-127">Beachten Sie in diesem Fall die `Content-Type` zurückgegebene `text/plain`.</span><span class="sxs-lookup"><span data-stu-id="263e8-127">Note in this case the `Content-Type` returned is `text/plain`.</span></span> <span data-ttu-id="263e8-128">Sie können dieses Verhalten mit nur einem Zeichenfolgentyp Antwort auch erreichen:</span><span class="sxs-lookup"><span data-stu-id="263e8-128">You can also achieve this same behavior using just a string response type:</span></span>

[!code-csharp[Main](./formatting/sample/Controllers/Api/AuthorsController.cs?highlight=3,5&range=54-59)]

>[!TIP]
> <span data-ttu-id="263e8-129">Für nicht triviale Aktionen mit mehreren Typen oder Optionen (z. B. verschiedene HTTP-Statuscodes, die abhängig vom Ergebnis der durchgeführten Operationen) zurückgeben, bevorzugen `IActionResult` als Rückgabetyp.</span><span class="sxs-lookup"><span data-stu-id="263e8-129">For non-trivial actions with multiple return types or options (for example, different HTTP status codes based on the result of operations performed), prefer `IActionResult` as the return type.</span></span>

## <a name="content-negotiation"></a><span data-ttu-id="263e8-130">Inhaltsaushandlung</span><span class="sxs-lookup"><span data-stu-id="263e8-130">Content Negotiation</span></span>

<span data-ttu-id="263e8-131">Inhalts-Aushandlung (*Conneg* kurz) tritt auf, wenn der Client gibt eine [Accept-Header](https://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html).</span><span class="sxs-lookup"><span data-stu-id="263e8-131">Content negotiation (*conneg* for short) occurs when the client specifies an [Accept header](https://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html).</span></span> <span data-ttu-id="263e8-132">Das Standardformat von Core ASP.NET-MVC verwendet ist JSON.</span><span class="sxs-lookup"><span data-stu-id="263e8-132">The default format used by ASP.NET Core MVC is JSON.</span></span> <span data-ttu-id="263e8-133">Inhaltsaushandlung wird dadurch implementiert, `ObjectResult`.</span><span class="sxs-lookup"><span data-stu-id="263e8-133">Content negotiation is implemented by `ObjectResult`.</span></span> <span data-ttu-id="263e8-134">Es ist auch der bestimmten Aktionsergebnisse aus den Hilfsmethoden zurückgegebene Statuscode integriert (die basieren alle auf `ObjectResult`).</span><span class="sxs-lookup"><span data-stu-id="263e8-134">It is also built into the status code specific action results returned from the helper methods (which are all based on `ObjectResult`).</span></span> <span data-ttu-id="263e8-135">Sie können auch einen Modelltyp (eine Klasse, die Sie, als der Datentyp für die Übertragung definiert haben) zurückgeben, und das Framework wird automatisch umgebrochen, ihn in ein `ObjectResult` für Sie.</span><span class="sxs-lookup"><span data-stu-id="263e8-135">You can also return a model type (a class you've defined as your data transfer type) and the framework will automatically wrap it in an `ObjectResult` for you.</span></span>

<span data-ttu-id="263e8-136">Die folgende Aktionsmethode verwendet die `Ok` und `NotFound` Hilfsmethoden:</span><span class="sxs-lookup"><span data-stu-id="263e8-136">The following action method uses the `Ok` and `NotFound` helper methods:</span></span>

[!code-csharp[Main](./formatting/sample/Controllers/Api/AuthorsController.cs?highlight=8,10&range=28-38)]

<span data-ttu-id="263e8-137">Eine JSON-formatierte Antwort wird zurückgegeben, es sei denn, ein anderes Format wurde angefordert, und der Server kann die angeforderte Format zurück.</span><span class="sxs-lookup"><span data-stu-id="263e8-137">A JSON-formatted response will be returned unless another format was requested and the server can return the requested format.</span></span> <span data-ttu-id="263e8-138">Sie können ein Tool wie [Fiddler](http://www.telerik.com/fiddler) erstellen eine Anforderung, die einen Accept-Header enthält, und geben Sie ein anderes Format.</span><span class="sxs-lookup"><span data-stu-id="263e8-138">You can use a tool like [Fiddler](http://www.telerik.com/fiddler) to create a request that includes an Accept header and specify another format.</span></span> <span data-ttu-id="263e8-139">In diesem Fall, wenn der Server hat eine *Formatierer* können erzeugt eine Antwort im gewünschten Format, das Ergebnis wird zurückgegeben werden im Format Client bevorzugt.</span><span class="sxs-lookup"><span data-stu-id="263e8-139">In that case, if the server has a *formatter* that can produce a response in the requested format, the result will be returned in the client-preferred format.</span></span>

![Fiddler-Konsole, die mit einem manuell erstellten GET-Anforderung mit einem Wert des Accept-Header Application/XML-](formatting/_static/fiddler-composer.png)

<span data-ttu-id="263e8-141">Im obigen Screenshot Fiddler Ersteller wurde verwendet, um eine Anforderung generieren angeben `Accept: application/xml`.</span><span class="sxs-lookup"><span data-stu-id="263e8-141">In the above screenshot, the Fiddler Composer has been used to generate a request, specifying `Accept: application/xml`.</span></span> <span data-ttu-id="263e8-142">Standardmäßig ASP.NET Core MVC unterstützt nur JSON, selbst wenn ein anderes Format angegeben wird, das zurückgegebene Ergebnis ist JSON-formatiert.</span><span class="sxs-lookup"><span data-stu-id="263e8-142">By default, ASP.NET Core MVC only supports JSON, so even when another format is specified, the result returned is still JSON-formatted.</span></span> <span data-ttu-id="263e8-143">Sie sehen, dass zusätzliche Formatierungsprogramme im nächsten Abschnitt hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="263e8-143">You'll see how to add additional formatters in the next section.</span></span>

<span data-ttu-id="263e8-144">Controlleraktionen können POCOs (Plain Old CLR Objects), zurück in diesem Fall ASP.NET MVC erstellt automatisch ein `ObjectResult` , der das Objekt umschließt.</span><span class="sxs-lookup"><span data-stu-id="263e8-144">Controller actions can return POCOs (Plain Old CLR Objects), in which case ASP.NET MVC will automatically create an `ObjectResult` for you that wraps the object.</span></span> <span data-ttu-id="263e8-145">Der Client erhält das formatierte serialisierte Objekt (JSON-Format ist die Standardeinstellung; Sie können XML-Index oder anderen Formaten konfigurieren).</span><span class="sxs-lookup"><span data-stu-id="263e8-145">The client will get the formatted serialized object (JSON format is the default; you can configure XML or other formats).</span></span> <span data-ttu-id="263e8-146">Wenn das Objekt, das zurückgegeben wird `null`, und klicken Sie dann das Framework zurückgegeben wird ein `204 No Content` Antwort.</span><span class="sxs-lookup"><span data-stu-id="263e8-146">If the object being returned is `null`, then the framework will return a `204 No Content` response.</span></span>

<span data-ttu-id="263e8-147">Zurückgeben eines Objekttyps:</span><span class="sxs-lookup"><span data-stu-id="263e8-147">Returning an object type:</span></span>

[!code-csharp[Main](./formatting/sample/Controllers/Api/AuthorsController.cs?highlight=3&range=40-45)]

<span data-ttu-id="263e8-148">In diesem Beispiel erhält eine Anforderung für eine gültige Autor-Alias eine 200 OK-Antwort mit der Autor-Daten.</span><span class="sxs-lookup"><span data-stu-id="263e8-148">In the sample, a request for a valid author alias will receive a 200 OK response with the author's data.</span></span> <span data-ttu-id="263e8-149">Eine Anforderung für einen ungültigen Alias erhalten eine 204 No Content-Antwort.</span><span class="sxs-lookup"><span data-stu-id="263e8-149">A request for an invalid alias will receive a 204 No Content response.</span></span> <span data-ttu-id="263e8-150">Screenshots, die mit der Antwort in XML und JSON-Formate werden unten gezeigt.</span><span class="sxs-lookup"><span data-stu-id="263e8-150">Screenshots showing the response in XML and JSON formats are shown below.</span></span>

### <a name="content-negotiation-process"></a><span data-ttu-id="263e8-151">Inhaltsaushandlung-Prozess</span><span class="sxs-lookup"><span data-stu-id="263e8-151">Content Negotiation Process</span></span>

<span data-ttu-id="263e8-152">Inhalt *Aushandlung* nur erfolgt, wenn ein `Accept` Header angezeigt wird, in der Anforderung.</span><span class="sxs-lookup"><span data-stu-id="263e8-152">Content *negotiation* only takes place if an `Accept` header appears in the request.</span></span> <span data-ttu-id="263e8-153">Wenn eine Anforderung einen Accept-Header enthält, wird das Framework Listet die Medientypen im Accept-Header in der Rangfolge und versucht, einen Formatierer finden, der eine Antwort in einem der Formate, die durch den Accept-Header angegebenen erstellen kann.</span><span class="sxs-lookup"><span data-stu-id="263e8-153">When a request contains an accept header, the framework will enumerate the media types in the accept header in preference order and will try to find a formatter that can produce a response in one of the formats specified by the accept header.</span></span> <span data-ttu-id="263e8-154">Für den Fall, dass kein Formatierer gefunden wird, kann die Anforderung des Clients erfüllen, wird das Framework versucht, den ersten Formatierer gefunden, die eine Antwort erstellen kann (es sei denn, der Entwickler die Option für konfiguriert hat `MvcOptions` zurückzugebenden 406 nicht akzeptabel stattdessen).</span><span class="sxs-lookup"><span data-stu-id="263e8-154">In case no formatter is found that can satisfy the client's request, the framework will try to find the first formatter that can produce a response (unless the developer has configured the option on `MvcOptions` to return 406 Not Acceptable instead).</span></span> <span data-ttu-id="263e8-155">Wenn die Anforderung XML-Datei gibt, aber der XML-Formatierer nicht konfiguriert wurde, wird der JSON-Formatierer verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="263e8-155">If the request specifies XML, but the XML formatter has not been configured, then the JSON formatter will be used.</span></span> <span data-ttu-id="263e8-156">Üblicher, wenn kein Formatierer konfiguriert ist, der das angeforderte Format bereitstellen kann, wird der erste Formatierer, der das Objekt nicht formatieren kann verwendet.</span><span class="sxs-lookup"><span data-stu-id="263e8-156">More generally, if no formatter is configured that can provide the requested format, then the first formatter that can format the object is used.</span></span> <span data-ttu-id="263e8-157">Keine Header angegeben ist, wird der erste Formatierer, der das Objekt, das zurückgegeben werden bewältigen können zum Serialisieren der Antwort verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="263e8-157">If no header is given, the first formatter that can handle the object to be returned will be used to serialize the response.</span></span> <span data-ttu-id="263e8-158">In diesem Fall gibt es keine stattgefunden hat Aushandlung - Server ist die Festlegung, in welchem Format verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="263e8-158">In this case, there isn't any negotiation taking place - the server is determining what format it will use.</span></span>

> [!NOTE]
> <span data-ttu-id="263e8-159">Wenn die Accept-Header enthält `*/*`, der Header wird ignoriert, es sei denn, `RespectBrowserAcceptHeader` auf festgelegt ist auf "true" `MvcOptions`.</span><span class="sxs-lookup"><span data-stu-id="263e8-159">If the Accept header contains `*/*`, the Header will be ignored unless `RespectBrowserAcceptHeader` is set to true on `MvcOptions`.</span></span>

### <a name="browsers-and-content-negotiation"></a><span data-ttu-id="263e8-160">Browser und Inhaltsaushandlung</span><span class="sxs-lookup"><span data-stu-id="263e8-160">Browsers and Content Negotiation</span></span>

<span data-ttu-id="263e8-161">Im Gegensatz zu typischen-API-Clients Webbrowsern angeben tendenziell `Accept` Header, die eine Vielzahl von Formaten, einschließlich Platzhalter enthalten.</span><span class="sxs-lookup"><span data-stu-id="263e8-161">Unlike typical API clients, web browsers tend to supply `Accept` headers that include a wide array of formats, including wildcards.</span></span> <span data-ttu-id="263e8-162">Standardmäßig, wenn das Framework erkennt, dass die Anforderung von einem Browser eingeben, stammt ignoriert er die `Accept` Header und stattdessen return der Inhalt in der Anwendung konfigurierten Standardformat (JSON Wenn nicht anders konfiguriert).</span><span class="sxs-lookup"><span data-stu-id="263e8-162">By default, when the framework detects that the request is coming from a browser, it will ignore the `Accept` header and instead return the content in the application's configured default format (JSON unless otherwise configured).</span></span> <span data-ttu-id="263e8-163">Dies bietet eine konsistente Umgebung Verwendung von verschiedenen Browsern APIs nutzen.</span><span class="sxs-lookup"><span data-stu-id="263e8-163">This provides a more consistent experience when using different browsers to consume APIs.</span></span>

<span data-ttu-id="263e8-164">Wenn Sie Ihre Anwendung akzeptieren Browser accept-Header möchten, können Sie diese als Teil der Konfiguration des MVC-Konfiguration, durch Festlegen von `RespectBrowserAcceptHeader` auf `true` in der `ConfigureServices` Methode in *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="263e8-164">If you would prefer your application honor browser accept headers, you can configure this as part of MVC's configuration by setting `RespectBrowserAcceptHeader` to `true` in the `ConfigureServices` method in *Startup.cs*.</span></span>

```csharp
services.AddMvc(options =>
{
    options.RespectBrowserAcceptHeader = true; // false by default
});
```

## <a name="configuring-formatters"></a><span data-ttu-id="263e8-165">Konfigurieren der Formatierer</span><span class="sxs-lookup"><span data-stu-id="263e8-165">Configuring Formatters</span></span>

<span data-ttu-id="263e8-166">Wenn Ihre Anwendung zusätzliche Formate jenseits der Standardgrenze von JSON unterstützen muss, können Sie NuGet-Pakete hinzufügen und Konfigurieren von MVC, um diese zu unterstützen.</span><span class="sxs-lookup"><span data-stu-id="263e8-166">If your application needs to support additional formats beyond the default of JSON, you can add NuGet packages and configure MVC to support them.</span></span> <span data-ttu-id="263e8-167">Es gibt separate Formatierungsprogramme für ein- und Ausgaben.</span><span class="sxs-lookup"><span data-stu-id="263e8-167">There are separate formatters for input and output.</span></span> <span data-ttu-id="263e8-168">Eingabe Formatierungsprogramme werden von verwendet [Modell Bindung](model-binding.md); Ausgabe Formatierungsprogramme werden verwendet, um Antworten zu formatieren.</span><span class="sxs-lookup"><span data-stu-id="263e8-168">Input formatters are used by [Model Binding](model-binding.md); output formatters are used to format responses.</span></span> <span data-ttu-id="263e8-169">Sie können auch konfigurieren, [benutzerdefinierten Formatierer](../advanced/custom-formatters.md).</span><span class="sxs-lookup"><span data-stu-id="263e8-169">You can also configure [Custom Formatters](../advanced/custom-formatters.md).</span></span>

### <a name="adding-xml-format-support"></a><span data-ttu-id="263e8-170">Hinzufügen von Unterstützung für XML-Format</span><span class="sxs-lookup"><span data-stu-id="263e8-170">Adding XML Format Support</span></span>

<span data-ttu-id="263e8-171">Installieren Sie zum Hinzufügen der Unterstützung für XML-Formatierung der `Microsoft.AspNetCore.Mvc.Formatters.Xml` NuGet-Paket.</span><span class="sxs-lookup"><span data-stu-id="263e8-171">To add support for XML formatting, install the `Microsoft.AspNetCore.Mvc.Formatters.Xml` NuGet package.</span></span>

<span data-ttu-id="263e8-172">Fügen Sie die XmlSerializerFormatters MVCs-Konfiguration in *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="263e8-172">Add the XmlSerializerFormatters to MVC's configuration in *Startup.cs*:</span></span>

[!code-csharp[Main](./formatting/sample/Startup.cs?name=snippet1&highlight=2)]

<span data-ttu-id="263e8-173">Alternativ können Sie einfach das Ausgabe-Formatierungsprogramm hinzufügen:</span><span class="sxs-lookup"><span data-stu-id="263e8-173">Alternately, you can add just the output formatter:</span></span>

```csharp
services.AddMvc(options =>
{
    options.OutputFormatters.Add(new XmlSerializerOutputFormatter());
});
```

<span data-ttu-id="263e8-174">Diese beiden Ansätze werden Serialisierung von Ergebnissen mit `System.Xml.Serialization.XmlSerializer`.</span><span class="sxs-lookup"><span data-stu-id="263e8-174">These two approaches will serialize results using `System.Xml.Serialization.XmlSerializer`.</span></span> <span data-ttu-id="263e8-175">Falls gewünscht, können Sie die `System.Runtime.Serialization.DataContractSerializer` durch seine zugeordneten Formatierer hinzufügen:</span><span class="sxs-lookup"><span data-stu-id="263e8-175">If you prefer, you can use the `System.Runtime.Serialization.DataContractSerializer` by adding its associated formatter:</span></span>

```csharp
services.AddMvc(options =>
{
    options.OutputFormatters.Add(new XmlDataContractSerializerOutputFormatter());
});
```

<span data-ttu-id="263e8-176">Nachdem Sie die Unterstützung für XML-Formatierung hinzugefügt haben, sollte Ihre Controllermethoden das entsprechende Format basierend auf der Anforderung zurückgeben `Accept` -Header, als diese Fiddler veranschaulicht:</span><span class="sxs-lookup"><span data-stu-id="263e8-176">Once you've added support for XML formatting, your controller methods should return the appropriate format based on the request's `Accept` header, as this Fiddler example demonstrates:</span></span>

![Fiddler-Konsole: das unformatierte Registerkarte für die Anforderung wird der Wert des Accept-Header ist Application/Xml.](formatting/_static/xml-response.png)

<span data-ttu-id="263e8-179">Sehen Sie in der Registerkarte "Inspektoren", die die Rohdaten GET-Anforderung erstellt wurde, mit einer `Accept: application/xml` Header festlegen.</span><span class="sxs-lookup"><span data-stu-id="263e8-179">You can see in the Inspectors tab that the Raw GET request was made with an `Accept: application/xml` header set.</span></span> <span data-ttu-id="263e8-180">Die Antwort Bereich zeigt die `Content-Type: application/xml` -Header, und die `Author` Objekt in XML serialisiert wurde.</span><span class="sxs-lookup"><span data-stu-id="263e8-180">The response pane shows the `Content-Type: application/xml` header, and the `Author` object has been serialized to XML.</span></span>

<span data-ttu-id="263e8-181">Verwenden Sie die Registerkarte "Composer" So ändern Sie die Anforderung an `application/json` in der `Accept` Header.</span><span class="sxs-lookup"><span data-stu-id="263e8-181">Use the Composer tab to modify the request to specify `application/json` in the `Accept` header.</span></span> <span data-ttu-id="263e8-182">Führen Sie die Anforderung und die Antwort als JSON formatiert:</span><span class="sxs-lookup"><span data-stu-id="263e8-182">Execute the request, and the response will be formatted as JSON:</span></span>

![Fiddler-Konsole: das unformatierte Registerkarte für die Anforderung wird der Wert des Accept-Header ist Application/Json.](formatting/_static/json-response-fiddler.png)

<span data-ttu-id="263e8-185">In dieser Abbildung sehen Sie die Anforderung legt Header `Accept: application/json` und die Antwort gibt an, die identisch mit der `Content-Type`.</span><span class="sxs-lookup"><span data-stu-id="263e8-185">In this screenshot, you can see the request sets a header of `Accept: application/json` and the response specifies the same as its `Content-Type`.</span></span> <span data-ttu-id="263e8-186">Die `Author` Objekt im Text der Antwort im JSON-Format dargestellt wird.</span><span class="sxs-lookup"><span data-stu-id="263e8-186">The `Author` object is shown in the body of the response, in JSON format.</span></span>

### <a name="forcing-a-particular-format"></a><span data-ttu-id="263e8-187">Erzwingen ein bestimmtes Format</span><span class="sxs-lookup"><span data-stu-id="263e8-187">Forcing a Particular Format</span></span>

<span data-ttu-id="263e8-188">Wenn Sie die Antwortformate für eine bestimmte Aktion einschränken möchten, können Sie, können Sie anwenden der `[Produces]` Filter.</span><span class="sxs-lookup"><span data-stu-id="263e8-188">If you would like to restrict the response formats for a specific action you can, you can apply the `[Produces]` filter.</span></span> <span data-ttu-id="263e8-189">Die `[Produces]` Filter gibt die Antwortformate für eine bestimmte Aktion (oder Domänencontroller).</span><span class="sxs-lookup"><span data-stu-id="263e8-189">The `[Produces]` filter specifies the response formats for a specific action (or controller).</span></span> <span data-ttu-id="263e8-190">Wie die meisten [Filter](../controllers/filters.md), dies kann auf die Aktion, Controller, oder im globalen Gültigkeitsbereich angewendet werden.</span><span class="sxs-lookup"><span data-stu-id="263e8-190">Like most [Filters](../controllers/filters.md), this can be applied at the action, controller, or global scope.</span></span>

```csharp
[Produces("application/json")]
public class AuthorsController
```

<span data-ttu-id="263e8-191">Die `[Produces]` Filter wird erzwingen, dass alle Aktionen innerhalb der `AuthorsController` JSON-formatierte Antworten zurückgeben, selbst wenn andere Formatierungsprogramme für die Anwendung und vom Client bereitgestellten konfiguriert wurden ein `Accept` Anfordern von einem anderen Header verfügbar das Format.</span><span class="sxs-lookup"><span data-stu-id="263e8-191">The `[Produces]` filter will force all actions within the `AuthorsController` to return JSON-formatted responses, even if other formatters were configured for the application and the client provided an `Accept` header requesting a different, available format.</span></span> <span data-ttu-id="263e8-192">Finden Sie unter [Filter](../controllers/filters.md) erfahren Sie mehr, einschließlich Informationen zum Anwenden von Filtern Global.</span><span class="sxs-lookup"><span data-stu-id="263e8-192">See [Filters](../controllers/filters.md) to learn more, including how to apply filters globally.</span></span>

### <a name="special-case-formatters"></a><span data-ttu-id="263e8-193">Spezielle Formatierungsprogramme für die Groß-/Kleinschreibung</span><span class="sxs-lookup"><span data-stu-id="263e8-193">Special Case Formatters</span></span>

<span data-ttu-id="263e8-194">Einige Sonderfälle werden mithilfe der integrierten Formatierer implementiert.</span><span class="sxs-lookup"><span data-stu-id="263e8-194">Some special cases are implemented using built-in formatters.</span></span> <span data-ttu-id="263e8-195">Standardmäßig `string` Rückgabetypen formatiert als *Text/Plain* (*Text/html* ggf. über `Accept` Header).</span><span class="sxs-lookup"><span data-stu-id="263e8-195">By default, `string` return types will be formatted as *text/plain* (*text/html* if requested via `Accept` header).</span></span> <span data-ttu-id="263e8-196">Dieses Verhalten kann entfernt werden, durch das Entfernen der `TextOutputFormatter`.</span><span class="sxs-lookup"><span data-stu-id="263e8-196">This behavior can be removed by removing the `TextOutputFormatter`.</span></span> <span data-ttu-id="263e8-197">Entfernen Sie Formatierer in die `Configure` Methode im *Startup.cs* (siehe unten).</span><span class="sxs-lookup"><span data-stu-id="263e8-197">You remove formatters in the `Configure` method in *Startup.cs* (shown below).</span></span> <span data-ttu-id="263e8-198">Aktionen, die ein Modellobjekt haben zurückgeben wird Rückgabetyp 204 ohne Inhalt Antwort beim Zurückgeben der `null`.</span><span class="sxs-lookup"><span data-stu-id="263e8-198">Actions that have a model object return type will return a 204 No Content response when returning `null`.</span></span> <span data-ttu-id="263e8-199">Dieses Verhalten kann entfernt werden, durch das Entfernen der `HttpNoContentOutputFormatter`.</span><span class="sxs-lookup"><span data-stu-id="263e8-199">This behavior can be removed by removing the `HttpNoContentOutputFormatter`.</span></span> <span data-ttu-id="263e8-200">Das folgende Codebeispiel entfernt die `TextOutputFormatter` und `HttpNoContentOutputFormatter`.</span><span class="sxs-lookup"><span data-stu-id="263e8-200">The following code removes the `TextOutputFormatter` and `HttpNoContentOutputFormatter`.</span></span>

```csharp
services.AddMvc(options =>
{
    options.OutputFormatters.RemoveType<TextOutputFormatter>();
    options.OutputFormatters.RemoveType<HttpNoContentOutputFormatter>();
});
```

<span data-ttu-id="263e8-201">Ohne die `TextOutputFormatter`, `string` Typen zurückgeben 406 nicht akzeptabel ist, z. B.</span><span class="sxs-lookup"><span data-stu-id="263e8-201">Without the `TextOutputFormatter`, `string` return types return 406 Not Acceptable, for example.</span></span> <span data-ttu-id="263e8-202">Beachten Sie, dass wenn ein XML-Formatierer vorhanden ist, wird es formatieren `string` Typen zurückgeben, wenn die `TextOutputFormatter` wird entfernt.</span><span class="sxs-lookup"><span data-stu-id="263e8-202">Note that if an XML formatter exists, it will format `string` return types if the `TextOutputFormatter` is removed.</span></span>

<span data-ttu-id="263e8-203">Ohne die `HttpNoContentOutputFormatter`, null-Objekte mithilfe des konfigurierten Formatierers formatiert sind.</span><span class="sxs-lookup"><span data-stu-id="263e8-203">Without the `HttpNoContentOutputFormatter`, null objects are formatted using the configured formatter.</span></span> <span data-ttu-id="263e8-204">Beispielsweise wird das JSON-Formatierungsprogramm einfach eine Antwort mit einem Text des zurückgeben `null`, während der XML-Formatierer ein leeres XML-Element mit dem Attribut zurückliefert `xsi:nil="true"` festgelegt.</span><span class="sxs-lookup"><span data-stu-id="263e8-204">For example, the JSON formatter will simply return a response with a body of `null`, while the XML formatter will return an empty XML element with the attribute `xsi:nil="true"` set.</span></span>

## <a name="response-format-url-mappings"></a><span data-ttu-id="263e8-205">Antwort Format URL Zuordnungen</span><span class="sxs-lookup"><span data-stu-id="263e8-205">Response Format URL Mappings</span></span>

<span data-ttu-id="263e8-206">Clients können Sie ein bestimmtes Format als Teil der URL, z. B. in der Abfragezeichenfolge oder einen Teil des Pfads oder mithilfe von Format-spezifische Erweiterung z. B. XML- oder JSON anfordern.</span><span class="sxs-lookup"><span data-stu-id="263e8-206">Clients can request a particular format as part of the URL, such as in the query string or part of the path, or by using a format-specific file extension such as .xml or .json.</span></span> <span data-ttu-id="263e8-207">In der Route an, die die API verwendet, sollte die Zuordnung von Anforderungspfad angegeben werden.</span><span class="sxs-lookup"><span data-stu-id="263e8-207">The mapping from request path should be specified in the route the API is using.</span></span> <span data-ttu-id="263e8-208">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="263e8-208">For example:</span></span>

```csharp
[FormatFilter]
public class ProductsController
{
    [Route("[controller]/[action]/{id}.{format?}")]
    public Product GetById(int id)
```

<span data-ttu-id="263e8-209">Diese Route entschied sich auf das angeforderte Format als Erweiterung optional angegeben werden.</span><span class="sxs-lookup"><span data-stu-id="263e8-209">This route would allow the requested format to be specified as an optional file extension.</span></span> <span data-ttu-id="263e8-210">Die `[FormatFilter]` Attribut überprüft das Vorhandensein des Werts Format in die `RouteData` und das Antwortformat wird an den entsprechenden Formatierer zugeordnet werden, wenn die Antwort erstellt wird.</span><span class="sxs-lookup"><span data-stu-id="263e8-210">The `[FormatFilter]` attribute checks for the existence of the format value in the `RouteData` and will map the response format to the appropriate formatter when the response is created.</span></span>

| <span data-ttu-id="263e8-211">Route</span><span class="sxs-lookup"><span data-stu-id="263e8-211">Route</span></span>                      | <span data-ttu-id="263e8-212">Formatierer</span><span class="sxs-lookup"><span data-stu-id="263e8-212">Formatter</span></span>                          |
| -------------------------- | ---------------------------------- |
| `/products/GetById/5`      | <span data-ttu-id="263e8-213">Der Standardformatierer-Ausgabe</span><span class="sxs-lookup"><span data-stu-id="263e8-213">The default output formatter</span></span>       |
| `/products/GetById/5.json` | <span data-ttu-id="263e8-214">Das JSON-Formatierungsprogramm (sofern konfiguriert)</span><span class="sxs-lookup"><span data-stu-id="263e8-214">The JSON formatter (if configured)</span></span> |
| `/products/GetById/5.xml`  | <span data-ttu-id="263e8-215">Der XML-Formatierer (sofern konfiguriert)</span><span class="sxs-lookup"><span data-stu-id="263e8-215">The XML formatter (if configured)</span></span>  |
