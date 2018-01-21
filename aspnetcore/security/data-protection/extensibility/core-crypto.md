---
title: Core-kryptografieerweiterbarkeit
author: rick-anderson
description: "Erläutert das IAuthenticatedEncryptor, IAuthenticatedEncryptorDescriptor, IAuthenticatedEncryptorDescriptorDeserializer sowie der obersten Ebene Factory."
ms.author: riande
manager: wpickett
ms.date: 8/11/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/extensibility/core-crypto
ms.openlocfilehash: b82c30fe40c4badc74645dafa9f0d13f6ffae031
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/19/2018
---
# <a name="core-cryptography-extensibility"></a><span data-ttu-id="b52c2-103">Core-kryptografieerweiterbarkeit</span><span class="sxs-lookup"><span data-stu-id="b52c2-103">Core cryptography extensibility</span></span>

<a name="data-protection-extensibility-core-crypto"></a>

>[!WARNING]
> <span data-ttu-id="b52c2-104">Typen, die jede der folgenden Schnittstellen implementieren, sollten threadsichere werden für mehrere Aufrufer.</span><span class="sxs-lookup"><span data-stu-id="b52c2-104">Types that implement any of the following interfaces should be thread-safe for multiple callers.</span></span>

<a name="data-protection-extensibility-core-crypto-iauthenticatedencryptor"></a>

## <a name="iauthenticatedencryptor"></a><span data-ttu-id="b52c2-105">IAuthenticatedEncryptor</span><span class="sxs-lookup"><span data-stu-id="b52c2-105">IAuthenticatedEncryptor</span></span>

<span data-ttu-id="b52c2-106">Die **IAuthenticatedEncryptor** Schnittstelle ist der grundlegende Baustein von kryptografischen Subsystem.</span><span class="sxs-lookup"><span data-stu-id="b52c2-106">The **IAuthenticatedEncryptor** interface is the basic building block of the cryptographic subsystem.</span></span> <span data-ttu-id="b52c2-107">Im Allgemeinen eine IAuthenticatedEncryptor pro Schlüssel vorhanden ist, und die Instanz IAuthenticatedEncryptor dient als Wrapper für alle kryptografischen schlüsselmaterialien und algorithmische Informationen, die erforderlich sind, kryptografische Vorgänge auszuführen.</span><span class="sxs-lookup"><span data-stu-id="b52c2-107">There is generally one IAuthenticatedEncryptor per key, and the IAuthenticatedEncryptor instance wraps all cryptographic key material and algorithmic information necessary to perform cryptographic operations.</span></span>

<span data-ttu-id="b52c2-108">Wie der Name andeutet, ist der Typ für die Bereitstellung des authentifizierten Ver- und Entschlüsselung Services verantwortlich.</span><span class="sxs-lookup"><span data-stu-id="b52c2-108">As its name suggests, the type is responsible for providing authenticated encryption and decryption services.</span></span> <span data-ttu-id="b52c2-109">Er stellt die folgenden zwei APIs bereit.</span><span class="sxs-lookup"><span data-stu-id="b52c2-109">It exposes the following two APIs.</span></span>

* <span data-ttu-id="b52c2-110">Entschlüsseln (ArraySegment<byte> Chiffretext ArraySegment<byte> AdditionalAuthenticatedData): Byte]</span><span class="sxs-lookup"><span data-stu-id="b52c2-110">Decrypt(ArraySegment<byte> ciphertext, ArraySegment<byte> additionalAuthenticatedData) : byte[]</span></span>

* <span data-ttu-id="b52c2-111">Verschlüsseln (ArraySegment<byte> als nur-Text ArraySegment<byte> AdditionalAuthenticatedData): Byte]</span><span class="sxs-lookup"><span data-stu-id="b52c2-111">Encrypt(ArraySegment<byte> plaintext, ArraySegment<byte> additionalAuthenticatedData) : byte[]</span></span>

<span data-ttu-id="b52c2-112">Die Encrypt-Methode gibt ein Blob, das die enciphered nur-Text und ein authentifizierungstag enthält.</span><span class="sxs-lookup"><span data-stu-id="b52c2-112">The Encrypt method returns a blob that includes the enciphered plaintext and an authentication tag.</span></span> <span data-ttu-id="b52c2-113">Die authentifizierungstag muss die zusätzlichen authentifizierten Daten (AAD) umfassen, obwohl das AAD selbst nicht über die letzte Nutzlast wiederhergestellt werden muss.</span><span class="sxs-lookup"><span data-stu-id="b52c2-113">The authentication tag must encompass the additional authenticated data (AAD), though the AAD itself need not be recoverable from the final payload.</span></span> <span data-ttu-id="b52c2-114">Die Decrypt-Methode überprüft die authentifizierungstag und gibt die deciphered Nutzlast.</span><span class="sxs-lookup"><span data-stu-id="b52c2-114">The Decrypt method validates the authentication tag and returns the deciphered payload.</span></span> <span data-ttu-id="b52c2-115">Alle Fehler auf (mit Ausnahme von ArgumentNullException und ähnliche) soll um CryptographicException antibiotischem.</span><span class="sxs-lookup"><span data-stu-id="b52c2-115">All failures (except ArgumentNullException and similar) should be homogenized to CryptographicException.</span></span>

> [!NOTE]
> <span data-ttu-id="b52c2-116">Die IAuthenticatedEncryptor-Instanz selbst muss nicht tatsächlich das Schlüsselmaterial enthalten.</span><span class="sxs-lookup"><span data-stu-id="b52c2-116">The IAuthenticatedEncryptor instance itself doesn't actually need to contain the key material.</span></span> <span data-ttu-id="b52c2-117">Beispielsweise könnte die Implementierung in ein HSM für alle Vorgänge delegieren.</span><span class="sxs-lookup"><span data-stu-id="b52c2-117">For example, the implementation could delegate to an HSM for all operations.</span></span>

<a name="data-protection-extensibility-core-crypto-iauthenticatedencryptorfactory"></a>
<a name="data-protection-extensibility-core-crypto-iauthenticatedencryptordescriptor"></a>

## <a name="how-to-create-an-iauthenticatedencryptor"></a><span data-ttu-id="b52c2-118">Vorgehensweise: Erstellen einer IAuthenticatedEncryptor</span><span class="sxs-lookup"><span data-stu-id="b52c2-118">How to create an IAuthenticatedEncryptor</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="b52c2-119">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="b52c2-119">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="b52c2-120">Die **IAuthenticatedEncryptorFactory** Schnittstelle darstellt, einen Typ, der weiß, wie zum Erstellen einer [IAuthenticatedEncryptor](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptor) Instanz.</span><span class="sxs-lookup"><span data-stu-id="b52c2-120">The **IAuthenticatedEncryptorFactory** interface represents a type that knows how to create an [IAuthenticatedEncryptor](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptor) instance.</span></span> <span data-ttu-id="b52c2-121">Seine API lautet wie folgt.</span><span class="sxs-lookup"><span data-stu-id="b52c2-121">Its API is as follows.</span></span>

* <span data-ttu-id="b52c2-122">CreateEncryptorInstance (IKey Key): IAuthenticatedEncryptor</span><span class="sxs-lookup"><span data-stu-id="b52c2-122">CreateEncryptorInstance(IKey key) : IAuthenticatedEncryptor</span></span>

<span data-ttu-id="b52c2-123">Für eine bestimmte Instanz IKey alle authentifizierten Verschlüsselungsprogramme mit seiner CreateEncryptorInstance-Methode erstellte sollte als äquivalent betrachtet, wie in der im folgenden Codebeispiel.</span><span class="sxs-lookup"><span data-stu-id="b52c2-123">For any given IKey instance, any authenticated encryptors created by its CreateEncryptorInstance method should be considered equivalent, as in the below code sample.</span></span>

```csharp
// we have an IAuthenticatedEncryptorFactory instance and an IKey instance
IAuthenticatedEncryptorFactory factory = ...;
IKey key = ...;

// get an encryptor instance and perform an authenticated encryption operation
ArraySegment<byte> plaintext = new ArraySegment<byte>(Encoding.UTF8.GetBytes("plaintext"));
ArraySegment<byte> aad = new ArraySegment<byte>(Encoding.UTF8.GetBytes("AAD"));
var encryptor1 = factory.CreateEncryptorInstance(key);
byte[] ciphertext = encryptor1.Encrypt(plaintext, aad);

// get another encryptor instance and perform an authenticated decryption operation
var encryptor2 = factory.CreateEncryptorInstance(key);
byte[] roundTripped = encryptor2.Decrypt(new ArraySegment<byte>(ciphertext), aad);


// the 'roundTripped' and 'plaintext' buffers should be equivalent
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="b52c2-124">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="b52c2-124">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="b52c2-125">Die **IAuthenticatedEncryptorDescriptor** Schnittstelle darstellt, einen Typ, der weiß, wie zum Erstellen einer [IAuthenticatedEncryptor](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptor) Instanz.</span><span class="sxs-lookup"><span data-stu-id="b52c2-125">The **IAuthenticatedEncryptorDescriptor** interface represents a type that knows how to create an [IAuthenticatedEncryptor](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptor) instance.</span></span> <span data-ttu-id="b52c2-126">Seine API lautet wie folgt.</span><span class="sxs-lookup"><span data-stu-id="b52c2-126">Its API is as follows.</span></span>

* <span data-ttu-id="b52c2-127">CreateEncryptorInstance(): IAuthenticatedEncryptor</span><span class="sxs-lookup"><span data-stu-id="b52c2-127">CreateEncryptorInstance() : IAuthenticatedEncryptor</span></span>

* <span data-ttu-id="b52c2-128">ExportToXml() : XmlSerializedDescriptorInfo</span><span class="sxs-lookup"><span data-stu-id="b52c2-128">ExportToXml() : XmlSerializedDescriptorInfo</span></span>

<span data-ttu-id="b52c2-129">Wie IAuthenticatedEncryptor wird eine Instanz von IAuthenticatedEncryptorDescriptor angenommen, um einen bestimmten Schlüssel zu umschließen.</span><span class="sxs-lookup"><span data-stu-id="b52c2-129">Like IAuthenticatedEncryptor, an instance of IAuthenticatedEncryptorDescriptor is assumed to wrap one specific key.</span></span> <span data-ttu-id="b52c2-130">Dies bedeutet, dass für eine bestimmte Instanz IAuthenticatedEncryptorDescriptor alle authentifizierten Verschlüsselungsprogramme mit seiner CreateEncryptorInstance-Methode erstellte entspricht, wie in einzustufen ist die im folgenden Codebeispiel.</span><span class="sxs-lookup"><span data-stu-id="b52c2-130">This means that for any given IAuthenticatedEncryptorDescriptor instance, any authenticated encryptors created by its CreateEncryptorInstance method should be considered equivalent, as in the below code sample.</span></span>

```csharp
// we have an IAuthenticatedEncryptorDescriptor instance
IAuthenticatedEncryptorDescriptor descriptor = ...;

// get an encryptor instance and perform an authenticated encryption operation
ArraySegment<byte> plaintext = new ArraySegment<byte>(Encoding.UTF8.GetBytes("plaintext"));
ArraySegment<byte> aad = new ArraySegment<byte>(Encoding.UTF8.GetBytes("AAD"));
var encryptor1 = descriptor.CreateEncryptorInstance();
byte[] ciphertext = encryptor1.Encrypt(plaintext, aad);

// get another encryptor instance and perform an authenticated decryption operation
var encryptor2 = descriptor.CreateEncryptorInstance();
byte[] roundTripped = encryptor2.Decrypt(new ArraySegment<byte>(ciphertext), aad);


// the 'roundTripped' and 'plaintext' buffers should be equivalent
```

---

<a name="data-protection-extensibility-core-crypto-iauthenticatedencryptordescriptor"></a>

## <a name="iauthenticatedencryptordescriptor-aspnet-core-2x-only"></a><span data-ttu-id="b52c2-131">IAuthenticatedEncryptorDescriptor (ASP.NET Core 2.x only)</span><span class="sxs-lookup"><span data-stu-id="b52c2-131">IAuthenticatedEncryptorDescriptor (ASP.NET Core 2.x only)</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="b52c2-132">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="b52c2-132">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="b52c2-133">Die **IAuthenticatedEncryptorDescriptor** Schnittstelle darstellt, einen Typ, der weiß, wie Sie selbst in XML exportieren.</span><span class="sxs-lookup"><span data-stu-id="b52c2-133">The **IAuthenticatedEncryptorDescriptor** interface represents a type that knows how to export itself to XML.</span></span> <span data-ttu-id="b52c2-134">Seine API lautet wie folgt.</span><span class="sxs-lookup"><span data-stu-id="b52c2-134">Its API is as follows.</span></span>

* <span data-ttu-id="b52c2-135">ExportToXml() : XmlSerializedDescriptorInfo</span><span class="sxs-lookup"><span data-stu-id="b52c2-135">ExportToXml() : XmlSerializedDescriptorInfo</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="b52c2-136">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="b52c2-136">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

---

## <a name="xml-serialization"></a><span data-ttu-id="b52c2-137">XML-Serialisierung</span><span class="sxs-lookup"><span data-stu-id="b52c2-137">XML Serialization</span></span>

<span data-ttu-id="b52c2-138">Der Hauptunterschied zwischen IAuthenticatedEncryptor und IAuthenticatedEncryptorDescriptor ist, dass der Deskriptor weiß, wie die Verschlüsselung zu erstellen und versorgt sie mit gültigen Argumente.</span><span class="sxs-lookup"><span data-stu-id="b52c2-138">The primary difference between IAuthenticatedEncryptor and IAuthenticatedEncryptorDescriptor is that the descriptor knows how to create the encryptor and supply it with valid arguments.</span></span> <span data-ttu-id="b52c2-139">Erwägen Sie eine IAuthenticatedEncryptor, deren Implementierung SymmetricAlgorithm und KeyedHashAlgorithm abhängt.</span><span class="sxs-lookup"><span data-stu-id="b52c2-139">Consider an IAuthenticatedEncryptor whose implementation relies on SymmetricAlgorithm and KeyedHashAlgorithm.</span></span> <span data-ttu-id="b52c2-140">Die Verschlüsselung der Auftrag ist, um diese Typen zu nutzen, aber es unbedingt weiß nicht, in denen diese Typen stammen, damit sie wirklich nicht ordnungsgemäße beschrieben, wie sich selbst neu erstellen schreiben kann, wenn die Anwendung neu gestartet wird.</span><span class="sxs-lookup"><span data-stu-id="b52c2-140">The encryptor's job is to consume these types, but it doesn't necessarily know where these types came from, so it can't really write out a proper description of how to recreate itself if the application restarts.</span></span> <span data-ttu-id="b52c2-141">Der Deskriptor fungiert als eine höhere zusätzlich.</span><span class="sxs-lookup"><span data-stu-id="b52c2-141">The descriptor acts as a higher level on top of this.</span></span> <span data-ttu-id="b52c2-142">Da der Deskriptor weiß, wie die Verschlüsselung-Instanz zu erstellen (z. B. er kennt die Algorithmen zu erstellen), damit die Verschlüsselung-Instanz neu erstellt werden kann, nach dem Zurücksetzen auf einer Anwendung kann dieses Wissen im XML-Format serialisieren.</span><span class="sxs-lookup"><span data-stu-id="b52c2-142">Since the descriptor knows how to create the encryptor instance (e.g., it knows how to create the required algorithms), it can serialize that knowledge in XML form so that the encryptor instance can be recreated after an application reset.</span></span>

<a name="data-protection-extensibility-core-crypto-exporttoxml"></a>

<span data-ttu-id="b52c2-143">Der Deskriptor kann über seine ExportToXml Routine serialisiert werden.</span><span class="sxs-lookup"><span data-stu-id="b52c2-143">The descriptor can be serialized via its ExportToXml routine.</span></span> <span data-ttu-id="b52c2-144">Diese Routine gibt eine XmlSerializedDescriptorInfo enthält zwei Eigenschaften: die "XElement"-Darstellung des Deskriptors und den Typ der darstellt ein [IAuthenticatedEncryptorDescriptorDeserializer](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptordescriptordeserializer) erfolgen kann zum Aufrufen dieses Deskriptors erhält die entsprechenden "XElement" verwendet.</span><span class="sxs-lookup"><span data-stu-id="b52c2-144">This routine returns an XmlSerializedDescriptorInfo which contains two properties: the XElement representation of the descriptor and the Type which represents an [IAuthenticatedEncryptorDescriptorDeserializer](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptordescriptordeserializer) which can be used to resurrect this descriptor given the corresponding XElement.</span></span>

<span data-ttu-id="b52c2-145">Der serialisierte Deskriptor enthalten möglicherweise vertrauliche Informationen wie z. B. kryptografischen schlüsselmaterialien.</span><span class="sxs-lookup"><span data-stu-id="b52c2-145">The serialized descriptor may contain sensitive information such as cryptographic key material.</span></span> <span data-ttu-id="b52c2-146">Die Datenschutzsystem verfügt über integrierte Unterstützung für die Verschlüsselung von Informationen, bevor sie in den Speicher beibehalten wurde.</span><span class="sxs-lookup"><span data-stu-id="b52c2-146">The data protection system has built-in support for encrypting information before it's persisted to storage.</span></span> <span data-ttu-id="b52c2-147">Davon profitieren, kennzeichnen der Deskriptor das Element, das sensible Informationen mit den Namen des Attributs ""RequiresEncryption "" (Xmlns "http://schemas.asp.net/2015/03/dataProtection"), Wert "True" enthält.</span><span class="sxs-lookup"><span data-stu-id="b52c2-147">To take advantage of this, the descriptor should mark the element which contains sensitive information with the attribute name "requiresEncryption" (xmlns "http://schemas.asp.net/2015/03/dataProtection"), value "true".</span></span>

>[!TIP]
> <span data-ttu-id="b52c2-148">Für das Festlegen dieses Attributs ist ein Hilfs-API.</span><span class="sxs-lookup"><span data-stu-id="b52c2-148">There's a helper API for setting this attribute.</span></span> <span data-ttu-id="b52c2-149">Rufen Sie die Erweiterungsmethode XElement.MarkAsRequiresEncryption() im Namespace Microsoft.AspNetCore.DataProtection.AuthenticatedEncryption.ConfigurationModel befindet.</span><span class="sxs-lookup"><span data-stu-id="b52c2-149">Call the extension method XElement.MarkAsRequiresEncryption() located in namespace Microsoft.AspNetCore.DataProtection.AuthenticatedEncryption.ConfigurationModel.</span></span>

<span data-ttu-id="b52c2-150">Es kann auch Fälle, in denen der serialisierte Deskriptor vertrauliche Informationen enthalten, nicht, vorhanden sein.</span><span class="sxs-lookup"><span data-stu-id="b52c2-150">There can also be cases where the serialized descriptor doesn't contain sensitive information.</span></span> <span data-ttu-id="b52c2-151">Schauen wir uns erneut die Groß-/Kleinschreibung ein kryptografischer Schlüssel in einem HSM gespeichert.</span><span class="sxs-lookup"><span data-stu-id="b52c2-151">Consider again the case of a cryptographic key stored in an HSM.</span></span> <span data-ttu-id="b52c2-152">Der Deskriptor kann nicht das Schlüsselmaterial schreiben beim selbst zu serialisieren, da das HSM nicht das Material in Klartext verfügbar macht.</span><span class="sxs-lookup"><span data-stu-id="b52c2-152">The descriptor cannot write out the key material when serializing itself since the HSM will not expose the material in plaintext form.</span></span> <span data-ttu-id="b52c2-153">Stattdessen kann der Deskriptor schreiben, die Schlüssel eingebunden Version des Schlüssels (falls das HSM Export auf diese Weise kann) oder den HSMs-Bezeichner für den Schlüssel.</span><span class="sxs-lookup"><span data-stu-id="b52c2-153">Instead, the descriptor might write out the key-wrapped version of the key (if the HSM allows export in this fashion) or the HSM's own unique identifier for the key.</span></span>

<a name="data-protection-extensibility-core-crypto-iauthenticatedencryptordescriptordeserializer"></a>

## <a name="iauthenticatedencryptordescriptordeserializer"></a><span data-ttu-id="b52c2-154">IAuthenticatedEncryptorDescriptorDeserializer</span><span class="sxs-lookup"><span data-stu-id="b52c2-154">IAuthenticatedEncryptorDescriptorDeserializer</span></span>

<span data-ttu-id="b52c2-155">Die **IAuthenticatedEncryptorDescriptorDeserializer** Schnittstelle darstellt, einen Typ, der weiß, wie eine Instanz eines "XElement" IAuthenticatedEncryptorDescriptor zu deserialisieren.</span><span class="sxs-lookup"><span data-stu-id="b52c2-155">The **IAuthenticatedEncryptorDescriptorDeserializer** interface represents a type that knows how to deserialize an IAuthenticatedEncryptorDescriptor instance from an XElement.</span></span> <span data-ttu-id="b52c2-156">Macht eine einzelne Methode:</span><span class="sxs-lookup"><span data-stu-id="b52c2-156">It exposes a single method:</span></span>

* <span data-ttu-id="b52c2-157">ImportFromXml ("XElement"-Element): IAuthenticatedEncryptorDescriptor</span><span class="sxs-lookup"><span data-stu-id="b52c2-157">ImportFromXml(XElement element) : IAuthenticatedEncryptorDescriptor</span></span>

<span data-ttu-id="b52c2-158">Die ImportFromXml-Methode verwendet die "XElement", der von zurückgegeben [IAuthenticatedEncryptorDescriptor.ExportToXml](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-exporttoxml) und entspricht der ursprünglichen IAuthenticatedEncryptorDescriptor erstellt.</span><span class="sxs-lookup"><span data-stu-id="b52c2-158">The ImportFromXml method takes the XElement that was returned by [IAuthenticatedEncryptorDescriptor.ExportToXml](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-exporttoxml) and creates an equivalent of the original IAuthenticatedEncryptorDescriptor.</span></span>

<span data-ttu-id="b52c2-159">Typen, die IAuthenticatedEncryptorDescriptorDeserializer implementieren müssen eine der folgenden zwei öffentlichen Konstruktoren aufweisen:</span><span class="sxs-lookup"><span data-stu-id="b52c2-159">Types which implement IAuthenticatedEncryptorDescriptorDeserializer should have one of the following two public constructors:</span></span>

* <span data-ttu-id="b52c2-160">.ctor(IServiceProvider)</span><span class="sxs-lookup"><span data-stu-id="b52c2-160">.ctor(IServiceProvider)</span></span>

* <span data-ttu-id="b52c2-161">.ctor()</span><span class="sxs-lookup"><span data-stu-id="b52c2-161">.ctor()</span></span>

> [!NOTE]
> <span data-ttu-id="b52c2-162">Der an den Konstruktor übergebene IServiceProvider kann null sein.</span><span class="sxs-lookup"><span data-stu-id="b52c2-162">The IServiceProvider passed to the constructor may be null.</span></span>

## <a name="the-top-level-factory"></a><span data-ttu-id="b52c2-163">Die Factory auf oberster Ebene</span><span class="sxs-lookup"><span data-stu-id="b52c2-163">The top-level factory</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="b52c2-164">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="b52c2-164">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="b52c2-165">Die **AlgorithmConfiguration** Klasse stellt einen Typ, der weiß, wie erstellen [IAuthenticatedEncryptorDescriptor](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptordescriptor) Instanzen.</span><span class="sxs-lookup"><span data-stu-id="b52c2-165">The **AlgorithmConfiguration** class represents a type which knows how to create [IAuthenticatedEncryptorDescriptor](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptordescriptor) instances.</span></span> <span data-ttu-id="b52c2-166">Er stellt eine einzelne API bereit.</span><span class="sxs-lookup"><span data-stu-id="b52c2-166">It exposes a single API.</span></span>

* <span data-ttu-id="b52c2-167">CreateNewDescriptor(): IAuthenticatedEncryptorDescriptor</span><span class="sxs-lookup"><span data-stu-id="b52c2-167">CreateNewDescriptor() : IAuthenticatedEncryptorDescriptor</span></span>

<span data-ttu-id="b52c2-168">Sichereres AlgorithmConfiguration der obersten Ebene Factory.</span><span class="sxs-lookup"><span data-stu-id="b52c2-168">Think of AlgorithmConfiguration as the top-level factory.</span></span> <span data-ttu-id="b52c2-169">Die Konfiguration dient als Vorlage.</span><span class="sxs-lookup"><span data-stu-id="b52c2-169">The configuration serves as a template.</span></span> <span data-ttu-id="b52c2-170">Es dient als Wrapper für algorithmische Informationen (z. B. diese Konfiguration erzeugt Deskriptoren mit einem Hauptschlüssel AES-128-GCM), aber es ist noch nicht mit einem bestimmten Schlüssel zugeordnete.</span><span class="sxs-lookup"><span data-stu-id="b52c2-170">It wraps algorithmic information (e.g., this configuration produces descriptors with an AES-128-GCM master key), but it is not yet associated with a specific key.</span></span>

<span data-ttu-id="b52c2-171">Wenn CreateNewDescriptor aufgerufen wird, neue Schlüsselmaterial wird ausschließlich für diesen Aufruf erstellt, und eine neue IAuthenticatedEncryptorDescriptor erstellt umschließt die dieser Schlüsselmaterial sowie die algorithmische Informationen erforderlich, um das Material zu verwenden.</span><span class="sxs-lookup"><span data-stu-id="b52c2-171">When CreateNewDescriptor is called, fresh key material is created solely for this call, and a new IAuthenticatedEncryptorDescriptor is produced which wraps this key material and the algorithmic information required to consume the material.</span></span> <span data-ttu-id="b52c2-172">Das Schlüsselmaterial konnte in der Software erstellt (und im Speicher gehalten werden), erstellt und innerhalb einer HSM usw. werden konnte.</span><span class="sxs-lookup"><span data-stu-id="b52c2-172">The key material could be created in software (and held in memory), it could be created and held within an HSM, and so on.</span></span> <span data-ttu-id="b52c2-173">Der wichtige Punkt ist, dass zwei Aufrufe CreateNewDescriptor nie entsprechende IAuthenticatedEncryptorDescriptor-Instanzen erstellen soll.</span><span class="sxs-lookup"><span data-stu-id="b52c2-173">The crucial point is that any two calls to CreateNewDescriptor should never create equivalent IAuthenticatedEncryptorDescriptor instances.</span></span>

<span data-ttu-id="b52c2-174">Der Typ AlgorithmConfiguration fungiert als Einstiegspunkt für die schlüsselerstellung Routinen wie z. B. [automatische Schlüssel parallelen](../implementation/key-management.md#key-expiration-and-rolling).</span><span class="sxs-lookup"><span data-stu-id="b52c2-174">The AlgorithmConfiguration type serves as the entry point for key creation routines such as [automatic key rolling](../implementation/key-management.md#key-expiration-and-rolling).</span></span> <span data-ttu-id="b52c2-175">Um die Implementierung für alle zukünftigen Schlüssel ändern zu können, legen Sie die Eigenschaft AuthenticatedEncryptorConfiguration in KeyManagementOptions.</span><span class="sxs-lookup"><span data-stu-id="b52c2-175">To change the implementation for all future keys, set the AuthenticatedEncryptorConfiguration property in KeyManagementOptions.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="b52c2-176">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="b52c2-176">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="b52c2-177">Die **IAuthenticatedEncryptorConfiguration** Schnittstelle darstellt, einen Typ, der weiß, wie erstellen [IAuthenticatedEncryptorDescriptor](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptordescriptor) Instanzen.</span><span class="sxs-lookup"><span data-stu-id="b52c2-177">The **IAuthenticatedEncryptorConfiguration** interface represents a type which knows how to create [IAuthenticatedEncryptorDescriptor](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptordescriptor) instances.</span></span> <span data-ttu-id="b52c2-178">Er stellt eine einzelne API bereit.</span><span class="sxs-lookup"><span data-stu-id="b52c2-178">It exposes a single API.</span></span>

* <span data-ttu-id="b52c2-179">CreateNewDescriptor(): IAuthenticatedEncryptorDescriptor</span><span class="sxs-lookup"><span data-stu-id="b52c2-179">CreateNewDescriptor() : IAuthenticatedEncryptorDescriptor</span></span>

<span data-ttu-id="b52c2-180">Sichereres IAuthenticatedEncryptorConfiguration der obersten Ebene Factory.</span><span class="sxs-lookup"><span data-stu-id="b52c2-180">Think of IAuthenticatedEncryptorConfiguration as the top-level factory.</span></span> <span data-ttu-id="b52c2-181">Die Konfiguration dient als Vorlage.</span><span class="sxs-lookup"><span data-stu-id="b52c2-181">The configuration serves as a template.</span></span> <span data-ttu-id="b52c2-182">Es dient als Wrapper für algorithmische Informationen (z. B. diese Konfiguration erzeugt Deskriptoren mit einem Hauptschlüssel AES-128-GCM), aber es ist noch nicht mit einem bestimmten Schlüssel zugeordnete.</span><span class="sxs-lookup"><span data-stu-id="b52c2-182">It wraps algorithmic information (e.g., this configuration produces descriptors with an AES-128-GCM master key), but it is not yet associated with a specific key.</span></span>

<span data-ttu-id="b52c2-183">Wenn CreateNewDescriptor aufgerufen wird, neue Schlüsselmaterial wird ausschließlich für diesen Aufruf erstellt, und eine neue IAuthenticatedEncryptorDescriptor erstellt umschließt die dieser Schlüsselmaterial sowie die algorithmische Informationen erforderlich, um das Material zu verwenden.</span><span class="sxs-lookup"><span data-stu-id="b52c2-183">When CreateNewDescriptor is called, fresh key material is created solely for this call, and a new IAuthenticatedEncryptorDescriptor is produced which wraps this key material and the algorithmic information required to consume the material.</span></span> <span data-ttu-id="b52c2-184">Das Schlüsselmaterial konnte in der Software erstellt (und im Speicher gehalten werden), erstellt und innerhalb einer HSM usw. werden konnte.</span><span class="sxs-lookup"><span data-stu-id="b52c2-184">The key material could be created in software (and held in memory), it could be created and held within an HSM, and so on.</span></span> <span data-ttu-id="b52c2-185">Der wichtige Punkt ist, dass zwei Aufrufe CreateNewDescriptor nie entsprechende IAuthenticatedEncryptorDescriptor-Instanzen erstellen soll.</span><span class="sxs-lookup"><span data-stu-id="b52c2-185">The crucial point is that any two calls to CreateNewDescriptor should never create equivalent IAuthenticatedEncryptorDescriptor instances.</span></span>

<span data-ttu-id="b52c2-186">Der Typ IAuthenticatedEncryptorConfiguration fungiert als Einstiegspunkt für die schlüsselerstellung Routinen wie z. B. [automatische Schlüssel parallelen](../implementation/key-management.md#key-expiration-and-rolling).</span><span class="sxs-lookup"><span data-stu-id="b52c2-186">The IAuthenticatedEncryptorConfiguration type serves as the entry point for key creation routines such as [automatic key rolling](../implementation/key-management.md#key-expiration-and-rolling).</span></span> <span data-ttu-id="b52c2-187">Um die Implementierung für alle zukünftigen Schlüssel zu ändern, registrieren Sie einen Singleton-IAuthenticatedEncryptorConfiguration im Dienstcontainer.</span><span class="sxs-lookup"><span data-stu-id="b52c2-187">To change the implementation for all future keys, register a singleton IAuthenticatedEncryptorConfiguration in the service container.</span></span>

---
