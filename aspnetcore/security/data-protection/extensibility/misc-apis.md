---
title: Sonstige ASP.NET Core Data Protection-APIs
author: rick-anderson
description: Informationen Sie zu ASP.NET Core Data Protection ISecret-Schnittstelle.
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/data-protection/extensibility/misc-apis
ms.openlocfilehash: 484c6a0979a10e7cf2b801873655caa99a42532c
ms.sourcegitcommit: 48beecfe749ddac52bc79aa3eb246a2dcdaa1862
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/22/2018
---
# <a name="miscellaneous-aspnet-core-data-protection-apis"></a>Sonstige ASP.NET Core Data Protection-APIs

<a name="data-protection-extensibility-mics-apis"></a>

>[!WARNING]
> Typen, die jede der folgenden Schnittstellen implementieren, sollten threadsichere werden für mehrere Aufrufer.

## <a name="isecret"></a>ISecret

Die `ISecret` Schnittstelle stellt einen Wert für den geheimen wie kryptografischen schlüsselmaterialien dar. Es enthält die folgenden API-Oberfläche:

* `Length`: `int`

* `Dispose()`: `void`

* `WriteSecretIntoBuffer(ArraySegment<byte> buffer)`: `void`

Die `WriteSecretIntoBuffer` Methode füllt den bereitgestellten Puffer mit den für den geheimen Rohwert. Der Grund, die diese API den Puffer als Parameter wird anstatt Zurückgeben einer `byte[]` direkt ist, dass dem Aufrufer die Möglichkeit, das Pufferobjekt Einschränken der geheimen Offenlegung der verwaltete Garbage Collector anheften können.

Die `Secret` Typ ist eine konkrete Implementierung der `ISecret` , in der geheime Wert in-Process-Speicher gespeichert ist. Auf Windows-Plattformen ist der Wert für den geheimen über verschlüsselt [CryptProtectMemory](https://msdn.microsoft.com/library/windows/desktop/aa380262(v=vs.85).aspx).
