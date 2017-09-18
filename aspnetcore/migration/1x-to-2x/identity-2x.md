---
title: "Migrieren von Authentifizierung und Identität zu ASP.NET Core 2.0"
author: scottaddie
description: "In diesem Artikel werden die am häufigsten verwendeten Schritte zum Migrieren von ASP.NET Core 1.x-Authentifizierung und Identität zu ASP.NET Core 2.0."
keywords: "ASP.NET Core, Identität und Authentifizierung"
ms.author: scaddie
manager: wpickett
ms.date: 08/02/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: migration/1x-to-2x/identity-2x
ms.openlocfilehash: b4e67e7cfea3c01e3ca8c0d5df2a04e789749932
ms.sourcegitcommit: f8f6b5934bd071a349f5bc1e389365c52b1c00fa
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/14/2017
---
# <a name="migrating-authentication-and-identity-to-aspnet-core-20"></a><span data-ttu-id="34eb0-104">Migrieren von Authentifizierung und Identität zu ASP.NET Core 2.0</span><span class="sxs-lookup"><span data-stu-id="34eb0-104">Migrating Authentication and Identity to ASP.NET Core 2.0</span></span>

<span data-ttu-id="34eb0-105">Durch [Scott Addie](https://github.com/scottaddie) und [Hao Kung](https://github.com/HaoK)</span><span class="sxs-lookup"><span data-stu-id="34eb0-105">By [Scott Addie](https://github.com/scottaddie) and [Hao Kung](https://github.com/HaoK)</span></span>

<span data-ttu-id="34eb0-106">ASP.NET Core 2.0 wurde ein neues Modell für die Authentifizierung und [Identität](xref:security/authentication/identity) vereinfacht die Konfiguration mithilfe von Diensten.</span><span class="sxs-lookup"><span data-stu-id="34eb0-106">ASP.NET Core 2.0 has a new model for authentication and [Identity](xref:security/authentication/identity) which simplifies configuration by using services.</span></span> <span data-ttu-id="34eb0-107">ASP.NET Core 1.x Anwendungen, die Authentifizierung oder die Identität können aktualisiert werden, um das neue Modell zu verwenden, wie im folgenden erläutert.</span><span class="sxs-lookup"><span data-stu-id="34eb0-107">ASP.NET Core 1.x applications that use authentication or Identity can be updated to use the new model as outlined below.</span></span>

<a name="auth-middleware"></a>

## <a name="authentication-middleware-and-services"></a><span data-ttu-id="34eb0-108">Authentifizierung-Middleware und Dienste</span><span class="sxs-lookup"><span data-stu-id="34eb0-108">Authentication Middleware and Services</span></span>
<span data-ttu-id="34eb0-109">Im 1.x-Projekte wird die Authentifizierung über Middleware konfiguriert.</span><span class="sxs-lookup"><span data-stu-id="34eb0-109">In 1.x projects, authentication is configured via middleware.</span></span> <span data-ttu-id="34eb0-110">Eine Middleware-Methode wird für jede Authentifizierungsschema aufgerufen, die Sie unterstützen möchten.</span><span class="sxs-lookup"><span data-stu-id="34eb0-110">A middleware method is invoked for each authentication scheme you want to support.</span></span>

<span data-ttu-id="34eb0-111">Das folgende 1.x-Beispiel konfiguriert die Facebook-Authentifizierung mit der Identität in *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="34eb0-111">The following 1.x example configures Facebook authentication with Identity in *Startup.cs*:</span></span>

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

<span data-ttu-id="34eb0-112">In Projekten auf 2.0 ist die Authentifizierung über Dienste konfiguriert.</span><span class="sxs-lookup"><span data-stu-id="34eb0-112">In 2.0 projects, authentication is configured via services.</span></span> <span data-ttu-id="34eb0-113">Jede Authentifizierungsschema ist registriert, der `ConfigureServices` Methode *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="34eb0-113">Each authentication scheme is registered in the `ConfigureServices` method of *Startup.cs*.</span></span> <span data-ttu-id="34eb0-114">Die `UseIdentity` Methode durch ersetzt `UseAuthentication`.</span><span class="sxs-lookup"><span data-stu-id="34eb0-114">The `UseIdentity` method is replaced with `UseAuthentication`.</span></span>

<span data-ttu-id="34eb0-115">Der folgende 2.0 konfiguriert Facebook-Authentifizierung mit der Identität in *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="34eb0-115">The following 2.0 example configures Facebook authentication with Identity in *Startup.cs*:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddIdentity<ApplicationUser, IdentityRole>()
            .AddEntityFrameworkStores<ApplicationDbContext>();

    // If you want to tweak Identity cookies, they're no longer part of IdentityOptions.
    services.ConfigureApplicationCookie(options => options.LoginPath = "/Account/LogIn");
    services.AddAuthentication()
            .AddFacebook(options => {
                options.AppId = Configuration["auth:facebook:appid"];
                options.AppSecret = Configuration["auth:facebook:appsecret"];
            });
}

public void Configure(IApplicationBuilder app, ILoggerFactory loggerfactory) {
    app.UseAuthentication();
}
```

<span data-ttu-id="34eb0-116">Die `UseAuthentication` Methode fügt eine einzelnen Komponente, die für die automatische Authentifizierung und die Behandlung von Anforderungen Remoteauthentifizierung zuständig ist.</span><span class="sxs-lookup"><span data-stu-id="34eb0-116">The `UseAuthentication` method adds a single authentication middleware component which is responsible for automatic authentication and the handling of remote authentication requests.</span></span> <span data-ttu-id="34eb0-117">Er ersetzt alle einzelnen Middleware-Komponenten mit einer einzelnen, gemeinsamen middlewarekomponente.</span><span class="sxs-lookup"><span data-stu-id="34eb0-117">It replaces all of the individual middleware components with a single, common middleware component.</span></span>

<span data-ttu-id="34eb0-118">Im folgenden sind 2.0 Anweisungen zur Migration für jede größere Authentifizierungsschema.</span><span class="sxs-lookup"><span data-stu-id="34eb0-118">Below are 2.0 migration instructions for each major authentication scheme.</span></span>

### <a name="cookie-based-authentication"></a><span data-ttu-id="34eb0-119">Cookie-basierte Authentifizierung</span><span class="sxs-lookup"><span data-stu-id="34eb0-119">Cookie-based Authentication</span></span>
<span data-ttu-id="34eb0-120">Wählen Sie eine der folgenden beiden Optionen aus, und stellen Sie die erforderlichen Änderungen im *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="34eb0-120">Select one of the two options below, and make the necessary changes in *Startup.cs*:</span></span>

1. <span data-ttu-id="34eb0-121">Cookies verwenden, mit der Identität</span><span class="sxs-lookup"><span data-stu-id="34eb0-121">Use cookies with Identity</span></span>
    - <span data-ttu-id="34eb0-122">Ersetzen Sie `UseIdentity` mit `UseAuthentication` in die `Configure` Methode:</span><span class="sxs-lookup"><span data-stu-id="34eb0-122">Replace `UseIdentity` with `UseAuthentication` in the `Configure` method:</span></span>

        ```csharp
        app.UseAuthentication();
        ```

    - <span data-ttu-id="34eb0-123">Aufrufen der `AddIdentity` Methode in der `ConfigureServices` Methode, um die Cookie-Authentifizierungsdienste hinzuzufügen.</span><span class="sxs-lookup"><span data-stu-id="34eb0-123">Invoke the `AddIdentity` method in the `ConfigureServices` method to add the cookie authentication services.</span></span>
    - <span data-ttu-id="34eb0-124">Rufen Sie optional die `ConfigureApplicationCookie` oder `ConfigureExternalCookie` Methode in der `ConfigureServices` Methode, um die Identität Cookie-Einstellungen zu optimieren.</span><span class="sxs-lookup"><span data-stu-id="34eb0-124">Optionally, invoke the `ConfigureApplicationCookie` or `ConfigureExternalCookie` method in the `ConfigureServices` method to tweak the Identity cookie settings.</span></span>

        ```csharp
        services.AddIdentity<ApplicationUser, IdentityRole>()
                .AddEntityFrameworkStores<ApplicationDbContext>()
                .AddDefaultTokenProviders();
    
        services.ConfigureApplicationCookie(options => options.LoginPath = "/Account/LogIn");
        ```

2. <span data-ttu-id="34eb0-125">Cookies ohne Identität verwenden</span><span class="sxs-lookup"><span data-stu-id="34eb0-125">Use cookies without Identity</span></span>
    - <span data-ttu-id="34eb0-126">Ersetzen Sie die `UseCookieAuthentication` -Methodenaufruf in der `Configure` Methode mit `UseAuthentication`:</span><span class="sxs-lookup"><span data-stu-id="34eb0-126">Replace the `UseCookieAuthentication` method call in the `Configure` method with `UseAuthentication`:</span></span>
  
        ```csharp
        app.UseAuthentication();
        ```
 
    - <span data-ttu-id="34eb0-127">Aufrufen der `AddAuthentication` und `AddCookie` Methoden in der `ConfigureServices` Methode:</span><span class="sxs-lookup"><span data-stu-id="34eb0-127">Invoke the `AddAuthentication` and `AddCookie` methods in the `ConfigureServices` method:</span></span>

        ```csharp
        // If you don't want the cookie to be automatically authenticated and assigned to HttpContext.User, 
        // remove the CookieAuthenticationDefaults.AuthenticationScheme parameter passed to AddAuthentication.
        services.AddAuthentication(CookieAuthenticationDefaults.AuthenticationScheme)
                .AddCookie(options => {
                    options.LoginPath = "/Account/LogIn";
                    options.LogoutPath = "/Account/LogOff";
                });
        ```

### <a name="jwt-bearer-authentication"></a><span data-ttu-id="34eb0-128">JWT-Trägertoken-Authentifizierung</span><span class="sxs-lookup"><span data-stu-id="34eb0-128">JWT Bearer Authentication</span></span>
<span data-ttu-id="34eb0-129">Nehmen Sie die folgenden Änderungen in *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="34eb0-129">Make the following changes in *Startup.cs*:</span></span>
- <span data-ttu-id="34eb0-130">Ersetzen Sie die `UseJwtBearerAuthentication` -Methodenaufruf in der `Configure` Methode mit `UseAuthentication`:</span><span class="sxs-lookup"><span data-stu-id="34eb0-130">Replace the `UseJwtBearerAuthentication` method call in the `Configure` method with `UseAuthentication`:</span></span>
 
    ```csharp
    app.UseAuthentication();
    ```

- <span data-ttu-id="34eb0-131">Aufrufen der `AddJwtBearer` Methode in der `ConfigureServices` Methode:</span><span class="sxs-lookup"><span data-stu-id="34eb0-131">Invoke the `AddJwtBearer` method in the `ConfigureServices` method:</span></span>

    ```csharp
    services.AddAuthentication(JwtBearerDefaults.AuthenticationScheme)
            .AddJwtBearer(options => {
                options.Audience = "http://localhost:5001/";
                options.Authority = "http://localhost:5000/";
            });
    ```

    <span data-ttu-id="34eb0-132">Dieser Codeausschnitt verwendet keine Identität, daher sollte das Standardschema durch Übergabe festgelegt werden `JwtBearerDefaults.AuthenticationScheme` auf die `AddAuthentication` Methode.</span><span class="sxs-lookup"><span data-stu-id="34eb0-132">This code snippet doesn't use Identity, so the default scheme should be set by passing `JwtBearerDefaults.AuthenticationScheme` to the `AddAuthentication` method.</span></span>

### <a name="openid-connect-oidc-authentication"></a><span data-ttu-id="34eb0-133">OpenID Connect-Authentifizierung (OIDC)</span><span class="sxs-lookup"><span data-stu-id="34eb0-133">OpenID Connect (OIDC) Authentication</span></span>
<span data-ttu-id="34eb0-134">Nehmen Sie die folgenden Änderungen in *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="34eb0-134">Make the following changes in *Startup.cs*:</span></span>

- <span data-ttu-id="34eb0-135">Ersetzen Sie die `UseOpenIdConnectAuthentication` -Methodenaufruf in der `Configure` Methode mit `UseAuthentication`:</span><span class="sxs-lookup"><span data-stu-id="34eb0-135">Replace the `UseOpenIdConnectAuthentication` method call in the `Configure` method with `UseAuthentication`:</span></span>

    ```csharp
    app.UseAuthentication();
    ```

- <span data-ttu-id="34eb0-136">Aufrufen der `AddOpenIdConnect` Methode in der `ConfigureServices` Methode:</span><span class="sxs-lookup"><span data-stu-id="34eb0-136">Invoke the `AddOpenIdConnect` method in the `ConfigureServices` method:</span></span>

    ```csharp
    services.AddAuthentication(options => {
        options.DefaultScheme = CookieAuthenticationDefaults.AuthenticationScheme;
        options.DefaultChallengeScheme = OpenIdConnectDefaults.AuthenticationScheme;
    })
    .AddCookie()
    .AddOpenIdConnect(options => {
        options.Authority = Configuration["auth:oidc:authority"];
        options.ClientId = Configuration["auth:oidc:clientid"];
    });
    ```

### <a name="facebook-authentication"></a><span data-ttu-id="34eb0-137">Facebook-Authentifizierung</span><span class="sxs-lookup"><span data-stu-id="34eb0-137">Facebook Authentication</span></span>
<span data-ttu-id="34eb0-138">Nehmen Sie die folgenden Änderungen in *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="34eb0-138">Make the following changes in *Startup.cs*:</span></span>
- <span data-ttu-id="34eb0-139">Ersetzen Sie die `UseFacebookAuthentication` -Methodenaufruf in der `Configure` Methode mit `UseAuthentication`:</span><span class="sxs-lookup"><span data-stu-id="34eb0-139">Replace the `UseFacebookAuthentication` method call in the `Configure` method with `UseAuthentication`:</span></span>
 
    ```csharp
    app.UseAuthentication();
    ```

- <span data-ttu-id="34eb0-140">Aufrufen der `AddFacebook` Methode in der `ConfigureServices` Methode:</span><span class="sxs-lookup"><span data-stu-id="34eb0-140">Invoke the `AddFacebook` method in the `ConfigureServices` method:</span></span>
    
    ```csharp
    services.AddAuthentication()
            .AddFacebook(options => {
                options.AppId = Configuration["auth:facebook:appid"];
                options.AppSecret = Configuration["auth:facebook:appsecret"];
            });
    ```

### <a name="google-authentication"></a><span data-ttu-id="34eb0-141">Google-Authentifizierung</span><span class="sxs-lookup"><span data-stu-id="34eb0-141">Google Authentication</span></span>
<span data-ttu-id="34eb0-142">Nehmen Sie die folgenden Änderungen in *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="34eb0-142">Make the following changes in *Startup.cs*:</span></span>
- <span data-ttu-id="34eb0-143">Ersetzen Sie die `UseGoogleAuthentication` -Methodenaufruf in der `Configure` Methode mit `UseAuthentication`:</span><span class="sxs-lookup"><span data-stu-id="34eb0-143">Replace the `UseGoogleAuthentication` method call in the `Configure` method with `UseAuthentication`:</span></span>
 
    ```csharp
    app.UseAuthentication();
    ```

- <span data-ttu-id="34eb0-144">Aufrufen der `AddGoogle` Methode in der `ConfigureServices` Methode:</span><span class="sxs-lookup"><span data-stu-id="34eb0-144">Invoke the `AddGoogle` method in the `ConfigureServices` method:</span></span>

    ```csharp
    services.AddAuthentication()
            .AddGoogle(options => {
                options.ClientId = Configuration["auth:google:clientid"];
                options.ClientSecret = Configuration["auth:google:clientsecret"];
            });    
    ```

### <a name="microsoft-account-authentication"></a><span data-ttu-id="34eb0-145">Microsoft-Konto-Authentifizierung</span><span class="sxs-lookup"><span data-stu-id="34eb0-145">Microsoft Account Authentication</span></span>
<span data-ttu-id="34eb0-146">Nehmen Sie die folgenden Änderungen in *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="34eb0-146">Make the following changes in *Startup.cs*:</span></span>
- <span data-ttu-id="34eb0-147">Ersetzen Sie die `UseMicrosoftAccountAuthentication` -Methodenaufruf in der `Configure` Methode mit `UseAuthentication`:</span><span class="sxs-lookup"><span data-stu-id="34eb0-147">Replace the `UseMicrosoftAccountAuthentication` method call in the `Configure` method with `UseAuthentication`:</span></span>

    ```csharp
    app.UseAuthentication();
    ```

- <span data-ttu-id="34eb0-148">Aufrufen der `AddMicrosoftAccount` Methode in der `ConfigureServices` Methode:</span><span class="sxs-lookup"><span data-stu-id="34eb0-148">Invoke the `AddMicrosoftAccount` method in the `ConfigureServices` method:</span></span>

    ```csharp
    services.AddAuthentication()
            .AddMicrosoftAccount(options => {
                options.ClientId = Configuration["auth:microsoft:clientid"];
                options.ClientSecret = Configuration["auth:microsoft:clientsecret"];
            });
    ``` 

### <a name="twitter-authentication"></a><span data-ttu-id="34eb0-149">Twitter-Authentifizierung</span><span class="sxs-lookup"><span data-stu-id="34eb0-149">Twitter Authentication</span></span>
<span data-ttu-id="34eb0-150">Nehmen Sie die folgenden Änderungen in *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="34eb0-150">Make the following changes in *Startup.cs*:</span></span>
- <span data-ttu-id="34eb0-151">Ersetzen Sie die `UseTwitterAuthentication` -Methodenaufruf in der `Configure` Methode mit `UseAuthentication`:</span><span class="sxs-lookup"><span data-stu-id="34eb0-151">Replace the `UseTwitterAuthentication` method call in the `Configure` method with `UseAuthentication`:</span></span>
 
    ```csharp
    app.UseAuthentication();
    ```

- <span data-ttu-id="34eb0-152">Aufrufen der `AddTwitter` Methode in der `ConfigureServices` Methode:</span><span class="sxs-lookup"><span data-stu-id="34eb0-152">Invoke the `AddTwitter` method in the `ConfigureServices` method:</span></span>

    ```csharp
    services.AddAuthentication()
            .AddTwitter(options => {
                options.ConsumerKey = Configuration["auth:twitter:consumerkey"];
                options.ConsumerSecret = Configuration["auth:twitter:consumersecret"];
            });
    ```

### <a name="setting-default-authentication-schemes"></a><span data-ttu-id="34eb0-153">Standard-Authentifizierungsschemas festlegen</span><span class="sxs-lookup"><span data-stu-id="34eb0-153">Setting Default Authentication Schemes</span></span>
<span data-ttu-id="34eb0-154">In 1.x die `AutomaticAuthenticate` und `AutomaticChallenge` Eigenschaften wurden auf einem einzelnen Authentifizierungsschema festgelegt werden soll.</span><span class="sxs-lookup"><span data-stu-id="34eb0-154">In 1.x, the `AutomaticAuthenticate` and `AutomaticChallenge` properties were intended to be set on a single authentication scheme.</span></span> <span data-ttu-id="34eb0-155">Es wurde keine gute Möglichkeit, dies zu erzwingen.</span><span class="sxs-lookup"><span data-stu-id="34eb0-155">There was no good way to enforce this.</span></span>

<span data-ttu-id="34eb0-156">In 2.0 wurden diese beiden Eigenschaften als Flags in den einzelnen entfernt `AuthenticationOptions` -Instanz und verschoben haben, in die Basis [authenticationoptions](/aspnet/core/api/microsoft.aspnetcore.builder.authenticationoptions) Klasse.</span><span class="sxs-lookup"><span data-stu-id="34eb0-156">In 2.0, these two properties have been removed as flags on the individual `AuthenticationOptions` instance and have moved into the base [AuthenticationOptions](/aspnet/core/api/microsoft.aspnetcore.builder.authenticationoptions) class.</span></span> <span data-ttu-id="34eb0-157">Die Eigenschaften können konfiguriert werden, der `AddAuthentication` Methodenaufruf innerhalb der `ConfigureServices` Methode *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="34eb0-157">The properties can be configured in the `AddAuthentication` method call within the `ConfigureServices` method of *Startup.cs*:</span></span>

```csharp
services.AddAuthentication(CookieAuthenticationDefaults.AuthenticationScheme);
```

<span data-ttu-id="34eb0-158">Alternativ können Sie eine überladene Version der verwenden die `AddAuthentication` Methode, um mehr als eine Eigenschaft festgelegt.</span><span class="sxs-lookup"><span data-stu-id="34eb0-158">Alternatively, use an overloaded version of the `AddAuthentication` method to set more than one property.</span></span> <span data-ttu-id="34eb0-159">Im folgenden Beispiel für die überladene Methode wird das Standardschema so eingerichtet `CookieAuthenticationDefaults.AuthenticationScheme`.</span><span class="sxs-lookup"><span data-stu-id="34eb0-159">In the following overloaded method example, the default scheme is set to `CookieAuthenticationDefaults.AuthenticationScheme`.</span></span> <span data-ttu-id="34eb0-160">Das Authentifizierungsschema kann auch angegeben werden, innerhalb der einzelnen `[Authorize]` Attribute oder Autorisierungsrichtlinien.</span><span class="sxs-lookup"><span data-stu-id="34eb0-160">The authentication scheme may alternatively be specified within your individual `[Authorize]` attributes or authorization policies.</span></span>

```csharp
services.AddAuthentication(options => {
    options.DefaultScheme = CookieAuthenticationDefaults.AuthenticationScheme;
    options.DefaultChallengeScheme = OpenIdConnectDefaults.AuthenticationScheme;
});
```

<span data-ttu-id="34eb0-161">Definieren Sie ein Standardschema in 2.0, wenn eine der folgenden Bedingungen erfüllt ist:</span><span class="sxs-lookup"><span data-stu-id="34eb0-161">Define a default scheme in 2.0 if one of the following conditions is true:</span></span>
- <span data-ttu-id="34eb0-162">Sie möchten die Benutzer automatisch angemeldet werden</span><span class="sxs-lookup"><span data-stu-id="34eb0-162">You want the user to be automatically signed in</span></span>
- <span data-ttu-id="34eb0-163">Sie verwenden die `[Authorize]` Attribut oder Autorisierung Richtlinien ohne Angabe von Schemas</span><span class="sxs-lookup"><span data-stu-id="34eb0-163">You use the `[Authorize]` attribute or authorization policies without specifying schemes</span></span>

<span data-ttu-id="34eb0-164">Eine Ausnahme von dieser Regel wird die `AddIdentity` Methode.</span><span class="sxs-lookup"><span data-stu-id="34eb0-164">An exception to this rule is the `AddIdentity` method.</span></span> <span data-ttu-id="34eb0-165">Diese Methode fügt Cookies für Sie und legt die Standardeinstellung zu authentifizieren und Aufforderung Schemas an, die das Cookie Anwendung `IdentityConstants.ApplicationScheme`.</span><span class="sxs-lookup"><span data-stu-id="34eb0-165">This method adds cookies for you and sets the default authenticate and challenge schemes to the application cookie `IdentityConstants.ApplicationScheme`.</span></span> <span data-ttu-id="34eb0-166">Darüber hinaus wird die Standard-Anmeldeseite-Schema an das externe Cookie `IdentityConstants.ExternalScheme`.</span><span class="sxs-lookup"><span data-stu-id="34eb0-166">Additionally, it sets the default sign-in scheme to the external cookie `IdentityConstants.ExternalScheme`.</span></span>

<a name="obsolete-interface"></a>

## <a name="use-httpcontext-authentication-extensions"></a><span data-ttu-id="34eb0-167">Verwenden Sie HttpContext Authentifizierungserweiterungen</span><span class="sxs-lookup"><span data-stu-id="34eb0-167">Use HttpContext Authentication Extensions</span></span>
<span data-ttu-id="34eb0-168">Die `IAuthenticationManager` Schnittstelle ist der Haupteinstiegspunkt, in dem 1.x-Authentifizierungssystem.</span><span class="sxs-lookup"><span data-stu-id="34eb0-168">The `IAuthenticationManager` interface is the main entry point into the 1.x authentication system.</span></span> <span data-ttu-id="34eb0-169">Es wurde durch einen neuen Satz von ersetzt `HttpContext` Erweiterungsmethoden in den `Microsoft.AspNetCore.Authentication` Namespace.</span><span class="sxs-lookup"><span data-stu-id="34eb0-169">It has been replaced with a new set of `HttpContext` extension methods in the `Microsoft.AspNetCore.Authentication` namespace.</span></span>

<span data-ttu-id="34eb0-170">Z. B. Projekten 1.x Verweis eine `Authentication` Eigenschaft:</span><span class="sxs-lookup"><span data-stu-id="34eb0-170">For example, 1.x projects reference an `Authentication` property:</span></span>

[!code-csharp[Main](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Controllers/AccountController.cs?name=snippet_AuthenticationProperty)]

<span data-ttu-id="34eb0-171">In Projekten auf 2.0 Importieren der `Microsoft.AspNetCore.Authentication` Namespace, und löschen Sie die `Authentication` Eigenschaftenverweise:</span><span class="sxs-lookup"><span data-stu-id="34eb0-171">In 2.0 projects, import the `Microsoft.AspNetCore.Authentication` namespace, and delete the `Authentication` property references:</span></span>

[!code-csharp[Main](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Controllers/AccountController.cs?name=snippet_AuthenticationProperty)]

<a name="windows-auth-changes"></a>

## <a name="windows-authentication-httpsys--iisintegration"></a><span data-ttu-id="34eb0-172">Windows-Authentifizierung (HTTP / IISIntegration)</span><span class="sxs-lookup"><span data-stu-id="34eb0-172">Windows Authentication (HTTP.sys / IISIntegration)</span></span>
<span data-ttu-id="34eb0-173">Es gibt zwei Variationen der Windows-Authentifizierung:</span><span class="sxs-lookup"><span data-stu-id="34eb0-173">There are two variations of Windows authentication:</span></span>
1. <span data-ttu-id="34eb0-174">Der Host nur ermöglicht authentifizierten Benutzern.</span><span class="sxs-lookup"><span data-stu-id="34eb0-174">The host only allows authenticated users</span></span>
2. <span data-ttu-id="34eb0-175">Der Host kann sowohl anonyme und authentifizierte Benutzer</span><span class="sxs-lookup"><span data-stu-id="34eb0-175">The host allows both anonymous and authenticated users</span></span>

<span data-ttu-id="34eb0-176">Die erste Variante, die oben beschriebenen ist von den 2.0 Änderungen nicht betroffen.</span><span class="sxs-lookup"><span data-stu-id="34eb0-176">The first variation described above is unaffected by the 2.0 changes.</span></span>

<span data-ttu-id="34eb0-177">Die zweite Variante, die oben beschriebenen wird durch die 2.0 Änderungen beeinflusst.</span><span class="sxs-lookup"><span data-stu-id="34eb0-177">The second variation described above is affected by the 2.0 changes.</span></span> <span data-ttu-id="34eb0-178">Beispielsweise können Sie möglicherweise werden zulassen, dass anonyme Benutzer in Ihre Anwendung auf IIS oder [HTTP.sys](xref:fundamentals/servers/weblistener) jedoch autorisierende von Benutzern auf Controllerebene Ebene.</span><span class="sxs-lookup"><span data-stu-id="34eb0-178">As an example, you may be allowing anonymous users into your application at the IIS or [HTTP.sys](xref:fundamentals/servers/weblistener) layer but authorizing users at the Controller level.</span></span> <span data-ttu-id="34eb0-179">Legen Sie in diesem Szenario wird das Standardschema auf `IISDefaults.AuthenticationScheme` in der `ConfigureServices` Methode *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="34eb0-179">In this scenario, set the default scheme to `IISDefaults.AuthenticationScheme` in the `ConfigureServices` method of *Startup.cs*:</span></span>

```csharp
services.AddAuthentication(IISDefaults.AuthenticationScheme);
```

<span data-ttu-id="34eb0-180">Fehler beim Festlegen der Standard-Schema wird entsprechend verhindert, dass der autorisierungsanforderung zu Herausforderung aus arbeiten.</span><span class="sxs-lookup"><span data-stu-id="34eb0-180">Failure to set the default scheme accordingly prevents the authorize request to challenge from working.</span></span>

<a name="identity-cookie-options"></a>

## <a name="identitycookieoptions-instances"></a><span data-ttu-id="34eb0-181">IdentityCookieOptions-Instanzen</span><span class="sxs-lookup"><span data-stu-id="34eb0-181">IdentityCookieOptions Instances</span></span>
<span data-ttu-id="34eb0-182">Ein Nebeneffekt der 2.0 Änderungen ist die Option zur Verwendung mit dem Namen Optionen anstelle von Instanzen der Cookie-Optionen.</span><span class="sxs-lookup"><span data-stu-id="34eb0-182">A side effect of the 2.0 changes is the switch to using named options instead of cookie options instances.</span></span> <span data-ttu-id="34eb0-183">Anpassen von die Identität Cookie Schemanamen wird entfernt.</span><span class="sxs-lookup"><span data-stu-id="34eb0-183">The ability to customize the Identity cookie scheme names is removed.</span></span>

<span data-ttu-id="34eb0-184">Beispielsweise 1.x Projekte verwenden [Konstruktoreinfügung](xref:mvc/controllers/dependency-injection#constructor-injection) Übergabe einer `IdentityCookieOptions` Parameter in *AccountController.cs*.</span><span class="sxs-lookup"><span data-stu-id="34eb0-184">For example, 1.x projects use [constructor injection](xref:mvc/controllers/dependency-injection#constructor-injection) to pass an `IdentityCookieOptions` parameter into *AccountController.cs*.</span></span> <span data-ttu-id="34eb0-185">Das Authentifizierungsschema externe Cookie wird aus der bereitgestellten Instanz zugegriffen werden:</span><span class="sxs-lookup"><span data-stu-id="34eb0-185">The external cookie authentication scheme is accessed from the provided instance:</span></span>

[!code-csharp[Main](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Controllers/AccountController.cs?name=snippet_AccountControllerConstructor&highlight=4,11)]

<span data-ttu-id="34eb0-186">Die zuvor erwähnten Konstruktoreinfügung unnötige in Projekten auf 2.0 wird und die `_externalCookieScheme` Feld kann gelöscht werden:</span><span class="sxs-lookup"><span data-stu-id="34eb0-186">The aforementioned constructor injection becomes unnecessary in 2.0 projects, and the `_externalCookieScheme` field can be deleted:</span></span>

[!code-csharp[Main](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Controllers/AccountController.cs?name=snippet_AccountControllerConstructor)]

<span data-ttu-id="34eb0-187">Die `IdentityConstants.ExternalScheme` Konstante direkt verwendet werden kann:</span><span class="sxs-lookup"><span data-stu-id="34eb0-187">The `IdentityConstants.ExternalScheme` constant can be used directly:</span></span>

[!code-csharp[Main](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Controllers/AccountController.cs?name=snippet_AuthenticationProperty)]

<a name="navigation-properties"></a>

## <a name="add-identityuser-poco-navigation-properties"></a><span data-ttu-id="34eb0-188">Hinzufügen von IdentityUser POCO-Navigationseigenschaften</span><span class="sxs-lookup"><span data-stu-id="34eb0-188">Add IdentityUser POCO Navigation Properties</span></span>
<span data-ttu-id="34eb0-189">Die Entity Framework (EF) Core Navigationseigenschaften des basistexts `IdentityUser` POCO (Plain Old CLR-Objekt) wurden entfernt.</span><span class="sxs-lookup"><span data-stu-id="34eb0-189">The Entity Framework (EF) Core navigation properties of the base `IdentityUser` POCO (Plain Old CLR Object) have been removed.</span></span> <span data-ttu-id="34eb0-190">Wenn das Projekt 1.x dieser Eigenschaften verwendet, manuell fügen Sie wieder zum Projekt 2.0 hinzu:</span><span class="sxs-lookup"><span data-stu-id="34eb0-190">If your 1.x project used these properties, manually add them back to the 2.0 project:</span></span>

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

<span data-ttu-id="34eb0-191">Um doppelte Fremdschlüssel zu verhindern, dass beim Ausführen von EF Core Migrationen, fügen Sie Folgendes verwenden, um Ihre `IdentityDbContext` Klasse `OnModelCreating` Methode (nach der `base.OnModelCreating();` aufrufen):</span><span class="sxs-lookup"><span data-stu-id="34eb0-191">To prevent duplicate foreign keys when running EF Core Migrations, add the following to your `IdentityDbContext` class' `OnModelCreating` method (after the `base.OnModelCreating();` call):</span></span>

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

## <a name="replace-getexternalauthenticationschemes"></a><span data-ttu-id="34eb0-192">Ersetzen Sie GetExternalAuthenticationSchemes</span><span class="sxs-lookup"><span data-stu-id="34eb0-192">Replace GetExternalAuthenticationSchemes</span></span>
<span data-ttu-id="34eb0-193">Die synchrone Methode `GetExternalAuthenticationSchemes` zugunsten eine asynchrone Version entfernt wurde.</span><span class="sxs-lookup"><span data-stu-id="34eb0-193">The synchronous method `GetExternalAuthenticationSchemes` was removed in favor of an asynchronous version.</span></span> <span data-ttu-id="34eb0-194">1.x-Projekte haben den folgenden Code in *ManageController.cs*:</span><span class="sxs-lookup"><span data-stu-id="34eb0-194">1.x projects have the following code in *ManageController.cs*:</span></span>

[!code-csharp[Main](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Controllers/ManageController.cs?name=snippet_GetExternalAuthenticationSchemes)]

<span data-ttu-id="34eb0-195">Diese Methode ist in *Login.cshtml* zu:</span><span class="sxs-lookup"><span data-stu-id="34eb0-195">This method appears in *Login.cshtml* too:</span></span>

[!code-cshtml[Main](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Views/Account/Login.cshtml?range=62,75-84)]

<span data-ttu-id="34eb0-196">In Projekten auf 2.0 verwenden die `GetExternalAuthenticationSchemesAsync` Methode:</span><span class="sxs-lookup"><span data-stu-id="34eb0-196">In 2.0 projects, use the `GetExternalAuthenticationSchemesAsync` method:</span></span>

[!code-csharp[Main](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Controllers/ManageController.cs?name=snippet_GetExternalAuthenticationSchemesAsync)]

<span data-ttu-id="34eb0-197">In *Login.cshtml*, `AuthenticationScheme` Eigenschaft zugegriffen wird, der `foreach` Schleife Änderungen an `Name`:</span><span class="sxs-lookup"><span data-stu-id="34eb0-197">In *Login.cshtml*, the `AuthenticationScheme` property accessed in the `foreach` loop changes to `Name`:</span></span>

[!code-cshtml[Main](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Views/Account/Login.cshtml?range=62,75-84)]

<a name="property-change"></a>

## <a name="manageloginsviewmodel-property-change"></a><span data-ttu-id="34eb0-198">ManageLoginsViewModel-Eigenschaftenänderung</span><span class="sxs-lookup"><span data-stu-id="34eb0-198">ManageLoginsViewModel Property Change</span></span>
<span data-ttu-id="34eb0-199">Ein `ManageLoginsViewModel` Objekt werden in der `ManageLogins` Aktion *ManageController.cs*.</span><span class="sxs-lookup"><span data-stu-id="34eb0-199">A `ManageLoginsViewModel` object is used in the `ManageLogins` action of *ManageController.cs*.</span></span> <span data-ttu-id="34eb0-200">In 1.x-Projekten, das Objekt des `OtherLogins` Eigenschaft Rückgabetyp ist `IList<AuthenticationDescription>`.</span><span class="sxs-lookup"><span data-stu-id="34eb0-200">In 1.x projects, the object's `OtherLogins` property return type is `IList<AuthenticationDescription>`.</span></span> <span data-ttu-id="34eb0-201">Dieser Rückgabetyp erfordert einen Import eines `Microsoft.AspNetCore.Http.Authentication`:</span><span class="sxs-lookup"><span data-stu-id="34eb0-201">This return type requires an import of `Microsoft.AspNetCore.Http.Authentication`:</span></span>

[!code-csharp[Main](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Models/ManageViewModels/ManageLoginsViewModel.cs?name=snippet_ManageLoginsViewModel&highlight=2,11)]

<span data-ttu-id="34eb0-202">In Projekten auf 2.0 ändert sich der Rückgabetyp in `IList<AuthenticationScheme>`.</span><span class="sxs-lookup"><span data-stu-id="34eb0-202">In 2.0 projects, the return type changes to `IList<AuthenticationScheme>`.</span></span> <span data-ttu-id="34eb0-203">Diese neue Rückgabetyp erfordert, und Ersetzen Sie dabei die `Microsoft.AspNetCore.Http.Authentication` importieren Sie mit einer `Microsoft.AspNetCore.Authentication` importieren.</span><span class="sxs-lookup"><span data-stu-id="34eb0-203">This new return type requires replacing the `Microsoft.AspNetCore.Http.Authentication` import with a `Microsoft.AspNetCore.Authentication` import.</span></span>

[!code-csharp[Main](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Models/ManageViewModels/ManageLoginsViewModel.cs?name=snippet_ManageLoginsViewModel&highlight=2,11)]

<a name="additional-resources"></a>

## <a name="additional-resources"></a><span data-ttu-id="34eb0-204">Zusätzliche Ressourcen</span><span class="sxs-lookup"><span data-stu-id="34eb0-204">Additional Resources</span></span>
<span data-ttu-id="34eb0-205">Weitere Details und Erläuterung finden Sie in der [Erörterung Auth 2.0](https://github.com/aspnet/Security/issues/1338) Problem auf GitHub.</span><span class="sxs-lookup"><span data-stu-id="34eb0-205">For additional details and discussion, see the [Discussion for Auth 2.0](https://github.com/aspnet/Security/issues/1338) issue on GitHub.</span></span>
