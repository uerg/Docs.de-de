---
title: "Konfigurieren von ASP.NET Core Identität"
author: AdrienTorris
description: "Verstehen Sie die ASP.NET Core Identity Standardwerte, und konfigurieren Sie die verschiedenen Identitätseigenschaften, um benutzerdefinierte Werte verwenden."
keywords: "ASP.NET Core, Identität und Authentifizierung, Sicherheit"
ms.author: scaddie
manager: wpickett
ms.date: 01/11/2018
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authentication/identity-configuration
ms.openlocfilehash: ac204cb89aac1f90adc64c4f0bec4e946cb8c4d9
ms.sourcegitcommit: 12e5194936b7e820efc5505a2d5d4f84e88eb5ef
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2018
---
# <a name="configure-identity"></a><span data-ttu-id="fbdc7-104">Konfigurieren von Identität</span><span class="sxs-lookup"><span data-stu-id="fbdc7-104">Configure Identity</span></span>

<span data-ttu-id="fbdc7-105">ASP.NET Core Identität hat allgemeine Verhalten in Clientanwendungen wie z. B. Kennwortrichtlinie, die Dauer der Sperrung und cookieeinstellungen, die Sie in der Anwendungsverzeichnis leicht überschreiben können `Startup` Klasse.</span><span class="sxs-lookup"><span data-stu-id="fbdc7-105">ASP.NET Core Identity has common behaviors in applications such as password policy, lockout time, and cookie settings that you can override easily in your application's `Startup` class.</span></span>

## <a name="passwords-policy"></a><span data-ttu-id="fbdc7-106">Kennwörter-Richtlinie</span><span class="sxs-lookup"><span data-stu-id="fbdc7-106">Passwords policy</span></span>

<span data-ttu-id="fbdc7-107">Standardmäßig erfordert Identität an, dass Kennwörter ein Großbuchstabe, Kleinbuchstabe, eine Ziffer und ein nicht alphanumerisches Zeichen enthalten.</span><span class="sxs-lookup"><span data-stu-id="fbdc7-107">By default, Identity requires that passwords contain an uppercase character, lowercase character, a digit, and a non-alphanumeric character.</span></span> <span data-ttu-id="fbdc7-108">Es gibt auch einige andere Einschränkungen.</span><span class="sxs-lookup"><span data-stu-id="fbdc7-108">There are also some other restrictions.</span></span> <span data-ttu-id="fbdc7-109">Zur Vereinfachung der kennworteinschränkungen Ändern der `ConfigureServices` Methode von der `Startup` Ihrer Anwendungsklasse.</span><span class="sxs-lookup"><span data-stu-id="fbdc7-109">To simplify password restrictions, modify the `ConfigureServices` method of the `Startup` class of your application.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="fbdc7-110">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="fbdc7-110">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="fbdc7-111">ASP.NET Core 2.0 hinzugefügt der `RequiredUniqueChars` Eigenschaft.</span><span class="sxs-lookup"><span data-stu-id="fbdc7-111">ASP.NET Core 2.0 added the `RequiredUniqueChars` property.</span></span> <span data-ttu-id="fbdc7-112">Andernfalls die Optionen sind von ASP.NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="fbdc7-112">Otherwise, the options are the same from ASP.NET Core 1.x.</span></span>

[!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-37,50-52)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="fbdc7-113">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="fbdc7-113">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

[!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Startup.cs?range=58-65,84)]

---

<span data-ttu-id="fbdc7-114">`IdentityOptions.Password`hat die folgenden Eigenschaften:</span><span class="sxs-lookup"><span data-stu-id="fbdc7-114">`IdentityOptions.Password` has the following properties:</span></span>

| <span data-ttu-id="fbdc7-115">Eigenschaft</span><span class="sxs-lookup"><span data-stu-id="fbdc7-115">Property</span></span>                | <span data-ttu-id="fbdc7-116">Beschreibung</span><span class="sxs-lookup"><span data-stu-id="fbdc7-116">Description</span></span>                       | <span data-ttu-id="fbdc7-117">Standard</span><span class="sxs-lookup"><span data-stu-id="fbdc7-117">Default</span></span> |
| ----------------------- | --------------------------------- | ------- |
| `RequireDigit`          | <span data-ttu-id="fbdc7-118">Erfordert eine Zahl zwischen 0-9, das Kennwort an.</span><span class="sxs-lookup"><span data-stu-id="fbdc7-118">Requires a number between 0-9 in the password.</span></span> | <span data-ttu-id="fbdc7-119">true</span><span class="sxs-lookup"><span data-stu-id="fbdc7-119">true</span></span> |
| `RequiredLength`        | <span data-ttu-id="fbdc7-120">Die Mindestlänge des Kennworts.</span><span class="sxs-lookup"><span data-stu-id="fbdc7-120">The minimum length of the password.</span></span> | <span data-ttu-id="fbdc7-121">6</span><span class="sxs-lookup"><span data-stu-id="fbdc7-121">6</span></span> |
| `RequireNonAlphanumeric`| <span data-ttu-id="fbdc7-122">Ist ein nicht alphanumerisches Zeichen im Kennwort erforderlich.</span><span class="sxs-lookup"><span data-stu-id="fbdc7-122">Requires a non-alphanumeric character in the password.</span></span> | <span data-ttu-id="fbdc7-123">true</span><span class="sxs-lookup"><span data-stu-id="fbdc7-123">true</span></span> |
| `RequireUppercase`      | <span data-ttu-id="fbdc7-124">Erfordert ein Großbuchstaben Zeichen im Kennwort.</span><span class="sxs-lookup"><span data-stu-id="fbdc7-124">Requires an upper case character in the password.</span></span> | <span data-ttu-id="fbdc7-125">true</span><span class="sxs-lookup"><span data-stu-id="fbdc7-125">true</span></span> |
| `RequireLowercase`      | <span data-ttu-id="fbdc7-126">Ist das Kennwort ein Kleinbuchstabe erforderlich.</span><span class="sxs-lookup"><span data-stu-id="fbdc7-126">Requires a lower case character in the password.</span></span> | <span data-ttu-id="fbdc7-127">true</span><span class="sxs-lookup"><span data-stu-id="fbdc7-127">true</span></span> |
| `RequiredUniqueChars`   | <span data-ttu-id="fbdc7-128">Ist die Anzahl der unterschiedlichen Zeichen im Kennwort erforderlich.</span><span class="sxs-lookup"><span data-stu-id="fbdc7-128">Requires the number of distinct characters in the password.</span></span> | <span data-ttu-id="fbdc7-129">1</span><span class="sxs-lookup"><span data-stu-id="fbdc7-129">1</span></span> |


## <a name="users-lockout"></a><span data-ttu-id="fbdc7-130">Benutzersperre</span><span class="sxs-lookup"><span data-stu-id="fbdc7-130">User's lockout</span></span>

[!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-30,39-42,50-52)]

<span data-ttu-id="fbdc7-131">`IdentityOptions.Lockout`hat die folgenden Eigenschaften:</span><span class="sxs-lookup"><span data-stu-id="fbdc7-131">`IdentityOptions.Lockout` has the following properties:</span></span>

| <span data-ttu-id="fbdc7-132">Eigenschaft</span><span class="sxs-lookup"><span data-stu-id="fbdc7-132">Property</span></span>                | <span data-ttu-id="fbdc7-133">Beschreibung</span><span class="sxs-lookup"><span data-stu-id="fbdc7-133">Description</span></span>                       | <span data-ttu-id="fbdc7-134">Standard</span><span class="sxs-lookup"><span data-stu-id="fbdc7-134">Default</span></span> |
| ----------------------- | --------------------------------- | ------- |
| `DefaultLockoutTimeSpan` | <span data-ttu-id="fbdc7-135">Die Zeitdauer ist ein Benutzer gesperrt, wenn eine Sperre auftritt.</span><span class="sxs-lookup"><span data-stu-id="fbdc7-135">The amount of time a user is locked out when a lockout occurs.</span></span>  | <span data-ttu-id="fbdc7-136">5 Minuten</span><span class="sxs-lookup"><span data-stu-id="fbdc7-136">5 minutes</span></span>  |
| `MaxFailedAccessAttempts` | <span data-ttu-id="fbdc7-137">Die Anzahl fehlerhafter Zugriffsversuche, bis ein Benutzer gesperrt ist, wenn die Sperre aktiviert ist.</span><span class="sxs-lookup"><span data-stu-id="fbdc7-137">The number of failed access attempts until a user is locked out, if lockout is enabled.</span></span>  | <span data-ttu-id="fbdc7-138">5</span><span class="sxs-lookup"><span data-stu-id="fbdc7-138">5</span></span> |
| `AllowedForNewUsers` | <span data-ttu-id="fbdc7-139">Bestimmt, ob ein neuer Benutzer gesperrt werden kann.</span><span class="sxs-lookup"><span data-stu-id="fbdc7-139">Determines if a new user can be locked out.</span></span>  | <span data-ttu-id="fbdc7-140">true</span><span class="sxs-lookup"><span data-stu-id="fbdc7-140">true</span></span> |

## <a name="sign-in-settings"></a><span data-ttu-id="fbdc7-141">Anmeldeeinstellungen</span><span class="sxs-lookup"><span data-stu-id="fbdc7-141">Sign in settings</span></span>

[!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-30,44-46,50-52)]

<span data-ttu-id="fbdc7-142">`IdentityOptions.SignIn`hat die folgenden Eigenschaften:</span><span class="sxs-lookup"><span data-stu-id="fbdc7-142">`IdentityOptions.SignIn` has the following properties:</span></span>

| <span data-ttu-id="fbdc7-143">Eigenschaft</span><span class="sxs-lookup"><span data-stu-id="fbdc7-143">Property</span></span>                | <span data-ttu-id="fbdc7-144">Beschreibung</span><span class="sxs-lookup"><span data-stu-id="fbdc7-144">Description</span></span>                       | <span data-ttu-id="fbdc7-145">Standard</span><span class="sxs-lookup"><span data-stu-id="fbdc7-145">Default</span></span> |
| ----------------------- | --------------------------------- | ------- |
| `RequireConfirmedEmail` | <span data-ttu-id="fbdc7-146">Erfordert eine bestätigte e-Mail, sich anzumelden.</span><span class="sxs-lookup"><span data-stu-id="fbdc7-146">Requires a confirmed email to sign in.</span></span> | <span data-ttu-id="fbdc7-147">False</span><span class="sxs-lookup"><span data-stu-id="fbdc7-147">false</span></span>  |
| `RequireConfirmedPhoneNumber` |  <span data-ttu-id="fbdc7-148">Erfordert eine bestätigte Telefonnummer, sich anzumelden.</span><span class="sxs-lookup"><span data-stu-id="fbdc7-148">Requires a confirmed phone number to sign in.</span></span> | <span data-ttu-id="fbdc7-149">False</span><span class="sxs-lookup"><span data-stu-id="fbdc7-149">false</span></span>  |

## <a name="user-validation-settings"></a><span data-ttu-id="fbdc7-150">Überprüfung von benutzereinstellungen</span><span class="sxs-lookup"><span data-stu-id="fbdc7-150">User validation settings</span></span>

[!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-30,48-52)]

<span data-ttu-id="fbdc7-151">`IdentityOptions.User`hat die folgenden Eigenschaften:</span><span class="sxs-lookup"><span data-stu-id="fbdc7-151">`IdentityOptions.User` has the following properties:</span></span>

| <span data-ttu-id="fbdc7-152">Eigenschaft</span><span class="sxs-lookup"><span data-stu-id="fbdc7-152">Property</span></span>                | <span data-ttu-id="fbdc7-153">Beschreibung</span><span class="sxs-lookup"><span data-stu-id="fbdc7-153">Description</span></span>                       | <span data-ttu-id="fbdc7-154">Standard</span><span class="sxs-lookup"><span data-stu-id="fbdc7-154">Default</span></span> |
| ----------------------- | --------------------------------- | ------- |
| `RequireUniqueEmail`  | <span data-ttu-id="fbdc7-155">Erfordert eine eindeutige e-Mail-Adresse für jeden Benutzer.</span><span class="sxs-lookup"><span data-stu-id="fbdc7-155">Requires each User to have a unique email.</span></span> | <span data-ttu-id="fbdc7-156">False</span><span class="sxs-lookup"><span data-stu-id="fbdc7-156">false</span></span>  |
| `AllowedUserNameCharacters`  | <span data-ttu-id="fbdc7-157">Zulässige Zeichen des Benutzernamens.</span><span class="sxs-lookup"><span data-stu-id="fbdc7-157">Allowed characters in the username.</span></span> | <span data-ttu-id="fbdc7-158">abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789-._@+</span><span class="sxs-lookup"><span data-stu-id="fbdc7-158">abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789-._@+</span></span> |


## <a name="applications-cookie-settings"></a><span data-ttu-id="fbdc7-159">Cookie-Einstellungen der Anwendung</span><span class="sxs-lookup"><span data-stu-id="fbdc7-159">Application's cookie settings</span></span>

<span data-ttu-id="fbdc7-160">Alle Einstellungen des Cookies für die Anwendung können z. B. die Richtlinie Kennwörter geändert werden die `Startup` Klasse.</span><span class="sxs-lookup"><span data-stu-id="fbdc7-160">Like the passwords policy, all the settings of the application's cookie can be changed in the `Startup` class.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="fbdc7-161">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="fbdc7-161">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="fbdc7-162">Unter `ConfigureServices` in der `Startup` -Klasse, Sie können die Anwendung Cookie konfigurieren.</span><span class="sxs-lookup"><span data-stu-id="fbdc7-162">Under `ConfigureServices` in the `Startup` class, you can configure the application's cookie.</span></span>

[!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?name=snippet_configurecookie)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="fbdc7-163">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="fbdc7-163">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

[!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Startup.cs?range=58-59,72-80,84)]

---

<span data-ttu-id="fbdc7-164">`CookieAuthenticationOptions`hat die folgenden Eigenschaften:</span><span class="sxs-lookup"><span data-stu-id="fbdc7-164">`CookieAuthenticationOptions` has the following properties:</span></span>

| <span data-ttu-id="fbdc7-165">Eigenschaft</span><span class="sxs-lookup"><span data-stu-id="fbdc7-165">Property</span></span>                | <span data-ttu-id="fbdc7-166">Beschreibung</span><span class="sxs-lookup"><span data-stu-id="fbdc7-166">Description</span></span>                       | <span data-ttu-id="fbdc7-167">Standard</span><span class="sxs-lookup"><span data-stu-id="fbdc7-167">Default</span></span> |
| ----------------------- | --------------------------------- | ------- |
| `Cookie.Name`  | <span data-ttu-id="fbdc7-168">Der Name des Cookies.</span><span class="sxs-lookup"><span data-stu-id="fbdc7-168">The name of the cookie.</span></span>  | <span data-ttu-id="fbdc7-169">. AspNetCore.Cookies.</span><span class="sxs-lookup"><span data-stu-id="fbdc7-169">.AspNetCore.Cookies.</span></span>  |
| `Cookie.HttpOnly`  | <span data-ttu-id="fbdc7-170">Bei "true", ist das Cookie nicht aus einer clientseitigen Skripts zugegriffen werden kann.</span><span class="sxs-lookup"><span data-stu-id="fbdc7-170">When true, the cookie is not accessible from client-side scripts.</span></span>  |  <span data-ttu-id="fbdc7-171">true</span><span class="sxs-lookup"><span data-stu-id="fbdc7-171">true</span></span> |
| `ExpireTimeSpan`  | <span data-ttu-id="fbdc7-172">Steuert, wie viel Zeit das Authentifizierungsticket im Cookie gespeichert ab dem Punkt gültig bleibt, die er erstellt wird.</span><span class="sxs-lookup"><span data-stu-id="fbdc7-172">Controls how much time the authentication ticket stored in the cookie will remain valid from the point it is created.</span></span>  | <span data-ttu-id="fbdc7-173">14 Tage</span><span class="sxs-lookup"><span data-stu-id="fbdc7-173">14 days</span></span>  |
| `LoginPath`  | <span data-ttu-id="fbdc7-174">Wenn ein Benutzer nicht autorisiert ist, werden er auf diesen Pfad für die Anmeldung umgeleitet.</span><span class="sxs-lookup"><span data-stu-id="fbdc7-174">When a user is unauthorized, they will be redirected to this path to login.</span></span> | <span data-ttu-id="fbdc7-175">/ /-Kontoanmeldung</span><span class="sxs-lookup"><span data-stu-id="fbdc7-175">/Account/Login</span></span>  |
| `LogoutPath`  | <span data-ttu-id="fbdc7-176">Wenn ein Benutzer abgemeldet ist, werden er auf diesen Pfad umgeleitet.</span><span class="sxs-lookup"><span data-stu-id="fbdc7-176">When a user is logged out, they will be redirected to this path.</span></span>  | <span data-ttu-id="fbdc7-177">/ Account/Abmeldung</span><span class="sxs-lookup"><span data-stu-id="fbdc7-177">/Account/Logout</span></span>  |
| `AccessDeniedPath`  | <span data-ttu-id="fbdc7-178">Wenn ein Benutzer mit einer autorisierungsprüfung fehlschlägt, werden er auf diesen Pfad umgeleitet.</span><span class="sxs-lookup"><span data-stu-id="fbdc7-178">When a user fails an authorization check, they will be redirected to this path.</span></span>  |   |
| `SlidingExpiration`  | <span data-ttu-id="fbdc7-179">Bei "true", wird ein neues Cookie mit einer neuen Ablaufzeit ausgegeben werden, wenn das aktuelle Cookie mehr als genau, über das Fenster "Ablaufdatum" liegt.</span><span class="sxs-lookup"><span data-stu-id="fbdc7-179">When true, a new cookie will be issued with a new expiration time when the current cookie is more than halfway through the expiration window.</span></span>  | <span data-ttu-id="fbdc7-180">/ Account/AccessDenied</span><span class="sxs-lookup"><span data-stu-id="fbdc7-180">/Account/AccessDenied</span></span> |
| `ReturnUrlParameter`  | <span data-ttu-id="fbdc7-181">Legt den Namen des Abfragezeichenfolgen-Parameters das von der Middleware angefügt wird, wenn ein Statuscode "401 nicht autorisiert" in eine 302-Umleitung Anmeldepfad geändert wird.</span><span class="sxs-lookup"><span data-stu-id="fbdc7-181">Determines the name of the query string parameter which is appended by the middleware when a 401 Unauthorized status code is changed to a 302 redirect onto the login path.</span></span>  |  <span data-ttu-id="fbdc7-182">true</span><span class="sxs-lookup"><span data-stu-id="fbdc7-182">true</span></span> |
| `AuthenticationScheme`  | <span data-ttu-id="fbdc7-183">Dies ist nur für ASP.NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="fbdc7-183">This is only relevant for ASP.NET Core 1.x.</span></span> <span data-ttu-id="fbdc7-184">Der logische Name für ein bestimmtes Authentifizierungsschema.</span><span class="sxs-lookup"><span data-stu-id="fbdc7-184">The logical name for a particular authentication scheme.</span></span> |  |
| `AutomaticAuthenticate`  | <span data-ttu-id="fbdc7-185">Dieses Flag ist nur relevant für ASP.NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="fbdc7-185">This flag is only relevant for ASP.NET Core 1.x.</span></span> <span data-ttu-id="fbdc7-186">Bei "true", sollte Cookieauthentifizierung für jede Anforderung ausgeführt und versuchen, zu überprüfen und rekonstruieren serialisierten Prinzipal, der Sie erstellt.</span><span class="sxs-lookup"><span data-stu-id="fbdc7-186">When true, cookie authentication should run on every request and attempt to validate and reconstruct any serialized principal it created.</span></span>  |  |