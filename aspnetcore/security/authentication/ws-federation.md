---
title: Authentifizieren von Benutzern mit WS-Verbund in ASP.NET Core
author: chlowell
description: Dieses Lernprogramm veranschaulicht die Verwendung von WS-Verbund in einer ASP.NET Core-app.
manager: wpickett
ms.author: scaddie
ms.custom: mvc
ms.date: 02/27/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authentication/ws-federation
ms.openlocfilehash: d4621c7b97678903b9f2562e353da3883334b599
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
ms.locfileid: "30898803"
---
# <a name="authenticate-users-with-ws-federation-in-aspnet-core"></a><span data-ttu-id="2f668-103">Authentifizieren von Benutzern mit WS-Verbund in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="2f668-103">Authenticate users with WS-Federation in ASP.NET Core</span></span>

<span data-ttu-id="2f668-104">Dieses Lernprogramm veranschaulicht, wie Benutzer zur Anmeldung mit eines WS-Verbund-Authentifizierungsanbieter wie Active Directory Federation Services (ADFS) oder [Azure Active Directory](/azure/active-directory/) (AAD).</span><span class="sxs-lookup"><span data-stu-id="2f668-104">This tutorial demonstrates how to enable users to sign in with a WS-Federation authentication provider like Active Directory Federation Services (ADFS) or [Azure Active Directory](/azure/active-directory/) (AAD).</span></span> <span data-ttu-id="2f668-105">Er verwendet die ASP.NET Core 2.0 Beispiel-app, die in beschriebenen [Facebook, Google, und die externe Anbieter Authentifizierung](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="2f668-105">It uses the ASP.NET Core 2.0 sample app described in [Facebook, Google, and external provider authentication](xref:security/authentication/social/index).</span></span>

<span data-ttu-id="2f668-106">Nähere Informationen zu ASP.NET Core 2.0 apps WS-Verbund-Unterstützung wird durch bereitgestellt [Microsoft.AspNetCore.Authentication.WsFederation](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.WsFederation).</span><span class="sxs-lookup"><span data-stu-id="2f668-106">For ASP.NET Core 2.0 apps, WS-Federation support is provided by [Microsoft.AspNetCore.Authentication.WsFederation](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.WsFederation).</span></span> <span data-ttu-id="2f668-107">Diese Komponente wird von portiert [Microsoft.Owin.Security.WsFederation](https://www.nuget.org/packages/Microsoft.Owin.Security.WsFederation) und teilen sich viele Mechanismen für diese Komponente.</span><span class="sxs-lookup"><span data-stu-id="2f668-107">This component is ported from [Microsoft.Owin.Security.WsFederation](https://www.nuget.org/packages/Microsoft.Owin.Security.WsFederation) and shares many of that component's mechanics.</span></span> <span data-ttu-id="2f668-108">Allerdings unterscheiden sich die Komponenten, in einigen wichtigen Aspekten zu erhalten.</span><span class="sxs-lookup"><span data-stu-id="2f668-108">However, the components differ in a couple of important ways.</span></span>

<span data-ttu-id="2f668-109">Standardmäßig ist die neue Middleware:</span><span class="sxs-lookup"><span data-stu-id="2f668-109">By default, the new middleware:</span></span>

* <span data-ttu-id="2f668-110">Keine unerwünschte Anmeldungen zu ermöglichen.</span><span class="sxs-lookup"><span data-stu-id="2f668-110">Doesn't allow unsolicited logins.</span></span> <span data-ttu-id="2f668-111">Diese Funktion von der WS-Verbund-Protokoll ist anfällig für XSRF-Angriffen.</span><span class="sxs-lookup"><span data-stu-id="2f668-111">This feature of the WS-Federation protocol is vulnerable to XSRF attacks.</span></span> <span data-ttu-id="2f668-112">Dies kann jedoch aktiviert werden, mit der `AllowUnsolicitedLogins` Option.</span><span class="sxs-lookup"><span data-stu-id="2f668-112">However, it can be enabled with the `AllowUnsolicitedLogins` option.</span></span>
* <span data-ttu-id="2f668-113">Jedes Formular-Post in Anmeldenachrichten nicht überprüft werden.</span><span class="sxs-lookup"><span data-stu-id="2f668-113">Doesn't check every form post for sign-in messages.</span></span> <span data-ttu-id="2f668-114">Nur Anforderungen an die `CallbackPath` Anmeldung Ins überprüft werden `CallbackPath` standardmäßig `/signin-wsfed` kann jedoch geändert werden.</span><span class="sxs-lookup"><span data-stu-id="2f668-114">Only requests to the `CallbackPath` are checked for sign-ins. `CallbackPath` defaults to `/signin-wsfed` but can be changed.</span></span> <span data-ttu-id="2f668-115">Dieser Pfad kann mit anderen Authentifizierungsanbieter freigegeben werden, durch Aktivieren der `SkipUnrecognizedRequests` Option.</span><span class="sxs-lookup"><span data-stu-id="2f668-115">This path can be shared with other authentication providers by enabling the `SkipUnrecognizedRequests` option.</span></span>

## <a name="register-the-app-with-active-directory"></a><span data-ttu-id="2f668-116">Registrieren einer app in Active Directory</span><span class="sxs-lookup"><span data-stu-id="2f668-116">Register the app with Active Directory</span></span>

### <a name="active-directory-federation-services"></a><span data-ttu-id="2f668-117">Active Directory-Verbunddienste</span><span class="sxs-lookup"><span data-stu-id="2f668-117">Active Directory Federation Services</span></span>

* <span data-ttu-id="2f668-118">Öffnen Sie den Server **der vertrauenden Seite Partei vertrauen Assistenten zum Hinzufügen von** über die AD FS-Verwaltungskonsole:</span><span class="sxs-lookup"><span data-stu-id="2f668-118">Open the server's **Add Relying Party Trust Wizard** from the ADFS Management console:</span></span>

![Hinzufügen der vertrauenden Seite Vertrauensstellung Assistenten: Willkommen](ws-federation/_static/AdfsAddTrust.png)

* <span data-ttu-id="2f668-120">Wählen Sie Daten manuell eingeben:</span><span class="sxs-lookup"><span data-stu-id="2f668-120">Choose to enter data manually:</span></span>

![Fügen Sie der vertrauenden Seite Assistent für Vertrauensstellung: Auswählen einer Datenquelle](ws-federation/_static/AdfsSelectDataSource.png)

* <span data-ttu-id="2f668-122">Geben Sie einen Anzeigenamen für die vertrauende Seite.</span><span class="sxs-lookup"><span data-stu-id="2f668-122">Enter a display name for the relying party.</span></span> <span data-ttu-id="2f668-123">Der Name ist nicht wichtig, die ASP.NET Core-app.</span><span class="sxs-lookup"><span data-stu-id="2f668-123">The name isn't important to the ASP.NET Core app.</span></span>

* <span data-ttu-id="2f668-124">[Microsoft.AspNetCore.Authentication.WsFederation](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.WsFederation) lacks support for token encryption, so don't configure a token encryption certificate:</span><span class="sxs-lookup"><span data-stu-id="2f668-124">[Microsoft.AspNetCore.Authentication.WsFederation](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.WsFederation) lacks support for token encryption, so don't configure a token encryption certificate:</span></span>

![Fügen Sie der vertrauenden Seite Assistent für Vertrauensstellung: Zertifikat konfigurieren](ws-federation/_static/AdfsConfigureCert.png)

* <span data-ttu-id="2f668-126">Aktivieren der Unterstützung für passiven WS-Verbund-Protokoll, mit der app-URL.</span><span class="sxs-lookup"><span data-stu-id="2f668-126">Enable support for WS-Federation Passive protocol, using the app's URL.</span></span> <span data-ttu-id="2f668-127">Stellen Sie sicher, dass der Port für die app richtig ist:</span><span class="sxs-lookup"><span data-stu-id="2f668-127">Verify the port is correct for the app:</span></span>

![Hinzufügen Assistent für Vertrauensstellung der vertrauenden Seite an: Konfigurieren von URL](ws-federation/_static/AdfsConfigureUrl.png)

> [!NOTE]
> <span data-ttu-id="2f668-129">Dies muss eine HTTPS-URL sein.</span><span class="sxs-lookup"><span data-stu-id="2f668-129">This must be an HTTPS URL.</span></span> <span data-ttu-id="2f668-130">IIS Express können ein selbstsigniertes Zertifikat bereitstellen, wenn Sie die app während der Entwicklung zu hosten.</span><span class="sxs-lookup"><span data-stu-id="2f668-130">IIS Express can provide a self-signed certificate when hosting the app during development.</span></span> <span data-ttu-id="2f668-131">Kestrel ist die manuelle Konfiguration erforderlich.</span><span class="sxs-lookup"><span data-stu-id="2f668-131">Kestrel requires manual certificate configuration.</span></span> <span data-ttu-id="2f668-132">Finden Sie unter der [Kestrel Dokumentation](xref:fundamentals/servers/kestrel) Weitere Details.</span><span class="sxs-lookup"><span data-stu-id="2f668-132">See the [Kestrel documentation](xref:fundamentals/servers/kestrel) for more details.</span></span>

* <span data-ttu-id="2f668-133">Klicken Sie auf **Weiter** über den Assistenten ab und **schließen** am Ende.</span><span class="sxs-lookup"><span data-stu-id="2f668-133">Click **Next** through the rest of the wizard and **Close** at the end.</span></span>

* <span data-ttu-id="2f668-134">ASP.NET Core Identität erfordert eine **Namensbezeichner** Anspruch.</span><span class="sxs-lookup"><span data-stu-id="2f668-134">ASP.NET Core Identity requires a **Name ID** claim.</span></span> <span data-ttu-id="2f668-135">Fügen Sie einen aus der **Edit Claim Rules** Dialogfeld:</span><span class="sxs-lookup"><span data-stu-id="2f668-135">Add one from the **Edit Claim Rules** dialog:</span></span>

![Anspruchsregeln bearbeiten](ws-federation/_static/EditClaimRules.png)

* <span data-ttu-id="2f668-137">In der **transformieren Anspruch Assistent zum Hinzufügen einer**, übernehmen Sie den Standardport **LDAP-Attribute als Ansprüche senden** Vorlage ausgewählt, und klicken Sie auf **Weiter**.</span><span class="sxs-lookup"><span data-stu-id="2f668-137">In the **Add Transform Claim Rule Wizard**, leave the default **Send LDAP Attributes as Claims** template selected, and click **Next**.</span></span> <span data-ttu-id="2f668-138">Fügen Sie eine Regel Zuordnung der **SAM-Kontoname** LDAP-Attribut, um die **namens-ID** ausgehenden Anspruchs:</span><span class="sxs-lookup"><span data-stu-id="2f668-138">Add a rule mapping the **SAM-Account-Name** LDAP attribute to the **Name ID** outgoing claim:</span></span>

![Transformation Anspruch Assistent zum Hinzufügen von: Konfigurieren Sie Anspruchsregel](ws-federation/_static/AddTransformClaimRule.png)

* <span data-ttu-id="2f668-140">Klicken Sie auf **Fertig stellen** > **OK** in der **Edit Claim Rules** Fenster.</span><span class="sxs-lookup"><span data-stu-id="2f668-140">Click **Finish** > **OK** in the **Edit Claim Rules** window.</span></span>

### <a name="azure-active-directory"></a><span data-ttu-id="2f668-141">Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="2f668-141">Azure Active Directory</span></span>

* <span data-ttu-id="2f668-142">Navigieren Sie zu der AAD-Mandanten app Registrierungen Blatt.</span><span class="sxs-lookup"><span data-stu-id="2f668-142">Navigate to the AAD tenant's app registrations blade.</span></span> <span data-ttu-id="2f668-143">Klicken Sie auf **neue anwendungsregistrierung**:</span><span class="sxs-lookup"><span data-stu-id="2f668-143">Click **New application registration**:</span></span>

![Azure Active Directory: App-Registrierungen](ws-federation/_static/AadNewAppRegistration.png)

* <span data-ttu-id="2f668-145">Geben Sie einen Namen für die app-Registrierung aus.</span><span class="sxs-lookup"><span data-stu-id="2f668-145">Enter a name for the app registration.</span></span> <span data-ttu-id="2f668-146">Dies ist wichtig, die ASP.NET Core-app nicht.</span><span class="sxs-lookup"><span data-stu-id="2f668-146">This isn't important to the ASP.NET Core app.</span></span>
* <span data-ttu-id="2f668-147">Geben Sie die URL die app auf wie lauscht die **Anmelde-URL**:</span><span class="sxs-lookup"><span data-stu-id="2f668-147">Enter the URL the app listens on as the **Sign-on URL**:</span></span>

![Azure Active Directory: Erstellen Sie app-Registrierung.](ws-federation/_static/AadCreateAppRegistration.png)

* <span data-ttu-id="2f668-149">Klicken Sie auf **Endpunkte** und notieren Sie sich die **Verbundmetadatendokument** URL.</span><span class="sxs-lookup"><span data-stu-id="2f668-149">Click **Endpoints** and note the **Federation Metadata Document** URL.</span></span> <span data-ttu-id="2f668-150">Dies ist der WS-Verbund-Middleware `MetadataAddress`:</span><span class="sxs-lookup"><span data-stu-id="2f668-150">This is the WS-Federation middleware's `MetadataAddress`:</span></span>

![Azure Active Directory: Endpunkte](ws-federation/_static/AadFederationMetadataDocument.png)

* <span data-ttu-id="2f668-152">Navigieren Sie zu der neuen app-Registrierung.</span><span class="sxs-lookup"><span data-stu-id="2f668-152">Navigate to the new app registration.</span></span> <span data-ttu-id="2f668-153">Klicken Sie auf **Einstellungen** > **Eigenschaften** und notieren Sie sich die **App ID-URI**.</span><span class="sxs-lookup"><span data-stu-id="2f668-153">Click **Settings** > **Properties** and make note of the **App ID URI**.</span></span> <span data-ttu-id="2f668-154">Dies ist der WS-Verbund-Middleware `Wtrealm`:</span><span class="sxs-lookup"><span data-stu-id="2f668-154">This is the WS-Federation middleware's `Wtrealm`:</span></span>

![Azure Active Directory: App-Registrierungseigenschaften](ws-federation/_static/AadAppIdUri.png)

## <a name="add-ws-federation-as-an-external-login-provider-for-aspnet-core-identity"></a><span data-ttu-id="2f668-156">Hinzufügen von WS-Verbund als einen externen Anmeldeanbieter für ASP.NET Core Identität</span><span class="sxs-lookup"><span data-stu-id="2f668-156">Add WS-Federation as an external login provider for ASP.NET Core Identity</span></span>

* <span data-ttu-id="2f668-157">Fügen Sie eine Abhängigkeit auf [Microsoft.AspNetCore.Authentication.WsFederation](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.WsFederation) zum Projekt.</span><span class="sxs-lookup"><span data-stu-id="2f668-157">Add a dependency on [Microsoft.AspNetCore.Authentication.WsFederation](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.WsFederation) to the project.</span></span>
* <span data-ttu-id="2f668-158">Hinzufügen von WS-Verbund, um die `Configure` Methode in *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="2f668-158">Add WS-Federation to the `Configure` method in *Startup.cs*:</span></span>

    ```csharp
    services.AddIdentity<ApplicationUser, IdentityRole>()
        .AddEntityFrameworkStores<ApplicationDbContext>()
        .AddDefaultTokenProviders();

    services.AddAuthentication()
        .AddWsFederation(options =>
        {
            // MetadataAddress represents the Active Directory instance used to authenticate users.
            options.MetadataAddress = "https://<ADFS FQDN or AAD tenant>/FederationMetadata/2007-06/FederationMetadata.xml";

            // Wtrealm is the app's identifier in the Active Directory instance.
            // For ADFS, use the relying party's identifier, its WS-Federation Passive protocol URL:
            options.Wtrealm = "https://localhost:44307/";

            // For AAD, use the App ID URI from the app registration's Properties blade:
            options.Wtrealm = "https://wsfedsample.onmicrosoft.com/bf0e7e6d-056e-4e37-b9a6-2c36797b9f01";
        });

    services.AddMvc()
     // ...
    ```

[!INCLUDE [default settings configuration](social/includes/default-settings.md)]

### <a name="log-in-with-ws-federation"></a><span data-ttu-id="2f668-159">Melden Sie sich mit WS-Verbund</span><span class="sxs-lookup"><span data-stu-id="2f668-159">Log in with WS-Federation</span></span>

<span data-ttu-id="2f668-160">Navigieren Sie zur app, und klicken Sie auf die **melden Sie sich** Link im Nav-Header.</span><span class="sxs-lookup"><span data-stu-id="2f668-160">Browse to the app and click the **Log in** link in the nav header.</span></span> <span data-ttu-id="2f668-161">Es ist eine Option aus, um die Anmeldung WsFederation: ![-Anmeldeseite](ws-federation/_static/WsFederationButton.png)</span><span class="sxs-lookup"><span data-stu-id="2f668-161">There's an option to log in with WsFederation: ![Log in page](ws-federation/_static/WsFederationButton.png)</span></span>

<span data-ttu-id="2f668-162">Mit AD FS als Anbieter, leitet die Schaltfläche mit der auf eine AD FS-Anmeldeseite: ![AD FS-Anmeldeseite](ws-federation/_static/AdfsLoginPage.png)</span><span class="sxs-lookup"><span data-stu-id="2f668-162">With ADFS as the provider, the button redirects to an ADFS sign-in page: ![ADFS sign-in page](ws-federation/_static/AdfsLoginPage.png)</span></span>

<span data-ttu-id="2f668-163">Mit Azure Active Directory als Anbieter, leitet die Schaltfläche zu einer Anmeldeseite für AAD: ![AAD-Anmeldeseite](ws-federation/_static/AadSignIn.png)</span><span class="sxs-lookup"><span data-stu-id="2f668-163">With Azure Active Directory as the provider, the button redirects to an AAD sign-in page: ![AAD sign-in page](ws-federation/_static/AadSignIn.png)</span></span>

<span data-ttu-id="2f668-164">Eine erfolgreiche Anmelden bei einem neuen Benutzer leitet an die app Benutzerregistrierungsseite: ![Seite "Register"](ws-federation/_static/Register.png)</span><span class="sxs-lookup"><span data-stu-id="2f668-164">A successful sign-in for a new user redirects to the app's user registration page: ![Register page](ws-federation/_static/Register.png)</span></span>

## <a name="use-ws-federation-without-aspnet-core-identity"></a><span data-ttu-id="2f668-165">Verwenden von WS-Verbund ohne ASP.NET Core Identität</span><span class="sxs-lookup"><span data-stu-id="2f668-165">Use WS-Federation without ASP.NET Core Identity</span></span>

<span data-ttu-id="2f668-166">Die WS-Verbund-Middleware kann ohne Identität verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="2f668-166">The WS-Federation middleware can be used without Identity.</span></span> <span data-ttu-id="2f668-167">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="2f668-167">For example:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddAuthentication(sharedOptions =>
    {
        sharedOptions.DefaultScheme = CookieAuthenticationDefaults.AuthenticationScheme;
        sharedOptions.DefaultSignInScheme = CookieAuthenticationDefaults.AuthenticationScheme;
        sharedOptions.DefaultChallengeScheme = WsFederationDefaults.AuthenticationScheme;
    })
    .AddWsFederation(options =>
    {
        options.Wtrealm = Configuration["wsfed:realm"];
        options.MetadataAddress = Configuration["wsfed:metadata"];
    })
    .AddCookie();
}

public void Configure(IApplicationBuilder app)
{
    app.UseAuthentication();
        // …
}
```
