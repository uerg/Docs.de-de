---
title: "Authentifizierte Verschlüsselungsdetails"
author: rick-anderson
description: "Dieses Dokument beschreibt die Implementierungsdetails des Datenschutzes ASP.NET Core authentifiziert Verschlüsselung."
keywords: "ASP.NET Core, Datenschutz, Authentifizierung, Verschlüsselung"
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 826e6d5d-9620-44e6-ad93-3b1d9969b70b
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/implementation/authenticated-encryption-details
ms.openlocfilehash: dc96412f6578e612a39e86ce00e1dc5a20cf84e9
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
# <a name="authenticated-encryption-details"></a><span data-ttu-id="16d7c-104">Authentifizierte Verschlüsselungsdetails</span><span class="sxs-lookup"><span data-stu-id="16d7c-104">Authenticated encryption details</span></span>

<a name="data-protection-implementation-authenticated-encryption-details"></a>

<span data-ttu-id="16d7c-105">Aufrufe von IDataProtector.Protect sind authentifizierte Verschlüsselungsvorgänge.</span><span class="sxs-lookup"><span data-stu-id="16d7c-105">Calls to IDataProtector.Protect are authenticated encryption operations.</span></span> <span data-ttu-id="16d7c-106">Die Protect-Methode bietet, Vertraulichkeit und Authentizität und es auf die Zweck-Kette, die verwendet wurde, dessen Stamm IDataProtectionProvider dieser bestimmten IDataProtector Instanz Ableitung gebunden ist.</span><span class="sxs-lookup"><span data-stu-id="16d7c-106">The Protect method offers both confidentiality and authenticity, and it is tied to the purpose chain that was used to derive this particular IDataProtector instance from its root IDataProtectionProvider.</span></span>

<span data-ttu-id="16d7c-107">IDataProtector.Protect nimmt einen Byte []-nur-Text-Parameter auf und erzeugt eine Byte [] geschützten Nutzlast, deren Format unten beschrieben ist.</span><span class="sxs-lookup"><span data-stu-id="16d7c-107">IDataProtector.Protect takes a byte[] plaintext parameter and produces a byte[] protected payload, whose format is described below.</span></span> <span data-ttu-id="16d7c-108">(Es ist auch eine Erweiterung methodenüberladung nimmt einen Zeichenfolgenparameter für nur-Text aus, und gibt eine Nutzlast mit geschützten zurück.</span><span class="sxs-lookup"><span data-stu-id="16d7c-108">(There is also an extension method overload which takes a string plaintext parameter and returns a string protected payload.</span></span> <span data-ttu-id="16d7c-109">Wenn diese API verwendet wird das geschützte nutzlastformat verfügen weiterhin über den unterhalb der Struktur ist jedoch sehr [base64url-codierte](https://tools.ietf.org/html/rfc4648#section-5).)</span><span class="sxs-lookup"><span data-stu-id="16d7c-109">If this API is used the protected payload format will still have the below structure, but it will be [base64url-encoded](https://tools.ietf.org/html/rfc4648#section-5).)</span></span>

## <a name="protected-payload-format"></a><span data-ttu-id="16d7c-110">Geschützte nutzlastformat</span><span class="sxs-lookup"><span data-stu-id="16d7c-110">Protected payload format</span></span>

<span data-ttu-id="16d7c-111">Die geschützte nutzlastformat besteht aus drei Hauptkomponenten:</span><span class="sxs-lookup"><span data-stu-id="16d7c-111">The protected payload format consists of three primary components:</span></span>

* <span data-ttu-id="16d7c-112">Eine 32-Bit-Header Magic, der die Version des Data Protection Systems angibt.</span><span class="sxs-lookup"><span data-stu-id="16d7c-112">A 32-bit magic header that identifies the version of the data protection system.</span></span>

* <span data-ttu-id="16d7c-113">Eine 128-Bit-Schlüssel-Id, die den Schlüssel zum Schützen dieser bestimmten Nutzlast identifiziert.</span><span class="sxs-lookup"><span data-stu-id="16d7c-113">A 128-bit key id that identifies the key used to protect this particular payload.</span></span>

* <span data-ttu-id="16d7c-114">Der Rest der geschützten Nutzlast ist [speziell für die Verschlüsselung durch diesen Schlüssel gekapselt](subkeyderivation.md#data-protection-implementation-subkey-derivation).</span><span class="sxs-lookup"><span data-stu-id="16d7c-114">The remainder of the protected payload is [specific to the encryptor encapsulated by this key](subkeyderivation.md#data-protection-implementation-subkey-derivation).</span></span> <span data-ttu-id="16d7c-115">Im folgenden Beispiel den Schlüssel darstellt, ein AES-256-CBC + HMACSHA256-Verschlüsselung und die Nutzlast ist weiter wie folgt unterteilt: * Ein 128-Bit-Schlüssel-Modifizierer.</span><span class="sxs-lookup"><span data-stu-id="16d7c-115">In the example below the key represents an AES-256-CBC + HMACSHA256 encryptor, and the payload is further subdivided as follows: * A 128-bit key modifier.</span></span> <span data-ttu-id="16d7c-116">* Eine 128-Bit-Initialisierungsvektor.</span><span class="sxs-lookup"><span data-stu-id="16d7c-116">* A 128-bit initialization vector.</span></span> <span data-ttu-id="16d7c-117">* 48 Byte der AES-256-CBC-Ausgabe.</span><span class="sxs-lookup"><span data-stu-id="16d7c-117">* 48 bytes of AES-256-CBC output.</span></span> <span data-ttu-id="16d7c-118">* Ein Tag für den HMACSHA256-Authentifizierung.</span><span class="sxs-lookup"><span data-stu-id="16d7c-118">* An HMACSHA256 authentication tag.</span></span>

<span data-ttu-id="16d7c-119">Eine geschützte Beispielnutzlast wird unten veranschaulicht.</span><span class="sxs-lookup"><span data-stu-id="16d7c-119">A sample protected payload is illustrated below.</span></span>

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

<span data-ttu-id="16d7c-120">Sind aus dem nutzlastformat über die ersten 32 Bits oder 4 Bytes der Magic-Header identifiziert die Version (09 d0 C9 d0)</span><span class="sxs-lookup"><span data-stu-id="16d7c-120">From the payload format above the first 32 bits, or 4 bytes are the magic header identifying the version (09 F0 C9 F0)</span></span>

<span data-ttu-id="16d7c-121">Die nächsten 128 Bits oder 16 Byte ist der Schlüsselbezeichner (80 9C 81 0C 19 66 19 40 95 36 53 F8 AA FF EE 57)</span><span class="sxs-lookup"><span data-stu-id="16d7c-121">The next 128 bits, or 16 bytes is the key identifier (80 9C 81 0C 19 66 19 40 95 36 53 F8 AA FF EE 57)</span></span>

<span data-ttu-id="16d7c-122">Im weiteren Verlauf enthält die Nutzlast und bezieht sich auf das verwendete Format.</span><span class="sxs-lookup"><span data-stu-id="16d7c-122">The remainder contains the payload and is specific to the format used.</span></span>

>[!WARNING]
> <span data-ttu-id="16d7c-123">Alle Nutzlasten, die auf einen bestimmten Schlüssel geschützt beginnt mit dem gleichen 20-Byte (Magic-Wert, Schlüssel-Id)-Header.</span><span class="sxs-lookup"><span data-stu-id="16d7c-123">All payloads protected to a given key will begin with the same 20-byte (magic value, key id) header.</span></span> <span data-ttu-id="16d7c-124">Administratoren können diese Tatsache zu Diagnosezwecken verwenden, um zu ermitteln, wenn eine Nutzlast generiert wurde.</span><span class="sxs-lookup"><span data-stu-id="16d7c-124">Administrators can use this fact for diagnostic purposes to approximate when a payload was generated.</span></span> <span data-ttu-id="16d7c-125">Beispielsweise entspricht die Nutzlast, die oben aufgeführten Schlüssel {0c819c80-6619-4019-9536-53f8aaffee57}.</span><span class="sxs-lookup"><span data-stu-id="16d7c-125">For example, the payload above corresponds to key {0c819c80-6619-4019-9536-53f8aaffee57}.</span></span> <span data-ttu-id="16d7c-126">Wenn nach dem Überprüfen der wichtigsten Repository finden Sie, dass dieser bestimmte Schlüssel Aktivierungsdatum 2015-01-01 wurde Ablaufdatum 2015-03-01 wurde, und es angemessen ist, anzunehmen, dass die Nutzlast (sofern nicht manipuliert) innerhalb dieses Zeitfensters, erteilen generiert wurde oder nehmen eine kleine Spielraum Faktor auf beiden Seiten an.</span><span class="sxs-lookup"><span data-stu-id="16d7c-126">If after checking the key repository you find that this specific key's activation date was 2015-01-01 and its expiration date was 2015-03-01, then it is reasonable to assume that the payload (if not tampered with) was generated within that window, give or take a small fudge factor on either side.</span></span>
