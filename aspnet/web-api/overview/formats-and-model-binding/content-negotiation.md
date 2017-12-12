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
<a name="content-negotiation-in-aspnet-web-api"></a><span data-ttu-id="82162-103">Inhaltsaushandlung in ASP.NET Web-API</span><span class="sxs-lookup"><span data-stu-id="82162-103">Content Negotiation in ASP.NET Web API</span></span>
====================
<span data-ttu-id="82162-104">durch [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="82162-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="82162-105">In diesem Artikel wird beschrieben, wie ASP.NET Web API inhaltsaushandlung implementiert.</span><span class="sxs-lookup"><span data-stu-id="82162-105">This article describes how ASP.NET Web API implements content negotiation.</span></span>

<span data-ttu-id="82162-106">Die HTTP-Spezifikation (RFC 2616) definiert die inhaltsaushandlung als "der Vorgang der Auswahl der bestmögliche Darstellung für eine bestimmte Antwort, wenn mehrere Darstellungen vorhanden sind."</span><span class="sxs-lookup"><span data-stu-id="82162-106">The HTTP specification (RFC 2616) defines content negotiation as "the process of selecting the best representation for a given response when there are multiple representations available."</span></span> <span data-ttu-id="82162-107">Der primäre Mechanismus für die inhaltsaushandlung in HTTP-sind, dass diese Header anfordern:</span><span class="sxs-lookup"><span data-stu-id="82162-107">The primary mechanism for content negotiation in HTTP are these request headers:</span></span>

- <span data-ttu-id="82162-108">**Akzeptiert:** Welche Medientypen sind zulässig, für die Antwort, z. B. "Application/Json", "Application/Xml" oder einen benutzerdefinierten Media-Typ, z. B. &quot;application/vnd.example+xml&quot;</span><span class="sxs-lookup"><span data-stu-id="82162-108">**Accept:** Which media types are acceptable for the response, such as "application/json," "application/xml," or a custom media type such as &quot;application/vnd.example+xml&quot;</span></span>
- <span data-ttu-id="82162-109">**Accept-Charset:** welche Zeichensätze sind zulässig, z. B. UTF-8 oder ISO 8859-1.</span><span class="sxs-lookup"><span data-stu-id="82162-109">**Accept-Charset:** Which character sets are acceptable, such as UTF-8 or ISO 8859-1.</span></span>
- <span data-ttu-id="82162-110">**Accept-Encoding:** welche inhaltscodierungen sind zulässig, z. B. Gzip.</span><span class="sxs-lookup"><span data-stu-id="82162-110">**Accept-Encoding:** Which content encodings are acceptable, such as gzip.</span></span>
- <span data-ttu-id="82162-111">**Accept-Language:** die bevorzugte natürlicher Sprache, z. B. "En-us".</span><span class="sxs-lookup"><span data-stu-id="82162-111">**Accept-Language:** The preferred natural language, such as "en-us".</span></span>

<span data-ttu-id="82162-112">Die Server kann auch andere Teile der HTTP-Anforderung überprüfen.</span><span class="sxs-lookup"><span data-stu-id="82162-112">The server can also look at other portions of the HTTP request.</span></span> <span data-ttu-id="82162-113">Z. B. wenn die Anforderung einen X-Requested-With-Header enthält, kann, der angibt, einer AJAX-Anforderung der Server JSON standardmäßig, wenn keine Accept-Header vorhanden ist.</span><span class="sxs-lookup"><span data-stu-id="82162-113">For example, if the request contains an X-Requested-With header, indicating an AJAX request, the server might default to JSON if there is no Accept header.</span></span>

<span data-ttu-id="82162-114">In diesem Artikel betrachten wir, wie Web-API die annehmen und Accept-Charset-Header verwendet.</span><span class="sxs-lookup"><span data-stu-id="82162-114">In this article, we'll look at how Web API uses the Accept and Accept-Charset headers.</span></span> <span data-ttu-id="82162-115">(Zu diesem Zeitpunkt besteht keine integrierte Unterstützung für den Accept-Encoding "oder" Accept-Language.)</span><span class="sxs-lookup"><span data-stu-id="82162-115">(At this time, there is no built-in support for Accept-Encoding or Accept-Language.)</span></span>

## <a name="serialization"></a><span data-ttu-id="82162-116">Serialisierung</span><span class="sxs-lookup"><span data-stu-id="82162-116">Serialization</span></span>

<span data-ttu-id="82162-117">Wenn ein Web-API-Controller eine Ressource als CLR-Typ zurückgibt, wird die Pipeline serialisiert den Rückgabewert und schreibt sie in den HTTP-Antworttext.</span><span class="sxs-lookup"><span data-stu-id="82162-117">If a Web API controller returns a resource as CLR type, the pipeline serializes the return value and writes it into the HTTP response body.</span></span>

<span data-ttu-id="82162-118">Betrachten Sie beispielsweise die folgende Controlleraktion aus:</span><span class="sxs-lookup"><span data-stu-id="82162-118">For example, consider the following controller action:</span></span>

[!code-csharp[Main](content-negotiation/samples/sample1.cs)]

<span data-ttu-id="82162-119">Ein Client kann diese HTTP-Anforderung senden:</span><span class="sxs-lookup"><span data-stu-id="82162-119">A client might send this HTTP request:</span></span>

[!code-console[Main](content-negotiation/samples/sample2.cmd)]

<span data-ttu-id="82162-120">Als Antwort möglicherweise vom Server gesendet:</span><span class="sxs-lookup"><span data-stu-id="82162-120">In response, the server might send:</span></span>

[!code-console[Main](content-negotiation/samples/sample3.cmd)]

<span data-ttu-id="82162-121">In diesem Beispiel wird der Client angefordert wird, JSON, Javascript, oder "nichts" (\*/\*).</span><span class="sxs-lookup"><span data-stu-id="82162-121">In this example, the client requested either JSON, Javascript, or "anything" (\*/\*).</span></span> <span data-ttu-id="82162-122">Der Server mit einem JSON-Darstellung des reagiert die `Product` Objekt.</span><span class="sxs-lookup"><span data-stu-id="82162-122">The server responsed with a JSON representation of the `Product` object.</span></span> <span data-ttu-id="82162-123">Beachten Sie, dass der Content-Type-Header in der Antwort auf &quot;Application/Json&quot;.</span><span class="sxs-lookup"><span data-stu-id="82162-123">Notice that the Content-Type header in the response is set to &quot;application/json&quot;.</span></span>

<span data-ttu-id="82162-124">Ein Controller kann auch Zurückgeben einer **HttpResponseMessage** Objekt.</span><span class="sxs-lookup"><span data-stu-id="82162-124">A controller can also return an **HttpResponseMessage** object.</span></span> <span data-ttu-id="82162-125">Rufen Sie zum Angeben einer CLR-Objekt für den Antworttext der **CreateResponse** Erweiterungsmethode:</span><span class="sxs-lookup"><span data-stu-id="82162-125">To specify a CLR object for the response body, call the **CreateResponse** extension method:</span></span>

[!code-csharp[Main](content-negotiation/samples/sample4.cs)]

<span data-ttu-id="82162-126">Diese Option bietet Ihnen mehr Kontrolle über die Details der Antwort.</span><span class="sxs-lookup"><span data-stu-id="82162-126">This option gives you more control over the details of the response.</span></span> <span data-ttu-id="82162-127">Festlegen den Statuscode HTTP-Header hinzufügen und so weiter.</span><span class="sxs-lookup"><span data-stu-id="82162-127">You can set the status code, add HTTP headers, and so forth.</span></span>

<span data-ttu-id="82162-128">Das Objekt, das die Ressource serialisiert wird aufgerufen, eine *medienformatierer*.</span><span class="sxs-lookup"><span data-stu-id="82162-128">The object that serializes the resource is called a *media formatter*.</span></span> <span data-ttu-id="82162-129">Medienformatierer Ableiten der **MediaTypeFormatter** Klasse.</span><span class="sxs-lookup"><span data-stu-id="82162-129">Media formatters derive from the **MediaTypeFormatter** class.</span></span> <span data-ttu-id="82162-130">Web-API bietet medienformatierer für XML und JSON, und Sie können den benutzerdefinierten Formatierer zur Unterstützung von anderen Medientypen erstellen.</span><span class="sxs-lookup"><span data-stu-id="82162-130">Web API provides media formatters for XML and JSON, and you can create custom formatters to support other media types.</span></span> <span data-ttu-id="82162-131">Informationen über das Schreiben von ein benutzerdefiniertes Formatierungsprogramm finden Sie unter [Medienformatierer](media-formatters.md).</span><span class="sxs-lookup"><span data-stu-id="82162-131">For information about writing a custom formatter, see [Media Formatters](media-formatters.md).</span></span>

## <a name="how-content-negotiation-works"></a><span data-ttu-id="82162-132">Wie funktioniert der Aushandlung</span><span class="sxs-lookup"><span data-stu-id="82162-132">How Content Negotiation Works</span></span>

<span data-ttu-id="82162-133">Die Pipeline zunächst Ruft die **IContentNegotiator** -Diensts von der **HttpConfiguration** Objekt.</span><span class="sxs-lookup"><span data-stu-id="82162-133">First, the pipeline gets the **IContentNegotiator** service from the **HttpConfiguration** object.</span></span> <span data-ttu-id="82162-134">Er ruft auch die Liste der medienformatierer aus der **HttpConfiguration.Formatters** Auflistung.</span><span class="sxs-lookup"><span data-stu-id="82162-134">It also gets the list of media formatters from the **HttpConfiguration.Formatters** collection.</span></span>

<span data-ttu-id="82162-135">Als Nächstes die Pipeline aufruft **IContentNegotiatior.Negotiate**, und übergeben Sie:</span><span class="sxs-lookup"><span data-stu-id="82162-135">Next, the pipeline calls **IContentNegotiatior.Negotiate**, passing in:</span></span>

- <span data-ttu-id="82162-136">Der Typ des zu serialisierenden Objekts</span><span class="sxs-lookup"><span data-stu-id="82162-136">The type of object to serialize</span></span>
- <span data-ttu-id="82162-137">Die Auflistung der medienformatierer</span><span class="sxs-lookup"><span data-stu-id="82162-137">The collection of media formatters</span></span>
- <span data-ttu-id="82162-138">Die HTTP-Anforderung</span><span class="sxs-lookup"><span data-stu-id="82162-138">The HTTP request</span></span>

<span data-ttu-id="82162-139">Die **Negotiate** Methodenrückgabe zwei Angaben:</span><span class="sxs-lookup"><span data-stu-id="82162-139">The **Negotiate** method returns two pieces of information:</span></span>

- <span data-ttu-id="82162-140">Die zu verwendende Formatierer</span><span class="sxs-lookup"><span data-stu-id="82162-140">Which formatter to use</span></span>
- <span data-ttu-id="82162-141">Der Medientyp für die Antwort</span><span class="sxs-lookup"><span data-stu-id="82162-141">The media type for the response</span></span>

<span data-ttu-id="82162-142">Wenn kein Formatierer gefunden wird, die **Negotiate** -Methode zurückkehrt **null**, und der Client empfängt HTTP-Fehler 406 (nicht akzeptabel).</span><span class="sxs-lookup"><span data-stu-id="82162-142">If no formatter is found, the **Negotiate** method returns **null**, and the client recevies HTTP error 406 (Not Acceptable).</span></span>

<span data-ttu-id="82162-143">Der folgende Code zeigt, wie ein Domänencontroller inhaltsaushandlung direkt aufrufen kann:</span><span class="sxs-lookup"><span data-stu-id="82162-143">The following code shows how a controller can directly invoke content negotiation:</span></span>

[!code-csharp[Main](content-negotiation/samples/sample5.cs)]

<span data-ttu-id="82162-144">Dieser Code entspricht, was die Pipeline automatisch ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="82162-144">This code is equivalent to the what the pipeline does automatically.</span></span>

## <a name="default-content-negotiator"></a><span data-ttu-id="82162-145">Standard-Inhaltsaushandlungsmodul</span><span class="sxs-lookup"><span data-stu-id="82162-145">Default Content Negotiator</span></span>

<span data-ttu-id="82162-146">Die **DefaultContentNegotiator** Klasse stellt die Standardimplementierung von **IContentNegotiator**.</span><span class="sxs-lookup"><span data-stu-id="82162-146">The **DefaultContentNegotiator** class provides the default implementation of **IContentNegotiator**.</span></span> <span data-ttu-id="82162-147">Mehrere Kriterien verwendet, um einen Formatierer auswählen.</span><span class="sxs-lookup"><span data-stu-id="82162-147">It uses several criteria to select a formatter.</span></span>

<span data-ttu-id="82162-148">Zuerst muss der Formatierer den Typ serialisieren können.</span><span class="sxs-lookup"><span data-stu-id="82162-148">First, the formatter must be able to serialize the type.</span></span> <span data-ttu-id="82162-149">Dies wird durch den Aufruf überprüft **MediaTypeFormatter.CanWriteType**.</span><span class="sxs-lookup"><span data-stu-id="82162-149">This is verified by calling **MediaTypeFormatter.CanWriteType**.</span></span>

<span data-ttu-id="82162-150">Als Nächstes das inhaltsaushandlungsmodul jeder Formatierer untersucht und ausgewertet wird, inwieweit sie die HTTP-Anforderung entspricht.</span><span class="sxs-lookup"><span data-stu-id="82162-150">Next, the content negotiator looks at each formatter and evaluates how well it matches the HTTP request.</span></span> <span data-ttu-id="82162-151">Um die Übereinstimmung zu evaluieren, prüft das inhaltsaushandlungsmodul zwei Dinge auf das Formatierungsprogramm:</span><span class="sxs-lookup"><span data-stu-id="82162-151">To evaluate the match, the content negotiator looks at two things on the formatter:</span></span>

- <span data-ttu-id="82162-152">Die **SupportedMediaTypes** -Auflistung, die eine Liste der unterstützten Medientypen enthält.</span><span class="sxs-lookup"><span data-stu-id="82162-152">The **SupportedMediaTypes** collection, which contains a list of supported media types.</span></span> <span data-ttu-id="82162-153">Das inhaltsaushandlungsmodul versucht, diese Liste für die Anforderung Accept-Header übereinstimmen.</span><span class="sxs-lookup"><span data-stu-id="82162-153">The content negotiator tries to match this list against the request Accept header.</span></span> <span data-ttu-id="82162-154">Beachten Sie, dass der Accept-Header Bereiche enthalten kann.</span><span class="sxs-lookup"><span data-stu-id="82162-154">Note that the Accept header can include ranges.</span></span> <span data-ttu-id="82162-155">Z. B. "Text/Plain" wird eine Entsprechung für Text /\* oder \* / \*.</span><span class="sxs-lookup"><span data-stu-id="82162-155">For example, "text/plain" is a match for text/\* or \*/\*.</span></span>
- <span data-ttu-id="82162-156">Die **MediaTypeMappings** -Auflistung, die eine Liste von enthält **MediaTypeMapping** Objekte.</span><span class="sxs-lookup"><span data-stu-id="82162-156">The **MediaTypeMappings** collection, which contains a list of **MediaTypeMapping** objects.</span></span> <span data-ttu-id="82162-157">Die **MediaTypeMapping** -Klasse stellt ein allgemeines Verfahren zum HTTP-Anforderungen mit Medientypen überein.</span><span class="sxs-lookup"><span data-stu-id="82162-157">The **MediaTypeMapping** class provides a generic way to match HTTP requests with media types.</span></span> <span data-ttu-id="82162-158">Beispielsweise konnten sie einen benutzerdefinierten HTTP-Header einen bestimmten Medientyp zuzuordnen.</span><span class="sxs-lookup"><span data-stu-id="82162-158">For example, it could map a custom HTTP header to a particular media type.</span></span>

<span data-ttu-id="82162-159">Wenn es mehrere übereinstimmt, die Übereinstimmung mit der höchsten Qualität Faktor gewinnt.</span><span class="sxs-lookup"><span data-stu-id="82162-159">If there are multiple matches, the match with the highest quality factor wins.</span></span> <span data-ttu-id="82162-160">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="82162-160">For example:</span></span>

[!code-console[Main](content-negotiation/samples/sample6.cmd)]

<span data-ttu-id="82162-161">In diesem Beispiel weist Application/Json eine impliziten Qualität Authentifizierungsstufe 1.0, deshalb über Application/Xml bevorzugte ist.</span><span class="sxs-lookup"><span data-stu-id="82162-161">In this example, application/json has an implied quality factor of 1.0, so it is preferred over application/xml.</span></span>

<span data-ttu-id="82162-162">Wenn keine Übereinstimmungen gefunden werden, versucht, das inhaltsaushandlungsmodul abzugleichende der Medientyp des Anforderungstexts, falls vorhanden.</span><span class="sxs-lookup"><span data-stu-id="82162-162">If no matches are found, the content negotiator tries to match on the media type of the request body, if any.</span></span> <span data-ttu-id="82162-163">Wenn die Anforderung JSON-Daten enthält, sucht das inhaltsaushandlungsmodul z. B. nach einem JSON-Formatierer.</span><span class="sxs-lookup"><span data-stu-id="82162-163">For example, if the request contains JSON data, the content negotiator looks for a JSON formatter.</span></span>

<span data-ttu-id="82162-164">Wenn noch keine Übereinstimmungen vorhanden sind, wählt das inhaltsaushandlungsmodul einfach den ersten Formatierer, der den Typ serialisieren kann.</span><span class="sxs-lookup"><span data-stu-id="82162-164">If there are still no matches, the content negotiator simply picks the first formatter that can serialize the type.</span></span>

## <a name="selecting-a-character-encoding"></a><span data-ttu-id="82162-165">Auswählen einer Zeichencodierung</span><span class="sxs-lookup"><span data-stu-id="82162-165">Selecting a Character Encoding</span></span>

<span data-ttu-id="82162-166">Nachdem ein Formatierer ausgewählt ist, das inhaltsaushandlungsmodul wählt die beste zeichencodierung durch einen Blick auf die **SupportedEncodings** -Eigenschaft für das Formatierungsprogramm und übereinstimmender den Accept-Charset-Header in der Anforderung (sofern vorhanden).</span><span class="sxs-lookup"><span data-stu-id="82162-166">After a formatter is selected, the content negotiator chooses the best character encoding by looking at the **SupportedEncodings** property on the formatter, and matching it against the Accept-Charset header in the request (if any).</span></span>
