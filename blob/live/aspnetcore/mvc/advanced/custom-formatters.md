---
title: Benutzerdefinierten Formatierer in ASP.NET Core MVC-Web-APIs
author: tdykstra
description: "Informationen Sie zum Erstellen und Verwenden von benutzerdefinierten Formatierer für Web-APIs in ASP.NET Core."
ms.author: tdykstra
manager: wpickett
ms.date: 02/08/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/models/custom-formatters
ms.openlocfilehash: 3a6474fdae29b170978226de74d523b20a16cd0c
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/19/2018
---
# <a name="custom-formatters-in-aspnet-core-mvc-web-apis"></a><span data-ttu-id="9938e-103">Benutzerdefinierten Formatierer in ASP.NET Core MVC-Web-APIs</span><span class="sxs-lookup"><span data-stu-id="9938e-103">Custom formatters in ASP.NET Core MVC web APIs</span></span>

<span data-ttu-id="9938e-104">Durch [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="9938e-104">By [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="9938e-105">ASP.NET Core MVC verfügt über integrierte Unterstützung für den Datenaustausch im Web-APIs mit JSON, XML oder nur-Text-Formate.</span><span class="sxs-lookup"><span data-stu-id="9938e-105">ASP.NET Core MVC has built-in support for data exchange in web APIs by using JSON, XML, or plain text formats.</span></span> <span data-ttu-id="9938e-106">In diesem Artikel wird die Unterstützung für zusätzliche Formate hinzufügen, durch das Erstellen von benutzerdefinierten Formatierer veranschaulicht.</span><span class="sxs-lookup"><span data-stu-id="9938e-106">This article shows how to add support for additional formats by creating custom formatters.</span></span>

<span data-ttu-id="9938e-107">[Anzeigen oder Herunterladen des Beispiels aus GitHub](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/advanced/custom-formatters/sample).</span><span class="sxs-lookup"><span data-stu-id="9938e-107">[View or download sample from GitHub](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/advanced/custom-formatters/sample).</span></span>

## <a name="when-to-use-custom-formatters"></a><span data-ttu-id="9938e-108">Verwenden von benutzerdefinierten Formatierer</span><span class="sxs-lookup"><span data-stu-id="9938e-108">When to use custom formatters</span></span>

<span data-ttu-id="9938e-109">Verwenden Sie ein benutzerdefiniertes Formatierungsprogramm aus, wenn Sie möchten die [content-Aushandlung](xref:mvc/models/formatting) Prozess, um einen Inhaltstyp zu unterstützen, die von den integrierten Formatierungsprogrammen (JSON, XML und nur-Text) nicht unterstützt.</span><span class="sxs-lookup"><span data-stu-id="9938e-109">Use a custom formatter when you want the [content negotiation](xref:mvc/models/formatting) process to support a content type that isn't supported by the built-in formatters (JSON, XML, and plain text).</span></span>

<span data-ttu-id="9938e-110">Wenn einige Clients für Ihre Web-API verarbeiten kann z. B. die [Protobuf](https://github.com/google/protobuf) Format, sollten Sie die Protobuf mit diesen Clients zu verwenden, da sie effizienter ist.</span><span class="sxs-lookup"><span data-stu-id="9938e-110">For example, if some of the clients for your web API can handle the [Protobuf](https://github.com/google/protobuf) format, you might want to use Protobuf with those clients because it's more efficient.</span></span>  <span data-ttu-id="9938e-111">Vielleicht möchten Sie Ihre Web-API zum Senden von Kontaktnamen und Adressen in [vCard](https://wikipedia.org/wiki/VCard) Format, eine häufig verwendete Format zum Austauschen von Daten.</span><span class="sxs-lookup"><span data-stu-id="9938e-111">Or you might want your web API to send contact names and addresses in [vCard](https://wikipedia.org/wiki/VCard) format, a commonly used format for exchanging contact data.</span></span> <span data-ttu-id="9938e-112">Die Beispiel-app, die mit diesem Artikel bereitgestellte implementiert einen einfachen vCard-Formatierer.</span><span class="sxs-lookup"><span data-stu-id="9938e-112">The sample app provided with this article implements a simple vCard formatter.</span></span>

## <a name="overview-of-how-to-use-a-custom-formatter"></a><span data-ttu-id="9938e-113">Übersicht darüber, wie ein benutzerdefiniertes Formatierungsprogramm verwenden</span><span class="sxs-lookup"><span data-stu-id="9938e-113">Overview of how to use a custom formatter</span></span>

<span data-ttu-id="9938e-114">Es folgen die Schritte zum Erstellen und verwenden ein benutzerdefiniertes Formatierungsprogramm:</span><span class="sxs-lookup"><span data-stu-id="9938e-114">Here are the steps to create and use a custom formatter:</span></span>

* <span data-ttu-id="9938e-115">Erstellen Sie eine Ausgabe Formatierer-Klasse, wenn Serialisieren von Daten, die an den Client gesendet werden sollen.</span><span class="sxs-lookup"><span data-stu-id="9938e-115">Create an output formatter class if you want to serialize data to send to the client.</span></span>
* <span data-ttu-id="9938e-116">Erstellen Sie eine Eingabe Formatierungsprogrammklasse, wenn beim Deserialisieren von Daten, die vom Client empfangen werden sollen.</span><span class="sxs-lookup"><span data-stu-id="9938e-116">Create an input formatter class if you want to deserialize data received from the client.</span></span> 
* <span data-ttu-id="9938e-117">Fügen Sie Instanzen der Formatierer den `InputFormatters` und `OutputFormatters` Auflistungen in [MvcOptions](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.mvcoptions).</span><span class="sxs-lookup"><span data-stu-id="9938e-117">Add instances of your formatters to the `InputFormatters` and `OutputFormatters` collections in [MvcOptions](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.mvcoptions).</span></span>

<span data-ttu-id="9938e-118">Die folgenden Abschnitte enthalten Anleitungen und Codebeispiele für jede der folgenden Schritte aus.</span><span class="sxs-lookup"><span data-stu-id="9938e-118">The following sections provide guidance and code examples for each of these steps.</span></span>

## <a name="how-to-create-a-custom-formatter-class"></a><span data-ttu-id="9938e-119">Vorgehensweise: erstellen eine benutzerdefinierten Formatierungsklasse</span><span class="sxs-lookup"><span data-stu-id="9938e-119">How to create a custom formatter class</span></span>

<span data-ttu-id="9938e-120">So erstellen Sie einen Formatierer</span><span class="sxs-lookup"><span data-stu-id="9938e-120">To create a formatter:</span></span>

* <span data-ttu-id="9938e-121">Leiten Sie Klasse aus der entsprechenden Basisklasse.</span><span class="sxs-lookup"><span data-stu-id="9938e-121">Derive the class from the appropriate base class.</span></span>
* <span data-ttu-id="9938e-122">Geben Sie gültige Medientypen und Codierungen im Konstruktor.</span><span class="sxs-lookup"><span data-stu-id="9938e-122">Specify valid media types and encodings in the constructor.</span></span>
* <span data-ttu-id="9938e-123">Überschreiben Sie `CanReadType` / `CanWriteType` Methoden</span><span class="sxs-lookup"><span data-stu-id="9938e-123">Override `CanReadType`/`CanWriteType` methods</span></span>
* <span data-ttu-id="9938e-124">Überschreiben Sie `ReadRequestBodyAsync` / `WriteResponseBodyAsync` Methoden</span><span class="sxs-lookup"><span data-stu-id="9938e-124">Override `ReadRequestBodyAsync`/`WriteResponseBodyAsync` methods</span></span>
  
### <a name="derive-from-the-appropriate-base-class"></a><span data-ttu-id="9938e-125">Von der entsprechenden Basisklasse abgeleitet werden</span><span class="sxs-lookup"><span data-stu-id="9938e-125">Derive from the appropriate base class</span></span>

<span data-ttu-id="9938e-126">Text-Medientypen (z. B. vCard), leiten Sie von der [TextInputFormatter](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.formatters.textinputformatter) oder [TextOutputFormatter](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.formatters.textoutputformatter) Basisklasse.</span><span class="sxs-lookup"><span data-stu-id="9938e-126">For text media types (for example, vCard), derive from the [TextInputFormatter](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.formatters.textinputformatter) or [TextOutputFormatter](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.formatters.textoutputformatter) base class.</span></span>

[!code-csharp[Main](custom-formatters/sample/Formatters/VcardOutputFormatter.cs?name=classdef)]

<span data-ttu-id="9938e-127">Leiten Sie für binäre Typen aus den [InputFormatter](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.formatters.inputformatter) oder [OutputFormatter](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.formatters.outputformatter) Basisklasse.</span><span class="sxs-lookup"><span data-stu-id="9938e-127">For binary types, derive from the [InputFormatter](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.formatters.inputformatter) or [OutputFormatter](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.formatters.outputformatter) base class.</span></span>

### <a name="specify-valid-media-types-and-encodings"></a><span data-ttu-id="9938e-128">Geben Sie gültige Medientypen und Codierungen</span><span class="sxs-lookup"><span data-stu-id="9938e-128">Specify valid media types and encodings</span></span>

<span data-ttu-id="9938e-129">Geben Sie im Konstruktor gültige Medientypen und Codierungen durch Hinzufügen der `SupportedMediaTypes` und `SupportedEncodings` Sammlungen.</span><span class="sxs-lookup"><span data-stu-id="9938e-129">In the constructor, specify valid media types and encodings by adding to the `SupportedMediaTypes` and `SupportedEncodings` collections.</span></span>

[!code-csharp[Main](custom-formatters/sample/Formatters/VcardOutputFormatter.cs?name=ctor&highlight=3,5-6)]

> [!NOTE]  
> <span data-ttu-id="9938e-130">Abhängigkeitsinjektion in einer Formatierungsklasse Konstruktor ist nicht möglich.</span><span class="sxs-lookup"><span data-stu-id="9938e-130">You can't do constructor dependency injection in a formatter class.</span></span> <span data-ttu-id="9938e-131">Beispielsweise kann keine Protokollierung durch Hinzufügen eines Parameters für die Protokollierung an den Konstruktor nicht abgerufen werden.</span><span class="sxs-lookup"><span data-stu-id="9938e-131">For example, you can't get a logger by adding a logger parameter to the constructor.</span></span> <span data-ttu-id="9938e-132">Um auf Dienste zuzugreifen, müssen Sie das Context-Objekt verwenden, das in Ihrer Methoden übergeben werden.</span><span class="sxs-lookup"><span data-stu-id="9938e-132">To access services, you have to use the context object that gets passed in to your methods.</span></span> <span data-ttu-id="9938e-133">Ein Codebeispiel hierfür [unten](#read-write) veranschaulicht dies.</span><span class="sxs-lookup"><span data-stu-id="9938e-133">A code example [below](#read-write) shows how to do this.</span></span>

### <a name="override-canreadtypecanwritetype"></a><span data-ttu-id="9938e-134">Überschreiben Sie CanReadType/CanWriteType</span><span class="sxs-lookup"><span data-stu-id="9938e-134">Override CanReadType/CanWriteType</span></span> 

<span data-ttu-id="9938e-135">Geben Sie den können in deserialisiert oder Serialisieren von durch Überschreiben der `CanReadType` oder `CanWriteType` Methoden.</span><span class="sxs-lookup"><span data-stu-id="9938e-135">Specify the type you can deserialize into or serialize from by overriding the `CanReadType` or `CanWriteType` methods.</span></span> <span data-ttu-id="9938e-136">Beispielsweise nur möglicherweise können zum Erstellen von vCard Text aus einer `Contact` Typ und umgekehrt.</span><span class="sxs-lookup"><span data-stu-id="9938e-136">For example, you might only be able to create vCard text from a `Contact` type and vice versa.</span></span>

[!code-csharp[Main](custom-formatters/sample/Formatters/VcardOutputFormatter.cs?name=canwritetype)]

#### <a name="the-canwriteresult-method"></a><span data-ttu-id="9938e-137">Die CanWriteResult-Methode</span><span class="sxs-lookup"><span data-stu-id="9938e-137">The CanWriteResult method</span></span>

<span data-ttu-id="9938e-138">In einigen Szenarien müssen Sie außer Kraft setzen `CanWriteResult` anstelle von `CanWriteType`.</span><span class="sxs-lookup"><span data-stu-id="9938e-138">In some scenarios you have to override `CanWriteResult` instead of `CanWriteType`.</span></span> <span data-ttu-id="9938e-139">Verwendung `CanWriteResult` wenn Folgendes zutrifft:</span><span class="sxs-lookup"><span data-stu-id="9938e-139">Use `CanWriteResult` if the following conditions are true:</span></span>

  * <span data-ttu-id="9938e-140">Die Aktionsmethode gibt eine Modellklasse zurück.</span><span class="sxs-lookup"><span data-stu-id="9938e-140">Your action method returns a model class.</span></span>
  * <span data-ttu-id="9938e-141">Es sind abgeleitete Klassen, die zur Laufzeit zurückgegeben werden können.</span><span class="sxs-lookup"><span data-stu-id="9938e-141">There are derived classes which might be returned at runtime.</span></span>
  * <span data-ttu-id="9938e-142">Sie müssen wissen zur Laufzeit die abgeleitete Klasse wird von der Aktion zurückgegeben wurde.</span><span class="sxs-lookup"><span data-stu-id="9938e-142">You need to know at runtime which derived class was returned by the action.</span></span>  

<span data-ttu-id="9938e-143">Nehmen wir beispielsweise an Ihre Aktion Methodensignatur gibt eine `Person` aufweisen, aber es gelegten eine `Student` oder `Instructor` aus abgeleiteter Typ `Person`.</span><span class="sxs-lookup"><span data-stu-id="9938e-143">For example, suppose your action method signature returns a `Person` type, but it may return a `Student` or `Instructor` type that derives from `Person`.</span></span> <span data-ttu-id="9938e-144">Wenn der Formatierer, nur behandeln soll `Student` Objekte, überprüfen Sie den Typ des [Objekt](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.formatters.outputformattercanwritecontext#Microsoft_AspNetCore_Mvc_Formatters_OutputFormatterCanWriteContext_Object) in das bereitgestellte Kontextobjekt der `CanWriteResult` Methode.</span><span class="sxs-lookup"><span data-stu-id="9938e-144">If you want your formatter to handle only `Student` objects, check the type of [Object](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.formatters.outputformattercanwritecontext#Microsoft_AspNetCore_Mvc_Formatters_OutputFormatterCanWriteContext_Object) in the context object provided to the `CanWriteResult` method.</span></span> <span data-ttu-id="9938e-145">Beachten Sie, dass es nicht notwendig, verwenden Sie `CanWriteResult` bei Rückgabe der Aktionsmethode `IActionResult`; in diesem Fall die `CanWriteType` Methode empfängt den Laufzeittyp.</span><span class="sxs-lookup"><span data-stu-id="9938e-145">Note that it's not necessary to use `CanWriteResult` when the action method returns `IActionResult`; in that case, the `CanWriteType` method receives the runtime type.</span></span>

<a id="read-write"></a>
### <a name="override-readrequestbodyasyncwriteresponsebodyasync"></a><span data-ttu-id="9938e-146">Override ReadRequestBodyAsync/WriteResponseBodyAsync</span><span class="sxs-lookup"><span data-stu-id="9938e-146">Override ReadRequestBodyAsync/WriteResponseBodyAsync</span></span> 

<span data-ttu-id="9938e-147">Führen Sie den eigentlichen deserialisiert oder Serialisieren `ReadRequestBodyAsync` oder `WriteResponseBodyAsync`.</span><span class="sxs-lookup"><span data-stu-id="9938e-147">You do the actual work of deserializing or serializing in `ReadRequestBodyAsync` or `WriteResponseBodyAsync`.</span></span>  <span data-ttu-id="9938e-148">Die hervorgehobenen Zeilen im folgenden Beispiel werden veranschaulicht Services von der abhängigkeitseinschleusungscontainer abrufen (Sie können nicht aus Konstruktorparameter sie erhalten).</span><span class="sxs-lookup"><span data-stu-id="9938e-148">The highlighted lines in the following example show how to get services from the dependency injection container (you can't get them from constructor parameters).</span></span>

[!code-csharp[Main](custom-formatters/sample/Formatters/VcardOutputFormatter.cs?name=writeresponse&highlight=3-4)]

## <a name="how-to-configure-mvc-to-use-a-custom-formatter"></a><span data-ttu-id="9938e-149">Konfigurieren von MVC, um ein benutzerdefiniertes Formatierungsprogramm verwenden</span><span class="sxs-lookup"><span data-stu-id="9938e-149">How to configure MVC to use a custom formatter</span></span>
 
<span data-ttu-id="9938e-150">Um ein benutzerdefiniertes Formatierungsprogramm verwenden, fügen Sie eine Instanz der Klasse Formatierer in die `InputFormatters` oder `OutputFormatters` Auflistung.</span><span class="sxs-lookup"><span data-stu-id="9938e-150">To use a custom formatter, add an instance of the formatter class to the `InputFormatters` or `OutputFormatters` collection.</span></span>

[!code-csharp[Main](custom-formatters/sample/Startup.cs?name=mvcoptions&highlight=3-4)]

<span data-ttu-id="9938e-151">Formatierungsprogramme werden ausgewertet, in der Reihenfolge an, dass Sie sie einfügen.</span><span class="sxs-lookup"><span data-stu-id="9938e-151">Formatters are evaluated in the order you insert them.</span></span> <span data-ttu-id="9938e-152">Die erste Vorlage hat Vorrang vor.</span><span class="sxs-lookup"><span data-stu-id="9938e-152">The first one takes precedence.</span></span> 

## <a name="next-steps"></a><span data-ttu-id="9938e-153">Nächste Schritte</span><span class="sxs-lookup"><span data-stu-id="9938e-153">Next steps</span></span>

<span data-ttu-id="9938e-154">Finden Sie unter der [beispielanwendung](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/advanced/custom-formatters/sample), implementiert einfache vCard Eingabe und Ausgabe-Formatierer.</span><span class="sxs-lookup"><span data-stu-id="9938e-154">See the [sample application](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/advanced/custom-formatters/sample), which implements simple vCard input and output formatters.</span></span>  <span data-ttu-id="9938e-155">Die Anwendung liest und schreibt vCards, die wie im folgenden Beispiel aussehen:</span><span class="sxs-lookup"><span data-stu-id="9938e-155">The application reads and writes vCards that look like the following example:</span></span>

```
BEGIN:VCARD
VERSION:2.1
N:Davolio;Nancy
FN:Nancy Davolio
UID:20293482-9240-4d68-b475-325df4a83728
END:VCARD
```

<span data-ttu-id="9938e-156">Sehen vCard auszugeben, führen Sie die Anwendung und senden eine Get-Anforderung mit Accept-Header "Text/Vcard" `http://localhost:63313/api/contacts/` (Wenn in Visual Studio ausgeführt wird) oder `http://localhost:5000/api/contacts/` (wenn von der Befehlszeile aus ausgeführt wird).</span><span class="sxs-lookup"><span data-stu-id="9938e-156">To see vCard output, run the application and send a Get request with Accept header "text/vcard" to `http://localhost:63313/api/contacts/` (when running from Visual Studio) or `http://localhost:5000/api/contacts/` (when running from the command line).</span></span>

<span data-ttu-id="9938e-157">Um die in-Memory-Sammlung von Kontakten vCard hinzugefügt haben, senden Sie eine Post-Anforderung zur gleichen URL, mit der Content-Type-Header "Text/Vcard" und vCard Text im Hauptteil, wie im Beispiel oben formatiert.</span><span class="sxs-lookup"><span data-stu-id="9938e-157">To add a vCard to the in-memory collection of contacts, send a Post request to the same URL, with Content-Type header "text/vcard" and with vCard text in the body, formatted like the example above.</span></span>
