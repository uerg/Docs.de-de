---
title: "Verwalten von Schlüsseln und Lebensdauer"
author: rick-anderson
description: "Beschreibt schlüsselverwaltung sowie seine Lebensdauer."
keywords: "ASP.NET Core, schlüsselverwaltung, DPAPI, DataProtection"
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: ef7dad2a-7029-4ae5-8f06-1fbebedccaa4
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/configuration/default-settings
ms.openlocfilehash: 5ac2d80e7d1cebcbc792e1247608e67991ec36f4
ms.sourcegitcommit: 8f4d4fad1ca27adf9e396f5c205c9875a3963664
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/13/2017
---
# <a name="key-management-and-lifetime"></a>Verwalten von Schlüsseln und Lebensdauer

<a name="data-protection-default-settings"></a>

## <a name="key-management"></a>Schlüsselverwaltung

Das System versucht, erkennen die betriebsumgebung, und geben Sie gute Konfigurationsfreie verhaltensbasierten Standardwerte. Die verwendete Heuristik lautet wie folgt.

1. Wenn das System in Azure-Websites gehostet wird, werden die Schlüssel in den Ordner "% HOME%\ASP.NET\DataProtection-Keys" beibehalten. Dieser Ordner wird vom Netzwerkspeicher gesichert und wird für alle Computer, der die Anwendung hostet synchronisiert. Schlüssel werden nicht im Ruhezustand geschützt. Dieser Ordner stellt den Schlüssel Ring für alle Instanzen einer Anwendung in einen einzelnen bereitstellungsslot bereit. Separate bereitstellungsslots, z. B. Staging und Produktion verwendet einen Schlüssel Ring nicht gemeinsam. Wenn Sie zwischen bereitstellungsslots, z. B. Staging bis zur Produktion ausgetauscht oder mithilfe von austauschen / B-Tests, werden jedem System mithilfe von Datenschutz nicht gespeicherte Daten mithilfe der Schlüssel Ring innerhalb des vorherigen Slots zu entschlüsseln. Dies wird für Benutzer, die aus einer ASP.NET-Anwendung, die die standard-ASP.NET-Cookie-Middleware verwendet protokolliert werden, da er Schutz von Daten verwendet, um die Cookies zu schützen. Wenn Slot unabhängig Schlüssel Ringe gewünscht Verwenden eines externen Schlüsselbund-Anbieters, z. B. Azure Blob-Speicher, Azure Key Vault, eine SQL-Speicher oder Redis-Cache.

2. Wenn das Benutzerprofil verfügbar ist, werden die Schlüssel in den Ordner "% LOCALAPPDATA%\ASP.NET\DataProtection-Keys" beibehalten. Darüber hinaus, wenn das Betriebssystem Windows ist, müssen sie im Ruhezustand mit DPAPI verschlüsselt werden.

3. Wenn die Anwendung in IIS gehostet wird, werden die Schlüssel in einen speziellen Registrierungsschlüssel der HKLM-Registrierung beibehalten, die nur für das Arbeitsprozesskonto ACLed ist. Schlüssel werden im Ruhezustand mit DPAPI verschlüsselt.

4. Wenn keine dieser Bedingungen übereinstimmt, werden die Schlüssel nicht außerhalb des aktuellen Prozesses beibehalten. Wenn der Prozess, alle generierten heruntergefahren verloren Schlüssel.

Der Entwickler ist immer vollständige Kontrolle und überschreiben kann, wie, und wo Schlüssel gespeichert werden. Die ersten drei oben aufgeführten Optionen sollten gute Standardwerte für die meisten Anwendungen, die ähnlich wie beim ASP.NET <machineKey> automatische Generierung Routinen in der Vergangenheit funktioniert hat. Der endgültige fallen-zurück-Option ist das einzige Szenario, die den Entwickler angeben tatsächlich erforderlich sind [Konfiguration](overview.md) vorgelagerten, wenn sie schlüsselpersistenz, aber diese fallen-zurück es nur in seltenen Fällen kommt.

>[!WARNING]
> Wenn der Entwickler dieser heuristische Wert überschreibt und die Datenschutzsystem an einem bestimmten Schlüssel Repository verweist, werden automatische Verschlüsselung der Schlüssel im Ruhezustand deaktiviert. Ruhende Schutz wieder aktiviert werden kann [Konfiguration](overview.md).

## <a name="key-lifetime"></a>Gültigkeitsdauer

Schlüssel in der Standardeinstellung haben eine 90-Tage-Lebensdauer. Wenn ein Schlüssel läuft ab, das System automatisch einen neuen Schlüssel generieren und den neuen Schlüssel als aktiver Schlüssel festgelegt. Als veraltet Schlüssel auf dem System bleiben sein Sie immer noch Entschlüsseln von Daten mit diesen geschützt. Finden Sie unter [schlüsselverwaltung](../implementation/key-management.md#data-protection-implementation-key-management-expiration) für Weitere Informationen.

## <a name="default-algorithms"></a>Standardalgorithmen

Die Nutzlast Schutz verwendete Standardalgorithmus ist AES-256-CBC für Vertraulichkeit und HMACSHA256 für erstellen. 512-Bit-Hauptschlüssel, jeweils nach 90 Tagen zurückgesetzt wird verwendet, die zwei untergeordneten Schlüssel für diese Algorithmen auf der Basis eines je Nutzlast verwendet abgeleitet werden. Finden Sie unter [Unterschlüssel Ableitung](../implementation/subkeyderivation.md#data-protection-implementation-subkey-derivation-aad) für Weitere Informationen.
