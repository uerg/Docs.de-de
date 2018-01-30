---
title: "Migrieren von Authentifizierung und Identität zu ASP.NET Core 2.0"
author: scottaddie
description: "In diesem Artikel werden die am häufigsten verwendeten Schritte zum Migrieren von ASP.NET Core 1.x-Authentifizierung und Identität zu ASP.NET Core 2.0."
manager: wpickett
ms.author: scaddie
ms.date: 10/26/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: migration/1x-to-2x/identity-2x
ms.openlocfilehash: dd48b2b027d22b570aa182e748ca91738e935f49
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2018
---
# <a name="migrating-authentication-and-identity-to-aspnet-core-20"></a>Migrieren von Authentifizierung und Identität zu ASP.NET Core 2.0

Durch [Scott Addie](https://github.com/scottaddie) und [Hao Kung](https://github.com/HaoK)

ASP.NET Core 2.0 wurde ein neues Modell für die Authentifizierung und [Identität](xref:security/authentication/identity) vereinfacht die Konfiguration mithilfe von Diensten. ASP.NET Core 1.x Anwendungen, die Authentifizierung oder die Identität können aktualisiert werden, um das neue Modell zu verwenden, wie im folgenden erläutert.

<a name="auth-middleware"></a>

## <a name="authentication-middleware-and-services"></a>Authentifizierung-Middleware und Dienste
Im 1.x-Projekte wird die Authentifizierung über Middleware konfiguriert. Eine Middleware-Methode wird für jede Authentifizierungsschema aufgerufen, die Sie unterstützen möchten.

Das folgende 1.x-Beispiel konfiguriert die Facebook-Authentifizierung mit der Identität in *Startup.cs*:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddIdentity<ApplicationUser, IdentityRole>()
            .AddEntityFrameworkStores<ApplicationDbContext>();
}

public void Configure(IApplicationBuilder app, ILoggerFactory loggerfactory)
{
    app.UseIdentity();
    app.UseFacebookAuthentication(new FacebookOptions { 
        AppId = Configuration["auth:facebook:appid"],
        AppSecret = Configuration["auth:facebook:appsecret"]
    });
} 
```

In Projekten auf 2.0 ist die Authentifizierung über Dienste konfiguriert. Jede Authentifizierungsschema ist registriert, der `ConfigureServices` Methode *Startup.cs*. Die `UseIdentity` Methode durch ersetzt `UseAuthentication`.

Der folgende 2.0 konfiguriert Facebook-Authentifizierung mit der Identität in *Startup.cs*:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddIdentity<ApplicationUser, IdentityRole>()
            .AddEntityFrameworkStores<ApplicationDbContext>();

    // If you want to tweak Identity cookies, they're no longer part of IdentityOptions.
    services.ConfigureApplicationCookie(options => options.LoginPath = "/Account/LogIn");
    services.AddAuthentication()
            .AddFacebook(options => 
            {
                options.AppId = Configuration["auth:facebook:appid"];
                options.AppSecret = Configuration["auth:facebook:appsecret"];
            });
}

public void Configure(IApplicationBuilder app, ILoggerFactory loggerfactory) {
    app.UseAuthentication();
}
```

Die `UseAuthentication` Methode fügt eine einzelnen Komponente, die für die automatische Authentifizierung und die Behandlung von Anforderungen Remoteauthentifizierung zuständig ist. Er ersetzt alle einzelnen Middleware-Komponenten mit einer einzelnen, gemeinsamen middlewarekomponente.

Im folgenden sind 2.0 Anweisungen zur Migration für jede größere Authentifizierungsschema.

### <a name="cookie-based-authentication"></a>Cookie-basierte Authentifizierung
Wählen Sie eine der folgenden beiden Optionen aus, und stellen Sie die erforderlichen Änderungen im *Startup.cs*:

1. Cookies verwenden, mit der Identität
    - Ersetzen Sie `UseIdentity` mit `UseAuthentication` in die `Configure` Methode:

        ```csharp
        app.UseAuthentication();
        ```

    - Aufrufen der `AddIdentity` Methode in der `ConfigureServices` Methode, um die Cookie-Authentifizierungsdienste hinzuzufügen.
    - Rufen Sie optional die `ConfigureApplicationCookie` oder `ConfigureExternalCookie` Methode in der `ConfigureServices` Methode, um die Identität Cookie-Einstellungen zu optimieren.

        ```csharp
        services.AddIdentity<ApplicationUser, IdentityRole>()
                .AddEntityFrameworkStores<ApplicationDbContext>()
                .AddDefaultTokenProviders();
    
        services.ConfigureApplicationCookie(options => options.LoginPath = "/Account/LogIn");
        ```

2. Cookies ohne Identität verwenden
    - Ersetzen Sie die `UseCookieAuthentication` -Methodenaufruf in der `Configure` Methode mit `UseAuthentication`:
  
        ```csharp
        app.UseAuthentication();
        ```
 
    - Aufrufen der `AddAuthentication` und `AddCookie` Methoden in der `ConfigureServices` Methode:

        ```csharp
        // If you don't want the cookie to be automatically authenticated and assigned to HttpContext.User, 
        // remove the CookieAuthenticationDefaults.AuthenticationScheme parameter passed to AddAuthentication.
        services.AddAuthentication(CookieAuthenticationDefaults.AuthenticationScheme)
                .AddCookie(options => 
                {
                    options.LoginPath = "/Account/LogIn";
                    options.LogoutPath = "/Account/LogOff";
                });
        ```

### <a name="jwt-bearer-authentication"></a>JWT-Trägertoken-Authentifizierung
Nehmen Sie die folgenden Änderungen in *Startup.cs*:
- Ersetzen Sie die `UseJwtBearerAuthentication` -Methodenaufruf in der `Configure` Methode mit `UseAuthentication`:
 
    ```csharp
    app.UseAuthentication();
    ```

- Aufrufen der `AddJwtBearer` Methode in der `ConfigureServices` Methode:

    ```csharp
    services.AddAuthentication(JwtBearerDefaults.AuthenticationScheme)
            .AddJwtBearer(options => 
            {
                options.Audience = "http://localhost:5001/";
                options.Authority = "http://localhost:5000/";
            });
    ```

    Dieser Codeausschnitt verwendet keine Identität, daher sollte das Standardschema durch Übergabe festgelegt werden `JwtBearerDefaults.AuthenticationScheme` auf die `AddAuthentication` Methode.

### <a name="openid-connect-oidc-authentication"></a>OpenID-Verbindung (OIDC)-Authentifizierung
Nehmen Sie die folgenden Änderungen in *Startup.cs*:

- Ersetzen Sie die `UseOpenIdConnectAuthentication` -Methodenaufruf in der `Configure` Methode mit `UseAuthentication`:

    ```csharp
    app.UseAuthentication();
    ```

- Aufrufen der `AddOpenIdConnect` Methode in der `ConfigureServices` Methode:

    ```csharp
    services.AddAuthentication(options => 
    {
        options.DefaultScheme = CookieAuthenticationDefaults.AuthenticationScheme;
        options.DefaultChallengeScheme = OpenIdConnectDefaults.AuthenticationScheme;
    })
    .AddCookie()
    .AddOpenIdConnect(options => 
    {
        options.Authority = Configuration["auth:oidc:authority"];
        options.ClientId = Configuration["auth:oidc:clientid"];
    });
    ```

### <a name="facebook-authentication"></a>Facebook-Authentifizierung
Nehmen Sie die folgenden Änderungen in *Startup.cs*:
- Ersetzen Sie die `UseFacebookAuthentication` -Methodenaufruf in der `Configure` Methode mit `UseAuthentication`:
 
    ```csharp
    app.UseAuthentication();
    ```

- Aufrufen der `AddFacebook` Methode in der `ConfigureServices` Methode:
    
    ```csharp
    services.AddAuthentication()
            .AddFacebook(options => 
            {
                options.AppId = Configuration["auth:facebook:appid"];
                options.AppSecret = Configuration["auth:facebook:appsecret"];
            });
    ```

### <a name="google-authentication"></a>Google-Authentifizierung
Nehmen Sie die folgenden Änderungen in *Startup.cs*:
- Ersetzen Sie die `UseGoogleAuthentication` -Methodenaufruf in der `Configure` Methode mit `UseAuthentication`:
 
    ```csharp
    app.UseAuthentication();
    ```

- Aufrufen der `AddGoogle` Methode in der `ConfigureServices` Methode:

    ```csharp
    services.AddAuthentication()
            .AddGoogle(options => 
            {
                options.ClientId = Configuration["auth:google:clientid"];
                options.ClientSecret = Configuration["auth:google:clientsecret"];
            });    
    ```

### <a name="microsoft-account-authentication"></a>Microsoft-Account-Authentifizierung
Nehmen Sie die folgenden Änderungen in *Startup.cs*:
- Ersetzen Sie die `UseMicrosoftAccountAuthentication` -Methodenaufruf in der `Configure` Methode mit `UseAuthentication`:

    ```csharp
    app.UseAuthentication();
    ```

- Aufrufen der `AddMicrosoftAccount` Methode in der `ConfigureServices` Methode:

    ```csharp
    services.AddAuthentication()
            .AddMicrosoftAccount(options => 
            {
                options.ClientId = Configuration["auth:microsoft:clientid"];
                options.ClientSecret = Configuration["auth:microsoft:clientsecret"];
            });
    ``` 

### <a name="twitter-authentication"></a>Twitter-Authentifizierung
Nehmen Sie die folgenden Änderungen in *Startup.cs*:
- Ersetzen Sie die `UseTwitterAuthentication` -Methodenaufruf in der `Configure` Methode mit `UseAuthentication`:
 
    ```csharp
    app.UseAuthentication();
    ```

- Aufrufen der `AddTwitter` Methode in der `ConfigureServices` Methode:

    ```csharp
    services.AddAuthentication()
            .AddTwitter(options => 
            {
                options.ConsumerKey = Configuration["auth:twitter:consumerkey"];
                options.ConsumerSecret = Configuration["auth:twitter:consumersecret"];
            });
    ```

### <a name="setting-default-authentication-schemes"></a>Standard-Authentifizierungsschemas festlegen
In 1.x die `AutomaticAuthenticate` und `AutomaticChallenge` Eigenschaften der [authenticationoptions](https://docs.microsoft.com/dotnet/api/Microsoft.AspNetCore.Builder.AuthenticationOptions?view=aspnetcore-1.1) Basisklasse wurden auf einem einzelnen Authentifizierungsschema festgelegt werden soll. Es wurde keine gute Möglichkeit, dies zu erzwingen.

In 2.0 wurden diese beiden Eigenschaften als Eigenschaften in den einzelnen entfernt `AuthenticationOptions` Instanz. Sie können konfiguriert werden, der `AddAuthentication` Methodenaufruf innerhalb der `ConfigureServices` Methode *Startup.cs*:

```csharp
services.AddAuthentication(CookieAuthenticationDefaults.AuthenticationScheme);
```

Im vorherigen Codeausschnitt wird das Standardschema zum festgelegt `CookieAuthenticationDefaults.AuthenticationScheme` ("Cookies").

Alternativ können Sie eine überladene Version der verwenden die `AddAuthentication` Methode, um mehr als eine Eigenschaft festgelegt. Im folgenden Beispiel für die überladene Methode wird das Standardschema so eingerichtet `CookieAuthenticationDefaults.AuthenticationScheme`. Das Authentifizierungsschema kann auch angegeben werden, innerhalb der einzelnen `[Authorize]` Attribute oder Autorisierungsrichtlinien.

```csharp
services.AddAuthentication(options => 
{
    options.DefaultScheme = CookieAuthenticationDefaults.AuthenticationScheme;
    options.DefaultChallengeScheme = OpenIdConnectDefaults.AuthenticationScheme;
});
```

Definieren Sie ein Standardschema in 2.0, wenn eine der folgenden Bedingungen erfüllt ist:
- Sie möchten die Benutzer automatisch angemeldet werden
- Sie verwenden die `[Authorize]` Attribut oder Autorisierung Richtlinien ohne Angabe von Schemas

Eine Ausnahme von dieser Regel wird die `AddIdentity` Methode. Diese Methode fügt Cookies für Sie und legt die Standardeinstellung zu authentifizieren und Aufforderung Schemas an, die das Cookie Anwendung `IdentityConstants.ApplicationScheme`. Darüber hinaus wird die Standard-Anmeldeseite-Schema an das externe Cookie `IdentityConstants.ExternalScheme`.

<a name="obsolete-interface"></a>

## <a name="use-httpcontext-authentication-extensions"></a>Verwenden Sie HttpContext authentifizierungserweiterungen
Die `IAuthenticationManager` Schnittstelle ist der Haupteinstiegspunkt, in dem 1.x-Authentifizierungssystem. Es wurde durch einen neuen Satz von ersetzt `HttpContext` Erweiterungsmethoden in den `Microsoft.AspNetCore.Authentication` Namespace.

Z. B. Projekten 1.x Verweis eine `Authentication` Eigenschaft:

[!code-csharp[Main](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Controllers/AccountController.cs?name=snippet_AuthenticationProperty)]

In Projekten auf 2.0 Importieren der `Microsoft.AspNetCore.Authentication` Namespace, und löschen Sie die `Authentication` Eigenschaftenverweise:

[!code-csharp[Main](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Controllers/AccountController.cs?name=snippet_AuthenticationProperty)]

<a name="windows-auth-changes"></a>

## <a name="windows-authentication-httpsys--iisintegration"></a>Windows-Authentifizierung (HTTP / IISIntegration)
Es gibt zwei Variationen der Windows-Authentifizierung:
1. Der Host nur ermöglicht authentifizierten Benutzern.
2. Der Host kann sowohl anonyme und authentifizierte Benutzer

Die erste Variante, die oben beschriebenen ist von den 2.0 Änderungen nicht betroffen.

Die zweite Variante, die oben beschriebenen wird durch die 2.0 Änderungen beeinflusst. Beispielsweise können Sie möglicherweise werden zulassen, dass anonyme Benutzer in Ihre Anwendung auf IIS oder [HTTP.sys](xref:fundamentals/servers/weblistener) jedoch autorisierende von Benutzern auf Controllerebene Ebene. Legen Sie in diesem Szenario wird das Standardschema auf `IISDefaults.AuthenticationScheme` in der `ConfigureServices` Methode *Startup.cs*:

```csharp
services.AddAuthentication(IISDefaults.AuthenticationScheme);
```

Fehler beim Festlegen der Standard-Schema wird entsprechend verhindert, dass der autorisierungsanforderung zu Herausforderung aus arbeiten.

<a name="identity-cookie-options"></a>

## <a name="identitycookieoptions-instances"></a>IdentityCookieOptions-Instanzen
Ein Nebeneffekt der 2.0 Änderungen ist die Option zur Verwendung mit dem Namen Optionen anstelle von Instanzen der Cookie-Optionen. Anpassen von die Identität Cookie Schemanamen wird entfernt.

Beispielsweise 1.x Projekte verwenden [Konstruktoreinfügung](xref:mvc/controllers/dependency-injection#constructor-injection) Übergabe einer `IdentityCookieOptions` Parameter in *AccountController.cs*. Das Authentifizierungsschema externe Cookie wird aus der bereitgestellten Instanz zugegriffen werden:

[!code-csharp[Main](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Controllers/AccountController.cs?name=snippet_AccountControllerConstructor&highlight=4,11)]

Die zuvor erwähnten Konstruktoreinfügung unnötige in Projekten auf 2.0 wird und die `_externalCookieScheme` Feld kann gelöscht werden:

[!code-csharp[Main](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Controllers/AccountController.cs?name=snippet_AccountControllerConstructor)]

Die `IdentityConstants.ExternalScheme` Konstante direkt verwendet werden kann:

[!code-csharp[Main](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Controllers/AccountController.cs?name=snippet_AuthenticationProperty)]

<a name="navigation-properties"></a>

## <a name="add-identityuser-poco-navigation-properties"></a>Hinzufügen von IdentityUser POCO Navigationseigenschaften
Die Entity Framework (EF) Core Navigationseigenschaften des basistexts `IdentityUser` POCO (Plain Old CLR-Objekt) wurden entfernt. Wenn das Projekt 1.x dieser Eigenschaften verwendet, manuell fügen Sie wieder zum Projekt 2.0 hinzu:

```csharp
/// <summary>
/// Navigation property for the roles this user belongs to.
/// </summary>
public virtual ICollection<IdentityUserRole<int>> Roles { get; } = new List<IdentityUserRole<int>>();

/// <summary>
/// Navigation property for the claims this user possesses.
/// </summary>
public virtual ICollection<IdentityUserClaim<int>> Claims { get; } = new List<IdentityUserClaim<int>>();

/// <summary>
/// Navigation property for this users login accounts.
/// </summary>
public virtual ICollection<IdentityUserLogin<int>> Logins { get; } = new List<IdentityUserLogin<int>>();
```

Um doppelte Fremdschlüssel zu verhindern, dass beim Ausführen von EF Core Migrationen, fügen Sie Folgendes verwenden, um Ihre `IdentityDbContext` Klasse `OnModelCreating` Methode (nach der `base.OnModelCreating();` aufrufen):

```csharp
protected override void OnModelCreating(ModelBuilder builder)
{
    base.OnModelCreating(builder);
    // Customize the ASP.NET Identity model and override the defaults if needed.
    // For example, you can rename the ASP.NET Identity table names and more.
    // Add your customizations after calling base.OnModelCreating(builder);

    builder.Entity<ApplicationUser>()
        .HasMany(e => e.Claims)
        .WithOne()
        .HasForeignKey(e => e.UserId)
        .IsRequired()
        .OnDelete(DeleteBehavior.Cascade);

    builder.Entity<ApplicationUser>()
        .HasMany(e => e.Logins)
        .WithOne()
        .HasForeignKey(e => e.UserId)
        .IsRequired()
        .OnDelete(DeleteBehavior.Cascade);

    builder.Entity<ApplicationUser>()
        .HasMany(e => e.Roles)
        .WithOne()
        .HasForeignKey(e => e.UserId)
        .IsRequired()
        .OnDelete(DeleteBehavior.Cascade);
}
```

<a name="synchronous-method-removal"></a>

## <a name="replace-getexternalauthenticationschemes"></a>Ersetzen Sie GetExternalAuthenticationSchemes
Die synchrone Methode `GetExternalAuthenticationSchemes` zugunsten eine asynchrone Version entfernt wurde. 1.x-Projekte haben den folgenden Code in *ManageController.cs*:

[!code-csharp[Main](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Controllers/ManageController.cs?name=snippet_GetExternalAuthenticationSchemes)]

Diese Methode ist in *Login.cshtml* zu:

[!code-cshtml[Main](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Views/Account/Login.cshtml?range=62,75-84)]

In Projekten auf 2.0 verwenden die `GetExternalAuthenticationSchemesAsync` Methode:

[!code-csharp[Main](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Controllers/ManageController.cs?name=snippet_GetExternalAuthenticationSchemesAsync)]

In *Login.cshtml*, `AuthenticationScheme` Eigenschaft zugegriffen wird, der `foreach` Schleife Änderungen an `Name`:

[!code-cshtml[Main](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Views/Account/Login.cshtml?range=62,75-84)]

<a name="property-change"></a>

## <a name="manageloginsviewmodel-property-change"></a>Ändern der ManageLoginsViewModel-Eigenschaft
Ein `ManageLoginsViewModel` Objekt werden in der `ManageLogins` Aktion *ManageController.cs*. In 1.x-Projekten, das Objekt des `OtherLogins` Eigenschaft Rückgabetyp ist `IList<AuthenticationDescription>`. Dieser Rückgabetyp erfordert einen Import eines `Microsoft.AspNetCore.Http.Authentication`:

[!code-csharp[Main](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Models/ManageViewModels/ManageLoginsViewModel.cs?name=snippet_ManageLoginsViewModel&highlight=2,11)]

In Projekten auf 2.0 ändert sich der Rückgabetyp in `IList<AuthenticationScheme>`. Diese neue Rückgabetyp erfordert, und Ersetzen Sie dabei die `Microsoft.AspNetCore.Http.Authentication` importieren Sie mit einer `Microsoft.AspNetCore.Authentication` importieren.

[!code-csharp[Main](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Models/ManageViewModels/ManageLoginsViewModel.cs?name=snippet_ManageLoginsViewModel&highlight=2,11)]

<a name="additional-resources"></a>

## <a name="additional-resources"></a>Zusätzliche Ressourcen
Weitere Details und Erläuterung finden Sie in der [Erörterung Auth 2.0](https://github.com/aspnet/Security/issues/1338) Problem auf GitHub.
