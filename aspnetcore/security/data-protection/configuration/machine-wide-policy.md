---
title: Data Protection computerweite Supportrichtlinie in ASP.NET Core
author: rick-anderson
description: "Informationen Sie zur Unterstützung für das Festlegen von computerweiten Standardrichtlinie für alle apps, die ASP.NET Core-Datenschutz nutzen."
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/configuration/machine-wide-policy
ms.openlocfilehash: 4c3ae3b628ebe17c7926c71f1fad664d719d1706
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/19/2018
---
# <a name="data-protection-machine-wide-policy-support-in-aspnet-core"></a>Data Protection computerweite Supportrichtlinie in ASP.NET Core

Von [Rick Anderson](https://twitter.com/RickAndMSFT)

Bei Ausführung auf Windows verfügt das System den Datenschutz über eingeschränkte Unterstützung für das Festlegen von computerweiten Standardrichtlinie für alle apps, die ASP.NET Core-Datenschutz nutzen. Die Idee ist, dass ein Administrator ändern möchten, kann einen Standardwert festlegen, z. B. die Algorithmen verwendet oder Schlüsselgültigkeitsdauer, ohne dass Sie jede app auf dem Computer manuell aktualisieren müssen.

> [!WARNING]
> Der Systemadministrator kann die Standardrichtlinie festlegen, aber sie können keine es erzwingen. App-Entwickler kann einen beliebigen Wert mit einem eigenen auswählen immer außer Kraft setzen. Die Standardrichtlinie wirkt sich nur auf apps, in denen der Entwickler einen expliziten Wert für eine Einstellung angegeben wurde nicht, aus.

## <a name="setting-default-policy"></a>Standardrichtlinie festlegen

Um die Standardrichtlinie festlegen, kann ein Administrator bekannte Werte in der Registrierung unter dem folgenden Registrierungsschlüssel festlegen:

**HKLM\SOFTWARE\Microsoft\DotNetPackages\Microsoft.AspNetCore.DataProtection**

Wenn Sie auf einem 64-Bit-Betriebssystem und beeinflussen das Verhalten von 32-Bit-apps, denken Sie daran, das Äquivalent Wow6432Node des oben angegebenen Schlüssels konfigurieren möchten.

Die unterstützten Werte sind unten dargestellt.

| Wert              | Typ   | Beschreibung |
| ------------------ | :----: | ----------- |
| EncryptionType     | Zeichenfolge | Gibt an, welche Algorithmen für den Datenschutz verwendet werden soll. Der Wert kann CNG-CBC, CNG-GCM oder verwaltet und wird im folgenden ausführlicher beschrieben. |
| DefaultKeyLifetime | DWORD  | Gibt die Gültigkeitsdauer für die neu generierten Schlüssel an. Der Wert wird in Tagen angegeben und muss > = 7. |
| KeyEscrowSinks     | Zeichenfolge | Gibt die Typen, die zur schlüsselhinterlegung verwendet werden. Der Wert ist eine durch Semikolons getrennte Liste von schlüsselhinterlegung senken, wobei jedes Element in der Liste ist die Assembly qualifizierte Name eines Typs, der implementiert [IKeyEscrowSink](/dotnet/api/microsoft.aspnetcore.dataprotection.keymanagement.ikeyescrowsink). |

## <a name="encryption-types"></a>Verschlüsselungstypen

Wenn EncryptionType CNG-CBC ist, das System für die Verwendung konfiguriert symmetrische Blockchiffre CBC-Modus für die Vertraulichkeit und HMAC Authentizität mit Windows CNG-Diensten (finden Sie unter [angeben benutzerdefinierte Windows CNG-Algorithmen](xref:security/data-protection/configuration/overview#specifying-custom-windows-cng-algorithms) für Weitere Informationen). Die folgenden zusätzlichen Werte werden unterstützt, von denen jeder eine Eigenschaft für die CngCbcAuthenticatedEncryptionSettings-Typ entspricht.

| Wert                       | Typ   | Beschreibung |
| --------------------------- | :----: | ----------- |
| EncryptionAlgorithm         | Zeichenfolge | Der Name des Verschlüsselungsalgorithmus einen symmetrischen Block CNG verständlich. Dieser Algorithmus wird im CBC-Modus geöffnet. |
| EncryptionAlgorithmProvider | Zeichenfolge | Der Name der CNG-Anbieter-Implementierung, die vom Algorithmus "EncryptionAlgorithm" erzeugt werden kann. |
| EncryptionAlgorithmKeySize  | DWORD  | Die Länge (in Bits) des Schlüssels, der für der Block symmetrischen Verschlüsselungsalgorithmus abgeleitet werden. |
| HashAlgorithm               | Zeichenfolge | Der Name eines Hashalgorithmus CNG verständlich. Dieser Algorithmus wird im HMAC-Modus geöffnet. |
| HashAlgorithmProvider       | Zeichenfolge | Der Name der CNG-Anbieter-Implementierung, die vom Algorithmus Hashalgorithmus erzeugt werden können. |

Wenn EncryptionType CNG-GCM ist, das System für die Verwendung konfiguriert symmetrische Blockchiffre Galois/Counter-Modus für die Vertraulichkeit und die Authentizität mit Windows CNG-Diensten (siehe [angeben benutzerdefinierte Windows CNG-Algorithmen](xref:security/data-protection/configuration/overview#specifying-custom-windows-cng-algorithms) Weitere Informationen). Die folgenden zusätzlichen Werte werden unterstützt, von denen jeder eine Eigenschaft für die CngGcmAuthenticatedEncryptionSettings-Typ entspricht.

| Wert                       | Typ   | Beschreibung |
| --------------------------- | :----: | ----------- |
| EncryptionAlgorithm         | Zeichenfolge | Der Name des Verschlüsselungsalgorithmus einen symmetrischen Block CNG verständlich. Dieser Algorithmus wird im Galois/Leistungsindikator-Modus geöffnet. |
| EncryptionAlgorithmProvider | Zeichenfolge | Der Name der CNG-Anbieter-Implementierung, die vom Algorithmus "EncryptionAlgorithm" erzeugt werden kann. |
| EncryptionAlgorithmKeySize  | DWORD  | Die Länge (in Bits) des Schlüssels, der für der Block symmetrischen Verschlüsselungsalgorithmus abgeleitet werden. |

Wenn EncryptionType verwaltet wird, wird das System konfiguriert, um eine verwaltete SymmetricAlgorithm für Vertraulichkeit und KeyedHashAlgorithm Authentizität verwenden (finden Sie unter [angeben benutzerdefinierter verwaltet Algorithmen](xref:security/data-protection/configuration/overview#specifying-custom-managed-algorithms) Weitere Details). Die folgenden zusätzlichen Werte werden unterstützt, von denen jeder eine Eigenschaft für die ManagedAuthenticatedEncryptionSettings-Typ entspricht.

| Wert                      | Typ   | Beschreibung |
| -------------------------- | :----: | ----------- |
| EncryptionAlgorithmType    | Zeichenfolge | Die Assembly qualifizierte Name eines Typs, der SymmetricAlgorithm implementiert. |
| EncryptionAlgorithmKeySize | DWORD  | Die Länge (in Bits) des Schlüssels, der für den symmetrischen Verschlüsselungsalgorithmus abgeleitet werden. |
| ValidationAlgorithmType    | Zeichenfolge | Die Assembly qualifizierte Name eines Typs, der KeyedHashAlgorithm implementiert. |

Wenn EncryptionType beliebiger anderer Wert als Null oder leer ist, löst die Datenschutz-System eine Ausnahme beim Start.

> [!WARNING]
> Wenn eine Standardeinstellung für die Richtlinie konfigurieren, die Typnamen (EncryptionAlgorithmType, ValidationAlgorithmType, KeyEscrowSinks) umfasst, müssen die Typen der App verfügbar sein. Dies bedeutet, dass für apps, die auf dem Desktop-CLR ausgeführt wird, die Assemblys, die diese Typen enthalten, die im globalen Assemblycache (GAC) vorhanden sein sollte. Für ASP.NET Core-apps unter [.NET Core](https://www.microsoft.com/net/core), die Pakete, die diese Typen enthalten, die installiert werden soll.
