---
uid: web-api/overview/advanced/http-cookies
title: HTTP-Cookies in ASP.NET Web-API | Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/17/2012
ms.topic: article
ms.assetid: 243db2ec-8f67-4a5e-a382-4ddcec4b4164
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/advanced/http-cookies
msc.type: authoredcontent
ms.openlocfilehash: 363ca975cf75b635b766a53eeda87cf957eed60c
ms.sourcegitcommit: 48beecfe749ddac52bc79aa3eb246a2dcdaa1862
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/22/2018
---
<a name="http-cookies-in-aspnet-web-api"></a><span data-ttu-id="8eddc-102">HTTP-Cookies in ASP.NET Web-API</span><span class="sxs-lookup"><span data-stu-id="8eddc-102">HTTP Cookies in ASP.NET Web API</span></span>
====================
<span data-ttu-id="8eddc-103">durch [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="8eddc-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="8eddc-104">In diesem Thema wird beschrieben, wie zum Senden und Empfangen von HTTP-Cookies in Web-API wird.</span><span class="sxs-lookup"><span data-stu-id="8eddc-104">This topic describes how to send and receive HTTP cookies in Web API.</span></span>

## <a name="background-on-http-cookies"></a><span data-ttu-id="8eddc-105">Hintergrundinformationen zu HTTP-Cookies</span><span class="sxs-lookup"><span data-stu-id="8eddc-105">Background on HTTP Cookies</span></span>

<span data-ttu-id="8eddc-106">Dieser Abschnitt bietet einen kurzen Überblick darüber, wie Cookies auf HTTP-Ebene implementiert werden.</span><span class="sxs-lookup"><span data-stu-id="8eddc-106">This section gives a brief overview of how cookies are implemented at the HTTP level.</span></span> <span data-ttu-id="8eddc-107">Weitere Informationen finden Sie in [RFC 6265](http://tools.ietf.org/html/rfc6265).</span><span class="sxs-lookup"><span data-stu-id="8eddc-107">For details, consult [RFC 6265](http://tools.ietf.org/html/rfc6265).</span></span>

<span data-ttu-id="8eddc-108">Ein Cookie ist ein Teil der Daten, die ein Server in der HTTP-Antwort sendet.</span><span class="sxs-lookup"><span data-stu-id="8eddc-108">A cookie is a piece of data that a server sends in the HTTP response.</span></span> <span data-ttu-id="8eddc-109">Der Client speichert das Cookie (optional) und wird für Anforderungen Subsequet zurückgegeben.</span><span class="sxs-lookup"><span data-stu-id="8eddc-109">The client (optionally) stores the cookie and returns it on subsequet requests.</span></span> <span data-ttu-id="8eddc-110">Dies ermöglicht dem Client und Server, um Ihren Zustand freigeben.</span><span class="sxs-lookup"><span data-stu-id="8eddc-110">This allows the client and server to share state.</span></span> <span data-ttu-id="8eddc-111">Um ein Cookie festlegen, enthält der Server einen Set-Cookie-Header in der Antwort.</span><span class="sxs-lookup"><span data-stu-id="8eddc-111">To set a cookie, the server includes a Set-Cookie header in the response.</span></span> <span data-ttu-id="8eddc-112">Das Format eines Cookies ist ein Name / Wert-Paar mit optionalen Attributen.</span><span class="sxs-lookup"><span data-stu-id="8eddc-112">The format of a cookie is a name-value pair, with optional attributes.</span></span> <span data-ttu-id="8eddc-113">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="8eddc-113">For example:</span></span>

[!code-powershell[Main](http-cookies/samples/sample1.ps1)]

<span data-ttu-id="8eddc-114">Hier ist ein Beispiel mit Attributen:</span><span class="sxs-lookup"><span data-stu-id="8eddc-114">Here is an example with attributes:</span></span>

[!code-powershell[Main](http-cookies/samples/sample2.ps1)]

<span data-ttu-id="8eddc-115">Um ein Cookie an den Server zurückzugeben, fügt der Client einen Cookie-Header in späteren Anforderungen.</span><span class="sxs-lookup"><span data-stu-id="8eddc-115">To return a cookie to the server, the client includes a Cookie header in later requests.</span></span>

[!code-console[Main](http-cookies/samples/sample3.cmd)]

![](http-cookies/_static/image1.png)

<span data-ttu-id="8eddc-116">Eine HTTP-Antwort kann mehrere Set-Cookie-Header enthalten.</span><span class="sxs-lookup"><span data-stu-id="8eddc-116">An HTTP response can include multiple Set-Cookie headers.</span></span>

[!code-powershell[Main](http-cookies/samples/sample4.ps1)]

<span data-ttu-id="8eddc-117">Der Client gibt mehrere Cookies, die mit einem einzelnen Cookieheader zurück.</span><span class="sxs-lookup"><span data-stu-id="8eddc-117">The client returns multiple cookies using a single Cookie header.</span></span>

[!code-console[Main](http-cookies/samples/sample5.cmd)]

<span data-ttu-id="8eddc-118">Der Bereich und die Dauer eines Cookies werden durch die folgenden Attribute in der Set-Cookie-Header gesteuert:</span><span class="sxs-lookup"><span data-stu-id="8eddc-118">The scope and duration of a cookie are controlled by following attributes in the Set-Cookie header:</span></span>

- <span data-ttu-id="8eddc-119">**Domäne**: teilt den Clientcomputern mit, welche Domäne des Cookies erhalten soll.</span><span class="sxs-lookup"><span data-stu-id="8eddc-119">**Domain**: Tells the client which domain should receive the cookie.</span></span> <span data-ttu-id="8eddc-120">Z. B. ein die Domäne "example.com" ist, gibt der Client das Cookie für jede Unterdomäne example.com zurück. Wenn nicht angegeben, ist die Domäne dem Ausgangsserver.</span><span class="sxs-lookup"><span data-stu-id="8eddc-120">For example, if the domain is "example.com", the client returns the cookie to every subdomain of example.com. If not specified, the domain is the origin server.</span></span>
- <span data-ttu-id="8eddc-121">**Pfad**: das Cookie an den angegebenen Pfad innerhalb der Domäne beschränkt.</span><span class="sxs-lookup"><span data-stu-id="8eddc-121">**Path**: Restricts the cookie to the specified path within the domain.</span></span> <span data-ttu-id="8eddc-122">Wenn nicht angegeben ist, wird der Pfad der Anforderungs-URI verwendet.</span><span class="sxs-lookup"><span data-stu-id="8eddc-122">If not specified, the path of the request URI is used.</span></span>
- <span data-ttu-id="8eddc-123">**Läuft ab**: Legt ein Ablaufdatum für das Cookie.</span><span class="sxs-lookup"><span data-stu-id="8eddc-123">**Expires**: Sets an expiration date for the cookie.</span></span> <span data-ttu-id="8eddc-124">Der Client löscht das Cookie an, wenn diese abläuft.</span><span class="sxs-lookup"><span data-stu-id="8eddc-124">The client deletes the cookie when it expires.</span></span>
- <span data-ttu-id="8eddc-125">**Max-Age**: Legt das maximale Alter für das Cookie.</span><span class="sxs-lookup"><span data-stu-id="8eddc-125">**Max-Age**: Sets the maximum age for the cookie.</span></span> <span data-ttu-id="8eddc-126">Der Client löscht das Cookie an, wenn sie das maximale Alter erreicht.</span><span class="sxs-lookup"><span data-stu-id="8eddc-126">The client deletes the cookie when it reaches the maximum age.</span></span>

<span data-ttu-id="8eddc-127">Wenn beide `Expires` und `Max-Age` festgelegt sind, `Max-Age` hat Vorrang vor.</span><span class="sxs-lookup"><span data-stu-id="8eddc-127">If both `Expires` and `Max-Age` are set, `Max-Age` takes precedence.</span></span> <span data-ttu-id="8eddc-128">Wenn keiner festgelegt ist, löscht der Client das Cookie an, wenn die aktuelle Sitzung beendet.</span><span class="sxs-lookup"><span data-stu-id="8eddc-128">If neither is set, the client deletes the cookie when the current session ends.</span></span> <span data-ttu-id="8eddc-129">(Die genaue Bedeutung von "Session" wird durch den Benutzer-Agent bestimmt.)</span><span class="sxs-lookup"><span data-stu-id="8eddc-129">(The exact meaning of "session" is determined by the user-agent.)</span></span>

<span data-ttu-id="8eddc-130">Bedenken Sie jedoch, dass Clients Cookies ignoriert werden können.</span><span class="sxs-lookup"><span data-stu-id="8eddc-130">However, be aware that clients may ignore cookies.</span></span> <span data-ttu-id="8eddc-131">Beispielsweise kann ein Benutzer Cookies Gründen des Datenschutzes deaktivieren.</span><span class="sxs-lookup"><span data-stu-id="8eddc-131">For example, a user might disable cookies for privacy reasons.</span></span> <span data-ttu-id="8eddc-132">Clients können Cookies zu löschen, bevor sie ablaufen, oder die Anzahl der Cookies gespeichert.</span><span class="sxs-lookup"><span data-stu-id="8eddc-132">Clients may delete cookies before they expire, or limit the number of cookies stored.</span></span> <span data-ttu-id="8eddc-133">Aus Gründen des Datenschutzes ablehnen Clients häufig "Fremdanbieter" Cookies, in der Domäne nicht den Ursprungsserver übereinstimmt.</span><span class="sxs-lookup"><span data-stu-id="8eddc-133">For privacy reasons, clients often reject "third party" cookies, where the domain does not match the origin server.</span></span> <span data-ttu-id="8eddc-134">Kurz gesagt, sollten der Server nicht verlassen, zum Abrufen von Cookies, die er festlegt.</span><span class="sxs-lookup"><span data-stu-id="8eddc-134">In short, the server should not rely on getting back the cookies that it sets.</span></span>

## <a name="cookies-in-web-api"></a><span data-ttu-id="8eddc-135">Cookies in Web-API</span><span class="sxs-lookup"><span data-stu-id="8eddc-135">Cookies in Web API</span></span>

<span data-ttu-id="8eddc-136">Um eine HTTP-Antwort einen Cookie hinzugefügt haben, erstellen Sie eine **CookieHeaderValue** Instanz, die das Cookie darstellt.</span><span class="sxs-lookup"><span data-stu-id="8eddc-136">To add a cookie to an HTTP response, create a **CookieHeaderValue** instance that represents the cookie.</span></span> <span data-ttu-id="8eddc-137">Rufen Sie anschließend die **AddCookies** Erweiterungsmethode ist, definiert in der **System.Net.Http. HttpResponseHeadersExtensions** Klasse, um das Cookie hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="8eddc-137">Then call the **AddCookies** extension method, which is defined in the **System.Net.Http. HttpResponseHeadersExtensions** class, to add the cookie.</span></span>

<span data-ttu-id="8eddc-138">Im folgende Code wird z. B. einen Cookie in eine Controlleraktion hinzugefügt:</span><span class="sxs-lookup"><span data-stu-id="8eddc-138">For example, the following code adds a cookie within a controller action:</span></span>

[!code-csharp[Main](http-cookies/samples/sample6.cs)]

<span data-ttu-id="8eddc-139">Beachten Sie, dass **AddCookies** wird ein Array von **CookieHeaderValue** Instanzen.</span><span class="sxs-lookup"><span data-stu-id="8eddc-139">Notice that **AddCookies** takes an array of **CookieHeaderValue** instances.</span></span>

<span data-ttu-id="8eddc-140">Um die Cookies aus einer Clientanforderung zu extrahieren, rufen die **GetCookies** Methode:</span><span class="sxs-lookup"><span data-stu-id="8eddc-140">To extract the cookies from a client request, call the **GetCookies** method:</span></span>

[!code-csharp[Main](http-cookies/samples/sample7.cs)]

<span data-ttu-id="8eddc-141">Ein **CookieHeaderValue** enthält eine Auflistung von **CookieState** Instanzen.</span><span class="sxs-lookup"><span data-stu-id="8eddc-141">A **CookieHeaderValue** contains a collection of **CookieState** instances.</span></span> <span data-ttu-id="8eddc-142">Jede **CookieState** ein Cookie darstellt.</span><span class="sxs-lookup"><span data-stu-id="8eddc-142">Each **CookieState** represents one cookie.</span></span> <span data-ttu-id="8eddc-143">Verwenden Sie die Indexermethode zum Abrufen einer **CookieState** anhand des Namens, wie dargestellt.</span><span class="sxs-lookup"><span data-stu-id="8eddc-143">Use the indexer method to get a **CookieState** by name, as shown.</span></span>

## <a name="structured-cookie-data"></a><span data-ttu-id="8eddc-144">Strukturierte Cookiedaten</span><span class="sxs-lookup"><span data-stu-id="8eddc-144">Structured Cookie Data</span></span>

<span data-ttu-id="8eddc-145">Viele Browser einschränken, wie viele Cookies sie speichern&#8212;sowohl die Gesamtzahl und die Anzahl pro Domäne.</span><span class="sxs-lookup"><span data-stu-id="8eddc-145">Many browsers limit how many cookies they will store&#8212;both the total number, and the number per domain.</span></span> <span data-ttu-id="8eddc-146">Aus diesem Grund kann es nützlich, um ein einzelnes Cookie, statt mehrere Cookies festzulegen strukturierte Daten abgelegt sein.</span><span class="sxs-lookup"><span data-stu-id="8eddc-146">Therefore, it can be useful to put structured data into a single cookie, instead of setting multiple cookies.</span></span>

> [!NOTE]
> <span data-ttu-id="8eddc-147">RFC 6265 werden keine Cookies Datenstruktur definiert.</span><span class="sxs-lookup"><span data-stu-id="8eddc-147">RFC 6265 does not define the structure of cookie data.</span></span>


<span data-ttu-id="8eddc-148">Mithilfe der **CookieHeaderValue** -Klasse, Sie können eine Liste von Name / Wert-Paare für die Cookiedaten übergeben.</span><span class="sxs-lookup"><span data-stu-id="8eddc-148">Using the **CookieHeaderValue** class, you can pass a list of name-value pairs for the cookie data.</span></span> <span data-ttu-id="8eddc-149">Diese Name-Wert-Paare werden als URL-codiert-Daten in der Set-Cookie-Header codiert:</span><span class="sxs-lookup"><span data-stu-id="8eddc-149">These name-value pairs are encoded as URL-encoded form data in the Set-Cookie header:</span></span>

[!code-csharp[Main](http-cookies/samples/sample8.cs)]

<span data-ttu-id="8eddc-150">Der obige Code erzeugt die folgenden Set-Cookie-Headers:</span><span class="sxs-lookup"><span data-stu-id="8eddc-150">The previous code produces the following Set-Cookie header:</span></span>

[!code-powershell[Main](http-cookies/samples/sample9.ps1)]

<span data-ttu-id="8eddc-151">Die **CookieState** Klasse stellt eine Indexermethode, um die untergeordneten Werte aus einem Cookie in der Anforderungsnachricht zu lesen:</span><span class="sxs-lookup"><span data-stu-id="8eddc-151">The **CookieState** class provides an indexer method to read the sub-values from a cookie in the request message:</span></span>

[!code-csharp[Main](http-cookies/samples/sample10.cs)]

## <a name="example-set-and-retrieve-cookies-in-a-message-handler"></a><span data-ttu-id="8eddc-152">Beispiel: Festlegen und Abrufen von Cookies in einen Meldungshandler</span><span class="sxs-lookup"><span data-stu-id="8eddc-152">Example: Set and Retrieve Cookies in a Message Handler</span></span>

<span data-ttu-id="8eddc-153">In den vorherigen Beispielen wurde gezeigt, wie Cookies von innerhalb einer Web-API-Controller verwenden können.</span><span class="sxs-lookup"><span data-stu-id="8eddc-153">The previous examples showed how to use cookies from within a Web API controller.</span></span> <span data-ttu-id="8eddc-154">Eine andere Möglichkeit ist die Verwendung [message-Handler](http-message-handlers.md).</span><span class="sxs-lookup"><span data-stu-id="8eddc-154">Another option is to use [message handlers](http-message-handlers.md).</span></span> <span data-ttu-id="8eddc-155">Meldungshandler werden weiter oben in der Pipeline als Controller aufgerufen.</span><span class="sxs-lookup"><span data-stu-id="8eddc-155">Message handlers are invoked earlier in the pipeline than controllers.</span></span> <span data-ttu-id="8eddc-156">Message-Handler kann Cookies aus der Anforderung zu lesen, bevor die Anforderung den Domänencontroller erreicht, oder Cookies an die Antwort hinzufügen, nachdem der Controller die Antwort generiert.</span><span class="sxs-lookup"><span data-stu-id="8eddc-156">A message handler can read cookies from the request before the request reaches the controller, or add cookies to the response after the controller generates the response.</span></span>

![](http-cookies/_static/image2.png)

<span data-ttu-id="8eddc-157">Der folgende Code zeigt einen Meldungshandler für Sitzungs-IDs erstellen.</span><span class="sxs-lookup"><span data-stu-id="8eddc-157">The following code shows a message handler for creating session IDs.</span></span> <span data-ttu-id="8eddc-158">Die Sitzungs-ID wird in einem Cookie gespeichert.</span><span class="sxs-lookup"><span data-stu-id="8eddc-158">The session ID is stored in a cookie.</span></span> <span data-ttu-id="8eddc-159">Der Ereignishandler überprüft die Anforderung für das Sitzungscookie.</span><span class="sxs-lookup"><span data-stu-id="8eddc-159">The handler checks the request for the session cookie.</span></span> <span data-ttu-id="8eddc-160">Wenn die Anforderung nicht das Cookie enthält, generiert der Handler eine neue Sitzungs-ID.</span><span class="sxs-lookup"><span data-stu-id="8eddc-160">If the request does not include the cookie, the handler generates a new session ID.</span></span> <span data-ttu-id="8eddc-161">In beiden Fällen wird der Ereignishandler speichert die Sitzungs-ID in der **HttpRequestMessage.Properties** Eigenschaftenbehälter.</span><span class="sxs-lookup"><span data-stu-id="8eddc-161">In either case, the handler stores the session ID in the **HttpRequestMessage.Properties** property bag.</span></span> <span data-ttu-id="8eddc-162">Außerdem werden die HTTP-Antwort das Sitzungscookie hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="8eddc-162">It also adds the session cookie to the HTTP response.</span></span>

<span data-ttu-id="8eddc-163">Diese Implementierung überprüft nicht, dass die Sitzungs-ID vom Client tatsächlich vom Server ausgestellt wurde.</span><span class="sxs-lookup"><span data-stu-id="8eddc-163">This implementation does not validate that the session ID from the client was actually issued by the server.</span></span> <span data-ttu-id="8eddc-164">Verwenden Sie nicht als eine Art der Authentifizierung!</span><span class="sxs-lookup"><span data-stu-id="8eddc-164">Don't use it as a form of authentication!</span></span> <span data-ttu-id="8eddc-165">Zum Zeitpunkt des im Beispiel werden HTTP-cookieverwaltung anzeigen.</span><span class="sxs-lookup"><span data-stu-id="8eddc-165">The point of the example is to show HTTP cookie management.</span></span>

[!code-csharp[Main](http-cookies/samples/sample11.cs)]

<span data-ttu-id="8eddc-166">Ein Controller erhalten die Sitzungs-ID aus der **HttpRequestMessage.Properties** Eigenschaftenbehälter.</span><span class="sxs-lookup"><span data-stu-id="8eddc-166">A controller can get the session ID from the **HttpRequestMessage.Properties** property bag.</span></span>

[!code-csharp[Main](http-cookies/samples/sample12.cs)]
