---
title: "Konfigurieren von ASP.NET Core Identität"
author: AdrienTorris
description: "Verstehen Sie die ASP.NET Core Identity Standardwerte, und konfigurieren Sie die verschiedenen Identitätseigenschaften, um benutzerdefinierte Werte verwenden."
keywords: "ASP.NET Core, Identität und Authentifizierung, Sicherheit"
ms.author: scaddie
manager: wpickett
ms.date: 09/18/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authentication/identity-configuration
ms.openlocfilehash: adf577ae1e1c752c3b1a332ec94a7a7627a7a4b4
ms.sourcegitcommit: 76d42f09f3e0dd2f2105493eca6b29994aa47706
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/19/2017
---
# <a name="configure-identity"></a>Konfigurieren von Identität

ASP.NET Core Identität hat einige Standardverhaltensweisen, die Sie in der Anwendungsverzeichnis leicht überschreiben können `Startup` Klasse.

## <a name="passwords-policy"></a>Kennwörter-Richtlinie

Standardmäßig erfordert Identität an, dass Kennwörter ein Großbuchstabe, Kleinbuchstabe, eine Ziffer und ein alphanumerisches Zeichen enthalten. Es gibt auch einige andere Einschränkungen. Wenn Sie kennworteinschränkungen vereinfachen möchten, Sie können dies tun, der `Startup` Ihrer Anwendungsklasse.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

ASP.NET Core 2.0 hinzugefügt der `RequiredUniqueChars` Eigenschaft. Andernfalls die Optionen sind von ASP.NET Core 1.x.

[!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-37,50-52)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

[!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Startup.cs?range=58-65,84)]

---

`IdentityOptions.Password`hat die folgenden Eigenschaften:
* `RequireDigit`: Eine Zahl zwischen 0-9, das Kennwort erfordert. Der Standardwert ist "true".
* `RequiredLength`: Die Mindestlänge des Kennworts. Der Standardwert ist 6.
* `RequireNonAlphanumeric`: Ein nicht alphanumerisches Zeichen im Kennwort erfordert. Der Standardwert ist "true".
* `RequireUppercase`: Ein Großbuchstaben Zeichen im Kennwort erfordert. Der Standardwert ist "true".
* `RequireLowercase`: Das Kennwort ein Kleinbuchstabe erforderlich. Der Standardwert ist "true".
* `RequiredUniqueChars`: Die Anzahl der unterschiedlichen Zeichen im Kennwort erfordert. Der Standardwert ist 1.


## <a name="users-lockout"></a>Benutzersperre

[!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-30,39-42,50-52)]

`IdentityOptions.Lockout`hat die folgenden Eigenschaften:
* `DefaultLockoutTimeSpan`: Die Zeitspanne ist ein Benutzer gesperrt, wenn eine Sperre auftritt. Der Standardwert ist 5 Minuten.
* `MaxFailedAccessAttempts`: Die Anzahl fehlerhafter Zugriffsversuche, bis ein Benutzer gesperrt ist, wenn die Sperre aktiviert ist. Der Standardwert ist 5.
* `AllowedForNewUsers`: Wird bestimmt, ob ein neuer Benutzer gesperrt werden kann. Der Standardwert ist "true".


## <a name="sign-in-settings"></a>Anmeldeeinstellungen

[!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-30,44-46,50-52)]

`IdentityOptions.SignIn`hat die folgenden Eigenschaften:
* `RequireConfirmedEmail`: Erfordert eine bestätigte e-Mail, sich anzumelden. Der Standardwert ist "false".
* `RequireConfirmedPhoneNumber`: Erfordert eine bestätigte Telefonnummer, sich anzumelden. Der Standardwert ist "false".


## <a name="user-validation-settings"></a>Überprüfung von benutzereinstellungen

[!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-30,48-52)]

`IdentityOptions.User`hat die folgenden Eigenschaften:
* `RequireUniqueEmail`: Muss jeder Benutzer eine eindeutige e-Mail-Adresse haben. Der Standardwert ist "false".
* `AllowedUserNameCharacters`: Zulässigen Zeichen des Benutzernamens. Der Standardwert ist abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789-._@+.

## <a name="applications-cookie-settings"></a>Cookie-Einstellungen der Anwendung

Alle Einstellungen des Cookies für die Anwendung können z. B. die Richtlinie Kennwörter geändert werden die `Startup` Klasse.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Unter `ConfigureServices` in der `Startup` -Klasse, Sie können die Anwendung Cookie konfigurieren.

[!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?name=snippet_configurecookie)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

[!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Startup.cs?range=58-59,72-80,84)]

--- 

`CookieAuthenticationOptions`hat die folgenden Eigenschaften:
* `Cookie.Name`: Der Name des Cookies. Der Standardwert ist. AspNetCore.Cookies.
* `Cookie.HttpOnly`: Bei "true", ist das Cookie nicht aus einer clientseitigen Skripts zugegriffen werden kann. Der Standardwert ist "true".
* `ExpireTimeSpan`: Steuert, wie viel Zeit das Authentifizierungsticket im Cookie gespeichert ab dem Punkt gültig bleibt, die er erstellt wird. Der Standardwert ist 14 Tage.
* `LoginPath`: Wenn ein Benutzer nicht autorisiert ist, werden er auf diesen Pfad für die Anmeldung umgeleitet. Der Standardwert ist/Account/Anmeldung.
* `LogoutPath`: Wenn ein Benutzer abgemeldet ist, werden er auf diesen Pfad umgeleitet. Der Standardwert ist/Account/Abmeldung.
* `AccessDeniedPath`: Wenn ein Benutzer mit einer autorisierungsprüfung fehlschlägt, werden er auf diesen Pfad umgeleitet. Der Standardwert ist/Account/AccessDenied.
* `SlidingExpiration`: Bei "true", wird ein neues Cookie mit einer neuen Ablaufzeit ausgegeben werden, wenn das aktuelle Cookie mehr als genau, über das Fenster "Ablaufdatum" liegt. Der Standardwert ist "true".
* `ReturnUrlParameter`: Die ReturnUrlParameter bestimmt den Namen des Abfragezeichenfolgen-Parameters das von der Middleware angefügt wird, wenn ein Statuscode "401 nicht autorisiert" in eine 302-Umleitung Anmeldepfad geändert wird.
* `AuthenticationScheme`: Dies ist nur für ASP.NET Core 1.x. Der logische Name für ein bestimmtes Authentifizierungsschema.
* `AutomaticAuthenticate`: Dieses Flag ist nur relevant für ASP.NET Core 1.x. Bei "true", sollte Cookieauthentifizierung für jede Anforderung ausgeführt und versuchen, zu überprüfen und rekonstruieren serialisierten Prinzipal, der Sie erstellt.

