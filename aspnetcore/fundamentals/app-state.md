---
title: Sitzung und Anwendungsstatus in ASP.NET Core
author: rick-anderson
description: "Ansätze zum Beibehalten der Anwendung und des Benutzerstatus (Sitzung) zwischen Anforderungen."
keywords: ASP.NET Core, Anwendungszustand, Sitzungszustand, Abfragezeichenfolgen, bereitstellen
ms.author: riande
manager: wpickett
ms.date: 11/27/2017
ms.topic: article
ms.assetid: 18cda488-0769-4cb9-82f6-4c6685f2045d
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/app-state
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 35b34f1a40e431e59e6b9c1d9bfb4ce3fced35e6
ms.sourcegitcommit: 8f42ab93402c1b8044815e1e48d0bb84c81f8b59
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/29/2017
---
# <a name="introduction-to-session-and-application-state-in-aspnet-core"></a><span data-ttu-id="5c98a-104">Einführung in die Sitzung und Anwendungsstatus in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="5c98a-104">Introduction to session and application state in ASP.NET Core</span></span>

<span data-ttu-id="5c98a-105">Durch [Rick Anderson](https://twitter.com/RickAndMSFT), [Steve Smith](https://ardalis.com/), und [Diana LaRose](https://github.com/DianaLaRose)</span><span class="sxs-lookup"><span data-stu-id="5c98a-105">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Steve Smith](https://ardalis.com/), and [Diana LaRose](https://github.com/DianaLaRose)</span></span>

<span data-ttu-id="5c98a-106">HTTP ist ein statusfreies Protokoll.</span><span class="sxs-lookup"><span data-stu-id="5c98a-106">HTTP is a stateless protocol.</span></span> <span data-ttu-id="5c98a-107">Ein Webserver jede HTTP-Anforderung als unabhängige Anforderung behandelt und behält die Benutzerwerte aus den vorherigen Anforderungen nicht.</span><span class="sxs-lookup"><span data-stu-id="5c98a-107">A web server treats each HTTP request as an independent request and does not retain user values from previous requests.</span></span> <span data-ttu-id="5c98a-108">Dieser Artikel beschreibt verschiedene Möglichkeiten, Anwendung und der Status der Sitzung zwischen den Anforderungen beizubehalten.</span><span class="sxs-lookup"><span data-stu-id="5c98a-108">This article discusses different ways to preserve application and session state between requests.</span></span> 

## <a name="session-state"></a><span data-ttu-id="5c98a-109">Sitzungsstatus</span><span class="sxs-lookup"><span data-stu-id="5c98a-109">Session state</span></span>

<span data-ttu-id="5c98a-110">Sitzungszustand ist ein Feature in ASP.NET Core, das Sie verwenden können, um Benutzerdaten zu speichern, während der Benutzer Ihre Web-App durchsucht.</span><span class="sxs-lookup"><span data-stu-id="5c98a-110">Session state is a feature in ASP.NET Core that you can use to save and store user data while the user browses your web app.</span></span> <span data-ttu-id="5c98a-111">Bestehend aus einem Wörterbuch oder Hash-Tabelle auf dem Server, bleiben Sitzungsstatus Daten anforderungsübergreifend in einem Browser.</span><span class="sxs-lookup"><span data-stu-id="5c98a-111">Consisting of a dictionary or hash table on the server, session state persists data across requests from a browser.</span></span> <span data-ttu-id="5c98a-112">Die Sitzungsdaten durch einen Cache gesichert.</span><span class="sxs-lookup"><span data-stu-id="5c98a-112">The session data is backed by a cache.</span></span>

<span data-ttu-id="5c98a-113">ASP.NET Core beibehalten Sitzungszustand, durch die Vergabe des Clients ein Cookie, das die Sitzungs-ID enthält, die mit jeder Anforderung an den Server gesendet wird.</span><span class="sxs-lookup"><span data-stu-id="5c98a-113">ASP.NET Core maintains session state by giving the client a cookie that contains the session ID, which is sent to the server with each request.</span></span> <span data-ttu-id="5c98a-114">Der Server verwendet die Sitzungs-ID, um die Sitzungsdaten abzurufen.</span><span class="sxs-lookup"><span data-stu-id="5c98a-114">The server uses the session ID to fetch the session data.</span></span> <span data-ttu-id="5c98a-115">Da das Sitzungscookie spezifisch für den Browser ist, kann nicht Browsern Sitzungen gemeinsam nutzen.</span><span class="sxs-lookup"><span data-stu-id="5c98a-115">Because the session cookie is specific to the browser, you cannot share sessions across browsers.</span></span> <span data-ttu-id="5c98a-116">Sitzungscookies sind nur gelöscht, wenn die Browsersitzung beendet.</span><span class="sxs-lookup"><span data-stu-id="5c98a-116">Session cookies are deleted only when the browser session ends.</span></span> <span data-ttu-id="5c98a-117">Wenn ein Cookie für eine Sitzung für abgelaufene empfangen wird, wird eine neue Sitzung, die das gleiche Sitzungscookie verwendet erstellt.</span><span class="sxs-lookup"><span data-stu-id="5c98a-117">If a cookie is received for an expired session, a new session that uses the same session cookie  is created.</span></span> 

<span data-ttu-id="5c98a-118">Der Server behält eine Sitzung für einen begrenzten Zeitraum nach der letzten Anforderung.</span><span class="sxs-lookup"><span data-stu-id="5c98a-118">The server retains a session for a limited time after the last request.</span></span> <span data-ttu-id="5c98a-119">Sie können das Sitzungstimeout festgelegt oder den Standardwert von 20 Minuten.</span><span class="sxs-lookup"><span data-stu-id="5c98a-119">You can either set the session timeout or use the default value of 20 minutes.</span></span> <span data-ttu-id="5c98a-120">Status der Sitzung ist ideal zum Speichern von Benutzerdaten, die bezieht sich auf einer bestimmten Sitzung jedoch nicht dauerhaft beibehalten werden müssen.</span><span class="sxs-lookup"><span data-stu-id="5c98a-120">Session state is ideal for storing user data that is specific to a particular session but doesn’t need to be persisted permanently.</span></span> <span data-ttu-id="5c98a-121">Daten werden gelöscht aus dem Sicherungsspeicher entweder beim Aufrufen von `Session.Clear` oder im Datenspeicher des Ablaufs der Sitzung.</span><span class="sxs-lookup"><span data-stu-id="5c98a-121">Data is deleted from the backing store either when you call `Session.Clear` or when the session expires in the data store.</span></span> <span data-ttu-id="5c98a-122">Der Server ist nicht bekannt, wenn der Browser geschlossen wird oder wenn das Sitzungscookie gelöscht wird.</span><span class="sxs-lookup"><span data-stu-id="5c98a-122">The server does not know when the browser is closed or when the session cookie is deleted.</span></span>

> [!WARNING]
> <span data-ttu-id="5c98a-123">Speichern Sie vertrauliche Daten nicht in der Sitzung.</span><span class="sxs-lookup"><span data-stu-id="5c98a-123">Do not store sensitive data in session.</span></span> <span data-ttu-id="5c98a-124">Der Client möglicherweise nicht den Browser schließen und löschen Sie das Sitzungscookie (und von einigen Browsern erhalten Sitzungscookies über Windows).</span><span class="sxs-lookup"><span data-stu-id="5c98a-124">The client might not close the browser and clear the session cookie (and some browsers keep session cookies alive across windows).</span></span> <span data-ttu-id="5c98a-125">Darüber hinaus kann eine Sitzung nicht auf einen einzelnen Benutzer beschränkt sein; der nächste Benutzer möglicherweise mit der gleichen Sitzung fortgesetzt.</span><span class="sxs-lookup"><span data-stu-id="5c98a-125">Also, a session might not be restricted to a single user; the next user might continue with the same session.</span></span>

<span data-ttu-id="5c98a-126">Die in-Memory-sitzungsanbieters speichert Sitzungsdaten auf dem lokalen Server.</span><span class="sxs-lookup"><span data-stu-id="5c98a-126">The in-memory session provider stores session data on the local server.</span></span> <span data-ttu-id="5c98a-127">Wenn Sie beabsichtigen, Ihre Web-app auf einer Serverfarm auszuführen, müssen Sie persistente Sitzungen verwenden, um jede Sitzung an einen bestimmten Server zu verbinden.</span><span class="sxs-lookup"><span data-stu-id="5c98a-127">If you plan to run your web app on a server farm, you must use sticky sessions to tie each session to a specific server.</span></span> <span data-ttu-id="5c98a-128">Die Websites der Windows Azure-Plattform wird standardmäßig auf persistente Sitzungen (Application Request Routing oder ARR).</span><span class="sxs-lookup"><span data-stu-id="5c98a-128">The Windows Azure Web Sites platform defaults to sticky sessions (Application Request Routing or ARR).</span></span> <span data-ttu-id="5c98a-129">Allerdings können persistente Sitzungen Auswirkungen auf die Skalierbarkeit und Web-app-Updates erschweren.</span><span class="sxs-lookup"><span data-stu-id="5c98a-129">However, sticky sessions can affect scalability and complicate web app updates.</span></span> <span data-ttu-id="5c98a-130">Eine bessere Option ist die Verwendung des Redis oder verteilten SQL Server zwischengespeichert werden, die nicht persistente Sitzungen erfordern.</span><span class="sxs-lookup"><span data-stu-id="5c98a-130">A better option is to use the Redis or SQL Server distributed caches, which don't require sticky sessions.</span></span> <span data-ttu-id="5c98a-131">Weitere Informationen finden Sie unter [arbeiten mit einem verteilten Cache](xref:performance/caching/distributed).</span><span class="sxs-lookup"><span data-stu-id="5c98a-131">For more information, see [Working with a Distributed Cache](xref:performance/caching/distributed).</span></span> <span data-ttu-id="5c98a-132">Weitere Informationen zum Einrichten der Dienstanbieter, finden Sie unter [konfigurieren Sitzung](#configuring-session) weiter unten in diesem Artikel.</span><span class="sxs-lookup"><span data-stu-id="5c98a-132">For details on setting up service providers, see [Configuring Session](#configuring-session) later in this article.</span></span>

<a name="temp"></a>
## <a name="tempdata"></a><span data-ttu-id="5c98a-133">TempData</span><span class="sxs-lookup"><span data-stu-id="5c98a-133">TempData</span></span>

<span data-ttu-id="5c98a-134">ASP.NET Core MVC macht die [TempData](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.controller.tempdata?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_Controller_TempData) Eigenschaft auf einen [Controller](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.controller?view=aspnetcore-2.0).</span><span class="sxs-lookup"><span data-stu-id="5c98a-134">ASP.NET Core MVC exposes the [TempData](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.controller.tempdata?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_Controller_TempData) property on a [controller](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.controller?view=aspnetcore-2.0).</span></span> <span data-ttu-id="5c98a-135">Diese Eigenschaft speichert Daten, bis sie gelesen wurden.</span><span class="sxs-lookup"><span data-stu-id="5c98a-135">This property stores data until it is read.</span></span> <span data-ttu-id="5c98a-136">Die Methoden `Keep` und `Peek` können verwendet werden, um die Daten zu überprüfen, ohne sie zu löschen.</span><span class="sxs-lookup"><span data-stu-id="5c98a-136">The `Keep` and `Peek` methods can be used to examine the data without deletion.</span></span> <span data-ttu-id="5c98a-137">`TempData`ist besonders nützlich für die Umleitung, wenn Daten für mehr als eine einzelne Anforderung benötigt werden.</span><span class="sxs-lookup"><span data-stu-id="5c98a-137">`TempData` is particularly useful for redirection, when data is needed for more than a single request.</span></span> <span data-ttu-id="5c98a-138">`TempData`wird von TempData-Anbieter, z. B. implementiert mithilfe von Cookies oder Sitzungsstatus.</span><span class="sxs-lookup"><span data-stu-id="5c98a-138">`TempData` is implemented by TempData providers, for example, using either cookies or session state.</span></span>

<a name="tempdata-providers"></a>
### <a name="tempdata-providers"></a><span data-ttu-id="5c98a-139">TempData-Anbieter</span><span class="sxs-lookup"><span data-stu-id="5c98a-139">TempData providers</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="5c98a-140">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="5c98a-140">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="5c98a-141">In ASP.NET Core 2.0 und höher, wird standardmäßig der Cookie-basierte TempData-Anbieter verwendet, um TempData in Cookies zu speichern.</span><span class="sxs-lookup"><span data-stu-id="5c98a-141">In ASP.NET Core 2.0 and later, the cookie-based TempData provider is used by default to store TempData in cookies.</span></span>

<span data-ttu-id="5c98a-142">Ist die Cookiedaten codiert die [Base64UrlTextEncoder](https://docs.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.webutilities.base64urltextencoder?view=aspnetcore-2.0).</span><span class="sxs-lookup"><span data-stu-id="5c98a-142">The cookie data is encoded with the [Base64UrlTextEncoder](https://docs.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.webutilities.base64urltextencoder?view=aspnetcore-2.0).</span></span> <span data-ttu-id="5c98a-143">Daran, dass das Cookie verschlüsselt und aus Ihren Segmenten zusammengesetzt wird, Größe der einzelne Cookie Grenzwert gefunden in ASP.NET Core 1.x nicht gilt.</span><span class="sxs-lookup"><span data-stu-id="5c98a-143">Because the cookie is encrypted and chunked, the single cookie size limit found in ASP.NET Core 1.x does not apply.</span></span> <span data-ttu-id="5c98a-144">Die Cookiedaten werden nicht komprimiert werden, da das Komprimieren verschlüsselter Daten Sicherheitsprobleme wie führen, kann die [CRIME](https://wikipedia.org/wiki/CRIME_(security_exploit)) und [Verletzung](https://wikipedia.org/wiki/BREACH_(security_exploit)) Angriffe.</span><span class="sxs-lookup"><span data-stu-id="5c98a-144">The cookie data is not compressed because compressing encrypted data can lead to security problems such as the [CRIME](https://wikipedia.org/wiki/CRIME_(security_exploit)) and [BREACH](https://wikipedia.org/wiki/BREACH_(security_exploit)) attacks.</span></span> <span data-ttu-id="5c98a-145">Weitere Informationen für den Cookie-basierte TempData-Anbieter finden Sie unter [CookieTempDataProvider](https://github.com/aspnet/Mvc/blob/dev/src/Microsoft.AspNetCore.Mvc.ViewFeatures/ViewFeatures/CookieTempDataProvider.cs).</span><span class="sxs-lookup"><span data-stu-id="5c98a-145">For more information on the cookie-based TempData provider, see [CookieTempDataProvider](https://github.com/aspnet/Mvc/blob/dev/src/Microsoft.AspNetCore.Mvc.ViewFeatures/ViewFeatures/CookieTempDataProvider.cs).</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="5c98a-146">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="5c98a-146">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="5c98a-147">In ASP.NET Core 1.0 und 1.1 ist der TempData Sitzungsstatusanbieter die Standardeinstellung.</span><span class="sxs-lookup"><span data-stu-id="5c98a-147">In ASP.NET Core 1.0 and 1.1, the session state TempData provider is the default.</span></span>

--------------

<a name="choose-temp"></a>
### <a name="choosing-a-tempdata-provider"></a><span data-ttu-id="5c98a-148">TempData-Anbieter auswählen</span><span class="sxs-lookup"><span data-stu-id="5c98a-148">Choosing a TempData provider</span></span>

<span data-ttu-id="5c98a-149">Auswählen eines Anbieters TempData umfasst verschiedene Aspekte im Zusammenhang, z. B.:</span><span class="sxs-lookup"><span data-stu-id="5c98a-149">Choosing a TempData provider involves several considerations, such as:</span></span>

1. <span data-ttu-id="5c98a-150">Wird die Anwendung bereits für andere Zwecke Sitzungsstatus verwendet?</span><span class="sxs-lookup"><span data-stu-id="5c98a-150">Does the application already use session state for other purposes?</span></span> <span data-ttu-id="5c98a-151">Wenn dies der Fall ist, hat die Verwendung des TempData sitzungsstatusanbieters ohne zusätzliche Kosten für die Anwendung (abgesehen von der Größe der Daten).</span><span class="sxs-lookup"><span data-stu-id="5c98a-151">If so, using the session state TempData provider has no additional cost to the application (aside from the size of the data).</span></span>
2. <span data-ttu-id="5c98a-152">Werden die Anwendung TempData nur selten für relativ kleine Datenmengen (bis zu 500 Bytes) verwendet?</span><span class="sxs-lookup"><span data-stu-id="5c98a-152">Does the application use TempData only sparingly, for relatively small amounts of data (up to 500 bytes)?</span></span> <span data-ttu-id="5c98a-153">Wenn also der Cookie-TempData-Anbieter eine kleine Kosten für jede Anforderung hinzufügen, die TempData führt.</span><span class="sxs-lookup"><span data-stu-id="5c98a-153">If so, the cookie TempData provider will add a small cost to each request that carries TempData.</span></span> <span data-ttu-id="5c98a-154">Wenn dies nicht der Fall ist, wird der TempData Sitzungsstatusanbieter kann nützlich sein, Round-Tripping eine große Menge von Daten in jeder Anforderung vermeiden, bis die TempData genutzt wird.</span><span class="sxs-lookup"><span data-stu-id="5c98a-154">If not, the session state TempData provider can be beneficial to avoid round-tripping a large amount of data in each request until the TempData is consumed.</span></span>
3. <span data-ttu-id="5c98a-155">Werden die Anwendung wird in einer Webfarm (mehrere Server) ausgeführt?</span><span class="sxs-lookup"><span data-stu-id="5c98a-155">Does the application run in a web farm (multiple servers)?</span></span> <span data-ttu-id="5c98a-156">Wenn dies der Fall ist, es ist keine zusätzliche Konfiguration erforderlich, um die Cookie-TempData-Anbieter verwenden.</span><span class="sxs-lookup"><span data-stu-id="5c98a-156">If so, there is no additional configuration needed to use the cookie TempData provider.</span></span>

> [!NOTE]
> <span data-ttu-id="5c98a-157">Die meisten Webclients (z. B. Webbrowser) zu erzwingen Grenzwerte für die maximale Größe der einzelnen Cookies, die Gesamtanzahl von Cookies oder beide.</span><span class="sxs-lookup"><span data-stu-id="5c98a-157">Most web clients (such as web browsers) enforce limits on the maximum size of each cookie, the total number of cookies, or both.</span></span> <span data-ttu-id="5c98a-158">Wenn das Cookie TempData-Anbieter verwenden, überprüfen Sie daher, dass die app beim Überschreiten dieser Grenzwerte wird nicht.</span><span class="sxs-lookup"><span data-stu-id="5c98a-158">Therefore, when using the cookie TempData provider, verify the app won't exceed these limits.</span></span> <span data-ttu-id="5c98a-159">Betrachten Sie die Gesamtgröße der Daten, für die Kosten der Verschlüsselung Kontoführung und Segmentierung.</span><span class="sxs-lookup"><span data-stu-id="5c98a-159">Consider the total size of the data, accounting for the overheads of encryption and chunking.</span></span>

<a name="config-temp"></a>
### <a name="configure-the-tempdata-provider"></a><span data-ttu-id="5c98a-160">Konfigurieren des TempData-Anbieters</span><span class="sxs-lookup"><span data-stu-id="5c98a-160">Configure the TempData provider</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="5c98a-161">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="5c98a-161">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="5c98a-162">Der Cookie-basierte TempData-Anbieter ist standardmäßig aktiviert.</span><span class="sxs-lookup"><span data-stu-id="5c98a-162">The cookie-based TempData provider is enabled by default.</span></span> <span data-ttu-id="5c98a-163">Die folgenden `Startup` Klassencode dient zum Konfigurieren des sitzungsbasierte TempData-Anbieters:</span><span class="sxs-lookup"><span data-stu-id="5c98a-163">The following `Startup` class code configures the session-based TempData provider:</span></span>

[!code-csharp[](app-state/sample/src/WebAppSessionDotNetCore2.0App/StartupTempDataSession.cs?name=snippet_TempDataSession&highlight=4,6,11)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="5c98a-164">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="5c98a-164">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="5c98a-165">Die folgenden `Startup` Klassencode dient zum Konfigurieren des sitzungsbasierte TempData-Anbieters:</span><span class="sxs-lookup"><span data-stu-id="5c98a-165">The following `Startup` class code configures the session-based TempData provider:</span></span>

[!code-csharp[](app-state/sample/src/WebAppSession/StartupTempDataSession.cs?name=snippet_TempDataSession&highlight=4,9)]

---

<span data-ttu-id="5c98a-166">Reihenfolge ist wichtig für middlewarekomponenten gemeinsam sind.</span><span class="sxs-lookup"><span data-stu-id="5c98a-166">Ordering is critical for middleware components.</span></span> <span data-ttu-id="5c98a-167">Im vorherigen Beispiel, eine Ausnahme vom Typ `InvalidOperationException` tritt auf, wenn `UseSession` wird aufgerufen, nachdem `UseMvcWithDefaultRoute`.</span><span class="sxs-lookup"><span data-stu-id="5c98a-167">In the preceding example, an exception of type `InvalidOperationException` occurs when `UseSession` is invoked after `UseMvcWithDefaultRoute`.</span></span> <span data-ttu-id="5c98a-168">Finden Sie unter [Middleware Sortierung](xref:fundamentals/middleware#ordering) für weitere Details.</span><span class="sxs-lookup"><span data-stu-id="5c98a-168">See [Middleware Ordering](xref:fundamentals/middleware#ordering) for more detail.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="5c98a-169">Wenn .NET Framework abzielt, und Verwenden des Anbieters sitzungsbasierte Hinzufügen der [Microsoft.AspNetCore.Session](https://www.nuget.org/packages/Microsoft.AspNetCore.Session) NuGet-Paket zum Projekt.</span><span class="sxs-lookup"><span data-stu-id="5c98a-169">If targeting .NET Framework and using the session-based provider, add the [Microsoft.AspNetCore.Session](https://www.nuget.org/packages/Microsoft.AspNetCore.Session) NuGet package to your project.</span></span>

## <a name="query-strings"></a><span data-ttu-id="5c98a-170">Abfragezeichenfolgen</span><span class="sxs-lookup"><span data-stu-id="5c98a-170">Query strings</span></span>

<span data-ttu-id="5c98a-171">Sie können eine begrenzte Menge von Daten aus einer Anforderung in eine andere übergeben, indem Abfragezeichenfolge für die neue Anforderung hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="5c98a-171">You can pass a limited amount of data from one request to another by adding it to the new request’s query string.</span></span> <span data-ttu-id="5c98a-172">Dies ist nützlich zum Aufzeichnen des Status in einer persistenten Weise, die Links mit eingebetteten Status über e-Mail oder soziale Netzwerke freigegeben werden kann.</span><span class="sxs-lookup"><span data-stu-id="5c98a-172">This is useful for capturing state in a persistent manner that allows links with embedded state to be shared through email or social networks.</span></span> <span data-ttu-id="5c98a-173">Allerdings sollten aus diesem Grund Sie nie Abfragezeichenfolgen für vertrauliche Daten verwenden.</span><span class="sxs-lookup"><span data-stu-id="5c98a-173">However, for this reason, you should never use query strings for sensitive data.</span></span> <span data-ttu-id="5c98a-174">Zusätzlich zu einfach freigegeben, einschließlich der Daten in Abfragezeichenfolgen kann erstellen Verkaufschancen für [Cross-Site Request Fälschung Websiteübergreifender](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF)) Angriffe, bei denen Benutzer für den Zugriff auf bösartige Websites während authentifiziert bringen können.</span><span class="sxs-lookup"><span data-stu-id="5c98a-174">In addition to being easily shared, including data in query strings can create opportunities for [Cross-Site Request Forgery (CSRF)](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF)) attacks, which can trick users into visiting malicious sites while authenticated.</span></span> <span data-ttu-id="5c98a-175">Angreifer können dann Benutzerdaten aus Ihrer app zu stehlen oder böswilligen Aktionen im Namen des Benutzers.</span><span class="sxs-lookup"><span data-stu-id="5c98a-175">Attackers can then steal user data from your app or take malicious actions on behalf of the user.</span></span> <span data-ttu-id="5c98a-176">Alle beibehaltenen Zustand der Anwendung oder die Sitzung muss vor CSRF-Angriffen schützen.</span><span class="sxs-lookup"><span data-stu-id="5c98a-176">Any preserved application or session state must protect against CSRF attacks.</span></span> <span data-ttu-id="5c98a-177">Weitere Informationen zu CSRF-Angriffen, finden Sie unter [verhindern Cross-Site Request XSRF/Websiteübergreifender Anforderungsfälschung Angriffe in ASP.NET Core](../security/anti-request-forgery.md).</span><span class="sxs-lookup"><span data-stu-id="5c98a-177">For more information on CSRF attacks, see [Preventing Cross-Site Request Forgery (XSRF/CSRF) Attacks in ASP.NET Core](../security/anti-request-forgery.md).</span></span>

## <a name="post-data-and-hidden-fields"></a><span data-ttu-id="5c98a-178">Bereitgestellte Daten und ausgeblendete Felder</span><span class="sxs-lookup"><span data-stu-id="5c98a-178">Post data and hidden fields</span></span>

<span data-ttu-id="5c98a-179">Daten können in ausgeblendeten Formularfelder gespeichert und wieder auf die nächste Anforderung gesendet werden.</span><span class="sxs-lookup"><span data-stu-id="5c98a-179">Data can be saved in hidden form fields and posted back on the next request.</span></span> <span data-ttu-id="5c98a-180">Dies ist häufig in mehreren Seiten Formularen.</span><span class="sxs-lookup"><span data-stu-id="5c98a-180">This is common in multi-page forms.</span></span> <span data-ttu-id="5c98a-181">Jedoch, da der Client möglicherweise die Daten manipulieren kann, muss der Server immer es erneut überprüfen.</span><span class="sxs-lookup"><span data-stu-id="5c98a-181">However, because the client can potentially tamper with the data, the server must always revalidate it.</span></span> 

## <a name="cookies"></a><span data-ttu-id="5c98a-182">Cookies</span><span class="sxs-lookup"><span data-stu-id="5c98a-182">Cookies</span></span>

<span data-ttu-id="5c98a-183">Cookies bieten eine Möglichkeit zum Speichern von benutzerspezifischen Daten in Webanwendungen.</span><span class="sxs-lookup"><span data-stu-id="5c98a-183">Cookies provide a way to store user-specific data in web applications.</span></span> <span data-ttu-id="5c98a-184">Da Cookies mit jeder Anforderung gesendet werden, sollte ihre Größe auf ein Minimum begrenzt.</span><span class="sxs-lookup"><span data-stu-id="5c98a-184">Because cookies are sent with every request, their size should be kept to a minimum.</span></span> <span data-ttu-id="5c98a-185">Im Idealfall sollten lediglich ein Bezeichner mit den tatsächlichen Daten, die auf dem Server gespeicherten in einem Cookie gespeichert werden.</span><span class="sxs-lookup"><span data-stu-id="5c98a-185">Ideally, only an identifier should be stored in a cookie with the actual data stored on the server.</span></span> <span data-ttu-id="5c98a-186">Die meisten Browser beschränken Cookies auf 4096 Bytes.</span><span class="sxs-lookup"><span data-stu-id="5c98a-186">Most browsers restrict cookies to 4096 bytes.</span></span> <span data-ttu-id="5c98a-187">Darüber hinaus sind nur eine begrenzte Anzahl von Cookies für jede Domäne verfügbar.</span><span class="sxs-lookup"><span data-stu-id="5c98a-187">In addition, only a limited number of cookies are available for each domain.</span></span>  

<span data-ttu-id="5c98a-188">Da Cookies sind leichter manipuliert werden, müssen sie auf dem Server überprüft werden.</span><span class="sxs-lookup"><span data-stu-id="5c98a-188">Because cookies are subject to tampering, they must be validated on the server.</span></span> <span data-ttu-id="5c98a-189">Die Dauerhaftigkeit des Cookies auf einem Client unterliegt zwar Eingreifen des Benutzers und das Ablaufdatum, sind sie in der Regel die meisten permanenten Form der Dauerhaftigkeit der Daten auf dem Client.</span><span class="sxs-lookup"><span data-stu-id="5c98a-189">Although the durability of the cookie on a client is subject to user intervention and expiration, they are generally the most durable form of data persistence on the client.</span></span>

<span data-ttu-id="5c98a-190">Cookies werden für die Personalisierung, häufig verwendet, in dem Inhalte für einen bekannten Benutzer angepasst.</span><span class="sxs-lookup"><span data-stu-id="5c98a-190">Cookies are often used for personalization, where content is customized for a known user.</span></span> <span data-ttu-id="5c98a-191">Da der Benutzer nur ermittelt und in den meisten Fällen nicht authentifiziert, können Sie einen Cookie in der Regel sichern, indem Sie den Benutzernamen, Kontoname oder eine eindeutige Benutzer-ID (z. B. eine GUID) im Cookie speichern.</span><span class="sxs-lookup"><span data-stu-id="5c98a-191">Because the user is only identified and not authenticated in most cases, you can typically secure a cookie by storing the user name, account name, or a unique user ID (such as a GUID) in the cookie.</span></span> <span data-ttu-id="5c98a-192">Das Cookie können dann die Benutzer Personalisierungsinfrastruktur einer Website zugreifen.</span><span class="sxs-lookup"><span data-stu-id="5c98a-192">You can then use the cookie to access the user personalization infrastructure of a site.</span></span>

## <a name="httpcontextitems"></a><span data-ttu-id="5c98a-193">HttpContext.Items</span><span class="sxs-lookup"><span data-stu-id="5c98a-193">HttpContext.Items</span></span>

<span data-ttu-id="5c98a-194">Die `Items` Auflistung ist ein guter Speicherort zum Speichern von Daten, die erforderlich sind nur while-Verarbeitung einer bestimmten Anforderung.</span><span class="sxs-lookup"><span data-stu-id="5c98a-194">The `Items` collection is a good location to store data that is needed only while processing one particular request.</span></span> <span data-ttu-id="5c98a-195">Der Inhalt der Auflistung werden nach jeder Anforderung verworfen.</span><span class="sxs-lookup"><span data-stu-id="5c98a-195">The collection's contents are discarded after each request.</span></span> <span data-ttu-id="5c98a-196">Die `Items` Auflistung wird am besten als eine Möglichkeit, Komponenten oder Middleware kommunizieren kann, wenn sie zu unterschiedlichen Zeitpunkten ausgeführt, in der Zeit, während eine Anforderung werden und keine direkte Möglichkeit zum Übergeben von Parametern haben verwendet.</span><span class="sxs-lookup"><span data-stu-id="5c98a-196">The `Items` collection is best used as a way for components or middleware to communicate when they operate at different points in time during a request and have no direct way to pass parameters.</span></span> <span data-ttu-id="5c98a-197">Weitere Informationen finden Sie unter [arbeiten mit HttpContext.Items](#working-with-httpcontextitems)weiter unten in diesem Artikel.</span><span class="sxs-lookup"><span data-stu-id="5c98a-197">For more information, see [Working with HttpContext.Items](#working-with-httpcontextitems), later in this article.</span></span>

## <a name="cache"></a><span data-ttu-id="5c98a-198">cache</span><span class="sxs-lookup"><span data-stu-id="5c98a-198">Cache</span></span>

<span data-ttu-id="5c98a-199">Caching ist eine effiziente Möglichkeit zum Speichern und Abrufen von Daten.</span><span class="sxs-lookup"><span data-stu-id="5c98a-199">Caching is an efficient way to store and retrieve data.</span></span> <span data-ttu-id="5c98a-200">Sie können die Lebensdauer für zwischengespeicherte Elemente basierend auf Uhrzeit und andere Faktoren steuern.</span><span class="sxs-lookup"><span data-stu-id="5c98a-200">You can control the lifetime of cached items based on time and other considerations.</span></span> <span data-ttu-id="5c98a-201">Erfahren Sie mehr über [Caching](../performance/caching/index.md).</span><span class="sxs-lookup"><span data-stu-id="5c98a-201">Learn more about [Caching](../performance/caching/index.md).</span></span>

<a name="session"></a>
## <a name="working-with-session-state"></a><span data-ttu-id="5c98a-202">Arbeiten mit Sitzungsstatus</span><span class="sxs-lookup"><span data-stu-id="5c98a-202">Working with Session State</span></span>

### <a name="configuring-session"></a><span data-ttu-id="5c98a-203">Konfigurieren der Sitzung</span><span class="sxs-lookup"><span data-stu-id="5c98a-203">Configuring Session</span></span>

<span data-ttu-id="5c98a-204">Die `Microsoft.AspNetCore.Session` Paket stellt Middleware für die Verwaltung des Sitzungsstatus bereit.</span><span class="sxs-lookup"><span data-stu-id="5c98a-204">The `Microsoft.AspNetCore.Session` package provides middleware for managing session state.</span></span> <span data-ttu-id="5c98a-205">So aktivieren Sie die Sitzung Middleware `Startup` muss enthalten:</span><span class="sxs-lookup"><span data-stu-id="5c98a-205">To enable the session middleware, `Startup` must contain:</span></span>

- <span data-ttu-id="5c98a-206">Keines der [IDistributedCache](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.caching.distributed.idistributedcache) arbeitsspeichercaches.</span><span class="sxs-lookup"><span data-stu-id="5c98a-206">Any of the [IDistributedCache](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.caching.distributed.idistributedcache) memory caches.</span></span> <span data-ttu-id="5c98a-207">Die `IDistributedCache` Implementierung ist für Sitzung als Sicherungsspeicher verwendet.</span><span class="sxs-lookup"><span data-stu-id="5c98a-207">The `IDistributedCache` implementation is used as a backing store for session.</span></span>
- <span data-ttu-id="5c98a-208">[AddSession](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.dependencyinjection.sessionservicecollectionextensions#Microsoft_Extensions_DependencyInjection_SessionServiceCollectionExtensions_AddSession_Microsoft_Extensions_DependencyInjection_IServiceCollection_) aufzurufen, erfordert die NuGet-Paket "Microsoft.AspNetCore.Session".</span><span class="sxs-lookup"><span data-stu-id="5c98a-208">[AddSession](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.dependencyinjection.sessionservicecollectionextensions#Microsoft_Extensions_DependencyInjection_SessionServiceCollectionExtensions_AddSession_Microsoft_Extensions_DependencyInjection_IServiceCollection_) call, which requires NuGet package "Microsoft.AspNetCore.Session".</span></span>
- <span data-ttu-id="5c98a-209">[UseSession](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.sessionmiddlewareextensions#methods_) aufrufen.</span><span class="sxs-lookup"><span data-stu-id="5c98a-209">[UseSession](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.sessionmiddlewareextensions#methods_) call.</span></span>

<span data-ttu-id="5c98a-210">Der folgende Code zeigt, wie der Sitzungsanbieter in-Memory-eingerichtet wird.</span><span class="sxs-lookup"><span data-stu-id="5c98a-210">The following code shows how to set up the in-memory session provider.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="5c98a-211">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="5c98a-211">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

[!code-csharp[Main](app-state/sample/src/WebAppSessionDotNetCore2.0App/Startup.cs?highlight=11-19,24)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="5c98a-212">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="5c98a-212">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

[!code-csharp[Main](app-state/sample/src/WebAppSession/Startup.cs?highlight=11-19,24)]

---

<span data-ttu-id="5c98a-213">Sie können die Sitzung von verweisen `HttpContext` nach installiert und konfiguriert ist.</span><span class="sxs-lookup"><span data-stu-id="5c98a-213">You can reference Session from `HttpContext` once it is installed and configured.</span></span>

<span data-ttu-id="5c98a-214">Wenn Sie versuchen, den Zugriff auf `Session` vor `UseSession` aufgerufen wurde, die Ausnahme `InvalidOperationException: Session has not been configured for this application or request` ausgelöst wird.</span><span class="sxs-lookup"><span data-stu-id="5c98a-214">If you try to access `Session` before `UseSession` has been called, the exception `InvalidOperationException: Session has not been configured for this application or request` is thrown.</span></span>

<span data-ttu-id="5c98a-215">Wenn Sie versuchen, ein neues erstellen `Session` (d. h. keine Sitzungscookie erstellt wurde), nachdem Sie das Schreiben in bereits begonnen haben die `Response` stream, der die Ausnahme `InvalidOperationException: The session cannot be established after the response has started` ausgelöst wird.</span><span class="sxs-lookup"><span data-stu-id="5c98a-215">If you try to create a new `Session` (that is, no session cookie has been created) after you have already begun writing to the `Response` stream, the exception `InvalidOperationException: The session cannot be established after the response has started` is thrown.</span></span> <span data-ttu-id="5c98a-216">Die Ausnahme finden Sie in der Web-Server-Protokoll; Es wird nicht im Browser angezeigt werden.</span><span class="sxs-lookup"><span data-stu-id="5c98a-216">The exception can be found in the web server log; it will not be displayed in the browser.</span></span>

### <a name="loading-session-asynchronously"></a><span data-ttu-id="5c98a-217">Laden die Sitzung asynchron</span><span class="sxs-lookup"><span data-stu-id="5c98a-217">Loading Session asynchronously</span></span> 

<span data-ttu-id="5c98a-218">Der Standardanbieter für die Sitzung in ASP.NET Core lädt die Sitzung Datensatz aus der zugrunde liegenden [IDistributedCache](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.caching.distributed.idistributedcache) Store asynchron nur, wenn die [ISession.LoadAsync](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.http.isession#Microsoft_AspNetCore_Http_ISession_LoadAsync) -Methode explizit aufgerufen wird, bevor  die `TryGetValue`, `Set`, oder `Remove` Methoden.</span><span class="sxs-lookup"><span data-stu-id="5c98a-218">The default session provider in ASP.NET Core loads the session record from the underlying [IDistributedCache](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.caching.distributed.idistributedcache) store asynchronously only if the [ISession.LoadAsync](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.http.isession#Microsoft_AspNetCore_Http_ISession_LoadAsync) method is explicitly called before  the `TryGetValue`, `Set`, or `Remove` methods.</span></span> <span data-ttu-id="5c98a-219">Wenn `LoadAsync` nicht zuerst aufgerufen wird, die zugrunde liegende Sitzung Datensatz synchron geladen wird, was kann potenziell die Möglichkeit zum Skalieren der app auswirken.</span><span class="sxs-lookup"><span data-stu-id="5c98a-219">If `LoadAsync` is not called first, the underlying session record is loaded synchronously, which could potentially impact the ability of the app to scale.</span></span>

<span data-ttu-id="5c98a-220">Damit Anwendungen, die dieses Muster zu erzwingen, umschließen die [DistributedSessionStore](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.session.distributedsessionstore) und [DistributedSession](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.session.distributedsession) Implementierungen mit Versionen, die eine Ausnahme auszulösen, wenn die `LoadAsync` Methode ist nicht wird aufgerufen, bevor `TryGetValue`, `Set`, oder `Remove`.</span><span class="sxs-lookup"><span data-stu-id="5c98a-220">To have applications enforce this pattern, wrap the [DistributedSessionStore](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.session.distributedsessionstore) and [DistributedSession](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.session.distributedsession) implementations with versions that throw an exception if the `LoadAsync` method is not called before `TryGetValue`, `Set`, or `Remove`.</span></span> <span data-ttu-id="5c98a-221">Die umschlossenen Versionen im Container zu registrieren.</span><span class="sxs-lookup"><span data-stu-id="5c98a-221">Register the wrapped versions in the services container.</span></span>

### <a name="implementation-details"></a><span data-ttu-id="5c98a-222">Details zur Implementierung</span><span class="sxs-lookup"><span data-stu-id="5c98a-222">Implementation Details</span></span>

<span data-ttu-id="5c98a-223">Sitzung wird ein Cookie verwendet, verfolgen und Anforderungen von einer einzelnen Browsersitzung bestimmen.</span><span class="sxs-lookup"><span data-stu-id="5c98a-223">Session uses a cookie to track and identify requests from a single browser.</span></span> <span data-ttu-id="5c98a-224">Standardmäßig lautet dieses Cookie ". AspNet.Session", und verwendet den Pfad"/".</span><span class="sxs-lookup"><span data-stu-id="5c98a-224">By default, this cookie is named ".AspNet.Session", and it uses a path of "/".</span></span> <span data-ttu-id="5c98a-225">Da der Cookie-Standardwert keine Domäne angegeben ist, es ist nicht zur Verfügung gestellt des clientseitigen Skripts auf der Seite (da `CookieHttpOnly` standardmäßig `true`).</span><span class="sxs-lookup"><span data-stu-id="5c98a-225">Because the cookie default does not specify a domain, it is not made available to the client-side script on the page (because `CookieHttpOnly` defaults to `true`).</span></span>

<span data-ttu-id="5c98a-226">Um Sitzung Standardwerte zu überschreiben, verwenden `SessionOptions`:</span><span class="sxs-lookup"><span data-stu-id="5c98a-226">To override session defaults, use `SessionOptions`:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="5c98a-227">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="5c98a-227">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

[!code-csharp[Main](app-state/sample/src/WebAppSessionDotNetCore2.0App/StartupCopy.cs?name=snippet1&highlight=8-12)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="5c98a-228">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="5c98a-228">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

[!code-csharp[Main](app-state/sample/src/WebAppSession/StartupCopy.cs?name=snippet1&highlight=8-12)]

---

<span data-ttu-id="5c98a-229">Der Server verwendet die `IdleTimeout` -Eigenschaft können Sie bestimmen, wie lange eine Sitzung im Leerlauf befinden kann, bevor Sie seinen Inhalt abgebrochen werden.</span><span class="sxs-lookup"><span data-stu-id="5c98a-229">The server uses the `IdleTimeout` property to determine how long a session can be idle before its contents are abandoned.</span></span> <span data-ttu-id="5c98a-230">Diese Eigenschaft ist unabhängig von den Ablauf der Cookies.</span><span class="sxs-lookup"><span data-stu-id="5c98a-230">This property is independent of the cookie expiration.</span></span> <span data-ttu-id="5c98a-231">Jede Anforderung, die die Sitzung Middleware (aus gelesen oder geschrieben) durchlaufen setzt das Timeout an.</span><span class="sxs-lookup"><span data-stu-id="5c98a-231">Each request that passes through the Session middleware (read from or written to) resets the timeout.</span></span>

<span data-ttu-id="5c98a-232">Da `Session` ist *nicht sperrende*, wenn zwei Anforderungen, die beide versuchen, zum Ändern des Inhalts der Sitzung, für das letzte Lesezeichen überschreibt das erste.</span><span class="sxs-lookup"><span data-stu-id="5c98a-232">Because `Session` is *non-locking*, if two requests both attempt to modify the contents of session, the last one overrides the first.</span></span> <span data-ttu-id="5c98a-233">`Session`wird als implementiert eine *kohärente Sitzung*, was bedeutet, dass der gesamte Inhalt zusammen gespeichert werden.</span><span class="sxs-lookup"><span data-stu-id="5c98a-233">`Session` is implemented as a *coherent session*, which means that all the contents are stored together.</span></span> <span data-ttu-id="5c98a-234">Zwei Anforderungen, die verschiedene Teile der Sitzung (verschiedene Schlüssel) zu ändern, werden möglicherweise weiterhin auswirken.</span><span class="sxs-lookup"><span data-stu-id="5c98a-234">Two requests that are modifying different parts of the session (different keys) might still impact each other.</span></span>

### <a name="setting-and-getting-session-values"></a><span data-ttu-id="5c98a-235">Festlegen und Abrufen von Sitzungsdaten</span><span class="sxs-lookup"><span data-stu-id="5c98a-235">Setting and getting Session values</span></span>

<span data-ttu-id="5c98a-236">Sitzung erfolgt über die `Session` Eigenschaft `HttpContext`.</span><span class="sxs-lookup"><span data-stu-id="5c98a-236">Session is accessed through the `Session` property on `HttpContext`.</span></span> <span data-ttu-id="5c98a-237">Diese Eigenschaft ist ein [ISession](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.http.isession) Implementierung.</span><span class="sxs-lookup"><span data-stu-id="5c98a-237">This property is an [ISession](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.http.isession) implementation.</span></span>

<span data-ttu-id="5c98a-238">Im folgende Beispiel wird gezeigt, einstellungs- und ein "Int" und eine Zeichenfolge:</span><span class="sxs-lookup"><span data-stu-id="5c98a-238">The following example shows setting and getting an int and a string:</span></span>

[!code-csharp[Main](app-state/sample/src/WebAppSession/Controllers/HomeController.cs?range=8-27,49)]

<span data-ttu-id="5c98a-239">Wenn Sie die folgenden Erweiterungsmethoden hinzufügen, können Sie festlegen und Abrufen von serialisierbare Objekte Sitzung:</span><span class="sxs-lookup"><span data-stu-id="5c98a-239">If you add the following extension methods, you can set and get serializable objects to Session:</span></span>

[!code-csharp[Main](app-state/sample/src/WebAppSession/Extensions/SessionExtensions.cs)]

<span data-ttu-id="5c98a-240">Im folgende Beispiel wird gezeigt, wie festgelegt und ein serialisierbares Objekt abgerufen wird:</span><span class="sxs-lookup"><span data-stu-id="5c98a-240">The following sample shows how to set and get a serializable object:</span></span>

[!code-csharp[Main](app-state/sample/src/WebAppSession/Controllers/HomeController.cs?name=snippet2)]


## <a name="working-with-httpcontextitems"></a><span data-ttu-id="5c98a-241">Arbeiten mit HttpContext.Items</span><span class="sxs-lookup"><span data-stu-id="5c98a-241">Working with HttpContext.Items</span></span>

<span data-ttu-id="5c98a-242">Die `HttpContext` Abstraktion bietet Unterstützung für ein Dictionary-Auflistung des Typs `IDictionary<object, object>`namens `Items`.</span><span class="sxs-lookup"><span data-stu-id="5c98a-242">The `HttpContext` abstraction provides support for a dictionary collection of type `IDictionary<object, object>`, called `Items`.</span></span> <span data-ttu-id="5c98a-243">Diese Sammlung ist verfügbar, ab dem Anfang einer *HttpRequest* und am Ende jeder Anforderung verworfen wird.</span><span class="sxs-lookup"><span data-stu-id="5c98a-243">This collection is available from the start of an *HttpRequest* and is discarded at the end of each request.</span></span> <span data-ttu-id="5c98a-244">Sie können darauf zugreifen, durch Zuweisen eines Werts zu einem schlüsselgebundenen Eintrag oder durch den Wert für einen bestimmten Schlüssel anfordern.</span><span class="sxs-lookup"><span data-stu-id="5c98a-244">You can access it by  assigning a value to a keyed entry, or by requesting the value for a particular key.</span></span>

<span data-ttu-id="5c98a-245">Im folgenden Beispiel wird [Middleware](middleware.md) fügt `isVerified` auf die `Items` Auflistung.</span><span class="sxs-lookup"><span data-stu-id="5c98a-245">In the sample below, [Middleware](middleware.md) adds `isVerified` to the `Items` collection.</span></span>

```csharp
app.Use(async (context, next) =>
{
    // perform some verification
    context.Items["isVerified"] = true;
    await next.Invoke();
});
```

<span data-ttu-id="5c98a-246">Weiter unten in der Pipeline kann eine andere Middleware darauf zugreifen:</span><span class="sxs-lookup"><span data-stu-id="5c98a-246">Later in the pipeline, another middleware could access it:</span></span>

```csharp
app.Run(async (context) =>
{
    await context.Response.WriteAsync("Verified request? " + 
        context.Items["isVerified"]);
});
```

<span data-ttu-id="5c98a-247">Für die Middleware, die nur von einer einzigen Anwendung verwendet werden, `string` Schlüssel sind zulässig.</span><span class="sxs-lookup"><span data-stu-id="5c98a-247">For middleware that will only be used by a single app, `string` keys are acceptable.</span></span> <span data-ttu-id="5c98a-248">Allerdings sollten Middleware, die zwischen Anwendungen gemeinsam genutzt werden, eindeutige Objektschlüssel verwenden, um alle Konflikte zu vermeiden.</span><span class="sxs-lookup"><span data-stu-id="5c98a-248">However, middleware that will be shared between applications should use unique object keys to avoid any chance of key collisions.</span></span> <span data-ttu-id="5c98a-249">Wenn Sie Middleware entwickeln, die für mehrere Anwendungen arbeiten müssen, verwenden Sie einen eindeutiges Objektschlüssel in die Middleware-Klasse, die wie folgt definiert:</span><span class="sxs-lookup"><span data-stu-id="5c98a-249">If you are developing middleware that must work across multiple applications, use a unique object key defined in your middleware class as shown below:</span></span>

```csharp
public class SampleMiddleware
{
    public static readonly object SampleKey = new Object();

    public async Task Invoke(HttpContext httpContext)
    {
        httpContext.Items[SampleKey] = "some value";
        // additional code omitted
    }
}
```

<span data-ttu-id="5c98a-250">Andere Codezugriff können in gespeicherten `HttpContext.Items` mithilfe des Schlüssels, der von der Middleware-Klasse verfügbar gemacht werden:</span><span class="sxs-lookup"><span data-stu-id="5c98a-250">Other code can access the value stored in `HttpContext.Items` using the key exposed by the middleware class:</span></span>

```csharp
public class HomeController : Controller
{
    public IActionResult Index()
    {
        string value = HttpContext.Items[SampleMiddleware.SampleKey];
    }
}
```

<span data-ttu-id="5c98a-251">Dieser Ansatz hat außerdem den Vorteil des Entfernens der Wiederholung von "Magic-Zeichenfolgen" an mehreren Stellen im Code.</span><span class="sxs-lookup"><span data-stu-id="5c98a-251">This approach also has the advantage of eliminating repetition of "magic strings" in multiple places in the code.</span></span>

<a name="appstate-errors"></a>

## <a name="application-state-data"></a><span data-ttu-id="5c98a-252">Anwendungszustandsdaten</span><span class="sxs-lookup"><span data-stu-id="5c98a-252">Application state data</span></span>

<span data-ttu-id="5c98a-253">Verwendung [Abhängigkeitsinjektion](xref:fundamentals/dependency-injection) um Daten für alle Benutzer verfügbar zu machen:</span><span class="sxs-lookup"><span data-stu-id="5c98a-253">Use [Dependency Injection](xref:fundamentals/dependency-injection) to make data available to all users:</span></span>

1. <span data-ttu-id="5c98a-254">Definieren Sie einen Dienst, der Daten enthält (z. B. eine Klasse, die mit dem Namen `MyAppData`).</span><span class="sxs-lookup"><span data-stu-id="5c98a-254">Define a service containing the data (for example, a class named `MyAppData`).</span></span>

```csharp
public class MyAppData
{
    // Declare properties/methods/etc.
} 
```
2. <span data-ttu-id="5c98a-255">Hinzufügen der Dienstklasse zum `ConfigureServices` (z. B. `services.AddSingleton<MyAppData>();`).</span><span class="sxs-lookup"><span data-stu-id="5c98a-255">Add the service class to `ConfigureServices` (for example `services.AddSingleton<MyAppData>();`).</span></span>
3. <span data-ttu-id="5c98a-256">Verwenden Sie die Datendienstklasse in jedem Controller:</span><span class="sxs-lookup"><span data-stu-id="5c98a-256">Consume the data service class in each controller:</span></span>

```csharp
public class MyController : Controller
{
    public MyController(MyAppData myService)
    {
        // Do something with the service (read some data from it, 
        // store it in a private field/property, etc.)
    }
} 
```

## <a name="common-errors-when-working-with-session"></a><span data-ttu-id="5c98a-257">Häufige Fehler bei der Arbeit mit der Sitzung</span><span class="sxs-lookup"><span data-stu-id="5c98a-257">Common errors when working with session</span></span>

* <span data-ttu-id="5c98a-258">"Dienst für Typ"Microsoft.Extensions.Caching.Distributed.IDistributedCache"aufgelöst, bei dem Versuch,"Microsoft.AspNetCore.Session.DistributedSessionStore"aktivieren kann."</span><span class="sxs-lookup"><span data-stu-id="5c98a-258">"Unable to resolve service for type 'Microsoft.Extensions.Caching.Distributed.IDistributedCache' while attempting to activate 'Microsoft.AspNetCore.Session.DistributedSessionStore'."</span></span>

  <span data-ttu-id="5c98a-259">Dies wird normalerweise verursacht durch wegen eines Fehlers beim Konfigurieren von mindestens einer `IDistributedCache` Implementierung.</span><span class="sxs-lookup"><span data-stu-id="5c98a-259">This is usually caused by failing to configure at least one `IDistributedCache` implementation.</span></span> <span data-ttu-id="5c98a-260">Weitere Informationen finden Sie unter [arbeiten mit einem verteilten Cache](xref:performance/caching/distributed) und [im Arbeitsspeicher zwischenspeichern](xref:performance/caching/memory).</span><span class="sxs-lookup"><span data-stu-id="5c98a-260">For more information, see [Working with a Distributed Cache](xref:performance/caching/distributed) and [In memory caching](xref:performance/caching/memory).</span></span>

* <span data-ttu-id="5c98a-261">Im Ereignisprotokoll die der Sitzung Middleware ein Fehler, um auftritt eine Sitzung beibehalten (zum Beispiel: Wenn die Datenbank nicht verfügbar ist), es auffängt und protokolliert die Ausnahme.</span><span class="sxs-lookup"><span data-stu-id="5c98a-261">In the event that the session middleware fails to persist a session (for example: if the database is not available), it logs the exception and swallows it.</span></span> <span data-ttu-id="5c98a-262">Die Anforderung wird dann in der Regel wird weiterhin Dies führt zu sehr zu unvorhersehbarem Verhalten.</span><span class="sxs-lookup"><span data-stu-id="5c98a-262">The request will then continue normally, which leads to very unpredictable behavior.</span></span>

<span data-ttu-id="5c98a-263">Ein typisches Beispiel:</span><span class="sxs-lookup"><span data-stu-id="5c98a-263">A typical example:</span></span>

<span data-ttu-id="5c98a-264">Eine Person speichert einen Warenkorb gelegt, in der Sitzung.</span><span class="sxs-lookup"><span data-stu-id="5c98a-264">Someone stores a shopping basket in session.</span></span> <span data-ttu-id="5c98a-265">Der Benutzer fügt ein Element aus, aber das Commit schlägt fehl.</span><span class="sxs-lookup"><span data-stu-id="5c98a-265">The user adds an item but the commit fails.</span></span> <span data-ttu-id="5c98a-266">Die app weiß nicht über den Fehler, damit es die Meldung gibt Aufschluss über "das Element wurde" wird die nicht "true", hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="5c98a-266">The app doesn't know about the failure so it reports the message "The item has been added", which isn't true.</span></span>

<span data-ttu-id="5c98a-267">Die empfohlene Methode zum Suchen nach solchen Fehlern besteht im Aufrufen `await feature.Session.CommitAsync();` von app-Code, wenn Sie fertig sind in der Sitzung geschrieben.</span><span class="sxs-lookup"><span data-stu-id="5c98a-267">The recommended way to check for such errors is to call `await feature.Session.CommitAsync();` from app code when you're done writing to the session.</span></span> <span data-ttu-id="5c98a-268">Anschließend können Sie tun, wie Sie mit dem Fehler.</span><span class="sxs-lookup"><span data-stu-id="5c98a-268">Then you can do what you like with the error.</span></span> <span data-ttu-id="5c98a-269">Es funktioniert die gleiche Weise wie beim Aufrufen von `LoadAsync`.</span><span class="sxs-lookup"><span data-stu-id="5c98a-269">It works the same way when calling `LoadAsync`.</span></span>


### <a name="additional-resources"></a><span data-ttu-id="5c98a-270">Zusätzliche Ressourcen</span><span class="sxs-lookup"><span data-stu-id="5c98a-270">Additional Resources</span></span>


* [<span data-ttu-id="5c98a-271">ASP.NET Core 1.x: Beispielcode in diesem Dokument verwendeten</span><span class="sxs-lookup"><span data-stu-id="5c98a-271">ASP.NET Core 1.x: Sample code used in this document</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/app-state/sample/src/WebAppSession)
* [<span data-ttu-id="5c98a-272">ASP.NET Core 2.x: Beispielcode in diesem Dokument verwendeten</span><span class="sxs-lookup"><span data-stu-id="5c98a-272">ASP.NET Core 2.x: Sample code used in this document</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/app-state/sample/src/WebAppSessionDotNetCore2.0App)
