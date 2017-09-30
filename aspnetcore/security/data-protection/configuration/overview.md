---
title: Konfigurieren des Datenschutzes
author: rick-anderson
description: 
keywords: ASP.NET Core,
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 0e4881a3-a94d-4e35-9c1c-f025d65dcff0
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/configuration/overview
ms.openlocfilehash: 9361dcec89a0f35067181523cc56637d629614ff
ms.sourcegitcommit: 6e83c55eb0450a3073ef2b95fa5f5bcb20dbbf89
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/28/2017
---
# <a name="configuring-data-protection"></a><span data-ttu-id="7fd5f-103">Konfigurieren des Datenschutzes</span><span class="sxs-lookup"><span data-stu-id="7fd5f-103">Configuring data protection</span></span>

<a name=data-protection-configuring></a>

<span data-ttu-id="7fd5f-104">Bei der Initialisierung der Datenschutzsystem gilt es einige [Standardeinstellungen](default-settings.md#data-protection-default-settings) basierend auf der betriebsumgebung.</span><span class="sxs-lookup"><span data-stu-id="7fd5f-104">When the data protection system is initialized it applies some [default settings](default-settings.md#data-protection-default-settings) based on the operational environment.</span></span> <span data-ttu-id="7fd5f-105">Diese Einstellungen sind im Allgemeinen gut für Anwendungen, die auf einem einzelnen Computer ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="7fd5f-105">These settings are generally good for applications running on a single machine.</span></span> <span data-ttu-id="7fd5f-106">Es gibt einige Fälle, in denen ein Entwickler möchte diese ändern (z. B. da ihre Anwendung auf mehrere Computer oder aus Compliance-Gründen verteilt wird), und für diese Szenarien die Datenschutzsystem bietet eine umfangreiche API-Konfiguration.</span><span class="sxs-lookup"><span data-stu-id="7fd5f-106">There are some cases where a developer may want to change these (perhaps because their application is spread across multiple machines or for compliance reasons), and for these scenarios the data protection system offers a rich configuration API.</span></span>

<a name=data-protection-configuration-callback></a>

<span data-ttu-id="7fd5f-107">Es ist eine Erweiterungsmethode AddDataProtection ein IDataProtectionBuilder zurück die selbst macht Erweiterungsmethoden, dass Sie miteinander Optionen verketten können so konfigurieren Sie verschiedene Datenschutz.</span><span class="sxs-lookup"><span data-stu-id="7fd5f-107">There is an extension method AddDataProtection which returns an IDataProtectionBuilder which itself exposes extension methods that you can chain together to configure various data protection options.</span></span> <span data-ttu-id="7fd5f-108">Z. B. zum Speichern von Schlüsseln auf einer UNC-Freigabe statt %LocalAppData% (Standard), konfigurieren Sie das System wie folgt:</span><span class="sxs-lookup"><span data-stu-id="7fd5f-108">For instance, to store keys at a UNC share instead of %LOCALAPPDATA% (the default), configure the system as follows:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .PersistKeysToFileSystem(new DirectoryInfo(@"\\server\share\directory\"));
}
```

>[!WARNING]
> <span data-ttu-id="7fd5f-109">Wenn Sie den Speicherort der schlüsselpersistenz ändern, wird das System nicht mehr automatisch verschlüsseln Schlüssel im Ruhezustand, da er nicht weiß, ob es sich bei DPAPI handelt es sich eine entsprechende Verschlüsselungsmechanismus.</span><span class="sxs-lookup"><span data-stu-id="7fd5f-109">If you change the key persistence location, the system will no longer automatically encrypt keys at rest since it doesn't know whether DPAPI is an appropriate encryption mechanism.</span></span>

<a name=configuring-x509-certificate></a>

<span data-ttu-id="7fd5f-110">Sie können konfigurieren, dass das System, um Schlüssel im Ruhezustand zu schützen, durch das Aufrufen einer der ProtectKeysWith\* Konfigurations-APIs.</span><span class="sxs-lookup"><span data-stu-id="7fd5f-110">You can configure the system to protect keys at rest by calling any of the ProtectKeysWith\* configuration APIs.</span></span> <span data-ttu-id="7fd5f-111">Betrachten Sie das folgenden Beispiel wird die Tasten auf einer UNC-Freigabe gespeichert, und diese Schlüssel im Ruhezustand mit einem bestimmten x. 509-Zertifikat verschlüsselt.</span><span class="sxs-lookup"><span data-stu-id="7fd5f-111">Consider the example below, which stores keys at a UNC share and encrypts those keys at rest with a specific X.509 certificate.</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .PersistKeysToFileSystem(new DirectoryInfo(@"\\server\share\directory\"))
        .ProtectKeysWithCertificate("thumbprint");
}
```

<span data-ttu-id="7fd5f-112">Finden Sie unter [Verschlüsselung ruhender](../implementation/key-encryption-at-rest.md#data-protection-implementation-key-encryption-at-rest) für weitere Beispiele und detaillierte Informationen zu den integrierten Schlüsselverschlüsselung Mechanismen.</span><span class="sxs-lookup"><span data-stu-id="7fd5f-112">See [key encryption at rest](../implementation/key-encryption-at-rest.md#data-protection-implementation-key-encryption-at-rest) for more examples and for discussion on the built-in key encryption mechanisms.</span></span>

<span data-ttu-id="7fd5f-113">So konfigurieren Sie das System eine standardmäßige Lebensdauer von 14 Tagen anstelle 90 Tage, betrachten das folgende Beispiel:</span><span class="sxs-lookup"><span data-stu-id="7fd5f-113">To configure the system to use a default key lifetime of 14 days instead of 90 days, consider the following example:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .SetDefaultKeyLifetime(TimeSpan.FromDays(14));
}
```

<span data-ttu-id="7fd5f-114">Standardmäßig isoliert die Datenschutzsystem Anwendungen von einem anderen, auch wenn sie die gleiche physischen Schlüssel Repository gemeinsam nutzen.</span><span class="sxs-lookup"><span data-stu-id="7fd5f-114">By default the data protection system isolates applications from one another, even if they're sharing the same physical key repository.</span></span> <span data-ttu-id="7fd5f-115">Dies verhindert, dass die Anwendungen aus der jeweils anderen geschützten Nutzlasten zu verstehen.</span><span class="sxs-lookup"><span data-stu-id="7fd5f-115">This prevents the applications from understanding each other's protected payloads.</span></span> <span data-ttu-id="7fd5f-116">Freigeben von geschützten Nutzlasten zwischen zwei verschiedenen konfigurieren Sie das System, übergeben in der gleichen Anwendungsname für beide Anwendungen wie in der folgenden Beispiel:</span><span class="sxs-lookup"><span data-stu-id="7fd5f-116">To share protected payloads between two different applications, configure the system passing in the same application name for both applications as in the below example:</span></span>

<a name=data-protection-code-sample-application-name></a>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .SetApplicationName("my application");
}
```

<a name=data-protection-configuring-disable-automatic-key-generation></a>

<span data-ttu-id="7fd5f-117">Schließlich können Sie ein Szenario haben, in dem Sie nicht möchten eine Anwendung die Schlüssel automatisch zurückzusetzen, wie sie Ablaufdatum nähern.</span><span class="sxs-lookup"><span data-stu-id="7fd5f-117">Finally, you may have a scenario where you do not want an application to automatically roll keys as they approach expiration.</span></span> <span data-ttu-id="7fd5f-118">Ein Beispiel hierfür ist möglicherweise Anwendungen richten Sie in eine primäre / sekundäre Beziehung, wobei nur die primäre Anwendung verantwortlich für die schlüsselverwaltung Bedenken und alle sekundären Anwendungen einfach eine schreibgeschützte Ansicht des Rings Schlüssel verfügen.</span><span class="sxs-lookup"><span data-stu-id="7fd5f-118">One example of this might be applications set up in a primary / secondary relationship, where only the primary application is responsible for key management concerns, and all secondary applications simply have a read-only view of the key ring.</span></span> <span data-ttu-id="7fd5f-119">Die sekundären Anwendungen können so konfiguriert werden, um den Schlüssel Ring als schreibgeschützt zu behandeln, indem Sie das System wie folgt konfigurieren:</span><span class="sxs-lookup"><span data-stu-id="7fd5f-119">The secondary applications can be configured to treat the key ring as read-only by configuring the system as below:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .DisableAutomaticKeyGeneration();
}
```

<a name=data-protection-configuration-per-app-isolation></a>

## <a name="per-application-isolation"></a><span data-ttu-id="7fd5f-120">Pro Anwendungsisolation</span><span class="sxs-lookup"><span data-stu-id="7fd5f-120">Per-application isolation</span></span>

<span data-ttu-id="7fd5f-121">Wenn das Data Protection-System von einem ASP.NET Core-Host bereitgestellt wird, wird es automatisch von einem anderen Isolieren von Anwendungen auch, wenn diese Anwendungen die gleichen Arbeitsprozesskonto ausgeführt werden, und die gleiche Schlüsselmaterial verwenden.</span><span class="sxs-lookup"><span data-stu-id="7fd5f-121">When the data protection system is provided by an ASP.NET Core host, it will automatically isolate applications from one another, even if those applications are running under the same worker process account and are using the same master keying material.</span></span> <span data-ttu-id="7fd5f-122">Dies ist zumindest ähneln den IsolateApps-Modifizierer aus System.Web <machineKey> Element.</span><span class="sxs-lookup"><span data-stu-id="7fd5f-122">This is somewhat similar to the IsolateApps modifier from System.Web's <machineKey> element.</span></span>

<span data-ttu-id="7fd5f-123">Die Isolation-Einschränkungsmechanismus Hinblick auf jede Anwendung auf dem lokalen Computer als eindeutige Mandant, enthält daher die IDataProtector Stamm für eine bestimmte Anwendung automatisch die Anwendungs-ID als Diskriminator.</span><span class="sxs-lookup"><span data-stu-id="7fd5f-123">The isolation mechanism works by considering each application on the local machine as a unique tenant, thus the IDataProtector rooted for any given application automatically includes the application ID as a discriminator.</span></span> <span data-ttu-id="7fd5f-124">Eindeutige ID der Anwendung stammen aus einer von zwei Stellen.</span><span class="sxs-lookup"><span data-stu-id="7fd5f-124">The application's unique ID comes from one of two places.</span></span>

1. <span data-ttu-id="7fd5f-125">Wenn die Anwendung in IIS gehostet wird, ist der eindeutige Bezeichner der Anwendung Konfigurationspfad an.</span><span class="sxs-lookup"><span data-stu-id="7fd5f-125">If the application is hosted in IIS, the unique identifier is the application's configuration path.</span></span> <span data-ttu-id="7fd5f-126">Wenn eine Anwendung in einer severfarmumgebung bereitgestellt wird, sollte dieser Wert stabil sein, vorausgesetzt, dass die IIS-Umgebungen für alle Computer in der Farm auf ähnliche Weise konfiguriert werden.</span><span class="sxs-lookup"><span data-stu-id="7fd5f-126">If an application is deployed in a farm environment, this value should be stable assuming that the IIS environments are configured similarly across all machines in the farm.</span></span>

2. <span data-ttu-id="7fd5f-127">Wenn die Anwendung nicht in IIS gehostet wird, ist der eindeutige Bezeichner der physische Pfad der Anwendung an.</span><span class="sxs-lookup"><span data-stu-id="7fd5f-127">If the application is not hosted in IIS, the unique identifier is the physical path of the application.</span></span>

<span data-ttu-id="7fd5f-128">Der eindeutige Bezeichner dient zum Zurücksetzen von Kennwörtern - sowohl der jeweiligen Anwendung und der Computer zu überstehen.</span><span class="sxs-lookup"><span data-stu-id="7fd5f-128">The unique identifier is designed to survive resets - both of the individual application and of the machine itself.</span></span>

<span data-ttu-id="7fd5f-129">Dieser Mechanismus für die Sicherheitsisolation wird davon ausgegangen, dass die Anwendungen nicht bösartig sind.</span><span class="sxs-lookup"><span data-stu-id="7fd5f-129">This isolation mechanism assumes that the applications are not malicious.</span></span> <span data-ttu-id="7fd5f-130">Eine böswillige Anwendung beeinträchtigt immer eine andere Anwendung, die unter der gleichen Arbeitsprozesskonto ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="7fd5f-130">A malicious application can always impact any other application running under the same worker process account.</span></span> <span data-ttu-id="7fd5f-131">In einer freigegebenen Hostingumgebung, in denen Anwendungen gegenseitig nicht vertrauenswürdig sind, sollte der Hostinganbieter um sicherzustellen, dass auf Betriebssystemebene Isolation zwischen Anwendungen, einschließlich der Anwendung zugrunde liegenden Key Repositorys trennen Schritten.</span><span class="sxs-lookup"><span data-stu-id="7fd5f-131">In a shared hosting environment where applications are mutually untrusted, the hosting provider should take steps to ensure OS-level isolation between applications, including separating the applications' underlying key repositories.</span></span>

<span data-ttu-id="7fd5f-132">Wenn die Datenschutzsystem nicht von einem ASP.NET Core-Host bereitgestellt wird, (z. B. wenn der Entwickler selbst über die DataProtectionProvider konkreten Typ instanziiert), das Isolieren von Webanwendungen standardmäßig deaktiviert ist, und alle Anwendungen, die durch die gleiche Erfassung gesichert Material kann Nutzlasten freigeben, solange sie die entsprechenden Zwecke bereitstellen.</span><span class="sxs-lookup"><span data-stu-id="7fd5f-132">If the data protection system is not provided by an ASP.NET Core host (e.g., if the developer instantiates it himself via the DataProtectionProvider concrete type), application isolation is disabled by default, and all applications backed by the same keying material can share payloads as long as they provide the appropriate purposes.</span></span> <span data-ttu-id="7fd5f-133">Um das Isolieren von Webanwendungen in dieser Umgebung bereitstellen möchten, rufen Sie die SetApplicationName-Methode auf das Konfigurationsobjekt, finden Sie unter der [Codebeispiel](#data-protection-code-sample-application-name) oben.</span><span class="sxs-lookup"><span data-stu-id="7fd5f-133">To provide application isolation in this environment, call the SetApplicationName method on the configuration object, see the [code sample](#data-protection-code-sample-application-name) above.</span></span>

<a name=data-protection-changing-algorithms></a>

## <a name="changing-algorithms"></a><span data-ttu-id="7fd5f-134">Ändern von Algorithmen</span><span class="sxs-lookup"><span data-stu-id="7fd5f-134">Changing algorithms</span></span>

<span data-ttu-id="7fd5f-135">Der Data Protection Stapel lässt den Wechsel der durch den neu generierten Schlüssel verwendete Standardalgorithmus.</span><span class="sxs-lookup"><span data-stu-id="7fd5f-135">The data protection stack allows changing the default algorithm used by newly-generated keys.</span></span> <span data-ttu-id="7fd5f-136">Die einfachste Möglichkeit hierzu ist UseCryptographicAlgorithms vom Rückruf Konfiguration aufrufen, wie in der folgenden Beispiel.</span><span class="sxs-lookup"><span data-stu-id="7fd5f-136">The simplest way to do this is to call UseCryptographicAlgorithms from the configuration callback, as in the below example.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="7fd5f-137">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="7fd5f-137">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
services.AddDataProtection()
    .UseCryptographicAlgorithms(new AuthenticatedEncryptorConfiguration()
    {
        EncryptionAlgorithm = EncryptionAlgorithm.AES_256_CBC,
        ValidationAlgorithm = ValidationAlgorithm.HMACSHA256
    });
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="7fd5f-138">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="7fd5f-138">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
services.AddDataProtection()
    .UseCryptographicAlgorithms(new AuthenticatedEncryptionSettings()
    {
        EncryptionAlgorithm = EncryptionAlgorithm.AES_256_CBC,
        ValidationAlgorithm = ValidationAlgorithm.HMACSHA256
    });
```

---

<span data-ttu-id="7fd5f-139">Die Standardeinstellung "EncryptionAlgorithm" und ValidationAlgorithm lauten AES-256-CBC bzw. HMACSHA256.</span><span class="sxs-lookup"><span data-stu-id="7fd5f-139">The default EncryptionAlgorithm and ValidationAlgorithm are AES-256-CBC and HMACSHA256, respectively.</span></span> <span data-ttu-id="7fd5f-140">Die Standardrichtlinie kann festgelegt werden, von einem Systemadministrator über [Wide Computerrichtlinie](machine-wide-policy.md), aber ein expliziter Aufruf von UseCryptographicAlgorithms wird die Standardrichtlinie überschreiben.</span><span class="sxs-lookup"><span data-stu-id="7fd5f-140">The default policy can be set by a system administrator via [Machine Wide Policy](machine-wide-policy.md), but an explicit call to UseCryptographicAlgorithms will override the default policy.</span></span>

<span data-ttu-id="7fd5f-141">UseCryptographicAlgorithms aufgerufen wird, dass der Entwickler zum Angeben des gewünschten Algorithmus (aus einer vordefinierten integrierten Liste), und der Entwickler muss sich nicht um die Implementierung des Algorithmus kümmern.</span><span class="sxs-lookup"><span data-stu-id="7fd5f-141">Calling UseCryptographicAlgorithms will allow the developer to specify the desired algorithm (from a predefined built-in list), and the developer does not need to worry about the implementation of the algorithm.</span></span> <span data-ttu-id="7fd5f-142">Z. B. im Szenario oben die Datenschutzsystem wird versucht, die CNG-Implementierung von AES verwenden, wenn unter Windows ausgeführt wird, andernfalls es wird ein Fallback auf den verwalteten System.Security.Cryptography.Aes-Klasse.</span><span class="sxs-lookup"><span data-stu-id="7fd5f-142">For instance, in the scenario above the data protection system will attempt to use the CNG implementation of AES if running on Windows, otherwise it will fall back to the managed System.Security.Cryptography.Aes class.</span></span>

<span data-ttu-id="7fd5f-143">Der Entwickler kann eine Implementierung manuell festlegen, bei Bedarf über einen Aufruf an UseCustomCryptographicAlgorithms, wie in den folgenden Beispielen.</span><span class="sxs-lookup"><span data-stu-id="7fd5f-143">The developer can manually specify an implementation if desired via a call to UseCustomCryptographicAlgorithms, as show in the below examples.</span></span>

>[!TIP]
> <span data-ttu-id="7fd5f-144">Ändern die Algorithmen wirkt sich nicht auf den vorhandenen Schlüssel im Schlüssel Ring aus.</span><span class="sxs-lookup"><span data-stu-id="7fd5f-144">Changing algorithms does not affect existing keys in the key ring.</span></span> <span data-ttu-id="7fd5f-145">Er wirkt sich nur auf neu generierten Schlüssel.</span><span class="sxs-lookup"><span data-stu-id="7fd5f-145">It only affects newly-generated keys.</span></span>

<a name=data-protection-changing-algorithms-custom-managed></a>

### <a name="specifying-custom-managed-algorithms"></a><span data-ttu-id="7fd5f-146">Angeben von benutzerdefinierten verwalteten Algorithmen</span><span class="sxs-lookup"><span data-stu-id="7fd5f-146">Specifying custom managed algorithms</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="7fd5f-147">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="7fd5f-147">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="7fd5f-148">Um benutzerdefinierte Algorithmen, die verwaltete anzugeben, erstellen Sie eine ManagedAuthenticatedEncryptorConfiguration-Instanz, die auf die Implementierungstypen verweist.</span><span class="sxs-lookup"><span data-stu-id="7fd5f-148">To specify custom managed algorithms, create a ManagedAuthenticatedEncryptorConfiguration instance that points to the implementation types.</span></span>

```csharp
serviceCollection.AddDataProtection()
    .UseCustomCryptographicAlgorithms(new ManagedAuthenticatedEncryptorConfiguration()
    {
        // a type that subclasses SymmetricAlgorithm
        EncryptionAlgorithmType = typeof(Aes),

        // specified in bits
        EncryptionAlgorithmKeySize = 256,

        // a type that subclasses KeyedHashAlgorithm
        ValidationAlgorithmType = typeof(HMACSHA256)
    });
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="7fd5f-149">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="7fd5f-149">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="7fd5f-150">Um benutzerdefinierte Algorithmen, die verwaltete anzugeben, erstellen Sie eine ManagedAuthenticatedEncryptionSettings-Instanz, die auf die Implementierungstypen verweist.</span><span class="sxs-lookup"><span data-stu-id="7fd5f-150">To specify custom managed algorithms, create a ManagedAuthenticatedEncryptionSettings instance that points to the implementation types.</span></span>

```csharp
serviceCollection.AddDataProtection()
    .UseCustomCryptographicAlgorithms(new ManagedAuthenticatedEncryptionSettings()
    {
        // a type that subclasses SymmetricAlgorithm
        EncryptionAlgorithmType = typeof(Aes),

        // specified in bits
        EncryptionAlgorithmKeySize = 256,

        // a type that subclasses KeyedHashAlgorithm
        ValidationAlgorithmType = typeof(HMACSHA256)
    });
```

---

<span data-ttu-id="7fd5f-151">Im Allgemeinen die \*Typeigenschaften müssen auf konkrete, zeigen instanziierbaren (über einen öffentlichen parameterlosen Ctor) Implementierungen von SymmetricAlgorithm und KeyedHashAlgorithm, obwohl das System spezielle-Fällen einige Werte wie typeof(Aes) der Einfachheit halber .</span><span class="sxs-lookup"><span data-stu-id="7fd5f-151">Generally the \*Type properties must point to concrete, instantiable (via a public parameterless ctor) implementations of SymmetricAlgorithm and KeyedHashAlgorithm, though the system special-cases some values like typeof(Aes) for convenience.</span></span>

> [!NOTE]
> <span data-ttu-id="7fd5f-152">Die SymmetricAlgorithm muss eine Schlüssellänge von ≥ 128 Bits und eine Blockgröße von ≥ 64 Bits haben, und es muss CBC-Modus-Verschlüsselung mit PKCS #7-Auffüllung unterstützen.</span><span class="sxs-lookup"><span data-stu-id="7fd5f-152">The SymmetricAlgorithm must have a key length of ≥ 128 bits and a block size of ≥ 64 bits, and it must support CBC-mode encryption with PKCS #7 padding.</span></span> <span data-ttu-id="7fd5f-153">Der KeyedHashAlgorithm benötigen Nachrichtenhash Größe > = 128 Bits und müssen Schlüssel, der den Hashalgorithmus Digestlänge gleich Länge unterstützt.</span><span class="sxs-lookup"><span data-stu-id="7fd5f-153">The KeyedHashAlgorithm must have a digest size of >= 128 bits, and it must support keys of length equal to the hash algorithm's digest length.</span></span> <span data-ttu-id="7fd5f-154">Die KeyedHashAlgorithm ist nicht zwingend erforderlich, HMAC sein.</span><span class="sxs-lookup"><span data-stu-id="7fd5f-154">The KeyedHashAlgorithm is not strictly required to be HMAC.</span></span>

<a name=data-protection-changing-algorithms-cng></a>

### <a name="specifying-custom-windows-cng-algorithms"></a><span data-ttu-id="7fd5f-155">Angeben von benutzerdefinierten Windows CNG-Algorithmen</span><span class="sxs-lookup"><span data-stu-id="7fd5f-155">Specifying custom Windows CNG algorithms</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="7fd5f-156">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="7fd5f-156">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="7fd5f-157">Erstellen Sie zum Angeben eines benutzerdefinierten Windows CNG-Algorithmus mit Verschlüsselung im CBC-Modus + HMAC-Überprüfung einer CngCbcAuthenticatedEncryptorConfiguration-Instanz, die die algorithmische Informationen enthält.</span><span class="sxs-lookup"><span data-stu-id="7fd5f-157">To specify a custom Windows CNG algorithm using CBC-mode encryption + HMAC validation, create a CngCbcAuthenticatedEncryptorConfiguration instance that contains the algorithmic information.</span></span>

```csharp
services.AddDataProtection()
    .UseCustomCryptographicAlgorithms(new CngCbcAuthenticatedEncryptorConfiguration()
    {
        // passed to BCryptOpenAlgorithmProvider
        EncryptionAlgorithm = "AES",
        EncryptionAlgorithmProvider = null,

        // specified in bits
        EncryptionAlgorithmKeySize = 256,

        // passed to BCryptOpenAlgorithmProvider
        HashAlgorithm = "SHA256",
        HashAlgorithmProvider = null
    });
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="7fd5f-158">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="7fd5f-158">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="7fd5f-159">Erstellen Sie zum Angeben eines benutzerdefinierten Windows CNG-Algorithmus mit Verschlüsselung im CBC-Modus + HMAC-Überprüfung einer CngCbcAuthenticatedEncryptionSettings-Instanz, die die algorithmische Informationen enthält.</span><span class="sxs-lookup"><span data-stu-id="7fd5f-159">To specify a custom Windows CNG algorithm using CBC-mode encryption + HMAC validation, create a CngCbcAuthenticatedEncryptionSettings instance that contains the algorithmic information.</span></span>

```csharp
services.AddDataProtection()
    .UseCustomCryptographicAlgorithms(new CngCbcAuthenticatedEncryptionSettings()
    {
        // passed to BCryptOpenAlgorithmProvider
        EncryptionAlgorithm = "AES",
        EncryptionAlgorithmProvider = null,

        // specified in bits
        EncryptionAlgorithmKeySize = 256,

        // passed to BCryptOpenAlgorithmProvider
        HashAlgorithm = "SHA256",
        HashAlgorithmProvider = null
    });
```

---

> [!NOTE]
> <span data-ttu-id="7fd5f-160">Der Block symmetrischen Verschlüsselungsalgorithmus muss eine Schlüssellänge von ≥ 128 Bits und eine Blockgröße von ≥ 64 Bits haben, und es muss CBC-Modus-Verschlüsselung mit PKCS #7-Auffüllung unterstützen.</span><span class="sxs-lookup"><span data-stu-id="7fd5f-160">The symmetric block cipher algorithm must have a key length of ≥ 128 bits and a block size of ≥ 64 bits, and it must support CBC-mode encryption with PKCS #7 padding.</span></span> <span data-ttu-id="7fd5f-161">Der Hashalgorithmus benötigen Nachrichtenhash Größe von > = 128 Bits und müssen unterstützen, die mit dem Flag "BCRYPT_ALG_HANDLE_HMAC_FLAG" geöffnet wird.</span><span class="sxs-lookup"><span data-stu-id="7fd5f-161">The hash algorithm must have a digest size of >= 128 bits and must support being opened with the BCRYPT_ALG_HANDLE_HMAC_FLAG flag.</span></span> <span data-ttu-id="7fd5f-162">Die \*Anbietereigenschaften können werden auf null festgelegt, um den Standardanbieter für den angegebenen Algorithmus zu verwenden.</span><span class="sxs-lookup"><span data-stu-id="7fd5f-162">The \*Provider properties can be set to null to use the default provider for the specified algorithm.</span></span> <span data-ttu-id="7fd5f-163">Finden Sie unter der [BCryptOpenAlgorithmProvider](https://msdn.microsoft.com/library/windows/desktop/aa375479(v=vs.85).aspx) Dokumentation weitere Informationen.</span><span class="sxs-lookup"><span data-stu-id="7fd5f-163">See the [BCryptOpenAlgorithmProvider](https://msdn.microsoft.com/library/windows/desktop/aa375479(v=vs.85).aspx) documentation for more information.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="7fd5f-164">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="7fd5f-164">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="7fd5f-165">Erstellen Sie zum Angeben eines benutzerdefinierten Windows CNG-Algorithmus mit Verschlüsselung Galois/Leistungsindikatoren im + Überprüfung eine CngGcmAuthenticatedEncryptorConfiguration-Instanz, die die algorithmische Informationen enthält.</span><span class="sxs-lookup"><span data-stu-id="7fd5f-165">To specify a custom Windows CNG algorithm using Galois/Counter Mode encryption + validation, create a CngGcmAuthenticatedEncryptorConfiguration instance that contains the algorithmic information.</span></span>

```csharp
services.AddDataProtection()
    .UseCustomCryptographicAlgorithms(new CngGcmAuthenticatedEncryptorConfiguration()
    {
        // passed to BCryptOpenAlgorithmProvider
        EncryptionAlgorithm = "AES",
        EncryptionAlgorithmProvider = null,

        // specified in bits
        EncryptionAlgorithmKeySize = 256
    });
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="7fd5f-166">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="7fd5f-166">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="7fd5f-167">Erstellen Sie zum Angeben eines benutzerdefinierten Windows CNG-Algorithmus mit Verschlüsselung Galois/Leistungsindikatoren im + Überprüfung eine CngGcmAuthenticatedEncryptionSettings-Instanz, die die algorithmische Informationen enthält.</span><span class="sxs-lookup"><span data-stu-id="7fd5f-167">To specify a custom Windows CNG algorithm using Galois/Counter Mode encryption + validation, create a CngGcmAuthenticatedEncryptionSettings instance that contains the algorithmic information.</span></span>

```csharp
services.AddDataProtection()
    .UseCustomCryptographicAlgorithms(new CngGcmAuthenticatedEncryptionSettings()
    {
        // passed to BCryptOpenAlgorithmProvider
        EncryptionAlgorithm = "AES",
        EncryptionAlgorithmProvider = null,

        // specified in bits
        EncryptionAlgorithmKeySize = 256
    });
```

---

> [!NOTE]
> <span data-ttu-id="7fd5f-168">Der Block symmetrischen Verschlüsselungsalgorithmus benötigen eine Schlüssellänge von ≥ 128 Bits und eine Blockgröße von genau 128 Bits und müssen GCM-Verschlüsselung unterstützt.</span><span class="sxs-lookup"><span data-stu-id="7fd5f-168">The symmetric block cipher algorithm must have a key length of ≥ 128 bits and a block size of exactly 128 bits, and it must support GCM encryption.</span></span> <span data-ttu-id="7fd5f-169">Die EncryptionAlgorithmProvider-Eigenschaft kann festgelegt werden, auf null fest, um den Standardanbieter für den angegebenen Algorithmus zu verwenden.</span><span class="sxs-lookup"><span data-stu-id="7fd5f-169">The EncryptionAlgorithmProvider property can be set to null to use the default provider for the specified algorithm.</span></span> <span data-ttu-id="7fd5f-170">Finden Sie unter der [BCryptOpenAlgorithmProvider](https://msdn.microsoft.com/library/windows/desktop/aa375479(v=vs.85).aspx) Dokumentation weitere Informationen.</span><span class="sxs-lookup"><span data-stu-id="7fd5f-170">See the [BCryptOpenAlgorithmProvider](https://msdn.microsoft.com/library/windows/desktop/aa375479(v=vs.85).aspx) documentation for more information.</span></span>

### <a name="specifying-other-custom-algorithms"></a><span data-ttu-id="7fd5f-171">Andere benutzerdefinierten Algorithmen angeben</span><span class="sxs-lookup"><span data-stu-id="7fd5f-171">Specifying other custom algorithms</span></span>

<span data-ttu-id="7fd5f-172">Obwohl nicht als eine erstklassige-API verfügbar gemacht, ist die Datenschutzsystem extensible genug, um zu ermöglichen, nahezu jede Art von Algorithmus angibt.</span><span class="sxs-lookup"><span data-stu-id="7fd5f-172">Though not exposed as a first-class API, the data protection system is extensible enough to allow specifying almost any kind of algorithm.</span></span> <span data-ttu-id="7fd5f-173">Beispielsweise ist es möglich, alle Schlüssel in einem HSM enthaltenen zu behalten und eine benutzerdefinierte Implementierung des Kerns Ver- und Entschlüsselung Routinen bereitstellen.</span><span class="sxs-lookup"><span data-stu-id="7fd5f-173">For example, it is possible to keep all keys contained within an HSM and to provide a custom implementation of the core encryption and decryption routines.</span></span> <span data-ttu-id="7fd5f-174">Finden Sie unter IAuthenticatedEncryptorConfiguration im Kern Kryptografie Erweiterbarkeit Abschnitt, um weitere Informationen.</span><span class="sxs-lookup"><span data-stu-id="7fd5f-174">See IAuthenticatedEncryptorConfiguration in the core cryptography extensibility section for more information.</span></span>

### <a name="see-also"></a><span data-ttu-id="7fd5f-175">Siehe auch</span><span class="sxs-lookup"><span data-stu-id="7fd5f-175">See also</span></span>

* [<span data-ttu-id="7fd5f-176">Szenarios, die die Abhängigkeitsinjektion nicht beachten</span><span class="sxs-lookup"><span data-stu-id="7fd5f-176">Non DI Aware Scenarios</span></span>](non-di-scenarios.md)
* [<span data-ttu-id="7fd5f-177">Computerübergreifende Richtlinie</span><span class="sxs-lookup"><span data-stu-id="7fd5f-177">Machine Wide Policy</span></span>](machine-wide-policy.md)
