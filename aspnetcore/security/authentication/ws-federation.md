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
---
# <a name="authenticate-users-with-ws-federation-in-aspnet-core"></a>Authentifizieren von Benutzern mit WS-Verbund in ASP.NET Core

Dieses Lernprogramm veranschaulicht, wie Benutzer zur Anmeldung mit eines WS-Verbund-Authentifizierungsanbieter wie Active Directory Federation Services (ADFS) oder [Azure Active Directory](/azure/active-directory/) (AAD). Er verwendet die ASP.NET Core 2.0 Beispiel-app, die in beschriebenen [Facebook, Google, und die externe Anbieter Authentifizierung](xref:security/authentication/social/index).

Nähere Informationen zu ASP.NET Core 2.0 apps WS-Verbund-Unterstützung wird durch bereitgestellt [Microsoft.AspNetCore.Authentication.WsFederation](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.WsFederation). Diese Komponente wird von portiert [Microsoft.Owin.Security.WsFederation](https://www.nuget.org/packages/Microsoft.Owin.Security.WsFederation) und teilen sich viele Mechanismen für diese Komponente. Allerdings unterscheiden sich die Komponenten, in einigen wichtigen Aspekten zu erhalten.

Standardmäßig ist die neue Middleware:

* Keine unerwünschte Anmeldungen zu ermöglichen. Diese Funktion von der WS-Verbund-Protokoll ist anfällig für XSRF-Angriffen. Dies kann jedoch aktiviert werden, mit der `AllowUnsolicitedLogins` Option.
* Jedes Formular-Post in Anmeldenachrichten nicht überprüft werden. Nur Anforderungen an die `CallbackPath` Anmeldung Ins überprüft werden `CallbackPath` standardmäßig `/signin-wsfed` kann jedoch geändert werden. Dieser Pfad kann mit anderen Authentifizierungsanbieter freigegeben werden, durch Aktivieren der `SkipUnrecognizedRequests` Option.

## <a name="register-the-app-with-active-directory"></a>Registrieren einer app in Active Directory

### <a name="active-directory-federation-services"></a>Active Directory-Verbunddienste

* Öffnen Sie den Server **der vertrauenden Seite Partei vertrauen Assistenten zum Hinzufügen von** über die AD FS-Verwaltungskonsole:

![Hinzufügen der vertrauenden Seite Vertrauensstellung Assistenten: Willkommen](ws-federation/_static/AdfsAddTrust.png)

* Wählen Sie Daten manuell eingeben:

![Fügen Sie der vertrauenden Seite Assistent für Vertrauensstellung: Auswählen einer Datenquelle](ws-federation/_static/AdfsSelectDataSource.png)

* Geben Sie einen Anzeigenamen für die vertrauende Seite. Der Name ist nicht wichtig, die ASP.NET Core-app.

* [Microsoft.AspNetCore.Authentication.WsFederation](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.WsFederation) lacks support for token encryption, so don't configure a token encryption certificate:

![Fügen Sie der vertrauenden Seite Assistent für Vertrauensstellung: Zertifikat konfigurieren](ws-federation/_static/AdfsConfigureCert.png)

* Aktivieren der Unterstützung für passiven WS-Verbund-Protokoll, mit der app-URL. Stellen Sie sicher, dass der Port für die app richtig ist:

![Hinzufügen Assistent für Vertrauensstellung der vertrauenden Seite an: Konfigurieren von URL](ws-federation/_static/AdfsConfigureUrl.png)

> [!NOTE]
> Dies muss eine HTTPS-URL sein. IIS Express können ein selbstsigniertes Zertifikat bereitstellen, wenn Sie die app während der Entwicklung zu hosten. Kestrel ist die manuelle Konfiguration erforderlich. Finden Sie unter der [Kestrel Dokumentation](xref:fundamentals/servers/kestrel) Weitere Details.

* Klicken Sie auf **Weiter** über den Assistenten ab und **schließen** am Ende.

* ASP.NET Core Identität erfordert eine **Namensbezeichner** Anspruch. Fügen Sie einen aus der **Edit Claim Rules** Dialogfeld:

![Anspruchsregeln bearbeiten](ws-federation/_static/EditClaimRules.png)

* In der **transformieren Anspruch Assistent zum Hinzufügen einer**, übernehmen Sie den Standardport **LDAP-Attribute als Ansprüche senden** Vorlage ausgewählt, und klicken Sie auf **Weiter**. Fügen Sie eine Regel Zuordnung der **SAM-Kontoname** LDAP-Attribut, um die **namens-ID** ausgehenden Anspruchs:

![Transformation Anspruch Assistent zum Hinzufügen von: Konfigurieren Sie Anspruchsregel](ws-federation/_static/AddTransformClaimRule.png)

* Klicken Sie auf **Fertig stellen** > **OK** in der **Edit Claim Rules** Fenster.

### <a name="azure-active-directory"></a>Azure Active Directory

* Navigieren Sie zu der AAD-Mandanten app Registrierungen Blatt. Klicken Sie auf **neue anwendungsregistrierung**:

![Azure Active Directory: App-Registrierungen](ws-federation/_static/AadNewAppRegistration.png)

* Geben Sie einen Namen für die app-Registrierung aus. Dies ist wichtig, die ASP.NET Core-app nicht.
* Geben Sie die URL die app auf wie lauscht die **Anmelde-URL**:

![Azure Active Directory: Erstellen Sie app-Registrierung.](ws-federation/_static/AadCreateAppRegistration.png)

* Klicken Sie auf **Endpunkte** und notieren Sie sich die **Verbundmetadatendokument** URL. Dies ist der WS-Verbund-Middleware `MetadataAddress`:

![Azure Active Directory: Endpunkte](ws-federation/_static/AadFederationMetadataDocument.png)

* Navigieren Sie zu der neuen app-Registrierung. Klicken Sie auf **Einstellungen** > **Eigenschaften** und notieren Sie sich die **App ID-URI**. Dies ist der WS-Verbund-Middleware `Wtrealm`:

![Azure Active Directory: App-Registrierungseigenschaften](ws-federation/_static/AadAppIdUri.png)

## <a name="add-ws-federation-as-an-external-login-provider-for-aspnet-core-identity"></a>Hinzufügen von WS-Verbund als einen externen Anmeldeanbieter für ASP.NET Core Identität

* Fügen Sie eine Abhängigkeit auf [Microsoft.AspNetCore.Authentication.WsFederation](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.WsFederation) zum Projekt.
* Hinzufügen von WS-Verbund, um die `Configure` Methode in *Startup.cs*:

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

### <a name="log-in-with-ws-federation"></a>Melden Sie sich mit WS-Verbund

Navigieren Sie zur app, und klicken Sie auf die **melden Sie sich** Link im Nav-Header. Es ist eine Option aus, um die Anmeldung WsFederation: ![-Anmeldeseite](ws-federation/_static/WsFederationButton.png)

Mit AD FS als Anbieter, leitet die Schaltfläche mit der auf eine AD FS-Anmeldeseite: ![AD FS-Anmeldeseite](ws-federation/_static/AdfsLoginPage.png)

Mit Azure Active Directory als Anbieter, leitet die Schaltfläche zu einer Anmeldeseite für AAD: ![AAD-Anmeldeseite](ws-federation/_static/AadSignIn.png)

Eine erfolgreiche Anmelden bei einem neuen Benutzer leitet an die app Benutzerregistrierungsseite: ![Seite "Register"](ws-federation/_static/Register.png)

## <a name="use-ws-federation-without-aspnet-core-identity"></a>Verwenden von WS-Verbund ohne ASP.NET Core Identität

Die WS-Verbund-Middleware kann ohne Identität verwendet werden. Zum Beispiel:

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
