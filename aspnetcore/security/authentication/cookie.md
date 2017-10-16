---
title: "Mithilfe der Cookieauthentifizierung ohne ASP.NET Core Identität"
author: rick-anderson
description: "Eine Erläuterung der Verwendung von Cookieauthentifizierung ohne ASP.NET Core Identity"
keywords: ASP.NET Core, cookies
ms.author: riande
manager: wpickett
ms.date: 08/14/2017
ms.topic: article
ms.assetid: 2bdcbf95-8d9d-4537-a4a0-a5ee439dcb62
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authentication/cookie
ms.openlocfilehash: ea9c93e34a3242b5b3716404228edb8902baf625
ms.sourcegitcommit: e3b1726cc04e80dc28464c35259edbd3bc39a438
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/12/2017
---
# <a name="using-cookie-authentication-without-aspnet-core-identity"></a><span data-ttu-id="48955-104">Mithilfe der Cookieauthentifizierung ohne ASP.NET Core Identität</span><span class="sxs-lookup"><span data-stu-id="48955-104">Using Cookie Authentication without ASP.NET Core Identity</span></span>

<a name="security-authentication-cookie-middleware"></a>

<span data-ttu-id="48955-105">ASP.NET Core 1.x bietet Cookie [Middleware](../../fundamentals/middleware.md#fundamentals-middleware) der serialisiert eines Benutzerprinzipals in einem verschlüsselten Cookie, und klicken Sie dann bei nachfolgenden Anforderungen überprüft das Cookie wird den Prinzipal erstellt, und weist sie der `HttpContext.User` Eigenschaft .</span><span class="sxs-lookup"><span data-stu-id="48955-105">ASP.NET Core 1.x provides cookie [middleware](../../fundamentals/middleware.md#fundamentals-middleware) which serializes a user principal into an encrypted cookie and then, on subsequent requests, validates the cookie, recreates the principal, and assigns it to the `HttpContext.User` property.</span></span> <span data-ttu-id="48955-106">Wenn Sie eine eigene Anmeldung Bildschirme und die Benutzerdatenbanken bereitstellen möchten, können Sie die Cookie-Middleware als eigenständiges Feature.</span><span class="sxs-lookup"><span data-stu-id="48955-106">If you want to provide your own login screens and user databases, you can use the cookie middleware as a standalone feature.</span></span>

<span data-ttu-id="48955-107">Eine wesentliche Änderung in ASP.NET Core 2.x ist, dass die Cookie-Middleware nicht vorhanden ist.</span><span class="sxs-lookup"><span data-stu-id="48955-107">A major change in ASP.NET Core 2.x is that the cookie middleware is absent.</span></span> <span data-ttu-id="48955-108">Stattdessen die `UseAuthentication` Methodenaufruf in der `Configure` Methode *Startup.cs* fügt die AuthenticationMiddleware festgelegt die `HttpContext.User` Eigenschaft.</span><span class="sxs-lookup"><span data-stu-id="48955-108">Instead, the `UseAuthentication` method invocation in the `Configure` method of *Startup.cs* adds the AuthenticationMiddleware which sets the `HttpContext.User` property.</span></span>

<a name="security-authentication-cookie-middleware-configuring"></a>

## <a name="adding-and-configuring"></a><span data-ttu-id="48955-109">Hinzufügen und konfigurieren</span><span class="sxs-lookup"><span data-stu-id="48955-109">Adding and configuring</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="48955-110">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="48955-110">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="48955-111">Führen Sie die folgenden Schritte aus:</span><span class="sxs-lookup"><span data-stu-id="48955-111">Complete the following steps:</span></span>

- <span data-ttu-id="48955-112">Wenn Sie nicht mit der `Microsoft.AspNetCore.All` [Metapackage](xref:fundamentals/metapackage), installieren Sie Version 2.0 + von der `Microsoft.AspNetCore.Authentication.Cookies` NuGet-Paket im Projekt.</span><span class="sxs-lookup"><span data-stu-id="48955-112">If not using the `Microsoft.AspNetCore.All` [metapackage](xref:fundamentals/metapackage), install version 2.0+ of the `Microsoft.AspNetCore.Authentication.Cookies` NuGet package in your project.</span></span>

- <span data-ttu-id="48955-113">Aufrufen der `UseAuthentication` Methode in der `Configure` Methode der *Startup.cs* Datei:</span><span class="sxs-lookup"><span data-stu-id="48955-113">Invoke the `UseAuthentication` method in the `Configure` method of the *Startup.cs* file:</span></span>

    ```csharp
    app.UseAuthentication();
    ```

- <span data-ttu-id="48955-114">Aufrufen der `AddAuthentication` und `AddCookie` Methoden in der `ConfigureServices` Methode der *Startup.cs* Datei:</span><span class="sxs-lookup"><span data-stu-id="48955-114">Invoke the `AddAuthentication` and `AddCookie` methods in the `ConfigureServices` method of the *Startup.cs* file:</span></span>

    ```csharp
    services.AddAuthentication("MyCookieAuthenticationScheme")
            .AddCookie("MyCookieAuthenticationScheme", options => {
                options.AccessDeniedPath = "/Account/Forbidden/";
                options.LoginPath = "/Account/Unauthorized/";
            });
    ```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="48955-115">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="48955-115">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="48955-116">Führen Sie die folgenden Schritte aus:</span><span class="sxs-lookup"><span data-stu-id="48955-116">Complete the following steps:</span></span>

- <span data-ttu-id="48955-117">Installieren der `Microsoft.AspNetCore.Authentication.Cookies` NuGet-Paket im Projekt.</span><span class="sxs-lookup"><span data-stu-id="48955-117">Install the `Microsoft.AspNetCore.Authentication.Cookies` NuGet package in your project.</span></span> <span data-ttu-id="48955-118">Dieses Paket enthält die Cookie-Middleware.</span><span class="sxs-lookup"><span data-stu-id="48955-118">This package contains the cookie middleware.</span></span>

- <span data-ttu-id="48955-119">Fügen Sie die folgenden Zeilen, die der `Configure` Methode in Ihrer *Startup.cs* Datei vor dem der `app.UseMvc()` Anweisung:</span><span class="sxs-lookup"><span data-stu-id="48955-119">Add the following lines to the `Configure` method in your *Startup.cs* file before the `app.UseMvc()` statement:</span></span>

    ```csharp
    app.UseCookieAuthentication(new CookieAuthenticationOptions()
    {
        AccessDeniedPath = "/Account/Forbidden/",
        AuthenticationScheme = "MyCookieAuthenticationScheme",
        AutomaticAuthenticate = true,
        AutomaticChallenge = true,
        LoginPath = "/Account/Unauthorized/"
    });
    ```

---

<span data-ttu-id="48955-120">Die oben aufgeführten Codeausschnitte konfigurieren Sie einige oder alle der folgenden Optionen:</span><span class="sxs-lookup"><span data-stu-id="48955-120">The code snippets above configure some or all of the following options:</span></span>

* <span data-ttu-id="48955-121">`AccessDeniedPath`-Dies ist der relative Pfad auf die Anforderungen weitergeleitet wird, wenn ein Benutzer versucht, auf eine Ressource zuzugreifen, aber keine besteht [Autorisierungsrichtlinien](xref:security/authorization/policies#security-authorization-policies-based) für diese Ressource.</span><span class="sxs-lookup"><span data-stu-id="48955-121">`AccessDeniedPath` - This is the relative path to which requests redirect when a user attempts to access a resource but does not pass any [authorization policies](xref:security/authorization/policies#security-authorization-policies-based) for that resource.</span></span>

* <span data-ttu-id="48955-122">`AuthenticationScheme`-Dies ist ein Wert, der unter dem ein bestimmtes Cookie-Authentifizierungsschema bekannt ist.</span><span class="sxs-lookup"><span data-stu-id="48955-122">`AuthenticationScheme` - This is a value by which a particular cookie authentication scheme is known.</span></span> <span data-ttu-id="48955-123">Dies ist nützlich, wenn mehrere der Cookieauthentifizierung und der app muss Instanzen [Beschränken der Autorisierung mit einer Instanz](xref:security/authorization/limitingidentitybyscheme).</span><span class="sxs-lookup"><span data-stu-id="48955-123">This is useful when there are multiple instances of cookie authentication and the app needs to [limit authorization to one instance](xref:security/authorization/limitingidentitybyscheme).</span></span>

* <span data-ttu-id="48955-124">`AutomaticAuthenticate`-Dieses Flag ist nur für ASP.NET Core relevant 1.x.</span><span class="sxs-lookup"><span data-stu-id="48955-124">`AutomaticAuthenticate` - This flag is relevant only for ASP.NET Core 1.x.</span></span> <span data-ttu-id="48955-125">Er gibt an, dass die Cookieauthentifizierung für jede Anforderung ausgeführt werden soll, und versuchen, zu überprüfen und rekonstruieren serialisierten Prinzipal, der Sie erstellt.</span><span class="sxs-lookup"><span data-stu-id="48955-125">It indicates that the cookie authentication should run on every request and attempt to validate and reconstruct any serialized principal it created.</span></span>

* <span data-ttu-id="48955-126">`AutomaticChallenge`-Dieses Flag ist nur für ASP.NET Core relevant 1.x.</span><span class="sxs-lookup"><span data-stu-id="48955-126">`AutomaticChallenge` - This flag is relevant only for ASP.NET Core 1.x.</span></span> <span data-ttu-id="48955-127">Angenommen, dass die 1.x Cookieauthentifizierung den Browser zum umleiten soll die `LoginPath` oder `AccessDeniedPath` Fehler bei der Autorisierung.</span><span class="sxs-lookup"><span data-stu-id="48955-127">It indicates that the 1.x cookie authentication should redirect the browser to the `LoginPath` or `AccessDeniedPath` when authorization fails.</span></span>

* <span data-ttu-id="48955-128">`LoginPath`-Dies ist der relative Pfad zu dem Anforderungen umgeleitet werden soll, wenn ein Benutzer versucht, eine Ressource zuzugreifen, aber wurde nicht authentifiziert.</span><span class="sxs-lookup"><span data-stu-id="48955-128">`LoginPath` - This is the relative path to which requests redirect when a user attempts to access a resource but has not been authenticated.</span></span>

<span data-ttu-id="48955-129">[Andere Optionen](xref:security/authentication/cookie#security-authentication-cookie-options) gehören die Möglichkeit, den Aussteller für Ansprüche, Festlegen der Cookieauthentifizierung erstellt hat, der Namen des Cookies die Authentifizierung absinkt, die Domäne für das Cookie und verschiedene Sicherheitseigenschaften für das Cookie.</span><span class="sxs-lookup"><span data-stu-id="48955-129">[Other options](xref:security/authentication/cookie#security-authentication-cookie-options) include the ability to set the issuer for any claims the cookie authentication creates, the name of the cookie the authentication drops, the domain for the cookie and various security properties on the cookie.</span></span> <span data-ttu-id="48955-130">Standardmäßig verwendet der Cookieauthentifizierung entsprechende Sicherheitsoptionen für Cookies erstellt, z. B.:</span><span class="sxs-lookup"><span data-stu-id="48955-130">By default, the cookie authentication uses appropriate security options for any cookies it creates, such as:</span></span>
- <span data-ttu-id="48955-131">Wenn das Flag HttpOnly Cookie Zugriff in clientseitiges JavaScript zu verhindern</span><span class="sxs-lookup"><span data-stu-id="48955-131">Setting the HttpOnly flag to prevent cookie access in client-side JavaScript</span></span>
- <span data-ttu-id="48955-132">Das Cookie auf HTTPS beschränken, wenn eine Anforderung über HTTPS zurückgelegt hat</span><span class="sxs-lookup"><span data-stu-id="48955-132">Limiting the cookie to HTTPS if a request has traveled over HTTPS</span></span>

<a name="security-authentication-cookie-middleware-creating-a-cookie"></a>

## <a name="creating-an-identity-cookie"></a><span data-ttu-id="48955-133">Erstellen ein Cookie Identität</span><span class="sxs-lookup"><span data-stu-id="48955-133">Creating an Identity cookie</span></span>

<span data-ttu-id="48955-134">Um ein Cookie, halten Ihre Benutzerinformationen zu erstellen, erstellen Sie eine [ClaimsPrincipal](https://docs.microsoft.com/dotnet/api/system.security.claims.claimsprincipal) mit den Informationen in Cookies serialisiert werden soll.</span><span class="sxs-lookup"><span data-stu-id="48955-134">To create a cookie holding your user information, you must construct a [ClaimsPrincipal](https://docs.microsoft.com/dotnet/api/system.security.claims.claimsprincipal) holding the information you wish to be serialized in the cookie.</span></span> <span data-ttu-id="48955-135">Nachdem Sie eine geeignete haben `ClaimsPrincipal` -Objekt, rufen Sie die folgenden in der Controllermethode:</span><span class="sxs-lookup"><span data-stu-id="48955-135">Once you have a suitable `ClaimsPrincipal` object, call the following inside your controller method:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="48955-136">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="48955-136">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
await HttpContext.SignInAsync("MyCookieAuthenticationScheme", principal);
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="48955-137">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="48955-137">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
await HttpContext.Authentication.SignInAsync("MyCookieAuthenticationScheme", principal);
```

---

<span data-ttu-id="48955-138">Dies ein verschlüsseltes Cookies erstellt und der aktuellen Antwort hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="48955-138">This creates an encrypted cookie and adds it to the current response.</span></span> <span data-ttu-id="48955-139">Die `AuthenticationScheme` während des angegebenen [Konfiguration](xref:security/authentication/cookie#security-authentication-cookie-middleware-configuring) muss verwendet werden, beim Aufrufen von `SignInAsync`.</span><span class="sxs-lookup"><span data-stu-id="48955-139">The `AuthenticationScheme` specified during [configuration](xref:security/authentication/cookie#security-authentication-cookie-middleware-configuring) must be used when calling `SignInAsync`.</span></span>

<span data-ttu-id="48955-140">Im Hintergrund, wird die Verschlüsselung verwendet ASP.NET Core des [Datenschutz](xref:security/data-protection/using-data-protection#security-data-protection-getting-started) System.</span><span class="sxs-lookup"><span data-stu-id="48955-140">Under the covers, the encryption used is ASP.NET Core's [Data Protection](xref:security/data-protection/using-data-protection#security-data-protection-getting-started) system.</span></span> <span data-ttu-id="48955-141">Wenn Sie auf mehreren Computern, Load, Lastenausgleich oder mithilfe einer Webfarm gehostet werden, dann Sie müssen [Schutz von Daten konfigurieren](xref:security/data-protection/configuration/overview#data-protection-configuring) und mit dem gleichen Schlüssel Ring Anwendungsbezeichner.</span><span class="sxs-lookup"><span data-stu-id="48955-141">If you are hosting on multiple machines, load balancing, or using a web farm, then you need to [configure data protection](xref:security/data-protection/configuration/overview#data-protection-configuring) to use the same key ring and application identifier.</span></span>

## <a name="signing-out"></a><span data-ttu-id="48955-142">Abmelden</span><span class="sxs-lookup"><span data-stu-id="48955-142">Signing out</span></span>

<span data-ttu-id="48955-143">Um den aktuellen Benutzer abmelden und ihre Cookies zu löschen, rufen Sie die folgenden innerhalb Ihres Controllers:</span><span class="sxs-lookup"><span data-stu-id="48955-143">To sign out the current user and delete their cookie, call the following inside your controller:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="48955-144">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="48955-144">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
await HttpContext.SignOutAsync("MyCookieAuthenticationScheme");
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="48955-145">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="48955-145">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
await HttpContext.Authentication.SignOutAsync("MyCookieAuthenticationScheme");
```

---

## <a name="reacting-to-back-end-changes"></a><span data-ttu-id="48955-146">Das reagieren auf Back-End-Änderungen</span><span class="sxs-lookup"><span data-stu-id="48955-146">Reacting to back-end changes</span></span>

>[!WARNING]
> <span data-ttu-id="48955-147">Nachdem ein Cookie Prinzipal erstellt wurde, wird es die einzige Quelle der Identität.</span><span class="sxs-lookup"><span data-stu-id="48955-147">Once a principal cookie has been created, it becomes the single source of identity.</span></span> <span data-ttu-id="48955-148">Auch wenn Sie einen Benutzer in Ihrem Back-End-Systemen deaktivieren, die Cookieauthentifizierung hat keine Kenntnis davon, und ein Benutzer angemeldet bleibt, solange ihre Cookie gültig ist.</span><span class="sxs-lookup"><span data-stu-id="48955-148">Even if you disable a user in your back-end systems, the cookie authentication has no knowledge of this, and a user stays logged in as long as their cookie is valid.</span></span>

<span data-ttu-id="48955-149">Der Cookieauthentifizierung bietet eine Reihe von Ereignissen in der Optionsklasse.</span><span class="sxs-lookup"><span data-stu-id="48955-149">The cookie authentication provides a series of events in its option class.</span></span> <span data-ttu-id="48955-150">Die `ValidateAsync()` -Ereignis kann zum Abfangen und Überschreiben der Überprüfung der Identität des Cookies verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="48955-150">The `ValidateAsync()` event can be used to intercept and override validation of the cookie identity.</span></span>

<span data-ttu-id="48955-151">Erwägen Sie eine Back-End-Benutzerdatenbank, die möglicherweise eine Spalte "LastChanged" aufweisen.</span><span class="sxs-lookup"><span data-stu-id="48955-151">Consider a back-end user database that may have a "LastChanged" column.</span></span> <span data-ttu-id="48955-152">Um ein Cookie für ungültig erklärt werden, wenn die Datenbank geändert wird, sollten Sie zuerst, wenn [erstellen das Cookie](xref:security/authentication/cookie#security-authentication-cookie-middleware-creating-a-cookie), fügen Sie eine "LastChanged"-Anspruch mit dem aktuellen Wert hinzu.</span><span class="sxs-lookup"><span data-stu-id="48955-152">In order to invalidate a cookie when the database changes, you should first, when [creating the cookie](xref:security/authentication/cookie#security-authentication-cookie-middleware-creating-a-cookie), add a "LastChanged" claim containing the current value.</span></span> <span data-ttu-id="48955-153">Wenn die Datenbank geändert wird, sollten der Wert "LastChanged" aktualisiert werden.</span><span class="sxs-lookup"><span data-stu-id="48955-153">When the database changes, the "LastChanged" value should be updated.</span></span>

<span data-ttu-id="48955-154">Implementieren Sie eine Außerkraftsetzung für die `ValidateAsync()` Ereignis müssen Sie eine Methode mit der folgenden Signatur schreiben:</span><span class="sxs-lookup"><span data-stu-id="48955-154">To implement an override for the `ValidateAsync()` event, you must write a method with the following signature:</span></span>

```csharp
Task ValidateAsync(CookieValidatePrincipalContext context);
```

<span data-ttu-id="48955-155">ASP.NET Core Identity implementiert diese Überprüfung als Teil seiner `SecurityStampValidator`.</span><span class="sxs-lookup"><span data-stu-id="48955-155">ASP.NET Core Identity implements this check as part of its `SecurityStampValidator`.</span></span> <span data-ttu-id="48955-156">Ein Beispiel sieht wie folgt aus:</span><span class="sxs-lookup"><span data-stu-id="48955-156">An example looks like the following:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="48955-157">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="48955-157">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
public static class LastChangedValidator
{
    public static async Task ValidateAsync(CookieValidatePrincipalContext context)
    {
        // Pull database from registered DI services.
        var userRepository = context.HttpContext.RequestServices.GetRequiredService<IUserRepository>();
        var userPrincipal = context.Principal;

        // Look for the last changed claim.
        string lastChanged;
        lastChanged = (from c in userPrincipal.Claims
                        where c.Type == "LastUpdated"
                        select c.Value).FirstOrDefault();

        if (string.IsNullOrEmpty(lastChanged) ||
            !userRepository.ValidateLastChanged(userPrincipal, lastChanged))
        {
            context.RejectPrincipal();
            await context.HttpContext.SignOutAsync("MyCookieAuthenticationScheme");
        }
    }
}
```

<span data-ttu-id="48955-158">Dies würde dann oben verknüpft werden, während der Cookie-Service-Registrierung in der `ConfigureServices` Methode *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="48955-158">This would then be wired up during cookie service registration in the `ConfigureServices` method of *Startup.cs*:</span></span>

```csharp
services.AddAuthentication("MyCookieAuthenticationScheme")
        .AddCookie("MyCookieAuthenticationScheme", options =>
        {
            options.Events = new CookieAuthenticationEvents
            {
                OnValidatePrincipal = LastChangedValidator.ValidateAsync
            };
        });
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="48955-159">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="48955-159">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
public static class LastChangedValidator
{
    public static async Task ValidateAsync(CookieValidatePrincipalContext context)
    {
        // Pull database from registered DI services.
        var userRepository = context.HttpContext.RequestServices.GetRequiredService<IUserRepository>();
        var userPrincipal = context.Principal;

        // Look for the last changed claim.
        string lastChanged;
        lastChanged = (from c in userPrincipal.Claims
                        where c.Type == "LastUpdated"
                        select c.Value).FirstOrDefault();

        if (string.IsNullOrEmpty(lastChanged) ||
            !userRepository.ValidateLastChanged(userPrincipal, lastChanged))
        {
            context.RejectPrincipal();
            await context.HttpContext.Authentication.SignOutAsync("MyCookieAuthenticationScheme");
        }
    }
}
```

<span data-ttu-id="48955-160">Dies würde dann oben verknüpft werden, während der Konfiguration der Cookie-Authentifizierung in der `Configure` Methode *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="48955-160">This would then be wired up during cookie authentication configuration in the `Configure` method of *Startup.cs*:</span></span>

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

<span data-ttu-id="48955-161">Betrachten Sie das Beispiel in der Buchstabenfolge aktualisiert wurde &mdash; eine Entscheidung, die Sicherheit in keiner Weise beeinflussen nicht.</span><span class="sxs-lookup"><span data-stu-id="48955-161">Consider the example in which their name has been updated &mdash; a decision which doesn't affect security in any way.</span></span> <span data-ttu-id="48955-162">Wenn Sie den Benutzerprinzipalnamen zerstörungsfrei aktualisieren möchten, können Sie aufrufen `context.ReplacePrincipal()` und legen Sie die `context.ShouldRenew` Eigenschaft `true`.</span><span class="sxs-lookup"><span data-stu-id="48955-162">If you want to non-destructively update the user principal, you can call `context.ReplacePrincipal()` and set the `context.ShouldRenew` property to `true`.</span></span>

<a name="security-authentication-cookie-options"></a>

## <a name="controlling-cookie-options"></a><span data-ttu-id="48955-163">Steuern von Cookieoptionen</span><span class="sxs-lookup"><span data-stu-id="48955-163">Controlling cookie options</span></span>

<span data-ttu-id="48955-164">Die [CookieAuthenticationOptions](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.cookieauthenticationoptions) -Klasse stammt mit diversen Konfigurationsoptionen zu optimieren, die Cookies, die erstellt wird.</span><span class="sxs-lookup"><span data-stu-id="48955-164">The [CookieAuthenticationOptions](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.cookieauthenticationoptions) class comes with various configuration options to fine-tune the cookies being created.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="48955-165">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="48955-165">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="48955-166">ASP.NET Core 2.x vereinigt die APIs, die zum Konfigurieren von Cookies verwendet.</span><span class="sxs-lookup"><span data-stu-id="48955-166">ASP.NET Core 2.x unifies the APIs used for configuring cookies.</span></span> <span data-ttu-id="48955-167">Die 1.x APIs wurden als veraltet markiert und eine neue `Cookie` Eigenschaft vom Typ `CookieBuilder` wurde eingeführt, der `CookieAuthenticationOptions` Klasse.</span><span class="sxs-lookup"><span data-stu-id="48955-167">The 1.x APIs have been marked as obsolete, and a new `Cookie` property of type `CookieBuilder` has been introduced in the `CookieAuthenticationOptions` class.</span></span> <span data-ttu-id="48955-168">Es wird empfohlen, dass die Migration zu 2.x-APIs.</span><span class="sxs-lookup"><span data-stu-id="48955-168">It's recommended that you migrate to the 2.x APIs.</span></span>

* <span data-ttu-id="48955-169">`ClaimsIssuer`ist der Aussteller zu verwendende für die [Aussteller](https://docs.microsoft.com/dotnet/api/system.security.claims.claim.issuer) Eigenschaft für Ansprüche, die von der Cookieauthentifizierung erstellt.</span><span class="sxs-lookup"><span data-stu-id="48955-169">`ClaimsIssuer` is the issuer to be used for the [Issuer](https://docs.microsoft.com/dotnet/api/system.security.claims.claim.issuer) property on any claims created by the cookie authentication.</span></span>

* <span data-ttu-id="48955-170">`CookieBuilder.Domain`ist der Domänenname, der das Cookie bedient wird.</span><span class="sxs-lookup"><span data-stu-id="48955-170">`CookieBuilder.Domain` is the domain name to which the cookie is served.</span></span> <span data-ttu-id="48955-171">Standardmäßig ist dies der Hostname, der die Anforderung gesendet wurde.</span><span class="sxs-lookup"><span data-stu-id="48955-171">By default, this is the host name the request was sent to.</span></span> <span data-ttu-id="48955-172">Der Browser dient nur das Cookie auf einen übereinstimmenden Hostnamen.</span><span class="sxs-lookup"><span data-stu-id="48955-172">The browser only serves the cookie to a matching host name.</span></span> <span data-ttu-id="48955-173">Sie möchten möglicherweise passen Sie diese Option, um Cookies zu einem Host in Ihrer Domäne verfügen.</span><span class="sxs-lookup"><span data-stu-id="48955-173">You may wish to adjust this to have cookies available to any host in your domain.</span></span> <span data-ttu-id="48955-174">Z. B. Festlegen der Domäne des Cookies auf `.contoso.com` zur Verfügung `contoso.com`, `www.contoso.com`, `staging.www.contoso.com`usw.</span><span class="sxs-lookup"><span data-stu-id="48955-174">For example, setting the cookie domain to `.contoso.com` makes it available to `contoso.com`, `www.contoso.com`, `staging.www.contoso.com`, etc.</span></span>

* <span data-ttu-id="48955-175">`CookieBuilder.HttpOnly`ein Flag, der angibt, wenn das Cookie nur an Server zugegriffen werden soll.</span><span class="sxs-lookup"><span data-stu-id="48955-175">`CookieBuilder.HttpOnly` is a flag indicating if the cookie should be accessible only to servers.</span></span> <span data-ttu-id="48955-176">Der Standardwert lautet `true`.</span><span class="sxs-lookup"><span data-stu-id="48955-176">This defaults to `true`.</span></span> <span data-ttu-id="48955-177">Durch Ändern dieses Werts möglicherweise anwendungskennworts Cookie Diebstahl öffnen Sie Ihre Anwendung einen Fehler siteübergreifende verfügen sollen.</span><span class="sxs-lookup"><span data-stu-id="48955-177">Changing this value may open your application to cookie theft should your application have a Cross-Site Scripting bug.</span></span>

* <span data-ttu-id="48955-178">`CookieBuilder.Path`kann verwendet werden, zum Isolieren von Anwendungen, die auf dem gleichen Hostnamen ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="48955-178">`CookieBuilder.Path` can be used to isolate applications running on the same host name.</span></span> <span data-ttu-id="48955-179">Wenn Sie eine app auf `/app1` Einschränken des Cookies ausgegeben, um nur an die Anwendung gesendet werden soll, und Sie sollten festlegen, die `CookiePath` Eigenschaft `/app1`.</span><span class="sxs-lookup"><span data-stu-id="48955-179">If you have an app running in `/app1` and want to limit the cookies issued to just be sent to that application, then you should set the `CookiePath` property to `/app1`.</span></span> <span data-ttu-id="48955-180">Auf diese Weise das Cookie ist nur verfügbar, um Anforderungen an `/app1` oder einem beliebigen Element darunter.</span><span class="sxs-lookup"><span data-stu-id="48955-180">By doing so, the cookie is only available to requests to `/app1` or anything underneath it.</span></span>

* <span data-ttu-id="48955-181">`CookieBuilder.SameSite`Gibt an, ob der Browser das Cookie an den gleichen Standort oder Cross-Site-Anforderungen angeschlossen werden darf.</span><span class="sxs-lookup"><span data-stu-id="48955-181">`CookieBuilder.SameSite` indicates whether the browser should allow the cookie to be attached to same-site or cross-site requests.</span></span> <span data-ttu-id="48955-182">Der Standardwert lautet `SameSiteMode.Lax`.</span><span class="sxs-lookup"><span data-stu-id="48955-182">This defaults to `SameSiteMode.Lax`.</span></span>

* <span data-ttu-id="48955-183">`CookieBuilder.SecurePolicy`ein Flag, der angibt, wenn das Cookie auf HTTPS, HTTP oder HTTPS oder das gleiche Protokoll wie die Anforderung beschränkt werden soll.</span><span class="sxs-lookup"><span data-stu-id="48955-183">`CookieBuilder.SecurePolicy` is a flag indicating if the cookie created should be limited to HTTPS, HTTP or HTTPS, or the same protocol as the request.</span></span> <span data-ttu-id="48955-184">Der Standardwert lautet `SameAsRequest`.</span><span class="sxs-lookup"><span data-stu-id="48955-184">This defaults to `SameAsRequest`.</span></span>

* <span data-ttu-id="48955-185">`ExpireTimeSpan`ist die `TimeSpan` nach dem das Cookie abläuft.</span><span class="sxs-lookup"><span data-stu-id="48955-185">`ExpireTimeSpan` is the `TimeSpan` after which the cookie expires.</span></span> <span data-ttu-id="48955-186">Es wird das aktuelle Datum und die Uhrzeit, erstellen Sie das Ablaufdatum für das Cookie hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="48955-186">It's added to the current date and time to create the expiry date for the cookie.</span></span>

* <span data-ttu-id="48955-187">`SlidingExpiration`ein Flag, der angibt, ob das Cookie Ablaufdatum, wenn mehr als die Hälfte der zurückgesetzt der `ExpireTimeSpan` Intervall ist verstrichen.</span><span class="sxs-lookup"><span data-stu-id="48955-187">`SlidingExpiration` is a flag indicating whether the cookie expiration date resets when more than half of the `ExpireTimeSpan` interval has passed.</span></span> <span data-ttu-id="48955-188">Das neue Ablaufdatum vorwärts verschoben ist, werden das aktuelle Datum plus dem `ExpireTimespan`.</span><span class="sxs-lookup"><span data-stu-id="48955-188">The new expiry date is moved forward to be the current date plus the `ExpireTimespan`.</span></span> <span data-ttu-id="48955-189">Ein [absolute Ablaufzeit](xref:security/authentication/cookie#security-authentication-absolute-expiry) kann festgelegt werden, mithilfe der `AuthenticationProperties` -Klasse beim Aufrufen von `SignInAsync`.</span><span class="sxs-lookup"><span data-stu-id="48955-189">An [absolute expiry time](xref:security/authentication/cookie#security-authentication-absolute-expiry) can be set by using the `AuthenticationProperties` class when calling `SignInAsync`.</span></span> <span data-ttu-id="48955-190">Ein absoluter Ablauf, kann die Sicherheit Ihrer Anwendung verbessern, indem Sie beschränken die Zeitspanne, für die das Authentifizierungscookie gültig ist.</span><span class="sxs-lookup"><span data-stu-id="48955-190">An absolute expiry can improve the security of your application by limiting the amount of time for which the authentication cookie is valid.</span></span>

<span data-ttu-id="48955-191">Ein Beispiel der Verwendung von `CookieAuthenticationOptions` in der `ConfigureServices` Methode *Startup.cs* folgt:</span><span class="sxs-lookup"><span data-stu-id="48955-191">An example of using `CookieAuthenticationOptions` in the `ConfigureServices` method of *Startup.cs* follows:</span></span>

```csharp
services.AddAuthentication("MyCookieAuthenticationScheme")
        .AddCookie("MyCookieAuthenticationScheme", options =>
        {
            options.Cookie.Name = "AuthCookie";
            options.Cookie.Domain = "contoso.com";
            options.Cookie.Path = "/";
            options.Cookie.HttpOnly = true;
            options.Cookie.SameSite = SameSiteMode.Lax;
            options.Cookie.SecurePolicy = CookieSecurePolicy.Always;
        });
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="48955-192">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="48955-192">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

* <span data-ttu-id="48955-193">`ClaimsIssuer`ist der Aussteller zu verwendende für die [Aussteller](https://docs.microsoft.com/dotnet/api/system.security.claims.claim.issuer) Eigenschaft für Ansprüche, die von der Middleware erstellt.</span><span class="sxs-lookup"><span data-stu-id="48955-193">`ClaimsIssuer` is the issuer to be used for the [Issuer](https://docs.microsoft.com/dotnet/api/system.security.claims.claim.issuer) property on any claims created by the middleware.</span></span>

* <span data-ttu-id="48955-194">`CookieDomain`ist der Domänenname, der das Cookie bedient wird.</span><span class="sxs-lookup"><span data-stu-id="48955-194">`CookieDomain` is the domain name to which the cookie is served.</span></span> <span data-ttu-id="48955-195">Standardmäßig ist dies der Hostname, der die Anforderung gesendet wurde.</span><span class="sxs-lookup"><span data-stu-id="48955-195">By default, this is the host name the request was sent to.</span></span> <span data-ttu-id="48955-196">Der Browser dient nur das Cookie auf einen übereinstimmenden Hostnamen.</span><span class="sxs-lookup"><span data-stu-id="48955-196">The browser only serves the cookie to a matching host name.</span></span> <span data-ttu-id="48955-197">Sie möchten möglicherweise passen Sie diese Option, um Cookies zu einem Host in Ihrer Domäne verfügen.</span><span class="sxs-lookup"><span data-stu-id="48955-197">You may wish to adjust this to have cookies available to any host in your domain.</span></span> <span data-ttu-id="48955-198">Z. B. Festlegen der Domäne des Cookies auf `.contoso.com` zur Verfügung `contoso.com`, `www.contoso.com`, `staging.www.contoso.com`usw.</span><span class="sxs-lookup"><span data-stu-id="48955-198">For example, setting the cookie domain to `.contoso.com` makes it available to `contoso.com`, `www.contoso.com`, `staging.www.contoso.com`, etc.</span></span>

* <span data-ttu-id="48955-199">`CookieHttpOnly`ein Flag, der angibt, wenn das Cookie nur an Server zugegriffen werden soll.</span><span class="sxs-lookup"><span data-stu-id="48955-199">`CookieHttpOnly` is a flag indicating if the cookie should be accessible only to servers.</span></span> <span data-ttu-id="48955-200">Der Standardwert lautet `true`.</span><span class="sxs-lookup"><span data-stu-id="48955-200">This defaults to `true`.</span></span> <span data-ttu-id="48955-201">Durch Ändern dieses Werts möglicherweise anwendungskennworts Cookie Diebstahl öffnen Sie Ihre Anwendung einen Fehler siteübergreifende verfügen sollen.</span><span class="sxs-lookup"><span data-stu-id="48955-201">Changing this value may open your application to cookie theft should your application have a Cross-Site Scripting bug.</span></span>

* <span data-ttu-id="48955-202">`CookiePath`kann verwendet werden, zum Isolieren von Anwendungen, die auf dem gleichen Hostnamen ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="48955-202">`CookiePath` can be used to isolate applications running on the same host name.</span></span> <span data-ttu-id="48955-203">Wenn Sie eine app auf `/app1` Einschränken des Cookies ausgegeben, um nur an die Anwendung gesendet werden soll, und Sie sollten festlegen, die `CookiePath` Eigenschaft `/app1`.</span><span class="sxs-lookup"><span data-stu-id="48955-203">If you have an app running in `/app1` and want to limit the cookies issued to just be sent to that application, then you should set the `CookiePath` property to `/app1`.</span></span> <span data-ttu-id="48955-204">Auf diese Weise das Cookie ist nur verfügbar, um Anforderungen an `/app1` oder einem beliebigen Element darunter.</span><span class="sxs-lookup"><span data-stu-id="48955-204">By doing so, the cookie is only available to requests to `/app1` or anything underneath it.</span></span>

* <span data-ttu-id="48955-205">`CookieSecure`ein Flag, der angibt, wenn das Cookie auf HTTPS, HTTP oder HTTPS oder das gleiche Protokoll wie die Anforderung beschränkt werden soll.</span><span class="sxs-lookup"><span data-stu-id="48955-205">`CookieSecure` is a flag indicating if the cookie created should be limited to HTTPS, HTTP or HTTPS, or the same protocol as the request.</span></span> <span data-ttu-id="48955-206">Der Standardwert lautet `SameAsRequest`.</span><span class="sxs-lookup"><span data-stu-id="48955-206">This defaults to `SameAsRequest`.</span></span>

* <span data-ttu-id="48955-207">`ExpireTimeSpan`ist die `TimeSpan` nach dem das Cookie abläuft.</span><span class="sxs-lookup"><span data-stu-id="48955-207">`ExpireTimeSpan` is the `TimeSpan` after which the cookie expires.</span></span> <span data-ttu-id="48955-208">Es wird das aktuelle Datum und die Uhrzeit, erstellen Sie das Ablaufdatum für das Cookie hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="48955-208">It's added to the current date and time to create the expiry date for the cookie.</span></span>

* <span data-ttu-id="48955-209">`SlidingExpiration`ein Flag, der angibt, ob das Cookie Ablaufdatum, wenn mehr als die Hälfte der zurückgesetzt der `ExpireTimeSpan` Intervall ist verstrichen.</span><span class="sxs-lookup"><span data-stu-id="48955-209">`SlidingExpiration` is a flag indicating whether the cookie expiration date resets when more than half of the `ExpireTimeSpan` interval has passed.</span></span> <span data-ttu-id="48955-210">Das neue Ablaufdatum vorwärts verschoben ist, werden das aktuelle Datum plus dem `ExpireTimespan`.</span><span class="sxs-lookup"><span data-stu-id="48955-210">The new expiry date is moved forward to be the current date plus the `ExpireTimespan`.</span></span> <span data-ttu-id="48955-211">Ein [absolute Ablaufzeit](xref:security/authentication/cookie#security-authentication-absolute-expiry) kann festgelegt werden, mithilfe der `AuthenticationProperties` -Klasse beim Aufrufen von `SignInAsync`.</span><span class="sxs-lookup"><span data-stu-id="48955-211">An [absolute expiry time](xref:security/authentication/cookie#security-authentication-absolute-expiry) can be set by using the `AuthenticationProperties` class when calling `SignInAsync`.</span></span> <span data-ttu-id="48955-212">Ein absoluter Ablauf, kann die Sicherheit Ihrer Anwendung verbessern, indem Sie beschränken die Zeitspanne, für die das Authentifizierungscookie gültig ist.</span><span class="sxs-lookup"><span data-stu-id="48955-212">An absolute expiry can improve the security of your application by limiting the amount of time for which the authentication cookie is valid.</span></span>

<span data-ttu-id="48955-213">Ein Beispiel der Verwendung von `CookieAuthenticationOptions` in der `Configure` Methode *Startup.cs* folgt:</span><span class="sxs-lookup"><span data-stu-id="48955-213">An example of using `CookieAuthenticationOptions` in the `Configure` method of *Startup.cs* follows:</span></span>

```csharp
app.UseCookieAuthentication(new CookieAuthenticationOptions
{
    CookieName = "AuthCookie",
    CookieDomain = "contoso.com",
    CookiePath = "/",
    CookieHttpOnly = true,
    CookieSecure = CookieSecurePolicy.Always
});
```

---

## <a name="persistent-cookies-and-absolute-expiry-times"></a><span data-ttu-id="48955-214">Permanente Cookies und wie oft die absolute Ablaufzeit</span><span class="sxs-lookup"><span data-stu-id="48955-214">Persistent cookies and absolute expiry times</span></span>

<span data-ttu-id="48955-215">Sie können den Ablauf Cookie über Browsersitzungen hinweg beibehalten werden und ein absoluter Ablauf die Identität und Transport sie Cookies werden soll.</span><span class="sxs-lookup"><span data-stu-id="48955-215">You may want the cookie expiry to persist across browser sessions and want an absolute expiry to the identity and the cookie transporting it.</span></span> <span data-ttu-id="48955-216">Dieser Persistenz sollte nur mit expliziten benutzerzustimmung, über ein Kontrollkästchen "Anmeldedaten" auf den Anmeldenamen oder einen ähnlichen Mechanismus aktiviert werden.</span><span class="sxs-lookup"><span data-stu-id="48955-216">This persistence should only be enabled with explicit user consent, via a "Remember Me" checkbox on login or a similar mechanism.</span></span> <span data-ttu-id="48955-217">Führen Sie diese Schritte, mit der `AuthenticationProperties` Parameter auf die `SignInAsync` Methode wird aufgerufen, wenn [einer Identität anmelden, und erstellen das Cookie](xref:security/authentication/cookie#security-authentication-cookie-middleware-creating-a-cookie).</span><span class="sxs-lookup"><span data-stu-id="48955-217">You can do these things by using the `AuthenticationProperties` parameter on the `SignInAsync` method called when [signing in an identity and creating the cookie](xref:security/authentication/cookie#security-authentication-cookie-middleware-creating-a-cookie).</span></span> <span data-ttu-id="48955-218">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="48955-218">For example:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="48955-219">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="48955-219">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
await HttpContext.SignInAsync(
    "MyCookieAuthenticationScheme",
    principal,
    new AuthenticationProperties
    {
        IsPersistent = true
    });
```

<span data-ttu-id="48955-220">Die `AuthenticationProperties` Klasse, die im vorherigen Codeausschnitt verwendet befindet sich in der `Microsoft.AspNetCore.Authentication` Namespace.</span><span class="sxs-lookup"><span data-stu-id="48955-220">The `AuthenticationProperties` class, used in the preceding code snippet, resides in the `Microsoft.AspNetCore.Authentication` namespace.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="48955-221">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="48955-221">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
await HttpContext.Authentication.SignInAsync(
    "MyCookieAuthenticationScheme",
    principal,
    new AuthenticationProperties
    {
        IsPersistent = true
    });
```

<span data-ttu-id="48955-222">Die `AuthenticationProperties` Klasse, die im vorherigen Codeausschnitt verwendet befindet sich in der `Microsoft.AspNetCore.Http.Authentication` Namespace.</span><span class="sxs-lookup"><span data-stu-id="48955-222">The `AuthenticationProperties` class, used in the preceding code snippet, resides in the `Microsoft.AspNetCore.Http.Authentication` namespace.</span></span>

---

<span data-ttu-id="48955-223">Der vorherigen Codeausschnitt erstellt eine Identität und die entsprechenden Cookies, die über Browser Abschlüsse überdauert.</span><span class="sxs-lookup"><span data-stu-id="48955-223">The preceding code snippet creates an identity and corresponding cookie which survives through browser closures.</span></span> <span data-ttu-id="48955-224">Alle gleitenden ablaufeinstellungen, die zuvor über konfiguriert [Cookieoptionen](xref:security/authentication/cookie#security-authentication-cookie-options) werden weiterhin berücksichtigt.</span><span class="sxs-lookup"><span data-stu-id="48955-224">Any sliding expiration settings previously configured via [cookie options](xref:security/authentication/cookie#security-authentication-cookie-options) are still honored.</span></span> <span data-ttu-id="48955-225">Wenn das Cookie abläuft, dabei wird der Browser geschlossen wird, löscht der Browser nach dem Neustart wird.</span><span class="sxs-lookup"><span data-stu-id="48955-225">If the cookie expires whilst the browser is closed, the browser clears it once it is restarted.</span></span>

<a name="security-authentication-absolute-expiry"></a>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="48955-226">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="48955-226">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
await HttpContext.SignInAsync(
    "MyCookieAuthenticationScheme",
    principal,
    new AuthenticationProperties
    {
        ExpiresUtc = DateTime.UtcNow.AddMinutes(20)
    });
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="48955-227">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="48955-227">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
await HttpContext.Authentication.SignInAsync(
    "MyCookieAuthenticationScheme",
    principal,
    new AuthenticationProperties
    {
        ExpiresUtc = DateTime.UtcNow.AddMinutes(20)
    });
```

---

<span data-ttu-id="48955-228">Der vorherigen Codeausschnitt erstellt eine Identität und die entsprechenden Cookie, das 20 Minuten dauert.</span><span class="sxs-lookup"><span data-stu-id="48955-228">The preceding code snippet creates an identity and corresponding cookie which lasts for 20 minutes.</span></span> <span data-ttu-id="48955-229">Dadurch werden alle gleitenden ablaufeinstellungen, die zuvor über konfiguriert ignoriert [Cookieoptionen](xref:security/authentication/cookie#security-authentication-cookie-options).</span><span class="sxs-lookup"><span data-stu-id="48955-229">This ignores any sliding expiration settings previously configured via [cookie options](xref:security/authentication/cookie#security-authentication-cookie-options).</span></span>

<span data-ttu-id="48955-230">Die `ExpiresUtc` und `IsPersistent` Eigenschaften schließen sich gegenseitig.</span><span class="sxs-lookup"><span data-stu-id="48955-230">The `ExpiresUtc` and `IsPersistent` properties are mutually exclusive.</span></span>
