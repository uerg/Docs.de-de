---
title: Verschiedene APIs
author: rick-anderson
description: In diesem Dokument werden die ASP.NET Core Data Protection ISecret-Schnittstelle.
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/extensibility/misc-apis
ms.openlocfilehash: 88a08a25abf4c4e1ba0746087b05b1cc8fa13024
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/19/2018
---
# <a name="miscellaneous-apis"></a><span data-ttu-id="ac4fd-103">Verschiedene APIs</span><span class="sxs-lookup"><span data-stu-id="ac4fd-103">Miscellaneous APIs</span></span>

<a name="data-protection-extensibility-mics-apis"></a>

>[!WARNING]
> <span data-ttu-id="ac4fd-104">Typen, die jede der folgenden Schnittstellen implementieren, sollten threadsichere werden für mehrere Aufrufer.</span><span class="sxs-lookup"><span data-stu-id="ac4fd-104">Types that implement any of the following interfaces should be thread-safe for multiple callers.</span></span>

## <a name="isecret"></a><span data-ttu-id="ac4fd-105">ISecret</span><span class="sxs-lookup"><span data-stu-id="ac4fd-105">ISecret</span></span>

<span data-ttu-id="ac4fd-106">Die `ISecret` Schnittstelle stellt einen Wert für den geheimen wie kryptografischen schlüsselmaterialien dar.</span><span class="sxs-lookup"><span data-stu-id="ac4fd-106">The `ISecret` interface represents a secret value, such as cryptographic key material.</span></span> <span data-ttu-id="ac4fd-107">Es enthält die folgenden API-Oberfläche:</span><span class="sxs-lookup"><span data-stu-id="ac4fd-107">It contains the following API surface:</span></span>

* <span data-ttu-id="ac4fd-108">`Length`: `int`</span><span class="sxs-lookup"><span data-stu-id="ac4fd-108">`Length`: `int`</span></span>

* <span data-ttu-id="ac4fd-109">`Dispose()`: `void`</span><span class="sxs-lookup"><span data-stu-id="ac4fd-109">`Dispose()`: `void`</span></span>

* <span data-ttu-id="ac4fd-110">`WriteSecretIntoBuffer(ArraySegment<byte> buffer)`: `void`</span><span class="sxs-lookup"><span data-stu-id="ac4fd-110">`WriteSecretIntoBuffer(ArraySegment<byte> buffer)`: `void`</span></span>

<span data-ttu-id="ac4fd-111">Die `WriteSecretIntoBuffer` Methode füllt den bereitgestellten Puffer mit den für den geheimen Rohwert.</span><span class="sxs-lookup"><span data-stu-id="ac4fd-111">The `WriteSecretIntoBuffer` method populates the supplied buffer with the raw secret value.</span></span> <span data-ttu-id="ac4fd-112">Der Grund, die diese API den Puffer als Parameter wird anstatt Zurückgeben einer `byte[]` direkt ist, dass dem Aufrufer die Möglichkeit, das Pufferobjekt Einschränken der geheimen Offenlegung der verwaltete Garbage Collector anheften können.</span><span class="sxs-lookup"><span data-stu-id="ac4fd-112">The reason this API takes the buffer as a parameter rather than returning a `byte[]` directly is that this gives the caller the opportunity to pin the buffer object, limiting secret exposure to the managed garbage collector.</span></span>

<span data-ttu-id="ac4fd-113">Die `Secret` Typ ist eine konkrete Implementierung der `ISecret` , in der geheime Wert in-Process-Speicher gespeichert ist.</span><span class="sxs-lookup"><span data-stu-id="ac4fd-113">The `Secret` type is a concrete implementation of `ISecret` where the secret value is stored in in-process memory.</span></span> <span data-ttu-id="ac4fd-114">Auf Windows-Plattformen ist der Wert für den geheimen über verschlüsselt [CryptProtectMemory](https://msdn.microsoft.com/library/windows/desktop/aa380262(v=vs.85).aspx).</span><span class="sxs-lookup"><span data-stu-id="ac4fd-114">On Windows platforms, the secret value is encrypted via [CryptProtectMemory](https://msdn.microsoft.com/library/windows/desktop/aa380262(v=vs.85).aspx).</span></span>
