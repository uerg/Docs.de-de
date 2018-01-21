---
title: "Formatieren von Antwortdaten in ASP.NET-MVC für Core"
author: ardalis
description: Informationen Sie zum Formatieren von Antwortdaten in ASP.NET-MVC-Core.
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/models/formatting
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 85398928164e75ec27c91870f03ee1c81725e080
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/19/2018
---
# <a name="introduction-to-formatting-response-data-in-aspnet-core-mvc"></a><span data-ttu-id="8feae-103">Einführung in die Antwort Formatierungsdaten in ASP.NET-MVC für Core</span><span class="sxs-lookup"><span data-stu-id="8feae-103">Introduction to formatting response data in ASP.NET Core MVC</span></span>

<span data-ttu-id="8feae-104">Durch [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="8feae-104">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="8feae-105">ASP.NET Core MVC verfügt über integrierte Unterstützung für die Formatierung von Antwortdaten, die mithilfe von fester Formaten oder als Antwort auf Client-Spezifikationen.</span><span class="sxs-lookup"><span data-stu-id="8feae-105">ASP.NET Core MVC has built-in support for formatting response data, using fixed formats or in response to client specifications.</span></span>

<span data-ttu-id="8feae-106">[Anzeigen oder Herunterladen des Beispiels aus GitHub](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/models/formatting/sample).</span><span class="sxs-lookup"><span data-stu-id="8feae-106">[View or download sample from GitHub](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/models/formatting/sample).</span></span>

## <a name="format-specific-action-results"></a><span data-ttu-id="8feae-107">Format-spezifische Aktionsergebnisse</span><span class="sxs-lookup"><span data-stu-id="8feae-107">Format-Specific Action Results</span></span>

<span data-ttu-id="8feae-108">Einige Aktionsergebnistypen sind spezifisch für ein bestimmtes Format, z. B. `JsonResult` und `ContentResult`.</span><span class="sxs-lookup"><span data-stu-id="8feae-108">Some action result types are specific to a particular format, such as `JsonResult` and `ContentResult`.</span></span> <span data-ttu-id="8feae-109">Aktionen können bestimmte Ergebnisse zurückgeben, die immer in einer bestimmten Weise formatiert werden.</span><span class="sxs-lookup"><span data-stu-id="8feae-109">Actions can return specific results that are always formatted in a particular manner.</span></span> <span data-ttu-id="8feae-110">Zurückgeben von z. B. eine `JsonResult` JSON-formatierte Daten, unabhängig von Clienteinstellungen zurück.</span><span class="sxs-lookup"><span data-stu-id="8feae-110">For example, returning a `JsonResult` will return JSON-formatted data, regardless of client preferences.</span></span> <span data-ttu-id="8feae-111">Ebenso Zurückgeben einer `ContentResult` Zeichenfolge im Format von nur-Text-Daten (wie eine Zeichenfolge zurückzugeben) zurück.</span><span class="sxs-lookup"><span data-stu-id="8feae-111">Likewise, returning a `ContentResult` will return plain-text-formatted string data (as will simply returning a string).</span></span>

> [!NOTE]
> <span data-ttu-id="8feae-112">Eine Aktion ist nicht erforderlich, um keinen bestimmten Typ zurückgeben; MVC unterstützt Objekt-Rückgabewert.</span><span class="sxs-lookup"><span data-stu-id="8feae-112">An action isn't required to return any particular type; MVC supports any object return value.</span></span> <span data-ttu-id="8feae-113">Wenn eine Aktion gibt eine `IActionResult` Implementierung und den Controller erbt von `Controller`, Entwickler haben viele Hilfsmethoden, die viele der Optionen für.</span><span class="sxs-lookup"><span data-stu-id="8feae-113">If an action returns an `IActionResult` implementation and the controller inherits from `Controller`, developers have many helper methods corresponding to many of the choices.</span></span> <span data-ttu-id="8feae-114">Ergebnisse von Aktionen, die Objekte zurückgeben, sind nicht `IActionResult` Typen serialisiert, die mit dem entsprechenden `IOutputFormatter` Implementierung.</span><span class="sxs-lookup"><span data-stu-id="8feae-114">Results from actions that return objects that are not `IActionResult` types will be serialized using the appropriate `IOutputFormatter` implementation.</span></span>

<span data-ttu-id="8feae-115">Um Daten in einem bestimmten Format von einem Controller zurückzugeben, die von erben die `Controller` Basisklasse, verwenden Sie die integrierte Hilfsmethode `Json` zurückzugebenden JSON und `Content` für nur-Text.</span><span class="sxs-lookup"><span data-stu-id="8feae-115">To return data in a specific format from a controller that inherits from the `Controller` base class, use the built-in helper method `Json` to return JSON and `Content` for plain text.</span></span> <span data-ttu-id="8feae-116">Die Aktionsmethode sollte die bestimmten Ergebnistyp zurückgeben (z. B. `JsonResult`) oder `IActionResult`.</span><span class="sxs-lookup"><span data-stu-id="8feae-116">Your action method should return either the specific result type (for instance, `JsonResult`) or `IActionResult`.</span></span>

<span data-ttu-id="8feae-117">Zurückgeben von JSON-formatierte Daten:</span><span class="sxs-lookup"><span data-stu-id="8feae-117">Returning JSON-formatted data:</span></span>

[!code-csharp[Main](./formatting/sample/Controllers/Api/AuthorsController.cs?highlight=3,5&range=21-26)]

<span data-ttu-id="8feae-118">Beispielantwort aus dieser Aktion:</span><span class="sxs-lookup"><span data-stu-id="8feae-118">Sample response from this action:</span></span>

![Registerkarte "Netzwerk" der Developer Tools in Microsoft Edge mit den Inhaltstyp der Antwort ist Application/json](formatting/_static/json-response.png)

<span data-ttu-id="8feae-120">Beachten Sie, dass der Inhaltstyp der Antwort `application/json`, die sowohl in der Liste der Anforderungen über das Netzwerk und im Antwortheader Abschnitt gezeigt.</span><span class="sxs-lookup"><span data-stu-id="8feae-120">Note that the content type of the response is `application/json`, shown both in the list of network requests and in the Response Headers section.</span></span> <span data-ttu-id="8feae-121">Beachten Sie außerdem die Liste der Optionen, die vom Browser im Accept-Header im Anforderungsheader Abschnitt (in diesem Fall Microsoft Edge) angezeigt.</span><span class="sxs-lookup"><span data-stu-id="8feae-121">Also note the list of options presented by the browser (in this case, Microsoft Edge) in the Accept header in the Request Headers section.</span></span> <span data-ttu-id="8feae-122">Das aktuelle Verfahren ist dieser Header wird ignoriert; Es obeying wird nachfolgend erläutert.</span><span class="sxs-lookup"><span data-stu-id="8feae-122">The current technique is ignoring this header; obeying it is discussed below.</span></span>

<span data-ttu-id="8feae-123">Nur-Text formatiert Daten zurückgeben möchten, verwenden `ContentResult` und `Content` Hilfsprogramm:</span><span class="sxs-lookup"><span data-stu-id="8feae-123">To return plain text formatted data, use `ContentResult` and the `Content` helper:</span></span>

[!code-csharp[Main](./formatting/sample/Controllers/Api/AuthorsController.cs?highlight=3,5&range=47-52)]

<span data-ttu-id="8feae-124">Eine Antwort von dieser Aktion:</span><span class="sxs-lookup"><span data-stu-id="8feae-124">A response from this action:</span></span>

![Registerkarte "Netzwerk" der Developer Tools in Microsoft Edge mit den Inhaltstyp der Antwort ist Text/plain](formatting/_static/text-response.png)

<span data-ttu-id="8feae-126">Beachten Sie in diesem Fall die `Content-Type` zurückgegebene `text/plain`.</span><span class="sxs-lookup"><span data-stu-id="8feae-126">Note in this case the `Content-Type` returned is `text/plain`.</span></span> <span data-ttu-id="8feae-127">Sie können dieses Verhalten mit nur einem Zeichenfolgentyp Antwort auch erreichen:</span><span class="sxs-lookup"><span data-stu-id="8feae-127">You can also achieve this same behavior using just a string response type:</span></span>

[!code-csharp[Main](./formatting/sample/Controllers/Api/AuthorsController.cs?highlight=3,5&range=54-59)]

>[!TIP]
> <span data-ttu-id="8feae-128">Für nicht triviale Aktionen mit mehreren Typen oder Optionen (z. B. verschiedene HTTP-Statuscodes, die abhängig vom Ergebnis der durchgeführten Operationen) zurückgeben, bevorzugen `IActionResult` als Rückgabetyp.</span><span class="sxs-lookup"><span data-stu-id="8feae-128">For non-trivial actions with multiple return types or options (for example, different HTTP status codes based on the result of operations performed), prefer `IActionResult` as the return type.</span></span>

## <a name="content-negotiation"></a><span data-ttu-id="8feae-129">Inhaltsaushandlung</span><span class="sxs-lookup"><span data-stu-id="8feae-129">Content Negotiation</span></span>

<span data-ttu-id="8feae-130">Inhalts-Aushandlung (*Conneg* kurz) tritt auf, wenn der Client gibt eine [Accept-Header](https://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html).</span><span class="sxs-lookup"><span data-stu-id="8feae-130">Content negotiation (*conneg* for short) occurs when the client specifies an [Accept header](https://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html).</span></span> <span data-ttu-id="8feae-131">Das Standardformat von Core ASP.NET-MVC verwendet ist JSON.</span><span class="sxs-lookup"><span data-stu-id="8feae-131">The default format used by ASP.NET Core MVC is JSON.</span></span> <span data-ttu-id="8feae-132">Inhaltsaushandlung wird dadurch implementiert, `ObjectResult`.</span><span class="sxs-lookup"><span data-stu-id="8feae-132">Content negotiation is implemented by `ObjectResult`.</span></span> <span data-ttu-id="8feae-133">Es ist auch der bestimmten Aktionsergebnisse aus den Hilfsmethoden zurückgegebene Statuscode integriert (die basieren alle auf `ObjectResult`).</span><span class="sxs-lookup"><span data-stu-id="8feae-133">It is also built into the status code specific action results returned from the helper methods (which are all based on `ObjectResult`).</span></span> <span data-ttu-id="8feae-134">Sie können auch einen Modelltyp (eine Klasse, die Sie, als der Datentyp für die Übertragung definiert haben) zurückgeben, und das Framework wird automatisch umgebrochen, ihn in ein `ObjectResult` für Sie.</span><span class="sxs-lookup"><span data-stu-id="8feae-134">You can also return a model type (a class you've defined as your data transfer type) and the framework will automatically wrap it in an `ObjectResult` for you.</span></span>

<span data-ttu-id="8feae-135">Die folgende Aktionsmethode verwendet die `Ok` und `NotFound` Hilfsmethoden:</span><span class="sxs-lookup"><span data-stu-id="8feae-135">The following action method uses the `Ok` and `NotFound` helper methods:</span></span>

[!code-csharp[Main](./formatting/sample/Controllers/Api/AuthorsController.cs?highlight=8,10&range=28-38)]

<span data-ttu-id="8feae-136">Eine JSON-formatierte Antwort wird zurückgegeben, es sei denn, ein anderes Format wurde angefordert, und der Server kann die angeforderte Format zurück.</span><span class="sxs-lookup"><span data-stu-id="8feae-136">A JSON-formatted response will be returned unless another format was requested and the server can return the requested format.</span></span> <span data-ttu-id="8feae-137">Sie können ein Tool wie [Fiddler](http://www.telerik.com/fiddler) erstellen eine Anforderung, die einen Accept-Header enthält, und geben Sie ein anderes Format.</span><span class="sxs-lookup"><span data-stu-id="8feae-137">You can use a tool like [Fiddler](http://www.telerik.com/fiddler) to create a request that includes an Accept header and specify another format.</span></span> <span data-ttu-id="8feae-138">In diesem Fall, wenn der Server hat eine *Formatierer* können erzeugt eine Antwort im gewünschten Format, das Ergebnis wird zurückgegeben werden im Format Client bevorzugt.</span><span class="sxs-lookup"><span data-stu-id="8feae-138">In that case, if the server has a *formatter* that can produce a response in the requested format, the result will be returned in the client-preferred format.</span></span>

![Fiddler-Konsole, die mit einem manuell erstellten GET-Anforderung mit einem Wert des Accept-Header Application/XML-](formatting/_static/fiddler-composer.png)

<span data-ttu-id="8feae-140">Im obigen Screenshot Fiddler Ersteller wurde verwendet, um eine Anforderung generieren angeben `Accept: application/xml`.</span><span class="sxs-lookup"><span data-stu-id="8feae-140">In the above screenshot, the Fiddler Composer has been used to generate a request, specifying `Accept: application/xml`.</span></span> <span data-ttu-id="8feae-141">Standardmäßig ASP.NET Core MVC unterstützt nur JSON, selbst wenn ein anderes Format angegeben wird, das zurückgegebene Ergebnis ist JSON-formatiert.</span><span class="sxs-lookup"><span data-stu-id="8feae-141">By default, ASP.NET Core MVC only supports JSON, so even when another format is specified, the result returned is still JSON-formatted.</span></span> <span data-ttu-id="8feae-142">Sie sehen, dass zusätzliche Formatierungsprogramme im nächsten Abschnitt hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="8feae-142">You'll see how to add additional formatters in the next section.</span></span>

<span data-ttu-id="8feae-143">Controlleraktionen können POCOs (Plain Old CLR Objects), zurück in diesem Fall ASP.NET MVC erstellt automatisch ein `ObjectResult` , der das Objekt umschließt.</span><span class="sxs-lookup"><span data-stu-id="8feae-143">Controller actions can return POCOs (Plain Old CLR Objects), in which case ASP.NET MVC will automatically create an `ObjectResult` for you that wraps the object.</span></span> <span data-ttu-id="8feae-144">Der Client erhält das formatierte serialisierte Objekt (JSON-Format ist die Standardeinstellung; Sie können XML-Index oder anderen Formaten konfigurieren).</span><span class="sxs-lookup"><span data-stu-id="8feae-144">The client will get the formatted serialized object (JSON format is the default; you can configure XML or other formats).</span></span> <span data-ttu-id="8feae-145">Wenn das Objekt, das zurückgegeben wird `null`, und klicken Sie dann das Framework zurückgegeben wird ein `204 No Content` Antwort.</span><span class="sxs-lookup"><span data-stu-id="8feae-145">If the object being returned is `null`, then the framework will return a `204 No Content` response.</span></span>

<span data-ttu-id="8feae-146">Zurückgeben eines Objekttyps:</span><span class="sxs-lookup"><span data-stu-id="8feae-146">Returning an object type:</span></span>

[!code-csharp[Main](./formatting/sample/Controllers/Api/AuthorsController.cs?highlight=3&range=40-45)]

<span data-ttu-id="8feae-147">In diesem Beispiel erhält eine Anforderung für eine gültige Autor-Alias eine 200 OK-Antwort mit der Autor-Daten.</span><span class="sxs-lookup"><span data-stu-id="8feae-147">In the sample, a request for a valid author alias will receive a 200 OK response with the author's data.</span></span> <span data-ttu-id="8feae-148">Eine Anforderung für einen ungültigen Alias erhalten eine 204 No Content-Antwort.</span><span class="sxs-lookup"><span data-stu-id="8feae-148">A request for an invalid alias will receive a 204 No Content response.</span></span> <span data-ttu-id="8feae-149">Screenshots, die mit der Antwort in XML und JSON-Formate werden unten gezeigt.</span><span class="sxs-lookup"><span data-stu-id="8feae-149">Screenshots showing the response in XML and JSON formats are shown below.</span></span>

### <a name="content-negotiation-process"></a><span data-ttu-id="8feae-150">Inhaltsaushandlung-Prozess</span><span class="sxs-lookup"><span data-stu-id="8feae-150">Content Negotiation Process</span></span>

<span data-ttu-id="8feae-151">Inhalt *Aushandlung* nur erfolgt, wenn ein `Accept` Header angezeigt wird, in der Anforderung.</span><span class="sxs-lookup"><span data-stu-id="8feae-151">Content *negotiation* only takes place if an `Accept` header appears in the request.</span></span> <span data-ttu-id="8feae-152">Wenn eine Anforderung einen Accept-Header enthält, wird das Framework Listet die Medientypen im Accept-Header in der Rangfolge und versucht, einen Formatierer finden, der eine Antwort in einem der Formate, die durch den Accept-Header angegebenen erstellen kann.</span><span class="sxs-lookup"><span data-stu-id="8feae-152">When a request contains an accept header, the framework will enumerate the media types in the accept header in preference order and will try to find a formatter that can produce a response in one of the formats specified by the accept header.</span></span> <span data-ttu-id="8feae-153">Für den Fall, dass kein Formatierer gefunden wird, kann die Anforderung des Clients erfüllen, wird das Framework versucht, den ersten Formatierer gefunden, die eine Antwort erstellen kann (es sei denn, der Entwickler die Option für konfiguriert hat `MvcOptions` zurückzugebenden 406 nicht akzeptabel stattdessen).</span><span class="sxs-lookup"><span data-stu-id="8feae-153">In case no formatter is found that can satisfy the client's request, the framework will try to find the first formatter that can produce a response (unless the developer has configured the option on `MvcOptions` to return 406 Not Acceptable instead).</span></span> <span data-ttu-id="8feae-154">Wenn die Anforderung XML-Datei gibt, aber der XML-Formatierer nicht konfiguriert wurde, wird der JSON-Formatierer verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="8feae-154">If the request specifies XML, but the XML formatter has not been configured, then the JSON formatter will be used.</span></span> <span data-ttu-id="8feae-155">Üblicher, wenn kein Formatierer konfiguriert ist, der das angeforderte Format bereitstellen kann, wird der erste Formatierer, der das Objekt nicht formatieren kann verwendet.</span><span class="sxs-lookup"><span data-stu-id="8feae-155">More generally, if no formatter is configured that can provide the requested format, then the first formatter that can format the object is used.</span></span> <span data-ttu-id="8feae-156">Keine Header angegeben ist, wird der erste Formatierer, der das Objekt, das zurückgegeben werden bewältigen können zum Serialisieren der Antwort verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="8feae-156">If no header is given, the first formatter that can handle the object to be returned will be used to serialize the response.</span></span> <span data-ttu-id="8feae-157">In diesem Fall gibt es keine stattgefunden hat Aushandlung - Server ist die Festlegung, in welchem Format verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="8feae-157">In this case, there isn't any negotiation taking place - the server is determining what format it will use.</span></span>

> [!NOTE]
> <span data-ttu-id="8feae-158">Wenn die Accept-Header enthält `*/*`, der Header wird ignoriert, es sei denn, `RespectBrowserAcceptHeader` auf festgelegt ist auf "true" `MvcOptions`.</span><span class="sxs-lookup"><span data-stu-id="8feae-158">If the Accept header contains `*/*`, the Header will be ignored unless `RespectBrowserAcceptHeader` is set to true on `MvcOptions`.</span></span>

### <a name="browsers-and-content-negotiation"></a><span data-ttu-id="8feae-159">Browser und Inhaltsaushandlung</span><span class="sxs-lookup"><span data-stu-id="8feae-159">Browsers and Content Negotiation</span></span>

<span data-ttu-id="8feae-160">Im Gegensatz zu typischen-API-Clients Webbrowsern angeben tendenziell `Accept` Header, die eine Vielzahl von Formaten, einschließlich Platzhalter enthalten.</span><span class="sxs-lookup"><span data-stu-id="8feae-160">Unlike typical API clients, web browsers tend to supply `Accept` headers that include a wide array of formats, including wildcards.</span></span> <span data-ttu-id="8feae-161">Standardmäßig, wenn das Framework erkennt, dass die Anforderung von einem Browser eingeben, stammt ignoriert er die `Accept` Header und stattdessen return der Inhalt in der Anwendung konfigurierten Standardformat (JSON Wenn nicht anders konfiguriert).</span><span class="sxs-lookup"><span data-stu-id="8feae-161">By default, when the framework detects that the request is coming from a browser, it will ignore the `Accept` header and instead return the content in the application's configured default format (JSON unless otherwise configured).</span></span> <span data-ttu-id="8feae-162">Dies bietet eine konsistente Umgebung Verwendung von verschiedenen Browsern APIs nutzen.</span><span class="sxs-lookup"><span data-stu-id="8feae-162">This provides a more consistent experience when using different browsers to consume APIs.</span></span>

<span data-ttu-id="8feae-163">Wenn Sie Ihre Anwendung akzeptieren Browser accept-Header möchten, können Sie diese als Teil der Konfiguration des MVC-Konfiguration, durch Festlegen von `RespectBrowserAcceptHeader` auf `true` in der `ConfigureServices` Methode in *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="8feae-163">If you would prefer your application honor browser accept headers, you can configure this as part of MVC's configuration by setting `RespectBrowserAcceptHeader` to `true` in the `ConfigureServices` method in *Startup.cs*.</span></span>

```csharp
services.AddMvc(options =>
{
    options.RespectBrowserAcceptHeader = true; // false by default
});
```

## <a name="configuring-formatters"></a><span data-ttu-id="8feae-164">Konfigurieren der Formatierer</span><span class="sxs-lookup"><span data-stu-id="8feae-164">Configuring Formatters</span></span>

<span data-ttu-id="8feae-165">Wenn Ihre Anwendung zusätzliche Formate jenseits der Standardgrenze von JSON unterstützen muss, können Sie NuGet-Pakete hinzufügen und Konfigurieren von MVC, um diese zu unterstützen.</span><span class="sxs-lookup"><span data-stu-id="8feae-165">If your application needs to support additional formats beyond the default of JSON, you can add NuGet packages and configure MVC to support them.</span></span> <span data-ttu-id="8feae-166">Es gibt separate Formatierungsprogramme für ein- und Ausgaben.</span><span class="sxs-lookup"><span data-stu-id="8feae-166">There are separate formatters for input and output.</span></span> <span data-ttu-id="8feae-167">Eingabe Formatierungsprogramme werden von verwendet [Modell Bindung](model-binding.md); Ausgabe Formatierungsprogramme werden verwendet, um Antworten zu formatieren.</span><span class="sxs-lookup"><span data-stu-id="8feae-167">Input formatters are used by [Model Binding](model-binding.md); output formatters are used to format responses.</span></span> <span data-ttu-id="8feae-168">Sie können auch konfigurieren, [benutzerdefinierten Formatierer](../advanced/custom-formatters.md).</span><span class="sxs-lookup"><span data-stu-id="8feae-168">You can also configure [Custom Formatters](../advanced/custom-formatters.md).</span></span>

### <a name="adding-xml-format-support"></a><span data-ttu-id="8feae-169">Hinzufügen von Unterstützung für XML-Format</span><span class="sxs-lookup"><span data-stu-id="8feae-169">Adding XML Format Support</span></span>

<span data-ttu-id="8feae-170">Installieren Sie zum Hinzufügen der Unterstützung für XML-Formatierung der `Microsoft.AspNetCore.Mvc.Formatters.Xml` NuGet-Paket.</span><span class="sxs-lookup"><span data-stu-id="8feae-170">To add support for XML formatting, install the `Microsoft.AspNetCore.Mvc.Formatters.Xml` NuGet package.</span></span>

<span data-ttu-id="8feae-171">Fügen Sie die XmlSerializerFormatters MVCs-Konfiguration in *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="8feae-171">Add the XmlSerializerFormatters to MVC's configuration in *Startup.cs*:</span></span>

[!code-csharp[Main](./formatting/sample/Startup.cs?name=snippet1&highlight=2)]

<span data-ttu-id="8feae-172">Alternativ können Sie einfach das Ausgabe-Formatierungsprogramm hinzufügen:</span><span class="sxs-lookup"><span data-stu-id="8feae-172">Alternately, you can add just the output formatter:</span></span>

```csharp
services.AddMvc(options =>
{
    options.OutputFormatters.Add(new XmlSerializerOutputFormatter());
});
```

<span data-ttu-id="8feae-173">Diese beiden Ansätze werden Serialisierung von Ergebnissen mit `System.Xml.Serialization.XmlSerializer`.</span><span class="sxs-lookup"><span data-stu-id="8feae-173">These two approaches will serialize results using `System.Xml.Serialization.XmlSerializer`.</span></span> <span data-ttu-id="8feae-174">Falls gewünscht, können Sie die `System.Runtime.Serialization.DataContractSerializer` durch seine zugeordneten Formatierer hinzufügen:</span><span class="sxs-lookup"><span data-stu-id="8feae-174">If you prefer, you can use the `System.Runtime.Serialization.DataContractSerializer` by adding its associated formatter:</span></span>

```csharp
services.AddMvc(options =>
{
    options.OutputFormatters.Add(new XmlDataContractSerializerOutputFormatter());
});
```

<span data-ttu-id="8feae-175">Nachdem Sie die Unterstützung für XML-Formatierung hinzugefügt haben, sollte Ihre Controllermethoden das entsprechende Format basierend auf der Anforderung zurückgeben `Accept` -Header, als diese Fiddler veranschaulicht:</span><span class="sxs-lookup"><span data-stu-id="8feae-175">Once you've added support for XML formatting, your controller methods should return the appropriate format based on the request's `Accept` header, as this Fiddler example demonstrates:</span></span>

![Fiddler-Konsole: das unformatierte Registerkarte für die Anforderung wird der Wert des Accept-Header ist Application/Xml.](formatting/_static/xml-response.png)

<span data-ttu-id="8feae-178">Sehen Sie in der Registerkarte "Inspektoren", die die Rohdaten GET-Anforderung erstellt wurde, mit einer `Accept: application/xml` Header festlegen.</span><span class="sxs-lookup"><span data-stu-id="8feae-178">You can see in the Inspectors tab that the Raw GET request was made with an `Accept: application/xml` header set.</span></span> <span data-ttu-id="8feae-179">Die Antwort Bereich zeigt die `Content-Type: application/xml` -Header, und die `Author` Objekt in XML serialisiert wurde.</span><span class="sxs-lookup"><span data-stu-id="8feae-179">The response pane shows the `Content-Type: application/xml` header, and the `Author` object has been serialized to XML.</span></span>

<span data-ttu-id="8feae-180">Verwenden Sie die Registerkarte "Composer" So ändern Sie die Anforderung an `application/json` in der `Accept` Header.</span><span class="sxs-lookup"><span data-stu-id="8feae-180">Use the Composer tab to modify the request to specify `application/json` in the `Accept` header.</span></span> <span data-ttu-id="8feae-181">Führen Sie die Anforderung und die Antwort als JSON formatiert:</span><span class="sxs-lookup"><span data-stu-id="8feae-181">Execute the request, and the response will be formatted as JSON:</span></span>

![Fiddler-Konsole: das unformatierte Registerkarte für die Anforderung wird der Wert des Accept-Header ist Application/Json.](formatting/_static/json-response-fiddler.png)

<span data-ttu-id="8feae-184">In dieser Abbildung sehen Sie die Anforderung legt Header `Accept: application/json` und die Antwort gibt an, die identisch mit der `Content-Type`.</span><span class="sxs-lookup"><span data-stu-id="8feae-184">In this screenshot, you can see the request sets a header of `Accept: application/json` and the response specifies the same as its `Content-Type`.</span></span> <span data-ttu-id="8feae-185">Die `Author` Objekt im Text der Antwort im JSON-Format dargestellt wird.</span><span class="sxs-lookup"><span data-stu-id="8feae-185">The `Author` object is shown in the body of the response, in JSON format.</span></span>

### <a name="forcing-a-particular-format"></a><span data-ttu-id="8feae-186">Erzwingen ein bestimmtes Format</span><span class="sxs-lookup"><span data-stu-id="8feae-186">Forcing a Particular Format</span></span>

<span data-ttu-id="8feae-187">Wenn Sie die Antwortformate für eine bestimmte Aktion einschränken möchten, können Sie, können Sie anwenden der `[Produces]` Filter.</span><span class="sxs-lookup"><span data-stu-id="8feae-187">If you would like to restrict the response formats for a specific action you can, you can apply the `[Produces]` filter.</span></span> <span data-ttu-id="8feae-188">Die `[Produces]` Filter gibt die Antwortformate für eine bestimmte Aktion (oder Domänencontroller).</span><span class="sxs-lookup"><span data-stu-id="8feae-188">The `[Produces]` filter specifies the response formats for a specific action (or controller).</span></span> <span data-ttu-id="8feae-189">Wie die meisten [Filter](../controllers/filters.md), dies kann auf die Aktion, Controller, oder im globalen Gültigkeitsbereich angewendet werden.</span><span class="sxs-lookup"><span data-stu-id="8feae-189">Like most [Filters](../controllers/filters.md), this can be applied at the action, controller, or global scope.</span></span>

```csharp
[Produces("application/json")]
public class AuthorsController
```

<span data-ttu-id="8feae-190">Die `[Produces]` Filter wird erzwingen, dass alle Aktionen innerhalb der `AuthorsController` JSON-formatierte Antworten zurückgeben, selbst wenn andere Formatierungsprogramme für die Anwendung und vom Client bereitgestellten konfiguriert wurden ein `Accept` Anfordern von einem anderen Header verfügbar das Format.</span><span class="sxs-lookup"><span data-stu-id="8feae-190">The `[Produces]` filter will force all actions within the `AuthorsController` to return JSON-formatted responses, even if other formatters were configured for the application and the client provided an `Accept` header requesting a different, available format.</span></span> <span data-ttu-id="8feae-191">Finden Sie unter [Filter](../controllers/filters.md) erfahren Sie mehr, einschließlich Informationen zum Anwenden von Filtern Global.</span><span class="sxs-lookup"><span data-stu-id="8feae-191">See [Filters](../controllers/filters.md) to learn more, including how to apply filters globally.</span></span>

### <a name="special-case-formatters"></a><span data-ttu-id="8feae-192">Spezielle Formatierungsprogramme für die Groß-/Kleinschreibung</span><span class="sxs-lookup"><span data-stu-id="8feae-192">Special Case Formatters</span></span>

<span data-ttu-id="8feae-193">Einige Sonderfälle werden mithilfe der integrierten Formatierer implementiert.</span><span class="sxs-lookup"><span data-stu-id="8feae-193">Some special cases are implemented using built-in formatters.</span></span> <span data-ttu-id="8feae-194">Standardmäßig `string` Rückgabetypen formatiert als *Text/Plain* (*Text/html* ggf. über `Accept` Header).</span><span class="sxs-lookup"><span data-stu-id="8feae-194">By default, `string` return types will be formatted as *text/plain* (*text/html* if requested via `Accept` header).</span></span> <span data-ttu-id="8feae-195">Dieses Verhalten kann entfernt werden, durch das Entfernen der `TextOutputFormatter`.</span><span class="sxs-lookup"><span data-stu-id="8feae-195">This behavior can be removed by removing the `TextOutputFormatter`.</span></span> <span data-ttu-id="8feae-196">Entfernen Sie Formatierer in die `Configure` Methode im *Startup.cs* (siehe unten).</span><span class="sxs-lookup"><span data-stu-id="8feae-196">You remove formatters in the `Configure` method in *Startup.cs* (shown below).</span></span> <span data-ttu-id="8feae-197">Aktionen, die ein Modellobjekt haben zurückgeben wird Rückgabetyp 204 ohne Inhalt Antwort beim Zurückgeben der `null`.</span><span class="sxs-lookup"><span data-stu-id="8feae-197">Actions that have a model object return type will return a 204 No Content response when returning `null`.</span></span> <span data-ttu-id="8feae-198">Dieses Verhalten kann entfernt werden, durch das Entfernen der `HttpNoContentOutputFormatter`.</span><span class="sxs-lookup"><span data-stu-id="8feae-198">This behavior can be removed by removing the `HttpNoContentOutputFormatter`.</span></span> <span data-ttu-id="8feae-199">Das folgende Codebeispiel entfernt die `TextOutputFormatter` und `HttpNoContentOutputFormatter`.</span><span class="sxs-lookup"><span data-stu-id="8feae-199">The following code removes the `TextOutputFormatter` and `HttpNoContentOutputFormatter`.</span></span>

```csharp
services.AddMvc(options =>
{
    options.OutputFormatters.RemoveType<TextOutputFormatter>();
    options.OutputFormatters.RemoveType<HttpNoContentOutputFormatter>();
});
```

<span data-ttu-id="8feae-200">Ohne die `TextOutputFormatter`, `string` Typen zurückgeben 406 nicht akzeptabel ist, z. B.</span><span class="sxs-lookup"><span data-stu-id="8feae-200">Without the `TextOutputFormatter`, `string` return types return 406 Not Acceptable, for example.</span></span> <span data-ttu-id="8feae-201">Beachten Sie, dass wenn ein XML-Formatierer vorhanden ist, wird es formatieren `string` Typen zurückgeben, wenn die `TextOutputFormatter` wird entfernt.</span><span class="sxs-lookup"><span data-stu-id="8feae-201">Note that if an XML formatter exists, it will format `string` return types if the `TextOutputFormatter` is removed.</span></span>

<span data-ttu-id="8feae-202">Ohne die `HttpNoContentOutputFormatter`, null-Objekte mithilfe des konfigurierten Formatierers formatiert sind.</span><span class="sxs-lookup"><span data-stu-id="8feae-202">Without the `HttpNoContentOutputFormatter`, null objects are formatted using the configured formatter.</span></span> <span data-ttu-id="8feae-203">Beispielsweise wird das JSON-Formatierungsprogramm einfach eine Antwort mit einem Text des zurückgeben `null`, während der XML-Formatierer ein leeres XML-Element mit dem Attribut zurückliefert `xsi:nil="true"` festgelegt.</span><span class="sxs-lookup"><span data-stu-id="8feae-203">For example, the JSON formatter will simply return a response with a body of `null`, while the XML formatter will return an empty XML element with the attribute `xsi:nil="true"` set.</span></span>

## <a name="response-format-url-mappings"></a><span data-ttu-id="8feae-204">Antwort Format URL Zuordnungen</span><span class="sxs-lookup"><span data-stu-id="8feae-204">Response Format URL Mappings</span></span>

<span data-ttu-id="8feae-205">Clients können Sie ein bestimmtes Format als Teil der URL, z. B. in der Abfragezeichenfolge oder einen Teil des Pfads oder mithilfe von Format-spezifische Erweiterung z. B. XML- oder JSON anfordern.</span><span class="sxs-lookup"><span data-stu-id="8feae-205">Clients can request a particular format as part of the URL, such as in the query string or part of the path, or by using a format-specific file extension such as .xml or .json.</span></span> <span data-ttu-id="8feae-206">In der Route an, die die API verwendet, sollte die Zuordnung von Anforderungspfad angegeben werden.</span><span class="sxs-lookup"><span data-stu-id="8feae-206">The mapping from request path should be specified in the route the API is using.</span></span> <span data-ttu-id="8feae-207">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="8feae-207">For example:</span></span>

```csharp
[FormatFilter]
public class ProductsController
{
    [Route("[controller]/[action]/{id}.{format?}")]
    public Product GetById(int id)
```

<span data-ttu-id="8feae-208">Diese Route entschied sich auf das angeforderte Format als Erweiterung optional angegeben werden.</span><span class="sxs-lookup"><span data-stu-id="8feae-208">This route would allow the requested format to be specified as an optional file extension.</span></span> <span data-ttu-id="8feae-209">Die `[FormatFilter]` Attribut überprüft das Vorhandensein des Werts Format in die `RouteData` und das Antwortformat wird an den entsprechenden Formatierer zugeordnet werden, wenn die Antwort erstellt wird.</span><span class="sxs-lookup"><span data-stu-id="8feae-209">The `[FormatFilter]` attribute checks for the existence of the format value in the `RouteData` and will map the response format to the appropriate formatter when the response is created.</span></span>

| <span data-ttu-id="8feae-210">Route</span><span class="sxs-lookup"><span data-stu-id="8feae-210">Route</span></span>                      | <span data-ttu-id="8feae-211">Formatter</span><span class="sxs-lookup"><span data-stu-id="8feae-211">Formatter</span></span>                          |
| -------------------------- | ---------------------------------- |
| `/products/GetById/5`      | <span data-ttu-id="8feae-212">Der Standardformatierer-Ausgabe</span><span class="sxs-lookup"><span data-stu-id="8feae-212">The default output formatter</span></span>       |
| `/products/GetById/5.json` | <span data-ttu-id="8feae-213">Das JSON-Formatierungsprogramm (sofern konfiguriert)</span><span class="sxs-lookup"><span data-stu-id="8feae-213">The JSON formatter (if configured)</span></span> |
| `/products/GetById/5.xml`  | <span data-ttu-id="8feae-214">Der XML-Formatierer (sofern konfiguriert)</span><span class="sxs-lookup"><span data-stu-id="8feae-214">The XML formatter (if configured)</span></span>  |
