---
title: Wide Computerrichtlinie
author: rick-anderson
description: 
keywords: ASP.NET Core
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 285ae47d-e0bf-4b03-b0a8-2b1fb18bc3a1
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/configuration/machine-wide-policy
ms.openlocfilehash: 7ada940acfbb7fb0887fd7c0cd722bf62f211248
ms.sourcegitcommit: 9cdbfd0d670d70b9c354216aabee260c52dad5ee
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/12/2017
---
# <a name="machine-wide-policy"></a><span data-ttu-id="3d12e-103">Wide Computerrichtlinie</span><span class="sxs-lookup"><span data-stu-id="3d12e-103">Machine Wide Policy</span></span>

<a name=data-protection-configuration-machinewidepolicy></a>

<span data-ttu-id="3d12e-104">Bei Ausführung auf Windows verfügt über die Datenschutzsystem eingeschränkte Unterstützung für das Festlegen von computerweiten Standardrichtlinie für alle Anwendungen, die Datenschutz nutzen.</span><span class="sxs-lookup"><span data-stu-id="3d12e-104">When running on Windows, the data protection system has limited support for setting default machine-wide policy for all applications which consume data protection.</span></span> <span data-ttu-id="3d12e-105">Die Idee ist, dass ein Administrator einige Standardeinstellung (z. B. Lebensdauer von Algorithmen verwendet, oder Schlüssel) zu ändern, ohne jede Anwendung auf dem Computer manuell aktualisieren möchten.</span><span class="sxs-lookup"><span data-stu-id="3d12e-105">The general idea is that an administrator might wish to change some default setting (such as algorithms used or key lifetime) without needing to manually update every application on the machine.</span></span>

>[!WARNING]
> <span data-ttu-id="3d12e-106">Der Systemadministrator kann die Standardrichtlinie festlegen, aber sie können keine es erzwingen.</span><span class="sxs-lookup"><span data-stu-id="3d12e-106">The system administrator can set default policy, but they cannot enforce it.</span></span> <span data-ttu-id="3d12e-107">Der Anwendungsentwickler kann einen beliebigen Wert mit einem eigenen auswählen immer außer Kraft setzen.</span><span class="sxs-lookup"><span data-stu-id="3d12e-107">The application developer can always override any value with one of their own choosing.</span></span> <span data-ttu-id="3d12e-108">Die Standardrichtlinie betrifft nur Anwendungen, in denen der Entwickler einen expliziten Wert für eine bestimmte Einstellung nicht angegeben wurde.</span><span class="sxs-lookup"><span data-stu-id="3d12e-108">The default policy only affects applications where the developer has not specified an explicit value for some particular setting.</span></span>

## <a name="setting-default-policy"></a><span data-ttu-id="3d12e-109">Standardrichtlinie festlegen</span><span class="sxs-lookup"><span data-stu-id="3d12e-109">Setting default policy</span></span>

<span data-ttu-id="3d12e-110">Um die Standardrichtlinie festlegen, kann ein Administrator bekannte Werte in der Registrierung unter folgendem Schlüssel festlegen.</span><span class="sxs-lookup"><span data-stu-id="3d12e-110">To set default policy, an administrator can set known values in the system registry under the following key.</span></span>

<span data-ttu-id="3d12e-111">Registrierungsschlüssel:`HKLM\SOFTWARE\Microsoft\DotNetPackages\Microsoft.AspNetCore.DataProtection`</span><span class="sxs-lookup"><span data-stu-id="3d12e-111">Reg key: `HKLM\SOFTWARE\Microsoft\DotNetPackages\Microsoft.AspNetCore.DataProtection`</span></span>

<span data-ttu-id="3d12e-112">Wenn Sie auf einem 64-Bit-Betriebssystem und das Verhalten von 32-Bit-Anwendungen zu beeinflussen möchten, beachten Sie außerdem das Wow6432Node äquivalent zu den oben angegebenen Schlüssels zu konfigurieren.</span><span class="sxs-lookup"><span data-stu-id="3d12e-112">If you're on a 64-bit operating system and want to affect the behavior of 32-bit applications, remember also to configure the Wow6432Node equivalent of the above key.</span></span>

<span data-ttu-id="3d12e-113">Die unterstützten Werte sind:</span><span class="sxs-lookup"><span data-stu-id="3d12e-113">The supported values are:</span></span>

* <span data-ttu-id="3d12e-114">EncryptionType [Zeichenfolge] – Gibt an, welche Algorithmen für den Datenschutz verwendet werden soll.</span><span class="sxs-lookup"><span data-stu-id="3d12e-114">EncryptionType [string] - specifies which algorithms should be used for data protection.</span></span> <span data-ttu-id="3d12e-115">Dieser Wert kann "CNG-CBC", "CNG-GCM" oder "Verwaltet", und wird ausführlicher beschrieben [unten](#data-protection-encryption-types).</span><span class="sxs-lookup"><span data-stu-id="3d12e-115">This value must be "CNG-CBC", "CNG-GCM", or "Managed" and is described in more detail [below](#data-protection-encryption-types).</span></span>

* <span data-ttu-id="3d12e-116">DefaultKeyLifetime [DWORD -] Gibt die Gültigkeitsdauer für die neu generierten Schlüssel.</span><span class="sxs-lookup"><span data-stu-id="3d12e-116">DefaultKeyLifetime [DWORD] - specifies the lifetime for newly-generated keys.</span></span> <span data-ttu-id="3d12e-117">Dieser Wert wird in Tagen angegeben und muss ≥ 7.</span><span class="sxs-lookup"><span data-stu-id="3d12e-117">This value is specified in days and must be ≥ 7.</span></span>

* <span data-ttu-id="3d12e-118">KeyEscrowSinks [Zeichenfolge] – Gibt die Typen, die zur schlüsselhinterlegung verwendet werden soll.</span><span class="sxs-lookup"><span data-stu-id="3d12e-118">KeyEscrowSinks [string] - specifies the types which will be used for key escrow.</span></span> <span data-ttu-id="3d12e-119">Dieser Wert ist eine durch Semikolons getrennte Liste von schlüsselhinterlegung senken, in dem jedes Element in der Liste der Assembly qualifizierte Name eines Typs ist das IKeyEscrowSink implementiert.</span><span class="sxs-lookup"><span data-stu-id="3d12e-119">This value is a semicolon-delimited list of key escrow sinks, where each element in the list is the assembly qualified name of a type which implements IKeyEscrowSink.</span></span>

<a name=data-protection-encryption-types></a>

### <a name="encryption-types"></a><span data-ttu-id="3d12e-120">Verschlüsselungstypen</span><span class="sxs-lookup"><span data-stu-id="3d12e-120">Encryption types</span></span>

<span data-ttu-id="3d12e-121">Wenn EncryptionType "CNG-CBC" ist, wird das System konfiguriert um symmetrische Blockchiffre CBC-Modus für die Vertraulichkeit und HMAC Authentizität mit Dienste von Windows CNG zu verwenden (finden Sie unter [angeben benutzerdefinierte Windows CNG-Algorithmen](overview.md#data-protection-changing-algorithms-cng)Weitere Details).</span><span class="sxs-lookup"><span data-stu-id="3d12e-121">If EncryptionType is "CNG-CBC", the system will be configured to use a CBC-mode symmetric block cipher for confidentiality and HMAC for authenticity with services provided by Windows CNG (see [Specifying custom Windows CNG algorithms](overview.md#data-protection-changing-algorithms-cng) for more details).</span></span> <span data-ttu-id="3d12e-122">Die folgenden zusätzlichen Werte werden unterstützt, von denen jeder eine Eigenschaft für die CngCbcAuthenticatedEncryptionSettings-Typ entspricht:</span><span class="sxs-lookup"><span data-stu-id="3d12e-122">The following additional values are supported, each of which corresponds to a property on the CngCbcAuthenticatedEncryptionSettings type:</span></span>

* <span data-ttu-id="3d12e-123">"EncryptionAlgorithm" [Zeichenfolge] - der Name des Verschlüsselungsalgorithmus einen symmetrischen Block CNG verständlich.</span><span class="sxs-lookup"><span data-stu-id="3d12e-123">EncryptionAlgorithm [string] - the name of a symmetric block cipher algorithm understood by CNG.</span></span> <span data-ttu-id="3d12e-124">Dieser Algorithmus wird im CBC-Modus geöffnet werden.</span><span class="sxs-lookup"><span data-stu-id="3d12e-124">This algorithm will be opened in CBC mode.</span></span>

* <span data-ttu-id="3d12e-125">EncryptionAlgorithmProvider [Zeichenfolge] - der Name der CNG-Anbieter-Implementierung, die vom Algorithmus "EncryptionAlgorithm" erzeugt werden kann.</span><span class="sxs-lookup"><span data-stu-id="3d12e-125">EncryptionAlgorithmProvider [string] - the name of the CNG provider implementation which can produce the algorithm EncryptionAlgorithm.</span></span>

* <span data-ttu-id="3d12e-126">EncryptionAlgorithmKeySize [DWORD -] die Länge (in Bits) des Schlüssels, der für der Block symmetrischen Verschlüsselungsalgorithmus abgeleitet werden.</span><span class="sxs-lookup"><span data-stu-id="3d12e-126">EncryptionAlgorithmKeySize [DWORD] - the length (in bits) of the key to derive for the symmetric block cipher algorithm.</span></span>

* <span data-ttu-id="3d12e-127">Hashalgorithmus [Zeichenfolge] - der Name eines Hashalgorithmus CNG verständlich.</span><span class="sxs-lookup"><span data-stu-id="3d12e-127">HashAlgorithm [string] - the name of a hash algorithm understood by CNG.</span></span> <span data-ttu-id="3d12e-128">Dieser Algorithmus wird im HMAC-Modus geöffnet werden.</span><span class="sxs-lookup"><span data-stu-id="3d12e-128">This algorithm will be opened in HMAC mode.</span></span>

* <span data-ttu-id="3d12e-129">HashAlgorithmProvider [Zeichenfolge] - der Name der CNG-Anbieter-Implementierung, die vom Algorithmus Hashalgorithmus erzeugt werden kann.</span><span class="sxs-lookup"><span data-stu-id="3d12e-129">HashAlgorithmProvider [string] - the name of the CNG provider implementation which can produce the algorithm HashAlgorithm.</span></span>

<span data-ttu-id="3d12e-130">Wenn EncryptionType "CNG-GCM" ist, wird das System konfiguriert um symmetrische Blockchiffre Galois/Counter-Modus für die Vertraulichkeit und die Authentizität mit Dienste von Windows CNG zu verwenden (finden Sie unter [angeben benutzerdefinierte Windows CNG-Algorithmen](overview.md#data-protection-changing-algorithms-cng)Weitere Details).</span><span class="sxs-lookup"><span data-stu-id="3d12e-130">If EncryptionType is "CNG-GCM", the system will be configured to use a Galois/Counter Mode symmetric block cipher for confidentiality and authenticity with services provided by Windows CNG (see [Specifying custom Windows CNG algorithms](overview.md#data-protection-changing-algorithms-cng) for more details).</span></span> <span data-ttu-id="3d12e-131">Die folgenden zusätzlichen Werte werden unterstützt, von denen jeder eine Eigenschaft für die CngGcmAuthenticatedEncryptionSettings-Typ entspricht:</span><span class="sxs-lookup"><span data-stu-id="3d12e-131">The following additional values are supported, each of which corresponds to a property on the CngGcmAuthenticatedEncryptionSettings type:</span></span>

* <span data-ttu-id="3d12e-132">"EncryptionAlgorithm" [Zeichenfolge] - der Name des Verschlüsselungsalgorithmus einen symmetrischen Block CNG verständlich.</span><span class="sxs-lookup"><span data-stu-id="3d12e-132">EncryptionAlgorithm [string] - the name of a symmetric block cipher algorithm understood by CNG.</span></span> <span data-ttu-id="3d12e-133">Dieser Algorithmus wird in Galois/Counter-Modus geöffnet werden.</span><span class="sxs-lookup"><span data-stu-id="3d12e-133">This algorithm will be opened in Galois/Counter Mode.</span></span>

* <span data-ttu-id="3d12e-134">EncryptionAlgorithmProvider [Zeichenfolge] - der Name der CNG-Anbieter-Implementierung, die vom Algorithmus "EncryptionAlgorithm" erzeugt werden kann.</span><span class="sxs-lookup"><span data-stu-id="3d12e-134">EncryptionAlgorithmProvider [string] - the name of the CNG provider implementation which can produce the algorithm EncryptionAlgorithm.</span></span>

* <span data-ttu-id="3d12e-135">EncryptionAlgorithmKeySize [DWORD -] die Länge (in Bits) des Schlüssels, der für der Block symmetrischen Verschlüsselungsalgorithmus abgeleitet werden.</span><span class="sxs-lookup"><span data-stu-id="3d12e-135">EncryptionAlgorithmKeySize [DWORD] - the length (in bits) of the key to derive for the symmetric block cipher algorithm.</span></span>

<span data-ttu-id="3d12e-136">Wenn EncryptionType "verwaltet", wird das System konfiguriert werden, um eine verwaltete SymmetricAlgorithm für Vertraulichkeit und KeyedHashAlgorithm für Authenticity (Echtheitszertifikat) zu verwenden (finden Sie unter [angeben benutzerdefinierter verwaltet Algorithmen](overview.md#data-protection-changing-algorithms-custom-managed) Weitere Details).</span><span class="sxs-lookup"><span data-stu-id="3d12e-136">If EncryptionType is "Managed", the system will be configured to use a managed SymmetricAlgorithm for confidentiality and KeyedHashAlgorithm for authenticity (see [Specifying custom managed algorithms](overview.md#data-protection-changing-algorithms-custom-managed) for more details).</span></span> <span data-ttu-id="3d12e-137">Die folgenden zusätzlichen Werte werden unterstützt, von denen jeder eine Eigenschaft für die ManagedAuthenticatedEncryptionSettings-Typ entspricht:</span><span class="sxs-lookup"><span data-stu-id="3d12e-137">The following additional values are supported, each of which corresponds to a property on the ManagedAuthenticatedEncryptionSettings type:</span></span>

* <span data-ttu-id="3d12e-138">EncryptionAlgorithmType [Zeichenfolge] – die Assembly qualifizierte Name eines Typs der SymmetricAlgorithm implementiert.</span><span class="sxs-lookup"><span data-stu-id="3d12e-138">EncryptionAlgorithmType [string] - the assembly-qualified name of a type which implements SymmetricAlgorithm.</span></span>

* <span data-ttu-id="3d12e-139">EncryptionAlgorithmKeySize [DWORD -] die Länge (in Bits) des Schlüssels, der für den symmetrischen Verschlüsselungsalgorithmus abgeleitet werden.</span><span class="sxs-lookup"><span data-stu-id="3d12e-139">EncryptionAlgorithmKeySize [DWORD] - the length (in bits) of the key to derive for the symmetric encryption algorithm.</span></span>

* <span data-ttu-id="3d12e-140">ValidationAlgorithmType [Zeichenfolge] – die Assembly qualifizierte Name eines Typs der KeyedHashAlgorithm implementiert.</span><span class="sxs-lookup"><span data-stu-id="3d12e-140">ValidationAlgorithmType [string] - the assembly-qualified name of a type which implements KeyedHashAlgorithm.</span></span>

<span data-ttu-id="3d12e-141">Wenn EncryptionType beliebiger anderer Wert (außer Null / leer) verfügt, löst das Data Protection-System eine Ausnahme beim Start.</span><span class="sxs-lookup"><span data-stu-id="3d12e-141">If EncryptionType has any other value (other than null / empty), the data protection system will throw an exception at startup.</span></span>

>[!WARNING]
> <span data-ttu-id="3d12e-142">Wenn eine Standardeinstellung für die Richtlinie konfigurieren, die Typnamen (EncryptionAlgorithmType, ValidationAlgorithmType, KeyEscrowSinks) umfasst, müssen die Typen für die Anwendung verfügbar sein.</span><span class="sxs-lookup"><span data-stu-id="3d12e-142">When configuring a default policy setting that involves type names (EncryptionAlgorithmType, ValidationAlgorithmType, KeyEscrowSinks), the types must be available to the application.</span></span> <span data-ttu-id="3d12e-143">In der Praxis bedeutet dies, dass für Anwendungen, die in Desktop-CLR ausgeführt werden, die Assemblys, die diese Typen enthalten GACed werden soll.</span><span class="sxs-lookup"><span data-stu-id="3d12e-143">In practice, this means that for applications running on Desktop CLR, the assemblies which contain these types should be GACed.</span></span> <span data-ttu-id="3d12e-144">ASP.NET Core Anwendungen auf [.NET Core](https://www.microsoft.com/net/core), die Pakete, mit denen diese Typen enthalten, die installiert werden soll.</span><span class="sxs-lookup"><span data-stu-id="3d12e-144">For ASP.NET Core applications running on [.NET Core](https://www.microsoft.com/net/core), the packages which contain these types should be installed.</span></span>
