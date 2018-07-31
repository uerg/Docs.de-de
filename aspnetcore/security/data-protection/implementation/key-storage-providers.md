---
title: Softwareschlüsselspeicher-Anbieter in ASP.NET Core
author: rick-anderson
description: Informationen Sie zu den softwareschlüsselspeicher-Anbieter in ASP.NET Core und wichtige Speicherorte zu konfigurieren.
ms.author: riande
ms.date: 07/16/2018
uid: security/data-protection/implementation/key-storage-providers
ms.openlocfilehash: e712ff09b5306bc4481c4cc105448d7cbfa39f3a
ms.sourcegitcommit: d99a8554c91f626cf5e466911cf504dcbff0e02e
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/31/2018
ms.locfileid: "39356765"
---
# <a name="key-storage-providers-in-aspnet-core"></a>Softwareschlüsselspeicher-Anbieter in ASP.NET Core

Das System zum Schutz von Daten [Discovery-Mechanismus in der Standardeinstellung setzt](xref:security/data-protection/configuration/default-settings) um zu bestimmen, in denen kryptografische Schlüssel beibehalten werden soll. Der Entwickler kann außer Kraft setzen als standardmäßiger Ermittlungsmechanismus verwendet und geben Sie den Speicherort manuell.

> [!WARNING]
> Wenn Sie einem Standort der explizite schlüsselpersistenz angeben, hebt die System zum Schutz von Daten, sodass Schlüssel nicht mehr im Ruhezustand verschlüsselt sind die wichtigsten standardverschlüsselung auf Rest-Mechanismus, Registrierung. Es wird empfohlen, die Sie darüber hinaus [Geben Sie einen Mechanismus für die explizite Schlüsselverschlüsselung](xref:security/data-protection/implementation/key-encryption-at-rest) für produktionsbereitstellungen.

## <a name="file-system"></a>Dateisystem

Um eine Datei systembasierte Key Repository zu konfigurieren, rufen Sie die [PersistKeysToFileSystem](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystofilesystem) Configuration-Routine, wie unten dargestellt. Geben Sie einen [DirectoryInfo](/dotnet/api/system.io.directoryinfo) auf das Repository, in dem Schlüssel gespeichert werden soll:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .PersistKeysToFileSystem(new DirectoryInfo(@"c:\temp-keys\"));
}
```

Die `DirectoryInfo` kann in ein Verzeichnis auf dem lokalen Computer, oder es kann auf einen Ordner auf einer Netzwerkfreigabe verweisen. Wenn ein Verzeichnis auf dem lokalen Computer verweist (und das Szenario ist, dass nur die apps auf dem lokalen Computer Zugriff auf dieses Repository verwenden müssen) in Betracht [Windows DPAPI](xref:security/data-protection/implementation/key-encryption-at-rest) (unter Windows), die Schlüssel im Ruhezustand verschlüsselt. Erwägen Sie andernfalls die Verwendung einer [x. 509-Zertifikat](xref:security/data-protection/implementation/key-encryption-at-rest) Schlüsseln im ruhenden Zustand verschlüsselt.

## <a name="azure-and-redis"></a>Azure und Redis

Die [Microsoft.AspNetCore.DataProtection.AzureStorage](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.AzureStorage/) und [Microsoft.AspNetCore.DataProtection.Redis](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.Redis/) Pakete ermöglichen das Speichern von Data Protection-Schlüssel in Azure Storage oder einem Redis-Cache. Schlüssel können auf mehrere Instanzen einer Web-App gemeinsam genutzt werden. Apps können Authentifizierungscookies oder CSRF-Schutz über mehrere Server hinweg freigeben. Zum Konfigurieren des Azure Blob Storage-Anbieters rufen Sie eine der der [PersistKeysToAzureBlobStorage](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.persistkeystoazureblobstorage) Überladungen:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .PersistKeysToAzureBlobStorage(new Uri("<blob URI including SAS token>"));
}
```

Um auf Redis konfigurieren, rufen Sie eine der der [PersistKeysToRedis](/dotnet/api/microsoft.aspnetcore.dataprotection.redisdataprotectionbuilderextensions.persistkeystoredis) Überladungen:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    var redis = ConnectionMultiplexer.Connect("<URI>");
    services.AddDataProtection()
        .PersistKeysToRedis(redis, "DataProtection-Keys");
}
```

Weitere Informationen finden Sie unter den folgenden Themen:

* ["Stackexchange.redis" ConnectionMultiplexer](https://github.com/StackExchange/StackExchange.Redis/blob/master/docs/Basics.md)
* [Azure Redis Cache](/azure/redis-cache/cache-dotnet-how-to-use-azure-redis-cache#connect-to-the-cache)
* [ASPNET/DataProtection-Beispiele](https://github.com/aspnet/DataProtection/tree/master/samples)

## <a name="registry"></a>Registrierung

**Gilt nur für Windows-Bereitstellungen.**

Die app möglicherweise manchmal nicht über Schreibzugriff auf das Dateisystem. Stellen Sie sich ein Szenario, in denen eine app als ein virtuelles Dienstkonto ausgeführt wird (z. B. *w3wp.exe*des app-Pool-Identität). In diesen Fällen kann der Administrator einen Registrierungsschlüssel bereitstellen, der von der dienstkontoidentität zugänglich ist. Rufen Sie die [PersistKeysToRegistry](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystoregistry) Erweiterungsmethode wie unten dargestellt. Geben Sie einen [RegistryKey](/dotnet/api/microsoft.aspnetcore.dataprotection.repositories.registryxmlrepository.registrykey) verweist auf den Speicherort, in der kryptografischen Schlüssel gespeichert werden soll:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .PersistKeysToRegistry(Registry.CurrentUser.OpenSubKey(@"SOFTWARE\Sample\keys"));
}
```

> [!IMPORTANT]
> Es wird empfohlen, [Windows DPAPI](xref:security/data-protection/implementation/key-encryption-at-rest) den Schlüsseln im ruhenden Zustand verschlüsselt.

## <a name="custom-key-repository"></a>Benutzerdefinierte Schlüssel-repository

Wenn die integrierte Mechanismen nicht geeignet sind, kann Entwickler ihre eigenen Schlüssel persistenzmechanismus angeben, durch Bereitstellen eines benutzerdefinierten [IXmlRepository](/dotnet/api/microsoft.aspnetcore.dataprotection.repositories.ixmlrepository).
