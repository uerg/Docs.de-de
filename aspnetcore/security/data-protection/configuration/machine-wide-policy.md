---
title: Wide Computerrichtlinie
author: rick-anderson
description: 
keywords: ASP.NET Core,
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 285ae47d-e0bf-4b03-b0a8-2b1fb18bc3a1
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/configuration/machine-wide-policy
ms.openlocfilehash: fde8f75422c9dd84311a65b21e1e38b47fbe0306
ms.sourcegitcommit: 8f4d4fad1ca27adf9e396f5c205c9875a3963664
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/13/2017
---
# <a name="machine-wide-policy"></a>Wide Computerrichtlinie

<a name="data-protection-configuration-machinewidepolicy"></a>

Bei Ausführung auf Windows verfügt über die Datenschutzsystem eingeschränkte Unterstützung für das Festlegen von computerweiten Standardrichtlinie für alle Anwendungen, die Datenschutz nutzen. Die Idee ist, dass ein Administrator einige Standardeinstellung (z. B. Lebensdauer von Algorithmen verwendet, oder Schlüssel) zu ändern, ohne jede Anwendung auf dem Computer manuell aktualisieren möchten.

>[!WARNING]
> Der Systemadministrator kann die Standardrichtlinie festlegen, aber sie können keine es erzwingen. Der Anwendungsentwickler kann einen beliebigen Wert mit einem eigenen auswählen immer außer Kraft setzen. Die Standardrichtlinie betrifft nur Anwendungen, in denen der Entwickler einen expliziten Wert für eine bestimmte Einstellung nicht angegeben wurde.

## <a name="setting-default-policy"></a>Standardrichtlinie festlegen

Um die Standardrichtlinie festlegen, kann ein Administrator bekannte Werte in der Registrierung unter folgendem Schlüssel festlegen.

Registrierungsschlüssel:`HKLM\SOFTWARE\Microsoft\DotNetPackages\Microsoft.AspNetCore.DataProtection`

Wenn Sie auf einem 64-Bit-Betriebssystem und das Verhalten von 32-Bit-Anwendungen zu beeinflussen möchten, beachten Sie außerdem das Wow6432Node äquivalent zu den oben angegebenen Schlüssels zu konfigurieren.

Die unterstützten Werte sind:

* EncryptionType [Zeichenfolge] – Gibt an, welche Algorithmen für den Datenschutz verwendet werden soll. Dieser Wert kann "CNG-CBC", "CNG-GCM" oder "Verwaltet", und wird ausführlicher beschrieben [unten](#data-protection-encryption-types).

* DefaultKeyLifetime [DWORD -] Gibt die Gültigkeitsdauer für die neu generierten Schlüssel. Dieser Wert wird in Tagen angegeben und muss ≥ 7.

* KeyEscrowSinks [Zeichenfolge] – Gibt die Typen, die zur schlüsselhinterlegung verwendet werden soll. Dieser Wert ist eine durch Semikolons getrennte Liste von schlüsselhinterlegung senken, in dem jedes Element in der Liste der Assembly qualifizierte Name eines Typs ist das IKeyEscrowSink implementiert.

<a name="data-protection-encryption-types"></a>

### <a name="encryption-types"></a>Verschlüsselungstypen

Wenn EncryptionType "CNG-CBC" ist, wird das System konfiguriert um symmetrische Blockchiffre CBC-Modus für die Vertraulichkeit und HMAC Authentizität mit Dienste von Windows CNG zu verwenden (finden Sie unter [angeben benutzerdefinierte Windows CNG-Algorithmen](overview.md#data-protection-changing-algorithms-cng)Weitere Details). Die folgenden zusätzlichen Werte werden unterstützt, von denen jeder eine Eigenschaft für die CngCbcAuthenticatedEncryptionSettings-Typ entspricht:

* "EncryptionAlgorithm" [Zeichenfolge] - der Name des Verschlüsselungsalgorithmus einen symmetrischen Block CNG verständlich. Dieser Algorithmus wird im CBC-Modus geöffnet werden.

* EncryptionAlgorithmProvider [Zeichenfolge] - der Name der CNG-Anbieter-Implementierung, die vom Algorithmus "EncryptionAlgorithm" erzeugt werden kann.

* EncryptionAlgorithmKeySize [DWORD -] die Länge (in Bits) des Schlüssels, der für der Block symmetrischen Verschlüsselungsalgorithmus abgeleitet werden.

* Hashalgorithmus [Zeichenfolge] - der Name eines Hashalgorithmus CNG verständlich. Dieser Algorithmus wird im HMAC-Modus geöffnet werden.

* HashAlgorithmProvider [Zeichenfolge] - der Name der CNG-Anbieter-Implementierung, die vom Algorithmus Hashalgorithmus erzeugt werden kann.

Wenn EncryptionType "CNG-GCM" ist, wird das System konfiguriert um symmetrische Blockchiffre Galois/Counter-Modus für die Vertraulichkeit und die Authentizität mit Dienste von Windows CNG zu verwenden (finden Sie unter [angeben benutzerdefinierte Windows CNG-Algorithmen](overview.md#data-protection-changing-algorithms-cng)Weitere Details). Die folgenden zusätzlichen Werte werden unterstützt, von denen jeder eine Eigenschaft für die CngGcmAuthenticatedEncryptionSettings-Typ entspricht:

* "EncryptionAlgorithm" [Zeichenfolge] - der Name des Verschlüsselungsalgorithmus einen symmetrischen Block CNG verständlich. Dieser Algorithmus wird in Galois/Counter-Modus geöffnet werden.

* EncryptionAlgorithmProvider [Zeichenfolge] - der Name der CNG-Anbieter-Implementierung, die vom Algorithmus "EncryptionAlgorithm" erzeugt werden kann.

* EncryptionAlgorithmKeySize [DWORD -] die Länge (in Bits) des Schlüssels, der für der Block symmetrischen Verschlüsselungsalgorithmus abgeleitet werden.

Wenn EncryptionType "verwaltet", wird das System konfiguriert werden, um eine verwaltete SymmetricAlgorithm für Vertraulichkeit und KeyedHashAlgorithm für Authenticity (Echtheitszertifikat) zu verwenden (finden Sie unter [angeben benutzerdefinierter verwaltet Algorithmen](overview.md#data-protection-changing-algorithms-custom-managed) Weitere Details). Die folgenden zusätzlichen Werte werden unterstützt, von denen jeder eine Eigenschaft für die ManagedAuthenticatedEncryptionSettings-Typ entspricht:

* EncryptionAlgorithmType [Zeichenfolge] – die Assembly qualifizierte Name eines Typs der SymmetricAlgorithm implementiert.

* EncryptionAlgorithmKeySize [DWORD -] die Länge (in Bits) des Schlüssels, der für den symmetrischen Verschlüsselungsalgorithmus abgeleitet werden.

* ValidationAlgorithmType [Zeichenfolge] – die Assembly qualifizierte Name eines Typs der KeyedHashAlgorithm implementiert.

Wenn EncryptionType beliebiger anderer Wert (außer Null / leer) verfügt, löst das Data Protection-System eine Ausnahme beim Start.

>[!WARNING]
> Wenn eine Standardeinstellung für die Richtlinie konfigurieren, die Typnamen (EncryptionAlgorithmType, ValidationAlgorithmType, KeyEscrowSinks) umfasst, müssen die Typen für die Anwendung verfügbar sein. In der Praxis bedeutet dies, dass für Anwendungen, die in Desktop-CLR ausgeführt werden, die Assemblys, die diese Typen enthalten GACed werden soll. ASP.NET Core Anwendungen auf [.NET Core](https://www.microsoft.com/net/core), die Pakete, mit denen diese Typen enthalten, die installiert werden soll.
