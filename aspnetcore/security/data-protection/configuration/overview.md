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
# <a name="configuring-data-protection"></a>Konfigurieren des Datenschutzes

<a name=data-protection-configuring></a>

Bei der Initialisierung der Datenschutzsystem gilt es einige [Standardeinstellungen](default-settings.md#data-protection-default-settings) basierend auf der betriebsumgebung. Diese Einstellungen sind im Allgemeinen gut für Anwendungen, die auf einem einzelnen Computer ausgeführt. Es gibt einige Fälle, in denen ein Entwickler möchte diese ändern (z. B. da ihre Anwendung auf mehrere Computer oder aus Compliance-Gründen verteilt wird), und für diese Szenarien die Datenschutzsystem bietet eine umfangreiche API-Konfiguration.

<a name=data-protection-configuration-callback></a>

Es ist eine Erweiterungsmethode AddDataProtection ein IDataProtectionBuilder zurück die selbst macht Erweiterungsmethoden, dass Sie miteinander Optionen verketten können so konfigurieren Sie verschiedene Datenschutz. Z. B. zum Speichern von Schlüsseln auf einer UNC-Freigabe statt %LocalAppData% (Standard), konfigurieren Sie das System wie folgt:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .PersistKeysToFileSystem(new DirectoryInfo(@"\\server\share\directory\"));
}
```

>[!WARNING]
> Wenn Sie den Speicherort der schlüsselpersistenz ändern, wird das System nicht mehr automatisch verschlüsseln Schlüssel im Ruhezustand, da er nicht weiß, ob es sich bei DPAPI handelt es sich eine entsprechende Verschlüsselungsmechanismus.

<a name=configuring-x509-certificate></a>

Sie können konfigurieren, dass das System, um Schlüssel im Ruhezustand zu schützen, durch das Aufrufen einer der ProtectKeysWith\* Konfigurations-APIs. Betrachten Sie das folgenden Beispiel wird die Tasten auf einer UNC-Freigabe gespeichert, und diese Schlüssel im Ruhezustand mit einem bestimmten x. 509-Zertifikat verschlüsselt.

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .PersistKeysToFileSystem(new DirectoryInfo(@"\\server\share\directory\"))
        .ProtectKeysWithCertificate("thumbprint");
}
```

Finden Sie unter [Verschlüsselung ruhender](../implementation/key-encryption-at-rest.md#data-protection-implementation-key-encryption-at-rest) für weitere Beispiele und detaillierte Informationen zu den integrierten Schlüsselverschlüsselung Mechanismen.

So konfigurieren Sie das System eine standardmäßige Lebensdauer von 14 Tagen anstelle 90 Tage, betrachten das folgende Beispiel:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .SetDefaultKeyLifetime(TimeSpan.FromDays(14));
}
```

Standardmäßig isoliert die Datenschutzsystem Anwendungen von einem anderen, auch wenn sie die gleiche physischen Schlüssel Repository gemeinsam nutzen. Dies verhindert, dass die Anwendungen aus der jeweils anderen geschützten Nutzlasten zu verstehen. Freigeben von geschützten Nutzlasten zwischen zwei verschiedenen konfigurieren Sie das System, übergeben in der gleichen Anwendungsname für beide Anwendungen wie in der folgenden Beispiel:

<a name=data-protection-code-sample-application-name></a>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .SetApplicationName("my application");
}
```

<a name=data-protection-configuring-disable-automatic-key-generation></a>

Schließlich können Sie ein Szenario haben, in dem Sie nicht möchten eine Anwendung die Schlüssel automatisch zurückzusetzen, wie sie Ablaufdatum nähern. Ein Beispiel hierfür ist möglicherweise Anwendungen richten Sie in eine primäre / sekundäre Beziehung, wobei nur die primäre Anwendung verantwortlich für die schlüsselverwaltung Bedenken und alle sekundären Anwendungen einfach eine schreibgeschützte Ansicht des Rings Schlüssel verfügen. Die sekundären Anwendungen können so konfiguriert werden, um den Schlüssel Ring als schreibgeschützt zu behandeln, indem Sie das System wie folgt konfigurieren:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .DisableAutomaticKeyGeneration();
}
```

<a name=data-protection-configuration-per-app-isolation></a>

## <a name="per-application-isolation"></a>Pro Anwendungsisolation

Wenn das Data Protection-System von einem ASP.NET Core-Host bereitgestellt wird, wird es automatisch von einem anderen Isolieren von Anwendungen auch, wenn diese Anwendungen die gleichen Arbeitsprozesskonto ausgeführt werden, und die gleiche Schlüsselmaterial verwenden. Dies ist zumindest ähneln den IsolateApps-Modifizierer aus System.Web <machineKey> Element.

Die Isolation-Einschränkungsmechanismus Hinblick auf jede Anwendung auf dem lokalen Computer als eindeutige Mandant, enthält daher die IDataProtector Stamm für eine bestimmte Anwendung automatisch die Anwendungs-ID als Diskriminator. Eindeutige ID der Anwendung stammen aus einer von zwei Stellen.

1. Wenn die Anwendung in IIS gehostet wird, ist der eindeutige Bezeichner der Anwendung Konfigurationspfad an. Wenn eine Anwendung in einer severfarmumgebung bereitgestellt wird, sollte dieser Wert stabil sein, vorausgesetzt, dass die IIS-Umgebungen für alle Computer in der Farm auf ähnliche Weise konfiguriert werden.

2. Wenn die Anwendung nicht in IIS gehostet wird, ist der eindeutige Bezeichner der physische Pfad der Anwendung an.

Der eindeutige Bezeichner dient zum Zurücksetzen von Kennwörtern - sowohl der jeweiligen Anwendung und der Computer zu überstehen.

Dieser Mechanismus für die Sicherheitsisolation wird davon ausgegangen, dass die Anwendungen nicht bösartig sind. Eine böswillige Anwendung beeinträchtigt immer eine andere Anwendung, die unter der gleichen Arbeitsprozesskonto ausgeführt wird. In einer freigegebenen Hostingumgebung, in denen Anwendungen gegenseitig nicht vertrauenswürdig sind, sollte der Hostinganbieter um sicherzustellen, dass auf Betriebssystemebene Isolation zwischen Anwendungen, einschließlich der Anwendung zugrunde liegenden Key Repositorys trennen Schritten.

Wenn die Datenschutzsystem nicht von einem ASP.NET Core-Host bereitgestellt wird, (z. B. wenn der Entwickler selbst über die DataProtectionProvider konkreten Typ instanziiert), das Isolieren von Webanwendungen standardmäßig deaktiviert ist, und alle Anwendungen, die durch die gleiche Erfassung gesichert Material kann Nutzlasten freigeben, solange sie die entsprechenden Zwecke bereitstellen. Um das Isolieren von Webanwendungen in dieser Umgebung bereitstellen möchten, rufen Sie die SetApplicationName-Methode auf das Konfigurationsobjekt, finden Sie unter der [Codebeispiel](#data-protection-code-sample-application-name) oben.

<a name=data-protection-changing-algorithms></a>

## <a name="changing-algorithms"></a>Ändern von Algorithmen

Der Data Protection Stapel lässt den Wechsel der durch den neu generierten Schlüssel verwendete Standardalgorithmus. Die einfachste Möglichkeit hierzu ist UseCryptographicAlgorithms vom Rückruf Konfiguration aufrufen, wie in der folgenden Beispiel.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```csharp
services.AddDataProtection()
    .UseCryptographicAlgorithms(new AuthenticatedEncryptorConfiguration()
    {
        EncryptionAlgorithm = EncryptionAlgorithm.AES_256_CBC,
        ValidationAlgorithm = ValidationAlgorithm.HMACSHA256
    });
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

```csharp
services.AddDataProtection()
    .UseCryptographicAlgorithms(new AuthenticatedEncryptionSettings()
    {
        EncryptionAlgorithm = EncryptionAlgorithm.AES_256_CBC,
        ValidationAlgorithm = ValidationAlgorithm.HMACSHA256
    });
```

---

Die Standardeinstellung "EncryptionAlgorithm" und ValidationAlgorithm lauten AES-256-CBC bzw. HMACSHA256. Die Standardrichtlinie kann festgelegt werden, von einem Systemadministrator über [Wide Computerrichtlinie](machine-wide-policy.md), aber ein expliziter Aufruf von UseCryptographicAlgorithms wird die Standardrichtlinie überschreiben.

UseCryptographicAlgorithms aufgerufen wird, dass der Entwickler zum Angeben des gewünschten Algorithmus (aus einer vordefinierten integrierten Liste), und der Entwickler muss sich nicht um die Implementierung des Algorithmus kümmern. Z. B. im Szenario oben die Datenschutzsystem wird versucht, die CNG-Implementierung von AES verwenden, wenn unter Windows ausgeführt wird, andernfalls es wird ein Fallback auf den verwalteten System.Security.Cryptography.Aes-Klasse.

Der Entwickler kann eine Implementierung manuell festlegen, bei Bedarf über einen Aufruf an UseCustomCryptographicAlgorithms, wie in den folgenden Beispielen.

>[!TIP]
> Ändern die Algorithmen wirkt sich nicht auf den vorhandenen Schlüssel im Schlüssel Ring aus. Er wirkt sich nur auf neu generierten Schlüssel.

<a name=data-protection-changing-algorithms-custom-managed></a>

### <a name="specifying-custom-managed-algorithms"></a>Angeben von benutzerdefinierten verwalteten Algorithmen

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Um benutzerdefinierte Algorithmen, die verwaltete anzugeben, erstellen Sie eine ManagedAuthenticatedEncryptorConfiguration-Instanz, die auf die Implementierungstypen verweist.

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

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Um benutzerdefinierte Algorithmen, die verwaltete anzugeben, erstellen Sie eine ManagedAuthenticatedEncryptionSettings-Instanz, die auf die Implementierungstypen verweist.

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

Im Allgemeinen die \*Typeigenschaften müssen auf konkrete, zeigen instanziierbaren (über einen öffentlichen parameterlosen Ctor) Implementierungen von SymmetricAlgorithm und KeyedHashAlgorithm, obwohl das System spezielle-Fällen einige Werte wie typeof(Aes) der Einfachheit halber .

> [!NOTE]
> Die SymmetricAlgorithm muss eine Schlüssellänge von ≥ 128 Bits und eine Blockgröße von ≥ 64 Bits haben, und es muss CBC-Modus-Verschlüsselung mit PKCS #7-Auffüllung unterstützen. Der KeyedHashAlgorithm benötigen Nachrichtenhash Größe > = 128 Bits und müssen Schlüssel, der den Hashalgorithmus Digestlänge gleich Länge unterstützt. Die KeyedHashAlgorithm ist nicht zwingend erforderlich, HMAC sein.

<a name=data-protection-changing-algorithms-cng></a>

### <a name="specifying-custom-windows-cng-algorithms"></a>Angeben von benutzerdefinierten Windows CNG-Algorithmen

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Erstellen Sie zum Angeben eines benutzerdefinierten Windows CNG-Algorithmus mit Verschlüsselung im CBC-Modus + HMAC-Überprüfung einer CngCbcAuthenticatedEncryptorConfiguration-Instanz, die die algorithmische Informationen enthält.

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

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Erstellen Sie zum Angeben eines benutzerdefinierten Windows CNG-Algorithmus mit Verschlüsselung im CBC-Modus + HMAC-Überprüfung einer CngCbcAuthenticatedEncryptionSettings-Instanz, die die algorithmische Informationen enthält.

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
> Der Block symmetrischen Verschlüsselungsalgorithmus muss eine Schlüssellänge von ≥ 128 Bits und eine Blockgröße von ≥ 64 Bits haben, und es muss CBC-Modus-Verschlüsselung mit PKCS #7-Auffüllung unterstützen. Der Hashalgorithmus benötigen Nachrichtenhash Größe von > = 128 Bits und müssen unterstützen, die mit dem Flag "BCRYPT_ALG_HANDLE_HMAC_FLAG" geöffnet wird. Die \*Anbietereigenschaften können werden auf null festgelegt, um den Standardanbieter für den angegebenen Algorithmus zu verwenden. Finden Sie unter der [BCryptOpenAlgorithmProvider](https://msdn.microsoft.com/library/windows/desktop/aa375479(v=vs.85).aspx) Dokumentation weitere Informationen.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Erstellen Sie zum Angeben eines benutzerdefinierten Windows CNG-Algorithmus mit Verschlüsselung Galois/Leistungsindikatoren im + Überprüfung eine CngGcmAuthenticatedEncryptorConfiguration-Instanz, die die algorithmische Informationen enthält.

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

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Erstellen Sie zum Angeben eines benutzerdefinierten Windows CNG-Algorithmus mit Verschlüsselung Galois/Leistungsindikatoren im + Überprüfung eine CngGcmAuthenticatedEncryptionSettings-Instanz, die die algorithmische Informationen enthält.

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
> Der Block symmetrischen Verschlüsselungsalgorithmus benötigen eine Schlüssellänge von ≥ 128 Bits und eine Blockgröße von genau 128 Bits und müssen GCM-Verschlüsselung unterstützt. Die EncryptionAlgorithmProvider-Eigenschaft kann festgelegt werden, auf null fest, um den Standardanbieter für den angegebenen Algorithmus zu verwenden. Finden Sie unter der [BCryptOpenAlgorithmProvider](https://msdn.microsoft.com/library/windows/desktop/aa375479(v=vs.85).aspx) Dokumentation weitere Informationen.

### <a name="specifying-other-custom-algorithms"></a>Andere benutzerdefinierten Algorithmen angeben

Obwohl nicht als eine erstklassige-API verfügbar gemacht, ist die Datenschutzsystem extensible genug, um zu ermöglichen, nahezu jede Art von Algorithmus angibt. Beispielsweise ist es möglich, alle Schlüssel in einem HSM enthaltenen zu behalten und eine benutzerdefinierte Implementierung des Kerns Ver- und Entschlüsselung Routinen bereitstellen. Finden Sie unter IAuthenticatedEncryptorConfiguration im Kern Kryptografie Erweiterbarkeit Abschnitt, um weitere Informationen.

### <a name="see-also"></a>Siehe auch

* [Szenarios, die die Abhängigkeitsinjektion nicht beachten](non-di-scenarios.md)
* [Computerübergreifende Richtlinie](machine-wide-policy.md)
