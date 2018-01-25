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
ms.openlocfilehash: b1fcaf70cc38d73da0a454764520197b97f34b26
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/24/2018
---
<a name="json-and-xml-serialization-in-aspnet-web-api"></a><span data-ttu-id="d051f-102">JSON und XML-Serialisierung in ASP.NET Web-API</span><span class="sxs-lookup"><span data-stu-id="d051f-102">JSON and XML Serialization in ASP.NET Web API</span></span>
====================
<span data-ttu-id="d051f-103">durch [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="d051f-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="d051f-104">Dieser Artikel beschreibt die JSON und XML-Formatierer in ASP.NET Web-API.</span><span class="sxs-lookup"><span data-stu-id="d051f-104">This article describes the JSON and XML formatters in ASP.NET Web API.</span></span>

<span data-ttu-id="d051f-105">In ASP.NET Web-API eine *medientypformatierer* ist ein Objekt, das haben kann:</span><span class="sxs-lookup"><span data-stu-id="d051f-105">In ASP.NET Web API, a *media-type formatter* is an object that can:</span></span>

- <span data-ttu-id="d051f-106">Read-CLR-Objekte aus einem HTTP-Nachrichtentext</span><span class="sxs-lookup"><span data-stu-id="d051f-106">Read CLR objects from an HTTP message body</span></span>
- <span data-ttu-id="d051f-107">Schreiben von CLR-Objekte in einer HTTP-Nachrichtentexts</span><span class="sxs-lookup"><span data-stu-id="d051f-107">Write CLR objects into an HTTP message body</span></span>

<span data-ttu-id="d051f-108">Web-API bietet die medientypformatierer für JSON und XML.</span><span class="sxs-lookup"><span data-stu-id="d051f-108">Web API provides media-type formatters for both JSON and XML.</span></span> <span data-ttu-id="d051f-109">Das Framework fügt diese Formatierer in die Pipeline in der Standardeinstellung.</span><span class="sxs-lookup"><span data-stu-id="d051f-109">The framework inserts these formatters into the pipeline by default.</span></span> <span data-ttu-id="d051f-110">Clients können JSON oder XML im Accept-Header der HTTP-Anforderung anfordern.</span><span class="sxs-lookup"><span data-stu-id="d051f-110">Clients can request either JSON or XML in the Accept header of the HTTP request.</span></span>

## <a name="contents"></a><span data-ttu-id="d051f-111">Inhalt</span><span class="sxs-lookup"><span data-stu-id="d051f-111">Contents</span></span>

- [<span data-ttu-id="d051f-112">JSON-Medientypformatierer</span><span class="sxs-lookup"><span data-stu-id="d051f-112">JSON Media-Type Formatter</span></span>](#json_media_type_formatter)

    - [<span data-ttu-id="d051f-113">Schreibschutzeigenschaften</span><span class="sxs-lookup"><span data-stu-id="d051f-113">Read-Only Properties</span></span>](#json_readonly)
    - [<span data-ttu-id="d051f-114">Datumsangaben</span><span class="sxs-lookup"><span data-stu-id="d051f-114">Dates</span></span>](#json_dates)
    - [<span data-ttu-id="d051f-115">Festlegen von Einzügen</span><span class="sxs-lookup"><span data-stu-id="d051f-115">Indenting</span></span>](#json_indenting)
    - [<span data-ttu-id="d051f-116">Kamel-Schreibweise</span><span class="sxs-lookup"><span data-stu-id="d051f-116">Camel Casing</span></span>](#json_camelcasing)
    - [<span data-ttu-id="d051f-117">Anonyme als auch schwach typisierte Objekte</span><span class="sxs-lookup"><span data-stu-id="d051f-117">Anonymous and Weakly-Typed Objects</span></span>](#json_anon)
- [<span data-ttu-id="d051f-118">XML-Medientypformatierer</span><span class="sxs-lookup"><span data-stu-id="d051f-118">XML Media-Type Formatter</span></span>](#xml_media_type_formatter)

    - [<span data-ttu-id="d051f-119">Schreibschutzeigenschaften</span><span class="sxs-lookup"><span data-stu-id="d051f-119">Read-Only Properties</span></span>](#xml_readonly)
    - [<span data-ttu-id="d051f-120">Datumsangaben</span><span class="sxs-lookup"><span data-stu-id="d051f-120">Dates</span></span>](#xml_dates)
    - [<span data-ttu-id="d051f-121">Festlegen von Einzügen</span><span class="sxs-lookup"><span data-stu-id="d051f-121">Indenting</span></span>](#xml_indenting)
    - [<span data-ttu-id="d051f-122">Festlegen von pro-Type-XML-Serialisierungsprogrammen</span><span class="sxs-lookup"><span data-stu-id="d051f-122">Setting Per-Type XML Serializers</span></span>](#xml_pertype)
- [<span data-ttu-id="d051f-123">Entfernen des JSON oder XML-Formatierer</span><span class="sxs-lookup"><span data-stu-id="d051f-123">Removing the JSON or XML Formatter</span></span>](#removing_the_json_or_xml_formatter)
- [<span data-ttu-id="d051f-124">Behandlung von zirkuläre Objektverweise</span><span class="sxs-lookup"><span data-stu-id="d051f-124">Handling Circular Object References</span></span>](#handling_circular_object_references)
- [<span data-ttu-id="d051f-125">Testen die Serialisierung von Objekten</span><span class="sxs-lookup"><span data-stu-id="d051f-125">Testing Object Serialization</span></span>](#testing_object_serialization)

<a id="json_media_type_formatter"></a>
## <a name="json-media-type-formatter"></a><span data-ttu-id="d051f-126">JSON-Medientypformatierer</span><span class="sxs-lookup"><span data-stu-id="d051f-126">JSON Media-Type Formatter</span></span>

<span data-ttu-id="d051f-127">JSON-Formatierung bereitgestellt wird, durch die **JsonMediaTypeFormatter** Klasse.</span><span class="sxs-lookup"><span data-stu-id="d051f-127">JSON formatting is provided by the **JsonMediaTypeFormatter** class.</span></span> <span data-ttu-id="d051f-128">Standardmäßig **JsonMediaTypeFormatter** verwendet die [Json.NET](https://github.com/JamesNK/Newtonsoft.Json) Bibliothek, die Serialisierungen durchführen.</span><span class="sxs-lookup"><span data-stu-id="d051f-128">By default, **JsonMediaTypeFormatter** uses the [Json.NET](https://github.com/JamesNK/Newtonsoft.Json) library to perform serialization.</span></span> <span data-ttu-id="d051f-129">Json.NET ist ein Drittanbieter-open-Source-Projekt.</span><span class="sxs-lookup"><span data-stu-id="d051f-129">Json.NET is a third-party open source project.</span></span>

<span data-ttu-id="d051f-130">Wenn Sie dies bevorzugen, können Sie konfigurieren die **JsonMediaTypeFormatter** -Klasse die **DataContractJsonSerializer** anstelle von Json.NET.</span><span class="sxs-lookup"><span data-stu-id="d051f-130">If you prefer, you can configure the **JsonMediaTypeFormatter** class to use the **DataContractJsonSerializer** instead of Json.NET.</span></span> <span data-ttu-id="d051f-131">Legen Sie hierzu die **UseDataContractJsonSerializer** Eigenschaft **"true"**:</span><span class="sxs-lookup"><span data-stu-id="d051f-131">To do so, set the **UseDataContractJsonSerializer** property to **true**:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample1.cs)]

### <a name="json-serialization"></a><span data-ttu-id="d051f-132">JSON-Serialisierung</span><span class="sxs-lookup"><span data-stu-id="d051f-132">JSON Serialization</span></span>

<span data-ttu-id="d051f-133">Dieser Abschnitt beschreibt einige bestimmten Verhaltensweisen von der JSON-Formatierer, die unter Verwendung des standardmäßigen [Json.NET](https://github.com/JamesNK/Newtonsoft.Json) Serialisierungsprogramm.</span><span class="sxs-lookup"><span data-stu-id="d051f-133">This section describes some specific behaviors of the JSON formatter, using the default [Json.NET](https://github.com/JamesNK/Newtonsoft.Json) serializer.</span></span> <span data-ttu-id="d051f-134">Dies ist nicht vorgesehen, eine umfassende Dokumentation der Bibliothek Json.NET sein; Weitere Informationen finden Sie unter der [Json.NET Dokumentation](http://james.newtonking.com/projects/json/help/).</span><span class="sxs-lookup"><span data-stu-id="d051f-134">This is not meant to be comprehensive documentation of the Json.NET library; for more information, see the [Json.NET Documentation](http://james.newtonking.com/projects/json/help/).</span></span>

#### <a name="what-gets-serialized"></a><span data-ttu-id="d051f-135">Was serialisiert werden?</span><span class="sxs-lookup"><span data-stu-id="d051f-135">What Gets Serialized?</span></span>

<span data-ttu-id="d051f-136">Standardmäßig sind alle öffentlichen Eigenschaften und Felder in der JSON-serialisierten enthalten.</span><span class="sxs-lookup"><span data-stu-id="d051f-136">By default, all public properties and fields are included in the serialized JSON.</span></span> <span data-ttu-id="d051f-137">Um eine Eigenschaft oder ein Feld zu unterdrücken, ergänzen sie die **JsonIgnore** Attribut.</span><span class="sxs-lookup"><span data-stu-id="d051f-137">To omit a property or field, decorate it with the **JsonIgnore** attribute.</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample2.cs)]

<span data-ttu-id="d051f-138">Falls gewünscht ein &quot;teilnehmen&quot; Ansatz, ergänzen Sie die Klasse mit dem **DataContract** Attribut.</span><span class="sxs-lookup"><span data-stu-id="d051f-138">If you prefer an &quot;opt-in&quot; approach, decorate the class with the **DataContract** attribute.</span></span> <span data-ttu-id="d051f-139">Wenn dieses Attribut vorhanden ist, werden Elemente ignoriert, es sei denn, sie haben die **DataMember**.</span><span class="sxs-lookup"><span data-stu-id="d051f-139">If this attribute is present, members are ignored unless they have the **DataMember**.</span></span> <span data-ttu-id="d051f-140">Sie können auch **DataMember** Private Member zu serialisieren.</span><span class="sxs-lookup"><span data-stu-id="d051f-140">You can also use **DataMember** to serialize private members.</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample3.cs)]

<a id="json_readonly"></a>
### <a name="read-only-properties"></a><span data-ttu-id="d051f-141">Schreibschutzeigenschaften</span><span class="sxs-lookup"><span data-stu-id="d051f-141">Read-Only Properties</span></span>

<span data-ttu-id="d051f-142">Schreibgeschützte Eigenschaften werden standardmäßig serialisiert.</span><span class="sxs-lookup"><span data-stu-id="d051f-142">Read-only properties are serialized by default.</span></span>

<a id="json_dates"></a>
### <a name="dates"></a><span data-ttu-id="d051f-143">Datumsangaben</span><span class="sxs-lookup"><span data-stu-id="d051f-143">Dates</span></span>

<span data-ttu-id="d051f-144">Standardmäßig schreibt Json.NET Datumsangaben in [ISO 8601](http://www.w3.org/TR/NOTE-datetime) Format.</span><span class="sxs-lookup"><span data-stu-id="d051f-144">By default, Json.NET writes dates in [ISO 8601](http://www.w3.org/TR/NOTE-datetime) format.</span></span> <span data-ttu-id="d051f-145">Datumsangaben, UTC (Coordinated Universal Time) werden mit dem Suffix "Z" geschrieben.</span><span class="sxs-lookup"><span data-stu-id="d051f-145">Dates in UTC (Coordinated Universal Time) are written with a "Z" suffix.</span></span> <span data-ttu-id="d051f-146">Datumsangaben in Ortszeit enthalten einen Zeitzonenoffset.</span><span class="sxs-lookup"><span data-stu-id="d051f-146">Dates in local time include a time-zone offset.</span></span> <span data-ttu-id="d051f-147">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="d051f-147">For example:</span></span>

[!code-console[Main](json-and-xml-serialization/samples/sample4.cmd)]

<span data-ttu-id="d051f-148">Standardmäßig behält Json.NET der Zeitzone.</span><span class="sxs-lookup"><span data-stu-id="d051f-148">By default, Json.NET preserves the time zone.</span></span> <span data-ttu-id="d051f-149">Dies kann durch Festlegen der Eigenschaft DateTimeZoneHandling überschrieben werden:</span><span class="sxs-lookup"><span data-stu-id="d051f-149">You can override this by setting the DateTimeZoneHandling property:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample5.cs)]

<span data-ttu-id="d051f-150">Wenn Sie es vorziehen, verwenden [Microsoft JSON-Datumsformat](https://msdn.microsoft.com/library/bb299886.aspx#intro_to_json_sidebarb) (`"\/Date(ticks)\/"`) statt ISO 8601, setzen die **DateFormatHandling** Eigenschaft auf die serialisierereinstellungen:</span><span class="sxs-lookup"><span data-stu-id="d051f-150">If you prefer to use [Microsoft JSON date format](https://msdn.microsoft.com/library/bb299886.aspx#intro_to_json_sidebarb) (`"\/Date(ticks)\/"`) instead of ISO 8601, set the **DateFormatHandling** property on the serializer settings:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample6.cs)]

<a id="json_indenting"></a>
### <a name="indenting"></a><span data-ttu-id="d051f-151">Einzug</span><span class="sxs-lookup"><span data-stu-id="d051f-151">Indenting</span></span>

<span data-ttu-id="d051f-152">Um eingezogen JSON zu schreiben, legen die **Formatierung** auf **Formatting.Indented**:</span><span class="sxs-lookup"><span data-stu-id="d051f-152">To write indented JSON, set the **Formatting** setting to **Formatting.Indented**:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample7.cs)]

<a id="json_camelcasing"></a>
### <a name="camel-casing"></a><span data-ttu-id="d051f-153">Kamel-Schreibweise</span><span class="sxs-lookup"><span data-stu-id="d051f-153">Camel Casing</span></span>

<span data-ttu-id="d051f-154">Um JSON-Eigenschaftennamen mit Kamel-Schreibweise, schreiben, ohne Ihr Datenmodell ändern, legen die **CamelCasePropertyNamesContractResolver** auf das Serialisierungsprogramm:</span><span class="sxs-lookup"><span data-stu-id="d051f-154">To write JSON property names with camel casing, without changing your data model, set the **CamelCasePropertyNamesContractResolver** on the serializer:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample8.cs)]

<a id="json_anon"></a>
### <a name="anonymous-and-weakly-typed-objects"></a><span data-ttu-id="d051f-155">Anonyme als auch schwach typisierte Objekte</span><span class="sxs-lookup"><span data-stu-id="d051f-155">Anonymous and Weakly-Typed Objects</span></span>

<span data-ttu-id="d051f-156">Eine Aktionsmethode kann ein anonymes Objekt zurückgeben und deren Serialisierung in JSON.</span><span class="sxs-lookup"><span data-stu-id="d051f-156">An action method can return an anonymous object and serialize it to JSON.</span></span> <span data-ttu-id="d051f-157">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="d051f-157">For example:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample9.cs)]

<span data-ttu-id="d051f-158">Der Nachrichtentext der Antwort enthält die folgenden JSON-Code:</span><span class="sxs-lookup"><span data-stu-id="d051f-158">The response message body will contain the following JSON:</span></span>

[!code-json[Main](json-and-xml-serialization/samples/sample10.json)]

<span data-ttu-id="d051f-159">Wenn Ihre Web-API-lose empfängt JSON-Objekten von den Clients strukturiert sind, können Sie der Anforderungstext zum Deserialisieren einer **Newtonsoft.Json.Linq.JObject** Typ.</span><span class="sxs-lookup"><span data-stu-id="d051f-159">If your web API receives loosely structured JSON objects from clients, you can deserialize the request body to a **Newtonsoft.Json.Linq.JObject** type.</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample11.cs)]

<span data-ttu-id="d051f-160">Es ist jedoch in der Regel besser, stark typisierte Datenobjekte verwenden.</span><span class="sxs-lookup"><span data-stu-id="d051f-160">However, it is usually better to use strongly typed data objects.</span></span> <span data-ttu-id="d051f-161">Klicken Sie dann brauchen Sie die Daten zu analysieren, und die Vorteile der modellvalidierung erhalten.</span><span class="sxs-lookup"><span data-stu-id="d051f-161">Then you don't need to parse the data yourself, and you get the benefits of model validation.</span></span>

<span data-ttu-id="d051f-162">Das XML-Serialisierungsprogramm unterstützt keine anonyme Typen oder **JObject** Instanzen.</span><span class="sxs-lookup"><span data-stu-id="d051f-162">The XML serializer does not support anonymous types or **JObject** instances.</span></span> <span data-ttu-id="d051f-163">Wenn Sie diese Funktionen für die JSON-Daten verwenden, sollten Sie die XML-Formatierer aus der Pipeline entfernen, wie weiter unten in diesem Artikel beschrieben.</span><span class="sxs-lookup"><span data-stu-id="d051f-163">If you use these features for your JSON data, you should remove the XML formatter from the pipeline, as described later in this article.</span></span>

<a id="xml_media_type_formatter"></a>
## <a name="xml-media-type-formatter"></a><span data-ttu-id="d051f-164">XML-Medientypformatierer</span><span class="sxs-lookup"><span data-stu-id="d051f-164">XML Media-Type Formatter</span></span>

<span data-ttu-id="d051f-165">XML-Formatierung wird bereitgestellt, indem Sie die **XmlMediaTypeFormatter** Klasse.</span><span class="sxs-lookup"><span data-stu-id="d051f-165">XML formatting is provided by the **XmlMediaTypeFormatter** class.</span></span> <span data-ttu-id="d051f-166">Standardmäßig **XmlMediaTypeFormatter** verwendet die **"DataContractSerializer"** Klasse, die Serialisierungen durchführen.</span><span class="sxs-lookup"><span data-stu-id="d051f-166">By default, **XmlMediaTypeFormatter** uses the **DataContractSerializer** class to perform serialization.</span></span>

<span data-ttu-id="d051f-167">Falls gewünscht, können Sie konfigurieren die **XmlMediaTypeFormatter** verwenden die **XmlSerializer** anstelle von der **"DataContractSerializer"**.</span><span class="sxs-lookup"><span data-stu-id="d051f-167">If you prefer, you can configure the **XmlMediaTypeFormatter** to use the **XmlSerializer** instead of the **DataContractSerializer**.</span></span> <span data-ttu-id="d051f-168">Legen Sie hierzu die **UseXmlSerializer** Eigenschaft **"true"**:</span><span class="sxs-lookup"><span data-stu-id="d051f-168">To do so, set the **UseXmlSerializer** property to **true**:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample12.cs)]

<span data-ttu-id="d051f-169">Die **XmlSerializer** Klasse unterstützt einen engeren Satz von Typen als **"DataContractSerializer"**, bietet jedoch mehr Kontrolle über die resultierenden XML-Daten.</span><span class="sxs-lookup"><span data-stu-id="d051f-169">The **XmlSerializer** class supports a narrower set of types than **DataContractSerializer**, but gives more control over the resulting XML.</span></span> <span data-ttu-id="d051f-170">Erwägen Sie **XmlSerializer** Wenn Sie ein vorhandenes XML-Schema entsprechen müssen.</span><span class="sxs-lookup"><span data-stu-id="d051f-170">Consider using **XmlSerializer** if you need to match an existing XML schema.</span></span>

### <a name="xml-serialization"></a><span data-ttu-id="d051f-171">XML-Serialisierung</span><span class="sxs-lookup"><span data-stu-id="d051f-171">XML Serialization</span></span>

<span data-ttu-id="d051f-172">Dieser Abschnitt beschreibt einige bestimmten Verhaltensweisen von der XML-Formatierer, der unter Verwendung des standardmäßigen **"DataContractSerializer"**.</span><span class="sxs-lookup"><span data-stu-id="d051f-172">This section describes some specific behaviors of the XML formatter, using the default **DataContractSerializer**.</span></span>

<span data-ttu-id="d051f-173">Standardmäßig verhält sich "DataContractSerializer" wie folgt:</span><span class="sxs-lookup"><span data-stu-id="d051f-173">By default, the DataContractSerializer behaves as follows:</span></span>

- <span data-ttu-id="d051f-174">Alle öffentlichen Lese-/Schreibeigenschaften und Felder serialisiert werden.</span><span class="sxs-lookup"><span data-stu-id="d051f-174">All public read/write properties and fields are serialized.</span></span> <span data-ttu-id="d051f-175">Um eine Eigenschaft oder ein Feld zu unterdrücken, ergänzen sie die **IgnoreDataMember** Attribut.</span><span class="sxs-lookup"><span data-stu-id="d051f-175">To omit a property or field, decorate it with the **IgnoreDataMember** attribute.</span></span>
- <span data-ttu-id="d051f-176">Private und geschützte Member werden nicht serialisiert.</span><span class="sxs-lookup"><span data-stu-id="d051f-176">Private and protected members are not serialized.</span></span>
- <span data-ttu-id="d051f-177">Schreibgeschützte Eigenschaften werden nicht serialisiert.</span><span class="sxs-lookup"><span data-stu-id="d051f-177">Read-only properties are not serialized.</span></span> <span data-ttu-id="d051f-178">(Allerdings werden der Inhalt einer schreibgeschützten Auflistung Eigenschaft serialisiert.)</span><span class="sxs-lookup"><span data-stu-id="d051f-178">(However, the contents of a read-only collection property are serialized.)</span></span>
- <span data-ttu-id="d051f-179">Klassen- und Membernamen werden in der XML-Code geschrieben werden, genau so, wie sie in der Klassendeklaration angezeigt werden.</span><span class="sxs-lookup"><span data-stu-id="d051f-179">Class and member names are written in the XML exactly as they appear in the class declaration.</span></span>
- <span data-ttu-id="d051f-180">Ein XML-Standardnamespace wird verwendet.</span><span class="sxs-lookup"><span data-stu-id="d051f-180">A default XML namespace is used.</span></span>

<span data-ttu-id="d051f-181">Wenn Sie mehr Kontrolle über die Serialisierung benötigen, können Sie die Klasse mit ergänzen die **DataContract** Attribut.</span><span class="sxs-lookup"><span data-stu-id="d051f-181">If you need more control over the serialization, you can decorate the class with the **DataContract** attribute.</span></span> <span data-ttu-id="d051f-182">Wenn dieses Attribut vorhanden ist, wird die Klasse wie folgt serialisiert:</span><span class="sxs-lookup"><span data-stu-id="d051f-182">When this attribute is present, the class is serialized as follows:</span></span>

- <span data-ttu-id="d051f-183">&quot;Abonnieren von&quot; Ansatz: Eigenschaften und Felder werden standardmäßig nicht serialisiert.</span><span class="sxs-lookup"><span data-stu-id="d051f-183">&quot;Opt in&quot; approach: Properties and fields are not serialized by default.</span></span> <span data-ttu-id="d051f-184">Um eine Eigenschaft oder ein Feld zu serialisieren, ergänzen sie die **DataMember** Attribut.</span><span class="sxs-lookup"><span data-stu-id="d051f-184">To serialize a property or field, decorate it with the **DataMember** attribute.</span></span>
- <span data-ttu-id="d051f-185">Um eine private oder geschützte Member zu serialisieren, ergänzen sie die **DataMember** Attribut.</span><span class="sxs-lookup"><span data-stu-id="d051f-185">To serialize a private or protected member, decorate it with the **DataMember** attribute.</span></span>
- <span data-ttu-id="d051f-186">Schreibgeschützte Eigenschaften werden nicht serialisiert.</span><span class="sxs-lookup"><span data-stu-id="d051f-186">Read-only properties are not serialized.</span></span>
- <span data-ttu-id="d051f-187">Um ändern, wie der Name der Klasse in der XML-Code angezeigt wird, legen Sie die *Namen* Parameter in der **DataContract** Attribut.</span><span class="sxs-lookup"><span data-stu-id="d051f-187">To change how the class name appears in the XML, set the *Name* parameter in the **DataContract** attribute.</span></span>
- <span data-ttu-id="d051f-188">Legen Sie zum Ändern der Darstellung der eines Elementnamen in der XML-Code die *Namen* Parameter in der **DataMember** Attribut.</span><span class="sxs-lookup"><span data-stu-id="d051f-188">To change how a member name appears in the XML, set the *Name* parameter in the **DataMember** attribute.</span></span>
- <span data-ttu-id="d051f-189">Um den XML-Namespace zu ändern, legen die *Namespace* Parameter in der **DataContract** Klasse.</span><span class="sxs-lookup"><span data-stu-id="d051f-189">To change the XML namespace, set the *Namespace* parameter in the **DataContract** class.</span></span>

<a id="xml_readonly"></a>
### <a name="read-only-properties"></a><span data-ttu-id="d051f-190">Schreibschutzeigenschaften</span><span class="sxs-lookup"><span data-stu-id="d051f-190">Read-Only Properties</span></span>

<span data-ttu-id="d051f-191">Schreibgeschützte Eigenschaften werden nicht serialisiert.</span><span class="sxs-lookup"><span data-stu-id="d051f-191">Read-only properties are not serialized.</span></span> <span data-ttu-id="d051f-192">Wenn eine schreibgeschützte Eigenschaft ein private dahinter liegende Feld verfügt, können, markieren Sie das private Feld mit der **DataMember** Attribut.</span><span class="sxs-lookup"><span data-stu-id="d051f-192">If a read-only property has a backing private field, you can mark the private field with the **DataMember** attribute.</span></span> <span data-ttu-id="d051f-193">Dieser Ansatz erfordert das **DataContract** -Attribut für die Klasse.</span><span class="sxs-lookup"><span data-stu-id="d051f-193">This approach requires the **DataContract** attribute on the class.</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample13.cs)]

<a id="xml_dates"></a>
### <a name="dates"></a><span data-ttu-id="d051f-194">Datumsangaben</span><span class="sxs-lookup"><span data-stu-id="d051f-194">Dates</span></span>

<span data-ttu-id="d051f-195">Datumsangaben werden im ISO 8601-Format geschrieben.</span><span class="sxs-lookup"><span data-stu-id="d051f-195">Dates are written in ISO 8601 format.</span></span> <span data-ttu-id="d051f-196">Beispielsweise &quot;2012-05-23T20:21:37.9116538Z&quot;.</span><span class="sxs-lookup"><span data-stu-id="d051f-196">For example, &quot;2012-05-23T20:21:37.9116538Z&quot;.</span></span>

<a id="xml_indenting"></a>
### <a name="indenting"></a><span data-ttu-id="d051f-197">Einzug</span><span class="sxs-lookup"><span data-stu-id="d051f-197">Indenting</span></span>

<span data-ttu-id="d051f-198">Um eingezogene XML zu schreiben, legen die **Einzug** Eigenschaft **"true"**:</span><span class="sxs-lookup"><span data-stu-id="d051f-198">To write indented XML, set the **Indent** property to **true**:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample14.cs)]

<a id="xml_pertype"></a>
## <a name="setting-per-type-xml-serializers"></a><span data-ttu-id="d051f-199">Festlegen von pro-Type-XML-Serialisierungsprogrammen</span><span class="sxs-lookup"><span data-stu-id="d051f-199">Setting Per-Type XML Serializers</span></span>

<span data-ttu-id="d051f-200">Sie können verschiedene XML-Serialisierer für andere CLR-Typen festlegen.</span><span class="sxs-lookup"><span data-stu-id="d051f-200">You can set different XML serializers for different CLR types.</span></span> <span data-ttu-id="d051f-201">Angenommen, Sie müssen möglicherweise ein bestimmtes Datenobjekt, das erfordert **XmlSerializer** Gründen der Abwärtskompatibilität.</span><span class="sxs-lookup"><span data-stu-id="d051f-201">For example, you might have a particular data object that requires **XmlSerializer** for backward compatibility.</span></span> <span data-ttu-id="d051f-202">Sie können **XmlSerializer** für dieses Objekt aus, und verwenden weiterhin **"DataContractSerializer"** für andere Typen.</span><span class="sxs-lookup"><span data-stu-id="d051f-202">You can use **XmlSerializer** for this object and continue to use **DataContractSerializer** for other types.</span></span>

<span data-ttu-id="d051f-203">Rufen Sie zum Festlegen einer XML-Serialisierungsprogramm für einen bestimmten Typ **SetSerializer**.</span><span class="sxs-lookup"><span data-stu-id="d051f-203">To set an XML serializer for a particular type, call **SetSerializer**.</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample15.cs)]

<span data-ttu-id="d051f-204">Sie können angeben, ein **XmlSerializer** oder jedes Objekt, das von abgeleitet ist **XmlObjectSerializer**.</span><span class="sxs-lookup"><span data-stu-id="d051f-204">You can specify an **XmlSerializer** or any object that derives from **XmlObjectSerializer**.</span></span>

<a id="removing_the_json_or_xml_formatter"></a>
## <a name="removing-the-json-or-xml-formatter"></a><span data-ttu-id="d051f-205">Entfernen des JSON oder XML-Formatierer</span><span class="sxs-lookup"><span data-stu-id="d051f-205">Removing the JSON or XML Formatter</span></span>

<span data-ttu-id="d051f-206">Sie können die JSON-Formatierungsprogramm oder der XML-Formatierer aus der Liste der Formatierer, entfernen, wenn Sie nicht verwenden möchten.</span><span class="sxs-lookup"><span data-stu-id="d051f-206">You can remove the JSON formatter or the XML formatter from the list of formatters, if you do not want to use them.</span></span> <span data-ttu-id="d051f-207">Die Gründe hierfür sind:</span><span class="sxs-lookup"><span data-stu-id="d051f-207">The main reasons to do this are:</span></span>

- <span data-ttu-id="d051f-208">Beschränken Sie Ihre Web-API-Antworten auf einen bestimmten Medientyp.</span><span class="sxs-lookup"><span data-stu-id="d051f-208">To restrict your web API responses to a particular media type.</span></span> <span data-ttu-id="d051f-209">Sie könnten z. B. nur JSON-Antworten zu unterstützen, und entfernen die XML-Formatierer.</span><span class="sxs-lookup"><span data-stu-id="d051f-209">For example, you might decide to support only JSON responses, and remove the XML formatter.</span></span>
- <span data-ttu-id="d051f-210">Der Standardformatierer durch ein benutzerdefiniertes Formatierungsprogramm ersetzen.</span><span class="sxs-lookup"><span data-stu-id="d051f-210">To replace the default formatter with a custom formatter.</span></span> <span data-ttu-id="d051f-211">Beispielsweise konnte durch eine eigene benutzerdefinierte Implementierung einer JSON-Formatierung den JSON-Formatierer ersetzt werden.</span><span class="sxs-lookup"><span data-stu-id="d051f-211">For example, you could replace the JSON formatter with your own custom implementation of a JSON formatter.</span></span>

<span data-ttu-id="d051f-212">Der folgende Code zeigt, wie Sie die Standard-Formatierer entfernen.</span><span class="sxs-lookup"><span data-stu-id="d051f-212">The following code shows how to remove the default formatters.</span></span> <span data-ttu-id="d051f-213">Rufen Sie diese von Ihrem **Anwendung\_starten** Methode, die in "Global.asax" definiert.</span><span class="sxs-lookup"><span data-stu-id="d051f-213">Call this from your **Application\_Start** method, defined in Global.asax.</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample16.cs)]

<a id="handling_circular_object_references"></a>
## <a name="handling-circular-object-references"></a><span data-ttu-id="d051f-214">Behandlung von zirkuläre Objektverweise</span><span class="sxs-lookup"><span data-stu-id="d051f-214">Handling Circular Object References</span></span>

<span data-ttu-id="d051f-215">Die JSON- und XML-Formatierer Schreiben aller Objekte als Werte.</span><span class="sxs-lookup"><span data-stu-id="d051f-215">By default, the JSON and XML formatters write all objects as values.</span></span> <span data-ttu-id="d051f-216">Wenn zwei Eigenschaften auf dasselbe Objekt verweisen oder das gleiche Objekt zweimal in einer Auflistung angezeigt wird, wird das Formatierungsprogramm Serialisieren des Objekts zweimal.</span><span class="sxs-lookup"><span data-stu-id="d051f-216">If two properties refer to the same object, or if the same object appears twice in a collection, the formatter will serialize the object twice.</span></span> <span data-ttu-id="d051f-217">Dies ist ein bestimmtes Problem enthält Ihre Objektdiagramm Zyklen, da das Serialisierungsprogramm eine Ausnahme auslöst, wenn eine Schleife im Diagramm erkannt wird.</span><span class="sxs-lookup"><span data-stu-id="d051f-217">This is a particular problem if your object graph contains cycles, because the serializer will throw an exception when it detects a loop in the graph.</span></span>

<span data-ttu-id="d051f-218">Betrachten Sie den folgenden Objektmodellen und Controller.</span><span class="sxs-lookup"><span data-stu-id="d051f-218">Consider the following object models and controller.</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample17.cs)]

<span data-ttu-id="d051f-219">Diese Aktion wird das Formatierungsprogramm löst das Aufrufen eine Ausnahme aus, das in einem Status Code 500 (Interner Serverfehler) Antwort an den Client übersetzt ausgelöst.</span><span class="sxs-lookup"><span data-stu-id="d051f-219">Invoking this action will cause the formatter to thrown an exception, which translates to a status code 500 (Internal Server Error) response to the client.</span></span>

<span data-ttu-id="d051f-220">Um Objektverweise im JSON-Format zu erhalten, fügen Sie den folgenden Code zum **Anwendung\_starten** Methode in der Datei "Global.asax":</span><span class="sxs-lookup"><span data-stu-id="d051f-220">To preserve object references in JSON, add the following code to **Application\_Start** method in the Global.asax file:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample18.cs)]

<span data-ttu-id="d051f-221">Jetzt wird der Controlleraktion JSON zurück, die wie folgt aussieht:</span><span class="sxs-lookup"><span data-stu-id="d051f-221">Now the controller action will return JSON that looks like this:</span></span>

[!code-json[Main](json-and-xml-serialization/samples/sample19.json)]

<span data-ttu-id="d051f-222">Beachten Sie, die das Serialisierungsprogramm fügt eine &quot;$id&quot; Eigenschaft, um beide Objekte.</span><span class="sxs-lookup"><span data-stu-id="d051f-222">Notice that the serializer adds an &quot;$id&quot; property to both objects.</span></span> <span data-ttu-id="d051f-223">Erkennt außerdem, dass die Employee.Department-Eigenschaft eine Schleife erstellt, sodass es den Wert durch einen Objektverweis ersetzt: {&quot;$ref&quot;:&quot;1&quot;}.</span><span class="sxs-lookup"><span data-stu-id="d051f-223">Also, it detects that the Employee.Department property creates a loop, so it replaces the value with an object reference: {&quot;$ref&quot;:&quot;1&quot;}.</span></span>

> [!NOTE]
> <span data-ttu-id="d051f-224">Objektverweise sind nicht in JSON standard.</span><span class="sxs-lookup"><span data-stu-id="d051f-224">Object references are not standard in JSON.</span></span> <span data-ttu-id="d051f-225">Beachten Sie, ob die Clients die Ergebnisse analysieren können, bevor Sie mit dieser Funktion.</span><span class="sxs-lookup"><span data-stu-id="d051f-225">Before using this feature, consider whether your clients will be able to parse the results.</span></span> <span data-ttu-id="d051f-226">Es möglicherweise besser einfach, Zyklen aus dem Diagramm zu entfernen.</span><span class="sxs-lookup"><span data-stu-id="d051f-226">It might be better simply to remove cycles from the graph.</span></span> <span data-ttu-id="d051f-227">In diesem Beispiel wird die Verknüpfung zwischen Mitarbeiter an Abteilung z. B. wirklich nicht erforderlich.</span><span class="sxs-lookup"><span data-stu-id="d051f-227">For example, the link from Employee back to Department is not really needed in this example.</span></span>


<span data-ttu-id="d051f-228">Um Objektverweise im XML-Format zu erhalten, müssen Sie zwei Optionen zur Verfügung.</span><span class="sxs-lookup"><span data-stu-id="d051f-228">To preserve object references in XML, you have two options.</span></span> <span data-ttu-id="d051f-229">Die einfachere Möglichkeit besteht darin hinzufügen `[DataContract(IsReference=true)]` der Modell-Klasse.</span><span class="sxs-lookup"><span data-stu-id="d051f-229">The simpler option is to add `[DataContract(IsReference=true)]` to your model class.</span></span> <span data-ttu-id="d051f-230">Die *IsReference* Parameter ermöglicht Objektverweise.</span><span class="sxs-lookup"><span data-stu-id="d051f-230">The *IsReference* parameter enables object references.</span></span> <span data-ttu-id="d051f-231">Beachten Sie, dass **DataContract** Serialisierung teilnehmen können, stellt, daher Sie auch hinzufügen müssen **DataMember** -Attribute verwenden, um die Eigenschaften:</span><span class="sxs-lookup"><span data-stu-id="d051f-231">Remember that **DataContract** makes serialization opt-in, so you will also need to add **DataMember** attributes to the properties:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample20.cs)]

<span data-ttu-id="d051f-232">Nun das Formatierungsprogramm XML-Code ähnelt generiert, die folgenden:</span><span class="sxs-lookup"><span data-stu-id="d051f-232">Now the formatter will produce XML similar to following:</span></span>

[!code-xml[Main](json-and-xml-serialization/samples/sample21.xml)]

<span data-ttu-id="d051f-233">Wenn Sie Attribute in Ihrer Modellklasse vermeiden möchten, ist es eine weitere Option: Erstellen einer neuen typspezifische **"DataContractSerializer"** -Instanz, und legen Sie *PreserveObjectReferences* zu **"true"**  im Konstruktor.</span><span class="sxs-lookup"><span data-stu-id="d051f-233">If you want to avoid attributes on your model class, there is another option: Create a new type-specific **DataContractSerializer** instance and set *preserveObjectReferences* to **true** in the constructor.</span></span> <span data-ttu-id="d051f-234">Legen Sie dann diese Instanz als ein Serialisierungsprogramm pro Typ, auf die XML-medientypformatierer.</span><span class="sxs-lookup"><span data-stu-id="d051f-234">Then set this instance as a per-type serializer on the XML media-type formatter.</span></span> <span data-ttu-id="d051f-235">Der folgende Code zeigt, wie dies:</span><span class="sxs-lookup"><span data-stu-id="d051f-235">The following code show how to do this:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample22.cs?highlight=3)]

<a id="testing_object_serialization"></a>
## <a name="testing-object-serialization"></a><span data-ttu-id="d051f-236">Testen die Serialisierung von Objekten</span><span class="sxs-lookup"><span data-stu-id="d051f-236">Testing Object Serialization</span></span>

<span data-ttu-id="d051f-237">Wie Sie Ihre Web-API entworfen haben, ist es hilfreich, testen, wie Ihre Datenobjekte serialisiert werden.</span><span class="sxs-lookup"><span data-stu-id="d051f-237">As you design your web API, it is useful to test how your data objects will be serialized.</span></span> <span data-ttu-id="d051f-238">Dies ist möglich, ohne Erstellen eines Domänencontrollers oder eine Controlleraktion aufgerufen.</span><span class="sxs-lookup"><span data-stu-id="d051f-238">You can do this without creating a controller or invoking a controller action.</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample23.cs)]
