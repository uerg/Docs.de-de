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
ms.openlocfilehash: 8b42e65bb6121355120a6f4fbe8cd4d1fea153de
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
# <a name="data-protection-in-aspnet-core-consumer-apis-configuration-extensibility-apis-and-implementation"></a><span data-ttu-id="15209-104">Schutz von Daten in ASP.NET Core: Consumer-APIs, Konfiguration, Erweiterbarkeits-APIs und Implementierung</span><span class="sxs-lookup"><span data-stu-id="15209-104">Data Protection in ASP.NET Core: Consumer APIs, configuration, extensibility APIs and implementation</span></span>

* [<span data-ttu-id="15209-105">Einführung in den Schutz von Daten</span><span class="sxs-lookup"><span data-stu-id="15209-105">Introduction to Data Protection</span></span>](introduction.md)

* [<span data-ttu-id="15209-106">Erste Schritte mit Datenschutz-APIs</span><span class="sxs-lookup"><span data-stu-id="15209-106">Getting Started with the Data Protection APIs</span></span>](using-data-protection.md)

* [<span data-ttu-id="15209-107">Consumer-APIs</span><span class="sxs-lookup"><span data-stu-id="15209-107">Consumer APIs</span></span>](consumer-apis/index.md)

  * [<span data-ttu-id="15209-108">Übersicht über Consumer-APIs</span><span class="sxs-lookup"><span data-stu-id="15209-108">Consumer APIs Overview</span></span>](consumer-apis/overview.md)

  * [<span data-ttu-id="15209-109">Zweckzeichenfolgen</span><span class="sxs-lookup"><span data-stu-id="15209-109">Purpose Strings</span></span>](consumer-apis/purpose-strings.md)

  * [<span data-ttu-id="15209-110">Zweckhierarchie und Mehrinstanzenfähigkeit</span><span class="sxs-lookup"><span data-stu-id="15209-110">Purpose hierarchy and multi-tenancy</span></span>](consumer-apis/purpose-strings-multitenancy.md)

  * [<span data-ttu-id="15209-111">Kennwort-Hashing</span><span class="sxs-lookup"><span data-stu-id="15209-111">Password Hashing</span></span>](consumer-apis/password-hashing.md)

  * [<span data-ttu-id="15209-112">Beschränken der Lebensdauer von geschützten Nutzlasten</span><span class="sxs-lookup"><span data-stu-id="15209-112">Limiting the lifetime of protected payloads</span></span>](consumer-apis/limited-lifetime-payloads.md)

  * [<span data-ttu-id="15209-113">Aufheben des Schutzes von Nutzlasten, deren Schlüssel gesperrt wurden</span><span class="sxs-lookup"><span data-stu-id="15209-113">Unprotecting payloads whose keys have been revoked</span></span>](consumer-apis/dangerous-unprotect.md)

* [<span data-ttu-id="15209-114">Konfiguration</span><span class="sxs-lookup"><span data-stu-id="15209-114">Configuration</span></span>](configuration/index.md)

  * [<span data-ttu-id="15209-115">Konfigurieren des Schutzes von Daten</span><span class="sxs-lookup"><span data-stu-id="15209-115">Configuring Data Protection</span></span>](configuration/overview.md)

  * [<span data-ttu-id="15209-116">Standardeinstellungen</span><span class="sxs-lookup"><span data-stu-id="15209-116">Default Settings</span></span>](configuration/default-settings.md)

  * [<span data-ttu-id="15209-117">Computerübergreifende Richtlinie</span><span class="sxs-lookup"><span data-stu-id="15209-117">Machine Wide Policy</span></span>](configuration/machine-wide-policy.md)

  * [<span data-ttu-id="15209-118">Szenarios, die die Abhängigkeitsinjektion nicht beachten</span><span class="sxs-lookup"><span data-stu-id="15209-118">Non DI Aware Scenarios</span></span>](configuration/non-di-scenarios.md)

* [<span data-ttu-id="15209-119">Erweiterbarkeits-APIs</span><span class="sxs-lookup"><span data-stu-id="15209-119">Extensibility APIs</span></span>](extensibility/index.md)

  * [<span data-ttu-id="15209-120">Kryptografieerweiterbarkeit in Core</span><span class="sxs-lookup"><span data-stu-id="15209-120">Core cryptography extensibility</span></span>](extensibility/core-crypto.md)

  * [<span data-ttu-id="15209-121">Schlüsselverwaltungserweiterbarkeit</span><span class="sxs-lookup"><span data-stu-id="15209-121">Key management extensibility</span></span>](extensibility/key-management.md)

  * [<span data-ttu-id="15209-122">Verschiedene APIs</span><span class="sxs-lookup"><span data-stu-id="15209-122">Miscellaneous APIs</span></span>](extensibility/misc-apis.md)

* [<span data-ttu-id="15209-123">Implementation (Implementierung)</span><span class="sxs-lookup"><span data-stu-id="15209-123">Implementation</span></span>](implementation/index.md)

  * [<span data-ttu-id="15209-124">Authentifizierte Verschlüsselungsdetails</span><span class="sxs-lookup"><span data-stu-id="15209-124">Authenticated encryption details</span></span>](implementation/authenticated-encryption-details.md)

  * [<span data-ttu-id="15209-125">Unterschlüsselableitung und authentifizierte Verschlüsselung</span><span class="sxs-lookup"><span data-stu-id="15209-125">Subkey Derivation and Authenticated Encryption</span></span>](implementation/subkeyderivation.md)

  * [<span data-ttu-id="15209-126">Kontextheader</span><span class="sxs-lookup"><span data-stu-id="15209-126">Context headers</span></span>](implementation/context-headers.md)

  * [<span data-ttu-id="15209-127">Schlüsselverwaltung</span><span class="sxs-lookup"><span data-stu-id="15209-127">Key Management</span></span>](implementation/key-management.md)

  * [<span data-ttu-id="15209-128">Schlüsselspeicheranbieter</span><span class="sxs-lookup"><span data-stu-id="15209-128">Key Storage Providers</span></span>](implementation/key-storage-providers.md)

  * [<span data-ttu-id="15209-129">Ruhende Verschlüsselung von Schlüsseln</span><span class="sxs-lookup"><span data-stu-id="15209-129">Key Encryption At Rest</span></span>](implementation/key-encryption-at-rest.md)

  * [<span data-ttu-id="15209-130">Unveränderlichkeit von Schlüsseln und Ändern von Einstellungen</span><span class="sxs-lookup"><span data-stu-id="15209-130">Key Immutability and Changing Settings</span></span>](implementation/key-immutability.md)

  * [<span data-ttu-id="15209-131">Schlüsselspeicherformat</span><span class="sxs-lookup"><span data-stu-id="15209-131">Key Storage Format</span></span>](implementation/key-storage-format.md)

  * [<span data-ttu-id="15209-132">Kurzlebige Datenschutzanbieter</span><span class="sxs-lookup"><span data-stu-id="15209-132">Ephemeral data protection providers</span></span>](implementation/key-storage-ephemeral.md)

* [<span data-ttu-id="15209-133">Kompatibilität</span><span class="sxs-lookup"><span data-stu-id="15209-133">Compatibility</span></span>](compatibility/index.md)

  * [<span data-ttu-id="15209-134">Freigeben von Cookies zwischen Anwendungen</span><span class="sxs-lookup"><span data-stu-id="15209-134">Sharing cookies between applications</span></span>](compatibility/cookie-sharing.md)

  * [<span data-ttu-id="15209-135">Ersetzen von <machineKey> in ASP.NET</span><span class="sxs-lookup"><span data-stu-id="15209-135">Replacing <machineKey> in ASP.NET</span></span>](compatibility/replacing-machinekey.md)
