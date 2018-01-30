---
title: "Wichtige Unveränderlichkeit und Ändern von Einstellungen"
author: rick-anderson
description: "In diesem Dokument werden die Implementierungsdetails der ASP.NET Core Data Protection Key Unveränderlichkeit APIs."
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/data-protection/implementation/key-immutability
ms.openlocfilehash: 98727c7a0c525edcda4fd8d004e0ac584cf0a5e5
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2018
---
# <a name="key-immutability-and-changing-settings"></a>Wichtige Unveränderlichkeit und Ändern von Einstellungen

Sobald ein Objekt mit dem Sicherungsspeicher persistent gespeichert wird, ist seine Darstellung ewig fest. Sicherungsspeicher können neue Daten hinzugefügt werden, aber vorhandene Daten können nicht geändert werden. Der Hauptzweck dieses Verhalten wird verhindert, dass Daten beschädigt.

Eine Folge dieses Verhalten ist, nachdem ein Schlüssel mit dem Sicherungsspeicher geschrieben wurde, unveränderlich ist. Die Erstellung, Aktivierung und Ablauf von Datumsangaben können nie geändert werden, obwohl es mit widerrufen können `IKeyManager`. Darüber hinaus sind die zugrunde liegenden algorithmische Informationen, Schlüsselmaterial und Verschlüsselung Rest Eigenschaften auch unveränderlich.

Wenn der Entwickler eine Einstellung geändert wird, die schlüsselpersistenz wirkt sich auf, diese Änderungen wird nicht wirksam, bis das nächste Mal ein Schlüssel generiert wird, entweder über ein expliziter Aufruf von `IKeyManager.CreateNewKey` oder über die Datenschutzsystem des eigenen [automatische Schlüssel Generierung](key-management.md#data-protection-implementation-key-management) Verhalten. Die Einstellungen, die beeinflussen schlüsselpersistenz sind wie folgt:

* [Die Standardlebensdauer für Schlüssel](key-management.md#data-protection-implementation-key-management)

* [Die Verschlüsselung auf Rest-Mechanismus](key-encryption-at-rest.md#data-protection-implementation-key-encryption-at-rest)

* [Die algorithmische Informationen in den Schlüssel enthalten sind](xref:security/data-protection/configuration/overview#changing-algorithms-with-usecryptographicalgorithms)

Wenn Sie diese Einstellungen älter als die nächste automatische Funktionstaste gleitenden Zeitpunkt starten möchten, erwägen Sie einen expliziten Aufruf von `IKeyManager.CreateNewKey` um die Erstellung eines neuen Schlüssels zu erzwingen. Denken Sie daran, geben Sie eine explizite Aktivierungsdatum ({jetzt + 2 Tage} ist eine gute Faustregel für die Änderung weitergegeben werden können) und Ablaufdatum im Aufruf.

>[!TIP]
> Alle Anwendungen, die im Repository berühren sollten mit die gleichen Einstellungen geben die `IDataProtectionBuilder` Erweiterungsmethoden. Andernfalls werden die Eigenschaften des permanenten Schlüssels die jeweilige Anwendung abhängig sein, die die schlüsselgenerierung Routinen aufgerufen.
