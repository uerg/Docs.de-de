---
title: "Wichtige Unveränderlichkeit und Ändern von Einstellungen"
author: rick-anderson
description: "In diesem Dokument werden die Implementierungsdetails der ASP.NET Core Data Protection Key Unveränderlichkeit APIs."
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/data-protection/implementation/key-immutability
ms.openlocfilehash: 98727c7a0c525edcda4fd8d004e0ac584cf0a5e5
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2018
---
# <a name="key-immutability-and-changing-settings"></a><span data-ttu-id="4f229-103">Wichtige Unveränderlichkeit und Ändern von Einstellungen</span><span class="sxs-lookup"><span data-stu-id="4f229-103">Key Immutability and changing settings</span></span>

<span data-ttu-id="4f229-104">Sobald ein Objekt mit dem Sicherungsspeicher persistent gespeichert wird, ist seine Darstellung ewig fest.</span><span class="sxs-lookup"><span data-stu-id="4f229-104">Once an object is persisted to the backing store, its representation is forever fixed.</span></span> <span data-ttu-id="4f229-105">Sicherungsspeicher können neue Daten hinzugefügt werden, aber vorhandene Daten können nicht geändert werden.</span><span class="sxs-lookup"><span data-stu-id="4f229-105">New data can be added to the backing store, but existing data can never be mutated.</span></span> <span data-ttu-id="4f229-106">Der Hauptzweck dieses Verhalten wird verhindert, dass Daten beschädigt.</span><span class="sxs-lookup"><span data-stu-id="4f229-106">The primary purpose of this behavior is to prevent data corruption.</span></span>

<span data-ttu-id="4f229-107">Eine Folge dieses Verhalten ist, nachdem ein Schlüssel mit dem Sicherungsspeicher geschrieben wurde, unveränderlich ist.</span><span class="sxs-lookup"><span data-stu-id="4f229-107">One consequence of this behavior is that once a key is written to the backing store, it's immutable.</span></span> <span data-ttu-id="4f229-108">Die Erstellung, Aktivierung und Ablauf von Datumsangaben können nie geändert werden, obwohl es mit widerrufen können `IKeyManager`.</span><span class="sxs-lookup"><span data-stu-id="4f229-108">Its creation, activation, and expiration dates can never be changed, though it can revoked by using `IKeyManager`.</span></span> <span data-ttu-id="4f229-109">Darüber hinaus sind die zugrunde liegenden algorithmische Informationen, Schlüsselmaterial und Verschlüsselung Rest Eigenschaften auch unveränderlich.</span><span class="sxs-lookup"><span data-stu-id="4f229-109">Additionally, its underlying algorithmic information, master keying material, and encryption at rest properties are also immutable.</span></span>

<span data-ttu-id="4f229-110">Wenn der Entwickler eine Einstellung geändert wird, die schlüsselpersistenz wirkt sich auf, diese Änderungen wird nicht wirksam, bis das nächste Mal ein Schlüssel generiert wird, entweder über ein expliziter Aufruf von `IKeyManager.CreateNewKey` oder über die Datenschutzsystem des eigenen [automatische Schlüssel Generierung](key-management.md#data-protection-implementation-key-management) Verhalten.</span><span class="sxs-lookup"><span data-stu-id="4f229-110">If the developer changes any setting that affects key persistence, those changes won't go into effect until the next time a key is generated, either via an explicit call to `IKeyManager.CreateNewKey` or via the data protection system's own [automatic key generation](key-management.md#data-protection-implementation-key-management) behavior.</span></span> <span data-ttu-id="4f229-111">Die Einstellungen, die beeinflussen schlüsselpersistenz sind wie folgt:</span><span class="sxs-lookup"><span data-stu-id="4f229-111">The settings that affect key persistence are as follows:</span></span>

* [<span data-ttu-id="4f229-112">Die Standardlebensdauer für Schlüssel</span><span class="sxs-lookup"><span data-stu-id="4f229-112">The default key lifetime</span></span>](key-management.md#data-protection-implementation-key-management)

* [<span data-ttu-id="4f229-113">Die Verschlüsselung auf Rest-Mechanismus</span><span class="sxs-lookup"><span data-stu-id="4f229-113">The key encryption at rest mechanism</span></span>](key-encryption-at-rest.md#data-protection-implementation-key-encryption-at-rest)

* [<span data-ttu-id="4f229-114">Die algorithmische Informationen in den Schlüssel enthalten sind</span><span class="sxs-lookup"><span data-stu-id="4f229-114">The algorithmic information contained within the key</span></span>](xref:security/data-protection/configuration/overview#changing-algorithms-with-usecryptographicalgorithms)

<span data-ttu-id="4f229-115">Wenn Sie diese Einstellungen älter als die nächste automatische Funktionstaste gleitenden Zeitpunkt starten möchten, erwägen Sie einen expliziten Aufruf von `IKeyManager.CreateNewKey` um die Erstellung eines neuen Schlüssels zu erzwingen.</span><span class="sxs-lookup"><span data-stu-id="4f229-115">If you need these settings to kick in earlier than the next automatic key rolling time, consider making an explicit call to `IKeyManager.CreateNewKey` to force the creation of a new key.</span></span> <span data-ttu-id="4f229-116">Denken Sie daran, geben Sie eine explizite Aktivierungsdatum ({jetzt + 2 Tage} ist eine gute Faustregel für die Änderung weitergegeben werden können) und Ablaufdatum im Aufruf.</span><span class="sxs-lookup"><span data-stu-id="4f229-116">Remember to provide an explicit activation date ({ now + 2 days } is a good rule of thumb to allow time for the change to propagate) and expiration date in the call.</span></span>

>[!TIP]
> <span data-ttu-id="4f229-117">Alle Anwendungen, die im Repository berühren sollten mit die gleichen Einstellungen geben die `IDataProtectionBuilder` Erweiterungsmethoden.</span><span class="sxs-lookup"><span data-stu-id="4f229-117">All applications touching the repository should specify the same settings with the `IDataProtectionBuilder` extension methods.</span></span> <span data-ttu-id="4f229-118">Andernfalls werden die Eigenschaften des permanenten Schlüssels die jeweilige Anwendung abhängig sein, die die schlüsselgenerierung Routinen aufgerufen.</span><span class="sxs-lookup"><span data-stu-id="4f229-118">Otherwise, the properties of the persisted key will be dependent on the particular application that invoked the key generation routines.</span></span>
