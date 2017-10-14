---
title: "Schlüsselverwaltungsdienst-Erweiterbarkeit"
author: rick-anderson
description: 
keywords: ASP.NET Core,
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 3606b251-8324-4485-8d52-582a2cd5cffb
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/extensibility/key-management
ms.openlocfilehash: ce23931e72404347ebc17c69ae90e70cd15328bc
ms.sourcegitcommit: 8f4d4fad1ca27adf9e396f5c205c9875a3963664
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/13/2017
---
# <a name="key-management-extensibility"></a>Schlüsselverwaltungsdienst-Erweiterbarkeit

<a name="data-protection-extensibility-key-management"></a>

>[!TIP]
> Lesen der [schlüsselverwaltung](../implementation/key-management.md#data-protection-implementation-key-management) Abschnitt vor dem Lesen in diesem Abschnitt, wie einige der grundlegenden Konzepte hinter dieser APIs erläutert wird.

>[!WARNING]
> Typen, die jede der folgenden Schnittstellen implementieren, sollten threadsichere werden für mehrere Aufrufer.

## <a name="key"></a>Key

Die IKey-Schnittstelle ist die grundlegende Darstellung eines Schlüssels in Kryptografiesystem. Der Begriff Schlüssel ist hier insofern abstrakte, nicht in der struktureller von "kryptografischen schlüsselmaterialien" verwendet. Ein Schlüssel hat die folgenden Eigenschaften:

* Aktivierung, Erstellung und Ablaufdaten

* Status der Zertifikatsperre

* Schlüsselbezeichner (eine GUID)

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Darüber hinaus macht IKey eine CreateEncryptor-Methode, die verwendet werden kann, erstellen eine [IAuthenticatedEncryptor](core-crypto.md#data-protection-extensibility-core-crypto-iauthenticatedencryptor) Instanz gebunden werden, um diesen Schlüssel.

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Darüber hinaus macht IKey eine CreateEncryptorInstance-Methode, die verwendet werden kann, erstellen eine [IAuthenticatedEncryptor](core-crypto.md#data-protection-extensibility-core-crypto-iauthenticatedencryptor) Instanz gebunden werden, um diesen Schlüssel.

---

> [!NOTE]
> Es ist keine API das unformatierte kryptografische Material aus einer IKey-Instanz abgerufen.

## <a name="ikeymanager"></a>IKeyManager

Die IKeyManager-Schnittstelle stellt ein Objekt, das verantwortlich für allgemeine Schlüsselspeicher, Abruf und Manipulation dar. Macht drei Vorgänge auf höherer Ebene:

* Erstellen Sie einen neuen Schlüssel, und klicken Sie in Speicher gespeichert.

* Alle Schlüssel aus dem Speicher abrufen.

* Einen oder mehrere Schlüssel widerrufen, und die Sperrinformationen in den Speicher beibehalten.

>[!WARNING]
> Schreiben einer IKeyManager ist eine sehr komplexe Aufgabe und die meisten Entwickler sollten nicht versuchen. Stattdessen die meisten Entwickler sollten nutzen die Funktionen von Angeboten die [XmlKeyManager](xref:security/data-protection/extensibility/key-management#data-protection-extensibility-key-management-xmlkeymanager) Klasse.

<a name="data-protection-extensibility-key-management-xmlkeymanager"></a>

## <a name="xmlkeymanager"></a>XmlKeyManager

Der XmlKeyManager-Typ ist die konkrete Implementierung der mitgelieferten IKeyManager. Er bietet mehrere nützliche Funktionen, einschließlich schlüsselhinterlegung und Verschlüsselung von Schlüsseln im Ruhezustand. Schlüssel in diesem System werden als XML-Elementen dargestellt (insbesondere ["XElement"](https://docs.microsoft.com/dotnet/csharp/programming-guide/concepts/linq/xelement-class-overview).

XmlKeyManager hängt davon ab, auf mehrere andere Komponenten im Verlauf die Aufgaben erfüllen:

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

* AlgorithmConfiguration, der bestimmt, die durch neue Schlüssel verwendeten Algorithmen.

* IXmlRepository, welche steuert, wo der Schlüssel im Speicher beibehalten werden.

* IXmlEncryptor [optional] die ermöglicht das Verschlüsseln der Schlüssel im Ruhezustand.

* IKeyEscrowSink [optional] die schlüsselhinterlegung Dienste bereitstellt.

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

* IXmlRepository, welche steuert, wo der Schlüssel im Speicher beibehalten werden.

* IXmlEncryptor [optional] die ermöglicht das Verschlüsseln der Schlüssel im Ruhezustand.

* IKeyEscrowSink [optional] die schlüsselhinterlegung Dienste bereitstellt.

---

Im folgenden finden Sie übersichtliche Diagramme, die angibt, wie diese Komponenten zusammen in XmlKeyManager verbunden sind.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

   ![Schlüssel erstellen](key-management/_static/keycreation2.png)

   *Schlüssels Erstellung / CreateNewKey*

In der Implementierung von CreateNewKey wird die AlgorithmConfiguration-Komponente verwendet, eine eindeutige IAuthenticatedEncryptorDescriptor zu erstellen, die dann als XML serialisiert wird. Wenn die Senke schlüsselhinterlegung vorhanden ist, wird der XML-Rohdaten (unverschlüsselt) für die langfristige Speicherung an die Senke bereitgestellt. Der nicht verschlüsselte XML-Code wird dann über eine IXmlEncryptor ausgeführt (falls erforderlich) um das verschlüsselte XML-Dokument zu generieren. Dieses verschlüsselte Dokument wird in langfristigen Speicher über die IXmlRepository beibehalten. (Wenn keine IXmlEncryptor konfiguriert ist, werden in der IXmlRepository unverschlüsselte Dokument beibehalten.)

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

   ![Schlüssel erstellen](key-management/_static/keycreation1.png)

   *Schlüssels Erstellung / CreateNewKey*

In der Implementierung von CreateNewKey wird die IAuthenticatedEncryptorConfiguration-Komponente verwendet, eine eindeutige IAuthenticatedEncryptorDescriptor zu erstellen, die dann als XML serialisiert wird. Wenn die Senke schlüsselhinterlegung vorhanden ist, wird der XML-Rohdaten (unverschlüsselt) für die langfristige Speicherung an die Senke bereitgestellt. Der nicht verschlüsselte XML-Code wird dann über eine IXmlEncryptor ausgeführt (falls erforderlich) um das verschlüsselte XML-Dokument zu generieren. Dieses verschlüsselte Dokument wird in langfristigen Speicher über die IXmlRepository beibehalten. (Wenn keine IXmlEncryptor konfiguriert ist, werden in der IXmlRepository unverschlüsselte Dokument beibehalten.)

---

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

   ![Abruf der](key-management/_static/keyretrieval2.png)
   
# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

   ![Abruf der](key-management/_static/keyretrieval1.png)

---

   *Abrufen von Schlüssel / GetAllKeys*

In der Implementierung der GetAllKeys werden die XML-Dokumente, die Schlüssel und Revocations darstellen aus der zugrunde liegenden IXmlRepository gelesen. Wenn diese Dokumente verschlüsselt sind, wird das System diese automatisch entschlüsselt. XmlKeyManager erstellt die entsprechenden IAuthenticatedEncryptorDescriptorDeserializer-Instanzen, um die Dokumente wieder in Instanzen IAuthenticatedEncryptorDescriptor, Deserialisieren der einzelnen IKey Instanzen umschlossen sind. Diese Auflistung von Instanzen IKey wird an den Aufrufer zurückgegeben.

Weitere Informationen zu bestimmten XML-Elemente finden Sie der [Schlüsselspeicher Format Dokument](../implementation/key-storage-format.md#data-protection-implementation-key-storage-format).

## <a name="ixmlrepository"></a>IXmlRepository

Die IXmlRepository-Schnittstelle stellt einen Typ, der dauerhaft zu XML und XML-Daten aus einem Sicherungsspeicher abrufen kann. Macht zwei APIs:

* GetAllElements(): IReadOnlyCollection<XElement>

* StoreElement ("XElement"-Element, FriendlyName Zeichenfolge)

Implementierungen von IXmlRepository müssen nicht den Umweg über diese XML-Code zu analysieren. Sie sollten die XML-Dokumente als nicht transparent behandelt und höhere Ebenen Gedanken machen, generieren und die Dokumente analysieren können.

Es gibt zwei integrierte konkrete Typen die IXmlRepository implementieren: FileSystemXmlRepository und RegistryXmlRepository. Finden Sie unter der [softwareschlüsselspeicher-Anbieter Dokument](../implementation/key-storage-providers.md#data-protection-implementation-key-storage-providers) für Weitere Informationen. Registrieren eine benutzerdefinierte IXmlRepository würde die geeignete Weise zu einem anderen Speicher, z. B. Azure Blob-Speicher sichern. Um die Standard-Repository eine anwendungsweite zu ändern, registrieren Sie benutzerdefinierte Singleton IXmlRepository in der Dienstanbieter.

<a name="data-protection-extensibility-key-management-ixmlencryptor"></a>

## <a name="ixmlencryptor"></a>IXmlEncryptor

Die IXmlEncryptor-Schnittstelle stellt einen Typ, der eine nur-Text-XML-Element verschlüsseln kann. Es stellt eine einzelne API bereit:

* ("XElement" PlaintextElement) zu verschlüsseln: EncryptedXmlInfo

Enthält einen serialisierten IAuthenticatedEncryptorDescriptor alle Elemente, die als "Verschlüsselung erfordert" gekennzeichnet, dann XmlKeyManager ausgeführt, diese Elemente über den konfigurierten IXmlEncryptor Encrypt-Methode, und bleiben enciphered Element stattdessen als den nur-Text-Element, das die IXmlRepository. Die Ausgabe der Encrypt-Methode ist ein EncryptedXmlInfo-Objekt. Dieses Objekt ist ein Wrapper enthält das resultierende enciphered "XElement" und der Typ, der eine IXmlDecryptor, die verwendet werden kann darstellt, um das entsprechende Element zu entschlüsseln.

Es gibt vier integrierte konkrete Typen die IXmlEncryptor implementieren: CertificateXmlEncryptor, DpapiNGXmlEncryptor DpapiXmlEncryptor und NullXmlEncryptor. Finden Sie unter der [Schlüsselverschlüsselung auf Rest Dokumentbibliotheks-](../implementation/key-encryption-at-rest.md#data-protection-implementation-key-encryption-at-rest) für Weitere Informationen. Um den Schlüssel Verschlüsselung auf Rest Standardmechanismus anwendungsweite zu ändern, registrieren Sie benutzerdefinierte Singleton IXmlEncryptor in der Dienstanbieter.

## <a name="ixmldecryptor"></a>IXmlDecryptor

Die IXmlDecryptor-Schnittstelle stellt einen Typ, der weiß, wie ein "XElement" entschlüsseln, die über eine IXmlEncryptor enciphered wurde. Es stellt eine einzelne API bereit:

* Entschlüsseln ("XElement" EncryptedElement): "XElement"

Die Decrypt-Methode macht die Verschlüsselung von IXmlEncryptor.Encrypt ausgeführt. Jede konkrete Implementierung der IXmlEncryptor müssen im Allgemeinen eine entsprechende konkrete IXmlDecryptor-Implementierung.

Typen, die IXmlDecryptor implementieren müssen eine der folgenden zwei öffentlichen Konstruktoren aufweisen:

* .ctor(IServiceProvider)

* .ctor()

> [!NOTE]
> Der an den Konstruktor übergebene IServiceProvider kann null sein.

## <a name="ikeyescrowsink"></a>IKeyEscrowSink

Die IKeyEscrowSink-Schnittstelle stellt einen Typ, der hinterlegte vertraulicher Informationen ausführen kann. Beachten Sie, dass die serialisierte Deskriptoren enthalten möglicherweise vertrauliche Informationen (z. B. das kryptografische Material), und dies ist, was mit der Einführung des geführt hat die [IXmlEncryptor](xref:security/data-protection/extensibility/key-management#data-protection-extensibility-key-management-ixmlencryptor) ursprünglich geben. Allerdings Unfall auftreten und Schlüsselbunde können gelöscht oder beschädigt ist.

Die hinterlegte-Schnittstelle bietet eine Notfall Escape-Schraffur, ermöglicht den Zugriff auf die unformatierten serialisierten XML-Code vor dem Transformieren von keiner konfiguriert ist [IXmlEncryptor](xref:security/data-protection/extensibility/key-management#data-protection-extensibility-key-management-ixmlencryptor). Die Schnittstelle macht eine einzige API verfügbar:

* Speicher (Guid KeyId, "XElement"-Element)

Es ist Aufgabe der IKeyEscrowSink-Implementierung, die das bereitgestellte Element auf sichere Weise mit Geschäftsrichtlinie konsistent behandeln. Eine mögliche Implementierung konnte für die Senke werden zum Verschlüsseln von XML-Elements mit einem bekannten Unternehmens x. 509-Zertifikat sein, in dem der private Zertifikatschlüssel hinterlegt hat. Der Typ CertificateXmlEncryptor kann mit diesem unterstützen. Die Implementierung IKeyEscrowSink ist auch für das bereitgestellte Element entsprechend beibehalten zuständig.

Keine hinterlegte-Mechanismus ist standardmäßig aktiviert, wenn Server-Administratoren können [konfigurieren Sie diese Option Global](../configuration/machine-wide-policy.md#data-protection-configuration-machinewidepolicy). Sie können auch programmgesteuert programmiert werden über die *IDataProtectionBuilder.AddKeyEscrowSink* Methode, wie im folgenden Beispiel dargestellt. Die *AddKeyEscrowSink* Methode Überladungen spiegeln die *IServiceCollection.AddSingleton* und *IServiceCollection.AddInstance* Überladungen, die als IKeyEscrowSink Instanzen sollten Singletons verwendet werden. Wenn mehrere Instanzen von IKeyEscrowSink registriert sind, wird jeweils während der Generierung, aufgerufen werden, damit Schlüssel gleichzeitig auf mehrere Mechanismen hinterlegt werden können.

Es ist keine API zum Lesen von Materialien, die aus einer IKeyEscrowSink-Instanz. Dies ist konsistent mit der Theorie zum Entwurf des hinterlegte angibt: beabsichtigt hat das Schlüsselmaterial einer vertrauenswürdigen Zertifizierungsstelle zu ermöglichen, und seit die Anwendung selbst keiner vertrauenswürdigen Zertifizierungsstelle ist, er keinen Zugriff haben sollten eigene escrowed Material.

Der folgende Beispielcode veranschaulicht das Erstellen und registrieren eine IKeyEscrowSink, in dem Schlüssel hinterlegt werden, dass nur Mitglieder der "CONTOSODomain-Admins" wiederhergestellt werden können.

> [!NOTE]
> Um dieses Beispiel ausführen zu können, muss auf eine Domäne eingebundenen Windows 8 / Windows Server 2012-Computer und dem Domänencontroller muss WindowsServer 2012 oder höher sein.

[!code-none[Main](key-management/samples/key-management-extensibility.cs)]
