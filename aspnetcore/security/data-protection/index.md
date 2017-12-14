---
title: Schutz von Daten in ASP.NET Core
author: rick-anderson
description: "Dieses Dokument dient als Inhaltsverzeichnis für die verschiedenen Themen zum Schutz von Daten in ASP.NET Core."
keywords: ASP.NET Core,Schutz von Daten
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 1f402da8-1052-4970-9835-9f9f16a02dbc
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/index
ms.openlocfilehash: 8b42e65bb6121355120a6f4fbe8cd4d1fea153de
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
# <a name="data-protection-in-aspnet-core-consumer-apis-configuration-extensibility-apis-and-implementation"></a>Schutz von Daten in ASP.NET Core: Consumer-APIs, Konfiguration, Erweiterbarkeits-APIs und Implementierung

* [Einführung in den Schutz von Daten](introduction.md)

* [Erste Schritte mit Datenschutz-APIs](using-data-protection.md)

* [Consumer-APIs](consumer-apis/index.md)

  * [Übersicht über Consumer-APIs](consumer-apis/overview.md)

  * [Zweckzeichenfolgen](consumer-apis/purpose-strings.md)

  * [Zweckhierarchie und Mehrinstanzenfähigkeit](consumer-apis/purpose-strings-multitenancy.md)

  * [Kennwort-Hashing](consumer-apis/password-hashing.md)

  * [Beschränken der Lebensdauer von geschützten Nutzlasten](consumer-apis/limited-lifetime-payloads.md)

  * [Aufheben des Schutzes von Nutzlasten, deren Schlüssel gesperrt wurden](consumer-apis/dangerous-unprotect.md)

* [Konfiguration](configuration/index.md)

  * [Konfigurieren des Schutzes von Daten](configuration/overview.md)

  * [Standardeinstellungen](configuration/default-settings.md)

  * [Computerübergreifende Richtlinie](configuration/machine-wide-policy.md)

  * [Szenarios, die die Abhängigkeitsinjektion nicht beachten](configuration/non-di-scenarios.md)

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

  * [Ruhende Verschlüsselung von Schlüsseln](implementation/key-encryption-at-rest.md)

  * [Unveränderlichkeit von Schlüsseln und Ändern von Einstellungen](implementation/key-immutability.md)

  * [Schlüsselspeicherformat](implementation/key-storage-format.md)

  * [Kurzlebige Datenschutzanbieter](implementation/key-storage-ephemeral.md)

* [Kompatibilität](compatibility/index.md)

  * [Freigeben von Cookies zwischen Anwendungen](compatibility/cookie-sharing.md)

  * [Ersetzen von <machineKey> in ASP.NET](compatibility/replacing-machinekey.md)
