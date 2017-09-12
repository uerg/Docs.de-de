---
title: "Aufheben des Schutzes Nutzlasten, deren Schlüssel gesperrt wurden."
author: rick-anderson
description: 
keywords: ASP.NET Core
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 6c4e6591-45d2-4d25-855e-062ad352d648
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/consumer-apis/dangerous-unprotect
ms.openlocfilehash: 5d176515792045545add66ba5aedb0358d8bdc70
ms.sourcegitcommit: 9cdbfd0d670d70b9c354216aabee260c52dad5ee
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/12/2017
---
# <a name="unprotecting-payloads-whose-keys-have-been-revoked"></a>Aufheben des Schutzes Nutzlasten, deren Schlüssel gesperrt wurden.

<a name=data-protection-consumer-apis-dangerous-unprotect></a>

Die ASP.NET Core Datenschutz-APIs sind nicht in erster Linie für unbestimmte Persistenz des vertraulichen Nutzlasten gedacht. Andere Technologien wie [Windows CNG DPAPI](https://msdn.microsoft.com/library/windows/desktop/hh706794%28v=vs.85%29.aspx) und [Azure Rights Management](https://docs.microsoft.com/rights-management/) eignen sich besser auf das Szenario der unbegrenzten Speicher und Verwaltungsfunktionen für entsprechend starken haben. Dies bedeutet, dass keine verbietet einen Entwickler mithilfe der ASP.NET Core Datenschutz-APIs für den langfristigen Schutz von vertraulichen Daten. Schlüssel werden nie aus dem Ring Schlüssel entfernt, damit IDataProtector.Unprotect immer vorhandene Nutzlasten wiederherstellen können, solange die Schlüssel verfügbar und gültig sind.

Allerdings tritt ein Problem auf, wenn der Entwickler beim Aufheben des Schutzes Daten, die mit einem gesperrten Schlüssel geschützt wurden, wie IDataProtector.Unprotect in diesem Fall eine Ausnahme auslöst, versucht. Dies kann gut für kurzlebige oder vorübergehender Nutzlasten (z. B.-Authentifizierungstoken) sein, wie diese Arten von Nutzlasten einfach vom System erstellt werden können, und im schlimmsten Fall der Besucher der Website möglicherweise erforderlich, um sich erneut anmelden. Aber für persistente Nutzlasten Unprotect lösen müssen kann zu Datenverlust nicht akzeptabel.

## <a name="ipersisteddataprotector"></a>IPersistedDataProtector

Um das Szenario ermöglicht die Nutzlasten als auch bei widerrufenen Schlüssel ungeschützt zu unterstützen, enthält die Datenschutzsystem einen IPersistedDataProtector-Typ. Zum Abrufen einer Instanz von IPersistedDataProtector Abrufen einer Instanz von IDataProtector in die normale Art und Weise und wiederholen Sie dann die IDataProtector zum IPersistedDataProtector umwandeln.

> [!NOTE]
> Nicht alle IDataProtector-Instanzen können in IPersistedDataProtector umgewandelt werden. Entwickler sollten verwenden C#-als-Operator oder ähnliche Laufzeitausnahmen vermieden, die durch ungültige Umwandlungen verursacht, und sie sollten darauf vorbereitet, die Groß-/Kleinschreibung Fehler entsprechend behandelt.

IPersistedDataProtector macht die folgenden API-Oberfläche:

```csharp
DangerousUnprotect(byte[] protectedData, bool ignoreRevocationErrors,
     out bool requiresMigration, out bool wasRevoked) : byte[]
   ```

Diese API nimmt die geschützte Nutzlast (als Bytearray) und gibt die ungeschützte Nutzlast. Keine Überladung zeichenfolgenbasierte ist vorhanden. Die beiden out-Parameter lauten wie folgt aus.

* RequiresMigration: Einstellung "true", wenn der Schlüssel zum Schützen dieser Nutzlast nicht mehr das aktive Standardschlüssel ist, z. B. der Schlüssel zum Schützen dieser Nutzlast veraltet sind und ein parallelen Vorgang Schlüssel, seitdem hat durchzuführenden platzieren. Der Aufrufer möchten möglicherweise sollten die Nutzlast je nach ihren geschäftsanforderungen erneut zu schützen.

* WasRevoked: wird auf "true", wenn der Schlüssel zum Schützen dieser Nutzlast widerrufen wurde festgelegt.

>[!WARNING]
> Führen Sie äußersten vorsichtig vor, bei der Übergabe von IgnoreRevocationErrors: true, wenn die DangerousUnprotect-Methode. Wenn nach dem Aufrufen dieser Methode der WasRevoked-Wert "true" ist, klicken Sie dann der Schlüssel zum Schützen dieser Nutzlast wurde gesperrt und die Nutzlast Authentizität als fehlerverdächtig behandelt werden soll. In diesem Fall nur weiterhin für die Nutzlast der ungeschützt ausgeführt wird, haben einige separate Gewissheit, dass es authentisch ist, z. B., dass die It des stammen aus einer geschützten Datenbank, anstatt von einem nicht vertrauenswürdigen WebClient gesendet werden.

[!code-none[Main](dangerous-unprotect/samples/dangerous-unprotect.cs)]
