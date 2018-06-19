---
title: Schutz von Daten in ASP.NET Core
author: rick-anderson
description: Dieses Dokument dient als Inhaltsverzeichnis für die verschiedenen Themen zum Schutz von Daten in ASP.NET Core.
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/data-protection/index
ms.openlocfilehash: 83b5bb1e6a4942a4d3e5ec0d445fa6e5a21fb533
ms.sourcegitcommit: 48beecfe749ddac52bc79aa3eb246a2dcdaa1862
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/22/2018
ms.locfileid: "30071694"
---
# <a name="data-protection-in-aspnet-core"></a><span data-ttu-id="c77c2-103">Schutz von Daten in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="c77c2-103">Data Protection in ASP.NET Core</span></span>

* [<span data-ttu-id="c77c2-104">Einführung in den Schutz von Daten</span><span class="sxs-lookup"><span data-stu-id="c77c2-104">Introduction to data protection</span></span>](xref:security/data-protection/introduction)

* [<span data-ttu-id="c77c2-105">Erste Schritte mit Datenschutz-APIs</span><span class="sxs-lookup"><span data-stu-id="c77c2-105">Get started with the Data Protection APIs</span></span>](xref:security/data-protection/using-data-protection)

* [<span data-ttu-id="c77c2-106">Consumer-APIs</span><span class="sxs-lookup"><span data-stu-id="c77c2-106">Consumer APIs</span></span>](xref:security/data-protection/consumer-apis/index)

  * [<span data-ttu-id="c77c2-107">Übersicht über Consumer-APIs</span><span class="sxs-lookup"><span data-stu-id="c77c2-107">Consumer APIs overview</span></span>](xref:security/data-protection/consumer-apis/overview)

  * [<span data-ttu-id="c77c2-108">purpose-Zeichenfolgen</span><span class="sxs-lookup"><span data-stu-id="c77c2-108">Purpose strings</span></span>](xref:security/data-protection/consumer-apis/purpose-strings)

  * [<span data-ttu-id="c77c2-109">Zweckhierarchie und Mehrinstanzenfähigkeit</span><span class="sxs-lookup"><span data-stu-id="c77c2-109">Purpose hierarchy and multi-tenancy</span></span>](xref:security/data-protection/consumer-apis/purpose-strings-multitenancy)

  * [<span data-ttu-id="c77c2-110">Hasherstellung für Kennwörter</span><span class="sxs-lookup"><span data-stu-id="c77c2-110">Hash passwords</span></span>](xref:security/data-protection/consumer-apis/password-hashing)

  * [<span data-ttu-id="c77c2-111">Beschränken der Lebensdauer von geschützten Payloads</span><span class="sxs-lookup"><span data-stu-id="c77c2-111">Limit the lifetime of protected payloads</span></span>](xref:security/data-protection/consumer-apis/limited-lifetime-payloads)

  * [<span data-ttu-id="c77c2-112">Aufheben des Schutzes von Payloads, deren Schlüssel gesperrt wurden</span><span class="sxs-lookup"><span data-stu-id="c77c2-112">Unprotect payloads whose keys have been revoked</span></span>](xref:security/data-protection/consumer-apis/dangerous-unprotect)

* [<span data-ttu-id="c77c2-113">Konfiguration</span><span class="sxs-lookup"><span data-stu-id="c77c2-113">Configuration</span></span>](xref:security/data-protection/configuration/index)

  * [<span data-ttu-id="c77c2-114">Konfigurieren des Schutzes von Daten in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="c77c2-114">Configure ASP.NET Core Data Protection</span></span>](xref:security/data-protection/configuration/overview)

  * [<span data-ttu-id="c77c2-115">Standardeinstellungen</span><span class="sxs-lookup"><span data-stu-id="c77c2-115">Default settings</span></span>](xref:security/data-protection/configuration/default-settings)

  * [<span data-ttu-id="c77c2-116">Computerweite Richtlinie</span><span class="sxs-lookup"><span data-stu-id="c77c2-116">Machine-wide policy</span></span>](xref:security/data-protection/configuration/machine-wide-policy)

  * [<span data-ttu-id="c77c2-117">Szenarios ohne Möglichkeiten zur Abhängigkeitsinjektion</span><span class="sxs-lookup"><span data-stu-id="c77c2-117">Non DI-aware scenarios</span></span>](xref:security/data-protection/configuration/non-di-scenarios)

* [<span data-ttu-id="c77c2-118">Erweiterbarkeits-APIs</span><span class="sxs-lookup"><span data-stu-id="c77c2-118">Extensibility APIs</span></span>](xref:security/data-protection/extensibility/index)

  * [<span data-ttu-id="c77c2-119">Kryptografieerweiterbarkeit in Core</span><span class="sxs-lookup"><span data-stu-id="c77c2-119">Core cryptography extensibility</span></span>](xref:security/data-protection/extensibility/core-crypto)

  * [<span data-ttu-id="c77c2-120">Schlüsselverwaltungserweiterbarkeit</span><span class="sxs-lookup"><span data-stu-id="c77c2-120">Key management extensibility</span></span>](xref:security/data-protection/extensibility/key-management)

  * [<span data-ttu-id="c77c2-121">Verschiedene APIs</span><span class="sxs-lookup"><span data-stu-id="c77c2-121">Miscellaneous APIs</span></span>](xref:security/data-protection/extensibility/misc-apis)

* [<span data-ttu-id="c77c2-122">Implementation (Implementierung)</span><span class="sxs-lookup"><span data-stu-id="c77c2-122">Implementation</span></span>](xref:security/data-protection/implementation/index)

  * [<span data-ttu-id="c77c2-123">Authentifizierte Verschlüsselungsdetails</span><span class="sxs-lookup"><span data-stu-id="c77c2-123">Authenticated encryption details</span></span>](xref:security/data-protection/implementation/authenticated-encryption-details)

  * [<span data-ttu-id="c77c2-124">Unterschlüsselableitung und authentifizierte Verschlüsselung</span><span class="sxs-lookup"><span data-stu-id="c77c2-124">Subkey derivation and authenticated encryption</span></span>](xref:security/data-protection/implementation/subkeyderivation)

  * [<span data-ttu-id="c77c2-125">Kontextheader</span><span class="sxs-lookup"><span data-stu-id="c77c2-125">Context headers</span></span>](xref:security/data-protection/implementation/context-headers)

  * [<span data-ttu-id="c77c2-126">Schlüsselverwaltung</span><span class="sxs-lookup"><span data-stu-id="c77c2-126">Key management</span></span>](xref:security/data-protection/implementation/key-management)

  * [<span data-ttu-id="c77c2-127">Schlüsselspeicheranbieter</span><span class="sxs-lookup"><span data-stu-id="c77c2-127">Key storage providers</span></span>](xref:security/data-protection/implementation/key-storage-providers)

  * [<span data-ttu-id="c77c2-128">Verschlüsselung ruhender Daten mit Schlüsseln</span><span class="sxs-lookup"><span data-stu-id="c77c2-128">Key encryption at rest</span></span>](xref:security/data-protection/implementation/key-encryption-at-rest)

  * [<span data-ttu-id="c77c2-129">Schlüsselunveränderlichkeit und -einstellungen</span><span class="sxs-lookup"><span data-stu-id="c77c2-129">Key immutability and settings</span></span>](xref:security/data-protection/implementation/key-immutability)

  * [<span data-ttu-id="c77c2-130">Schlüsselspeicherformat</span><span class="sxs-lookup"><span data-stu-id="c77c2-130">Key storage format</span></span>](xref:security/data-protection/implementation/key-storage-format)

  * [<span data-ttu-id="c77c2-131">Kurzlebige Datenschutzanbieter</span><span class="sxs-lookup"><span data-stu-id="c77c2-131">Ephemeral data protection providers</span></span>](xref:security/data-protection/implementation/key-storage-ephemeral)

* [<span data-ttu-id="c77c2-132">Kompatibilität</span><span class="sxs-lookup"><span data-stu-id="c77c2-132">Compatibility</span></span>](xref:security/data-protection/compatibility/index)

  * [<span data-ttu-id="c77c2-133">Ersetzen von ASP.NET <machineKey> in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="c77c2-133">Replacing ASP.NET <machineKey> in ASP.NET Core</span></span>](xref:security/data-protection/compatibility/replacing-machinekey)
