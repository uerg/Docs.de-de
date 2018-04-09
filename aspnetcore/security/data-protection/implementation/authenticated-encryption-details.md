---
title: Authentifizierte Verschlüsselungsdetails in ASP.NET Core
author: rick-anderson
description: Erfahren Sie mehr Details zur Implementierung der Verschlüsselung von ASP.NET Core Data Protection authentifiziert.
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/data-protection/implementation/authenticated-encryption-details
ms.openlocfilehash: 3ca5231e84156ede59793825e1a3e3bea0313055
ms.sourcegitcommit: 48beecfe749ddac52bc79aa3eb246a2dcdaa1862
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/22/2018
---
# <a name="authenticated-encryption-details-in-aspnet-core"></a><span data-ttu-id="d7670-103">Authentifizierte Verschlüsselungsdetails in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="d7670-103">Authenticated encryption details in ASP.NET Core</span></span>

<a name="data-protection-implementation-authenticated-encryption-details"></a>

<span data-ttu-id="d7670-104">Aufrufe von IDataProtector.Protect sind authentifizierte Verschlüsselungsvorgänge.</span><span class="sxs-lookup"><span data-stu-id="d7670-104">Calls to IDataProtector.Protect are authenticated encryption operations.</span></span> <span data-ttu-id="d7670-105">Die Protect-Methode bietet, Vertraulichkeit und Authentizität und es auf die Zweck-Kette, die verwendet wurde, dessen Stamm IDataProtectionProvider dieser bestimmten IDataProtector Instanz Ableitung gebunden ist.</span><span class="sxs-lookup"><span data-stu-id="d7670-105">The Protect method offers both confidentiality and authenticity, and it's tied to the purpose chain that was used to derive this particular IDataProtector instance from its root IDataProtectionProvider.</span></span>

<span data-ttu-id="d7670-106">IDataProtector.Protect nimmt einen Byte []-nur-Text-Parameter auf und erzeugt eine Byte [] geschützten Nutzlast, deren Format unten beschrieben ist.</span><span class="sxs-lookup"><span data-stu-id="d7670-106">IDataProtector.Protect takes a byte[] plaintext parameter and produces a byte[] protected payload, whose format is described below.</span></span> <span data-ttu-id="d7670-107">(Es ist auch eine Erweiterung methodenüberladung nimmt einen Zeichenfolgenparameter für nur-Text aus, und gibt eine Nutzlast mit geschützten zurück.</span><span class="sxs-lookup"><span data-stu-id="d7670-107">(There's also an extension method overload which takes a string plaintext parameter and returns a string protected payload.</span></span> <span data-ttu-id="d7670-108">Wenn diese API verwendet wird das geschützte nutzlastformat verfügen weiterhin über den unterhalb der Struktur ist jedoch sehr [base64url-codierte](https://tools.ietf.org/html/rfc4648#section-5).)</span><span class="sxs-lookup"><span data-stu-id="d7670-108">If this API is used the protected payload format will still have the below structure, but it will be [base64url-encoded](https://tools.ietf.org/html/rfc4648#section-5).)</span></span>

## <a name="protected-payload-format"></a><span data-ttu-id="d7670-109">Geschützte nutzlastformat</span><span class="sxs-lookup"><span data-stu-id="d7670-109">Protected payload format</span></span>

<span data-ttu-id="d7670-110">Die geschützte nutzlastformat besteht aus drei Hauptkomponenten:</span><span class="sxs-lookup"><span data-stu-id="d7670-110">The protected payload format consists of three primary components:</span></span>

* <span data-ttu-id="d7670-111">Eine 32-Bit-Header Magic, der die Version des Data Protection Systems angibt.</span><span class="sxs-lookup"><span data-stu-id="d7670-111">A 32-bit magic header that identifies the version of the data protection system.</span></span>

* <span data-ttu-id="d7670-112">Eine 128-Bit-Schlüssel-Id, die den Schlüssel zum Schützen dieser bestimmten Nutzlast identifiziert.</span><span class="sxs-lookup"><span data-stu-id="d7670-112">A 128-bit key id that identifies the key used to protect this particular payload.</span></span>

* <span data-ttu-id="d7670-113">Der Rest der geschützten Nutzlast ist [speziell für die Verschlüsselung durch diesen Schlüssel gekapselt](xref:security/data-protection/implementation/subkeyderivation#data-protection-implementation-subkey-derivation).</span><span class="sxs-lookup"><span data-stu-id="d7670-113">The remainder of the protected payload is [specific to the encryptor encapsulated by this key](xref:security/data-protection/implementation/subkeyderivation#data-protection-implementation-subkey-derivation).</span></span> <span data-ttu-id="d7670-114">Im folgenden Beispiel den Schlüssel darstellt, ein AES-256-CBC + HMACSHA256-Verschlüsselung und die Nutzlast ist weiter wie folgt unterteilt: \* Ein 128-Bit-Schlüssel-Modifizierer.</span><span class="sxs-lookup"><span data-stu-id="d7670-114">In the example below the key represents an AES-256-CBC + HMACSHA256 encryptor, and the payload is further subdivided as follows: \* A 128-bit key modifier.</span></span> <span data-ttu-id="d7670-115">\* Eine 128-Bit-Initialisierungsvektor.</span><span class="sxs-lookup"><span data-stu-id="d7670-115">\* A 128-bit initialization vector.</span></span> <span data-ttu-id="d7670-116">\* 48 Byte der AES-256-CBC-Ausgabe.</span><span class="sxs-lookup"><span data-stu-id="d7670-116">\* 48 bytes of AES-256-CBC output.</span></span> <span data-ttu-id="d7670-117">\* Ein Tag für den HMACSHA256-Authentifizierung.</span><span class="sxs-lookup"><span data-stu-id="d7670-117">\* An HMACSHA256 authentication tag.</span></span>

<span data-ttu-id="d7670-118">Eine geschützte Beispielnutzlast wird unten veranschaulicht.</span><span class="sxs-lookup"><span data-stu-id="d7670-118">A sample protected payload is illustrated below.</span></span>

```
09 F0 C9 F0 80 9C 81 0C 19 66 19 40 95 36 53 F8
AA FF EE 57 57 2F 40 4C 3F 7F CC 9D CC D9 32 3E
84 17 99 16 EC BA 1F 4A A1 18 45 1F 2D 13 7A 28
79 6B 86 9C F8 B7 84 F9 26 31 FC B1 86 0A F1 56
61 CF 14 58 D3 51 6F CF 36 50 85 82 08 2D 3F 73
5F B0 AD 9E 1A B2 AE 13 57 90 C8 F5 7C 95 4E 6A
8A AA 06 EF 43 CA 19 62 84 7C 11 B2 C8 71 9D AA
52 19 2E 5B 4C 1E 54 F0 55 BE 88 92 12 C1 4B 5E
52 C9 74 A0
```

<span data-ttu-id="d7670-119">Sind aus dem nutzlastformat über die ersten 32 Bits oder 4 Bytes der Magic-Header identifiziert die Version (09 d0 C9 d0)</span><span class="sxs-lookup"><span data-stu-id="d7670-119">From the payload format above the first 32 bits, or 4 bytes are the magic header identifying the version (09 F0 C9 F0)</span></span>

<span data-ttu-id="d7670-120">Die nächsten 128 Bits oder 16 Byte ist der Schlüsselbezeichner (80 9C 81 0C 19 66 19 40 95 36 53 F8 AA FF EE 57)</span><span class="sxs-lookup"><span data-stu-id="d7670-120">The next 128 bits, or 16 bytes is the key identifier (80 9C 81 0C 19 66 19 40 95 36 53 F8 AA FF EE 57)</span></span>

<span data-ttu-id="d7670-121">Im weiteren Verlauf enthält die Nutzlast und bezieht sich auf das verwendete Format.</span><span class="sxs-lookup"><span data-stu-id="d7670-121">The remainder contains the payload and is specific to the format used.</span></span>

>[!WARNING]
> <span data-ttu-id="d7670-122">Alle Nutzlasten, die auf einen bestimmten Schlüssel geschützt beginnt mit dem gleichen 20-Byte (Magic-Wert, Schlüssel-Id)-Header.</span><span class="sxs-lookup"><span data-stu-id="d7670-122">All payloads protected to a given key will begin with the same 20-byte (magic value, key id) header.</span></span> <span data-ttu-id="d7670-123">Administratoren können diese Tatsache zu Diagnosezwecken verwenden, um zu ermitteln, wenn eine Nutzlast generiert wurde.</span><span class="sxs-lookup"><span data-stu-id="d7670-123">Administrators can use this fact for diagnostic purposes to approximate when a payload was generated.</span></span> <span data-ttu-id="d7670-124">Beispielsweise entspricht die Nutzlast, die oben aufgeführten Schlüssel {0c819c80-6619-4019-9536-53f8aaffee57}.</span><span class="sxs-lookup"><span data-stu-id="d7670-124">For example, the payload above corresponds to key {0c819c80-6619-4019-9536-53f8aaffee57}.</span></span> <span data-ttu-id="d7670-125">Wenn nach dem Überprüfen der wichtigsten Repository finden Sie, dass dieser bestimmte Schlüssel Aktivierungsdatum 2015-01-01 wurde Ablaufdatum 2015-03-01 wurde, und es angemessen ist, anzunehmen, dass die Nutzlast (sofern nicht manipuliert) innerhalb dieses Zeitfensters, erteilen generiert wurde oder nehmen eine kleine Spielraum Faktor auf beiden Seiten an.</span><span class="sxs-lookup"><span data-stu-id="d7670-125">If after checking the key repository you find that this specific key's activation date was 2015-01-01 and its expiration date was 2015-03-01, then it's reasonable to assume that the payload (if not tampered with) was generated within that window, give or take a small fudge factor on either side.</span></span>
