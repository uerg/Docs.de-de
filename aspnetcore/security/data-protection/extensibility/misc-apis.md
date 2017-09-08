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
# <a name="miscellaneous-apis"></a>Verschiedene APIs

<a name=data-protection-extensibility-mics-apis></a>

>[!WARNING]
> Typen, die jede der folgenden Schnittstellen implementieren, sollten threadsichere werden für mehrere Aufrufer.

## <a name="isecret"></a>ISecret

Die Schnittstelle ISecret stellt einen Wert für den geheimen wie kryptografischen schlüsselmaterialien dar. Es enthält die folgenden API-Oberfläche.

* Länge: Int

* Dispose(): "void"

* WriteSecretIntoBuffer (ArraySegment<byte> Puffer): "void"

Die WriteSecretIntoBuffer-Methode füllt den bereitgestellten Puffer mit den für den geheimen Rohwert. Der Grund dafür, die diese API den Puffer als einen Parameter anstelle eines Byte [] direkt liegt darin erfordert, dass dadurch dem Aufrufer die Möglichkeit, das Pufferobjekt anheften zurückgeben Einschränken der geheimen Offenlegung der verwaltete Garbage Collector.

Der geheime Typ ist eine konkrete Implementierung der ISecret, in dem der Wert für den geheimen in-Process-Speicher gespeichert ist. Auf Windows-Plattformen ist der Wert für den geheimen über verschlüsselt [CryptProtectMemory](https://msdn.microsoft.com/library/windows/desktop/aa380262(v=vs.85).aspx).
