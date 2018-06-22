---
title: Sonstige ASP.NET Core Data Protection-APIs
author: rick-anderson
description: Informationen Sie zu ASP.NET Core Data Protection ISecret-Schnittstelle.
ms.author: riande
ms.date: 10/14/2016
uid: security/data-protection/extensibility/misc-apis
ms.openlocfilehash: 114cdd6209970e46b827e403fbe79b95692d0242
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/20/2018
ms.locfileid: "36279154"
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
