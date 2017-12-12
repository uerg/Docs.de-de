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
ms.openlocfilehash: 2861ca474e7e82da81943966394a92040ce96ab8
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
# <a name="configure-identity"></a><span data-ttu-id="5a4c6-104">Konfigurieren von Identität</span><span class="sxs-lookup"><span data-stu-id="5a4c6-104">Configure Identity</span></span>

<span data-ttu-id="5a4c6-105">ASP.NET Core Identität hat einige Standardverhaltensweisen, die Sie in der Anwendungsverzeichnis leicht überschreiben können `Startup` Klasse.</span><span class="sxs-lookup"><span data-stu-id="5a4c6-105">ASP.NET Core Identity has some default behaviors that you can override easily in your application's `Startup` class.</span></span>

## <a name="passwords-policy"></a><span data-ttu-id="5a4c6-106">Kennwörter-Richtlinie</span><span class="sxs-lookup"><span data-stu-id="5a4c6-106">Passwords policy</span></span>

<span data-ttu-id="5a4c6-107">Standardmäßig erfordert Identität an, dass Kennwörter ein Großbuchstabe, Kleinbuchstabe, eine Ziffer und ein nicht alphanumerisches Zeichen enthalten.</span><span class="sxs-lookup"><span data-stu-id="5a4c6-107">By default, Identity requires that passwords contain an uppercase character, lowercase character, a digit, and a non-alphanumeric character.</span></span> <span data-ttu-id="5a4c6-108">Es gibt auch einige andere Einschränkungen.</span><span class="sxs-lookup"><span data-stu-id="5a4c6-108">There are also some other restrictions.</span></span> <span data-ttu-id="5a4c6-109">Wenn Sie kennworteinschränkungen vereinfachen möchten, Sie können dies tun, der `Startup` Ihrer Anwendungsklasse.</span><span class="sxs-lookup"><span data-stu-id="5a4c6-109">If you want to simplify password restrictions, you can do that in the `Startup` class of your application.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="5a4c6-110">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="5a4c6-110">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="5a4c6-111">ASP.NET Core 2.0 hinzugefügt der `RequiredUniqueChars` Eigenschaft.</span><span class="sxs-lookup"><span data-stu-id="5a4c6-111">ASP.NET Core 2.0 added the `RequiredUniqueChars` property.</span></span> <span data-ttu-id="5a4c6-112">Andernfalls die Optionen sind von ASP.NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="5a4c6-112">Otherwise, the options are the same from ASP.NET Core 1.x.</span></span>

[!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-37,50-52)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="5a4c6-113">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="5a4c6-113">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

[!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Startup.cs?range=58-65,84)]

---

<span data-ttu-id="5a4c6-114">`IdentityOptions.Password`hat die folgenden Eigenschaften:</span><span class="sxs-lookup"><span data-stu-id="5a4c6-114">`IdentityOptions.Password` has the following properties:</span></span>
* <span data-ttu-id="5a4c6-115">`RequireDigit`: Eine Zahl zwischen 0-9, das Kennwort erfordert.</span><span class="sxs-lookup"><span data-stu-id="5a4c6-115">`RequireDigit`: Requires a number between 0-9 in the password.</span></span> <span data-ttu-id="5a4c6-116">Der Standardwert ist "true".</span><span class="sxs-lookup"><span data-stu-id="5a4c6-116">Defaults to true.</span></span>
* <span data-ttu-id="5a4c6-117">`RequiredLength`: Die Mindestlänge des Kennworts.</span><span class="sxs-lookup"><span data-stu-id="5a4c6-117">`RequiredLength`: The minimum length of the password.</span></span> <span data-ttu-id="5a4c6-118">Der Standardwert ist 6.</span><span class="sxs-lookup"><span data-stu-id="5a4c6-118">Defaults to 6.</span></span>
* <span data-ttu-id="5a4c6-119">`RequireNonAlphanumeric`: Ein nicht alphanumerisches Zeichen im Kennwort erfordert.</span><span class="sxs-lookup"><span data-stu-id="5a4c6-119">`RequireNonAlphanumeric`: Requires a non-alphanumeric character in the password.</span></span> <span data-ttu-id="5a4c6-120">Der Standardwert ist "true".</span><span class="sxs-lookup"><span data-stu-id="5a4c6-120">Defaults to true.</span></span>
* <span data-ttu-id="5a4c6-121">`RequireUppercase`: Ein Großbuchstaben Zeichen im Kennwort erfordert.</span><span class="sxs-lookup"><span data-stu-id="5a4c6-121">`RequireUppercase`: Requires an upper case character in the password.</span></span> <span data-ttu-id="5a4c6-122">Der Standardwert ist "true".</span><span class="sxs-lookup"><span data-stu-id="5a4c6-122">Defaults to true.</span></span>
* <span data-ttu-id="5a4c6-123">`RequireLowercase`: Das Kennwort ein Kleinbuchstabe erforderlich.</span><span class="sxs-lookup"><span data-stu-id="5a4c6-123">`RequireLowercase`: Requires a lower case character in the password.</span></span> <span data-ttu-id="5a4c6-124">Der Standardwert ist "true".</span><span class="sxs-lookup"><span data-stu-id="5a4c6-124">Defaults to true.</span></span>
* <span data-ttu-id="5a4c6-125">`RequiredUniqueChars`: Die Anzahl der unterschiedlichen Zeichen im Kennwort erfordert.</span><span class="sxs-lookup"><span data-stu-id="5a4c6-125">`RequiredUniqueChars`: Requires the number of distinct characters in the password.</span></span> <span data-ttu-id="5a4c6-126">Der Standardwert ist 1.</span><span class="sxs-lookup"><span data-stu-id="5a4c6-126">Defaults to 1.</span></span>


## <a name="users-lockout"></a><span data-ttu-id="5a4c6-127">Benutzersperre</span><span class="sxs-lookup"><span data-stu-id="5a4c6-127">User's lockout</span></span>

[!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-30,39-42,50-52)]

<span data-ttu-id="5a4c6-128">`IdentityOptions.Lockout`hat die folgenden Eigenschaften:</span><span class="sxs-lookup"><span data-stu-id="5a4c6-128">`IdentityOptions.Lockout` has the following properties:</span></span>
* <span data-ttu-id="5a4c6-129">`DefaultLockoutTimeSpan`: Die Zeitspanne ist ein Benutzer gesperrt, wenn eine Sperre auftritt.</span><span class="sxs-lookup"><span data-stu-id="5a4c6-129">`DefaultLockoutTimeSpan`: The amount of time a user is locked out when a lockout occurs.</span></span> <span data-ttu-id="5a4c6-130">Der Standardwert ist 5 Minuten.</span><span class="sxs-lookup"><span data-stu-id="5a4c6-130">Defaults to 5 minutes.</span></span>
* <span data-ttu-id="5a4c6-131">`MaxFailedAccessAttempts`: Die Anzahl fehlerhafter Zugriffsversuche, bis ein Benutzer gesperrt ist, wenn die Sperre aktiviert ist.</span><span class="sxs-lookup"><span data-stu-id="5a4c6-131">`MaxFailedAccessAttempts`: The number of failed access attempts until a user is locked out, if lockout is enabled.</span></span> <span data-ttu-id="5a4c6-132">Der Standardwert ist 5.</span><span class="sxs-lookup"><span data-stu-id="5a4c6-132">Defaults to 5.</span></span>
* <span data-ttu-id="5a4c6-133">`AllowedForNewUsers`: Wird bestimmt, ob ein neuer Benutzer gesperrt werden kann. Der Standardwert ist "true".</span><span class="sxs-lookup"><span data-stu-id="5a4c6-133">`AllowedForNewUsers`: Determines if a new user can be locked out. Defaults to true.</span></span>


## <a name="sign-in-settings"></a><span data-ttu-id="5a4c6-134">Anmeldeeinstellungen</span><span class="sxs-lookup"><span data-stu-id="5a4c6-134">Sign in settings</span></span>

[!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-30,44-46,50-52)]

<span data-ttu-id="5a4c6-135">`IdentityOptions.SignIn`hat die folgenden Eigenschaften:</span><span class="sxs-lookup"><span data-stu-id="5a4c6-135">`IdentityOptions.SignIn` has the following properties:</span></span>
* <span data-ttu-id="5a4c6-136">`RequireConfirmedEmail`: Erfordert eine bestätigte e-Mail, sich anzumelden.</span><span class="sxs-lookup"><span data-stu-id="5a4c6-136">`RequireConfirmedEmail`: Requires a confirmed email to sign in.</span></span> <span data-ttu-id="5a4c6-137">Der Standardwert ist "false".</span><span class="sxs-lookup"><span data-stu-id="5a4c6-137">Defaults to false.</span></span>
* <span data-ttu-id="5a4c6-138">`RequireConfirmedPhoneNumber`: Erfordert eine bestätigte Telefonnummer, sich anzumelden.</span><span class="sxs-lookup"><span data-stu-id="5a4c6-138">`RequireConfirmedPhoneNumber`: Requires a confirmed phone number to sign in.</span></span> <span data-ttu-id="5a4c6-139">Der Standardwert ist "false".</span><span class="sxs-lookup"><span data-stu-id="5a4c6-139">Defaults to false.</span></span>


## <a name="user-validation-settings"></a><span data-ttu-id="5a4c6-140">Überprüfung von benutzereinstellungen</span><span class="sxs-lookup"><span data-stu-id="5a4c6-140">User validation settings</span></span>

[!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-30,48-52)]

<span data-ttu-id="5a4c6-141">`IdentityOptions.User`hat die folgenden Eigenschaften:</span><span class="sxs-lookup"><span data-stu-id="5a4c6-141">`IdentityOptions.User` has the following properties:</span></span>
* <span data-ttu-id="5a4c6-142">`RequireUniqueEmail`: Muss jeder Benutzer eine eindeutige e-Mail-Adresse haben.</span><span class="sxs-lookup"><span data-stu-id="5a4c6-142">`RequireUniqueEmail`: Requires each User to have a unique email.</span></span> <span data-ttu-id="5a4c6-143">Der Standardwert ist "false".</span><span class="sxs-lookup"><span data-stu-id="5a4c6-143">Defaults to false.</span></span>
* <span data-ttu-id="5a4c6-144">`AllowedUserNameCharacters`: Zulässigen Zeichen des Benutzernamens.</span><span class="sxs-lookup"><span data-stu-id="5a4c6-144">`AllowedUserNameCharacters`: Allowed characters in the username.</span></span> <span data-ttu-id="5a4c6-145">Der Standardwert ist abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789-._@+.</span><span class="sxs-lookup"><span data-stu-id="5a4c6-145">Defaults to abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789-._@+.</span></span>

## <a name="applications-cookie-settings"></a><span data-ttu-id="5a4c6-146">Cookie-Einstellungen der Anwendung</span><span class="sxs-lookup"><span data-stu-id="5a4c6-146">Application's cookie settings</span></span>

<span data-ttu-id="5a4c6-147">Alle Einstellungen des Cookies für die Anwendung können z. B. die Richtlinie Kennwörter geändert werden die `Startup` Klasse.</span><span class="sxs-lookup"><span data-stu-id="5a4c6-147">Like the passwords policy, all the settings of the application's cookie can be changed in the `Startup` class.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="5a4c6-148">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="5a4c6-148">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="5a4c6-149">Unter `ConfigureServices` in der `Startup` -Klasse, Sie können die Anwendung Cookie konfigurieren.</span><span class="sxs-lookup"><span data-stu-id="5a4c6-149">Under `ConfigureServices` in the `Startup` class, you can configure the application's cookie.</span></span>

[!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?name=snippet_configurecookie)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="5a4c6-150">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="5a4c6-150">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

[!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Startup.cs?range=58-59,72-80,84)]

--- 

<span data-ttu-id="5a4c6-151">`CookieAuthenticationOptions`hat die folgenden Eigenschaften:</span><span class="sxs-lookup"><span data-stu-id="5a4c6-151">`CookieAuthenticationOptions` has the following properties:</span></span>
* <span data-ttu-id="5a4c6-152">`Cookie.Name`: Der Name des Cookies.</span><span class="sxs-lookup"><span data-stu-id="5a4c6-152">`Cookie.Name`: The name of the cookie.</span></span> <span data-ttu-id="5a4c6-153">Der Standardwert ist. AspNetCore.Cookies.</span><span class="sxs-lookup"><span data-stu-id="5a4c6-153">Defaults to .AspNetCore.Cookies.</span></span>
* <span data-ttu-id="5a4c6-154">`Cookie.HttpOnly`: Bei "true", ist das Cookie nicht aus einer clientseitigen Skripts zugegriffen werden kann.</span><span class="sxs-lookup"><span data-stu-id="5a4c6-154">`Cookie.HttpOnly`: When true, the cookie is not accessible from client-side scripts.</span></span> <span data-ttu-id="5a4c6-155">Der Standardwert ist "true".</span><span class="sxs-lookup"><span data-stu-id="5a4c6-155">Defaults to true.</span></span>
* <span data-ttu-id="5a4c6-156">`ExpireTimeSpan`: Steuert, wie viel Zeit das Authentifizierungsticket im Cookie gespeichert ab dem Punkt gültig bleibt, die er erstellt wird.</span><span class="sxs-lookup"><span data-stu-id="5a4c6-156">`ExpireTimeSpan`: Controls how much time the authentication ticket stored in the cookie will remain valid from the point it is created.</span></span> <span data-ttu-id="5a4c6-157">Der Standardwert ist 14 Tage.</span><span class="sxs-lookup"><span data-stu-id="5a4c6-157">Defaults to 14 days.</span></span>
* <span data-ttu-id="5a4c6-158">`LoginPath`: Wenn ein Benutzer nicht autorisiert ist, werden er auf diesen Pfad für die Anmeldung umgeleitet.</span><span class="sxs-lookup"><span data-stu-id="5a4c6-158">`LoginPath`: When a user is unauthorized, they will be redirected to this path to login.</span></span> <span data-ttu-id="5a4c6-159">Der Standardwert ist/Account/Anmeldung.</span><span class="sxs-lookup"><span data-stu-id="5a4c6-159">Defaults to /Account/Login.</span></span>
* <span data-ttu-id="5a4c6-160">`LogoutPath`: Wenn ein Benutzer abgemeldet ist, werden er auf diesen Pfad umgeleitet.</span><span class="sxs-lookup"><span data-stu-id="5a4c6-160">`LogoutPath`: When a user is logged out, they will be redirected to this path.</span></span> <span data-ttu-id="5a4c6-161">Der Standardwert ist/Account/Abmeldung.</span><span class="sxs-lookup"><span data-stu-id="5a4c6-161">Defaults to /Account/Logout.</span></span>
* <span data-ttu-id="5a4c6-162">`AccessDeniedPath`: Wenn ein Benutzer mit einer autorisierungsprüfung fehlschlägt, werden er auf diesen Pfad umgeleitet.</span><span class="sxs-lookup"><span data-stu-id="5a4c6-162">`AccessDeniedPath`: When a user fails an authorization check, they will be redirected to this path.</span></span> <span data-ttu-id="5a4c6-163">Der Standardwert ist/Account/AccessDenied.</span><span class="sxs-lookup"><span data-stu-id="5a4c6-163">Defaults to /Account/AccessDenied.</span></span>
* <span data-ttu-id="5a4c6-164">`SlidingExpiration`: Bei "true", wird ein neues Cookie mit einer neuen Ablaufzeit ausgegeben werden, wenn das aktuelle Cookie mehr als genau, über das Fenster "Ablaufdatum" liegt.</span><span class="sxs-lookup"><span data-stu-id="5a4c6-164">`SlidingExpiration`: When true, a new cookie will be issued with a new expiration time when the current cookie is more than halfway through the expiration window.</span></span> <span data-ttu-id="5a4c6-165">Der Standardwert ist "true".</span><span class="sxs-lookup"><span data-stu-id="5a4c6-165">Defaults to true.</span></span>
* <span data-ttu-id="5a4c6-166">`ReturnUrlParameter`: Die ReturnUrlParameter bestimmt den Namen des Abfragezeichenfolgen-Parameters das von der Middleware angefügt wird, wenn ein Statuscode "401 nicht autorisiert" in eine 302-Umleitung Anmeldepfad geändert wird.</span><span class="sxs-lookup"><span data-stu-id="5a4c6-166">`ReturnUrlParameter`: The ReturnUrlParameter determines the name of the query string parameter which is appended by the middleware when a 401 Unauthorized status code is changed to a 302 redirect onto the login path.</span></span>
* <span data-ttu-id="5a4c6-167">`AuthenticationScheme`: Dies ist nur für ASP.NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="5a4c6-167">`AuthenticationScheme`: This is only relevant for ASP.NET Core 1.x.</span></span> <span data-ttu-id="5a4c6-168">Der logische Name für ein bestimmtes Authentifizierungsschema.</span><span class="sxs-lookup"><span data-stu-id="5a4c6-168">The logical name for a particular authentication scheme.</span></span>
* <span data-ttu-id="5a4c6-169">`AutomaticAuthenticate`: Dieses Flag ist nur relevant für ASP.NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="5a4c6-169">`AutomaticAuthenticate`: This flag is only relevant for ASP.NET Core 1.x.</span></span> <span data-ttu-id="5a4c6-170">Bei "true", sollte Cookieauthentifizierung für jede Anforderung ausgeführt und versuchen, zu überprüfen und rekonstruieren serialisierten Prinzipal, der Sie erstellt.</span><span class="sxs-lookup"><span data-stu-id="5a4c6-170">When true, cookie authentication should run on every request and attempt to validate and reconstruct any serialized principal it created.</span></span>

