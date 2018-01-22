---
uid: web-api/overview/formats-and-model-binding/bson-support-in-web-api-21
title: "Unterstützung von BSON in ASP.NET Web-API 2.1 | Microsoft Docs"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/20/2014
ms.topic: article
ms.assetid: ce11b017-0ca6-4376-aa9d-a7f3288101de
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/formats-and-model-binding/bson-support-in-web-api-21
msc.type: authoredcontent
ms.openlocfilehash: 53ad705fad6d2225cecca4d73355bd6ebfcf56d5
ms.sourcegitcommit: 459cb3289741a3f46325e605a617dc926ee0563d
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/22/2018
---
<a name="bson-support-in-aspnet-web-api-21"></a><span data-ttu-id="bd6d0-102">Unterstützung von BSON in ASP.NET Web-API 2.1</span><span class="sxs-lookup"><span data-stu-id="bd6d0-102">BSON Support in ASP.NET Web API 2.1</span></span>
====================
<span data-ttu-id="bd6d0-103">durch [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="bd6d0-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="bd6d0-104">Web-API 2.1 wird Unterstützung für BSON eingeführt.</span><span class="sxs-lookup"><span data-stu-id="bd6d0-104">Web API 2.1 introduces support for BSON.</span></span> <span data-ttu-id="bd6d0-105">In diesem Thema wird gezeigt, wie BSON in Ihrer Web-API-Controller (serverseitig) und in einer .NET-Client-App verwenden.</span><span class="sxs-lookup"><span data-stu-id="bd6d0-105">This topic shows how to use BSON in your Web API controller (server side) and in a .NET client app.</span></span>

## <a name="what-is-bson"></a><span data-ttu-id="bd6d0-106">Was ist BSON?</span><span class="sxs-lookup"><span data-stu-id="bd6d0-106">What is BSON?</span></span>

<span data-ttu-id="bd6d0-107">[BSON](http://bsonspec.org/) ist eine binäre Serialisierung-Format.</span><span class="sxs-lookup"><span data-stu-id="bd6d0-107">[BSON](http://bsonspec.org/) is a binary serialization format.</span></span> <span data-ttu-id="bd6d0-108">"BSON" steht für "Binary JSON", aber BSON und JSON serialisiert werden sehr unterschiedlich.</span><span class="sxs-lookup"><span data-stu-id="bd6d0-108">"BSON" stands for "Binary JSON", but BSON and JSON are serialized very differently.</span></span> <span data-ttu-id="bd6d0-109">BSON ist "JSON-Like", da Objekte als Name / Wert-Paare, ähnlich wie JSON dargestellt werden.</span><span class="sxs-lookup"><span data-stu-id="bd6d0-109">BSON is "JSON-like", because objects are represented as name-value pairs, similar to JSON.</span></span> <span data-ttu-id="bd6d0-110">Im Gegensatz zu JSON werden numerische Datentypen als Bytes, die keine Zeichenfolgen gespeichert.</span><span class="sxs-lookup"><span data-stu-id="bd6d0-110">Unlike JSON, numeric data types are stored as bytes, not strings</span></span>

<span data-ttu-id="bd6d0-111">BSON wurde entwickelt, einfache, leicht zu scannen und zum Codieren/decodieren schnell sein.</span><span class="sxs-lookup"><span data-stu-id="bd6d0-111">BSON was designed to be lightweight, easy to scan, and fast to encode/decode.</span></span>

- <span data-ttu-id="bd6d0-112">BSON ist die Größe in JSON vergleichbar.</span><span class="sxs-lookup"><span data-stu-id="bd6d0-112">BSON is comparable in size to JSON.</span></span> <span data-ttu-id="bd6d0-113">Abhängig von den Daten möglicherweise eine Nutzlast BSON kleiner oder größer als ein JSON-Nutzlast.</span><span class="sxs-lookup"><span data-stu-id="bd6d0-113">Depending on the data, a BSON payload may be smaller or larger than a JSON payload.</span></span> <span data-ttu-id="bd6d0-114">Für die Serialisierung von Binärdaten, z. B. eine Bilddatei ist BSON kleiner als JSON, da die Binärdaten nicht base64-codiert ist.</span><span class="sxs-lookup"><span data-stu-id="bd6d0-114">For serializing binary data, such as an image file, BSON is smaller than JSON, because the binary data is not base64-encoded.</span></span>
- <span data-ttu-id="bd6d0-115">BSON Dokumente sind einfach zu überprüfen, da die Elemente eines Feldes mit Länge vorangestellt werden, sodass ein Parser Elemente überspringen kann, ohne sie decodieren.</span><span class="sxs-lookup"><span data-stu-id="bd6d0-115">BSON documents are easy to scan because elements are prefixed with a length field, so a parser can skip elements without decoding them.</span></span>
- <span data-ttu-id="bd6d0-116">Codierung und Decodierung von sind effizient, da numerische Datentypen als Zahlen und keine Zeichenfolgen gespeichert werden.</span><span class="sxs-lookup"><span data-stu-id="bd6d0-116">Encoding and decoding are efficient, because numeric data types are stored as numbers, not strings.</span></span>

<span data-ttu-id="bd6d0-117">Native Clients, z. B. .NET Client-apps nutzen BSON anstelle textbasierte Formate wie JSON oder XML.</span><span class="sxs-lookup"><span data-stu-id="bd6d0-117">Native clients, such as .NET client apps, can benefit from using BSON in place of text-based formats such as JSON or XML.</span></span> <span data-ttu-id="bd6d0-118">Für Browser-Clients sollten Sie wahrscheinlich mit JSON, bleiben, da JavaScript den JSON-Nutzlast direkt konvertiert werden kann.</span><span class="sxs-lookup"><span data-stu-id="bd6d0-118">For browser clients, you will probably want to stick with JSON, because JavaScript can directly convert the JSON payload.</span></span>

<span data-ttu-id="bd6d0-119">Glücklicherweise Web-API verwendet [content-Aushandlung](content-negotiation.md), damit Ihre API beide Formate unterstützen und lassen Sie den Client auswählen kann.</span><span class="sxs-lookup"><span data-stu-id="bd6d0-119">Fortunately, Web API uses [content negotiation](content-negotiation.md), so your API can support both formats and let the client choose.</span></span>

## <a name="enabling-bson-on-the-server"></a><span data-ttu-id="bd6d0-120">BSON auf dem Server aktivieren</span><span class="sxs-lookup"><span data-stu-id="bd6d0-120">Enabling BSON on the Server</span></span>

<span data-ttu-id="bd6d0-121">Fügen Sie in Ihrer Web-API-Konfiguration der **BsonMediaTypeFormatter** der Formatierer-Auflistung.</span><span class="sxs-lookup"><span data-stu-id="bd6d0-121">In your Web API configuration, add the **BsonMediaTypeFormatter** to the formatters collection.</span></span>

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample1.cs)]

<span data-ttu-id="bd6d0-122">Jetzt der Client "Application/Bson" angefordert, verwenden Web-API den BSON-Formatierer.</span><span class="sxs-lookup"><span data-stu-id="bd6d0-122">Now if the client requests "application/bson", Web API will use the BSON formatter.</span></span>

<span data-ttu-id="bd6d0-123">Um anderen Medientypen BSON zuzuordnen, fügen Sie der SupportedMediaTypes-Auflistung.</span><span class="sxs-lookup"><span data-stu-id="bd6d0-123">To associate BSON with other media types, add them to the SupportedMediaTypes collection.</span></span> <span data-ttu-id="bd6d0-124">Der folgende Code fügt "application/vnd.contoso" zu den unterstützten Medientypen:</span><span class="sxs-lookup"><span data-stu-id="bd6d0-124">The following code adds "application/vnd.contoso" to the supported media types:</span></span>

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample2.cs)]

## <a name="example-http-session"></a><span data-ttu-id="bd6d0-125">Beispiel für HTTP-Sitzung</span><span class="sxs-lookup"><span data-stu-id="bd6d0-125">Example HTTP Session</span></span>

<span data-ttu-id="bd6d0-126">In diesem Beispiel verwenden wir die folgenden Modellklasse plus eine einfache Web-API-Controller:</span><span class="sxs-lookup"><span data-stu-id="bd6d0-126">For this example, we'll use the following model class plus a simple Web API controller:</span></span>

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample3.cs)]

<span data-ttu-id="bd6d0-127">Ein Client möglicherweise die folgende HTTP-Anforderung senden:</span><span class="sxs-lookup"><span data-stu-id="bd6d0-127">A client might send the following HTTP request:</span></span>

[!code-console[Main](bson-support-in-web-api-21/samples/sample4.cmd)]

<span data-ttu-id="bd6d0-128">So sieht die Antwort aus:</span><span class="sxs-lookup"><span data-stu-id="bd6d0-128">Here is the response:</span></span>

[!code-console[Main](bson-support-in-web-api-21/samples/sample5.cmd)]

<span data-ttu-id="bd6d0-129">Hier habe ich die binären Daten mit ersetzt &quot;.&quot; Zeichen.</span><span class="sxs-lookup"><span data-stu-id="bd6d0-129">Here I've replaced the binary data with &quot;.&quot; characters.</span></span> <span data-ttu-id="bd6d0-130">Der folgende Screenshot Fiddler zeigt die unformatierten hexadezimale Werte.</span><span class="sxs-lookup"><span data-stu-id="bd6d0-130">The following screen shot from Fiddler shows the raw hex values.</span></span>

[![](bson-support-in-web-api-21/_static/image2.png)](bson-support-in-web-api-21/_static/image1.png)

## <a name="using-bson-with-httpclient"></a><span data-ttu-id="bd6d0-131">Verwenden von BSON mit "HttpClient"</span><span class="sxs-lookup"><span data-stu-id="bd6d0-131">Using BSON with HttpClient</span></span>

<span data-ttu-id="bd6d0-132">.NET Clients-apps können das Formatierungsprogramm BSON mit **"HttpClient"**.</span><span class="sxs-lookup"><span data-stu-id="bd6d0-132">.NET clients apps can use the BSON formatter with **HttpClient**.</span></span> <span data-ttu-id="bd6d0-133">Weitere Informationen zu **"HttpClient"**, finden Sie unter [ein Web-API aus einer .NET Client aufrufen](../advanced/calling-a-web-api-from-a-net-client.md).</span><span class="sxs-lookup"><span data-stu-id="bd6d0-133">For more information about **HttpClient**, see [Calling a Web API From a .NET Client](../advanced/calling-a-web-api-from-a-net-client.md).</span></span>

<span data-ttu-id="bd6d0-134">Der folgende Code sendet eine GET-Anforderung, die BSON akzeptiert, und klicken Sie dann deserialisiert die BSON-Nutzlast in der Antwort.</span><span class="sxs-lookup"><span data-stu-id="bd6d0-134">The following code sends a GET request that accepts BSON, and then deserializes the BSON payload in the response.</span></span>

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample6.cs)]

<span data-ttu-id="bd6d0-135">Festlegen des Accept-Headers auf "Application/Bson" BSON vom Server anfordern:</span><span class="sxs-lookup"><span data-stu-id="bd6d0-135">To request BSON from the server, set the Accept header to "application/bson":</span></span>

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample7.cs)]

<span data-ttu-id="bd6d0-136">Verwenden Sie zum Deserialisieren des Antworttexts die **BsonMediaTypeFormatter**.</span><span class="sxs-lookup"><span data-stu-id="bd6d0-136">To deserialize the response body, use the **BsonMediaTypeFormatter**.</span></span> <span data-ttu-id="bd6d0-137">Dieser Formatierer ist nicht in der standardauflistung Formatierer, daher müssen Sie es angeben, wenn Sie den Antworttext lesen:</span><span class="sxs-lookup"><span data-stu-id="bd6d0-137">This formatter is not in the default formatters collection, so you have to specify it when you read the response body:</span></span>

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample8.cs)]

<span data-ttu-id="bd6d0-138">Im nächste Beispiel wird gezeigt, wie eine POST-Anforderung gesendet, die BSON enthält.</span><span class="sxs-lookup"><span data-stu-id="bd6d0-138">The next example shows how to send a POST request that contains BSON.</span></span>

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample9.cs)]

<span data-ttu-id="bd6d0-139">Ein großer Teil dieser Code ist im vorherigen Beispiel identisch.</span><span class="sxs-lookup"><span data-stu-id="bd6d0-139">Much of this code is the same as the previous example.</span></span> <span data-ttu-id="bd6d0-140">Aber in der **PostAsync** -Methode, geben Sie **BsonMediaTypeFormatter** als das Formatierungsprogramm:</span><span class="sxs-lookup"><span data-stu-id="bd6d0-140">But in the **PostAsync** method, specify **BsonMediaTypeFormatter** as the formatter:</span></span>

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample10.cs)]

## <a name="serializing-top-level-primitive-types"></a><span data-ttu-id="bd6d0-141">Serialisieren von primitiven Typen oberster Ebene</span><span class="sxs-lookup"><span data-stu-id="bd6d0-141">Serializing Top-Level Primitive Types</span></span>

<span data-ttu-id="bd6d0-142">Jedes Dokument BSON ist eine Liste von Schlüssel/Wert-Paaren. Der BSON-Spezifikation definiert weder eine Syntax für einen einzelnen raw-Wert, z. B. eine ganze Zahl oder Zeichenfolge serialisieren.</span><span class="sxs-lookup"><span data-stu-id="bd6d0-142">Every BSON document is a list of key/value pairs.The BSON specification does not define a syntax for serializing a single raw value, such as an integer or string.</span></span>

<span data-ttu-id="bd6d0-143">Zum Umgehen dieser Einschränkung können die **BsonMediaTypeFormatter** primitive Typen als Sonderfall behandelt.</span><span class="sxs-lookup"><span data-stu-id="bd6d0-143">To work around this limitation, the **BsonMediaTypeFormatter** treats primitive types as a special case.</span></span> <span data-ttu-id="bd6d0-144">Vor dem Serialisieren, konvertiert sie den Wert in ein Schlüssel/Wert-Paar mit dem Schlüssel "Value" ein.</span><span class="sxs-lookup"><span data-stu-id="bd6d0-144">Before serializing, it converts the value into a key/value pair with the key "Value".</span></span> <span data-ttu-id="bd6d0-145">Angenommen Sie, dass die API-Controller eine ganze Zahl zurückgibt:</span><span class="sxs-lookup"><span data-stu-id="bd6d0-145">For example, suppose your API controller returns an integer:</span></span>

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample11.cs)]

<span data-ttu-id="bd6d0-146">Vor dem Serialisieren konvertiert der BSON Formatierer das Ereignis in den folgenden Schlüssel/Wert-Paar:</span><span class="sxs-lookup"><span data-stu-id="bd6d0-146">Before serializing, the BSON formatter converts this to the following key/value pair:</span></span>

[!code-json[Main](bson-support-in-web-api-21/samples/sample12.json)]

<span data-ttu-id="bd6d0-147">Wenn Sie deserialisieren, konvertiert der Formatierer die Daten auf den ursprünglichen Wert zurückzusetzen.</span><span class="sxs-lookup"><span data-stu-id="bd6d0-147">When you deserialize, the formatter converts the data back to the original value.</span></span> <span data-ttu-id="bd6d0-148">Allerdings müssen mit einer anderen BSON Parser Clients in diesem Fall, wenn Ihre Web-API die unformatierte Werte zurückgibt.</span><span class="sxs-lookup"><span data-stu-id="bd6d0-148">However, clients using a different BSON parser will need to handle this case, if your web API returns raw values.</span></span> <span data-ttu-id="bd6d0-149">Im Allgemeinen sollten Sie erwägen, strukturierte Daten, anstatt unformatierte Werte zurückgeben.</span><span class="sxs-lookup"><span data-stu-id="bd6d0-149">In general, you should consider returning structured data, rather than raw values.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="bd6d0-150">Zusätzliche Ressourcen</span><span class="sxs-lookup"><span data-stu-id="bd6d0-150">Additional Resources</span></span>

[<span data-ttu-id="bd6d0-151">Beispiel für eine Web-API BSON</span><span class="sxs-lookup"><span data-stu-id="bd6d0-151">Web API BSON Sample</span></span>](https://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/BSONSample/)

[<span data-ttu-id="bd6d0-152">Medienformatierer</span><span class="sxs-lookup"><span data-stu-id="bd6d0-152">Media Formatters</span></span>](media-formatters.md)
