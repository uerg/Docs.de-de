---
title: "Übersicht über die Consumer-APIs"
author: rick-anderson
description: "Dieses Dokument enthält eine kurze Übersicht über die verschiedenen Consumer APIs, die innerhalb der ASP.NET Core Data Protection-Bibliothek verfügbar."
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/data-protection/consumer-apis/overview
ms.openlocfilehash: 7f335681581b73e36e5b4deaf513255770900965
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2018
---
# <a name="consumer-apis-overview"></a>Übersicht über die Consumer-APIs

Die `IDataProtectionProvider` und `IDataProtector` sind die grundlegenden Schnittstellen, die über die Consumer die Datenschutzsystem verwenden. Sie sind im Verzeichnis der [Microsoft.AspNetCore.DataProtection.Abstractions](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.Abstractions/) Paket.

## <a name="idataprotectionprovider"></a>IDataProtectionProvider

Die Provider-Schnittstelle stellt den Stamm der Datenschutzsystem dar. Er kann nicht direkt zu schützen oder Aufheben des Schutzes von Daten verwendet werden. Stattdessen muss der Consumer Abrufen eines Verweises auf ein `IDataProtector` durch Aufrufen von `IDataProtectionProvider.CreateProtector(purpose)`, wobei der Zweck einer Zeichenfolge ist, die die beabsichtigte Consumer Anwendungsfall beschreibt. Finden Sie unter [Zweck Zeichenfolgen](purpose-strings.md) viel mehr Informationen auf der Absicht dieses Parameters und wie Sie einen geeigneten Wert auswählen.

## <a name="idataprotector"></a>IDataProtector

Die Schutzvorrichtung-Schnittstelle wird zurückgegeben, durch den Aufruf von `CreateProtector`, und den zugehörigen diese Schnittstelle, über die Consumer ausführen können schützen und Aufheben des Schutzes Vorgänge.

Um Daten zu schützen, übergeben Sie die Daten in die `Protect` Methode. Die grundlegende Schnittstelle definiert eine Methode an, welche konvertiert Byte [] -> Byte [], aber es ist auch eine Überladung (angegeben als Erweiterungsmethode einer) der Zeichenfolge konvertiert-Zeichenfolge >. Die Sicherheit durch die beiden Methoden ist identisch. Entwickler sollten auswählen, unabhängig davon, welche Überladung am einfachsten für ihre Verwendungsfall ist. Unabhängig von der Überladung gewählt, den Rückgabewert von schützen Methode jetzt geschützt ist (enciphered und manipulationssichere geprüft) und die Anwendung auf einem nicht vertrauenswürdigen Client senden.

Übergeben Sie zum Aufheben des Schutzes von einer zuvor geschützten Datenmenge, die geschützten Daten auf die `Unprotect` Methode. (Es gibt Byte []-basiert und zeichenfolgenbasierte Überladungen der Einfachheit halber Developer.) Wenn die geschützte Nutzlast durch einen früheren Aufruf generiert wurde `Protect` auf diesem gleichen `IDataProtector`, die `Unprotect` Methode wird die ursprüngliche ungeschützte Nutzlast zurück. Wenn die geschützte Nutzlast manipuliert wurde, oder wurde von einem anderen erzeugt `IDataProtector`die `Unprotect` Methode löst CryptographicException.

Das Konzept der gleiche im Vergleich zu anderen `IDataProtector` Verknüpfungen in Bezug auf das Konzept der Zweck zu sichern. Wenn zwei `IDataProtector` Instanzen aus demselben Stamm generiert wurden `IDataProtectionProvider` jedoch über die unterschiedlichen Zweck Zeichenfolgen im Aufruf `IDataProtectionProvider.CreateProtector`, wird sie als betrachtet werden [unterschiedliche Schutzvorrichtungen](purpose-strings.md), einer Lage beim Aufheben des Schutzes Nutzlasten, die von der anderen generiert werden.

## <a name="consuming-these-interfaces"></a>Nutzen diese Schnittstellen

Für eine Komponente DI-fähig ist die beabsichtigte Verwendung, ergreifen die Komponente eine `IDataProtectionProvider` Parameter in seinem Konstruktor, und das System DI automatisch diesen Dienst bereitstellt, wenn die Komponente instanziiert wird.

> [!NOTE]
> Einige Anwendungen (z. B. konsolenanwendungen oder 4.x ASP.NET-Anwendungen) möglicherweise nicht DI-fähigen daher den Mechanismus beschrieben kann hier verwendet werden. Für diese Szenarien finden Sie in der [nicht bewusst DI-Szenarien](../configuration/non-di-scenarios.md) Dokument Weitere Informationen zum Abrufen einer Instanz von einem `IDataProtection` Anbieter ohne DI durchlaufen.

Das folgende Beispiel zeigt drei einzelkonzepten:

1. [Hinzufügen der Datenschutzsystem](../configuration/overview.md) dem Dienstcontainer

2. Mithilfe der Abhängigkeitsinjektion zum Empfangen von einer Instanz von einem `IDataProtectionProvider`, und

3. Erstellen einer `IDataProtector` aus einem `IDataProtectionProvider` und dazu verwenden, schützen und Aufheben des Schutzes von Daten.

[!code-csharp[Main](../using-data-protection/samples/protectunprotect.cs?highlight=26,34,35,36,37,38,39,40)]

Das Paket Microsoft.AspNetCore.DataProtection.Abstractions enthält eine Erweiterungsmethode `IServiceProvider.GetDataProtector` als Annehmlichkeit Developer. Gekapselten als einzelner Vorgang beide Abrufen einer `IDataProtectionProvider` aus der Service-Anbieter und der Aufruf `IDataProtectionProvider.CreateProtector`. Das folgende Beispiel veranschaulicht die Verwendung.

[!code-csharp[Main](./overview/samples/getdataprotector.cs?highlight=15)]

>[!TIP]
> Instanzen von `IDataProtectionProvider` und `IDataProtector` für mehrere Aufrufer threadsicher sind. Es ist vorgesehen, die nach eine Komponente einen Verweis auf Ruft eine `IDataProtector` über einen Aufruf an `CreateProtector`, verwenden sie diesen Verweis für mehrere Aufrufe von `Protect` und `Unprotect`. Ein Aufruf von `Unprotect` löst CryptographicException aus, wenn die geschützte Nutzlast überprüft oder entschlüsselt werden kann. Einige Komponenten Fehler ignorieren möchten Aufheben des Schutzes von während der Vorgänge eine Komponente, die Authentifizierungscookies liest möglicherweise diesen Fehler zu behandeln und die Anforderung zu behandeln, als ob es überhaupt kein Cookie hatte anstelle der sofortiges Anforderung schlägt fehl. Komponenten, die über dieses Verhalten soll sollten CryptographicException speziell statt Angriffspunkt alle Ausnahmen abfangen.
