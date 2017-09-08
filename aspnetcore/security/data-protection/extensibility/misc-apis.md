---
title: Verschiedene APIs
author: rick-anderson
description: 
keywords: ASP.NET Core
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 512c6ba7-88ec-47e4-a656-6b30350b34e6
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/extensibility/misc-apis
ms.openlocfilehash: 541dd721a00495632f0d633577b55933c9be03fa
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/11/2017
---
# <a name="miscellaneous-apis"></a><span data-ttu-id="f79c2-103">Verschiedene APIs</span><span class="sxs-lookup"><span data-stu-id="f79c2-103">Miscellaneous APIs</span></span>

<a name=data-protection-extensibility-mics-apis></a>

>[!WARNING]
> <span data-ttu-id="f79c2-104">Typen, die jede der folgenden Schnittstellen implementieren, sollten threadsichere werden für mehrere Aufrufer.</span><span class="sxs-lookup"><span data-stu-id="f79c2-104">Types that implement any of the following interfaces should be thread-safe for multiple callers.</span></span>

## <a name="isecret"></a><span data-ttu-id="f79c2-105">ISecret</span><span class="sxs-lookup"><span data-stu-id="f79c2-105">ISecret</span></span>

<span data-ttu-id="f79c2-106">Die Schnittstelle ISecret stellt einen Wert für den geheimen wie kryptografischen schlüsselmaterialien dar.</span><span class="sxs-lookup"><span data-stu-id="f79c2-106">The ISecret interface represents a secret value, such as cryptographic key material.</span></span> <span data-ttu-id="f79c2-107">Es enthält die folgenden API-Oberfläche.</span><span class="sxs-lookup"><span data-stu-id="f79c2-107">It contains the following API surface.</span></span>

* <span data-ttu-id="f79c2-108">Länge: Int</span><span class="sxs-lookup"><span data-stu-id="f79c2-108">Length : int</span></span>

* <span data-ttu-id="f79c2-109">Dispose(): "void"</span><span class="sxs-lookup"><span data-stu-id="f79c2-109">Dispose() : void</span></span>

* <span data-ttu-id="f79c2-110">WriteSecretIntoBuffer (ArraySegment<byte> Puffer): "void"</span><span class="sxs-lookup"><span data-stu-id="f79c2-110">WriteSecretIntoBuffer(ArraySegment<byte> buffer) : void</span></span>

<span data-ttu-id="f79c2-111">Die WriteSecretIntoBuffer-Methode füllt den bereitgestellten Puffer mit den für den geheimen Rohwert.</span><span class="sxs-lookup"><span data-stu-id="f79c2-111">The WriteSecretIntoBuffer method populates the supplied buffer with the raw secret value.</span></span> <span data-ttu-id="f79c2-112">Der Grund dafür, die diese API den Puffer als einen Parameter anstelle eines Byte [] direkt liegt darin erfordert, dass dadurch dem Aufrufer die Möglichkeit, das Pufferobjekt anheften zurückgeben Einschränken der geheimen Offenlegung der verwaltete Garbage Collector.</span><span class="sxs-lookup"><span data-stu-id="f79c2-112">The reason this API takes the buffer as a parameter rather than returning a byte[] directly is that this gives the caller the opportunity to pin the buffer object, limiting secret exposure to the managed garbage collector.</span></span>

<span data-ttu-id="f79c2-113">Der geheime Typ ist eine konkrete Implementierung der ISecret, in dem der Wert für den geheimen in-Process-Speicher gespeichert ist.</span><span class="sxs-lookup"><span data-stu-id="f79c2-113">The Secret type is a concrete implementation of ISecret where the secret value is stored in in-process memory.</span></span> <span data-ttu-id="f79c2-114">Auf Windows-Plattformen ist der Wert für den geheimen über verschlüsselt [CryptProtectMemory](https://msdn.microsoft.com/library/windows/desktop/aa380262(v=vs.85).aspx).</span><span class="sxs-lookup"><span data-stu-id="f79c2-114">On Windows platforms, the secret value is encrypted via [CryptProtectMemory](https://msdn.microsoft.com/library/windows/desktop/aa380262(v=vs.85).aspx).</span></span>
