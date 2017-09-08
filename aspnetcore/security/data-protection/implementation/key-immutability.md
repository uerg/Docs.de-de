---
title: "Wichtige Unveränderlichkeit und Ändern von Einstellungen"
author: rick-anderson
description: 
keywords: ASP.NET Core
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: fc911ae3-feca-4ffe-8b43-362bc632fe04
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/implementation/key-immutability
ms.openlocfilehash: 3afce8f84ebe3b709ea169c7db27f99829f157ab
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/11/2017
---
# <a name="key-immutability-and-changing-settings"></a>Wichtige Unveränderlichkeit und Ändern von Einstellungen

Sobald ein Objekt mit dem Sicherungsspeicher persistent gespeichert wird, ist seine Darstellung ewig fest. Sicherungsspeicher können neue Daten hinzugefügt werden, aber vorhandene Daten können nicht geändert werden. Der Hauptzweck dieses Verhalten wird verhindert, dass Daten beschädigt.

Eine Folge dieses Verhalten ist, nachdem ein Schlüssel mit dem Sicherungsspeicher geschrieben wurde, unveränderlich ist. Die Erstellung, Aktivierung und Ablauf von Datumsangaben können nicht geändert werden, obwohl es mithilfe von IKeyManager widerrufen können. Darüber hinaus sind die zugrunde liegenden algorithmische Informationen, Schlüsselmaterial und Verschlüsselung Rest Eigenschaften auch unveränderlich.

Wenn der Entwickler eine Einstellung geändert wird, die schlüsselpersistenz wirkt sich auf, diese Änderungen werden nicht wirksam werden erst beim nächsten Mal ein Schlüssel wird, entweder ein expliziter Aufruf von IKeyManager.CreateNewKey oder über die Datenschutzsystem des eigenen generiert [automatische Schlüssel generieren](key-management.md#data-protection-implementation-key-management) Verhalten. Die Einstellungen, die beeinflussen schlüsselpersistenz sind wie folgt:

* [Die Standardlebensdauer für Schlüssel](key-management.md#data-protection-implementation-key-management)

* [Die Verschlüsselung auf Rest-Mechanismus](key-encryption-at-rest.md#data-protection-implementation-key-encryption-at-rest)

* [Die algorithmische Informationen in den Schlüssel enthalten sind](../configuration/overview.md#data-protection-changing-algorithms)

Wenn Sie diese Einstellungen älter als die nächste automatische Funktionstaste gleitenden Zeitpunkt starten möchten, sollten erwägen Sie, einen expliziten Aufruf von IKeyManager.CreateNewKey erzwingen Sie die Erstellung eines neuen Schlüssels zu verwenden. Denken Sie daran, geben Sie eine explizite Aktivierungsdatum ({jetzt + 2 Tage} ist eine gute Faustregel für die Änderung weitergegeben werden können) und Ablaufdatum im Aufruf.

>[!TIP]
> Alle Anwendungen, die im Repository berühren sollten dieselben Einstellungen angeben, mit der IDataProtectionBuilder Erweiterungsmethoden [c#], andernfalls werden die Eigenschaften des permanenten Schlüssels von abhängt, die die betreffende Anwendung, die die schlüsselgenerierung Routinen aufgerufen .
