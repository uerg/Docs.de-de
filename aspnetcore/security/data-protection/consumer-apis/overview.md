---
title: "Übersicht über die Consumer-APIs"
author: rick-anderson
description: 
keywords: ASP.NET Core
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: f69beb9d-a519-43a8-857c-f6b01886a903
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/consumer-apis/overview
ms.openlocfilehash: d23a6ce50eef71f393124b9420f4ba473904d8b4
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/11/2017
---
# <a name="consumer-apis-overview"></a>Übersicht über die Consumer-APIs

Die IDataProtectionProvider und IDataProtector sind die grundlegenden Schnittstellen, die über die Consumer der Datenschutzsystem verwenden. Sie befinden sich die Microsoft.AspNetCore.DataProtection.Abstractions-Paket.

## <a name="idataprotectionprovider"></a>IDataProtectionProvider

Die Provider-Schnittstelle stellt den Stamm der Datenschutzsystem dar. Er kann nicht direkt zu schützen oder Aufheben des Schutzes von Daten verwendet werden. Stattdessen muss der Consumer durch Aufrufen von IDataProtectionProvider.CreateProtector(purpose), wobei Zweck eine Zeichenfolge ist, die die beabsichtigte Consumer Anwendungsfall beschreibt einen Verweis auf einen IDataProtector erhalten. Finden Sie unter [Zweck Zeichenfolgen](purpose-strings.md) viel mehr Informationen auf der Absicht dieses Parameters und wie Sie einen geeigneten Wert auswählen.

## <a name="idataprotector"></a>IDataProtector

Die Schutzvorrichtung-Schnittstelle wird durch einen Aufruf von CreateProtector zurückgegeben und kann diese Schnittstelle, über die Consumer ausführen können, schützen und Aufheben des Schutzes von Operations.

Um Daten zu schützen, übergeben Sie die Daten an die Protect-Methode. Die grundlegende Schnittstelle definiert eine Methode an, welche konvertiert Byte [] -> Byte [], aber es gibt auch eine Überladung (angegeben als Erweiterungsmethode einer) der Zeichenfolge konvertiert -> Zeichenfolge. Die Sicherheit durch die beiden Methoden ist identisch. Entwickler sollten auswählen, unabhängig davon, welche Überladung am einfachsten für ihre Verwendungsfall ist. Unabhängig von der Überladung gewählt, den Rückgabewert von schützen Methode jetzt geschützt ist (enciphered und manipulationssichere geprüft) und die Anwendung auf einem nicht vertrauenswürdigen Client senden.

Übergeben Sie zum Aufheben des Schutzes von einer zuvor geschützten Datenmenge, die geschützten Daten an die Unprotect-Methode. (Es gibt Byte []-basiert und zeichenfolgenbasierte Überladungen der Einfachheit halber Developer.) Geschützte Nutzlast durch einen früheren Aufruf von Protect für diese gleichen IDataProtector generiert wurde, gibt die Unprotect-Methode die ursprünglichen ungeschützte Nutzlast zurück. Wenn die geschützte Nutzlast manipuliert wurde oder von einem anderen IDataProtector erzeugt wurde, löst die Unprotect-Methode CryptographicException.

Das Konzept der gleiche im Vergleich zu anderen IDataProtector bindet wieder auf das Konzept der Zweck. Wenn zwei Instanzen von IDataProtector aus demselben Stamm IDataProtectionProvider jedoch über die unterschiedlichen Zweck Zeichenfolgen im Aufruf der IDataProtectionProvider.CreateProtector generiert wurden, werden Sie als [unterschiedliche Schutzvorrichtungen](purpose-strings.md), und eine ist nicht in der Lage sind, Aufheben des Schutzes von Nutzlasten, die von der anderen generiert.

## <a name="consuming-these-interfaces"></a>Nutzen diese Schnittstellen

Für eine Komponente DI-fähig ist die beabsichtigte Verwendung an, dass die Komponente einen IDataProtectionProvider-Parameter in seinem Konstruktor annehmen und das System DI automatisch diesen Dienst bereitstellt, wenn die Komponente instanziiert wird.

> [!NOTE]
> Einige Anwendungen (z. B. konsolenanwendungen oder 4.x ASP.NET-Anwendungen) möglicherweise nicht DI-fähigen daher den Mechanismus beschrieben kann hier verwendet werden. Für diese Szenarien finden Sie in der [nicht bewusst DI-Szenarien](../configuration/non-di-scenarios.md) Dokument für Weitere Informationen zum Abrufen einer Instanz von IDataProtection-Anbieter ohne DI durchlaufen.

Das folgende Beispiel zeigt drei einzelkonzepten:

1. [Hinzufügen der Datenschutzsystem](../configuration/overview.md) dem Dienstcontainer

2. Mithilfe der Abhängigkeitsinjektion zum Empfangen von einer Instanz von einem IDataProtectionProvider und

3. Erstellen einen IDataProtector aus einem IDataProtectionProvider und dazu verwenden, schützen und Aufheben des Schutzes von Daten.

[!code-csharp[Main](../using-data-protection/samples/protectunprotect.cs?highlight=26,34,35,36,37,38,39,40)]

Das Paket Microsoft.AspNetCore.DataProtection.Abstractions enthält eine Erweiterungsmethode IServiceProvider.GetDataProtector als Annehmlichkeit Developer. Sie kapselt als einzelner Vorgang sowohl eine IDataProtectionProvider vom Dienstanbieter abrufen und IDataProtectionProvider.CreateProtector aufrufen. Das folgende Beispiel veranschaulicht die Verwendung.

[!code-csharp[Main](./overview/samples/getdataprotector.cs?highlight=15)]

>[!TIP]
> Instanzen von IDataProtectionProvider und IDataProtector sind threadsicher für mehrere Aufrufer. Es richtet sich an, dass nach eine Komponente einen Verweis auf einen IDataProtector über einen Aufruf an CreateProtector abruft, dieser Verweis für mehrere Aufrufe von Protect verwendet wird und Unprotect.A Aufruf Unprotect löst CryptographicException aus, wenn die geschützte Nutzlast nicht möglich nicht überprüft oder entschlüsselt. Einige Komponenten Fehler ignorieren möchten Aufheben des Schutzes von während der Vorgänge eine Komponente, die Authentifizierungscookies liest möglicherweise diesen Fehler zu behandeln und die Anforderung zu behandeln, als ob es überhaupt kein Cookie hatte anstelle der sofortiges Anforderung schlägt fehl. Komponenten, die über dieses Verhalten soll sollten CryptographicException speziell statt Angriffspunkt alle Ausnahmen abfangen.
