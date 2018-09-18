---
title: Konfigurieren von ASP.NET Core-Identität
author: AdrienTorris
description: Verstehen Sie ASP.NET Core Identity-Standardwerte, und erfahren Sie, wie so konfigurieren Sie die Identitätseigenschaften, um benutzerdefinierte Werte verwenden.
ms.author: riande
ms.date: 08/14/2018
uid: security/authentication/identity-configuration
ms.openlocfilehash: 0faab001b981c79f6afa16b2a8cf80c1ef141b11
ms.sourcegitcommit: b2723654af4969a24545f09ebe32004cb5e84a96
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/18/2018
ms.locfileid: "46011299"
---
# <a name="configure-aspnet-core-identity"></a><span data-ttu-id="b557f-103">Konfigurieren von ASP.NET Core-Identität</span><span class="sxs-lookup"><span data-stu-id="b557f-103">Configure ASP.NET Core Identity</span></span>

<span data-ttu-id="b557f-104">ASP.NET Core Identity verwendet Standardwerte für Einstellungen wie z. B. Kennwortrichtlinien, kontosperrung und Cookie-Konfiguration.</span><span class="sxs-lookup"><span data-stu-id="b557f-104">ASP.NET Core Identity uses default values for settings such as password policy, lockout, and cookie configuration.</span></span> <span data-ttu-id="b557f-105">Diese Einstellungen können überschrieben werden, der `Startup` Klasse.</span><span class="sxs-lookup"><span data-stu-id="b557f-105">These settings can be overridden in the `Startup` class.</span></span>

## <a name="identity-options"></a><span data-ttu-id="b557f-106">Identity-Optionen</span><span class="sxs-lookup"><span data-stu-id="b557f-106">Identity options</span></span>

<span data-ttu-id="b557f-107">Die [IdentityOptions](/dotnet/api/microsoft.aspnetcore.identity.identityoptions) -Klasse stellt die Optionen, die zum Konfigurieren des Identity-Systems verwendet werden können.</span><span class="sxs-lookup"><span data-stu-id="b557f-107">The [IdentityOptions](/dotnet/api/microsoft.aspnetcore.identity.identityoptions) class represents the options that can be used to configure the Identity system.</span></span> <span data-ttu-id="b557f-108">`IdentityOptions` nutno nastavit **nach** Aufrufen `AddIdentity` oder `AddDefaultIdentity`.</span><span class="sxs-lookup"><span data-stu-id="b557f-108">`IdentityOptions` must be set **after** calling `AddIdentity` or `AddDefaultIdentity`.</span></span>

### <a name="claims-identity"></a><span data-ttu-id="b557f-109">Anspruchsbasierte Identität</span><span class="sxs-lookup"><span data-stu-id="b557f-109">Claims Identity</span></span>

<span data-ttu-id="b557f-110">[IdentityOptions.ClaimsIdentity](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.claimsidentity) gibt an, die [ClaimsIdentityOptions](/dotnet/api/microsoft.aspnetcore.identity.claimsidentityoptions) mit den Eigenschaften, die in der folgenden Tabelle gezeigt.</span><span class="sxs-lookup"><span data-stu-id="b557f-110">[IdentityOptions.ClaimsIdentity](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.claimsidentity) specifies the [ClaimsIdentityOptions](/dotnet/api/microsoft.aspnetcore.identity.claimsidentityoptions) with the properties shown in the following table.</span></span>

| <span data-ttu-id="b557f-111">Eigenschaft</span><span class="sxs-lookup"><span data-stu-id="b557f-111">Property</span></span> | <span data-ttu-id="b557f-112">Beschreibung</span><span class="sxs-lookup"><span data-stu-id="b557f-112">Description</span></span> | <span data-ttu-id="b557f-113">Standard</span><span class="sxs-lookup"><span data-stu-id="b557f-113">Default</span></span> |
| -------- | ----------- | :-----: |
| [<span data-ttu-id="b557f-114">RoleClaimType</span><span class="sxs-lookup"><span data-stu-id="b557f-114">RoleClaimType</span></span>](/dotnet/api/microsoft.aspnetcore.identity.claimsidentityoptions.roleclaimtype) | <span data-ttu-id="b557f-115">Übernimmt oder bestimmt die für einen Rollenanspruch verwendeten Anspruchstyp.</span><span class="sxs-lookup"><span data-stu-id="b557f-115">Gets or sets the claim type used for a role claim.</span></span> | [<span data-ttu-id="b557f-116">ClaimTypes.Role</span><span class="sxs-lookup"><span data-stu-id="b557f-116">ClaimTypes.Role</span></span>](/dotnet/api/system.security.claims.claimtypes.role) |
| [<span data-ttu-id="b557f-117">SecurityStampClaimType</span><span class="sxs-lookup"><span data-stu-id="b557f-117">SecurityStampClaimType</span></span>](/dotnet/api/microsoft.aspnetcore.identity.claimsidentityoptions.securitystampclaimtype) | <span data-ttu-id="b557f-118">Übernimmt oder bestimmt die für den Stempel sicherheitsanspruch verwendeten Anspruchstyp.</span><span class="sxs-lookup"><span data-stu-id="b557f-118">Gets or sets the claim type used for the security stamp claim.</span></span> | `AspNet.Identity.SecurityStamp` |
| [<span data-ttu-id="b557f-119">UserIdClaimType</span><span class="sxs-lookup"><span data-stu-id="b557f-119">UserIdClaimType</span></span>](/dotnet/api/microsoft.aspnetcore.identity.claimsidentityoptions.useridclaimtype) | <span data-ttu-id="b557f-120">Übernimmt oder bestimmt die für die Benutzer-ID-Anspruch verwendeten Anspruchstyp.</span><span class="sxs-lookup"><span data-stu-id="b557f-120">Gets or sets the claim type used for the user identifier claim.</span></span> | [<span data-ttu-id="b557f-121">ClaimTypes.NameIdentifier</span><span class="sxs-lookup"><span data-stu-id="b557f-121">ClaimTypes.NameIdentifier</span></span>](/dotnet/api/system.security.claims.claimtypes.nameidentifier) |
| [<span data-ttu-id="b557f-122">UserNameClaimType</span><span class="sxs-lookup"><span data-stu-id="b557f-122">UserNameClaimType</span></span>](/dotnet/api/microsoft.aspnetcore.identity.claimsidentityoptions.usernameclaimtype) | <span data-ttu-id="b557f-123">Übernimmt oder bestimmt den Anspruchstyp für den Namensanspruch des Benutzers verwendet.</span><span class="sxs-lookup"><span data-stu-id="b557f-123">Gets or sets the claim type used for the user name claim.</span></span> | [<span data-ttu-id="b557f-124">ClaimTypes.Name</span><span class="sxs-lookup"><span data-stu-id="b557f-124">ClaimTypes.Name</span></span>](/dotnet/api/system.security.claims.claimtypes.name) |

### <a name="lockout"></a><span data-ttu-id="b557f-125">Kontosperrung</span><span class="sxs-lookup"><span data-stu-id="b557f-125">Lockout</span></span>

<span data-ttu-id="b557f-126">Sperre wird festgelegt, der [PasswordSignInAsync](/dotnet/api/microsoft.aspnetcore.identity.signinmanager-1.passwordsigninasync#Microsoft_AspNetCore_Identity_SignInManager_1_PasswordSignInAsync_System_String_System_String_System_Boolean_System_Boolean_) Methode:</span><span class="sxs-lookup"><span data-stu-id="b557f-126">Lockout is set in the [PasswordSignInAsync](/dotnet/api/microsoft.aspnetcore.identity.signinmanager-1.passwordsigninasync#Microsoft_AspNetCore_Identity_SignInManager_1_PasswordSignInAsync_System_String_System_String_System_Boolean_System_Boolean_) method:</span></span>

[!code-csharp[](identity-configuration/sample/Areas/Identity/Pages/Account/Login.cshtml.cs?name=snippet&highlight=9)]

<span data-ttu-id="b557f-127">Der vorangehende Code basiert auf der `Login` Identity-Vorlage.</span><span class="sxs-lookup"><span data-stu-id="b557f-127">The preceding code is based on the `Login` Identity template.</span></span> 

<span data-ttu-id="b557f-128">Sperre Optionen werden in festgelegt `StartUp.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="b557f-128">Lockout options are set in `StartUp.ConfigureServices`:</span></span>

[!code-csharp[](identity-configuration/sample/Startup.cs?name=snippet_lock)]

<span data-ttu-id="b557f-129">Im obigen Code wird die [IdentityOptions](/dotnet/api/microsoft.aspnetcore.identity.identityoptions) [LockoutOptions](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions) mit Standardwerten.</span><span class="sxs-lookup"><span data-stu-id="b557f-129">The preceding code sets the [IdentityOptions](/dotnet/api/microsoft.aspnetcore.identity.identityoptions) [LockoutOptions](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions) with default values.</span></span>

<span data-ttu-id="b557f-130">Eine erfolgreiche Authentifizierung setzt die Anzahl der fehlgeschlagenen Zugriffe Versuche und setzt die Uhr.</span><span class="sxs-lookup"><span data-stu-id="b557f-130">A successful authentication resets the failed access attempts count and resets the clock.</span></span>

<span data-ttu-id="b557f-131">[IdentityOptions.Lockout](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.lockout) gibt an, die [LockoutOptions](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions) mit den Eigenschaften, die in der Tabelle dargestellt.</span><span class="sxs-lookup"><span data-stu-id="b557f-131">[IdentityOptions.Lockout](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.lockout) specifies the [LockoutOptions](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions) with the properties shown in the table.</span></span>

| <span data-ttu-id="b557f-132">Eigenschaft</span><span class="sxs-lookup"><span data-stu-id="b557f-132">Property</span></span> | <span data-ttu-id="b557f-133">Beschreibung</span><span class="sxs-lookup"><span data-stu-id="b557f-133">Description</span></span> | <span data-ttu-id="b557f-134">Standard</span><span class="sxs-lookup"><span data-stu-id="b557f-134">Default</span></span> |
| -------- | ----------- | :-----: |
| [<span data-ttu-id="b557f-135">AllowedForNewUsers</span><span class="sxs-lookup"><span data-stu-id="b557f-135">AllowedForNewUsers</span></span>](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions.allowedfornewusers) | <span data-ttu-id="b557f-136">Bestimmt, ob ein neuer Benutzer gesperrt werden kann.</span><span class="sxs-lookup"><span data-stu-id="b557f-136">Determines if a new user can be locked out.</span></span> | `true` |
| [<span data-ttu-id="b557f-137">DefaultLockoutTimeSpan</span><span class="sxs-lookup"><span data-stu-id="b557f-137">DefaultLockoutTimeSpan</span></span>](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions.defaultlockouttimespan) | <span data-ttu-id="b557f-138">Die Zeitspanne ist ein Benutzer gesperrt, wenn eine Sperre auftritt.</span><span class="sxs-lookup"><span data-stu-id="b557f-138">The amount of time a user is locked out when a lockout occurs.</span></span> | <span data-ttu-id="b557f-139">5 Minuten</span><span class="sxs-lookup"><span data-stu-id="b557f-139">5 minutes</span></span> |
| [<span data-ttu-id="b557f-140">MaxFailedAccessAttempts</span><span class="sxs-lookup"><span data-stu-id="b557f-140">MaxFailedAccessAttempts</span></span>](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions.maxfailedaccessattempts) | <span data-ttu-id="b557f-141">Die Anzahl fehlerhafter Zugriffsversuche, bis ein Benutzer gesperrt ist, wenn die Sperre aktiviert ist.</span><span class="sxs-lookup"><span data-stu-id="b557f-141">The number of failed access attempts until a user is locked out, if lockout is enabled.</span></span> | <span data-ttu-id="b557f-142">5</span><span class="sxs-lookup"><span data-stu-id="b557f-142">5</span></span> |

### <a name="password"></a><span data-ttu-id="b557f-143">Kennwort</span><span class="sxs-lookup"><span data-stu-id="b557f-143">Password</span></span>

<span data-ttu-id="b557f-144">Standardmäßig erfordert Identität an, dass die Kennwörter, einen Großbuchstaben, Kleinbuchstaben, eine Ziffer und ein nicht alphanumerisches Zeichen enthalten.</span><span class="sxs-lookup"><span data-stu-id="b557f-144">By default, Identity requires that passwords contain an uppercase character, lowercase character, a digit, and a non-alphanumeric character.</span></span> <span data-ttu-id="b557f-145">Kennwörter müssen mindestens sechs Zeichen lang sein.</span><span class="sxs-lookup"><span data-stu-id="b557f-145">Passwords must be at least six characters long.</span></span> <span data-ttu-id="b557f-146">[PasswordOptions](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions) kann festgelegt werden, `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="b557f-146">[PasswordOptions](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions) can be set in `Startup.ConfigureServices`.</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](identity-configuration/sample/Startup.cs?name=snippet_pw)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-37,50-52)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-csharp[](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Startup.cs?range=58-65,84)]

::: moniker-end

<span data-ttu-id="b557f-147">[IdentityOptions.Password](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.password) gibt an, die [PasswordOptions](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions) mit den Eigenschaften, die in der Tabelle dargestellt.</span><span class="sxs-lookup"><span data-stu-id="b557f-147">[IdentityOptions.Password](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.password) specifies the [PasswordOptions](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions) with the properties shown in the table.</span></span>

| <span data-ttu-id="b557f-148">Eigenschaft</span><span class="sxs-lookup"><span data-stu-id="b557f-148">Property</span></span> | <span data-ttu-id="b557f-149">Beschreibung</span><span class="sxs-lookup"><span data-stu-id="b557f-149">Description</span></span> | <span data-ttu-id="b557f-150">Standard</span><span class="sxs-lookup"><span data-stu-id="b557f-150">Default</span></span> |
| -------- | ----------- | :-----: |
| [<span data-ttu-id="b557f-151">RequireDigit</span><span class="sxs-lookup"><span data-stu-id="b557f-151">RequireDigit</span></span>](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requiredigit) | <span data-ttu-id="b557f-152">Erfordert eine Zahl zwischen 0-9, das Kennwort an.</span><span class="sxs-lookup"><span data-stu-id="b557f-152">Requires a number between 0-9 in the password.</span></span> | `true` |
| [<span data-ttu-id="b557f-153">RequiredLength</span><span class="sxs-lookup"><span data-stu-id="b557f-153">RequiredLength</span></span>](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requiredlength) | <span data-ttu-id="b557f-154">Die Mindestlänge des Kennworts.</span><span class="sxs-lookup"><span data-stu-id="b557f-154">The minimum length of the password.</span></span> | <span data-ttu-id="b557f-155">6</span><span class="sxs-lookup"><span data-stu-id="b557f-155">6</span></span> |

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="b557f-156">| [RequiredUniqueChars](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requireduniquechars) | Gilt nur für ASP.NET Core 2.0 oder höher aus.</span><span class="sxs-lookup"><span data-stu-id="b557f-156">| [RequiredUniqueChars](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requireduniquechars) | Only applies to ASP.NET Core 2.0 or later.</span></span><br><br> <span data-ttu-id="b557f-157">Ist die Anzahl der unterschiedlichen Zeichen im Kennwort erforderlich.</span><span class="sxs-lookup"><span data-stu-id="b557f-157">Requires the number of distinct characters in the password.</span></span> <span data-ttu-id="b557f-158">| 1 |</span><span class="sxs-lookup"><span data-stu-id="b557f-158">| 1 |</span></span>

::: moniker-end

<span data-ttu-id="b557f-159">| [RequireLowercase](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requirelowercase) | Erfordert ein Kleinbuchstabe, das Kennwort an.</span><span class="sxs-lookup"><span data-stu-id="b557f-159">| [RequireLowercase](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requirelowercase) | Requires a lowercase character in the password.</span></span><span data-ttu-id="b557f-160"> | `true` | | [RequireNonAlphanumeric](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requirenonalphanumeric) | Ist ein nicht alphanumerisches Zeichen im Kennwort erforderlich.</span><span class="sxs-lookup"><span data-stu-id="b557f-160"> | `true` | | [RequireNonAlphanumeric](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requirenonalphanumeric) | Requires a non-alphanumeric character in the password.</span></span><span data-ttu-id="b557f-161"> | `true` | | [RequireUppercase](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requireuppercase) | Ist ein Großbuchstabe Zeichen im Kennwort erforderlich.</span><span class="sxs-lookup"><span data-stu-id="b557f-161"> | `true` | | [RequireUppercase](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requireuppercase) | Requires an uppercase character in the password.</span></span> | `true` |

### <a name="sign-in"></a><span data-ttu-id="b557f-162">Anmeldung</span><span class="sxs-lookup"><span data-stu-id="b557f-162">Sign-in</span></span>

<span data-ttu-id="b557f-163">Im folgenden code wird `SignIn` Einstellungen (Standardwerte):</span><span class="sxs-lookup"><span data-stu-id="b557f-163">The following code sets `SignIn` settings (to default values):</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](identity-configuration/sample/Startup.cs?name=snippet_si)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-30,44-46,50-52)] 

::: moniker-end

<span data-ttu-id="b557f-164">[IdentityOptions.SignIn](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.signin) gibt an, die [SignInOptions](/dotnet/api/microsoft.aspnetcore.identity.signinoptions) mit den Eigenschaften, die in der Tabelle dargestellt.</span><span class="sxs-lookup"><span data-stu-id="b557f-164">[IdentityOptions.SignIn](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.signin) specifies the [SignInOptions](/dotnet/api/microsoft.aspnetcore.identity.signinoptions) with the properties shown in the table.</span></span>

| <span data-ttu-id="b557f-165">Eigenschaft</span><span class="sxs-lookup"><span data-stu-id="b557f-165">Property</span></span> | <span data-ttu-id="b557f-166">Beschreibung</span><span class="sxs-lookup"><span data-stu-id="b557f-166">Description</span></span> | <span data-ttu-id="b557f-167">Standard</span><span class="sxs-lookup"><span data-stu-id="b557f-167">Default</span></span> |
| -------- | ----------- | :-----: |
| [<span data-ttu-id="b557f-168">RequireConfirmedEmail</span><span class="sxs-lookup"><span data-stu-id="b557f-168">RequireConfirmedEmail</span></span>](/dotnet/api/microsoft.aspnetcore.identity.signinoptions.requireconfirmedemail) | <span data-ttu-id="b557f-169">Erfordert eine bestätigte e-Mail-Adresse für die Anmeldung an.</span><span class="sxs-lookup"><span data-stu-id="b557f-169">Requires a confirmed email to sign in.</span></span> | `false` |
| [<span data-ttu-id="b557f-170">RequireConfirmedPhoneNumber</span><span class="sxs-lookup"><span data-stu-id="b557f-170">RequireConfirmedPhoneNumber</span></span>](/dotnet/api/microsoft.aspnetcore.identity.signinoptions.requireconfirmedphonenumber) | <span data-ttu-id="b557f-171">Erfordert eine bestätigte Telefonnummer für die Anmeldung an.</span><span class="sxs-lookup"><span data-stu-id="b557f-171">Requires a confirmed phone number to sign in.</span></span> | `false` |

### <a name="tokens"></a><span data-ttu-id="b557f-172">tokens</span><span class="sxs-lookup"><span data-stu-id="b557f-172">Tokens</span></span>

<span data-ttu-id="b557f-173">[IdentityOptions.Tokens](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.tokens) gibt an, die [TokenOptions](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions) mit den Eigenschaften, die in der Tabelle dargestellt.</span><span class="sxs-lookup"><span data-stu-id="b557f-173">[IdentityOptions.Tokens](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.tokens) specifies the [TokenOptions](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions) with the properties shown in the table.</span></span>


|                                                        <span data-ttu-id="b557f-174">Eigenschaft</span><span class="sxs-lookup"><span data-stu-id="b557f-174">Property</span></span>                                                         |                                                                                      <span data-ttu-id="b557f-175">Beschreibung</span><span class="sxs-lookup"><span data-stu-id="b557f-175">Description</span></span>                                                                                      |
|-------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|     [<span data-ttu-id="b557f-176">AuthenticatorTokenProvider</span><span class="sxs-lookup"><span data-stu-id="b557f-176">AuthenticatorTokenProvider</span></span>](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions.authenticatortokenprovider)     |                                       <span data-ttu-id="b557f-177">Übernimmt oder bestimmt den `AuthenticatorTokenProvider` verwendet, um zwei-Faktor-Anmeldungen mit einem Authentifikator überprüfen.</span><span class="sxs-lookup"><span data-stu-id="b557f-177">Gets or sets the `AuthenticatorTokenProvider` used to validate two-factor sign-ins with an authenticator.</span></span>                                       |
|       [<span data-ttu-id="b557f-178">ChangeEmailTokenProvider</span><span class="sxs-lookup"><span data-stu-id="b557f-178">ChangeEmailTokenProvider</span></span>](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions.changeemailtokenprovider)       |                                     <span data-ttu-id="b557f-179">Übernimmt oder bestimmt den `ChangeEmailTokenProvider` verwendet zum Generieren von Token, die in e-Mail-Adresse ändern Bestätigungs-e-Mails verwendet.</span><span class="sxs-lookup"><span data-stu-id="b557f-179">Gets or sets the `ChangeEmailTokenProvider` used to generate tokens used in email change confirmation emails.</span></span>                                     |
| [<span data-ttu-id="b557f-180">ChangePhoneNumberTokenProvider</span><span class="sxs-lookup"><span data-stu-id="b557f-180">ChangePhoneNumberTokenProvider</span></span>](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions.changephonenumbertokenprovider) |                                      <span data-ttu-id="b557f-181">Übernimmt oder bestimmt die `ChangePhoneNumberTokenProvider` verwendet zum Generieren von Tokens verwendet, wenn die Telefonnummern zu ändern.</span><span class="sxs-lookup"><span data-stu-id="b557f-181">Gets or sets the `ChangePhoneNumberTokenProvider` used to generate tokens used when changing phone numbers.</span></span>                                      |
| [<span data-ttu-id="b557f-182">EmailConfirmationTokenProvider</span><span class="sxs-lookup"><span data-stu-id="b557f-182">EmailConfirmationTokenProvider</span></span>](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions.emailconfirmationtokenprovider) |                                             <span data-ttu-id="b557f-183">Übernimmt oder bestimmt den Tokenanbieter, der zum Generieren von Tokens, die im Konto Bestätigungs-e-Mails verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="b557f-183">Gets or sets the token provider used to generate tokens used in account confirmation emails.</span></span>                                              |
|     [<span data-ttu-id="b557f-184">PasswordResetTokenProvider</span><span class="sxs-lookup"><span data-stu-id="b557f-184">PasswordResetTokenProvider</span></span>](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions.passwordresettokenprovider)     | <span data-ttu-id="b557f-185">Ruft ab oder legt die [IUserTwoFactorTokenProvider<TUser> ](/dotnet/api/microsoft.aspnetcore.identity.iusertwofactortokenprovider-1) verwendet zum Generieren von Token, die im Kennwort zurücksetzen der e-Mail-Nachrichten verwendet.</span><span class="sxs-lookup"><span data-stu-id="b557f-185">Gets or sets the [IUserTwoFactorTokenProvider<TUser>](/dotnet/api/microsoft.aspnetcore.identity.iusertwofactortokenprovider-1) used to generate tokens used in password reset emails.</span></span> |
|                    [<span data-ttu-id="b557f-186">ProviderMap</span><span class="sxs-lookup"><span data-stu-id="b557f-186">ProviderMap</span></span>](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions.providermap)                    |                <span data-ttu-id="b557f-187">Zum Erstellen einer [Benutzer Tokenanbieter](/dotnet/api/microsoft.aspnetcore.identity.tokenproviderdescriptor) mit dem Schlüssel verwendet wird, wie der Name des Anbieters.</span><span class="sxs-lookup"><span data-stu-id="b557f-187">Used to construct a [User Token Provider](/dotnet/api/microsoft.aspnetcore.identity.tokenproviderdescriptor) with the key used as the provider's name.</span></span>                 |

### <a name="user"></a><span data-ttu-id="b557f-188">Benutzer</span><span class="sxs-lookup"><span data-stu-id="b557f-188">User</span></span>

[!code-csharp[](identity-configuration/sample/Startup.cs?name=snippet_user)]

<span data-ttu-id="b557f-189">[IdentityOptions.User](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.user) gibt an, die [UserOptions](/dotnet/api/microsoft.aspnetcore.identity.useroptions) mit den Eigenschaften, die in der Tabelle dargestellt.</span><span class="sxs-lookup"><span data-stu-id="b557f-189">[IdentityOptions.User](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.user) specifies the [UserOptions](/dotnet/api/microsoft.aspnetcore.identity.useroptions) with the properties shown in the table.</span></span>

| <span data-ttu-id="b557f-190">Eigenschaft</span><span class="sxs-lookup"><span data-stu-id="b557f-190">Property</span></span> | <span data-ttu-id="b557f-191">Beschreibung</span><span class="sxs-lookup"><span data-stu-id="b557f-191">Description</span></span> | <span data-ttu-id="b557f-192">Standard</span><span class="sxs-lookup"><span data-stu-id="b557f-192">Default</span></span> |
| -------- | ----------- | :-----: |
| [<span data-ttu-id="b557f-193">AllowedUserNameCharacters</span><span class="sxs-lookup"><span data-stu-id="b557f-193">AllowedUserNameCharacters</span></span>](/dotnet/api/microsoft.aspnetcore.identity.useroptions.allowedusernamecharacters) | <span data-ttu-id="b557f-194">Zulässige Zeichen des Benutzernamens.</span><span class="sxs-lookup"><span data-stu-id="b557f-194">Allowed characters in the username.</span></span> | <span data-ttu-id="b557f-195">abcdefghijklmnopqrstuvwxyz</span><span class="sxs-lookup"><span data-stu-id="b557f-195">abcdefghijklmnopqrstuvwxyz</span></span><br><span data-ttu-id="b557f-196">ABCDEFGHIJKLMNOPQRSTUVWXYZ</span><span class="sxs-lookup"><span data-stu-id="b557f-196">ABCDEFGHIJKLMNOPQRSTUVWXYZ</span></span><br><span data-ttu-id="b557f-197">0123456789</span><span class="sxs-lookup"><span data-stu-id="b557f-197">0123456789</span></span><br><span data-ttu-id="b557f-198">-._@+</span><span class="sxs-lookup"><span data-stu-id="b557f-198">-._@+</span></span> |
| [<span data-ttu-id="b557f-199">RequireUniqueEmail</span><span class="sxs-lookup"><span data-stu-id="b557f-199">RequireUniqueEmail</span></span>](/dotnet/api/microsoft.aspnetcore.identity.useroptions.requireuniqueemail) | <span data-ttu-id="b557f-200">Müssen alle Benutzer eine eindeutige e-Mail-Adresse haben.</span><span class="sxs-lookup"><span data-stu-id="b557f-200">Requires each user to have a unique email.</span></span> | `false` |

### <a name="cookie-settings"></a><span data-ttu-id="b557f-201">Cookie-Einstellungen</span><span class="sxs-lookup"><span data-stu-id="b557f-201">Cookie settings</span></span>

<span data-ttu-id="b557f-202">Konfigurieren Sie in der app-Cookie `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="b557f-202">Configure the app's cookie in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="b557f-203">[ConfigureApplicationCookie](/dotnet/api/microsoft.extensions.dependencyinjection.identityservicecollectionextensions.configureapplicationcookie#Microsoft_Extensions_DependencyInjection_IdentityServiceCollectionExtensions_ConfigureApplicationCookie_Microsoft_Extensions_DependencyInjection_IServiceCollection_System_Action_Microsoft_AspNetCore_Authentication_Cookies_CookieAuthenticationOptions__) muss aufgerufen werden, **nach** Aufrufen `AddIdentity` oder `AddDefaultIdentity`.</span><span class="sxs-lookup"><span data-stu-id="b557f-203">[ConfigureApplicationCookie](/dotnet/api/microsoft.extensions.dependencyinjection.identityservicecollectionextensions.configureapplicationcookie#Microsoft_Extensions_DependencyInjection_IdentityServiceCollectionExtensions_ConfigureApplicationCookie_Microsoft_Extensions_DependencyInjection_IServiceCollection_System_Action_Microsoft_AspNetCore_Authentication_Cookies_CookieAuthenticationOptions__) must be called **after** calling `AddIdentity` or `AddDefaultIdentity`.</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](identity-configuration/sample/Startup.cs?name=snippet_cookie)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?name=snippet_configurecookie)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-csharp[](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Startup.cs?range=58-59,72-80,84)]

::: moniker-end

<span data-ttu-id="b557f-204">Weitere Informationen finden Sie unter [CookieAuthenticationOptions](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions).</span><span class="sxs-lookup"><span data-stu-id="b557f-204">For more information, see [CookieAuthenticationOptions](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions).</span></span>
