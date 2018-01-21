---
title: "Beschränken die Lebensdauer des geschützten Nutzlasten"
author: rick-anderson
description: "Dieses Dokument wird erläutert, wie die Lebensdauer einer geschützten Nutzlast, die mit der ASP.NET Core-Datenschutz APIs beschränkt."
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/consumer-apis/limited-lifetime-payloads
ms.openlocfilehash: 144097cd1551c1d0aece5df20ce01e14146a41d1
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/19/2018
---
# <a name="limiting-the-lifetime-of-protected-payloads"></a>Beschränken die Lebensdauer des geschützten Nutzlasten

Es gibt Szenarien, bei denen der Anwendungsentwickler möchte eine geschützte Nutzlast zu erstellen, die nach einem festgelegten Zeitraum abläuft. Geschützte Nutzlast könnte z. B. ein kennwortzurücksetzungstoken darstellen, die nur für eine Stunde gültig sein soll. Es ist sicherlich möglich, dass der Entwickler, ihre eigenen nutzlastformat zu erstellen, ein eingebettetes Ablaufdatum enthält, und erfahrene Entwickler und trotzdem fortsetzen möchten, jedoch für die meisten Entwickler verwalten diese Ablaufzeiten kann erweitert mühsam.

Um dies für unsere entwicklerzielgruppe, das Paket vereinfachen [Microsoft.AspNetCore.DataProtection.Extensions](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.Extensions/) enthält Dienstprogramm-APIs zum Erstellen von Nutzlasten, das automatisch nach einem festgelegten Zeitraum abläuft. Diese APIs der hängen die `ITimeLimitedDataProtector` Typ.

## <a name="api-usage"></a>Zur Verwendung der API

Die `ITimeLimitedDataProtector` Schnittstelle ist die Kernschnittstelle zum Schützen und Aufheben des Schutzes zeitlich begrenzte / selbst ablaufender-Nutzlasten. Zum Erstellen einer Instanz von einem `ITimeLimitedDataProtector`, Sie benötigen zunächst eine Instanz von einer regulären [IDataProtector](overview.md) mit einem bestimmten Zweck erstellt. Einmal die `IDataProtector` Instanz verfügbar ist, rufen die `IDataProtector.ToTimeLimitedDataProtector` Erweiterungsmethode, um wieder eine Schutzvorrichtung mit Ablauf von integrierten Funktionen zugreifen.

`ITimeLimitedDataProtector`macht die folgenden API-Oberfläche und die Erweiterung der Methoden verfügbar:

* CreateProtector (Zeichenfolge Zweck): ITimeLimitedDataProtector - diese API ist ähnlich den vorhandenen `IDataProtectionProvider.CreateProtector` , da er verwendet werden kann, erstellen [Zweck Ketten](purpose-strings.md) über einen Stamm zeitlich begrenzte Schutzvorrichtung.

* Protect(byte[] plaintext, DateTimeOffset expiration) : byte[]

* Protect(byte[] plaintext, TimeSpan lifetime) : byte[]

* Protect(byte[] plaintext) : byte[]

* Schützen (Zeichenfolge als nur-Text "DateTimeOffset" Ablauf): Zeichenfolge

* Schützen (Zeichenfolge als nur-Text, TimeSpan-Lebensdauer): Zeichenfolge

* Schützen (Klartext Zeichenfolge): Zeichenfolge

Zusätzlich zu den Kern `Protect` Methoden, die nur die als nur-Text werden es sind neue Überladungen, die es ermöglichen, die Nutzlast Ablaufdatum angeben. Das Ablaufdatum kann als ein absolutes Datum angegeben werden (über eine `DateTimeOffset`) oder als ein relativer Zeitpunkt (aus dem aktuellen System Zeit, über eine `TimeSpan`). Wenn eine Überladung, die eine Ablaufzeit wird nicht aufgerufen wird, ist die Nutzlast davon ausgegangen, dass nie ablaufen.

* Unprotect(byte[] protectedData, out DateTimeOffset expiration) : byte[]

* Unprotect(byte[] protectedData) : byte[]

* Aufheben des Schutzes (out "DateTimeOffset" Ablaufdatum Zeichenfolge ProtectedData): Zeichenfolge

* Aufheben des Schutzes (Zeichenfolge ProtectedData): Zeichenfolge

Die `Unprotect` Methoden die ursprünglichen ungeschützten Daten zurück. Wenn die Nutzlast nicht noch abgelaufen ist, wird die absolute Ablaufzeit optional out-Parameter zusammen mit den ursprünglichen ungeschützten Daten zurückgegeben. Wenn die Nutzlast abgelaufen ist, löst alle Überladungen der Methode Unprotect CryptographicException.

>[!WARNING]
> Es wird davon abgeraten, diese APIs verwenden, um Nutzlasten zu schützen, die langfristige oder unbestimmtes Persistenz erfordern. "Kann ich für die geschützten Nutzlasten nach einem Monat dauerhaft nicht mehr wiederherstellbar sein leisten?" kann als eine gute Faustregel dienen; Wenn die Antwort ist sollten keine dann Entwickler alternative APIs.

Im Beispiel unten verwendet die [nicht DI Codepfade](../configuration/non-di-scenarios.md) zum Instanziieren der Datenschutzsystem. Um dieses Beispiel ausführen zu können, stellen Sie sicher, dass Sie zunächst einen Verweis auf das Paket Microsoft.AspNetCore.DataProtection.Extensions hinzugefügt haben.

[!code-csharp[Main](limited-lifetime-payloads/samples/limitedlifetimepayloads.cs)]
