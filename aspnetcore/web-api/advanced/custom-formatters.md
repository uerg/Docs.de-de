---
title: Benutzerdefinierte Formatierer in Web-APIs in ASP.NET Core
author: rick-anderson
description: Informationen zum Erstellen und Verwenden von benutzerdefinierten Formatierern für Web-APIs in ASP.NET Core.
ms.author: tdykstra
ms.date: 02/08/2017
uid: web-api/advanced/custom-formatters
ms.openlocfilehash: ee6f166ced41c41506f2a17a7d362399c165b718
ms.sourcegitcommit: 2d3e5422d530203efdaf2014d1d7df31f88d08d0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/05/2018
ms.locfileid: "51020649"
---
# <a name="custom-formatters-in-aspnet-core-web-api"></a><span data-ttu-id="b7ce8-103">Benutzerdefinierte Formatierer in Web-APIs in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="b7ce8-103">Custom formatters in ASP.NET Core Web API</span></span>

<span data-ttu-id="b7ce8-104">Von [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="b7ce8-104">By [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="b7ce8-105">Im ASP.NET Core MVC ist die Unterstützung von Datenaustausch in Web-APIs über JSON, XML oder Nur-Text-Formate integriert.</span><span class="sxs-lookup"><span data-stu-id="b7ce8-105">ASP.NET Core MVC has built-in support for data exchange in web APIs by using JSON, XML, or plain text formats.</span></span> <span data-ttu-id="b7ce8-106">In diesem Artikel wird dargestellt, wie Sie die Unterstützung von zusätzlichen Formaten hinzufügen, indem Sie benutzerdefinierte Formatierer erstellen.</span><span class="sxs-lookup"><span data-stu-id="b7ce8-106">This article shows how to add support for additional formats by creating custom formatters.</span></span>

<span data-ttu-id="b7ce8-107">[Anzeigen oder Herunterladen von Beispielcode](https://github.com/aspnet/Docs/tree/master/aspnetcore/web-api/advanced/custom-formatters/sample) ([Vorgehensweise zum Herunterladen](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="b7ce8-107">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/web-api/advanced/custom-formatters/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="when-to-use-custom-formatters"></a><span data-ttu-id="b7ce8-108">Empfohlene Verwendung von benutzerdefinierten Formatierern</span><span class="sxs-lookup"><span data-stu-id="b7ce8-108">When to use custom formatters</span></span>

<span data-ttu-id="b7ce8-109">Verwenden Sie einen benutzerdefinierten Formatierer, wenn Sie möchten, dass der [Inhaltaushandlungsvorgang](xref:web-api/advanced/formatting#content-negotiation) einen Inhaltstyp unterstützt, der nicht von den integrierten Formatieren (JSON, XML und Nur-Text) unterstützt wird.</span><span class="sxs-lookup"><span data-stu-id="b7ce8-109">Use a custom formatter when you want the [content negotiation](xref:web-api/advanced/formatting#content-negotiation) process to support a content type that isn't supported by the built-in formatters (JSON, XML, and plain text).</span></span>

<span data-ttu-id="b7ce8-110">Wenn z.B. einige der Clients für Ihre Web-API das [Protobuf](https://github.com/google/protobuf)-Format verarbeiten können, sollten Sie auch Protobuf für diese Clients verwenden, wenn Sie effizient arbeiten möchten.</span><span class="sxs-lookup"><span data-stu-id="b7ce8-110">For example, if some of the clients for your web API can handle the [Protobuf](https://github.com/google/protobuf) format, you might want to use Protobuf with those clients because it's more efficient.</span></span> <span data-ttu-id="b7ce8-111">Möglicherweise möchten Sie aber lieber Ihre Web-API verwenden, um Namen und Adressen von Kontakten im [vCard](https://wikipedia.org/wiki/VCard)-Format zu versenden – ein häufig zum Austauschen von Kontaktdaten verwendetes Format.</span><span class="sxs-lookup"><span data-stu-id="b7ce8-111">Or you might want your web API to send contact names and addresses in [vCard](https://wikipedia.org/wiki/VCard) format, a commonly used format for exchanging contact data.</span></span> <span data-ttu-id="b7ce8-112">Die in diesem Artikel enthaltene Beispiel-App implementiert einen einfachen vCard-Formatierer.</span><span class="sxs-lookup"><span data-stu-id="b7ce8-112">The sample app provided with this article implements a simple vCard formatter.</span></span>

## <a name="overview-of-how-to-use-a-custom-formatter"></a><span data-ttu-id="b7ce8-113">Übersicht über die Verwendung des Formatierers</span><span class="sxs-lookup"><span data-stu-id="b7ce8-113">Overview of how to use a custom formatter</span></span>

<span data-ttu-id="b7ce8-114">Im Folgenden werden einige Schritte zum Erstellen und Verwenden eines benutzerdefinierten Formatierers beschrieben:</span><span class="sxs-lookup"><span data-stu-id="b7ce8-114">Here are the steps to create and use a custom formatter:</span></span>

* <span data-ttu-id="b7ce8-115">Erstellen Sie eine Klasse für den Ausgabeformatierer, wenn Sie die Daten serialisieren möchten, um diese an den Client zu senden.</span><span class="sxs-lookup"><span data-stu-id="b7ce8-115">Create an output formatter class if you want to serialize data to send to the client.</span></span>
* <span data-ttu-id="b7ce8-116">Erstellen Sie eine Klasse für den Eingabeformatierer, wenn Sie die Daten deserialisieren möchten, die vom Client gesendet wurden.</span><span class="sxs-lookup"><span data-stu-id="b7ce8-116">Create an input formatter class if you want to deserialize data received from the client.</span></span>
* <span data-ttu-id="b7ce8-117">Fügen Sie Instanzen Ihrer Formatierer zu den Auflistungen `InputFormatters` und `OutputFormatters` in [MvcOptions](/dotnet/api/microsoft.aspnetcore.mvc.mvcoptions) hinzu.</span><span class="sxs-lookup"><span data-stu-id="b7ce8-117">Add instances of your formatters to the `InputFormatters` and `OutputFormatters` collections in [MvcOptions](/dotnet/api/microsoft.aspnetcore.mvc.mvcoptions).</span></span>

<span data-ttu-id="b7ce8-118">In den Folgenden Abschnitten erhalten Sie Anweisungen und Codebeispiele zu diesen Schritten.</span><span class="sxs-lookup"><span data-stu-id="b7ce8-118">The following sections provide guidance and code examples for each of these steps.</span></span>

## <a name="how-to-create-a-custom-formatter-class"></a><span data-ttu-id="b7ce8-119">Erstellen einer benutzerdefinierten Formatiererklasse</span><span class="sxs-lookup"><span data-stu-id="b7ce8-119">How to create a custom formatter class</span></span>

<span data-ttu-id="b7ce8-120">Erstellen eines Formatierers:</span><span class="sxs-lookup"><span data-stu-id="b7ce8-120">To create a formatter:</span></span>

* <span data-ttu-id="b7ce8-121">Leiten Sie die Klasse von der entsprechenden Basisklasse ab.</span><span class="sxs-lookup"><span data-stu-id="b7ce8-121">Derive the class from the appropriate base class.</span></span>
* <span data-ttu-id="b7ce8-122">Geben Sie gültige Medientypen und Codierungen im Konstruktor an.</span><span class="sxs-lookup"><span data-stu-id="b7ce8-122">Specify valid media types and encodings in the constructor.</span></span>
* <span data-ttu-id="b7ce8-123">Überschreiben Sie die Methoden `CanReadType`/`CanWriteType`</span><span class="sxs-lookup"><span data-stu-id="b7ce8-123">Override `CanReadType`/`CanWriteType` methods</span></span>
* <span data-ttu-id="b7ce8-124">Überschreiben Sie die Methoden `ReadRequestBodyAsync`/`WriteResponseBodyAsync`</span><span class="sxs-lookup"><span data-stu-id="b7ce8-124">Override `ReadRequestBodyAsync`/`WriteResponseBodyAsync` methods</span></span>
  
### <a name="derive-from-the-appropriate-base-class"></a><span data-ttu-id="b7ce8-125">Ableiten von der entsprechenden Basisklasse</span><span class="sxs-lookup"><span data-stu-id="b7ce8-125">Derive from the appropriate base class</span></span>

<span data-ttu-id="b7ce8-126">Leiten Sie für Textmedientypen wie vCard von den Basisklassen [TextInputFormatter](/dotnet/api/microsoft.aspnetcore.mvc.formatters.textinputformatter) oder [TextOutputFormatter](/dotnet/api/microsoft.aspnetcore.mvc.formatters.textoutputformatter) ab.</span><span class="sxs-lookup"><span data-stu-id="b7ce8-126">For text media types (for example, vCard), derive from the [TextInputFormatter](/dotnet/api/microsoft.aspnetcore.mvc.formatters.textinputformatter) or [TextOutputFormatter](/dotnet/api/microsoft.aspnetcore.mvc.formatters.textoutputformatter) base class.</span></span>

[!code-csharp[](custom-formatters/sample/Formatters/VcardOutputFormatter.cs?name=classdef)]

<span data-ttu-id="b7ce8-127">Einen Eingabeformatierer finden Sie z.B. in der [Beispiel-App](https://github.com/aspnet/Docs/tree/master/aspnetcore/web-api/advanced/custom-formatters/sample).</span><span class="sxs-lookup"><span data-stu-id="b7ce8-127">For an input formatter example, see the [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/web-api/advanced/custom-formatters/sample).</span></span>

<span data-ttu-id="b7ce8-128">Leiten Sie für Binärtypen von den Basisklassen [InputFormatter](/dotnet/api/microsoft.aspnetcore.mvc.formatters.inputformatter) oder [OutputFormatter](/dotnet/api/microsoft.aspnetcore.mvc.formatters.outputformatter) ab.</span><span class="sxs-lookup"><span data-stu-id="b7ce8-128">For binary types, derive from the [InputFormatter](/dotnet/api/microsoft.aspnetcore.mvc.formatters.inputformatter) or [OutputFormatter](/dotnet/api/microsoft.aspnetcore.mvc.formatters.outputformatter) base class.</span></span>

### <a name="specify-valid-media-types-and-encodings"></a><span data-ttu-id="b7ce8-129">Angeben von gültigen Medientypen und Codierungen</span><span class="sxs-lookup"><span data-stu-id="b7ce8-129">Specify valid media types and encodings</span></span>

<span data-ttu-id="b7ce8-130">Geben Sie im Konstruktor gültige Medientypen und Codierungen an, indem Sie `SupportedMediaTypes`- und `SupportedEncodings`-Sammlungen hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="b7ce8-130">In the constructor, specify valid media types and encodings by adding to the `SupportedMediaTypes` and `SupportedEncodings` collections.</span></span>

[!code-csharp[](custom-formatters/sample/Formatters/VcardOutputFormatter.cs?name=ctor&highlight=3,5-6)]

<span data-ttu-id="b7ce8-131">Einen Eingabeformatierer finden Sie z.B. in der [Beispiel-App](https://github.com/aspnet/Docs/tree/master/aspnetcore/web-api/advanced/custom-formatters/sample).</span><span class="sxs-lookup"><span data-stu-id="b7ce8-131">For an input formatter example, see the [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/web-api/advanced/custom-formatters/sample).</span></span>

> [!NOTE]
> <span data-ttu-id="b7ce8-132">Sie können in einer Formatiererklasse keine Dependency Injection für den Konstruktor durchführen.</span><span class="sxs-lookup"><span data-stu-id="b7ce8-132">You can't do constructor dependency injection in a formatter class.</span></span> <span data-ttu-id="b7ce8-133">Beispielsweise können Sie keine Protokollierung abrufen, indem Sie dem Konstruktor den Protokollparameter hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="b7ce8-133">For example, you can't get a logger by adding a logger parameter to the constructor.</span></span> <span data-ttu-id="b7ce8-134">Sie müssen das Kontextobjekt verwenden, das an Ihre Methoden übergeben wird, um auf Dienste zuzugreifen.</span><span class="sxs-lookup"><span data-stu-id="b7ce8-134">To access services, you have to use the context object that gets passed in to your methods.</span></span> <span data-ttu-id="b7ce8-135">Im [nachfolgenden](#read-write) Codebeispiel wird deutlich, wie Sie hierzu vorgehen.</span><span class="sxs-lookup"><span data-stu-id="b7ce8-135">A code example [below](#read-write) shows how to do this.</span></span>

### <a name="override-canreadtypecanwritetype"></a><span data-ttu-id="b7ce8-136">Überschreiben von „CanReadType/CanWriteType“</span><span class="sxs-lookup"><span data-stu-id="b7ce8-136">Override CanReadType/CanWriteType</span></span>

<span data-ttu-id="b7ce8-137">Geben Sie den Typ an, den Sie deserialisieren bzw. serialisieren möchten, indem Sie die Methoden `CanReadType` oder `CanWriteType` überschreiben.</span><span class="sxs-lookup"><span data-stu-id="b7ce8-137">Specify the type you can deserialize into or serialize from by overriding the `CanReadType` or `CanWriteType` methods.</span></span> <span data-ttu-id="b7ce8-138">Es kann z.B. sein, dass Sie nur vCard-Text über einen `Contact`-Typ bzw. umgekehrt erstellen können.</span><span class="sxs-lookup"><span data-stu-id="b7ce8-138">For example, you might only be able to create vCard text from a `Contact` type and vice versa.</span></span>

[!code-csharp[](custom-formatters/sample/Formatters/VcardOutputFormatter.cs?name=canwritetype)]

<span data-ttu-id="b7ce8-139">Einen Eingabeformatierer finden Sie z.B. in der [Beispiel-App](https://github.com/aspnet/Docs/tree/master/aspnetcore/web-api/advanced/custom-formatters/sample).</span><span class="sxs-lookup"><span data-stu-id="b7ce8-139">For an input formatter example, see the [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/web-api/advanced/custom-formatters/sample).</span></span>

#### <a name="the-canwriteresult-method"></a><span data-ttu-id="b7ce8-140">Die CanWriteResult-Methode</span><span class="sxs-lookup"><span data-stu-id="b7ce8-140">The CanWriteResult method</span></span>

<span data-ttu-id="b7ce8-141">Manchmal müssen Sie `CanWriteResult` anstelle von `CanWriteType` überschreiben.</span><span class="sxs-lookup"><span data-stu-id="b7ce8-141">In some scenarios you have to override `CanWriteResult` instead of `CanWriteType`.</span></span> <span data-ttu-id="b7ce8-142">Verwenden Sie `CanWriteResult`, wenn alle der folgenden Bedingungen zutreffen:</span><span class="sxs-lookup"><span data-stu-id="b7ce8-142">Use `CanWriteResult` if the following conditions are true:</span></span>

* <span data-ttu-id="b7ce8-143">Ihre Aktionsmethode gibt eine Modellklasse zurück.</span><span class="sxs-lookup"><span data-stu-id="b7ce8-143">Your action method returns a model class.</span></span>
* <span data-ttu-id="b7ce8-144">Es gibt abgeleitete Klassen, die möglicherweise zur Laufzeit zurückgegeben werden.</span><span class="sxs-lookup"><span data-stu-id="b7ce8-144">There are derived classes which might be returned at runtime.</span></span>
* <span data-ttu-id="b7ce8-145">Sie müssen zur Laufzeit wissen, welche abgeleitete Klasse von der Aktion zurückgegeben wurde.</span><span class="sxs-lookup"><span data-stu-id="b7ce8-145">You need to know at runtime which derived class was returned by the action.</span></span>

<span data-ttu-id="b7ce8-146">Nehmen Sie z.B. an, dass die Signatur Ihrer Aktionsmethode einen `Person`-Typ zurückgibt. Sie kann dann aber auch den Typ `Student` oder `Instructor` zurückgeben, der von `Person` abgeleitet wird.</span><span class="sxs-lookup"><span data-stu-id="b7ce8-146">For example, suppose your action method signature returns a `Person` type, but it may return a `Student` or `Instructor` type that derives from `Person`.</span></span> <span data-ttu-id="b7ce8-147">Wenn Sie möchten, dass Ihr Formatierer nur `Student`-Objekte verarbeitet, überprüfen Sie den [Objekt](/dotnet/api/microsoft.aspnetcore.mvc.formatters.outputformattercanwritecontext#Microsoft_AspNetCore_Mvc_Formatters_OutputFormatterCanWriteContext_Object)-Typ im Kontextobjekt, das in der `CanWriteResult`-Methode zur Verfügung gestellt wurde.</span><span class="sxs-lookup"><span data-stu-id="b7ce8-147">If you want your formatter to handle only `Student` objects, check the type of [Object](/dotnet/api/microsoft.aspnetcore.mvc.formatters.outputformattercanwritecontext#Microsoft_AspNetCore_Mvc_Formatters_OutputFormatterCanWriteContext_Object) in the context object provided to the `CanWriteResult` method.</span></span> <span data-ttu-id="b7ce8-148">Beachten Sie, dass es nicht nötig ist, `CanWriteResult` zu verwenden, wenn die Aktionsmethode `IActionResult` zurückgibt. In diesem Fall erhält die `CanWriteType`-Methode den Runtimetyp.</span><span class="sxs-lookup"><span data-stu-id="b7ce8-148">Note that it's not necessary to use `CanWriteResult` when the action method returns `IActionResult`; in that case, the `CanWriteType` method receives the runtime type.</span></span>

<a id="read-write"></a>
### <a name="override-readrequestbodyasyncwriteresponsebodyasync"></a><span data-ttu-id="b7ce8-149">Überschreiben von „ReadRequestBodyAsync/WriteResponseBodyAsync“</span><span class="sxs-lookup"><span data-stu-id="b7ce8-149">Override ReadRequestBodyAsync/WriteResponseBodyAsync</span></span>

<span data-ttu-id="b7ce8-150">Sie deserialisieren oder serialisieren manuell in `ReadRequestBodyAsync` oder `WriteResponseBodyAsync`.</span><span class="sxs-lookup"><span data-stu-id="b7ce8-150">You do the actual work of deserializing or serializing in `ReadRequestBodyAsync` or `WriteResponseBodyAsync`.</span></span> <span data-ttu-id="b7ce8-151">Die markierten Zeilen im folgenden Beispiel zeigen, wie Sie Dienste aus dem Dependency Injection-Container abrufen. Sie können diese nicht aus den Konstruktorparametern abrufen.</span><span class="sxs-lookup"><span data-stu-id="b7ce8-151">The highlighted lines in the following example show how to get services from the dependency injection container (you can't get them from constructor parameters).</span></span>

[!code-csharp[](custom-formatters/sample/Formatters/VcardOutputFormatter.cs?name=writeresponse&highlight=3-4)]

<span data-ttu-id="b7ce8-152">Einen Eingabeformatierer finden Sie z.B. in der [Beispiel-App](https://github.com/aspnet/Docs/tree/master/aspnetcore/web-api/advanced/custom-formatters/sample).</span><span class="sxs-lookup"><span data-stu-id="b7ce8-152">For an input formatter example, see the [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/web-api/advanced/custom-formatters/sample).</span></span>

## <a name="how-to-configure-mvc-to-use-a-custom-formatter"></a><span data-ttu-id="b7ce8-153">Konfigurieren von MVC zum Verwenden eines benutzerdefinierten Formatierers</span><span class="sxs-lookup"><span data-stu-id="b7ce8-153">How to configure MVC to use a custom formatter</span></span>

<span data-ttu-id="b7ce8-154">Wenn Sie einen benutzerdefinierten Formatierer verwenden, fügen Sie der `InputFormatters`- oder `OutputFormatters`-Sammlung eine Instanz der Formatiererklasse hinzu.</span><span class="sxs-lookup"><span data-stu-id="b7ce8-154">To use a custom formatter, add an instance of the formatter class to the `InputFormatters` or `OutputFormatters` collection.</span></span>

[!code-csharp[](custom-formatters/sample/Startup.cs?name=mvcoptions&highlight=3-4)]

<span data-ttu-id="b7ce8-155">Formatierer werden in der Reihenfolge überprüft, in der Sie sie einfügen.</span><span class="sxs-lookup"><span data-stu-id="b7ce8-155">Formatters are evaluated in the order you insert them.</span></span> <span data-ttu-id="b7ce8-156">Der erste hat Vorrang.</span><span class="sxs-lookup"><span data-stu-id="b7ce8-156">The first one takes precedence.</span></span>

## <a name="next-steps"></a><span data-ttu-id="b7ce8-157">Nächste Schritte</span><span class="sxs-lookup"><span data-stu-id="b7ce8-157">Next steps</span></span>

<span data-ttu-id="b7ce8-158">Überprüfen Sie die [Beispielanwendung](https://github.com/aspnet/Docs/tree/master/aspnetcore/web-api/advanced/custom-formatters/sample), die einfache Eingabe- und Ausgabeformatierer für vCard implementiert.</span><span class="sxs-lookup"><span data-stu-id="b7ce8-158">See the [sample application](https://github.com/aspnet/Docs/tree/master/aspnetcore/web-api/advanced/custom-formatters/sample), which implements simple vCard input and output formatters.</span></span> <span data-ttu-id="b7ce8-159">Die Anwendung liest und schreibt vCards, die dem folgenden Beispiel ähneln:</span><span class="sxs-lookup"><span data-stu-id="b7ce8-159">The application reads and writes vCards that look like the following example:</span></span>

```
BEGIN:VCARD
VERSION:2.1
N:Davolio;Nancy
FN:Nancy Davolio
UID:20293482-9240-4d68-b475-325df4a83728
END:VCARD
```

<span data-ttu-id="b7ce8-160">Wenn Sie die vCard-Ausgabe anzeigen wollen, führen Sie die Anwendung aus, und senden Sie eine Get-Anforderung mit dem Accept-Header „text/vcard“ an `http://localhost:63313/api/contacts/` (wenn Sie über Visual Studio arbeiten) oder `http://localhost:5000/api/contacts/` (wenn Sie über die Befehlszeile arbeiten).</span><span class="sxs-lookup"><span data-stu-id="b7ce8-160">To see vCard output, run the application and send a Get request with Accept header "text/vcard" to `http://localhost:63313/api/contacts/` (when running from Visual Studio) or `http://localhost:5000/api/contacts/` (when running from the command line).</span></span>

<span data-ttu-id="b7ce8-161">Wenn Sie eine vCard einer speicherinterne Sammlung von Kontakten hinzufügen möchten, senden Sie eine Post-Anforderung an dieselbe URL mit dem Content-Type-Header „text/vcard“ und einem vCard-Text, der wie im obenstehenden Beispiel formatiert ist.</span><span class="sxs-lookup"><span data-stu-id="b7ce8-161">To add a vCard to the in-memory collection of contacts, send a Post request to the same URL, with Content-Type header "text/vcard" and with vCard text in the body, formatted like the example above.</span></span>
