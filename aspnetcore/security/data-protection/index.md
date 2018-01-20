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
ms.openlocfilehash: 7bbd203a67b32032ba2ab82448a5fc9a495b52aa
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/19/2018
---
# <a name="data-protection-in-aspnet-core-consumer-apis-configuration-extensibility-apis-and-implementation"></a><span data-ttu-id="50736-103">Schutz von Daten in ASP.NET Core: Consumer-APIs, Konfiguration, Erweiterbarkeits-APIs und Implementierung</span><span class="sxs-lookup"><span data-stu-id="50736-103">Data Protection in ASP.NET Core: Consumer APIs, configuration, extensibility APIs and implementation</span></span>

* [<span data-ttu-id="50736-104">Einführung in den Schutz von Daten</span><span class="sxs-lookup"><span data-stu-id="50736-104">Introduction to data protection</span></span>](introduction.md)

* [<span data-ttu-id="50736-105">Erste Schritte mit Datenschutz-APIs</span><span class="sxs-lookup"><span data-stu-id="50736-105">Get started with the Data Protection APIs</span></span>](using-data-protection.md)

* [<span data-ttu-id="50736-106">Consumer-APIs</span><span class="sxs-lookup"><span data-stu-id="50736-106">Consumer APIs</span></span>](consumer-apis/index.md)

  * [<span data-ttu-id="50736-107">Übersicht über Consumer-APIs</span><span class="sxs-lookup"><span data-stu-id="50736-107">Consumer APIs overview</span></span>](consumer-apis/overview.md)

  * [<span data-ttu-id="50736-108">purpose-Zeichenfolgen</span><span class="sxs-lookup"><span data-stu-id="50736-108">Purpose strings</span></span>](consumer-apis/purpose-strings.md)

  * [<span data-ttu-id="50736-109">Zweckhierarchie und Mehrinstanzenfähigkeit</span><span class="sxs-lookup"><span data-stu-id="50736-109">Purpose hierarchy and multi-tenancy</span></span>](consumer-apis/purpose-strings-multitenancy.md)

  * [<span data-ttu-id="50736-110">Kennworthashing</span><span class="sxs-lookup"><span data-stu-id="50736-110">Password hashing</span></span>](consumer-apis/password-hashing.md)

  * [<span data-ttu-id="50736-111">Beschränken der Lebensdauer von geschützten Nutzlasten</span><span class="sxs-lookup"><span data-stu-id="50736-111">Limiting the lifetime of protected payloads</span></span>](consumer-apis/limited-lifetime-payloads.md)

  * [<span data-ttu-id="50736-112">Aufheben des Schutzes von Nutzlasten, deren Schlüssel gesperrt wurden</span><span class="sxs-lookup"><span data-stu-id="50736-112">Unprotecting payloads whose keys have been revoked</span></span>](consumer-apis/dangerous-unprotect.md)

* [<span data-ttu-id="50736-113">Konfiguration</span><span class="sxs-lookup"><span data-stu-id="50736-113">Configuration</span></span>](configuration/index.md)

  * [<span data-ttu-id="50736-114">Konfigurieren des Schutzes von Daten</span><span class="sxs-lookup"><span data-stu-id="50736-114">Configuring data protection</span></span>](configuration/overview.md)

  * [<span data-ttu-id="50736-115">Standardeinstellungen</span><span class="sxs-lookup"><span data-stu-id="50736-115">Default settings</span></span>](configuration/default-settings.md)

  * [<span data-ttu-id="50736-116">Computerweite Richtlinie</span><span class="sxs-lookup"><span data-stu-id="50736-116">Machine-wide policy</span></span>](configuration/machine-wide-policy.md)

  * [<span data-ttu-id="50736-117">Szenarios ohne Möglichkeiten zur Abhängigkeitsinjektion</span><span class="sxs-lookup"><span data-stu-id="50736-117">Non DI-aware scenarios</span></span>](configuration/non-di-scenarios.md)

* [<span data-ttu-id="50736-118">Erweiterbarkeits-APIs</span><span class="sxs-lookup"><span data-stu-id="50736-118">Extensibility APIs</span></span>](extensibility/index.md)

  * [<span data-ttu-id="50736-119">Kryptografieerweiterbarkeit in Core</span><span class="sxs-lookup"><span data-stu-id="50736-119">Core cryptography extensibility</span></span>](extensibility/core-crypto.md)

  * [<span data-ttu-id="50736-120">Schlüsselverwaltungserweiterbarkeit</span><span class="sxs-lookup"><span data-stu-id="50736-120">Key management extensibility</span></span>](extensibility/key-management.md)

  * [<span data-ttu-id="50736-121">Verschiedene APIs</span><span class="sxs-lookup"><span data-stu-id="50736-121">Miscellaneous APIs</span></span>](extensibility/misc-apis.md)

* [<span data-ttu-id="50736-122">Implementation (Implementierung)</span><span class="sxs-lookup"><span data-stu-id="50736-122">Implementation</span></span>](implementation/index.md)

  * [<span data-ttu-id="50736-123">Authentifizierte Verschlüsselungsdetails</span><span class="sxs-lookup"><span data-stu-id="50736-123">Authenticated encryption details</span></span>](implementation/authenticated-encryption-details.md)

  * [<span data-ttu-id="50736-124">Unterschlüsselableitung und authentifizierte Verschlüsselung</span><span class="sxs-lookup"><span data-stu-id="50736-124">Subkey derivation and authenticated encryption</span></span>](implementation/subkeyderivation.md)

  * [<span data-ttu-id="50736-125">Kontextheader</span><span class="sxs-lookup"><span data-stu-id="50736-125">Context headers</span></span>](implementation/context-headers.md)

  * [<span data-ttu-id="50736-126">Schlüsselverwaltung</span><span class="sxs-lookup"><span data-stu-id="50736-126">Key management</span></span>](implementation/key-management.md)

  * [<span data-ttu-id="50736-127">Schlüsselspeicheranbieter</span><span class="sxs-lookup"><span data-stu-id="50736-127">Key storage providers</span></span>](implementation/key-storage-providers.md)

  * [<span data-ttu-id="50736-128">Verschlüsselung ruhender Daten mit Schlüsseln</span><span class="sxs-lookup"><span data-stu-id="50736-128">Key encryption at rest</span></span>](implementation/key-encryption-at-rest.md)

  * [<span data-ttu-id="50736-129">Unveränderlichkeit von Schlüsseln und Ändern von Einstellungen</span><span class="sxs-lookup"><span data-stu-id="50736-129">Key immutability and changing settings</span></span>](implementation/key-immutability.md)

  * [<span data-ttu-id="50736-130">Schlüsselspeicherformat</span><span class="sxs-lookup"><span data-stu-id="50736-130">Key storage format</span></span>](implementation/key-storage-format.md)

  * [<span data-ttu-id="50736-131">Kurzlebige Datenschutzanbieter</span><span class="sxs-lookup"><span data-stu-id="50736-131">Ephemeral data protection providers</span></span>](implementation/key-storage-ephemeral.md)

* [<span data-ttu-id="50736-132">Kompatibilität</span><span class="sxs-lookup"><span data-stu-id="50736-132">Compatibility</span></span>](compatibility/index.md)

  * [<span data-ttu-id="50736-133">Freigeben von Cookies zwischen Apps</span><span class="sxs-lookup"><span data-stu-id="50736-133">Sharing cookies between apps</span></span>](compatibility/cookie-sharing.md)

  * [<span data-ttu-id="50736-134">Ersetzen von <machineKey> in ASP.NET</span><span class="sxs-lookup"><span data-stu-id="50736-134">Replacing <machineKey> in ASP.NET</span></span>](compatibility/replacing-machinekey.md)
