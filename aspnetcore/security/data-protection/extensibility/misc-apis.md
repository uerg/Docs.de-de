---
title: Verschiedene APIs
author: rick-anderson
description: 
keywords: ASP.NET Core,
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 512c6ba7-88ec-47e4-a656-6b30350b34e6
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/extensibility/misc-apis
ms.openlocfilehash: 72274a0da94a14bcd5af11d245ba626214fa2607
ms.sourcegitcommit: 8f4d4fad1ca27adf9e396f5c205c9875a3963664
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/13/2017
---
# <a name="miscellaneous-apis"></a>Verschiedene APIs

<a name="data-protection-extensibility-mics-apis"></a>

>[!WARNING]
> Typen, die jede der folgenden Schnittstellen implementieren, sollten threadsichere werden für mehrere Aufrufer.

## <a name="isecret"></a>ISecret

Die Schnittstelle ISecret stellt einen Wert für den geheimen wie kryptografischen schlüsselmaterialien dar. Es enthält die folgenden API-Oberfläche.

* Länge: Int

* Dispose(): "void"

* WriteSecretIntoBuffer (ArraySegment<byte> Puffer): "void"

Die WriteSecretIntoBuffer-Methode füllt den bereitgestellten Puffer mit den für den geheimen Rohwert. Der Grund dafür, die diese API den Puffer als einen Parameter anstelle eines Byte [] direkt liegt darin erfordert, dass dadurch dem Aufrufer die Möglichkeit, das Pufferobjekt anheften zurückgeben Einschränken der geheimen Offenlegung der verwaltete Garbage Collector.

Der geheime Typ ist eine konkrete Implementierung der ISecret, in dem der Wert für den geheimen in-Process-Speicher gespeichert ist. Auf Windows-Plattformen ist der Wert für den geheimen über verschlüsselt [CryptProtectMemory](https://msdn.microsoft.com/library/windows/desktop/aa380262(v=vs.85).aspx).
