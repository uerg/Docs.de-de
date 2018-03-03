---
title: "Konfigurieren von ASP.NET Core Identität"
author: AdrienTorris
description: "Grundlegendes zu ASP.NET Core Identity-Standardwerten, und erfahren Sie, wie so konfigurieren Sie die Identitätseigenschaften, um benutzerdefinierte Werte verwenden."
manager: wpickett
ms.author: scaddie
ms.date: 02/21/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authentication/identity-configuration
ms.openlocfilehash: 6aeb85063b4b6f97822062b523a0c1f7ee6b595c
ms.sourcegitcommit: 7ac15eaae20b6d70e65f3650af050a7880115cbf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/02/2018
---
# <a name="configure-identity"></a>Konfigurieren von Identität

ASP.NET Core Identität verwendet die Standardkonfiguration für Einstellungen wie z. B. Kennwortrichtlinie, die Dauer der Sperrung und Cookie-Einstellungen. Diese Einstellung können überschrieben werden, in der app `Startup` Klasse.

## <a name="identity-options"></a>Identity-Optionen

Die [IdentityOptions](/dotnet/api/microsoft.aspnetcore.identity.identityoptions) Klasse stellt die Optionen, die zum Konfigurieren des Systems Identität verwendet werden können.

### <a name="claims-identity"></a>Forderungsidentität

[IdentityOptions.ClaimsIdentity](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.claimsidentity) gibt an, die [ClaimsIdentityOptions](/dotnet/api/microsoft.aspnetcore.identity.claimsidentityoptions) mit den Eigenschaften, die in der Tabelle dargestellt.

| Eigenschaft | Beschreibung | Standard |
| -------- | ----------- | :-----: |
| [RoleClaimType](/dotnet/api/microsoft.aspnetcore.identity.claimsidentityoptions.roleclaimtype) | Ruft ab oder legt den für einen Rollenanspruch verwendeten Anspruchstyp. | [ClaimTypes.Role](/dotnet/api/system.security.claims.claimtypes.role) |
| [SecurityStampClaimType](/dotnet/api/microsoft.aspnetcore.identity.claimsidentityoptions.securitystampclaimtype) | Abrufen oder Festlegen der für den Stempel sicherheitsanspruch verwendeten Anspruchstyp. | `AspNet.Identity.SecurityStamp` |
| [UserIdClaimType](/dotnet/api/microsoft.aspnetcore.identity.claimsidentityoptions.useridclaimtype) | Ruft ab oder legt den für den Benutzer-ID-Anspruch verwendeten Anspruchstyp. | [ClaimTypes.NameIdentifier](/dotnet/api/system.security.claims.claimtypes.nameidentifier) |
| [UserNameClaimType](/dotnet/api/microsoft.aspnetcore.identity.claimsidentityoptions.usernameclaimtype) | Ruft ab, oder legt ihn fest für den Benutzer Name-Anspruch verwendeten Anspruchstyp. | [ClaimTypes.Name](/dotnet/api/system.security.claims.claimtypes.name) |

### <a name="lockout"></a>Sperre

[!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-30,39-42,50-52)]

[IdentityOptions.Lockout](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.lockout) gibt an, die [LockoutOptions](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions) mit den Eigenschaften, die in der Tabelle dargestellt.

| Eigenschaft | Beschreibung | Standard |
| -------- | ----------- | :-----: |
| [AllowedForNewUsers](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions.allowedfornewusers) | Bestimmt, ob ein neuer Benutzer gesperrt werden kann. | `true` |
| [DefaultLockoutTimeSpan](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions.defaultlockouttimespan) | Die Zeitdauer ist ein Benutzer gesperrt, wenn eine Sperre auftritt. | 5 Minuten |
| [MaxFailedAccessAttempts](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions.maxfailedaccessattempts) | Die Anzahl fehlerhafter Zugriffsversuche, bis ein Benutzer gesperrt ist, wenn die Sperre aktiviert ist. | 5 |

### <a name="password"></a>Kennwort

Standardmäßig erfordert Identität an, dass Kennwörter ein Großbuchstabe, Kleinbuchstabe, eine Ziffer und ein nicht alphanumerisches Zeichen enthalten. Kennwörter müssen mindestens sechs Zeichen lang sein. [PasswordOptions](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions) kann geändert werden, `Startup.ConfigureServices`.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

ASP.NET Core 2.0 hinzugefügt der [RequiredUniqueChars](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requireduniquechars) Eigenschaft. Andernfalls die Optionen sind identisch mit ASP.NET Core 1.x.

[!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-37,50-52)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

[!code-csharp[](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Startup.cs?range=58-65,84)]

---

[IdentityOptions.Password](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.password) gibt an, die [PasswordOptions](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions) mit den Eigenschaften, die in der Tabelle dargestellt.

| Eigenschaft | Beschreibung | Standard |
| -------- | ----------- | :-----: |
| [RequireDigit](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requiredigit) | Erfordert eine Zahl zwischen 0-9, das Kennwort an. | `true` |
| [RequiredLength](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requiredlength) | Die Mindestlänge des Kennworts. | 6 |
| [RequiredUniqueChars](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requireduniquechars) | Gilt nur für ASP.NET Core 2.0 oder höher.<br><br> Ist die Anzahl der unterschiedlichen Zeichen im Kennwort erforderlich. | 1 |
| [RequireLowercase](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requirelowercase) | Erfordert ein Kleinbuchstabe, das Kennwort an. | `true` |
| [RequireNonAlphanumeric](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requirenonalphanumeric) | Ist ein nicht alphanumerisches Zeichen im Kennwort erforderlich. | `true` |
| [RequireUppercase](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requireuppercase) | Erfordert einen Großbuchstaben in das Kennwort an. | `true` |

### <a name="sign-in"></a>Anmelden

[!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-30,44-46,50-52)]

[IdentityOptions.SignIn](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.signin) gibt an, die [SignInOptions](/dotnet/api/microsoft.aspnetcore.identity.signinoptions) mit den Eigenschaften, die in der Tabelle dargestellt.

| Eigenschaft | Beschreibung | Standard |
| -------- | ----------- | :-----: |
| [RequireConfirmedEmail](/dotnet/api/microsoft.aspnetcore.identity.signinoptions.requireconfirmedemail) | Erfordert eine bestätigte e-Mail, sich anzumelden. | `false` |
| [RequireConfirmedPhoneNumber](/dotnet/api/microsoft.aspnetcore.identity.signinoptions.requireconfirmedphonenumber) | Erfordert eine bestätigte Telefonnummer, sich anzumelden. | `false` |

### <a name="tokens"></a>tokens

[IdentityOptions.Tokens](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.tokens) gibt an, die [TokenOptions](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions) mit den Eigenschaften, die in der Tabelle dargestellt.

| Eigenschaft | Beschreibung |
| -------- | ----------- |
| [AuthenticatorTokenProvider](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions.authenticatortokenprovider) | Ruft ab oder legt den `AuthenticatorTokenProvider` zum Überprüfen einer zweistufigen Anmeldungen mit einem Authentifikator verwendet. |
| [ChangeEmailTokenProvider](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions.changeemailtokenprovider) | Ruft ab oder legt die `ChangeEmailTokenProvider` verwendet zum Generieren von Token in e-Mails Change-Bestätigung verwendet. |
| [ChangePhoneNumberTokenProvider](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions.changephonenumbertokenprovider) | Ruft ab oder legt die `ChangePhoneNumberTokenProvider` verwendet zum Generieren von Tokens verwendet, wenn die Telefonnummern ändern. |
| [EmailConfirmationTokenProvider](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions.emailconfirmationtokenprovider) | Ermittelt oder definiert den Tokenanbieter, der zum Generieren von Tokens verwendet Konto Bestätigung-e-Mails verwendet. |
| [PasswordResetTokenProvider](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions.passwordresettokenprovider) | Ruft ab oder legt die [IUserTwoFactorTokenProvider<TUser> ](/dotnet/api/microsoft.aspnetcore.identity.iusertwofactortokenprovider-1) zum Generieren von Tokens verwendet, das Kennwort zurücksetzen-e-Mails verwendet. |
| [ProviderMap](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions.providermap) | Zum Erstellen einer [Anbieter von Sicherheitstoken](/dotnet/api/microsoft.aspnetcore.identity.tokenproviderdescriptor) mit dem Schlüssel, der als Name für den Anbieter verwendet. |

### <a name="user"></a>Benutzer

[!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-30,48-52)]

[IdentityOptions.User](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.user) gibt an, die [UserOptions](/dotnet/api/microsoft.aspnetcore.identity.useroptions) mit den Eigenschaften, die in der Tabelle dargestellt.

| Eigenschaft | Beschreibung | Standard |
| -------- | ----------- | :-----: |
| [AllowedUserNameCharacters](/dotnet/api/microsoft.aspnetcore.identity.useroptions.allowedusernamecharacters) | Zulässige Zeichen des Benutzernamens. | abcdefghijklmnopqrstuvwxyz<br>ABCDEFGHIJKLMNOPQRSTUVWXYZ<br>0123456789<br>-._@+ |
| [RequireUniqueEmail](/dotnet/api/microsoft.aspnetcore.identity.useroptions.requireuniqueemail) | Erfordert eine eindeutige e-Mail-Adresse für jeden Benutzer. | `false` |

## <a name="cookie-settings"></a>Cookie-Einstellungen

Konfigurieren Sie die app in Cookie `Startup.ConfigureServices`:

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

[!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?name=snippet_configurecookie)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

[!code-csharp[](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Startup.cs?range=58-59,72-80,84)]

---

[CookieAuthenticationOptions](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions) hat die folgenden Eigenschaften:

| Eigenschaft | Beschreibung |
| -------- | ----------- |
| [AccessDeniedPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.accessdeniedpath) | Dem Handler teilt mit, dass es eine ausgehende ändern, sollten *"403 Forbidden"* Statuscode in einer *302-Umleitung* auf dem angegebenen Pfad.<br><br>Der Standardwert ist `/Account/AccessDenied`. |
| [AuthenticationScheme](/dotnet/api/microsoft.aspnetcore.builder.authenticationoptions.authenticationscheme) | Gilt nur für ASP.NET Core 1.x.<br><br> Der logische Name für ein bestimmtes Authentifizierungsschema. |
| [AutomaticAuthenticate](/dotnet/api/microsoft.aspnetcore.builder.authenticationoptions.automaticauthenticate) | Gilt nur für ASP.NET Core 1.x.<br><br> Bei "true", sollte Cookieauthentifizierung für jede Anforderung ausgeführt und versuchen, zu überprüfen und rekonstruieren serialisierten Prinzipal, der Sie erstellt. |
| [AutomaticChallenge](/dotnet/api/microsoft.aspnetcore.builder.authenticationoptions.automaticchallenge) | Gilt nur für ASP.NET Core 1.x.<br><br> Bei "true", behandelt die authentifizierungsmiddleware automatische Herausforderungen. Wenn "false", die authentifizierungsmiddleware nur Antworten, wenn dies ausdrücklich angegeben ändert die `AuthenticationScheme`. |
| [ClaimsIssuer](/dotnet/api/microsoft.aspnetcore.authentication.authenticationschemeoptions.claimsissuer) | Ruft ab oder legt den Aussteller, die für alle Ansprüche verwendet werden soll, die erstellt werden (geerbt von [AuthenticationSchemeOptions](/dotnet/api/microsoft.aspnetcore.authentication.authenticationschemeoptions)). |
| [Cookie.Domain](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.domain) | Die Domäne, der das Cookie zugeordnet werden soll. |
| [Cookie.Expiration](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.expiration) | Ruft ab oder legt die Lebensdauer von Cookies. |
| [Cookie.HttpOnly](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.httponly) | Gibt an, ob ein Cookie durch clientseitiges Skript zugegriffen werden kann.<br><br>Der Standardwert ist `true`. |
| [Cookie.Name](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.name) | Der Name des Cookies.<br><br>Der Standardwert ist `.AspNetCore.Cookies`. |
| [Cookie.Path](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.path) | Der Cookiepfad. |
| [Cookie.SameSite](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.samesite) | Die `SameSite` Attribut des Cookies.<br><br>Der Standardwert ist [SameSiteMode.Lax](/dotnet/api/microsoft.aspnetcore.http.samesitemode). |
| [Cookie.SecurePolicy](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.securepolicy) | Die [CookieSecurePolicy](/dotnet/api/microsoft.aspnetcore.http.cookiesecurepolicy) Konfiguration.<br><br>Der Standardwert ist [CookieSecurePolicy.SameAsRequest](/dotnet/api/microsoft.aspnetcore.http.cookiesecurepolicy). |
| [CookieDomain](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.cookiedomain) | Gilt nur für ASP.NET Core 1.x.<br><br> Der Domänenname, in denen das Cookie bedient wird. |
| [CookieHttpOnly](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.cookiehttponly) | Gilt nur für ASP.NET Core 1.x.<br><br> Ein Flag, das angibt, ob das Cookie nur an Server zugegriffen werden soll.<br><br>Der Standardwert ist `true`. |
| [CookiePath](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.cookiepath) | Gilt nur für ASP.NET Core 1.x.<br><br> Zum Isolieren von apps auf dem gleichen Hostnamen verwendet. |
| [CookieSecure](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.cookiesecure) | Gilt nur für ASP.NET Core 1.x.<br><br> Ein Flag, das angibt, ob das Cookie auf HTTPS beschränkt werden soll (`CookieSecurePolicy.Always`), HTTP oder HTTPS (`CookieSecurePolicy.None`), oder das gleiche Protokoll wie die Anforderung (`CookieSecurePolicy.SameAsRequest`).<br><br>Der Standardwert ist `CookieSecurePolicy.SameAsRequest`. |
| [CookieManager](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.cookiemanager) | Die Komponente, die zum Abrufen von Cookies aus der Anforderung, oder legen Sie sie in der Antwort verwendet wird. | [ChunkingCookieManager](/dotnet/api/microsoft.aspnetcore.authentication.cookies.chunkingcookiemanager) |
| [DataProtectionProvider](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.dataprotectionprovider) | Wenn der Anbieter vom festgelegt, verwendet werden. die [CookieAuthenticationHandler](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationhandler) für den Datenschutz. |
| [Beschreibung](/dotnet/api/microsoft.aspnetcore.builder.authenticationoptions.description) | Gilt nur für ASP.NET Core 1.x.<br><br> Weitere Informationen zu den Authentifizierungstyp für die app zur Verfügung gestellt werden. |
| [Ereignisse](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.events) | Der Handler ruft Methoden für den Anbieter der app-Steuerelements an bestimmten Punkten ermöglichen, in denen Verarbeitung stattfindet. |
| [EventsType](/dotnet/api/microsoft.aspnetcore.authentication.authenticationschemeoptions.eventstype) | Wenn festgelegt, der Dienst geben Sie zum Abrufen der `Events` Instanz statt mit der Eigenschaft (geerbt von [AuthenticationSchemeOptions](/dotnet/api/microsoft.aspnetcore.authentication.authenticationschemeoptions)). |
| [ExpireTimeSpan](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.expiretimespan) | Steuerelemente Zeitpunkt, zu wie viel das Authentifizierungsticket in das Cookie bleibt gültig, an dem Punkt seiner Erstellung gespeichert.<br><br>Der Standardwert beträgt 14 Tage. |
| [LoginPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.loginpath) | Wenn ein Benutzer nicht autorisiert ist, sind sie auf diesen Pfad für die Anmeldung umgeleitet.<br><br>Der Standardwert ist `/Account/Login`. |
| [LogoutPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.logoutpath) | Wenn ein Benutzer abgemeldet ist, sind sie auf diesen Pfad umgeleitet.<br><br>Der Standardwert ist `/Account/Logout`. |
| [ReturnUrlParameter](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.returnurlparameter) | Legt den Namen des Abfragezeichenfolgen-Parameters die von der Middleware angefügt wird bei einem *401 nicht autorisiert* Statuscode wird geändert, um eine *302-Umleitung* Anmeldepfad.<br><br>Der Standardwert ist `ReturnUrl`. |
| [SessionStore](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.sessionstore) | Ein optionaler Container, in dem die Identität anforderungsübergreifend gespeichert werden soll. |
| [SlidingExpiration](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.slidingexpiration) | Bei "true", wird ein neues Cookie mit einer neuen Ablaufzeit ausgegeben, wenn das aktuelle Cookie mehr als genau, über das Fenster "Ablaufdatum" liegt.<br><br>Der Standardwert ist `true`. |
| [TicketDataFormat](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.ticketdataformat) | Die `TicketDataFormat` dient zum Schützen und Aufheben des Schutzes der Identität und andere Eigenschaften, die im Cookiewert gespeichert werden. |
