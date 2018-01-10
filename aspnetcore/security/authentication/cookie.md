---
title: "Mithilfe der Cookieauthentifizierung ohne ASP.NET Core Identität"
author: rick-anderson
description: "Eine Erläuterung der Verwendung von Cookieauthentifizierung ohne ASP.NET Core Identity"
keywords: ASP.NET Core, cookies
ms.author: riande
manager: wpickett
ms.date: 10/11/2017
ms.topic: article
ms.assetid: 2bdcbf95-8d9d-4537-a4a0-a5ee439dcb62
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authentication/cookie
ms.openlocfilehash: ee660667251ec4a64f2b3e83f39214e9defcea03
ms.sourcegitcommit: 2d23ea501e0213bbacf65298acf1c8bd17209540
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/09/2018
---
# <a name="using-cookie-authentication-without-aspnet-core-identity"></a><span data-ttu-id="e643a-104">Mithilfe der Cookieauthentifizierung ohne ASP.NET Core Identität</span><span class="sxs-lookup"><span data-stu-id="e643a-104">Using Cookie Authentication without ASP.NET Core Identity</span></span>

<span data-ttu-id="e643a-105">Durch [Rick Anderson](https://twitter.com/RickAndMSFT) und [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="e643a-105">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="e643a-106">Wie Sie in den früheren Authentifizierungsthemen gesehen haben [ASP.NET Core Identity](xref:security/authentication/identity) ist ein vollständige, voll ausgestatteten Authentifizierungsanbieter zum Erstellen und Verwalten von Anmeldungen.</span><span class="sxs-lookup"><span data-stu-id="e643a-106">As you've seen in the earlier authentication topics, [ASP.NET Core Identity](xref:security/authentication/identity) is a complete, full-featured authentication provider for creating and maintaining logins.</span></span> <span data-ttu-id="e643a-107">Allerdings sollten Sie Ihre eigenen benutzerdefinierten Authentifizierungslogik mit Zeiten Cookie-basierte Authentifizierung zu verwenden.</span><span class="sxs-lookup"><span data-stu-id="e643a-107">However, you may want to use your own custom authentication logic with cookie-based authentication at times.</span></span> <span data-ttu-id="e643a-108">Sie können als eigenständige Authentifizierungsanbieter ohne ASP.NET Core Identity Cookie-basierte Authentifizierung verwenden.</span><span class="sxs-lookup"><span data-stu-id="e643a-108">You can use cookie-based authentication as a standalone authentication provider without ASP.NET Core Identity.</span></span>

<span data-ttu-id="e643a-109">[Anzeigen oder Herunterladen von Beispielcode](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authentication/cookie/sample) ([Vorgehensweise zum Herunterladen](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="e643a-109">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authentication/cookie/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="e643a-110">Informationen zum Migrieren von Cookie-basierte Authentifizierung von ASP.NET Core 1.x auf 2.0 finden Sie unter [Migrieren von Authentifizierung und Identität zu ASP.NET Core 2.0 Thema (Cookie-basierte Authentifizierung)](xref:migration/1x-to-2x/identity-2x#cookie-based-authentication).</span><span class="sxs-lookup"><span data-stu-id="e643a-110">For information on migrating cookie-based authentication from ASP.NET Core 1.x to 2.0, see [Migrating Authentication and Identity to ASP.NET Core 2.0 topic (Cookie-based Authentication)](xref:migration/1x-to-2x/identity-2x#cookie-based-authentication).</span></span>

## <a name="configuration"></a><span data-ttu-id="e643a-111">Konfiguration</span><span class="sxs-lookup"><span data-stu-id="e643a-111">Configuration</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="e643a-112">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="e643a-112">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="e643a-113">Wenn Sie nicht die [Microsoft.AspNetCore.All Metapackage](xref:fundamentals/metapackage), installieren Sie Version 2.0 + von der [Microsoft.AspNetCore.Authentication.Cookies](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Cookies/) NuGet-Paket.</span><span class="sxs-lookup"><span data-stu-id="e643a-113">If you aren't using the [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage), install version 2.0+ of the [Microsoft.AspNetCore.Authentication.Cookies](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Cookies/) NuGet package.</span></span>

<span data-ttu-id="e643a-114">In der `ConfigureServices` -Methode den Authentifizierung-Middleware-Dienst mit dem Erstellen der `AddAuthentication` und `AddCookie` Methoden:</span><span class="sxs-lookup"><span data-stu-id="e643a-114">In the `ConfigureServices` method, create the Authentication Middleware service with the `AddAuthentication` and `AddCookie` methods:</span></span>

[!code-csharp[Main](cookie/sample/Startup.cs?name=snippet1)]

<span data-ttu-id="e643a-115">`AuthenticationScheme`übergeben zum `AddAuthentication` legt das Standard-Authentifizierungsschema für die app.</span><span class="sxs-lookup"><span data-stu-id="e643a-115">`AuthenticationScheme` passed to `AddAuthentication` sets the default authentication scheme for the app.</span></span> <span data-ttu-id="e643a-116">`AuthenticationScheme`ist nützlich, wenn mehrere Instanzen der Cookieauthentifizierung vorhanden sind und Sie möchten [mit einem bestimmten Schema autorisieren](xref:security/authorization/limitingidentitybyscheme).</span><span class="sxs-lookup"><span data-stu-id="e643a-116">`AuthenticationScheme` is useful when there are multiple instances of cookie authentication and you want to [authorize with a specific scheme](xref:security/authorization/limitingidentitybyscheme).</span></span> <span data-ttu-id="e643a-117">Festlegen der `AuthenticationScheme` zu `CookieAuthenticationDefaults.AuthenticationScheme` "Cookies" der Wert für das Schema enthält.</span><span class="sxs-lookup"><span data-stu-id="e643a-117">Setting the `AuthenticationScheme` to `CookieAuthenticationDefaults.AuthenticationScheme` provides a value of "Cookies" for the scheme.</span></span> <span data-ttu-id="e643a-118">Sie können einen beliebigen Zeichenfolgenwert angeben, der das Schema unterscheidet.</span><span class="sxs-lookup"><span data-stu-id="e643a-118">You can supply any string value that distinguishes the scheme.</span></span>

<span data-ttu-id="e643a-119">In der `Configure` -Methode, mit der `UseAuthentication` Authentifizierung-Middleware aufgerufen werden soll, der festlegt der `HttpContext.User` Eigenschaft.</span><span class="sxs-lookup"><span data-stu-id="e643a-119">In the `Configure` method, use the `UseAuthentication` method to invoke the Authentication Middleware that sets the `HttpContext.User` property.</span></span> <span data-ttu-id="e643a-120">Rufen Sie die `UseAuthentication` Methode vor dem Aufruf `UseMvcWithDefaultRoute` oder `UseMvc`:</span><span class="sxs-lookup"><span data-stu-id="e643a-120">Call the `UseAuthentication` method before calling `UseMvcWithDefaultRoute` or `UseMvc`:</span></span>

[!code-csharp[Main](cookie/sample/Startup.cs?name=snippet2)]

<span data-ttu-id="e643a-121">**AddCookie-Optionen**</span><span class="sxs-lookup"><span data-stu-id="e643a-121">**AddCookie Options**</span></span>

<span data-ttu-id="e643a-122">Die [CookieAuthenticationOptions](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions?view=aspnetcore-2.0) Klasse wird verwendet, um die Anbieteroptionen für die Authentifizierung zu konfigurieren.</span><span class="sxs-lookup"><span data-stu-id="e643a-122">The [CookieAuthenticationOptions](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions?view=aspnetcore-2.0) class is used to configure the authentication provider options.</span></span>

| <span data-ttu-id="e643a-123">Option</span><span class="sxs-lookup"><span data-stu-id="e643a-123">Option</span></span> | <span data-ttu-id="e643a-124">Beschreibung</span><span class="sxs-lookup"><span data-stu-id="e643a-124">Description</span></span> |
| ------ | ----------- |
| [<span data-ttu-id="e643a-125">AccessDeniedPath</span><span class="sxs-lookup"><span data-stu-id="e643a-125">AccessDeniedPath</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.accessdeniedpath?view=aspnetcore-2.0) | <span data-ttu-id="e643a-126">Gibt den Pfad zu einer 302 gefunden (URL-Umleitung) zur Verfügung stellen, wenn von ausgelöst `HttpContext.ForbidAsync`.</span><span class="sxs-lookup"><span data-stu-id="e643a-126">Provides the path to supply with a 302 Found (URL redirect) when triggered by `HttpContext.ForbidAsync`.</span></span> <span data-ttu-id="e643a-127">Der Standardwert ist `/Account/AccessDenied`.</span><span class="sxs-lookup"><span data-stu-id="e643a-127">The default value is `/Account/AccessDenied`.</span></span> |
| [<span data-ttu-id="e643a-128">ClaimsIssuer</span><span class="sxs-lookup"><span data-stu-id="e643a-128">ClaimsIssuer</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.authenticationschemeoptions.claimsissuer?view=aspnetcore-2.0) | <span data-ttu-id="e643a-129">Der Aussteller, verwenden Sie für die [Aussteller](/dotnet/api/system.security.claims.claim.issuer) Eigenschaft für Ansprüche, die vom Cookie Authentication-Dienst erstellt.</span><span class="sxs-lookup"><span data-stu-id="e643a-129">The issuer to use for the [Issuer](/dotnet/api/system.security.claims.claim.issuer) property on any claims created by the cookie authentication service.</span></span> |
| [<span data-ttu-id="e643a-130">Cookie.Domain</span><span class="sxs-lookup"><span data-stu-id="e643a-130">Cookie.Domain</span></span>](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.domain?view=aspnetcore-2.0) | <span data-ttu-id="e643a-131">Der Domänenname, in denen das Cookie bedient wird.</span><span class="sxs-lookup"><span data-stu-id="e643a-131">The domain name where the cookie is served.</span></span> <span data-ttu-id="e643a-132">Standardmäßig ist dies der Hostname der Anforderung.</span><span class="sxs-lookup"><span data-stu-id="e643a-132">By default, this is the host name of the request.</span></span> <span data-ttu-id="e643a-133">Der Browser sendet das Cookie nur bei den Anforderungen auf einen übereinstimmenden Hostnamen.</span><span class="sxs-lookup"><span data-stu-id="e643a-133">The browser only sends the cookie in requests to a matching host name.</span></span> <span data-ttu-id="e643a-134">Sie möchten möglicherweise passen Sie diese Option, um Cookies zu einem Host in Ihrer Domäne verfügen.</span><span class="sxs-lookup"><span data-stu-id="e643a-134">You may wish to adjust this to have cookies available to any host in your domain.</span></span> <span data-ttu-id="e643a-135">Z. B. Festlegen der Domäne des Cookies auf `.contoso.com` zur Verfügung `contoso.com`, `www.contoso.com`, und `staging.www.contoso.com`.</span><span class="sxs-lookup"><span data-stu-id="e643a-135">For example, setting the cookie domain to `.contoso.com` makes it available to `contoso.com`, `www.contoso.com`, and `staging.www.contoso.com`.</span></span> |
| [<span data-ttu-id="e643a-136">Cookie.Expiration</span><span class="sxs-lookup"><span data-stu-id="e643a-136">Cookie.Expiration</span></span>](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.expiration?view=aspnetcore-2.0) | <span data-ttu-id="e643a-137">Ruft ab oder legt die Lebensdauer von Cookies.</span><span class="sxs-lookup"><span data-stu-id="e643a-137">Gets or sets the lifespan of a cookie.</span></span> <span data-ttu-id="e643a-138">Aktuell, dies keine Ops option und veraltete Elemente in ASP.NET Core 2.1 + werden.</span><span class="sxs-lookup"><span data-stu-id="e643a-138">Currently, this option no-ops and will become obsolete in ASP.NET Core 2.1+.</span></span> <span data-ttu-id="e643a-139">Verwenden der `ExpireTimeSpan` Ablauf der Cookies festzulegende Option.</span><span class="sxs-lookup"><span data-stu-id="e643a-139">Use the `ExpireTimeSpan` option to set cookie expiration.</span></span> <span data-ttu-id="e643a-140">Weitere Informationen finden Sie unter [verdeutlichen Verhalten des CookieAuthenticationOptions.Cookie.Expiration](https://github.com/aspnet/Security/issues/1293).</span><span class="sxs-lookup"><span data-stu-id="e643a-140">For more information, see [Clarify behavior of CookieAuthenticationOptions.Cookie.Expiration](https://github.com/aspnet/Security/issues/1293).</span></span> |
| [<span data-ttu-id="e643a-141">Cookie.HttpOnly</span><span class="sxs-lookup"><span data-stu-id="e643a-141">Cookie.HttpOnly</span></span>](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.httponly?view=aspnetcore-2.0) | <span data-ttu-id="e643a-142">Ein Flag, das angibt, ob das Cookie nur an Server zugegriffen werden soll.</span><span class="sxs-lookup"><span data-stu-id="e643a-142">A flag indicating if the cookie should be accessible only to servers.</span></span> <span data-ttu-id="e643a-143">Durch Ändern dieses Werts auf `false` ermöglicht die clientseitige Skripts auf das Cookie zugreifen und möglicherweise Ihrer app das Cookie Diebstahl muss Ihrer app ein [siteübergreifendem Skripting (XSS)](xref:security/cross-site-scripting) Sicherheitsrisiko.</span><span class="sxs-lookup"><span data-stu-id="e643a-143">Changing this value to `false` permits client-side scripts to access the cookie and may open your app to cookie theft should your app have a [Cross-site scripting (XSS)](xref:security/cross-site-scripting) vulnerability.</span></span> <span data-ttu-id="e643a-144">Der Standardwert ist `true`.</span><span class="sxs-lookup"><span data-stu-id="e643a-144">The default value is `true`.</span></span> |
| [<span data-ttu-id="e643a-145">Cookie.Name</span><span class="sxs-lookup"><span data-stu-id="e643a-145">Cookie.Name</span></span>](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.name?view=aspnetcore-2.0) | <span data-ttu-id="e643a-146">Legt den Namen des Cookies.</span><span class="sxs-lookup"><span data-stu-id="e643a-146">Sets the name of the cookie.</span></span> |
| [<span data-ttu-id="e643a-147">Cookie.Path</span><span class="sxs-lookup"><span data-stu-id="e643a-147">Cookie.Path</span></span>](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.path?view=aspnetcore-2.0) | <span data-ttu-id="e643a-148">Zum Isolieren von apps auf dem gleichen Hostnamen verwendet.</span><span class="sxs-lookup"><span data-stu-id="e643a-148">Used to isolate apps running on the same host name.</span></span> <span data-ttu-id="e643a-149">Wenn Sie eine app zu ausgeführt `/app1` und möchten Cookies für diese Anwendung zu beschränken, legen die `CookiePath` Eigenschaft `/app1`.</span><span class="sxs-lookup"><span data-stu-id="e643a-149">If you have an app running at `/app1` and want to restrict cookies to that app, set the `CookiePath` property to `/app1`.</span></span> <span data-ttu-id="e643a-150">Auf diese Weise das Cookie steht nur für Anforderungen an `/app1` und jede app darunter.</span><span class="sxs-lookup"><span data-stu-id="e643a-150">By doing so, the cookie is only available on requests to `/app1` and any app underneath it.</span></span> |
| [<span data-ttu-id="e643a-151">Cookie.SameSite</span><span class="sxs-lookup"><span data-stu-id="e643a-151">Cookie.SameSite</span></span>](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.samesite?view=aspnetcore-2.0) | <span data-ttu-id="e643a-152">Gibt an, ob der Browser das Cookie an demselben Standort Anforderungen nur angefügt werden dürfen (`SameSiteMode.Strict`) oder Cross-Site-Anforderungen, die von sicheren HTTP-Methoden und derselben Website-Anforderungen (`SameSiteMode.Lax`).</span><span class="sxs-lookup"><span data-stu-id="e643a-152">Indicates whether the browser should allow the cookie to be attached to same-site requests only (`SameSiteMode.Strict`) or cross-site requests using safe HTTP methods and same-site requests (`SameSiteMode.Lax`).</span></span> <span data-ttu-id="e643a-153">Bei Festlegung auf `SameSiteMode.None`, der Wert der Cookie-Header nicht festgelegt ist.</span><span class="sxs-lookup"><span data-stu-id="e643a-153">When set to `SameSiteMode.None`, the cookie header value isn't set.</span></span> <span data-ttu-id="e643a-154">Beachten Sie, dass [Cookie-Middleware bewirken die Richtlinie](#cookie-policy-middleware) möglicherweise den Wert überschreiben, die Sie bereitstellen.</span><span class="sxs-lookup"><span data-stu-id="e643a-154">Note that [Cookie Policy Middleware](#cookie-policy-middleware) might overwrite the value that you provide.</span></span> <span data-ttu-id="e643a-155">Um die OAuth-Authentifizierung zu unterstützen, der Standardwert ist `SameSiteMode.Lax`.</span><span class="sxs-lookup"><span data-stu-id="e643a-155">To support OAuth authentication, the default value is `SameSiteMode.Lax`.</span></span> <span data-ttu-id="e643a-156">Weitere Informationen finden Sie unter [OAuth-Authentifizierung aufgrund SameSite Cookierichtlinie unterteilt](https://github.com/aspnet/Security/issues/1231).</span><span class="sxs-lookup"><span data-stu-id="e643a-156">For more information, see [OAuth authentication broken due to SameSite cookie policy](https://github.com/aspnet/Security/issues/1231).</span></span> |
| [<span data-ttu-id="e643a-157">Cookie.SecurePolicy</span><span class="sxs-lookup"><span data-stu-id="e643a-157">Cookie.SecurePolicy</span></span>](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.securepolicy?view=aspnetcore-2.0) | <span data-ttu-id="e643a-158">Ein Flag, das angibt, ob das Cookie auf HTTPS beschränkt werden soll (`CookieSecurePolicy.Always`), HTTP oder HTTPS (`CookieSecurePolicy.None`), oder das gleiche Protokoll wie die Anforderung (`CookieSecurePolicy.SameAsRequest`).</span><span class="sxs-lookup"><span data-stu-id="e643a-158">A flag indicating if the cookie created should be limited to HTTPS (`CookieSecurePolicy.Always`), HTTP or HTTPS (`CookieSecurePolicy.None`), or the same protocol as the request (`CookieSecurePolicy.SameAsRequest`).</span></span> <span data-ttu-id="e643a-159">Der Standardwert ist `CookieSecurePolicy.SameAsRequest`.</span><span class="sxs-lookup"><span data-stu-id="e643a-159">The default value is `CookieSecurePolicy.SameAsRequest`.</span></span> |
| [<span data-ttu-id="e643a-160">DataProtectionProvider</span><span class="sxs-lookup"><span data-stu-id="e643a-160">DataProtectionProvider</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.dataprotectionprovider?view=aspnetcore-2.0) | <span data-ttu-id="e643a-161">Legt die `DataProtectionProvider` , dient zum Erstellen der standardmäßigen `TicketDataFormat`.</span><span class="sxs-lookup"><span data-stu-id="e643a-161">Sets the `DataProtectionProvider` that's used to create the default `TicketDataFormat`.</span></span> <span data-ttu-id="e643a-162">Wenn die `TicketDataFormat` Eigenschaft festgelegt ist, die `DataProtectionProvider` Option wird nicht verwendet.</span><span class="sxs-lookup"><span data-stu-id="e643a-162">If the `TicketDataFormat` property is set, the `DataProtectionProvider` option isn't used.</span></span> <span data-ttu-id="e643a-163">Wenn nicht angegeben, wird die app Datenschutz-Standardanbieter verwendet.</span><span class="sxs-lookup"><span data-stu-id="e643a-163">If not provided, the app's default data protection provider is used.</span></span> |
| [<span data-ttu-id="e643a-164">Ereignisse</span><span class="sxs-lookup"><span data-stu-id="e643a-164">Events</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.events?view=aspnetcore-2.0) | <span data-ttu-id="e643a-165">Der Handler ruft Methoden für den Anbieter, der das app-Steuerelement an bestimmten Punkten Verarbeitung zu ermöglichen.</span><span class="sxs-lookup"><span data-stu-id="e643a-165">The handler calls methods on the provider that give the app control at certain processing points.</span></span> <span data-ttu-id="e643a-166">Wenn `Events` sind nicht angegeben wird, eine Standardinstanz bereitgestellt wird, die keine Aktionen ausführt, wenn die Methoden aufgerufen werden.</span><span class="sxs-lookup"><span data-stu-id="e643a-166">If `Events` aren't provided, a default instance is supplied that does nothing when the methods are called.</span></span> |
| [<span data-ttu-id="e643a-167">EventsType</span><span class="sxs-lookup"><span data-stu-id="e643a-167">EventsType</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.authenticationschemeoptions.eventstype?view=aspnetcore-2.0) | <span data-ttu-id="e643a-168">Als den Diensttyp verwendet, um das Abrufen der `Events` Instanz statt mit der Eigenschaft.</span><span class="sxs-lookup"><span data-stu-id="e643a-168">Used as the service type to get the `Events` instance instead of the property.</span></span> |
| [<span data-ttu-id="e643a-169">ExpireTimeSpan</span><span class="sxs-lookup"><span data-stu-id="e643a-169">ExpireTimeSpan</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.expiretimespan?view=aspnetcore-2.0) | <span data-ttu-id="e643a-170">Die `TimeSpan` nach dem das Authentifizierungsticket gespeichert, in das Cookie abläuft.</span><span class="sxs-lookup"><span data-stu-id="e643a-170">The `TimeSpan` after which the authentication ticket stored inside the cookie expires.</span></span> <span data-ttu-id="e643a-171">`ExpireTimeSpan`wird die aktuelle Uhrzeit auf die Ablaufzeit für das Ticket erstellen hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="e643a-171">`ExpireTimeSpan` is added to the current time to create the expiration time for the ticket.</span></span> <span data-ttu-id="e643a-172">Die `ExpiredTimeSpan` Wert wird immer in der verschlüsselten Authentifizierungsticket, die vom Server überprüft.</span><span class="sxs-lookup"><span data-stu-id="e643a-172">The `ExpiredTimeSpan` value always goes into the encrypted AuthTicket verified by the server.</span></span> <span data-ttu-id="e643a-173">Es kann auch ein Wechsel in den [Set-Cookie](https://tools.ietf.org/html/rfc6265#section-4.1) Header jedoch nur, wenn `IsPersistent` festgelegt ist.</span><span class="sxs-lookup"><span data-stu-id="e643a-173">It may also go into the [Set-Cookie](https://tools.ietf.org/html/rfc6265#section-4.1) header, but only if `IsPersistent` is set.</span></span> <span data-ttu-id="e643a-174">Festzulegende `IsPersistent` zu `true`, konfigurieren Sie die [AuthenticationProperties](/dotnet/api/microsoft.aspnetcore.authentication.authenticationproperties) übergeben `SignInAsync`.</span><span class="sxs-lookup"><span data-stu-id="e643a-174">To set `IsPersistent` to `true`, configure the [AuthenticationProperties](/dotnet/api/microsoft.aspnetcore.authentication.authenticationproperties) passed to `SignInAsync`.</span></span> <span data-ttu-id="e643a-175">Der Standardwert von `ExpireTimeSpan` beträgt 14 Tage.</span><span class="sxs-lookup"><span data-stu-id="e643a-175">The default value of `ExpireTimeSpan` is 14 days.</span></span> |
| [<span data-ttu-id="e643a-176">LoginPath</span><span class="sxs-lookup"><span data-stu-id="e643a-176">LoginPath</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.loginpath?view=aspnetcore-2.0) | <span data-ttu-id="e643a-177">Gibt den Pfad zu einer 302 gefunden (URL-Umleitung) zur Verfügung stellen, wenn von ausgelöst `HttpContext.ChallengeAsync`.</span><span class="sxs-lookup"><span data-stu-id="e643a-177">Provides the path to supply with a 302 Found (URL redirect) when triggered by `HttpContext.ChallengeAsync`.</span></span> <span data-ttu-id="e643a-178">Die aktuelle URL, die den Statuscode 401 generiert wird hinzugefügt, um die `LoginPath` als ein Abfragezeichenfolgen-Parameter mit dem Namen, indem Sie die `ReturnUrlParameter`.</span><span class="sxs-lookup"><span data-stu-id="e643a-178">The current URL that generated the 401 is added to the `LoginPath` as a query string parameter named by the `ReturnUrlParameter`.</span></span> <span data-ttu-id="e643a-179">Einmal eine Anforderung an die `LoginPath` eine neue Identität anmelden gewährt der `ReturnUrlParameter` Wert wird verwendet, um den Browser zurück an die URL umgeleitet, die den ursprünglichen Statuscode nicht autorisiert verursacht hat.</span><span class="sxs-lookup"><span data-stu-id="e643a-179">Once a request to the `LoginPath` grants a new sign-in identity, the `ReturnUrlParameter` value is used to redirect the browser back to the URL that caused the original unauthorized status code.</span></span> <span data-ttu-id="e643a-180">Der Standardwert ist `/Account/Login`.</span><span class="sxs-lookup"><span data-stu-id="e643a-180">The default value is `/Account/Login`.</span></span> |
| [<span data-ttu-id="e643a-181">Logoutpath für</span><span class="sxs-lookup"><span data-stu-id="e643a-181">LogoutPath</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.logoutpath?view=aspnetcore-2.0) | <span data-ttu-id="e643a-182">Wenn die `LogoutPath` wird bereitgestellt, um den Handler auf, dann eine Anforderung an diesen Pfad leitet basierend auf dem Wert, der die `ReturnUrlParameter`.</span><span class="sxs-lookup"><span data-stu-id="e643a-182">If the `LogoutPath` is provided to the handler, then a request to that path redirects based on the value of the `ReturnUrlParameter`.</span></span> <span data-ttu-id="e643a-183">Der Standardwert ist `/Account/Logout`.</span><span class="sxs-lookup"><span data-stu-id="e643a-183">The default value is `/Account/Logout`.</span></span> |
| [<span data-ttu-id="e643a-184">ReturnUrlParameter</span><span class="sxs-lookup"><span data-stu-id="e643a-184">ReturnUrlParameter</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.returnurlparameter?view=aspnetcore-2.0) | <span data-ttu-id="e643a-185">Legt den Namen des Abfragezeichenfolgen-Parameters, der durch den Handler für eine 302 gefunden (URL-Umleitung) Antwort angefügt wird.</span><span class="sxs-lookup"><span data-stu-id="e643a-185">Determines the name of the query string parameter that's appended by the handler for a 302 Found (URL redirect) response.</span></span> <span data-ttu-id="e643a-186">`ReturnUrlParameter`wird verwendet, wenn eine Anforderung eingeht, auf die `LoginPath` oder `LogoutPath` den Browser die ursprüngliche URL zurückgegeben, nachdem die an- bzw. Abmeldung Aktion ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="e643a-186">`ReturnUrlParameter` is used when a request arrives on the `LoginPath` or `LogoutPath` to return the browser to the original URL after the login or logout action is performed.</span></span> <span data-ttu-id="e643a-187">Der Standardwert ist `ReturnUrl`.</span><span class="sxs-lookup"><span data-stu-id="e643a-187">The default value is `ReturnUrl`.</span></span> |
| [<span data-ttu-id="e643a-188">SessionStore</span><span class="sxs-lookup"><span data-stu-id="e643a-188">SessionStore</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.sessionstore?view=aspnetcore-2.0) | <span data-ttu-id="e643a-189">Ein optionaler Container verwendet, um die Identität anforderungsübergreifend gespeichert.</span><span class="sxs-lookup"><span data-stu-id="e643a-189">An optional container used to store identity across requests.</span></span> <span data-ttu-id="e643a-190">Wenn verwendet, wird nur ein Sitzungsbezeichner an den Client gesendet.</span><span class="sxs-lookup"><span data-stu-id="e643a-190">When used, only a session identifier is sent to the client.</span></span> <span data-ttu-id="e643a-191">`SessionStore`kann verwendet werden, um potenzielle Probleme mit großen Identitäten zu verringern.</span><span class="sxs-lookup"><span data-stu-id="e643a-191">`SessionStore` can be used to mitigate potential problems with large identities.</span></span> |
| [<span data-ttu-id="e643a-192">SlidingExpiration</span><span class="sxs-lookup"><span data-stu-id="e643a-192">SlidingExpiration</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.slidingexpiration?view=aspnetcore-2.0) | <span data-ttu-id="e643a-193">Ein Flag, das angibt, ob ein neues Cookie mit einer aktualisierten Ablaufzeit dynamisch ausgegeben werden soll.</span><span class="sxs-lookup"><span data-stu-id="e643a-193">A flag indicating if a new cookie with an updated expiration time should be issued dynamically.</span></span> <span data-ttu-id="e643a-194">Dies kann für jede Anforderung vorkommen, in denen das aktuelle Cookie Ablaufdatum mehr als 50 % abgelaufen ist.</span><span class="sxs-lookup"><span data-stu-id="e643a-194">This can happen on any request where the current cookie expiration period is more than 50% expired.</span></span> <span data-ttu-id="e643a-195">Neuen Ablaufdatum vorwärts verschoben wird, werden das aktuelle Datum plus dem `ExpireTimespan`.</span><span class="sxs-lookup"><span data-stu-id="e643a-195">The new expiration date is moved forward to be the current date plus the `ExpireTimespan`.</span></span> <span data-ttu-id="e643a-196">Ein [Cookie absolute Ablaufzeit](xref:security/authentication/cookie#absolute-cookie-expiration) kann festgelegt werden, mithilfe der `AuthenticationProperties` -Klasse beim Aufrufen von `SignInAsync`.</span><span class="sxs-lookup"><span data-stu-id="e643a-196">An [absolute cookie expiration time](xref:security/authentication/cookie#absolute-cookie-expiration) can be set by using the `AuthenticationProperties` class when calling `SignInAsync`.</span></span> <span data-ttu-id="e643a-197">Eine absolute Ablaufzeit kann verbessern die Sicherheit Ihrer App beschränken die Zeitspanne, die das Authentifizierungscookie gültig ist.</span><span class="sxs-lookup"><span data-stu-id="e643a-197">An absolute expiration time can improve the security of your app by limiting the amount of time that the authentication cookie is valid.</span></span> <span data-ttu-id="e643a-198">Der Standardwert ist `true`.</span><span class="sxs-lookup"><span data-stu-id="e643a-198">The default value is `true`.</span></span> |
| [<span data-ttu-id="e643a-199">TicketDataFormat</span><span class="sxs-lookup"><span data-stu-id="e643a-199">TicketDataFormat</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.ticketdataformat?view=aspnetcore-2.0) | <span data-ttu-id="e643a-200">Die `TicketDataFormat` dient zum Schützen und Aufheben des Schutzes der Identität und andere Eigenschaften, die im Cookiewert gespeichert sind.</span><span class="sxs-lookup"><span data-stu-id="e643a-200">The `TicketDataFormat` is used to protect and unprotect the identity and other properties that are stored in the cookie value.</span></span> <span data-ttu-id="e643a-201">Wenn nicht angegeben wird, eine `TicketDataFormat` erstellt, wobei die [DataProtectionProvider](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.dataprotectionprovider?view=aspnetcore-2.0).</span><span class="sxs-lookup"><span data-stu-id="e643a-201">If not provided, a `TicketDataFormat` is created using the [DataProtectionProvider](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.dataprotectionprovider?view=aspnetcore-2.0).</span></span> |
| [<span data-ttu-id="e643a-202">Überprüfen</span><span class="sxs-lookup"><span data-stu-id="e643a-202">Validate</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.authenticationschemeoptions.validate?view=aspnetcore-2.0) | <span data-ttu-id="e643a-203">Methode, die überprüft, dass die Optionen gültig sind.</span><span class="sxs-lookup"><span data-stu-id="e643a-203">Method that checks that the options are valid.</span></span> |

<span data-ttu-id="e643a-204">Legen Sie `CookieAuthenticationOptions` in der Dienstkonfiguration für die Authentifizierung in der `ConfigureServices` Methode:</span><span class="sxs-lookup"><span data-stu-id="e643a-204">Set `CookieAuthenticationOptions` in the service configuration for authentication in the `ConfigureServices` method:</span></span>

```csharp
services.AddAuthentication(CookieAuthenticationDefaults.AuthenticationScheme)
    .AddCookie(options =>
    {
        ...
    });
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="e643a-205">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="e643a-205">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="e643a-206">ASP.NET Core 1.x verwendet Cookies [Middleware](xref:fundamentals/middleware) , die einen Benutzerprinzipal in einem verschlüsselten Cookie serialisiert.</span><span class="sxs-lookup"><span data-stu-id="e643a-206">ASP.NET Core 1.x uses cookie [middleware](xref:fundamentals/middleware) that serializes a user principal into an encrypted cookie.</span></span> <span data-ttu-id="e643a-207">Bei nachfolgenden Anforderungen das Cookie wird überprüft, und der Prinzipal neu erstellt und zugewiesen wird die `HttpContext.User` Eigenschaft.</span><span class="sxs-lookup"><span data-stu-id="e643a-207">On subsequent requests, the cookie is validated, and the principal is recreated and assigned to the `HttpContext.User` property.</span></span>

<span data-ttu-id="e643a-208">Installieren der [Microsoft.AspNetCore.Authentication.Cookies](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Cookies/) NuGet-Paket im Projekt.</span><span class="sxs-lookup"><span data-stu-id="e643a-208">Install the [Microsoft.AspNetCore.Authentication.Cookies](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Cookies/) NuGet package in your project.</span></span> <span data-ttu-id="e643a-209">Dieses Paket enthält die Cookie-Middleware.</span><span class="sxs-lookup"><span data-stu-id="e643a-209">This package contains the cookie middleware.</span></span>

<span data-ttu-id="e643a-210">Verwenden der `UseCookieAuthentication` Methode in der `Configure` Methode in Ihrer *Startup.cs* Datei vor dem `UseMvc` oder `UseMvcWithDefaultRoute`:</span><span class="sxs-lookup"><span data-stu-id="e643a-210">Use the `UseCookieAuthentication` method in the `Configure` method in your *Startup.cs* file before `UseMvc` or `UseMvcWithDefaultRoute`:</span></span>

```csharp
app.UseCookieAuthentication(new CookieAuthenticationOptions()
{
    AccessDeniedPath = "/Account/Forbidden/",
    AuthenticationScheme = CookieAuthenticationDefaults.AuthenticationScheme,
    AutomaticAuthenticate = true,
    AutomaticChallenge = true,
    LoginPath = "/Account/Unauthorized/"
});
```

<span data-ttu-id="e643a-211">**CookieAuthenticationOptions-Optionen**</span><span class="sxs-lookup"><span data-stu-id="e643a-211">**CookieAuthenticationOptions Options**</span></span>

<span data-ttu-id="e643a-212">Die [CookieAuthenticationOptions](/dotnet/api/Microsoft.AspNetCore.Builder.CookieAuthenticationOptions?view=aspnetcore-1.1) Klasse wird verwendet, um die Anbieteroptionen für die Authentifizierung zu konfigurieren.</span><span class="sxs-lookup"><span data-stu-id="e643a-212">The [CookieAuthenticationOptions](/dotnet/api/Microsoft.AspNetCore.Builder.CookieAuthenticationOptions?view=aspnetcore-1.1) class is used to configure the authentication provider options.</span></span>

| <span data-ttu-id="e643a-213">Option</span><span class="sxs-lookup"><span data-stu-id="e643a-213">Option</span></span> | <span data-ttu-id="e643a-214">Beschreibung</span><span class="sxs-lookup"><span data-stu-id="e643a-214">Description</span></span> |
| ------ | ----------- |
| [<span data-ttu-id="e643a-215">AuthenticationScheme</span><span class="sxs-lookup"><span data-stu-id="e643a-215">AuthenticationScheme</span></span>](/dotnet/api/microsoft.aspnetcore.builder.authenticationoptions.authenticationscheme?view=aspnetcore-1.1) | <span data-ttu-id="e643a-216">Legt das Authentifizierungsschema fest.</span><span class="sxs-lookup"><span data-stu-id="e643a-216">Sets the authentication scheme.</span></span> <span data-ttu-id="e643a-217">`AuthenticationScheme`ist nützlich, wenn mehrere Instanzen von Authentifizierung vorhanden sind und Sie möchten [mit einem bestimmten Schema autorisieren](xref:security/authorization/limitingidentitybyscheme).</span><span class="sxs-lookup"><span data-stu-id="e643a-217">`AuthenticationScheme` is useful when there are multiple instances of authentication and you want to [authorize with a specific scheme](xref:security/authorization/limitingidentitybyscheme).</span></span> <span data-ttu-id="e643a-218">Festlegen der `AuthenticationScheme` zu `CookieAuthenticationDefaults.AuthenticationScheme` "Cookies" der Wert für das Schema enthält.</span><span class="sxs-lookup"><span data-stu-id="e643a-218">Setting the `AuthenticationScheme` to `CookieAuthenticationDefaults.AuthenticationScheme` provides a value of "Cookies" for the scheme.</span></span> <span data-ttu-id="e643a-219">Sie können einen beliebigen Zeichenfolgenwert angeben, der das Schema unterscheidet.</span><span class="sxs-lookup"><span data-stu-id="e643a-219">You can supply any string value that distinguishes the scheme.</span></span> |
| [<span data-ttu-id="e643a-220">AutomaticAuthenticate</span><span class="sxs-lookup"><span data-stu-id="e643a-220">AutomaticAuthenticate</span></span>](/dotnet/api/microsoft.aspnetcore.builder.authenticationoptions.automaticauthenticate?view=aspnetcore-1.1) | <span data-ttu-id="e643a-221">Legt einen Wert aus, um anzugeben, dass die Cookieauthentifizierung für jede Anforderung ausgeführt werden soll, und versuchen, zu überprüfen und serialisierten Prinzipal selbst erstellten zu rekonstruieren.</span><span class="sxs-lookup"><span data-stu-id="e643a-221">Sets a value to indicate that the cookie authentication should run on every request and attempt to validate and reconstruct any serialized principal it created.</span></span> |
| [<span data-ttu-id="e643a-222">AutomaticChallenge</span><span class="sxs-lookup"><span data-stu-id="e643a-222">AutomaticChallenge</span></span>](/dotnet/api/microsoft.aspnetcore.builder.authenticationoptions.automaticchallenge?view=aspnetcore-1.1) | <span data-ttu-id="e643a-223">Bei "true", behandelt die authentifizierungsmiddleware automatische Herausforderungen.</span><span class="sxs-lookup"><span data-stu-id="e643a-223">If true, the authentication middleware handles automatic challenges.</span></span> <span data-ttu-id="e643a-224">Wenn "false", die authentifizierungsmiddleware nur Antworten, wenn dies ausdrücklich angegeben ändert die `AuthenticationScheme`.</span><span class="sxs-lookup"><span data-stu-id="e643a-224">If false, the authentication middleware only alters responses when explicitly indicated by the `AuthenticationScheme`.</span></span> |
| [<span data-ttu-id="e643a-225">ClaimsIssuer</span><span class="sxs-lookup"><span data-stu-id="e643a-225">ClaimsIssuer</span></span>](/dotnet/api/microsoft.aspnetcore.builder.authenticationoptions.claimsissuer?view=aspnetcore-1.1) | <span data-ttu-id="e643a-226">Der Aussteller, verwenden Sie für die [Aussteller](/dotnet/api/system.security.claims.claim.issuer) Eigenschaft für Ansprüche, die von der cookieauthentifizierungsmiddleware erstellt.</span><span class="sxs-lookup"><span data-stu-id="e643a-226">The issuer to use for the [Issuer](/dotnet/api/system.security.claims.claim.issuer) property on any claims created by the cookie authentication middleware.</span></span> |
| [<span data-ttu-id="e643a-227">CookieDomain</span><span class="sxs-lookup"><span data-stu-id="e643a-227">CookieDomain</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.cookiedomain?view=aspnetcore-1.1) | <span data-ttu-id="e643a-228">Der Domänenname, in denen das Cookie bedient wird.</span><span class="sxs-lookup"><span data-stu-id="e643a-228">The domain name where the cookie is served.</span></span> <span data-ttu-id="e643a-229">Standardmäßig ist dies der Hostname der Anforderung.</span><span class="sxs-lookup"><span data-stu-id="e643a-229">By default, this is the host name of the request.</span></span> <span data-ttu-id="e643a-230">Der Browser dient nur das Cookie auf einen übereinstimmenden Hostnamen.</span><span class="sxs-lookup"><span data-stu-id="e643a-230">The browser only serves the cookie to a matching host name.</span></span> <span data-ttu-id="e643a-231">Sie möchten möglicherweise passen Sie diese Option, um Cookies zu einem Host in Ihrer Domäne verfügen.</span><span class="sxs-lookup"><span data-stu-id="e643a-231">You may wish to adjust this to have cookies available to any host in your domain.</span></span> <span data-ttu-id="e643a-232">Z. B. Festlegen der Domäne des Cookies auf `.contoso.com` zur Verfügung `contoso.com`, `www.contoso.com`, und `staging.www.contoso.com`.</span><span class="sxs-lookup"><span data-stu-id="e643a-232">For example, setting the cookie domain to `.contoso.com` makes it available to `contoso.com`, `www.contoso.com`, and `staging.www.contoso.com`.</span></span> |
| [<span data-ttu-id="e643a-233">CookieHttpOnly</span><span class="sxs-lookup"><span data-stu-id="e643a-233">CookieHttpOnly</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.cookiehttponly?view=aspnetcore-1.1) | <span data-ttu-id="e643a-234">Ein Flag, das angibt, ob das Cookie nur an Server zugegriffen werden soll.</span><span class="sxs-lookup"><span data-stu-id="e643a-234">A flag indicating if the cookie should be accessible only to servers.</span></span> <span data-ttu-id="e643a-235">Durch Ändern dieses Werts auf `false` ermöglicht die clientseitige Skripts auf das Cookie zugreifen und möglicherweise Ihrer app das Cookie Diebstahl muss Ihrer app ein [siteübergreifendem Skripting (XSS)](xref:security/cross-site-scripting) Sicherheitsrisiko.</span><span class="sxs-lookup"><span data-stu-id="e643a-235">Changing this value to `false` permits client-side scripts to access the cookie and may open your app to cookie theft should your app have a [Cross-site scripting (XSS)](xref:security/cross-site-scripting) vulnerability.</span></span> <span data-ttu-id="e643a-236">Der Standardwert ist `true`.</span><span class="sxs-lookup"><span data-stu-id="e643a-236">The default value is `true`.</span></span> |
| [<span data-ttu-id="e643a-237">CookiePath</span><span class="sxs-lookup"><span data-stu-id="e643a-237">CookiePath</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.cookiepath?view=aspnetcore-1.1) | <span data-ttu-id="e643a-238">Zum Isolieren von apps auf dem gleichen Hostnamen verwendet.</span><span class="sxs-lookup"><span data-stu-id="e643a-238">Used to isolate apps running on the same host name.</span></span> <span data-ttu-id="e643a-239">Wenn Sie eine app zu ausgeführt `/app1` und möchten Cookies für diese Anwendung zu beschränken, legen die `CookiePath` Eigenschaft `/app1`.</span><span class="sxs-lookup"><span data-stu-id="e643a-239">If you have an app running at `/app1` and want to restrict cookies to that app, set the `CookiePath` property to `/app1`.</span></span> <span data-ttu-id="e643a-240">Auf diese Weise das Cookie steht nur für Anforderungen an `/app1` und jede app darunter.</span><span class="sxs-lookup"><span data-stu-id="e643a-240">By doing so, the cookie is only available on requests to `/app1` and any app underneath it.</span></span> |
| [<span data-ttu-id="e643a-241">CookieSecure</span><span class="sxs-lookup"><span data-stu-id="e643a-241">CookieSecure</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.cookiesecure?view=aspnetcore-1.1) | <span data-ttu-id="e643a-242">Ein Flag, das angibt, ob das Cookie auf HTTPS beschränkt werden soll (`CookieSecurePolicy.Always`), HTTP oder HTTPS (`CookieSecurePolicy.None`), oder das gleiche Protokoll wie die Anforderung (`CookieSecurePolicy.SameAsRequest`).</span><span class="sxs-lookup"><span data-stu-id="e643a-242">A flag indicating if the cookie created should be limited to HTTPS (`CookieSecurePolicy.Always`), HTTP or HTTPS (`CookieSecurePolicy.None`), or the same protocol as the request (`CookieSecurePolicy.SameAsRequest`).</span></span> <span data-ttu-id="e643a-243">Der Standardwert ist `CookieSecurePolicy.SameAsRequest`.</span><span class="sxs-lookup"><span data-stu-id="e643a-243">The default value is `CookieSecurePolicy.SameAsRequest`.</span></span> |
| [<span data-ttu-id="e643a-244">Beschreibung</span><span class="sxs-lookup"><span data-stu-id="e643a-244">Description</span></span>](/dotnet/api/microsoft.aspnetcore.builder.authenticationoptions.description?view=aspnetcore-1.1) | <span data-ttu-id="e643a-245">Weitere Informationen zu den Authentifizierungstyp für die app zur Verfügung gestellt werden.</span><span class="sxs-lookup"><span data-stu-id="e643a-245">Additional information about the authentication type which is made available to the app.</span></span> |
| [<span data-ttu-id="e643a-246">ExpireTimeSpan</span><span class="sxs-lookup"><span data-stu-id="e643a-246">ExpireTimeSpan</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.expiretimespan?view=aspnetcore-1.1) | <span data-ttu-id="e643a-247">Die `TimeSpan` nach dem das Authentifizierungsticket abläuft.</span><span class="sxs-lookup"><span data-stu-id="e643a-247">The `TimeSpan` after which the authentication ticket expires.</span></span> <span data-ttu-id="e643a-248">Es wird die aktuelle Uhrzeit auf die Ablaufzeit für das Ticket erstellen hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="e643a-248">It's added to the current time to create the expiration time for the ticket.</span></span> <span data-ttu-id="e643a-249">Mit `ExpireTimeSpan`, müssen Sie festlegen `IsPersistent` auf `true` in der `AuthenticationProperties` übergeben `SignInAsync`.</span><span class="sxs-lookup"><span data-stu-id="e643a-249">To use `ExpireTimeSpan`, you must set `IsPersistent` to `true` in the `AuthenticationProperties` passed to `SignInAsync`.</span></span> <span data-ttu-id="e643a-250">Der Standardwert beträgt 14 Tage.</span><span class="sxs-lookup"><span data-stu-id="e643a-250">The default value is 14 days.</span></span> |
| [<span data-ttu-id="e643a-251">SlidingExpiration</span><span class="sxs-lookup"><span data-stu-id="e643a-251">SlidingExpiration</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.slidingexpiration?view=aspnetcore-1.1) | <span data-ttu-id="e643a-252">Ein Flag, das angibt, ob das Ablaufdatum der Cookie wird, wenn mehr als die Hälfte der zurückgesetzt der `ExpireTimeSpan` Intervall ist verstrichen.</span><span class="sxs-lookup"><span data-stu-id="e643a-252">A flag indicating whether the cookie expiration date resets when more than half of the `ExpireTimeSpan` interval has passed.</span></span> <span data-ttu-id="e643a-253">Die neue Exipiration Zeit vorwärts verschoben ist, werden das aktuelle Datum plus dem `ExpireTimespan`.</span><span class="sxs-lookup"><span data-stu-id="e643a-253">The new exipiration time is moved forward to be the current date plus the `ExpireTimespan`.</span></span> <span data-ttu-id="e643a-254">Ein [Cookie absolute Ablaufzeit](xref:security/authentication/cookie#absolute-cookie-expiration) kann festgelegt werden, mithilfe der `AuthenticationProperties` -Klasse beim Aufrufen von `SignInAsync`.</span><span class="sxs-lookup"><span data-stu-id="e643a-254">An [absolute cookie expiration time](xref:security/authentication/cookie#absolute-cookie-expiration) can be set by using the `AuthenticationProperties` class when calling `SignInAsync`.</span></span> <span data-ttu-id="e643a-255">Eine absolute Ablaufzeit kann verbessern die Sicherheit Ihrer App beschränken die Zeitspanne, die das Authentifizierungscookie gültig ist.</span><span class="sxs-lookup"><span data-stu-id="e643a-255">An absolute expiration time can improve the security of your app by limiting the amount of time that the authentication cookie is valid.</span></span> <span data-ttu-id="e643a-256">Der Standardwert ist `true`.</span><span class="sxs-lookup"><span data-stu-id="e643a-256">The default value is `true`.</span></span> |

<span data-ttu-id="e643a-257">Legen Sie `CookieAuthenticationOptions` für die Cookieauthentifizierungsmiddleware in die `Configure` Methode:</span><span class="sxs-lookup"><span data-stu-id="e643a-257">Set `CookieAuthenticationOptions` for the Cookie Authentication Middleware in the `Configure` method:</span></span>

```csharp
app.UseCookieAuthentication(new CookieAuthenticationOptions
{
    ...
});
```

---

## <a name="cookie-policy-middleware"></a><span data-ttu-id="e643a-258">Cookie-Middleware bewirken die Richtlinie</span><span class="sxs-lookup"><span data-stu-id="e643a-258">Cookie Policy Middleware</span></span>

<span data-ttu-id="e643a-259">[Cookie-Middleware bewirken die Richtlinie](/dotnet/api/microsoft.aspnetcore.cookiepolicy.cookiepolicymiddleware) Cookie Richtlinie Funktionen in einer app ermöglicht.</span><span class="sxs-lookup"><span data-stu-id="e643a-259">[Cookie Policy Middleware](/dotnet/api/microsoft.aspnetcore.cookiepolicy.cookiepolicymiddleware) enables cookie policy capabilities in an app.</span></span> <span data-ttu-id="e643a-260">Der app-Verarbeitungspipeline die Middleware hinzugefügt ist die Reihenfolge, die vertrauliche; Er wirkt sich nur auf Komponenten, die nach ihm in der Pipeline registriert.</span><span class="sxs-lookup"><span data-stu-id="e643a-260">Adding the middleware to the app processing pipeline is order sensitive; it only affects components registered after it in the pipeline.</span></span>

```csharp
app.UseCookiePolicy(cookiePolicyOptions);
```

 <span data-ttu-id="e643a-261">Die [CookiePolicyOptions](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions) bereitgestellt, um die Cookie-Middleware Richtlinie können Sie globale Eigenschaften der Cookie-Verarbeitung und Hook in Cookie Verarbeitungshandler steuern, wenn Cookies angefügt oder gelöscht werden.</span><span class="sxs-lookup"><span data-stu-id="e643a-261">The [CookiePolicyOptions](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions) provided to the Cookie Policy Middleware allow you to control global characteristics of cookie processing and hook into cookie processing handlers when cookies are appended or deleted.</span></span>

| <span data-ttu-id="e643a-262">Eigenschaft</span><span class="sxs-lookup"><span data-stu-id="e643a-262">Property</span></span> | <span data-ttu-id="e643a-263">Beschreibung</span><span class="sxs-lookup"><span data-stu-id="e643a-263">Description</span></span> |
| -------- | ----------- |
| [<span data-ttu-id="e643a-264">HttpOnly</span><span class="sxs-lookup"><span data-stu-id="e643a-264">HttpOnly</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions.httponly) | <span data-ttu-id="e643a-265">Wirkt sich auf, ob Cookies HttpOnly sein müssen, die ein Flag, der angibt, wenn das Cookie nur an Server zugegriffen werden soll.</span><span class="sxs-lookup"><span data-stu-id="e643a-265">Affects whether cookies must be HttpOnly, which is a flag indicating if the cookie should be accessible only to servers.</span></span> <span data-ttu-id="e643a-266">Der Standardwert ist `HttpOnlyPolicy.None`.</span><span class="sxs-lookup"><span data-stu-id="e643a-266">The default value is `HttpOnlyPolicy.None`.</span></span> |
| [<span data-ttu-id="e643a-267">MinimumSameSitePolicy</span><span class="sxs-lookup"><span data-stu-id="e643a-267">MinimumSameSitePolicy</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions.minimumsamesitepolicy) | <span data-ttu-id="e643a-268">Wirkt sich auf das Cookie gleicher Attribut (siehe unten).</span><span class="sxs-lookup"><span data-stu-id="e643a-268">Affects the cookie's same-site attribute (see below).</span></span> <span data-ttu-id="e643a-269">Der Standardwert ist `SameSiteMode.Lax`.</span><span class="sxs-lookup"><span data-stu-id="e643a-269">The default value is `SameSiteMode.Lax`.</span></span> <span data-ttu-id="e643a-270">Diese Option ist verfügbar für ASP.NET Core 2.0 +.</span><span class="sxs-lookup"><span data-stu-id="e643a-270">This option is available for ASP.NET Core 2.0+.</span></span> |
| [<span data-ttu-id="e643a-271">OnAppendCookie</span><span class="sxs-lookup"><span data-stu-id="e643a-271">OnAppendCookie</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions.onappendcookie) | <span data-ttu-id="e643a-272">Wird aufgerufen, wenn ein Cookie angehängt wird.</span><span class="sxs-lookup"><span data-stu-id="e643a-272">Called when a cookie is appended.</span></span> |
| [<span data-ttu-id="e643a-273">OnDeleteCookie</span><span class="sxs-lookup"><span data-stu-id="e643a-273">OnDeleteCookie</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions.ondeletecookie) | <span data-ttu-id="e643a-274">Wird aufgerufen, wenn ein Cookie gelöscht wird.</span><span class="sxs-lookup"><span data-stu-id="e643a-274">Called when a cookie is deleted.</span></span> |
| [<span data-ttu-id="e643a-275">Sichern</span><span class="sxs-lookup"><span data-stu-id="e643a-275">Secure</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions.secure) | <span data-ttu-id="e643a-276">Bestimmt, ob Cookies Secure sein müssen.</span><span class="sxs-lookup"><span data-stu-id="e643a-276">Affects whether cookies must be Secure.</span></span> <span data-ttu-id="e643a-277">Der Standardwert ist `CookieSecurePolicy.None`.</span><span class="sxs-lookup"><span data-stu-id="e643a-277">The default value is `CookieSecurePolicy.None`.</span></span> |

<span data-ttu-id="e643a-278">**MinimumSameSitePolicy** (ASP.NET 2.0 + nur Kern)</span><span class="sxs-lookup"><span data-stu-id="e643a-278">**MinimumSameSitePolicy** (ASP.NET Core 2.0+ only)</span></span>

<span data-ttu-id="e643a-279">Die Standardeinstellung `MinimumSameSitePolicy` Wert `SameSiteMode.Lax` "oauth2" Authentifizierung zulassen.</span><span class="sxs-lookup"><span data-stu-id="e643a-279">The default `MinimumSameSitePolicy` value is `SameSiteMode.Lax` to permit OAuth2 authentication.</span></span> <span data-ttu-id="e643a-280">Um eine Richtlinie gleichen Standort vom streng erzwingen `SameSiteMode.Strict`legen die `MinimumSameSitePolicy`.</span><span class="sxs-lookup"><span data-stu-id="e643a-280">To strictly enforce a same-site policy of `SameSiteMode.Strict`, set the `MinimumSameSitePolicy`.</span></span> <span data-ttu-id="e643a-281">Obwohl diese Einstellung "oauth2" und andere Cross-Origin-Authentifizierungsschemas unterbrochen wird, erhöht die Cookies für andere Arten von apps, die Cross-Origin-anforderungsverarbeitung benötigen keine Sicherheitsebene.</span><span class="sxs-lookup"><span data-stu-id="e643a-281">Although this setting breaks OAuth2 and other cross-origin authentication schemes, it elevates the level of cookie security for other types of apps that don't rely on cross-origin request processing.</span></span>

```csharp
var cookiePolicyOptions = new CookiePolicyOptions
{
    MinimumSameSitePolicy = SameSiteMode.Strict,
};
```

<span data-ttu-id="e643a-282">Die Einstellung der Cookie-Middleware bewirken die Richtlinie für `MinimumSameSitePolicy` kann Auswirkungen auf die Einstellung für `Cookie.SameSite` in `CookieAuthenticationOptions` Einstellungen entsprechend der Matrix unten.</span><span class="sxs-lookup"><span data-stu-id="e643a-282">The Cookie Policy Middleware setting for `MinimumSameSitePolicy` can affect your setting of `Cookie.SameSite` in `CookieAuthenticationOptions` settings according to the matrix below.</span></span>

| <span data-ttu-id="e643a-283">MinimumSameSitePolicy</span><span class="sxs-lookup"><span data-stu-id="e643a-283">MinimumSameSitePolicy</span></span> | <span data-ttu-id="e643a-284">Cookie.SameSite</span><span class="sxs-lookup"><span data-stu-id="e643a-284">Cookie.SameSite</span></span> | <span data-ttu-id="e643a-285">Resultierende Cookie.SameSite-Einstellung</span><span class="sxs-lookup"><span data-stu-id="e643a-285">Resultant Cookie.SameSite setting</span></span> |
| --------------------- | --------------- | --------------------------------- |
| <span data-ttu-id="e643a-286">SameSiteMode.None</span><span class="sxs-lookup"><span data-stu-id="e643a-286">SameSiteMode.None</span></span>     | <span data-ttu-id="e643a-287">SameSiteMode.None</span><span class="sxs-lookup"><span data-stu-id="e643a-287">SameSiteMode.None</span></span><br><span data-ttu-id="e643a-288">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="e643a-288">SameSiteMode.Lax</span></span><br><span data-ttu-id="e643a-289">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="e643a-289">SameSiteMode.Strict</span></span> | <span data-ttu-id="e643a-290">SameSiteMode.None</span><span class="sxs-lookup"><span data-stu-id="e643a-290">SameSiteMode.None</span></span><br><span data-ttu-id="e643a-291">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="e643a-291">SameSiteMode.Lax</span></span><br><span data-ttu-id="e643a-292">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="e643a-292">SameSiteMode.Strict</span></span> |
| <span data-ttu-id="e643a-293">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="e643a-293">SameSiteMode.Lax</span></span>      | <span data-ttu-id="e643a-294">SameSiteMode.None</span><span class="sxs-lookup"><span data-stu-id="e643a-294">SameSiteMode.None</span></span><br><span data-ttu-id="e643a-295">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="e643a-295">SameSiteMode.Lax</span></span><br><span data-ttu-id="e643a-296">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="e643a-296">SameSiteMode.Strict</span></span> | <span data-ttu-id="e643a-297">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="e643a-297">SameSiteMode.Lax</span></span><br><span data-ttu-id="e643a-298">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="e643a-298">SameSiteMode.Lax</span></span><br><span data-ttu-id="e643a-299">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="e643a-299">SameSiteMode.Strict</span></span> |
| <span data-ttu-id="e643a-300">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="e643a-300">SameSiteMode.Strict</span></span>   | <span data-ttu-id="e643a-301">SameSiteMode.None</span><span class="sxs-lookup"><span data-stu-id="e643a-301">SameSiteMode.None</span></span><br><span data-ttu-id="e643a-302">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="e643a-302">SameSiteMode.Lax</span></span><br><span data-ttu-id="e643a-303">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="e643a-303">SameSiteMode.Strict</span></span> | <span data-ttu-id="e643a-304">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="e643a-304">SameSiteMode.Strict</span></span><br><span data-ttu-id="e643a-305">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="e643a-305">SameSiteMode.Strict</span></span><br><span data-ttu-id="e643a-306">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="e643a-306">SameSiteMode.Strict</span></span> |

## <a name="creating-an-authentication-cookie"></a><span data-ttu-id="e643a-307">Erstellen ein Authentifizierungscookie</span><span class="sxs-lookup"><span data-stu-id="e643a-307">Creating an authentication cookie</span></span>

<span data-ttu-id="e643a-308">Um ein Cookie mit Benutzerinformationen zu erstellen, erstellen Sie eine [ClaimsPrincipal](https://docs.microsoft.com/dotnet/api/system.security.claims.claimsprincipal).</span><span class="sxs-lookup"><span data-stu-id="e643a-308">To create a cookie holding user information, you must construct a [ClaimsPrincipal](https://docs.microsoft.com/dotnet/api/system.security.claims.claimsprincipal).</span></span> <span data-ttu-id="e643a-309">Die Benutzerinformationen serialisiert und im Cookie gespeichert.</span><span class="sxs-lookup"><span data-stu-id="e643a-309">The user information is serialized and stored in the cookie.</span></span> 

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="e643a-310">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="e643a-310">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="e643a-311">Rufen Sie [SignInAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhttpcontextextensions.signinasync?view=aspnetcore-2.0) der Benutzer zur Anmeldung:</span><span class="sxs-lookup"><span data-stu-id="e643a-311">Call [SignInAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhttpcontextextensions.signinasync?view=aspnetcore-2.0) to sign in the user:</span></span>

```csharp
await HttpContext.SignInAsync(
    CookieAuthenticationDefaults.AuthenticationScheme, 
    new ClaimsPrincipal(claimsIdentity));
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="e643a-312">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="e643a-312">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="e643a-313">Rufen Sie [SignInAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhandler-1.signinasync?view=aspnetcore-1.1) der Benutzer zur Anmeldung:</span><span class="sxs-lookup"><span data-stu-id="e643a-313">Call [SignInAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhandler-1.signinasync?view=aspnetcore-1.1) to sign in the user:</span></span>

```csharp
await HttpContext.Authentication.SignInAsync(
    CookieAuthenticationDefaults.AuthenticationScheme,
    new ClaimsPrincipal(claimsIdentity));
```

---

<span data-ttu-id="e643a-314">`SignInAsync`ein verschlüsseltes Cookies erstellt, und der aktuellen Antwort hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="e643a-314">`SignInAsync` creates an encrypted cookie and adds it to the current response.</span></span> <span data-ttu-id="e643a-315">Wenn Sie nicht angeben einer `AuthenticationScheme`, wird das Standardschema verwendet.</span><span class="sxs-lookup"><span data-stu-id="e643a-315">If you don't specify an `AuthenticationScheme`, the default scheme is used.</span></span>

<span data-ttu-id="e643a-316">Im Hintergrund, wird die Verschlüsselung verwendet ASP.NET Core des [Datenschutz](xref:security/data-protection/using-data-protection#security-data-protection-getting-started) System.</span><span class="sxs-lookup"><span data-stu-id="e643a-316">Under the covers, the encryption used is ASP.NET Core's [Data Protection](xref:security/data-protection/using-data-protection#security-data-protection-getting-started) system.</span></span> <span data-ttu-id="e643a-317">Wenn Sie sind auf mehreren Computern, den Lastenausgleich für apps oder mit einer Webfarm app hosten, müssen Sie [Schutz von Daten konfigurieren](xref:security/data-protection/configuration/overview) und mit dem gleichen Schlüssel Ring app-Bezeichner.</span><span class="sxs-lookup"><span data-stu-id="e643a-317">If you're hosting app on multiple machines, load balancing across apps, or using a web farm, then you must [configure data protection](xref:security/data-protection/configuration/overview) to use the same key ring and app identifier.</span></span>

## <a name="signing-out"></a><span data-ttu-id="e643a-318">Abmelden</span><span class="sxs-lookup"><span data-stu-id="e643a-318">Signing out</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="e643a-319">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="e643a-319">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="e643a-320">Rufen Sie den aktuellen Benutzer abmelden, und löschen ihre Cookies, [SignOutAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhttpcontextextensions.signoutasync?view=aspnetcore-2.0):</span><span class="sxs-lookup"><span data-stu-id="e643a-320">To sign out the current user and delete their cookie, call [SignOutAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhttpcontextextensions.signoutasync?view=aspnetcore-2.0):</span></span>

```csharp
await HttpContext.SignOutAsync(
    CookieAuthenticationDefaults.AuthenticationScheme);
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="e643a-321">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="e643a-321">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="e643a-322">Rufen Sie den aktuellen Benutzer abmelden, und löschen ihre Cookies, [SignOutAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhandler-1.signoutasync?view=aspnetcore-1.1):</span><span class="sxs-lookup"><span data-stu-id="e643a-322">To sign out the current user and delete their cookie, call [SignOutAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhandler-1.signoutasync?view=aspnetcore-1.1):</span></span>

```csharp
await HttpContext.Authentication.SignOutAsync(
    CookieAuthenticationDefaults.AuthenticationScheme);
```

---

<span data-ttu-id="e643a-323">Wenn Sie nicht `CookieAuthenticationDefaults.AuthenticationScheme` (oder "Cookies") als Formate (z. B. "ContosoCookie"), geben Sie das Schema, das Sie beim Konfigurieren des Authentifizierungsanbieters verwendet.</span><span class="sxs-lookup"><span data-stu-id="e643a-323">If you aren't using `CookieAuthenticationDefaults.AuthenticationScheme` (or "Cookies") as the scheme (for example, "ContosoCookie"), supply the scheme you used when configuring the authentication provider.</span></span> <span data-ttu-id="e643a-324">Andernfalls ist das Standardschema verwendet.</span><span class="sxs-lookup"><span data-stu-id="e643a-324">Otherwise, the default scheme is used.</span></span>

## <a name="reacting-to-back-end-changes"></a><span data-ttu-id="e643a-325">Das reagieren auf Back-End-Änderungen</span><span class="sxs-lookup"><span data-stu-id="e643a-325">Reacting to back-end changes</span></span>

<span data-ttu-id="e643a-326">Sobald ein Cookie erstellt wurde, wird es die einzige Quelle der Identität.</span><span class="sxs-lookup"><span data-stu-id="e643a-326">Once a cookie is created, it becomes the single source of identity.</span></span> <span data-ttu-id="e643a-327">Auch wenn Sie einen Benutzer in Ihrem Back-End-Systemen deaktivieren, die Cookie-Authentifizierungssystem hat keine Informationen über diese, und ein Benutzer angemeldet bleibt, solange ihre Cookie gültig ist.</span><span class="sxs-lookup"><span data-stu-id="e643a-327">Even if you disable a user in your back-end systems, the cookie authentication system has no knowledge of this, and a user stays logged in as long as their cookie is valid.</span></span>

<span data-ttu-id="e643a-328">Die [ValidatePrincipal](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationevents.validateprincipal) Ereignis in ASP.NET Core 2.x oder [ValidateAsync](/dotnet/api/microsoft.aspnetcore.identity.isecuritystampvalidator.validateasync?view=aspnetcore-1.1) Methode in ASP.NET Core 1.x abfangen und Überschreiben der Überprüfung der Identität des Cookies verwendet werden können.</span><span class="sxs-lookup"><span data-stu-id="e643a-328">The [ValidatePrincipal](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationevents.validateprincipal) event in ASP.NET Core 2.x or the [ValidateAsync](/dotnet/api/microsoft.aspnetcore.identity.isecuritystampvalidator.validateasync?view=aspnetcore-1.1) method in ASP.NET Core 1.x can be used to intercept and override validation of the cookie identity.</span></span> <span data-ttu-id="e643a-329">Dieser Ansatz reduziert das Risiko gesperrte Benutzer Zugriff auf die app.</span><span class="sxs-lookup"><span data-stu-id="e643a-329">This approach mitigates the risk of revoked users accessing the app.</span></span>

<span data-ttu-id="e643a-330">Ein Ansatz für die gültigkeitsprüfung für Cookies basiert auf Nachverfolgen der bei die Datenbank geändert wurde.</span><span class="sxs-lookup"><span data-stu-id="e643a-330">One approach to cookie validation is based on keeping track of when the user database has been changed.</span></span> <span data-ttu-id="e643a-331">Wenn die Datenbank nicht geändert wurde, da der Benutzer Cookie ausgestellt wurde, besteht keine Notwendigkeit, den Benutzer erneut zu authentifizieren, wenn ihre Cookie noch gültig ist.</span><span class="sxs-lookup"><span data-stu-id="e643a-331">If the database hasn't been changed since the user's cookie was issued, there is no need to re-authenticate the user if their cookie is still valid.</span></span> <span data-ttu-id="e643a-332">Zum Implementieren dieses Szenarios, die Datenbank, die in implementiert wird `IUserRepository` in diesem Beispiel speichert ein `LastChanged` Wert.</span><span class="sxs-lookup"><span data-stu-id="e643a-332">To implement this scenario, the database, which is implemented in `IUserRepository` for this example, stores a `LastChanged` value.</span></span> <span data-ttu-id="e643a-333">Wenn jeder Benutzer in der Datenbank aktualisiert wird der `LastChanged` Wert auf die aktuelle Uhrzeit festgelegt wird.</span><span class="sxs-lookup"><span data-stu-id="e643a-333">When any user is updated in the database, the `LastChanged` value is set to the current time.</span></span>

<span data-ttu-id="e643a-334">Um ein Cookie für ungültig zu erklären, wenn Änderungen an der Datenbank auf Grundlage der `LastChanged` -Wert, erstellen Sie das Cookie mit einer `LastChanged` Anspruch mit dem aktuellen `LastChanged` Wert aus der Datenbank:</span><span class="sxs-lookup"><span data-stu-id="e643a-334">In order to invalidate a cookie when the database changes based on the `LastChanged` value, create the cookie with a `LastChanged` claim containing the current `LastChanged` value from the database:</span></span>

```csharp
var claims = new List<Claim>
{
    new Claim(ClaimTypes.Name, user.Email),
    new Claim("LastChanged", {Database Value})
};

var claimsIdentity = new ClaimsIdentity(
    claims, 
    CookieAuthenticationDefaults.AuthenticationScheme);

await HttpContext.SignInAsync(
    CookieAuthenticationDefaults.AuthenticationScheme, 
    new ClaimsPrincipal(claimsIdentity));
```

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="e643a-335">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="e643a-335">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="e643a-336">Implementieren Sie eine Außerkraftsetzung für die `ValidatePrincipal` Ereignis, Schreiben Sie eine Methode mit der folgenden Signatur in einer Klasse, die Sie ableiten [CookieAuthenticationEvents](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationevents):</span><span class="sxs-lookup"><span data-stu-id="e643a-336">To implement an override for the `ValidatePrincipal` event, write a method with the following signature in a class that you derive from [CookieAuthenticationEvents](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationevents):</span></span>

```csharp
ValidatePrincipal(CookieValidatePrincipalContext)
```

<span data-ttu-id="e643a-337">Ein Beispiel sieht wie folgt aus:</span><span class="sxs-lookup"><span data-stu-id="e643a-337">An example looks like the following:</span></span>

```csharp
using System.Linq;
using System.Threading.Tasks;
using Microsoft.AspNetCore.Authentication;
using Microsoft.AspNetCore.Authentication.Cookies;

public class CustomCookieAuthenticationEvents : CookieAuthenticationEvents
{
    private readonly IUserRepository _userRepository;

    public CustomCookieAuthenticationEvents(IUserRepository userRepository)
    {
        // Get the database from registered DI services.
        _userRepository = userRepository;
    }

    public override async Task ValidatePrincipal(CookieValidatePrincipalContext context)
    {
        var userPrincipal = context.Principal;

        // Look for the LastChanged claim.
        var lastChanged = (from c in userPrincipal.Claims
                           where c.Type == "LastChanged"
                           select c.Value).FirstOrDefault();

        if (string.IsNullOrEmpty(lastChanged) ||
            !_userRepository.ValidateLastChanged(lastChanged))
        {
            context.RejectPrincipal();

            await context.HttpContext.SignOutAsync(
                CookieAuthenticationDefaults.AuthenticationScheme);
        }
    }
}
```

<span data-ttu-id="e643a-338">Registrieren die Instanz Ereignisse während der Cookie-Service-Registrierung in der `ConfigureServices` Methode.</span><span class="sxs-lookup"><span data-stu-id="e643a-338">Register the events instance during cookie service registration in the `ConfigureServices` method.</span></span> <span data-ttu-id="e643a-339">Geben Sie eine Bereichsbezogene Service-Registrierung für Ihre `CustomCookieAuthenticationEvents` Klasse:</span><span class="sxs-lookup"><span data-stu-id="e643a-339">Provide a scoped service registration for your `CustomCookieAuthenticationEvents` class:</span></span>

```csharp
services.AddAuthentication(CookieAuthenticationDefaults.AuthenticationScheme)
    .AddCookie(options =>
    {
        options.EventsType = typeof(CustomCookieAuthenticationEvents);
    });

services.AddScoped<CustomCookieAuthenticationEvents>();
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="e643a-340">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="e643a-340">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="e643a-341">Implementieren Sie eine Außerkraftsetzung für die `ValidateAsync` Ereignis, Schreiben Sie eine Methode mit der folgenden Signatur:</span><span class="sxs-lookup"><span data-stu-id="e643a-341">To implement an override for the `ValidateAsync` event, write a method with the following signature:</span></span>

```csharp
ValidateAsync(CookieValidatePrincipalContext)
```

<span data-ttu-id="e643a-342">ASP.NET Core Identity implementiert diese Überprüfung als Teil seiner [SecurityStampValidator](/dotnet/api/microsoft.aspnetcore.identity.securitystampvalidator-1.validateasync).</span><span class="sxs-lookup"><span data-stu-id="e643a-342">ASP.NET Core Identity implements this check as part of its [SecurityStampValidator](/dotnet/api/microsoft.aspnetcore.identity.securitystampvalidator-1.validateasync).</span></span> <span data-ttu-id="e643a-343">Ein Beispiel sieht wie folgt aus:</span><span class="sxs-lookup"><span data-stu-id="e643a-343">An example looks like the following:</span></span>

```csharp
public static class LastChangedValidator
{
    public static async Task ValidateAsync(CookieValidatePrincipalContext context)
    {
        // Pull database from registered DI services.
        var userRepository = 
            context.HttpContext.RequestServices
                .GetRequiredService<IUserRepository>();
        var userPrincipal = context.Principal;

        // Look for the last changed claim.
        var lastChanged = (from c in userPrincipal.Claims
                           where c.Type == "LastChanged"
                           select c.Value).FirstOrDefault();

        if (string.IsNullOrEmpty(lastChanged) ||
            !userRepository.ValidateLastChanged(lastChanged))
        {
            context.RejectPrincipal();

            await context.HttpContext.SignOutAsync(
                CookieAuthenticationDefaults.AuthenticationScheme);
        }
    }
}
```

<span data-ttu-id="e643a-344">Registrieren Sie das Ereignis während der Konfiguration der Cookie-Authentifizierung in der `Configure` Methode:</span><span class="sxs-lookup"><span data-stu-id="e643a-344">Register the event during cookie authentication configuration in the `Configure` method:</span></span>

```csharp
app.UseCookieAuthentication(new CookieAuthenticationOptions
{
    Events = new CookieAuthenticationEvents
    {
        OnValidatePrincipal = LastChangedValidator.ValidateAsync
    }
});
```

---

<span data-ttu-id="e643a-345">Betrachten Sie eine Situation, in dem der Name des Benutzers aktualisiert &mdash; eine Entscheidung, die Sicherheit in keiner Weise beeinflussen nicht.</span><span class="sxs-lookup"><span data-stu-id="e643a-345">Consider a situation in which the user's name is updated &mdash; a decision that doesn't affect security in any way.</span></span> <span data-ttu-id="e643a-346">Wenn Sie den Benutzerprinzipalnamen zerstörungsfrei aktualisieren möchten, rufen Sie `context.ReplacePrincipal` und legen Sie die `context.ShouldRenew` Eigenschaft `true`.</span><span class="sxs-lookup"><span data-stu-id="e643a-346">If you want to non-destructively update the user principal, call `context.ReplacePrincipal` and set the `context.ShouldRenew` property to `true`.</span></span>

> [!WARNING]
> <span data-ttu-id="e643a-347">Die hier beschriebene Vorgehensweise wird bei jeder Anforderung ausgelöst.</span><span class="sxs-lookup"><span data-stu-id="e643a-347">The approach described here is triggered on every request.</span></span> <span data-ttu-id="e643a-348">Dies kann in großen Leistungseinbußen bei der app führen.</span><span class="sxs-lookup"><span data-stu-id="e643a-348">This can result in a large performance penalty for the app.</span></span>

## <a name="persistent-cookies"></a><span data-ttu-id="e643a-349">Permanente cookies</span><span class="sxs-lookup"><span data-stu-id="e643a-349">Persistent cookies</span></span>

<span data-ttu-id="e643a-350">Sie sollten das Cookie über Browsersitzungen hinweg beibehalten werden.</span><span class="sxs-lookup"><span data-stu-id="e643a-350">You may want the cookie to persist across browser sessions.</span></span> <span data-ttu-id="e643a-351">Dieser Persistenz sollte nur mit expliziten benutzerzustimmung mit einem Kontrollkästchen "Anmeldedaten" auf den Anmeldenamen oder einen ähnlichen Mechanismus aktiviert werden.</span><span class="sxs-lookup"><span data-stu-id="e643a-351">This persistence should only be enabled with explicit user consent with a "Remember Me" checkbox on login or a similar mechanism.</span></span> 

<span data-ttu-id="e643a-352">Der folgende Codeausschnitt erstellt eine Identität und die entsprechenden Cookie, das über Browser Abschlüsse überdauert.</span><span class="sxs-lookup"><span data-stu-id="e643a-352">The following code snippet creates an identity and corresponding cookie that survives through browser closures.</span></span> <span data-ttu-id="e643a-353">Alle gleitenden ablaufeinstellungen, die zuvor konfiguriert haben, werden berücksichtigt.</span><span class="sxs-lookup"><span data-stu-id="e643a-353">Any sliding expiration settings previously configured are honored.</span></span> <span data-ttu-id="e643a-354">Wenn das Cookie abläuft, während der Browser geschlossen wird, löscht der Browser das Cookie an, nach dem Neustart wird.</span><span class="sxs-lookup"><span data-stu-id="e643a-354">If the cookie expires while the browser is closed, the browser clears the cookie once it's restarted.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="e643a-355">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="e643a-355">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
await HttpContext.SignInAsync(
    CookieAuthenticationDefaults.AuthenticationScheme,
    new ClaimsPrincipal(claimsIdentity),
    new AuthenticationProperties
    {
        IsPersistent = true
    });
```

<span data-ttu-id="e643a-356">Die [AuthenticationProperties](/dotnet/api/microsoft.aspnetcore.authentication.authenticationproperties?view=aspnetcore-2.0) Klasse befindet sich in der `Microsoft.AspNetCore.Authentication` Namespace.</span><span class="sxs-lookup"><span data-stu-id="e643a-356">The [AuthenticationProperties](/dotnet/api/microsoft.aspnetcore.authentication.authenticationproperties?view=aspnetcore-2.0) class resides in the `Microsoft.AspNetCore.Authentication` namespace.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="e643a-357">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="e643a-357">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
await HttpContext.Authentication.SignInAsync(
    CookieAuthenticationDefaults.AuthenticationScheme,
    new ClaimsPrincipal(claimsIdentity),
    new AuthenticationProperties
    {
        IsPersistent = true
    });
```

<span data-ttu-id="e643a-358">Die [AuthenticationProperties](/dotnet/api/microsoft.aspnetcore.http.authentication.authenticationproperties?view=aspnetcore-1.1) Klasse befindet sich in der `Microsoft.AspNetCore.Http.Authentication` Namespace.</span><span class="sxs-lookup"><span data-stu-id="e643a-358">The [AuthenticationProperties](/dotnet/api/microsoft.aspnetcore.http.authentication.authenticationproperties?view=aspnetcore-1.1) class resides in the `Microsoft.AspNetCore.Http.Authentication` namespace.</span></span>

---

## <a name="absolute-cookie-expiration"></a><span data-ttu-id="e643a-359">Absolute cookieablauf</span><span class="sxs-lookup"><span data-stu-id="e643a-359">Absolute cookie expiration</span></span>

<span data-ttu-id="e643a-360">Sie können festlegen, dass eine absolute Ablaufzeit mit `ExpiresUtc`.</span><span class="sxs-lookup"><span data-stu-id="e643a-360">You can set an absolute expiration time with `ExpiresUtc`.</span></span> <span data-ttu-id="e643a-361">Sie müssen auch festlegen `IsPersistent`ist, andernfalls `ExpiresUtc` wird ignoriert, und ein einzelnes Sitzungscookie wird erstellt.</span><span class="sxs-lookup"><span data-stu-id="e643a-361">You must also set `IsPersistent`; otherwise, `ExpiresUtc` is ignored and a single-session cookie is created.</span></span> <span data-ttu-id="e643a-362">Wenn `ExpiresUtc` auf festgelegt ist `SignInAsync`, er überschreibt den Wert für die `ExpireTimeSpan` -Option von `CookieAuthenticationOptions`, sofern festgelegt.</span><span class="sxs-lookup"><span data-stu-id="e643a-362">When `ExpiresUtc` is set on `SignInAsync`, it overrides the value of the `ExpireTimeSpan` option of `CookieAuthenticationOptions`, if set.</span></span>

<span data-ttu-id="e643a-363">Der folgende Codeausschnitt erstellt eine Identität und die entsprechenden Cookie, das 20 Minuten dauert.</span><span class="sxs-lookup"><span data-stu-id="e643a-363">The following code snippet creates an identity and corresponding cookie that lasts for 20 minutes.</span></span> <span data-ttu-id="e643a-364">Dies ignoriert alle gleitenden ablaufeinstellungen, die zuvor konfiguriert haben.</span><span class="sxs-lookup"><span data-stu-id="e643a-364">This ignores any sliding expiration settings previously configured.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="e643a-365">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="e643a-365">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
await HttpContext.SignInAsync(
    CookieAuthenticationDefaults.AuthenticationScheme,
    new ClaimsPrincipal(claimsIdentity),
    new AuthenticationProperties
    {
        IsPersistent = true,
        ExpiresUtc = DateTime.UtcNow.AddMinutes(20)
    });
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="e643a-366">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="e643a-366">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
await HttpContext.Authentication.SignInAsync(
    CookieAuthenticationDefaults.AuthenticationScheme,
    new ClaimsPrincipal(claimsIdentity),
    new AuthenticationProperties
    {
        IsPersistent = true,
        ExpiresUtc = DateTime.UtcNow.AddMinutes(20)
    });
```

---

## <a name="see-also"></a><span data-ttu-id="e643a-367">Siehe auch</span><span class="sxs-lookup"><span data-stu-id="e643a-367">See also</span></span>

* [<span data-ttu-id="e643a-368">Auth 2.0 Änderungen / Migration Ankündigung</span><span class="sxs-lookup"><span data-stu-id="e643a-368">Auth 2.0 Changes / Migration Announcement</span></span>](https://github.com/aspnet/Announcements/issues/262)
* [<span data-ttu-id="e643a-369">Einschränken der Identität nach Schema</span><span class="sxs-lookup"><span data-stu-id="e643a-369">Limiting identity by scheme</span></span>](xref:security/authorization/limitingidentitybyscheme)
