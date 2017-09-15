---
title: Schutz von Daten in ASP.NET Core
author: rick-anderson
description: 
keywords: ASP.NET Core,
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 1f402da8-1052-4970-9835-9f9f16a02dbc
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/index
ms.openlocfilehash: 39f2ca96f8542de033274ea957b5c7736948c981
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/11/2017
---
# <a name="data-protection-in-aspnet-core-consumer-apis-configuration-extensibility-apis-and-implementation"></a><span data-ttu-id="45aef-103">Schutz von Daten in ASP.NET Core: Consumer-APIs, Konfiguration, Erweiterbarkeits-APIs und Implementierung</span><span class="sxs-lookup"><span data-stu-id="45aef-103">Data Protection in ASP.NET Core: Consumer APIs, configuration, extensibility APIs and implementation</span></span>

* [<span data-ttu-id="45aef-104">Einführung in den Schutz von Daten</span><span class="sxs-lookup"><span data-stu-id="45aef-104">Introduction to Data Protection</span></span>](introduction.md)

* [<span data-ttu-id="45aef-105">Erste Schritte mit Datenschutz-APIs</span><span class="sxs-lookup"><span data-stu-id="45aef-105">Getting Started with the Data Protection APIs</span></span>](using-data-protection.md)

* [<span data-ttu-id="45aef-106">Consumer-APIs</span><span class="sxs-lookup"><span data-stu-id="45aef-106">Consumer APIs</span></span>](consumer-apis/index.md)

  * [<span data-ttu-id="45aef-107">Übersicht über Consumer-APIs</span><span class="sxs-lookup"><span data-stu-id="45aef-107">Consumer APIs Overview</span></span>](consumer-apis/overview.md)

  * [<span data-ttu-id="45aef-108">Zweckzeichenfolgen</span><span class="sxs-lookup"><span data-stu-id="45aef-108">Purpose Strings</span></span>](consumer-apis/purpose-strings.md)

  * [<span data-ttu-id="45aef-109">Zweckhierarchie und Mehrinstanzenfähigkeit</span><span class="sxs-lookup"><span data-stu-id="45aef-109">Purpose hierarchy and multi-tenancy</span></span>](consumer-apis/purpose-strings-multitenancy.md)

  * [<span data-ttu-id="45aef-110">Kennwort-Hashing</span><span class="sxs-lookup"><span data-stu-id="45aef-110">Password Hashing</span></span>](consumer-apis/password-hashing.md)

  * [<span data-ttu-id="45aef-111">Beschränken der Lebensdauer von geschützten Nutzlasten</span><span class="sxs-lookup"><span data-stu-id="45aef-111">Limiting the lifetime of protected payloads</span></span>](consumer-apis/limited-lifetime-payloads.md)

  * [<span data-ttu-id="45aef-112">Aufheben des Schutzes von Nutzlasten, deren Schlüssel gesperrt wurden</span><span class="sxs-lookup"><span data-stu-id="45aef-112">Unprotecting payloads whose keys have been revoked</span></span>](consumer-apis/dangerous-unprotect.md)

* [<span data-ttu-id="45aef-113">Konfiguration</span><span class="sxs-lookup"><span data-stu-id="45aef-113">Configuration</span></span>](configuration/index.md)

  * [<span data-ttu-id="45aef-114">Konfigurieren des Schutzes von Daten</span><span class="sxs-lookup"><span data-stu-id="45aef-114">Configuring Data Protection</span></span>](configuration/overview.md)

  * [<span data-ttu-id="45aef-115">Standardeinstellungen</span><span class="sxs-lookup"><span data-stu-id="45aef-115">Default Settings</span></span>](configuration/default-settings.md)

  * [<span data-ttu-id="45aef-116">Computerübergreifende Richtlinie</span><span class="sxs-lookup"><span data-stu-id="45aef-116">Machine Wide Policy</span></span>](configuration/machine-wide-policy.md)

  * [<span data-ttu-id="45aef-117">Szenarios, die die Abhängigkeitsinjektion nicht beachten</span><span class="sxs-lookup"><span data-stu-id="45aef-117">Non DI Aware Scenarios</span></span>](configuration/non-di-scenarios.md)

* [<span data-ttu-id="45aef-118">Erweiterbarkeits-APIs</span><span class="sxs-lookup"><span data-stu-id="45aef-118">Extensibility APIs</span></span>](extensibility/index.md)

  * [<span data-ttu-id="45aef-119">Kryptografieerweiterbarkeit in Core</span><span class="sxs-lookup"><span data-stu-id="45aef-119">Core cryptography extensibility</span></span>](extensibility/core-crypto.md)

  * [<span data-ttu-id="45aef-120">Schlüsselverwaltungserweiterbarkeit</span><span class="sxs-lookup"><span data-stu-id="45aef-120">Key management extensibility</span></span>](extensibility/key-management.md)

  * [<span data-ttu-id="45aef-121">Verschiedene APIs</span><span class="sxs-lookup"><span data-stu-id="45aef-121">Miscellaneous APIs</span></span>](extensibility/misc-apis.md)

* [<span data-ttu-id="45aef-122">Implementierung</span><span class="sxs-lookup"><span data-stu-id="45aef-122">Implementation</span></span>](implementation/index.md)

  * [<span data-ttu-id="45aef-123">Authentifizierte Verschlüsselungsdetails</span><span class="sxs-lookup"><span data-stu-id="45aef-123">Authenticated encryption details.</span></span>](implementation/authenticated-encryption-details.md)

  * [<span data-ttu-id="45aef-124">Unterschlüsselableitung und authentifizierte Verschlüsselung</span><span class="sxs-lookup"><span data-stu-id="45aef-124">Subkey Derivation and Authenticated Encryption</span></span>](implementation/subkeyderivation.md)

  * [<span data-ttu-id="45aef-125">Kontextheader</span><span class="sxs-lookup"><span data-stu-id="45aef-125">Context headers</span></span>](implementation/context-headers.md)

  * [<span data-ttu-id="45aef-126">Schlüsselverwaltung</span><span class="sxs-lookup"><span data-stu-id="45aef-126">Key Management</span></span>](implementation/key-management.md)

  * [<span data-ttu-id="45aef-127">Schlüsselspeicheranbieter</span><span class="sxs-lookup"><span data-stu-id="45aef-127">Key Storage Providers</span></span>](implementation/key-storage-providers.md)

  * [<span data-ttu-id="45aef-128">Ruhende Verschlüsselung von Schlüsseln</span><span class="sxs-lookup"><span data-stu-id="45aef-128">Key Encryption At Rest</span></span>](implementation/key-encryption-at-rest.md)

  * [<span data-ttu-id="45aef-129">Unveränderlichkeit von Schlüsseln und Ändern von Einstellungen</span><span class="sxs-lookup"><span data-stu-id="45aef-129">Key Immutability and Changing Settings</span></span>](implementation/key-immutability.md)

  * [<span data-ttu-id="45aef-130">Schlüsselspeicherformat</span><span class="sxs-lookup"><span data-stu-id="45aef-130">Key Storage Format</span></span>](implementation/key-storage-format.md)

  * [<span data-ttu-id="45aef-131">Kurzlebige Datenschutzanbieter</span><span class="sxs-lookup"><span data-stu-id="45aef-131">Ephemeral data protection providers</span></span>](implementation/key-storage-ephemeral.md)

* [<span data-ttu-id="45aef-132">Kompatibilität</span><span class="sxs-lookup"><span data-stu-id="45aef-132">Compatibility</span></span>](compatibility/index.md)

  * [<span data-ttu-id="45aef-133">Freigeben von Cookies zwischen Anwendungen</span><span class="sxs-lookup"><span data-stu-id="45aef-133">Sharing cookies between applications</span></span>](compatibility/cookie-sharing.md)

  * [<span data-ttu-id="45aef-134">Ersetzen von <machineKey> in ASP.NET</span><span class="sxs-lookup"><span data-stu-id="45aef-134">Replacing <machineKey> in ASP.NET</span></span>](compatibility/replacing-machinekey.md)
