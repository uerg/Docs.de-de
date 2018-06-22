---
title: Konfigurieren von ASP.NET Core Datenschutz
author: rick-anderson
description: Informationen Sie zum Datenschutz in ASP.NET Core konfigurieren.
ms.author: riande
ms.custom: mvc
ms.date: 07/17/2017
uid: security/data-protection/configuration/overview
ms.openlocfilehash: b2fa1921120d297f4b0dbb0294a00e0e573f4e04
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/20/2018
ms.locfileid: "36272578"
---
# <a name="configure-aspnet-core-data-protection"></a><span data-ttu-id="69339-103">Konfigurieren von ASP.NET Core Datenschutz</span><span class="sxs-lookup"><span data-stu-id="69339-103">Configure ASP.NET Core Data Protection</span></span>

<span data-ttu-id="69339-104">Von [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="69339-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="69339-105">Wenn das System den Datenschutz initialisiert wird, gilt es [Standardeinstellungen](xref:security/data-protection/configuration/default-settings) basierend auf der betriebsumgebung.</span><span class="sxs-lookup"><span data-stu-id="69339-105">When the Data Protection system is initialized, it applies [default settings](xref:security/data-protection/configuration/default-settings) based on the operational environment.</span></span> <span data-ttu-id="69339-106">Diese Einstellungen sind im Allgemeinen geeignet für apps, die auf einem einzelnen Computer ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="69339-106">These settings are generally appropriate for apps running on a single machine.</span></span> <span data-ttu-id="69339-107">Es gibt Fälle, in denen ein Entwickler sollten, um die Standardeinstellungen zu ändern:</span><span class="sxs-lookup"><span data-stu-id="69339-107">There are cases where a developer may want to change the default settings:</span></span>

* <span data-ttu-id="69339-108">Die app wird auf mehrere Computer verteilt.</span><span class="sxs-lookup"><span data-stu-id="69339-108">The app is spread across multiple machines.</span></span>
* <span data-ttu-id="69339-109">Aus Compliance-Gründen.</span><span class="sxs-lookup"><span data-stu-id="69339-109">For compliance reasons.</span></span>

<span data-ttu-id="69339-110">Für die folgenden Szenarien bietet das Data Protection-System eine umfangreiche Konfigurations-API.</span><span class="sxs-lookup"><span data-stu-id="69339-110">For these scenarios, the Data Protection system offers a rich configuration API.</span></span>

> [!WARNING]
> <span data-ttu-id="69339-111">Ähnlich wie bei den Konfigurationsdateien sollte der Data Protection Schlüssel Ring geschützt werden mit entsprechenden Berechtigungen.</span><span class="sxs-lookup"><span data-stu-id="69339-111">Similar to configuration files, the data protection key ring should be protected using appropriate permissions.</span></span> <span data-ttu-id="69339-112">Sie können ruhende Verschlüsselung, aber dadurch nicht verhindert, dass Angreifer das Erstellen neuer Schlüssel.</span><span class="sxs-lookup"><span data-stu-id="69339-112">You can choose to encrypt keys at rest, but this doesn't prevent attackers from creating new keys.</span></span> <span data-ttu-id="69339-113">Folglich wird die Sicherheit Ihrer app beeinträchtigt.</span><span class="sxs-lookup"><span data-stu-id="69339-113">Consequently, your app's security is impacted.</span></span> <span data-ttu-id="69339-114">Der Speicherort, die mit Data Protection konfiguriert müssen den Zugriff auf die app selbst, ähnlich wie die Konfigurationsdateien zu schützen, ist beschränkt.</span><span class="sxs-lookup"><span data-stu-id="69339-114">The storage location configured with Data Protection should have its access limited to the app itself, similar to the way you would protect configuration files.</span></span> <span data-ttu-id="69339-115">Wenn Ihr Schlüssel Ring auf dem Datenträger gespeichert werden soll, verwenden Sie z. B. Dateisystemberechtigungen.</span><span class="sxs-lookup"><span data-stu-id="69339-115">For example, if you choose to store your key ring on disk, use file system permissions.</span></span> <span data-ttu-id="69339-116">Stellen Sie sicher nur die Identität unter Leseberechtigungen der Ihre Web-app ausgeführt wird, schreiben und Zugriff auf dieses Verzeichnis zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="69339-116">Ensure only the identity under which your web app runs has read, write, and create access to that directory.</span></span> <span data-ttu-id="69339-117">Bei Verwendung von Azure-Tabellenspeicher sollte nur die Web-app haben die Möglichkeit zum Lesen, schreiben oder neue Einträge in den Tabellenspeicher usw. erstellen.</span><span class="sxs-lookup"><span data-stu-id="69339-117">If you use Azure Table Storage, only the web app should have the ability to read, write, or create new entries in the table store, etc.</span></span>
>
> <span data-ttu-id="69339-118">Die Erweiterungsmethode [AddDataProtection](/dotnet/api/microsoft.extensions.dependencyinjection.dataprotectionservicecollectionextensions.adddataprotection) gibt eine [IDataProtectionBuilder](/dotnet/api/microsoft.aspnetcore.dataprotection.idataprotectionbuilder).</span><span class="sxs-lookup"><span data-stu-id="69339-118">The extension method [AddDataProtection](/dotnet/api/microsoft.extensions.dependencyinjection.dataprotectionservicecollectionextensions.adddataprotection) returns an [IDataProtectionBuilder](/dotnet/api/microsoft.aspnetcore.dataprotection.idataprotectionbuilder).</span></span> <span data-ttu-id="69339-119">`IDataProtectionBuilder` macht Erweiterungsmethoden, dass Sie miteinander Optionen verketten können so konfigurieren Sie den Schutz von Daten an.</span><span class="sxs-lookup"><span data-stu-id="69339-119">`IDataProtectionBuilder` exposes extension methods that you can chain together to configure Data Protection options.</span></span>

::: moniker range=">= aspnetcore-2.1"

## <a name="protectkeyswithazurekeyvault"></a><span data-ttu-id="69339-120">ProtectKeysWithAzureKeyVault</span><span class="sxs-lookup"><span data-stu-id="69339-120">ProtectKeysWithAzureKeyVault</span></span>

<span data-ttu-id="69339-121">Zum Speichern von Schlüsseln in [Azure Key Vault](https://azure.microsoft.com/services/key-vault/), konfigurieren Sie das System mit [ProtectKeysWithAzureKeyVault](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.protectkeyswithazurekeyvault) in die `Startup` Klasse:</span><span class="sxs-lookup"><span data-stu-id="69339-121">To store keys in [Azure Key Vault](https://azure.microsoft.com/services/key-vault/), configure the system with [ProtectKeysWithAzureKeyVault](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.protectkeyswithazurekeyvault) in the `Startup` class:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .PersistKeysToAzureBlobStorage(new Uri("<blobUriWithSasToken>"))
        .ProtectKeysWithAzureKeyVault("<keyIdentifier>", "<clientId>", "<clientSecret>");
}
```

<span data-ttu-id="69339-122">Legen Sie den Schlüssel Ring-Speicherort (z. B. [PersistKeysToAzureBlobStorage](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.persistkeystoazureblobstorage)).</span><span class="sxs-lookup"><span data-stu-id="69339-122">Set the key ring storage location (for example, [PersistKeysToAzureBlobStorage](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.persistkeystoazureblobstorage)).</span></span> <span data-ttu-id="69339-123">Der Speicherort muss festgelegt werden, da Aufrufen `ProtectKeysWithAzureKeyVault` implementiert eine [IXmlEncryptor](/dotnet/api/microsoft.aspnetcore.dataprotection.xmlencryption.ixmlencryptor) , von denen Daten automatisch schutzeinstellungen, einschließlich den Schlüsselbund Speicherort deaktiviert.</span><span class="sxs-lookup"><span data-stu-id="69339-123">The location must be set because calling `ProtectKeysWithAzureKeyVault` implements an [IXmlEncryptor](/dotnet/api/microsoft.aspnetcore.dataprotection.xmlencryption.ixmlencryptor) that disables automatic data protection settings, including the key ring storage location.</span></span> <span data-ttu-id="69339-124">Im vorherige Beispiel verwendet Azure Blob-Speicher, den Schlüssel Ring beizubehalten.</span><span class="sxs-lookup"><span data-stu-id="69339-124">The preceding example uses Azure Blob Storage to persist the key ring.</span></span> <span data-ttu-id="69339-125">Weitere Informationen finden Sie unter [Key Storage Provider: Azure und Redis](xref:security/data-protection/implementation/key-storage-providers#azure-and-redis).</span><span class="sxs-lookup"><span data-stu-id="69339-125">For more information, see [Key storage providers: Azure and Redis](xref:security/data-protection/implementation/key-storage-providers#azure-and-redis).</span></span> <span data-ttu-id="69339-126">Sie können auch den Schlüssel Ring lokal mit beibehalten [PersistKeysToFileSystem](xref:security/data-protection/implementation/key-storage-providers#file-system).</span><span class="sxs-lookup"><span data-stu-id="69339-126">You can also persist the key ring locally with [PersistKeysToFileSystem](xref:security/data-protection/implementation/key-storage-providers#file-system).</span></span>

<span data-ttu-id="69339-127">Die `keyIdentifier` schlüsseltresor-Schlüsselbezeichner, die für die Verschlüsselung verwendet wird (z. B. `https://contosokeyvault.vault.azure.net/keys/dataprotection/`).</span><span class="sxs-lookup"><span data-stu-id="69339-127">The `keyIdentifier` is the key vault key identifier used for key encryption (for example, `https://contosokeyvault.vault.azure.net/keys/dataprotection/`).</span></span>

<span data-ttu-id="69339-128">`ProtectKeysWithAzureKeyVault` Überladungen:</span><span class="sxs-lookup"><span data-stu-id="69339-128">`ProtectKeysWithAzureKeyVault` overloads:</span></span>

* <span data-ttu-id="69339-129">[ProtectKeysWithAzureKeyVault (IDataProtectionBuilder KeyVaultClient, String)](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.protectkeyswithazurekeyvault#Microsoft_AspNetCore_DataProtection_AzureDataProtectionBuilderExtensions_ProtectKeysWithAzureKeyVault_Microsoft_AspNetCore_DataProtection_IDataProtectionBuilder_Microsoft_Azure_KeyVault_KeyVaultClient_System_String_) erlaubt die Verwendung einer [KeyVaultClient](/dotnet/api/microsoft.azure.keyvault.keyvaultclient) So aktivieren Sie die Datenschutzsystem schlüsseltresor verwenden.</span><span class="sxs-lookup"><span data-stu-id="69339-129">[ProtectKeysWithAzureKeyVault(IDataProtectionBuilder, KeyVaultClient, String)](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.protectkeyswithazurekeyvault#Microsoft_AspNetCore_DataProtection_AzureDataProtectionBuilderExtensions_ProtectKeysWithAzureKeyVault_Microsoft_AspNetCore_DataProtection_IDataProtectionBuilder_Microsoft_Azure_KeyVault_KeyVaultClient_System_String_) permits the use of a [KeyVaultClient](/dotnet/api/microsoft.azure.keyvault.keyvaultclient) to enable the data protection system to use the key vault.</span></span>
* <span data-ttu-id="69339-130">[ProtectKeysWithAzureKeyVault (IDataProtectionBuilder, String, String, X509Certificate2)](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.protectkeyswithazurekeyvault#Microsoft_AspNetCore_DataProtection_AzureDataProtectionBuilderExtensions_ProtectKeysWithAzureKeyVault_Microsoft_AspNetCore_DataProtection_IDataProtectionBuilder_System_String_System_String_System_Security_Cryptography_X509Certificates_X509Certificate2_) erlaubt die Verwendung einer `ClientId` und [X509Certificate](/dotnet/api/system.security.cryptography.x509certificates.x509certificate2) So aktivieren Sie die Datenschutzsystem schlüsseltresor verwenden.</span><span class="sxs-lookup"><span data-stu-id="69339-130">[ProtectKeysWithAzureKeyVault(IDataProtectionBuilder, String, String, X509Certificate2)](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.protectkeyswithazurekeyvault#Microsoft_AspNetCore_DataProtection_AzureDataProtectionBuilderExtensions_ProtectKeysWithAzureKeyVault_Microsoft_AspNetCore_DataProtection_IDataProtectionBuilder_System_String_System_String_System_Security_Cryptography_X509Certificates_X509Certificate2_) permits the use of a `ClientId` and [X509Certificate](/dotnet/api/system.security.cryptography.x509certificates.x509certificate2) to enable the data protection system to use the key vault.</span></span>
* <span data-ttu-id="69339-131">[ProtectKeysWithAzureKeyVault (IDataProtectionBuilder, String, String, String)](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.protectkeyswithazurekeyvault#Microsoft_AspNetCore_DataProtection_AzureDataProtectionBuilderExtensions_ProtectKeysWithAzureKeyVault_Microsoft_AspNetCore_DataProtection_IDataProtectionBuilder_System_String_System_String_System_String_) erlaubt die Verwendung einer `ClientId` und `ClientSecret` So aktivieren Sie die Datenschutzsystem schlüsseltresor verwenden.</span><span class="sxs-lookup"><span data-stu-id="69339-131">[ProtectKeysWithAzureKeyVault(IDataProtectionBuilder, String, String, String)](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.protectkeyswithazurekeyvault#Microsoft_AspNetCore_DataProtection_AzureDataProtectionBuilderExtensions_ProtectKeysWithAzureKeyVault_Microsoft_AspNetCore_DataProtection_IDataProtectionBuilder_System_String_System_String_System_String_) permits the use of a `ClientId` and `ClientSecret` to enable the data protection system to use the key vault.</span></span>

::: moniker-end

## <a name="persistkeystofilesystem"></a><span data-ttu-id="69339-132">PersistKeysToFileSystem</span><span class="sxs-lookup"><span data-stu-id="69339-132">PersistKeysToFileSystem</span></span>

<span data-ttu-id="69339-133">Zum Speichern von Schlüsseln auf einer UNC-Freigabe statt an den *%LocalAppData%* Standardspeicherort, konfigurieren Sie das System mit [PersistKeysToFileSystem](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystofilesystem):</span><span class="sxs-lookup"><span data-stu-id="69339-133">To store keys on a UNC share instead of at the *%LOCALAPPDATA%* default location, configure the system with [PersistKeysToFileSystem](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystofilesystem):</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .PersistKeysToFileSystem(new DirectoryInfo(@"\\server\share\directory\"));
}
```

> [!WARNING]
> <span data-ttu-id="69339-134">Wenn Sie den Speicherort der schlüsselpersistenz ändern, verschlüsselt das System nicht mehr automatisch Schlüssel im Ruhezustand, da er nicht weiß, ob eine entsprechende Verschlüsselungsmechanismus bei DPAPI handelt es.</span><span class="sxs-lookup"><span data-stu-id="69339-134">If you change the key persistence location, the system no longer automatically encrypts keys at rest, since it doesn't know whether DPAPI is an appropriate encryption mechanism.</span></span>

## <a name="protectkeyswith"></a><span data-ttu-id="69339-135">ProtectKeysWith\*</span><span class="sxs-lookup"><span data-stu-id="69339-135">ProtectKeysWith\*</span></span>

<span data-ttu-id="69339-136">Sie können konfigurieren, dass Schlüssel im Ruhezustand zu schützen, durch das Aufrufen einer der im System die [ProtectKeysWith\* ](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions) Konfigurations-APIs.</span><span class="sxs-lookup"><span data-stu-id="69339-136">You can configure the system to protect keys at rest by calling any of the [ProtectKeysWith\*](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions) configuration APIs.</span></span> <span data-ttu-id="69339-137">Betrachten Sie das folgenden Beispiel wird die Tasten auf einer UNC-Freigabe gespeichert und verschlüsselt den Schlüssel im Ruhezustand mit einem bestimmten x. 509-Zertifikat aus:</span><span class="sxs-lookup"><span data-stu-id="69339-137">Consider the example below, which stores keys on a UNC share and encrypts those keys at rest with a specific X.509 certificate:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .PersistKeysToFileSystem(new DirectoryInfo(@"\\server\share\directory\"))
        .ProtectKeysWithCertificate("thumbprint");
}
```

<span data-ttu-id="69339-138">Finden Sie unter [Schlüssel Verschlüsselung ruhender](xref:security/data-protection/implementation/key-encryption-at-rest) Weitere Beispiele und detaillierte Informationen zu den integrierten Schlüsselverschlüsselung Mechanismen.</span><span class="sxs-lookup"><span data-stu-id="69339-138">See [Key Encryption At Rest](xref:security/data-protection/implementation/key-encryption-at-rest) for more examples and discussion on the built-in key encryption mechanisms.</span></span>

## <a name="setdefaultkeylifetime"></a><span data-ttu-id="69339-139">SetDefaultKeyLifetime</span><span class="sxs-lookup"><span data-stu-id="69339-139">SetDefaultKeyLifetime</span></span>

<span data-ttu-id="69339-140">Verwenden Sie zum Konfigurieren der im System eine Lebensdauer von 14 Tagen verwenden Sie anstelle des standardmäßigen 90 Tage [SetDefaultKeyLifetime](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setdefaultkeylifetime):</span><span class="sxs-lookup"><span data-stu-id="69339-140">To configure the system to use a key lifetime of 14 days instead of the default 90 days, use [SetDefaultKeyLifetime](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setdefaultkeylifetime):</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .SetDefaultKeyLifetime(TimeSpan.FromDays(14));
}
```

## <a name="setapplicationname"></a><span data-ttu-id="69339-141">SetApplicationName</span><span class="sxs-lookup"><span data-stu-id="69339-141">SetApplicationName</span></span>

<span data-ttu-id="69339-142">Standardmäßig isoliert die Datenschutz-System apps von einem anderen, auch wenn sie die gleiche physischen Schlüssel Repository gemeinsam nutzen.</span><span class="sxs-lookup"><span data-stu-id="69339-142">By default, the Data Protection system isolates apps from one another, even if they're sharing the same physical key repository.</span></span> <span data-ttu-id="69339-143">Dadurch wird verhindert, dass die apps Grundlegendes zur jeweils anderen geschützten Nutzlasten.</span><span class="sxs-lookup"><span data-stu-id="69339-143">This prevents the apps from understanding each other's protected payloads.</span></span> <span data-ttu-id="69339-144">Zum Freigeben von geschützter Nutzlasten zwischen zwei Web-apps verwenden [SetApplicationName](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setapplicationname) mit demselben Wert für jede app:</span><span class="sxs-lookup"><span data-stu-id="69339-144">To share protected payloads between two apps, use [SetApplicationName](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setapplicationname) with the same value for each app:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .SetApplicationName("shared app name");
}
```

## <a name="disableautomatickeygeneration"></a><span data-ttu-id="69339-145">DisableAutomaticKeyGeneration</span><span class="sxs-lookup"><span data-stu-id="69339-145">DisableAutomaticKeyGeneration</span></span>

<span data-ttu-id="69339-146">Sie möglicherweise ein Szenario, in dem Sie nicht möchten eine app an (zum Erstellen neuer Schlüssel) Schlüssel automatisch zurücksetzen, wie sie Ablaufdatum nähern.</span><span class="sxs-lookup"><span data-stu-id="69339-146">You may have a scenario where you don't want an app to automatically roll keys (create new keys) as they approach expiration.</span></span> <span data-ttu-id="69339-147">Ein Beispiel hierfür ist möglicherweise apps, die in einer Beziehung primär/Sekundär, in dem nur die primäre app ist verantwortlich für schlüsselverwaltung Bedenken und sekundären apps haben einfach eine schreibgeschützte Ansicht des Rings Schlüssel einrichten.</span><span class="sxs-lookup"><span data-stu-id="69339-147">One example of this might be apps set up in a primary/secondary relationship, where only the primary app is responsible for key management concerns and secondary apps simply have a read-only view of the key ring.</span></span> <span data-ttu-id="69339-148">Die sekundären apps konfiguriert werden können um der Schlüssel Ring als schreibgeschützt zu behandeln, indem Sie die Konfiguration des Systems mit [DisableAutomaticKeyGeneration](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.disableautomatickeygeneration):</span><span class="sxs-lookup"><span data-stu-id="69339-148">The secondary apps can be configured to treat the key ring as read-only by configuring the system with [DisableAutomaticKeyGeneration](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.disableautomatickeygeneration):</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .DisableAutomaticKeyGeneration();
}
```

## <a name="per-application-isolation"></a><span data-ttu-id="69339-149">Pro Anwendungsisolation</span><span class="sxs-lookup"><span data-stu-id="69339-149">Per-application isolation</span></span>

<span data-ttu-id="69339-150">Wenn die Datenschutz-System von einem ASP.NET Core-Host bereitgestellt wird, isoliert es automatisch apps von einem anderen, selbst wenn diese apps mit der gleichen Arbeitsprozesskonto ausgeführt werden und die gleiche Schlüsselmaterial verwenden.</span><span class="sxs-lookup"><span data-stu-id="69339-150">When the Data Protection system is provided by an ASP.NET Core host, it automatically isolates apps from one another, even if those apps are running under the same worker process account and are using the same master keying material.</span></span> <span data-ttu-id="69339-151">Dies ist zumindest ähneln den IsolateApps-Modifizierer aus System.Web  **\<MachineKey >** Element.</span><span class="sxs-lookup"><span data-stu-id="69339-151">This is somewhat similar to the IsolateApps modifier from System.Web's **\<machineKey>** element.</span></span>

<span data-ttu-id="69339-152">Der Mechanismus für die Sicherheitsisolation Hinblick auf jede app auf dem lokalen Computer als einen eindeutigen Mandanten daher funktioniert die [IDataProtector](/dotnet/api/microsoft.aspnetcore.dataprotection.idataprotector) Stamm eine bestimmte app automatisch als Diskriminator app-ID enthält.</span><span class="sxs-lookup"><span data-stu-id="69339-152">The isolation mechanism works by considering each app on the local machine as a unique tenant, thus the [IDataProtector](/dotnet/api/microsoft.aspnetcore.dataprotection.idataprotector) rooted for any given app automatically includes the app ID as a discriminator.</span></span> <span data-ttu-id="69339-153">Eindeutige ID für die app stammt aus einer von zwei Stellen:</span><span class="sxs-lookup"><span data-stu-id="69339-153">The app's unique ID comes from one of two places:</span></span>

1. <span data-ttu-id="69339-154">Wenn die app in IIS gehostet wird, ist der eindeutige Bezeichner der app-Konfigurationspfad an.</span><span class="sxs-lookup"><span data-stu-id="69339-154">If the app is hosted in IIS, the unique identifier is the app's configuration path.</span></span> <span data-ttu-id="69339-155">Wenn eine app in einer webfarmumgebung bereitgestellt wird, sollte dieser Wert stabil sein, vorausgesetzt, dass die IIS-Umgebungen für alle Computer in der Webfarm auf ähnliche Weise konfiguriert werden.</span><span class="sxs-lookup"><span data-stu-id="69339-155">If an app is deployed in a web farm environment, this value should be stable assuming that the IIS environments are configured similarly across all machines in the web farm.</span></span>

2. <span data-ttu-id="69339-156">Wenn die app in IIS gehostet wird nicht, ist der eindeutige Bezeichner der physische Pfad der app an.</span><span class="sxs-lookup"><span data-stu-id="69339-156">If the app isn't hosted in IIS, the unique identifier is the physical path of the app.</span></span>

<span data-ttu-id="69339-157">Der eindeutige Bezeichner dient zum Zurücksetzen von Kennwörtern überstehen &mdash; sowohl der einzelnen app und der Computer.</span><span class="sxs-lookup"><span data-stu-id="69339-157">The unique identifier is designed to survive resets &mdash; both of the individual app and of the machine itself.</span></span>

<span data-ttu-id="69339-158">Dieser Mechanismus für die Sicherheitsisolation wird davon ausgegangen, dass die apps nicht bösartig sind.</span><span class="sxs-lookup"><span data-stu-id="69339-158">This isolation mechanism assumes that the apps are not malicious.</span></span> <span data-ttu-id="69339-159">Eine böswillige Anwendung beeinträchtigt immer jede andere app unter der gleichen Arbeitsprozesskonto ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="69339-159">A malicious app can always impact any other app running under the same worker process account.</span></span> <span data-ttu-id="69339-160">In einer freigegebenen Hostingumgebung nicht gegenseitig vertrauenswürdigen apps gegangenem geboten des hostenden Anbieters Schritte, um auf Betriebssystemebene Isolation zwischen apps, einschließlich trennen, die apps zugrunde liegenden Key Repositorys sicherzustellen.</span><span class="sxs-lookup"><span data-stu-id="69339-160">In a shared hosting environment where apps are mutually untrusted, the hosting provider should take steps to ensure OS-level isolation between apps, including separating the apps' underlying key repositories.</span></span>

<span data-ttu-id="69339-161">Wenn das System Schutz von Daten von einem ASP.NET Core-Host nicht bereitgestellt wird (z. B., wenn Sie auch über Instanziieren der `DataProtectionProvider` konkreten Typ) Isolieren von Apps ist standardmäßig deaktiviert.</span><span class="sxs-lookup"><span data-stu-id="69339-161">If the Data Protection system isn't provided by an ASP.NET Core host (for example, if you instantiate it via the `DataProtectionProvider` concrete type) app isolation is disabled by default.</span></span> <span data-ttu-id="69339-162">Beim Isolieren von Apps deaktiviert ist, alle apps, die durch das gleiche Schlüsselmaterial gesichert können freigeben Nutzlasten solange sie die entsprechende bieten [Zwecke](xref:security/data-protection/consumer-apis/purpose-strings).</span><span class="sxs-lookup"><span data-stu-id="69339-162">When app isolation is disabled, all apps backed by the same keying material can share payloads as long as they provide the appropriate [purposes](xref:security/data-protection/consumer-apis/purpose-strings).</span></span> <span data-ttu-id="69339-163">Zum Isolieren von Apps in dieser Umgebung bereitstellen möchten, rufen Sie die [SetApplicationName](#setapplicationname) Methode für die Konfiguration Objekt, und geben Sie einen eindeutigen Namen für jede app.</span><span class="sxs-lookup"><span data-stu-id="69339-163">To provide app isolation in this environment, call the [SetApplicationName](#setapplicationname) method on the configuration object and provide a unique name for each app.</span></span>

## <a name="changing-algorithms-with-usecryptographicalgorithms"></a><span data-ttu-id="69339-164">Algorithmen mit UseCryptographicAlgorithms ändern</span><span class="sxs-lookup"><span data-stu-id="69339-164">Changing algorithms with UseCryptographicAlgorithms</span></span>

<span data-ttu-id="69339-165">Der Datenschutz-Stapel können Sie ändern, der durch den neu generierten Schlüssel verwendete Standardalgorithmus.</span><span class="sxs-lookup"><span data-stu-id="69339-165">The Data Protection stack allows you to change the default algorithm used by newly-generated keys.</span></span> <span data-ttu-id="69339-166">Die einfachste Möglichkeit hierzu ist das Aufrufen [UseCryptographicAlgorithms](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.usecryptographicalgorithms) vom Rückruf Konfiguration:</span><span class="sxs-lookup"><span data-stu-id="69339-166">The simplest way to do this is to call [UseCryptographicAlgorithms](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.usecryptographicalgorithms) from the configuration callback:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="69339-167">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="69339-167">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
services.AddDataProtection()
    .UseCryptographicAlgorithms(
        new AuthenticatedEncryptorConfiguration()
    {
        EncryptionAlgorithm = EncryptionAlgorithm.AES_256_CBC,
        ValidationAlgorithm = ValidationAlgorithm.HMACSHA256
    });
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="69339-168">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="69339-168">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

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

<span data-ttu-id="69339-169">Der Standardwert "EncryptionAlgorithm" ist AES-256-CBC, und der Standardwert ValidationAlgorithm ist HMACSHA256.</span><span class="sxs-lookup"><span data-stu-id="69339-169">The default EncryptionAlgorithm is AES-256-CBC, and the default ValidationAlgorithm is HMACSHA256.</span></span> <span data-ttu-id="69339-170">Die Standardrichtlinie kann festgelegt werden, von einem Systemadministrator über eine [computerweite Richtlinie](xref:security/data-protection/configuration/machine-wide-policy), aber ein expliziter Aufruf von `UseCryptographicAlgorithms` überschreibt der Standardrichtlinie.</span><span class="sxs-lookup"><span data-stu-id="69339-170">The default policy can be set by a system administrator via a [machine-wide policy](xref:security/data-protection/configuration/machine-wide-policy), but an explicit call to `UseCryptographicAlgorithms` overrides the default policy.</span></span>

<span data-ttu-id="69339-171">Aufrufen von `UseCryptographicAlgorithms` können Sie den gewünschten Algorithmus aus einer vordefinierten integrierten Liste angeben.</span><span class="sxs-lookup"><span data-stu-id="69339-171">Calling `UseCryptographicAlgorithms` allows you to specify the desired algorithm from a predefined built-in list.</span></span> <span data-ttu-id="69339-172">Sie müssen nicht die Implementierung des Algorithmus kümmern.</span><span class="sxs-lookup"><span data-stu-id="69339-172">You don't need to worry about the implementation of the algorithm.</span></span> <span data-ttu-id="69339-173">Im obigen Szenario versucht Datenschutz, der CNG-Implementierung von AES zu verwenden, wenn unter Windows ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="69339-173">In the scenario above, the Data Protection system attempts to use the CNG implementation of AES if running on Windows.</span></span> <span data-ttu-id="69339-174">Andernfalls es fragt die verwaltete [System.Security.Cryptography.Aes](/dotnet/api/system.security.cryptography.aes) Klasse.</span><span class="sxs-lookup"><span data-stu-id="69339-174">Otherwise, it falls back to the managed [System.Security.Cryptography.Aes](/dotnet/api/system.security.cryptography.aes) class.</span></span>

<span data-ttu-id="69339-175">Sie können manuell angeben, um über einen Aufruf an eine Implementierung [UseCustomCryptographicAlgorithms](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.usecustomcryptographicalgorithms).</span><span class="sxs-lookup"><span data-stu-id="69339-175">You can manually specify an implementation via a call to [UseCustomCryptographicAlgorithms](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.usecustomcryptographicalgorithms).</span></span>

> [!TIP]
> <span data-ttu-id="69339-176">Ändern die Algorithmen wirkt sich nicht auf vorhandene Schlüssel im Schlüssel Ring aus.</span><span class="sxs-lookup"><span data-stu-id="69339-176">Changing algorithms doesn't affect existing keys in the key ring.</span></span> <span data-ttu-id="69339-177">Er wirkt sich nur auf neu generierten Schlüssel.</span><span class="sxs-lookup"><span data-stu-id="69339-177">It only affects newly-generated keys.</span></span>

### <a name="specifying-custom-managed-algorithms"></a><span data-ttu-id="69339-178">Angeben von benutzerdefinierten verwalteten Algorithmen</span><span class="sxs-lookup"><span data-stu-id="69339-178">Specifying custom managed algorithms</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="69339-179">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="69339-179">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="69339-180">Um benutzerdefinierte Algorithmen, die verwaltete anzugeben, erstellen Sie eine [ManagedAuthenticatedEncryptorConfiguration](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.configurationmodel.managedauthenticatedencryptorconfiguration) Instanz, die auf die Implementierungstypen verweist:</span><span class="sxs-lookup"><span data-stu-id="69339-180">To specify custom managed algorithms, create a [ManagedAuthenticatedEncryptorConfiguration](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.configurationmodel.managedauthenticatedencryptorconfiguration) instance that points to the implementation types:</span></span>

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

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="69339-181">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="69339-181">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="69339-182">Um benutzerdefinierte Algorithmen, die verwaltete anzugeben, erstellen Sie eine [ManagedAuthenticatedEncryptionSettings](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.managedauthenticatedencryptionsettings) Instanz, die auf die Implementierungstypen verweist:</span><span class="sxs-lookup"><span data-stu-id="69339-182">To specify custom managed algorithms, create a [ManagedAuthenticatedEncryptionSettings](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.managedauthenticatedencryptionsettings) instance that points to the implementation types:</span></span>

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

<span data-ttu-id="69339-183">Im Allgemeinen die \*Typeigenschaften müssen auf konkrete, zeigen instanziierbaren (über einen öffentlichen parameterlosen Ctor) Implementierungen von [SymmetricAlgorithm](/dotnet/api/system.security.cryptography.symmetricalgorithm) und [KeyedHashAlgorithm](/dotnet/api/system.security.cryptography.keyedhashalgorithm), obwohl die System spezielle-Fällen einige Werte wie `typeof(Aes)` der Einfachheit halber.</span><span class="sxs-lookup"><span data-stu-id="69339-183">Generally the \*Type properties must point to concrete, instantiable (via a public parameterless ctor) implementations of [SymmetricAlgorithm](/dotnet/api/system.security.cryptography.symmetricalgorithm) and [KeyedHashAlgorithm](/dotnet/api/system.security.cryptography.keyedhashalgorithm), though the system special-cases some values like `typeof(Aes)` for convenience.</span></span>

> [!NOTE]
> <span data-ttu-id="69339-184">Die SymmetricAlgorithm muss eine Schlüssellänge von ≥ 128 Bits und eine Blockgröße von ≥ 64 Bits haben, und es muss CBC-Modus-Verschlüsselung mit PKCS #7-Auffüllung unterstützen.</span><span class="sxs-lookup"><span data-stu-id="69339-184">The SymmetricAlgorithm must have a key length of ≥ 128 bits and a block size of ≥ 64 bits, and it must support CBC-mode encryption with PKCS #7 padding.</span></span> <span data-ttu-id="69339-185">Der KeyedHashAlgorithm benötigen Nachrichtenhash Größe > = 128 Bits und müssen Schlüssel, der den Hashalgorithmus Digestlänge gleich Länge unterstützt.</span><span class="sxs-lookup"><span data-stu-id="69339-185">The KeyedHashAlgorithm must have a digest size of >= 128 bits, and it must support keys of length equal to the hash algorithm's digest length.</span></span> <span data-ttu-id="69339-186">Die KeyedHashAlgorithm ist nicht zwingend erforderlich, dass HMAC.</span><span class="sxs-lookup"><span data-stu-id="69339-186">The KeyedHashAlgorithm isn't strictly required to be HMAC.</span></span>

### <a name="specifying-custom-windows-cng-algorithms"></a><span data-ttu-id="69339-187">Angeben von benutzerdefinierten Windows CNG-Algorithmen</span><span class="sxs-lookup"><span data-stu-id="69339-187">Specifying custom Windows CNG algorithms</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="69339-188">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="69339-188">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="69339-189">Um einen benutzerdefinierten Windows CNG-Algorithmus HMAC-Validierung mit Verschlüsselung im CBC-Modus anzugeben, erstellen eine [CngCbcAuthenticatedEncryptorConfiguration](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.configurationmodel.cngcbcauthenticatedencryptorconfiguration) Instanz, die die algorithmische Informationen enthält:</span><span class="sxs-lookup"><span data-stu-id="69339-189">To specify a custom Windows CNG algorithm using CBC-mode encryption with HMAC validation, create a [CngCbcAuthenticatedEncryptorConfiguration](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.configurationmodel.cngcbcauthenticatedencryptorconfiguration) instance that contains the algorithmic information:</span></span>

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

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="69339-190">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="69339-190">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="69339-191">Um einen benutzerdefinierten Windows CNG-Algorithmus HMAC-Validierung mit Verschlüsselung im CBC-Modus anzugeben, erstellen eine [CngCbcAuthenticatedEncryptionSettings](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.cngcbcauthenticatedencryptionsettings) Instanz, die die algorithmische Informationen enthält:</span><span class="sxs-lookup"><span data-stu-id="69339-191">To specify a custom Windows CNG algorithm using CBC-mode encryption with HMAC validation, create a [CngCbcAuthenticatedEncryptionSettings](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.cngcbcauthenticatedencryptionsettings) instance that contains the algorithmic information:</span></span>

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
> <span data-ttu-id="69339-192">Der Block symmetrischen Verschlüsselungsalgorithmus benötigen eine Schlüssellänge von > = 128 Bits und eine Blockgröße von > = 64 Bits und müssen entdeckt CBC-Modus-Verschlüsselung mit PKCS #7-Auffüllung.</span><span class="sxs-lookup"><span data-stu-id="69339-192">The symmetric block cipher algorithm must have a key length of >= 128 bits, a block size of >= 64 bits, and it must support CBC-mode encryption with PKCS #7 padding.</span></span> <span data-ttu-id="69339-193">Der Hashalgorithmus benötigen Nachrichtenhash Größe von > = 128 Bits und müssen unterstützen geöffnet, die mit dem symmetrischen BCRYPT\_"alg"\_BEHANDELN\_HMAC\_FLAG-Flag.</span><span class="sxs-lookup"><span data-stu-id="69339-193">The hash algorithm must have a digest size of >= 128 bits and must support being opened with the BCRYPT\_ALG\_HANDLE\_HMAC\_FLAG flag.</span></span> <span data-ttu-id="69339-194">Die \*Anbietereigenschaften können werden auf null festgelegt, um den Standardanbieter für den angegebenen Algorithmus zu verwenden.</span><span class="sxs-lookup"><span data-stu-id="69339-194">The \*Provider properties can be set to null to use the default provider for the specified algorithm.</span></span> <span data-ttu-id="69339-195">Finden Sie unter der [BCryptOpenAlgorithmProvider](https://msdn.microsoft.com/library/windows/desktop/aa375479(v=vs.85).aspx) Dokumentation weitere Informationen.</span><span class="sxs-lookup"><span data-stu-id="69339-195">See the [BCryptOpenAlgorithmProvider](https://msdn.microsoft.com/library/windows/desktop/aa375479(v=vs.85).aspx) documentation for more information.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="69339-196">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="69339-196">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="69339-197">Erstellen Sie zum Angeben eines benutzerdefinierten Windows CNG-Algorithmus Überprüfung Galois/Counter-Modus-Verschlüsselung mit einem [CngGcmAuthenticatedEncryptorConfiguration](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.configurationmodel.cnggcmauthenticatedencryptorconfiguration) Instanz, die die algorithmische Informationen enthält:</span><span class="sxs-lookup"><span data-stu-id="69339-197">To specify a custom Windows CNG algorithm using Galois/Counter Mode encryption with validation, create a [CngGcmAuthenticatedEncryptorConfiguration](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.configurationmodel.cnggcmauthenticatedencryptorconfiguration) instance that contains the algorithmic information:</span></span>

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

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="69339-198">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="69339-198">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="69339-199">Erstellen Sie zum Angeben eines benutzerdefinierten Windows CNG-Algorithmus Überprüfung Galois/Counter-Modus-Verschlüsselung mit einem [CngGcmAuthenticatedEncryptionSettings](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.cnggcmauthenticatedencryptionsettings) Instanz, die die algorithmische Informationen enthält:</span><span class="sxs-lookup"><span data-stu-id="69339-199">To specify a custom Windows CNG algorithm using Galois/Counter Mode encryption with validation, create a [CngGcmAuthenticatedEncryptionSettings](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.cnggcmauthenticatedencryptionsettings) instance that contains the algorithmic information:</span></span>

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
> <span data-ttu-id="69339-200">Der Block symmetrischen Verschlüsselungsalgorithmus benötigen eine Schlüssellänge von > = 128 Bits und eine Blockgröße von genau 128 Bits und müssen GCM-Verschlüsselung unterstützt.</span><span class="sxs-lookup"><span data-stu-id="69339-200">The symmetric block cipher algorithm must have a key length of >= 128 bits, a block size of exactly 128 bits, and it must support GCM encryption.</span></span> <span data-ttu-id="69339-201">Sie können festlegen, die [EncryptionAlgorithmProvider](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.configurationmodel.cngcbcauthenticatedencryptorconfiguration.encryptionalgorithmprovider) -Eigenschaft auf null, um den Standardanbieter für den angegebenen Algorithmus zu verwenden.</span><span class="sxs-lookup"><span data-stu-id="69339-201">You can set the [EncryptionAlgorithmProvider](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.configurationmodel.cngcbcauthenticatedencryptorconfiguration.encryptionalgorithmprovider) property to null to use the default provider for the specified algorithm.</span></span> <span data-ttu-id="69339-202">Finden Sie unter der [BCryptOpenAlgorithmProvider](https://msdn.microsoft.com/library/windows/desktop/aa375479(v=vs.85).aspx) Dokumentation weitere Informationen.</span><span class="sxs-lookup"><span data-stu-id="69339-202">See the [BCryptOpenAlgorithmProvider](https://msdn.microsoft.com/library/windows/desktop/aa375479(v=vs.85).aspx) documentation for more information.</span></span>

### <a name="specifying-other-custom-algorithms"></a><span data-ttu-id="69339-203">Andere benutzerdefinierten Algorithmen angeben</span><span class="sxs-lookup"><span data-stu-id="69339-203">Specifying other custom algorithms</span></span>

<span data-ttu-id="69339-204">Obwohl nicht als eine erstklassige-API verfügbar gemacht, ist das Data Protection-System extensible genug, um zu ermöglichen, nahezu jede Art von Algorithmus angibt.</span><span class="sxs-lookup"><span data-stu-id="69339-204">Though not exposed as a first-class API, the Data Protection system is extensible enough to allow specifying almost any kind of algorithm.</span></span> <span data-ttu-id="69339-205">Beispielsweise ist es möglich, alle Schlüssel in einem Hardwaresicherheitsmodul (HSM) enthaltenen zu behalten und eine benutzerdefinierte Implementierung des Kerns Ver- und Entschlüsselung Routinen bereitstellen.</span><span class="sxs-lookup"><span data-stu-id="69339-205">For example, it's possible to keep all keys contained within a Hardware Security Module (HSM) and to provide a custom implementation of the core encryption and decryption routines.</span></span> <span data-ttu-id="69339-206">Finden Sie unter [IAuthenticatedEncryptor](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.iauthenticatedencryptor) in [Core kryptografieerweiterbarkeit](xref:security/data-protection/extensibility/core-crypto) für Weitere Informationen.</span><span class="sxs-lookup"><span data-stu-id="69339-206">See [IAuthenticatedEncryptor](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.iauthenticatedencryptor) in [Core cryptography extensibility](xref:security/data-protection/extensibility/core-crypto) for more information.</span></span>

## <a name="persisting-keys-when-hosting-in-a-docker-container"></a><span data-ttu-id="69339-207">Beibehalten von Schlüsseln, wenn in einem Docker-Container gehostet wird.</span><span class="sxs-lookup"><span data-stu-id="69339-207">Persisting keys when hosting in a Docker container</span></span>

<span data-ttu-id="69339-208">Beim Hosten in einer [Docker](/dotnet/standard/microservices-architecture/container-docker-introduction/) Container Schlüssel sollte entweder beibehalten werden:</span><span class="sxs-lookup"><span data-stu-id="69339-208">When hosting in a [Docker](/dotnet/standard/microservices-architecture/container-docker-introduction/) container, keys should be maintained in either:</span></span>

* <span data-ttu-id="69339-209">Einen Ordner, der ein Docker-Volume ist, die nach Ablauf des Containers Lebensdauer, z. B. ein freigegebenes Volume oder einem Host bereitgestellten Volume beibehält.</span><span class="sxs-lookup"><span data-stu-id="69339-209">A folder that's a Docker volume that persists beyond the container's lifetime, such as a shared volume or a host-mounted volume.</span></span>
* <span data-ttu-id="69339-210">Ein externer Anbieter, wie z. B. [Azure Key Vault](https://azure.microsoft.com/services/key-vault/) oder [Redis](https://redis.io/).</span><span class="sxs-lookup"><span data-stu-id="69339-210">An external provider, such as [Azure Key Vault](https://azure.microsoft.com/services/key-vault/) or [Redis](https://redis.io/).</span></span>

## <a name="see-also"></a><span data-ttu-id="69339-211">Siehe auch</span><span class="sxs-lookup"><span data-stu-id="69339-211">See also</span></span>

* [<span data-ttu-id="69339-212">Szenarios, die die Abhängigkeitsinjektion nicht beachten</span><span class="sxs-lookup"><span data-stu-id="69339-212">Non DI Aware Scenarios</span></span>](xref:security/data-protection/configuration/non-di-scenarios)
* [<span data-ttu-id="69339-213">Computerübergreifende Richtlinie</span><span class="sxs-lookup"><span data-stu-id="69339-213">Machine Wide Policy</span></span>](xref:security/data-protection/configuration/machine-wide-policy)
