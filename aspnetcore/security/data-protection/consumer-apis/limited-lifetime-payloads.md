---
title: "Beschränken die Lebensdauer des geschützten Nutzlasten"
author: rick-anderson
description: 
keywords: ASP.NET Core
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 000175e2-10fc-43dd-bfc2-51e004b97b44
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/consumer-apis/limited-lifetime-payloads
ms.openlocfilehash: 4ff13803b328c1e9dd2934c38c88b43f5798de03
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/11/2017
---
# <a name="limiting-the-lifetime-of-protected-payloads"></a>Beschränken die Lebensdauer des geschützten Nutzlasten

Es gibt Szenarien, bei denen der Anwendungsentwickler möchte eine geschützte Nutzlast zu erstellen, die nach einem festgelegten Zeitraum abläuft. Geschützte Nutzlast könnte z. B. ein kennwortzurücksetzungstoken darstellen, die nur für eine Stunde gültig sein soll. Es ist sicherlich möglich, dass der Entwickler, ihre eigenen nutzlastformat zu erstellen, ein eingebettetes Ablaufdatum enthält, und erfahrene Entwickler und trotzdem fortsetzen möchten, jedoch für die meisten Entwickler verwalten diese Ablaufzeiten kann erweitert mühsam.

Um dies für unsere entwicklerzielgruppe zu vereinfachen, enthält das Paket Microsoft.AspNetCore.DataProtection.Extensions Dienstprogramm-APIs zum Erstellen von Nutzlasten, die automatisch nach einem festgelegten Zeitraum abläuft. Diese APIs hängen von der ITimeLimitedDataProtector-Typ.

## <a name="api-usage"></a>Zur Verwendung der API

Die ITimeLimitedDataProtector-Schnittstelle ist die Kernschnittstelle für schützen und Aufheben des Schutzes zeitlich begrenzte / Self-Nutzlasten abläuft. Um eine Instanz einer ITimeLimitedDataProtector erstellen zu können, müssen zunächst benötigen Sie eine Instanz von einer regulären [IDataProtector](overview.md) mit einem bestimmten Zweck erstellt. Sobald die IDataProtector-Instanz verfügbar ist, rufen Sie die IDataProtector.ToTimeLimitedDataProtector-Erweiterungsmethode, um wieder eine Schutzvorrichtung mit Ablauf von integrierten Funktionen.

ITimeLimitedDataProtector macht die folgenden API-Oberfläche und die Erweiterung der Methoden verfügbar:

* CreateProtector (Zeichenfolge Zweck): Diese ITimeLimitedDataProtector-API ähnelt der vorhandenen IDataProtectionProvider.CreateProtector, da sie zum Erstellen verwendet werden kann [Zweck Ketten](purpose-strings.md) über einen Stamm zeitlich begrenzte Schutzvorrichtung.

* Schützen (Byte [] als nur-Text "DateTimeOffset" Ablauf): Byte]

* Schützen (Byte []-nur-Text, TimeSpan-Lebensdauer): Byte]

* Schützen (Byte [] Klartext): Byte]

* Schützen (Zeichenfolge als nur-Text "DateTimeOffset" Ablauf): Zeichenfolge

* Schützen (Zeichenfolge als nur-Text, TimeSpan-Lebensdauer): Zeichenfolge

* Schützen (Klartext Zeichenfolge): Zeichenfolge

Zusätzlich zu den Methoden der Core-Protect, die dies nur den nur-Text in Anspruch nehmen, sind es neue Überladungen, die es ermöglichen, die Nutzlast Ablaufdatum angeben. Das Ablaufdatum kann als ein absolutes Datum (über eine "DateTimeOffset") oder als eine relative Zeit (in die aktuelle Systemzeit, über einen TimeSpan-Wert) angegeben werden. Wenn eine Überladung, die eine Ablaufzeit wird nicht aufgerufen wird, ist die Nutzlast davon ausgegangen, dass nie ablaufen.

* Aufheben des Schutzes (Byte [] ProtectedData, "DateTimeOffset" Ablauf): Byte]

* Aufheben des Schutzes (Byte [] ProtectedData): Byte]

* Aufheben des Schutzes (out "DateTimeOffset" Ablaufdatum Zeichenfolge ProtectedData): Zeichenfolge

* Aufheben des Schutzes (Zeichenfolge ProtectedData): Zeichenfolge

Die ursprünglichen ungeschützten Daten zurück, die Unprotect-Methoden. Wenn die Nutzlast nicht noch abgelaufen ist, wird die absolute Ablaufzeit optional out-Parameter zusammen mit den ursprünglichen ungeschützten Daten zurückgegeben. Wenn die Nutzlast abgelaufen ist, löst alle Überladungen der Methode Unprotect CryptographicException.

>[!WARNING]
> Es wird davon abgeraten, diese APIs verwenden, um Nutzlasten zu schützen, die langfristige oder unbestimmtes Persistenz erfordern. "Kann ich für die geschützten Nutzlasten nach einem Monat dauerhaft nicht mehr wiederherstellbar sein leisten?" kann als eine gute Faustregel dienen; Wenn die Antwort ist sollten keine dann Entwickler alternative APIs.

Im Beispiel unten verwendet die [nicht DI Codepfade](../configuration/non-di-scenarios.md) zum Instanziieren der Datenschutzsystem. Um dieses Beispiel ausführen zu können, stellen Sie sicher, dass Sie zunächst einen Verweis auf das Paket Microsoft.AspNetCore.DataProtection.Extensions hinzugefügt haben.

[!code-none[Main](limited-lifetime-payloads/samples/limitedlifetimepayloads.cs)]
