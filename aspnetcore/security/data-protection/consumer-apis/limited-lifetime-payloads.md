---
title: "Beschränken die Lebensdauer des geschützten Nutzlasten"
author: rick-anderson
description: "Dieses Dokument wird erläutert, wie die Lebensdauer einer geschützten Nutzlast, die mit der ASP.NET Core-Datenschutz APIs beschränkt."
keywords: ASP.NET Core, Datenschutz, Nutzlast Lebensdauer
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 000175e2-10fc-43dd-bfc2-51e004b97b44
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/consumer-apis/limited-lifetime-payloads
ms.openlocfilehash: 3fe2e97db118a92cf6fa003b9ce8a9dedf253136
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
# <a name="limiting-the-lifetime-of-protected-payloads"></a><span data-ttu-id="596eb-104">Beschränken die Lebensdauer des geschützten Nutzlasten</span><span class="sxs-lookup"><span data-stu-id="596eb-104">Limiting the lifetime of protected payloads</span></span>

<span data-ttu-id="596eb-105">Es gibt Szenarien, bei denen der Anwendungsentwickler möchte eine geschützte Nutzlast zu erstellen, die nach einem festgelegten Zeitraum abläuft.</span><span class="sxs-lookup"><span data-stu-id="596eb-105">There are scenarios where the application developer wants to create a protected payload that expires after a set period of time.</span></span> <span data-ttu-id="596eb-106">Geschützte Nutzlast könnte z. B. ein kennwortzurücksetzungstoken darstellen, die nur für eine Stunde gültig sein soll.</span><span class="sxs-lookup"><span data-stu-id="596eb-106">For instance, the protected payload might represent a password reset token that should only be valid for one hour.</span></span> <span data-ttu-id="596eb-107">Es ist sicherlich möglich, dass der Entwickler, ihre eigenen nutzlastformat zu erstellen, ein eingebettetes Ablaufdatum enthält, und erfahrene Entwickler und trotzdem fortsetzen möchten, jedoch für die meisten Entwickler verwalten diese Ablaufzeiten kann erweitert mühsam.</span><span class="sxs-lookup"><span data-stu-id="596eb-107">It is certainly possible for the developer to create their own payload format that contains an embedded expiration date, and advanced developers may wish to do this anyway, but for the majority of developers managing these expirations can grow tedious.</span></span>

<span data-ttu-id="596eb-108">Um dies für unsere entwicklerzielgruppe, das Paket vereinfachen [Microsoft.AspNetCore.DataProtection.Extensions](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.Extensions/) enthält Dienstprogramm-APIs zum Erstellen von Nutzlasten, das automatisch nach einem festgelegten Zeitraum abläuft.</span><span class="sxs-lookup"><span data-stu-id="596eb-108">To make this easier for our developer audience, the package [Microsoft.AspNetCore.DataProtection.Extensions](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.Extensions/) contains utility APIs for creating payloads that automatically expire after a set period of time.</span></span> <span data-ttu-id="596eb-109">Diese APIs der hängen die `ITimeLimitedDataProtector` Typ.</span><span class="sxs-lookup"><span data-stu-id="596eb-109">These APIs hang off of the `ITimeLimitedDataProtector` type.</span></span>

## <a name="api-usage"></a><span data-ttu-id="596eb-110">Zur Verwendung der API</span><span class="sxs-lookup"><span data-stu-id="596eb-110">API usage</span></span>

<span data-ttu-id="596eb-111">Die `ITimeLimitedDataProtector` Schnittstelle ist die Kernschnittstelle zum Schützen und Aufheben des Schutzes zeitlich begrenzte / selbst ablaufender-Nutzlasten.</span><span class="sxs-lookup"><span data-stu-id="596eb-111">The `ITimeLimitedDataProtector` interface is the core interface for protecting and unprotecting time-limited / self-expiring payloads.</span></span> <span data-ttu-id="596eb-112">Zum Erstellen einer Instanz von einem `ITimeLimitedDataProtector`, Sie benötigen zunächst eine Instanz von einer regulären [IDataProtector](overview.md) mit einem bestimmten Zweck erstellt.</span><span class="sxs-lookup"><span data-stu-id="596eb-112">To create an instance of an `ITimeLimitedDataProtector`, you'll first need an instance of a regular [IDataProtector](overview.md) constructed with a specific purpose.</span></span> <span data-ttu-id="596eb-113">Einmal die `IDataProtector` Instanz verfügbar ist, rufen die `IDataProtector.ToTimeLimitedDataProtector` Erweiterungsmethode, um wieder eine Schutzvorrichtung mit Ablauf von integrierten Funktionen zugreifen.</span><span class="sxs-lookup"><span data-stu-id="596eb-113">Once the `IDataProtector` instance is available, call the `IDataProtector.ToTimeLimitedDataProtector` extension method to get back a protector with built-in expiration capabilities.</span></span>

<span data-ttu-id="596eb-114">`ITimeLimitedDataProtector`macht die folgenden API-Oberfläche und die Erweiterung der Methoden verfügbar:</span><span class="sxs-lookup"><span data-stu-id="596eb-114">`ITimeLimitedDataProtector` exposes the following API surface and extension methods:</span></span>

* <span data-ttu-id="596eb-115">CreateProtector (Zeichenfolge Zweck): ITimeLimitedDataProtector - diese API ist ähnlich den vorhandenen `IDataProtectionProvider.CreateProtector` , da er verwendet werden kann, erstellen [Zweck Ketten](purpose-strings.md) über einen Stamm zeitlich begrenzte Schutzvorrichtung.</span><span class="sxs-lookup"><span data-stu-id="596eb-115">CreateProtector(string purpose) : ITimeLimitedDataProtector - This API is similar to the existing `IDataProtectionProvider.CreateProtector` in that it can be used to create [purpose chains](purpose-strings.md) from a root time-limited protector.</span></span>

* <span data-ttu-id="596eb-116">Schützen (Byte [] als nur-Text "DateTimeOffset" Ablauf): Byte]</span><span class="sxs-lookup"><span data-stu-id="596eb-116">Protect(byte[] plaintext, DateTimeOffset expiration) : byte[]</span></span>

* <span data-ttu-id="596eb-117">Schützen (Byte []-nur-Text, TimeSpan-Lebensdauer): Byte]</span><span class="sxs-lookup"><span data-stu-id="596eb-117">Protect(byte[] plaintext, TimeSpan lifetime) : byte[]</span></span>

* <span data-ttu-id="596eb-118">Schützen (Byte [] Klartext): Byte]</span><span class="sxs-lookup"><span data-stu-id="596eb-118">Protect(byte[] plaintext) : byte[]</span></span>

* <span data-ttu-id="596eb-119">Schützen (Zeichenfolge als nur-Text "DateTimeOffset" Ablauf): Zeichenfolge</span><span class="sxs-lookup"><span data-stu-id="596eb-119">Protect(string plaintext, DateTimeOffset expiration) : string</span></span>

* <span data-ttu-id="596eb-120">Schützen (Zeichenfolge als nur-Text, TimeSpan-Lebensdauer): Zeichenfolge</span><span class="sxs-lookup"><span data-stu-id="596eb-120">Protect(string plaintext, TimeSpan lifetime) : string</span></span>

* <span data-ttu-id="596eb-121">Schützen (Klartext Zeichenfolge): Zeichenfolge</span><span class="sxs-lookup"><span data-stu-id="596eb-121">Protect(string plaintext) : string</span></span>

<span data-ttu-id="596eb-122">Zusätzlich zu den Kern `Protect` Methoden, die nur die als nur-Text werden es sind neue Überladungen, die es ermöglichen, die Nutzlast Ablaufdatum angeben.</span><span class="sxs-lookup"><span data-stu-id="596eb-122">In addition to the core `Protect` methods which take only the plaintext, there are new overloads which allow specifying the payload's expiration date.</span></span> <span data-ttu-id="596eb-123">Das Ablaufdatum kann als ein absolutes Datum angegeben werden (über eine `DateTimeOffset`) oder als ein relativer Zeitpunkt (aus dem aktuellen System Zeit, über eine `TimeSpan`).</span><span class="sxs-lookup"><span data-stu-id="596eb-123">The expiration date can be specified as an absolute date (via a `DateTimeOffset`) or as a relative time (from the current system time, via a `TimeSpan`).</span></span> <span data-ttu-id="596eb-124">Wenn eine Überladung, die eine Ablaufzeit wird nicht aufgerufen wird, ist die Nutzlast davon ausgegangen, dass nie ablaufen.</span><span class="sxs-lookup"><span data-stu-id="596eb-124">If an overload which doesn't take an expiration is called, the payload is assumed never to expire.</span></span>

* <span data-ttu-id="596eb-125">Aufheben des Schutzes (Byte [] ProtectedData, "DateTimeOffset" Ablauf): Byte]</span><span class="sxs-lookup"><span data-stu-id="596eb-125">Unprotect(byte[] protectedData, out DateTimeOffset expiration) : byte[]</span></span>

* <span data-ttu-id="596eb-126">Aufheben des Schutzes (Byte [] ProtectedData): Byte]</span><span class="sxs-lookup"><span data-stu-id="596eb-126">Unprotect(byte[] protectedData) : byte[]</span></span>

* <span data-ttu-id="596eb-127">Aufheben des Schutzes (out "DateTimeOffset" Ablaufdatum Zeichenfolge ProtectedData): Zeichenfolge</span><span class="sxs-lookup"><span data-stu-id="596eb-127">Unprotect(string protectedData, out DateTimeOffset expiration) : string</span></span>

* <span data-ttu-id="596eb-128">Aufheben des Schutzes (Zeichenfolge ProtectedData): Zeichenfolge</span><span class="sxs-lookup"><span data-stu-id="596eb-128">Unprotect(string protectedData) : string</span></span>

<span data-ttu-id="596eb-129">Die `Unprotect` Methoden die ursprünglichen ungeschützten Daten zurück.</span><span class="sxs-lookup"><span data-stu-id="596eb-129">The `Unprotect` methods return the original unprotected data.</span></span> <span data-ttu-id="596eb-130">Wenn die Nutzlast nicht noch abgelaufen ist, wird die absolute Ablaufzeit optional out-Parameter zusammen mit den ursprünglichen ungeschützten Daten zurückgegeben.</span><span class="sxs-lookup"><span data-stu-id="596eb-130">If the payload hasn't yet expired, the absolute expiration is returned as an optional out parameter along with the original unprotected data.</span></span> <span data-ttu-id="596eb-131">Wenn die Nutzlast abgelaufen ist, löst alle Überladungen der Methode Unprotect CryptographicException.</span><span class="sxs-lookup"><span data-stu-id="596eb-131">If the payload is expired, all overloads of the Unprotect method will throw CryptographicException.</span></span>

>[!WARNING]
> <span data-ttu-id="596eb-132">Es wird davon abgeraten, diese APIs verwenden, um Nutzlasten zu schützen, die langfristige oder unbestimmtes Persistenz erfordern.</span><span class="sxs-lookup"><span data-stu-id="596eb-132">It is not advised to use these APIs to protect payloads which require long-term or indefinite persistence.</span></span> <span data-ttu-id="596eb-133">"Kann ich für die geschützten Nutzlasten nach einem Monat dauerhaft nicht mehr wiederherstellbar sein leisten?"</span><span class="sxs-lookup"><span data-stu-id="596eb-133">"Can I afford for the protected payloads to be permanently unrecoverable after a month?"</span></span> <span data-ttu-id="596eb-134">kann als eine gute Faustregel dienen; Wenn die Antwort ist sollten keine dann Entwickler alternative APIs.</span><span class="sxs-lookup"><span data-stu-id="596eb-134">can serve as a good rule of thumb; if the answer is no then developers should consider alternative APIs.</span></span>

<span data-ttu-id="596eb-135">Im Beispiel unten verwendet die [nicht DI Codepfade](../configuration/non-di-scenarios.md) zum Instanziieren der Datenschutzsystem.</span><span class="sxs-lookup"><span data-stu-id="596eb-135">The sample below uses the [non-DI code paths](../configuration/non-di-scenarios.md) for instantiating the data protection system.</span></span> <span data-ttu-id="596eb-136">Um dieses Beispiel ausführen zu können, stellen Sie sicher, dass Sie zunächst einen Verweis auf das Paket Microsoft.AspNetCore.DataProtection.Extensions hinzugefügt haben.</span><span class="sxs-lookup"><span data-stu-id="596eb-136">To run this sample, ensure that you have first added a reference to the Microsoft.AspNetCore.DataProtection.Extensions package.</span></span>

[!code-csharp[Main](limited-lifetime-payloads/samples/limitedlifetimepayloads.cs)]
