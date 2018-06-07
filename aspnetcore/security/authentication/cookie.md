---
title: Cookie-Authentifizierung ohne ASP.NET Core Identität verwenden
author: rick-anderson
description: Eine Erläuterung der Verwendung von Cookieauthentifizierung ohne ASP.NET Core Identity
manager: wpickett
ms.author: riande
ms.date: 10/11/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authentication/cookie
ms.openlocfilehash: 82f826bbc2bb19339851d5e25c539ea2c03acfb3
ms.sourcegitcommit: 6784510cfb589308c3875ccb5113eb31031766b4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/06/2018
ms.locfileid: "34819109"
---
# <a name="use-cookie-authentication-without-aspnet-core-identity"></a><span data-ttu-id="e9323-103">Cookie-Authentifizierung ohne ASP.NET Core Identität verwenden</span><span class="sxs-lookup"><span data-stu-id="e9323-103">Use cookie authentication without ASP.NET Core Identity</span></span>

<span data-ttu-id="e9323-104">Durch [Rick Anderson](https://twitter.com/RickAndMSFT) und [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="e9323-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="e9323-105">Wie Sie in den früheren Authentifizierungsthemen gesehen haben [ASP.NET Core Identity](xref:security/authentication/identity) ist ein vollständige, voll ausgestatteten Authentifizierungsanbieter zum Erstellen und Verwalten von Anmeldungen.</span><span class="sxs-lookup"><span data-stu-id="e9323-105">As you've seen in the earlier authentication topics, [ASP.NET Core Identity](xref:security/authentication/identity) is a complete, full-featured authentication provider for creating and maintaining logins.</span></span> <span data-ttu-id="e9323-106">Allerdings sollten Sie Ihre eigenen benutzerdefinierten Authentifizierungslogik mit Zeiten Cookie-basierte Authentifizierung zu verwenden.</span><span class="sxs-lookup"><span data-stu-id="e9323-106">However, you may want to use your own custom authentication logic with cookie-based authentication at times.</span></span> <span data-ttu-id="e9323-107">Sie können als eigenständige Authentifizierungsanbieter ohne ASP.NET Core Identity Cookie-basierte Authentifizierung verwenden.</span><span class="sxs-lookup"><span data-stu-id="e9323-107">You can use cookie-based authentication as a standalone authentication provider without ASP.NET Core Identity.</span></span>

<span data-ttu-id="e9323-108">[Anzeigen oder Herunterladen von Beispielcode](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authentication/cookie/samples) ([Vorgehensweise zum Herunterladen](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="e9323-108">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authentication/cookie/samples) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="e9323-109">Zu Demonstrationszwecken in der Beispiel-app ist das Benutzerkonto für den Benutzer hypothetischen Maria Rodriguez, in der Anwendung hartcodiert.</span><span class="sxs-lookup"><span data-stu-id="e9323-109">For demonstration purposes in the sample app, the user account for the hypothetical user, Maria Rodriguez, is hardcoded into the app.</span></span> <span data-ttu-id="e9323-110">Verwenden Sie die e-Mail-Benutzernamens "maria.rodriguez@contoso.com" und einem Kennwort zur Anmeldung des Benutzers.</span><span class="sxs-lookup"><span data-stu-id="e9323-110">Use the Email username "maria.rodriguez@contoso.com" and any password to sign in the user.</span></span> <span data-ttu-id="e9323-111">Der Benutzer wird authentifiziert, der `AuthenticateUser` Methode in der *Pages/Account/Login.cshtml.cs* Datei.</span><span class="sxs-lookup"><span data-stu-id="e9323-111">The user is authenticated in the `AuthenticateUser` method in the *Pages/Account/Login.cshtml.cs* file.</span></span> <span data-ttu-id="e9323-112">In einem realen Beispiel würde der Benutzer für eine Datenbank authentifiziert werden.</span><span class="sxs-lookup"><span data-stu-id="e9323-112">In a real-world example, the user would be authenticated against a database.</span></span>

<span data-ttu-id="e9323-113">Informationen zum Migrieren von Cookie-basierte Authentifizierung von ASP.NET Core 1.x auf 2.0 finden Sie unter [migrieren Authentifizierung und Identität zu ASP.NET Core 2.0 Thema (Cookie-basierte Authentifizierung)](xref:migration/1x-to-2x/identity-2x#cookie-based-authentication).</span><span class="sxs-lookup"><span data-stu-id="e9323-113">For information on migrating cookie-based authentication from ASP.NET Core 1.x to 2.0, see [Migrate Authentication and Identity to ASP.NET Core 2.0 topic (Cookie-based Authentication)](xref:migration/1x-to-2x/identity-2x#cookie-based-authentication).</span></span>

<span data-ttu-id="e9323-114">Um ASP.NET Core Identity verwenden zu können, finden Sie unter der [Einführung in die Identität](xref:security/authentication/identity) Thema.</span><span class="sxs-lookup"><span data-stu-id="e9323-114">To use ASP.NET Core Identity, see the [Introduction to Identity](xref:security/authentication/identity) topic.</span></span>

## <a name="configuration"></a><span data-ttu-id="e9323-115">Konfiguration</span><span class="sxs-lookup"><span data-stu-id="e9323-115">Configuration</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="e9323-116">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="e9323-116">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

<span data-ttu-id="e9323-117">Wenn Sie die app keine der [Microsoft.AspNetCore.App Metapackage](xref:fundamentals/metapackage-app), erstellen Sie einen Paket Verweis in der Projektdatei für die [Microsoft.AspNetCore.Authentication.Cookies](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Cookies/) Paket (Version 2.1.0 oder weiter unten).</span><span class="sxs-lookup"><span data-stu-id="e9323-117">If the app doesn't use the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app), create a package reference in the project file for the [Microsoft.AspNetCore.Authentication.Cookies](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Cookies/) package (version 2.1.0 or later).</span></span>

<span data-ttu-id="e9323-118">In der `ConfigureServices` -Methode den Authentifizierung-Middleware-Dienst mit dem Erstellen der `AddAuthentication` und `AddCookie` Methoden:</span><span class="sxs-lookup"><span data-stu-id="e9323-118">In the `ConfigureServices` method, create the Authentication Middleware service with the `AddAuthentication` and `AddCookie` methods:</span></span>

[!code-csharp[](cookie/samples/2.x/CookieSample/Startup.cs?name=snippet1)]

<span data-ttu-id="e9323-119">`AuthenticationScheme` übergeben zum `AddAuthentication` legt das Standard-Authentifizierungsschema für die app.</span><span class="sxs-lookup"><span data-stu-id="e9323-119">`AuthenticationScheme` passed to `AddAuthentication` sets the default authentication scheme for the app.</span></span> <span data-ttu-id="e9323-120">`AuthenticationScheme` ist nützlich, wenn mehrere Instanzen der Cookieauthentifizierung vorhanden sind und Sie möchten [mit einem bestimmten Schema autorisieren](xref:security/authorization/limitingidentitybyscheme).</span><span class="sxs-lookup"><span data-stu-id="e9323-120">`AuthenticationScheme` is useful when there are multiple instances of cookie authentication and you want to [authorize with a specific scheme](xref:security/authorization/limitingidentitybyscheme).</span></span> <span data-ttu-id="e9323-121">Festlegen der `AuthenticationScheme` zu `CookieAuthenticationDefaults.AuthenticationScheme` "Cookies" der Wert für das Schema enthält.</span><span class="sxs-lookup"><span data-stu-id="e9323-121">Setting the `AuthenticationScheme` to `CookieAuthenticationDefaults.AuthenticationScheme` provides a value of "Cookies" for the scheme.</span></span> <span data-ttu-id="e9323-122">Sie können einen beliebigen Zeichenfolgenwert angeben, der das Schema unterscheidet.</span><span class="sxs-lookup"><span data-stu-id="e9323-122">You can supply any string value that distinguishes the scheme.</span></span>

<span data-ttu-id="e9323-123">In der `Configure` -Methode, mit der `UseAuthentication` Authentifizierung-Middleware aufgerufen werden soll, der festlegt der `HttpContext.User` Eigenschaft.</span><span class="sxs-lookup"><span data-stu-id="e9323-123">In the `Configure` method, use the `UseAuthentication` method to invoke the Authentication Middleware that sets the `HttpContext.User` property.</span></span> <span data-ttu-id="e9323-124">Rufen Sie die `UseAuthentication` Methode vor dem Aufruf `UseMvcWithDefaultRoute` oder `UseMvc`:</span><span class="sxs-lookup"><span data-stu-id="e9323-124">Call the `UseAuthentication` method before calling `UseMvcWithDefaultRoute` or `UseMvc`:</span></span>

[!code-csharp[](cookie/samples/2.x/CookieSample/Startup.cs?name=snippet2)]

<span data-ttu-id="e9323-125">**AddCookie-Optionen**</span><span class="sxs-lookup"><span data-stu-id="e9323-125">**AddCookie Options**</span></span>

<span data-ttu-id="e9323-126">Die [CookieAuthenticationOptions](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions?view=aspnetcore-2.0) Klasse wird verwendet, um die Anbieteroptionen für die Authentifizierung zu konfigurieren.</span><span class="sxs-lookup"><span data-stu-id="e9323-126">The [CookieAuthenticationOptions](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions?view=aspnetcore-2.0) class is used to configure the authentication provider options.</span></span>

| <span data-ttu-id="e9323-127">Option</span><span class="sxs-lookup"><span data-stu-id="e9323-127">Option</span></span> | <span data-ttu-id="e9323-128">Beschreibung</span><span class="sxs-lookup"><span data-stu-id="e9323-128">Description</span></span> |
| ------ | ----------- |
| [<span data-ttu-id="e9323-129">AccessDeniedPath</span><span class="sxs-lookup"><span data-stu-id="e9323-129">AccessDeniedPath</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.accessdeniedpath?view=aspnetcore-2.0) | <span data-ttu-id="e9323-130">Gibt den Pfad zu einer 302 gefunden (URL-Umleitung) zur Verfügung stellen, wenn von ausgelöst `HttpContext.ForbidAsync`.</span><span class="sxs-lookup"><span data-stu-id="e9323-130">Provides the path to supply with a 302 Found (URL redirect) when triggered by `HttpContext.ForbidAsync`.</span></span> <span data-ttu-id="e9323-131">Der Standardwert ist `/Account/AccessDenied`.</span><span class="sxs-lookup"><span data-stu-id="e9323-131">The default value is `/Account/AccessDenied`.</span></span> |
| [<span data-ttu-id="e9323-132">ClaimsIssuer</span><span class="sxs-lookup"><span data-stu-id="e9323-132">ClaimsIssuer</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.authenticationschemeoptions.claimsissuer?view=aspnetcore-2.0) | <span data-ttu-id="e9323-133">Der Aussteller, verwenden Sie für die [Aussteller](/dotnet/api/system.security.claims.claim.issuer) Eigenschaft für Ansprüche, die vom Cookie Authentication-Dienst erstellt.</span><span class="sxs-lookup"><span data-stu-id="e9323-133">The issuer to use for the [Issuer](/dotnet/api/system.security.claims.claim.issuer) property on any claims created by the cookie authentication service.</span></span> |
| [<span data-ttu-id="e9323-134">Cookie.Domain</span><span class="sxs-lookup"><span data-stu-id="e9323-134">Cookie.Domain</span></span>](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.domain?view=aspnetcore-2.0) | <span data-ttu-id="e9323-135">Der Domänenname, in denen das Cookie bedient wird.</span><span class="sxs-lookup"><span data-stu-id="e9323-135">The domain name where the cookie is served.</span></span> <span data-ttu-id="e9323-136">Standardmäßig ist dies der Hostname der Anforderung.</span><span class="sxs-lookup"><span data-stu-id="e9323-136">By default, this is the host name of the request.</span></span> <span data-ttu-id="e9323-137">Der Browser sendet das Cookie nur bei den Anforderungen auf einen übereinstimmenden Hostnamen.</span><span class="sxs-lookup"><span data-stu-id="e9323-137">The browser only sends the cookie in requests to a matching host name.</span></span> <span data-ttu-id="e9323-138">Sie möchten möglicherweise passen Sie diese Option, um Cookies zu einem Host in Ihrer Domäne verfügen.</span><span class="sxs-lookup"><span data-stu-id="e9323-138">You may wish to adjust this to have cookies available to any host in your domain.</span></span> <span data-ttu-id="e9323-139">Z. B. Festlegen der Domäne des Cookies auf `.contoso.com` zur Verfügung `contoso.com`, `www.contoso.com`, und `staging.www.contoso.com`.</span><span class="sxs-lookup"><span data-stu-id="e9323-139">For example, setting the cookie domain to `.contoso.com` makes it available to `contoso.com`, `www.contoso.com`, and `staging.www.contoso.com`.</span></span> |
| [<span data-ttu-id="e9323-140">Cookie.Expiration</span><span class="sxs-lookup"><span data-stu-id="e9323-140">Cookie.Expiration</span></span>](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.expiration?view=aspnetcore-2.0) | <span data-ttu-id="e9323-141">Ruft ab oder legt die Lebensdauer von Cookies.</span><span class="sxs-lookup"><span data-stu-id="e9323-141">Gets or sets the lifespan of a cookie.</span></span> <span data-ttu-id="e9323-142">Aktuell, dies keine Ops option und veraltete Elemente in ASP.NET Core 2.1 + werden.</span><span class="sxs-lookup"><span data-stu-id="e9323-142">Currently, this option no-ops and will become obsolete in ASP.NET Core 2.1+.</span></span> <span data-ttu-id="e9323-143">Verwenden der `ExpireTimeSpan` Ablauf der Cookies festzulegende Option.</span><span class="sxs-lookup"><span data-stu-id="e9323-143">Use the `ExpireTimeSpan` option to set cookie expiration.</span></span> <span data-ttu-id="e9323-144">Weitere Informationen finden Sie unter [verdeutlichen Verhalten des CookieAuthenticationOptions.Cookie.Expiration](https://github.com/aspnet/Security/issues/1293).</span><span class="sxs-lookup"><span data-stu-id="e9323-144">For more information, see [Clarify behavior of CookieAuthenticationOptions.Cookie.Expiration](https://github.com/aspnet/Security/issues/1293).</span></span> |
| [<span data-ttu-id="e9323-145">Cookie.HttpOnly</span><span class="sxs-lookup"><span data-stu-id="e9323-145">Cookie.HttpOnly</span></span>](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.httponly?view=aspnetcore-2.0) | <span data-ttu-id="e9323-146">Ein Flag, das angibt, ob das Cookie nur an Server zugegriffen werden soll.</span><span class="sxs-lookup"><span data-stu-id="e9323-146">A flag indicating if the cookie should be accessible only to servers.</span></span> <span data-ttu-id="e9323-147">Durch Ändern dieses Werts auf `false` ermöglicht die clientseitige Skripts auf das Cookie zugreifen und möglicherweise Ihrer app das Cookie Diebstahl muss Ihrer app ein [siteübergreifendem Skripting (XSS)](xref:security/cross-site-scripting) Sicherheitsrisiko.</span><span class="sxs-lookup"><span data-stu-id="e9323-147">Changing this value to `false` permits client-side scripts to access the cookie and may open your app to cookie theft should your app have a [Cross-site scripting (XSS)](xref:security/cross-site-scripting) vulnerability.</span></span> <span data-ttu-id="e9323-148">Der Standardwert ist `true`.</span><span class="sxs-lookup"><span data-stu-id="e9323-148">The default value is `true`.</span></span> |
| [<span data-ttu-id="e9323-149">Cookie.Name</span><span class="sxs-lookup"><span data-stu-id="e9323-149">Cookie.Name</span></span>](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.name?view=aspnetcore-2.0) | <span data-ttu-id="e9323-150">Legt den Namen des Cookies.</span><span class="sxs-lookup"><span data-stu-id="e9323-150">Sets the name of the cookie.</span></span> |
| [<span data-ttu-id="e9323-151">Cookie.Path</span><span class="sxs-lookup"><span data-stu-id="e9323-151">Cookie.Path</span></span>](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.path?view=aspnetcore-2.0) | <span data-ttu-id="e9323-152">Zum Isolieren von apps auf dem gleichen Hostnamen verwendet.</span><span class="sxs-lookup"><span data-stu-id="e9323-152">Used to isolate apps running on the same host name.</span></span> <span data-ttu-id="e9323-153">Wenn Sie eine app zu ausgeführt `/app1` und möchten Cookies für diese Anwendung zu beschränken, legen die `CookiePath` Eigenschaft `/app1`.</span><span class="sxs-lookup"><span data-stu-id="e9323-153">If you have an app running at `/app1` and want to restrict cookies to that app, set the `CookiePath` property to `/app1`.</span></span> <span data-ttu-id="e9323-154">Auf diese Weise das Cookie steht nur für Anforderungen an `/app1` und jede app darunter.</span><span class="sxs-lookup"><span data-stu-id="e9323-154">By doing so, the cookie is only available on requests to `/app1` and any app underneath it.</span></span> |
| [<span data-ttu-id="e9323-155">Cookie.SameSite</span><span class="sxs-lookup"><span data-stu-id="e9323-155">Cookie.SameSite</span></span>](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.samesite?view=aspnetcore-2.0) | <span data-ttu-id="e9323-156">Gibt an, ob der Browser das Cookie an demselben Standort Anforderungen nur angefügt werden dürfen (`SameSiteMode.Strict`) oder Cross-Site-Anforderungen, die von sicheren HTTP-Methoden und derselben Website-Anforderungen (`SameSiteMode.Lax`).</span><span class="sxs-lookup"><span data-stu-id="e9323-156">Indicates whether the browser should allow the cookie to be attached to same-site requests only (`SameSiteMode.Strict`) or cross-site requests using safe HTTP methods and same-site requests (`SameSiteMode.Lax`).</span></span> <span data-ttu-id="e9323-157">Bei Festlegung auf `SameSiteMode.None`, der Wert der Cookie-Header nicht festgelegt ist.</span><span class="sxs-lookup"><span data-stu-id="e9323-157">When set to `SameSiteMode.None`, the cookie header value isn't set.</span></span> <span data-ttu-id="e9323-158">Beachten Sie, dass [Cookie-Middleware bewirken die Richtlinie](#cookie-policy-middleware) möglicherweise den Wert überschreiben, die Sie bereitstellen.</span><span class="sxs-lookup"><span data-stu-id="e9323-158">Note that [Cookie Policy Middleware](#cookie-policy-middleware) might overwrite the value that you provide.</span></span> <span data-ttu-id="e9323-159">Um die OAuth-Authentifizierung zu unterstützen, der Standardwert ist `SameSiteMode.Lax`.</span><span class="sxs-lookup"><span data-stu-id="e9323-159">To support OAuth authentication, the default value is `SameSiteMode.Lax`.</span></span> <span data-ttu-id="e9323-160">Weitere Informationen finden Sie unter [OAuth-Authentifizierung aufgrund SameSite Cookierichtlinie unterteilt](https://github.com/aspnet/Security/issues/1231).</span><span class="sxs-lookup"><span data-stu-id="e9323-160">For more information, see [OAuth authentication broken due to SameSite cookie policy](https://github.com/aspnet/Security/issues/1231).</span></span> |
| [<span data-ttu-id="e9323-161">Cookie.SecurePolicy</span><span class="sxs-lookup"><span data-stu-id="e9323-161">Cookie.SecurePolicy</span></span>](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.securepolicy?view=aspnetcore-2.0) | <span data-ttu-id="e9323-162">Ein Flag, das angibt, ob das Cookie auf HTTPS beschränkt werden soll (`CookieSecurePolicy.Always`), HTTP oder HTTPS (`CookieSecurePolicy.None`), oder das gleiche Protokoll wie die Anforderung (`CookieSecurePolicy.SameAsRequest`).</span><span class="sxs-lookup"><span data-stu-id="e9323-162">A flag indicating if the cookie created should be limited to HTTPS (`CookieSecurePolicy.Always`), HTTP or HTTPS (`CookieSecurePolicy.None`), or the same protocol as the request (`CookieSecurePolicy.SameAsRequest`).</span></span> <span data-ttu-id="e9323-163">Der Standardwert ist `CookieSecurePolicy.SameAsRequest`.</span><span class="sxs-lookup"><span data-stu-id="e9323-163">The default value is `CookieSecurePolicy.SameAsRequest`.</span></span> |
| [<span data-ttu-id="e9323-164">DataProtectionProvider</span><span class="sxs-lookup"><span data-stu-id="e9323-164">DataProtectionProvider</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.dataprotectionprovider?view=aspnetcore-2.0) | <span data-ttu-id="e9323-165">Legt die `DataProtectionProvider` , dient zum Erstellen der standardmäßigen `TicketDataFormat`.</span><span class="sxs-lookup"><span data-stu-id="e9323-165">Sets the `DataProtectionProvider` that's used to create the default `TicketDataFormat`.</span></span> <span data-ttu-id="e9323-166">Wenn die `TicketDataFormat` Eigenschaft festgelegt ist, die `DataProtectionProvider` Option wird nicht verwendet.</span><span class="sxs-lookup"><span data-stu-id="e9323-166">If the `TicketDataFormat` property is set, the `DataProtectionProvider` option isn't used.</span></span> <span data-ttu-id="e9323-167">Wenn nicht angegeben, wird die app Datenschutz-Standardanbieter verwendet.</span><span class="sxs-lookup"><span data-stu-id="e9323-167">If not provided, the app's default data protection provider is used.</span></span> |
| [<span data-ttu-id="e9323-168">Ereignisse</span><span class="sxs-lookup"><span data-stu-id="e9323-168">Events</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.events?view=aspnetcore-2.0) | <span data-ttu-id="e9323-169">Der Handler ruft Methoden für den Anbieter, der das app-Steuerelement an bestimmten Punkten Verarbeitung zu ermöglichen.</span><span class="sxs-lookup"><span data-stu-id="e9323-169">The handler calls methods on the provider that give the app control at certain processing points.</span></span> <span data-ttu-id="e9323-170">Wenn `Events` sind nicht angegeben wird, eine Standardinstanz bereitgestellt wird, die keine Aktionen ausführt, wenn die Methoden aufgerufen werden.</span><span class="sxs-lookup"><span data-stu-id="e9323-170">If `Events` aren't provided, a default instance is supplied that does nothing when the methods are called.</span></span> |
| [<span data-ttu-id="e9323-171">EventsType</span><span class="sxs-lookup"><span data-stu-id="e9323-171">EventsType</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.authenticationschemeoptions.eventstype?view=aspnetcore-2.0) | <span data-ttu-id="e9323-172">Als den Diensttyp verwendet, um das Abrufen der `Events` Instanz statt mit der Eigenschaft.</span><span class="sxs-lookup"><span data-stu-id="e9323-172">Used as the service type to get the `Events` instance instead of the property.</span></span> |
| [<span data-ttu-id="e9323-173">ExpireTimeSpan</span><span class="sxs-lookup"><span data-stu-id="e9323-173">ExpireTimeSpan</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.expiretimespan?view=aspnetcore-2.0) | <span data-ttu-id="e9323-174">Die `TimeSpan` nach dem das Authentifizierungsticket gespeichert, in das Cookie abläuft.</span><span class="sxs-lookup"><span data-stu-id="e9323-174">The `TimeSpan` after which the authentication ticket stored inside the cookie expires.</span></span> <span data-ttu-id="e9323-175">`ExpireTimeSpan` wird die aktuelle Uhrzeit auf die Ablaufzeit für das Ticket erstellen hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="e9323-175">`ExpireTimeSpan` is added to the current time to create the expiration time for the ticket.</span></span> <span data-ttu-id="e9323-176">Die `ExpiredTimeSpan` Wert wird immer in der verschlüsselten Authentifizierungsticket, die vom Server überprüft.</span><span class="sxs-lookup"><span data-stu-id="e9323-176">The `ExpiredTimeSpan` value always goes into the encrypted AuthTicket verified by the server.</span></span> <span data-ttu-id="e9323-177">Es kann auch ein Wechsel in den [Set-Cookie](https://tools.ietf.org/html/rfc6265#section-4.1) Header jedoch nur, wenn `IsPersistent` festgelegt ist.</span><span class="sxs-lookup"><span data-stu-id="e9323-177">It may also go into the [Set-Cookie](https://tools.ietf.org/html/rfc6265#section-4.1) header, but only if `IsPersistent` is set.</span></span> <span data-ttu-id="e9323-178">Festzulegende `IsPersistent` zu `true`, konfigurieren Sie die [AuthenticationProperties](/dotnet/api/microsoft.aspnetcore.authentication.authenticationproperties) übergeben `SignInAsync`.</span><span class="sxs-lookup"><span data-stu-id="e9323-178">To set `IsPersistent` to `true`, configure the [AuthenticationProperties](/dotnet/api/microsoft.aspnetcore.authentication.authenticationproperties) passed to `SignInAsync`.</span></span> <span data-ttu-id="e9323-179">Der Standardwert von `ExpireTimeSpan` beträgt 14 Tage.</span><span class="sxs-lookup"><span data-stu-id="e9323-179">The default value of `ExpireTimeSpan` is 14 days.</span></span> |
| [<span data-ttu-id="e9323-180">LoginPath</span><span class="sxs-lookup"><span data-stu-id="e9323-180">LoginPath</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.loginpath?view=aspnetcore-2.0) | <span data-ttu-id="e9323-181">Gibt den Pfad zu einer 302 gefunden (URL-Umleitung) zur Verfügung stellen, wenn von ausgelöst `HttpContext.ChallengeAsync`.</span><span class="sxs-lookup"><span data-stu-id="e9323-181">Provides the path to supply with a 302 Found (URL redirect) when triggered by `HttpContext.ChallengeAsync`.</span></span> <span data-ttu-id="e9323-182">Die aktuelle URL, die den Statuscode 401 generiert wird hinzugefügt, um die `LoginPath` als ein Abfragezeichenfolgen-Parameter mit dem Namen, indem Sie die `ReturnUrlParameter`.</span><span class="sxs-lookup"><span data-stu-id="e9323-182">The current URL that generated the 401 is added to the `LoginPath` as a query string parameter named by the `ReturnUrlParameter`.</span></span> <span data-ttu-id="e9323-183">Einmal eine Anforderung an die `LoginPath` eine neue Identität anmelden gewährt der `ReturnUrlParameter` Wert wird verwendet, um den Browser zurück an die URL umgeleitet, die den ursprünglichen Statuscode nicht autorisiert verursacht hat.</span><span class="sxs-lookup"><span data-stu-id="e9323-183">Once a request to the `LoginPath` grants a new sign-in identity, the `ReturnUrlParameter` value is used to redirect the browser back to the URL that caused the original unauthorized status code.</span></span> <span data-ttu-id="e9323-184">Der Standardwert ist `/Account/Login`.</span><span class="sxs-lookup"><span data-stu-id="e9323-184">The default value is `/Account/Login`.</span></span> |
| [<span data-ttu-id="e9323-185">LogoutPath</span><span class="sxs-lookup"><span data-stu-id="e9323-185">LogoutPath</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.logoutpath?view=aspnetcore-2.0) | <span data-ttu-id="e9323-186">Wenn die `LogoutPath` wird bereitgestellt, um den Handler auf, dann eine Anforderung an diesen Pfad leitet basierend auf dem Wert, der die `ReturnUrlParameter`.</span><span class="sxs-lookup"><span data-stu-id="e9323-186">If the `LogoutPath` is provided to the handler, then a request to that path redirects based on the value of the `ReturnUrlParameter`.</span></span> <span data-ttu-id="e9323-187">Der Standardwert ist `/Account/Logout`.</span><span class="sxs-lookup"><span data-stu-id="e9323-187">The default value is `/Account/Logout`.</span></span> |
| [<span data-ttu-id="e9323-188">ReturnUrlParameter</span><span class="sxs-lookup"><span data-stu-id="e9323-188">ReturnUrlParameter</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.returnurlparameter?view=aspnetcore-2.0) | <span data-ttu-id="e9323-189">Legt den Namen des Abfragezeichenfolgen-Parameters, der durch den Handler für eine 302 gefunden (URL-Umleitung) Antwort angefügt wird.</span><span class="sxs-lookup"><span data-stu-id="e9323-189">Determines the name of the query string parameter that's appended by the handler for a 302 Found (URL redirect) response.</span></span> <span data-ttu-id="e9323-190">`ReturnUrlParameter` wird verwendet, wenn eine Anforderung eingeht, auf die `LoginPath` oder `LogoutPath` den Browser die ursprüngliche URL zurückgegeben, nachdem die an- bzw. Abmeldung Aktion ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="e9323-190">`ReturnUrlParameter` is used when a request arrives on the `LoginPath` or `LogoutPath` to return the browser to the original URL after the login or logout action is performed.</span></span> <span data-ttu-id="e9323-191">Der Standardwert ist `ReturnUrl`.</span><span class="sxs-lookup"><span data-stu-id="e9323-191">The default value is `ReturnUrl`.</span></span> |
| [<span data-ttu-id="e9323-192">SessionStore</span><span class="sxs-lookup"><span data-stu-id="e9323-192">SessionStore</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.sessionstore?view=aspnetcore-2.0) | <span data-ttu-id="e9323-193">Ein optionaler Container verwendet, um die Identität anforderungsübergreifend gespeichert.</span><span class="sxs-lookup"><span data-stu-id="e9323-193">An optional container used to store identity across requests.</span></span> <span data-ttu-id="e9323-194">Wenn verwendet, wird nur ein Sitzungsbezeichner an den Client gesendet.</span><span class="sxs-lookup"><span data-stu-id="e9323-194">When used, only a session identifier is sent to the client.</span></span> <span data-ttu-id="e9323-195">`SessionStore` kann verwendet werden, um potenzielle Probleme mit großen Identitäten zu verringern.</span><span class="sxs-lookup"><span data-stu-id="e9323-195">`SessionStore` can be used to mitigate potential problems with large identities.</span></span> |
| [<span data-ttu-id="e9323-196">SlidingExpiration</span><span class="sxs-lookup"><span data-stu-id="e9323-196">SlidingExpiration</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.slidingexpiration?view=aspnetcore-2.0) | <span data-ttu-id="e9323-197">Ein Flag, das angibt, ob ein neues Cookie mit einer aktualisierten Ablaufzeit dynamisch ausgegeben werden soll.</span><span class="sxs-lookup"><span data-stu-id="e9323-197">A flag indicating if a new cookie with an updated expiration time should be issued dynamically.</span></span> <span data-ttu-id="e9323-198">Dies kann für jede Anforderung vorkommen, in denen das aktuelle Cookie Ablaufdatum mehr als 50 % abgelaufen ist.</span><span class="sxs-lookup"><span data-stu-id="e9323-198">This can happen on any request where the current cookie expiration period is more than 50% expired.</span></span> <span data-ttu-id="e9323-199">Neuen Ablaufdatum vorwärts verschoben wird, werden das aktuelle Datum plus dem `ExpireTimespan`.</span><span class="sxs-lookup"><span data-stu-id="e9323-199">The new expiration date is moved forward to be the current date plus the `ExpireTimespan`.</span></span> <span data-ttu-id="e9323-200">Ein [Cookie absolute Ablaufzeit](xref:security/authentication/cookie#absolute-cookie-expiration) kann festgelegt werden, mithilfe der `AuthenticationProperties` -Klasse beim Aufrufen von `SignInAsync`.</span><span class="sxs-lookup"><span data-stu-id="e9323-200">An [absolute cookie expiration time](xref:security/authentication/cookie#absolute-cookie-expiration) can be set by using the `AuthenticationProperties` class when calling `SignInAsync`.</span></span> <span data-ttu-id="e9323-201">Eine absolute Ablaufzeit kann verbessern die Sicherheit Ihrer App beschränken die Zeitspanne, die das Authentifizierungscookie gültig ist.</span><span class="sxs-lookup"><span data-stu-id="e9323-201">An absolute expiration time can improve the security of your app by limiting the amount of time that the authentication cookie is valid.</span></span> <span data-ttu-id="e9323-202">Der Standardwert ist `true`.</span><span class="sxs-lookup"><span data-stu-id="e9323-202">The default value is `true`.</span></span> |
| [<span data-ttu-id="e9323-203">TicketDataFormat</span><span class="sxs-lookup"><span data-stu-id="e9323-203">TicketDataFormat</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.ticketdataformat?view=aspnetcore-2.0) | <span data-ttu-id="e9323-204">Die `TicketDataFormat` dient zum Schützen und Aufheben des Schutzes der Identität und andere Eigenschaften, die im Cookiewert gespeichert sind.</span><span class="sxs-lookup"><span data-stu-id="e9323-204">The `TicketDataFormat` is used to protect and unprotect the identity and other properties that are stored in the cookie value.</span></span> <span data-ttu-id="e9323-205">Wenn nicht angegeben wird, eine `TicketDataFormat` erstellt, wobei die [DataProtectionProvider](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.dataprotectionprovider?view=aspnetcore-2.0).</span><span class="sxs-lookup"><span data-stu-id="e9323-205">If not provided, a `TicketDataFormat` is created using the [DataProtectionProvider](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.dataprotectionprovider?view=aspnetcore-2.0).</span></span> |
| [<span data-ttu-id="e9323-206">Überprüfen</span><span class="sxs-lookup"><span data-stu-id="e9323-206">Validate</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.authenticationschemeoptions.validate?view=aspnetcore-2.0) | <span data-ttu-id="e9323-207">Methode, die überprüft, dass die Optionen gültig sind.</span><span class="sxs-lookup"><span data-stu-id="e9323-207">Method that checks that the options are valid.</span></span> |

<span data-ttu-id="e9323-208">Legen Sie `CookieAuthenticationOptions` in der Dienstkonfiguration für die Authentifizierung in der `ConfigureServices` Methode:</span><span class="sxs-lookup"><span data-stu-id="e9323-208">Set `CookieAuthenticationOptions` in the service configuration for authentication in the `ConfigureServices` method:</span></span>

```csharp
services.AddAuthentication(CookieAuthenticationDefaults.AuthenticationScheme)
    .AddCookie(options =>
    {
        ...
    });
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="e9323-209">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="e9323-209">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

<span data-ttu-id="e9323-210">ASP.NET Core 1.x verwendet Cookies [Middleware](xref:fundamentals/middleware/index) , die einen Benutzerprinzipal in einem verschlüsselten Cookie serialisiert.</span><span class="sxs-lookup"><span data-stu-id="e9323-210">ASP.NET Core 1.x uses cookie [middleware](xref:fundamentals/middleware/index) that serializes a user principal into an encrypted cookie.</span></span> <span data-ttu-id="e9323-211">Bei nachfolgenden Anforderungen das Cookie wird überprüft, und der Prinzipal neu erstellt und zugewiesen wird die `HttpContext.User` Eigenschaft.</span><span class="sxs-lookup"><span data-stu-id="e9323-211">On subsequent requests, the cookie is validated, and the principal is recreated and assigned to the `HttpContext.User` property.</span></span>

<span data-ttu-id="e9323-212">Installieren der [Microsoft.AspNetCore.Authentication.Cookies](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Cookies/) NuGet-Paket im Projekt.</span><span class="sxs-lookup"><span data-stu-id="e9323-212">Install the [Microsoft.AspNetCore.Authentication.Cookies](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Cookies/) NuGet package in your project.</span></span> <span data-ttu-id="e9323-213">Dieses Paket enthält die Cookie-Middleware.</span><span class="sxs-lookup"><span data-stu-id="e9323-213">This package contains the cookie middleware.</span></span>

<span data-ttu-id="e9323-214">Verwenden der `UseCookieAuthentication` Methode in der `Configure` Methode in Ihrer *Startup.cs* Datei vor dem `UseMvc` oder `UseMvcWithDefaultRoute`:</span><span class="sxs-lookup"><span data-stu-id="e9323-214">Use the `UseCookieAuthentication` method in the `Configure` method in your *Startup.cs* file before `UseMvc` or `UseMvcWithDefaultRoute`:</span></span>

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

<span data-ttu-id="e9323-215">**CookieAuthenticationOptions-Optionen**</span><span class="sxs-lookup"><span data-stu-id="e9323-215">**CookieAuthenticationOptions Options**</span></span>

<span data-ttu-id="e9323-216">Die [CookieAuthenticationOptions](/dotnet/api/Microsoft.AspNetCore.Builder.CookieAuthenticationOptions?view=aspnetcore-1.1) Klasse wird verwendet, um die Anbieteroptionen für die Authentifizierung zu konfigurieren.</span><span class="sxs-lookup"><span data-stu-id="e9323-216">The [CookieAuthenticationOptions](/dotnet/api/Microsoft.AspNetCore.Builder.CookieAuthenticationOptions?view=aspnetcore-1.1) class is used to configure the authentication provider options.</span></span>

| <span data-ttu-id="e9323-217">Option</span><span class="sxs-lookup"><span data-stu-id="e9323-217">Option</span></span> | <span data-ttu-id="e9323-218">Beschreibung</span><span class="sxs-lookup"><span data-stu-id="e9323-218">Description</span></span> |
| ------ | ----------- |
| [<span data-ttu-id="e9323-219">AuthenticationScheme</span><span class="sxs-lookup"><span data-stu-id="e9323-219">AuthenticationScheme</span></span>](/dotnet/api/microsoft.aspnetcore.builder.authenticationoptions.authenticationscheme?view=aspnetcore-1.1) | <span data-ttu-id="e9323-220">Legt das Authentifizierungsschema fest.</span><span class="sxs-lookup"><span data-stu-id="e9323-220">Sets the authentication scheme.</span></span> <span data-ttu-id="e9323-221">`AuthenticationScheme` ist nützlich, wenn mehrere Instanzen von Authentifizierung vorhanden sind und Sie möchten [mit einem bestimmten Schema autorisieren](xref:security/authorization/limitingidentitybyscheme).</span><span class="sxs-lookup"><span data-stu-id="e9323-221">`AuthenticationScheme` is useful when there are multiple instances of authentication and you want to [authorize with a specific scheme](xref:security/authorization/limitingidentitybyscheme).</span></span> <span data-ttu-id="e9323-222">Festlegen der `AuthenticationScheme` zu `CookieAuthenticationDefaults.AuthenticationScheme` "Cookies" der Wert für das Schema enthält.</span><span class="sxs-lookup"><span data-stu-id="e9323-222">Setting the `AuthenticationScheme` to `CookieAuthenticationDefaults.AuthenticationScheme` provides a value of "Cookies" for the scheme.</span></span> <span data-ttu-id="e9323-223">Sie können einen beliebigen Zeichenfolgenwert angeben, der das Schema unterscheidet.</span><span class="sxs-lookup"><span data-stu-id="e9323-223">You can supply any string value that distinguishes the scheme.</span></span> |
| [<span data-ttu-id="e9323-224">AutomaticAuthenticate</span><span class="sxs-lookup"><span data-stu-id="e9323-224">AutomaticAuthenticate</span></span>](/dotnet/api/microsoft.aspnetcore.builder.authenticationoptions.automaticauthenticate?view=aspnetcore-1.1) | <span data-ttu-id="e9323-225">Legt einen Wert aus, um anzugeben, dass die Cookieauthentifizierung für jede Anforderung ausgeführt werden soll, und versuchen, zu überprüfen und serialisierten Prinzipal selbst erstellten zu rekonstruieren.</span><span class="sxs-lookup"><span data-stu-id="e9323-225">Sets a value to indicate that the cookie authentication should run on every request and attempt to validate and reconstruct any serialized principal it created.</span></span> |
| [<span data-ttu-id="e9323-226">AutomaticChallenge</span><span class="sxs-lookup"><span data-stu-id="e9323-226">AutomaticChallenge</span></span>](/dotnet/api/microsoft.aspnetcore.builder.authenticationoptions.automaticchallenge?view=aspnetcore-1.1) | <span data-ttu-id="e9323-227">Bei "true", behandelt die authentifizierungsmiddleware automatische Herausforderungen.</span><span class="sxs-lookup"><span data-stu-id="e9323-227">If true, the authentication middleware handles automatic challenges.</span></span> <span data-ttu-id="e9323-228">Wenn "false", die authentifizierungsmiddleware nur Antworten, wenn dies ausdrücklich angegeben ändert die `AuthenticationScheme`.</span><span class="sxs-lookup"><span data-stu-id="e9323-228">If false, the authentication middleware only alters responses when explicitly indicated by the `AuthenticationScheme`.</span></span> |
| [<span data-ttu-id="e9323-229">ClaimsIssuer</span><span class="sxs-lookup"><span data-stu-id="e9323-229">ClaimsIssuer</span></span>](/dotnet/api/microsoft.aspnetcore.builder.authenticationoptions.claimsissuer?view=aspnetcore-1.1) | <span data-ttu-id="e9323-230">Der Aussteller, verwenden Sie für die [Aussteller](/dotnet/api/system.security.claims.claim.issuer) Eigenschaft für Ansprüche, die von der cookieauthentifizierungsmiddleware erstellt.</span><span class="sxs-lookup"><span data-stu-id="e9323-230">The issuer to use for the [Issuer](/dotnet/api/system.security.claims.claim.issuer) property on any claims created by the cookie authentication middleware.</span></span> |
| [<span data-ttu-id="e9323-231">CookieDomain</span><span class="sxs-lookup"><span data-stu-id="e9323-231">CookieDomain</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.cookiedomain?view=aspnetcore-1.1) | <span data-ttu-id="e9323-232">Der Domänenname, in denen das Cookie bedient wird.</span><span class="sxs-lookup"><span data-stu-id="e9323-232">The domain name where the cookie is served.</span></span> <span data-ttu-id="e9323-233">Standardmäßig ist dies der Hostname der Anforderung.</span><span class="sxs-lookup"><span data-stu-id="e9323-233">By default, this is the host name of the request.</span></span> <span data-ttu-id="e9323-234">Der Browser dient nur das Cookie auf einen übereinstimmenden Hostnamen.</span><span class="sxs-lookup"><span data-stu-id="e9323-234">The browser only serves the cookie to a matching host name.</span></span> <span data-ttu-id="e9323-235">Sie möchten möglicherweise passen Sie diese Option, um Cookies zu einem Host in Ihrer Domäne verfügen.</span><span class="sxs-lookup"><span data-stu-id="e9323-235">You may wish to adjust this to have cookies available to any host in your domain.</span></span> <span data-ttu-id="e9323-236">Z. B. Festlegen der Domäne des Cookies auf `.contoso.com` zur Verfügung `contoso.com`, `www.contoso.com`, und `staging.www.contoso.com`.</span><span class="sxs-lookup"><span data-stu-id="e9323-236">For example, setting the cookie domain to `.contoso.com` makes it available to `contoso.com`, `www.contoso.com`, and `staging.www.contoso.com`.</span></span> |
| [<span data-ttu-id="e9323-237">CookieHttpOnly</span><span class="sxs-lookup"><span data-stu-id="e9323-237">CookieHttpOnly</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.cookiehttponly?view=aspnetcore-1.1) | <span data-ttu-id="e9323-238">Ein Flag, das angibt, ob das Cookie nur an Server zugegriffen werden soll.</span><span class="sxs-lookup"><span data-stu-id="e9323-238">A flag indicating if the cookie should be accessible only to servers.</span></span> <span data-ttu-id="e9323-239">Durch Ändern dieses Werts auf `false` ermöglicht die clientseitige Skripts auf das Cookie zugreifen und möglicherweise Ihrer app das Cookie Diebstahl muss Ihrer app ein [siteübergreifendem Skripting (XSS)](xref:security/cross-site-scripting) Sicherheitsrisiko.</span><span class="sxs-lookup"><span data-stu-id="e9323-239">Changing this value to `false` permits client-side scripts to access the cookie and may open your app to cookie theft should your app have a [Cross-site scripting (XSS)](xref:security/cross-site-scripting) vulnerability.</span></span> <span data-ttu-id="e9323-240">Der Standardwert ist `true`.</span><span class="sxs-lookup"><span data-stu-id="e9323-240">The default value is `true`.</span></span> |
| [<span data-ttu-id="e9323-241">CookiePath</span><span class="sxs-lookup"><span data-stu-id="e9323-241">CookiePath</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.cookiepath?view=aspnetcore-1.1) | <span data-ttu-id="e9323-242">Zum Isolieren von apps auf dem gleichen Hostnamen verwendet.</span><span class="sxs-lookup"><span data-stu-id="e9323-242">Used to isolate apps running on the same host name.</span></span> <span data-ttu-id="e9323-243">Wenn Sie eine app zu ausgeführt `/app1` und möchten Cookies für diese Anwendung zu beschränken, legen die `CookiePath` Eigenschaft `/app1`.</span><span class="sxs-lookup"><span data-stu-id="e9323-243">If you have an app running at `/app1` and want to restrict cookies to that app, set the `CookiePath` property to `/app1`.</span></span> <span data-ttu-id="e9323-244">Auf diese Weise das Cookie steht nur für Anforderungen an `/app1` und jede app darunter.</span><span class="sxs-lookup"><span data-stu-id="e9323-244">By doing so, the cookie is only available on requests to `/app1` and any app underneath it.</span></span> |
| [<span data-ttu-id="e9323-245">CookieSecure</span><span class="sxs-lookup"><span data-stu-id="e9323-245">CookieSecure</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.cookiesecure?view=aspnetcore-1.1) | <span data-ttu-id="e9323-246">Ein Flag, das angibt, ob das Cookie auf HTTPS beschränkt werden soll (`CookieSecurePolicy.Always`), HTTP oder HTTPS (`CookieSecurePolicy.None`), oder das gleiche Protokoll wie die Anforderung (`CookieSecurePolicy.SameAsRequest`).</span><span class="sxs-lookup"><span data-stu-id="e9323-246">A flag indicating if the cookie created should be limited to HTTPS (`CookieSecurePolicy.Always`), HTTP or HTTPS (`CookieSecurePolicy.None`), or the same protocol as the request (`CookieSecurePolicy.SameAsRequest`).</span></span> <span data-ttu-id="e9323-247">Der Standardwert ist `CookieSecurePolicy.SameAsRequest`.</span><span class="sxs-lookup"><span data-stu-id="e9323-247">The default value is `CookieSecurePolicy.SameAsRequest`.</span></span> |
| [<span data-ttu-id="e9323-248">Beschreibung</span><span class="sxs-lookup"><span data-stu-id="e9323-248">Description</span></span>](/dotnet/api/microsoft.aspnetcore.builder.authenticationoptions.description?view=aspnetcore-1.1) | <span data-ttu-id="e9323-249">Weitere Informationen zu den Authentifizierungstyp für die app zur Verfügung gestellt werden.</span><span class="sxs-lookup"><span data-stu-id="e9323-249">Additional information about the authentication type which is made available to the app.</span></span> |
| [<span data-ttu-id="e9323-250">ExpireTimeSpan</span><span class="sxs-lookup"><span data-stu-id="e9323-250">ExpireTimeSpan</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.expiretimespan?view=aspnetcore-1.1) | <span data-ttu-id="e9323-251">Die `TimeSpan` nach dem das Authentifizierungsticket abläuft.</span><span class="sxs-lookup"><span data-stu-id="e9323-251">The `TimeSpan` after which the authentication ticket expires.</span></span> <span data-ttu-id="e9323-252">Es wird die aktuelle Uhrzeit auf die Ablaufzeit für das Ticket erstellen hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="e9323-252">It's added to the current time to create the expiration time for the ticket.</span></span> <span data-ttu-id="e9323-253">Mit `ExpireTimeSpan`, müssen Sie festlegen `IsPersistent` auf `true` in der `AuthenticationProperties` übergeben `SignInAsync`.</span><span class="sxs-lookup"><span data-stu-id="e9323-253">To use `ExpireTimeSpan`, you must set `IsPersistent` to `true` in the `AuthenticationProperties` passed to `SignInAsync`.</span></span> <span data-ttu-id="e9323-254">Der Standardwert beträgt 14 Tage.</span><span class="sxs-lookup"><span data-stu-id="e9323-254">The default value is 14 days.</span></span> |
| [<span data-ttu-id="e9323-255">SlidingExpiration</span><span class="sxs-lookup"><span data-stu-id="e9323-255">SlidingExpiration</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.slidingexpiration?view=aspnetcore-1.1) | <span data-ttu-id="e9323-256">Ein Flag, das angibt, ob das Ablaufdatum der Cookie wird, wenn mehr als die Hälfte der zurückgesetzt der `ExpireTimeSpan` Intervall ist verstrichen.</span><span class="sxs-lookup"><span data-stu-id="e9323-256">A flag indicating whether the cookie expiration date resets when more than half of the `ExpireTimeSpan` interval has passed.</span></span> <span data-ttu-id="e9323-257">Die neue Exipiration Zeit vorwärts verschoben ist, werden das aktuelle Datum plus dem `ExpireTimespan`.</span><span class="sxs-lookup"><span data-stu-id="e9323-257">The new exipiration time is moved forward to be the current date plus the `ExpireTimespan`.</span></span> <span data-ttu-id="e9323-258">Ein [Cookie absolute Ablaufzeit](xref:security/authentication/cookie#absolute-cookie-expiration) kann festgelegt werden, mithilfe der `AuthenticationProperties` -Klasse beim Aufrufen von `SignInAsync`.</span><span class="sxs-lookup"><span data-stu-id="e9323-258">An [absolute cookie expiration time](xref:security/authentication/cookie#absolute-cookie-expiration) can be set by using the `AuthenticationProperties` class when calling `SignInAsync`.</span></span> <span data-ttu-id="e9323-259">Eine absolute Ablaufzeit kann verbessern die Sicherheit Ihrer App beschränken die Zeitspanne, die das Authentifizierungscookie gültig ist.</span><span class="sxs-lookup"><span data-stu-id="e9323-259">An absolute expiration time can improve the security of your app by limiting the amount of time that the authentication cookie is valid.</span></span> <span data-ttu-id="e9323-260">Der Standardwert ist `true`.</span><span class="sxs-lookup"><span data-stu-id="e9323-260">The default value is `true`.</span></span> |

<span data-ttu-id="e9323-261">Legen Sie `CookieAuthenticationOptions` für die Cookieauthentifizierungsmiddleware in die `Configure` Methode:</span><span class="sxs-lookup"><span data-stu-id="e9323-261">Set `CookieAuthenticationOptions` for the Cookie Authentication Middleware in the `Configure` method:</span></span>

```csharp
app.UseCookieAuthentication(new CookieAuthenticationOptions
{
    ...
});
```

---

## <a name="cookie-policy-middleware"></a><span data-ttu-id="e9323-262">Cookie-Middleware bewirken die Richtlinie</span><span class="sxs-lookup"><span data-stu-id="e9323-262">Cookie Policy Middleware</span></span>

<span data-ttu-id="e9323-263">[Cookie-Middleware bewirken die Richtlinie](/dotnet/api/microsoft.aspnetcore.cookiepolicy.cookiepolicymiddleware) Cookie Richtlinie Funktionen in einer app ermöglicht.</span><span class="sxs-lookup"><span data-stu-id="e9323-263">[Cookie Policy Middleware](/dotnet/api/microsoft.aspnetcore.cookiepolicy.cookiepolicymiddleware) enables cookie policy capabilities in an app.</span></span> <span data-ttu-id="e9323-264">Der app-Verarbeitungspipeline die Middleware hinzugefügt ist die Reihenfolge, die vertrauliche; Er wirkt sich nur auf Komponenten, die nach ihm in der Pipeline registriert.</span><span class="sxs-lookup"><span data-stu-id="e9323-264">Adding the middleware to the app processing pipeline is order sensitive; it only affects components registered after it in the pipeline.</span></span>

```csharp
app.UseCookiePolicy(cookiePolicyOptions);
```

 <span data-ttu-id="e9323-265">Die [CookiePolicyOptions](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions) bereitgestellt, um die Cookie-Middleware Richtlinie können Sie globale Eigenschaften der Cookie-Verarbeitung und Hook in Cookie Verarbeitungshandler steuern, wenn Cookies angefügt oder gelöscht werden.</span><span class="sxs-lookup"><span data-stu-id="e9323-265">The [CookiePolicyOptions](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions) provided to the Cookie Policy Middleware allow you to control global characteristics of cookie processing and hook into cookie processing handlers when cookies are appended or deleted.</span></span>

| <span data-ttu-id="e9323-266">Eigenschaft</span><span class="sxs-lookup"><span data-stu-id="e9323-266">Property</span></span> | <span data-ttu-id="e9323-267">Beschreibung</span><span class="sxs-lookup"><span data-stu-id="e9323-267">Description</span></span> |
| -------- | ----------- |
| [<span data-ttu-id="e9323-268">HttpOnly</span><span class="sxs-lookup"><span data-stu-id="e9323-268">HttpOnly</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions.httponly) | <span data-ttu-id="e9323-269">Wirkt sich auf, ob Cookies HttpOnly sein müssen, die ein Flag, der angibt, wenn das Cookie nur an Server zugegriffen werden soll.</span><span class="sxs-lookup"><span data-stu-id="e9323-269">Affects whether cookies must be HttpOnly, which is a flag indicating if the cookie should be accessible only to servers.</span></span> <span data-ttu-id="e9323-270">Der Standardwert ist `HttpOnlyPolicy.None`.</span><span class="sxs-lookup"><span data-stu-id="e9323-270">The default value is `HttpOnlyPolicy.None`.</span></span> |
| [<span data-ttu-id="e9323-271">MinimumSameSitePolicy</span><span class="sxs-lookup"><span data-stu-id="e9323-271">MinimumSameSitePolicy</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions.minimumsamesitepolicy) | <span data-ttu-id="e9323-272">Wirkt sich auf das Cookie gleicher Attribut (siehe unten).</span><span class="sxs-lookup"><span data-stu-id="e9323-272">Affects the cookie's same-site attribute (see below).</span></span> <span data-ttu-id="e9323-273">Der Standardwert ist `SameSiteMode.Lax`.</span><span class="sxs-lookup"><span data-stu-id="e9323-273">The default value is `SameSiteMode.Lax`.</span></span> <span data-ttu-id="e9323-274">Diese Option ist verfügbar für ASP.NET Core 2.0 +.</span><span class="sxs-lookup"><span data-stu-id="e9323-274">This option is available for ASP.NET Core 2.0+.</span></span> |
| [<span data-ttu-id="e9323-275">OnAppendCookie</span><span class="sxs-lookup"><span data-stu-id="e9323-275">OnAppendCookie</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions.onappendcookie) | <span data-ttu-id="e9323-276">Wird aufgerufen, wenn ein Cookie angehängt wird.</span><span class="sxs-lookup"><span data-stu-id="e9323-276">Called when a cookie is appended.</span></span> |
| [<span data-ttu-id="e9323-277">OnDeleteCookie</span><span class="sxs-lookup"><span data-stu-id="e9323-277">OnDeleteCookie</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions.ondeletecookie) | <span data-ttu-id="e9323-278">Wird aufgerufen, wenn ein Cookie gelöscht wird.</span><span class="sxs-lookup"><span data-stu-id="e9323-278">Called when a cookie is deleted.</span></span> |
| [<span data-ttu-id="e9323-279">Sichern</span><span class="sxs-lookup"><span data-stu-id="e9323-279">Secure</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions.secure) | <span data-ttu-id="e9323-280">Bestimmt, ob Cookies Secure sein müssen.</span><span class="sxs-lookup"><span data-stu-id="e9323-280">Affects whether cookies must be Secure.</span></span> <span data-ttu-id="e9323-281">Der Standardwert ist `CookieSecurePolicy.None`.</span><span class="sxs-lookup"><span data-stu-id="e9323-281">The default value is `CookieSecurePolicy.None`.</span></span> |

<span data-ttu-id="e9323-282">**MinimumSameSitePolicy** (ASP.NET 2.0 + nur Kern)</span><span class="sxs-lookup"><span data-stu-id="e9323-282">**MinimumSameSitePolicy** (ASP.NET Core 2.0+ only)</span></span>

<span data-ttu-id="e9323-283">Die Standardeinstellung `MinimumSameSitePolicy` Wert `SameSiteMode.Lax` "oauth2" Authentifizierung zulassen.</span><span class="sxs-lookup"><span data-stu-id="e9323-283">The default `MinimumSameSitePolicy` value is `SameSiteMode.Lax` to permit OAuth2 authentication.</span></span> <span data-ttu-id="e9323-284">Um eine Richtlinie gleichen Standort vom streng erzwingen `SameSiteMode.Strict`legen die `MinimumSameSitePolicy`.</span><span class="sxs-lookup"><span data-stu-id="e9323-284">To strictly enforce a same-site policy of `SameSiteMode.Strict`, set the `MinimumSameSitePolicy`.</span></span> <span data-ttu-id="e9323-285">Obwohl diese Einstellung "oauth2" und andere Cross-Origin-Authentifizierungsschemas unterbrochen wird, erhöht die Cookies für andere Arten von apps, die Cross-Origin-anforderungsverarbeitung benötigen keine Sicherheitsebene.</span><span class="sxs-lookup"><span data-stu-id="e9323-285">Although this setting breaks OAuth2 and other cross-origin authentication schemes, it elevates the level of cookie security for other types of apps that don't rely on cross-origin request processing.</span></span>

```csharp
var cookiePolicyOptions = new CookiePolicyOptions
{
    MinimumSameSitePolicy = SameSiteMode.Strict,
};
```

<span data-ttu-id="e9323-286">Die Einstellung der Cookie-Middleware bewirken die Richtlinie für `MinimumSameSitePolicy` kann Auswirkungen auf die Einstellung für `Cookie.SameSite` in `CookieAuthenticationOptions` Einstellungen entsprechend der Matrix unten.</span><span class="sxs-lookup"><span data-stu-id="e9323-286">The Cookie Policy Middleware setting for `MinimumSameSitePolicy` can affect your setting of `Cookie.SameSite` in `CookieAuthenticationOptions` settings according to the matrix below.</span></span>

| <span data-ttu-id="e9323-287">MinimumSameSitePolicy</span><span class="sxs-lookup"><span data-stu-id="e9323-287">MinimumSameSitePolicy</span></span> | <span data-ttu-id="e9323-288">Cookie.SameSite</span><span class="sxs-lookup"><span data-stu-id="e9323-288">Cookie.SameSite</span></span> | <span data-ttu-id="e9323-289">Resultierende Cookie.SameSite-Einstellung</span><span class="sxs-lookup"><span data-stu-id="e9323-289">Resultant Cookie.SameSite setting</span></span> |
| --------------------- | --------------- | --------------------------------- |
| <span data-ttu-id="e9323-290">SameSiteMode.None</span><span class="sxs-lookup"><span data-stu-id="e9323-290">SameSiteMode.None</span></span>     | <span data-ttu-id="e9323-291">SameSiteMode.None</span><span class="sxs-lookup"><span data-stu-id="e9323-291">SameSiteMode.None</span></span><br><span data-ttu-id="e9323-292">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="e9323-292">SameSiteMode.Lax</span></span><br><span data-ttu-id="e9323-293">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="e9323-293">SameSiteMode.Strict</span></span> | <span data-ttu-id="e9323-294">SameSiteMode.None</span><span class="sxs-lookup"><span data-stu-id="e9323-294">SameSiteMode.None</span></span><br><span data-ttu-id="e9323-295">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="e9323-295">SameSiteMode.Lax</span></span><br><span data-ttu-id="e9323-296">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="e9323-296">SameSiteMode.Strict</span></span> |
| <span data-ttu-id="e9323-297">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="e9323-297">SameSiteMode.Lax</span></span>      | <span data-ttu-id="e9323-298">SameSiteMode.None</span><span class="sxs-lookup"><span data-stu-id="e9323-298">SameSiteMode.None</span></span><br><span data-ttu-id="e9323-299">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="e9323-299">SameSiteMode.Lax</span></span><br><span data-ttu-id="e9323-300">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="e9323-300">SameSiteMode.Strict</span></span> | <span data-ttu-id="e9323-301">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="e9323-301">SameSiteMode.Lax</span></span><br><span data-ttu-id="e9323-302">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="e9323-302">SameSiteMode.Lax</span></span><br><span data-ttu-id="e9323-303">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="e9323-303">SameSiteMode.Strict</span></span> |
| <span data-ttu-id="e9323-304">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="e9323-304">SameSiteMode.Strict</span></span>   | <span data-ttu-id="e9323-305">SameSiteMode.None</span><span class="sxs-lookup"><span data-stu-id="e9323-305">SameSiteMode.None</span></span><br><span data-ttu-id="e9323-306">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="e9323-306">SameSiteMode.Lax</span></span><br><span data-ttu-id="e9323-307">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="e9323-307">SameSiteMode.Strict</span></span> | <span data-ttu-id="e9323-308">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="e9323-308">SameSiteMode.Strict</span></span><br><span data-ttu-id="e9323-309">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="e9323-309">SameSiteMode.Strict</span></span><br><span data-ttu-id="e9323-310">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="e9323-310">SameSiteMode.Strict</span></span> |

## <a name="create-an-authentication-cookie"></a><span data-ttu-id="e9323-311">Erstellen Sie ein Authentifizierungscookie</span><span class="sxs-lookup"><span data-stu-id="e9323-311">Create an authentication cookie</span></span>

<span data-ttu-id="e9323-312">Um ein Cookie mit Benutzerinformationen zu erstellen, erstellen Sie eine [ClaimsPrincipal](/dotnet/api/system.security.claims.claimsprincipal).</span><span class="sxs-lookup"><span data-stu-id="e9323-312">To create a cookie holding user information, you must construct a [ClaimsPrincipal](/dotnet/api/system.security.claims.claimsprincipal).</span></span> <span data-ttu-id="e9323-313">Die Benutzerinformationen serialisiert und im Cookie gespeichert.</span><span class="sxs-lookup"><span data-stu-id="e9323-313">The user information is serialized and stored in the cookie.</span></span> 

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="e9323-314">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="e9323-314">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

<span data-ttu-id="e9323-315">Erstellen einer ["ClaimsIdentity"](/dotnet/api/system.security.claims.claimsidentity) mit allen erforderlichen [Anspruch](/dotnet/api/system.security.claims.claim)s, und rufen [SignInAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhttpcontextextensions.signinasync?view=aspnetcore-2.0) der Benutzer zur Anmeldung:</span><span class="sxs-lookup"><span data-stu-id="e9323-315">Create a [ClaimsIdentity](/dotnet/api/system.security.claims.claimsidentity) with any required [Claim](/dotnet/api/system.security.claims.claim)s and call [SignInAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhttpcontextextensions.signinasync?view=aspnetcore-2.0) to sign in the user:</span></span>

[!code-csharp[](cookie/samples/2.x/CookieSample/Pages/Account/Login.cshtml.cs?name=snippet1)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="e9323-316">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="e9323-316">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

<span data-ttu-id="e9323-317">Rufen Sie [SignInAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhandler-1.signinasync?view=aspnetcore-1.1) der Benutzer zur Anmeldung:</span><span class="sxs-lookup"><span data-stu-id="e9323-317">Call [SignInAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhandler-1.signinasync?view=aspnetcore-1.1) to sign in the user:</span></span>

```csharp
await HttpContext.Authentication.SignInAsync(
    CookieAuthenticationDefaults.AuthenticationScheme,
    new ClaimsPrincipal(claimsIdentity));
```

---

<span data-ttu-id="e9323-318">`SignInAsync` ein verschlüsseltes Cookies erstellt, und der aktuellen Antwort hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="e9323-318">`SignInAsync` creates an encrypted cookie and adds it to the current response.</span></span> <span data-ttu-id="e9323-319">Wenn Sie nicht angeben einer `AuthenticationScheme`, wird das Standardschema verwendet.</span><span class="sxs-lookup"><span data-stu-id="e9323-319">If you don't specify an `AuthenticationScheme`, the default scheme is used.</span></span>

<span data-ttu-id="e9323-320">Im Hintergrund, wird die Verschlüsselung verwendet ASP.NET Core des [Datenschutz](xref:security/data-protection/using-data-protection#security-data-protection-getting-started) System.</span><span class="sxs-lookup"><span data-stu-id="e9323-320">Under the covers, the encryption used is ASP.NET Core's [Data Protection](xref:security/data-protection/using-data-protection#security-data-protection-getting-started) system.</span></span> <span data-ttu-id="e9323-321">Wenn Sie sind auf mehreren Computern, den Lastenausgleich für apps oder mit einer Webfarm app hosten, müssen Sie [Schutz von Daten konfigurieren](xref:security/data-protection/configuration/overview) und mit dem gleichen Schlüssel Ring app-Bezeichner.</span><span class="sxs-lookup"><span data-stu-id="e9323-321">If you're hosting app on multiple machines, load balancing across apps, or using a web farm, then you must [configure data protection](xref:security/data-protection/configuration/overview) to use the same key ring and app identifier.</span></span>

## <a name="sign-out"></a><span data-ttu-id="e9323-322">Abmelden</span><span class="sxs-lookup"><span data-stu-id="e9323-322">Sign out</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="e9323-323">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="e9323-323">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

<span data-ttu-id="e9323-324">Rufen Sie den aktuellen Benutzer abmelden, und löschen ihre Cookies, [SignOutAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhttpcontextextensions.signoutasync?view=aspnetcore-2.0):</span><span class="sxs-lookup"><span data-stu-id="e9323-324">To sign out the current user and delete their cookie, call [SignOutAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhttpcontextextensions.signoutasync?view=aspnetcore-2.0):</span></span>

[!code-csharp[](cookie/samples/2.x/CookieSample/Pages/Account/Login.cshtml.cs?name=snippet2)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="e9323-325">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="e9323-325">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

<span data-ttu-id="e9323-326">Rufen Sie den aktuellen Benutzer abmelden, und löschen ihre Cookies, [SignOutAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhandler-1.signoutasync?view=aspnetcore-1.1):</span><span class="sxs-lookup"><span data-stu-id="e9323-326">To sign out the current user and delete their cookie, call [SignOutAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhandler-1.signoutasync?view=aspnetcore-1.1):</span></span>

```csharp
await HttpContext.Authentication.SignOutAsync(
    CookieAuthenticationDefaults.AuthenticationScheme);
```

---

<span data-ttu-id="e9323-327">Wenn Sie nicht `CookieAuthenticationDefaults.AuthenticationScheme` (oder "Cookies") als Formate (z. B. "ContosoCookie"), geben Sie das Schema, das Sie beim Konfigurieren des Authentifizierungsanbieters verwendet.</span><span class="sxs-lookup"><span data-stu-id="e9323-327">If you aren't using `CookieAuthenticationDefaults.AuthenticationScheme` (or "Cookies") as the scheme (for example, "ContosoCookie"), supply the scheme you used when configuring the authentication provider.</span></span> <span data-ttu-id="e9323-328">Andernfalls ist das Standardschema verwendet.</span><span class="sxs-lookup"><span data-stu-id="e9323-328">Otherwise, the default scheme is used.</span></span>

## <a name="react-to-back-end-changes"></a><span data-ttu-id="e9323-329">Reagieren Sie auf Back-End-Änderungen</span><span class="sxs-lookup"><span data-stu-id="e9323-329">React to back-end changes</span></span>

<span data-ttu-id="e9323-330">Sobald ein Cookie erstellt wurde, wird es die einzige Quelle der Identität.</span><span class="sxs-lookup"><span data-stu-id="e9323-330">Once a cookie is created, it becomes the single source of identity.</span></span> <span data-ttu-id="e9323-331">Auch wenn Sie einen Benutzer in Ihrem Back-End-Systemen deaktivieren, die Cookie-Authentifizierungssystem hat keine Informationen über diese, und ein Benutzer angemeldet bleibt, solange ihre Cookie gültig ist.</span><span class="sxs-lookup"><span data-stu-id="e9323-331">Even if you disable a user in your back-end systems, the cookie authentication system has no knowledge of this, and a user stays logged in as long as their cookie is valid.</span></span>

<span data-ttu-id="e9323-332">Die [ValidatePrincipal](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationevents.validateprincipal) Ereignis in ASP.NET Core 2.x oder [ValidateAsync](/dotnet/api/microsoft.aspnetcore.identity.isecuritystampvalidator.validateasync?view=aspnetcore-1.1) Methode in ASP.NET Core 1.x abfangen und Überschreiben der Überprüfung der Identität des Cookies verwendet werden können.</span><span class="sxs-lookup"><span data-stu-id="e9323-332">The [ValidatePrincipal](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationevents.validateprincipal) event in ASP.NET Core 2.x or the [ValidateAsync](/dotnet/api/microsoft.aspnetcore.identity.isecuritystampvalidator.validateasync?view=aspnetcore-1.1) method in ASP.NET Core 1.x can be used to intercept and override validation of the cookie identity.</span></span> <span data-ttu-id="e9323-333">Dieser Ansatz reduziert das Risiko gesperrte Benutzer Zugriff auf die app.</span><span class="sxs-lookup"><span data-stu-id="e9323-333">This approach mitigates the risk of revoked users accessing the app.</span></span>

<span data-ttu-id="e9323-334">Ein Ansatz für die gültigkeitsprüfung für Cookies basiert auf Nachverfolgen der bei die Datenbank geändert wurde.</span><span class="sxs-lookup"><span data-stu-id="e9323-334">One approach to cookie validation is based on keeping track of when the user database has been changed.</span></span> <span data-ttu-id="e9323-335">Wenn die Datenbank nicht geändert wurde, da der Benutzer Cookie ausgestellt wurde, besteht keine Notwendigkeit, den Benutzer erneut zu authentifizieren, wenn ihre Cookie noch gültig ist.</span><span class="sxs-lookup"><span data-stu-id="e9323-335">If the database hasn't been changed since the user's cookie was issued, there's no need to re-authenticate the user if their cookie is still valid.</span></span> <span data-ttu-id="e9323-336">Zum Implementieren dieses Szenarios, die Datenbank, die in implementiert wird `IUserRepository` in diesem Beispiel speichert ein `LastChanged` Wert.</span><span class="sxs-lookup"><span data-stu-id="e9323-336">To implement this scenario, the database, which is implemented in `IUserRepository` for this example, stores a `LastChanged` value.</span></span> <span data-ttu-id="e9323-337">Wenn jeder Benutzer in der Datenbank aktualisiert wird der `LastChanged` Wert auf die aktuelle Uhrzeit festgelegt wird.</span><span class="sxs-lookup"><span data-stu-id="e9323-337">When any user is updated in the database, the `LastChanged` value is set to the current time.</span></span>

<span data-ttu-id="e9323-338">Um ein Cookie für ungültig zu erklären, wenn Änderungen an der Datenbank auf Grundlage der `LastChanged` -Wert, erstellen Sie das Cookie mit einer `LastChanged` Anspruch mit dem aktuellen `LastChanged` Wert aus der Datenbank:</span><span class="sxs-lookup"><span data-stu-id="e9323-338">In order to invalidate a cookie when the database changes based on the `LastChanged` value, create the cookie with a `LastChanged` claim containing the current `LastChanged` value from the database:</span></span>

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

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="e9323-339">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="e9323-339">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="e9323-340">Implementieren Sie eine Außerkraftsetzung für die `ValidatePrincipal` Ereignis, Schreiben Sie eine Methode mit der folgenden Signatur in einer Klasse, die Sie ableiten [CookieAuthenticationEvents](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationevents):</span><span class="sxs-lookup"><span data-stu-id="e9323-340">To implement an override for the `ValidatePrincipal` event, write a method with the following signature in a class that you derive from [CookieAuthenticationEvents](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationevents):</span></span>

```csharp
ValidatePrincipal(CookieValidatePrincipalContext)
```

<span data-ttu-id="e9323-341">Ein Beispiel sieht wie folgt aus:</span><span class="sxs-lookup"><span data-stu-id="e9323-341">An example looks like the following:</span></span>

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

<span data-ttu-id="e9323-342">Registrieren die Instanz Ereignisse während der Cookie-Service-Registrierung in der `ConfigureServices` Methode.</span><span class="sxs-lookup"><span data-stu-id="e9323-342">Register the events instance during cookie service registration in the `ConfigureServices` method.</span></span> <span data-ttu-id="e9323-343">Geben Sie eine Bereichsbezogene Service-Registrierung für Ihre `CustomCookieAuthenticationEvents` Klasse:</span><span class="sxs-lookup"><span data-stu-id="e9323-343">Provide a scoped service registration for your `CustomCookieAuthenticationEvents` class:</span></span>

```csharp
services.AddAuthentication(CookieAuthenticationDefaults.AuthenticationScheme)
    .AddCookie(options =>
    {
        options.EventsType = typeof(CustomCookieAuthenticationEvents);
    });

services.AddScoped<CustomCookieAuthenticationEvents>();
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="e9323-344">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="e9323-344">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="e9323-345">Implementieren Sie eine Außerkraftsetzung für die `ValidateAsync` Ereignis, Schreiben Sie eine Methode mit der folgenden Signatur:</span><span class="sxs-lookup"><span data-stu-id="e9323-345">To implement an override for the `ValidateAsync` event, write a method with the following signature:</span></span>

```csharp
ValidateAsync(CookieValidatePrincipalContext)
```

<span data-ttu-id="e9323-346">ASP.NET Core Identity implementiert diese Überprüfung als Teil seiner [SecurityStampValidator](/dotnet/api/microsoft.aspnetcore.identity.securitystampvalidator-1.validateasync).</span><span class="sxs-lookup"><span data-stu-id="e9323-346">ASP.NET Core Identity implements this check as part of its [SecurityStampValidator](/dotnet/api/microsoft.aspnetcore.identity.securitystampvalidator-1.validateasync).</span></span> <span data-ttu-id="e9323-347">Ein Beispiel sieht wie folgt aus:</span><span class="sxs-lookup"><span data-stu-id="e9323-347">An example looks like the following:</span></span>

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

<span data-ttu-id="e9323-348">Registrieren Sie das Ereignis während der Konfiguration der Cookie-Authentifizierung in der `Configure` Methode:</span><span class="sxs-lookup"><span data-stu-id="e9323-348">Register the event during cookie authentication configuration in the `Configure` method:</span></span>

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

<span data-ttu-id="e9323-349">Betrachten Sie eine Situation, in dem der Name des Benutzers aktualisiert &mdash; eine Entscheidung, die Sicherheit in keiner Weise beeinflussen nicht.</span><span class="sxs-lookup"><span data-stu-id="e9323-349">Consider a situation in which the user's name is updated &mdash; a decision that doesn't affect security in any way.</span></span> <span data-ttu-id="e9323-350">Wenn Sie den Benutzerprinzipalnamen zerstörungsfrei aktualisieren möchten, rufen Sie `context.ReplacePrincipal` und legen Sie die `context.ShouldRenew` Eigenschaft `true`.</span><span class="sxs-lookup"><span data-stu-id="e9323-350">If you want to non-destructively update the user principal, call `context.ReplacePrincipal` and set the `context.ShouldRenew` property to `true`.</span></span>

> [!WARNING]
> <span data-ttu-id="e9323-351">Die hier beschriebene Vorgehensweise wird bei jeder Anforderung ausgelöst.</span><span class="sxs-lookup"><span data-stu-id="e9323-351">The approach described here is triggered on every request.</span></span> <span data-ttu-id="e9323-352">Dies kann in großen Leistungseinbußen bei der app führen.</span><span class="sxs-lookup"><span data-stu-id="e9323-352">This can result in a large performance penalty for the app.</span></span>

## <a name="persistent-cookies"></a><span data-ttu-id="e9323-353">Permanente cookies</span><span class="sxs-lookup"><span data-stu-id="e9323-353">Persistent cookies</span></span>

<span data-ttu-id="e9323-354">Sie sollten das Cookie über Browsersitzungen hinweg beibehalten werden.</span><span class="sxs-lookup"><span data-stu-id="e9323-354">You may want the cookie to persist across browser sessions.</span></span> <span data-ttu-id="e9323-355">Dieser Persistenz sollte nur mit expliziten benutzerzustimmung mit einem Kontrollkästchen "Anmeldedaten" auf den Anmeldenamen oder einen ähnlichen Mechanismus aktiviert werden.</span><span class="sxs-lookup"><span data-stu-id="e9323-355">This persistence should only be enabled with explicit user consent with a "Remember Me" checkbox on login or a similar mechanism.</span></span> 

<span data-ttu-id="e9323-356">Der folgende Codeausschnitt erstellt eine Identität und die entsprechenden Cookie, das über Browser Abschlüsse überdauert.</span><span class="sxs-lookup"><span data-stu-id="e9323-356">The following code snippet creates an identity and corresponding cookie that survives through browser closures.</span></span> <span data-ttu-id="e9323-357">Alle gleitenden ablaufeinstellungen, die zuvor konfiguriert haben, werden berücksichtigt.</span><span class="sxs-lookup"><span data-stu-id="e9323-357">Any sliding expiration settings previously configured are honored.</span></span> <span data-ttu-id="e9323-358">Wenn das Cookie abläuft, während der Browser geschlossen wird, löscht der Browser das Cookie an, nach dem Neustart wird.</span><span class="sxs-lookup"><span data-stu-id="e9323-358">If the cookie expires while the browser is closed, the browser clears the cookie once it's restarted.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="e9323-359">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="e9323-359">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
await HttpContext.SignInAsync(
    CookieAuthenticationDefaults.AuthenticationScheme,
    new ClaimsPrincipal(claimsIdentity),
    new AuthenticationProperties
    {
        IsPersistent = true
    });
```

<span data-ttu-id="e9323-360">Die [AuthenticationProperties](/dotnet/api/microsoft.aspnetcore.authentication.authenticationproperties?view=aspnetcore-2.0) Klasse befindet sich in der `Microsoft.AspNetCore.Authentication` Namespace.</span><span class="sxs-lookup"><span data-stu-id="e9323-360">The [AuthenticationProperties](/dotnet/api/microsoft.aspnetcore.authentication.authenticationproperties?view=aspnetcore-2.0) class resides in the `Microsoft.AspNetCore.Authentication` namespace.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="e9323-361">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="e9323-361">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
await HttpContext.Authentication.SignInAsync(
    CookieAuthenticationDefaults.AuthenticationScheme,
    new ClaimsPrincipal(claimsIdentity),
    new AuthenticationProperties
    {
        IsPersistent = true
    });
```

<span data-ttu-id="e9323-362">Die [AuthenticationProperties](/dotnet/api/microsoft.aspnetcore.http.authentication.authenticationproperties?view=aspnetcore-1.1) Klasse befindet sich in der `Microsoft.AspNetCore.Http.Authentication` Namespace.</span><span class="sxs-lookup"><span data-stu-id="e9323-362">The [AuthenticationProperties](/dotnet/api/microsoft.aspnetcore.http.authentication.authenticationproperties?view=aspnetcore-1.1) class resides in the `Microsoft.AspNetCore.Http.Authentication` namespace.</span></span>

---

## <a name="absolute-cookie-expiration"></a><span data-ttu-id="e9323-363">Absolute cookieablauf</span><span class="sxs-lookup"><span data-stu-id="e9323-363">Absolute cookie expiration</span></span>

<span data-ttu-id="e9323-364">Sie können festlegen, dass eine absolute Ablaufzeit mit `ExpiresUtc`.</span><span class="sxs-lookup"><span data-stu-id="e9323-364">You can set an absolute expiration time with `ExpiresUtc`.</span></span> <span data-ttu-id="e9323-365">Sie müssen auch festlegen `IsPersistent`ist, andernfalls `ExpiresUtc` wird ignoriert, und ein einzelnes Sitzungscookie wird erstellt.</span><span class="sxs-lookup"><span data-stu-id="e9323-365">You must also set `IsPersistent`; otherwise, `ExpiresUtc` is ignored and a single-session cookie is created.</span></span> <span data-ttu-id="e9323-366">Wenn `ExpiresUtc` auf festgelegt ist `SignInAsync`, er überschreibt den Wert für die `ExpireTimeSpan` -Option von `CookieAuthenticationOptions`, sofern festgelegt.</span><span class="sxs-lookup"><span data-stu-id="e9323-366">When `ExpiresUtc` is set on `SignInAsync`, it overrides the value of the `ExpireTimeSpan` option of `CookieAuthenticationOptions`, if set.</span></span>

<span data-ttu-id="e9323-367">Der folgende Codeausschnitt erstellt eine Identität und die entsprechenden Cookie, das 20 Minuten dauert.</span><span class="sxs-lookup"><span data-stu-id="e9323-367">The following code snippet creates an identity and corresponding cookie that lasts for 20 minutes.</span></span> <span data-ttu-id="e9323-368">Dies ignoriert alle gleitenden ablaufeinstellungen, die zuvor konfiguriert haben.</span><span class="sxs-lookup"><span data-stu-id="e9323-368">This ignores any sliding expiration settings previously configured.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="e9323-369">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="e9323-369">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

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

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="e9323-370">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="e9323-370">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

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

## <a name="additional-resources"></a><span data-ttu-id="e9323-371">Zusätzliche Ressourcen</span><span class="sxs-lookup"><span data-stu-id="e9323-371">Additional resources</span></span>

* [<span data-ttu-id="e9323-372">Auth 2.0 Änderungen / Migration Ankündigung</span><span class="sxs-lookup"><span data-stu-id="e9323-372">Auth 2.0 Changes / Migration Announcement</span></span>](https://github.com/aspnet/Announcements/issues/262)
* [<span data-ttu-id="e9323-373">Einschränken der Identität nach Schema</span><span class="sxs-lookup"><span data-stu-id="e9323-373">Limit identity by scheme</span></span>](xref:security/authorization/limitingidentitybyscheme)
* [<span data-ttu-id="e9323-374">Anspruchsbasierte Autorisierung</span><span class="sxs-lookup"><span data-stu-id="e9323-374">Claims-Based Authorization</span></span>](xref:security/authorization/claims)
* [<span data-ttu-id="e9323-375">Mittels richtlinienbasierter rollenüberprüfungen</span><span class="sxs-lookup"><span data-stu-id="e9323-375">Policy-based role checks</span></span>](xref:security/authorization/roles#policy-based-role-checks)
