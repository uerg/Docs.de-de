---
title: "Schlüsselverwaltungsdienst-Erweiterbarkeit"
author: rick-anderson
description: "Dieses Dokument beschreibt ASP.NET Core Data Protection schlüsselverwaltung Erweiterbarkeit."
ms.author: riande
manager: wpickett
ms.date: 11/22/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/extensibility/key-management
ms.openlocfilehash: 2bd0f945306a7acd6252c2a00aafb648cf722616
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/19/2018
---
# <a name="key-management-extensibility"></a><span data-ttu-id="54ba5-103">Schlüsselverwaltungsdienst-Erweiterbarkeit</span><span class="sxs-lookup"><span data-stu-id="54ba5-103">Key management extensibility</span></span>

<a name="data-protection-extensibility-key-management"></a>

>[!TIP]
> <span data-ttu-id="54ba5-104">Lesen der [schlüsselverwaltung](../implementation/key-management.md#data-protection-implementation-key-management) Abschnitt vor dem Lesen in diesem Abschnitt, wie einige der grundlegenden Konzepte hinter dieser APIs erläutert wird.</span><span class="sxs-lookup"><span data-stu-id="54ba5-104">Read the [key management](../implementation/key-management.md#data-protection-implementation-key-management) section before reading this section, as it explains some of the fundamental concepts behind these APIs.</span></span>

>[!WARNING]
> <span data-ttu-id="54ba5-105">Typen, die jede der folgenden Schnittstellen implementieren, sollten threadsichere werden für mehrere Aufrufer.</span><span class="sxs-lookup"><span data-stu-id="54ba5-105">Types that implement any of the following interfaces should be thread-safe for multiple callers.</span></span>

## <a name="key"></a><span data-ttu-id="54ba5-106">Key</span><span class="sxs-lookup"><span data-stu-id="54ba5-106">Key</span></span>

<span data-ttu-id="54ba5-107">Die `IKey` Schnittstelle ist die grundlegende Darstellung eines Schlüssels in Kryptografiesystem.</span><span class="sxs-lookup"><span data-stu-id="54ba5-107">The `IKey` interface is the basic representation of a key in cryptosystem.</span></span> <span data-ttu-id="54ba5-108">Der Begriff Schlüssel ist hier insofern abstrakte, nicht in der struktureller von "kryptografischen schlüsselmaterialien" verwendet.</span><span class="sxs-lookup"><span data-stu-id="54ba5-108">The term key is used here in the abstract sense, not in the literal sense of "cryptographic key material".</span></span> <span data-ttu-id="54ba5-109">Ein Schlüssel hat die folgenden Eigenschaften:</span><span class="sxs-lookup"><span data-stu-id="54ba5-109">A key has the following properties:</span></span>

* <span data-ttu-id="54ba5-110">Aktivierung, Erstellung und Ablaufdaten</span><span class="sxs-lookup"><span data-stu-id="54ba5-110">Activation, creation, and expiration dates</span></span>

* <span data-ttu-id="54ba5-111">Status der Zertifikatsperre</span><span class="sxs-lookup"><span data-stu-id="54ba5-111">Revocation status</span></span>

* <span data-ttu-id="54ba5-112">Schlüsselbezeichner (eine GUID)</span><span class="sxs-lookup"><span data-stu-id="54ba5-112">Key identifier (a GUID)</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="54ba5-113">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="54ba5-113">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="54ba5-114">Darüber hinaus `IKey` macht eine `CreateEncryptor` Methode, die verwendet werden kann, erstellen eine [IAuthenticatedEncryptor](core-crypto.md#data-protection-extensibility-core-crypto-iauthenticatedencryptor) Instanz gebunden werden, um diesen Schlüssel.</span><span class="sxs-lookup"><span data-stu-id="54ba5-114">Additionally, `IKey` exposes a `CreateEncryptor` method which can be used to create an [IAuthenticatedEncryptor](core-crypto.md#data-protection-extensibility-core-crypto-iauthenticatedencryptor) instance tied to this key.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="54ba5-115">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="54ba5-115">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="54ba5-116">Darüber hinaus `IKey` macht eine `CreateEncryptorInstance` Methode, die verwendet werden kann, erstellen eine [IAuthenticatedEncryptor](core-crypto.md#data-protection-extensibility-core-crypto-iauthenticatedencryptor) Instanz gebunden werden, um diesen Schlüssel.</span><span class="sxs-lookup"><span data-stu-id="54ba5-116">Additionally, `IKey` exposes a `CreateEncryptorInstance` method which can be used to create an [IAuthenticatedEncryptor](core-crypto.md#data-protection-extensibility-core-crypto-iauthenticatedencryptor) instance tied to this key.</span></span>

---

> [!NOTE]
> <span data-ttu-id="54ba5-117">Es ist keine API zum Abrufen der kryptografischen Rohmaterialien aus einer `IKey` Instanz.</span><span class="sxs-lookup"><span data-stu-id="54ba5-117">There is no API to retrieve the raw cryptographic material from an `IKey` instance.</span></span>

## <a name="ikeymanager"></a><span data-ttu-id="54ba5-118">IKeyManager</span><span class="sxs-lookup"><span data-stu-id="54ba5-118">IKeyManager</span></span>

<span data-ttu-id="54ba5-119">Die `IKeyManager` Schnittstelle stellt ein Objekt, das verantwortlich für allgemeine Schlüsselspeicher, Abruf und Manipulation dar.</span><span class="sxs-lookup"><span data-stu-id="54ba5-119">The `IKeyManager` interface represents an object responsible for general key storage, retrieval, and manipulation.</span></span> <span data-ttu-id="54ba5-120">Macht drei Vorgänge auf höherer Ebene:</span><span class="sxs-lookup"><span data-stu-id="54ba5-120">It exposes three high-level operations:</span></span>

* <span data-ttu-id="54ba5-121">Erstellen Sie einen neuen Schlüssel, und klicken Sie in Speicher gespeichert.</span><span class="sxs-lookup"><span data-stu-id="54ba5-121">Create a new key and persist it to storage.</span></span>

* <span data-ttu-id="54ba5-122">Alle Schlüssel aus dem Speicher abrufen.</span><span class="sxs-lookup"><span data-stu-id="54ba5-122">Get all keys from storage.</span></span>

* <span data-ttu-id="54ba5-123">Einen oder mehrere Schlüssel widerrufen, und die Sperrinformationen in den Speicher beibehalten.</span><span class="sxs-lookup"><span data-stu-id="54ba5-123">Revoke one or more keys and persist the revocation information to storage.</span></span>

>[!WARNING]
> <span data-ttu-id="54ba5-124">Schreiben einer `IKeyManager` ist eine sehr komplexe Aufgabe und die meisten Entwickler sollten nicht versuchen.</span><span class="sxs-lookup"><span data-stu-id="54ba5-124">Writing an `IKeyManager` is a very advanced task, and the majority of developers should not attempt it.</span></span> <span data-ttu-id="54ba5-125">Stattdessen die meisten Entwickler sollten nutzen die Funktionen von Angeboten die [XmlKeyManager](xref:security/data-protection/extensibility/key-management#data-protection-extensibility-key-management-xmlkeymanager) Klasse.</span><span class="sxs-lookup"><span data-stu-id="54ba5-125">Instead, most developers should take advantage of the facilities offered by the [XmlKeyManager](xref:security/data-protection/extensibility/key-management#data-protection-extensibility-key-management-xmlkeymanager) class.</span></span>

<a name="data-protection-extensibility-key-management-xmlkeymanager"></a>

## <a name="xmlkeymanager"></a><span data-ttu-id="54ba5-126">XmlKeyManager</span><span class="sxs-lookup"><span data-stu-id="54ba5-126">XmlKeyManager</span></span>

<span data-ttu-id="54ba5-127">Die `XmlKeyManager` Typ ist die mitgelieferten konkrete Implementierung der `IKeyManager`.</span><span class="sxs-lookup"><span data-stu-id="54ba5-127">The `XmlKeyManager` type is the in-box concrete implementation of `IKeyManager`.</span></span> <span data-ttu-id="54ba5-128">Er bietet mehrere nützliche Funktionen, einschließlich schlüsselhinterlegung und Verschlüsselung von Schlüsseln im Ruhezustand.</span><span class="sxs-lookup"><span data-stu-id="54ba5-128">It provides several useful facilities, including key escrow and encryption of keys at rest.</span></span> <span data-ttu-id="54ba5-129">Schlüssel in diesem System werden als XML-Elementen dargestellt (insbesondere ["XElement"](https://docs.microsoft.com/dotnet/csharp/programming-guide/concepts/linq/xelement-class-overview)).</span><span class="sxs-lookup"><span data-stu-id="54ba5-129">Keys in this system are represented as XML elements (specifically, [XElement](https://docs.microsoft.com/dotnet/csharp/programming-guide/concepts/linq/xelement-class-overview)).</span></span>

<span data-ttu-id="54ba5-130">`XmlKeyManager`hängt von mehreren anderen Komponenten im Verlauf die Aufgaben erfüllen:</span><span class="sxs-lookup"><span data-stu-id="54ba5-130">`XmlKeyManager` depends on several other components in the course of fulfilling its tasks:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="54ba5-131">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="54ba5-131">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

* <span data-ttu-id="54ba5-132">`AlgorithmConfiguration`, der bestimmt, die durch neue Schlüssel verwendeten Algorithmen.</span><span class="sxs-lookup"><span data-stu-id="54ba5-132">`AlgorithmConfiguration`, which dictates the algorithms used by new keys.</span></span>

* <span data-ttu-id="54ba5-133">`IXmlRepository`, welche steuert, auf dem Schlüssel im Speicher gespeichert werden.</span><span class="sxs-lookup"><span data-stu-id="54ba5-133">`IXmlRepository`, which controls where keys are persisted in storage.</span></span>

* <span data-ttu-id="54ba5-134">`IXmlEncryptor`[optional] die ermöglicht das Verschlüsseln der Schlüssel im Ruhezustand.</span><span class="sxs-lookup"><span data-stu-id="54ba5-134">`IXmlEncryptor` [optional], which allows encrypting keys at rest.</span></span>

* <span data-ttu-id="54ba5-135">`IKeyEscrowSink`[optional] stellt schlüsselhinterlegung Dienste.</span><span class="sxs-lookup"><span data-stu-id="54ba5-135">`IKeyEscrowSink` [optional], which provides key escrow services.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="54ba5-136">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="54ba5-136">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

* <span data-ttu-id="54ba5-137">`IXmlRepository`, welche steuert, auf dem Schlüssel im Speicher gespeichert werden.</span><span class="sxs-lookup"><span data-stu-id="54ba5-137">`IXmlRepository`, which controls where keys are persisted in storage.</span></span>

* <span data-ttu-id="54ba5-138">`IXmlEncryptor`[optional] die ermöglicht das Verschlüsseln der Schlüssel im Ruhezustand.</span><span class="sxs-lookup"><span data-stu-id="54ba5-138">`IXmlEncryptor` [optional], which allows encrypting keys at rest.</span></span>

* <span data-ttu-id="54ba5-139">`IKeyEscrowSink`[optional] stellt schlüsselhinterlegung Dienste.</span><span class="sxs-lookup"><span data-stu-id="54ba5-139">`IKeyEscrowSink` [optional], which provides key escrow services.</span></span>

---

<span data-ttu-id="54ba5-140">Im folgenden finden Sie übersichtliche Diagramme, die angibt, wie diese Komponenten zusammen in sind kabelgebundene `XmlKeyManager`.</span><span class="sxs-lookup"><span data-stu-id="54ba5-140">Below are high-level diagrams which indicate how these components are wired together within `XmlKeyManager`.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="54ba5-141">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="54ba5-141">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

   ![Schlüssel erstellen](key-management/_static/keycreation2.png)

   <span data-ttu-id="54ba5-143">*Schlüssels Erstellung / CreateNewKey*</span><span class="sxs-lookup"><span data-stu-id="54ba5-143">*Key Creation / CreateNewKey*</span></span>

<span data-ttu-id="54ba5-144">In der Implementierung der `CreateNewKey`, `AlgorithmConfiguration` Komponente dient zum Erstellen Sie einen eindeutigen `IAuthenticatedEncryptorDescriptor`, die dann als XML serialisiert wird.</span><span class="sxs-lookup"><span data-stu-id="54ba5-144">In the implementation of `CreateNewKey`, the `AlgorithmConfiguration` component is used to create a unique `IAuthenticatedEncryptorDescriptor`, which is then serialized as XML.</span></span> <span data-ttu-id="54ba5-145">Wenn die Senke schlüsselhinterlegung vorhanden ist, wird der XML-Rohdaten (unverschlüsselt) für die langfristige Speicherung an die Senke bereitgestellt.</span><span class="sxs-lookup"><span data-stu-id="54ba5-145">If a key escrow sink is present, the raw (unencrypted) XML is provided to the sink for long-term storage.</span></span> <span data-ttu-id="54ba5-146">Der nicht verschlüsselte XML-Code wird dann ausgeführt, über eine `IXmlEncryptor` (falls erforderlich) das verschlüsselte XML-Dokument generieren.</span><span class="sxs-lookup"><span data-stu-id="54ba5-146">The unencrypted XML is then run through an `IXmlEncryptor` (if required) to generate the encrypted XML document.</span></span> <span data-ttu-id="54ba5-147">Dieses verschlüsselte Dokument werden beibehalten, in langfristigen Speicher über die `IXmlRepository`.</span><span class="sxs-lookup"><span data-stu-id="54ba5-147">This encrypted document is persisted to long-term storage via the `IXmlRepository`.</span></span> <span data-ttu-id="54ba5-148">(Wenn kein `IXmlEncryptor` wird konfiguriert, wird die unverschlüsselte Dokument im beibehalten der `IXmlRepository`.)</span><span class="sxs-lookup"><span data-stu-id="54ba5-148">(If no `IXmlEncryptor` is configured, the unencrypted document is persisted in the `IXmlRepository`.)</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="54ba5-149">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="54ba5-149">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

   ![Schlüssel erstellen](key-management/_static/keycreation1.png)

   <span data-ttu-id="54ba5-151">*Schlüssels Erstellung / CreateNewKey*</span><span class="sxs-lookup"><span data-stu-id="54ba5-151">*Key Creation / CreateNewKey*</span></span>

<span data-ttu-id="54ba5-152">In der Implementierung der `CreateNewKey`, `IAuthenticatedEncryptorConfiguration` Komponente dient zum Erstellen Sie einen eindeutigen `IAuthenticatedEncryptorDescriptor`, die dann als XML serialisiert wird.</span><span class="sxs-lookup"><span data-stu-id="54ba5-152">In the implementation of `CreateNewKey`, the `IAuthenticatedEncryptorConfiguration` component is used to create a unique `IAuthenticatedEncryptorDescriptor`, which is then serialized as XML.</span></span> <span data-ttu-id="54ba5-153">Wenn die Senke schlüsselhinterlegung vorhanden ist, wird der XML-Rohdaten (unverschlüsselt) für die langfristige Speicherung an die Senke bereitgestellt.</span><span class="sxs-lookup"><span data-stu-id="54ba5-153">If a key escrow sink is present, the raw (unencrypted) XML is provided to the sink for long-term storage.</span></span> <span data-ttu-id="54ba5-154">Der nicht verschlüsselte XML-Code wird dann ausgeführt, über eine `IXmlEncryptor` (falls erforderlich) das verschlüsselte XML-Dokument generieren.</span><span class="sxs-lookup"><span data-stu-id="54ba5-154">The unencrypted XML is then run through an `IXmlEncryptor` (if required) to generate the encrypted XML document.</span></span> <span data-ttu-id="54ba5-155">Dieses verschlüsselte Dokument werden beibehalten, in langfristigen Speicher über die `IXmlRepository`.</span><span class="sxs-lookup"><span data-stu-id="54ba5-155">This encrypted document is persisted to long-term storage via the `IXmlRepository`.</span></span> <span data-ttu-id="54ba5-156">(Wenn kein `IXmlEncryptor` wird konfiguriert, wird die unverschlüsselte Dokument im beibehalten der `IXmlRepository`.)</span><span class="sxs-lookup"><span data-stu-id="54ba5-156">(If no `IXmlEncryptor` is configured, the unencrypted document is persisted in the `IXmlRepository`.)</span></span>

---

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="54ba5-157">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="54ba5-157">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

   ![Abruf der](key-management/_static/keyretrieval2.png)
   
# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="54ba5-159">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="54ba5-159">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

   ![Abruf der](key-management/_static/keyretrieval1.png)

---

   <span data-ttu-id="54ba5-161">*Abrufen von Schlüssel / GetAllKeys*</span><span class="sxs-lookup"><span data-stu-id="54ba5-161">*Key Retrieval / GetAllKeys*</span></span>

<span data-ttu-id="54ba5-162">In der Implementierung der `GetAllKeys`, die XML-Dokumente darstellen Schlüssel und Revocations werden gelesen, aus der zugrunde liegenden `IXmlRepository`.</span><span class="sxs-lookup"><span data-stu-id="54ba5-162">In the implementation of `GetAllKeys`, the XML documents representing keys and revocations are read from the underlying `IXmlRepository`.</span></span> <span data-ttu-id="54ba5-163">Wenn diese Dokumente verschlüsselt sind, wird das System diese automatisch entschlüsselt.</span><span class="sxs-lookup"><span data-stu-id="54ba5-163">If these documents are encrypted, the system will automatically decrypt them.</span></span> <span data-ttu-id="54ba5-164">`XmlKeyManager`erstellt das entsprechende `IAuthenticatedEncryptorDescriptorDeserializer` -Instanzen, die Dokumente zu deserialisierenden gestaffelte `IAuthenticatedEncryptorDescriptor` -Instanzen, die einzelnen umschlossen werden `IKey` Instanzen.</span><span class="sxs-lookup"><span data-stu-id="54ba5-164">`XmlKeyManager` creates the appropriate `IAuthenticatedEncryptorDescriptorDeserializer` instances to deserialize the documents back into `IAuthenticatedEncryptorDescriptor` instances, which are then wrapped in individual `IKey` instances.</span></span> <span data-ttu-id="54ba5-165">Diese Auflistung von `IKey` Instanzen wird an den Aufrufer zurückgegeben.</span><span class="sxs-lookup"><span data-stu-id="54ba5-165">This collection of `IKey` instances is returned to the caller.</span></span>

<span data-ttu-id="54ba5-166">Weitere Informationen zu bestimmten XML-Elemente finden Sie der [Schlüsselspeicher Format Dokument](../implementation/key-storage-format.md#data-protection-implementation-key-storage-format).</span><span class="sxs-lookup"><span data-stu-id="54ba5-166">Further information on the particular XML elements can be found in the [key storage format document](../implementation/key-storage-format.md#data-protection-implementation-key-storage-format).</span></span>

## <a name="ixmlrepository"></a><span data-ttu-id="54ba5-167">IXmlRepository</span><span class="sxs-lookup"><span data-stu-id="54ba5-167">IXmlRepository</span></span>

<span data-ttu-id="54ba5-168">Die `IXmlRepository` Schnittstelle darstellt, einen Typ, der dauerhaft zu XML und XML-Daten aus einem Sicherungsspeicher abrufen kann.</span><span class="sxs-lookup"><span data-stu-id="54ba5-168">The `IXmlRepository` interface represents a type that can persist XML to and retrieve XML from a backing store.</span></span> <span data-ttu-id="54ba5-169">Macht zwei APIs:</span><span class="sxs-lookup"><span data-stu-id="54ba5-169">It exposes two APIs:</span></span>

* <span data-ttu-id="54ba5-170">GetAllElements() : IReadOnlyCollection<XElement></span><span class="sxs-lookup"><span data-stu-id="54ba5-170">GetAllElements() : IReadOnlyCollection<XElement></span></span>

* <span data-ttu-id="54ba5-171">StoreElement ("XElement"-Element, FriendlyName Zeichenfolge)</span><span class="sxs-lookup"><span data-stu-id="54ba5-171">StoreElement(XElement element, string friendlyName)</span></span>

<span data-ttu-id="54ba5-172">Implementierungen von `IXmlRepository` müssen nicht den Umweg über diese XML-Code zu analysieren.</span><span class="sxs-lookup"><span data-stu-id="54ba5-172">Implementations of `IXmlRepository` don't need to parse the XML passing through them.</span></span> <span data-ttu-id="54ba5-173">Sie sollten die XML-Dokumente als nicht transparent behandelt und höhere Ebenen Gedanken machen, generieren und die Dokumente analysieren können.</span><span class="sxs-lookup"><span data-stu-id="54ba5-173">They should treat the XML documents as opaque and let higher layers worry about generating and parsing the documents.</span></span>

<span data-ttu-id="54ba5-174">Es gibt zwei integrierte konkrete Typen die implementieren `IXmlRepository`: `FileSystemXmlRepository` und `RegistryXmlRepository`.</span><span class="sxs-lookup"><span data-stu-id="54ba5-174">There are two built-in concrete types which implement `IXmlRepository`: `FileSystemXmlRepository` and `RegistryXmlRepository`.</span></span> <span data-ttu-id="54ba5-175">Finden Sie unter der [softwareschlüsselspeicher-Anbieter Dokument](../implementation/key-storage-providers.md#data-protection-implementation-key-storage-providers) für Weitere Informationen.</span><span class="sxs-lookup"><span data-stu-id="54ba5-175">See the [key storage providers document](../implementation/key-storage-providers.md#data-protection-implementation-key-storage-providers) for more information.</span></span> <span data-ttu-id="54ba5-176">Registrieren ein benutzerdefinierten `IXmlRepository` wäre die geeignete Weise verwenden Sie einen anderen Sicherungsspeicher, z. B. Azure Blob-Speicher.</span><span class="sxs-lookup"><span data-stu-id="54ba5-176">Registering a custom `IXmlRepository` would be the appropriate manner to use a different backing store, e.g., Azure Blob Storage.</span></span>

<span data-ttu-id="54ba5-177">Um das Standard-Repository anwendungsweite ändern möchten, registrieren Sie eine benutzerdefinierte `IXmlRepository` Instanz:</span><span class="sxs-lookup"><span data-stu-id="54ba5-177">To change the default repository application-wide, register a custom `IXmlRepository` instance:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="54ba5-178">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="54ba5-178">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

   ```csharp
   services.Configure<KeyManagementOptions>(options => options.XmlRepository = new MyCustomXmlRepository());
   ```
   
# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="54ba5-179">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="54ba5-179">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

   ```csharp
   services.AddSingleton<IXmlRepository>(new MyCustomXmlRepository());
   ```

---

<a name="data-protection-extensibility-key-management-ixmlencryptor"></a>

## <a name="ixmlencryptor"></a><span data-ttu-id="54ba5-180">IXmlEncryptor</span><span class="sxs-lookup"><span data-stu-id="54ba5-180">IXmlEncryptor</span></span>

<span data-ttu-id="54ba5-181">Die `IXmlEncryptor` Schnittstelle darstellt, einen Typ, der eine nur-Text-XML-Element verschlüsseln kann.</span><span class="sxs-lookup"><span data-stu-id="54ba5-181">The `IXmlEncryptor` interface represents a type that can encrypt a plaintext XML element.</span></span> <span data-ttu-id="54ba5-182">Es stellt eine einzelne API bereit:</span><span class="sxs-lookup"><span data-stu-id="54ba5-182">It exposes a single API:</span></span>

* <span data-ttu-id="54ba5-183">("XElement" PlaintextElement) zu verschlüsseln: EncryptedXmlInfo</span><span class="sxs-lookup"><span data-stu-id="54ba5-183">Encrypt(XElement plaintextElement) : EncryptedXmlInfo</span></span>

<span data-ttu-id="54ba5-184">Wenn ein serialisiertes `IAuthenticatedEncryptorDescriptor` enthält alle Elemente, die als "erfordert Verschlüsselung," gekennzeichnet sind, klicken Sie dann `XmlKeyManager` führt diese Elemente über den konfigurierten `IXmlEncryptor`des `Encrypt` -Methode, und es bleiben enciphered Element statt über das Nur-Text-Element, um die `IXmlRepository`.</span><span class="sxs-lookup"><span data-stu-id="54ba5-184">If a serialized `IAuthenticatedEncryptorDescriptor` contains any elements marked as "requires encryption", then `XmlKeyManager` will run those elements through the configured `IXmlEncryptor`'s `Encrypt` method, and it will persist the enciphered element rather than the plaintext element to the `IXmlRepository`.</span></span> <span data-ttu-id="54ba5-185">Die Ausgabe der `Encrypt` Methode ist ein `EncryptedXmlInfo` Objekt.</span><span class="sxs-lookup"><span data-stu-id="54ba5-185">The output of the `Encrypt` method is an `EncryptedXmlInfo` object.</span></span> <span data-ttu-id="54ba5-186">Dieses Objekt ist ein Wrapper enthält sowohl die resultierenden enciphered `XElement` und den Typ der darstellt ein `IXmlDecryptor` die verwendet werden können, um das entsprechende Element zu entschlüsseln.</span><span class="sxs-lookup"><span data-stu-id="54ba5-186">This object is a wrapper which contains both the resultant enciphered `XElement` and the Type which represents an `IXmlDecryptor` which can be used to decipher the corresponding element.</span></span>

<span data-ttu-id="54ba5-187">Es gibt vier integrierte konkrete Typen die implementieren `IXmlEncryptor`:</span><span class="sxs-lookup"><span data-stu-id="54ba5-187">There are four built-in concrete types which implement `IXmlEncryptor`:</span></span>
* `CertificateXmlEncryptor`
* `DpapiNGXmlEncryptor`
* `DpapiXmlEncryptor`
* `NullXmlEncryptor`

<span data-ttu-id="54ba5-188">Finden Sie unter der [Schlüsselverschlüsselung auf Rest Dokumentbibliotheks-](../implementation/key-encryption-at-rest.md#data-protection-implementation-key-encryption-at-rest) für Weitere Informationen.</span><span class="sxs-lookup"><span data-stu-id="54ba5-188">See the [key encryption at rest document](../implementation/key-encryption-at-rest.md#data-protection-implementation-key-encryption-at-rest) for more information.</span></span>

<span data-ttu-id="54ba5-189">Um den Schlüssel Verschlüsselung auf Rest Standardmechanismus anwendungsweite zu ändern, registrieren Sie ein benutzerdefiniertes `IXmlEncryptor` Instanz:</span><span class="sxs-lookup"><span data-stu-id="54ba5-189">To change the default key-encryption-at-rest mechanism application-wide, register a custom `IXmlEncryptor` instance:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="54ba5-190">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="54ba5-190">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

   ```csharp
   services.Configure<KeyManagementOptions>(options => options.XmlEncryptor = new MyCustomXmlEncryptor());
   ```
   
# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="54ba5-191">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="54ba5-191">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

   ```csharp
   services.AddSingleton<IXmlEncryptor>(new MyCustomXmlEncryptor());
   ```

---

## <a name="ixmldecryptor"></a><span data-ttu-id="54ba5-192">IXmlDecryptor</span><span class="sxs-lookup"><span data-stu-id="54ba5-192">IXmlDecryptor</span></span>

<span data-ttu-id="54ba5-193">Die `IXmlDecryptor` Schnittstelle darstellt, einen Typ, der weiß, wie zum Entschlüsseln einer `XElement` , die über enciphered wurde ein `IXmlEncryptor`.</span><span class="sxs-lookup"><span data-stu-id="54ba5-193">The `IXmlDecryptor` interface represents a type that knows how to decrypt an `XElement` that was enciphered via an `IXmlEncryptor`.</span></span> <span data-ttu-id="54ba5-194">Es stellt eine einzelne API bereit:</span><span class="sxs-lookup"><span data-stu-id="54ba5-194">It exposes a single API:</span></span>

* <span data-ttu-id="54ba5-195">Entschlüsseln ("XElement" EncryptedElement): "XElement"</span><span class="sxs-lookup"><span data-stu-id="54ba5-195">Decrypt(XElement encryptedElement) : XElement</span></span>

<span data-ttu-id="54ba5-196">Die `Decrypt` Methode macht die Verschlüsselung von `IXmlEncryptor.Encrypt`.</span><span class="sxs-lookup"><span data-stu-id="54ba5-196">The `Decrypt` method undoes the encryption performed by `IXmlEncryptor.Encrypt`.</span></span> <span data-ttu-id="54ba5-197">Im Allgemeinen jede konkrete `IXmlEncryptor` Implementierung weist einen entsprechenden konkreten `IXmlDecryptor` Implementierung.</span><span class="sxs-lookup"><span data-stu-id="54ba5-197">Generally, each concrete `IXmlEncryptor` implementation will have a corresponding concrete `IXmlDecryptor` implementation.</span></span>

<span data-ttu-id="54ba5-198">Typen der implementieren `IXmlDecryptor` sollte eines der folgenden zwei öffentlichen Konstruktoren aufweisen:</span><span class="sxs-lookup"><span data-stu-id="54ba5-198">Types which implement `IXmlDecryptor` should have one of the following two public constructors:</span></span>

* <span data-ttu-id="54ba5-199">.ctor(IServiceProvider)</span><span class="sxs-lookup"><span data-stu-id="54ba5-199">.ctor(IServiceProvider)</span></span>
* <span data-ttu-id="54ba5-200">.ctor()</span><span class="sxs-lookup"><span data-stu-id="54ba5-200">.ctor()</span></span>

> [!NOTE]
> <span data-ttu-id="54ba5-201">Die `IServiceProvider` zum Übergeben des Konstruktors kann null sein.</span><span class="sxs-lookup"><span data-stu-id="54ba5-201">The `IServiceProvider` passed to the constructor may be null.</span></span>

## <a name="ikeyescrowsink"></a><span data-ttu-id="54ba5-202">IKeyEscrowSink</span><span class="sxs-lookup"><span data-stu-id="54ba5-202">IKeyEscrowSink</span></span>

<span data-ttu-id="54ba5-203">Die `IKeyEscrowSink` Schnittstelle darstellt, einen Typ, der hinterlegte vertraulicher Informationen ausführen kann.</span><span class="sxs-lookup"><span data-stu-id="54ba5-203">The `IKeyEscrowSink` interface represents a type that can perform escrow of sensitive information.</span></span> <span data-ttu-id="54ba5-204">Beachten Sie, dass die serialisierte Deskriptoren enthalten möglicherweise vertrauliche Informationen (z. B. das kryptografische Material), und dies ist, was mit der Einführung des geführt hat die [IXmlEncryptor](xref:security/data-protection/extensibility/key-management#data-protection-extensibility-key-management-ixmlencryptor) ursprünglich geben.</span><span class="sxs-lookup"><span data-stu-id="54ba5-204">Recall that serialized descriptors might contain sensitive information (such as cryptographic material), and this is what led to the introduction of the [IXmlEncryptor](xref:security/data-protection/extensibility/key-management#data-protection-extensibility-key-management-ixmlencryptor) type in the first place.</span></span> <span data-ttu-id="54ba5-205">Allerdings Unfall auftreten, und Schlüssel Ringe können gelöscht oder beschädigt ist.</span><span class="sxs-lookup"><span data-stu-id="54ba5-205">However, accidents happen, and key rings can be deleted or become corrupted.</span></span>

<span data-ttu-id="54ba5-206">Die hinterlegte-Schnittstelle bietet eine Notfall Escape-Schraffur, ermöglicht den Zugriff auf die unformatierten serialisierten XML-Code vor dem Transformieren von keiner konfiguriert ist [IXmlEncryptor](xref:security/data-protection/extensibility/key-management#data-protection-extensibility-key-management-ixmlencryptor).</span><span class="sxs-lookup"><span data-stu-id="54ba5-206">The escrow interface provides an emergency escape hatch, allowing access to the raw serialized XML before it is transformed by any configured [IXmlEncryptor](xref:security/data-protection/extensibility/key-management#data-protection-extensibility-key-management-ixmlencryptor).</span></span> <span data-ttu-id="54ba5-207">Die Schnittstelle macht eine einzige API verfügbar:</span><span class="sxs-lookup"><span data-stu-id="54ba5-207">The interface exposes a single API:</span></span>

* <span data-ttu-id="54ba5-208">Speicher (Guid KeyId, "XElement"-Element)</span><span class="sxs-lookup"><span data-stu-id="54ba5-208">Store(Guid keyId, XElement element)</span></span>

<span data-ttu-id="54ba5-209">Es liegt die `IKeyEscrowSink` -Implementierung, die das bereitgestellte Element auf sichere Weise mit Geschäftsrichtlinie konsistent behandeln.</span><span class="sxs-lookup"><span data-stu-id="54ba5-209">It is up to the `IKeyEscrowSink` implementation to handle the provided element in a secure manner consistent with business policy.</span></span> <span data-ttu-id="54ba5-210">Eine mögliche Implementierung konnte für die Senke werden zum Verschlüsseln von XML-Elements mit einem bekannten Unternehmens x. 509-Zertifikat sein, in dem der private Zertifikatschlüssel hinterlegt hat. die `CertificateXmlEncryptor` Typ dabei helfen kann.</span><span class="sxs-lookup"><span data-stu-id="54ba5-210">One possible implementation could be for the escrow sink to encrypt the XML element using a known corporate X.509 certificate where the certificate's private key has been escrowed; the `CertificateXmlEncryptor` type can assist with this.</span></span> <span data-ttu-id="54ba5-211">Die `IKeyEscrowSink` Implementierung ist auch für das bereitgestellte Element entsprechend beibehalten zuständig.</span><span class="sxs-lookup"><span data-stu-id="54ba5-211">The `IKeyEscrowSink` implementation is also responsible for persisting the provided element appropriately.</span></span>

<span data-ttu-id="54ba5-212">Keine hinterlegte-Mechanismus ist standardmäßig aktiviert, wenn Server-Administratoren können [konfigurieren Sie diese Option Global](xref:security/data-protection/configuration/machine-wide-policy).</span><span class="sxs-lookup"><span data-stu-id="54ba5-212">By default no escrow mechanism is enabled, though server administrators can [configure this globally](xref:security/data-protection/configuration/machine-wide-policy).</span></span> <span data-ttu-id="54ba5-213">Sie können auch programmgesteuert programmiert werden über die `IDataProtectionBuilder.AddKeyEscrowSink` Methode, wie im folgenden Beispiel dargestellt.</span><span class="sxs-lookup"><span data-stu-id="54ba5-213">It can also be configured programmatically via the `IDataProtectionBuilder.AddKeyEscrowSink` method as shown in the sample below.</span></span> <span data-ttu-id="54ba5-214">Die `AddKeyEscrowSink` Methode Überladungen spiegeln die `IServiceCollection.AddSingleton` und `IServiceCollection.AddInstance` Überladungen, als `IKeyEscrowSink` Instanzen sollten Singletons verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="54ba5-214">The `AddKeyEscrowSink` method overloads mirror the `IServiceCollection.AddSingleton` and `IServiceCollection.AddInstance` overloads, as `IKeyEscrowSink` instances are intended to be singletons.</span></span> <span data-ttu-id="54ba5-215">Wenn mehrere `IKeyEscrowSink` -Instanzen registriert sind, jeweils wird während der Generierung, aufgerufen werden, damit der Schlüssel können gleichzeitig auf mehrere Mechanismen hinterlegt werden.</span><span class="sxs-lookup"><span data-stu-id="54ba5-215">If multiple `IKeyEscrowSink` instances are registered, each one will be called during key generation, so keys can be escrowed to multiple mechanisms simultaneously.</span></span>

<span data-ttu-id="54ba5-216">Es ist keine API zum Lesen von Materialien aus einer `IKeyEscrowSink` Instanz.</span><span class="sxs-lookup"><span data-stu-id="54ba5-216">There is no API to read material from an `IKeyEscrowSink` instance.</span></span> <span data-ttu-id="54ba5-217">Dies ist konsistent mit der Theorie zum Entwurf des hinterlegte angibt: beabsichtigt hat das Schlüsselmaterial einer vertrauenswürdigen Zertifizierungsstelle zu ermöglichen, und seit die Anwendung selbst keiner vertrauenswürdigen Zertifizierungsstelle ist, er keinen Zugriff haben sollten eigene escrowed Material.</span><span class="sxs-lookup"><span data-stu-id="54ba5-217">This is consistent with the design theory of the escrow mechanism: it's intended to make the key material accessible to a trusted authority, and since the application is itself not a trusted authority, it shouldn't have access to its own escrowed material.</span></span>

<span data-ttu-id="54ba5-218">Der folgende Beispielcode veranschaulicht das Erstellen und registrieren einen `IKeyEscrowSink` , in dem Schlüssel hinterlegt werden, dass nur Mitglieder der "CONTOSODomain-Admins" wiederhergestellt werden können.</span><span class="sxs-lookup"><span data-stu-id="54ba5-218">The following sample code demonstrates creating and registering an `IKeyEscrowSink` where keys are escrowed such that only members of "CONTOSODomain Admins" can recover them.</span></span>

> [!NOTE]
> <span data-ttu-id="54ba5-219">Um dieses Beispiel ausführen zu können, muss auf eine Domäne eingebundenen Windows 8 / Windows Server 2012-Computer und dem Domänencontroller muss WindowsServer 2012 oder höher sein.</span><span class="sxs-lookup"><span data-stu-id="54ba5-219">To run this sample, you must be on a domain-joined Windows 8 / Windows Server 2012 machine, and the domain controller must be Windows Server 2012 or later.</span></span>

[!code-csharp[Main](key-management/samples/key-management-extensibility.cs)]
