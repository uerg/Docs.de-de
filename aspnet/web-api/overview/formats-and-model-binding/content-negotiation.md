---
uid: web-api/overview/formats-and-model-binding/content-negotiation
title: Inhalts-Aushandlung in ASP.NET Web-API | Microsoft Docs
author: MikeWasson
description: Beschreibt, wie HTTP-inhaltsaushandlung von ASP.NET Web-API implementiert.
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/20/2012
ms.topic: article
ms.assetid: 0dd51b30-bf5a-419f-a1b7-2817ccca3c7d
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/formats-and-model-binding/content-negotiation
msc.type: authoredcontent
ms.openlocfilehash: ca373af6754e82889dc100b63f73b76aaa4e4f27
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="content-negotiation-in-aspnet-web-api"></a>Inhaltsaushandlung in ASP.NET Web-API
====================
durch [Mike Wasson](https://github.com/MikeWasson)

In diesem Artikel wird beschrieben, wie ASP.NET Web API inhaltsaushandlung implementiert.

Die HTTP-Spezifikation (RFC 2616) definiert die inhaltsaushandlung als "der Vorgang der Auswahl der bestmögliche Darstellung für eine bestimmte Antwort, wenn mehrere Darstellungen vorhanden sind." Der primäre Mechanismus für die inhaltsaushandlung in HTTP-sind, dass diese Header anfordern:

- **Akzeptiert:** Welche Medientypen sind zulässig, für die Antwort, z. B. "Application/Json", "Application/Xml" oder einen benutzerdefinierten Media-Typ, z. B. &quot;application/vnd.example+xml&quot;
- **Accept-Charset:** welche Zeichensätze sind zulässig, z. B. UTF-8 oder ISO 8859-1.
- **Accept-Encoding:** welche inhaltscodierungen sind zulässig, z. B. Gzip.
- **Accept-Language:** die bevorzugte natürlicher Sprache, z. B. "En-us".

Die Server kann auch andere Teile der HTTP-Anforderung überprüfen. Z. B. wenn die Anforderung einen X-Requested-With-Header enthält, kann, der angibt, einer AJAX-Anforderung der Server JSON standardmäßig, wenn keine Accept-Header vorhanden ist.

In diesem Artikel betrachten wir, wie Web-API die annehmen und Accept-Charset-Header verwendet. (Zu diesem Zeitpunkt besteht keine integrierte Unterstützung für den Accept-Encoding "oder" Accept-Language.)

## <a name="serialization"></a>Serialisierung

Wenn ein Web-API-Controller eine Ressource als CLR-Typ zurückgibt, wird die Pipeline serialisiert den Rückgabewert und schreibt sie in den HTTP-Antworttext.

Betrachten Sie beispielsweise die folgende Controlleraktion aus:

[!code-csharp[Main](content-negotiation/samples/sample1.cs)]

Ein Client kann diese HTTP-Anforderung senden:

[!code-console[Main](content-negotiation/samples/sample2.cmd)]

Als Antwort möglicherweise vom Server gesendet:

[!code-console[Main](content-negotiation/samples/sample3.cmd)]

In diesem Beispiel wird der Client angefordert wird, JSON, Javascript, oder "nichts" (\*/\*). Der Server mit einem JSON-Darstellung des reagiert die `Product` Objekt. Beachten Sie, dass der Content-Type-Header in der Antwort auf &quot;Application/Json&quot;.

Ein Controller kann auch Zurückgeben einer **HttpResponseMessage** Objekt. Rufen Sie zum Angeben einer CLR-Objekt für den Antworttext der **CreateResponse** Erweiterungsmethode:

[!code-csharp[Main](content-negotiation/samples/sample4.cs)]

Diese Option bietet Ihnen mehr Kontrolle über die Details der Antwort. Festlegen den Statuscode HTTP-Header hinzufügen und so weiter.

Das Objekt, das die Ressource serialisiert wird aufgerufen, eine *medienformatierer*. Medienformatierer Ableiten der **MediaTypeFormatter** Klasse. Web-API bietet medienformatierer für XML und JSON, und Sie können den benutzerdefinierten Formatierer zur Unterstützung von anderen Medientypen erstellen. Informationen über das Schreiben von ein benutzerdefiniertes Formatierungsprogramm finden Sie unter [Medienformatierer](media-formatters.md).

## <a name="how-content-negotiation-works"></a>Wie funktioniert der Aushandlung

Die Pipeline zunächst Ruft die **IContentNegotiator** -Diensts von der **HttpConfiguration** Objekt. Er ruft auch die Liste der medienformatierer aus der **HttpConfiguration.Formatters** Auflistung.

Als Nächstes die Pipeline aufruft **IContentNegotiatior.Negotiate**, und übergeben Sie:

- Der Typ des zu serialisierenden Objekts
- Die Auflistung der medienformatierer
- Die HTTP-Anforderung

Die **Negotiate** Methodenrückgabe zwei Angaben:

- Die zu verwendende Formatierer
- Der Medientyp für die Antwort

Wenn kein Formatierer gefunden wird, die **Negotiate** -Methode zurückkehrt **null**, und der Client empfängt HTTP-Fehler 406 (nicht akzeptabel).

Der folgende Code zeigt, wie ein Domänencontroller inhaltsaushandlung direkt aufrufen kann:

[!code-csharp[Main](content-negotiation/samples/sample5.cs)]

Dieser Code entspricht, was die Pipeline automatisch ausgeführt wird.

## <a name="default-content-negotiator"></a>Standard-Inhaltsaushandlungsmodul

Die **DefaultContentNegotiator** Klasse stellt die Standardimplementierung von **IContentNegotiator**. Mehrere Kriterien verwendet, um einen Formatierer auswählen.

Zuerst muss der Formatierer den Typ serialisieren können. Dies wird durch den Aufruf überprüft **MediaTypeFormatter.CanWriteType**.

Als Nächstes das inhaltsaushandlungsmodul jeder Formatierer untersucht und ausgewertet wird, inwieweit sie die HTTP-Anforderung entspricht. Um die Übereinstimmung zu evaluieren, prüft das inhaltsaushandlungsmodul zwei Dinge auf das Formatierungsprogramm:

- Die **SupportedMediaTypes** -Auflistung, die eine Liste der unterstützten Medientypen enthält. Das inhaltsaushandlungsmodul versucht, diese Liste für die Anforderung Accept-Header übereinstimmen. Beachten Sie, dass der Accept-Header Bereiche enthalten kann. Z. B. "Text/Plain" wird eine Entsprechung für Text /\* oder \* / \*.
- Die **MediaTypeMappings** -Auflistung, die eine Liste von enthält **MediaTypeMapping** Objekte. Die **MediaTypeMapping** -Klasse stellt ein allgemeines Verfahren zum HTTP-Anforderungen mit Medientypen überein. Beispielsweise konnten sie einen benutzerdefinierten HTTP-Header einen bestimmten Medientyp zuzuordnen.

Wenn es mehrere übereinstimmt, die Übereinstimmung mit der höchsten Qualität Faktor gewinnt. Zum Beispiel:

[!code-console[Main](content-negotiation/samples/sample6.cmd)]

In diesem Beispiel weist Application/Json eine impliziten Qualität Authentifizierungsstufe 1.0, deshalb über Application/Xml bevorzugte ist.

Wenn keine Übereinstimmungen gefunden werden, versucht, das inhaltsaushandlungsmodul abzugleichende der Medientyp des Anforderungstexts, falls vorhanden. Wenn die Anforderung JSON-Daten enthält, sucht das inhaltsaushandlungsmodul z. B. nach einem JSON-Formatierer.

Wenn noch keine Übereinstimmungen vorhanden sind, wählt das inhaltsaushandlungsmodul einfach den ersten Formatierer, der den Typ serialisieren kann.

## <a name="selecting-a-character-encoding"></a>Auswählen einer Zeichencodierung

Nachdem ein Formatierer ausgewählt ist, das inhaltsaushandlungsmodul wählt die beste zeichencodierung durch einen Blick auf die **SupportedEncodings** -Eigenschaft für das Formatierungsprogramm und übereinstimmender den Accept-Charset-Header in der Anforderung (sofern vorhanden).
