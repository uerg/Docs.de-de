---
title: Sitzungs- und App-Zustand in ASP.NET Core
author: rick-anderson
description: Ansätze zum Erhalten des Sitzungs- und App-Zustands zwischen Sitzungen.
ms.author: riande
ms.custom: mvc
ms.date: 06/14/2018
uid: fundamentals/app-state
ms.openlocfilehash: 5ca909681ca9da3fae0391991902da97581852be
ms.sourcegitcommit: 4a6bbe84db24c2f3dd2de065de418fde952c8d40
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/30/2018
ms.locfileid: "50253181"
---
# <a name="session-and-app-state-in-aspnet-core"></a><span data-ttu-id="d5347-103">Sitzungs- und App-Zustand in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="d5347-103">Session and app state in ASP.NET Core</span></span>

<span data-ttu-id="d5347-104">Von [Rick Anderson](https://twitter.com/RickAndMSFT), [Steve Smith](https://ardalis.com/), [Diana LaRose](https://github.com/DianaLaRose) und [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="d5347-104">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Steve Smith](https://ardalis.com/), [Diana LaRose](https://github.com/DianaLaRose), and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="d5347-105">Bei HTTP handelt es sich um ein zustandsloses Protokoll.</span><span class="sxs-lookup"><span data-stu-id="d5347-105">HTTP is a stateless protocol.</span></span> <span data-ttu-id="d5347-106">HTTP-Anforderungen sind ohne zusätzliche Schritte unabhängige Nachrichten, die keine Benutzerwerte oder App-Zustände beibehalten.</span><span class="sxs-lookup"><span data-stu-id="d5347-106">Without taking additional steps, HTTP requests are independent messages that don't retain user values or app state.</span></span> <span data-ttu-id="d5347-107">In diesem Artikel werden mehrere Ansätze zum Beibehalten von Benutzerdaten und App-Zuständen zwischen Anforderungen beschrieben.</span><span class="sxs-lookup"><span data-stu-id="d5347-107">This article describes several approaches to preserve user data and app state between requests.</span></span>

<span data-ttu-id="d5347-108">[Anzeigen oder Herunterladen von Beispielcode](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/app-state/samples) ([Vorgehensweise zum Herunterladen](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="d5347-108">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/app-state/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="state-management"></a><span data-ttu-id="d5347-109">Zustandsverwaltung</span><span class="sxs-lookup"><span data-stu-id="d5347-109">State management</span></span>

<span data-ttu-id="d5347-110">Zustände können mithilfe mehrerer Ansätze gespeichert werden.</span><span class="sxs-lookup"><span data-stu-id="d5347-110">State can be stored using several approaches.</span></span> <span data-ttu-id="d5347-111">Die Ansätze werden im Verlauf dieses Artikels beschrieben.</span><span class="sxs-lookup"><span data-stu-id="d5347-111">Each approach is described later in this topic.</span></span>

| <span data-ttu-id="d5347-112">Speicheransatz</span><span class="sxs-lookup"><span data-stu-id="d5347-112">Storage approach</span></span> | <span data-ttu-id="d5347-113">Speichermechanismus</span><span class="sxs-lookup"><span data-stu-id="d5347-113">Storage mechanism</span></span> |
| ---------------- | ----------------- |
| [<span data-ttu-id="d5347-114">Cookies</span><span class="sxs-lookup"><span data-stu-id="d5347-114">Cookies</span></span>](#cookies) | <span data-ttu-id="d5347-115">HTTP-Cookies (schließt möglicherweise Daten ein, die mit serverseitigem App-Code gespeichert wurden)</span><span class="sxs-lookup"><span data-stu-id="d5347-115">HTTP cookies (may include data stored using server-side app code)</span></span> |
| [<span data-ttu-id="d5347-116">Sitzungszustand</span><span class="sxs-lookup"><span data-stu-id="d5347-116">Session state</span></span>](#session-state) | <span data-ttu-id="d5347-117">HTTP-Cookies und serverseitiger App-Code</span><span class="sxs-lookup"><span data-stu-id="d5347-117">HTTP cookies and server-side app code</span></span> |
| [<span data-ttu-id="d5347-118">TempData</span><span class="sxs-lookup"><span data-stu-id="d5347-118">TempData</span></span>](#tempdata) | <span data-ttu-id="d5347-119">HTTP-Cookies oder Sitzungszustand</span><span class="sxs-lookup"><span data-stu-id="d5347-119">HTTP cookies or session state</span></span> |
| [<span data-ttu-id="d5347-120">Abfragezeichenfolgen</span><span class="sxs-lookup"><span data-stu-id="d5347-120">Query strings</span></span>](#query-strings) | <span data-ttu-id="d5347-121">HTTP-Abfragezeichenfolgen</span><span class="sxs-lookup"><span data-stu-id="d5347-121">HTTP query strings</span></span> |
| [<span data-ttu-id="d5347-122">Verborgene Felder</span><span class="sxs-lookup"><span data-stu-id="d5347-122">Hidden fields</span></span>](#hidden-fields) | <span data-ttu-id="d5347-123">HTTP-Formularfelder</span><span class="sxs-lookup"><span data-stu-id="d5347-123">HTTP form fields</span></span> |
| [<span data-ttu-id="d5347-124">HttpContext.Items</span><span class="sxs-lookup"><span data-stu-id="d5347-124">HttpContext.Items</span></span>](#httpcontextitems) | <span data-ttu-id="d5347-125">Serverseitiger App-Code</span><span class="sxs-lookup"><span data-stu-id="d5347-125">Server-side app code</span></span> |
| [<span data-ttu-id="d5347-126">Cache</span><span class="sxs-lookup"><span data-stu-id="d5347-126">Cache</span></span>](#cache) | <span data-ttu-id="d5347-127">Serverseitiger App-Code</span><span class="sxs-lookup"><span data-stu-id="d5347-127">Server-side app code</span></span> |
| [<span data-ttu-id="d5347-128">Abhängigkeitsinjektion</span><span class="sxs-lookup"><span data-stu-id="d5347-128">Dependency Injection</span></span>](#dependency-injection) | <span data-ttu-id="d5347-129">Serverseitiger App-Code</span><span class="sxs-lookup"><span data-stu-id="d5347-129">Server-side app code</span></span> |

## <a name="cookies"></a><span data-ttu-id="d5347-130">Cookies</span><span class="sxs-lookup"><span data-stu-id="d5347-130">Cookies</span></span>

<span data-ttu-id="d5347-131">Cookies speichern Daten anforderungsübergreifend.</span><span class="sxs-lookup"><span data-stu-id="d5347-131">Cookies store data across requests.</span></span> <span data-ttu-id="d5347-132">Da mit jeder Anforderung Cookies gesendet werden, sollte deren Größe auf ein Minimum begrenzt sein.</span><span class="sxs-lookup"><span data-stu-id="d5347-132">Because cookies are sent with every request, their size should be kept to a minimum.</span></span> <span data-ttu-id="d5347-133">Idealerweise sollte nur ein Bezeichner in einem Cookie gespeichert werden, während die Daten von der App gespeichert werden sollten.</span><span class="sxs-lookup"><span data-stu-id="d5347-133">Ideally, only an identifier should be stored in a cookie with the data stored by the app.</span></span> <span data-ttu-id="d5347-134">Die meisten Browser beschränken die Größe von Cookies auf 4096 Byte.</span><span class="sxs-lookup"><span data-stu-id="d5347-134">Most browsers restrict cookie size to 4096 bytes.</span></span> <span data-ttu-id="d5347-135">Nur eine begrenzte Anzahl von Cookies ist für jede Domäne verfügbar.</span><span class="sxs-lookup"><span data-stu-id="d5347-135">Only a limited number of cookies are available for each domain.</span></span>

<span data-ttu-id="d5347-136">Da Cookies manipuliert werden können, müssen sie von der App überprüft werden.</span><span class="sxs-lookup"><span data-stu-id="d5347-136">Because cookies are subject to tampering, they must be validated by the app.</span></span> <span data-ttu-id="d5347-137">Cookies können von Benutzern gelöscht werden und können auf Clients ablaufen.</span><span class="sxs-lookup"><span data-stu-id="d5347-137">Cookies can be deleted by users and expire on clients.</span></span> <span data-ttu-id="d5347-138">Cookies sind für gewöhnlich die dauerhafteste Form der Datenpersistenz auf dem Client.</span><span class="sxs-lookup"><span data-stu-id="d5347-138">However, cookies are generally the most durable form of data persistence on the client.</span></span>

<span data-ttu-id="d5347-139">Cookies werden häufig aus Personalisierungsgründen verwendet, wenn Inhalt für einen bekannten Benutzer angepasst wird.</span><span class="sxs-lookup"><span data-stu-id="d5347-139">Cookies are often used for personalization, where content is customized for a known user.</span></span> <span data-ttu-id="d5347-140">In den meisten Fällen wird der Benutzer nur identifiziert und nicht authentifiziert.</span><span class="sxs-lookup"><span data-stu-id="d5347-140">The user is only identified and not authenticated in most cases.</span></span> <span data-ttu-id="d5347-141">Das Cookie kann den Benutzernamen, Kontonamen oder eine eindeutige Benutzer-ID (z.B. die GUID) speichern.</span><span class="sxs-lookup"><span data-stu-id="d5347-141">The cookie can store the user's name, account name, or unique user ID (such as a GUID).</span></span> <span data-ttu-id="d5347-142">Dann können Sie mit dem Cookie auf die persönlichen Einstellungen (z.B. die bevorzugte Farbe des Websitehintergrunds) des Benutzers zugreifen.</span><span class="sxs-lookup"><span data-stu-id="d5347-142">You can then use the cookie to access the user's personalized settings, such as their preferred website background color.</span></span>

<span data-ttu-id="d5347-143">Beachten Sie die [Europäische Datenschutz-Grundverordnung (DSGVO)](https://ec.europa.eu/info/law/law-topic/data-protection) beim Ausstellen von Cookies und beim Umgang mit Aspekten des Datenschutzes.</span><span class="sxs-lookup"><span data-stu-id="d5347-143">Be mindful of the [European Union General Data Protection Regulations (GDPR)](https://ec.europa.eu/info/law/law-topic/data-protection) when issuing cookies and dealing with privacy concerns.</span></span> <span data-ttu-id="d5347-144">Weitere In finden Sie unter [General Data Protection Regulation (GDPR) support in ASP.NET Core (DSGVO-Unterstützung in ASP.NET Core)](xref:security/gdpr).</span><span class="sxs-lookup"><span data-stu-id="d5347-144">For more information, see [General Data Protection Regulation (GDPR) support in ASP.NET Core](xref:security/gdpr).</span></span>

## <a name="session-state"></a><span data-ttu-id="d5347-145">Sitzungszustand</span><span class="sxs-lookup"><span data-stu-id="d5347-145">Session state</span></span>

<span data-ttu-id="d5347-146">Der Sitzungszustand ist ein Szenario in ASP.NET Core zum Speichern von Benutzerdaten, wenn der Benutzer eine Web-App verwendet.</span><span class="sxs-lookup"><span data-stu-id="d5347-146">Session state is an ASP.NET Core scenario for storage of user data while the user browses a web app.</span></span> <span data-ttu-id="d5347-147">Der Sitzungszustand verwendet einen von der App verwalteten Speicher, um Daten für mehrere Anforderungen eines Clients beizubehalten.</span><span class="sxs-lookup"><span data-stu-id="d5347-147">Session state uses a store maintained by the app to persist data across requests from a client.</span></span> <span data-ttu-id="d5347-148">Die Sitzungsdaten werden in einem Cache zwischengespeichert und sind kurzlebig. Die Website sollte auch ohne die Sitzungsdaten weiterhin funktionieren.</span><span class="sxs-lookup"><span data-stu-id="d5347-148">The session data is backed by a cache and considered ephemeral data&mdash;the site should continue to function without the session data.</span></span>

> [!NOTE]
> <span data-ttu-id="d5347-149">Sitzungen werden in [SignalR](xref:signalr/index)-Apps nicht unterstützt, weil ein [SignalR-Hub](xref:signalr/hubs) unabhängig vom HTTP-Kontext ausgeführt werden kann.</span><span class="sxs-lookup"><span data-stu-id="d5347-149">Session isn't supported in [SignalR](xref:signalr/index) apps because a [SignalR Hub](xref:signalr/hubs) may execute independent of an HTTP context.</span></span> <span data-ttu-id="d5347-150">Das kann z.B. passieren, wenn eine lange Abrufanforderung von einem Hub länger als die Lebensdauer des HTTP-Kontexts einer Anforderung offen gehalten wird.</span><span class="sxs-lookup"><span data-stu-id="d5347-150">For example, this can occur when a long polling request is held open by a hub beyond the lifetime of the request's HTTP context.</span></span>

<span data-ttu-id="d5347-151">ASP.NET Core verwaltet den Sitzungszustand, indem ein Cookie an den Client übergeben wird, das die Sitzungs-ID enthält, die mit jeder Anforderung an den Server gesendet wird.</span><span class="sxs-lookup"><span data-stu-id="d5347-151">ASP.NET Core maintains session state by providing a cookie to the client that contains a session ID, which is sent to the app with each request.</span></span> <span data-ttu-id="d5347-152">Die App verwendet die Sitzungs-ID, um die Sitzungsdaten abzurufen.</span><span class="sxs-lookup"><span data-stu-id="d5347-152">The app uses the session ID to fetch the session data.</span></span>

<span data-ttu-id="d5347-153">Der Sitzungszustand verhält sich wie folgt:</span><span class="sxs-lookup"><span data-stu-id="d5347-153">Session state exhibits the following behaviors:</span></span>

* <span data-ttu-id="d5347-154">Da das Sitzungscookie browserspezifisch ist, können Sitzungen nicht für mehrere Browser gleichzeitig verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="d5347-154">Because the session cookie is specific to the browser, sessions aren't shared across browsers.</span></span>
* <span data-ttu-id="d5347-155">Sitzungscookies werden gelöscht, wenn die Browsersitzung abläuft.</span><span class="sxs-lookup"><span data-stu-id="d5347-155">Session cookies are deleted when the browser session ends.</span></span>
* <span data-ttu-id="d5347-156">Wenn für eine abgelaufene Sitzung ein Cookie empfangen wird, wird eine neue Sitzung erstellt, die dasselbe Cookie verwendet.</span><span class="sxs-lookup"><span data-stu-id="d5347-156">If a cookie is received for an expired session, a new session is created that uses the same session cookie.</span></span>
* <span data-ttu-id="d5347-157">Leere Sitzungen werden nicht beibehalten. Die Sitzung muss mindestens einen Wert enthalten, damit die Sitzung anforderungsübergreifend beibehalten wird.</span><span class="sxs-lookup"><span data-stu-id="d5347-157">Empty sessions aren't retained&mdash;the session must have at least one value set into it to persist the session across requests.</span></span> <span data-ttu-id="d5347-158">Wenn eine Sitzung nicht beibehalten wird, wird eine neue Sitzungs-ID für jede neue Anforderung erzeugt.</span><span class="sxs-lookup"><span data-stu-id="d5347-158">When a session isn't retained, a new session ID is generated for each new request.</span></span>
* <span data-ttu-id="d5347-159">Die App speichert Sitzungen für einen beschränkten Zeitraum nach der letzten Anforderung.</span><span class="sxs-lookup"><span data-stu-id="d5347-159">The app retains a session for a limited time after the last request.</span></span> <span data-ttu-id="d5347-160">Die App legt entweder ein Zeitlimit für die Sitzungen fest oder verwendet den Standardwert von 20 Minuten.</span><span class="sxs-lookup"><span data-stu-id="d5347-160">The app either sets the session timeout or uses the default value of 20 minutes.</span></span> <span data-ttu-id="d5347-161">Der Sitzungszustand eignet sich ideal zum Speichern von Benutzerdaten, die für eine bestimmte Sitzung zwar wichtig sind, die jedoch nicht dauerhaft sitzungsübergreifend gespeichert werden müssen.</span><span class="sxs-lookup"><span data-stu-id="d5347-161">Session state is ideal for storing user data that's specific to a particular session but where the data doesn't require permanent storage across sessions.</span></span>
* <span data-ttu-id="d5347-162">Sitzungsdaten werden entweder gelöscht, wenn die [ISession.Clear](/dotnet/api/microsoft.aspnetcore.http.isession.clear)-Implementierung aufgerufen wird oder wenn die Sitzung abläuft.</span><span class="sxs-lookup"><span data-stu-id="d5347-162">Session data is deleted either when the [ISession.Clear](/dotnet/api/microsoft.aspnetcore.http.isession.clear) implementation is called or when the session expires.</span></span>
* <span data-ttu-id="d5347-163">Es gibt kein Standardverfahren, wie App-Code darüber informiert wird, dass ein Clientbrowser geschlossen wurde oder dass ein Sitzungscookie gelöscht wurde oder auf dem Client abgelaufen ist.</span><span class="sxs-lookup"><span data-stu-id="d5347-163">There's no default mechanism to inform app code that a client browser has been closed or when the session cookie is deleted or expired on the client.</span></span>

> [!WARNING]
> <span data-ttu-id="d5347-164">Speichern Sie keine vertraulichen Daten im Sitzungszustand.</span><span class="sxs-lookup"><span data-stu-id="d5347-164">Don't store sensitive data in session state.</span></span> <span data-ttu-id="d5347-165">Es besteht die Möglichkeit, dass der Benutzer seinen Browser nicht schließt oder die Sitzungscookies nicht löscht.</span><span class="sxs-lookup"><span data-stu-id="d5347-165">The user might not close the browser and clear the session cookie.</span></span> <span data-ttu-id="d5347-166">Einige Browser behalten gültige Sitzungscookies browserfensterübergreifend bei.</span><span class="sxs-lookup"><span data-stu-id="d5347-166">Some browsers maintain valid session cookies across browser windows.</span></span> <span data-ttu-id="d5347-167">Eine Sitzung ist möglicherweise nicht nur auf einen Benutzer beschränkt, sodass der nächste Benutzer dasselbe Sitzungscookie verwendet.</span><span class="sxs-lookup"><span data-stu-id="d5347-167">A session might not be restricted to a single user&mdash;the next user might continue to browse the app with the same session cookie.</span></span>

<span data-ttu-id="d5347-168">Der Cacheanbieter im Arbeitsspeicher speichert Sitzungsdaten im Arbeitsspeicher des Servers, auf dem sich die App befindet.</span><span class="sxs-lookup"><span data-stu-id="d5347-168">The in-memory cache provider stores session data in the memory of the server where the app resides.</span></span> <span data-ttu-id="d5347-169">Beachten Sie in einem Szenario mit einer Serverfarm Folgendes:</span><span class="sxs-lookup"><span data-stu-id="d5347-169">In a server farm scenario:</span></span>

* <span data-ttu-id="d5347-170">Verwenden Sie *Sticky Sessions* (anhaftende Sitzungen), um jede Sitzung an eine spezifische App-Instanz auf einem separaten Server zu binden.</span><span class="sxs-lookup"><span data-stu-id="d5347-170">Use *sticky sessions* to tie each session to a specific app instance on an individual server.</span></span> <span data-ttu-id="d5347-171">[Azure App Service](https://azure.microsoft.com/services/app-service/) verwendet [Routing von Anwendungsanforderungen (ARR)](/iis/extensions/planning-for-arr/using-the-application-request-routing-module), um Sticky Sessions standardmäßig zu erzwingen.</span><span class="sxs-lookup"><span data-stu-id="d5347-171">[Azure App Service](https://azure.microsoft.com/services/app-service/) uses [Application Request Routing (ARR)](/iis/extensions/planning-for-arr/using-the-application-request-routing-module) to enforce sticky sessions by default.</span></span> <span data-ttu-id="d5347-172">Diese Sitzungen können allerdings Auswirkungen auf die Skalierbarkeit haben und Updates für Web-Apps erschweren.</span><span class="sxs-lookup"><span data-stu-id="d5347-172">However, sticky sessions can affect scalability and complicate web app updates.</span></span> <span data-ttu-id="d5347-173">Ein geeigneteres Vorgehen ist das Verwenden von Redis oder über SQL Server verteilte Caches, für die keine Sticky Sessions notwendig sind.</span><span class="sxs-lookup"><span data-stu-id="d5347-173">A better approach is to use a Redis or SQL Server distributed cache, which doesn't require sticky sessions.</span></span> <span data-ttu-id="d5347-174">Weitere Informationen finden Sie unter <xref:performance/caching/distributed>.</span><span class="sxs-lookup"><span data-stu-id="d5347-174">For more information, see <xref:performance/caching/distributed>.</span></span>
* <span data-ttu-id="d5347-175">Das Sitzungscookie wird über [IDataProtector](/dotnet/api/microsoft.aspnetcore.dataprotection.idataprotector) verschlüsselt.</span><span class="sxs-lookup"><span data-stu-id="d5347-175">The session cookie is encrypted via [IDataProtector](/dotnet/api/microsoft.aspnetcore.dataprotection.idataprotector).</span></span> <span data-ttu-id="d5347-176">Der Datenschutz muss ordnungsgemäß konfiguriert sein, sodass er Sitzungscookies auf jedem Computer liest.</span><span class="sxs-lookup"><span data-stu-id="d5347-176">Data Protection must be properly configured to read session cookies on each machine.</span></span> <span data-ttu-id="d5347-177">Weitere Informationen finden Sie unter <xref:security/data-protection/introduction> und [Schlüsselspeicheranbieter](xref:security/data-protection/implementation/key-storage-providers).</span><span class="sxs-lookup"><span data-stu-id="d5347-177">For more information, see <xref:security/data-protection/introduction> and [Key storage providers](xref:security/data-protection/implementation/key-storage-providers).</span></span>

### <a name="configure-session-state"></a><span data-ttu-id="d5347-178">Konfigurieren des Sitzungszustands</span><span class="sxs-lookup"><span data-stu-id="d5347-178">Configure session state</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="d5347-179">Das Paket [Microsoft.AspNetCore.Session](https://www.nuget.org/packages/Microsoft.AspNetCore.Session/), das im [Metapaket Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) enthalten ist, bietet Middleware zum Verwalten des Sitzungszustands.</span><span class="sxs-lookup"><span data-stu-id="d5347-179">The [Microsoft.AspNetCore.Session](https://www.nuget.org/packages/Microsoft.AspNetCore.Session/) package, which is included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app), provides middleware for managing session state.</span></span> <span data-ttu-id="d5347-180">`Startup` muss folgende Elemente enthalten, um die Sitzungsmiddleware zu aktivieren:</span><span class="sxs-lookup"><span data-stu-id="d5347-180">To enable the session middleware, `Startup` must contain:</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="d5347-181">Das Paket [Microsoft.AspNetCore.Session](https://www.nuget.org/packages/Microsoft.AspNetCore.Session/) stellt Middleware zum Verwalten des Sitzungszustands zur Verfügung.</span><span class="sxs-lookup"><span data-stu-id="d5347-181">The [Microsoft.AspNetCore.Session](https://www.nuget.org/packages/Microsoft.AspNetCore.Session/) package provides middleware for managing session state.</span></span> <span data-ttu-id="d5347-182">`Startup` muss folgende Elemente enthalten, um die Sitzungsmiddleware zu aktivieren:</span><span class="sxs-lookup"><span data-stu-id="d5347-182">To enable the session middleware, `Startup` must contain:</span></span>

::: moniker-end

* <span data-ttu-id="d5347-183">Einen der [IDistributedCache](/dotnet/api/microsoft.extensions.caching.distributed.idistributedcache)-Arbeitsspeichercaches</span><span class="sxs-lookup"><span data-stu-id="d5347-183">Any of the [IDistributedCache](/dotnet/api/microsoft.extensions.caching.distributed.idistributedcache) memory caches.</span></span> <span data-ttu-id="d5347-184">Die `IDistributedCache`-Implementierung wird als Sicherungsspeicher für Sitzungen verwendet.</span><span class="sxs-lookup"><span data-stu-id="d5347-184">The `IDistributedCache` implementation is used as a backing store for session.</span></span> <span data-ttu-id="d5347-185">Weitere Informationen finden Sie unter <xref:performance/caching/distributed>.</span><span class="sxs-lookup"><span data-stu-id="d5347-185">For more information, see <xref:performance/caching/distributed>.</span></span>
* <span data-ttu-id="d5347-186">Einen Aufruf von [AddSession](/dotnet/api/microsoft.extensions.dependencyinjection.sessionservicecollectionextensions.addsession) in `ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="d5347-186">A call to [AddSession](/dotnet/api/microsoft.extensions.dependencyinjection.sessionservicecollectionextensions.addsession) in `ConfigureServices`.</span></span>
* <span data-ttu-id="d5347-187">Einen Aufruf von [UseSession](/dotnet/api/microsoft.aspnetcore.builder.sessionmiddlewareextensions#methods_) in `Configure`.</span><span class="sxs-lookup"><span data-stu-id="d5347-187">A call to [UseSession](/dotnet/api/microsoft.aspnetcore.builder.sessionmiddlewareextensions#methods_) in `Configure`.</span></span>

<span data-ttu-id="d5347-188">Der folgende Code zeigt, wie Sie den speicherinternen Sitzungsanbieter mit einer Standardimplementierung von `IDistributedCache` im Arbeitsspeicher einrichten:</span><span class="sxs-lookup"><span data-stu-id="d5347-188">The following code shows how to set up the in-memory session provider with a default in-memory implementation of `IDistributedCache`:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](app-state/samples/2.x/SessionSample/Startup.cs?name=snippet1&highlight=11,13-18,39)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](app-state/samples/1.x/SessionSample/Startup.cs?name=snippet1&highlight=5,7-12,19)]

::: moniker-end

<span data-ttu-id="d5347-189">Die Reihenfolge der Middleware ist wichtig.</span><span class="sxs-lookup"><span data-stu-id="d5347-189">The order of middleware is important.</span></span> <span data-ttu-id="d5347-190">Im vorherigen Beispiel kommt es zu einer `InvalidOperationException`-Ausnahme, wenn `UseSession` nach `UseMvc` aufgerufen wird.</span><span class="sxs-lookup"><span data-stu-id="d5347-190">In the preceding example, an `InvalidOperationException` exception occurs when `UseSession` is invoked after `UseMvc`.</span></span> <span data-ttu-id="d5347-191">Weitere Informationen finden Sie unter [Middleware](xref:fundamentals/middleware/index#order).</span><span class="sxs-lookup"><span data-stu-id="d5347-191">For more information, see [Middleware Ordering](xref:fundamentals/middleware/index#order).</span></span>

<span data-ttu-id="d5347-192">[HttpContext.Session](/dotnet/api/microsoft.aspnetcore.http.httpcontext.session) ist verfügbar, nachdem der Sitzungszustand konfiguriert wurde.</span><span class="sxs-lookup"><span data-stu-id="d5347-192">[HttpContext.Session](/dotnet/api/microsoft.aspnetcore.http.httpcontext.session) is available after session state is configured.</span></span>

<span data-ttu-id="d5347-193">Auf `HttpContext.Session` kann vor einem Aufruf von `UseSession` nicht zugegriffen werden.</span><span class="sxs-lookup"><span data-stu-id="d5347-193">`HttpContext.Session` can't be accessed before `UseSession` has been called.</span></span>

<span data-ttu-id="d5347-194">Nachdem die App mit dem Schreiben in den Antwortdatenstrom begonnen hat, kann keine neue Sitzung mit einem neuen Sitzungscookie erstellt werden.</span><span class="sxs-lookup"><span data-stu-id="d5347-194">A new session with a new session cookie can't be created after the app has begun writing to the response stream.</span></span> <span data-ttu-id="d5347-195">Die Ausnahme wird im Webserverprotokoll erfasst und nicht im Browser angezeigt.</span><span class="sxs-lookup"><span data-stu-id="d5347-195">The exception is recorded in the web server log and not displayed in the browser.</span></span>

### <a name="load-session-state-asynchronously"></a><span data-ttu-id="d5347-196">Asynchrones Laden des Sitzungszustands</span><span class="sxs-lookup"><span data-stu-id="d5347-196">Load session state asynchronously</span></span>

<span data-ttu-id="d5347-197">Der Standardsitzungsanbieter in ASP.NET Core lädt Sitzungsaufzeichnungen aus dem zugrunde liegenden [IDistributedCache](/dotnet/api/microsoft.extensions.caching.distributed.idistributedcache)-Sicherungsspeicher nur asynchron, wenn die [ISession.LoadAsync](/dotnet/api/microsoft.aspnetcore.http.isession.loadasync)-Methode explizit vor den Methoden [TryGetValue](/dotnet/api/microsoft.aspnetcore.http.isession.trygetvalue), [Set](/dotnet/api/microsoft.aspnetcore.http.isession.set) oder [Remove](/dotnet/api/microsoft.aspnetcore.http.isession.remove) aufgerufen wird.</span><span class="sxs-lookup"><span data-stu-id="d5347-197">The default session provider in ASP.NET Core loads session records from the underlying [IDistributedCache](/dotnet/api/microsoft.extensions.caching.distributed.idistributedcache) backing store asynchronously only if the [ISession.LoadAsync](/dotnet/api/microsoft.aspnetcore.http.isession.loadasync) method is explicitly called before the [TryGetValue](/dotnet/api/microsoft.aspnetcore.http.isession.trygetvalue), [Set](/dotnet/api/microsoft.aspnetcore.http.isession.set), or [Remove](/dotnet/api/microsoft.aspnetcore.http.isession.remove) methods.</span></span> <span data-ttu-id="d5347-198">Wenn `LoadAsync` nicht zuerst aufgerufen wird, werden die zugrunde liegenden Sitzungsaufzeichnungen synchron geladen, was entsprechende Auswirkungen auf die Leistung haben kann.</span><span class="sxs-lookup"><span data-stu-id="d5347-198">If `LoadAsync` isn't called first, the underlying session record is loaded synchronously, which can incur a performance penalty at scale.</span></span>

<span data-ttu-id="d5347-199">Damit Apps dieses Muster erzwingen, umschließen Sie die Implementierungen [DistributedSessionStore](/dotnet/api/microsoft.aspnetcore.session.distributedsessionstore) und [DistributedSession](/dotnet/api/microsoft.aspnetcore.session.distributedsession) mit Versionen, die eine Ausnahme auslösen, wenn die `LoadAsync`-Methode nicht vor `TryGetValue`, `Set` oder `Remove` aufgerufen wird.</span><span class="sxs-lookup"><span data-stu-id="d5347-199">To have apps enforce this pattern, wrap the [DistributedSessionStore](/dotnet/api/microsoft.aspnetcore.session.distributedsessionstore) and [DistributedSession](/dotnet/api/microsoft.aspnetcore.session.distributedsession) implementations with versions that throw an exception if the `LoadAsync` method isn't called before `TryGetValue`, `Set`, or `Remove`.</span></span> <span data-ttu-id="d5347-200">Registrieren Sie die umschlossenen Versionen in den Dienstcontainern.</span><span class="sxs-lookup"><span data-stu-id="d5347-200">Register the wrapped versions in the services container.</span></span>

### <a name="session-options"></a><span data-ttu-id="d5347-201">Sitzungsoptionen</span><span class="sxs-lookup"><span data-stu-id="d5347-201">Session options</span></span>

<span data-ttu-id="d5347-202">Verwenden Sie [Sitzungsoptionen](/dotnet/api/microsoft.aspnetcore.builder.sessionoptions), um Standardwerte für Sitzungen zu überschreiben.</span><span class="sxs-lookup"><span data-stu-id="d5347-202">To override session defaults, use [SessionOptions](/dotnet/api/microsoft.aspnetcore.builder.sessionoptions).</span></span>

::: moniker range=">= aspnetcore-2.0"

| <span data-ttu-id="d5347-203">Option</span><span class="sxs-lookup"><span data-stu-id="d5347-203">Option</span></span> | <span data-ttu-id="d5347-204">Beschreibung </span><span class="sxs-lookup"><span data-stu-id="d5347-204">Description</span></span> |
| ------ | ----------- |
| [<span data-ttu-id="d5347-205">Cookie</span><span class="sxs-lookup"><span data-stu-id="d5347-205">Cookie</span></span>](/dotnet/api/microsoft.aspnetcore.builder.sessionoptions.cookie) | <span data-ttu-id="d5347-206">Bestimmt die Einstellungen, die zum Erstellen des Cookies verwendet wurden.</span><span class="sxs-lookup"><span data-stu-id="d5347-206">Determines the settings used to create the cookie.</span></span> <span data-ttu-id="d5347-207">Der Standardwert von [Name](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.name) ist [SessionDefaults.CookieName](/dotnet/api/microsoft.aspnetcore.session.sessiondefaults.cookiename) (`.AspNetCore.Session`).</span><span class="sxs-lookup"><span data-stu-id="d5347-207">[Name](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.name) defaults to [SessionDefaults.CookieName](/dotnet/api/microsoft.aspnetcore.session.sessiondefaults.cookiename) (`.AspNetCore.Session`).</span></span> <span data-ttu-id="d5347-208">Der Standardwert von [Path](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.path) ist [SessionDefaults.CookiePath](/dotnet/api/microsoft.aspnetcore.session.sessiondefaults.cookiepath) (`/`).</span><span class="sxs-lookup"><span data-stu-id="d5347-208">[Path](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.path) defaults to [SessionDefaults.CookiePath](/dotnet/api/microsoft.aspnetcore.session.sessiondefaults.cookiepath) (`/`).</span></span> <span data-ttu-id="d5347-209">Der Standardwert von [SameSite](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.samesite) ist [SameSiteMode.Lax](/dotnet/api/microsoft.aspnetcore.http.samesitemode) (`1`).</span><span class="sxs-lookup"><span data-stu-id="d5347-209">[SameSite](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.samesite) defaults to [SameSiteMode.Lax](/dotnet/api/microsoft.aspnetcore.http.samesitemode) (`1`).</span></span> <span data-ttu-id="d5347-210">Der Standardwert von [HttpOnly](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.httponly) ist `true`.</span><span class="sxs-lookup"><span data-stu-id="d5347-210">[HttpOnly](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.httponly) defaults to `true`.</span></span> <span data-ttu-id="d5347-211">Der Standardwert von [IsEssential](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.isessential) ist `false`.</span><span class="sxs-lookup"><span data-stu-id="d5347-211">[IsEssential](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.isessential) defaults to `false`.</span></span> |
| [<span data-ttu-id="d5347-212">IdleTimeout</span><span class="sxs-lookup"><span data-stu-id="d5347-212">IdleTimeout</span></span>](/dotnet/api/microsoft.aspnetcore.builder.sessionoptions.idletimeout) | <span data-ttu-id="d5347-213">`IdleTimeout` gibt an, wie lang die Sitzung sich im Leerlauf befinden darf, bevor die Inhalte verworfen werden.</span><span class="sxs-lookup"><span data-stu-id="d5347-213">The `IdleTimeout` indicates how long the session can be idle before its contents are abandoned.</span></span> <span data-ttu-id="d5347-214">Jeder Zugriff auf eine Sitzung setzt das Zeitlimit zurück.</span><span class="sxs-lookup"><span data-stu-id="d5347-214">Each session access resets the timeout.</span></span> <span data-ttu-id="d5347-215">Diese Einstellung gilt nur für den Inhalt der Sitzung und nicht für den Cookie.</span><span class="sxs-lookup"><span data-stu-id="d5347-215">This setting only applies to the content of the session, not the cookie.</span></span> <span data-ttu-id="d5347-216">Der Standardwert beträgt 20 Minuten.</span><span class="sxs-lookup"><span data-stu-id="d5347-216">The default is 20 minutes.</span></span> |
| [<span data-ttu-id="d5347-217">IOTimeout</span><span class="sxs-lookup"><span data-stu-id="d5347-217">IOTimeout</span></span>](/dotnet/api/microsoft.aspnetcore.builder.sessionoptions.iotimeout) | <span data-ttu-id="d5347-218">Das Zeitlimit, wie lange eine Sitzung aus dem Speicher geladen werden soll oder wie lange sie in den Speicher committet werden soll.</span><span class="sxs-lookup"><span data-stu-id="d5347-218">The maximim amount of time allowed to load a session from the store or to commit it back to the store.</span></span> <span data-ttu-id="d5347-219">Diese Einstellung gilt möglicherweise nur für asynchrone Vorgänge.</span><span class="sxs-lookup"><span data-stu-id="d5347-219">This setting may only apply to asynchronous operations.</span></span> <span data-ttu-id="d5347-220">Dieses Zeitlimit kann mit [InfiniteTimeSpan](/dotnet/api/system.threading.timeout.infinitetimespan) deaktiviert werden.</span><span class="sxs-lookup"><span data-stu-id="d5347-220">This timeout can be disabled using [InfiniteTimeSpan](/dotnet/api/system.threading.timeout.infinitetimespan).</span></span> <span data-ttu-id="d5347-221">Der Standardwert beträgt 1&#160;Minute.</span><span class="sxs-lookup"><span data-stu-id="d5347-221">The default is 1 minute.</span></span> |

<span data-ttu-id="d5347-222">Die Sitzung verwendet ein Cookie, um Anforderungen eines Browsers nachzuverfolgen und zu identifizieren.</span><span class="sxs-lookup"><span data-stu-id="d5347-222">Session uses a cookie to track and identify requests from a single browser.</span></span> <span data-ttu-id="d5347-223">Standardmäßig wird das Cookie `.AspNetCore.Session` genannt und verwendet den Pfad von `/`.</span><span class="sxs-lookup"><span data-stu-id="d5347-223">By default, this cookie is named `.AspNetCore.Session`, and it uses a path of `/`.</span></span> <span data-ttu-id="d5347-224">Da das Standardcookie keine Domäne festlegt, wird es dem clientseitigen Skript auf der Seite nicht zur Verfügung gestellt, da der Standardwert von [HttpOnly](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.httponly) `true` ist.</span><span class="sxs-lookup"><span data-stu-id="d5347-224">Because the cookie default doesn't specify a domain, it isn't made available to the client-side script on the page (because [HttpOnly](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.httponly) defaults to `true`).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

| <span data-ttu-id="d5347-225">Option</span><span class="sxs-lookup"><span data-stu-id="d5347-225">Option</span></span> | <span data-ttu-id="d5347-226">Beschreibung </span><span class="sxs-lookup"><span data-stu-id="d5347-226">Description</span></span> |
| ------ | ----------- |
| [<span data-ttu-id="d5347-227">CookieDomain</span><span class="sxs-lookup"><span data-stu-id="d5347-227">CookieDomain</span></span>](/dotnet/api/microsoft.aspnetcore.builder.sessionoptions.cookiedomain) | <span data-ttu-id="d5347-228">Bestimmt die Domäne, mit der ein Cookie erstellt wurde.</span><span class="sxs-lookup"><span data-stu-id="d5347-228">Determines the domain used to create the cookie.</span></span> <span data-ttu-id="d5347-229">`CookieDomain` ist standardmäßig nicht festgelegt.</span><span class="sxs-lookup"><span data-stu-id="d5347-229">`CookieDomain` isn't set by default.</span></span> |
| [<span data-ttu-id="d5347-230">CookieHttpOnly</span><span class="sxs-lookup"><span data-stu-id="d5347-230">CookieHttpOnly</span></span>](/dotnet/api/microsoft.aspnetcore.builder.sessionoptions.cookiehttponly) | <span data-ttu-id="d5347-231">Bestimmt, ob der Browser zulässt, dass auf das Cookie über clientseitiges JavaScript zugegriffen wird.</span><span class="sxs-lookup"><span data-stu-id="d5347-231">Determines if the browser should allow the cookie to be accessed by client-side JavaScript.</span></span> <span data-ttu-id="d5347-232">Der Standardwert ist `true`. D.h., dass das Cookie nur an HTTP-Anforderungen übergeben und nicht für Skript auf der Seite verfügbar gemacht wird.</span><span class="sxs-lookup"><span data-stu-id="d5347-232">The default is `true`, which means the cookie is only passed to HTTP requests and isn't made available to script on the page.</span></span> |
| [<span data-ttu-id="d5347-233">CookieName</span><span class="sxs-lookup"><span data-stu-id="d5347-233">CookieName</span></span>](/dotnet/api/microsoft.aspnetcore.builder.sessionoptions.cookiename) | <span data-ttu-id="d5347-234">Bestimmt den Cookienamen, der zum Beibehalten der Sitzungs-ID verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="d5347-234">Determines the cookie name used to persist the session ID.</span></span> <span data-ttu-id="d5347-235">Der Standardwert ist [SessionDefaults.CookieName](/dotnet/api/microsoft.aspnetcore.session.sessiondefaults.cookiename) (`.AspNetCore.Session`).</span><span class="sxs-lookup"><span data-stu-id="d5347-235">The default is [SessionDefaults.CookieName](/dotnet/api/microsoft.aspnetcore.session.sessiondefaults.cookiename) (`.AspNetCore.Session`).</span></span> |
| [<span data-ttu-id="d5347-236">CookiePath</span><span class="sxs-lookup"><span data-stu-id="d5347-236">CookiePath</span></span>](/dotnet/api/microsoft.aspnetcore.builder.sessionoptions.cookiepath) | <span data-ttu-id="d5347-237">Bestimmt den Pfad, mit dem ein Cookie erstellt wurde.</span><span class="sxs-lookup"><span data-stu-id="d5347-237">Determines the path used to create the cookie.</span></span> <span data-ttu-id="d5347-238">Der Standardwert ist [SessionDefaults.CookiePath](/dotnet/api/microsoft.aspnetcore.session.sessiondefaults.cookiepath) (`/`).</span><span class="sxs-lookup"><span data-stu-id="d5347-238">Defaults to [SessionDefaults.CookiePath](/dotnet/api/microsoft.aspnetcore.session.sessiondefaults.cookiepath) (`/`).</span></span> |
| [<span data-ttu-id="d5347-239">CookieSecure</span><span class="sxs-lookup"><span data-stu-id="d5347-239">CookieSecure</span></span>](/dotnet/api/microsoft.aspnetcore.builder.sessionoptions.cookiesecure) | <span data-ttu-id="d5347-240">Bestimmt, ob das Cookie nur bei HTTPS-Anforderungen übertragen werden soll.</span><span class="sxs-lookup"><span data-stu-id="d5347-240">Determines if the cookie should only be transmitted on HTTPS requests.</span></span> <span data-ttu-id="d5347-241">Der Standardwert ist [CookieSecurePolicy.None](/dotnet/api/microsoft.aspnetcore.http.cookiesecurepolicy) (`2`).</span><span class="sxs-lookup"><span data-stu-id="d5347-241">The default is [CookieSecurePolicy.None](/dotnet/api/microsoft.aspnetcore.http.cookiesecurepolicy) (`2`).</span></span> |
| [<span data-ttu-id="d5347-242">IdleTimeout</span><span class="sxs-lookup"><span data-stu-id="d5347-242">IdleTimeout</span></span>](/dotnet/api/microsoft.aspnetcore.builder.sessionoptions.idletimeout) | <span data-ttu-id="d5347-243">`IdleTimeout` gibt an, wie lang die Sitzung sich im Leerlauf befinden darf, bevor die Inhalte verworfen werden.</span><span class="sxs-lookup"><span data-stu-id="d5347-243">The `IdleTimeout` indicates how long the session can be idle before its contents are abandoned.</span></span> <span data-ttu-id="d5347-244">Jeder Zugriff auf eine Sitzung setzt das Zeitlimit zurück.</span><span class="sxs-lookup"><span data-stu-id="d5347-244">Each session access resets the timeout.</span></span> <span data-ttu-id="d5347-245">Beachten Sie, dass dies nur für den Inhalt der Sitzung und nicht den Cookie gilt.</span><span class="sxs-lookup"><span data-stu-id="d5347-245">Note this only applies to the content of the session, not the cookie.</span></span> <span data-ttu-id="d5347-246">Der Standardwert beträgt 20 Minuten.</span><span class="sxs-lookup"><span data-stu-id="d5347-246">The default is 20 minutes.</span></span> |

<span data-ttu-id="d5347-247">Die Sitzung verwendet ein Cookie, um Anforderungen eines Browsers nachzuverfolgen und zu identifizieren.</span><span class="sxs-lookup"><span data-stu-id="d5347-247">Session uses a cookie to track and identify requests from a single browser.</span></span> <span data-ttu-id="d5347-248">Standardmäßig wird das Cookie `.AspNet.Session` genannt und verwendet den Pfad von `/`.</span><span class="sxs-lookup"><span data-stu-id="d5347-248">By default, this cookie is named `.AspNet.Session`, and it uses a path of `/`.</span></span>

::: moniker-end

<span data-ttu-id="d5347-249">Verwenden Sie `SessionOptions`, um Standardwerte für Sitzungscookies zu überschreiben:</span><span class="sxs-lookup"><span data-stu-id="d5347-249">To override cookie session defaults, use `SessionOptions`:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](app-state/samples_snapshot/2.x/SessionSample/Startup.cs?name=snippet1&highlight=13-18)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](app-state/samples_snapshot/1.x/SessionSample/Startup.cs?name=snippet1&highlight=5-9)]

::: moniker-end

<span data-ttu-id="d5347-250">Die App verwendet die [IdleTimeout](/dotnet/api/microsoft.aspnetcore.builder.sessionoptions.idletimeout)-Eigenschaft, um zu bestimmen, wie lange sich die Sitzung im Leerlauf befinden kann, bevor ihre Inhalte im Cache des Servers verworfen werden.</span><span class="sxs-lookup"><span data-stu-id="d5347-250">The app uses the [IdleTimeout](/dotnet/api/microsoft.aspnetcore.builder.sessionoptions.idletimeout) property to determine how long a session can be idle before its contents in the server's cache are abandoned.</span></span> <span data-ttu-id="d5347-251">Diese Eigenschaft ist unabhängig vom Ablauf der Cookies.</span><span class="sxs-lookup"><span data-stu-id="d5347-251">This property is independent of the cookie expiration.</span></span> <span data-ttu-id="d5347-252">Jede Anforderung, die über die [Sitzungsmiddleware](/dotnet/api/microsoft.aspnetcore.session.sessionmiddleware) übergeben wird, setzt das Zeitlimit zurück.</span><span class="sxs-lookup"><span data-stu-id="d5347-252">Each request that passes through the [Session Middleware](/dotnet/api/microsoft.aspnetcore.session.sessionmiddleware) resets the timeout.</span></span>

<span data-ttu-id="d5347-253">Der Sitzungszustand ist *nicht sperrend*.</span><span class="sxs-lookup"><span data-stu-id="d5347-253">Session state is *non-locking*.</span></span> <span data-ttu-id="d5347-254">Die letzte Anforderung überschreibt die erste, wenn zwei Anforderungen gleichzeitig versuchen, die Inhalte einer Sitzung zu bearbeiten.</span><span class="sxs-lookup"><span data-stu-id="d5347-254">If two requests simultaneously attempt to modify the contents of a session, the last request overrides the first.</span></span> <span data-ttu-id="d5347-255">`Session` wird als *kohärente Sitzung* implementiert, d.h., dass alle Inhalte zusammen gespeichert werden.</span><span class="sxs-lookup"><span data-stu-id="d5347-255">`Session` is implemented as a *coherent session*, which means that all the contents are stored together.</span></span> <span data-ttu-id="d5347-256">Wenn zwei Anforderungen unterschiedliche Sitzungswerte bearbeiten möchten, überschreibt die letzte Anforderung möglicherweise Anforderungen der ersten.</span><span class="sxs-lookup"><span data-stu-id="d5347-256">When two requests seek to modify different session values, the last request may override session changes made by the first.</span></span>

### <a name="set-and-get-session-values"></a><span data-ttu-id="d5347-257">Festlegen und Abrufen von Sitzungswerten</span><span class="sxs-lookup"><span data-stu-id="d5347-257">Set and get Session values</span></span>

<span data-ttu-id="d5347-258">Sie können über die Razor Pages-Klasse [PageModel](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel) oder die MVC-Klasse [Controller](/dotnet/api/microsoft.aspnetcore.mvc.controller) mit [HttpContext.Session](/dotnet/api/microsoft.aspnetcore.http.httpcontext.session) auf den Sitzungszustand zugreifen.</span><span class="sxs-lookup"><span data-stu-id="d5347-258">Session state is accessed from a Razor Pages [PageModel](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel) class or MVC [Controller](/dotnet/api/microsoft.aspnetcore.mvc.controller) class with [HttpContext.Session](/dotnet/api/microsoft.aspnetcore.http.httpcontext.session).</span></span> <span data-ttu-id="d5347-259">Bei der Eigenschaft handelt es sich um eine [ISession](/dotnet/api/microsoft.aspnetcore.http.isession)-Implementierung.</span><span class="sxs-lookup"><span data-stu-id="d5347-259">This property is an [ISession](/dotnet/api/microsoft.aspnetcore.http.isession) implementation.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="d5347-260">Die `ISession`-Implementierung bietet mehrere Erweiterungsmethoden zum Festlegen und Abrufen von ganzzahligen und Zeichenfolgenwerten.</span><span class="sxs-lookup"><span data-stu-id="d5347-260">The `ISession` implementation provides several extension methods to set and retreive integer and string values.</span></span> <span data-ttu-id="d5347-261">Die Erweiterungsmethoden befinden sich im Namespace [Microsoft.AspNetCore.Http](/dotnet/api/microsoft.aspnetcore.http), wenn vom Projekt auf das Paket `using Microsoft.AspNetCore.Http;`Microsoft.AspNetCore.Http.Extensions[ verwiesen wird (fügen Sie eine ](https://www.nuget.org/packages/Microsoft.AspNetCore.Http.Extensions/)-Anweisung hinzu, um Zugriff auf die Erweiterungsmethoden zu erhalten).</span><span class="sxs-lookup"><span data-stu-id="d5347-261">The extension methods are in the [Microsoft.AspNetCore.Http](/dotnet/api/microsoft.aspnetcore.http) namespace (add a `using Microsoft.AspNetCore.Http;` statement to gain access to the extension methods) when the [Microsoft.AspNetCore.Http.Extensions](https://www.nuget.org/packages/Microsoft.AspNetCore.Http.Extensions/) package is referenced by the project.</span></span> <span data-ttu-id="d5347-262">Beide Pakete sind im Metapaket [Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) enthalten.</span><span class="sxs-lookup"><span data-stu-id="d5347-262">Both packages are included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="d5347-263">Die `ISession`-Implementierung bietet mehrere Erweiterungsmethoden zum Festlegen und Abrufen von ganzzahligen und Zeichenfolgenwerten.</span><span class="sxs-lookup"><span data-stu-id="d5347-263">The `ISession` implementation provides several extension methods to set and retreive integer and string values.</span></span> <span data-ttu-id="d5347-264">Die Erweiterungsmethoden befinden sich im Namespace [Microsoft.AspNetCore.Http](/dotnet/api/microsoft.aspnetcore.http), wenn vom Projekt auf das Paket `using Microsoft.AspNetCore.Http;`Microsoft.AspNetCore.Http.Extensions[ verwiesen wird (fügen Sie eine ](https://www.nuget.org/packages/Microsoft.AspNetCore.Http.Extensions/)-Anweisung hinzu, um Zugriff auf die Erweiterungsmethoden zu erhalten).</span><span class="sxs-lookup"><span data-stu-id="d5347-264">The extension methods are in the [Microsoft.AspNetCore.Http](/dotnet/api/microsoft.aspnetcore.http) namespace (add a `using Microsoft.AspNetCore.Http;` statement to gain access to the extension methods) when the [Microsoft.AspNetCore.Http.Extensions](https://www.nuget.org/packages/Microsoft.AspNetCore.Http.Extensions/) package is referenced by the project.</span></span>

::: moniker-end

<span data-ttu-id="d5347-265">`ISession`-Erweiterungsmethoden:</span><span class="sxs-lookup"><span data-stu-id="d5347-265">`ISession` extension methods:</span></span>

* [<span data-ttu-id="d5347-266">Get(ISession, String)</span><span class="sxs-lookup"><span data-stu-id="d5347-266">Get(ISession, String)</span></span>](/dotnet/api/microsoft.aspnetcore.http.sessionextensions.get)
* [<span data-ttu-id="d5347-267">GetInt32(ISession, String)</span><span class="sxs-lookup"><span data-stu-id="d5347-267">GetInt32(ISession, String)</span></span>](/dotnet/api/microsoft.aspnetcore.http.sessionextensions.getint32)
* [<span data-ttu-id="d5347-268">GetString(ISession, String)</span><span class="sxs-lookup"><span data-stu-id="d5347-268">GetString(ISession, String)</span></span>](/dotnet/api/microsoft.aspnetcore.http.sessionextensions.getstring)
* [<span data-ttu-id="d5347-269">SetInt32(ISession, String, Int32)</span><span class="sxs-lookup"><span data-stu-id="d5347-269">SetInt32(ISession, String, Int32)</span></span>](/dotnet/api/microsoft.aspnetcore.http.sessionextensions.setint32)
* [<span data-ttu-id="d5347-270">SetString(ISession, String, String)</span><span class="sxs-lookup"><span data-stu-id="d5347-270">SetString(ISession, String, String)</span></span>](/dotnet/api/microsoft.aspnetcore.http.sessionextensions.setstring)

<span data-ttu-id="d5347-271">Mit dem folgenden Codebeispiel wird der Sitzungswert für den `IndexModel.SessionKeyName`-Schlüssel (`_Name` in der Beispiel-App) auf einer Razor Pages-Seite abgerufen:</span><span class="sxs-lookup"><span data-stu-id="d5347-271">The following example retrieves the session value for the `IndexModel.SessionKeyName` key (`_Name` in the sample app) in a Razor Pages page:</span></span>

```csharp
@page
@using Microsoft.AspNetCore.Http
@model IndexModel

...

Name: @HttpContext.Session.GetString(IndexModel.SessionKeyName)
```

<span data-ttu-id="d5347-272">Im folgenden Codebeispiel wird veranschaulicht, wie Sie eine ganze Zahl und eine Zeichenfolge abrufen und festlegen können:</span><span class="sxs-lookup"><span data-stu-id="d5347-272">The following example shows how to set and get an integer and a string:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](app-state/samples/2.x/SessionSample/Pages/Index.cshtml.cs?name=snippet1&highlight=18-19,22-23)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](app-state/samples/1.x/SessionSample/Controllers/HomeController.cs?name=snippet1&highlight=10-11,18-19)]

::: moniker-end

<span data-ttu-id="d5347-273">Alle Sitzungsdaten müssen serialisiert werden, um ein Szenario mit einem verteilten Cache zu ermöglichen, auch wenn Sie den speicherinternen Cache verwenden.</span><span class="sxs-lookup"><span data-stu-id="d5347-273">All session data must be serialized to enable a distributed cache scenario, even when using the in-memory cache.</span></span> <span data-ttu-id="d5347-274">Die mindestens erforderlichen Zeichenfolgen- und Zahlenserialisierungsmodule werden bereitgestellt (siehe Methoden und Erweiterungsmethoden von [ISession](/dotnet/api/microsoft.aspnetcore.http.isession)).</span><span class="sxs-lookup"><span data-stu-id="d5347-274">Minimal string and number serializers are provided (see the methods and extension methods of [ISession](/dotnet/api/microsoft.aspnetcore.http.isession)).</span></span> <span data-ttu-id="d5347-275">Komplexe Typen müssen vom Benutzer mit einem anderen Verfahren wie etwa JSON serialisiert werden.</span><span class="sxs-lookup"><span data-stu-id="d5347-275">Complex types must be serialized by the user using another mechanism, such as JSON.</span></span>

<span data-ttu-id="d5347-276">Fügen Sie die folgenden Erweiterungsmethoden hinzu, um serialisierbare Objekte für eine Sitzung festzulegen und abzurufen:</span><span class="sxs-lookup"><span data-stu-id="d5347-276">Add the following extension methods to set and get serializable objects:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](app-state/samples/2.x/SessionSample/Extensions/SessionExtensions.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](app-state/samples/1.x/SessionSample/Extensions/SessionExtensions.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="d5347-277">Im folgenden Beispiel wird dargestellt, wie Sie serialisierbare Objekte mit den Erweiterungsmethoden festlegen und abrufen:</span><span class="sxs-lookup"><span data-stu-id="d5347-277">The following example shows how to set and get a serializable object with the extension methods:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](app-state/samples/2.x/SessionSample/Pages/Index.cshtml.cs?name=snippet2)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](app-state/samples/1.x/SessionSample/Controllers/HomeController.cs?name=snippet2&highlight=4,12)]

::: moniker-end

## <a name="tempdata"></a><span data-ttu-id="d5347-278">TempData</span><span class="sxs-lookup"><span data-stu-id="d5347-278">TempData</span></span>

<span data-ttu-id="d5347-279">ASP.NET Core macht [die TempData-Eigenschaft des Razor Pages-Seitenmodells](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel.tempdata) oder [TempData eines MVC-Controllers](/dotnet/api/microsoft.aspnetcore.mvc.controller.tempdata) verfügbar.</span><span class="sxs-lookup"><span data-stu-id="d5347-279">ASP.NET Core exposes the [TempData property of a Razor Pages page model](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel.tempdata) or [TempData of an MVC controller](/dotnet/api/microsoft.aspnetcore.mvc.controller.tempdata).</span></span> <span data-ttu-id="d5347-280">Diese Eigenschaft speichert Daten, bis sie gelesen wurden.</span><span class="sxs-lookup"><span data-stu-id="d5347-280">This property stores data until it's read.</span></span> <span data-ttu-id="d5347-281">Die Methoden [Keep](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.itempdatadictionary.keep) und [Peek](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.itempdatadictionary.peek) können verwendet werden, um die Daten zu überprüfen, ohne sie zu löschen.</span><span class="sxs-lookup"><span data-stu-id="d5347-281">The [Keep](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.itempdatadictionary.keep) and [Peek](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.itempdatadictionary.peek) methods can be used to examine the data without deletion.</span></span> <span data-ttu-id="d5347-282">TempData eignet sich besonders für die Weiterleitung, wenn Daten für mehr als eine Anforderung erforderlich sind.</span><span class="sxs-lookup"><span data-stu-id="d5347-282">TempData is particularly useful for redirection when data is required for more than a single request.</span></span> <span data-ttu-id="d5347-283">TempData wird von TempData-Anbietern implementiert, indem Cookies oder der Sitzungszustand verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="d5347-283">TempData is implemented by TempData providers using either cookies or session state.</span></span>

### <a name="tempdata-providers"></a><span data-ttu-id="d5347-284">TempData-Anbieter</span><span class="sxs-lookup"><span data-stu-id="d5347-284">TempData providers</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="d5347-285">In ASP.NET Core 2.0 und höher wird der cookiebasierte TempData-Anbieter standardmäßig verwendet, um TempData in Cookies zu speichern.</span><span class="sxs-lookup"><span data-stu-id="d5347-285">In ASP.NET Core 2.0 or later, the cookie-based TempData provider is used by default to store TempData in cookies.</span></span>

<span data-ttu-id="d5347-286">Die Cookiedaten werden mit [IDataProtector](/dotnet/api/microsoft.aspnetcore.dataprotection.idataprotector) verschlüsselt, mit [Base64UrlTextEncoder](/dotnet/api/microsoft.aspnetcore.webutilities.base64urltextencoder) codiert und anschließend in Blöcke unterteilt.</span><span class="sxs-lookup"><span data-stu-id="d5347-286">The cookie data is encrypted using [IDataProtector](/dotnet/api/microsoft.aspnetcore.dataprotection.idataprotector), encoded with [Base64UrlTextEncoder](/dotnet/api/microsoft.aspnetcore.webutilities.base64urltextencoder), then chunked.</span></span> <span data-ttu-id="d5347-287">Da das Cookie in Blöcke unterteilt wird, gilt die in ASP.NET Core 1.x gefundene Größenbeschränkung nicht für einzelne Cookies.</span><span class="sxs-lookup"><span data-stu-id="d5347-287">Because the cookie is chunked, the single cookie size limit found in ASP.NET Core 1.x doesn't apply.</span></span> <span data-ttu-id="d5347-288">Die Cookiedaten werden nicht komprimiert, da es zu Sicherheitsproblemen kommen kann, wenn Daten verschlüsselt werden, z.B. durch [CRIME](https://wikipedia.org/wiki/CRIME_(security_exploit))- und [BREACH](https://wikipedia.org/wiki/BREACH_(security_exploit))-Angriffe.</span><span class="sxs-lookup"><span data-stu-id="d5347-288">The cookie data isn't compressed because compressing encrypted data can lead to security problems such as the [CRIME](https://wikipedia.org/wiki/CRIME_(security_exploit)) and [BREACH](https://wikipedia.org/wiki/BREACH_(security_exploit)) attacks.</span></span> <span data-ttu-id="d5347-289">Weitere Informationen zum cookiebasierten TempData-Anbieter finden Sie unter [CookieTempDataProvider](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.cookietempdataprovider).</span><span class="sxs-lookup"><span data-stu-id="d5347-289">For more information on the cookie-based TempData provider, see [CookieTempDataProvider](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.cookietempdataprovider).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="d5347-290">In ASP.NET Core 1.0 und 1.1 wird der TempData-Anbieter für den Sitzungszustand als Standardanbieter verwendet.</span><span class="sxs-lookup"><span data-stu-id="d5347-290">In ASP.NET Core 1.0 and 1.1, the session state TempData provider is the default provider.</span></span>

::: moniker-end

### <a name="choose-a-tempdata-provider"></a><span data-ttu-id="d5347-291">Auswählen eines TempData-Anbieters</span><span class="sxs-lookup"><span data-stu-id="d5347-291">Choose a TempData provider</span></span>

<span data-ttu-id="d5347-292">Bevor Sie einen TempData-Anbieter auswählen, müssen Sie folgende Überlegungen anstellen:</span><span class="sxs-lookup"><span data-stu-id="d5347-292">Choosing a TempData provider involves several considerations, such as:</span></span>

1. <span data-ttu-id="d5347-293">Verwendet die App bereits den Sitzungszustand?</span><span class="sxs-lookup"><span data-stu-id="d5347-293">Does the app already use session state?</span></span> <span data-ttu-id="d5347-294">Falls dies der Fall ist, hat die Verwendung des TempData-Anbieters abgesehen von der Größe der Daten keine zusätzlichen Auswirkungen auf die App.</span><span class="sxs-lookup"><span data-stu-id="d5347-294">If so, using the session state TempData provider has no additional cost to the app (aside from the size of the data).</span></span>
2. <span data-ttu-id="d5347-295">Verwendet die App TempData nur selten für verhältnismäßig kleine Datenmengen (bis zu 500 Bytes)?</span><span class="sxs-lookup"><span data-stu-id="d5347-295">Does the app use TempData only sparingly for relatively small amounts of data (up to 500 bytes)?</span></span> <span data-ttu-id="d5347-296">Falls dies der Fall ist, entsteht durch das Cookie des TempData-Anbieters nur ein kleiner zusätzlicher Aufwand für jede Anforderung, die TempData enthält.</span><span class="sxs-lookup"><span data-stu-id="d5347-296">If so, the cookie TempData provider adds a small cost to each request that carries TempData.</span></span> <span data-ttu-id="d5347-297">Falls dies nicht der Fall ist, kann der Sitzungszustand des TempData-Anbieters nützlich sein, um Roundtrips für große Datenmengen für jede Anforderung durchzuführen, bis TempData verarbeitet wird.</span><span class="sxs-lookup"><span data-stu-id="d5347-297">If not, the session state TempData provider can be beneficial to avoid round-tripping a large amount of data in each request until the TempData is consumed.</span></span>
3. <span data-ttu-id="d5347-298">Wird die App in einer Serverfarm auf mehreren Servern ausgeführt?</span><span class="sxs-lookup"><span data-stu-id="d5347-298">Does the app run in a server farm on multiple servers?</span></span> <span data-ttu-id="d5347-299">Wenn dies der Fall ist, ist keine weitere Konfiguration erforderlich, um das Cookie des TempData-Anbieters außerhalb des Schutzes von Daten zu verwenden (siehe <xref:security/data-protection/introduction> und [Schlüsselspeicheranbieter](xref:security/data-protection/implementation/key-storage-providers)).</span><span class="sxs-lookup"><span data-stu-id="d5347-299">If so, there's no additional configuration required to use the cookie TempData provider outside of Data Protection (see <xref:security/data-protection/introduction> and [Key storage providers](xref:security/data-protection/implementation/key-storage-providers)).</span></span>

> [!NOTE]
> <span data-ttu-id="d5347-300">Die meisten Webclients (z.B. Webbrowser) erzwingen Einschränkungen für die maximale Größe der Cookies, für die Gesamtanzahl der Cookies oder für beides.</span><span class="sxs-lookup"><span data-stu-id="d5347-300">Most web clients (such as web browsers) enforce limits on the maximum size of each cookie, the total number of cookies, or both.</span></span> <span data-ttu-id="d5347-301">Wenn Sie das Cookie des TempData-Anbieters verwenden, sollten Sie überprüfen, ob die App diese Einschränkungen überschreitet.</span><span class="sxs-lookup"><span data-stu-id="d5347-301">When using the cookie TempData provider, verify the app won't exceed these limits.</span></span> <span data-ttu-id="d5347-302">Beachten Sie die Gesamtgröße der Daten.</span><span class="sxs-lookup"><span data-stu-id="d5347-302">Consider the total size of the data.</span></span> <span data-ttu-id="d5347-303">Rechnen Sie mit erhöhter Cookiegröße aufgrund von Verschlüsselung und Segmentierung.</span><span class="sxs-lookup"><span data-stu-id="d5347-303">Account for increases in cookie size due to encryption and chunking.</span></span>

### <a name="configure-the-tempdata-provider"></a><span data-ttu-id="d5347-304">Konfigurieren des TempData-Anbieters</span><span class="sxs-lookup"><span data-stu-id="d5347-304">Configure the TempData provider</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="d5347-305">Der cookiebasierte TempData-Anbieter ist standardmäßig aktiviert.</span><span class="sxs-lookup"><span data-stu-id="d5347-305">The cookie-based TempData provider is enabled by default.</span></span>

<span data-ttu-id="d5347-306">Verwenden Sie die Erweiterungsmethode [AddSessionStateTempDataProvider](/dotnet/api/microsoft.extensions.dependencyinjection.mvcviewfeaturesmvcbuilderextensions.addsessionstatetempdataprovider), um sitzungsbasierte TempData-Anbieter zu ermöglichen:</span><span class="sxs-lookup"><span data-stu-id="d5347-306">To enable the session-based TempData provider, use the [AddSessionStateTempDataProvider](/dotnet/api/microsoft.extensions.dependencyinjection.mvcviewfeaturesmvcbuilderextensions.addsessionstatetempdataprovider) extension method:</span></span>

[!code-csharp[](app-state/samples_snapshot_2/2.x/SessionSample/Startup.cs?name=snippet1&highlight=11,13,32)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="d5347-307">Der folgende `Startup`-Klassencode konfiguriert den sitzungsbasierten TempData-Anbieter:</span><span class="sxs-lookup"><span data-stu-id="d5347-307">The following `Startup` class code configures the session-based TempData provider:</span></span>

[!code-csharp[](app-state/samples_snapshot_2/1.x/SessionSample/Startup.cs?name=snippet1&highlight=4,9)]

::: moniker-end

<span data-ttu-id="d5347-308">Die Reihenfolge der Middleware ist wichtig.</span><span class="sxs-lookup"><span data-stu-id="d5347-308">The order of middleware is important.</span></span> <span data-ttu-id="d5347-309">Im vorherigen Beispiel kommt es zu einer `InvalidOperationException`-Ausnahme, wenn `UseSession` nach `UseMvc` aufgerufen wird.</span><span class="sxs-lookup"><span data-stu-id="d5347-309">In the preceding example, an `InvalidOperationException` exception occurs when `UseSession` is invoked after `UseMvc`.</span></span> <span data-ttu-id="d5347-310">Weitere Informationen finden Sie unter [Middleware](xref:fundamentals/middleware/index#order).</span><span class="sxs-lookup"><span data-stu-id="d5347-310">For more information, see [Middleware Ordering](xref:fundamentals/middleware/index#order).</span></span>

> [!IMPORTANT]
> <span data-ttu-id="d5347-311">Wenn Sie für .NET Framework entwickeln und den sitzungsbasierten TempData-Anbieter verwenden, fügen Sie das Paket [Microsoft.AspNetCore.Session](https://www.nuget.org/packages/Microsoft.AspNetCore.Session/) zu Ihrem Projekt hinzu.</span><span class="sxs-lookup"><span data-stu-id="d5347-311">If targeting .NET Framework and using the session-based TempData provider, add the [Microsoft.AspNetCore.Session](https://www.nuget.org/packages/Microsoft.AspNetCore.Session/) package to the project.</span></span>

## <a name="query-strings"></a><span data-ttu-id="d5347-312">Abfragezeichenfolgen</span><span class="sxs-lookup"><span data-stu-id="d5347-312">Query strings</span></span>

<span data-ttu-id="d5347-313">Eine begrenzte Menge an Daten kann von einer Anforderung an eine andere übergeben werden, indem Sie diese zu der Abfragezeichenfolge einer neuen Anforderung hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="d5347-313">A limited amount of data can be passed from one request to another by adding it to the new request's query string.</span></span> <span data-ttu-id="d5347-314">Dies ist nützlich, um den Zustand dauerhaft zu erfassen und Links mit einem eingebetteten Zustand zuzulassen, die über E-Mail oder soziale Netzwerke geteilt werden sollen.</span><span class="sxs-lookup"><span data-stu-id="d5347-314">This is useful for capturing state in a persistent manner that allows links with embedded state to be shared through email or social networks.</span></span> <span data-ttu-id="d5347-315">Verwenden Sie keine Abfragezeichenfolgen für vertrauliche Daten, da URL-Abfragezeichenfolgen öffentlich sind.</span><span class="sxs-lookup"><span data-stu-id="d5347-315">Because URL query strings are public, never use query strings for sensitive data.</span></span>

<span data-ttu-id="d5347-316">Wenn Daten in Abfragezeichenfolgen eingefügt werden, können sie nicht nur versehentlich freigegeben werden, sondern es entstehen auch Gelegenheiten für Versuche einer [websiteübergreifenden Anforderungsfälschung](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF)), wodurch Benutzer dazu gebracht werden können, auf bösartige Websites zuzugreifen, während sie authentifiziert sind.</span><span class="sxs-lookup"><span data-stu-id="d5347-316">In addition to unintended sharing, including data in query strings can create opportunities for [Cross-Site Request Forgery (CSRF)](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF)) attacks, which can trick users into visiting malicious sites while authenticated.</span></span> <span data-ttu-id="d5347-317">Angreifer können dann Benutzerdaten von der App stehlen oder schädliche Aktionen im Namen eines Benutzers durchführen.</span><span class="sxs-lookup"><span data-stu-id="d5347-317">Attackers can then steal user data from the app or take malicious actions on behalf of the user.</span></span> <span data-ttu-id="d5347-318">Jeder gespeicherte App- oder Sitzungszustand muss vor solchen Anforderungsfälschungen geschützt sein.</span><span class="sxs-lookup"><span data-stu-id="d5347-318">Any preserved app or session state must protect against CSRF attacks.</span></span> <span data-ttu-id="d5347-319">Weitere Informationen finden Sie unter [Preventing Cross-Site Request Forgery (XSRF/CSRF) attacks (Verhindern von websiteübergreifenden Anforderungsfälschungen (XSRF/CSRF))](xref:security/anti-request-forgery).</span><span class="sxs-lookup"><span data-stu-id="d5347-319">For more information, see [Prevent Cross-Site Request Forgery (XSRF/CSRF) attacks](xref:security/anti-request-forgery).</span></span>

## <a name="hidden-fields"></a><span data-ttu-id="d5347-320">Verborgene Felder</span><span class="sxs-lookup"><span data-stu-id="d5347-320">Hidden fields</span></span>

<span data-ttu-id="d5347-321">Daten können in ausgeblendeten Formularfeldern gespeichert und an die nächste Anforderung zurückgesendet werden.</span><span class="sxs-lookup"><span data-stu-id="d5347-321">Data can be saved in hidden form fields and posted back on the next request.</span></span> <span data-ttu-id="d5347-322">Dies ist häufig in mehrseitigen Formularen der Fall.</span><span class="sxs-lookup"><span data-stu-id="d5347-322">This is common in multi-page forms.</span></span> <span data-ttu-id="d5347-323">Die App muss die in verborgenen Feldern gespeicherten Daten immer wieder überprüfen, da der Client die Daten manipulieren könnte.</span><span class="sxs-lookup"><span data-stu-id="d5347-323">Because the client can potentially tamper with the data, the app must always revalidate the data stored in hidden fields.</span></span>

## <a name="httpcontextitems"></a><span data-ttu-id="d5347-324">HttpContext.Items</span><span class="sxs-lookup"><span data-stu-id="d5347-324">HttpContext.Items</span></span>

<span data-ttu-id="d5347-325">Die Auflistung [HttpContext.Items](/dotnet/api/microsoft.aspnetcore.http.httpcontext.items) wird verwendet, um Daten zu speichern, während eine einzelne Anforderung behandelt wird.</span><span class="sxs-lookup"><span data-stu-id="d5347-325">The [HttpContext.Items](/dotnet/api/microsoft.aspnetcore.http.httpcontext.items) collection is used to store data while processing a single request.</span></span> <span data-ttu-id="d5347-326">Die Inhalte der Auflistung werden nach der Verarbeitung jeder Anforderung verworfen.</span><span class="sxs-lookup"><span data-stu-id="d5347-326">The collection's contents are discarded after a request is processed.</span></span> <span data-ttu-id="d5347-327">Die `Items`-Auflistung wird häufig von Komponenten oder Middleware zur Kommunikation verwendet, wenn sie zu unterschiedlichen Zeitpunkten während einer Anforderung ausgeführt werden und es keinen direkten Weg gibt, Parameter zu übergeben.</span><span class="sxs-lookup"><span data-stu-id="d5347-327">The `Items` collection is often used to allow components or middleware to communicate when they operate at different points in time during a request and have no direct way to pass parameters.</span></span>

<span data-ttu-id="d5347-328">Im folgenden Beispiel fügt [Middleware](xref:fundamentals/middleware/index) `isVerified` zur Auflistung `Items` hinzu.</span><span class="sxs-lookup"><span data-stu-id="d5347-328">In the following example, [middleware](xref:fundamentals/middleware/index) adds `isVerified` to the `Items` collection.</span></span>

```csharp
app.Use(async (context, next) =>
{
    // perform some verification
    context.Items["isVerified"] = true;
    await next.Invoke();
});
```

<span data-ttu-id="d5347-329">An einem späteren Punkt in der Pipeline kann eine andere Middleware den Wert von `isVerified` zugreifen:</span><span class="sxs-lookup"><span data-stu-id="d5347-329">Later in the pipeline, another middleware can access the value of `isVerified`:</span></span>

```csharp
app.Run(async (context) =>
{
    await context.Response.WriteAsync($"Verified: {context.Items["isVerified"]}");
});
```

<span data-ttu-id="d5347-330">Für Middleware, die nur von einer App verwendet wird, werden `string`-Schlüssel akzeptiert.</span><span class="sxs-lookup"><span data-stu-id="d5347-330">For middleware that's only used by a single app, `string` keys are acceptable.</span></span> <span data-ttu-id="d5347-331">Middleware, die von mehreren App-Instanzen verwendet wird, sollte eindeutige Objektschlüssel verwenden, um Schlüsselkonflikte zu vermeiden.</span><span class="sxs-lookup"><span data-stu-id="d5347-331">Middleware shared between app instances should use unique object keys to avoid key collisions.</span></span> <span data-ttu-id="d5347-332">Das folgenden Beispiel zeigt, wie Sie einen eindeutigen Objektschlüssel verwenden, der in einer Middlewareklasse definiert wurde:</span><span class="sxs-lookup"><span data-stu-id="d5347-332">The following example shows how to use a unique object key defined in a middleware class:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](app-state/samples/2.x/SessionSample/Middleware/HttpContextItemsMiddleware.cs?name=snippet1&highlight=4,13)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](app-state/samples/1.x/SessionSample/Middleware/HttpContextItemsMiddleware.cs?name=snippet1&highlight=5,14)]

::: moniker-end

<span data-ttu-id="d5347-333">Anderer Code kann auf den Wert zugreifen, der in `HttpContext.Items` gespeichert ist, indem er den Schlüssel verwendet, der von der Middlewareklasse zur Verfügung gestellt wird:</span><span class="sxs-lookup"><span data-stu-id="d5347-333">Other code can access the value stored in `HttpContext.Items` using the key exposed by the middleware class:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](app-state/samples/2.x/SessionSample/Pages/Index.cshtml.cs?name=snippet3)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](app-state/samples/1.x/SessionSample/Controllers/HomeController.cs?name=snippet3)]

::: moniker-end

<span data-ttu-id="d5347-334">Dieser Ansatz hat außerdem den Vorteil, dass das Verwenden von Schlüsselzeichenfolgen im Code vermieden wird.</span><span class="sxs-lookup"><span data-stu-id="d5347-334">This approach also has the advantage of eliminating the use of key strings in the code.</span></span>

## <a name="cache"></a><span data-ttu-id="d5347-335">cache</span><span class="sxs-lookup"><span data-stu-id="d5347-335">Cache</span></span>

<span data-ttu-id="d5347-336">Das Caching stellt eine effiziente Möglichkeit zum Speichern und Abrufen von Daten dar.</span><span class="sxs-lookup"><span data-stu-id="d5347-336">Caching is an efficient way to store and retrieve data.</span></span> <span data-ttu-id="d5347-337">Die App kann die Lebensdauer zwischengespeicherter Elemente bestimmen.</span><span class="sxs-lookup"><span data-stu-id="d5347-337">The app can control the lifetime of cached items.</span></span>

<span data-ttu-id="d5347-338">Zwischengespeicherte Daten sind nicht mit einer spezifischen Anforderung, einem spezifischen Benutzer oder einer spezifischen Sitzung verknüpft.</span><span class="sxs-lookup"><span data-stu-id="d5347-338">Cached data isn't associated with a specific request, user, or session.</span></span> <span data-ttu-id="d5347-339">**Achten Sie darauf, dass Sie keine benutzerspezifischen Daten zwischenspeichern, die von den Anforderungen anderen Benutzer abgerufen werden können.**</span><span class="sxs-lookup"><span data-stu-id="d5347-339">**Be careful not to cache user-specific data that may be retrieved by other users' requests.**</span></span>

<span data-ttu-id="d5347-340">Weitere Informationen finden Sie im Artikel zu [Cacheantworten](xref:performance/caching/index).</span><span class="sxs-lookup"><span data-stu-id="d5347-340">For more information, see the [Cache responses](xref:performance/caching/index) topic.</span></span>

## <a name="dependency-injection"></a><span data-ttu-id="d5347-341">Dependency Injection</span><span class="sxs-lookup"><span data-stu-id="d5347-341">Dependency Injection</span></span>

<span data-ttu-id="d5347-342">Verwenden Sie [Dependency Injection](xref:fundamentals/dependency-injection), um Daten für Benutzer bereitzustellen:</span><span class="sxs-lookup"><span data-stu-id="d5347-342">Use [Dependency Injection](xref:fundamentals/dependency-injection) to make data available to all users:</span></span>

1. <span data-ttu-id="d5347-343">Definieren Sie einen Dienst, der die Daten enthält.</span><span class="sxs-lookup"><span data-stu-id="d5347-343">Define a service containing the data.</span></span> <span data-ttu-id="d5347-344">Definieren Sie z.B. die Klasse `MyAppData`:</span><span class="sxs-lookup"><span data-stu-id="d5347-344">For example, a class named `MyAppData` is defined:</span></span>

    ```csharp
    public class MyAppData
    {
        // Declare properties and methods
    }
    ```

2. <span data-ttu-id="d5347-345">Fügen Sie die Dienstklasse zu `Startup.ConfigureServices` hinzu:</span><span class="sxs-lookup"><span data-stu-id="d5347-345">Add the service class to `Startup.ConfigureServices`:</span></span>

    ```csharp
    public void ConfigureServices(IServiceCollection services)
    {
        services.AddSingleton<MyAppData>();
    }
    ```

3. <span data-ttu-id="d5347-346">Verwenden Sie die Datendienstklasse:</span><span class="sxs-lookup"><span data-stu-id="d5347-346">Consume the data service class:</span></span>

    ::: moniker range=">= aspnetcore-2.0"

    ```csharp
    public class IndexModel : PageModel
    {
        public IndexModel(MyAppData myService)
        {
            // Do something with the service
            //    Examples: Read data, store in a field or property
        }
    }
    ```

    ::: moniker-end

    ::: moniker range="< aspnetcore-2.0"

    ```csharp
    public class HomeController : Controller
    {
        public HomeController(MyAppData myService)
        {
            // Do something with the service
            //    Examples: Read data, store in a field or property
        }
    }
    ```

    ::: moniker-end

## <a name="common-errors"></a><span data-ttu-id="d5347-347">Häufige Fehler</span><span class="sxs-lookup"><span data-stu-id="d5347-347">Common errors</span></span>

* <span data-ttu-id="d5347-348">„Unable to resolve service for type 'Microsoft.Extensions.Caching.Distributed.IDistributedCache' while attempting to activate 'Microsoft.AspNetCore.Session.DistributedSessionStore'. (Dienst kann nicht für den Typ „Microsoft.Extensions.Caching.Distributed.IDistributedCache“ aufgelöst werden, während versucht wird, auf „Microsoft.AspNetCore.Session.DistributedSessionStore“ zuzugreifen.)</span><span class="sxs-lookup"><span data-stu-id="d5347-348">"Unable to resolve service for type 'Microsoft.Extensions.Caching.Distributed.IDistributedCache' while attempting to activate 'Microsoft.AspNetCore.Session.DistributedSessionStore'."</span></span>

  <span data-ttu-id="d5347-349">Dieser Fehler entsteht in der Regel, wenn nicht alle `IDistributedCache`-Implementierungen konfiguriert werden.</span><span class="sxs-lookup"><span data-stu-id="d5347-349">This is usually caused by failing to configure at least one `IDistributedCache` implementation.</span></span> <span data-ttu-id="d5347-350">Weitere Informationen finden Sie unter <xref:performance/caching/distributed> und <xref:performance/caching/memory>.</span><span class="sxs-lookup"><span data-stu-id="d5347-350">For more information, see <xref:performance/caching/distributed> and <xref:performance/caching/memory>.</span></span>

* <span data-ttu-id="d5347-351">Falls die Sitzungsmiddleware eine Sitzung nicht speichert (z.B. wenn der Sicherungsspeicher nicht verfügbar ist), protokolliert sie die Ausnahme, und die Anforderung wird ordnungsgemäß ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="d5347-351">In the event that the session middleware fails to persist a session (for example, if the backing store isn't available), the middleware logs the exception and the request continues normally.</span></span> <span data-ttu-id="d5347-352">Das führt zu unvorhersehbarem Verhalten.</span><span class="sxs-lookup"><span data-stu-id="d5347-352">This leads to unpredictable behavior.</span></span>

  <span data-ttu-id="d5347-353">Nehmen wir an, dass ein Benutzer seinen Einkaufswagen in einer Sitzung speichert.</span><span class="sxs-lookup"><span data-stu-id="d5347-353">For example, a user stores a shopping cart in session.</span></span> <span data-ttu-id="d5347-354">Der Benutzer fügt ein Element zum Einkaufswagen hinzu, aber der Commit schlägt fehl.</span><span class="sxs-lookup"><span data-stu-id="d5347-354">The user adds an item to the cart but the commit fails.</span></span> <span data-ttu-id="d5347-355">Die App wird nicht über den Fehler informiert und meldet dem Benutzer, dass das Element zum Einkaufswagen hinzugefügt wurde. Dies stimmt jedoch nicht.</span><span class="sxs-lookup"><span data-stu-id="d5347-355">The app doesn't know about the failure so it reports to the user that the item was added to their cart, which isn't true.</span></span>

  <span data-ttu-id="d5347-356">Es wird empfohlen, nach Fehlern zu suchen, indem Sie `await feature.Session.CommitAsync();` über App-Code aufrufen, wenn die App mit dem Schreiben in die Sitzung fertig ist.</span><span class="sxs-lookup"><span data-stu-id="d5347-356">The recommended approach to check for errors is to call `await feature.Session.CommitAsync();` from app code when the app is done writing to the session.</span></span> <span data-ttu-id="d5347-357">`CommitAsync` löst eine Ausnahme aus, wenn der Sicherungsspeicher nicht verfügbar ist.</span><span class="sxs-lookup"><span data-stu-id="d5347-357">`CommitAsync` throws an exception if the backing store is unavailable.</span></span> <span data-ttu-id="d5347-358">Wenn `CommitAsync` fehlschlägt, kann die App die Ausnahme verarbeiten.</span><span class="sxs-lookup"><span data-stu-id="d5347-358">If `CommitAsync` fails, the app can process the exception.</span></span> <span data-ttu-id="d5347-359">`LoadAsync` wird unter den gleichen Bedingungen ausgelöst, wenn der Sicherungsspeicher nicht verfügbar ist.</span><span class="sxs-lookup"><span data-stu-id="d5347-359">`LoadAsync` throws under the same conditions where the data store is unavailable.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="d5347-360">Zusätzliche Ressourcen</span><span class="sxs-lookup"><span data-stu-id="d5347-360">Additional resources</span></span>

<xref:host-and-deploy/web-farm>
