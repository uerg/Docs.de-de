---
title: "In softwareschlüsselspeicher-Anbieter"
author: rick-anderson
description: "In softwareschlüsselspeicher-Anbieter"
keywords: "ASP.NET Core-Verschlüsselung,"
ms.author: riande
manager: wpickett
ms.date: 01/14/2017
ms.topic: article
ms.assetid: 423e0a79-2f34-44c4-aaf3-146a53c39251
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/implementation/key-storage-providers
ms.openlocfilehash: 0d604c1aab1b7bad5b9f5c7d21edf12e8db7c563
ms.sourcegitcommit: 67f54fabbfa4e3942f5bfe1f8a7fdfe4a7a75358
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/19/2017
---
# <a name="key-storage-providers"></a><span data-ttu-id="c43da-104">In softwareschlüsselspeicher-Anbieter</span><span class="sxs-lookup"><span data-stu-id="c43da-104">Key storage providers</span></span>

<a name=data-protection-implementation-key-storage-providers></a>

<span data-ttu-id="c43da-105">Standardmäßig die Datenschutzsystem [verwendet eine Heuristik](../configuration/default-settings.md#data-protection-default-settings) um zu bestimmen, wo die kryptografischen schlüsselmaterialien beibehalten werden soll.</span><span class="sxs-lookup"><span data-stu-id="c43da-105">By default the data protection system [employs a heuristic](../configuration/default-settings.md#data-protection-default-settings) to determine where cryptographic key material should be persisted.</span></span> <span data-ttu-id="c43da-106">Der Entwickler kann außer Kraft setzen die Heuristik und manuell Geben Sie den Speicherort.</span><span class="sxs-lookup"><span data-stu-id="c43da-106">The developer can override the heuristic and manually specify the location.</span></span>

> [!NOTE]
> <span data-ttu-id="c43da-107">Wenn Sie einen expliziten schlüsselpersistenz-Ort angeben, wird die Datenschutzsystem Aufheben der Registrierung der wichtigsten standardverschlüsselung zur Rest-Mechanismus, der die Heuristik zur Verfügung gestellt, sodass Schlüssel nicht mehr im Ruhezustand verschlüsselt werden.</span><span class="sxs-lookup"><span data-stu-id="c43da-107">If you specify an explicit key persistence location, the data protection system will deregister the default key encryption at rest mechanism that the heuristic provided, so keys will no longer be encrypted at rest.</span></span> <span data-ttu-id="c43da-108">Es wird empfohlen, die Sie darüber hinaus [geben den Kommunikationsmechanismus für eine explizite Schlüsselverschlüsselung](key-encryption-at-rest.md#data-protection-implementation-key-encryption-at-rest-providers) für Produktionsanwendungen.</span><span class="sxs-lookup"><span data-stu-id="c43da-108">It is recommended that you additionally [specify an explicit key encryption mechanism](key-encryption-at-rest.md#data-protection-implementation-key-encryption-at-rest-providers) for production applications.</span></span>

<span data-ttu-id="c43da-109">Im Lieferumfang von der Datenschutzsystem sind mehrere integrierte-Schlüsselspeicher-Anbieter.</span><span class="sxs-lookup"><span data-stu-id="c43da-109">The data protection system ships with several in-box key storage providers.</span></span>

## <a name="file-system"></a><span data-ttu-id="c43da-110">Dateisystem</span><span class="sxs-lookup"><span data-stu-id="c43da-110">File system</span></span>

<span data-ttu-id="c43da-111">Wir erwarten, dass viele apps Datei systembasierten Key-Repository verwendet.</span><span class="sxs-lookup"><span data-stu-id="c43da-111">We anticipate that many apps will use a file system-based key repository.</span></span> <span data-ttu-id="c43da-112">Um dies zu konfigurieren, rufen Sie die [PersistKeysToFileSystem](https://github.com/aspnet/DataProtection/blob/rel/1.1.0/src/Microsoft.AspNetCore.DataProtection/DataProtectionBuilderExtensions.cs) Konfiguration Routine, wie unten dargestellt.</span><span class="sxs-lookup"><span data-stu-id="c43da-112">To configure this, call the [PersistKeysToFileSystem](https://github.com/aspnet/DataProtection/blob/rel/1.1.0/src/Microsoft.AspNetCore.DataProtection/DataProtectionBuilderExtensions.cs) configuration routine as shown below.</span></span> <span data-ttu-id="c43da-113">Geben Sie einen `DirectoryInfo` verweist auf das Repository, in dem Schlüssel gespeichert werden sollen.</span><span class="sxs-lookup"><span data-stu-id="c43da-113">Provide a `DirectoryInfo` pointing to the repository where keys should be stored.</span></span>

```csharp
sc.AddDataProtection()
       // persist keys to a specific directory
       .PersistKeysToFileSystem(new DirectoryInfo(@"c:\temp-keys\"));
   ```

<span data-ttu-id="c43da-114">Die `DirectoryInfo` kann auf ein Verzeichnis auf dem lokalen Computer verweisen, oder kann auf einen Ordner auf einer Netzwerkfreigabe zeigen.</span><span class="sxs-lookup"><span data-stu-id="c43da-114">The `DirectoryInfo` can point to a directory on the local machine, or it can point to a folder on a network share.</span></span> <span data-ttu-id="c43da-115">Wenn verweist auf ein Verzeichnis auf dem lokalen Computer (und das Szenario liegt vor, dass nur die Anwendungen auf dem lokalen Computer diesem Repository verwendet müssen), erwägen Sie [Windows DPAPI](key-encryption-at-rest.md#data-protection-implementation-key-encryption-at-rest) zum Verschlüsseln der Schlüssel im Ruhezustand.</span><span class="sxs-lookup"><span data-stu-id="c43da-115">If pointing to a directory on the local machine (and the scenario is that only applications on the local machine will need to use this repository), consider using [Windows DPAPI](key-encryption-at-rest.md#data-protection-implementation-key-encryption-at-rest) to encrypt the keys at rest.</span></span> <span data-ttu-id="c43da-116">Erwägen Sie andernfalls mit einem [x. 509-Zertifikat](key-encryption-at-rest.md#data-protection-implementation-key-encryption-at-rest) um ruhende Verschlüsselung.</span><span class="sxs-lookup"><span data-stu-id="c43da-116">Otherwise consider using an [X.509 certificate](key-encryption-at-rest.md#data-protection-implementation-key-encryption-at-rest) to encrypt keys at rest.</span></span>

## <a name="azure-and-redis"></a><span data-ttu-id="c43da-117">Azure und Redis</span><span class="sxs-lookup"><span data-stu-id="c43da-117">Azure and Redis</span></span>

<span data-ttu-id="c43da-118">Die `Microsoft.AspNetCore.DataProtection.AzureStorage` und `Microsoft.AspNetCore.DataProtection.Redis` Pakete zu ermöglichen, Ihre Data Protection-Schlüssel in Azure-Speicher oder eine Redis-Cache gespeichert.</span><span class="sxs-lookup"><span data-stu-id="c43da-118">The `Microsoft.AspNetCore.DataProtection.AzureStorage` and `Microsoft.AspNetCore.DataProtection.Redis` packages allow storing your data protection keys in Azure Storage or a Redis cache.</span></span> <span data-ttu-id="c43da-119">Schlüssel können über mehrere Instanzen von einer Web-app freigegeben werden.</span><span class="sxs-lookup"><span data-stu-id="c43da-119">Keys can be shared across several instances of a web app.</span></span> <span data-ttu-id="c43da-120">Ihre app ASP.NET Core kann Authentifizierungscookies oder CSRF-Schutz für mehrere Server freigeben.</span><span class="sxs-lookup"><span data-stu-id="c43da-120">Your ASP.NET Core app can share authentication cookies or CSRF protection across multiple servers.</span></span> <span data-ttu-id="c43da-121">Um auf Azure zu konfigurieren, rufen Sie eine von der [PersistKeysToAzureBlobStorage](https://github.com/aspnet/DataProtection/blob/rel/1.1.0/src/Microsoft.AspNetCore.DataProtection.AzureStorage/AzureDataProtectionBuilderExtensions.cs) überlädt, wie unten dargestellt.</span><span class="sxs-lookup"><span data-stu-id="c43da-121">To configure on Azure, call one of the [PersistKeysToAzureBlobStorage](https://github.com/aspnet/DataProtection/blob/rel/1.1.0/src/Microsoft.AspNetCore.DataProtection.AzureStorage/AzureDataProtectionBuilderExtensions.cs) overloads as shown below.</span></span>

```
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .PersistKeysToAzureBlobStorage(new Uri("<blob URI including SAS token>"));

    services.AddMvc();
}
```

<span data-ttu-id="c43da-122">Siehe auch die [Azure Testcode](https://github.com/aspnet/DataProtection/blob/rel/1.1.0/samples/AzureBlob/Program.cs).</span><span class="sxs-lookup"><span data-stu-id="c43da-122">See also the [Azure test code](https://github.com/aspnet/DataProtection/blob/rel/1.1.0/samples/AzureBlob/Program.cs).</span></span>

<span data-ttu-id="c43da-123">Um auf Redis konfigurieren, rufen Sie eine von der [PersistKeysToRedis](https://github.com/aspnet/DataProtection/blob/rel/1.1.0/src/Microsoft.AspNetCore.DataProtection.Redis/RedisDataProtectionBuilderExtensions.cs) überlädt, wie unten dargestellt.</span><span class="sxs-lookup"><span data-stu-id="c43da-123">To configure on Redis, call one of the [PersistKeysToRedis](https://github.com/aspnet/DataProtection/blob/rel/1.1.0/src/Microsoft.AspNetCore.DataProtection.Redis/RedisDataProtectionBuilderExtensions.cs) overloads as shown below.</span></span>

```
public void ConfigureServices(IServiceCollection services)
{
    // Connect to Redis database.
    var redis = ConnectionMultiplexer.Connect("<URI>");
    services.AddDataProtection()
        .PersistKeysToRedis(redis, "DataProtection-Keys");

    services.AddMvc();
}
```

<span data-ttu-id="c43da-124">Über die folgenden Links erhalten Sie weitere Informationen:</span><span class="sxs-lookup"><span data-stu-id="c43da-124">See the following for more information:</span></span>

- [<span data-ttu-id="c43da-125">StackExchange.Redis ConnectionMultiplexer</span><span class="sxs-lookup"><span data-stu-id="c43da-125">StackExchange.Redis ConnectionMultiplexer</span></span>](https://github.com/StackExchange/StackExchange.Redis/blob/master/docs/Basics.md)
- [<span data-ttu-id="c43da-126">Azure Redis Cache:</span><span class="sxs-lookup"><span data-stu-id="c43da-126">Azure Redis Cache</span></span>](https://docs.microsoft.com/azure/redis-cache/cache-dotnet-how-to-use-azure-redis-cache#connect-to-the-cache)
- <span data-ttu-id="c43da-127">[Redis Testcode](https://github.com/aspnet/DataProtection/blob/rel/1.1.0/samples/Redis/Program.cs).</span><span class="sxs-lookup"><span data-stu-id="c43da-127">[Redis test code](https://github.com/aspnet/DataProtection/blob/rel/1.1.0/samples/Redis/Program.cs).</span></span>

## <a name="registry"></a><span data-ttu-id="c43da-128">Registrierung</span><span class="sxs-lookup"><span data-stu-id="c43da-128">Registry</span></span>

<span data-ttu-id="c43da-129">In einigen Fällen möglicherweise die app nicht Schreibzugriff auf das Dateisystem.</span><span class="sxs-lookup"><span data-stu-id="c43da-129">Sometimes the app might not have write access to the file system.</span></span> <span data-ttu-id="c43da-130">Betrachten Sie ein Szenario, in denen eine app als virtuelle Dienstkonto (z. B. die app-Anwendungspoolidentität des W3wp.exe) ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="c43da-130">Consider a scenario where an app is running as a virtual service account (such as w3wp.exe's app pool identity).</span></span> <span data-ttu-id="c43da-131">In diesen Fällen ist kann der Administrator einen Registrierungsschlüssel bereitgestellt haben, der entsprechenden ACLed für die Dienstidentität für das Konto ist.</span><span class="sxs-lookup"><span data-stu-id="c43da-131">In these cases, the administrator may have provisioned a registry key that is appropriate ACLed for the service account identity.</span></span> <span data-ttu-id="c43da-132">Rufen Sie die [PersistKeysToRegistry](https://github.com/aspnet/DataProtection/blob/rel/1.1.0/src/Microsoft.AspNetCore.DataProtection/DataProtectionBuilderExtensions.cs) Konfiguration Routine, wie unten dargestellt.</span><span class="sxs-lookup"><span data-stu-id="c43da-132">Call the [PersistKeysToRegistry](https://github.com/aspnet/DataProtection/blob/rel/1.1.0/src/Microsoft.AspNetCore.DataProtection/DataProtectionBuilderExtensions.cs) configuration routine as shown below.</span></span> <span data-ttu-id="c43da-133">Geben Sie einen `RegistryKey` verweist auf den Speicherort, in dem kryptografischen Schlüssel/Werte gespeichert werden sollen.</span><span class="sxs-lookup"><span data-stu-id="c43da-133">Provide a `RegistryKey` pointing to the location where cryptographic keys/values should be stored.</span></span>

```csharp
   sc.AddDataProtection()
       // persist keys to a specific location in the system registry
       .PersistKeysToRegistry(Registry.CurrentUser.OpenSubKey(@"SOFTWARE\Sample\keys"));
   ```

<span data-ttu-id="c43da-134">Wenn Sie die Registrierung des Systems als Mechanismus für die Persistenz verwenden, erwägen Sie [Windows DPAPI](key-encryption-at-rest.md#data-protection-implementation-key-encryption-at-rest) zum Verschlüsseln der Schlüssel im Ruhezustand.</span><span class="sxs-lookup"><span data-stu-id="c43da-134">If you use the system registry as a persistence mechanism, consider using [Windows DPAPI](key-encryption-at-rest.md#data-protection-implementation-key-encryption-at-rest) to encrypt the keys at rest.</span></span>

## <a name="custom-key-repository"></a><span data-ttu-id="c43da-135">Benutzerdefinierte Schlüssel-repository</span><span class="sxs-lookup"><span data-stu-id="c43da-135">Custom key repository</span></span>

<span data-ttu-id="c43da-136">Wenn die integrierten Mechanismen nicht geeignet sind, kann der Entwickler einen eigenen Mechanismus schlüsselpersistenz angeben, durch Bereitstellen eines benutzerdefinierten `IXmlRepository`.</span><span class="sxs-lookup"><span data-stu-id="c43da-136">If the in-box mechanisms are not appropriate, the developer can specify their own key persistence mechanism by providing a custom `IXmlRepository`.</span></span>
