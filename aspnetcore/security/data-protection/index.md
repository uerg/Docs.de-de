---
title: Schutz von Daten in ASP.NET Core
author: rick-anderson
description: "Dieses Dokument dient als Inhaltsverzeichnis für die verschiedenen Themen zum Schutz von Daten in ASP.NET Core."
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/data-protection/index
ms.openlocfilehash: e08dea63f012c4a758f2e5561c4930d09cfee0ac
ms.sourcegitcommit: 493a215355576cfa481773365de021bcf04bb9c7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/15/2018
---
# <a name="data-protection-in-aspnet-core-consumer-apis-configuration-extensibility-apis-and-implementation"></a>Schutz von Daten in ASP.NET Core: Consumer-APIs, Konfiguration, Erweiterbarkeits-APIs und Implementierung

* [Einführung in den Schutz von Daten](introduction.md)

* [Erste Schritte mit Datenschutz-APIs](using-data-protection.md)

* [Consumer-APIs](consumer-apis/index.md)

  * [Übersicht über Consumer-APIs](consumer-apis/overview.md)

  * [purpose-Zeichenfolgen](consumer-apis/purpose-strings.md)

  * [Zweckhierarchie und Mehrinstanzenfähigkeit](consumer-apis/purpose-strings-multitenancy.md)

  * [Kennworthashing](consumer-apis/password-hashing.md)

  * [Beschränken der Lebensdauer von geschützten Nutzlasten](consumer-apis/limited-lifetime-payloads.md)

  * [Aufheben des Schutzes von Nutzlasten, deren Schlüssel gesperrt wurden](consumer-apis/dangerous-unprotect.md)

* [Konfiguration](configuration/index.md)

  * [Konfigurieren des Schutzes von Daten](configuration/overview.md)

  * [Standardeinstellungen](configuration/default-settings.md)

  * [Computerweite Richtlinie](configuration/machine-wide-policy.md)

  * [Szenarios ohne Möglichkeiten zur Abhängigkeitsinjektion](configuration/non-di-scenarios.md)

* [Erweiterbarkeits-APIs](extensibility/index.md)

  * [Kryptografieerweiterbarkeit in Core](extensibility/core-crypto.md)

  * [Schlüsselverwaltungserweiterbarkeit](extensibility/key-management.md)

  * [Verschiedene APIs](extensibility/misc-apis.md)

* [Implementation (Implementierung)](implementation/index.md)

  * [Authentifizierte Verschlüsselungsdetails](implementation/authenticated-encryption-details.md)

  * [Unterschlüsselableitung und authentifizierte Verschlüsselung](implementation/subkeyderivation.md)

  * [Kontextheader](implementation/context-headers.md)

  * [Schlüsselverwaltung](implementation/key-management.md)

  * [Schlüsselspeicheranbieter](implementation/key-storage-providers.md)

  * [Verschlüsselung ruhender Daten mit Schlüsseln](implementation/key-encryption-at-rest.md)

  * [Unveränderlichkeit von Schlüsseln und Ändern von Einstellungen](implementation/key-immutability.md)

  * [Schlüsselspeicherformat](implementation/key-storage-format.md)

  * [Kurzlebige Datenschutzanbieter](implementation/key-storage-ephemeral.md)

* [Kompatibilität](compatibility/index.md)

  * [Ersetzen von <machineKey> in ASP.NET](xref:security/data-protection/compatibility/replacing-machinekey)
