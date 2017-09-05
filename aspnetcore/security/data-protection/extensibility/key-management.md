---
title: "Schlüsselverwaltungsdienst-Erweiterbarkeit"
author: rick-anderson
description: 
keywords: ASP.NET Core
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 3606b251-8324-4485-8d52-582a2cd5cffb
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/extensibility/key-management
ms.openlocfilehash: fb74905660015b9a83503e1f74b25c66ae9df9e3
ms.sourcegitcommit: bd05f7ea8f87ad076ef6e8b704698ebcba5ca80c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/25/2017
---
# <a name="key-management-extensibility"></a><span data-ttu-id="7014f-103">Schlüsselverwaltungsdienst-Erweiterbarkeit</span><span class="sxs-lookup"><span data-stu-id="7014f-103">Key management extensibility</span></span>

<a name=data-protection-extensibility-key-management></a>

>[!TIP]
> <span data-ttu-id="7014f-104">Lesen der [schlüsselverwaltung](../implementation/key-management.md#data-protection-implementation-key-management) Abschnitt vor dem Lesen in diesem Abschnitt, wie einige der grundlegenden Konzepte hinter dieser APIs erläutert wird.</span><span class="sxs-lookup"><span data-stu-id="7014f-104">Read the [key management](../implementation/key-management.md#data-protection-implementation-key-management) section before reading this section, as it explains some of the fundamental concepts behind these APIs.</span></span>

>[!WARNING]
> <span data-ttu-id="7014f-105">Typen, die jede der folgenden Schnittstellen implementieren, sollten threadsichere werden für mehrere Aufrufer.</span><span class="sxs-lookup"><span data-stu-id="7014f-105">Types that implement any of the following interfaces should be thread-safe for multiple callers.</span></span>

## <a name="key"></a><span data-ttu-id="7014f-106">Key</span><span class="sxs-lookup"><span data-stu-id="7014f-106">Key</span></span>

<span data-ttu-id="7014f-107">Die IKey-Schnittstelle ist die grundlegende Darstellung eines Schlüssels in Kryptografiesystem.</span><span class="sxs-lookup"><span data-stu-id="7014f-107">The IKey interface is the basic representation of a key in cryptosystem.</span></span> <span data-ttu-id="7014f-108">Der Begriff Schlüssel ist hier insofern abstrakte, nicht in der struktureller von "kryptografischen schlüsselmaterialien" verwendet.</span><span class="sxs-lookup"><span data-stu-id="7014f-108">The term key is used here in the abstract sense, not in the literal sense of "cryptographic key material".</span></span> <span data-ttu-id="7014f-109">Ein Schlüssel hat die folgenden Eigenschaften:</span><span class="sxs-lookup"><span data-stu-id="7014f-109">A key has the following properties:</span></span>

* <span data-ttu-id="7014f-110">Aktivierung, Erstellung und Ablaufdaten</span><span class="sxs-lookup"><span data-stu-id="7014f-110">Activation, creation, and expiration dates</span></span>

* <span data-ttu-id="7014f-111">Status der Zertifikatsperre</span><span class="sxs-lookup"><span data-stu-id="7014f-111">Revocation status</span></span>

* <span data-ttu-id="7014f-112">Schlüsselbezeichner (eine GUID)</span><span class="sxs-lookup"><span data-stu-id="7014f-112">Key identifier (a GUID)</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="7014f-113">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="7014f-113">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="7014f-114">Darüber hinaus macht IKey eine CreateEncryptor-Methode, die verwendet werden kann, erstellen eine [IAuthenticatedEncryptor](core-crypto.md#data-protection-extensibility-core-crypto-iauthenticatedencryptor) Instanz gebunden werden, um diesen Schlüssel.</span><span class="sxs-lookup"><span data-stu-id="7014f-114">Additionally, IKey exposes a CreateEncryptor method which can be used to create an [IAuthenticatedEncryptor](core-crypto.md#data-protection-extensibility-core-crypto-iauthenticatedencryptor) instance tied to this key.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="7014f-115">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="7014f-115">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="7014f-116">Darüber hinaus macht IKey eine CreateEncryptorInstance-Methode, die verwendet werden kann, erstellen eine [IAuthenticatedEncryptor](core-crypto.md#data-protection-extensibility-core-crypto-iauthenticatedencryptor) Instanz gebunden werden, um diesen Schlüssel.</span><span class="sxs-lookup"><span data-stu-id="7014f-116">Additionally, IKey exposes a CreateEncryptorInstance method which can be used to create an [IAuthenticatedEncryptor](core-crypto.md#data-protection-extensibility-core-crypto-iauthenticatedencryptor) instance tied to this key.</span></span>

---

> [!NOTE]
> <span data-ttu-id="7014f-117">Es ist keine API das unformatierte kryptografische Material aus einer IKey-Instanz abgerufen.</span><span class="sxs-lookup"><span data-stu-id="7014f-117">There is no API to retrieve the raw cryptographic material from an IKey instance.</span></span>

## <a name="ikeymanager"></a><span data-ttu-id="7014f-118">IKeyManager</span><span class="sxs-lookup"><span data-stu-id="7014f-118">IKeyManager</span></span>

<span data-ttu-id="7014f-119">Die IKeyManager-Schnittstelle stellt ein Objekt, das verantwortlich für allgemeine Schlüsselspeicher, Abruf und Manipulation dar.</span><span class="sxs-lookup"><span data-stu-id="7014f-119">The IKeyManager interface represents an object responsible for general key storage, retrieval, and manipulation.</span></span> <span data-ttu-id="7014f-120">Macht drei Vorgänge auf höherer Ebene:</span><span class="sxs-lookup"><span data-stu-id="7014f-120">It exposes three high-level operations:</span></span>

* <span data-ttu-id="7014f-121">Erstellen Sie einen neuen Schlüssel, und klicken Sie in Speicher gespeichert.</span><span class="sxs-lookup"><span data-stu-id="7014f-121">Create a new key and persist it to storage.</span></span>

* <span data-ttu-id="7014f-122">Alle Schlüssel aus dem Speicher abrufen.</span><span class="sxs-lookup"><span data-stu-id="7014f-122">Get all keys from storage.</span></span>

* <span data-ttu-id="7014f-123">Einen oder mehrere Schlüssel widerrufen, und die Sperrinformationen in den Speicher beibehalten.</span><span class="sxs-lookup"><span data-stu-id="7014f-123">Revoke one or more keys and persist the revocation information to storage.</span></span>

>[!WARNING]
> <span data-ttu-id="7014f-124">Schreiben einer IKeyManager ist eine sehr komplexe Aufgabe und die meisten Entwickler sollten nicht versuchen.</span><span class="sxs-lookup"><span data-stu-id="7014f-124">Writing an IKeyManager is a very advanced task, and the majority of developers should not attempt it.</span></span> <span data-ttu-id="7014f-125">Stattdessen die meisten Entwickler sollten nutzen die Funktionen von Angeboten die [XmlKeyManager](xref:security/data-protection/extensibility/key-management#data-protection-extensibility-key-management-xmlkeymanager) Klasse.</span><span class="sxs-lookup"><span data-stu-id="7014f-125">Instead, most developers should take advantage of the facilities offered by the [XmlKeyManager](xref:security/data-protection/extensibility/key-management#data-protection-extensibility-key-management-xmlkeymanager) class.</span></span>

<a name=data-protection-extensibility-key-management-xmlkeymanager></a>

## <a name="xmlkeymanager"></a><span data-ttu-id="7014f-126">XmlKeyManager</span><span class="sxs-lookup"><span data-stu-id="7014f-126">XmlKeyManager</span></span>

<span data-ttu-id="7014f-127">Der XmlKeyManager-Typ ist die konkrete Implementierung der mitgelieferten IKeyManager.</span><span class="sxs-lookup"><span data-stu-id="7014f-127">The XmlKeyManager type is the in-box concrete implementation of IKeyManager.</span></span> <span data-ttu-id="7014f-128">Er bietet mehrere nützliche Funktionen, einschließlich schlüsselhinterlegung und Verschlüsselung von Schlüsseln im Ruhezustand.</span><span class="sxs-lookup"><span data-stu-id="7014f-128">It provides several useful facilities, including key escrow and encryption of keys at rest.</span></span> <span data-ttu-id="7014f-129">Schlüssel in diesem System werden als XML-Elementen dargestellt (insbesondere ["XElement"](https://msdn.microsoft.com/library/system.xml.linq.xelement(v=vs.110).aspx)).</span><span class="sxs-lookup"><span data-stu-id="7014f-129">Keys in this system are represented as XML elements (specifically, [XElement](https://msdn.microsoft.com/library/system.xml.linq.xelement(v=vs.110).aspx)).</span></span>

<span data-ttu-id="7014f-130">XmlKeyManager hängt davon ab, auf mehrere andere Komponenten im Verlauf die Aufgaben erfüllen:</span><span class="sxs-lookup"><span data-stu-id="7014f-130">XmlKeyManager depends on several other components in the course of fulfilling its tasks:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="7014f-131">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="7014f-131">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

* <span data-ttu-id="7014f-132">AlgorithmConfiguration, der bestimmt, die durch neue Schlüssel verwendeten Algorithmen.</span><span class="sxs-lookup"><span data-stu-id="7014f-132">AlgorithmConfiguration, which dictates the algorithms used by new keys.</span></span>

* <span data-ttu-id="7014f-133">IXmlRepository, welche steuert, wo der Schlüssel im Speicher beibehalten werden.</span><span class="sxs-lookup"><span data-stu-id="7014f-133">IXmlRepository, which controls where keys are persisted in storage.</span></span>

* <span data-ttu-id="7014f-134">IXmlEncryptor [optional] die ermöglicht das Verschlüsseln der Schlüssel im Ruhezustand.</span><span class="sxs-lookup"><span data-stu-id="7014f-134">IXmlEncryptor [optional], which allows encrypting keys at rest.</span></span>

* <span data-ttu-id="7014f-135">IKeyEscrowSink [optional] die schlüsselhinterlegung Dienste bereitstellt.</span><span class="sxs-lookup"><span data-stu-id="7014f-135">IKeyEscrowSink [optional], which provides key escrow services.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="7014f-136">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="7014f-136">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

* <span data-ttu-id="7014f-137">IXmlRepository, welche steuert, wo der Schlüssel im Speicher beibehalten werden.</span><span class="sxs-lookup"><span data-stu-id="7014f-137">IXmlRepository, which controls where keys are persisted in storage.</span></span>

* <span data-ttu-id="7014f-138">IXmlEncryptor [optional] die ermöglicht das Verschlüsseln der Schlüssel im Ruhezustand.</span><span class="sxs-lookup"><span data-stu-id="7014f-138">IXmlEncryptor [optional], which allows encrypting keys at rest.</span></span>

* <span data-ttu-id="7014f-139">IKeyEscrowSink [optional] die schlüsselhinterlegung Dienste bereitstellt.</span><span class="sxs-lookup"><span data-stu-id="7014f-139">IKeyEscrowSink [optional], which provides key escrow services.</span></span>

---

<span data-ttu-id="7014f-140">Im folgenden finden Sie übersichtliche Diagramme, die angibt, wie diese Komponenten zusammen in XmlKeyManager verbunden sind.</span><span class="sxs-lookup"><span data-stu-id="7014f-140">Below are high-level diagrams which indicate how these components are wired together within XmlKeyManager.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="7014f-141">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="7014f-141">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

   ![Schlüssel erstellen](key-management/_static/keycreation2.png)

   <span data-ttu-id="7014f-143">*Schlüssels Erstellung / CreateNewKey*</span><span class="sxs-lookup"><span data-stu-id="7014f-143">*Key Creation / CreateNewKey*</span></span>

<span data-ttu-id="7014f-144">In der Implementierung von CreateNewKey wird die AlgorithmConfiguration-Komponente verwendet, eine eindeutige IAuthenticatedEncryptorDescriptor zu erstellen, die dann als XML serialisiert wird.</span><span class="sxs-lookup"><span data-stu-id="7014f-144">In the implementation of CreateNewKey, the AlgorithmConfiguration component is used to create a unique IAuthenticatedEncryptorDescriptor, which is then serialized as XML.</span></span> <span data-ttu-id="7014f-145">Wenn die Senke schlüsselhinterlegung vorhanden ist, wird der XML-Rohdaten (unverschlüsselt) für die langfristige Speicherung an die Senke bereitgestellt.</span><span class="sxs-lookup"><span data-stu-id="7014f-145">If a key escrow sink is present, the raw (unencrypted) XML is provided to the sink for long-term storage.</span></span> <span data-ttu-id="7014f-146">Der nicht verschlüsselte XML-Code wird dann über eine IXmlEncryptor ausgeführt (falls erforderlich) um das verschlüsselte XML-Dokument zu generieren.</span><span class="sxs-lookup"><span data-stu-id="7014f-146">The unencrypted XML is then run through an IXmlEncryptor (if required) to generate the encrypted XML document.</span></span> <span data-ttu-id="7014f-147">Dieses verschlüsselte Dokument wird in langfristigen Speicher über die IXmlRepository beibehalten.</span><span class="sxs-lookup"><span data-stu-id="7014f-147">This encrypted document is persisted to long-term storage via the IXmlRepository.</span></span> <span data-ttu-id="7014f-148">(Wenn keine IXmlEncryptor konfiguriert ist, werden in der IXmlRepository unverschlüsselte Dokument beibehalten.)</span><span class="sxs-lookup"><span data-stu-id="7014f-148">(If no IXmlEncryptor is configured, the unencrypted document is persisted in the IXmlRepository.)</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="7014f-149">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="7014f-149">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

   ![Schlüssel erstellen](key-management/_static/keycreation1.png)

   <span data-ttu-id="7014f-151">*Schlüssels Erstellung / CreateNewKey*</span><span class="sxs-lookup"><span data-stu-id="7014f-151">*Key Creation / CreateNewKey*</span></span>

<span data-ttu-id="7014f-152">In der Implementierung von CreateNewKey wird die IAuthenticatedEncryptorConfiguration-Komponente verwendet, eine eindeutige IAuthenticatedEncryptorDescriptor zu erstellen, die dann als XML serialisiert wird.</span><span class="sxs-lookup"><span data-stu-id="7014f-152">In the implementation of CreateNewKey, the IAuthenticatedEncryptorConfiguration component is used to create a unique IAuthenticatedEncryptorDescriptor, which is then serialized as XML.</span></span> <span data-ttu-id="7014f-153">Wenn die Senke schlüsselhinterlegung vorhanden ist, wird der XML-Rohdaten (unverschlüsselt) für die langfristige Speicherung an die Senke bereitgestellt.</span><span class="sxs-lookup"><span data-stu-id="7014f-153">If a key escrow sink is present, the raw (unencrypted) XML is provided to the sink for long-term storage.</span></span> <span data-ttu-id="7014f-154">Der nicht verschlüsselte XML-Code wird dann über eine IXmlEncryptor ausgeführt (falls erforderlich) um das verschlüsselte XML-Dokument zu generieren.</span><span class="sxs-lookup"><span data-stu-id="7014f-154">The unencrypted XML is then run through an IXmlEncryptor (if required) to generate the encrypted XML document.</span></span> <span data-ttu-id="7014f-155">Dieses verschlüsselte Dokument wird in langfristigen Speicher über die IXmlRepository beibehalten.</span><span class="sxs-lookup"><span data-stu-id="7014f-155">This encrypted document is persisted to long-term storage via the IXmlRepository.</span></span> <span data-ttu-id="7014f-156">(Wenn keine IXmlEncryptor konfiguriert ist, werden in der IXmlRepository unverschlüsselte Dokument beibehalten.)</span><span class="sxs-lookup"><span data-stu-id="7014f-156">(If no IXmlEncryptor is configured, the unencrypted document is persisted in the IXmlRepository.)</span></span>

---

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="7014f-157">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="7014f-157">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

   ![Abruf der](key-management/_static/keyretrieval2.png)
   
# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="7014f-159">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="7014f-159">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

   ![Abruf der](key-management/_static/keyretrieval1.png)

---

   <span data-ttu-id="7014f-161">*Abrufen von Schlüssel / GetAllKeys*</span><span class="sxs-lookup"><span data-stu-id="7014f-161">*Key Retrieval / GetAllKeys*</span></span>

<span data-ttu-id="7014f-162">In der Implementierung der GetAllKeys werden die XML-Dokumente, die Schlüssel und Revocations darstellen aus der zugrunde liegenden IXmlRepository gelesen.</span><span class="sxs-lookup"><span data-stu-id="7014f-162">In the implementation of GetAllKeys, the XML documents representing keys and revocations are read from the underlying IXmlRepository.</span></span> <span data-ttu-id="7014f-163">Wenn diese Dokumente verschlüsselt sind, wird das System diese automatisch entschlüsselt.</span><span class="sxs-lookup"><span data-stu-id="7014f-163">If these documents are encrypted, the system will automatically decrypt them.</span></span> <span data-ttu-id="7014f-164">XmlKeyManager erstellt die entsprechenden IAuthenticatedEncryptorDescriptorDeserializer-Instanzen, um die Dokumente wieder in Instanzen IAuthenticatedEncryptorDescriptor, Deserialisieren der einzelnen IKey Instanzen umschlossen sind.</span><span class="sxs-lookup"><span data-stu-id="7014f-164">XmlKeyManager creates the appropriate IAuthenticatedEncryptorDescriptorDeserializer instances to deserialize the documents back into IAuthenticatedEncryptorDescriptor instances, which are then wrapped in individual IKey instances.</span></span> <span data-ttu-id="7014f-165">Diese Auflistung von Instanzen IKey wird an den Aufrufer zurückgegeben.</span><span class="sxs-lookup"><span data-stu-id="7014f-165">This collection of IKey instances is returned to the caller.</span></span>

<span data-ttu-id="7014f-166">Weitere Informationen zu bestimmten XML-Elemente finden Sie der [Schlüsselspeicher Format Dokument](../implementation/key-storage-format.md#data-protection-implementation-key-storage-format).</span><span class="sxs-lookup"><span data-stu-id="7014f-166">Further information on the particular XML elements can be found in the [key storage format document](../implementation/key-storage-format.md#data-protection-implementation-key-storage-format).</span></span>

## <a name="ixmlrepository"></a><span data-ttu-id="7014f-167">IXmlRepository</span><span class="sxs-lookup"><span data-stu-id="7014f-167">IXmlRepository</span></span>

<span data-ttu-id="7014f-168">Die IXmlRepository-Schnittstelle stellt einen Typ, der dauerhaft zu XML und XML-Daten aus einem Sicherungsspeicher abrufen kann.</span><span class="sxs-lookup"><span data-stu-id="7014f-168">The IXmlRepository interface represents a type that can persist XML to and retrieve XML from a backing store.</span></span> <span data-ttu-id="7014f-169">Macht zwei APIs:</span><span class="sxs-lookup"><span data-stu-id="7014f-169">It exposes two APIs:</span></span>

* <span data-ttu-id="7014f-170">GetAllElements(): IReadOnlyCollection<XElement></span><span class="sxs-lookup"><span data-stu-id="7014f-170">GetAllElements() : IReadOnlyCollection<XElement></span></span>

* <span data-ttu-id="7014f-171">StoreElement ("XElement"-Element, FriendlyName Zeichenfolge)</span><span class="sxs-lookup"><span data-stu-id="7014f-171">StoreElement(XElement element, string friendlyName)</span></span>

<span data-ttu-id="7014f-172">Implementierungen von IXmlRepository müssen nicht den Umweg über diese XML-Code zu analysieren.</span><span class="sxs-lookup"><span data-stu-id="7014f-172">Implementations of IXmlRepository don't need to parse the XML passing through them.</span></span> <span data-ttu-id="7014f-173">Sie sollten die XML-Dokumente als nicht transparent behandelt und höhere Ebenen Gedanken machen, generieren und die Dokumente analysieren können.</span><span class="sxs-lookup"><span data-stu-id="7014f-173">They should treat the XML documents as opaque and let higher layers worry about generating and parsing the documents.</span></span>

<span data-ttu-id="7014f-174">Es gibt zwei integrierte konkrete Typen die IXmlRepository implementieren: FileSystemXmlRepository und RegistryXmlRepository.</span><span class="sxs-lookup"><span data-stu-id="7014f-174">There are two built-in concrete types which implement IXmlRepository: FileSystemXmlRepository and RegistryXmlRepository.</span></span> <span data-ttu-id="7014f-175">Finden Sie unter der [softwareschlüsselspeicher-Anbieter Dokument](../implementation/key-storage-providers.md#data-protection-implementation-key-storage-providers) für Weitere Informationen.</span><span class="sxs-lookup"><span data-stu-id="7014f-175">See the [key storage providers document](../implementation/key-storage-providers.md#data-protection-implementation-key-storage-providers) for more information.</span></span> <span data-ttu-id="7014f-176">Registrieren eine benutzerdefinierte IXmlRepository würde die geeignete Weise zu einem anderen Speicher, z. B. Azure Blob-Speicher sichern.</span><span class="sxs-lookup"><span data-stu-id="7014f-176">Registering a custom IXmlRepository would be the appropriate manner to use a different backing store, e.g., Azure Blob Storage.</span></span> <span data-ttu-id="7014f-177">Um die Standard-Repository eine anwendungsweite zu ändern, registrieren Sie benutzerdefinierte Singleton IXmlRepository in der Dienstanbieter.</span><span class="sxs-lookup"><span data-stu-id="7014f-177">To change the default repository application-wide, register a custom singleton IXmlRepository in the service provider.</span></span>

<a name=data-protection-extensibility-key-management-ixmlencryptor></a>

## <a name="ixmlencryptor"></a><span data-ttu-id="7014f-178">IXmlEncryptor</span><span class="sxs-lookup"><span data-stu-id="7014f-178">IXmlEncryptor</span></span>

<span data-ttu-id="7014f-179">Die IXmlEncryptor-Schnittstelle stellt einen Typ, der eine nur-Text-XML-Element verschlüsseln kann.</span><span class="sxs-lookup"><span data-stu-id="7014f-179">The IXmlEncryptor interface represents a type that can encrypt a plaintext XML element.</span></span> <span data-ttu-id="7014f-180">Es stellt eine einzelne API bereit:</span><span class="sxs-lookup"><span data-stu-id="7014f-180">It exposes a single API:</span></span>

* <span data-ttu-id="7014f-181">("XElement" PlaintextElement) zu verschlüsseln: EncryptedXmlInfo</span><span class="sxs-lookup"><span data-stu-id="7014f-181">Encrypt(XElement plaintextElement) : EncryptedXmlInfo</span></span>

<span data-ttu-id="7014f-182">Enthält einen serialisierten IAuthenticatedEncryptorDescriptor alle Elemente, die als "Verschlüsselung erfordert" gekennzeichnet, dann XmlKeyManager ausgeführt, diese Elemente über den konfigurierten IXmlEncryptor Encrypt-Methode, und bleiben enciphered Element stattdessen als den nur-Text-Element, das die IXmlRepository.</span><span class="sxs-lookup"><span data-stu-id="7014f-182">If a serialized IAuthenticatedEncryptorDescriptor contains any elements marked as "requires encryption", then XmlKeyManager will run those elements through the configured IXmlEncryptor's Encrypt method, and it will persist the enciphered element rather than the plaintext element to the IXmlRepository.</span></span> <span data-ttu-id="7014f-183">Die Ausgabe der Encrypt-Methode ist ein EncryptedXmlInfo-Objekt.</span><span class="sxs-lookup"><span data-stu-id="7014f-183">The output of the Encrypt method is an EncryptedXmlInfo object.</span></span> <span data-ttu-id="7014f-184">Dieses Objekt ist ein Wrapper enthält das resultierende enciphered "XElement" und der Typ, der eine IXmlDecryptor, die verwendet werden kann darstellt, um das entsprechende Element zu entschlüsseln.</span><span class="sxs-lookup"><span data-stu-id="7014f-184">This object is a wrapper which contains both the resultant enciphered XElement and the Type which represents an IXmlDecryptor which can be used to decipher the corresponding element.</span></span>

<span data-ttu-id="7014f-185">Es gibt vier integrierte konkrete Typen die IXmlEncryptor implementieren: CertificateXmlEncryptor, DpapiNGXmlEncryptor DpapiXmlEncryptor und NullXmlEncryptor.</span><span class="sxs-lookup"><span data-stu-id="7014f-185">There are four built-in concrete types which implement IXmlEncryptor: CertificateXmlEncryptor, DpapiNGXmlEncryptor, DpapiXmlEncryptor, and NullXmlEncryptor.</span></span> <span data-ttu-id="7014f-186">Finden Sie unter der [Schlüsselverschlüsselung auf Rest Dokumentbibliotheks-](../implementation/key-encryption-at-rest.md#data-protection-implementation-key-encryption-at-rest) für Weitere Informationen.</span><span class="sxs-lookup"><span data-stu-id="7014f-186">See the [key encryption at rest document](../implementation/key-encryption-at-rest.md#data-protection-implementation-key-encryption-at-rest) for more information.</span></span> <span data-ttu-id="7014f-187">Um den Schlüssel Verschlüsselung auf Rest Standardmechanismus anwendungsweite zu ändern, registrieren Sie benutzerdefinierte Singleton IXmlEncryptor in der Dienstanbieter.</span><span class="sxs-lookup"><span data-stu-id="7014f-187">To change the default key-encryption-at-rest mechanism application-wide, register a custom singleton IXmlEncryptor in the service provider.</span></span>

## <a name="ixmldecryptor"></a><span data-ttu-id="7014f-188">IXmlDecryptor</span><span class="sxs-lookup"><span data-stu-id="7014f-188">IXmlDecryptor</span></span>

<span data-ttu-id="7014f-189">Die IXmlDecryptor-Schnittstelle stellt einen Typ, der weiß, wie ein "XElement" entschlüsseln, die über eine IXmlEncryptor enciphered wurde.</span><span class="sxs-lookup"><span data-stu-id="7014f-189">The IXmlDecryptor interface represents a type that knows how to decrypt an XElement that was enciphered via an IXmlEncryptor.</span></span> <span data-ttu-id="7014f-190">Es stellt eine einzelne API bereit:</span><span class="sxs-lookup"><span data-stu-id="7014f-190">It exposes a single API:</span></span>

* <span data-ttu-id="7014f-191">Entschlüsseln ("XElement" EncryptedElement): "XElement"</span><span class="sxs-lookup"><span data-stu-id="7014f-191">Decrypt(XElement encryptedElement) : XElement</span></span>

<span data-ttu-id="7014f-192">Die Decrypt-Methode macht die Verschlüsselung von IXmlEncryptor.Encrypt ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="7014f-192">The Decrypt method undoes the encryption performed by IXmlEncryptor.Encrypt.</span></span> <span data-ttu-id="7014f-193">Jede konkrete Implementierung der IXmlEncryptor müssen im Allgemeinen eine entsprechende konkrete IXmlDecryptor-Implementierung.</span><span class="sxs-lookup"><span data-stu-id="7014f-193">Generally each concrete IXmlEncryptor implementation will have a corresponding concrete IXmlDecryptor implementation.</span></span>

<span data-ttu-id="7014f-194">Typen, die IXmlDecryptor implementieren müssen eine der folgenden zwei öffentlichen Konstruktoren aufweisen:</span><span class="sxs-lookup"><span data-stu-id="7014f-194">Types which implement IXmlDecryptor should have one of the following two public constructors:</span></span>

* <span data-ttu-id="7014f-195">.ctor(IServiceProvider)</span><span class="sxs-lookup"><span data-stu-id="7014f-195">.ctor(IServiceProvider)</span></span>

* <span data-ttu-id="7014f-196">.ctor()</span><span class="sxs-lookup"><span data-stu-id="7014f-196">.ctor()</span></span>

> [!NOTE]
> <span data-ttu-id="7014f-197">Der an den Konstruktor übergebene IServiceProvider kann null sein.</span><span class="sxs-lookup"><span data-stu-id="7014f-197">The IServiceProvider passed to the constructor may be null.</span></span>

## <a name="ikeyescrowsink"></a><span data-ttu-id="7014f-198">IKeyEscrowSink</span><span class="sxs-lookup"><span data-stu-id="7014f-198">IKeyEscrowSink</span></span>

<span data-ttu-id="7014f-199">Die IKeyEscrowSink-Schnittstelle stellt einen Typ, der hinterlegte vertraulicher Informationen ausführen kann.</span><span class="sxs-lookup"><span data-stu-id="7014f-199">The IKeyEscrowSink interface represents a type that can perform escrow of sensitive information.</span></span> <span data-ttu-id="7014f-200">Beachten Sie, dass die serialisierte Deskriptoren enthalten möglicherweise vertrauliche Informationen (z. B. das kryptografische Material), und dies ist, was mit der Einführung des geführt hat die [IXmlEncryptor](xref:security/data-protection/extensibility/key-management#data-protection-extensibility-key-management-ixmlencryptor) ursprünglich geben.</span><span class="sxs-lookup"><span data-stu-id="7014f-200">Recall that serialized descriptors might contain sensitive information (such as cryptographic material), and this is what led to the introduction of the [IXmlEncryptor](xref:security/data-protection/extensibility/key-management#data-protection-extensibility-key-management-ixmlencryptor) type in the first place.</span></span> <span data-ttu-id="7014f-201">Allerdings Unfall auftreten und Schlüsselbunde können gelöscht oder beschädigt ist.</span><span class="sxs-lookup"><span data-stu-id="7014f-201">However, accidents happen, and keyrings can be deleted or become corrupted.</span></span>

<span data-ttu-id="7014f-202">Die hinterlegte-Schnittstelle bietet eine Notfall Escape-Schraffur, ermöglicht den Zugriff auf die unformatierten serialisierten XML-Code vor dem Transformieren von keiner konfiguriert ist [IXmlEncryptor](xref:security/data-protection/extensibility/key-management#data-protection-extensibility-key-management-ixmlencryptor).</span><span class="sxs-lookup"><span data-stu-id="7014f-202">The escrow interface provides an emergency escape hatch, allowing access to the raw serialized XML before it is transformed by any configured [IXmlEncryptor](xref:security/data-protection/extensibility/key-management#data-protection-extensibility-key-management-ixmlencryptor).</span></span> <span data-ttu-id="7014f-203">Die Schnittstelle macht eine einzige API verfügbar:</span><span class="sxs-lookup"><span data-stu-id="7014f-203">The interface exposes a single API:</span></span>

* <span data-ttu-id="7014f-204">Speicher (Guid KeyId, "XElement"-Element)</span><span class="sxs-lookup"><span data-stu-id="7014f-204">Store(Guid keyId, XElement element)</span></span>

<span data-ttu-id="7014f-205">Es ist Aufgabe der IKeyEscrowSink-Implementierung, die das bereitgestellte Element auf sichere Weise mit Geschäftsrichtlinie konsistent behandeln.</span><span class="sxs-lookup"><span data-stu-id="7014f-205">It is up to the IKeyEscrowSink implementation to handle the provided element in a secure manner consistent with business policy.</span></span> <span data-ttu-id="7014f-206">Eine mögliche Implementierung konnte für die Senke werden zum Verschlüsseln von XML-Elements mit einem bekannten Unternehmens x. 509-Zertifikat sein, in dem der private Zertifikatschlüssel hinterlegt hat. Der Typ CertificateXmlEncryptor kann mit diesem unterstützen.</span><span class="sxs-lookup"><span data-stu-id="7014f-206">One possible implementation could be for the escrow sink to encrypt the XML element using a known corporate X.509 certificate where the certificate's private key has been escrowed; the CertificateXmlEncryptor type can assist with this.</span></span> <span data-ttu-id="7014f-207">Die Implementierung IKeyEscrowSink ist auch für das bereitgestellte Element entsprechend beibehalten zuständig.</span><span class="sxs-lookup"><span data-stu-id="7014f-207">The IKeyEscrowSink implementation is also responsible for persisting the provided element appropriately.</span></span>

<span data-ttu-id="7014f-208">Keine hinterlegte-Mechanismus ist standardmäßig aktiviert, wenn Server-Administratoren können [konfigurieren Sie diese Option Global](../configuration/machine-wide-policy.md#data-protection-configuration-machinewidepolicy).</span><span class="sxs-lookup"><span data-stu-id="7014f-208">By default no escrow mechanism is enabled, though server administrators can [configure this globally](../configuration/machine-wide-policy.md#data-protection-configuration-machinewidepolicy).</span></span> <span data-ttu-id="7014f-209">Sie können auch programmgesteuert programmiert werden über die *IDataProtectionBuilder.AddKeyEscrowSink* Methode, wie im folgenden Beispiel dargestellt.</span><span class="sxs-lookup"><span data-stu-id="7014f-209">It can also be configured programmatically via the *IDataProtectionBuilder.AddKeyEscrowSink* method as shown in the sample below.</span></span> <span data-ttu-id="7014f-210">Die *AddKeyEscrowSink* Methode Überladungen spiegeln die *IServiceCollection.AddSingleton* und *IServiceCollection.AddInstance* Überladungen, die als IKeyEscrowSink Instanzen sollten Singletons verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="7014f-210">The *AddKeyEscrowSink* method overloads mirror the *IServiceCollection.AddSingleton* and *IServiceCollection.AddInstance* overloads, as IKeyEscrowSink instances are intended to be singletons.</span></span> <span data-ttu-id="7014f-211">Wenn mehrere Instanzen von IKeyEscrowSink registriert sind, wird jeweils während der Generierung, aufgerufen werden, damit Schlüssel gleichzeitig auf mehrere Mechanismen hinterlegt werden können.</span><span class="sxs-lookup"><span data-stu-id="7014f-211">If multiple IKeyEscrowSink instances are registered, each one will be called during key generation, so keys can be escrowed to multiple mechanisms simultaneously.</span></span>

<span data-ttu-id="7014f-212">Es ist keine API zum Lesen von Materialien, die aus einer IKeyEscrowSink-Instanz.</span><span class="sxs-lookup"><span data-stu-id="7014f-212">There is no API to read material from an IKeyEscrowSink instance.</span></span> <span data-ttu-id="7014f-213">Dies ist konsistent mit der Theorie zum Entwurf des hinterlegte angibt: beabsichtigt hat das Schlüsselmaterial einer vertrauenswürdigen Zertifizierungsstelle zu ermöglichen, und seit die Anwendung selbst keiner vertrauenswürdigen Zertifizierungsstelle ist, er keinen Zugriff haben sollten eigene escrowed Material.</span><span class="sxs-lookup"><span data-stu-id="7014f-213">This is consistent with the design theory of the escrow mechanism: it's intended to make the key material accessible to a trusted authority, and since the application is itself not a trusted authority, it shouldn't have access to its own escrowed material.</span></span>

<span data-ttu-id="7014f-214">Der folgende Beispielcode veranschaulicht das Erstellen und registrieren eine IKeyEscrowSink, in dem Schlüssel hinterlegt werden, dass nur Mitglieder der "CONTOSODomain-Admins" wiederhergestellt werden können.</span><span class="sxs-lookup"><span data-stu-id="7014f-214">The following sample code demonstrates creating and registering an IKeyEscrowSink where keys are escrowed such that only members of "CONTOSODomain Admins" can recover them.</span></span>

> [!NOTE]
> <span data-ttu-id="7014f-215">Um dieses Beispiel ausführen zu können, muss auf eine Domäne eingebundenen Windows 8 / Windows Server 2012-Computer und dem Domänencontroller muss WindowsServer 2012 oder höher sein.</span><span class="sxs-lookup"><span data-stu-id="7014f-215">To run this sample, you must be on a domain-joined Windows 8 / Windows Server 2012 machine, and the domain controller must be Windows Server 2012 or later.</span></span>

<span data-ttu-id="7014f-216">[!code-none[Main](key-management/samples/key-management-extensibility.cs)]</span><span class="sxs-lookup"><span data-stu-id="7014f-216">[!code-none[Main](key-management/samples/key-management-extensibility.cs)]</span></span>
