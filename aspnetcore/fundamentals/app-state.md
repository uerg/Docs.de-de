---
title: Sitzung und Anwendungsstatus in ASP.NET Core
author: rick-anderson
description: "Ansätze zum Beibehalten der Anwendung und des Benutzerstatus (Sitzung) zwischen Anforderungen."
keywords: ASP.NET Core, Anwendungszustand, Sitzungszustand, Abfragezeichenfolgen, bereitstellen
ms.author: riande
manager: wpickett
ms.date: 06/08/2017
ms.topic: article
ms.assetid: 18cda488-0769-4cb9-82f6-4c6685f2045d
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/app-state
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: c639d3b0d896b927bb2b70658032fc1bd8e87191
ms.sourcegitcommit: 78d28178345a0eea91556e4cd1adad98b1446db8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/22/2017
---
# <a name="introduction-to-session-and-application-state-in-aspnet-core"></a><span data-ttu-id="04f0c-104">Einführung in die Sitzung und Anwendungsstatus in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="04f0c-104">Introduction to session and application state in ASP.NET Core</span></span>

<span data-ttu-id="04f0c-105">Durch [Rick Anderson](https://twitter.com/RickAndMSFT), [Steve Smith](https://ardalis.com/), und [Diana LaRose](https://github.com/DianaLaRose)</span><span class="sxs-lookup"><span data-stu-id="04f0c-105">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Steve Smith](https://ardalis.com/), and [Diana LaRose](https://github.com/DianaLaRose)</span></span>

<span data-ttu-id="04f0c-106">HTTP ist ein statusfreies Protokoll.</span><span class="sxs-lookup"><span data-stu-id="04f0c-106">HTTP is a stateless protocol.</span></span> <span data-ttu-id="04f0c-107">Ein Webserver jede HTTP-Anforderung als unabhängige Anforderung behandelt und behält die Benutzerwerte aus den vorherigen Anforderungen nicht.</span><span class="sxs-lookup"><span data-stu-id="04f0c-107">A  web server treats each HTTP request as an independent request and does not retain user values from previous requests.</span></span> <span data-ttu-id="04f0c-108">Dieser Artikel beschreibt verschiedene Möglichkeiten, Anwendung und der Status der Sitzung zwischen den Anforderungen beizubehalten.</span><span class="sxs-lookup"><span data-stu-id="04f0c-108">This article discusses different ways to preserve application and session state between requests.</span></span> 

## <a name="session-state"></a><span data-ttu-id="04f0c-109">Sitzungsstatus</span><span class="sxs-lookup"><span data-stu-id="04f0c-109">Session state</span></span>

<span data-ttu-id="04f0c-110">Status der Sitzung ist ein Feature in ASP.NET Core, die Sie verwenden können, um zu speichern, und Speichern von Benutzerdaten, während der Benutzer auf Ihre Web-app durchsucht.</span><span class="sxs-lookup"><span data-stu-id="04f0c-110">Session state is a feature in ASP.NET Core that you can use to save and store user data while the user browses your web app.</span></span> <span data-ttu-id="04f0c-111">Bestehend aus einem Wörterbuch oder Hash-Tabelle auf dem Server, bleiben Sitzungsstatus Daten anforderungsübergreifend in einem Browser.</span><span class="sxs-lookup"><span data-stu-id="04f0c-111">Consisting of a dictionary or hash table on the server, session state persists data across requests from a browser.</span></span> <span data-ttu-id="04f0c-112">Die Sitzungsdaten durch einen Cache gesichert.</span><span class="sxs-lookup"><span data-stu-id="04f0c-112">The session data is backed by a cache.</span></span>

<span data-ttu-id="04f0c-113">ASP.NET Core beibehalten Sitzungszustand, durch die Vergabe des Clients ein Cookie, das die Sitzungs-ID enthält, die mit jeder Anforderung an den Server gesendet wird.</span><span class="sxs-lookup"><span data-stu-id="04f0c-113">ASP.NET Core maintains session state by giving the client a cookie that contains the session ID, which is sent to the server with each request.</span></span> <span data-ttu-id="04f0c-114">Der Server verwendet die Sitzungs-ID, um die Sitzungsdaten abzurufen.</span><span class="sxs-lookup"><span data-stu-id="04f0c-114">The server uses the session ID to fetch the session data.</span></span> <span data-ttu-id="04f0c-115">Da das Sitzungscookie spezifisch für den Browser ist, kann nicht Browsern Sitzungen gemeinsam nutzen.</span><span class="sxs-lookup"><span data-stu-id="04f0c-115">Because the session cookie is specific to the browser, you cannot share sessions across browsers.</span></span> <span data-ttu-id="04f0c-116">Sitzungscookies sind nur gelöscht, wenn die Browsersitzung beendet.</span><span class="sxs-lookup"><span data-stu-id="04f0c-116">Session cookies are deleted only when the browser session ends.</span></span> <span data-ttu-id="04f0c-117">Wenn ein Cookie für eine Sitzung für abgelaufene empfangen wird, wird eine neue Sitzung, die das gleiche Sitzungscookie verwendet erstellt.</span><span class="sxs-lookup"><span data-stu-id="04f0c-117">If a cookie is received for an expired session, a new session that uses the same session cookie  is created.</span></span> 

<span data-ttu-id="04f0c-118">Der Server behält eine Sitzung für einen begrenzten Zeitraum nach der letzten Anforderung.</span><span class="sxs-lookup"><span data-stu-id="04f0c-118">The server retains a session for a limited time after the last request.</span></span> <span data-ttu-id="04f0c-119">Sie können das Sitzungstimeout festgelegt oder den Standardwert von 20 Minuten.</span><span class="sxs-lookup"><span data-stu-id="04f0c-119">You can either set the session timeout or use the default value of 20 minutes.</span></span> <span data-ttu-id="04f0c-120">Status der Sitzung ist ideal zum Speichern von Benutzerdaten, die bezieht sich auf einer bestimmten Sitzung jedoch nicht dauerhaft beibehalten werden müssen.</span><span class="sxs-lookup"><span data-stu-id="04f0c-120">Session state is ideal for storing user data that is specific to a particular session but doesn’t need to be persisted permanently.</span></span> <span data-ttu-id="04f0c-121">Daten werden gelöscht aus dem Sicherungsspeicher entweder beim Aufrufen von `Session.Clear` oder im Datenspeicher des Ablaufs der Sitzung.</span><span class="sxs-lookup"><span data-stu-id="04f0c-121">Data is deleted from the backing store either when you call `Session.Clear` or when the session expires in the data store.</span></span> <span data-ttu-id="04f0c-122">Der Server ist nicht bekannt, wenn der Browser geschlossen wird oder wenn das Sitzungscookie gelöscht wird.</span><span class="sxs-lookup"><span data-stu-id="04f0c-122">The server does not know when the browser is closed or when the session cookie is deleted.</span></span>

> [!WARNING]
> <span data-ttu-id="04f0c-123">Speichern Sie vertrauliche Daten nicht in der Sitzung.</span><span class="sxs-lookup"><span data-stu-id="04f0c-123">Do not store sensitive data in session.</span></span> <span data-ttu-id="04f0c-124">Der Client möglicherweise nicht den Browser schließen und löschen Sie das Sitzungscookie (und von einigen Browsern erhalten Sitzungscookies über Windows).</span><span class="sxs-lookup"><span data-stu-id="04f0c-124">The client might not close the browser and clear the session cookie (and some browsers keep session cookies alive across windows).</span></span> <span data-ttu-id="04f0c-125">Darüber hinaus kann eine Sitzung nicht auf einen einzelnen Benutzer beschränkt sein; der nächste Benutzer möglicherweise mit der gleichen Sitzung fortgesetzt.</span><span class="sxs-lookup"><span data-stu-id="04f0c-125">Also, a session might not be restricted to a single user; the next user might continue with the same session.</span></span>

<span data-ttu-id="04f0c-126">Die in-Memory-sitzungsanbieters speichert Sitzungsdaten auf dem lokalen Server.</span><span class="sxs-lookup"><span data-stu-id="04f0c-126">The in-memory session provider stores session data on the local server.</span></span> <span data-ttu-id="04f0c-127">Wenn Sie beabsichtigen, Ihre Web-app auf einer Serverfarm auszuführen, müssen Sie persistente Sitzungen verwenden, um jede Sitzung an einen bestimmten Server zu verbinden.</span><span class="sxs-lookup"><span data-stu-id="04f0c-127">If you plan to run your web app on a server farm, you must use sticky sessions to tie each session to a specific server.</span></span> <span data-ttu-id="04f0c-128">Die Websites der Windows Azure-Plattform wird standardmäßig auf persistente Sitzungen (Application Request Routing oder ARR).</span><span class="sxs-lookup"><span data-stu-id="04f0c-128">The Windows Azure Web Sites platform defaults to sticky sessions (Application Request Routing or ARR).</span></span> <span data-ttu-id="04f0c-129">Allerdings können persistente Sitzungen Auswirkungen auf die Skalierbarkeit und Web-app-Updates erschweren.</span><span class="sxs-lookup"><span data-stu-id="04f0c-129">However, sticky sessions can affect scalability and complicate web app updates.</span></span> <span data-ttu-id="04f0c-130">Eine bessere Option ist die Verwendung des Redis oder verteilten SQL Server zwischengespeichert werden, die nicht persistente Sitzungen erfordern.</span><span class="sxs-lookup"><span data-stu-id="04f0c-130">A better option is to use the Redis or SQL Server distributed caches, which don't require sticky sessions.</span></span> <span data-ttu-id="04f0c-131">Weitere Informationen finden Sie unter [arbeiten mit einem verteilten Cache](xref:performance/caching/distributed).</span><span class="sxs-lookup"><span data-stu-id="04f0c-131">For more information, see [Working with a Distributed Cache](xref:performance/caching/distributed).</span></span> <span data-ttu-id="04f0c-132">Weitere Informationen zum Einrichten der Dienstanbieter, finden Sie unter [konfigurieren Sitzung](#configuring-session) weiter unten in diesem Artikel.</span><span class="sxs-lookup"><span data-stu-id="04f0c-132">For details on setting up service providers, see [Configuring Session](#configuring-session) later in this article.</span></span>

<span data-ttu-id="04f0c-133">Der übrige Teil dieses Abschnitts wird beschrieben, die Optionen zum Speichern von Benutzerdaten.</span><span class="sxs-lookup"><span data-stu-id="04f0c-133">The remainder of this section describes the options for storing user data.</span></span>

<a name="temp"></a>
### <a name="tempdata"></a><span data-ttu-id="04f0c-134">TempData</span><span class="sxs-lookup"><span data-stu-id="04f0c-134">TempData</span></span>

<span data-ttu-id="04f0c-135">ASP.NET Core MVC macht die [TempData](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.controller#Microsoft_AspNetCore_Mvc_Controller_TempData) Eigenschaft auf einen [Controller](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.controller).</span><span class="sxs-lookup"><span data-stu-id="04f0c-135">ASP.NET Core MVC exposes the [TempData](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.controller#Microsoft_AspNetCore_Mvc_Controller_TempData) property on a [controller](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.controller).</span></span> <span data-ttu-id="04f0c-136">Diese Eigenschaft speichert Daten, bis sie gelesen wurden.</span><span class="sxs-lookup"><span data-stu-id="04f0c-136">This property stores data until it is read.</span></span> <span data-ttu-id="04f0c-137">Die Methoden `Keep` und `Peek` können verwendet werden, um die Daten zu überprüfen, ohne sie zu löschen.</span><span class="sxs-lookup"><span data-stu-id="04f0c-137">The `Keep` and `Peek` methods can be used to examine the data without deletion.</span></span> <span data-ttu-id="04f0c-138">`TempData`ist besonders nützlich für die Umleitung, wenn Daten für mehr als eine einzelne Anforderung benötigt werden.</span><span class="sxs-lookup"><span data-stu-id="04f0c-138">`TempData` is particularly useful for redirection, when data is needed for more than a single request.</span></span> <span data-ttu-id="04f0c-139">`TempData`Sitzungsstatus baut auf.</span><span class="sxs-lookup"><span data-stu-id="04f0c-139">`TempData` is built on top of session state.</span></span> 

## <a name="cookie-based-tempdata-provider"></a><span data-ttu-id="04f0c-140">Cookie-basierte TempData-Anbieter</span><span class="sxs-lookup"><span data-stu-id="04f0c-140">Cookie-based TempData provider</span></span> 

<span data-ttu-id="04f0c-141">In ASP.NET Core 1.1 und höher wird können den Cookie-basierte TempData-Anbieter Sie TempData eines Benutzers in einem Cookie gespeichert.</span><span class="sxs-lookup"><span data-stu-id="04f0c-141">In ASP.NET Core 1.1 and higher, you can use the cookie-based TempData provider to store a user's TempData in a cookie.</span></span> <span data-ttu-id="04f0c-142">Zum Aktivieren des Cookie-basierte TempData-Anbieters Registrieren der `CookieTempDataProvider` -Dienst in `ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="04f0c-142">To enable the  cookie-based TempData provider, register the `CookieTempDataProvider` service in `ConfigureServices`:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddMvc();
    // Add CookieTempDataProvider after AddMvc and include ViewFeatures.
    // using Microsoft.AspNetCore.Mvc.ViewFeatures;
    services.AddSingleton<ITempDataProvider, CookieTempDataProvider>();
}
```

<span data-ttu-id="04f0c-143">Ist die Cookiedaten codiert die [Base64UrlTextEncoder](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.authentication.base64urltextencoder).</span><span class="sxs-lookup"><span data-stu-id="04f0c-143">The cookie data is encoded with the [Base64UrlTextEncoder](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.authentication.base64urltextencoder).</span></span> <span data-ttu-id="04f0c-144">Da das Cookie verschlüsselt und aus Ihren Segmenten zusammengesetzt wird, gilt das Größenlimit für die einzelnen Cookies nicht zu.</span><span class="sxs-lookup"><span data-stu-id="04f0c-144">Because the cookie is encrypted and chunked, the single cookie size limit does not apply.</span></span> <span data-ttu-id="04f0c-145">Der Cookiedaten sind nicht komprimiert, da beim Komprimieren von Daten Encryped Sicherheitsprobleme wie führen, kann die [CRIME](https://wikipedia.org/wiki/CRIME_(security_exploit)) und [Verletzung](https://wikipedia.org/wiki/BREACH_(security_exploit)) Angriffe.</span><span class="sxs-lookup"><span data-stu-id="04f0c-145">The cookie data is not compressed, because compressing encryped data can lead to security problems such as the [CRIME](https://wikipedia.org/wiki/CRIME_(security_exploit)) and [BREACH](https://wikipedia.org/wiki/BREACH_(security_exploit)) attacks.</span></span> <span data-ttu-id="04f0c-146">Weitere Informationen für den Cookie-basierte TempData-Anbieter finden Sie unter [CookieTempDataProvider](https://github.com/aspnet/Mvc/blob/dev/src/Microsoft.AspNetCore.Mvc.ViewFeatures/ViewFeatures/CookieTempDataProvider.cs).</span><span class="sxs-lookup"><span data-stu-id="04f0c-146">For more information on the cookie-based TempData provider, see [CookieTempDataProvider](https://github.com/aspnet/Mvc/blob/dev/src/Microsoft.AspNetCore.Mvc.ViewFeatures/ViewFeatures/CookieTempDataProvider.cs).</span></span>

### <a name="query-strings"></a><span data-ttu-id="04f0c-147">Abfragezeichenfolgen</span><span class="sxs-lookup"><span data-stu-id="04f0c-147">Query strings</span></span>

<span data-ttu-id="04f0c-148">Sie können eine begrenzte Menge von Daten aus einer Anforderung in eine andere übergeben, indem Abfragezeichenfolge für die neue Anforderung hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="04f0c-148">You can pass a limited amount of data from one request to another by adding it to the new request’s query string.</span></span> <span data-ttu-id="04f0c-149">Dies ist nützlich zum Aufzeichnen des Status in einer persistenten Weise, die Links mit eingebetteten Status über e-Mail oder soziale Netzwerke freigegeben werden kann.</span><span class="sxs-lookup"><span data-stu-id="04f0c-149">This is useful for capturing state in a persistent manner that allows links with embedded state to be shared through email or social networks.</span></span> <span data-ttu-id="04f0c-150">Allerdings sollten aus diesem Grund Sie nie Abfragezeichenfolgen für vertrauliche Daten verwenden.</span><span class="sxs-lookup"><span data-stu-id="04f0c-150">However, for this reason,  you should never use query strings for sensitive data.</span></span> <span data-ttu-id="04f0c-151">Zusätzlich zu einfach freigegeben, einschließlich der Daten in Abfragezeichenfolgen kann erstellen Verkaufschancen für [Cross-Site Request Fälschung Websiteübergreifender](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF)) Angriffe, bei denen Benutzer für den Zugriff auf bösartige Websites während authentifiziert bringen können.</span><span class="sxs-lookup"><span data-stu-id="04f0c-151">In addition to being easily shared, including data in query strings can create opportunities for [Cross-Site Request Forgery (CSRF)](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF)) attacks, which can trick users into visiting malicious sites while authenticated.</span></span> <span data-ttu-id="04f0c-152">Angreifer können dann Benutzerdaten aus Ihrer app zu stehlen oder böswilligen Aktionen im Namen des Benutzers.</span><span class="sxs-lookup"><span data-stu-id="04f0c-152">Attackers can then steal user data from your app or take malicious actions on behalf of the user.</span></span> <span data-ttu-id="04f0c-153">Alle beibehaltenen Zustand der Anwendung oder die Sitzung muss vor CSRF-Angriffen schützen.</span><span class="sxs-lookup"><span data-stu-id="04f0c-153">Any preserved application or session state must protect against CSRF attacks.</span></span> <span data-ttu-id="04f0c-154">Weitere Informationen zu CSRF-Angriffen, finden Sie unter [verhindern Cross-Site Request XSRF/Websiteübergreifender Anforderungsfälschung Angriffe in ASP.NET Core](../security/anti-request-forgery.md).</span><span class="sxs-lookup"><span data-stu-id="04f0c-154">For more information on CSRF attacks, see [Preventing Cross-Site Request Forgery (XSRF/CSRF) Attacks in ASP.NET Core](../security/anti-request-forgery.md).</span></span>

### <a name="post-data-and-hidden-fields"></a><span data-ttu-id="04f0c-155">Bereitgestellte Daten und ausgeblendete Felder</span><span class="sxs-lookup"><span data-stu-id="04f0c-155">Post data and hidden fields</span></span>

<span data-ttu-id="04f0c-156">Daten können in ausgeblendeten Formularfelder gespeichert und wieder auf die nächste Anforderung gesendet werden.</span><span class="sxs-lookup"><span data-stu-id="04f0c-156">Data can be saved in hidden form fields and posted back on the next request.</span></span> <span data-ttu-id="04f0c-157">Dies ist häufig in mehrseitigen Formularen.</span><span class="sxs-lookup"><span data-stu-id="04f0c-157">This is common in multipage forms.</span></span> <span data-ttu-id="04f0c-158">Jedoch, da der Client möglicherweise die Daten manipulieren kann, muss der Server immer es erneut überprüfen.</span><span class="sxs-lookup"><span data-stu-id="04f0c-158">However, because the  client can potentially tamper with the data, the server must always revalidate it.</span></span> 

### <a name="cookies"></a><span data-ttu-id="04f0c-159">Cookies</span><span class="sxs-lookup"><span data-stu-id="04f0c-159">Cookies</span></span>

<span data-ttu-id="04f0c-160">Cookies bieten eine Möglichkeit zum Speichern von benutzerspezifischen Daten in Webanwendungen.</span><span class="sxs-lookup"><span data-stu-id="04f0c-160">Cookies provide a way to store user-specific data in web applications.</span></span> <span data-ttu-id="04f0c-161">Da Cookies mit jeder Anforderung gesendet werden, sollte ihre Größe auf ein Minimum begrenzt.</span><span class="sxs-lookup"><span data-stu-id="04f0c-161">Because cookies are sent with every request, their size should be kept to a minimum.</span></span> <span data-ttu-id="04f0c-162">Im Idealfall sollten lediglich ein Bezeichner mit den tatsächlichen Daten, die auf dem Server gespeicherten in einem Cookie gespeichert werden.</span><span class="sxs-lookup"><span data-stu-id="04f0c-162">Ideally, only an identifier should be stored in a cookie with the actual data stored on the server.</span></span> <span data-ttu-id="04f0c-163">Die meisten Browser beschränken Cookies auf 4096 Bytes.</span><span class="sxs-lookup"><span data-stu-id="04f0c-163">Most browsers restrict cookies to 4096 bytes.</span></span> <span data-ttu-id="04f0c-164">Darüber hinaus sind nur eine begrenzte Anzahl von Cookies für jede Domäne verfügbar.</span><span class="sxs-lookup"><span data-stu-id="04f0c-164">In addition, only a limited number of cookies are available for each domain.</span></span>  

<span data-ttu-id="04f0c-165">Da Cookies sind leichter manipuliert werden, müssen sie auf dem Server überprüft werden.</span><span class="sxs-lookup"><span data-stu-id="04f0c-165">Because cookies are subject to tampering, they must be validated on the server.</span></span> <span data-ttu-id="04f0c-166">Die Dauerhaftigkeit des Cookies auf einem Client unterliegt zwar Eingreifen des Benutzers und das Ablaufdatum, sind sie in der Regel die meisten permanenten Form der Dauerhaftigkeit der Daten auf dem Client.</span><span class="sxs-lookup"><span data-stu-id="04f0c-166">Although the durability of the cookie on a client is subject to user intervention and expiration, they are generally the most durable form of data persistence on the client.</span></span>

<span data-ttu-id="04f0c-167">Cookies werden für die Personalisierung, häufig verwendet, in dem Inhalte für einen bekannten Benutzer angepasst.</span><span class="sxs-lookup"><span data-stu-id="04f0c-167">Cookies are often used for personalization, where content is customized for a known user.</span></span> <span data-ttu-id="04f0c-168">Da der Benutzer nur ermittelt und in den meisten Fällen nicht authentifiziert, können Sie einen Cookie in der Regel sichern, indem Sie den Benutzernamen, Kontoname oder eine eindeutige Benutzer-ID (z. B. eine GUID) im Cookie speichern.</span><span class="sxs-lookup"><span data-stu-id="04f0c-168">Because the user is only identified and not authenticated in most cases, you can typically secure a cookie by storing the user name, account name, or a unique user ID (such as a GUID) in the cookie.</span></span> <span data-ttu-id="04f0c-169">Das Cookie können dann die Benutzer Personalisierungsinfrastruktur einer Website zugreifen.</span><span class="sxs-lookup"><span data-stu-id="04f0c-169">You can then use the cookie to access the user personalization infrastructure of a site.</span></span>

### <a name="httpcontextitems"></a><span data-ttu-id="04f0c-170">HttpContext.Items</span><span class="sxs-lookup"><span data-stu-id="04f0c-170">HttpContext.Items</span></span>

<span data-ttu-id="04f0c-171">Die `Items` Auflistung ist ein guter Speicherort zum Speichern von Daten, die erforderlich sind nur while-Verarbeitung einer bestimmten Anforderung.</span><span class="sxs-lookup"><span data-stu-id="04f0c-171">The `Items` collection is a good location to store data that is needed only while processing one particular request.</span></span> <span data-ttu-id="04f0c-172">Der Inhalt der Auflistung werden nach jeder Anforderung verworfen.</span><span class="sxs-lookup"><span data-stu-id="04f0c-172">The collection's contents are discarded after each request.</span></span> <span data-ttu-id="04f0c-173">Die `Items` Auflistung wird am besten als eine Möglichkeit, Komponenten oder Middleware kommunizieren kann, wenn sie zu unterschiedlichen Zeitpunkten ausgeführt, in der Zeit, während eine Anforderung werden und keine direkte Möglichkeit zum Übergeben von Parametern haben verwendet.</span><span class="sxs-lookup"><span data-stu-id="04f0c-173">The `Items` collection is best used as a way for components or middleware to communicate when they operate at different points in time during a request and have no direct way to pass parameters.</span></span> <span data-ttu-id="04f0c-174">Weitere Informationen finden Sie unter [arbeiten mit HttpContext.Items](#working-with-httpcontextitems)weiter unten in diesem Artikel.</span><span class="sxs-lookup"><span data-stu-id="04f0c-174">For more information, see [Working with HttpContext.Items](#working-with-httpcontextitems), later in this article.</span></span>

### <a name="cache"></a><span data-ttu-id="04f0c-175">cache</span><span class="sxs-lookup"><span data-stu-id="04f0c-175">Cache</span></span>

<span data-ttu-id="04f0c-176">Caching ist eine effiziente Möglichkeit zum Speichern und Abrufen von Daten.</span><span class="sxs-lookup"><span data-stu-id="04f0c-176">Caching is an efficient way to store and retrieve data.</span></span> <span data-ttu-id="04f0c-177">Sie können die Lebensdauer für zwischengespeicherte Elemente basierend auf Uhrzeit und andere Faktoren steuern.</span><span class="sxs-lookup"><span data-stu-id="04f0c-177">You can control the lifetime of cached items based on time and other considerations.</span></span> <span data-ttu-id="04f0c-178">Erfahren Sie mehr über [Caching](../performance/caching/index.md).</span><span class="sxs-lookup"><span data-stu-id="04f0c-178">Learn more about [Caching](../performance/caching/index.md).</span></span>

<a name=session></a>

## <a name="configuring-session"></a><span data-ttu-id="04f0c-179">Konfigurieren der Sitzung</span><span class="sxs-lookup"><span data-stu-id="04f0c-179">Configuring Session</span></span>

<span data-ttu-id="04f0c-180">Die `Microsoft.AspNetCore.Session` Paket stellt Middleware für die Verwaltung des Sitzungsstatus bereit.</span><span class="sxs-lookup"><span data-stu-id="04f0c-180">The `Microsoft.AspNetCore.Session` package provides middleware for managing session state.</span></span> <span data-ttu-id="04f0c-181">So aktivieren Sie die Sitzung Middleware `Startup`muss enthalten:</span><span class="sxs-lookup"><span data-stu-id="04f0c-181">To enable the session middleware, `Startup`must contain:</span></span>

- <span data-ttu-id="04f0c-182">Keines der [IDistributedCache](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.caching.distributed.idistributedcache) arbeitsspeichercaches.</span><span class="sxs-lookup"><span data-stu-id="04f0c-182">Any of the [IDistributedCache](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.caching.distributed.idistributedcache) memory caches.</span></span> <span data-ttu-id="04f0c-183">Die `IDistributedCache` Implementierung ist für Sitzung als Sicherungsspeicher verwendet.</span><span class="sxs-lookup"><span data-stu-id="04f0c-183">The `IDistributedCache` implementation is used as a backing store for session.</span></span>
- <span data-ttu-id="04f0c-184">[AddSession](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.dependencyinjection.sessionservicecollectionextensions#Microsoft_Extensions_DependencyInjection_SessionServiceCollectionExtensions_AddSession_Microsoft_Extensions_DependencyInjection_IServiceCollection_) aufzurufen, erfordert die NuGet-Paket "Microsoft.AspNetCore.Session".</span><span class="sxs-lookup"><span data-stu-id="04f0c-184">[AddSession](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.dependencyinjection.sessionservicecollectionextensions#Microsoft_Extensions_DependencyInjection_SessionServiceCollectionExtensions_AddSession_Microsoft_Extensions_DependencyInjection_IServiceCollection_) call, which requires NuGet package "Microsoft.AspNetCore.Session".</span></span>
- <span data-ttu-id="04f0c-185">[UseSession](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.sessionmiddlewareextensions#methods_) aufrufen.</span><span class="sxs-lookup"><span data-stu-id="04f0c-185">[UseSession](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.sessionmiddlewareextensions#methods_) call.</span></span>

<span data-ttu-id="04f0c-186">Der folgende Code zeigt, wie der Sitzungsanbieter in-Memory-eingerichtet wird.</span><span class="sxs-lookup"><span data-stu-id="04f0c-186">The following code shows how to set up the in-memory session provider.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="04f0c-187">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="04f0c-187">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

[!code-csharp[Main](app-state/sample/src/WebAppSessionDotNetCore2.0App/Startup.cs?highlight=11-19,24)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="04f0c-188">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="04f0c-188">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

[!code-csharp[Main](app-state/sample/src/WebAppSession/Startup.cs?highlight=11-19,24)]

---

<span data-ttu-id="04f0c-189">Sie können die Sitzung von verweisen `HttpContext` nach installiert und konfiguriert ist.</span><span class="sxs-lookup"><span data-stu-id="04f0c-189">You can reference Session from `HttpContext` once it is installed and configured.</span></span>

<span data-ttu-id="04f0c-190">Wenn Sie versuchen, den Zugriff auf `Session` vor `UseSession` aufgerufen wurde, die Ausnahme `InvalidOperationException: Session has not been configured for this application or request` ausgelöst wird.</span><span class="sxs-lookup"><span data-stu-id="04f0c-190">If you try to access `Session` before `UseSession` has been called, the exception `InvalidOperationException: Session has not been configured for this application or request` is thrown.</span></span>

<span data-ttu-id="04f0c-191">Wenn Sie versuchen, ein neues erstellen `Session` (d. h. keine Sitzungscookie erstellt wurde), nachdem Sie das Schreiben in bereits begonnen haben die `Response` stream, der die Ausnahme `InvalidOperationException: The session cannot be established after the response has started` ausgelöst wird.</span><span class="sxs-lookup"><span data-stu-id="04f0c-191">If you try to create a new `Session` (that is, no session cookie has been created) after you have already begun writing to the `Response` stream, the exception `InvalidOperationException: The session cannot be established after the response has started` is thrown.</span></span> <span data-ttu-id="04f0c-192">Die Ausnahme finden Sie in der Web-Server-Protokoll; Es wird nicht im Browser angezeigt werden.</span><span class="sxs-lookup"><span data-stu-id="04f0c-192">The exception can be found in the web server log; it will not be displayed in the browser.</span></span>

### <a name="loading-session-asynchronously"></a><span data-ttu-id="04f0c-193">Laden die Sitzung asynchron</span><span class="sxs-lookup"><span data-stu-id="04f0c-193">Loading Session asynchronously</span></span> 

<span data-ttu-id="04f0c-194">Der Standardanbieter für die Sitzung in ASP.NET Core lädt die Sitzung Datensatz aus der zugrunde liegenden [IDistributedCache](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.caching.distributed.idistributedcache) Store asynchron nur, wenn die [ISession.LoadAsync](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.http.isession#Microsoft_AspNetCore_Http_ISession_LoadAsync) -Methode explizit aufgerufen wird, bevor  die `TryGetValue`, `Set`, oder `Remove` Methoden.</span><span class="sxs-lookup"><span data-stu-id="04f0c-194">The default session provider in ASP.NET Core loads the session record from the underlying [IDistributedCache](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.caching.distributed.idistributedcache) store asynchronously only if the [ISession.LoadAsync](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.http.isession#Microsoft_AspNetCore_Http_ISession_LoadAsync) method is explicitly called before  the `TryGetValue`, `Set`, or `Remove` methods.</span></span> <span data-ttu-id="04f0c-195">Wenn `LoadAsync` nicht zuerst aufgerufen wird, die zugrunde liegende Sitzung Datensatz synchron geladen wird, was kann potenziell die Möglichkeit zum Skalieren der app auswirken.</span><span class="sxs-lookup"><span data-stu-id="04f0c-195">If `LoadAsync` is not called first, the underlying session record is loaded synchronously, which could potentially impact the ability of the app to scale.</span></span>

<span data-ttu-id="04f0c-196">Damit Anwendungen, die dieses Muster zu erzwingen, umschließen die [DistributedSessionStore](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.session.distributedsessionstore) und [DistributedSession](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.session.distributedsession) Implementierungen mit Versionen, die eine Ausnahme auszulösen, wenn die `LoadAsync` Methode ist nicht wird aufgerufen, bevor `TryGetValue`, `Set`, oder `Remove`.</span><span class="sxs-lookup"><span data-stu-id="04f0c-196">To have applications enforce this pattern, wrap the [DistributedSessionStore](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.session.distributedsessionstore) and [DistributedSession](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.session.distributedsession) implementations with versions that throw an exception if the `LoadAsync` method is not called before `TryGetValue`, `Set`, or `Remove`.</span></span> <span data-ttu-id="04f0c-197">Die umschlossenen Versionen im Container zu registrieren.</span><span class="sxs-lookup"><span data-stu-id="04f0c-197">Register the wrapped versions in the services container.</span></span>

### <a name="implementation-details"></a><span data-ttu-id="04f0c-198">Details zur Implementierung</span><span class="sxs-lookup"><span data-stu-id="04f0c-198">Implementation Details</span></span>

<span data-ttu-id="04f0c-199">Sitzung wird ein Cookie verwendet, verfolgen und Anforderungen von einer einzelnen Browsersitzung bestimmen.</span><span class="sxs-lookup"><span data-stu-id="04f0c-199">Session uses a cookie to track and identify requests from a single browser.</span></span> <span data-ttu-id="04f0c-200">Standardmäßig lautet dieses Cookie ". AspNet.Session", und verwendet den Pfad"/".</span><span class="sxs-lookup"><span data-stu-id="04f0c-200">By default, this cookie is named ".AspNet.Session", and it uses a path of "/".</span></span> <span data-ttu-id="04f0c-201">Da der Cookie-Standardwert keine Domäne angegeben ist, es ist nicht zur Verfügung gestellt des clientseitigen Skripts auf der Seite (da `CookieHttpOnly` standardmäßig `true`).</span><span class="sxs-lookup"><span data-stu-id="04f0c-201">Because the cookie default does not specify a domain, it is not made available to the client-side script on the page (because `CookieHttpOnly` defaults to `true`).</span></span>

<span data-ttu-id="04f0c-202">Um Sitzung Standardwerte zu überschreiben, verwenden `SessionOptions`:</span><span class="sxs-lookup"><span data-stu-id="04f0c-202">To override session defaults, use `SessionOptions`:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="04f0c-203">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="04f0c-203">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

[!code-csharp[Main](app-state/sample/src/WebAppSessionDotNetCore2.0App/StartupCopy.cs?name=snippet1&highlight=8-12)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="04f0c-204">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="04f0c-204">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

[!code-csharp[Main](app-state/sample/src/WebAppSession/StartupCopy.cs?name=snippet1&highlight=8-12)]

---

<span data-ttu-id="04f0c-205">Der Server verwendet die `IdleTimeout` -Eigenschaft können Sie bestimmen, wie lange eine Sitzung im Leerlauf befinden kann, bevor Sie seinen Inhalt abgebrochen werden.</span><span class="sxs-lookup"><span data-stu-id="04f0c-205">The server uses the `IdleTimeout` property to determine how long a session can be idle before its contents are abandoned.</span></span> <span data-ttu-id="04f0c-206">Diese Eigenschaft ist unabhängig von den Ablauf der Cookies.</span><span class="sxs-lookup"><span data-stu-id="04f0c-206">This property is independent of the cookie expiration.</span></span> <span data-ttu-id="04f0c-207">Jede Anforderung, die die Sitzung Middleware (aus gelesen oder geschrieben) durchlaufen setzt das Timeout an.</span><span class="sxs-lookup"><span data-stu-id="04f0c-207">Each request that passes through the Session middleware (read from or written to) resets the timeout.</span></span>

<span data-ttu-id="04f0c-208">Da `Session` ist *nicht sperrende*, wenn zwei Anforderungen, die beide versuchen, zum Ändern des Inhalts der Sitzung, für das letzte Lesezeichen überschreibt das erste.</span><span class="sxs-lookup"><span data-stu-id="04f0c-208">Because `Session` is *non-locking*, if two requests both attempt to modify the contents of session, the last one overrides the first.</span></span> <span data-ttu-id="04f0c-209">`Session`wird als implementiert eine *kohärente Sitzung*, was bedeutet, dass der gesamte Inhalt zusammen gespeichert werden.</span><span class="sxs-lookup"><span data-stu-id="04f0c-209">`Session` is implemented as a *coherent session*, which means that all the contents are stored together.</span></span> <span data-ttu-id="04f0c-210">Zwei Anforderungen, die verschiedene Teile der Sitzung (verschiedene Schlüssel) zu ändern, werden möglicherweise weiterhin auswirken.</span><span class="sxs-lookup"><span data-stu-id="04f0c-210">Two requests that are modifying different parts of the session (different keys) might still impact each other.</span></span>

## <a name="setting-and-getting-session-values"></a><span data-ttu-id="04f0c-211">Festlegen und Abrufen von Sitzungsdaten</span><span class="sxs-lookup"><span data-stu-id="04f0c-211">Setting and getting Session values</span></span>

<span data-ttu-id="04f0c-212">Sitzung erfolgt über die `Session` Eigenschaft `HttpContext`.</span><span class="sxs-lookup"><span data-stu-id="04f0c-212">Session is accessed through the `Session` property on `HttpContext`.</span></span> <span data-ttu-id="04f0c-213">Diese Eigenschaft ist ein [ISession](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.http.isession) Implementierung.</span><span class="sxs-lookup"><span data-stu-id="04f0c-213">This property is an [ISession](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.http.isession) implementation.</span></span>

<span data-ttu-id="04f0c-214">Im folgende Beispiel wird gezeigt, einstellungs- und ein "Int" und eine Zeichenfolge:</span><span class="sxs-lookup"><span data-stu-id="04f0c-214">The following example shows setting and getting an int and a string:</span></span>

[!code-csharp[Main](app-state/sample/src/WebAppSession/Controllers/HomeController.cs?name=snippet1)]

<span data-ttu-id="04f0c-215">Wenn Sie die folgenden Erweiterungsmethoden hinzufügen, können Sie festlegen und Abrufen von serialisierbare Objekte Sitzung:</span><span class="sxs-lookup"><span data-stu-id="04f0c-215">If you add the following extension methods, you can set and get serializable objects to Session:</span></span>

[!code-csharp[Main](app-state/sample/src/WebAppSession/Extensions/SessionExtensions.cs)]

<span data-ttu-id="04f0c-216">Im folgende Beispiel wird gezeigt, wie festgelegt und ein serialisierbares Objekt abgerufen wird:</span><span class="sxs-lookup"><span data-stu-id="04f0c-216">The following sample shows how to set and get a serializable object:</span></span>

[!code-csharp[Main](app-state/sample/src/WebAppSession/Controllers/HomeController.cs?name=snippet2)]


## <a name="working-with-httpcontextitems"></a><span data-ttu-id="04f0c-217">Arbeiten mit HttpContext.Items</span><span class="sxs-lookup"><span data-stu-id="04f0c-217">Working with HttpContext.Items</span></span>

<span data-ttu-id="04f0c-218">Die `HttpContext` Abstraktion bietet Unterstützung für ein Dictionary-Auflistung des Typs `IDictionary<object, object>`namens `Items`.</span><span class="sxs-lookup"><span data-stu-id="04f0c-218">The `HttpContext` abstraction provides support for a dictionary collection of type `IDictionary<object, object>`, called `Items`.</span></span> <span data-ttu-id="04f0c-219">Diese Sammlung ist verfügbar, ab dem Anfang einer *HttpRequest* und am Ende jeder Anforderung verworfen wird.</span><span class="sxs-lookup"><span data-stu-id="04f0c-219">This collection is available from the start of an *HttpRequest* and is discarded at the end of each request.</span></span> <span data-ttu-id="04f0c-220">Sie können darauf zugreifen, durch Zuweisen eines Werts zu einem schlüsselgebundenen Eintrag oder durch den Wert für einen bestimmten Schlüssel anfordern.</span><span class="sxs-lookup"><span data-stu-id="04f0c-220">You can access it by  assigning a value to a keyed entry, or by requesting the value for a particular key.</span></span>

<span data-ttu-id="04f0c-221">Im folgenden Beispiel wird [Middleware](middleware.md) fügt `isVerified` auf die `Items` Auflistung.</span><span class="sxs-lookup"><span data-stu-id="04f0c-221">In the sample below, [Middleware](middleware.md) adds `isVerified` to the `Items` collection.</span></span>

```csharp
app.Use(async (context, next) =>
{
    // perform some verification
    context.Items["isVerified"] = true;
    await next.Invoke();
});
```

<span data-ttu-id="04f0c-222">Weiter unten in der Pipeline kann eine andere Middleware darauf zugreifen:</span><span class="sxs-lookup"><span data-stu-id="04f0c-222">Later in the pipeline, another middleware could access it:</span></span>

```csharp
app.Run(async (context) =>
{
    await context.Response.WriteAsync("Verified request? " + 
        context.Items["isVerified"]);
});
```

<span data-ttu-id="04f0c-223">Für die Middleware, die nur von einer einzigen Anwendung verwendet werden, `string` Schlüssel sind zulässig.</span><span class="sxs-lookup"><span data-stu-id="04f0c-223">For middleware that will only be used by a single app, `string` keys are acceptable.</span></span> <span data-ttu-id="04f0c-224">Allerdings sollten Middleware, die zwischen Anwendungen gemeinsam genutzt werden, eindeutige Objektschlüssel verwenden, um alle Konflikte zu vermeiden.</span><span class="sxs-lookup"><span data-stu-id="04f0c-224">However, middleware that will be shared between applications should use unique object keys to avoid any chance of key collisions.</span></span> <span data-ttu-id="04f0c-225">Wenn Sie Middleware entwickeln, die für mehrere Anwendungen arbeiten müssen, verwenden Sie einen eindeutiges Objektschlüssel in die Middleware-Klasse, die wie folgt definiert:</span><span class="sxs-lookup"><span data-stu-id="04f0c-225">If you are developing middleware that must work across multiple applications, use a unique object key defined in your middleware class as shown below:</span></span>

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

<span data-ttu-id="04f0c-226">Andere Codezugriff können in gespeicherten `HttpContext.Items` mithilfe des Schlüssels, der von der Middleware-Klasse verfügbar gemacht werden:</span><span class="sxs-lookup"><span data-stu-id="04f0c-226">Other code can access the value stored in `HttpContext.Items` using the key exposed by the middleware class:</span></span>

```csharp
public class HomeController : Controller
{
    public IActionResult Index()
    {
        string value = HttpContext.Items[SampleMiddleware.SampleKey];
    }
}
```

<span data-ttu-id="04f0c-227">Dieser Ansatz hat außerdem den Vorteil des Entfernens der Wiederholung von "Magic-Zeichenfolgen" an mehreren Stellen im Code.</span><span class="sxs-lookup"><span data-stu-id="04f0c-227">This approach also has the advantage of eliminating repetition of "magic strings" in multiple places in the code.</span></span>

<a name=appstate-errors></a>

## <a name="application-state-data"></a><span data-ttu-id="04f0c-228">Anwendungszustandsdaten</span><span class="sxs-lookup"><span data-stu-id="04f0c-228">Application state data</span></span>

<span data-ttu-id="04f0c-229">Verwendung [Abhängigkeitsinjektion](xref:fundamentals/dependency-injection) um Daten für alle Benutzer verfügbar zu machen:</span><span class="sxs-lookup"><span data-stu-id="04f0c-229">Use [Dependency Injection](xref:fundamentals/dependency-injection) to make data available to all users:</span></span>

1. <span data-ttu-id="04f0c-230">Definieren Sie einen Dienst, der Daten enthält (z. B. eine Klasse, die mit dem Namen `MyAppData`).</span><span class="sxs-lookup"><span data-stu-id="04f0c-230">Define a service containing the data (for example, a class named `MyAppData`).</span></span>

```csharp
public class MyAppData
{
    // Declare properties/methods/etc.
} 
```
2. <span data-ttu-id="04f0c-231">Hinzufügen der Dienstklasse zum `ConfigureServices` (z. B. `services.AddSingleton<MyAppData>();`).</span><span class="sxs-lookup"><span data-stu-id="04f0c-231">Add the service class to `ConfigureServices` (for example `services.AddSingleton<MyAppData>();`).</span></span>
3. <span data-ttu-id="04f0c-232">Verwenden Sie die Datendienstklasse in jedem Controller:</span><span class="sxs-lookup"><span data-stu-id="04f0c-232">Consume the data service class in each controller:</span></span>

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

### <a name="common-errors-when-working-with-session"></a><span data-ttu-id="04f0c-233">Häufige Fehler bei der Arbeit mit der Sitzung</span><span class="sxs-lookup"><span data-stu-id="04f0c-233">Common errors when working with session</span></span>

* <span data-ttu-id="04f0c-234">"Dienst für Typ"Microsoft.Extensions.Caching.Distributed.IDistributedCache"aufgelöst, bei dem Versuch,"Microsoft.AspNetCore.Session.DistributedSessionStore"aktivieren kann."</span><span class="sxs-lookup"><span data-stu-id="04f0c-234">"Unable to resolve service for type 'Microsoft.Extensions.Caching.Distributed.IDistributedCache' while attempting to activate 'Microsoft.AspNetCore.Session.DistributedSessionStore'."</span></span>

  <span data-ttu-id="04f0c-235">Dies wird normalerweise verursacht durch wegen eines Fehlers beim Konfigurieren von mindestens einer `IDistributedCache` Implementierung.</span><span class="sxs-lookup"><span data-stu-id="04f0c-235">This is usually caused by failing to configure at least one `IDistributedCache` implementation.</span></span> <span data-ttu-id="04f0c-236">Weitere Informationen finden Sie unter [arbeiten mit einem verteilten Cache](xref:performance/caching/distributed) und [im Arbeitsspeicher zwischenspeichern](xref:performance/caching/memory).</span><span class="sxs-lookup"><span data-stu-id="04f0c-236">For more information, see [Working with a Distributed Cache](xref:performance/caching/distributed) and [In memory caching](xref:performance/caching/memory).</span></span>

### <a name="additional-resources"></a><span data-ttu-id="04f0c-237">Zusätzliche Ressourcen</span><span class="sxs-lookup"><span data-stu-id="04f0c-237">Additional Resources</span></span>


* [<span data-ttu-id="04f0c-238">ASP.NET Core 1.x: Beispielcode in diesem Dokument verwendeten</span><span class="sxs-lookup"><span data-stu-id="04f0c-238">ASP.NET Core 1.x: Sample code used in this document</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/app-state/sample/src/WebAppSession)
* [<span data-ttu-id="04f0c-239">ASP.NET Core 2.x: Beispielcode in diesem Dokument verwendeten</span><span class="sxs-lookup"><span data-stu-id="04f0c-239">ASP.NET Core 2.x: Sample code used in this document</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/app-state/sample/src/WebAppSessionDotNetCore2.0App)
