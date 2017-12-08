---
uid: web-api/overview/formats-and-model-binding/json-and-xml-serialization
title: JSON und XML-Serialisierung in ASP.NET Web-API | Microsoft Docs
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/30/2012
ms.topic: article
ms.assetid: 1cd7525d-de5e-4ab6-94f0-51480d3255d1
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/formats-and-model-binding/json-and-xml-serialization
msc.type: authoredcontent
ms.openlocfilehash: 7aafe4823d3a6090fae4a63f1a66fb2670ecb025
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="json-and-xml-serialization-in-aspnet-web-api"></a><span data-ttu-id="f8ecf-102">JSON und XML-Serialisierung in ASP.NET Web-API</span><span class="sxs-lookup"><span data-stu-id="f8ecf-102">JSON and XML Serialization in ASP.NET Web API</span></span>
====================
<span data-ttu-id="f8ecf-103">durch [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="f8ecf-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="f8ecf-104">Dieser Artikel beschreibt die JSON und XML-Formatierer in ASP.NET Web-API.</span><span class="sxs-lookup"><span data-stu-id="f8ecf-104">This article describes the JSON and XML formatters in ASP.NET Web API.</span></span>

<span data-ttu-id="f8ecf-105">In ASP.NET Web-API eine *medientypformatierer* ist ein Objekt, das haben kann:</span><span class="sxs-lookup"><span data-stu-id="f8ecf-105">In ASP.NET Web API, a *media-type formatter* is an object that can:</span></span>

- <span data-ttu-id="f8ecf-106">Read-CLR-Objekte aus einem HTTP-Nachrichtentext</span><span class="sxs-lookup"><span data-stu-id="f8ecf-106">Read CLR objects from an HTTP message body</span></span>
- <span data-ttu-id="f8ecf-107">Schreiben von CLR-Objekte in einer HTTP-Nachrichtentexts</span><span class="sxs-lookup"><span data-stu-id="f8ecf-107">Write CLR objects into an HTTP message body</span></span>

<span data-ttu-id="f8ecf-108">Web-API bietet die medientypformatierer für JSON und XML.</span><span class="sxs-lookup"><span data-stu-id="f8ecf-108">Web API provides media-type formatters for both JSON and XML.</span></span> <span data-ttu-id="f8ecf-109">Das Framework fügt diese Formatierer in die Pipeline in der Standardeinstellung.</span><span class="sxs-lookup"><span data-stu-id="f8ecf-109">The framework inserts these formatters into the pipeline by default.</span></span> <span data-ttu-id="f8ecf-110">Clients können JSON oder XML im Accept-Header der HTTP-Anforderung anfordern.</span><span class="sxs-lookup"><span data-stu-id="f8ecf-110">Clients can request either JSON or XML in the Accept header of the HTTP request.</span></span>

## <a name="contents"></a><span data-ttu-id="f8ecf-111">Inhalt</span><span class="sxs-lookup"><span data-stu-id="f8ecf-111">Contents</span></span>

- [<span data-ttu-id="f8ecf-112">JSON-Medientypformatierer</span><span class="sxs-lookup"><span data-stu-id="f8ecf-112">JSON Media-Type Formatter</span></span>](#json_media_type_formatter)

    - [<span data-ttu-id="f8ecf-113">Schreibschutzeigenschaften</span><span class="sxs-lookup"><span data-stu-id="f8ecf-113">Read-Only Properties</span></span>](#json_readonly)
    - [<span data-ttu-id="f8ecf-114">Datumsangaben</span><span class="sxs-lookup"><span data-stu-id="f8ecf-114">Dates</span></span>](#json_dates)
    - [<span data-ttu-id="f8ecf-115">Festlegen von Einzügen</span><span class="sxs-lookup"><span data-stu-id="f8ecf-115">Indenting</span></span>](#json_indenting)
    - [<span data-ttu-id="f8ecf-116">Kamel-Schreibweise</span><span class="sxs-lookup"><span data-stu-id="f8ecf-116">Camel Casing</span></span>](#json_camelcasing)
    - [<span data-ttu-id="f8ecf-117">Anonyme als auch schwach typisierte Objekte</span><span class="sxs-lookup"><span data-stu-id="f8ecf-117">Anonymous and Weakly-Typed Objects</span></span>](#json_anon)
- [<span data-ttu-id="f8ecf-118">XML-Medientypformatierer</span><span class="sxs-lookup"><span data-stu-id="f8ecf-118">XML Media-Type Formatter</span></span>](#xml_media_type_formatter)

    - [<span data-ttu-id="f8ecf-119">Schreibschutzeigenschaften</span><span class="sxs-lookup"><span data-stu-id="f8ecf-119">Read-Only Properties</span></span>](#xml_readonly)
    - [<span data-ttu-id="f8ecf-120">Datumsangaben</span><span class="sxs-lookup"><span data-stu-id="f8ecf-120">Dates</span></span>](#xml_dates)
    - [<span data-ttu-id="f8ecf-121">Festlegen von Einzügen</span><span class="sxs-lookup"><span data-stu-id="f8ecf-121">Indenting</span></span>](#xml_indenting)
    - [<span data-ttu-id="f8ecf-122">Festlegen von pro-Type-XML-Serialisierungsprogrammen</span><span class="sxs-lookup"><span data-stu-id="f8ecf-122">Setting Per-Type XML Serializers</span></span>](#xml_pertype)
- [<span data-ttu-id="f8ecf-123">Entfernen des JSON oder XML-Formatierer</span><span class="sxs-lookup"><span data-stu-id="f8ecf-123">Removing the JSON or XML Formatter</span></span>](#removing_the_json_or_xml_formatter)
- [<span data-ttu-id="f8ecf-124">Behandlung von zirkuläre Objektverweise</span><span class="sxs-lookup"><span data-stu-id="f8ecf-124">Handling Circular Object References</span></span>](#handling_circular_object_references)
- [<span data-ttu-id="f8ecf-125">Testen die Serialisierung von Objekten</span><span class="sxs-lookup"><span data-stu-id="f8ecf-125">Testing Object Serialization</span></span>](#testing_object_serialization)

<a id="json_media_type_formatter"></a>
## <a name="json-media-type-formatter"></a><span data-ttu-id="f8ecf-126">JSON-Medientypformatierer</span><span class="sxs-lookup"><span data-stu-id="f8ecf-126">JSON Media-Type Formatter</span></span>

<span data-ttu-id="f8ecf-127">JSON-Formatierung bereitgestellt wird, durch die **JsonMediaTypeFormatter** Klasse.</span><span class="sxs-lookup"><span data-stu-id="f8ecf-127">JSON formatting is provided by the **JsonMediaTypeFormatter** class.</span></span> <span data-ttu-id="f8ecf-128">Standardmäßig **JsonMediaTypeFormatter** verwendet die [Json.NET](https://github.com/JamesNK/Newtonsoft.Json) Bibliothek, die Serialisierungen durchführen.</span><span class="sxs-lookup"><span data-stu-id="f8ecf-128">By default, **JsonMediaTypeFormatter** uses the [Json.NET](https://github.com/JamesNK/Newtonsoft.Json) library to perform serialization.</span></span> <span data-ttu-id="f8ecf-129">Json.NET ist ein Drittanbieter-open-Source-Projekt.</span><span class="sxs-lookup"><span data-stu-id="f8ecf-129">Json.NET is a third-party open source project.</span></span>

<span data-ttu-id="f8ecf-130">Wenn Sie dies bevorzugen, können Sie konfigurieren die **JsonMediaTypeFormatter** -Klasse die **DataContractJsonSerializer** anstelle von Json.NET.</span><span class="sxs-lookup"><span data-stu-id="f8ecf-130">If you prefer, you can configure the **JsonMediaTypeFormatter** class to use the **DataContractJsonSerializer** instead of Json.NET.</span></span> <span data-ttu-id="f8ecf-131">Legen Sie hierzu die **UseDataContractJsonSerializer** Eigenschaft **"true"**:</span><span class="sxs-lookup"><span data-stu-id="f8ecf-131">To do so, set the **UseDataContractJsonSerializer** property to **true**:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample1.cs)]

### <a name="json-serialization"></a><span data-ttu-id="f8ecf-132">JSON-Serialisierung</span><span class="sxs-lookup"><span data-stu-id="f8ecf-132">JSON Serialization</span></span>

<span data-ttu-id="f8ecf-133">Dieser Abschnitt beschreibt einige bestimmten Verhaltensweisen von der JSON-Formatierer, die unter Verwendung des standardmäßigen [Json.NET](https://github.com/JamesNK/Newtonsoft.Json) Serialisierungsprogramm.</span><span class="sxs-lookup"><span data-stu-id="f8ecf-133">This section describes some specific behaviors of the JSON formatter, using the default [Json.NET](https://github.com/JamesNK/Newtonsoft.Json) serializer.</span></span> <span data-ttu-id="f8ecf-134">Dies ist nicht vorgesehen, eine umfassende Dokumentation der Bibliothek Json.NET sein; Weitere Informationen finden Sie unter der [Json.NET Dokumentation](http://james.newtonking.com/projects/json/help/).</span><span class="sxs-lookup"><span data-stu-id="f8ecf-134">This is not meant to be comprehensive documentation of the Json.NET library; for more information, see the [Json.NET Documentation](http://james.newtonking.com/projects/json/help/).</span></span>

#### <a name="what-gets-serialized"></a><span data-ttu-id="f8ecf-135">Was serialisiert werden?</span><span class="sxs-lookup"><span data-stu-id="f8ecf-135">What Gets Serialized?</span></span>

<span data-ttu-id="f8ecf-136">Standardmäßig sind alle öffentlichen Eigenschaften und Felder in der JSON-serialisierten enthalten.</span><span class="sxs-lookup"><span data-stu-id="f8ecf-136">By default, all public properties and fields are included in the serialized JSON.</span></span> <span data-ttu-id="f8ecf-137">Um eine Eigenschaft oder ein Feld zu unterdrücken, ergänzen sie die **JsonIgnore** Attribut.</span><span class="sxs-lookup"><span data-stu-id="f8ecf-137">To omit a property or field, decorate it with the **JsonIgnore** attribute.</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample2.cs)]

<span data-ttu-id="f8ecf-138">Falls gewünscht ein &quot;teilnehmen&quot; Ansatz, ergänzen Sie die Klasse mit dem **DataContract** Attribut.</span><span class="sxs-lookup"><span data-stu-id="f8ecf-138">If you prefer an &quot;opt-in&quot; approach, decorate the class with the **DataContract** attribute.</span></span> <span data-ttu-id="f8ecf-139">Wenn dieses Attribut vorhanden ist, werden Elemente ignoriert, es sei denn, sie haben die **DataMember**.</span><span class="sxs-lookup"><span data-stu-id="f8ecf-139">If this attribute is present, members are ignored unless they have the **DataMember**.</span></span> <span data-ttu-id="f8ecf-140">Sie können auch **DataMember** Private Member zu serialisieren.</span><span class="sxs-lookup"><span data-stu-id="f8ecf-140">You can also use **DataMember** to serialize private members.</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample3.cs)]

<a id="json_readonly"></a>
### <a name="read-only-properties"></a><span data-ttu-id="f8ecf-141">Schreibschutzeigenschaften</span><span class="sxs-lookup"><span data-stu-id="f8ecf-141">Read-Only Properties</span></span>

<span data-ttu-id="f8ecf-142">Schreibgeschützte Eigenschaften werden standardmäßig serialisiert.</span><span class="sxs-lookup"><span data-stu-id="f8ecf-142">Read-only properties are serialized by default.</span></span>

<a id="json_dates"></a>
### <a name="dates"></a><span data-ttu-id="f8ecf-143">Datumsangaben</span><span class="sxs-lookup"><span data-stu-id="f8ecf-143">Dates</span></span>

<span data-ttu-id="f8ecf-144">Standardmäßig schreibt Json.NET Datumsangaben in [ISO 8601](http://www.w3.org/TR/NOTE-datetime) Format.</span><span class="sxs-lookup"><span data-stu-id="f8ecf-144">By default, Json.NET writes dates in [ISO 8601](http://www.w3.org/TR/NOTE-datetime) format.</span></span> <span data-ttu-id="f8ecf-145">Datumsangaben, UTC (Coordinated Universal Time) werden mit dem Suffix "Z" geschrieben.</span><span class="sxs-lookup"><span data-stu-id="f8ecf-145">Dates in UTC (Coordinated Universal Time) are written with a "Z" suffix.</span></span> <span data-ttu-id="f8ecf-146">Datumsangaben in Ortszeit enthalten einen Zeitzonenoffset.</span><span class="sxs-lookup"><span data-stu-id="f8ecf-146">Dates in local time include a time-zone offset.</span></span> <span data-ttu-id="f8ecf-147">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="f8ecf-147">For example:</span></span>

[!code-console[Main](json-and-xml-serialization/samples/sample4.cmd)]

<span data-ttu-id="f8ecf-148">Standardmäßig behält Json.NET der Zeitzone.</span><span class="sxs-lookup"><span data-stu-id="f8ecf-148">By default, Json.NET preserves the time zone.</span></span> <span data-ttu-id="f8ecf-149">Dies kann durch Festlegen der Eigenschaft DateTimeZoneHandling überschrieben werden:</span><span class="sxs-lookup"><span data-stu-id="f8ecf-149">You can override this by setting the DateTimeZoneHandling property:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample5.cs)]

<span data-ttu-id="f8ecf-150">Wenn Sie es vorziehen, verwenden [Microsoft JSON-Datumsformat](https://msdn.microsoft.com/en-us/library/bb299886.aspx#intro_to_json_sidebarb) (`"\/Date(ticks)\/"`) statt ISO 8601, setzen die **DateFormatHandling** Eigenschaft auf die serialisierereinstellungen:</span><span class="sxs-lookup"><span data-stu-id="f8ecf-150">If you prefer to use [Microsoft JSON date format](https://msdn.microsoft.com/en-us/library/bb299886.aspx#intro_to_json_sidebarb) (`"\/Date(ticks)\/"`) instead of ISO 8601, set the **DateFormatHandling** property on the serializer settings:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample6.cs)]

<a id="json_indenting"></a>
### <a name="indenting"></a><span data-ttu-id="f8ecf-151">Einzug</span><span class="sxs-lookup"><span data-stu-id="f8ecf-151">Indenting</span></span>

<span data-ttu-id="f8ecf-152">Um eingezogen JSON zu schreiben, legen die **Formatierung** auf **Formatting.Indented**:</span><span class="sxs-lookup"><span data-stu-id="f8ecf-152">To write indented JSON, set the **Formatting** setting to **Formatting.Indented**:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample7.cs)]

<a id="json_camelcasing"></a>
### <a name="camel-casing"></a><span data-ttu-id="f8ecf-153">Kamel-Schreibweise</span><span class="sxs-lookup"><span data-stu-id="f8ecf-153">Camel Casing</span></span>

<span data-ttu-id="f8ecf-154">Um JSON-Eigenschaftennamen mit Kamel-Schreibweise, schreiben, ohne Ihr Datenmodell ändern, legen die **CamelCasePropertyNamesContractResolver** auf das Serialisierungsprogramm:</span><span class="sxs-lookup"><span data-stu-id="f8ecf-154">To write JSON property names with camel casing, without changing your data model, set the **CamelCasePropertyNamesContractResolver** on the serializer:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample8.cs)]

<a id="json_anon"></a>
### <a name="anonymous-and-weakly-typed-objects"></a><span data-ttu-id="f8ecf-155">Anonyme als auch schwach typisierte Objekte</span><span class="sxs-lookup"><span data-stu-id="f8ecf-155">Anonymous and Weakly-Typed Objects</span></span>

<span data-ttu-id="f8ecf-156">Eine Aktionsmethode kann ein anonymes Objekt zurückgeben und deren Serialisierung in JSON.</span><span class="sxs-lookup"><span data-stu-id="f8ecf-156">An action method can return an anonymous object and serialize it to JSON.</span></span> <span data-ttu-id="f8ecf-157">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="f8ecf-157">For example:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample9.cs)]

<span data-ttu-id="f8ecf-158">Der Nachrichtentext der Antwort enthält die folgenden JSON-Code:</span><span class="sxs-lookup"><span data-stu-id="f8ecf-158">The response message body will contain the following JSON:</span></span>

[!code-json[Main](json-and-xml-serialization/samples/sample10.json)]

<span data-ttu-id="f8ecf-159">Wenn Ihre Web-API-lose empfängt JSON-Objekten von den Clients strukturiert sind, können Sie der Anforderungstext zum Deserialisieren einer **Newtonsoft.Json.Linq.JObject** Typ.</span><span class="sxs-lookup"><span data-stu-id="f8ecf-159">If your web API receives loosely structured JSON objects from clients, you can deserialize the request body to a **Newtonsoft.Json.Linq.JObject** type.</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample11.cs)]

<span data-ttu-id="f8ecf-160">Es ist jedoch in der Regel besser, stark typisierte Datenobjekte verwenden.</span><span class="sxs-lookup"><span data-stu-id="f8ecf-160">However, it is usually better to use strongly typed data objects.</span></span> <span data-ttu-id="f8ecf-161">Klicken Sie dann brauchen Sie die Daten zu analysieren, und die Vorteile der modellvalidierung erhalten.</span><span class="sxs-lookup"><span data-stu-id="f8ecf-161">Then you don't need to parse the data yourself, and you get the benefits of model validation.</span></span>

<span data-ttu-id="f8ecf-162">Das XML-Serialisierungsprogramm unterstützt keine anonyme Typen oder **JObject** Instanzen.</span><span class="sxs-lookup"><span data-stu-id="f8ecf-162">The XML serializer does not support anonymous types or **JObject** instances.</span></span> <span data-ttu-id="f8ecf-163">Wenn Sie diese Funktionen für die JSON-Daten verwenden, sollten Sie die XML-Formatierer aus der Pipeline entfernen, wie weiter unten in diesem Artikel beschrieben.</span><span class="sxs-lookup"><span data-stu-id="f8ecf-163">If you use these features for your JSON data, you should remove the XML formatter from the pipeline, as described later in this article.</span></span>

<a id="xml_media_type_formatter"></a>
## <a name="xml-media-type-formatter"></a><span data-ttu-id="f8ecf-164">XML-Medientypformatierer</span><span class="sxs-lookup"><span data-stu-id="f8ecf-164">XML Media-Type Formatter</span></span>

<span data-ttu-id="f8ecf-165">XML-Formatierung wird bereitgestellt, indem Sie die **XmlMediaTypeFormatter** Klasse.</span><span class="sxs-lookup"><span data-stu-id="f8ecf-165">XML formatting is provided by the **XmlMediaTypeFormatter** class.</span></span> <span data-ttu-id="f8ecf-166">Standardmäßig **XmlMediaTypeFormatter** verwendet die **"DataContractSerializer"** Klasse, die Serialisierungen durchführen.</span><span class="sxs-lookup"><span data-stu-id="f8ecf-166">By default, **XmlMediaTypeFormatter** uses the **DataContractSerializer** class to perform serialization.</span></span>

<span data-ttu-id="f8ecf-167">Falls gewünscht, können Sie konfigurieren die **XmlMediaTypeFormatter** verwenden die **XmlSerializer** anstelle von der **"DataContractSerializer"**.</span><span class="sxs-lookup"><span data-stu-id="f8ecf-167">If you prefer, you can configure the **XmlMediaTypeFormatter** to use the **XmlSerializer** instead of the **DataContractSerializer**.</span></span> <span data-ttu-id="f8ecf-168">Legen Sie hierzu die **UseXmlSerializer** Eigenschaft **"true"**:</span><span class="sxs-lookup"><span data-stu-id="f8ecf-168">To do so, set the **UseXmlSerializer** property to **true**:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample12.cs)]

<span data-ttu-id="f8ecf-169">Die **XmlSerializer** Klasse unterstützt einen engeren Satz von Typen als **"DataContractSerializer"**, bietet jedoch mehr Kontrolle über die resultierenden XML-Daten.</span><span class="sxs-lookup"><span data-stu-id="f8ecf-169">The **XmlSerializer** class supports a narrower set of types than **DataContractSerializer**, but gives more control over the resulting XML.</span></span> <span data-ttu-id="f8ecf-170">Erwägen Sie **XmlSerializer** Wenn Sie ein vorhandenes XML-Schema entsprechen müssen.</span><span class="sxs-lookup"><span data-stu-id="f8ecf-170">Consider using **XmlSerializer** if you need to match an existing XML schema.</span></span>

### <a name="xml-serialization"></a><span data-ttu-id="f8ecf-171">XML-Serialisierung</span><span class="sxs-lookup"><span data-stu-id="f8ecf-171">XML Serialization</span></span>

<span data-ttu-id="f8ecf-172">Dieser Abschnitt beschreibt einige bestimmten Verhaltensweisen von der XML-Formatierer, der unter Verwendung des standardmäßigen **"DataContractSerializer"**.</span><span class="sxs-lookup"><span data-stu-id="f8ecf-172">This section describes some specific behaviors of the XML formatter, using the default **DataContractSerializer**.</span></span>

<span data-ttu-id="f8ecf-173">Standardmäßig verhält sich "DataContractSerializer" wie folgt:</span><span class="sxs-lookup"><span data-stu-id="f8ecf-173">By default, the DataContractSerializer behaves as follows:</span></span>

- <span data-ttu-id="f8ecf-174">Alle öffentlichen Lese-/Schreibeigenschaften und Felder serialisiert werden.</span><span class="sxs-lookup"><span data-stu-id="f8ecf-174">All public read/write properties and fields are serialized.</span></span> <span data-ttu-id="f8ecf-175">Um eine Eigenschaft oder ein Feld zu unterdrücken, ergänzen sie die **IgnoreDataMember** Attribut.</span><span class="sxs-lookup"><span data-stu-id="f8ecf-175">To omit a property or field, decorate it with the **IgnoreDataMember** attribute.</span></span>
- <span data-ttu-id="f8ecf-176">Private und geschützte Member werden nicht serialisiert.</span><span class="sxs-lookup"><span data-stu-id="f8ecf-176">Private and protected members are not serialized.</span></span>
- <span data-ttu-id="f8ecf-177">Schreibgeschützte Eigenschaften werden nicht serialisiert.</span><span class="sxs-lookup"><span data-stu-id="f8ecf-177">Read-only properties are not serialized.</span></span> <span data-ttu-id="f8ecf-178">(Allerdings werden der Inhalt einer schreibgeschützten Auflistung Eigenschaft serialisiert.)</span><span class="sxs-lookup"><span data-stu-id="f8ecf-178">(However, the contents of a read-only collection property are serialized.)</span></span>
- <span data-ttu-id="f8ecf-179">Klassen- und Membernamen werden in der XML-Code geschrieben werden, genau so, wie sie in der Klassendeklaration angezeigt werden.</span><span class="sxs-lookup"><span data-stu-id="f8ecf-179">Class and member names are written in the XML exactly as they appear in the class declaration.</span></span>
- <span data-ttu-id="f8ecf-180">Ein XML-Standardnamespace wird verwendet.</span><span class="sxs-lookup"><span data-stu-id="f8ecf-180">A default XML namespace is used.</span></span>

<span data-ttu-id="f8ecf-181">Wenn Sie mehr Kontrolle über die Serialisierung benötigen, können Sie die Klasse mit ergänzen die **DataContract** Attribut.</span><span class="sxs-lookup"><span data-stu-id="f8ecf-181">If you need more control over the serialization, you can decorate the class with the **DataContract** attribute.</span></span> <span data-ttu-id="f8ecf-182">Wenn dieses Attribut vorhanden ist, wird die Klasse wie folgt serialisiert:</span><span class="sxs-lookup"><span data-stu-id="f8ecf-182">When this attribute is present, the class is serialized as follows:</span></span>

- <span data-ttu-id="f8ecf-183">&quot;Abonnieren von&quot; Ansatz: Eigenschaften und Felder werden standardmäßig nicht serialisiert.</span><span class="sxs-lookup"><span data-stu-id="f8ecf-183">&quot;Opt in&quot; approach: Properties and fields are not serialized by default.</span></span> <span data-ttu-id="f8ecf-184">Um eine Eigenschaft oder ein Feld zu serialisieren, ergänzen sie die **DataMember** Attribut.</span><span class="sxs-lookup"><span data-stu-id="f8ecf-184">To serialize a property or field, decorate it with the **DataMember** attribute.</span></span>
- <span data-ttu-id="f8ecf-185">Um eine private oder geschützte Member zu serialisieren, ergänzen sie die **DataMember** Attribut.</span><span class="sxs-lookup"><span data-stu-id="f8ecf-185">To serialize a private or protected member, decorate it with the **DataMember** attribute.</span></span>
- <span data-ttu-id="f8ecf-186">Schreibgeschützte Eigenschaften werden nicht serialisiert.</span><span class="sxs-lookup"><span data-stu-id="f8ecf-186">Read-only properties are not serialized.</span></span>
- <span data-ttu-id="f8ecf-187">Um ändern, wie der Name der Klasse in der XML-Code angezeigt wird, legen Sie die *Namen* Parameter in der **DataContract** Attribut.</span><span class="sxs-lookup"><span data-stu-id="f8ecf-187">To change how the class name appears in the XML, set the *Name* parameter in the **DataContract** attribute.</span></span>
- <span data-ttu-id="f8ecf-188">Legen Sie zum Ändern der Darstellung der eines Elementnamen in der XML-Code die *Namen* Parameter in der **DataMember** Attribut.</span><span class="sxs-lookup"><span data-stu-id="f8ecf-188">To change how a member name appears in the XML, set the *Name* parameter in the **DataMember** attribute.</span></span>
- <span data-ttu-id="f8ecf-189">Um den XML-Namespace zu ändern, legen die *Namespace* Parameter in der **DataContract** Klasse.</span><span class="sxs-lookup"><span data-stu-id="f8ecf-189">To change the XML namespace, set the *Namespace* parameter in the **DataContract** class.</span></span>

<a id="xml_readonly"></a>
### <a name="read-only-properties"></a><span data-ttu-id="f8ecf-190">Schreibschutzeigenschaften</span><span class="sxs-lookup"><span data-stu-id="f8ecf-190">Read-Only Properties</span></span>

<span data-ttu-id="f8ecf-191">Schreibgeschützte Eigenschaften werden nicht serialisiert.</span><span class="sxs-lookup"><span data-stu-id="f8ecf-191">Read-only properties are not serialized.</span></span> <span data-ttu-id="f8ecf-192">Wenn eine schreibgeschützte Eigenschaft ein private dahinter liegende Feld verfügt, können, markieren Sie das private Feld mit der **DataMember** Attribut.</span><span class="sxs-lookup"><span data-stu-id="f8ecf-192">If a read-only property has a backing private field, you can mark the private field with the **DataMember** attribute.</span></span> <span data-ttu-id="f8ecf-193">Dieser Ansatz erfordert das **DataContract** -Attribut für die Klasse.</span><span class="sxs-lookup"><span data-stu-id="f8ecf-193">This approach requires the **DataContract** attribute on the class.</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample13.cs)]

<a id="xml_dates"></a>
### <a name="dates"></a><span data-ttu-id="f8ecf-194">Datumsangaben</span><span class="sxs-lookup"><span data-stu-id="f8ecf-194">Dates</span></span>

<span data-ttu-id="f8ecf-195">Datumsangaben werden im ISO 8601-Format geschrieben.</span><span class="sxs-lookup"><span data-stu-id="f8ecf-195">Dates are written in ISO 8601 format.</span></span> <span data-ttu-id="f8ecf-196">Beispielsweise &quot;2012-05-23T20:21:37.9116538Z&quot;.</span><span class="sxs-lookup"><span data-stu-id="f8ecf-196">For example, &quot;2012-05-23T20:21:37.9116538Z&quot;.</span></span>

<a id="xml_indenting"></a>
### <a name="indenting"></a><span data-ttu-id="f8ecf-197">Einzug</span><span class="sxs-lookup"><span data-stu-id="f8ecf-197">Indenting</span></span>

<span data-ttu-id="f8ecf-198">Um eingezogene XML zu schreiben, legen die **Einzug** Eigenschaft **"true"**:</span><span class="sxs-lookup"><span data-stu-id="f8ecf-198">To write indented XML, set the **Indent** property to **true**:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample14.cs)]

<a id="xml_pertype"></a>
## <a name="setting-per-type-xml-serializers"></a><span data-ttu-id="f8ecf-199">Festlegen von pro-Type-XML-Serialisierungsprogrammen</span><span class="sxs-lookup"><span data-stu-id="f8ecf-199">Setting Per-Type XML Serializers</span></span>

<span data-ttu-id="f8ecf-200">Sie können verschiedene XML-Serialisierer für andere CLR-Typen festlegen.</span><span class="sxs-lookup"><span data-stu-id="f8ecf-200">You can set different XML serializers for different CLR types.</span></span> <span data-ttu-id="f8ecf-201">Angenommen, Sie müssen möglicherweise ein bestimmtes Datenobjekt, das erfordert **XmlSerializer** Gründen der Abwärtskompatibilität.</span><span class="sxs-lookup"><span data-stu-id="f8ecf-201">For example, you might have a particular data object that requires **XmlSerializer** for backward compatibility.</span></span> <span data-ttu-id="f8ecf-202">Sie können **XmlSerializer** für dieses Objekt aus, und verwenden weiterhin **"DataContractSerializer"** für andere Typen.</span><span class="sxs-lookup"><span data-stu-id="f8ecf-202">You can use **XmlSerializer** for this object and continue to use **DataContractSerializer** for other types.</span></span>

<span data-ttu-id="f8ecf-203">Rufen Sie zum Festlegen einer XML-Serialisierungsprogramm für einen bestimmten Typ **SetSerializer**.</span><span class="sxs-lookup"><span data-stu-id="f8ecf-203">To set an XML serializer for a particular type, call **SetSerializer**.</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample15.cs)]

<span data-ttu-id="f8ecf-204">Sie können angeben, ein **XmlSerializer** oder jedes Objekt, das von abgeleitet ist **XmlObjectSerializer**.</span><span class="sxs-lookup"><span data-stu-id="f8ecf-204">You can specify an **XmlSerializer** or any object that derives from **XmlObjectSerializer**.</span></span>

<a id="removing_the_json_or_xml_formatter"></a>
## <a name="removing-the-json-or-xml-formatter"></a><span data-ttu-id="f8ecf-205">Entfernen des JSON oder XML-Formatierer</span><span class="sxs-lookup"><span data-stu-id="f8ecf-205">Removing the JSON or XML Formatter</span></span>

<span data-ttu-id="f8ecf-206">Sie können die JSON-Formatierungsprogramm oder der XML-Formatierer aus der Liste der Formatierer, entfernen, wenn Sie nicht verwenden möchten.</span><span class="sxs-lookup"><span data-stu-id="f8ecf-206">You can remove the JSON formatter or the XML formatter from the list of formatters, if you do not want to use them.</span></span> <span data-ttu-id="f8ecf-207">Die Gründe hierfür sind:</span><span class="sxs-lookup"><span data-stu-id="f8ecf-207">The main reasons to do this are:</span></span>

- <span data-ttu-id="f8ecf-208">Beschränken Sie Ihre Web-API-Antworten auf einen bestimmten Medientyp.</span><span class="sxs-lookup"><span data-stu-id="f8ecf-208">To restrict your web API responses to a particular media type.</span></span> <span data-ttu-id="f8ecf-209">Sie könnten z. B. nur JSON-Antworten zu unterstützen, und entfernen die XML-Formatierer.</span><span class="sxs-lookup"><span data-stu-id="f8ecf-209">For example, you might decide to support only JSON responses, and remove the XML formatter.</span></span>
- <span data-ttu-id="f8ecf-210">Der Standardformatierer durch ein benutzerdefiniertes Formatierungsprogramm ersetzen.</span><span class="sxs-lookup"><span data-stu-id="f8ecf-210">To replace the default formatter with a custom formatter.</span></span> <span data-ttu-id="f8ecf-211">Beispielsweise konnte durch eine eigene benutzerdefinierte Implementierung einer JSON-Formatierung den JSON-Formatierer ersetzt werden.</span><span class="sxs-lookup"><span data-stu-id="f8ecf-211">For example, you could replace the JSON formatter with your own custom implementation of a JSON formatter.</span></span>

<span data-ttu-id="f8ecf-212">Der folgende Code zeigt, wie Sie die Standard-Formatierer entfernen.</span><span class="sxs-lookup"><span data-stu-id="f8ecf-212">The following code shows how to remove the default formatters.</span></span> <span data-ttu-id="f8ecf-213">Rufen Sie diese von Ihrem **Anwendung\_starten** Methode, die in "Global.asax" definiert.</span><span class="sxs-lookup"><span data-stu-id="f8ecf-213">Call this from your **Application\_Start** method, defined in Global.asax.</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample16.cs)]

<a id="handling_circular_object_references"></a>
## <a name="handling-circular-object-references"></a><span data-ttu-id="f8ecf-214">Behandlung von zirkuläre Objektverweise</span><span class="sxs-lookup"><span data-stu-id="f8ecf-214">Handling Circular Object References</span></span>

<span data-ttu-id="f8ecf-215">Die JSON- und XML-Formatierer Schreiben aller Objekte als Werte.</span><span class="sxs-lookup"><span data-stu-id="f8ecf-215">By default, the JSON and XML formatters write all objects as values.</span></span> <span data-ttu-id="f8ecf-216">Wenn zwei Eigenschaften auf dasselbe Objekt verweisen oder das gleiche Objekt zweimal in einer Auflistung angezeigt wird, wird das Formatierungsprogramm Serialisieren des Objekts zweimal.</span><span class="sxs-lookup"><span data-stu-id="f8ecf-216">If two properties refer to the same object, or if the same object appears twice in a collection, the formatter will serialize the object twice.</span></span> <span data-ttu-id="f8ecf-217">Dies ist ein bestimmtes Problem enthält Ihre Objektdiagramm Zyklen, da das Serialisierungsprogramm eine Ausnahme auslöst, wenn eine Schleife im Diagramm erkannt wird.</span><span class="sxs-lookup"><span data-stu-id="f8ecf-217">This is a particular problem if your object graph contains cycles, because the serializer will throw an exception when it detects a loop in the graph.</span></span>

<span data-ttu-id="f8ecf-218">Betrachten Sie den folgenden Objektmodellen und Controller.</span><span class="sxs-lookup"><span data-stu-id="f8ecf-218">Consider the following object models and controller.</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample17.cs)]

<span data-ttu-id="f8ecf-219">Diese Aktion wird das Formatierungsprogramm löst das Aufrufen eine Ausnahme aus, das in einem Status Code 500 (Interner Serverfehler) Antwort an den Client übersetzt ausgelöst.</span><span class="sxs-lookup"><span data-stu-id="f8ecf-219">Invoking this action will cause the formatter to thrown an exception, which translates to a status code 500 (Internal Server Error) response to the client.</span></span>

<span data-ttu-id="f8ecf-220">Um Objektverweise im JSON-Format zu erhalten, fügen Sie den folgenden Code zum **Anwendung\_starten** Methode in der Datei "Global.asax":</span><span class="sxs-lookup"><span data-stu-id="f8ecf-220">To preserve object references in JSON, add the following code to **Application\_Start** method in the Global.asax file:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample18.cs)]

<span data-ttu-id="f8ecf-221">Jetzt wird der Controlleraktion JSON zurück, die wie folgt aussieht:</span><span class="sxs-lookup"><span data-stu-id="f8ecf-221">Now the controller action will return JSON that looks like this:</span></span>

[!code-json[Main](json-and-xml-serialization/samples/sample19.json)]

<span data-ttu-id="f8ecf-222">Beachten Sie, die das Serialisierungsprogramm fügt eine &quot;$id&quot; Eigenschaft, um beide Objekte.</span><span class="sxs-lookup"><span data-stu-id="f8ecf-222">Notice that the serializer adds an &quot;$id&quot; property to both objects.</span></span> <span data-ttu-id="f8ecf-223">Erkennt außerdem, dass die Employee.Department-Eigenschaft eine Schleife erstellt, sodass es den Wert durch einen Objektverweis ersetzt: {&quot;$ref&quot;:&quot;1&quot;}.</span><span class="sxs-lookup"><span data-stu-id="f8ecf-223">Also, it detects that the Employee.Department property creates a loop, so it replaces the value with an object reference: {&quot;$ref&quot;:&quot;1&quot;}.</span></span>

> [!NOTE]
> <span data-ttu-id="f8ecf-224">Objektverweise sind nicht in JSON standard.</span><span class="sxs-lookup"><span data-stu-id="f8ecf-224">Object references are not standard in JSON.</span></span> <span data-ttu-id="f8ecf-225">Beachten Sie, ob die Clients die Ergebnisse analysieren können, bevor Sie mit dieser Funktion.</span><span class="sxs-lookup"><span data-stu-id="f8ecf-225">Before using this feature, consider whether your clients will be able to parse the results.</span></span> <span data-ttu-id="f8ecf-226">Es möglicherweise besser einfach, Zyklen aus dem Diagramm zu entfernen.</span><span class="sxs-lookup"><span data-stu-id="f8ecf-226">It might be better simply to remove cycles from the graph.</span></span> <span data-ttu-id="f8ecf-227">In diesem Beispiel wird die Verknüpfung zwischen Mitarbeiter an Abteilung z. B. wirklich nicht erforderlich.</span><span class="sxs-lookup"><span data-stu-id="f8ecf-227">For example, the link from Employee back to Department is not really needed in this example.</span></span>


<span data-ttu-id="f8ecf-228">Um Objektverweise im XML-Format zu erhalten, müssen Sie zwei Optionen zur Verfügung.</span><span class="sxs-lookup"><span data-stu-id="f8ecf-228">To preserve object references in XML, you have two options.</span></span> <span data-ttu-id="f8ecf-229">Die einfachere Möglichkeit besteht darin hinzufügen `[DataContract(IsReference=true)]` der Modell-Klasse.</span><span class="sxs-lookup"><span data-stu-id="f8ecf-229">The simpler option is to add `[DataContract(IsReference=true)]` to your model class.</span></span> <span data-ttu-id="f8ecf-230">Die *IsReference* Parameter ermöglicht Objektverweise.</span><span class="sxs-lookup"><span data-stu-id="f8ecf-230">The *IsReference* parameter enables object references.</span></span> <span data-ttu-id="f8ecf-231">Beachten Sie, dass **DataContract** Serialisierung teilnehmen können, stellt, daher Sie auch hinzufügen müssen **DataMember** -Attribute verwenden, um die Eigenschaften:</span><span class="sxs-lookup"><span data-stu-id="f8ecf-231">Remember that **DataContract** makes serialization opt-in, so you will also need to add **DataMember** attributes to the properties:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample20.cs)]

<span data-ttu-id="f8ecf-232">Nun das Formatierungsprogramm XML-Code ähnelt generiert, die folgenden:</span><span class="sxs-lookup"><span data-stu-id="f8ecf-232">Now the formatter will produce XML similar to following:</span></span>

[!code-xml[Main](json-and-xml-serialization/samples/sample21.xml)]

<span data-ttu-id="f8ecf-233">Wenn Sie Attribute in Ihrer Modellklasse vermeiden möchten, ist es eine weitere Option: Erstellen einer neuen typspezifische **"DataContractSerializer"** -Instanz, und legen Sie *PreserveObjectReferences* zu **"true"**  im Konstruktor.</span><span class="sxs-lookup"><span data-stu-id="f8ecf-233">If you want to avoid attributes on your model class, there is another option: Create a new type-specific **DataContractSerializer** instance and set *preserveObjectReferences* to **true** in the constructor.</span></span> <span data-ttu-id="f8ecf-234">Legen Sie dann diese Instanz als ein Serialisierungsprogramm pro Typ, auf die XML-medientypformatierer.</span><span class="sxs-lookup"><span data-stu-id="f8ecf-234">Then set this instance as a per-type serializer on the XML media-type formatter.</span></span> <span data-ttu-id="f8ecf-235">Der folgende Code zeigt, wie dies:</span><span class="sxs-lookup"><span data-stu-id="f8ecf-235">The following code show how to do this:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample22.cs?highlight=3)]

<a id="testing_object_serialization"></a>
## <a name="testing-object-serialization"></a><span data-ttu-id="f8ecf-236">Testen die Serialisierung von Objekten</span><span class="sxs-lookup"><span data-stu-id="f8ecf-236">Testing Object Serialization</span></span>

<span data-ttu-id="f8ecf-237">Wie Sie Ihre Web-API entworfen haben, ist es hilfreich, testen, wie Ihre Datenobjekte serialisiert werden.</span><span class="sxs-lookup"><span data-stu-id="f8ecf-237">As you design your web API, it is useful to test how your data objects will be serialized.</span></span> <span data-ttu-id="f8ecf-238">Dies ist möglich, ohne Erstellen eines Domänencontrollers oder eine Controlleraktion aufgerufen.</span><span class="sxs-lookup"><span data-stu-id="f8ecf-238">You can do this without creating a controller or invoking a controller action.</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample23.cs)]
