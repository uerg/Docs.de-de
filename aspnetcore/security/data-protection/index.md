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
ms.openlocfilehash: e08dea63f012c4a758f2e5561c4930d09cfee0ac
ms.sourcegitcommit: 493a215355576cfa481773365de021bcf04bb9c7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/15/2018
---
# <a name="data-protection-in-aspnet-core-consumer-apis-configuration-extensibility-apis-and-implementation"></a><span data-ttu-id="40583-103">Schutz von Daten in ASP.NET Core: Consumer-APIs, Konfiguration, Erweiterbarkeits-APIs und Implementierung</span><span class="sxs-lookup"><span data-stu-id="40583-103">Data Protection in ASP.NET Core: Consumer APIs, configuration, extensibility APIs and implementation</span></span>

* [<span data-ttu-id="40583-104">Einführung in den Schutz von Daten</span><span class="sxs-lookup"><span data-stu-id="40583-104">Introduction to data protection</span></span>](introduction.md)

* [<span data-ttu-id="40583-105">Erste Schritte mit Datenschutz-APIs</span><span class="sxs-lookup"><span data-stu-id="40583-105">Get started with the Data Protection APIs</span></span>](using-data-protection.md)

* [<span data-ttu-id="40583-106">Consumer-APIs</span><span class="sxs-lookup"><span data-stu-id="40583-106">Consumer APIs</span></span>](consumer-apis/index.md)

  * [<span data-ttu-id="40583-107">Übersicht über Consumer-APIs</span><span class="sxs-lookup"><span data-stu-id="40583-107">Consumer APIs overview</span></span>](consumer-apis/overview.md)

  * [<span data-ttu-id="40583-108">purpose-Zeichenfolgen</span><span class="sxs-lookup"><span data-stu-id="40583-108">Purpose strings</span></span>](consumer-apis/purpose-strings.md)

  * [<span data-ttu-id="40583-109">Zweckhierarchie und Mehrinstanzenfähigkeit</span><span class="sxs-lookup"><span data-stu-id="40583-109">Purpose hierarchy and multi-tenancy</span></span>](consumer-apis/purpose-strings-multitenancy.md)

  * [<span data-ttu-id="40583-110">Kennworthashing</span><span class="sxs-lookup"><span data-stu-id="40583-110">Password hashing</span></span>](consumer-apis/password-hashing.md)

  * [<span data-ttu-id="40583-111">Beschränken der Lebensdauer von geschützten Nutzlasten</span><span class="sxs-lookup"><span data-stu-id="40583-111">Limiting the lifetime of protected payloads</span></span>](consumer-apis/limited-lifetime-payloads.md)

  * [<span data-ttu-id="40583-112">Aufheben des Schutzes von Nutzlasten, deren Schlüssel gesperrt wurden</span><span class="sxs-lookup"><span data-stu-id="40583-112">Unprotecting payloads whose keys have been revoked</span></span>](consumer-apis/dangerous-unprotect.md)

* [<span data-ttu-id="40583-113">Konfiguration</span><span class="sxs-lookup"><span data-stu-id="40583-113">Configuration</span></span>](configuration/index.md)

  * [<span data-ttu-id="40583-114">Konfigurieren des Schutzes von Daten</span><span class="sxs-lookup"><span data-stu-id="40583-114">Configuring data protection</span></span>](configuration/overview.md)

  * [<span data-ttu-id="40583-115">Standardeinstellungen</span><span class="sxs-lookup"><span data-stu-id="40583-115">Default settings</span></span>](configuration/default-settings.md)

  * [<span data-ttu-id="40583-116">Computerweite Richtlinie</span><span class="sxs-lookup"><span data-stu-id="40583-116">Machine-wide policy</span></span>](configuration/machine-wide-policy.md)

  * [<span data-ttu-id="40583-117">Szenarios ohne Möglichkeiten zur Abhängigkeitsinjektion</span><span class="sxs-lookup"><span data-stu-id="40583-117">Non DI-aware scenarios</span></span>](configuration/non-di-scenarios.md)

* [<span data-ttu-id="40583-118">Erweiterbarkeits-APIs</span><span class="sxs-lookup"><span data-stu-id="40583-118">Extensibility APIs</span></span>](extensibility/index.md)

  * [<span data-ttu-id="40583-119">Kryptografieerweiterbarkeit in Core</span><span class="sxs-lookup"><span data-stu-id="40583-119">Core cryptography extensibility</span></span>](extensibility/core-crypto.md)

  * [<span data-ttu-id="40583-120">Schlüsselverwaltungserweiterbarkeit</span><span class="sxs-lookup"><span data-stu-id="40583-120">Key management extensibility</span></span>](extensibility/key-management.md)

  * [<span data-ttu-id="40583-121">Verschiedene APIs</span><span class="sxs-lookup"><span data-stu-id="40583-121">Miscellaneous APIs</span></span>](extensibility/misc-apis.md)

* [<span data-ttu-id="40583-122">Implementation (Implementierung)</span><span class="sxs-lookup"><span data-stu-id="40583-122">Implementation</span></span>](implementation/index.md)

  * [<span data-ttu-id="40583-123">Authentifizierte Verschlüsselungsdetails</span><span class="sxs-lookup"><span data-stu-id="40583-123">Authenticated encryption details</span></span>](implementation/authenticated-encryption-details.md)

  * [<span data-ttu-id="40583-124">Unterschlüsselableitung und authentifizierte Verschlüsselung</span><span class="sxs-lookup"><span data-stu-id="40583-124">Subkey derivation and authenticated encryption</span></span>](implementation/subkeyderivation.md)

  * [<span data-ttu-id="40583-125">Kontextheader</span><span class="sxs-lookup"><span data-stu-id="40583-125">Context headers</span></span>](implementation/context-headers.md)

  * [<span data-ttu-id="40583-126">Schlüsselverwaltung</span><span class="sxs-lookup"><span data-stu-id="40583-126">Key management</span></span>](implementation/key-management.md)

  * [<span data-ttu-id="40583-127">Schlüsselspeicheranbieter</span><span class="sxs-lookup"><span data-stu-id="40583-127">Key storage providers</span></span>](implementation/key-storage-providers.md)

  * [<span data-ttu-id="40583-128">Verschlüsselung ruhender Daten mit Schlüsseln</span><span class="sxs-lookup"><span data-stu-id="40583-128">Key encryption at rest</span></span>](implementation/key-encryption-at-rest.md)

  * [<span data-ttu-id="40583-129">Unveränderlichkeit von Schlüsseln und Ändern von Einstellungen</span><span class="sxs-lookup"><span data-stu-id="40583-129">Key immutability and changing settings</span></span>](implementation/key-immutability.md)

  * [<span data-ttu-id="40583-130">Schlüsselspeicherformat</span><span class="sxs-lookup"><span data-stu-id="40583-130">Key storage format</span></span>](implementation/key-storage-format.md)

  * [<span data-ttu-id="40583-131">Kurzlebige Datenschutzanbieter</span><span class="sxs-lookup"><span data-stu-id="40583-131">Ephemeral data protection providers</span></span>](implementation/key-storage-ephemeral.md)

* [<span data-ttu-id="40583-132">Kompatibilität</span><span class="sxs-lookup"><span data-stu-id="40583-132">Compatibility</span></span>](compatibility/index.md)

  * [<span data-ttu-id="40583-133">Ersetzen von <machineKey> in ASP.NET</span><span class="sxs-lookup"><span data-stu-id="40583-133">Replacing <machineKey> in ASP.NET</span></span>](xref:security/data-protection/compatibility/replacing-machinekey)
