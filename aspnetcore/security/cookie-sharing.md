---
title: ASP.NET und ASP.NET Core freigeben Sie Cookies zwischen apps
author: rick-anderson
description: Informationen zum Freigeben von Authentifizierungscookies zwischen ASP.NET 4.x und ASP.NET Core-apps.
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 01/19/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/cookie-sharing
ms.openlocfilehash: f6d62d5f6e446e3e2001ed6bde72a6c409aa2833
ms.sourcegitcommit: 726ffab258070b4fe6cf950bf030ce10c0c07bb4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/04/2018
ms.locfileid: "34734678"
---
# <a name="share-cookies-among-apps-with-aspnet-and-aspnet-core"></a><span data-ttu-id="2cb5c-103">ASP.NET und ASP.NET Core freigeben Sie Cookies zwischen apps</span><span class="sxs-lookup"><span data-stu-id="2cb5c-103">Share cookies among apps with ASP.NET and ASP.NET Core</span></span>

<span data-ttu-id="2cb5c-104">Durch [Rick Anderson](https://twitter.com/RickAndMSFT) und [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="2cb5c-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="2cb5c-105">Websites bestehen häufig aus einzelnen Web-apps, die durch die Zusammenarbeit.</span><span class="sxs-lookup"><span data-stu-id="2cb5c-105">Websites often consist of individual web apps working together.</span></span> <span data-ttu-id="2cb5c-106">Um ein einmaliges Anmelden für (Unternehmen SSO) zu bieten zu können, müssen die Web-apps innerhalb eines Standorts Authentifizierungscookies freigeben.</span><span class="sxs-lookup"><span data-stu-id="2cb5c-106">To provide a single sign-on (SSO) experience, web apps within a site must share authentication cookies.</span></span> <span data-ttu-id="2cb5c-107">Zur Unterstützung dieses Szenarios kann Data Protection Stapel Katana-Cookieauthentifizierung und ASP.NET Core Cookie Authentifizierungstickets freigeben.</span><span class="sxs-lookup"><span data-stu-id="2cb5c-107">To support this scenario, the data protection stack allows sharing Katana cookie authentication and ASP.NET Core cookie authentication tickets.</span></span>

<span data-ttu-id="2cb5c-108">[Anzeigen oder Herunterladen von Beispielcode](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/cookie-sharing/sample/) ([Vorgehensweise zum Herunterladen](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="2cb5c-108">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/cookie-sharing/sample/) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="2cb5c-109">Im Beispiel werden alle drei apps, die Cookieauthentifizierung gemeinsam Cookie veranschaulicht:</span><span class="sxs-lookup"><span data-stu-id="2cb5c-109">The sample illustrates cookie sharing across three apps that use cookie authentication:</span></span>

* <span data-ttu-id="2cb5c-110">ASP.NET 2.0-Razor-Seiten Core app ohne [ASP.NET Core Identity](xref:security/authentication/identity)</span><span class="sxs-lookup"><span data-stu-id="2cb5c-110">ASP.NET Core 2.0 Razor Pages app without using [ASP.NET Core Identity](xref:security/authentication/identity)</span></span>
* <span data-ttu-id="2cb5c-111">ASP.NET Core 2.0 MVC-Anwendung mit ASP.NET Core Identität</span><span class="sxs-lookup"><span data-stu-id="2cb5c-111">ASP.NET Core 2.0 MVC app with ASP.NET Core Identity</span></span>
* <span data-ttu-id="2cb5c-112">ASP.NET Framework 4.6.1 MVC-Anwendung mit ASP.NET Identity</span><span class="sxs-lookup"><span data-stu-id="2cb5c-112">ASP.NET Framework 4.6.1 MVC app with ASP.NET Identity</span></span>

<span data-ttu-id="2cb5c-113">In den folgenden Beispielen:</span><span class="sxs-lookup"><span data-stu-id="2cb5c-113">In the examples that follow:</span></span>

* <span data-ttu-id="2cb5c-114">Der Cookienamen für die Authentifizierung wird festgelegt, um einen allgemeinen Wert des `.AspNet.SharedCookie`.</span><span class="sxs-lookup"><span data-stu-id="2cb5c-114">The authentication cookie name is set to a common value of `.AspNet.SharedCookie`.</span></span>
* <span data-ttu-id="2cb5c-115">Die `AuthenticationType` festgelegt ist, um `Identity.Application` entweder explizit oder standardmäßig.</span><span class="sxs-lookup"><span data-stu-id="2cb5c-115">The `AuthenticationType` is set to `Identity.Application` either explicitly or by default.</span></span>
* <span data-ttu-id="2cb5c-116">Ein allgemeiner app-Name dient zum Aktivieren der Datenschutzsystem zur Freigabe von Data Protection Schlüsseln (`SharedCookieApp`).</span><span class="sxs-lookup"><span data-stu-id="2cb5c-116">A common app name is used to enable the data protection system to share data protection keys (`SharedCookieApp`).</span></span>
* <span data-ttu-id="2cb5c-117">`Identity.Application` Dient als Authentifizierungsschema.</span><span class="sxs-lookup"><span data-stu-id="2cb5c-117">`Identity.Application` is used as the authentication scheme.</span></span> <span data-ttu-id="2cb5c-118">Alle Schema verwendet wird, müssen einheitlich verwendet werden *innerhalb und außerhalb von* freigegebenen Cookie-apps als Standardschema oder explizit festlegen.</span><span class="sxs-lookup"><span data-stu-id="2cb5c-118">Whatever scheme is used, it must be used consistently *within and across* the shared cookie apps either as the default scheme or by explicitly setting it.</span></span> <span data-ttu-id="2cb5c-119">Das Schema wird verwendet, beim Ver- und Entschlüsslung von Cookies, damit ein konsistentes Schema für apps verwendet werden muss.</span><span class="sxs-lookup"><span data-stu-id="2cb5c-119">The scheme is used when encrypting and decrypting cookies, so a consistent scheme must be used across apps.</span></span>
* <span data-ttu-id="2cb5c-120">Eine häufige [Daten Schutzschlüssels](xref:security/data-protection/implementation/key-management) Speicherort verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="2cb5c-120">A common [data protection key](xref:security/data-protection/implementation/key-management) storage location is used.</span></span> <span data-ttu-id="2cb5c-121">Die Beispiel-app verwendet einen Ordner namens *Schlüsselsammlung* im Stammverzeichnis der Projektmappe, die Data Protection Schlüssel enthalten soll.</span><span class="sxs-lookup"><span data-stu-id="2cb5c-121">The sample app uses a folder named *KeyRing* at the root of the solution to hold the data protection keys.</span></span>
* <span data-ttu-id="2cb5c-122">Bei den apps ASP.NET Core [PersistKeysToFileSystem](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystofilesystem) wird verwendet, um die wichtigsten Speicherort festgelegt.</span><span class="sxs-lookup"><span data-stu-id="2cb5c-122">In the ASP.NET Core apps, [PersistKeysToFileSystem](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystofilesystem) is used to set the key storage location.</span></span> <span data-ttu-id="2cb5c-123">[SetApplicationName](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setapplicationname) wird verwendet, um ein freigegebenes app-Antragstellernamens zu konfigurieren.</span><span class="sxs-lookup"><span data-stu-id="2cb5c-123">[SetApplicationName](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setapplicationname) is used to configure a common shared app name.</span></span>
* <span data-ttu-id="2cb5c-124">In der .NET Framework-app verwendet die cookieauthentifizierungsmiddleware eine Implementierung des [DataProtectionProvider](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionprovider).</span><span class="sxs-lookup"><span data-stu-id="2cb5c-124">In the .NET Framework app, the cookie authentication middleware uses an implementation of [DataProtectionProvider](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionprovider).</span></span> <span data-ttu-id="2cb5c-125">`DataProtectionProvider` bietet Datenschutzdienste für die Ver- und Entschlüsselung von Authentifizierungsdaten Cookie-Nutzlast an.</span><span class="sxs-lookup"><span data-stu-id="2cb5c-125">`DataProtectionProvider` provides data protection services for the encryption and decryption of authentication cookie payload data.</span></span> <span data-ttu-id="2cb5c-126">Die `DataProtectionProvider` Instanz isoliert die Datenschutzsystem durch andere Teile der app verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="2cb5c-126">The `DataProtectionProvider` instance is isolated from the data protection system used by other parts of the app.</span></span>
  * <span data-ttu-id="2cb5c-127">[DataProtectionProvider.Create (System.IO.DirectoryInfo, Aktion\<IDataProtectionBuilder >)](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionprovider.create?view=aspnetcore-2.0#Microsoft_AspNetCore_DataProtection_DataProtectionProvider_Create_System_IO_DirectoryInfo_System_Action_Microsoft_AspNetCore_DataProtection_IDataProtectionBuilder__) akzeptiert eine [DirectoryInfo](/dotnet/api/system.io.directoryinfo) den Speicherort zum Speichern von Data Protection Schlüsseln an.</span><span class="sxs-lookup"><span data-stu-id="2cb5c-127">[DataProtectionProvider.Create(System.IO.DirectoryInfo, Action\<IDataProtectionBuilder>)](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionprovider.create?view=aspnetcore-2.0#Microsoft_AspNetCore_DataProtection_DataProtectionProvider_Create_System_IO_DirectoryInfo_System_Action_Microsoft_AspNetCore_DataProtection_IDataProtectionBuilder__) accepts a [DirectoryInfo](/dotnet/api/system.io.directoryinfo) to specify the location for data protection key storage.</span></span> <span data-ttu-id="2cb5c-128">Die Beispiel-app stellt den Pfad zu der *Schlüsselsammlung* Ordner `DirectoryInfo`.</span><span class="sxs-lookup"><span data-stu-id="2cb5c-128">The sample app provides the path of the *KeyRing* folder to `DirectoryInfo`.</span></span> <span data-ttu-id="2cb5c-129">[DataProtectionBuilderExtensions.SetApplicationName](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setapplicationname?view=aspnetcore-2.0#Microsoft_AspNetCore_DataProtection_DataProtectionBuilderExtensions_SetApplicationName_Microsoft_AspNetCore_DataProtection_IDataProtectionBuilder_System_String_) sets the common app name.</span><span class="sxs-lookup"><span data-stu-id="2cb5c-129">[DataProtectionBuilderExtensions.SetApplicationName](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setapplicationname?view=aspnetcore-2.0#Microsoft_AspNetCore_DataProtection_DataProtectionBuilderExtensions_SetApplicationName_Microsoft_AspNetCore_DataProtection_IDataProtectionBuilder_System_String_) sets the common app name.</span></span>
  * <span data-ttu-id="2cb5c-130">[DataProtectionProvider](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionprovider) erfordert die [Microsoft.AspNetCore.DataProtection.Extensions](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.Extensions/) NuGet-Paket.</span><span class="sxs-lookup"><span data-stu-id="2cb5c-130">[DataProtectionProvider](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionprovider) requires the [Microsoft.AspNetCore.DataProtection.Extensions](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.Extensions/) NuGet package.</span></span> <span data-ttu-id="2cb5c-131">Um dieses Paket für ASP.NET Core 2.1 und höher apps erhalten, verweisen die [Microsoft.AspNetCore.App Metapackage](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="2cb5c-131">To obtain this package for ASP.NET Core 2.1 and later apps, reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span> <span data-ttu-id="2cb5c-132">Wenn .NET Framework verwenden möchten, fügen Sie einen Paket-Verweis auf `Microsoft.AspNetCore.DataProtection.Extensions`.</span><span class="sxs-lookup"><span data-stu-id="2cb5c-132">When targeting the .NET Framework, add a package reference to `Microsoft.AspNetCore.DataProtection.Extensions`.</span></span>

## <a name="share-authentication-cookies-among-aspnet-core-apps"></a><span data-ttu-id="2cb5c-133">Authentifizierungscookies zwischen ASP.NET Core apps freigeben</span><span class="sxs-lookup"><span data-stu-id="2cb5c-133">Share authentication cookies among ASP.NET Core apps</span></span>

<span data-ttu-id="2cb5c-134">Bei Verwendung von ASP.NET Core Identität:</span><span class="sxs-lookup"><span data-stu-id="2cb5c-134">When using ASP.NET Core Identity:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="2cb5c-135">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="2cb5c-135">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

<span data-ttu-id="2cb5c-136">In der `ConfigureServices` -Methode, mit der [ConfigureApplicationCookie](/dotnet/api/microsoft.extensions.dependencyinjection.identityservicecollectionextensions.configureapplicationcookie) Erweiterungsmethode zum Einrichten des Data Protection-Diensts für Cookies.</span><span class="sxs-lookup"><span data-stu-id="2cb5c-136">In the `ConfigureServices` method, use the [ConfigureApplicationCookie](/dotnet/api/microsoft.extensions.dependencyinjection.identityservicecollectionextensions.configureapplicationcookie) extension method to set up the data protection service for cookies.</span></span>

[!code-csharp[](cookie-sharing/sample/CookieAuthWithIdentity.Core/Startup.cs?name=snippet1)]

<span data-ttu-id="2cb5c-137">Data Protection Schlüssel und der app-Name müssen zwischen apps freigegeben werden.</span><span class="sxs-lookup"><span data-stu-id="2cb5c-137">Data protection keys and the app name must be shared among apps.</span></span> <span data-ttu-id="2cb5c-138">In der beispielapps `GetKeyRingDirInfo` gibt den Speicherort des allgemeinen Schlüsselspeicher, der [PersistKeysToFileSystem](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystofilesystem) Methode.</span><span class="sxs-lookup"><span data-stu-id="2cb5c-138">In the sample apps, `GetKeyRingDirInfo` returns the common key storage location to the [PersistKeysToFileSystem](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystofilesystem) method.</span></span> <span data-ttu-id="2cb5c-139">Verwendung [SetApplicationName](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setapplicationname) so konfigurieren Sie einen allgemeinen freigegebenes app-Namen (`SharedCookieApp` im Beispiel).</span><span class="sxs-lookup"><span data-stu-id="2cb5c-139">Use [SetApplicationName](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setapplicationname) to configure a common shared app name (`SharedCookieApp` in the sample).</span></span> <span data-ttu-id="2cb5c-140">Weitere Informationen finden Sie unter [Konfigurieren von Data Protection](xref:security/data-protection/configuration/overview).</span><span class="sxs-lookup"><span data-stu-id="2cb5c-140">For more information, see [Configuring Data Protection](xref:security/data-protection/configuration/overview).</span></span>

<span data-ttu-id="2cb5c-141">Finden Sie unter der *CookieAuthWithIdentity.Core* -Projekt in der [Beispielcode](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/cookie-sharing/sample/) ([zum Herunterladen von](xref:tutorials/index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="2cb5c-141">See the *CookieAuthWithIdentity.Core* project in the [sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/cookie-sharing/sample/) ([how to download](xref:tutorials/index#how-to-download-a-sample)).</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="2cb5c-142">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="2cb5c-142">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

<span data-ttu-id="2cb5c-143">In der `Configure` -Methode, mit der [CookieAuthenticationOptions](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions) einrichten:</span><span class="sxs-lookup"><span data-stu-id="2cb5c-143">In the `Configure` method, use the [CookieAuthenticationOptions](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions) to set up:</span></span>

* <span data-ttu-id="2cb5c-144">Der Data Protection-Dienst für Cookies.</span><span class="sxs-lookup"><span data-stu-id="2cb5c-144">The data protection service for cookies.</span></span>
* <span data-ttu-id="2cb5c-145">Die `AuthenticationScheme` entsprechend der ASP.NET 4.x.</span><span class="sxs-lookup"><span data-stu-id="2cb5c-145">The `AuthenticationScheme` to match ASP.NET 4.x.</span></span>

```csharp
app.AddIdentity<ApplicationUser, IdentityRole>(options =>
{
    options.Cookies.ApplicationCookie.AuthenticationScheme = 
        "ApplicationCookie";

    var protectionProvider = 
        DataProtectionProvider.Create(
            new DirectoryInfo(@"PATH_TO_KEY_RING_FOLDER"));

    options.Cookies.ApplicationCookie.DataProtectionProvider = 
        protectionProvider;

    options.Cookies.ApplicationCookie.TicketDataFormat = 
        new TicketDataFormat(protectionProvider.CreateProtector(
            "Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationMiddleware", 
            "Cookies", 
            "v2"));
});
```

---

<span data-ttu-id="2cb5c-146">Wenn Sie Cookies direkt verwenden:</span><span class="sxs-lookup"><span data-stu-id="2cb5c-146">When using cookies directly:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="2cb5c-147">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="2cb5c-147">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

[!code-csharp[](cookie-sharing/sample/CookieAuth.Core/Startup.cs?name=snippet1)]

<span data-ttu-id="2cb5c-148">Data Protection Schlüssel und der app-Name müssen zwischen apps freigegeben werden.</span><span class="sxs-lookup"><span data-stu-id="2cb5c-148">Data protection keys and the app name must be shared among apps.</span></span> <span data-ttu-id="2cb5c-149">In der beispielapps `GetKeyRingDirInfo` gibt den Speicherort des allgemeinen Schlüsselspeicher, der [PersistKeysToFileSystem](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystofilesystem) Methode.</span><span class="sxs-lookup"><span data-stu-id="2cb5c-149">In the sample apps, `GetKeyRingDirInfo` returns the common key storage location to the [PersistKeysToFileSystem](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystofilesystem) method.</span></span> <span data-ttu-id="2cb5c-150">Verwendung [SetApplicationName](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setapplicationname) so konfigurieren Sie einen allgemeinen freigegebenes app-Namen (`SharedCookieApp` im Beispiel).</span><span class="sxs-lookup"><span data-stu-id="2cb5c-150">Use [SetApplicationName](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setapplicationname) to configure a common shared app name (`SharedCookieApp` in the sample).</span></span> <span data-ttu-id="2cb5c-151">Weitere Informationen finden Sie unter [Konfigurieren von Data Protection](xref:security/data-protection/configuration/overview).</span><span class="sxs-lookup"><span data-stu-id="2cb5c-151">For more information, see [Configuring Data Protection](xref:security/data-protection/configuration/overview).</span></span> 

<span data-ttu-id="2cb5c-152">Finden Sie unter der *CookieAuth.Core* -Projekt in der [Beispielcode](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/cookie-sharing/sample/) ([zum Herunterladen von](xref:tutorials/index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="2cb5c-152">See the *CookieAuth.Core* project in the [sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/cookie-sharing/sample/) ([how to download](xref:tutorials/index#how-to-download-a-sample)).</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="2cb5c-153">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="2cb5c-153">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

```csharp
app.UseCookieAuthentication(new CookieAuthenticationOptions
{
    DataProtectionProvider = 
        DataProtectionProvider.Create(
            new DirectoryInfo(@"PATH_TO_KEY_RING_FOLDER"))
});
```

---

## <a name="encrypting-data-protection-keys-at-rest"></a><span data-ttu-id="2cb5c-154">Verschlüsseln von Data Protection Schlüssel im Ruhezustand</span><span class="sxs-lookup"><span data-stu-id="2cb5c-154">Encrypting data protection keys at rest</span></span>

<span data-ttu-id="2cb5c-155">Für produktionsbereitstellungen, konfigurieren die `DataProtectionProvider` im Ruhezustand mit DPAPI oder ein X509Certificate Schlüssel verschlüsseln.</span><span class="sxs-lookup"><span data-stu-id="2cb5c-155">For production deployments, configure the `DataProtectionProvider` to encrypt keys at rest with DPAPI or an X509Certificate.</span></span> <span data-ttu-id="2cb5c-156">Finden Sie unter [Schlüssel Verschlüsselung ruhender](xref:security/data-protection/implementation/key-encryption-at-rest) für Weitere Informationen.</span><span class="sxs-lookup"><span data-stu-id="2cb5c-156">See [Key Encryption At Rest](xref:security/data-protection/implementation/key-encryption-at-rest) for more information.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="2cb5c-157">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="2cb5c-157">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
services.AddDataProtection()
    .ProtectKeysWithCertificate("thumbprint");
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="2cb5c-158">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="2cb5c-158">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
app.UseCookieAuthentication(new CookieAuthenticationOptions
{
    DataProtectionProvider = DataProtectionProvider.Create(
        new DirectoryInfo(@"PATH_TO_KEY_RING"),
        configure =>
        {
            configure.ProtectKeysWithCertificate("thumbprint");
        })
});
```

---

## <a name="sharing-authentication-cookies-between-aspnet-4x-and-aspnet-core-apps"></a><span data-ttu-id="2cb5c-159">Freigeben von Authentifizierungscookies zwischen ASP.NET 4.x und ASP.NET Core-apps</span><span class="sxs-lookup"><span data-stu-id="2cb5c-159">Sharing authentication cookies between ASP.NET 4.x and ASP.NET Core apps</span></span>

<span data-ttu-id="2cb5c-160">ASP.NET 4.x-apps, die Katana-cookieauthentifizierungsmiddleware verwenden, können um Authentifizierungscookies zu generieren, die mit der ASP.NET Core cookieauthentifizierungsmiddleware kompatibel sind, konfiguriert werden.</span><span class="sxs-lookup"><span data-stu-id="2cb5c-160">ASP.NET 4.x apps which use Katana cookie authentication middleware can be configured to generate authentication cookies that are compatible with the ASP.NET Core cookie authentication middleware.</span></span> <span data-ttu-id="2cb5c-161">Dies ermöglicht eine große Site einzelne apps schrittweise beim Bereitstellen einer smooth benutzerumgebung am gesamten Standort aktualisieren.</span><span class="sxs-lookup"><span data-stu-id="2cb5c-161">This allows upgrading a large site's individual apps piecemeal while providing a smooth SSO experience across the site.</span></span>

> [!TIP]
> <span data-ttu-id="2cb5c-162">Wenn eine app Katana-cookieauthentifizierungsmiddleware verwendet, ruft er `UseCookieAuthentication` des Projekts *Startup.Auth.cs* Datei.</span><span class="sxs-lookup"><span data-stu-id="2cb5c-162">When an app uses Katana cookie authentication middleware, it calls `UseCookieAuthentication` in the project's *Startup.Auth.cs* file.</span></span> <span data-ttu-id="2cb5c-163">ASP.NET 4.x-Web-app-Projekte mit Visual Studio 2013 erstellt und später die Katana-cookieauthentifizierungsmiddleware wird standardmäßig verwendet.</span><span class="sxs-lookup"><span data-stu-id="2cb5c-163">ASP.NET 4.x web app projects created with Visual Studio 2013 and later use the Katana cookie authentication middleware by default.</span></span>

> [!NOTE]
> <span data-ttu-id="2cb5c-164">Eine ASP.NET 4.x-app muss als Ziel .NET Framework 4.5.1 oder höher.</span><span class="sxs-lookup"><span data-stu-id="2cb5c-164">An ASP.NET 4.x app must target .NET Framework 4.5.1 or higher.</span></span> <span data-ttu-id="2cb5c-165">Andernfalls kann die nötigen NuGet-Pakete nicht installieren.</span><span class="sxs-lookup"><span data-stu-id="2cb5c-165">Otherwise, the necessary NuGet packages fail to install.</span></span>

<span data-ttu-id="2cb5c-166">Um Authentifizierungscookies zwischen ASP.NET 4.x und ASP.NET Core apps gemeinsam nutzen, konfigurieren Sie die ASP.NET Core-app zu, wie bereits erwähnt, und konfigurieren Sie der ASP.NET 4.x-apps, indem Sie die folgenden Schritte aus.</span><span class="sxs-lookup"><span data-stu-id="2cb5c-166">To share authentication cookies among ASP.NET 4.x apps and ASP.NET Core apps, configure the ASP.NET Core app as stated above, then configure the ASP.NET 4.x apps by following the steps below.</span></span>

1. <span data-ttu-id="2cb5c-167">Installieren Sie das Paket [Microsoft.Owin.Security.Interop](https://www.nuget.org/packages/Microsoft.Owin.Security.Interop/) in jeder ASP.NET 4.x-app.</span><span class="sxs-lookup"><span data-stu-id="2cb5c-167">Install the package [Microsoft.Owin.Security.Interop](https://www.nuget.org/packages/Microsoft.Owin.Security.Interop/) into each ASP.NET 4.x app.</span></span>

2. <span data-ttu-id="2cb5c-168">In *Startup.Auth.cs*, suchen Sie den Aufruf von `UseCookieAuthentication` und ändern Sie sie wie folgt.</span><span class="sxs-lookup"><span data-stu-id="2cb5c-168">In *Startup.Auth.cs*, locate the call to `UseCookieAuthentication` and modify it as follows.</span></span> <span data-ttu-id="2cb5c-169">Ändern Sie den Namen des Cookies mit dem Namen verwendet, die für die ASP.NET Core cookieauthentifizierungsmiddleware übereinstimmen.</span><span class="sxs-lookup"><span data-stu-id="2cb5c-169">Change the cookie name to match the name used by the ASP.NET Core cookie authentication middleware.</span></span> <span data-ttu-id="2cb5c-170">Geben Sie eine Instanz von einem `DataProtectionProvider` mit den allgemeinen Schutz Key Datenspeicherort initialisiert.</span><span class="sxs-lookup"><span data-stu-id="2cb5c-170">Provide an instance of a `DataProtectionProvider` initialized to the common data protection key storage location.</span></span> <span data-ttu-id="2cb5c-171">Stellen Sie sicher, dass der Name der app festgelegt ist, zur allgemeinen Namen der app verwendet, die für alle apps, die Cookies, freigeben `SharedCookieApp` in der Beispiel-app.</span><span class="sxs-lookup"><span data-stu-id="2cb5c-171">Make sure that the app name is set to the common app name used by all apps that share cookies, `SharedCookieApp` in the sample app.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="2cb5c-172">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="2cb5c-172">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

[!code-csharp[](cookie-sharing/sample/CookieAuthWithIdentity.NETFramework/CookieAuthWithIdentity.NETFramework/App_Start/Startup.Auth.cs?name=snippet1)]

<span data-ttu-id="2cb5c-173">Finden Sie unter der *CookieAuthWithIdentity.NETFramework* -Projekt in der [Beispielcode](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/cookie-sharing/sample/) ([zum Herunterladen von](xref:tutorials/index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="2cb5c-173">See the *CookieAuthWithIdentity.NETFramework* project in the [sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/cookie-sharing/sample/) ([how to download](xref:tutorials/index#how-to-download-a-sample)).</span></span>

<span data-ttu-id="2cb5c-174">Zur Erstellung eine Benutzeridentität übereinstimmen der Authentifizierungstyp definierten Datentyps in einer `AuthenticationType` set mit `UseCookieAuthentication`.</span><span class="sxs-lookup"><span data-stu-id="2cb5c-174">When generating a user identity, the authentication type must match the type defined in `AuthenticationType` set with `UseCookieAuthentication`.</span></span>

<span data-ttu-id="2cb5c-175">*Models/IdentityModels.cs*:</span><span class="sxs-lookup"><span data-stu-id="2cb5c-175">*Models/IdentityModels.cs*:</span></span>

[!code-csharp[](cookie-sharing/sample/CookieAuthWithIdentity.NETFramework/CookieAuthWithIdentity.NETFramework/Models/IdentityModels.cs?name=snippet1)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="2cb5c-176">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="2cb5c-176">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

<span data-ttu-id="2cb5c-177">Legen Sie die `CookieManager` für Interop `ChunkingCookieManager` damit das segmentierungsformat kompatibel ist.</span><span class="sxs-lookup"><span data-stu-id="2cb5c-177">Set the `CookieManager` to interop `ChunkingCookieManager` so the chunking format is compatible.</span></span>

```csharp
app.UseCookieAuthentication(new CookieAuthenticationOptions
{
    AuthenticationType = DefaultAuthenticationTypes.ApplicationCookie,
    CookieName = ".AspNetCore.Cookies",
    // CookieName = ".AspNetCore.ApplicationCookie", (if using ASP.NET Identity)
    // CookiePath = "...", (if necessary)
    // ...
    TicketDataFormat = new AspNetTicketDataFormat(
        new DataProtectorShim(
            DataProtectionProvider.Create(
                new DirectoryInfo(@"PATH_TO_KEY_RING_FOLDER"))
            .CreateProtector(
                "Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationMiddleware",
                "Cookies", 
                "v2"))),
    CookieManager = new ChunkingCookieManager()
});
```

---

## <a name="use-a-common-user-database"></a><span data-ttu-id="2cb5c-178">Eine allgemeine Benutzerdatenbank verwenden</span><span class="sxs-lookup"><span data-stu-id="2cb5c-178">Use a common user database</span></span>

<span data-ttu-id="2cb5c-179">Vergewissern Sie sich, dass das Identitätssystem für jede app in der gleichen Benutzerdatenbank gezeigt wird.</span><span class="sxs-lookup"><span data-stu-id="2cb5c-179">Confirm that the identity system for each app is pointed at the same user database.</span></span> <span data-ttu-id="2cb5c-180">Andernfalls führt das Identitätssystem Fehler zur Laufzeit, wenn er versucht, die Informationen in das Authentifizierungscookie anhand der Informationen in der Datenbank übereinstimmen.</span><span class="sxs-lookup"><span data-stu-id="2cb5c-180">Otherwise, the identity system produces failures at runtime when it attempts to match the information in the authentication cookie against the information in its database.</span></span>
