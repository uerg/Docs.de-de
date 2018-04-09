---
title: Aufheben des Schutzes von Nutzlasten, deren Schlüssel in ASP.NET Core gesperrt wurden.
author: rick-anderson
description: Informationen Sie zum Aufheben des Schutzes von Daten, mit Schlüsseln, die zwischenzeitlich, in einer ASP.NET Core app gesperrt wurden geschützt.
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/data-protection/consumer-apis/dangerous-unprotect
ms.openlocfilehash: b721bba63d0673f4e22fd9d1456af33489a2a389
ms.sourcegitcommit: 48beecfe749ddac52bc79aa3eb246a2dcdaa1862
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/22/2018
---
# <a name="unprotect-payloads-whose-keys-have-been-revoked-in-aspnet-core"></a>Aufheben des Schutzes von Nutzlasten, deren Schlüssel in ASP.NET Core gesperrt wurden.


<a name="data-protection-consumer-apis-dangerous-unprotect"></a>

Die ASP.NET Core Datenschutz-APIs sind nicht in erster Linie für unbestimmte Persistenz des vertraulichen Nutzlasten gedacht. Andere Technologien wie [Windows CNG DPAPI](https://msdn.microsoft.com/library/windows/desktop/hh706794%28v=vs.85%29.aspx) und [Azure Rights Management](https://docs.microsoft.com/rights-management/) eignen sich besser auf das Szenario der unbegrenzten Speicher und Verwaltungsfunktionen für entsprechend starken haben. Dies bedeutet, dass keine verbietet einen Entwickler mithilfe der ASP.NET Core Datenschutz-APIs für den langfristigen Schutz von vertraulichen Daten. Schlüssel werden nie daher aus dem Schlüssel Ring entfernt `IDataProtector.Unprotect` können vorhandene Nutzlasten immer wiederherstellen, solange die Schlüssel verfügbar und gültig sind.

Allerdings ein Problem tritt auf, wenn der Entwickler versucht, Daten aufzuheben, die mit einem gesperrten Schlüssel, als geschützt wurde `IDataProtector.Unprotect` in diesem Fall löst eine Ausnahme. Dies kann gut für kurzlebige oder vorübergehender Nutzlasten (z. B.-Authentifizierungstoken) sein, wie diese Arten von Nutzlasten einfach vom System erstellt werden können, und im schlimmsten Fall der Besucher der Website möglicherweise erforderlich, um sich erneut anmelden. Für persistente Nutzlasten, müssen jedoch `Unprotect` Throw zu unzulässigen Datenverlust führen.

## <a name="ipersisteddataprotector"></a>IPersistedDataProtector

Um das Szenario ermöglicht die Nutzlasten als auch bei widerrufenen Schlüssel ungeschützt zu unterstützen, die Datenschutzsystem enthält ein `IPersistedDataProtector` Typ. Zum Abrufen einer Instanz des `IPersistedDataProtector`, rufen Sie einfach eine Instanz des `IDataProtector` in die normale Art und Weise und versuchen Sie es Umwandlung der `IDataProtector` auf `IPersistedDataProtector`.

> [!NOTE]
> Nicht alle `IDataProtector` Instanzen umgewandelt werden können, um `IPersistedDataProtector`. Entwickler sollten verwenden C#-als-Operator oder ähnliche Laufzeitausnahmen vermieden, die durch ungültige Umwandlungen verursacht, und sie sollten darauf vorbereitet, die Groß-/Kleinschreibung Fehler entsprechend behandelt.

`IPersistedDataProtector` macht die folgenden API-Oberfläche:

```csharp
DangerousUnprotect(byte[] protectedData, bool ignoreRevocationErrors,
     out bool requiresMigration, out bool wasRevoked) : byte[]
```

Diese API nimmt die geschützte Nutzlast (als Bytearray) und gibt die ungeschützte Nutzlast. Keine Überladung zeichenfolgenbasierte ist vorhanden. Die beiden out-Parameter lauten wie folgt aus.

* `requiresMigration`: Einstellung "true", wenn der Schlüssel zum Schützen dieser Nutzlast nicht mehr das aktive Standardschlüssel ist, z. B. der Schlüssel zum Schützen dieser Nutzlast veraltet sind und ein parallelen Vorgang Schlüssel, seitdem hat durchzuführenden platzieren. Der Aufrufer möchten möglicherweise sollten die Nutzlast je nach ihren geschäftsanforderungen erneut zu schützen.

* `wasRevoked`: wird auf "true", wenn der Schlüssel zum Schützen dieser Nutzlast widerrufen wurde festgelegt.

>[!WARNING]
> Führen Sie äußersten vorsichtig vor, bei der Übergabe von `ignoreRevocationErrors: true` auf die `DangerousUnprotect` Methode. Wenn Sie nach einer beim Aufrufen dieser Methode die `wasRevoked` Wert "true" ist der Schlüssel zum Schützen dieser Nutzlast wurde gesperrt und die Nutzlast Authentizität als fehlerverdächtig behandelt werden soll. In diesem Fall den Vorgang nur fortsetzen Sie für die Nutzlast der ungeschützt ausgeführt wird, haben einige separate Gewissheit, dass es z. B. authentisch ist, ist, dass es aus, die von einer nicht vertrauenswürdigen WebClient gesendet werden, anstatt einer geschützten Datenbank stammt.

[!code-csharp[](dangerous-unprotect/samples/dangerous-unprotect.cs)]
