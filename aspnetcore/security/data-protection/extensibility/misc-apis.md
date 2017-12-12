---
title: Verschiedene APIs
author: rick-anderson
description: In diesem Dokument werden die ASP.NET Core Data Protection ISecret-Schnittstelle.
keywords: ASP.NET Core, ISecret den Schutz von Daten
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 512c6ba7-88ec-47e4-a656-6b30350b34e6
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/extensibility/misc-apis
ms.openlocfilehash: f5d6920f9f229bd480a76c952dab30efb7d9eff5
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
# <a name="miscellaneous-apis"></a><span data-ttu-id="15dea-104">Verschiedene APIs</span><span class="sxs-lookup"><span data-stu-id="15dea-104">Miscellaneous APIs</span></span>

<a name="data-protection-extensibility-mics-apis"></a>

>[!WARNING]
> <span data-ttu-id="15dea-105">Typen, die jede der folgenden Schnittstellen implementieren, sollten threadsichere werden für mehrere Aufrufer.</span><span class="sxs-lookup"><span data-stu-id="15dea-105">Types that implement any of the following interfaces should be thread-safe for multiple callers.</span></span>

## <a name="isecret"></a><span data-ttu-id="15dea-106">ISecret</span><span class="sxs-lookup"><span data-stu-id="15dea-106">ISecret</span></span>

<span data-ttu-id="15dea-107">Die `ISecret` Schnittstelle stellt einen Wert für den geheimen wie kryptografischen schlüsselmaterialien dar.</span><span class="sxs-lookup"><span data-stu-id="15dea-107">The `ISecret` interface represents a secret value, such as cryptographic key material.</span></span> <span data-ttu-id="15dea-108">Es enthält die folgenden API-Oberfläche:</span><span class="sxs-lookup"><span data-stu-id="15dea-108">It contains the following API surface:</span></span>

* <span data-ttu-id="15dea-109">`Length`: `int`</span><span class="sxs-lookup"><span data-stu-id="15dea-109">`Length`: `int`</span></span>

* <span data-ttu-id="15dea-110">`Dispose()`: `void`</span><span class="sxs-lookup"><span data-stu-id="15dea-110">`Dispose()`: `void`</span></span>

* <span data-ttu-id="15dea-111">`WriteSecretIntoBuffer(ArraySegment<byte> buffer)`: `void`</span><span class="sxs-lookup"><span data-stu-id="15dea-111">`WriteSecretIntoBuffer(ArraySegment<byte> buffer)`: `void`</span></span>

<span data-ttu-id="15dea-112">Die `WriteSecretIntoBuffer` Methode füllt den bereitgestellten Puffer mit den für den geheimen Rohwert.</span><span class="sxs-lookup"><span data-stu-id="15dea-112">The `WriteSecretIntoBuffer` method populates the supplied buffer with the raw secret value.</span></span> <span data-ttu-id="15dea-113">Der Grund, die diese API den Puffer als Parameter wird anstatt Zurückgeben einer `byte[]` direkt ist, dass dem Aufrufer die Möglichkeit, das Pufferobjekt Einschränken der geheimen Offenlegung der verwaltete Garbage Collector anheften können.</span><span class="sxs-lookup"><span data-stu-id="15dea-113">The reason this API takes the buffer as a parameter rather than returning a `byte[]` directly is that this gives the caller the opportunity to pin the buffer object, limiting secret exposure to the managed garbage collector.</span></span>

<span data-ttu-id="15dea-114">Die `Secret` Typ ist eine konkrete Implementierung der `ISecret` , in der geheime Wert in-Process-Speicher gespeichert ist.</span><span class="sxs-lookup"><span data-stu-id="15dea-114">The `Secret` type is a concrete implementation of `ISecret` where the secret value is stored in in-process memory.</span></span> <span data-ttu-id="15dea-115">Auf Windows-Plattformen ist der Wert für den geheimen über verschlüsselt [CryptProtectMemory](https://msdn.microsoft.com/library/windows/desktop/aa380262(v=vs.85).aspx).</span><span class="sxs-lookup"><span data-stu-id="15dea-115">On Windows platforms, the secret value is encrypted via [CryptProtectMemory](https://msdn.microsoft.com/library/windows/desktop/aa380262(v=vs.85).aspx).</span></span>
