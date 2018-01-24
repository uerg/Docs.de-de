---
title: Schutz von Daten in ASP.NET Core
author: rick-anderson
description: "Dieses Dokument dient als Inhaltsverzeichnis für die verschiedenen Themen zum Schutz von Daten in ASP.NET Core."
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/index
ms.openlocfilehash: 151385964d877fc9eadaa219320e5f5a195164e4
ms.sourcegitcommit: 3f491f887074310fc0f145cd01a670aa63b969e3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/22/2018
---
# <a name="data-protection-in-aspnet-core-consumer-apis-configuration-extensibility-apis-and-implementation"></a><span data-ttu-id="55a68-103">Schutz von Daten in ASP.NET Core: Consumer-APIs, Konfiguration, Erweiterbarkeits-APIs und Implementierung</span><span class="sxs-lookup"><span data-stu-id="55a68-103">Data Protection in ASP.NET Core: Consumer APIs, configuration, extensibility APIs and implementation</span></span>

* [<span data-ttu-id="55a68-104">Einführung in den Schutz von Daten</span><span class="sxs-lookup"><span data-stu-id="55a68-104">Introduction to data protection</span></span>](introduction.md)

* [<span data-ttu-id="55a68-105">Erste Schritte mit Datenschutz-APIs</span><span class="sxs-lookup"><span data-stu-id="55a68-105">Get started with the Data Protection APIs</span></span>](using-data-protection.md)

* [<span data-ttu-id="55a68-106">Consumer-APIs</span><span class="sxs-lookup"><span data-stu-id="55a68-106">Consumer APIs</span></span>](consumer-apis/index.md)

  * [<span data-ttu-id="55a68-107">Übersicht über Consumer-APIs</span><span class="sxs-lookup"><span data-stu-id="55a68-107">Consumer APIs overview</span></span>](consumer-apis/overview.md)

  * [<span data-ttu-id="55a68-108">purpose-Zeichenfolgen</span><span class="sxs-lookup"><span data-stu-id="55a68-108">Purpose strings</span></span>](consumer-apis/purpose-strings.md)

  * [<span data-ttu-id="55a68-109">Zweckhierarchie und Mehrinstanzenfähigkeit</span><span class="sxs-lookup"><span data-stu-id="55a68-109">Purpose hierarchy and multi-tenancy</span></span>](consumer-apis/purpose-strings-multitenancy.md)

  * [<span data-ttu-id="55a68-110">Kennworthashing</span><span class="sxs-lookup"><span data-stu-id="55a68-110">Password hashing</span></span>](consumer-apis/password-hashing.md)

  * [<span data-ttu-id="55a68-111">Beschränken der Lebensdauer von geschützten Nutzlasten</span><span class="sxs-lookup"><span data-stu-id="55a68-111">Limiting the lifetime of protected payloads</span></span>](consumer-apis/limited-lifetime-payloads.md)

  * [<span data-ttu-id="55a68-112">Aufheben des Schutzes von Nutzlasten, deren Schlüssel gesperrt wurden</span><span class="sxs-lookup"><span data-stu-id="55a68-112">Unprotecting payloads whose keys have been revoked</span></span>](consumer-apis/dangerous-unprotect.md)

* [<span data-ttu-id="55a68-113">Konfiguration</span><span class="sxs-lookup"><span data-stu-id="55a68-113">Configuration</span></span>](configuration/index.md)

  * [<span data-ttu-id="55a68-114">Konfigurieren des Schutzes von Daten</span><span class="sxs-lookup"><span data-stu-id="55a68-114">Configuring data protection</span></span>](configuration/overview.md)

  * [<span data-ttu-id="55a68-115">Standardeinstellungen</span><span class="sxs-lookup"><span data-stu-id="55a68-115">Default settings</span></span>](configuration/default-settings.md)

  * [<span data-ttu-id="55a68-116">Computerweite Richtlinie</span><span class="sxs-lookup"><span data-stu-id="55a68-116">Machine-wide policy</span></span>](configuration/machine-wide-policy.md)

  * [<span data-ttu-id="55a68-117">Szenarios ohne Möglichkeiten zur Abhängigkeitsinjektion</span><span class="sxs-lookup"><span data-stu-id="55a68-117">Non DI-aware scenarios</span></span>](configuration/non-di-scenarios.md)

* [<span data-ttu-id="55a68-118">Erweiterbarkeits-APIs</span><span class="sxs-lookup"><span data-stu-id="55a68-118">Extensibility APIs</span></span>](extensibility/index.md)

  * [<span data-ttu-id="55a68-119">Kryptografieerweiterbarkeit in Core</span><span class="sxs-lookup"><span data-stu-id="55a68-119">Core cryptography extensibility</span></span>](extensibility/core-crypto.md)

  * [<span data-ttu-id="55a68-120">Schlüsselverwaltungserweiterbarkeit</span><span class="sxs-lookup"><span data-stu-id="55a68-120">Key management extensibility</span></span>](extensibility/key-management.md)

  * [<span data-ttu-id="55a68-121">Verschiedene APIs</span><span class="sxs-lookup"><span data-stu-id="55a68-121">Miscellaneous APIs</span></span>](extensibility/misc-apis.md)

* [<span data-ttu-id="55a68-122">Implementation (Implementierung)</span><span class="sxs-lookup"><span data-stu-id="55a68-122">Implementation</span></span>](implementation/index.md)

  * [<span data-ttu-id="55a68-123">Authentifizierte Verschlüsselungsdetails</span><span class="sxs-lookup"><span data-stu-id="55a68-123">Authenticated encryption details</span></span>](implementation/authenticated-encryption-details.md)

  * [<span data-ttu-id="55a68-124">Unterschlüsselableitung und authentifizierte Verschlüsselung</span><span class="sxs-lookup"><span data-stu-id="55a68-124">Subkey derivation and authenticated encryption</span></span>](implementation/subkeyderivation.md)

  * [<span data-ttu-id="55a68-125">Kontextheader</span><span class="sxs-lookup"><span data-stu-id="55a68-125">Context headers</span></span>](implementation/context-headers.md)

  * [<span data-ttu-id="55a68-126">Schlüsselverwaltung</span><span class="sxs-lookup"><span data-stu-id="55a68-126">Key management</span></span>](implementation/key-management.md)

  * [<span data-ttu-id="55a68-127">Schlüsselspeicheranbieter</span><span class="sxs-lookup"><span data-stu-id="55a68-127">Key storage providers</span></span>](implementation/key-storage-providers.md)

  * [<span data-ttu-id="55a68-128">Verschlüsselung ruhender Daten mit Schlüsseln</span><span class="sxs-lookup"><span data-stu-id="55a68-128">Key encryption at rest</span></span>](implementation/key-encryption-at-rest.md)

  * [<span data-ttu-id="55a68-129">Unveränderlichkeit von Schlüsseln und Ändern von Einstellungen</span><span class="sxs-lookup"><span data-stu-id="55a68-129">Key immutability and changing settings</span></span>](implementation/key-immutability.md)

  * [<span data-ttu-id="55a68-130">Schlüsselspeicherformat</span><span class="sxs-lookup"><span data-stu-id="55a68-130">Key storage format</span></span>](implementation/key-storage-format.md)

  * [<span data-ttu-id="55a68-131">Kurzlebige Datenschutzanbieter</span><span class="sxs-lookup"><span data-stu-id="55a68-131">Ephemeral data protection providers</span></span>](implementation/key-storage-ephemeral.md)

* [<span data-ttu-id="55a68-132">Kompatibilität</span><span class="sxs-lookup"><span data-stu-id="55a68-132">Compatibility</span></span>](compatibility/index.md)

  * [<span data-ttu-id="55a68-133">Freigeben von Cookies für mehrere Apps</span><span class="sxs-lookup"><span data-stu-id="55a68-133">Sharing cookies among apps</span></span>](xref:security/data-protection/compatibility/cookie-sharing)

  * [<span data-ttu-id="55a68-134">Ersetzen von <machineKey> in ASP.NET</span><span class="sxs-lookup"><span data-stu-id="55a68-134">Replacing <machineKey> in ASP.NET</span></span>](xref:security/data-protection/compatibility/replacing-machinekey)
