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
ms.openlocfilehash: 60ac318cb47b5a5b4c651c88e60d43772ce59958
ms.sourcegitcommit: bd05f7ea8f87ad076ef6e8b704698ebcba5ca80c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/25/2017
---
# <a name="using-cookie-authentication-without-aspnet-core-identity"></a>Mithilfe der Cookieauthentifizierung ohne ASP.NET Core Identität

<a name="security-authentication-cookie-middleware"></a>

ASP.NET Core 1.x bietet Cookie [Middleware](../../fundamentals/middleware.md#fundamentals-middleware) der serialisiert eines Benutzerprinzipals in einem verschlüsselten Cookie, und klicken Sie dann bei nachfolgenden Anforderungen überprüft das Cookie wird den Prinzipal erstellt, und weist sie der `HttpContext.User` Eigenschaft . Wenn Sie eine eigene Anmeldung Bildschirme und die Benutzerdatenbanken bereitstellen möchten, können Sie die Cookie-Middleware als eigenständiges Feature.

Eine wesentliche Änderung in ASP.NET Core 2.x ist, dass die Cookie-Middleware nicht vorhanden ist. Stattdessen die `UseAuthentication` Methodenaufruf in der `Configure` Methode *Startup.cs* fügt die AuthenticationMiddleware festgelegt die `HttpContext.User` Eigenschaft.

<a name="security-authentication-cookie-middleware-configuring"></a>

## <a name="adding-and-configuring"></a>Hinzufügen und konfigurieren

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Führen Sie die folgenden Schritte aus:

- Wenn Sie nicht mit der `Microsoft.AspNetCore.All` [Metapackage](xref:fundamentals/metapackage), installieren Sie Version 2.0 + von der `Microsoft.AspNetCore.Authentication.Cookies` NuGet-Paket im Projekt.

- Aufrufen der `UseAuthentication` Methode in der `Configure` Methode der *Startup.cs* Datei:

    ```csharp
    app.UseAuthentication();
    ```

- Aufrufen der `AddAuthentication` und `AddCookie` Methoden in der `ConfigureServices` Methode der *Startup.cs* Datei:

    ```csharp
    services.AddAuthentication("MyCookieAuthenticationScheme")
            .AddCookie(options => {
                options.AccessDeniedPath = "/Account/Forbidden/";
                options.LoginPath = "/Account/Unauthorized/";
            });
    ```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Führen Sie die folgenden Schritte aus:

- Installieren der `Microsoft.AspNetCore.Authentication.Cookies` NuGet-Paket im Projekt. Dieses Paket enthält die Cookie-Middleware.

- Fügen Sie die folgenden Zeilen, die der `Configure` Methode in Ihrer *Startup.cs* Datei vor dem der `app.UseMvc()` Anweisung:

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

Die oben aufgeführten Codeausschnitte konfigurieren Sie einige oder alle der folgenden Optionen:

* `AccessDeniedPath`-Dies ist der relative Pfad auf die Anforderungen weitergeleitet wird, wenn ein Benutzer versucht, auf eine Ressource zuzugreifen, aber keine besteht [Autorisierungsrichtlinien](xref:security/authorization/policies#security-authorization-policies-based) für diese Ressource.

* `AuthenticationScheme`-Dies ist ein Wert, der unter dem ein bestimmtes Cookie-Authentifizierungsschema bekannt ist. Dies ist nützlich, wenn mehrere Instanzen der Cookieauthentifizierung vorhanden sind und Sie möchten [Beschränken der Autorisierung mit einer Instanz](xref:security/authorization/limitingidentitybyscheme#security-authorization-limiting-by-scheme).

* `AutomaticAuthenticate`-Dieses Flag ist nur für ASP.NET Core relevant 1.x. Er gibt an, dass die Cookieauthentifizierung für jede Anforderung ausgeführt werden soll, und versuchen, zu überprüfen und rekonstruieren serialisierten Prinzipal, der Sie erstellt.

* `AutomaticChallenge`-Dieses Flag ist nur für ASP.NET Core relevant 1.x. Angenommen, dass die 1.x Cookieauthentifizierung den Browser zum umleiten soll die `LoginPath` oder `AccessDeniedPath` Fehler bei der Autorisierung.

* `LoginPath`-Dies ist der relative Pfad zu dem Anforderungen umgeleitet werden soll, wenn ein Benutzer versucht, eine Ressource zuzugreifen, aber wurde nicht authentifiziert.

[Andere Optionen](xref:security/authentication/cookie#security-authentication-cookie-options) gehören die Möglichkeit, den Aussteller für Ansprüche, Festlegen der Cookieauthentifizierung erstellt hat, der Namen des Cookies die Authentifizierung absinkt, die Domäne für das Cookie und verschiedene Sicherheitseigenschaften für das Cookie. Standardmäßig verwendet der Cookieauthentifizierung entsprechende Sicherheitsoptionen für Cookies erstellt, z. B.:
- Wenn das Flag HttpOnly Cookie Zugriff in clientseitiges JavaScript zu verhindern
- Das Cookie auf HTTPS beschränken, wenn eine Anforderung über HTTPS zurückgelegt hat

<a name="security-authentication-cookie-middleware-creating-a-cookie"></a>

## <a name="creating-an-identity-cookie"></a>Erstellen ein Cookie Identität

Um ein Cookie, halten Ihre Benutzerinformationen zu erstellen, erstellen Sie eine [ClaimsPrincipal](https://docs.microsoft.com/dotnet/api/system.security.claims.claimsprincipal) mit den Informationen in Cookies serialisiert werden soll. Nachdem Sie eine geeignete haben `ClaimsPrincipal` -Objekt, rufen Sie die folgenden in der Controllermethode:

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```csharp
await HttpContext.SignInAsync("MyCookieAuthenticationScheme", principal);
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

```csharp
await HttpContext.Authentication.SignInAsync("MyCookieAuthenticationScheme", principal);
```

---

Dies ein verschlüsseltes Cookies erstellt und der aktuellen Antwort hinzugefügt. Die `AuthenticationScheme` während des angegebenen [Konfiguration](xref:security/authentication/cookie#security-authentication-cookie-middleware-configuring) muss verwendet werden, beim Aufrufen von `SignInAsync`.

Im Hintergrund, wird die Verschlüsselung verwendet ASP.NET Core des [Datenschutz](xref:security/data-protection/using-data-protection#security-data-protection-getting-started) System. Wenn Sie auf mehreren Computern, Load, Lastenausgleich oder mithilfe einer Webfarm gehostet werden, dann Sie müssen [Schutz von Daten konfigurieren](xref:security/data-protection/configuration/overview#data-protection-configuring) und mit dem gleichen Schlüssel Ring Anwendungsbezeichner.

## <a name="signing-out"></a>Abmelden

Um den aktuellen Benutzer abmelden und ihre Cookies zu löschen, rufen Sie die folgenden innerhalb Ihres Controllers:

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```csharp
await HttpContext.SignOutAsync("MyCookieAuthenticationScheme");
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

```csharp
await HttpContext.Authentication.SignOutAsync("MyCookieAuthenticationScheme");
```

---

## <a name="reacting-to-back-end-changes"></a>Das reagieren auf Back-End-Änderungen

>[!WARNING]
> Nachdem ein Cookie Prinzipal erstellt wurde, wird es die einzige Quelle der Identität. Auch wenn Sie einen Benutzer in Ihrem Back-End-Systemen deaktivieren, die Cookieauthentifizierung hat keine Kenntnis davon, und ein Benutzer angemeldet bleibt, solange ihre Cookie gültig ist.

Der Cookieauthentifizierung bietet eine Reihe von Ereignissen in der Optionsklasse. Die `ValidateAsync()` -Ereignis kann zum Abfangen und Überschreiben der Überprüfung der Identität des Cookies verwendet werden.

Erwägen Sie eine Back-End-Benutzerdatenbank, die möglicherweise eine Spalte "LastChanged" aufweisen. Um ein Cookie für ungültig erklärt werden, wenn die Datenbank geändert wird, sollten Sie zuerst, wenn [erstellen das Cookie](xref:security/authentication/cookie#security-authentication-cookie-middleware-creating-a-cookie), fügen Sie eine "LastChanged"-Anspruch mit dem aktuellen Wert hinzu. Wenn die Datenbank geändert wird, sollten der Wert "LastChanged" aktualisiert werden.

Implementieren Sie eine Außerkraftsetzung für die `ValidateAsync()` Ereignis müssen Sie eine Methode mit der folgenden Signatur schreiben:

```csharp
Task ValidateAsync(CookieValidatePrincipalContext context);
```

ASP.NET Core Identity implementiert diese Überprüfung als Teil seiner `SecurityStampValidator`. Ein Beispiel sieht wie folgt aus:

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

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

Dies würde dann oben verknüpft werden, während der Cookie-Service-Registrierung in der `ConfigureServices` Methode *Startup.cs*:

```csharp
services.AddAuthentication("MyCookieAuthenticationScheme")
        .AddCookie(options =>
        {
            options.Events = new CookieAuthenticationEvents
            {
                OnValidatePrincipal = LastChangedValidator.ValidateAsync
            };
        });
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

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

Dies würde dann oben verknüpft werden, während der Konfiguration der Cookie-Authentifizierung in der `Configure` Methode *Startup.cs*:

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

Betrachten Sie das Beispiel in der Buchstabenfolge aktualisiert wurde &mdash; eine Entscheidung, die Sicherheit in keiner Weise beeinflussen nicht. Wenn Sie den Benutzerprinzipalnamen zerstörungsfrei aktualisieren möchten, können Sie aufrufen `context.ReplacePrincipal()` und legen Sie die `context.ShouldRenew` Eigenschaft `true`.

<a name="security-authentication-cookie-options"></a>

## <a name="controlling-cookie-options"></a>Steuern von Cookieoptionen

Die [CookieAuthenticationOptions](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.cookieauthenticationoptions) -Klasse stammt mit diversen Konfigurationsoptionen zu optimieren, die Cookies, die erstellt wird.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

ASP.NET Core 2.x vereinigt die APIs, die zum Konfigurieren von Cookies verwendet. Die 1.x APIs wurden als veraltet markiert und eine neue `Cookie` Eigenschaft vom Typ `CookieBuilder` wurde eingeführt, der `CookieAuthenticationOptions` Klasse. Es wird empfohlen, dass die Migration zu 2.x-APIs.

* `ClaimsIssuer`ist der Aussteller zu verwendende für die [Aussteller](https://docs.microsoft.com/dotnet/api/system.security.claims.claim.issuer) Eigenschaft für Ansprüche, die von der Cookieauthentifizierung erstellt.

* `CookieBuilder.Domain`ist der Domänenname, der das Cookie bedient wird. Standardmäßig ist dies der Hostname, der die Anforderung gesendet wurde. Der Browser dient nur das Cookie auf einen übereinstimmenden Hostnamen. Sie möchten möglicherweise passen Sie diese Option, um Cookies zu einem Host in Ihrer Domäne verfügen. Z. B. Festlegen der Domäne des Cookies auf `.contoso.com` zur Verfügung `contoso.com`, `www.contoso.com`, `staging.www.contoso.com`usw.

* `CookieBuilder.HttpOnly`ein Flag, der angibt, wenn das Cookie nur an Server zugegriffen werden soll. Der Standardwert lautet `true`. Durch Ändern dieses Werts möglicherweise anwendungskennworts Cookie Diebstahl öffnen Sie Ihre Anwendung einen Fehler siteübergreifende verfügen sollen.

* `CookieBuilder.Path`kann verwendet werden, zum Isolieren von Anwendungen, die auf dem gleichen Hostnamen ausgeführt wird. Wenn Sie eine app auf `/app1` Einschränken des Cookies ausgegeben, um nur an die Anwendung gesendet werden soll, und Sie sollten festlegen, die `CookiePath` Eigenschaft `/app1`. Auf diese Weise das Cookie ist nur verfügbar, um Anforderungen an `/app1` oder einem beliebigen Element darunter.

* `CookieBuilder.SameSite`Gibt an, ob der Browser das Cookie an den gleichen Standort oder Cross-Site-Anforderungen angeschlossen werden darf. Der Standardwert lautet `SameSiteMode.Lax`.

* `CookieBuilder.SecurePolicy`ein Flag, der angibt, wenn das Cookie auf HTTPS, HTTP oder HTTPS oder das gleiche Protokoll wie die Anforderung beschränkt werden soll. Der Standardwert lautet `SameAsRequest`.

* `ExpireTimeSpan`ist die `TimeSpan` nach dem das Cookie abläuft. Es wird das aktuelle Datum und die Uhrzeit, erstellen Sie das Ablaufdatum für das Cookie hinzugefügt.

* `SlidingExpiration`ein Flag, der angibt, ob das Cookie Ablaufdatum, wenn mehr als die Hälfte der zurückgesetzt der `ExpireTimeSpan` Intervall ist verstrichen. Das neue Ablaufdatum vorwärts verschoben ist, werden das aktuelle Datum plus dem `ExpireTimespan`. Ein [absolute Ablaufzeit](xref:security/authentication/cookie#security-authentication-absolute-expiry) kann festgelegt werden, mithilfe der `AuthenticationProperties` -Klasse beim Aufrufen von `SignInAsync`. Ein absoluter Ablauf, kann die Sicherheit Ihrer Anwendung verbessern, indem Sie beschränken die Zeitspanne, für die das Authentifizierungscookie gültig ist.

Ein Beispiel der Verwendung von `CookieAuthenticationOptions` in der `ConfigureServices` Methode *Startup.cs* folgt:

```csharp
services.AddAuthentication()
        .AddCookie(options =>
        {
            options.Cookie.Name = "AuthCookie";
            options.Cookie.Domain = "contoso.com";
            options.Cookie.Path = "/";
            options.Cookie.HttpOnly = true;
            options.Cookie.SameSite = SameSiteMode.Lax;
            options.Cookie.SecurePolicy = CookieSecurePolicy.Always;
        });
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

* `ClaimsIssuer`ist der Aussteller zu verwendende für die [Aussteller](https://docs.microsoft.com/dotnet/api/system.security.claims.claim.issuer) Eigenschaft für Ansprüche, die von der Middleware erstellt.

* `CookieDomain`ist der Domänenname, der das Cookie bedient wird. Standardmäßig ist dies der Hostname, der die Anforderung gesendet wurde. Der Browser dient nur das Cookie auf einen übereinstimmenden Hostnamen. Sie möchten möglicherweise passen Sie diese Option, um Cookies zu einem Host in Ihrer Domäne verfügen. Z. B. Festlegen der Domäne des Cookies auf `.contoso.com` zur Verfügung `contoso.com`, `www.contoso.com`, `staging.www.contoso.com`usw.

* `CookieHttpOnly`ein Flag, der angibt, wenn das Cookie nur an Server zugegriffen werden soll. Der Standardwert lautet `true`. Durch Ändern dieses Werts möglicherweise anwendungskennworts Cookie Diebstahl öffnen Sie Ihre Anwendung einen Fehler siteübergreifende verfügen sollen.

* `CookiePath`kann verwendet werden, zum Isolieren von Anwendungen, die auf dem gleichen Hostnamen ausgeführt wird. Wenn Sie eine app auf `/app1` Einschränken des Cookies ausgegeben, um nur an die Anwendung gesendet werden soll, und Sie sollten festlegen, die `CookiePath` Eigenschaft `/app1`. Auf diese Weise das Cookie ist nur verfügbar, um Anforderungen an `/app1` oder einem beliebigen Element darunter.

* `CookieSecure`ein Flag, der angibt, wenn das Cookie auf HTTPS, HTTP oder HTTPS oder das gleiche Protokoll wie die Anforderung beschränkt werden soll. Der Standardwert lautet `SameAsRequest`.

* `ExpireTimeSpan`ist die `TimeSpan` nach dem das Cookie abläuft. Es wird das aktuelle Datum und die Uhrzeit, erstellen Sie das Ablaufdatum für das Cookie hinzugefügt.

* `SlidingExpiration`ein Flag, der angibt, ob das Cookie Ablaufdatum, wenn mehr als die Hälfte der zurückgesetzt der `ExpireTimeSpan` Intervall ist verstrichen. Das neue Ablaufdatum vorwärts verschoben ist, werden das aktuelle Datum plus dem `ExpireTimespan`. Ein [absolute Ablaufzeit](xref:security/authentication/cookie#security-authentication-absolute-expiry) kann festgelegt werden, mithilfe der `AuthenticationProperties` -Klasse beim Aufrufen von `SignInAsync`. Ein absoluter Ablauf, kann die Sicherheit Ihrer Anwendung verbessern, indem Sie beschränken die Zeitspanne, für die das Authentifizierungscookie gültig ist.

Ein Beispiel der Verwendung von `CookieAuthenticationOptions` in der `Configure` Methode *Startup.cs* folgt:

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

## <a name="persistent-cookies-and-absolute-expiry-times"></a>Permanente Cookies und wie oft die absolute Ablaufzeit

Sie können den Ablauf Cookie über Browsersitzungen hinweg beibehalten werden und ein absoluter Ablauf die Identität und Transport sie Cookies werden soll. Dieser Persistenz sollte nur mit expliziten benutzerzustimmung, über ein Kontrollkästchen "Anmeldedaten" auf den Anmeldenamen oder einen ähnlichen Mechanismus aktiviert werden. Führen Sie diese Schritte, mit der `AuthenticationProperties` Parameter auf die `SignInAsync` Methode wird aufgerufen, wenn [einer Identität anmelden, und erstellen das Cookie](xref:security/authentication/cookie#security-authentication-cookie-middleware-creating-a-cookie). Zum Beispiel:

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```csharp
await HttpContext.SignInAsync(
    "MyCookieAuthenticationScheme",
    principal,
    new AuthenticationProperties
    {
        IsPersistent = true
    });
```

Die `AuthenticationProperties` Klasse, die im vorherigen Codeausschnitt verwendet befindet sich in der `Microsoft.AspNetCore.Authentication` Namespace.

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

```csharp
await HttpContext.Authentication.SignInAsync(
    "MyCookieAuthenticationScheme",
    principal,
    new AuthenticationProperties
    {
        IsPersistent = true
    });
```

Die `AuthenticationProperties` Klasse, die im vorherigen Codeausschnitt verwendet befindet sich in der `Microsoft.AspNetCore.Http.Authentication` Namespace.

---

Der vorherigen Codeausschnitt erstellt eine Identität und die entsprechenden Cookies, die über Browser Abschlüsse überdauert. Alle gleitenden ablaufeinstellungen, die zuvor über konfiguriert [Cookieoptionen](xref:security/authentication/cookie#security-authentication-cookie-options) werden weiterhin berücksichtigt. Wenn das Cookie abläuft, dabei wird der Browser geschlossen wird, löscht der Browser nach dem Neustart wird.

<a name="security-authentication-absolute-expiry"></a>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```csharp
await HttpContext.SignInAsync(
    "MyCookieAuthenticationScheme",
    principal,
    new AuthenticationProperties
    {
        ExpiresUtc = DateTime.UtcNow.AddMinutes(20)
    });
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

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

Der vorherigen Codeausschnitt erstellt eine Identität und die entsprechenden Cookie, das 20 Minuten dauert. Dadurch werden alle gleitenden ablaufeinstellungen, die zuvor über konfiguriert ignoriert [Cookieoptionen](xref:security/authentication/cookie#security-authentication-cookie-options).

Die `ExpiresUtc` und `IsPersistent` Eigenschaften schließen sich gegenseitig.