---
title: Konfigurieren des Datenschutzes in ASP.NET Core
author: rick-anderson
description: Informationen Sie zum Datenschutz in ASP.NET Core konfigurieren.
ms.author: riande
manager: wpickett
ms.date: 07/17/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/configuration/overview
ms.openlocfilehash: c19b1a9cba89a02420c8792b575827d439bc262b
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/19/2018
---
# <a name="configuring-data-protection-in-aspnet-core"></a><span data-ttu-id="2e9fc-103">Konfigurieren des Datenschutzes in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="2e9fc-103">Configuring Data Protection in ASP.NET Core</span></span>

<span data-ttu-id="2e9fc-104">Von [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="2e9fc-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="2e9fc-105">Wenn das System den Datenschutz initialisiert wird, gilt es [Standardeinstellungen](xref:security/data-protection/configuration/default-settings) basierend auf der betriebsumgebung.</span><span class="sxs-lookup"><span data-stu-id="2e9fc-105">When the Data Protection system is initialized, it applies [default settings](xref:security/data-protection/configuration/default-settings) based on the operational environment.</span></span> <span data-ttu-id="2e9fc-106">Diese Einstellungen sind im Allgemeinen geeignet für apps, die auf einem einzelnen Computer ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="2e9fc-106">These settings are generally appropriate for apps running on a single machine.</span></span> <span data-ttu-id="2e9fc-107">Es gibt Fälle, in denen ein Entwickler möchten vielleicht die Standardeinstellungen geändert werden, da es sich bei ihrer app auf mehrere Computer oder aus Compliance-Gründen verteilt wird.</span><span class="sxs-lookup"><span data-stu-id="2e9fc-107">There are cases where a developer may want to change the default settings, perhaps because their app is spread across multiple machines or for compliance reasons.</span></span> <span data-ttu-id="2e9fc-108">Für die folgenden Szenarien bietet das Data Protection-System eine umfangreiche Konfigurations-API.</span><span class="sxs-lookup"><span data-stu-id="2e9fc-108">For these scenarios, the Data Protection system offers a rich configuration API.</span></span>

<span data-ttu-id="2e9fc-109">Es ist eine Erweiterungsmethode [AddDataProtection](/dotnet/api/microsoft.extensions.dependencyinjection.dataprotectionservicecollectionextensions.adddataprotection) zurückgibt ein [IDataProtectionBuilder](/dotnet/api/microsoft.aspnetcore.dataprotection.idataprotectionbuilder).</span><span class="sxs-lookup"><span data-stu-id="2e9fc-109">There's an extension method [AddDataProtection](/dotnet/api/microsoft.extensions.dependencyinjection.dataprotectionservicecollectionextensions.adddataprotection) that returns an [IDataProtectionBuilder](/dotnet/api/microsoft.aspnetcore.dataprotection.idataprotectionbuilder).</span></span> <span data-ttu-id="2e9fc-110">`IDataProtectionBuilder`macht Erweiterungsmethoden, dass Sie miteinander Optionen verketten können so konfigurieren Sie den Schutz von Daten an.</span><span class="sxs-lookup"><span data-stu-id="2e9fc-110">`IDataProtectionBuilder` exposes extension methods that you can chain together to configure Data Protection options.</span></span>

## <a name="persistkeystofilesystem"></a><span data-ttu-id="2e9fc-111">PersistKeysToFileSystem</span><span class="sxs-lookup"><span data-stu-id="2e9fc-111">PersistKeysToFileSystem</span></span>

<span data-ttu-id="2e9fc-112">Zum Speichern von Schlüsseln auf einer UNC-Freigabe statt an den *%LocalAppData%* Standardspeicherort, konfigurieren Sie das System mit [PersistKeysToFileSystem](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystofilesystem):</span><span class="sxs-lookup"><span data-stu-id="2e9fc-112">To store keys on a UNC share instead of at the *%LOCALAPPDATA%* default location, configure the system with [PersistKeysToFileSystem](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystofilesystem):</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .PersistKeysToFileSystem(new DirectoryInfo(@"\\server\share\directory\"));
}
```

> [!WARNING]
> <span data-ttu-id="2e9fc-113">Wenn Sie den Speicherort der schlüsselpersistenz ändern, verschlüsselt das System nicht mehr automatisch Schlüssel im Ruhezustand, da er nicht weiß, ob eine entsprechende Verschlüsselungsmechanismus bei DPAPI handelt es.</span><span class="sxs-lookup"><span data-stu-id="2e9fc-113">If you change the key persistence location, the system no longer automatically encrypts keys at rest, since it doesn't know whether DPAPI is an appropriate encryption mechanism.</span></span>

## <a name="protectkeyswith"></a><span data-ttu-id="2e9fc-114">ProtectKeysWith\*</span><span class="sxs-lookup"><span data-stu-id="2e9fc-114">ProtectKeysWith\*</span></span>

<span data-ttu-id="2e9fc-115">Sie können konfigurieren, dass Schlüssel im Ruhezustand zu schützen, durch das Aufrufen einer der im System die [ProtectKeysWith\* ](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions) Konfigurations-APIs.</span><span class="sxs-lookup"><span data-stu-id="2e9fc-115">You can configure the system to protect keys at rest by calling any of the [ProtectKeysWith\*](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions) configuration APIs.</span></span> <span data-ttu-id="2e9fc-116">Betrachten Sie das folgenden Beispiel wird die Tasten auf einer UNC-Freigabe gespeichert und verschlüsselt den Schlüssel im Ruhezustand mit einem bestimmten x. 509-Zertifikat aus:</span><span class="sxs-lookup"><span data-stu-id="2e9fc-116">Consider the example below, which stores keys on a UNC share and encrypts those keys at rest with a specific X.509 certificate:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .PersistKeysToFileSystem(new DirectoryInfo(@"\\server\share\directory\"))
        .ProtectKeysWithCertificate("thumbprint");
}
```

<span data-ttu-id="2e9fc-117">Finden Sie unter [Schlüssel Verschlüsselung ruhender](xref:security/data-protection/implementation/key-encryption-at-rest) Weitere Beispiele und detaillierte Informationen zu den integrierten Schlüsselverschlüsselung Mechanismen.</span><span class="sxs-lookup"><span data-stu-id="2e9fc-117">See [Key Encryption At Rest](xref:security/data-protection/implementation/key-encryption-at-rest) for more examples and discussion on the built-in key encryption mechanisms.</span></span>

## <a name="setdefaultkeylifetime"></a><span data-ttu-id="2e9fc-118">SetDefaultKeyLifetime</span><span class="sxs-lookup"><span data-stu-id="2e9fc-118">SetDefaultKeyLifetime</span></span>

<span data-ttu-id="2e9fc-119">Verwenden Sie zum Konfigurieren der im System eine Lebensdauer von 14 Tagen verwenden Sie anstelle des standardmäßigen 90 Tage [SetDefaultKeyLifetime](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setdefaultkeylifetime):</span><span class="sxs-lookup"><span data-stu-id="2e9fc-119">To configure the system to use a key lifetime of 14 days instead of the default 90 days, use [SetDefaultKeyLifetime](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setdefaultkeylifetime):</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .SetDefaultKeyLifetime(TimeSpan.FromDays(14));
}
```

## <a name="setapplicationname"></a><span data-ttu-id="2e9fc-120">SetApplicationName</span><span class="sxs-lookup"><span data-stu-id="2e9fc-120">SetApplicationName</span></span>

<span data-ttu-id="2e9fc-121">Standardmäßig isoliert die Datenschutz-System apps von einem anderen, auch wenn sie die gleiche physischen Schlüssel Repository gemeinsam nutzen.</span><span class="sxs-lookup"><span data-stu-id="2e9fc-121">By default, the Data Protection system isolates apps from one another, even if they're sharing the same physical key repository.</span></span> <span data-ttu-id="2e9fc-122">Dadurch wird verhindert, dass die apps Grundlegendes zur jeweils anderen geschützten Nutzlasten.</span><span class="sxs-lookup"><span data-stu-id="2e9fc-122">This prevents the apps from understanding each other's protected payloads.</span></span> <span data-ttu-id="2e9fc-123">Zum Freigeben von geschützter Nutzlasten zwischen zwei Web-apps verwenden [SetApplicationName](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setapplicationname) mit demselben Wert für jede app:</span><span class="sxs-lookup"><span data-stu-id="2e9fc-123">To share protected payloads between two apps, use [SetApplicationName](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setapplicationname) with the same value for each app:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .SetApplicationName("shared app name");
}
```

## <a name="disableautomatickeygeneration"></a><span data-ttu-id="2e9fc-124">DisableAutomaticKeyGeneration</span><span class="sxs-lookup"><span data-stu-id="2e9fc-124">DisableAutomaticKeyGeneration</span></span>

<span data-ttu-id="2e9fc-125">Sie möglicherweise ein Szenario, in dem Sie nicht möchten eine app an (zum Erstellen neuer Schlüssel) Schlüssel automatisch zurücksetzen, wie sie Ablaufdatum nähern.</span><span class="sxs-lookup"><span data-stu-id="2e9fc-125">You may have a scenario where you don't want an app to automatically roll keys (create new keys) as they approach expiration.</span></span> <span data-ttu-id="2e9fc-126">Ein Beispiel hierfür ist möglicherweise apps, die in einer Beziehung primär/Sekundär, in dem nur die primäre app ist verantwortlich für schlüsselverwaltung Bedenken und sekundären apps haben einfach eine schreibgeschützte Ansicht des Rings Schlüssel einrichten.</span><span class="sxs-lookup"><span data-stu-id="2e9fc-126">One example of this might be apps set up in a primary/secondary relationship, where only the primary app is responsible for key management concerns and secondary apps simply have a read-only view of the key ring.</span></span> <span data-ttu-id="2e9fc-127">Die sekundären apps konfiguriert werden können um der Schlüssel Ring als schreibgeschützt zu behandeln, indem Sie die Konfiguration des Systems mit [DisableAutomaticKeyGeneration](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.disableautomatickeygeneration):</span><span class="sxs-lookup"><span data-stu-id="2e9fc-127">The secondary apps can be configured to treat the key ring as read-only by configuring the system with [DisableAutomaticKeyGeneration](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.disableautomatickeygeneration):</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .DisableAutomaticKeyGeneration();
}
```

## <a name="per-application-isolation"></a><span data-ttu-id="2e9fc-128">Pro Anwendungsisolation</span><span class="sxs-lookup"><span data-stu-id="2e9fc-128">Per-application isolation</span></span>

<span data-ttu-id="2e9fc-129">Wenn die Datenschutz-System von einem ASP.NET Core-Host bereitgestellt wird, isoliert es automatisch apps von einem anderen, selbst wenn diese apps mit der gleichen Arbeitsprozesskonto ausgeführt werden und die gleiche Schlüsselmaterial verwenden.</span><span class="sxs-lookup"><span data-stu-id="2e9fc-129">When the Data Protection system is provided by an ASP.NET Core host, it automatically isolates apps from one another, even if those apps are running under the same worker process account and are using the same master keying material.</span></span> <span data-ttu-id="2e9fc-130">Dies ist zumindest ähneln den IsolateApps-Modifizierer aus System.Web  **\<MachineKey >** Element.</span><span class="sxs-lookup"><span data-stu-id="2e9fc-130">This is somewhat similar to the IsolateApps modifier from System.Web's **\<machineKey>** element.</span></span>

<span data-ttu-id="2e9fc-131">Der Mechanismus für die Sicherheitsisolation Hinblick auf jede app auf dem lokalen Computer als einen eindeutigen Mandanten daher funktioniert die [IDataProtector](/dotnet/api/microsoft.aspnetcore.dataprotection.idataprotector) Stamm eine bestimmte app automatisch als Diskriminator app-ID enthält.</span><span class="sxs-lookup"><span data-stu-id="2e9fc-131">The isolation mechanism works by considering each app on the local machine as a unique tenant, thus the [IDataProtector](/dotnet/api/microsoft.aspnetcore.dataprotection.idataprotector) rooted for any given app automatically includes the app ID as a discriminator.</span></span> <span data-ttu-id="2e9fc-132">Eindeutige ID für die app stammt aus einer von zwei Stellen:</span><span class="sxs-lookup"><span data-stu-id="2e9fc-132">The app's unique ID comes from one of two places:</span></span>

1. <span data-ttu-id="2e9fc-133">Wenn die app in IIS gehostet wird, ist der eindeutige Bezeichner der app-Konfigurationspfad an.</span><span class="sxs-lookup"><span data-stu-id="2e9fc-133">If the app is hosted in IIS, the unique identifier is the app's configuration path.</span></span> <span data-ttu-id="2e9fc-134">Wenn eine app in einer webfarmumgebung bereitgestellt wird, sollte dieser Wert stabil sein, vorausgesetzt, dass die IIS-Umgebungen für alle Computer in der Webfarm auf ähnliche Weise konfiguriert werden.</span><span class="sxs-lookup"><span data-stu-id="2e9fc-134">If an app is deployed in a web farm environment, this value should be stable assuming that the IIS environments are configured similarly across all machines in the web farm.</span></span>

2. <span data-ttu-id="2e9fc-135">Wenn die app in IIS gehostet wird nicht, ist der eindeutige Bezeichner der physische Pfad der app an.</span><span class="sxs-lookup"><span data-stu-id="2e9fc-135">If the app isn't hosted in IIS, the unique identifier is the physical path of the app.</span></span>

<span data-ttu-id="2e9fc-136">Der eindeutige Bezeichner dient zum Zurücksetzen von Kennwörtern überstehen &mdash; sowohl der einzelnen app und der Computer.</span><span class="sxs-lookup"><span data-stu-id="2e9fc-136">The unique identifier is designed to survive resets &mdash; both of the individual app and of the machine itself.</span></span>

<span data-ttu-id="2e9fc-137">Dieser Mechanismus für die Sicherheitsisolation wird davon ausgegangen, dass die apps nicht bösartig sind.</span><span class="sxs-lookup"><span data-stu-id="2e9fc-137">This isolation mechanism assumes that the apps are not malicious.</span></span> <span data-ttu-id="2e9fc-138">Eine böswillige Anwendung beeinträchtigt immer jede andere app unter der gleichen Arbeitsprozesskonto ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="2e9fc-138">A malicious app can always impact any other app running under the same worker process account.</span></span> <span data-ttu-id="2e9fc-139">In einer freigegebenen Hostingumgebung nicht gegenseitig vertrauenswürdigen apps gegangenem geboten des hostenden Anbieters Schritte, um auf Betriebssystemebene Isolation zwischen apps, einschließlich trennen, die apps zugrunde liegenden Key Repositorys sicherzustellen.</span><span class="sxs-lookup"><span data-stu-id="2e9fc-139">In a shared hosting environment where apps are mutually untrusted, the hosting provider should take steps to ensure OS-level isolation between apps, including separating the apps' underlying key repositories.</span></span>

<span data-ttu-id="2e9fc-140">Wenn das System Schutz von Daten von einem ASP.NET Core-Host nicht bereitgestellt wird (z. B., wenn Sie auch über Instanziieren der `DataProtectionProvider` konkreten Typ) Isolieren von Apps ist standardmäßig deaktiviert.</span><span class="sxs-lookup"><span data-stu-id="2e9fc-140">If the Data Protection system isn't provided by an ASP.NET Core host (for example, if you instantiate it via the `DataProtectionProvider` concrete type) app isolation is disabled by default.</span></span> <span data-ttu-id="2e9fc-141">Beim Isolieren von Apps deaktiviert ist, alle apps, die durch das gleiche Schlüsselmaterial gesichert können freigeben Nutzlasten solange sie die entsprechende bieten [Zwecke](xref:security/data-protection/consumer-apis/purpose-strings).</span><span class="sxs-lookup"><span data-stu-id="2e9fc-141">When app isolation is disabled, all apps backed by the same keying material can share payloads as long as they provide the appropriate [purposes](xref:security/data-protection/consumer-apis/purpose-strings).</span></span> <span data-ttu-id="2e9fc-142">Zum Isolieren von Apps in dieser Umgebung bereitstellen möchten, rufen Sie die [SetApplicationName](#setapplicationname) Methode für die Konfiguration Objekt, und geben Sie einen eindeutigen Namen für jede app.</span><span class="sxs-lookup"><span data-stu-id="2e9fc-142">To provide app isolation in this environment, call the [SetApplicationName](#setapplicationname) method on the configuration object and provide a unique name for each app.</span></span>

## <a name="changing-algorithms-with-usecryptographicalgorithms"></a><span data-ttu-id="2e9fc-143">Algorithmen mit UseCryptographicAlgorithms ändern</span><span class="sxs-lookup"><span data-stu-id="2e9fc-143">Changing algorithms with UseCryptographicAlgorithms</span></span>

<span data-ttu-id="2e9fc-144">Der Datenschutz-Stapel können Sie ändern, der durch den neu generierten Schlüssel verwendete Standardalgorithmus.</span><span class="sxs-lookup"><span data-stu-id="2e9fc-144">The Data Protection stack allows you to change the default algorithm used by newly-generated keys.</span></span> <span data-ttu-id="2e9fc-145">Die einfachste Möglichkeit hierzu ist das Aufrufen [UseCryptographicAlgorithms](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.usecryptographicalgorithms) vom Rückruf Konfiguration:</span><span class="sxs-lookup"><span data-stu-id="2e9fc-145">The simplest way to do this is to call [UseCryptographicAlgorithms](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.usecryptographicalgorithms) from the configuration callback:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="2e9fc-146">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="2e9fc-146">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
services.AddDataProtection()
    .UseCryptographicAlgorithms(
        new AuthenticatedEncryptorConfiguration()
    {
        EncryptionAlgorithm = EncryptionAlgorithm.AES_256_CBC,
        ValidationAlgorithm = ValidationAlgorithm.HMACSHA256
    });
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="2e9fc-147">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="2e9fc-147">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
services.AddDataProtection()
    .UseCryptographicAlgorithms(
        new AuthenticatedEncryptionSettings()
    {
        EncryptionAlgorithm = EncryptionAlgorithm.AES_256_CBC,
        ValidationAlgorithm = ValidationAlgorithm.HMACSHA256
    });
```

---

<span data-ttu-id="2e9fc-148">Der Standardwert "EncryptionAlgorithm" ist AES-256-CBC, und der Standardwert ValidationAlgorithm ist HMACSHA256.</span><span class="sxs-lookup"><span data-stu-id="2e9fc-148">The default EncryptionAlgorithm is AES-256-CBC, and the default ValidationAlgorithm is HMACSHA256.</span></span> <span data-ttu-id="2e9fc-149">Die Standardrichtlinie kann festgelegt werden, von einem Systemadministrator über eine [computerweite Richtlinie](xref:security/data-protection/configuration/machine-wide-policy), aber ein expliziter Aufruf von `UseCryptographicAlgorithms` überschreibt der Standardrichtlinie.</span><span class="sxs-lookup"><span data-stu-id="2e9fc-149">The default policy can be set by a system administrator via a [machine-wide policy](xref:security/data-protection/configuration/machine-wide-policy), but an explicit call to `UseCryptographicAlgorithms` overrides the default policy.</span></span>

<span data-ttu-id="2e9fc-150">Aufrufen von `UseCryptographicAlgorithms` können Sie den gewünschten Algorithmus aus einer vordefinierten integrierten Liste angeben.</span><span class="sxs-lookup"><span data-stu-id="2e9fc-150">Calling `UseCryptographicAlgorithms` allows you to specify the desired algorithm from a predefined built-in list.</span></span> <span data-ttu-id="2e9fc-151">Sie müssen nicht die Implementierung des Algorithmus kümmern.</span><span class="sxs-lookup"><span data-stu-id="2e9fc-151">You don't need to worry about the implementation of the algorithm.</span></span> <span data-ttu-id="2e9fc-152">Im obigen Szenario versucht Datenschutz, der CNG-Implementierung von AES zu verwenden, wenn unter Windows ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="2e9fc-152">In the scenario above, the Data Protection system attempts to use the CNG implementation of AES if running on Windows.</span></span> <span data-ttu-id="2e9fc-153">Andernfalls es fragt die verwaltete [System.Security.Cryptography.Aes](/dotnet/api/system.security.cryptography.aes) Klasse.</span><span class="sxs-lookup"><span data-stu-id="2e9fc-153">Otherwise, it falls back to the managed [System.Security.Cryptography.Aes](/dotnet/api/system.security.cryptography.aes) class.</span></span>

<span data-ttu-id="2e9fc-154">Sie können manuell angeben, um über einen Aufruf an eine Implementierung [UseCustomCryptographicAlgorithms](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.usecustomcryptographicalgorithms).</span><span class="sxs-lookup"><span data-stu-id="2e9fc-154">You can manually specify an implementation via a call to [UseCustomCryptographicAlgorithms](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.usecustomcryptographicalgorithms).</span></span>

> [!TIP]
> <span data-ttu-id="2e9fc-155">Ändern die Algorithmen wirkt sich nicht auf vorhandene Schlüssel im Schlüssel Ring aus.</span><span class="sxs-lookup"><span data-stu-id="2e9fc-155">Changing algorithms doesn't affect existing keys in the key ring.</span></span> <span data-ttu-id="2e9fc-156">Er wirkt sich nur auf neu generierten Schlüssel.</span><span class="sxs-lookup"><span data-stu-id="2e9fc-156">It only affects newly-generated keys.</span></span>

### <a name="specifying-custom-managed-algorithms"></a><span data-ttu-id="2e9fc-157">Angeben von benutzerdefinierten verwalteten Algorithmen</span><span class="sxs-lookup"><span data-stu-id="2e9fc-157">Specifying custom managed algorithms</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="2e9fc-158">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="2e9fc-158">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="2e9fc-159">Um benutzerdefinierte Algorithmen, die verwaltete anzugeben, erstellen Sie eine [ManagedAuthenticatedEncryptorConfiguration](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.configurationmodel.managedauthenticatedencryptorconfiguration) Instanz, die auf die Implementierungstypen verweist:</span><span class="sxs-lookup"><span data-stu-id="2e9fc-159">To specify custom managed algorithms, create a [ManagedAuthenticatedEncryptorConfiguration](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.configurationmodel.managedauthenticatedencryptorconfiguration) instance that points to the implementation types:</span></span>

```csharp
serviceCollection.AddDataProtection()
    .UseCustomCryptographicAlgorithms(
        new ManagedAuthenticatedEncryptorConfiguration()
    {
        // A type that subclasses SymmetricAlgorithm
        EncryptionAlgorithmType = typeof(Aes),

        // Specified in bits
        EncryptionAlgorithmKeySize = 256,

        // A type that subclasses KeyedHashAlgorithm
        ValidationAlgorithmType = typeof(HMACSHA256)
    });
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="2e9fc-160">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="2e9fc-160">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="2e9fc-161">Um benutzerdefinierte Algorithmen, die verwaltete anzugeben, erstellen Sie eine [ManagedAuthenticatedEncryptionSettings](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.managedauthenticatedencryptionsettings) Instanz, die auf die Implementierungstypen verweist:</span><span class="sxs-lookup"><span data-stu-id="2e9fc-161">To specify custom managed algorithms, create a [ManagedAuthenticatedEncryptionSettings](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.managedauthenticatedencryptionsettings) instance that points to the implementation types:</span></span>

```csharp
serviceCollection.AddDataProtection()
    .UseCustomCryptographicAlgorithms(
        new ManagedAuthenticatedEncryptionSettings()
    {
        // A type that subclasses SymmetricAlgorithm
        EncryptionAlgorithmType = typeof(Aes),

        // Specified in bits
        EncryptionAlgorithmKeySize = 256,

        // A type that subclasses KeyedHashAlgorithm
        ValidationAlgorithmType = typeof(HMACSHA256)
    });
```

---

<span data-ttu-id="2e9fc-162">Im Allgemeinen die \*Typeigenschaften müssen auf konkrete, zeigen instanziierbaren (über einen öffentlichen parameterlosen Ctor) Implementierungen von [SymmetricAlgorithm](/dotnet/api/system.security.cryptography.symmetricalgorithm) und [KeyedHashAlgorithm](/dotnet/api/system.security.cryptography.keyedhashalgorithm), obwohl die System spezielle-Fällen einige Werte wie `typeof(Aes)` der Einfachheit halber.</span><span class="sxs-lookup"><span data-stu-id="2e9fc-162">Generally the \*Type properties must point to concrete, instantiable (via a public parameterless ctor) implementations of [SymmetricAlgorithm](/dotnet/api/system.security.cryptography.symmetricalgorithm) and [KeyedHashAlgorithm](/dotnet/api/system.security.cryptography.keyedhashalgorithm), though the system special-cases some values like `typeof(Aes)` for convenience.</span></span>

> [!NOTE]
> <span data-ttu-id="2e9fc-163">Die SymmetricAlgorithm muss eine Schlüssellänge von ≥ 128 Bits und eine Blockgröße von ≥ 64 Bits haben, und es muss CBC-Modus-Verschlüsselung mit PKCS #7-Auffüllung unterstützen.</span><span class="sxs-lookup"><span data-stu-id="2e9fc-163">The SymmetricAlgorithm must have a key length of ≥ 128 bits and a block size of ≥ 64 bits, and it must support CBC-mode encryption with PKCS #7 padding.</span></span> <span data-ttu-id="2e9fc-164">Der KeyedHashAlgorithm benötigen Nachrichtenhash Größe > = 128 Bits und müssen Schlüssel, der den Hashalgorithmus Digestlänge gleich Länge unterstützt.</span><span class="sxs-lookup"><span data-stu-id="2e9fc-164">The KeyedHashAlgorithm must have a digest size of >= 128 bits, and it must support keys of length equal to the hash algorithm's digest length.</span></span> <span data-ttu-id="2e9fc-165">Die KeyedHashAlgorithm ist nicht zwingend erforderlich, HMAC sein.</span><span class="sxs-lookup"><span data-stu-id="2e9fc-165">The KeyedHashAlgorithm is not strictly required to be HMAC.</span></span>

### <a name="specifying-custom-windows-cng-algorithms"></a><span data-ttu-id="2e9fc-166">Angeben von benutzerdefinierten Windows CNG-Algorithmen</span><span class="sxs-lookup"><span data-stu-id="2e9fc-166">Specifying custom Windows CNG algorithms</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="2e9fc-167">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="2e9fc-167">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="2e9fc-168">Um einen benutzerdefinierten Windows CNG-Algorithmus HMAC-Validierung mit Verschlüsselung im CBC-Modus anzugeben, erstellen eine [CngCbcAuthenticatedEncryptorConfiguration](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.configurationmodel.cngcbcauthenticatedencryptorconfiguration) Instanz, die die algorithmische Informationen enthält:</span><span class="sxs-lookup"><span data-stu-id="2e9fc-168">To specify a custom Windows CNG algorithm using CBC-mode encryption with HMAC validation, create a [CngCbcAuthenticatedEncryptorConfiguration](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.configurationmodel.cngcbcauthenticatedencryptorconfiguration) instance that contains the algorithmic information:</span></span>

```csharp
services.AddDataProtection()
    .UseCustomCryptographicAlgorithms(
        new CngCbcAuthenticatedEncryptorConfiguration()
    {
        // Passed to BCryptOpenAlgorithmProvider
        EncryptionAlgorithm = "AES",
        EncryptionAlgorithmProvider = null,

        // Specified in bits
        EncryptionAlgorithmKeySize = 256,

        // Passed to BCryptOpenAlgorithmProvider
        HashAlgorithm = "SHA256",
        HashAlgorithmProvider = null
    });
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="2e9fc-169">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="2e9fc-169">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="2e9fc-170">Um einen benutzerdefinierten Windows CNG-Algorithmus HMAC-Validierung mit Verschlüsselung im CBC-Modus anzugeben, erstellen eine [CngCbcAuthenticatedEncryptionSettings](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.cngcbcauthenticatedencryptionsettings) Instanz, die die algorithmische Informationen enthält:</span><span class="sxs-lookup"><span data-stu-id="2e9fc-170">To specify a custom Windows CNG algorithm using CBC-mode encryption with HMAC validation, create a [CngCbcAuthenticatedEncryptionSettings](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.cngcbcauthenticatedencryptionsettings) instance that contains the algorithmic information:</span></span>

```csharp
services.AddDataProtection()
    .UseCustomCryptographicAlgorithms(
        new CngCbcAuthenticatedEncryptionSettings()
    {
        // Passed to BCryptOpenAlgorithmProvider
        EncryptionAlgorithm = "AES",
        EncryptionAlgorithmProvider = null,

        // Specified in bits
        EncryptionAlgorithmKeySize = 256,

        // Passed to BCryptOpenAlgorithmProvider
        HashAlgorithm = "SHA256",
        HashAlgorithmProvider = null
    });
```

---

> [!NOTE]
> <span data-ttu-id="2e9fc-171">Der Block symmetrischen Verschlüsselungsalgorithmus benötigen eine Schlüssellänge von > = 128 Bits und eine Blockgröße von > = 64 Bits und müssen entdeckt CBC-Modus-Verschlüsselung mit PKCS #7-Auffüllung.</span><span class="sxs-lookup"><span data-stu-id="2e9fc-171">The symmetric block cipher algorithm must have a key length of >= 128 bits, a block size of >= 64 bits, and it must support CBC-mode encryption with PKCS #7 padding.</span></span> <span data-ttu-id="2e9fc-172">Der Hashalgorithmus benötigen Nachrichtenhash Größe von > = 128 Bits und müssen unterstützen geöffnet, die mit dem symmetrischen BCRYPT\_"alg"\_BEHANDELN\_HMAC\_FLAG-Flag.</span><span class="sxs-lookup"><span data-stu-id="2e9fc-172">The hash algorithm must have a digest size of >= 128 bits and must support being opened with the BCRYPT\_ALG\_HANDLE\_HMAC\_FLAG flag.</span></span> <span data-ttu-id="2e9fc-173">Die \*Anbietereigenschaften können werden auf null festgelegt, um den Standardanbieter für den angegebenen Algorithmus zu verwenden.</span><span class="sxs-lookup"><span data-stu-id="2e9fc-173">The \*Provider properties can be set to null to use the default provider for the specified algorithm.</span></span> <span data-ttu-id="2e9fc-174">Finden Sie unter der [BCryptOpenAlgorithmProvider](https://msdn.microsoft.com/library/windows/desktop/aa375479(v=vs.85).aspx) Dokumentation weitere Informationen.</span><span class="sxs-lookup"><span data-stu-id="2e9fc-174">See the [BCryptOpenAlgorithmProvider](https://msdn.microsoft.com/library/windows/desktop/aa375479(v=vs.85).aspx) documentation for more information.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="2e9fc-175">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="2e9fc-175">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="2e9fc-176">Erstellen Sie zum Angeben eines benutzerdefinierten Windows CNG-Algorithmus Überprüfung Galois/Counter-Modus-Verschlüsselung mit einem [CngGcmAuthenticatedEncryptorConfiguration](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.configurationmodel.cnggcmauthenticatedencryptorconfiguration) Instanz, die die algorithmische Informationen enthält:</span><span class="sxs-lookup"><span data-stu-id="2e9fc-176">To specify a custom Windows CNG algorithm using Galois/Counter Mode encryption with validation, create a [CngGcmAuthenticatedEncryptorConfiguration](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.configurationmodel.cnggcmauthenticatedencryptorconfiguration) instance that contains the algorithmic information:</span></span>

```csharp
services.AddDataProtection()
    .UseCustomCryptographicAlgorithms(
        new CngGcmAuthenticatedEncryptorConfiguration()
    {
        // Passed to BCryptOpenAlgorithmProvider
        EncryptionAlgorithm = "AES",
        EncryptionAlgorithmProvider = null,

        // Specified in bits
        EncryptionAlgorithmKeySize = 256
    });
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="2e9fc-177">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="2e9fc-177">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="2e9fc-178">Erstellen Sie zum Angeben eines benutzerdefinierten Windows CNG-Algorithmus Überprüfung Galois/Counter-Modus-Verschlüsselung mit einem [CngGcmAuthenticatedEncryptionSettings](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.cnggcmauthenticatedencryptionsettings) Instanz, die die algorithmische Informationen enthält:</span><span class="sxs-lookup"><span data-stu-id="2e9fc-178">To specify a custom Windows CNG algorithm using Galois/Counter Mode encryption with validation, create a [CngGcmAuthenticatedEncryptionSettings](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.cnggcmauthenticatedencryptionsettings) instance that contains the algorithmic information:</span></span>

```csharp
services.AddDataProtection()
    .UseCustomCryptographicAlgorithms(
        new CngGcmAuthenticatedEncryptionSettings()
    {
        // Passed to BCryptOpenAlgorithmProvider
        EncryptionAlgorithm = "AES",
        EncryptionAlgorithmProvider = null,

        // Specified in bits
        EncryptionAlgorithmKeySize = 256
    });
```

---

> [!NOTE]
> <span data-ttu-id="2e9fc-179">Der Block symmetrischen Verschlüsselungsalgorithmus benötigen eine Schlüssellänge von > = 128 Bits und eine Blockgröße von genau 128 Bits und müssen GCM-Verschlüsselung unterstützt.</span><span class="sxs-lookup"><span data-stu-id="2e9fc-179">The symmetric block cipher algorithm must have a key length of >= 128 bits, a block size of exactly 128 bits, and it must support GCM encryption.</span></span> <span data-ttu-id="2e9fc-180">Sie können festlegen, die [EncryptionAlgorithmProvider](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.configurationmodel.cngcbcauthenticatedencryptorconfiguration.encryptionalgorithmprovider) -Eigenschaft auf null, um den Standardanbieter für den angegebenen Algorithmus zu verwenden.</span><span class="sxs-lookup"><span data-stu-id="2e9fc-180">You can set the [EncryptionAlgorithmProvider](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.configurationmodel.cngcbcauthenticatedencryptorconfiguration.encryptionalgorithmprovider) property to null to use the default provider for the specified algorithm.</span></span> <span data-ttu-id="2e9fc-181">Finden Sie unter der [BCryptOpenAlgorithmProvider](https://msdn.microsoft.com/library/windows/desktop/aa375479(v=vs.85).aspx) Dokumentation weitere Informationen.</span><span class="sxs-lookup"><span data-stu-id="2e9fc-181">See the [BCryptOpenAlgorithmProvider](https://msdn.microsoft.com/library/windows/desktop/aa375479(v=vs.85).aspx) documentation for more information.</span></span>

### <a name="specifying-other-custom-algorithms"></a><span data-ttu-id="2e9fc-182">Andere benutzerdefinierten Algorithmen angeben</span><span class="sxs-lookup"><span data-stu-id="2e9fc-182">Specifying other custom algorithms</span></span>

<span data-ttu-id="2e9fc-183">Obwohl nicht als eine erstklassige-API verfügbar gemacht, ist das Data Protection-System extensible genug, um zu ermöglichen, nahezu jede Art von Algorithmus angibt.</span><span class="sxs-lookup"><span data-stu-id="2e9fc-183">Though not exposed as a first-class API, the Data Protection system is extensible enough to allow specifying almost any kind of algorithm.</span></span> <span data-ttu-id="2e9fc-184">Beispielsweise ist es möglich, alle Schlüssel in einem Hardwaresicherheitsmodul (HSM) enthaltenen zu behalten und eine benutzerdefinierte Implementierung des Kerns Ver- und Entschlüsselung Routinen bereitstellen.</span><span class="sxs-lookup"><span data-stu-id="2e9fc-184">For example, it's possible to keep all keys contained within a Hardware Security Module (HSM) and to provide a custom implementation of the core encryption and decryption routines.</span></span> <span data-ttu-id="2e9fc-185">Finden Sie unter [IAuthenticatedEncryptor](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.iauthenticatedencryptor) in [Core kryptografieerweiterbarkeit](xref:security/data-protection/extensibility/core-crypto) für Weitere Informationen.</span><span class="sxs-lookup"><span data-stu-id="2e9fc-185">See [IAuthenticatedEncryptor](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.iauthenticatedencryptor) in [Core cryptography extensibility](xref:security/data-protection/extensibility/core-crypto) for more information.</span></span>

## <a name="persisting-keys-when-hosting-in-a-docker-container"></a><span data-ttu-id="2e9fc-186">Beibehalten von Schlüsseln, wenn in einem Docker-Container gehostet wird.</span><span class="sxs-lookup"><span data-stu-id="2e9fc-186">Persisting keys when hosting in a Docker container</span></span>

<span data-ttu-id="2e9fc-187">Beim Hosten in einer [Docker](/dotnet/standard/microservices-architecture/container-docker-introduction/) Container Schlüssel sollte entweder beibehalten werden:</span><span class="sxs-lookup"><span data-stu-id="2e9fc-187">When hosting in a [Docker](/dotnet/standard/microservices-architecture/container-docker-introduction/) container, keys should be maintained in either:</span></span>

* <span data-ttu-id="2e9fc-188">Einen Ordner, der ein Docker-Volume ist, die nach Ablauf des Containers Lebensdauer, z. B. ein freigegebenes Volume oder einem Host bereitgestellten Volume beibehält.</span><span class="sxs-lookup"><span data-stu-id="2e9fc-188">A folder that's a Docker volume that persists beyond the container's lifetime, such as a shared volume or a host-mounted volume.</span></span>
* <span data-ttu-id="2e9fc-189">Ein externer Anbieter, wie z. B. [Azure Key Vault](https://azure.microsoft.com/services/key-vault/) oder [Redis](https://redis.io/).</span><span class="sxs-lookup"><span data-stu-id="2e9fc-189">An external provider, such as [Azure Key Vault](https://azure.microsoft.com/services/key-vault/) or [Redis](https://redis.io/).</span></span>

## <a name="see-also"></a><span data-ttu-id="2e9fc-190">Siehe auch</span><span class="sxs-lookup"><span data-stu-id="2e9fc-190">See also</span></span>

* [<span data-ttu-id="2e9fc-191">Szenarios, die die Abhängigkeitsinjektion nicht beachten</span><span class="sxs-lookup"><span data-stu-id="2e9fc-191">Non DI Aware Scenarios</span></span>](xref:security/data-protection/configuration/non-di-scenarios)
* [<span data-ttu-id="2e9fc-192">Computerübergreifende Richtlinie</span><span class="sxs-lookup"><span data-stu-id="2e9fc-192">Machine Wide Policy</span></span>](xref:security/data-protection/configuration/machine-wide-policy)
