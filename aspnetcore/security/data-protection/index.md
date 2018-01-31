---
title: Schutz von Daten in ASP.NET Core
author: rick-anderson
description: "Dieses Dokument dient als Inhaltsverzeichnis für die verschiedenen Themen zum Schutz von Daten in ASP.NET Core."
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/data-protection/index
ms.openlocfilehash: b846fb7cb28eeceb8c0bdb47135e1cf014ae08a7
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2018
---
# <a name="data-protection-in-aspnet-core-consumer-apis-configuration-extensibility-apis-and-implementation"></a><span data-ttu-id="a9c8b-103">Schutz von Daten in ASP.NET Core: Consumer-APIs, Konfiguration, Erweiterbarkeits-APIs und Implementierung</span><span class="sxs-lookup"><span data-stu-id="a9c8b-103">Data Protection in ASP.NET Core: Consumer APIs, configuration, extensibility APIs and implementation</span></span>

* [<span data-ttu-id="a9c8b-104">Einführung in den Schutz von Daten</span><span class="sxs-lookup"><span data-stu-id="a9c8b-104">Introduction to data protection</span></span>](introduction.md)

* [<span data-ttu-id="a9c8b-105">Erste Schritte mit Datenschutz-APIs</span><span class="sxs-lookup"><span data-stu-id="a9c8b-105">Get started with the Data Protection APIs</span></span>](using-data-protection.md)

* [<span data-ttu-id="a9c8b-106">Consumer-APIs</span><span class="sxs-lookup"><span data-stu-id="a9c8b-106">Consumer APIs</span></span>](consumer-apis/index.md)

  * [<span data-ttu-id="a9c8b-107">Übersicht über Consumer-APIs</span><span class="sxs-lookup"><span data-stu-id="a9c8b-107">Consumer APIs overview</span></span>](consumer-apis/overview.md)

  * [<span data-ttu-id="a9c8b-108">purpose-Zeichenfolgen</span><span class="sxs-lookup"><span data-stu-id="a9c8b-108">Purpose strings</span></span>](consumer-apis/purpose-strings.md)

  * [<span data-ttu-id="a9c8b-109">Zweckhierarchie und Mehrinstanzenfähigkeit</span><span class="sxs-lookup"><span data-stu-id="a9c8b-109">Purpose hierarchy and multi-tenancy</span></span>](consumer-apis/purpose-strings-multitenancy.md)

  * [<span data-ttu-id="a9c8b-110">Kennworthashing</span><span class="sxs-lookup"><span data-stu-id="a9c8b-110">Password hashing</span></span>](consumer-apis/password-hashing.md)

  * [<span data-ttu-id="a9c8b-111">Beschränken der Lebensdauer von geschützten Nutzlasten</span><span class="sxs-lookup"><span data-stu-id="a9c8b-111">Limiting the lifetime of protected payloads</span></span>](consumer-apis/limited-lifetime-payloads.md)

  * [<span data-ttu-id="a9c8b-112">Aufheben des Schutzes von Nutzlasten, deren Schlüssel gesperrt wurden</span><span class="sxs-lookup"><span data-stu-id="a9c8b-112">Unprotecting payloads whose keys have been revoked</span></span>](consumer-apis/dangerous-unprotect.md)

* [<span data-ttu-id="a9c8b-113">Konfiguration</span><span class="sxs-lookup"><span data-stu-id="a9c8b-113">Configuration</span></span>](configuration/index.md)

  * [<span data-ttu-id="a9c8b-114">Konfigurieren des Schutzes von Daten</span><span class="sxs-lookup"><span data-stu-id="a9c8b-114">Configuring data protection</span></span>](configuration/overview.md)

  * [<span data-ttu-id="a9c8b-115">Standardeinstellungen</span><span class="sxs-lookup"><span data-stu-id="a9c8b-115">Default settings</span></span>](configuration/default-settings.md)

  * [<span data-ttu-id="a9c8b-116">Computerweite Richtlinie</span><span class="sxs-lookup"><span data-stu-id="a9c8b-116">Machine-wide policy</span></span>](configuration/machine-wide-policy.md)

  * [<span data-ttu-id="a9c8b-117">Szenarios ohne Möglichkeiten zur Abhängigkeitsinjektion</span><span class="sxs-lookup"><span data-stu-id="a9c8b-117">Non DI-aware scenarios</span></span>](configuration/non-di-scenarios.md)

* [<span data-ttu-id="a9c8b-118">Erweiterbarkeits-APIs</span><span class="sxs-lookup"><span data-stu-id="a9c8b-118">Extensibility APIs</span></span>](extensibility/index.md)

  * [<span data-ttu-id="a9c8b-119">Kryptografieerweiterbarkeit in Core</span><span class="sxs-lookup"><span data-stu-id="a9c8b-119">Core cryptography extensibility</span></span>](extensibility/core-crypto.md)

  * [<span data-ttu-id="a9c8b-120">Schlüsselverwaltungserweiterbarkeit</span><span class="sxs-lookup"><span data-stu-id="a9c8b-120">Key management extensibility</span></span>](extensibility/key-management.md)

  * [<span data-ttu-id="a9c8b-121">Verschiedene APIs</span><span class="sxs-lookup"><span data-stu-id="a9c8b-121">Miscellaneous APIs</span></span>](extensibility/misc-apis.md)

* [<span data-ttu-id="a9c8b-122">Implementation (Implementierung)</span><span class="sxs-lookup"><span data-stu-id="a9c8b-122">Implementation</span></span>](implementation/index.md)

  * [<span data-ttu-id="a9c8b-123">Authentifizierte Verschlüsselungsdetails</span><span class="sxs-lookup"><span data-stu-id="a9c8b-123">Authenticated encryption details</span></span>](implementation/authenticated-encryption-details.md)

  * [<span data-ttu-id="a9c8b-124">Unterschlüsselableitung und authentifizierte Verschlüsselung</span><span class="sxs-lookup"><span data-stu-id="a9c8b-124">Subkey derivation and authenticated encryption</span></span>](implementation/subkeyderivation.md)

  * [<span data-ttu-id="a9c8b-125">Kontextheader</span><span class="sxs-lookup"><span data-stu-id="a9c8b-125">Context headers</span></span>](implementation/context-headers.md)

  * [<span data-ttu-id="a9c8b-126">Schlüsselverwaltung</span><span class="sxs-lookup"><span data-stu-id="a9c8b-126">Key management</span></span>](implementation/key-management.md)

  * [<span data-ttu-id="a9c8b-127">Schlüsselspeicheranbieter</span><span class="sxs-lookup"><span data-stu-id="a9c8b-127">Key storage providers</span></span>](implementation/key-storage-providers.md)

  * [<span data-ttu-id="a9c8b-128">Verschlüsselung ruhender Daten mit Schlüsseln</span><span class="sxs-lookup"><span data-stu-id="a9c8b-128">Key encryption at rest</span></span>](implementation/key-encryption-at-rest.md)

  * [<span data-ttu-id="a9c8b-129">Unveränderlichkeit von Schlüsseln und Ändern von Einstellungen</span><span class="sxs-lookup"><span data-stu-id="a9c8b-129">Key immutability and changing settings</span></span>](implementation/key-immutability.md)

  * [<span data-ttu-id="a9c8b-130">Schlüsselspeicherformat</span><span class="sxs-lookup"><span data-stu-id="a9c8b-130">Key storage format</span></span>](implementation/key-storage-format.md)

  * [<span data-ttu-id="a9c8b-131">Kurzlebige Datenschutzanbieter</span><span class="sxs-lookup"><span data-stu-id="a9c8b-131">Ephemeral data protection providers</span></span>](implementation/key-storage-ephemeral.md)

* [<span data-ttu-id="a9c8b-132">Kompatibilität</span><span class="sxs-lookup"><span data-stu-id="a9c8b-132">Compatibility</span></span>](compatibility/index.md)

  * [<span data-ttu-id="a9c8b-133">Freigeben von Cookies für mehrere Apps</span><span class="sxs-lookup"><span data-stu-id="a9c8b-133">Sharing cookies among apps</span></span>](xref:security/data-protection/compatibility/cookie-sharing)

  * [<span data-ttu-id="a9c8b-134">Ersetzen von <machineKey> in ASP.NET</span><span class="sxs-lookup"><span data-stu-id="a9c8b-134">Replacing <machineKey> in ASP.NET</span></span>](xref:security/data-protection/compatibility/replacing-machinekey)
