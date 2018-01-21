---
title: "Konfigurieren von ASP.NET Core Identität"
author: AdrienTorris
description: "Verstehen Sie die ASP.NET Core Identity Standardwerte, und konfigurieren Sie die verschiedenen Identitätseigenschaften, um benutzerdefinierte Werte verwenden."
ms.author: scaddie
manager: wpickett
ms.date: 01/11/2018
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authentication/identity-configuration
ms.openlocfilehash: d3a13d1cef3417522460b44c52c1361c3e9d1162
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/19/2018
---
# <a name="configure-identity"></a><span data-ttu-id="9130c-103">Konfigurieren von Identität</span><span class="sxs-lookup"><span data-stu-id="9130c-103">Configure Identity</span></span>

<span data-ttu-id="9130c-104">ASP.NET Core Identität hat allgemeine Verhalten in Clientanwendungen wie z. B. Kennwortrichtlinie, die Dauer der Sperrung und cookieeinstellungen, die Sie in der Anwendungsverzeichnis leicht überschreiben können `Startup` Klasse.</span><span class="sxs-lookup"><span data-stu-id="9130c-104">ASP.NET Core Identity has common behaviors in applications such as password policy, lockout time, and cookie settings that you can override easily in your application's `Startup` class.</span></span>

## <a name="passwords-policy"></a><span data-ttu-id="9130c-105">Kennwörter-Richtlinie</span><span class="sxs-lookup"><span data-stu-id="9130c-105">Passwords policy</span></span>

<span data-ttu-id="9130c-106">Standardmäßig erfordert Identität an, dass Kennwörter ein Großbuchstabe, Kleinbuchstabe, eine Ziffer und ein nicht alphanumerisches Zeichen enthalten.</span><span class="sxs-lookup"><span data-stu-id="9130c-106">By default, Identity requires that passwords contain an uppercase character, lowercase character, a digit, and a non-alphanumeric character.</span></span> <span data-ttu-id="9130c-107">Es gibt auch einige andere Einschränkungen.</span><span class="sxs-lookup"><span data-stu-id="9130c-107">There are also some other restrictions.</span></span> <span data-ttu-id="9130c-108">Zur Vereinfachung der kennworteinschränkungen Ändern der `ConfigureServices` Methode von der `Startup` Ihrer Anwendungsklasse.</span><span class="sxs-lookup"><span data-stu-id="9130c-108">To simplify password restrictions, modify the `ConfigureServices` method of the `Startup` class of your application.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="9130c-109">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="9130c-109">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="9130c-110">ASP.NET Core 2.0 hinzugefügt der `RequiredUniqueChars` Eigenschaft.</span><span class="sxs-lookup"><span data-stu-id="9130c-110">ASP.NET Core 2.0 added the `RequiredUniqueChars` property.</span></span> <span data-ttu-id="9130c-111">Andernfalls die Optionen sind von ASP.NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="9130c-111">Otherwise, the options are the same from ASP.NET Core 1.x.</span></span>

[!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-37,50-52)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="9130c-112">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="9130c-112">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

[!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Startup.cs?range=58-65,84)]

---

<span data-ttu-id="9130c-113">`IdentityOptions.Password`hat die folgenden Eigenschaften:</span><span class="sxs-lookup"><span data-stu-id="9130c-113">`IdentityOptions.Password` has the following properties:</span></span>

| <span data-ttu-id="9130c-114">Eigenschaft</span><span class="sxs-lookup"><span data-stu-id="9130c-114">Property</span></span>                | <span data-ttu-id="9130c-115">Beschreibung</span><span class="sxs-lookup"><span data-stu-id="9130c-115">Description</span></span>                       | <span data-ttu-id="9130c-116">Standard</span><span class="sxs-lookup"><span data-stu-id="9130c-116">Default</span></span> |
| ----------------------- | --------------------------------- | ------- |
| `RequireDigit`          | <span data-ttu-id="9130c-117">Erfordert eine Zahl zwischen 0-9, das Kennwort an.</span><span class="sxs-lookup"><span data-stu-id="9130c-117">Requires a number between 0-9 in the password.</span></span> | <span data-ttu-id="9130c-118">true</span><span class="sxs-lookup"><span data-stu-id="9130c-118">true</span></span> |
| `RequiredLength`        | <span data-ttu-id="9130c-119">Die Mindestlänge des Kennworts.</span><span class="sxs-lookup"><span data-stu-id="9130c-119">The minimum length of the password.</span></span> | <span data-ttu-id="9130c-120">6</span><span class="sxs-lookup"><span data-stu-id="9130c-120">6</span></span> |
| `RequireNonAlphanumeric`| <span data-ttu-id="9130c-121">Ist ein nicht alphanumerisches Zeichen im Kennwort erforderlich.</span><span class="sxs-lookup"><span data-stu-id="9130c-121">Requires a non-alphanumeric character in the password.</span></span> | <span data-ttu-id="9130c-122">true</span><span class="sxs-lookup"><span data-stu-id="9130c-122">true</span></span> |
| `RequireUppercase`      | <span data-ttu-id="9130c-123">Erfordert ein Großbuchstaben Zeichen im Kennwort.</span><span class="sxs-lookup"><span data-stu-id="9130c-123">Requires an upper case character in the password.</span></span> | <span data-ttu-id="9130c-124">true</span><span class="sxs-lookup"><span data-stu-id="9130c-124">true</span></span> |
| `RequireLowercase`      | <span data-ttu-id="9130c-125">Ist das Kennwort ein Kleinbuchstabe erforderlich.</span><span class="sxs-lookup"><span data-stu-id="9130c-125">Requires a lower case character in the password.</span></span> | <span data-ttu-id="9130c-126">true</span><span class="sxs-lookup"><span data-stu-id="9130c-126">true</span></span> |
| `RequiredUniqueChars`   | <span data-ttu-id="9130c-127">Ist die Anzahl der unterschiedlichen Zeichen im Kennwort erforderlich.</span><span class="sxs-lookup"><span data-stu-id="9130c-127">Requires the number of distinct characters in the password.</span></span> | <span data-ttu-id="9130c-128">1</span><span class="sxs-lookup"><span data-stu-id="9130c-128">1</span></span> |


## <a name="users-lockout"></a><span data-ttu-id="9130c-129">Benutzersperre</span><span class="sxs-lookup"><span data-stu-id="9130c-129">User's lockout</span></span>

[!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-30,39-42,50-52)]

<span data-ttu-id="9130c-130">`IdentityOptions.Lockout`hat die folgenden Eigenschaften:</span><span class="sxs-lookup"><span data-stu-id="9130c-130">`IdentityOptions.Lockout` has the following properties:</span></span>

| <span data-ttu-id="9130c-131">Eigenschaft</span><span class="sxs-lookup"><span data-stu-id="9130c-131">Property</span></span>                | <span data-ttu-id="9130c-132">Beschreibung</span><span class="sxs-lookup"><span data-stu-id="9130c-132">Description</span></span>                       | <span data-ttu-id="9130c-133">Standard</span><span class="sxs-lookup"><span data-stu-id="9130c-133">Default</span></span> |
| ----------------------- | --------------------------------- | ------- |
| `DefaultLockoutTimeSpan` | <span data-ttu-id="9130c-134">Die Zeitdauer ist ein Benutzer gesperrt, wenn eine Sperre auftritt.</span><span class="sxs-lookup"><span data-stu-id="9130c-134">The amount of time a user is locked out when a lockout occurs.</span></span>  | <span data-ttu-id="9130c-135">5 Minuten</span><span class="sxs-lookup"><span data-stu-id="9130c-135">5 minutes</span></span>  |
| `MaxFailedAccessAttempts` | <span data-ttu-id="9130c-136">Die Anzahl fehlerhafter Zugriffsversuche, bis ein Benutzer gesperrt ist, wenn die Sperre aktiviert ist.</span><span class="sxs-lookup"><span data-stu-id="9130c-136">The number of failed access attempts until a user is locked out, if lockout is enabled.</span></span>  | <span data-ttu-id="9130c-137">5</span><span class="sxs-lookup"><span data-stu-id="9130c-137">5</span></span> |
| `AllowedForNewUsers` | <span data-ttu-id="9130c-138">Bestimmt, ob ein neuer Benutzer gesperrt werden kann.</span><span class="sxs-lookup"><span data-stu-id="9130c-138">Determines if a new user can be locked out.</span></span>  | <span data-ttu-id="9130c-139">true</span><span class="sxs-lookup"><span data-stu-id="9130c-139">true</span></span> |

## <a name="sign-in-settings"></a><span data-ttu-id="9130c-140">Anmeldeeinstellungen</span><span class="sxs-lookup"><span data-stu-id="9130c-140">Sign in settings</span></span>

[!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-30,44-46,50-52)]

<span data-ttu-id="9130c-141">`IdentityOptions.SignIn`hat die folgenden Eigenschaften:</span><span class="sxs-lookup"><span data-stu-id="9130c-141">`IdentityOptions.SignIn` has the following properties:</span></span>

| <span data-ttu-id="9130c-142">Eigenschaft</span><span class="sxs-lookup"><span data-stu-id="9130c-142">Property</span></span>                | <span data-ttu-id="9130c-143">Beschreibung</span><span class="sxs-lookup"><span data-stu-id="9130c-143">Description</span></span>                       | <span data-ttu-id="9130c-144">Standard</span><span class="sxs-lookup"><span data-stu-id="9130c-144">Default</span></span> |
| ----------------------- | --------------------------------- | ------- |
| `RequireConfirmedEmail` | <span data-ttu-id="9130c-145">Erfordert eine bestätigte e-Mail, sich anzumelden.</span><span class="sxs-lookup"><span data-stu-id="9130c-145">Requires a confirmed email to sign in.</span></span> | <span data-ttu-id="9130c-146">False</span><span class="sxs-lookup"><span data-stu-id="9130c-146">false</span></span>  |
| `RequireConfirmedPhoneNumber` |  <span data-ttu-id="9130c-147">Erfordert eine bestätigte Telefonnummer, sich anzumelden.</span><span class="sxs-lookup"><span data-stu-id="9130c-147">Requires a confirmed phone number to sign in.</span></span> | <span data-ttu-id="9130c-148">False</span><span class="sxs-lookup"><span data-stu-id="9130c-148">false</span></span>  |

## <a name="user-validation-settings"></a><span data-ttu-id="9130c-149">Überprüfung von benutzereinstellungen</span><span class="sxs-lookup"><span data-stu-id="9130c-149">User validation settings</span></span>

[!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-30,48-52)]

<span data-ttu-id="9130c-150">`IdentityOptions.User`hat die folgenden Eigenschaften:</span><span class="sxs-lookup"><span data-stu-id="9130c-150">`IdentityOptions.User` has the following properties:</span></span>

| <span data-ttu-id="9130c-151">Eigenschaft</span><span class="sxs-lookup"><span data-stu-id="9130c-151">Property</span></span>                | <span data-ttu-id="9130c-152">Beschreibung</span><span class="sxs-lookup"><span data-stu-id="9130c-152">Description</span></span>                       | <span data-ttu-id="9130c-153">Standard</span><span class="sxs-lookup"><span data-stu-id="9130c-153">Default</span></span> |
| ----------------------- | --------------------------------- | ------- |
| `RequireUniqueEmail`  | <span data-ttu-id="9130c-154">Erfordert eine eindeutige e-Mail-Adresse für jeden Benutzer.</span><span class="sxs-lookup"><span data-stu-id="9130c-154">Requires each User to have a unique email.</span></span> | <span data-ttu-id="9130c-155">False</span><span class="sxs-lookup"><span data-stu-id="9130c-155">false</span></span>  |
| `AllowedUserNameCharacters`  | <span data-ttu-id="9130c-156">Zulässige Zeichen des Benutzernamens.</span><span class="sxs-lookup"><span data-stu-id="9130c-156">Allowed characters in the username.</span></span> | <span data-ttu-id="9130c-157">abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789-._@+</span><span class="sxs-lookup"><span data-stu-id="9130c-157">abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789-._@+</span></span> |


## <a name="applications-cookie-settings"></a><span data-ttu-id="9130c-158">Cookie-Einstellungen der Anwendung</span><span class="sxs-lookup"><span data-stu-id="9130c-158">Application's cookie settings</span></span>

<span data-ttu-id="9130c-159">Alle Einstellungen des Cookies für die Anwendung können z. B. die Richtlinie Kennwörter geändert werden die `Startup` Klasse.</span><span class="sxs-lookup"><span data-stu-id="9130c-159">Like the passwords policy, all the settings of the application's cookie can be changed in the `Startup` class.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="9130c-160">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="9130c-160">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="9130c-161">Unter `ConfigureServices` in der `Startup` -Klasse, Sie können die Anwendung Cookie konfigurieren.</span><span class="sxs-lookup"><span data-stu-id="9130c-161">Under `ConfigureServices` in the `Startup` class, you can configure the application's cookie.</span></span>

[!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?name=snippet_configurecookie)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="9130c-162">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="9130c-162">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

[!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Startup.cs?range=58-59,72-80,84)]

---

<span data-ttu-id="9130c-163">`CookieAuthenticationOptions`hat die folgenden Eigenschaften:</span><span class="sxs-lookup"><span data-stu-id="9130c-163">`CookieAuthenticationOptions` has the following properties:</span></span>

| <span data-ttu-id="9130c-164">Eigenschaft</span><span class="sxs-lookup"><span data-stu-id="9130c-164">Property</span></span>                | <span data-ttu-id="9130c-165">Beschreibung</span><span class="sxs-lookup"><span data-stu-id="9130c-165">Description</span></span>                       | <span data-ttu-id="9130c-166">Standard</span><span class="sxs-lookup"><span data-stu-id="9130c-166">Default</span></span> |
| ----------------------- | --------------------------------- | ------- |
| `Cookie.Name`  | <span data-ttu-id="9130c-167">Der Name des Cookies.</span><span class="sxs-lookup"><span data-stu-id="9130c-167">The name of the cookie.</span></span>  | <span data-ttu-id="9130c-168">.AspNetCore.Cookies.</span><span class="sxs-lookup"><span data-stu-id="9130c-168">.AspNetCore.Cookies.</span></span>  |
| `Cookie.HttpOnly`  | <span data-ttu-id="9130c-169">Bei "true", ist das Cookie nicht aus einer clientseitigen Skripts zugegriffen werden kann.</span><span class="sxs-lookup"><span data-stu-id="9130c-169">When true, the cookie is not accessible from client-side scripts.</span></span>  |  <span data-ttu-id="9130c-170">true</span><span class="sxs-lookup"><span data-stu-id="9130c-170">true</span></span> |
| `ExpireTimeSpan`  | <span data-ttu-id="9130c-171">Steuert, wie viel Zeit das Authentifizierungsticket im Cookie gespeichert ab dem Punkt gültig bleibt, die er erstellt wird.</span><span class="sxs-lookup"><span data-stu-id="9130c-171">Controls how much time the authentication ticket stored in the cookie will remain valid from the point it is created.</span></span>  | <span data-ttu-id="9130c-172">14 Tage</span><span class="sxs-lookup"><span data-stu-id="9130c-172">14 days</span></span>  |
| `LoginPath`  | <span data-ttu-id="9130c-173">Wenn ein Benutzer nicht autorisiert ist, werden er auf diesen Pfad für die Anmeldung umgeleitet.</span><span class="sxs-lookup"><span data-stu-id="9130c-173">When a user is unauthorized, they will be redirected to this path to login.</span></span> | <span data-ttu-id="9130c-174">/Account/Login</span><span class="sxs-lookup"><span data-stu-id="9130c-174">/Account/Login</span></span>  |
| `LogoutPath`  | <span data-ttu-id="9130c-175">Wenn ein Benutzer abgemeldet ist, werden er auf diesen Pfad umgeleitet.</span><span class="sxs-lookup"><span data-stu-id="9130c-175">When a user is logged out, they will be redirected to this path.</span></span>  | <span data-ttu-id="9130c-176">/Account/Logout</span><span class="sxs-lookup"><span data-stu-id="9130c-176">/Account/Logout</span></span>  |
| `AccessDeniedPath`  | <span data-ttu-id="9130c-177">Wenn ein Benutzer mit einer autorisierungsprüfung fehlschlägt, werden er auf diesen Pfad umgeleitet.</span><span class="sxs-lookup"><span data-stu-id="9130c-177">When a user fails an authorization check, they will be redirected to this path.</span></span>  |   |
| `SlidingExpiration`  | <span data-ttu-id="9130c-178">Bei "true", wird ein neues Cookie mit einer neuen Ablaufzeit ausgegeben werden, wenn das aktuelle Cookie mehr als genau, über das Fenster "Ablaufdatum" liegt.</span><span class="sxs-lookup"><span data-stu-id="9130c-178">When true, a new cookie will be issued with a new expiration time when the current cookie is more than halfway through the expiration window.</span></span>  | <span data-ttu-id="9130c-179">/Account/AccessDenied</span><span class="sxs-lookup"><span data-stu-id="9130c-179">/Account/AccessDenied</span></span> |
| `ReturnUrlParameter`  | <span data-ttu-id="9130c-180">Legt den Namen des Abfragezeichenfolgen-Parameters das von der Middleware angefügt wird, wenn ein Statuscode "401 nicht autorisiert" in eine 302-Umleitung Anmeldepfad geändert wird.</span><span class="sxs-lookup"><span data-stu-id="9130c-180">Determines the name of the query string parameter which is appended by the middleware when a 401 Unauthorized status code is changed to a 302 redirect onto the login path.</span></span>  |  <span data-ttu-id="9130c-181">true</span><span class="sxs-lookup"><span data-stu-id="9130c-181">true</span></span> |
| `AuthenticationScheme`  | <span data-ttu-id="9130c-182">Dies ist nur für ASP.NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="9130c-182">This is only relevant for ASP.NET Core 1.x.</span></span> <span data-ttu-id="9130c-183">Der logische Name für ein bestimmtes Authentifizierungsschema.</span><span class="sxs-lookup"><span data-stu-id="9130c-183">The logical name for a particular authentication scheme.</span></span> |  |
| `AutomaticAuthenticate`  | <span data-ttu-id="9130c-184">Dieses Flag ist nur relevant für ASP.NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="9130c-184">This flag is only relevant for ASP.NET Core 1.x.</span></span> <span data-ttu-id="9130c-185">Bei "true", sollte Cookieauthentifizierung für jede Anforderung ausgeführt und versuchen, zu überprüfen und rekonstruieren serialisierten Prinzipal, der Sie erstellt.</span><span class="sxs-lookup"><span data-stu-id="9130c-185">When true, cookie authentication should run on every request and attempt to validate and reconstruct any serialized principal it created.</span></span>  |  |
