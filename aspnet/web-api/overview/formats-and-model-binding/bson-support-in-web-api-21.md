---
uid: web-api/overview/formats-and-model-binding/bson-support-in-web-api-21
title: BSON-Unterstützung in ASP.NET Web-API 2.1 | Microsoft-Dokumentation
author: MikeWasson
description: ''
ms.author: riande
ms.date: 01/20/2014
ms.assetid: ce11b017-0ca6-4376-aa9d-a7f3288101de
msc.legacyurl: /web-api/overview/formats-and-model-binding/bson-support-in-web-api-21
msc.type: authoredcontent
ms.openlocfilehash: 709fb0266c0725176358a1bd0d08b3e07fa6e2a6
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2018
ms.locfileid: "41836533"
---
<a name="bson-support-in-aspnet-web-api-21"></a><span data-ttu-id="729d2-102">BSON-Unterstützung in ASP.NET Web-API 2.1</span><span class="sxs-lookup"><span data-stu-id="729d2-102">BSON Support in ASP.NET Web API 2.1</span></span>
====================
<span data-ttu-id="729d2-103">durch [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="729d2-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="729d2-104">Web-API 2.1 führt die Unterstützung für BSON.</span><span class="sxs-lookup"><span data-stu-id="729d2-104">Web API 2.1 introduces support for BSON.</span></span> <span data-ttu-id="729d2-105">In diesem Thema veranschaulicht, wie BSON in Ihren Web-API-Controller (serverseitig) und einer .NET Client-app verwenden.</span><span class="sxs-lookup"><span data-stu-id="729d2-105">This topic shows how to use BSON in your Web API controller (server side) and in a .NET client app.</span></span>

## <a name="what-is-bson"></a><span data-ttu-id="729d2-106">Was ist BSON?</span><span class="sxs-lookup"><span data-stu-id="729d2-106">What is BSON?</span></span>

<span data-ttu-id="729d2-107">[BSON](http://bsonspec.org/) ist ein binäres Serialisierungsformat.</span><span class="sxs-lookup"><span data-stu-id="729d2-107">[BSON](http://bsonspec.org/) is a binary serialization format.</span></span> <span data-ttu-id="729d2-108">"BSON" steht für "Binary JSON", aber BSON und JSON-Serialisierung sind sehr unterschiedlich.</span><span class="sxs-lookup"><span data-stu-id="729d2-108">"BSON" stands for "Binary JSON", but BSON and JSON are serialized very differently.</span></span> <span data-ttu-id="729d2-109">BSON ist "JSON-gefällt mir", da Objekte, die als Name / Wert-Paare, etwa JSON dargestellt werden.</span><span class="sxs-lookup"><span data-stu-id="729d2-109">BSON is "JSON-like", because objects are represented as name-value pairs, similar to JSON.</span></span> <span data-ttu-id="729d2-110">Im Gegensatz zu JSON werden numerische Datentypen als Bytes, die keine Zeichenfolgen gespeichert.</span><span class="sxs-lookup"><span data-stu-id="729d2-110">Unlike JSON, numeric data types are stored as bytes, not strings</span></span>

<span data-ttu-id="729d2-111">BSON wurde entwickelt, um einfache, leicht zu scannen und schnell codieren/decodieren werden.</span><span class="sxs-lookup"><span data-stu-id="729d2-111">BSON was designed to be lightweight, easy to scan, and fast to encode/decode.</span></span>

- <span data-ttu-id="729d2-112">BSON ist die Größe mit JSON vergleichbar.</span><span class="sxs-lookup"><span data-stu-id="729d2-112">BSON is comparable in size to JSON.</span></span> <span data-ttu-id="729d2-113">Abhängig von den Daten möglicherweise eine BSON-Nutzlast kleiner oder größer als eine JSON-Nutzlast.</span><span class="sxs-lookup"><span data-stu-id="729d2-113">Depending on the data, a BSON payload may be smaller or larger than a JSON payload.</span></span> <span data-ttu-id="729d2-114">Für das Serialisieren von Binärdaten, z. B. eine Bilddatei, liegt BSON kleiner ist als JSON, die Binärdaten nicht base64-codiert ist.</span><span class="sxs-lookup"><span data-stu-id="729d2-114">For serializing binary data, such as an image file, BSON is smaller than JSON, because the binary data is not base64-encoded.</span></span>
- <span data-ttu-id="729d2-115">BSON-Dokumente sind leicht zu scannen, da ein Längenfeld, Elementen vorangestellt werden, also ein Parser ohne Decodierung diese Elemente überspringen.</span><span class="sxs-lookup"><span data-stu-id="729d2-115">BSON documents are easy to scan because elements are prefixed with a length field, so a parser can skip elements without decoding them.</span></span>
- <span data-ttu-id="729d2-116">Codieren und Decodieren von sind effizient, da numerische Datentypen als Zahlen, die keine Zeichenfolgen gespeichert werden.</span><span class="sxs-lookup"><span data-stu-id="729d2-116">Encoding and decoding are efficient, because numeric data types are stored as numbers, not strings.</span></span>

<span data-ttu-id="729d2-117">Native Clients, z. B. .NET Client-apps nutzen BSON anstelle textbasierte Formate wie JSON oder XML.</span><span class="sxs-lookup"><span data-stu-id="729d2-117">Native clients, such as .NET client apps, can benefit from using BSON in place of text-based formats such as JSON or XML.</span></span> <span data-ttu-id="729d2-118">Für Browser-Clients möchten wahrscheinlich Sie bleiben Sie mit JSON-Code, da die JSON-Nutzlast direkt in JavaScript konvertieren kann.</span><span class="sxs-lookup"><span data-stu-id="729d2-118">For browser clients, you will probably want to stick with JSON, because JavaScript can directly convert the JSON payload.</span></span>

<span data-ttu-id="729d2-119">Zum Glück, Web-API verwendet [inhaltsaushandlung](content-negotiation.md), damit Ihre API beide Formate unterstützen und lassen den Client auswählen kann.</span><span class="sxs-lookup"><span data-stu-id="729d2-119">Fortunately, Web API uses [content negotiation](content-negotiation.md), so your API can support both formats and let the client choose.</span></span>

## <a name="enabling-bson-on-the-server"></a><span data-ttu-id="729d2-120">BSON, auf dem Server aktivieren</span><span class="sxs-lookup"><span data-stu-id="729d2-120">Enabling BSON on the Server</span></span>

<span data-ttu-id="729d2-121">Fügen Sie in Ihrer Web-API-Konfiguration können die **BsonMediaTypeFormatter** der Formatierer-Auflistung.</span><span class="sxs-lookup"><span data-stu-id="729d2-121">In your Web API configuration, add the **BsonMediaTypeFormatter** to the formatters collection.</span></span>

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample1.cs)]

<span data-ttu-id="729d2-122">Nun, wenn der Client "Application/Bson" anfordert, verwendet Web-API den BSON-Formatierer.</span><span class="sxs-lookup"><span data-stu-id="729d2-122">Now if the client requests "application/bson", Web API will use the BSON formatter.</span></span>

<span data-ttu-id="729d2-123">Um andere Medientypen BSON zuzuordnen, fügen sie der Auflistung SupportedMediaTypes hinzu.</span><span class="sxs-lookup"><span data-stu-id="729d2-123">To associate BSON with other media types, add them to the SupportedMediaTypes collection.</span></span> <span data-ttu-id="729d2-124">Der folgende Code fügt "application/vnd.contoso" zu den unterstützten Medientypen:</span><span class="sxs-lookup"><span data-stu-id="729d2-124">The following code adds "application/vnd.contoso" to the supported media types:</span></span>

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample2.cs)]

## <a name="example-http-session"></a><span data-ttu-id="729d2-125">Beispiel für HTTP-Sitzung</span><span class="sxs-lookup"><span data-stu-id="729d2-125">Example HTTP Session</span></span>

<span data-ttu-id="729d2-126">In diesem Beispiel verwenden wir die folgende Modellklasse sowie eine einfache Web-API-Controller:</span><span class="sxs-lookup"><span data-stu-id="729d2-126">For this example, we'll use the following model class plus a simple Web API controller:</span></span>

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample3.cs)]

<span data-ttu-id="729d2-127">Ein Client kann die folgende HTTP-Anforderung senden:</span><span class="sxs-lookup"><span data-stu-id="729d2-127">A client might send the following HTTP request:</span></span>

[!code-console[Main](bson-support-in-web-api-21/samples/sample4.cmd)]

<span data-ttu-id="729d2-128">Hier ist die Antwort aus:</span><span class="sxs-lookup"><span data-stu-id="729d2-128">Here is the response:</span></span>

[!code-console[Main](bson-support-in-web-api-21/samples/sample5.cmd)]

<span data-ttu-id="729d2-129">Hier habe ich lediglich die binären Daten mit &quot;.&quot; Zeichen.</span><span class="sxs-lookup"><span data-stu-id="729d2-129">Here I've replaced the binary data with &quot;.&quot; characters.</span></span> <span data-ttu-id="729d2-130">Der folgende Screenshot Fiddler zeigt die unformatierten hexadezimale Werte.</span><span class="sxs-lookup"><span data-stu-id="729d2-130">The following screen shot from Fiddler shows the raw hex values.</span></span>

[![](bson-support-in-web-api-21/_static/image2.png)](bson-support-in-web-api-21/_static/image1.png)

## <a name="using-bson-with-httpclient"></a><span data-ttu-id="729d2-131">Verwenden von BSON mit "HttpClient"</span><span class="sxs-lookup"><span data-stu-id="729d2-131">Using BSON with HttpClient</span></span>

<span data-ttu-id="729d2-132">Apps für .NET-Clients können das Formatierungsprogramm BSON mit **"HttpClient"**.</span><span class="sxs-lookup"><span data-stu-id="729d2-132">.NET clients apps can use the BSON formatter with **HttpClient**.</span></span> <span data-ttu-id="729d2-133">Weitere Informationen zu **"HttpClient"**, finden Sie unter [Aufrufen einer Web-API aus einer .NET Client](../advanced/calling-a-web-api-from-a-net-client.md).</span><span class="sxs-lookup"><span data-stu-id="729d2-133">For more information about **HttpClient**, see [Calling a Web API From a .NET Client](../advanced/calling-a-web-api-from-a-net-client.md).</span></span>

<span data-ttu-id="729d2-134">Der folgende Code sendet eine GET-Anforderung, die BSON akzeptiert und dann deserialisiert die BSON-Nutzlast in der Antwort.</span><span class="sxs-lookup"><span data-stu-id="729d2-134">The following code sends a GET request that accepts BSON, and then deserializes the BSON payload in the response.</span></span>

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample6.cs)]

<span data-ttu-id="729d2-135">Um BSON vom Server anzufordern, legen Sie den Accept-Header auf "Application/Bson":</span><span class="sxs-lookup"><span data-stu-id="729d2-135">To request BSON from the server, set the Accept header to "application/bson":</span></span>

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample7.cs)]

<span data-ttu-id="729d2-136">Verwenden Sie zum Deserialisieren des Antworttexts die **BsonMediaTypeFormatter**.</span><span class="sxs-lookup"><span data-stu-id="729d2-136">To deserialize the response body, use the **BsonMediaTypeFormatter**.</span></span> <span data-ttu-id="729d2-137">Dieses Formatierungsprogramm ist nicht in der Auflistung für den Standard-Formatierer, daher müssen Sie es angeben, wenn Sie den Antworttext lesen:</span><span class="sxs-lookup"><span data-stu-id="729d2-137">This formatter is not in the default formatters collection, so you have to specify it when you read the response body:</span></span>

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample8.cs)]

<span data-ttu-id="729d2-138">Im nächste Beispiel zeigt, wie Sie eine POST-Anforderung senden, die BSON enthält.</span><span class="sxs-lookup"><span data-stu-id="729d2-138">The next example shows how to send a POST request that contains BSON.</span></span>

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample9.cs)]

<span data-ttu-id="729d2-139">Ein großer Teil dieses Codes ist identisch mit dem vorherigen Beispiel.</span><span class="sxs-lookup"><span data-stu-id="729d2-139">Much of this code is the same as the previous example.</span></span> <span data-ttu-id="729d2-140">Aber in der **"postasync"** -Methode, geben Sie **BsonMediaTypeFormatter** als das Formatierungsprogramm:</span><span class="sxs-lookup"><span data-stu-id="729d2-140">But in the **PostAsync** method, specify **BsonMediaTypeFormatter** as the formatter:</span></span>

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample10.cs)]

## <a name="serializing-top-level-primitive-types"></a><span data-ttu-id="729d2-141">Serialisieren von primitiven Typen der obersten Ebene</span><span class="sxs-lookup"><span data-stu-id="729d2-141">Serializing Top-Level Primitive Types</span></span>

<span data-ttu-id="729d2-142">Jedes BSON-Dokument ist eine Liste von Schlüssel/Wert-Paaren. Die BSON-Spezifikation definiert keine Syntax für einen einzelnen raw-Wert, z. B. eine ganze Zahl oder eine Zeichenfolge serialisieren.</span><span class="sxs-lookup"><span data-stu-id="729d2-142">Every BSON document is a list of key/value pairs.The BSON specification does not define a syntax for serializing a single raw value, such as an integer or string.</span></span>

<span data-ttu-id="729d2-143">Zum Umgehen dieser Einschränkung können die **BsonMediaTypeFormatter** primitive Typen als Sonderfall behandelt.</span><span class="sxs-lookup"><span data-stu-id="729d2-143">To work around this limitation, the **BsonMediaTypeFormatter** treats primitive types as a special case.</span></span> <span data-ttu-id="729d2-144">Vor dem Serialisieren, wird der Wert in ein Schlüssel/Wert-Paar konvertiert, mit dem Schlüssel "Value".</span><span class="sxs-lookup"><span data-stu-id="729d2-144">Before serializing, it converts the value into a key/value pair with the key "Value".</span></span> <span data-ttu-id="729d2-145">Nehmen wir beispielsweise an, dass Ihre API-Controller eine ganze Zahl zurückgibt:</span><span class="sxs-lookup"><span data-stu-id="729d2-145">For example, suppose your API controller returns an integer:</span></span>

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample11.cs)]

<span data-ttu-id="729d2-146">Vor dem Serialisieren konvertiert der BSON-Formatierer das Ereignis in den folgenden Schlüssel/Wert-Paar:</span><span class="sxs-lookup"><span data-stu-id="729d2-146">Before serializing, the BSON formatter converts this to the following key/value pair:</span></span>

[!code-json[Main](bson-support-in-web-api-21/samples/sample12.json)]

<span data-ttu-id="729d2-147">Wenn Sie deserialisieren, konvertiert das Formatierungsprogramm die Daten auf den ursprünglichen Wert zurück.</span><span class="sxs-lookup"><span data-stu-id="729d2-147">When you deserialize, the formatter converts the data back to the original value.</span></span> <span data-ttu-id="729d2-148">Allerdings müssen mit einem anderen BSON-Parser Clients in diesem Fall, wenn Ihre Web-API die unformatierte Werte zurückgibt.</span><span class="sxs-lookup"><span data-stu-id="729d2-148">However, clients using a different BSON parser will need to handle this case, if your web API returns raw values.</span></span> <span data-ttu-id="729d2-149">Im Allgemeinen sollten Sie erwägen, zurückgeben, strukturierte Daten, anstatt Rohwerte.</span><span class="sxs-lookup"><span data-stu-id="729d2-149">In general, you should consider returning structured data, rather than raw values.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="729d2-150">Zusätzliche Ressourcen</span><span class="sxs-lookup"><span data-stu-id="729d2-150">Additional Resources</span></span>

[<span data-ttu-id="729d2-151">Web-API-BSON-Beispiel</span><span class="sxs-lookup"><span data-stu-id="729d2-151">Web API BSON Sample</span></span>](https://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/BSONSample/)

[<span data-ttu-id="729d2-152">Medienformatierer</span><span class="sxs-lookup"><span data-stu-id="729d2-152">Media Formatters</span></span>](media-formatters.md)
