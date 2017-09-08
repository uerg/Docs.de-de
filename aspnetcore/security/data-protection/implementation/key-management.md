---
title: "Schlüsselverwaltung"
author: rick-anderson
description: 
keywords: ASP.NET Core
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: fb9b807a-d143-4861-9ddb-005d8796afa3
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/implementation/key-management
ms.openlocfilehash: 507c00edc5bade2427151ecadfed581817e4d088
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/11/2017
---
# <a name="key-management"></a>Schlüsselverwaltung

<a name=data-protection-implementation-key-management></a>

Die Datenschutzsystem verwaltet automatisch die Lebensdauer der Master-Schlüssel zum Schützen und Aufheben des Schutzes von Nutzlasten. Jeder Schlüssel kann in einer der vier Phasen vorhanden sein.

* Erstellt: der Schlüssel im Ring Schlüssel vorhanden ist, jedoch noch nicht aktiviert. Der Schlüssel darf nicht für neue Protect-Vorgänge verwendet werden, bis ausreichend Zeit verstrichen ist, dass der Schlüssel wurde eine Möglichkeit, auf alle Computer weitergegeben werden, die diesen Schlüssel Ring in Anspruch genommen.

* Aktiv – der Schlüssel im Ring Schlüssel vorhanden ist und für alle neuen Protect-Vorgänge verwendet werden soll.

* Abgelaufen - der Schlüssel Lebensdauer natürliche ausgeführt wurde und nicht mehr für neue Protect-Vorgänge verwendet werden soll.

* Widerrufen - der Schlüssel gefährdet sein und dürfen nicht für neue Protect-Vorgänge verwendet werden.

Erstellte, aktiven und abgelaufene Schlüssel möglicherweise alle zum Aufheben des Schutzes von eingehenden Nutzlasten verwendet. Gesperrte Schlüssel standardmäßig dürfen nicht zum Aufheben des Schutzes von Nutzlasten verwendet werden, aber der Anwendungsentwickler kann [dieses Verhalten überschreiben](../consumer-apis/dangerous-unprotect.md#data-protection-consumer-apis-dangerous-unprotect) bei Bedarf.

>[!WARNING]
> Der Entwickler möglicherweise versucht, einen Schlüssel aus dem Schlüssel Ring zu löschen, (z. B. durch die entsprechende Datei aus dem Dateisystem gelöscht). An diesem Punkt alle Daten, die durch den Schlüssel geschützt wird dauerhaft, und es gibt keine Notfall Überschreibung mit gesperrten Schlüssel vorhanden ist. Löschen einen Schlüssel wirklich destruktiven Verhalten ist und daher macht die Datenschutzsystem keine erstrangige-API zum Ausführen des Vorgangs.

## <a name="default-key-selection"></a>Wichtige Standardauswahl

Wenn die Datenschutzsystem des Rings Schlüssel aus dem Repository dahinter liegende liest, wird versucht, einen Schlüssel "Default" aus dem Schlüssel Ring zu suchen. Der Standardschlüssel wird für neue Protect-Vorgänge verwendet.

Die allgemeine Heuristik ist, dass die Datenschutzsystem die Taste mit dem letzten Aktivierungsdatum als den Standardschlüssel auswählt. (Es ist eine kleine Mogelfaktor um Server Serveruhr Zeitversatz zu ermöglichen.) Wenn der Schlüssel ist abgelaufen oder gesperrt, und wenn die Anwendung nicht automatisch deaktiviert wurden generieren Schlüssel und dann ein neuer Schlüssel mit sofortigem Aktivierung pro generiert werden die [Schlüssel Ablauf- und parallelen](xref:security/data-protection/implementation/key-management#data-protection-implementation-key-management-expiration) Richtlinie weiter unten.

Der Grund der Datenschutzsystem generiert einen neuen Schlüssel sofort anstatt Fallback auf einen anderen Schlüssel besteht darin, dass neue schlüsselgenerierung als eine implizite Ablaufzeit aller Schlüssel behandelt werden sollen, die vor der neue Schlüssel aktiviert wurden. Die Idee ist, dass neue Schlüssel mit verschiedenen Algorithmen oder Verschlüsselung ruhender Mechanismen als die alten Schlüssel konfiguriert wurden können, und das System sollte vorziehen, dass die aktuelle Konfiguration zurückgreifen.

Eine Ausnahme ist aufgetreten. Verfügt der Anwendungsentwickler [deaktiviert die automatische Generierung](../configuration/overview.md#data-protection-configuring-disable-automatic-key-generation), und klicken Sie dann die Datenschutzsystem etwas als den standardmäßigen Schlüssel auswählen muss. In diesem Szenario fallback werden das System, wählen Sie den Schlüssel nicht widerrufen, mit dem letzten Aktivierungsdatum eingeräumt an Schlüsseln, die Zeit an andere Computer im Cluster weitergegeben wurden. Die Fallbacksystem annehmen kann eine abgelaufene Standardschlüssel daher auswählen. Die Fallbacksystem wird nie einen gesperrten Schlüssel als den standardmäßigen Schlüssel auswählen und wenn der Schlüssel Ring leer ist oder jeden Schlüssel wurde gesperrt klicken Sie dann das System erzeugt einen Fehler bei der Initialisierung.

<a name=data-protection-implementation-key-management-expiration></a>

## <a name="key-expiration-and-rolling"></a>Schlüsselablauf und Rollen

Wenn ein Schlüssel erstellt wird, erhält er automatisch ein Datum der Aktivierung von {Now + 2 Tage} und {Now + 90 Tage} Ablaufdatum. Die Verzögerung 2 Tagen vor der Aktivierung erhalten die Schlüsselzeit durch das System weitergegeben. Somit ermöglicht es anderen Anwendungen, die auf den in den Sicherungsspeicher beobachten den Schlüssel bei ihrer nächsten Zeitraum für die automatische Aktualisierung daher maximieren die Gefahr, dass bei der Schlüssel ring geworden ist aktiv, die es verteilt wurde für alle Anwendungen, die möglicherweise zum müssen diese verwendet werden.

Wenn die Standardschlüssel innerhalb von 2 Tagen abläuft und der Schlüssel Ring nicht bereits einen Schlüssel aufweist, der nach Ablauf der Standardschlüssel aktiv sind, bleiben die Datenschutzsystem automatisch einen neuen Schlüssel für den Schlüssel Ring. Dieses neuen Schlüssels verfügt über ein Datum der Aktivierung von {Ablaufdatum des Standardschlüssel} und {Now + 90 Tage} Ablaufdatum. Dadurch wird das System die Schlüssel automatisch in regelmäßigen Abständen ohne Unterbrechung des Diensts zurückzusetzen.

Es gibt möglicherweise Situationen, in denen ein Schlüssel mit sofortige Aktivierung erstellt wird. Ein Beispiel wäre, wenn die Anwendung noch nicht für eine Zeit ausgeführt und sämtliche Schlüssel in den Schlüssel Ring ist abgelaufen. In diesem Fall wird der Schlüssel ein Datum der Aktivierung von {jetzt} ohne die normalen aktivierungsverzögerung von 2 Tagen angegeben.

Die wichtigsten Standardlebensdauer ist 90 Tage, obwohl dies konfigurierbar, wie im folgenden Beispiel ist.

```csharp
services.AddDataProtection()
       // use 14-day lifetime instead of 90-day lifetime
       .SetDefaultKeyLifetime(TimeSpan.FromDays(14));
   ```

Ein Administrator kann auch die systemweite Standardeinstellung ändern, wenn ein expliziter Aufruf von SetDefaultKeyLifetime systemweite Richtlinie außer Kraft setzt. Die Standardlebensdauer für den Schlüssel darf nicht kürzer als 7 Tage sein.

## <a name="automatic-keyring-refresh"></a>Automatische Schlüsselsammlung aktualisieren

Wenn die Datenschutzsystem initialisiert wird, liest der Ring Schlüssel aus dem zugrunde liegenden Repository und im Arbeitsspeicher zwischengespeichert. Dieser Cache ermöglicht das schützen und Unprotect-Vorgängen um den Vorgang fortzusetzen, ohne dafür das Sicherungsspeicher. Vom System wird den Sicherungsspeicher für Änderungen automatisch geprüft, ungefähr alle 24 Stunden oder wenn der aktuelle Standardschlüssel abläuft, je nachdem, was zuerst eintritt.

>[!WARNING]
> Entwickler sollten nur sehr selten (wenn überhaupt) müssen die Schlüssel-APIs direkt verwenden. Die Datenschutzsystem führt automatischen schlüsselverwaltung, wie oben beschrieben.

Die Datenschutzsystem stellt eine Schnittstelle, die verwendet werden kann, um zu überprüfen, und nehmen Sie Änderungen an der Schlüssel Ring IKeyManager. Das DI-System, das die Instanz IDataProtectionProvider bereitgestellt kann auch eine Instanz des IKeyManager für Ihre Nutzung bereitstellen. Alternativ können Sie die IKeyManager gerade aus der IServiceProvider wie im folgenden Beispiel per Pull beziehen.

Jeder Vorgang, der bestimmt, der Schlüssel Ring (einen neuen Schlüssel explizit erstellen oder Ausführen einer Sperrung) wird der in-Memory-Cache ungültig. Beim nächste Aufruf von Protect oder Unprotect bewirkt, dass die Datenschutzsystem liest die Schlüsselbund und erneut erstellen des Caches.

Im Beispiel unten zeigt mithilfe der IKeyManager-Benutzeroberfläche der Schlüssel Ring, einschließlich entziehen vorhandenen Schlüssel und generieren manuell einen neuen Schlüssel zu überprüfen.

[!code-none[Main](key-management/samples/key-management.cs)]

## <a name="key-storage"></a>Schlüsselspeicher

Die Datenschutzsystem hat eine heuristische, bei dem Versuch, einen geeigneten Schlüssel Speicherort und die Verschlüsselung auf Rest-Mechanismus automatisch hergeleitet werden. Dies kann auch durch den app-Entwickler. In den folgenden Dokumenten erläutert die integrierten Implementierungen der folgenden Mechanismen:

* [In-Box-Schlüsselspeicher-Anbieter](key-storage-providers.md#data-protection-implementation-key-storage-providers)

* [Integrierte Schlüsselverschlüsselung zur Rest-Anbieter](key-encryption-at-rest.md#data-protection-implementation-key-encryption-at-rest-providers)
