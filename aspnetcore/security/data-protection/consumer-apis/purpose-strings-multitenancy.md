---
title: Zweck Zeichenfolgen in ASP.NET Core
author: rick-anderson
description: "In diesem Dokument werden Zweck Zeichenfolge Hierarchie und mehrinstanzenfähigkeit in Bezug auf die ASP.NET Core Datenschutz-APIs."
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/data-protection/consumer-apis/purpose-strings-multitenancy
ms.openlocfilehash: 490896563db514aba3904b01e69a23b61659d830
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2018
---
# <a name="purpose-hierarchy-and-multi-tenancy-in-aspnet-core"></a>Zweck Hierarchie und mehrinstanzenfähigkeit in ASP.NET Core

Da ein `IDataProtector` ist implizit auch eine `IDataProtectionProvider`, Zwecke können miteinander verkettet werden. In diesem Sinn `provider.CreateProtector([ "purpose1", "purpose2" ])` entspricht `provider.CreateProtector("purpose1").CreateProtector("purpose2")`.

Dadurch für einige interessanten hierarchischen Beziehungen durch das Data Protection-System. Im vorangehenden Beispiel der [Contoso.Messaging.SecureMessage](purpose-strings.md#data-protection-contoso-purpose), kann die Komponente SecureMessage Aufrufen `provider.CreateProtector("Contoso.Messaging.SecureMessage")` einmal im Vorfeld und das Ergebnis in einer privaten zwischenspeichern `_myProvide` Feld. Zukünftige Schutzvorrichtungen können dann erstellt werden, über Aufrufe von `_myProvider.CreateProtector("User: username")`, und diese Schutzvorrichtungen für die einzelnen Nachrichten sichern verwendet werden würde.

Dies kann auch umgekehrt werden. Erwägen Sie eine einzelne logische Anwendung Hosten mehrerer Mandanten (CMS scheint sinnvoll), und jeder Mandant mit eigenen Verwaltungssystem Authentifizierung und Status konfiguriert werden kann. Die Dach-Anwendung hat einen einzelnen master-Anbieter, und er ruft `provider.CreateProtector("Tenant 1")` und `provider.CreateProtector("Tenant 2")` jeder Mandant einen eigenen isolierten Segment des Systems Data Protection gewähren. Mandanten konnte dann ihre eigenen einzelne Schutzvorrichtungen, die je nach ihren eigenen Anforderungen ableiten, aber unabhängig davon, wie starke wiederholen sie dann keine sie Schutzvorrichtungen, die in Konflikt stehen mit keinem anderen Mandanten erstellen, im System. Dies wird grafisch dargestellt, wie im folgenden dargestellt.

![Mehrere Mandanten Zwecke](purpose-strings-multitenancy/_static/purposes-multi-tenancy.png)

>[!WARNING]
> Dabei wird vorausgesetzt die Schirm Anwendungssteuerelemente, die APIs für die einzelnen Mandanten zur Verfügung stehen, sodass Mandanten beliebigen Code auf dem Server nicht ausgeführt werden können. Wenn ein Mandant beliebigen Code ausgeführt werden kann, sie private Reflektion führen konnte, um die Isolation Garantien zu unterbrechen, oder sie einfach das Schlüsselmaterial direkt lesen und leiten Sie den Unterschlüssel konnte wünschen sie.

Die Datenschutzsystem wird tatsächlich eine Sortierung der mehrinstanzenfähigkeit in der Out-of-Box-Standardkonfiguration verwendet. Standardmäßig wird die Schlüsselmaterial in Benutzerprofilordner das Arbeitsprozesskonto (oder der Registrierung für IIS-Anwendungspoolidentitäten) gespeichert. Jedoch wird tatsächlich relativ häufig ein einzelnes Konto verwenden, um mehrere Anwendungen auszuführen und somit alle diese Anwendungen gemeinsam verwenden das Schlüsselmaterial Master enden würde. Um dieses Problem zu lösen, fügt die Datenschutzsystem automatisch einen Bezeichner der eindeutigen pro Anwendung als erstes Element in der Kette für allgemeine Zwecke. Diese implizite Zweck dient, [isolieren einzelanwendungen](xref:security/data-protection/configuration/overview#per-application-isolation) voneinander, indem Sie jede Anwendung effektiv behandeln, wie eine eindeutige Mandant innerhalb des Systems und der Erstellungsvorgang Schutzvorrichtung identisch mit der Abbildung oben dargestellt wird.
