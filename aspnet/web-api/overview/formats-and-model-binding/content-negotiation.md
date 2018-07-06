---
uid: web-api/overview/formats-and-model-binding/content-negotiation
title: Inhaltsaushandlung in ASP.NET Web-API | Microsoft-Dokumentation
author: MikeWasson
description: Beschreibt, wie HTTP-Aushandlung von Inhalten von ASP.NET Web-API implementiert.
ms.author: aspnetcontent
ms.date: 05/20/2012
ms.assetid: 0dd51b30-bf5a-419f-a1b7-2817ccca3c7d
msc.legacyurl: /web-api/overview/formats-and-model-binding/content-negotiation
msc.type: authoredcontent
ms.openlocfilehash: 2314a263a12c74e80c08391ae03425955a82458a
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2018
ms.locfileid: "37810316"
---
<a name="content-negotiation-in-aspnet-web-api"></a>Aushandlung von Inhalten in ASP.NET Web-API
====================
durch [Mike Wasson](https://github.com/MikeWasson)

In diesem Artikel wird beschrieben, wie ASP.NET Web-API die Aushandlung von Inhalten implementiert.

Die HTTP-Spezifikation (RFC 2616) definiert die Aushandlung von Inhalten als "der Vorgang der Auswahl der besten Darstellung für eine bestimmte Antwort, wenn mehrere Darstellungen verfügbar sind." Der primäre Mechanismus für die Content Negotiation in HTTP gibt, dass diese Header anfordern:

- **Akzeptiert:** die Medientypen sind zulässig, für die Antwort, z. B. "Application/Json", "Application/Xml" oder einen benutzerdefinierten Media-Typ, z. B. &quot;application/vnd.example+xml&quot;
- **Accept-Charset:** welche Zeichensätze sind zulässig, z. B. UTF-8 oder ISO 8859-1.
- **Accept-Encoding:** welche inhaltscodierungen sind zulässig, z. B. Gzip.
- **Accept-Language:** die bevorzugte Sprache wie z. B. "En-us".

Der Server kann auch andere Teile der HTTP-Anforderung überprüfen. Z. B. wenn die Anforderung einen X-Requested-With-Header enthält, kann, der angibt, einer AJAX-Anforderung, die Server standardmäßig in JSON, wenn keine Accept-Header vorhanden ist.

In diesem Artikel betrachten wir, wie Web-API den Accept "und" Accept-Charset-Header verwendet. (Zu diesem Zeitpunkt besteht keine integrierte Unterstützung für den Accept-Encoding "oder" Accept-Language.)

## <a name="serialization"></a>Serialisierung

Wenn ein Web-API-Controller im CLR-Typ eine Ressource zurückgibt, wird die Pipeline serialisiert den Rückgabewert und schreibt sie in den HTTP-Antworttext.

Betrachten Sie beispielsweise die folgende Controlleraktion:

[!code-csharp[Main](content-negotiation/samples/sample1.cs)]

Ein Client kann diese HTTP-Anforderung senden:

[!code-console[Main](content-negotiation/samples/sample2.cmd)]

Im Gegenzug kann der Server senden:

[!code-console[Main](content-negotiation/samples/sample3.cmd)]

In diesem Beispiel ist der Client angefordert hat, JSON, Javascript oder "anything" (\*/\*). Die Antwort eine JSON-Darstellung der Server die `Product` Objekt. Beachten Sie, die der Content-Type-Header in der Antwort, um festgelegt wird &quot;Application/Json&quot;.

Ein Controller kann auch Zurückgeben einer **HttpResponseMessage** Objekt. Rufen Sie zum Angeben einer CLR-Objekt, für den Antworttext der **CreateResponse** Erweiterungsmethode:

[!code-csharp[Main](content-negotiation/samples/sample4.cs)]

Diese Option erhalten Sie mehr Kontrolle über die Details der Antwort. Festlegen den Statuscode HTTP-Header hinzufügen und so weiter.

Das Objekt, das die Ressource serialisiert wird aufgerufen, eine *medienformatierer*. Medienformatierer leiten Sie von der **MediaTypeFormatter** Klasse. Web-API stellt medienformatierer für XML und JSON, und Sie können benutzerdefinierte Formatierer zur Unterstützung anderer Medientypen erstellen. Weitere Informationen über das Schreiben eines benutzerdefinierten Formatierers finden Sie unter [Medienformatierer](media-formatters.md).

## <a name="how-content-negotiation-works"></a>Wie funktioniert der Aushandlung

Die Pipeline ruft zunächst die **IContentNegotiator** -Diensts von der **HttpConfiguration** Objekt. Er ruft auch die Liste der medienformatierer aus der **HttpConfiguration.Formatters** Auflistung.

Als Nächstes die Pipeline ruft **IContentNegotiatior.Negotiate**, und übergeben Sie:

- Der Typ des zu serialisierenden Objekts
- Die Auflistung der medienformatierer
- Die HTTP-Anforderung

Die **Negotiate** Methodenrückgabe zwei Angaben:

- Die zu verwendende Formatierer
- Der Medientyp für die Antwort

Wenn kein Formatierer gefunden wird, die **Negotiate** Methodenrückgabe **null**, und der Client empfängt HTTP-Fehler 406 (nicht akzeptabel).

Der folgende Code zeigt, wie ein Controller die Aushandlung von Inhalten direkt aufrufen kann:

[!code-csharp[Main](content-negotiation/samples/sample5.cs)]

Dieser Code entspricht, was die Pipeline automatisch ausgeführt.

## <a name="default-content-negotiator"></a>Standardmäßigen Negotiator

Die **DefaultContentNegotiator** Klasse stellt die Standardimplementierung von **IContentNegotiator**. Verschiedene Kriterien verwendet, um ein Formatierungsprogramm zu wählen.

Zunächst muss der Formatierer den Typ serialisieren können. Dies wird durch den Aufruf überprüft **MediaTypeFormatter.CanWriteType**.

Als Nächstes das inhaltsaushandlungsmodul jeder Formatierer untersucht und ausgewertet wird, wie gut sie die HTTP-Anforderung übereinstimmt. Um die Übereinstimmung zu evaluieren, untersucht das inhaltsaushandlungsmodul zwei Dinge in das Formatierungsprogramm:

- Die **SupportedMediaTypes** -Auflistung, die eine Liste der unterstützten Medientypen enthält. Das inhaltsaushandlungsmodul versucht, diese Liste für die Anforderung Accept-Header. Beachten Sie, dass der Accept-Header Bereiche enthalten kann. Beispielsweise ist "Text/Plain" eine Übereinstimmung für den Text /\* oder \* / \*.
- Die **MediaTypeMappings** -Auflistung, die eine Liste der enthält **MediaTypeMapping** Objekte. Die **MediaTypeMapping** -Klasse stellt ein allgemeines Verfahren zum HTTP-Anforderungen mit Medientypen überein. Beispielsweise können sie einen benutzerdefinierten HTTP-Header einen bestimmten Medientyp zuordnen.

Es sind mehrere übereinstimmt, die Übereinstimmung mit der höchsten Qualität Faktor gewinnt. Zum Beispiel:

[!code-console[Main](content-negotiation/samples/sample6.cmd)]

In diesem Beispiel hat Application/Json eine implizite Qualitätsfaktor 1.0, daher ist es bevorzugt über Application/Xml.

Wenn keine Übereinstimmungen gefunden werden, versucht das inhaltsaushandlungsmodul entsprechend auf den Medientyp des Anforderungstexts, sofern vorhanden. Wenn die Anforderung JSON-Daten enthält, sucht das inhaltsaushandlungsmodul z. B. für ein JSON-Formatierungsprogramm.

Wenn noch keine Übereinstimmungen vorhanden sind, wählt das inhaltsaushandlungsmodul einfach das erste Formatierungsprogramm, das den Typ serialisieren kann.

## <a name="selecting-a-character-encoding"></a>Auswählen einer Zeichencodierung

Nachdem ein Formatierer ausgewählt wurde, das inhaltsaushandlungsmodul wählt die beste zeichencodierung anhand der **SupportedEncodings** Eigenschaft für das Formatierungsprogramm und Abgleichen mit dem Accept-Charset-Header in der Anforderung (falls vorhanden).
