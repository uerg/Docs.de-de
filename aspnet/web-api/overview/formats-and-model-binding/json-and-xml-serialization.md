---
uid: web-api/overview/formats-and-model-binding/json-and-xml-serialization
title: JSON- und XML-Serialisierung in ASP.NET Web-API | Microsoft-Dokumentation
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/30/2012
ms.topic: article
ms.assetid: 1cd7525d-de5e-4ab6-94f0-51480d3255d1
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/formats-and-model-binding/json-and-xml-serialization
msc.type: authoredcontent
ms.openlocfilehash: e7fbcd41d64651255763c7629f0232788dcb3d30
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/03/2018
ms.locfileid: "37400920"
---
<a name="json-and-xml-serialization-in-aspnet-web-api"></a><span data-ttu-id="d20b7-102">JSON- und XML-Serialisierung in ASP.NET Web-API</span><span class="sxs-lookup"><span data-stu-id="d20b7-102">JSON and XML Serialization in ASP.NET Web API</span></span>
====================
<span data-ttu-id="d20b7-103">durch [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="d20b7-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="d20b7-104">Dieser Artikel beschreibt die JSON- und XML-Formatierer in ASP.NET Web-API.</span><span class="sxs-lookup"><span data-stu-id="d20b7-104">This article describes the JSON and XML formatters in ASP.NET Web API.</span></span>

<span data-ttu-id="d20b7-105">In ASP.NET Web-API eine *medientypformatierer* ist ein Objekt, das können:</span><span class="sxs-lookup"><span data-stu-id="d20b7-105">In ASP.NET Web API, a *media-type formatter* is an object that can:</span></span>

- <span data-ttu-id="d20b7-106">Read-CLR-Objekte aus einer HTTP-Nachrichtentext</span><span class="sxs-lookup"><span data-stu-id="d20b7-106">Read CLR objects from an HTTP message body</span></span>
- <span data-ttu-id="d20b7-107">Schreiben von CLR-Objekten in einen HTTP-Nachrichtentext</span><span class="sxs-lookup"><span data-stu-id="d20b7-107">Write CLR objects into an HTTP message body</span></span>

<span data-ttu-id="d20b7-108">Web-API bietet die medientypformatierer für JSON- und XML.</span><span class="sxs-lookup"><span data-stu-id="d20b7-108">Web API provides media-type formatters for both JSON and XML.</span></span> <span data-ttu-id="d20b7-109">Das Framework fügt dieser Formatierer in die Pipeline in der Standardeinstellung.</span><span class="sxs-lookup"><span data-stu-id="d20b7-109">The framework inserts these formatters into the pipeline by default.</span></span> <span data-ttu-id="d20b7-110">Clients können XML oder JSON im Accept-Header der HTTP-Anforderung anfordern.</span><span class="sxs-lookup"><span data-stu-id="d20b7-110">Clients can request either JSON or XML in the Accept header of the HTTP request.</span></span>

## <a name="contents"></a><span data-ttu-id="d20b7-111">Inhalt</span><span class="sxs-lookup"><span data-stu-id="d20b7-111">Contents</span></span>

- [<span data-ttu-id="d20b7-112">JSON-Medientypformatierer</span><span class="sxs-lookup"><span data-stu-id="d20b7-112">JSON Media-Type Formatter</span></span>](#json_media_type_formatter)

    - [<span data-ttu-id="d20b7-113">Schreibgeschützte Eigenschaften</span><span class="sxs-lookup"><span data-stu-id="d20b7-113">Read-Only Properties</span></span>](#json_readonly)
    - [<span data-ttu-id="d20b7-114">Datumsangaben</span><span class="sxs-lookup"><span data-stu-id="d20b7-114">Dates</span></span>](#json_dates)
    - [<span data-ttu-id="d20b7-115">Einzug</span><span class="sxs-lookup"><span data-stu-id="d20b7-115">Indenting</span></span>](#json_indenting)
    - [<span data-ttu-id="d20b7-116">Kamel-Schreibweise</span><span class="sxs-lookup"><span data-stu-id="d20b7-116">Camel Casing</span></span>](#json_camelcasing)
    - [<span data-ttu-id="d20b7-117">Anonyme und schwach typisierte Objekte</span><span class="sxs-lookup"><span data-stu-id="d20b7-117">Anonymous and Weakly-Typed Objects</span></span>](#json_anon)
- [<span data-ttu-id="d20b7-118">XML-Medientypformatierer</span><span class="sxs-lookup"><span data-stu-id="d20b7-118">XML Media-Type Formatter</span></span>](#xml_media_type_formatter)

    - [<span data-ttu-id="d20b7-119">Schreibgeschützte Eigenschaften</span><span class="sxs-lookup"><span data-stu-id="d20b7-119">Read-Only Properties</span></span>](#xml_readonly)
    - [<span data-ttu-id="d20b7-120">Datumsangaben</span><span class="sxs-lookup"><span data-stu-id="d20b7-120">Dates</span></span>](#xml_dates)
    - [<span data-ttu-id="d20b7-121">Einzug</span><span class="sxs-lookup"><span data-stu-id="d20b7-121">Indenting</span></span>](#xml_indenting)
    - [<span data-ttu-id="d20b7-122">Festlegen von pro-Type-XML-Serialisierungsprogrammen</span><span class="sxs-lookup"><span data-stu-id="d20b7-122">Setting Per-Type XML Serializers</span></span>](#xml_pertype)
- [<span data-ttu-id="d20b7-123">Entfernen die JSON- oder XML-Formatierungsprogramm</span><span class="sxs-lookup"><span data-stu-id="d20b7-123">Removing the JSON or XML Formatter</span></span>](#removing_the_json_or_xml_formatter)
- [<span data-ttu-id="d20b7-124">Behandeln von zyklischen Objektverweisen</span><span class="sxs-lookup"><span data-stu-id="d20b7-124">Handling Circular Object References</span></span>](#handling_circular_object_references)
- [<span data-ttu-id="d20b7-125">Testen die Serialisierung von Objekten</span><span class="sxs-lookup"><span data-stu-id="d20b7-125">Testing Object Serialization</span></span>](#testing_object_serialization)

<a id="json_media_type_formatter"></a>
## <a name="json-media-type-formatter"></a><span data-ttu-id="d20b7-126">JSON-Medientypformatierer</span><span class="sxs-lookup"><span data-stu-id="d20b7-126">JSON Media-Type Formatter</span></span>

<span data-ttu-id="d20b7-127">JSON-Formatierung erfolgt über die **JsonMediaTypeFormatter** Klasse.</span><span class="sxs-lookup"><span data-stu-id="d20b7-127">JSON formatting is provided by the **JsonMediaTypeFormatter** class.</span></span> <span data-ttu-id="d20b7-128">In der Standardeinstellung **JsonMediaTypeFormatter** verwendet die [Json.NET](https://github.com/JamesNK/Newtonsoft.Json) Bibliothek zur Serialisierung.</span><span class="sxs-lookup"><span data-stu-id="d20b7-128">By default, **JsonMediaTypeFormatter** uses the [Json.NET](https://github.com/JamesNK/Newtonsoft.Json) library to perform serialization.</span></span> <span data-ttu-id="d20b7-129">Json.NET wird eine Drittanbieter-open-Source-Projekt.</span><span class="sxs-lookup"><span data-stu-id="d20b7-129">Json.NET is a third-party open source project.</span></span>

<span data-ttu-id="d20b7-130">Wenn Sie es vorziehen, können Sie konfigurieren die **JsonMediaTypeFormatter** zu verwendende Klasse an die **DataContractJsonSerializer** anstelle von Json.NET.</span><span class="sxs-lookup"><span data-stu-id="d20b7-130">If you prefer, you can configure the **JsonMediaTypeFormatter** class to use the **DataContractJsonSerializer** instead of Json.NET.</span></span> <span data-ttu-id="d20b7-131">Zu diesem Zweck legen Sie die **UseDataContractJsonSerializer** Eigenschaft **"true"**:</span><span class="sxs-lookup"><span data-stu-id="d20b7-131">To do so, set the **UseDataContractJsonSerializer** property to **true**:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample1.cs)]

### <a name="json-serialization"></a><span data-ttu-id="d20b7-132">JSON-Serialisierung</span><span class="sxs-lookup"><span data-stu-id="d20b7-132">JSON Serialization</span></span>

<span data-ttu-id="d20b7-133">Dieser Abschnitt beschreibt einige bestimmten Verhaltensweisen von JSON-Formatierungsprogramm, über das standardmäßige [Json.NET](https://github.com/JamesNK/Newtonsoft.Json) Serialisierungsprogramm.</span><span class="sxs-lookup"><span data-stu-id="d20b7-133">This section describes some specific behaviors of the JSON formatter, using the default [Json.NET](https://github.com/JamesNK/Newtonsoft.Json) serializer.</span></span> <span data-ttu-id="d20b7-134">Dies soll keine umfassende Dokumentation der JSON.NET-Bibliothek werden; Weitere Informationen finden Sie unter den [JSON.NET-Dokumentation](http://james.newtonking.com/projects/json/help/).</span><span class="sxs-lookup"><span data-stu-id="d20b7-134">This is not meant to be comprehensive documentation of the Json.NET library; for more information, see the [Json.NET Documentation](http://james.newtonking.com/projects/json/help/).</span></span>

#### <a name="what-gets-serialized"></a><span data-ttu-id="d20b7-135">Was serialisiert werden?</span><span class="sxs-lookup"><span data-stu-id="d20b7-135">What Gets Serialized?</span></span>

<span data-ttu-id="d20b7-136">Standardmäßig sind alle öffentlichen Eigenschaften und Felder in den serialisierten JSON-Code enthalten.</span><span class="sxs-lookup"><span data-stu-id="d20b7-136">By default, all public properties and fields are included in the serialized JSON.</span></span> <span data-ttu-id="d20b7-137">Um eine Eigenschaft oder ein Feld zu unterdrücken, versehen sie mit der **JsonIgnore** Attribut.</span><span class="sxs-lookup"><span data-stu-id="d20b7-137">To omit a property or field, decorate it with the **JsonIgnore** attribute.</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample2.cs)]

<span data-ttu-id="d20b7-138">Falls gewünscht ein &quot;teilnehmen&quot; Ansatz, ergänzen die Klasse der **DataContract** Attribut.</span><span class="sxs-lookup"><span data-stu-id="d20b7-138">If you prefer an &quot;opt-in&quot; approach, decorate the class with the **DataContract** attribute.</span></span> <span data-ttu-id="d20b7-139">Wenn dieses Attribut vorhanden ist, werden Elemente ignoriert, es sei denn, sie haben die **DataMember**.</span><span class="sxs-lookup"><span data-stu-id="d20b7-139">If this attribute is present, members are ignored unless they have the **DataMember**.</span></span> <span data-ttu-id="d20b7-140">Sie können auch **DataMember** Private Member zu serialisieren.</span><span class="sxs-lookup"><span data-stu-id="d20b7-140">You can also use **DataMember** to serialize private members.</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample3.cs)]

<a id="json_readonly"></a>
### <a name="read-only-properties"></a><span data-ttu-id="d20b7-141">Schreibgeschützte Eigenschaften</span><span class="sxs-lookup"><span data-stu-id="d20b7-141">Read-Only Properties</span></span>

<span data-ttu-id="d20b7-142">Standardmäßig werden die schreibgeschützten Eigenschaften serialisiert.</span><span class="sxs-lookup"><span data-stu-id="d20b7-142">Read-only properties are serialized by default.</span></span>

<a id="json_dates"></a>
### <a name="dates"></a><span data-ttu-id="d20b7-143">Datumsangaben</span><span class="sxs-lookup"><span data-stu-id="d20b7-143">Dates</span></span>

<span data-ttu-id="d20b7-144">Standardmäßig schreibt Json.NET Datumsangaben [ISO 8601](http://www.w3.org/TR/NOTE-datetime) Format.</span><span class="sxs-lookup"><span data-stu-id="d20b7-144">By default, Json.NET writes dates in [ISO 8601](http://www.w3.org/TR/NOTE-datetime) format.</span></span> <span data-ttu-id="d20b7-145">Datumsangaben in UTC (Coordinated Universal Time) werden mit dem Suffix "Z" geschrieben.</span><span class="sxs-lookup"><span data-stu-id="d20b7-145">Dates in UTC (Coordinated Universal Time) are written with a "Z" suffix.</span></span> <span data-ttu-id="d20b7-146">Datumsangaben in Ortszeit enthalten einen Zeitzonenoffset.</span><span class="sxs-lookup"><span data-stu-id="d20b7-146">Dates in local time include a time-zone offset.</span></span> <span data-ttu-id="d20b7-147">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="d20b7-147">For example:</span></span>

[!code-console[Main](json-and-xml-serialization/samples/sample4.cmd)]

<span data-ttu-id="d20b7-148">Standardmäßig behält Json.NET die Zeitzone an.</span><span class="sxs-lookup"><span data-stu-id="d20b7-148">By default, Json.NET preserves the time zone.</span></span> <span data-ttu-id="d20b7-149">Sie können dies durch Festlegen der DateTimeZoneHandling-Eigenschaft außer Kraft setzen:</span><span class="sxs-lookup"><span data-stu-id="d20b7-149">You can override this by setting the DateTimeZoneHandling property:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample5.cs)]

<span data-ttu-id="d20b7-150">Wenn Sie lieber mit [Microsoft JSON-Datumsformat](https://msdn.microsoft.com/library/bb299886.aspx#intro_to_json_sidebarb) (`"\/Date(ticks)\/"`) anstelle von ISO 8601, legen Sie die **DateFormatHandling** Eigenschaft für die serialisierereinstellungen:</span><span class="sxs-lookup"><span data-stu-id="d20b7-150">If you prefer to use [Microsoft JSON date format](https://msdn.microsoft.com/library/bb299886.aspx#intro_to_json_sidebarb) (`"\/Date(ticks)\/"`) instead of ISO 8601, set the **DateFormatHandling** property on the serializer settings:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample6.cs)]

<a id="json_indenting"></a>
### <a name="indenting"></a><span data-ttu-id="d20b7-151">Einzug</span><span class="sxs-lookup"><span data-stu-id="d20b7-151">Indenting</span></span>

<span data-ttu-id="d20b7-152">Um eingezogen JSON zu schreiben, legen die **Formatierung** auf **Formatting.Indented**:</span><span class="sxs-lookup"><span data-stu-id="d20b7-152">To write indented JSON, set the **Formatting** setting to **Formatting.Indented**:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample7.cs)]

<a id="json_camelcasing"></a>
### <a name="camel-casing"></a><span data-ttu-id="d20b7-153">Kamel-Schreibweise</span><span class="sxs-lookup"><span data-stu-id="d20b7-153">Camel Casing</span></span>

<span data-ttu-id="d20b7-154">Um JSON-Eigenschaftennamen mit Kamel-Schreibweise, schreiben, ohne Ihr Datenmodell ändern, legen die **CamelCasePropertyNamesContractResolver** auf das Serialisierungsprogramm:</span><span class="sxs-lookup"><span data-stu-id="d20b7-154">To write JSON property names with camel casing, without changing your data model, set the **CamelCasePropertyNamesContractResolver** on the serializer:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample8.cs)]

<a id="json_anon"></a>
### <a name="anonymous-and-weakly-typed-objects"></a><span data-ttu-id="d20b7-155">Anonyme und schwach typisierte Objekte</span><span class="sxs-lookup"><span data-stu-id="d20b7-155">Anonymous and Weakly-Typed Objects</span></span>

<span data-ttu-id="d20b7-156">Eine Aktionsmethode kann ein anonymes Objekt zurückgeben und deren Serialisierung in JSON.</span><span class="sxs-lookup"><span data-stu-id="d20b7-156">An action method can return an anonymous object and serialize it to JSON.</span></span> <span data-ttu-id="d20b7-157">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="d20b7-157">For example:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample9.cs)]

<span data-ttu-id="d20b7-158">Text der Antwortnachricht enthält die folgenden JSON-Code:</span><span class="sxs-lookup"><span data-stu-id="d20b7-158">The response message body will contain the following JSON:</span></span>

[!code-json[Main](json-and-xml-serialization/samples/sample10.json)]

<span data-ttu-id="d20b7-159">Wenn Ihre Web-API lose empfängt JSON-Objekte von den Clients strukturiert sind, können Sie den Hauptteil der Anforderung zum Deserialisieren einer **Newtonsoft.Json.Linq.JObject** Typ.</span><span class="sxs-lookup"><span data-stu-id="d20b7-159">If your web API receives loosely structured JSON objects from clients, you can deserialize the request body to a **Newtonsoft.Json.Linq.JObject** type.</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample11.cs)]

<span data-ttu-id="d20b7-160">Allerdings ist es normalerweise besser, stark typisierte Datenobjekte zu verwenden.</span><span class="sxs-lookup"><span data-stu-id="d20b7-160">However, it is usually better to use strongly typed data objects.</span></span> <span data-ttu-id="d20b7-161">Klicken Sie dann Sie müssen nicht die Daten zu analysieren, und erhalten Sie die Vorteile der modellvalidierung.</span><span class="sxs-lookup"><span data-stu-id="d20b7-161">Then you don't need to parse the data yourself, and you get the benefits of model validation.</span></span>

<span data-ttu-id="d20b7-162">Das XML-Serialisierungsprogramm unterstützt keine anonyme Typen oder **"jobject"** Instanzen.</span><span class="sxs-lookup"><span data-stu-id="d20b7-162">The XML serializer does not support anonymous types or **JObject** instances.</span></span> <span data-ttu-id="d20b7-163">Wenn Sie diese Funktionen für die JSON-Daten verwenden, sollten Sie das XML-Formatierungsprogramm aus der Pipeline entfernen, wie weiter unten in diesem Artikel beschrieben.</span><span class="sxs-lookup"><span data-stu-id="d20b7-163">If you use these features for your JSON data, you should remove the XML formatter from the pipeline, as described later in this article.</span></span>

<a id="xml_media_type_formatter"></a>
## <a name="xml-media-type-formatter"></a><span data-ttu-id="d20b7-164">XML-Medientypformatierer</span><span class="sxs-lookup"><span data-stu-id="d20b7-164">XML Media-Type Formatter</span></span>

<span data-ttu-id="d20b7-165">XML-Formatierung erfolgt über die **XmlMediaTypeFormatter** Klasse.</span><span class="sxs-lookup"><span data-stu-id="d20b7-165">XML formatting is provided by the **XmlMediaTypeFormatter** class.</span></span> <span data-ttu-id="d20b7-166">In der Standardeinstellung **XmlMediaTypeFormatter** verwendet die **DataContractSerializer** Klasse zur Serialisierung.</span><span class="sxs-lookup"><span data-stu-id="d20b7-166">By default, **XmlMediaTypeFormatter** uses the **DataContractSerializer** class to perform serialization.</span></span>

<span data-ttu-id="d20b7-167">Falls gewünscht, können Sie konfigurieren die **XmlMediaTypeFormatter** verwenden die **XmlSerializer** anstelle von der **DataContractSerializer**.</span><span class="sxs-lookup"><span data-stu-id="d20b7-167">If you prefer, you can configure the **XmlMediaTypeFormatter** to use the **XmlSerializer** instead of the **DataContractSerializer**.</span></span> <span data-ttu-id="d20b7-168">Zu diesem Zweck legen Sie die **UseXmlSerializer** Eigenschaft **"true"**:</span><span class="sxs-lookup"><span data-stu-id="d20b7-168">To do so, set the **UseXmlSerializer** property to **true**:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample12.cs)]

<span data-ttu-id="d20b7-169">Die **XmlSerializer** Klasse unterstützt eine eingeschränktere Gruppe von Typen als **DataContractSerializer**, jedoch bietet mehr Kontrolle über den sich ergebenden XML-Code.</span><span class="sxs-lookup"><span data-stu-id="d20b7-169">The **XmlSerializer** class supports a narrower set of types than **DataContractSerializer**, but gives more control over the resulting XML.</span></span> <span data-ttu-id="d20b7-170">Erwägen Sie die Verwendung **XmlSerializer** Wenn Sie ein vorhandenes XML-Schema entsprechen müssen.</span><span class="sxs-lookup"><span data-stu-id="d20b7-170">Consider using **XmlSerializer** if you need to match an existing XML schema.</span></span>

### <a name="xml-serialization"></a><span data-ttu-id="d20b7-171">XML-Serialisierung</span><span class="sxs-lookup"><span data-stu-id="d20b7-171">XML Serialization</span></span>

<span data-ttu-id="d20b7-172">Dieser Abschnitt beschreibt einige bestimmten Verhaltensweisen von der XML-Formatierer, der über das standardmäßige **DataContractSerializer**.</span><span class="sxs-lookup"><span data-stu-id="d20b7-172">This section describes some specific behaviors of the XML formatter, using the default **DataContractSerializer**.</span></span>

<span data-ttu-id="d20b7-173">Standardmäßig verhält sich das DataContractSerializer-Element wie folgt:</span><span class="sxs-lookup"><span data-stu-id="d20b7-173">By default, the DataContractSerializer behaves as follows:</span></span>

- <span data-ttu-id="d20b7-174">Es werden alle öffentlichen Lese-/Schreibeigenschaften und Felder serialisiert.</span><span class="sxs-lookup"><span data-stu-id="d20b7-174">All public read/write properties and fields are serialized.</span></span> <span data-ttu-id="d20b7-175">Um eine Eigenschaft oder ein Feld zu unterdrücken, versehen sie mit der **IgnoreDataMember** Attribut.</span><span class="sxs-lookup"><span data-stu-id="d20b7-175">To omit a property or field, decorate it with the **IgnoreDataMember** attribute.</span></span>
- <span data-ttu-id="d20b7-176">Private und geschützte Member werden nicht serialisiert.</span><span class="sxs-lookup"><span data-stu-id="d20b7-176">Private and protected members are not serialized.</span></span>
- <span data-ttu-id="d20b7-177">Schreibgeschützte Eigenschaften werden nicht serialisiert.</span><span class="sxs-lookup"><span data-stu-id="d20b7-177">Read-only properties are not serialized.</span></span> <span data-ttu-id="d20b7-178">(Allerdings werden die Inhalte einer nur-Lese Auflistungseigenschaft serialisiert.)</span><span class="sxs-lookup"><span data-stu-id="d20b7-178">(However, the contents of a read-only collection property are serialized.)</span></span>
- <span data-ttu-id="d20b7-179">Klassen- und Membernamen werden in der XML-Code geschrieben werden, genau wie in der Klassendeklaration.</span><span class="sxs-lookup"><span data-stu-id="d20b7-179">Class and member names are written in the XML exactly as they appear in the class declaration.</span></span>
- <span data-ttu-id="d20b7-180">Es wird ein XML-Standardnamespace verwendet.</span><span class="sxs-lookup"><span data-stu-id="d20b7-180">A default XML namespace is used.</span></span>

<span data-ttu-id="d20b7-181">Wenn Sie mehr Kontrolle über die Serialisierung benötigen, können Sie ergänzen die Klasse der **DataContract** Attribut.</span><span class="sxs-lookup"><span data-stu-id="d20b7-181">If you need more control over the serialization, you can decorate the class with the **DataContract** attribute.</span></span> <span data-ttu-id="d20b7-182">Wenn dieses Attribut vorhanden ist, wird die Klasse wie folgt serialisiert:</span><span class="sxs-lookup"><span data-stu-id="d20b7-182">When this attribute is present, the class is serialized as follows:</span></span>

- <span data-ttu-id="d20b7-183">&quot;Abonnieren von&quot; Ansatz: Eigenschaften und Felder werden standardmäßig nicht serialisiert.</span><span class="sxs-lookup"><span data-stu-id="d20b7-183">&quot;Opt in&quot; approach: Properties and fields are not serialized by default.</span></span> <span data-ttu-id="d20b7-184">Um eine Eigenschaft oder ein Feld serialisieren, versehen sie mit der **DataMember** Attribut.</span><span class="sxs-lookup"><span data-stu-id="d20b7-184">To serialize a property or field, decorate it with the **DataMember** attribute.</span></span>
- <span data-ttu-id="d20b7-185">Um ein privates oder geschütztes Member zu serialisieren, versehen sie mit der **DataMember** Attribut.</span><span class="sxs-lookup"><span data-stu-id="d20b7-185">To serialize a private or protected member, decorate it with the **DataMember** attribute.</span></span>
- <span data-ttu-id="d20b7-186">Schreibgeschützte Eigenschaften werden nicht serialisiert.</span><span class="sxs-lookup"><span data-stu-id="d20b7-186">Read-only properties are not serialized.</span></span>
- <span data-ttu-id="d20b7-187">Um ändern, wie der Klassenname in der XML-Code angezeigt wird, legen die *Namen* Parameter in der **DataContract** Attribut.</span><span class="sxs-lookup"><span data-stu-id="d20b7-187">To change how the class name appears in the XML, set the *Name* parameter in the **DataContract** attribute.</span></span>
- <span data-ttu-id="d20b7-188">Um ändern, wie ein Elementnamen in der XML-Code angezeigt wird, legen die *Namen* Parameter in der **DataMember** Attribut.</span><span class="sxs-lookup"><span data-stu-id="d20b7-188">To change how a member name appears in the XML, set the *Name* parameter in the **DataMember** attribute.</span></span>
- <span data-ttu-id="d20b7-189">Um den XML-Namespace zu ändern, legen die *Namespace* Parameter in der **DataContract** Klasse.</span><span class="sxs-lookup"><span data-stu-id="d20b7-189">To change the XML namespace, set the *Namespace* parameter in the **DataContract** class.</span></span>

<a id="xml_readonly"></a>
### <a name="read-only-properties"></a><span data-ttu-id="d20b7-190">Schreibgeschützte Eigenschaften</span><span class="sxs-lookup"><span data-stu-id="d20b7-190">Read-Only Properties</span></span>

<span data-ttu-id="d20b7-191">Schreibgeschützte Eigenschaften werden nicht serialisiert.</span><span class="sxs-lookup"><span data-stu-id="d20b7-191">Read-only properties are not serialized.</span></span> <span data-ttu-id="d20b7-192">Verfügt eine nur-Lese Eigenschaft auf ein privates Unterstützungsfeld, können Sie mit das private Feld markieren die **DataMember** Attribut.</span><span class="sxs-lookup"><span data-stu-id="d20b7-192">If a read-only property has a backing private field, you can mark the private field with the **DataMember** attribute.</span></span> <span data-ttu-id="d20b7-193">Dieser Ansatz erfordert den **DataContract** -Attribut in der Klasse.</span><span class="sxs-lookup"><span data-stu-id="d20b7-193">This approach requires the **DataContract** attribute on the class.</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample13.cs)]

<a id="xml_dates"></a>
### <a name="dates"></a><span data-ttu-id="d20b7-194">Datumsangaben</span><span class="sxs-lookup"><span data-stu-id="d20b7-194">Dates</span></span>

<span data-ttu-id="d20b7-195">Datumsangaben werden im ISO 8601-Format geschrieben.</span><span class="sxs-lookup"><span data-stu-id="d20b7-195">Dates are written in ISO 8601 format.</span></span> <span data-ttu-id="d20b7-196">Z. B. &quot;2012-05-23T20:21:37.9116538Z&quot;.</span><span class="sxs-lookup"><span data-stu-id="d20b7-196">For example, &quot;2012-05-23T20:21:37.9116538Z&quot;.</span></span>

<a id="xml_indenting"></a>
### <a name="indenting"></a><span data-ttu-id="d20b7-197">Einzug</span><span class="sxs-lookup"><span data-stu-id="d20b7-197">Indenting</span></span>

<span data-ttu-id="d20b7-198">Um eingezogene XML zu schreiben, legen die **Einzug** Eigenschaft **"true"**:</span><span class="sxs-lookup"><span data-stu-id="d20b7-198">To write indented XML, set the **Indent** property to **true**:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample14.cs)]

<a id="xml_pertype"></a>
## <a name="setting-per-type-xml-serializers"></a><span data-ttu-id="d20b7-199">Festlegen von pro-Type-XML-Serialisierungsprogrammen</span><span class="sxs-lookup"><span data-stu-id="d20b7-199">Setting Per-Type XML Serializers</span></span>

<span data-ttu-id="d20b7-200">Sie können verschiedene XML-Serialisierer für andere CLR-Typen festlegen.</span><span class="sxs-lookup"><span data-stu-id="d20b7-200">You can set different XML serializers for different CLR types.</span></span> <span data-ttu-id="d20b7-201">Angenommen, Sie müssen möglicherweise ein bestimmtes Objekt, das erfordert **XmlSerializer** um Abwärtskompatibilität zu gewährleisten.</span><span class="sxs-lookup"><span data-stu-id="d20b7-201">For example, you might have a particular data object that requires **XmlSerializer** for backward compatibility.</span></span> <span data-ttu-id="d20b7-202">Sie können **XmlSerializer** für dieses Objekt aus, und verwenden weiterhin **DataContractSerializer** für andere Typen.</span><span class="sxs-lookup"><span data-stu-id="d20b7-202">You can use **XmlSerializer** for this object and continue to use **DataContractSerializer** for other types.</span></span>

<span data-ttu-id="d20b7-203">Rufen Sie zum Festlegen einer XML-Serialisierungsprogramm für einen bestimmten Typ **SetSerializer**.</span><span class="sxs-lookup"><span data-stu-id="d20b7-203">To set an XML serializer for a particular type, call **SetSerializer**.</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample15.cs)]

<span data-ttu-id="d20b7-204">Sie können angeben, ein **XmlSerializer** oder ein beliebiges Objekt, das von abgeleitet ist **XmlObjectSerializer-Elements**.</span><span class="sxs-lookup"><span data-stu-id="d20b7-204">You can specify an **XmlSerializer** or any object that derives from **XmlObjectSerializer**.</span></span>

<a id="removing_the_json_or_xml_formatter"></a>
## <a name="removing-the-json-or-xml-formatter"></a><span data-ttu-id="d20b7-205">Entfernen die JSON- oder XML-Formatierungsprogramm</span><span class="sxs-lookup"><span data-stu-id="d20b7-205">Removing the JSON or XML Formatter</span></span>

<span data-ttu-id="d20b7-206">Sie können das JSON-Formatierungsprogramm oder das XML-Formatierungsprogramm aus der Liste der Formatierer, entfernen, wenn Sie nicht, deren Verwendung möchten.</span><span class="sxs-lookup"><span data-stu-id="d20b7-206">You can remove the JSON formatter or the XML formatter from the list of formatters, if you do not want to use them.</span></span> <span data-ttu-id="d20b7-207">Zu diesem Zweck die wichtigsten Gründe sind:</span><span class="sxs-lookup"><span data-stu-id="d20b7-207">The main reasons to do this are:</span></span>

- <span data-ttu-id="d20b7-208">Um Ihre Web-API-Antworten auf einen bestimmten Medientyp zu beschränken.</span><span class="sxs-lookup"><span data-stu-id="d20b7-208">To restrict your web API responses to a particular media type.</span></span> <span data-ttu-id="d20b7-209">Sie könnten z. B. nur JSON-Antworten zu unterstützen, und entfernen das XML-Formatierungsprogramm.</span><span class="sxs-lookup"><span data-stu-id="d20b7-209">For example, you might decide to support only JSON responses, and remove the XML formatter.</span></span>
- <span data-ttu-id="d20b7-210">Um das Standardformatierungsprogramm durch ein benutzerdefiniertes Formatierungsprogramm zu ersetzen.</span><span class="sxs-lookup"><span data-stu-id="d20b7-210">To replace the default formatter with a custom formatter.</span></span> <span data-ttu-id="d20b7-211">Beispielsweise können Sie das JSON-Formatierungsprogramm durch Ihre eigene benutzerdefinierte Implementierung einer JSON-Formatierung ersetzen.</span><span class="sxs-lookup"><span data-stu-id="d20b7-211">For example, you could replace the JSON formatter with your own custom implementation of a JSON formatter.</span></span>

<span data-ttu-id="d20b7-212">Der folgende Code zeigt, wie Sie die Standard-Formatierer zu entfernen.</span><span class="sxs-lookup"><span data-stu-id="d20b7-212">The following code shows how to remove the default formatters.</span></span> <span data-ttu-id="d20b7-213">Rufen Sie diese aus Ihrem **Anwendung\_starten** Methode, die in "Global.asax" definiert.</span><span class="sxs-lookup"><span data-stu-id="d20b7-213">Call this from your **Application\_Start** method, defined in Global.asax.</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample16.cs)]

<a id="handling_circular_object_references"></a>
## <a name="handling-circular-object-references"></a><span data-ttu-id="d20b7-214">Behandeln von zyklischen Objektverweisen</span><span class="sxs-lookup"><span data-stu-id="d20b7-214">Handling Circular Object References</span></span>

<span data-ttu-id="d20b7-215">Standardmäßig schreiben die JSON- und XML-Formatierer alle Objekte als Werte.</span><span class="sxs-lookup"><span data-stu-id="d20b7-215">By default, the JSON and XML formatters write all objects as values.</span></span> <span data-ttu-id="d20b7-216">Wenn zwei Eigenschaften, die auf dasselbe Objekt verweisen oder wenn das gleiche Objekt zweimal in einer Auflistung angezeigt wird, das Formatierungsprogramm das Objekt zweimal serialisiert.</span><span class="sxs-lookup"><span data-stu-id="d20b7-216">If two properties refer to the same object, or if the same object appears twice in a collection, the formatter will serialize the object twice.</span></span> <span data-ttu-id="d20b7-217">Dies ist ein bestimmtes Problem enthält Ihre Objektdiagramm Zyklen, da das Serialisierungsprogramm eine Ausnahme auslöst, wenn im Diagramm eine Schleife erkannt.</span><span class="sxs-lookup"><span data-stu-id="d20b7-217">This is a particular problem if your object graph contains cycles, because the serializer will throw an exception when it detects a loop in the graph.</span></span>

<span data-ttu-id="d20b7-218">Erwägen Sie die folgenden Objektmodellen und den Controller.</span><span class="sxs-lookup"><span data-stu-id="d20b7-218">Consider the following object models and controller.</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample17.cs)]

<span data-ttu-id="d20b7-219">Aufrufen von dieser Aktion wird den Formatierer, dazu führen, dass eine Ausnahme, die in einem Status Code 500 (Interner Serverfehler) Antwort an den Client übersetzt.</span><span class="sxs-lookup"><span data-stu-id="d20b7-219">Invoking this action will cause the formatter to thrown an exception, which translates to a status code 500 (Internal Server Error) response to the client.</span></span>

<span data-ttu-id="d20b7-220">Um Objektverweise im JSON-Format zu erhalten, fügen Sie den folgenden Code **Anwendung\_starten** -Methode in der Datei "Global.asax":</span><span class="sxs-lookup"><span data-stu-id="d20b7-220">To preserve object references in JSON, add the following code to **Application\_Start** method in the Global.asax file:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample18.cs)]

<span data-ttu-id="d20b7-221">Jetzt wird die Controlleraktion JSON zurück, die wie folgt aussieht:</span><span class="sxs-lookup"><span data-stu-id="d20b7-221">Now the controller action will return JSON that looks like this:</span></span>

[!code-json[Main](json-and-xml-serialization/samples/sample19.json)]

<span data-ttu-id="d20b7-222">Beachten Sie, die das Serialisierungsprogramm fügt eine &quot;$id&quot; Eigenschaft, um beide Objekte.</span><span class="sxs-lookup"><span data-stu-id="d20b7-222">Notice that the serializer adds an &quot;$id&quot; property to both objects.</span></span> <span data-ttu-id="d20b7-223">Darüber hinaus erkannt wird, dass die Employee.Department-Eigenschaft eine Schleife erstellt, sodass den Wert durch einen Objektverweis ersetzt: {&quot;$ref&quot;:&quot;1&quot;}.</span><span class="sxs-lookup"><span data-stu-id="d20b7-223">Also, it detects that the Employee.Department property creates a loop, so it replaces the value with an object reference: {&quot;$ref&quot;:&quot;1&quot;}.</span></span>

> [!NOTE]
> <span data-ttu-id="d20b7-224">Objektverweise sind nicht in JSON-standard.</span><span class="sxs-lookup"><span data-stu-id="d20b7-224">Object references are not standard in JSON.</span></span> <span data-ttu-id="d20b7-225">Beachten Sie, ob die Clients die Ergebnisse analysieren können, werden, bevor Sie mit dieser Funktion können.</span><span class="sxs-lookup"><span data-stu-id="d20b7-225">Before using this feature, consider whether your clients will be able to parse the results.</span></span> <span data-ttu-id="d20b7-226">Es möglicherweise besser einfach, Zyklen aus dem Diagramm zu entfernen.</span><span class="sxs-lookup"><span data-stu-id="d20b7-226">It might be better simply to remove cycles from the graph.</span></span> <span data-ttu-id="d20b7-227">In diesem Beispiel ist die Verknüpfung aus der Mitarbeiter zur Abteilung z. B. wirklich nicht erforderlich.</span><span class="sxs-lookup"><span data-stu-id="d20b7-227">For example, the link from Employee back to Department is not really needed in this example.</span></span>


<span data-ttu-id="d20b7-228">Um Objektverweise in XML zu beizubehalten, müssen Sie zwei Optionen zur Verfügung.</span><span class="sxs-lookup"><span data-stu-id="d20b7-228">To preserve object references in XML, you have two options.</span></span> <span data-ttu-id="d20b7-229">Die einfachere Option besteht darin hinzufügen `[DataContract(IsReference=true)]` der Model-Klasse.</span><span class="sxs-lookup"><span data-stu-id="d20b7-229">The simpler option is to add `[DataContract(IsReference=true)]` to your model class.</span></span> <span data-ttu-id="d20b7-230">Die *IsReference* Parameter kann Objektverweise.</span><span class="sxs-lookup"><span data-stu-id="d20b7-230">The *IsReference* parameter enables object references.</span></span> <span data-ttu-id="d20b7-231">Beachten Sie, dass **DataContract** Serialisierung teilnehmen, macht, sodass Sie auch hinzufügen müssen **DataMember** -Attribute auf die Eigenschaften:</span><span class="sxs-lookup"><span data-stu-id="d20b7-231">Remember that **DataContract** makes serialization opt-in, so you will also need to add **DataMember** attributes to the properties:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample20.cs)]

<span data-ttu-id="d20b7-232">Jetzt XML-Code ähnelt das Formatierungsprogramm erzeugt, die folgenden:</span><span class="sxs-lookup"><span data-stu-id="d20b7-232">Now the formatter will produce XML similar to following:</span></span>

[!code-xml[Main](json-and-xml-serialization/samples/sample21.xml)]

<span data-ttu-id="d20b7-233">Wenn Sie Attribute in Ihrer Modellklasse vermeiden möchten, steht eine andere Option: Erstellen Sie eine neue typspezifische **DataContractSerializer** -Instanz, und legen Sie *PreserveObjectReferences* zu **"true"**  im Konstruktor.</span><span class="sxs-lookup"><span data-stu-id="d20b7-233">If you want to avoid attributes on your model class, there is another option: Create a new type-specific **DataContractSerializer** instance and set *preserveObjectReferences* to **true** in the constructor.</span></span> <span data-ttu-id="d20b7-234">Legen Sie dann diese Instanz als Serialisierungsprogramm pro Typ, für die XML-medientypformatierer.</span><span class="sxs-lookup"><span data-stu-id="d20b7-234">Then set this instance as a per-type serializer on the XML media-type formatter.</span></span> <span data-ttu-id="d20b7-235">Der folgende Code zeigt, wie Sie dies durchführen:</span><span class="sxs-lookup"><span data-stu-id="d20b7-235">The following code show how to do this:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample22.cs?highlight=3)]

<a id="testing_object_serialization"></a>
## <a name="testing-object-serialization"></a><span data-ttu-id="d20b7-236">Testen die Serialisierung von Objekten</span><span class="sxs-lookup"><span data-stu-id="d20b7-236">Testing Object Serialization</span></span>

<span data-ttu-id="d20b7-237">Beim Entwerfen Ihrer Web-API ist es hilfreich, testen, wie Ihre Datenobjekte serialisiert werden.</span><span class="sxs-lookup"><span data-stu-id="d20b7-237">As you design your web API, it is useful to test how your data objects will be serialized.</span></span> <span data-ttu-id="d20b7-238">Dies ist möglich, ohne einen Controller oder eine Controlleraktion aufgerufen.</span><span class="sxs-lookup"><span data-stu-id="d20b7-238">You can do this without creating a controller or invoking a controller action.</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample23.cs)]
