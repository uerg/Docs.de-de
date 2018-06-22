---
title: In softwareschlüsselspeicher-Anbieter in ASP.NET Core
author: rick-anderson
description: Informationen Sie zur softwareschlüsselspeicher-Anbieter in ASP.NET Core und wichtige Speicherorte zu konfigurieren.
ms.author: riande
ms.date: 01/14/2017
uid: security/data-protection/implementation/key-storage-providers
ms.openlocfilehash: 432c2690f216325470bbea9b974ea772bcdc39ed
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/20/2018
ms.locfileid: "36273768"
---
# <a name="key-storage-providers-in-aspnet-core"></a><span data-ttu-id="7c14c-103">In softwareschlüsselspeicher-Anbieter in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="7c14c-103">Key storage providers in ASP.NET Core</span></span>

<a name="data-protection-implementation-key-storage-providers"></a>

<span data-ttu-id="7c14c-104">Standardmäßig die Datenschutzsystem [verwendet eine Heuristik](xref:security/data-protection/configuration/default-settings) um zu bestimmen, wo die kryptografischen schlüsselmaterialien beibehalten werden soll.</span><span class="sxs-lookup"><span data-stu-id="7c14c-104">By default the data protection system [employs a heuristic](xref:security/data-protection/configuration/default-settings) to determine where cryptographic key material should be persisted.</span></span> <span data-ttu-id="7c14c-105">Der Entwickler kann außer Kraft setzen die Heuristik und manuell Geben Sie den Speicherort.</span><span class="sxs-lookup"><span data-stu-id="7c14c-105">The developer can override the heuristic and manually specify the location.</span></span>

> [!NOTE]
> <span data-ttu-id="7c14c-106">Wenn Sie einen expliziten schlüsselpersistenz-Ort angeben, wird die Datenschutzsystem Aufheben der Registrierung der wichtigsten standardverschlüsselung zur Rest-Mechanismus, der die Heuristik zur Verfügung gestellt, sodass Schlüssel nicht mehr im Ruhezustand verschlüsselt werden.</span><span class="sxs-lookup"><span data-stu-id="7c14c-106">If you specify an explicit key persistence location, the data protection system will deregister the default key encryption at rest mechanism that the heuristic provided, so keys will no longer be encrypted at rest.</span></span> <span data-ttu-id="7c14c-107">Es wird empfohlen, die Sie darüber hinaus [geben den Kommunikationsmechanismus für eine explizite Schlüsselverschlüsselung](xref:security/data-protection/implementation/key-encryption-at-rest#data-protection-implementation-key-encryption-at-rest-providers) für Produktionsanwendungen.</span><span class="sxs-lookup"><span data-stu-id="7c14c-107">It's recommended that you additionally [specify an explicit key encryption mechanism](xref:security/data-protection/implementation/key-encryption-at-rest#data-protection-implementation-key-encryption-at-rest-providers) for production applications.</span></span>

<span data-ttu-id="7c14c-108">Im Lieferumfang von der Datenschutzsystem sind mehrere integrierte-Schlüsselspeicher-Anbieter.</span><span class="sxs-lookup"><span data-stu-id="7c14c-108">The data protection system ships with several in-box key storage providers.</span></span>

## <a name="file-system"></a><span data-ttu-id="7c14c-109">Dateisystem</span><span class="sxs-lookup"><span data-stu-id="7c14c-109">File system</span></span>

<span data-ttu-id="7c14c-110">Wir erwarten, dass viele apps Datei systembasierten Key-Repository verwendet.</span><span class="sxs-lookup"><span data-stu-id="7c14c-110">We anticipate that many apps will use a file system-based key repository.</span></span> <span data-ttu-id="7c14c-111">Um dies zu konfigurieren, rufen Sie die [PersistKeysToFileSystem](https://github.com/aspnet/DataProtection/blob/rel/1.1.0/src/Microsoft.AspNetCore.DataProtection/DataProtectionBuilderExtensions.cs) Konfiguration Routine, wie unten dargestellt.</span><span class="sxs-lookup"><span data-stu-id="7c14c-111">To configure this, call the [PersistKeysToFileSystem](https://github.com/aspnet/DataProtection/blob/rel/1.1.0/src/Microsoft.AspNetCore.DataProtection/DataProtectionBuilderExtensions.cs) configuration routine as shown below.</span></span> <span data-ttu-id="7c14c-112">Geben Sie einen `DirectoryInfo` verweist auf das Repository, in dem Schlüssel gespeichert werden sollen.</span><span class="sxs-lookup"><span data-stu-id="7c14c-112">Provide a `DirectoryInfo` pointing to the repository where keys should be stored.</span></span>

```csharp
sc.AddDataProtection()
       // persist keys to a specific directory
       .PersistKeysToFileSystem(new DirectoryInfo(@"c:\temp-keys\"));
   ```

<span data-ttu-id="7c14c-113">Die `DirectoryInfo` kann auf ein Verzeichnis auf dem lokalen Computer verweisen, oder kann auf einen Ordner auf einer Netzwerkfreigabe zeigen.</span><span class="sxs-lookup"><span data-stu-id="7c14c-113">The `DirectoryInfo` can point to a directory on the local machine, or it can point to a folder on a network share.</span></span> <span data-ttu-id="7c14c-114">Wenn verweist auf ein Verzeichnis auf dem lokalen Computer (und das Szenario liegt vor, dass nur die Anwendungen auf dem lokalen Computer diesem Repository verwendet müssen), erwägen Sie [Windows DPAPI](xref:security/data-protection/implementation/key-encryption-at-rest#data-protection-implementation-key-encryption-at-rest) zum Verschlüsseln der Schlüssel im Ruhezustand.</span><span class="sxs-lookup"><span data-stu-id="7c14c-114">If pointing to a directory on the local machine (and the scenario is that only applications on the local machine will need to use this repository), consider using [Windows DPAPI](xref:security/data-protection/implementation/key-encryption-at-rest#data-protection-implementation-key-encryption-at-rest) to encrypt the keys at rest.</span></span> <span data-ttu-id="7c14c-115">Erwägen Sie andernfalls mit einem [x. 509-Zertifikat](xref:security/data-protection/implementation/key-encryption-at-rest#data-protection-implementation-key-encryption-at-rest) um ruhende Verschlüsselung.</span><span class="sxs-lookup"><span data-stu-id="7c14c-115">Otherwise consider using an [X.509 certificate](xref:security/data-protection/implementation/key-encryption-at-rest#data-protection-implementation-key-encryption-at-rest) to encrypt keys at rest.</span></span>

## <a name="azure-and-redis"></a><span data-ttu-id="7c14c-116">Azure und Redis</span><span class="sxs-lookup"><span data-stu-id="7c14c-116">Azure and Redis</span></span>

<span data-ttu-id="7c14c-117">Die `Microsoft.AspNetCore.DataProtection.AzureStorage` und `Microsoft.AspNetCore.DataProtection.Redis` Pakete zu ermöglichen, Ihre Data Protection-Schlüssel in Azure-Speicher oder eine Redis-Cache gespeichert.</span><span class="sxs-lookup"><span data-stu-id="7c14c-117">The `Microsoft.AspNetCore.DataProtection.AzureStorage` and `Microsoft.AspNetCore.DataProtection.Redis` packages allow storing your data protection keys in Azure Storage or a Redis cache.</span></span> <span data-ttu-id="7c14c-118">Schlüssel können über mehrere Instanzen von einer Web-app freigegeben werden.</span><span class="sxs-lookup"><span data-stu-id="7c14c-118">Keys can be shared across several instances of a web app.</span></span> <span data-ttu-id="7c14c-119">Ihre app ASP.NET Core kann Authentifizierungscookies oder CSRF-Schutz für mehrere Server freigeben.</span><span class="sxs-lookup"><span data-stu-id="7c14c-119">Your ASP.NET Core app can share authentication cookies or CSRF protection across multiple servers.</span></span> <span data-ttu-id="7c14c-120">Um auf Azure zu konfigurieren, rufen Sie eine von der [PersistKeysToAzureBlobStorage](https://github.com/aspnet/DataProtection/blob/rel/1.1.0/src/Microsoft.AspNetCore.DataProtection.AzureStorage/AzureDataProtectionBuilderExtensions.cs) überlädt, wie unten dargestellt.</span><span class="sxs-lookup"><span data-stu-id="7c14c-120">To configure on Azure, call one of the [PersistKeysToAzureBlobStorage](https://github.com/aspnet/DataProtection/blob/rel/1.1.0/src/Microsoft.AspNetCore.DataProtection.AzureStorage/AzureDataProtectionBuilderExtensions.cs) overloads as shown below.</span></span>

```
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .PersistKeysToAzureBlobStorage(new Uri("<blob URI including SAS token>"));

    services.AddMvc();
}
```

<span data-ttu-id="7c14c-121">Siehe auch die [Azure Testcode](https://github.com/aspnet/DataProtection/blob/rel/1.1.0/samples/AzureBlob/Program.cs).</span><span class="sxs-lookup"><span data-stu-id="7c14c-121">See also the [Azure test code](https://github.com/aspnet/DataProtection/blob/rel/1.1.0/samples/AzureBlob/Program.cs).</span></span>

<span data-ttu-id="7c14c-122">Um auf Redis konfigurieren, rufen Sie eine von der [PersistKeysToRedis](https://github.com/aspnet/DataProtection/blob/rel/1.1.0/src/Microsoft.AspNetCore.DataProtection.Redis/RedisDataProtectionBuilderExtensions.cs) überlädt, wie unten dargestellt.</span><span class="sxs-lookup"><span data-stu-id="7c14c-122">To configure on Redis, call one of the [PersistKeysToRedis](https://github.com/aspnet/DataProtection/blob/rel/1.1.0/src/Microsoft.AspNetCore.DataProtection.Redis/RedisDataProtectionBuilderExtensions.cs) overloads as shown below.</span></span>

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

<span data-ttu-id="7c14c-123">Über die folgenden Links erhalten Sie weitere Informationen:</span><span class="sxs-lookup"><span data-stu-id="7c14c-123">See the following for more information:</span></span>

- [<span data-ttu-id="7c14c-124">StackExchange.Redis ConnectionMultiplexer</span><span class="sxs-lookup"><span data-stu-id="7c14c-124">StackExchange.Redis ConnectionMultiplexer</span></span>](https://github.com/StackExchange/StackExchange.Redis/blob/master/docs/Basics.md)
- [<span data-ttu-id="7c14c-125">Azure Redis Cache:</span><span class="sxs-lookup"><span data-stu-id="7c14c-125">Azure Redis Cache</span></span>](https://docs.microsoft.com/azure/redis-cache/cache-dotnet-how-to-use-azure-redis-cache#connect-to-the-cache)
- <span data-ttu-id="7c14c-126">[Redis Testcode](https://github.com/aspnet/DataProtection/blob/rel/1.1.0/samples/Redis/Program.cs).</span><span class="sxs-lookup"><span data-stu-id="7c14c-126">[Redis test code](https://github.com/aspnet/DataProtection/blob/rel/1.1.0/samples/Redis/Program.cs).</span></span>

## <a name="registry"></a><span data-ttu-id="7c14c-127">Registrierung</span><span class="sxs-lookup"><span data-stu-id="7c14c-127">Registry</span></span>

<span data-ttu-id="7c14c-128">In einigen Fällen möglicherweise die app nicht Schreibzugriff auf das Dateisystem.</span><span class="sxs-lookup"><span data-stu-id="7c14c-128">Sometimes the app might not have write access to the file system.</span></span> <span data-ttu-id="7c14c-129">Betrachten Sie ein Szenario, in denen eine app als virtuelle Dienstkonto (z. B. die app-Anwendungspoolidentität des W3wp.exe) ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="7c14c-129">Consider a scenario where an app is running as a virtual service account (such as w3wp.exe's app pool identity).</span></span> <span data-ttu-id="7c14c-130">In diesen Fällen ist kann der Administrator einen Registrierungsschlüssel bereitgestellt haben, der entsprechenden ACLed für die Dienstidentität für das Konto ist.</span><span class="sxs-lookup"><span data-stu-id="7c14c-130">In these cases, the administrator may have provisioned a registry key that's appropriate ACLed for the service account identity.</span></span> <span data-ttu-id="7c14c-131">Rufen Sie die [PersistKeysToRegistry](https://github.com/aspnet/DataProtection/blob/rel/1.1.0/src/Microsoft.AspNetCore.DataProtection/DataProtectionBuilderExtensions.cs) Konfiguration Routine, wie unten dargestellt.</span><span class="sxs-lookup"><span data-stu-id="7c14c-131">Call the [PersistKeysToRegistry](https://github.com/aspnet/DataProtection/blob/rel/1.1.0/src/Microsoft.AspNetCore.DataProtection/DataProtectionBuilderExtensions.cs) configuration routine as shown below.</span></span> <span data-ttu-id="7c14c-132">Geben Sie einen `RegistryKey` verweist auf den Speicherort, in dem kryptografischen Schlüssel/Werte gespeichert werden sollen.</span><span class="sxs-lookup"><span data-stu-id="7c14c-132">Provide a `RegistryKey` pointing to the location where cryptographic keys/values should be stored.</span></span>

```csharp
   sc.AddDataProtection()
       // persist keys to a specific location in the system registry
       .PersistKeysToRegistry(Registry.CurrentUser.OpenSubKey(@"SOFTWARE\Sample\keys"));
   ```

<span data-ttu-id="7c14c-133">Wenn Sie die Registrierung des Systems als Mechanismus für die Persistenz verwenden, erwägen Sie [Windows DPAPI](xref:security/data-protection/implementation/key-encryption-at-rest#data-protection-implementation-key-encryption-at-rest) zum Verschlüsseln der Schlüssel im Ruhezustand.</span><span class="sxs-lookup"><span data-stu-id="7c14c-133">If you use the system registry as a persistence mechanism, consider using [Windows DPAPI](xref:security/data-protection/implementation/key-encryption-at-rest#data-protection-implementation-key-encryption-at-rest) to encrypt the keys at rest.</span></span>

## <a name="custom-key-repository"></a><span data-ttu-id="7c14c-134">Benutzerdefinierte Schlüssel-repository</span><span class="sxs-lookup"><span data-stu-id="7c14c-134">Custom key repository</span></span>

<span data-ttu-id="7c14c-135">Wenn die integrierten Mechanismen nicht geeignet sind, kann der Entwickler einen eigenen Mechanismus schlüsselpersistenz angeben, durch Bereitstellen eines benutzerdefinierten `IXmlRepository`.</span><span class="sxs-lookup"><span data-stu-id="7c14c-135">If the in-box mechanisms are not appropriate, the developer can specify their own key persistence mechanism by providing a custom `IXmlRepository`.</span></span>
