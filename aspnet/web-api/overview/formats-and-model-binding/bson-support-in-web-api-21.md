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
<a name="bson-support-in-aspnet-web-api-21"></a>Unterstützung von BSON in ASP.NET Web-API 2.1
====================
durch [Mike Wasson](https://github.com/MikeWasson)

Web-API 2.1 wird Unterstützung für BSON eingeführt. In diesem Thema wird gezeigt, wie BSON in Ihrer Web-API-Controller (serverseitig) und in einer .NET-Client-App verwenden.

## <a name="what-is-bson"></a>Was ist BSON?

[BSON](http://bsonspec.org/) ist eine binäre Serialisierung-Format. "BSON" steht für "Binary JSON", aber BSON und JSON serialisiert werden sehr unterschiedlich. BSON ist "JSON-Like", da Objekte als Name / Wert-Paare, ähnlich wie JSON dargestellt werden. Im Gegensatz zu JSON werden numerische Datentypen als Bytes, die keine Zeichenfolgen gespeichert.

BSON wurde entwickelt, einfache, leicht zu scannen und zum Codieren/decodieren schnell sein.

- BSON ist die Größe in JSON vergleichbar. Abhängig von den Daten möglicherweise eine Nutzlast BSON kleiner oder größer als ein JSON-Nutzlast. Für die Serialisierung von Binärdaten, z. B. eine Bilddatei ist BSON kleiner als JSON, da die Binärdaten nicht base64-codiert ist.
- BSON Dokumente sind einfach zu überprüfen, da die Elemente eines Feldes mit Länge vorangestellt werden, sodass ein Parser Elemente überspringen kann, ohne sie decodieren.
- Codierung und Decodierung von sind effizient, da numerische Datentypen als Zahlen und keine Zeichenfolgen gespeichert werden.

Native Clients, z. B. .NET Client-apps nutzen BSON anstelle textbasierte Formate wie JSON oder XML. Für Browser-Clients sollten Sie wahrscheinlich mit JSON, bleiben, da JavaScript den JSON-Nutzlast direkt konvertiert werden kann.

Glücklicherweise Web-API verwendet [content-Aushandlung](content-negotiation.md), damit Ihre API beide Formate unterstützen und lassen Sie den Client auswählen kann.

## <a name="enabling-bson-on-the-server"></a>BSON auf dem Server aktivieren

Fügen Sie in Ihrer Web-API-Konfiguration der **BsonMediaTypeFormatter** der Formatierer-Auflistung.

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample1.cs)]

Jetzt der Client "Application/Bson" angefordert, verwenden Web-API den BSON-Formatierer.

Um anderen Medientypen BSON zuzuordnen, fügen Sie der SupportedMediaTypes-Auflistung. Der folgende Code fügt "application/vnd.contoso" zu den unterstützten Medientypen:

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample2.cs)]

## <a name="example-http-session"></a>Beispiel für HTTP-Sitzung

In diesem Beispiel verwenden wir die folgenden Modellklasse plus eine einfache Web-API-Controller:

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample3.cs)]

Ein Client möglicherweise die folgende HTTP-Anforderung senden:

[!code-console[Main](bson-support-in-web-api-21/samples/sample4.cmd)]

So sieht die Antwort aus:

[!code-console[Main](bson-support-in-web-api-21/samples/sample5.cmd)]

Hier habe ich die binären Daten mit ersetzt &quot;.&quot; Zeichen. Der folgende Screenshot Fiddler zeigt die unformatierten hexadezimale Werte.

[![](bson-support-in-web-api-21/_static/image2.png)](bson-support-in-web-api-21/_static/image1.png)

## <a name="using-bson-with-httpclient"></a>Verwenden von BSON mit "HttpClient"

.NET Clients-apps können das Formatierungsprogramm BSON mit **"HttpClient"**. Weitere Informationen zu **"HttpClient"**, finden Sie unter [ein Web-API aus einer .NET Client aufrufen](../advanced/calling-a-web-api-from-a-net-client.md).

Der folgende Code sendet eine GET-Anforderung, die BSON akzeptiert, und klicken Sie dann deserialisiert die BSON-Nutzlast in der Antwort.

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample6.cs)]

Festlegen des Accept-Headers auf "Application/Bson" BSON vom Server anfordern:

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample7.cs)]

Verwenden Sie zum Deserialisieren des Antworttexts die **BsonMediaTypeFormatter**. Dieser Formatierer ist nicht in der standardauflistung Formatierer, daher müssen Sie es angeben, wenn Sie den Antworttext lesen:

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample8.cs)]

Im nächste Beispiel wird gezeigt, wie eine POST-Anforderung gesendet, die BSON enthält.

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample9.cs)]

Ein großer Teil dieser Code ist im vorherigen Beispiel identisch. Aber in der **PostAsync** -Methode, geben Sie **BsonMediaTypeFormatter** als das Formatierungsprogramm:

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample10.cs)]

## <a name="serializing-top-level-primitive-types"></a>Serialisieren von primitiven Typen oberster Ebene

Jedes Dokument BSON ist eine Liste von Schlüssel/Wert-Paaren. Der BSON-Spezifikation definiert weder eine Syntax für einen einzelnen raw-Wert, z. B. eine ganze Zahl oder Zeichenfolge serialisieren.

Zum Umgehen dieser Einschränkung können die **BsonMediaTypeFormatter** primitive Typen als Sonderfall behandelt. Vor dem Serialisieren, konvertiert sie den Wert in ein Schlüssel/Wert-Paar mit dem Schlüssel "Value" ein. Angenommen Sie, dass die API-Controller eine ganze Zahl zurückgibt:

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample11.cs)]

Vor dem Serialisieren konvertiert der BSON Formatierer das Ereignis in den folgenden Schlüssel/Wert-Paar:

[!code-json[Main](bson-support-in-web-api-21/samples/sample12.json)]

Wenn Sie deserialisieren, konvertiert der Formatierer die Daten auf den ursprünglichen Wert zurückzusetzen. Allerdings müssen mit einer anderen BSON Parser Clients in diesem Fall, wenn Ihre Web-API die unformatierte Werte zurückgibt. Im Allgemeinen sollten Sie erwägen, strukturierte Daten, anstatt unformatierte Werte zurückgeben.

## <a name="additional-resources"></a>Zusätzliche Ressourcen

[Beispiel für eine Web-API BSON](https://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/BSONSample/)

[Medienformatierer](media-formatters.md)
