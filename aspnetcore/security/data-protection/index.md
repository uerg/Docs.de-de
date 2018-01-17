---
title: Schutz von Daten in ASP.NET Core
author: rick-anderson
description: "Dieses Dokument dient als Inhaltsverzeichnis für die verschiedenen Themen zum Schutz von Daten in ASP.NET Core."
keywords: ASP.NET Core,Schutz von Daten
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 1f402da8-1052-4970-9835-9f9f16a02dbc
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/index
ms.openlocfilehash: cbf18c6ec867fefec22980f3e3493562594ef72d
ms.sourcegitcommit: 12e5194936b7e820efc5505a2d5d4f84e88eb5ef
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2018
---
# <a name="data-protection-in-aspnet-core-consumer-apis-configuration-extensibility-apis-and-implementation"></a><span data-ttu-id="5fd14-104">Schutz von Daten in ASP.NET Core: Consumer-APIs, Konfiguration, Erweiterbarkeits-APIs und Implementierung</span><span class="sxs-lookup"><span data-stu-id="5fd14-104">Data Protection in ASP.NET Core: Consumer APIs, configuration, extensibility APIs and implementation</span></span>

* [<span data-ttu-id="5fd14-105">Einführung in den Schutz von Daten</span><span class="sxs-lookup"><span data-stu-id="5fd14-105">Introduction to data protection</span></span>](introduction.md)

* [<span data-ttu-id="5fd14-106">Erste Schritte mit Datenschutz-APIs</span><span class="sxs-lookup"><span data-stu-id="5fd14-106">Get started with the Data Protection APIs</span></span>](using-data-protection.md)

* [<span data-ttu-id="5fd14-107">Consumer-APIs</span><span class="sxs-lookup"><span data-stu-id="5fd14-107">Consumer APIs</span></span>](consumer-apis/index.md)

  * [<span data-ttu-id="5fd14-108">Übersicht über Consumer-APIs</span><span class="sxs-lookup"><span data-stu-id="5fd14-108">Consumer APIs overview</span></span>](consumer-apis/overview.md)

  * [<span data-ttu-id="5fd14-109">purpose-Zeichenfolgen</span><span class="sxs-lookup"><span data-stu-id="5fd14-109">Purpose strings</span></span>](consumer-apis/purpose-strings.md)

  * [<span data-ttu-id="5fd14-110">Zweckhierarchie und Mehrinstanzenfähigkeit</span><span class="sxs-lookup"><span data-stu-id="5fd14-110">Purpose hierarchy and multi-tenancy</span></span>](consumer-apis/purpose-strings-multitenancy.md)

  * [<span data-ttu-id="5fd14-111">Kennworthashing</span><span class="sxs-lookup"><span data-stu-id="5fd14-111">Password hashing</span></span>](consumer-apis/password-hashing.md)

  * [<span data-ttu-id="5fd14-112">Beschränken der Lebensdauer von geschützten Nutzlasten</span><span class="sxs-lookup"><span data-stu-id="5fd14-112">Limiting the lifetime of protected payloads</span></span>](consumer-apis/limited-lifetime-payloads.md)

  * [<span data-ttu-id="5fd14-113">Aufheben des Schutzes von Nutzlasten, deren Schlüssel gesperrt wurden</span><span class="sxs-lookup"><span data-stu-id="5fd14-113">Unprotecting payloads whose keys have been revoked</span></span>](consumer-apis/dangerous-unprotect.md)

* [<span data-ttu-id="5fd14-114">Konfiguration</span><span class="sxs-lookup"><span data-stu-id="5fd14-114">Configuration</span></span>](configuration/index.md)

  * [<span data-ttu-id="5fd14-115">Konfigurieren des Schutzes von Daten</span><span class="sxs-lookup"><span data-stu-id="5fd14-115">Configuring data protection</span></span>](configuration/overview.md)

  * [<span data-ttu-id="5fd14-116">Standardeinstellungen</span><span class="sxs-lookup"><span data-stu-id="5fd14-116">Default settings</span></span>](configuration/default-settings.md)

  * [<span data-ttu-id="5fd14-117">Computerweite Richtlinie</span><span class="sxs-lookup"><span data-stu-id="5fd14-117">Machine-wide policy</span></span>](configuration/machine-wide-policy.md)

  * [<span data-ttu-id="5fd14-118">Szenarios ohne Möglichkeiten zur Abhängigkeitsinjektion</span><span class="sxs-lookup"><span data-stu-id="5fd14-118">Non DI-aware scenarios</span></span>](configuration/non-di-scenarios.md)

* [<span data-ttu-id="5fd14-119">Erweiterbarkeits-APIs</span><span class="sxs-lookup"><span data-stu-id="5fd14-119">Extensibility APIs</span></span>](extensibility/index.md)

  * [<span data-ttu-id="5fd14-120">Kryptografieerweiterbarkeit in Core</span><span class="sxs-lookup"><span data-stu-id="5fd14-120">Core cryptography extensibility</span></span>](extensibility/core-crypto.md)

  * [<span data-ttu-id="5fd14-121">Schlüsselverwaltungserweiterbarkeit</span><span class="sxs-lookup"><span data-stu-id="5fd14-121">Key management extensibility</span></span>](extensibility/key-management.md)

  * [<span data-ttu-id="5fd14-122">Verschiedene APIs</span><span class="sxs-lookup"><span data-stu-id="5fd14-122">Miscellaneous APIs</span></span>](extensibility/misc-apis.md)

* [<span data-ttu-id="5fd14-123">Implementation (Implementierung)</span><span class="sxs-lookup"><span data-stu-id="5fd14-123">Implementation</span></span>](implementation/index.md)

  * [<span data-ttu-id="5fd14-124">Authentifizierte Verschlüsselungsdetails</span><span class="sxs-lookup"><span data-stu-id="5fd14-124">Authenticated encryption details</span></span>](implementation/authenticated-encryption-details.md)

  * [<span data-ttu-id="5fd14-125">Unterschlüsselableitung und authentifizierte Verschlüsselung</span><span class="sxs-lookup"><span data-stu-id="5fd14-125">Subkey derivation and authenticated encryption</span></span>](implementation/subkeyderivation.md)

  * [<span data-ttu-id="5fd14-126">Kontextheader</span><span class="sxs-lookup"><span data-stu-id="5fd14-126">Context headers</span></span>](implementation/context-headers.md)

  * [<span data-ttu-id="5fd14-127">Schlüsselverwaltung</span><span class="sxs-lookup"><span data-stu-id="5fd14-127">Key management</span></span>](implementation/key-management.md)

  * [<span data-ttu-id="5fd14-128">Schlüsselspeicheranbieter</span><span class="sxs-lookup"><span data-stu-id="5fd14-128">Key storage providers</span></span>](implementation/key-storage-providers.md)

  * [<span data-ttu-id="5fd14-129">Verschlüsselung ruhender Daten mit Schlüsseln</span><span class="sxs-lookup"><span data-stu-id="5fd14-129">Key encryption at rest</span></span>](implementation/key-encryption-at-rest.md)

  * [<span data-ttu-id="5fd14-130">Unveränderlichkeit von Schlüsseln und Ändern von Einstellungen</span><span class="sxs-lookup"><span data-stu-id="5fd14-130">Key immutability and changing settings</span></span>](implementation/key-immutability.md)

  * [<span data-ttu-id="5fd14-131">Schlüsselspeicherformat</span><span class="sxs-lookup"><span data-stu-id="5fd14-131">Key storage format</span></span>](implementation/key-storage-format.md)

  * [<span data-ttu-id="5fd14-132">Kurzlebige Datenschutzanbieter</span><span class="sxs-lookup"><span data-stu-id="5fd14-132">Ephemeral data protection providers</span></span>](implementation/key-storage-ephemeral.md)

* [<span data-ttu-id="5fd14-133">Kompatibilität</span><span class="sxs-lookup"><span data-stu-id="5fd14-133">Compatibility</span></span>](compatibility/index.md)

  * [<span data-ttu-id="5fd14-134">Freigeben von Cookies zwischen Apps</span><span class="sxs-lookup"><span data-stu-id="5fd14-134">Sharing cookies between apps</span></span>](compatibility/cookie-sharing.md)

  * [<span data-ttu-id="5fd14-135">Ersetzen von <machineKey> in ASP.NET</span><span class="sxs-lookup"><span data-stu-id="5fd14-135">Replacing <machineKey> in ASP.NET</span></span>](compatibility/replacing-machinekey.md)
