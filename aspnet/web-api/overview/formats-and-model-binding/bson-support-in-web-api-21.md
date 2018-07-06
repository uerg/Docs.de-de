---
uid: web-api/overview/formats-and-model-binding/bson-support-in-web-api-21
title: BSON-Unterstützung in ASP.NET Web-API 2.1 | Microsoft-Dokumentation
author: MikeWasson
description: ''
ms.author: aspnetcontent
ms.date: 01/20/2014
ms.assetid: ce11b017-0ca6-4376-aa9d-a7f3288101de
msc.legacyurl: /web-api/overview/formats-and-model-binding/bson-support-in-web-api-21
msc.type: authoredcontent
ms.openlocfilehash: 7b9a0c2def4703f6fad65790dfe4227117189933
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2018
ms.locfileid: "37839879"
---
<a name="bson-support-in-aspnet-web-api-21"></a>BSON-Unterstützung in ASP.NET Web-API 2.1
====================
durch [Mike Wasson](https://github.com/MikeWasson)

Web-API 2.1 führt die Unterstützung für BSON. In diesem Thema veranschaulicht, wie BSON in Ihren Web-API-Controller (serverseitig) und einer .NET Client-app verwenden.

## <a name="what-is-bson"></a>Was ist BSON?

[BSON](http://bsonspec.org/) ist ein binäres Serialisierungsformat. "BSON" steht für "Binary JSON", aber BSON und JSON-Serialisierung sind sehr unterschiedlich. BSON ist "JSON-gefällt mir", da Objekte, die als Name / Wert-Paare, etwa JSON dargestellt werden. Im Gegensatz zu JSON werden numerische Datentypen als Bytes, die keine Zeichenfolgen gespeichert.

BSON wurde entwickelt, um einfache, leicht zu scannen und schnell codieren/decodieren werden.

- BSON ist die Größe mit JSON vergleichbar. Abhängig von den Daten möglicherweise eine BSON-Nutzlast kleiner oder größer als eine JSON-Nutzlast. Für das Serialisieren von Binärdaten, z. B. eine Bilddatei, liegt BSON kleiner ist als JSON, die Binärdaten nicht base64-codiert ist.
- BSON-Dokumente sind leicht zu scannen, da ein Längenfeld, Elementen vorangestellt werden, also ein Parser ohne Decodierung diese Elemente überspringen.
- Codieren und Decodieren von sind effizient, da numerische Datentypen als Zahlen, die keine Zeichenfolgen gespeichert werden.

Native Clients, z. B. .NET Client-apps nutzen BSON anstelle textbasierte Formate wie JSON oder XML. Für Browser-Clients möchten wahrscheinlich Sie bleiben Sie mit JSON-Code, da die JSON-Nutzlast direkt in JavaScript konvertieren kann.

Zum Glück, Web-API verwendet [inhaltsaushandlung](content-negotiation.md), damit Ihre API beide Formate unterstützen und lassen den Client auswählen kann.

## <a name="enabling-bson-on-the-server"></a>BSON, auf dem Server aktivieren

Fügen Sie in Ihrer Web-API-Konfiguration können die **BsonMediaTypeFormatter** der Formatierer-Auflistung.

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample1.cs)]

Nun, wenn der Client "Application/Bson" anfordert, verwendet Web-API den BSON-Formatierer.

Um andere Medientypen BSON zuzuordnen, fügen sie der Auflistung SupportedMediaTypes hinzu. Der folgende Code fügt "application/vnd.contoso" zu den unterstützten Medientypen:

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample2.cs)]

## <a name="example-http-session"></a>Beispiel für HTTP-Sitzung

In diesem Beispiel verwenden wir die folgende Modellklasse sowie eine einfache Web-API-Controller:

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample3.cs)]

Ein Client kann die folgende HTTP-Anforderung senden:

[!code-console[Main](bson-support-in-web-api-21/samples/sample4.cmd)]

Hier ist die Antwort aus:

[!code-console[Main](bson-support-in-web-api-21/samples/sample5.cmd)]

Hier habe ich lediglich die binären Daten mit &quot;.&quot; Zeichen. Der folgende Screenshot Fiddler zeigt die unformatierten hexadezimale Werte.

[![](bson-support-in-web-api-21/_static/image2.png)](bson-support-in-web-api-21/_static/image1.png)

## <a name="using-bson-with-httpclient"></a>Verwenden von BSON mit "HttpClient"

Apps für .NET-Clients können das Formatierungsprogramm BSON mit **"HttpClient"**. Weitere Informationen zu **"HttpClient"**, finden Sie unter [Aufrufen einer Web-API aus einer .NET Client](../advanced/calling-a-web-api-from-a-net-client.md).

Der folgende Code sendet eine GET-Anforderung, die BSON akzeptiert und dann deserialisiert die BSON-Nutzlast in der Antwort.

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample6.cs)]

Um BSON vom Server anzufordern, legen Sie den Accept-Header auf "Application/Bson":

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample7.cs)]

Verwenden Sie zum Deserialisieren des Antworttexts die **BsonMediaTypeFormatter**. Dieses Formatierungsprogramm ist nicht in der Auflistung für den Standard-Formatierer, daher müssen Sie es angeben, wenn Sie den Antworttext lesen:

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample8.cs)]

Im nächste Beispiel zeigt, wie Sie eine POST-Anforderung senden, die BSON enthält.

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample9.cs)]

Ein großer Teil dieses Codes ist identisch mit dem vorherigen Beispiel. Aber in der **"postasync"** -Methode, geben Sie **BsonMediaTypeFormatter** als das Formatierungsprogramm:

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample10.cs)]

## <a name="serializing-top-level-primitive-types"></a>Serialisieren von primitiven Typen der obersten Ebene

Jedes BSON-Dokument ist eine Liste von Schlüssel/Wert-Paaren. Die BSON-Spezifikation definiert keine Syntax für einen einzelnen raw-Wert, z. B. eine ganze Zahl oder eine Zeichenfolge serialisieren.

Zum Umgehen dieser Einschränkung können die **BsonMediaTypeFormatter** primitive Typen als Sonderfall behandelt. Vor dem Serialisieren, wird der Wert in ein Schlüssel/Wert-Paar konvertiert, mit dem Schlüssel "Value". Nehmen wir beispielsweise an, dass Ihre API-Controller eine ganze Zahl zurückgibt:

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample11.cs)]

Vor dem Serialisieren konvertiert der BSON-Formatierer das Ereignis in den folgenden Schlüssel/Wert-Paar:

[!code-json[Main](bson-support-in-web-api-21/samples/sample12.json)]

Wenn Sie deserialisieren, konvertiert das Formatierungsprogramm die Daten auf den ursprünglichen Wert zurück. Allerdings müssen mit einem anderen BSON-Parser Clients in diesem Fall, wenn Ihre Web-API die unformatierte Werte zurückgibt. Im Allgemeinen sollten Sie erwägen, zurückgeben, strukturierte Daten, anstatt Rohwerte.

## <a name="additional-resources"></a>Zusätzliche Ressourcen

[Web-API-BSON-Beispiel](https://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/BSONSample/)

[Medienformatierer](media-formatters.md)
