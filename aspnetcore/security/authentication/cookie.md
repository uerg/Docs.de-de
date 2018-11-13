---
title: Verwenden der Cookieauthentifizierung ohne ASP.NET Core Identity
author: rick-anderson
description: Eine Erläuterung der verwenden der Cookieauthentifizierung ohne ASP.NET Core Identity
ms.author: riande
ms.date: 10/11/2017
uid: security/authentication/cookie
ms.openlocfilehash: f55b36cf3fc3b60e9d592348625f58ebaba90da7
ms.sourcegitcommit: 408921a932448f66cb46fd53c307a864f5323fe5
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/12/2018
ms.locfileid: "51570112"
---
# <a name="use-cookie-authentication-without-aspnet-core-identity"></a><span data-ttu-id="49a0c-103">Verwenden der Cookieauthentifizierung ohne ASP.NET Core Identity</span><span class="sxs-lookup"><span data-stu-id="49a0c-103">Use cookie authentication without ASP.NET Core Identity</span></span>

<span data-ttu-id="49a0c-104">Durch [Rick Anderson](https://twitter.com/RickAndMSFT) und [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="49a0c-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="49a0c-105">Wie Sie in den früheren Authentifizierungsthemen gesehen haben [ASP.NET Core Identity](xref:security/authentication/identity) ist ein vollständige, voll ausgestattete Authentication-Anbieter zum Erstellen und Verwalten von Anmeldungen.</span><span class="sxs-lookup"><span data-stu-id="49a0c-105">As you've seen in the earlier authentication topics, [ASP.NET Core Identity](xref:security/authentication/identity) is a complete, full-featured authentication provider for creating and maintaining logins.</span></span> <span data-ttu-id="49a0c-106">Möglicherweise möchten jedoch Ihre eigene Logik für die benutzerdefinierte Authentifizierung mit gelegentlich cookiebasierte Authentifizierung zu verwenden.</span><span class="sxs-lookup"><span data-stu-id="49a0c-106">However, you may want to use your own custom authentication logic with cookie-based authentication at times.</span></span> <span data-ttu-id="49a0c-107">Sie können als eigenständige Authentifizierungsanbieter ohne ASP.NET Core Identity cookiebasierte Authentifizierung.</span><span class="sxs-lookup"><span data-stu-id="49a0c-107">You can use cookie-based authentication as a standalone authentication provider without ASP.NET Core Identity.</span></span>

<span data-ttu-id="49a0c-108">[Anzeigen oder Herunterladen von Beispielcode](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authentication/cookie/samples) ([Vorgehensweise zum Herunterladen](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="49a0c-108">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authentication/cookie/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="49a0c-109">Zu Demonstrationszwecken in der Beispiel-app ist das Benutzerkonto für den hypothetischen Benutzer Maria Rodriguez, in der app hartcodiert.</span><span class="sxs-lookup"><span data-stu-id="49a0c-109">For demonstration purposes in the sample app, the user account for the hypothetical user, Maria Rodriguez, is hardcoded into the app.</span></span> <span data-ttu-id="49a0c-110">Verwenden Sie den Benutzernamen des e-Mail-Adresse "maria.rodriguez@contoso.com" und eines Kennworts zum Anmelden des Benutzers.</span><span class="sxs-lookup"><span data-stu-id="49a0c-110">Use the Email username "maria.rodriguez@contoso.com" and any password to sign in the user.</span></span> <span data-ttu-id="49a0c-111">Der Benutzer wird authentifiziert, der `AuthenticateUser` -Methode in der die *Pages/Account/Login.cshtml.cs* Datei.</span><span class="sxs-lookup"><span data-stu-id="49a0c-111">The user is authenticated in the `AuthenticateUser` method in the *Pages/Account/Login.cshtml.cs* file.</span></span> <span data-ttu-id="49a0c-112">Im Real-World-Beispiel würde der Benutzer für eine Datenbank authentifiziert werden.</span><span class="sxs-lookup"><span data-stu-id="49a0c-112">In a real-world example, the user would be authenticated against a database.</span></span>

<span data-ttu-id="49a0c-113">Weitere Informationen zur Migration cookiebasierte Authentifizierung von ASP.NET Core 1.x zu 2.0 finden Sie unter [Migrieren von Authentifizierungs- und Identitätseinstellungen nach ASP.NET Core 2.0-Thema (cookiebasierte Authentifizierung)](xref:migration/1x-to-2x/identity-2x#cookie-based-authentication).</span><span class="sxs-lookup"><span data-stu-id="49a0c-113">For information on migrating cookie-based authentication from ASP.NET Core 1.x to 2.0, see [Migrate Authentication and Identity to ASP.NET Core 2.0 topic (Cookie-based Authentication)](xref:migration/1x-to-2x/identity-2x#cookie-based-authentication).</span></span>

<span data-ttu-id="49a0c-114">Um ASP.NET Core Identity verwenden zu können, finden Sie unter den [Einführung in die Identität](xref:security/authentication/identity) Thema.</span><span class="sxs-lookup"><span data-stu-id="49a0c-114">To use ASP.NET Core Identity, see the [Introduction to Identity](xref:security/authentication/identity) topic.</span></span>

## <a name="configuration"></a><span data-ttu-id="49a0c-115">Konfiguration</span><span class="sxs-lookup"><span data-stu-id="49a0c-115">Configuration</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="49a0c-116">Wenn die app verwenden, nicht die [Microsoft.AspNetCore.App metapaket](xref:fundamentals/metapackage-app), erstellen Sie einen Paketverweis in die Projektdatei für die [Microsoft.AspNetCore.Authentication.Cookies](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Cookies/) Paket (Version 2.1.0 oder weiter unten).</span><span class="sxs-lookup"><span data-stu-id="49a0c-116">If the app doesn't use the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app), create a package reference in the project file for the [Microsoft.AspNetCore.Authentication.Cookies](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Cookies/) package (version 2.1.0 or later).</span></span>

<span data-ttu-id="49a0c-117">In der `ConfigureServices` -Methode, erstellen Sie den Authentifizierungs-Middleware-Dienst, mit der `AddAuthentication` und `AddCookie` Methoden:</span><span class="sxs-lookup"><span data-stu-id="49a0c-117">In the `ConfigureServices` method, create the Authentication Middleware service with the `AddAuthentication` and `AddCookie` methods:</span></span>

[!code-csharp[](cookie/samples/2.x/CookieSample/Startup.cs?name=snippet1)]

<span data-ttu-id="49a0c-118">`AuthenticationScheme` die an `AddAuthentication` legt das Authentifizierungsschema "Standard" für die app fest.</span><span class="sxs-lookup"><span data-stu-id="49a0c-118">`AuthenticationScheme` passed to `AddAuthentication` sets the default authentication scheme for the app.</span></span> <span data-ttu-id="49a0c-119">`AuthenticationScheme` ist nützlich, wenn mehrere Instanzen der Cookie-Authentifizierung vorhanden sind, und Sie möchten [autorisieren mit einem bestimmten Schema](xref:security/authorization/limitingidentitybyscheme).</span><span class="sxs-lookup"><span data-stu-id="49a0c-119">`AuthenticationScheme` is useful when there are multiple instances of cookie authentication and you want to [authorize with a specific scheme](xref:security/authorization/limitingidentitybyscheme).</span></span> <span data-ttu-id="49a0c-120">Festlegen der `AuthenticationScheme` zu `CookieAuthenticationDefaults.AuthenticationScheme` "Cookies" der Wert für das Schema enthält.</span><span class="sxs-lookup"><span data-stu-id="49a0c-120">Setting the `AuthenticationScheme` to `CookieAuthenticationDefaults.AuthenticationScheme` provides a value of "Cookies" for the scheme.</span></span> <span data-ttu-id="49a0c-121">Sie können einen beliebigen Zeichenfolgenwert angeben, der das Schema unterscheidet.</span><span class="sxs-lookup"><span data-stu-id="49a0c-121">You can supply any string value that distinguishes the scheme.</span></span>

<span data-ttu-id="49a0c-122">Der app-Authentifizierungsschema unterscheidet sich von der app Cookie-Authentifizierungsschema.</span><span class="sxs-lookup"><span data-stu-id="49a0c-122">The app's authentication scheme is different from the app's cookie authentication scheme.</span></span> <span data-ttu-id="49a0c-123">Wenn ein Cookie-Authentifizierungsschema bereitgestellt ist nicht <xref:Microsoft.Extensions.DependencyInjection.CookieExtensions.AddCookie*>, verwendet [CookieAuthenticationDefaults.AuthenticationScheme](xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationDefaults.AuthenticationScheme) ("Cookies").</span><span class="sxs-lookup"><span data-stu-id="49a0c-123">When a cookie authentication scheme isn't provided to <xref:Microsoft.Extensions.DependencyInjection.CookieExtensions.AddCookie*>, it uses [CookieAuthenticationDefaults.AuthenticationScheme](xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationDefaults.AuthenticationScheme) ("Cookies").</span></span>

<span data-ttu-id="49a0c-124">In der `Configure` -Methode ist, die `UseAuthentication` Middleware für die Authentifizierung, die festlegt, aufzurufenden Methode der `HttpContext.User` Eigenschaft.</span><span class="sxs-lookup"><span data-stu-id="49a0c-124">In the `Configure` method, use the `UseAuthentication` method to invoke the Authentication Middleware that sets the `HttpContext.User` property.</span></span> <span data-ttu-id="49a0c-125">Rufen Sie die `UseAuthentication` Methode vor dem Aufruf `UseMvcWithDefaultRoute` oder `UseMvc`:</span><span class="sxs-lookup"><span data-stu-id="49a0c-125">Call the `UseAuthentication` method before calling `UseMvcWithDefaultRoute` or `UseMvc`:</span></span>

[!code-csharp[](cookie/samples/2.x/CookieSample/Startup.cs?name=snippet2)]

<span data-ttu-id="49a0c-126">**AddCookie-Optionen**</span><span class="sxs-lookup"><span data-stu-id="49a0c-126">**AddCookie Options**</span></span>

<span data-ttu-id="49a0c-127">Die [CookieAuthenticationOptions](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions?view=aspnetcore-2.0) Klasse wird verwendet, um die Optionen für die Authentifizierung-Anbieter konfigurieren.</span><span class="sxs-lookup"><span data-stu-id="49a0c-127">The [CookieAuthenticationOptions](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions?view=aspnetcore-2.0) class is used to configure the authentication provider options.</span></span>

| <span data-ttu-id="49a0c-128">Option</span><span class="sxs-lookup"><span data-stu-id="49a0c-128">Option</span></span> | <span data-ttu-id="49a0c-129">Beschreibung</span><span class="sxs-lookup"><span data-stu-id="49a0c-129">Description</span></span> |
| ------ | ----------- |
| [<span data-ttu-id="49a0c-130">AccessDeniedPath</span><span class="sxs-lookup"><span data-stu-id="49a0c-130">AccessDeniedPath</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.accessdeniedpath?view=aspnetcore-2.0) | <span data-ttu-id="49a0c-131">Gibt den Pfad zu einer 302 gefunden (URL-Umleitung) liefern die Auslösung von `HttpContext.ForbidAsync`.</span><span class="sxs-lookup"><span data-stu-id="49a0c-131">Provides the path to supply with a 302 Found (URL redirect) when triggered by `HttpContext.ForbidAsync`.</span></span> <span data-ttu-id="49a0c-132">Der Standardwert ist `/Account/AccessDenied`.</span><span class="sxs-lookup"><span data-stu-id="49a0c-132">The default value is `/Account/AccessDenied`.</span></span> |
| [<span data-ttu-id="49a0c-133">ClaimsIssuer</span><span class="sxs-lookup"><span data-stu-id="49a0c-133">ClaimsIssuer</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.authenticationschemeoptions.claimsissuer?view=aspnetcore-2.0) | <span data-ttu-id="49a0c-134">Der Aussteller für die Verwendung der [Aussteller](/dotnet/api/system.security.claims.claim.issuer) Eigenschaft für alle Ansprüche, die von dem Cookie Authentication-Dienst erstellt.</span><span class="sxs-lookup"><span data-stu-id="49a0c-134">The issuer to use for the [Issuer](/dotnet/api/system.security.claims.claim.issuer) property on any claims created by the cookie authentication service.</span></span> |
| [<span data-ttu-id="49a0c-135">Cookie.Domain</span><span class="sxs-lookup"><span data-stu-id="49a0c-135">Cookie.Domain</span></span>](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.domain?view=aspnetcore-2.0) | <span data-ttu-id="49a0c-136">Der Domänenname, in dem das Cookie bereitgestellt wird.</span><span class="sxs-lookup"><span data-stu-id="49a0c-136">The domain name where the cookie is served.</span></span> <span data-ttu-id="49a0c-137">Standardmäßig ist dies der Hostname der Anforderung.</span><span class="sxs-lookup"><span data-stu-id="49a0c-137">By default, this is the host name of the request.</span></span> <span data-ttu-id="49a0c-138">Der Browser sendet das Cookie nur in Anforderungen auf einen entsprechenden Hostnamen.</span><span class="sxs-lookup"><span data-stu-id="49a0c-138">The browser only sends the cookie in requests to a matching host name.</span></span> <span data-ttu-id="49a0c-139">Sie möchten möglicherweise passen Sie diese Option, um Cookies zu einem beliebigen Host in der Domäne verfügen.</span><span class="sxs-lookup"><span data-stu-id="49a0c-139">You may wish to adjust this to have cookies available to any host in your domain.</span></span> <span data-ttu-id="49a0c-140">Wenn z. B. Domäne des Cookies auf `.contoso.com` verfügbar macht `contoso.com`, `www.contoso.com`, und `staging.www.contoso.com`.</span><span class="sxs-lookup"><span data-stu-id="49a0c-140">For example, setting the cookie domain to `.contoso.com` makes it available to `contoso.com`, `www.contoso.com`, and `staging.www.contoso.com`.</span></span> |
| [<span data-ttu-id="49a0c-141">Cookie.Expiration</span><span class="sxs-lookup"><span data-stu-id="49a0c-141">Cookie.Expiration</span></span>](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.expiration?view=aspnetcore-2.0) | <span data-ttu-id="49a0c-142">Übernimmt oder bestimmt die Lebensdauer eines Cookies.</span><span class="sxs-lookup"><span data-stu-id="49a0c-142">Gets or sets the lifespan of a cookie.</span></span> <span data-ttu-id="49a0c-143">Derzeit diese option keine Funktion und wird in ASP.NET Core 2.1 und höher veraltet.</span><span class="sxs-lookup"><span data-stu-id="49a0c-143">Currently, this option no-ops and will become obsolete in ASP.NET Core 2.1+.</span></span> <span data-ttu-id="49a0c-144">Verwenden der `ExpireTimeSpan` cookieablauf festlegen.</span><span class="sxs-lookup"><span data-stu-id="49a0c-144">Use the `ExpireTimeSpan` option to set cookie expiration.</span></span> <span data-ttu-id="49a0c-145">Weitere Informationen finden Sie unter [verdeutlichen Verhalten CookieAuthenticationOptions.Cookie.Expiration](https://github.com/aspnet/Security/issues/1293).</span><span class="sxs-lookup"><span data-stu-id="49a0c-145">For more information, see [Clarify behavior of CookieAuthenticationOptions.Cookie.Expiration](https://github.com/aspnet/Security/issues/1293).</span></span> |
| [<span data-ttu-id="49a0c-146">Cookie.HttpOnly</span><span class="sxs-lookup"><span data-stu-id="49a0c-146">Cookie.HttpOnly</span></span>](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.httponly?view=aspnetcore-2.0) | <span data-ttu-id="49a0c-147">Ein Flag, das angibt, ob das Cookie nur Server zugegriffen werden soll.</span><span class="sxs-lookup"><span data-stu-id="49a0c-147">A flag indicating if the cookie should be accessible only to servers.</span></span> <span data-ttu-id="49a0c-148">Durch Ändern dieses Werts auf `false` clientseitige Skripts auf Cookies zugreifen können und möglicherweise öffnen Sie Ihre app zum Diebstahl von Cookies muss Ihrer app eine [Cross-Site-scripting (XSS)](xref:security/cross-site-scripting) Sicherheitsrisiko.</span><span class="sxs-lookup"><span data-stu-id="49a0c-148">Changing this value to `false` permits client-side scripts to access the cookie and may open your app to cookie theft should your app have a [Cross-site scripting (XSS)](xref:security/cross-site-scripting) vulnerability.</span></span> <span data-ttu-id="49a0c-149">Der Standardwert ist `true`.</span><span class="sxs-lookup"><span data-stu-id="49a0c-149">The default value is `true`.</span></span> |
| [<span data-ttu-id="49a0c-150">Cookie.Name</span><span class="sxs-lookup"><span data-stu-id="49a0c-150">Cookie.Name</span></span>](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.name?view=aspnetcore-2.0) | <span data-ttu-id="49a0c-151">Legt den Namen des Cookies.</span><span class="sxs-lookup"><span data-stu-id="49a0c-151">Sets the name of the cookie.</span></span> |
| [<span data-ttu-id="49a0c-152">Cookie.Path</span><span class="sxs-lookup"><span data-stu-id="49a0c-152">Cookie.Path</span></span>](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.path?view=aspnetcore-2.0) | <span data-ttu-id="49a0c-153">Zum Isolieren von apps auf dem gleichen Hostnamen verwendet.</span><span class="sxs-lookup"><span data-stu-id="49a0c-153">Used to isolate apps running on the same host name.</span></span> <span data-ttu-id="49a0c-154">Wenn Sie eine app mit haben `/app1` und Cookies für diese app zu beschränken, legen Sie die `CookiePath` Eigenschaft `/app1`.</span><span class="sxs-lookup"><span data-stu-id="49a0c-154">If you have an app running at `/app1` and want to restrict cookies to that app, set the `CookiePath` property to `/app1`.</span></span> <span data-ttu-id="49a0c-155">Auf diese Weise das Cookie ist nur verfügbar für Anforderungen an `/app1` und alle Apps, darunter.</span><span class="sxs-lookup"><span data-stu-id="49a0c-155">By doing so, the cookie is only available on requests to `/app1` and any app underneath it.</span></span> |
| [<span data-ttu-id="49a0c-156">Cookie.SameSite</span><span class="sxs-lookup"><span data-stu-id="49a0c-156">Cookie.SameSite</span></span>](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.samesite?view=aspnetcore-2.0) | <span data-ttu-id="49a0c-157">Gibt an, ob der Browser Cookies, die auf Anforderungen der gleichen Website nur angefügt werden darf (`SameSiteMode.Strict`) oder Cross-Site-Anforderungen, die mithilfe von sicheren HTTP-Methoden und Anforderungen der gleichen Website (`SameSiteMode.Lax`).</span><span class="sxs-lookup"><span data-stu-id="49a0c-157">Indicates whether the browser should allow the cookie to be attached to same-site requests only (`SameSiteMode.Strict`) or cross-site requests using safe HTTP methods and same-site requests (`SameSiteMode.Lax`).</span></span> <span data-ttu-id="49a0c-158">Bei Festlegung auf `SameSiteMode.None`, der Wert der Cookie-Header nicht festgelegt.</span><span class="sxs-lookup"><span data-stu-id="49a0c-158">When set to `SameSiteMode.None`, the cookie header value isn't set.</span></span> <span data-ttu-id="49a0c-159">Beachten Sie, dass [Richtlinie Cookiemiddleware](#cookie-policy-middleware) möglicherweise Überschreiben des Werts, den Sie bereitstellen.</span><span class="sxs-lookup"><span data-stu-id="49a0c-159">Note that [Cookie Policy Middleware](#cookie-policy-middleware) might overwrite the value that you provide.</span></span> <span data-ttu-id="49a0c-160">Um OAuth-Authentifizierung zu unterstützen, der Standardwert ist `SameSiteMode.Lax`.</span><span class="sxs-lookup"><span data-stu-id="49a0c-160">To support OAuth authentication, the default value is `SameSiteMode.Lax`.</span></span> <span data-ttu-id="49a0c-161">Weitere Informationen finden Sie unter [OAuth-Authentifizierung, die aufgrund von Richtlinien für SameSite-Cookies unterteilt](https://github.com/aspnet/Security/issues/1231).</span><span class="sxs-lookup"><span data-stu-id="49a0c-161">For more information, see [OAuth authentication broken due to SameSite cookie policy](https://github.com/aspnet/Security/issues/1231).</span></span> |
| [<span data-ttu-id="49a0c-162">Cookie.SecurePolicy</span><span class="sxs-lookup"><span data-stu-id="49a0c-162">Cookie.SecurePolicy</span></span>](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.securepolicy?view=aspnetcore-2.0) | <span data-ttu-id="49a0c-163">Ein Flag, das angibt, ob das Cookie erstellt auf HTTPS beschränkt werden soll (`CookieSecurePolicy.Always`), HTTP oder HTTPS (`CookieSecurePolicy.None`), oder das gleiche Protokoll wie die Anforderung (`CookieSecurePolicy.SameAsRequest`).</span><span class="sxs-lookup"><span data-stu-id="49a0c-163">A flag indicating if the cookie created should be limited to HTTPS (`CookieSecurePolicy.Always`), HTTP or HTTPS (`CookieSecurePolicy.None`), or the same protocol as the request (`CookieSecurePolicy.SameAsRequest`).</span></span> <span data-ttu-id="49a0c-164">Der Standardwert ist `CookieSecurePolicy.SameAsRequest`.</span><span class="sxs-lookup"><span data-stu-id="49a0c-164">The default value is `CookieSecurePolicy.SameAsRequest`.</span></span> |
| [<span data-ttu-id="49a0c-165">DataProtectionProvider</span><span class="sxs-lookup"><span data-stu-id="49a0c-165">DataProtectionProvider</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.dataprotectionprovider?view=aspnetcore-2.0) | <span data-ttu-id="49a0c-166">Legt die `DataProtectionProvider` dient zum Erstellen der standardmäßigen `TicketDataFormat`.</span><span class="sxs-lookup"><span data-stu-id="49a0c-166">Sets the `DataProtectionProvider` that's used to create the default `TicketDataFormat`.</span></span> <span data-ttu-id="49a0c-167">Wenn die `TicketDataFormat` Eigenschaft festgelegt ist, die `DataProtectionProvider` Option wird nicht verwendet.</span><span class="sxs-lookup"><span data-stu-id="49a0c-167">If the `TicketDataFormat` property is set, the `DataProtectionProvider` option isn't used.</span></span> <span data-ttu-id="49a0c-168">Wenn nicht angegeben, wird die app Datenschutz-Standardanbieter verwendet.</span><span class="sxs-lookup"><span data-stu-id="49a0c-168">If not provided, the app's default data protection provider is used.</span></span> |
| [<span data-ttu-id="49a0c-169">Ereignisse</span><span class="sxs-lookup"><span data-stu-id="49a0c-169">Events</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.events?view=aspnetcore-2.0) | <span data-ttu-id="49a0c-170">Der Handler ruft Methoden für den Anbieter, der die app-Steuerung an bestimmten Punkten für die Verarbeitung zu ermöglichen.</span><span class="sxs-lookup"><span data-stu-id="49a0c-170">The handler calls methods on the provider that give the app control at certain processing points.</span></span> <span data-ttu-id="49a0c-171">Wenn `Events` sind nicht angegeben wird, wird eine Standardinstanz bereitgestellt wird, die keine Aktion ausführt, wenn die Methoden aufgerufen werden.</span><span class="sxs-lookup"><span data-stu-id="49a0c-171">If `Events` aren't provided, a default instance is supplied that does nothing when the methods are called.</span></span> |
| [<span data-ttu-id="49a0c-172">EventsType</span><span class="sxs-lookup"><span data-stu-id="49a0c-172">EventsType</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.authenticationschemeoptions.eventstype?view=aspnetcore-2.0) | <span data-ttu-id="49a0c-173">Als den Diensttyp verwendet, um das Abrufen der `Events` Instanz statt mit der Eigenschaft.</span><span class="sxs-lookup"><span data-stu-id="49a0c-173">Used as the service type to get the `Events` instance instead of the property.</span></span> |
| [<span data-ttu-id="49a0c-174">ExpireTimeSpan</span><span class="sxs-lookup"><span data-stu-id="49a0c-174">ExpireTimeSpan</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.expiretimespan?view=aspnetcore-2.0) | <span data-ttu-id="49a0c-175">Die `TimeSpan` nach dem in Cookies gespeicherten Authentifizierungsticket abläuft.</span><span class="sxs-lookup"><span data-stu-id="49a0c-175">The `TimeSpan` after which the authentication ticket stored inside the cookie expires.</span></span> <span data-ttu-id="49a0c-176">`ExpireTimeSpan` wird hinzugefügt, dass die aktuelle Uhrzeit aus, um die Ablaufzeit für das Ticket zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="49a0c-176">`ExpireTimeSpan` is added to the current time to create the expiration time for the ticket.</span></span> <span data-ttu-id="49a0c-177">Die `ExpiredTimeSpan` Wert ist immer in das verschlüsselte Authentifizierungsticket, die vom Server überprüft wird.</span><span class="sxs-lookup"><span data-stu-id="49a0c-177">The `ExpiredTimeSpan` value always goes into the encrypted AuthTicket verified by the server.</span></span> <span data-ttu-id="49a0c-178">Es kann auch wechseln Sie in der [Set-Cookie](https://tools.ietf.org/html/rfc6265#section-4.1) Header jedoch nur, wenn `IsPersistent` festgelegt ist.</span><span class="sxs-lookup"><span data-stu-id="49a0c-178">It may also go into the [Set-Cookie](https://tools.ietf.org/html/rfc6265#section-4.1) header, but only if `IsPersistent` is set.</span></span> <span data-ttu-id="49a0c-179">Festzulegende `IsPersistent` zu `true`, konfigurieren die [AuthenticationProperties](/dotnet/api/microsoft.aspnetcore.authentication.authenticationproperties) übergeben `SignInAsync`.</span><span class="sxs-lookup"><span data-stu-id="49a0c-179">To set `IsPersistent` to `true`, configure the [AuthenticationProperties](/dotnet/api/microsoft.aspnetcore.authentication.authenticationproperties) passed to `SignInAsync`.</span></span> <span data-ttu-id="49a0c-180">Der Standardwert von `ExpireTimeSpan` beträgt 14 Tage.</span><span class="sxs-lookup"><span data-stu-id="49a0c-180">The default value of `ExpireTimeSpan` is 14 days.</span></span> |
| [<span data-ttu-id="49a0c-181">LoginPath</span><span class="sxs-lookup"><span data-stu-id="49a0c-181">LoginPath</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.loginpath?view=aspnetcore-2.0) | <span data-ttu-id="49a0c-182">Gibt den Pfad zu einer 302 gefunden (URL-Umleitung) liefern die Auslösung von `HttpContext.ChallengeAsync`.</span><span class="sxs-lookup"><span data-stu-id="49a0c-182">Provides the path to supply with a 302 Found (URL redirect) when triggered by `HttpContext.ChallengeAsync`.</span></span> <span data-ttu-id="49a0c-183">Die aktuelle URL, die den Statuscode 401 generiert wird hinzugefügt, um die `LoginPath` als ein Abfragezeichenfolgen-Parameter mit dem Namen, indem Sie die `ReturnUrlParameter`.</span><span class="sxs-lookup"><span data-stu-id="49a0c-183">The current URL that generated the 401 is added to the `LoginPath` as a query string parameter named by the `ReturnUrlParameter`.</span></span> <span data-ttu-id="49a0c-184">Einmal eine Anforderung an die `LoginPath` eine neue Identität Anmeldung gewährt der `ReturnUrlParameter` Wert wird verwendet, um den Browser zurück an die URL umgeleitet wird, die den ursprünglichen Statuscode nicht autorisiert verursacht hat.</span><span class="sxs-lookup"><span data-stu-id="49a0c-184">Once a request to the `LoginPath` grants a new sign-in identity, the `ReturnUrlParameter` value is used to redirect the browser back to the URL that caused the original unauthorized status code.</span></span> <span data-ttu-id="49a0c-185">Der Standardwert ist `/Account/Login`.</span><span class="sxs-lookup"><span data-stu-id="49a0c-185">The default value is `/Account/Login`.</span></span> |
| [<span data-ttu-id="49a0c-186">LogoutPath</span><span class="sxs-lookup"><span data-stu-id="49a0c-186">LogoutPath</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.logoutpath?view=aspnetcore-2.0) | <span data-ttu-id="49a0c-187">Wenn die `LogoutPath` wird bereitgestellt, um den Handler auf, und klicken Sie dann eine Anforderung an diesen Pfad leitet basierend auf den Wert der `ReturnUrlParameter`.</span><span class="sxs-lookup"><span data-stu-id="49a0c-187">If the `LogoutPath` is provided to the handler, then a request to that path redirects based on the value of the `ReturnUrlParameter`.</span></span> <span data-ttu-id="49a0c-188">Der Standardwert ist `/Account/Logout`.</span><span class="sxs-lookup"><span data-stu-id="49a0c-188">The default value is `/Account/Logout`.</span></span> |
| [<span data-ttu-id="49a0c-189">ReturnUrlParameter</span><span class="sxs-lookup"><span data-stu-id="49a0c-189">ReturnUrlParameter</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.returnurlparameter?view=aspnetcore-2.0) | <span data-ttu-id="49a0c-190">Legt den Namen des Abfragezeichenfolgen-Parameters, der durch den Handler für eine Antwort mit dem 302 gefunden (URL-Umleitung) angefügt wird.</span><span class="sxs-lookup"><span data-stu-id="49a0c-190">Determines the name of the query string parameter that's appended by the handler for a 302 Found (URL redirect) response.</span></span> <span data-ttu-id="49a0c-191">`ReturnUrlParameter` wird verwendet, wenn eine Anforderung eingeht, auf die `LoginPath` oder `LogoutPath` den Browser die ursprüngliche URL zurückgegeben, nachdem die an- bzw. Abmeldung Aktion ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="49a0c-191">`ReturnUrlParameter` is used when a request arrives on the `LoginPath` or `LogoutPath` to return the browser to the original URL after the login or logout action is performed.</span></span> <span data-ttu-id="49a0c-192">Der Standardwert ist `ReturnUrl`.</span><span class="sxs-lookup"><span data-stu-id="49a0c-192">The default value is `ReturnUrl`.</span></span> |
| [<span data-ttu-id="49a0c-193">SessionStore</span><span class="sxs-lookup"><span data-stu-id="49a0c-193">SessionStore</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.sessionstore?view=aspnetcore-2.0) | <span data-ttu-id="49a0c-194">Ein optionaler Container verwendet, um die Identität anforderungsübergreifend gespeichert.</span><span class="sxs-lookup"><span data-stu-id="49a0c-194">An optional container used to store identity across requests.</span></span> <span data-ttu-id="49a0c-195">Wenn verwendet, wird nur ein Sitzungsbezeichner an den Client gesendet.</span><span class="sxs-lookup"><span data-stu-id="49a0c-195">When used, only a session identifier is sent to the client.</span></span> <span data-ttu-id="49a0c-196">`SessionStore` kann verwendet werden, um potenzielle Probleme mit großen Identitäten zu verringern.</span><span class="sxs-lookup"><span data-stu-id="49a0c-196">`SessionStore` can be used to mitigate potential problems with large identities.</span></span> |
| [<span data-ttu-id="49a0c-197">slidingExpiration</span><span class="sxs-lookup"><span data-stu-id="49a0c-197">SlidingExpiration</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.slidingexpiration?view=aspnetcore-2.0) | <span data-ttu-id="49a0c-198">Ein Flag, das angibt, ob ein neues Cookie mit einer aktualisierten Ablaufzeit dynamisch ausgegeben werden soll.</span><span class="sxs-lookup"><span data-stu-id="49a0c-198">A flag indicating if a new cookie with an updated expiration time should be issued dynamically.</span></span> <span data-ttu-id="49a0c-199">Dies kann bei jeder Anforderung auftreten, in dem das aktuelle Cookie Ablaufdatum mehr als 50 % abgelaufen ist.</span><span class="sxs-lookup"><span data-stu-id="49a0c-199">This can happen on any request where the current cookie expiration period is more than 50% expired.</span></span> <span data-ttu-id="49a0c-200">Das neue Ablaufdatum nach unten verschoben werden das aktuelle Datum plus dem `ExpireTimespan`.</span><span class="sxs-lookup"><span data-stu-id="49a0c-200">The new expiration date is moved forward to be the current date plus the `ExpireTimespan`.</span></span> <span data-ttu-id="49a0c-201">Ein [absolute Cookie Ablaufzeit](xref:security/authentication/cookie#absolute-cookie-expiration) kann festgelegt werden, mithilfe der `AuthenticationProperties` -Klasse beim Aufrufen von `SignInAsync`.</span><span class="sxs-lookup"><span data-stu-id="49a0c-201">An [absolute cookie expiration time](xref:security/authentication/cookie#absolute-cookie-expiration) can be set by using the `AuthenticationProperties` class when calling `SignInAsync`.</span></span> <span data-ttu-id="49a0c-202">Eine absolute Ablaufzeit kann verbessern Sie die Sicherheit Ihrer App durch Einschränken der Menge der Zeit, die das Authentifizierungscookie gültig ist.</span><span class="sxs-lookup"><span data-stu-id="49a0c-202">An absolute expiration time can improve the security of your app by limiting the amount of time that the authentication cookie is valid.</span></span> <span data-ttu-id="49a0c-203">Der Standardwert ist `true`.</span><span class="sxs-lookup"><span data-stu-id="49a0c-203">The default value is `true`.</span></span> |
| [<span data-ttu-id="49a0c-204">TicketDataFormat</span><span class="sxs-lookup"><span data-stu-id="49a0c-204">TicketDataFormat</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.ticketdataformat?view=aspnetcore-2.0) | <span data-ttu-id="49a0c-205">Die `TicketDataFormat` wird zum Schützen und Aufheben des Schutzes der Identität und andere Eigenschaften, die im Cookiewert gespeichert sind.</span><span class="sxs-lookup"><span data-stu-id="49a0c-205">The `TicketDataFormat` is used to protect and unprotect the identity and other properties that are stored in the cookie value.</span></span> <span data-ttu-id="49a0c-206">Wenn nicht angegeben ist, eine `TicketDataFormat` wurde mit der [DataProtectionProvider](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.dataprotectionprovider?view=aspnetcore-2.0).</span><span class="sxs-lookup"><span data-stu-id="49a0c-206">If not provided, a `TicketDataFormat` is created using the [DataProtectionProvider](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.dataprotectionprovider?view=aspnetcore-2.0).</span></span> |
| [<span data-ttu-id="49a0c-207">Überprüfen</span><span class="sxs-lookup"><span data-stu-id="49a0c-207">Validate</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.authenticationschemeoptions.validate?view=aspnetcore-2.0) | <span data-ttu-id="49a0c-208">Methode, die überprüft, ob die Optionen gültig sind.</span><span class="sxs-lookup"><span data-stu-id="49a0c-208">Method that checks that the options are valid.</span></span> |

<span data-ttu-id="49a0c-209">Legen Sie `CookieAuthenticationOptions` in der Dienstkonfiguration für die Authentifizierung in der `ConfigureServices` Methode:</span><span class="sxs-lookup"><span data-stu-id="49a0c-209">Set `CookieAuthenticationOptions` in the service configuration for authentication in the `ConfigureServices` method:</span></span>

```csharp
services.AddAuthentication(CookieAuthenticationDefaults.AuthenticationScheme)
    .AddCookie(options =>
    {
        ...
    });
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="49a0c-210">ASP.NET Core 1.x verwendet Cookies [Middleware](xref:fundamentals/middleware/index) , der einen Benutzerprinzipal in einem verschlüsselten Cookie serialisiert.</span><span class="sxs-lookup"><span data-stu-id="49a0c-210">ASP.NET Core 1.x uses cookie [middleware](xref:fundamentals/middleware/index) that serializes a user principal into an encrypted cookie.</span></span> <span data-ttu-id="49a0c-211">Bei nachfolgenden Anforderungen, der das Cookie wird überprüft, und der Prinzipal neu erstellt und zugewiesen wird die `HttpContext.User` Eigenschaft.</span><span class="sxs-lookup"><span data-stu-id="49a0c-211">On subsequent requests, the cookie is validated, and the principal is recreated and assigned to the `HttpContext.User` property.</span></span>

<span data-ttu-id="49a0c-212">Installieren Sie die [Microsoft.AspNetCore.Authentication.Cookies](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Cookies/) NuGet-Paket in Ihrem Projekt.</span><span class="sxs-lookup"><span data-stu-id="49a0c-212">Install the [Microsoft.AspNetCore.Authentication.Cookies](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Cookies/) NuGet package in your project.</span></span> <span data-ttu-id="49a0c-213">Dieses Paket enthält die Cookie-Middleware.</span><span class="sxs-lookup"><span data-stu-id="49a0c-213">This package contains the cookie middleware.</span></span>

<span data-ttu-id="49a0c-214">Verwenden der `UseCookieAuthentication` -Methode in der die `Configure` -Methode in Ihrer *"Startup.cs"* -Datei, bevor `UseMvc` oder `UseMvcWithDefaultRoute`:</span><span class="sxs-lookup"><span data-stu-id="49a0c-214">Use the `UseCookieAuthentication` method in the `Configure` method in your *Startup.cs* file before `UseMvc` or `UseMvcWithDefaultRoute`:</span></span>

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

<span data-ttu-id="49a0c-215">**CookieAuthenticationOptions-Optionen**</span><span class="sxs-lookup"><span data-stu-id="49a0c-215">**CookieAuthenticationOptions Options**</span></span>

<span data-ttu-id="49a0c-216">Die [CookieAuthenticationOptions](/dotnet/api/Microsoft.AspNetCore.Builder.CookieAuthenticationOptions?view=aspnetcore-1.1) Klasse wird verwendet, um die Optionen für die Authentifizierung-Anbieter konfigurieren.</span><span class="sxs-lookup"><span data-stu-id="49a0c-216">The [CookieAuthenticationOptions](/dotnet/api/Microsoft.AspNetCore.Builder.CookieAuthenticationOptions?view=aspnetcore-1.1) class is used to configure the authentication provider options.</span></span>

| <span data-ttu-id="49a0c-217">Option</span><span class="sxs-lookup"><span data-stu-id="49a0c-217">Option</span></span> | <span data-ttu-id="49a0c-218">Beschreibung</span><span class="sxs-lookup"><span data-stu-id="49a0c-218">Description</span></span> |
| ------ | ----------- |
| [<span data-ttu-id="49a0c-219">AuthenticationScheme</span><span class="sxs-lookup"><span data-stu-id="49a0c-219">AuthenticationScheme</span></span>](/dotnet/api/microsoft.aspnetcore.builder.authenticationoptions.authenticationscheme?view=aspnetcore-1.1) | <span data-ttu-id="49a0c-220">Legt das Authentifizierungsschema fest.</span><span class="sxs-lookup"><span data-stu-id="49a0c-220">Sets the authentication scheme.</span></span> <span data-ttu-id="49a0c-221">`AuthenticationScheme` ist nützlich, wenn mehrere Instanzen der Authentifizierung vorhanden sind, und Sie möchten [autorisieren mit einem bestimmten Schema](xref:security/authorization/limitingidentitybyscheme).</span><span class="sxs-lookup"><span data-stu-id="49a0c-221">`AuthenticationScheme` is useful when there are multiple instances of authentication and you want to [authorize with a specific scheme](xref:security/authorization/limitingidentitybyscheme).</span></span> <span data-ttu-id="49a0c-222">Festlegen der `AuthenticationScheme` zu `CookieAuthenticationDefaults.AuthenticationScheme` "Cookies" der Wert für das Schema enthält.</span><span class="sxs-lookup"><span data-stu-id="49a0c-222">Setting the `AuthenticationScheme` to `CookieAuthenticationDefaults.AuthenticationScheme` provides a value of "Cookies" for the scheme.</span></span> <span data-ttu-id="49a0c-223">Sie können einen beliebigen Zeichenfolgenwert angeben, der das Schema unterscheidet.</span><span class="sxs-lookup"><span data-stu-id="49a0c-223">You can supply any string value that distinguishes the scheme.</span></span> |
| [<span data-ttu-id="49a0c-224">AutomaticAuthenticate</span><span class="sxs-lookup"><span data-stu-id="49a0c-224">AutomaticAuthenticate</span></span>](/dotnet/api/microsoft.aspnetcore.builder.authenticationoptions.automaticauthenticate?view=aspnetcore-1.1) | <span data-ttu-id="49a0c-225">Legt einen Wert aus, um anzugeben, dass die Cookie-Authentifizierung sollte für jede Anforderung ausgeführt und versucht wird, um zu überprüfen, und rekonstruieren serialisierte Prinzipal, dem Sie erstellt.</span><span class="sxs-lookup"><span data-stu-id="49a0c-225">Sets a value to indicate that the cookie authentication should run on every request and attempt to validate and reconstruct any serialized principal it created.</span></span> |
| [<span data-ttu-id="49a0c-226">AutomaticChallenge</span><span class="sxs-lookup"><span data-stu-id="49a0c-226">AutomaticChallenge</span></span>](/dotnet/api/microsoft.aspnetcore.builder.authenticationoptions.automaticchallenge?view=aspnetcore-1.1) | <span data-ttu-id="49a0c-227">True gibt an, behandelt die authentifizierungsmiddleware automatische Herausforderungen.</span><span class="sxs-lookup"><span data-stu-id="49a0c-227">If true, the authentication middleware handles automatic challenges.</span></span> <span data-ttu-id="49a0c-228">Wenn "false", die authentifizierungsmiddleware nur Antworten, wenn dies ausdrücklich angegeben ändert die `AuthenticationScheme`.</span><span class="sxs-lookup"><span data-stu-id="49a0c-228">If false, the authentication middleware only alters responses when explicitly indicated by the `AuthenticationScheme`.</span></span> |
| [<span data-ttu-id="49a0c-229">ClaimsIssuer</span><span class="sxs-lookup"><span data-stu-id="49a0c-229">ClaimsIssuer</span></span>](/dotnet/api/microsoft.aspnetcore.builder.authenticationoptions.claimsissuer?view=aspnetcore-1.1) | <span data-ttu-id="49a0c-230">Der Aussteller für die Verwendung der [Aussteller](/dotnet/api/system.security.claims.claim.issuer) Eigenschaft für alle Ansprüche, die durch die cookieauthentifizierungs-Middleware erstellt.</span><span class="sxs-lookup"><span data-stu-id="49a0c-230">The issuer to use for the [Issuer](/dotnet/api/system.security.claims.claim.issuer) property on any claims created by the cookie authentication middleware.</span></span> |
| [<span data-ttu-id="49a0c-231">CookieDomain</span><span class="sxs-lookup"><span data-stu-id="49a0c-231">CookieDomain</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.cookiedomain?view=aspnetcore-1.1) | <span data-ttu-id="49a0c-232">Der Domänenname, in dem das Cookie bereitgestellt wird.</span><span class="sxs-lookup"><span data-stu-id="49a0c-232">The domain name where the cookie is served.</span></span> <span data-ttu-id="49a0c-233">Standardmäßig ist dies der Hostname der Anforderung.</span><span class="sxs-lookup"><span data-stu-id="49a0c-233">By default, this is the host name of the request.</span></span> <span data-ttu-id="49a0c-234">Der Browser dient nur Cookies, das einen entsprechenden Hostnamen an.</span><span class="sxs-lookup"><span data-stu-id="49a0c-234">The browser only serves the cookie to a matching host name.</span></span> <span data-ttu-id="49a0c-235">Sie möchten möglicherweise passen Sie diese Option, um Cookies zu einem beliebigen Host in der Domäne verfügen.</span><span class="sxs-lookup"><span data-stu-id="49a0c-235">You may wish to adjust this to have cookies available to any host in your domain.</span></span> <span data-ttu-id="49a0c-236">Wenn z. B. Domäne des Cookies auf `.contoso.com` verfügbar macht `contoso.com`, `www.contoso.com`, und `staging.www.contoso.com`.</span><span class="sxs-lookup"><span data-stu-id="49a0c-236">For example, setting the cookie domain to `.contoso.com` makes it available to `contoso.com`, `www.contoso.com`, and `staging.www.contoso.com`.</span></span> |
| [<span data-ttu-id="49a0c-237">CookieHttpOnly</span><span class="sxs-lookup"><span data-stu-id="49a0c-237">CookieHttpOnly</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.cookiehttponly?view=aspnetcore-1.1) | <span data-ttu-id="49a0c-238">Ein Flag, das angibt, ob das Cookie nur Server zugegriffen werden soll.</span><span class="sxs-lookup"><span data-stu-id="49a0c-238">A flag indicating if the cookie should be accessible only to servers.</span></span> <span data-ttu-id="49a0c-239">Durch Ändern dieses Werts auf `false` clientseitige Skripts auf Cookies zugreifen können und möglicherweise öffnen Sie Ihre app zum Diebstahl von Cookies muss Ihrer app eine [Cross-Site-scripting (XSS)](xref:security/cross-site-scripting) Sicherheitsrisiko.</span><span class="sxs-lookup"><span data-stu-id="49a0c-239">Changing this value to `false` permits client-side scripts to access the cookie and may open your app to cookie theft should your app have a [Cross-site scripting (XSS)](xref:security/cross-site-scripting) vulnerability.</span></span> <span data-ttu-id="49a0c-240">Der Standardwert ist `true`.</span><span class="sxs-lookup"><span data-stu-id="49a0c-240">The default value is `true`.</span></span> |
| [<span data-ttu-id="49a0c-241">CookiePath</span><span class="sxs-lookup"><span data-stu-id="49a0c-241">CookiePath</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.cookiepath?view=aspnetcore-1.1) | <span data-ttu-id="49a0c-242">Zum Isolieren von apps auf dem gleichen Hostnamen verwendet.</span><span class="sxs-lookup"><span data-stu-id="49a0c-242">Used to isolate apps running on the same host name.</span></span> <span data-ttu-id="49a0c-243">Wenn Sie eine app mit haben `/app1` und Cookies für diese app zu beschränken, legen Sie die `CookiePath` Eigenschaft `/app1`.</span><span class="sxs-lookup"><span data-stu-id="49a0c-243">If you have an app running at `/app1` and want to restrict cookies to that app, set the `CookiePath` property to `/app1`.</span></span> <span data-ttu-id="49a0c-244">Auf diese Weise das Cookie ist nur verfügbar für Anforderungen an `/app1` und alle Apps, darunter.</span><span class="sxs-lookup"><span data-stu-id="49a0c-244">By doing so, the cookie is only available on requests to `/app1` and any app underneath it.</span></span> |
| [<span data-ttu-id="49a0c-245">CookieSecure</span><span class="sxs-lookup"><span data-stu-id="49a0c-245">CookieSecure</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.cookiesecure?view=aspnetcore-1.1) | <span data-ttu-id="49a0c-246">Ein Flag, das angibt, ob das Cookie erstellt auf HTTPS beschränkt werden soll (`CookieSecurePolicy.Always`), HTTP oder HTTPS (`CookieSecurePolicy.None`), oder das gleiche Protokoll wie die Anforderung (`CookieSecurePolicy.SameAsRequest`).</span><span class="sxs-lookup"><span data-stu-id="49a0c-246">A flag indicating if the cookie created should be limited to HTTPS (`CookieSecurePolicy.Always`), HTTP or HTTPS (`CookieSecurePolicy.None`), or the same protocol as the request (`CookieSecurePolicy.SameAsRequest`).</span></span> <span data-ttu-id="49a0c-247">Der Standardwert ist `CookieSecurePolicy.SameAsRequest`.</span><span class="sxs-lookup"><span data-stu-id="49a0c-247">The default value is `CookieSecurePolicy.SameAsRequest`.</span></span> |
| [<span data-ttu-id="49a0c-248">Beschreibung</span><span class="sxs-lookup"><span data-stu-id="49a0c-248">Description</span></span>](/dotnet/api/microsoft.aspnetcore.builder.authenticationoptions.description?view=aspnetcore-1.1) | <span data-ttu-id="49a0c-249">Weitere Informationen zum Authentifizierungstyp, der für die app zur Verfügung gestellt wird.</span><span class="sxs-lookup"><span data-stu-id="49a0c-249">Additional information about the authentication type which is made available to the app.</span></span> |
| [<span data-ttu-id="49a0c-250">ExpireTimeSpan</span><span class="sxs-lookup"><span data-stu-id="49a0c-250">ExpireTimeSpan</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.expiretimespan?view=aspnetcore-1.1) | <span data-ttu-id="49a0c-251">Die `TimeSpan` nach dem das Authentifizierungsticket abläuft.</span><span class="sxs-lookup"><span data-stu-id="49a0c-251">The `TimeSpan` after which the authentication ticket expires.</span></span> <span data-ttu-id="49a0c-252">Es wird auf die aktuelle Uhrzeit aus, um die Ablaufzeit für das Ticket zu erstellen, hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="49a0c-252">It's added to the current time to create the expiration time for the ticket.</span></span> <span data-ttu-id="49a0c-253">Mit `ExpireTimeSpan`, müssen Sie festlegen, `IsPersistent` zu `true` in die `AuthenticationProperties` übergeben `SignInAsync`.</span><span class="sxs-lookup"><span data-stu-id="49a0c-253">To use `ExpireTimeSpan`, you must set `IsPersistent` to `true` in the `AuthenticationProperties` passed to `SignInAsync`.</span></span> <span data-ttu-id="49a0c-254">Der Standardwert beträgt 14 Tage.</span><span class="sxs-lookup"><span data-stu-id="49a0c-254">The default value is 14 days.</span></span> |
| [<span data-ttu-id="49a0c-255">slidingExpiration</span><span class="sxs-lookup"><span data-stu-id="49a0c-255">SlidingExpiration</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.slidingexpiration?view=aspnetcore-1.1) | <span data-ttu-id="49a0c-256">Ein Flag, der angibt, ob das Cookie-Ablaufdatum wird, wenn mehr als die Hälfte der zurückgesetzt der `ExpireTimeSpan` Intervall ist verstrichen.</span><span class="sxs-lookup"><span data-stu-id="49a0c-256">A flag indicating whether the cookie expiration date resets when more than half of the `ExpireTimeSpan` interval has passed.</span></span> <span data-ttu-id="49a0c-257">Die neue Exipiration Zeit nach unten verschoben werden das aktuelle Datum plus dem `ExpireTimespan`.</span><span class="sxs-lookup"><span data-stu-id="49a0c-257">The new exipiration time is moved forward to be the current date plus the `ExpireTimespan`.</span></span> <span data-ttu-id="49a0c-258">Ein [absolute Cookie Ablaufzeit](xref:security/authentication/cookie#absolute-cookie-expiration) kann festgelegt werden, mithilfe der `AuthenticationProperties` -Klasse beim Aufrufen von `SignInAsync`.</span><span class="sxs-lookup"><span data-stu-id="49a0c-258">An [absolute cookie expiration time](xref:security/authentication/cookie#absolute-cookie-expiration) can be set by using the `AuthenticationProperties` class when calling `SignInAsync`.</span></span> <span data-ttu-id="49a0c-259">Eine absolute Ablaufzeit kann verbessern Sie die Sicherheit Ihrer App durch Einschränken der Menge der Zeit, die das Authentifizierungscookie gültig ist.</span><span class="sxs-lookup"><span data-stu-id="49a0c-259">An absolute expiration time can improve the security of your app by limiting the amount of time that the authentication cookie is valid.</span></span> <span data-ttu-id="49a0c-260">Der Standardwert ist `true`.</span><span class="sxs-lookup"><span data-stu-id="49a0c-260">The default value is `true`.</span></span> |

<span data-ttu-id="49a0c-261">Legen Sie `CookieAuthenticationOptions` für die Cookieauthentifizierungs-Middleware in der `Configure` Methode:</span><span class="sxs-lookup"><span data-stu-id="49a0c-261">Set `CookieAuthenticationOptions` for the Cookie Authentication Middleware in the `Configure` method:</span></span>

```csharp
app.UseCookieAuthentication(new CookieAuthenticationOptions
{
    ...
});
```

::: moniker-end

## <a name="cookie-policy-middleware"></a><span data-ttu-id="49a0c-262">Richtlinie Cookie-Middleware</span><span class="sxs-lookup"><span data-stu-id="49a0c-262">Cookie Policy Middleware</span></span>

<span data-ttu-id="49a0c-263">[Richtlinie Cookiemiddleware](/dotnet/api/microsoft.aspnetcore.cookiepolicy.cookiepolicymiddleware) Cookie Funktionen in einer app ermöglicht.</span><span class="sxs-lookup"><span data-stu-id="49a0c-263">[Cookie Policy Middleware](/dotnet/api/microsoft.aspnetcore.cookiepolicy.cookiepolicymiddleware) enables cookie policy capabilities in an app.</span></span> <span data-ttu-id="49a0c-264">Hinzufügen der Middleware zur Verarbeitungspipeline app ist die Reihenfolge, die vertrauliche; Sie wirkt sich nur auf Komponenten, die danach in der Pipeline registriert.</span><span class="sxs-lookup"><span data-stu-id="49a0c-264">Adding the middleware to the app processing pipeline is order sensitive; it only affects components registered after it in the pipeline.</span></span>

```csharp
app.UseCookiePolicy(cookiePolicyOptions);
```

 <span data-ttu-id="49a0c-265">Die [CookiePolicyOptions](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions) bereitgestellt, um die Cookie-Middleware für Richtlinie können Sie globale Eigenschaften der Cookie-Verarbeitung und Hook in von Cookie-Verarbeitungshandler zu steuern, wenn Cookies angefügt oder gelöscht werden.</span><span class="sxs-lookup"><span data-stu-id="49a0c-265">The [CookiePolicyOptions](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions) provided to the Cookie Policy Middleware allow you to control global characteristics of cookie processing and hook into cookie processing handlers when cookies are appended or deleted.</span></span>

| <span data-ttu-id="49a0c-266">Eigenschaft</span><span class="sxs-lookup"><span data-stu-id="49a0c-266">Property</span></span> | <span data-ttu-id="49a0c-267">Beschreibung</span><span class="sxs-lookup"><span data-stu-id="49a0c-267">Description</span></span> |
| -------- | ----------- |
| [<span data-ttu-id="49a0c-268">HttpOnly</span><span class="sxs-lookup"><span data-stu-id="49a0c-268">HttpOnly</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions.httponly) | <span data-ttu-id="49a0c-269">Wirkt sich auf, ob Cookies "HttpOnly" sein müssen, die ein Flag, der angibt, wenn das Cookie nur Server zugegriffen werden soll.</span><span class="sxs-lookup"><span data-stu-id="49a0c-269">Affects whether cookies must be HttpOnly, which is a flag indicating if the cookie should be accessible only to servers.</span></span> <span data-ttu-id="49a0c-270">Der Standardwert ist `HttpOnlyPolicy.None`.</span><span class="sxs-lookup"><span data-stu-id="49a0c-270">The default value is `HttpOnlyPolicy.None`.</span></span> |
| [<span data-ttu-id="49a0c-271">MinimumSameSitePolicy</span><span class="sxs-lookup"><span data-stu-id="49a0c-271">MinimumSameSitePolicy</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions.minimumsamesitepolicy) | <span data-ttu-id="49a0c-272">Wirkt sich auf das Cookie des SameSite-Attribut (siehe unten).</span><span class="sxs-lookup"><span data-stu-id="49a0c-272">Affects the cookie's same-site attribute (see below).</span></span> <span data-ttu-id="49a0c-273">Der Standardwert ist `SameSiteMode.Lax`.</span><span class="sxs-lookup"><span data-stu-id="49a0c-273">The default value is `SameSiteMode.Lax`.</span></span> <span data-ttu-id="49a0c-274">Diese Option ist für ASP.NET Core 2.0 und höher verfügbar.</span><span class="sxs-lookup"><span data-stu-id="49a0c-274">This option is available for ASP.NET Core 2.0+.</span></span> |
| [<span data-ttu-id="49a0c-275">OnAppendCookie</span><span class="sxs-lookup"><span data-stu-id="49a0c-275">OnAppendCookie</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions.onappendcookie) | <span data-ttu-id="49a0c-276">Aufgerufen, wenn ein Cookie angefügt wird.</span><span class="sxs-lookup"><span data-stu-id="49a0c-276">Called when a cookie is appended.</span></span> |
| [<span data-ttu-id="49a0c-277">OnDeleteCookie</span><span class="sxs-lookup"><span data-stu-id="49a0c-277">OnDeleteCookie</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions.ondeletecookie) | <span data-ttu-id="49a0c-278">Wird aufgerufen, wenn ein Cookie gelöscht wird.</span><span class="sxs-lookup"><span data-stu-id="49a0c-278">Called when a cookie is deleted.</span></span> |
| [<span data-ttu-id="49a0c-279">Sichern</span><span class="sxs-lookup"><span data-stu-id="49a0c-279">Secure</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions.secure) | <span data-ttu-id="49a0c-280">Wirkt sich auf, ob Cookies sicher sein müssen.</span><span class="sxs-lookup"><span data-stu-id="49a0c-280">Affects whether cookies must be Secure.</span></span> <span data-ttu-id="49a0c-281">Der Standardwert ist `CookieSecurePolicy.None`.</span><span class="sxs-lookup"><span data-stu-id="49a0c-281">The default value is `CookieSecurePolicy.None`.</span></span> |

<span data-ttu-id="49a0c-282">**MinimumSameSitePolicy** (ASP.NET Core 2.0 und höher nur)</span><span class="sxs-lookup"><span data-stu-id="49a0c-282">**MinimumSameSitePolicy** (ASP.NET Core 2.0+ only)</span></span>

<span data-ttu-id="49a0c-283">Der Standardwert `MinimumSameSitePolicy` Wert `SameSiteMode.Lax` um OAuth2-Authentifizierung zu ermöglichen.</span><span class="sxs-lookup"><span data-stu-id="49a0c-283">The default `MinimumSameSitePolicy` value is `SameSiteMode.Lax` to permit OAuth2 authentication.</span></span> <span data-ttu-id="49a0c-284">Genau eine SameSite-Richtlinie erzwingen `SameSiteMode.Strict`legen die `MinimumSameSitePolicy`.</span><span class="sxs-lookup"><span data-stu-id="49a0c-284">To strictly enforce a same-site policy of `SameSiteMode.Strict`, set the `MinimumSameSitePolicy`.</span></span> <span data-ttu-id="49a0c-285">Auch wenn diese Einstellung OAuth2 und andere Cross-Origin-Authentifizierungsschemas unterbrochen wird, erhöht sie die Sicherheitsstufe Cookie für andere Typen von apps, die Verarbeitung von Cross-Origin-Anforderungen nicht benötigen.</span><span class="sxs-lookup"><span data-stu-id="49a0c-285">Although this setting breaks OAuth2 and other cross-origin authentication schemes, it elevates the level of cookie security for other types of apps that don't rely on cross-origin request processing.</span></span>

```csharp
var cookiePolicyOptions = new CookiePolicyOptions
{
    MinimumSameSitePolicy = SameSiteMode.Strict,
};
```

<span data-ttu-id="49a0c-286">Die Richtlinie Cookie-Middleware-Einstellung für `MinimumSameSitePolicy` kann die Einstellung für beeinflussen `Cookie.SameSite` in `CookieAuthenticationOptions` gemäß der folgenden Matrix.</span><span class="sxs-lookup"><span data-stu-id="49a0c-286">The Cookie Policy Middleware setting for `MinimumSameSitePolicy` can affect your setting of `Cookie.SameSite` in `CookieAuthenticationOptions` settings according to the matrix below.</span></span>

| <span data-ttu-id="49a0c-287">MinimumSameSitePolicy</span><span class="sxs-lookup"><span data-stu-id="49a0c-287">MinimumSameSitePolicy</span></span> | <span data-ttu-id="49a0c-288">Cookie.SameSite</span><span class="sxs-lookup"><span data-stu-id="49a0c-288">Cookie.SameSite</span></span> | <span data-ttu-id="49a0c-289">Resultierende Cookie.SameSite-Einstellung</span><span class="sxs-lookup"><span data-stu-id="49a0c-289">Resultant Cookie.SameSite setting</span></span> |
| --------------------- | --------------- | --------------------------------- |
| <span data-ttu-id="49a0c-290">SameSiteMode.None</span><span class="sxs-lookup"><span data-stu-id="49a0c-290">SameSiteMode.None</span></span>     | <span data-ttu-id="49a0c-291">SameSiteMode.None</span><span class="sxs-lookup"><span data-stu-id="49a0c-291">SameSiteMode.None</span></span><br><span data-ttu-id="49a0c-292">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="49a0c-292">SameSiteMode.Lax</span></span><br><span data-ttu-id="49a0c-293">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="49a0c-293">SameSiteMode.Strict</span></span> | <span data-ttu-id="49a0c-294">SameSiteMode.None</span><span class="sxs-lookup"><span data-stu-id="49a0c-294">SameSiteMode.None</span></span><br><span data-ttu-id="49a0c-295">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="49a0c-295">SameSiteMode.Lax</span></span><br><span data-ttu-id="49a0c-296">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="49a0c-296">SameSiteMode.Strict</span></span> |
| <span data-ttu-id="49a0c-297">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="49a0c-297">SameSiteMode.Lax</span></span>      | <span data-ttu-id="49a0c-298">SameSiteMode.None</span><span class="sxs-lookup"><span data-stu-id="49a0c-298">SameSiteMode.None</span></span><br><span data-ttu-id="49a0c-299">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="49a0c-299">SameSiteMode.Lax</span></span><br><span data-ttu-id="49a0c-300">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="49a0c-300">SameSiteMode.Strict</span></span> | <span data-ttu-id="49a0c-301">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="49a0c-301">SameSiteMode.Lax</span></span><br><span data-ttu-id="49a0c-302">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="49a0c-302">SameSiteMode.Lax</span></span><br><span data-ttu-id="49a0c-303">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="49a0c-303">SameSiteMode.Strict</span></span> |
| <span data-ttu-id="49a0c-304">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="49a0c-304">SameSiteMode.Strict</span></span>   | <span data-ttu-id="49a0c-305">SameSiteMode.None</span><span class="sxs-lookup"><span data-stu-id="49a0c-305">SameSiteMode.None</span></span><br><span data-ttu-id="49a0c-306">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="49a0c-306">SameSiteMode.Lax</span></span><br><span data-ttu-id="49a0c-307">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="49a0c-307">SameSiteMode.Strict</span></span> | <span data-ttu-id="49a0c-308">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="49a0c-308">SameSiteMode.Strict</span></span><br><span data-ttu-id="49a0c-309">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="49a0c-309">SameSiteMode.Strict</span></span><br><span data-ttu-id="49a0c-310">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="49a0c-310">SameSiteMode.Strict</span></span> |

## <a name="create-an-authentication-cookie"></a><span data-ttu-id="49a0c-311">Erstellen Sie ein Authentifizierungscookie</span><span class="sxs-lookup"><span data-stu-id="49a0c-311">Create an authentication cookie</span></span>

<span data-ttu-id="49a0c-312">Um ein Cookie mit Benutzerinformationen zu erstellen, müssen Sie erstellen eine ["ClaimsPrincipal"](/dotnet/api/system.security.claims.claimsprincipal).</span><span class="sxs-lookup"><span data-stu-id="49a0c-312">To create a cookie holding user information, you must construct a [ClaimsPrincipal](/dotnet/api/system.security.claims.claimsprincipal).</span></span> <span data-ttu-id="49a0c-313">Die Benutzerinformationen serialisiert und im Cookie gespeichert.</span><span class="sxs-lookup"><span data-stu-id="49a0c-313">The user information is serialized and stored in the cookie.</span></span> 

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="49a0c-314">Erstellen Sie eine ["ClaimsIdentity"](/dotnet/api/system.security.claims.claimsidentity) mit ggf. erforderlichen [Anspruch](/dotnet/api/system.security.claims.claim)s, und rufen [SignInAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhttpcontextextensions.signinasync?view=aspnetcore-2.0) zum Anmelden des Benutzers:</span><span class="sxs-lookup"><span data-stu-id="49a0c-314">Create a [ClaimsIdentity](/dotnet/api/system.security.claims.claimsidentity) with any required [Claim](/dotnet/api/system.security.claims.claim)s and call [SignInAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhttpcontextextensions.signinasync?view=aspnetcore-2.0) to sign in the user:</span></span>

[!code-csharp[](cookie/samples/2.x/CookieSample/Pages/Account/Login.cshtml.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="49a0c-315">Rufen Sie [SignInAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhandler-1.signinasync?view=aspnetcore-1.1) zum Anmelden des Benutzers:</span><span class="sxs-lookup"><span data-stu-id="49a0c-315">Call [SignInAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhandler-1.signinasync?view=aspnetcore-1.1) to sign in the user:</span></span>

```csharp
await HttpContext.Authentication.SignInAsync(
    CookieAuthenticationDefaults.AuthenticationScheme,
    new ClaimsPrincipal(claimsIdentity));
```

::: moniker-end

<span data-ttu-id="49a0c-316">`SignInAsync` erstellt ein verschlüsseltes Cookie aus, und die aktuelle Antwort hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="49a0c-316">`SignInAsync` creates an encrypted cookie and adds it to the current response.</span></span> <span data-ttu-id="49a0c-317">Wenn Sie nicht angeben einer `AuthenticationScheme`, wird das Standardschema verwendet.</span><span class="sxs-lookup"><span data-stu-id="49a0c-317">If you don't specify an `AuthenticationScheme`, the default scheme is used.</span></span>

<span data-ttu-id="49a0c-318">Darüber hinaus ist die Verschlüsselung verwendet ASP.NET Core [den Datenschutz](xref:security/data-protection/using-data-protection#security-data-protection-getting-started) System.</span><span class="sxs-lookup"><span data-stu-id="49a0c-318">Under the covers, the encryption used is ASP.NET Core's [Data Protection](xref:security/data-protection/using-data-protection#security-data-protection-getting-started) system.</span></span> <span data-ttu-id="49a0c-319">Wenn Sie die app auf mehrere Computer, den Lastenausgleich für apps oder mit einer Webfarm hosten, müssen Sie [Schutz von Daten konfigurieren](xref:security/data-protection/configuration/overview) und mit dem Schlüsselbund für den gleichen app-Bezeichner.</span><span class="sxs-lookup"><span data-stu-id="49a0c-319">If you're hosting app on multiple machines, load balancing across apps, or using a web farm, then you must [configure data protection](xref:security/data-protection/configuration/overview) to use the same key ring and app identifier.</span></span>

## <a name="sign-out"></a><span data-ttu-id="49a0c-320">Abmelden</span><span class="sxs-lookup"><span data-stu-id="49a0c-320">Sign out</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="49a0c-321">Um den aktuellen Benutzer abmelden, und ihre Cookies zu löschen, rufen Sie [SignOutAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhttpcontextextensions.signoutasync?view=aspnetcore-2.0):</span><span class="sxs-lookup"><span data-stu-id="49a0c-321">To sign out the current user and delete their cookie, call [SignOutAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhttpcontextextensions.signoutasync?view=aspnetcore-2.0):</span></span>

[!code-csharp[](cookie/samples/2.x/CookieSample/Pages/Account/Login.cshtml.cs?name=snippet2)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="49a0c-322">Um den aktuellen Benutzer abmelden, und ihre Cookies zu löschen, rufen Sie [SignOutAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhandler-1.signoutasync?view=aspnetcore-1.1):</span><span class="sxs-lookup"><span data-stu-id="49a0c-322">To sign out the current user and delete their cookie, call [SignOutAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhandler-1.signoutasync?view=aspnetcore-1.1):</span></span>

```csharp
await HttpContext.Authentication.SignOutAsync(
    CookieAuthenticationDefaults.AuthenticationScheme);
```

::: moniker-end

<span data-ttu-id="49a0c-323">Wenn Sie nicht verwenden `CookieAuthenticationDefaults.AuthenticationScheme` (oder "Cookies") als das Schema (z. B. "ContosoCookie"), geben Sie das Schema, das Sie beim Konfigurieren des Authentifizierungsanbieters verwendet.</span><span class="sxs-lookup"><span data-stu-id="49a0c-323">If you aren't using `CookieAuthenticationDefaults.AuthenticationScheme` (or "Cookies") as the scheme (for example, "ContosoCookie"), supply the scheme you used when configuring the authentication provider.</span></span> <span data-ttu-id="49a0c-324">Andernfalls wird das Standardschema verwendet.</span><span class="sxs-lookup"><span data-stu-id="49a0c-324">Otherwise, the default scheme is used.</span></span>

## <a name="react-to-back-end-changes"></a><span data-ttu-id="49a0c-325">Reagieren Sie auf Änderungen der Back-end</span><span class="sxs-lookup"><span data-stu-id="49a0c-325">React to back-end changes</span></span>

<span data-ttu-id="49a0c-326">Sobald ein Cookie erstellt wurde, wird es die einzige Quelle der Identität.</span><span class="sxs-lookup"><span data-stu-id="49a0c-326">Once a cookie is created, it becomes the single source of identity.</span></span> <span data-ttu-id="49a0c-327">Auch wenn Sie einen Benutzer in Ihrem Back-End-Systemen deaktiviert haben, das Cookie-Authentifizierungssystem hat keine Kenntnis davon aus, und ein Benutzer bleibt angemeldet, solange ihre Cookie gültig ist.</span><span class="sxs-lookup"><span data-stu-id="49a0c-327">Even if you disable a user in your back-end systems, the cookie authentication system has no knowledge of this, and a user stays logged in as long as their cookie is valid.</span></span>

<span data-ttu-id="49a0c-328">Die [ValidatePrincipal](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationevents.validateprincipal) Ereignis in ASP.NET Core 2.x oder [ValidateAsync](/dotnet/api/microsoft.aspnetcore.identity.isecuritystampvalidator.validateasync?view=aspnetcore-1.1) -Methode in der ASP.NET Core 1.x abfangen und überschreiben die Überprüfung der Identität ein Cookie verwendet werden können.</span><span class="sxs-lookup"><span data-stu-id="49a0c-328">The [ValidatePrincipal](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationevents.validateprincipal) event in ASP.NET Core 2.x or the [ValidateAsync](/dotnet/api/microsoft.aspnetcore.identity.isecuritystampvalidator.validateasync?view=aspnetcore-1.1) method in ASP.NET Core 1.x can be used to intercept and override validation of the cookie identity.</span></span> <span data-ttu-id="49a0c-329">Dieser Ansatz verringert das Risiko von gesperrten Benutzer auf die app zugreifen.</span><span class="sxs-lookup"><span data-stu-id="49a0c-329">This approach mitigates the risk of revoked users accessing the app.</span></span>

<span data-ttu-id="49a0c-330">Ein Ansatz für die gültigkeitsprüfung basiert zu verfolgen, wenn die Datenbank geändert wurde.</span><span class="sxs-lookup"><span data-stu-id="49a0c-330">One approach to cookie validation is based on keeping track of when the user database has been changed.</span></span> <span data-ttu-id="49a0c-331">Wenn die Datenbank geändert wurde, nachdem der Benutzer Cookies ausgegeben wurde, ist es nicht erforderlich, die der Benutzer erneut authentifizieren, wenn ihre Cookie noch gültig ist.</span><span class="sxs-lookup"><span data-stu-id="49a0c-331">If the database hasn't been changed since the user's cookie was issued, there's no need to re-authenticate the user if their cookie is still valid.</span></span> <span data-ttu-id="49a0c-332">Zu diesem Szenario wird die Datenbank zu implementieren, die Implementierung in `IUserRepository` in diesem Beispiel speichert ein `LastChanged` Wert.</span><span class="sxs-lookup"><span data-stu-id="49a0c-332">To implement this scenario, the database, which is implemented in `IUserRepository` for this example, stores a `LastChanged` value.</span></span> <span data-ttu-id="49a0c-333">Wenn alle Benutzer in der Datenbank aktualisiert wird die `LastChanged` Wert wird auf die aktuelle Uhrzeit festgelegt.</span><span class="sxs-lookup"><span data-stu-id="49a0c-333">When any user is updated in the database, the `LastChanged` value is set to the current time.</span></span>

<span data-ttu-id="49a0c-334">Um ein Cookie für ungültig erklären, wenn die Änderungen in der Datenbank auf Grundlage der `LastChanged` Wert, erstellen Sie das Cookie mit einem `LastChanged` Anspruch mit dem aktuellen `LastChanged` Wert aus der Datenbank:</span><span class="sxs-lookup"><span data-stu-id="49a0c-334">In order to invalidate a cookie when the database changes based on the `LastChanged` value, create the cookie with a `LastChanged` claim containing the current `LastChanged` value from the database:</span></span>

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

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="49a0c-335">Implementieren Sie eine Außerkraftsetzung für die `ValidatePrincipal` Ereignis schreiben, eine Methode mit der folgenden Signatur in einer Klasse, die Sie ableiten [CookieAuthenticationEvents](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationevents):</span><span class="sxs-lookup"><span data-stu-id="49a0c-335">To implement an override for the `ValidatePrincipal` event, write a method with the following signature in a class that you derive from [CookieAuthenticationEvents](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationevents):</span></span>

```csharp
ValidatePrincipal(CookieValidatePrincipalContext)
```

<span data-ttu-id="49a0c-336">Ein Beispiel sieht folgendermaßen aus:</span><span class="sxs-lookup"><span data-stu-id="49a0c-336">An example looks like the following:</span></span>

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

<span data-ttu-id="49a0c-337">Registrieren Sie die Ereignisse-Instanz während der Cookie-Service-Registrierung in der `ConfigureServices` Methode.</span><span class="sxs-lookup"><span data-stu-id="49a0c-337">Register the events instance during cookie service registration in the `ConfigureServices` method.</span></span> <span data-ttu-id="49a0c-338">Geben Sie eine Bereichsbezogene Service-Registrierung für Ihre `CustomCookieAuthenticationEvents` Klasse:</span><span class="sxs-lookup"><span data-stu-id="49a0c-338">Provide a scoped service registration for your `CustomCookieAuthenticationEvents` class:</span></span>

```csharp
services.AddAuthentication(CookieAuthenticationDefaults.AuthenticationScheme)
    .AddCookie(options =>
    {
        options.EventsType = typeof(CustomCookieAuthenticationEvents);
    });

services.AddScoped<CustomCookieAuthenticationEvents>();
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="49a0c-339">Implementieren Sie eine Außerkraftsetzung für die `ValidateAsync` Ereignis schreiben, eine Methode mit der folgenden Signatur:</span><span class="sxs-lookup"><span data-stu-id="49a0c-339">To implement an override for the `ValidateAsync` event, write a method with the following signature:</span></span>

```csharp
ValidateAsync(CookieValidatePrincipalContext)
```

<span data-ttu-id="49a0c-340">ASP.NET Core Identity implementiert diese Überprüfung als Teil seiner [SecurityStampValidator](/dotnet/api/microsoft.aspnetcore.identity.securitystampvalidator-1.validateasync).</span><span class="sxs-lookup"><span data-stu-id="49a0c-340">ASP.NET Core Identity implements this check as part of its [SecurityStampValidator](/dotnet/api/microsoft.aspnetcore.identity.securitystampvalidator-1.validateasync).</span></span> <span data-ttu-id="49a0c-341">Ein Beispiel sieht folgendermaßen aus:</span><span class="sxs-lookup"><span data-stu-id="49a0c-341">An example looks like the following:</span></span>

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

<span data-ttu-id="49a0c-342">Registrieren Sie das Ereignis während der Konfiguration der Cookie-Authentifizierung in der `Configure` Methode:</span><span class="sxs-lookup"><span data-stu-id="49a0c-342">Register the event during cookie authentication configuration in the `Configure` method:</span></span>

```csharp
app.UseCookieAuthentication(new CookieAuthenticationOptions
{
    Events = new CookieAuthenticationEvents
    {
        OnValidatePrincipal = LastChangedValidator.ValidateAsync
    }
});
```

::: moniker-end

<span data-ttu-id="49a0c-343">Betrachten Sie eine Situation, in dem der Name des Benutzers aktualisiert &mdash; eine Entscheidung, die Sicherheit in irgendeiner Weise nicht beeinträchtigt.</span><span class="sxs-lookup"><span data-stu-id="49a0c-343">Consider a situation in which the user's name is updated &mdash; a decision that doesn't affect security in any way.</span></span> <span data-ttu-id="49a0c-344">Wenn Sie den Benutzerprinzipal zerstörungsfrei aktualisieren möchten, rufen Sie `context.ReplacePrincipal` und legen Sie die `context.ShouldRenew` Eigenschaft `true`.</span><span class="sxs-lookup"><span data-stu-id="49a0c-344">If you want to non-destructively update the user principal, call `context.ReplacePrincipal` and set the `context.ShouldRenew` property to `true`.</span></span>

> [!WARNING]
> <span data-ttu-id="49a0c-345">Die hier beschriebene Vorgehensweise wird bei jeder Anforderung ausgelöst.</span><span class="sxs-lookup"><span data-stu-id="49a0c-345">The approach described here is triggered on every request.</span></span> <span data-ttu-id="49a0c-346">Dies kann zu einer Leistungseinbuße für die app führen.</span><span class="sxs-lookup"><span data-stu-id="49a0c-346">This can result in a large performance penalty for the app.</span></span>

## <a name="persistent-cookies"></a><span data-ttu-id="49a0c-347">Beständige cookies</span><span class="sxs-lookup"><span data-stu-id="49a0c-347">Persistent cookies</span></span>

<span data-ttu-id="49a0c-348">Sie sollten die Cookies, das über Browsersitzungen hinweg beizubehalten.</span><span class="sxs-lookup"><span data-stu-id="49a0c-348">You may want the cookie to persist across browser sessions.</span></span> <span data-ttu-id="49a0c-349">Diese Persistenz sollte nur mit expliziten benutzerzustimmung mit "Erinnerung" das Kontrollkästchen für die Anmeldung oder ein ähnlicher Mechanismus aktiviert werden.</span><span class="sxs-lookup"><span data-stu-id="49a0c-349">This persistence should only be enabled with explicit user consent with a "Remember Me" check box on login or a similar mechanism.</span></span> 

<span data-ttu-id="49a0c-350">Der folgende Codeausschnitt erstellt eine Identität und die entsprechenden Cookie, das über den Browser Closures zurückgegriffen werden kann.</span><span class="sxs-lookup"><span data-stu-id="49a0c-350">The following code snippet creates an identity and corresponding cookie that survives through browser closures.</span></span> <span data-ttu-id="49a0c-351">Alle gleitenden ablaufeinstellungen, die zuvor konfiguriert haben, werden berücksichtigt.</span><span class="sxs-lookup"><span data-stu-id="49a0c-351">Any sliding expiration settings previously configured are honored.</span></span> <span data-ttu-id="49a0c-352">Wenn das Cookie abläuft, während der Browser geschlossen wird, löscht der Browser das Cookie an, nachdem er neu gestartet wurde.</span><span class="sxs-lookup"><span data-stu-id="49a0c-352">If the cookie expires while the browser is closed, the browser clears the cookie once it's restarted.</span></span>

::: moniker range=">= aspnetcore-2.0"

```csharp
await HttpContext.SignInAsync(
    CookieAuthenticationDefaults.AuthenticationScheme,
    new ClaimsPrincipal(claimsIdentity),
    new AuthenticationProperties
    {
        IsPersistent = true
    });
```

<span data-ttu-id="49a0c-353">Die [AuthenticationProperties](/dotnet/api/microsoft.aspnetcore.authentication.authenticationproperties?view=aspnetcore-2.0) Klasse befindet sich in der `Microsoft.AspNetCore.Authentication` Namespace.</span><span class="sxs-lookup"><span data-stu-id="49a0c-353">The [AuthenticationProperties](/dotnet/api/microsoft.aspnetcore.authentication.authenticationproperties?view=aspnetcore-2.0) class resides in the `Microsoft.AspNetCore.Authentication` namespace.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
await HttpContext.Authentication.SignInAsync(
    CookieAuthenticationDefaults.AuthenticationScheme,
    new ClaimsPrincipal(claimsIdentity),
    new AuthenticationProperties
    {
        IsPersistent = true
    });
```

<span data-ttu-id="49a0c-354">Die [AuthenticationProperties](/dotnet/api/microsoft.aspnetcore.http.authentication.authenticationproperties?view=aspnetcore-1.1) Klasse befindet sich in der `Microsoft.AspNetCore.Http.Authentication` Namespace.</span><span class="sxs-lookup"><span data-stu-id="49a0c-354">The [AuthenticationProperties](/dotnet/api/microsoft.aspnetcore.http.authentication.authenticationproperties?view=aspnetcore-1.1) class resides in the `Microsoft.AspNetCore.Http.Authentication` namespace.</span></span>

::: moniker-end

## <a name="absolute-cookie-expiration"></a><span data-ttu-id="49a0c-355">Absolute cookieablauf</span><span class="sxs-lookup"><span data-stu-id="49a0c-355">Absolute cookie expiration</span></span>

<span data-ttu-id="49a0c-356">Sie können festlegen, dass eine absolute Ablaufzeit mit `ExpiresUtc`.</span><span class="sxs-lookup"><span data-stu-id="49a0c-356">You can set an absolute expiration time with `ExpiresUtc`.</span></span> <span data-ttu-id="49a0c-357">Außerdem müssen Sie festlegen `IsPersistent`ist, andernfalls `ExpiresUtc` wird ignoriert, und es wird ein Single-Sitzungs-Cookie erstellt.</span><span class="sxs-lookup"><span data-stu-id="49a0c-357">You must also set `IsPersistent`; otherwise, `ExpiresUtc` is ignored and a single-session cookie is created.</span></span> <span data-ttu-id="49a0c-358">Beim `ExpiresUtc` wird festgelegt, `SignInAsync`, er überschreibt den Wert von der `ExpireTimeSpan` Option `CookieAuthenticationOptions`, sofern festgelegt.</span><span class="sxs-lookup"><span data-stu-id="49a0c-358">When `ExpiresUtc` is set on `SignInAsync`, it overrides the value of the `ExpireTimeSpan` option of `CookieAuthenticationOptions`, if set.</span></span>

<span data-ttu-id="49a0c-359">Der folgende Codeausschnitt erstellt eine Identität und die entsprechenden Cookie, das in der Regel 20 Minuten dauert.</span><span class="sxs-lookup"><span data-stu-id="49a0c-359">The following code snippet creates an identity and corresponding cookie that lasts for 20 minutes.</span></span> <span data-ttu-id="49a0c-360">Dies ignoriert alle gleitenden ablaufeinstellungen, die zuvor konfiguriert haben.</span><span class="sxs-lookup"><span data-stu-id="49a0c-360">This ignores any sliding expiration settings previously configured.</span></span>

::: moniker range=">= aspnetcore-2.0"

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

::: moniker-end

::: moniker range="< aspnetcore-2.0"

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

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="49a0c-361">Zusätzliche Ressourcen</span><span class="sxs-lookup"><span data-stu-id="49a0c-361">Additional resources</span></span>

* [<span data-ttu-id="49a0c-362">Auth 2.0 Änderungen / Migration-Ankündigung</span><span class="sxs-lookup"><span data-stu-id="49a0c-362">Auth 2.0 Changes / Migration Announcement</span></span>](https://github.com/aspnet/Announcements/issues/262)
* <xref:security/authorization/limitingidentitybyscheme>
* <xref:security/authorization/claims>
* [<span data-ttu-id="49a0c-363">Überprüfen der Richtlinie der richtlinienbasierten-Funktion</span><span class="sxs-lookup"><span data-stu-id="49a0c-363">Policy-based role checks</span></span>](xref:security/authorization/roles#policy-based-role-checks)
* <xref:host-and-deploy/web-farm>
