---
title: Kontext-Header
author: rick-anderson
description: 
keywords: ASP.NET Core,
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: d026a58c-67f4-411e-a410-c35f29c2c517
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/implementation/context-headers
ms.openlocfilehash: 5688ff2c36907231f88d45cef4ae1b1c60ab44ab
ms.sourcegitcommit: 67f54fabbfa4e3942f5bfe1f8a7fdfe4a7a75358
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/19/2017
---
# <a name="context-headers"></a><span data-ttu-id="ea443-103">Kontext-Header</span><span class="sxs-lookup"><span data-stu-id="ea443-103">Context headers</span></span>

<a name=data-protection-implementation-context-headers></a>

## <a name="background-and-theory"></a><span data-ttu-id="ea443-104">Hintergrund und die Theorie</span><span class="sxs-lookup"><span data-stu-id="ea443-104">Background and theory</span></span>

<span data-ttu-id="ea443-105">In das Data Protection System bedeutet eine "Key" an, dass ein Objekt, das bereitstellen kann Verschlüsselungsdienste authentifiziert.</span><span class="sxs-lookup"><span data-stu-id="ea443-105">In the data protection system, a "key" means an object that can provide authenticated encryption services.</span></span> <span data-ttu-id="ea443-106">Jeder Schlüssel wird durch eine eindeutige Id (eine GUID) identifiziert, und mit ihm enthält algorithmische Informationen und entropic Materialien.</span><span class="sxs-lookup"><span data-stu-id="ea443-106">Each key is identified by a unique id (a GUID), and it carries with it algorithmic information and entropic material.</span></span> <span data-ttu-id="ea443-107">Es richtet sich an, dass jeder Schlüssel enthalten eindeutige Entropie jedoch das System kann nicht zu erzwingen, und wir auch erforderlich, dass Entwickler, die den Schlüssel Ring manuell durch Ändern der algorithmischen Informationen eines vorhandenen Schlüssels im Schlüssel Ring ändern kann.</span><span class="sxs-lookup"><span data-stu-id="ea443-107">It is intended that each key carry unique entropy, but the system cannot enforce that, and we also need to account for developers who might change the key ring manually by modifying the algorithmic information of an existing key in the key ring.</span></span> <span data-ttu-id="ea443-108">Unsere sicherheitsanforderungen erhält diesen Fällen erzielen die Datenschutzsystem ein initialisierungskonzept hat [kryptografische Flexibilität](https://www.microsoft.com/en-us/research/publication/cryptographic-agility-and-its-relation-to-circular-encryption/), womit sicher über einen Einzelwert entropic über mehrere kryptografischen Algorithmen.</span><span class="sxs-lookup"><span data-stu-id="ea443-108">To achieve our security requirements given these cases the data protection system has a concept of [cryptographic agility](https://www.microsoft.com/en-us/research/publication/cryptographic-agility-and-its-relation-to-circular-encryption/), which allows securely using a single entropic value across multiple cryptographic algorithms.</span></span>

<span data-ttu-id="ea443-109">Die meisten Systeme die kryptografische Flexibilität unterstützen dazu einige identifizierende Informationen zum Algorithmus in die Nutzlast einschließlich.</span><span class="sxs-lookup"><span data-stu-id="ea443-109">Most systems which support cryptographic agility do so by including some identifying information about the algorithm inside the payload.</span></span> <span data-ttu-id="ea443-110">Der Algorithmus die OID ist im Allgemeinen eignet sich für die dies.</span><span class="sxs-lookup"><span data-stu-id="ea443-110">The algorithm's OID is generally a good candidate for this.</span></span> <span data-ttu-id="ea443-111">Ist jedoch ein Problem, das wir wurde, es mehrere Möglichkeiten gibt, die denselben Algorithmusnamen angeben: "AES" (CNG) und den verwalteten Aes, AesManaged AesCryptoServiceProvider, AesCng und RijndaelManaged (bestimmte bestimmte Parameter) Klassen sind alle tatsächlich identisch Bedeutung, und es müsste eine Zuordnung aller dieser an die richtige OID verwalten.</span><span class="sxs-lookup"><span data-stu-id="ea443-111">However, one problem that we ran into is that there are multiple ways to specify the same algorithm: "AES" (CNG) and the managed Aes, AesManaged, AesCryptoServiceProvider, AesCng, and RijndaelManaged (given specific parameters) classes are all actually the same thing, and we'd need to maintain a mapping of all of these to the correct OID.</span></span> <span data-ttu-id="ea443-112">Wenn ein Entwickler ein benutzerdefinierter Algorithmus (oder sogar eine andere Implementierung der AES!) sind möchte, müssten sie uns informieren, dass die OID.</span><span class="sxs-lookup"><span data-stu-id="ea443-112">If a developer wanted to provide a custom algorithm (or even another implementation of AES!), they'd have to tell us its OID.</span></span> <span data-ttu-id="ea443-113">Diese zusätzlichen Registrierungsschritt macht Systemkonfiguration besonders qualvoll.</span><span class="sxs-lookup"><span data-stu-id="ea443-113">This extra registration step makes system configuration particularly painful.</span></span>

<span data-ttu-id="ea443-114">Ausführen in Einzelschritten zurück, haben wir entschieden, dass wir das Problem aus der falschen Richtung fast erreicht wurden.</span><span class="sxs-lookup"><span data-stu-id="ea443-114">Stepping back, we decided that we were approaching the problem from the wrong direction.</span></span> <span data-ttu-id="ea443-115">Eine OID gibt an, was der Algorithmus ist, aber wir nicht tatsächlich zu diesem erledigen.</span><span class="sxs-lookup"><span data-stu-id="ea443-115">An OID tells you what the algorithm is, but we don't actually care about this.</span></span> <span data-ttu-id="ea443-116">Wenn wir einen Einzelwert entropic sicher in zwei verschiedene Algorithmen verwenden müssen, ist es nicht für uns wissen, was tatsächlich die Algorithmen sind erforderlich.</span><span class="sxs-lookup"><span data-stu-id="ea443-116">If we need to use a single entropic value securely in two different algorithms, it's not necessary for us to know what the algorithms actually are.</span></span> <span data-ttu-id="ea443-117">Was tatsächlich wichtig ist, wie sie sich Verhalten.</span><span class="sxs-lookup"><span data-stu-id="ea443-117">What we actually care about is how they behave.</span></span> <span data-ttu-id="ea443-118">Alle ordentliche symmetrischen Block-Verschlüsselungsalgorithmus wird auch eine starke pseudozufälligen Permutation (PRP): Korrigieren Sie die Eingaben (Schlüssel, IV-Modus nur-Text verketten) und die Chiffretext Ausgabe mit einer Wahrscheinlichkeit Überlastung werden andere symmetrische Blockchiffre unterscheiden der Algorithmus den gleichen Eingaben.</span><span class="sxs-lookup"><span data-stu-id="ea443-118">Any decent symmetric block cipher algorithm is also a strong pseudorandom permutation (PRP): fix the inputs (key, chaining mode, IV, plaintext) and the ciphertext output will with overwhelming probability be distinct from any other symmetric block cipher algorithm given the same inputs.</span></span> <span data-ttu-id="ea443-119">Auf ähnliche Weise alle ordentliche eines schlüsselgebundenen Hashalgorithmus-Funktion ist auch ein sicheres pseudozufälligen (PRF), und angegebene einen festen Eingabeset seine Ausgabe überwältigend werden alle anderen eines schlüsselgebundenen Hashalgorithmus-Funktion unterscheidet.</span><span class="sxs-lookup"><span data-stu-id="ea443-119">Similarly, any decent keyed hash function is also a strong pseudorandom function (PRF), and given a fixed input set its output will overwhelmingly be distinct from any other keyed hash function.</span></span>

<span data-ttu-id="ea443-120">Wir verwenden dieses Konzept der starken PRPs und PRFs einen Kontextheader erstellen.</span><span class="sxs-lookup"><span data-stu-id="ea443-120">We use this concept of strong PRPs and PRFs to build up a context header.</span></span> <span data-ttu-id="ea443-121">Diese Kontextheader fungiert im Wesentlichen als einen stabilen Fingerabdruck über die Algorithmen für einen bestimmten Vorgang verwendet, und bietet die kryptografische Flexibilität, die von der Datenschutzsystem benötigt werden.</span><span class="sxs-lookup"><span data-stu-id="ea443-121">This context header essentially acts as a stable thumbprint over the algorithms in use for any given operation, and it provides the cryptographic agility needed by the data protection system.</span></span> <span data-ttu-id="ea443-122">Dieser Header ist reproduzierbar und wird später verwendet, als Teil der [Unterschlüssel Ableitung Prozess](subkeyderivation.md#data-protection-implementation-subkey-derivation).</span><span class="sxs-lookup"><span data-stu-id="ea443-122">This header is reproducible and is used later as part of the [subkey derivation process](subkeyderivation.md#data-protection-implementation-subkey-derivation).</span></span> <span data-ttu-id="ea443-123">Es gibt zwei Möglichkeiten, um die Kontextheader je nach den Betriebsmodi der zugrunde liegenden Algorithmen zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="ea443-123">There are two different ways to build the context header depending on the modes of operation of the underlying algorithms.</span></span>

## <a name="cbc-mode-encryption--hmac-authentication"></a><span data-ttu-id="ea443-124">Verschlüsselung im CBC-Modus + HMAC-Authentifizierung</span><span class="sxs-lookup"><span data-stu-id="ea443-124">CBC-mode encryption + HMAC authentication</span></span>

<a name=data-protection-implementation-context-headers-cbc-components></a>

<span data-ttu-id="ea443-125">Die Kontextheader besteht aus folgenden Komponenten:</span><span class="sxs-lookup"><span data-stu-id="ea443-125">The context header consists of the following components:</span></span>

* <span data-ttu-id="ea443-126">[16 Bits] Der Wert 00 00, also ein Marker bedeutet "CBC-Verschlüsselung + HMAC-Authentifizierung".</span><span class="sxs-lookup"><span data-stu-id="ea443-126">[16 bits] The value 00 00, which is a marker meaning "CBC encryption + HMAC authentication".</span></span>

* <span data-ttu-id="ea443-127">[32 Bits] Die Schlüssellänge (in Byte, big-Endian) von der Block symmetrischen Verschlüsselungsalgorithmus.</span><span class="sxs-lookup"><span data-stu-id="ea443-127">[32 bits] The key length (in bytes, big-endian) of the symmetric block cipher algorithm.</span></span>

* <span data-ttu-id="ea443-128">[32 Bits] Die Blockgröße (in Byte, big-Endian) von der Block symmetrischen Verschlüsselungsalgorithmus.</span><span class="sxs-lookup"><span data-stu-id="ea443-128">[32 bits] The block size (in bytes, big-endian) of the symmetric block cipher algorithm.</span></span>

* <span data-ttu-id="ea443-129">[32 Bits] Die Schlüssellänge (in Byte, big-Endian) des HMAC-Algorithmus.</span><span class="sxs-lookup"><span data-stu-id="ea443-129">[32 bits] The key length (in bytes, big-endian) of the HMAC algorithm.</span></span> <span data-ttu-id="ea443-130">(Derzeit entspricht die Größe des Schlüssels immer die Digest-Größe.)</span><span class="sxs-lookup"><span data-stu-id="ea443-130">(Currently the key size always matches the digest size.)</span></span>

* <span data-ttu-id="ea443-131">[32 Bits] Die Digest-Größe (in Bytes, big-Endian) des HMAC-Algorithmus.</span><span class="sxs-lookup"><span data-stu-id="ea443-131">[32 bits] The digest size (in bytes, big-endian) of the HMAC algorithm.</span></span>

* <span data-ttu-id="ea443-132">EncCBC (K_E, IV, ""), die die Ausgabe der Block symmetrischen Verschlüsselungsalgorithmus eine leere Zeichenfolge Eingaben und IV ist ein Vektor Nullen.</span><span class="sxs-lookup"><span data-stu-id="ea443-132">EncCBC(K_E, IV, ""), which is the output of the symmetric block cipher algorithm given an empty string input and where IV is an all-zero vector.</span></span> <span data-ttu-id="ea443-133">Die zur Erstellung der K_E wird im folgenden beschrieben.</span><span class="sxs-lookup"><span data-stu-id="ea443-133">The construction of K_E is described below.</span></span>

* <span data-ttu-id="ea443-134">MAC (K_H, ""), ist die Ausgabe des HMAC-Algorithmus eine leere Zeichenfolge Eingaben.</span><span class="sxs-lookup"><span data-stu-id="ea443-134">MAC(K_H, ""), which is the output of the HMAC algorithm given an empty string input.</span></span> <span data-ttu-id="ea443-135">Die zur Erstellung der K_H wird im folgenden beschrieben.</span><span class="sxs-lookup"><span data-stu-id="ea443-135">The construction of K_H is described below.</span></span>

<span data-ttu-id="ea443-136">Im Idealfall könnten wir Nullen Vektoren für K_E und K_H übergeben.</span><span class="sxs-lookup"><span data-stu-id="ea443-136">Ideally we could pass all-zero vectors for K_E and K_H.</span></span> <span data-ttu-id="ea443-137">Allerdings möchten wir die Situation zu vermeiden, der zugrunde liegende Algorithmus, in denen das Vorhandensein der schwachen Schlüssel vor der Durchführung aller Vorgänge (insbesondere DES und 3DES) überprüft werden kann, die unter Verwendung eines einfachen oder repeatable-Musters wie ein Vektor Nullen, schließt.</span><span class="sxs-lookup"><span data-stu-id="ea443-137">However, we want to avoid the situation where the underlying algorithm checks for the existence of weak keys before performing any operations (notably DES and 3DES), which precludes using a simple or repeatable pattern like an all-zero vector.</span></span>

<span data-ttu-id="ea443-138">Stattdessen verwenden wir die NIST SP800 108 KDF in Counter-Modus (finden Sie unter [NIST SP800-108](http://nvlpubs.nist.gov/nistpubs/Legacy/SP/nistspecialpublication800-108.pdf), Sek. 5.1) mit NULL-Schlüssel, Bezeichnung und Kontext und HMACSHA512 als zugrunde liegenden PRF.</span><span class="sxs-lookup"><span data-stu-id="ea443-138">Instead, we use the NIST SP800-108 KDF in Counter Mode (see [NIST SP800-108](http://nvlpubs.nist.gov/nistpubs/Legacy/SP/nistspecialpublication800-108.pdf), Sec. 5.1) with a zero-length key, label, and context and HMACSHA512 as the underlying PRF.</span></span> <span data-ttu-id="ea443-139">Wir leiten | K_E | + | K_H | Bytes der Ausgabe, klicken Sie dann das Ergebnis wird in grundtypelemente zerlegt K_E und K_H selbst.</span><span class="sxs-lookup"><span data-stu-id="ea443-139">We derive | K_E | + | K_H | bytes of output, then decompose the result into K_E and K_H themselves.</span></span> <span data-ttu-id="ea443-140">Mathematisch, dies wie folgt dargestellt wird.</span><span class="sxs-lookup"><span data-stu-id="ea443-140">Mathematically, this is represented as follows.</span></span>

<span data-ttu-id="ea443-141">(K_E || K_H) = SP800_108_CTR (prf = HMACSHA512 Key = "", Label = "", Kontext = "")</span><span class="sxs-lookup"><span data-stu-id="ea443-141">( K_E || K_H ) = SP800_108_CTR(prf = HMACSHA512, key = "", label = "", context = "")</span></span>

### <a name="example-aes-192-cbc--hmacsha256"></a><span data-ttu-id="ea443-142">Beispiel: AES-192-CBC + HMACSHA256</span><span class="sxs-lookup"><span data-stu-id="ea443-142">Example: AES-192-CBC + HMACSHA256</span></span>

<span data-ttu-id="ea443-143">Betrachten Sie beispielsweise die Groß-/Kleinschreibung, wobei der Block symmetrischen Verschlüsselungsalgorithmus AES-192-CBC und der Validierungsalgorithmus ist HMACSHA256, aus.</span><span class="sxs-lookup"><span data-stu-id="ea443-143">As an example, consider the case where the symmetric block cipher algorithm is AES-192-CBC and the validation algorithm is HMACSHA256.</span></span> <span data-ttu-id="ea443-144">Das System würde die Kontextheader mithilfe der folgenden Schritte generieren.</span><span class="sxs-lookup"><span data-stu-id="ea443-144">The system would generate the context header using the following steps.</span></span>

<span data-ttu-id="ea443-145">Erstens können (K_E || K_H) = SP800_108_CTR (prf = HMACSHA512 Key = "", Label = "", Kontext = ""), wobei | K_E | = 192 Bits und | K_H | = 256 Bits pro angegebenen Algorithmen.</span><span class="sxs-lookup"><span data-stu-id="ea443-145">First, let ( K_E || K_H ) = SP800_108_CTR(prf = HMACSHA512, key = "", label = "", context = ""), where | K_E | = 192 bits and | K_H | = 256 bits per the specified algorithms.</span></span> <span data-ttu-id="ea443-146">Dies führt zu K_E = 5BB6... 21DD und K_H = A04A... 00A9 im folgenden Beispiel:</span><span class="sxs-lookup"><span data-stu-id="ea443-146">This leads to K_E = 5BB6..21DD and K_H = A04A..00A9 in the example below:</span></span>

<!-- literal_block {"ids": [], "xml:space": "preserve"} -->

```
5B B6 C9 83 13 78 22 1D 8E 10 73 CA CF 65 8E B0
   61 62 42 71 CB 83 21 DD A0 4A 05 00 5B AB C0 A2
   49 6F A5 61 E3 E2 49 87 AA 63 55 CD 74 0A DA C4
   B7 92 3D BF 59 90 00 A9
   ```

<span data-ttu-id="ea443-147">Als Nächstes berechnen Enc_CBC (K_E, IV, "") für AES-192-CBC angegebene IV = 0 * und K_E wie oben beschrieben.</span><span class="sxs-lookup"><span data-stu-id="ea443-147">Next, compute Enc_CBC (K_E, IV, "") for AES-192-CBC given IV = 0* and K_E as above.</span></span>

<span data-ttu-id="ea443-148">Ergebnis: F474B1872B3B53E4721DE19C0841DB6F =</span><span class="sxs-lookup"><span data-stu-id="ea443-148">result := F474B1872B3B53E4721DE19C0841DB6F</span></span>

<span data-ttu-id="ea443-149">Als Nächstes berechnen MAC (K_H, "") für HMACSHA256 K_H wie oben angegeben.</span><span class="sxs-lookup"><span data-stu-id="ea443-149">Next, compute MAC(K_H, "") for HMACSHA256 given K_H as above.</span></span>

<span data-ttu-id="ea443-150">Ergebnis: D4791184B996092EE1202F36E8608FA8FBD98ABDFF5402F264B1D7211536220C =</span><span class="sxs-lookup"><span data-stu-id="ea443-150">result := D4791184B996092EE1202F36E8608FA8FBD98ABDFF5402F264B1D7211536220C</span></span>

<span data-ttu-id="ea443-151">Dadurch wird die vollständige Kontextheader unten generiert:</span><span class="sxs-lookup"><span data-stu-id="ea443-151">This produces the full context header below:</span></span>

<!-- literal_block {"ids": [], "xml:space": "preserve"} -->

```
00 00 00 00 00 18 00 00 00 10 00 00 00 20 00 00
   00 20 F4 74 B1 87 2B 3B 53 E4 72 1D E1 9C 08 41
   DB 6F D4 79 11 84 B9 96 09 2E E1 20 2F 36 E8 60
   8F A8 FB D9 8A BD FF 54 02 F2 64 B1 D7 21 15 36
   22 0C
   ```

<span data-ttu-id="ea443-152">Dieser Kontextheader ist der Fingerabdruck des Paars Algorithmus authentifizierten Verschlüsselung (Verschlüsselung mit AES-192-CBC + HMACSHA256-Überprüfung).</span><span class="sxs-lookup"><span data-stu-id="ea443-152">This context header is the thumbprint of the authenticated encryption algorithm pair (AES-192-CBC encryption + HMACSHA256 validation).</span></span> <span data-ttu-id="ea443-153">Die Komponenten, wie beschrieben [oben](xref:security/data-protection/implementation/context-headers#data-protection-implementation-context-headers-cbc-components) sind:</span><span class="sxs-lookup"><span data-stu-id="ea443-153">The components, as described [above](xref:security/data-protection/implementation/context-headers#data-protection-implementation-context-headers-cbc-components) are:</span></span>

* <span data-ttu-id="ea443-154">die Markierung (00 00)</span><span class="sxs-lookup"><span data-stu-id="ea443-154">the marker (00 00)</span></span>

* <span data-ttu-id="ea443-155">Block Cipher Schlüssellänge (00-00-00-18)</span><span class="sxs-lookup"><span data-stu-id="ea443-155">the block cipher key length (00 00 00 18)</span></span>

* <span data-ttu-id="ea443-156">die Blockgröße für das "Cipher Block" (00-00-00-10)</span><span class="sxs-lookup"><span data-stu-id="ea443-156">the block cipher block size (00 00 00 10)</span></span>

* <span data-ttu-id="ea443-157">der HMAC-Schlüssellänge (00-00-00-20)</span><span class="sxs-lookup"><span data-stu-id="ea443-157">the HMAC key length (00 00 00 20)</span></span>

* <span data-ttu-id="ea443-158">der HMAC-Digest-Größe (00-00-00-20)</span><span class="sxs-lookup"><span data-stu-id="ea443-158">the HMAC digest size (00 00 00 20)</span></span>

* <span data-ttu-id="ea443-159">die Blockchiffre PRP Ausgabe (F4 74 - DB 6F) und</span><span class="sxs-lookup"><span data-stu-id="ea443-159">the block cipher PRP output (F4 74 - DB 6F) and</span></span>

* <span data-ttu-id="ea443-160">die Ausgabe des HMAC-PRF (D4 79 - End).</span><span class="sxs-lookup"><span data-stu-id="ea443-160">the HMAC PRF output (D4 79 - end).</span></span>

> [!NOTE]
> <span data-ttu-id="ea443-161">Die Verschlüsselung im CBC-Modus + HMAC Authentifizierungsheader-Kontext basiert die gleiche Weise, unabhängig davon, ob die Algorithmen Implementierungen von Windows CNG oder von verwalteten SymmetricAlgorithm und KeyedHashAlgorithm Typen bereitgestellt werden.</span><span class="sxs-lookup"><span data-stu-id="ea443-161">The CBC-mode encryption + HMAC authentication context header is built the same way regardless of whether the algorithms implementations are provided by Windows CNG or by managed SymmetricAlgorithm and KeyedHashAlgorithm types.</span></span> <span data-ttu-id="ea443-162">Dadurch können Anwendungen, die auf anderen Betriebssystemen, die die gleichen Kontextheader zuverlässig zu erzeugen, obwohl Betriebssysteme Implementierungen der Algorithmen unterscheiden.</span><span class="sxs-lookup"><span data-stu-id="ea443-162">This allows applications running on different operating systems to reliably produce the same context header even though the implementations of the algorithms differ between OSes.</span></span> <span data-ttu-id="ea443-163">(In der Praxis nicht die KeyedHashAlgorithm muss einen richtigen HMAC sein.</span><span class="sxs-lookup"><span data-stu-id="ea443-163">(In practice, the KeyedHashAlgorithm doesn't have to be a proper HMAC.</span></span> <span data-ttu-id="ea443-164">Es kann eines schlüsselgebundenen Hashalgorithmus Algorithmustyp sein.)</span><span class="sxs-lookup"><span data-stu-id="ea443-164">It can be any keyed hash algorithm type.)</span></span>

### <a name="example-3des-192-cbc--hmacsha1"></a><span data-ttu-id="ea443-165">Beispiel: 3DES-192-CBC + HMACSHA1</span><span class="sxs-lookup"><span data-stu-id="ea443-165">Example: 3DES-192-CBC + HMACSHA1</span></span>

<span data-ttu-id="ea443-166">Erstens können (K_E || K_H) = SP800_108_CTR (prf = HMACSHA512 Key = "", Label = "", Kontext = ""), wobei | K_E | = 192 Bits und | K_H | = 160 Bits pro angegebenen Algorithmen.</span><span class="sxs-lookup"><span data-stu-id="ea443-166">First, let ( K_E || K_H ) = SP800_108_CTR(prf = HMACSHA512, key = "", label = "", context = ""), where | K_E | = 192 bits and | K_H | = 160 bits per the specified algorithms.</span></span> <span data-ttu-id="ea443-167">Dies führt zu K_E = A219... E2BB und K_H = DC4A... B464 im folgenden Beispiel:</span><span class="sxs-lookup"><span data-stu-id="ea443-167">This leads to K_E = A219..E2BB and K_H = DC4A..B464 in the example below:</span></span>

<!-- literal_block {"ids": [], "xml:space": "preserve"} -->

```
A2 19 60 2F 83 A9 13 EA B0 61 3A 39 B8 A6 7E 22
   61 D9 F8 6C 10 51 E2 BB DC 4A 00 D7 03 A2 48 3E
   D1 F7 5A 34 EB 28 3E D7 D4 67 B4 64
   ```

<span data-ttu-id="ea443-168">Als Nächstes berechnen Enc_CBC (K_E, IV, "") für 3DES-192-CBC angegebene IV = 0 * und K_E wie oben beschrieben.</span><span class="sxs-lookup"><span data-stu-id="ea443-168">Next, compute Enc_CBC (K_E, IV, "") for 3DES-192-CBC given IV = 0* and K_E as above.</span></span>

<span data-ttu-id="ea443-169">Ergebnis: ABB100F81E53E10E =</span><span class="sxs-lookup"><span data-stu-id="ea443-169">result := ABB100F81E53E10E</span></span>

<span data-ttu-id="ea443-170">Als Nächstes berechnen MAC (K_H, "") für HMACSHA1 K_H wie oben angegeben.</span><span class="sxs-lookup"><span data-stu-id="ea443-170">Next, compute MAC(K_H, "") for HMACSHA1 given K_H as above.</span></span>

<span data-ttu-id="ea443-171">Ergebnis: 76EB189B35CF03461DDF877CD9F4B1B4D63A7555 =</span><span class="sxs-lookup"><span data-stu-id="ea443-171">result := 76EB189B35CF03461DDF877CD9F4B1B4D63A7555</span></span>

<span data-ttu-id="ea443-172">Dies erzeugt die vollständige Kontextheader also einen Fingerabdruck, der die authentifizierte Verschlüsselung Algorithmus Paar (3DES-192-CBC-Verschlüsselung + HMACSHA1 Überprüfung) unten angezeigt:</span><span class="sxs-lookup"><span data-stu-id="ea443-172">This produces the full context header which is a thumbprint of the authenticated encryption algorithm pair (3DES-192-CBC encryption + HMACSHA1 validation), shown below:</span></span>

<!-- literal_block {"ids": [], "xml:space": "preserve"} -->

```
00 00 00 00 00 18 00 00 00 08 00 00 00 14 00 00
   00 14 AB B1 00 F8 1E 53 E1 0E 76 EB 18 9B 35 CF
   03 46 1D DF 87 7C D9 F4 B1 B4 D6 3A 75 55
   ```

<span data-ttu-id="ea443-173">Die Komponenten aufgliedern wie folgt:</span><span class="sxs-lookup"><span data-stu-id="ea443-173">The components break down as follows:</span></span>

* <span data-ttu-id="ea443-174">die Markierung (00 00)</span><span class="sxs-lookup"><span data-stu-id="ea443-174">the marker (00 00)</span></span>

* <span data-ttu-id="ea443-175">Block Cipher Schlüssellänge (00-00-00-18)</span><span class="sxs-lookup"><span data-stu-id="ea443-175">the block cipher key length (00 00 00 18)</span></span>

* <span data-ttu-id="ea443-176">die Blockgröße für das "Cipher Block" (00-00-00-08)</span><span class="sxs-lookup"><span data-stu-id="ea443-176">the block cipher block size (00 00 00 08)</span></span>

* <span data-ttu-id="ea443-177">der HMAC-Schlüssellänge (00-00-00-14)</span><span class="sxs-lookup"><span data-stu-id="ea443-177">the HMAC key length (00 00 00 14)</span></span>

* <span data-ttu-id="ea443-178">der HMAC-Digest-Größe (00-00-00-14)</span><span class="sxs-lookup"><span data-stu-id="ea443-178">the HMAC digest size (00 00 00 14)</span></span>

* <span data-ttu-id="ea443-179">die Blockchiffre PRP Ausgabe (B1 AB - E1 0E) und</span><span class="sxs-lookup"><span data-stu-id="ea443-179">the block cipher PRP output (AB B1 - E1 0E) and</span></span>

* <span data-ttu-id="ea443-180">die Ausgabe des HMAC-PRF (76 "EB" - End).</span><span class="sxs-lookup"><span data-stu-id="ea443-180">the HMAC PRF output (76 EB - end).</span></span>

## <a name="galoiscounter-mode-encryption--authentication"></a><span data-ttu-id="ea443-181">Galois/Counter-Modus-Verschlüsselung und Authentifizierung</span><span class="sxs-lookup"><span data-stu-id="ea443-181">Galois/Counter Mode encryption + authentication</span></span>

<span data-ttu-id="ea443-182">Die Kontextheader besteht aus folgenden Komponenten:</span><span class="sxs-lookup"><span data-stu-id="ea443-182">The context header consists of the following components:</span></span>

* <span data-ttu-id="ea443-183">[16 Bits] Der Wert 00 01, also ein Marker bedeutet "GCM-Verschlüsselung + Authentifizierung".</span><span class="sxs-lookup"><span data-stu-id="ea443-183">[16 bits] The value 00 01, which is a marker meaning "GCM encryption + authentication".</span></span>

* <span data-ttu-id="ea443-184">[32 Bits] Die Schlüssellänge (in Byte, big-Endian) von der Block symmetrischen Verschlüsselungsalgorithmus.</span><span class="sxs-lookup"><span data-stu-id="ea443-184">[32 bits] The key length (in bytes, big-endian) of the symmetric block cipher algorithm.</span></span>

* <span data-ttu-id="ea443-185">[32 Bits] Die Nonce Größe (in Byte, big-Endian) während der authentifizierten Verschlüsselungsvorgänge verwendet.</span><span class="sxs-lookup"><span data-stu-id="ea443-185">[32 bits] The nonce size (in bytes, big-endian) used during authenticated encryption operations.</span></span> <span data-ttu-id="ea443-186">(Für unser System ist dies korrigiert in Nonce Größe = 96 Bit.)</span><span class="sxs-lookup"><span data-stu-id="ea443-186">(For our system, this is fixed at nonce size = 96 bits.)</span></span>

* <span data-ttu-id="ea443-187">[32 Bits] Die Blockgröße (in Byte, big-Endian) von der Block symmetrischen Verschlüsselungsalgorithmus.</span><span class="sxs-lookup"><span data-stu-id="ea443-187">[32 bits] The block size (in bytes, big-endian) of the symmetric block cipher algorithm.</span></span> <span data-ttu-id="ea443-188">(Für GCM, wird dies zur Blockgröße behoben = 128 Bit.)</span><span class="sxs-lookup"><span data-stu-id="ea443-188">(For GCM, this is fixed at block size = 128 bits.)</span></span>

* <span data-ttu-id="ea443-189">[32 Bits] Die Authentifizierung Tag Größe (in Bytes, big-Endian) von den authentifizierten Verschlüsselungsfunktion generiert wurde.</span><span class="sxs-lookup"><span data-stu-id="ea443-189">[32 bits] The authentication tag size (in bytes, big-endian) produced by the authenticated encryption function.</span></span> <span data-ttu-id="ea443-190">(Für unser System ist dies am Tag Größe behoben = 128 Bit.)</span><span class="sxs-lookup"><span data-stu-id="ea443-190">(For our system, this is fixed at tag size = 128 bits.)</span></span>

* <span data-ttu-id="ea443-191">[128 Bits] Das Tag Enc_GCM (K_E Nonce, ""), die die Ausgabe der Block symmetrischen Verschlüsselungsalgorithmus eine leere Zeichenfolge Eingaben und Nonce ist ein 96-Bit-Nullen Vektor.</span><span class="sxs-lookup"><span data-stu-id="ea443-191">[128 bits] The tag of Enc_GCM (K_E, nonce, ""), which is the output of the symmetric block cipher algorithm given an empty string input and where nonce is a 96-bit all-zero vector.</span></span>

<span data-ttu-id="ea443-192">Nutzen denselben Mechanismus wie die Verschlüsselung CBC + HMAC-Szenario für Verbundauthentifizierung K_E abgeleitet.</span><span class="sxs-lookup"><span data-stu-id="ea443-192">K_E is derived using the same mechanism as in the CBC encryption + HMAC authentication scenario.</span></span> <span data-ttu-id="ea443-193">Da im Play hier keine K_H vorhanden ist, wird im Wesentlichen haben jedoch | K_H | = 0, und der Algorithmus wird reduziert die im folgenden Format.</span><span class="sxs-lookup"><span data-stu-id="ea443-193">However, since there is no K_H in play here, we essentially have | K_H | = 0, and the algorithm collapses to the below form.</span></span>

<span data-ttu-id="ea443-194">K_E = SP800_108_CTR (prf = HMACSHA512 Key = "", Label = "", Kontext = "")</span><span class="sxs-lookup"><span data-stu-id="ea443-194">K_E = SP800_108_CTR(prf = HMACSHA512, key = "", label = "", context = "")</span></span>

### <a name="example-aes-256-gcm"></a><span data-ttu-id="ea443-195">Beispiel: AES-256-GCM</span><span class="sxs-lookup"><span data-stu-id="ea443-195">Example: AES-256-GCM</span></span>

<span data-ttu-id="ea443-196">Zunächst können K_E = SP800_108_CTR (prf HMACSHA512 = Schlüssel = "", Label = "", Kontext = ""), wobei | K_E | = 256 Bits.</span><span class="sxs-lookup"><span data-stu-id="ea443-196">First, let K_E = SP800_108_CTR(prf = HMACSHA512, key = "", label = "", context = ""), where | K_E | = 256 bits.</span></span>

<span data-ttu-id="ea443-197">K_E: 22BC6F1B171C08C4AE2F27444AF8FC8B3087A90006CAEA91FDCFB47C1B8733B8 =</span><span class="sxs-lookup"><span data-stu-id="ea443-197">K_E := 22BC6F1B171C08C4AE2F27444AF8FC8B3087A90006CAEA91FDCFB47C1B8733B8</span></span>

<span data-ttu-id="ea443-198">Als Nächstes Berechnen der authentifizierungstag von Enc_GCM (K_E Nonce, "") für AES-256-GCM angegebene Nonce = 096 und K_E wie oben beschrieben.</span><span class="sxs-lookup"><span data-stu-id="ea443-198">Next, compute the authentication tag of Enc_GCM (K_E, nonce, "") for AES-256-GCM given nonce = 096 and K_E as above.</span></span>

<span data-ttu-id="ea443-199">Ergebnis: E7DCCE66DF855A323A6BB7BD7A59BE45 =</span><span class="sxs-lookup"><span data-stu-id="ea443-199">result := E7DCCE66DF855A323A6BB7BD7A59BE45</span></span>

<span data-ttu-id="ea443-200">Dadurch wird die vollständige Kontextheader unten generiert:</span><span class="sxs-lookup"><span data-stu-id="ea443-200">This produces the full context header below:</span></span>

<!-- literal_block {"ids": [], "xml:space": "preserve"} -->

```
00 01 00 00 00 20 00 00 00 0C 00 00 00 10 00 00
   00 10 E7 DC CE 66 DF 85 5A 32 3A 6B B7 BD 7A 59
   BE 45
   ```

<span data-ttu-id="ea443-201">Die Komponenten aufgliedern wie folgt:</span><span class="sxs-lookup"><span data-stu-id="ea443-201">The components break down as follows:</span></span>

   * <span data-ttu-id="ea443-202">die Markierung (00 01)</span><span class="sxs-lookup"><span data-stu-id="ea443-202">the marker (00 01)</span></span>

   * <span data-ttu-id="ea443-203">Block Cipher Schlüssellänge (00-00-00-20)</span><span class="sxs-lookup"><span data-stu-id="ea443-203">the block cipher key length (00 00 00 20)</span></span>

   * <span data-ttu-id="ea443-204">die Größe des Nonce (00 00 00 0 C)</span><span class="sxs-lookup"><span data-stu-id="ea443-204">the nonce size (00 00 00 0C)</span></span>

   * <span data-ttu-id="ea443-205">die Blockgröße für das "Cipher Block" (00-00-00-10)</span><span class="sxs-lookup"><span data-stu-id="ea443-205">the block cipher block size (00 00 00 10)</span></span>

   * <span data-ttu-id="ea443-206">die Authentifizierung Tag Größe (00 00 00 10) und</span><span class="sxs-lookup"><span data-stu-id="ea443-206">the authentication tag size (00 00 00 10) and</span></span>

   * <span data-ttu-id="ea443-207">die authentifizierungstag von der Ausführung der Blockchiffre (E7 DC - End).</span><span class="sxs-lookup"><span data-stu-id="ea443-207">the authentication tag from running the block cipher (E7 DC - end).</span></span>
