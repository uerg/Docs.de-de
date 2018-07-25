---
title: Freigeben von Cookies zwischen apps mit ASP.NET und ASP.NET Core
author: rick-anderson
description: Informationen zum Freigeben des Authentifizierungscookies zwischen ASP.NET 4.x und ASP.NET Core-apps.
ms.author: riande
ms.custom: mvc
ms.date: 01/19/2017
uid: security/cookie-sharing
ms.openlocfilehash: ed3496db3f7a63a704f0e57faef6b2f6c085a8bd
ms.sourcegitcommit: b4c7b1a4c48dec0865f27874275c73da1f75e918
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/24/2018
ms.locfileid: "39228597"
---
# <a name="share-cookies-among-apps-with-aspnet-and-aspnet-core"></a><span data-ttu-id="2509c-103">Freigeben von Cookies zwischen apps mit ASP.NET und ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="2509c-103">Share cookies among apps with ASP.NET and ASP.NET Core</span></span>

<span data-ttu-id="2509c-104">Durch [Rick Anderson](https://twitter.com/RickAndMSFT) und [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="2509c-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="2509c-105">Websites bestehen häufig aus einzelner Web-apps, die zusammenarbeiten.</span><span class="sxs-lookup"><span data-stu-id="2509c-105">Websites often consist of individual web apps working together.</span></span> <span data-ttu-id="2509c-106">Um einen einmaligen Anmeldens (SSO) zu gewährleisten, müssen die Web-apps innerhalb eines Standorts Authentifizierungscookies freigeben.</span><span class="sxs-lookup"><span data-stu-id="2509c-106">To provide a single sign-on (SSO) experience, web apps within a site must share authentication cookies.</span></span> <span data-ttu-id="2509c-107">Zur Unterstützung dieses Szenarios kann die Stapel für den Schutz von Daten Freigeben von Katana-Cookie-Authentifizierung und ASP.NET Core-Cookie-Authentifizierungstickets.</span><span class="sxs-lookup"><span data-stu-id="2509c-107">To support this scenario, the data protection stack allows sharing Katana cookie authentication and ASP.NET Core cookie authentication tickets.</span></span>

<span data-ttu-id="2509c-108">[Anzeigen oder Herunterladen von Beispielcode](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/cookie-sharing/sample/) ([Vorgehensweise zum Herunterladen](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="2509c-108">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/cookie-sharing/sample/) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="2509c-109">Das Beispiel veranschaulicht die Cookies, die Freigabe über drei apps, die Cookie-Authentifizierung zu verwenden:</span><span class="sxs-lookup"><span data-stu-id="2509c-109">The sample illustrates cookie sharing across three apps that use cookie authentication:</span></span>

* <span data-ttu-id="2509c-110">ASP.NET Core 2.0-Razor-Seiten-app ohne [ASP.NET Core Identity](xref:security/authentication/identity)</span><span class="sxs-lookup"><span data-stu-id="2509c-110">ASP.NET Core 2.0 Razor Pages app without using [ASP.NET Core Identity](xref:security/authentication/identity)</span></span>
* <span data-ttu-id="2509c-111">ASP.NET Core 2.0 MVC-app mit ASP.NET Core-Identität</span><span class="sxs-lookup"><span data-stu-id="2509c-111">ASP.NET Core 2.0 MVC app with ASP.NET Core Identity</span></span>
* <span data-ttu-id="2509c-112">ASP.NET Framework 4.6.1-MVC-app mit ASP.NET Identity</span><span class="sxs-lookup"><span data-stu-id="2509c-112">ASP.NET Framework 4.6.1 MVC app with ASP.NET Identity</span></span>

<span data-ttu-id="2509c-113">In den folgenden Beispielen:</span><span class="sxs-lookup"><span data-stu-id="2509c-113">In the examples that follow:</span></span>

* <span data-ttu-id="2509c-114">Der Name des Cookies Authentifizierung festgelegt ist, um einen gemeinsamen Wert `.AspNet.SharedCookie`.</span><span class="sxs-lookup"><span data-stu-id="2509c-114">The authentication cookie name is set to a common value of `.AspNet.SharedCookie`.</span></span>
* <span data-ttu-id="2509c-115">Die `AuthenticationType` nastaven NA hodnotu `Identity.Application` entweder explizit oder standardmäßig.</span><span class="sxs-lookup"><span data-stu-id="2509c-115">The `AuthenticationType` is set to `Identity.Application` either explicitly or by default.</span></span>
* <span data-ttu-id="2509c-116">Ein allgemeine app-Name wird verwendet, aktivieren Sie das System zum Schutz von Daten zum Freigeben von Data Protection-Schlüssel (`SharedCookieApp`).</span><span class="sxs-lookup"><span data-stu-id="2509c-116">A common app name is used to enable the data protection system to share data protection keys (`SharedCookieApp`).</span></span>
* <span data-ttu-id="2509c-117">`Identity.Application` wird als das Authentifizierungsschema verwendet.</span><span class="sxs-lookup"><span data-stu-id="2509c-117">`Identity.Application` is used as the authentication scheme.</span></span> <span data-ttu-id="2509c-118">Alle Schema verwendet, müssen einheitlich verwendet werden *innerhalb und außerhalb von* die freigegebenen Cookie-apps als der Standard-Schema oder indem Sie ihn explizit festlegen.</span><span class="sxs-lookup"><span data-stu-id="2509c-118">Whatever scheme is used, it must be used consistently *within and across* the shared cookie apps either as the default scheme or by explicitly setting it.</span></span> <span data-ttu-id="2509c-119">Das Schema wird verwendet, beim Ver- und Entschlüsslung von Cookies, damit ein konsistentes Schema für apps verwendet werden muss.</span><span class="sxs-lookup"><span data-stu-id="2509c-119">The scheme is used when encrypting and decrypting cookies, so a consistent scheme must be used across apps.</span></span>
* <span data-ttu-id="2509c-120">Eine allgemeine [Data Protection-Schlüssels](xref:security/data-protection/implementation/key-management) Speicherort verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="2509c-120">A common [data protection key](xref:security/data-protection/implementation/key-management) storage location is used.</span></span> <span data-ttu-id="2509c-121">Die Beispiel-app verwendet einen Ordner namens *Schlüsselbund* am Stamm der Projektmappe, die die Schlüssel zum Schutz von Daten enthalten soll.</span><span class="sxs-lookup"><span data-stu-id="2509c-121">The sample app uses a folder named *KeyRing* at the root of the solution to hold the data protection keys.</span></span>
* <span data-ttu-id="2509c-122">In den ASP.NET Core-apps [PersistKeysToFileSystem](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystofilesystem) wird verwendet, um den Schlüsselspeicher Speicherort festzulegen.</span><span class="sxs-lookup"><span data-stu-id="2509c-122">In the ASP.NET Core apps, [PersistKeysToFileSystem](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystofilesystem) is used to set the key storage location.</span></span> <span data-ttu-id="2509c-123">[SetApplicationName](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setapplicationname) wird verwendet, um einen allgemeinen Namen für die freigegebene app konfigurieren.</span><span class="sxs-lookup"><span data-stu-id="2509c-123">[SetApplicationName](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setapplicationname) is used to configure a common shared app name.</span></span>
* <span data-ttu-id="2509c-124">In der .NET Framework-app verwendet die cookieauthentifizierungs-Middleware eine Implementierung des [DataProtectionProvider](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionprovider).</span><span class="sxs-lookup"><span data-stu-id="2509c-124">In the .NET Framework app, the cookie authentication middleware uses an implementation of [DataProtectionProvider](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionprovider).</span></span> <span data-ttu-id="2509c-125">`DataProtectionProvider` enthält Data Protection-Diensten für die Ver- und Entschlüsselung von Authentifizierungsdaten Cookie-Nutzlast an.</span><span class="sxs-lookup"><span data-stu-id="2509c-125">`DataProtectionProvider` provides data protection services for the encryption and decryption of authentication cookie payload data.</span></span> <span data-ttu-id="2509c-126">Die `DataProtectionProvider` Instanz wird isoliert vom System zum Schutz von Daten, die von anderen Teilen der app verwendet.</span><span class="sxs-lookup"><span data-stu-id="2509c-126">The `DataProtectionProvider` instance is isolated from the data protection system used by other parts of the app.</span></span>
  * <span data-ttu-id="2509c-127">[DataProtectionProvider.Create (System.IO.DirectoryInfo, Aktion\<IDataProtectionBuilder >)](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionprovider.create?view=aspnetcore-2.0#Microsoft_AspNetCore_DataProtection_DataProtectionProvider_Create_System_IO_DirectoryInfo_System_Action_Microsoft_AspNetCore_DataProtection_IDataProtectionBuilder__) akzeptiert eine [DirectoryInfo](/dotnet/api/system.io.directoryinfo) den Speicherort für die datenspeicherung für Schutz an.</span><span class="sxs-lookup"><span data-stu-id="2509c-127">[DataProtectionProvider.Create(System.IO.DirectoryInfo, Action\<IDataProtectionBuilder>)](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionprovider.create?view=aspnetcore-2.0#Microsoft_AspNetCore_DataProtection_DataProtectionProvider_Create_System_IO_DirectoryInfo_System_Action_Microsoft_AspNetCore_DataProtection_IDataProtectionBuilder__) accepts a [DirectoryInfo](/dotnet/api/system.io.directoryinfo) to specify the location for data protection key storage.</span></span> <span data-ttu-id="2509c-128">Die Beispiel-app stellt den Pfad zu der *Schlüsselbund* Ordner `DirectoryInfo`.</span><span class="sxs-lookup"><span data-stu-id="2509c-128">The sample app provides the path of the *KeyRing* folder to `DirectoryInfo`.</span></span> <span data-ttu-id="2509c-129">[DataProtectionBuilderExtensions.SetApplicationName](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setapplicationname?view=aspnetcore-2.0#Microsoft_AspNetCore_DataProtection_DataProtectionBuilderExtensions_SetApplicationName_Microsoft_AspNetCore_DataProtection_IDataProtectionBuilder_System_String_) sets the common app name.</span><span class="sxs-lookup"><span data-stu-id="2509c-129">[DataProtectionBuilderExtensions.SetApplicationName](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setapplicationname?view=aspnetcore-2.0#Microsoft_AspNetCore_DataProtection_DataProtectionBuilderExtensions_SetApplicationName_Microsoft_AspNetCore_DataProtection_IDataProtectionBuilder_System_String_) sets the common app name.</span></span>
  * <span data-ttu-id="2509c-130">[DataProtectionProvider](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionprovider) erfordert die [Microsoft.AspNetCore.DataProtection.Extensions](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.Extensions/) NuGet-Paket.</span><span class="sxs-lookup"><span data-stu-id="2509c-130">[DataProtectionProvider](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionprovider) requires the [Microsoft.AspNetCore.DataProtection.Extensions](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.Extensions/) NuGet package.</span></span> <span data-ttu-id="2509c-131">Um dieses Paket für ASP.NET Core 2.1 und höher apps zu erhalten, verweisen die [Microsoft.AspNetCore.App metapaket](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="2509c-131">To obtain this package for ASP.NET Core 2.1 and later apps, reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span> <span data-ttu-id="2509c-132">Wenn die .NET Framework abzielen, fügen Sie einen Paketverweis auf `Microsoft.AspNetCore.DataProtection.Extensions`.</span><span class="sxs-lookup"><span data-stu-id="2509c-132">When targeting the .NET Framework, add a package reference to `Microsoft.AspNetCore.DataProtection.Extensions`.</span></span>

## <a name="share-authentication-cookies-among-aspnet-core-apps"></a><span data-ttu-id="2509c-133">Authentifizierungscookies zwischen ASP.NET Core-apps freigeben</span><span class="sxs-lookup"><span data-stu-id="2509c-133">Share authentication cookies among ASP.NET Core apps</span></span>

<span data-ttu-id="2509c-134">Bei Verwendung von ASP.NET Core Identity:</span><span class="sxs-lookup"><span data-stu-id="2509c-134">When using ASP.NET Core Identity:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="2509c-135">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="2509c-135">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

<span data-ttu-id="2509c-136">In der `ConfigureServices` -Methode, mit der [ConfigureApplicationCookie](/dotnet/api/microsoft.extensions.dependencyinjection.identityservicecollectionextensions.configureapplicationcookie) Erweiterungsmethode, um den Datenschutzdienst für Cookies einzurichten.</span><span class="sxs-lookup"><span data-stu-id="2509c-136">In the `ConfigureServices` method, use the [ConfigureApplicationCookie](/dotnet/api/microsoft.extensions.dependencyinjection.identityservicecollectionextensions.configureapplicationcookie) extension method to set up the data protection service for cookies.</span></span>

[!code-csharp[](cookie-sharing/sample/CookieAuthWithIdentity.Core/Startup.cs?name=snippet1)]

<span data-ttu-id="2509c-137">Data Protection-Schlüssel und app-Name müssen zwischen apps freigegeben werden.</span><span class="sxs-lookup"><span data-stu-id="2509c-137">Data protection keys and the app name must be shared among apps.</span></span> <span data-ttu-id="2509c-138">In den beispielapps `GetKeyRingDirInfo` gibt den allgemeinen Schlüssel Speicherort, der [PersistKeysToFileSystem](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystofilesystem) Methode.</span><span class="sxs-lookup"><span data-stu-id="2509c-138">In the sample apps, `GetKeyRingDirInfo` returns the common key storage location to the [PersistKeysToFileSystem](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystofilesystem) method.</span></span> <span data-ttu-id="2509c-139">Verwendung [SetApplicationName](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setapplicationname) so konfigurieren Sie einen allgemeinen Namen für die freigegebene app (`SharedCookieApp` im Beispiel).</span><span class="sxs-lookup"><span data-stu-id="2509c-139">Use [SetApplicationName](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setapplicationname) to configure a common shared app name (`SharedCookieApp` in the sample).</span></span> <span data-ttu-id="2509c-140">Weitere Informationen finden Sie unter [Konfigurieren von Data Protection](xref:security/data-protection/configuration/overview).</span><span class="sxs-lookup"><span data-stu-id="2509c-140">For more information, see [Configuring Data Protection](xref:security/data-protection/configuration/overview).</span></span>

<span data-ttu-id="2509c-141">Beim Hosten von apps, die Cookies für Unterdomänen freigeben, geben Sie eine gemeinsame Domäne in der [Cookie.Domain](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.domain) Eigenschaft.</span><span class="sxs-lookup"><span data-stu-id="2509c-141">When hosting apps that share cookies across subdomains, specify a common domain in the [Cookie.Domain](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.domain) property.</span></span> <span data-ttu-id="2509c-142">Zum Freigeben von Cookies für apps im `contoso.com`, z. B. `first_subdomain.contoso.com` und `second_subdomain.contoso.com`, geben Sie die `Cookie.Domain` als `.contoso.com`:</span><span class="sxs-lookup"><span data-stu-id="2509c-142">To share cookies across apps at `contoso.com`, such as `first_subdomain.contoso.com` and `second_subdomain.contoso.com`, specify the `Cookie.Domain` as `.contoso.com`:</span></span>

```csharp
options.Cookie.Domain = ".contoso.com";
```

<span data-ttu-id="2509c-143">Finden Sie unter den *CookieAuthWithIdentity.Core* -Projekt in der [Beispielcode](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/cookie-sharing/sample/) ([herunterladen](xref:tutorials/index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="2509c-143">See the *CookieAuthWithIdentity.Core* project in the [sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/cookie-sharing/sample/) ([how to download](xref:tutorials/index#how-to-download-a-sample)).</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="2509c-144">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="2509c-144">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

<span data-ttu-id="2509c-145">In der `Configure` -Methode, mit der [CookieAuthenticationOptions](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions) einrichten:</span><span class="sxs-lookup"><span data-stu-id="2509c-145">In the `Configure` method, use the [CookieAuthenticationOptions](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions) to set up:</span></span>

* <span data-ttu-id="2509c-146">Den Datenschutzdienst für Cookies.</span><span class="sxs-lookup"><span data-stu-id="2509c-146">The data protection service for cookies.</span></span>
* <span data-ttu-id="2509c-147">Die `AuthenticationScheme` entsprechend der ASP.NET 4.x.</span><span class="sxs-lookup"><span data-stu-id="2509c-147">The `AuthenticationScheme` to match ASP.NET 4.x.</span></span>

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

<span data-ttu-id="2509c-148">Wenn Sie Cookies direkt verwenden:</span><span class="sxs-lookup"><span data-stu-id="2509c-148">When using cookies directly:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="2509c-149">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="2509c-149">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

[!code-csharp[](cookie-sharing/sample/CookieAuth.Core/Startup.cs?name=snippet1)]

<span data-ttu-id="2509c-150">Data Protection-Schlüssel und app-Name müssen zwischen apps freigegeben werden.</span><span class="sxs-lookup"><span data-stu-id="2509c-150">Data protection keys and the app name must be shared among apps.</span></span> <span data-ttu-id="2509c-151">In den beispielapps `GetKeyRingDirInfo` gibt den allgemeinen Schlüssel Speicherort, der [PersistKeysToFileSystem](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystofilesystem) Methode.</span><span class="sxs-lookup"><span data-stu-id="2509c-151">In the sample apps, `GetKeyRingDirInfo` returns the common key storage location to the [PersistKeysToFileSystem](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystofilesystem) method.</span></span> <span data-ttu-id="2509c-152">Verwendung [SetApplicationName](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setapplicationname) so konfigurieren Sie einen allgemeinen Namen für die freigegebene app (`SharedCookieApp` im Beispiel).</span><span class="sxs-lookup"><span data-stu-id="2509c-152">Use [SetApplicationName](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setapplicationname) to configure a common shared app name (`SharedCookieApp` in the sample).</span></span> <span data-ttu-id="2509c-153">Weitere Informationen finden Sie unter [Konfigurieren von Data Protection](xref:security/data-protection/configuration/overview).</span><span class="sxs-lookup"><span data-stu-id="2509c-153">For more information, see [Configuring Data Protection](xref:security/data-protection/configuration/overview).</span></span>

<span data-ttu-id="2509c-154">Beim Hosten von apps, die Cookies für Unterdomänen freigeben, geben Sie eine gemeinsame Domäne in der [Cookie.Domain](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.domain) Eigenschaft.</span><span class="sxs-lookup"><span data-stu-id="2509c-154">When hosting apps that share cookies across subdomains, specify a common domain in the [Cookie.Domain](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.domain) property.</span></span> <span data-ttu-id="2509c-155">Zum Freigeben von Cookies für apps im `contoso.com`, z. B. `first_subdomain.contoso.com` und `second_subdomain.contoso.com`, geben Sie die `Cookie.Domain` als `.contoso.com`:</span><span class="sxs-lookup"><span data-stu-id="2509c-155">To share cookies across apps at `contoso.com`, such as `first_subdomain.contoso.com` and `second_subdomain.contoso.com`, specify the `Cookie.Domain` as `.contoso.com`:</span></span>

```csharp
options.Cookie.Domain = ".contoso.com";
```

<span data-ttu-id="2509c-156">Finden Sie unter den *CookieAuth.Core* -Projekt in der [Beispielcode](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/cookie-sharing/sample/) ([herunterladen](xref:tutorials/index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="2509c-156">See the *CookieAuth.Core* project in the [sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/cookie-sharing/sample/) ([how to download](xref:tutorials/index#how-to-download-a-sample)).</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="2509c-157">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="2509c-157">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

```csharp
app.UseCookieAuthentication(new CookieAuthenticationOptions
{
    DataProtectionProvider = 
        DataProtectionProvider.Create(
            new DirectoryInfo(@"PATH_TO_KEY_RING_FOLDER"))
});
```

---

## <a name="encrypting-data-protection-keys-at-rest"></a><span data-ttu-id="2509c-158">Data Protection-Schlüssel im Ruhezustand verschlüsselt</span><span class="sxs-lookup"><span data-stu-id="2509c-158">Encrypting data protection keys at rest</span></span>

<span data-ttu-id="2509c-159">Konfigurieren Sie bei produktionsbereitstellungen die `DataProtectionProvider` Schlüssel im Ruhezustand mit DPAPI oder ein X509Certificate verschlüsselt.</span><span class="sxs-lookup"><span data-stu-id="2509c-159">For production deployments, configure the `DataProtectionProvider` to encrypt keys at rest with DPAPI or an X509Certificate.</span></span> <span data-ttu-id="2509c-160">Finden Sie unter [Schlüssel Verschlüsselung ruhender](xref:security/data-protection/implementation/key-encryption-at-rest) für Weitere Informationen.</span><span class="sxs-lookup"><span data-stu-id="2509c-160">See [Key Encryption At Rest](xref:security/data-protection/implementation/key-encryption-at-rest) for more information.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="2509c-161">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="2509c-161">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
services.AddDataProtection()
    .ProtectKeysWithCertificate("thumbprint");
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="2509c-162">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="2509c-162">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

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

## <a name="sharing-authentication-cookies-between-aspnet-4x-and-aspnet-core-apps"></a><span data-ttu-id="2509c-163">Freigeben von Authentifizierungscookies zwischen ASP.NET 4.x und ASP.NET Core-apps</span><span class="sxs-lookup"><span data-stu-id="2509c-163">Sharing authentication cookies between ASP.NET 4.x and ASP.NET Core apps</span></span>

<span data-ttu-id="2509c-164">ASP.NET 4.x-apps, die Katana-Cookie-Authentifizierung-Middleware verwenden können konfiguriert werden, um Authentifizierungscookies zu generieren, die mit ASP.NET Core cookieauthentifizierungsmiddleware kompatibel sind.</span><span class="sxs-lookup"><span data-stu-id="2509c-164">ASP.NET 4.x apps which use Katana cookie authentication middleware can be configured to generate authentication cookies that are compatible with the ASP.NET Core cookie authentication middleware.</span></span> <span data-ttu-id="2509c-165">Dadurch wird eine große Site einzelne apps schrittweise und gleichzeitig eine reibungslos funktionierende Umgebung für SSO für den Standort aktualisieren.</span><span class="sxs-lookup"><span data-stu-id="2509c-165">This allows upgrading a large site's individual apps piecemeal while providing a smooth SSO experience across the site.</span></span>

<span data-ttu-id="2509c-166">Wenn eine app Katana cookieauthentifizierungs-Middleware verwendet wird, ruft er `UseCookieAuthentication` des Projekts *Startup.Auth.cs* Datei.</span><span class="sxs-lookup"><span data-stu-id="2509c-166">When an app uses Katana cookie authentication middleware, it calls `UseCookieAuthentication` in the project's *Startup.Auth.cs* file.</span></span> <span data-ttu-id="2509c-167">ASP.NET 4.x-Web-app-Projekten mit Visual Studio 2013 erstellt und später die Katana cookieauthentifizierungs-Middleware standardmäßig verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="2509c-167">ASP.NET 4.x web app projects created with Visual Studio 2013 and later use the Katana cookie authentication middleware by default.</span></span> <span data-ttu-id="2509c-168">Obwohl `UseCookieAuthentication` ist veraltet und wird nicht unterstützt für ASP.NET Core-apps Aufrufen `UseCookieAuthentication` in einer ASP.NET 4.x-app, die Katana verwendet cookieauthentifizierungsmiddleware gültig ist.</span><span class="sxs-lookup"><span data-stu-id="2509c-168">Although `UseCookieAuthentication` is obsolete and unsupported for ASP.NET Core apps, calling `UseCookieAuthentication` in an ASP.NET 4.x app that uses Katana cookie authentication middleware is valid.</span></span>

<span data-ttu-id="2509c-169">Eine ASP.NET 4.x-app muss als Ziel .NET Framework 4.5.1 oder höher.</span><span class="sxs-lookup"><span data-stu-id="2509c-169">An ASP.NET 4.x app must target .NET Framework 4.5.1 or higher.</span></span> <span data-ttu-id="2509c-170">Anderenfalls nicht die erforderlichen NuGet-Pakete installiert.</span><span class="sxs-lookup"><span data-stu-id="2509c-170">Otherwise, the necessary NuGet packages fail to install.</span></span>

<span data-ttu-id="2509c-171">Wenn Authentifizierungscookies zwischen einer ASP.NET 4.x-app und einer ASP.NET Core-app freigeben möchten, konfigurieren Sie die ASP.NET Core-app aus, wie bereits erwähnt, und konfigurieren Sie die ASP.NET 4.x-app, indem Sie folgende Schritte:</span><span class="sxs-lookup"><span data-stu-id="2509c-171">To share authentication cookies between an ASP.NET 4.x app and an ASP.NET Core app, configure the ASP.NET Core app as stated above, then configure the ASP.NET 4.x app by following these steps:</span></span>

1. <span data-ttu-id="2509c-172">Installieren Sie das Paket [Microsoft.Owin.Security.Interop](https://www.nuget.org/packages/Microsoft.Owin.Security.Interop/) in jeder ASP.NET 4.x-app.</span><span class="sxs-lookup"><span data-stu-id="2509c-172">Install the package [Microsoft.Owin.Security.Interop](https://www.nuget.org/packages/Microsoft.Owin.Security.Interop/) into each ASP.NET 4.x app.</span></span>

2. <span data-ttu-id="2509c-173">In *Startup.Auth.cs*, suchen Sie den Aufruf von `UseCookieAuthentication` und ändern Sie sie wie folgt.</span><span class="sxs-lookup"><span data-stu-id="2509c-173">In *Startup.Auth.cs*, locate the call to `UseCookieAuthentication` and modify it as follows.</span></span> <span data-ttu-id="2509c-174">Ändern Sie den Namen des Cookies mit dem Namen ein, die die ASP.NET Core cookieauthentifizierungs-Middleware übereinstimmen.</span><span class="sxs-lookup"><span data-stu-id="2509c-174">Change the cookie name to match the name used by the ASP.NET Core cookie authentication middleware.</span></span> <span data-ttu-id="2509c-175">Geben Sie eine Instanz von einer `DataProtectionProvider` mit den allgemeinen Schutz Key Datenspeicherort initialisiert.</span><span class="sxs-lookup"><span data-stu-id="2509c-175">Provide an instance of a `DataProtectionProvider` initialized to the common data protection key storage location.</span></span> <span data-ttu-id="2509c-176">Stellen Sie sicher, dass der Name der app, auf die allgemeiner app-Name festgelegt ist, der alle Apps, die Cookies, freigeben `SharedCookieApp` in der Beispiel-app.</span><span class="sxs-lookup"><span data-stu-id="2509c-176">Make sure that the app name is set to the common app name used by all apps that share cookies, `SharedCookieApp` in the sample app.</span></span>

[!code-csharp[](cookie-sharing/sample/CookieAuthWithIdentity.NETFramework/CookieAuthWithIdentity.NETFramework/App_Start/Startup.Auth.cs?name=snippet1)]

<span data-ttu-id="2509c-177">Finden Sie unter den *CookieAuthWithIdentity.NETFramework* -Projekt in der [Beispielcode](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/cookie-sharing/sample/) ([herunterladen](xref:tutorials/index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="2509c-177">See the *CookieAuthWithIdentity.NETFramework* project in the [sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/cookie-sharing/sample/) ([how to download](xref:tutorials/index#how-to-download-a-sample)).</span></span>

<span data-ttu-id="2509c-178">Zur Erstellung eine Benutzeridentität muss der Authentifizierungstyp in definierten Typ übereinstimmen `AuthenticationType` festgelegt `UseCookieAuthentication`.</span><span class="sxs-lookup"><span data-stu-id="2509c-178">When generating a user identity, the authentication type must match the type defined in `AuthenticationType` set with `UseCookieAuthentication`.</span></span>

<span data-ttu-id="2509c-179">*Models/IdentityModels.cs*:</span><span class="sxs-lookup"><span data-stu-id="2509c-179">*Models/IdentityModels.cs*:</span></span>

[!code-csharp[](cookie-sharing/sample/CookieAuthWithIdentity.NETFramework/CookieAuthWithIdentity.NETFramework/Models/IdentityModels.cs?name=snippet1)]

## <a name="use-a-common-user-database"></a><span data-ttu-id="2509c-180">Verwenden Sie eine allgemeine Benutzerdatenbank</span><span class="sxs-lookup"><span data-stu-id="2509c-180">Use a common user database</span></span>

<span data-ttu-id="2509c-181">Vergewissern Sie sich, dass das Identitätssystem für jede app in der gleichen Benutzerdatenbank gezeigt wird.</span><span class="sxs-lookup"><span data-stu-id="2509c-181">Confirm that the identity system for each app is pointed at the same user database.</span></span> <span data-ttu-id="2509c-182">Andernfalls erzeugt das Identitätssystem Fehler zur Laufzeit auf, wenn er versucht, die Informationen in das Authentifizierungscookie anhand der Informationen in der Datenbank übereinstimmen.</span><span class="sxs-lookup"><span data-stu-id="2509c-182">Otherwise, the identity system produces failures at runtime when it attempts to match the information in the authentication cookie against the information in its database.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="2509c-183">Zusätzliche Ressourcen</span><span class="sxs-lookup"><span data-stu-id="2509c-183">Additional resources</span></span>

<xref:host-and-deploy/web-farm>
