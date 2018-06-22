---
title: Wichtige Unveränderlichkeit und schlüsseleinstellungen in ASP.NET Core
author: rick-anderson
description: Erfahren Sie, den Implementierungsdetails der ASP.NET Core Datenschutz Key Unveränderlichkeit APIs.
ms.author: riande
ms.date: 10/14/2016
uid: security/data-protection/implementation/key-immutability
ms.openlocfilehash: 45460e0bdf6ad0a978f984d55b64f562c13fb575
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/20/2018
ms.locfileid: "36279453"
---
# <a name="key-immutability-and-key-settings-in-aspnet-core"></a>Wichtige Unveränderlichkeit und schlüsseleinstellungen in ASP.NET Core

Sobald ein Objekt mit dem Sicherungsspeicher persistent gespeichert wird, ist seine Darstellung ewig fest. Sicherungsspeicher können neue Daten hinzugefügt werden, aber vorhandene Daten können nicht geändert werden. Der Hauptzweck dieses Verhalten wird verhindert, dass Daten beschädigt.

Eine Folge dieses Verhalten ist, nachdem ein Schlüssel mit dem Sicherungsspeicher geschrieben wurde, unveränderlich ist. Die Erstellung, Aktivierung und Ablauf von Datumsangaben können nie geändert werden, obwohl es mit widerrufen können `IKeyManager`. Darüber hinaus sind die zugrunde liegenden algorithmische Informationen, Schlüsselmaterial und Verschlüsselung Rest Eigenschaften auch unveränderlich.

Wenn der Entwickler eine Einstellung geändert wird, die schlüsselpersistenz wirkt sich auf, diese Änderungen wird nicht wirksam, bis das nächste Mal ein Schlüssel generiert wird, entweder über ein expliziter Aufruf von `IKeyManager.CreateNewKey` oder über die Datenschutzsystem des eigenen [automatische Schlüssel Generierung](xref:security/data-protection/implementation/key-management#data-protection-implementation-key-management) Verhalten. Die Einstellungen, die beeinflussen schlüsselpersistenz sind wie folgt:

* [Die Standardlebensdauer für Schlüssel](xref:security/data-protection/implementation/key-management#data-protection-implementation-key-management)

* [Die Verschlüsselung auf Rest-Mechanismus](xref:security/data-protection/implementation/key-encryption-at-rest#data-protection-implementation-key-encryption-at-rest)

* [Die algorithmische Informationen in den Schlüssel enthalten sind](xref:security/data-protection/configuration/overview#changing-algorithms-with-usecryptographicalgorithms)

Wenn Sie diese Einstellungen älter als die nächste automatische Funktionstaste gleitenden Zeitpunkt starten möchten, erwägen Sie einen expliziten Aufruf von `IKeyManager.CreateNewKey` um die Erstellung eines neuen Schlüssels zu erzwingen. Denken Sie daran, geben Sie eine explizite Aktivierungsdatum ({jetzt + 2 Tage} ist eine gute Faustregel für die Änderung weitergegeben werden können) und Ablaufdatum im Aufruf.

>[!TIP]
> Alle Anwendungen, die im Repository berühren sollten mit die gleichen Einstellungen geben die `IDataProtectionBuilder` Erweiterungsmethoden. Andernfalls werden die Eigenschaften des permanenten Schlüssels die jeweilige Anwendung abhängig sein, die die schlüsselgenerierung Routinen aufgerufen.
