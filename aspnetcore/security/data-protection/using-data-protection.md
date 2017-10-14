---
title: Erste Schritte mit Data Protection-APIs
author: rick-anderson
description: 
keywords: ASP.NET Core,
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 39b7a73c-29d4-4137-b311-49529adcf3cb
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/using-data-protection
ms.openlocfilehash: 9489b55b1de626b77bcbe21cb9678e561b559099
ms.sourcegitcommit: 8f4d4fad1ca27adf9e396f5c205c9875a3963664
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/13/2017
---
# <a name="getting-started-with-the-data-protection-apis"></a>Erste Schritte mit Data Protection-APIs

<a name="security-data-protection-getting-started"></a>

Der einfachste Schutz Daten besteht aus der folgenden Schritte aus:

1. Erstellen Sie eine Schutzvorrichtung aus einen Datenschutzanbieter.

2. Rufen Sie die Protect-Methode mit den Daten, die Sie schützen möchten.

3. Rufen Sie die Unprotect-Methode mit den Daten, die Sie in nur-Text wieder aktivieren möchten.

Die meisten Frameworks wie ASP.NET oder SignalR bereits das Data Protection-System konfigurieren und einen Dienstcontainer, die den Zugriff über Abhängigkeitsinjektion hinzugefügt. Das folgende Beispiel zeigt einen Dienstcontainer für zielabhängigkeit konfigurieren und registrieren den Data Protection-Stapel, der Datenschutzanbieter über DI empfangen, erstellen eine Schutzvorrichtung und schützen und Aufheben des Schutzes von Daten

[!code-csharp[Main](../../security/data-protection/using-data-protection/samples/protectunprotect.cs?highlight=26,34,35,36,37,38,39,40)]

Wenn Sie eine Schutzvorrichtung erstellen müssen Sie angeben, eine oder mehrere [Zweck Zeichenfolgen](consumer-apis/purpose-strings.md). Eine Zeichenfolge Zweck wird eine Isolation zwischen den Consumern, z. B. eine Schutzvorrichtung erstellt mit einer Zeichenfolge Zweck "Grün" würde nicht zum Aufheben des Schutzes von Daten, die durch eine Schutzvorrichtung bereitgestellt, mit dem Zweck "Violett".

>[!TIP]
> Instanzen von IDataProtectionProvider und IDataProtector sind threadsicher für mehrere Aufrufer. Es richtet sich an, dass nach eine Komponente einen Verweis auf einen IDataProtector über einen Aufruf an CreateProtector abruft, diesen Verweis für mehrere Aufrufe von Schutz- und Unprotect verwendet wird.
>
>Ein Aufruf von Unprotect löst CryptographicException aus, wenn die geschützte Nutzlast überprüft oder entschlüsselt werden kann. Einige Komponenten Fehler ignorieren möchten Aufheben des Schutzes von während der Vorgänge eine Komponente, die Authentifizierungscookies liest möglicherweise diesen Fehler zu behandeln und die Anforderung zu behandeln, als ob es überhaupt kein Cookie hatte anstelle der sofortiges Anforderung schlägt fehl. Komponenten, die über dieses Verhalten soll sollten CryptographicException speziell statt Angriffspunkt alle Ausnahmen abfangen.
