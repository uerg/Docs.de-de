---
title: "Konfigurieren von ASP.NET Core Identität"
author: AdrienTorris
description: "Verstehen Sie die ASP.NET Core Identity Standardwerte, und konfigurieren Sie die verschiedenen Identitätseigenschaften, um benutzerdefinierte Werte verwenden."
manager: wpickett
ms.author: scaddie
ms.date: 01/11/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authentication/identity-configuration
ms.openlocfilehash: 0ec223ce06ff116c36182b8de507138e96a277a4
ms.sourcegitcommit: d43c84c4c80527c85e49d53691b293669557a79d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/20/2018
---
# <a name="configure-identity"></a>Konfigurieren von Identität

ASP.NET Core Identität hat allgemeine Verhalten in Clientanwendungen wie z. B. Kennwortrichtlinie, die Dauer der Sperrung und cookieeinstellungen, die Sie in der Anwendungsverzeichnis leicht überschreiben können `Startup` Klasse.

## <a name="passwords-policy"></a>Kennwörter-Richtlinie

Standardmäßig erfordert Identität an, dass Kennwörter ein Großbuchstabe, Kleinbuchstabe, eine Ziffer und ein nicht alphanumerisches Zeichen enthalten. Es gibt auch einige andere Einschränkungen. Zur Vereinfachung der kennworteinschränkungen Ändern der `ConfigureServices` Methode von der `Startup` Ihrer Anwendungsklasse.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

ASP.NET Core 2.0 hinzugefügt der `RequiredUniqueChars` Eigenschaft. Andernfalls die Optionen sind von ASP.NET Core 1.x.

[!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-37,50-52)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

[!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Startup.cs?range=58-65,84)]

---

`IdentityOptions.Password` hat die folgenden Eigenschaften:

| Eigenschaft                | Beschreibung                       | Standard |
| ----------------------- | --------------------------------- | ------- |
| `RequireDigit`          | Erfordert eine Zahl zwischen 0-9, das Kennwort an. | true |
| `RequiredLength`        | Die Mindestlänge des Kennworts. | 6 |
| `RequireNonAlphanumeric`| Ist ein nicht alphanumerisches Zeichen im Kennwort erforderlich. | true |
| `RequireUppercase`      | Erfordert ein Großbuchstaben Zeichen im Kennwort. | true |
| `RequireLowercase`      | Ist das Kennwort ein Kleinbuchstabe erforderlich. | true |
| `RequiredUniqueChars`   | Ist die Anzahl der unterschiedlichen Zeichen im Kennwort erforderlich. | 1 |


## <a name="users-lockout"></a>Benutzersperre

[!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-30,39-42,50-52)]

`IdentityOptions.Lockout` hat die folgenden Eigenschaften:

| Eigenschaft                | Beschreibung                       | Standard |
| ----------------------- | --------------------------------- | ------- |
| `DefaultLockoutTimeSpan` | Die Zeitdauer ist ein Benutzer gesperrt, wenn eine Sperre auftritt.  | 5 Minuten  |
| `MaxFailedAccessAttempts` | Die Anzahl fehlerhafter Zugriffsversuche, bis ein Benutzer gesperrt ist, wenn die Sperre aktiviert ist.  | 5 |
| `AllowedForNewUsers` | Bestimmt, ob ein neuer Benutzer gesperrt werden kann.  | true |

## <a name="sign-in-settings"></a>Anmeldeeinstellungen

[!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-30,44-46,50-52)]

`IdentityOptions.SignIn` hat die folgenden Eigenschaften:

| Eigenschaft                | Beschreibung                       | Standard |
| ----------------------- | --------------------------------- | ------- |
| `RequireConfirmedEmail` | Erfordert eine bestätigte e-Mail, sich anzumelden. | False  |
| `RequireConfirmedPhoneNumber` |  Erfordert eine bestätigte Telefonnummer, sich anzumelden. | False  |

## <a name="user-validation-settings"></a>Überprüfung von benutzereinstellungen

[!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-30,48-52)]

`IdentityOptions.User` hat die folgenden Eigenschaften:

| Eigenschaft                | Beschreibung                       | Standard |
| ----------------------- | --------------------------------- | ------- |
| `RequireUniqueEmail`  | Erfordert eine eindeutige e-Mail-Adresse für jeden Benutzer. | False  |
| `AllowedUserNameCharacters`  | Zulässige Zeichen des Benutzernamens. | abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789-._@+ |


## <a name="applications-cookie-settings"></a>Cookie-Einstellungen der Anwendung

Alle Einstellungen des Cookies für die Anwendung können z. B. die Richtlinie Kennwörter geändert werden die `Startup` Klasse.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Unter `ConfigureServices` in der `Startup` -Klasse, Sie können die Anwendung Cookie konfigurieren.

[!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?name=snippet_configurecookie)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

[!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Startup.cs?range=58-59,72-80,84)]

---

`CookieAuthenticationOptions` hat die folgenden Eigenschaften:

| Eigenschaft                | Beschreibung                       | Standard |
| ----------------------- | --------------------------------- | ------- |
| `Cookie.Name`  | Der Name des Cookies.  | .AspNetCore.Cookies.  |
| `Cookie.HttpOnly`  | Bei "true", nicht das Cookie aus einer clientseitigen Skripts zugegriffen werden.  |  true |
| `ExpireTimeSpan`  | Steuert, wie viel Zeit das Authentifizierungsticket im Cookie gespeichert ab dem Punkt gültig bleibt, die er erstellt wird.  | 14 Tage  |
| `LoginPath`  | Wenn ein Benutzer nicht autorisiert ist, werden er auf diesen Pfad für die Anmeldung umgeleitet. | /Account/Login  |
| `LogoutPath`  | Wenn ein Benutzer abgemeldet ist, werden er auf diesen Pfad umgeleitet.  | /Account/Logout  |
| `AccessDeniedPath`  | Wenn ein Benutzer mit einer autorisierungsprüfung fehlschlägt, werden er auf diesen Pfad umgeleitet.  |  /Account/AccessDenied |
| `SlidingExpiration`  | Bei "true", wird ein neues Cookie mit einer neuen Ablaufzeit ausgegeben werden, wenn das aktuelle Cookie mehr als genau, über das Fenster "Ablaufdatum" liegt.  | true |
| `ReturnUrlParameter`  | Legt den Namen des Abfragezeichenfolgen-Parameters das von der Middleware angefügt wird, wenn ein Statuscode "401 nicht autorisiert" in eine 302-Umleitung Anmeldepfad geändert wird.  | ReturnUrl |
| `AuthenticationScheme`  | Dies ist nur für ASP.NET Core 1.x. Der logische Name für ein bestimmtes Authentifizierungsschema. |  |
| `AutomaticAuthenticate`  | Dieses Flag ist nur relevant für ASP.NET Core 1.x. Bei "true", sollte Cookieauthentifizierung für jede Anforderung ausgeführt und versuchen, zu überprüfen und rekonstruieren serialisierten Prinzipal, der Sie erstellt.  |  |
