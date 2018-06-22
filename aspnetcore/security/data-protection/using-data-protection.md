---
title: Erste Schritte mit der Data Protection-APIs in ASP.NET Core
author: rick-anderson
description: Erfahren Sie, wie die ASP.NET Core Datenschutz-APIs zum Schützen und Aufheben des Schutzes Daten in eine app zu verwenden.
ms.author: riande
ms.date: 10/14/2016
uid: security/data-protection/using-data-protection
ms.openlocfilehash: ab2551d87d1a2cd22e9f421cabe0288311cb2ec3
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/20/2018
ms.locfileid: "36275748"
---
# <a name="get-started-with-the-data-protection-apis-in-aspnet-core"></a>Erste Schritte mit der Data Protection-APIs in ASP.NET Core

<a name="security-data-protection-getting-started"></a>

Die einfachste und schützen Daten besteht aus folgenden Schritten:

1. Erstellen Sie eine Schutzvorrichtung aus einen Datenschutzanbieter.

2. Rufen Sie die `Protect` Methode mit den Daten, die Sie schützen möchten.

3. Rufen Sie die `Unprotect` Methode mit den Daten zurück in Klartext umgewandelt werden sollen.

Die meisten Frameworks und app-Modelle, z. B. ASP.NET oder SignalR, bereits das Data Protection-System konfigurieren und einen Dienstcontainer, die den Zugriff über Abhängigkeitsinjektion hinzugefügt. Das folgende Beispiel zeigt einen Dienstcontainer für zielabhängigkeit konfigurieren und registrieren den Data Protection-Stapel, der Datenschutzanbieter über DI empfangen, erstellen eine Schutzvorrichtung und schützen und Aufheben des Schutzes von Daten

[!code-csharp[](../../security/data-protection/using-data-protection/samples/protectunprotect.cs?highlight=26,34,35,36,37,38,39,40)]

Wenn Sie eine Schutzvorrichtung erstellen müssen Sie angeben, eine oder mehrere [Zweck Zeichenfolgen](xref:security/data-protection/consumer-apis/purpose-strings). Eine Zeichenfolge Zweck wird eine Isolation zwischen Consumer. Beispielsweise wäre eine Schutzvorrichtung, die durch die Zeichenfolge "Grün" Zweck erstellt Aufheben des Schutzes von Daten, die durch eine Schutzvorrichtung bereitgestellt, mit dem Zweck "Violett" können nicht.

>[!TIP]
> Instanzen von `IDataProtectionProvider` und `IDataProtector` für mehrere Aufrufer threadsicher sind. Es ist vorgesehen, die nach eine Komponente einen Verweis auf Ruft eine `IDataProtector` über einen Aufruf an `CreateProtector`, verwenden sie diesen Verweis für mehrere Aufrufe von `Protect` und `Unprotect`.
>
>Ein Aufruf von `Unprotect` löst CryptographicException aus, wenn die geschützte Nutzlast überprüft oder entschlüsselt werden kann. Einige Komponenten Fehler ignorieren möchten Aufheben des Schutzes von während der Vorgänge eine Komponente, die Authentifizierungscookies liest möglicherweise diesen Fehler zu behandeln und die Anforderung zu behandeln, als ob es überhaupt kein Cookie hatte anstelle der sofortiges Anforderung schlägt fehl. Komponenten, die über dieses Verhalten soll sollten CryptographicException speziell statt Angriffspunkt alle Ausnahmen abfangen.
