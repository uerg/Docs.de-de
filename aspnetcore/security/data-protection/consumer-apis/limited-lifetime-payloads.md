---
title: "Beschränken die Lebensdauer des geschützten Nutzlasten"
author: rick-anderson
description: "Dieses Dokument wird erläutert, wie die Lebensdauer einer geschützten Nutzlast, die mit der ASP.NET Core-Datenschutz APIs beschränkt."
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/data-protection/consumer-apis/limited-lifetime-payloads
ms.openlocfilehash: 812d0373d24c8578bae83db4876549246f189be3
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2018
---
# <a name="limiting-the-lifetime-of-protected-payloads"></a><span data-ttu-id="ce7a3-103">Beschränken die Lebensdauer des geschützten Nutzlasten</span><span class="sxs-lookup"><span data-stu-id="ce7a3-103">Limiting the lifetime of protected payloads</span></span>

<span data-ttu-id="ce7a3-104">Es gibt Szenarien, bei denen der Anwendungsentwickler möchte eine geschützte Nutzlast zu erstellen, die nach einem festgelegten Zeitraum abläuft.</span><span class="sxs-lookup"><span data-stu-id="ce7a3-104">There are scenarios where the application developer wants to create a protected payload that expires after a set period of time.</span></span> <span data-ttu-id="ce7a3-105">Geschützte Nutzlast könnte z. B. ein kennwortzurücksetzungstoken darstellen, die nur für eine Stunde gültig sein soll.</span><span class="sxs-lookup"><span data-stu-id="ce7a3-105">For instance, the protected payload might represent a password reset token that should only be valid for one hour.</span></span> <span data-ttu-id="ce7a3-106">Es ist sicherlich möglich, dass der Entwickler, ihre eigenen nutzlastformat zu erstellen, ein eingebettetes Ablaufdatum enthält, und erfahrene Entwickler und trotzdem fortsetzen möchten, jedoch für die meisten Entwickler verwalten diese Ablaufzeiten kann erweitert mühsam.</span><span class="sxs-lookup"><span data-stu-id="ce7a3-106">It's certainly possible for the developer to create their own payload format that contains an embedded expiration date, and advanced developers may wish to do this anyway, but for the majority of developers managing these expirations can grow tedious.</span></span>

<span data-ttu-id="ce7a3-107">Um dies für unsere entwicklerzielgruppe, das Paket vereinfachen [Microsoft.AspNetCore.DataProtection.Extensions](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.Extensions/) enthält Dienstprogramm-APIs zum Erstellen von Nutzlasten, das automatisch nach einem festgelegten Zeitraum abläuft.</span><span class="sxs-lookup"><span data-stu-id="ce7a3-107">To make this easier for our developer audience, the package [Microsoft.AspNetCore.DataProtection.Extensions](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.Extensions/) contains utility APIs for creating payloads that automatically expire after a set period of time.</span></span> <span data-ttu-id="ce7a3-108">Diese APIs der hängen die `ITimeLimitedDataProtector` Typ.</span><span class="sxs-lookup"><span data-stu-id="ce7a3-108">These APIs hang off of the `ITimeLimitedDataProtector` type.</span></span>

## <a name="api-usage"></a><span data-ttu-id="ce7a3-109">Zur Verwendung der API</span><span class="sxs-lookup"><span data-stu-id="ce7a3-109">API usage</span></span>

<span data-ttu-id="ce7a3-110">Die `ITimeLimitedDataProtector` Schnittstelle ist die Kernschnittstelle zum Schützen und Aufheben des Schutzes zeitlich begrenzte / selbst ablaufender-Nutzlasten.</span><span class="sxs-lookup"><span data-stu-id="ce7a3-110">The `ITimeLimitedDataProtector` interface is the core interface for protecting and unprotecting time-limited / self-expiring payloads.</span></span> <span data-ttu-id="ce7a3-111">Zum Erstellen einer Instanz von einem `ITimeLimitedDataProtector`, Sie benötigen zunächst eine Instanz von einer regulären [IDataProtector](overview.md) mit einem bestimmten Zweck erstellt.</span><span class="sxs-lookup"><span data-stu-id="ce7a3-111">To create an instance of an `ITimeLimitedDataProtector`, you'll first need an instance of a regular [IDataProtector](overview.md) constructed with a specific purpose.</span></span> <span data-ttu-id="ce7a3-112">Einmal die `IDataProtector` Instanz verfügbar ist, rufen die `IDataProtector.ToTimeLimitedDataProtector` Erweiterungsmethode, um wieder eine Schutzvorrichtung mit Ablauf von integrierten Funktionen zugreifen.</span><span class="sxs-lookup"><span data-stu-id="ce7a3-112">Once the `IDataProtector` instance is available, call the `IDataProtector.ToTimeLimitedDataProtector` extension method to get back a protector with built-in expiration capabilities.</span></span>

<span data-ttu-id="ce7a3-113">`ITimeLimitedDataProtector`macht die folgenden API-Oberfläche und die Erweiterung der Methoden verfügbar:</span><span class="sxs-lookup"><span data-stu-id="ce7a3-113">`ITimeLimitedDataProtector` exposes the following API surface and extension methods:</span></span>

* <span data-ttu-id="ce7a3-114">CreateProtector (Zeichenfolge Zweck): ITimeLimitedDataProtector - diese API ist ähnlich den vorhandenen `IDataProtectionProvider.CreateProtector` , da er verwendet werden kann, erstellen [Zweck Ketten](purpose-strings.md) über einen Stamm zeitlich begrenzte Schutzvorrichtung.</span><span class="sxs-lookup"><span data-stu-id="ce7a3-114">CreateProtector(string purpose) : ITimeLimitedDataProtector - This API is similar to the existing `IDataProtectionProvider.CreateProtector` in that it can be used to create [purpose chains](purpose-strings.md) from a root time-limited protector.</span></span>

* <span data-ttu-id="ce7a3-115">Protect(byte[] plaintext, DateTimeOffset expiration) : byte[]</span><span class="sxs-lookup"><span data-stu-id="ce7a3-115">Protect(byte[] plaintext, DateTimeOffset expiration) : byte[]</span></span>

* <span data-ttu-id="ce7a3-116">Protect(byte[] plaintext, TimeSpan lifetime) : byte[]</span><span class="sxs-lookup"><span data-stu-id="ce7a3-116">Protect(byte[] plaintext, TimeSpan lifetime) : byte[]</span></span>

* <span data-ttu-id="ce7a3-117">Protect(byte[] plaintext) : byte[]</span><span class="sxs-lookup"><span data-stu-id="ce7a3-117">Protect(byte[] plaintext) : byte[]</span></span>

* <span data-ttu-id="ce7a3-118">Schützen (Zeichenfolge als nur-Text "DateTimeOffset" Ablauf): Zeichenfolge</span><span class="sxs-lookup"><span data-stu-id="ce7a3-118">Protect(string plaintext, DateTimeOffset expiration) : string</span></span>

* <span data-ttu-id="ce7a3-119">Schützen (Zeichenfolge als nur-Text, TimeSpan-Lebensdauer): Zeichenfolge</span><span class="sxs-lookup"><span data-stu-id="ce7a3-119">Protect(string plaintext, TimeSpan lifetime) : string</span></span>

* <span data-ttu-id="ce7a3-120">Schützen (Klartext Zeichenfolge): Zeichenfolge</span><span class="sxs-lookup"><span data-stu-id="ce7a3-120">Protect(string plaintext) : string</span></span>

<span data-ttu-id="ce7a3-121">Zusätzlich zu den Kern `Protect` Methoden, die nur die als nur-Text werden es sind neue Überladungen, die es ermöglichen, die Nutzlast Ablaufdatum angeben.</span><span class="sxs-lookup"><span data-stu-id="ce7a3-121">In addition to the core `Protect` methods which take only the plaintext, there are new overloads which allow specifying the payload's expiration date.</span></span> <span data-ttu-id="ce7a3-122">Das Ablaufdatum kann als ein absolutes Datum angegeben werden (über eine `DateTimeOffset`) oder als ein relativer Zeitpunkt (aus dem aktuellen System Zeit, über eine `TimeSpan`).</span><span class="sxs-lookup"><span data-stu-id="ce7a3-122">The expiration date can be specified as an absolute date (via a `DateTimeOffset`) or as a relative time (from the current system time, via a `TimeSpan`).</span></span> <span data-ttu-id="ce7a3-123">Wenn eine Überladung, die eine Ablaufzeit wird nicht aufgerufen wird, ist die Nutzlast davon ausgegangen, dass nie ablaufen.</span><span class="sxs-lookup"><span data-stu-id="ce7a3-123">If an overload which doesn't take an expiration is called, the payload is assumed never to expire.</span></span>

* <span data-ttu-id="ce7a3-124">Unprotect(byte[] protectedData, out DateTimeOffset expiration) : byte[]</span><span class="sxs-lookup"><span data-stu-id="ce7a3-124">Unprotect(byte[] protectedData, out DateTimeOffset expiration) : byte[]</span></span>

* <span data-ttu-id="ce7a3-125">Unprotect(byte[] protectedData) : byte[]</span><span class="sxs-lookup"><span data-stu-id="ce7a3-125">Unprotect(byte[] protectedData) : byte[]</span></span>

* <span data-ttu-id="ce7a3-126">Aufheben des Schutzes (out "DateTimeOffset" Ablaufdatum Zeichenfolge ProtectedData): Zeichenfolge</span><span class="sxs-lookup"><span data-stu-id="ce7a3-126">Unprotect(string protectedData, out DateTimeOffset expiration) : string</span></span>

* <span data-ttu-id="ce7a3-127">Aufheben des Schutzes (Zeichenfolge ProtectedData): Zeichenfolge</span><span class="sxs-lookup"><span data-stu-id="ce7a3-127">Unprotect(string protectedData) : string</span></span>

<span data-ttu-id="ce7a3-128">Die `Unprotect` Methoden die ursprünglichen ungeschützten Daten zurück.</span><span class="sxs-lookup"><span data-stu-id="ce7a3-128">The `Unprotect` methods return the original unprotected data.</span></span> <span data-ttu-id="ce7a3-129">Wenn die Nutzlast nicht noch abgelaufen ist, wird die absolute Ablaufzeit optional out-Parameter zusammen mit den ursprünglichen ungeschützten Daten zurückgegeben.</span><span class="sxs-lookup"><span data-stu-id="ce7a3-129">If the payload hasn't yet expired, the absolute expiration is returned as an optional out parameter along with the original unprotected data.</span></span> <span data-ttu-id="ce7a3-130">Wenn die Nutzlast abgelaufen ist, löst alle Überladungen der Methode Unprotect CryptographicException.</span><span class="sxs-lookup"><span data-stu-id="ce7a3-130">If the payload is expired, all overloads of the Unprotect method will throw CryptographicException.</span></span>

>[!WARNING]
> <span data-ttu-id="ce7a3-131">Es wurde nicht empfohlen. diese APIs verwenden, um Nutzlasten zu schützen, die langfristige oder unbestimmtes Persistenz erfordern.</span><span class="sxs-lookup"><span data-stu-id="ce7a3-131">It's not advised to use these APIs to protect payloads which require long-term or indefinite persistence.</span></span> <span data-ttu-id="ce7a3-132">"Kann ich für die geschützten Nutzlasten nach einem Monat dauerhaft nicht mehr wiederherstellbar sein leisten?"</span><span class="sxs-lookup"><span data-stu-id="ce7a3-132">"Can I afford for the protected payloads to be permanently unrecoverable after a month?"</span></span> <span data-ttu-id="ce7a3-133">kann als eine gute Faustregel dienen; Wenn die Antwort ist sollten keine dann Entwickler alternative APIs.</span><span class="sxs-lookup"><span data-stu-id="ce7a3-133">can serve as a good rule of thumb; if the answer is no then developers should consider alternative APIs.</span></span>

<span data-ttu-id="ce7a3-134">Im Beispiel unten verwendet die [nicht DI Codepfade](../configuration/non-di-scenarios.md) zum Instanziieren der Datenschutzsystem.</span><span class="sxs-lookup"><span data-stu-id="ce7a3-134">The sample below uses the [non-DI code paths](../configuration/non-di-scenarios.md) for instantiating the data protection system.</span></span> <span data-ttu-id="ce7a3-135">Um dieses Beispiel ausführen zu können, stellen Sie sicher, dass Sie zunächst einen Verweis auf das Paket Microsoft.AspNetCore.DataProtection.Extensions hinzugefügt haben.</span><span class="sxs-lookup"><span data-stu-id="ce7a3-135">To run this sample, ensure that you have first added a reference to the Microsoft.AspNetCore.DataProtection.Extensions package.</span></span>

[!code-csharp[Main](limited-lifetime-payloads/samples/limitedlifetimepayloads.cs)]
